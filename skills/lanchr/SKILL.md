---
name: lanchr
description: This skill should be used when the user asks to "manage launch agents", "create a launchd service", "debug a daemon", "view service logs", "enable or disable a service", "check broken plists", "schedule a task with launchd", "list running services", "restart a service", or mentions lanchr, launchd, launchctl, plist, launch agent, or launch daemon.
version: 1.0.0
allowed-tools: Bash(lanchr:*)
---

# lanchr — Launch Agent & Daemon Manager

Diagnose launch agent and daemon health:

!`lanchr doctor 2>&1 || echo "lanchr not installed — brew install lu-zhengda/tap/lanchr"`

Analyze the output above. Report broken plists, orphaned agents, and missing binaries. For each issue, explain the impact and recommend a fix (restart, disable, or remove). Offer to execute fixes.

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
