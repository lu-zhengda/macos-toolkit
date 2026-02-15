# macos-toolkit

A Claude Code plugin for managing macOS systems with CLI tools.

## Install

```bash
# Add marketplace
claude plugin marketplace add https://github.com/lu-zhengda/macos-toolkit

# Install plugin
claude plugin add macos-toolkit@macos-toolkit
```

### Prerequisites

Install the macOS CLI tools:

```bash
brew tap lu-zhengda/tap
brew install macfig netwhiz whport lanchr macbroom updater
```

## Tools

| Tool | Purpose |
|------|---------|
| [macfig](https://github.com/lu-zhengda/macfig) | macOS hidden defaults manager |
| [netwhiz](https://github.com/lu-zhengda/netwhiz) | Network diagnostics |
| [whport](https://github.com/lu-zhengda/whport) | Port & process manager |
| [lanchr](https://github.com/lu-zhengda/lanchr) | Launch agent & daemon manager |
| [macbroom](https://github.com/lu-zhengda/macbroom) | System cleanup tool |
| [updater](https://github.com/lu-zhengda/updater) | App update manager |

## Skills

6 skills that activate automatically when Claude detects relevant context (e.g., asking "what's using port 3000?" triggers the whport skill).

## Commands

| Command | Tool | Description |
|---------|------|-------------|
| `/cleanup` | macbroom | Scan for reclaimable disk space |
| `/updates` | updater | Check for available app updates |
| `/ports` | whport | List processes listening on ports |
| `/services` | lanchr | Diagnose broken launch agents/daemons |
| `/network` | netwhiz | Show network overview and status |
| `/defaults` | macfig | Browse and manage hidden defaults |

## License

MIT
