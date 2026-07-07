# Current Status

EchoSense is in the architecture and scaffold phase.

## What Exists

- Project README with vision, scope, planned API, dashboard states, and current priority.
- Planning docs under `docs/`:
  - `architecture.md`
  - `system-design.md`
  - `data-flow.md`
  - `roadmap.md`
- Placeholder folders:
  - `backend/simulator/`
  - `datasets/`
  - `logs/`
  - `shared/`
  - `firmware/`
  - `frontend/`
  - `models/`
- `TODO.md` with a four-phase task list.
- `.gitignore` covering Python, Node.js, ESP-IDF, logs, datasets, and model artifacts.

## What Does Not Exist Yet

- No firmware source.
- No backend application.
- No API server.
- No WebSocket server.
- No packet parser.
- No simulator implementation.
- No frontend app.
- No database or database schema.
- No dataset recordings.
- No model files.
- No package manifests such as `requirements.txt`, `pyproject.toml`, `package.json`, or ESP-IDF project files.
- No tests or CI configuration.

## Current Priority

The documented priority is Hardware Validation:

1. ESP32 NodeMCU-32 captures CSI.
2. UDP packets are transmitted.
3. Laptop receives packets.
4. Packet rate is stable.
5. Raw CSI is parsed successfully.

Motion detection, occupancy detection, ML, and dashboard polish should wait until this milestone is complete.

## TODOs Already Present

From `TODO.md`:

- Architecture.
- ESP32 CSI firmware.
- UDP streaming.
- Python receiver.
- FastAPI.
- WebSockets.
- Dashboard.
- Three.js room.
- Motion detection.
- Occupancy detection.
- Dataset recording.
- ML integration.
- Multiple ESP32 support.

## Missing Documentation

- Exact UDP packet format.
- Dataset metadata schema.
- WebSocket payload schemas.
- REST request/response examples.
- Backend run instructions.
- Firmware flashing instructions.
- Hardware validation checklist with real measured results.
- Development environment setup once runtime dependencies exist.

