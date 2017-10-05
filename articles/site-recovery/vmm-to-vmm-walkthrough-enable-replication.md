---
title: "Egy másodlagos helyre, az Azure Site Recovery szolgáltatással a Hyper-V replikáció engedélyezése |} Microsoft Docs"
description: "Engedélyezze a replikálást a Hyper-V virtuális gépek replikálása az Azure Site Recovery System Center VMM másodlagos hely ismerteti."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a><span data-ttu-id="9bd6f-103">9. lépés: Engedélyezze a Hyper-V virtuális gépek replikálása másodlagos helyre</span><span class="sxs-lookup"><span data-stu-id="9bd6f-103">Step 9: Enable replication to a secondary site for Hyper-V VMs</span></span>


<span data-ttu-id="9bd6f-104">Miután egy replikációs házirend beállítása a jelen cikk segítségével engedélyezze a replikálást egy másodlagos System Center Virtual Machine Manager (VMM) helyre a helyszíni Hyper-V virtuális gépek (VM), használja [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9bd6f-104">After setting up a replication policy, use this article to enable replication to a secondary System Center Virtual Machine Manager (VMM) site for on-premises Hyper-V virtual machines (VM), using [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="9bd6f-105">A cikk elolvasása után felmerülő megjegyzéseit alul vagy az [Azure Recovery Services fórumban](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti közzé.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="select-vms-to-replicate"></a><span data-ttu-id="9bd6f-106">Válassza ki a virtuális gépek replikálásához</span><span class="sxs-lookup"><span data-stu-id="9bd6f-106">Select VMs to replicate</span></span>

1. <span data-ttu-id="9bd6f-107">Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-107">Click **Step 2: Replicate application** > **Source**.</span></span> 

    ![A replikáció engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. <span data-ttu-id="9bd6f-109">A **forrás**, válassza ki a VMM-kiszolgáló és a felhőben, amelyben a replikálni kívánt Hyper-V-gazdagépek találhatók.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-109">In **Source**, select the VMM server, and the cloud in which the Hyper-V hosts you want to replicate are located.</span></span> <span data-ttu-id="9bd6f-110">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-110">Then click **OK**.</span></span>

    ![A replikáció engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. <span data-ttu-id="9bd6f-112">A **cél**, ellenőrizze a másodlagos VMM-kiszolgáló és a felhőben.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-112">In **Target**, verify the secondary VMM server and cloud.</span></span>
4. <span data-ttu-id="9bd6f-113">A **virtuális gépek**, válassza ki a listából védeni kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-113">In **Virtual machines**, select the VMs you want to protect from the list.</span></span>

    ![A virtuális gép védelmének engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

<span data-ttu-id="9bd6f-115">Előrehaladásának nyomon követheti a **Védelemengedélyezési** műveletét **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-115">You can track progress of the **Enable Protection** action in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="9bd6f-116">Miután a **Védelemvéglegesítési** feladat befejeződik, a kezdeti replikálás befejeződik, és a virtuális gép készen áll a feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-116">After the **Finalize Protection** job completes, the initial replication is complete, and the virtual machine is ready for failover.</span></span>

<span data-ttu-id="9bd6f-117">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="9bd6f-117">Note that:</span></span>

- <span data-ttu-id="9bd6f-118">A VMM-konzol virtuális gépek védelmét is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-118">You can also enable protection for virtual machines in the VMM console.</span></span> <span data-ttu-id="9bd6f-119">Kattintson a **Védelemengedélyezési** az eszköztáron a virtuális gép tulajdonságai > **Azure Site Recovery** fülre.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-119">Click **Enable Protection** on the toolbar in the virtual machine properties > **Azure Site Recovery** tab.</span></span>
- <span data-ttu-id="9bd6f-120">Replikáció engedélyezése után megtekintheti a virtuális gép a Tulajdonságok **replikált elemek**.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-120">After you've enabled replication, you can view properties for the VM in **Replicated Items**.</span></span> <span data-ttu-id="9bd6f-121">Az a **Essentials** irányítópult, láthatja, hogy a virtuális gép és annak állapotát a replikációs házirend kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-121">On the **Essentials** dashboard, you can see information about the replication policy for the VM and its status.</span></span> <span data-ttu-id="9bd6f-122">Kattintson a **tulajdonságok** további részleteket.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-122">Click **Properties** for more details.</span></span>

## <a name="onboard-existing-vms"></a><span data-ttu-id="9bd6f-123">A bevezetni meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="9bd6f-123">Onboard existing VMs</span></span>

<span data-ttu-id="9bd6f-124">Ha meglévő virtuális gépek a VMM Alkalmazásban – a Hyper-V replika replikálja, hogy discoveryt őket az Azure Site Recovery replikációs az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9bd6f-124">If you have existing virtual machines in VMM that are replicating with Hyper-V Replica, you can onboard them for Azure Site Recovery replication as follows:</span></span>

1. <span data-ttu-id="9bd6f-125">Győződjön meg arról, hogy a Hyper-V kiszolgáló, a meglévő virtuális Gépet üzemeltető az elsődleges felhőben található, és, hogy a Hyper-V kiszolgáló, a replika virtuális gépet szolgáltató a másodlagos felhőben található.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-125">Ensure that the Hyper-V server hosting the existing VM is located in the primary cloud, and that the Hyper-V server hosting the replica virtual machine is located in the secondary cloud.</span></span>
2. <span data-ttu-id="9bd6f-126">Győződjön meg arról, hogy a replikációs házirend úgy van konfigurálva, az elsődleges VMM-felhő.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-126">Make sure a replication policy is configured for the primary VMM cloud.</span></span>
3. <span data-ttu-id="9bd6f-127">Engedélyezze az elsődleges virtuális gép replikálását.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-127">Enable replication for the primary virtual machine.</span></span> <span data-ttu-id="9bd6f-128">Az Azure Site Recovery és VMM győződjön meg arról, hogy az azonos másodpéldány-állomás és a virtuális gép észlel, és Azure Site Recovery lesznek ismét felhasználni, és a megadott beállítások replikálási szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9bd6f-128">Azure Site Recovery and VMM ensure that the same replica host and virtual machine is detected, and Azure Site Recovery will reuse and reestablish replication using the specified settings.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9bd6f-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9bd6f-129">Next steps</span></span>

<span data-ttu-id="9bd6f-130">Ugrás a [10. lépés: feladatátvételi teszt futtatása](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="9bd6f-130">Go to [Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
