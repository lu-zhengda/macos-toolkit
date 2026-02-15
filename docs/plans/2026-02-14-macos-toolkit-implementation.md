# macos-toolkit Plugin Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a Claude Code plugin that provides skills for 6 macOS CLI tools plus cross-tool workflow skills and slash commands.

**Architecture:** Flat plugin with 9 skills (6 per-tool + 3 workflow) and 3 slash commands. No agents, hooks, or MCP servers. Plugin manifest at `.claude-plugin/plugin.json`, skills auto-discovered from `skills/`, commands auto-discovered from `commands/`.

**Tech Stack:** Claude Code plugin system (Markdown + YAML frontmatter)

---

### Task 1: Scaffold plugin structure

**Files:**
- Create: `macos-toolkit/.claude-plugin/plugin.json`

**Step 1: Create plugin manifest**

Create `.claude-plugin/plugin.json`:

```json
{
  "name": "macos-toolkit",
  "version": "1.0.0",
  "description": "Skills for managing macOS systems with macfig, netwhiz, whport, lanchr, macbroom, and updater CLI tools",
  "author": "lu-zhengda",
  "repository": "https://github.com/lu-zhengda/macos-toolkit",
  "license": "MIT",
  "keywords": ["macos", "system", "cleanup", "network", "defaults", "launchd"]
}
```

**Step 2: Create directory skeleton**

Run:
```bash
mkdir -p macos-toolkit/skills/{macfig,netwhiz,whport,lanchr,macbroom,updater,system-health,dev-setup,network-debug}
mkdir -p macos-toolkit/commands
```

**Step 3: Verify structure**

Run: `find macos-toolkit -type d | sort`

Expected: all directories exist.

---

### Task 2: Create macfig skill

**Files:**
- Create: `macos-toolkit/skills/macfig/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: macfig
description: This skill should be used when the user asks to "change macOS defaults", "configure system preferences", "set hidden settings", "adjust dock speed", "show Finder hidden files", "set keyboard repeat rate", "apply system preset", "backup macOS settings", "restore defaults", "search macOS preferences", or mentions macfig, defaults write, or macOS hidden settings.
version: 1.0.0
---

# macfig — macOS Defaults Manager

## Overview

macfig manages macOS hidden defaults (system preferences not exposed in System Settings). It provides preset bundles, backup/restore, and cross-domain search. All changes take effect immediately with automatic process restart (Dock, Finder, etc.).

## Availability

Verify macfig is installed before use:

```bash
command -v macfig >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/macfig`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `macfig list` | List preset categories | `macfig list` |
| `macfig list <category>` | Show settings in category with current values | `macfig list dock` |
| `macfig get <domain> <key>` | Read a specific defaults value | `macfig get com.apple.dock autohide` |
| `macfig set <domain> <key> <value>` | Write a defaults value | `macfig set com.apple.dock autohide true` |
| `macfig preset <name>` | Apply a named preset bundle | `macfig preset dev-setup` |
| `macfig backup` | Backup all tracked settings to JSON | `macfig backup` |
| `macfig restore <file>` | Restore from backup file | `macfig restore ~/.config/macfig/backup.json` |
| `macfig reset <domain> <key>` | Delete a key (restore macOS default) | `macfig reset com.apple.dock autohide` |
| `macfig search <query>` | Search keys across all domains | `macfig search animation` |

## Built-in Presets

| Preset | Effect |
|--------|--------|
| `dock-speed` | Faster Dock animations and autohide delay |
| `finder-dev` | Show hidden files, extensions, path bar, status bar |
| `keyboard-fast` | Faster key repeat rate and shorter delay |
| `no-animations` | Disable most macOS animations |
| `privacy` | Disable analytics, Siri suggestions, ad tracking |
| `dev-setup` | Combined developer-friendly defaults |

## Safety Guidelines

- **Always backup first**: Run `macfig backup` before applying presets or bulk changes
- **Use presets over raw set**: Presets handle type inference and process restart automatically
- **Use search to discover**: Run `macfig search <term>` instead of guessing domain/key pairs
- **Revert with restore**: If something breaks, `macfig restore <backup-file>` reverts all changes

## TUI Mode

Launch `macfig` without arguments for interactive mode — browse categories, toggle settings, and preview changes.
```

