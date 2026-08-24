---
title:  MSIX Support on Server 2019
description: This article provides details on on how MSIX support on Server 2019
ms.date: 08/21/2026
ms.topic: article
author: andreww-msft
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.custom: "RS5, seodec18"
---

# MSIX support on Windows Server 2019

MSIX is now supported on Windows Server 2019 (LTSC) with the Desktop Experience. This support enables you to distribute the same MSIX packages within your enterprise on both client and server SKUs, installing via **PowerShell** or installing directly via the [Package Manager API](/uwp/api/Windows.Management.Deployment.PackageManager).

> [!NOTE]
> The authorization requirements for the [PackageManager](/uwp/api/windows.management.deployment.packagemanager) class depend on the caller. An app that runs in an AppContainer must declare the `packageManagement` [restricted capability](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) in its [package manifest](/uwp/schemas/appxpackage/appx-package-manifest). PowerShell and other callers that don't run in an AppContainer don't declare this capability. On Windows Server 2019, an unpackaged desktop process running at medium integrity level or higher can use the deployment APIs. A packaged desktop process can manage a package from the same publisher; managing packages from another publisher requires elevation.

## Considerations

* The Microsoft Store and the Microsoft Store for Business are not supported on Windows Server 2019. All applications must be installed using **PowerShell**.
* Because Windows Server 2019 is based on Windows 10, version 1809, it supports applications that have a minimum target version of **10.0.17763** or lower.
* The following APIs and frameworks are not available on Windows Server 2019:
  * Microsoft Advertising
  * Geolocation
  * Cortana Search
  * In-app purchase
* The following frameworks are not installed on Windows Server 2019. It is your responsibility to make sure they are installed with your app when needed.
  * .NET runtime
  * .NET Framework
  * VCLibs
* The following package extensions are not supported on Windows Server 2019:
  * windows.firewallRules
  * windows.appExecutionAlias
  * windows.comServer
  * windows.comInterface
  * windows.posPaymentConnector
  * windows.dataProtection

> [!NOTE]
> Because Windows Server 2019 with the Desktop Experience does not support all features of Windows 10 client SKUs, we recommend you test applications on Windows Server 2019 before deploying in production.
