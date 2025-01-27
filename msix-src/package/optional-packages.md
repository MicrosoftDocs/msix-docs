---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Optional packages and related set authoring
description: Optional packages contain content that can be integrated with a main package. These are useful for downloadable content (DLC) and other scenarios.
ms.date: 07/02/2019
ms.topic: article
author: andreww-msft
ms.author: andreww
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio
---

# Optional packages and related set authoring

Optional packages contain content that can be integrated with a main package. These are useful for downloadable content (DLC), dividing a large app for size restraints, or for shipping any additional content separate from your original app. For more information about optional packages, see [Blog post: Extend your application using optional packages](/archive/blogs/appinstaller/uwpoptionalpackages).

Related sets are an extension of optional packages. Related sets allow you to enforce a strict set of versions across main and optional packages. Related sets can have different publishers from the main app if it is deployed outside of the Store. For more information about related sets, see [Blog post: Tooling to create a related set](/archive/blogs/appinstaller/tooling-to-create-a-related-set).

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
4. Locate the `<Dependencies>` element in the file. Add the following, and replace `[MainPackageDependency]` with your **package family name** from Step 2. This will specify that your **optional package** is dependent on your **main package**.
    ```XML
    <uap3:MainPackageDependency Name="[MainPackageDependency]"/>
    ```

> [!NOTE]
> If you would like to create an optional package from a different publisher, you will need to specify the publisher of the main application if they are different. Like so <uap4:MainPackageDependency Name="Main_app" Publisher="CN=Contoso..." />. This will not work if you are publishing to the Store.

After you have your package dependencies set up from Steps 1 through 4, you can continue developing as you normally would. For more information, see [Blog post: Build your first optional package](/archive/blogs/appinstaller/build-your-first-optional-package).

Visual Studio can be configured to re-deploy your main package each time you deploy an optional package. To set the build dependency in Visual Studio, you should:

1. Right click the optional package project and select **Build Dependencies > Project Dependencies...**
2. Check the main package project and select "OK". 

Now, every time you enter F5 or build an optional package project, Visual Studio will build the main package project first. This will ensure that your main project and optional projects are in sync.

## Related sets

A related set consists of a main package and an optional package that are tightly coupled via metadata that is specified in the .appxbundle or .msixbundle file of the main package. This metadata links the main package to the optional package (using the name of the .appxbundle file + version), and the optional package to the main package (using the version independent name). Visual Studio helps you get the correct metadata in your files. 

The versioning of packages in a related set is synchronized in a way that won't allow the latest version of any package to be used until all of the related set packages (specified by version in the main package) are installed. Packages are serviced independently, but packages specified in the set may not be used until all of them have been updated. For more information about related sets, see [Blog post: Tooling to create a related set](/archive/blogs/appinstaller/tooling-to-create-a-related-set).

To configure your app's solution for related sets, use the following steps:

1. Right click the main package project, select **Add > New Item...**
2. From the window, search the Installed Templates for ".txt" and add a new text file.
    > [!IMPORTANT]
    > The new text file must be named: `Bundle.Mapping.txt`.
3. In the `Bundle.Mapping.txt` file enter the string "[OptionalProjects]" followed by the relative paths to your optional package projects. Here is an example `Bundle.Mapping.txt` file:
    ```syntax
    [OptionalProjects]
    "..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
    "..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"
    ```

When your solution is configured this way, Visual Studio will create a [bundle manifest](/uwp/schemas/bundlemanifestschema/bundle-manifest) named AppxBundleManifest.xml for the main package with all of the required metadata for related sets. 

Note that like optional packages, a `Bundle.Mapping.txt` file for related sets will only work on Windows 10, version 1703 or higher. Additionally, your app's Target Platform Min Version should be set to 10.0.15063.0 or higher.

## Removing Optional Packages

Users can go into their **Settings** app and remove the optional packages. Similarly, developers can use the [RemoveOptionalPackageAsync](/uwp/api/Windows.ApplicationModel.PackageCatalog) to remove a list of optional packages. 

```csharp
PackageCatalog catalog = PackageCatalog.OpenForCurrentPackage();
List<string> optionalList = new List<string>();
optionalList.Add("FabrikamAgeAnalysis_kwpnjs8c36mz0");
    
// Warn user that application will be restarted. 
var result = await catalog.RemoveOptionalPackagesAsync(optionalList);
if (result.ExtendedError != null)
{
    throw removalResult.ExtendedError;
}
```
> [!NOTE]
> In the case of a related set the platform will need to restart the main application to finalize the removal to avoid situations where the app has content that is loaded from the package that is being removed. The apps must notify the users that the application will need to be restarted before the app calls the API.

If the optional package is content only then, the developer should explicitly tell the platform that the package that is about to remove is 'not in use' by the application before the developer removes the optional package. This also allows the developer to remove the package without a restart.

## Known issues

Debugging a related set optional project is not currently supported in Visual Studio. To work around this issue, you can deploy and launch the activation (Ctrl + F5) and manually attach the debugger to a process. To attach the debugger, go the "Debug" menu in Visual Studio, select "Attach to Process...", and attach the debugger to the **main app process**.

## Related articles

* [Blog post: Extend your application using optional packages](/archive/blogs/appinstaller/uwpoptionalpackages)
* [Blog post: Build your first optional package](/archive/blogs/appinstaller/build-your-first-optional-package)
* [Blog post: Loading code from an optional package](/archive/blogs/appinstaller/loading-code-from-an-optional-package)
* [Blog post: Tooling to create a related set](/archive/blogs/appinstaller/tooling-to-create-a-related-set)
