---
name: network-debug
description: This skill should be used when the user asks to "debug network", "fix network issues", "why is my internet slow", "diagnose connectivity", "troubleshoot DNS", "check network connection", "fix WiFi", or mentions network debugging, connection problems, slow internet, or network troubleshooting.
version: 1.0.0
---

# Network Debugging — Cross-Tool Workflow

## Overview

Systematic network troubleshooting combining netwhiz (diagnostics) and whport (port conflicts) to identify and fix connectivity issues.

## Prerequisites

```bash
for tool in netwhiz whport; do
  command -v "$tool" >/dev/null && echo "$tool: OK" || echo "$tool: MISSING — brew install lu-zhengda/tap/$tool"
done
```

## Workflow

### Step 1: Network overview

```bash
netwhiz info
```

Check: IP address assigned, gateway reachable, DNS configured, public IP resolving. If no IP or gateway, the issue is at Layer 2/3.

### Step 2: WiFi signal quality (if wireless)

```bash
netwhiz wifi
```

Check RSSI (signal strength) and SNR (signal-to-noise ratio):
- RSSI > -50 dBm: Excellent
- RSSI -50 to -70: Good
- RSSI -70 to -80: Weak (may cause issues)
- RSSI < -80: Very weak (likely causing problems)

If signal is weak, recommend moving closer to AP or checking for interference with `netwhiz wifi scan`.

### Step 3: DNS check

```bash
netwhiz dns
```

Verify DNS servers are responsive. If using ISP DNS and experiencing slow resolution, recommend switching:

```bash
netwhiz dns set cloudflare
```

### Step 4: Connectivity test

```bash
netwhiz ping 8.8.8.8 -c 5
netwhiz ping google.com -c 5
```

- First ping tests raw IP connectivity (bypasses DNS)
- Second ping tests DNS resolution + connectivity
- If first works but second fails: DNS issue
- If both fail: routing or connectivity issue

### Step 5: Port conflict check

```bash
whport list
```

Check if any local process is interfering with network traffic (e.g., VPN, proxy, firewall on unexpected ports).

### Step 6: Route analysis (if needed)

```bash
netwhiz trace <target>
```

Only run if ping shows issues. Identifies where in the path packets are being dropped or delayed.

### Step 7: Present diagnosis

Summarize findings:
- **Connection status**: Connected / No IP / No gateway
- **Signal quality**: Strong / Weak / N/A (wired)
- **DNS**: Working / Slow / Failing
- **Latency**: Low / High / Packet loss
- **Port conflicts**: None / List conflicts
- **Recommended fixes**: Specific actions based on findings
