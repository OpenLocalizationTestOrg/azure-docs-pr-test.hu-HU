---
title: "Egyéni lemezképként az Azure méretezési készlet sablonná hivatkozhat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd egyéni rendszerképet tooan meglévő Azure virtuálisgép-méretezési csoport sablon"
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
ms.date: 5/10/2017
ms.author: negat
ms.openlocfilehash: 6a17d989e44d241b460238c0106350c3ef038e56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-custom-image-tooan-azure-scale-set-template"></a>Egy egyéni lemezkép tooan Azure méretezési állítsa be a sablon hozzáadása

Ez a cikk bemutatja, hogyan toomodify hello [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) toodeploy egyéni lemezképéről.

## <a name="change-hello-template-definition"></a>Hello sablonleírásnak módosítása

A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon telepítéséhez hello méretezési készletben egyéni lemezképéről látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json). Ez a sablon hello használt különbözeti toocreate vizsgáljuk meg (`git diff minimum-viable-scale-set custom-image`) adat által adat:

### <a name="creating-a-managed-disk-image"></a>Felügyelt lemezes lemezkép létrehozása

Ha már van egy egyéni felügyelt lemezképet (típusú erőforrást `Microsoft.Compute/images`), akkor kihagyhatja ezt a részt.

Első lépésként azt adja meg a `sourceImageVhdUri` hello URI általánosítva toohello BLOB az Azure Storage hello egyéni lemezkép toodeploy a tartalmazó paramétert.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "hello source of hello generalized blob containing hello custom image"
+      }
     }
   },
   "variables": {},
```

Ezután adja hozzá azt az típusú erőforrást `Microsoft.Compute/images`, amely felügyelt hello lemezképét alapuló általánosítva hello blob URI-címen található `sourceImageVhdUri`. Ez a rendszerkép hello kell lennie, amely használja ezt a hello méretezési és ugyanabban a régióban. Hello lemezkép hello tulajdonságaiban adtuk meg hello az operációs rendszer típusa, hello blob hello helyét (a hello `sourceImageVhdUri` paraméter), és a tárfiók típusa hello:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

A hello méretezési erőforrás, jelenleg felvenni egy `dependsOn` toohello egyéni lemezkép toomake meg arról, hogy hello kép végrehajtásakor létrejön, mielőtt hello méretezési megpróbál toodeploy erről a képről utaló záradékban:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-toouse-hello-managed-disk-image"></a>Skála módosítása csoportosító tulajdonságok toouse hello felügyelt lemezes képe

A hello `imageReference` hello skála beállítása `storageProfile`, hello közzétevő, az ajánlat, sku és platformlemezkép verziójának megadása, helyett adtuk meg hello `id` a hello `Microsoft.Compute/images` erőforrás:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

A jelen példában használjuk hello `resourceId` függvény tooget hello erőforrás-azonosító hello lemezkép létrehozott hello azonos sablont. Ha előzetesen létrehozta hello felügyelt lemezképet, hello azonosító lemezkép ehelyett kell megadnia. Ezt az azonosítót hello formátumúnak kell lennie: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Következő lépések

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
