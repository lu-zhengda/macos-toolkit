---
name: pstop
description: This skill should be used when the user asks to "show running processes", "find what's using CPU", "check memory usage", "kill a process", "show process tree", "find developer processes", "watch a process", "what's draining battery", "search for a process", or mentions pstop, process management, top, htop, activity monitor, or resource hogs.
version: 1.0.0
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
| `pstop find <query>` | Search processes by name or command | `pstop find node` |
| `pstop info <pid>` | Detailed process info (files, ports, children) | `pstop info 1234` |
| `pstop kill <pid>` | Kill process (SIGTERM) | `pstop kill 1234` |
| `pstop kill <pid> --force` | Force kill (SIGKILL) | `pstop kill 1234 --force` |
| `pstop tree` | Display process tree | `pstop tree` |
| `pstop dev` | Group processes by dev stack (Node, Python, Docker, etc.) | `pstop dev` |
| `pstop watch <pid>` | Live-monitor a process | `pstop watch 1234 --interval 5` |

## Sort Options

Use `--sort` with `list` to sort by: `cpu`, `mem`, `pid`, `name` (default: cpu).

## Safety Guidelines

- **Always `info` before `kill`**: Check what a process does before terminating
- **Prefer SIGTERM (default)**: Allows graceful shutdown. Only use `--force` as last resort
- **Use `find` for discovery**: Safer than guessing PIDs

## TUI Mode

Launch `pstop` without arguments for an interactive process dashboard.
