**`04-Display-Manager-Setup.md`**: Setting up and configuring `ly` as your display manager.

This file will cover installing and enabling `ly`, a lightweight and minimalist display manager, to give you a clean graphical login screen for your Hyprland environment.

---

# `04 - Display Manager Setup with` ly`

You've got Hyprland installed, and now it's time to add a display manager. A display manager (DM) provides a graphical login screen, allowing you to select your user and desktop environment (in our case, Hyprland) before launching.

We've chosen `ly` because it's incredibly lightweight, simple, and terminal-based, perfectly aligning with our minimalist, black-and-white aesthetic.

**Your Learning Journey Continues:** As always, **type out every command yourself**. This ensures you understand each action and reinforces your command-line skills.

---

## 1. Install `ly` Display Manager

`ly` is available in the Arch User Repository (AUR), so we'll use `yay` to install it.

Bash

```
yay -S ly
```

_Explanation_: `yay -S ly` will download the `PKGBUILD` for `ly`, resolve its dependencies, build the package, and install it on your system. You'll likely be prompted to review changes and confirm the installation.

---

## 2. Enable `ly` Systemd Service

After installation, `ly` needs to be enabled as a systemd service so that it starts automatically when your system boots.

Bash

```
sudo systemctl enable ly
```

_Explanation_:

- `sudo`: Executes the command with superuser privileges.
    
- `systemctl`: The command-line utility for controlling `systemd` services.
    
- `enable`: Configures `ly.service` to start automatically at boot.
    

You can verify its status (though it won't be active until you reboot):

Bash

```
sudo systemctl status ly
```

You should see `Loaded: loaded (.../ly.service; enabled; vendor preset: disabled)`

---

## 3. Configure Hyprland Session for `ly`

`ly`, like other display managers, needs to know how to launch your chosen desktop environment or Wayland compositor. Hyprland typically provides a `.desktop` file that tells display managers how to launch it. We'll verify its presence.

By default, Hyprland installs its session file to `/usr/share/wayland-sessions/hyprland.desktop`. Let's confirm it exists:

Bash

```
ls /usr/share/wayland-sessions/hyprland.desktop
```

_Explanation_: This command lists the file. If it exists, you should see its path printed. If it doesn't, you might need to ensure Hyprland was installed correctly or check the Hyprland installation guide/wiki for details on its session file location. (In most cases, with `hyprland` package, this file is automatically installed).

**Content of `hyprland.desktop` (for reference - you usually don't need to edit this):**

```
[Desktop Entry]
Name=Hyprland
Comment=A dynamic tiling Wayland compositor
Exec=Hyprland
Type=Application
```

The crucial line here is `Exec=Hyprland`, which tells `ly` (and other DMs) to simply execute the `Hyprland` command to start your session.

---

## 4. Reboot and Test `ly`

Now that `ly` is installed and enabled, it's time to reboot your system. If everything is set up correctly, you should be greeted by the `ly` login screen.

Bash

```
sudo reboot
```

_Explanation_: Reboots your system.

**After rebooting:**

1. You should see the minimalist `ly` login screen.
    
2. Enter your username and press Enter.
    
3. Enter your password and press Enter.
    
4. `ly` should then launch your Hyprland session.
    

---

## 5. Troubleshooting `ly`

If `ly` doesn't appear or Hyprland fails to launch after login:

- **Black Screen after `ly` login:**
    
    - This usually means Hyprland failed to start. Switch to a TTY (`Ctrl+Alt+F2` or `F3`), log in, and check Hyprland's logs:
        
        Bash
        
        ```
        cat /tmp/hypr/<your_username>.log
        ```
        
    - Review the `03-Hyprland-Installation.md` steps, especially GPU driver installation and environment variables for NVIDIA users.
        
- **`ly` doesn't show up at all (still just a text login):**
    
    - Ensure `ly` is enabled: `sudo systemctl status ly`. If it's not enabled, run `sudo systemctl enable ly` and reboot.
        
    - Check `ly`'s journal for errors: `sudo journalctl -u ly --pager`.
        
    - Ensure there are no other display managers enabled. Only one display manager should be enabled at a time. If you had `gdm`, `sddm`, etc., enabled previously, disable them: `sudo systemctl disable <other_dm_service_name>`.
        
- **No Wayland Session Option in `ly` (if you see a session selection):**
    
    - This is unlikely with `ly` as it's very minimal, but if there's ever a session selection, ensure the `hyprland.desktop` file is correctly placed in `/usr/share/wayland-sessions/`.
        

---

## 6. Next Steps

You now have a clean Arch Linux installation that boots into a minimalist `ly` display manager, which in turn launches your Hyprland compositor. You're ready to personalize and configure your Hyprland environment with dotfiles.

Proceed to the next stage of your journey: **[05-Dotfiles-Configuration.md](https://www.google.com/search?q=05-Dotfiles-Configuration.md)**.