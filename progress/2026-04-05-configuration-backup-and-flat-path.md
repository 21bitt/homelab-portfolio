# Milestone: Configuration backup and flat path verified

**Date:** 2026-04-05  
**Status:** verified

## Objective

Preserve the working OPNsense baseline and confirm end-to-end connectivity through the access switch before introducing VLANs and trunking.

## Implementation summary

- Downloaded **current OPNsense configuration** (XML backup) for rollback and change tracking.
- Confirmed **physical path**: **ISP → OPNsense WAN → OPNsense LAN → UniFi switch → workstation** (single L3 hop at the edge; flat LAN through the switch).

## Verification

- Workstation on-path through the switch; management and general use align with the intended topology in `docs/topology.md` (edge first, switch as access layer).

## Next steps

1. On paper: list intended **VLAN names**, **purposes**, and which need **internet** vs **internal-only**.
2. **OPNsense:** create VLANs on the **parent interface** that faces the switch; assign interfaces; **DHCP** per segment as needed.
3. **UniFi:** configure the **switch port to OPNsense** as a **trunk** (all relevant VLANs **tagged**); map **access ports / SSIDs** to VLANs.
4. **Firewall:** inter-VLAN rules **after** each segment gets DHCP and basic connectivity—one change at a time.
