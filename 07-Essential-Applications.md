**`07-Essential-Applications.md`**: Installing and Configuring Common Applications for Daily Use.

Now that your system is installed, themed, and Hyprland is configured, it's time to populate it with the applications you'll use daily. We'll focus on selecting applications that either inherently support dark modes, are highly themeable, or fit naturally into our black-and-white aesthetic.

**Empower Your Workflow:** As you install these applications, **type each command carefully**. Observe how each new tool integrates (or needs to be integrated) into your minimalist, monochrome workspace.

---

# `07 - Essential Applications`

A beautiful desktop is nothing without the tools to get things done. This section will guide you through installing a set of common and essential applications that complement a minimalist Hyprland setup and can be easily configured to match your black-and-white theme.

We'll primarily use `yay` for installation, as it handles both official repositories and the Arch User Repository (AUR).

---

## 1. File Manager: Thunar

Thunar is a lightweight and feature-rich file manager from the Xfce desktop environment. It's GTK-based and integrates well with our theming.

Bash

```
yay -S thunar thunar-archive-plugin thunar-volman file-roller tumbler gvfs udisks2
```

_Explanation_:

- `thunar`: The main file manager application.
    
- `thunar-archive-plugin`: Adds archive creation/extraction context menu options.
    
- `thunar-volman`: Manages removable drives.
    
- `file-roller`: GNOME's archive manager, often used by Thunar's plugin.
    
- `tumbler`: Thumbnail service for Thunar.
    
- `gvfs`: Virtual filesystem support for network shares, trash, etc.
    
- `udisks2`: Disk management service.
    

---

## 2. Web Browser: Firefox

Firefox is a powerful and open-source web browser that supports extensive theming, including excellent dark mode capabilities.

Bash

```
yay -S firefox
```

_Explanation_: Installs the Firefox web browser.

**Post-Installation (Firefox):**

- Open Firefox.
    
- Go to `about:preferences` (or Settings -> General -> Language and Appearance).
    
- Under "Language and Appearance", set "Website appearance" to **`Dark`**.
    
- You might want to explore browser add-ons like "Dark Reader" for websites that don't have native dark modes.
    

---

## 3. Text Editor (Graphical): VS Code

Visual Studio Code (VS Code) is a popular, highly customizable code editor with excellent dark theme support and a vast extension ecosystem. It's available in the AUR.

Bash

```
yay -S visual-studio-code-bin
```

_Explanation_: Installs the VS Code binary from the AUR.

**Post-Installation (VS Code):**

- Open VS Code.
    
- Go to `File -> Preferences -> Theme -> Color Theme`.
    
- Choose a dark theme like **`Dark+ (default dark)`** or search the marketplace for black-and-white specific themes.
    

---

## 4. Image Viewer: Feh

`feh` is a very fast, lightweight, and minimalist image viewer, often used in tiling window managers. It's excellent for quickly viewing images and even setting wallpapers (though we use `hyprpaper`).

Bash

```
yay -S feh
```

_Explanation_: Installs the `feh` image viewer.

To open an image with `feh`:

Bash

```
feh /path/to/your/image.jpg
```

---

## 5. PDF Viewer: Zathura

Zathura is a highly customizable, keyboard-driven document viewer that is simple and efficient, perfectly fitting a minimalist workflow.

Bash

```
yay -S zathura zathura-pdf-mupdf
```

_Explanation_:

- `zathura`: The core PDF viewer.
    
- `zathura-pdf-mupdf`: The backend for rendering PDF files.
    

**Post-Installation (Zathura):**

- Zathura's configuration is typically in `~/.config/zathura/zathurarc`. You can modify its colors to ensure it aligns with your black-and-white theme. Look for `set default-bg` and `set default-fg` and other color definitions.
    

---

## 6. Video Player: MPV

MPV is a free, open-source, and highly customizable media player. It has a minimalist interface by default and is controlled primarily via keyboard shortcuts, making it ideal for a Hyprland setup.

Bash

```
yay -S mpv
```

_Explanation_: Installs the `mpv` media player.

To play a video with `mpv`:

Bash

```
mpv /path/to/your/video.mp4
```

---

## 7. Clipboard Manager: `wl-clipboard` & `cliphist`

On Wayland, a good clipboard manager is essential. `wl-clipboard` provides command-line utilities for interacting with the Wayland clipboard, and `cliphist` adds clipboard history.

Bash

```
yay -S wl-clipboard cliphist
```

_Explanation_:

- `wl-clipboard`: Provides `wl-copy` and `wl-paste` commands.
    
- `cliphist`: A Wayland clipboard manager that stores clipboard history.
    

