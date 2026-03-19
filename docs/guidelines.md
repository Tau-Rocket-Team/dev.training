# Diretrizes de Desenvolvimento

Este documento define os padrões que todo desenvolvedor do time deve seguir. Práticas consistentes reduzem o atrito, tornam as revisões de código mais rápidas e mantêm a base de código sustentável à medida que o time cresce.

---

## Índice

1. [Fluxo de Trabalho Git](#1-fluxo-de-trabalho-git)
2. [Estratégia de Branches](#2-estratégia-de-branches)
3. [Mensagens de Commit](#3-mensagens-de-commit)
4. [Pull Requests](#4-pull-requests)
5. [Etiqueta de Code Review](#5-etiqueta-de-code-review)
6. [Convenções de Nomenclatura](#6-convenções-de-nomenclatura)
7. [Princípios de Código Limpo](#7-princípios-de-código-limpo)
8. [Estrutura do Projeto](#8-estrutura-do-projeto)
9. [Padrões de Documentação](#9-padrões-de-documentação)

---

## 1. Fluxo de Trabalho Git

Seguimos um **GitHub Flow** simplificado:

```
main  ←── branch de funcionalidade (PR + revisão) ←── seu branch local
```

**Regras:**
- `main` é sempre implantável / estável.
- **Nunca** faça force-push no `main`.
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
- Crie branches a partir do `main` mais recente: `git checkout -b feat/my-feature origin/main`.

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
- Use `snake_case` para arquivos Python e scripts: `sensor_parser.py`
- Use `kebab-case` para arquivos Markdown e de configuração: `onboarding.md`, `docker-compose.yml`
- Use `PascalCase` para arquivos de classe onde a convenção da linguagem exige (ex.: C++): `TelemetryFrame.hpp`

### Variáveis e Funções (Python)
```python
# Variáveis: snake_case
packet_count = 0
sensor_data = []

# Funções: snake_case, verbo primeiro
def parse_gps_frame(raw_bytes):
    ...

# Classes: PascalCase
class TelemetryParser:
    ...

# Constantes: UPPER_SNAKE_CASE
MAX_PACKET_SIZE = 1024
DEFAULT_BAUD_RATE = 115200
```

### Variáveis e Funções (C / C++)
```cpp
// Variáveis: camelCase ou snake_case (seja consistente por projeto)
uint32_t packetCount = 0;

// Funções: camelCase
void parseTelemetryFrame(uint8_t* buffer, size_t len);

// Classes / Structs: PascalCase
class TelemetryParser { ... };

// Constantes / Macros: UPPER_SNAKE_CASE
#define MAX_PACKET_SIZE 1024
```

### Branches e Tags
- Branches: `feat/short-description` (veja §2)
- Tags de release: `v<major>.<minor>.<patch>` (ex.: `v1.2.0`)

---

## 7. Princípios de Código Limpo

1. **Legível em vez de inteligente** — o código é lido muito mais do que escrito.
2. **Responsabilidade única** — cada função e classe faz uma coisa bem.
3. **Nomes significativos** — um bom nome é melhor que um comentário.
4. **Mantenha funções pequenas** — se uma função precisa de rolagem, provavelmente faz demais.
5. **Sem números mágicos** — substitua literais por constantes nomeadas.
6. **Não se repita (DRY)** — extraia a lógica repetida em funções reutilizáveis.
7. **Falhe alto** — lance exceções / erros em vez de ignorar falhas silenciosamente.
8. **Deixe o código mais limpo do que encontrou** — corrija pequenos problemas que encontrar, mesmo que não estejam na sua tarefa.

### Exemplo: Antes vs. Depois

```python
# ❌ Difícil de entender
def p(d, t):
    return d * 1000 / t

# ✅ Claro e sustentável
METERS_PER_KILOMETER = 1000

def calculate_speed_kph(distance_meters: float, time_seconds: float) -> float:
    """Return speed in km/h given distance in metres and time in seconds."""
    return (distance_meters / METERS_PER_KILOMETER) / (time_seconds / 3600)
```

---

## 8. Estrutura do Projeto

Todo repositório `dev.*` deve seguir este layout:

```
dev.<projectName>/
├── README.md           # Visão geral do projeto (use templates/project_template.md)
├── CONTRIBUTING.md     # Como contribuir com este projeto específico
├── src/                # Código-fonte
│   ├── <module>/
│   └── main.py / main.cpp
├── tests/              # Testes unitários e de integração
├── docs/               # Documentação específica do projeto
├── scripts/            # Scripts de build, deploy e utilitários
├── .github/
│   ├── workflows/      # Pipelines de CI/CD
│   └── PULL_REQUEST_TEMPLATE.md
└── .gitignore
```

---

## 9. Padrões de Documentação

- Toda função pública, classe e módulo deve ter uma docstring / comentário de documentação.
- Mantenha a documentação próxima ao código — atualize os docs no mesmo PR que a mudança de código.
- Use Markdown para toda documentação escrita.
- Use tabelas, blocos de código e cabeçalhos para melhorar a legibilidade.

**Exemplo de docstring Python:**
```python
def parse_nmea_sentence(sentence: str) -> dict:
    """
    Parse a raw NMEA 0183 sentence into a structured dictionary.

    Args:
        sentence: Raw NMEA string, e.g. "$GPGGA,123519,4807.038,N,...*47"

    Returns:
        Dictionary with keys: type, fields, checksum_valid

    Raises:
        ValueError: If the sentence does not start with '$'.
    """
    ...
```
