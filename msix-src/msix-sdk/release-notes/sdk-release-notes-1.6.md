---
title: MSIX SDK Release 1.6 release notes
description: This article provides release notes for version 1.6 of the MSIX SDK. This SDK is available on GitHub.
ms.date: 02/06/2018
ms.topic: release-notes
keywords: windows 10, uwp, msix
ms.custom: 19H1
---

# MSIX SDK 1.6 update

With the SDK release (1.6), we heard the feedback from our partners and added more APIs to provide developers with more options and flexibility in handling MSIX packages. 

## UTF8 API Variants

In this SDK release, we add about 14 new UTF8 API variants for existing API calls. With the inclusion of these new APIs, developers can choose to use the Utf8 variant for string manipulation according to their environment/platform. As with AppxPackaging APIs, the caller is responsible for deallocating the memory used by LPSTR* out parameters.

The following are the new UTF8 interfaces:
- IAppxBlockMapFileUtf8
- IAppxBlockMapReaderUtf8
- IAppxBundleManifestPackageInfoUtf8
- IAppxBundleReaderUtf8
- IAppxFactoryUtf8
- IAppxFileUtf8
- IAppxManifestApplicationUtf8
- IAppxManifestPackageDependencyUtf8
- IAppxManifestPackageIdUtf8
- IAppxManifestPropertiesUtf8
- IAppxManifestQualifiedResourceUtf8
- IAppxManifestResourcesEnumeratorUtf8
- IAppxManifestTargetDeviceFamilyUtf8
- IAppxPackageReaderUtf8


## Override Language Selection 

By default, when handling app bundles, MSIX SDK returns the language package that is applicable by selecting the language that is also default on the system. This API allows the app to enumerate the language packages that are available and override the language package that will be returned while handling app bundles. 

## Other updates and improvements

In this update, 
- Update the OpenSSL lib dependency to 1.0.2q
- Fixed how we handle international characters 

You can get the latest SDK on GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.6" data-linktype="external">MSIX SDK on GitHub</a></p></div>

