---
title: App Installer file API issues
description: This article lists known issues with the PackageManager and Package APIs for managing App Installer files.
ms.date: 2/20/2019
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, API
ms.custom: 19H1
---

# App Installer file API issues

## JavaScript support for App Installer file APIs

The [PackageManager](/uwp/api/windows.management.deployment.packagemanager) and [Package](/uwp/api/windows.applicationmodel.package) classes in the Windows SDK provide methods you can use to add or modify packages via App Installer files or to retrieve information about apps with an App Installer association. For more information, see [Related documentation](app-installer-documentation.md).

Of these methods, [PackageManager.AddPackageByAppInstallerFileAsync](/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync), [PackageManager.RequestAddPackageByAppInstallerFileAsync](/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync), and [Package.CheckUpdateAvailabilityAsync](/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync) are not supported in JavaScript. However, you can create a [Windows Runtime component](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript) that calls these methods and then call this component from a JavaScript UWP app.

Here is an example.

```csharp
namespace CSRuntimeComponent
{
    public sealed class UpdateAvailabilityChecker
    {
        public static IAsyncOperation<PackageUpdateAvailability> CheckForUpdatesAsync()
        {
            return AsyncInfo.Run<PackageUpdateAvailability>((result) => Task.Run<PackageUpdateAvailability>(async () =>
            {
                PackageManager pm = new PackageManager();
                Package currentPackage = pm.FindPackageForUser(string.Empty, Package.Current.Id.FullName);
                PackageUpdateAvailabilityResult apiResult = await currentPackage.CheckUpdateAvailabilityAsync();

                if (apiResult.Availability == PackageUpdateAvailability.Error)
                {
                    Logger.Error($"Error occurred, extended code: {apiResult.ExtendedError}");
                }

                return apiResult.Availability;
            }));
        }
    }
}
```

```javascript
window.onload = function () {
    document.getElementById('mainButton').onclick = function (evt) {
        CSRuntimeComponent.UpdateAvailabilityChecker.checkForUpdatesAsync().done(function (result) {
            document.getElementById("resultLabel").innerHTML = "Update availability result:" + result;
        });
    }
}
```
