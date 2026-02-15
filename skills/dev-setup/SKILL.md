---
name: dev-setup
description: This skill should be used when the user asks to "set up Mac for development", "configure developer defaults", "optimize dev environment", "new Mac setup", "developer settings", "show hidden files", "faster keyboard repeat", or mentions dev setup, developer configuration, or new Mac for coding.
version: 1.0.0
---

# Developer Environment Setup — Cross-Tool Workflow

## Overview

Configures macOS for development by applying developer-friendly defaults, checking tool freshness, and reviewing background services. Uses macfig (defaults), updater (apps), and lanchr (services).

## Prerequisites

```bash
for tool in macfig updater lanchr; do
  command -v "$tool" >/dev/null && echo "$tool: OK" || echo "$tool: MISSING — brew install lu-zhengda/tap/$tool"
done
```

## Workflow

### Step 1: Backup current settings

```bash
macfig backup
```

**Important**: Always backup before changing defaults. Note the backup file path for potential rollback.

### Step 2: Apply developer presets

```bash
macfig preset dev-setup
macfig preset keyboard-fast
macfig preset finder-dev
```

These configure:
- **dev-setup**: Developer-friendly Dock and system behavior
- **keyboard-fast**: Fast key repeat rate, short initial delay
- **finder-dev**: Show hidden files, all file extensions, path bar, status bar

### Step 3: Check development tools

```bash
updater check
```

Review which dev tools have updates available. Recommend updating.

### Step 4: Review background services

```bash
lanchr list --no-apple
```

List third-party services. Flag any that might conflict with development (e.g., services using common dev ports).

### Step 5: Present summary

Report what changed:
- Defaults applied (list each preset and its effects)
- Backup location for rollback (`macfig restore <path>`)
- Outdated dev tools to update
- Third-party services to be aware of
