---
title: How Group Policy works with MSIX packaged apps
description: Describes how Group Policy works with apps that are converted to MSIX.
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# Group Policy and MSIX packaged apps

Developers who use MSIX can leverage Group Policy in a similar way to other installer types.

If you have packaged your Win32 app into an MSIX (or if you built your app using the Desktop Bridge), your app has the full trust capability enabled. This allows you to read from the Group Policy registry keys. At run time, your app will have the same view of the Group Policy registry as it would if it had been installed using a different method. Starting in Windows 10, version 1809, if your app is a Universal Windows Platform (UWP) app it can access the same Group Policy keys. For more information about creating Group Policy, see [this article](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

If you are converting an existing installer to MSIX by using the [MSIX Packaging Tool](mpt-overview.md), there is no new work needed for your app to support Group Policy. Continue to manage Group Policies as you normally would for the original installer. Apps converted to MSIX will still be able to read from existing Group Policy registry keys. 

Group Policy does not have native support to install MSIX applications. 

## Policies for blocking Microsoft Store and MSIX 

You may have your own requirements on how you want to configure app udpates from the Microsoft Store app. The Store app triggers updates for apps, including third-party apps as well as first-party apps such as Calculater and Photos. If the Store app is removed from a computer, that can lead to no app updates on that computer. 

Here is the list of Store policies and how it impacts your MSIX packages. 

| Policy | Description | GPO | Registry | App updates |
|:--------|:--------------------|:--------|:--------|:--------|
|Turn off Automatic Download and Install of updates|Enables or disables the automatic download and installation of app updates. If you enable this setting, the automatic download and installation of app updates is turned off. If you disable this setting, the automatic download and installation of app updates is turned on. If you don't configure this setting, the automatic download and installation of app updates is determined by a registry setting that the user can change using **Settings** in the Windows Store.|Computer Configuration\Administrative Templates\Windows Components\Store |HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\WindowsStoreAutoDownload REG_DWORD (NB: enable = 2 = apps will not be updated, disable = 4 = app will be automatically updated) |Yes and No, Keyword here is automatic, the **Get Updates** button in the Store app will not be disabled.|
|Turn off Store application| Denies or allows access to the Store application. If you enable this setting, access to the Store application is denied. Access to the Store is required for installing app updates. If you disable or don't configure this setting, access to the Store application is allowed. | Computer Configuration\Administrative Templates\Windows Components\Store or User Configuration\Administrative Templates\Windows Components\Store | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\WindowsStoreRemoveWindowsStore REG_DWORD or HKEY_CURRENT_USER\Software\Policies\Microsoft\WindowsStoreRemoveWindowsStore REG_DWORD | If configured in the computer context, this policy turns off app updates. |
