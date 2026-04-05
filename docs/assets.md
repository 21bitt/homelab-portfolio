# Asset list

Maintain a single source of truth. Update this file when hardware or services change.

| Asset | Role | Notes |
|-------|------|--------|
| **T-Mobile gateway** | ISP modem / CPE | WAN uplink; may be CGNAT; double NAT possible behind OPNsense. |
| **Beelink mini PC (dual NIC)** | **OPNsense** router/firewall | Edge policy, IDS/IPS (Suricata), VPN, inter-VLAN routing. |
| **UniFi Cloud Gateway** | UniFi gateway / UniFi OS | See [topology](topology.md) — optional alternate edge or downstream placement; avoid undocumented double NAT. |
| **UniFi Switch Ultra** | Managed L2/L3-capable switch | VLAN trunk to OPNsense, PoE for AP, wired homelab devices. |
| **UniFi U6+ Access Point** | WiFi 6 AP | Map SSIDs to VLANs (e.g. LAN, IoT, guest). |
| **Custom PC** | Admin / analysis workstation | SSH, browser to UIs, MobaXterm or Windows Terminal; prefer management VLAN access. |
| **Pi-hole / DNS** (optional placement) | DNS filtering & query logging | Often VM or Pi; may integrate with OPNsense. |
| **Wazuh** (planned) | SIEM / agent telemetry | Typically one server + agents on key hosts. |
| **Security Onion** (planned) | NSM / PCAP / analyst workflows | Heavy; usually dedicated VM + mirror port if used. |
| **Honeypot** (planned) | Deception / telemetry | Isolated VLAN; document egress and safety. |
| **Nextcloud** (planned) | Personal cloud & backups | Config exports, lab notes, images, Git bundles. |
| **Tailscale / VPN** (planned) | Remote secure admin | Prefer over exposing SSH to the public internet. |

## Software / access tools (workstation)

| Tool | Use |
|------|-----|
| **MobaXterm / Windows Terminal + SSH** | Remote shell to OPNsense, Linux VMs, Pis — key-based auth. |
| **Git** | Version control for this repo and lab journals. |

## Retirement / spare

- List decommissioned gear here with date so reports stay coherent.
