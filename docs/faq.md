# FAQ

## Do I need to open any ports on my router?

No. The agent connects outbound — no router configuration, port forwarding, or firewall changes needed. Works behind NAT and CGNAT.

## Can I expose multiple services from one machine?

Yes. A single agent handles every service you add to that device — you don't need to run multiple agents. Create each service in the dashboard (one subdomain → one local port); the agent picks them up on its next config poll (within ~10 seconds) and runs all of them concurrently.

## Can I be in multiple networks?

Not currently. Each account belongs to one network at a time.

## My invited user joined but I don't see their devices.

They need to install the agent and connect at least one device. Once they do, it appears in all team members' dashboards automatically.

## Does the agent need admin / root access?

Admin access is needed when first setting up the private network interface:
- **Windows installer** handles this automatically.
- **macOS .pkg installer** handles this automatically.
- **Linux** — the agent needs `sudo` or `CAP_NET_ADMIN` to create the network interface.

Regular tunnel traffic does not need elevated privileges.

## Is traffic secure?

Yes. Public services use HTTPS end-to-end. Private mesh traffic is encrypted between devices with WireGuard — the relay never sees the content.

## What happens when I hit a plan limit?

The action that would exceed the limit (adding a device, inviting a user, creating a public service) is blocked with a clear message. Nothing existing is removed. Upgrade your plan to continue.

## How is my backup encrypted?

Each file is encrypted on your phone **before** it's uploaded, using AES-256-GCM with convergent encryption. The data-encryption key is derived from the file's own SHA-256 hash, then wrapped with a master key that lives only on your phones (escrowed via iCloud Keychain on iOS, Android Keystore on Android). HostAnywhere has no copy of the master key and can't decrypt anything.

See [Storage Server & Backups](./storage-backups.md) for full details.

## Can I run multiple storage servers?

Yes. Run the storage server on as many devices as you like — phones automatically pick the one with the lowest latency on each sync. The storage servers also replicate to each other automatically in the background: a blob uploaded to one server propagates to the others within a few minutes via the mesh.

You get geographic + drive redundancy for free — a primary at home, a second copy on a NAS, a third on a friend's machine — all stay in sync without manual intervention.

## Can HostAnywhere see my internet traffic when I use Internet Gateway?

No. All traffic flows agent-to-agent over WireGuard, encrypted end-to-end. HostAnywhere's servers see only mesh metadata — which peers are online, peer counts, and so on — never the contents of the tunnels themselves.

## What if my storage server is offline when a phone tries to back up?

The phone retries periodically. Nothing is ever uploaded to HostAnywhere as a fallback — your data only goes to storage servers you own. When the server comes back online, the queued items sync automatically. If you have multiple storage servers, the phone tries the others first before queuing.

## Is my data sent to HostAnywhere's servers?

No. The architecture is intentional:

- **Mesh traffic** flows peer-to-peer over WireGuard. The relay forwards encrypted packets when direct connection isn't possible, but never decrypts them.
- **Public service traffic** terminates TLS at the relay and forwards over an encrypted tunnel to your agent. HostAnywhere can see the destination subdomain (for routing) but the request body is forwarded as-is.
- **Phone backup data** is encrypted on the phone and uploaded directly to your storage server over the mesh. HostAnywhere sees per-blob hashes for dedup checks, never contents.

The control plane sees metadata (peer presence, public service definitions, device names, posture scores) — never the contents of your traffic or backups.

## Can I self-host the control plane?

Not at this time. The control plane (dashboard, device registry, relay) is operated by HostAnywhere as a managed service. The agent, storage server, and gateway features all run on hardware you own.

Enterprise customers who need on-prem deployment options can contact sales@hostanywhere.io.

## What platforms are supported?

| Platform | Mesh | Public services | Internet Gateway | VPN Gateway | Storage Server | Backups |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| macOS (Intel + Apple Silicon) | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Windows 10 / 11 | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| Linux (Debian, Ubuntu, Fedora, Arch, Raspberry Pi OS) | ✓ | ✓ | ✓ | ✓ | ✓ | — |
| iOS / iPadOS | ✓ | — | ✓ (client) | — | — | ✓ |
| Android | ✓ | — | ✓ (client) | — | — | ✓ |

## How is HostAnywhere different from Tailscale / Cloudflare Tunnel / Twingate / ZeroTier?

Detailed comparisons live in our [compare repo](https://github.com/hostanywhere/compare). Short version:

- **vs Tailscale**: HostAnywhere adds public service URLs, phone backups, and built-in EDR/MDM posture in the same product.
- **vs Cloudflare Tunnel**: HostAnywhere adds a private mesh between your devices, access control between peers, and phone backups.
- **vs Twingate**: HostAnywhere is self-serve priced, includes public service URLs and phone backups, and works for individuals/families/homelabs in addition to enterprise.
- **vs ZeroTier**: HostAnywhere uses WireGuard (rather than a custom protocol), includes public service URLs and phone backups, and ships built-in EDR/MDM integration rather than scripting it.

## Can I use HostAnywhere offline / air-gapped?

Once connected, mesh traffic flows peer-to-peer without contacting HostAnywhere's servers. New devices need internet access to join the mesh. The agent caches its last-known peer and rule set, so existing devices keep working through brief control-plane outages.

## I have a security or privacy question not answered here.

Email **security@hostanywhere.io**.
