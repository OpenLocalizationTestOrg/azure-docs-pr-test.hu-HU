---
title: "Egy már meglévő virtuális hálózatot az Azure méretezési készlet sablonban hivatkozhat |} Microsoft Docs"
description: "Útmutató: virtuális hálózat hozzáadása egy meglévő Azure virtuálisgép-méretezési csoport sablon"
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
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a><span data-ttu-id="6517d-103">Adjon hozzá egy meglévő virtuális hálózathoz való hivatkozást Azure méretezési készlet sablonban</span><span class="sxs-lookup"><span data-stu-id="6517d-103">Add reference to an existing virtual network in an Azure scale set template</span></span>

<span data-ttu-id="6517d-104">Ez a cikk bemutatja, hogyan lehet módosítani a [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) való üzembe helyezés helyett egy új létrehozása meglévő virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="6517d-104">This article shows how to modify the [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) to deploy into an existing virtual network instead of creating a new one.</span></span>

## <a name="change-the-template-definition"></a><span data-ttu-id="6517d-105">Módosítsa a sablon-definíciót</span><span class="sxs-lookup"><span data-stu-id="6517d-105">Change the template definition</span></span>

<span data-ttu-id="6517d-106">A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon a méretezési készletben meglévő virtuális hálózatban való üzembe helyezésének látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="6517d-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying the scale set into an existing virtual network can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json).</span></span> <span data-ttu-id="6517d-107">Ez a sablon létrehozásához használt különbözeti vizsgáljuk meg (`git diff minimum-viable-scale-set existing-vnet`) adat által adat:</span><span class="sxs-lookup"><span data-stu-id="6517d-107">Let's examine the diff used to create this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="6517d-108">Első lépésként azt adja meg a `subnetId` paraméter.</span><span class="sxs-lookup"><span data-stu-id="6517d-108">First, we add a `subnetId` parameter.</span></span> <span data-ttu-id="6517d-109">Ez a karakterlánc továbbítódnak a méretezési készlet konfigurációja, amely lehetővé teszi a méretezés, a virtuális gépek telepítéséhez a korábban létrehozott alhálózati azonosító állíthat be.</span><span class="sxs-lookup"><span data-stu-id="6517d-109">This string will be passed into the scale set configuration, allowing the scale set to identify the pre-created subnet to deploy virtual machines into.</span></span> <span data-ttu-id="6517d-110">Ez a karakterlánc a következő formátumban kell lennie: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span><span class="sxs-lookup"><span data-stu-id="6517d-110">This string must be of the form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`.</span></span> <span data-ttu-id="6517d-111">Például a skála telepítendő beállítani egy meglévő virtuális hálózat névvel `myvnet`, alhálózati `mysubnet`, erőforráscsoport `myrg`, és az előfizetés `00000000-0000-0000-0000-000000000000`, a subnetId lenne: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span><span class="sxs-lookup"><span data-stu-id="6517d-111">For instance, to deploy the scale set into an existing virtual network with name `myvnet`, subnet `mysubnet`, resource group `myrg`, and subscription `00000000-0000-0000-0000-000000000000`, the subnetId would be: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.</span></span>

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

<span data-ttu-id="6517d-112">Ezt követően a virtuális hálózati erőforrás törölheti azt a `resources` tömbben, mert azt egy meglévő virtuális hálózatot használ, és nem szükséges telepítenie egy új.</span><span class="sxs-lookup"><span data-stu-id="6517d-112">Next, we can delete the virtual network resource from the `resources` array, since we are using an existing virtual network and don't need to deploy a new one.</span></span>

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

<span data-ttu-id="6517d-113">A virtuális hálózat már létezik a sablon telepítése előtt, nincs szükség a dependsOn záradékot a méretezési készletben, a virtuális hálózathoz meg.</span><span class="sxs-lookup"><span data-stu-id="6517d-113">The virtual network already exists before the template is deployed, so there is no need to specify a dependsOn clause from the scale set to the virtual network.</span></span> <span data-ttu-id="6517d-114">Ebből kifolyólag azt ezek a sorok törlése:</span><span class="sxs-lookup"><span data-stu-id="6517d-114">Thus, we delete these lines:</span></span>

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

<span data-ttu-id="6517d-115">Végül azt adjon át a `subnetId` a felhasználó által megadott paraméter (használata helyett `resourceId` ahhoz, hogy a virtuális hálózat azonosítóját azonos környezetben, amely, mi a minimális életképes méretezési sablon does).</span><span class="sxs-lookup"><span data-stu-id="6517d-115">Finally, we pass in the `subnetId` parameter set by the user (instead of using `resourceId` to get the id of a vnet in the same deployment, which is what the minimum viable scale set template does).</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="6517d-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6517d-116">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
