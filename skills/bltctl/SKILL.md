---
name: bltctl
description: This skill should be used when the user asks to "list Bluetooth devices", "connect Bluetooth", "disconnect Bluetooth", "check AirPods battery", "pair a device", "unpair a device", "fix Bluetooth issues", "reset Bluetooth", "turn Bluetooth on or off", "scan for devices", "watch battery level", "Bluetooth history", "low battery alert", or mentions bltctl, Bluetooth management, wireless devices, Bluetooth diagnostics, or battery monitoring.
version: 1.1.0
allowed-tools: Bash(bltctl:*)
---

# bltctl — Bluetooth Manager

List paired Bluetooth devices:

!`bltctl list 2>&1 || echo "bltctl not installed — brew install lu-zhengda/tap/bltctl"`

Analyze the output above. Show device names, types, connection status, and battery levels where available. Offer to connect/disconnect devices or check battery with `bltctl battery`.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `bltctl list` | List all paired devices | `bltctl list` |
| `bltctl scan` | Scan for devices (alias for list) | `bltctl scan` |
| `bltctl connect <device>` | Connect to a paired device | `bltctl connect "AirPods Pro"` |
| `bltctl disconnect <device>` | Disconnect a device | `bltctl disconnect "AirPods Pro"` |
| `bltctl battery` | Show battery levels for connected devices | `bltctl battery` |
| `bltctl battery --watch` | Monitor battery with low-level alerts | `bltctl battery --watch --low 20` |
| `bltctl info <device>` | Detailed device info | `bltctl info "Magic Keyboard"` |
| `bltctl remove <device>` | Unpair a device | `bltctl remove "Old Headphones"` |
| `bltctl power on` | Turn Bluetooth on | `bltctl power on` |
| `bltctl power off` | Turn Bluetooth off | `bltctl power off` |
| `bltctl reset` | Reset Bluetooth module (requires sudo) | `bltctl reset` |
| `bltctl diagnose` | Run Bluetooth diagnostics | `bltctl diagnose` |
| `bltctl history` | Show connect/disconnect event log | `bltctl history` |

## Battery Monitoring

Watch device battery levels and alert when they drop below a threshold:

```bash
# Alert when any device drops below 20%
bltctl battery --watch --low 20

# Useful for monitoring AirPods during long calls
bltctl battery --watch --low 10
```

## Connection History

View connect/disconnect event timeline:

```bash
bltctl history
```

Shows timestamped events for all paired devices — useful for debugging intermittent connection issues.

## Dependencies

- **blueutil** (optional): Required for connect, disconnect, and remove operations. Install with `brew install blueutil`.
- Power and reset commands require **sudo**.

## Safety Guidelines

- **Diagnose before reset**: Run `bltctl diagnose` to understand the issue before resetting
- **Reset is disruptive**: Resets the Bluetooth module — all devices will briefly disconnect
- **Power off warning**: Turning Bluetooth off disconnects all devices immediately

## TUI Mode

Launch `bltctl` without arguments for an interactive Bluetooth device dashboard.
