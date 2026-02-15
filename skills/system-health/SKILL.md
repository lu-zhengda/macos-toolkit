---
name: system-health
description: This skill should be used when the user asks to "check system health", "run system health check", "how is my Mac doing", "check macOS status", "system maintenance", "overall system check", or mentions system health, Mac health check, or system diagnostics.
version: 1.0.0
---

# System Health Check — Cross-Tool Workflow

## Overview

A comprehensive macOS health check combining macbroom (disk), updater (apps), lanchr (services), and whport (ports) to produce an actionable system status report.

## Prerequisites

This workflow uses multiple tools. Verify availability:

```bash
for tool in macbroom updater lanchr whport; do
  command -v "$tool" >/dev/null && echo "$tool: OK" || echo "$tool: MISSING — brew install lu-zhengda/tap/$tool"
done
```

Proceed with available tools, skip checks for missing ones.

## Workflow

Execute these steps in order, presenting results as a unified report:

### Step 1: Disk space check

```bash
macbroom scan
```

Report: total reclaimable space, top categories by size.

### Step 2: App update check

```bash
updater check
```

Report: number of outdated apps, list with current vs available versions.

### Step 3: Launch agent health

```bash
lanchr doctor
```

Report: broken plists, orphaned agents, missing binaries.

### Step 4: Port audit

```bash
whport list
```

Report: number of listening ports, flag any unexpected listeners on common ports (80, 443, 8080, 3000, 5432, 6379, 27017).

### Step 5: Present summary

Combine all results into a health report with sections:
- **Disk**: reclaimable space and recommendation
- **Apps**: outdated count and recommendation to update
- **Services**: broken service count and fixes
- **Ports**: unexpected listeners and what to investigate

Offer to fix issues: clean disk (`macbroom clean`), update apps (`updater update --all`), restart broken services (`lanchr restart`).
