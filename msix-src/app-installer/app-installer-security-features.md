---
title: App Installer Security Features
description: This article provides information on the security features provided by the App Installer.
ms.date: 7/1/2024
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload
ms.custom: 19H1
---

# App Installer Security Features

Build 1.24.1981 introduced the following App Installer security features:

* Internet warning
* Microsoft SmartScreen Reputation-based URL Validation
* URL Security Zones

## Internet Warning

App Installer displays a warning banner to the user whenever the user is installing a package from the internet. When the internet warning is shown, users should be careful to verify that the source listed on the dialog is trusted.

![Microsoft SmartScreen Error](./images/app-installer-ui-dialog-update.png)

Installing software from an untrusted site on the internet can be risky and expose you to malware and other exploits. For more information, see [Protect yourself from online scams and attacks](https://support.microsoft.com/office/protect-yourself-from-online-scams-and-attacks-0109ae3f-fe61-4262-8dce-2ee3cd43bac7)

## Microsoft SmartScreen Reputation-based URL Validation
The App Installer now takes advantage of [Microsoft SmartScreen](https://learn.microsoft.com/windows/security/operating-system-security/virus-and-threat-protection/microsoft-defender-smartscreen/) to help users make informed decsions before installing software.
Prior to downloading a package from an Internet source, App Installer will consult Microsoft SmartScreen's URL Reputation service. 

![Microsoft SmartScreen Error](./images/app-installer-smart-screen.png)

When presented with this error, the user can choose to **Cancel** or **Continue** (Not recommended).

Clicking continue will allow App Installer to open the package for installation.

## URL Security Zones
In addition to enabling and disabling the MS-AppInstaller protocol, IT Professionals can now prevent users from installing apps from URIs that the enterprise does not allow. IT Pros can disable installation from specific URL Security Zones.

When a user attempts to open a blocked URL, they will be presented with the following dialog.

![Internet Zone Error](./images/app-installer-zone-error.png)

### Configuring App Installers Zone

**EnableMSAppInstallerProtocol**
The entry *EnableMSAppInstallerProtocol* allows the IT Professionals to enable or disable the MS-AppInstaller protocol.
Enabled: <code>HKLM:\Software\Policies\Microsoft\Windows\AppInstaller EnableMSAppInstallerProtocol=1'</code>

**EnableMsixAllowedZones**

If *EnableMsixAllowedZones* is enabled (set to "1"), you will have the option to override whether App Installer allows a Security Zone or not.

Enabled: <code>'HKLM:\Software\Policies\Microsoft\Windows\AppInstaller" EnableMsixAllowedZones=1'</code>

**MsixAllowedZones**

When the *EnableMsixAllowedZones* is enabled, the App Installer will look to honor the restrictions specified in *MsixAllowedZones*. By default, the URLs in the *UntrustedSites* security zone will be rejected and all other zones will be allowed.

Allow zone: <code>HKLM:\Software\Policies\Microsoft\Windows\AppInstaller\MsixAllowedZones" UntrustedSites=1</code>

### Zone data

| Security Zone | Default | Detail 
| --- | --- | --- 
| Local Machine | Allow | Setting to *Blocked* will prevent any local MSIX from being installed.
| Intranet | Allow | Setting to *Blocked* will prevent files from enterprise servers from being downloaded and installed.
| Trusted Sites | Allow | When set to *Allow*, allows the IT professional to allow specific Internet URIs.
| Internet | Allow | When set to *Allow*, allows the IT professional to restrict installing apps from all Internet URIs.
| Untrusted Sites | Blocked | Used in conjunction with Trusted Sites, and Internet, allows the IT professional to block specific Internet URIs.


## App Installer CSP Security Zones
The App Installer access to URL Security Zones is controlled by the [DesktopAppinstaller CSP](https://learn.microsoft.com/windows/client-management/mdm/policy-csp-desktopappinstaller#enableappinstaller). If an App Installer attempts to load a URL from a zone that is blocked, the user will be presented with an error.

![Internet Zone Error](./images/app-installer-zone-error.png)

IT Professionals can add sites to the Restricted or Trusted Sites Zone by use of the [policy-csp-internetexplorer](https://learn.microsoft.com/windows/client-management/mdm/policy-csp-internetexplorer). If a URL appears in a zone that is blocked, the App Installer will block installation.


