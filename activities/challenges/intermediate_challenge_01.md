# Intermediate Challenge 01 — Telemetry Packet Parser

**Difficulty:** Intermediate  
**Estimated time:** 2–3 hours  
**Skills practiced:** Binary data parsing, protocol implementation, OOP, error handling, testing

---

## Background

Flight computers transmit telemetry data as binary packets over a serial link. Being able to parse these packets reliably — and handle corrupt or malformed data gracefully — is a core skill for the avionics and ground station teams.

---

## Problem Statement

Implement a **telemetry packet parser** for the following binary protocol:

### Packet Format

```
┌──────┬──────────┬────────┬───────────────┬──────┐
│ 0xAA │ MSG_TYPE │ LENGTH │    PAYLOAD    │ CRC8 │
│  1B  │    1B    │   2B   │  0–255 bytes  │  1B  │
└──────┴──────────┴────────┴───────────────┴──────┘
```

- **Sync byte** (`0xAA`): Always the first byte. If not present, scan forward until found.
- **MSG_TYPE** (1 byte): Identifies the payload format (see table below).
- **LENGTH** (2 bytes, little-endian): Number of bytes in the payload.
- **PAYLOAD**: Variable-length data interpreted according to MSG_TYPE.
- **CRC8** (1 byte): Computed over `MSG_TYPE + LENGTH + PAYLOAD` using the polynomial `0x07`.

### Message Types

| MSG_TYPE | Name | Payload Format |
|---|---|---|
| `0x01` | IMU | `ax(f32), ay(f32), az(f32), gx(f32), gy(f32), gz(f32)` — 24 bytes |
| `0x02` | BARO | `pressure(f32), temperature(f32)` — 8 bytes |
| `0x03` | GPS | `lat(f64), lon(f64), alt(f32)` — 20 bytes |

All multi-byte values are **little-endian**.

### Required Interface

```python
from dataclasses import dataclass

@dataclass
class IMUFrame:
    ax: float; ay: float; az: float
    gx: float; gy: float; gz: float

@dataclass
class BaroFrame:
    pressure: float
    temperature: float

@dataclass
class GPSFrame:
    lat: float; lon: float; alt: float

class TelemetryParser:
    def parse(self, data: bytes) -> list:
        """
        Parse a byte stream containing one or more telemetry packets.

        Returns a list of decoded frame objects (IMUFrame, BaroFrame, GPSFrame).
        Silently skips packets with invalid CRC or unknown MSG_TYPE.
        Logs a warning for each skipped packet.
        """
```

---

## Requirements

1. Implement `TelemetryParser` with the `parse(data)` method.
2. Implement `crc8(data: bytes) -> int` — CRC-8 with polynomial `0x07`.
3. Handle edge cases:
   - Stream starts mid-packet (scan for `0xAA`).
   - Multiple packets in a single call.
   - Truncated packet at end of buffer.
   - Invalid CRC — skip and continue.
4. Write unit tests for:
   - Parsing each message type correctly.
   - CRC validation (valid and invalid).
   - Handling mid-stream resync.
   - Empty / truncated inputs.

---

## Bonus

- Add a `TelemetrySerializer` class that does the reverse: takes a frame object and returns the correctly formed bytes.
- Verify that `parse(serialize(frame)) == [frame]` for all frame types in your tests.

---

## Submission

1. Create a branch: `feat/int-challenge-01-<your-github-username>`.
2. Place your solution at: `activities/submissions/<your-github-username>/int_challenge_01/`.
3. Include a `README.md` in your submission folder explaining your approach and any design decisions.
4. Open a pull request and request your mentor as reviewer.
