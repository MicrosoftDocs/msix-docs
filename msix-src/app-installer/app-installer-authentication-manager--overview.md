---
title: App Installer Authentication Manager
description: Overview of the App Installer Authentication Manager
ms.date: 12/12/2023
ms.topic: article
keywords: windows 11, windows 10, uwp, app installer, AppInstaller
---

# App Installer Authentication Manager

The Microsoft Desktop App Installer on Windows 11 supports OAuth 2.0 authentication requests for identities hosted in Microsoft Azure Active Directory (AAD) to access the Windows app installation media. The user will only be prompted for authentication if the Windows app installation media is inaccessible, by first attempting to access the media for display to the user. 

If authentication is required, the URI targeting the installation media must provide the identity hosting solution by including the suffix of `?msixauth=<identity provider>` to the URI. By specifying the identity provider, the Microsoft Desktop App Installer will connect to the desired identity provider to request authentication.

| Identity Service  | Identity Connection String  |
|-------------------|-----------------------------|
| Microsoft AAD     | msixauth=aad                |

> [!Note]
> 1.  The values of each field must be URL-encoded, that is with non-printing characters and spaces. The use of question mark ("?") to separate the main Source from the field values, and ampersands ("&") to separate each subsequent fields in the `ms-appinstaller:`.
>     Example: `ms-appinstaller:?source=https://website.com/app.msix&msixauth=aad`
> 
> 2.The ability to use the *ms-appinstaller* URI (Uniform Resource Identifier) scheme (protocol) can be controlled by an IT professional (an administrator). To disable *ms-appinstaller* on your network, set the Group Policy **EnableMSAppInstallerProtocol** (/windows/client-management/mdm/policy-csp-desktopappinstaller) to diabled (see [Policy CSP - DesktopAppInstaller](/windows/client-management/mdm/policy-csp-desktopappinstaller#enablemsappinstallerprotocol)). For more info, see [Installing Windows 10 apps from a web page](/windows/msix/app-installer/installing-windows10-apps-web).
