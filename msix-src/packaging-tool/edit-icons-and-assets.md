---
title: Edit icons and assets using the MSIX Packaging Tool
description: Describes how to edit icons and assets during the app conversion process while using the MSIX Packaging Tool.
ms.date: 06/27/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.custom: "RS5, seodec18"
---

# Edit icons and assets using the MSIX Packaging Tool

There are several ways you can modify your app package assets during the conversion process while using the MSIX Packaging Tool:

* You can modify your app package assets during the conversion process.
* After your package has been created, you can modify your app package assets via the [Package editor](package-editor.md).

Refer to the [guidelines for icon and assets](/windows/uwp/design/style/iconography/app-icon-design) as you make your modifications.

## Modify assets during the conversion process

During the monitored installation phase of the MSIX Packaging Tool workflow, the MSIX Packaging Tool will capture any changes that you make to files or folders for your application. If you change your assets during the monitoring, those changes will be captured in your final package.

## Modify assets after your package has been created

To modify your app package assets after you create your MSIX package, open your MSIX package in [Package editor](package-editor.md), and then open the **Package files** page. You can delete existing assets or add new icons or assets.

- To add a new asset file, right-click the assets folder, and select **Add file** or **Add folder**.
- To delete an existing asset file, right-click the file and select **Delete**.

To verify your asset changes, go to the **Package information** page and open your manifest file. Confirm that the assets you added or removed are represented in the [uap:VisualElements](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-visualelements) node. The manifest reference needs to be specific, but the asset files can be named whatever you like.
