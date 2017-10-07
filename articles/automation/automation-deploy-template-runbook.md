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
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Egy Azure Automation PowerShell runbookban egy Azure Resource Manager-sablon üzembe helyezése

Írhat egy [Azure Automation PowerShell-forgatókönyv](automation-first-runbook-textual-powershell.md) használatával egy Azure-erőforrás, amely telepít egy [Azure Resource Manager sablon](../azure-resource-manager/resource-manager-create-first-template.md).

Ezzel az Azure-erőforrások telepítési automatizálható. Akkor is fenntartható a Resource Manager-sablonok, például az Azure Storage központi, biztonságos helyen.

Ebben a témakörben egy Resource Manager-sablon tárolt használó PowerShell runbookot létrehozhatunk [Azure Storage](../storage/common/storage-introduction.md) toodeploy egy új Azure Storage-fiók.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* Egy Azure-előfizetés. Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).
* [Automation-fiók](automation-sec-configure-azure-runas-account.md) toohold hello runbook és tooAzure erőforrások hitelesítéséhez.  Ezt a fiókot kell toostart engedéllyel rendelkezik, és állítsa le a virtuális gép hello.
* [Az Azure Storage-fiók](../storage/common/storage-create-storage-account.md) mely toostore hello Resource Manager sablon
* Az Azure Powershell telepítve a helyi számítógépen. Lásd: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) információ tooget Azure PowerShell.

## <a name="create-hello-resource-manager-template"></a>Hello Resource Manager-sablon létrehozása

A jelen példában használjuk egy Resource Manager-sablon, amely telepít egy új Azure Storage-fiók.

Egy szövegszerkesztőben másolja a következő szöveg hello:

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

Mentse hello fájlt helyben `TemplateTest.json`.

## <a name="save-hello-resource-manager-template-in-azure-storage"></a>Az Azure Storage hello Resource Manager sablon mentése

PowerShell toocreate egy Azure Storage fájlmegosztás használni és feltölteni hello `TemplateTest.json` fájlt.
Hogyan toocreate fájl megosztása és hello Azure-portálon a fájl feltöltése, lásd: [Ismerkedés az Azure File storage on Windows](../storage/files/storage-dotnet-how-to-use-files.md).

A helyi számítógépen indítsa el a Powershellt, és futtassa a következő parancsok toocreate fájlmegosztás hello, és töltse fel a hello Resource Manager sablon toothat fájlmegosztást.

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

## <a name="create-hello-powershell-runbook-script"></a>Hello PowerShell runbook parancsfájl létrehozása

Most egy PowerShell-parancsfájlt, amely lekérdezi a hello létrehozhatunk `TemplateTest.json` Azure Storage-ból fájlt, és telepíti a hello sablon toocreate egy új Azure Storage-fiók.

Egy szövegszerkesztőben illessze be a következő szöveg hello:

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

Mentse hello fájlt helyben `DeployTemplate.ps1`.

## <a name="import-and-publish-hello-runbook-into-your-azure-automation-account"></a>Importálja és hello runbook közzététele az Azure Automation-fiók be

Most azt PowerShell tooimport hello runbook használja az Azure Automation-fiók be, és tegye közzé az hello runbook.
További információ tooimport és runbook közzététele a hello Azure-portálon, lásd: [létrehozása vagy importálása az Azure Automationben runbook](automation-creating-importing-runbook.md).

tooimport `DeployTemplate.ps1` be az Automation-fiók egy PowerShell-forgatókönyv szerint, futtassa a következő PowerShell-parancsok hello:

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

## <a name="start-hello-runbook"></a>Hello runbook elindítása

Most először hello runbook által hívó hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) parancsmag.

További információ a hogyan toostart egy runbookot a hello Azure-portálon: [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md).

Futtassa a következő PowerShell-konzol hello hello:

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

hello runbook fut, és annak állapotát futtatásával ellenőrizheti `$job.Status`.

hello runbook lekérdezi hello Resource Manager-sablon, és toodeploy egy új Azure-tárfiókot használja.
Láthatja, hogy az új tárfiók hello létrejött-e hello a következő parancs futtatásával:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Összefoglalás

Ennyi az egész! Most már használhatja Azure Automation és az Azure Storage és a Resource Manager sablonok toodeploy összes Azure-erőforrások.

## <a name="next-steps"></a>Következő lépések

* További információ a Resource Manager-sablonok, toolearn lásd [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md)
* Lásd az Azure Storage tooget [bemutatása tooAzure tárolási](../storage/common/storage-introduction.md).
* toofind más hasznos Azure Automation-forgatókönyv, lásd: [az Azure Automation forgatókönyv- és gyűjtemények](automation-runbook-gallery.md).
* toofind más hasznos Resource Manager-sablonok, lásd: [Azure gyors üzembe helyezés sablonok](https://azure.microsoft.com/resources/templates/)

