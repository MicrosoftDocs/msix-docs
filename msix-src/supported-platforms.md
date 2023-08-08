---
title: MSIX features and supported platforms 
description: This topic describes supported platforms for MSIX, and lists feature additions by release. 
author: dianmsft
ms.date: 04/11/2023
ms.topic: article
keywords: windows 10, uwp, msix
ms.custom: RS5
---

# MSIX features and supported platforms

MSIX is currently supported on these versions of Windows:

* Windows 10, version 1709, and later.
* Windows Server 2019 LTSC, and later.
* Windows Enterprise 2019 LTSC and, later.

For more details on Windows lifecycle support such as end-of-service dates, see [Windows lifecycle fact sheet](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

This topic describes how key features of MSIX are supported in those versions of Windows.

> [!NOTE]
> Windows Server 2019 LTSC and Windows Enterprise 2019 LTSC require the **App Installer** app in order to support double-click install or install directly from a website for `.msix`, `.msixbundle`, `.appx`, and `.appxbundle`. Without that app, packages can be installed via PowerShell, API, or use a supported systems management product. For more considerations about Windows Server 2019 LTSC, see [MSIX support on Windows Server 2019](/windows/msix/msix-server-2019).

> [!NOTE]
> For versions of Windows earlier than Windows 10, version 1709, use [MSIX Core](msix-core/msixcore.md) to install MSIX packages.

## MSIX feature support

The table below shows which MSIX features and scenarios are supported in different versions of Windows.

### Windows desktop


> [!div class="mx-tableFixed"]
| Features                                                                                                                  | Windows 10 1809 (LTSC 2019) | Windows 10 1903    | Windows 10 1909    | Windows 10 2004    | Windows 10 20H2 (LTSC 2021) | Windows 10 21H1    | Windows 10 21H2    | Windows 11 21H2    |
|---------------------------------------------------------------------------------------------------------------------------|-----------------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|--------------------|--------------------|
| [Allow elevation](/windows/uwp/packaging/app-capability-declarations)                                                     | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [App Installer File Support](app-installer/installing-windows10-apps-web.md)                                              | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Defer registration flag](desktop/managing-your-msix-deployment-update.md)                                                | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Force update from any version downgrade](/windows/msix/desktop/managing-your-msix-deployment-targetdevices#upgrade-downgrade-and-architecture-considerations)                         | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Force provisioning](desktop/deploy-preinstalled-apps.md#provisioning-packages)        | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Identity for packaged desktop apps](package/persistent-identity.md)                                                                                        | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Modification packages](modification-packages.md)                                                                         | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Native MSIX install and uninstall](packaging-tool/know-your-installer.md)                                                                                         | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Package Support Framework (PSF)](psf/package-support-framework-overview.md)                                              | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Windows services](packaging-tool/convert-an-installer-with-services.md)                                                  | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Package Integrity Enforcement for non-Store packages](package/signing-package-overview.md#package-integrity-enforcement) | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Support for Windows App App Services](package/app-package-formats.md#app-services)         | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Shared package containers](manage/shared-package-container.md)                             | :x:                         | :x:                | :x:                | :x:                | :x:                         | :x:                | :x:                | :heavy_check_mark: |
| [Packages with external location](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)                                     | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Hosted Apps](/windows/uwp/launch-resume/hosted-apps)                                                                     | :x:                         | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Flexible virtualization](desktop/flexible-virtualization.md) | :x:                         | :x:                | :x:                | :x: | :x:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Start menu groups](packaging-tool/create-start-group.md) | :x:                         | :x:                | :x:                | :x: | :x:          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Legacy context menu support](packaging-tool/support-legacy-context-menus.md) | :x:                         | :x:                | :x:                | :x: | :x:          |  :x:  |  :x:  | :heavy_check_mark: |
| [Mutable package directories](manage/create-directory.md) | :x:                         | :x:                | :x:                | :x: | :x:          |  :x:  |  :x:  | :heavy_check_mark: |
| [MSIX Persistent Identity](package/persistent-identity.md) | :x:                         | :x:                | :x:                | :x: | :x:          |  :x:  |  :x:  | :heavy_check_mark: |
| [Dynamic dependencies](/windows/apps/desktop/modernize/framework-packages/use-the-dynamic-dependency-api) | :x:                         | :x:                | :x:                | :x: | :x:          |  :x:  |  :x:  | :heavy_check_mark: |

### Windows Server Support

