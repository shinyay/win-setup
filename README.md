# Windows 11 Enterprise - 23H2 (T14s)

---
# 1. Initial Setup
## App Installer

![image](https://github.com/shinyay/win-setup/assets/3072734/4b96ae8b-761c-4e38-9bb8-9b72b6ceb25b)

If `winget` does not work, you should upgrade `App Installer`

- [x] Installation

- [App Installer](https://apps.microsoft.com/detail/9nblggh4nns1)

### `winget source`

- [x] Installation

If you cannot search the target item with `winget search`, confirm `winget source list`

```shell
winget source list
Name    Argument                                      Explicit
--------------------------------------------------------------
msstore https://storeedgefd.dsx.mp.microsoft.com/v9.0 false
```

If you cannot find the source target, `winget`, then you should add it.

Run Terminal as Administrator and add the target to source:

```shell
winget source add -n winget -a https://winget.azureedge.net/cache
```

```shell
winget source list
```

```shell
Name    Argument                                      Explicit
--------------------------------------------------------------
winget  https://winget.azureedge.net/cache            false
msstore https://storeedgefd.dsx.mp.microsoft.com/v9.0 false
```

## Microsoft Terminal Preview

- [x] Installation

```shell
winget search 'Windows Terminal'
winget install Microsoft.WindowsTerminal.Preview
```

## Microsoft Terminal Canary

- [x] Installation

Install Microsoft Terminal Canary from the following site:
- [Microsoft Terminal Canary](https://github.com/microsoft/terminal#installing-windows-terminal-canary)

## ULE4JIS / alt-ime-ahk

If your keyaboard layout is **Japanese**, you can try the following tools when you use the US Keyboad layout.

- [x] Installation

- [ULE4JIS](https://www.vector.co.jp/soft/dl/winnt/util/se476294.html)
  - [ULE4JIS-GitHub](https://github.com/dezz/ULE4JIS/tree/master/publish)
- [alt-ime-ahk](https://github.com/karakaram/alt-ime-ahk)

I extracted to `C:<USER_DIR>\Tools\MyToolsForStartup`

It is useful that you create shortcuts and deploy the `Startup` folder.

```shell
shell:startup
```

## Google Japanese IME

- [ ] Installation

```shell
winget install Google.JapaneseIME
```

### Keymap Configuration

1. **Properties** > **Keymap Stype** > **Customize** > Convert `Hankaku/Zenkaku` to `Ctrl` + `Space`
2. Remove the followings:
  - **Composition** / **Insert half-width space**
  - **Coversion** / **Insert half-width space**
3. You can use new keymaps after opening a new application

## PowerShell

- [x] Installation

Check your PowerShell Version

```shell
$PSVersionTable
```

```shell
Name                           Value
----                           -----
PSVersion                      5.1.22621.2506
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.22621.2506
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

If your PowerShell version is not latest, you should upgrade

```shell
winget search Microsoft.PowerShell
```

```shell
winget search Microsoft.PowerShell
Name               Id                           Version Source
---------------------------------------------------------------
PowerShell         Microsoft.PowerShell         7.4.2.0 winget
PowerShell Preview Microsoft.PowerShell.Preview 7.5.0.2 winget
```

```shell
winget install --id Microsoft.Powershell
winget install --id Microsoft.Powershell.Preview
```

## WSL

> WSL2 is a **Windows Subsystem for Linux** that allows access to Linux tools and applications directly from the Windows environment12. It offers the best of both worlds by allowing you to run Windows apps, like Visual Studio, alongside a Linux shell for easier command line access2. WSL2 uses a virtual machine, and uses a full Linux kernel built and shipped with Windows23. With WSL2, you can build docker images that paravirtualize your GPU4.

- [x] Installation

<img width="348" alt="Turn Windows features on or off" src="https://github.com/shinyay/win-setup/assets/3072734/f419e143-f766-4e5d-9763-a53ea07c4a73">

<img width="348" alt="Turn Windows features on or off" src="https://github.com/shinyay/win-setup/assets/3072734/820ae88d-62c7-4405-bfb6-17510e1a44d7">

### Prepare for installation

It is also possible to activate WSL from the command line below without activating it from the GUI.

- [ ] Installation

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

- [x] Installation

Update WSL

```shell
wsl --update
```

Check the available distributions

```shell
wsl --list --online
```

```shell
NAME                                   FRIENDLY NAME
Ubuntu                                 Ubuntu
Debian                                 Debian GNU/Linux
kali-linux                             Kali Linux Rolling
Ubuntu-18.04                           Ubuntu 18.04 LTS
Ubuntu-20.04                           Ubuntu 20.04 LTS
Ubuntu-22.04                           Ubuntu 22.04 LTS
Ubuntu-24.04                           Ubuntu 24.04 LTS
OracleLinux_7_9                        Oracle Linux 7.9
OracleLinux_8_7                        Oracle Linux 8.7
OracleLinux_9_1                        Oracle Linux 9.1
openSUSE-Leap-15.5                     openSUSE Leap 15.5
SUSE-Linux-Enterprise-Server-15-SP4    SUSE Linux Enterprise Server 15 SP4
SUSE-Linux-Enterprise-15-SP5           SUSE Linux Enterprise
```

Install the latest Ubuntu.

```shell
wsl --install Ubuntu-24.04
```

```shell
Installing: Ubuntu 24.04 LTS
Ubuntu 24.04 LTS has been installed.
Launching Ubuntu 24.04 LTS...
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: shinyay
New password:
Retype new password:
passwd: password updated successfully
Installation successful!
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

Welcome to Ubuntu 24.04 LTS (GNU/Linux 5.15.146.1-microsoft-standard-WSL2 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sun Apr 28 13:57:02 JST 2024

  System load:  0.66                Processes:             28
  Usage of /:   0.1% of 1006.85GB   Users logged in:       0
  Memory usage: 2%                  IPv4 address for eth0: 172.18.224.114
  Swap usage:   0%


This message is shown once a day. To disable it please create the
/home/shinyay/.hushlogin file.
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

![image](https://github.com/shinyay/win-setup/assets/3072734/7a2890f2-f610-442d-9081-94bdbadb0dc4)

- **Default Profile**: `Ubuntu24.04 LTS`
- **Default Terminal Application**: `Windows Terminal Preview`

## Homebrew

![image](https://github.com/shinyay/win-setup/assets/3072734/8c7641e7-26b5-4d09-91c0-9324f0f9c558)

> Homebrew is an open-source software package manager that makes it easier to install software on macOS (Apple's operating system) and Linux. Basically, a package manager's job is to find and install the right software packages that will allow you to compile and run various apps/software on your specific operating system.

- [x] Installation

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
```

```shell
==> This script will install:
/home/linuxbrew/.linuxbrew/bin/brew
/home/linuxbrew/.linuxbrew/share/doc/homebrew
/home/linuxbrew/.linuxbrew/share/man/man1/brew.1
/home/linuxbrew/.linuxbrew/share/zsh/site-functions/_brew
/home/linuxbrew/.linuxbrew/etc/bash_completion.d/brew
/home/linuxbrew/.linuxbrew/Homebrew
==> The following new directories will be created:
/home/linuxbrew/.linuxbrew/bin
/home/linuxbrew/.linuxbrew/etc
/home/linuxbrew/.linuxbrew/include
/home/linuxbrew/.linuxbrew/lib
/home/linuxbrew/.linuxbrew/sbin
/home/linuxbrew/.linuxbrew/share
/home/linuxbrew/.linuxbrew/var
/home/linuxbrew/.linuxbrew/opt
/home/linuxbrew/.linuxbrew/share/zsh
/home/linuxbrew/.linuxbrew/share/zsh/site-functions
/home/linuxbrew/.linuxbrew/var/homebrew
/home/linuxbrew/.linuxbrew/var/homebrew/linked
/home/linuxbrew/.linuxbrew/Cellar
/home/linuxbrew/.linuxbrew/Caskroom
/home/linuxbrew/.linuxbrew/Frameworks
```

```shell
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/shinyay/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
sudo apt-get install build-essential
brew install gcc
brew doctor
```

### Update and Upgrade

- Update the list of formulae and versions in Homebrew

```shell
brew update
```

- Check for installed formulae that have been updated since their installation

```shell
brew outdated
```

- Upgrade the installed packages to their latest versions

```shell
brew upgrade
```

## Git

![image](https://github.com/shinyay/win-setup/assets/3072734/936457fa-c308-420f-a0cc-46aae10aeef9)

- [x] Installation

Check pre-installed Git

```shell
which git

/usr/bin/git
```

```shell
git -v

git version 2.43.0
```

```shell
brew install git
```

```shell
which git

/home/linuxbrew/.linuxbrew/bin/git
```

```shell
git --version

git version 2.47.0
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
git config --global init.defaultBranch main && \
git config pull.ff only
```

Check your configuration:

```shell
git config list --global
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

### GPG Key for GitHub

#### Generate GPG Key

```shell
gpg --full-generate-key
```

```shell
gpg (GnuPG) 2.4.4; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection?
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: <GITHUB_USER_NAME>
Email address: <GITHUB_EMAIL_ADDRESS>
You selected this USER-ID:
    "YOUR_NAME <YOUR_EMAIL_ADDRESS>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```

And then, Type a secure passphrase.

#### Confirm your GPG Key

```shell
gpg --list-secret-keys --keyid-format LONG
```

```shell
-----------------------------------------
sec   rsa4096 2022-03-21 [SC] [expires: 2024-03-20]
      XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
uid           [ ultimate ] <user@example.com>
ssb   rsa4096 2022-03-21 [E] [expires: 2024-03-20]

```

`XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX` is your GPG Key

#### Configure GPG Key to Git

```shell
git config --global gpg.program gpg
git config --global user.signingkey <GPG Key>
git config --global commit.gpgsign true
```

#### Add new GPG key

- [Add new GPG Key](https://github.com/settings/gpg/new)

![Add new GPG Key](https://github.com/shinyay/win-setup/assets/3072734/8b25238a-27bf-4218-8e90-4d29b85cd5ae)

```shell
gpg --export --armor <YOUR_EMAI_ADDRESS_FOR_GPGKEY> | clip.exe
```

![GPG Key on GitHub](https://github.com/shinyay/win-setup/assets/3072734/f3f7da6a-4689-47bf-8e83-9f0bc3d1c020)

#### Commit with GPG Signature

```shell
git commit -S -m 'YOUR_COMMIT_MESSAGE'
```

#### Confirmation of your Commits with GPG Signature

You can find **verified** on your commit.

![Image](https://github.com/shinyay/win-setup/assets/3072734/07f296ab-9de9-4d02-97a9-b1e822c4d6b5)

# 2. WSL Customization

## WSL

Update and Upgrade packages.

- [x] Configuration

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
git config --global alias.st 'status --verbose --short --branch --untracked-files'
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

gh version 2.62.0 (2024-11-14)
https://github.com/cli/cli/releases/tag/v2.62.0
```

### GitHub Authentication

- [x] Configuration

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

### GitHub Copilot in the CLI

- [x] Installation

> GitHub Copilot in the CLI is an extension for GitHub CLI which provides a chat-like interface in the terminal that allows you to ask questions about commands you run from the command line.


If you are have not authenticated with your GitHub account, login.

```shell
gh auth login
```

Install the Copilot in the CLI extension

```shell
gh extension install github/gh-copilot
```

Update Copilot in the CLI

```shell
gh extension upgrade gh-copilot
```

```shell
[copilot]: upgraded from v1.0.1 to v1.0.5
✓ Successfully upgraded extension
```

Confirm your version

```shell
gh copilot -v

version 1.0.5 (2024-09-12)
```

#### Explain a command

> You can ask Copilot in the CLI to explain a command

```shell
gh copilot explain

? Which command would you like to explain?
> apt-get

Explanation:

  • apt-get is the package management command-line tool for Debian-based systems.
    • It is used to install, upgrade, and remove software packages.
    • Commonly used flags include:
      • install is used to install packages.
      • remove is used to remove packages.
      • update is used to update package lists from repositories.
      • upgrade is used to upgrade installed packages to their latest versions.
      • search is used to search for packages based on keywords.
      • show is used to display detailed information about a specific package.
      • upgrade is used to upgrade installed packages to their latest versions.
      • dist-upgrade is used to perform a distribution upgrade, including installing and removing packages as
      necessary.
      • autoremove is used to remove automatically installed packages that are no longer needed.
      • clean is used to remove downloaded package files from the package cache.
      • purge is used to remove packages and their configuration files.
    • For more information and usage examples, refer to the apt-get manual page by running man apt-get.
```

Alternatively, you can add the command you want explained directly to the prompt:

```shell
gh copilot explain 'apt-get'
```

#### Suggest a command

```shell
gh copilot suggest "Upgrade apt-get itself"
```

```shell
? What kind of command can I help you with?
> generic shell command

Suggestion:

  sudo apt-get update && sudo apt-get upgrade -y

? Select an option  [Use arrows to move, type to filter]
> Copy command to clipboard
  Explain command
  Execute command
  Revise command
  Rate response
  Exit
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

```shell
fish --version

fish, version 3.7.1
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

Restart **Terminal**

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

Fisher version, as of Oct 24, 2024

```shell
fisher -v
fisher, version 4.4.5
```

### Fish Prompt - Tide

![tide](https://github.com/IlanCosman/tide/raw/assets/images/logo.svg)

- [x] Installation

> The ultimate Fish prompt.

- [IlanCosman/tide](https://github.com/IlanCosman/tide)
- [tide - Configuration Wiki](https://github.com/IlanCosman/tide/wiki/Configuration)

```shell
fisher install IlanCosman/tide@v6
```

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

- [x] Installation

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

Then delete downloaded fonts.

```shell
cd ..
rm -fr download
```

#### Setting font for Terminal

- [x] Configuration

1. Open settings: `ctrl` + `,`
2. Select **Ubuntu 24.04 LTS**
3. Select **Aditional settings** > **Appearance**
4. Choose **Moralerspace Radon NF** from **Font face**

### Cica font

- [Cica](https://github.com/miiton/Cica)

- [ ] Installation

### Custome Functions for Fish

#### Clear Cahaces

Clear Caches and turn off and on Swap aria

```shell
function clearCaches
    sudo sh -c "echo 3 > '/proc/sys/vm/drop_caches' && swapoff -a && swapon -a"
end
```

```shell
funcsave clearCaches
```

Confirm a persisntent file

```shell
ls -l ~/.config/fish/functions/clearCaches.fish
```

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

Confirm your version

```shell
peco --version

peco version v0.5.11 (built with go1.20.2)
```

### fzf

#### fzf

- [x] Installation `fzf`

> `fzf` is a general-purpose command-line fuzzy finder.

- [fzf](https://github.com/junegunn/fzf)

```shell
brew install fzf
```

Confirm your version

```shell
fzf --version

0.56.3 (brew)
```

#### bat

- [x] Installation bat

> A cat(1) clone with syntax highlighting and Git integration.

- [bat](https://github.com/sharkdp/bat)

```shell
brew install bat
```

Confirm your version

```shell
bat --version

bat 0.24.0
```

#### fd

- [x] Installation fd

> `fd` is a program to find entries in your filesystem. It is a simple, fast and user-friendly alternative to `find`.

- [fd](https://github.com/sharkdp/fd)

```shell
brew install fd
```

Confirm your version

```shell
fd --version

fd 10.2.0
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
echo "Hello $(whoami) at $(date)"
fish -v
fisher -v
fish_logo blue cyan green
```

### SDKMAN! for fish

![sdkman](https://github.com/shinyay/win-setup/assets/3072734/a133c080-33d9-410a-b78f-99c4c37e1809)

- [x] Installation

#### SDKMAN with Homebrew

- [SDKMAN!](https://sdkman.io/), The Software Development Kit Manager

```shell
brew tap SDKMAN/tap
brew install SDKMAN-cli
```

#### SDKMAN for Fish extension

Makes command sdk from SDKMAN! usable from fish, including auto-completion. Also adds binaries from installed SDKs to the PATH.

- [reitzig/sdkman-for-fish](https://github.com/reitzig/sdkman-for-fish)

```shell
fisher install reitzig/sdkman-for-fish@v2.1.0

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

### git

- [ ] Installation

> `git` - the stupid content tracker

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/git.fish > $HOME/.config/fish/completions/git.fish
```

### gh

- [x] Installation

> `gh` - gh - GitHub CLI

```shell
curl https://raw.githubusercontent.com/fish-shell/fish-shell/master/share/completions/gh.fish > $HOME/.config/fish/completions/gh.fish
```

### docker

- [x] Installation

After Docker installation, you can do the following:

```shell
docker completion fish > $HOME/.config/fish/completions/docker.fish
```

## Docker

![Image](https://github.com/shinyay/win-setup/assets/3072734/c0351b8f-f1c4-43ee-9dea-542e9a28aa53)

### Docker Desktop for Windows

- [x] Installation

```powershell
winget search docker
winget install Docker.DockerDesktop
```

### Docker Engine for Ubuntu

- [x] Installation

#### Uninstall old versions

- [x] Uninstallation

```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; sudo apt-get remove $pkg; end
```

#### Set up Docker's apt repository

- [x] Configuration

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

- [x] Installation

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Manage Docker as a non-root user

- [x] Configuration

> The Docker daemon binds to a Unix socket, not a TCP port. By default it's the root user that owns the Unix socket, and other users can only access it using sudo. The Docker daemon always runs as the root user.
> If you don't want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

```shell
sudo usermod -aG docker $USER
```

Reboot your WSL by **PowerShell**.

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

## SDKMAN!

### Enable Beta Channel

```shell
sed -ie "s/sdkman_beta_channel=false/sdkman_beta_channel=true/g" ~/.sdkman/etc/config
```

### Java

- OpenJDK
  - `24.ea.23`

```shell
sdk install java 24.ea.23-open
```

- Microsoft OpenJDK
  - `21.0.5-ms`
  - `11.0.25-ms`

```shell
sdk install java 21.0.5-ms
sdk install java 11.0.25-ms
```

### Kotlin

- `2.0.21`

```shell
sdk install kotlin
```

### Ki

- `0.5.2`

```shell
sdk install ki
```

### Spring Boot

- `3.3.5`

```shell
sdk install springboot
```

### Maven

- `3.9.9`

```shell
sdk install maven 3.9.9
```

## Abbreviation for Fish

`abbr`: manage fish abbreviations

```shell
abbr --add NAME [--position command | anywhere] [-r | --regex PATTERN]
                [--set-cursor[=MARKER]] ([-f | --function FUNCTION] | EXPANSION)
abbr --erase NAME ...
abbr --rename OLD_WORD NEW_WORD
abbr --show
abbr --list
abbr --query NAME ...
```

### Add abbreviated commands

```shell
vim $HOME/.config/fish/config.fish
```

```shell
function set_abbr
	abbr --add code		code-insiders
	abbr --add history	peco_select_history
	abbr --add l	 	ls -lahF
	abbr --add ga		git add -v
	abbr --add gc		git commit -S -m
	abbr --add gcpast	git commit -S --date=format:relative:1.day.ago -m
	abbr --add copilot	gh copilot suggest
end

if status is-interactive
    # Commands to run in interactive sessions can go here
    set_abbr
end
```

# 3. Terminal Customization

## Startup

- [x] Configuration

- **Default Profile**: `Ubuntu 24.04 LTS`
- **Default Terminal Application**: `Windows Terminal Preview`
- **Launch Size**:
  - **Columns**: `110`
  - **Rows**: `22`

## Interaction

- Remove trailing white-space in recangular selection: On
  - > You can select in rectangular with `Alt` + Mouse select

## Appearance

- [x] Configuration

- **Show acrylic tab row**: `On`

## Ubuntu - Appearance

- [x] Configuration

- **Transparency - Background opacity**: `90%`
- **Window - Scrollbar Visibility**: `Hidden`

# 4. Visual Studio Code

! [vscode](https://code.visualstudio.com/assets/images/code-stable.png)

> Visual Studio Code, often referred to as VSCode, is a free and open-source code editor optimized for building and debugging modern web and cloud applications. It supports various programming languages and comes with features like IntelliSense for smart completions based on variable types, function definitions, and imported modules, built-in Git commands, and debugging tools. It's highly customizable and extensible with various extensions. It's available on multiple platforms including Linux, macOS, and Windows.

## Visual Studioc Code on Windows

- [x] Installation

```shell
winget search VisualStudioCode
winget install Microsoft.VisualStudioCode.Insiders
```

## Visual Studioc Code on Ubuntu with Windows Subsystem for Linux (WSL)

- [x] Installation

```shell
snap find vscode
sudo snap install code-insiders --classic
```

The following abbreviation is added to `config.fish`

```shell
abbr --add code 'code-insiders'
```

## VSCode Extension

### Remote Development

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

  ![image](https://github.com/shinyay/win-setup/assets/3072734/d2a8dec6-3a3f-4893-8a4f-4b7ebaa8fedb)

### GitHub

- [GitHub Codespaces](https://marketplace.visualstudio.com/items?itemName=GitHub.codespaces)
- [GitHub Theme](https://marketplace.visualstudio.com/items?itemName=GitHub.github-vscode-theme)
- [GitHub Repositories](https://marketplace.visualstudio.com/items?itemName=GitHub.remotehub)
- [GitHub Actions](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)
- [GitHub Pull Requests](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)
- [CodeQL](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)

### GitHub Copilot

- [x] Installation
  - Windows
  - WSL2(Linux)

> GitHub Copilot provides autocomplete-style suggestions from an AI pair programmer as you code. You can receive suggestions from GitHub Copilot either by starting to write the code you want to use, or by writing a natural language comment describing what you want the code to do.

- [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)

The folloing extention is installed:

- [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)

Use `ctrl` + `i` to open incline chat.
Type `/` to view all available chat commands.

- [GitHub Copilot (Next Edit Suggestions)]()
- [GitHub Copilot Workspace](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-workspace)

### Java

- [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

### Azure
- [GitHub Copilot for Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azure-github-copilot)
- [Azure Tools](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)
- [Azure Account](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)

### Docker for Visual Studio Code

- [x] Installation
  - WSL2(Linux)

> The Docker extension makes it easy to build, manage, and deploy containerized applications from Visual Studio Code. It also provides one-click debugging of Node.js, Python, and .NET inside a container.

## `Text Editor`

**Settings** - Everything starts from `Ctrl` + `,`

### Mouse Wheel Zoom

> Zoom the font of the editor when using mouse wheel and holding `Ctrl`

- [x] Enabled

#### Reset Editor Font Size

`Ctrl` + `Shift` + `p` and choose **Reset Editor Font Size**

### Render Control Characters

Controls whether the editor should render control characters.

- [x] Enabled

### Render Line Highlight

> Controls how the editor render the current line highlight

- [x] `all`

## `Text Editor` - `Cursor`

### Cursor Blinking

> Control the cursor animation style

- [x] `expand`

### Cursor Smooth Caret Animation

Control whether the sooth caret animation be enabled.

- [x] Enabled

### Cursor Style

> Control the cursor style

- [x] `block`

## `Text Editor` - `Formatting`

### Format On Paste

> Controls whether the editor should automatically format the pasted content. A formatter must be available and the formatter should be able to format a range in a document.

- [x] Enabled

### Format On Type

Controls whether the editor should automatically format the line after typing.

- [x] Enabled

## `Text Editor` - `Minimap`

### Enabled

Controls whether the minimap is shown.

- [x] Disabled

## `Text Editor` - `Files`

### Insert Final Newline

When Enabled, insert a final new line at the end of the file saving it.

- [x] Enabled

### Trim Trailing Whitespace

When enabled, will trim trailing whitespace when saving a file.

- [x] Enabled

## `Workbench` - `Appearance`

### Color Theme

> Specifies the color theme used in the workbench when Window

- [x] `Tomorrow Night Blue`

## `Workbench` - `Zen Mode`

### Center Layout

Controls whether turning on Zen Mode also centers the layout.

- [x] Disabled

### Hide Line Numbers

Controls whether turning on Zen Mode also hides the editor line numbers.

- [x] Disabled

### Hide Status Bar

Controls whether turning on Zen Mode also hides the status bar at the bottom of the workbench.

- [x] Disabled

## Profile

> VS Code profiles provide a way to organize and isolate customizations within the editor. A profile represents a specific set of configurations that can be easily activated or deactivated. With profiles, users can maintain separate configurations for different projects or teams, to help ensure a seamless transition between development environments.

### Create Profile

![create-profile](https://github.com/shinyay/win-setup/assets/3072734/430da389-8cb3-40b2-b82f-5f9932575398)

# 5. Microsoft Azure

- [Microsoft Azure Portal](https://portal.azure.com/#home)

![azure](https://github.com/shinyay/win-setup/assets/3072734/11209062-a229-4e70-9263-4843a14ed7f5)

## Azure CLI

- [What is the Azure CLI?](https://learn.microsoft.com/cli/azure/what-is-azure-cli)

### Install on Linux(WSL2)

- [x] Installation

You can install Azure CLI by **Homebrew**.

```shell
brew update && brew install azure-cli
```

```shell
az version
{
  "azure-cli": "2.60.0",
  "azure-cli-core": "2.60.0",
  "azure-cli-telemetry": "1.1.0",
  "extensions": {}
}
```

### Install on Linux(WSL2) with apt-get

```shell
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

```shell
sudo mkdir -p /etc/apt/keyrings
curl -sLS https://packages.microsoft.com/keys/microsoft.asc |
  gpg --dearmor | sudo tee /etc/apt/keyrings/microsoft.gpg > /dev/null
sudo chmod go+r /etc/apt/keyrings/microsoft.gpg
```

```shell
set AZ_DIST (lsb_release -cs)
echo "Types: deb
URIs: https://packages.microsoft.com/repos/azure-cli/
Suites: $AZ_DIST
Components: main
Architectures: "(dpkg --print-architecture)"
Signed-By: /etc/apt/keyrings/microsoft.gpg" | sudo tee /etc/apt/sources.list.d/azure-cli.sources
```

```shell
sudo apt-get update
sudo apt-get install azure-cli
```

### Install on Windows

You can install Azure CLI by `winget` on PowerShell.

```shell
winget install -e --id Microsoft.AzureCLI
```

### Sign in interactively

```shell
az login
```

### Azure CLI Help

```shell
az -h
```

<details>
  <summary>Azure CLI Help</summary>

```shell
Group
    az

Subgroups:
    account               : Manage Azure subscription information.
    acr                   : Manage private registries with Azure Container Registries.
    ad                    : Manage Microsoft Entra ID (formerly known as Azure Active Directory,
                            Azure AD, AAD) entities needed for Azure role-based access control
                            (Azure RBAC) through Microsoft Graph API.
    advisor               : Manage Azure Advisor.
    afd                   : Manage Azure Front Door Standard/Premium.
    aks                   : Manage Azure Kubernetes Services.
    ams                   : Manage Azure Media Services resources.
    apim                  : Manage Azure API Management services.
    appconfig             : Manage App Configurations.
    appservice            : Manage App Service plans.
    aro                   : Manage Azure Red Hat OpenShift clusters.
    backup                : Manage Azure Backups.
    batch                 : Manage Azure Batch.
    bicep                 : Bicep CLI command group.
    billing               : Manage Azure Billing.
    bot                   : Manage Microsoft Azure Bot Service.
    cache                 : Commands to manage CLI objects cached using the `--defer` argument.
    capacity              : Manage capacity.
    cdn                   : Manage Azure Content Delivery Networks (CDNs).
    cloud                 : Manage registered Azure clouds.
    cognitiveservices     : Manage Azure Cognitive Services accounts.
    config [Experimental] : Manage Azure CLI configuration.
    connection            : Commands to manage Service Connector local connections which allow local
                            environment to connect Azure Resource. If you want to manage connection
                            for compute service, please run 'az webapp/containerapp/spring
                            connection'.
    consumption [Preview] : Manage consumption of Azure resources.
    container             : Manage Azure Container Instances.
    containerapp          : Manage Azure Container Apps.
    cosmosdb              : Manage Azure Cosmos DB database accounts.
    databoxedge [Preview] : Manage device with databoxedge.
    deployment            : Manage Azure Resource Manager template deployment at subscription scope.
    deployment-scripts    : Manage deployment scripts at subscription or resource group scope.
    disk                  : Manage Azure Managed Disks.
    disk-access           : Manage disk access resources.
    disk-encryption-set   : Disk Encryption Set resource.
    dla         [Preview] : Manage Data Lake Analytics accounts, jobs, and catalogs.
    dls         [Preview] : Manage Data Lake Store accounts and filesystems.
    dms                   : Manage Azure Data Migration Service (classic) instances.
    eventgrid             : Manage Azure Event Grid topics, domains, domain topics, system topics
                            partner topics, event subscriptions, system topic event subscriptions
                            and partner topic event subscriptions.
    eventhubs             : Eventhubs.
    extension             : Manage and update CLI extensions.
    feature               : Manage resource provider features.
    functionapp           : Manage function apps. To install the Azure Functions Core tools see
                            https://github.com/Azure/azure-functions-core-tools.
    group                 : Manage resource groups and template deployments.
    hdinsight             : Manage HDInsight resources.
    identity              : Managed Identities.
    image                 : Manage custom virtual machine images.
    iot                   : Manage Internet of Things (IoT) assets.
    keyvault              : Manage KeyVault keys, secrets, and certificates.
    kusto                 : Manage Azure Kusto resources.
    lab         [Preview] : Manage Azure DevTest Labs.
    lock                  : Manage Azure locks.
    logicapp              : Manage logic apps.
    managed-cassandra     : Azure Managed Cassandra.
    managedapp            : Manage template solutions provided and maintained by Independent
                            Software Vendors (ISVs).
    managedservices       : Manage the registration assignments and definitions in Azure.
    maps                  : Manage Azure Maps.
    mariadb               : Manage Azure Database for MariaDB servers.
    monitor               : Manage the Azure Monitor Service.
    mysql                 : Manage Azure Database for MySQL servers.
    netappfiles           : Manage Azure NetApp Files (ANF) Resources.
    network               : Manage Azure Network resources.
    policy                : Manage resource policies.
    postgres              : Manage Azure Database for PostgreSQL servers.
    ppg                   : Manage Proximity Placement Groups.
    private-link          : Private-link association CLI command group.
    provider              : Manage resource providers.
    redis                 : Manage dedicated Redis caches for your Azure applications.
    relay                 : Manage Azure Relay Service namespaces, WCF relays, hybrid connections,
                            and rules.
    resource              : Manage Azure resources.
    resourcemanagement    : Resourcemanagement CLI command group.
    restore-point         : Manage restore point with res.
    role                  : Manage Azure role-based access control (Azure RBAC).
    search                : Manage Azure Search services, admin keys and query keys.
    security              : Manage your security posture with Microsoft Defender for Cloud.
    servicebus            : Servicebus.
    sf                    : Manage and administer Azure Service Fabric clusters.
    sig                   : Manage shared image gallery.
    signalr               : Manage Azure SignalR Service.
    snapshot              : Manage point-in-time copies of managed disks, native blobs, or other
                            snapshots.
    sql                   : Manage Azure SQL Databases and Data Warehouses.
    sshkey                : Manage ssh public key with vm.
    stack                 : A deployment stack is a native Azure resource type that enables you to
                            perform operations on a resource collection as an atomic unit.
    staticwebapp          : Manage static apps.
    storage               : Manage Azure Cloud Storage resources.
    synapse               : Manage and operate Synapse Workspace, Spark Pool, SQL Pool.
    tag                   : Tag Management on a resource.
    term   [Experimental] : Manage marketplace agreement with marketplaceordering.
    ts                    : Manage template specs at subscription or resource group scope.
    vm                    : Manage Linux or Windows virtual machines.
    vmss                  : Manage groupings of virtual machines in an Azure Virtual Machine Scale
                            Set (VMSS).
    webapp                : Manage web apps.

Commands:
    configure             : Manage Azure CLI configuration. This command is interactive.
    feedback              : Send feedback to the Azure CLI Team.
    find                  : I'm an AI robot, my advice is based on our Azure documentation as well
                            as the usage patterns of Azure CLI and Azure ARM users. Using me
                            improves Azure products and documentation.
    interactive [Preview] : Start interactive mode. Installs the Interactive extension if
                            not installed already.
    login                 : Log in to Azure.
    logout                : Log out to remove access to Azure subscriptions.
    rest                  : Invoke a custom request.
    survey                : Take Azure CLI survey.
    upgrade     [Preview] : Upgrade Azure CLI and extensions.
    version               : Show the versions of Azure CLI modules and extensions in JSON format by
                            default or format configured by --output.

To search AI knowledge base for examples, use: az find "az "
```

</details>

### Getting Started with Azure CLI

```shell
az login
```

You check your subscription at the folloing and set it to Azure CLI

```shell
az account set --subscription "<YOUR_SUBSCRIPTION_NAME>"
```

### Microsoft Dev Box

![Image](https://github.com/shinyay/win-setup/assets/3072734/e4e07ccd-e1df-4e2f-ba3b-f6d823d092f2)

#### Install the Dev Center extension

```shell
az extension add --name devcenter --allow-preview true
```

```shell
az extension list
```

### Azure Spring Apps

# 6. Development Environment on Dev containers

![devcontainers](https://user-images.githubusercontent.com/10041279/93239062-e1b9a480-f747-11ea-94fb-3d50b14fd9b1.png)

## Java

This Dev Container consists of the following three files:

- Dockerfile
- compose.yaml
- devcontainer.json

### Dockerfile

> This file is used to build the container image that will contain your development environment. It specifies the base image, installs necessary dependencies, sets up the environment, and copies any project-specific files into the container. The Dockerfile is essential for defining the environment's infrastructure and dependencies.

```dockerfile
FROM mcr.microsoft.com/devcontainers/java:1-21-bullseye
USER vscode
WORKDIR /workspace
```

- **USER**: Non-root user.
  - In `mcr.microsoft.com/devcontainers/java`, the `vscode` user is already defined and can be used by simply specifying it
- **WORKDIR**: To separate the workspace from the location of the dev container definition files

If you want a base image which Microsoft pre-built, you can find it at the following:

- [Available Dev Container Templates](https://containers.dev/templates)


### compose.yaml

>  Docker Compose is a tool for defining and running multi-container Docker applications. The `compose.yaml` file defines services, networks, and volumes for your application's containers. While not strictly required for every Dev Container setup, it can be useful for defining more complex development environments with multiple services or containers that need to interact with each other.

```yaml
services:
  playground-java:
    container_name: 'playground'
    hostname: 'java'
    build:
      context: .
      dockerfile: Dockerfile
    restart: always
    working_dir: '/workspace'
    tty: true
    volumes:
      - type: bind
        source: ../workspace
        target: /workspace
```

#### Explanation

```yaml
services:
  playground-java:
    container_name: 'playground'
    hostname: 'java'
```

- `services`: This is the top-level key in a Docker Compose file, and it defines the services that make up your application.
- `playground-java`: This is the name of the service, which in this case is named `playground-java`.

```yaml
    build:
      context: .
      dockerfile: Dockerfile
```

- `build`: This specifies how to build the Docker image for the service.
- `context: .`: This specifies the build context, which is the directory where the Dockerfile and any other files needed for building the image are located. In this case, it's set to the current directory (`.`).
- `dockerfile: Dockerfile`: This specifies the name of the Dockerfile to use for building the image. In this case, it's named `Dockerfile`.

```yaml
    restart: always
```

- `restart: always`: This instructs Docker to always restart the container if it stops for any reason.

```yaml
    working_dir: '/workspace'
```

- `working_dir: '/workspace'`: This sets the working directory inside the container to `/workspace`. When the container starts, commands executed within the container will run relative to this directory.

```yaml
    tty: true
```

- `tty: true`: This allocates a pseudo-TTY for the service container, which enables interactive shell sessions.

```yaml
    volumes:
      - type: bind
        source: ../workspace
        target: /workspace
```

- `volumes`: This specifies any volumes to mount into the container.
- `- type: bind`: This indicates that you're using a bind mount, which mounts a directory from the host machine into the container.
- `source: ../workspace`: This specifies the source directory on the host machine to bind mount into the container. It's relative to the location of the `compose.yaml` file and points to a directory named `workspace` in the parent directory (`..`).
- `target: /workspace`: This specifies the target directory inside the container where the source directory will be mounted.

### devcontainer.json

> The `devcontainer.json` file configures how VS Code connects to and interacts with the Dev Container, including settings for extensions, environment variables, mount points, and more.

```json
{
    "name": "Playground - Java",
    "dockerComposeFile": "compose.yaml",
    "service": "playground-java",
    "workspaceFolder": "/workspace",
    "remoteUser": "vscode",
    "features": {
        "ghcr.io/devcontainers/features/java:1": {
            "version": "none",
            "installMaven": "false",
            "installGradle": "true"
        }
    },
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-azuretools.vscode-docker",
                "redhat.java",
                "vscjava.vscode-java-debug",
                "vscjava.vscode-java-dependency",
                "vscjava.vscode-java-test",
                "vscjava.vscode-maven",
                "SonarSource.sonarlint-vscode",
                "vmware.vscode-boot-dev-pack",
                "MicroProfile-Community.vscode-microprofile-pack",
                "redhat.vscode-quarkus"
            ],
            "settings": {
                "editor.formatOnSave": true,
                "workbench.colorCustomizations": {
                    "titleBar.activeBackground": "#19549C",
                    "titleBar.activeForeground": "#ffffff",
                    "activityBar.background": "#02A7E3",
                    "activityBar.foreground": "#ffffff"
                }
            }
        }
    }
    // "forwardPorts": [],
    // "postCreateCommand": "java -version"
}
```

#### Explanation

```json
{
	"name": "Playground - Java",
  "dockerComposeFile": "compose.yaml",
	"service": "playground-java",
	"workspaceFolder": "/workspace",
	"remoteUser": "vscode",
```

- `name`: This is the name of the Dev Container configuration, which appears in the VS Code UI.
- `dockerComposeFile`: This specifies the Docker Compose file to use for creating the Dev Container. In this case, it's set to `compose.yaml`.
- `service`: This specifies the service within the Docker Compose file that represents the Dev Container. Here, it's set to `playground-java`.
- `workspaceFolder`: This specifies the workspace folder within the Dev Container. It's set to `/workspace`.
- `remoteUser`: This specifies the user to use within the Dev Container. Here, it's set to `vscode`, which is a common choice for development within containers.

```json
	"features": {
		"ghcr.io/devcontainers/features/java:1": {
			"version": "none",
			"installMaven": "false",
			"installGradle": "true"
		}
	},
```

- `features`: This section allows you to specify additional features or tools to include in the Dev Container. In this case, it's specifying a Java feature from a predefined set of Dev Container features. It specifies that Maven should not be installed (`installMaven: "false"`) but Gradle should be installed (`installGradle: "true"`).

```json
	"customizations": {
		"vscode": {
			"extensions": [
				"ms-azuretools.vscode-docker"
			],
			"settings": {
				"editor.formatOnSave": true,
				"workbench.colorCustomizations": {
					"titleBar.activeBackground": "#19549C",
					"titleBar.activeForeground": "#ffffff",
					"activityBar.background": "#02A7E3",
					"activityBar.foreground": "#ffffff"
				}
			}
		}
	}
```

- `customizations`: This section allows you to customize the VS Code environment within the Dev Container. It specifies extensions to install (`ms-azuretools.vscode-docker`) and VS Code settings to apply. In this case, it enables automatic formatting on save (`"editor.formatOnSave": true`) and customizes the colors of the title bar and activity bar.

##### forwardPorts

The `forwardPorts` setting in the `devcontainer.json` file is used to specify port forwarding rules from the Dev Container to the local machine, allowing you to access services running inside the container from your local environment. This can be useful when you're running services or applications inside the Dev Container that need to be accessed from outside the container, such as web servers, APIs, or databases.

Here's example of the `forwardPorts` setting:

```json
"forwardPorts": [3000, 8080]
```

This example would forward ports 3000 and 8080 from the Dev Container to the local machine. So if there's a web server running inside the Dev Container on port 3000, you could access it from your local browser at `http://localhost:3000`.

You would typically set the `forwardPorts` setting when you're developing applications or services that need to be accessible from your local environment but are running inside a container. It's especially useful for web development, where you might have a web server running inside the container serving your application, and you want to test it locally in your browser.

##### postCreateCommand

The `postCreateCommand` setting in the `devcontainer.json` file is used to specify a command that should be executed after the Dev Container is created. This command runs once, immediately after the container is created but before it is started.

You might use the `postCreateCommand` setting for various purposes, such as:

1. **Setting up the development environment**: You could use this command to perform additional setup steps required for your development environment after the container is created. For example, you might install additional tools or dependencies, initialize databases, or set up configuration files.

2. **Running initialization scripts**: If your project requires specific initialization scripts to be executed before development can begin, you can specify these scripts as the `postCreateCommand`. This could include running database migrations, setting up test data, or performing any other necessary setup tasks.

3. **Customizing the container environment**: You could use the `postCreateCommand` to customize the container environment based on specific project requirements. This might involve configuring system settings, setting environment variables, or performing any other customizations needed for your project.

Here's an example of how you might set the `postCreateCommand` in the `devcontainer.json` file:

```json
"postCreateCommand": "git fetch origin && git diff origin/main"
```

In this example, this command sequence would execute git fetch origin to fetch the latest changes from the remote repository and then run git diff origin/main to compare the local branch with the main branch on the remote repository.

Overall, the `postCreateCommand` setting allows you to automate additional setup steps or customization tasks that need to be performed after the Dev Container is created, helping to streamline the development environment setup process for your project.

#### Recommended VS Code Extensions

I recommend the following Visual Studio Code Extensions for Java Development:

- [Language Support for Java(TM) by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java)

[![redhat.java](https://redhat.gallerycdn.vsassets.io/extensions/redhat/java/1.30.2024040408/1712218493729/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=redhat.java)

- [Debugger for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)

[![vscjava.vscode-java-debug](https://vscjava.gallerycdn.vsassets.io/extensions/vscjava/vscode-java-debug/0.57.0/1711429312949/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)

- [Project Manager for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-dependency)

[![vscjava.vscode-java-dependency](https://vscjava.gallerycdn.vsassets.io/extensions/vscjava/vscode-java-dependency/0.23.2024032505/1711346978256/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-dependency)

- [Test Runner for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test)

[![vscjava.vscode-java-test](https://vscjava.gallerycdn.vsassets.io/extensions/vscjava/vscode-java-test/0.41.0/1712283540135/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-test)

- [Maven for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-maven)

[![vscjava.vscode-maven](https://vscjava.gallerycdn.vsassets.io/extensions/vscjava/vscode-maven/0.44.2024013105/1706679025282/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-maven)

- [SonarLint](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode)

[![SonarSource.sonarlint-vscode](https://sonarsource.gallerycdn.vsassets.io/extensions/sonarsource/sonarlint-vscode/4.4.2/1712239357006/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode)

- [Spring Boot Extension Pack](https://marketplace.visualstudio.com/items?itemName=vmware.vscode-boot-dev-pack)

[![vmware.vscode-boot-dev-pack](https://vmware.gallerycdn.vsassets.io/extensions/vmware/vscode-boot-dev-pack/0.2.1/1675235820676/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=vmware.vscode-boot-dev-pack)

- [Extension Pack for MicroProfile](https://marketplace.visualstudio.com/items?itemName=MicroProfile-Community.vscode-microprofile-pack)

[![MicroProfile-Community.vscode-microprofile-pack](https://microprofile-community.gallerycdn.vsassets.io/extensions/microprofile-community/vscode-microprofile-pack/0.1.3/1641330099100/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=MicroProfile-Community.vscode-microprofile-pack)

- [Quarkus](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-quarkus)

[![redhat.vscode-quarkus](https://redhat.gallerycdn.vsassets.io/extensions/redhat/vscode-quarkus/1.18.2024040604/1712391376590/Microsoft.VisualStudio.Services.Icons.Default)](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-quarkus)

## Kotlin
## Rust
## TypeScript
## Python

## Dev Container CLI

- [ ] Installation

- [Dev Container CLI](https://code.visualstudio.com/docs/devcontainers/devcontainer-cli)
  - [devcontainers/cli](https://github.com/devcontainers/cli)

Install Dev Container CLI.

1. Open VSCode
2. Select the **Dev Containers: Install devcontainer CLI** command from the Command Palette (`F1`).

## Docker for Visual Studio Code

> The Docker extension makes it easy to build, manage, and deploy containerized applications from Visual Studio Code. It also provides one-click debugging of Node.js, Python, and .NET inside a container.

- [ ] Installation

- [Docker for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

## Azure CLI

- [ ] Installation

> The Azure command-line interface (Azure CLI) is a set of commands used to create and manage Azure resources. The Azure CLI is available across Azure services and is designed to get you working quickly with Azure, with an emphasis on automation.

- [Azure Command-Line Interface (CLI) documentation](https://learn.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)
- [How to install the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

```shell
brew update && brew install azure-cli
```


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

# X. Virtual Machine Environment with Multipass

## Multipass

- [ ] Installation

> Get an instant Ubuntu VM with a single command. Multipass can launch and run virtual machines and configure them with cloud-init like a public cloud.

- [Multipass](https://multipass.run/)

```shell
winget search multipass
winget install Canonical.Multipass
```

```shell
> multipass --version
multipass   1.13.1+win
multipassd  1.13.1+win
```

## VM for MicroK8s

# 2. Windows Tools

## OBS Studio

![obs](https://github.com/shinyay/win-setup/assets/3072734/24380db9-d604-497d-aa4b-c32a7393964d)

- [x] Installation

> OBS Studio is a free and open-source software for video recording and live streaming.

```shell
winget search obs
winget install OBSProject.OBSStudio
```

## DaVinci Resolve 19

![davinci](https://github.com/shinyay/win-setup/assets/3072734/44b14fe8-09dd-4a6f-b98a-4357d47dba21)

- [x] Installation

> DaVinci Resolve 19 is a professional video editing software that includes everything professional editors need to cut blockbuster films, television shows, and commercials.

- [DaVinci Resolve 19](https://www.blackmagicdesign.com/products/davinciresolve/)
  - DaVinci Resolve 19 Beta 3 (as of May 25, 2024)

## ShurePlus MOTIV

![shure](https://github.com/shinyay/win-setup/assets/3072734/f1c77c42-ae01-4047-b0de-c1c1c3e31ca8)

- [x] Installation

> ShurePlus MOTIV is a free app that allows users to record high-quality audio with their MOTIV microphones and make real-time adjustments to gain, stereo width, equalization, and compression.

```shell
winget search shure
winget install Shure.ShurePlusMOTIV
```

## Obsidian

- [ ] Installation

> It's a note-taking and knowledge base app that uses a local folder of plain text files to store notes and allows users to easily link notes together and build a network of knowledge.

```shell
winget search obsidian
winget install Obsidian.Obsidian
```

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

- [ ] Installation

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

## PowerToys

- [PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/)

> Microsoft PowerToys is a set of utilities for power users to tune and streamline their Windows experience for greater productivity.

- [x] Installation

```shell
winget install --id Microsoft.PowerToys --source winget
```
