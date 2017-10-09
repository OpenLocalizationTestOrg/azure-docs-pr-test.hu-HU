---
title: "a sablon aaaAzure erőforrás helye |} Microsoft Docs"
description: "Bemutatja, hogyan tooset egy Azure Resource Manager sablon egy erőforrás helye"
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
ms.openlocfilehash: f2ad6ca6ac5f34484a2e5e57dd8d67c77dacc41a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a><span data-ttu-id="94231-103">Az Azure Resource Manager sablonokban erőforrás hely beállítása</span><span class="sxs-lookup"><span data-stu-id="94231-103">Set resource location in Azure Resource Manager templates</span></span>
<span data-ttu-id="94231-104">Ha a sablonok telepítésével, meg kell adnia az egyes erőforrások helyét.</span><span class="sxs-lookup"><span data-stu-id="94231-104">When deploying a template, you must provide a location for each resource.</span></span> <span data-ttu-id="94231-105">Ez a témakör bemutatja, hogyan írja be a toodetermine hello helyeken elérhető tooyour előfizetés minden erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="94231-105">This topic shows how toodetermine hello locations that are available tooyour subscription for each resource type.</span></span>

## <a name="determine-supported-locations"></a><span data-ttu-id="94231-106">Határozza meg a támogatott helyek</span><span class="sxs-lookup"><span data-stu-id="94231-106">Determine supported locations</span></span>

<span data-ttu-id="94231-107">Az egyes erőforrás támogatott helyek teljes listáját lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="94231-107">For a complete list of supported locations for each resource type, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="94231-108">Az előfizetés azonban nem lehet tooall hello helyeivel, hogy a listában.</span><span class="sxs-lookup"><span data-stu-id="94231-108">However, your subscription might not have access tooall hello locations in that list.</span></span> <span data-ttu-id="94231-109">toosee helyeken elérhető tooyour előfizetés testreszabott listájának Azure PowerShell vagy Azure CLI használata.</span><span class="sxs-lookup"><span data-stu-id="94231-109">toosee a customized list of locations that are available tooyour subscription, use Azure PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="94231-110">hello alábbi példa használ PowerShell tooget hello helyek hello `Microsoft.Web\sites` erőforrás típusa:</span><span class="sxs-lookup"><span data-stu-id="94231-110">hello following example uses PowerShell tooget hello locations for hello `Microsoft.Web\sites` resource type:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="94231-111">hello alábbi példa használ Azure CLI 2.0 tooget hello helyek hello `Microsoft.Web\sites` erőforrás típusa:</span><span class="sxs-lookup"><span data-stu-id="94231-111">hello following example uses Azure CLI 2.0 tooget hello locations for hello `Microsoft.Web\sites` resource type:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a><span data-ttu-id="94231-112">Állítsa helyre a sablon</span><span class="sxs-lookup"><span data-stu-id="94231-112">Set location in template</span></span>

<span data-ttu-id="94231-113">Hello támogatott helyek erőforrások meghatározása, után kell tooset erre a helyre a sablonban.</span><span class="sxs-lookup"><span data-stu-id="94231-113">After determining hello supported locations for your resources, you need tooset that location in your template.</span></span> <span data-ttu-id="94231-114">Ezt az értéket az egy erőforráscsoport, amely támogatja a hello erőforrástípusok helyen toocreate legegyszerűbb módja tooset hello, és állítsa be túl mindegyik helyen`[resourceGroup().location]`.</span><span class="sxs-lookup"><span data-stu-id="94231-114">hello easiest way tooset this value is toocreate a resource group in a location that supports hello resource types, and set each location too`[resourceGroup().location]`.</span></span> <span data-ttu-id="94231-115">Telepítse újra a hello sablon tooresource csoportok különböző helyeken, és nem módosítható a hello sablon vagy paraméterek értékeket.</span><span class="sxs-lookup"><span data-stu-id="94231-115">You can redeploy hello template tooresource groups in different locations, and not change any values in hello template or parameters.</span></span> 

<span data-ttu-id="94231-116">hello alábbi példa azt mutatja, amelyek a telepített toohello van ugyanazon a helyen hello erőforráscsoportként működnek:</span><span class="sxs-lookup"><span data-stu-id="94231-116">hello following example shows a storage account that is deployed toohello same location as hello resource group:</span></span>

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

<span data-ttu-id="94231-117">Ha a sablonban kell toohardcode hello helyét, adja meg a hello nevét hello támogatott régiók egyikéhez sem.</span><span class="sxs-lookup"><span data-stu-id="94231-117">If you need toohardcode hello location in your template, provide hello name of one of hello supported regions.</span></span> <span data-ttu-id="94231-118">a következő példa hello jeleníti meg, amelyek mindig telepítve tooNorth USA középső RÉGIÓJA:</span><span class="sxs-lookup"><span data-stu-id="94231-118">hello following example shows a storage account that is always deployed tooNorth Central US:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="94231-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94231-119">Next steps</span></span>
* <span data-ttu-id="94231-120">A fenyegetés elhárítására vonatkozó javaslatokról toocreate sablonjainak használatáról [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="94231-120">For recommendations about how toocreate templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>

