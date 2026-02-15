# macos-toolkit

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform: macOS](https://img.shields.io/badge/Platform-macOS-lightgrey.svg)](https://github.com/lu-zhengda/macos-toolkit)
[![Claude Code Plugin](https://img.shields.io/badge/Claude_Code-Plugin-blueviolet.svg)](https://github.com/lu-zhengda/macos-toolkit)

A Claude Code plugin that gives Claude native macOS system management capabilities — network diagnostics, disk cleanup, port management, app updates, launch agents, system defaults, process monitoring, security auditing, and Bluetooth management.

**Ask Claude naturally:** "What's using port 3000?", "Clean up my Mac", "Check for app updates", "Why is my WiFi slow?", "What's eating my CPU?", "Run a security audit", "Check my AirPods battery" — and it runs the right tool automatically.

## Install

```bash
claude plugin marketplace add https://github.com/lu-zhengda/macos-toolkit
claude plugin add macos-toolkit@macos-toolkit
```

Then install the CLI tools:

```bash
brew tap lu-zhengda/tap
brew install macfig netwhiz whport lanchr macbroom updater pstop macdog bltctl
```

## Tools

### [whport](https://github.com/lu-zhengda/whport) — Port & Process Manager

Find what's listening on any port, get detailed process info, and kill processes by port.

```
$ whport list
PORT   PROTO  PID    PROCESS    USER    STATE
5000   TCP    644    ControlCe  user    LISTEN
7000   TCP    644    ControlCe  user    LISTEN
7265   TCP    76742  Raycast    user    LISTEN
26443  TCP    87971  OrbStack   user    LISTEN

$ whport info 5000
Port:        5000/TCP
State:       LISTEN
Process:     ControlCe (PID 644)
Command:     /System/Library/CoreServices/ControlCenter.app/Contents/MacOS/ControlCenter
User:        user
CPU:         0.0%
Memory:      116.0 MB (RSS)
Parent PID:  1
```

### [netwhiz](https://github.com/lu-zhengda/netwhiz) — Network Diagnostics

Full network overview, WiFi signal analysis, DNS management, speed tests, and LAN scanning.

```
$ netwhiz info
Network Overview
════════════════

  Interface:     en0
  Status:        Active
  IP Address:    192.168.1.42
  Subnet Mask:   255.255.252.0
  Router:        192.168.1.1
  DNS Servers:   (auto)

  Public IP:     203.0.113.1

$ netwhiz wifi
WiFi Information
════════════════

  SSID:          MyNetwork
  Channel:       52 (5GHz)
  RSSI:          -44 dBm (Excellent)
  Noise:         -89 dBm
  SNR:           45 dB
  Tx Rate:       864 Mbps
  Security:      WPA2 Personal
  Country:       US
```

### [macbroom](https://github.com/lu-zhengda/macbroom) — System Cleanup

Scan for reclaimable disk space across system caches, Xcode, Docker, node_modules, and more. Results are colored and sorted by size with risk breakdown, scan diff tracking, JSON output (`--json`), and config validation.

```
$ macbroom scan
Scanning...

System Junk (8.2 GB, 130 items)  +1.2 GB
Homebrew (2.1 GB, 117 items)  -200.0 MB
Browser Cache (1.8 GB, 3 items)  (new)
Node.js (453.0 MB, 1 items)  unchanged

Total reclaimable: 12.5 GB
Safe: 4.0 GB (32%)  Moderate: 8.0 GB (64%)  Risky: 500.0 KB (4%)
+2.8 GB since Feb 13

$ macbroom scan --system --json
$ macbroom config validate
```

### [updater](https://github.com/lu-zhengda/updater) — App Update Manager

Check for outdated apps, update individually or in bulk, pin versions, and rollback.

```
$ updater check
NAME                  CURRENT        LATEST         SOURCE     STATUS
GitHub Desktop        3.5.4          3.5.4          github     ok
Google Chrome         145.0.7632.76  145.0.7632.76  homebrew   ok
iTerm2                3.6.6          3.6.6          sparkle    ok
Code                  1.109.3        1.109.2        github     ok
Xcode                 26.2           26.2           app store  ok
```

### [macfig](https://github.com/lu-zhengda/macfig) — macOS Defaults Manager

Browse and change hidden macOS preferences with presets for common developer setups.

```
$ macfig list
Categories:

  🚢 Dock  (10 settings)
  📁 Finder  (9 settings)
  📷 Screenshots  (5 settings)
  💻 Keyboard  (6 settings)
  🤚 Trackpad  (3 settings)
  🎬 Animations  (2 settings)
  🔒 Privacy  (2 settings)
  🔧 Misc  (4 settings)

Use 'macfig list <category>' to view settings.

$ macfig list dock
🚢 Dock

  Auto-hide Dock                      0 -> recommended: true
    Automatically hide and show the Dock  (com.apple.dock autohide)
  Auto-hide delay                     0 -> recommended: 0
    Delay before Dock auto-hides (seconds)  (com.apple.dock autohide-delay)
  Show recent apps                    0 -> recommended: false
    Show recent applications in the Dock  (com.apple.dock show-recents)
```

### [lanchr](https://github.com/lu-zhengda/lanchr) — Launch Agent & Daemon Manager

List, diagnose, create, and manage launchd services without wrestling with plist XML.

```
$ lanchr doctor
DOCTOR REPORT
=============

CRITICAL (6)
  [!] com.apple.cvmsCompAgent_arm64_1: binary not found at /System/Library/...
      Suggestion: Remove or update the plist to point to a valid binary
  [!] com.apple.menuextra.battery.helper: binary not found at /System/Library/...
      Suggestion: Remove or update the plist to point to a valid binary
  [!] com.apple.knowledgeconstructiond: last exit status: -9
      Suggestion: Check logs for the service to diagnose the crash

WARNING (71)
  [~] com.apple.akd: duplicate label found in 2 plists
      Suggestion: Remove duplicate plists or use unique labels

Run 'lanchr list' to see all services.
```

### [pstop](https://github.com/lu-zhengda/pstop) — Process Explorer

Browse, search, and manage processes with a developer-friendly CLI.

```
$ pstop top --n 5
PID    NAME          USER           CPU%  MEM%  STATE  COMMAND
5647   claude        user           50.2  8.6   S+     claude
612    iTerm2        user           40.8  4.1   S      /Applications/iTerm.app/Contents/MacOS/iTerm2
382    WindowServer  _windowserver  39.5  1.7   Ss     /System/Library/PrivateFrameworks/...
21635  claude        user           27.0  14.3  S+     claude
71898  Magnet        user           14.1  0.7   S      /Applications/Magnet.app/Contents/MacOS/Magnet
```

### [macdog](https://github.com/lu-zhengda/macdog) — Security & Privacy Suite

Audit your security posture, manage firewall rules, review privacy permissions, and harden your system.

```
$ macdog audit
Security Grade: B (75/100)

CHECK                        STATUS
-----                        ------
System Integrity Protection  enabled
Firewall                     off
FileVault                    on
Gatekeeper                   enabled
Remote Login                 off
```

### [bltctl](https://github.com/lu-zhengda/bltctl) — Bluetooth Manager

Browse, connect, and manage Bluetooth devices with battery monitoring and diagnostics.

```
$ bltctl list
STATUS  NAME                  TYPE        ADDRESS            BATTERY
●       Headphones            Headphones  70:F9:4A:7A:8B:CA  -
○       AirPods               Headphones  98:DD:60:D2:4C:FF  -
○       AirPods Pro           Headphones  74:15:F5:4E:D0:50  [██████████] 100%
○       Beats Flex            Headphones  A8:91:3D:DE:91:C6  -
○       Beats Studio Buds     Headphones  F4:34:F0:96:DD:A0  [██████████] 100%
```

## Skills

Each tool is exposed as a skill that auto-triggers from natural language and is available as a slash command:

| Skill | Trigger examples | Slash command |
|-------|-----------------|---------------|
| **whport** | "what's on port 8080", "kill the process on port 3000" | `/macos-toolkit:whport` |
| **netwhiz** | "check my WiFi", "run a speed test", "change DNS" | `/macos-toolkit:netwhiz` |
| **macbroom** | "clean up my Mac", "free disk space", "find duplicates" | `/macos-toolkit:macbroom` |
| **updater** | "check for updates", "update Firefox", "find unused apps" | `/macos-toolkit:updater` |
| **macfig** | "show hidden files", "speed up the Dock", "apply dev preset" | `/macos-toolkit:macfig` |
| **lanchr** | "list launch agents", "create a scheduled task", "debug a daemon" | `/macos-toolkit:lanchr` |
| **pstop** | "what's eating CPU", "show process tree", "find node processes" | `/macos-toolkit:pstop` |
| **macdog** | "run security audit", "check firewall", "review privacy permissions" | `/macos-toolkit:macdog` |
| **bltctl** | "list Bluetooth devices", "check AirPods battery", "fix Bluetooth" | `/macos-toolkit:bltctl` |

## Prerequisites

- **macOS** (tested on Sonoma and Sequoia)
- **Claude Code** with plugin support
- **[Homebrew](https://github.com/lu-zhengda/homebrew-tap)** for installing the CLI tools

## License

[MIT](LICENSE)
