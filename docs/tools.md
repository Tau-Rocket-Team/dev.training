# Ferramentas Recomendadas e Configuração

Este documento lista todas as ferramentas utilizadas pelo time de desenvolvimento, por que são usadas e como instalá-las e configurá-las.

---

## Índice

1. [Ferramentas Essenciais](#1-ferramentas-essenciais)
2. [Editor — VS Code](#2-editor--vs-code)
3. [Controle de Versão — Git](#3-controle-de-versão--git)
4. [Ambiente JavaScript/Node.js](#4-ambiente-javascriptnodejs)
5. [Conteinerização — Docker](#5-conteinerização--docker)
6. [Comunicação](#6-comunicação)

---

## 1. Ferramentas Essenciais

| Ferramenta | Versão | Instalação |
|---|---|---|
| Git | 2.50+ | [git-scm.com](https://git-scm.com/downloads) |
| VS Code | Última estável | [code.visualstudio.com](https://code.visualstudio.com/) |
| Node.js | 24.14.0 / LTS mais recente | [node.js](https://nodejs.org/pt-br/download) |
| Docker Desktop | 24.0+ | [docker.com](https://www.docker.com/products/docker-desktop/) |

---

## 2. Editor — VS Code

O VS Code é o editor principal do time. Usar um editor consistente torna a programação em par e o compartilhamento de tela muito mais fáceis.

### Instalação

Baixe em [code.visualstudio.com](https://code.visualstudio.com/) e siga o instalador para o seu SO.

### Extensões Obrigatórias

Instale pelo painel de Extensões (`Ctrl+Shift+X` / `Cmd+Shift+X`) ou via CLI:

```bash
# Core & Utilitários
code --install-extension formulahendry.code-runner
code --install-extension ritwickdey.LiveServer
code --install-extension WakaTime.vscode-wakatime
```

```bash
# Inteligência & Git
code --install-extension GitHub.copilot
code --install-extension eamodio.gitlens
code --install-extension yzhang.markdown-all-in-one
```

### Extensões Sugeridas (Produtividade & Estética)

  * **Prettier - Code formatter:** Padronização visual do código.
  * **Dracula Theme Official:** O tema padrão do time.
  * **Material Icon Theme:** Melhora a visualização de arquivos na árvore.
  * **Auto Close Tag:** Facilita escrita de HTML/React.
  * **Bookmarks:** Navegação rápida entre linhas importantes.
  * **CodeSnap:** Para gerar prints bonitos do código.
  * **GitHub Pull Requests:** Gerencie PRs sem sair do editor.

### Configurações Recomendadas

Adicione o seguinte ao `settings.json` do VS Code (`Ctrl+,` → abrir JSON):

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": "active",
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  
  // --- Configuração de AutoSave ---
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000, // Salva automaticamente após 1 segundo de inatividade

  "workbench.colorTheme": "Dracula",
  "workbench.iconTheme": "material-icon-theme",
  "liveServer.settings.donotShowInfoMsg": true,
  "[javascript]": {
    "editor.maxTokenizationLineLength": 2500,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

---

## 3. Controle de Versão — Git

### Instalação

- **macOS**: `brew install git` ou via Xcode Command Line Tools (`xcode-select --install`) ou [git-scm.com](https://git-scm.com/install/mac)
- **Ubuntu/Debian**: `sudo apt update && sudo apt install git` ou [git-scm.com](https://git-scm.com/install/linux)
- **Windows**: Baixe o instalador em [git-scm.com](https://git-scm.com/install/windows) (inclui o Git Bash)

### Configuração Inicial

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
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

## 4. Ambiente Javascript (Node.js)

### 1. Instalação do Node.js (O Motor)

O Node.js permite que o JavaScript seja executado fora do navegador (no seu terminal).

1.  Acesse o site oficial: [nodejs.org](https://nodejs.org/).
2.  Escolha a versão **LTS (Long Term Support)**. Ela é a mais segura e estável para estudos e produção.
3.  Execute o instalador e avance com as opções padrão.
4.  **Verificação:** Abra seu terminal (PowerShell ou CMD) e digite:
    * `node -v` (deve retornar algo como `v20.x.x` ou superior)
    * `npm -v` (o gerenciador de pacotes que vem junto)

---

### 2. Uso do Code Runner

Com o Code Runner instalado, ele é excelente para testes rápidos sem precisar digitar comandos no terminal toda hora.

1.  Crie um novo arquivo chamado `hello.js`.
2.  Escreva um código simples:
    ```javascript
    console.log("Hello, World!");
    ```
3.  **Para rodar:** * Clique no botão de **Play** (canto superior direito).
    * Ou use o atalho: `Ctrl+Alt+N`.
    * O resultado aparecerá na aba "Output" (Saída) logo abaixo.

---

### Qualidade de Código (Lint & Format)

Usamos **ESLint** e **Prettier**. Se o projeto tiver um arquivo `.prettierrc`, o VS Code o seguirá automaticamente.

---

## 5. Conteinerização — Docker

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
| GitHub Projects | Planejamento de sprint e backlog | Incluído no GitHub |
| E-mail / Chat do time | Coordenação em tempo real | Pergunte ao seu mentor |

---
