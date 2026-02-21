# Visual Studio Code — Detailed Configuration Reference

> **Environment:** WSL2 / Ubuntu on Windows 11  
> **Last updated:** 2026-02-22

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Extension Inventory](#extension-inventory)
  - [GitHub & Copilot (9)](#github--copilot-9)
  - [Azure (15)](#azure-15)
  - [Java (13)](#java-13)
  - [Docker & Kubernetes (5)](#docker--kubernetes-5)
  - [Python (5)](#python-5)
  - [AI & Copilot Agents (3)](#ai--copilot-agents-3)
  - [Remote Development (2)](#remote-development-2)
  - [Other (6)](#other-6)
  - [Recommended Productivity Extensions (2)](#recommended-productivity-extensions-2)
- [Editor Settings](#editor-settings)
- [Workbench Settings](#workbench-settings)
- [GitHub Copilot Configuration](#github-copilot-configuration)
- [Git Settings](#git-settings)
- [Terminal Settings](#terminal-settings)
- [Agent & MCP Configuration](#agent--mcp-configuration)
- [Recommended Productivity Extensions](#recommended-productivity-extensions)
- [Profile Info](#profile-info)
- [Fish Shell Integration](#fish-shell-integration)

---

## Overview

| Item | Value |
|------|-------|
| **Editor** | Visual Studio Code (Insiders) |
| **Version** | `1.110.0-insider` |
| **Stable version** | `1.109.5` |
| **Installed via** | Snap (classic confinement) |
| **Primary binary** | `code-insiders` |
| **Fish abbreviation** | `code` → `code-insiders` |
| **Total extensions** | 60 |
| **Color theme** | GitHub Dark Colorblind (Beta) |
| **Settings location** | Windows side: `%APPDATA%/Code - Insiders/User/settings.json` |

- [VS Code Official Site](https://code.visualstudio.com/)
- [VS Code Insiders](https://code.visualstudio.com/insiders/)
- [VS Code GitHub](https://github.com/microsoft/vscode)

---

## Installation

### 1. Install VS Code Insiders via Snap

```shell
sudo snap install code-insiders --classic
```

### 2. Install VS Code Stable via Snap

```shell
sudo snap install code --classic
```

### 3. Verify installations

```shell
code-insiders --version
# 1.110.0-insider

code --version
# 1.109.5
```

### 4. Fish shell abbreviation

The `code` command is abbreviated to `code-insiders` in Fish so that typing `code` always launches the Insiders build:

```fish
abbr --add code code-insiders
```

This abbreviation is defined in `~/.config/fish/config.fish` inside the `set_abbr` function.

---

## Extension Inventory

**60 extensions** installed across 9 categories.

### GitHub & Copilot (9)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `github.copilot-chat` | GitHub Copilot Chat | AI-powered chat interface for code assistance |
| `github.github-vscode-theme` | GitHub Theme | Official GitHub color themes for VS Code |
| `github.remotehub` | GitHub Repositories (RemoteHub) | Browse and edit GitHub repos without cloning |
| `github.vscode-codeql` | CodeQL | Query language for code analysis and security |
| `github.vscode-github-actions` | GitHub Actions | Manage and monitor GitHub Actions workflows |
| `github.vscode-pull-request-github` | GitHub Pull Requests | Review and manage pull requests from VS Code |
| `ms-vscode.vscode-copilot-vision` | Copilot Vision | Visual understanding capabilities for Copilot |
| `ms-vscode.vscode-websearchforcopilot` | Web Search for Copilot | Web search integration for Copilot Chat |
| `github.codespaces` | GitHub Codespaces | Cloud-hosted development environments |

---

### Azure (15)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `apidev.azure-api-center` | Azure API Center | Discover, try, and consume APIs from Azure API Center |
| `ms-azuretools.azure-dev` | Azure Developer CLI | Integration with `azd` for Azure developer workflows |
| `ms-azuretools.vscode-azure-github-copilot` | Azure for Copilot | Azure-specific Copilot chat participant (`@azure`) |
| `ms-azuretools.vscode-azure-mcp-server` | Azure MCP Server | Model Context Protocol server for Azure services |
| `ms-azuretools.vscode-azureappservice` | Azure App Service | Deploy and manage Azure App Service web apps |
| `ms-azuretools.vscode-azurecontainerapps` | Azure Container Apps | Deploy and manage Azure Container Apps |
| `ms-azuretools.vscode-azurefunctions` | Azure Functions | Create, debug, and deploy Azure Functions |
| `ms-azuretools.vscode-azureresourcegroups` | Azure Resources | Unified view of all Azure resources |
| `ms-azuretools.vscode-azurestaticwebapps` | Azure Static Web Apps | Create and manage Azure Static Web Apps |
| `ms-azuretools.vscode-azurestorage` | Azure Storage | Manage Azure Blob, File, Queue, and Table storage |
| `ms-azuretools.vscode-azurevirtualmachines` | Azure Virtual Machines | Create and manage Azure Virtual Machines |
| `ms-azuretools.vscode-bicep` | Bicep | Language support for Azure Bicep (IaC) |
| `ms-azuretools.vscode-cosmosdb` | Azure Databases | Manage Azure Cosmos DB and MongoDB resources |
| `ms-vscode.vscode-node-azure-pack` | Azure Tools (Node.js) | All-in-one extension pack for Node.js + Azure |
| `teamsdevapp.vscode-ai-foundry` | Azure AI Foundry | Build and deploy AI models with Azure AI |

---

### Java (13)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `redhat.java` | Language Support for Java | IntelliSense, refactoring, and compilation via Eclipse JDT.LS |
| `vscjava.vscode-java-debug` | Debugger for Java | Lightweight Java debugger based on Java Debug Server |
| `vscjava.vscode-java-dependency` | Project Manager for Java | Manage Java projects, dependencies, and classpath |
| `vscjava.vscode-java-pack` | Extension Pack for Java | Meta-pack bundling core Java extensions |
| `vscjava.vscode-java-test` | Test Runner for Java | Run and debug JUnit / TestNG tests |
| `vscjava.vscode-java-upgrade` | Java Upgrade | Assist with Java version upgrades and migration |
| `vscjava.vscode-maven` | Maven for Java | Maven project management and POM editing |
| `vscjava.vscode-gradle` | Gradle for Java | Gradle project management and task execution |
| `vscjava.vscode-spring-boot-dashboard` | Spring Boot Dashboard | Manage and launch Spring Boot applications |
| `vscjava.vscode-spring-initializr` | Spring Initializr | Generate Spring Boot projects from VS Code |
| `vmware.vscode-boot-dev-pack` | Spring Boot Extension Pack | Meta-pack for Spring Boot development |
| `vmware.vscode-spring-boot` | Spring Boot Tools | Rich support for Spring Boot `application.properties` / YAML |
| `vscjava.migrate-java-to-azure` | Migrate Java to Azure | Guided migration of Java applications to Azure |

---

### Docker & Kubernetes (5)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `ms-azuretools.vscode-docker` | Docker | Build, manage, and deploy containerized applications |
| `ms-azuretools.vscode-containers` | Dev Containers | Open folders inside Docker containers for development |
| `ms-kubernetes-tools.vscode-kubernetes-tools` | Kubernetes | Develop, deploy, and debug Kubernetes applications |
| `ms-kubernetes-tools.vscode-aks-tools` | Azure Kubernetes Service | Manage and monitor AKS clusters |
| `docker.docker` | Docker DX | Docker Desktop integration and developer experience |

---

### Python (5)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `ms-python.python` | Python | IntelliSense, linting, debugging, and formatting |
| `ms-python.vscode-pylance` | Pylance | Fast, feature-rich Python language server |
| `ms-python.debugpy` | Python Debugger | Python debugging with debugpy |
| `ms-python.vscode-python-envs` | Python Environments | Manage Python environments and interpreters |
| `charliermarsh.ruff` | Ruff | Extremely fast Python linter and formatter (Rust-based) |

---

### AI & Copilot Agents (3)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `ms-windows-ai-studio.windows-ai-studio` | Windows AI Studio | Develop and test AI models locally on Windows |
| `rooveterinaryinc.roo-cline` | Roo Cline | AI coding agent (fork of Cline) |
| `saoudrizwan.claude-dev` | Cline (Claude Dev) | Autonomous AI coding agent powered by Claude |

---

### Remote Development (2)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `ms-vscode.remote-repositories` | Remote Repositories | Browse and edit remote repos without cloning |
| `ms-vscode.azure-repos` | Azure Repos | Browse and edit Azure DevOps repositories |

---

### Other (6)

| Extension ID | Name | Description |
|-------------|------|-------------|
| `ms-ceintl.vscode-language-pack-ja` | Japanese Language Pack | Japanese localization for VS Code UI |
| `marp-team.marp-vscode` | Marp for VS Code | Create slide presentations in Markdown |
| `ms-dotnettools.vscode-dotnet-modernize` | .NET Modernize | Modernize .NET applications |
| `ms-dotnettools.vscode-dotnet-runtime` | .NET Runtime | Acquire .NET runtime for VS Code extensions |
| `ms-vscode.makefile-tools` | Makefile Tools | IntelliSense and build support for Makefiles |
| `redhat.vscode-yaml` | YAML | YAML language support with JSON Schema validation |

---

### Recommended Productivity Extensions (2)

| Extension ID | Name | Version | Description |
|-------------|------|---------|-------------|
| `eamodio.gitlens` | GitLens | 17.10.1 | Advanced Git integration: blame, history, file annotations, graph |
| `usernamehw.errorlens` | Error Lens | 3.28.0 | Highlights errors and warnings inline in the editor |

> These extensions were installed as part of the VS Code ecosystem modernization (2026-02-22).

---

## Editor Settings

Settings that control the code editing experience.

| Setting | Value | Description |
|---------|-------|-------------|
| `editor.mouseWheelZoom` | `true` | Zoom in/out with `Ctrl+MouseWheel` |
| `editor.renderControlCharacters` | `true` | Show control characters (e.g., tabs, zero-width spaces) |
| `editor.renderLineHighlight` | `"all"` | Highlight current line in both editor and gutter |
| `editor.cursorBlinking` | `"expand"` | Cursor blinks with an expand animation |
| `editor.cursorSmoothCaretAnimation` | `"on"` | Smooth animated cursor movement |
| `editor.cursorStyle` | `"block"` | Block cursor |
| `editor.formatOnPaste` | `true` | Auto-format code when pasted |
| `editor.formatOnType` | `true` | Auto-format code as you type |
| `editor.minimap.enabled` | `false` | Minimap (code overview) is disabled |
| `files.insertFinalNewline` | `true` | Ensure files end with a newline character |
| `files.trimTrailingWhitespace` | `true` | Remove trailing whitespace on save |
| `editor.formatOnSave` | `true` | Auto-format code when saving |
| `editor.bracketPairColorization.enabled` | `true` | Colorize matching bracket pairs |
| `editor.guides.bracketPairs` | `true` | Show bracket pair guides |
| `editor.linkedEditing` | `true` | Auto-rename paired HTML/XML tags |
| `editor.stickyScroll.enabled` | `true` | Show current scope at top of editor |

### JSON representation

```json
{
    "editor.mouseWheelZoom": true,
    "editor.renderControlCharacters": true,
    "editor.renderLineHighlight": "all",
    "editor.cursorBlinking": "expand",
    "editor.cursorSmoothCaretAnimation": "on",
    "editor.cursorStyle": "block",
    "editor.formatOnPaste": true,
    "editor.formatOnType": true,
    "editor.formatOnSave": true,
    "editor.minimap.enabled": false,
    "editor.bracketPairColorization.enabled": true,
    "editor.guides.bracketPairs": true,
    "editor.linkedEditing": true,
    "editor.stickyScroll.enabled": true,
    "files.insertFinalNewline": true,
    "files.trimTrailingWhitespace": true
}
```

---

## Workbench Settings

Settings that control the VS Code workbench appearance and behavior.

| Setting | Value | Description |
|---------|-------|-------------|
| `workbench.colorTheme` | `"GitHub Dark Colorblind (Beta)"` | Color theme from `github.github-vscode-theme` |

### Color theme details

| Item | Value |
|------|-------|
| **Theme name** | GitHub Dark Colorblind (Beta) |
| **Extension** | `github.github-vscode-theme` |
| **Type** | Dark |
| **Publisher** | GitHub |

### JSON representation

```json
{
    "workbench.colorTheme": "GitHub Dark Colorblind (Beta)"
}
```

---

## GitHub Copilot Configuration

Settings controlling GitHub Copilot behavior and AI-assisted development.

| Setting | Value | Description |
|---------|-------|-------------|
| `github.copilot.enable.*` | `true` | Enable Copilot for all file types |
| `github.copilot.enable.plaintext` | `true` | Enable Copilot in plain text files |
| `github.copilot.enable.markdown` | `true` | Enable Copilot in Markdown files |
| `github.copilot.enable.scminput` | `false` | Disable Copilot in SCM commit input |
| `github.copilot.advanced.debug.overrideEngine` | `"chat-v3"` | Override Copilot chat engine |
| `github.copilot.chat.scopeSelection` | `true` | Enable scope selection in chat |
| `github.copilot.chat.followUps` | `"always"` | Always show follow-up suggestions |
| `github.copilot.chat.search.semanticTextResults` | `true` | Enable semantic search in chat |
| `github.copilot.chat.generateTests.codeLens` | `true` | Show "Generate Tests" CodeLens |
| `github.copilot.chat.inlineChatCompletionTrigger.enabled` | `true` | Enable inline chat completion trigger |
| `github.copilot.chat.inlineChatHint.enabled` | `true` | Show inline chat hint |
| `github.copilot.chat.agent.thinkingTool` | `true` | Enable agent thinking tool |
| `github.copilot.chat.search.keywordSuggestions` | `true` | Enable keyword suggestions in chat search |
| `github.copilot.chat.alternateGptPrompt.enabled` | `true` | Enable alternate GPT prompting |
| `github.copilot.chat.newWorkspace.useContext7` | `true` | Use Context7 for new workspaces |
| `github.copilot.chat.cli.customAgents.enabled` | `true` | Enable custom CLI agents |
| `github.copilot.chat.cli.mcp.enabled` | `true` | Enable MCP in CLI |
| `github.copilot.chat.githubMcpServer.enabled` | `true` | Enable GitHub MCP server |
| `chat.agent.maxRequests` | `99999` | Virtually unlimited agent requests |
| `chat.agent.showThinking` | `true` | Show agent thinking process |
| `chat.useAgentSkills` | `true` | Enable agent skills |
| `chat.useNestedAgentsMdFiles` | `true` | Support nested AGENTS.md files |

### Commit Message Generation

Copilot generates commit messages with Japanese prefixes:

```json
"github.copilot.chat.commitMessageGeneration.instructions": [
    { "text": "Begin your message with 追加, 修正, 削除, etc." }
]
```

### Instructions Files

```json
"chat.instructionsFilesLocations": {
    ".github/instructions": true
}
```

---

## Git Settings

| Setting | Value | Description |
|---------|-------|-------------|
| `git.autofetch` | `true` | Automatically fetch from remotes |

---

## Terminal Settings

| Setting | Value | Description |
|---------|-------|-------------|
| `terminal.integrated.fontFamily` | `"Moralerspace Radon HWNF"` | Custom programming font with ligatures |
| `terminal.integrated.fontLigatures` | `true` | Enable font ligatures in terminal |

---

## Agent & MCP Configuration

### Chat Agent Settings

| Setting | Value | Description |
|---------|-------|-------------|
| `chat.agent.maxRequests` | `99999` | Virtually unlimited agent requests per session |
| `chat.agent.showThinking` | `true` | Show agent's thinking/reasoning process |
| `chat.agentSessionsViewLocation` | `"showChatsMenu"` | Agent sessions accessible from chats menu |
| `chat.useAgentSkills` | `true` | Enable agent skills (tools) |
| `chat.tools.terminal.outputLocation` | `"chat"` | Terminal tool output shown in chat |
| `inlineChat.notebookAgent` | `true` | Enable agent in notebook inline chat |

### GitHub Pull Requests Agent

| Setting | Value | Description |
|---------|-------|-------------|
| `githubPullRequests.experimental.useQuickChat` | `true` | Quick chat for PR reviews |
| `githubPullRequests.experimental.chat` | `true` | Chat integration for PRs |
| `githubPullRequests.showPullRequestNumberInTree` | `true` | Show PR number in tree view |
| `githubPullRequests.quickDiff` | `true` | Quick diff for PR files |
| `githubPullRequests.experimental.notificationsView` | `true` | PR notifications panel |
| `githubPullRequests.codingAgent.uiIntegration` | `true` | Copilot coding agent for PRs |

### Roo Cline Configuration

```json
"roo-cline.allowedCommands": [
    "npm test", "npm install", "tsc",
    "git log", "git diff", "git show"
]
```

### Zen Mode

| Setting | Value | Description |
|---------|-------|-------------|
| `zenMode.centerLayout` | `false` | Don't center layout in zen mode |
| `zenMode.hideLineNumbers` | `false` | Keep line numbers in zen mode |
| `zenMode.hideStatusBar` | `false` | Keep status bar in zen mode |

---

## Profile Info

VS Code supports multiple profiles to switch between different configurations, extensions, and settings.

| Item | Value |
|------|-------|
| **Active profile** | Default |
| **Settings file** | `/mnt/c/Users/syanagihara/AppData/Roaming/Code - Insiders/User/settings.json` |
| **Extensions dir** | `~/.vscode-insiders/extensions/` |
| **Stable settings** | `~/.config/Code/User/settings.json` |
| **Stable extensions** | `~/.vscode/extensions/` |

### Listing installed extensions

```shell
code-insiders --list-extensions
```

### Installing an extension from the command line

```shell
code-insiders --install-extension <publisher>.<extension-id>
```

### Exporting current settings

```shell
cat "/mnt/c/Users/syanagihara/AppData/Roaming/Code - Insiders/User/settings.json"
```

---

## Fish Shell Integration

The Fish shell is configured to launch VS Code Insiders by default via an abbreviation.

| Item | Value |
|------|-------|
| **Abbreviation** | `code` → `code-insiders` |
| **Defined in** | `~/.config/fish/config.fish` (`set_abbr` function) |

### Usage

```shell
code .                  # Opens current directory in VS Code Insiders
code myfile.py          # Opens a specific file
code --diff a.txt b.txt # Diff two files
code --goto file:10:5   # Open file at line 10, column 5
```

Because `code` is a Fish abbreviation (not an alias), it expands inline when pressed, showing the full `code-insiders` command before execution.
