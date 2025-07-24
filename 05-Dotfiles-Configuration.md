
**`05-Dotfiles-Configuration.md`**: Setting up and understanding the configuration files for Hyprland, Waybar, Rofi, and more.

This is where you'll begin to personalize your Hyprland environment and truly make it your own, embracing the black-and-white aesthetic. We'll explore the "dotfiles" – the hidden configuration files that shape your Linux experience.

**Dedication to Learning:** Continue your commitment to **typing out every command and exploring every file yourself**. This hands-on approach is vital for understanding how your system is put together.

---

# `05 - Dotfiles Configuration`

"Dotfiles" are simply configuration files that typically reside in your home directory, often hidden (their names start with a dot, e.g., `.bashrc`, `.config/hypr/hyprland.conf`). They control almost every aspect of your desktop environment, from keybindings and aesthetics to application behavior.

In this step, you'll grab the pre-configured dotfiles from _this repository_ and place them in your system. These dotfiles are designed to give you a working black-and-white Hyprland setup out-of-the-box, which you can then modify and expand upon.

---

## 1. Clone the `HyprArch-BW-Journey` Repository

First, ensure you are in your home directory (`~` or `/home/<your_username>`) and clone this entire repository.

Bash

```
cd ~
git clone https://github.com/your-username/HyprArch-BW-Journey.git
```

_Explanation_:

- `cd ~`: Changes your current directory to your home directory.
    
- `git clone ...`: Downloads a copy of this entire repository from GitHub to a new directory named `HyprArch-BW-Journey` in your home folder.
    

---

## 2. Understand the Dotfiles Structure

Inside the cloned `HyprArch-BW-Journey` directory, you'll find a `dotfiles` subdirectory. This directory contains the configurations for various components:

```
~/HyprArch-BW-Journey/dotfiles/
├── hypr/
│   └── hyprland.conf             # Hyprland's main configuration
├── waybar/
│   ├── config                    # Waybar module configuration
│   └── style.css                 # Waybar styling for B&W theme
├── rofi/
│   ├── config.rasi               # Rofi's main configuration
│   └── themes/
│       └── bw-theme.rasi         # Rofi theme for B&W
├── foot/
│   └── foot.ini                  # Foot terminal configuration
└── hyprlock/
    └── hyprlock.conf             # Hyprlock configuration
```

Your system expects these configuration files to be in specific locations, usually within `~/.config/`. So, for example, Hyprland's config will be `~/.config/hypr/hyprland.conf`.

---

## 3. Apply the Dotfiles to Your System

We will copy these dotfiles to their correct locations. We'll use the `cp -r` command, which copies directories recursively.

Bash

```
# Create the .config directory if it doesn't exist (it usually does)
mkdir -p ~/.config

# Copy Hyprland dotfiles
cp -r ~/HyprArch-BW-Journey/dotfiles/hypr ~/.config/

# Copy Waybar dotfiles
cp -r ~/HyprArch-BW-Journey/dotfiles/waybar ~/.config/

# Copy Rofi dotfiles
cp -r ~/HyprArch-BW-Journey/dotfiles/rofi ~/.config/

# Copy Foot terminal dotfiles
cp -r ~/HyprArch-BW-Journey/dotfiles/foot ~/.config/

# Copy Hyprlock dotfiles
cp -r ~/HyprArch-BW-Journey/dotfiles/hyprlock ~/.config/
```

_Explanation_:

- `cp -r`: Copies directories and their contents recursively.
    
- `~/.config/`: The standard location for user-specific configuration files on Linux.
    

**Note on Symlinking (Advanced):** For more advanced users or those who want to easily update their dotfiles directly from Git, you might consider using symbolic links (`ln -s`) instead of copying. This allows you to manage your dotfiles with Git in your home directory and have live links to `~/.config/`. This is beyond the scope of this initial setup but is a common practice for dotfile management.

---

## 4. Explore Key Configuration Files

Now that the dotfiles are in place, let's take a closer look at some of the most important ones. Use `nano` or `vim` to open these files and read through them. Don't be afraid to experiment with values after you understand their purpose!

### A. Hyprland Configuration (`~/.config/hypr/hyprland.conf`)

This is the heart of your Hyprland setup. It controls almost everything from monitors to keybindings.

Bash

```
nano ~/.config/hypr/hyprland.conf
```

_Key Sections to Observe:_

