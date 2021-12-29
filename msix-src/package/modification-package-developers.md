---
title: Building an app with a modification package 
description: This article describes how modification packages will work with their app when it is packaged as an MSIX 
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.custom: "RS5, seodec18"
---

# Building an app with a modification package 
Modification packages allow IT Pros to customize their MSIX apps without having to repackage the app again. What this means is that when the IT Pro deploys the main app and the modification package and the user runs the app, the customization contained in the modification package will overlay on top of the main app such that it will feel like one app to the user. This is because modification packages run inside the main app's MSIX container and can only be accessed through the main app. Modification package example are plugin/addons, files and registry keys. This is something to keep in mind when developing your application, especially for enterprises. 

By having the main app and modification package model, as a developer, your enterprise customer will get the latest and greatest from you when you an update without needing to repackage. This is because the customization is all stored in the modification package which can be deployed and maintained independently. 

