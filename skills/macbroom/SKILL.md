---
name: macbroom
description: This skill should be used when the user asks to "clean up Mac", "free disk space", "remove system caches", "find duplicate files", "uninstall an app completely", "visualize disk usage", "clean Xcode caches", "remove node_modules", "clean Docker", "schedule cleanup", or mentions macbroom, system cleanup, disk space, junk files, or cache clearing.
version: 1.0.0
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
