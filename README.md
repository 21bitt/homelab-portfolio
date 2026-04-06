# Homelab journal

I’m building a **residential lab** to grow toward two roles I care about: **network technician** and **SOC analyst**. The network is the classroom—routing, segmentation, and DNS are as important to me as the detection stack on top. I want to show that I can design something defensible, operate it, and explain *why* it’s built that way, not just paste screenshots.

**What is public here**

This repo is intentionally **abstract**: design intent, trust boundaries, and learning narrative—**not** live IPs, VLAN numbers, port maps, or configs. See **[public portfolio scope](docs/portfolio-scope.md)** for what belongs in GitHub vs private notes.

**What I’m aiming for**

- Solid layer-3 and VLAN discipline: clear boundaries between trusted users, servers, IoT, and guests, with least privilege between them.
- An edge I control: OPNsense for policy, NAT, VPN, and eventually Suricata in a state I can tune without taking the house offline for a week.
- Visibility that matches how SOC work actually feels—logs that answer questions, alerts I can triage, and tools (Wazuh, DNS insight, and room for NSM or honeypots later) that don’t run on vibes alone.
- A habit of writing things down: design, test, rollback plan, and what broke. That’s as much the portfolio as the VLANs.

**Where this repo fits**

The [topology](docs/topology.md), [asset notes](docs/assets.md), and [wireless policy](docs/wireless.md) describe logical design. **[Study notes](docs/study/)** capture organized learning (L2/L3, segmentation, etc.) for recruiters and future me. Ongoing lab summaries live under [`progress/`](progress/) as dated `.md` files (kept high-level per [portfolio scope](docs/portfolio-scope.md)). Copy [`progress/TEMPLATE-milestone.md`](progress/TEMPLATE-milestone.md) for new entries.

**Rough direction on the bench**

UniFi switching and WiFi tie VLANs to the access layer; a dual-NIC host runs OPNsense at the edge. Down the road: central logging and agents, DNS filtering with something I can query, optional NSM or deception on isolated segments, and private storage for configs and snapshots so I can roll back when an experiment goes wrong.

I’m early in the build. Commits here are honest progress, not a finished datacenter—and that’s the point.