**Integration with Rofi (via Dotfiles):** Your provided Rofi dotfiles (`~/.config/rofi/config.rasi` and keybindings in `~/.config/hypr/hyprland.conf`) should already include a binding to launch `cliphist` through Rofi. This allows you to paste from your clipboard history using a convenient popup.

---

## 8. Screenshot Tool: Grim, Slurp & Swappy

These are the standard Wayland-native screenshot tools that work seamlessly with Hyprland.

Bash

```
yay -S grim slurp swappy
```

_Explanation_:

- `grim`: Takes screenshots of Wayland surfaces.
    
- `slurp`: Allows you to select a region of the screen.
    
- `swappy`: A simple image editor to annotate or crop screenshots.
    

**Keybindings (via `~/.config/hypr/hyprland.conf`):** Your `hyprland.conf` should contain keybindings to use these tools. Common examples:

- **Full screenshot:** `bind = $mainMod, Print, exec, grim`
    
- **Region screenshot:** `bind = $mainMod SHIFT, S, exec, grim -g "$(slurp)" - | swappy -f -` _(Note: This sends the screenshot to `swappy` for editing immediately.)_
    
- **Screenshot to clipboard:** `bind = $mainMod ALT, S, exec, grim -g "$(slurp)" - | wl-copy`
    

**Test these bindings** after installation and reloading Hyprland.

---

## 9. Install Ghostty Terminal

Earlier, we installed `foot` as a basic terminal. Now, let's install `Ghostty`, which you chose for your setup, as it offers unique features and performance. Ghostty is typically installed by downloading its pre-built binary.

1. **Download Ghostty:** Visit the official Ghostty releases page on GitHub: [https://github.com/ghostty/ghostty/releases](https://www.google.com/search?q=https://github.com/ghostty/ghostty/releases) Download the latest Linux `x86_64` binary (e.g., `ghostty-linux-x86_64.tar.gz`).
    
2. **Extract and Install:**
    
    Bash
    
    ```
    cd ~/Downloads/ # Or wherever you downloaded the file
    tar -xzf ghostty-linux-x86_64.tar.gz
    sudo mv ghostty /usr/local/bin/ # Move the executable to your PATH
    ```
    
    _Explanation_:
    
    - `tar -xzf`: Extracts the gzipped tar archive.
        
    - `sudo mv ghostty /usr/local/bin/`: Moves the `ghostty` executable to `/usr/local/bin/`, which is typically in your system's PATH, making it executable from anywhere.
        
3. **Configure Ghostty (`~/.config/ghostty/config.toml`):** Ghostty uses a TOML format for its configuration. Your dotfiles might already include a basic one.
    
    Bash
    
    ```
    mkdir -p ~/.config/ghostty
    cp ~/HyprArch-BW-Journey/dotfiles/ghostty/config.toml ~/.config/ghostty/ # If you create this file in your dotfiles repo
    nano ~/.config/ghostty/config.toml
    ```
    
    _Key sections to look for_:
    
    - **`font`**: Set your preferred font and size.
        
    - **`colors`**: This is where you'll define your black-and-white color scheme for the terminal background, foreground, and ANSI colors (black, red, green, etc.). Ensure they are shades of gray, black, and white.
        
4. **Update Hyprland Keybinding:** Edit your `~/.config/hypr/hyprland.conf` and change the terminal keybinding from `foot` to `ghostty`:
    
    Ini, TOML
    
    ```
    # Example in hyprland.conf
    bind = $mainMod, Q, exec, ghostty
    ```
    
    Save and **reload Hyprland (`Mod + R`)**. Now, when you press `Mod + Q`, `ghostty` should launch!
    

---

## 10. Optional but Useful Applications

Consider installing these for an even more complete setup, ensuring they have dark mode or minimalist themes:

- **File Archiver (GUI):** `engrampa` (MATE-based, simple) or `xarchiver`.
    
- **Music Player:** `mpd` (Music Player Daemon) with a client like `ncmpcpp` (CLI) or `nwg-look` (for GTK theme application visually, though `lxappearance` handles most needs).
    
- **Volume Control:** `pavucontrol` (PulseAudio/Pipewire volume control GUI).
    
    Bash
    
    ```
    yay -S pavucontrol
    ```
    

---

## 11. Next Steps

You now have a functional and aesthetically consistent Hyprland desktop with your essential applications installed. You're almost at the finish line for your core setup! The final step involves general tips, tricks, and troubleshooting advice to keep your system running smoothly.

Proceed to the final stage of your initial journey: **[08-Tips-Tricks-Troubleshooting.md](https://www.google.com/search?q=08-Tips-Tricks-Troubleshooting.md)**.