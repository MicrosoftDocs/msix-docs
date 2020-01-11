---
title:  MSIX Support on Server 2019
description: This article provides details on on how MSIX support on Server 2019
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX Support on Server 2019

MSIX is now supported on Windows Server 2019 (LTSC) release with the Desktop Experience. Windows Server 2019 is based on the Windows version 1809 codebase where most of the MSIX capabilities are available and you will be able to take advantage of them.
 
The new support allows you to distribute the same MSIX packages within your enterprise on both client and server SKUs, using sideloading via **PowerShell** or installing directly via the **Package Manager APIs**. 
 
## Consideration
1. The Microsoft Store and the Microsoft Store for business are not supported on Windows server, so all applications will need to be sideloaded via **PowerShell** or **AppInstaller**.
2. As Windows Server 2019 is based on Windows 10 1809, that means it supports applications that have a minimum target version of **10.0.17763** or lower.
3. In addition, the following APIs and frameworks are not available on the server SKU: Microsoft Advertising framework, Geo Location, Cortana Search, In-App Purchase.
4. The following frameworks are not installed on server sku, ensure they are installed with the app when needed: .NET runtime, .NET framework, VCLibs.
5. Some MSIX extensions are not supported: Windows.firewallRules, Windows.appExecutionAlias, Windows.comServer, Windows.comInterface, Windows.posPaymentConnector, Windows.dataProtection.
 
As Windows Server 2019 with the Desktop Experience does not support all features of Windows 10 client skus, we recommend you test applications on server before deploying in production.
