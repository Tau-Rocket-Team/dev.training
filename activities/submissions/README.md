# Submissions

This directory is where developers submit their completed challenges and tasks.

---

## Directory Structure

Each developer has their own sub-directory named after their GitHub username:

```
submissions/
├── README.md               ← This file
├── jane-smith/
│   ├── challenge_01/
│   │   ├── solution.py
│   │   └── test_solution.py
│   └── int_challenge_01/
│       ├── solution.py
│       ├── test_solution.py
│       └── README.md
└── john-doe/
    └── challenge_01/
        ├── solution.py
        └── test_solution.py
```

---

## Submission Instructions

### Step 1: Create Your Directory

If your `submissions/<your-github-username>/` directory does not exist yet, create it:

```bash
mkdir -p activities/submissions/<your-github-username>
```

### Step 2: Add Your Solution

Place your files in the correct sub-directory as specified by the challenge:

```bash
# Example for challenge 01
mkdir -p activities/submissions/<your-github-username>/challenge_01
# Add solution.py and test_solution.py
```

### Step 3: Run Tests and Lint Locally

Always verify your work passes before submitting:

```bash
cd activities/submissions/<your-github-username>/challenge_01

# Run tests
pytest test_solution.py -v

# Lint
ruff check solution.py
ruff format --check solution.py
```

### Step 4: Open a Pull Request

1. Push your branch.
2. Open a PR against `main` using the [pull request template](../../templates/pull_request_template.md).
3. Title the PR: `submission: <challenge-name> — <your-github-username>`.
4. Request your mentor as a reviewer.

---

## Review Criteria

Mentors will evaluate submissions on:

| Criterion | Description |
|---|---|
| **Correctness** | Does the solution produce the right output for all cases? |
| **Tests** | Are edge cases covered? Do all tests pass? |
| **Code quality** | Is the code readable, well-named, and properly documented? |
| **Style** | Does it pass linting without warnings? |
| **Design** | Is the solution well-structured and easy to extend? |
