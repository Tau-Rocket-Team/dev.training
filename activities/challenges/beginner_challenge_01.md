# Beginner Challenge 01 — FizzBuzz with a Twist

**Difficulty:** Beginner  
**Estimated time:** 20–30 minutes  
**Skills practiced:** Loops, conditionals, functions, clean code

---

## Background

FizzBuzz is a classic programming exercise. This version extends the original to make it more interesting and to practice writing clean, well-structured code.

---

## Problem Statement

Write a function `fizzbuzz(n)` that takes a positive integer `n` and returns a **list of strings** for all numbers from 1 to `n` (inclusive), following these rules:

- If the number is divisible by **3**, return `"Fizz"`.
- If the number is divisible by **5**, return `"Buzz"`.
- If the number is divisible by **both 3 and 5**, return `"FizzBuzz"`.
- If the number is divisible by **7**, **append** `"Rocket"` to whatever the result is so far (e.g., `"Rocket"`, `"FizzRocket"`, `"BuzzRocket"`, `"FizzBuzzRocket"`).
- Otherwise, return the number as a string.

### Example

```
fizzbuzz(20) → [
  "1", "2", "Fizz", "4", "Buzz", "Fizz", "Rocket", "8", "Fizz",
  "Buzz", "11", "Fizz", "13", "Rocket", "FizzBuzz", "16", "17",
  "Fizz", "19", "Buzz"
]
```

---

## Requirements

1. Implement `fizzbuzz(n: int) -> list[str]` in Python (or the language of your choice).
2. The function must handle edge cases:
   - `n = 0` → return an empty list.
   - Negative `n` → raise a `ValueError` with a descriptive message.
3. Write at least **5 unit tests** covering different scenarios.
4. Your code must be properly formatted and linted before submission.

---

## Bonus

- Accept an optional `rules` parameter that is a list of `(divisor, label)` tuples, making the function fully configurable. The standard FizzBuzz rules should be the default.

```python
# Example usage with custom rules
fizzbuzz(15, rules=[(3, "Fizz"), (5, "Buzz"), (7, "Rocket")])
```

---

## Submission

1. Create a branch: `feat/challenge-01-<your-github-username>`.
2. Place your solution at: `activities/submissions/<your-github-username>/challenge_01/solution.py`.
3. Place your tests at: `activities/submissions/<your-github-username>/challenge_01/test_solution.py`.
4. Open a pull request and request your mentor as reviewer.

See [`activities/submissions/README.md`](../submissions/README.md) for full submission instructions.
