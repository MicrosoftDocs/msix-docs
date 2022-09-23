---
title: Detect package identity and runtime context
description: Describes how an app can determine whether its shipped as an MSIX package on Win 1709 or later. 
author: Huios
ms.date: 01/23/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.custom: "RS5, seodec18"
---

# Detect package identity and runtime context

You may have some versions of your app that were not distributed in an MSIX package. At runtime your app can detect whether it was deployed as an MSIX package by using the Windows Package Manager API, or your own custom installer. You may want to change the app behavior such as update settings or you may want to take advantage of functionality only available to MSIX packages.

To determine whether your application is running as an MSIX package on a version of Windows that supports the full MSIX feature set, you can use the [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) native function in kernel32.dll. When a desktop application is running as a unpackaged application without package identity, this function returns an error which can help you infer the context in which the app is running.

If the function succeeds, it means:

* Your app is packaged in an MSIX package.
* Your app is running on Windows 10, version 1709 (build 16299) or later with full MSIX support.

## Use GetCurrentPackageFullName in native code

The following code example demonstrates how to use [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) to determine the context of an app.

```cpp
#define _UNICODE 1
#define UNICODE 1

#include <Windows.h>
#include <appmodel.h>
#include <malloc.h>
#include <stdio.h>

int __cdecl wmain()
{
    UINT32 length = 0;
    LONG rc = GetCurrentPackageFullName(&length, NULL);
    if (rc != ERROR_INSUFFICIENT_BUFFER)
    {
        if (rc == APPMODEL_ERROR_NO_PACKAGE)
            wprintf(L"Process has no package identity\n");
        else
            wprintf(L"Error %d in GetCurrentPackageFullName\n", rc);
        return 1;
    }

    PWSTR fullName = (PWSTR) malloc(length * sizeof(*fullName));
    if (fullName == NULL)
    {
        wprintf(L"Error allocating memory\n");
        return 2;
    }

    rc = GetCurrentPackageFullName(&length, fullName);
    if (rc != ERROR_SUCCESS)
    {
        wprintf(L"Error %d retrieving PackageFullName\n", rc);
        return 3;
    }
    wprintf(L"%s\n", fullName);

    free(fullName);

    return 0;
}
```

## Use GetCurrentPackageFullName function in managed code

To call [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) in a managed .NET Framework app, you'll need to use [Platform Invoke (P/Invoke)](/dotnet/standard/native-interop/pinvoke) or some other form of interop.

To simplify this process, you can use the [DesktopBridgeHelpers](https://github.com/qmatteoq/DesktopBridgeHelpers/) library. This library supports .NET Framework 4 and later, and it uses P/Invoke internally to provide a helper class that determines whether the app is running on a version of Windows that supports the full MSIX feature set. This library is also available as a [NuGet Package](https://www.nuget.org/packages/DesktopBridge.Helpers/).

After you install the package in your project, you can create a new instance of the `DesktopBridge.Helpers` class and call the `IsRunningAsUwp` method. This method returns true if your app is running as an MSIX package on Windows 10, version 1709 (build 16299) or later and false if either of those are not true. The following sample demonstrates how to call this method.

```csharp
private bool IsRunningAsUwp()
{
   UwpHelpers helpers = new UwpHelpers();
   return helpers.IsRunningAsUwp();
}

private void Form1_Load(object sender, EventArgs e)
{
   if (IsRunningAsUwp())
   {
       txtUwp.Text = "I'm running as MSIX";
   }
   else
   {
       txtUwp.Text = "I'm running as a native desktop app";
   }
}
```
