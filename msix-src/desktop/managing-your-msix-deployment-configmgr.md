---
description: This guide provides complete instructions for deploying MSIX apps to managed Windows devices using Microsoft Configuration Manager, including code signing, certificate trust, application creation, supersedence-based updates, and troubleshooting.
title: Deploy MSIX apps with Microsoft Configuration Manager
ms.date: 04/17/2026
ms.topic: how-to
keywords: windows 10, windows 11, configmgr, configuration manager, sccm, msix, lob app, code signing, certificate, deployment, sideloading
---

# Deploy MSIX apps with Microsoft Configuration Manager

Microsoft Configuration Manager (ConfigMgr) can deploy MSIX-packaged apps to on-premises managed Windows devices silently, without user interaction. This guide covers the full deployment workflow: code signing, certificate trust distribution, application creation, collection targeting, supersedence-based updates, and troubleshooting.

## Prerequisites

Before deploying an MSIX app through ConfigMgr, make sure you have:

- An MSIX package built and signed (see [Code signing requirements](#code-signing-requirements))
- A UNC path to the MSIX package accessible from the ConfigMgr distribution point, or the package available on a ConfigMgr distribution point
- A device or user collection to target
- Configuration Manager Current Branch (or SCCM 1706 or later)
- Sideloading enabled on target devices (see [Enable sideloading](#enable-sideloading-on-target-devices))

## Code signing requirements

Every MSIX package deployed through ConfigMgr must be signed. Windows will not install an unsigned MSIX, even when deployed silently via ConfigMgr.

You have two options:

### Option 1: Self-signed certificate (no cost, for domain-managed devices only)

A self-signed certificate can be used at no cost, but the certificate must be trusted on every target device before deployment. On ConfigMgr-managed devices, you distribute certificate trust using **Group Policy** or a ConfigMgr **Certificate Profile**.

This option is suitable when all devices are domain-joined and on-premises managed, and the app will never be distributed beyond your organization.

### Option 2: Azure Artifact Signing (formerly Trusted Signing) (recommended for most scenarios)

[Azure Artifact Signing (formerly Trusted Signing)](/azure/trusted-signing/) provides a CA-trusted certificate with no hardware token required. Because the certificate is issued by a CA that Windows already trusts, no additional certificate deployment is needed on target devices. For current pricing, see the [Azure Artifact Signing pricing page](https://azure.microsoft.com/pricing/details/trusted-signing/).

For guidance on signing, see [Sign an app package using SignTool](/windows/msix/package/sign-app-package-using-signtool).

---

## Enable sideloading on target devices

MSIX apps deployed outside the Microsoft Store require sideloading to be enabled on target devices.

| Windows version | Sideloading default | Action required |
|---|---|---|
| Windows 11 | Enabled by default | None |
| Windows 10 (version 2004 and later) | Enabled by default | None |
| Windows 10 (before version 2004) | Disabled by default | Enable via Group Policy |

To enable sideloading via Group Policy on older Windows 10 devices:

1. Open **Group Policy Management** and create or edit a GPO for the OU containing your managed devices
2. Navigate to **Computer Configuration** > **Administrative Templates** > **Windows Components** > **App Package Deployment**
3. Enable **Allow all trusted apps to install** (also referred to as `AllowAllTrustedApps`)
4. Link the GPO to the OU containing your managed devices

> [!NOTE]
> On Windows 10 version 2004 and later, and all Windows 11 devices, sideloading is on by default. The **Allow all trusted apps to install** policy is not required.

---

## Distribute certificate trust (self-signed only)

If you're using a **self-signed certificate**, you must deploy the certificate's public key to target devices before the app arrives. If using Azure Artifact Signing, skip this section.

### Using Group Policy

1. Export the public key (`.cer`) from your signing certificate
2. In **Group Policy Management**, create or edit a GPO targeting your device OU
3. Navigate to **Computer Configuration** > **Windows Settings** > **Security Settings** > **Public Key Policies** > **Trusted People**
4. Right-click **Trusted People** > **Import** and select your `.cer` file
5. Apply and link the GPO — devices receive the certificate at next Group Policy refresh

### Using a ConfigMgr Certificate Profile

Alternatively, create a **Certificate Profile** in ConfigMgr:

1. In the ConfigMgr console, go to **Assets and Compliance** > **Compliance Settings** > **Company Resource Access** > **Certificate Profiles**
2. Create a **Trusted CA Certificate** profile and upload your `.cer` file
3. Set the **Certificate store** to **Trusted People**
4. Deploy the profile to the same device collection before deploying the app

> [!IMPORTANT]
> Certificate trust must reach devices **before** the app is installed. If the app arrives first, installation can fail with a certificate trust error. Deploy the certificate policy first, then the app.

---

## Create the application in ConfigMgr

1. In the ConfigMgr console, go to **Software Library** > **Application Management** > **Applications**
2. On the ribbon, select **Create Application**
3. On the **General** page, select **Automatically detect information about this application from installation files**, set the type to **Windows app package (\*.appx, \*.appxbundle, \*.msix, \*.msixbundle)**, and provide the UNC path to your MSIX file
4. ConfigMgr reads the package manifest and auto-populates **Name**, **Publisher**, **Version**, and detection method — review these fields
5. Complete **Software Center** display information (description, icon) on the **Software Center** tab if desired
6. On the **Deployment Types** page, confirm the auto-created deployment type. The installation and uninstall commands are set automatically for MSIX packages.
7. Select **Next** through the remaining pages, review, and select **Finish**

> [!NOTE]
> ConfigMgr automatically determines the install and detection logic for MSIX packages. You do not need to specify custom install strings or detection scripts.

## Deploy the application to a collection

1. In **Applications**, right-click your app and select **Deploy**
2. On the **General** page, select the device or user collection to target
3. On the **Content** page, confirm that the package is distributed to the appropriate distribution points
4. On the **Deployment Settings** page:
   - Set **Action** to **Install**
   - Set **Purpose** to **Required** for silent install, or **Available** to show in Software Center
5. On the **Scheduling** page, set when the deployment becomes available and any deadline
6. Review and select **OK** to save the deployment

Clients receive the deployment on their next policy refresh cycle (default: 60 minutes). You can trigger an immediate sync from the client: open **Configuration Manager** in the system tray and select **Machine Policy Retrieval & Evaluation Cycle**.

## Update management

ConfigMgr manages MSIX updates using **application supersedence**:

1. Create a new application for the updated MSIX version (repeat the steps above)
2. In the original application's properties, go to the **Supersedence** tab
3. Select **Add** and choose the new application as the superseding app
4. For a normal MSIX update where the package identity stays the same and the version increases, leave **Uninstall** unchecked — MSIX upgrades in place. Only check **Uninstall** when the new package cannot supersede the old one in place, such as after a package identity change or major repackaging.
5. Deploy the new application as **Required** to the same collection

> [!NOTE]
> Unlike Intune, ConfigMgr does not auto-detect MSIX version changes on the same application entry. You must create a new application entry and configure supersedence for each update.

## Windows 10 vs Windows 11 considerations

Some MSIX deployment behaviors differ between Windows 10 and Windows 11:

| Scenario | Windows 10 | Windows 11 |
|---|---|---|
| Sideloading default | May require GPO (pre-2004) | On by default |
| App registration for all users | Requires elevated session | Simplified |
| Known MSIX packaging bugs | More unbackported issues | Better platform support |

If you are seeing MSIX failures specifically on Windows 10 that do not reproduce on Windows 11, check open issues in [microsoft/msix-packaging](https://github.com/microsoft/msix-packaging/issues) — some bugs affecting Windows 10 have not been backported.

## Troubleshooting

### Certificate trust failure

If using a self-signed certificate and the certificate hasn't reached the device yet, MSIX installation fails silently. To diagnose:

- Run `gpresult /r` on the device and confirm the certificate Group Policy is listed under applied policies
- Open **certlm.msc** and verify the signing certificate appears in **Local Computer > Trusted People**
- If the certificate is missing, check policy application timing — the cert must arrive before the app

### 0x80073CF3 — Package failed update, dependency, or conflict validation

This error indicates `ERROR_INSTALL_PACKAGE_DOWNGRADE` — a newer version of the package is already installed on the device. Check:

- The version in the MSIX manifest is higher than the version currently installed on the device
- If needed, uninstall the existing package from the device before retrying the deployment
- For additional causes, see [MSIX deployment troubleshooting](managing-your-msix-deployment-troubleshooting.md)

### 0x80073CF0 — Package file can't be opened

The MSIX file cannot be opened from the distribution point path. Check:
- The UNC path is accessible from the client
- The distribution point has the content distributed (check **Monitoring** > **Distribution Status**)
- The file is not corrupted — copy it locally and try `Add-AppxPackage` to confirm

### 0x8007000D — Invalid data / publisher mismatch

The `Publisher` value in the MSIX manifest does not match the Subject of the signing certificate exactly (including spacing and capitalization). Rebuild and re-sign the package after correcting the manifest.

### App stuck in "Waiting for content"

The content has not been distributed to the distribution point serving the target devices. In the ConfigMgr console:
1. Right-click the deployment type and select **Update Distribution Points**
2. Monitor distribution progress in **Monitoring** > **Distribution Status** > **Content Status**

### Check the client deployment log

On the target device, open:
```
C:\Windows\CCM\Logs\AppEnforce.log
```

Search for your application name to find the relevant install attempt, exit code, and error details.

## Related content

- [Sign an app package using SignTool](/windows/msix/package/sign-app-package-using-signtool)
- [Azure Artifact Signing (formerly Trusted Signing) documentation](/azure/trusted-signing/)
- [Deploy MSIX apps with Microsoft Intune](managing-your-msix-deployment-intune.md)
- [MSIX deployment troubleshooting](managing-your-msix-deployment-troubleshooting.md)
- [Create Windows applications in ConfigMgr](/mem/configmgr/apps/get-started/creating-windows-applications)

