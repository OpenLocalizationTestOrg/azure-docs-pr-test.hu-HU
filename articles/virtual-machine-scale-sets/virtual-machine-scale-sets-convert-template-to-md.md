---
title: "Alakítsa át az Azure Resource Manager méretezési készlet használt sablon felügyelt lemezes |} Microsoft Docs"
description: "A méretezési készlet sablon átalakítása felügyelt lemezes méretezési készlet sablont."
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
ms.openlocfilehash: 2f5cb85703888c5056611d466f508547ee72e44b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="convert-a-scale-set-template-to-a-managed-disk-scale-set-template"></a><span data-ttu-id="1b2ab-104">A méretezési készlet sablon átalakítása felügyelt lemezes méretezési sablon beállítása</span><span class="sxs-lookup"><span data-stu-id="1b2ab-104">Convert a scale set template to a managed disk scale set template</span></span>

<span data-ttu-id="1b2ab-105">A Resource Manager-sablon létrehozásához egy méretezési készlet nem használja a felügyelt lemezes rendelkező ügyfelek kíván felügyelt lemezes módosítja.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-105">Customers with a Resource Manager template for creating a scale set not using managed disk may wish to modify it to use managed disk.</span></span> <span data-ttu-id="1b2ab-106">Ez a cikk bemutatja, hogyan, ne használja, mint például a lekérési kérelmet a [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates), egy minta Resource Manager-sablonok közösségi szerkesztésű tárháza.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-106">This article shows how to do this, using as an example a pull request from the [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates), a community-driven repo for sample Resource Manager templates.</span></span> <span data-ttu-id="1b2ab-107">A teljes lekérési kérelmet itt találja: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), és a különbözeti vonatkozó részeinek az alábbi magyarázatokat együtt:</span><span class="sxs-lookup"><span data-stu-id="1b2ab-107">The full pull request can be seen here: [https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998), and the relevant parts of the diff are below, along with explanations:</span></span>

## <a name="making-the-os-disks-managed"></a><span data-ttu-id="1b2ab-108">Az operációs rendszer lemezeknek elvégzése</span><span class="sxs-lookup"><span data-stu-id="1b2ab-108">Making the OS disks managed</span></span>

<span data-ttu-id="1b2ab-109">Az alábbi különbözeti is látható, hogy több tároló lemez és a fiók tulajdonságainak kapcsolódó változók eltávolítottuk.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-109">In the diff below, we can see that we have removed several variables related to storage account and disk properties.</span></span> <span data-ttu-id="1b2ab-110">Tárfióktípus már nincs szükség (Standard_LRS érték az alapértelmezett), de azt továbbra is megadhatja, ha azt kíván.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-110">Storage account type is no longer necessary (Standard_LRS is the default), but we could still specify it if we wished to.</span></span> <span data-ttu-id="1b2ab-111">Csak Standard_LRS és Premium_LRS felügyelt lemezes használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-111">Only Standard_LRS and Premium_LRS are supported with managed disk.</span></span> <span data-ttu-id="1b2ab-112">Új tárolási fiók utótag, egyedi karakterlánc-tömbben és sa száma használták a régi sablonban a tárfiókneveket generálni.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-112">New storage account suffix, unique string array, and sa count were used in the old template to generate storage account names.</span></span> <span data-ttu-id="1b2ab-113">Ezek a változók már nincs szükség az új sablonban, mert a felügyelt lemezes automatikusan létrehozza a storage-fiókok az ügyfél nevében.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-113">These variables are no longer necessary in the new template because managed disk automatically creates storage accounts on the customer's behalf.</span></span> <span data-ttu-id="1b2ab-114">Hasonlóképpen vhd-tároló neve és az operációs rendszer lemezének neve már nincs szükség, mert a felügyelt lemezes automatikusan nevet az alapul szolgáló tárolási blob tárolók és lemezek.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-114">Similarly, vhd container name and os disk name are no longer necessary because managed disk automatically names the underlying storage blob containers and disks.</span></span>

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


