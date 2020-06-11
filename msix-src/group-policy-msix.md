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

If you have packaged your Win32 app into an MSIX (or if you built your app using the Desktop Bridge), your app has the full trust capability enabled. This allows you to read from the Group Policy Registry keys. At run time, your app will have the same view of the Group Policy Registry as it would if it had been installed using a different method. Starting in Windows 10 1809, if your app is a Universal Windows Platform (UWP) app it can access the same Group Policy keys. For more information about creating Group Policy, see [this article](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

If you are converting an existing installer to MSIX by using the [MSIX Packaging Tool](mpt-overview.md), there is no new work needed for your app to support Group Policy. Continue to manage Group Policies as you normally would for the original installer. Apps converted to MSIX will still be able to read from existing Group Policy Registry keys. 

Group Policy does not have native support to install MSIX applications. 

## Policies for blocking Microsoft Store and MSIX 
You may have your own requirements on how you want to configure the Microsoft Store and its App Updates. The Microsoft Store app is the one that triggers updates for Apps, which include the Calculater, Photos etc.. If the Microsoft Store App is removed, than that can lead to no updates. Here is the list of Microsoft Store policies and how it impacts your MSIX packages. 

| Policy | Description | GPO | Registry | App Updates |
|--------|--------------------|-----|--------------------|--------------|
|Turn Off Access to the Store|This policy setting specifies whether to use the Store service for finding an application to open a file with an unhandled file type or protocol association. When a user opens a file type or protocol that is not associated with any applications on the computer, the user is given the choice to select a local application or use the Store service to find an application. If you enable this policy setting, the "Look for an app in the Store" item in the Open With dialog is removed. If you disable or do not configure this policy setting, the user is allowed to use the Store service and the Store item is available in the Open With dialog.|Computer Configuration\Administrative Templates\System\Internet Communication Management\Internet Communication settings\
Registry: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer
NoUseStoreOpenWith REG_DWORD|not affected|
