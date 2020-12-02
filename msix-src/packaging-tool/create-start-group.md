---
title: Group applications under a folder in the Start menu
description: Describes how to enable multiple packaged MSIX application to be grouped under a single folder in the Start menu
ms.date: 12/02/2020
ms.topic: article
keywords: windows 10, uwp, msix, appx
ms.localizationpriority: medium
---

# Group applications under a folder in the Start menu

> [!IMPORTANT]
> This feature is currently available in preliminary Windows 10 builds which are distributed through the Dev Ring of the [Windows Insider program](https://insider.windows.com/en/). It might be included in a future official Windows 10 release.

The manifest of a MSIX packaged application contains one or more `<Application>` entries, which are the available entry points. Each of them will become an icon in the Start menu.

A MSIX package can contain multiple applications. Alternatively, a company can build multiple applications, which are packaged as separate MSIX packages, but they all belong to the same suite.
In both scenarios, you may want to group together all the entries in the Start menu under a single folder, so that for the user it's easier to find all the applications in the same place.

This goal can be achieved using the `VisualGroup` property of the `VisualElements` item.
Here are the steps to implement this change:

1) Open the manifest file of your application with a text editor of choice. Alternativaly, if you're using the MSIX Packaging Tool, you can press the *Open manifest* button in the Package Editor.
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