- **`monitor = ...`**: Defines your monitor setup (resolution, refresh rate, scaling, position). You'll likely need to adjust this for your specific display(s).
    
    - Example: `monitor=DP-1,2560x1440@144,0x0,1.25` (DisplayPort-1, 2560x1440 at 144Hz, at 0,0 position, scaled by 1.25)
        
- **`exec-once = ...`**: Commands that run once when Hyprland starts (e.g., setting wallpaper, starting Waybar, setting environment variables).
    
    - **For NVIDIA users**: This is where you might set crucial environment variables like `env = WLR_NO_CC_BUFFER_ALLOCATOR,1` and `env = __GLX_VENDOR_LIBRARY_NAME,nvidia` etc.
        
- **`input { ... }`**: Configures your keyboard, mouse, touchpad. Look for sensitivity, natural scrolling, keyboard layout.
    
    - `kb_layout = us` (or `ph` for Philippines, or `gb` for Great Britain, etc.)
        
    - `kb_variant =` (leave empty unless you have a specific variant)
        
- **`general { ... }`**: General look and feel (gaps between windows, border size, animations).
    
- **`decoration { ... }`**: Window decoration settings like blur, shadows, rounded corners.
    
- **`dwindle { ... }`** / **`master { ... }`**: Settings for the tiling layouts.
    
- **`bind = ...`**: Defines all your keyboard shortcuts (keybindings). This is where you'll find shortcuts for launching terminals, moving windows, switching workspaces, etc.
    
    - Example: `bind = $mainMod, Q, exec, foot` (Mod key + Q launches `foot` terminal).
        
- **`windowrule = ...`**: Rules for specific windows (e.g., floating certain applications, setting specific sizes).
    
- **`source = ...`**: Allows you to split your config into multiple files (e.g., a separate file for all keybinds).
    

**Action**: Read through the comments in `hyprland.conf`. **Adjust the `monitor` line** to match your display's resolution and refresh rate if it's different from the default in the provided dotfile.

### B. Waybar Configuration (`~/.config/waybar/config` and `~/.config/waybar/style.css`)

Waybar is your highly customizable status bar.

Bash

```
nano ~/.config/waybar/config
nano ~/.config/waybar/style.css
```

- `config`: Defines which modules appear on your bar (e.g., clock, battery, network, workspaces, volume) and their order.
    
- `style.css`: Controls the visual appearance of Waybar and its modules using CSS. This is where the black-and-white theme magic happens! Experiment with colors (background, foreground, border) to fine-tune your aesthetic.
    

### C. Rofi Configuration (`~/.config/rofi/config.rasi` and `~/.config/rofi/themes/bw-theme.rasi`)

Rofi is a versatile application launcher.

Bash

```
nano ~/.config/rofi/config.rasi
nano ~/.config/rofi/themes/bw-theme.rasi
```

- `config.rasi`: Main settings for Rofi.
    
- `bw-theme.rasi`: This file defines the black-and-white theme for Rofi. Explore the color definitions and adjust them if you wish.
    

### D. Foot Terminal Configuration (`~/.config/foot/foot.ini`)

The `foot` terminal's look and behavior.

Bash

```
nano ~/.config/foot/foot.ini
```

- **Font**: Adjust the `font` line to your preferred font and size.
    
- **Colors**: The `colors` section will be crucial for your black-and-white terminal scheme.
    

### E. Hyprlock Configuration (`~/.config/hyprlock/hyprlock.conf`)

Hyprlock is your screen locker.

Bash

```
nano ~/.config/hyprlock/hyprlock.conf
```

- **Background**: You can set a background image or color. For a black-and-white theme, a solid black or a grayscale image is often effective.
    
- **Text/Input Fields**: Customize the appearance of the unlock prompt.
    

---

## 5. Reload Hyprland to Apply Changes

After modifying any of your dotfiles, especially `hyprland.conf`, you need to reload Hyprland for the changes to take effect.

The default keybinding in the provided `hyprland.conf` to reload Hyprland is typically **`Mod + R`**.

If you're unsure or this doesn't work, you can always:

1. Log out of Hyprland (`Mod + Shift + E` by default).
    
2. Log back in through `ly`.
    

This will ensure a fresh Hyprland session with your new configurations.

---

## 6. Next Steps

Your Hyprland environment is now tailored with the dotfiles, giving it the initial black-and-white aesthetic and functionality. You've got the core look and feel established.

Next, we'll install additional applications and fine-tune them to ensure they blend seamlessly with your monochrome theme and enhance your daily workflow.

Proceed to the next stage of your journey: **[06-Theming-Black-White.md](https://www.google.com/search?q=06-Theming-Black-White.md)**.