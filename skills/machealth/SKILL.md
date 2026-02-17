---
name: machealth
description: This skill should be used when the user asks to "check system health", "run a health check", "is my Mac healthy", "check CPU load", "check memory pressure", "check thermal throttling", "check disk space health", "check iCloud sync", "check Time Machine backup", "monitor system health", "diagnose system issues", "what's wrong with my Mac", "system health score", or mentions machealth, system health, health assessment, health score, thermal throttling, memory pressure, or subsystem diagnostics.
version: 1.0.0
allowed-tools: Bash(machealth:*)
---

# machealth — macOS System Health Checker

Run a one-shot health assessment:

!`machealth --human 2>&1 || echo "machealth not installed — brew install lu-zhengda/tap/machealth"`

Analyze the output above. Report the overall health score and status (green/yellow/red). Flag any subsystems that are degraded or critical. Offer to run `machealth diagnose --human` for actionable recommendations or `machealth watch --human` for continuous monitoring.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `machealth` | One-shot health assessment (JSON) | `machealth` |
| `machealth check` | Same as default (explicit) | `machealth check` |
| `machealth --human` | Human-readable health report | `machealth --human` |
| `machealth diagnose` | Health check with actionable recommendations (JSON) | `machealth diagnose` |
| `machealth diagnose --human` | Human-readable diagnose output | `machealth diagnose --human` |
| `machealth watch` | Continuous monitoring (JSON Lines) | `machealth watch --interval 10s` |
| `machealth watch --human` | Live terminal dashboard | `machealth watch --human` |

## Monitored Subsystems

| Subsystem | Weight | Metrics |
|-----------|--------|---------|
| CPU | 20% | Load averages, per-core utilization |
| Memory | 25% | Memory pressure, swap usage |
| Thermal | 20% | CPU speed limit, throttling detection |
| Disk | 15% | Available space, usage percentage |
| Battery | 10% | Charge level, power source, health |
| iCloud | 5% | Sync status |
| Network | 5% | Reachability, active interface |
| Time Machine | 0% | Backup state (informational only) |

## Exit Codes

| Code | Status | Score Range |
|------|--------|-------------|
| 0 | Healthy (green) | 80-100 |
| 1 | Degraded (yellow) | 50-79 |
| 2 | Critical (red) | 0-49 |

## JSON Output

All commands output JSON by default. Use `--human` for human-readable format:

```bash
machealth                    # JSON output
machealth --human            # Human-readable output
machealth diagnose           # JSON with recommendations
machealth diagnose --human   # Human-readable recommendations
```

## Safety Guidelines

- **Read-only**: All commands are non-destructive — they only read system metrics
- **Low overhead**: Health checks are lightweight and safe to run frequently
- **Watch mode**: Continuous monitoring exits cleanly on Ctrl+C
