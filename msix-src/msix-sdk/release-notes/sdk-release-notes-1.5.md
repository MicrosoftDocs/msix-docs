---
title: MSIX SDK Version 1.5 release notes
description: This article provides release notes for version 1.5 of the MSIX SDK. This SDK is available on GitHub.
ms.date: 02/06/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX SDK 1.5 update

With the SDK release (1.5), our main investment was in reducing the binary size of the SDK. One of the key feedback about our SDK was the size and especially for mobile apps having a binary dependency that is going to cost ~5MB isn't satisfactory. 

To resolve this, we replaced the static binary dependencies for XML parsing with inbox binaries on Android and iOS/MacOS. As part of the binary size reduction, we also added switches for the different functionality in the build scripts that is available with the SDK. By only adding functionality that your app needs in the binary, the size of the MSIX SDK binary size can be further reduced. 

You can get the latest SDK on GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.5" data-linktype="external">MSIX SDK on GitHub</a></p></div>

