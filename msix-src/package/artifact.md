---
title: MSIX Persistent Identity 
description: This article describes how to sign a package with a different certificate while maintaining the app update experience
ms.date: 07/02/2021
ms.topic: article
keywords: windows 10, msix, uwp, desktop, package
ms.localizationpriority: medium
---

# MSIX Persistent Identity 
This feature enables the ability to sign packages with a new certificate while still maintaining the app’s update experience. In other words, this allows the package to persist their old package identity in the platform without having the need to sign with the old (original) certificate. To achieve this, an artifact will need to be created to show the relationship between the old certificate and the new certificate that is being used for signing. Below is a step-by-step explanation of how to persist with the package identity to maintain he update experience. 
