# Homelab Portfolio — SOC & Network Engineering

Personal portfolio project: design, harden, segment, and monitor a home network while documenting decisions like production engineering work.

## Goals

- **Network technician:** VLANs, subnetting, routing, DNS, VPN, troubleshooting, and clean documentation.
- **SOC analyst:** Visibility (logs, alerts), detection (IDS/IPS, SIEM-style workflows), and triage (hypothesis → evidence → conclusion).

## Repository layout

| Path | Purpose |
|------|---------|
| `docs/assets.md` | Inventory of hardware and software (source of truth). |
| `docs/topology.md` | Logical network design and VLAN intent. |
| `reports/` | Milestone write-ups (design, verification, rollback, lessons learned). |

Each milestone should be a **merge request** or a dated folder under `reports/` with enough detail that someone else could reproduce or audit your work.

## Quick links

- [Asset list](docs/assets.md)
- [Topology](docs/topology.md)

## Stack (high level)

- **Edge:** OPNsense on a dual-NIC host — firewall, routing, VPN, Suricata when tuned.
- **Access:** UniFi switch and WiFi 6 AP — trunks, PoE, SSIDs mapped to VLANs.
- **Optional:** UniFi Cloud Gateway — see [topology](docs/topology.md) if both UniFi and OPNsense are in path.
- **Visibility:** Wazuh, DNS filtering / logging, optional NSM and honeypots on isolated VLANs.
- **Backups:** Private cloud storage for configs and snapshots (not committed here).

## How to use this repo

1. Update `docs/assets.md` whenever you add or retire a system.
2. After each stable change, add a **report** under `reports/YYYY-MM-DD-short-name/` with objective, design, verification, and rollback.
3. **Never commit secrets** — use placeholders and sanitize configs (see `.gitignore`).

## Author

Aspiring SOC analyst & network technician — progress documented for learning and hiring conversations.
