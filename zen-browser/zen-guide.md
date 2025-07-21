# Zen Browser: Complete Installation & Troubleshooting Guide for Ubuntu

This README will help you install Zen Browser on Ubuntu, set it up with full app integration, and resolve common problems encountered. Targeted at tech learners and power users interested in innovative browsers.

## Table of Contents

- [Installation Methods](#installation-methods)
  - [Flatpak (Recommended)](#flatpak-recommended)
  - [AppImage (Portable/Test Only)](#appimage-portabletest-only)
  - [Manual Extraction (Advanced)](#manual-extraction-advanced)
- [Making Zen Browser a Native App](#making-zen-browser-a-native-app)
- [Creating and Managing Profiles](#creating-and-managing-profiles)
- [Troubleshooting: Desktop Entry & Menu Visibility](#troubleshooting-desktop-entry--menu-visibility)
- [Troubleshooting: Copying the Current URL](#troubleshooting-copying-the-current-url)
- [Summary Table](#summary-table)

## Installation Methods

### Flatpak (Recommended)

This is the easiest method for full-featured, up-to-date app integration.

1. **Install Flatpak**
   ```bash
   sudo apt update
   sudo apt install flatpak
   sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
   ```

2. **Install Zen Browser via Flatpak**
   ```bash
   flatpak install flathub app.zen_browser.zen
   ```
   *(If this doesn't work, try `flatpak install io.github.zen_browser.zen`.)*

3. **Launch Zen**
   - From the app menu, search for “Zen”.
   - Or in a terminal:
     ```bash
     flatpak run app.zen_browser.zen
     ```

### AppImage (Portable/Test Only)

1. **Download the `.AppImage`**  
   From the [official Zen Browser releases].

2. **Make the AppImage Executable**
   ```bash
   chmod +x /path/to/ZenBrowser.AppImage
   ```

3. **Run**
   ```bash
   ./ZenBrowser.AppImage
   ```
   *Note: Will not show in your app menu unless you manually create a desktop entry.*

### Manual Extraction (Advanced)

1. **Extract the tar archive**
   ```bash
   sudo tar xvjf zen-linux.tar.bz2 -C /opt/
   ```

2. **Symlink/Add to PATH** as needed.

## Making Zen Browser a Native App

If Zen does not appear in your app list, manually add a launcher:

1. **Check for Flatpak Desktop Entry**
   ```bash
   ls /var/lib/flatpak/exports/share/applications/ | grep zen
   ```
   Should see: `app.zen_browser.zen.desktop`

2. **If it doesn’t appear in your menu, copy it to your user directory**
   ```bash
   cp /var/lib/flatpak/exports/share/applications/app.zen_browser.zen.desktop ~/.local/share/applications/
   update-desktop-database ~/.local/share/applications
   ```

3. **Refresh the application list**
   - Log out and back in, or reboot if necessary.

4. **Pin Zen to Dock**
   - Search for Zen in the application menu, launch it, right-click its icon in the Dock, and select "Add to Favorites".

*You do NOT need to set the executable or icon path manually for Flatpak — those are included in the official desktop entry.*

## Creating and Managing Profiles

Zen allows multiple user profiles similar to Chrome/Firefox:

- Enable via `about:config` by setting `browser.profiles.enabled` to `true` (if needed).
- Open Profile Manager (via menu or `about:profiles` URL).
- Create, delete, or switch profiles for separation of work/personal/testing contexts.

## Troubleshooting: Desktop Entry & Menu Visibility

If Zen doesn’t appear in your menu:

- Confirm presence of system desktop entry:
  ```bash
  ls /var/lib/flatpak/exports/share/applications/ | grep zen
  ```
- Refresh desktop database:
  ```bash
  sudo update-desktop-database /var/lib/flatpak/exports/share/applications/
  ```
- If still missing, copy to user applications:
  ```bash
  cp /var/lib/flatpak/exports/share/applications/app.zen_browser.zen.desktop ~/.local/share/applications/
  update-desktop-database ~/.local/share/applications
  ```

## Troubleshooting: Copying the Current URL

If you cannot copy the address bar URL (context menu disabled due to an extension or recent update), try:

- Press `Ctrl + Shift + C` — instantly copies the current tab’s URL to clipboard.
- Or, `Ctrl + L` to focus address bar, then `Ctrl + C`.
- Disable any suspicious extensions temporarily if shortcuts do not work.
- Install an extension that adds a “Copy Tab URL” toolbar button as a workaround.

## Summary Table

| Task                                   | Flatpak Method                           | AppImage/Manual                     |
|-----------------------------------------|------------------------------------------|-------------------------------------|
| Install                                | `flatpak install ...`                    | Download and run .AppImage or extract|
| App Menu Entry                         | Automatic (or copy .desktop if missing)  | Manual desktop entry required        |
| Pin to Dock/Workspace                   | Launch, right-click Dock icon, Pin       | Same after entry created             |
| Profiles Support                       | Yes                                      | Yes                                  |
| Copy Current URL                        | `Ctrl + Shift + C`, `Ctrl + L` + `Ctrl + C` | Same                                 |
| Update Process                         | Auto with Flatpak                        | Manual download                     |

## Appendix: Common Issues & Solutions

- **App not in launcher:** Ensure Flatpak desktop entry, refresh desktop database, copy to user directory if required.
- **Failed to copy URL:** Use keyboard shortcuts, check for extension conflicts, try a profile with extensions disabled.
- **No icon visible:** Do not set Icon path manually with Flatpak; it's set up in the official desktop entry.

**This guide is designed to help fellow Linux tech enthusiasts and students quickly resolve common Zen Browser installation/usage hurdles on Ubuntu, including application integration and UI behaviors.**

For questions or improvements, open an issue or PR!