> [!div class="mx-tableFixed"]
| Features                                                                                                                  | Windows Server 2019 | Windows Server 2022 |
|---------------------------------------------------------------------------------------------------------------------------|---------------------|---------------------|
| [Allow elevation](/windows/uwp/packaging/app-capability-declarations)                                                     | :heavy_check_mark:  | :heavy_check_mark:  |
| [App Installer File Support](app-installer/installing-windows10-apps-web.md)                                              | :heavy_check_mark:  | :heavy_check_mark:  |
| [Defer registration flag](desktop/managing-your-msix-deployment-update.md)                                                | :x:                 | :heavy_check_mark:  |
| [Force update from any version downgrade](desktop/managing-your-msix-deployment-targetdevices.md)                         | :heavy_check_mark:  | :heavy_check_mark:  |
| [Force provisioning](desktop/deploy-preinstalled-apps.md#provisioning-packages)                                                                 | :x:                 | :x:                 |
| [Identity for packaged desktop apps](package/persistent-identity.md)                                                      | :heavy_check_mark:  | :heavy_check_mark:  |
| [Modification packages](modification-packages.md)                                                                         | :heavy_check_mark:  | :heavy_check_mark:  |
| Native MSIX install and uninstall                                                                                         | :heavy_check_mark:  | :heavy_check_mark:  |
| [Package Support Framework (PSF)](psf/package-support-framework-overview.md)                                              | :heavy_check_mark:  | :heavy_check_mark:  |
| [Windows services](packaging-tool/convert-an-installer-with-services.md)                                                  | :x:                 | :heavy_check_mark:                 |
| [Package Integrity Enforcement for non-Store packages](package/signing-package-overview.md#package-integrity-enforcement) | :x:                 | :heavy_check_mark:                 |
| [Support for Windows App App Services](package/app-package-formats.md#app-services)                                      | :x:                 | :heavy_check_mark:                 |
| [Shared package containers](manage/shared-package-container.md)                                                           | :x:                 | :x:                 |
| [Packages with external location](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)                                     | :x:                 | :heavy_check_mark:  |
| [Hosted Apps](/windows/uwp/launch-resume/hosted-apps)                                                                     | :x:                 | :heavy_check_mark:  |

## Package format support

The following table shows which package formats are supported in different versions of Windows.

| Package format | Windows 10 (1809)  | Windows 10 (1903)  | Windows 10 (1909)  | Windows 10 (2004)  | Windows 10 (20H2)  | Windows 10 (21H1)  | Windows 10 (21H2)  | Windows 11 (21H2)  |
|----------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| .msixbundle    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| .appx          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| .appxbundle    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!Important]
> Prior to Windows 10 2004, sideloading of Windows apps must be enabled to allow non-Store Windows apps to be installed on Enterprise, Education, and LTSC SKUs. Windows 10 Home and Professional SKUs had sideloading of Windows apps enabled by default.

## Microsoft Store

The following table shows which Microsoft Store features are supported in different versions of Windows.

| Features            | Windows 10 (1809)  | Windows 10 (1903)  | Windows 10 (1909)  | Windows 10 (2004)  | Windows 10 (20H2)  | Windows 10 (21H1)  | Windows 10 (21H2)  | Windows 11 (21H2)  |
|---------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Publishing          | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Update Notification | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Streaming Install   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Delta Updates       | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> `.appx` and `.appxbundle` will work for all versions of Windows listed above. The table reflects only `.msix` or `.msixbundle` behavior.

### Microsoft Store submissions

The minimum supported OS version of an MSIX package is listed in the manifest file of the package as `MinVersion` in the `TargetDeviceFamily` element. For example, an MSIX package might list `MinVersion="10.0.17701.0"` as the minimum supported version, which means that the MSIX package can run on that and later versions of the OS.

On Windows 10, versions 1709, 1803, and 1809, we support the mainstream enterprise deployment scenarios. Those include installation through Microsoft Endpoint Configuration Manager, Microsoft Intune, PowerShell, or double-click installation.

Currently, MSIX installation through the Microsoft Store and Microsoft Store for Business requires Windows 10, version 1809, or later.

### Non-Windows platform

The [MSIX SDK](https://github.com/Microsoft/msix-packaging) is an open-source project that allows developers to use the MSIX package format universally on all platforms. The SDK can be used by any cross-platform client app that allows for third parties to build plugins or extensions. The client app developers can use the app extension model that's available on Windows, and use the MSIX SDK on the non-Windows platforms such as macOS, iOS, Android, and Linux.
