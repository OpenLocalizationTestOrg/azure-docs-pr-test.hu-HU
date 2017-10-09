---
title: az Azure PowerShell aaaExport Resource Manager-sablon |} Microsoft Docs
description: "Használja az Azure Resource Manager és az Azure PowerShell tooexport egy sablon, egy erőforráscsoportot."
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
ms.openlocfilehash: 9a239b7bce8209326c0e267a4d3d69f7014bdaed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a><span data-ttu-id="88af9-103">A PowerShell-lel Azure Resource Manager-sablonok exportálása</span><span class="sxs-lookup"><span data-stu-id="88af9-103">Export Azure Resource Manager templates with PowerShell</span></span>

<span data-ttu-id="88af9-104">Erőforrás-kezelő lehetővé teszi a Resource Manager-sablon a meglévő erőforrást az előfizetésében tooexport.</span><span class="sxs-lookup"><span data-stu-id="88af9-104">Resource Manager enables you tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="88af9-105">A létrehozott sablon toolearn kapcsolatos hello sablon szintaxis vagy tooautomate hello a megoldás újbóli telepítését igény szerint is használhatja.</span><span class="sxs-lookup"><span data-stu-id="88af9-105">You can use that generated template toolearn about hello template syntax or tooautomate hello redeployment of your solution as needed.</span></span>

<span data-ttu-id="88af9-106">Fontos, hogy nincsenek két különböző módon tooexport sablon toonote:</span><span class="sxs-lookup"><span data-stu-id="88af9-106">It is important toonote that there are two different ways tooexport a template:</span></span>

* <span data-ttu-id="88af9-107">Hello tényleges sablon, amelyet a központi telepítéshez használt exportálhatja.</span><span class="sxs-lookup"><span data-stu-id="88af9-107">You can export hello actual template that you used for a deployment.</span></span> <span data-ttu-id="88af9-108">hello exportált sablon tartalmaz minden hello paraméterek és változók pontosan úgy, mint ahogy az eredeti sablon hello.</span><span class="sxs-lookup"><span data-stu-id="88af9-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="88af9-109">Ez a módszer akkor hasznos, ha egy sablon tooretrieve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="88af9-109">This approach is helpful when you need tooretrieve a template.</span></span>
* <span data-ttu-id="88af9-110">Exportálhatja a sablont, amely hello hello erőforráscsoport aktuális állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="88af9-110">You can export a template that represents hello current state of hello resource group.</span></span> <span data-ttu-id="88af9-111">hello exportált sablon nem alapul, amely a központi telepítéshez használt sablonokat.</span><span class="sxs-lookup"><span data-stu-id="88af9-111">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="88af9-112">Ehelyett az alkalmazás létrehozza a sablont, amely hello erőforráscsoportban található egy pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="88af9-112">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="88af9-113">hello exportált sablonjának sok változtatható értékek és valószínűleg nem annyi megadott paraméterek általában határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="88af9-113">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="88af9-114">Ez a módszer akkor hasznos, ha hello erőforráscsoport módosította.</span><span class="sxs-lookup"><span data-stu-id="88af9-114">This approach is useful when you have modified hello resource group.</span></span> <span data-ttu-id="88af9-115">A továbbiakban létre kell toocapture hello erőforráscsoport sablonként.</span><span class="sxs-lookup"><span data-stu-id="88af9-115">Now, you need toocapture hello resource group as a template.</span></span>

<span data-ttu-id="88af9-116">Ebben a témakörben mind a két megoldást bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="88af9-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="88af9-117">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="88af9-117">Deploy a solution</span></span>

<span data-ttu-id="88af9-118">mindkét tooillustrate megközelíti a sablon exportálása, először megoldás tooyour előfizetés telepítése.</span><span class="sxs-lookup"><span data-stu-id="88af9-118">tooillustrate both approaches for exporting a template, let's start by deploying a solution tooyour subscription.</span></span> <span data-ttu-id="88af9-119">Ha már van erőforráscsoport, amelyet az tooexport előfizetését, nincs toodeploy ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="88af9-119">If you already have a resource group in your subscription that you want tooexport, you do not have toodeploy this solution.</span></span> <span data-ttu-id="88af9-120">Azonban hello a cikk hátralévő része toohello sablon ehhez a megoldáshoz hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="88af9-120">However, hello remainder of this article refers toohello template for this solution.</span></span> <span data-ttu-id="88af9-121">hello a példaként megadott parancsfájlt a storage-fiók telepíti.</span><span class="sxs-lookup"><span data-stu-id="88af9-121">hello example script deploys a storage account.</span></span>

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="88af9-122">Menti a sablont a központi telepítés előzményei</span><span class="sxs-lookup"><span data-stu-id="88af9-122">Save template from deployment history</span></span>

<span data-ttu-id="88af9-123">Beolvasható egy sablont az üzembe helyezési előzményeket hello segítségével [mentés-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) parancsot.</span><span class="sxs-lookup"><span data-stu-id="88af9-123">You can retrieve a template from your deployment history by using hello [Save-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) command.</span></span> <span data-ttu-id="88af9-124">a következő példa menti hello sablont, amely korábban központilag hello:</span><span class="sxs-lookup"><span data-stu-id="88af9-124">hello following example saves hello template that you previously deploy:</span></span>

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

<span data-ttu-id="88af9-125">Hello sablon hello helyét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="88af9-125">It returns hello location of hello template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

