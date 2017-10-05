---
title: "Az Azure PowerShell Resource Manager sablon exportálása |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell használatával sablonokat exportálhat egy erőforráscsoportot."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 7543811eb9448222b6e7c266756e68debc7d54be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a><span data-ttu-id="e5c75-103">A PowerShell-lel Azure Resource Manager-sablonok exportálása</span><span class="sxs-lookup"><span data-stu-id="e5c75-103">Export Azure Resource Manager templates with PowerShell</span></span>

<span data-ttu-id="e5c75-104">A Resource Manager lehetővé teszi az előfizetéshez tartozó meglévő erőforrások Resource Manager-sablonjainak exportálását.</span><span class="sxs-lookup"><span data-stu-id="e5c75-104">Resource Manager enables you to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="e5c75-105">Az így létrehozott sablon használatával megismerheti a sablonok szintaxisát, illetve igény szerint automatizálhatja a megoldás újbóli telepítését.</span><span class="sxs-lookup"><span data-stu-id="e5c75-105">You can use that generated template to learn about the template syntax or to automate the redeployment of your solution as needed.</span></span>

<span data-ttu-id="e5c75-106">Fontos megjegyezni, hogy sablonokat két különböző módon lehet exportálni:</span><span class="sxs-lookup"><span data-stu-id="e5c75-106">It is important to note that there are two different ways to export a template:</span></span>

* <span data-ttu-id="e5c75-107">Exportálhatja az üzembe helyezéshez is használt tényleges sablont.</span><span class="sxs-lookup"><span data-stu-id="e5c75-107">You can export the actual template that you used for a deployment.</span></span> <span data-ttu-id="e5c75-108">Ebben az esetben az exportált sablon pontosan úgy tartalmazza a különböző paramétereket és változókat, ahogy azok az eredeti sablonban szerepeltek.</span><span class="sxs-lookup"><span data-stu-id="e5c75-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="e5c75-109">Ez a módszer akkor hasznos, ha egy sablon visszakeresését.</span><span class="sxs-lookup"><span data-stu-id="e5c75-109">This approach is helpful when you need to retrieve a template.</span></span>
* <span data-ttu-id="e5c75-110">A másik megoldás, hogy úgy exportálja a sablont, hogy az az erőforráscsoport aktuális állapotát tükrözze.</span><span class="sxs-lookup"><span data-stu-id="e5c75-110">You can export a template that represents the current state of the resource group.</span></span> <span data-ttu-id="e5c75-111">Ebben az esetben az exportált sablon nem az üzembe helyezéshez használt sablonon alapul.</span><span class="sxs-lookup"><span data-stu-id="e5c75-111">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="e5c75-112">A rendszer ehelyett új sablont hoz létre az erőforráscsoport aktuális állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="e5c75-112">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="e5c75-113">Az exportált sablon számos nem módosítható értéket tartalmaz, és valószínűleg kevesebb paraméter található benne, mint amennyit általában használni szokott.</span><span class="sxs-lookup"><span data-stu-id="e5c75-113">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="e5c75-114">Ez a módszer akkor hasznos, ha módosította az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e5c75-114">This approach is useful when you have modified the resource group.</span></span> <span data-ttu-id="e5c75-115">és most szeretne létrehozni egy sablont az így létrejött egyedi erőforráscsoport alapján.</span><span class="sxs-lookup"><span data-stu-id="e5c75-115">Now, you need to capture the resource group as a template.</span></span>

<span data-ttu-id="e5c75-116">Ebben a témakörben mind a két megoldást bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="e5c75-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="e5c75-117">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="e5c75-117">Deploy a solution</span></span>

<span data-ttu-id="e5c75-118">A sablon exportálása mindkét megközelítés mutatja be, először egy megoldás telepítésére az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="e5c75-118">To illustrate both approaches for exporting a template, let's start by deploying a solution to your subscription.</span></span> <span data-ttu-id="e5c75-119">Ha már rendelkezik egy erőforráscsoportot az előfizetés az exportálni kívánt, nem rendelkeznek a megoldás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="e5c75-119">If you already have a resource group in your subscription that you want to export, you do not have to deploy this solution.</span></span> <span data-ttu-id="e5c75-120">Ez a cikk fennmaradó azonban ez a megoldás sablonját hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e5c75-120">However, the remainder of this article refers to the template for this solution.</span></span> <span data-ttu-id="e5c75-121">A példaként megadott parancsfájlt a storage-fiók telepíti.</span><span class="sxs-lookup"><span data-stu-id="e5c75-121">The example script deploys a storage account.</span></span>

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="e5c75-122">Menti a sablont a központi telepítés előzményei</span><span class="sxs-lookup"><span data-stu-id="e5c75-122">Save template from deployment history</span></span>

