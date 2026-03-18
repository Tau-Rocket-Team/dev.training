# Recommended Tools and Setup

This document lists every tool used by the development team, why it is used, and how to install and configure it.

---

## Table of Contents

1. [Core Tools](#1-core-tools)
2. [Editor — VS Code](#2-editor--vs-code)
3. [Version Control — Git](#3-version-control--git)
4. [Python Environment](#4-python-environment)
5. [C / C++ Toolchain](#5-c--c-toolchain)
6. [Containerisation — Docker](#6-containerisation--docker)
7. [Communication](#7-communication)
8. [Optional but Recommended](#8-optional-but-recommended)

---

## 1. Core Tools

| Tool | Version | Installation |
|---|---|---|
| Git | 2.40+ | [git-scm.com](https://git-scm.com/downloads) |
| VS Code | Latest stable | [code.visualstudio.com](https://code.visualstudio.com/) |
| Python | 3.11+ | [python.org](https://www.python.org/downloads/) |
| Docker Desktop | 24.0+ | [docker.com](https://www.docker.com/products/docker-desktop/) |

---

## 2. Editor — VS Code

VS Code is the team's primary editor. Using a consistent editor makes pair programming and screen sharing much easier.

### Installation

Download from [code.visualstudio.com](https://code.visualstudio.com/) and follow the installer for your OS.

### Required Extensions

Install these from the Extensions panel (`Ctrl+Shift+X` / `Cmd+Shift+X`) or via the CLI:

```bash
code --install-extension ms-python.python
code --install-extension ms-python.ruff
code --install-extension ms-vscode.cpptools
code --install-extension ms-vscode.cmake-tools
code --install-extension GitHub.copilot
code --install-extension eamodio.gitlens
code --install-extension yzhang.markdown-all-in-one
code --install-extension streetsidesoftware.code-spell-checker
```

### Recommended Settings

Add the following to your VS Code `settings.json` (`Ctrl+,` → open JSON):

```json
{
  "editor.formatOnSave": true,
  "editor.rulers": [88],
  "editor.tabSize": 4,
  "editor.insertSpaces": true,
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff"
  },
  "[markdown]": {
    "editor.wordWrap": "on"
  }
}
```

---

## 3. Version Control — Git

### Installation

- **macOS**: `brew install git` or via Xcode Command Line Tools (`xcode-select --install`)
- **Ubuntu/Debian**: `sudo apt update && sudo apt install git`
- **Windows**: Download the installer from [git-scm.com](https://git-scm.com/downloads) (includes Git Bash)

### Initial Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

### SSH Key Setup

```bash
# Generate a new Ed25519 key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Start the agent and add the key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Print the public key — copy and paste it into GitHub Settings → SSH Keys
cat ~/.ssh/id_ed25519.pub

# Verify the connection
ssh -T git@github.com
```

---

## 4. Python Environment

We use **virtual environments** to isolate project dependencies. Never install project packages into the global Python environment.

### Installation

- **macOS**: `brew install python@3.11`
- **Ubuntu/Debian**: `sudo apt install python3.11 python3.11-venv python3-pip`
- **Windows**: Download from [python.org](https://www.python.org/downloads/)

### Creating a Virtual Environment

```bash
# Inside a project directory
python3 -m venv .venv

# Activate (macOS / Linux)
source .venv/bin/activate

# Activate (Windows)
.venv\Scripts\activate

# Install project dependencies
pip install -r requirements.txt

# Deactivate when done
deactivate
```

### Linting and Formatting

We use **Ruff** for both linting and formatting (it replaces flake8, isort, and black):

```bash
pip install ruff

# Check for issues
ruff check .

# Auto-fix issues
ruff check --fix .

# Format code
ruff format .
```

### Testing

```bash
pip install pytest pytest-cov

# Run all tests
pytest

# Run with coverage report
pytest --cov=src --cov-report=term-missing
```

---

## 5. C / C++ Toolchain

Used primarily for `dev.avionics` (flight computer firmware).

### Installation

**macOS:**
```bash
# Install LLVM/Clang
brew install llvm cmake ninja

# Install ARM cross-compiler (for embedded targets)
brew install --cask gcc-arm-embedded
```

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install build-essential cmake ninja-build clang clang-format clang-tidy
# ARM cross-compiler
sudo apt install gcc-arm-none-eabi
```

### Build System — CMake

```bash
# Configure and build
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Debug
cmake --build build

# Run tests
ctest --test-dir build --output-on-failure
```

### Code Formatting — clang-format

A `.clang-format` file in each project root defines the style. Run before committing:

```bash
# Format all C/C++ files
find src -name "*.cpp" -o -name "*.hpp" -o -name "*.c" -o -name "*.h" \
  | xargs clang-format -i
```

---

## 6. Containerisation — Docker

Docker is used to provide reproducible development and build environments.

### Installation

Download **Docker Desktop** from [docker.com](https://www.docker.com/products/docker-desktop/) and follow the installer.

Verify the installation:
```bash
docker --version
docker compose version
```

### Common Commands

```bash
# Build an image
docker build -t tau/groundstation:dev .

# Run a container interactively
docker run -it --rm tau/groundstation:dev bash

# Start services defined in docker-compose.yml
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f
```

---

## 7. Communication

| Tool | Purpose | Access |
|---|---|---|
| GitHub Issues | Bug reports, feature requests, task tracking | Included in GitHub |
| GitHub Discussions | Team Q&A, announcements | Included in GitHub |
| GitHub Projects | Sprint planning and backlog | Included in GitHub |
| Email / Team chat | Real-time coordination | Ask your mentor |

---

## 8. Optional but Recommended

| Tool | Purpose | Installation |
|---|---|---|
| [GitHub CLI (`gh`)](https://cli.github.com/) | Create PRs and issues from the terminal | `brew install gh` / `apt install gh` |
| [pre-commit](https://pre-commit.com/) | Auto-run linters before every commit | `pip install pre-commit && pre-commit install` |
| [htop](https://htop.dev/) | Process monitor | `brew install htop` / `apt install htop` |
| [jq](https://stedolan.github.io/jq/) | JSON processing in shell scripts | `brew install jq` / `apt install jq` |
| [bat](https://github.com/sharkdp/bat) | Better `cat` with syntax highlighting | `brew install bat` / `apt install bat` |

### Setting Up pre-commit

Add a `.pre-commit-config.yaml` to your project:

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-merge-conflict
```

Install and activate:
```bash
pip install pre-commit
pre-commit install
```
