# MUST-compatible inverter Modbus RTU reference

This is a **public, generic protocol reference**, not a log or measurement from a particular installation.

## Transport

An inverter in the PV3300 TLV family can expose a CH340-style USB-to-UART adapter over USB-B. The adapter class is recognized by ESPHome USB UART as:

- Type: \`ch340\`
- USB VID: \`0x1A86\`
- USB PID: \`0x7523\`

The serial format **must be confirmed for each hardware/firmware revision**. An ESPHome example currently uses *provisional* \`19200\` baud and \`8N1\` based on related MUST models. Those values are **not a verified specification** for the PV3300 TLV.

## Modbus RTU

Observed structure is standard Modbus RTU **slave address 0x04**, function **0x03** (read holding registers). This use of a USB-UART bridge does **not** imply the inverter has a physical RS485 connector.

**Read-only** polling blocks associated with a public MUST register map:

| Register range | Registers | Function |
|---|---:|---|
| \`0x3B61\`–\`0x3B75\` | 21 | PV charger status and measurements |
| \`0x6271\`–\`0x62BF\` | 79 | Inverter status and measurements |
| \`0x4E20\`–\`0x4E30\` | 17 | Inverter identification and revision information — **avoid publishing raw identity results** |

Generic interpreted examples of register *definitions* (not actual readings):

| Register | Description from community map | Units/scale |
|---|---|---|
| \`0x3B61\` | Solar charger state | Enum |
| \`0x3B65\` | PV voltage | 0.1 V |
| \`0x3B66\` | Battery voltage (charger-side) | 0.1 V |
| \`0x3B67\` | PV charging current | 0.1 A |
| \`0x3B68\` | Charger power | 1 W |
| \`0x3B69\` | Charger heatsink temperature | 1 °C |
| \`0x6271\` | Inverter state | Enum |
| \`0x6276\` | Inverter output voltage | 0.1 V |
| \`0x627D\` | Inverter active power | 1 W |

**Caution:** The public map was originally documented for other MUST PV/PH/EP families. Some meanings, AC phase conventions and scaling may differ for split-phase or newer devices. Cross-check values against the actual device UI before relying on them.

## Source

- MUST protocol 1.4.15 community transcription: https://github.com/ZatolokinPavel/esp32-lifepo4-manager/blob/main/protocols/MUST%20RS485%20Modbus%20RTU%20communication%20protocol%201.4.15.md
- ESPHome: https://esphome.io/components/usb_uart/
- ESPHome Modbus Controller: https://esphome.io/components/modbus_controller/

**Do not commit actual USB packets, model serial fields, device IDs or telemetry samples from a private installation.**
