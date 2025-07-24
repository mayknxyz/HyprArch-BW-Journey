06-Theming-Black-White.md: Achieving a Consistent Black and White Aesthetic.

This guide is all about perfecting the monochrome look across your entire Hyprland desktop. It's not just about wallpaper; it's about ensuring every application, every menu, and every element adheres to your minimalist vision.

Embrace the Art of Theming:
This step requires attention to detail. Experiment and carefully type out each command and configuration change to see its effect. The consistency of your black-and-white theme is built piece by piece.

06 - Theming: Achieving a Consistent Black and White Aesthetic
A truly minimalist and striking Hyprland setup isn't just about functionality; it's about a cohesive visual experience. Our goal is a consistent black-and-white (monochrome or grayscale) theme across all aspects of your desktop. This involves configuring GTK applications, Qt applications, terminal colors, cursors, and more.

1. Wallpaper: The Foundation of Your Aesthetic
Your wallpaper sets the tone. For a black-and-white theme, a simple, abstract, or even solid black/white background is often most effective.

We'll use hyprpaper, Hyprland's native wallpaper utility, for best performance and integration.

Install hyprpaper:

Bash

yay -S hyprpaper
Prepare your Wallpaper:

Find or create a black and white image. High-resolution images work best.

Place your chosen wallpaper file in a dedicated directory, e.g., ~/Pictures/Wallpapers/.

Bash

mkdir -p ~/Pictures/Wallpapers
# Example: copy your_bw_wallpaper.jpg to this directory
cp /path/to/your_bw_wallpaper.jpg ~/Pictures/Wallpapers/
Configure hyprpaper:
Edit your Hyprland configuration file to load the wallpaper. This should already be present in your hyprland.conf from the previous step, but let's confirm.

Bash

nano ~/.config/hypr/hyprland.conf
Find the exec-once section. You should have lines related to hyprpaper. Ensure they look something like this, replacing your_bw_wallpaper.jpg with your actual file name:

Ini, TOML

# hyprpaper config for wallpaper
exec-once = hyprpaper
preload = ~/Pictures/Wallpapers/your_bw_wallpaper.jpg
wallpaper = <your_monitor_name>,~/Pictures/Wallpapers/your_bw_wallpaper.jpg
# Example for a single monitor named 'DP-1':
# wallpaper = DP-1,~/Pictures/Wallpapers/your_bw_wallpaper.jpg
Explanation:

exec-once = hyprpaper: Starts the hyprpaper daemon.

preload = ...: Loads the image into memory.

wallpaper = <monitor_name>,...: Sets the wallpaper for a specific monitor. To find your monitor name, type hyprctl monitors in a terminal when Hyprland is running. It will show names like DP-1, HDMI-A-1, eDP-1, etc. Adjust this line to your monitor.

Save the file (Ctrl+O, Enter) and exit (Ctrl+X). Reload Hyprland (Mod + R) to apply.

2. GTK Themes (For GNOME/GTK Applications)
Many applications (like Firefox, Thunar, GEdit, GTK-based settings) use the GTK toolkit. To make them black and white, you need a GTK theme.

Install adw-gtk3 (GTK3 theme) & lxappearance:
adw-gtk3 offers a nice dark variant which can be customized further. lxappearance is a GUI tool to easily change GTK themes.

Bash

yay -S adw-gtk3 lxappearance
Set GTK Theme with lxappearance:
Launch lxappearance:

Bash

lxappearance
In the "Widget" tab, select a dark theme like adw-gtk3-dark or Adwaita-dark.

In the "Icon Theme" tab, choose an icon theme that blends well, such as Adwaita (default, often works) or Papirus-Dark.

