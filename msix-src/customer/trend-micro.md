---
title: Trend Micro
description: TREND Micro describes their experience with MSIX
author: andreww-msft
ms.date: 03/10/2021
ms.topic: article
keywords: windows 10, uwp, msix
ms.custom: RS5
---
# Trend Micro

![Trend Micro logo](../images/Trend_Micro-lightmode.png)

Trend Micro Incorporated., a global leader in cybersecurity, helps to make the world safe for exchanging digital information. In an increasingly connected world, our innovative solutions for businesses, governments, and consumers provide layered security for data centers, cloud environments, networks, and endpoints.

Besides the security sector, we are also looking for new opportunities in other domains, such as system maintenance and optimization. For example, we are developing Cleaner One, an innovative app to help users gain more free disk space (by removing junks, big files, duplicate files, etc.) and optimize their computer performance. At present, Cleaner One has two distribution channels, Microsoft Store and Online.

During our development, we faced some challenges and finally resolved them by using new Windows development technologies. 

Previously, Cleaner One Store Version was developed for the Universal Windows Platform (UWP); Online Version was a desktop app adopting Win32 tech. It was difficult to maintain two different code branches. In order to unify both branches, we chose and applied Electron and Windows Packaging (Desktop Bridge), and it worked out well in practice. Furthermore, by leveraging C++/WinRT, we successfully implemented Windows 10 “Windows Toast Notification” and “Startup Task” APIs in the unified version.  

In Cleaner One,  Electron includes Chromium Engine whose package size is large, making downloading and upgrading the whole package difficult, especially when there are network connection issues. Since MSIX is a modern packaging method on Windows and supports Incremental Upgrading well, with the help of MS Windows AppConsult, we started implementing MSIX packaging, which helps a lot not only on incremental upgrading, but also on simplifying CI/CD in our DevOps pipeline. Now Windows modern packaging runs smoothly in our environment. Meanwhile, our Online Version of product package can even benefit from MSIX.

With these technologies, we helped our users and improved our acquisitions as well. 
-	By leveraging Windows Packaging, we unified our code branches of Store Version and Online Version.
-	By integrating “Windows Toast Notification” API, we delivered better and more consistent user experience with less interference.
-	By integrating “Startup Task” API, we provided users the option to enable or disable Cleaner One. We used to get lots of user concerns regarding the ability to control auto startup of the app.
-	By using MSIX, we are able to make our product modernized in deployment, improve upgrade experience for users, and simplify our DevOps pipeline properly.

*“MSIX and WinRT both are exciting techs for us. MSIX unifies the format of our Store Version and Online Version, makes packaging and deployment easier for developers.  I am hoping that we can further digest MSIX and use it to empower our deployment process. Compared to Win32 API, C++/WinRT is object-oriented, powerful and yet easier to understand. More importantly, it not only supports UWP apps, but also gives us the opportunity to use latest Windows 10 techs in traditional Windows apps.”* -  Developer Leader, Trend Micro
