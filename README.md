# ESPsun ☀️

**Local, open-source solar telemetry with ESP32 and MQTT.**

ESPsun is an experimental project to replace a dedicated single-board computer used for solar telemetry with a small, energy-efficient ESP32 device. The target is a **Seeed Studio XIAO ESP32-S3 Sense**.

The initial hardware targets are:
- A MUST PV3300 TLV-family inverter, connected through its **USB-B** port (CH340 USB-to-serial bridge).
- A PACE/PYLON-compatible lithium battery BMS, connected by **wired RS485** using a **3.3 V transceiver**.
- A local MQTT broker and Home Assistant.

> **Status: experimental / work in progress.** No firmware or wiring design in this repository is presently claimed to be tested on the listed equipment. The example configuration is intentionally inverter-only until its USB host power arrangement, serial settings, and telemetry are validated.

## Planned architecture

```text
                         USB-B (CH340, Modbus RTU)
MUST-compatible inverter -----------------------------+
                                                      |
                                                      v
                                               ESP32-S3 XIAO
                                                      |
PACE/PYLON battery BMS -- RS485 -- 3.3 V transceiver -+
                                                      |
                                                   Wi-Fi
                                                      |
                                                MQTT broker
                                                      |
                                               Home Assistant
```

Only one device uses USB Host; RS485 is attached to the ESP32's hardware UART. The inverter's CAN and USB-A ports are not used by ESPsun.

## Documentation

- [Architecture and project milestones](docs/architecture.md)
- [MUST inverter protocol and generic register map](docs/inverter-modbus.md)
- [Hardware and USB Host safety notes](docs/hardware-safety.md)
- [Privacy and responsible disclosure](SECURITY.md)
- [Inverter-only ESPHome example (experimental)](esphome/espsun-inverter.example.yaml)
- [Placeholder secrets file](esphome/secrets.example.yaml)

## Design principles

- **Local-first**: no vendor cloud dependency.
- **Read-only**: no control or write operations to the inverter or BMS in the initial versions.
- **Power-conscious**: small MCU instead of a dedicated computer.
- **Privacy by default**: no private installation data, network identifiers, serial numbers, credentials, or production traces in source control.
- **Reproducible**: protocols, wiring assumptions and firmware limitations documented clearly.

## Roadmap

- [x] Identify the inverter's USB-to-serial chip and Modbus RTU transport.
- [x] Match generic inverter register blocks against a publicly documented protocol.
- [ ] Verify inverter serial parameters and USB Host VBUS electrical design.
- [ ] Compile and test the inverter-only ESPHome configuration.
- [ ] Add PACE/PYLON RS485 BMS support on a separate hardware UART.
- [ ] Validate MQTT telemetry, availability and Home Assistant discovery.
- [ ] Compare functionality with the incumbent telemetry system before switching.

## Contributing

Compatibility reports, hardware suggestions and pull requests are welcome. Please read [SECURITY.md](SECURITY.md) before attaching diagnostic traces or logs. Never post secrets or unredacted installation information.

**License:** not selected yet. Until a license is added, please do not assume permission to redistribute or reuse this repository's contents beyond what copyright law allows.
