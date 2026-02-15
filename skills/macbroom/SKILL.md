---
name: macbroom
description: This skill should be used when the user asks to "clean up Mac", "free disk space", "remove system caches", "find duplicate files", "uninstall an app completely", "visualize disk usage", "clean Xcode caches", "remove node_modules", "clean Docker", "schedule cleanup", "cleanup report", "watch disk space", "monitor free space", "filter by size", "disk trends", "storage forecast", "disk full prediction", "storage growth", or mentions macbroom, system cleanup, disk space, junk files, cache clearing, disk monitoring, cleanup history, or storage trends.
version: 1.2.0
allowed-tools: Bash(macbroom:*)
---

# macbroom — macOS Cleanup Tool

Scan for reclaimable disk space:

!`macbroom scan 2>&1 || echo "macbroom not installed — brew install lu-zhengda/tap/macbroom"`

Analyze the scan results above. Present a summary of reclaimable space by category. Then ask which categories to clean, or if the user wants to clean all. Use `macbroom clean --dry-run` first to preview, then `macbroom clean` to execute.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `macbroom scan` | Scan for reclaimable space | `macbroom scan` |
| `macbroom scan --<category>` | Scan specific category | `macbroom scan --xcode --docker` |
| `macbroom scan --threshold N` | Only show items above size threshold | `macbroom scan --threshold 100M` |
| `macbroom clean` | Clean scanned junk files | `macbroom clean --dry-run` |
| `macbroom clean --permanent` | Delete permanently (skip Trash) | `macbroom clean --permanent -y` |
| `macbroom uninstall <app>` | Fully uninstall app + related files | `macbroom uninstall Slack` |
| `macbroom maintain` | Run system maintenance tasks | `macbroom maintain` |
| `macbroom spacelens [path]` | Visualize disk space usage | `macbroom spacelens ~ --depth 3` |
| `macbroom spacelens -i` | Interactive disk space explorer | `macbroom spacelens -i` |
| `macbroom dupes [dirs...]` | Find duplicate files | `macbroom dupes ~/Documents` |
| `macbroom stats` | Show cleanup history | `macbroom stats` |
| `macbroom report` | Cleanup history report | `macbroom report` |
| `macbroom watch` | Monitor free disk space | `macbroom watch --free 10G` |
| `macbroom schedule enable` | Enable scheduled cleaning | `macbroom schedule enable` |
| `macbroom schedule disable` | Disable scheduled cleaning | `macbroom schedule disable` |
| `macbroom schedule status` | Show schedule status | `macbroom schedule status` |
| `macbroom trends` | Storage usage trends from snapshots | `macbroom trends --last 30d` |
| `macbroom trends --forecast` | Predict when disk will fill up | `macbroom trends --forecast` |
| `macbroom trends record` | Capture storage snapshot (for lanchr agent) | `macbroom trends record` |

## Storage Trends

Track storage usage over time and forecast disk capacity:

```bash
# View storage usage trends
macbroom trends

# Filter to last 30 days
macbroom trends --last 30d

# Predict when disk will fill up
macbroom trends --forecast

# Capture a storage snapshot (for periodic recording via lanchr agent)
macbroom trends record

# JSON output for scripting
macbroom trends --json
```

Combine with `lanchr create --template monitor-disk` to automatically record storage snapshots and alert on predicted disk full dates.

## Size Filtering

Filter scan results to only show items above a threshold:

```bash
# Only items larger than 100 MB
macbroom scan --threshold 100M

# Combine with category
macbroom scan --docker --threshold 500M
```

## Disk Space Monitoring

Watch free disk space and alert when it drops below a threshold:

```bash
# Alert when free space drops below 10 GB
macbroom watch --free 10G
```

Combine with `lanchr create --template monitor-disk` for persistent disk monitoring.

## Cleanup Reports

View cleanup history and trends:

```bash
macbroom report
```

Shows total space reclaimed, cleanup frequency, and category breakdown over time.

## JSON Output

Add `--json` to any read command for machine-readable output:

```bash
macbroom scan --json
macbroom dupes ~/Documents --json
macbroom stats --json
macbroom spacelens ~ --json
macbroom report --json
macbroom trends --json
macbroom trends --forecast --json
```

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
