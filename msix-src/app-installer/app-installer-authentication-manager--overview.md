---
title: App Installer Authentication Manager
description: Overview of the App Installer Authentication Manager
ms.date: 4/15/2021
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller
---

# App Installer Authentication Manager

The Microsoft Desktop App Installer supports OAuth 2.0 authentication requests for identities hosted in Microsoft Azure Active Directory (AAD), Google, and Dropbox to access the Windows app installation media. The user will only be prompted for authentication if the Windows app installation media is inaccessible, by first attempting to access the media for display to the user. 

If authentication is required, the URI targeting the installation media must provide the identity hosting solution by including the suffix of `?msixauth=<identity provider>` to the URI. By specifying the identity provider, the Microsoft Desktop App Installer will connect to the desired identity provider to request authentication.

| Identity Service  | Identity Connection String  |
|-------------------|-----------------------------|
| Microsoft AAD     | msixauth=aad                |
| Google            | msixauth=google             |
| Dropbox           | msixauth=dropbox            |