<span data-ttu-id="1b2ab-115">Az alábbi különbözeti is látható, hogy a számítási frissítése api-verzió 2016-04-30-előzetes, amely a méretezési készlet által felügyelt lemezes támogatott legkorábbi szükséges verziója.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-115">In the diff below, we can see that we updated the compute api version to 2016-04-30-preview, which is the earliest required version for managed disk support with scale sets.</span></span> <span data-ttu-id="1b2ab-116">Vegye figyelembe, hogy sikerült továbbra is használjuk nem felügyelt lemezeket a régi használatával az új api-verzióban igény.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-116">Note that we could still use unmanaged disks in the new api version with the old syntax if desired.</span></span> <span data-ttu-id="1b2ab-117">Ez azt jelenti Ha a számítási csak frissítjük api-verziót, és ne módosítsa bármi más, a sablon továbbra is működnek, mint korábban.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-117">In other words, if we only update the compute api version and don't change anything else, the template should continue to work as before.</span></span>

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

<span data-ttu-id="1b2ab-118">Az alábbi különbözeti is látható, hogy megszüntetjük a tárolási fiók erőforrások az erőforrások tömbből teljesen.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-118">In the diff below, we can see that we are removing the storage account resource from the resources array completely.</span></span> <span data-ttu-id="1b2ab-119">Már nem kell őket felügyelt lemezes őket automatikusan létrehozza a nevünkben óta.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-119">We no longer need them since managed disk creates them automatically on our behalf.</span></span>

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

<span data-ttu-id="1b2ab-120">Az alábbi különbözeti is látható, hogy megszüntetjük a záradékot a méretezési készletben, amely létrehozta a storage-fiókok a hurok a hivatkozó függ.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-120">In the diff below, we can see that we are removing the depends on clause referring from the scale set to the loop that was creating storage accounts.</span></span> <span data-ttu-id="1b2ab-121">A régi sablonban ezzel biztosítva, hogy a storage-fiókok létrejöttek-e, mielőtt a méretezési megkezdte létrehozása, de ehhez a záradékhoz már nincs szükség a felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-121">In the old template, this was ensuring that the storage accounts were created before the scale set began creation, but this clause is no longer necessary with managed disk.</span></span> <span data-ttu-id="1b2ab-122">Azt is távolítsa el a vhd-tárolók tulajdonság, és az operációs rendszer lemez name tulajdonság szerint ezek a tulajdonságok automatikusan kezeli a technikai részletek a felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-122">We also remove the vhd containers property, and the os disk name property as these properties are automatically handled under the hood by managed disk.</span></span> <span data-ttu-id="1b2ab-123">Ha azt igénylését, azt hozzáadhatja `"managedDisk": { "storageAccountType": "Premium_LRS" }` a "osDisk" konfigurációban, ha a prémium szintű operációsrendszer-lemezek meg akartunk.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-123">If we wished, we could add `"managedDisk": { "storageAccountType": "Premium_LRS" }` in the "osDisk" configuration if we wanted premium OS disks.</span></span> <span data-ttu-id="1b2ab-124">Csak virtuális gépek nagybetűt vagy kisbetűk meg "a virtuális gép sku premium lemezeket is használhasson.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-124">Only VMs with an uppercase or lowercase 's' in the VM sku can use premium disks.</span></span>

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

<span data-ttu-id="1b2ab-125">Nincs explicit tulajdonság méretezési készlet konfigurációjában felügyelt vagy nem felügyelt lemezt használ-e.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-125">There is no explicit property in the scale set configuration for whether to use managed or unmanaged disk.</span></span> <span data-ttu-id="1b2ab-126">A méretezési tudja kell használni a tárolási profilban található tulajdonságok alapján.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-126">The scale set knows which to use based on the properties that are present in the storage profile.</span></span> <span data-ttu-id="1b2ab-127">Ezért fontos annak érdekében, hogy a megfelelő tulajdonságok a tárolási profilban, a méretezési sablon módosításakor.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-127">Thus, it is important when modifying the template to ensure that the right properties are in the storage profile of the scale set.</span></span>