**Step 2: Verify file exists**

Run: `test -f macos-toolkit/skills/macfig/SKILL.md && echo OK`
Expected: OK

---

### Task 3: Create netwhiz skill

**Files:**
- Create: `macos-toolkit/skills/netwhiz/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: netwhiz
description: This skill should be used when the user asks to "diagnose network issues", "check WiFi signal", "change DNS server", "run speed test", "trace route", "scan local network", "check IP address", "flush DNS", "find network devices", or mentions netwhiz, network diagnostics, WiFi troubleshooting, or DNS configuration.
version: 1.0.0
---

# netwhiz — Network Diagnostics

## Overview

netwhiz is a network diagnostics Swiss-army-knife for macOS. It combines IP info, WiFi analysis, DNS management, ping, traceroute, speed testing, and LAN scanning into one tool.

## Availability

```bash
command -v netwhiz >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/netwhiz`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `netwhiz info` | Network overview (IP, gateway, DNS, interface, public IP) | `netwhiz info` |
| `netwhiz wifi` | WiFi details (SSID, channel, RSSI, SNR, security) | `netwhiz wifi` |
| `netwhiz wifi scan` | Scan nearby WiFi networks with signal bars | `netwhiz wifi scan` |
| `netwhiz dns` | Show current DNS servers and presets | `netwhiz dns` |
| `netwhiz dns set <server>` | Set DNS (cloudflare, google, or custom IP) | `netwhiz dns set cloudflare` |
| `netwhiz dns flush` | Flush DNS cache | `netwhiz dns flush` |
| `netwhiz ping <host>` | Enhanced ping with stats and RTT bars | `netwhiz ping 8.8.8.8 -c 10` |
| `netwhiz trace <host>` | Traceroute with per-hop RTT | `netwhiz trace example.com` |
| `netwhiz speed` | Download speed test via Cloudflare | `netwhiz speed` |
| `netwhiz scan` | ARP scan to discover LAN devices | `netwhiz scan` |

## DNS Presets

Use preset names with `dns set` instead of raw IPs:

| Preset | Servers |
|--------|---------|
| `cloudflare` | 1.1.1.1, 1.0.0.1 |
| `google` | 8.8.8.8, 8.8.4.4 |
| `quad9` | 9.9.9.9, 149.112.112.112 |

## Diagnostic Workflow

For network troubleshooting, follow this order:

1. `netwhiz info` — get network overview and identify interface
2. `netwhiz wifi` — check signal quality (if wireless)
3. `netwhiz dns` — verify DNS configuration
4. `netwhiz ping <target>` — test connectivity and latency
5. `netwhiz trace <target>` — identify where packets are dropping
6. `netwhiz speed` — measure throughput

## TUI Mode

Launch `netwhiz` without arguments for an interactive network dashboard.
```

---

### Task 4: Create whport skill

**Files:**
- Create: `macos-toolkit/skills/whport/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: whport
description: This skill should be used when the user asks to "check what's on a port", "find port listeners", "kill process on port", "free up a port", "check port conflicts", "monitor ports", "which process is using port", or mentions whport, port management, lsof, or process killing by port number.
version: 1.0.0
---

# whport — Port & Process Manager

## Overview

whport manages ports and their associated processes on macOS. It lists listeners, shows detailed process info, kills by port, and monitors in real-time.

## Availability

```bash
command -v whport >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/whport`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `whport list` | List all listening ports | `whport list` |
| `whport list --port <n>` | Filter by port number | `whport list --port 3000` |
| `whport list --process <name>` | Filter by process name | `whport list --process node` |
| `whport list --protocol <tcp\|udp>` | Filter by protocol | `whport list --protocol tcp` |
| `whport list --all` | Include ESTABLISHED connections | `whport list --all` |
| `whport info <port>` | Detailed process info (PID, CPU, memory, children) | `whport info 8080` |
| `whport kill <port>` | Kill process on port (SIGTERM) | `whport kill 3000` |
| `whport kill <port> --force` | Force kill (SIGKILL) | `whport kill 3000 --force` |
| `whport kill <port> --signal <sig>` | Custom signal | `whport kill 3000 --signal SIGHUP` |
| `whport watch` | Live auto-refresh port table | `whport watch --interval 5` |

## JSON Output

Add `--json` to any command for machine-readable output:

```bash
whport list --json
whport info 8080 --json
```

## Safety Guidelines

- **Always `info` before `kill`**: Check what process owns the port before terminating
- **Prefer SIGTERM (default)**: Allows graceful shutdown. Only use `--force` (SIGKILL) as last resort
- **Verify after kill**: Run `whport list --port <n>` to confirm port is freed

## Common Scenarios

**Port conflict during development:**
```bash
whport info 3000        # See what's using port 3000
whport kill 3000        # Gracefully stop it
whport list --port 3000 # Verify it's free
```

## TUI Mode

Launch `whport` without arguments for a live port dashboard.
```

