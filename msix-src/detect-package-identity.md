---
title: Detect package identity and runtime context
description: Determine whether a process has package identity by using Task Manager, the Win32 app model API, or managed code.
author: Huios
ms.date: 09/30/2026
ms.topic: how-to
keywords: windows 10, uwp, app package, app update, msix, appx
ms.custom: "RS5, seodec18"
---

# Detect package identity and runtime context

A process is **packaged** when it has package identity. A process is **unpackaged** when it doesn't have package identity. This distinction applies to processes rather than only to applications because background tasks, out-of-process servers, and other processes can also have package identity.

Package identity can come from a package that contains the application files or from a [package with external location](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps). Therefore, detecting package identity doesn't tell you where the application files are stored or which packaging model the app uses.

An app might check its runtime identity before it uses a feature that requires package identity, selects an update mechanism, or chooses a storage location.

## Check a process in Task Manager

You can inspect a running process without changing the app:

1. Open Task Manager and select **Details**.
1. Right-click a column heading, and then select **Select columns**.
1. Select **Package name**, and then select **OK**.

The **Package name** field is blank for an unpackaged process. For a packaged process, it normally contains the package's localized display name. Display names aren't guaranteed to be unique. If Task Manager can't resolve the localized resource, it might display an `ms-resource:` reference instead.

## Use GetCurrentPackageFullName in native code

Call [GetCurrentPackageFullName](/windows/win32/api/appmodel/nf-appmodel-getcurrentpackagefullname) without a buffer:

- `ERROR_INSUFFICIENT_BUFFER` means the process has package identity.
- `APPMODEL_ERROR_NO_PACKAGE` means the process doesn't have package identity.
- Any other value is an error that your app should handle.

```cpp
#include <Windows.h>
#include <appmodel.h>

LONG IsPackagedProcess(bool* isPackaged)
{
    if (isPackaged == nullptr)
    {
        return ERROR_INVALID_PARAMETER;
    }

    *isPackaged = false;
    UINT32 length = 0;
    const LONG result = GetCurrentPackageFullName(&length, nullptr);

    if (result == ERROR_INSUFFICIENT_BUFFER)
    {
        *isPackaged = true;
        return ERROR_SUCCESS;
    }

    if (result == APPMODEL_ERROR_NO_PACKAGE)
    {
        return ERROR_SUCCESS;
    }

    return result;
}
```

This check doesn't need to retrieve the package full name. If you also need the identity value, allocate a buffer of the returned length and call `GetCurrentPackageFullName` again.

## Use GetCurrentPackageFullName function in managed code

Use platform invocation to call `GetCurrentPackageFullName` from .NET:

```csharp
using System;
using System.ComponentModel;
using System.Runtime.InteropServices;

internal static class PackageIdentity
{
    private const int ErrorInsufficientBuffer = 122;
    private const int AppModelErrorNoPackage = 15700;

    [DllImport("kernel32.dll", CharSet = CharSet.Unicode, ExactSpelling = true)]
    private static extern int GetCurrentPackageFullName(
        ref int packageFullNameLength,
        IntPtr packageFullName);

    public static bool IsPackagedProcess()
    {
        var length = 0;
        var result = GetCurrentPackageFullName(ref length, IntPtr.Zero);

        return result switch
        {
            ErrorInsufficientBuffer => true,
            AppModelErrorNoPackage => false,
            _ => throw new Win32Exception(result),
        };
    }
}
```

Don't treat every nonzero return value as meaning that the process is unpackaged. Values other than `ERROR_INSUFFICIENT_BUFFER` and `APPMODEL_ERROR_NO_PACKAGE` represent errors.

## Related content

- [Package identity overview](/windows/apps/desktop/modernize/package-identity-overview)
- [Features that require package identity](/windows/apps/desktop/modernize/modernize-packaged-apps)
- [Packaging overview](/windows/apps/package-and-deploy/packaging/)
- [Is it MSIX? How to tell if a process is packaged](https://devblogs.microsoft.com/insidemsix/how-to-tell-if-a-process-is-packaged/)
