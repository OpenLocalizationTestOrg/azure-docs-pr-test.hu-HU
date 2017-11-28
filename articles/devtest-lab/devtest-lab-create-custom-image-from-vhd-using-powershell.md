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
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a><span data-ttu-id="0d208-103">Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl</span><span class="sxs-lookup"><span data-stu-id="0d208-103">Create a custom image from a VHD file using PowerShell</span></span>

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a><span data-ttu-id="0d208-104">Lépésenkénti utasítások</span><span class="sxs-lookup"><span data-stu-id="0d208-104">Step-by-step instructions</span></span>

<span data-ttu-id="0d208-105">hello következő lépések végigvezetik egyéni lemezkép létrehozása a PowerShell használatával VHD-fájlt:</span><span class="sxs-lookup"><span data-stu-id="0d208-105">hello following steps walk you through creating a custom image from a VHD file using PowerShell:</span></span>

1. <span data-ttu-id="0d208-106">Egy PowerShell-parancssorba, jelentkezzen be Azure-fiók tooyour a következő hívást toohello hello **Login-AzureRmAccount** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0d208-106">At a PowerShell prompt, log in tooyour Azure account with hello following call toohello **Login-AzureRmAccount** cmdlet.</span></span>  
    
    ```PowerShell
    Login-AzureRmAccount
    ```

1.  <span data-ttu-id="0d208-107">Jelölje be hello Azure-előfizetés szükséges hívási hello által **Select-AzureRmSubscription** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0d208-107">Select hello desired Azure subscription by calling hello **Select-AzureRmSubscription** cmdlet.</span></span> <span data-ttu-id="0d208-108">Cserélje le a következő hello helyőrzője hello **$subscriptionId** változó, egy érvényes Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="0d208-108">Replace hello following placeholder for hello **$subscriptionId** variable with a valid Azure subscription ID.</span></span> 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  <span data-ttu-id="0d208-109">Hello labor objektum lekéréséhez hívó hello **Get-AzureRmResource** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0d208-109">Get hello lab object by calling hello **Get-AzureRmResource** cmdlet.</span></span> <span data-ttu-id="0d208-110">Cserélje le a következő hello helyőrzőit hello **$labRg** és **$labName** hello változók a megfelelő értékek környezetnek.</span><span class="sxs-lookup"><span data-stu-id="0d208-110">Replace hello following placeholders for hello **$labRg** and **$labName** variables with hello appropriate values for your environment.</span></span> 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  <span data-ttu-id="0d208-111">Beszerzése hello tesztkörnyezet tárfiókja és a tesztkörnyezet tárfiókja kulcsértékei hello labor objektum.</span><span class="sxs-lookup"><span data-stu-id="0d208-111">Get hello lab storage account and lab storage account key values from hello lab object.</span></span> 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  <span data-ttu-id="0d208-112">Cserélje le a következő hello helyőrzője hello **$vhdUri** változó hello URI, tooyour feltöltött VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="0d208-112">Replace hello following placeholder for hello **$vhdUri** variable with hello URI tooyour uploaded VHD file.</span></span> <span data-ttu-id="0d208-113">Hello VHD fájl URI Azonosítóját az hello tárolási fiók blob panel az Azure-portálon hello kérheti le.</span><span class="sxs-lookup"><span data-stu-id="0d208-113">You can get hello VHD file's URI from hello storage account's blob blade in hello Azure portal.</span></span>

    ```PowerShell
    $vhdUri = '<Specify hello VHD URI here>'
    ```

1.  <span data-ttu-id="0d208-114">Hello egyéni lemezkép létrehozása hello **New-AzureRmResourceGroupDeployment** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0d208-114">Create hello custom image using hello **New-AzureRmResourceGroupDeployment** cmdlet.</span></span> <span data-ttu-id="0d208-115">Cserélje le a következő hello helyőrzőit hello **$customImageName** és **$customImageDescription** változók toomeaningful nevek a környezethez.</span><span class="sxs-lookup"><span data-stu-id="0d208-115">Replace hello following placeholders for hello **$customImageName** and **$customImageDescription** variables toomeaningful names for your environment.</span></span>

    ```PowerShell
    $customImageName = '<Specify hello custom image name>'
    $customImageDescription = '<Specify hello custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-toocreate-a-custom-image-from-a-vhd-file"></a><span data-ttu-id="0d208-116">PowerShell parancsfájl toocreate egy VHD-fájlt egy egyéni lemezkép</span><span class="sxs-lookup"><span data-stu-id="0d208-116">PowerShell script toocreate a custom image from a VHD file</span></span>

<span data-ttu-id="0d208-117">a következő PowerShell-parancsfájl hello használt toocreate egy VHD-fájlt az egyéni kép is lehet.</span><span class="sxs-lookup"><span data-stu-id="0d208-117">hello following PowerShell script can be used toocreate a custom image from a VHD file.</span></span> <span data-ttu-id="0d208-118">(Kezdő és Záró csúcsos zárójelek rendelkező) hello helyőrzőket cserélje le az igényeinek megfelelő értékeket hello.</span><span class="sxs-lookup"><span data-stu-id="0d208-118">Replace hello placeholders (starting and ending with angle brackets) with hello appropriate values for your needs.</span></span> 

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

## <a name="related-blog-posts"></a><span data-ttu-id="0d208-119">Kapcsolódó blogbejegyzések</span><span class="sxs-lookup"><span data-stu-id="0d208-119">Related blog posts</span></span>

- [<span data-ttu-id="0d208-120">Egyéni lemezképek vagy képletek?</span><span class="sxs-lookup"><span data-stu-id="0d208-120">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [<span data-ttu-id="0d208-121">Az Azure DevTest Labs között egyéni lemezképek másolása</span><span class="sxs-lookup"><span data-stu-id="0d208-121">Copying Custom Images between Azure DevTest Labs</span></span>](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a><span data-ttu-id="0d208-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d208-122">Next steps</span></span>

- [<span data-ttu-id="0d208-123">Virtuális gép tooyour labor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0d208-123">Add a VM tooyour lab</span></span>](./devtest-lab-add-vm-with-artifacts.md)