---

### Task 5: Create lanchr skill

**Files:**
- Create: `macos-toolkit/skills/lanchr/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: lanchr
description: This skill should be used when the user asks to "manage launch agents", "create a launchd service", "debug a daemon", "view service logs", "enable or disable a service", "check broken plists", "schedule a task with launchd", "list running services", "restart a service", or mentions lanchr, launchd, launchctl, plist, launch agent, or launch daemon.
version: 1.0.0
---

# lanchr — Launch Agent & Daemon Manager

## Overview

lanchr manages macOS launchd services (agents and daemons). It lists, inspects, creates, enables/disables, and diagnoses services across user, global, and system domains.

## Availability

```bash
command -v lanchr >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/lanchr`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `lanchr list` | List all services | `lanchr list --no-apple` |
| `lanchr list -d <domain>` | Filter by domain (user/global/system) | `lanchr list -d user` |
| `lanchr list -s <status>` | Filter by status (running/stopped/error) | `lanchr list -s error` |
| `lanchr info <label>` | Detailed service info (all plist keys + runtime) | `lanchr info com.example.myapp` |
| `lanchr search <query>` | Search by label, path, or content | `lanchr search redis` |
| `lanchr enable <label>` | Enable a disabled service (persists) | `lanchr enable com.example.myapp` |
| `lanchr disable <label>` | Disable a service (persists) | `lanchr disable com.example.myapp` |
| `lanchr load <path>` | Bootstrap a plist file | `lanchr load ~/Library/LaunchAgents/com.example.plist` |
| `lanchr unload <label>` | Bootout a service | `lanchr unload com.example.myapp` |
| `lanchr restart <label>` | Force restart a running service | `lanchr restart com.example.myapp` |
| `lanchr logs <label>` | View service logs | `lanchr logs com.example.myapp -f` |
| `lanchr doctor` | Diagnose broken plists and orphaned agents | `lanchr doctor` |
| `lanchr create` | Scaffold a new plist from template | See below |
| `lanchr edit <label>` | Open plist in $EDITOR | `lanchr edit com.example.myapp` |

## Creating Launch Agents

Use `lanchr create` with templates instead of writing plist XML manually:

```bash
# Simple agent that runs a script at login
lanchr create -l com.me.backup -p /usr/local/bin/backup.sh --template simple --run-at-load

# Interval-based agent (every 30 minutes)
lanchr create -l com.me.sync -p /usr/local/bin/sync.sh --template interval --interval 1800

# Calendar-based agent (daily at 9am)
lanchr create -l com.me.report -p /usr/local/bin/report.sh --template calendar --calendar "Hour=9 Minute=0"

# Keep-alive agent (restart on crash)
lanchr create -l com.me.server -p /usr/local/bin/server --template keepalive --keep-alive

# File watcher agent
lanchr create -l com.me.watcher -p /usr/local/bin/process.sh --template watcher
```

Additional flags: `--stdout <path>`, `--stderr <path>`, `--env KEY=VAL`, `--load` (bootstrap after creation).

## Diagnostic Workflow

1. `lanchr doctor` — identify broken plists, orphaned agents, missing binaries
2. `lanchr list -s error` — find services in error state
3. `lanchr logs <label> -f` — follow logs for a specific service
4. `lanchr restart <label>` — restart a misbehaving service

## TUI Mode

Launch `lanchr` without arguments for interactive service management.
```

---

### Task 6: Create macbroom skill

