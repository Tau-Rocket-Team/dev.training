# Contribuindo com os Repositórios da Tau Rocket Team

Obrigado por contribuir! Este guia explica como contribuir com qualquer repositório `dev.*` na organização Tau Rocket Team.

---

## Índice

1. [Código de Conduta](#1-código-de-conduta)
2. [Antes de Começar](#2-antes-de-começar)
3. [Como Reportar um Bug](#3-como-reportar-um-bug)
4. [Como Solicitar uma Funcionalidade](#4-como-solicitar-uma-funcionalidade)
5. [Como Enviar um Pull Request](#5-como-enviar-um-pull-request)
6. [Fluxo de Desenvolvimento](#6-fluxo-de-desenvolvimento)
7. [Diretrizes de Mensagem de Commit](#7-diretrizes-de-mensagem-de-commit)
8. [Estilo de Código](#8-estilo-de-código)
9. [Obtendo Ajuda](#9-obtendo-ajuda)

---

## 1. Código de Conduta

Estamos comprometidos com um ambiente acolhedor, respeitoso e inclusivo.

- Seja gentil e respeitoso em todas as interações.
- Critique o código, nunca a pessoa.
- Aceite feedback com graciosidade — estamos todos aqui para aprender.
- Faça perguntas livremente; nenhuma pergunta é básica demais.

Violações devem ser reportadas ao líder do time.

---

## 2. Antes de Começar

1. **Complete a integração** — Leia [`docs/onboarding.md`](docs/onboarding.md) se ainda não o fez.
2. **Leia as diretrizes** — Entenda [`docs/guidelines.md`](docs/guidelines.md) antes de escrever qualquer código.
3. **Verifique as issues existentes** — Sua ideia pode já estar registrada. Pesquise antes de abrir uma duplicata.
4. **Discuta grandes mudanças primeiro** — Para funcionalidades significativas ou mudanças de arquitetura, abra uma issue para discussão antes de escrever código. Isso economiza o tempo de todos.

---

## 3. Como Reportar um Bug

1. Pesquise nas [issues existentes](../../issues) para verificar se já foi reportado.
2. Se não, abra uma nova issue usando o [template de issue](templates/issue_template.md).
3. Inclua:
   - Um título claro descrevendo o bug.
   - Passos para reproduzir.
   - O que você esperava vs. o que aconteceu.
   - Seu ambiente (SO, versão da linguagem, branch).
   - Quaisquer logs ou saídas de erro relevantes.

---

## 4. Como Solicitar uma Funcionalidade

1. Abra uma issue usando o [template de issue](templates/issue_template.md) e selecione "Solicitação de funcionalidade".
2. Descreva o problema que sua funcionalidade resolve.
3. Explique sua solução proposta.
4. Aguarde a discussão e a aprovação de um membro do time antes de implementar.

---

## 5. Como Enviar um Pull Request

### Referência Rápida

```bash
# 1. Atualize seu main local
git checkout main
git pull origin main

# 2. Crie um branch de funcionalidade
git checkout -b feat/my-feature

# 3. Faça suas alterações
# ... edite os arquivos ...

# 4. Adicione ao stage e faça commit
git add .
git commit -m "feat(scope): describe the change"

# 5. Faça push e abra o PR
git push origin feat/my-feature
# → Abra o PR no GitHub e use o pull_request_template.md
```

### Requisitos do Pull Request

- Vincule a issue que seu PR resolve (`Closes #42`).
- Preencha o template de PR completamente.
- Todas as verificações de CI devem estar verdes antes de solicitar revisão.
- Pelo menos **uma aprovação** de um membro do time antes de fazer merge.
- Mantenha os PRs pequenos e focados — um objetivo por PR.

---

## 6. Fluxo de Desenvolvimento

Seguimos o **GitHub Flow**:

```
main  ←── PR (revisado + CI verde) ←── branch de funcionalidade
```

1. **Nunca faça commit diretamente no `main`.**
2. Sempre crie branches a partir do `main` mais recente.
3. Exclua seu branch após o merge do PR.

### Nomenclatura de Branches

```
feat/short-description       # Novas funcionalidades
fix/short-description        # Correções de bugs
docs/short-description       # Documentação
refactor/short-description   # Refatoração
test/short-description       # Testes
chore/short-description      # Build/CI/deps
```

---

## 7. Diretrizes de Mensagem de Commit

Seguimos o [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

[corpo opcional — o quê e por quê, não como]

[rodapé opcional — Closes #42]
```

**Exemplos:**

```
feat(parser): add support for GPS NMEA sentences
fix(telemetry): prevent crash on empty packet
docs(readme): add usage examples
test(parser): add edge case tests for CRC validation
```

---

## 8. Estilo de Código

- **Python**: [Ruff](https://docs.astral.sh/ruff/) para linting e formatação. Execute `ruff check .` e `ruff format .` antes de fazer commit.
- **C/C++**: [clang-format](https://clang.llvm.org/docs/ClangFormat.html) com o arquivo `.clang-format` do projeto. Execute antes de fazer commit.
- **Markdown**: Cabeçalhos, blocos de código e tabelas consistentes. Quebre linhas longas.

Todos os projetos incluem uma etapa de linting no CI que falhará o build se problemas de estilo forem encontrados.

---

## 9. Obtendo Ajuda

- **GitHub Issues** — Para bugs e solicitações de funcionalidades.
- **GitHub Discussions** — Para perguntas e conversa geral do time.
- **Mensagem direta** — Entre em contato com seu mentor ou um desenvolvedor sênior.
- **Comentários de PR** — Marque um revisor se precisar de orientação durante o PR.

Queremos que você tenha sucesso — não hesite em pedir ajuda!
