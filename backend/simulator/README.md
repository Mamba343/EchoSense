# Backend Simulator

The simulator will generate synthetic CSI packets that match the EchoSense protocol.

It is for development only. It should help test the backend and dashboard before hardware is available or when replay data is not enough.

Planned scenarios:

- idle room
- small motion
- walking
- noisy environment

The simulator must use the same packet format as the firmware. It should not define a second protocol.
