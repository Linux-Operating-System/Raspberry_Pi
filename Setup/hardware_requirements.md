# Hardware Checklist & System Requirements

Before configuring your Raspberry Pi, it is vital to ensure you have the proper hardware components. Using mismatched accessories—specifically poor-quality power supplies or slow SD cards—is the leading cause of system instability, boot failures, and filesystem corruption.

This guide details the essential, optional, and performance-critical hardware needed to build a stable Raspberry Pi environment.

---

## 📋 Core Hardware Checklist

To boot a Raspberry Pi and run a fully functional operating system, you will need the following core components:

```
┌────────────────────────────────────────────────────────┐
│               RASPBERRY PI HARDWARE SETUP               │
└───────────────────────────┬────────────────────────────┘
                            ▼
      ┌─────────────────────┴─────────────────────┐
      │  Essential Elements                       │
      │  [ ] Raspberry Pi SBC Board               │
      │  [ ] High-Speed storage (MicroSD/SSD)      │
      │  [ ] Stable, High-Amperage Power Supply    │
      └─────────────────────┬─────────────────────┘
                            ▼
      ┌─────────────────────┴─────────────────────┐
      │  Output & Interfacing                     │
      │  [ ] Display Cable (HDMI/Micro-HDMI)      │
      │  [ ] Input devices (Keyboard/Mouse)       │
      │  [ ] Network access (Ethernet/Wi-Fi)      │
      └─────────────────────┬─────────────────────┘
                            ▼
      ┌─────────────────────┴─────────────────────┐
      │  Thermals & Enclosure                     │
      │  [ ] Passive Heatsink / Active Cooler     │
      │  [ ] Protective Enclosure Case            │
      └───────────────────────────────────────────┘
```

---

## 💾 1. Storage Media: MicroSD Cards vs. SSDs

The storage drive acts as the boot drive and system hard disk. Because Raspberry Pi does not contain built-in flash storage (except for some custom industrial Compute Module variations), your drive speed directly limits overall performance.

### Option A: MicroSD Card (Standard)
*   **Minimum Capacity:** 16GB (for Lite headless OS), **32GB to 64GB recommended** (for Desktop OS and projects).
*   **Speed Rating:** Must be at least **Class 10**, with **UHS-I (U3)** and **Application Performance Class A1 or A2** ratings highly recommended.
*   *Why A1/A2?* Standard SD cards optimize for sequential write (cameras). A1 and A2 cards optimize for high random Read/Write operations (IOPS), which is critical for operating system tasks.

### Option B: USB 3.0 SSD or PCIe NVMe SSD (Advanced/High Performance)
*   **Compatibility:** Raspberry Pi 3 (USB 2.0), Raspberry Pi 4 (USB 3.0), and Raspberry Pi 5 (USB 3.0 or PCIe M.2 HAT).
*   **Advantages:** 5x to 20x faster read/write speeds, extreme durability, and virtually immune to the filesystem corruption common with long-term SD card write cycles.

---

## ⚡ 2. Power Supply Unit (PSU): The Critical Element

> [!WARNING]
> Do not use a generic mobile phone charger or a standard PC USB port to power your Raspberry Pi! 
> Mobile chargers are designed to charge batteries and often drop voltage under load, leading to under-voltage errors, CPU throttling, and system crashes.

Your power supply must deliver a stable, tightly regulated voltage and supply enough amperage to support both the board and connected USB peripherals.

| Board Model | Connector Type | Required Voltage | Required Current | Recommended Power Supply | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Raspberry Pi 5** | USB Type-C | 5.1V | 5.0A (27W) | Official Raspberry Pi 27W USB-C PD PSU | Requires a 5A-compatible USB-C cable and Power Delivery negotiation. If a standard 3A PSU is used, USB peripheral current is automatically limited to 600mA. |
| **Raspberry Pi 4 B** | USB Type-C | 5.1V | 3.0A (15.3W) | Official Raspberry Pi 15W USB-C PSU | Extremely sensitive to voltage drops. A 5.1V supply is used to compensate for cable resistance. |
| **Raspberry Pi 3 B+** | Micro-USB | 5.1V | 2.5A (12.75W) | Official Raspberry Pi 2.5A Micro-USB PSU | Thin charging cables cause heavy voltage drops. Use a power supply with a built-in thick-gauge cable. |
| **Pi Zero 2 W** | Micro-USB | 5.0V / 5.1V | 2.0A (10W) | Standard 5V 2A USB power adapter | Very energy efficient. Can be powered off a standard power bank. |

---

## 🖥️ 3. Cables and Peripheral Interfacing

Depending on your model, the display ports differ significantly:

*   **Raspberry Pi 5 & 4:** Dual **Micro-HDMI** ports. You will need a Micro-HDMI to Standard HDMI cable (or adapter) to connect to standard monitors.
*   **Raspberry Pi 3 & earlier:** Standard **HDMI** port.
*   **Raspberry Pi Zero / Zero 2 W:** A single **Mini-HDMI** port and a **Micro-USB OTG** port (requires an OTG adapter cable to connect standard USB-A devices).

---

## ❄️ 4. Thermal Management & Cooling Systems

Modern Raspberry Pi models pack immense processing power into a tiny form factor, generating significant heat. When the SoC temperature reaches **80°C (176°F)**, the system engages **thermal throttling**, intentionally slowing down the CPU frequency to protect the hardware.

```
SoC Temperature
  ┌───────────────┐
  │  Under 60°C   │  ►  Optimal Operating Range (Full performance)
  └───────┬───────┘
          ▼
  ┌───────────────┐
  │   60°C-80°C   │  ►  Warm (Runs normally, close to thermal thresholds)
  └───────┬───────┘
          ▼
  ┌───────────────┐
  │   Over 80°C   │  ►  THERMAL THROTTLING ACTIVATED (CPU clocks drop from 2.4GHz to 1.5GHz or lower)
  └───────────────┘
```

### Cooling Approaches:
1.  **Passive Cooling (Silent):** Uses solid aluminum or copper heatsinks attached to the SoC, RAM, and PMIC using thermal tape. Ideal for silent deployments (e.g., media players, home automation) on Raspberry Pi 4 and below.
2.  **Active Cooling (High Performance):** Combines a physical heatsink with a small fan connected to the Pi’s GPIO 5V/3.3V pins or a dedicated PWM fan header.
    *   **Recommended for Pi 5:** The **Official Raspberry Pi Active Cooler** (an integrated aluminum heatsink and temperature-controlled blower fan) is highly recommended.
    *   **Recommended for Pi 4:** The **FLIRC Case** (a fully metal enclosure serving as a giant passive heatsink) or an active ICE Tower cooler.

---

> [!TIP]
> Ready with all your hardware components? Proceed to **[OS Installation](file:///d:/Coding/Programming%20Languages/OS/Raspberry_Pi/Setup/os_installation.md)** to choose your Operating System and flash your boot drive.
