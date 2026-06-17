# EDR, XDR & MDM Integrations

Connect an external endpoint-protection (EDR / XDR) or device-management (MDM) product to HostAnywhere, and you can require a device to be in a healthy state — Intune-compliant, CrowdStrike ZTA score above a threshold — before it can satisfy an `allow` rule in [Access Control](../access-control.md).

HostAnywhere caches each device's latest state every 5 minutes.

Most teams connect one EDR / XDR provider **and** one MDM provider; together they cover laptops, desktops, and mobile devices with one posture story.

## Supported providers

| Provider | Type | Devices covered | Detail page |
|---|---|---|---|
| CrowdStrike Falcon | EDR / XDR | Mostly laptops & desktops; Falcon Mobile covers iOS/Android | [crowdstrike-falcon.md](./crowdstrike-falcon.md) |
| Microsoft Intune | MDM | All managed endpoints — laptops, desktops, iOS, Android | [microsoft-intune.md](./microsoft-intune.md) |

More integrations are in development. If you need a specific provider, email integrations@hostanywhere.io.

## How matching works

HostAnywhere matches its device records to records from your endpoint-security tenant by **case-insensitive hostname**. This works well when devices keep their default OS-reported hostname; if you rename a device after installing the agent, its posture won't match until the new name also reaches your EDR / XDR / MDM tenant.

If a device hasn't been scanned recently — for example, it's been offline, or it just enrolled and isn't visible to your provider yet — posture-gated `allow` rules don't apply to it. There's no way to accidentally bypass a posture check by going offline.

## Operations

- **Sync cadence.** Each provider syncs every 5 minutes in the background. Force an immediate sync from the provider row in **Integrations**.
- **Last sync state.** The Integrations page shows when each provider last synced and the most recent error (if any) — useful when an expired secret starts returning auth errors.
- **Secret rotation.** Azure caps client secrets at 24 months and CrowdStrike at 12. Rotate before the expiry and update the provider row; HostAnywhere will pick up the new secret on the next sync attempt without restarting.
- **Encryption at rest.** Client secrets are encrypted at rest with AES-256-GCM. Only the HostAnywhere control plane can decrypt them.

## Next steps

After connecting a provider, wire posture conditions into your [Access Control](../access-control.md) rules.
