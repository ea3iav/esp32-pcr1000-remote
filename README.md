# esp32-pcr1000-remote
ESP32-based touchscreen controller for the ICOM PCR1000 with full frequency control, S-meter, and rotary encoder.
# 📡 ESP32 + ICOM PCR1000 Controller

---

## ⚠️ TFT_eSPI Configuration (IMPORTANT)

After installing **TFT_eSPI**, you must edit:

```
Documents/Arduino/libraries/TFT_eSPI/User_Setup.h
```

### ✔ Recommended method (clean and safe)

👉 **Delete or comment EVERYTHING inside `User_Setup.h`**
👉 Then paste ONLY this:

```cpp
#define ILI9341_DRIVER

#define TFT_MOSI 23
#define TFT_MISO 19
#define TFT_SCLK 18
#define TFT_CS   15
#define TFT_DC   2
#define TFT_RST  4

#define TOUCH_CS 5

#define SPI_FREQUENCY 40000000
#define SPI_TOUCH_FREQUENCY 2500000
```

### ❗ Notes

* Restart Arduino IDE after saving
* Only one setup must be active
* This project uses **HSPI bus (GPIO23/19/18)**

---

## 🧰 Hardware Components

* ESP32 Dev Board
* 2.8" TFT ILI9341 + XPT2046
* **MAX3232 RS232 ↔ TTL module (PCB with TX/RX/VCC/GND)**
* ICOM PCR1000
* Rotary encoder
* 10k potentiometer (Volume)
* 22k potentiometer (Squelch)

---

# 🔌 SCHEMATIC (TEXT / NETLIST STYLE)

## 🖥️ TFT ILI9341 (SPI)

```
ESP32 GPIO23 ───── TFT MOSI
ESP32 GPIO19 ───── TFT MISO
ESP32 GPIO18 ───── TFT SCK
ESP32 GPIO15 ───── TFT CS
ESP32 GPIO2  ───── TFT DC
ESP32 GPIO4  ───── TFT RESET
ESP32 3V3    ───── TFT VCC
ESP32 GND    ───── TFT GND
ESP32 3V3    ───── TFT LED
```

---

## 👆 TOUCH XPT2046

```
ESP32 GPIO5  ───── T_CS
ESP32 GPIO23 ───── T_DIN (shared MOSI)
ESP32 GPIO19 ───── T_DO  (MISO)
ESP32 GPIO18 ───── T_CLK (shared SCK)
ESP32 GND    ───── GND
ESP32 3V3    ───── VCC
```

---

## 📡 RS232 (MAX3232 MODULE)

### 🔹 TTL SIDE (ESP32 ↔ MAX3232)

```
ESP32 GPIO17 (TX2) ───── MAX3232 RX (TTL)
ESP32 GPIO16 (RX2) ───── MAX3232 TX (TTL)
ESP32 GND          ───── MAX3232 GND
ESP32 3V3 / 5V     ───── MAX3232 VCC
```

👉 IMPORTANTE:

* TX siempre cruza con RX
* (ESP TX → módulo RX)

---

### 🔹 RS232 SIDE (MAX3232 ↔ PCR1000 DB9)

```
MAX3232 TX (RS232) ───── DB9 PIN 2 (PCR RX)
MAX3232 RX (RS232) ───── DB9 PIN 3 (PCR TX)
MAX3232 GND        ───── DB9 PIN 5 (GND)
```

---

## 🔌 DB9 PINOUT (PCR1000)

```
PIN 2 → RX (data into PCR1000)
PIN 3 → TX (data from PCR1000)
PIN 5 → GND
```

---

## 🎛️ POTENTIOMETERS

### Volume (10k)

```
3V3 ─────┐
         ├── GPIO34 (ADC)
GND ─────┘
```

### Squelch (22k)

```
3V3 ─────┐
         ├── GPIO35 (ADC)
GND ─────┘
```

---

## 🔘 ROTARY ENCODER

```
GPIO33 ───── CLK
GPIO32 ───── DT
GPIO25 ───── SW
3V3    ───── VCC
GND    ───── GND
```

---

## ⚠️ CRITICAL NOTES

* All GND must be common
* SPI bus is shared (TFT + Touch)
* MISO (GPIO19) is required for touch
* MAX3232 must NOT be connected directly to ESP32 without level conversion

---

## ✅ RESULT

* Full touchscreen UI
* Encoder + analog control
* Real RS232 communication with PCR1000
* Stable and correct hardware mapping

---
## 👉 3d Enclosure:
Custom 3d case to house potentionmeter, encoder connectors and wires 
https://www.thingiverse.com/thing:7321618
