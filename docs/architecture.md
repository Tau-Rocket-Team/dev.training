# Arquitetura do Sistema

Este documento descreve os padrões de arquitetura geral utilizados nos projetos de software da Tau Rocket Team. Entender esses padrões ajuda você a contribuir com qualquer repositório `dev.*` com o mínimo de tempo de adaptação.

---

## Índice

1. [Visão Geral](#1-visão-geral)
2. [Ecossistema de Repositórios](#2-ecossistema-de-repositórios)
3. [Padrão de Arquitetura em Camadas](#3-padrão-de-arquitetura-em-camadas)
4. [Fluxo de Dados](#4-fluxo-de-dados)
5. [Padrões de Comunicação](#5-padrões-de-comunicação)
6. [Estratégia de Tratamento de Erros](#6-estratégia-de-tratamento-de-erros)
7. [Arquitetura de Testes](#7-arquitetura-de-testes)
8. [Implantação e CI/CD](#8-implantação-e-cicd)

---

## 1. Visão Geral

A pilha de software do time suporta o ciclo de vida completo de um lançamento de foguete:

```
┌─────────────────────────────────────────────────────────┐
│                     Estação Terrestre                    │
│   dev.groundstation  ←──telemetria──→  dev.simulation   │
└──────────────────────────┬──────────────────────────────┘
                           │ USB / RF
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    Computador de Voo                     │
│              dev.avionics  (embedded C/C++)              │
└─────────────────────────────────────────────────────────┘
```

Cada camada tem uma responsabilidade clara e se comunica com as outras por meio de interfaces bem definidas (protocolos seriais, APIs REST ou formatos de arquivo compartilhados).

---

## 2. Ecossistema de Repositórios

| Repositório | Linguagem(ns) | Responsabilidade |
|---|---|---|
| `dev.training` | Markdown | Integração e padrões do time |
| `dev.avionics` | C / C++ | Firmware do computador de voo |
| `dev.groundstation` | Python | Ingestão, exibição e registro de telemetria |
| `dev.simulation` | Python | Simulação de trajetória e ambiente pré-voo |

### Direção das Dependências

```
dev.avionics  ──(sem deps)──  firmware embarcado standalone
dev.groundstation  ──lê──  telemetria do dev.avionics
dev.simulation  ──valida──  parâmetros usados no dev.avionics
```

Nenhum repositório deve criar dependências circulares. Se estruturas de dados compartilhadas forem necessárias, defina-as em um documento de especificação compartilhado ou em um repositório `dev.protocols` separado.

---

## 3. Padrão de Arquitetura em Camadas

Cada projeto segue uma **arquitetura em camadas** para separar responsabilidades:

```
┌──────────────────┐
│   Apresentação   │  CLI, GUI, saída serial
├──────────────────┤
│   Aplicação      │  Lógica de negócio, máquinas de estado
├──────────────────┤
│     Domínio      │  Estruturas de dados e algoritmos centrais
├──────────────────┤
│  Infraestrutura  │  Drivers de hardware, E/S, sistema de arquivos, rede
└──────────────────┘
```

**Regras:**
- Camadas superiores podem depender de camadas inferiores, mas nunca o contrário.
- A camada de domínio não tem **dependências externas** — é lógica pura.
- A camada de infraestrutura é o único lugar para interagir com hardware ou o SO.

### Exemplo (firmware de aviônica)

```
src/
├── presentation/    # Formatação de saída serial
├── application/     # Máquina de estado de voo (IDLE → ARMED → POWERED → COAST → ...)
├── domain/          # Cálculos físicos, fusão de sensores
└── infrastructure/  # Drivers SPI/I2C, armazenamento flash, UART
```

---

## 4. Fluxo de Dados

### Pipeline de Telemetria

```
Sensor (IMU/Baro/GPS)
    │
    ▼ bytes brutos
Driver (camada de infraestrutura)
    │
    ▼ leitura estruturada
Fusão de Sensores (camada de domínio)
    │
    ▼ vetor de estado fundido
Máquina de Estado de Voo (camada de aplicação)
    │
    ▼ pacote de telemetria
Codificador Serial (camada de apresentação)
    │
    ▼ UART / RF
Estação Terrestre
```

### Pipeline da Estação Terrestre

```
Entrada serial / RF
    │
    ▼
Parser de Pacotes  ──erro──▶  Logger de Erros
    │
    ▼ frame analisado
Armazenamento de Telemetria (memória + arquivo)
    │
    ├──▶  Dashboard em Tempo Real (GUI)
    └──▶  Logger de Dados (CSV / SQLite)
```

---

## 5. Padrões de Comunicação

### Protocolo de Telemetria Serial

Os pacotes têm um cabeçalho de comprimento fixo seguido de um payload variável:

```
┌──────┬──────────┬────────┬───────────────┬──────┐
│ 0xAA │ MSG_TYPE │ LENGTH │    PAYLOAD    │ CRC8 │
│  1B  │    1B    │   2B   │  0–255 bytes  │  1B  │
└──────┴──────────┴────────┴───────────────┴──────┘
```

- `0xAA` — byte de sincronização, sempre presente.
- `MSG_TYPE` — identifica o esquema do payload.
- `LENGTH` — comprimento do payload em bytes (little-endian).
- `CRC8` — checksum sobre `MSG_TYPE + LENGTH + PAYLOAD`.

### API REST (Estação Terrestre ↔ Simulação)

Onde aplicável, os serviços expõem uma API REST simples. Sempre versione a API:

```
GET  /api/v1/telemetry/latest
GET  /api/v1/telemetry/history?since=<timestamp>
POST /api/v1/simulation/run
```

---

## 6. Estratégia de Tratamento de Erros

- **Embarcado (C/C++)**: Retorne códigos de erro; nunca use exceções em contexto de ISR. Use `assert()` apenas em builds de debug.
- **Python**: Use exceções tipadas. Defina uma hierarquia de exceções específica do projeto que estende os tipos embutidos.
- **Nunca engula erros silenciosamente** — no mínimo, registre-os.
- **Falhe rapidamente** em caminhos de código não críticos; **degrade graciosamente** em caminhos críticos (ex.: recorra aos dados do último sensor válido conhecido antes de desabilitar um canal completamente).

---

## 7. Arquitetura de Testes

```
tests/
├── unit/         # Teste funções / classes individuais em isolamento
├── integration/  # Teste interações entre módulos
└── e2e/          # Testes de cenário end-to-end (ex.: pipeline completo de telemetria)
```

| Tipo de Teste | Velocidade | Escopo | Executar no CI |
|---|---|---|---|
| Unitário | Rápido (< 1s cada) | Única função/classe | Sempre |
| Integração | Médio (< 10s cada) | Fronteira do módulo | Sempre |
| E2E | Lento (minutos) | Sistema completo | Ao fazer merge no `main` |

Todos os testes devem passar antes que um PR possa ser integrado.

---

## 8. Implantação e CI/CD

Todo repositório `dev.*` deve incluir um diretório `.github/workflows/` com no mínimo:

- **`ci.yml`** — executado a cada push e PR: lint, build e testes unitários.
- **`release.yml`** — acionado em tags `v*`: cria artefatos de release e uma GitHub Release.

### Pipeline Típico de CI

```
push / PR aberto
      │
      ▼
┌─────────────┐     ┌──────────┐     ┌─────────────┐
│    Lint     │────▶│  Build   │────▶│    Test     │
│ (ruff/clang)│     │(cmake/py)│     │(pytest/gtest)│
└─────────────┘     └──────────┘     └─────────────┘
                                           │
                                     ✅ Tudo passou?
                                           │
                                     PR pode ser integrado
```
