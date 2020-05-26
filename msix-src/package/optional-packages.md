---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Optional packages and related set authoring
description: Optional packages contain content that can be integrated with a main package. These are useful for downloadable content (DLC) and other scenarios.
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio
ms.localizationpriority: medium
---

# Optional packages and related set authoring

Optional packages contain content that can be integrated with a main package. These are useful for downloadable content (DLC), dividing a large app for size restraints, or for shipping any additional content separate from your original app.

Related sets are an extension of optional packages -- they allow you to enforce a strict set of versions across main and optional packages. They also let you load native code (C++) from optional packages. Related sets can have different publishers from the main app if it is deployed outside of the store.

Optional packages and related sets all run inside the main app's MSIX container.

## Prerequisites

- Visual Studio 2019 or Visual Studio 2017 (version 15.1 or later)
- Windows 10, version 1703 or later
- Windows 10, version 1703 SDK or later

To get all of the latest development tools, see [Downloads and tools for Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> To submit an app that uses optional packages and/or related sets to the Microsoft Store, you will need permission. Optional packages and related sets can be used for Line of Business (LOB) or enterprise apps without Partner Center permission if they are not submitted to the Store. See [Windows developer support](https://developer.microsoft.com/windows/support) to get permission to submit an app that uses optional packages and related sets.

## Code sample

While you're reading this article, it's recommended that you follow along with the [optional package code sample](https://github.com/AppInstaller/OptionalPackageSample) on GitHub for a hands-on understanding of how optional packages and related sets work within Visual Studio.

## Optional packages

To create an optional package in Visual Studio, you'll need to:
1. Make sure your app's **Target Platform Min Version** is set to: 10.0.15063.0 or higher.
2. From your **main package** project, open the `Package.appxmanifest` file. Navigate to the "Packaging" tab and make a note of your **package family name**, which is everything before the "_" character.
3. From your **optional package** project, right click the `Package.appxmanifest` and select **Open with > XML (Text) Editor**.
4. Locate the `<Dependencies>` element in the file. Add the following:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Replace `[MainPackageDependency]` with your **package family name** from Step 2. This will specify that your **optional package** is dependent on your **main package**.

Once you have your package dependencies set up from Steps 1 through 4, you can continue developing as you normally would. If you would like to load code from the optional package into the main package, you will need to build a related set. See the [Related sets](#related_sets) section for more details.

Visual Studio can be configured to re-deploy your main package each time you deploy an optional package. To set the build dependency in Visual Studio, you should:

- Right click the optional package project and select **Build Dependencies > Project Dependencies...**
- Check the main package project and select "OK". 

Now, every time you enter F5 or build an optional package project, Visual Studio will build the main package project first. This will ensure that your main project and optional projects are in sync.

## Related sets<a name="related_sets"></a>

If you want to load code from an optional package into the main package, you will need to build a related set. To build a related set, your main package and optional package must be tightly coupled. The metadata for related sets is specified in the .appxbundle or .msixbundle file of the main package. Visual Studio helps you get the correct metadata in your files. To configure your app's solution for related sets, use the following steps:

1. Right click the main package project, select **Add > New Item...**
2. From the window, search the Installed Templates for ".txt" and add a new text file.
    > [!IMPORTANT]
    > The new text file must be named: `Bundle.Mapping.txt`.
3. In the `Bundle.Mapping.txt` file you'll specify relative paths to any optional package projects or external packages. A sample `Bundle.Mapping.txt` file should look something like this:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

When your solution is configured this way, Visual Studio will create a bundle manifest for the main package with all of the required metadata for related sets. 

Note that like optional packages, a `Bundle.Mapping.txt` file for related sets will only work on Windows 10, version 1703 or higher. Additionally, your app's Target Platform Min Version should be set to 10.0.15063.0 or higher.

## Removing Optional Packages 
Users can go into their **Settings** app and remove the optional packages. Similarly, developers can use the [RemoveOptionalPackageAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.PackageCatalog) to remove a list of optional packages. 

```
 
    PackageCatalog catalog = PackageCatalog.OpenForCurrentPackage();
    List<string> optionalList = new List<string>();
    optionalList.Add("FabrikamAgeAnalysis_kwpnjs8c36mz0");
    
     //Warn user that application will be restarted. 
    var result = await catalog.RemoveOptionalPackagesAsync(optionalList);
    if(result.ExtendedError != null)
    {
        throw removalResult.ExtendedError;
    }
    
```
> [!NOTE]
> In the case of a related set the platform will need to restart the main application to finalize the removal to avoid situations where the app has content that is loaded from the package that is being removed. The apps must notify the users that the application will need to be restarted before the app calls the API.

If the optional package is content only then, the developer should explicitly tell the platform that the package that is about to remove is 'not in use' by the application before the developer removes the optional package. This also allows the developer to remove the package without a restart.

## Known issues<a name="known_issues"></a>

Debugging a related set optional project is not currently supported in Visual Studio. To work around this issue, you can deploy and launch the activation (Ctrl + F5) and manually attach the debugger to a process. To attach the debugger, go the "Debug" menu in Visual Studio, select "Attach to Process...", and attach the debugger to the **main app process**.
