# VPN Gateway

A **VPN Gateway** lets one device act as a bridge between your HostAnywhere mesh and a private network the mesh can't reach directly — your home LAN, an office network, or a datacenter subnet.

Any mesh peer can then reach resources on those networks (printers, NAS, servers, cameras, management interfaces) without running the HostAnywhere agent on each one.

## When you'd use this

- You have legacy devices (printers, IoT, IPMI/iDRAC consoles) that can't run an agent.
- You want one device to tunnel your mesh into a corporate LAN.
- You're bridging a datacenter subnet or VPC range into the mesh.

## How it works

Designate any device as a VPN Gateway and tell it which private CIDRs it can route to (e.g. `192.168.1.0/24`, up to 10 subnets per gateway).

Other mesh peers learn those routes automatically and forward traffic for those ranges through the gateway. The gateway forwards the traffic onto its LAN interface and back. Peer-to-peer traffic between mesh devices continues to flow directly as before — only traffic destined for the advertised subnets is routed through the gateway.

## Enable a VPN Gateway

1. **Open the device detail page** — Sign in to the dashboard, go to **Devices**, and click the device you want to act as a gateway.
2. **Toggle VPN Gateway on** — Scroll to the **VPN Gateway** section and flip the toggle. You must be the network owner — members can see the section but can't configure it.
3. **Add advertised subnets** — Enter each private CIDR the gateway can reach. For a typical home LAN: `192.168.1.0/24`. For a corporate network: `10.0.0.0/16`. Click **Add subnet**.

You can add up to 10 subnets per gateway. Other mesh peers pick up the new routes within seconds and can start reaching addresses in those ranges.

## Rules and validation

- CIDRs overlapping the mesh range `100.64.0.0/10` are rejected.
- `0.0.0.0/0` is reserved for [Internet Gateway](./internet-gateway.md) — use that feature instead if you want to route all traffic through a device.

## Multiple gateways

You can designate more than one device as a VPN Gateway — useful for bridging your mesh into multiple private networks (home, office, datacenter) at once. Each gateway advertises its own subnet list independently.

If two gateways advertise overlapping subnets, the closer peer (lower latency) is preferred by clients.

## Plan requirements

VPN Gateway requires the **Developer plan or higher**. Only the network owner can enable it or edit advertised subnets. See [Plans & limits](./plans.md).

## Operational notes

The gateway device must have network connectivity to the advertised subnets. For home LANs, install the agent on a device that's always on (NAS, Raspberry Pi, always-on server) rather than a laptop that moves in and out of the network.

If the gateway goes offline, mesh peers lose access to those subnets until it's back. Running two gateways for the same subnet gives you redundancy.

## See also

- [Private mesh network](./private-mesh.md) — for direct device-to-device access
- [Internet Gateway](./internet-gateway.md) — for routing all outbound traffic through a device
- [Access Control](./access-control.md) — for restricting who can use a gateway
