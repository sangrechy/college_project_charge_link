# ChargeLink ⚡🔋

**ChargeLink** is an intelligent charging controller and real-time telemetry system combining custom ESP32 hardware and a cross-platform Flutter mobile application.

---

## 📂 Repository Structure & Releases

The project code is organized into versioned release directories:

| Release | Status | Architecture & Highlights |
| :--- | :--- | :--- |
| [**`v1/`**](v1/README.md) | **Active / Production** | Dual-cell aware mobile charging dashboard with BLE telemetry, SQLite power persistence, INA219 current sensing, automated relay cutoffs, and voice alerts. |

---

## 🧭 v1 Subfolder Documentation

For technical details, specifications, and setup instructions, refer to each subfolder inside `v1/`:

| Subfolder | Documentation | Highlights |
| :--- | :--- | :--- |
| [**`v1/apks/`**](v1/apks/ChargeLink-v3-release.apk) | [**Prebuilt Android APK**](v1/apks/) | Pre-compiled standalone release APK (`ChargeLink-v3-release.apk`, v3.0.0). |
| [**`v1/application/`**](v1/application/) | [**Flutter Application**](v1/application/) | Flutter mobile app source code containing `application_v3` (production) and `application_v2`. |
| [**`v1/firmware/`**](v1/firmware/) | [**ESP32 Firmware**](v1/firmware/) | `SmartChargeBox` Arduino sketch (INA219 sensing, BLE GATT server, relay control) and audio generation tools. |
| [**`v1/doc/`**](v1/doc/) | [**Project Documents**](v1/doc/) | BLE protocol specs (`esp32_api.md`), system flowcharts, review presentations, and demo simulations. |
| [**`v1/res/`**](v1/res/) | [**Audio & Resources**](v1/res/) | Raw voice prompt audio WAV files and conversion utilities. |

---

## ⚡ Quick Start (v1)

### 1. Install Android APK
Download and install [**`v1/apks/ChargeLink-v3-release.apk`**](v1/apks/ChargeLink-v3-release.apk) directly on your Android phone.

### 2. Run Flutter App from Source
```bash
cd v1/application/application_v3
flutter pub get
flutter run
```

### 3. Flash ESP32 Firmware
1. Open `v1/firmware/SmartChargeBox/SmartChargeBox.ino` in Arduino IDE or PlatformIO.
2. Install required libraries: `Adafruit_INA219`, `ESP32 BLE Arduino`.
3. Compile and flash to your ESP32 controller.

---

## 📜 License & Acknowledgments

Developed as a college engineering project (Semester 5 CCP / EP).
