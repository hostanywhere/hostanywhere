# Quick Start

Get a device on your HostAnywhere network in a few minutes. The universal first step is the same for every use case; the rest depends on what you want to do.

## 1. Add your first device

### Sign in
Sign in at [hostanywhere.io](https://hostanywhere.io) with Google, GitHub, Microsoft, Apple, email, or SSO.

### Install the agent
Download for Mac, Linux, or Windows from the [Download page](https://hostanywhere.io/download), or get the mobile app from the [App Store](https://apps.apple.com/) or [Google Play](https://play.google.com/store).

Your device joins your private mesh automatically.

→ See [Add a device](./add-device.md) for per-platform install steps.

## 2. Pick what you want to do

### Connect your devices privately

1. Install the agent on each device.
2. SSH, RDP, or browse to any other device — they all share a private mesh IP in the `100.64.x.x` range.

Works behind NAT, CGNAT, and corporate Wi-Fi without port forwarding.

→ See [Private mesh network](./private-mesh.md).

### Expose a local service to the internet

1. Click **+ Add Service** in the dashboard.
2. Pick a subdomain (e.g. `myapp`) and your local address (e.g. `localhost:3000`).
3. Your service is live at `https://myapp.hostanywhere.io`. No router config, no static IP.

→ See [Expose a service](./expose-service.md).

### Bridge a private network into the mesh

1. Install the agent on a device that's already on that private network — a Raspberry Pi on your home LAN, a server in your office network, or a small VM in a datacenter VPC.
2. Open that device's detail page in the dashboard, toggle **VPN Gateway** on, and add the CIDR(s) it can route to (e.g. `192.168.1.0/24`).

Other mesh peers can now reach printers, NAS, internal apps — anything on those subnets — without running the HostAnywhere agent on each one.

→ See [VPN Gateway](./vpn-gateway.md).

### Route internet traffic through another device

1. Pick an always-on device (home server, NAS, cloud VPS) and set its **Internet Gateway** priority to **Primary**.
2. Your laptop now exits the internet via that device — like a personal VPN you fully own.

Add **Secondary** and **Tertiary** gateways for automatic failover.

→ See [Internet Gateway](./internet-gateway.md).

### Back up your phone to your own storage

1. On a Mac, Linux, or Windows device, open the device detail page and toggle **Storage Server** on.
2. Install the HostAnywhere mobile app, sign in, and in the Backups tab toggle Photos, Contacts, Calendars, etc.

Every file is end-to-end encrypted on the phone before it leaves the device.

→ See [Storage Server, Backups & Library](./storage-backups.md).

## What's next

- Configure [Access Control](./access-control.md) — who can reach what on which ports.
- Connect [CrowdStrike Falcon](./integrations/crowdstrike-falcon.md) or [Microsoft Intune](./integrations/microsoft-intune.md) to gate access on device posture.
- Invite teammates (see [Members & roles](./members.md)).
