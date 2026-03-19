# Guia de Integração

Bem-vindo ao time de desenvolvimento da Tau Rocket Team! Este guia orienta você em tudo o que precisa fazer antes de escrever sua primeira linha de código.

---

## Índice

1. [Pré-requisitos](#1-pré-requisitos)
2. [Solicitar Acesso](#2-solicitar-acesso)
3. [Configurar Seu Ambiente de Desenvolvimento](#3-configurar-seu-ambiente-de-desenvolvimento)
4. [Clonar os Repositórios Necessários](#4-clonar-os-repositórios-necessários)
5. [Entender o Fluxo de Trabalho do Time](#5-entender-o-fluxo-de-trabalho-do-time)
6. [Completar Sua Primeira Atividade](#6-completar-sua-primeira-atividade)
7. [Apresentar-se](#7-apresentar-se)
8. [Checklist de Integração](#8-checklist-de-integração)

---

## 1. Pré-requisitos

Antes do seu primeiro dia, certifique-se de ter:

- Uma conta no GitHub (envie seu nome de usuário ao seu mentor para ser adicionado à organização).
- Familiaridade básica com a linha de comando / terminal.
- Um computador rodando macOS, Linux ou Windows com WSL2.

Se faltar algum dos itens acima, peça ajuda ao seu mentor antes de prosseguir.

---

## 2. Solicitar Acesso

1. Envie seu nome de usuário do GitHub ao seu mentor ou ao líder do time.
2. Aceite o convite para ingressar na organização **Tau-Rocket-Team** no GitHub.
3. Confirme que você consegue ver os repositórios privados do time em `https://github.com/Tau-Rocket-Team`.

---

## 3. Configurar Seu Ambiente de Desenvolvimento

Siga [`docs/tools.md`](tools.md) para uma lista completa das ferramentas necessárias e instruções de instalação passo a passo. No mínimo você precisará de:

| Ferramenta | Versão Mínima | Finalidade |
|---|---|---|
| Git | 2.40+ | Controle de versão |
| VS Code | Última versão estável | Editor principal |
| Python | 3.11+ | Scripts e ferramentas |
| Docker | 24.0+ | Desenvolvimento em contêiner |

### Configurar o Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
```

### Configurar Autenticação SSH

Usar SSH em vez de HTTPS é **fortemente recomendado**:

```bash
# Gere uma nova chave SSH
ssh-keygen -t ed25519 -C "your.email@example.com"

# Adicione sua chave ao agente SSH
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copie sua chave pública e adicione-a ao GitHub
cat ~/.ssh/id_ed25519.pub
# → Cole em https://github.com/settings/keys
```

---

## 4. Clonar os Repositórios Necessários

Sempre clone via SSH:

```bash
# Clone este repositório de treinamento
git clone git@github.com:Tau-Rocket-Team/dev.training.git
cd dev.training

# Clone o repositório do projeto atribuído a você (exemplo)
git clone git@github.com:Tau-Rocket-Team/dev.avionics.git
```

---

## 5. Entender o Fluxo de Trabalho do Time

Leia [`docs/guidelines.md`](guidelines.md) na íntegra antes de escrever qualquer código. Pontos principais:

- **Crie branches a partir do `main`** — nunca faça commit diretamente no `main`.
- **Convenção de nomenclatura** — branches seguem `type/short-description` (ex.: `feat/sensor-calibration`, `fix/telemetry-overflow`).
- **Mensagens de commit** — siga o formato [Conventional Commits](https://www.conventionalcommits.org/).
- **Pull requests** — toda mudança passa por um PR e requer pelo menos uma aprovação de revisor antes do merge.
- **Code review** — seja respeitoso, específico e construtivo.

---

## 6. Completar Sua Primeira Atividade

1. Navegue até [`activities/challenges/`](../activities/challenges/).
2. Comece com `beginner_challenge_01.md`.
3. Crie um branch: `git checkout -b feat/onboarding-<seu-github-username>`.
4. Complete o desafio e coloque sua solução em [`activities/submissions/`](../activities/submissions/) seguindo a convenção de nomenclatura descrita lá.
5. Abra um pull request contra `main` usando o template [`templates/pull_request_template.md`](../templates/pull_request_template.md).
6. Solicite seu mentor como revisor.

---

## 7. Apresentar-se

Abra uma GitHub Issue com o título **"👋 Apresentação — \<Seu Nome\>"** usando o template de issue e conte ao time:

- Seu histórico e por que ingressou no time.
- Em qual sub-time você está (aviônica, software, simulação, etc.).
- No que você está mais animado para trabalhar.

---

## 8. Checklist de Integração

Use este checklist para acompanhar seu progresso de integração. Copie-o para um comentário na sua issue de apresentação.

- [ ] Conta do GitHub criada e acesso concedido
- [ ] Git configurado com nome, e-mail e chave SSH
- [ ] Ferramentas necessárias instaladas (veja `docs/tools.md`)
- [ ] Repositório `dev.training` clonado
- [ ] `docs/guidelines.md` lido na íntegra
- [ ] `docs/architecture.md` lido na íntegra
- [ ] Primeiro desafio concluído e PR enviado
- [ ] Issue de apresentação aberta
- [ ] Primeiro encontro individual com mentor realizado

---

> **Precisa de ajuda?** Abra uma GitHub Issue ou fale diretamente com seu mentor. Nenhuma pergunta é pequena demais.
