---
title: az Azure Resource Manager-sablon egy Azure Automation-runbook aaaDeploy |} Microsoft Docs
description: "Hogyan toodeploy Azure Resource Manager-sablon Azure Storage szolgáltatásban tárolja egy runbookból"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: PowerShell, a runbook, json, az azure automation
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 07/09/2017
ms.author: eslesar
ms.openlocfilehash: f489a8e8635a48f5a6a2f1a88e1c803f56f01832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a><span data-ttu-id="d79cd-104">Egy Azure Automation PowerShell runbookban egy Azure Resource Manager-sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d79cd-104">Deploy an Azure Resource Manager template in an Azure Automation PowerShell runbook</span></span>

<span data-ttu-id="d79cd-105">Írhat egy [Azure Automation PowerShell-forgatókönyv](automation-first-runbook-textual-powershell.md) használatával egy Azure-erőforrás, amely telepít egy [Azure Resource Manager sablon](../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="d79cd-105">You can write an [Azure Automation PowerShell runbook](automation-first-runbook-textual-powershell.md) that deploys an Azure resource by using an [Azure Resource Management template](../azure-resource-manager/resource-manager-create-first-template.md).</span></span>

<span data-ttu-id="d79cd-106">Ezzel az Azure-erőforrások telepítési automatizálható.</span><span class="sxs-lookup"><span data-stu-id="d79cd-106">By doing this, you can automate deployment of Azure resources.</span></span> <span data-ttu-id="d79cd-107">Akkor is fenntartható a Resource Manager-sablonok, például az Azure Storage központi, biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="d79cd-107">You can maintain your Resource Manager templates in a central,secure location such as Azure Storage.</span></span>

<span data-ttu-id="d79cd-108">Ebben a témakörben egy Resource Manager-sablon tárolt használó PowerShell runbookot létrehozhatunk [Azure Storage](../storage/common/storage-introduction.md) toodeploy egy új Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d79cd-108">In this topic, we create a PowerShell runbook that uses an Resource Manager template stored in [Azure Storage](../storage/common/storage-introduction.md) toodeploy a new Azure Storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d79cd-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d79cd-109">Prerequisites</span></span>

<span data-ttu-id="d79cd-110">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d79cd-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d79cd-111">Egy Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d79cd-111">Azure subscription.</span></span> <span data-ttu-id="d79cd-112">Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="d79cd-112">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="d79cd-113">[Automation-fiók](automation-sec-configure-azure-runas-account.md) toohold hello runbook és tooAzure erőforrások hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d79cd-113">[Automation account](automation-sec-configure-azure-runas-account.md) toohold hello runbook and authenticate tooAzure resources.</span></span>  <span data-ttu-id="d79cd-114">Ezt a fiókot kell toostart engedéllyel rendelkezik, és állítsa le a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="d79cd-114">This account must have permission toostart and stop hello virtual machine.</span></span>
* <span data-ttu-id="d79cd-115">[Az Azure Storage-fiók](../storage/common/storage-create-storage-account.md) mely toostore hello Resource Manager sablon</span><span class="sxs-lookup"><span data-stu-id="d79cd-115">[Azure Storage account](../storage/common/storage-create-storage-account.md) in which toostore hello Resource Manager template</span></span>
* <span data-ttu-id="d79cd-116">Az Azure Powershell telepítve a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d79cd-116">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="d79cd-117">Lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) információ tooget Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d79cd-117">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how tooget Azure PowerShell.</span></span>

## <a name="create-hello-resource-manager-template"></a><span data-ttu-id="d79cd-118">Hello Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="d79cd-118">Create hello Resource Manager template</span></span>

<span data-ttu-id="d79cd-119">A jelen példában használjuk egy Resource Manager-sablon, amely telepít egy új Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d79cd-119">For this example, we use an Resource Manager template that deploys a new Azure Storage account.</span></span>

<span data-ttu-id="d79cd-120">Egy szövegszerkesztőben másolja a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="d79cd-120">In a text editor, copy hello following text:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

<span data-ttu-id="d79cd-121">Mentse hello fájlt helyben `TemplateTest.json`.</span><span class="sxs-lookup"><span data-stu-id="d79cd-121">Save hello file locally as `TemplateTest.json`.</span></span>

## <a name="save-hello-resource-manager-template-in-azure-storage"></a><span data-ttu-id="d79cd-122">Az Azure Storage hello Resource Manager sablon mentése</span><span class="sxs-lookup"><span data-stu-id="d79cd-122">Save hello Resource Manager template in Azure Storage</span></span>

<span data-ttu-id="d79cd-123">PowerShell toocreate egy Azure Storage fájlmegosztás használni és feltölteni hello `TemplateTest.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d79cd-123">Now we use PowerShell toocreate an Azure Storage file share and upload hello `TemplateTest.json` file.</span></span>
<span data-ttu-id="d79cd-124">Hogyan toocreate fájl megosztása és hello Azure-portálon a fájl feltöltése, lásd: [Ismerkedés az Azure File storage on Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="d79cd-124">For instructions on how toocreate a file share and upload a file in hello Azure portal, see [Get started with Azure File storage on Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="d79cd-125">A helyi számítógépen indítsa el a Powershellt, és futtassa a következő parancsok toocreate fájlmegosztás hello, és töltse fel a hello Resource Manager sablon toothat fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="d79cd-125">Launch PowerShell on your local machine, and run hello following commands toocreate a file share and upload hello Resource Manager template toothat file share.</span></span>

```powershell
# Login tooAzure
Login-AzureRmAccount

# Get hello access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using hello first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add hello TemplateTest.json file toohello new file share
# "TemplatePath" is hello path where you saved hello TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-hello-powershell-runbook-script"></a><span data-ttu-id="d79cd-126">Hello PowerShell runbook parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d79cd-126">Create hello PowerShell runbook script</span></span>

<span data-ttu-id="d79cd-127">Most egy PowerShell-parancsfájlt, amely lekérdezi a hello létrehozhatunk `TemplateTest.json` Azure Storage-ból fájlt, és telepíti a hello sablon toocreate egy új Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="d79cd-127">Now we create a PowerShell script that gets hello `TemplateTest.json` file from Azure Storage and deploys hello template toocreate a new Azure Storage account.</span></span>

<span data-ttu-id="d79cd-128">Egy szövegszerkesztőben illessze be a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="d79cd-128">In a text editor, paste hello following text:</span></span>

```powershell
param (
    [Parameter(Mandatory=$true)]
    [string]
    $ResourceGroupName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountKey,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageFileName
)



# Authenticate tooAzure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set hello parameter values for hello Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy hello storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

<span data-ttu-id="d79cd-129">Mentse hello fájlt helyben `DeployTemplate.ps1`.</span><span class="sxs-lookup"><span data-stu-id="d79cd-129">Save hello file locally as `DeployTemplate.ps1`.</span></span>

## <a name="import-and-publish-hello-runbook-into-your-azure-automation-account"></a><span data-ttu-id="d79cd-130">Importálja és hello runbook közzététele az Azure Automation-fiók be</span><span class="sxs-lookup"><span data-stu-id="d79cd-130">Import and publish hello runbook into your Azure Automation account</span></span>

<span data-ttu-id="d79cd-131">Most azt PowerShell tooimport hello runbook használja az Azure Automation-fiók be, és tegye közzé az hello runbook.</span><span class="sxs-lookup"><span data-stu-id="d79cd-131">Now we use PowerShell tooimport hello runbook into your Azure Automation account, and then publish hello runbook.</span></span>
<span data-ttu-id="d79cd-132">További információ tooimport és runbook közzététele a hello Azure-portálon, lásd: [létrehozása vagy importálása az Azure Automationben runbook](automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="d79cd-132">For information about how tooimport and publish a runbook in hello Azure portal, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md).</span></span>

<span data-ttu-id="d79cd-133">tooimport `DeployTemplate.ps1` be az Automation-fiók egy PowerShell-forgatókönyv szerint, futtassa a következő PowerShell-parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="d79cd-133">tooimport `DeployTemplate.ps1` into your Automation account as a PowerShell runbook, run hello following PowerShell commands:</span></span>

```powershell
# MyPath is hello path where you saved DeployTemplate.ps1
# MyResourceGroup is hello name of hello Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is hello name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @

# Publish hello runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-hello-runbook"></a><span data-ttu-id="d79cd-134">Hello runbook elindítása</span><span class="sxs-lookup"><span data-stu-id="d79cd-134">Start hello runbook</span></span>

<span data-ttu-id="d79cd-135">Most először hello runbook által hívó hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="d79cd-135">Now we start hello runbook by calling hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.</span></span>

<span data-ttu-id="d79cd-136">További információ a hogyan toostart egy runbookot a hello Azure-portálon: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="d79cd-136">For information about how toostart a runbook in hello Azure portal, see [Starting a runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>

<span data-ttu-id="d79cd-137">Futtassa a következő PowerShell-konzol hello hello:</span><span class="sxs-lookup"><span data-stu-id="d79cd-137">Run hello following commands in hello PowerShell console:</span></span>

```powershell
# Set up hello parameters for hello runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for hello Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start hello runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

<span data-ttu-id="d79cd-138">hello runbook fut, és annak állapotát futtatásával ellenőrizheti `$job.Status`.</span><span class="sxs-lookup"><span data-stu-id="d79cd-138">hello runbook runs, and you can check its status by running `$job.Status`.</span></span>

<span data-ttu-id="d79cd-139">hello runbook lekérdezi hello Resource Manager-sablon, és toodeploy egy új Azure-tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="d79cd-139">hello runbook gets hello Resource Manager template and uses it toodeploy a new Azure Storage account.</span></span>
<span data-ttu-id="d79cd-140">Láthatja, hogy az új tárfiók hello létrejött-e hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d79cd-140">You can see that hello new storage account was created by running hello following command:</span></span>
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a><span data-ttu-id="d79cd-141">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d79cd-141">Summary</span></span>

<span data-ttu-id="d79cd-142">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="d79cd-142">That's it!</span></span> <span data-ttu-id="d79cd-143">Most már használhatja Azure Automation és az Azure Storage és a Resource Manager sablonok toodeploy összes Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d79cd-143">Now you can use Azure Automation and Azure Storage, and Resource Manager templates toodeploy all your Azure resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d79cd-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d79cd-144">Next steps</span></span>

* <span data-ttu-id="d79cd-145">További információ a Resource Manager-sablonok, toolearn lásd [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d79cd-145">toolearn more about Resource Manager templates, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="d79cd-146">Lásd az Azure Storage tooget [bemutatása tooAzure tárolási](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d79cd-146">tooget started with Azure Storage, see [Introduction tooAzure Storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="d79cd-147">toofind más hasznos Azure Automation-forgatókönyv, lásd: [az Azure Automation forgatókönyv- és gyűjtemények](automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="d79cd-147">toofind other useful Azure Automation runbooks, see [Runbook and module galleries for Azure Automation](automation-runbook-gallery.md).</span></span>
* <span data-ttu-id="d79cd-148">toofind más hasznos Resource Manager-sablonok, lásd: [Azure gyors üzembe helyezés sablonok](https://azure.microsoft.com/resources/templates/)</span><span class="sxs-lookup"><span data-stu-id="d79cd-148">toofind other useful Resource Manager templates, see [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/)</span></span>

