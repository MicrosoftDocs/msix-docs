---
title: Update non-Store published apps from your code
description: Describes how MSIX packages shipped outside the Store can be updated by developers in code. 
author: Huios
ms.date: 06/12/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Update non-Store published app packages from your code

When shipping your app as an MSIX you can programmatically kick-off an update of your application. If you deploy your app outside the Store, all you need to do is check your server for a new version of your app and install the new version. How you apply the update depends on whether you are deploying your app package using an App Installer file or not. In order to apply updates from your code, your app package must declare the `packageManagement` capability.

This article provides examples that demonstrate how to declare the `packageManagement` capability in your package manifest and how to apply an update from your code. The first section looks at how to do this if you're using the App Installer file and the second section is about how to do so when **not** using the App Installer file. The last section looks at how to make sure your app restarts after an update has been applied.

## Add the PackageManagement Capability to your package manifest

To use the `PackageManager` APIs, your app must declare the `packageManagement` [restricted capability](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) in your [package manifest](/uwp/schemas/appxpackage/appx-package-manifest).

```xml
<Package>
...

  <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
...
</Package>
```

## Updating packages deployed using an App Installer file

If you are deploying your application using the App Installer file, any code driven updates you perform must make use of the [App Installer file APIs](./app-installer/app-installer-documentation.md#app-installer-file-apis). Doing so ensures that your regular App Installer file updates will continue to work. For intiating an App Installer based update from your code you can use [PackageManager.AddPackageByAppInstallerFileAsync](/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync?view=winrt-19041) or [PackageManager.RequestAddPackageByAppInstallerFileAsync](/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync?view=winrt-19041). You can check if an update is available using the [Package.CheckUpdateAvailabilityAsync](/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync?view=winrt-19041) API. Below is example code:

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

## Updating packages deployed without an App Installer file


### Check for updates on your server

If you are **not** using the App Installer file to deploy your app package, the first step is to directly check if a new version of your application available. The following example checks to see of the version of the package on a server is greater than the current version of the app (this example refers to a test server for demonstration purposes).

```csharp
using Windows.Management.Deployment;

//check for an update on my server
private async void CheckUpdate(object sender, TappedRoutedEventArgs e)
{
    WebClient client = new WebClient();
    Stream stream = client.OpenRead("https://trial3.azurewebsites.net/HRApp/Version.txt");
    StreamReader reader = new StreamReader(stream);
    var newVersion = new Version(await reader.ReadToEndAsync());
    Package package = Package.Current;
    PackageVersion packageVersion = package.Id.Version;
    var currentVersion = new Version(string.Format("{0}.{1}.{2}.{3}", packageVersion.Major, packageVersion.Minor, packageVersion.Build, packageVersion.Revision));

    //compare package versions
    if (newVersion.CompareTo(currentVersion) > 0)
    {
        var messageDialog = new MessageDialog("Found an update.");
        messageDialog.Commands.Add(new UICommand(
            "Update",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.Commands.Add(new UICommand(
            "Close",
            new UICommandInvokedHandler(this.CommandInvokedHandler)));
        messageDialog.DefaultCommandIndex = 0;
        messageDialog.CancelCommandIndex = 1;
        await messageDialog.ShowAsync();
    } else
    {
        var messageDialog = new MessageDialog("Did not find an update.");
        await messageDialog.ShowAsync();
    }
}
```

### Apply the update 

After you determined that an update is available, you can queue it up for download and install using the [AddPackageAsync](/uwp/api/windows.management.deployment.packagemanager.addpackageasync?view=winrt-19041) API. It should also work to install an optional package as long as the main package is already installed on the device. The update will be applied the next time your app is shut down. After the app is restarted, the new version will be available to the user. Below is example code:

```csharp

// Queue up the update and close the current app instance.
private async void CommandInvokedHandler(IUICommand command)
{
    if (command.Label == "Update")
    {
        PackageManager packagemanager = new PackageManager();
        await packagemanager.AddPackageAsync(
            new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
            null,
            AddPackageOptions.ForceApplicationShutdown
        );
    }
}
```

## Automatically restarting your app after an update

If your application is a UWP app, passing in AddPackageByAppInstallerOptions.ForceApplicationShutdown OR AddPackageOptions.ForceTargetAppShutdown when applying an update should schedule the app to restart after the shutdown + update. For non-UWP apps you need to call [RegisterApplicationRestart](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#updates) before applying the update.

You must call RegisterApplicationRestart before your app begins to shut down. Below is an example of doing so using interop services to call the native method in C#:

```csharp
 // Register the active instance of an application for restart in your Update method
 uint res = RelaunchHelper.RegisterApplicationRestart(null, RelaunchHelper.RestartFlags.NONE);
```

An example of the helper class to call the native RegisterApplicationRestart method in C#:

```csharp
using System;
using System.Runtime.InteropServices;

namespace MyEmployees.Helpers
{
    class RelaunchHelper
    {
        #region Restart Manager Methods
        /// <summary>
        /// Registers the active instance of an application for restart.
        /// </summary>
        /// <param name="pwzCommandLine">
        /// A pointer to a Unicode string that specifies the command-line arguments for the application when it is restarted.
        /// The maximum size of the command line that you can specify is RESTART_MAX_CMD_LINE characters. Do not include the name of the executable
        /// in the command line; this function adds it for you.
        /// If this parameter is NULL or an empty string, the previously registered command line is removed. If the argument contains spaces,
        /// use quotes around the argument.
        /// </param>
        /// <param name="dwFlags">One of the options specified in RestartFlags</param>
        /// <returns>
        /// This function returns S_OK on success or one of the following error codes:
        /// E_FAIL for internal error.
        /// E_INVALIDARG if rhe specified command line is too long.
        /// </returns>
        [DllImport("kernel32.dll", CharSet = CharSet.Unicode)]
        internal static extern uint RegisterApplicationRestart(string pwzCommandLine, RestartFlags dwFlags);
        #endregion Restart Manager Methods

        #region Restart Manager Enums
        /// <summary>
        /// Flags for the RegisterApplicationRestart function
        /// </summary>
        [Flags]
        internal enum RestartFlags
        {
            /// <summary>None of the options below.</summary>
            NONE = 0,

            /// <summary>Do not restart the process if it terminates due to an unhandled exception.</summary>
            RESTART_NO_CRASH = 1,
            /// <summary>Do not restart the process if it terminates due to the application not responding.</summary>
            RESTART_NO_HANG = 2,
            /// <summary>Do not restart the process if it terminates due to the installation of an update.</summary>
            RESTART_NO_PATCH = 4,
            /// <summary>Do not restart the process if the computer is restarted as the result of an update.</summary>
            RESTART_NO_REBOOT = 8
        }
        #endregion Restart Manager Enums

    }
}
```
