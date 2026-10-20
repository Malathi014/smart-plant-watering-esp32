# 🌱 Smart Automatic Plant Watering System Using ESP32

An automated embedded irrigation controller built on the **ESP32 microcontroller** that monitors soil moisture dynamics and dispenses water autonomously. The system uses closed-loop feedback via a soil moisture probe and an optically isolated relay module to prevent overwatering, minimize water wastage, and eliminate manual monitoring.

---

## 🚀 Key Features

- **Continuous Moisture Tracking:** Real-time ADC sampling of soil moisture levels via ESP32 GPIO 34.
- **Closed-Loop Actuation:** Automatic switching of a 5V mini submersible pump using an active-LOW relay driver.
- **Multi-State Visual & Audio Alerts:**
  - 🟢 **Green LED:** Soil is MOIST / Optimal condition (Pump OFF).
  - 🟡 **Yellow LED:** System active / Reading heartbeat indicator.
  - 🔴 **Red LED + Buzzer:** Soil is DRY / Critical threshold breach (Pump ON + Warning beeps).
- **Hardware Protection:** Complete electrical isolation between the microcontroller logic and the pump power bus to prevent inductive back-EMF and brownout resets.

---

## 🛠️ Hardware Pinout & Wiring Specifications

| Component | ESP32 Pin | Interface Mode | Function |
| :--- | :--- | :--- | :--- |
| **Soil Moisture Sensor (AO)** | GPIO 34 | Analog Input (ADC1) | Continuous soil moisture measurement |
| **Relay Module (IN)** | GPIO 22 | Digital Output | 5V pump switching (Active-LOW) |
| **Green LED** | GPIO 18 | Digital Output (220Ω) | Safe / Moist soil status indicator |
| **Yellow LED** | GPIO 19 | Digital Output (220Ω) | Active monitoring heartbeat |
| **Red LED** | GPIO 21 | Digital Output (220Ω) | Dry soil alert indicator |
| **Active Buzzer** | GPIO 23 | Digital Output | Audible double-beep low-moisture alarm |
| **5V Water Pump** | External 5V / Relay NO | Actuator | Water delivery through irrigation tubing |

> **Electrical Note:** The 5V water pump is driven via an external isolated 5V power supply/bank through the relay's COM and NO terminals to safeguard the ESP32 GPIO from high inductive current surges.

---

## 📂 Project Structure

```
├── smart_plant_watering.ino    # Core ESP32 firmware loop & threshold control
└── README.md                   # System documentation & hardware pin mapping
```

---

## ⚙️ Logic & Threshold Calibration

- **ADC Range:** 12-bit resolution (`0` to `4095`)
- **Threshold Value:** `2000`
  - `Soil Value > 2000` ➔ **DRY** (Relay ON, Red LED ON, Buzzer Alert)
  - `Soil Value <= 2000` ➔ **WET / MOIST** (Relay OFF, Green LED ON, Buzzer OFF)

---

## 💻 How to Run

1. **Clone the Repository:**
   ```bash
   git clone [https://github.com/Malathi014/smart-plant-watering-esp32.git](https://github.com/Malathi014/smart-plant-watering-esp32.git)
   cd smart-plant-watering-esp32
   ```

2. **Open in Arduino IDE:**
   - Open `smart_plant_watering.ino`.
   - Select Board: **DOIT ESP32 DEVKIT V1**.
   - Select the respective COM Port and click **Upload**.
   - Open the **Serial Monitor** at `115200 baud` to observe real-time moisture logs.
