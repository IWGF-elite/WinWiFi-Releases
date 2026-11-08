# WinWiFi Security Auditor

**Current direct-download version: 3.0.14.** [Download the Windows installer](https://github.com/IWGF-elite/WinWiFi-Releases/releases/latest/download/WinWiFi-Security-Auditor-Setup.exe) · [Release notes and checksum](https://github.com/IWGF-elite/WinWiFi-Releases/releases/latest)

## Updating an existing installation

- **Direct-download installations:** use **Check for updates** at the bottom of the sidebar, or in **Settings & Lab → Application Updates**. A newer installer is downloaded and checked against its published SHA-256 before **Restart now** is offered. Approve the normal Windows administrator prompt to install it.
- If an older version checks an obsolete release address or cannot find this update, close WinWiFi and run the latest installer once. Keep the existing installation folder. You do not need to uninstall or clear saved history. Subsequent compatible direct-download builds use this release channel.
- **Microsoft Store installations:** update through Microsoft Store. The GitHub installer does not upgrade a Store MSIX. Version 3.0.10 here is a separate direct-download build; it does not alter the submitted Store 3.0.9 package.
- The current installer is **unsigned**. Windows may display an unknown-publisher or SmartScreen warning. A matching SHA-256 checks the file against this release; it is not a code-signing certificate or a guarantee of publisher identity.
## About

WinWiFi Security Auditor is a Windows desktop tool for reviewing nearby Wi-Fi security settings and authorized local-network audit information. It is intended for network owners, IT administrators, and security auditors working with networks they own or have written permission to assess.

The application performs passive wireless discovery and defensive analysis. Do not use it on networks or devices without authorization. It does not provide deauthentication, packet injection, or password-cracking features.

## What WinWiFi includes

- Live Windows Wi-Fi discovery with access-point identity, signal, channel, security, and available vendor details
- Explainable wireless risk factors, including encryption, WPS, and Protected Management Frames where the adapter exposes them
- Network inventory, project scope, authorization records, configuration history, and drift review
- Local network and connected-device visibility, gateway and DNS details, and network diagnostics supported by Windows
- Threat and security alerts based on observed scan data and configured scope
- Import and analysis of supported 802.11 PCAP/PCAPNG captures, including frame inspection and EAPOL handshake analysis
- Reports and exports based on data collected by the application

Capabilities vary by Windows version, Wi-Fi adapter, driver, permissions, and available capture support. The app reports unavailable hardware-dependent capabilities rather than inventing scan results. Raw 802.11 monitor-mode capture may require Npcap and a compatible adapter/driver; normal Windows Wi-Fi discovery does not require monitor mode.

## Install on Windows

1. Open the [latest release](https://github.com/IWGF-elite/WinWiFi-Releases/releases/latest).
2. Download **WinWiFi-Security-Auditor-Setup.exe** and its matching **.sha256** file.
3. Verify the download in PowerShell from the folder containing both files:

   ```powershell
   $expected = ((Get-Content .\WinWiFi-Security-Auditor-Setup.exe.sha256 -Raw) -split '\s+')[0]
   $actual = (Get-FileHash .\WinWiFi-Security-Auditor-Setup.exe -Algorithm SHA256).Hash.ToLowerInvariant()
   if ($actual -ne $expected) { throw "Checksum mismatch. Do not run this installer." }
   "Checksum verified."
   ```

4. Run the installer and follow its prompts. Windows may ask for administrator approval to install the app and enable supported WLAN operations.
5. Start **WinWiFi Security Auditor** from the Start menu.

### Requirements

- Windows 10 (version 19041 or later) or Windows 11, 64-bit
- A working Wi-Fi adapter for live wireless discovery
- The 3.0.10 installer includes the self-contained .NET runtime and Microsoft's signed offline WebView2 Runtime installer; no separate dashboard-runtime download is needed
- Optional: Npcap plus a monitor-mode-capable adapter and driver for raw 802.11 capture

The installer includes the app and its .NET runtime. Adapter capabilities differ across computers. LAN and capture features may also be limited by the local network, Windows permissions, and driver support.

## Release files

Each release provides the Windows setup executable and a SHA-256 checksum file. Compare the checksum before running the installer. Release notes describe changes in that version.

## Privacy Policy

**Effective date:** 6 November 2026

**Product:** WinWiFi Security Auditor

**Publisher display name:** Aldren Systems

**Current jurisdiction:** Pakistan; update if the publisher's legal establishment or markets change.
**Privacy contact:** `aldrensystems@gmail.com`

### What WinWiFi processes

WinWiFi is a Windows desktop network-auditing application. It does not require an online WinWiFi account and has no WinWiFi cloud workspace, advertising, behavioral analytics, or in-app checkout configured.

Depending on which features an operator uses, the application can read or process:

- **Wireless observations:** network names (including hidden-network status), BSSID/MAC addresses, vendors derived from public OUI data, radio band, channel, channel width, signal strength, advertised security settings, and observation times.
- **Local network inventory and diagnostics:** local IP and MAC addresses, hostnames discovered from the local network, gateway, DNS/DHCP configuration, reachability, connection state, and ports/services found by an operator-started local scan.
- **This PC's network details:** selected adapter information, the current Windows Wi-Fi connection, local addresses and routes, DNS configuration, and active connection metadata where the relevant view is used. Process names may be shown alongside this PC's own active connections.
- **Audit workspace records:** project and scope names, authorization confirmations, known-device choices, analyst names/roles entered by the operator, sites, notes, findings, alerts, investigation state, configuration history, and audit events.
- **Imported evidence:** parsed PCAP/PCAPNG or other supported import data. Captures can contain identifiers, addresses, DNS names, protocol metadata, and packet bytes. Treat capture files and derived reports as sensitive and import only evidence the operator is authorized to handle.
- **Windows-saved Wi-Fi credentials:** only if the operator explicitly requests the reveal action for the network Windows currently reports as connected. WinWiFi asks Windows for the saved profile value, displays the result locally, and does not intentionally log or retain the key. Windows remains responsible for saving and managing that profile. The key may be visible on screen while revealed.

The product uses these details to display current observations, compare history, calculate findings, maintain an authorized audit workspace, and produce reports requested by the operator. Results depend on the adapter, driver, Windows permissions, network, and imported evidence available at the time.

Recent Windows versions may require Windows Location consent to expose nearby Wi-Fi identifiers such as BSSIDs. WinWiFi uses that access to display and assess Wi-Fi observations. This build does not request a GPS position or calculate a precise physical location. Signal-survey room names are labels entered by the operator.

### Where information is stored and protected

The native workspace database is stored in the Windows user profile at `%LOCALAPPDATA%\WinWiFi\winwifi_audit.db`. The embedded WebView2 dashboard also stores a local workspace mirror and preferences in that Windows user profile. The local bridge listens on the PC's loopback interface; it is not a WinWiFi cloud endpoint.

Encrypted-storage builds encrypt the SQLite workspace with authenticated ChaCha20-Poly1305 encryption. A random key is protected by Windows DPAPI for the current Windows account. The embedded dashboard encrypts WebView2 local workspace values before writing them to local storage. On an existing installation, the app migrates the plaintext database in place, compares every table's row count, checks database integrity, and retains a temporary recovery copy until the encrypted replacement passes verification. If migration validation fails, the original database is restored. Keep the Windows account protected and back up the database and its matching `winwifi_audit.key.dpapi` file together; DPAPI-protected data is tied to the Windows user profile and is not portable to another account by copying the database alone.

The encryption update has not yet been published as a new installer or Store package. Older installed versions may store workspace data in plaintext. Rewriting encrypted WebView values does not guarantee removal of previous plaintext from browser storage files or backups.

This at-rest protection does not protect data while WinWiFi is running under your Windows account, from malware already running as that user or an administrator, from exported reports/captures, or from separate Windows and third-party records. Deleting workspace data is not a guarantee of forensic erasure from storage media, backups, or volume snapshots.

WinWiFi processes imported capture evidence locally and does not upload captures, scan results, inventory, projects, or reports to a WinWiFi service. When the operator exports a report or inventory, the chosen destination and any subsequent sharing are controlled by the operator.

### Connections outside this PC

The current direct-download build checks the **GitHub Releases** service for application updates and may download a versioned installer and its SHA-256 checksum. GitHub receives the request and ordinary network metadata such as the public IP address, time, and requested release asset. The update request does not include Wi-Fi observations, LAN inventory, project contents, imported captures, or report contents. The app verifies an update against its published checksum before staging it.

The application does not include an analytics or advertising service in the reviewed direct dependencies/source. Windows and Microsoft WebView2 provide operating-system and dashboard runtime functions. The direct installer may obtain the WebView2 runtime from Microsoft if it is not already present, depending on the installer build. Packaged builds open the Windows Store update view and block the GitHub/Inno Setup installer updater.

When the operator chooses **Open router page**, Windows opens the gateway address in the default browser. That browser then communicates with the router; this is not a WinWiFi upload.

### Retention and deletion

Workspace records remain in the local Windows profile until the operator deletes them or removes the app data. Settings & Lab provides a preview of native SQLite record counts and browser-workspace key counts, followed by a typed confirmation. The deletion action pauses and drains scanning, deletes WinWiFi workspace tables in a database transaction, checkpoints the database, and clears the browser-local workspace records while preserving UI preferences.

This control does not remove Windows Wi-Fi profiles, separately exported reports or captures, manually copied files, backups, volume snapshots, or data held by Windows or other software. Database deletion is not a guarantee of forensic erasure from storage media or backups. Uninstalling the app should not be treated as a substitute for using the in-app data-deletion control.

### Your choices and requests

The operator can choose which adapters and authorized projects to use, whether to import evidence, when to export a report, and whether to delete the local WinWiFi workspace. Access, correction, or privacy questions can be directed to the contact below; because the data is stored locally, the operator can also review or delete it directly in WinWiFi and Windows. Applicable legal rights depend on the relevant jurisdiction.

### Cookies, children, and contact

The Windows app does not set browser cookies. WebView2 local storage is used for application preferences and the local workspace mirror; it is not a cookie. No cookie-consent banner is shown because the desktop product does not currently use non-essential cookies. Any future website that introduces analytics, advertising, or other non-essential cookies must provide the consent controls required for that website and its visitors' jurisdictions.

WinWiFi is a network administration tool and is not directed at children. It does not ask for a user's age.

For privacy requests, contact `aldrensystems@gmail.com`. “Aldren Systems” is the publisher display name; the publisher should use the legal identity associated with its Partner Center account where Microsoft requires it.

### Changes to this policy

Update this policy when data handling, external connections, storage, or product features change. The policy supplied to Microsoft Store must match the submitted build and Store metadata. The current policy text is also published in the public WinWiFi-Releases README.

## Source code

This repository is for end-user downloads and release information. It does not contain the application's source code or project files.
