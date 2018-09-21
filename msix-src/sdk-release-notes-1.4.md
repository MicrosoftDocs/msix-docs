---
title: MSIX SDK Ver 1.4 Release Notes | Microsoft Docs
description: release notes for msix sdk ver 1.4
author: c-don
ms.author: cdon
ms.date: 09/12/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# MSIX SDK 1.4 Update

With the SDK version (1.4), we are continuing to add more functionality and make performance improvements to our SDK.  With the release of 1.4, developers can use the SDK to now unpack and extract app bundles and [flat bundles](https://docs.microsoft.com/en-us/windows/uwp/packaging/flat-bundles?context=/windows/msix/render). 

With the support of bundles, client apps can now extract app bundles and only download and extract the packages inside the bundle that are applicable. In this version, the bundle support extends to flat bundles as well. This means that the client app that consuming the bundle can access the bundle manifest and specify the app packages that it wants to extract - leaving the choice and control to the developer. The SDK calls the native OS for language and locale information and provides that info to the client app which it can use to make the choice of appropriate package from the bundle as well.

With the new SDK, we added support for MSXML6 for Windows which removes the out of box dependency and thus reduces the size of the binary and use the native XML library. 

Along with removing dependency in Windows, we also removed the dependency on zlib(3rd library) and use inbox implementations on MacOS, iOS and Android(AOSP).  Additionally, we made other improvements to reduce the binary size on all platforms. 

Along with the performance and feature improvements, we are also including better samples that developers can use to get started with the SDK. Using the samples, developers can learn how to implement some of the public interfaces required to read the msix packages. 

You can get the latest SDK on GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging" data-linktype="external">MSIX SDK on GitHub</a></p></div>

<div class="container centered pageFooter">
        <h2>Have feedback for us? We'd love to hear it.</h2>
        <ul class="links">
           <li>
                <a href="https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX">
                    MSIX tech community
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/MSIX-PackageSupportFramework/issues">
                    Package Support Framework
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/msix-packaging/issues">
                    MSIX SDK
                </a>
            </li>
            <li>
                <a href="https://twitter.com/#!/search/realtime/%23msix">
                    Twitter
                </a>
            </li>
            
        </ul>
        
        <br>
        <br>
        
</div>


