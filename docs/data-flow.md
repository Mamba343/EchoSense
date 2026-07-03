# EchoSense Data Flow

EchoSense has two production-relevant data paths and one development-only path.

- Live mode: ESP32 to UDP to backend to dashboard.
- Replay mode: recorded dataset to backend to dashboard.
- Simulator mode: synthetic packets to backend to dashboard for development only.

The frontend contract must remain the same for all paths: REST control endpoints plus `/ws/live` messages using `{ "type": "...", "payload": { ... } }`.

## Live Mode

```mermaid
sequenceDiagram
    participant Router as Wi-Fi Router
    participant ESP32 as ESP32 NodeMCU-32
    participant UDP as Backend Live Source
    participant Parser as Packet Parser
    participant Buffer as Ring Buffer
    participant Pipe as Processing Pipeline
    participant State as Dashboard State Machine
    participant API as REST + /ws/live
    participant UI as Dashboard

    Router-->>ESP32: Wi-Fi packets in RF environment
    ESP32->>ESP32: CSI callback captures raw CSI
    ESP32->>UDP: EchoSense UDP packet
    UDP->>Parser: packet bytes + arrival metadata
    Parser->>Buffer: validated CSI frame
    Buffer->>Pipe: rolling frame window
    Pipe->>State: summaries and state signals
    State->>API: csi_summary, device_health, occupancy_state
    API->>UI: /ws/live message envelope
    UI->>API: REST actions
```

## Replay Mode

```mermaid
sequenceDiagram
    participant Data as Dataset Session
    participant Replay as Backend Replay Source
    participant Parser as Frame Loader
    participant Buffer as Ring Buffer
    participant Pipe as Processing Pipeline
    participant State as Dashboard State Machine
    participant API as REST + /ws/live
    participant UI as Dashboard

    Data->>Replay: metadata.json + raw/processed files
    Replay->>Parser: recorded frames in timestamp order
    Parser->>Buffer: replayed CSI frame
    Buffer->>Pipe: rolling frame window
    Pipe->>State: summaries and state signals
    State->>API: same message types as live mode
    API->>UI: /ws/live message envelope
```

Replay mode should preserve timing by default, with a future option for faster-than-real-time testing. It must use the same message schema as live mode.

## Simulator Mode

The simulator is a development source that creates synthetic packets matching the shared EchoSense protocol.

```mermaid
flowchart LR
    Scenario["idle / small motion / walking / noisy"] --> Sim["backend/simulator"]
    Sim -->|"EchoSense packet format"| Parser["Backend parser"]
    Parser --> Pipeline["same backend pipeline"]
    Pipeline --> UI["same dashboard contract"]
```

Simulator output must be useful for backend and UI development, but it should never be used as evidence for real sensing claims.

## Unified Backend Pipeline

```mermaid
flowchart TB
    Live["Live UDP source"] --> Source["Unified source interface"]
    Replay["Replay source"] --> Source
    Simulator["Simulator source"] --> Source
    Source --> Validate["Validate protocol version\nlengths and payload caps"]
    Validate --> Parse["Parse CSI frame"]
    Parse --> Buffer["Ring buffer"]
    Buffer --> Record["Optional session recorder"]
    Buffer --> Features["Feature summaries"]
    Features --> State["Dashboard state machine"]
    State --> WS["/ws/live"]
    State --> REST["/api/state"]
```

## Hardware Validation Flow

Before motion or occupancy work begins, EchoSense must prove the hardware path:

1. ESP32 NodeMCU-32 captures CSI.
2. ESP32 transmits UDP packets.
3. Laptop receives packets.
4. Packet rate is stable enough for analysis.
5. Raw CSI is parsed successfully.

Only after this milestone should the project proceed to motion detection, occupancy detection, or polished dashboard work.

## Dashboard State Flow

```mermaid
stateDiagram-v2
    [*] --> UNKNOWN
    UNKNOWN --> CONNECTING
    CONNECTING --> CALIBRATING
    CONNECTING --> DISCONNECTED
    CALIBRATING --> EMPTY
    EMPTY --> PRESENT
    EMPTY --> MOTION
    PRESENT --> MOTION
    MOTION --> PRESENT
    PRESENT --> EMPTY
    MOTION --> EMPTY
    CONNECTING --> ERROR
    CALIBRATING --> ERROR
    EMPTY --> ERROR
    PRESENT --> ERROR
    MOTION --> ERROR
    ERROR --> CONNECTING
    DISCONNECTED --> CONNECTING
```

State definitions:

| State | Meaning |
| --- | --- |
| `UNKNOWN` | Backend has not established a source or replay session |
| `CONNECTING` | Backend is waiting for live packets or preparing replay/simulator source |
| `CALIBRATING` | Baseline calibration is running |
| `EMPTY` | Signal resembles calibrated empty-room baseline |
| `PRESENT` | Signal suggests presence without active motion |
| `MOTION` | Motion energy is above threshold |
| `ERROR` | Backend, parser, source, or calibration error |
| `DISCONNECTED` | Live source stopped receiving packets |

## WebSocket Message Flow

All live dashboard messages use one envelope:

```json
{
  "type": "device_health",
  "payload": {
    "source": "live",
    "packet_rate_hz": 20.0,
    "packet_loss": 0,
    "last_seen_ms": 120
  }
}
```

Initial message types:

- `csi_summary`
- `motion_state`
- `device_health`
- `occupancy_state`
- `calibration_status`

The backend should decimate raw CSI before sending it to the browser. The browser should receive UI-friendly summaries unless a debug mode is explicitly enabled.

## Recording Flow

```mermaid
flowchart LR
    Meta["metadata.json"] --> Session["datasets/YYYY-MM-DD_HH-MM-SS/"]
    Raw["raw.csv"] --> Session
    Processed["processed.csv"] --> Session
    Session --> Replay["Replay mode"]
```

Dataset recording should be available as soon as packet parsing works. This makes later algorithm changes testable without requiring the ESP32 every time.

## Logging Flow

```mermaid
flowchart LR
    Backend["Backend modules"] --> LogRouter["Structured logger"]
    LogRouter --> BackendLog["logs/backend.log"]
    LogRouter --> EventsLog["logs/events.log"]
```

Suggested logged events:

- backend startup and shutdown,
- source mode selected,
- device connected/disconnected,
- packet rate changes,
- packet parse errors,
- calibration start/finish/failure,
- recording start/stop,
- dashboard state transitions,
- warnings and errors.
