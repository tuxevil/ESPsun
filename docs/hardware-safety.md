# Hardware safety — XIAO ESP32-S3 USB Host + RS485

**No wiring schematic has been verified yet.** The information here is a design checklist, not permission to connect wires to live power equipment.

## USB Host to the inverter

- Use the XIAO ESP32-S3's native USB OTG/Host interface for the inverter's USB-B connection.
- A USB host must provide the peripheral with the required 5 V **VBUS** supply. Confirm the specific XIAO board revision's schematics and required power path. Do not assume its USB-C receptacle sources VBUS when externally powered.
- Provide current-limited 5 V VBUS with correct isolation and **no backfeeding** into the computer, inverter, USB-C programmer, or unrelated supplies.
- The USB-C OTG port is shared with flashing/debugging; plan an initial programming phase and subsequent OTA, or separate debug UART as appropriate.
- Never connect USB cabling to live power terminals.

## Wired RS485 to battery BMS

- Use a **3.3 V logic compatible** RS485 transceiver, e.g. MAX3485/SP3485 (module pin compatibility still to be verified).
- Never connect RS485 A/B directly to MCU GPIO.
- Verify BMS connector pinout, A/B polarity, reference ground, termination, bias resistors and transceiver power rails from the device documentation.
- Prefer galvanic isolation when grounding and equipment potentials are uncertain. Never tie battery pack positive or negative directly to a logic supply in lieu of a verified reference.
- The BMS-to-inverter CAN connection is independent and must remain intact.
- Do not poll the same RS485 BMS from two masters during migration.

## Firmware safety

- Implement **Modbus function 0x03 only** for inverter measurements initially.
- Avoid exposing power-control functions, write registers, and maintenance commands.
- Fail safely on stale data: represent device availability explicitly and do not publish a stale measurement as a verified current value.
- Keep OTA authentication credentials in an ignored secrets file.

## References

- Seeed XIAO ESP32-S3 documentation: https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/
- ESPHome USB host: https://esphome.io/components/usb_host/
- ESPHome USB UART: https://esphome.io/components/usb_uart/
