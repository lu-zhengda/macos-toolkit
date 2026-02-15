---
name: pstop
description: This skill should be used when the user asks to "show running processes", "find what's using CPU", "check memory usage", "kill a process", "show process tree", "find developer processes", "watch a process", "what's draining battery", "search for a process", "alert on high CPU", "alert on high memory", "show CPU sparklines", "crash reports", "app crashes", "app hangs", "kernel panics", "diagnostic reports", or mentions pstop, process management, top, htop, activity monitor, resource hogs, threshold alerting, or crash diagnostics.
version: 1.2.0
allowed-tools: Bash(pstop:*)
---

# pstop — Process Explorer

Show top resource-consuming processes:

!`pstop top 2>&1 || echo "pstop not installed — brew install lu-zhengda/tap/pstop"`

Analyze the output above. Flag any processes consuming excessive CPU (>50%) or memory (>1 GB). Identify what each process is and whether it looks normal. Offer to get more details with `pstop info <pid>` or kill with `pstop kill <pid>`.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `pstop list` | List all running processes | `pstop list --sort cpu` |
| `pstop list --user <name>` | Filter by user | `pstop list --user root` |
| `pstop top` | Show top N processes by CPU | `pstop top -n 20` |
| `pstop top --battery` | Highlight battery-draining processes | `pstop top --battery` |
| `pstop top --format spark` | Show CPU/mem sparkline trends | `pstop top --format spark` |
| `pstop find <query>` | Search processes by name or command | `pstop find node` |
| `pstop info <pid>` | Detailed process info (files, ports, children) | `pstop info 1234` |
| `pstop kill <pid>` | Kill process (SIGTERM) | `pstop kill 1234` |
| `pstop kill <pid> --force` | Force kill (SIGKILL) | `pstop kill 1234 --force` |
| `pstop tree` | Display process tree | `pstop tree` |
| `pstop dev` | Group processes by dev stack (Node, Python, Docker, etc.) | `pstop dev` |
| `pstop watch <pid>` | Live-monitor a process | `pstop watch 1234 --interval 5` |
| `pstop watch --alert` | Threshold-based alerting | `pstop watch --alert --cpu 80 --mem 90` |
| `pstop crashes` | Recent crash reports and app hangs | `pstop crashes --last 7d` |
| `pstop crashes info <path>` | Detailed crash report with backtrace | `pstop crashes info /path/to/report.ips` |

## Crash Reports

View recent crash reports, app hangs, and kernel panics:

```bash
# Show recent crash reports
pstop crashes

# Filter by time range
pstop crashes --last 7d

# Filter by process name
pstop crashes --process Safari

# Combine filters
pstop crashes --last 24h --process node

# View detailed crash report with backtrace
pstop crashes info /Library/Logs/DiagnosticReports/Safari_2026-02-15.ips

# JSON output for scripting
pstop crashes --json
```

## Threshold Alerting

Monitor processes and alert when thresholds are exceeded:

```bash
# Alert when any process exceeds 80% CPU or 90% memory
pstop watch --alert --cpu 80 --mem 90

# Exit code 1 when thresholds exceeded — useful in scripts and lanchr agents
pstop watch --alert --cpu 95 --mem 95
```

Combine with `lanchr create --template monitor-cpu` to set up persistent alerting.

## Sparkline Format

View inline CPU/memory trend history:

```bash
pstop top --format spark
```

Renders compact sparkline graphs alongside each process, showing resource usage over time.

## Sort Options

Use `--sort` with `list` to sort by: `cpu`, `mem`, `pid`, `name` (default: cpu).

## Safety Guidelines

- **Always `info` before `kill`**: Check what a process does before terminating
- **Prefer SIGTERM (default)**: Allows graceful shutdown. Only use `--force` as last resort
- **Use `find` for discovery**: Safer than guessing PIDs

## TUI Mode

Launch `pstop` without arguments for an interactive process dashboard.
