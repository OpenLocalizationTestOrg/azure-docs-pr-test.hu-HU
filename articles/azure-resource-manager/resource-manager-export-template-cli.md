---
title: "Azure parancssori felülettel Resource Manager sablon exportálása |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület használatával sablonokat exportálhat egy erőforráscsoportot."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 617664129a5353e25da1e90c742c4b009db172ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a><span data-ttu-id="165c6-103">Az Azure CLI Azure Resource Manager-sablonok exportálása</span><span class="sxs-lookup"><span data-stu-id="165c6-103">Export Azure Resource Manager templates with Azure CLI</span></span>

<span data-ttu-id="165c6-104">A Resource Manager lehetővé teszi az előfizetéshez tartozó meglévő erőforrások Resource Manager-sablonjainak exportálását.</span><span class="sxs-lookup"><span data-stu-id="165c6-104">Resource Manager enables you to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="165c6-105">Az így létrehozott sablon használatával megismerheti a sablonok szintaxisát, illetve igény szerint automatizálhatja a megoldás újbóli telepítését.</span><span class="sxs-lookup"><span data-stu-id="165c6-105">You can use that generated template to learn about the template syntax or to automate the redeployment of your solution as needed.</span></span>

<span data-ttu-id="165c6-106">Fontos megjegyezni, hogy sablonokat két különböző módon lehet exportálni:</span><span class="sxs-lookup"><span data-stu-id="165c6-106">It is important to note that there are two different ways to export a template:</span></span>

* <span data-ttu-id="165c6-107">Exportálhatja az üzembe helyezéshez is használt tényleges sablont.</span><span class="sxs-lookup"><span data-stu-id="165c6-107">You can export the actual template that you used for a deployment.</span></span> <span data-ttu-id="165c6-108">Ebben az esetben az exportált sablon pontosan úgy tartalmazza a különböző paramétereket és változókat, ahogy azok az eredeti sablonban szerepeltek.</span><span class="sxs-lookup"><span data-stu-id="165c6-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="165c6-109">Ez a módszer akkor hasznos, ha egy sablon visszakeresését.</span><span class="sxs-lookup"><span data-stu-id="165c6-109">This approach is helpful when you need to retrieve a template.</span></span>
* <span data-ttu-id="165c6-110">A másik megoldás, hogy úgy exportálja a sablont, hogy az az erőforráscsoport aktuális állapotát tükrözze.</span><span class="sxs-lookup"><span data-stu-id="165c6-110">You can export a template that represents the current state of the resource group.</span></span> <span data-ttu-id="165c6-111">Ebben az esetben az exportált sablon nem az üzembe helyezéshez használt sablonon alapul.</span><span class="sxs-lookup"><span data-stu-id="165c6-111">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="165c6-112">A rendszer ehelyett új sablont hoz létre az erőforráscsoport aktuális állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="165c6-112">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="165c6-113">Az exportált sablon számos nem módosítható értéket tartalmaz, és valószínűleg kevesebb paraméter található benne, mint amennyit általában használni szokott.</span><span class="sxs-lookup"><span data-stu-id="165c6-113">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="165c6-114">Ez a módszer akkor hasznos, ha módosította az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="165c6-114">This approach is useful when you have modified the resource group.</span></span> <span data-ttu-id="165c6-115">és most szeretne létrehozni egy sablont az így létrejött egyedi erőforráscsoport alapján.</span><span class="sxs-lookup"><span data-stu-id="165c6-115">Now, you need to capture the resource group as a template.</span></span>

<span data-ttu-id="165c6-116">Ebben a témakörben mind a két megoldást bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="165c6-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="165c6-117">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="165c6-117">Deploy a solution</span></span>

<span data-ttu-id="165c6-118">A sablon exportálása mindkét megközelítés mutatja be, először egy megoldás telepítésére az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="165c6-118">To illustrate both approaches for exporting a template, let's start by deploying a solution to your subscription.</span></span> <span data-ttu-id="165c6-119">Ha már rendelkezik egy erőforráscsoportot az előfizetés az exportálni kívánt, nem rendelkeznek a megoldás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="165c6-119">If you already have a resource group in your subscription that you want to export, you do not have to deploy this solution.</span></span> <span data-ttu-id="165c6-120">Ez a cikk fennmaradó azonban ez a megoldás sablonját hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="165c6-120">However, the remainder of this article refers to the template for this solution.</span></span> <span data-ttu-id="165c6-121">A példaként megadott parancsfájlt a storage-fiók telepíti.</span><span class="sxs-lookup"><span data-stu-id="165c6-121">The example script deploys a storage account.</span></span>

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="165c6-122">Menti a sablont a központi telepítés előzményei</span><span class="sxs-lookup"><span data-stu-id="165c6-122">Save template from deployment history</span></span>

