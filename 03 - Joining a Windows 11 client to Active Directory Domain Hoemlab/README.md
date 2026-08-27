# My Windows Server 2025 Homelab: Joining a Windows 11 Client to the Domain!

This phase is where the home lab starts acting like a real company network! Below is a casual walkthrough of how I locked down my server's network settings, built my corporate folder structure, and joined my Windows 11 machine to the network.

---

## 🔒 Step 1: Locking Down the Server's IP (Static IP)

In the real world, servers can never use automatic IP addresses (DHCP) because if the IP changes, the clients won't know where to look for the domain controller. I need to lock my server's IP down.

1. I opened the Command Prompt (`cmd`) on the server, typed `ipconfig`, and hit Enter.
2. I noted down the auto-assigned IP address so I could reuse it as my permanent static address.
3. Navigated to **Settings > Network & internet > Ethernet**.
4. Under **IP assignment**, I clicked **Edit** and switched it from automatic to **Manual**.
5. I typed in the exact IP, subnet mask, and gateway I copied from my command prompt.
6. **The Critical Part:** For the Preferred DNS, I made sure it pointed to `127.0.0.1` (the loopback address), which tells the server to look at itself for domain requests.

<img src="https://i.imgur.com/McXzwLb.png"/>

7. I gave the server a quick reboot and ran `ipconfig /all` in cmd to make absolutely sure the changes stuck.

<img src="https://i.imgur.com/arzTBwq.png"/>

---

## 📂 Step 2: Creating OUs and User Accounts

Before spinning up the user's PC, I need to build the directory structure (Organizational Units, or OUs) and create accounts so someone can actually log in.

1. Opened up **Active Directory Users and Computers**.
2. Right-clicked my domain name (`hicksnaz.local`), went to **New**, and chose **Organizational Unit**.
3. I named the main folder `AFRICA`.
4. Inside the `AFRICA` folder, I right-clicked to make two sub-folders (sub-OUs): one named `AFRICA` and one named `Computer`.
5. Inside the `AFRICA` folder, I created three more sub-folders for different departments: `IT`, `HR`, and `Accounting`.

<img src="https://i.imgur.com/G1ObEbR.png"/>

### Making the Accounts
Next, I created two test users so I could use them on the client machine later:
* **The Domain Admin Account:** Created a user inside the `IT` folder. I unchecked "User must change password at next logon" and checked "Password never expires" to keep my lab simple. Then, I double-clicked the user, went to the **Member Of** tab, and added them to the **Domain Admins** group. (We need an admin account to authorize joining new PCs to the network)!
* **The Regular User Account:** Created a normal employee account inside the `HR` folder with the same password settings, leaving them as a standard domain member.

<img src="https://i.imgur.com/3DxCtOm.png"/>

---

## 💻 Step 3: Spinning up the Windows 11 Virtual Machine

1. I went to the official Microsoft site and downloaded the **Windows 11 Disk Image (ISO)**.
2. Opened VirtualBox, clicked **New**, and named it `Windows 11 Client`.
3. Attached my Windows 11 ISO file.
4. **Crucial Requirement:** Windows 11 requires a virtual TPM chip to install, so I enabled the encryption password settings in the VM wizard.
5. Allocated a healthy amount of RAM and set the hard drive to **70 GB** (Windows 11 needs at least 64GB).
6. Booted the VM, accepted the terms, and **explicitly chose Windows 11 Pro**. *(The Home version cannot join domains, so Pro or Enterprise is mandatory!)*.
7. When the Windows setup asked how to set up the device, I chose **Set up for work or school**, went to **Sign-in options**, and clicked **Domain join instead** to finish creating my local setup user.

<img src="https://i.imgur.com/D1zW9Hf.png"/>
<img src="https://i.imgur.com/Q43A13J.png"/>
<img src="https://i.imgur.com/u72DY6f.png"/>
<img src="https://i.imgur.com/cbbgPpP.png"/>

---

## 🌐 Step 4: Connecting the Client to the Network Brain

Now I have a server and a Windows 11 machine, but they aren't talking yet. I need to point the Windows 11 machine directly to our server's DNS address.

1. On the Windows 11 machine, I opened **Settings > Network & internet > Ethernet**.
2. Scrolled down to **DNS server assignment** and clicked **Edit**.
3. Switched it to **Manual**, turned on IPv4, and typed in the **exact Static IP address of my Windows Server** as the Preferred DNS.
4. To test the connection, I opened Command Prompt on the client and typed `ping [Server_IP]`. Got 4 successful replies back!

<img src="https://i.imgur.com/QWp99j4.png"/>

---

## 🤝 Step 5: Joining the Domain!

The moment of truth! Let's officially connect this client computer to the domain.

1. On the Windows 11 machine, opened File Explorer, right-clicked **This PC**, and opened **Properties**.
2. Clicked on **Advanced system settings** (or Domain/Workgroup settings).
3. Under the Computer Name tab, clicked **Change**, selected the **Domain** radio button, and typed in my forest name: `homelab.local`.
4. Hit Enter, and a login prompt popped up! I typed in the credentials for the **Domain Admin account** I created back in Step 2.
5. **Boom!** A box popped up saying *"Welcome to the homelab.local domain"*. I clicked OK and let the machine restart.

<img src="https://i.imgur.com/krziIwt.png"/>

6. Once it restarted, I clicked **Other User** at the login screen, typed in my regular HR user's domain login, and successfully logged in to the desktop!

---

## 🏢 Step 6: Active Directory Clean-Up (Best Practice)

When you join a computer to a domain, Windows automatically drops it into a generic, hidden folder called "Computers". In a professional environment, you want your active assets sitting inside your custom OUs so group rules can apply to them.

1. I went back over to my **Windows Server** screen and opened Active Directory Users and Computers.
2. I clicked the default **Computers** folder and found my new client machine sitting there.
3. Right-clicked the computer name, selected **Move**, expanded the `AFRICA` tree, and selected my custom `Computer` OU.
4. Double-clicked the computer asset inside its new home and typed a clean inventory asset note into the **Description** box (e.g., *Assigned to HR Team - Main Office*).

<img src="https://i.imgur.com/MXoEKxR.png"/>
