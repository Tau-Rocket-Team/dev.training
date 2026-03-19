# dev.<projectName>

> Descrição em uma frase do que este projeto faz.

---

## Índice

1. [Visão Geral](#1-visão-geral)
2. [Pré-requisitos](#2-pré-requisitos)
3. [Primeiros Passos](#3-primeiros-passos)
4. [Estrutura do Projeto](#4-estrutura-do-projeto)
5. [Uso](#5-uso)
6. [Testes](#6-testes)
7. [Contribuindo](#7-contribuindo)
8. [Licença](#8-licença)

---

## 1. Visão Geral

<!-- 
  Descreva o projeto em 3–5 frases:
  - Qual problema ele resolve?
  - Como ele se encaixa no sistema mais amplo? (veja docs/architecture.md)
  - Quais são suas principais entradas e saídas?
-->

---

## 2. Pré-requisitos

| Requisito | Versão | Observações |
|---|---|---|
| Python | 3.11+ | Ou substitua pela sua linguagem |
| Docker | 24.0+ | Opcional, para builds em contêiner |
| <!-- ferramenta --> | <!-- versão --> | <!-- observações --> |

Veja [`dev.training/docs/tools.md`](https://github.com/Tau-Rocket-Team/dev.training/blob/main/docs/tools.md) para instruções de instalação.

---

## 3. Primeiros Passos

```bash
# 1. Clone o repositório
git clone git@github.com:Tau-Rocket-Team/dev.<projectName>.git
cd dev.<projectName>

# 2. Crie e ative um ambiente virtual (projetos Python)
python3 -m venv .venv
source .venv/bin/activate

# 3. Instale as dependências
pip install -r requirements.txt

# 4. Execute a aplicação
python src/main.py
```

---

## 4. Estrutura do Projeto

```
dev.<projectName>/
├── README.md
├── CONTRIBUTING.md
├── src/
│   ├── <module>/
│   └── main.py
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
├── scripts/
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── release.yml
└── .gitignore
```

---

## 5. Uso

<!-- Forneça exemplos claros de uso com blocos de código -->

```python
# Exemplo de uso
from src.<module> import <Class>

obj = <Class>()
result = obj.do_something()
print(result)
```

---

## 6. Testes

```bash
# Executar todos os testes
pytest

# Executar com relatório de cobertura
pytest --cov=src --cov-report=term-missing

# Executar apenas testes unitários
pytest tests/unit/
```

Todos os testes devem passar antes de abrir um pull request. Veja a configuração de CI em `.github/workflows/ci.yml`.

---

## 7. Contribuindo

Por favor, leia [`CONTRIBUTING.md`](CONTRIBUTING.md) antes de fazer qualquer alteração.

Em resumo:
1. Fork / crie branch a partir de `main` seguindo a convenção de nomenclatura nas diretrizes do time.
2. Faça suas alterações em um branch de funcionalidade.
3. Abra um pull request usando o template de PR.
4. Todas as verificações de CI devem passar e pelo menos uma aprovação de revisão é necessária.

---

## 8. Licença

<!-- 
  Adicione a licença apropriada. Consulte o líder do time antes de publicar.
  Exemplo: MIT, Apache 2.0, ou proprietária.
-->

© Tau Rocket Team. Todos os direitos reservados.
