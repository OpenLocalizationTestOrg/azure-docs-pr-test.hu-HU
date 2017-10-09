---
title: "Egy már meglévő virtuális hálózatot az Azure méretezési készlet sablonban hivatkozhat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd a virtuális hálózati tooan meglévő Azure virtuálisgép-méretezési csoport sablon"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: negat
ms.openlocfilehash: c3034b577e17abc4643dc26d7c38ad643fa26322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-reference-tooan-existing-virtual-network-in-an-azure-scale-set-template"></a><span data-ttu-id="0990e-103">Hivatkozás tooan meglévő virtuális hálózat hozzáadása a sablon egy Azure méretezési beállítása</span><span class="sxs-lookup"><span data-stu-id="0990e-103">Add reference tooan existing virtual network in an Azure scale set template</span></span>

<span data-ttu-id="0990e-104">Ez a cikk bemutatja, hogyan toomodify hello [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) toodeploy létrehozni meglévő virtuális hálózatban egy új létrehozása helyett.</span><span class="sxs-lookup"><span data-stu-id="0990e-104">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toodeploy into an existing virtual network instead of creating a new one.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="0990e-105">Hello sablonleírásnak módosítása</span><span class="sxs-lookup"><span data-stu-id="0990e-105">Change hello template definition</span></span>

<span data-ttu-id="0990e-106">A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon telepítéséhez hello méretezési készletben létrehozni meglévő virtuális hálózatban látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="0990e-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello scale set into an existing virtual network can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span></span> <span data-ttu-id="0990e-107">Ez a sablon hello használt különbözeti toocreate vizsgáljuk meg (`git diff minimum-viable-scale-set existing-vnet`) adat által adat:</span><span class="sxs-lookup"><span data-stu-id="0990e-107">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="0990e-108">Első lépésként azt adja meg a `subnetId` paraméter.</span><span class="sxs-lookup"><span data-stu-id="0990e-108">First, we add a `subnetId` parameter.</span></span> <span data-ttu-id="0990e-109">Ez a karakterlánc fog átadását hello méretezési készlet konfigurációja, így hello méretezési tooidentify hello előre létrehozott alhálózati toodeploy virtuális gépek be.</span><span class="sxs-lookup"><span data-stu-id="0990e-109">This string will be passed into hello scale set configuration, allowing hello scale set tooidentify hello pre-created subnet toodeploy virtual machines into.</span></span> <span data-ttu-id="0990e-110">Ez a karakterlánc hello formátumúnak kell lennie: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span><span class="sxs-lookup"><span data-stu-id="0990e-110">This string must be of hello form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span></span> <span data-ttu-id="0990e-111">Például toodeploy hello méretezési létrehozni meglévő virtuális hálózatban nevű `myvnet`, alhálózati `mysubnet`, erőforráscsoport `myrg`, és az előfizetés `00000000-0000-0000-0000-000000000000`, hello subnetId lenne: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span><span class="sxs-lookup"><span data-stu-id="0990e-111">For instance, toodeploy hello scale set into an existing virtual network with name `myvnet`, subnet `mysubnet`, resource group `myrg`, and subscription `00000000-0000-0000-0000-000000000000`, hello subnetId would be: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span></span>

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

<span data-ttu-id="0990e-112">A következő azt törölheti hello virtuális hálózati erőforrás hello `resources` tömbben, mert azt egy meglévő virtuális hálózatot használ, és nem szükséges toodeploy egy újat.</span><span class="sxs-lookup"><span data-stu-id="0990e-112">Next, we can delete hello virtual network resource from hello `resources` array, since we are using an existing virtual network and don't need toodeploy a new one.</span></span>

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

<span data-ttu-id="0990e-113">hello virtuális hálózat már létezik hello sablon telepítése előtt, így nincs szükség toospecify a dependsOn záradékot a skálától hello beállítása toohello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="0990e-113">hello virtual network already exists before hello template is deployed, so there is no need toospecify a dependsOn clause from hello scale set toohello virtual network.</span></span> <span data-ttu-id="0990e-114">Ebből kifolyólag azt ezek a sorok törlése:</span><span class="sxs-lookup"><span data-stu-id="0990e-114">Thus, we delete these lines:</span></span>

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

<span data-ttu-id="0990e-115">Végül azt adjon át hello `subnetId` hello felhasználó által megadott paraméter (használata helyett `resourceId` egy vnetet az azonos környezetben, amely milyen hello minimális életképes méretezési sablon does hello tooget hello azonosítója).</span><span class="sxs-lookup"><span data-stu-id="0990e-115">Finally, we pass in hello `subnetId` parameter set by hello user (instead of using `resourceId` tooget hello id of a vnet in hello same deployment, which is what hello minimum viable scale set template does).</span></span>

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a><span data-ttu-id="0990e-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0990e-116">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
