---
name: whport
description: This skill should be used when the user asks to "check what's on a port", "find port listeners", "kill process on port", "free up a port", "check port conflicts", "monitor ports", "which process is using port", "alert on new port listeners", "port history", "port timeline", or mentions whport, port management, lsof, process killing by port number, port monitoring, or port alerting.
version: 1.1.0
allowed-tools: Bash(whport:*)
---

# whport — Port & Process Manager

List all processes listening on ports:

!`whport list 2>&1 || echo "whport not installed — brew install lu-zhengda/tap/whport"`

Analyze the output above. Flag any unexpected listeners on common ports (80, 443, 3000, 5432, 6379, 8080, 27017). For each listener, explain what the process likely is. Offer to get more details with `whport info <port>` or kill with `whport kill <port>`.

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
| `whport watch --alert` | Alert on new port listeners | `whport watch --alert` |
| `whport history` | Port open/close timeline | `whport history` |

## Port Alerting

Monitor for new port listeners and alert immediately:

```bash
# Watch for any new port listeners
whport watch --alert

# Combine with interval
whport watch --alert --interval 10
```

Combine with `lanchr create --template monitor-ports` for persistent port monitoring.

## Port History

View a timeline of port open/close events:

```bash
whport history
```

Useful for tracking when services start and stop, debugging intermittent port conflicts.

## JSON Output

Add `--json` to any command for machine-readable output:

```bash
whport list --json
whport info 8080 --json
whport history --json
```

## Safety Guidelines

- **Always `info` before `kill`**: Check what process owns the port before terminating
- **Prefer SIGTERM (default)**: Allows graceful shutdown. Only use `--force` (SIGKILL) as last resort
- **Verify after kill**: Run `whport list --port <n>` to confirm port is freed

## TUI Mode

Launch `whport` without arguments for a live port dashboard.
