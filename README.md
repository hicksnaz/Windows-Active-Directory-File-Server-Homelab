# Windows-Active-Directory-File-Server-Homelab


## 📺 The Series
 
| Episode | What It's About |
|---------|------------------|
| 1 | Getting Windows Server installed and running |
| 2 | Turning it into a real network "brain" (Active Directory) |
| 3 | Adding a second computer and getting them talking |
| 4 | Locking down folders so people can't snoop where they shouldn't |
 
---
 
## 🧱 Episode 1: Laying the Foundation
 
**What I was trying to do:** Just get the operating system installed and working properly — nothing fancy yet.
 
**What actually happened:**
- Grabbed the official Windows Server 2025 ISO
- Spun up a brand new virtual machine to install it on
- Here's the trick most people miss — VMware tries to auto-install everything for you with "Easy Install," but that gives you a boring text-only version with no desktop. I skipped that and manually picked the **full Desktop Experience** version instead, so I'd actually have a GUI to click around in
- Installed some drivers so the screen wasn't a blurry mess
- Gave the server an actual name instead of the generic default — `fileserver01`, so it actually feels like a real machine with a job to do
Basically: get the computer turned on and looking presentable before doing anything serious with it.
 
---
 
## 🛡️ Episode 2: Giving the Server a Brain
 
**What I was trying to do:** Turn this from "just a computer" into something that can actually manage a whole network of other computers and users — like a real office IT setup.
 
**What actually happened:**
- Ran all the Windows updates first (never skip this)
- Turned on Remote Management so I could control the server without sitting in front of it
- Installed **Active Directory Domain Services** — this is the actual "brain" software
- Went through the setup wizard to create my own private company network, called a "forest" (yes, that's really the term), under a custom domain: `homelab.local`
- Once that was done, I unlocked the good stuff — tools like Active Directory Users and Computers, which is basically mission control for managing everyone on the network
This episode is the real turning point — after this, the server isn't just an OS anymore, it's an actual domain controller.
 
---
 
## 🌐 Episode 3: Getting Everyone Talking
 
**What I was trying to do:** Build a second computer (the "employee's" laptop) and get it properly connected into the same network as the server.
 
**What actually happened:**
- Gave the server a fixed, permanent IP address (a "static IP") so it doesn't randomly change and break everything
- Told the server to use its own domain for DNS, so it can find itself and everyone else on the network
- Created a proper folder structure for different departments — think HR, IT, Finance
- Made some fake employee accounts to go with those departments
- Installed a **Windows 11 Pro** virtual machine to act as the employee's computer
- Pointed that computer's network settings at the server
- Officially "joined" it to the domain — meaning now an employee can actually log in using their company account, not just a local Windows login
This is the episode where it starts feeling like a real office network instead of two random VMs sitting next to each other.
 
---
 
## 🔒 Episode 4: Locking Down the Vault
 
**What I was trying to do:** Make sure people can only see the files they're supposed to see. HR shouldn't be able to open Finance's folder, and vice versa.
 
**What actually happened:**
- Built out real folders for each department on the server's storage
- Created security groups in Active Directory (basically "teams" you can assign permissions to instead of doing it person-by-person)
- This is where it got genuinely tricky — there are **two layers of permissions** that both matter:
  - **NTFS permissions** — the deep, folder-level security. This is where I turned off inheritance on sensitive folders so the rules didn't just cascade down from the parent folder
  - **Share permissions** — the "front door" settings for accessing the folder over the network. The trick here is to keep these wide open and let the NTFS permissions do the real work
- Logged in as different fake employees on the Windows 11 client to actually test it — and confirmed people really were getting blocked from folders they shouldn't have access to
This was the most satisfying part — watching the permissions actually work exactly as intended.
 
---
 
## 🧰 What I Used
 
- Windows Server 2025 (Desktop Experience)
- Windows 11 Pro
- VMware
- Active Directory Domain Services (AD DS)
- NTFS & Share Permissions
---
 
## 💡 Why I Did This
 
Honestly, the best way to actually understand how a company network works is to build one yourself — even a fake one. This project walks through the exact stuff a real IT admin would set up on day one at a small business: a domain, user accounts, departments, and file security that actually holds up.
 
If you're learning IT/sysadmin stuff too, feel free to follow along or fork this and build your own version.
 
