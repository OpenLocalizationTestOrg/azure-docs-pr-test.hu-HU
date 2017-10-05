---
title: "Hálózati kapcsolat visszaállítása az Azure virtuális gépekhez Windows |} Microsoft Docs"
description: "Bemutatja, hogyan Azure Windows virtuális hálózati adapter alaphelyzetbe"
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
ms.openlocfilehash: 220e426be20086841854d89831f6c9d67529867f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a><span data-ttu-id="d11fd-103">Az Azure virtuális gépekhez Windows hálózati kapcsolat visszaállítása</span><span class="sxs-lookup"><span data-stu-id="d11fd-103">How to reset network interface for Azure Windows VM</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="d11fd-104">Nem lehet csatlakoztatni a Microsoft Azure Windows virtuális gép (VM) után tiltsa le az alapértelmezett hálózati illesztő (NIC), vagy manuálisan egy statikus IP-cím beállítása a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="d11fd-104">You cannot connect to Microsoft Azure Windows Virtual Machine (VM) after you disable the default Network Interface (NIC) or manually sets a static IP for the NIC.</span></span> <span data-ttu-id="d11fd-105">Ez a cikk bemutatja, hogyan alaphelyzetbe állítani a hálózati illesztő Azure Windows virtuális gép, amely a távoli kapcsolati probléma elhárítható.</span><span class="sxs-lookup"><span data-stu-id="d11fd-105">This article shows how to reset the network interface for Azure Windows VM, which will resolve the remote connection issue.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a><span data-ttu-id="d11fd-106">Hálózati adapter alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="d11fd-106">Reset network interface</span></span>

### <a name="for-classic-vms"></a><span data-ttu-id="d11fd-107">Klasszikus virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="d11fd-107">For Classic VMs</span></span>

<span data-ttu-id="d11fd-108">Hálózati illesztő visszaállításához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d11fd-108">To reset network interface, follow these steps:</span></span>

1.  <span data-ttu-id="d11fd-109">Nyissa meg az [Azure Portal]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d11fd-109">Go to the [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="d11fd-110">Válassza ki **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-110">Select **Virtual Machines (Classic)**.</span></span>
3.  <span data-ttu-id="d11fd-111">Válassza ki az érintett virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d11fd-111">Select the affected Virtual Machine.</span></span>
4.  <span data-ttu-id="d11fd-112">Válassza ki **IP-címek**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-112">Select **IP addresses**.</span></span>
5.  <span data-ttu-id="d11fd-113">Ha a **privát IP-hozzárendelés** nem **statikus**, módosítsa úgy, hogy **statikus**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-113">If the **Private IP assignment**  is not  **Static**, change it to **Static**.</span></span>
6.  <span data-ttu-id="d11fd-114">Módosítsa a **IP-cím** egy másik, elérhető az alhálózat IP-címre.</span><span class="sxs-lookup"><span data-stu-id="d11fd-114">Change the **IP address** to another IP address that is available in the Subnet.</span></span>
7.  <span data-ttu-id="d11fd-115">A mentés kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="d11fd-115">Select Save.</span></span>
8.  <span data-ttu-id="d11fd-116">A virtuális gép újraindul, a rendszer az új hálózati inicializálása.</span><span class="sxs-lookup"><span data-stu-id="d11fd-116">The virtual machine will restart to initialize the new NIC to the system.</span></span>
9.  <span data-ttu-id="d11fd-117">Próbálja meg az RDP a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d11fd-117">Try to RDP to your machine.</span></span> <span data-ttu-id="d11fd-118">Ha sikeres, módosíthatja a magánhálózati IP-cím vissza az eredeti Ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="d11fd-118">If successful, you can change the Private IP address back to the original if you would like.</span></span> <span data-ttu-id="d11fd-119">Ellenkező esetben beállíthatja, hogy azt.</span><span class="sxs-lookup"><span data-stu-id="d11fd-119">Otherwise, you can keep it.</span></span> 

### <a name="for-vms-deployed-in-resource-group-model"></a><span data-ttu-id="d11fd-120">Erőforrás csoport modellben telepített virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="d11fd-120">For VMs deployed in Resource group model</span></span>

