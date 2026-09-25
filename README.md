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
    <img src="https://skillicons.dev/icons?i=raspberrypi,linux,github&theme=dark" alt="Tech Stack">
  </a>

  <br><br>

  <img src="https://img.shields.io/badge/-Zigbee-E52B50?style=flat-square&logo=zigbee&logoColor=white">
  <img src="https://img.shields.io/badge/-AdGuard_Home-68BC71?style=flat-square&logo=adguard&logoColor=white">
  <img src="https://img.shields.io/badge/-Mosquitto-3C873A?style=flat-square&logo=eclipse-mosquitto&logoColor=white">

</div>



<br>



<!-- TABLE OF CONTENTS -->

<div align="center">



**[📊 Current Status](#current-status) · [🧩 Stack](#stack) · [🖧 System Topology](#system-topology) · [💡 Devices](#devices) · [🤖 Automations](#automations) · [🧭 Architecture Decisions](#architecture-decisions) · [🗺️ Roadmap](#roadmap) · [💰 Project Investment](#project-investment)**



</div>



<br><hr><br>



<a id="current-status"></a>

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

### Recent updates

- **Sept 2026** — Repository initialized; secrets externalized from `configuration.yaml`.

<br><hr><br>



<a id="stack"></a>

# 🧩 Stack



> Each layer chosen for a reason, not just because it's popular right now.



| Layer | Technology | Purpose |
|:---|:---|:---|
| Hardware | Raspberry Pi 4 (8 GB) + 240 GB SSD, USB boot | Headless home server; SSD avoids microSD wear and I/O bottlenecks |
| Zigbee coordinator | SONOFF Zigbee 3.0 USB Dongle | Own coordinator hardware, no dependency on a closed integration |
| Automation engine | Home Assistant Core | Central automation and state engine |
| Device network | Zigbee2MQTT + Mosquitto | Decouples the Zigbee stack from HA core via MQTT |
| DNS / filtering | AdGuard Home | Network-wide filtering for every device on the LAN |
| Remote access | Tailscale (WireGuard mesh) | Remote access without exposing ports to the internet |
| Configuration management | Git + GitHub, VS Code Remote-SSH | Version-controlled configuration, edited directly on the host |
| Secrets management | `secrets.yaml` / `secrets.yaml.example` | Credentials kept out of version control, validated with `ha core check` |
| Persistence | Recorder (SQLite) | State history with a bounded retention policy |



<br><hr><br>



<a id="system-topology"></a>

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

PC["Desktop PC"]

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



HA -->|Wake-on-LAN| PC

HA -->|SSH shutdown| PC

```



<br><hr><br>



<a id="devices"></a>

# 💡 Devices



> What's actually deployed and controlled today.



| Device | Location | Protocol | Function |
|:---|:---|:---|:---|
| Paulmann 50127 Zigbee Bulb | Bedroom table lamp | Zigbee | On/off, RGBW color control |
| SONOFF MINI-D relay | Bedroom ceiling light | Zigbee | Switched via a Jung double push button |
| Desktop PC | Bedroom | Wake-on-LAN + SSH | Remote power on/off; remote desktop control via RustDesk |



<br><hr><br>



<a id="automations"></a>

# 🤖 Automations



> Two automations running today.



### Table lamp — solar elevation trigger

- **Trigger:** sun elevation drops below 3°, and the Desktop PC is powered on.

- **Action:** turns on the bedroom table lamp with a smooth brightness transition.



### Double push button — light and color control

- **Trigger:** single press on the Jung double push button.

- **Action:** toggles the bedroom ceiling light on/off.

- **Trigger:** quick double press.

- **Action:** cycles the table lamp's color.



<br><hr><br>



<a id="architecture-decisions"></a>

# 🧭 Architecture Decisions



> Short-form record of the trade-offs behind this setup — not just what was built, but why. Full write-ups live in [`docs/adr/`](docs/adr).



| ID | Title | Status |
|:---|:---|:---:|
| [0001](docs/adr/0001-tailscale-over-port-forwarding.md) | Tailscale over port forwarding | Accepted |
| [0002](docs/adr/0002-zigbee-over-wifi-and-matter.md) | Zigbee over Wi-Fi (and Matter) for device connectivity | Accepted |
| [0003](docs/adr/0003-zigbee2mqtt-over-zha.md) | Zigbee2MQTT over Home Assistant's native ZHA integration | Accepted |
| [0004](docs/adr/0004-vscode-remote-ssh-over-code-server.md) | VS Code Remote-SSH over code-server | Accepted |



<br><hr><br>



<a id="roadmap"></a>

# 🗺️ Roadmap

> Prioritized by impact vs. effort.

- [ ] Split `configuration.yaml` into domain-specific files
- [ ] Harden the SSH shutdown command (fixed `known_hosts` + restricted `authorized_keys`)
- [ ] Assign areas and rename entities
- [ ] Reusable blueprint for the double push button
- [ ] Dashboard with Mushroom Cards (HACS)
- [ ] Automated backups
- [ ] pre-commit + detect-secrets
- [ ] GitHub Actions (yamllint)


<br><hr><br>



<a id="project-investment"></a>

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