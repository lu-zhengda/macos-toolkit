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