In the "Cursor Theme" tab, select Adwaita or DMZ-Black. (We'll install a specific B&W cursor theme later).

Click "Apply" or "Save" (usually changes apply immediately).

Manual GTK Configuration (Advanced / Fine-Tuning):
lxappearance writes its settings to ~/.config/gtk-3.0/settings.ini and ~/.config/gtk-4.0/settings.ini. You can manually tweak these or create a ~/.config/gtk-3.0/gtk.css (and gtk-4.0/gtk.css) for more granular black-and-white styling if you know CSS.

For example, to force dark mode and a specific theme:

Ini, TOML

# ~/.config/gtk-3.0/settings.ini
[Settings]
gtk-application-prefer-dark-theme=true
gtk-theme-name=Adwaita-dark
gtk-icon-theme-name=Adwaita
gtk-cursor-theme-name=Adwaita
3. Qt Themes (For KDE/Qt Applications)
Applications like VLC, LXQt components, or some KDE utilities use the Qt toolkit. They don't directly use GTK themes but can be made to follow similar styling.

Install qt5ct and qt6ct:
These tools allow you to configure Qt 5 and Qt 6 applications' appearance.

Bash

yay -S qt5ct qt6ct
Set Environment Variables:
For Qt applications to use qt5ct/qt6ct, you need to set an environment variable. Add these to your ~/.config/hypr/hyprland.conf (in an exec-once block or env section), or in your ~/.profile / ~/.zprofile:

Ini, TOML

# Add to ~/.config/hypr/hyprland.conf (or similar startup script)
exec-once = dbus-update-activation-environment --systemd --all
env = QT_QPA_PLATFORMTHEME,qt5ct
Save and reload Hyprland (Mod + R).

Configure Qt Themes:
Launch qt5ct and qt6ct from your terminal:

Bash

qt5ct
qt6ct
In both applications, under "Style", select a dark style like Fusion or Adwaita-Dark (if available and compatible through qt5ct-gtk2 or similar bridge, which you can install if you want tighter integration). Fusion is a safe bet for a dark look.

Under "Palette", choose "Dark".

Under "Icon Theme", select an icon theme (e.g., Adwaita or Papirus-Dark).

Click "Apply" and "OK".

4. Terminal Theming (Foot and Ghostty)
Your terminal is a central part of a Hyprland setup.

A. Foot Terminal (~/.config/foot/foot.ini)
You already copied the foot.ini in the previous step. Open it:

Bash

nano ~/.config/foot/foot.ini
Fonts: Ensure your chosen monospace font looks good.

Colors: The [colors] section is key. Adjust the background, foreground, regular, and bright colors to be shades of black and white/gray. For a pure black-and-white, stick to #000000 for black, #FFFFFF for white, and shades of gray like #333333, #AAAAAA.

B. Ghostty Terminal (Preparation)
Since Ghostty is installed differently (usually binary download or build), we'll prepare for its config here. You can find example configs on its GitHub. The principle will be similar to foot's colors section. We'll install Ghostty in the next application section.

5. Cursor Theme
A unified cursor theme contributes significantly to the overall aesthetic.

Install a Black/White Cursor Theme (e.g., Capitaine Cursors):

Bash

yay -S capitaine-cursors
Apply Cursor Theme:

Use lxappearance as described in the GTK section (Section 2) and select capitaine-cursors from the "Cursor Theme" tab.

Manual Method: You can also set it in ~/.icons/default/index.theme:

Ini, TOML

# ~/.icons/default/index.theme
[Icon Theme]
Inherits=capitaine-cursors
And in GTK settings files (~/.config/gtk-3.0/settings.ini and ~/.config/gtk-4.0/settings.ini), ensure gtk-cursor-theme-name=capitaine-cursors.

Reload Hyprland (Mod + R) to see the cursor changes.

6. Icon Theme
While Adwaita or Papirus-Dark work, for a truly monochromatic setup, you might want to explore icon themes designed for that.

Explore Monochromatic Icon Themes (AUR):
Search the AUR for themes like "candy-icons-black-white" or "numix-circle-black-white".
Example (if available and suits your taste):

Bash

yay -S candy-icons-black-white-git
Apply Icon Theme:
Use lxappearance (GTK section) and qt5ct/qt6ct (Qt section) to select your chosen black and white icon theme.

7. Font Consistency (Review)
We installed essential fonts in 02-Post-Install-Setup.md. Now, ensure they are consistently applied where appropriate:

Hyprland: Set fonts in ~/.config/hypr/hyprland.conf for notifications, etc.

Waybar: Set fonts in ~/.config/waybar/style.css.

Rofi: Set fonts in ~/.config/rofi/config.rasi or its theme file.

Terminals: Set fonts in ~/.config/foot/foot.ini.

Choosing simple, clean sans-serif fonts for most text and a good monospace font for your terminal will enhance the minimalist look.

8. Verification and Iteration
After making changes, open various applications (a file manager, a text editor, a browser, your terminal) and observe their appearance.

Are GTK apps dark?

Are Qt apps dark?

Are your terminal colors consistently black and white/gray?

Does the cursor blend in?

Are icons consistent?

Theming is an iterative process. Feel free to tweak values in your .config files and reload Hyprland until you achieve your perfect black-and-white setup.

9. Next Steps
Your Hyprland desktop should now look consistently black and white, reflecting your minimalist vision. The aesthetic foundation is solid. Now, let's install the essential applications that will power your daily workflow, ensuring they also integrate well with your new theme.

Proceed to the next stage of your journey: 07-Essential-Applications.md.