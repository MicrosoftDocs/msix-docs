---
description: Quick reference for deploying MSIX apps using the Microsoft Intune admin center web console.
title: Deploy MSIX apps using the Intune admin center
ms.date: 04/17/2026
ms.topic: how-to
keywords: windows 10, windows 11, intune, admin center, msix, lob app
---

# Deploy MSIX apps using the Intune admin center

The [Microsoft Intune admin center](https://intune.microsoft.com/) is the web console for creating and managing Line-of-Business app deployments. This article is a quick reference for the admin center steps. For full guidance — including code signing, certificate trust distribution, update management, and troubleshooting — see [Deploy MSIX apps with Microsoft Intune](managing-your-msix-deployment-intune.md).

## Quick steps: Add an MSIX Line-of-Business app

1. Sign in to the [Intune admin center](https://intune.microsoft.com/)
2. Go to **Apps** > **All apps** > **Add**
3. Under **App type**, select **Line-of-business app**, then select **Select**
4. Under **App package file**, upload your signed `.msix` or `.msixbundle` file
5. Review the auto-populated app metadata (Name, Publisher, Version)
6. Complete the **Description** field, set **App install context** to **Device** (recommended), then select **Next**
7. On **Assignments**, add the Entra ID groups that should receive the app and set the intent (**Required** for silent install)
8. Select **Next**, review, then **Create**

> [!NOTE]
> Your MSIX package must be signed before upload. If using a self-signed certificate, deploy a **Trusted Certificate** configuration profile to target devices first — otherwise installation will fail. For details, see [Deploy MSIX apps with Microsoft Intune](managing-your-msix-deployment-intune.md).

## Monitor deployment status

After creating the app:

1. Go to **Apps** > **All apps** and select your app
2. Select **Device install status** or **User install status**
3. Look for **Installed** per device; investigate **Failed** entries using the error code shown

## Related content

- [Deploy MSIX apps with Microsoft Intune (full guide)](managing-your-msix-deployment-intune.md)
- [Create a Line-of-Business app in Intune](/mem/intune/apps/lob-apps-windows)

