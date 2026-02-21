# ECU Emulator (RP2040 Zero) — EN

ECU emulator for testing/demonstrating diagnostic scanners over CAN and K-Line.

## Hardware

- MCU: RP2040 Zero
- Display: OLED 128x64 I2C (SDA=GPIO4, SCL=GPIO5)
- Buttons: UP=GPIO14, DOWN=GPIO15, SELECT=GPIO26
- Status LED: WS2812 on GPIO16 (optional)
- CAN transceiver VP230: TX=GPIO6, RX=GPIO7, RS=GPIO8
- K-Line (UART0/Serial1): TX=GPIO0, RX=GPIO1 (9637)

## Supported protocols

- CAN 11bit 250k (CAN11250)
- CAN 11bit 500k (CAN500K)
- CAN 11bit 500k (alternate) — CAN11500
- CAN 29bit 250k (CAN29250)
- CAN 29bit 500k (CAN29500)
- J1939 250k (J1939250)
- J1939 500k (J1939500)
- ISO9141-2 (5 baud init)
- KWP2000 (5 baud init) — KWP5BAUD
- KWP2000 (fast init) — KWPFAST

## Diagnostic capabilities (OBD-II)

For CAN/J1939 modules 1/5/6/9/10:

- Mode 01: current data (PID masks aligned with CAN500K)
- Mode 02: freeze frame
- Mode 03/07/0A: DTCs
- Mode 04: clear DTCs
- Mode 05: O2/λ
- Mode 06: on-board monitoring (ISO-TP multi-frame)
- Mode 08: control (echo, mask support)
- Mode 09: VIN/CALID/CVN/ECU Name/ESN/Emission family (ISO-TP multi-frame)
- Mode 22/3E: as implemented in CAN500K

## UI and indicators

- OLED: ELM327 connection state, protocol name, ECU name, selected module number.
- WS2812: blinks on activity, green with traffic, blue without; off when protocol inactive.
- Buttons:
  - UP/DOWN: browse protocols (stops current when switching)
  - SELECT: start/stop selected protocol (start after 1s delay)

## Power and links

- CAN bitrate chosen by module (250k/500k, etc.). Initial bus: 250k on RX=7, TX=6.
- K-Line: 10.4 kbaud, UART0 (Serial1) on pins 0/1.

## Build & flash

- Platform: Arduino for RP2040 (board package 5.5.0), Earle Philhower core.
- Libraries: Adafruit_SSD1306, Adafruit_GFX, Adafruit_NeoPixel, ACAN2040 (Can2040Bus wrapper).
- Build: standard Arduino. UF2/bin/elf in `build/` are not versioned.
- Flash: upload UF2 to RP2040 Zero (BOOTSEL) or via debugger/USB.

## Structure

- `ProtocolManager.*` — protocol selection/switching.
- `protocols/` — CAN/K-Line/KWP/J1939 implementations.
- `CANConfig.h`, `KLineConfig.h` — pins and speeds.
- `RP2040zero_emule_CAN.ino` — UI, display, buttons, status.

## Limitations

- Intended for bench/demo only, not production automotive use.
- ISO-TP transfers expect a Flow Control from the scanner (block size 0 supported).
