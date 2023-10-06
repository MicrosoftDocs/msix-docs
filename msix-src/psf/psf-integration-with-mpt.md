---
# Required metadata
# For more information, see https://review.learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata?branch=main
# For valid values of ms.service, ms.prod, and ms.topic, see https://review.learn.microsoft.com/en-us/help/platform/metadata-taxonomies?branch=main

title:       Automated PSF config generation 
description: Integration of PSF with MSIX Packaging tool to simplify conversion by automating PSF config file generation
author:      fiza-microsoft # GitHub alias
ms.author:   fizaazmi # Microsoft alias
ms.topic:    article
ms.date:     10/06/2023
---

# Automated PSF config generation

This article talks about the integration of adding Package Support Framework (PSF) fixups with the MSIX Packaging Tool. This integration aims to simplify applying PSF fixups by automating the PSF config.json file generation, eliminating the need for manual configuration and reducing the potential for errors.

## Prerequisites

To try out this feature in an early-access preview build, join the [MSIX Packaging Tool Insider Program](/windows/msix/packaging-tool/insider-program).

### Automatically generate PSF config.json

To apply known fixups,

1. Launch the MSIX Packaging Tool, select to __Package Editor__ and navigate to the __PSF Fixups__ tab.

1. Select the __Application ID__ as present in the manifest. The Executable path will be populated automatically.

1. Select the __Fixup__ that you want to apply and fill all the mandatory parameters in the right pane.

1. You can review the configuration applied by clicking on __Preview config.json__ and then click on __Save__ to automatically apply the fixups.

  
![PSF fixups tab in MPT](media/psf-integration-with-mpt/psf-fixups-tab-in-mpt1.jpg)

### Supported PSF Features

1. Working Directory

1. Command Line Arguments 

1. PowerShell Start/End Script 

1. In Package Context 

1. File Redirection Fixup 

1. Reg Legacy Fixup 


