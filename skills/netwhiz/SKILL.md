---
name: netwhiz
description: This skill should be used when the user asks to "diagnose network issues", "check WiFi signal", "change DNS server", "run speed test", "trace route", "scan local network", "check IP address", "flush DNS", "find network devices", "speed test history", "monitor WiFi signal", "benchmark DNS", "network diagnosis", "compare DNS providers", "connect VPN", "disconnect VPN", "VPN status", "network events", "WiFi disconnect history", "IP change log", or mentions netwhiz, network diagnostics, WiFi troubleshooting, DNS configuration, WiFi monitoring, DNS benchmark, network diagnosis, VPN management, or network events.
version: 1.3.0
allowed-tools: Bash(netwhiz:*)
argument-hint: [target-host]
---

# netwhiz — Network Diagnostics

Show network overview:

!`netwhiz info 2>&1 || echo "netwhiz not installed — brew install lu-zhengda/tap/netwhiz"`

Analyze the output above. Summarize connection status, IP, gateway, DNS, and public IP. If anything looks wrong, run additional diagnostics: `netwhiz wifi` for signal quality, `netwhiz dns` for DNS health, `netwhiz ping <host>` for connectivity. If a target host was provided ($ARGUMENTS), test connectivity to it.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `netwhiz info` | Network overview (IP, gateway, DNS, interface, public IP) | `netwhiz info` |
| `netwhiz wifi` | WiFi details (SSID, channel, RSSI, SNR, security) | `netwhiz wifi` |
| `netwhiz wifi scan` | Scan nearby WiFi networks with signal bars | `netwhiz wifi scan` |
| `netwhiz wifi --monitor` | Live WiFi signal monitoring with alerts | `netwhiz wifi --monitor --min-rssi -70` |
| `netwhiz dns` | Show current DNS servers and presets | `netwhiz dns` |
| `netwhiz dns set <server>` | Set DNS (cloudflare, google, or custom IP) | `netwhiz dns set cloudflare` |
| `netwhiz dns flush` | Flush DNS cache | `netwhiz dns flush` |
| `netwhiz dns benchmark` | Compare DNS providers by response time | `netwhiz dns benchmark` |
| `netwhiz ping <host>` | Enhanced ping with stats and RTT bars | `netwhiz ping 8.8.8.8 -c 10` |
| `netwhiz trace <host>` | Traceroute with per-hop RTT | `netwhiz trace example.com` |
| `netwhiz speed` | Download speed test via Cloudflare | `netwhiz speed` |
| `netwhiz speed --history` | Show speed test history and trends | `netwhiz speed --history` |
| `netwhiz scan` | ARP scan to discover LAN devices | `netwhiz scan` |
| `netwhiz diagnose` | Comprehensive network diagnosis | `netwhiz diagnose` |
| `netwhiz vpn` | List configured VPN connections with status | `netwhiz vpn` |
| `netwhiz vpn connect <name>` | Connect to VPN | `netwhiz vpn connect "Office VPN"` |
| `netwhiz vpn disconnect [name]` | Disconnect VPN (specific or all) | `netwhiz vpn disconnect` |
| `netwhiz vpn status [name]` | Detailed VPN connection info | `netwhiz vpn status "Office VPN"` |
| `netwhiz events` | Network events: wifi, IP change, DNS change, connection drop, VPN | `netwhiz events` |
| `netwhiz events --last <duration>` | Filter events by time window | `netwhiz events --last 1h` |
| `netwhiz events --type <type>` | Filter by event type | `netwhiz events --type connection_drop` |
| `netwhiz events --json` | JSON output for scripting | `netwhiz events --json` |

## VPN Management

List, connect, and disconnect VPN connections:

```bash
# List all configured VPNs and their status
netwhiz vpn

# Connect to a VPN
netwhiz vpn connect "Office VPN"

# Check detailed VPN status
netwhiz vpn status "Office VPN"

# Disconnect a specific VPN
netwhiz vpn disconnect "Office VPN"

# Disconnect all active VPNs
netwhiz vpn disconnect
```

## Network Events

View network change events from the system log:

```bash
# Show recent network events
netwhiz events

# Filter to last hour
netwhiz events --last 1h

# Filter by event type
netwhiz events --type wifi_connect
netwhiz events --type wifi_disconnect
netwhiz events --type ip_change
netwhiz events --type dns_change
netwhiz events --type path_satisfied
netwhiz events --type path_unsatisfied
netwhiz events --type connection_drop
netwhiz events --type vpn

# JSON output for scripting
netwhiz events --json
```

Event types:
- **wifi_connect** — WiFi network associations
- **wifi_disconnect** — WiFi disconnections and drops
- **ip_change** — IP address changes (DHCP renewal, manual)
- **dns_change** — DNS server configuration changes
- **path_satisfied** — Network path becomes available
- **path_unsatisfied** — Network path becomes unavailable
- **connection_drop** — Active connection drops
- **vpn** — VPN connect/disconnect events

> Events are automatically deduplicated — consecutive same-type events within 30s are collapsed with a count.

## Auto-Diagnose

Run a comprehensive network diagnosis that chains info, DNS, WiFi, and ping checks:

```bash
netwhiz diagnose
```

Outputs a structured diagnosis with identified issues and recommendations.

## WiFi Signal Monitoring

Monitor WiFi signal strength and alert on degradation:

```bash
# Alert when RSSI drops below -70 dBm
netwhiz wifi --monitor --min-rssi -70
```

## DNS Benchmark

Compare all DNS presets and recommend the fastest:

```bash
netwhiz dns benchmark
```

Tests Cloudflare, Google, Quad9, and current DNS. Shows response times and recommends the best option.

## Speed Test History

Track speed test results over time:

```bash
# Run a speed test (result is saved automatically)
netwhiz speed

# View history and trends
netwhiz speed --history
```

## JSON Output

Add `--json` to any read command for machine-readable output:

```bash
netwhiz info --json
netwhiz wifi --json
netwhiz dns --json
netwhiz speed --json
netwhiz speed --history --json
netwhiz scan --json
netwhiz ping 8.8.8.8 --json
netwhiz trace example.com --json
netwhiz diagnose --json
netwhiz dns benchmark --json
netwhiz vpn --json
netwhiz vpn status "Office VPN" --json
netwhiz events --json
```

## DNS Presets

| Preset | Servers |
|--------|---------|
| `cloudflare` | 1.1.1.1, 1.0.0.1 |
| `google` | 8.8.8.8, 8.8.4.4 |
| `quad9` | 9.9.9.9, 149.112.112.112 |

## Diagnostic Workflow

For network troubleshooting, follow this order:

1. `netwhiz diagnose` — automated comprehensive check
2. `netwhiz info` — get network overview and identify interface
3. `netwhiz wifi` — check signal quality (if wireless)
4. `netwhiz dns` — verify DNS configuration
5. `netwhiz dns benchmark` — find the fastest DNS provider
6. `netwhiz ping <target>` — test connectivity and latency
7. `netwhiz trace <target>` — identify where packets are dropping
8. `netwhiz speed` — measure throughput

## TUI Mode

Launch `netwhiz` without arguments for an interactive network dashboard.
