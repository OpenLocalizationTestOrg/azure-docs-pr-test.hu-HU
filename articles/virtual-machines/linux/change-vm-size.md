---
title: "Linux virtuális gép hello Azure CLI 2.0 és aaaHow tooresize |} Microsoft Docs"
description: "Hogyan tooscale fel vagy le egy Linux virtuális gépet, módosításával méretezési hello Virtuálisgép-méretet."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8fba485b5bcc7824f546de5cf3df77624a28008
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a><span data-ttu-id="4d708-103">A Linux virtuális gépek CLI 2.0 átméretezése</span><span class="sxs-lookup"><span data-stu-id="4d708-103">Resize a Linux virtual machine using CLI 2.0</span></span>

<span data-ttu-id="4d708-104">(VM) virtuális gép kiépítése után méretezheti hello VM felfelé vagy lefelé hello módosításával [Virtuálisgép-méretet][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="4d708-104">After you provision a virtual machine (VM), you can scale hello VM up or down by changing hello [VM size][vm-sizes].</span></span> <span data-ttu-id="4d708-105">Néhány esetben először hello VM kell felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="4d708-105">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="4d708-106">Toodeallocate hello virtuális gép mérete nem érhető el a virtuális gép hello üzemeltető hello hardver fürt hello igény kell.</span><span class="sxs-lookup"><span data-stu-id="4d708-106">You need toodeallocate hello VM if hello desired size is not available on hello hardware cluster that is hosting hello VM.</span></span> <span data-ttu-id="4d708-107">Ez a cikk részletesen, hogyan tooresize rendelkező Linux virtuális gépet a hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="4d708-107">This article details how tooresize a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="4d708-108">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d708-108">You can also perform these steps with hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="resize-a-vm"></a><span data-ttu-id="4d708-109">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="4d708-109">Resize a VM</span></span>
<span data-ttu-id="4d708-110">a virtuális gépek tooresize, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="4d708-110">tooresize a VM, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

1. <span data-ttu-id="4d708-111">Rendelkezésre álló virtuális gép listájának megtekintése hello méretének hello hardveres fürtre, ahol hello virtuális gép tárolása a [az vm-vm-átméretezési-beállításai](/cli/azure/vm#list-vm-resize-options).</span><span class="sxs-lookup"><span data-stu-id="4d708-111">View hello list of available VM sizes on hello hardware cluster where hello VM is hosted with [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options).</span></span> <span data-ttu-id="4d708-112">hello alábbi példa felsorolja hello nevű virtuális gép Virtuálisgép-méretek `myVM` hello erőforráscsoportban `myResourceGroup` régió:</span><span class="sxs-lookup"><span data-stu-id="4d708-112">hello following example lists VM sizes for hello VM named `myVM` in hello resource group `myResourceGroup` region:</span></span>
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. <span data-ttu-id="4d708-113">Igény hello szerepel a Virtuálisgép-méretet, méretezze át a virtuális gép hello [az vm átméretezése](/cli/azure/vm#resize).</span><span class="sxs-lookup"><span data-stu-id="4d708-113">If hello desired VM size is listed, resize hello VM with [az vm resize](/cli/azure/vm#resize).</span></span> <span data-ttu-id="4d708-114">a következő példa átméretezi hello hello nevű virtuális gép `myVM` toohello `Standard_DS3_v2` mérete:</span><span class="sxs-lookup"><span data-stu-id="4d708-114">hello following example resizes hello VM named `myVM` toohello `Standard_DS3_v2` size:</span></span>
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    <span data-ttu-id="4d708-115">hello virtuális gép újraindul, a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="4d708-115">hello VM restarts during this process.</span></span> <span data-ttu-id="4d708-116">Hello az újraindítás után a meglévő operációs rendszer és az adatlemezek újra társítani.</span><span class="sxs-lookup"><span data-stu-id="4d708-116">After hello restart, your existing OS and data disks are remapped.</span></span> <span data-ttu-id="4d708-117">Bármi hello ideiglenes lemezen elvész.</span><span class="sxs-lookup"><span data-stu-id="4d708-117">Anything on hello temporary disk is lost.</span></span>

3. <span data-ttu-id="4d708-118">Hello szükséges Virtuálisgép-méret nem szerepel a listában, ha szüksége van-e toofirst felszabadítani a virtuális gép és hello [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="4d708-118">If hello desired VM size is not listed, you need toofirst deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="4d708-119">Ez a folyamat lehetővé teszi, hogy hello VM toothen átméretezett tooany méret érhető el, amely támogatja a régió hello, majd el.</span><span class="sxs-lookup"><span data-stu-id="4d708-119">This process allows hello VM toothen be resized tooany size available that hello region supports and then started.</span></span> <span data-ttu-id="4d708-120">hello lépések felszabadítani, méretezze át, és indítsa el hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="4d708-120">hello following steps deallocate, resize, and then start hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="4d708-121">Virtuális gép hello felszabadítása is dinamikus IP-címek hozzárendelése a virtuális gép toohello kiadását.</span><span class="sxs-lookup"><span data-stu-id="4d708-121">Deallocating hello VM also releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="4d708-122">az operációs rendszer hello és adatlemezek nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="4d708-122">hello OS and data disks are not affected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d708-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d708-123">Next steps</span></span>
<span data-ttu-id="4d708-124">További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése. További információkért lásd: [automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek][scale-set].</span><span class="sxs-lookup"><span data-stu-id="4d708-124">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
