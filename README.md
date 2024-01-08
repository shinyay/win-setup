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

### Fish

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
