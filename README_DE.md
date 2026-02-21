# ECU Emulator (RP2040 Zero) — DE

ECU-Emulator zum Testen/Demonstrieren von Diagnosescannern über CAN und K-Line.

## Hardware

- MCU: RP2040 Zero
- Display: OLED 128x64 I2C (SDA=GPIO4, SCL=GPIO5)
- Tasten: UP=GPIO14, DOWN=GPIO15, SELECT=GPIO26
- Status-LED: WS2812 an GPIO16 (optional)
- CAN-Transceiver VP230: TX=GPIO6, RX=GPIO7, RS=GPIO8
- K-Line (UART0/Serial1): TX=GPIO0, RX=GPIO1 (9637)

## Unterstützte Protokolle

- CAN 11bit 250k (CAN11250)
- CAN 11bit 500k (CAN500K)
- CAN 11bit 500k (Alternative) — CAN11500
- CAN 29bit 250k (CAN29250)
- CAN 29bit 500k (CAN29500)
- J1939 250k (J1939250)
- J1939 500k (J1939500)
- ISO9141-2 (5 Baud Init)
- KWP2000 (5 Baud Init) — KWP5BAUD
- KWP2000 (Fast Init) — KWPFAST

## Diagnose-Funktionen (OBD-II)

Für CAN/J1939 Module 1/5/6/9/10:

- Mode 01: aktuelle Daten (PID-Masken wie CAN500K)
- Mode 02: Freeze Frame
- Mode 03/07/0A: DTCs
- Mode 04: DTC löschen
- Mode 05: O2/λ
- Mode 06: On-Board Monitoring (ISO-TP Multi-Frame)
- Mode 08: Steuerung (Echo, Maskenunterstützung)
- Mode 09: VIN/CALID/CVN/ECU Name/ESN/Emissionsfamilie (ISO-TP Multi-Frame)
- Mode 22/3E: wie in CAN500K implementiert

## Bedienung & Anzeige

- OLED: ELM327-Verbindungsstatus, Protokollname, ECU-Name, Nummer des ausgewählten Moduls.
- WS2812: blinkt bei Aktivität, grün mit Traffic, blau ohne; aus, wenn Protokoll inaktiv.
- Tasten:
  - UP/DOWN: Protokolle blättern (stoppt aktuelles beim Wechsel)
  - SELECT: gewähltes Protokoll starten/stoppen (Start nach 1s Verzögerung)

## Versorgung und Verbindungen

- CAN-Bitrate wird durch Modul gewählt (250k/500k usw.). Start-Bus: 250k auf RX=7, TX=6.
- K-Line: 10,4 kbaud, UART0 (Serial1) auf Pins 0/1.

## Build & Flash

- Plattform: Arduino für RP2040 (Board-Paket 5.5.0), Earle Philhower Core.
- Bibliotheken: Adafruit_SSD1306, Adafruit_GFX, Adafruit_NeoPixel, ACAN2040 (Can2040Bus Wrapper).
- Build: Standard-Arduino. UF2/bin/elf im `build/` werden nicht versioniert.
- Flash: UF2 auf RP2040 Zero (BOOTSEL) oder via Debugger/USB laden.

## Struktur

- `ProtocolManager.*` — Auswahl/Umschalten der Protokolle.
- `protocols/` — Implementierungen für CAN/K-Line/KWP/J1939.
- `CANConfig.h`, `KLineConfig.h` — Pins und Geschwindigkeiten.
- `RP2040zero_emule_CAN.ino` — UI, Display, Tasten, Status.

## Einschränkungen

- Nur für Bench/Demo gedacht, nicht für den produktiven Fahrzeugeinsatz.
- ISO-TP Transfers erwarten Flow Control vom Scanner (Blockgröße 0 wird unterstützt).
