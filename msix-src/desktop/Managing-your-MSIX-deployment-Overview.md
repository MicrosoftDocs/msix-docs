---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT Pros.
title: Managing your MSIX deployment Overview
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

# Managing your MSIX deployment

Packaging your application is only half the battle. Next you need to be able to deploy your application to your users. How you deploy your application depends on who your customer is.  This section will discuss deployment for both enterprise and retail markets. It will provide links and tips and tricks to ensuring a successful experience. 

## Enterprise Developers
Enterprise Developers and IT Pros typically use a software management system to install and manage their apps.  In this section we will discuss:
* Using Configman and Intune to manage your apps
* Using provisioning and DISM to preconfigure machines with MSIX
* Controlling app access with Group Policies and AppLocker
* Distributing apps through the WEB or Microsoft Store for Business
* Using AppCenter for building, testing, releasing, and desktop apps
 
## Retail Developers
Retail developers have different tools to distribute their applications.  For retail Developers we will discuss:
* Microsoft Store
* Side loading
* Using AppCenter for building, testing, releasing, and desktop apps

## Know your target devices 
No matter if you are targeting the retail market or enterprise, the key to successful distribution is knowing the devices your deployment is targeting.  Some enterprise companies have a single operating system distributed through out the organization.  Others have a mixed collection of hardware and operating systems.  Inorder to be succcesful in an mixed environment is to create a solution that will install easily on all operating systems while limiting the variations in installer technologies. 

All developers also need to know the minimum supported operating system they want to target.  Targeting the lowest common denominator of the operating system, may get you the best potential reach, but often earlier releases of the operating system may not support certain API calls your application is built using.   The following table will help all developers determine if they should us MSIXCore or MSIX. 


| Operating System | Version | MSIX Requirement |
| -----------------------|------------------------|------------------------|
|Windows 10 Client| 1709 (10.0.16299.0) or greater| MSIX Native Support|
|Windows 10 Server with Desktop Experience | 1709 (10.0.16299.0) or greater| Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|
|Windows 10 Client| 10.0.10240.0 – 10.0.15063.0 | Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|
|Windows 8 | | 		Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|
|Windows 7 | | 		Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|

In addition, MSIX Native Support has improved with each release of Windows 10.  For a complete breakdown of MSIX features and the supporting operating system, see [LINKINKINKI BUGBUG ](..link link)
