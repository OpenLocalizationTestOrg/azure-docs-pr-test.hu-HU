---
title: "aaaExport Resource Manager-sablon Azure parancssori felülettel |} Microsoft Docs"
description: "Használja az Azure Resource Manager és az Azure parancssori felület tooexport egy sablon, egy erőforráscsoportot."
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
ms.openlocfilehash: 2d44a0a6e9717504d4c2a01254d826679b381f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a><span data-ttu-id="37315-103">Az Azure CLI Azure Resource Manager-sablonok exportálása</span><span class="sxs-lookup"><span data-stu-id="37315-103">Export Azure Resource Manager templates with Azure CLI</span></span>

<span data-ttu-id="37315-104">Erőforrás-kezelő lehetővé teszi a Resource Manager-sablon a meglévő erőforrást az előfizetésében tooexport.</span><span class="sxs-lookup"><span data-stu-id="37315-104">Resource Manager enables you tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="37315-105">A létrehozott sablon toolearn kapcsolatos hello sablon szintaxis vagy tooautomate hello a megoldás újbóli telepítését igény szerint is használhatja.</span><span class="sxs-lookup"><span data-stu-id="37315-105">You can use that generated template toolearn about hello template syntax or tooautomate hello redeployment of your solution as needed.</span></span>

<span data-ttu-id="37315-106">Fontos, hogy nincsenek két különböző módon tooexport sablon toonote:</span><span class="sxs-lookup"><span data-stu-id="37315-106">It is important toonote that there are two different ways tooexport a template:</span></span>

* <span data-ttu-id="37315-107">Hello tényleges sablon, amelyet a központi telepítéshez használt exportálhatja.</span><span class="sxs-lookup"><span data-stu-id="37315-107">You can export hello actual template that you used for a deployment.</span></span> <span data-ttu-id="37315-108">hello exportált sablon tartalmaz minden hello paraméterek és változók pontosan úgy, mint ahogy az eredeti sablon hello.</span><span class="sxs-lookup"><span data-stu-id="37315-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="37315-109">Ez a módszer akkor hasznos, ha egy sablon tooretrieve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="37315-109">This approach is helpful when you need tooretrieve a template.</span></span>
* <span data-ttu-id="37315-110">Exportálhatja a sablont, amely hello hello erőforráscsoport aktuális állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="37315-110">You can export a template that represents hello current state of hello resource group.</span></span> <span data-ttu-id="37315-111">hello exportált sablon nem alapul, amely a központi telepítéshez használt sablonokat.</span><span class="sxs-lookup"><span data-stu-id="37315-111">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="37315-112">Ehelyett az alkalmazás létrehozza a sablont, amely hello erőforráscsoportban található egy pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="37315-112">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="37315-113">hello exportált sablonjának sok változtatható értékek és valószínűleg nem annyi megadott paraméterek általában határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="37315-113">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="37315-114">Ez a módszer akkor hasznos, ha hello erőforráscsoport módosította.</span><span class="sxs-lookup"><span data-stu-id="37315-114">This approach is useful when you have modified hello resource group.</span></span> <span data-ttu-id="37315-115">A továbbiakban létre kell toocapture hello erőforráscsoport sablonként.</span><span class="sxs-lookup"><span data-stu-id="37315-115">Now, you need toocapture hello resource group as a template.</span></span>

<span data-ttu-id="37315-116">Ebben a témakörben mind a két megoldást bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="37315-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="37315-117">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="37315-117">Deploy a solution</span></span>

<span data-ttu-id="37315-118">mindkét tooillustrate megközelíti a sablon exportálása, először megoldás tooyour előfizetés telepítése.</span><span class="sxs-lookup"><span data-stu-id="37315-118">tooillustrate both approaches for exporting a template, let's start by deploying a solution tooyour subscription.</span></span> <span data-ttu-id="37315-119">Ha már van erőforráscsoport, amelyet az tooexport előfizetését, nincs toodeploy ebben a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="37315-119">If you already have a resource group in your subscription that you want tooexport, you do not have toodeploy this solution.</span></span> <span data-ttu-id="37315-120">Azonban hello a cikk hátralévő része toohello sablon ehhez a megoldáshoz hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="37315-120">However, hello remainder of this article refers toohello template for this solution.</span></span> <span data-ttu-id="37315-121">hello a példaként megadott parancsfájlt a storage-fiók telepíti.</span><span class="sxs-lookup"><span data-stu-id="37315-121">hello example script deploys a storage account.</span></span>

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="37315-122">Menti a sablont a központi telepítés előzményei</span><span class="sxs-lookup"><span data-stu-id="37315-122">Save template from deployment history</span></span>

