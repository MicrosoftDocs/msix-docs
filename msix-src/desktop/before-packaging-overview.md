---
Description: An overview of topics to know before building an MSIX package
title: Key concepts to know before packing to MSIX
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.custom: RS5
---

# What to know before packaging your desktop application

This section contains requirements for a desktop app to run successfully as an MSIX package, and how a packaged desktop application interacts with Windows once packaged. This is useful to know before you begin the packaging process. If you're building a UWP app you can skip the sections on desktop applications below.

|Topic| Description |
|:---|:---|
|[Preparing to package your desktop application](desktop-to-uwp-prepare.md)| A list of requirements for a desktop application  to run successfully  once packaged as MSIX. |
|[How packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md)| A deeper dive into what happens to file system and registry entries when you package your Desktop application as MSIX. |
|[Flexible virtualization](flexible-virtualization.md)| The flexible virtualization feature provides a way for your app to declare that *some set* of its files and Registry entries should be visible to other apps; and that those should persist on app uninstall. *All other* files and Registry entries are not visible to other apps; and are removed on uninstall. |
|[Bundling MSIX Packages](../package/bundling-overview.md)| This section gives an overview of bundling multiple MSIX packages into a single .msixbundle.|
