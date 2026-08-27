# My Windows Server 2025 Homelab: Installation & Base Setup (VMware Edition)

This project documents how I built and configured a Windows Server 2025 Virtual Machine (VM) from scratch using **VMware Workstation Pro**. This setup serves as the foundation for enterprise networking and Active Directory testing, covering everything from software procurement to initial system configuration.

---

## 🛠️ Step 1: Image & Software Acquisition

1.  **Windows Server 2025 ISO:** I downloaded the official **64-bit English Evaluation ISO** from the Microsoft Evaluation Center.
2.  **VMware Workstation Pro:** I downloaded the personal-use version of VMware Workstation Pro from the Broadcom portal.

<img src="https://i.imgur.com/wzrOTZT.png"/>
<img src="https://i.imgur.com/YIEKbuj.png"/>

---

## 💻 Step 2: Virtual Machine Provisioning in VMware

I created a new VM in VMware Workstation Pro:
1.  Selected **Typical (recommended)** setup.
2.  Selected **"I will install the operating system later"** to bypass Easy Install issues.
3.  Set Guest OS to **Microsoft Windows** > **Windows Server 2025**.
4.  Allocated **40 GB** of virtual storage space.
5.  Under **Customize Hardware**, pointed the CD/DVD drive to the downloaded ISO file.

<img src="https://i.imgur.com/AvWcgA1.png"/>

---

## 💿 Step 3: Operating System Installation

1.  Powered on the VM and pressed a key to boot from the ISO.
2.  Selected the **Desktop Experience** version to ensure a graphical interface.
3.  Installed the OS on the 40GB virtual disk.
4.  Set a strong local **Administrator** password.

<img src="https://i.imgur.com/Xp9M5wP.png"/>

---

## 🚀 Step 4: Installing VMware Tools (Optimized Drivers)

1.  Inside the running VM, selected **VM > Install VMware Tools** from the VMware menu.
2.  Ran `setup64.exe` inside the virtual DVD drive to install drivers, enabling proper display resolution.
3.  Rebooted the system.

---

## ⚙️ Step 5: Post-Installation Hostname Configuration

To prepare for Active Directory, I renamed the machine to a functional name:
1.  Accessed **System Properties** to rename the PC (e.g., to `fileserver01`).
2.  Rebooted and verified the change by running `hostname` in the Command Prompt.

<img src="https://i.imgur.com/ieOfBMl.png"/>

