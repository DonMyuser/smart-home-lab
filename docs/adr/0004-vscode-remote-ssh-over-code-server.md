# 0004 - VS Code Remote-SSH over code-server

* **Status:** Accepted
* **Date:** 2026-09

## Context

Home Assistant configuration files need to be edited directly on the Raspberry Pi using a full-featured editor with YAML validation, syntax highlighting, and integrated terminal access, going beyond the basic capabilities of the built-in File Editor add-on.

## Alternatives Considered

- **code-server:** Running a web-based VS Code instance directly on the Raspberry Pi, accessed via a web browser.
- **VS Code Remote-SSH extension:** Running the editor client locally on the host machine and connecting securely to the Pi over SSH.

## Decision

Use **VS Code with the Remote-SSH extension** on the local machine, connecting to the Raspberry Pi over the established Tailscale network.

## Consequences

- **Reduced System Overhead:** The editor GUI runs locally on the host machine, saving CPU and RAM resources on the Raspberry Pi server.
- **Minimal Attack Surface:** Leverages the existing SSH daemon over Tailscale without exposing an additional web server or port (see ADR 0001).
- **Git Workflows:** Seamlessly integrates with the local Git client for committing and pushing configuration changes to GitHub via SSH keys.
- **Tooling Requirement:** Requires VS Code and the Remote-SSH extension to be installed on the client machine; quick edits from an arbitrary web browser are not possible compared to `code-server`.