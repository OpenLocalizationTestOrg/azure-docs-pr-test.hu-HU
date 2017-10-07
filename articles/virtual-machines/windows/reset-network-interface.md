---
title: "az Azure virtuális gépekhez Windows aaaHow tooreset hálózati illesztő |} Microsoft Docs"
description: "Bemutatja, hogyan tooreset a hálózati kapcsolat az Azure virtuális gépekhez Windows"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.openlocfilehash: 1b653820927ef4c3bb8f384a7e752846a8be3da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-network-interface-for-azure-windows-vm"></a><span data-ttu-id="504c4-103">Hogyan tooreset a hálózati kapcsolat az Azure virtuális gépekhez Windows</span><span class="sxs-lookup"><span data-stu-id="504c4-103">How tooreset network interface for Azure Windows VM</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="504c4-104">Hello alapértelmezett hálózati illesztő (NIC) letiltása után a tooMicrosoft Azure Windows virtuális gép (VM) nem tud csatlakozni, vagy manuálisan egy statikus IP-cím beállítása hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="504c4-104">You cannot connect tooMicrosoft Azure Windows Virtual Machine (VM) after you disable hello default Network Interface (NIC) or manually sets a static IP for hello NIC.</span></span> <span data-ttu-id="504c4-105">Ez a cikk bemutatja, hogyan tooreset hello hello távoli kapcsolati probléma elhárítható Azure Windows virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="504c4-105">This article shows how tooreset hello network interface for Azure Windows VM, which will resolve hello remote connection issue.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a><span data-ttu-id="504c4-106">Hálózati adapter alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="504c4-106">Reset network interface</span></span>

### <a name="for-classic-vms"></a><span data-ttu-id="504c4-107">Klasszikus virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="504c4-107">For Classic VMs</span></span>

<span data-ttu-id="504c4-108">tooreset hálózati csatoló, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="504c4-108">tooreset network interface, follow these steps:</span></span>

