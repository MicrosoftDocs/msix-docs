---
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop application.
title: Package a desktop app from source code using Visual Studio
ms.date: 07/18/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
---

# Package a desktop app from source code using Visual Studio

You can use the **Windows Application Packaging Project** project in Visual Studio to generate a package for your desktop app. Then, you can publish that package to the Microsoft Store or sideload it onto one or more PCs.

The **Windows Application Packaging Project** project is available in the following versions of Visual Studio. For the best experience, we recommend that you use the latest release.

* Visual Studio 2019
* Visual Studio 2017 15.5 and later

> [!IMPORTANT]
> The **Windows Application Packaging Project** project in Visual Studio is supported on Windows 10, version 1607, and later. It can only be used in projects that target Windows 10 Anniversary Update (10.0; Build 14393) or a later release.

## Prepare your application

Review this guide before you begin creating a package for your application: [Prepare to package a desktop application](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## Create a package

1. In Visual Studio, open the solution that contains your desktop application project.

2. Add a **Windows Application Packaging Project** project to your solution.

   You won't have to add any code to it. It's just there to generate a package for you. We'll refer to this project as the "packaging project".

   ![Packaging project](images/packaging-project.png)

3. Set the **Target Version** of this project to any version that you want, but make sure to set the **Minimum Version** to **Windows 10 Anniversary Update**.

   ![Packaging version selector dialog box](images/packaging-version.png)

4. In Solution Explorer, right-click the **Applications** folder under the packaging project and choose **Add Reference**.

   ![Add Project Reference](images/add-project-reference.png)

5. Choose your desktop application project, and then choose the **OK** button.

   ![Desktop project](images/reference-project.png)

   You can include multiple desktop applications in your package, but only one of them can start when users choose your app tile. In the **Applications** node, right-click the application that you want users to start when they choose the app's tile, and then choose **Set as Entry Point**.

   ![Set entry point](images/entry-point-set.png)

6. If the application you are packaging targets .NET Core 3, follow these steps to add a new build target to the project file. This is only necessary for applications that target .NET Core 3.  

    1. In Solution Explorer, right-click the packaging project node and select **Edit Project File**.

    2. Add the following XML to the project file, immediately before the closing `</Project>` element.

        ``` xml
        <!-- Stomp the path to application executable. This task will copy the main exe to the appx root folder. -->
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
          <ItemGroup>
            <!-- Stomp all "SourceProject" values for all incoming dependencies to flatten the package. -->
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
              <!-- Blank the SourceProject here to vend all files into the root of the package. -->
              <SourceProject></SourceProject>
            </_FilteredNonWapProjProjectOutput>
          </ItemGroup>
        </Target>
        ```

    3. Save the project file and close it.

7. Build the packaging project to ensure that no errors appear. If you receive errors, open **Configuration Manager** and ensure that your projects target the same platform.

   ![Config manager](images/config-manager.png)

8. Use the [Create App Packages](../package/packaging-uwp-apps.md) wizard to generate an .msixupload/.appxupload file.

   You can upload that file directly to the Store.

**Video**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## Next steps

**Find answers to your questions**

Have questions? Ask us on Stack Overflow. Our team monitors these [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). You can also ask us [here](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Give feedback or make feature suggestions**

See [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Run, debug or test your desktop application**

See [Run, debug, and test a packaged desktop application](desktop-to-uwp-debug.md)

**Enhance your desktop application by adding UWP APIs**

See [Enhance your desktop application for Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Extend your desktop application by adding UWP projects and Windows Runtime Components**

See [Extend your desktop application with modern UWP components](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

**Distribute your app**

See [Distribute a packaged desktop application](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
