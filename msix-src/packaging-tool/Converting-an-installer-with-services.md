---
title: Converting an installer with services
description: This article explains how to convert an existing installer with services to MSIX using the MSIX Packaging Tool
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, MSIX, MSIX Packaging Tool, services
ms.localizationpriority: medium
---

# Converting an installer with services

Coming in the 20H1 release of the Windows OS, you can run an MSIX package that has services in it. In order to get that MSIX with 
services, you can use the MSIX Packaging Tool to take an existing installer with services and convert it to MSIX. This support will be
available in the January release of the MSIX Packaging Tool, but you can try it out now in a preview release as part of our [MSIX 
Insider Preview Program](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/insider-program).

## Converting an installer with services using the MSIX Packaging Tool

Go through the MSIX Packaging Tool as you would with any [application package](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/create-app-package-msi-vm),
select an installer that has services, and you will see the Services report page before being able to create your MSIX. 

The Services report page is a report of which services were detected in your installer during conversion. Services
that have all the information they need and are supported will be shown in the ‘included’ table. Services that need additional 
information, need a fix, or aren’t supported will be shown in the 'excluded' table.

To fix a service or see additional data about the service, double click on the service entry in the table. There will be a 
pop-up containing information about the service. Here you can see your service information that you can then edit if you need to.  

- **Key name:** The name of the service - *not modifiable in the UI*
- **Description:** The description of the service entry
- **Display name:** The display name of the service
- **Image path:** Location of the service executable – *not modifiable in the UI*
- **Start account:** The start account for the service
- **Startup type:** Type of startup for the service. Supports Automatic, Manual, and Disabled
- **Arguments:** List of arguments to be run when the service starts
- **Dependencies:** List of dependencies that the service has

Once a service has been fixed, you can move it to the include table from the excluded table or you can choose to leave it in
the excluded table if you don’t want it in your final package. Then you can continue to the final step to create your MSIX 
package with services. 

## Known issues
- Currently the services executable path("Image path") is not editable within the UI. In order to fix any issues with your path, 
you will need to manually edit your service executable path before converting your installer. Alternatively, after conversion,
you can edit the manifest manually using the MSIX Packaging Tool’s Package Editor.
- Currently we do not have a service report available in the Package Editor, so you will need to manually edit the manifest
to make changes to the services included in your MSIX. 
