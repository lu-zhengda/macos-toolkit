---
description: Run a full macOS system health check
allowed-tools: Bash(*)
---

Run a comprehensive macOS system health check. Execute ALL of the following diagnostic steps, then present a unified health report.

## Step 1: Disk space

!`macbroom scan 2>&1 || echo "macbroom not installed — brew install lu-zhengda/tap/macbroom"`

## Step 2: App updates

!`updater check 2>&1 || echo "updater not installed — brew install lu-zhengda/tap/updater"`

## Step 3: Launch agent health

!`lanchr doctor 2>&1 || echo "lanchr not installed — brew install lu-zhengda/tap/lanchr"`

## Step 4: Port audit

!`whport list 2>&1 || echo "whport not installed — brew install lu-zhengda/tap/whport"`

## Present Results

Combine all output above into a health report with these sections:
- **Disk**: reclaimable space and top categories
- **Apps**: outdated apps with versions
- **Services**: broken plists, orphaned agents
- **Ports**: unexpected listeners on common ports (80, 443, 3000, 5432, 8080)

End with actionable recommendations and offer to fix issues.
