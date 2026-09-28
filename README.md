# ⚡🔋 ChargeLink

[![GitHub Repository](https://img.shields.io/badge/GitHub-college__project__charge__link-181717?style=flat&logo=github)](https://github.com/sangrechy/college_project_charge_link)

**ChargeLink** is a **Smart Bypass Charger Box** that works as an **intermediate between the charger and the phone**.

The main idea is to bypass **brand-specific charging restrictions** and improve charging compatibility between different chargers and smartphones, where technically possible.

Different phone brands use different fast-charging technologies, communication methods and charging policies. Because of this, even if a charger can provide high power, another brand's phone may not be able to use that full charging capability.

ChargeLink works as an **intelligent middle layer** between the charger and phone to analyze the charging communication, monitor the charging process and explore better charging compatibility.

---

## 🌟 What I'm Trying to Do

The main goal of ChargeLink is to make a smart inline box that can:

- **Bypass brand restrictions:** Explore charging compatibility between different brands and charging standards.
- **Handle charging communication:** Intercept and analyze the communication between the charger and phone.
- **Select charging profiles:** Negotiate and select the highest mutually supported power profile.
- **Monitor charging:** Measure voltage, current, power and temperature in real time.
- **Analyze charging behaviour:** Record and study how charging behaves at different stages.
- **Estimate battery wear:** Use charging data to estimate battery wear and thermal stress.
- **Maintain charging profiles:** Store device and charging profiles for future compatibility improvements.

The project is designed as a portable inline device used as an intermediate between the charger and smartphone.

---

## 🛠️ What I Actually Built

For now, I have built the **Smart Charge Box and its charging telemetry system**.

- **ESP32 Smart Box:** Built around an ESP32 with an INA219 high-side current/voltage sensor, relay cutoff and 8-bit DAC voice output.
- **BLE Communication:** Added a custom ESP32 BLE GATT server to send real-time voltage, current and power data and receive commands.
- **Flutter Mobile App (`application_v2`):** Built a Flutter dashboard with live charging graphs, phone battery and temperature tracking, and control features.
- **Power Calculation:** Added dual-cell battery compensation for OnePlus/Oppo type split-cell batteries and compensation for phone display/system power usage.
- **Local Data Storage:** Added SQLite storage in the mobile app for keeping around 30–60 days of charging history.
- **Android APK:** Added a prebuilt Android APK that can be installed directly without setting up the Flutter project.

---

## 🔌 Main Hardware

- **ESP32** — Main controller and BLE communication
- **USB-PD Controller** — Charging power profile negotiation
- **INA219 / INA226** — Voltage, current and power monitoring
- **Relay Module** — Charging path cutoff
- **Audio Output / Speaker** — Voice status alerts
- **OLED Display** — Charging information on the box
- **Dual USB-C Connectors** — Charger input and phone output
- **Physical Buttons** — Manual control
- **SD Card (Optional)** — Charging data logging

---
## 📸 Current Setup

<div align="center">

<img src="https://github.com/user-attachments/assets/b3eb91c9-eb70-4900-81a0-c56a11329bae" width="850" alt="ChargeLink Setup" />

<br><br>

<img src="https://github.com/user-attachments/assets/01f41ecf-b757-4bf8-a08a-82f25afcca6a" width="300" alt="ChargeLink Hardware 1" />
<img src="https://github.com/user-attachments/assets/ea918293-95b5-4f8a-a7a9-1f13126c8bc9" width="300" alt="ChargeLink Hardware 2" />
<img src="https://github.com/user-attachments/assets/98b622d8-6f2c-44d4-88b2-8e5a7d85db9c" width="300" alt="ChargeLink Hardware 3" />

<br><br>

<img src="https://github.com/user-attachments/assets/414ec06d-ce05-4336-8a7d-456bef959f8c" width="400" alt="ChargeLink Hardware 4" />

</div>
---

## 🎥 UI

<div align="center">

<table>
  <tr>
    <td><img width="250" alt="u1" src="https://github.com/user-attachments/assets/c719ba1c-530e-41d8-a853-e4ba6a0a6e1e" /></td>
    <td><img width="250" alt="u2" src="https://github.com/user-attachments/assets/1fad308f-9c3b-4574-91ca-c43e157272a1" /></td>
    <td><img width="250" alt="u3" src="https://github.com/user-attachments/assets/781edabb-526f-4415-a9af-0e98d5fa4d23" /></td>
    <td><img width="250" alt="u4" src="https://github.com/user-attachments/assets/091af093-890c-4ca0-a3d0-80c8286d8d36" /></td>
  </tr>
</table>

</div>
---

## 📂 Repository

The project is organized into versioned directories.

For the **hardware pinouts, wiring, firmware, Flutter app, APK and BLE details**, go to the **v1 README**:

👉 [**Go to v1 README**](v1/README.md)

All the detailed setup and implementation are available inside `v1/`.

