# WinWiFi Security Auditor

Download the current Windows installer from the [latest release](https://github.com/IWGF-elite/WinWiFi-Releases/releases/latest). If that page has no version yet, a release has not been published here.

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
   if ($actual -ne $expected) { throw "Checksum mismatch �w^~)�t do not run this installer." }
   "Checksum verified."
   ```

4. Run the installer and follow its prompts. Windows may ask for administrator approval to install the app and enable supported WLAN operations.
5. Start **WinWiFi Security Auditor** from the Start menu.

### Requirements

- Windows 10 (version 19041 or later) or Windows 11, 64-bit
- A working Wi-Fi adapter for live wireless discovery
- Internet access during installation if Microsoft Edge WebView2 Runtime is not already installed
- Optional: Npcap plus a monitor-mode-capable adapter and driver for raw 802.11 capture

The installer includes the app and its .NET runtime. Adapter capabilities differ across computers. LAN and capture features may also be limited by the local network, Windows permissions, and driver support.

## Release files

Each release provides the Windows setup executable and a SHA-256 checksum file. Compare the checksum before running the installer. Release notes describe changes in that version.

## Privacy

WinWiFi stores audit workspace data on the Windows computer. It checks this public GitHub Releases page for application updates. Do not import or export data that you are not authorized to handle.

## Source code

This repository is for end-user downloads and release information. It does not contain the application's source code or project files.
