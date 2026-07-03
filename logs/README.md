# Logs

Runtime logs will be written here during local development.

Planned files:

- `backend.log`: backend health, packet statistics, device connection status, warnings, and errors
- `events.log`: calibration events, recording events, detector state transitions, and operator actions

Backend diagnostics should use structured logging rather than `print()` calls.
