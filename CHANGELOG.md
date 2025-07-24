# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2025-07-25

### Added
- **Complete Arch Linux + Hyprland Setup Guide**: Comprehensive step-by-step documentation for building a minimalist black and white Linux desktop environment
- **Arch Installation Guide** (`01-Arch-Installation.md`): Detailed instructions for installing Arch Linux using `archinstall` script with explanations for each step
- **Post-Installation Setup** (`02-Post-Install-Setup.md`): Initial system configuration, user setup, and essential package installation
- **Hyprland Installation Guide** (`03-Hyprland-Installation.md`): Complete setup for Hyprland Wayland compositor with GPU driver installation for AMD, Intel, and NVIDIA
- **Display Manager Configuration** (`04-Display-Manager-Setup.md`): Setup and configuration of `ly` display manager
- **Dotfiles Configuration** (`05-Dotfiles-Configuration.md`): Comprehensive guide for setting up Hyprland, Waybar, Rofi, and other component configurations
- **Black & White Theming Guide** (`06-Theming-Black-White.md`): Detailed theming instructions for achieving consistent monochrome aesthetics across GTK, Qt, and terminal applications
- **Essential Applications Guide** (`07-Essential-Applications.md`): Installation and configuration of daily-use applications including file managers, browsers, text editors, and media players
- **Tips, Tricks & Troubleshooting** (`08-Tips-Tricks-Troubleshooting.md`): System maintenance, optimization tips, and solutions for common issues
- **Project Documentation**:
  - `README.md`: Comprehensive project overview with feature list and journey map
  - `CONTRIBUTING.md`: Guidelines for contributing to the project
  - `LICENSE`: MIT License for the project
- **Astro-based Documentation Site**: Set up with Starlight theme for hosting the guides
- **Package Configuration**: Node.js project setup with Astro dependencies

### Features
- **Educational Approach**: Each guide emphasizes learning by doing with detailed explanations for every command
- **Minimalist Black & White Aesthetic**: Consistent monochrome theming across all desktop components
- **Comprehensive Component Coverage**: 
  - Hyprland Wayland compositor
  - Waybar status bar
  - Rofi application launcher
  - Foot terminal emulator
  - Hyprlock screen locker
  - ly display manager
- **GPU Driver Support**: Complete setup instructions for AMD, Intel, and NVIDIA graphics
- **Application Integration**: Guides for installing and configuring essential daily-use applications
- **System Maintenance**: Regular update procedures and optimization tips
- **Troubleshooting**: Common issues and their solutions

### Technical Details
- **Target System**: Arch Linux with Hyprland Wayland compositor
- **Theme**: Consistent black and white/minimalist aesthetic
- **Package Management**: Uses `pacman` and `yay` for official and AUR packages
- **Documentation Framework**: Astro with Starlight theme for web-based documentation
- **Version Control**: Git-based with contribution guidelines

### Documentation Structure
- 8 comprehensive setup guides covering the complete journey from Arch installation to daily use
- Detailed explanations for every command and configuration choice
- Emphasis on understanding the "why" behind each step
- Organized progression from basic system setup to advanced customization
