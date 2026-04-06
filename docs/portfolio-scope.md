# Public portfolio scope

This repository is a **recruiter- and peer-facing** record of **design intent** and **how I think** about a home lab. It is **not** a live runbook.

## What belongs here (low risk, high signal)

- **Goals** — roles you are building toward (e.g. network operations, security operations).
- **Architecture in words** — edge vs access vs segments, trust zones, “why OPNsense + UniFi,” future tools as *categories* (Suricata, Wazuh, NSM).
- **Design decisions** — WPA3 policy, isolated backup/storage segment, honeypot on its own VLAN, least privilege between zones.
- **Process** — milestones, verification mindset, rollback thinking, questions you asked yourself while learning.
- **Study notes** (`docs/study/`) — conceptual layering and security *ideas* only; still no live numbering or exports.
- **Product families** without serial numbers — e.g. “UniFi switch and WiFi 7 AP,” “dual-NIC OPNsense box.”

## What stays out of this repo (private runbooks only)

| Never commit or publish | Why |
|-------------------------|-----|
| **Public IPs**, **dynamic DNS names**, **real WAN details** | Recon and targeting |
| **Internal subnets**, **VLAN IDs**, **interface names** from live configs | Maps your network |
| **Switch port numbers**, **rack positions**, **room layout** | Physical targeting; low value to readers |
| **Hostnames**, **usernames**, **email addresses**, **API keys**, **tokens** | Identity and takeover |
| **Config exports**, **certificates**, **keys**, **`.ovpn` / WireGuard files** | Direct abuse |
| **Backup filenames** that echo real device names | Information leakage |
| **Exact** firewall rule dumps | Operational map |

Sanitized or fictional examples are fine **only** if clearly labeled and not copy-pasted from production.

## Progress notes (`progress/`)

Milestone files should read like **engineering summaries**: objective, assumption, what changed at a **high level**, verification approach. They should **not** read like a paste from your private documentation.

If you are unsure, **generalize** (“uplink port to the firewall”) instead of **specifics** (“port 8”).

## How this ties to interviews

Readers learn: **threat thinking**, **segmentation mindset**, **tool familiarity**, **documentation habit**. They do **not** need your **addressing plan** to assess that.
