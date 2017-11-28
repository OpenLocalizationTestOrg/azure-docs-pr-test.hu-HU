---
title: "A sablon az Azure erőforrás helye |} Microsoft Docs"
description: "Bemutatja, hogyan erőforrás helyének beállítása az Azure Resource Manager-sablonok"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a><span data-ttu-id="16ffc-103">Az Azure Resource Manager sablonokban erőforrás hely beállítása</span><span class="sxs-lookup"><span data-stu-id="16ffc-103">Set resource location in Azure Resource Manager templates</span></span>
<span data-ttu-id="16ffc-104">Ha a sablonok telepítésével, meg kell adnia az egyes erőforrások helyét.</span><span class="sxs-lookup"><span data-stu-id="16ffc-104">When deploying a template, you must provide a location for each resource.</span></span> <span data-ttu-id="16ffc-105">Ez a témakör azt ismerteti, hogyan határozza meg a helyek, az előfizetés minden erőforrástípus számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="16ffc-105">This topic shows how to determine the locations that are available to your subscription for each resource type.</span></span>

## <a name="determine-supported-locations"></a><span data-ttu-id="16ffc-106">Határozza meg a támogatott helyek</span><span class="sxs-lookup"><span data-stu-id="16ffc-106">Determine supported locations</span></span>

<span data-ttu-id="16ffc-107">Az egyes erőforrás támogatott helyek teljes listáját lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="16ffc-107">For a complete list of supported locations for each resource type, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="16ffc-108">Azonban az előfizetés nem tudja elérni, hogy a listában a helyekhez.</span><span class="sxs-lookup"><span data-stu-id="16ffc-108">However, your subscription might not have access to all the locations in that list.</span></span> <span data-ttu-id="16ffc-109">Az előfizetés számára elérhető helyek testreszabott listájának megjelenítéséhez használja az Azure PowerShell vagy Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="16ffc-109">To see a customized list of locations that are available to your subscription, use Azure PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="16ffc-110">Az alábbi példában PowerShell a helyek az beszerzése a `Microsoft.Web\sites` erőforrás típusa:</span><span class="sxs-lookup"><span data-stu-id="16ffc-110">The following example uses PowerShell to get the locations for the `Microsoft.Web\sites` resource type:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="16ffc-111">Az alábbi példában Azure CLI 2.0 a helyek az beszerzése a `Microsoft.Web\sites` erőforrás típusa:</span><span class="sxs-lookup"><span data-stu-id="16ffc-111">The following example uses Azure CLI 2.0 to get the locations for the `Microsoft.Web\sites` resource type:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a><span data-ttu-id="16ffc-112">Állítsa helyre a sablon</span><span class="sxs-lookup"><span data-stu-id="16ffc-112">Set location in template</span></span>

<span data-ttu-id="16ffc-113">Után annak meghatározásához, hogy az erőforrások támogatott helyeket, be kell erre a helyre a sablonban.</span><span class="sxs-lookup"><span data-stu-id="16ffc-113">After determining the supported locations for your resources, you need to set that location in your template.</span></span> <span data-ttu-id="16ffc-114">Állítsa ezt az értéket a legegyszerűbb módja, amely támogatja a erőforrástípusok helyen hozzon létre egy erőforráscsoportot, és mindegyik helyen `[resourceGroup().location]`.</span><span class="sxs-lookup"><span data-stu-id="16ffc-114">The easiest way to set this value is to create a resource group in a location that supports the resource types, and set each location to `[resourceGroup().location]`.</span></span> <span data-ttu-id="16ffc-115">A sablon különböző helyeken való újbóli, és nem módosítható a sablonból vagy a paraméterek az értékeket.</span><span class="sxs-lookup"><span data-stu-id="16ffc-115">You can redeploy the template to resource groups in different locations, and not change any values in the template or parameters.</span></span> 

<span data-ttu-id="16ffc-116">A következő példa bemutatja, amelyek az erőforráscsoportot és ugyanazon a helyen a rendszer:</span><span class="sxs-lookup"><span data-stu-id="16ffc-116">The following example shows a storage account that is deployed to the same location as the resource group:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

<span data-ttu-id="16ffc-117">Ha szeretné kódba foglalni a hely a sablonban, adja meg a támogatott régiók egyikéhez sem nevét.</span><span class="sxs-lookup"><span data-stu-id="16ffc-117">If you need to hardcode the location in your template, provide the name of one of the supported regions.</span></span> <span data-ttu-id="16ffc-118">A következő példa bemutatja egy tárfiókot, északi középső Régiójában mindig központilag telepített:</span><span class="sxs-lookup"><span data-stu-id="16ffc-118">The following example shows a storage account that is always deployed to North Central US:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a><span data-ttu-id="16ffc-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16ffc-119">Next steps</span></span>
* <span data-ttu-id="16ffc-120">Sablonok létrehozásával kapcsolatos ajánlások, lásd: [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="16ffc-120">For recommendations about how to create templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>