<span data-ttu-id="165c6-123">Beolvasható egy sablont az üzembe helyezési előzményeket használva a [az telepítési exportálása](/cli/azure/group/deployment#export) parancsot.</span><span class="sxs-lookup"><span data-stu-id="165c6-123">You can retrieve a template from your deployment history by using the [az group deployment export](/cli/azure/group/deployment#export) command.</span></span> <span data-ttu-id="165c6-124">Az alábbi példa menti a sablont, amely korábban központilag:</span><span class="sxs-lookup"><span data-stu-id="165c6-124">The following example saves the template that you previously deploy:</span></span>

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

<span data-ttu-id="165c6-125">A sablon adja vissza.</span><span class="sxs-lookup"><span data-stu-id="165c6-125">It returns the template.</span></span> <span data-ttu-id="165c6-126">Másolja át a JSON, és menteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="165c6-126">Copy the JSON, and save as a file.</span></span> <span data-ttu-id="165c6-127">Figyelje meg, hogy a rendszer a központi telepítéshez használt pontos sablont.</span><span class="sxs-lookup"><span data-stu-id="165c6-127">Notice that it is the exact template you used for deployment.</span></span> <span data-ttu-id="165c6-128">A paraméterek és változók felel meg a sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="165c6-128">The parameters and variables match the template from GitHub.</span></span> <span data-ttu-id="165c6-129">Ez a sablon központilag telepítheti.</span><span class="sxs-lookup"><span data-stu-id="165c6-129">You can redeploy this template.</span></span>


## <a name="export-resource-group-as-template"></a><span data-ttu-id="165c6-130">Erőforráscsoportok exportálása sablonként</span><span class="sxs-lookup"><span data-stu-id="165c6-130">Export resource group as template</span></span>

<span data-ttu-id="165c6-131">Helyett egy sablon fogadása az üzembe helyezési előzményeket, a sablont, amely az erőforráscsoport aktuális állapotát jeleníti meg használatával kérheti le a [az exportálása](/cli/azure/group#export) parancsot.</span><span class="sxs-lookup"><span data-stu-id="165c6-131">Instead of retrieving a template from the deployment history, you can retrieve a template that represents the current state of a resource group by using the [az group export](/cli/azure/group#export) command.</span></span> <span data-ttu-id="165c6-132">Ez a parancs használni, amikor sok módosításokat végzett az erőforráscsoporton, és nincs meglévő sablon jelenti. a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="165c6-132">You use this command when you have made many changes to your resource group and no existing template represents all the changes.</span></span>

```azurecli
az group export --name ExampleGroup
```

<span data-ttu-id="165c6-133">A sablon adja vissza.</span><span class="sxs-lookup"><span data-stu-id="165c6-133">It returns the template.</span></span> <span data-ttu-id="165c6-134">Másolja át a JSON, és menteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="165c6-134">Copy the JSON, and save as a file.</span></span> <span data-ttu-id="165c6-135">Figyelje meg, hogy nem egyezik a GitHub-sablon.</span><span class="sxs-lookup"><span data-stu-id="165c6-135">Notice that it is different than the template in GitHub.</span></span> <span data-ttu-id="165c6-136">Különböző paraméterek és változók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="165c6-136">It has different parameters and no variables.</span></span> <span data-ttu-id="165c6-137">A tárolási SKU és hely érték nem módosítható értékeket is.</span><span class="sxs-lookup"><span data-stu-id="165c6-137">The storage SKU and location are hard-coded to values.</span></span> <span data-ttu-id="165c6-138">A következő példa bemutatja az exportált sablon, de a sablon némileg eltérő névvel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="165c6-138">The following example shows the exported template, but your template has a slightly different parameter name:</span></span>

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

<span data-ttu-id="165c6-139">Ez a sablon központilag telepítheti, de egy egyedi nevet a tárfiók találgatás igényel.</span><span class="sxs-lookup"><span data-stu-id="165c6-139">You can redeploy this template, but it requires guessing a unique name for the storage account.</span></span> <span data-ttu-id="165c6-140">A paraméter neve kis mértékben eltér.</span><span class="sxs-lookup"><span data-stu-id="165c6-140">The name of your parameter is slightly different.</span></span>

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a><span data-ttu-id="165c6-141">Exportált sablon személyre szabása</span><span class="sxs-lookup"><span data-stu-id="165c6-141">Customize exported template</span></span>

<span data-ttu-id="165c6-142">Ez a sablon használatával egyszerűbbé és rugalmasabbá módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="165c6-142">You can modify this template to make it easier to use and more flexible.</span></span> <span data-ttu-id="165c6-143">Engedélyezi a további helyeket, módosítsa a helyet jelölő tulajdonsághoz használandó ugyanazon a helyen az erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="165c6-143">To allow for more locations, change the location property to use the same location as the resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="165c6-144">Ne kelljen kitalálni a tárfiók uniques nevét, távolítsa el a paramétert a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="165c6-144">To avoid having to guess a uniques name for storage account, remove the parameter for the storage account name.</span></span> <span data-ttu-id="165c6-145">Tárolási névutótag, és egy tárolási SKU paraméter hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="165c6-145">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="165c6-146">Adja hozzá egy változó, amely a tárfiók nevét a uniqueString függvénnyel hoz létre:</span><span class="sxs-lookup"><span data-stu-id="165c6-146">Add a variable that constructs the storage account name with the uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="165c6-147">A változó értéke a tárfiók nevét:</span><span class="sxs-lookup"><span data-stu-id="165c6-147">Set the name of the storage account to the variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="165c6-148">A paraméter értéke a Termékváltozat:</span><span class="sxs-lookup"><span data-stu-id="165c6-148">Set the SKU to the parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="165c6-149">A sablon most a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="165c6-149">Your template now looks like:</span></span>

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

<span data-ttu-id="165c6-150">A módosított sablon újbóli.</span><span class="sxs-lookup"><span data-stu-id="165c6-150">Redeploy the modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="165c6-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="165c6-151">Next steps</span></span>
* <span data-ttu-id="165c6-152">Sablon exportálása a portál használatával kapcsolatos információkért lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="165c6-152">For information about using the portal to export a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="165c6-153">Sablon paraméterek megadásához tekintse meg a [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="165c6-153">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="165c6-154">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="165c6-154">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>