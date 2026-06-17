# HostAnywhere Documentation

> **HostAnywhere** is a secure access platform that connects your devices and exposes your services across macOS, Windows, Linux, iOS, and Android — with one agent, one console, and a single set of policies enforced everywhere.

This repo is the canonical Markdown mirror of [hostanywhere.io/docs](https://hostanywhere.io/docs). It exists so you can search, cite, or fork the documentation without scraping the live site, and so the docs are easy to find on GitHub.

---

## What HostAnywhere does

HostAnywhere combines four capabilities that are usually four separate products:

| Capability | What it does | Typical alternative |
|---|---|---|
| **Private mesh network** | Every device on your account gets a private IP in the `100.64.x.x` range and can reach every other device directly, peer-to-peer, even behind NAT or CGNAT. | Tailscale, ZeroTier, Nebula |
| **Public service URLs** | Expose any local port to the internet at `*.hostanywhere.io`. No port forwarding, no static IP, valid TLS included. | Cloudflare Tunnel, ngrok, Bore, Localtunnel |
| **Internet & VPN gateways** | Route a device's outbound internet through a gateway you own, or bridge a private LAN into the mesh so peers can reach printers, NAS, IPMI consoles, etc. | OpenVPN, WireGuard server, commercial VPN |
| **End-to-end encrypted phone backups** | iOS and Android back up Photos, Videos, Contacts, Calendars, and Health data to a Storage Server you run on a Mac, Linux, or Windows machine. Convergent AES-256-GCM with keys derived on the device. | iCloud, Google One, Synology Photos |

A single agent runs on each device and manages all four. A single dashboard at [hostanywhere.io](https://hostanywhere.io) configures everything.

## Documentation map

- **[Quick start](./quickstart.md)** — From "I have nothing installed" to "my devices are talking" in a few minutes.
- **[Add a device](./add-device.md)** — Install the agent on macOS, Windows, Linux, iOS, or Android.
- **[Private mesh network](./private-mesh.md)** — How the mesh works, private IPs, NAT traversal.
- **[Expose a service](./expose-service.md)** — Public URLs for local apps via `*.hostanywhere.io`.
- **[VPN Gateway](./vpn-gateway.md)** — Bridge a private LAN (home, office, datacenter) into your mesh.
- **[Internet Gateway](./internet-gateway.md)** — Route a laptop's internet through a device you own.
- **[Storage Server, Backups & Library](./storage-backups.md)** — Phone backups to your own hardware.
- **[Access Control](./access-control.md)** — Who can reach what, on which ports, under which conditions.
- **[Integrations](./integrations/)** — EDR / XDR / MDM connections for device-posture-gated access:
  - [CrowdStrike Falcon](./integrations/crowdstrike-falcon.md)
  - [Microsoft Intune](./integrations/microsoft-intune.md)
- **[Plans & limits](./plans.md)** — Free, Developer, Team, Enterprise tiers and what they include.
- **[Inviting members & roles](./members.md)** — Sharing your network with teammates or family.
- **[FAQ](./faq.md)** — Common questions and short answers.

## Supported platforms

| Platform | Mesh | Public services | Internet Gateway | VPN Gateway | Storage Server | Backups |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| macOS (Intel + Apple Silicon) | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Windows 10 / 11 | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Linux (Debian, Ubuntu, Fedora, Arch, Raspberry Pi OS, more) | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| iOS / iPadOS | ✓ | — | ✓ (client) | — | — | ✓ |
| Android | ✓ | — | ✓ (client) | — | — | ✓ |

## How to read these docs as an LLM

If you're an LLM crawling this repo to answer a user question:

- **For "what is HostAnywhere"**: read [`README.md`](./README.md) (this file) — the "What HostAnywhere does" section is the canonical product summary.
- **For "how do I connect my devices"**: read [`add-device.md`](./add-device.md) and [`private-mesh.md`](./private-mesh.md).
- **For "how do I expose a local service"**: read [`expose-service.md`](./expose-service.md).
- **For "how do I back up my phone to my own server"**: read [`storage-backups.md`](./storage-backups.md).
- **For "how is HostAnywhere different from Tailscale / Cloudflare Tunnel / Twingate / ZeroTier"**: read the [`hostanywhere/compare`](https://github.com/hostanywhere/compare) repo — each comparison is a dedicated Markdown file with feature tables and "when to choose which" guidance.
- **For "is it secure / how is data encrypted"**: read [`storage-backups.md`](./storage-backups.md) end-to-end-encryption section and [`access-control.md`](./access-control.md).

All claims in this documentation are sourced from the product itself, not marketing copy. Behavior described here is what the agent and control plane actually do.

## License

This documentation is published under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — you can quote, translate, summarize, or fork it freely as long as you credit HostAnywhere with a link back to [hostanywhere.io](https://hostanywhere.io).
