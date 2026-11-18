# ESPsun architecture

**Status:** proposal, not hardware-validated (2026-11-18).

## Goal

Read an inverter and battery BMS using wired connections and publish the resulting measurements to a local MQTT broker for Home Assistant.

\`\`\`text
USB-B inverter ── CH340 USB serial ── USB Host (ESP32-S3)
                                             │
Battery BMS ── RS485 ── 3.3 V transceiver ─ UART
                                             │
                                         Wi-Fi/MQTT
                                             │
                                         MQTT broker
                                             │
                                       Home Assistant
\`\`\`

The ESP32-S3's native USB controller is used as **USB Host for the inverter**; a distinct hardware UART reads the **battery BMS through RS485**. Inverter CAN communication and the Wi-Fi-dongle port are out of scope.

## Selected technologies

| Layer | Initial candidate | Status |
|---|---|---|
| MCU | Seeed Studio XIAO ESP32-S3 Sense | Selected target, not tested |
| Inverter transport | USB Host → CH340 USB UART | Device class and VID/PID known |
| Inverter application protocol | Modbus RTU slave 0x04, read holding registers | Observed and cross-checked against public register map |
| Inverter serial baud/parity | TBD; example uses provisional 19200 8N1 | **Not yet measured** for this model |
| Battery | PACE/PYLON-family ASCII over wired RS485 | Integration pending; confirm variant physically |
| Firmware | ESPHome ESP-IDF + USB UART + Modbus Controller | Candidate, example only |
| Telemetry | MQTT with Home Assistant discovery | Candidate, not end-to-end tested |

## Milestones

1. **Safe foundation:** verify XIAO revision, USB-C role, USB Host VBUS 5 V supply, backfeed protection and power budget.
2. **Inverter-only prototype:** confirm baud, parity and stop bits; read a few public measurement registers using Modbus function 0x03.
3. **MQTT validation:** ensure correct units, fault handling, reconnects, entity availability and discovery; compare with an independent measurement source.
4. **Battery:** connect UART through a 3.3 V RS485 transceiver and implement PACE/PYLON read-only protocol; don't share the same RS485 link with another polling master.
5. **Consolidation:** robust scheduling, OTA, watchdog, Wi-Fi reconnects and monitoring.
6. **Migration:** confirm compatible or explicitly migrated Home Assistant entity IDs and complete at least one meaningful stability observation window before retiring existing telemetry equipment.

## Non-goals for the first release

- Modifying charge limits or inverter settings.
- Replacing physical BMS protections.
- Remote cloud control.
- Accessing the inverter CAN port or USB-A Wi-Fi accessory.
- Copying historical measurements or local infrastructure into this public repository.

## Public references

- https://esphome.io/components/usb_host/
- https://esphome.io/components/usb_uart/
- https://esphome.io/components/modbus/
- https://esphome.io/components/modbus_controller/
- https://esphome.io/components/mqtt/
- https://wiki.seeedstudio.com/XIAO_ESP32S3_esphome/
