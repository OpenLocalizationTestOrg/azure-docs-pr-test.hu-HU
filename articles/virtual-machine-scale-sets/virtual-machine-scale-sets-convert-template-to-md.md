---
title: "az Azure Resource Manager méretezési aaaConvert állítsa be a sablon toouse felügyelt lemezes |} Microsoft Docs"
description: "A skála sablon tooa felügyelt lemezes méretezési készlet sablon beállítása az átalakítás."
keywords: "Virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/18/2017
ms.author: negat
ms.openlocfilehash: 66c2217647e57ed2cfa39660c0175710ae2e63be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-scale-set-template-tooa-managed-disk-scale-set-template"></a>Alakítsa át a méretezési készlet sablon tooa felügyelt lemezes méretezési sablon beállítása

A Resource Manager-sablon létrehozásához egy méretezési készletben, felügyelt lemezzel nem rendelkező ügyfelek toomodify előfordulhat, hogy kívánja azt toouse kezelt lemezre. Ez a cikk bemutatja, hogyan toodo ezt, használja, mint például egy lekérést a hello [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates), egy minta Resource Manager-sablonok közösségi szerkesztésű tárháza. Itt látható a hello teljes lekérési kérelmet: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), és a vonatkozó részeinek hello hello különbözeti az alábbi magyarázatokat együtt:

## <a name="making-hello-os-disks-managed"></a>Így felügyelt hello OS lemezeken

Az alábbi hello különbözeti láthatja, hogy több változók kapcsolódó toostorage fiók és a lemez tulajdonságainak eltávolítottuk. Tárfióktípus már nincs szükség (Standard_LRS hello alapértelmezett érték), de azt továbbra is megadhatja, ha azt kíván. Csak Standard_LRS és Premium_LRS felügyelt lemezes használata támogatott. Új tárolási fiók utótag, egyedi karakterlánc-tömbben és sa száma használták hello régi sablon toogenerate tárfiókok neve. Ezek a változók már nincs szükség a hello nevű új sablont, mert felügyelt lemezes automatikusan létrehozza a storage-fiókok hello az ügyfél nevében. Ehhez hasonlóan vhd-tároló neve és az operációs rendszer lemezének neve már nincs szükség, mert a felügyelt lemezes automatikusan nevet hello alapul szolgáló tárolási blob tárolók és a lemezek.

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```


Hello különbözeti azt alább a tekintse meg, hogy hello frissítése számíthatja api verziója too2016-04-30-előzetes, ez az hello legkorábbi szükséges verziója méretezési készlet által felügyelt lemezes támogatott. Vegye figyelembe, hogy sikerült továbbra is használjuk nem felügyelt lemezeket az új api verzió hello hello régi szintaxissal igény. Ez azt jelenti Ha csak frissítése hello számítási api-verziót, és ne módosítsa bármi más, hello sablon továbbra is mint toowork előtt.

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

Az alábbi hello különbözeti láthatja, hogy megszüntetjük hello tárolási fiók erőforrás hello erőforrások tömbből teljesen. Már nem kell őket felügyelt lemezes őket automatikusan létrehozza a nevünkben óta.

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

A hello különbözeti alatt, azt is, hogy megszüntetjük hello lásd: függ hello méretezési készlet toohello ciklust, amely létrehozta a storage-fiókok a hivatkozó záradékban. Hello régi sablonban ezzel biztosítva, hogy a hello storage-fiókok létrejöttek-e, mielőtt hello méretezési megkezdte létrehozása, de ehhez a záradékhoz már nincs szükség a felügyelt lemezes. Azt is hello vhd-tárolók tulajdonság, és az operációs rendszer lemez name tulajdonság hello, mivel ezek a tulajdonságok automatikusan kezeli a hello technikai részletek felügyelt lemezes. Ha azt igénylését, azt hozzáadhatja `"managedDisk": { "storageAccountType": "Premium_LRS" }` hello "osDisk" konfigurációban, ha a prémium szintű operációsrendszer-lemezek meg akartunk. Csak virtuális gépek nagybetűt vagy kisbetűk meg "a virtuális gép hello sku premium lemezeket is használhasson.

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.windows.net/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },

```

Explicit tulajdonság nincs hello méretezési készlet konfigurációja toouse által felügyelt vagy nem kezelt lemezre. hello méretezési tudja, melyik toouse hello tárolási profilban található hello tulajdonságok alapján. Ezért fontos hello sablon tooensure hello engedély tulajdonságai közül a hello tárolási profilban hello méretezési módosításakor.


## <a name="data-disks"></a>Az adatlemezek

A fenti hello módosításainak köszönhetően hello méretezési készlet által használt felügyelt lemezeket a hello OS lemez, de mi a helyzet adatlemezek? tooadd adatlemezek, azonos szinten, mint a "osDisk" hello hozzáadása a "storageProfile" hello "dataDisks" tulajdonság. hello hello tulajdonság értéke JSON objektumok listáját, amelyek mindegyikének Tulajdonságok "logikai" (amely a virtuális gép adatok lemezenként egyedinek kell lennie), "createOption" ("üres" értéke jelenleg hello csak a támogatott beállítás), és a "diskSizeGB" (gigabájtban hello lemez mérete hello; kell lennie nagyobb, mint 0 és kisebb, mint 1024) a következő példa hello a: 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

Ha megad `n` lemezeket a tömbben, egyes virtuális gépek méretezési hello beállítása lekérdezi `n` adatlemezek. Vegye figyelembe azonban, hogy ezek az adatlemezek-e a nyers eszközökön. Ezek nem formátumú. Használat előtt toohello ügyfél tooattach, paritition és hello lemezek formátumban van. Másik lehetőségként azt is megadhatja `"managedDisk": { "storageAccountType": "Premium_LRS" }` az egyes adatok lemez objektum toospecify, hogy legyen-e a prémium szintű adatlemez. Csak virtuális gépek nagybetűt vagy kisbetűk meg "a virtuális gép hello sku premium lemezeket is használhasson.

További információ az adatlemezek használata méretezési csoportok toolearn lásd [Ez a cikk](./virtual-machine-scale-sets-attached-disks.md).


## <a name="next-steps"></a>Következő lépések
Például Resource Manager-sablonok használatával méretezési csoportok keresendő "vmss" hello [Azure gyors üzembe helyezési sablonokat github-tárház](https://github.com/Azure/azure-quickstart-templates).

Általános információkért tekintse meg a hello [méretezési csoportok fő kezdőlapjának](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

