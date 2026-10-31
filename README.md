# ESP32 Pocket Lab

A handheld 2.4 GHz RF lab built on an ESP32 with an integrated 2.8" LCDWIKI E32R28T board. It scans Wi-Fi and BLE, cuts a Wi-Fi link with deauth frames, and — the main feature — acts as a **controlled Bluetooth interference source with a built-in stopwatch**, so you can measure how fast an audio link breaks up and how fast it recovers.

Built by **Jazii & Hamed** as a school mini-project on Bluetooth interference.

## Demo

▶️ **Watch the demo:** [REEMPLAZAR CON EL ENLACE DEL VIDEO]

## Features

- **Bluetooth interference engine** with five modes: full 1-13 channel sweep, fixed channel at max rate, BLE advertising flood, BT Classic inquiry, and mixed.
- **Stopwatch test protocol**: start, mark first audible artifact, stop interference, mark recovery. Times are logged automatically.
- **Results table** with mean, min and max times, exportable as CSV over serial.
- Wi-Fi scanner with RSSI, channel and search field.
- Wi-Fi deauth jammer, per-AP or broadcast.
- BLE device scanner with search field.
- Channel analyzer, packet monitor, chip info, I2C scanner, GPIO state, battery ADC, display test and touch canvas.
- Fully on-screen keyboard, no serial needed to operate.
- Touch UI in Spanish, custom bit-banged XPT2046 driver on a dedicated bus.

## Why the board matters

The LCDWIKI E32R28T is not a generic module. Display and touch sit on **separate** SPI buses with fixed pins, and the backlight is on GPIO21. TFT_eSPI's built-in touch does not work here, so this project ships its own touch driver.

## Hardware

| Part | Detail |
|---|---|
| MCU | ESP32-WROOM-32 (LCDWIKI E32R28T / E32N28T board) |
| Display | 2.8" 240x320, ILI9341V (ST7789P3 on the `-1` board revision) |
| Touch | XPT2046, separate SPI bus |
| Backlight | GPIO21, active HIGH, PWM dimming |
| Power | USB 5 V |

## Pinout

| Signal | GPIO |
|---|---|
| TFT SCK / MOSI / MISO / CS / DC | 14 / 13 / 12 / 15 / 2 |
| TFT RST | tied to EN (-1) |
| TFT BL | 21 |
| TP SCK / DIN / DOUT / CS / IRQ | 25 / 32 / 39 / 33 / 36 |
| Battery ADC | 34 |
| SD CS / MOSI / SCK / MISO | 5 / 23 / 18 / 19 (unused) |

## Build

1. Arduino IDE 2.x with **esp32 core 3.3.x**.
2. Install **TFT_eSPI**.
3. Copy `firmware/esp32-pocket-lab/User_Setup.h` into the TFT_eSPI library folder.
4. Board: *ESP32 Dev Module*. Partition scheme: **Huge APP (3MB No OTA)**. Core Debug Level: **None**.
5. Open `firmware/esp32-pocket-lab/esp32-pocket-lab.ino` and flash.
6. On first boot the device asks for touch calibration.

## Usage

1. Pair the phone with the speaker and start playback.
2. Put the ESP32 right next to the **speaker**, not next to the phone, and move the phone a few metres away.
3. `Bluetooth → Interference`, pick **Full 1-13 sweep**.
4. `Bluetooth → Stopwatch test`, press **1. Start**.
5. Press **2. Artifact detected** on the first stutter.
6. Press **3. Stop** to cut the interference.
7. Press **4. Recovered** when audio returns.
8. Press **Save**, repeat at least three times, then `Results → Export CSV`.

## Measurements

| Trial | Distance | Time to artifact | Interference | Recovery |
|---|---|---|---|---|
| 1 | 0.5 m | 1.4 s | 10 s | 3.2 s |
| 2 | 0.5 m | 1.1 s | 10 s | 2.8 s |
| 3 | 1.0 m | 2.9 s | 10 s | 4.6 s |

## Limitations

- Effective range is about 3 m. The PCB antenna is internal and the link under test has its own margin. External antenna, higher duty cycle and closer placement extend it.
- It interferes, it does not decode: no packet sniffing of the audio link.
- 2.4 GHz only. 5 GHz links are unaffected.
- Authorised-band operation only: 2.4 GHz is licence-free, but a jammer next door will notice.

## License

MIT — see [LICENSE](LICENSE).
