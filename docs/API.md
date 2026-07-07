# API

No API is implemented yet. This page documents the planned v1 API from the existing README and planning docs.

## Planned REST Endpoints

| Method | Path | Planned purpose | Status |
| --- | --- | --- | --- |
| `GET` | `/api/health` | Backend health, version, uptime, logging status | Not implemented |
| `GET` | `/api/state` | Current dashboard state, source mode, device stats, recording status | Not implemented |
| `POST` | `/api/calibrate` | Start calibration | Not implemented |
| `POST` | `/api/record/start` | Start recording a dataset session | Not implemented |
| `POST` | `/api/record/stop` | Stop active recording | Not implemented |

## Planned WebSocket Endpoint

| Path | Planned purpose | Status |
| --- | --- | --- |
| `/ws/live` | Stream live, replay, or simulator state messages to the dashboard | Not implemented |

Every planned WebSocket message uses this envelope:

```json
{
  "type": "...",
  "payload": {}
}
```

## Planned Message Types

| Type | Planned purpose | Status |
| --- | --- | --- |
| `csi_summary` | UI-friendly CSI summaries | Not implemented |
| `motion_state` | Motion feature state and confidence | Not implemented |
| `device_health` | Packet rate, packet loss, RSSI, source mode, last seen | Not implemented |
| `occupancy_state` | Room-level occupancy state and confidence | Not implemented |
| `calibration_status` | Calibration progress, quality, completion/failure | Not implemented |

## Planned Dashboard States

- `UNKNOWN`
- `CONNECTING`
- `CALIBRATING`
- `EMPTY`
- `PRESENT`
- `MOTION`
- `ERROR`
- `DISCONNECTED`

## Security Notes for Future Implementation

- Bind to `127.0.0.1` by default.
- Require explicit configuration before LAN exposure.
- Treat UDP packets as untrusted input.
- Validate packet lengths before parsing payloads.
- Cap packet and WebSocket message sizes.
- Keep CORS narrow during development.

## Open Contract Work

- Define response schemas.
- Define error response shape.
- Define WebSocket payload schemas.
- Define packet schema and conversion from raw CSI to UI summaries.
- Add contract tests after API code exists.

