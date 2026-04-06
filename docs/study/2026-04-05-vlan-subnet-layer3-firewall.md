# Study: VLANs, subnets, and L3 firewalls

**Date:** 2026-04-05  
**Audience:** Self — conceptual notes for interviews and lab design (no live addressing). Aligns with [public portfolio scope](../portfolio-scope.md).

---

## 1. VLAN vs subnet (different layers)

| | **VLAN** | **Subnet** |
|---|----------|------------|
| **Layer** | **L2** (switching / Ethernet broadcast domain) | **L3** (IP addressing and routing) |
| **Implemented by** | Switches, APs (802.1Q tags; SSID → VLAN) | Routers / firewalls (interfaces, DHCP, routes) |
| **Main job** | Separate **who shares the same switched “wire”** | Define **which IPs are local** vs **must go through a router** |

- A **VLAN** does not assign IP addresses by itself; it groups **ports and wireless networks** into **different broadcast domains**.
- A **subnet** is a **block of IP addresses** (e.g. a `/24` prefix). Same subnet → hosts expect **direct** L3 delivery; different subnet → traffic goes to a **default gateway**.

**Mental model:** VLAN = **separate hallways** (L2). Subnet = **address ranges on the map** (L3).

---

## 2. How VLAN and subnet work together

Typical pattern:

1. **VLANs** on the access layer isolate **Ethernet/WiFi segments** (IoT, guest, trusted users, etc.).
2. The **firewall/router** has a **logical interface per VLAN**, each with its **own subnet** (one subnet per VLAN is the usual rule of thumb).
3. **DHCP** hands out addresses **in that subnet** only to clients on that VLAN.

Result:

- **L2 separation** reduces shared broadcast scope and keeps **zones** on different switch/AP lanes.
- **L3 separation** gives the firewall **clear source/destination networks** for policy: “from this subnet to that subnet: allow or deny.”

**Security takeaway:** VLANs **structure** the network; subnets **label** each segment for **routing and rules**. Neither replaces the other.

---

## 3. Hardening: what each piece contributes **alone**

**VLAN only (tagged, separate domains):**

- Limits **broadcast/multicast scope** and keeps low-trust devices off the **same L2 segment** as high-trust devices (helps with some L2 noise and misconfiguration blast radius).
- Does **not** stop **IP traffic between zones** unless traffic is **routed** and a **policy** applies.

**Subnet only (no VLAN separation on the wire):**

- You can still **plan** IP space, but if everyone shares **one flat VLAN**, you lose **clean physical/wireless separation** at L2.

**Firewall (L3 device) between subnets:**

- Enforces **packet** (and stateful connection) rules when traffic **crosses** from one subnet to another **through the router**.
- **VLANs + subnets** make those rules **simple to write and audit** (explicit zones).

**Critical:** Misconfigured **allow** rules between VLANs (e.g. wide “any to any”) **undo** segmentation. **Default deny** between untrusted and trusted zones, with **explicit** exceptions, is the usual goal.

---

## 4. Does a firewall “fine-tune” rules because of VLANs?

The firewall engine does not change; **VLANs change how many interfaces and networks you have**.

- Each **VLAN terminated on the firewall** = **another L3 interface** and **another subnet**.
- Rules attach to **interfaces** and **source/destination** address groups — so VLANs enable **finer policy** by giving **distinct sources/destinations**, not a different filtering algorithm.

---

## 5. Frames vs packets — “filtered twice”?

**Terms:**

- **Frame** — L2 (Ethernet).
- **Packet** — L3 (IP); what most **firewall filter rules** match when traffic is **routed**.

**Subnetting does not add a second pass of “frame filtering” after packet filtering** in a normal routed design. Traffic crossing from one VLAN/subnet to another is evaluated at the **router/firewall** for that **forwarding path** (stateful rules, etc.). You do not typically stack “filter frames, then filter packets, then filter frames again” on the same hop.

**What subnetting adds:** **clarity** for rules and logs (“from IoT range to management range: deny”), not an extra **Ethernet** filter pass.

Optional **additional** inspection (e.g. IDS/IPS) is a **separate feature**, not “subnetting doing double frame filtering.”

---

## 6. One-sentence summary

**VLANs** separate **L2 domains**; **subnets** give each domain a **distinct IP neighborhood**; the **L3 firewall** enforces policy when traffic **moves between** those neighborhoods — **structured segmentation plus explicit allow/deny**, not redundant frame-level filtering for the same hop.
