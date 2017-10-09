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
# <a name="convert-a-scale-set-template-tooa-managed-disk-scale-set-template"></a><span data-ttu-id="cb3cf-104">Alakítsa át a méretezési készlet sablon tooa felügyelt lemezes méretezési sablon beállítása</span><span class="sxs-lookup"><span data-stu-id="cb3cf-104">Convert a scale set template tooa managed disk scale set template</span></span>

<span data-ttu-id="cb3cf-105">A Resource Manager-sablon létrehozásához egy méretezési készletben, felügyelt lemezzel nem rendelkező ügyfelek toomodify előfordulhat, hogy kívánja azt toouse kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-105">Customers with a Resource Manager template for creating a scale set not using managed disk may wish toomodify it toouse managed disk.</span></span> <span data-ttu-id="cb3cf-106">Ez a cikk bemutatja, hogyan toodo ezt, használja, mint például egy lekérést a hello [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates), egy minta Resource Manager-sablonok közösségi szerkesztésű tárháza.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-106">This article shows how toodo this, using as an example a pull request from hello [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates), a community-driven repo for sample Resource Manager templates.</span></span> <span data-ttu-id="cb3cf-107">Itt látható a hello teljes lekérési kérelmet: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), és a vonatkozó részeinek hello hello különbözeti az alábbi magyarázatokat együtt:</span><span class="sxs-lookup"><span data-stu-id="cb3cf-107">hello full pull request can be seen here: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), and hello relevant parts of hello diff are below, along with explanations:</span></span>

## <a name="making-hello-os-disks-managed"></a><span data-ttu-id="cb3cf-108">Így felügyelt hello OS lemezeken</span><span class="sxs-lookup"><span data-stu-id="cb3cf-108">Making hello OS disks managed</span></span>

<span data-ttu-id="cb3cf-109">Az alábbi hello különbözeti láthatja, hogy több változók kapcsolódó toostorage fiók és a lemez tulajdonságainak eltávolítottuk.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-109">In hello diff below, we can see that we have removed several variables related toostorage account and disk properties.</span></span> <span data-ttu-id="cb3cf-110">Tárfióktípus már nincs szükség (Standard_LRS hello alapértelmezett érték), de azt továbbra is megadhatja, ha azt kíván.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-110">Storage account type is no longer necessary (Standard_LRS is hello default), but we could still specify it if we wished to.</span></span> <span data-ttu-id="cb3cf-111">Csak Standard_LRS és Premium_LRS felügyelt lemezes használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-111">Only Standard_LRS and Premium_LRS are supported with managed disk.</span></span> <span data-ttu-id="cb3cf-112">Új tárolási fiók utótag, egyedi karakterlánc-tömbben és sa száma használták hello régi sablon toogenerate tárfiókok neve.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-112">New storage account suffix, unique string array, and sa count were used in hello old template toogenerate storage account names.</span></span> <span data-ttu-id="cb3cf-113">Ezek a változók már nincs szükség a hello nevű új sablont, mert felügyelt lemezes automatikusan létrehozza a storage-fiókok hello az ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-113">These variables are no longer necessary in hello new template because managed disk automatically creates storage accounts on hello customer's behalf.</span></span> <span data-ttu-id="cb3cf-114">Ehhez hasonlóan vhd-tároló neve és az operációs rendszer lemezének neve már nincs szükség, mert a felügyelt lemezes automatikusan nevet hello alapul szolgáló tárolási blob tárolók és a lemezek.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-114">Similarly, vhd container name and os disk name are no longer necessary because managed disk automatically names hello underlying storage blob containers and disks.</span></span>

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


