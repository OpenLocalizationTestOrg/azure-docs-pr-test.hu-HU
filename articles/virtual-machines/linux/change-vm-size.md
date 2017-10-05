---
title: "Linux virtuális gép és az Azure CLI 2.0 átméretezése |} Microsoft Docs"
description: "Hogyan növelheti vagy csökkentheti a Linux virtuális gépek, a Virtuálisgép-méretet módosításával."
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
ms.openlocfilehash: 23fc9f7f34732079682857d4ee685fe811751698
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a><span data-ttu-id="ec8a3-103">A Linux virtuális gépek CLI 2.0 átméretezése</span><span class="sxs-lookup"><span data-stu-id="ec8a3-103">Resize a Linux virtual machine using CLI 2.0</span></span>

<span data-ttu-id="ec8a3-104">(VM) virtuális gép kiépítése után méretezheti a virtuális gép felfelé vagy lefelé módosításával a [Virtuálisgép-méretet][vm-sizes].</span><span class="sxs-lookup"><span data-stu-id="ec8a3-104">After you provision a virtual machine (VM), you can scale the VM up or down by changing the [VM size][vm-sizes].</span></span> <span data-ttu-id="ec8a3-105">Néhány esetben először a virtuális gép kell felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-105">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="ec8a3-106">Szeretné felszabadítani a virtuális gép, ha a kívánt méretet a hardver fürtön, amelyen a virtuális gép nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-106">You need to deallocate the VM if the desired size is not available on the hardware cluster that is hosting the VM.</span></span> <span data-ttu-id="ec8a3-107">Ez a cikk a Linux virtuális gép és az Azure CLI 2.0 átméretezése részletezi.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-107">This article details how to resize a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="ec8a3-108">Az [Azure CLI 1.0-s](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-108">You can also perform these steps with the [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="resize-a-vm"></a><span data-ttu-id="ec8a3-109">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="ec8a3-109">Resize a VM</span></span>
<span data-ttu-id="ec8a3-110">A legújabb kell átméretezni egy virtuális Gépet, [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ec8a3-110">To resize a VM, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

1. <span data-ttu-id="ec8a3-111">A rendelkezésre álló Virtuálisgép-méretek listáját megtekintheti a hardver fürt, ahol a virtuális gép tárolása a [az vm-vm-átméretezési-beállításai](/cli/azure/vm#list-vm-resize-options).</span><span class="sxs-lookup"><span data-stu-id="ec8a3-111">View the list of available VM sizes on the hardware cluster where the VM is hosted with [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options).</span></span> <span data-ttu-id="ec8a3-112">Az alábbi példa felsorolja a Virtuálisgép-méretek a virtuális gép nevű `myVM` erőforráscsoportban `myResourceGroup` régió:</span><span class="sxs-lookup"><span data-stu-id="ec8a3-112">The following example lists VM sizes for the VM named `myVM` in the resource group `myResourceGroup` region:</span></span>
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. <span data-ttu-id="ec8a3-113">Ha a kívánt virtuális gép mérete szerepel, méretezze át a virtuális Géphez a [az vm átméretezése](/cli/azure/vm#resize).</span><span class="sxs-lookup"><span data-stu-id="ec8a3-113">If the desired VM size is listed, resize the VM with [az vm resize](/cli/azure/vm#resize).</span></span> <span data-ttu-id="ec8a3-114">Az alábbi példa átméretezi nevű virtuális gép `myVM` számára a `Standard_DS3_v2` mérete:</span><span class="sxs-lookup"><span data-stu-id="ec8a3-114">The following example resizes the VM named `myVM` to the `Standard_DS3_v2` size:</span></span>
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    <span data-ttu-id="ec8a3-115">A virtuális gép újraindul, a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-115">The VM restarts during this process.</span></span> <span data-ttu-id="ec8a3-116">Az újraindítás után a meglévő operációs rendszer és az adatlemezek újra társítani.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-116">After the restart, your existing OS and data disks are remapped.</span></span> <span data-ttu-id="ec8a3-117">Az ideiglenes lemezen semmit nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-117">Anything on the temporary disk is lost.</span></span>

3. <span data-ttu-id="ec8a3-118">Ha a kívánt virtuális gép mérete nem szerepel, szeretné-e először felszabadítani a virtuális Géphez a [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="ec8a3-118">If the desired VM size is not listed, you need to first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="ec8a3-119">Ez a folyamat lehetővé teszi, hogy a virtuális Gépet, majd átméretezése az összes rendelkezésre álló terület az, hogy a régió támogatja, majd el.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-119">This process allows the VM to then be resized to any size available that the region supports and then started.</span></span> <span data-ttu-id="ec8a3-120">Az alábbi lépéseket felszabadítani, méretezze át, és indítsa el a nevű virtuális gép `myVM` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="ec8a3-120">The following steps deallocate, resize, and then start the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > <span data-ttu-id="ec8a3-121">A virtuális Géphez rendelt dinamikus IP-címek is a virtuális gép felszabadítása kiadását.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-121">Deallocating the VM also releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="ec8a3-122">Az operációsrendszer- és adatlemezek, nem érintettek.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-122">The OS and data disks are not affected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec8a3-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec8a3-123">Next steps</span></span>
<span data-ttu-id="ec8a3-124">További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="ec8a3-124">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="ec8a3-125">További információkért lásd: [automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek][scale-set].</span><span class="sxs-lookup"><span data-stu-id="ec8a3-125">For more information, see [Automatically scale Linux machines in a Virtual Machine Scale Set][scale-set].</span></span> 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
