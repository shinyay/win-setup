# Docker Engine — Detailed Configuration Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-22

---

## Table of Contents

- [Overview](#overview)
- [Engine Details](#engine-details)
  - [Server Configuration](#server-configuration)
  - [Runtime & Containerd](#runtime--containerd)
  - [Security](#security)
- [Plugins](#plugins)
  - [Buildx](#buildx)
  - [Compose](#compose)
- [Storage](#storage)
  - [Storage Driver](#storage-driver)
  - [Docker Root Directory](#docker-root-directory)
- [Networking](#networking)
  - [Network Plugins](#network-plugins)
  - [Default Networks](#default-networks)
- [Logging](#logging)
- [Containers & Images Summary](#containers--images-summary)
  - [Images](#images)
  - [Key Dev Container Images](#key-dev-container-images)
  - [Containers](#containers)
- [Swarm](#swarm)
- [Installation Reference](#installation-reference)
  - [1. Set Up Docker's Official APT Repository](#1-set-up-dockers-official-apt-repository)
  - [2. Install Docker Engine Packages](#2-install-docker-engine-packages)
  - [3. Verify Installation](#3-verify-installation)
- [Non-Root Setup (docker Group)](#non-root-setup-docker-group)
- [systemd Service](#systemd-service)
- [Fish Shell Integration](#fish-shell-integration)
  - [Shell Completion](#shell-completion)
  - [Useful Fish Abbreviations](#useful-fish-abbreviations)
- [VS Code Integration](#vs-code-integration)
- [Useful Commands Reference](#useful-commands-reference)
  - [System Information](#system-information)
  - [Image Management](#image-management)
  - [Container Management](#container-management)
  - [Cleanup](#cleanup)
- [Misc / Notes](#misc--notes)

---

## Overview

| Item | Value |
|------|-------|
| **Product** | Docker Engine (Community Edition) |
| **Version** | `27.3.1` |
| **API Version** | `1.47` |
| **Platform** | `linux/amd64` |
| **OS** | Ubuntu 24.04.1 LTS (WSL2) |
| **Kernel** | `6.6.87.2-microsoft-standard-WSL2` |
| **Hostname** | `wsl` |
| **CPUs** | `12` |
| **Total Memory** | `15.45 GiB` |
| **Installation method** | Docker Engine via `apt` (Docker's official repository) |
| **Docker Desktop** | Not installed — engine runs directly on Ubuntu |

- [Docker Engine Documentation](https://docs.docker.com/engine/)
- [Docker Engine Install on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Docker GitHub](https://github.com/moby/moby)

---

## Engine Details

### Server Configuration

| Item | Value |
|------|-------|
| **Docker Root Dir** | `/var/lib/docker` |
| **Context** | `default` |
| **Cgroup Driver** | `systemd` |
| **Cgroup Version** | `2` |
| **Init Binary** | `docker-init` |
| **Default Runtime** | `runc` |
| **Live Restore** | Enabled |
| **Swarm** | `inactive` |

### Runtime & Containerd

| Component | Version / Path |
|-----------|---------------|
| **runc** | `1.1.14` |
| **containerd** | `57f17b0a` |
| **docker-init** | bundled with Docker Engine |

### Security

| Feature | Status |
|---------|--------|
| **AppArmor** | Available (Ubuntu default) |
| **Seccomp** | Default profile active |
| **User Namespaces** | Not enabled (default) |
| **Rootless mode** | Not enabled (uses docker group instead) |

---

## Plugins

### Buildx

| Item | Value |
|------|-------|
| **Plugin** | Docker Buildx |
| **Version** | `v0.17.1` |
| **Path** | `/usr/libexec/docker/cli-plugins/docker-buildx` |
| **GitHub** | [docker/buildx](https://github.com/docker/buildx) |

Buildx extends `docker build` with BuildKit features including multi-platform builds, cache management, and build contexts.

```shell
# Build with Buildx (default builder)
docker buildx build -t myimage:latest .

# List builders
docker buildx ls

# Create a new builder with docker-container driver
docker buildx create --name mybuilder --use

# Multi-platform build
docker buildx build --platform linux/amd64,linux/arm64 -t myimage:latest .
```

### Compose

| Item | Value |
|------|-------|
| **Plugin** | Docker Compose |
| **Version** | `v2.29.7` |
| **Path** | `/usr/libexec/docker/cli-plugins/docker-compose` |
| **GitHub** | [docker/compose](https://github.com/docker/compose) |

Docker Compose v2 is installed as a CLI plugin (invoked as `docker compose`, not the legacy `docker-compose`).

```shell
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f

# List running services
docker compose ps
```

---

## Storage

### Storage Driver

| Item | Value |
|------|-------|
| **Driver** | `overlay2` |
| **Backing Filesystem** | `extfs` |
| **Supports d_type** | `true` |
| **Using metacopy** | `false` |
| **Native Overlay Diff** | `true` |

`overlay2` is the recommended storage driver for Linux. It operates at the file level and is more efficient than the legacy `aufs` or `devicemapper` drivers.

### Docker Root Directory

| Item | Value |
|------|-------|
| **Path** | `/var/lib/docker` |
| **Contains** | `overlay2/`, `containers/`, `image/`, `volumes/`, `network/`, `buildkit/`, `tmp/` |

```shell
# Check disk usage
docker system df

# Detailed disk usage
docker system df -v
```

---

## Networking

### Network Plugins

| Plugin | Type | Description |
|--------|------|-------------|
| `bridge` | Local | Default network driver; isolated bridge per container |
| `host` | Local | Container shares host's network stack |
| `ipvlan` | Local | L2/L3 mode; assigns VLAN-aware IPs |
| `macvlan` | Local | Assigns unique MAC address; appears as physical device |
| `null` | Local | No networking (loopback only) |
| `overlay` | Global | Multi-host networking for Swarm/multi-node setups |

### Default Networks

| Network | Driver | Scope |
|---------|--------|-------|
| `bridge` | `bridge` | local |
| `host` | `host` | local |
| `none` | `null` | local |

```shell
# List networks
docker network ls

# Inspect a network
docker network inspect bridge

# Create a custom bridge network
docker network create --driver bridge my-network
```

---

## Logging

### Available Log Drivers

| Driver | Description |
|--------|-------------|
| `json-file` | **Default.** Logs stored as JSON on the host filesystem |
| `local` | Optimized local logging with automatic rotation |
| `journald` | Sends logs to systemd journal |
| `syslog` | Sends logs to a syslog server |
| `fluentd` | Sends logs to Fluentd |
| `gelf` | Sends logs in GELF format (e.g., Graylog) |
| `awslogs` | Sends logs to Amazon CloudWatch |
| `splunk` | Sends logs to Splunk via HTTP Event Collector |
| `gcplogs` | Sends logs to Google Cloud Logging |

```shell
# View container logs
docker logs <container>

# Follow logs in real time
docker logs -f --tail 100 <container>

# Run container with a specific log driver
docker run --log-driver=local --log-opt max-size=10m myimage
```

---

## Containers & Images Summary

### Images

| Item | Value |
|------|-------|
| **Total images** | `50` (after cleanup) |
| **Total size** | `37.24 GB` |
| **Registry** | Docker Hub (default) |

> **Cleanup performed (2026-02-22):** Pruned dangling images, stopped containers, orphaned networks, and build cache — reclaimed ~20 GB total.

```shell
# List all images
docker images

# List images with size (sorted)
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}" | sort -k3 -h
```

### Key Dev Container Images

| Image | Approximate Size |
|-------|-----------------|
| `getting-started-with-spec-kit` | ~2.00 GB |
| `getting-started-with-copilot-cli` | ~2.18 GB |
| `getting-started-with-agent-framework` | ~2.50 GB |
| `legacy-spring-boot-23-app` | ~3.06 GB |
| `specflow-mcp-template` | ~2.52 GB |
| `agentic-inner-loop-workshop` | ~0.53 – 1.51 GB |
| `legacy-java-app-for-copilot-quest` | ~964 MB |
| `legacy-struts-hibernate-bookstore` | ~964 MB |

### Containers

| Item | Value |
|------|-------|
| **Running** | `0` |
| **Stopped** | `0` (pruned) |
| **Total** | `0` |

```shell
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Remove all stopped containers
docker container prune
```

---

## Swarm

| Item | Value |
|------|-------|
| **Status** | `inactive` |

Docker Swarm is not initialized. This is a single-node development environment.

```shell
# Initialize Swarm (if needed)
docker swarm init

# Check Swarm status
docker info --format '{{.Swarm.LocalNodeState}}'
```

---

## Installation Reference

Docker Engine is installed directly on Ubuntu (WSL2) using Docker's official APT repository. This is **not** Docker Desktop.

### 1. Set Up Docker's Official APT Repository

```shell
# Remove any conflicting packages
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
    sudo apt-get remove -y $pkg
done

# Add Docker's official GPG key
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the Docker repository to APT sources
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

### 2. Install Docker Engine Packages

```shell
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 3. Verify Installation

```shell
docker --version
# Docker version 27.3.1, build ce12230

docker compose version
# Docker Compose version v2.29.7

docker buildx version
# github.com/docker/buildx v0.17.1

sudo docker run hello-world
```

---

## Non-Root Setup (docker Group)

Running Docker commands without `sudo` by adding the user to the `docker` group.

| Item | Value |
|------|-------|
| **User** | `shinyay` |
| **Group** | `docker` |
| **Status** | ✅ Configured |

### Setup Steps

```shell
# Create docker group (if not exists)
sudo groupadd docker

# Add user to docker group
sudo usermod -aG docker shinyay

# Apply group changes (or restart WSL)
newgrp docker

# Verify non-root access
docker run hello-world
```

### Verify Group Membership

```shell
groups shinyay
# shinyay : shinyay adm ... docker

id -nG shinyay | grep docker
# docker
```

> **Security note:** Members of the `docker` group have root-equivalent privileges on the host. Only add trusted users.

---

## systemd Service

Docker runs as a systemd service on Ubuntu WSL2.

| Item | Value |
|------|-------|
| **Service name** | `docker.service` |
| **Socket** | `docker.socket` |
| **Status** | `active (running)` |
| **Enabled on boot** | Yes |
| **Socket path** | `/var/run/docker.sock` |

### Service Management

```shell
# Check Docker service status
sudo systemctl status docker

# Start Docker service
sudo systemctl start docker

# Stop Docker service
sudo systemctl stop docker

# Restart Docker service
sudo systemctl restart docker

# Enable Docker to start on boot
sudo systemctl enable docker.service
sudo systemctl enable containerd.service

# Disable auto-start
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```

### WSL2 systemd Requirement

systemd must be enabled in WSL2 for Docker to run as a service. Verify or enable in `/etc/wsl.conf`:

```ini
[boot]
systemd=true
```

After changing, restart WSL from PowerShell:

```powershell
wsl --shutdown
```

---

## Fish Shell Integration

### Shell Completion

| Item | Value |
|------|-------|
| **Completion file** | `~/.config/fish/completions/docker.fish` |
| **Status** | ✅ Generated (235 lines) |
| **Generator** | Built-in `docker completion` command |

#### Generate Docker Fish Completion

```shell
docker completion fish > $HOME/.config/fish/completions/docker.fish
```

This provides tab completion for all Docker CLI commands, flags, image names, container names, and more.

#### Verify Completion Works

```shell
# Restart Fish or source completions
source $HOME/.config/fish/completions/docker.fish

# Test: type "docker " then press Tab
docker <Tab>
```

> **Note:** Docker Compose and Buildx completions are included via the main `docker` completion since they are CLI plugins.

### Configured Fish Abbreviations

The following abbreviations are configured in the `set_abbr` function in `~/.config/fish/config.fish`:

| Abbreviation | Expansion | Description |
|-------------|-----------|-------------|
| `dps` | `docker ps` | List running containers |
| `di` | `docker images` | List images |
| `dcu` | `docker compose up -d` | Start Compose services in detached mode |
| `dcd` | `docker compose down` | Stop Compose services |

#### Additional Useful Abbreviations (Optional)

These can be added to `config.fish` if desired:

```fish
abbr --add dpsa  docker ps -a
abbr --add dex   docker exec -it
abbr --add dcl   docker compose logs -f
abbr --add dsp   docker system prune -f
```

---

## VS Code Integration

### Installed Extensions

| Extension ID | Name | Description |
|-------------|------|-------------|
| `ms-azuretools.vscode-docker` | Docker | Build, manage, and deploy containerized apps from VS Code |
| `ms-azuretools.vscode-containers` | Dev Containers | Open any folder or repo inside a Docker container |

### Docker Extension Features

- **Explorer panel:** View and manage images, containers, registries, networks, and volumes
- **Compose support:** Start/stop Compose services from the editor
- **Dockerfile support:** Syntax highlighting, IntelliSense, linting
- **Image inspection:** View image layers and history
- **Container logs:** Stream container logs in the terminal

### Dev Containers Extension Features

- **Remote development:** Open a repository inside a Docker container as a full development environment
- **`.devcontainer/`:** Reads `devcontainer.json` configuration for automated container setup
- **Pre-built images:** Supports Microsoft's dev container images and custom Dockerfiles

### Connecting to Docker Engine

The VS Code Docker extension connects to the Docker Engine via the Unix socket:

| Item | Value |
|------|-------|
| **Docker host** | `unix:///var/run/docker.sock` |
| **Context** | `default` |

> Since Docker Engine runs natively in WSL2 (not Docker Desktop), VS Code connects through the WSL remote extension to the local Docker socket.

---

## Useful Commands Reference

### System Information

| Command | Description |
|---------|-------------|
| `docker version` | Show client and server version |
| `docker info` | Display system-wide information |
| `docker system df` | Show disk usage |
| `docker system df -v` | Show detailed disk usage per image/container/volume |
| `docker context ls` | List available contexts |
| `docker context inspect default` | Inspect the default context |

### Image Management

| Command | Description |
|---------|-------------|
| `docker images` | List local images |
| `docker pull <image>` | Pull an image from a registry |
| `docker build -t <name> .` | Build an image from a Dockerfile |
| `docker buildx build --platform linux/amd64 -t <name> .` | Build with Buildx |
| `docker tag <image> <new-tag>` | Tag an image |
| `docker push <image>` | Push an image to a registry |
| `docker rmi <image>` | Remove an image |
| `docker image prune` | Remove dangling images |
| `docker image prune -a` | Remove all unused images |
| `docker history <image>` | Show image layer history |
| `docker inspect <image>` | Show detailed image metadata |

### Container Management

| Command | Description |
|---------|-------------|
| `docker run -it <image> /bin/bash` | Run interactively with a shell |
| `docker run -d -p 8080:80 <image>` | Run detached with port mapping |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker stop <container>` | Stop a running container |
| `docker start <container>` | Start a stopped container |
| `docker restart <container>` | Restart a container |
| `docker rm <container>` | Remove a stopped container |
| `docker exec -it <container> /bin/bash` | Exec into a running container |
| `docker logs -f <container>` | Follow container logs |
| `docker cp <src> <container>:<dest>` | Copy files to/from container |
| `docker stats` | Live resource usage stats |

### Cleanup

| Command | Description |
|---------|-------------|
| `docker system prune` | Remove stopped containers, unused networks, dangling images |
| `docker system prune -a --volumes` | Remove **all** unused data including volumes |
| `docker container prune` | Remove all stopped containers |
| `docker image prune -a` | Remove all unused images |
| `docker volume prune` | Remove all unused volumes |
| `docker network prune` | Remove all unused networks |
| `docker builder prune` | Remove BuildKit build cache |

---

## Misc / Notes

### Docker Desktop vs Docker Engine

This environment uses **Docker Engine** installed directly on Ubuntu via APT, **not** Docker Desktop. Key differences:

| Aspect | Docker Engine (this setup) | Docker Desktop |
|--------|---------------------------|----------------|
| **Installation** | `apt install docker-ce` | GUI installer (`.exe` / `.dmg`) |
| **Runs on** | Linux directly (WSL2) | Managed Linux VM |
| **GUI** | None (CLI only) | Dashboard, settings UI |
| **License** | Free (Apache 2.0) | Free for personal / small business |
| **Kubernetes** | Not bundled (install separately) | Bundled single-node cluster |
| **Extensions** | CLI plugins only | Docker Desktop extensions marketplace |

### WSL2 Considerations

- Docker Engine runs natively inside the WSL2 Ubuntu instance
- No Windows-side Docker daemon is required
- File system performance is best when working within the Linux filesystem (`/home/...`) rather than `/mnt/c/...`
- The kernel version (`6.6.87.2-microsoft-standard-WSL2`) is managed by Microsoft and updated via Windows Update

### Docker Daemon Configuration

Custom daemon configuration can be added to `/etc/docker/daemon.json`:

```json
{
  "storage-driver": "overlay2",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

> ⚠️ **No `daemon.json` exists on this environment.** Without it, container logs grow unbounded. It is **strongly recommended** to create this file with log rotation settings.

After modifying, restart the daemon:

```shell
sudo systemctl restart docker
```

### Fish Completion Status

Docker Fish shell completion has been **generated** at `~/.config/fish/completions/docker.fish` (235 lines).

```shell
# Regenerate if Docker is upgraded
docker completion fish > $HOME/.config/fish/completions/docker.fish
```

### Upgrade Recommendation

Docker Engine is currently at **27.3.1** (September 2024), which is **2 major versions behind** the latest 29.x release. The upgrade brings:

| Feature | Version | Benefit |
|---------|---------|---------|
| Model Runner (local AI inference) | 28.0+ | Run LLMs locally via `docker model` |
| GPU support for containers | 28.0+ | Native `--gpus` flag support in WSL2 |
| Improved BuildKit performance | 28.0+ | Faster multi-stage builds, better caching |
| Health check improvements | 29.0+ | `start-interval` for faster startup detection |
| Compose Watch v2 | Compose 2.35+ | Hot-reload for development containers |

#### Upgrade Commands (requires sudo)

```shell
# Update package index
sudo apt-get update

# Upgrade Docker Engine + all plugins
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Verify upgrade
docker version

# Regenerate Fish completion after upgrade
docker completion fish > $HOME/.config/fish/completions/docker.fish
```

> **Note:** This upgrade uses Docker's official APT repository already configured on this system.

### Maintenance Schedule

Regular cleanup prevents disk bloat in dev container environments:

```shell
# Weekly: remove dangling images and stopped containers
docker system prune -f

# Monthly: clear build cache
docker builder prune -f

# Check disk usage
docker system df
```
