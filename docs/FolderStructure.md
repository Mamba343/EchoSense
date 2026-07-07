# Folder Structure

The repository is currently a scaffold. Several folders are intentionally empty or contain only placeholder READMEs.

```text
EchoSense/
  AGENTS.md
  README.md
  TODO.md
  docs/
  shared/
    protocol/
    schemas/
    constants/
  firmware/
  backend/
    simulator/
  frontend/
  datasets/
  logs/
  models/
```

## Root Files

| Path | Purpose |
| --- | --- |
| `README.md` | Project overview, scope, planned API, docs links, status |
| `TODO.md` | Original concise roadmap |
| `AGENTS.md` | Operational guide for future AI coding agents |
| `.gitignore` | Ignores Python, Node, ESP-IDF, logs, datasets, and model artifacts |
| `LICENSE` | Repository license |

## `docs/`

Architecture and planning documentation. Existing lowercase files predate the new agent-oriented docs and contain useful planning context.

| Path | Purpose |
| --- | --- |
| `docs/Architecture.md` | Current architecture summary and implementation status |
| `docs/CurrentStatus.md` | What exists, what is missing, and current priority |
| `docs/Roadmap.md` | Consolidated implementation roadmap |
| `docs/FolderStructure.md` | Folder responsibilities |
| `docs/DesignDecisions.md` | Architectural decisions and rationale |
| `docs/Setup.md` | Setup status and future setup expectations |
| `docs/KnownIssues.md` | Risks, technical debt, TODOs, missing docs |
| `docs/API.md` | Planned API contract |
| `docs/Database.md` | Current no-database status and dataset plan |
| `docs/Hardware.md` | Hardware assumptions and validation checklist |
| `docs/DevelopmentGuide.md` | Contribution conventions and safe workflow |
| `docs/architecture.md` | Existing architecture plan |
| `docs/system-design.md` | Existing system design plan |
| `docs/data-flow.md` | Existing data-flow plan |
| `docs/roadmap.md` | Existing detailed roadmap |

## `shared/`

Planned source of truth for cross-layer contracts. It currently contains placeholder READMEs only.

| Path | Planned purpose |
| --- | --- |
| `shared/protocol/` | UDP packet format, WebSocket envelope, REST contract notes, protocol versions |
| `shared/schemas/` | CSI frame, dashboard state, dataset metadata, calibration metadata schemas |
| `shared/constants/` | Protocol versions, states, message types, default ports, dataset format versions |

## `firmware/`

Planned ESP32 NodeMCU-32 firmware folder. It is currently empty.

Planned responsibilities:

- Connect to Wi-Fi.
- Enable CSI capture.
- Serialize CSI frames into EchoSense packets.
- Send UDP packets to the backend.

## `backend/`

Planned Python backend folder. It currently contains only `backend/simulator/README.md`.

Planned responsibilities:

- Receive UDP packets.
- Replay recorded datasets.
- Run simulator packets.
- Parse and validate CSI.
- Maintain ring buffers and feature summaries.
- Record datasets.
- Serve REST and WebSocket APIs.
- Write structured logs.

## `frontend/`

Planned browser dashboard folder. It is currently empty.

Planned responsibilities:

- Read current state from REST.
- Subscribe to `/ws/live`.
- Show connection, packet, calibration, recording, and room state.
- Eventually render CSI plots and a Three.js room view.

## `datasets/`

Planned storage for versioned CSI sessions. It currently contains a README only.

Planned session shape:

```text
datasets/
  YYYY-MM-DD_HH-MM-SS/
    metadata.json
    raw.csv
    processed.csv
```

## `logs/`

Planned local runtime logs. It currently contains a README only.

Planned files:

- `backend.log`
- `events.log`

## `models/`

Planned storage for future local model files. It is currently empty.

