# Windows 11 Pro - 22H2

---
# 1. Initial Setup
## VSCode

- [x] Installation

```shell
winget search vscode
winget install Microsoft.VisualStudioCode
```

- [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
- [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

## WSL

> WSL2 is a **Windows Subsystem for Linux** that allows access to Linux tools and applications directly from the Windows environment12. It offers the best of both worlds by allowing you to run Windows apps, like Visual Studio, alongside a Linux shell for easier command line access2. WSL2 uses a virtual machine, and uses a full Linux kernel built and shipped with Windows23. With WSL2, you can build docker images that paravirtualize your GPU4.

- [x] Installation

Enable **Windows Subsystem for Linux**

<img width="348" alt="Screenshot 2024-01-08 144911" src="https://github.com/shinyay/win-setup/assets/3072734/821dcbc2-0c8d-412d-8414-c26e63d74fdc">

WSL version might not be the latest.

```shell
wsl --verion

WSL version: 2.0.9.0
Kernel version: 5.15.133.1-1
WSLg version: 1.0.59
MSRDC version: 1.2.4677
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.25131.1002-220531-1700.rs-onecore-base2-hyp
Windows version: 10.0.22621.2861
```

Update WSL.

```shell
wsl --update
```

```shell
wsl --version

WSL version: 2.0.14.0
Kernel version: 5.15.133.1-1
WSLg version: 1.0.59
MSRDC version: 1.2.4677
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.25131.1002-220531-1700.rs-onecore-base2-hyp
Windows version: 10.0.22621.2861
```



```shell
wsl --install
```

### WSL Setup

Update and Upgrade packages

- [x] Configuration

```shell
sudo apt update && sudo apt upgrade
```

```shell
sudo apt-get install unzip zip
sudo apt-get install fontconfig
sudo apt-get install wget ca-certificates
sudo apt-get install wslu
sudo apt-get install build-essential
```

### `wslu` - A collection of utilities for WSL

> This is a collection of utilities for the Linux Subsystem for Windows (WSL), such as converting Linux paths to Windows paths or creating Linux application shortcuts on the Windows Desktop.

- [GitHub: wslutilities/wslu](https://github.com/wslutilities/wslu)
- [wslu Wiki](https://wslutiliti.es/wslu/)

```shell
dpkg -L wslu | grep bin/
```

## Homebrew

- [x] Installation

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
```

```shell
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
brew doctor
```

## Git

- [x] Installation

```shell
sudo add-apt-repository ppa:git-core/ppa
sudo apt update && sudo apt upgrade
```

```shell
git --version
git version 2.43.0
```

### Git Global Configuration

- [x] Configuration

```shell
git config --global user.name "shinyay" && \
git config --global user.email "" && \
git config --global core.quotepath false && \
git config --global core.safecrlf true && \
git config --global core.autocrlf false && \
git config --global core.editor 'vim -c "set fenc=utf-8"' && \
git config --global color.diff auto && \
git config --global color.status auto && \
git config --global color.branch auto && \
git config pull.ff only
```

### SSH Key for GitHub

#### Fine-grained personal access tokens

- [x] [Generate new token](https://github.com/settings/tokens?type=beta)
  - Repository access: `All repositories`
  - Permissions - Repository permissions - Contents: `Read and write`

#### ssh-keygen
- [x] Generate SSH Key pair

```
ssh-keygen -t ed25519 -C 'mail address for github'
cat $HOME/.ssh/id_ed25519.pub | clip.exe
```

#### SSH Keys on GitHub
- [x] Add a public key on the following site:

- [https://github.com/settings/keys](https://github.com/settings/keys)


### Git Alias

#### `git status`

- [x] Configuration

```shell
git config --global alias.st status
```

## GitHub CLI

> GitHub CLI, or gh, is a command-line interface to GitHub for use in your terminal or your scripts.

- [GitHub CLI](https://cli.github.com/)

```shell
brew install gh
```

```shell
gh --version

gh version 2.41.0 (2024-01-08)
https://github.com/cli/cli/releases/tag/v2.41.0
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

By the folloing command allows you to configures `git`` to use GitHub CLI as a credential helper

```shell
gh auth setup-git
```

## Fish
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
echo 'eval (/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> ~/.config/fish/config.fish
```

Check `brew`.

```shell
brew doctor
```

### Fisher
A plugin manager for Fish (https://nicedoc.io/jorgebucaran/fisher)

- [x] Installation

```shell
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

```shell
fisher -v
fisher, version 4.4.4
```

### Fish Theme - bobthefish

- [x] Installation

- [oh-my-fish/theme-bobthefish](https://github.com/oh-my-fish/theme-bobthefish)

```shell
fisher install oh-my-fish/theme-bobthefish
```

#### bobthefish Configuration

- [x] Configuration

```shell
vim ~/.config/fish/config.fish

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

### Cica font for bobthefish

- [x] Installation

```shell
curl -L -O https://github.com/miiton/Cica/releases/download/v5.0.3/Cica_v5.0.3.zip
unzip Cica_v5.0.3.zip -d temp
sudo mkdir  /usr/share/fonts/truetype/cica
sudo cp temp/*.ttf /usr/share/fonts/truetype/cica/
sudo fc-cache -vf
fc-list | grep -i cica
explorer.exe ./temp/
```

Then install `*ttf`.

## Fish Plugin

### Peco
- [x] Installation

By pushing ctrl + r, you can search shell history

- [oh-my-fish/plugin-peco](https://github.com/oh-my-fish/plugin-peco)

```shell
brew install peco
fisher install oh-my-fish/plugin-peco
abbr -
```

### fzf

Interactice command search

- [fzf.fish](https://github.com/PatrickF1/fzf.fish)

- [x] Installation fzf

```shell
brew install fzf
```


- [x] Installation bat
- [bat](https://github.com/sharkdp/bat)
cat clone with syntax highlight and Git integration

```shell
brew install bat
```

- [x] Installation fd
- [fd](https://github.com/sharkdp/fd)
find entries in your filesystem

```shell
brew install fd
```

- [x] Installation fzf
- [fzf](https://github.com/PatrickF1/fzf.fish)
interactive find 

```shell
fisher install PatrickF1/fzf.fish

echo "### fzf.fish" | tee -a $HOME/.config/fish/config.fish
echo "fzf_configure_bindings --directory=\cf --git_log=\cl --git_status=\cs" | tee -a $HOME/.config/fish/config.fish
echo "" | tee -a $HOME/.config/fish/config.fish
```

|Key bind|Description|
|--------|-----------|
|`Ctrl`+`r`|Command history|
|`Ctrl`+`v`|Environment variables|
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

### SDKMAN! for fish

- [x] Installation

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

### Fish Completion

- [Fish Completion List](https://github.com/fish-shell/fish-shell/tree/master/share/completions)

#### git

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/git.fish > $HOME/.config/fish/completions/git.fish
```