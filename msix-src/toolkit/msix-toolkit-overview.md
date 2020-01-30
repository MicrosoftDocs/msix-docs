---
title: MSIX Toolkit Overview
description: This article provides details about the available MSIX toolkit.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix, msix toolkit
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX Toolkit
The [MSIX Toolkit](http://aka.ms/msixtoolkit) is a collection of tools and scripts focused on assisting IT Professionals and Developers to enhance their app package modernization journey. The toolkit will be open sourced on GitHub to allow customers and enthusiasts to contribute directly and provide suggestions and deefback on the content that is available.

### Principles
1. MSIX toolkit is a community led space where customers can freely contribute source code along with binaries and executables.
2. Users can't post artifacts that don't have a corresponding source code unless they are redistributables
3. Until enough community involvement, a Microsoft employee will be overseeing the decisions to approval of a apull request into the GitHub master branch.
4. All contributed source code will need to include a readme file with detailed instructions on the setup and how to build the source code.

### How to contribute
The MSIX toolkit GitHub repository will accept external contributions via a [pull request from a fork](https://help.github.com/en/articles/creating-a-pull-request-from-a-fork). Create only a single pull request per issue, while keeping the pull requests as small and contained to to a single scenario as possible. Before we can accept a pull request from you, you will need to sign a Contributor License Agreement (CLA).

Please visit the [MSIX toolkit ReadMe](https://github.com/microsoft/MSIX-Toolkit/blob/master/README.md) for more details on how to contribute. 

## Microsoft MSIX Toolkit Scripts
| Name | Description |
|------|-------------|
| [BatchConversion](./msix-toolkit-msixbatchconversion.md) | A set of PowerShell scripts that provide the ability to bulk conversion of applications into the MSIX application package format. |
| [ModifyPackagePublisher](./msix-toolkit-modifypackagepublisher.md) | A PowerShell script that provides the ability to update the publisher information of an MSIX application to align with the certificate used to sign the application. |


## Microsoft MSIX Toolkit Redistributables
| Name | Description|
|------|------------|
| [Redist.x64](https://github.com/microsoft/MSIX-Toolkit/tree/master/Redist.x64) | Includes the binaries and tools from the Windows 10 SDK that are essential while working with MSIX packages. These binaries are intended for a device operating on a 64-bit architecture. |
| [Redist.x86](https://github.com/microsoft/MSIX-Toolkit/tree/master/Redist.x86) | Includes the binaries and tools from the Windows 10 SDK that are essential while working with MSIX packages. These binaries are intended for a device operating on a 32-bit architecture. |

The redistributables (x86/x64) are used with the Microsoft MSIX Toolkit scripts providing the required executables.

## Microsoft MSIX Toolkit Tools
| Name | Description|
|------|------------|
| [AppInstallerFileBuilder](./msix-toolkit-appinstallerfilebuilder.md) | The AppInstaller File builder is a Windows 10 application intended to simplify the creation of AppInstaller files. This app allows the user to specify the app packages they would like to distribute, along with the update options. |

## Contributing
This project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the Microsoft Open Source Code of Conduct. For more information see the Code of Conduct FAQ or contact opencode@microsoft.com with any additional questions or comments.
