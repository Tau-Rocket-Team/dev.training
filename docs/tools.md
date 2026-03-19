# Ferramentas Recomendadas e Configuração

Este documento lista todas as ferramentas utilizadas pelo time de desenvolvimento, por que são usadas e como instalá-las e configurá-las.

---

## Índice

1. [Ferramentas Essenciais](#1-ferramentas-essenciais)
2. [Editor — VS Code](#2-editor--vs-code)
3. [Controle de Versão — Git](#3-controle-de-versão--git)
4. [Ambiente Python](#4-ambiente-python)
5. [Toolchain C / C++](#5-toolchain-c--c)
6. [Conteinerização — Docker](#6-conteinerização--docker)
7. [Comunicação](#7-comunicação)
8. [Opcional, mas Recomendado](#8-opcional-mas-recomendado)

---

## 1. Ferramentas Essenciais

| Ferramenta | Versão | Instalação |
|---|---|---|
| Git | 2.40+ | [git-scm.com](https://git-scm.com/downloads) |
| VS Code | Última estável | [code.visualstudio.com](https://code.visualstudio.com/) |
| Python | 3.11+ | [python.org](https://www.python.org/downloads/) |
| Docker Desktop | 24.0+ | [docker.com](https://www.docker.com/products/docker-desktop/) |

---

## 2. Editor — VS Code

O VS Code é o editor principal do time. Usar um editor consistente torna a programação em par e o compartilhamento de tela muito mais fáceis.

### Instalação

Baixe em [code.visualstudio.com](https://code.visualstudio.com/) e siga o instalador para o seu SO.

### Extensões Obrigatórias

Instale pelo painel de Extensões (`Ctrl+Shift+X` / `Cmd+Shift+X`) ou via CLI:

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

### Configurações Recomendadas

Adicione o seguinte ao `settings.json` do VS Code (`Ctrl+,` → abrir JSON):

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

## 3. Controle de Versão — Git

### Instalação

- **macOS**: `brew install git` ou via Xcode Command Line Tools (`xcode-select --install`)
- **Ubuntu/Debian**: `sudo apt update && sudo apt install git`
- **Windows**: Baixe o instalador em [git-scm.com](https://git-scm.com/downloads) (inclui o Git Bash)

### Configuração Inicial

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

### Configuração de Chave SSH

```bash
# Gere uma nova chave Ed25519
ssh-keygen -t ed25519 -C "your.email@example.com"

# Inicie o agente e adicione a chave
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Exiba a chave pública — copie e cole em GitHub Settings → SSH Keys
cat ~/.ssh/id_ed25519.pub

# Verifique a conexão
ssh -T git@github.com
```

---

## 4. Ambiente Python

Usamos **ambientes virtuais** para isolar dependências de projeto. Nunca instale pacotes de projeto no ambiente Python global.

### Instalação

- **macOS**: `brew install python@3.11`
- **Ubuntu/Debian**: `sudo apt install python3.11 python3.11-venv python3-pip`
- **Windows**: Baixe em [python.org](https://www.python.org/downloads/)

### Criando um Ambiente Virtual

```bash
# Dentro do diretório do projeto
python3 -m venv .venv

# Ativar (macOS / Linux)
source .venv/bin/activate

# Ativar (Windows)
.venv\Scripts\activate

# Instalar dependências do projeto
pip install -r requirements.txt

# Desativar ao terminar
deactivate
```

### Lint e Formatação

Usamos **Ruff** tanto para lint quanto para formatação (substitui flake8, isort e black):

```bash
pip install ruff

# Verificar problemas
ruff check .

# Corrigir problemas automaticamente
ruff check --fix .

# Formatar código
ruff format .
```

### Testes

```bash
pip install pytest pytest-cov

# Executar todos os testes
pytest

# Executar com relatório de cobertura
pytest --cov=src --cov-report=term-missing
```

---

## 5. Toolchain C / C++

Usado principalmente para `dev.avionics` (firmware do computador de voo).

### Instalação

**macOS:**
```bash
# Instalar LLVM/Clang
brew install llvm cmake ninja

# Instalar cross-compilador ARM (para alvos embarcados)
brew install --cask gcc-arm-embedded
```

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install build-essential cmake ninja-build clang clang-format clang-tidy
# Cross-compilador ARM
sudo apt install gcc-arm-none-eabi
```

### Sistema de Build — CMake

```bash
# Configurar e compilar
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Debug
cmake --build build

# Executar testes
ctest --test-dir build --output-on-failure
```

### Formatação de Código — clang-format

Um arquivo `.clang-format` na raiz de cada projeto define o estilo. Execute antes de fazer commit:

```bash
# Formatar todos os arquivos C/C++
find src -name "*.cpp" -o -name "*.hpp" -o -name "*.c" -o -name "*.h" \
  | xargs clang-format -i
```

---

## 6. Conteinerização — Docker

O Docker é usado para fornecer ambientes de desenvolvimento e build reproduzíveis.

### Instalação

Baixe o **Docker Desktop** em [docker.com](https://www.docker.com/products/docker-desktop/) e siga o instalador.

Verifique a instalação:
```bash
docker --version
docker compose version
```

### Comandos Comuns

```bash
# Compilar uma imagem
docker build -t tau/groundstation:dev .

# Executar um contêiner interativamente
docker run -it --rm tau/groundstation:dev bash

# Iniciar serviços definidos em docker-compose.yml
docker compose up -d

# Parar serviços
docker compose down

# Ver logs
docker compose logs -f
```

---

## 7. Comunicação

| Ferramenta | Finalidade | Acesso |
|---|---|---|
| GitHub Issues | Relatórios de bugs, solicitações de funcionalidades, acompanhamento de tarefas | Incluído no GitHub |
| GitHub Discussions | Perguntas do time, anúncios | Incluído no GitHub |
| GitHub Projects | Planejamento de sprint e backlog | Incluído no GitHub |
| E-mail / Chat do time | Coordenação em tempo real | Pergunte ao seu mentor |

---

## 8. Opcional, mas Recomendado

| Ferramenta | Finalidade | Instalação |
|---|---|---|
| [GitHub CLI (`gh`)](https://cli.github.com/) | Criar PRs e issues pelo terminal | `brew install gh` / `apt install gh` |
| [pre-commit](https://pre-commit.com/) | Executar linters automaticamente antes de cada commit | `pip install pre-commit && pre-commit install` |
| [htop](https://htop.dev/) | Monitor de processos | `brew install htop` / `apt install htop` |
| [jq](https://stedolan.github.io/jq/) | Processamento de JSON em scripts shell | `brew install jq` / `apt install jq` |
| [bat](https://github.com/sharkdp/bat) | `cat` aprimorado com realce de sintaxe | `brew install bat` / `apt install bat` |

### Configurando o pre-commit

Adicione um `.pre-commit-config.yaml` ao seu projeto:

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

Instale e ative:
```bash
pip install pre-commit
pre-commit install
```
