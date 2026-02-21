# Development Tools — Detailed Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-21

---

## Table of Contents

- [Overview](#overview)
- [Node.js / NVM](#nodejs--nvm)
  - [NVM Installation](#nvm-installation)
  - [Node.js & npm](#nodejs--npm)
  - [Global npm Packages](#global-npm-packages)
- [Python / uv](#python--uv)
  - [System Python](#system-python)
  - [uv (Python Package Manager)](#uv-python-package-manager)
- [Kubernetes Tools](#kubernetes-tools)
  - [kubectl](#kubectl)
  - [Helm](#helm)
  - [VS Code Kubernetes Extensions](#vs-code-kubernetes-extensions)
- [Azure Tools](#azure-tools)
  - [Azure CLI](#azure-cli)
  - [Azure Developer CLI (azd)](#azure-developer-cli-azd)
  - [Azure ML Extension](#azure-ml-extension)
- [AI / Copilot Tools](#ai--copilot-tools)
  - [Copilot CLI](#copilot-cli)
  - [Cline](#cline)
- [Marp CLI (Markdown Presentations)](#marp-cli-markdown-presentations)
- [GnuCOBOL](#gnucobol)
- [Chromium](#chromium)
- [Utility Tools](#utility-tools)
  - [jq (JSON Processor)](#jq-json-processor)
  - [tmux (Terminal Multiplexer)](#tmux-terminal-multiplexer)
  - [Vim (Text Editor)](#vim-text-editor)
  - [bat (Cat with Syntax Highlighting)](#bat-cat-with-syntax-highlighting)
  - [fd (Fast Find Alternative)](#fd-fast-find-alternative)
  - [fzf (Fuzzy Finder)](#fzf-fuzzy-finder)
- [Homebrew — Full Package Inventory](#homebrew--full-package-inventory)
  - [Formulae](#formulae)
  - [Casks](#casks)
- [Snap — Full Package Inventory](#snap--full-package-inventory)

---

## Overview

All development tools installed in this WSL2 environment, with version, install method, and purpose.

| Tool | Version | Installed via | Purpose |
|------|---------|---------------|---------|
| **NVM** | 0.40.1 | install script | Node.js version manager |
| **Node.js** | v22.22.0 | NVM | JavaScript runtime |
| **npm** | 10.9.4 | NVM (bundled) | Node.js package manager |
| **Python** | 3.12.3 | Ubuntu (system) | Python runtime |
| **uv** | latest | standalone installer | Fast Python package manager |
| **kubectl** | latest | Homebrew | Kubernetes CLI |
| **Helm** | v4.1.1 | Homebrew | Kubernetes package manager |
| **Azure CLI** | 2.75.0 | Homebrew / apt | Azure management CLI |
| **azd** | 1.23.6 | Homebrew | Azure Developer CLI |
| **Copilot CLI** | 0.0.414 | Homebrew (cask) | AI-powered CLI assistant |
| **Marp CLI** | 4.2.3 | npm (global) | Markdown → slides |
| **GnuCOBOL** | 3.1.2 | apt | COBOL compiler |
| **Chromium** | latest | Snap | Web browser |
| **jq** | 1.8.1 | Homebrew | JSON processor |
| **tmux** | 3.4 | Ubuntu (system) | Terminal multiplexer |
| **Vim** | 9.1 | Ubuntu (system) | Text editor |
| **bat** | 0.26.1 | Homebrew | Syntax-highlighted cat |
| **fd** | 10.3.0 | Homebrew | Fast find alternative |
| **fzf** | 0.68.0 | Homebrew | Fuzzy finder |

---

## Node.js / NVM

### NVM Installation

> NVM (Node Version Manager) lets you install and switch between multiple Node.js versions.

| Item | Value |
|------|-------|
| **Version** | `0.40.1` |
| **Install dir** | `~/.nvm` |
| **GitHub** | [nvm-sh/nvm](https://github.com/nvm-sh/nvm) |

#### Install NVM

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

#### Fish Shell Integration

NVM is designed for bash/zsh. In Fish, the active Node.js binary path is added to `fish_user_paths` as a universal variable:

```
fish_user_paths = /home/shinyay/.nvm/versions/node/v22.22.0/bin
```

This ensures `node`, `npm`, `npx`, and globally installed packages are available in Fish without needing a bash-compat wrapper.

#### Useful commands (run in bash)

```shell
nvm install --lts            # Install latest LTS
nvm install 22               # Install specific major version
nvm use 22                   # Switch to Node 22
nvm alias default 22         # Set default version
nvm ls                       # List installed versions
nvm ls-remote --lts          # List available LTS versions
```

---

### Node.js & npm

| Item | Value |
|------|-------|
| **Node.js version** | `v22.22.0` |
| **npm version** | `10.9.4` |
| **Binary (node)** | `~/.nvm/versions/node/v22.22.0/bin/node` |
| **Binary (npm)** | `~/.nvm/versions/node/v22.22.0/bin/npm` |

#### Verify

```shell
node --version    # v22.22.0
npm --version     # 10.9.4
```

---

### Global npm Packages

Installed globally via `npm install -g`:

| Package | Version | Description |
|---------|---------|-------------|
| `@marp-team/marp-cli` | 4.2.3 | Markdown presentation CLI (see [Marp CLI](#marp-cli-markdown-presentations)) |
| `corepack` | 0.34.0 | Zero-install package manager manager (pnpm, yarn) |

#### List global packages

```shell
npm list -g --depth=0
```

#### Install a global package

```shell
npm install -g <package-name>
```

---

## Python / uv

### System Python

| Item | Value |
|------|-------|
| **Version** | `3.12.3` |
| **Binary** | `/usr/bin/python3` |
| **Installed via** | Ubuntu (system package) |
| **pyenv** | **Not installed** |

#### Verify

```shell
python3 --version    # Python 3.12.3
which python3        # /usr/bin/python3
```

---

### uv (Python Package Manager)

> An extremely fast Python package and project manager, written in Rust. Replacement for `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `virtualenv`, and more.

| Item | Value |
|------|-------|
| **Binary** | `~/.local/bin/uv` |
| **Installed via** | Standalone installer |
| **GitHub** | [astral-sh/uv](https://github.com/astral-sh/uv) |

#### Install

```shell
curl -LsSf https://astral.sh/uv/install.sh | sh
```

#### Fish Shell Integration

**File:** `~/.config/fish/conf.d/uv.env.fish`

```fish
source "$HOME/.local/bin/env.fish"
```

This sources the uv environment file, which adds `~/.local/bin` to PATH and configures uv shell completion.

#### Usage

```shell
uv init my-project           # Create a new Python project
uv add requests              # Add a dependency
uv run python main.py        # Run within the project environment
uv venv                      # Create a virtual environment
uv pip install flask         # pip-compatible install
uv python install 3.13       # Install a Python version
uv tool install ruff         # Install a CLI tool globally
```

---

## Kubernetes Tools

### kubectl

> The Kubernetes command-line tool for managing clusters.

| Item | Value |
|------|-------|
| **Installed via** | Homebrew (`kubernetes-cli`) |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/kubectl` |

#### Install

```shell
brew install kubernetes-cli
```

#### Useful commands

```shell
kubectl version --client          # Show client version
kubectl cluster-info              # Display cluster info
kubectl get pods -A               # List all pods in all namespaces
kubectl get nodes                 # List cluster nodes
kubectl apply -f manifest.yaml    # Apply a manifest
kubectl logs <pod-name>           # View pod logs
kubectl exec -it <pod> -- bash    # Shell into a pod
kubectl config get-contexts       # List available contexts
kubectl config use-context <ctx>  # Switch context
```

---

### Helm

> The package manager for Kubernetes — install, upgrade, and manage Kubernetes applications.

| Item | Value |
|------|-------|
| **Version** | `v4.1.1` |
| **Installed via** | Homebrew |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/helm` |

#### Install

```shell
brew install helm
```

#### Useful commands

```shell
helm version                      # Show Helm version
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update                  # Update chart repositories
helm search repo nginx            # Search for charts
helm install my-release bitnami/nginx    # Install a chart
helm list                         # List installed releases
helm upgrade my-release bitnami/nginx    # Upgrade a release
helm uninstall my-release         # Uninstall a release
helm template my-release bitnami/nginx   # Render templates locally
```

---

### VS Code Kubernetes Extensions

| Extension ID | Description |
|-------------|-------------|
| `ms-kubernetes-tools.vscode-kubernetes-tools` | Kubernetes cluster explorer, manifest editing, debugging |
| `ms-kubernetes-tools.vscode-aks-tools` | Azure Kubernetes Service (AKS) management |

> See `README-VSCode.md` for the full VS Code extension inventory.

---

## Azure Tools

### Azure CLI

> The Azure command-line interface for managing Azure resources.

| Item | Value |
|------|-------|
| **Version** | `2.75.0` |
| **Installed via** | Homebrew and apt (both available) |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/az` |

#### Install

```shell
# Via Homebrew (recommended)
brew install azure-cli

# Via apt (alternative)
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

#### Useful commands

```shell
az version                        # Show CLI version and extensions
az login                          # Login to Azure (browser flow)
az login --use-device-code        # Login via device code (WSL)
az account show                   # Show current subscription
az account list -o table          # List all subscriptions
az group list -o table            # List resource groups
az aks list -o table              # List AKS clusters
az aks get-credentials -g <rg> -n <cluster>   # Get kubectl credentials
```

---

### Azure Developer CLI (azd)

> Developer-centric CLI for building, deploying, and managing Azure applications using templates.

| Item | Value |
|------|-------|
| **Version** | `1.23.6` |
| **Installed via** | Homebrew |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/azd` |

#### Install

```shell
brew install azd
```

#### Useful commands

```shell
azd version                       # Show version
azd init                          # Initialize a new project from a template
azd up                            # Provision infrastructure + deploy application
azd deploy                        # Deploy application code only
azd provision                     # Provision Azure infrastructure only
azd down                          # Delete all Azure resources
azd env list                      # List environments
azd monitor                       # Open Application Insights dashboard
```

---

### Azure ML Extension

> Azure CLI extension for Azure Machine Learning.

| Item | Value |
|------|-------|
| **Version** | `2.37.2` |
| **Installed via** | `az extension add` |

#### Install

```shell
az extension add -n ml
```

#### Useful commands

```shell
az ml workspace list -o table             # List ML workspaces
az ml compute list -g <rg> -w <ws>        # List compute targets
az ml job create -f job.yml               # Submit a training job
az ml model list -g <rg> -w <ws>          # List registered models
az ml online-endpoint list -g <rg> -w <ws>  # List online endpoints
```

---

## AI / Copilot Tools

### Copilot CLI

> AI-powered CLI assistant by GitHub. Provides terminal-based AI assistance for commands, code, and explanations.

| Item | Value |
|------|-------|
| **Version** | `0.0.414` |
| **Installed via** | Homebrew (cask, prerelease) |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/copilot` |

#### Install

```shell
brew install --cask copilot-cli@prerelease
```

#### Verify

```shell
copilot --version    # 0.0.414
which copilot        # /home/linuxbrew/.linuxbrew/bin/copilot
```

#### Usage

```shell
copilot              # Start interactive Copilot CLI session
```

---

### Cline

> AI coding assistant for VS Code — autonomous code editing agent powered by Claude / GPT models.

| Item | Value |
|------|-------|
| **Config directory** | `~/Cline/` |
| **Subdirectories** | `Rules/`, `Workflows/` |

#### Directory structure

```
~/Cline/
├── Rules/        # Custom rules for Cline behavior
└── Workflows/    # Workflow definitions
```

#### VS Code extensions

| Extension ID | Description |
|-------------|-------------|
| `rooveterinaryinc.roo-cline` | Roo Cline — forked Cline variant |
| `saoudrizwan.claude-dev` | Claude Dev (original Cline) |

---

## Marp CLI (Markdown Presentations)

> Create slide decks from Markdown files. Supports HTML, PDF, PPTX, and image output.

| Item | Value |
|------|-------|
| **Version** | `4.2.3` |
| **Installed via** | npm (global) |
| **Binary** | `~/.nvm/versions/node/v22.22.0/bin/marp` |
| **GitHub** | [marp-team/marp-cli](https://github.com/marp-team/marp-cli) |
| **VS Code extension** | `marp-team.marp-vscode` |

#### Install

```shell
npm install -g @marp-team/marp-cli
```

#### Usage

```shell
marp slide.md                     # Convert to HTML
marp slide.md --pdf               # Convert to PDF
marp slide.md --pptx              # Convert to PowerPoint
marp slide.md --images png        # Convert to PNG images
marp -s slide.md                  # Start preview server
marp --theme custom.css slide.md  # Use custom CSS theme
```

#### Slide syntax (in Markdown)

```markdown
---
marp: true
theme: default
paginate: true
---

# Slide 1

Content here

---

# Slide 2

- Bullet point
- Another point
```

---

## GnuCOBOL

> A free/open-source COBOL compiler — transpiles COBOL to C, then compiles with GCC.

| Item | Value |
|------|-------|
| **Version** | `3.1.2` |
| **Installed via** | apt (`gnucobol3`) |
| **Binary** | `cobc` |

#### Install

```shell
sudo apt install gnucobol3
```

#### Verify

```shell
cobc --version    # cobc (GnuCOBOL) 3.1.2
```

#### Usage

```shell
# Compile and run a COBOL program
cobc -x -o hello hello.cob    # Compile to executable
./hello                        # Run

# Compile to object file
cobc -c hello.cob              # Compile only

# Compile as a module (shared library)
cobc -m module.cob             # Build shared module
```

---

## Chromium

> Open-source web browser, installed via Snap.

| Item | Value |
|------|-------|
| **Installed via** | Snap |
| **Binary** | `/snap/bin/chromium` |

#### Install

```shell
sudo snap install chromium
```

#### Launch

```shell
chromium                       # Open Chromium browser
chromium --headless --dump-dom https://example.com   # Headless mode
```

---

## Utility Tools

### jq (JSON Processor)

> Lightweight and flexible command-line JSON processor — like `sed` for JSON data.

| Item | Value |
|------|-------|
| **Version** | `1.8.1` |
| **Installed via** | Homebrew |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/jq` |
| **Website** | [jqlang.org](https://jqlang.org/) |

#### Install

```shell
brew install jq
```

#### Usage

```shell
# Pretty-print JSON
echo '{"name":"Alice","age":30}' | jq .

# Extract a field
echo '{"name":"Alice","age":30}' | jq '.name'    # "Alice"

# Filter arrays
echo '[1,2,3,4,5]' | jq '[.[] | select(. > 3)]'  # [4,5]

# Transform API responses
curl -s https://api.github.com/users/octocat | jq '{login, id, bio}'

# Process NDJSON (newline-delimited)
cat data.jsonl | jq -c '.status'
```

---

### tmux (Terminal Multiplexer)

> Terminal multiplexer — split panes, create windows, detach and reattach sessions.

| Item | Value |
|------|-------|
| **Version** | `3.4` |
| **Installed via** | Ubuntu (system) |
| **Binary** | `/usr/bin/tmux` |

#### Install

```shell
# Already available as a system package
sudo apt install tmux
```

#### Usage

```shell
tmux                              # Start new session
tmux new -s work                  # Start named session
tmux ls                           # List sessions
tmux attach -t work               # Reattach to session
tmux kill-session -t work         # Kill a session
```

#### Key bindings (default prefix: `Ctrl+B`)

| Key | Action |
|-----|--------|
| `Ctrl+B` `c` | Create new window |
| `Ctrl+B` `n` | Next window |
| `Ctrl+B` `p` | Previous window |
| `Ctrl+B` `%` | Split pane vertically |
| `Ctrl+B` `"` | Split pane horizontally |
| `Ctrl+B` `d` | Detach from session |
| `Ctrl+B` `x` | Kill current pane |
| `Ctrl+B` `[` | Enter scroll/copy mode |
| `Ctrl+B` `z` | Toggle pane zoom |
| `Ctrl+B` `o` | Cycle through panes |

---

### Vim (Text Editor)

> Vi IMproved — the ubiquitous terminal text editor. Configured as the default git editor.

| Item | Value |
|------|-------|
| **Version** | `9.1` |
| **Installed via** | Ubuntu (system) |
| **Binary** | `/usr/bin/vim` |
| **Git integration** | `git config --global core.editor vim` |

#### Verify

```shell
vim --version | head -1    # VIM - Vi IMproved 9.1
git config --global core.editor    # vim
```

#### Useful commands

```shell
vim file.txt               # Open a file
vim +42 file.txt           # Open at line 42
vim -d file1 file2         # Diff mode
vim -R file.txt            # Read-only mode
```

---

### bat (Cat with Syntax Highlighting)

> A `cat` clone with syntax highlighting, line numbers, and git integration.

| Item | Value |
|------|-------|
| **Version** | `0.26.1` |
| **Installed via** | Homebrew |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/bat` |
| **GitHub** | [sharkdp/bat](https://github.com/sharkdp/bat) |

#### Install

```shell
brew install bat
```

#### Usage

```shell
bat file.py                       # View with syntax highlighting
bat -l json data.txt              # Force language highlighting
bat --style=numbers file.py       # Show line numbers only
bat -A file.txt                   # Show non-printable characters
bat --diff file.py                # Show git diff decorations
bat src/*.rs                      # View multiple files
bat -p file.py                    # Plain mode (no decorations)
```

#### Integration with other tools

```shell
# Used by fzf.fish for previews
# Used by man pages (if configured)
export BAT_THEME="TwoDark"        # Set color theme
bat --list-themes                 # List available themes
```

---

### fd (Fast Find Alternative)

> A simple, fast, and user-friendly alternative to `find`.

| Item | Value |
|------|-------|
| **Version** | `10.3.0` |
| **Installed via** | Homebrew |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/fd` |
| **GitHub** | [sharkdp/fd](https://github.com/sharkdp/fd) |

#### Install

```shell
brew install fd
```

#### Usage

```shell
fd                                # List all files recursively
fd README                         # Find files matching "README"
fd -e py                          # Find all .py files
fd -e py -x wc -l                 # Count lines in all Python files
fd -H .gitignore                  # Include hidden files
fd -t d src                       # Find directories matching "src"
fd -t f -s 'README.md'            # Case-sensitive exact match
fd --exclude node_modules         # Exclude directories
fd -e log --changed-within 1d     # Files changed in last day
```

#### Integration with fzf.fish

`fd` is used by `fzf.fish` as the default file finder backend (instead of `find`), providing faster and more intuitive directory/file search via `Ctrl+F`.

---

### fzf (Fuzzy Finder)

> A general-purpose command-line fuzzy finder — interactive filtering for any list of text.

| Item | Value |
|------|-------|
| **Version** | `0.68.0` |
| **Installed via** | Homebrew |
| **Binary** | `/home/linuxbrew/.linuxbrew/bin/fzf` |
| **GitHub** | [junegunn/fzf](https://github.com/junegunn/fzf) |

#### Install

```shell
brew install fzf
```

#### Usage

```shell
# Interactive file selection
vim (fzf)                         # Open selected file in Vim

# Filter any input
ls | fzf                          # Select from file list
git log --oneline | fzf           # Select a commit
ps aux | fzf                      # Select a process

# Preview files while searching
fzf --preview 'bat --color=always {}'

# Multi-select
fzf --multi                       # Tab to select multiple items
```

#### Fish integration

fzf is primarily used through the `fzf.fish` Fisher plugin. See [README-Fish.md](README-Fish.md#fzffish-fuzzy-finder) for keybinding configuration.

---

## Homebrew — Full Package Inventory

| Item | Value |
|------|-------|
| **Prefix** | `/home/linuxbrew/.linuxbrew` |
| **Formulae count** | 53 |
| **Cask count** | 1 |

### Formulae

All Homebrew formulae installed on this system (53 packages):

| # | Package | Description |
|---|---------|-------------|
| 1 | `autoconf` | Automatic configure script builder |
| 2 | `azure-cli` | Azure command-line interface |
| 3 | `azd` | Azure Developer CLI |
| 4 | `bat` | Cat clone with syntax highlighting |
| 5 | `bzip2` | Compression utility |
| 6 | `ca-certificates` | Mozilla's CA certificate bundle |
| 7 | `curl` | HTTP client |
| 8 | `expat` | XML parser library |
| 9 | `fd` | Simple, fast find alternative |
| 10 | `fish` | Friendly Interactive Shell |
| 11 | `fzf` | Fuzzy finder |
| 12 | `gcc` | GNU Compiler Collection |
| 13 | `gettext` | Internationalization utilities |
| 14 | `gh` | GitHub CLI |
| 15 | `git` | Distributed version control |
| 16 | `gmp` | GNU Multiple Precision Arithmetic |
| 17 | `helm` | Kubernetes package manager |
| 18 | `isl` | Integer Set Library |
| 19 | `jq` | JSON processor |
| 20 | `kubernetes-cli` | Kubernetes CLI (kubectl) |
| 21 | `libcrypt` | Library for one-way data hashing |
| 22 | `libevent` | Asynchronous event library |
| 23 | `libgit2` | Git library (C implementation) |
| 24 | `libidn2` | Internationalized domain names library |
| 25 | `libnghttp2` | HTTP/2 C library |
| 26 | `libpsl` | Public Suffix List library |
| 27 | `libssh2` | SSH2 library |
| 28 | `libunistring` | Unicode string library |
| 29 | `libxml2` | XML parser |
| 30 | `linux-headers@5.15` | Linux kernel headers |
| 31 | `lz4` | Fast compression algorithm |
| 32 | `m4` | Macro processor |
| 33 | `mpc` | Complex number arithmetic library |
| 34 | `mpfr` | Multiple-precision floating-point |
| 35 | `ncurses` | Terminal handling library |
| 36 | `oniguruma` | Regular expression library |
| 37 | `openssl@3` | TLS/SSL toolkit |
| 38 | `pcre2` | Regular expression library (Perl-compatible) |
| 39 | `pkg-config` | Compiler flag configuration tool |
| 40 | `readline` | Library for command-line editing |
| 41 | `sdkman-cli` | Software Development Kit Manager |
| 42 | `sqlite` | Embedded SQL database |
| 43 | `unzip` | Archive decompression |
| 44 | `utf8proc` | Unicode processing library |
| 45 | `xz` | Data compression (LZMA) |
| 46 | `zip` | Archive compression |
| 47 | `zlib` | Compression library |
| 48 | `zoxide` | Smarter cd command |
| 49 | `zstd` | Fast compression algorithm |
| 50 | `glib` | GLib core application library |
| 51 | `libyaml` | YAML parser library |
| 52 | `python@3.12` | Python (Homebrew build) |
| 53 | `ruby` | Ruby programming language |

> **Note:** Run `brew list --formula` to verify the current list. Some packages are dependencies installed automatically.

### Casks

| # | Cask | Description |
|---|------|-------------|
| 1 | `copilot-cli@prerelease` | GitHub Copilot CLI (prerelease) |

> Run `brew list --cask` to verify.

---

## Snap — Full Package Inventory

All Snap packages installed on this system:

| Package | Description |
|---------|-------------|
| `bare` | Base snap (empty root filesystem) |
| `chromium` | Open-source web browser |
| `code-insiders` | VS Code Insiders (editor) |
| `core20` | Core snap (Ubuntu 20.04 base) |
| `core22` | Core snap (Ubuntu 22.04 base) |
| `core24` | Core snap (Ubuntu 24.04 base) |
| `cups` | Common Unix Printing System |
| `snapd` | Snap daemon (package manager) |

#### List installed snaps

```shell
snap list
```

#### Install / Remove

```shell
sudo snap install <package>       # Install a snap
sudo snap remove <package>        # Remove a snap
sudo snap refresh                 # Update all snaps
sudo snap refresh <package>       # Update a specific snap
```
