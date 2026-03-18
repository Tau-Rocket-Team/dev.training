# System Architecture

This document describes the general architecture patterns used across Tau Rocket Team software projects. Understanding these patterns helps you contribute to any `dev.*` repository with minimal ramp-up time.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Repository Ecosystem](#2-repository-ecosystem)
3. [Layered Architecture Pattern](#3-layered-architecture-pattern)
4. [Data Flow](#4-data-flow)
5. [Communication Patterns](#5-communication-patterns)
6. [Error Handling Strategy](#6-error-handling-strategy)
7. [Testing Architecture](#7-testing-architecture)
8. [Deployment and CI/CD](#8-deployment-and-cicd)

---

## 1. Overview

The team's software stack supports the full lifecycle of a rocket launch:

```
┌─────────────────────────────────────────────────────────┐
│                     Ground Station                       │
│   dev.groundstation  ←──telemetry──→  dev.simulation    │
└──────────────────────────┬──────────────────────────────┘
                           │ USB / RF
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    Flight Computer                       │
│              dev.avionics  (embedded C/C++)              │
└─────────────────────────────────────────────────────────┘
```

Each layer has a clear responsibility and communicates with others through well-defined interfaces (serial protocols, REST APIs, or shared file formats).

---

## 2. Repository Ecosystem

| Repository | Language(s) | Responsibility |
|---|---|---|
| `dev.training` | Markdown | Team onboarding and standards |
| `dev.avionics` | C / C++ | Flight computer firmware |
| `dev.groundstation` | Python | Telemetry ingestion, display, and logging |
| `dev.simulation` | Python | Pre-flight trajectory and environment simulation |

### Dependency Direction

```
dev.avionics  ──(no deps)──  standalone embedded firmware
dev.groundstation  ──reads──  telemetry from dev.avionics
dev.simulation  ──validates──  parameters used in dev.avionics
```

No repository should create circular dependencies. If shared data structures are needed, define them in a shared specification document or a separate `dev.protocols` repository.

---

## 3. Layered Architecture Pattern

Each project follows a **layered architecture** to separate concerns:

```
┌──────────────────┐
│   Presentation   │  CLI, GUI, serial output
├──────────────────┤
│   Application    │  Business logic, state machines
├──────────────────┤
│     Domain       │  Core data structures and algorithms
├──────────────────┤
│  Infrastructure  │  Hardware drivers, I/O, file system, networking
└──────────────────┘
```

**Rules:**
- Higher layers may depend on lower layers, but never the reverse.
- Domain layer has **no external dependencies** — it is pure logic.
- Infrastructure layer is the only place to interact with hardware or the OS.

### Example (avionics firmware)

```
src/
├── presentation/    # Serial output formatting
├── application/     # Flight state machine (IDLE → ARMED → POWERED → COAST → ...)
├── domain/          # Physics calculations, sensor fusion
└── infrastructure/  # SPI/I2C drivers, flash storage, UART
```

---

## 4. Data Flow

### Telemetry Pipeline

```
Sensor (IMU/Baro/GPS)
    │
    ▼ raw bytes
Driver (infrastructure layer)
    │
    ▼ structured reading
Sensor Fusion (domain layer)
    │
    ▼ fused state vector
Flight State Machine (application layer)
    │
    ▼ telemetry packet
Serial Encoder (presentation layer)
    │
    ▼ UART / RF
Ground Station
```

### Ground Station Pipeline

```
Serial / RF input
    │
    ▼
Packet Parser  ──error──▶  Error Logger
    │
    ▼ parsed frame
Telemetry Store (in-memory + file)
    │
    ├──▶  Real-time Dashboard (GUI)
    └──▶  Data Logger (CSV / SQLite)
```

---

## 5. Communication Patterns

### Serial Telemetry Protocol

Packets have a fixed-length header followed by a variable payload:

```
┌──────┬──────────┬────────┬───────────────┬──────┐
│ 0xAA │ MSG_TYPE │ LENGTH │    PAYLOAD    │ CRC8 │
│  1B  │    1B    │   2B   │  0–255 bytes  │  1B  │
└──────┴──────────┴────────┴───────────────┴──────┘
```

- `0xAA` — sync byte, always present.
- `MSG_TYPE` — identifies the payload schema.
- `LENGTH` — payload length in bytes (little-endian).
- `CRC8` — checksum over `MSG_TYPE + LENGTH + PAYLOAD`.

### REST API (Ground Station ↔ Simulation)

Where applicable, services expose a simple REST API. Always version the API:

```
GET  /api/v1/telemetry/latest
GET  /api/v1/telemetry/history?since=<timestamp>
POST /api/v1/simulation/run
```

---

## 6. Error Handling Strategy

- **Embedded (C/C++)**: Return error codes; never use exceptions in ISR context. Use `assert()` only in debug builds.
- **Python**: Use typed exceptions. Define a project-specific exception hierarchy that extends built-in types.
- **Never swallow errors silently** — at minimum, log them.
- **Fail fast** in non-critical code paths; **degrade gracefully** in mission-critical paths (e.g., fall back to last-known-good sensor data before disabling a channel entirely).

---

## 7. Testing Architecture

```
tests/
├── unit/         # Test individual functions / classes in isolation
├── integration/  # Test interactions between modules
└── e2e/          # End-to-end scenario tests (e.g., full telemetry pipeline)
```

| Test Type | Speed | Scope | Run in CI |
|---|---|---|---|
| Unit | Fast (< 1s each) | Single function/class | Always |
| Integration | Medium (< 10s each) | Module boundary | Always |
| E2E | Slow (minutes) | Full system | On merge to `main` |

All tests must pass before a PR can be merged.

---

## 8. Deployment and CI/CD

Every `dev.*` repository should include a `.github/workflows/` directory with at minimum:

- **`ci.yml`** — runs on every push and PR: lint, build, and unit tests.
- **`release.yml`** — triggered on `v*` tags: builds release artifacts and creates a GitHub Release.

### Typical CI Pipeline

```
push / PR opened
      │
      ▼
┌─────────────┐     ┌──────────┐     ┌─────────────┐
│    Lint     │────▶│  Build   │────▶│    Test     │
│ (ruff/clang)│     │(cmake/py)│     │(pytest/gtest)│
└─────────────┘     └──────────┘     └─────────────┘
                                           │
                                     ✅ All pass?
                                           │
                                     PR can be merged
```
