# macos-toolkit

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform: macOS](https://img.shields.io/badge/Platform-macOS-lightgrey.svg)](https://github.com/lu-zhengda/macos-toolkit)
[![Claude Code Plugin](https://img.shields.io/badge/Claude_Code-Plugin-blueviolet.svg)](https://github.com/lu-zhengda/macos-toolkit)

A Claude Code plugin that gives Claude native macOS system management capabilities — network diagnostics, disk cleanup, port management, app updates, launch agents, and system defaults.

**Ask Claude naturally:** "What's using port 3000?", "Clean up my Mac", "Check for app updates", "Why is my WiFi slow?" — and it runs the right tool automatically.

## Install

```bash
claude plugin marketplace add https://github.com/lu-zhengda/macos-toolkit
claude plugin add macos-toolkit@macos-toolkit
```

Then install the CLI tools:

```bash
brew tap lu-zhengda/tap
brew install macfig netwhiz whport lanchr macbroom updater
```

## CLI Tools

### whport — Port & Process Manager

Find what's listening on any port, get detailed process info, and kill processes by port.

```
$ whport list --port 3000
PORT   PID    PROCESS   USER    PROTOCOL
3000   12345  node      zhengda tcp

$ whport kill 3000
```

### netwhiz — Network Diagnostics

Full network overview, WiFi signal analysis, DNS management, speed tests, and LAN scanning.

```
$ netwhiz info
Interface: en0 (Wi-Fi)  Status: active
Local IP:  192.168.1.42  Public IP: 203.0.113.1
Gateway:   192.168.1.1   DNS: 1.1.1.1, 1.0.0.1

$ netwhiz speed
Download: 245.3 Mbps
```

### macbroom — System Cleanup

Scan for reclaimable disk space across system caches, Xcode, Docker, node_modules, and more.

```
$ macbroom scan
System caches    1.2 GB
Xcode derived    4.8 GB
Docker images    3.1 GB
node_modules     2.4 GB
Total            11.5 GB reclaimable
```

### updater — App Update Manager

Check for outdated apps, update individually or in bulk, pin versions, and rollback.

```
$ updater check
Firefox     128.0 → 129.0   (brew cask)
VS Code     1.89  → 1.90    (brew cask)
Slack       4.38  → 4.39    (App Store)
```

### macfig — macOS Defaults Manager

Browse and change hidden macOS preferences with presets for common developer setups.

```
$ macfig preset dev-setup
Applied: faster dock, show hidden files, fast key repeat, no animations
```

### lanchr — Launch Agent & Daemon Manager

List, diagnose, create, and manage launchd services without wrestling with plist XML.

```
$ lanchr doctor
2 broken plists found
1 orphaned agent (binary missing)

$ lanchr create -l com.me.backup -p ~/scripts/backup.sh --template interval --interval 3600
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

## Prerequisites

- **macOS** (tested on Sonoma and Sequoia)
- **Claude Code** with plugin support
- **Homebrew** for installing the CLI tools

## License

[MIT](LICENSE)
