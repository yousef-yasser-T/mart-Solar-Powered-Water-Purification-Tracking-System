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

><img width="1083" height="561" alt="Screenshot 2026-06-24 172416" src="https://github.com/user-attachments/assets/34bc6597-158e-4d81-bf94-62c81fa7b080" />
 📸 *[Add mechanical assembly photos here]*

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
> 
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
#include <Servo.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

int LDR1 = A0;
int LDR2 = A1;
int LDR3 = A2;
int LDR4 = A3;

int value1, value2, value3, value4;

Servo servoPan;
Servo servoTilt;

float panPos = 90;
float tiltPos = 90;

unsigned long previousMillis = 0;
const int interval = 20;

// ------ الإضافات ------

// ريليه 4
int relay1 = 2;
int relay2 = 4;
int relay3 = 7;
int relay4 = 8;

// ريليه واحد
int relaySingle = 11;

// زرار
int buttonPin = 5;

// ------ Flow Sensor ------
volatile int flowPulseCount = 0;
float flowRate;
unsigned long oldTime = 0;
int flowSensorPin = 3;

// ------ LCD I2C ------
LiquidCrystal_I2C lcd(0x27, 16, 2);

void flowPulse() {
  flowPulseCount++;
}

void setup() {
  Serial.begin(9600);
  servoPan.attach(9);
  servoTilt.attach(10);
  servoPan.write(panPos);
  servoTilt.write(tiltPos);

  pinMode(relay1, OUTPUT);
  pinMode(relay2, OUTPUT);
  pinMode(relay3, OUTPUT);
  pinMode(relay4, OUTPUT);

  pinMode(relaySingle, OUTPUT);
  pinMode(buttonPin, INPUT);

  digitalWrite(relay1, LOW);
  digitalWrite(relay2, LOW);
  digitalWrite(relay3, LOW);
  digitalWrite(relay4, LOW);

  digitalWrite(relaySingle, HIGH);

  // ===== Flow Sensor =====
  pinMode(flowSensorPin, INPUT);
  attachInterrupt(digitalPinToInterrupt(flowSensorPin), flowPulse, RISING);

  // ===== LCD =====
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0,0);
  lcd.print("Flow Meter");

  oldTime = millis();
}

void loop() {

  if (digitalRead(buttonPin) == HIGH) {
    digitalWrite(relaySingle, LOW);
  }

  if (Serial.available()) {
    char c = Serial.read();
    if (c == 'N') {
      digitalWrite(relaySingle, LOW);
    }
  }

  if ((millis() - oldTime) > 1000) {
    
    detachInterrupt(digitalPinToInterrupt(flowSensorPin));
    
    flowRate = ((1000.0 / (millis() - oldTime)) * flowPulseCount) / 7.5 / 60.0 * 1000.0;
    
    oldTime = millis();
    flowPulseCount = 0;
    
    attachInterrupt(digitalPinToInterrupt(flowSensorPin), flowPulse, RISING);
    
    // عرض على LCD
    lcd.clear();
    lcd.setCursor(0,0);
    lcd.print("Flow Rate:");
    
    lcd.setCursor(0,1);
    lcd.print(flowRate);
    lcd.print(" mL/s");
  }

  unsigned long currentMillis = millis();
  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;

    value1 = analogRead(LDR1);
    value2 = analogRead(LDR2);
    value3 = analogRead(LDR3);
    value4 = analogRead(LDR4);

    float x1 = round(value1 / 20.0) / 5.0;
    float x2 = round(value2 / 20.0) / 5.0;
    float x3 = round(value3 / 20.0) / 5.0;
    float x4 = round(value4 / 20.0) / 5.0;

    float rightLDR = (x1 + x3) / 2.0;
    float leftLDR  = (x2 + x4) / 2.0;
    float topLDR   = (x1 + x2) / 2.0;
    float bottomLDR = (x3 + x4) / 2.0;

    float panDiff = rightLDR - leftLDR;
    float tiltDiff = topLDR - bottomLDR;

    if (abs(panDiff) > 0.02) {
      panPos += (panDiff > 0 ? 0.1 : -0.1);
      panPos = constrain(panPos, 0, 180);
      servoPan.write(panPos);
    }

    if (abs(tiltDiff) > 0.02) {
      tiltPos += (tiltDiff > 0 ? -0.1 : 0.1);
      tiltPos = constrain(tiltPos, 0, 180);
      servoTilt.write(tiltPos);
    }
  }
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
