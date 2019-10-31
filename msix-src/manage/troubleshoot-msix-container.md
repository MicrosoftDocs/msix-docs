---
Description: This guide explains how to troubleshoot runtime issues in an MSIX container.
title: Troubleshoot runtime issues in an MSIX container 
ms.date: 07/11/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
---

# Troubleshoot runtime issues in an MSIX container 

In this article, we will review how you can troubleshoot runtime issues occurring in a MSIX container. MSIX containers by themselves are relatively simple and straightforward. As more applications are run inside the same package identity with the help of modification packages, the virtual registry and virtual file system will be over-layed in the order in which the applications are installed. 

There can be cases where the order in which these applications are installed could cause unforseen issues where the expected registry keys might be overwritten and expected files might be replaced. 

To assist in diagnosing such issues, [Invoke-CommandInDesktopPackage](https://docs.microsoft.com/powershell/module/appx/invoke-commandindesktoppackage?view=win10-ps) is a PowerShell cmdlet that can be used to run an application inside the MSIX container. This allows for users to run command prompt, registry editor, PowerShell inside the MSIX container and get a view of the merged file system and merged registry hive. 

 > [!IMPORTANT]
 > Invoke-CommandInDesktopPackage requires the device to be in Developer mode for Windows 10 builds prior to 18922.


## View the merged file system

To view the file system as observed by the applications that are running inside the container, use the following PowerShell command:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "Contoso.AppPackage1_8h66172c634n0" -Command "cmd.exe" -PreventBreakaway
```

The above command will launch an instance of cmd.exe in the *Contoso.AppPackage1_8h66172c634n0* package container. As you are running the command prompt from inside the container, you can browse through the file system and view the merged files. 

## View the merged registry hive

To view the full device registry hive as observed by the applications that are running insider the container, use the following PowerShell command:

``` PowerShell
Invoke-CommandInDesktopPackage -AppId "AppPackage1" -PackageFamilyName "Contoso.AppPackage1_8h66172c634n0" -Command "regedit.exe" -PreventBreakaway
```

The above command will launch registry editor within the context of the *Contoso.AppPackage1_8h66172c634n0* package container. Here you can browse through local machine and current user registry keys and identify possible offender that is causing the issue. 

 >[!TIP]
 > Use '-PreventBreakaway' flag while using Invoke-CommandInDesktopPackage if you would like to launch subsequent processes in the same container. Else, any subsequent launch will break out of the container. 

 >[!NOTE]
 > Not all applications can be launched within the container. For instance, explorer.exe will breakout of the container.
