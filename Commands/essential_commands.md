# Essential Commands & Configuration Handbook

Once you are connected to your Raspberry Pi via terminal (SSH), you can perform updates, check hardware health, modify system settings, and manage running software. 

This handbook is a structured quick-reference reference sheet for standard Debian Linux commands alongside Raspberry Pi-specific diagnostics.

---

## 🔄 1. Package Management (Keeping Your Pi Updated)

Raspberry Pi OS is based on Debian Linux, which utilizes the `apt` (Advanced Package Tool) package manager. Running updates regularly secures your system and installs stability patches.

```bash
# 1. Update the local package index (checks what packages have newer versions)
sudo apt update

# 2. Upgrade all installed packages to their latest versions
sudo apt full-upgrade -y

# 3. Clean up obsolete packages and free up disk space
sudo apt autoremove -y
sudo apt clean
```

> [!IMPORTANT]
> `sudo apt upgrade` and `sudo apt full-upgrade` differ: `full-upgrade` is smarter. It is capable of resolving dependency changes, installing new dependencies, and safely removing obsolete packages if required by a kernel update. Always use `full-upgrade` on Raspberry Pi.

---

## ⚙️ 2. The Configuration Tool (`raspi-config`)

The `raspi-config` utility is an official console-based menu system that simplifies system configurations without manually editing complex configuration files.

```bash
# Open the interactive configuration menu
sudo raspi-config
```

### Useful CLI Commands (Non-Interactive `raspi-config` scripts):
If you are writing scripts or prefer not to use the visual menu, you can execute configurations directly:

```bash
# Enable SSH server instantly
sudo raspi-config nonint do_ssh 0

# Disable SSH server instantly
sudo raspi-config nonint do_ssh 1

# Change the GPU memory allocation to 128MB
sudo raspi-config nonint do_gputsplit 128

# Reboot the system safely (always do this after major configuration edits)
sudo reboot
```

---

## 🌡️ 3. Hardware Diagnostics & System Health (`vcgencmd`)

`vcgencmd` is a powerful built-in command-line utility designed specifically to query the Broadcom System-on-Chip (SoC) for internal hardware statistics, thermals, voltages, and clock speeds.

```bash
# 1. Check current SoC Core Temperature (Crucial for testing cooling)
vcgencmd measure_temp

# 2. Check current CPU Core Voltage
vcgencmd measure_volts core

# 3. Check current CPU ARM Clock Speed (Output in Hertz, e.g., 2400000000 = 2.4GHz)
vcgencmd measure_clock arm

# 4. Check memory allocation split between the CPU and GPU
vcgencmd get_mem arm
vcgencmd get_mem gpu
```

### ⚡ Troubleshooting Throttling and Under-Voltage (`vcgencmd get_throttled`)
If your Raspberry Pi is lagging or experiencing stability issues, run:
```bash
vcgencmd get_throttled
```
This returns a hexadecimal status code (e.g., `0x50000` or `0x50005`). Use the binary bit breakdown below to decipher the hexadecimal code:

| Bit Position | Hex Mask | Meaning when Bit is Active (`1`) |
| :--- | :--- | :--- |
| **Bit 0** | `0x1` | **Under-voltage detected** (voltage dropped below 4.63V right now) |
| **Bit 1** | `0x2` | **ARM frequency capped** (CPU is temporarily slowed due to high temperatures right now) |
| **Bit 2** | `0x4` | **Currently throttled** (system is thermal-throttling right now) |
| **Bit 3** | `0x8` | **Soft temperature limit active** (CPU temp > 60°C, throttles slightly right now) |
| **Bit 16** | `0x10000` | **Under-voltage has occurred** since the last boot cycle |
| **Bit 17** | `0x20000` | **ARM frequency capped has occurred** since the last boot cycle |
| **Bit 18** | `0x40000` | **Throttling has occurred** since the last boot cycle |
| **Bit 19** | `0x80000` | **Soft temperature limit has occurred** since the last boot cycle |

#### Example Analyses:
*   `0x0` – Perfect. Normal operating voltage, no throttling history.
*   `0x50000` – Under-voltage and frequency capping have occurred in the past since boot, but voltage is stable right now. Check your power cable!
*   `0x50005` – **CRITICAL.** Under-voltage and throttling are actively occurring right now. Your power supply is insufficient, and the CPU has slowed down to prevent a crash.

---

## 🛠️ 4. Managing System Services (systemd)

On modern Debian-based OS versions, system services (like SSH, VNC, web servers, docker) are managed using `systemctl`.

```bash
# Check if a service is actively running (e.g., SSH)
sudo systemctl status ssh

# Start a service immediately
sudo systemctl start docker

# Stop a service immediately
sudo systemctl stop docker

# Restart a service immediately (useful after configuration edits)
sudo systemctl restart docker

# Enable a service to start automatically on system boot
sudo systemctl enable docker

# Disable a service from starting automatically on system boot
sudo systemctl disable docker
```

---

## 💾 5. Storage & Memory Inspection

Because SD cards can fill up or fail, checking your disk and RAM utilization is an important maintenance task.

```bash
# View human-readable disk space utilization for all partitions
df -h

# Check RAM utilization, cache, and swap space
free -h

# List all block storage devices (SD cards, USB drives) and their partition structures
lsblk

# Monitor CPU, RAM, and running processes in real-time
htop
```

---

## 🌐 6. Network Management (NetworkManager / nmcli)

> [!NOTE]
> In recent releases of Raspberry Pi OS (specifically Debian "Bookworm" and later), the classic `dhcpcd` networking configuration daemon has been replaced by standard Linux **NetworkManager**.

```bash
# 1. View all active and available network connections
nmcli connection show

# 2. Scan for available local Wi-Fi access points
nmcli device wifi list

# 3. Connect to a Wi-Fi network directly from CLI
sudo nmcli device wifi connect "Your_WiFi_SSID" password "Your_WiFi_Password"

# 4. View network interface details (IP addresses, MAC addresses)
ip a
```

---

## 🔌 7. Graceful Shutdown & Reboots

Never unplug the power cable of your running Raspberry Pi. Doing so risks corrupting the active system files on the SD card. Always shut down gracefully:

```bash
# Shut down the system completely and power down hardware safely
sudo poweroff
# OR
sudo shutdown -h now

# Reboot the operating system safely
sudo reboot
```
