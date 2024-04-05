# Windows 11 Pro - 23H2 (UM790Pro)

---
# 1. Initial Setup
## App Installer

If `winget` does not work, you should upgrade `App Installer`

- [x] Installation

- [App Installer](https://apps.microsoft.com/detail/9nblggh4nns1)

## Google Japanese IME

- [x] Installation

```shell
winget install Google.JapaneseIME
```

### Keymap Configuration

1. **Properties** > **Keymap Stype** > **Customize** > Convert `Hankaku/Zenkaku` to `Ctrl` + `Space`
2. Remove the followings:
  - **Composition** / **Insert half-width space**
  - **Coversion** / **Insert half-width space**
3. You can use new keymaps after opening a new application

## WSL

> WSL2 is a **Windows Subsystem for Linux** that allows access to Linux tools and applications directly from the Windows environment12. It offers the best of both worlds by allowing you to run Windows apps, like Visual Studio, alongside a Linux shell for easier command line access2. WSL2 uses a virtual machine, and uses a full Linux kernel built and shipped with Windows23. With WSL2, you can build docker images that paravirtualize your GPU4.

- [x] Installation

<img width="348" alt="Screenshot 2024-01-08 144911" src="https://github.com/shinyay/win-setup/assets/3072734/821dcbc2-0c8d-412d-8414-c26e63d74fdc">

### Prepare for installation

`Run as Administrator` **PowerShell** and enable the following features:

- VirtualMachinePlatform 
- Microsoft-Windows-Subsystem-Linux
- HypervisorPlatform

```shell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:HypervisorPlatform /all /norestart
```

And restart

```shell
Restart-Computer -Force
```

### WSL Installation

```shell
wsl --install
```

Upgrade packages

```shell
sudo apt update && sudo apt upgrade
```

### WSL Setup

#### Hostname

- [x] Configuration

```shell
sudo vim /etc/wsl.conf
```

Add the followings:

```shell
[network]
hostname=wsl
```

Then you should shutdown WSL with PowerShell.

```shell
wsl --shutdown
```

Run your WSL again, now your configuration is enabled.

#### Other WSL Settings

|SECTION|ITEM|SETTING|NOTE|
|-------|----|-------|----|
|`boot`|`systemd`|`true`|Enable **systemd** support|
|`network`|`hostname`|`<YOUR_HOSTNAME>`|Hostname for your WSL Instance|
|`interop`|`appendWindowsPath`|`false` / `true`|Apped your Windows PATH or not|
|`user`|`default`|`<YOUR_USERNAME>`|WSL Login Username|

## Terminal

Terminal settings:

### Startup

- [x] Configuration

- **Default Profile**: `Ubuntu`
- **Default Terminal Application**: `Terminal`

## Homebrew

> Homebrew is an open-source software package manager that makes it easier to install software on macOS (Apple's operating system) and Linux. Basically, a package manager's job is to find and install the right software packages that will allow you to compile and run various apps/software on your specific operating system.

- [x] Installation

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
```

```shell
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/shinyay/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
sudo apt-get install build-essential
brew install gcc
brew doctor
```

## Git

- [x] Installation

```shell
brew install git
```

```shell
git --version
git version 2.44.0
```

### Git Global Configuration

- [x] Configuration

```shell
git config --global user.name "shinyay" && \
git config --global user.email "<YOUR_MAILADDRESS>" && \
git config --global core.quotepath false && \
git config --global core.safecrlf true && \
git config --global core.autocrlf false && \
git config --global core.editor 'vim -c "set fenc=utf-8"' && \
git config --global color.diff auto && \
git config --global color.status auto && \
git config --global color.branch auto && \
git config --global fetch.prune true && \
git config pull.ff only
```

### SSH Key for GitHub

#### Fine-grained personal access tokens

- [ ] [Generate new token](https://github.com/settings/tokens?type=beta)
  - Repository access: `All repositories`
  - Permissions - Repository permissions - Contents: `Read and write`

#### ssh-keygen

- [x] Generate SSH Key pair

```shell
ssh-keygen -t ed25519 -C 'mail address for github'
cat $HOME/.ssh/id_ed25519.pub | clip.exe
```

#### SSH Keys on GitHub

- [x] Add a public key on the following site:

- [https://github.com/settings/keys](https://github.com/settings/keys)

## VSCode

> Visual Studio Code, often referred to as VSCode, is a free and open-source code editor optimized for building and debugging modern web and cloud applications. It supports various programming languages and comes with features like IntelliSense for smart completions based on variable types, function definitions, and imported modules, built-in Git commands, and debugging tools. It's highly customizable and extensible with various extensions. It's available on multiple platforms including Linux, macOS, and Windows.

- [x] Installation

```shell
winget search vscode
winget install Microsoft.VisualStudioCode
```

### VSCode Extension

- [x] Installation

> The Remote Development extension pack allows you to open any folder in a container, on a remote machine, or in the Windows Subsystem for Linux (WSL) and take advantage of VS Code's full feature set. Since this lets you set up a full-time development environment anywhere.


```shell
sudo apt-get install wget ca-certificates
```

- [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

The following extesions are also installed:

- [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
- [Remote - SSH: Editing Configuration Files](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh-edit)
- [Remote -Tunnels](https://marketplace.visualstudio.com/items?itemName=ms-vscode.remote-server)
- [Remote Explorer](https://marketplace.visualstudio.com/items?itemName=ms-vscode.remote-explorer)
- [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

# 2. WSL Customization

## WSL

Update and Upgrade packages.

- [ ] Configuration

```shell
sudo apt update && sudo apt upgrade
brew update && brew upgrade
```

### Requred Packages Installation

#### Basic Packages

- [x] Installation

- zip
- unzip

```shell
brew install zip unzip
```

#### Fontconfig

- [x] Installation

> Fontconfig is a library designed to provide system-wide font configuration, customization and application access. 

- [fontconfig](https://en.wikipedia.org/wiki/Fontconfig)

```shell
sudo apt-get install fontconfig
```

#### wslu

- [x] Installation

> This is a collection of utilities for the Windows Subsystem for Linux (WSL), such as converting Linux paths to Windows paths or creating Linux application shortcuts on the Windows Desktop.

- [wslu](https://github.com/wslutilities/wslu)
  - [Guide](https://wslutiliti.es/wslu/)

```shell
sudo apt-get install wslu
```

Commands list:

```shell
dpkg -L wslu | grep bin/
```

## Git

### Git Alias

List aliases:

```shell
git config --get-regexp ^alias\.
```

Alias for Alias list

- [x] Configuration

```shell
git config --global alias.alias 'config --get-regexp ^alias\.'
```

#### `git status`

- [x] Configuration

```shell
git config --global alias.st 'status --short
```

#### `git log`

- [x] Configuration

```shell
git config --global alias.plog "log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=iso"
git config --global alias.glog "log --pretty='format:%C(yellow)%h %C(green)%cd %C(reset)%s %C(red)%d %C(cyan)[%an]' --date=format:'%c' --all --graph"
```

#### Commit Count

- [x] Configuration

```shell
git config --global alias.count 'shortlog -e -s -n'
```

## GitHub CLI

- [x] Installation

> GitHub CLI, or gh, is a command-line interface to GitHub for use in your terminal or your scripts.

- [GitHub CLI](https://cli.github.com/)

```shell
brew install gh
```

```shell
gh --version

gh version 2.46.0 (2024-03-20)
https://github.com/cli/cli/releases/tag/v2.46.0
```

### GitHub Authentication

```shell
gh auth login

? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations on this host? SSH
? Upload your SSH public key to your GitHub account? $HOME/.ssh/id_ed25519.pub
? Title for your SSH key: GitHub CLI
? How would you like to authenticate GitHub CLI? Login with a web browser
```

Text editor program to use for authoring text

```shell
gh config set editor vim
```

## Fish

> Fish, or the "Friendly Interactive SHell", is a Unix shell designed with an emphasis on user-friendliness and interactive use. It was introduced in 2005 and has since gained a following due to its unique features, helpful defaults, and focus on a pleasant user experience.

![image](https://github.com/shinyay/win-setup/assets/3072734/183c576c-1737-4f3b-bd3b-5d16b03d0575)

- [Fish](https://fishshell.com/)

### Fish Install

- [x] Installation

```shell
brew install fish
```

Check the PATH for `fish`.

```shell
which fish

/home/linuxbrew/.linuxbrew/bin/fish
```

Write the PATH for `fish` to define the valid shell.

```shell
sudo sh -c "echo /home/linuxbrew/.linuxbrew/bin/fish >> /etc/shells"
```

Define `fish` as a Default Shell.

```shell
chsh -s /home/linuxbrew/.linuxbrew/bin/fish
```

Reboot WSL and check `fish`.

```shell
echo $SHELL
```

Define the PATH of `brew` on `fish`.

```shell
echo 'eval (/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> $HOME/.config/fish/config.fish
```

Check `brew doctor`.

```shell
brew doctor
```

### Fisher

> Fisher is a plugin manager for the Fish shell. It lets you install, update, and remove plugins like a boss. You can take control of functions, completions, bindings, and snippets from the command line. Fisher's zero impact on shell startup keeps your shell zippy and responsive1. Fisher is 100% pure-Fish, making it easy to contribute or modify.

- [x] Installation

- [Fisher](https://github.com/jorgebucaran/fisher)

```shell
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

```shell
fisher -v
fisher, version 4.4.4
```

### Fish Prompt - Tide

![tide](https://github.com/IlanCosman/tide/raw/assets/images/logo.svg)

- [x] Installation

> The ultimate Fish prompt.

- [IlanCosman/tide](https://github.com/IlanCosman/tide)
- [tide - Configuration Wiki](https://github.com/IlanCosman/tide/wiki/Configuration)

```shell
tide configure --auto --style=Rainbow --prompt_colors='True color' --show_time='12-hour format' --rainbow_prompt_separators=Round --powerline_prompt_heads=Round --powerline_prompt_tails=Round --powerline_prompt_style='Two lines, character and frame' --prompt_connection=Disconnected --powerline_right_prompt_frame=Yes --prompt_connection_andor_frame_color=Darkest --prompt_spacing=Sparse --icons='Many icons' --transient=Yes
```

### Fish Theme - bobthefish

If you install `tide`, you can't install bobthefish

- [ ] Installation

- [oh-my-fish/theme-bobthefish](https://github.com/oh-my-fish/theme-bobthefish)

```shell
fisher install oh-my-fish/theme-bobthefish
```

#### bobthefish Configuration

- [ ] Configuration

```shell
vim $HOME/.config/fish/config.fish

set -g theme_display_git_master_branch yes
set -g theme_display_git yes
set -g theme_display_git_dirty yes
set -g theme_display_git_untracked yes
set -g theme_display_git_ahead_verbose yes
set -g theme_display_git_dirty_verbose yes
set -g theme_display_git_stashed_verbose yes
set -g theme_git_worktree_support no
set -g theme_use_abbreviated_branch_name no

set -g theme_display_vagrant no
set -g theme_display_docker_machine no
set -g theme_display_k8s_context no
set -g theme_display_hg no
set -g theme_display_virtualenv no
set -g theme_display_nix no
set -g theme_display_ruby no
set -g theme_display_nvm no
set -g theme_display_user ssh
set -g theme_display_hostname ssh
set -g theme_display_vi no
set -g theme_display_date yes
set -g theme_display_cmd_duration yes

set -g theme_title_display_process no
set -g theme_title_display_path yes
set -g theme_title_display_user no
set -g theme_title_use_abbreviated_path no

set -g theme_date_format "+%F %H:%M"
set -g theme_date_timezone Asia/Tokyo
set -g theme_avoid_ambiguous_glyphs yes
set -g theme_powerline_fonts yes
set -g theme_nerd_fonts no
set -g theme_show_exit_status yes
set -g theme_display_jobs_verbose yes
set -g default_user your_normal_user
set -g theme_color_scheme loght
set -g fish_prompt_pwd_dir_length 0
set -g theme_project_dir_length 1
set -g theme_newline_cursor yes
set -g theme_newline_prompt ''
```

### Moralerspace font

<img width="271" alt="image" src="https://github.com/shinyay/win-setup/assets/3072734/9d6adc03-11ba-42be-99b3-14d99b7845dc">

- [Moralerspace](https://github.com/yuru7/moralerspace)

```shell
mkdir download && cd download
curl -L -O https://github.com/yuru7/moralerspace/releases/download/v1.0.0/MoralerspaceHWJPDOC_v1.0.0.zip
curl -L -O https://github.com/yuru7/moralerspace/releases/download/v1.0.0/MoralerspaceHWNF_v1.0.0.zip
curl -L -O https://github.com/yuru7/moralerspace/releases/download/v1.0.0/MoralerspaceHW_v1.0.0.zip
curl -L -O https://github.com/yuru7/moralerspace/releases/download/v1.0.0/MoralerspaceJPDOC_v1.0.0.zip
curl -L -O https://github.com/yuru7/moralerspace/releases/download/v1.0.0/MoralerspaceNF_v1.0.0.zip
curl -L -O https://github.com/yuru7/moralerspace/releases/download/v1.0.0/Moralerspace_v1.0.0.zip
unzip MoralerspaceHWJPDOC_v1.0.0.zip
unzip MoralerspaceHWNF_v1.0.0.zip
unzip MoralerspaceHW_v1.0.0.zip
unzip MoralerspaceJPDOC_v1.0.0.zip
unzip MoralerspaceNF_v1.0.0.zip
unzip Moralerspace_v1.0.0.zip
sudo mkdir  /usr/share/fonts/truetype/moralerspace
sudo cp MoralerspaceHWJPDOC_v1.0.0/*.ttf /usr/share/fonts/truetype/moralerspace/
sudo cp MoralerspaceHWNF_v1.0.0/*.ttf /usr/share/fonts/truetype/moralerspace/
sudo cp MoralerspaceHW_v1.0.0/*.ttf /usr/share/fonts/truetype/moralerspace/
sudo cp MoralerspaceJPDOC_v1.0.0/*.ttf /usr/share/fonts/truetype/moralerspace/
sudo cp MoralerspaceNF_v1.0.0/*.ttf /usr/share/fonts/truetype/moralerspace/
sudo cp Moralerspace_v1.0.0/*.ttf /usr/share/fonts/truetype/moralerspace/
sudo fc-cache -vf
fc-list | grep -i moralerspace
explorer.exe .
```

Then install `*ttf`.

1. Open settings: `ctrl` + `,`
2. Select **Ubuntu**
3. Select **Aditional settings** > **Appearance**
4. Choose **Moralerspace Radon NF** from **Font face**

Then delete downloaded fonts.

```shell
cd ..
rm -fr download
```

### Cica font

- [Cica](https://github.com/miiton/Cica)

- [ ] Installation

## Fish Plugin

### Peco
- [x] Installation

> Simplistic interactive filtering tool

By pushing `ctrl` + `r`, you can search shell history

- [oh-my-fish/plugin-peco](https://github.com/oh-my-fish/plugin-peco)

```shell
brew install peco
fisher install oh-my-fish/plugin-peco
abbr -a history peco_select_history
```

### fzf

#### fzf

- [x] Installation `fzf`

> `fzf` is a general-purpose command-line fuzzy finder.

- [fzf](https://github.com/junegunn/fzf)

```shell
brew install fzf
```

#### bat

- [x] Installation bat

> A cat(1) clone with syntax highlighting and Git integration.

- [bat](https://github.com/sharkdp/bat)

```shell
brew install bat
```

#### fd

- [x] Installation fd

> `fd` is a program to find entries in your filesystem. It is a simple, fast and user-friendly alternative to `find`.

- [fd](https://github.com/sharkdp/fd)

```shell
brew install fd
```

#### fzf for Fish

- [x] Installation fzf

> Augment your Fish command line with mnemonic key bindings to efficiently find what you need using fzf.
> Use fzf.fish to interactively find and insert file paths, git commit hashes, and other entities into your command line. Tab to select multiple entries. If you trigger a search while your cursor is on a word, that word will be used to seed the fzf query and will be replaced by your selection. All searches include a preview of the entity hovered over to help you find what you're looking for.

- [fzf](https://github.com/PatrickF1/fzf.fish)

```shell
fisher install PatrickF1/fzf.fish

echo "### fzf.fish" | tee -a $HOME/.config/fish/config.fish
echo "fzf_configure_bindings --directory=\cf --git_log=\cl --git_status=\cs" | tee -a $HOME/.config/fish/config.fish
echo "" | tee -a $HOME/.config/fish/config.fish
```

|Key bind|Description|
|--------|-----------|
|`Ctrl`+`r`|Command history|
|`Ctrl`+`Alt`+`v`|Environment variables|
|`Ctrl`+`Alt`+`f`|Recursive listing|
|`Ctrl`+`Alt`+`s`|`git status`|
|`Ctrl`+`Alt`+`l`|`git log`|

### z

- [x] Installation

By z, it tracks the directory you have visited

- [jethrokuan/z](https://github.com/jethrokuan/z)

```shell
fisher install jethrokuan/z
```

### fish-bd

- [x] Installation

By bd, you can quickly go back to a parent directory in your current working directory tree

- [0rax/fish-bd](https://github.com/0rax/fish-bd)

```shell
fisher install 0rax/fish-bd
```

### fish_logo

- [x] Installation

> This plugin adds a function to print out the fish-shell ASCII-art logo.

- [laughedelic/fish_logo](https://github.com/laughedelic/fish_logo)

![fishlogo](https://gist.githubusercontent.com/laughedelic/b7d5e572b0a35afd51fd40a2d9eef66b/raw/blue-base16-solarized.dark.png)

```shell
fisher install laughedelic/fish_logo
```

The description in `fish_greeting.fish` is executed when you log in to fish.

```shell
vim $HOME/.config/fish/functions/fish_greeting.fish
```

Add the followings:

```shell
    # fish_logo
    echo "Hello Fish!"
    fish_logo blue cyan green
```

### SDKMAN! for fish

- [ ] Installation

Makes command sdk from SDKMAN! usable from fish, including auto-completion. Also adds binaries from installed SDKs to the PATH.

- [reitzig/sdkman-for-fish](https://github.com/reitzig/sdkman-for-fish)

```shell
fisher install reitzig/sdkman-for-fish@v2.0.0

sdk
You don't seem to have SDKMAN! installed. Install now? [y/N] y
```

And then open a new terminal/shell to load SDKMAN!.

```shell
sdk version

SDKMAN!
script: 5.18.2
native: 0.4.6
```

## Fish Completion

- [Fish Completion List](https://github.com/fish-shell/fish-shell/tree/master/share/completions)

### abbr

- [x] Installation

> `abbr` - manage fish abbreviations

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/abbr.fish > $HOME/.config/fish/completions/abbr.fish
```

### apt-get

- [x] Installation

> `apt-get` - APT package handling utility -- command-line interface

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/apt-get.fish > $HOME/.config/fish/completions/apt-get.fish
```

### bd

- [x] Installation

> `bd` - Quickly go back to a parent directory up in your current working directory tree

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/bd.fish > $HOME/.config/fish/completions/bd.fish
```

### code

- [x] Installation

> `code` - Visual Studio Code

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/code.fish > $HOME/.config/fish/completions/code.fish
```

### curl

- [x] Installation

> `curl` - transfer a URL

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/curl.fish > $HOME/.config/fish/completions/curl.fish
```

### docker

- [x] Installation

After Docker installation, you can do the following:

```shell
docker completion fish > $HOME/.config/fish/completions/docker.fish
```

### git

- [x] Installation

> `git` - the stupid content tracker

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/git.fish > $HOME/.config/fish/completions/git.fish
```

## Docker

### Docker Desktop for Windows

- [ ] Installation

```powershell
winget search docker
winget install Docker.DockerDesktop
```

### Docker Engine for Ubuntu

- [x] Installation

#### Uninstall old versions

```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; sudo apt-get remove $pkg; end
```

#### Set up Docker's apt repository

```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
set arch (dpkg --print-architecture)
set codename (awk -F= '/VERSION_CODENAME/ {print $2}' /etc/os-release | tr -d '"')
echo "deb [arch=$arch signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $codename stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

#### Install the Docker packages

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Manage Docker as a non-root user

> The Docker daemon binds to a Unix socket, not a TCP port. By default it's the root user that owns the Unix socket, and other users can only access it using sudo. The Docker daemon always runs as the root user.
> If you don't want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

```shell
sudo usermod -aG docker $USER
```

Reboot your WSL.

```shell
wsl --shutdown
```

Activate the changes to groups

```shell
newgrp docker
```

#### Check Docker Service running by Systemd

```shell
systemctl list-units --type=service --all
```

or

```shell
systemctl list-units --type=service --state=active 
```

You can see like the below:

```shell
  :
  dmesg.service                                          loaded    inactive dead    Save initial kernel messages af>
  docker.service                                         loaded    active   running Docker Application Container En>
  dpkg-db-backup.service                                 loaded    inactive dead    Daily dpkg database backup serv>
  :
```

#### Shutdown Docker Service

```shell
sudo systemctl stop docker
```

#### Start Docker Service

```shell
sudo systemctl start docker
```

### Docker for Visual Studio Code

> The Docker extension makes it easy to build, manage, and deploy containerized applications from Visual Studio Code. It also provides one-click debugging of Node.js, Python, and .NET inside a container.

- [ ] Installation

- [Docker for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)


## Dev Container CLI

- [ ] Installation

- [Dev Container CLI](https://code.visualstudio.com/docs/devcontainers/devcontainer-cli)

Install Dev Container CLI.

1. Open VSCode
2. Select the **Dev Containers: Install devcontainer CLI** command from the Command Palette (`F1`).

## Azure CLI

- [ ] Installation

> The Azure command-line interface (Azure CLI) is a set of commands used to create and manage Azure resources. The Azure CLI is available across Azure services and is designed to get you working quickly with Azure, with an emphasis on automation.

- [Azure Command-Line Interface (CLI) documentation](https://learn.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)
- [How to install the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

```shell
brew update && brew install azure-cli
```

# 3. Terminal Customization

## Startup

- **Default Profile**: `Ubuntu`
- **Default Terminal Application**: `Windows Terminal`
- **Launch Size**:
  - **Columns**: `110`
  - **Rows**: `20`

## Interaction

- Remove trailing white-space in recangular selection: On
  - > You can select in rectangular with `Alt` + Mouse select

## Appearance

- **Show acrylic tab row**: `On`

## Ubuntu - Appearance

- **Transparency - Background opacity**: `90%`
- **Window - Scrollbar Visibility**: `Hidden`


# 4. Visual Studio Code

**Settings** - Everything starts from `Ctrl` + `,`

## `Text Editor` - `Formatting`

### Format On Paste

Controls whether the editor should automatically format the pasted content. A formatter must be available and the formatter should be able to format a range in a document.

- [x] Enabled

### Format On Type

Controls whether the editor should automatically format the line after typing.

- [x] Enabled

#### Minimap: Enabled

Controls whether the minimap is shown.

- [ ] Disabled

#### Render Control Characters

Controls whether the editor should render control characters.

- [ ] Enabled

#### Render Line Highlight

- Controls how the editor render the current line highlight: `all`

### `Text Editor` - `Files`

#### Insert Final Newline

When Enabled, insert a final new line at the end of the file saving it.

- [ ] Enabled

#### Trim Trailing Whitespace

When enabled, will trim trailing whitespace when saving a file.

- [ ] Enabled

### `Text Editor` - `Cursor`

#### Cursor Blinking

- Control the cursor animation style: `expand`

#### Cursor Smooth Caret Animation

Control whether the sooth caret animation be enabled.

- [ ] Enabled

#### Cursor Style

- Control the cursor style: `block`

### `Workbench` - `Zen Mode`

#### Center Layout

Controls whether turning on Zen Mode also centers the layout.

- [ ] Disabled

#### Hide Line Numbers

Controls whether turning on Zen Mode also hides the editor line numbers.

- [ ] Disabled

#### Hide Status Bar

Controls whether turning on Zen Mode also hides the status bar at the bottom of the workbench.

- [ ] Disabled

## Profile

> VS Code profiles provide a way to organize and isolate customizations within the editor. A profile represents a specific set of configurations that can be easily activated or deactivated. With profiles, users can maintain separate configurations for different projects or teams, to help ensure a seamless transition between development environments.

### Create Profile

![create-profile](https://github.com/shinyay/win-setup/assets/3072734/430da389-8cb3-40b2-b82f-5f9932575398)

# etc

## Python

### pyenv

Simple Python Version Management: pyenv

> pyenv lets you easily switch between multiple versions of Python. It's simple, unobtrusive, and follows the UNIX tradition of single-purpose tools that do one thing well.

- pyenv
  - <https://github.com/pyenv/pyenv>

```shell
brew install pyenv
```

### Python

- [Release Info](https://www.python.org/downloads/)

List all available versions.

```shell
pyenv install --list
```

Install Python latest version (as of Jan 9, 2024)

```shell
pyenv install 3.12.1
```

If you failed the build, you should run the following command.

```shell
brew unlink pkg-config
# brew link pkg-config
brew reinstall pkg-config
```

Set up the fish shell for Python

```shell
vim ~/.config/fish/config.fish
```

```shell
### Python
set -Ux PYENV_ROOT $HOME/.pyenv
fish_add_path $PYENV_ROOT/bin
pyenv init - | source
```

```shell
source $HOME/.config/fish/config.fish
```

```shell
pyenv global 3.12.1
python --version

Python 3.12.1
```

# 2. Windows Tools

## Steam

- [ ] Installation

```shell
winget install Valve.Steam
```

## Git for Windows

- [ ] Installation

```shell
winget install --id Git.Git -e --source winget
```

## Slack

> Slack is a messaging app for businesses that connects people to the information they need. It allows people to work as one unified team, transforming the way organizations communicate.

- [ ] Installation

```shell
winget search slack
winget install slacktechnologies.slack
```

## JDownloader 2

- [JDownloader](https://jdownloader.org/home/index)

> JDownloader is a free, open-source download management tool with a huge community that makes downloading as easy and fast as it should be.

- [ ] Installation

```shell
winget search jdownloader
winget install AppWork.JDownloader
```

## Streamlabs Desktop

- [Streamlabs Desktop](https://streamlabs.com/streamlabs-live-streaming-software)

> Streamlabs Desktop has everything you need to stream and create a memorable brand.

- [x] Installation

```shell
winget search streamlabs
winget install Streamlabs.Streamlabs
```

## Spotify

- [Spotify](https://www.spotify.com/download/windows/)

> Play your favorite songs, podcasts and albums free on Windows with Spotify.

- [ ] Installation

```shell
winget search spotify
winget install Spotify.Spotify
```

## Zoom

```shell
winget search zoom
winget install Zoom.Zoom
```


## Google Chrome

- [ ] Installation

```shell
winget search chrome
winget install Google.Chrome
```

## QuickLook

- [QL-Win/QuickLook](https://github.com/QL-Win/QuickLook)

> One of the few features I missed from macOS is Quick Look. It allows users to peek into a file content in lightning speed by just pressing the Space key.

- [ ] Installation

```shell
winget search QuickLook
winget install QL-Win.QuickLook
```

## Rufus

- [Rufus](https://rufus.ie/en/)

> Create bootable USB drives the easy way

- [ ] Installation

```shell
winget install Rufus.Rufus
```

## Multipass

- [Multipass](https://multipass.run/)

> Get an instant Ubuntu VM with a single command. Multipass can launch and run virtual machines and configure them with cloud-init like a public cloud.

- [ ] Installation

```shell
winget search multipass
winget install Canonical.Multipass
```

## FastCopy

- [ ] Installation

```shell
winget search fastcopy
winget install FastCopy.FastCopy
```

## Anki

- [Anki](https://apps.ankiweb.net/)

> Anki is a program which makes remembering things easy.

- [ ] Installation

```shell
winget search anki
winget install Anki.Anki
```

## ShurePlus MOTIV

- [MOTIV Desktop App](https://www.shure.com/ja-JP/products/software/shure_plus_motiv_desktop)

> Adjust mic gain, monitor mix, EQ, limiter, compressor and more. MV7 users have the additional option of enabling Auto Level Mode; a ‘set it and forget it’ application for consistent recordings every time.

- [ ] Installation

```shell
winget search ankimotiv
winget install Shure.ShurePlusMOTIV
```
