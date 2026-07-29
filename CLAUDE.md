# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal dotfiles repository managed by [Chezmoi](https://www.chezmoi.io). Contains configuration for shell, git, and Claude Code across macOS and Windows.

## Key Architecture

### Directory Structure

Chezmoi source root is `configs/` (set via `.chezmoiroot`).

- `configs/dot_claude/` → `~/.claude/` — Claude Code config, commands, statusline scripts
- `configs/dot_gitconfig*` → `~/.gitconfig*`
- `configs/dot_zshrc` → `~/.zshrc` (macOS only)
- `configs/Documents/` → `~/Documents/` (Windows only)
- `setup/` - Bootstrap and installation scripts

## Common Commands

### Setup and Bootstrap

- `setup/{shared,macos,windows}/` each have their own `Taskfile.yml`, run via [Task](https://taskfile.dev) (e.g. `task -d setup/macos setup`) — replaced the old numbered `.sh`/`.ps1` scripts
- `setup/shared/Taskfile.yml` — cross-platform tasks (node, python, third-party skills). One file runs on both OSes: Task's default shell interpreter (`mvdan.cc/sh`) executes the same POSIX-style syntax on macOS and Windows
- `setup/macos/Taskfile.yml` — macOS-only tasks (Homebrew, `Brewfile`, `defaults write`), fully inlined POSIX shell
- `setup/windows/Taskfile.yml` — Windows-only tasks. `install-packages` is inlined POSIX shell (winget calls translate cleanly); `windows-defaults` shells out to `windows-defaults.ps1` via `pwsh -File`, since that logic (JSON regex edits, `Get-ChildItem` globbing) needs real PowerShell, not `mvdan.cc/sh`

## Chezmoi Conventions

Only two conventions are actually used in this repo (do not assume others, e.g. `private_`/`symlink_`/`executable_` — unused):

- `dot_` prefix → deployed with `.` prefix (e.g. `dot_zshrc` → `~/.zshrc`)
- `.tmpl` extension → Go template, processed before deployment
- `.chezmoiignore` patterns match the **target** path (post `dot_`/`.tmpl` transformation), not the source path — e.g. `.claude/statusline-command.ps1`, not `dot_claude/statusline-command.ps1`

## Development Workflow

When working with this repository:

1. Configuration changes should be made to the source files in this repository
2. Use `chezmoi apply` to apply changes to the actual dotfiles
3. Test changes in the appropriate environment before committing
4. `Dockerfile` tests templating/`.chezmoiignore` only — not a substitute for a real macOS/Windows test

## Important Notes

- This is a personal configuration repository - modifications should respect the owner's preferences
- Setup scripts must install **stable releases only**. Do not use experimental/preview/beta/nightly flags or channels (e.g. `--preview`, `--preview-features`, `--pre`, `--nightly`, `--beta`) — they can break on tool upgrades. If a desired behavior is only available behind such a flag, prefer the stable alternative even if it's less convenient
- Always keep macOS and Windows in sync: most platform pairs in `configs/` are the same filename, different extension (`.sh` ↔ `.ps1`, e.g. `statusline-command.sh`/`.ps1`). Editing one means editing its counterpart in the same commit — except `settings.json.tmpl`, which branches with `{{ if eq .chezmoi.os "windows" }}` instead of having a separate file. `setup/` is the other exception: it has no cross-OS filename pairs at all — `setup/shared/Taskfile.yml` is one file that runs on both OSes via Task, while `setup/macos/Taskfile.yml` and `setup/windows/Taskfile.yml` are independent and don't need to mirror each other
- `.chezmoiignore` platform blocks — if you add an entry for one OS, add the mirrored exclusion for the other
