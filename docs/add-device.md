# Add a Device

Install the agent on any device to connect it to your private mesh network and (on desktop platforms) expose local services or host a Storage Server.

## macOS

1. **Install** — Download HostAnywhere for macOS from [hostanywhere.io/download](https://hostanywhere.io/download), double-click the installer, and follow the steps. If macOS blocks it, go to **System Settings → Privacy & Security** and click **Open Anyway**.
2. **Sign in** — Connect using QR code, token, or your account (Google, GitHub, Microsoft, Apple, email, or SSO).
3. **You're connected** — The HostAnywhere menu bar icon appears.

Universal binary supports both Intel and Apple Silicon Macs. The agent installs as a launchd service and starts automatically.

## Windows

1. **Install** — Download HostAnywhere for Windows from [hostanywhere.io/download](https://hostanywhere.io/download) and run the installer — click **Yes** on the UAC prompt.
2. **Sign in** — Connect using QR code, token, or your account (Google, GitHub, Microsoft, Apple, email, or SSO).
3. **You're connected** — The HostAnywhere tray icon appears.

The installer registers the agent as a Windows Service. Tested on Windows 10 and Windows 11.

## Linux

1. **Install** — Run the one-line installer:
   ```sh
   curl -fsSL https://hostanywhere.io/install.sh | sh
   ```
   This downloads the binary and installs a systemd service.
2. **Sign in** — Run `hostanywhere login` to open a browser automatically (or generate a unique URL to paste into a browser on another machine). Connect with QR code, token, or your account.
3. **You're connected** — Verify with `hostanywhere status`.

Supported on Debian, Ubuntu, Fedora, Arch, Raspberry Pi OS, and most other systemd-based Linux distributions. ARM and x86_64 builds are auto-selected by the installer.

## iOS

1. **Install** — Install HostAnywhere from the [App Store](https://apps.apple.com/) on your iPhone or iPad.
2. **Sign in** — Open HostAnywhere and sign in with Apple, Google, GitHub, Microsoft, or email.
3. **You're connected** — Your phone joins the mesh and can reach every other device on your account.

The iOS app uses the Network Extension framework with WireGuard. You'll see a one-time VPN configuration prompt the first time you connect.

## Android

1. **Install** — Install HostAnywhere from [Google Play](https://play.google.com/store).
2. **Sign in** — Open HostAnywhere and sign in with Google, GitHub, Microsoft, Apple, or email.
3. **You're connected** — Your phone joins the mesh and can reach every other device on your account.

The Android app uses the system VPN service. You'll see a one-time VPN configuration prompt the first time you connect.

## After connecting

Once connected, your device appears in the **Devices** section of your dashboard with its private mesh IP (in the `100.64.x.x` range). From there you can:

- Reach any other device on your account directly by mesh IP.
- Enable [Storage Server](./storage-backups.md), [VPN Gateway](./vpn-gateway.md), or [Internet Gateway](./internet-gateway.md) on desktop devices.
- Apply [Access Control](./access-control.md) tags.
- Expose [public services](./expose-service.md) from any port the device is listening on.

## Common issues

- **macOS Gatekeeper blocks the installer**: Go to **System Settings → Privacy & Security** and click **Open Anyway** for the HostAnywhere installer.
- **Windows shows "Windows protected your PC"**: The installer is EV-code-signed and SmartScreen warnings should not appear on current Windows versions. If you see one, click **More info → Run anyway** — the binary is genuine.
- **Linux install fails with permission errors**: The installer needs `sudo` to create the network interface and install the systemd service. Re-run with `sudo`.
- **Mobile app says "VPN already in use"**: Disconnect any other VPN apps before connecting HostAnywhere — iOS and Android only allow one VPN profile to be active at a time.