**Files:**
- Create: `macos-toolkit/skills/macbroom/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: macbroom
description: This skill should be used when the user asks to "clean up Mac", "free disk space", "remove system caches", "find duplicate files", "uninstall an app completely", "visualize disk usage", "clean Xcode caches", "remove node_modules", "clean Docker", "schedule cleanup", or mentions macbroom, system cleanup, disk space, junk files, or cache clearing.
version: 1.0.0
---

# macbroom — macOS Cleanup Tool

## Overview

macbroom scans and cleans system junk, developer caches, browser data, and more. It supports 12 scanner categories, duplicate detection, app uninstallation, disk visualization, and scheduled cleaning.

## Availability

```bash
command -v macbroom >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/macbroom`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `macbroom scan` | Scan for reclaimable space | `macbroom scan` |
| `macbroom scan --<category>` | Scan specific category | `macbroom scan --xcode --docker` |
| `macbroom clean` | Clean scanned junk files | `macbroom clean --dry-run` |
| `macbroom clean --permanent` | Delete permanently (skip Trash) | `macbroom clean --permanent -y` |
| `macbroom uninstall <app>` | Fully uninstall app + related files | `macbroom uninstall Slack` |
| `macbroom maintain` | Run system maintenance tasks | `macbroom maintain` |
| `macbroom spacelens [path]` | Visualize disk space usage | `macbroom spacelens ~ --depth 3` |
| `macbroom spacelens -i` | Interactive disk space explorer | `macbroom spacelens -i` |
| `macbroom dupes [dirs...]` | Find duplicate files | `macbroom dupes ~/Documents` |
| `macbroom stats` | Show cleanup history | `macbroom stats` |
| `macbroom schedule enable` | Enable scheduled cleaning | `macbroom schedule enable` |
| `macbroom schedule disable` | Disable scheduled cleaning | `macbroom schedule disable` |
| `macbroom schedule status` | Show schedule status | `macbroom schedule status` |

## Scanner Categories

Use category flags with `scan` or `clean`:

| Flag | What it scans |
|------|---------------|
| `--system` | System caches, logs, temp files |
| `--browser` | Browser caches and data |
| `--xcode` | Derived data, archives, simulators |
| `--large` | Large files (configurable threshold) |
| `--docker` | Docker images, containers, volumes |
| `--node` | node_modules, npm/yarn cache |
| `--homebrew` | Homebrew cache, old versions |
| `--simulator` | iOS Simulator data |
| `--python` | pip cache, __pycache__, venvs |
| `--rust` | Cargo cache, target directories |
| `--go` | Go module cache, build cache |
| `--jetbrains` | JetBrains IDE caches and logs |

No flags = scan all categories.

## Safety Guidelines

- **Always scan before clean**: `macbroom scan` first to review what will be removed
- **Use `--dry-run`**: `macbroom clean --dry-run` shows what would be deleted without deleting
- **Default goes to Trash**: Files go to Trash unless `--permanent` is specified
- **Backup important data**: Especially before `uninstall` or `dupes` cleanup

## TUI Mode

Launch `macbroom` without arguments for interactive cleanup with category selection and preview.
```

---

### Task 7: Create updater skill

