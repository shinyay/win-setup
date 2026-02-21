# Git & GitHub — Detailed Configuration Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-22

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
  - [\[gpg\] / \[commit\] / \[tag\]](#gpg--commit--tag)
  - [\[pull\]](#pull)
  - [\[push\]](#push)
  - [\[merge\]](#merge)
  - [\[diff\]](#diff)
  - [\[rerere\]](#rerere)
  - [\[rebase\]](#rebase)
  - [\[interactive\] / \[delta\]](#interactive--delta)
  - [\[alias\]](#alias)
- [Alias Usage Examples](#alias-usage-examples)
- [Diff Pager — delta](#diff-pager--delta)
- [Git TUI — lazygit](#git-tui--lazygit)
- [SSH Setup](#ssh-setup)
  - [Key Inventory](#key-inventory)
  - [SSH Config (`~/.ssh/config`)](#ssh-config-sshconfig)
  - [Testing SSH Connection](#testing-ssh-connection)
  - [Generating a New Key](#generating-a-new-key)
- [GPG Setup](#gpg-setup)
  - [Key Details](#key-details)
  - [Commit Signing (Enabled)](#commit-signing-enabled)
  - [Useful GPG Commands](#useful-gpg-commands)
- [GitHub CLI (`gh`)](#github-cli-gh)
  - [Authentication](#authentication)
  - [Aliases](#aliases)
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
| **Commit signing** | GPG, globally enabled (`commit.gpgsign = true`) |
| **Diff pager** | `delta` 0.18.2 (syntax-highlighting, side-by-side) |
| **Git TUI** | `lazygit` 0.59.0 |
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
| `pager` | `delta` | Use delta as the diff pager (syntax-highlighting, side-by-side) |

```ini
[core]
	quotepath = false
	safecrlf = true
	autocrlf = false
	editor = vim -c "set fenc=utf-8"
	pager = delta
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

### [gpg] / [commit] / [tag]

| Key | Value | Description |
|-----|-------|-------------|
| `gpg.format` | `openpgp` | Use OpenPGP (GPG) format for commit signing |
| `user.signingkey` | `C6D9D44B7BC5117F` | GPG key ID used for signing |
| `commit.gpgsign` | `true` | **Automatically sign all commits** |
| `commit.verbose` | `true` | Show diff in commit editor for more informed messages |
| `tag.gpgsign` | `true` | **Automatically sign all tags** |

```ini
[gpg]
	format = openpgp
[commit]
	verbose = true
	gpgsign = true
[tag]
	gpgsign = true
```

> ✅ **GPG signing is globally enabled.** All commits and tags are automatically signed with key `C6D9D44B7BC5117F`. The Fish abbreviation `gc` also includes `-S` for explicit signing.

---

### [pull]

| Key | Value | Description |
|-----|-------|-------------|
| `rebase` | `true` | Rebase local commits on top of upstream changes instead of creating merge commits |

```ini
[pull]
	rebase = true
```

This keeps history linear. When you `git pull`, your local commits are replayed on top of the remote branch rather than creating a merge commit.

---

### [push]

| Key | Value | Description |
|-----|-------|-------------|
| `autoSetupRemote` | `true` | Automatically set upstream tracking on first push (no more `--set-upstream`) |
| `default` | `current` | Push only the current branch (not all branches) |

```ini
[push]
	autoSetupRemote = true
	default = current
```

- **`autoSetupRemote`** eliminates the need for `git push --set-upstream origin branch-name` on the first push of a new branch.
- **`default = current`** is safer than `matching` — it only pushes the branch you're currently on.

---

### [merge]

| Key | Value | Description |
|-----|-------|-------------|
| `conflictstyle` | `zdiff3` | Show three-way conflict markers with ancestor context (cleaner than diff3) |

```ini
[merge]
	conflictstyle = zdiff3
```

`zdiff3` (zero-diff3) is an improvement over `diff3` that provides cleaner conflict markers by removing lines that are identical in all three versions. Requires Git 2.37+.

---

### [diff]

| Key | Value | Description |
|-----|-------|-------------|
| `algorithm` | `histogram` | Use the histogram diff algorithm (better output than default Myers) |
| `colorMoved` | `default` | Highlight moved code blocks in a different color |

```ini
[diff]
	algorithm = histogram
	colorMoved = default
```

- **`histogram`** produces more readable diffs, especially for code where lines are moved or refactored.
- **`colorMoved`** visually distinguishes moved lines from added/removed lines — very useful during code review.

---

### [rerere]

| Key | Value | Description |
|-----|-------|-------------|
| `enabled` | `true` | **Re**use **Re**corded **Re**solution — remember how you resolved conflicts |

```ini
[rerere]
	enabled = true
```

When you resolve a merge conflict, Git records the resolution. If the same conflict appears again (e.g., during a rebase), Git automatically applies the same resolution. This is a huge time-saver for long-running branches.

---

### [rebase]

| Key | Value | Description |
|-----|-------|-------------|
| `autoStash` | `true` | Automatically stash uncommitted changes before rebase, re-apply after |
| `autoSquash` | `true` | Automatically apply `fixup!` and `squash!` prefixes during interactive rebase |

```ini
[rebase]
	autoStash = true
	autoSquash = true
```

- **`autoStash`** means you never need to manually `git stash` before rebasing — Git does it automatically.
- **`autoSquash`** works with `git commit --fixup=<SHA>` to automatically squash fixup commits during `git rebase -i`.

---

### [interactive] / [delta]

| Key | Value | Description |
|-----|-------|-------------|
| `interactive.diffFilter` | `delta --color-only` | Use delta for `git add -p` interactive diff display |
| `delta.navigate` | `true` | Enable `n`/`N` navigation between diff hunks in delta |
| `delta.side-by-side` | `true` | Show diffs in side-by-side view |
| `delta.line-numbers` | `true` | Show line numbers in diff output |

```ini
[interactive]
	diffFilter = delta --color-only
[delta]
	navigate = true
	side-by-side = true
	line-numbers = true
```

See [Diff Pager — delta](#diff-pager--delta) for full delta documentation.

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

## Diff Pager — delta

| Item | Value |
|------|-------|
| **Tool** | [delta](https://github.com/dandavison/delta) |
| **Version** | `0.18.2` |
| **Installed via** | Homebrew (`brew install git-delta`) |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/delta` |

delta is a syntax-highlighting pager for `git diff`, `git log`, `git show`, and `grep` output. It uses bat's syntax themes for language-aware highlighting.

### Features

- **Syntax highlighting** — Language-aware coloring using bat's themes
- **Side-by-side view** — Two-column diff display (`delta.side-by-side = true`)
- **Line numbers** — Shown in gutter (`delta.line-numbers = true`)
- **Word-level diff** — Highlights changed words within lines, not just changed lines
- **Hunk navigation** — Press `n`/`N` to jump between diff hunks (`delta.navigate = true`)
- **Moved code detection** — Works with `diff.colorMoved = default`
- **Interactive staging** — Renders `git add -p` with syntax highlighting via `interactive.diffFilter`

### Configuration in `~/.gitconfig`

```ini
[core]
	pager = delta
[interactive]
	diffFilter = delta --color-only
[delta]
	navigate = true
	side-by-side = true
	line-numbers = true
```

### Useful Commands

```shell
# Preview available dark themes
delta --list-syntax-themes --dark

# Show syntax themes with sample output
delta --show-syntax-themes --dark

# Set a specific theme
git config --global delta.syntax-theme 'Dracula'

# Disable side-by-side for narrow terminals
git config --global delta.side-by-side false
```

### Fish Completion

delta installs Fish completions automatically to:
```
/home/linuxbrew/.linuxbrew/share/fish/vendor_completions.d/
```

---

## Git TUI — lazygit

| Item | Value |
|------|-------|
| **Tool** | [lazygit](https://github.com/jesseduffield/lazygit) |
| **Version** | `0.59.0` |
| **Installed via** | Homebrew (`brew install lazygit`) |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/lazygit` |
| **Fish abbreviation** | `lg` → `lazygit` |

lazygit is a terminal UI for Git that provides a keyboard-driven interface for all Git operations.

### Key Features

- **Stage individual lines/hunks** — Precise staging without `git add -p`
- **Interactive rebasing** — Visual rebase with drag-and-drop reordering
- **Cherry-pick & bisect** — Visual commit selection
- **Merge conflict resolution** — Side-by-side conflict viewer
- **Undo/redo** — Safely reverse any operation
- **Worktree support** — Manage multiple working directories
- **Custom commands** — Bind your own shortcuts

### Quick Start

```shell
# Launch (from any Git repository)
lazygit
# or use the Fish abbreviation:
lg
```

### Navigation

| Key | Action |
|-----|--------|
| `1`-`5` | Switch panels (Status, Files, Branches, Commits, Stash) |
| `Enter` | View details / expand |
| `Space` | Stage/unstage file |
| `c` | Commit |
| `p` | Push |
| `P` | Pull |
| `z` | Undo |
| `?` | Show all keybindings |

---

## SSH Setup

### Key Inventory

| File | Algorithm | Created | Purpose |
|------|-----------|---------|---------|
| `~/.ssh/id_ed25519` | Ed25519 | 2024-10-23 | Primary key for GitHub |
| `~/.ssh/id_ed25519.pub` | Ed25519 | 2024-10-23 | Public key (uploaded to GitHub) |
| `~/.ssh/id_rsa` | RSA | 2025-02-24 | RSA key |
| `~/.ssh/id_rsa.pub` | RSA | 2025-02-24 | RSA public key |
| `~/.ssh/config` | — | — | SSH host configuration |
| `~/.ssh/known_hosts` | — | — | Host key fingerprints for verified servers |

> **Primary key:** `id_ed25519` — Ed25519 is the recommended algorithm for GitHub. It provides better security and performance than RSA with shorter keys.

### SSH Config (`~/.ssh/config`)

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

| Directive | Value | Purpose |
|-----------|-------|---------|
| `Host` | `github.com` | Match rule for GitHub connections |
| `HostName` | `github.com` | Actual hostname to connect to |
| `User` | `git` | SSH user (always `git` for GitHub) |
| `IdentityFile` | `~/.ssh/id_ed25519` | Use Ed25519 key specifically |
| `IdentitiesOnly` | `yes` | Only use the specified key (don't try all keys) |

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

### Commit Signing (Enabled)

> ✅ **GPG signing is globally enabled.** All commits and tags are automatically signed.

The following settings in `~/.gitconfig` enable automatic signing:

```ini
[gpg]
	format = openpgp
[user]
	signingkey = C6D9D44B7BC5117F
[commit]
	gpgsign = true
[tag]
	gpgsign = true
```

No manual `-S` flag needed — every `git commit` and `git tag` is automatically signed.

#### Verifying signed commits

```shell
# Check signature of latest commit
git log --show-signature -1

# Verify a specific commit
git verify-commit HEAD
```

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

### Aliases

| Alias | Expands to | Description |
|-------|-----------|-------------|
| `co` | `pr checkout` | Check out a PR locally |
| `prc` | `pr create --fill` | Create a PR with auto-filled title/body |
| `prv` | `pr view --web` | View current PR in browser |
| `prm` | `pr merge --squash --delete-branch` | Squash-merge PR and delete branch |
| `prl` | `pr list` | List open PRs |
| `rv` | `repo view --web` | View current repo in browser |
| `il` | `issue list` | List open issues |
| `ic` | `issue create` | Create a new issue |

```shell
# Usage examples
gh co 42          # Check out PR #42
gh prc            # Create PR from current branch
gh prv            # Open current PR in browser
gh prm 42         # Squash-merge PR #42
gh il             # List issues
```

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
| `lg` | `lazygit` | Launch lazygit TUI |

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

> **Why `-S`?** The abbreviation includes `-S` (uppercase) to GPG-sign each commit. With `commit.gpgsign = true` now set globally, the `-S` flag is redundant but harmless — it serves as an explicit reminder that signing is active.

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
	signingkey = C6D9D44B7BC5117F
[core]
	quotepath = false
	safecrlf = true
	autocrlf = false
	editor = vim -c "set fenc=utf-8"
	pager = delta
[color]
	diff = auto
	status = auto
	branch = auto
[fetch]
	prune = true
[init]
	defaultBranch = main
[gpg]
	format = openpgp
[commit]
	verbose = true
	gpgsign = true
[alias]
	alias = config --get-regexp ^alias\.
	st = status --verbose --short --branch --untracked-files
	plog = log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=iso
	glog = log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=format:'%c' --all --graph
	count = shortlog -e -s -n
[pull]
	rebase = true
[push]
	autoSetupRemote = true
	default = current
[merge]
	conflictstyle = zdiff3
[diff]
	algorithm = histogram
	colorMoved = default
[rerere]
	enabled = true
[rebase]
	autoStash = true
	autoSquash = true
[tag]
	gpgsign = true
[interactive]
	diffFilter = delta --color-only
[delta]
	navigate = true
	side-by-side = true
	line-numbers = true
```
