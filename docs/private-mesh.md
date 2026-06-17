# Private Mesh Network

Every device running the HostAnywhere agent is assigned a stable private IP address and can reach any other device on your network directly — no VPN setup, no configuration required.

## What you get

- Devices get a stable private IP in the **100.64.x.x** range (CGNAT space — never routed on the public internet).
- Any port on any device is accessible from other devices on the same network.
- Works even if devices are behind different routers, carriers, or on different continents.
- When you invite teammates, their devices automatically join the same private network.

## How it works

Each device opens an outbound connection to the HostAnywhere control plane to learn about its peers and establish encrypted WireGuard tunnels. Whenever possible, traffic flows directly peer-to-peer; when NAT traversal isn't possible (symmetric NATs, restrictive networks), traffic is relayed through HostAnywhere infrastructure — still end-to-end encrypted, just one extra hop.

The agent handles all of this automatically. You don't configure peers, exchange keys, or open ports.

## Finding your private IP

Your device's private IP appears in the dashboard under **Devices** — click a device card to see it. It's also shown in:

- The Windows system tray icon
- The macOS menu bar icon
- The mobile app's main screen
- The output of `hostanywhere status` on Linux

## What you can do over the mesh

| Use case | Example |
|---|---|
| SSH to a remote machine | `ssh user@100.64.1.5` |
| RDP into a Windows desktop | Connect to `100.64.1.7:3389` |
| Browse a self-hosted web app | `http://100.64.1.8:8080` |
| Access a NAS share | `smb://100.64.1.10/Documents` |
| Hit a database on a private server | `psql -h 100.64.1.12 -U postgres` |
| Run a remote backup | `rsync -avz files/ user@100.64.1.5:/backup/` |

Anything that works over TCP or UDP on a LAN works over the mesh.

## Security model

- All mesh traffic is encrypted with WireGuard using per-peer Curve25519 keys.
- Keys are rotated regularly without manual intervention.
- The relay (when used) never sees decrypted content — it forwards encrypted packets between peers that can't establish a direct tunnel.
- The HostAnywhere control plane sees metadata (which peers are online, peer counts) but never the contents of mesh traffic.

## Limits

Mesh devices count against your network's **Devices** limit (see [Plans & limits](./plans.md)). The mesh itself has no per-network device cap below the plan limit — meshes scale to hundreds of peers without performance degradation, because each device only maintains tunnels to peers it's actively communicating with.

## See also

- [Add a device](./add-device.md) — installing the agent
- [Access Control](./access-control.md) — restricting who can reach what on the mesh
- [VPN Gateway](./vpn-gateway.md) — bridging non-mesh networks into the mesh
- [FAQ](./faq.md)
