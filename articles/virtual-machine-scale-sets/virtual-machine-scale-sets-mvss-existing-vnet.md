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
# <a name="add-reference-tooan-existing-virtual-network-in-an-azure-scale-set-template"></a>Hivatkozás tooan meglévő virtuális hálózat hozzáadása a sablon egy Azure méretezési beállítása

Ez a cikk bemutatja, hogyan toomodify hello [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) toodeploy létrehozni meglévő virtuális hálózatban egy új létrehozása helyett.

## <a name="change-hello-template-definition"></a>Hello sablonleírásnak módosítása

A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon telepítéséhez hello méretezési készletben létrehozni meglévő virtuális hálózatban látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json). Ez a sablon hello használt különbözeti toocreate vizsgáljuk meg (`git diff minimum-viable-scale-set existing-vnet`) adat által adat:

Első lépésként azt adja meg a `subnetId` paraméter. Ez a karakterlánc fog átadását hello méretezési készlet konfigurációja, így hello méretezési tooidentify hello előre létrehozott alhálózati toodeploy virtuális gépek be. Ez a karakterlánc hello formátumúnak kell lennie: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Például toodeploy hello méretezési létrehozni meglévő virtuális hálózatban nevű `myvnet`, alhálózati `mysubnet`, erőforráscsoport `myrg`, és az előfizetés `00000000-0000-0000-0000-000000000000`, hello subnetId lenne: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

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

A következő azt törölheti hello virtuális hálózati erőforrás hello `resources` tömbben, mert azt egy meglévő virtuális hálózatot használ, és nem szükséges toodeploy egy újat.

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

hello virtuális hálózat már létezik hello sablon telepítése előtt, így nincs szükség toospecify a dependsOn záradékot a skálától hello beállítása toohello virtuális hálózat. Ebből kifolyólag azt ezek a sorok törlése:

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

Végül azt adjon át hello `subnetId` hello felhasználó által megadott paraméter (használata helyett `resourceId` egy vnetet az azonos környezetben, amely milyen hello minimális életképes méretezési sablon does hello tooget hello azonosítója).

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




## <a name="next-steps"></a>Következő lépések

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
