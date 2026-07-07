# Hardware

The planned v1 hardware is one ESP32 NodeMCU-32, one Wi-Fi router, one laptop, and one browser.

## Components

| Component | Role | Current status |
| --- | --- | --- |
| ESP32 NodeMCU-32 | Capture Wi-Fi CSI and transmit UDP packets | Firmware not implemented |
| Wi-Fi router | Provides local network and RF environment | Not configured in repo |
| Laptop | Runs planned Python backend and browser dashboard | Backend not implemented |
| Browser | Displays planned dashboard | Frontend not implemented |

## Hardware Validation Milestone

The first implementation milestone is Hardware Validation:

1. ESP32 NodeMCU-32 captures CSI.
2. ESP32 transmits UDP packets.
3. Laptop receives packets.
4. Packet rate is stable.
5. Raw CSI is parsed successfully.

No motion detection, occupancy detection, ML, or dashboard polish should begin before this succeeds.

## Planned Firmware Responsibilities

- Connect to Wi-Fi.
- Enable CSI capture.
- Serialize CSI frames into EchoSense-specific UDP packets.
- Include sequence and timestamp data sufficient to detect packet loss and ordering.
- Send packets to the backend.

## Planned Backend Hardware Responsibilities

- Listen for UDP packets.
- Validate packet version and length.
- Track packet rate, packet loss, and last-seen time.
- Log hardware connection and parsing events.
- Record validation sessions.

## Known Hardware Risks

- NodeMCU-32 CSI capture may be unstable or environment-dependent.
- Single-node CSI may not reliably distinguish all room states.
- Router channel, room layout, and interference may affect calibration.
- Phase data may be noisy and should be treated as experimental until measured.

## Required Future Documentation

- Exact board model and revision.
- Firmware toolchain.
- Router configuration.
- Wi-Fi channel used during validation.
- Backend UDP host and port.
- Measured packet rate.
- Packet loss behavior.
- Example validation dataset.