<span data-ttu-id="cb3cf-115">Hello különbözeti azt alább a tekintse meg, hogy hello frissítése számíthatja api verziója too2016-04-30-előzetes, ez az hello legkorábbi szükséges verziója méretezési készlet által felügyelt lemezes támogatott.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-115">In hello diff below, we can see that we updated hello compute api version too2016-04-30-preview, which is hello earliest required version for managed disk support with scale sets.</span></span> <span data-ttu-id="cb3cf-116">Vegye figyelembe, hogy sikerült továbbra is használjuk nem felügyelt lemezeket az új api verzió hello hello régi szintaxissal igény.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-116">Note that we could still use unmanaged disks in hello new api version with hello old syntax if desired.</span></span> <span data-ttu-id="cb3cf-117">Ez azt jelenti Ha csak frissítése hello számítási api-verziót, és ne módosítsa bármi más, hello sablon továbbra is mint toowork előtt.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-117">In other words, if we only update hello compute api version and don't change anything else, hello template should continue toowork as before.</span></span>

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

<span data-ttu-id="cb3cf-118">Az alábbi hello különbözeti láthatja, hogy megszüntetjük hello tárolási fiók erőforrás hello erőforrások tömbből teljesen.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-118">In hello diff below, we can see that we are removing hello storage account resource from hello resources array completely.</span></span> <span data-ttu-id="cb3cf-119">Már nem kell őket felügyelt lemezes őket automatikusan létrehozza a nevünkben óta.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-119">We no longer need them since managed disk creates them automatically on our behalf.</span></span>

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

<span data-ttu-id="cb3cf-120">A hello különbözeti alatt, azt is, hogy megszüntetjük hello lásd: függ hello méretezési készlet toohello ciklust, amely létrehozta a storage-fiókok a hivatkozó záradékban.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-120">In hello diff below, we can see that we are removing hello depends on clause referring from hello scale set toohello loop that was creating storage accounts.</span></span> <span data-ttu-id="cb3cf-121">Hello régi sablonban ezzel biztosítva, hogy a hello storage-fiókok létrejöttek-e, mielőtt hello méretezési megkezdte létrehozása, de ehhez a záradékhoz már nincs szükség a felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-121">In hello old template, this was ensuring that hello storage accounts were created before hello scale set began creation, but this clause is no longer necessary with managed disk.</span></span> <span data-ttu-id="cb3cf-122">Azt is hello vhd-tárolók tulajdonság, és az operációs rendszer lemez name tulajdonság hello, mivel ezek a tulajdonságok automatikusan kezeli a hello technikai részletek felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-122">We also remove hello vhd containers property, and hello os disk name property as these properties are automatically handled under hello hood by managed disk.</span></span> <span data-ttu-id="cb3cf-123">Ha azt igénylését, azt hozzáadhatja `"managedDisk": { "storageAccountType": "Premium_LRS" }` hello "osDisk" konfigurációban, ha a prémium szintű operációsrendszer-lemezek meg akartunk.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-123">If we wished, we could add `"managedDisk": { "storageAccountType": "Premium_LRS" }` in hello "osDisk" configuration if we wanted premium OS disks.</span></span> <span data-ttu-id="cb3cf-124">Csak virtuális gépek nagybetűt vagy kisbetűk meg "a virtuális gép hello sku premium lemezeket is használhasson.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-124">Only VMs with an uppercase or lowercase 's' in hello VM sku can use premium disks.</span></span>

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

<span data-ttu-id="cb3cf-125">Explicit tulajdonság nincs hello méretezési készlet konfigurációja toouse által felügyelt vagy nem kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-125">There is no explicit property in hello scale set configuration for whether toouse managed or unmanaged disk.</span></span> <span data-ttu-id="cb3cf-126">hello méretezési tudja, melyik toouse hello tárolási profilban található hello tulajdonságok alapján.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-126">hello scale set knows which toouse based on hello properties that are present in hello storage profile.</span></span> <span data-ttu-id="cb3cf-127">Ezért fontos hello sablon tooensure hello engedély tulajdonságai közül a hello tárolási profilban hello méretezési módosításakor.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-127">Thus, it is important when modifying hello template tooensure that hello right properties are in hello storage profile of hello scale set.</span></span>


