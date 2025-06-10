---
title: Resource packages
description: This article describes resource packages and how developers can use them in their code.
ms.date: 02/06/2020
author: andreww-msft
ms.topic: article
ms.author: andreww
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio
---

# Resource Packages 
Resource packages offers a great way to reduce users disk footprint by segmenting language or scale specific asset into separate packages that are downloaded automatically by Windows depending on the users machine configuration. If the user adds a new language to their list of OS languages in **Region & language** settings or changes the display configuration, across an automatic store update, the OS will fetch applicable resource packages for all installed apps on the device.

In Windows SDK 10.0.17095.0 [AddResourcePackageAsync API](/uwp/api/Windows.ApplicationModel.PackageCatalog) allows developers to install a resource package for an app on demand. 


## How to use the AddResourcePackageAsync API 
- AddResourcePackageAsync takes the **PackageFamilyName** of the application and the resource ID that uniquely identifies the resource package that is trying to download. The resource ID corresponds to the **ResourceId** element in the **AppxManifest.xml** of the resource package.

- The application must restart to get a merged view of all resources that are available to the application. The API offers the **ForceTargetApplicationShutdown** option that can pass to restart the application.

- If there is an update available for the application when the resource package is being downloaded, the API will fail as the older version of the app is no longer available. By passing in the **ApplyUpdateIfAvailable** flag, the API will update the app as well as get the requested resource package as part of the same download. 

- The API returns a progress for the download that can display in the application. Also note that for apps published to the store,  the resource package acquisition shows up in the Microsoft Store **downloads and updates** page and shows up as an update to your application. The end user can choose to pause or cancel the download of the resource package from the Microsoft Store. 

Example 
```
 private async void DownloadGermanResourcePackage(object sender, RoutedEventArgs e)
{            
    // ResourceId that is unique to the resource package
    string resourceId = "split.language-de";
    // Warn user that application will need to restart.
    // To take update if available pass in the ApplyUpdateIfAvailable flag
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    PackageCatalogAddResourcePackageResult result = await packageCatalog.AddResourcePackageAsync("29270depappf.CaffeMacchiato_gah1vdar1nn7a", resourceId, 
        AddResourcePackageOptions.ApplyUpdateIfAvailable | AddResourcePackageOptions.ForceTargetApplicationShutdown)
        .AsTask<PackageCatalogAddResourcePackageResult, PackageInstallProgress>(new Progress
        (progress =>;
        {
                // Draw progress
        }));

        if (result.ExtendedError != null)
        {
                //Display error or retry if needed
        }
}
```
## Validation
 For local validation, developers can create an .msixbundle or .appxbundle and install from your local drive, network share or webserver. When the app calls the AddResourcePackageAsync API, Windows will  acquire the resource package from the location where the original application was installed. This makes the validation simple. Once this works, the app is ready to be deployed. 

## Removing resource packages 
The app can choose to remove to remove the resource package(s) it downloaded via the [RemoveResourcePackageAsync API](/uwp/api/Windows.ApplicationModel.PackageCatalog). However, Resource packages that are  applicable for the user or the device as a whole cannot be uninstalled. 

Example 
```
 private async void RemoveResourcePackage(object sender, RoutedEventArgs e)
{            
    var packageCatalog = PackageCatalog.OpenForCurrentPackage();
    List resourcePackagesToRemove = new List();
    resourcePackagesToRemove.Add("split.language-de");
    //Warn user that application will be restarted
    var removePackageResult = await packageCatalog.RemoveResourcePackagesAsync(resourcePackagesToRemove);
    if (removePackageResult.ExtendedError != null)
    {
        // display error
    }
}
```
