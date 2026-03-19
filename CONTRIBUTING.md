# Contributing to Tau Rocket Team Repositories

Thank you for contributing! This guide explains how to contribute to any `dev.*` repository in the Tau Rocket Team organization.

---

## Table of Contents

1. [Code of Conduct](#1-code-of-conduct)
2. [Before You Start](#2-before-you-start)
3. [How to Report a Bug](#3-how-to-report-a-bug)
4. [How to Request a Feature](#4-how-to-request-a-feature)
5. [How to Submit a Pull Request](#5-how-to-submit-a-pull-request)
6. [Development Workflow](#6-development-workflow)
7. [Commit Message Guidelines](#7-commit-message-guidelines)
8. [Code Style](#8-code-style)
9. [Getting Help](#9-getting-help)

---

## 1. Code of Conduct

We are committed to a welcoming, respectful, and inclusive environment.

- Be kind and respectful in all interactions.
- Critique the code, never the person.
- Accept feedback gracefully — we are all here to learn.
- Ask questions freely; no question is too basic.

Violations should be reported to the team lead.

---

## 2. Before You Start

1. **Complete onboarding** — Read [`docs/onboarding.md`](docs/onboarding.md) if you have not already.
2. **Read the guidelines** — Understand [`docs/guidelines.md`](docs/guidelines.md) before writing any code.
3. **Check existing issues** — Your idea may already be tracked. Search before opening a duplicate.
4. **Discuss large changes first** — For significant features or architecture changes, open an issue to discuss before writing code. This saves everyone time.

---

## 3. How to Report a Bug

1. Search [existing issues](../../issues) to check if it has been reported already.
2. If not, open a new issue using the [issue template](templates/issue_template.md).
3. Include:
   - A clear title describing the bug.
   - Steps to reproduce.
   - What you expected vs. what happened.
   - Your environment (OS, language version, branch).
   - Any relevant logs or error output.

---

## 4. How to Request a Feature

1. Open an issue using the [issue template](templates/issue_template.md) and select "Feature request".
2. Describe the problem your feature solves.
3. Explain your proposed solution.
4. Wait for discussion and a team member to approve the idea before building it.

---

## 5. How to Submit a Pull Request

### Quick Reference

```bash
# 1. Update your local main
git checkout main
git pull origin main

# 2. Create a feature branch
git checkout -b feat/my-feature

# 3. Make your changes
# ... edit files ...

# 4. Stage and commit
git add .
git commit -m "feat(scope): describe the change"

# 5. Push and open PR
git push origin feat/my-feature
# → Open PR on GitHub and use the pull_request_template.md
```

### Pull Request Requirements

- Link the issue your PR addresses (`Closes #42`).
- Fill in the PR template completely.
- All CI checks must be green before requesting review.
- At least **one approval** from a team member before merging.
- Keep PRs small and focused — one purpose per PR.

---

## 6. Development Workflow

We follow **GitHub Flow**:

```
main  ←── PR (reviewed + CI green) ←── feature branch
```

1. **Never commit directly to `main`.**
2. Always branch from the latest `main`.
3. Delete your branch after the PR is merged.

### Branch Naming

```
feat/short-description       # New features
fix/short-description        # Bug fixes
docs/short-description       # Documentation
refactor/short-description   # Refactoring
test/short-description       # Tests
chore/short-description      # Build/CI/deps
```

---

## 7. Commit Message Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

[optional body — what and why, not how]

[optional footer — Closes #42]
```

**Examples:**

```
feat(parser): add support for GPS NMEA sentences
fix(telemetry): prevent crash on empty packet
docs(readme): add usage examples
test(parser): add edge case tests for CRC validation
```

---

## 8. Code Style

- **Python**: [Ruff](https://docs.astral.sh/ruff/) for linting and formatting. Run `ruff check .` and `ruff format .` before committing.
- **C/C++**: [clang-format](https://clang.llvm.org/docs/ClangFormat.html) with the project `.clang-format` file. Run before committing.
- **Markdown**: Consistent headers, code blocks, and tables. Wrap long lines.

All projects include a linting step in CI that will fail the build if style issues are found.

---

## 9. Getting Help

- **GitHub Issues** — For bugs and feature requests.
- **GitHub Discussions** — For questions and general team conversation.
- **Direct message** — Reach out to your mentor or a senior developer.
- **PR comments** — Tag a reviewer if you need guidance mid-PR.

We want you to succeed — don't hesitate to ask for help!
