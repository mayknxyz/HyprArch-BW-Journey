03-Hyprland-Installation.md: Installing Hyprland and its core dependencies.

This step will get the heart of your desktop environment – Hyprland – up and running, along with the necessary graphics drivers and Wayland components.

03-Hyprland-Installation
With your Arch Linux base system prepared, it's time to install Hyprland, the dynamic Wayland compositor that will power your new desktop. This guide also covers installing the necessary GPU drivers and other core dependencies.

Remember our learning philosophy:
Type out every command yourself. This isn't just about getting a working system; it's about building your understanding and proficiency.

1. Install GPU Drivers
This is a critical step for a smooth Hyprland (Wayland) experience. Incorrect or missing drivers can lead to issues like black screens, tearing, or poor performance. Identify your GPU and install the appropriate drivers.

You can often identify your GPU using:

Bash

lspci -k | grep -EA3 'VGA|3D|Display'
A. For AMD GPUs
Bash

sudo pacman -S --needed mesa vulkan-radeon libva-mesa-driver mesa-vdpau
Explanation:

mesa: Open-source graphics drivers for AMD, Intel, and integrated GPUs.

vulkan-radeon: Vulkan driver for AMD Radeon GPUs.

libva-mesa-driver: VA-API (Video Acceleration API) driver for Mesa.

mesa-vdpau: VDPAU (Video Decode and Presentation API for Unix) driver for Mesa.

B. For Intel GPUs
Bash

sudo pacman -S --needed mesa vulkan-intel libva-mesa-driver mesa-vdpau intel-media-driver
Explanation:

mesa: Open-source graphics drivers for AMD, Intel, and integrated GPUs.

vulkan-intel: Vulkan driver for Intel GPUs.

libva-mesa-driver: VA-API driver for Mesa.

mesa-vdpau: VDPAU driver for Mesa.

intel-media-driver: For hardware video acceleration on newer Intel GPUs. (Older GPUs might need libva-intel-driver).

C. For NVIDIA GPUs (Proprietary Drivers)
NVIDIA drivers on Wayland can be more complex and require specific Hyprland configurations. It is generally recommended to use the latest proprietary drivers.

Bash

sudo pacman -S --needed nvidia nvidia-dkms nvidia-utils nvidia-settings
Explanation:

nvidia: The main NVIDIA proprietary driver.

nvidia-dkms: Ensures the NVIDIA kernel module is rebuilt automatically when your kernel updates.

nvidia-utils: Utilities for NVIDIA drivers.

nvidia-settings: NVIDIA X Server Settings application.

Important for NVIDIA Users:
After installing NVIDIA drivers, you will likely need to add specific environment variables for Hyprland to function correctly. This will be covered in the 04-Dotfiles-Configuration.md file, but be aware that NVIDIA on Wayland requires extra steps. You will typically need to set WLR_NO_CC_BUFFER_ALLOCATOR=1 and __GLX_VENDOR_LIBRARY_NAME=nvidia among others.

2. Install Hyprland and Core Wayland Components
Now, let's install Hyprland itself and other essential packages for a complete Wayland environment.

Bash

yay -S --needed hyprland xorg-xwayland xdg-desktop-portal-hyprland qt5-wayland qt6-wayland glfw-wayland polkit-gnome foot
Explanation:

hyprland: The Hyprland Wayland compositor.

xorg-xwayland: Provides an X server that runs on Wayland, allowing older X11 applications to run seamlessly. This is crucial for compatibility.

xdg-desktop-portal-hyprland: Implements XDG Desktop Portal interfaces for Hyprland, improving compatibility with Flatpak/Snap apps and allowing screen sharing, file pickers, etc.

qt5-wayland, qt6-wayland: Wayland backend for Qt 5 and Qt 6 applications, ensuring they run natively on Wayland.

glfw-wayland: GLFW (Graphics Library Framework) support for Wayland, needed by some games and applications.

polkit-gnome: A PolicyKit authentication agent. Many applications require a PolicyKit agent for administrative actions (e.g., mounting drives, managing power settings). This is a common and reliable choice.

foot: A fast, lightweight Wayland-native terminal emulator. You chose Ghostty in the README.md, but foot is a good initial native terminal to ensure things work, and you can install ghostty later in the applications section. (We'll update 06-Essential-Applications.md to include ghostty).

Self-correction for Ghostty: Ghostty is not usually in the official repos or AUR directly. It's often installed by downloading a binary or building from source. Given the "learning journey" and focus on commands, foot is a good initial pacman/yay install. We'll adjust 06-Essential-Applications.md to guide users on installing Ghostty via its preferred method (likely binary download/build). For now, foot ensures a working terminal for debugging Hyprland.

3. Verify Pipewire Audio Setup (Optional Check)
archinstall should have configured Pipewire as your audio server, which is the recommended choice for Wayland. You can quickly verify its status.

Bash

systemctl --user status pipewire pipewire-pulse
Explanation: Checks the status of the user-level Pipewire and Pipewire-Pulse services. Both should show active (running). If they are not running, you might need to enable them:

Bash

systemctl --user enable --now pipewire pipewire-pulse
4. First Test Launch of Hyprland (from TTY)
Before setting up your display manager, it's good practice to attempt launching Hyprland directly from a TTY (text-only terminal) to ensure basic functionality.

Log out of your current session (if you're still logged in after installing packages):

Bash

exit
(Or simply press Ctrl+Alt+F2 to switch to a new TTY if you're in a graphical session or current TTY).

Log in to a fresh TTY:
Log in with your non-root user credentials.

Start Hyprland:

Bash

Hyprland
Explanation: This command attempts to start the Hyprland Wayland compositor.

What to expect: If successful, your screen should change, and you might see a basic Hyprland desktop (likely just a black screen or a minimal wallpaper, and you'll need mod+q to open a terminal or similar if you have basic configs). If you press mod+q (default keybind for terminal in default Hyprland config), a foot terminal should launch if it's the default.

To exit Hyprland: Usually mod+shift+e (or mod+q if it's the exit bind) will bring up a logout/exit prompt. If nothing works, Ctrl+Alt+F1 (or F2-F6) to switch back to a TTY and kill the Hyprland process.

Troubleshooting the First Launch:

Black Screen / Nothing Happens / Returns to TTY:

Check GPU Drivers: Did you install the correct drivers for your hardware?

Check Hyprland Logs: After attempting to launch, switch back to the TTY (Ctrl+Alt+F1 or F2) and look for Hyprland's logs. They are usually in /tmp/hypr/<username>.log. You can view them with cat /tmp/hypr/<your_username>.log or less /tmp/hypr/<your_username>.log. Look for ERROR messages.

xorg-xwayland missing: Ensure you installed xorg-xwayland.

Environment Variables: For NVIDIA users especially, missing WLR_NO_CC_BUFFER_ALLOCATOR=1 or __GLX_VENDOR_LIBRARY_NAME=nvidia can cause issues. These are set in ~/.profile or ~/.zprofile and will be part of the dotfile setup. For a first launch test, you might need to export them directly in the TTY before running Hyprland:

Bash

export WLR_NO_CC_BUFFER_ALLOCATOR=1
export __GLX_VENDOR_LIBRARY_NAME=nvidia
Hyprland
(Only for NVIDIA users as a test, these will be persistent later).

System Up-to-Date: Ensure sudo pacman -Syu was run successfully.

5. Next Steps
You've successfully installed Hyprland and its essential components! Now that the core graphical environment is in place, we'll set up the display manager to provide a graphical login.

Proceed to the next stage of your journey: 04-Display-Manager-Setup.md.