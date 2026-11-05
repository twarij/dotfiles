# Jirawat's dotfiles

Personal dotfiles and dev environment setup, managed with [Chezmoi](https://www.chezmoi.io).

## Setup

**macOS**

```bash
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply twarij/dotfiles
```

**Windows**

Docker Desktop needs hardware virtualization. Before setup, enter BIOS/UEFI and enable **SVM Mode** (AMD) or **Intel Virtualization Technology** (Intel). Reboot after setup so WSL takes effect.

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
winget install twpayne.chezmoi
# reopen the terminal so the new PATH from winget takes effect
chezmoi init --apply twarij/dotfiles
```

## Tools

**Shell**

- [Powerlevel10k](https://github.com/romkatv/powerlevel10k) — zsh prompt (macOS)
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) — fish-style suggestions (macOS)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) — command coloring (macOS)
- [PowerShell 7](https://github.com/PowerShell/PowerShell) — default shell, set as Windows Terminal's default profile (Windows)

**Version managers**

- [Chezmoi](https://www.chezmoi.io) — dotfiles manager
- [fnm](https://github.com/Schniz/fnm) — Node.js version manager
- [uv](https://github.com/astral-sh/uv) — Python version & package manager

**Git**

- [Git](https://git-scm.com)
- [gh](https://cli.github.com) — GitHub CLI
- [Fork](https://git-fork.com) — Git GUI

**AI**

- [Claude Code](https://claude.ai/code) — AI coding assistant. See [`configs/dot_claude/CLAUDE.md`](configs/dot_claude/CLAUDE.md) for custom skills, plugins, and MCP servers.

**Dev tools**

- [Task](https://taskfile.dev) — runs setup tasks, e.g. `task -d setup/macos setup` or `task -d setup/shared setup`
- [VS Code](https://code.visualstudio.com)
- [Docker](https://www.docker.com)
- [Postman](https://www.postman.com)
- [TablePlus](https://tableplus.com)
- [DBeaver](https://dbeaver.io)
