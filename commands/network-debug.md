---
description: Diagnose network connectivity and performance issues
allowed-tools: Bash(*)
argument-hint: [target-host]
---

Diagnose network issues. Target host: $ARGUMENTS (default: 8.8.8.8 if none provided).

## Step 1: Network overview

!`netwhiz info 2>&1 || echo "netwhiz not installed — brew install lu-zhengda/tap/netwhiz"`

## Step 2: WiFi signal quality

!`netwhiz wifi 2>&1`

## Step 3: DNS configuration

!`netwhiz dns 2>&1`

## Step 4: Connectivity test

!`netwhiz ping 8.8.8.8 -c 5 2>&1`

## Step 5: Port conflicts

!`whport list 2>&1 || echo "whport not installed — brew install lu-zhengda/tap/whport"`

## Present Diagnosis

Based on the output above, summarize:
- **Connection status**: Connected / No IP / No gateway
- **Signal quality**: RSSI level and assessment
- **DNS**: Working / Slow / Failing
- **Latency**: Low / High / Packet loss
- **Port conflicts**: Any unexpected listeners

If issues found, provide specific fix commands. If target host was specified ($ARGUMENTS), also run `netwhiz trace <target>` and `netwhiz ping <target> -c 5`.
