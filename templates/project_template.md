# dev.<projectName>

> One-sentence description of what this project does.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Prerequisites](#2-prerequisites)
3. [Getting Started](#3-getting-started)
4. [Project Structure](#4-project-structure)
5. [Usage](#5-usage)
6. [Testing](#6-testing)
7. [Contributing](#7-contributing)
8. [License](#8-license)

---

## 1. Overview

<!-- 
  Describe the project in 3–5 sentences:
  - What problem does it solve?
  - How does it fit into the broader system? (see docs/architecture.md)
  - What are its primary inputs and outputs?
-->

---

## 2. Prerequisites

| Requirement | Version | Notes |
|---|---|---|
| Python | 3.11+ | Or replace with your language |
| Docker | 24.0+ | Optional, for containerised builds |
| <!-- tool --> | <!-- version --> | <!-- notes --> |

See [`dev.training/docs/tools.md`](https://github.com/Tau-Rocket-Team/dev.training/blob/main/docs/tools.md) for installation instructions.

---

## 3. Getting Started

```bash
# 1. Clone the repository
git clone git@github.com:Tau-Rocket-Team/dev.<projectName>.git
cd dev.<projectName>

# 2. Create and activate a virtual environment (Python projects)
python3 -m venv .venv
source .venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Run the application
python src/main.py
```

---

## 4. Project Structure

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

## 5. Usage

<!-- Provide clear usage examples with code blocks -->

```python
# Example usage
from src.<module> import <Class>

obj = <Class>()
result = obj.do_something()
print(result)
```

---

## 6. Testing

```bash
# Run all tests
pytest

# Run with coverage report
pytest --cov=src --cov-report=term-missing

# Run only unit tests
pytest tests/unit/
```

All tests must pass before opening a pull request. See CI configuration at `.github/workflows/ci.yml`.

---

## 7. Contributing

Please read [`CONTRIBUTING.md`](CONTRIBUTING.md) before making any changes.

In brief:
1. Fork / branch from `main` following the naming convention in the team guidelines.
2. Make your changes on a feature branch.
3. Open a pull request using the PR template.
4. All CI checks must pass and at least one review approval is required.

---

## 8. License

<!-- 
  Add the appropriate license. Check with the team lead before publishing.
  Example: MIT, Apache 2.0, or proprietary.
-->

© Tau Rocket Team. All rights reserved.
