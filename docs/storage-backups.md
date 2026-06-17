# Storage Server, Backups & Library

HostAnywhere lets you back up your phones to your own hardware. Three pieces work together:

- **Storage Server** — a small encrypted-blob store the desktop agent runs on a Mac, Linux, or Windows device you own.
- **Backups** — the iOS and Android apps push selected categories (Photos, Contacts, Calendars, etc.) to your storage server.
- **Library** — a tab in the mobile app to browse and restore everything you've backed up, from any of your signed-in devices.

**Every file is encrypted on your phone before it leaves the device.** HostAnywhere's servers never hold your data.

## Storage Server

A storage server runs as a background process inside the desktop agent. It listens on TCP port `36129` inside your private mesh, serves a self-signed TLS certificate (its SHA-256 fingerprint is pinned by the phones), and accepts encrypted blob uploads from your phones.

### Enable a storage server

1. **Open the device detail page** — Sign in to the dashboard, go to **Devices**, and click a Mac, Linux, or Windows device you want to use for backups. Pick a device that's online often — a NAS, always-on server, or a desktop is ideal.
2. **Toggle Storage Server on** — Scroll to the **Storage Server** section and flip the toggle. Within a few seconds the badge changes from **Starting…** to **Running**, and the dashboard shows the mesh URL and (optionally) internet URL the phones will use.
3. **(Optional) Adjust advanced settings** — Open the **Advanced** disclosure to set a custom storage path, cap maximum disk usage (defaults to 50% of free space, capped at 500 GB), or change internet access mode (mesh-only, mesh + internet fallback, always internet).

### Where backups are stored on disk

| Platform | Default path |
|---|---|
| Linux (systemd, as root) | `/var/lib/hostanywhere/backups` |
| macOS | `~/HostAnywhere/Backups` (under the console user's home) |
| Windows | `%LOCALAPPDATA%\HostAnywhere\Backups` |

You can override the path under **Advanced**. Useful when you want backups on a specific external drive, a separate NAS volume, or a non-default partition.

### Multiple storage servers

You can run storage servers on more than one device. Phones automatically pick the one with the lowest latency on each sync (mesh first, internet fallback).

Storage servers replicate to each other automatically in the background: a blob uploaded to one server propagates to the others within a few minutes via the mesh. You get geographic + drive redundancy for free — a primary at home, a second copy on a NAS, a third on a friend's machine — all stay in sync without manual intervention.

## Backups (iOS & Android)

The mobile app's **Backups** tab is where each phone picks what to back up. Categories are off by default; you opt in per category, and each toggle prompts for the OS permission it needs the first time you enable it.

### What can be backed up

| Category | iOS | Android |
|---|:---:|:---:|
| Photos | ✓ | ✓ |
| Videos | ✓ | ✓ |
| Contacts | ✓ | ✓ |
| Calendars | ✓ | ✓ |
| Health | ✓ | — |

### Enable a backup category

1. **Open the app** — Open HostAnywhere on the phone, sign in, and go to the **Backups** tab.
2. **Toggle a category** — Flip the switch next to a category. The OS prompts for the matching permission (Photos, Contacts, etc.) — tap Allow. You can toggle as many categories as you like; each prompts independently.
3. **Tap Sync now** — The first sync starts immediately and runs in the background. After that, the app re-syncs in the background as you add new photos, contacts, etc. Watch the status card for progress and the per-category counters for what's been backed up.

### End-to-end encryption

Every file is encrypted on the phone before it's uploaded.

- **Algorithm**: AES-256-GCM with convergent encryption.
- **Key derivation**: The data-encryption key for a given file is derived from its own SHA-256 hash, then wrapped with a master key that exists only on your phones.
- **Master keys**: Generated locally on first sync — escrowed via iCloud Keychain on iOS, stored in Android Keystore-backed encrypted preferences on Android.
- **HostAnywhere has no copy of the master key and can't decrypt anything.**

On iOS, encryption is always on (no toggle). On Android, encryption defaults to on with a Settings toggle for parity with iOS conventions.

### Cellular vs Wi-Fi

By default, backups only run on Wi-Fi to avoid surprise data usage. A "Back up over cellular" toggle in Settings flips that.

## Library

The **Library** tab in the mobile app lists everything you've backed up, grouped by category, across all your storage servers. Each item shows its category, size, and the date it was backed up.

### Restore from another device

1. **Install HostAnywhere on the new device** — Get HostAnywhere from the [App Store](https://apps.apple.com/) or [Google Play](https://play.google.com/store) and sign in with the same account.
2. **Open the Library tab** — Everything you've backed up from your other phones shows up automatically — the app discovers your storage servers via the control plane and the master key is restored from iCloud Keychain (iOS) or your account (Android).
3. **Tap to restore** — Tap any item to download it. Photos and videos restore to the camera roll; contacts merge into the system address book; calendars merge into the system calendar; health records open in Apple Health (iOS).

## Availability and limits

Storage Server, Backups, and Library are available on **all plans, including Free**. You provide the storage; we don't charge for the bytes.

## Privacy details

Your data flows phone → mesh → your own storage server. HostAnywhere sees only the per-blob hash for deduplication checks; the encrypted contents go peer-to-peer over WireGuard.

If you want extra paranoia, set **Internet access mode** to **Mesh-only** in the Storage Server advanced settings — phones will then require a mesh connection (no internet fallback) before they can back up or restore.

## What happens when the storage server is offline

The phone retries periodically. Nothing is ever uploaded to HostAnywhere as a fallback — your data only goes to storage servers you own. When the server comes back online, the queued items sync automatically. If you have multiple storage servers, the phone tries the others first before queuing.

## See also

- [Add a device](./add-device.md) — to install the agent on the machine you'll use as a storage server
- [Private mesh network](./private-mesh.md) — phones reach storage servers over the mesh
- [Access Control](./access-control.md) — for restricting which devices can reach storage servers
- [FAQ](./faq.md)
