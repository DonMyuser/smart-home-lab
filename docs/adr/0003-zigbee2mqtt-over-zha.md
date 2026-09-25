# 0003 - Zigbee2MQTT over Home Assistant's native ZHA integration

* **Status:** Accepted
* **Date:** 2026-04

## Context

A Zigbee coordinator (SONOFF Zigbee 3.0 USB Dongle) needs to be paired with Home Assistant to control Zigbee devices such as the Paulmann bulb and the SONOFF MINI-D relay.

## Alternatives Considered

- **ZHA (Zigbee Home Automation):** Home Assistant's built-in Zigbee integration, running directly inside HA Core.
- **Zigbee2MQTT:** A standalone service that interacts directly with the coordinator hardware and publishes device state over MQTT.

## Decision

Run **Zigbee2MQTT** as an independent service, bridged to Home Assistant through a **Mosquitto MQTT** broker.

## Consequences

- **Decoupled Architecture:** The MQTT layer decouples the Zigbee network from Home Assistant itself. HA can be restarted, reconfigured, or reinstalled without losing Zigbee network state or requiring re-pairing of end devices.
- **Fine-grained Control:** Direct control over the dongle's firmware, pairing process, and device binding independent of Home Assistant's release cycle.
- **Broader Device Support:** Access to Zigbee2MQTT's extensive device database and advanced configuration options for complex exposed clusters.
- **Increased Complexity:** Introduces two additional services to maintain (`Zigbee2MQTT` + `Mosquitto`) compared to ZHA's single-integration simplicity.
- **Debugging Indirection:** Device states and commands are passed through MQTT topics rather than native HA entity calls directly from the coordinator hardware.