<span data-ttu-id="e5c75-123">Beolvasható egy sablont az üzembe helyezési előzményeket használva a [mentés-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e5c75-123">You can retrieve a template from your deployment history by using the [Save-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) command.</span></span> <span data-ttu-id="e5c75-124">Az alábbi példa menti a sablont, amely korábban központilag:</span><span class="sxs-lookup"><span data-stu-id="e5c75-124">The following example saves the template that you previously deploy:</span></span>

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

<span data-ttu-id="e5c75-125">A sablon elérési útját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e5c75-125">It returns the location of the template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

<span data-ttu-id="e5c75-126">Nyissa meg a fájlt, és figyelje meg, hogy a rendszer a központi telepítéshez használt pontos sablont.</span><span class="sxs-lookup"><span data-stu-id="e5c75-126">Open the file, and notice that it is the exact template you used for deployment.</span></span> <span data-ttu-id="e5c75-127">A paraméterek és változók felel meg a sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="e5c75-127">The parameters and variables match the template from GitHub.</span></span> <span data-ttu-id="e5c75-128">Ez a sablon központilag telepítheti.</span><span class="sxs-lookup"><span data-stu-id="e5c75-128">You can redeploy this template.</span></span>

## <a name="export-resource-group-as-template"></a><span data-ttu-id="e5c75-129">Erőforráscsoportok exportálása sablonként</span><span class="sxs-lookup"><span data-stu-id="e5c75-129">Export resource group as template</span></span>

<span data-ttu-id="e5c75-130">Helyett egy sablon fogadása az üzembe helyezési előzményeket, a sablont, amely az erőforráscsoport aktuális állapotát jeleníti meg használatával kérheti le a [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e5c75-130">Instead of retrieving a template from the deployment history, you can retrieve a template that represents the current state of a resource group by using the [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) command.</span></span> <span data-ttu-id="e5c75-131">Ez a parancs használni, amikor sok módosításokat végzett az erőforráscsoporton, és nincs meglévő sablon jelenti. a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e5c75-131">You use this command when you have made many changes to your resource group and no existing template represents all the changes.</span></span>

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

<span data-ttu-id="e5c75-132">A sablon elérési útját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e5c75-132">It returns the location of the template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

<span data-ttu-id="e5c75-133">Nyissa meg a fájlt, és figyelje meg, hogy nem egyezik a GitHub-sablon.</span><span class="sxs-lookup"><span data-stu-id="e5c75-133">Open the file, and notice that it is different than the template in GitHub.</span></span> <span data-ttu-id="e5c75-134">Különböző paraméterek és változók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e5c75-134">It has different parameters and no variables.</span></span> <span data-ttu-id="e5c75-135">A tárolási SKU és hely érték nem módosítható értékeket is.</span><span class="sxs-lookup"><span data-stu-id="e5c75-135">The storage SKU and location are hard-coded to values.</span></span> <span data-ttu-id="e5c75-136">A következő példa bemutatja az exportált sablon, de a sablon némileg eltérő névvel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e5c75-136">The following example shows the exported template, but your template has a slightly different parameter name:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

<span data-ttu-id="e5c75-137">Ez a sablon központilag telepítheti, de egy egyedi nevet a tárfiók találgatás igényel.</span><span class="sxs-lookup"><span data-stu-id="e5c75-137">You can redeploy this template, but it requires guessing a unique name for the storage account.</span></span> <span data-ttu-id="e5c75-138">A paraméter neve kis mértékben eltér.</span><span class="sxs-lookup"><span data-stu-id="e5c75-138">The name of your parameter is slightly different.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a><span data-ttu-id="e5c75-139">Exportált sablon személyre szabása</span><span class="sxs-lookup"><span data-stu-id="e5c75-139">Customize exported template</span></span>

<span data-ttu-id="e5c75-140">Ez a sablon használatával egyszerűbbé és rugalmasabbá módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="e5c75-140">You can modify this template to make it easier to use and more flexible.</span></span> <span data-ttu-id="e5c75-141">Engedélyezi a további helyeket, módosítsa a helyet jelölő tulajdonsághoz használandó ugyanazon a helyen az erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="e5c75-141">To allow for more locations, change the location property to use the same location as the resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="e5c75-142">Ne kelljen kitalálni a tárfiók uniques nevét, távolítsa el a paramétert a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e5c75-142">To avoid having to guess a uniques name for storage account, remove the parameter for the storage account name.</span></span> <span data-ttu-id="e5c75-143">Tárolási névutótag, és egy tárolási SKU paraméter hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="e5c75-143">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

<span data-ttu-id="e5c75-144">Adja hozzá egy változó, amely a tárfiók nevét a uniqueString függvénnyel hoz létre:</span><span class="sxs-lookup"><span data-stu-id="e5c75-144">Add a variable that constructs the storage account name with the uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="e5c75-145">A változó értéke a tárfiók nevét:</span><span class="sxs-lookup"><span data-stu-id="e5c75-145">Set the name of the storage account to the variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="e5c75-146">A paraméter értéke a Termékváltozat:</span><span class="sxs-lookup"><span data-stu-id="e5c75-146">Set the SKU to the parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="e5c75-147">A sablon most a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="e5c75-147">Your template now looks like:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

<span data-ttu-id="e5c75-148">A módosított sablon újbóli.</span><span class="sxs-lookup"><span data-stu-id="e5c75-148">Redeploy the modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5c75-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5c75-149">Next steps</span></span>
* <span data-ttu-id="e5c75-150">Sablon exportálása a portál használatával kapcsolatos információkért lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="e5c75-150">For information about using the portal to export a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="e5c75-151">Sablon paraméterek megadásához tekintse meg a [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="e5c75-151">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="e5c75-152">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e5c75-152">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
