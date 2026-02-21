# Java & SDKMAN! — Detailed Configuration Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-21

---

## Table of Contents

- [Overview](#overview)
- [SDKMAN! Setup](#sdkman-setup)
  - [Installation](#installation)
  - [Version Information](#version-information)
  - [Directory Structure](#directory-structure)
  - [Fish Shell Integration (sdkman-for-fish)](#fish-shell-integration-sdkman-for-fish)
- [Installed Java Versions](#installed-java-versions)
  - [OpenJDK 24 EA 23 (`24.ea.23-open`)](#openjdk-24-ea-23-24ea23-open)
  - [OpenJDK 23.0.1 (`23.0.1-open`) — Current](#openjdk-2301-2301-open--current)
  - [Microsoft Build of OpenJDK 21.0.5 (`21.0.5-ms`)](#microsoft-build-of-openjdk-2105-2105-ms)
  - [Microsoft Build of OpenJDK 17.0.14 (`17.0.14-ms`)](#microsoft-build-of-openjdk-17014-17014-ms)
  - [Microsoft Build of OpenJDK 11.0.25 (`11.0.25-ms`)](#microsoft-build-of-openjdk-11025-11025-ms)
  - [Azul Zulu 8.0.432 (`8.0.432-zulu`)](#azul-zulu-80432-80432-zulu)
  - [Azul Zulu 7.0.352 (`7.0.352-zulu`)](#azul-zulu-70352-70352-zulu)
- [Other SDKs](#other-sdks)
  - [Kotlin (`2.0.21`)](#kotlin-2021)
  - [Ki — Kotlin Interactive Shell (`0.5.2`)](#ki--kotlin-interactive-shell-052)
  - [Maven (`3.9.9`)](#maven-399)
  - [Spring Boot CLI (`3.3.5`)](#spring-boot-cli-335)
- [Switching Versions](#switching-versions)
  - [Switching Java Versions](#switching-java-versions)
  - [Switching Other SDK Versions](#switching-other-sdk-versions)
  - [Project-Level Version Pinning (`.sdkmanrc`)](#project-level-version-pinning-sdkmanrc)
- [PATH Configuration](#path-configuration)
- [Common SDKMAN! Commands Reference](#common-sdkman-commands-reference)
- [VS Code Integration](#vs-code-integration)
  - [Java Extension Pack](#java-extension-pack)
  - [Spring Boot Extensions](#spring-boot-extensions)
  - [All Java-Related Extensions](#all-java-related-extensions)
  - [VS Code Java Settings](#vs-code-java-settings)

---

## Overview

| Item | Value |
|------|-------|
| **Current Java** | OpenJDK 23.0.1 (`23.0.1-open`) |
| **Total JDKs installed** | 7 |
| **SDK manager** | SDKMAN! (script: 5.18.2, native: 0.4.6) |
| **SDKMAN_DIR** | `~/.sdkman` |
| **Fish integration** | `reitzig/sdkman-for-fish@v2.1.0` |
| **Additional SDKs** | Kotlin 2.0.21, Ki 0.5.2, Maven 3.9.9, Spring Boot CLI 3.3.5 |
| **VS Code extensions** | 12 Java-related extensions installed |

- [SDKMAN! Official Site](https://sdkman.io/)
- [SDKMAN! GitHub](https://github.com/sdkman)
- [OpenJDK](https://openjdk.org/)
- [Microsoft Build of OpenJDK](https://learn.microsoft.com/en-us/java/openjdk/)
- [Azul Zulu](https://www.azul.com/downloads/)

---

## SDKMAN! Setup

### Installation

#### 1. Install SDKMAN! via Homebrew

```shell
brew tap sdkman/tap
brew install sdkman-cli
```

#### 2. Install the Fish shell plugin

```shell
fisher install reitzig/sdkman-for-fish@v2.1.0
```

> **Note:** SDKMAN! is natively a Bash tool. The `sdkman-for-fish` plugin wraps the `sdk` command so it works seamlessly in Fish, including tab completions.

#### 3. Verify installation

```shell
sdk version
# SDKMAN!
#  script: 5.18.2
#  native: 0.4.6
```

### Version Information

| Component | Value |
|-----------|-------|
| **Script version** | 5.18.2 |
| **Native version** | 0.4.6 |
| **SDKMAN_DIR** | `~/.sdkman` |
| **Candidates dir** | `~/.sdkman/candidates/` |
| **Config file** | `~/.sdkman/etc/config` |
| **Init script** | `~/.sdkman/bin/sdkman-init.sh` |

### Directory Structure

```
~/.sdkman/
├── archives/                       # Downloaded SDK archives (.zip, .tar.gz)
├── bin/
│   └── sdkman-init.sh              # SDKMAN! initialization script (Bash)
├── candidates/                     # Installed SDK versions
│   ├── java/
│   │   ├── 24.ea.23-open/          #   OpenJDK 24 EA 23
│   │   ├── 23.0.1-open/            #   OpenJDK 23.0.1
│   │   ├── 21.0.5-ms/              #   Microsoft OpenJDK 21
│   │   ├── 17.0.14-ms/             #   Microsoft OpenJDK 17
│   │   ├── 11.0.25-ms/             #   Microsoft OpenJDK 11
│   │   ├── 8.0.432-zulu/           #   Azul Zulu 8
│   │   ├── 7.0.352-zulu/           #   Azul Zulu 7
│   │   └── current -> 23.0.1-open  #   Symlink to active version
│   ├── kotlin/
│   │   ├── 2.0.21/
│   │   └── current -> 2.0.21
│   ├── ki/
│   │   ├── 0.5.2/
│   │   └── current -> 0.5.2
│   ├── maven/
│   │   ├── 3.9.9/
│   │   └── current -> 3.9.9
│   └── springboot/
│       ├── 3.3.5/
│       └── current -> 3.3.5
├── etc/
│   └── config                      # SDKMAN! configuration
├── src/                            # SDKMAN! source scripts
├── tmp/                            # Temporary files
└── var/                            # State files
```

### Fish Shell Integration (sdkman-for-fish)

| Item | Value |
|------|-------|
| **Plugin** | [reitzig/sdkman-for-fish](https://github.com/reitzig/sdkman-for-fish) |
| **Version** | v2.1.0 |
| **Installed via** | Fisher |
| **Config file** | `~/.config/fish/conf.d/sdk.fish` |
| **Completions** | `~/.config/fish/completions/sdk.fish` |
| **Function** | `~/.config/fish/functions/sdk.fish` |

#### How it works

The plugin wraps SDKMAN!'s Bash-based `sdk` command so it can be used from Fish. It:

1. Sources `sdkman-init.sh` in a Bash subshell
2. Captures environment variable changes (e.g., `JAVA_HOME`, `PATH`)
3. Applies them to the Fish session
4. Provides Fish-native tab completions for all `sdk` subcommands

#### Usage in Fish

```fish
sdk install java 23.0.1-open    # Install a Java version
sdk use java 21.0.5-ms          # Switch Java for current session
sdk default java 23.0.1-open    # Set default Java version
sdk list java                   # List available Java versions
sdk current                     # Show current SDK versions
```

---

## Installed Java Versions

7 JDK versions are installed, covering Java 7 through Java 24 (Early Access).

| # | Vendor | Version | Identifier | LTS | Status |
|---|--------|---------|-----------|-----|--------|
| 1 | OpenJDK | 24 EA 23 | `24.ea.23-open` | No (EA) | local only |
| 2 | OpenJDK | 23.0.1 | `23.0.1-open` | No | **installed, current** |
| 3 | Microsoft | 21.0.5 | `21.0.5-ms` | **Yes** | local only |
| 4 | Microsoft | 17.0.14 | `17.0.14-ms` | **Yes** | local only |
| 5 | Microsoft | 11.0.25 | `11.0.25-ms` | **Yes** | local only |
| 6 | Azul Zulu | 8.0.432 | `8.0.432-zulu` | **Yes** | local only |
| 7 | Azul Zulu | 7.0.352 | `7.0.352-zulu` | **Yes** | installed |

> **"local only"** means the version was installed and is available locally but is not set as the current default.

---

### OpenJDK 24 EA 23 (`24.ea.23-open`)

| Item | Value |
|------|-------|
| **Vendor** | OpenJDK (Oracle) |
| **Version** | 24 Early Access, Build 23 |
| **Identifier** | `24.ea.23-open` |
| **LTS** | No (Early Access) |
| **Status** | local only |
| **Install path** | `~/.sdkman/candidates/java/24.ea.23-open/` |
| **Release** | [openjdk.org/projects/jdk/24](https://openjdk.org/projects/jdk/24/) |

#### Install

```shell
sdk install java 24.ea.23-open
```

#### Key features (preview)

- Ahead-of-Time class loading & linking
- Stream Gatherers (standard)
- Structured Concurrency (preview)
- Scoped Values (preview)
- Flexible Constructor Bodies (standard)

---

### OpenJDK 23.0.1 (`23.0.1-open`) — Current

| Item | Value |
|------|-------|
| **Vendor** | OpenJDK (Oracle) |
| **Version** | 23.0.1 |
| **Identifier** | `23.0.1-open` |
| **LTS** | No |
| **Status** | **installed, current default** |
| **Install path** | `~/.sdkman/candidates/java/23.0.1-open/` |
| **Symlink** | `~/.sdkman/candidates/java/current -> 23.0.1-open` |
| **Release** | [openjdk.org/projects/jdk/23](https://openjdk.org/projects/jdk/23/) |

#### Install & set as default

```shell
sdk install java 23.0.1-open
sdk default java 23.0.1-open
```

#### Key features

- Primitive Types in Patterns, `instanceof`, and `switch` (preview)
- Stream Gatherers (preview)
- Structured Concurrency (preview)
- Scoped Values (preview)
- Implicitly Declared Classes and Instance Main Methods (preview)
- Markdown Documentation Comments

---

### Microsoft Build of OpenJDK 21.0.5 (`21.0.5-ms`)

| Item | Value |
|------|-------|
| **Vendor** | Microsoft |
| **Version** | 21.0.5 |
| **Identifier** | `21.0.5-ms` |
| **LTS** | **Yes** (until Sep 2028) |
| **Status** | local only |
| **Install path** | `~/.sdkman/candidates/java/21.0.5-ms/` |
| **Release** | [learn.microsoft.com/en-us/java/openjdk](https://learn.microsoft.com/en-us/java/openjdk/) |

#### Install

```shell
sdk install java 21.0.5-ms
```

#### Key features (Java 21 LTS)

- Virtual Threads (standard — Project Loom)
- Record Patterns (standard)
- Pattern Matching for `switch` (standard)
- Sequenced Collections
- String Templates (preview)
- Unnamed Patterns and Variables (preview)
- Unnamed Classes and Instance Main Methods (preview)

---

### Microsoft Build of OpenJDK 17.0.14 (`17.0.14-ms`)

| Item | Value |
|------|-------|
| **Vendor** | Microsoft |
| **Version** | 17.0.14 |
| **Identifier** | `17.0.14-ms` |
| **LTS** | **Yes** (until Sep 2027) |
| **Status** | local only |
| **Install path** | `~/.sdkman/candidates/java/17.0.14-ms/` |
| **Release** | [learn.microsoft.com/en-us/java/openjdk](https://learn.microsoft.com/en-us/java/openjdk/) |

#### Install

```shell
sdk install java 17.0.14-ms
```

#### Key features (Java 17 LTS)

- Sealed Classes (standard)
- Pattern Matching for `instanceof` (standard)
- Records (standard)
- Text Blocks (standard)
- Switch Expressions (standard)
- Strong encapsulation of JDK internals

---

### Microsoft Build of OpenJDK 11.0.25 (`11.0.25-ms`)

| Item | Value |
|------|-------|
| **Vendor** | Microsoft |
| **Version** | 11.0.25 |
| **Identifier** | `11.0.25-ms` |
| **LTS** | **Yes** (until Sep 2027) |
| **Status** | local only |
| **Install path** | `~/.sdkman/candidates/java/11.0.25-ms/` |
| **Release** | [learn.microsoft.com/en-us/java/openjdk](https://learn.microsoft.com/en-us/java/openjdk/) |

#### Install

```shell
sdk install java 11.0.25-ms
```

#### Key features (Java 11 LTS)

- Local-Variable Syntax for Lambda Parameters (`var`)
- HTTP Client (standard)
- Nest-Based Access Control
- Dynamic Class-File Constants
- Flight Recorder (open-sourced)
- `String` new methods: `isBlank()`, `lines()`, `strip()`, `repeat()`

---

### Azul Zulu 8.0.432 (`8.0.432-zulu`)

| Item | Value |
|------|-------|
| **Vendor** | Azul Systems (Zulu) |
| **Version** | 8.0.432 (Java SE 8) |
| **Identifier** | `8.0.432-zulu` |
| **LTS** | **Yes** (extended support) |
| **Status** | local only |
| **Install path** | `~/.sdkman/candidates/java/8.0.432-zulu/` |
| **Release** | [azul.com/downloads](https://www.azul.com/downloads/) |

#### Install

```shell
sdk install java 8.0.432-zulu
```

#### Key features (Java 8)

- Lambda Expressions
- Stream API
- `java.time` (Date and Time API)
- Default Methods in Interfaces
- Optional class
- Nashorn JavaScript engine

> **Note:** Java 8 is used for legacy project maintenance and backward-compatibility testing.

---

### Azul Zulu 7.0.352 (`7.0.352-zulu`)

| Item | Value |
|------|-------|
| **Vendor** | Azul Systems (Zulu) |
| **Version** | 7.0.352 (Java SE 7) |
| **Identifier** | `7.0.352-zulu` |
| **LTS** | **Yes** (end of life) |
| **Status** | installed |
| **Install path** | `~/.sdkman/candidates/java/7.0.352-zulu/` |
| **Release** | [azul.com/downloads](https://www.azul.com/downloads/) |

#### Install

```shell
sdk install java 7.0.352-zulu
```

#### Key features (Java 7)

- Try-with-resources
- Diamond Operator (`<>`)
- Strings in `switch`
- Multi-catch exceptions
- NIO.2 (New I/O)
- Fork/Join framework

> **Note:** Java 7 is used for legacy project maintenance. It has reached end of life.

---

## Other SDKs

### Kotlin (`2.0.21`)

| Item | Value |
|------|-------|
| **Version** | 2.0.21 |
| **Status** | current |
| **Install path** | `~/.sdkman/candidates/kotlin/2.0.21/` |
| **PATH entry** | `~/.sdkman/candidates/kotlin/current/bin` |
| **Official site** | [kotlinlang.org](https://kotlinlang.org/) |

#### Install

```shell
sdk install kotlin 2.0.21
```

#### Included tools

| Tool | Description |
|------|-------------|
| `kotlinc` | Kotlin compiler (JVM) |
| `kotlin` | Run Kotlin scripts / REPL |
| `kotlinc-js` | Kotlin/JS compiler |
| `kotlin-dce-js` | Dead code elimination for Kotlin/JS |

#### Key features (Kotlin 2.0)

- K2 compiler (stable) — significantly faster compilation
- Smart cast improvements
- Kotlin Multiplatform stable
- `data object` declarations

---

### Ki — Kotlin Interactive Shell (`0.5.2`)

| Item | Value |
|------|-------|
| **Version** | 0.5.2 |
| **Status** | current |
| **Install path** | `~/.sdkman/candidates/ki/0.5.2/` |
| **PATH entry** | `~/.sdkman/candidates/ki/current/bin` |
| **GitHub** | [Kotlin/kotlin-interactive-shell](https://github.com/Kotlin/kotlin-interactive-shell) |

#### Install

```shell
sdk install ki 0.5.2
```

#### Usage

```shell
ki           # Launch interactive Kotlin REPL
```

Ki provides an enhanced Kotlin REPL with syntax highlighting, auto-completion, and dependency loading via Maven coordinates.

---

### Maven (`3.9.9`)

| Item | Value |
|------|-------|
| **Version** | 3.9.9 |
| **Status** | current |
| **Install path** | `~/.sdkman/candidates/maven/3.9.9/` |
| **PATH entry** | `~/.sdkman/candidates/maven/current/bin` |
| **Official site** | [maven.apache.org](https://maven.apache.org/) |

#### Install

```shell
sdk install maven 3.9.9
```

#### Common commands

```shell
mvn --version                          # Check version
mvn clean install                      # Clean build and install to local repo
mvn clean package -DskipTests          # Build without tests
mvn dependency:tree                    # Show dependency tree
mvn spring-boot:run                    # Run Spring Boot app (with plugin)
mvn archetype:generate                 # Generate project from archetype
```

---

### Spring Boot CLI (`3.3.5`)

| Item | Value |
|------|-------|
| **Version** | 3.3.5 |
| **Status** | current |
| **Install path** | `~/.sdkman/candidates/springboot/3.3.5/` |
| **PATH entry** | `~/.sdkman/candidates/springboot/current/bin` |
| **Official site** | [spring.io](https://spring.io/) |

#### Install

```shell
sdk install springboot 3.3.5
```

#### Common commands

```shell
spring version                         # Check version
spring init --dependencies=web myapp   # Generate new project
spring init --list                     # List available dependencies
spring shell                           # Launch Spring Shell
```

---

## Switching Versions

### Switching Java Versions

#### Switch for the current session only

```shell
sdk use java 21.0.5-ms
```

> This sets `JAVA_HOME` and updates `PATH` for the current shell session only. When you open a new terminal, it reverts to the default.

#### Switch the default permanently

```shell
sdk default java 21.0.5-ms
```

> This updates the `~/.sdkman/candidates/java/current` symlink. All new shells will use this version.

#### Verify the active version

```shell
sdk current java
# Using java version 23.0.1-open

java -version
# openjdk version "23.0.1" 2024-10-15
# OpenJDK Runtime Environment (build 23.0.1+11)
# OpenJDK 64-Bit Server VM (build 23.0.1+11, mixed mode, sharing)
```

#### Quick-switch examples

```shell
# Switch to Java 21 LTS for a Spring Boot 3.x project
sdk use java 21.0.5-ms

# Switch to Java 17 LTS for an older Spring Boot 2.x project
sdk use java 17.0.14-ms

# Switch to Java 11 for legacy microservices
sdk use java 11.0.25-ms

# Switch to Java 8 for legacy enterprise apps
sdk use java 8.0.432-zulu

# Switch back to latest
sdk use java 23.0.1-open
```

### Switching Other SDK Versions

```shell
sdk use kotlin 2.0.21
sdk use maven 3.9.9
sdk use springboot 3.3.5
```

### Project-Level Version Pinning (`.sdkmanrc`)

Create a `.sdkmanrc` file in your project root to automatically switch SDK versions when entering the directory:

```properties
# .sdkmanrc
java=21.0.5-ms
maven=3.9.9
```

#### Enable auto-env in SDKMAN! config

Edit `~/.sdkman/etc/config`:

```properties
sdkman_auto_env=true
```

When `sdkman_auto_env=true`, SDKMAN! automatically runs `sdk env` when you `cd` into a directory containing `.sdkmanrc`.

#### Manual usage

```shell
sdk env               # Apply versions from .sdkmanrc
sdk env init          # Create .sdkmanrc with current versions
sdk env install       # Install all versions listed in .sdkmanrc
sdk env clear         # Reset to default versions
```

---

## PATH Configuration

SDKMAN! adds the following entries to `PATH` (via `current` symlinks):

```
~/.sdkman/candidates/springboot/current/bin
~/.sdkman/candidates/maven/current/bin
~/.sdkman/candidates/kotlin/current/bin
~/.sdkman/candidates/ki/current/bin
~/.sdkman/candidates/java/current/bin
```

Each `current` directory is a symlink to the active version:

| SDK | `current` → | Resolved path |
|-----|-------------|---------------|
| Java | `23.0.1-open` | `~/.sdkman/candidates/java/23.0.1-open/bin` |
| Kotlin | `2.0.21` | `~/.sdkman/candidates/kotlin/2.0.21/bin` |
| Ki | `0.5.2` | `~/.sdkman/candidates/ki/0.5.2/bin` |
| Maven | `3.9.9` | `~/.sdkman/candidates/maven/3.9.9/bin` |
| Spring Boot | `3.3.5` | `~/.sdkman/candidates/springboot/3.3.5/bin` |

> **Note:** `JAVA_HOME` is automatically set by SDKMAN! to the active Java candidate directory (e.g., `~/.sdkman/candidates/java/current`).

---

## Common SDKMAN! Commands Reference

### Installation & Management

| Command | Description |
|---------|-------------|
| `sdk install java 23.0.1-open` | Install a specific Java version |
| `sdk uninstall java 7.0.352-zulu` | Uninstall a Java version |
| `sdk rm java 7.0.352-zulu` | Alias for `uninstall` |

### Version Switching

| Command | Description |
|---------|-------------|
| `sdk use java 21.0.5-ms` | Switch Java for current session |
| `sdk default java 23.0.1-open` | Set default Java version |
| `sdk current` | Show all current SDK versions |
| `sdk current java` | Show current Java version |

### Listing & Discovery

| Command | Description |
|---------|-------------|
| `sdk list` | List all available SDK candidates |
| `sdk list java` | List all available Java versions |
| `sdk list kotlin` | List all available Kotlin versions |
| `sdk list maven` | List all available Maven versions |

### Environment & Configuration

| Command | Description |
|---------|-------------|
| `sdk env` | Apply versions from `.sdkmanrc` |
| `sdk env init` | Create `.sdkmanrc` with current versions |
| `sdk env install` | Install all versions in `.sdkmanrc` |
| `sdk env clear` | Reset to default versions |
| `sdk home java 21.0.5-ms` | Print the home directory of a specific version |

### Maintenance

| Command | Description |
|---------|-------------|
| `sdk version` | Show SDKMAN! version |
| `sdk selfupdate` | Update SDKMAN! itself |
| `sdk update` | Refresh the local candidate list |
| `sdk flush` | Clear SDKMAN! caches |
| `sdk flush archives` | Clear downloaded archives |
| `sdk flush tmp` | Clear temporary files |
| `sdk upgrade` | Show outdated candidates |
| `sdk upgrade java` | Upgrade Java to latest version |

### Offline Mode

| Command | Description |
|---------|-------------|
| `sdk offline enable` | Enable offline mode |
| `sdk offline disable` | Disable offline mode |

---

## VS Code Integration

### Java Extension Pack

The **Extension Pack for Java** (`vscjava.vscode-java-pack`) bundles the essential Java extensions into a single install:

| Extension | ID | Description |
|-----------|----|-------------|
| Language Support for Java | `redhat.java` | IntelliSense, code navigation, refactoring (powered by Eclipse JDT Language Server) |
| Debugger for Java | `vscjava.vscode-java-debug` | Debugging support (breakpoints, watch, call stack) |
| Java Test Runner | `vscjava.vscode-java-test` | Run/debug JUnit 4/5 and TestNG tests |
| Maven for Java | `vscjava.vscode-maven` | Maven project management, POM editing, dependency management |
| Project Manager for Java | `vscjava.vscode-java-dependency` | Project view, dependency management, classpath configuration |
| Gradle for Java | `vscjava.vscode-gradle` | Gradle build tool integration |

#### Install

```shell
code --install-extension vscjava.vscode-java-pack
```

### Spring Boot Extensions

| Extension | ID | Description |
|-----------|----|-------------|
| Spring Boot Extension Pack | `vmware.vscode-boot-dev-pack` | Bundles Spring Boot + Spring Initializr + Dashboard |
| Spring Boot Tools | `vmware.vscode-spring-boot` | Rich support for Spring Boot `application.properties`/`application.yml`, live running app info |
| Spring Initializr | `vscjava.vscode-spring-initializr` | Generate Spring Boot projects from within VS Code |
| Spring Boot Dashboard | `vscjava.vscode-spring-boot-dashboard` | Manage and run Spring Boot apps from a sidebar panel |

#### Install

```shell
code --install-extension vmware.vscode-boot-dev-pack
```

### All Java-Related Extensions

| # | Extension | ID | Category |
|---|-----------|-----|----------|
| 1 | Language Support for Java | `redhat.java` | Core |
| 2 | Debugger for Java | `vscjava.vscode-java-debug` | Core |
| 3 | Project Manager for Java | `vscjava.vscode-java-dependency` | Core |
| 4 | Extension Pack for Java | `vscjava.vscode-java-pack` | Core (meta) |
| 5 | Java Test Runner | `vscjava.vscode-java-test` | Core |
| 6 | Java Upgrade Assistant | `vscjava.vscode-java-upgrade` | Migration |
| 7 | Maven for Java | `vscjava.vscode-maven` | Build |
| 8 | Gradle for Java | `vscjava.vscode-gradle` | Build |
| 9 | Spring Boot Dashboard | `vscjava.vscode-spring-boot-dashboard` | Spring |
| 10 | Spring Initializr | `vscjava.vscode-spring-initializr` | Spring |
| 11 | Spring Boot Extension Pack | `vmware.vscode-boot-dev-pack` | Spring (meta) |
| 12 | Spring Boot Tools | `vmware.vscode-spring-boot` | Spring |
| 13 | Migrate Java to Azure | `vscjava.migrate-java-to-azure` | Cloud |

#### Install all at once

```shell
code --install-extension redhat.java
code --install-extension vscjava.vscode-java-debug
code --install-extension vscjava.vscode-java-dependency
code --install-extension vscjava.vscode-java-pack
code --install-extension vscjava.vscode-java-test
code --install-extension vscjava.vscode-java-upgrade
code --install-extension vscjava.vscode-maven
code --install-extension vscjava.vscode-gradle
code --install-extension vscjava.vscode-spring-boot-dashboard
code --install-extension vscjava.vscode-spring-initializr
code --install-extension vmware.vscode-boot-dev-pack
code --install-extension vmware.vscode-spring-boot
code --install-extension vscjava.migrate-java-to-azure
```

### VS Code Java Settings

Recommended `settings.json` entries for Java development with SDKMAN!-managed JDKs:

```jsonc
{
  // Point to the SDKMAN! current JDK for the Language Server
  "java.jdt.ls.java.home": "~/.sdkman/candidates/java/current",

  // Configure multiple installed JDK runtimes for project-specific use
  "java.configuration.runtimes": [
    {
      "name": "JavaSE-24",
      "path": "~/.sdkman/candidates/java/24.ea.23-open"
    },
    {
      "name": "JavaSE-23",
      "path": "~/.sdkman/candidates/java/23.0.1-open",
      "default": true
    },
    {
      "name": "JavaSE-21",
      "path": "~/.sdkman/candidates/java/21.0.5-ms"
    },
    {
      "name": "JavaSE-17",
      "path": "~/.sdkman/candidates/java/17.0.14-ms"
    },
    {
      "name": "JavaSE-11",
      "path": "~/.sdkman/candidates/java/11.0.25-ms"
    },
    {
      "name": "JavaSE-1.8",
      "path": "~/.sdkman/candidates/java/8.0.432-zulu"
    },
    {
      "name": "JavaSE-1.7",
      "path": "~/.sdkman/candidates/java/7.0.352-zulu"
    }
  ],

  // Maven executable managed by SDKMAN!
  "maven.executable.path": "~/.sdkman/candidates/maven/current/bin/mvn",

  // Spring Boot Live Information
  "boot-java.live-information.automatic-tracking.on": true
}
```

> **Tip:** VS Code uses `java.configuration.runtimes` to determine which JDK to use for compiling a project based on the source level specified in `pom.xml` or `build.gradle`. The `java.jdt.ls.java.home` setting controls which JDK runs the Language Server itself (should be the latest available).
