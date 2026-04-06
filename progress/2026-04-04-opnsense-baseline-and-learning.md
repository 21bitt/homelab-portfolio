# Milestone: OPNsense baseline and networking mental model

> Conceptual learning notes only—no live subnets, device names, or exports. See [`docs/portfolio-scope.md`](../docs/portfolio-scope.md).

**Date:** 2026-04-04  
**Status:** draft

## Objective

Stand up OPNsense as the lab edge and build a clear, accurate mental model of how management access and WAN inbound policy work before adding VLANs, UniFi trunking, and segmentation.

## Threat / operations assumption

Management interfaces (web GUI, SSH) are high-value targets. Exposing them to the internet multiplies risk without adding day-one value. Understanding default-deny behavior on WAN avoids both fear (“no rules = open”) and complacency (“I will add explicit deny lines for everything”). Documenting questions shows learning trajectory—useful for interviews and future you.

## Design

- VLANs / subnets affected: none yet; flat LAN / wizard baseline only.
- Firewall rule philosophy (least privilege): treat unsolicited **inbound from WAN** as blocked unless explicitly allowed; keep GUI and SSH on **listen scopes** and networks you control; add inter-VLAN rules only after trunking and DHCP per segment are stable.
- Dependencies (other services): ISP CPE → OPNsense WAN; LAN clients for validation; future UniFi Network (controller / console) for switch port profiles and SSID → VLAN mapping.

## Implementation summary

- Installed OPNsense and completed the **initial setup wizard** on the edge host (dual-NIC).
- Reviewed **System → Settings → Administration** and the **Listen interfaces** option under **Web GUI** to bind management to **LAN only** (recommended after backup; official docs warn about static/stable interfaces when binding).
- Confirmed **Firewall → Rules → WAN** can show **no user rules** while still enforcing **default deny** for new inbound sessions from the internet.
- Clarified **UniFi** topology of control: configuration (e.g. **trunk** on the port toward OPNsense) is done in **UniFi Network** (browser or app tied to a **controller / console**), not by guessing a generic “switch UI IP.” The switch has a LAN IP for adoption; the **management UI** is the controller/console (or app) you use for UniFi.

## Verification

- Tests run (ping, DNS, blocked path, expected alert):
  - Wizard completed; LAN clients can reach the internet through OPNsense (as experienced during setup).
  - Conceptual check: **no broad “allow all inbound” on WAN**; optional Live View / external probe later for extra assurance.
- Evidence (screenshot references or log excerpts — no secrets):
  - None stored in-repo for this milestone; config backup recommended before binding GUI to specific interfaces.

## Rollback

- Restore prior **configuration XML** from **System → Configuration → Backups** if GUI or SSH binding causes lockout; physical console access as last resort per OPNsense documentation.

## Questions explored (and short answers)

1. **Why use the web UI from LAN only / why restrict the GUI to LAN?**  
   The GUI is full administrative control. Using it from **inside** the trusted network is the normal case. Restricting **where the service listens** (e.g. LAN only) and **not** exposing management on WAN reduces attack surface; remote access later should prefer **VPN into LAN**, not opening the GUI on the internet.

2. **How do I “restrict GUI to LAN” and keep WAN safe?**  
   **Listen interfaces** (Web GUI) limits where the daemon binds. **WAN** inbound is already **default deny** for unsolicited new connections; empty WAN rules does **not** mean “no firewall.”

3. **Should there be explicit rules like “deny WAN → gateway” for the GUI?**  
   Usually **not required** for baseline security: without an **allow** on WAN, unsolicited inbound is not passed. **Listen interfaces** adds another layer. Explicit **block** rules can still be used for **documentation, logging, or** guarding against a future overly broad **allow** rule.

4. **Is this a whitelist approach by default?**  
   **Yes**, in the sense that **new inbound from WAN** is not allowed unless you add something that passes it. **Outbound** sessions you start still work via **stateful** tracking and LAN-side rules.

5. **What is the “UniFi switch UI IP” for trunk setup?**  
   There is no universal switch-admin URL for this. **Trunk / port profiles** are set in **UniFi Network** management (wherever the controller runs—gateway, Cloud Key, or host). The switch’s IP is for operation/adoption, not the primary story for “where do I click for trunk.”

## Lessons learned

- An **empty WAN rule list** can coexist with **strong default inbound posture**; the missing piece was understanding **implicit deny** and **stateful** outbound use—not a need to paste “deny” lines everywhere.
- **Terminology matters:** “UniFi UI” in lab notes meant **UniFi Network management**, which avoided a wild goose chase for a non-existent standalone switch portal.
- Next milestone should be **physical path validation** (OPNsense LAN → switch), then **VLANs + trunk + DHCP**, then **inter-VLAN rules**—one variable at a time.

## Next steps

1. Download and store a **sanitized** config backup after any Listen-interface change.
2. Connect **UniFi switch** uplink to OPNsense **LAN**; confirm stable **flat** operation before VLANs.
3. Define VLAN intent on paper; implement VLANs on OPNsense and **trunk** on the switch port to the firewall; then SSIDs and **least-privilege** rules between segments.
