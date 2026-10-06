---

copyright:
  years: 2026

lastupdated: "2026-10-06"

keywords: smart card, PIN pad, PPD, crypto unit, smartcard setup, smartcard troubleshooting

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Working with smart cards
{: #smartcard}

Several `kp crypto-unit` commands support hardware-backed smart cards as an alternative to file-based credentials and master key shares. This topic covers the JSON configuration format, setting up the PIN Pad Daemon (PPD) for remote connectivity, and troubleshooting common issues.
{: shortdesc}

## What is a smart card?
{: #smartcard-overview}

A smart card is a hardware-backed security device that stores cryptographic key material in tamper-resistant hardware. In {{site.data.keyword.keymanagementserviceshort}} Dedicated, smart cards can store the following sensitive materials:

- **Administrator authentication keys** — the RSA or ECDSA signature keys used to authenticate administrative operations on crypto units
- **Master Key (MK) shares** — the key share parts used to reconstruct and load the Master Key into crypto units

Smart cards coexist with the existing file-based approach. You can use either method or migrate from files to smart cards at any time. No changes are required if you continue using file-based credentials.

### Hardware requirements
{: #smartcard-hardware}

Smart card support requires a compatible **CyberJack One PIN pad reader**. The PIN pad is used for both secure PIN entry and card communication.

- On [Linux]{: tag-linux} and [macOS]{: tag-macos}: the reader operates as a plug-and-connect device. No additional drivers are needed.
- On [Windows]{: tag-windows}: the CyberJack One driver must be installed separately before using the reader. See [Installing the CyberJack One driver on Windows](#smartcard-windows-driver).

Each smart card can store up to 16 Master Key share slots and one RSA and one ECDSA administrator key. For remote smart card access, the PIN Pad Daemon (PPD) must be running on the system where the reader is connected. See [Setting up PPD](#kp-ppd-setup).

### Installing the CyberJack One driver on Windows
{: #smartcard-windows-driver}

Verify whether the driver is present and install it if it is not already on your system.

1. Connect the USB plug of the PIN pad reader to the host computer.
2. Start the **Device Manager**.
3. Double-click the **Universal Serial Bus devices** (or **USBDevices**) entry in the displayed list.
   - If you see **cyberJack ONE** in the devices list, the driver is already installed.
   - If the driver is not installed:
     1. Double-click the **Other Devices** entry in the displayed list where **cyberJack ONE** is listed.
     2. Double-click **cyberJack ONE**. The **cyberJack ONE Properties** dialog box opens.
     3. Click **Update Driver**. The **Update Drivers - cyberJack ONE** dialog box opens.
     4. Click **Browse my computer for driver software**.
     5. Click **Browse** and select the PIN pad driver from the previously downloaded and extracted driver archive (for example, `\Downloads\cyberJack\Driver`).
     6. Click **Next**. If a **Windows Security Warning** dialog opens, click **Install**. The PIN pad driver is installed.
     7. When completed, the successful driver installation is confirmed in a separate dialog box. Click **Close** in the **cyberJack ONE Properties** dialog box.

The PIN pad is now displayed as **cyberJack ONE** in the Windows Device Manager as a subentry of the **Universal Serial Bus devices** entry.

## Smart card JSON file format
{: #kp-crypto-unit-smartcard-json}

Several `kp crypto-unit` commands accept a smart card reference in the format `@SC_JSON_FILE,CARD_ID`. The JSON file maps user-defined card IDs to smart card connection details.

**File format:**

```json
{
  "sc1": {
    "host": "host_ip",
    "port": 6072,
    "pin": "",
    "pwd": "pwd"
  },
  "sc2": {
    "host": "localhost",
    "port": 6070,
    "pin": "",
    "pwd": "pwd"
  }
}
```

**Fields:**

| Field | Description |
|-------|-------------|
| `CARD_ID` | User-defined identifier for the smart card entry. Must be unique and alphanumeric. Case-sensitive |
| `host` | IP address or hostname of the system where the smart card reader is connected. Leave empty or omit for a local card. If `localhost` is used, PPD is required |
| `port` | Port on which the PIN Pad Daemon (PPD) is running. Omit to use the default port (6070) |
| `pin` | Smart card PIN. Must be at least 6 numeric digits if provided. Leave empty to be prompted via the PIN pad (recommended) |
| `pwd` | PPD password configured in the password file. Required for remote PIN pad access |

The `@` prefix and the `.json` extension are required when referencing a file (`@file.json,CARD_ID`). Use `-` alone to reference a local smart card without a JSON file.

Providing the PIN in the JSON file is less secure than entering it on the PIN pad, as it might be exposed in command history or log files.
{: note}

## PIN Pad Daemon (PPD)
{: #kp-crypto-unit-ppd}

The PIN Pad Daemon (PPD) is required for remote smart card connectivity. It opens a port (default `6070`) on the system where the smart card reader is physically connected, allowing remote commands to reach the card.

IBM distributes the PPD bundle for customer use. The bundle includes the vendor-provided PPD executable together with configuration files used to run it in your environment.
{: note}

### Setting up PPD
{: #kp-ppd-setup}

PPD enables remote smart card access over the network. For remote connectivity to work, the following conditions must be met:

- Both the local system (where the smart card reader is connected) and the remote system must be on the same network or connected via VPN.
- SSH must be enabled on the local system.
- The firewall on the local system must allow inbound connections on the PPD port (default `6070`).

If the local system is behind NAT or a restrictive firewall without a shared network path to the remote system, remote access fails. In that case, a public IP, port forwarding, or SSH tunneling is required.
{: important}

Follow these steps to download, configure, and run PPD on the system where your smart card reader is connected.

1. Download the PPD bundle for your operating system from the {{site.data.keyword.keymanagementserviceshort}} UI:
   1. In the {{site.data.keyword.keymanagementserviceshort}} Dedicated dashboard, navigate to the **Getting started** page.
   2. Click the **Get started** link in the **Smart cards** section.
   3. Download the PPD archive for your operating system.

   For macOS, contact [IBM Support](/docs/key-protect?topic=key-protect-getting-help) to get the PPD archive.
   {: note}

2. Extract the bundle and locate the files for your platform.
3. Optional: Edit `ppd.cfg` to change the default port or log file location. If you skip this step, PPD runs with the default configuration.
4. Update the default password in `ppd.pwd`. The default password is `12345678`.
5. Run the PPD executable.

   For [Linux]{: tag-linux} and [macOS]{: tag-macos} — background mode:
   ```sh
   ./ppd -config=./ppd.cfg
   ```
   {: pre}

   For [Linux]{: tag-linux} and [macOS]{: tag-macos} — foreground mode:
   ```sh
   ./ppd -config=./ppd.cfg -foreground
   ```
   {: pre}

   For [Windows]{: tag-windows} — background mode:
   ```sh
   start ppd.exe -config=ppd.cfg
   ```
   {: pre}

   For [Windows]{: tag-windows} — foreground mode:
   ```sh
   ppd.exe -config=ppd.cfg -foreground
   ```
   {: pre}

The PPD bundle includes:
- PPD executable (per OS/architecture)
- `ppd.cfg` — configuration file (port, protocol, authentication settings)
- `ppd.pwd` — PPD password file
- `ppd.sig` — Signature files (Linux only)

PPD logging is enabled by default. Logs are written to the file specified by the `LogFile` setting in `ppd.cfg`. You can review these logs to diagnose connectivity or authentication issues.
{: tip}

### PPD password file behavior
{: #kp-ppd-password-file}

PPD requires a valid password file (`ppd.pwd`) to authorize device connections. By default, `ppd.cfg` references the password file using the path `<CONFIGPATH>/ppd.pwd`, where `<CONFIGPATH>` is resolved relative to how the daemon is launched. The maximum password length is 79 characters.

If `PassFile` points to a file that does not exist, PPD can still start and accept connections without prompting for a password. To avoid running the service without password protection, keep the shipped password filename unless you have a reason to change it, make sure the password file exists at the configured path, and verify the configuration after any change.
{: important}

Choose one of the following methods depending on how you want to organize your environment.

#### Keep the password file in the same folder (recommended)
{: #kp-ppd-password-same-folder}

If you keep `ppd.cfg` and `ppd.pwd` in the same directory, launch PPD with the `./` prefix on the config path. This tells PPD to resolve the password file path relative to the current directory.

For [Linux]{: tag-linux} and [macOS]{: tag-macos}:

```sh
./ppd -config=./ppd.cfg -foreground
```
{: pre}

You must include the `./` prefix before `ppd.cfg`. Without it, PPD resolves the password file path from the system root (`/ppd.pwd`), which causes a file access failure.
{: important}

#### Store the password file in a custom location
{: #kp-ppd-password-custom-location}

If you prefer to store the password file in a different directory, set the absolute path in `ppd.cfg`:

1. Open `ppd.cfg` in a text editor.
2. Locate the `Passfile` setting and replace the `<CONFIGPATH>` template with the absolute path to your password file.

   For [Linux]{: tag-linux} and [macOS]{: tag-macos}:
   ```text
   Passfile = /Users/yourusername/secure/ppd.pwd
   ```
   {: codeblock}

   For [Windows]{: tag-windows}:
   ```text
   Passfile = C:\ProgramData\ppd\ppd.pwd
   ```
   {: codeblock}

3. Save the file. Because the path is absolute, you can launch PPD from any directory without the `./` prefix:

   ```sh
   ./ppd -config=ppd.cfg -foreground
   ```
   {: pre}

### Supported platforms for smart card operations
{: #kp-ppd-supported-platforms}

| Platform | Notes |
|----------|-------|
| Windows AMD64 | Requires CyberJack One driver installation |
| Linux AMD64 | No additional drivers needed |
| Linux ARM64 | No additional drivers needed |
| macOS AMD64 | Supported up to macOS Tahoe; no additional drivers needed |
| macOS ARM64 | Supported up to macOS Tahoe; no additional drivers needed |

The smart card feature is supported on macOS up to macOS Tahoe. For any newer versions of macOS, [open a support ticket](/docs/key-protect?topic=key-protect-getting-help).
{: note}

### Providing the smart card PIN
{: #kp-ppd-providing-pin}

- **From the JSON file**: Add `pin` in the JSON file. This step skips the manual PIN entry through the PIN pad.
- **From the PIN pad (recommended)**: Leave the PIN field empty. The PIN pad prompts you to enter the PIN securely. After the command is issued, you have 60 seconds to enter the PIN on the reader before the session times out.

**PIN retry limit:** Entering the wrong PIN 5 consecutive times permanently blocks the smart card. There is no known method to unblock a card. If a wrong PIN is entered, the remaining retry count is shown on the PIN pad display.
{: important}

For help diagnosing and resolving smart card issues, see [Troubleshooting smart card operations](/docs/key-protect?topic=key-protect-kp-smartcard-troubleshooting).