<span data-ttu-id="88af9-126">Nyissa meg hello fájlt, és figyelje meg, hogy a rendszer hello pontos sablon központi telepítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="88af9-126">Open hello file, and notice that it is hello exact template you used for deployment.</span></span> <span data-ttu-id="88af9-127">hello paraméterek és változók megfelelő hello sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="88af9-127">hello parameters and variables match hello template from GitHub.</span></span> <span data-ttu-id="88af9-128">Ez a sablon központilag telepítheti.</span><span class="sxs-lookup"><span data-stu-id="88af9-128">You can redeploy this template.</span></span>

## <a name="export-resource-group-as-template"></a><span data-ttu-id="88af9-129">Erőforráscsoportok exportálása sablonként</span><span class="sxs-lookup"><span data-stu-id="88af9-129">Export resource group as template</span></span>

<span data-ttu-id="88af9-130">Egy sablon fogadása hello üzembe helyezési előzményeket, helyett a sablont, amely hello az erőforráscsoport aktuális állapotát jeleníti meg hello segítségével kérheti le [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) parancsot.</span><span class="sxs-lookup"><span data-stu-id="88af9-130">Instead of retrieving a template from hello deployment history, you can retrieve a template that represents hello current state of a resource group by using hello [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) command.</span></span> <span data-ttu-id="88af9-131">Ez a parancs használni, amikor sok módosítások tooyour erőforráscsoport el, és nincs meglévő sablon változtatásainak hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="88af9-131">You use this command when you have made many changes tooyour resource group and no existing template represents all hello changes.</span></span>

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

<span data-ttu-id="88af9-132">Hello sablon hello helyét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="88af9-132">It returns hello location of hello template.</span></span>

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

<span data-ttu-id="88af9-133">Nyissa meg hello fájlt, és figyelje meg, hogy nem egyezik a Githubon hello sablont.</span><span class="sxs-lookup"><span data-stu-id="88af9-133">Open hello file, and notice that it is different than hello template in GitHub.</span></span> <span data-ttu-id="88af9-134">Különböző paraméterek és változók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="88af9-134">It has different parameters and no variables.</span></span> <span data-ttu-id="88af9-135">hello tárolási SKU és hely érték is kódolt toovalues.</span><span class="sxs-lookup"><span data-stu-id="88af9-135">hello storage SKU and location are hard-coded toovalues.</span></span> <span data-ttu-id="88af9-136">hello alábbi példa bemutatja hello exportált sablon, de a sablon némileg eltérő névvel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="88af9-136">hello following example shows hello exported template, but your template has a slightly different parameter name:</span></span>

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

<span data-ttu-id="88af9-137">Ez a sablon központilag telepítheti, de egy egyedi nevet a tárfiók hello találgatás igényel.</span><span class="sxs-lookup"><span data-stu-id="88af9-137">You can redeploy this template, but it requires guessing a unique name for hello storage account.</span></span> <span data-ttu-id="88af9-138">a paraméter neve hello kis mértékben eltér.</span><span class="sxs-lookup"><span data-stu-id="88af9-138">hello name of your parameter is slightly different.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a><span data-ttu-id="88af9-139">Exportált sablon személyre szabása</span><span class="sxs-lookup"><span data-stu-id="88af9-139">Customize exported template</span></span>

<span data-ttu-id="88af9-140">Módosíthatja a sablon toomake azt toouse egyszerűbb és rugalmasabb.</span><span class="sxs-lookup"><span data-stu-id="88af9-140">You can modify this template toomake it easier toouse and more flexible.</span></span> <span data-ttu-id="88af9-141">további helyeket, a módosítás hello hely tulajdonság toouse tooallow hello hello erőforráscsoport és ugyanazon a helyen:</span><span class="sxs-lookup"><span data-stu-id="88af9-141">tooallow for more locations, change hello location property toouse hello same location as hello resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="88af9-142">tooavoid rendelkező tooguess tárfiókot, távolítsa el a hello paraméter hello a tárfiók nevének uniques nevét.</span><span class="sxs-lookup"><span data-stu-id="88af9-142">tooavoid having tooguess a uniques name for storage account, remove hello parameter for hello storage account name.</span></span> <span data-ttu-id="88af9-143">Tárolási névutótag, és egy tárolási SKU paraméter hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="88af9-143">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="88af9-144">Adja hozzá egy változó, amely hello tárfióknév hello uniqueString függvénnyel hoz létre:</span><span class="sxs-lookup"><span data-stu-id="88af9-144">Add a variable that constructs hello storage account name with hello uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="88af9-145">Hello hello tárolási fiók toohello változó nevének megadása:</span><span class="sxs-lookup"><span data-stu-id="88af9-145">Set hello name of hello storage account toohello variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="88af9-146">Hello SKU toohello paraméter beállítása:</span><span class="sxs-lookup"><span data-stu-id="88af9-146">Set hello SKU toohello parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="88af9-147">A sablon most a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="88af9-147">Your template now looks like:</span></span>

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

<span data-ttu-id="88af9-148">Hello módosított sablon újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="88af9-148">Redeploy hello modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="88af9-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88af9-149">Next steps</span></span>
* <span data-ttu-id="88af9-150">Hello portál tooexport egy sablon használatával kapcsolatos információkért lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="88af9-150">For information about using hello portal tooexport a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="88af9-151">a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="88af9-151">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="88af9-152">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="88af9-152">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
