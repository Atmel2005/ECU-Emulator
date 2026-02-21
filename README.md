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
