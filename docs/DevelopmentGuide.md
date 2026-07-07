# Development Guide

EchoSense is currently a scaffold. Use this guide to keep future implementation aligned with the existing architecture.

## Before Coding

1. Read [AGENTS.md](../AGENTS.md).
2. Read [Current Status](CurrentStatus.md).
3. Read [Architecture](Architecture.md).
4. Check [Known Issues](KnownIssues.md).
5. Confirm the work belongs to the current milestone.

## Current Milestone Discipline

The documented priority is Hardware Validation. Do not start motion detection, occupancy detection, ML, or dashboard polish before live CSI capture and parsing are proven.

## Suggested Implementation Order

1. Shared protocol, schemas, and constants.
2. Minimal ESP32 firmware.
3. Minimal Python UDP receiver.
4. Parser and packet-rate logging.
5. First validation dataset.
6. Replay source.
7. Simulator source.
8. API server.
9. Dashboard.
10. Motion and occupancy features.

## Backend Conventions

Existing docs imply these conventions:

- Use Python.
- Keep one backend process for v1.
- Use type hints where practical.
- Prefer clear module boundaries over early abstractions.
- Use structured logging, not `print()`.
- Validate all UDP input.
- Keep bind host local by default.
- Make live, replay, and simulator feed the same pipeline.

## Frontend Conventions

Existing docs imply these conventions:

- Render from explicit dashboard states.
- Use `/api/state` for current status.
- Use `/ws/live` for streaming updates.
- Treat live and replay data the same except for source diagnostics.
- Do not imply precise location, identity, skeleton tracking, or biometric sensing.
- Add Three.js only after the data path is stable.

## Firmware Conventions

Existing docs imply these conventions:

- Keep firmware focused on CSI capture and UDP transport.
- Avoid detection algorithms on the ESP32 in v1.
- Use the shared packet contract.
- Include metadata needed for packet-rate and loss analysis.

## Data and Logging

- Store datasets in versioned session folders under `datasets/`.
- Store structured logs under `logs/`.
- Do not commit large raw recordings unless a sample dataset policy is created.
- Do not write Wi-Fi credentials to committed files, logs, or datasets.

## Testing Expectations

No tests exist yet. Future tests should cover:

- packet parsing,
- schema validation,
- replay ordering,
- simulator protocol compatibility,
- REST contracts,
- WebSocket message envelopes,
- dashboard state transitions.

## Documentation Expectations

When adding implementation, update the relevant docs in the same change:

- API changes: `docs/API.md`.
- Dataset changes: `docs/Database.md`.
- Firmware or board changes: `docs/Hardware.md`.
- Setup commands: `docs/Setup.md`.
- Architectural changes: `docs/Architecture.md` and `docs/DesignDecisions.md`.

