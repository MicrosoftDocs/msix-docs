---
title: App package architectures
description: Learn more about which processor architecture(s) you should use when building your UWP app package.
ms.date: 03/14/2023
ms.topic: article
keywords: windows 10, uwp, packaging, architecture, package configuration
---

# App package architectures

App packages are configured to run on a specific processor architecture. By selecting an architecture, you are specifying which device(s) you want your app to run on. Universal Windows Platform (UWP) apps can be configured to run on the following architectures:
- x86
- x64
- ARM
- ARM64

It is **highly** recommended that you build your app package to target all architectures. By deselecting a device architecture, you are limiting the number of devices your app can run on, which in turn will limit the amount of people who can use your app!

## Windows devices and architectures

> [!div class="mx-tableFixed"]
| UWP Architecture | Desktop (x86)      | Desktop (x64)      | Desktop (Arm64)      | Mobile             | Windows Mixed Reality and HoloLens           | Xbox               | IoT Core (Device dependent) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                 | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM               | :x:                | :x:                | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |
| ARM64              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:| :x:                | :heavy_check_mark:          | :x:                |

Let’s talk about these architectures in more detail.

### x86

Choosing x86 is the most common configuration for an app package due to the many x86 devices on the market. On some devices, an app package with the x86 configuration won't run, such as the Xbox or some IoT Core devices. However, for a PC, an x86 package is the safest choice and has the largest reach for device deployment. A substantial portion of Windows devices continue to run the x86 version of Windows. Arm devices running [Windows 10 or Windows 11 also support x86 apps via emulation](/windows/arm/apps-on-arm-x86-emulation).

### x64

This configuration is used less frequently than the x86 configuration. It should be noted that this configuation is reserved for desktops using 64-bit versions of Windows, [UWP apps on Xbox](/windows/uwp/xbox-apps/system-resource-allocation), and Windows IoT Core on the Intel Joule.

### Arm64

The Windows on Arm configuration includes desktop PCs and some IoT Core devices (Rasperry Pi 2, Raspberry Pi 3, and DragonBoard). Arm-powered devices are particularly interesting because the power-frugal nature of the Arm architecture enables these devices to offer longer battery life while delivering great performance. Arm Systems on Chip (SoC) often include other key features such as a powerful CPU, GPU, Wi-Fi & mobile data networks, as well as Neural Processor Units (NPUs) for accelerating AI workloads. Windows 10 enables existing unmodified x86 apps to run on Arm devices. Windows 11 adds the ability to run unmodified x64 Windows apps on Arm devices. This ability to run x86 & x64 apps on Arm devices gives end-users confidence that the majority of their existing apps & tools will run well even on new Arm-powered devices.

For more information, see the [Windows on Arm documentation](/windows/arm/overview), where you can find info on [Arm64EC](/windows/arm/arm64ec) (to incrementally transition existing x64 apps to take advantage of the native speed and performance possible with Arm-powered devices), Arm developer tools like [Visual Studio for Arm](/visualstudio/install/visual-studio-on-arm-devices), services like [Azure VMs with Arm-based processors](https://azure.microsoft.com/blog/now-in-preview-azure-virtual-machines-with-ampere-altra-armbased-processors/), and new devices like the [Windows Dev Kit 2023](/windows/arm/dev-kit/).

## Arm 32-bit no longer supported 

Windows devices running on an Arm processor *(for example, Snapdragon processors from Qualcomm)* will no longer support AArch32 (Arm32). This change only impacts Universal Windows Platform apps that presently target AArch32 (Arm32). [Support for 32-bit Arm versions of applications will be removed in a future release of Windows 11.](https://www.microsoft.com/windows/windows-11-specifications#table3). After this change, for the small number of applications affected, app features might be different and you might notice a difference in performance. For guidance on updating your targeted platforms to AArch64 (Arm64), which is supported on all Windows on Arm devices, see [Update app architecture from Arm32 to Arm64](/windows/arm/arm32-to-arm64).

## IoT specific deployment

For more information about IoT specific topics, see [Deploying an App with Visual Studio](/windows/iot-core/develop-your-app/AppDeployment).
