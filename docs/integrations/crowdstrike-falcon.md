# CrowdStrike Falcon Integration

Connect CrowdStrike Falcon to HostAnywhere to gate access on each device's Falcon **Zero Trust Assessment** score.

## What you get

Falcon's **Zero Trust Assessment** score (ZTA) rates each device on a 0-100 scale based on sensor health, OS patch status, login behavior, and account hygiene. Higher is better.

Once connected:

- Each HostAnywhere device gets a current ZTA score that updates every 5 minutes.
- [Access Control](../access-control.md) rules can require a minimum ZTA score (e.g. "allow SSH to production only if ZTA ≥ 75").
- The dashboard shows each device's current score and posture state at a glance.

## Setup on the Falcon side

1. Sign in to the Falcon console as a Falcon Administrator and go to **Support & resources → API clients and keys → Create API client**.
2. Give the client a name (e.g. `HostAnywhere posture sync`) and grant scopes:
   - **Hosts** — Read
   - **Zero Trust Assessment** — Read *(optional but recommended)*
3. Save and copy the **Client ID**, **Secret**, and **Base URL** (e.g. `https://api.crowdstrike.com` or one of the regional variants).

> Some Falcon tenants don't surface the **Zero Trust Assessment** scope on the API client form even when ZTA is enabled in the console. HostAnywhere handles that gracefully — see [Score fallback](#score-fallback) below.

## Setup on the HostAnywhere side

1. In the dashboard, open **Integrations** and click **+ Add provider**.
2. Pick **CrowdStrike Falcon**, paste the **Base URL**, **Client ID**, and **Secret**, and hit **Test connection**. A green check means the credentials work and at least one host is visible.
3. Click **Sync now** to populate the score table immediately, or wait for the 5-minute background worker.

## Score fallback

If the API client doesn't have **Zero Trust Assessment Read** permission, HostAnywhere falls back to a 0-100 score derived from Falcon's other health signals — whether the agent is enrolled, how recently it checked in, and whether Falcon reports the sensor as normal.

Your rules don't need to know which path produced the score; they compare against a minimum score either way.

## Using ZTA in access rules

In the Access Control rule editor, check **Device posture** and pick:

- **Provider**: CrowdStrike Falcon
- **Condition**: Score ≥ N

A device that scores below the threshold (or that Falcon doesn't know about, or that hasn't been seen recently) won't satisfy the rule. Default-deny everything else.

Example: "Engineering can SSH to production servers, but only if Falcon ZTA ≥ 75."

```
allow out
source:      tag = engineering
destination: tag = production
ports:       22
protocol:    TCP
posture:     CrowdStrike Falcon — score ≥ 75
```

## Secret rotation

CrowdStrike caps API secrets at 12 months. Before expiry, generate a new secret in the Falcon console, update the provider row in HostAnywhere's Integrations page, and HostAnywhere will use the new secret on the next sync.

## Troubleshooting

- **"Auth failed" on Test connection**: Verify the Base URL matches your tenant's region. CrowdStrike has separate endpoints for US-1, US-2, EU-1, and US-Gov clouds.
- **No hosts appearing after Sync now**: Confirm the API client has the **Hosts: Read** scope. Without it, HostAnywhere can authenticate but can't enumerate devices.
- **Hostname mismatches**: HostAnywhere matches by case-insensitive hostname. If you renamed a HostAnywhere device, Falcon may still know it by the old name until Falcon rescans the device.

## See also

- [Microsoft Intune integration](./microsoft-intune.md)
- [Access Control](../access-control.md)
- [Integrations overview](./README.md)
