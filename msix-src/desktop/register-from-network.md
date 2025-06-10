---
description: This article provides guidance on how to register a package layout from a network share
title: Registering a package layout from a network share
ms.date: 02/06/2020
ms.topic: how-to
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
---

# Registering a package layout from a network share

Collaborative and multi-device development can be time consuming due to the need to copy packages, repositories, build folders etc. If you're developing on Windows 10 version 1709 and later you can take advantage of a feature to build your package layout to a network share and then register the layout on a remote device directly from the network.

Multiple people can contribute to a single app package layout on a network share and changes will be visible to other collaborators and users who registered the app. If you are building an app for multiple devices you can take advantage of the feature and avoid having to copy the app to each device for testing.

## Prerequisites

1. Your devices must be on Windows 10 Creators Update Insider Build 14965 or later.

2. You will need to enable developer mode and device discovery on all devices.

3. Collaborators will need read and write access to the build folder.

4. Users will only need to read access to the build folder.

## In Visual Studio

If you are developing in Visual Studio you can follow the steps outlined [here](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#advanced-remote-deployment-options).

## From the command line

If you are not developing in Visual Studio, and using command line tools you can use  [WinAppDeployCmd](/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool). Below is an example of how to do so from a command line window:

```
WinAppDeployCmd.exe registerfiles -remotedeploydir <network path> -ip <IP Address> -pin <target machine PIN>
```
- Network Path – Path to the app’s loose files

- P Address – enter the IP Address of the target machine here

- Pin - A pin if it is required to establish a connection with the target device. (You will be prompted to retry with the -pin option if authentication is required.) Click here to learn how to get a PIN.
 

You can also build a fully packaged application that accesses files in a network share during testing and validation. See this sample for an [example](https://github.com/AppInstaller/Windows-appsample-marble-maze).
