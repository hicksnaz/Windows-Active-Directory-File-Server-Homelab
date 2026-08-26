# Windows Server 2025 Homelab: VirtualBox Setup Guide

Welcome to my Windows Server 2025 Homelab repository! This project documents how I built and configured a Windows Server 2025 Virtual Machine (VM) from scratch using **Oracle VirtualBox**. This deployment serves as the core foundational layer for my enterprise environment testing.

---

## 🛠️ Step 1: Image & Software Acquisition

To get started, I needed to gather the required installation media:
1. **Windows Server 2025 ISO:** I navigated to the **Microsoft Evaluation Center** page, registered for the free trial, and downloaded the **ISO 64-bit edition** in English.
2. **Hypervisor:** I downloaded the official **Oracle VirtualBox** installer directly from their download page. I prefer VirtualBox for this because it doesn't require creating an account to download the platform packages.

<img src="https://i.imgur.com/4Z3BmfI.png"/>

---

## 💻 Step 2: Virtual Machine Provisioning

I completed the following configuration steps inside VirtualBox to provision a lean, optimized home lab environment:

1. Clicked **New** on the top menu to create a brand new virtual machine.
2. Named the VM `Windows Server 2025`.
3. Linked my downloaded ISO file directly inside the **ISO Image** field.
4. Checked the box to **skip unattended installation** to ensure I could manually configure the operating system options.
5. Selected **Windows Server 2025 (64-bit)** as the OS version.
6. Allocated **40 GB** of virtual storage space to keep my physical host machine's drive usage low.

<img src="https://i.imgur.com/wA9iJCf.png"/>

---

## 💿 Step 3: Operating System Installation

1. Launched the new VM by right-clicking it and selecting **Start > Normal Start** (with GUI).
2. Selected my preferred language settings and proceeded through the initial steps.
3. **Critical Choice:** I explicitly chose the **Desktop Experience** layout. Selecting standard evaluation drops you straight into a command prompt environment with no graphical interface, so this is essential to get the GUI.
4. Accepted the software terms, skipped custom disk partitioning, and initiated the Windows installation.
5. Set up a secure local **Administrator account password** to complete the final setup phase.

---

## 🔓 Step 4: Console Access in VirtualBox

Because pressing `Ctrl + Alt + Del` on my physical keyboard triggers my host machine instead of the virtual machine, I use VirtualBox's internal commands to log in:
* **Option A (Menu):** I click **Input > Keyboard > Insert Ctrl-Alt-Del** from the top menu bar.
* **Option B (Hotkey):** I press the **Right Ctrl Key + Delete** combination on my keyboard.

---

## ⚙️ Step 5: Post-Installation Hostname Configuration

Windows assigns a completely random, generic hostname upon installation that is difficult to remember. As an absolute best practice, I customize my server identity **before** promoting it to a Domain Controller or installing Active Directory roles. Changing a computer name *after* it becomes a live domain controller is much more complex and unsafe.

1. Opened **File Explorer**, right-clicked **This PC**, and opened **Properties**.
2. Clicked on **Rename this PC**.
3. Changed the computer name to a functional, easily identifiable name: `fileserver01`.
4. Kept it under a default Workgroup for now and clicked **Next**.
5. Promptly initiated a full machine reboot to apply the new name settings.

### System Verification
Once the server restarted, I verified the server configuration by opening the Command Prompt (`cmd`) and running:
```cmd
hostname
```
