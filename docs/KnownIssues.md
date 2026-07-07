# Known Issues

This project is intentionally incomplete. The items below are gaps and risks visible from the current repository.

## Implementation Gaps

- No firmware implementation.
- No backend implementation.
- No API implementation.
- No simulator implementation.
- No replay implementation.
- No frontend implementation.
- No shared packet/schema/constant files.
- No tests.
- No CI.
- No package manifests.
- No real datasets.
- No structured log output yet.

## Technical Debt

- Existing docs are mostly aspirational and lowercase names duplicate some of the new agent-facing docs.
- `shared/` is only a concept until concrete protocol/schema/constant files exist.
- The planned API is described in README/docs but has no executable contract tests.
- Dataset format is described but not formalized as a schema.
- Hardware validation criteria are documented but no measurement results exist.
- `.gitignore` ignores all dataset contents except `datasets/README.md`; this is probably correct for raw recordings, but sample metadata policy is not documented yet.

## Risks

| Risk | Impact | Mitigation |
| --- | --- | --- |
| NodeMCU-32 CSI capture is unstable | Blocks project | Complete Hardware Validation before feature work |
| UDP packets are malformed or lost | Bad parsing and charts | Add sequence numbers, length validation, packet-rate logs |
| Simulator diverges from firmware | False confidence | Generate simulator packets from the shared protocol |
| Dashboard overstates capability | Misleading UX | Use room-level states only |
| Single-node sensing is ambiguous | Detection errors | Collect labeled datasets and expose uncertainty |
| Router or room changes break calibration | False positives | Make calibration visible and repeatable |

## Missing Documentation

- Exact UDP packet layout.
- Field units and data types for CSI frames.
- WebSocket payload schemas.
- REST response schemas.
- Firmware build and flash instructions.
- Backend environment setup.
- Frontend environment setup.
- Dataset labeling guide.
- Calibration procedure.
- Troubleshooting guide.

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

