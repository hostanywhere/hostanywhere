# Expose a Service

**Expose a Service** creates a public URL for something running on your local machine — a dev server, a self-hosted app, a webhook receiver, a media server — without opening ports on your router or having a public IP.

The agent on your machine opens an outbound connection to the HostAnywhere relay; visitors hit the relay over HTTPS at your chosen subdomain (e.g. `https://myapp.hostanywhere.io`), and the relay forwards the request through that tunnel to your local port.

## Typical use cases

- Sharing a dev preview with a teammate or client without deploying anywhere.
- Receiving webhooks during development (Stripe, GitHub, Twilio, Slack).
- Sharing a self-hosted app with family or friends without setting up a VPN for them.
- Letting a phone reach a local app on a desktop when off the same Wi-Fi.
- Quick demos that need a real public URL.

## Why it works

- The agent only makes **outbound** connections, so there's nothing to configure on your router — works behind NAT and CGNAT.
- **HTTPS is built in**. Every subdomain lands on `*.hostanywhere.io` with a valid TLS certificate, no configuration needed.
- **Multiple services per machine** — one tunnel per service, all managed by a single agent.

> **Note:** Joining the [private mesh network](./private-mesh.md) is optional for this use case. Exposing a service works on its own — install the agent, point it at a local port, done. The private mesh is a separate feature; you only need it if you also want the devices on your account to reach each other directly.

## How to expose a service

From the dashboard, click **+ Expose a Service** and fill in:

- **Subdomain** — the public URL prefix, e.g. `myapp` → `https://myapp.hostanywhere.io`
- **Local address** — where the traffic should go on your machine, e.g. `localhost:3000`

You can expose multiple services from the same machine.

## Examples

| Local service | Subdomain | Resulting URL |
|---|---|---|
| Next.js dev server on port 3000 | `dev` | `https://dev.hostanywhere.io` |
| Jellyfin on port 8096 | `media` | `https://media.hostanywhere.io` |
| Webhook receiver on port 4000 | `hooks` | `https://hooks.hostanywhere.io` |
| Stable Diffusion WebUI on port 7860 | `sd` | `https://sd.hostanywhere.io` |
| Home Assistant on port 8123 | `home` | `https://home.hostanywhere.io` |

Subdomains are global to HostAnywhere — first-come, first-served per account. Pick descriptive prefixes early.

## What gets sent through the tunnel

- HTTPS requests from the public internet (terminated at the HostAnywhere relay).
- Forwarded as the original request over an outbound tunnel from your agent.
- Your local service sees the request as if it had arrived on `127.0.0.1` (with `X-Forwarded-For` and other proxy headers indicating the real client).

WebSockets work transparently. Long-running connections (server-sent events, streaming) work — the tunnel has no idle timeout.

## What does not go through the tunnel

- Anything not specifically routed to a configured service. The relay won't forward arbitrary subdomains to your machine.
- DNS lookups — `myapp.hostanywhere.io` resolves to HostAnywhere's relay IPs, not your home IP.

## Limits

The number of public services you can run depends on your plan — see [Plans & limits](./plans.md). Free includes 3, Developer includes 25, Team includes 100, Enterprise unlimited.

## Security

- All public traffic is HTTPS — TLS terminates at the relay with a valid cert for `*.hostanywhere.io`.
- The tunnel from relay to agent is TLS-encrypted.
- Anyone with the public URL can reach your service. If you want to restrict access:
  - Put auth in your local app (most common).
  - Or run the service inside the mesh and gate it with [Access Control](./access-control.md) instead of exposing it publicly.

## See also

- [Private mesh network](./private-mesh.md) — for device-to-device access without public exposure
- [Access Control](./access-control.md) — for restricting mesh-side access to services
- [FAQ](./faq.md)
