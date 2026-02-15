# macos-toolkit Plugin Design

**Date**: 2026-02-14
**Status**: Approved

## Overview

A Claude Code plugin that provides skills for 6 macOS CLI/TUI tools, plus cross-tool workflow skills and slash commands. Single plugin, public distribution.

**Install**: All tools via `brew tap lu-zhengda/tap && brew install <tool>`

## Architecture

Flat skills + commands. No nesting, no agents, no hooks.

```
macos-toolkit/
├── plugin.json
├── skills/
│   ├── macfig/SKILL.md
│   ├── netwhiz/SKILL.md
│   ├── whport/SKILL.md
│   ├── lanchr/SKILL.md
│   ├── macbroom/SKILL.md
│   ├── updater/SKILL.md
│   ├── system-health/SKILL.md
│   ├── dev-setup/SKILL.md
│   └── network-debug/SKILL.md
├── commands/
│   ├── system-health.md
│   ├── dev-setup.md
│   └── network-debug.md
└── README.md
```

## Plugin Manifest

```json
{
  "name": "macos-toolkit",
  "version": "1.0.0",
  "description": "Skills for managing macOS systems with macfig, netwhiz, whport, lanchr, macbroom, and updater CLI tools",
  "author": "lu-zhengda"
}
```

## Per-Tool Skills (6)

### macfig — macOS defaults manager

- **Trigger**: configuring macOS system preferences, hidden settings, defaults, dock behavior, Finder options, keyboard repeat rate, applying system presets
- **Commands**: `list`, `get`, `set`, `preset`, `backup`, `restore`, `reset`, `search`
- **Presets**: dock-speed, finder-dev, keyboard-fast, no-animations, privacy, dev-setup
- **Safety**: always `backup` before bulk changes, use preset names when possible

### netwhiz — network diagnostics

- **Trigger**: diagnosing network issues, checking WiFi signal, changing DNS, speed tests, tracing routes, scanning local network
- **Commands**: `info`, `wifi`, `wifi scan`, `dns`, `dns set`, `dns flush`, `ping`, `trace`, `speed`, `scan`
- **DNS presets**: cloudflare, google, quad9
- **Safety**: start with `info` for overview

### whport — port/process manager

- **Trigger**: checking port listeners, killing stuck processes by port, investigating port conflicts, monitoring port activity
- **Commands**: `list`, `kill`, `info`, `watch`
- **Key flags**: `--port`, `--process`, `--protocol`, `--force`, `--json`
- **Safety**: use `info` before `kill`, prefer SIGTERM over `--force`

### lanchr — launch agent/daemon manager

- **Trigger**: managing launchd services, creating launch agents, debugging broken daemons, viewing service logs, scheduling tasks via launchd
- **Commands**: `list`, `info`, `search`, `enable`, `disable`, `load`, `unload`, `restart`, `logs`, `doctor`, `create`, `edit`
- **Templates**: simple, interval, calendar, keepalive, watcher
- **Safety**: use `doctor` first, use `create --template` not manual plist editing

### macbroom — system cleanup

- **Trigger**: cleaning system caches, finding duplicates, uninstalling apps, reclaiming disk space, visualizing disk usage
- **Commands**: `scan`, `clean`, `uninstall`, `maintain`, `spacelens`, `stats`, `dupes`, `schedule`
- **Categories**: system, browser, xcode, large, docker, node, homebrew, simulator, python, rust, go, jetbrains
- **Safety**: always `scan` before `clean`, use `--dry-run` first

### updater — app update manager

- **Trigger**: checking for app updates, updating applications, managing update sources, pinning app versions, cleaning unused apps
- **Commands**: `scan`, `check`, `outdated`, `update`, `install`, `upgrade`, `pin`, `unpin`, `rollback`, `cleanup`, `history`, `schedule`, `config`, `notify`
- **Sources**: App Store, Homebrew, Sparkle, GitHub, Electron, System, Setapp, JetBrains, Adobe
- **Safety**: `check` before `update`, `pin` critical apps, `rollback` available

## Cross-Tool Workflow Skills (3)

### system-health — full system health check

- **Trigger**: full system health check, overall macOS status, running maintenance
- **Workflow**:
  1. `macbroom scan` — reclaimable space
  2. `updater check` — outdated apps
  3. `lanchr doctor` — broken launch agents
  4. `whport list` — unexpected listeners
  5. Present summary with recommended actions

### dev-setup — developer environment configuration

- **Trigger**: setting up a new Mac for development, configuring developer defaults, optimizing dev environment
- **Workflow**:
  1. `macfig backup` — backup current settings
  2. `macfig preset dev-setup` — dev defaults
  3. `macfig preset keyboard-fast` — fast key repeat
  4. `macfig preset finder-dev` — show hidden files, extensions
  5. `updater check` — ensure dev tools current
  6. `lanchr list --no-apple` — review third-party services

### network-debug — network troubleshooting

- **Trigger**: debugging network connectivity, diagnosing slow connections, troubleshooting DNS or port issues
- **Workflow**:
  1. `netwhiz info` — network overview
  2. `netwhiz wifi` — WiFi signal quality
  3. `netwhiz dns` — DNS config
  4. `netwhiz ping <target>` — connectivity
  5. `whport list` — port conflicts
  6. `netwhiz trace <target>` — route trace if needed
  7. Present diagnosis with fixes

## Slash Commands (3)

| Command | Description | Maps to |
|---------|-------------|---------|
| `/system-health` | Run full macOS system health check | system-health skill |
| `/dev-setup` | Configure macOS for development | dev-setup skill |
| `/network-debug` | Diagnose network issues | network-debug skill |

## Skill Pattern

Every skill follows the same structure:

1. **Availability check**: verify tool installed, suggest `brew install lu-zhengda/tap/<tool>` if not
2. **Command reference**: subcommands, flags, examples
3. **Safety guidance**: backup, dry-run, preferred workflows
4. **TUI alternative**: mention `<tool>` without args launches interactive mode

## Implementation Notes

- All tools use Cobra CLI + Bubble Tea TUI
- All distributed as Homebrew casks from `lu-zhengda/homebrew-tap`
- Skills should use Bash tool to run commands
- Commands run with user's permissions (some operations need sudo)
