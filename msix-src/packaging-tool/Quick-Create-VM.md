---
title: MSIX packaging environment on Hyper-V Quick Create
description: Create a virtual environment for MSIX packaging projects using the Hyper-V Quick Create feature.
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool, Hyper-V Quick Create
ms.localizationpriority: medium
ms.custom: RS5
---

# MSIX packaging environment on Hyper-V Quick Create
 
You can create a virtual environment for MSIX packaging projects using the [Hyper-V Quick Create](/virtualization/hyper-v-on-windows/quick-start/quick-create-virtual-machine) feature. This feature is available starting in Windows 10, version 1709.

To get started, type 'Hyper-V Quick Create' in your Start menu, select **MSIX Packaging Tool Environment**, and click **Create Virtual Machine**. After you finish the wizard, start the MSIX Packaging tool on the VM from the Start menu. Or if you have Hyper-V Manager open, Click 'Quick Create...' in the top right corner of the application and it will display the same UI.

The MSIX Packaging Tool Environment is a custom Windows 10 evaluation build (version 1909) that includes the latest version of the MSIX Packaging Tool and other pre-requisites so that you can get started quickly with limited setup tasks.

![quickCreatepic1](images/QuickCreateVM.png)

> [!NOTE]
> This feature requires Hyper-V. You can learn more about Hyper-V and how to enable it [here](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).