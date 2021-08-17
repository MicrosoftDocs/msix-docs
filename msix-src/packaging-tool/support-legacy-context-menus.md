---
title: Support legacy context menus
description: Describes how to support legacy context menus for MSIX apps.
ms.date: 8/16/2021
ms.topic: article
keywords: MSIX, IContextMenu, DropHandler, win32 shell extensions, MPT, MSIX Packaging Tool
ms.localizationpriority: medium
---

# Support legacy context menus for packaged apps

If your desktop app implements the legacy IContextMenu interface for shell extensions such as the context menu handler or drag and drop handler, the shell extension might not work after you package your app. In order for shell to recognize and register the extension, you will need to modify the package manifest file.
(This feature is available on Windows 11 build 22000+)

- Use the desktop9 namespace

    `xmlns:desktop9="http://schemas.microsoft.com/appx/manifest/desktop/windows10/9"`

- Add com namespace and [windows.comServer extension](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver) for your shellex dll
    
    `xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"`

    Below is an example code snippet:
    ```
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer DisplayName="<display-name-for-the-com-server>">
                <com:Class Id="<GUID-for-the-com-server>" Path="<path-to-the-com-server-or-dll>" ThreadingModel="STA" />
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

- Add windows.fileExplorerClassicContextMenuHandler or windows.fileExplorerClassicDragDropContextMenuHandler extension

    Below is an example code snippet:
    ```
    <desktop9:Extension Category="windows.fileExplorerClassicContextMenuHandler">
        <desktop9:FileExplorerClassicContextMenuHandler>
            <desktop9:ExtensionHandler Type="*" Clsid="<GUID-for-the-com-server>" />
            <desktop9:ExtensionHandler Type=".txt" Clsid="<GUID-for-the-com-server>" />
            <desktop9:ExtensionHandler Type="Directory" Clsid="<GUID-for-the-com-server>" />
        </desktop9:FileExplorerClassicContextMenuHandler>
    </desktop9:Extension>
    
    <desktop9:Extension Category="windows.fileExplorerClassicDragDropContextMenuHandler">
        <desktop9:FileExplorerClassicDragDropContextMenuHandler>
            <desktop9:ExtensionHandler Type="Directory" Clsid="<GUID-for-the-com-server>" />
            <desktop9:ExtensionHandler Type="Drive" Clsid="<GUID-for-the-com-server>" />
        </desktop9:FileExplorerClassicDragDropContextMenuHandler>
    </desktop9:Extension>
    ```

- Change MaxVersionTested to be greater than 10.0.21300.0
  
    Below is an example code snippet:
    ```
    <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17763.0" MaxVersionTested="10.0.21301.0" />
    </Dependencies>
    ```


Note: IExplorerCommand interface