**Files:**
- Create: `macos-toolkit/skills/updater/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: updater
description: This skill should be used when the user asks to "check for app updates", "update my apps", "update macOS applications", "pin an app version", "rollback an update", "find unused apps", "schedule update checks", "install an app via brew", "manage app update sources", or mentions updater, app updates, outdated apps, Homebrew cask updates, or App Store updates.
version: 1.0.0
---

# updater — macOS App Update Manager

## Overview

updater manages application updates across multiple sources: App Store, Homebrew (cask + formula), Sparkle, GitHub releases, Electron auto-update, Setapp, JetBrains Toolbox, and Adobe Creative Cloud. It supports pinning, rollback, scheduling, and unused app detection.

## Availability

```bash
command -v updater >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/updater`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `updater scan` | Discover installed apps and their update sources | `updater scan` |
| `updater check` | Check for available updates | `updater check` |
| `updater check -v` | Check with release notes | `updater check -v` |
| `updater outdated` | List outdated apps as JSON | `updater outdated` |
| `updater update <app>` | Update specific app | `updater update Firefox` |
| `updater update --all` | Update all apps | `updater update --all` |
| `updater update --auto` | Unattended update (skip major, pinned, manual) | `updater update --auto` |
| `updater install <name>` | Install app via Homebrew Cask | `updater install visual-studio-code` |
| `updater upgrade` | Self-update updater | `updater upgrade` |
| `updater pin <app>` | Pin app to prevent updates | `updater pin Photoshop` |
| `updater unpin <app>` | Unpin app | `updater unpin Photoshop` |
| `updater rollback <app>` | Restore previous version from backup | `updater rollback Firefox` |
| `updater cleanup` | Find unused apps | `updater cleanup --days 90` |
| `updater cleanup --delete` | Move unused apps to Trash | `updater cleanup --days 90 --delete` |
| `updater history` | Show update history | `updater history -n 50` |
| `updater schedule` | Schedule periodic checks | `updater schedule --interval 24` |
| `updater schedule --remove` | Remove scheduled checks | `updater schedule --remove` |
| `updater config export` | Export config to YAML | `updater config export` |
| `updater config import <file>` | Import config | `updater config import config.yaml` |

## Update Workflow

1. `updater check` — see what's outdated
2. `updater update <app>` — update specific apps, or `--all` for everything
3. `updater history` — verify updates applied

For unattended use: `updater update --auto` skips major version bumps, pinned apps, and manual-only sources.

## Safety Guidelines

- **Check before update**: Always `updater check` first to review available updates
- **Pin critical apps**: Use `updater pin <app>` for apps where version matters
- **Rollback available**: `updater rollback <app>` restores from pre-update backup
- **Schedule for awareness**: `updater schedule` sends macOS notifications, does not auto-update

## TUI Mode

Launch `updater` without arguments for interactive update management.
```

---

### Task 8: Create system-health workflow skill

**Files:**
- Create: `macos-toolkit/skills/system-health/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: system-health
description: This skill should be used when the user asks to "check system health", "run system health check", "how is my Mac doing", "check macOS status", "system maintenance", "overall system check", or mentions system health, Mac health check, or system diagnostics.
version: 1.0.0
---

# System Health Check — Cross-Tool Workflow

## Overview

A comprehensive macOS health check combining macbroom (disk), updater (apps), lanchr (services), and whport (ports) to produce an actionable system status report.

## Prerequisites

This workflow uses multiple tools. Verify availability:

```bash
for tool in macbroom updater lanchr whport; do
  command -v "$tool" >/dev/null && echo "$tool: OK" || echo "$tool: MISSING — brew install lu-zhengda/tap/$tool"
done
```

Proceed with available tools, skip checks for missing ones.

## Workflow

Execute these steps in order, presenting results as a unified report:

### Step 1: Disk space check

```bash
macbroom scan
```

Report: total reclaimable space, top categories by size.

### Step 2: App update check

```bash
updater check
```

Report: number of outdated apps, list with current vs available versions.

### Step 3: Launch agent health

```bash
lanchr doctor
```

Report: broken plists, orphaned agents, missing binaries.

### Step 4: Port audit

```bash
whport list
```

Report: number of listening ports, flag any unexpected listeners on common ports (80, 443, 8080, 3000, 5432, 6379, 27017).

### Step 5: Present summary

Combine all results into a health report with sections:
- **Disk**: reclaimable space and recommendation
- **Apps**: outdated count and recommendation to update
- **Services**: broken service count and fixes
- **Ports**: unexpected listeners and what to investigate

Offer to fix issues: clean disk (`macbroom clean`), update apps (`updater update --all`), restart broken services (`lanchr restart`).
```

---

### Task 9: Create dev-setup workflow skill

**Files:**
- Create: `macos-toolkit/skills/dev-setup/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: dev-setup
description: This skill should be used when the user asks to "set up Mac for development", "configure developer defaults", "optimize dev environment", "new Mac setup", "developer settings", "show hidden files", "faster keyboard repeat", or mentions dev setup, developer configuration, or new Mac for coding.
version: 1.0.0
---

# Developer Environment Setup — Cross-Tool Workflow

## Overview

Configures macOS for development by applying developer-friendly defaults, checking tool freshness, and reviewing background services. Uses macfig (defaults), updater (apps), and lanchr (services).

## Prerequisites

```bash
for tool in macfig updater lanchr; do
  command -v "$tool" >/dev/null && echo "$tool: OK" || echo "$tool: MISSING — brew install lu-zhengda/tap/$tool"
