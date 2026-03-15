# CLAUDE.md — Dotfiles Repository Guide

## Overview

This is a personal dotfiles repository for a Linux (Ubuntu/Debian) environment. It contains shell configuration files intended to be symlinked or copied to the user's home directory (`~`).

## Repository Structure

```
dotfiles/
├── .bashrc       # Main bash shell configuration
├── .aliases      # Custom shell aliases (sourced by .bashrc)
└── CLAUDE.md     # This file
```

## File Descriptions

### `.bashrc`
The primary bash startup file sourced for interactive login shells. Key sections:

- **History settings**: `HISTCONTROL=ignoreboth`, `HISTSIZE=9999`, `HISTFILESIZE=2000`, `histappend`
- **Prompt customization**: Custom `PS1` with date/time prefix (`\d \t`) for xterm terminals, with optional color support
- **Color support**: Enables colored output for `ls`, `grep`, `fgrep`, `egrep` via `dircolors`
- **Built-in aliases**: `ll`, `la`, `l` for directory listing; `alert` for long-running command notifications
- **Alias loading**: Sources `~/.bash_aliases` if it exists
- **Bash completion**: Loads from `/usr/share/bash-completion/bash_completion` or `/etc/bash_completion`

### `.aliases`
Supplemental alias definitions (not auto-sourced by `.bashrc` — must be linked/copied as `~/.bash_aliases` or sourced manually). Current aliases:

| Alias | Command | Purpose |
|-------|---------|---------|
| `vi` | `vim` | Use vim instead of vi |
| `grep` | `grep --color=auto` | Colored grep output |
| `h` | `history` | Shell history shortcut |
| `c` | `clear` | Clear terminal |
| `svim` | `sudo vim` | Edit files as root |
| `ping` | `ping -c 1` | Single-packet ping by default |
| `pbcopy` | `xclip -selection clipboard` | Copy to clipboard (Linux equivalent of macOS pbcopy) |
| `pbpaste` | `xclip -selection clipboard -o` | Paste from clipboard |
| `sr` | `sudo rsh` | Remote shell as root |
| `diff` | `colordiff` | Colored diff output |
| `mount` | `mount\|column -t` | Formatted mount output |
| `fastping` | `ping -c 2 -s.2` | Quick network ping test |
| `ports` | `netstat -tulanp` | List all open ports and listeners |

## Development Conventions

### Editing dotfiles
- Keep changes minimal and well-commented
- Group related settings together with section comments (e.g., `#git`, `# history settings`)
- Prefer appending to `.aliases` over embedding aliases directly in `.bashrc`
- When adding new aliases, follow the existing flat format — one alias per line, no blank lines between related aliases except to denote sections

### Adding new dotfiles
- Add the file at the repo root with a leading dot (e.g., `.vimrc`, `.gitconfig`)
- Document the file's purpose and key settings in this CLAUDE.md under "File Descriptions"
- Consider whether the file should be symlinked (`ln -s`) vs. copied to `~`

### Deployment / Installation
There is no automated install script currently. To apply these dotfiles manually:

```bash
# From the repo root:
cp .bashrc ~/.bashrc
cp .aliases ~/.bash_aliases
source ~/.bashrc
```

Or using symlinks (preferred to keep in sync with the repo):

```bash
ln -sf "$(pwd)/.bashrc" ~/.bashrc
ln -sf "$(pwd)/.aliases" ~/.bash_aliases
source ~/.bashrc
```

## Git Workflow

- Default branch: `master`
- Feature branches use the `claude/` prefix (e.g., `claude/add-claude-documentation-PUOPd`)
- Commit messages are short and descriptive (see history: "added some aliases", "changed bash prompt settings")
- No CI/CD, linting, or automated tests — changes are validated manually by sourcing the files

## Dependencies

The `.aliases` file assumes the following tools are installed:

- `vim` — for the `vi` alias
- `xclip` — for `pbcopy`/`pbpaste` clipboard aliases
- `colordiff` — for the colored `diff` alias
- `netstat` (from `net-tools`) — for the `ports` alias
- `rsh` — for the `sr` alias (uncommon; may need `rsh-client` package)

Install missing dependencies on Debian/Ubuntu:
```bash
sudo apt-get install vim xclip colordiff net-tools
```

## Notes for AI Assistants

- This repo has no build system, test suite, or linter — do not attempt to run tests
- Do not reformat or restructure existing files; match the surrounding style when making edits
- Changes to `.bashrc` should be backwards-compatible with Bash 4+
- Aliases that shadow system commands (e.g., `grep`, `ping`, `diff`, `mount`) are intentional
- The `xterm*` terminal detection in `.bashrc` includes a date/time prefix in `PS1` — this is deliberate
