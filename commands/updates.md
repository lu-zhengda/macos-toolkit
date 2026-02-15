---
description: Check for available app updates
allowed-tools: Bash(updater:*)
---

Check this Mac for outdated applications using updater.

!`updater check 2>&1 || echo "updater not installed — brew install lu-zhengda/tap/updater"`

Analyze the output above. List outdated apps with current vs available versions. Ask which apps to update, or offer `updater update --all` to update everything.
