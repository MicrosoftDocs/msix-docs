---
title: Using an embedded AppInstaller file to update your app
description: This article describes how to embed a pre-existing AppInstaller file into your Windows application
ms.date: 05/26/2021
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.custom: "RS5, seodec18"
---

# Using an embedded App Installer file to update your app
>[!Important]
>The use of an embedded App Installer file is available in Windows version 10.0.21300.0. To make use of this feature, ensure that the MaxVersionTested is referencing this or a newer version of the Windows operating system.

The App Installer file provides an update path that a Windows app can traverse searching for updates, and repairs.

When you use Visual Studio to build and publish your Windows app with an embedded App Installer file, you must ensure that the Windows 10 SDK 2104 (or newer) has been installed, and the project properties has Windows 10 21H1 (or newer) as the Targetted versions (MaxVersionTested and MinVersion). If this has not been configured, the Windows app will not set the embedded AppInstaller configurations to the device when the Windows app is installed.

## How to - MSIX Packaging Tool
The following steps will guide you through how to use the MSIX Packaging Tool to edit a pre-existing Windows app to include an embedded App Installer app. 

>[!Note]
>The following guidance assumes that you've previously created the App Installer file using Visual Studio to automate the creation of an App Installer file, with use of the MSIX Toolkit, or manually. For guidance on creating an App Installer file, please visit one of the following Docs Articles:
>- [How to create App Installer file with Visual Studio](create-appinstallerfile-vs.md)
>- [How to create App Installer file manually](how-to-create-appinstaller-file.md)

### Open the Windows app for Editing
The following steps will guide you through how to use the Microsoft MSIX Packaging Tool App to begin editing a Windows app.

1. Launch the Microsoft MSIX Packaging Tool (Available in the Microsoft Store: [MSIX Packaging Tool](ms-windows-store://pdp/?ProductId=9N5LW3JBCXKF)).
1. Select the **Package editor** button to edit an existing package.
1. Select the **Browse** button, and in the prompted window locate your Windows app and select the **Open** button.
1. Select the **Open Package** button.


### Import the App Installer file into the Windows app
The following steps will guide you through how to embed an App Installer file into a pre-existing Windows app using the Microsoft MSIX Packaging Tool App. These steps assume you've already opened your Windows app for editing using the Microsoft MSIX Packaging Tool app.

1. On the left side of the **MSIX Packaging Tool**, select **Package files**.
1. Expand the **Package** entry inside of the tree view.
1. Right-click on **Package** and select **Add file** from the drop-down menu.
1. Select the **Browse** button inside of the prompted window, navigate to and select the App Installer file, and select the **Open** button.
1. Select the **Save** button.

### Update the AppxManifest
The following steps will guide you through updating the AppxManifest to point to the App Installer file previously added to the Windows app.

1. On the Left side of the **MSIX Packaging Tool**, select **Package information**.
1. Scroll to the bottom of the **Package information** section.
1. Select the **Open file** button to open the AppxManifest in a Notepad window.
1. Ensure the `<Package>` properties include the following Namespaces and Ignorable Namespaces:
    ```xml
    <Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
        xmlns:uap13="http://schemas.microsoft.com/appx/manifest/uap/windows10/13" 
        xmlns:build="http://schemas.microsoft.com/developer/appx/2015/build"
        IgnorableNamespaces="uap mp uap13 build">
    ```
1. Inside of the `<Properties>` include the following `<uap13:AutoUpdate>` and child parameters:
    ```xml
    <Properties>
        ...
        <uap13:AutoUpdate>
            <uap13:AppInstaller File="Update.appinstaller" />
        </uap13:AutoUpdate>
    </Properties>
    ```
1. Save the changes you made to the AppxManifest.
1. Close the **AppxManifest** Notepad window, and return to the **MSIX Packaging Tool**.

>[!Note]
>The above instructions assume that the App Installer file name is "Update.appinstaller".

### Close and Package the Windows app
The following steps will guide you through packaging your Windows app as a newer version. These steps assume you have configured your *Signing Preferences* to meet your organization or client requirements.

1. In the **MSIX Packaging Tool** select the **Save** button.
1. In the prompted window, select the **Yes, Increment** button.
1. Navigate to where you wish to save your newly updated Windows app to, and select the **Save** button.
1. Select the **Close** button.
1. Close the **MSIX Packaging Tool** window.


## How to - Visual Studio
Before you begin, ensure you are working on a Windows 10 device with the Windows 10 SDK 2104 or higher installed. This SDK is required to ensure that the Target Version, and Minimum Version properties are set with the proper values as you build your app.

### Embed the App Installer File
The following steps will guide you through embedding your App Installer file into your Windows app (UWP) Visual Studio project.

1. In your **Visual Studio project**, **Solution Explorer** right-click on your Windows app name.
1. Select **Add** >> **Existing Item** from the drop-down menu.
1. Navigate to your App Installer file, select it and select the **Add** button.
1. In the Solution Explorer, double-click on **Update.appinstaller** to open the file for review.
1. Confirm the App Installer file is correct, and close the file.

### Updating the AppxManifest
The following steps will provide guidance on how to update the AppxManifest in your Visual Studio project to target the newly embedded App Installer file.

This guide assumes:
- The Windows 10 SDK 2104 or higher is installed
- The project properties are set to target Windows 10, version 2104 or higher.
- The name of the App Installer file is *Update.appinstaller*

1. In your **Visual Studio project**, select **Build** from the top menu.
1. Select **Build Solution** from the drop-down menu. Make sure the Windows app build is successful.
1. Select **Local Machine** from the ribbon, to test the functionality of the Windows app.
1. Close the Windows app shortly after it launches, and stop debugging.
1. In the Solution Explorer, right-click on **Package.appxmanifest** 
1. Select **View Code** from the drop-down menu.
1. Ensure the `<Package>` properties include the following Namespaces and Ignorable Namespaces:
    ```xml
    <Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
        xmlns:uap13="http://schemas.microsoft.com/appx/manifest/uap/windows10/13" 
        xmlns:build="http://schemas.microsoft.com/developer/appx/2015/build"
        IgnorableNamespaces="uap mp uap13 build">
    ```
1. Inside of the `<Properties>` include the following `<uap13:AutoUpdate>` and child parameters:
    ```xml
    <Properties>
        ...
        <uap13:AutoUpdate>
            <uap13:AppInstaller File="Update.appinstaller" />
        </uap13:AutoUpdate>
    </Properties>
    ```
1. Save your changes to the file, and close.

### Building your Windows app
The following steps will guide you through creating the Windows app package for installating on supported operating systems.

1. In your **Visual Studio project**, right-click on the Windows app name.
1. Select **Publish** >> **Create App Packages...** from the drop-down menu.
1. Select the **Sideloading** radio button, in the new *Create App Packages* dialog window.
1. Select the **Next** button.
1. Select the **Yes, use the current certificate:** radio button.
1. Import an existing Certificate, or auto-generate a certificate to sign your Windows app.
1. Select the **Next** button.
1. Specify the Solution Configuration, version, and optional build of a Windows app Bundle for your Windows app.
1. Select the **Create** button.
