# Package your app as an MSIX using Visual Studio


Visual studio makes it easy to generate MSIX packages and is the recommended approach for applications in development. Desktop apps (Winfors, WPF, Win32 etc.) can use the Application Packaging Project to generate an MSIX package. UWP appshave MSIX generation built into their default project type. 

Here are a few other things you can do from Visual Studio:

:heavy_check_mark: Automatically generate visual assets.

:heavy_check_mark: Make changes to your manifest by using a visual designer.

:heavy_check_mark: Generate your package by using a wizard.

:heavy_check_mark: Easily assign an identity to your application from a name that you've already reserved in [Partner Center](https://partner.microsoft.com/dashboard).

|Topic| Description |
|:---|:---|
|[Packaging a Desktop app in Visual Studio](before-packaging-overview.md)| Background on MSIX requirments and packaged Desktop app runtime behaviour. This is useful to know before building an MSIX package for your Desktop application. If you're building a UWP app you can skip this section. | 
|[Packaging your Desktop app in Visual Studio](sign-app-package-using-signtool.md#using-signtool)| This section discusses how to package your Desktop app (e.g. Winforms, WPF, Win32) as an MSIX in Visual Studio.|
|[Packaging your UWP app in Visual Studio](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how to package your UWP app as an MSIX in Visual Studio.|
|[Extending your MSIX application](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how you can to extend your application using extensions and optional packages.|
