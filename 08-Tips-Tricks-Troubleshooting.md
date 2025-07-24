**`08-Tips-Tricks-Troubleshooting.md`**: Optimization Tips, Common Issues, and Solutions.

This guide will equip you with the knowledge to maintain your Arch Linux Hyprland system, optimize its performance, and tackle common problems you might encounter. This concludes the initial setup phase, providing you with a solid foundation.

**Continue to Learn and Explore:** The journey with Arch Linux is continuous. **Explore these tips, understand the commands, and use them as a starting point** for deeper customization and problem-solving.

---

# `08 - Tips, Tricks & Troubleshooting`

You've successfully installed and configured your Hyprland Arch Linux system! This final guide provides essential information for day-to-day maintenance, common optimizations, and solutions to frequently encountered issues.

---

## 1. System Maintenance & Updates

Keeping your Arch Linux system updated is paramount. Arch is a rolling release, meaning you get the latest software constantly.

### A. Regular System Updates

Always update your system regularly (e.g., weekly or bi-weekly). This ensures you have the latest features, bug fixes, and security patches.

Bash

```
sudo pacman -Syu && yay -Syu
```

_Explanation_:

- `sudo pacman -Syu`: Updates packages from the official repositories.
    
- `&&`: Ensures the next command runs only if the previous one was successful.
    
- `yay -Syu`: Updates packages from both official repositories and the AUR.
    

**Always read the `pacman` output!** Pay attention to warnings about configuration file changes (`.pacnew` files) and any specific instructions for major package updates.

### B. Clean Package Cache

Over time, `pacman` stores downloaded packages in its cache (`/var/cache/pacman/pkg/`). This can consume significant disk space.

- **Remove old package versions (keep latest 3):**
    
    Bash
    
    ```
    sudo paccache -rk3
    ```
    
    _Explanation_: Keeps the last 3 versions of each package in the cache.
    
- **Remove all but the latest version:**
    
    Bash
    
    ```
    sudo paccache -rk1
    ```
    
