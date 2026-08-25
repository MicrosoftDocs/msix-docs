---
title: Bundling MSIX packages
description: An overview of MSIX bundles
ms.date: 08/25/2026
ms.topic: concept-article
keywords: windows 10, msix
ms.custom: "RS5, seodec18"
---

# Bundling MSIX packages

## Overview

An MSIX bundle is made up of multiple MSIX packages and can reduce the download
size for users. MSIX bundles are helpful for different architectures, language-specific
assets, varying image-scale assets, or resources that apply to specific devices. By
bundling multiple architecture versions of your application into one entity, only the
bundle needs to be uploaded to your distribution location (instead of having one for
each architecture). The Windows 10 deployment platform is aware of the `.msixbundle`
package type and will only download the files that are applicable to a device's
architecture.

### Bundle and package update constraints

For the same package family, an MSIX package update can move from a standalone `.msix`
package to an `.msixbundle`. After you ship an `.msixbundle`, plan to keep later
package updates for that package family in the bundle format. A later package update
cannot move from the `.msixbundle` back to a standalone `.msix` package. For more
information, see
[reverse update constraint](../app-package-updates.md#packages-cant-update-from-an-msixbundle-to-a-standalone-msix).

<!--
SME question: Confirm the rationale for why a later update cannot move from an
.msixbundle back to a standalone .msix package.
-->

If you're [packaging your application in Visual Studio](../package/packaging-uwp-apps.md),
the Create Package Wizard gives you a "Generate app bundle" option during package
creation. You can set this option to have Visual Studio generate an MSIX bundle for you.
If you're generating MSIX packages using the
[MSIX Packaging Tool](../packaging-tool/tool-overview.md) or
[MakeAppx.exe](manual-packaging-root.md) you can take advantage of MakeAppx.exe to
generate a bundle from individual packages e.g. after generating or converting an x86
and x64 MSIX package of your app you can use the process outlined below to bundle the
packages using MakeAppx.exe.



## Additional Resources

For more information about generating an MSIX bundle see:
- [Creating a bundle using a directory structure](/windows/win32/appxpkg/make-appx-package--makeappx-exe-#to-create-a-package-bundle-using-a-directory-structure)
- [Creating a bundle using a mapping file](/windows/win32/appxpkg/make-appx-package--makeappx-exe-#to-create-a-package-bundle-using-a-mapping-file)

For more information about signing app packages with SignTool.exe, see [this article](../package/sign-app-package-using-signtool.md).

