# Fish Shell — Detailed Configuration Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-21

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Shell Configuration (`config.fish`)](#shell-configuration-configfish)
- [Directory Structure](#directory-structure)
- [Plugin Manager — Fisher](#plugin-manager--fisher)
- [Installed Fisher Plugins](#installed-fisher-plugins)
  - [Tide (Prompt)](#tide-prompt)
  - [fzf.fish (Fuzzy Finder)](#fzffish-fuzzy-finder)
  - [zoxide (Directory Jumping)](#zoxide-directory-jumping)
  - [z (Directory Jumping)](#z-directory-jumping)
  - [fish-bd (Go Back to Parent)](#fish-bd-go-back-to-parent)
  - [fish_logo (ASCII Art)](#fish_logo-ascii-art)
  - [sdkman-for-fish (Java SDK Manager)](#sdkman-for-fish-java-sdk-manager)
- [Abbreviations](#abbreviations)
- [Custom Functions](#custom-functions)
- [Completions](#completions)
- [Syntax Highlighting / Theme Colors](#syntax-highlighting--theme-colors)
- [Tide Prompt — Full Configuration](#tide-prompt--full-configuration)
- [Key Bindings](#key-bindings)
- [Environment Variables & PATH](#environment-variables--path)
- [Supporting CLI Tools](#supporting-cli-tools)
- [Misc / Cleanup Notes](#misc--cleanup-notes)

---

## Overview

| Item | Value |
|------|-------|
| **Shell** | Fish (Friendly Interactive Shell) |
| **Version** | `4.5.0` |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/fish` |
| **Installed via** | Homebrew (Linuxbrew) |
| **Default shell** | Yes (`$SHELL` = `/home/linuxbrew/.linuxbrew/bin/fish`) |
| **Config dir** | `~/.config/fish/` |

- [Fish Shell Official Site](https://fishshell.com/)
- [Fish Shell GitHub](https://github.com/fish-shell/fish-shell)

---

## Installation

### 1. Install Fish via Homebrew

```shell
brew install fish
```

### 2. Register as a valid login shell

```shell
sudo sh -c "echo /home/linuxbrew/.linuxbrew/bin/fish >> /etc/shells"
```

Verify:

```shell
grep fish /etc/shells
# /home/linuxbrew/.linuxbrew/bin/fish
```

### 3. Set Fish as the default shell

```shell
chsh -s /home/linuxbrew/.linuxbrew/bin/fish
```

### 4. Reboot WSL and verify

```shell
echo $SHELL
# /home/linuxbrew/.linuxbrew/bin/fish
```

### 5. Configure Homebrew PATH for Fish

```shell
echo 'eval (/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> $HOME/.config/fish/config.fish
```

---

## Shell Configuration (`config.fish`)

**File:** `~/.config/fish/config.fish`

```fish
function set_abbr
	abbr --add code		code-insiders
	abbr --add history	_fzf_search_history
	abbr --add l	 	eza -lahF
	abbr --add tree		eza --tree
	abbr --add ga		git add -v
	abbr --add gc		git commit -S -m
	abbr --add gcpast	git commit -S --date=format:relative:1.day.ago -m
	#	abbr --add copilot	gh copilot suggest
end

if status is-interactive
    # Commands to run in interactive sessions can go here
    set_abbr
end
eval (/home/linuxbrew/.linuxbrew/bin/brew shellenv)
zoxide init fish | source
fzf_configure_bindings --directory=\cf --git_log=\cl --git_status=\cs
```

### What this does

1. **`set_abbr`** — Defines abbreviations (expanded inline when typed) for common commands
2. **Interactive guard** — Only sets abbreviations in interactive sessions
3. **Homebrew shellenv** — Adds Homebrew's `bin`, `sbin`, `HOMEBREW_PREFIX`, etc. to the environment
4. **zoxide init** — Initializes zoxide (`z` command) for smart directory jumping
4. **fzf bindings** — Overrides default fzf.fish keybindings:
   - `Ctrl+F` → Search directories
   - `Ctrl+L` → Search git log
   - `Ctrl+S` → Search git status

---

## Directory Structure

```
~/.config/fish/
├── config.fish                    # Main configuration
├── fish_plugins                   # Fisher plugin list (8 plugins)
├── fish_variables                 # Universal variables (auto-managed)
├── conf.d/                        # Auto-loaded configuration snippets
│   ├── _tide_init.fish            #   Tide prompt initialization
│   ├── autopair.fish              #   autopair.fish plugin init
│   ├── done.fish                  #   done notification plugin init
│   ├── fish_frozen_key_bindings.fish  # Key bindings migration (Fish 4.3)
│   ├── fish_frozen_theme.fish     #   Theme colors migration (Fish 4.3)
│   ├── fzf.fish                   #   fzf.fish plugin init
│   ├── sdk.fish                   #   SDKMAN! for Fish init
│   ├── sponge.fish                #   sponge history cleanup init
│   ├── uv.env.fish                #   UV (Python) environment
│   └── z.fish                     #   z directory jumper init
├── completions/                   # Tab completion scripts
│   ├── abbr.fish
│   ├── apt-get.fish
│   ├── bd.fish
│   ├── code.fish
│   ├── curl.fish
│   ├── fish_logo.fish
│   ├── fisher.fish
│   ├── fzf_configure_bindings.fish
│   ├── gh.fish
│   ├── sdk.fish
│   └── tide.fish
├── functions/                     # Autoloaded functions
│   ├── bd.fish                    #   fish-bd plugin
│   ├── clearCaches.fish           #   Custom: clear system caches
│   ├── fish_greeting.fish         #   Custom: login greeting
│   ├── fish_logo.fish             #   fish_logo plugin
│   ├── fish_mode_prompt.fish      #   Tide vi mode
│   ├── fish_prompt.fish           #   Tide prompt
│   ├── fisher.fish                #   Fisher plugin manager
│   ├── fzf_configure_bindings.fish #  fzf.fish bindings
│   ├── sdk.fish                   #   SDKMAN! wrapper
│   ├── sponge_filter_*.fish       #   sponge history cleanup
│   ├── tide.fish                  #   Tide CLI
│   ├── tide/                      #   Tide sub-functions
│   └── _tide_*, _fzf_*, _autopair_*, _sponge_*, __z*  # Plugin internal functions
└── themes/                        # Custom themes (empty)
```

---

## Plugin Manager — Fisher

| Item | Value |
|------|-------|
| **Version** | `4.4.8` |
| **GitHub** | [jorgebucaran/fisher](https://github.com/jorgebucaran/fisher) |

### Install Fisher

```shell
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

### Useful commands

```shell
fisher list                    # List installed plugins
fisher install <plugin>        # Install a plugin
fisher update                  # Update all plugins
fisher remove <plugin>         # Remove a plugin
```

---

## Installed Fisher Plugins

**File:** `~/.config/fish/fish_plugins`

```
jorgebucaran/fisher
ilancosman/tide@v6
patrickf1/fzf.fish
0rax/fish-bd
laughedelic/fish_logo
reitzig/sdkman-for-fish@v2.1.0
jorgebucaran/autopair.fish
meaningful-ooo/sponge
franciscolourenco/done
```

---

### Tide (Prompt)

> The ultimate Fish prompt — fast, extensible, with powerline/rainbow styles.

| Item | Value |
|------|-------|
| **Version** | v6 |
| **GitHub** | [IlanCosman/tide](https://github.com/IlanCosman/tide) |
| **Wiki** | [Configuration](https://github.com/IlanCosman/tide/wiki/Configuration) |

#### Install

```shell
fisher install IlanCosman/tide@v6
```

#### Current Tide configuration command

```shell
tide configure --auto \
  --style=Rainbow \
  --prompt_colors='True color' \
  --show_time='12-hour format' \
  --rainbow_prompt_separators=Round \
  --powerline_prompt_heads=Round \
  --powerline_prompt_tails=Round \
  --powerline_prompt_style='Two lines, character and frame' \
  --prompt_connection=Disconnected \
  --powerline_right_prompt_frame=Yes \
  --prompt_connection_andor_frame_color=Darkest \
  --prompt_spacing=Sparse \
  --icons='Many icons' \
  --transient=Yes
```

#### Prompt layout

**Left prompt items:**

| Position | Item | Description |
|----------|------|-------------|
| 1 | `vi_mode` | Vi mode indicator (D/I/R/V) |
| 2 | `os` | OS icon (Ubuntu: ) |
| 3 | `pwd` | Current working directory |
| 4 | `git` | Git branch, status, upstream |
| 5 | `newline` | Two-line prompt break |

**Right prompt items:**

| Position | Item | Description |
|----------|------|-------------|
| 1 | `status` | Exit code (✔ / ✘) |
| 2 | `cmd_duration` | Command execution time (>3s) |
| 3 | `context` | user@host (shown in SSH) |
| 4 | `jobs` | Background jobs count |
| 5 | `direnv` | direnv status |
| 6–24 | Language/tool items | bun, node, python, rustc, java, php, pulumi, ruby, go, gcloud, kubectl, distrobox, toolbox, terraform, aws, nix_shell, crystal, elixir, zig |
| 25 | `time` | Current time (12-hour: `%r`) |

See [Tide Prompt — Full Configuration](#tide-prompt--full-configuration) for all variables.

---

### fzf.fish (Fuzzy Finder)

> Augment your Fish command line with fzf-powered interactive search.

| Item | Value |
|------|-------|
| **GitHub** | [PatrickF1/fzf.fish](https://github.com/PatrickF1/fzf.fish) |
| **Dependencies** | `fzf`, `bat`, `fd` |

#### Install

```shell
brew install fzf bat fd
fisher install PatrickF1/fzf.fish
```

#### Key bindings (customized in `config.fish`)

| Key | Action | Default |
|-----|--------|---------|
| `Ctrl+R` | Search command history | ✅ (default) |
| `Ctrl+Alt+V` | Search environment variables | ✅ (default) |
| `Ctrl+F` | Search directories recursively | Customized (default: `Ctrl+Alt+F`) |
| `Ctrl+L` | Search git log | Customized (default: `Ctrl+Alt+L`) |
| `Ctrl+S` | Search git status | Customized (default: `Ctrl+Alt+S`) |

Configuration line in `config.fish`:

```fish
fzf_configure_bindings --directory=\cf --git_log=\cl --git_status=\cs
```

---

### ~~peco~~ (Removed)

> Replaced by `fzf.fish` which provides superior `Ctrl+R` history search. The `history` abbreviation now maps to `_fzf_search_history`.

---

### zoxide (Directory Jumping)

> A smarter `cd` command, inspired by `z` and `autojump`. Remembers which directories you use most frequently. Replaces `jethrokuan/z`.

| Item | Value |
|------|-------|
| **GitHub** | [ajeetdsouza/zoxide](https://github.com/ajeetdsouza/zoxide) |
| **Version** | `0.9.9` |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/zoxide` |

#### Install

```shell
brew install zoxide
```

Add to `config.fish`:

```fish
zoxide init fish | source
```

#### Usage

```shell
z work        # Jump to most frecent directory matching "work"
zi work       # Interactive selection with fzf
z -           # Jump to previous directory
```

---

### fish-bd (Go Back to Parent)

> Quickly go back to a parent directory in your current working directory tree.

| Item | Value |
|------|-------|
| **GitHub** | [0rax/fish-bd](https://github.com/0rax/fish-bd) |

#### Install

```shell
fisher install 0rax/fish-bd
```

#### Usage

```shell
# You are in /home/user/my/path/is/very/long/
bd path       # Jump back to /home/user/my/path/
bd -s pa      # Starts-with mode
bd -i Pat     # Case-insensitive mode
```

---

### fish_logo (ASCII Art)

> Prints out the fish-shell ASCII-art logo.

| Item | Value |
|------|-------|
| **GitHub** | [laughedelic/fish_logo](https://github.com/laughedelic/fish_logo) |

#### Install

```shell
fisher install laughedelic/fish_logo
```

Used in `fish_greeting.fish` to display a colorful logo on shell startup.

---

### sdkman-for-fish (Java SDK Manager)

> Makes the `sdk` command from SDKMAN! usable from Fish, including auto-completion.

| Item | Value |
|------|-------|
| **GitHub** | [reitzig/sdkman-for-fish](https://github.com/reitzig/sdkman-for-fish) |
| **Version** | v2.1.0 |
| **SDKMAN! version** | script: 5.18.2, native: 0.4.6 |
| **SDKMAN_DIR** | `~/.sdkman` |

#### Install

```shell
brew tap SDKMAN/tap
brew install SDKMAN-cli
fisher install reitzig/sdkman-for-fish@v2.1.0
```

#### Currently active SDKs

| SDK | Version |
|-----|---------|
| Java | 23.0.1-open |
| Kotlin | 2.0.21 |
| Maven | 3.9.9 |
| Spring Boot | 3.3.5 |
| Ki | 0.5.2 |

---

### autopair.fish (Bracket Auto-Completion)

> Auto-complete matching pairs in the Fish command line — like IDE bracket pairing for the terminal.

| Item | Value |
|------|-------|
| **GitHub** | [jorgebucaran/autopair.fish](https://github.com/jorgebucaran/autopair.fish) |

#### Install

```shell
fisher install jorgebucaran/autopair.fish
```

#### Features

- **Auto-insert:** Typing `(` produces `()` with cursor between them
- **Smart deletion:** Backspace on `(|)` removes both brackets
- **Skip-over:** Typing `)` when cursor is before `)` skips instead of doubling
- Works with: `()`, `[]`, `{}`, `""`, `''`

---

### sponge (History Cleanup)

> Automatically removes failed commands and typos from Fish history.

| Item | Value |
|------|-------|
| **GitHub** | [meaningful-ooo/sponge](https://github.com/meaningful-ooo/sponge) |
| **Requires** | Fish 3.2+ |

#### Install

```shell
fisher install meaningful-ooo/sponge
```

Zero configuration needed — works automatically. Commands with non-zero exit codes are removed from history.

#### Configuration (optional)

```fish
# Filter by regex pattern
set -U sponge_regex_patterns "password|token|secret"

# Purge on exit instead of immediately
set -U sponge_purge_only_on_exit true
```

---

### done (Command Notifications)

> Sends a desktop notification when a long-running command finishes and your terminal is not focused.

| Item | Value |
|------|-------|
| **GitHub** | [franciscolourenco/done](https://github.com/franciscolourenco/done) |
| **Notification backend** | `notify-send` (Linux) |

#### Install

```shell
fisher install franciscolourenco/done
```

#### Configuration

```fish
# Minimum command duration to trigger notification (default: 5000ms)
set -U __done_min_cmd_duration 5000

# Exclude specific commands from notifications
set -U __done_exclude '^git (?!push|pull|fetch)'

# Enable notification sound
set -U __done_notify_sound 1
```

---

## Abbreviations

Abbreviations are defined in the `set_abbr` function in `config.fish` and loaded on interactive sessions.

| Abbreviation | Expands To | Description |
|-------------|------------|-------------|
| `code` | `code-insiders` | Open VS Code Insiders |
| `history` | `_fzf_search_history` | Interactive history search with fzf |
| `l` | `eza -lahF` | Detailed file listing (eza) |
| `tree` | `eza --tree` | Tree view of directory |
| `ga` | `git add -v` | Git add (verbose) |
| `gc` | `git commit -S -m` | Git signed commit |
| `gcpast` | `git commit -S --date=format:relative:1.day.ago -m` | Backdated signed commit |

> **Note:** The `copilot` abbreviation (`gh copilot suggest`) is commented out.

---

## Custom Functions

### `clearCaches`

**File:** `~/.config/fish/functions/clearCaches.fish`

Clears Linux kernel caches (page cache, dentries, inodes) and resets swap.

```fish
function clearCaches
    sudo sh -c "echo 3 > '/proc/sys/vm/drop_caches' && swapoff -a && swapon -a"
end
```

Saved with:

```shell
funcsave clearCaches
```

### `fish_greeting`

**File:** `~/.config/fish/functions/fish_greeting.fish`

Displayed every time a new Fish session starts.

```fish
# fish_logo
echo "Hello $(whoami) at $(date)"
fish -v
fisher -v
fish_logo blue cyan green
```

Shows: greeting message, Fish version, Fisher version, and a colorful Fish ASCII logo.

---

## Completions

**Directory:** `~/.config/fish/completions/`

| File | Description |
|------|-------------|
| `abbr.fish` | Fish abbreviation management |
| `apt-get.fish` | APT package manager |
| `bd.fish` | fish-bd plugin (auto-installed) |
| `code.fish` | Visual Studio Code / Code Insiders |
| `curl.fish` | cURL HTTP client |
| `fish_logo.fish` | fish_logo plugin (auto-installed) |
| `fisher.fish` | Fisher plugin manager (auto-installed) |
| `fzf_configure_bindings.fish` | fzf.fish bindings (auto-installed) |
| `gh.fish` | GitHub CLI |
| `sdk.fish` | SDKMAN! (auto-installed) |
| `tide.fish` | Tide prompt (auto-installed) |

> **Note:** Docker fish completion (`docker.fish`) is documented in README.md but has not been generated yet. To generate it:
> ```shell
> docker completion fish > $HOME/.config/fish/completions/docker.fish
> ```

### Manually installed completions

Downloaded from [fish-shell/completions](https://github.com/fish-shell/fish-shell/tree/master/share/completions):

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/abbr.fish > $HOME/.config/fish/completions/abbr.fish
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/apt-get.fish > $HOME/.config/fish/completions/apt-get.fish
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/code.fish > $HOME/.config/fish/completions/code.fish
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/curl.fish > $HOME/.config/fish/completions/curl.fish
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/gh.fish > $HOME/.config/fish/completions/gh.fish
```

---

## Syntax Highlighting / Theme Colors

**File:** `~/.config/fish/conf.d/fish_frozen_theme.fish`  
(Migrated to global scope in Fish 4.3)

| Variable | Value | Description |
|----------|-------|-------------|
| `fish_color_autosuggestion` | `brblack` | Autosuggestion text |
| `fish_color_cancel` | `-r` (reverse) | Cancelled command |
| `fish_color_command` | `blue` | Valid commands |
| `fish_color_comment` | `red` | Comments |
| `fish_color_cwd` | `green` | Current working directory |
| `fish_color_cwd_root` | `red` | CWD when root |
| `fish_color_end` | `green` | Statement terminators (`;`, `&`) |
| `fish_color_error` | `brred` | Errors |
| `fish_color_escape` | `brcyan` | Escape sequences (`\n`, `\x`) |
| `fish_color_history_current` | `--bold` | Current history item |
| `fish_color_host` | `normal` | Hostname |
| `fish_color_host_remote` | `yellow` | Remote hostname (SSH) |
| `fish_color_normal` | `normal` | Default text |
| `fish_color_operator` | `brcyan` | Operators (`*`, `?`) |
| `fish_color_param` | `cyan` | Command parameters |
| `fish_color_quote` | `yellow` | Quoted strings |
| `fish_color_redirection` | `cyan --bold` | Redirections (`>`, `<`, `|`) |
| `fish_color_search_match` | `bryellow --background=brblack` | Search matches |
| `fish_color_selection` | `white --bold --background=brblack` | Selected text |
| `fish_color_status` | `red` | Exit status |
| `fish_color_user` | `brgreen` | Username |
| `fish_color_valid_path` | `--underline` | Valid file paths |
| `fish_pager_color_completion` | `normal` | Pager completions |
| `fish_pager_color_description` | `yellow -i` | Pager descriptions |
| `fish_pager_color_prefix` | `normal --bold --underline` | Pager prefix |
| `fish_pager_color_progress` | `brwhite --background=cyan` | Pager progress |
| `fish_pager_color_selected_background` | `-r` (reverse) | Selected pager item |

---

## Tide Prompt — Full Configuration

All Tide variables are stored as **universal variables** and persist across sessions.

### General

| Variable | Value |
|----------|-------|
| `tide_prompt_add_newline_before` | `true` |
| `tide_prompt_color_frame_and_connection` | `808080` |
| `tide_prompt_color_separator_same_color` | `949494` |
| `tide_prompt_icon_connection` | `·` |
| `tide_prompt_min_cols` | `34` |
| `tide_prompt_pad_items` | `true` |
| `tide_prompt_transient_enabled` | `false` |

### Left prompt

| Variable | Value |
|----------|-------|
| `tide_left_prompt_frame_enabled` | `true` |
| `tide_left_prompt_items` | `vi_mode` `os` `pwd` `git` `newline` |
| `tide_left_prompt_prefix` | `` |
| `tide_left_prompt_separator_diff_color` | `` |
| `tide_left_prompt_separator_same_color` | `` |
| `tide_left_prompt_suffix` | `` |

### Right prompt

| Variable | Value |
|----------|-------|
| `tide_right_prompt_frame_enabled` | `true` |
| `tide_right_prompt_items` | `status` `cmd_duration` `context` `jobs` `direnv` `bun` `node` `python` `rustc` `java` `php` `pulumi` `ruby` `go` `gcloud` `kubectl` `distrobox` `toolbox` `terraform` `aws` `nix_shell` `crystal` `elixir` `zig` `time` |
| `tide_right_prompt_prefix` | `` |
| `tide_right_prompt_separator_diff_color` | `` |
| `tide_right_prompt_separator_same_color` | `` |
| `tide_right_prompt_suffix` | `` |

### Character (prompt symbol)

| Variable | Value |
|----------|-------|
| `tide_character_color` | `5FD700` (green) |
| `tide_character_color_failure` | `FF0000` (red) |
| `tide_character_icon` | `❯` |
| `tide_character_vi_icon_default` | `❮` |
| `tide_character_vi_icon_replace` | `▶` |
| `tide_character_vi_icon_visual` | `V` |

### Vi mode

| Variable | Value |
|----------|-------|
| `tide_vi_mode_bg_color_default` | `949494` |
| `tide_vi_mode_bg_color_insert` | `87AFAF` |
| `tide_vi_mode_bg_color_replace` | `87AF87` |
| `tide_vi_mode_bg_color_visual` | `FF8700` |
| `tide_vi_mode_color_default` | `000000` |
| `tide_vi_mode_color_insert` | `000000` |
| `tide_vi_mode_color_replace` | `000000` |
| `tide_vi_mode_color_visual` | `000000` |
| `tide_vi_mode_icon_default` | `D` |
| `tide_vi_mode_icon_insert` | `I` |
| `tide_vi_mode_icon_replace` | `R` |
| `tide_vi_mode_icon_visual` | `V` |

### Git

| Variable | Value |
|----------|-------|
| `tide_git_bg_color` | `4E9A06` (green) |
| `tide_git_bg_color_unstable` | `C4A000` (yellow) |
| `tide_git_bg_color_urgent` | `CC0000` (red) |
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

### PWD

| Variable | Value |
|----------|-------|
| `tide_pwd_bg_color` | `3465A4` |
| `tide_pwd_color_anchors` | `E4E4E4` |
| `tide_pwd_color_dirs` | `E4E4E4` |
| `tide_pwd_color_truncated_dirs` | `BCBCBC` |
| `tide_pwd_icon` | `` |
| `tide_pwd_icon_home` | `` |
| `tide_pwd_icon_unwritable` | `` |

### Status

| Variable | Value |
|----------|-------|
| `tide_status_bg_color` | `2E3436` |
| `tide_status_bg_color_failure` | `CC0000` |
| `tide_status_color` | `4E9A06` |
| `tide_status_color_failure` | `FFFF00` |
| `tide_status_icon` | `✔` |
| `tide_status_icon_failure` | `✘` |

### Cmd Duration

| Variable | Value |
|----------|-------|
| `tide_cmd_duration_bg_color` | `C4A000` |
| `tide_cmd_duration_color` | `000000` |
| `tide_cmd_duration_decimals` | `0` |
| `tide_cmd_duration_threshold` | `3000` (ms) |

### Time

| Variable | Value |
|----------|-------|
| `tide_time_bg_color` | `D3D7CF` |
| `tide_time_color` | `000000` |
| `tide_time_format` | `%r` (12-hour with AM/PM) |

### OS

| Variable | Value |
|----------|-------|
| `tide_os_bg_color` | `D4D4D4` |
| `tide_os_color` | `E95420` (Ubuntu orange) |
| `tide_os_icon` | `` |

### Context

| Variable | Value |
|----------|-------|
| `tide_context_always_display` | `false` |
| `tide_context_bg_color` | `444444` |
| `tide_context_color_default` | `D7AF87` |
| `tide_context_color_root` | `D7AF00` |
| `tide_context_color_ssh` | `D7AF87` |
| `tide_context_hostname_parts` | `1` |

### Jobs

| Variable | Value |
|----------|-------|
| `tide_jobs_bg_color` | `444444` |
| `tide_jobs_color` | `4E9A06` |
| `tide_jobs_icon` | `` |
| `tide_jobs_number_threshold` | `1000` |

### Language / Tool items

| Item | BG Color | Color | Icon |
|------|----------|-------|------|
| AWS | `FF9900` | `232F3E` | `` |
| Bun | `FBF0DF` | `14151A` | `󰳓` |
| Crystal | `FFFFFF` | `000000` | `` |
| Distrobox | `FF00FF` | `000000` | `󰆧` |
| Docker | `2496ED` | `000000` | `` |
| Elixir | `4E2A8E` | `000000` | `` |
| GCloud | `4285F4` | `000000` | `󰊭` |
| Go | `00ACD7` | `000000` | `` |
| Java | `ED8B00` | `000000` | `` |
| Kubectl | `326CE5` | `000000` | `󱃾` |
| Nix Shell | `7EBAE4` | `000000` | `` |
| Node | `44883E` | `000000` | `` |
| PHP | `617CBE` | `000000` | `` |
| Pulumi | `F7BF2A` | `000000` | `` |
| Python | `444444` | `00AFAF` | `󰌠` |
| Ruby | `B31209` | `000000` | `` |
| Rust | `F74C00` | `000000` | `` |
| Terraform | `800080` | `000000` | `󱁢` |
| Toolbox | `613583` | `000000` | `` |
| Zig | `F7A41D` | `000000` | `` |

---

## Key Bindings

Fish 4.3 migrated key bindings from universal to global scope.  
**File:** `~/.config/fish/conf.d/fish_frozen_key_bindings.fish`

### Default Fish bindings (preset)

| Key | Action |
|-----|--------|
| `Enter` | Execute command |
| `Tab` | Complete |
| `Ctrl+C` | Cancel command line |
| `Ctrl+D` | Exit shell |
| `Ctrl+U` | Kill line backward |
| `Ctrl+P` / `Up` | Previous history |
| `Ctrl+N` / `Down` | Next history |
| `Ctrl+F` / `Right` | Forward char (overridden by fzf directory search) |
| `Ctrl+B` / `Left` | Backward char |

### fzf.fish bindings (custom)

| Key | Action |
|-----|--------|
| `Ctrl+R` | Search command history |
| `Ctrl+Alt+V` | Search environment variables |
| `Ctrl+F` | Search directories (customized) |
| `Ctrl+L` | Search git log (customized) |
| `Ctrl+S` | Search git status (customized) |

---

## Environment Variables & PATH

### PATH (in order)

```
/home/linuxbrew/.linuxbrew/bin
/home/linuxbrew/.linuxbrew/sbin
/home/shinyay/.nvm/versions/node/v22.22.0/bin
/home/shinyay/.local/bin
/home/shinyay/.sdkman/candidates/springboot/current/bin
/home/shinyay/.sdkman/candidates/maven/current/bin
/home/shinyay/.sdkman/candidates/kotlin/current/bin
/home/shinyay/.sdkman/candidates/ki/current/bin
/home/shinyay/.sdkman/candidates/java/current/bin
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
```

### Key environment variables

| Variable | Value | Scope |
|----------|-------|-------|
| `SHELL` | `/home/linuxbrew/.linuxbrew/bin/fish` | global |
| `HOMEBREW_PREFIX` | `/home/linuxbrew/.linuxbrew` | global |
| `SDKMAN_DIR` | `/home/shinyay/.sdkman` | global |
| `fish_user_paths` | `/home/shinyay/.nvm/versions/node/v22.22.0/bin` | universal |
| `VIRTUAL_ENV_DISABLE_PROMPT` | `true` | universal (set by Tide) |
| `Z_DATA` | `~/.local/share/z/data` | universal (set by z plugin) |
| `Z_CMD` | `z` | universal |

### conf.d/uv.env.fish

```fish
source "$HOME/.local/bin/env.fish"
```

Loads the [uv](https://github.com/astral-sh/uv) Python package manager environment. `uv` is an extremely fast Python package and project manager written in Rust.

### NVM / Node.js Integration

Node.js is managed via [NVM](https://github.com/nvm-sh/nvm) (v0.40.1). The active Node.js binary path is added to `fish_user_paths` as a universal variable:

```
fish_user_paths = /home/shinyay/.nvm/versions/node/v22.22.0/bin
```

| Tool | Version |
|------|---------|
| Node.js | v22.22.0 |
| npm | 10.9.4 |

**Global npm packages:**

| Package | Version |
|---------|---------|
| `@marp-team/marp-cli` | 4.2.3 |
| `corepack` | 0.34.0 |

---

## Supporting CLI Tools

| Tool | Version | Installed via | Binary |
|------|---------|--------------|--------|
| `fish` | 4.5.0 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/fish` |
| `fisher` | 4.4.8 | Fisher | `~/.config/fish/functions/fisher.fish` |
| `fzf` | 0.68.0 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/fzf` |
| `bat` | 0.26.1 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/bat` |
| `fd` | 10.3.0 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/fd` |
| `peco` | ~~0.5.11~~ | ~~Homebrew~~ | Removed (replaced by fzf.fish) |
| `zoxide` | 0.9.9 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/zoxide` |
| `git` | 2.53.0 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/git` |
| `gh` | 2.87.2 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/gh` |
| `jq` | 1.8.1 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/jq` |
| `helm` | 4.1.0 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/helm` |
| `kubectl` | 1.35.0 | Homebrew | `/home/linuxbrew/.linuxbrew/bin/kubectl` |

---

## Misc / Cleanup Notes

### ~~Stale temporary files in `~/.config/fish/`~~ ✅ Cleaned

The following leftover temp files have been removed (2026-02-21):

- `fish_variablesKVhQGicCtv`, `fish_variablesUTAdZScUNs`, `fish_variableslaMTd8ikBP`, `fish_variablesqctenvqm0x`, `fish_variableszjlr134h9G`
- `fishd.tmp.CB4lfO`, `fishd.tmp.E2corw`, `fishd.tmp.xsNiKE`
- `functions/fish_prompt.fish.bak`

### `~/git.fish`

A large (~200KB) git completion file exists at `~/git.fish`. This appears to be a standalone fish completion script for git. If it is not being sourced or referenced, it may be unnecessary since Fish ships with built-in git completions.
