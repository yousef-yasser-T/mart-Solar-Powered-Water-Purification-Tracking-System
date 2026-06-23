<div align="center">

# ☀️ Solar-Powered Water Purification System
### نظام تنقية المياه بالطاقة الشمسية

![Arduino](https://img.shields.io/badge/Arduino-UNO-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![Solar](https://img.shields.io/badge/Solar-0.5W_5.5V-FFD700?style=for-the-badge&logo=sun&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

> **An innovative embedded system that combines solar tracking, dual-battery management via a custom charging protocol, and real-time water purification monitoring — all controlled by a single Arduino UNO.**

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [System Architecture](#-system-architecture)
- [Mechanical Components](#-mechanical-components)
- [Hardware Components](#-hardware-components)
- [Software & Firmware](#-software--firmware)
- [Custom Charging Protocol](#-custom-charging-protocol-novel-design)
- [Relay Logic & Battery Switching](#-relay-logic--battery-switching)
- [Solar Tracking Algorithm](#-solar-tracking-algorithm)
- [Water Flow Monitoring](#-water-flow-monitoring)
- [Wiring Diagram](#-wiring-diagram)
- [Circuit Schematic](#-circuit-schematic)
- [Installation & Upload](#-installation--upload)
- [Demo Video](#-demo-video)
- [Future Development](#-future-development)
- [Contributing](#-contributing)

---

## 🌍 Overview

This project is a **self-powered water purification unit** designed for off-grid and low-resource environments. It uses a solar panel with a 3-axis sun-tracking system to maximize energy harvesting, even from a tiny **0.5W / 220mA panel**. The energy is stored through a novel step-up charging protocol into a **12V Lithium battery pack**, which then powers a water pump, filter, and all sensors.

### Key Innovations:
| Feature | Description |
|---|---|
| 🔭 3-Axis Solar Tracker | 4x LDR sensors + 2x Servo motors for precise sun following |
| ⚡ Custom Charging Protocol | XL6009 Step-Up → LM7805 → Lithium Charger Module — unique ground-sharing design |
| 🔋 Dual Battery System | Battery 1 charges Battery 2 via relay switching |
| 💧 Real-Time Flow Monitoring | YF-S201 flow sensor with interrupt-driven pulse counting |
| 🔌 Optional Mains Power | Can also be powered from AC mains electricity (220V via adapter) |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    SOLAR PANEL (5.5V / 220mA)                   │
│                    With Sun Tracking (3-Axis)                    │
└────────────────────────┬────────────────────────────────────────┘
                         │
                    ┌────▼────┐
                    │ XL6009  │  Step-Up Converter
                    │ >12V    │  (Boost to >12V)
                    └────┬────┘
                         │
                    ┌────▼────┐
                    │ LM7805  │  Voltage Regulator
                    │  5V     │  + Noise Capacitors
                    └────┬────┘
                         │
               ┌─────────▼──────────┐
               │  Li Battery Charger │  (USB Type-C 3S 12.6V 2A)
               │  Module (3S BMS)    │
               └─────────┬──────────┘
                         │
         ┌───────────────▼───────────────┐
         │         BATTERY 1 (12V)        │
         │  3x Li 3.4V cells + BMS        │
         └───────────────┬───────────────┘
                         │
              ┌──────────▼──────────┐
              │   RELAY MODULE (4ch) │  ◄── Arduino Control
              │   Automatic Switch   │
              └──────────┬──────────┘
                         │
         ┌───────────────▼───────────────┐
         │         BATTERY 2 (12V)        │
         │  1800mAh Lithium Pack + BMS    │
         └───────────────┬───────────────┘
                         │
         ┌───────────────▼───────────────┐
         │           LOADS                │
         │  Water Pump │ Servos │ Arduino │
         └────────────────────────────────┘
```

---

## ⚙️ Mechanical Components

> 📸 *[Add mechanical assembly photos here]*

### Structure & Filter Assembly

| Part | Description |
|---|---|
| 🪣 Water Tank | Main reservoir for storing purified water |
| 🔩 Filter Housing | UK-standard single-candle water filter (1 cartridge) |
| 🚿 Hose Pipe | White 12V-compatible flexible hose (1 meter) |
| 🔧 Pump Mount | Bracket holding the 370C-12 micro water pump |
| 🌞 Solar Panel Frame | Rotating mount with 2-axis servo-driven gimbal |
| 🔭 Tracker Base | Rigid base for horizontal servo (azimuth axis) |
| 📐 Tracker Arm | Pivot arm for vertical servo (elevation axis) |
| 📦 Electronics Box | Enclosure for Arduino, relay module, battery, and charging PCB |

### Solar Tracker Mechanical Design

The tracker uses a **2-servo gimbal** attached to the solar panel:
- **Horizontal servo** → controls left/right (azimuth)
- **Vertical servo** → controls up/down (elevation)
- **4 LDR sensors** are mounted at the four corners of the panel face to detect light direction

```
        [ LDR_TL ]  [ LDR_TR ]
              ┌──────────┐
              │  SOLAR   │
              │  PANEL   │
              └──────────┘
        [ LDR_BL ]  [ LDR_BR ]
```

---

## 🔌 Hardware Components

> 📸 *[Add hardware/wiring photos here]*

### Full Bill of Materials (BOM)

| # | Component | Spec | Qty |
|---|---|---|---|
| 1 | Arduino UNO | ATmega328P, 5V logic | 1 |
| 2 | Solar Cell | 5.5V, 220mA, 110×80mm, LED indicator | 1 |
| 3 | XL6009 Step-Up Module | DC-DC Boost, 4A max | 1 |
| 4 | LM7805 Voltage Regulator | 5V output, TO-220 | 1 |
| 5 | Li Battery Charger Module | 3S 12.6V, 2A, USB Type-C | 1 |
| 6 | Lithium Battery Pack | 12V 1800mAh, BMS included | 1 |
| 7 | Li Cells (inside pack) | 3× 3.4V Lithium cells | 3 |
| 8 | Relay Module | 5V, 4-channel, Active-Low | 1 |
| 9 | MG996R Servo Motor | 180°, Full Metal Gear | 2 |
| 10 | YF-S201 Water Flow Sensor | Hall-effect, 1–30 L/min | 1 |
| 11 | 370C-12 Micro Water Pump | 12V, 1.5 LPM, 200kPa | 1 |
| 12 | LDR GL5528 | 8kΩ–20kΩ, 5mm | 4 |
| 13 | LCD Display | 16×2, I2C (0x27) | 1 |
| 14 | Capacitor 330µF 50V | Noise filtering | 3 |
| 15 | Capacitor 470µF 50V | Noise filtering | 3 |
| 16 | Resistor 10kΩ | LDR voltage dividers | 4 |
| 17 | Safety Switch (K) | User/safety selector | 1 |
| 18 | White Hose Pipe | 12V compatible, 1 meter | 1 |
| 19 | Protection Diode | Reverse current block | 1+ |

---

## 💻 Software & Firmware

> 📸 *[Add code/IDE screenshot here]*

### Arduino Libraries Required

```cpp
#include <Wire.h>                // I2C communication
#include <LiquidCrystal_I2C.h>  // LCD over I2C
#include <Servo.h>               // Servo motor control
```

Install via Arduino IDE → **Sketch → Include Library → Manage Libraries**:
- `LiquidCrystal I2C` by Frank de Brabander

### Pin Map

| Arduino Pin | Connected To | Type |
|---|---|---|
| `D2` | YF-S201 Flow Sensor | Interrupt Input |
| `D4` | Relay 1 | Digital Output |
| `D5` | Relay 2 | Digital Output |
| `D6` | Relay 3 | Digital Output |
| `D7` | Relay 4 | Digital Output |
| `D9` | Servo Horizontal | PWM Output |
| `D10` | Servo Vertical | PWM Output |
| `A0` | LDR Top-Left | Analog Input |
| `A1` | LDR Top-Right | Analog Input |
| `A2` | LDR Bottom-Left | Analog Input |
| `A3` | LDR Bottom-Right | Analog Input |
| `A4` | Safety Switch K | Analog/Digital Input |
| `SDA (A4*)` | LCD I2C SDA | I2C |
| `SCL (A5*)` | LCD I2C SCL | I2C |

> *Note: A4/A5 are dedicated I2C pins on UNO. Safety switch uses `INPUT_PULLUP`.*

### Full Firmware

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

// --- Pin Definitions ---
#define FLOW_SENSOR_PIN 2
#define RELAY_1 4
#define RELAY_2 5
#define RELAY_3 6
#define RELAY_4 7
#define SWITCH_SAFETY A4

// Servos
Servo servoHorizontal;
Servo servoVertical;
int servoH_Pos = 90;
int servoV_Pos = 90;

// LDRs (Voltage Dividers with 10k resistors)
#define LDR_TOP_LEFT     A0
#define LDR_TOP_RIGHT    A1
#define LDR_BOTTOM_LEFT  A2
#define LDR_BOTTOM_RIGHT A3

// Flow Sensor Variables
volatile int pulseCount = 0;
float flowRate = 0.0;
unsigned long oldTime = 0;

LiquidCrystal_I2C lcd(0x27, 16, 2);

void pulseCounter() {
  pulseCount++;
}

void setup() {
  Serial.begin(9600);

  // Relay Pins Setup (Active Low - HIGH = OFF)
  pinMode(RELAY_1, OUTPUT); digitalWrite(RELAY_1, HIGH);
  pinMode(RELAY_2, OUTPUT); digitalWrite(RELAY_2, HIGH);
  pinMode(RELAY_3, OUTPUT); digitalWrite(RELAY_3, HIGH);
  pinMode(RELAY_4, OUTPUT); digitalWrite(RELAY_4, HIGH);

  pinMode(SWITCH_SAFETY, INPUT_PULLUP);
  pinMode(FLOW_SENSOR_PIN, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(FLOW_SENSOR_PIN), pulseCounter, FALLING);

  servoHorizontal.attach(9);
  servoVertical.attach(10);
  servoHorizontal.write(servoH_Pos);
  servoVertical.write(servoV_Pos);

  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Solar Purifier");
  delay(2000);
  lcd.clear();
}

void loop() {
  // --- 1. Solar Tracking Logic ---
  int topLeft     = analogRead(LDR_TOP_LEFT);
  int topRight    = analogRead(LDR_TOP_RIGHT);
  int bottomLeft  = analogRead(LDR_BOTTOM_LEFT);
  int bottomRight = analogRead(LDR_BOTTOM_RIGHT);

  int avgTop   = (topLeft + topRight) / 2;
  int avgBot   = (bottomLeft + bottomRight) / 2;
  int avgLeft  = (topLeft + bottomLeft) / 2;
  int avgRight = (topRight + bottomRight) / 2;

  // Vertical tracking
  if (avgTop > avgBot && servoV_Pos < 180) servoV_Pos++;
  else if (avgBot > avgTop && servoV_Pos > 0) servoV_Pos--;
  servoVertical.write(servoV_Pos);

  // Horizontal tracking
  if (avgLeft > avgRight && servoH_Pos < 180) servoH_Pos++;
  else if (avgRight > avgLeft && servoH_Pos > 0) servoH_Pos--;
  servoHorizontal.write(servoH_Pos);

  // --- 2. Water Flow Calculation ---
  if ((millis() - oldTime) > 1000) {
    detachInterrupt(digitalPinToInterrupt(FLOW_SENSOR_PIN));
    flowRate = ((1000.0 / (millis() - oldTime)) * pulseCount) / 7.5;
    oldTime = millis();
    pulseCount = 0;

    lcd.setCursor(0, 0);
    lcd.print("Flow: ");
    lcd.print(flowRate);
    lcd.print(" L/Min   ");

    attachInterrupt(digitalPinToInterrupt(FLOW_SENSOR_PIN), pulseCounter, FALLING);
  }

  // --- 3. Safety Switch & Relay Protocol Logic ---
  if (digitalRead(SWITCH_SAFETY) == LOW) {
    // Battery 1 charges Battery 2 (Ground-sharing protocol)
    digitalWrite(RELAY_1, LOW);
    digitalWrite(RELAY_2, LOW);
    digitalWrite(RELAY_3, LOW);
    digitalWrite(RELAY_4, LOW);
    lcd.setCursor(0, 1);
    lcd.print("BATT1 -> BATT2  ");
  } else {
    // Standard Solar → Battery 1 charging mode
    digitalWrite(RELAY_1, HIGH);
    digitalWrite(RELAY_2, HIGH);
    digitalWrite(RELAY_3, HIGH);
    digitalWrite(RELAY_4, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Solar Charging  ");
  }

  delay(50);
}
```

---

## ⚡ Custom Charging Protocol (Novel Design)

This is a **newly invented charging protocol** designed specifically to charge a 12V Lithium battery from a very small **0.5W / 220mA solar panel**.

### Protocol Chain:

```
Solar Panel (5.5V/220mA)
        │
        ▼
  XL6009 Step-Up
  (Boost → >12V)
        │
        ▼
  LM7805 + Capacitors
  (Regulate → stable 5V)
  [330µF × 3 + 470µF × 3 for noise filtering]
        │
        ▼
  3S Li Charger Module
  (USB Type-C, 2A, 12.6V)
        │
        ▼
  Battery 1 (12V Li, 3S BMS)
        │
     [Relay Module]
        │
        ▼
  Battery 2 (12V 1800mAh)
```

### Ground-Sharing Innovation

The relay module exploits a **shared-ground topology** to switch the same charging circuit between:
- **Solar → Battery 1** (normal operation)
- **Battery 1 → Battery 2** (user-triggered via Switch K)

A **protection diode** prevents reverse current flow between stages.

---

## 🔋 Relay Logic & Battery Switching

| Switch K State | Relay State | Mode |
|---|---|---|
| **OFF** (HIGH) | All Relays OFF (HIGH) | Solar charges Battery 1 + Solar Tracker active |
| **ON** (LOW) | All Relays ON (LOW) | Battery 1 charges Battery 2 (isolated from solar) |

> ⚠️ The battery-to-battery transfer only activates when the user manually enables Switch K — this acts as both a **user choice** and a **safety interlock**.

---

## ☀️ Solar Tracking Algorithm

Four LDR sensors compare light intensity across quadrants:

```
Vertical:   avgTop  vs avgBot   → move servoV up/down
Horizontal: avgLeft vs avgRight → move servoH left/right
```

Each loop cycle moves the servo by **±1 degree** for smooth, jitter-free tracking. The system stabilizes when all four LDRs read approximately equal values (sun centered on panel).

---

## 💧 Water Flow Monitoring

Uses the **YF-S201** Hall-effect flow sensor on interrupt pin D2:

```
Flow Rate (L/min) = (pulses per second) / 7.5
```

- Measured every **1000ms** using `millis()` timer
- Interrupt safely detached/re-attached during calculation
- Result displayed live on LCD Row 0

---

## 🗺️ Wiring Diagram

> 📸 *[Add your Fritzing or hand-drawn wiring diagram image here]*

```markdown
![Wiring Diagram](docs/wiring_diagram.png)
```

---

## 🔧 Circuit Schematic

> 📸 *[Add your circuit schematic image here]*

```markdown
![Schematic](docs/schematic.png)
```

---

## 🚀 Installation & Upload

### Requirements
- Arduino IDE 1.8+ or Arduino IDE 2.x
- Arduino UNO board
- USB Type-A to Type-B cable

### Steps

```bash
# 1. Clone this repository
git clone https://github.com/YOUR_USERNAME/solar-water-purifier.git
cd solar-water-purifier

# 2. Open the firmware
#    Open Arduino IDE → File → Open → firmware/solar_purifier.ino

# 3. Install Libraries
#    Sketch → Include Library → Manage Libraries
#    Search and install: "LiquidCrystal I2C" by Frank de Brabander

# 4. Select Board
#    Tools → Board → Arduino UNO

# 5. Select Port
#    Tools → Port → (your COM port)

# 6. Upload
#    Click Upload (→)
```

---

## 🎬 Demo Video

> A full walkthrough of the system — from solar tracking to water purification and battery switching.

### ▶️ Watch on YouTube

[![Solar Water Purifier Demo](https://img.youtube.com/vi/YOUR_VIDEO_ID/maxresdefault.jpg)](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)

> 📌 *Click the thumbnail above to watch the demo on YouTube.*

### 📹 What the video covers:
- 🔭 Solar tracker following the sun in real time across 3 axes
- 💧 Water pump operation and flow rate on the LCD display
- 🔋 Battery switching via Safety Switch K (Battery 1 → Battery 2)
- ⚡ Charging protocol demonstration (Solar → Step-Up → Regulator → Battery)
- 📟 Arduino code walkthrough and relay logic explanation

---

> **To add your video:**
> 1. Upload your video to YouTube
> 2. Copy the video ID from the URL: `https://youtube.com/watch?v=`**`YOUR_VIDEO_ID`**
> 3. Replace `YOUR_VIDEO_ID` in both the image link and the URL above

---

## 🔮 Future Development

This project is designed to be expandable. Planned upgrades include:

### 📡 Wireless Control (Bluetooth / Wi-Fi)
- Replace or extend Arduino with **ESP32** for built-in Wi-Fi + Bluetooth
- Mobile app to monitor flow rate, battery status, and switch relay modes remotely
- MQTT integration for IoT dashboards (Home Assistant, Blynk, etc.)

### 🔌 AC Mains Power Option
- Add a **12V DC adapter input** in parallel with the battery
- Automatic source switching (solar → battery → mains) via relay priority logic
- Ideal for indoor or semi-outdoor installation near power outlets

### 📊 Data Logging
- SD card module to log flow rate, solar tracking angle, and battery voltage over time
- Export CSV for analysis

### 🔔 Smart Alerts
- Low battery / low flow rate notifications via Bluetooth or buzzer
- Filter replacement reminder based on total liters processed

### 🌊 Multi-Stage Filtration
- Add pre-filter and UV sterilization stage
- Sensors for water quality (TDS, pH) before and after filtration

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the project
2. Create your feature branch: `git checkout -b feature/AmazingFeature`
3. Commit your changes: `git commit -m 'Add some AmazingFeature'`
4. Push to the branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

<div align="center">

**Built with ❤️ using Arduino, Solar Energy & Clean Water Innovation**

⭐ If this project helped you, please give it a star!

</div>
