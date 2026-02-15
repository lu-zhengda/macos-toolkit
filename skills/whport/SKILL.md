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
