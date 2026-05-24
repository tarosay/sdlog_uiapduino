# SDLog for UIAPduino

OpenLog-compatible UART data logger for UIAPduino (CH32V003).  
Receives data on UART RX and writes it to a microSD card.

This sketch was developed for the **UIAP LOG** board, a dedicated logging board based on UIAPduino.

| Front | Back |
|-------|------|
| ![UIAP LOG front](sdlog1.jpg) | ![UIAP LOG back](sdlog2.jpg) |

> Originally shared on X: https://x.com/momoonga/status/2057563370535223761

---

## Requirements

### UIAPduino HID board package

This sketch is optimized for the **UIAPduino HID** board package, which produces binaries small enough to fit in the CH32V003's 16 KB Flash.

1. Open Arduino IDE and go to **File > Preferences**
2. Add the following URL to **Additional boards manager URLs**:
   ```
   https://github.com/tarosay/board_manager_files/raw/main/package_uiap_hid_index.json
   ```
3. Open **Tools > Board > Boards Manager**, search for `UIAPduino`, and install **UIAPduino HID**

### Arduino IDE settings

| Setting | Value |
|---------|-------|
| Board | `Tools > Board > UIAP_HID > HID ProMicro CH32V003` |
| USB | `Tools > USB > No USB (SD log / UART only)` |
| FQBN | `UIAP_HID:ch32v:CH32V003:usb=nousb,opt=oslto` |

---

## Wiring

| UIAPduino | microSD adapter |
|-----------|----------------|
| A2 (PC4, pin 6) | CS (DAT3) |
| 8 (PC6) | MOSI (CMD / DI) |
| 7 (PC5) | SCK (CLK) |
| 9 (PC7) | MISO (DAT0 / DO) |
| 3V3 | VDD |
| GND | VSS |

**UART (data input):**
- `RX  PD6` — connect to TX of the device to be logged
- `TX  PD5` — not used for logging

---

## Usage

### Log files

A new log file is created on every power cycle:  
`LOG00001.TXT`, `LOG00002.TXT`, ...

### Baud rate (CONFIG.TXT)

Place `CONFIG.TXT` on the SD card to set the baud rate.  
Format: `9600,26,3,0` (only the first value is used)

If the file does not exist, 9600 bps is used and `CONFIG.TXT` is created automatically.  
Baud rates of 300 bps or higher can be specified.

### LED behavior

| LED | Meaning |
|-----|---------|
| Fast blink | Error (SD init failed, cannot create log file, etc.) |
| Blink | Writing data to SD |
| Steady | Waiting for data |

---

## License

MIT
