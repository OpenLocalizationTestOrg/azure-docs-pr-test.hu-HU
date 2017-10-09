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
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokban erőforrás hely beállítása
Ha a sablonok telepítésével, meg kell adnia az egyes erőforrások helyét. Ez a témakör bemutatja, hogyan írja be a toodetermine hello helyeken elérhető tooyour előfizetés minden erőforráshoz.

## <a name="determine-supported-locations"></a>Határozza meg a támogatott helyek

Az egyes erőforrás támogatott helyek teljes listáját lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/). Az előfizetés azonban nem lehet tooall hello helyeivel, hogy a listában. toosee helyeken elérhető tooyour előfizetés testreszabott listájának Azure PowerShell vagy Azure CLI használata. 

hello alábbi példa használ PowerShell tooget hello helyek hello `Microsoft.Web\sites` erőforrás típusa:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

hello alábbi példa használ Azure CLI 2.0 tooget hello helyek hello `Microsoft.Web\sites` erőforrás típusa:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Állítsa helyre a sablon

Hello támogatott helyek erőforrások meghatározása, után kell tooset erre a helyre a sablonban. Ezt az értéket az egy erőforráscsoport, amely támogatja a hello erőforrástípusok helyen toocreate legegyszerűbb módja tooset hello, és állítsa be túl mindegyik helyen`[resourceGroup().location]`. Telepítse újra a hello sablon tooresource csoportok különböző helyeken, és nem módosítható a hello sablon vagy paraméterek értékeket. 

hello alábbi példa azt mutatja, amelyek a telepített toohello van ugyanazon a helyen hello erőforráscsoportként működnek:

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

Ha a sablonban kell toohardcode hello helyét, adja meg a hello nevét hello támogatott régiók egyikéhez sem. a következő példa hello jeleníti meg, amelyek mindig telepítve tooNorth USA középső RÉGIÓJA:

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

## <a name="next-steps"></a>Következő lépések
* A fenyegetés elhárítására vonatkozó javaslatokról toocreate sablonjainak használatáról [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).

