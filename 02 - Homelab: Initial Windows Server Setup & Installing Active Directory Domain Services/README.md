# My Windows Server 2025 Homelab: Setting Up Active Directory! 

Think of this as the "brain" upgrade for my home network setup. Below is exactly how I knocked out the pre-setup chores, loaded the active directory tools, and officially created my own little test domain.

---

## 🛠️ Step 1: Taking Care of Chores First (Best Practices)

Before rushing in to install Active Directory, I needed to make sure the server was updated and accessible. Doing this now saves a massive headache later, because updates require reboots that can break a domain setup midway through.

### 1. Running Windows Update
1. I opened up the search bar, typed `Settings`, and jumped into the menu.
2. Clicked on **Windows Update** at the very bottom left.
3. Saw a whole list of updates waiting for me, so I clicked **Install all**. 
4. I let it do its thing, grab all the security patches, and do a quick reboot.

<img src="https://i.imgur.com/WSNif6W.png"/>

### 2. Turning on Remote Management
In the real world, sysadmins don't sit in front of the actual server screen; they log in remotely. I wanted my lab to copy real life.
1. I opened up the **Server Manager** app.
2. Clicked on **Local Server** on the left side menu.
3. Looked for **Remote Management** and made sure it said `Enabled`. If yours is off, just click it, check the box to allow it, and save.

<img src="https://i.imgur.com/GpXuG5Y.png"/>

---

## 💿 Step 2: Grabbing the Active Directory Files

Now that the server is clean and updated, it's time to install the Active Directory role. This basically copies the necessary files onto our virtual hard drive.

1. Inside **Server Manager**, I looked at the top right corner, clicked **Manage**, and then hit **Add Roles and Features**.
2. Clicked **Next** past the welcome screen, and kept **Role-based or feature-based** checked.
3. Selected my server (`fileserver01`) from the little list and hit **Next**.
4. Scroll down a tiny bit until you see **Active Directory Domain Services** and check that box.
5. A pop-up wizard will appear asking to add some required management tools. I just clicked **Add Features** to approve it.

<img src="https://i.imgur.com/noHwgtc.png"/>

6. I clicked **Next** through the rest of the generic menus until I reached the final review page.
7. Clicked **Install** and waited for the progress bar to fill up.
8. **Don't close the window yet!** Once it finishes, we have to do one more thing right from this screen to bring it to life.

<img src="https://i.imgur.com/qiOCZfs.png"/>

---

## 🚀 Step 3: Promoting the Server to a Domain Controller

Right now, the files are on our computer, but the server doesn't actually know it's a domain controller yet. We need to "promote" it.

1. On that same success screen, I clicked the blue link that says: **Promote this server to a domain controller**.
2. A new window popped up. Since this is our very first domain, I selected **Add a new forest**.
3. **Choosing a Name:** Because this is a private home lab, you want to avoid using a real website name (like google.com). I went with `homelab.local` to keep things simple and safe.

<img src="https://i.imgur.com/kKOfuy3.png"/>

4. Hit **Next** and verified that the functional level was set to **Windows Server 2025**.
5. Typed in a backup password for the **Restore Mode (DSRM)**—make sure to write this down somewhere safe!
6. Clicked **Next** past the DNS warning (completely normal for a brand new lab setup) and left all the paths and NetBIOS names as defaults.
7. Clicked **Next** one last time so the server could run a quick health check.
8. Once the top bar read *"All prerequisite checks passed successfully"*, I hit **Install**. The server will take a few minutes to configure everything and then automatically reboot.

<img src="https://i.imgur.com/SdmqbXZ.png"/>

---

## 🔓 Step 4: Logging In & Checking My Work

When the virtual machine boots back up, the login screen looks completely different because we are no longer logging into a basic local computer—we are logging into our brand new domain!

1. I sent the `Ctrl + Alt + Del` command through VirtualBox (**Input > Keyboard > Insert Ctrl-Alt-Del**).
2. Looked at the username line and confirmed it changed to say `HOMELAB\Administrator`. 
3. Typed in my main admin password and hit enter.

### Where did the tools go?
Because Windows Server 2025 has a slightly updated look, finding our active directory management shortcuts can be a little confusing at first:

1. Click the **Start Menu** and choose **All** apps.
2. Scroll all the way down to the bottom and click on **Windows Tools**.
3. A folder will pop up, and you should see all your shiny new management tools sitting right there!

The two big ones I checked to confirm everything worked perfectly are:
* **Active Directory Users and Computers** (Where I'll make users later)
* **Group Policy Management** (Where I can set rules for computers)

<img src="https://i.imgur.com/FCOTE6R.png"/>

