---
description: Scan Mac for reclaimable disk space
allowed-tools: Bash(macbroom:*)
---

Scan this Mac for junk files and reclaimable space using macbroom.

!`macbroom scan 2>&1 || echo "macbroom not installed — brew install lu-zhengda/tap/macbroom"`

Analyze the scan results above. Present a summary of reclaimable space by category. Then ask which categories to clean, or if the user wants to clean all. Use `macbroom clean --dry-run` first to preview, then `macbroom clean` to execute.
