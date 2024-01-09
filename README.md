# Windows 11 Pro - 22H2

---
# 1. Initial Setup
## VSCode

- [x] Installation

```powershell
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
sudo apt-get install unzip
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

## Homebrew

- [x] Installation

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
```

```shell
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
brew doctor
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

### Fisher
A plugin manager for Fish (https://nicedoc.io/jorgebucaran/fisher)

- [x] Installation

```shell
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

```
fisher -v
fisher, version 4.4.4
```

### Fish Theme - bobthefish

- [x] Installation

- [oh-my-fish/theme-bobthefish](https://github.com/oh-my-fish/theme-bobthefish)

```shell
fisher install oh-my-fish/theme-bobthefish
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