## <a name="data-disks"></a><span data-ttu-id="1b2ab-128">Az adatlemezek</span><span class="sxs-lookup"><span data-stu-id="1b2ab-128">Data disks</span></span>

<span data-ttu-id="1b2ab-129">A fenti módosítások a méretezési készlet által használt kezelt lemez az operációs rendszer lemezre, de mi a helyzet adatlemezek?</span><span class="sxs-lookup"><span data-stu-id="1b2ab-129">With the changes above, the scale set uses managed disks for the OS disk, but what about data disks?</span></span> <span data-ttu-id="1b2ab-130">Adatok lemezt ad hozzá, vegye fel a "dataDisks" tulajdonság "storageProfile" a "osDisk" ugyanazon a szinten.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-130">To add data disks, add the "dataDisks" property under "storageProfile" at the same level as "osDisk".</span></span> <span data-ttu-id="1b2ab-131">A tulajdonság értéke JSON objektumok listáját, amelyek mindegyikének Tulajdonságok "logikai" (amely a virtuális gép adatok lemezenként egyedinek kell lennie), "createOption" ("empty" jelenleg az egyetlen támogatott beállítás), és a "diskSizeGB" (gigabájtban; a lemez mérete nagyobbnak kell lennie 0 és kisebb, mint 1024) az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="1b2ab-131">The value of the property is a JSON list of objects, each of which has properties "lun" (which must be unique per data disk on a VM), "createOption" ("empty" is currently the only supported option), and "diskSizeGB" (the size of the disk in gigabytes; must be greater than 0 and less than 1024) as in the following example:</span></span> 

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

<span data-ttu-id="1b2ab-132">Ha megad `n` a tömb lemezek, a rendszer minden virtuális gép beállítása lekérdezi `n` adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-132">If you specify `n` disks in this array, each VM in the scale set gets `n` data disks.</span></span> <span data-ttu-id="1b2ab-133">Vegye figyelembe azonban, hogy ezek az adatlemezek-e a nyers eszközökön.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-133">Do note, however, that these data disks are raw devices.</span></span> <span data-ttu-id="1b2ab-134">Ezek nem formátumú.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-134">They are not formatted.</span></span> <span data-ttu-id="1b2ab-135">Esetén a kell csatolni, paritition, és formázza a lemezeket a használatuk előtt.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-135">It is up to the customer to attach, paritition, and format the disks before using them.</span></span> <span data-ttu-id="1b2ab-136">Másik lehetőségként azt is megadhatja `"managedDisk": { "storageAccountType": "Premium_LRS" }` egyes adatok lemez objektumban adhatja meg, hogy legyen-e a prémium szintű adatlemez.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-136">Optionally, we could also specify `"managedDisk": { "storageAccountType": "Premium_LRS" }` in each data disk object to specify that it should be a premium data disk.</span></span> <span data-ttu-id="1b2ab-137">Csak virtuális gépek nagybetűt vagy kisbetűk meg "a virtuális gép sku premium lemezeket is használhasson.</span><span class="sxs-lookup"><span data-stu-id="1b2ab-137">Only VMs with an uppercase or lowercase 's' in the VM sku can use premium disks.</span></span>

<span data-ttu-id="1b2ab-138">Az adatlemezek méretezési csoportok használatával kapcsolatban további tudnivalókért lásd: [Ez a cikk](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="1b2ab-138">To learn more about using data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="1b2ab-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b2ab-139">Next steps</span></span>
<span data-ttu-id="1b2ab-140">Például Resource Manager-sablonok használatával méretezési csoportok keresendő "vmss" a [Azure gyors üzembe helyezési sablonokat github-tárház](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="1b2ab-140">For example Resource Manager templates using scale sets, search for "vmss" in the [Azure Quickstart Templates github repo](https://github.com/Azure/azure-quickstart-templates).</span></span>

<span data-ttu-id="1b2ab-141">Általános információkért tekintse meg a [méretezési csoportok fő kezdőlapjának](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="1b2ab-141">For general information, check out the [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