1.  <span data-ttu-id="504c4-109">Nyissa meg toohello [Azure-portálon]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="504c4-109">Go toohello [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="504c4-110">Válassza ki **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="504c4-110">Select **Virtual Machines (Classic)**.</span></span>
3.  <span data-ttu-id="504c4-111">Jelölje be hello hatással a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="504c4-111">Select hello affected Virtual Machine.</span></span>
4.  <span data-ttu-id="504c4-112">Válassza ki **IP-címek**.</span><span class="sxs-lookup"><span data-stu-id="504c4-112">Select **IP addresses**.</span></span>
5.  <span data-ttu-id="504c4-113">Ha hello **privát IP-hozzárendelés** nem **statikus**, túl megváltoztatnia**statikus**.</span><span class="sxs-lookup"><span data-stu-id="504c4-113">If hello **Private IP assignment**  is not  **Static**, change it too**Static**.</span></span>
6.  <span data-ttu-id="504c4-114">Változás hello **IP-cím** hello alhálózat elérhető IP-cím tooanother.</span><span class="sxs-lookup"><span data-stu-id="504c4-114">Change hello **IP address** tooanother IP address that is available in hello Subnet.</span></span>
7.  <span data-ttu-id="504c4-115">A mentés kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="504c4-115">Select Save.</span></span>
8.  <span data-ttu-id="504c4-116">virtuális gép hello tooinitialize hello új hálózati toohello rendszer újraindul.</span><span class="sxs-lookup"><span data-stu-id="504c4-116">hello virtual machine will restart tooinitialize hello new NIC toohello system.</span></span>
9.  <span data-ttu-id="504c4-117">Próbálja meg tooRDP tooyour gép.</span><span class="sxs-lookup"><span data-stu-id="504c4-117">Try tooRDP tooyour machine.</span></span> <span data-ttu-id="504c4-118">Ha sikeres, hello magán IP cím hátsó toohello eredeti módosíthatja, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="504c4-118">If successful, you can change hello Private IP address back toohello original if you would like.</span></span> <span data-ttu-id="504c4-119">Ellenkező esetben beállíthatja, hogy azt.</span><span class="sxs-lookup"><span data-stu-id="504c4-119">Otherwise, you can keep it.</span></span> 

### <a name="for-vms-deployed-in-resource-group-model"></a><span data-ttu-id="504c4-120">Erőforrás csoport modellben telepített virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="504c4-120">For VMs deployed in Resource group model</span></span>

1.  <span data-ttu-id="504c4-121">Nyissa meg toohello [Azure-portálon]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="504c4-121">Go toohello [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="504c4-122">Jelölje be hello hatással a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="504c4-122">Select hello affected Virtual Machine.</span></span>
3.  <span data-ttu-id="504c4-123">Válassza ki **hálózati illesztőt**.</span><span class="sxs-lookup"><span data-stu-id="504c4-123">Select **Network Interfaces**.</span></span>
4.  <span data-ttu-id="504c4-124">Válassza ki a géphez tartozó hálózati illesztőt hello</span><span class="sxs-lookup"><span data-stu-id="504c4-124">Select hello Network Interface associated with your machine</span></span>
5.  <span data-ttu-id="504c4-125">Válassza ki **IP-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="504c4-125">Select **IP configurations**.</span></span>
6.  <span data-ttu-id="504c4-126">Jelölje be hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="504c4-126">Select hello IP.</span></span> 
7.  <span data-ttu-id="504c4-127">Ha hello **privát IP-hozzárendelés** nem **statikus**, túl megváltoztatnia**statikus**.</span><span class="sxs-lookup"><span data-stu-id="504c4-127">If hello **Private IP assignment**  is not  **Static**, change it too**Static**.</span></span>
8.  <span data-ttu-id="504c4-128">Változás hello **IP-cím** hello alhálózat elérhető IP-cím tooanother.</span><span class="sxs-lookup"><span data-stu-id="504c4-128">Change hello **IP address** tooanother IP address that is available in hello Subnet.</span></span>
9. <span data-ttu-id="504c4-129">virtuális gép hello tooinitialize hello új hálózati toohello rendszer újraindul.</span><span class="sxs-lookup"><span data-stu-id="504c4-129">hello virtual machine will restart tooinitialize hello new NIC toohello system.</span></span>
10. <span data-ttu-id="504c4-130">Próbálja meg tooRDP tooyour gép.</span><span class="sxs-lookup"><span data-stu-id="504c4-130">Try tooRDP tooyour machine.</span></span> <span data-ttu-id="504c4-131">Ha sikeres, hello magán IP cím hátsó toohello eredeti módosíthatja, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="504c4-131">If successful, you can change hello Private IP address back toohello original if you would like.</span></span> <span data-ttu-id="504c4-132">Ellenkező esetben beállíthatja, hogy azt.</span><span class="sxs-lookup"><span data-stu-id="504c4-132">Otherwise, you can keep it.</span></span> 

## <a name="delete-hello-unavailable-nics"></a><span data-ttu-id="504c4-133">Törlés hello nem érhető el a hálózati adapterek</span><span class="sxs-lookup"><span data-stu-id="504c4-133">Delete hello unavailable NICs</span></span>
<span data-ttu-id="504c4-134">Után is távoli asztali toohello géphez, törölnie kell az hello régi hálózati adapterek tooavoid hello lehetséges problémát:</span><span class="sxs-lookup"><span data-stu-id="504c4-134">After you can remote desktop toohello machine, you must delete hello old NICs tooavoid hello potential problem:</span></span>

1.  <span data-ttu-id="504c4-135">Nyissa meg az Eszközkezelőben.</span><span class="sxs-lookup"><span data-stu-id="504c4-135">Open Device Manager.</span></span>
2.  <span data-ttu-id="504c4-136">Válassza ki **nézet** > **rejtett eszközök megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="504c4-136">Select **View** > **Show hidden devices**.</span></span>
3.  <span data-ttu-id="504c4-137">Válassza ki **hálózati adapterek**.</span><span class="sxs-lookup"><span data-stu-id="504c4-137">Select **Network Adapters**.</span></span> 
4.  <span data-ttu-id="504c4-138">Ellenőrizze, mint a "Microsoft Hyper-V hálózati Adapter" nevű hello adapterek.</span><span class="sxs-lookup"><span data-stu-id="504c4-138">Check for hello adapters named as "Microsoft Hyper-V Network Adapter".</span></span>
5.  <span data-ttu-id="504c4-139">Láthatja, hogy az nem érhető el adapter, amely szürkén jelenik meg. Kattintson a jobb gombbal a hello adapter, és válassza az eltávolítás.</span><span class="sxs-lookup"><span data-stu-id="504c4-139">You might see an unavailable adapter that is grayed out. Right-click hello adapter and then select Uninstall.</span></span>

    ![a hálózati adapter hello hello képe](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > <span data-ttu-id="504c4-141">Csak eltávolítása nem érhető el adapterek hello hello neve "Microsoft Hyper-V hálózati Adapter" használatát.</span><span class="sxs-lookup"><span data-stu-id="504c4-141">Only uninstall hello unavailable adapters that have hello name "Microsoft Hyper-V Network Adapter".</span></span> <span data-ttu-id="504c4-142">Ha eltávolítja a hello bármely más rejtett adapterek, akkor további problémák.</span><span class="sxs-lookup"><span data-stu-id="504c4-142">If you uninstall any of hello other hidden adapters, it could cause additional issues.</span></span>
    >
    >

6.  <span data-ttu-id="504c4-143">Most már nem érhető el az összes adapter jelölését törölje a rendszerből.</span><span class="sxs-lookup"><span data-stu-id="504c4-143">Now all unavailable adapter should be cleared out from your system.</span></span>