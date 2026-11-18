# Security and privacy policy

ESPsun targets energy equipment. Treat diagnostic material and configuration files as potentially sensitive even if an inverter or BMS protocol is publicly documented.

## Never include in public issues, commits, pull requests, or screenshots

- Wi-Fi SSIDs/passwords, MQTT usernames/passwords, OTA credentials or API keys.
- Public or private IP addresses of a real installation, hostnames, VPN names or DNS records.
- Device serial numbers, MAC addresses, Bluetooth IDs, hardware-specific tokens or license keys.
- Unredacted USB/RS485 traffic dumps, firmware logs, BMS diagnostics, production measurements or historical telemetry.
- Photos that reveal identifiable installation location, account details, or equipment serial labels.
- Personal stories, private correspondence, precise geographic location, or private project notes.

**Generic protocol bytes** (Modbus function, public register addresses, VID/PID) are acceptable when not tied to a real installation. Replace actual measurements in examples with clearly labeled synthetic values.

## Private diagnostics

Keep raw captures and installation details outside the repository, in an appropriately controlled private workspace. Before reporting a bug, extract only the minimum protocol fields needed and manually redact hardware identifiers and readings. A \`.gitignore\` entry is defense-in-depth, **not a data-loss-prevention guarantee**: \`git add -f\` and copied text can bypass it.

## Device safety

- Read-only telemetry in the initial versions. Do not introduce register writes, settings changes, relay controls or charging commands.
- Do not bridge 48 V battery wiring to ESP32 GPIO.
- Verify USB Host VBUS circuitry and power-source isolation before connecting live inverter USB equipment.
- Use a properly rated 3.3 V UART/RS485 transceiver, with electrical isolation if grounds differ.
- Do not interrupt the BMS-to-inverter CAN link.
- Do not run two masters on the same RS485 connection during validation.

## Reporting a security issue

Avoid publishing sensitive proof-of-concept details in a public GitHub issue. Contact the repository owner privately through an appropriate channel before sharing non-public vulnerabilities or diagnostics.
