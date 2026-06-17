# Access Control

**Access Control** lets you decide who on your mesh can reach what. Every connection between two devices is checked against an ordered list of rules; the first rule that matches the connection decides whether to allow or deny it.

By default a new network is fully open — every device can reach every other device on every port. Add rules to lock things down when you need finer control.

## Typical use cases

- Lock a home NAS so only your laptop and phone can reach it, even though other family members are on the same mesh.
- Allow contractors to reach a single dev server on port 22 and nothing else.
- Block a guest device from reaching anything except the internet gateway.
- Require devices to be currently **compliant in Intune** or have a **CrowdStrike ZTA score above 75** before they can reach production systems.

## How rules are evaluated

Rules have a **priority** (lower number = higher priority). For every connection, HostAnywhere walks the list in priority order and stops at the first rule whose source, destination, port, and protocol all match — that rule's action (**allow** / **deny**) decides the outcome. If no rule matches, the network's **default policy** applies.

- **Default policy: allow** (the default for new networks). The mesh is open; rules carve out specific denies or posture-gated allows.
- **Default policy: deny**. The mesh is locked down; everything you want to permit needs an explicit allow rule. This is the right stance for production / multi-user networks.

Rules have a **direction**:
- **in** — someone is dialing this device.
- **out** — this device is dialing someone.

A symmetric "X can talk to Y" usually needs both directions, or you can write one direction and let the default policy cover the return path.

## Where it's enforced

Rules are resolved server-side and pushed to every device. Each platform enforces them with its native firewall, so there's no proxy / sidecar to install:

| Platform | Enforcer |
|---|---|
| Linux | `iptables` in the `FORWARD` chain, managed by the agent daemon |
| macOS | `pf` via a HostAnywhere-owned anchor under `com.apple/*` |
| Windows | `NetFirewall` rules in the HostAnywhere profile |
| iOS & Android | The agent filters out denied peers before they reach the mesh engine |

Enforcement is local to the device, so even if the control plane is unreachable, the last-known rule set keeps applying.

## Writing a rule

Open the dashboard, click **Access Control** (under **Users**), then **+ New rule**. Fill in:

- **Action** — `allow` or `deny`.
- **Direction** — `in` (others dialing this device) or `out` (this device dialing out).
- **Source** — a single device, a tag, the device's owner (any device they own), an OS family (Windows, macOS, Linux, iOS, Android), an address group, a CIDR, or "All devices".
- **Destination** — same options as Source.
- **Ports** — a single port (`22`), a list (`80,443`), a range (`5000-5010`), a port group (`HTTPS`, `SSH`, …), or "All ports".
- **Protocol** — TCP, UDP, both, or Any (Any covers ICMP — useful for a "no ping" rule).
- **Priority** — defaults to the next available number; drag rows in the list to reorder.
- **Posture conditions** (optional) — gate the rule on the connecting device's EDR / XDR / MDM state. See [Integrations](./integrations/).

## Tags

**Tags** are labels you attach to devices to group them for access-control rules. They're the most common way to write rules: instead of naming individual devices, you tag the devices that share a purpose and then write rules against the tag. A device can wear any number of tags — your work laptop might be tagged `engineering`, `production-access`, and `office` at once.

Typical tags:
- `home`, `office` — physical location
- `engineering`, `finance`, `support` — team or department
- `production`, `staging` — environment
- `byod`, `guest`, `contractor` — device class or access level

Create and manage tags in the dashboard under **Access Control → Tags**. Each tag can have an optional description (so future-you remembers what it's for) and a color (so it's easy to spot in the rules list). Adding or removing a device from a tag instantly updates every rule that references the tag — there's no need to edit each rule when the membership changes.

> Reach for tags whenever the same group of devices shows up in more than one rule. They make rule logic explicit ("engineering can SSH to production") instead of relying on lists of device names that drift as people come and go.

## Address groups and port groups

**Address groups** let you reuse a set of devices or CIDRs across many rules — "Engineering laptops", "Office subnet", "Production servers". Use them when you need to mix in raw IP ranges that aren't HostAnywhere devices (e.g. a corporate subnet or a cloud VPC). For pure device groupings, tags are simpler.

Built-in groups always exist:
- **All devices** — every device on the network.
- **All users** — every device owned by any human member (excludes service / unattended hosts).
- **My devices** — the requesting device's own owner's devices. Useful for "let me reach my own machines from anywhere" rules.

**Port groups** are reusable port lists with a protocol — e.g. `HTTPS` = TCP 80,443 or `SQL` = TCP 1433,3306,5432.

Both groups are managed under **Access Control → Groups** in the dashboard and selected by name in the rule editor.

## Rule examples

### "Production servers can be reached by engineering only."

Default policy `deny`. Add `allow in`, source = tag `engineering`, destination = tag `production`, all ports. Adding or removing engineers from the team is just a matter of toggling the tag on their device — no rule edits.

### "Only Intune-compliant devices can reach the database."

Default policy `deny`. Add `allow in` on devices tagged `database`: source = `All devices`, ports = your database port (e.g. `5432` for PostgreSQL), protocol = TCP, posture condition = "Intune: must be compliant".

### "Engineering needs SSH everywhere, but only if Falcon ZTA ≥ 75."

`allow out`, source = tag `engineering`, destination = `All devices`, ports = `22`, protocol = TCP, posture condition = "Falcon: score ≥ 75".

### "Block guest phones from anything except the internet gateway."

Default policy `deny`. One `allow out` rule with source = tag `guest`, destination = `Internet gateway` (built-in role group), all ports.

> To "deny non-compliant" devices, you don't write a deny rule with a negated condition. Instead, write the `allow` with the condition you want and let default-deny catch everything else. Negative conditions tend to interact badly with rule ordering.

## Posture conditions

A rule can optionally require the connecting device to be in a healthy state, as judged by an EDR, XDR, or MDM product you've connected to HostAnywhere. Set this up under [Integrations](./integrations/), then check the **Device posture** box in the rule editor and pick a provider with a minimum score (CrowdStrike) or required compliance (Intune).

## See also

- [Integrations: CrowdStrike Falcon](./integrations/crowdstrike-falcon.md)
- [Integrations: Microsoft Intune](./integrations/microsoft-intune.md)
- [Plans & limits](./plans.md)
