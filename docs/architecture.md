# Arquitetura do Sistema

Este documento descreve os padrões de arquitetura geral utilizados nos projetos de software da Tau Rocket Team. Entender esses padrões ajuda você a contribuir com qualquer repositório `dev.*` com o mínimo de tempo de adaptação.

---

## Índice

- [Arquitetura do Sistema](#arquitetura-do-sistema)
  - [Índice](#índice)
- [Arquitetura do Sistema](#arquitetura-do-sistema-1)
  - [1. Visão Geral](#1-visão-geral)
  - [2. Ecossistema de Repositórios](#2-ecossistema-de-repositórios)
    - [Direção das Dependências](#direção-das-dependências)
  - [3. Padrão de Arquitetura em Camadas](#3-padrão-de-arquitetura-em-camadas)
  - [4. Fluxo de Dados Padronizado](#4-fluxo-de-dados-padronizado)
  - [5. Padrões de Comunicação](#5-padrões-de-comunicação)
    - [APIs e Protocolos](#apis-e-protocolos)
  - [6. Estratégia de Tratamento de Erros](#6-estratégia-de-tratamento-de-erros)
  - [7. Arquitetura de Testes](#7-arquitetura-de-testes)
  - [8. Automação e CI/CD](#8-automação-e-cicd)

---

# Arquitetura do Sistema

Este documento descreve os padrões de arquitetura geral utilizados nos projetos de software. Entender esses padrões ajuda você a contribuir com qualquer repositório com o mínimo de tempo de adaptação.

---

## 1. Visão Geral

Os softwares seguem uma arquitetura **modular e orientada a camadas** para garantir que sejam fáceis de entender, manter e escalar. Cada projeto é organizado em torno de um conjunto de responsabilidades claramente definidas, com interfaces padronizadas entre os módulos.

---

## 2. Ecossistema de Repositórios

A colaboração entre projetos baseia-se em uma hierarquia de dependências clara:

| Categoria | Responsabilidade |
| :--- | :--- |
| **Core / Protocols** | Definições de tipos, contratos e protocolos compartilhados. |
| **Engines / Services** | Lógica de processamento, serviços de backend e motores de cálculo. |
| **Clients / Interfaces** | Dashboards, GUIs e ferramentas de interação com o usuário. |
| **Tooling** | Scripts de automação, documentação e infraestrutura de CI/CD. |

### Direção das Dependências

1. **Dependência Linear:** Repositórios de aplicação dependem de bibliotecas core, mas bibliotecas core nunca dependem de aplicações.
2. **Zero Dependência Circular:** Se dois módulos precisam compartilhar dados, a estrutura deve ser movida para um nível inferior na hierarquia ou para um pacote de protocolos comum.

---

## 3. Padrão de Arquitetura em Camadas

Cada projeto segue uma separação de responsabilidades em quatro níveis:

```text
┌──────────────────┐
│   Apresentação   │  Interfaces (CLI, GUI, API Exposta)
├──────────────────┤
│   Aplicação      │  Orquestração, Casos de Uso, Máquinas de Estado
├──────────────────┤
│     Domínio      │  Regras de negócio, Modelos de dados, Algoritmos
├──────────────────┤
│  Infraestrutura  │  Persistência, Drivers, Comunicação externa, OS
└──────────────────┘
```

**Regras de Ouro:**
* **Isolamento do Domínio:** A camada de Domínio não deve ter dependências externas. Ela deve ser composta de código puro da linguagem escolhida.
* **Injeção de Dependência:** Camadas superiores utilizam as inferiores através de interfaces.
* **Sentido Único:** O fluxo de conhecimento sempre aponta para baixo.

---

## 4. Fluxo de Dados Padronizado

Independentemente da aplicação, os dados seguem um ciclo de vida previsível:

1. **Ingestão:** Captura de dados brutos (sensores, entrada de usuário, rede).
2. **Normalização/Parsing:** Transformação de bytes ou strings em estruturas de dados tipadas.
3. **Processamento:** Aplicação da lógica de domínio sobre os dados estruturados.
4. **Saída/Ação:** Persistência em disco, exibição visual ou transmissão para outros sistemas.

---

## 5. Padrões de Comunicação

### APIs e Protocolos
* **Interno:** Onde a performance é crítica, utiliza-se protocolos binários com cabeçalho de sincronização e Checksum (CRC).
* **Externo:** Serviços expõem interfaces **RESTful** ou **WebSockets**.
* **Versionamento:** Toda interface de comunicação deve ser versionada no caminho (ex: `/api/v1/...`).

---

## 6. Estratégia de Tratamento de Erros

* **Tipagem de Erros:** Utilize exceções ou códigos de erro tipados específicos do projeto.
* **Falha Segura (Fail-fast):** Erros de configuração ou validação devem interromper a execução no início.
* **Degradação Graciosa:** Em sistemas de tempo real, se um módulo falhar, o sistema deve tentar operar em modo reduzido em vez de travar completamente.
* **Logging:** Todo erro deve ser registrado com contexto suficiente (Timestamp, Módulo, Severidade) para depuração.

---

## 7. Arquitetura de Testes

A validação é dividida em níveis de granularidade:

| Nível | Objetivo | Execução |
| :--- | :--- | :--- |
| **Unitário** | Valida funções e classes isoladas. | Automática (CI) |
| **Integração** | Valida a comunicação entre dois ou mais módulos. | Automática (CI) |
| **End-to-End (E2E)** | Valida o fluxo completo do usuário/sistema. | Pull Requests p/ Main |

---

## 8. Automação e CI/CD

Todo repositório deve ser "plug-and-play" para automação via GitHub Actions ou ferramentas similares:

* **Lint/Format:** Verificação estática de estilo antes do build.
* **Build:** Geração de artefatos (binários, pacotes npm, imagens Docker).
* **Test:** Execução da suite de testes obrigatória.
* **Release:** Geração automática de Tags e Changelogs baseada em commits semânticos.

---
