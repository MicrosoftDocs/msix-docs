# Build an MSIX from source code using Visual Studio


If you maintain your application by using Visual Studio, and your application doesn't have an installer or your installer doesn't perform too many complicated tasks, consider using Visual Studio instead.

Visual Studio makes it easy to create a package. You'll add a **Windows Application Package Project** to your solution, reference your desktop project, and then press F5 to debug your app. Here's a few other things you can do with it.

:heavy_check_mark: Automatically generate visual assets.

:heavy_check_mark: Make changes to your manifest by using a visual designer.

:heavy_check_mark: Generate your package by using a wizard.

:heavy_check_mark: Easily assign an identity to your application from a name that you've already reserved in [Partner Center](https://partner.microsoft.com/dashboard).

|Topic| Description |
|:---|:---|
|Packaging a Desktop app in Visual Studio](before-packaging-overview.md)| Background on MSIX requirments and packaged Desktop app runtime behaviour. This is useful to know before building an MSIX package for your Desktop application. If you're building a UWP app you can skip this section. | 
|[Packaging your Desktop app in Visual Studio](sign-app-package-using-signtool.md#using-signtool)| This section discusses how to package your Desktop app (e.g. Winforms, WPF, Win32) as an MSIX in Visual Studio.|
|[Packaging your UWP app in Visual Studio](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how to package your UWP app as an MSIX in Visual Studio.|
|[Extending your MSIX application](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how you can to extend your application using extensions and optional packages.|


For instructions, see [Package a desktop application by using Visual Studio](desktop-to-uwp-packaging-dot-net.md). The following table shows the supported versions of Visual Studio, Windows 10, and package formats.

|  Supported versions of Visual Studio | Supported OS versions for creating packages  | Supported OS versions for installed packages  |  Supported package formats  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5 and later       |  Windows 10, version 1607 and later           |  Windows 10, version 1607 and later            |  .msix (for Windows 10, version 1709 and later)<br/>.appx (for Windows 10, version 1607 and later)                 |
