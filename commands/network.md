---
description: Show network overview and connection status
allowed-tools: Bash(netwhiz:*)
argument-hint: [target-host]
---

Show network overview using netwhiz.

!`netwhiz info 2>&1 || echo "netwhiz not installed — brew install lu-zhengda/tap/netwhiz"`

Analyze the output above. Summarize connection status, IP, gateway, DNS, and public IP. If anything looks wrong, run additional diagnostics: `netwhiz wifi` for signal quality, `netwhiz dns` for DNS health, `netwhiz ping <host>` for connectivity. If a target host was provided ($ARGUMENTS), test connectivity to it.
