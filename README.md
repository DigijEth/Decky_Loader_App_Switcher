I will upload the individual files shortly, the .zip is just a holder

# App Switcher - Decky Loader Plugin

A Decky Loader plugin for SteamOS that provides Alt+Tab-like window switching functionality.

## Features

- **Next Window**: Switch to the next application (simulates Alt+Tab)
- **Previous Window**: Switch to the previous application (simulates Alt+Shift+Tab)
- **Window List**: View and directly switch to any open window

## Requirements

The following tools must be available on your SteamOS:

- `xdotool` - For simulating keyboard input
- `wmctrl` - For window management (optional, for window list feature)

### Installing Dependencies

On SteamOS, enter Desktop Mode and run:

```bash
sudo steamos-readonly disable
sudo pacman -S xdotool wmctrl
sudo steamos-readonly enable
```

## Installation

1. Install Decky Loader on your Steam Deck
2. Copy this plugin folder to `~/homebrew/plugins/`
3. Restart Decky Loader or reboot

## Building

```bash
pnpm install
pnpm run build
```

## Usage

1. Open the Quick Access Menu (... button)
2. Navigate to the Decky Loader section
3. Select "App Switcher"
4. Use the buttons to switch between windows
