---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enterprise and retail environment.  This article is targeted at enterprise and IT Pros.
title: Manage your MSIX deployment Overview
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

# Manage your MSIX deployment

Packaging your application is only half the battle. Next you need to be able to deploy your application to your users. How you deploy your application depends on who your customer is.  This section, Managing your MSIX deployment, will discuss deployment of MSIX packages for both enterprise and retail markets. It will provide links and tips and tricks to ensuring a successful experience. 

In order to successfully deploy MSIX, you need to consider the following:
* Who is my customer?
* What dependencies do I have?
* How will I provide the best support for my customer?

## Who is my customer?
How you deploy often depends on who is your customer and your role as a developer or administrator.   It is important to identify your role to know what tools to use.

### IT Pros
IT Pros typically use a software management system to install and manage their apps.  In the [Distribute your MSIX in an enterprise environment](managing-your-msix-deployment-enterprise.md) section we will discuss:
* Using Microsoft Endpoint Configuration Manager and Intune to manage your apps
* Using provisioning and DISM to preconfigure machines with MSIX
* Controlling app access with Group Policies and AppLocker
* Distributing apps through the web or Microsoft Store for Business
* Using AppCenter for building, testing, releasing, and desktop apps
 
### Developers
Developers have different tools to distribute their applications.  In the [Distribute your MSIX in a retail environment](managing-your-msix-deployment-retail.md) section we will discuss:  
* Microsoft Store
* Web Installer
* Using AppCenter for building, testing, releasing, and desktop apps

## Dependencies
MSIX is a robust and reliable modern app install experience. The MSIX experience delivers a 99.96% successful install rate.  But there are some caveats. MSIX is not supported by default on all versions of Windows, and the supported feature set may very depending on which version of Windows 10 you are deploying to.  In the [Plan for your deployment](managing-your-msix-deployment-targetdevices.md) section we will discuss the importance of understanding the target device that the MSIX will be deployed to. 

## Providing support for my customer
Though MSIX has a 99.96% successful install rate, you still need to plan how to support your customer.  In the [MSIX Validation and Troubleshooting section](managing-your-msix-deployment-overview.md) we discuss tools available for you to diagnose installation issues.


 
