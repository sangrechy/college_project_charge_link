# ChargeLink — Firmware & Hardware Pinout Guide 🔌⚡

This directory contains the firmware for the ESP32 used in **ChargeLink**, along with the complete hardware wiring, relay cutoff logic, GPIO pinout, and multi-ESP device configuration.

---

## 🧭 Submodule Navigation

- [**SmartChargeBox Firmware — `SmartChargeBox/`**](SmartChargeBox/SmartChargeBox.ino) — Arduino firmware, dual independent I2C buses, INA219 current/power telemetry, SSD1306 OLED display, relay cutoff protection, 8-bit DAC voice prompts, and BLE GATT server.
- [**Voice Alert Tools — `audio_tools/`**](audio_tools/) — Python script (`wav_to_h.py`) to convert 16 kHz mono WAV audio prompts into PROGMEM 8-bit unsigned integer arrays for the ESP32 internal DAC.

---

## 🆔 Using a Different ESP32 (Device ID & MAC Address Config)

Every ESP32 has its own unique factory Bluetooth MAC address (e.g. `70:4B:CA:46:D3:BA`). If you swap the board with another ESP32, follow these steps to detect and bind the new ID.

### 1. How to Detect / Find Your New ESP32's ID
You have three easy ways to grab the MAC address of the new board:

- **Option A (Serial Monitor on Boot):**  
  Flash the sketch to your new ESP32 and open Serial Monitor at **`115200`** baud. The ESP32 prints its BLE address on initialization:
  ```text
  [BLE] Initializing BLE Server: Charge Link
  [BLE] Device MAC Address: XX:XX:XX:XX:XX:XX
  ```
  *(Or run `Serial.println(ESP.getEfuseMac(), HEX);` in any quick test sketch).*

- **Option B (ChargeLink Mobile App Device Scanner):**  
  Open the **ChargeLink** mobile app ➔ Go to the **Device** tab ➔ Tap **Scan**. It scans all nearby advertising BLE peripherals and lists their **Device Name**, **MAC address / UUID**, and **RSSI**.

- **Option C (Generic BLE Scanner):**  
  Install **nRF Connect** or **LightBlue** on your phone, power the ESP32, and look for **`Charge Link`**. The app displays its hardware MAC address directly.

---

### 2. Where to Update the ID & Name

Once you have the new ESP32's ID, update these specific configuration files:

#### 📁 A. In ESP32 Firmware (`SmartChargeBox.ino`)
If you want to customize the broadcasted name for this specific box:
- **File:** [`SmartChargeBox/SmartChargeBox.ino`](SmartChargeBox/SmartChargeBox.ino)
- **Line 165:**
  ```cpp
  #define DEVICE_NAME          "Charge Link"     // Change to "Charge Link 2" if running multiple boxes
  #define MODEL_NAME           "CL-SCB-01"
  ```
- **Lines 230–244 (Optional):** If you wish to use unique BLE UUIDs for a separate box.

---

#### 📱 B. In Flutter Mobile App (`application_v2`)
To bind the app directly to your new ESP32's hardware address:
- **File:** [`v1/application/application_v2/lib/services/esp32/esp32_ble_config.dart`](../application/application_v2/lib/services/esp32/esp32_ble_config.dart)
- **Lines 4–5:**
  ```dart
  static const String deviceName = 'Charge Link';        // Must match DEVICE_NAME in firmware
  static const String deviceAddress = 'XX:XX:XX:XX:XX:XX'; // <-- Put your new ESP32 MAC address here (lowercase)
  ```

---

#### 🐍 C. In Python Test Cockpit / Scripts (Optional)
If you run the standalone PC BLE test scripts in `v1/firmware/dummy/`:
- **Files:** `v1/firmware/dummy/charge_link_ble_test.py` & `charge_link_ble_web_test.py`
- Update the target address:
  ```python
  DEVICE_MAC = "XX:XX:XX:XX:XX:XX"  # <-- Put new ESP32 MAC address here
  DEVICE_NAME = "Charge Link"
  ```

---

## 📌 Master Hardware Pinout

The following pinout is the soldered and verified hardware configuration used in v1.

### ⚡ Sensor & Display (Dual I2C Buses)
The firmware runs **two completely separate hardware I2C buses** (`INA_WIRE(0)` and `OLED_WIRE(1)`) to avoid bus lockups, address conflicts, and sensor polling latency.

