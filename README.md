<!-- HERO SECTION -->
<div align="center">
  
  <img src="https://brands.home-assistant.io/homeassistant/icon@2x.png" alt="Home Assistant Logo" width="140">

  <br><br>

  <img src="https://img.shields.io/badge/Core-Home_Assistant-41bdf5?style=for-the-badge&logo=home-assistant&logoColor=white" alt="Home Assistant">
  <img src="https://img.shields.io/badge/Network-Tailscale_VPN-orange?style=for-the-badge&logo=tailscale&logoColor=white" alt="VPN">

  <h3>Building a private, self-hosted, and locally controlled smart home.</h3>
  
</div>

<br>

<!-- TECH STACK -->
<div align="center">

  <p><strong>CURRENT INFRASTRUCTURE</strong></p>

  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=raspberrypi,linux,github&theme=dark" alt="Tech Stack" />
  </a>

  <br><br>

  <img src="https://img.shields.io/badge/-Zigbee-E52B50?style=flat-square&logo=zigbee&logoColor=white">
  <img src="https://img.shields.io/badge/-AdGuard_Home-68BC71?style=flat-square&logo=adguard&logoColor=white">
  <img src="https://img.shields.io/badge/-Tailscale-5468FF?style=flat-square&logo=tailscale&logoColor=white">

</div>

<br><hr><br>

# 📊 Current Status

> Building a solid foundation: infrastructure first, devices second.

| Area | Status | Progress |
|:---:|:---:|:---:|
| 🧠 Core Services | Operational | ![Progress](https://geps.dev/progress/90?dangerColor=808080&warningColor=808080&successColor=41bdf5) |
| 📻 Zigbee Network | Expanding | ![Progress](https://geps.dev/progress/20?dangerColor=808080&warningColor=808080&successColor=41bdf5) |
| 🤖 Automations | Early Development | ![Progress](https://geps.dev/progress/10?dangerColor=808080&warningColor=808080&successColor=41bdf5) |

### Currently running

- 🧠 Home Assistant as the central automation platform.
- 🌍 Secure remote access via Tailscale VPN.
- 🛡️ DNS-level network filtering with AdGuard Home.
- 💡 First Zigbee-enabled smart room.

<br><hr><br>

# 🖧 System Topology

> Current architecture of the smart home infrastructure.

```mermaid
flowchart LR

subgraph Client["👤 Client"]
User["Mobile / PC"]
end

subgraph Network["🌐 Network"]
VPN["Tailscale VPN"]
Router["Router"]
end

subgraph Server["🧠 Server"]
HA["Home Assistant"]
AG["AdGuard Home"]
Z2M["Zigbee2MQTT"]
end

subgraph Zigbee["📻 Zigbee Network"]
Dongle["SONOFF Zigbee Dongle"]
end

subgraph Bedroom["🛏️ Bedroom"]
Mini["SONOFF MINI-D"]
Bulb["Paulmann Zigbee Bulb"]
Switch["Jung Push Button"]
end

User --> VPN
VPN --> Router
Router --> HA

HA --> AG
HA --> Z2M
Z2M --> Dongle

Dongle --> Mini
Dongle --> Bulb

Switch -. controls .-> Mini
Mini --> Bulb
```

<br><hr><br>

# 💰 Project Investment

> Hardware purchased to build the smart home infrastructure.

| Date | Component | Purpose | Cost |
|:---|:---|:---|---:|
| March 2026 | Raspberry Pi 4 (8 GB) | Home Assistant server | €30.00 |
| March 2026 | 5V 3A Power Supply | Raspberry Pi power supply | €9.99 |
| March 2026 | **SanDisk 240 GB SSD** *(reused)* | System storage | €0.00 |
| April 2026 | **SONOFF Zigbee 3.0 USB Dongle** | Zigbee network coordinator | €25.13 |
| April 2026 | USB Extension Cable *(reused)* | Dongle positioning | €0.00 |
| April 2026 | **UGREEN USB-to-SATA Adapter** | SSD connection | €11.89 |
| April 2026 | **Paulmann 50127 Zigbee Bulb** | First smart lighting device | €11.99 |
| May 2026 | **SONOFF MINI-D** | Smart switch relay | €18.20 |
| June 2026 | **Jung LS** Single Frame (Matte White) | Switch finish | €9.92 |
| June 2026 | **Jung LS** Double Rocker (Matte White) | Double push button | €14.99 |
| June 2026 | **Jung 535U** Push Button Mechanism | Momentary switch mechanism | €13.33 |
| July 2026 | Aluminum Raspberry Pi Case | Passive cooling | €13.99 |

---

## 📊 Summary

| Category | Total |
|:---|---:|
| 🖥️ Infrastructure | **€65.87** |
| 📻 Zigbee Network | **€25.13** |
| 💡 Smart Lighting | **€11.99** |
| 🔘 Smart Switching | **€56.44** |
| **💰 Total Investment** | **€159.43** |

> **Note:** Some components were reused from previous hardware and therefore do not contribute to the total project cost.
