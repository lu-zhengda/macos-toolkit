# macos-toolkit

A Claude Code plugin for managing macOS systems with CLI tools.

## Install

```bash
claude plugin add lu-zhengda/macos-toolkit
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

The plugin provides 6 tool-specific skills that activate automatically when relevant, plus 3 cross-tool workflow skills.

## Commands

| Command | Description |
|---------|-------------|
| `/system-health` | Run full macOS system health check |
| `/dev-setup` | Configure macOS for development |
| `/network-debug` | Diagnose network issues |

## License

MIT
