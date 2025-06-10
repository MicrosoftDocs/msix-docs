---
ms.assetid: df4d227c-21f9-4f99-9e95-3305b149d9c5
title: App streaming install
description: App streaming install enables you to specify which parts of your app you would like the Microsoft Store to download first. 
ms.date: 07/02/2019
author: andreww-msft
ms.author: andreww
ms.topic: install-set-up-deploy
keywords: windows 10, msix, uwp, streaming install
---

# App streaming install

App streaming install enables you to specify which parts of your app you would like the Microsoft Store to download first. When the essential files of the app are downloaded first, the user can launch and interact with the app while the rest of it finishes downloading in the background.

To use app streaming install you'll need to divide your app's files into sections. To do this, you'll create a content group map, which is an XML file that's packaged with your app, allowing you to set download priority and order. See the topic linked below for more information.


| Topic | Description |
|-------|-------------|
| [Create and convert a source content group map](create-cgm.md) | To get your app ready for streaming install, you'll need to create a content group map. This article will help you with the specifics of creating and converting a content group map while providing some tips and tricks along the way. |
