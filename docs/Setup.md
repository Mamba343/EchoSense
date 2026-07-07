# Setup

EchoSense does not currently have a runnable firmware, backend, frontend, or simulator. There are no package manifests or build scripts yet.

## Current Setup

Clone the repository and read the docs:

```powershell
git clone <repo-url>
cd EchoSense
```

There is no current command to start the backend or frontend.

## Expected Future Backend Setup

The existing docs indicate a future Python backend. When implemented, it should include a dependency manifest such as `requirements.txt` or `pyproject.toml`, plus run commands for:

- live UDP receiver mode,
- replay mode,
- simulator mode,
- API server,
- structured logging.

## Expected Future Firmware Setup

The existing docs indicate future ESP32 NodeMCU-32 firmware. When implemented, it should document:

- ESP-IDF or Arduino toolchain choice,
- board target,
- Wi-Fi configuration handling,
- flash command,
- monitor command,
- backend host and UDP port configuration,
- CSI enablement steps.

## Expected Future Frontend Setup

The existing docs indicate a future browser dashboard and Three.js view. When implemented, it should document:

- package manager,
- install command,
- local dev command,
- build command,
- backend API base URL configuration.

## Build and Deployment

No build or deployment process exists yet.

Planned local deployment shape:

1. Flash ESP32 firmware.
2. Start Python backend on the laptop.
3. Open browser dashboard locally.
4. Record datasets under `datasets/`.
5. Write runtime logs under `logs/`.

## Configuration

No configuration files exist yet.

Expected future configuration areas:

- backend bind host and port,
- UDP listen port,
- source mode: live, replay, or simulator,
- dataset path,
- log path,
- firmware destination host and port,
- Wi-Fi credentials handled outside committed files.

