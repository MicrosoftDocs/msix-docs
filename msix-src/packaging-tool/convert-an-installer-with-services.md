---
title: Converting an installer with services
description: This article explains how to convert an existing installer with services to MSIX using the MSIX Packaging Tool
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, MSIX, MSIX Packaging Tool, services
ms.localizationpriority: medium
---

# Convert an installer that includes services

Windows 10, version 2004, introduces support for running an MSIX package that includes services. You can use the MSIX Packaging Tool to take an existing installer with services and convert it to MSIX. This support is as of the January 2020 release of the [MSIX Packaging Tool](tool-overview.md)(1.2019.1220.0). Once you have a packaged MSIX with a service, it will require admin privileges to install on a machine.

## Instructions

To convert an installer that includes services, use the MSIX Packaging Tool as you would with any [application package](create-app-package.md). Select an installer that has services, and you will see the **Services** report page before the final step to create your MSIX package.

The **Services** report page lists services that were detected in your installer during conversion. Services that have all the information they need and are supported will be shown in the **Included** table. Services that need additional information, need a fix, or aren’t supported will be shown in the **Excluded** table.

To fix a service or see additional data about the service, double-click the service entry in the table to view a pop-up with more information about the service. You can edit some of this information if you need to.

- **Key name:** The name of the service. This is not editable.
- **Description:** The description of the service entry.
- **Display name:** The display name of the service.
- **Image path:** Location of the service executable. This is not editable.
- **Start account:** The start account for the service.
- **Startup type:** Type of startup for the service. Supports **Automatic**, **Manual**, and **Disabled**.
- **Arguments:** Arguments to be run when the service starts.
- **Dependencies:** Dependencies for the service.

After a service has been fixed, you can move it to the **Included** table or you can choose to leave it in the **Excluded** table if you don’t want it in your final package. Then, you can continue to the final step to create your MSIX package.

## Known limitations

The services executable path (also called the image path) is currently not editable. To fix any issues with your path, you must manually edit your service executable path before converting your installer. Alternatively, after conversion you can edit the manifest manually using the [Package Editor](package-editor.md) in the MSIX Packaging Tool.

The Services report is currently not available in the **Package Editor**. You must manually edit the manifest to make changes to the services included in your MSIX package.

We currently do not support services with dependencies outside the package.

## Add a service manually using your manifest

If you are manually adding a service to your application, you will need to [add a service](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-service) to your app manifest. This does require a [restricted capability](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) to add to your application.
