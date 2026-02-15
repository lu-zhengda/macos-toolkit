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

6 skills — one per tool. Each skill auto-triggers when Claude detects relevant context (e.g., asking "what's using port 3000?" triggers the whport skill) and is also available as a slash command:

| Skill | Tool | Slash command |
|-------|------|---------------|
| macfig | [macfig](https://github.com/lu-zhengda/macfig) | `/macos-toolkit:macfig` |
| updater | [updater](https://github.com/lu-zhengda/updater) | `/macos-toolkit:updater` |
| macbroom | [macbroom](https://github.com/lu-zhengda/macbroom) | `/macos-toolkit:macbroom` |
| whport | [whport](https://github.com/lu-zhengda/whport) | `/macos-toolkit:whport` |
| lanchr | [lanchr](https://github.com/lu-zhengda/lanchr) | `/macos-toolkit:lanchr` |
| netwhiz | [netwhiz](https://github.com/lu-zhengda/netwhiz) | `/macos-toolkit:netwhiz` |

## License

MIT
