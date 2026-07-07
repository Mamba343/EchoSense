# Design Decisions

This page records decisions already expressed by the existing repository documentation.

## Clean-Room Implementation

Decision: EchoSense may learn from high-level architecture patterns, but must not copy RuView code, assets, UI, CSS, diagrams, protocol definitions, API names, firmware, models, branding, or implementation details.

Reason: Keep the project legally and technically independent.

## Local-First and Privacy-Preserving

Decision: EchoSense is designed around local hardware, local backend processing, and a local browser dashboard.

Reason: The project scope excludes cameras, cloud inference as a requirement, identity tracking, wearables, and biometric sensing.

## Hardware Validation First

Decision: Hardware Validation must precede motion detection, occupancy detection, ML, and dashboard polish.

Reason: Future features depend on proven CSI capture, UDP delivery, stable packet rates, and raw parsing.

## One Python Backend

Decision: Keep the backend as one Python application with clear internal modules.

Reason: The current system is small and does not need microservices. A single process keeps live, replay, recording, API, and logging easier to reason about during v1.

## Shared Contracts Before Runtime Code

Decision: Define shared protocol, schema, and constant contracts before firmware/backend/frontend implementation.

Reason: Firmware, simulator, replay, backend, and frontend should not invent duplicate packet names, message names, states, or constants.

## Live and Replay Parity

Decision: Live mode and replay mode should expose the same frontend contract.

Reason: Developers should be able to test parsing, visualization, state logic, and algorithms without hardware.

## Simulator Uses the Same Protocol

Decision: The simulator must generate packets matching the EchoSense firmware protocol.

Reason: A separate simulator protocol would hide integration problems and weaken tests.

## Structured Logging

Decision: Runtime diagnostics should use structured logs, not `print()`.

Reason: Packet rates, parse failures, state transitions, calibration events, and recording events need to be searchable and machine-readable.

## Amplitude-First Detection

Decision: Initial feature work should use amplitude summaries first and treat phase features as experimental until measured.

Reason: The existing docs identify phase noise as a risk and recommend an amplitude-first path.

## Minimal v1 API Surface

Decision: Start with five REST endpoints and one WebSocket endpoint.

Reason: The API should stay small until the data path is validated.

## Assumptions Not Yet Proven

- ESP32 NodeMCU-32 can capture CSI reliably enough for this use case.
- UDP packet rate will be stable enough for analysis.
- Single-node CSI can support useful room-level motion or occupancy states in controlled conditions.
- The planned dashboard state machine will be sufficient after real data is collected.