<span data-ttu-id="37315-123">Beolvasható egy sablont az üzembe helyezési előzményeket hello segítségével [az telepítési exportálása](/cli/azure/group/deployment#export) parancsot.</span><span class="sxs-lookup"><span data-stu-id="37315-123">You can retrieve a template from your deployment history by using hello [az group deployment export](/cli/azure/group/deployment#export) command.</span></span> <span data-ttu-id="37315-124">a következő példa menti hello sablont, amely korábban központilag hello:</span><span class="sxs-lookup"><span data-stu-id="37315-124">hello following example saves hello template that you previously deploy:</span></span>

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

<span data-ttu-id="37315-125">Hello sablon adja vissza.</span><span class="sxs-lookup"><span data-stu-id="37315-125">It returns hello template.</span></span> <span data-ttu-id="37315-126">Hello JSON másolja, és menteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="37315-126">Copy hello JSON, and save as a file.</span></span> <span data-ttu-id="37315-127">Figyelje meg, hogy a rendszer hello pontos sablon központi telepítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="37315-127">Notice that it is hello exact template you used for deployment.</span></span> <span data-ttu-id="37315-128">hello paraméterek és változók megfelelő hello sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="37315-128">hello parameters and variables match hello template from GitHub.</span></span> <span data-ttu-id="37315-129">Ez a sablon központilag telepítheti.</span><span class="sxs-lookup"><span data-stu-id="37315-129">You can redeploy this template.</span></span>


## <a name="export-resource-group-as-template"></a><span data-ttu-id="37315-130">Erőforráscsoportok exportálása sablonként</span><span class="sxs-lookup"><span data-stu-id="37315-130">Export resource group as template</span></span>

<span data-ttu-id="37315-131">Egy sablon fogadása hello üzembe helyezési előzményeket, helyett a sablont, amely hello az erőforráscsoport aktuális állapotát jeleníti meg hello segítségével kérheti le [az exportálása](/cli/azure/group#export) parancsot.</span><span class="sxs-lookup"><span data-stu-id="37315-131">Instead of retrieving a template from hello deployment history, you can retrieve a template that represents hello current state of a resource group by using hello [az group export](/cli/azure/group#export) command.</span></span> <span data-ttu-id="37315-132">Ez a parancs használni, amikor sok módosítások tooyour erőforráscsoport el, és nincs meglévő sablon változtatásainak hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="37315-132">You use this command when you have made many changes tooyour resource group and no existing template represents all hello changes.</span></span>

```azurecli
az group export --name ExampleGroup
```

<span data-ttu-id="37315-133">Hello sablon adja vissza.</span><span class="sxs-lookup"><span data-stu-id="37315-133">It returns hello template.</span></span> <span data-ttu-id="37315-134">Hello JSON másolja, és menteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="37315-134">Copy hello JSON, and save as a file.</span></span> <span data-ttu-id="37315-135">Figyelje meg, hogy nem egyezik a Githubon hello sablont.</span><span class="sxs-lookup"><span data-stu-id="37315-135">Notice that it is different than hello template in GitHub.</span></span> <span data-ttu-id="37315-136">Különböző paraméterek és változók nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="37315-136">It has different parameters and no variables.</span></span> <span data-ttu-id="37315-137">hello tárolási SKU és hely érték is kódolt toovalues.</span><span class="sxs-lookup"><span data-stu-id="37315-137">hello storage SKU and location are hard-coded toovalues.</span></span> <span data-ttu-id="37315-138">hello alábbi példa bemutatja hello exportált sablon, de a sablon némileg eltérő névvel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="37315-138">hello following example shows hello exported template, but your template has a slightly different parameter name:</span></span>

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

<span data-ttu-id="37315-139">Ez a sablon központilag telepítheti, de egy egyedi nevet a tárfiók hello találgatás igényel.</span><span class="sxs-lookup"><span data-stu-id="37315-139">You can redeploy this template, but it requires guessing a unique name for hello storage account.</span></span> <span data-ttu-id="37315-140">a paraméter neve hello kis mértékben eltér.</span><span class="sxs-lookup"><span data-stu-id="37315-140">hello name of your parameter is slightly different.</span></span>

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a><span data-ttu-id="37315-141">Exportált sablon személyre szabása</span><span class="sxs-lookup"><span data-stu-id="37315-141">Customize exported template</span></span>

<span data-ttu-id="37315-142">Módosíthatja a sablon toomake azt toouse egyszerűbb és rugalmasabb.</span><span class="sxs-lookup"><span data-stu-id="37315-142">You can modify this template toomake it easier toouse and more flexible.</span></span> <span data-ttu-id="37315-143">további helyeket, a módosítás hello hely tulajdonság toouse tooallow hello hello erőforráscsoport és ugyanazon a helyen:</span><span class="sxs-lookup"><span data-stu-id="37315-143">tooallow for more locations, change hello location property toouse hello same location as hello resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="37315-144">tooavoid rendelkező tooguess tárfiókot, távolítsa el a hello paraméter hello a tárfiók nevének uniques nevét.</span><span class="sxs-lookup"><span data-stu-id="37315-144">tooavoid having tooguess a uniques name for storage account, remove hello parameter for hello storage account name.</span></span> <span data-ttu-id="37315-145">Tárolási névutótag, és egy tárolási SKU paraméter hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="37315-145">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="37315-146">Adja hozzá egy változó, amely hello tárfióknév hello uniqueString függvénnyel hoz létre:</span><span class="sxs-lookup"><span data-stu-id="37315-146">Add a variable that constructs hello storage account name with hello uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="37315-147">Hello hello tárolási fiók toohello változó nevének megadása:</span><span class="sxs-lookup"><span data-stu-id="37315-147">Set hello name of hello storage account toohello variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="37315-148">Hello SKU toohello paraméter beállítása:</span><span class="sxs-lookup"><span data-stu-id="37315-148">Set hello SKU toohello parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="37315-149">A sablon most a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="37315-149">Your template now looks like:</span></span>

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

<span data-ttu-id="37315-150">Hello módosított sablon újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="37315-150">Redeploy hello modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37315-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37315-151">Next steps</span></span>
* <span data-ttu-id="37315-152">Hello portál tooexport egy sablon használatával kapcsolatos információkért lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="37315-152">For information about using hello portal tooexport a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="37315-153">a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="37315-153">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="37315-154">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="37315-154">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>