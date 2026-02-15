---
name: macfig
description: This skill should be used when the user asks to "change macOS defaults", "configure system preferences", "set hidden settings", "adjust dock speed", "show Finder hidden files", "set keyboard repeat rate", "apply system preset", "backup macOS settings", "restore defaults", "search macOS preferences", or mentions macfig, defaults write, or macOS hidden settings.
version: 1.0.0
allowed-tools: Bash(macfig:*)
argument-hint: [category]
---

# macfig — macOS Defaults Manager

Browse macOS hidden defaults:

!`macfig list $ARGUMENTS 2>&1 || echo "macfig not installed — brew install lu-zhengda/tap/macfig"`

Analyze the output above. If showing categories, explain what each controls. If showing settings, explain each and its current value. Offer to change settings or apply presets. Always recommend `macfig backup` before changes.

## Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `macfig list` | List preset categories | `macfig list` |
| `macfig list <category>` | Show settings in category with current values | `macfig list dock` |
| `macfig get <domain> <key>` | Read a specific defaults value | `macfig get com.apple.dock autohide` |
| `macfig set <domain> <key> <value>` | Write a defaults value | `macfig set com.apple.dock autohide true` |
| `macfig preset <name>` | Apply a named preset bundle | `macfig preset dev-setup` |
| `macfig backup` | Backup all tracked settings to JSON | `macfig backup` |
| `macfig restore <file>` | Restore from backup file | `macfig restore ~/.config/macfig/backup.json` |
| `macfig reset <domain> <key>` | Delete a key (restore macOS default) | `macfig reset com.apple.dock autohide` |
| `macfig search <query>` | Search keys across all domains | `macfig search animation` |

## Built-in Presets

| Preset | Effect |
|--------|--------|
| `dock-speed` | Faster Dock animations and autohide delay |
| `finder-dev` | Show hidden files, extensions, path bar, status bar |
| `keyboard-fast` | Faster key repeat rate and shorter delay |
| `no-animations` | Disable most macOS animations |
| `privacy` | Disable analytics, Siri suggestions, ad tracking |
| `dev-setup` | Combined developer-friendly defaults |

## Safety Guidelines

- **Always backup first**: Run `macfig backup` before applying presets or bulk changes
- **Use presets over raw set**: Presets handle type inference and process restart automatically
- **Use search to discover**: Run `macfig search <term>` instead of guessing domain/key pairs
- **Revert with restore**: If something breaks, `macfig restore <backup-file>` reverts all changes

## TUI Mode

Launch `macfig` without arguments for interactive mode — browse categories, toggle settings, and preview changes.
