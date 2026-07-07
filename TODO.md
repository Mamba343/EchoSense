# EchoSense Roadmap

EchoSense should advance in small, testable phases. The first real milestone is Hardware Validation: proving the ESP32 NodeMCU-32 can capture CSI, transmit UDP packets, and produce data the laptop can parse.

## Phase 0: Architecture and Clean-Room Foundation

- [x] Create initial project folders.
- [x] Review RuView architecture for inspiration only.
- [x] Document EchoSense architecture, system design, data flow, and roadmap.
- [x] Approve architecture direction.
- [x] Add shared module plan.
- [x] Add replay mode, simulator, structured logging, dataset organization, dashboard state machine, and simplified v1 networking.
- [ ] Define EchoSense-specific packet format, schemas, constants, and message types in `shared/`.

Exit criteria:

- Clean-room requirement is documented.
- Scope exclusions are clear: no vitals, DensePose, skeletons, multi-person localization, identity tracking, or copied implementation.
- Shared contracts are defined before runtime modules depend on them.

## Phase 1: Hardware Validation

Success criteria:

- ESP32 NodeMCU-32 successfully captures CSI.
- UDP packets are transmitted.
- Laptop receives packets.
- Packet rate is stable.
- Raw CSI is parsed successfully.

Tasks:

- [ ] Define minimal EchoSense UDP packet contract in `shared/protocol/`.
- [ ] Create minimal firmware only for Wi-Fi connection, CSI callback, packet serialization, and UDP send.
- [ ] Create minimal backend receiver only for packet reception, validation, structured logging, and packet-rate reporting.
- [ ] Confirm packet parsing on the laptop.
- [ ] Record one raw validation session in the versioned dataset format.
- [ ] Document board, router channel, sampling rate, firmware version, backend version, and calibration status.

Exit criteria:

- Live CSI frames are captured and parsed from the target NodeMCU-32.
- Packet rate and packet loss are visible in structured logs.
- Dataset replay can load the validation session.
- No motion or occupancy feature work begins before this phase is complete.

## Phase 2: Shared Contracts, Replay, and Simulator

- [ ] Add shared constants for dashboard states and WebSocket message types.
- [ ] Add schema documentation for CSI frames, dataset metadata, calibration metadata, and `/ws/live` messages.
- [ ] Implement replay source against versioned session folders.
- [ ] Implement development simulator scenarios: idle room, small motion, walking, noisy environment.
- [ ] Ensure live, replay, and simulator sources use the same parser and pipeline interface.

Exit criteria:

- Frontend-facing messages are identical for live, replay, and simulator modes.
- Simulator packets match the firmware packet format.
- Replay mode can drive `/ws/live` without hardware.

## Phase 3: Python Backend Data Pipeline

- [ ] Add ring buffer.
- [ ] Convert raw I/Q values to amplitude summaries.
- [ ] Treat phase features as experimental until measured.
- [ ] Add structured logging to backend modules.
- [ ] Add versioned session recording: `metadata.json`, `raw.csv`, `processed.csv`.
- [ ] Add configuration for source mode, ports, recording path, and log path.

Exit criteria:

- Backend is one Python application with clear internal modules.
- No runtime diagnostics use `print()`.
- Recording does not block packet ingestion.
- Recorded sessions replay through the same pipeline.

## Phase 4: Simplified API Server

- [ ] Add `GET /api/health`.
- [ ] Add `GET /api/state`.
- [ ] Add `POST /api/calibrate`.
- [ ] Add `POST /api/record/start`.
- [ ] Add `POST /api/record/stop`.
- [ ] Add one WebSocket endpoint: `/ws/live`.
- [ ] Use the envelope `{ "type": "...", "payload": { ... } }` for every WebSocket message.

Exit criteria:

- Dashboard can connect to one WebSocket stream.
- REST covers health, state, calibration, and recording.
- Backend binds to `127.0.0.1` by default.

## Phase 5: Motion and Occupancy Detection

- [ ] Add empty-room calibration.
- [ ] Implement amplitude-first motion energy.
- [ ] Implement dashboard state transitions: `UNKNOWN`, `CONNECTING`, `CALIBRATING`, `EMPTY`, `PRESENT`, `MOTION`, `ERROR`, `DISCONNECTED`.
- [ ] Implement occupancy confidence and quality indicators.
- [ ] Evaluate against recorded sessions.
- [ ] Tune thresholds per room.

Exit criteria:

- EchoSense distinguishes empty, motion, and likely present states in controlled sessions.
- State transitions are logged in `events.log`.
- The UI/API exposes uncertainty instead of pretending all detections are final.

## Phase 6: Dashboard

- [ ] Build frontend around `/api/state` and `/ws/live`.
- [ ] Render dashboard from explicit states, not simple booleans.
- [ ] Show connection status, packet rate, packet loss, calibration state, and source mode.
- [ ] Plot live CSI summaries and motion energy.
- [ ] Add controls for calibration and recording.
- [ ] Add Three.js room visualization after the data path is stable.

Exit criteria:

- Live mode and replay mode look the same to the frontend.
- Dashboard handles errors and disconnects gracefully.
- Three.js visualization communicates room-level signal state without implying pose, identity, or precise location.

## Phase 7: Dataset and Evaluation

- [ ] Expand dataset metadata schema.
- [ ] Add manual labels: empty, small motion, walking, noisy environment, present.
- [ ] Add replay-based comparison workflow.
- [ ] Track false positives and false negatives.
- [ ] Publish a privacy-safe sample dataset if appropriate.

Exit criteria:

- Detector changes can be compared against the same sessions.
- EchoSense has evidence for motion and occupancy claims.

## Phase 8: Future Expansion

Potential future work after v1 is stable:

- Multiple ESP32 nodes with synchronization.
- Local ML model trained on EchoSense datasets.
- MQTT/Home Assistant occupancy integration.
- Better ESP32 provisioning tooling.
- Lightweight local database for long-running monitoring.
- Cross-platform desktop packaging.
- Optional model export for edge experimentation.

Out-of-scope unless explicitly reapproved:

- Heart rate.
- Respiration.
- DensePose.
- Human skeleton estimation.
- Identity or person re-identification.
- Multi-person localization.
- Cloud inference as a requirement.

## Risk Register

| Risk | Probability | Impact | Mitigation |
| --- | --- | --- | --- |
| NodeMCU-32 CSI capture is unstable | Medium | High | Hardware Validation milestone before feature work |
| NodeMCU-32 is underpowered for edge logic | High | Medium | Keep firmware limited to capture and UDP |
| Phase data is noisy | High | Medium | Use amplitude-first features, mark phase experimental |
| Single-node detection is ambiguous | High | Medium | Present room-level states only |
| Dashboard overstates capability | Medium | High | Use signal/room visuals, not body or skeleton visuals |
| Dataset quality is weak | Medium | High | Record validation sessions early and require metadata |
| Router/environment changes break baselines | High | Medium | Make recalibration fast, visible, and logged |
| UDP packet loss affects charts | Medium | Low | Track loss, decimate UI streams, use rolling summaries |
| API surface grows too early | Medium | Medium | Keep v1 to five REST endpoints and one WebSocket |

## Approval Gate

No feature implementation should begin until these architecture updates are approved.
