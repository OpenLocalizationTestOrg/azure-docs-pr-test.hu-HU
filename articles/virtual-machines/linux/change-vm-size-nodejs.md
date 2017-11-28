---
title: "Linux virtuális gép és az Azure CLI 1.0 átméretezése |} Microsoft Docs"
description: "Hogyan növelheti vagy csökkentheti a Linux virtuális gépek, a Virtuálisgép-méretet módosításával."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 72f5a3cd6463befd5108040ed166984281bfc5f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a><span data-ttu-id="07357-103">Linux virtuális gép az Azure CLI 1.0 és átméretezése</span><span class="sxs-lookup"><span data-stu-id="07357-103">Resize a Linux VM with Azure CLI 1.0</span></span>

## <a name="overview"></a><span data-ttu-id="07357-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="07357-104">Overview</span></span>

<span data-ttu-id="07357-105">(VM) virtuális gép kiépítése után méretezheti a virtuális gép felfelé vagy lefelé módosításával a [Virtuálisgép-méretet][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="07357-105">After you provision a virtual machine (VM), you can scale the VM up or down by changing the [VM size][vm-sizes].</span></span> <span data-ttu-id="07357-106">Néhány esetben először a virtuális gép kell felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="07357-106">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="07357-107">Ez akkor fordulhat elő, ha új mérete nem érhető el a hardver fürtön, amelyen a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="07357-107">This can happen if the new size is not available on the hardware cluster that is hosting the VM.</span></span>

<span data-ttu-id="07357-108">Ez a cikk bemutatja, hogyan méretezze át a Linux virtuális gépet a [Azure CLI][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="07357-108">This article shows how to resize a Linux VM using the [Azure CLI][azure-cli].</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="07357-109">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="07357-109">CLI versions to complete the task</span></span>
<span data-ttu-id="07357-110">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="07357-110">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="07357-111">[Az Azure CLI 1.0](#resize-a-linux-vm) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="07357-111">[Azure CLI 1.0](#resize-a-linux-vm) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="07357-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="07357-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>


## <a name="resize-a-linux-vm"></a><span data-ttu-id="07357-113">Linux virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="07357-113">Resize a Linux VM</span></span>
<span data-ttu-id="07357-114">Átméretezni egy virtuális Géphez, hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="07357-114">To resize a VM, perform the following steps.</span></span>

1. <span data-ttu-id="07357-115">A következő parancssori parancsot.</span><span class="sxs-lookup"><span data-stu-id="07357-115">Run the following CLI command.</span></span> <span data-ttu-id="07357-116">Ez a parancs felsorolja a hardver fürt, ahol a virtuális gép tárolása elérhető Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="07357-116">This command lists the VM sizes that are available on the hardware cluster where the VM is hosted.</span></span>
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. <span data-ttu-id="07357-117">Ha a kívánt méretet, a következő parancsot a virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="07357-117">If the desired size is listed, run the following command to resize the VM.</span></span>
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    <span data-ttu-id="07357-118">A virtuális gép újraindul, a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="07357-118">The VM will restart during this process.</span></span> <span data-ttu-id="07357-119">Az újraindítás után a meglévő operációs rendszer és az adatlemezek fogja képezni.</span><span class="sxs-lookup"><span data-stu-id="07357-119">After the restart, your existing OS and data disks will be remapped.</span></span> <span data-ttu-id="07357-120">Az ideiglenes lemezen semmit elvesznek.</span><span class="sxs-lookup"><span data-stu-id="07357-120">Anything on the temporary disk will be lost.</span></span>
   
    <span data-ttu-id="07357-121">Használja a `--enable-boot-diagnostics` beállítás lehetővé teszi, hogy [rendszerindítási diagnosztika][boot-diagnostics], bejelentkezési indítási kapcsolatos hibákat.</span><span class="sxs-lookup"><span data-stu-id="07357-121">Use the `--enable-boot-diagnostics` option enables [boot diagnostics][boot-diagnostics], to log any errors related to startup.</span></span>
3. <span data-ttu-id="07357-122">Ha a kívánt méretet nem szerepel, a következő parancsokat a virtuális gép felszabadítása méretezze át, és indítsa újra a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="07357-122">Otherwise, if the desired size is not listed, run the following commands to deallocate the VM, resize it, and then restart the VM.</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="07357-123">A virtuális Géphez rendelt dinamikus IP-címek is a virtuális gép felszabadítása kiadását.</span><span class="sxs-lookup"><span data-stu-id="07357-123">Deallocating the VM also releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="07357-124">Az operációsrendszer- és adatlemezek, nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="07357-124">The OS and data disks are not affected.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="07357-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07357-125">Next steps</span></span>
<span data-ttu-id="07357-126">További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="07357-126">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="07357-127">További információkért lásd: [automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek][scale-set].</span><span class="sxs-lookup"><span data-stu-id="07357-127">For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
