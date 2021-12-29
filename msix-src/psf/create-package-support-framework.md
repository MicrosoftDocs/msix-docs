---
description: Fix issues that prevent your desktop application from running in an MSIX container
title: Use the Package Support Framework to create a new runtime fix
ms.date: 05/13/2020
ms.topic: article
keywords: windows 10, uwp
---

# Create a Package Support Framework fixup 

If there is no runtime fix for your issue, you can create a new runtime fix by writing replacement functions and including any configuration data that makes sense. Let's look at each part.

### Replacement functions

First, identify which function calls fail when your application runs in an MSIX container. Then, you can create replacement functions that you'd like the runtime manager to call instead. This gives you an opportunity to replace the implementation of a function with behavior that conforms to the rules of the modern runtime environment.

Declare the ``FIXUP_DEFINE_EXPORTS`` macro and then add a include statement for the `fixup_framework.h` at the top of each .CPP file where you intend to add the functions of your runtime fix.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Make sure that the `FIXUP_DEFINE_EXPORTS` macro appears before the include statement.

Create a function that has the same signature of the function who's behavior you want to modify. Here's an example function that replaces the `MessageBoxW` function.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

The call to `DECLARE_FIXUP` maps the `MessageBoxW` function to your new replacement function. When your application attempts to call the `MessageBoxW` function, it will call the replacement function instead.

#### Protect against recursive calls to functions in runtime fixes

The `reentrancy_guard` type can be added to your functions to protect them against recursive function calls.

For example, you might produce a replacement function for the `CreateFile` function. Your implementation might call the `CopyFile` function, but the implementation of the `CopyFile` function might call the `CreateFile` function. This may lead to an infinite recursive cycle of calls to the `CreateFile` function.

For more information on `reentrancy_guard` see [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### Configuration data

If you want to add configuration data to your runtime fix, consider adding it to the ``config.json``. That way, you can use the `FixupQueryCurrentDllConfig` to easily parse that data. This example parses a boolean and string value from that configuration file.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

### Fixup metadata

Each fixup and the PSF Launcher application has an XML metadata file that contains the following information:

* Version: The version of the PSF is in MAJOR.MINOR.PATCH format according to [Sem Version 2](https://semver.org/).
* Minimum Windows Platform: The minimum windows version required for the fixup or PSF Launcher.
* Description: A short description of the fixup.
* WhenToUse: Heuristics on when you should apply the fixup.

For an example, see the [FileRedirectionFixupMetadata.xml](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/fixups/FileRedirectionFixup/FileRedirectionFixupMetadata.xml) metadata file for the redirection fixup. The metadata schema is available [here](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/master/MetadataSchema.xsd).
