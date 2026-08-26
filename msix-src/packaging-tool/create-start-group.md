---
title: Configure MSIX application entries in the Start menu
description: Learn how to hide an MSIX application entry point or group multiple entries under a folder in the Start menu.
ms.date: 08/26/2026
ms.topic: how-to
keywords: windows 10, windows 11, uwp, msix, appx
---

# Configure MSIX application entries in the Start menu

An MSIX package can contain one or more applications. By default, each `<Application>` entry point in the package manifest appears in the **All** list in the Start menu. You can hide entry points that users don't launch directly or, on Windows 11, group visible entries under a folder.

## Hide an application entry point

Set the [`AppListEntry`](https://learn.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-visualelements#attributes) attribute of `uap:VisualElements` to `none` when an entry point doesn't need its own Start menu entry. For example, use this setting for a command-line tool launched through an app execution alias, an app extension, or a helper application activated by another application.

```xml
<Application
    Id="CommandLineTool"
    Executable="tool.exe"
    EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements
        DisplayName="Command-line tool"
        Description="Command-line tool"
        BackgroundColor="transparent"
        Square150x150Logo="Assets\Square150x150Logo.png"
        Square44x44Logo="Assets\Square44x44Logo.png"
        AppListEntry="none" />
</Application>
```

The `none` value suppresses the entry point from the Start menu. It doesn't remove the application from the package or disable other activation mechanisms declared for that application. If users should launch the application directly from the Start menu, omit `AppListEntry` or set it to `default`.

## Group application entries under a folder

Start menu folders for MSIX application entries are available on Windows 11.

You might want to group visible entries when a package contains multiple applications. You can also group applications from separate MSIX packages that belong to the same suite.

This goal can be achieved using the `VisualGroup` property of the `VisualElements` item.
Here are the steps to implement this change:

1) Open the manifest file of your application with a text editor of choice. Alternatively, if you're using the MSIX Packaging Tool, you can press the *Open manifest* button in the Package Editor.
2) Make sure that the `uap3` namespace is declared in the `<Package>` node of the manifest:

    ```xml
    <Package ...
         xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"  
         IgnorableNamespaces="... uap3">
        ...
   </Package>
    ```

3) Locate the `Applications` section. Inside you will find one or more `Application` entries, one for every icon which will be created in the Start menu. This is how it will look like:

    ```xml
      <Applications>
          <Application>
              <VisualElements DisplayName="App1" 
                              Square150x150Logo="images/150x150.png"
                              Square44x44Logo="images/44x44.png"
                              Description="App1"
                              BackgroundColor="#777777"
                              AppListEntry="default">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

4) Add the `uap3` prefix to the `VisualElements` section. Remember to add it both to the opening and ending tags:

    ```xml
      <Applications>
          <Application>
              <uap3:VisualElements DisplayName="App1"
                                   Square150x150Logo="images/150x150.png"
                                   Square44x44Logo="images/44x44.png"
                                   Description="App1"
                                   BackgroundColor="#777777"
                                   AppListEntry="default">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </uap3:VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

5) Finally, add the `VisualGroup` attribute to the `VisualElements` item. As value, set the name you want to give to the folder that will be created in the Start menu.

    ```xml
      <Applications>
          <Application>
              <uap3:VisualElements DisplayName="App1"
                                   Square150x150Logo="images/150x150.png"
                                   Square44x44Logo="images/44x44.png"
                                   Description="App1"
                                   BackgroundColor="#777777"
                                   AppListEntry="default"
                                   VisualGroup="MyFolder">  
                  <uap:SplashScreen BackgroundColor="#777777"
                                    Image="images/splash.png"/>  
              </uap3:VisualElements>  
          </Application>
          <Application>
              ...
          </Application>
      </Applications>
    ```

Now you can repeat the process for all the other `<Application>` entries that you want to include in the same folder. Optionally, you can do the same also with other applications, by simply editing the manifest file included in their MSIX package in the same way and using the same value for the `VisualGroup` attribute.
