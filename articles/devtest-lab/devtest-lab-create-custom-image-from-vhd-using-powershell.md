---
title: "PowerShell-lel VHD-fájl az Azure DevTest Labs egyéni lemezképének aaaCreate |} Microsoft Docs"
description: "Egyéni lemezképként az Azure DevTest Labs szolgáltatásban, a PowerShell használatával VHD-fájl létrehozásának automatizálása"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 39b4005fa46cdf86cf0800ca376128134bcfb650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások

hello következő lépések végigvezetik egyéni lemezkép létrehozása a PowerShell használatával VHD-fájlt:

1. Egy PowerShell-parancssorba, jelentkezzen be Azure-fiók tooyour a következő hívást toohello hello **Login-AzureRmAccount** parancsmag.  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  Jelölje be hello Azure-előfizetés szükséges hívási hello által **Select-AzureRmSubscription** parancsmag. Cserélje le a következő hello helyőrzője hello **$subscriptionId** változó, egy érvényes Azure-előfizetéssel. 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  Hello labor objektum lekéréséhez hívó hello **Get-AzureRmResource** parancsmag. Cserélje le a következő hello helyőrzőit hello **$labRg** és **$labName** hello változók a megfelelő értékek környezetnek. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  Beszerzése hello tesztkörnyezet tárfiókja és a tesztkörnyezet tárfiókja kulcsértékei hello labor objektum. 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Cserélje le a következő hello helyőrzője hello **$vhdUri** változó hello URI, tooyour feltöltött VHD-fájlt. Hello VHD fájl URI Azonosítóját az hello tárolási fiók blob panel az Azure-portálon hello kérheti le.

    ```PowerShell
    $vhdUri = '<Specify hello VHD URI here>'
    ```

1.  Hello egyéni lemezkép létrehozása hello **New-AzureRmResourceGroupDeployment** parancsmag. Cserélje le a következő hello helyőrzőit hello **$customImageName** és **$customImageDescription** változók toomeaningful nevek a környezethez.

    ```PowerShell
    $customImageName = '<Specify hello custom image name>'
    $customImageDescription = '<Specify hello custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-toocreate-a-custom-image-from-a-vhd-file"></a>PowerShell parancsfájl toocreate egy VHD-fájlt egy egyéni lemezkép

a következő PowerShell-parancsfájl hello használt toocreate egy VHD-fájlt az egyéni kép is lehet. (Kezdő és Záró csúcsos zárójelek rendelkező) hello helyőrzőket cserélje le az igényeinek megfelelő értékeket hello. 

```PowerShell
# Log in tooyour Azure account.  
Login-AzureRmAccount

# Select hello desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Get hello lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get hello lab storage account and lab storage account key values.
$labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set hello URI of hello VHD file.  
$vhdUri = '<Specify hello VHD URI here>'

# Set hello custom image name and description values.
$customImageName = '<Specify hello custom image name>'
$customImageDescription = '<Specify hello custom image description>'

# Set up hello parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create hello custom image. 
New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Kapcsolódó blogbejegyzések

- [Egyéni lemezképek vagy képletek?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Az Azure DevTest Labs között egyéni lemezképek másolása](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Következő lépések

- [Virtuális gép tooyour labor hozzáadása](./devtest-lab-add-vm-with-artifacts.md)
