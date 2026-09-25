
# 🛠️ Lab Deployment & Infrastructure Installation Guide
**Document Context:** Blue-Team Engineering Sandbox Environment Provisioning Blueprint  
**Target Environment:** Oracle VM VirtualBox Hypervisor Engine  
**Host Architecture Integration:** Windows 10 Pro Local Workspace  
**Guest Environment Platform:** Ubuntu Linux 22.04.5 LTS Monitored Gateway Core  



## 💻 Section 1: Hypervisor & Virtual Machine Environment Engineering

### Step 1.1: Install Microsoft Visual C++ Core Prerequisites (Windows 10 Pro Host)
Before provisioning the hypervisor core, the host operating system runtime libraries must be satisfied to prevent initialization failures on Windows 10 Pro.
1. Open a web browser on your Windows 10 Pro host desktop and connect to the official Microsoft Visual Studio Support panel [https://microsoft.com].
2. Scroll to the **Visual Studio 2015, 2017, 2019, and 2022** matrix category section.
3. Click the architecture package download link for **`vc_redist.x64.exe`** (64-bit Windows platforms).
4. Run the downloaded executable as an Administrator, check the box to agree to the licensing terms and conditions, click **Install**, and restart your Windows 10 Pro host computer once setup completes.

![Microsoft Prerequisite Installed](screenshots/install_1_1_microsoft_prerequisite.png)





### Step 1.2: Oracle VM VirtualBox Hypervisor Deployment
1. Navigate to the official Oracle virtualization portal [https://virtualbox.org] and download the latest platform package installer for **Windows hosts**.
2. Run the downloaded executable file (`VirtualBox-7.2.14-174565-Win') using standard administrative privileges on your Windows 10 Pro machine.
3. Step through the setup wizard windows, keeping all default configuration options (Core components, networking interfaces, and shortcuts) selected.
4. Click **Install**, wait for the process bar to finish executing, and click **Finish** to open the VirtualBox Manager dashboard.


 **VirtualBox Manager interface displaying clean initialization state before VM provisioning** 

<img width="1366" height="768" alt="2026-09-23 (1)" src="https://github.com/user-attachments/assets/66269619-6cb8-4fa6-98e2-d33fea65dd02" />








### Step 1.3: Download the Ubuntu Jammy Jellyfish Operating System ISO
1. Access the official Ubuntu image distribution website [https://ubuntu.com].
2. Click on the download link for the **Ubuntu 22.04.5 LTS (Jammy Jellyfish) Desktop or Server ISO image**.
3. Save the file into your local Windows 10 Pro host computer's **Downloads** folder. Ensure the download completes fully before provisioning the VM.

---

### Step 1.4: Provision the Virtual Machine Shell in VirtualBox
1. Open the VirtualBox Manager console and click the **New** (blue star) button to open the VM creation wizard.
2. Configure the following parameter blocks:
   * **Name:** `Ubuntu(defender infrastructure)`
   * **Folder:** Leave as default path.
   * **ISO Image:** Click the dropdown arrow, select **Other**, browse to your Windows Downloads folder, and select your downloaded `ubuntu-22.04.5-live-server-amd64` file.
   * **Type:** Linux
   * **Version:** Ubuntu (64-bit)
3. Check the box for **Skip Unattended Installation** if prompted, then click **Next**.
4. Configure **Hardware Allocation**:
   * **Base Memory (RAM):** Allocate at least **4096 MB (4 GB)** for adequate Splunk engine performance.
   * **Processors (CPU):** Allocate at least **2 vCPUs**. Click **Next**.
5. Configure **Virtual Hard Disk**:
   * Select **Create a Virtual Hard Disk Now**.
   * Set the disk size space to a minimum allocation limit of **30 GB** to account for database log ingestion pipelines. Click **Next** and then click **Finish**.


**VirtualBox Manager panel showing the fresh Ubuntu(defender infrastructure) VM shell listed in the left-hand column**

<img width="1363" height="766" alt="ubuntu22" src="https://github.com/user-attachments/assets/8b0d5060-ee5b-43bc-bf0d-c150e11d6367" />

---

### Step 1.5: Install the Ubuntu 22.04 LTS (Jammy Jellyfish) Operating System Core
1. Highlight your newly configured VM in VirtualBox Manager and click the green **Start** arrow button to boot the machine.
2. Use your keyboard arrow keys to highlight **Try or Install Ubuntu** on the GRUB bootloader menu screen and hit **Enter**.
3. Select your preferred language setup and click **Install Ubuntu**.
4. Follow the configuration steps in sequence:
   * **Keyboard Layout:** Choose your layout and click Continue.
   * **Updates and Other Software:** Select **Normal installation** and check the box for **Download updates while installing Ubuntu**. Click Continue.
   * **Installation Type:** Choose **Erase disk and install Ubuntu** (this safely deletes the blank virtual disk inside the hypervisor, not your Windows 10 Pro host data). Click **Install Now**, then click **Continue** to confirm writing partition maps to the virtual disk.
   * **Where are you?** Select your current geographic timezone and click Continue.
5. Configure **User Persona Creation Details**:
   * **Your name:** `uzezi`
   * **Your computer's name:** `Ubuntu(defender infrastructure)`
   * **Pick a username:** `zez012`
   * **Choose a password:** Enter a strong password and re-enter it to verify.
   * Select **Require my password to log in**. Click Continue.
6. Wait for the file extraction script wizard to finish executing. Once the dialog box prompts you with *Installation Complete*, click **Restart Now**.
7. When the console text prints the instruction *Please remove the installation medium, then press ENTER*, tap the **Enter** key on your keyboard to complete the OS baseline initialization process.




 **Ubuntu 22.04 LTS Monitored Gateway presentation layout presenting a clean login verification portal**


<img width="800" height="600" alt="zez012" src="https://github.com/user-attachments/assets/2d5b7909-2211-4519-aafa-fb4f23501097" />






### Step 1.6: Dual-Adapter Network Topology Customization
To simulate an investigation where traffic comes from an external source IP, your Ubuntu VM needs to be accessible from your Windows 10 Pro host machine or a lab network.
1. Power off your Ubuntu VM in VirtualBox.
2. In VirtualBox under Ubuntu network settings:
   * Under **Adapter 1**, ensure it is checked "Enable Network Adapter" and attached to **NAT**.
   * Click on the **Adapter 2** tab, check the box to **Enable Network Adapter**, and change it to **Host-only Adapter**.



**VirtualBox Network configuration window displaying Adapter 1 (NAT) and Adapter 2 (Host-only) metrics**

<img width="1365" height="768" alt="gbese 1" src="https://github.com/user-attachments/assets/7e7a405a-8bb4-426d-945e-2362d8e89663" />
<img width="1349" height="764" alt="gbese 2" src="https://github.com/user-attachments/assets/ec020ed1-02c4-4767-a0ba-0ac9b591dfd7" />


---

## 📁 Section 2: Shared Folder Mount Configuration & File Transport

### Step 2.1: Hypervisor File Share Integration
Set up the shared folder in VirtualBox to pass installer files from the host to the guest machine.
1. Shut down your Ubuntu VM.
2. In VirtualBox Manager, select your VM ➔ **Settings** ➔ **Shared Folders**.
3. Add a new shared folder:
   * **Folder Path:** Choose your Windows Downloads folder.
   * **Folder Name:** e.g., `Downloads`.
   * Check **Auto-mount** and **Make Permanent** if you want it always available.


**Shared Folders settings panel showcasing the active mapping to my Windows Downloads folder**

<img width="1365" height="768" alt="cyborg" src="https://github.com/user-attachments/assets/ec3bce8d-4361-440a-8a3c-ee7ac49d0856" />






### Step 2.2: Mounting the Shared Directory Inside Ubuntu Core
Manually link the hypervisor folder mapping to a persistent path inside the guest shell.
1. Start your Ubuntu VM.
2. Mount the shared folder manually (if auto-mount didn't work):
   * Create a mount point:
     ```bash
     mkdir ~/windows_downloads
     ```
   * Mount the folder:
     ```bash
     sudo mount -t vboxsf Downloads ~/windows_downloads
     ```
     *(Here Downloads is the folder name you set in VirtualBox settings.)*


**Linux terminal executing the mount string with zero error flags generated**

<img width="800" height="600" alt="windows downloads" src="https://github.com/user-attachments/assets/655ac97d-e4c1-40b7-acc2-bd74f3b7cf25" />







### Step 2.3: Automating Mount Persistence on Boot
Modify system initialization maps to enforce permanent file share attachment across operating system reboot windows.
1. Navigating to the folder:
   ```bash
   cd ~/windows_downloads
   ls
   ```
   *`cd` moves you into the folder. `ls` lists the files inside (you should see the contents of your Windows Downloads folder).*
2. **Tip:** If you want this to mount automatically on boot, add the following line to `/etc/fstab`:
   ```text
   Downloads /home/zez012/windows_downloads vboxsf defaults 0 0
   ```
3. Open the file with root privileges:
   ```bash
   sudo nano /etc/fstab
   ```
4. Scroll to the bottom and add:
   ```text
   Downloads /home/zez012/windows_downloads vboxsf defaults 0 0
   ```
  
5. Save and exit (`Ctrl+O`, `Enter`, then `Ctrl+X` in nano).
6. Apply the changes by either rebooting or running:
   ```bash
   sudo mount -a
   ```
   *This will mount everything listed in `/etc/fstab` immediately.*
7. **✔ Verify:** Check that your Windows Downloads folder is accessible:
   ```bash
   ls ~/windows_downloads
   ```

```text
