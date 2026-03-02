---
name: updater
description: This skill should be used when the user asks to check/update macOS apps, list outdated software, run updater in JSON/agent mode, pin or set app update policy, rollback updates, clean unused apps, schedule checks, manage the menu bar agent, install apps with Homebrew casks, or self-update updater.
version: 1.1.0
allowed-tools: Bash(updater:*)
---

# updater — macOS App Update Manager

Run a health + update discovery pass:

!`updater doctor --json 2>&1 || echo "updater not installed — brew install --cask lu-zhengda/tap/updater"`

If updater is installed, use this default flow:

1. `updater scan --json`
2. `updater check --json`
3. `updater update --all --dry-run --json`
4. Ask for confirmation before any non-dry-run update command.

Summarize what is outdated (current vs latest), call out major updates, pinned apps, and errors, then propose specific next actions.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `updater scan --json` | Discover installed apps and update sources | `updater scan --json` |
| `updater check --json` | Check available updates | `updater check --json` |
| `updater check --json -v` | Check with release notes | `updater check --json -v` |
| `updater outdated --json` | List only outdated apps | `updater outdated --json` |
| `updater update <app>` | Update specific app | `updater update Firefox` |
| `updater update --all` | Update all apps | `updater update --all` |
| `updater update --auto` | Unattended update (skip major, pinned, manual) | `updater update --auto` |
| `updater update --all --dry-run --json` | Plan updates without changes | `updater update --all --dry-run --json` |
| `updater install <name>` | Install app via Homebrew Cask | `updater install visual-studio-code` |
| `updater upgrade --json` | Self-update updater | `updater upgrade --json` |
| `updater pin <app>` | Pin app to prevent updates | `updater pin Photoshop` |
| `updater unpin <app>` | Unpin app | `updater unpin Photoshop` |
| `updater rollback <app>` | Restore previous version from backup | `updater rollback Firefox` |
| `updater rollback --all --json` | Roll back all restorable apps | `updater rollback --all --json` |
| `updater cleanup --json` | Find unused apps | `updater cleanup --days 90 --json` |
| `updater cleanup --delete --json` | Move unused apps to Trash | `updater cleanup --days 90 --delete --json` |
| `updater history --json` | Show update history | `updater history --json` |
| `updater schedule --json` | Schedule periodic checks | `updater schedule --interval 24 --json` |
| `updater schedule --remove --json` | Remove scheduled checks | `updater schedule --remove --json` |
| `updater menubar --json` | Install/remove menu bar agent | `updater menubar --remove --json` |
| `updater config export --json` | Export config as JSON | `updater config export --json` |
| `updater config import <file> --json` | Import config and return status | `updater config import config.yaml --json` |
| `updater ui --json` | Machine-readable UI command response | `updater ui --json` |

## JSON + Agent Mode

- All updater commands accept `--json`.
- In Codex or Claude Code environments, updater auto-enables JSON output.
- Override auto-detection with:
  - `UPDATER_AGENT_MODE=1` to force JSON mode
  - `UPDATER_AGENT_MODE=0` to force human-readable mode
- `updater update --json` is for dry-run planning and requires `--dry-run`.

## Output Handling Notes

- `check --json` status values: `ok`, `update_available`, `major_update`, `pinned`, `error`.
- `install --json` can return `installed`, `no_match`, or `multiple_matches`.
- `ui --json` returns a structured `unsupported` response instead of launching TUI.

## Safety Guidelines

- **Plan before apply**: Use `update --dry-run --json` before applying updates.
- **Confirm destructive actions**: Require confirmation before `cleanup --delete` and broad update/rollback operations.
- **Pin critical apps**: Use `pin`/`policy manual` for version-sensitive software.
- **Rollback available**: `rollback` restores from pre-update backups when present.

## TUI Mode

Launch `updater` (or `updater ui`) without JSON mode for interactive TUI management.
