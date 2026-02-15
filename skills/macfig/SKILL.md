---
name: macfig
description: This skill should be used when the user asks to "change macOS defaults", "configure system preferences", "set hidden settings", "adjust dock speed", "show Finder hidden files", "set keyboard repeat rate", "apply system preset", "backup macOS settings", "restore defaults", "search macOS preferences", "diff settings against backup", "export macOS defaults", "import macOS defaults", "watch a defaults key", "monitor preference changes", or mentions macfig, defaults write, macOS hidden settings, defaults export, or defaults diff.
version: 1.1.0
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
| `macfig diff <backup>` | Compare current state against a backup | `macfig diff ~/.config/macfig/backup.json` |
| `macfig export [file]` | Export all tracked defaults to JSON | `macfig export defaults.json` |
| `macfig import <file>` | Import and apply defaults from JSON | `macfig import defaults.json` |
| `macfig watch <domain> <key>` | Monitor a defaults key for changes | `macfig watch com.apple.dock autohide` |

## Diff and Migration

Compare current settings against a backup to see what changed:

```bash
macfig backup
# ... make changes ...
macfig diff ~/.config/macfig/backup.json
```

Export and import defaults for machine migration:

```bash
# Export from old machine
macfig export my-defaults.json

# Import on new machine
macfig import my-defaults.json
```

## Watch Mode

Monitor a specific defaults key for live changes:

```bash
macfig watch com.apple.dock autohide
```

Useful for debugging preference changes made by apps or System Settings.

## JSON Output

Add `--json` to any read command for machine-readable output:

```bash
macfig list --json
macfig list dock --json
macfig get com.apple.dock autohide --json
macfig search animation --json
macfig diff backup.json --json
macfig export --json
```

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
- **Diff before import**: Run `macfig diff` to preview changes before applying an import

## TUI Mode

Launch `macfig` without arguments for interactive mode — browse categories, toggle settings, and preview changes.
