---
title: "Válassza le a Linux virtuális gép - Azure adatlemezét |} Microsoft Docs"
description: "Ismerje meg, egy data lemezt leválasztani a virtuális gépen az Azure CLI 2.0 vagy az Azure-portálon."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: 3f29547e1da6028b1e4b91d9e29fd3bcdfe08d50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a><span data-ttu-id="f27f8-103">Hogyan egy Linux virtuális gép adatlemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="f27f8-103">How to detach a data disk from a Linux virtual machine</span></span>

<span data-ttu-id="f27f8-104">Ha már nincs szüksége egy virtuális géphez csatolt adatlemezre, könnyedén leválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="f27f8-104">When you no longer need a data disk that's attached to a virtual machine, you can easily detach it.</span></span> <span data-ttu-id="f27f8-105">Ez a lemez eltávolítása a virtuális gépről, de ez nem távolítja el a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="f27f8-105">This removes the disk from the virtual machine, but doesn't remove it from storage.</span></span> 

> [!WARNING]
> <span data-ttu-id="f27f8-106">A lemez leválasztása esetén nem automatikusan törlődik.</span><span class="sxs-lookup"><span data-stu-id="f27f8-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="f27f8-107">Ha prémium szintű Storage, a lemez tárolási költségek továbbra.</span><span class="sxs-lookup"><span data-stu-id="f27f8-107">If you have subscribed to Premium storage, you will continue to incur storage charges for the disk.</span></span> <span data-ttu-id="f27f8-108">További információt talál [árak és számlázás prémium szintű Storage használatakor](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="f27f8-108">For more information refer to [Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span> 
> 
> 

<span data-ttu-id="f27f8-109">Ha ismét használni szeretné a lemezen lévő adatokat, újból csatolhatja ugyanahhoz vagy egy másik virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="f27f8-109">If you want to use the existing data on the disk again, you can reattach it to the same virtual machine, or another one.</span></span>  

## <a name="detach-a-data-disk-using-cli-20"></a><span data-ttu-id="f27f8-110">Parancssori felület 2.0 használatával adatlemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="f27f8-110">Detach a data disk using CLI 2.0</span></span>

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

<span data-ttu-id="f27f8-111">A lemez a tárolóban marad, de már nincs virtuális géphez csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="f27f8-111">The disk remains in storage but is no longer attached to a virtual machine.</span></span>


## <a name="detach-a-data-disk-using-the-portal"></a><span data-ttu-id="f27f8-112">Adatlemez leválasztása a portállal</span><span class="sxs-lookup"><span data-stu-id="f27f8-112">Detach a data disk using the portal</span></span>
1. <span data-ttu-id="f27f8-113">Válassza ki a portál hub **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="f27f8-113">In the portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="f27f8-114">Válassza ki a virtuális gépet, amely rendelkezik a adatlemez leválasztása, és kattintson a kívánt **leállítása** felszabadítani a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="f27f8-114">Select the virtual machine that has the data disk you want to detach and click **Stop** to deallocate the VM.</span></span>
3. <span data-ttu-id="f27f8-115">Válassza ki a virtuális gépek paneljét **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="f27f8-115">In the virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="f27f8-116">Felső részén a **lemezek** panelen válassza **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="f27f8-116">At the top of the **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="f27f8-117">Az a **lemezek** panelen a adatlemez, amelyeket szeretne leválasztani, kattintson a jobb a ![leválasztási gomb képe](./media/detach-disk/detach.png) leválasztani gombra.</span><span class="sxs-lookup"><span data-stu-id="f27f8-117">In the **Disks** blade, to the far right of the data disk that you would like to detach, click the ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="f27f8-118">A lemez eltávolítása után a panel tetején kattintson a Mentés gombra.</span><span class="sxs-lookup"><span data-stu-id="f27f8-118">After the disk has been removed, click Save on the top of the blade.</span></span>
6. <span data-ttu-id="f27f8-119">A virtuális gép paneljén kattintson **áttekintése** , majd a **Start** gombra a virtuális gép újraindítására a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="f27f8-119">In the virtual machine blade, click **Overview** and then click the **Start** button at the top of the blade to restart the VM.</span></span>

<span data-ttu-id="f27f8-120">A lemez a tárolóban marad, de már nincs virtuális géphez csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="f27f8-120">The disk remains in storage but is no longer attached to a virtual machine.</span></span>








## <a name="next-steps"></a><span data-ttu-id="f27f8-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f27f8-121">Next steps</span></span>
<span data-ttu-id="f27f8-122">Ha szeretné ismét felhasználni a adatlemez, most [csatlakoztassa azt egy másik virtuális gép](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f27f8-122">If you want to reuse the data disk, you can just [attach it to another VM](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

