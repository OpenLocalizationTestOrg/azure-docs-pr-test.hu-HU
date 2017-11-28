---
title: "aaaUsing felügyelt lemezt az Azure virtuális gép méretezési csoportok |} Microsoft Docs"
description: "Ismerje meg, miért és hogyan toouse kezeli-e a lemezek, amelyek a virtuálisgép-méretezési csoportok"
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
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 0e2a21e9f8b114ae1c8b81e1e6124621366f5643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a><span data-ttu-id="61fab-103">Azure-beli virtuálisgép-méretezési csoportok és felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="61fab-103">Azure VM scale sets and managed disks</span></span>

<span data-ttu-id="61fab-104">Az Azure-beli [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/) támogatják a felügyelt lemezekkel rendelkező virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="61fab-104">Azure [virtual machine scale sets](/azure/virtual-machine-scale-sets/) supports virtual machines with managed disks.</span></span> <span data-ttu-id="61fab-105">A felügyelt lemezek méretezési csoportokkal való használatának több előnye van, például:</span><span class="sxs-lookup"><span data-stu-id="61fab-105">Using managed disks with scale sets has several benefits, including:</span></span>

* <span data-ttu-id="61fab-106">Már nincs szüksége toopre-létrehozásához és kezeléséhez fiókok – toostore hello OS tárolólemezek hello méretezési virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="61fab-106">You no longer need toopre-create and manage storage accounts toostore hello OS disks for hello scale set VMs.</span></span>

* <span data-ttu-id="61fab-107">Felügyelt adatok lemezek toohello méretezési csatolható.</span><span class="sxs-lookup"><span data-stu-id="61fab-107">You can attach managed data disks toohello scale set.</span></span>

* <span data-ttu-id="61fab-108">Felügyelt lemezzel a méretezési csoportok kapacitása akár platformlemezkép-alapú 1000 vagy 100 egyéni lemezképen alapuló virtuális gép lehet.</span><span class="sxs-lookup"><span data-stu-id="61fab-108">With managed disk, a scale set can have capacity as high as 1,000 VMs if based on a platform image or 100 VMs if based on a custom image.</span></span>

## <a name="get-started"></a><span data-ttu-id="61fab-109">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="61fab-109">Get started</span></span>

<span data-ttu-id="61fab-110">Egyszerűen felügyelt lemezes méretezési csoportok használatába tooget toodeploy a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="61fab-110">A simple way tooget started with managed disk scale sets is toodeploy one from hello Azure portal.</span></span> <span data-ttu-id="61fab-111">További információkért tekintse meg [ezt a cikket](./virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="61fab-111">For more information, see [this article](./virtual-machine-scale-sets-portal-create.md).</span></span> <span data-ttu-id="61fab-112">Egy másik egyszerűen lépések tooget toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy egy méretezési csoportban.</span><span class="sxs-lookup"><span data-stu-id="61fab-112">Another simple way tooget started is toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy a scale set.</span></span> <span data-ttu-id="61fab-113">hello következő példa bemutatja, hogyan toocreate egy Ubuntu alapuló méretezési készletben 10 olyan 50 GB-os és 100 GB-os adatokat tartalmazó virtuális:</span><span class="sxs-lookup"><span data-stu-id="61fab-113">hello following example shows how toocreate an Ubuntu based scale set with 10 VMs, each with a 50-GB and 100-GB data disk:</span></span>

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

<span data-ttu-id="61fab-114">Azt is megteheti, hogy hello hely sikerült [Azure gyors üzembe helyezési sablonokat GitHub-tárház](https://github.com/Azure/azure-quickstart-templates) tartalmazó mappák `vmss` toosee előzetesen elkészített példák a sablonok, amelyek telepítése a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="61fab-114">Alternatively, you could look in hello [Azure Quickstart Templates GitHub repo](https://github.com/Azure/azure-quickstart-templates) for folders that contain `vmss` toosee pre-built examples of templates that deploy scale sets.</span></span> <span data-ttu-id="61fab-115">mely sablonok már használja a felügyelt lemezek tootell, olvassa el a túl[ebben a listában](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span><span class="sxs-lookup"><span data-stu-id="61fab-115">tootell which templates are already using managed disks, you can refer too[this list](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="61fab-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61fab-116">Next steps</span></span>

<span data-ttu-id="61fab-117">A felügyelt lemezekkel kapcsolatban [ebben a cikkben](../virtual-machines/windows/managed-disks-overview.md) talál további információt.</span><span class="sxs-lookup"><span data-stu-id="61fab-117">For more information on managed disks in general, see [this article](../virtual-machines/windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="61fab-118">toosee hogyan egy Resource Manager sablon tooprovision skálázási készletekben rendelkező tooconvert által kezelt lemezeken, lásd: [Ez a cikk](./virtual-machine-scale-sets-convert-template-to-md.md).</span><span class="sxs-lookup"><span data-stu-id="61fab-118">toosee how tooconvert a Resource Manager template tooprovision scale sets with managed disks, see [this article](./virtual-machine-scale-sets-convert-template-to-md.md).</span></span> <span data-ttu-id="61fab-119">hello azonos módosítások toohello Resource Manager-sablonok alkalmazása toohello Azure REST API-t is.</span><span class="sxs-lookup"><span data-stu-id="61fab-119">hello same modifications toohello Resource Manager templates apply toohello Azure REST API as well.</span></span>

<span data-ttu-id="61fab-120">toolearn több felügyelt adatlemezek méretezési csoportok használatáról lásd: [Ez a cikk](./virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="61fab-120">toolearn more about using managed data disks with scale sets, see [this article](./virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="61fab-121">túl nagy méretű méretezési csoportok használata toobegin tekintse meg[Ez a cikk](./virtual-machine-scale-sets-placement-groups.md).</span><span class="sxs-lookup"><span data-stu-id="61fab-121">toobegin working with large scale sets, refer too[this article](./virtual-machine-scale-sets-placement-groups.md).</span></span>


