---
title: DB-Systel
description: DB Systel describes their experience with MSIX
author: dianmsft
ms.date: 65/25/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---
# DB Systel

![DB Systel Logo](../images/DB_logo_red_outlined_200px_rgb.png)

DB Systel GmbH, headquartered in Frankfurt am Main, is a wholly owned subsidiary of DB AG and a digital partner for all Group companies. Deutsche Bahn AG is the second-largest transport company in the world and is the largest railway operator and infrastructure owner in Europe. It operates large parts of the German railway and It carries about two billion passengers annually.

DB Systel employees around 4,600 people, they operate 600 line-of-business applications, 100,000 PC workstations, 93,000 VoIP PBXs and 200,000 mobile devices, etc. They handle all the IT infrastructure of the company, from the traditional IT services to the development of all the internal applications used to control all the aspects of the railway system. 

For DB Systel, desktop applications are a critical component of the infrastructure. They are the main interface for many critical tasks, from managing employees to ensure the correct functioning of the railway system. DB Systel develop, maintain, and deploy a total of 600 fat client desktop applications and around 200 Java applications.

When it comes to desktop applications, they were facing a few challenges mainly around the following topics:

* Many of their server-side applications are built, tested, and provided via build pipelines using highly automated processes – several times a day (DevOps). However, the current deployment technologies made impossible, so far, to achieve the same goal with Windows desktop applications.
* Many teams are involved in the development and deployment process which delayed in several days before users could get the latest versions of the software.
* The old software deployment process was very time-consuming, lengthy and expensive.
* Many of their business applications are based on the Java Web Start technology, which has been discontinued.

As a result of these challenges, DB Systel was only able to provide short-term updates with great effort. This became a critical problem because many of their applications rely on a specific software version in the backend. It's essential that the client software for the user is updated directly after the software update in the back end. If this is not the case, the user's ability to work with the software in question is no longer guaranteed and it can lead to disruptions to the rail services.

DB Systel first heard about MSIX when they started to investigate how to replace the Java Web Start technology. MSIX was promising because it would enable them to create self-contained applications that aren't dependent on the Java Runtime Environment being installed. This would save the teams time-consuming coordination and synchronization efforts and lead to more stable operation. When they started to experiment with MSIX, they quickly understood it was the right technology not just to support the Java Web Start migration, but also to solve their top pain points around packaging and distribution.

MSIX enabled DB Systel to:

* Simplify the traditional packaging and deployment of software packages.
* Enable software developers to own the whole end-to-end process of building and deploying software, instead of delegating the packaging and distribution processes to special teams.
* Automate existing manual processes thanks to pipelines.
* Enable speed and simplicity in Windows desktop apps deployment, which will lead to significant cost savings through the new self-service approach.

*"In the past, we’d have lots of teams involved in the process and it took us time before reaching the point where our application managers could use and update our software. Consequently, we were only able to distribute releases (updates) to our customers with great effort.. Following a very informative and fruitful MSIX workshop together with Microsoft experts, we are certain that we can revolutionize the software provisioning process at DB Systel by using MSIX self-service. MSIX offers big advantages as a container format in terms of speed and simplicity. Application managers themselves can package software using MSIX and provide their software via our store."*
 -Markus Thomann, Software Consultant in the Modern Deployment team at DB

DB system is integrating MSIX into the build process as a container format. Most of their applications, including many mission-critical applications, will be ported to the MSIX format. This will make the software provisioning process simpler, faster, and cheaper. Thanks to MSIX and the Modern Deployment team, application managers can now provide end users software updates directly - and many times a day.

*"The MSIX technology allows us to adopt the DevOps approach even though we provide client software rather than cloud software. This was inconceivable until very recently."* -Markus Thomann, Software Consultant in the Modern Deployment team at DB
