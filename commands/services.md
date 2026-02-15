---
description: Diagnose broken launch agents and daemons
allowed-tools: Bash(lanchr:*)
---

Diagnose launch agent and daemon health using lanchr.

!`lanchr doctor 2>&1 || echo "lanchr not installed — brew install lu-zhengda/tap/lanchr"`

Analyze the output above. Report broken plists, orphaned agents, and missing binaries. For each issue, explain the impact and recommend a fix (restart, disable, or remove). Offer to execute fixes.
