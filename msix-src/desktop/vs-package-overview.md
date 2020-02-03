# Package your app as an MSIX in Visual Studio


Visual studio makes it easy to generate MSIX packages and is the recommended approach for applications in development. Desktop apps (Winfors, WPF, Win32 etc.) can use the Application Packaging Project to generate an MSIX package and UWP apps have MSIX generation built into their default project type. To generate an MSIX package for a desktop application you first need to add the Windows Application Packaging Project to your solution, this step is not required for UWP apps.

|Topic| Description |
|:---|:---|
|[Setup your Desktop app for packaging in Visual Studio](desktop-to-uwp-packaging-dot-net.md)| This section discusses how to setup your Desktop application (e.g. Winforms, WPF, Win32 etc.) for packaging using the Windows Application Packaging Project in Visual Studio. | 
|[Packaging a Desktop or UWP app in Visual Studio](../package/packaging-uwp-apps.md)| This section discusses how to generate MSIX packages for your Desktop or UWP apps in Visual Studio.|
|[Extending your packaged application](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how you can to extend your application using extensions and optional packages.|