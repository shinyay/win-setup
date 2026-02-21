# Git & GitHub — Detailed Configuration Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-21

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Git Configuration (`~/.gitconfig`)](#git-configuration-gitconfig)
  - [\[user\]](#user)
  - [\[core\]](#core)
  - [\[color\]](#color)
  - [\[fetch\]](#fetch)
  - [\[init\]](#init)
  - [\[gpg\] / \[commit\]](#gpg--commit)
  - [\[alias\]](#alias)
- [Alias Usage Examples](#alias-usage-examples)
- [SSH Setup](#ssh-setup)
  - [Key Inventory](#key-inventory)
  - [Testing SSH Connection](#testing-ssh-connection)
  - [Generating a New Key](#generating-a-new-key)
- [GPG Setup](#gpg-setup)
  - [Key Details](#key-details)
  - [Enabling Commit Signing](#enabling-commit-signing)
  - [Useful GPG Commands](#useful-gpg-commands)
- [GitHub CLI (`gh`)](#github-cli-gh)
  - [Authentication](#authentication)
  - [Common Commands](#common-commands)
  - [Extension: gh-copilot](#extension-gh-copilot)
- [GitHub Copilot CLI](#github-copilot-cli)
  - [Commands](#commands)
  - [Usage Examples](#usage-examples)
- [Fish Shell Integration](#fish-shell-integration)
  - [Git Abbreviations](#git-abbreviations)
  - [fzf.fish Git Bindings](#fzffish-git-bindings)
  - [Tide Git Prompt](#tide-git-prompt)
- [Full `~/.gitconfig` (Raw)](#full-gitconfig-raw)

---

## Overview

| Item | Value |
|------|-------|
| **Git version** | `2.53.0` |
| **Installed via** | Homebrew (Linuxbrew) |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/git` |
| **Config file** | `~/.gitconfig` |
| **User name** | `shinyay` |
| **User email** | `shinya.com@gmail.com` |
| **Default branch** | `main` |
| **Editor** | `vim -c "set fenc=utf-8"` |
| **SSH key (primary)** | `~/.ssh/id_ed25519` (Ed25519, 2024-10-23) |
| **GPG key** | `ed25519/C6D9D44B7BC5117F` (2024-11-15) |
| **GitHub CLI** | `gh` v2.87.2, logged in as `shinyay` |
| **Copilot CLI** | v0.0.414 (`copilot-cli@prerelease`) |

- [Git Official Site](https://git-scm.com/)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub CLI Manual](https://cli.github.com/manual/)

---

## Installation

### 1. Install Git via Homebrew

```shell
brew install git
```

### 2. Verify installation

```shell
git --version
# git version 2.53.0

which git
# /home/linuxbrew/.linuxbrew/bin/git
```

> **Note:** Homebrew's Git takes precedence over the system Git (`/usr/bin/git`) because `/home/linuxbrew/.linuxbrew/bin` appears earlier in `$PATH`.

---

## Git Configuration (`~/.gitconfig`)

### [user]

| Key | Value | Description |
|-----|-------|-------------|
| `name` | `shinyay` | Author name used in commits |
| `email` | `shinya.com@gmail.com` | Author email used in commits |

```ini
[user]
	name = shinyay
	email = shinya.com@gmail.com
```

These values appear in every commit as the `Author:` line. They should match your GitHub account's primary email to ensure commits are linked to your profile.

---

### [core]

| Key | Value | Description |
|-----|-------|-------------|
| `quotepath` | `false` | Show non-ASCII file names as-is (e.g., Japanese characters) instead of quoting them |
| `safecrlf` | `true` | Warn if a line-ending conversion could corrupt data |
| `autocrlf` | `false` | Do **not** auto-convert line endings (keep LF on Linux) |
| `editor` | `vim -c "set fenc=utf-8"` | Use Vim as the commit message editor, forcing UTF-8 encoding |

```ini
[core]
	quotepath = false
	safecrlf = true
	autocrlf = false
	editor = vim -c "set fenc=utf-8"
```

#### Why these values?

- **`quotepath = false`** — Essential when working with repos that contain non-ASCII file names. Without this, `git status` would show them as octal escapes (e.g., `"\343\203\206\343\202\271\343\203\210"`).
- **`safecrlf = true`** — Prevents silent data corruption when line endings differ across platforms. Git will refuse to commit if the conversion is irreversible.
- **`autocrlf = false`** — Since this is a Linux (WSL) environment, line endings should stay as LF. Setting this to `true` or `input` is only needed when sharing repos with native Windows editors.
- **`editor = vim -c "set fenc=utf-8"`** — Ensures commit messages are always saved in UTF-8, even if the system locale differs.

---

### [color]

| Key | Value | Description |
|-----|-------|-------------|
| `diff` | `auto` | Colorize `git diff` output when writing to a terminal |
| `status` | `auto` | Colorize `git status` output when writing to a terminal |
| `branch` | `auto` | Colorize `git branch` output when writing to a terminal |

```ini
[color]
	diff = auto
	status = auto
	branch = auto
```

`auto` means colors are used only when the output is a terminal (not when piped or redirected).

---

### [fetch]

| Key | Value | Description |
|-----|-------|-------------|
| `prune` | `true` | Automatically delete remote-tracking branches that no longer exist on the remote |

```ini
[fetch]
	prune = true
```

With `prune = true`, every `git fetch` (and `git pull`) automatically runs the equivalent of `git fetch --prune`. This keeps your local remote-tracking branches clean — if someone deletes `feature/old` on the remote, your local `origin/feature/old` is removed automatically.

---

### [init]

| Key | Value | Description |
|-----|-------|-------------|
| `defaultBranch` | `main` | New repositories use `main` instead of `master` as the initial branch |

```ini
[init]
	defaultBranch = main
```

---

### [gpg] / [commit]

```ini
[gpg]
[commit]
```

> **⚠️ Note:** These sections are present but **empty**. GPG commit signing is **not currently enabled** in the global gitconfig.
>
> The GPG key (`ed25519/C6D9D44B7BC5117F`) exists and is valid, but the following settings are **not configured**:
>
> | Missing setting | What it would do |
> |----------------|------------------|
> | `gpg.format = ssh` or omitted | Specify the signing format |
> | `commit.gpgsign = true` | Auto-sign all commits |
> | `tag.gpgsign = true` | Auto-sign all tags |
> | `user.signingkey = C6D9D44B7BC5117F` | Specify which GPG key to use |
>
> See [Enabling Commit Signing](#enabling-commit-signing) for how to activate GPG signing globally.
>
> **However**, the Fish abbreviation `gc` expands to `git commit -S -m`, which passes the `-S` flag to sign individual commits on a per-command basis.

---

### [alias]

| Alias | Command | Description |
|-------|---------|-------------|
| `alias` | `config --get-regexp ^alias\.` | List all defined Git aliases |
| `st` | `status --verbose --short --branch --untracked-files` | Compact, informative status |
| `plog` | `log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=iso` | Pretty log with ISO dates |
| `glog` | `log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=format:'%c' --all --graph` | Graphical log of all branches |
| `count` | `shortlog -e -s -n` | Commit count per author |

```ini
[alias]
	alias = config --get-regexp ^alias\.
	st = status --verbose --short --branch --untracked-files
	plog = log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=iso
	glog = log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=format:'%c' --all --graph
	count = shortlog -e -s -n
```

---

## Alias Usage Examples

### `git alias` — List all aliases

```shell
$ git alias
alias.alias config --get-regexp ^alias\.
alias.st status --verbose --short --branch --untracked-files
alias.plog log --pretty='format:%C(yellow)%h %C(green)%cd ...
alias.glog log --pretty='format:%C(yellow)%h %C(green)%cd ...
alias.count shortlog -e -s -n
```

### `git st` — Compact status

```shell
$ git st
## main...origin/main
 M README.md
?? new-file.txt
```

| Symbol | Meaning |
|--------|---------|
| `M` | Modified |
| `A` | Added (staged) |
| `D` | Deleted |
| `R` | Renamed |
| `??` | Untracked |

### `git plog` — Pretty log (ISO dates)

```shell
$ git plog
a1b2c3d 2026-02-20 15:30:00 +0900 Fix typo in README  (HEAD -> main) [shinyay]
e4f5g6h 2026-02-19 10:15:00 +0900 Add feature X  (origin/main) [shinyay]
```

Format breakdown:

| Color | Field | Format code |
|-------|-------|-------------|
| Yellow | Short hash | `%h` |
| Green | Commit date (ISO) | `%cd` with `--date=iso` |
| Default | Subject | `%s` |
| Red | Ref names (branches/tags) | `%d` |
| Cyan | Author name | `%an` |

### `git glog` — Graph log (all branches)

```shell
$ git glog
* a1b2c3d Thu Feb 20 15:30:00 2026 Fix typo in README  (HEAD -> main) [shinyay]
| * i7j8k9l Thu Feb 20 14:00:00 2026 WIP on feature  (feature/new) [shinyay]
|/
* e4f5g6h Wed Feb 19 10:15:00 2026 Add feature X  (origin/main) [shinyay]
```

- `--all` shows all branches (including remote-tracking)
- `--graph` draws ASCII branch topology
- `--date=format:'%c'` uses locale-aware date format

### `git count` — Commit leaderboard

```shell
$ git count
    42  shinyay <shinya.com@gmail.com>
     5  dependabot[bot] <49699333+dependabot[bot]@users.noreply.github.com>
```

- `-e` includes email addresses
- `-s` shows counts only (no commit subjects)
- `-n` sorts by number of commits (descending)

---

## SSH Setup

### Key Inventory

| File | Algorithm | Created | Purpose |
|------|-----------|---------|---------|
| `~/.ssh/id_ed25519` | Ed25519 | 2024-10-23 | Primary key for GitHub |
| `~/.ssh/id_ed25519.pub` | Ed25519 | 2024-10-23 | Public key (uploaded to GitHub) |
| `~/.ssh/id_rsa` | RSA | 2025-02-24 | RSA key |
| `~/.ssh/id_rsa.pub` | RSA | 2025-02-24 | RSA public key |
| `~/.ssh/known_hosts` | — | — | Host key fingerprints for verified servers |

> **Primary key:** `id_ed25519` — Ed25519 is the recommended algorithm for GitHub. It provides better security and performance than RSA with shorter keys.

### Testing SSH Connection

```shell
ssh -T git@github.com
# Hi shinyay! You've successfully authenticated, but GitHub does not provide shell access.
```

### Generating a New Key

```shell
# Generate Ed25519 key (recommended)
ssh-keygen -t ed25519 -C "shinya.com@gmail.com"

# Generate RSA key (4096-bit)
ssh-keygen -t rsa -b 4096 -C "shinya.com@gmail.com"

# Copy public key to clipboard (WSL)
cat ~/.ssh/id_ed25519.pub | clip.exe

# Add key to GitHub
gh ssh-key add ~/.ssh/id_ed25519.pub --title "WSL Ubuntu"
```

### SSH Agent

```shell
# Start ssh-agent
eval (ssh-agent -c)    # Fish shell syntax

# Add key to agent
ssh-add ~/.ssh/id_ed25519
```

---

## GPG Setup

### Key Details

| Item | Value |
|------|-------|
| **Algorithm** | Ed25519 |
| **Key ID** | `C6D9D44B7BC5117F` |
| **Fingerprint** | `A8775A1BB72D3BF4A02A6F8CC6D9D44B7BC5117F` |
| **Created** | 2024-11-15 |
| **Capabilities** | `[SC]` (Sign + Certify) |
| **UID** | `shinyay <shinya.com@gmail.com>` |
| **Sub-key** | `cv25519/230DB758D3E040D1` `[E]` (Encrypt) |

```
sec   ed25519/C6D9D44B7BC5117F 2024-11-15 [SC]
      A8775A1BB72D3BF4A02A6F8CC6D9D44B7BC5117F
uid                 [ultimate] shinyay <shinya.com@gmail.com>
ssb   cv25519/230DB758D3E040D1 2024-11-15 [E]
```

#### Key capabilities explained

| Flag | Meaning | Used for |
|------|---------|----------|
| `S` | Sign | Signing commits, tags, and data |
| `C` | Certify | Certifying other keys (key signing) |
| `E` | Encrypt | Encrypting data (sub-key only) |

---

### Enabling Commit Signing

> **Current state:** GPG signing is **not globally enabled** in `~/.gitconfig`. The `[gpg]` and `[commit]` sections are empty.

To enable GPG commit signing globally, run:

```shell
# Tell Git which key to use
git config --global user.signingkey C6D9D44B7BC5117F

# Auto-sign all commits
git config --global commit.gpgsign true

# Auto-sign all tags
git config --global tag.gpgsign true
```

This would add the following to `~/.gitconfig`:

```ini
[user]
	signingkey = C6D9D44B7BC5117F
[commit]
	gpgsign = true
[tag]
	gpgsign = true
```

> **Workaround in use:** The Fish abbreviation `gc` → `git commit -S -m` already passes the `-S` flag, which signs individual commits without requiring global configuration.

### Uploading GPG Key to GitHub

```shell
# Export public key
gpg --armor --export C6D9D44B7BC5117F

# Add to GitHub via CLI
gpg --armor --export C6D9D44B7BC5117F | gh gpg-key add -
```

### Useful GPG Commands

| Command | Description |
|---------|-------------|
| `gpg --list-keys` | List all public keys |
| `gpg --list-secret-keys --keyid-format long` | List secret keys with long IDs |
| `gpg --armor --export <KEY_ID>` | Export public key (ASCII-armored) |
| `gpg --edit-key <KEY_ID>` | Interactive key editing |
| `gpg --verify <file>.sig <file>` | Verify a detached signature |
| `git log --show-signature -1` | Show GPG signature of the latest commit |
| `git verify-commit HEAD` | Verify the signature of a specific commit |

---

## GitHub CLI (`gh`)

| Item | Value |
|------|-------|
| **Version** | `2.87.2` |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/gh` |
| **Installed via** | Homebrew |

### Authentication

| Item | Value |
|------|-------|
| **Logged in as** | `shinyay` |
| **Protocol** | SSH |
| **Token scopes** | `admin:public_key`, `gist`, `read:org`, `repo` |

```shell
# Check auth status
gh auth status

# Login (interactive)
gh auth login

# Refresh token with additional scopes
gh auth refresh -s <scope>
```

#### Token Scopes Explained

| Scope | Access |
|-------|--------|
| `repo` | Full access to private and public repositories |
| `read:org` | Read-only access to organization membership |
| `gist` | Create and manage gists |
| `admin:public_key` | Manage SSH public keys |

### Common Commands

#### Repository operations

```shell
# Clone a repository (uses SSH protocol)
gh repo clone owner/repo

# Create a new repository
gh repo create my-repo --public --source=. --remote=origin

# View repo in browser
gh repo view --web

# Fork a repository
gh repo fork owner/repo
```

#### Pull request workflow

```shell
# Create a PR
gh pr create --title "Feature X" --body "Description"

# List open PRs
gh pr list

# Check out a PR locally
gh pr checkout 42

# Merge a PR
gh pr merge 42 --squash --delete-branch

# View PR diff
gh pr diff 42
```

#### Issue management

```shell
# Create an issue
gh issue create --title "Bug report" --body "Details"

# List issues
gh issue list --label bug

# Close an issue
gh issue close 42
```

#### Gist operations

```shell
# Create a gist from file
gh gist create file.txt --public

# List your gists
gh gist list
```

### Extension: gh-copilot

| Item | Value |
|------|-------|
| **Extension** | `gh-copilot` |
| **Version** | `v1.2.0` |

```shell
# Install
gh extension install github/gh-copilot

# Update
gh extension upgrade gh-copilot
```

#### Usage

```shell
# Ask Copilot to explain a command
gh copilot explain "awk '{print $1}' file.txt"

# Ask Copilot to suggest a command
gh copilot suggest "find large files over 100MB"
```

---

## GitHub Copilot CLI

| Item | Value |
|------|-------|
| **Version** | `0.0.414` |
| **Installed via** | Homebrew Cask (`copilot-cli@prerelease`) |
| **Binary** | `copilot` |

### Installation

```shell
brew install --cask copilot-cli@prerelease
```

### Commands

| Command | Description |
|---------|-------------|
| `copilot explain` | Explain a command or code snippet in natural language |
| `copilot suggest` | Suggest a shell command for a given task |

### Usage Examples

```shell
# Explain a complex command
copilot explain "find . -name '*.log' -mtime +30 -delete"

# Get a command suggestion
copilot suggest "compress all PNG files in current directory"
```

> **Note:** The Fish abbreviation `copilot` → `gh copilot suggest` is defined in `config.fish` but is currently **commented out**.

---

## Fish Shell Integration

### Git Abbreviations

Defined in `~/.config/fish/config.fish` inside the `set_abbr` function:

| Abbreviation | Expands To | Description |
|-------------|------------|-------------|
| `ga` | `git add -v` | Stage files with verbose output showing each added file |
| `gc` | `git commit -S -m` | Create a GPG-signed commit with an inline message |
| `gcpast` | `git commit -S --date=format:relative:1.day.ago -m` | Create a GPG-signed commit backdated by one day |

#### Usage

```shell
# Stage all changes (verbose)
$ ga .
add 'README.md'
add 'src/main.rs'

# Signed commit
$ gc "feat: add new feature"
# Expands to: git commit -S -m "feat: add new feature"

# Backdated signed commit
$ gcpast "fix: patch applied yesterday"
# Expands to: git commit -S --date=format:relative:1.day.ago -m "fix: patch applied yesterday"
```

> **Why `-S`?** The abbreviation includes `-S` (uppercase) to GPG-sign each commit, even though `commit.gpgsign` is not set globally. This is an explicit, per-commit signing approach.

---

### fzf.fish Git Bindings

The fzf.fish plugin provides interactive Git search via custom key bindings configured in `config.fish`:

```fish
fzf_configure_bindings --git_log=\cl --git_status=\cs
```

| Key | Action | Description |
|-----|--------|-------------|
| `Ctrl+L` | Search git log | Interactive commit browser with preview |
| `Ctrl+S` | Search git status | Interactive modified file selector |

---

### Tide Git Prompt

The Tide prompt shows Git repository status inline in the left prompt.

#### Prompt position

Git is the 4th item in the left prompt: `vi_mode` → `os` → `pwd` → **`git`** → `newline`

#### Tide Git colors

| State | BG Color | Meaning |
|-------|----------|---------|
| Clean | `#4E9A06` (green) | Working tree is clean |
| Unstable | `#C4A000` (yellow) | Modified or staged files |
| Urgent | `#CC0000` (red) | Merge conflicts |

#### Tide Git variables

| Variable | Value |
|----------|-------|
| `tide_git_bg_color` | `4E9A06` |
| `tide_git_bg_color_unstable` | `C4A000` |
| `tide_git_bg_color_urgent` | `CC0000` |
| `tide_git_color_branch` | `000000` |
| `tide_git_color_conflicted` | `000000` |
| `tide_git_color_dirty` | `000000` |
| `tide_git_color_operation` | `000000` |
| `tide_git_color_staged` | `000000` |
| `tide_git_color_stash` | `000000` |
| `tide_git_color_untracked` | `000000` |
| `tide_git_color_upstream` | `000000` |
| `tide_git_icon` | `` |
| `tide_git_truncation_length` | `24` |

---

## Full `~/.gitconfig` (Raw)

```ini
[user]
	name = shinyay
	email = shinya.com@gmail.com
[core]
	quotepath = false
	safecrlf = true
	autocrlf = false
	editor = vim -c "set fenc=utf-8"
[color]
	diff = auto
	status = auto
	branch = auto
[fetch]
	prune = true
[init]
	defaultBranch = main
[gpg]
[commit]
[alias]
	alias = config --get-regexp ^alias\.
	st = status --verbose --short --branch --untracked-files
	plog = log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=iso
	glog = log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=format:'%c' --all --graph
	count = shortlog -e -s -n
```
