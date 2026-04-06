# Network topology

The lab is built around a single edge firewall so routing policy, NAT, and inspection stay in one place. Downstream gear handles switching and wireless; VLANs are defined on the firewall and carried over a trunk to the access layer.

**Edge:** OPNsense on a dedicated dual-NIC host. The WAN interface faces the ISP gateway (DHCP); the LAN side feeds a managed switch. Suricata runs here for IDS/IPS once the baseline is stable.

**Access:** A UniFi switch uplinks to the firewall (trunk if multiple VLANs, or a flat LAN during early setup). A UniFi U7 Lite access point hangs off the switch with PoE. SSIDs map to VLANs so wireless clients land in the same trust zones as wired ones.

**Segments (intent):** **Family** (trusted users), **guest**, **IoT**, **honeypot** (deliberately vulnerable lab targets), **management**, and **cloud storage** each get their own VLANs with non-overlapping private address space. Lower-trust segments (guest, IoT, honeypot) do not reach management, family, or storage unless a rule explicitly allows it—usually DNS/NTP only where needed.

**Cloud storage VLAN:** Backups include security appliance and tooling configuration exports; that data is treated as high sensitivity. The storage segment is isolated so east–west access from IoT, guest, and honeypot is denied by default.

If a UniFi Cloud Gateway is used in series with OPNsense, that adds another routing hop (often double NAT). That is fine for learning if it is documented; port forwards and remote access need to be traced through both layers.

Later, traffic mirroring on the switch can feed a sensor or NSM stack for full packet capture on selected VLANs.

```mermaid
flowchart TB
    WAN[ISP gateway]
    FW[OPNsense firewall]
    SW[Managed switch]
    AP[WiFi access point]
    SRV[Servers and VMs]
    IOT[IoT VLAN]
    ADM[Admin workstations]

    WAN --> FW
    FW --> SW
    SW --> AP
    SW --> SRV
    SW --> IOT
    SW --> ADM
```

Details that change often (exact VLAN IDs, interface names, and addressing) stay in private runbooks and sanitized configs—not in this public repo.

**Public documentation:** This repo states **design intent** (roles, trust boundaries, tool categories). It does **not** include live subnets, hostnames, credentials, VPN endpoints, or exports. Publishing that level of detail would add risk without helping a portfolio reader; keeping this layer abstract is deliberate and aligns with common practice for public write-ups.
