# Microsoft Intune Integration

Connect Microsoft Intune to HostAnywhere to gate access on each device's Intune compliance status.

## What you get

Intune is a binary model — each device is either **compliant** or **non-compliant** against the compliance policies you've configured in Endpoint Manager. Devices in a remediation grace period are counted as compliant, the same way Microsoft's Conditional Access treats them.

Once connected:

- Each HostAnywhere device's Intune compliance state syncs every 5 minutes.
- [Access Control](../access-control.md) rules can require compliance (e.g. "allow access to the database only if device is Intune-compliant").
- The dashboard shows each device's current compliance status and the policy decision behind it.

## Setup in Microsoft Entra ID

1. Sign in to [entra.microsoft.com](https://entra.microsoft.com) as a Global Administrator.
2. Go to **Applications → App registrations → + New registration**. Name it `HostAnywhere posture sync`, choose **Single tenant**, leave the Redirect URI blank. Click **Register**.
3. On the Overview page, copy the **Application (client) ID** and the **Directory (tenant) ID**.
4. Go to **Certificates & secrets → + New client secret**. Set the longest expiry your policy allows (Microsoft caps at 24 months). After clicking **Add**, immediately copy the **Value** column — it's only shown once.
5. Go to **API permissions → + Add a permission → Microsoft Graph → Application permissions**. Add **DeviceManagementManagedDevices.Read.All**. Then click **✓ Grant admin consent for [your directory]**.

## Setup on the HostAnywhere side

1. In the dashboard, open **Integrations** and click **+ Add provider → Microsoft Intune**.
2. For **Base URL / Tenant**, paste the **Directory (tenant) ID**. For **Client ID** and **Client Secret**, paste the values from steps 3 and 4 above.
3. Hit **Test connection**, then **Sync now**.

## License requirements

Devices must be **enrolled in Intune** (not just Azure AD-joined) to appear in `/deviceManagement/managedDevices`. Verify the tenant has Intune-eligible licenses (e.g. Microsoft 365 Business Premium, EMS E3/E5, or Intune standalone) assigned to the users whose devices you want to gate.

## Using compliance in access rules

In the Access Control rule editor, check **Device posture** and pick:

- **Provider**: Microsoft Intune
- **Condition**: Must be compliant

A device that isn't compliant (or that Intune doesn't know about) won't satisfy the rule. Default-deny everything else.

Example: "Only Intune-compliant devices can reach the production database."

```
allow in
source:      All devices
destination: tag = production-db
ports:       5432
protocol:    TCP
posture:     Microsoft Intune — must be compliant
```

## Secret rotation

Microsoft caps client secrets at 24 months. Before expiry, generate a new secret in Entra ID, update the provider row in HostAnywhere's Integrations page, and HostAnywhere will use the new secret on the next sync.

## Troubleshooting

- **"Auth failed" on Test connection**: Verify the Client Secret value (not the secret ID) was pasted correctly. The value is only shown once at creation time.
- **No devices appearing after Sync now**: Confirm the app has `DeviceManagementManagedDevices.Read.All` granted with admin consent. Without admin consent, the permission shows as "not consented" in Entra ID and Graph API calls return 403.
- **Personal-account error AADSTS7000215**: This misleading error usually means the app registration is in a personal Microsoft account tenant rather than a work tenant. Intune requires a work/school tenant with Intune-eligible licenses.
- **Hostname mismatches**: HostAnywhere matches by case-insensitive hostname. If a device shows up in Intune but not in HostAnywhere posture matches, confirm the hostname matches what the OS is reporting.

## See also

- [CrowdStrike Falcon integration](./crowdstrike-falcon.md)
- [Access Control](../access-control.md)
- [Integrations overview](./README.md)
