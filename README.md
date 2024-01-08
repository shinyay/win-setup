# Windows 11 Pro - 22H2

---
# 1. Initial Setup
## WSL

> WSL2 is a **Windows Subsystem for Linux** that allows access to Linux tools and applications directly from the Windows environment12. It offers the best of both worlds by allowing you to run Windows apps, like Visual Studio, alongside a Linux shell for easier command line access2. WSL2 uses a virtual machine, and uses a full Linux kernel built and shipped with Windows23. With WSL2, you can build docker images that paravirtualize your GPU4.

- [x] Installation

Enable **Windows Subsystem for Linux**

<img width="348" alt="Screenshot 2024-01-08 144911" src="https://github.com/shinyay/win-setup/assets/3072734/821dcbc2-0c8d-412d-8414-c26e63d74fdc">

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
```

## Fish
### Fish Install

- [x] Installation

```shell
sudo apt-get install fish
```

Configure `fish` as Default Shell

- [x] Configuration

```shell
chsh
/usr/bin/fish
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


