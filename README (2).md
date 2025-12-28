# ROG Ally Control

Hardware integration tool for ASUS ROG Ally running SteamOS/Linux. Provides:

- **Custom button mapping** for paddle buttons (M1/M2) and ROG button
- **LED control** for joystick ring RGB lights
- **Profile system** for saving/loading configurations

## Requirements

- ASUS ROG Ally or ROG Ally X
- SteamOS, Bazzite, or any Linux distribution
- Python 3.10+
- `ydotool` (for window switching actions)

## Installation

```bash
sudo ./install.sh
```

This will:
1. Install the control script to `/opt/ally-control/`
2. Set up udev rules for device access
3. Install the systemd service
4. Create the `ally-control` CLI command

## Quick Start

### Start the daemon

```bash
# Start now
sudo systemctl start ally-control

# Enable at boot
sudo systemctl enable ally-control
```

### LED Control

```bash
# Static colors
ally-control led static ff0000          # Red
ally-control led static 00ff00          # Green  
ally-control led static 0000ff          # Blue
ally-control led static ff00ff          # Purple

# Effects
ally-control led breathing 00ffff       # Cyan breathing
ally-control led cycle                  # Color cycle
ally-control led rainbow                # Rainbow spiral

# Brightness (0-3)
ally-control led brightness 3           # Max brightness
ally-control led brightness 0           # Dim

# Turn off
ally-control led off

# Control individual zones
ally-control led static ff0000 --zone left   # Left stick only
ally-control led static 0000ff --zone right  # Right stick only
```

### Button Mapping

#### View current mappings
```bash
ally-control button show
```

#### Available actions
```bash
ally-control button list
```

Actions:
- `none` - No action
- `switch_window` - Alt+Tab (next window)
- `switch_window_prev` - Shift+Alt+Tab (previous window)
- `minimize_window` - Minimize current window
- `close_window` - Close current window (Alt+F4)
- `screenshot` - Take screenshot
- `steam_overlay` - Open Steam overlay
- `steam_qam` - Open Steam Quick Access Menu

#### Set button actions
```bash
# Map paddles to window switching
ally-control button set m1 switch_window
ally-control button set m2 switch_window_prev

# Map ROG button
ally-control button set rog_short steam_qam
ally-control button set rog_long steam_overlay

# Custom command
ally-control button set m1 "cmd:firefox"
ally-control button set m2 "cmd:steam steam://open/bigpicture"
```

### Profiles

Save and load complete configurations:

```bash
# Save current settings
ally-control profile save gaming

# Load a profile
ally-control profile load gaming

# List profiles
ally-control profile list
```

## Configuration

Configuration is stored in `~/.config/ally-control/config.json`

Example:
```json
{
  "active_profile": "default",
  "led": {
    "mode": "static",
    "color": [255, 0, 0],
    "speed": 2,
    "brightness": 2,
    "zone": "both"
  },
  "buttons": {
    "m1": "switch_window",
    "m2": "switch_window_prev",
    "rog_short": "steam_qam",
    "rog_long": "steam_overlay"
  }
}
```

## Button Reference

| Button | Key Code | Default Action |
|--------|----------|----------------|
| M1 (Left paddle) | F15 | switch_window |
| M2 (Right paddle) | F16 | switch_window_prev |
| ROG short press | - | steam_qam |
| ROG long press | F17 | steam_overlay |

## Troubleshooting

### Check device status
```bash
ally-control status
```

### Permission issues
```bash
# Reload udev rules
sudo udevadm control --reload-rules
sudo udevadm trigger

# Or manually set permissions
sudo chmod 666 /dev/hidraw*
```

### Daemon not starting
```bash
# Check logs
sudo journalctl -u ally-control -f
```

### LEDs not responding
The ROG Ally requires kernel support from `hid-asus`. Make sure you're running:
- SteamOS 3.5+
- Bazzite
- A kernel with ROG Ally patches (6.5+)

### Button mapping not working
1. Ensure `ydotool` is installed and running
2. Check that the daemon is running: `systemctl status ally-control`
3. Verify the N-Key device is detected: `ally-control status`

## Uninstall

```bash
sudo ./uninstall.sh
```

## Technical Details

### Hardware Access
- LEDs controlled via HID feature reports on `/dev/hidraw*`
- Vendor ID: 0x0B05 (ASUS)
- Product IDs: 0x1ABE (Ally), 0x1B4C (Ally X)

### LED Commands
- Report ID: 0x5D
- Command byte: 0xB3 (set mode)
- Zone byte: 0x01 (left), 0x02 (right), 0xFF (both)

### Input Events
- N-Key device emits standard keyboard events
- M1/M2 mapped to F15/F16 in kernel hid-asus driver

## License

MIT License

## Credits

- [hhd-dev/hhd](https://github.com/hhd-dev/hhd) - Handheld Daemon (reference implementation)
- [asus-linux.org](https://asus-linux.org) - ASUS Linux kernel driver development
- [g-helper](https://github.com/seerge/g-helper) - Windows reference for LED commands
