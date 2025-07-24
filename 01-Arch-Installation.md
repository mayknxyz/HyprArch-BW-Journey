````
# 01 - Arch Linux Base Installation with `archinstall`

Welcome to the beginning of your HyprArch-BW-Journey! This first step will guide you through installing the Arch Linux base system using the `archinstall` script, a powerful and user-friendly automated installer. This guide assumes you are starting with a fresh system and an Arch Linux ISO USB drive.

While `archinstall` automates many steps, understanding the prompts and making informed choices is part of the learning process.

**Important Philosophy for this Journey:**
Even with automation, we encourage you to **read every prompt carefully and type out any necessary inputs yourself**. This active participation helps build familiarity with the installation process and deepens your comprehension of each choice.

---

## 1. Prepare Your Installation Media

First, you need to download the Arch Linux ISO and create a bootable USB drive.

1.  **Download the Arch Linux ISO:**
    Visit the official Arch Linux website and download the latest ISO:
    [https://archlinux.org/download/](https://archlinux.org/download/)

2.  **Create a Bootable USB Drive using Ventoy (Recommended):**
    Ventoy is an excellent tool that allows you to create a bootable USB drive once, and then simply copy ISO files directly to it. This means you can store multiple ISOs on one drive and simply boot from them.

    * **Steps to use Ventoy:**
        1.  **Download Ventoy:** Go to [https://www.ventoy.net/en/download.html](https://www.ventoy.net/en/download.html) and download the appropriate package for your operating system (Windows, Linux, macOS).
        2.  **Install Ventoy to your USB drive:**
            * **On Windows:** Extract the downloaded `.zip` file. Run `Ventoy2Disk.exe`. Select your USB drive from the "Device" dropdown. Click "Install" and confirm the warning messages (this will format the USB drive).
            * **On Linux:** Extract the downloaded `.tar.gz` file. Navigate into the extracted directory in your terminal. Run `sudo sh Ventoy2Disk.sh -i /dev/sdX` (replace `/dev/sdX` with your USB device, e.g., `/dev/sdb` or `/dev/sdc` - **be extremely careful to select the correct device!** You can find your device using `lsblk`). Confirm the installation.
        3.  **Copy the Arch Linux ISO:** Once Ventoy is installed on your USB, it will have two partitions (one hidden, one exFAT/NTFS). Simply **copy your downloaded `archlinux.iso` file directly into the large exFAT/NTFS partition** on the Ventoy USB drive. Do NOT use `dd` or other flashing tools after installing Ventoy.

    * **Alternative: Balena Etcher (GUI) or `dd` (CLI):**
        If you prefer a more traditional method or are having issues with Ventoy:

        * **Balena Etcher (Cross-platform: Windows, macOS, Linux):**
            A very user-friendly graphical tool.
            1.  Download Balena Etcher from [https://www.balena.io/etcher/](https://www.balena.io/etcher/).
            2.  Install (if necessary) and launch Etcher.
            3.  Click "Flash from file" and select your downloaded Arch Linux ISO.
            4.  Click "Select target" and choose your USB drive. **Double-check that you select the correct drive!**
            5.  Click "Flash!" and wait for the process to complete.

        * **`dd` (Linux/macOS - Command Line):**
            For command-line enthusiasts.
            * First, find your USB device. **Be extremely careful!**
                ```bash
                lsblk # On Linux
                diskutil list # On macOS
                ```
                Identify your USB drive (e.g., `/dev/sdX` on Linux, `/dev/diskX` on macOS).
            * Unmount the drive if it's mounted (replace `/dev/sdX` with your drive):
                ```bash
                sudo umount /dev/sdX* # On Linux
                sudo diskutil unmountDisk /dev/diskX # On macOS
                ```
            * Write the ISO (replace `/path/to/your/archlinux.iso` and `/dev/sdX`):
                ```bash
                sudo dd bs=4M if=/path/to/your/archlinux.iso of=/dev/sdX status=progress && sync
                ```

---

## 2. Boot into the Arch Linux Live Environment

Once your USB is ready, insert it into your target machine and boot from it. You may need to enter your BIOS/UEFI settings (often by pressing `F2`, `F10`, `F12`, or `Del` during startup) to set the boot order or select the USB drive.

Upon successful boot, you will be greeted by the Arch Linux boot menu (or Ventoy's menu, from which you select the Arch Linux ISO). Select **"Arch Linux install medium (x86_64)"** and press Enter.

You will land in a Zsh shell with a root prompt (e.g., `root@archiso ~#`).

---

## 3. Connect to the Internet (If Not Already Connected)

While `archinstall` can manage networking, it's a good idea to ensure you have a connection before starting the script, especially for wireless.

### A. For Wired Connection (Ethernet)

Usually, a wired connection will be automatically configured via DHCP. Test it:

```bash
ping -c 3 archlinux.org
````

_Explanation_: `ping -c 3` sends 3 packets to `archlinux.org`. If you see replies, your connection is working. If not, check your cable or router.

### B. For Wireless Connection (Wi-Fi)

1. **Scan for Wi-Fi networks:**
    
    Bash
    
    ```
    iwctl device list
    ```
    
    _Explanation_: Lists your wireless devices (e.g., `wlan0`).
    
    Bash
    
    ```
    iwctl station <device_name> scan
    ```
    
    _Explanation_: Scans for available Wi-Fi networks using your device name. Replace `<device_name>` (e.g., `wlan0`).
    
2. **List available networks:**
    
    Bash
    
    ```
    iwctl station <device_name> get-networks
    ```
    
    _Explanation_: Shows the scanned networks.
    
3. **Connect to your network:**
    
    Bash
    
    ```
    iwctl station <device_name> connect <SSID_of_your_network>
    ```
    
    _Explanation_: Connects to your chosen Wi-Fi network. Replace `<SSID_of_your_network>` with the actual name of your network. You will be prompted for the password if it's protected.
    
4. **Verify connection:**
    
    Bash
    
    ```
    ping -c 3 archlinux.org
    ```
    
    _Explanation_: Check for replies.
    

---

## 4. Launch the `archinstall` Script

Once you have internet access, you can launch the installer.

Bash

```
archinstall
```

_Explanation_: This command starts the interactive `archinstall` script. You will be guided through a series of questions and options. Read each one carefully!

---

## 5. Navigating `archinstall` - Key Choices for HyprArch-BW-Journey

The `archinstall` script is quite intuitive, but here are the key choices and considerations to make for our setup. The specific order of questions might vary slightly with `archinstall` updates, but the concepts remain the same.

- **Language & Keyboard Layout:**
    
    - Select your preferred language.
        
    - Choose your keyboard layout.
        
- **Mirrors:**
    
    - `archinstall` will often detect the fastest mirrors for you. Confirm or choose mirrors geographically close to your location (e.g., for users in Davao City, look for mirrors in the Philippines, Singapore, Hong Kong, or other nearby regions for optimal speed).
        
- **Disk Configuration:**
    
    - **Disk Selection:** Choose the drive where you want to install Arch Linux. **Be extremely careful here to select the correct drive! This will wipe the chosen disk.** (e.g., `/dev/sda`, `/dev/nvme0n1`).
        
    - **Partitioning Scheme:**
        
        - **Recommended: `Wipe all selected drives and use a default layout`**. This is the easiest for beginners and what we'll base our Hyprland setup on (UEFI boot, swap, root).
            
        - Alternatively, you can choose `Manual partitioning` if you have specific needs, but that's beyond the scope of this beginner-friendly guide.
            
    - **Filesystem:** `ext4` for the root partition is a solid choice.
        
    - **Swap:** `archinstall` will usually create a swap partition. Confirm its size (typically 4GB or more, depending on your RAM and if you plan to hibernate).
        
    - **Encryption:** For this basic guide, we'll assume **no disk encryption**. If you desire encryption, `archinstall` supports it, but it adds complexity.
        
- **Bootloader:**
    
    - Choose **`GRUB`**. It's robust and widely used.
        
- **Hostname:**
    
    - Enter your desired hostname (e.g., `myarchhypr`).
        
- **Root Password:**
    
    - Set a strong password for the `root` user.
        
- **User Account:**
    
    - **Create a new user.** This is highly recommended for daily use.
        
    - Set a username (e.g., `<your_username>`).
        
    - Set a strong password for this user.
        
    - **Crucial:** When prompted, select **`Add this user to the sudoers file? (Recommended)`**. This will allow your new user to run commands with `sudo` privileges.
        
- **Profile:**
    
    - **Do NOT select a Desktop Environment profile** (like Plasma, GNOME, Xfce, etc.). For our Hyprland setup, we want a minimal base.
        
    - Select **`minimal`** or ensure no graphical profile is chosen. We will install Hyprland and its components manually in later steps to ensure a clean, customized setup.
        
- **Audio Server:**
    
    - Choose **`Pipewire`**. It's the modern standard for audio in Linux and works well with Wayland.
        
- **Kernels:**
    
    - Keep the default **`linux`** kernel.
        
- **Additional Packages:**
    
    - This is important! We need some initial packages to make the next steps smoother. When prompted, enter these space-separated packages:
        
        ```
        git vim nano networkmanager dhcpcd reflector
        ```
        
        - `git`: For cloning dotfiles later.
            
        - `vim` & `nano`: Text editors you'll definitely need.
            
        - `networkmanager`: For easier network management post-installation.
            
        - `dhcpcd`: A DHCP client.
            
        - `reflector`: To update Pacman mirror lists.
            
- **Network Configuration:**
    
    - Choose **`Use NetworkManager`**. This aligns with the `networkmanager` package we installed.
        
- **Timezone:**
    
    - Select your geographical region and city (e.g., `Asia -> Manila`).
        
- **Optional Repositories:**
    
    - Enable **`multilib`**. This repository is needed for some 32-bit applications and certain games.
        
- **Save Configuration:**
    
    - `archinstall` will ask if you want to save your configuration. You can choose `Yes` if you want a JSON file of your settings for future reference or automated installations.
        
- **Install:**
    
    - Confirm your choices and let `archinstall` proceed with the installation. This will take some time.
        

---

## 6. Post-Installation Steps (After `archinstall` Completes)

Once `archinstall` finishes, it will give you options.

1. **Chroot into the new system?**
    
    - **Select `Yes`**. This is highly recommended so you can perform initial post-installation tasks directly. You will be placed in a `chroot` environment.
        
2. **Exit `chroot` and Reboot:** If you did _not_ choose to chroot, or once you are done inside the chroot environment after performing any immediate tweaks:
    
    Bash
    
    ```
    exit       # If you are still in chroot
    sudo reboot
    ```
    
    _Explanation_: Your system will now reboot. **Remember to remove your USB installation media** when prompted or before the system tries to boot from it again.
    

---

**Congratulations!** If all went well, your system should now boot into your newly installed Arch Linux. You can log in using the user account you created during the `archinstall` process.

You're ready for the next stage of your HyprArch-BW-Journey!