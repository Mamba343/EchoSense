# Roadmap

This roadmap consolidates the existing README, `TODO.md`, and planning documents. It distinguishes current scaffold work from future implementation.

## Phase 0: Documentation and Contracts

Status: in progress.

- [x] Create initial folder scaffold.
- [x] Document architecture, system design, data flow, and roadmap.
- [x] Document clean-room requirements.
- [ ] Define EchoSense-specific UDP packet format.
- [ ] Define shared dashboard states and WebSocket message types.
- [ ] Define dataset metadata schema.
- [ ] Define calibration metadata schema.

## Phase 1: Hardware Validation

Status: not started.

- [ ] Implement minimal ESP32 NodeMCU-32 firmware.
- [ ] Connect ESP32 to Wi-Fi.
- [ ] Enable CSI capture.
- [ ] Serialize CSI frames into EchoSense packets.
- [ ] Send packets by UDP.
- [ ] Implement minimal Python receiver.
- [ ] Log packet rate and receive errors.
- [ ] Parse raw CSI successfully.
- [ ] Record one validation session.

Exit criteria:

- Live CSI frames are captured from the target board.
- Laptop receives and parses packets.
- Packet rate and packet loss are visible.
- A validation dataset exists.

## Phase 2: Replay and Simulator

Status: not started.

- [ ] Implement replay source for versioned dataset folders.
- [ ] Implement simulator scenarios: idle room, small motion, walking, noisy environment.
- [ ] Ensure live, replay, and simulator paths use the same parser and message contracts.

## Phase 3: Backend Pipeline

Status: not started.

- [ ] Add ring buffer.
- [ ] Add feature summaries.
- [ ] Add structured logging.
- [ ] Add recording flow.
- [ ] Add source-mode configuration.

## Phase 4: API Server

Status: not started.

- [ ] Add `GET /api/health`.
- [ ] Add `GET /api/state`.
- [ ] Add `POST /api/calibrate`.
- [ ] Add `POST /api/record/start`.
- [ ] Add `POST /api/record/stop`.
- [ ] Add `/ws/live`.

## Phase 5: Motion and Occupancy

Status: not started.

- [ ] Add empty-room calibration.
- [ ] Add amplitude-first motion energy.
- [ ] Add room-level occupancy state.
- [ ] Evaluate against recorded sessions.

## Phase 6: Dashboard

Status: not started.

- [ ] Build browser frontend.
- [ ] Connect to REST and `/ws/live`.
- [ ] Show source mode, packet rate, packet loss, calibration, recording, and state.
- [ ] Plot CSI summaries and motion energy.
- [ ] Add Three.js room visualization after data path is stable.

## Phase 7: Future Expansion

Status: future only.

- [ ] Local ML model integration.
- [ ] Multiple ESP32 support.
- [ ] MQTT/Home Assistant integration.
- [ ] Desktop packaging.
- [ ] Optional local database if long-running monitoring needs it.

## Explicitly Out of Scope

- Heart rate.
- Respiration.
- DensePose.
- Skeleton estimation.
- Person identification.
- Multi-person localization.
- Cloud inference as a requirement.

