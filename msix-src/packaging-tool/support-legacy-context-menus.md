---
title: Support legacy context menus
description: Describes how to support legacy context menus for MSIX apps.
ms.date: 8/16/2021
ms.topic: how-to
keywords: MSIX, IContextMenu, DropHandler, win32 shell extensions, MPT, MSIX Packaging Tool
---

# Support legacy context menus for packaged apps

The context menu is one of the most popular and useful shell extensions. If you are already in File Explorer or on the Desktop, it significantly reduces the number of steps to complete a file operation compared to opening a separate app.

If your desktop app implements the legacy IContextMenu interface for shell extensions such as the context menu handler or drag and drop handler, the shell extension might not work after you package your app. In order for shell to recognize and register the extension, you will need to modify the package manifest file.
(This feature is available on Windows 11 build 22000+, which is currently available via [Windows Insider](https://insider.windows.com/) builds)

- Add com namespace and [windows.comServer extension](/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver) for your shellex dll
    
    `xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"`

    Below is an example code snippet:
    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer DisplayName="<display-name-for-the-com-server>">
                <com:Class Id="<GUID-for-the-com-server>" Path="<path-to-the-com-server-or-dll>" ThreadingModel="STA" />
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

- Add desktop9 namespace and the windows.fileExplorerClassicContextMenuHandler or windows.fileExplorerClassicDragDropContextMenuHandler extension

    `xmlns:desktop9="http://schemas.microsoft.com/appx/manifest/desktop/windows10/9"`

    Below is an example code snippet:
    ```xml
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
    ```xml
    <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17763.0" MaxVersionTested="10.0.21301.0" />
    </Dependencies>
    ```

> [!NOTE]
> If you are implementing shell extensions instead of packaging an existing desktop app with legacy [IContextMenu](/windows/win32/api/shobjidl_core/nn-shobjidl_core-icontextmenu) implementation, we suggest implementing the [IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) interface and using [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) instead. Refer [here](/windows/win32/shell/shortcut-choose-method) for more information. 
