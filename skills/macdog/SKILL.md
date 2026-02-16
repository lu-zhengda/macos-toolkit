---
name: macdog
description: This skill should be used when the user asks to "run a security audit", "check firewall status", "enable the firewall", "review privacy permissions", "check what apps have camera access", "list login items", "harden my Mac", "check security settings", "revoke app permissions", "auto-fix security issues", "export firewall rules", "import firewall rules", "export privacy permissions", "monitor security score", "security events", "login attempts", "permission changes", "firewall blocks", "app install history", "TCC events", or mentions macdog, macOS security, privacy audit, TCC permissions, system hardening, firewall export, continuous security monitoring, or security event log.
version: 1.3.0
allowed-tools: Bash(macdog:*)
---

# macdog — macOS Security & Privacy Suite

Run a full security audit:

!`macdog audit 2>&1 || echo "macdog not installed — brew install lu-zhengda/tap/macdog"`

Analyze the audit results above. Explain the overall security grade and highlight any failing checks. For each issue, explain the risk and offer to fix it with `macdog audit --fix` or specific commands.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `macdog audit` | Full security audit with letter grade (A-F) | `macdog audit` |
| `macdog audit --fix` | Auto-apply recommended hardening for failing checks | `macdog audit --fix` |
| `macdog audit --watch` | Continuous monitoring, alert on score drop | `macdog audit --watch --min-score 70` |
| `macdog firewall` | Show firewall status and rules | `macdog firewall` |
| `macdog firewall enable` | Enable the firewall | `macdog firewall enable` |
| `macdog firewall disable` | Disable the firewall | `macdog firewall disable` |
| `macdog firewall allow <app>` | Allow an app through firewall | `macdog firewall allow /Applications/Slack.app` |
| `macdog firewall block <app>` | Block an app in firewall | `macdog firewall block /Applications/Suspicious.app` |
| `macdog firewall export [file]` | Export firewall rules to JSON | `macdog firewall export rules.json` |
| `macdog firewall import <file>` | Import firewall rules from JSON | `macdog firewall import rules.json` |
| `macdog privacy` | List TCC privacy permissions | `macdog privacy` |
| `macdog privacy revoke <app> <service>` | Revoke a permission | `macdog privacy revoke com.app.name Camera` |
| `macdog privacy export [file]` | Export TCC permissions snapshot | `macdog privacy export perms.json` |
| `macdog login` | List login items and launch agents | `macdog login` |
| `macdog login remove <item>` | Remove a login item | `macdog login remove "Some App"` |
| `macdog harden` | Apply security hardening preset | `macdog harden` |
| `macdog harden --dry-run` | Preview hardening changes | `macdog harden --dry-run` |
| `macdog events` | Security events: auth, tcc, gatekeeper, xprotect, firewall | `macdog events` |
| `macdog events --last <duration>` | Filter events by time window | `macdog events --last 1h` |
| `macdog events --type <type>` | Filter by event type | `macdog events --type tcc` |
| `macdog events --severity <level>` | Filter by minimum severity (info, warning, critical) | `macdog events --severity warning` |
| `macdog events --json` | JSON output for scripting | `macdog events --json` |

## Security Events

View security-related events from the system log:

```bash
# Show recent security events
macdog events

# Filter to last hour
macdog events --last 1h

# Filter by event type
macdog events --type auth
macdog events --type tcc
macdog events --type gatekeeper
macdog events --type xprotect
macdog events --type firewall

# Filter by minimum severity
macdog events --severity warning
macdog events --severity critical

# JSON output for scripting
macdog events --json
```

Event types:
- **auth** — Login attempts, sudo usage, authentication failures
- **tcc** — TCC permission grants and denials (camera, microphone, etc.)
- **gatekeeper** — Gatekeeper checks and notarization events
- **xprotect** — XProtect malware detection and remediation events
- **firewall** — Firewall blocks and connection denials

Severity levels: **info**, **warning**, **critical**

> Events are automatically deduplicated — consecutive same-type events within 30s are collapsed with a count.

## Auto-Fix Workflow

Automatically apply recommended fixes for failing audit checks:

```bash
# Preview what audit --fix would change
macdog audit --fix --dry-run

# Apply fixes (enables firewall, Gatekeeper, etc.)
macdog audit --fix
```

## Continuous Monitoring

Watch security posture and alert on score drops:

```bash
# Alert when score drops below 70
macdog audit --watch --min-score 70
```

Combine with `lanchr create --template monitor-security` for persistent monitoring.

## Firewall Rule Portability

Export and import firewall rules for backup or migration:

```bash
macdog firewall export rules.json
macdog firewall import rules.json
```

## Security Audit Checks

The audit evaluates: FileVault encryption, firewall status, Gatekeeper, SIP, remote login, screen lock, sharing services, and more. Each check gets a pass/fail with explanation.

## Safety Guidelines

- **Always `--dry-run` before `--fix`**: Preview what changes will be applied
- **Audit first**: Run `macdog audit` to understand your current posture before changing anything
- **Firewall changes require sudo**: Enabling/disabling firewall needs admin privileges
- **Privacy revocations are immediate**: Apps lose access as soon as permissions are revoked
- **Export before import**: Back up existing rules before importing new ones

## TUI Mode

Launch `macdog` without arguments for an interactive security dashboard.
