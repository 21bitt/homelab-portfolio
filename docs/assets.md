# Hardware and services

Rough inventory for this lab. Roles matter more than model numbers for understanding the design.

**Edge**

- Dedicated **x86_64 host with two Ethernet interfaces** running **OPNsense** — routing, firewalling, VPN, and Suricata (IDS/IPS) at the perimeter.
- **ISP gateway** — consumer broadband CPE; WAN is typically DHCP and may sit behind carrier NAT depending on the provider.

**Access layer**

- **UniFi Cloud Gateway** — optional; UniFi OS and routing in one box. Placement relative to OPNsense is documented in the topology note so double NAT is not accidental.
- **UniFi Switch Ultra** — VLAN trunking, access ports, PoE for the AP.
- **UniFi U6+** — WiFi 6 coverage; WLANs tied to VLANs for segmentation.

**Endpoints**

- **Primary workstation** — day-to-day admin: SSH to appliances, browser to dashboards, terminal-based tooling. Management traffic is restricted where possible.

**Security and visibility (rolled out over time)**

- Central **DNS** (Pi-hole or resolver on the firewall) for filtering and query visibility.
- **Wazuh** for host telemetry and alerting.
- **Security Onion** or similar for NSM where resources allow.
- **Honeypot** on an isolated segment with controlled egress.
- **Remote access** via VPN or an overlay (e.g. Tailscale) instead of exposing management to the internet.

**Ops**

- **Personal cloud** (Nextcloud or equivalent) for offline backups of configs, exports, and VM snapshots.
- **Git** for this repository and change history.

Spare or retired gear gets a one-line note here when it drops out of the design so older reports still make sense.
