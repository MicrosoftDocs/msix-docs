---
title: Detecting Package Identity and Runtime Context
description: Describes how an app can determine whether its shipped as an MSIX package on Win 1709 or later. 
ms.date: 01/23/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Detecting Package Identity and Runtime Context

Even after moving your desktop app to MSIX you may still have versions of the same application that are not distributed as MSIX. At runtime your app can detect whether it was deployed as an MSIX using the Windows Package Manager, or your own custom installer. You may want to change the app behavior such as update settings or you may want to take advantage of functionality only available to MSIX packages.

To determine whether your application is running as an MSIX package on a Windows version with the full MSIX feature set, you can use the [GetCurrentPackageFullName( )]( https://msdn.microsoft.com/en-us/library/windows/desktop/hh446599(v=vs.85).aspx) native Windows API included in the kernel32.dll System library. When a desktop application is running as a non-packaged application without package identity, the API will return an error which can help you infer the context in which the app is running. 

If the API call succeeds, it means:
1.	Your desktop app is packages as an MSIX
2.	Your app is running on Windows 10, version 1709 (build 16299) or later with full MSIX support

## Using GetCurrentPackageFullName( ) in native code

Below is some sample C++ code using GetCurrentPackageFullName( ) to determine the context of an app:
```
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
## Using GetCurrentPackageFullName( ) in managed code

If you are working on a C# application based on the .NET framework (the API isn’t directly exposed by the .NET framework, it’s a native C++ method offered directly by System. To take advantage of the API in managed code you need to make use of interop, similar to using the P/Invoke feature or C++ / CLI libraries. 
To make development easier you can use a .NET library (works with .NET 4+ applications), which leverages:
- The P/Invoke approach to invoke the GetPackageFullName() native method
- The native .NET Framework APIs to check if the app is running on an operating system where this API isn’t supported e.g. Windows 7.

The library is open source and available on [Github]( https://github.com/qmatteoq/DesktopBridgeHelpers/). It’s also available as a [NuGet Package](https://www.nuget.org/packages/DesktopBridge.Helpers/).
Once installed in your .NET project you can create a new instance of the DesktopBridge.Helpers class and call the IsRunningAsUwp() method. It will return true if your desktop application is running as an MSIX package on Windows 10, version 1709 (build 16299) or later and false if either of those are not true. Below is sample C# code making use of the NuGet package: 

```
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
