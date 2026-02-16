---
name: macctl
description: This skill should be used when the user asks to "check battery status", "check CPU temperature", "set display brightness", "change screen resolution", "toggle Night Shift", "switch audio output", "set volume", "mute audio", "enable Do Not Disturb", "enable focus mode", "apply deep work preset", "set up for a meeting", "set up for a presentation", "save battery", "check thermal pressure", "what's preventing sleep", "list displays", "list audio devices", "check SSD health", "disk wear", "battery trends", "wake sleep history", "power events", "disk I/O", "SSD wear level", or mentions macctl, power management, display settings, audio routing, focus mode, environment presets, thermal monitoring, Do Not Disturb, disk health, or power events.
version: 1.2.0
allowed-tools: Bash(macctl:*)
---

# macctl — macOS Environment Controller

Show power and environment status:

!`macctl power status 2>&1 || echo "macctl not installed — brew install lu-zhengda/tap/macctl"`

Analyze the output above. Report battery level, charging state, and health. Offer to check thermals with `macctl power thermal`, adjust display with `macctl display`, or apply a preset with `macctl preset`.

## Commands — Power

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl power status` | Battery %, health, cycles, charging, time remaining | `macctl power status` |
| `macctl power health` | Design vs max capacity, condition, age | `macctl power health` |
| `macctl power thermal` | CPU/GPU temp, thermal pressure, fans | `macctl power thermal` |
| `macctl power assertions` | What is preventing sleep | `macctl power assertions` |
| `macctl power hogs` | Top energy consumers | `macctl power hogs` |
| `macctl power history` | Battery/thermal trends from persistent store | `macctl power history --last 7d` |
| `macctl power record` | Capture power snapshot (for lanchr agent periodic recording) | `macctl power record` |

## Commands — Disk

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl disk status` | SSD health: model, wear level, SMART status | `macctl disk status` |
| `macctl disk io` | Current read/write MB/s and IOPS | `macctl disk io` |
| `macctl disk history` | SSD wear trends over time | `macctl disk history --last 30d` |
| `macctl disk record` | Capture disk health snapshot (for lanchr agent) | `macctl disk record` |

## Commands — Events

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl events` | Power-related events: wake, sleep, lid, thermal, power source | `macctl events` |
| `macctl events --last <duration>` | Filter events by time window | `macctl events --last 1h` |
| `macctl events --type <type>` | Filter by event type | `macctl events --type wake` |
| `macctl events --json` | JSON output for scripting | `macctl events --json` |

Event types: **wake**, **sleep**, **lid**, **thermal**, **power_source**

> Events are automatically deduplicated — consecutive same-type events within 30s are collapsed with a count.

## Commands — Display

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl display list` | Connected displays with resolution, refresh, profile | `macctl display list` |
| `macctl display resolution` | Get current resolution | `macctl display resolution` |
| `macctl display resolution <WxH>` | Set resolution | `macctl display resolution 1920x1080` |
| `macctl display brightness` | Get current brightness | `macctl display brightness` |
| `macctl display brightness <0-100>` | Set brightness | `macctl display brightness 70` |
| `macctl display nightshift [on\|off]` | Toggle Night Shift | `macctl display nightshift on` |
| `macctl display truetone [on\|off]` | Toggle True Tone | `macctl display truetone on` |

## Commands — Audio

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl audio list` | List input and output devices | `macctl audio list` |
| `macctl audio output` | Get current output device | `macctl audio output` |
| `macctl audio output <device>` | Switch audio output | `macctl audio output "AirPods Pro"` |
| `macctl audio input` | Get current input device | `macctl audio input` |
| `macctl audio input <device>` | Switch audio input | `macctl audio input "MacBook Pro Microphone"` |
| `macctl audio volume` | Get current volume | `macctl audio volume` |
| `macctl audio volume <0-100>` | Set volume | `macctl audio volume 50` |
| `macctl audio mute [on\|off\|toggle]` | Control mute | `macctl audio mute toggle` |
| `macctl audio test` | Play test tone and mic check | `macctl audio test` |

## Commands — Focus

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl focus status` | Current focus mode and DnD state | `macctl focus status` |
| `macctl focus on [mode]` | Enable a focus mode | `macctl focus on "Do Not Disturb"` |
| `macctl focus off` | Disable current focus mode | `macctl focus off` |
| `macctl focus list` | List configured focus modes | `macctl focus list` |

## Commands — Presets

| Command | Purpose | Example |
|---------|---------|---------|
| `macctl preset <name>` | Apply a compound preset | `macctl preset deep-work` |

## Compound Presets

Presets span all four domains in a single command:

| Preset | Display | Audio | Focus | Notes |
|--------|---------|-------|-------|-------|
| `deep-work` | Dim to 50% | Mute | DnD on | Minimize distractions |
| `meeting` | No change | AirPods + unmute | DnD on (allow calls) | Ready for calls |
| `present` | Mirror 1080p, 100% brightness | Speakers | DnD on | Presentation mode |
| `chill` | Dim to 40%, Night Shift on | Volume 30% | DnD off | Relaxed evening |
| `battery-saver` | Dim to 30% | Mute | No change | Extend battery life |

Example workflow:

```bash
# Start a focused coding session
macctl preset deep-work

# Switch to a meeting
macctl preset meeting

# Done for the day
macctl preset chill
```

## JSON Output

Add `--json` to any read command for machine-readable output:

```bash
macctl power status --json
macctl power thermal --json
macctl power history --json
macctl disk status --json
macctl disk io --json
macctl disk history --json
macctl events --json
macctl display list --json
macctl audio list --json
macctl focus status --json
macctl focus list --json
```

## Safety Guidelines

- **Presets are reversible**: Apply another preset or manually adjust individual settings
- **Brightness changes are immediate**: Display brightness adjusts instantly
- **Audio routing affects all apps**: Switching output device redirects all audio
- **Focus mode respects system config**: Focus presets use modes configured in System Settings
- **Resolution changes may flash**: Screen may briefly flash when changing resolution

## TUI Mode

Launch `macctl` without arguments for an interactive environment dashboard.
