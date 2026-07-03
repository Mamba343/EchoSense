# EchoSense

EchoSense is an open-source, local-first Wi-Fi sensing project for room-level motion and occupancy detection using ESP32 Channel State Information (CSI).

The project is inspired by high-level architecture patterns from RuView, but it is a clean-room implementation. EchoSense must not copy RuView code, assets, branding, UI, CSS, diagrams, protocol definitions, API names, firmware, models, or implementation details.

## Vision

EchoSense turns a single ESP32 NodeMCU-32 and a laptop into a privacy-preserving sensing lab:

- validate ESP32 CSI capture,
- stream CSI packets to a Python backend over the local network,
- record versioned CSI sessions,
- replay sessions without hardware,
- simulate development CSI scenarios,
- detect room-level motion and occupancy,
- visualize live and replayed CSI in a dashboard,
- build a modern Three.js room view after the data path is stable.

EchoSense does not use cameras, cloud inference, wearables, or identity tracking.

## Current Priority

The first implementation milestone is Hardware Validation.

Success criteria:

- ESP32 NodeMCU-32 successfully captures CSI.
- UDP packets are transmitted.
- Laptop receives packets.
- Packet rate is stable.
- Raw CSI is parsed successfully.

Motion detection, occupancy detection, and dashboard polish should wait until this milestone is complete.

## Scope

In scope:

- Wi-Fi CSI collection
- Hardware validation
- Shared protocol definitions
- Live mode and replay mode
- Development simulator
- Structured backend logging
- Versioned dataset recording
- Motion detection
- Occupancy detection
- Live CSI visualization
- Local Python backend
- Three.js dashboard

Not in scope:

- Heart rate
- Respiration
- DensePose
- Human skeleton estimation
- Multi-person localization
- Person identification
- Cloud inference as a requirement

## Project Structure

```text
docs/       Architecture and planning documents
shared/     Shared protocol definitions, schemas, constants, and common types
firmware/   Future ESP32 NodeMCU-32 firmware
backend/    Future single Python backend application
frontend/   Future dashboard
datasets/   Versioned CSI recording sessions
logs/       Structured runtime logs
models/     Future lightweight local models
```

Key scaffold folders:

```text
shared/
  protocol/
  schemas/
  constants/
backend/
  simulator/
datasets/
logs/
```

## v1 API Plan

REST:

- `GET /api/health`
- `GET /api/state`
- `POST /api/calibrate`
- `POST /api/record/start`
- `POST /api/record/stop`

WebSocket:

- `/ws/live`

Every WebSocket message uses:

```json
{
  "type": "...",
  "payload": {}
}
```

Initial message types:

- `csi_summary`
- `motion_state`
- `device_health`
- `occupancy_state`
- `calibration_status`

## Dashboard States

The dashboard should render from explicit states:

```text
UNKNOWN
CONNECTING
CALIBRATING
EMPTY
PRESENT
MOTION
ERROR
DISCONNECTED
```

## Documentation

- [Architecture](docs/architecture.md)
- [System Design](docs/system-design.md)
- [Data Flow](docs/data-flow.md)
- [Roadmap](docs/roadmap.md)

## Status

Architecture approved and updated. Scaffold folders and placeholder documentation exist, but firmware, backend logic, frontend code, and detection algorithms have not been implemented yet.
