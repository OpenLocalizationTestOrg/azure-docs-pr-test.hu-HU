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
# <a name="add-a-custom-image-tooan-azure-scale-set-template"></a><span data-ttu-id="f725f-103">Egy egyéni lemezkép tooan Azure méretezési állítsa be a sablon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f725f-103">Add a custom image tooan Azure scale set template</span></span>

<span data-ttu-id="f725f-104">Ez a cikk bemutatja, hogyan toomodify hello [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) toodeploy egyéni lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="f725f-104">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toodeploy from custom image.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="f725f-105">Hello sablonleírásnak módosítása</span><span class="sxs-lookup"><span data-stu-id="f725f-105">Change hello template definition</span></span>

<span data-ttu-id="f725f-106">A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon telepítéséhez hello méretezési készletben egyéni lemezképéről látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="f725f-106">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello scale set from a custom image can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json).</span></span> <span data-ttu-id="f725f-107">Ez a sablon hello használt különbözeti toocreate vizsgáljuk meg (`git diff minimum-viable-scale-set custom-image`) adat által adat:</span><span class="sxs-lookup"><span data-stu-id="f725f-107">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set custom-image`) piece by piece:</span></span>

### <a name="creating-a-managed-disk-image"></a><span data-ttu-id="f725f-108">Felügyelt lemezes lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f725f-108">Creating a managed disk image</span></span>

<span data-ttu-id="f725f-109">Ha már van egy egyéni felügyelt lemezképet (típusú erőforrást `Microsoft.Compute/images`), akkor kihagyhatja ezt a részt.</span><span class="sxs-lookup"><span data-stu-id="f725f-109">If you already have a custom managed disk image (a resource of type `Microsoft.Compute/images`), then you can skip this section.</span></span>

<span data-ttu-id="f725f-110">Első lépésként azt adja meg a `sourceImageVhdUri` hello URI általánosítva toohello BLOB az Azure Storage hello egyéni lemezkép toodeploy a tartalmazó paramétert.</span><span class="sxs-lookup"><span data-stu-id="f725f-110">First, we add a `sourceImageVhdUri` parameter, which is hello URI toohello generalized blob in Azure Storage that contains hello custom image toodeploy from.</span></span>


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

<span data-ttu-id="f725f-111">Ezután adja hozzá azt az típusú erőforrást `Microsoft.Compute/images`, amely felügyelt hello lemezképét alapuló általánosítva hello blob URI-címen található `sourceImageVhdUri`.</span><span class="sxs-lookup"><span data-stu-id="f725f-111">Next, we add a resource of type `Microsoft.Compute/images`, which is hello managed disk image based on hello generalized blob located at URI `sourceImageVhdUri`.</span></span> <span data-ttu-id="f725f-112">Ez a rendszerkép hello kell lennie, amely használja ezt a hello méretezési és ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="f725f-112">This image must be in hello same region as hello scale set that uses it.</span></span> <span data-ttu-id="f725f-113">Hello lemezkép hello tulajdonságaiban adtuk meg hello az operációs rendszer típusa, hello blob hello helyét (a hello `sourceImageVhdUri` paraméter), és a tárfiók típusa hello:</span><span class="sxs-lookup"><span data-stu-id="f725f-113">In hello properties of hello image, we specify hello OS type, hello location of hello blob (from hello `sourceImageVhdUri` parameter), and hello storage account type:</span></span>

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

<span data-ttu-id="f725f-114">A hello méretezési erőforrás, jelenleg felvenni egy `dependsOn` toohello egyéni lemezkép toomake meg arról, hogy hello kép végrehajtásakor létrejön, mielőtt hello méretezési megpróbál toodeploy erről a képről utaló záradékban:</span><span class="sxs-lookup"><span data-stu-id="f725f-114">In hello scale set resource, we add a `dependsOn` clause referring toohello custom image toomake sure hello image gets created before hello scale set tries toodeploy from that image:</span></span>

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

### <a name="changing-scale-set-properties-toouse-hello-managed-disk-image"></a><span data-ttu-id="f725f-115">Skála módosítása csoportosító tulajdonságok toouse hello felügyelt lemezes képe</span><span class="sxs-lookup"><span data-stu-id="f725f-115">Changing scale set properties toouse hello managed disk image</span></span>

<span data-ttu-id="f725f-116">A hello `imageReference` hello skála beállítása `storageProfile`, hello közzétevő, az ajánlat, sku és platformlemezkép verziójának megadása, helyett adtuk meg hello `id` a hello `Microsoft.Compute/images` erőforrás:</span><span class="sxs-lookup"><span data-stu-id="f725f-116">In hello `imageReference` of hello scale set `storageProfile`, instead of specifying hello publisher, offer, sku, and version of a platform image, we specify hello `id` of hello `Microsoft.Compute/images` resource:</span></span>

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

<span data-ttu-id="f725f-117">A jelen példában használjuk hello `resourceId` függvény tooget hello erőforrás-azonosító hello lemezkép létrehozott hello azonos sablont.</span><span class="sxs-lookup"><span data-stu-id="f725f-117">In this example, we use hello `resourceId` function tooget hello resource ID of hello image created in hello same template.</span></span> <span data-ttu-id="f725f-118">Ha előzetesen létrehozta hello felügyelt lemezképet, hello azonosító lemezkép ehelyett kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="f725f-118">If you have created hello managed disk image beforehand, you should provide hello id of that image instead.</span></span> <span data-ttu-id="f725f-119">Ezt az azonosítót hello formátumúnak kell lennie: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span><span class="sxs-lookup"><span data-stu-id="f725f-119">This id must be of hello form: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f725f-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f725f-120">Next Steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
