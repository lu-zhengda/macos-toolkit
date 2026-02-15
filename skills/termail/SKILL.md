---
name: termail
description: This skill should be used when the user asks to "check my email", "read my inbox", "send an email", "compose an email", "reply to email", "forward email", "search emails", "archive email", "star email", "trash email", "list email threads", "sync emails", "add Gmail account", "switch email account", "list labels", "mark email as read", "mark email as unread", or mentions termail, Gmail, inbox management, email client, or email search.
version: 1.0.0
allowed-tools: Bash(termail:*)
---

# termail — Terminal Email Client

Sync and list recent email threads:

!`termail sync 2>&1 && termail list --limit 5 2>&1 || echo "termail not installed — brew install lu-zhengda/tap/termail"`

Analyze the output above. Summarize unread threads (marked with `*`), highlight anything that looks urgent or time-sensitive, and offer to read specific threads with `termail read <thread-id>`, reply, compose, or search. If sync fails with a credentials error, guide the user through `termail account add`.

## Commands — Reading

| Command | Purpose | Example |
|---------|---------|---------|
| `termail list` | List email threads | `termail list --limit 20` |
| `termail list --label <label>` | Filter by label | `termail list --label SENT --limit 10` |
| `termail list --account <email>` | List from a specific account | `termail list --account work@gmail.com` |
| `termail read <thread-id>` | Read a full thread | `termail read 19c5d4f2ea3c4478` |
| `termail search "<query>"` | Full-text search (FTS5) | `termail search "quarterly report"` |
| `termail search --account <email>` | Search a specific account | `termail search "invoice" --account work@gmail.com` |
| `termail labels` | List all labels | `termail labels` |

## Commands — Composing

| Command | Purpose | Example |
|---------|---------|---------|
| `termail compose` | Send a new email | `termail compose --to user@example.com --subject "Hi" --body "Hello"` |
| `termail reply <message-id>` | Reply to a message | `termail reply <message-id> --body "Thanks!"` |
| `termail reply <message-id> --all` | Reply all | `termail reply <message-id> --body "Thanks!" --all` |
| `termail forward <message-id>` | Forward a message | `termail forward <message-id> --to other@example.com` |

## Commands — Organizing

| Command | Purpose | Example |
|---------|---------|---------|
| `termail archive <message-id>` | Archive (remove from Inbox) | `termail archive <message-id>` |
| `termail trash <message-id>` | Move to trash | `termail trash <message-id>` |
| `termail star <message-id>` | Star a message | `termail star <message-id>` |
| `termail star <message-id> --remove` | Unstar a message | `termail star <message-id> --remove` |
| `termail mark-read <message-id>` | Mark as read | `termail mark-read <message-id>` |
| `termail mark-read <message-id> --unread` | Mark as unread | `termail mark-read <message-id> --unread` |
| `termail label-modify <id>` | Add/remove labels | `termail label-modify <id> --add STARRED --remove INBOX` |

## Commands — Accounts & Sync

| Command | Purpose | Example |
|---------|---------|---------|
| `termail account list` | List configured accounts | `termail account list` |
| `termail account add` | Add a Gmail account (opens browser for OAuth) | `termail account add` |
| `termail account remove <email>` | Remove an account | `termail account remove user@gmail.com` |
| `termail sync` | Sync emails for all accounts | `termail sync` |
| `termail sync --account <email>` | Sync a specific account | `termail sync --account user@gmail.com` |

## Multi-Account

termail supports multiple Gmail accounts. Use `--account <email>` with `list`, `search`, and `sync` to target a specific account. Without the flag, commands use the default account from config.

Switch accounts in the TUI with `@`.

## Search

Full-text search powered by SQLite FTS5:

```bash
termail search "quarterly report"
termail search "from:alice subject:meeting"
```

## Safety Guidelines

- **Confirm before sending**: Always verify recipients and content before `compose`, `reply`, or `forward`
- **Archive over trash**: Prefer `archive` to keep emails accessible; use `trash` only for unwanted mail
- **Sync before reading**: Run `termail sync` to fetch the latest emails before reading or searching
- **OAuth tokens are secure**: Tokens are stored in the OS keyring (macOS Keychain), never in plain files

## TUI Mode

Launch `termail` without arguments for an interactive email dashboard with keyboard navigation.
