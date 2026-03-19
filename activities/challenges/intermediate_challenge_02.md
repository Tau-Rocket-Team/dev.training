# Intermediate Challenge 02 — Flight State Machine

**Difficulty:** Intermediate  
**Estimated time:** 3–4 hours  
**Skills practiced:** State machines, OOP, event-driven design, testing

---

## Background

A flight computer's core logic is a **finite state machine (FSM)** that transitions through well-defined phases based on sensor events. Implementing this correctly — and verifying every transition — is safety-critical work.

---

## Problem Statement

Implement a `FlightStateMachine` class that models the lifecycle of a rocket flight.

### States

```
IDLE → ARMED → POWERED_ASCENT → COASTING → APOGEE → DESCENT → LANDED
                                                ↓
                                          (on detection)
```

| State | Description |
|---|---|
| `IDLE` | Pre-launch; awaiting arm command |
| `ARMED` | Ready to detect liftoff |
| `POWERED_ASCENT` | Motor burning; high acceleration |
| `COASTING` | Motor burned out; ascending on momentum |
| `APOGEE` | Peak altitude detected; deploy drogue |
| `DESCENT` | Descending under parachute |
| `LANDED` | Below threshold velocity on ground |

### Events / Transitions

| From | Event | To |
|---|---|---|
| `IDLE` | `arm()` called | `ARMED` |
| `ARMED` | Acceleration > 20 m/s² | `POWERED_ASCENT` |
| `POWERED_ASCENT` | Acceleration < 5 m/s² (burnout) | `COASTING` |
| `COASTING` | Vertical velocity ≤ 0 (apogee) | `APOGEE` |
| `APOGEE` | Automatically | `DESCENT` |
| `DESCENT` | Altitude < 50m AND velocity < 5 m/s | `LANDED` |

### Required Interface

```python
class FlightStateMachine:
    def __init__(self):
        self.state: str = "IDLE"
        self.events: list[tuple[str, str]] = []  # (timestamp, event_description)

    def arm(self) -> None:
        """Transition IDLE → ARMED. Raises RuntimeError if not in IDLE."""

    def update(self, accel_mps2: float, velocity_mps: float, altitude_m: float) -> None:
        """
        Feed sensor readings into the FSM. Triggers state transitions where
        conditions are met. Appends a record to self.events on each transition.
        """

    def is_terminal(self) -> bool:
        """Return True if the FSM has reached LANDED."""
```

### Example Usage

```python
fsm = FlightStateMachine()
fsm.arm()                           # IDLE → ARMED
fsm.update(25.0, 10.0, 5.0)        # ARMED → POWERED_ASCENT (accel > 20)
fsm.update(3.0, 150.0, 800.0)      # POWERED_ASCENT → COASTING (accel < 5)
fsm.update(3.0, 0.0, 1200.0)       # COASTING → APOGEE → DESCENT (vel ≤ 0)
fsm.update(3.0, -8.0, 30.0)        # DESCENT → LANDED (alt < 50 AND vel < 5)
print(fsm.state)                    # "LANDED"
print(fsm.is_terminal())            # True
```

---

## Requirements

1. Implement `FlightStateMachine` with all states and transitions.
2. Illegal transitions must raise `RuntimeError` with a descriptive message.
3. Each state transition must append an entry to `self.events`.
4. Write unit tests covering:
   - Happy-path flight from `IDLE` to `LANDED`.
   - Illegal transition attempts.
   - Each individual state transition.
   - Multiple sensor updates that don't trigger a transition.

---

## Bonus

- Add a `disarm()` method that transitions `ARMED → IDLE` (abort scenario).
- Add a `reset()` method that returns the FSM to `IDLE` from any state (safe recovery after anomaly).
- Persist the event log to a JSON file.

---

## Submission

1. Create a branch: `feat/int-challenge-02-<your-github-username>`.
2. Place your solution at: `activities/submissions/<your-github-username>/int_challenge_02/`.
3. Include a `README.md` in your submission folder explaining your state machine design.
4. Open a pull request and request your mentor as reviewer.
