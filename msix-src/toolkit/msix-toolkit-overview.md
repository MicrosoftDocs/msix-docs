---
title: MSIX Toolkit
description: This article provides details about the available MSIX toolkit.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix, msix toolkit
ms.custom: 19H1
---

# MSIX Toolkit

The [MSIX Toolkit](https://aka.ms/msixtoolkit) is a collection of tools and scripts that enable IT Pros and developers to build and manage MSIX packages. The toolkit is an open source project on GitHub to allow customers and enthusiasts to contribute directly and provide suggestions and feedback on the content that is available.

## Principles

The MSIX Toolkit follows these principles:

* It is a community-led space where customers can freely contribute source code along with binaries and executables.
* Users can't post artifacts that don't have a corresponding source code unless they are redistributables.
* Until there is sufficient community involvement, a Microsoft employee will oversee the pull requests.
* All contributed source code must include a readme file with detailed instructions on the setup and how to build the source code.

## How to contribute

The [MSIX Toolkit](https://aka.ms/msixtoolkit) repository accepts external contributions via a [pull request from a fork](https://help.github.com/en/articles/creating-a-pull-request-from-a-fork). Create only a single pull request per issue, while keeping the pull requests as small and contained to to a single scenario as possible. Before we can accept a pull request from you, you must sign a Contributor License Agreement (CLA).

See the [readme](https://github.com/microsoft/MSIX-Toolkit/blob/master/README.md) for more details on how to contribute.

## Microsoft-provided content in the toolkit

Microsoft includes the following scripts, redistributables, and tools in the MSIX toolkit.

## Scripts

| Name | Description |
|------|-------------|
| [BulkConversion](msix-toolkit-msixbatchconversion.md) | A set of PowerShell scripts that provide the ability to bulk conversion of applications into the MSIX application package format. |
| [ModifyPackagePublisher](msix-toolkit-modifypackagepublisher.md) | A PowerShell script that provides the ability to update the publisher information of an MSIX application to align with the certificate used to sign the application. |

## Redistributables

| Name | Description|
|------|------------|
| [Redist.x64](https://github.com/microsoft/MSIX-Toolkit/tree/master/Redist.x64) | Includes the binaries and tools from the Windows 10 SDK that are essential while working with MSIX packages. These binaries are intended for a device operating on a 64-bit architecture. |
| [Redist.x86](https://github.com/microsoft/MSIX-Toolkit/tree/master/Redist.x86) | Includes the binaries and tools from the Windows 10 SDK that are essential while working with MSIX packages. These binaries are intended for a device operating on a 32-bit architecture. |

The redistributables (x86/x64) are used with the Microsoft MSIX Toolkit scripts providing the required executables.

## Tools

| Name | Description|
|------|------------|
| [AppInstallerFileBuilder](./msix-toolkit-appinstallerfilebuilder.md) | The AppInstaller File Builder tool is a Windows 10 app that simplifies the creation of AppInstaller files. This app allows the user to specify the app packages they would like to distribute, along with the update options. |
