# Windows 11 Pro - 22H2

---
# 1. Initial Setup
## VSCode

> Visual Studio Code, often referred to as VSCode, is a free and open-source code editor optimized for building and debugging modern web and cloud applications. It supports various programming languages and comes with features like IntelliSense for smart completions based on variable types, function definitions, and imported modules, built-in Git commands, and debugging tools. It's highly customizable and extensible with various extensions. It's available on multiple platforms including Linux, macOS, and Windows.

- [x] Installation

```shell
winget search vscode
winget install Microsoft.VisualStudioCode
```

### VSCode Extension

- [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
- [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
- [Python extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- [Jupyter Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter)

### Dev Container CLI

- [Dev Container CLI](https://code.visualstudio.com/docs/devcontainers/devcontainer-cli)

Install Dev Container CLI.

1. Open VSCode
2. Select the **Dev Containers: Install devcontainer CLI** command from the Command Palette (`F1`).

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

Install WSL.

```shell
wsl --install
```

### WSL Setup

Update and Upgrade packages.

- [x] Configuration

```shell
sudo apt update && sudo apt upgrade
```

Install requred packages.

```shell
sudo apt-get install unzip zip
sudo apt-get install fontconfig
sudo apt-get install wget ca-certificates
sudo apt-get install wslu
sudo apt-get install build-essential
sudo apt-get install libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev libffi-dev liblzma-dev tk-dev uuid-dev xz-utils libxml2-dev libxmlsec1-dev libdb-dev libgdbm-dev
```

### `wslu` - A collection of utilities for WSL

> This is a collection of utilities for the Linux Subsystem for Windows (WSL), such as converting Linux paths to Windows paths or creating Linux application shortcuts on the Windows Desktop.

- [GitHub: wslutilities/wslu](https://github.com/wslutilities/wslu)
- [wslu Wiki](https://wslutiliti.es/wslu/)

```shell
dpkg -L wslu | grep bin/
```

## Homebrew

> Homebrew is an open-source software package manager that makes it easier to install software on macOS (Apple's operating system) and Linux. Basically, a package manager's job is to find and install the right software packages that will allow you to compile and run various apps/software on your specific operating system.

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

```shell
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

### Fetch configuration

- [x] Configuration

Before fetching, remove any remote-tracking references that no longer exist on the remote.

```shell
git config --global fetch.prune true
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
echo 'eval (/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >> ~/.config/fish/config.fish
```

Check `brew`.

```shell
brew doctor
```

### Fisher

> Fisher is a plugin manager for the Fish shell. It lets you install, update, and remove plugins like a boss. You can take control of functions, completions, bindings, and snippets from the command line. Fisher's zero impact on shell startup keeps your shell zippy and responsive1. Fisher is 100% pure-Fish, making it easy to contribute or modify.

- [x] Installation

- [Fisher](https://nicedoc.io/jorgebucaran/fisher)

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

- [x] Installation

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/git.fish > $HOME/.config/fish/completions/git.fish
```

## Docker

- [x] Installation

```powershell
winget search docker
winget install Docker.DockerDesktop
```

### Docker for Visual Studio Code

> 

- [x] Installation

- [Docker for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

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

## Google Japanese IME

```shell
winget install Google.JapaneseIME
```

## Slack

> Slack is a messaging app for businesses that connects people to the information they need. It allows people to work as one unified team, transforming the way organizations communicate.

- [x] Installation

```shell
winget search slack
winget install slacktechnologies.slack
```

## JDownloader 2

- [JDownloader](https://jdownloader.org/home/index)

> JDownloader is a free, open-source download management tool with a huge community that makes downloading as easy and fast as it should be.

- [x] Installation

```shell
winget search jdownloader
winget install AppWork.JDownloader
```

## Zoom

```shell
winget search zoom
winget install Zoom.Zoom
```


## Google Chrome

- [x] Installation

```shell
winget search chrome
winget install Google.Chrome
```

## QuickLook

- [QL-Win/QuickLook](https://github.com/QL-Win/QuickLook)

> One of the few features I missed from macOS is Quick Look. It allows users to peek into a file content in lightning speed by just pressing the Space key.

- [x] Installation

```shell
winget search QuickLook
winget install QL-Win.QuickLook
```

## Rufus

- [Rufus](https://rufus.ie/en/)

> Create bootable USB drives the easy way

- [x] Installation

```shell
winget install Rufus.Rufus
```

# 3. Visual Studio Code

## Settings - Everything starts from `Ctrl` + `,`

### `Text Editor`

#### Render Line Highligh

- Controls how the editor render the current line highlight: `all`

### `Text Editor` - `Files`

#### Insert Final Newline

When Enabled, insert a final new line at the end of the file saving it.

- [x] Enabled

#### Trim Trailling Whitespace

When enabled, will trim trailling whitespace when saving a file.

- [x] Enabled

### `Text Editor` - `Cursor`

#### Cursor Blinking

- Control the cursor animation style: `expand`

#### Cursor Smooth Caret Animation

Control whether the sooth caret animation be enabled.

- [x] Enabled

#### Cursor Style

- Control the cursor style: `block`

