# 0001 - Tailscale over port forwarding

* **Status:** Accepted
* **Date:** 2026-04

## Context

The Raspberry Pi needs to be reachable from outside the home network: the Home Assistant mobile app, SSH access for maintenance, and RustDesk to remote-control the Desktop PC. The ISP-provided router made straightforward port forwarding unreliable, and setting up a self-managed WireGuard tunnel hit the same underlying problem, since it also depends on a port being consistently reachable from the outside.

## Alternatives Considered

- **Port forwarding on the router:** Exposing HA's HTTPS port (and SSH) directly to the internet — ruled out due to inconsistent behavior with this ISP's router.
- **Self-hosted WireGuard instance:** Run directly on the Pi — hit the same reachability problem, since it still needs an open, stable port to accept incoming connections.
- **Tailscale:** A WireGuard-based mesh VPN that handles NAT traversal and connection coordination without requiring an open port.

## Decision

Use Tailscale for all remote access. No port is opened on the router, and no port needs to stay reachable from the outside.

## Consequences

- Solves the actual blocker: Tailscale establishes connections via NAT traversal (falling back to a relay when direct peer-to-peer isn't possible), so the ISP's unreliable port forwarding stops being a dependency at all.
- Zero attack surface exposed to the public internet — every connection is authenticated at the VPN layer before it ever reaches Home Assistant.
- Access is scoped to devices already enrolled in the tailnet; a stolen password alone isn't enough to reach the Pi.
- Adds a dependency on Tailscale's coordination service for establishing new connections (though already-established WireGuard tunnels keep working if it's briefly unavailable).
- Every new remote-controlled device (like the Desktop PC) is added to the same tailnet instead of fighting the router for another forwarded port.