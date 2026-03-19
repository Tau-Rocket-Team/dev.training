# Development Guidelines

This document defines the standards every developer on the team must follow. Consistent practices reduce friction, make code reviews faster, and keep the codebase maintainable as the team grows.

---

## Table of Contents

1. [Git Workflow](#1-git-workflow)
2. [Branching Strategy](#2-branching-strategy)
3. [Commit Messages](#3-commit-messages)
4. [Pull Requests](#4-pull-requests)
5. [Code Review Etiquette](#5-code-review-etiquette)
6. [Naming Conventions](#6-naming-conventions)
7. [Clean Code Principles](#7-clean-code-principles)
8. [Project Structure](#8-project-structure)
9. [Documentation Standards](#9-documentation-standards)

---

## 1. Git Workflow

We follow a simplified **GitHub Flow**:

```
main  ←── feature branch (PR + review) ←── your local branch
```

**Rules:**
- `main` is always deployable / stable.
- **Never** force-push to `main`.
- Every change must be made in its own branch and merged via a pull request.
- Delete your branch after the PR is merged.

---

## 2. Branching Strategy

Branch names follow the pattern `<type>/<short-description>`:

| Type | When to use | Example |
|---|---|---|
| `feat` | New feature or capability | `feat/add-gps-parser` |
| `fix` | Bug fix | `fix/sensor-overflow-bug` |
| `docs` | Documentation only | `docs/update-onboarding` |
| `refactor` | Code restructuring, no behaviour change | `refactor/extract-telemetry-utils` |
| `test` | Adding or fixing tests | `test/gps-unit-tests` |
| `chore` | Build scripts, CI, dependencies | `chore/update-dependencies` |

**Rules:**
- Use **lowercase** and **hyphens** only — no spaces or underscores.
- Keep the description short (2–4 words).
- Branch from the latest `main`: `git checkout -b feat/my-feature origin/main`.

---

## 3. Commit Messages

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

### Rules

- **Subject line**: imperative mood, ≤72 characters, no period at the end.
- **Body**: explain *what* and *why*, not *how*. Wrap at 72 characters.
- **Footer**: reference issues (`Closes #42`, `Refs #7`).

### Examples

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

Use the [`templates/pull_request_template.md`](../templates/pull_request_template.md) when opening a PR.

**Requirements:**
- Link the related issue (`Closes #<number>`).
- Provide a clear description of *what* changed and *why*.
- Include screenshots or test output where applicable.
- All CI checks must pass before requesting review.
- At least **one approval** from a team member is required before merging.
- Prefer **squash and merge** to keep `main` history clean.

**PR Size:**
- Keep PRs small and focused. A good rule of thumb: a reviewer should be able to fully understand the change in 20 minutes.
- If a PR is getting large, split it into smaller, sequential PRs.

---

## 5. Code Review Etiquette

**As a reviewer:**
- Review within 48 hours of being assigned.
- Be specific: point to the exact line and explain the concern.
- Distinguish between blockers (`BLOCKING:`) and suggestions (`NIT:` / `SUGGESTION:`).
- Approve when you are satisfied — don't leave PRs open indefinitely.

**As an author:**
- Respond to every comment, even if only to acknowledge it.
- Don't take feedback personally — it's about the code, not you.
- If you disagree with a comment, explain your reasoning clearly.

---

## 6. Naming Conventions

### Files and Directories
- Use `snake_case` for Python files and scripts: `sensor_parser.py`
- Use `kebab-case` for Markdown and config files: `onboarding.md`, `docker-compose.yml`
- Use `PascalCase` for class files where the language convention demands it (e.g., C++): `TelemetryFrame.hpp`

### Variables and Functions (Python)
```python
# Variables: snake_case
packet_count = 0
sensor_data = []

# Functions: snake_case, verb-first
def parse_gps_frame(raw_bytes):
    ...

# Classes: PascalCase
class TelemetryParser:
    ...

# Constants: UPPER_SNAKE_CASE
MAX_PACKET_SIZE = 1024
DEFAULT_BAUD_RATE = 115200
```

### Variables and Functions (C / C++)
```cpp
// Variables: camelCase or snake_case (be consistent per project)
uint32_t packetCount = 0;

// Functions: camelCase
void parseTelemetryFrame(uint8_t* buffer, size_t len);

// Classes / Structs: PascalCase
class TelemetryParser { ... };

// Constants / Macros: UPPER_SNAKE_CASE
#define MAX_PACKET_SIZE 1024
```

### Branches and Tags
- Branches: `feat/short-description` (see §2)
- Release tags: `v<major>.<minor>.<patch>` (e.g., `v1.2.0`)

---

## 7. Clean Code Principles

1. **Readable over clever** — code is read far more than it is written.
2. **Single responsibility** — every function and class does one thing well.
3. **Meaningful names** — a good name is better than a comment.
4. **Keep functions small** — if a function needs scrolling, it probably does too much.
5. **No magic numbers** — replace literals with named constants.
6. **Don't repeat yourself (DRY)** — extract repeated logic into reusable functions.
7. **Fail loudly** — raise exceptions / errors rather than silently ignoring failures.
8. **Leave the code cleaner than you found it** — fix small issues you encounter, even if they are not in your ticket.

### Example: Before vs. After

```python
# ❌ Hard to understand
def p(d, t):
    return d * 1000 / t

# ✅ Clear and maintainable
METERS_PER_KILOMETER = 1000

def calculate_speed_kph(distance_meters: float, time_seconds: float) -> float:
    """Return speed in km/h given distance in metres and time in seconds."""
    return (distance_meters / METERS_PER_KILOMETER) / (time_seconds / 3600)
```

---

## 8. Project Structure

Every `dev.*` repository should follow this layout:

```
dev.<projectName>/
├── README.md           # Project overview (use templates/project_template.md)
├── CONTRIBUTING.md     # How to contribute to this specific project
├── src/                # Source code
│   ├── <module>/
│   └── main.py / main.cpp
├── tests/              # Unit and integration tests
├── docs/               # Project-specific documentation
├── scripts/            # Build, deploy, and utility scripts
├── .github/
│   ├── workflows/      # CI/CD pipelines
│   └── PULL_REQUEST_TEMPLATE.md
└── .gitignore
```

---

## 9. Documentation Standards

- Every public function, class, and module must have a docstring / doc comment.
- Keep documentation close to the code — update docs in the same PR as the code change.
- Use Markdown for all written documentation.
- Use tables, code blocks, and headers to improve readability.

**Python docstring example:**
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