## <a name="data-disks"></a><span data-ttu-id="cb3cf-128">Az adatlemezek</span><span class="sxs-lookup"><span data-stu-id="cb3cf-128">Data disks</span></span>

<span data-ttu-id="cb3cf-129">A fenti hello módosításainak köszönhetően hello méretezési készlet által használt felügyelt lemezeket a hello OS lemez, de mi a helyzet adatlemezek? tooadd adatlemezek, azonos szinten, mint a "osDisk" hello hozzáadása a "storageProfile" hello "dataDisks" tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-129">With hello changes above, hello scale set uses managed disks for hello OS disk, but what about data disks? tooadd data disks, add hello "dataDisks" property under "storageProfile" at hello same level as "osDisk".</span></span> <span data-ttu-id="cb3cf-130">hello hello tulajdonság értéke JSON objektumok listáját, amelyek mindegyikének Tulajdonságok "logikai" (amely a virtuális gép adatok lemezenként egyedinek kell lennie), "createOption" ("üres" értéke jelenleg hello csak a támogatott beállítás), és a "diskSizeGB" (gigabájtban hello lemez mérete hello; kell lennie nagyobb, mint 0 és kisebb, mint 1024) a következő példa hello a:</span><span class="sxs-lookup"><span data-stu-id="cb3cf-130">hello value of hello property is a JSON list of objects, each of which has properties "lun" (which must be unique per data disk on a VM), "createOption" ("empty" is currently hello only supported option), and "diskSizeGB" (hello size of hello disk in gigabytes; must be greater than 0 and less than 1024) as in hello following example:</span></span> 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

<span data-ttu-id="cb3cf-131">Ha megad `n` lemezeket a tömbben, egyes virtuális gépek méretezési hello beállítása lekérdezi `n` adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-131">If you specify `n` disks in this array, each VM in hello scale set gets `n` data disks.</span></span> <span data-ttu-id="cb3cf-132">Vegye figyelembe azonban, hogy ezek az adatlemezek-e a nyers eszközökön.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-132">Do note, however, that these data disks are raw devices.</span></span> <span data-ttu-id="cb3cf-133">Ezek nem formátumú.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-133">They are not formatted.</span></span> <span data-ttu-id="cb3cf-134">Használat előtt toohello ügyfél tooattach, paritition és hello lemezek formátumban van.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-134">It is up toohello customer tooattach, paritition, and format hello disks before using them.</span></span> <span data-ttu-id="cb3cf-135">Másik lehetőségként azt is megadhatja `"managedDisk": { "storageAccountType": "Premium_LRS" }` az egyes adatok lemez objektum toospecify, hogy legyen-e a prémium szintű adatlemez.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-135">Optionally, we could also specify `"managedDisk": { "storageAccountType": "Premium_LRS" }` in each data disk object toospecify that it should be a premium data disk.</span></span> <span data-ttu-id="cb3cf-136">Csak virtuális gépek nagybetűt vagy kisbetűk meg "a virtuális gép hello sku premium lemezeket is használhasson.</span><span class="sxs-lookup"><span data-stu-id="cb3cf-136">Only VMs with an uppercase or lowercase 's' in hello VM sku can use premium disks.</span></span>

<span data-ttu-id="cb3cf-137">További információ az adatlemezek használata méretezési csoportok toolearn lásd [Ez a cikk](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="cb3cf-137">toolearn more about using data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="cb3cf-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb3cf-138">Next steps</span></span>
<span data-ttu-id="cb3cf-139">Például Resource Manager-sablonok használatával méretezési csoportok keresendő "vmss" hello [Azure gyors üzembe helyezési sablonokat github-tárház](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="cb3cf-139">For example Resource Manager templates using scale sets, search for "vmss" in hello [Azure Quickstart Templates github repo](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="cb3cf-140">Általános információkért tekintse meg a hello [méretezési csoportok fő kezdőlapjának](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="cb3cf-140">For general information, check out hello [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

