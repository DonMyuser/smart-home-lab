# 0002 - Zigbee over Wi-Fi (and Matter) for device connectivity

* **Status:** Accepted
* **Date:** 2026-04

## Context

Smart home devices (bulbs, relays, and future additions like sensors) need a wireless protocol to join the network. The main candidates were Wi-Fi-native devices, a Zigbee mesh via a dedicated coordinator, or Matter, the newer unified smart home standard.

## Alternatives Considered

- **Wi-Fi-native devices:** Joining the main home network directly.
- **Matter:** The emerging unified standard designed to work across ecosystems.
- **Zigbee:** A low-power mesh protocol requiring its own coordinator (SONOFF dongle).

## Decision

Use **Zigbee** as the primary protocol for smart home end-devices, managed through a dedicated USB coordinator.

## Consequences

- **Wi-Fi offloading:** Wi-Fi devices compete for airtime and DHCP leases on the home router. Zigbee devices form their own mesh, allowing the device count to grow without degrading local Wi-Fi performance.
- **Network resilience:** The Zigbee mesh operates independently of the Wi-Fi network. If the router or internet connection drops, local communication between devices and Home Assistant continues without interruption.
- **Matter position:** Matter was rejected for current deployment due to its maturing ecosystem, smaller device catalog, and limited real-world track record compared to Zigbee. This is an open decision to revisit in the future.
- **Trade-off:** Requires dedicated hardware (USB coordinator) and an independent pairing process, rather than leveraging the existing Wi-Fi infrastructure.