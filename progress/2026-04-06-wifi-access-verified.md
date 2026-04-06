# Milestone: WiFi access verified (flat LAN)

**Date:** 2026-04-06  
**Status:** verified

## Objective

Confirm wireless clients receive DHCP from OPNsense and reach the internet through the same edge path as wired clients, before introducing VLANs and trunking.

## Implementation summary

- **Switch uplink:** port **8** to **OPNsense LAN** (management PC and other wired devices on the switch).
- **UniFi:** Switch Ultra and **U7 Lite** adopted; firmware current; dashboard used for management.
- **WiFi:** Internet access confirmed from wireless clients on the flat LAN (SSID on default/LAN scope).

## Verification

- Wired path previously verified; **WiFi** now matches—clients on the same logical network behind OPNsense.

## Next steps

1. Document or label **port 8** in UniFi as the future **trunk** to OPNsense when VLANs are added.
2. **Paper plan:** VLAN names, purposes, internet vs isolated.
3. **OPNsense:** VLANs + DHCP on the LAN parent toward the switch.
4. **UniFi:** port **8** → **trunk** (tagged VLANs); map SSIDs/access ports; then **firewall** rules between segments.
