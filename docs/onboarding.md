# Onboarding Guide

Welcome to the Tau Rocket Team development team! This guide walks you through everything you need to do before writing your first line of code.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Request Access](#2-request-access)
3. [Set Up Your Development Environment](#3-set-up-your-development-environment)
4. [Clone the Repositories You Need](#4-clone-the-repositories-you-need)
5. [Understand the Team Workflow](#5-understand-the-team-workflow)
6. [Complete Your First Activity](#6-complete-your-first-activity)
7. [Introduce Yourself](#7-introduce-yourself)
8. [Onboarding Checklist](#8-onboarding-checklist)

---

## 1. Prerequisites

Before your first day, make sure you have:

- A GitHub account (send your username to your mentor so you can be added to the organization).
- Basic familiarity with the command line / terminal.
- A computer running macOS, Linux, or Windows with WSL2.

If you are missing any of the above, ask your mentor for help before proceeding.

---

## 2. Request Access

1. Send your GitHub username to your mentor or the team lead.
2. Accept the invitation to join the **Tau-Rocket-Team** GitHub organization.
3. Confirm you can see the team's private repositories at `https://github.com/Tau-Rocket-Team`.

---

## 3. Set Up Your Development Environment

Follow [`docs/tools.md`](tools.md) for a complete list of required tools and step-by-step installation instructions. At minimum you will need:

| Tool | Minimum Version | Purpose |
|---|---|---|
| Git | 2.40+ | Version control |
| VS Code | Latest stable | Primary editor |
| Python | 3.11+ | Scripting and tooling |
| Docker | 24.0+ | Containerised development |

### Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
```

### Set Up SSH Authentication

Using SSH instead of HTTPS is **strongly recommended**:

```bash
# Generate a new SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Add your key to the SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy your public key and add it to GitHub
cat ~/.ssh/id_ed25519.pub
# → Paste this at https://github.com/settings/keys
```

---

## 4. Clone the Repositories You Need

Always clone via SSH:

```bash
# Clone this training repository
git clone git@github.com:Tau-Rocket-Team/dev.training.git
cd dev.training

# Clone the project repository assigned to you (example)
git clone git@github.com:Tau-Rocket-Team/dev.avionics.git
```

---

## 5. Understand the Team Workflow

Read [`docs/guidelines.md`](guidelines.md) in full before writing any code. Key points:

- **Branch from `main`** — never commit directly to `main`.
- **Naming convention** — branches follow `type/short-description` (e.g., `feat/sensor-calibration`, `fix/telemetry-overflow`).
- **Commit messages** — follow the [Conventional Commits](https://www.conventionalcommits.org/) format.
- **Pull requests** — every change goes through a PR and requires at least one reviewer approval before merging.
- **Code review** — be respectful, specific, and constructive.

---

## 6. Complete Your First Activity

1. Navigate to [`activities/challenges/`](../activities/challenges/).
2. Start with `beginner_challenge_01.md`.
3. Create a branch: `git checkout -b feat/onboarding-<your-github-username>`.
4. Complete the challenge and place your solution in [`activities/submissions/`](../activities/submissions/) following the naming convention described there.
5. Open a pull request against `main` using the [`templates/pull_request_template.md`](../templates/pull_request_template.md) template.
6. Request your mentor as a reviewer.

---

## 7. Introduce Yourself

Open a GitHub Issue titled **"👋 Introduction — \<Your Name\>"** using the issue template and tell the team:

- Your background and why you joined the team.
- Which sub-team you are on (avionics, software, simulation, etc.).
- What you are most excited to work on.

---

## 8. Onboarding Checklist

Use this checklist to track your onboarding progress. Copy it into a comment on your introduction issue.

- [ ] GitHub account created and access granted
- [ ] Git configured with name, email, and SSH key
- [ ] Required tools installed (see `docs/tools.md`)
- [ ] `dev.training` repository cloned
- [ ] `docs/guidelines.md` read in full
- [ ] `docs/architecture.md` read in full
- [ ] First challenge completed and PR submitted
- [ ] Introduction issue opened
- [ ] Met with mentor for first 1-on-1 session

---

> **Need help?** Open a GitHub Issue or ping your mentor directly. No question is too small.
