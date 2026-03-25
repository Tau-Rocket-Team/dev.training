# Diretrizes de Desenvolvimento

Este documento define os padrões que todo desenvolvedor do time deve seguir. Práticas consistentes reduzem o atrito, tornam as revisões de código mais rápidas e mantêm a base de código sustentável à medida que o time cresce.

---

## Índice

- [Diretrizes de Desenvolvimento](#diretrizes-de-desenvolvimento)
  - [Índice](#índice)
  - [1. Fluxo de Trabalho Git](#1-fluxo-de-trabalho-git)
  - [2. Estratégia de Branches](#2-estratégia-de-branches)
  - [3. Mensagens de Commit](#3-mensagens-de-commit)
    - [Formato](#formato)
    - [Regras](#regras)
    - [Exemplos](#exemplos)
  - [4. Pull Requests](#4-pull-requests)
  - [5. Etiqueta de Code Review](#5-etiqueta-de-code-review)
  - [6. Convenções de Nomenclatura](#6-convenções-de-nomenclatura)
    - [Arquivos e Diretórios](#arquivos-e-diretórios)
    - [Variáveis e Funções (Python)](#variáveis-e-funções-python)
    - [Branches e Tags](#branches-e-tags)
  - [7. Padrões de Documentação (JSDoc)](#7-padrões-de-documentação-jsdoc)
    - [Por que usar JSDoc em vez de comentários simples?](#por-que-usar-jsdoc-em-vez-de-comentários-simples)
  - [8. Princípios de Código Limpo](#8-princípios-de-código-limpo)
    - [Exemplo: Antes vs. Depois](#exemplo-antes-vs-depois)
  - [9. Estrutura do Projeto](#9-estrutura-do-projeto)

---

## 1. Fluxo de Trabalho Git

Seguimos o seguinte **GitHub Flow**:

```
main  ←── dev ←── branch de funcionalidade (PR + revisão) ←── seu branch local
```

**Regras:**
- `main` é sempre implantável / estável.
- **Nunca** faça force-push no `main`.
- `dev` é um ambiente de integração / deve se manter estável.
- Toda mudança deve ser feita em seu próprio branch e integrada via pull request.
- Exclua seu branch após o merge do PR.

---

## 2. Estratégia de Branches

Os nomes de branches seguem o padrão `<type>/<short-description>`:

| Tipo | Quando usar | Exemplo |
|---|---|---|
| `feat` | Nova funcionalidade ou capacidade | `feat/add-gps-parser` |
| `fix` | Correção de bug | `fix/sensor-overflow-bug` |
| `docs` | Apenas documentação | `docs/update-onboarding` |
| `refactor` | Reestruturação de código, sem mudança de comportamento | `refactor/extract-telemetry-utils` |
| `test` | Adicionando ou corrigindo testes | `test/gps-unit-tests` |
| `chore` | Scripts de build, CI, dependências | `chore/update-dependencies` |

**Regras:**
- Use apenas **letras minúsculas** e **hífens** — sem espaços ou underscores.
- Mantenha a descrição curta (2–4 palavras).
- Crie branches a partir do `dev` mais recente: `git checkout -b feat/my-feature origin/dev`.

---

## 3. Mensagens de Commit

Seguimos a especificação [Conventional Commits](https://www.conventionalcommits.org/).

### Formato

```
<type>(<scope>): <subject>

[corpo opcional]

[rodapé opcional]
```

### Regras

- **Linha de assunto**: modo imperativo, ≤72 caracteres, sem ponto no final.
- **Corpo**: explique *o quê* e *por quê*, não *como*. Quebre em 72 caracteres.
- **Rodapé**: referencie issues (`Closes #42`, `Refs #7`).

### Exemplos

```
feat(telemetry): add CRC validation for incoming packets

Packets without a valid CRC were being silently dropped. Now they are
logged at WARN level and counted in the health metrics.

Closes #34
```

```
fix(parser): handle empty GPS frame gracefully

An empty frame caused an index-out-of-bounds panic. Added an early
return with an appropriate error log.

Refs #51
```

```
docs(onboarding): add SSH setup instructions
```

Obs.: "Closes #34" fecha a issue automaticamente quando o PR for mergeado, enquanto "Refs #51" apenas referencia a issue sem fechá-la.

---

## 4. Pull Requests

Use o [`templates/pull_request_template.md`](../templates/pull_request_template.md) ao abrir um PR.

**Requisitos:**
- Vincule a issue relacionada (`Closes #<número>`).
- Forneça uma descrição clara do *que* mudou e *por quê*.
- Inclua capturas de tela ou saída de testes quando aplicável.
- Todas as verificações de CI devem passar antes de solicitar revisão.
- Pelo menos **uma aprovação** de um membro do time é necessária antes do merge.
- Prefira **squash and merge** para manter o histórico do `main` limpo.

**Tamanho do PR:**
- Mantenha os PRs pequenos e focados. Uma boa regra: um revisor deve conseguir entender completamente a mudança em 20 minutos.
- Se um PR está ficando grande, divida-o em PRs menores e sequenciais.

---

## 5. Etiqueta de Code Review

**Como revisor:**
- Revise dentro de 48 horas após ser atribuído.
- Seja específico: aponte a linha exata e explique a preocupação.
- Distinga entre bloqueadores (`BLOCKING:`) e sugestões (`NIT:` / `SUGGESTION:`).
- Aprove quando estiver satisfeito — não deixe PRs abertos indefinidamente.

**Como autor:**
- Responda a cada comentário, mesmo que apenas para reconhecê-lo.
- Não leve o feedback para o lado pessoal — é sobre o código, não sobre você.
- Se discordar de um comentário, explique seu raciocínio claramente.

---

## 6. Convenções de Nomenclatura

### Arquivos e Diretórios
- Arquivos e Pastas: Use kebab-case para nomes de arquivos e diretórios. É o padrão absoluto no ecossistema Node.js (ex: auth-middleware.js, user-controller.ts).
- Use `kebab-case` para arquivos Markdown e de configuração: `onboarding.md`, `docker-compose.yml`
- Use `PascalCase` para arquivos de classe onde a convenção da linguagem exige (ex.: C++): `TelemetryFrame.hpp`

### Variáveis e Funções (Python)
```javascript
// Constantes Globais/Configurações: UPPER_SNAKE_CASE
const MAX_PACKET_SIZE = 1024;
const DEFAULT_PORT = 3000;

// Enums (ou Objetos de configuração congelados): PascalCase nas chaves ou UPPER_SNAKE_CASE
const Status = {
    ACTIVE: 'active',
    INACTIVE: 'inactive'
};
```

### Branches e Tags
- Branches: `feat/short-description` (veja §2)
- Tags de release: `v<major>.<minor>.<patch>` (ex.: `v1.2.0`)

---

## 7. Padrões de Documentação (JSDoc)

* Toda função exportada, classe e middleware deve ter um bloco de comentário **JSDoc**.
* Inicie o bloco sempre com `/**` (dois asteriscos) para que o editor de código reconheça como documentação oficial.
* Mantenha a documentação sincronizada — atualize o JSDoc no mesmo PR da mudança lógica.
* Use as tags `@param`, `@returns` e `@throws` para definir a estrutura da função.

**Exemplo de documentação JSDoc (JavaScript):**

```javascript
/**
 * Converte uma sentença NMEA 0183 bruta em um objeto estruturado.
 * * @param {string} sentence - Sentença NMEA bruta, ex: "$GPGGA,123519,4807.038,N,...*47"
 * @returns {Object} Objeto contendo as chaves: type, fields e checksumValid.
 * * @throws {Error} Se a sentença não começar com o caractere '$'.
 */
function parseNmeaSentence(sentence) {
  if (!sentence.startsWith('$')) {
    throw new Error("Sentença NMEA inválida: deve começar com '$'");
  }
  // ... lógica de processamento
}
```

Os parametros e o valor de retorno devem ser claramente descritos, incluindo seus tipos. Isso melhora a legibilidade e permite que ferramentas de análise estática forneçam melhores insights sobre o código.

### Por que usar JSDoc em vez de comentários simples?

1.  **Hover de Informação:** No VS Code, ao passar o mouse sobre `parseNmeaSentence` em qualquer outro arquivo, você verá exatamente o que ela espera e o que ela retorna.
2.  **Facilita o TypeScript:** Se um dia você migrar o **PGI-PROA** para TypeScript, o compilador consegue aproveitar grande parte das informações do seu JSDoc.
3.  **Geração Automática de Docs:** Ferramentas como o *Swagger* (para APIs) ou *JSDoc Generator* podem ler esses comentários e criar uma página web de documentação para a sua equipe automaticamente.

---

## 8. Princípios de Código Limpo

1. **Legível em vez de inteligente** — o código é lido muito mais do que escrito.
2. **Responsabilidade única** — cada função e classe faz uma coisa bem.
3. **Nomes significativos** — um bom nome é melhor que um comentário.
4. **Mantenha funções pequenas** — se uma função precisa de rolagem, provavelmente faz demais.
5. **Sem números mágicos** — substitua literais por constantes nomeadas.
6. **Não se repita (DRY)** — extraia a lógica repetida em funções reutilizáveis.
7. **Falhe alto** — lance exceções / erros em vez de ignorar falhas silenciosamente.
8. **Deixe o código mais limpo do que encontrou** — corrija pequenos problemas que encontrar, mesmo que não estejam na sua tarefa.

### Exemplo: Antes vs. Depois

```javascript
// ❌ Difícil de entender e manter
function p(d, t) {
  return d * 1000 / t;
}

// ✅ Claro, sustentável e com tipagem via JSDoc
const METERS_PER_KILOMETER = 1000;
const SECONDS_PER_HOUR = 3600;

/**
 * Calcula a velocidade em km/h.
 * @param {number} distanceMeters - Distância percorrida em metros.
 * @param {number} timeSeconds - Tempo gasto em segundos.
 * @returns {number} Velocidade resultante em km/h.
 */
function calculateSpeedKph(distanceMeters, timeSeconds) {
  const distanceKm = distanceMeters / METERS_PER_KILOMETER;
  const timeHours = timeSeconds / SECONDS_PER_HOUR;
  
  return distanceKm / timeHours;
}
```

---

## 9. Estrutura do Projeto

Todo repositório `dev.*` deve seguir este layout:

```
dev.<projectName>/
├── src/                # Código-fonte (Lógica de negócio)
│   ├── app.js          # Ponto de entrada da aplicação (ou index.js)
│   ├── controllers/    # Lógica de rotas e requisições
│   ├── models/         # Definição de dados/esquemas
│   ├── middlewares/    # Interceptadores (auth, logs, etc)
│   └── utils/          # Funções utilitárias e helpers
├── tests/              # Testes unitários (Jest/Vitest) e integração
├── docs/               # Documentação específica (Diagramas, Swagger)
├── scripts/            # Scripts de automação e migração de banco
├── .github/
│   └── workflows/      # Pipelines de CI/CD (GitHub Actions)
├── .gitignore          # Essencial: sempre incluir node_modules/ e .env
├── package.json        # Manifesto do projeto e dependências
├── package-lock.json   # Trava de versões das dependências
└── README.md           # Visão geral (use templates/project_template.md)
```

---