done
```

## Workflow

### Step 1: Backup current settings

```bash
macfig backup
```

**Important**: Always backup before changing defaults. Note the backup file path for potential rollback.

### Step 2: Apply developer presets

```bash
macfig preset dev-setup
macfig preset keyboard-fast
macfig preset finder-dev
```

These configure:
- **dev-setup**: Developer-friendly Dock and system behavior
- **keyboard-fast**: Fast key repeat rate, short initial delay
- **finder-dev**: Show hidden files, all file extensions, path bar, status bar

### Step 3: Check development tools

```bash
updater check
```

Review which dev tools have updates available. Recommend updating.

### Step 4: Review background services

```bash
lanchr list --no-apple
```

List third-party services. Flag any that might conflict with development (e.g., services using common dev ports).

### Step 5: Present summary

Report what changed:
- Defaults applied (list each preset and its effects)
- Backup location for rollback (`macfig restore <path>`)
- Outdated dev tools to update
- Third-party services to be aware of
```

---

### Task 10: Create network-debug workflow skill

**Files:**
- Create: `macos-toolkit/skills/network-debug/SKILL.md`

**Step 1: Write SKILL.md**

```markdown
---
name: network-debug
description: This skill should be used when the user asks to "debug network", "fix network issues", "why is my internet slow", "diagnose connectivity", "troubleshoot DNS", "check network connection", "fix WiFi", or mentions network debugging, connection problems, slow internet, or network troubleshooting.
version: 1.0.0
---

# Network Debugging — Cross-Tool Workflow

## Overview

Systematic network troubleshooting combining netwhiz (diagnostics) and whport (port conflicts) to identify and fix connectivity issues.

## Prerequisites

```bash
for tool in netwhiz whport; do
  command -v "$tool" >/dev/null && echo "$tool: OK" || echo "$tool: MISSING — brew install lu-zhengda/tap/$tool"
done
```

## Workflow

### Step 1: Network overview

```bash
netwhiz info
```

Check: IP address assigned, gateway reachable, DNS configured, public IP resolving. If no IP or gateway, the issue is at Layer 2/3.

### Step 2: WiFi signal quality (if wireless)

```bash
netwhiz wifi
```

Check RSSI (signal strength) and SNR (signal-to-noise ratio):
- RSSI > -50 dBm: Excellent
- RSSI -50 to -70: Good
- RSSI -70 to -80: Weak (may cause issues)
- RSSI < -80: Very weak (likely causing problems)

If signal is weak, recommend moving closer to AP or checking for interference with `netwhiz wifi scan`.

### Step 3: DNS check

```bash
netwhiz dns
```

Verify DNS servers are responsive. If using ISP DNS and experiencing slow resolution, recommend switching:

```bash
netwhiz dns set cloudflare
```

### Step 4: Connectivity test

```bash
netwhiz ping 8.8.8.8 -c 5
netwhiz ping google.com -c 5
```

- First ping tests raw IP connectivity (bypasses DNS)
- Second ping tests DNS resolution + connectivity
- If first works but second fails: DNS issue
- If both fail: routing or connectivity issue

### Step 5: Port conflict check

```bash
whport list
```

Check if any local process is interfering with network traffic (e.g., VPN, proxy, firewall on unexpected ports).

### Step 6: Route analysis (if needed)

```bash
netwhiz trace <target>
```

Only run if ping shows issues. Identifies where in the path packets are being dropped or delayed.

### Step 7: Present diagnosis

Summarize findings:
- **Connection status**: Connected / No IP / No gateway
- **Signal quality**: Strong / Weak / N/A (wired)
- **DNS**: Working / Slow / Failing
- **Latency**: Low / High / Packet loss
- **Port conflicts**: None / List conflicts
- **Recommended fixes**: Specific actions based on findings
```

---

### Task 11: Create slash commands

**Files:**
- Create: `macos-toolkit/commands/system-health.md`
- Create: `macos-toolkit/commands/dev-setup.md`
- Create: `macos-toolkit/commands/network-debug.md`

**Step 1: Create /system-health command**

```markdown
---
description: Run a full macOS system health check
allowed-tools: Bash(macbroom:*, updater:*, lanchr:*, whport:*)
---

Run a comprehensive macOS system health check using the system-health skill.

Execute each diagnostic step, collect results, and present a unified health report with actionable recommendations.
```

