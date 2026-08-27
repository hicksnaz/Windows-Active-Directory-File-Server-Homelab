# My Windows Server 2025 Homelab: Setting Up a File Server & Managing Permissions!

In this phase, I am turning the server into a secure corporate storage hub. I'll be mapping out department folders, locking things down with Active Directory security groups, and configuring both **NTFS** and **Share Permissions** to make sure employees can only see the data they are authorized to access.

---

## 📂 Step 1: Creating the Shared Storage Folders

First things first, I needed a central directory structure on the server's local hard drive to hold all of our company data.

1. On the Windows Server, I opened up **File Explorer** and clicked on **This PC**.
2. Double-clicked the **C: Drive** and created a brand new root folder named `company data`.
3. Jumped inside the `company data` folder and built four distinct department folders:
   * `HR`
   * `IT`
   * `Finance`
   * `Public`

<img src="https://i.imgur.com/HRf6itp.png"/>

---

## 👥 Step 2: Creating Active Directory Security Groups

In a professional network environment, you never assign access rights to individual users one by one (that would be a nightmare to manage). Instead, we assign rights to a **Security Group** and just drop users into that group.

1. Opened up **Active Directory Users and Computers**.
2. Navigated to my main User Organizational Unit (OU).
3. Right-clicked an empty space, selected **New > Group**, and created three separate security groups matching our departments:
   * `HR`
   * `IT`
   * `Finance`

<img src="https://i.imgur.com/Qm2gxJ9.png"/>

---

## 👤 Step 3: Generating Test Users & Assigning Groups

Now I need some test employees to populate these departments and verify that our security barriers actually work.

1. Right-clicked my User OU, selected **New > User**, and built out three separate test user accounts (one for each department).
2. For each user, I unchecked "User must change password at next logon" and checked **Password never expires** to keep things friendly for my lab environment.
3. **Linking Users to Groups:** I right-clicked each user, went to the **Properties** window, jumped to the **Member Of** tab, and clicked **Add** to search for and link them to their corresponding department group.

<img src="https://i.imgur.com/EQxb5w4.png"/>

---

## 🛠️ Step 4: Configuring Deep Disk Security (NTFS Permissions)

This is where the real security happens. **NTFS Permissions** apply whether a user logs into the machine locally or accesses it over the network. My goal is to ensure that only the HR group can access the HR folder, and so on.

Let's look at how I locked down the `HR` folder as the template:
1. Right-clicked the `HR` folder and selected **Properties > Security tab**.
2. Clicked **Edit**, then **Add**, typed in `HR`, and clicked **Check Names** to pull up our HR security group.
3. Checked the box to give the HR group **Modify** permissions.
4. **Breaking Inheritance:** Because standard users automatically inherit read access from the root drive, I clicked **Advanced** at the bottom of the Security tab and selected **Disable Inheritance**.
5. When prompted, I chose **"Convert inherited permissions into explicit permissions"**. This copies the active list safely so I can manually remove unauthorized users without accidentally locking out the Admin account.
6. Selected the generic `Users` group and hit **Remove**, ensuring only `System`, `Administrators`, and the target `HR` group remained.

<img src="https://i.imgur.com/gfuwrvm.png"/>

### Completing the Rest of the Folders:
Following that exact same blueprint, I configured the remaining spaces:
* **IT Folder:** Removed generic users; granted the `IT` security group **Modify** rights.
* **Finance Folder:** Removed generic users; granted the `Finance` security group **Modify** rights.
* **Public Folder:** Granted the entire `Domain Users` group **Read** rights, and gave the `IT` group **Modify** rights so they can manage it.

---

## 🌐 Step 5: Opening the Network Gate (Share Permissions)

Share permissions control the initial security gateway when someone tries to access our folder over the network using a UNC path like `\\fileserver01\company data`.

Instead of trying to micromanage permissions in two separate places, the industry best practice is to open the Share doors completely to **Everyone** and let our strict NTFS settings handle the actual sorting on the disk! (When both apply, Windows automatically enforces whichever setting is most restrictive).

1. Right-clicked the root `company data` folder and selected **Properties**.
2. Headed over to the **Sharing** tab and clicked **Advanced Sharing**.
3. Checked the box for **Share this folder**.
4. Clicked the **Permissions** button at the bottom.
5. Highlighted the default `Everyone` group and checked the box to allow **Full Control**.
6. Clicked **Apply** and **OK** all the way out to save it.

<img src="https://i.imgur.com/sokKlSY.png"/>