- **Clear all cached packages (use with caution, if you need to downgrade, you'll have to redownload):**
    
    Bash
    
    ```
    sudo pacman -Scc
    ```
    
    _Explanation_: Removes all files from the cache.
    

### C. Remove Orphan Packages

Orphan packages are dependencies that were installed for a specific program but are no longer needed after that program has been removed.

Bash

```
sudo pacman -Rns $(pacman -Qtdq)
```

_Explanation_:

- `pacman -Qtdq`: Lists all orphan packages.
    
- `pacman -Rns`: Removes packages and their unneeded dependencies (including config files).
    

---

## 2. Performance & Optimization Tips

Small tweaks can enhance your system's responsiveness and efficiency.

### A. Swappiness

`swappiness` is a kernel parameter that controls how aggressively the kernel uses swap space. A lower value means the system will try to keep more data in RAM, which can improve performance on systems with sufficient RAM.

1. **Check current swappiness:**
    
    Bash
    
    ```
    cat /proc/sys/vm/swappiness
    ```
    
    (Default is usually 60)
    
2. **Set swappiness to a lower value (e.g., 10):** Create a new sysctl configuration file:
    
    Bash
    
    ```
    echo "vm.swappiness=10" | sudo tee /etc/sysctl.d/99-swappiness.conf
    ```
    
    _Explanation_: This sets swappiness to 10. You can choose any value between 0 and 100. For desktops with plenty of RAM (8GB+), a value like 10 is often good. For systems with less RAM, a higher value might be necessary.
    
3. **Apply changes immediately (or reboot):**
    
    Bash
    
    ```
    sudo sysctl -p /etc/sysctl.d/99-swappiness.conf
    ```
    

### B. ZRAM (Alternative to Disk Swap)

Consider using ZRAM if you have plenty of RAM (e.g., 8GB+) and want faster "swap" that uses compressed RAM instead of disk. This can extend RAM life and improve responsiveness.

1. **Install `zram-generator`:**
    
    Bash
    
    ```
    yay -S zram-generator
    ```
    
2. **Configure ZRAM:** Create/edit `/etc/systemd/zram-generator.conf`:
    
    Bash
    
    ```
    sudo nano /etc/systemd/zram-generator.conf
    ```
    
    Ini, TOML
    
    ```
    [zram0]
    zram-size = min(ram / 2, 4096)  # Use half of RAM, up to 4GB
    compression-algorithm = zstd    # Fast compression algorithm
    fs-type = swap
    ```
    
    _Explanation_: This configures a 4GB ZRAM device (or half your RAM, whichever is smaller) using `zstd` compression. Adjust `zram-size` as needed.
    
3. **Disable your disk swap (if you have one):** First, disable it:
    
    Bash
    
    ```
    sudo swapoff -a
    ```
    
    Then, remove its entry from `/etc/fstab` (comment it out with `#` or delete the line) to prevent it from activating on boot.
    
    Bash
    
    ```
    sudo nano /etc/fstab
    ```
    
    (Look for the line containing `swap` and `/dev/sdX` or similar, then comment it out).
    
4. **Reboot for ZRAM to take effect.**
    

### C. Hyprland Animations

While beautiful, complex animations can sometimes affect performance, especially on less powerful hardware. You can adjust or disable them in `~/.config/hypr/hyprland.conf`.

Look for sections like `animation { ... }` and `cursor_inactive_timeout`. You can reduce `animation_speed` or set `enabled = false` for specific animations.

### D. Startup Applications

Review your `exec-once` commands in `~/.config/hypr/hyprland.conf`. Only run essential applications at startup to keep your system lean.

---

## 3. Common Issues & Solutions

### A. Wayland Specific Issues

- **Screen Tearing / Stuttering**:
    
    - **GPU Drivers**: The most common culprit. Ensure you have the correct and latest proprietary (NVIDIA) or open-source (AMD/Intel Mesa) drivers installed.
        
    - **NVIDIA**: Verify all required environment variables are set in your `hyprland.conf` or `~/.profile`:
        
        Ini, TOML
        
        ```
        env = WLR_NO_CC_BUFFER_ALLOCATOR,1
        env = __GLX_VENDOR_LIBRARY_NAME,nvidia
        env = LIBVA_DRIVER_NAME,nvidia # For hardware video acceleration
        ```
        
    - **Monitor Refresh Rate**: Ensure your monitor's refresh rate is correctly detected and set in `hyprland.conf`.
        
- **XWayland (X11) Apps Look Blurry / Wrongly Scaled**: This happens if your Wayland compositor is scaling, but the XWayland app isn't aware.
    
    - **Environment Variable**: You can try setting `XWAYLAND_SCALE=1` or `XWAYLAND_SCALE=2` (for 2x scaling) in your `hyprland.conf` or `~/.profile`.
        
    - **Application Specific**: Some X11 applications have their own scaling settings.
        
    - **Prefer Native Wayland Apps**: When possible, use native Wayland applications (like `foot`, `mpv`, `firefox` with Wayland backend enabled) to avoid XWayland issues.
        
- **Copy/Paste Not Working Reliably**: Ensure `wl-clipboard` is installed. If using `cliphist`, confirm it's running (usually autostarted via `hyprland.conf`). Some applications might have specific Wayland clipboard issues; restarting them often helps.
    
- **Screen Sharing Not Working (e.g., in Chrome/Firefox on Discord/Zoom)**: Ensure `xdg-desktop-portal-hyprland` is installed and running. Check your browser for specific Wayland compatibility settings (e.g., `chrome://flags/#enable-webrtc-pipewire-capturer` for Chrome/Chromium).
    

### B. Audio Issues

- **No Sound**:
    
    - Verify Pipewire status: `systemctl --user status pipewire pipewire-pulse`. If not running, enable them: `systemctl --user enable --now pipewire pipewire-pulse`.
        
    - Check `pavucontrol` (PulseAudio Volume Control) settings: `pavucontrol`. Ensure output devices are selected and volumes aren't muted.
        
    - Check if the correct audio output is selected in your `hyprland.conf` for Waybar's volume module, or if you use `pamixer`.
        

### C. Brightness / Volume Keys Not Working

- You need utilities and keybindings.
    
    - For **Brightness**: Install `brightnessctl`.
        
        Bash
        
        ```
        yay -S brightnessctl
        ```
        
        Then add keybinds to `~/.config/hypr/hyprland.conf`:
        
        Ini, TOML
        
        ```
        # Example brightness binds
        bind = , XF86MonBrightnessUp, exec, brightnessctl set +5%
        bind = , XF86MonBrightnessDown, exec, brightnessctl set 5%-
        ```
        
        (Adjust `XF86MonBrightnessUp`/`Down` if your keys are different; use `wev` to find keycodes.)
        
    - For **Volume**: Install `pamixer` (for Pipewire/PulseAudio).
        
        Bash
        
        ```
        yay -S pamixer
        ```
        
        Then add keybinds to `~/.config/hypr/hyprland.conf`:
        
        Ini, TOML
        
        ```
        # Example volume binds
        bind = , XF86AudioRaiseVolume, exec, pamixer --allow-boost -i 5
        bind = , XF86AudioLowerVolume, exec, pamixer --allow-boost -d 5
        bind = , XF86AudioMute, exec, pamixer -t
        ```
        
        (Again, adjust keycodes as needed.)
        

### D. Fonts Look Bad / Missing Glyphs (Boxes instead of characters)

- Ensure you have installed comprehensive font packages.
    
    - `yay -S noto-fonts noto-fonts-emoji ttf-dejavu ttf-liberation terminus-font`
        
- If emojis show as boxes, it's definitely a missing emoji font.
    
- Ensure your `fontconfig` cache is up-to-date (usually happens automatically but can be forced): `fc-cache -fv`.
    

### E. Polkit Authentication Issues (e.g., Can't Mount Drives, Admin Prompts)

- If you're prompted for a password by a graphical application (like Thunar when mounting a drive) and nothing happens or it fails:
    
    - Ensure `polkit-gnome` is running. It should be started by `exec-once` in your `hyprland.conf`:
        
        Ini, TOML
        
        ```
        exec-once = /usr/lib/polkit-gnome/polkit-agent-helper-1
        # Or if you prefer the full command:
        # exec-once = /usr/lib/polkit-gnome/polkit-agent-helper-1 &
        ```
        
    - Also ensure your user is in the `wheel` group for `sudo` and PolicyKit privileges (as set in `01-Arch-Installation.md`).
        

---

## 4. Where to Find More Help

The Arch Linux community and documentation are incredibly robust.

- **Arch Wiki**: The absolute best resource for anything Arch Linux. Almost every problem or package has a dedicated wiki page.
    
    - [https://wiki.archlinux.org/](https://wiki.archlinux.org/)
        
- **Hyprland Wiki**: Comprehensive documentation for Hyprland itself.
    
    - [https://wiki.hyprland.org/](https://wiki.hyprland.org/)
        
- **Community Forums/Subreddits**:
    
    - r/archlinux on Reddit
        
    - r/hyprland on Reddit
        
    - Official Arch Linux Forums
        

---

## Congratulations on Your HyprArch-BW-Journey!

You have successfully built a functional, minimalist, and aesthetically consistent Arch Linux system with the Hyprland compositor. This journey has covered everything from base installation to advanced theming and troubleshooting.

This is not the end, but merely the beginning of your personalized Linux experience. Continue to explore, customize, and refine your setup. The power of Arch Linux lies in its flexibility and your ability to shape it precisely to your needs.

Enjoy your Hyprland desktop!