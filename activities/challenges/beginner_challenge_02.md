# Beginner Challenge 02 — Temperature Converter

**Difficulty:** Beginner  
**Estimated time:** 30–45 minutes  
**Skills practiced:** Functions, dictionaries, input validation, error handling

---

## Background

Temperature conversions are a common real-world task. Rockets deal with extreme temperatures, so accurate conversion between units is genuinely important in aerospace software.

---

## Problem Statement

Create a `TemperatureConverter` class that can convert between **Celsius (C)**, **Fahrenheit (F)**, and **Kelvin (K)**.

### Conversion Formulas

| From → To | Formula |
|---|---|
| C → F | `F = (C × 9/5) + 32` |
| C → K | `K = C + 273.15` |
| F → C | `C = (F − 32) × 5/9` |
| F → K | `K = (F − 32) × 5/9 + 273.15` |
| K → C | `C = K − 273.15` |
| K → F | `F = (K − 273.15) × 9/5 + 32` |

### Class Interface

```python
class TemperatureConverter:
    def convert(self, value: float, from_unit: str, to_unit: str) -> float:
        """
        Convert a temperature value from one unit to another.

        Args:
            value: The temperature value to convert.
            from_unit: Source unit — "C", "F", or "K".
            to_unit: Target unit — "C", "F", or "K".

        Returns:
            Converted temperature rounded to 4 decimal places.

        Raises:
            ValueError: If either unit is not "C", "F", or "K".
            ValueError: If the resulting temperature is below absolute zero.
        """
```

### Example Usage

```python
conv = TemperatureConverter()

conv.convert(100, "C", "F")   # → 212.0
conv.convert(32, "F", "C")    # → 0.0
conv.convert(0, "C", "K")     # → 273.15
conv.convert(0, "K", "C")     # → -273.15
conv.convert(-300, "C", "K")  # → raises ValueError (below absolute zero)
```

---

## Requirements

1. Implement the `TemperatureConverter` class.
2. Validate inputs — raise `ValueError` with a meaningful message for invalid units or physically impossible temperatures (below 0 K).
3. Write at least **8 unit tests** including edge cases (absolute zero, same unit conversion, invalid input).
4. Do **not** use any external libraries — standard Python only.

---

## Bonus

- Add a `convert_batch(values, from_unit, to_unit)` method that accepts a list and returns a list of converted values.
- Add a command-line interface so the script can be run as: `python solution.py 100 C F`.

---

## Submission

1. Create a branch: `feat/challenge-02-<your-github-username>`.
2. Place your solution at: `activities/submissions/<your-github-username>/challenge_02/solution.py`.
3. Place your tests at: `activities/submissions/<your-github-username>/challenge_02/test_solution.py`.
4. Open a pull request and request your mentor as reviewer.