**Step 2: Create /dev-setup command**

```markdown
---
description: Configure macOS for development with optimal defaults
allowed-tools: Bash(macfig:*, updater:*, lanchr:*)
---

Set up this Mac for development using the dev-setup skill.

Start by backing up current settings, then apply developer presets, check for dev tool updates, and review background services. Present a summary of all changes with rollback instructions.
```

**Step 3: Create /network-debug command**

```markdown
---
description: Diagnose network connectivity and performance issues
allowed-tools: Bash(netwhiz:*, whport:*)
argument-hint: [target-host]
---

Diagnose network issues using the network-debug skill.

Target host for connectivity tests: $ARGUMENTS

If no target specified, use 8.8.8.8 as default. Run through the full diagnostic workflow and present findings with recommended fixes.
```

**Step 4: Verify all commands exist**

Run: `ls macos-toolkit/commands/`
Expected: `dev-setup.md  network-debug.md  system-health.md`

---

### Task 12: Create README and commit

**Files:**
- Create: `macos-toolkit/README.md`

**Step 1: Write README**

```markdown
# macos-toolkit

A Claude Code plugin for managing macOS systems with CLI tools.

## Install

```bash
claude plugin add lu-zhengda/macos-toolkit
```

### Prerequisites

Install the macOS CLI tools:

```bash
brew tap lu-zhengda/tap
brew install macfig netwhiz whport lanchr macbroom updater
```

## Tools

| Tool | Purpose |
|------|---------|
| [macfig](https://github.com/lu-zhengda/macfig) | macOS hidden defaults manager |
| [netwhiz](https://github.com/lu-zhengda/netwhiz) | Network diagnostics |
| [whport](https://github.com/lu-zhengda/whport) | Port & process manager |
| [lanchr](https://github.com/lu-zhengda/lanchr) | Launch agent & daemon manager |
| [macbroom](https://github.com/lu-zhengda/macbroom) | System cleanup tool |
| [updater](https://github.com/lu-zhengda/updater) | App update manager |

## Skills

The plugin provides 6 tool-specific skills that activate automatically when relevant, plus 3 cross-tool workflow skills.

## Commands

| Command | Description |
|---------|-------------|
| `/system-health` | Run full macOS system health check |
| `/dev-setup` | Configure macOS for development |
| `/network-debug` | Diagnose network issues |

## License

MIT
```

**Step 2: Git init and commit**

```bash
cd macos-toolkit
git init -b main
git add -A
git commit -m "feat: initial macos-toolkit plugin"
```

**Step 3: Verify**

Run: `find macos-toolkit -type f | sort`

Expected output should list all 14 files:
- `.claude-plugin/plugin.json`
- `README.md`
- `commands/dev-setup.md`
- `commands/network-debug.md`
- `commands/system-health.md`
- `skills/dev-setup/SKILL.md`
- `skills/lanchr/SKILL.md`
- `skills/macbroom/SKILL.md`
- `skills/macfig/SKILL.md`
- `skills/netwhiz/SKILL.md`
- `skills/network-debug/SKILL.md`
- `skills/system-health/SKILL.md`
- `skills/updater/SKILL.md`
- `skills/whport/SKILL.md`

---

### Task 13: Create GitHub repo and release

**Step 1: Create GitHub repo**

```bash
cd macos-toolkit
gh repo create lu-zhengda/macos-toolkit --public --source=. --push
```

**Step 2: Verify**

```bash
gh repo view lu-zhengda/macos-toolkit
```

---

### Task 14: Install and test plugin locally

**Step 1: Install plugin**

```bash
claude plugin add --local /Users/zhengda.lu/Documents/GitHub/macos-toolkit
```

Or if that doesn't work, try:
```bash
claude plugin add lu-zhengda/macos-toolkit
```

**Step 2: Verify plugin loads**

Start a new Claude Code session and verify:
- Skills appear in the skill list
- `/system-health`, `/dev-setup`, `/network-debug` appear as available commands
- Ask "what's using port 3000?" — should trigger whport skill
- Ask "clean up my Mac" — should trigger macbroom skill

**Step 3: Test each slash command**

Run `/system-health`, `/dev-setup`, `/network-debug` and verify they work.
