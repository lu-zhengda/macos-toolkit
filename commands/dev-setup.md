---
description: Configure macOS for development with optimal defaults
allowed-tools: Bash(*)
---

Set up this Mac for development. Execute ALL steps below in order.

## Step 1: Backup current settings

!`macfig backup 2>&1 || echo "macfig not installed — brew install lu-zhengda/tap/macfig"`

Note the backup file path — this is the rollback point.

## Step 2: Apply developer presets

!`macfig preset dev-setup 2>&1`
!`macfig preset keyboard-fast 2>&1`
!`macfig preset finder-dev 2>&1`

## Step 3: Check development tools for updates

!`updater check 2>&1 || echo "updater not installed — brew install lu-zhengda/tap/updater"`

## Step 4: Review background services

!`lanchr list --no-apple 2>&1 || echo "lanchr not installed — brew install lu-zhengda/tap/lanchr"`

## Present Results

Summarize what changed:
- Which presets were applied and their effects
- Backup location for rollback (`macfig restore <path>`)
- Any outdated dev tools to update
- Third-party services to be aware of
