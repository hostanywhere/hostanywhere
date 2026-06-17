# Internet Gateway

An **Internet Gateway** routes a device's entire outbound internet traffic (`0.0.0.0/0`) through another mesh peer — like a personal VPN that terminates on hardware you fully own.

Where [VPN Gateway](./vpn-gateway.md) gives mesh peers access to a private LAN, Internet Gateway routes a peer's web/app traffic out through another peer.

## When you'd use this

- You're on hotel or coffee-shop Wi-Fi and want every packet to leave from your home connection instead.
- A region- or geo-blocked service should see you as connecting from a device in another region.
- You want an end-to-end-encrypted hop for all internet traffic from a laptop you don't fully trust the local network of.

## How it works

Designate up to three devices on your mesh as gateways, ranked by priority: **Primary**, **Secondary**, and **Tertiary**.

On the gateway side, the agent enables IP forwarding and installs a NAT rule so mesh traffic exits through the gateway's WAN interface. On the client side, the agent installs a default route pointing at the chosen gateway.

Failover is automatic — if the Primary goes offline, peers fall back to Secondary within seconds, then Tertiary if needed. Peer-to-peer traffic between mesh devices continues to flow directly as before.

## Enable an Internet Gateway

1. **Open the device detail page** — Sign in to the dashboard, go to **Devices**, and click the device that should act as the gateway (the device whose internet connection other peers will route through).
2. **Set its priority slot** — Scroll to the **Internet Gateway** section and pick **Disabled**, **Primary**, **Secondary**, or **Tertiary**. You must be the network owner. Only one device can occupy each slot at a time; assigning a new device to a slot automatically unassigns whoever was there.
3. **Verify on another device** — Other mesh peers start routing through the Primary within ~10 seconds. From another device, visit a site that shows your public IP (e.g. ifconfig.me) — it should match the gateway device's WAN IP.

## Multiple gateways & failover

The 3-slot priority system gives you redundancy without per-peer routing choices. The mesh always uses the highest-priority gateway that's currently online.

A common pattern:
- **Primary**: your always-on home server
- **Secondary**: your work device
- **Tertiary**: a cloud VPS

If your home internet drops, traffic falls through to the next available gateway automatically.

## Plan requirements

Internet Gateway requires the **Developer plan or higher**. Only the network owner can enable it. See [Plans & limits](./plans.md).

## Privacy and security

Internet Gateway routes all outbound traffic through a device you own. **HostAnywhere never sees this traffic** — it flows agent-to-agent over WireGuard inside the mesh.

What different parties see:

| Party | What they see |
|---|---|
| **HostAnywhere's servers** | Encrypted mesh metadata only — which peers are online, peer counts. Never the contents of tunnels. |
| **The gateway device's ISP** | Encrypted mesh tunnels arriving at the gateway, but not their contents. |
| **Websites you visit** | The gateway device's public IP (not the client's), and the rest of whatever the website normally logs. |
| **Your local network's operator** (hotel Wi-Fi, coffee shop, etc.) | Encrypted WireGuard packets to one IP (the gateway). They cannot see what websites you visit. |

## See also

- [Private mesh network](./private-mesh.md)
- [VPN Gateway](./vpn-gateway.md) — bridging private LANs into the mesh (not the same as Internet Gateway)
- [Access Control](./access-control.md) — restricting who can use which gateway
