---
name: netwhiz
description: This skill should be used when the user asks to "diagnose network issues", "check WiFi signal", "change DNS server", "run speed test", "trace route", "scan local network", "check IP address", "flush DNS", "find network devices", or mentions netwhiz, network diagnostics, WiFi troubleshooting, or DNS configuration.
version: 1.0.0
---

# netwhiz — Network Diagnostics

## Overview

netwhiz is a network diagnostics Swiss-army-knife for macOS. It combines IP info, WiFi analysis, DNS management, ping, traceroute, speed testing, and LAN scanning into one tool.

## Availability

```bash
command -v netwhiz >/dev/null || echo "NOT INSTALLED"
```

If not installed: `brew install lu-zhengda/tap/netwhiz`

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `netwhiz info` | Network overview (IP, gateway, DNS, interface, public IP) | `netwhiz info` |
| `netwhiz wifi` | WiFi details (SSID, channel, RSSI, SNR, security) | `netwhiz wifi` |
| `netwhiz wifi scan` | Scan nearby WiFi networks with signal bars | `netwhiz wifi scan` |
| `netwhiz dns` | Show current DNS servers and presets | `netwhiz dns` |
| `netwhiz dns set <server>` | Set DNS (cloudflare, google, or custom IP) | `netwhiz dns set cloudflare` |
| `netwhiz dns flush` | Flush DNS cache | `netwhiz dns flush` |
| `netwhiz ping <host>` | Enhanced ping with stats and RTT bars | `netwhiz ping 8.8.8.8 -c 10` |
| `netwhiz trace <host>` | Traceroute with per-hop RTT | `netwhiz trace example.com` |
| `netwhiz speed` | Download speed test via Cloudflare | `netwhiz speed` |
| `netwhiz scan` | ARP scan to discover LAN devices | `netwhiz scan` |

## DNS Presets

Use preset names with `dns set` instead of raw IPs:

| Preset | Servers |
|--------|---------|
| `cloudflare` | 1.1.1.1, 1.0.0.1 |
| `google` | 8.8.8.8, 8.8.4.4 |
| `quad9` | 9.9.9.9, 149.112.112.112 |

## Diagnostic Workflow

For network troubleshooting, follow this order:

1. `netwhiz info` — get network overview and identify interface
2. `netwhiz wifi` — check signal quality (if wireless)
3. `netwhiz dns` — verify DNS configuration
4. `netwhiz ping <target>` — test connectivity and latency
5. `netwhiz trace <target>` — identify where packets are dropping
6. `netwhiz speed` — measure throughput

## TUI Mode

Launch `netwhiz` without arguments for an interactive network dashboard.
