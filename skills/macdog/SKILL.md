---
name: macdog
description: This skill should be used when the user asks to "run a security audit", "check firewall status", "enable the firewall", "review privacy permissions", "check what apps have camera access", "list login items", "harden my Mac", "check security settings", "revoke app permissions", or mentions macdog, macOS security, privacy audit, TCC permissions, or system hardening.
version: 1.0.0
allowed-tools: Bash(macdog:*)
---

# macdog — macOS Security & Privacy Suite

Run a full security audit:

!`macdog audit 2>&1 || echo "macdog not installed — brew install lu-zhengda/tap/macdog"`

Analyze the audit results above. Explain the overall security grade and highlight any failing checks. For each issue, explain the risk and offer to fix it with `macdog harden` or specific commands.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `macdog audit` | Full security audit with letter grade (A-F) | `macdog audit` |
| `macdog firewall` | Show firewall status and rules | `macdog firewall` |
| `macdog firewall enable` | Enable the firewall | `macdog firewall enable` |
| `macdog firewall disable` | Disable the firewall | `macdog firewall disable` |
| `macdog firewall allow <app>` | Allow an app through firewall | `macdog firewall allow /Applications/Slack.app` |
| `macdog firewall block <app>` | Block an app in firewall | `macdog firewall block /Applications/Suspicious.app` |
| `macdog privacy` | List TCC privacy permissions | `macdog privacy` |
| `macdog privacy revoke <app> <service>` | Revoke a permission | `macdog privacy revoke com.app.name Camera` |
| `macdog login` | List login items and launch agents | `macdog login` |
| `macdog login remove <item>` | Remove a login item | `macdog login remove "Some App"` |
| `macdog harden` | Apply security hardening preset | `macdog harden` |
| `macdog harden --dry-run` | Preview hardening changes | `macdog harden --dry-run` |

## Security Audit Checks

The audit evaluates: FileVault encryption, firewall status, Gatekeeper, SIP, remote login, screen lock, sharing services, and more. Each check gets a pass/fail with explanation.

## Safety Guidelines

- **Always `--dry-run` before `harden`**: Preview what changes will be applied
- **Audit first**: Run `macdog audit` to understand your current posture before changing anything
- **Firewall changes require sudo**: Enabling/disabling firewall needs admin privileges
- **Privacy revocations are immediate**: Apps lose access as soon as permissions are revoked

## TUI Mode

Launch `macdog` without arguments for an interactive security dashboard.