| Module | Signal | ESP32 GPIO | Bus Channel | Meaning |
| :--- | :--- | :--- | :--- | :--- |
| **INA219 Power Sensor** | **SDA** | **21** | Wire 0 | High-side current/voltage data |
| **INA219 Power Sensor** | **SCL** | **22** | Wire 0 | I2C clock |
| **SSD1306 OLED (128x64)**| **SDA** | **18** | Wire 1 | Display graphics & telemetry data |
| **SSD1306 OLED (128x64)**| **SCL** | **19** | Wire 1 | I2C display clock |

---

### 🔌 Relay Cutoff Switch

| Signal | ESP32 GPIO | Active State | Meaning |
| :--- | :--- | :--- | :--- |
| **RELAY_PIN** | **25** | Active LOW | Controls charging pass-through relay coil |

**Relay Physical Contact Logic:**
- **GPIO 25 `HIGH`** ➔ Relay coil **OFF** ➔ **NC (Normally Closed)** contact closed ➔ **Charging path ENABLED** (default pass-through).
- **GPIO 25 `LOW`** ➔ Relay coil **ON** ➔ **NC OPEN** ➔ **Charging path DISABLED** (emergency/auto-limit cutoff).

> **Hardware note:** Using the NC contact ensures your charger naturally powers the phone even if the ESP32 reboots, while GPIO25 actively trips the relay to disconnect when the battery target or overcharge limit is hit.

---

### 🔊 8-Bit DAC Voice Audio

| Signal | ESP32 GPIO | Output Mode | Meaning |
| :--- | :--- | :--- | :--- |
| **AUDIO_PIN** | **26 (DAC2)** | Analog 8-Bit DAC | Direct audio output to mini amplifier / speaker |

- **Sample Rate:** 16,000 Hz (16 kHz), 8-bit unsigned PCM.
- **Triggered Prompts:** `charging_started.h`, `charging_stopped.h`, `power_limit.h`, `full_charge.h`, `no_device.h`, `charging_error.h`, `app_connected.h`, `app_disconnected.h`, `system_activated.h`.

---

## 📡 BLE GATT Telemetry Server

The ESP32 acts as a GATT peripheral advertising as **`Charge Link`**.

| Characteristic | UUID | Direction | Meaning |
| :--- | :--- | :--- | :--- |
| **SERVICE** | `7f4a0001-6d3b-4a91-9c21-123456789001` | — | Primary ChargeLink GATT service |
| **COMMAND** | `7f4a0002-6d3b-4a91-9c21-123456789001` | Flutter ➔ ESP32 | Control commands (`start_charging`, `stop_charging`, `set_limit`) |
| **RESPONSE** | `7f4a0003-6d3b-4a91-9c21-123456789001` | ESP32 ➔ Flutter | Command acknowledge / status payload |
| **LIVE_DATA** | `7f4a0004-6d3b-4a91-9c21-123456789001` | ESP32 ➔ Flutter | Real-time voltage, current, wattage stream (1 Hz) |
| **HISTORY** | `7f4a0005-6d3b-4a91-9c21-123456789001` | ESP32 ➔ Flutter | RAM ring-buffer historical chunks (max 300 samples) |

---

## ⚡ Power Supply & Wiring

#### Logic Power
- ESP32 board is powered through **5V VIN** or the programming micro-USB / Type-C port.
- All sensors (INA219, OLED), the relay coil board, and the ESP32 **must share a common GND**.

#### High-Power Fast-Charging Path
- The charger's **VBUS (+)** line routes through the **INA219 shunt** (`VIN+` and `VIN-`) and then through the **Relay COM ➔ NC** contact before reaching the phone output connector.
- **Do not route high-current VBUS through standard breadboard traces** — use thick, short 20–22 AWG silicone wires to minimize resistive voltage drop across the box.

---

## 🚀 Flashing & Dependencies

### Required Arduino Libraries
Install the following via the Arduino IDE Library Manager:
- **`Adafruit INA219`** (by Adafruit)
- **`Adafruit SSD1306`** & **`Adafruit GFX Library`** (by Adafruit)
- **`ESP32 BLE Arduino`** (included in the official ESP32 Arduino Core)

### Build Settings
- **Board:** `ESP32 Dev Module`
- **CPU Frequency:** `240MHz (WiFi/BT)`
- **Flash Frequency:** `80MHz`
- **Upload Speed:** `921600` or `115200`
- **Serial Monitor Baud Rate:** `115200`
