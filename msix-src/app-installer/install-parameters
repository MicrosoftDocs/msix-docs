---
title: Passing installation parameters to your app via App Installer
description: Describes how to pass installation parameters to an app via App Installer and protocol activation.
author: Huios
ms.date: 12/17/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Passing installation parameters to your app via App Installer

When deploying your app as an MSIX via the web, you can pass installation parameters to your MSIX packaged application after it is installed. 
This article shows how you can use configure your MSIX packaged app to receive installation parameters, and setting up your installation uri with the parameters you need to pacc into your app. For more details see this [blog post](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/passing-installation-parameters-to-a-windows-application-with/ba-p/1719829).
## Configure your application for protocol activation

This enables your app to be launched using your custom defined protocol. When your packaged application is started by using a protocol, specified parameters can be passed to its activation event arguments so it can use the arguments when launched. 

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

##  Write code to handle parameters when app is launched

You need to write code in your application to handle the installation parameters that will be passed to your app at first launch.
```csharp
using Windows.Management.Deployment;

public async void CheckForAppInstallerUpdatesAndLaunchAsync(string targetPackageFullName, PackageVolume packageVolume)
{
    // Get the current app's package for the current user.
    PackageManager pm = new PackageManager();
    Package package = pm.FindPackageForUser(string.Empty, targetPackageFullName);

    PackageUpdateAvailabilityResult result = await package.CheckUpdateAvailabilityAsync();
    switch (result.Availability)
    {
        case PackageUpdateAvailability.Available:
        case PackageUpdateAvailability.Required:
            //Queue up the update and close the current instance
            await pm.AddPackageByAppInstallerFileAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.appinstaller"),
            AddPackageByAppInstallerOptions.ForceApplicationShutdown,
            packageVolume);
            break;
        case PackageUpdateAvailability.NoUpdates:
            // Close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
        case PackageUpdateAvailability.Unknown:
        default:
            // Log and ignore error.
            Logger.Log($"No update information associated with app {targetPackageFullName}");
            // Launch target app and close AppInstaller.
            await ConsolidateAppInstallerView();
            break;
    }
}
```

## Add your app activation protocol and parameters to the installation uri

Once your app is set up to handle your installation parameters, you can define specific parameters in your app installation uri and they will be passed in to your app at first launch. 
