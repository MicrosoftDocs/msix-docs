---
title: Deploy MSIX Core with Microsoft Endpoint Configuration Manager
description: Describes how to deploy MSIX Core with Microsoft Endpoint Configuration Manager.
ms.date: 03/02/2020
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Deploy MSIX Core with Microsoft Endpoint Configuration Manager
MSIX Core enables an enterprise to standardize on the MSIX packaging format for their applications. IT Pros can easily deploy the MSIX Core installer to their devices and enables: install, query, and removal of MSIX packaged apps (that already work on those Windows versions).

MSIX Core provides a subset of features of native MSIX, functioning similar to existing Win32 installer types. MSIX Core does not provide the container benefits of native MSIX, nor enable an app that uses Windows 10 specific features to work on early Windows versions.

## Get started
The following steps will guide you through setting up an MSIX Core deployment strategy by using Microsoft Endpoint Configuration Manager:

1. Deploy MSIX Core with Microsoft Endpoint Configuration Manager
1. [Update your existing MSIX package to support MSIX Core](support-msix-core)
1. [Deploy MSIX Core Apps with Microsoft Endpoint Configuration Manager](deploy-msix-core-app-with-configmgr)

> [!Note]
> Optionally, after following this guide, you may elect to push the installation of the MSIX Core application on to all required devices within your environment prior to installing any MSIX Core enlightened apps.

## Creating the MSIX Core Application
The following will guide you through the creation of a Microsoft Endpoint Configuration Manager application, for the purpose of deploying MSIX Core to Windows 7 devices. If you intend to push this to other versions of the Windows operating system, please alter your application requirements to target those select operating system versions.
 
### Download MSIX Core
1. Download the latest version of [MSIX Core](https://github.com/microsoft/msix-packaging/releases).
1. Move the downloaded MSIX Core (x86 / x64) installation media to a file share which is accessible by your Microsoft Endpoint Configuration Manager environment.

### Create Microsoft Endpoint Configuration Manager Application - MSIX Core
1. Open your Microsoft Endpoint Configuration Manager Console. Navigate to **Software Library > Overview \ Application Management \ Applications.**
1. Select **Create Application** from the ribbon.
1. Ensure the **Type** has been set to **Windows Installer (*.msi file)**. 
1. Select the **Browse...** button and navigate to the downloaded MSIX Core (x64) installation media, then select the **Open** button.
1. Select the **Next** button twice.
1. Review the information listed, and update any blank or missing fields (optional).
1. Select the **Next** button twice.
1. Select the **Close** button.
1. Right-click on **MSIX Core** from within the list of applications and choose **Properties** from the drop-down menu.
1. Select the **Deployment Types** tab from the top of the newly opened window.
1. Select **MSIX Core - Windows Installer (*.msi file)** from the list of Deployment Types, and select the  **Edit** button.
1. Set the **Name** as "**MSIX Core - Windows Installer (*.msi file) - 64-bit**".
1. Select the **Requirements** tab at the top of the newly opened properties window.
1. Select the **Add** button.
1. Set the Category to **Device**, and set the Condition to be **Operating System**.
1. In the list of Operating Systems select the **Windows 7 > All Windows 7 (64-bit)** checkbox.
1. Select the **Ok** button two times.
1. Select the **Add** button.
1. Ensure the **Type** has been set to **Windows Installer(*.msi file)**.
1. Select the **Browse...** button and navigate to the downloaded MSIX Core (x86) installation media, then select the **Open** button.
1. Select the **Next** button twice.
1. Review the information listed, and update any blank or missing fields (optional).
1. Select the **Next** button twice.
1. Select the **Close** button.
1.  Select **MSIX Core - Windows Installer (*.msi file)** from the list of Deployment Types, and select the  **Edit** button.
1. Set the **Name** as "**MSIX Core - Windows Installer (*.msi file) - 32-bit**".
1. Select the **Requirements** tab at the top of the newly opened properties window.
1. Select the **Add** button.
1. Set the Category to **Device**, and set the Condition to be **Operating System**.
1. In the list of Operating Systems select the **Windows 7 > All Windows 7 (32-bit)** checkbox.
1. Select the **Ok** button three times.

## Distribute MSIX Core Application to Distribution Points
The following will guide you through distributing the MSIX Core installation media to all Distribution Points - assuming the Distribution Point group "All Distribution Points" exists in your environment. Please select the appropriate target (Distribution Point, Distribution Point Group, or Collection) for your environment.

1. From within the Microsoft Endpoint Configuration Manager console: **Software Library > Overview / Application Management / Applications** 
1. Right-click on **MSIX Core** from the list of applications.
1. Select **Distribute Content** from the drop-down menu.
1. Select the **Next** button twice.
1. Select the **Add** button and choose **Distribution Point Group** from the drop-down menu.
1. Select the **All Distribution Points** checkbox, and select the **Ok** button.
1. Select the **Next** button twice.
1. Select the **Close** button.
