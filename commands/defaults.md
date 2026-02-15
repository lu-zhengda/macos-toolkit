---
description: Browse and manage macOS hidden defaults
allowed-tools: Bash(macfig:*)
argument-hint: [category]
---

Browse macOS hidden defaults using macfig.

!`macfig list $ARGUMENTS 2>&1 || echo "macfig not installed — brew install lu-zhengda/tap/macfig"`

Analyze the output above. If showing categories, explain what each category controls. If showing settings in a category, explain each setting and its current value. Offer to change settings or apply presets (dock-speed, finder-dev, keyboard-fast, no-animations, privacy, dev-setup). Always recommend `macfig backup` before making changes.