1.  <span data-ttu-id="d11fd-121">Nyissa meg az [Azure Portal]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d11fd-121">Go to the [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="d11fd-122">Válassza ki az érintett virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d11fd-122">Select the affected Virtual Machine.</span></span>
3.  <span data-ttu-id="d11fd-123">Válassza ki **hálózati illesztőt**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-123">Select **Network Interfaces**.</span></span>
4.  <span data-ttu-id="d11fd-124">Válassza ki a géphez társított hálózati illesztőt</span><span class="sxs-lookup"><span data-stu-id="d11fd-124">Select the Network Interface associated with your machine</span></span>
5.  <span data-ttu-id="d11fd-125">Válassza ki **IP-konfigurációk**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-125">Select **IP configurations**.</span></span>
6.  <span data-ttu-id="d11fd-126">Válassza ki az IP-cím.</span><span class="sxs-lookup"><span data-stu-id="d11fd-126">Select the IP.</span></span> 
7.  <span data-ttu-id="d11fd-127">Ha a **privát IP-hozzárendelés** nem **statikus**, módosítsa úgy, hogy **statikus**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-127">If the **Private IP assignment**  is not  **Static**, change it to **Static**.</span></span>
8.  <span data-ttu-id="d11fd-128">Módosítsa a **IP-cím** egy másik, elérhető az alhálózat IP-címre.</span><span class="sxs-lookup"><span data-stu-id="d11fd-128">Change the **IP address** to another IP address that is available in the Subnet.</span></span>
9. <span data-ttu-id="d11fd-129">A virtuális gép újraindul, a rendszer az új hálózati inicializálása.</span><span class="sxs-lookup"><span data-stu-id="d11fd-129">The virtual machine will restart to initialize the new NIC to the system.</span></span>
10. <span data-ttu-id="d11fd-130">Próbálja meg az RDP a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d11fd-130">Try to RDP to your machine.</span></span> <span data-ttu-id="d11fd-131">Ha sikeres, módosíthatja a magánhálózati IP-cím vissza az eredeti Ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="d11fd-131">If successful, you can change the Private IP address back to the original if you would like.</span></span> <span data-ttu-id="d11fd-132">Ellenkező esetben beállíthatja, hogy azt.</span><span class="sxs-lookup"><span data-stu-id="d11fd-132">Otherwise, you can keep it.</span></span> 

## <a name="delete-the-unavailable-nics"></a><span data-ttu-id="d11fd-133">Az elérhető hálózati adapterek törlése</span><span class="sxs-lookup"><span data-stu-id="d11fd-133">Delete the unavailable NICs</span></span>
<span data-ttu-id="d11fd-134">Után is a távoli asztal a géphez, törölnie kell a régi hálózati adaptert a potenciális problémák elkerülése érdekében:</span><span class="sxs-lookup"><span data-stu-id="d11fd-134">After you can remote desktop to the machine, you must delete the old NICs to avoid the potential problem:</span></span>

1.  <span data-ttu-id="d11fd-135">Nyissa meg az Eszközkezelőben.</span><span class="sxs-lookup"><span data-stu-id="d11fd-135">Open Device Manager.</span></span>
2.  <span data-ttu-id="d11fd-136">Válassza ki **nézet** > **rejtett eszközök megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-136">Select **View** > **Show hidden devices**.</span></span>
3.  <span data-ttu-id="d11fd-137">Válassza ki **hálózati adapterek**.</span><span class="sxs-lookup"><span data-stu-id="d11fd-137">Select **Network Adapters**.</span></span> 
4.  <span data-ttu-id="d11fd-138">Ellenőrizze, hogy az adapterek, mint a "Microsoft Hyper-V hálózati Adapter" nevű.</span><span class="sxs-lookup"><span data-stu-id="d11fd-138">Check for the adapters named as "Microsoft Hyper-V Network Adapter".</span></span>
5.  <span data-ttu-id="d11fd-139">Láthatja, hogy az nem érhető el adapter, amely szürkén jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d11fd-139">You might see an unavailable adapter that is grayed out.</span></span> <span data-ttu-id="d11fd-140">Kattintson a jobb gombbal az adaptert, és válassza az eltávolítás.</span><span class="sxs-lookup"><span data-stu-id="d11fd-140">Right-click the adapter and then select Uninstall.</span></span>

    ![a hálózati adapter képe](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > <span data-ttu-id="d11fd-142">Csak távolítsa el a "Microsoft Hyper-V hálózati Adapter" nevű nem érhető el adaptereket.</span><span class="sxs-lookup"><span data-stu-id="d11fd-142">Only uninstall the unavailable adapters that have the name "Microsoft Hyper-V Network Adapter".</span></span> <span data-ttu-id="d11fd-143">Ha bármelyik más rejtett adapter távolítja el, további problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="d11fd-143">If you uninstall any of the other hidden adapters, it could cause additional issues.</span></span>
    >
    >

6.  <span data-ttu-id="d11fd-144">Most már nem érhető el az összes adapter jelölését törölje a rendszerből.</span><span class="sxs-lookup"><span data-stu-id="d11fd-144">Now all unavailable adapter should be cleared out from your system.</span></span>