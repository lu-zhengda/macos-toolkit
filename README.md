# macos-toolkit

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform: macOS](https://img.shields.io/badge/Platform-macOS-lightgrey.svg)](https://github.com/lu-zhengda/macos-toolkit)
[![Claude Code Plugin](https://img.shields.io/badge/Claude_Code-Plugin-blueviolet.svg)](https://github.com/lu-zhengda/macos-toolkit)

A Claude Code plugin that gives Claude native macOS system management capabilities — network diagnostics, disk cleanup, app updates, launch agents, system defaults, process monitoring, security auditing, environment control, system health checks, and email.

**Ask Claude naturally:** "Run a health check", "Clean up my Mac", "Check for app updates", "Why is my WiFi slow?", "What's eating my CPU?", "Run a security audit", "Set up for a meeting", "Check my email" — and it runs the right tool automatically.

## Install

```bash
claude plugin marketplace add https://github.com/lu-zhengda/macos-toolkit
claude plugin add macos-toolkit@macos-toolkit
```

Then install the CLI tools:

```bash
brew tap lu-zhengda/tap
brew install macfig netwhiz lanchr macbroom updater pstop macdog macctl termail machealth
```

## Tools

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

### [macctl](https://github.com/lu-zhengda/macctl) — Environment Controller

Manage power, display, audio, focus modes, and apply compound presets from the CLI.

```
$ macctl power status
Battery:       100%
State:         on AC power
Time:          fully charged
Cycles:        351
Temperature:   30.1 C
Capacity:      5059 / 5209 mAh

$ macctl preset
NAME           DESCRIPTION
deep-work      Focus on, display brightness 50%, audio mute
meeting        Focus on (allow calls), audio unmute
present        Focus on, display brightness 100%
chill          Focus off, Night Shift on, display brightness 40%, audio volume 30%
battery-saver  Display brightness 30%, show power hogs
```

### [termail](https://github.com/lu-zhengda/termail) — Terminal Email Client

Read, compose, reply, search, and manage Gmail from the command line with full-text search and multi-account support.

```
$ termail list --limit 3
UNREAD  FROM          SUBJECT                                     DATE          MSGS  THREAD_ID
*       GitHub        [GitHub] A personal access token has been…   Feb 14, 2026  4     19c5d4f2ea3c4478
        Citi Alerts   Your daily alerts summary                   Feb 15, 2026  1     19c5ecc5e553f0b6
        Airmart Team  Your order has been completed                Feb 15, 2026  1     19c5f633501ea8e9

$ termail search "quarterly report"
FROM     SUBJECT                    DATE          ID
Finance  Q4 Quarterly Report 2025   Jan 10, 2026  19c12345abcdef
```

### [machealth](https://github.com/lu-zhengda/machealth) — System Health Checker

Unified health assessment across 8 subsystems (CPU, memory, thermal, disk, battery, iCloud, network, Time Machine) with a single health score and JSON output for AI agent automation.

```
$ machealth --human
System Health: 92/100 (green)

  [OK] CPU         load 1.2 / 2.4 / 1.8
  [OK] Memory      pressure: normal, swap: 0 MB
  [OK] Thermal     speed limit: 100%, no throttling
  [OK] Disk        184 GB available (45% used)
  [OK] Battery     100%, on AC power, healthy
  [OK] iCloud      synced
  [OK] Network     reachable via en0
  [OK] Time Machine last backup: 2h ago

$ machealth diagnose --human
```

## Skills

Each tool is exposed as a skill that auto-triggers from natural language and is available as a slash command:

| Skill | Trigger examples | Slash command |
|-------|-----------------|---------------|
| **netwhiz** | "check my WiFi", "run a speed test", "change DNS" | `/macos-toolkit:netwhiz` |
| **macbroom** | "clean up my Mac", "free disk space", "find duplicates" | `/macos-toolkit:macbroom` |
| **updater** | "check for updates", "update Firefox", "find unused apps" | `/macos-toolkit:updater` |
| **macfig** | "show hidden files", "speed up the Dock", "apply dev preset" | `/macos-toolkit:macfig` |
| **lanchr** | "list launch agents", "create a scheduled task", "debug a daemon" | `/macos-toolkit:lanchr` |
| **pstop** | "what's eating CPU", "show process tree", "find node processes" | `/macos-toolkit:pstop` |
| **macdog** | "run security audit", "check firewall", "review privacy permissions" | `/macos-toolkit:macdog` |
| **macctl** | "check battery", "set brightness", "apply deep-work preset", "mute audio" | `/macos-toolkit:macctl` |
| **termail** | "check my email", "send an email", "search emails", "reply to email" | `/macos-toolkit:termail` |
| **machealth** | "run a health check", "is my Mac healthy", "check system health", "diagnose issues" | `/macos-toolkit:machealth` |

## Prerequisites

- **macOS** (tested on Sonoma and Sequoia)
- **Claude Code** with plugin support
- **[Homebrew](https://github.com/lu-zhengda/homebrew-tap)** for installing the CLI tools

## License

[MIT](LICENSE)
