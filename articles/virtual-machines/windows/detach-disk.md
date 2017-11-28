---
title: "Windowsos VM – Azure adatlemezt aaaDetach |} Microsoft Docs"
description: "Ismerje meg, hogy a virtuális gép az Azure-ban hello Resource Manager üzembe helyezési modellben adatlemezt toodetach."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: f3f581d3f33329db2ecb7d25a68bc59af7361aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-windows-virtual-machine"></a><span data-ttu-id="01a2c-103">Hogyan toodetach az adatok lemezre Windows virtuális gépről</span><span class="sxs-lookup"><span data-stu-id="01a2c-103">How toodetach a data disk from a Windows virtual machine</span></span>
<span data-ttu-id="01a2c-104">Ha már nincs szüksége, amely virtuális géphez csatolt tooa adatlemezt, könnyen leválasztás.</span><span class="sxs-lookup"><span data-stu-id="01a2c-104">When you no longer need a data disk that's attached tooa virtual machine, you can easily detach it.</span></span> <span data-ttu-id="01a2c-105">Hello lemez eltávolítása a hello virtuális gépről, de ez nem távolítja el a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="01a2c-105">This removes hello disk from hello virtual machine, but doesn't remove it from storage.</span></span>

> [!WARNING]
> <span data-ttu-id="01a2c-106">A lemez leválasztása esetén nem automatikusan törlődik.</span><span class="sxs-lookup"><span data-stu-id="01a2c-106">If you detach a disk it is not automatically deleted.</span></span> <span data-ttu-id="01a2c-107">Ha tooPremium tárolási, Ön továbbra tooincur tárolási költségek hello lemezhez.</span><span class="sxs-lookup"><span data-stu-id="01a2c-107">If you have subscribed tooPremium storage, you will continue tooincur storage charges for hello disk.</span></span> <span data-ttu-id="01a2c-108">További információt talál a túl[árak és számlázás prémium szintű Storage használatakor](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="01a2c-108">For more information refer too[Pricing and Billing when using Premium Storage](../../storage/common/storage-premium-storage.md#pricing-and-billing).</span></span>
>
>

<span data-ttu-id="01a2c-109">Ha újra toouse hello meglévő hello a lemezen lévő adatokat, akkor is csatlakoztassa újra toohello ugyanahhoz a virtuális géphez, vagy egy másikra.</span><span class="sxs-lookup"><span data-stu-id="01a2c-109">If you want toouse hello existing data on hello disk again, you can reattach it toohello same virtual machine, or another one.</span></span>

## <a name="detach-a-data-disk-using-hello-portal"></a><span data-ttu-id="01a2c-110">Hello portálon adatlemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="01a2c-110">Detach a data disk using hello portal</span></span>
1. <span data-ttu-id="01a2c-111">Hello portál központban, válassza ki a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="01a2c-111">In hello portal hub, select **Virtual Machines**.</span></span>
2. <span data-ttu-id="01a2c-112">Válassza ki a hello virtuális gépekről, amelyek hello adatlemez toodetach ki, majd kattintson **leállítása** toodeallocate hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="01a2c-112">Select hello virtual machine that has hello data disk you want toodetach and click **Stop** toodeallocate hello VM.</span></span>
3. <span data-ttu-id="01a2c-113">Hello virtuális gép panelén válassza **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="01a2c-113">In hello virtual machine blade, select **Disks**.</span></span>
4. <span data-ttu-id="01a2c-114">Hello hello tetején **lemezek** panelen válassza **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="01a2c-114">At hello top of hello **Disks** blade, select **Edit**.</span></span>
5. <span data-ttu-id="01a2c-115">A hello **lemezek** panelen toohello hello adatlemez, hogy szeretné-e toodetach, a jobb szélén kattintson hello ![leválasztási gomb képe](./media/detach-disk/detach.png) leválasztani gombra.</span><span class="sxs-lookup"><span data-stu-id="01a2c-115">In hello **Disks** blade, toohello far right of hello data disk that you would like toodetach, click hello ![Detach button image](./media/detach-disk/detach.png) detach button.</span></span>
5. <span data-ttu-id="01a2c-116">Hello lemez eltávolítása után kattintson a Mentés gombra hello felül hello panelről.</span><span class="sxs-lookup"><span data-stu-id="01a2c-116">After hello disk has been removed, click Save on hello top of hello blade.</span></span>
6. <span data-ttu-id="01a2c-117">Hello virtuális gép paneljén kattintson **áttekintése** majd hello **Start** hello panel toorestart hello VM hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="01a2c-117">In hello virtual machine blade, click **Overview** and then click hello **Start** button at hello top of hello blade toorestart hello VM.</span></span>



<span data-ttu-id="01a2c-118">hello lemez tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="01a2c-118">hello disk remains in storage but is no longer attached tooa virtual machine.</span></span>

## <a name="detach-a-data-disk-using-powershell"></a><span data-ttu-id="01a2c-119">PowerShell-lel adatlemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="01a2c-119">Detach a data disk using PowerShell</span></span>
<span data-ttu-id="01a2c-120">Ebben a példában hello lekérdezi hello nevű virtuális gép első parancs **MyVM07** a hello **RG11** erőforráscsoport hello Get-AzureRmVM parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="01a2c-120">In this example, hello first command gets hello virtual machine named **MyVM07** in hello **RG11** resource group using hello Get-AzureRmVM cmdlet.</span></span> <span data-ttu-id="01a2c-121">tárolja a virtuális gép hello hello parancs hello **$VirtualMachine** változó.</span><span class="sxs-lookup"><span data-stu-id="01a2c-121">hello command stores hello virtual machine in hello **$VirtualMachine** variable.</span></span>

<span data-ttu-id="01a2c-122">hello második parancs eltávolítja az hello adatlemez DataDisk3 nevű hello virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="01a2c-122">hello second command removes hello data disk named DataDisk3 from hello virtual machine.</span></span>

<span data-ttu-id="01a2c-123">hello utolsó parancs frissíti hello virtuális gép toocomplete hello folyamat hello adatlemez eltávolításával hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="01a2c-123">hello final command updates hello state of hello virtual machine toocomplete hello process of removing hello data disk.</span></span>

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

<span data-ttu-id="01a2c-124">További információkért lásd: [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span><span class="sxs-lookup"><span data-stu-id="01a2c-124">For more information, see [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).</span></span>

## <a name="next-steps"></a><span data-ttu-id="01a2c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01a2c-125">Next steps</span></span>
<span data-ttu-id="01a2c-126">Ha azt szeretné, hogy tooreuse hello adatlemez, most [tooanother virtuális gép csatlakoztatása](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="01a2c-126">If you want tooreuse hello data disk, you can just [attach it tooanother VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

