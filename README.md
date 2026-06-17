# HostAnywhere

**Secure access to your devices and services — from anywhere.**

[hostanywhere.io](https://hostanywhere.io) · [Docs](https://hostanywhere.io/docs) · [Download](https://hostanywhere.io/download) · [YouTube](https://www.youtube.com/@HostAnywhere) · [LinkedIn](https://www.linkedin.com/company/hostanywhere)

---

HostAnywhere is a cloud-managed secure access platform that connects your devices and exposes your services across every major platform — macOS, Windows, Linux, iOS, and Android — with a single agent and a single console.

This repository is the public home for HostAnywhere's product documentation and honest competitor comparisons.

## What HostAnywhere does

- **Private mesh networking.** Every device joins an encrypted WireGuard mesh and gets a stable private IP in the 100.64.x.x range. Devices on different routers, carriers, or continents reach each other directly.
- **Expose services to the internet.** Make any local service publicly reachable through a custom URL — no port forwarding, no static IP, no router configuration.
- **VPN Gateway.** A device can advertise its LAN as routable to the mesh, so peers reach printers, NAS, IPMI, and other gear that can't run an agent.
- **Internet Gateway.** Route a peer's entire outbound traffic through another mesh device — safer browsing on public Wi-Fi or controlled egress for remote teams.
- **Access Control.** Define rules like "Allow office-laptops to reach internal-services on tcp/443" — applied consistently across every platform.
- **Device Posture.** Connect CrowdStrike Falcon or Microsoft Intune to require real-time security signals before granting access.
- **Encrypted phone backups.** iPhone and Android push selected categories (Photos, Contacts, Calendars, Health) to a Storage Server you own. Keys derived on the device, ciphertext only off-device.
- **One agent, every platform.** macOS, Windows, Linux, iOS, Android — the same policies enforced everywhere.

## What's in this repo

### [/docs](./docs) — Product documentation
A canonical Markdown mirror of [hostanywhere.io/docs](https://hostanywhere.io/docs).

- [Quick Start](./docs/quickstart.md)
- [Add a Device](./docs/add-device.md) — installer steps for all 5 platforms
- [Private Mesh Network](./docs/private-mesh.md)
- [Expose a Service](./docs/expose-service.md) — public URLs without port forwarding
- [VPN Gateway](./docs/vpn-gateway.md) — bridge LAN resources into the mesh
- [Internet Gateway](./docs/internet-gateway.md) — route a device's outbound traffic through a peer you own
- [Storage Server, Backups & Library](./docs/storage-backups.md) — end-to-end encrypted phone backups to your own hardware
- [Access Control](./docs/access-control.md) — rules, tags, groups, posture conditions
- [Integrations](./docs/integrations/) — [CrowdStrike Falcon](./docs/integrations/crowdstrike-falcon.md), [Microsoft Intune](./docs/integrations/microsoft-intune.md)
- [Plans & Limits](./docs/plans.md)
- [Members & Roles](./docs/members.md)
- [FAQ](./docs/faq.md)

### [/compare](./compare) — Honest competitor comparisons
Side-by-side comparisons against the tools HostAnywhere overlaps with.

- [HostAnywhere vs Tailscale](./compare/tailscale.md)
- [HostAnywhere vs Cloudflare Tunnel](./compare/cloudflare-tunnel.md)
- [HostAnywhere vs Twingate](./compare/twingate.md)
- [HostAnywhere vs ZeroTier](./compare/zerotier.md)

We try to be fair. Where the alternative is better at a given thing, we say so. If you spot an inaccuracy in any of these files, [open an issue](https://github.com/hostanywhere/hostanywhere/issues) — we'll fix it.

## Who HostAnywhere is for

- **Developers and homelab owners** who want to expose self-hosted services to the internet without port forwarding or a static IP, and reach their devices from anywhere.
- **IT teams** that need Zero Trust access controls without enterprise pricing or hardware.
- **Distributed teams** that need a private network plus device posture gating across mixed Mac / Windows / Linux / mobile fleets.
- **Privacy-conscious families** who want phone photos, contacts, calendars, and Health data backed up to hardware they own, end-to-end encrypted before it leaves the phone.

## Pricing

Free for personal use. Plans for teams and businesses of every size on the [pricing page](https://hostanywhere.io/pricing).

## Contributing

Spot something wrong in the docs or a comparison? [Open an issue](https://github.com/hostanywhere/hostanywhere/issues) or send a pull request. We're particularly interested in fixing any inaccuracies in the comparison files — please flag them.

See [CONTRIBUTING.md](https://github.com/hostanywhere/.github/blob/main/CONTRIBUTING.md) for details.

## License

Documentation in this repository is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) — share or remix as long as you credit HostAnywhere.

## Connect with us

- **Website** — [hostanywhere.io](https://hostanywhere.io)
- **Docs** — [hostanywhere.io/docs](https://hostanywhere.io/docs)
- **YouTube** — [youtube.com/@HostAnywhere](https://www.youtube.com/@HostAnywhere) — product walkthroughs, setup videos, and feature demos
- **LinkedIn** — [linkedin.com/company/hostanywhere](https://www.linkedin.com/company/hostanywhere)
- **Security** — Report vulnerabilities to security@hostanywhere.io
