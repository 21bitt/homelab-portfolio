# Wireless LAN policy

Design choices for the **primary household SSID** on the **UniFi U7 Lite**, with reasoning. This is not a step-by-step UniFi manual—only **what** is configured and **why**.

## Configuration

- **Encryption:** **WPA3** enabled manually on the first (main) WLAN used for **general household** clients—phones, laptops, and shared use.
- **Scope:** This SSID is **not** reserved for IoT. It is the **default trusted WiFi** for people in the house.
- **PMF (802.11w):** Not treated as a blocking concern up front. If a client misbehaves with **Protected Management Frames** (required vs optional), that can be tuned later in UniFi; no need to optimize before anything fails.

## Reasoning

1. **Modern baseline** — WPA3 is the appropriate default for **current** personal devices in favor of WPA2-only on the main network.
2. **Separation of concerns** — IoT and other **low-trust or legacy** devices are expected to land on **separate SSIDs** (and eventually **VLANs**) so the **household** WLAN does not get held to the **weakest** client’s security or feature set.
3. **Pragmatism** — Prefer a strong default for everyone; address **edge-case compatibility** (old gear, game consoles, printers) only when a real device fails to connect—e.g. **WPA2/WPA3 transition** on this SSID or a **dedicated legacy SSID**, rather than weakening the primary policy preemptively.

## When to revisit

- A device **cannot associate** → consider **mixed WPA2/WPA3** or a **second SSID** with WPA2 for legacy hardware.
- Unexplained **disconnects** on one client → review **PMF** settings if available.
- When **IoT VLANs** go live → new **IoT-specific** WLANs with explicit policy, not mixed into this household SSID by default.
