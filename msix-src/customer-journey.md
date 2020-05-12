---
title: Customer journeys
description: This article describes MSIX customer journey. 
author: dianmsft
ms.date: 05/12/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# MSIX Customer Journeys 
* [How DB Systel is using Microsoft technology to enable agile deployment of Windows Applications](customer-journey.md#DB-Systel) 
* [How ECNO is using Microsoft technology for improving the Desktop App delivery and install experience for Ontario students](customer-journey.md#ecno)
* [How Schneider Electric is using Microsoft technology for developing and deploying WPF Desktop Applications](customer-journey.md#schneider-electric) 

## DB Systel
![DB Systel Logo](images/DB_logo_red_outlined_200px_rgb.png)

DB Systel GmbH, headquartered in Frankfurt am Main, is a wholly owned subsidiary of DB AG and a digital partner for all Group companies. Deutsche Bahn AG is the second-largest transport company in the world and is the largest railway operator and infrastructure owner in Europe. It operates large parts of the German railway and It carries about two billion passengers annually.
 
DB Systel employees around 4,600 people, they operate 600 line-of-business applications, 100,000 PC workstations, 93,000 VoIP PBXs and 200,000 mobile devices, etc. They handle all the IT infrastructure of the company, from the traditional IT services to the development of all the internal applications used to control all the aspects of the railway system. 
 
For DB Systel, Desktop Applications are a critical component of the infrastructure. They are the main interface for many critical tasks, from managing employees to ensure the correct functioning of the railway system. DB Systel develop, maintain, and deploy a total of 600 fat client desktop applications and around 200 Java applications.

When it comes to desktop applications, they were facing a few challenges mainly around the following topics:
* Many of their server-side applications are built, tested, and provided via build pipelines using highly automated processes – several times a day (DevOps).  However, the current deployment technologies made impossible, so far, to achieve the same goal with Windows desktop applications.
* Many teams are involved in the development and deployment process which delayed in several days before users could get the latest versions of the software. 
* The old software deployment process was very time-consuming, lengthy and expensive.
* Many of their business applications are based on the Java Web Start technology, which has been discontinued.
 
As a result of these challenges, DB Systel was only able to provide short-term updates with great effort, which became a critical problem since many of their applications rely on a specific software version in the backend. It's essential that the client software for the user is updated directly after the software update in the backend. If this is not the case, the user's ability to work with the software in question is no longer guaranteed and it can lead to disruptions to the rail services.
 
They first heard about MSIX when they started to investigate how to replace the Java Web Start technology. MSIX was promising because it would have allowed them to create self-contained application, that aren't dependent on the Java Runtime Environment being installed. This would have saved the teams time-consuming coordination and synchronization efforts and lead to more stable operation. When they started to experiment with MSIX, they quickly  understood it was the right technology not just to support the Java Web Start migration, but also to solve their top pain points around packaging and distribution.

MSIX enabled DB Systel to:
* Simplify the traditional packaging and deployment of software packages.
* Enable software developers to own the whole end-to-end process of building and deploying software, instead of delegating the packaging and distribution processes to special teams.
* Automate existing manual processes thanks to pipelines.
* Enable speed and simplicity in Windows desktop apps deployment, which will lead to significant cost savings through the new self-service approach.

*In the past, we’d have lots of teams involved in the process and it took us time before reaching the point where our application managers could use and update our software. Consequently, we were only able to distribute releases (updates) to our customers with great effort.. Following a very informative and fruitful MSIX workshop together with Microsoft experts, we are certain that we can revolutionize the software provisioning process at DB Systel by using MSIX self-service. MSIX offers big advantages as a container format in terms of speed and simplicity. Application managers themselves can package software using MSIX and provide their software via our store.*
-Markus Thomann, Software Consultant in the Modern Deployment team at DB

DB system is integrating MSIX into the build process as a container format. Most of their applications, including many mission-critical applications, will be ported to the MSIX format. This will make the software provisioning process simpler, faster, and cheaper. Thanks to MSIX and the Modern Deployment team, application managers can now provide end users software updates directly - and many times a day.
 
*The MSIX technology allows us to adopt the DevOps approach even though we provide client software rather than cloud software. This was inconceivable until very recently.*-Markus Thomann, Software Consultant in the Modern Deployment team at DB

## ECNO 

![ECNO logo](images/ECNO_masterlogo.png)

The Educational Computing Network of Ontario (ECNO) is an organization that collaboratively finds and executes effective IT solutions for Ontario’s 72 K-12 school boards (2 million students). TECNO is tasked with keeping up on the latest computing technologies as they relate to the education goals of province of Ontario. They make IT infrastructure recommendations to and share best practices with the member school boards. 

The school boards are autonomous and are free to implement their own IT strategies for what works best for them. For example, one school board uses Configuration Manager exclusively to distribute applications, while another uses it only for machine imaging and a small percentage of apps. 
 
Under the umbrella of ECNO, the Shared Technology Services Project Team evaluates, tests, and packages an app catalog of 450 third party apps for the spectrum of needs for K – 12 students as well as staff.

To date, the ECNO STS Team has converted most of their apps via the MSIX Packaging Tools and the Bulk Conversion tool included in the MSIX Toolbox. 

ECNO currently have approximately 170 apps in Partner Center, with ten school boards linked to their LOB account. Two are currently allowing access on a ‘self-service’ basis to our apps in their store. The 3rd has Configuration Manager linked to their store already and is looking to add access to our apps that way. Six more are  looking to move into software management/assignment via Intune and has requested some sample apps to work with. And another School board is finishing a project to move their iPads into Intune as an MDM tool and then will be moving windows devices into Intune (where they are looking at using MSIX and the store as their software distribution model). They expect more boards to start switching over as they move to adopt Intune/Configuration Manager.

*It’s a push in the right direction for us to deliver software to our users that are not able to be on a school network at this time during Covid 19. AppV packages have been a boon to us for years and this next iteration using MSIX will allow us to service our staff and students with the software they require using a delivery system that is modern and seamless from any internet connection in the world.* 
-Jason David Flannery, Systems Engineer, Simcoe County District School Board

It’s easy to configure and easy to deploy once we enrolled devices into Windows Intune using autopilot. 

What benefits ECNO is getting by leveraging MSIX?
-	As a leader in software deployment for the school boards in Ontario, MSIX has provided a seamless transition from our App-V processes.
-	 Allows our school boards to keep current in modern software deployment tools and end-point management.
-	For our project team it has increased productivity, provided easier deployment and support for our school boards.

*As our IT team strives to become more agile, especially now with distance learning upon us, we are relying on the cloud more and more.  The next logical evolution for our organization is cloud management.  We want our school based IT staff to focus more on the successful implementation of technology, less on deployment and troubleshooting.  As we work towards zero touch deployment of our devices and apps we will be working closely with ECNO's STS team to integrate the apps we all use in the classroom into our cloud managed approach.*
-Sean Heuchert, Manager of Information, Peterborough Victoria Northumberland Clarington Catholic District School Board

## Schneider Electric
![Schneider Electric](images/Logo_SE_Green_RGB-Screen.png)

Schneider Electric develops GIS software to support both small and large utilities (electric, gas, water, fiber, coax) within the US and internationally.

In working with Microsoft, we wanted to bring our WPF applications to the latest installer technology, MSIX.  We have a specific client driven use case; we need to release and deploy frequently as a dev-ops minded software company, but each utility company wants to manage changes and individually determine what version of the application will be released to their users and when.  We have been working closely with Microsoft to meet this customer requirement.  Once we complete our upgrade to MSIX we will be able to remove some 3rd party dependencies and be a step closer to updating our applications to .NET Core 3.0. 

By leveraging MSIX, we are simplifying our development stack.  We will be using Microsoft technologies from code (C#/WPF) to build (Azure Dev Ops) to installer (MSIX).  One of the key benefits of MSIX is that developers can easily develop, debug and test the installer directly from Visual Studio.  This greatly simplifies installation specific troubleshooting and has allowed us to work closely with Microsoft. 

*The support from Microsoft as we move to MSIX has been invaluable.  We have a unique use case and Microsoft has helped us navigate any obstacles we encountered as we updated our applications.  The developers are all excited to move our applications to MSIX.* –Darlene Rouleau from Schneider Electric 