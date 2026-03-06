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

# ECU Emulator (RP2040 Zero) — UA

Емулятор ЕБУ для тестування/демонстрації діагностичних сканерів по CAN і K-Line.

## Апаратна частина

- MCU: RP2040 Zero
- Дисплей: OLED 128x64 I2C (SDA=GPIO4, SCL=GPIO5)
- Кнопки: UP=GPIO14, DOWN=GPIO15, SELECT=GPIO26
- Статус-світлодіод: WS2812 на GPIO16 (опційно)
- CAN-трансивер VP230: TX=GPIO6, RX=GPIO7, RS=GPIO8
- K-Line (UART0/Serial1): TX=GPIO0, RX=GPIO1 (9637)

## Підтримувані протоколи

- CAN 11bit 250k (CAN11250)
- CAN 11bit 500k (CAN500K)
- CAN 11bit 500k (альтернатива) — CAN11500
- CAN 29bit 250k (CAN29250)
- CAN 29bit 500k (CAN29500)
- J1939 250k (J1939250)
- J1939 500k (J1939500)
- ISO9141-2 (5 baud init)
- KWP2000 (5 baud init) — KWP5BAUD
- KWP2000 (fast init) — KWPFAST

## Діагностичні можливості (OBD-II)

Для CAN/J1939 модулів 1/5/6/9/10:

- Mode 01: поточні дані (маски PID як у CAN500K)
- Mode 02: freeze frame
- Mode 03/07/0A: DTC
- Mode 04: очищення DTC
- Mode 05: O2/λ
- Mode 06: On-board monitoring (ISO-TP мультикадр)
- Mode 08: керування (ехо, підтримка маски)
- Mode 09: VIN/CALID/CVN/ECU Name/ESN/Emission family (ISO-TP мультикадр)
- Mode 22/3E: згідно реалізації CAN500K

## Керування та індикація

- OLED: статус підключення ELM327, назва протоколу, назва ЕБУ, номер вибраного модуля.
- WS2812: блимає при активності, зелений при трафіку, синій без; вимкнений, якщо протокол не активний.
- Кнопки:
  - UP/DOWN: гортати протоколи (зупиняє поточний при перемиканні)
  - SELECT: запуск/зупинка вибраного протоколу (старт із затримкою 1s)

## Живлення та підключення

- CAN-швидкість обирається модулем (250k/500k тощо). Стартова шина — 250k на RX=7, TX=6.
- K-Line: 10.4 kbaud, UART0 (Serial1) на пинах 0/1.

## Збірка та прошивка

- Платформа: Arduino для RP2040 (board package 5.5.0), ядро Earle Philhower.
- Бібліотеки: Adafruit_SSD1306, Adafruit_GFX, Adafruit_NeoPixel, ACAN2040 (Can2040Bus wrapper).
- Збірка: стандартний Arduino. UF2/bin/elf у `build/` не версіонуються.
- Прошивка: залити UF2 в RP2040 Zero (BOOTSEL) або через відладчик/USB.

## Структура

- `ProtocolManager.*` — вибір та перемикання протоколів.
- `protocols/` — реалізації CAN/K-Line/KWP/J1939.
- `CANConfig.h`, `KLineConfig.h` — піни та швидкості.
- `RP2040zero_emule_CAN.ino` — UI, дисплей, кнопки, статус.

## Обмеження

- Реальний CAN/K-Line стек не призначений для бойової експлуатації — лише стенд/демо.
- ISO-TP передачі потребують FC від сканера (block size 0 підтримується).

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

# ECU Emulator (RP2040 Zero) — RU

Эмулятор ЭБУ для теста/демонстрации диагностических сканеров по CAN и K-Line.

## Аппаратная часть

- MCU: RP2040 Zero
- Дисплей: OLED 128x64 I2C (SDA=GPIO4, SCL=GPIO5)
- Кнопки: UP=GPIO14, DOWN=GPIO15, SELECT=GPIO26
- Статус-светодиод: WS2812 на GPIO16 (опционально)
- CAN-трансивер VP230: TX=GPIO6, RX=GPIO7, RS=GPIO8
- K-Line (UART0/Serial1): TX=GPIO0, RX=GPIO1 (9637)

## Поддерживаемые протоколы

- CAN 11bit 250k (CAN11250)
- CAN 11bit 500k (CAN500K)
- CAN 11bit 500k (альтернатива) — CAN11500
- CAN 29bit 250k (CAN29250)
- CAN 29bit 500k (CAN29500)
- J1939 250k (J1939250)
- J1939 500k (J1939500)
- ISO9141-2 (5 baud init)
- KWP2000 (5 baud init) — KWP5BAUD
- KWP2000 (fast init) — KWPFAST

## Диагностические возможности (OBD-II)

Варианты CAN/J1939 модулей 1/5/6/9/10 поддерживают:

- Mode 01: текущие данные (маски PID под CAN500K)
- Mode 02: freeze frame
- Mode 03/07/0A: DTC
- Mode 04: очистка DTC
- Mode 05: O2/λ
- Mode 06: On-board monitoring (ISO-TP мульткадр)
- Mode 08: управление (эхо, поддержка маски)
- Mode 09: VIN/CALID/CVN/ECU Name/ESN/Emission family (ISO-TP мульткадр)
- Mode 22/3E: в соответствии с реализацией CAN500K

## Управление и индикация

- OLED: статус ELM327 подключения, имя протокола, имя ЭБУ, номер выбранного модуля.
- WS2812: мигает при активности, цвет: зелёный при трафике, синий без трафика; выключен, если протокол не активен.
- Кнопки:
  - UP/DOWN: листать протоколы (останавливает текущий при переключении)
  - SELECT: запуск/останов выбранного протокола (старт с задержкой 1s)

## Питание и соединение

- CAN скорость выбирается модулем (250k/500k и др.). Стартовая шина — 250k на RX=7, TX=6.
- K-Line: 10.4 kbaud, UART0 (Serial1) на пинах 0/1.

## Сборка и прошивка

- Платформа: Arduino для RP2040 (board package 5.5.0), ядро Earle Philhower.
- Библиотеки: Adafruit_SSD1306, Adafruit_GFX, Adafruit_NeoPixel, ACAN2040 (Can2040Bus wrapper).
- Сборка: стандартный Arduino. UF2/bin/elf в `build/` не версионируются.
- Прошивка: залить UF2 в RP2040 Zero (BOOTSEL) или через отладчик/USB.

## Структура

- `ProtocolManager.*` — выбор и переключение протоколов.
- `protocols/` — реализации CAN/K-Line/KWP/J1939.
- `CANConfig.h`, `KLineConfig.h` — пины и скорости.
- `RP2040zero_emule_CAN.ino` — UI, дисплей, кнопки, статус.

## Ограничения

- Реальный CAN/K-Line стек не предназначен для боевой эксплуатации — только стенд/демо.
- ISO-TP передачи требуют FC от сканера (block size 0 поддерживается).
