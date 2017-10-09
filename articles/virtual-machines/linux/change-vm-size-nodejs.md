---
title: "Linux virtuális gép hello Azure CLI 1.0 és aaaHow tooresize |} Microsoft Docs"
description: "Hogyan tooscale fel vagy le egy Linux virtuális gépet, módosításával méretezési hello Virtuálisgép-méretet."
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
ms.openlocfilehash: 43dd955dc2f2dd9d1b2da07ecbfbf2459bcaa4d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a><span data-ttu-id="6c1a9-103">Linux virtuális gép az Azure CLI 1.0 és átméretezése</span><span class="sxs-lookup"><span data-stu-id="6c1a9-103">Resize a Linux VM with Azure CLI 1.0</span></span>

## <a name="overview"></a><span data-ttu-id="6c1a9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6c1a9-104">Overview</span></span>

<span data-ttu-id="6c1a9-105">(VM) virtuális gép kiépítése után méretezheti hello VM felfelé vagy lefelé hello módosításával [Virtuálisgép-méretet][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="6c1a9-105">After you provision a virtual machine (VM), you can scale hello VM up or down by changing hello [VM size][vm-sizes].</span></span> <span data-ttu-id="6c1a9-106">Néhány esetben először hello VM kell felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-106">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="6c1a9-107">Ez akkor fordulhat elő, ha hello új mérete nem érhető el a virtuális gép hello üzemeltető hello hardver fürt.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-107">This can happen if hello new size is not available on hello hardware cluster that is hosting hello VM.</span></span>

<span data-ttu-id="6c1a9-108">Ez a cikk bemutatja, hogyan tooresize a Linux virtuális gép hello [Azure CLI][azure-cli].</span><span class="sxs-lookup"><span data-stu-id="6c1a9-108">This article shows how tooresize a Linux VM using hello [Azure CLI][azure-cli].</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="6c1a9-109">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="6c1a9-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="6c1a9-110">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="6c1a9-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="6c1a9-111">[Az Azure CLI 1.0](#resize-a-linux-vm) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="6c1a9-111">[Azure CLI 1.0](#resize-a-linux-vm) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="6c1a9-112">[Az Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="6c1a9-112">[Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="resize-a-linux-vm"></a><span data-ttu-id="6c1a9-113">Linux virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="6c1a9-113">Resize a Linux VM</span></span>
<span data-ttu-id="6c1a9-114">tooresize egy virtuális Gépet, hajtsa végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-114">tooresize a VM, perform hello following steps.</span></span>

1. <span data-ttu-id="6c1a9-115">Futtassa a következő parancsot a CLI hello.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-115">Run hello following CLI command.</span></span> <span data-ttu-id="6c1a9-116">Ez a parancs kilistázza hello Virtuálisgép-méretek elérhető hello hardver fürtön, amelyen hello virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-116">This command lists hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span>
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. <span data-ttu-id="6c1a9-117">Ha hello szükséges méret szerepel, futtassa a következő parancs tooresize hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-117">If hello desired size is listed, run hello following command tooresize hello VM.</span></span>
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    <span data-ttu-id="6c1a9-118">hello virtuális gép újraindul, a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-118">hello VM will restart during this process.</span></span> <span data-ttu-id="6c1a9-119">Hello az újraindítás után a meglévő operációs rendszer és az adatlemezek fogja képezni.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-119">After hello restart, your existing OS and data disks will be remapped.</span></span> <span data-ttu-id="6c1a9-120">Bármi hello ideiglenes lemezen elvesznek.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-120">Anything on hello temporary disk will be lost.</span></span>
   
    <span data-ttu-id="6c1a9-121">Használjon hello `--enable-boot-diagnostics` beállítás lehetővé teszi, hogy [rendszerindítási diagnosztika][boot-diagnostics], toolog bármely hibák kapcsolódó toostartup.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-121">Use hello `--enable-boot-diagnostics` option enables [boot diagnostics][boot-diagnostics], toolog any errors related toostartup.</span></span>
3. <span data-ttu-id="6c1a9-122">Ha hello szükséges méret nem szerepel a listában, egyébként futtassa a következő parancsok toodeallocate hello VM, méretezze át, és indítsa újra hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-122">Otherwise, if hello desired size is not listed, run hello following commands toodeallocate hello VM, resize it, and then restart hello VM.</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="6c1a9-123">Virtuális gép hello felszabadítása is dinamikus IP-címek hozzárendelése a virtuális gép toohello kiadását.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-123">Deallocating hello VM also releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="6c1a9-124">az operációs rendszer hello és adatlemezek nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="6c1a9-124">hello OS and data disks are not affected.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="6c1a9-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c1a9-125">Next steps</span></span>
<span data-ttu-id="6c1a9-126">További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése. További információkért lásd: [automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek][scale-set].</span><span class="sxs-lookup"><span data-stu-id="6c1a9-126">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
