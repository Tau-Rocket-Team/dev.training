# dev.training

> The central hub for onboarding, training, standards, and activity tracking for the Tau Rocket Team development team.

---

## Purpose

This repository helps new and existing developers get up to speed quickly, follow consistent practices, and track their progress over time. It contains:

- **Documentation** — onboarding guides, coding standards, architecture overviews, and tool setup instructions.
- **Activities** — coding challenges, assigned tasks, and a submission area for completed work.
- **Templates** — reusable templates for pull requests, issues, and new `dev.*` projects.
- **Tracking** — developer progress logs and mentor feedback records.

---

## How Developers Should Use This Repository

1. **Start with onboarding** — Read [`docs/onboarding.md`](docs/onboarding.md) first. It walks you through environment setup, team conventions, and your first contribution.
2. **Learn the guidelines** — Study [`docs/guidelines.md`](docs/guidelines.md) before writing any code. It covers branching strategy, commit messages, naming conventions, and code quality standards.
3. **Understand the architecture** — Review [`docs/architecture.md`](docs/architecture.md) to understand how team projects are structured and how they communicate.
4. **Set up your tools** — Follow [`docs/tools.md`](docs/tools.md) to install and configure every tool the team uses.
5. **Complete activities** — Work through challenges and tasks in the [`activities/`](activities/) directory and submit your work in [`activities/submissions/`](activities/submissions/).
6. **Track your progress** — Your mentor will log your progress in [`tracking/progress.md`](tracking/progress.md) and leave feedback in [`tracking/feedback.md`](tracking/feedback.md).
7. **Contribute** — Read [`CONTRIBUTING.md`](CONTRIBUTING.md) before opening pull requests or issues.

---

## How Projects Are Organized

All team repositories follow the **`dev.<projectName>`** naming pattern. Examples:

| Repository | Description |
|---|---|
| `dev.training` | This repository — onboarding and team standards |
| `dev.avionics` | Flight computer firmware and avionics software |
| `dev.groundstation` | Ground station telemetry and control software |
| `dev.simulation` | Rocket flight simulation tools |

Every `dev.*` project should use the [`templates/project_template.md`](templates/project_template.md) as its starting README to keep structure consistent across all repositories.

---

## Quick Navigation

| Path | Purpose |
|---|---|
| [`docs/onboarding.md`](docs/onboarding.md) | New developer setup guide |
| [`docs/guidelines.md`](docs/guidelines.md) | Coding standards and git workflow |
| [`docs/architecture.md`](docs/architecture.md) | System architecture and patterns |
| [`docs/tools.md`](docs/tools.md) | Recommended tools and setup |
| [`activities/challenges/`](activities/challenges/) | Coding challenges (beginner → intermediate) |
| [`activities/tasks/`](activities/tasks/) | Assigned training tasks |
| [`activities/submissions/`](activities/submissions/) | Submit your completed work here |
| [`templates/`](templates/) | PR, issue, and project templates |
| [`tracking/progress.md`](tracking/progress.md) | Developer progress tracker |
| [`tracking/feedback.md`](tracking/feedback.md) | Mentor feedback and evaluations |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How to contribute to team repositories |

---

## Getting Help

- Open a GitHub Issue using the [`templates/issue_template.md`](templates/issue_template.md) template.
- Tag your mentor or a senior developer in your pull request for review.
- Check the [discussions tab](../../discussions) for team Q&A.
