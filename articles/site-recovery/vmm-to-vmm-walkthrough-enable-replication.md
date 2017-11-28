---
title: "Hyper-V aaaEnable replikációs tooa másodlagos helyet az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Ismerteti, hogyan tooenable tooa replikálásához a Hyper-V virtuális gépek replikálása másodlagos System Center VMM helyet az Azure Site Recovery szolgáltatással."
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
ms.openlocfilehash: b4484e0118cb23f343187fe867d9795d30926baf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-enable-replication-tooa-secondary-site-for-hyper-v-vms"></a><span data-ttu-id="616e2-103">9. lépés: A Hyper-V virtuális gépek replikációs tooa másodlagos hely engedélyezése</span><span class="sxs-lookup"><span data-stu-id="616e2-103">Step 9: Enable replication tooa secondary site for Hyper-V VMs</span></span>


<span data-ttu-id="616e2-104">Miután beállította a replikációs házirendet, helyszíni Hyper-V virtuális gépek (VM), ez a cikk tooenable replikációs tooa másodlagos System Center Virtual Machine Manager (VMM) helyet használja [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="616e2-104">After setting up a replication policy, use this article tooenable replication tooa secondary System Center Virtual Machine Manager (VMM) site for on-premises Hyper-V virtual machines (VM), using [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="616e2-105">A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="616e2-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="select-vms-tooreplicate"></a><span data-ttu-id="616e2-106">Válassza ki a virtuális gépek tooreplicate</span><span class="sxs-lookup"><span data-stu-id="616e2-106">Select VMs tooreplicate</span></span>

1. <span data-ttu-id="616e2-107">Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="616e2-107">Click **Step 2: Replicate application** > **Source**.</span></span> 

    ![A replikáció engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. <span data-ttu-id="616e2-109">A **forrás**, válassza ki a VMM-kiszolgáló hello és hello felhő, mely hello tooreplicate kívánt Hyper-V-gazdagépek találhatók.</span><span class="sxs-lookup"><span data-stu-id="616e2-109">In **Source**, select hello VMM server, and hello cloud in which hello Hyper-V hosts you want tooreplicate are located.</span></span> <span data-ttu-id="616e2-110">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="616e2-110">Then click **OK**.</span></span>

    ![A replikáció engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. <span data-ttu-id="616e2-112">A **cél**, ellenőrizze a hello másodlagos VMM-kiszolgáló és a felhő.</span><span class="sxs-lookup"><span data-stu-id="616e2-112">In **Target**, verify hello secondary VMM server and cloud.</span></span>
4. <span data-ttu-id="616e2-113">A **virtuális gépek**, válassza ki a kívánt hello listából tooprotect hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="616e2-113">In **Virtual machines**, select hello VMs you want tooprotect from hello list.</span></span>

    ![A virtuális gép védelmének engedélyezése](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

<span data-ttu-id="616e2-115">Hello állapotának nyomon követheti **Védelemengedélyezési** műveletét **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="616e2-115">You can track progress of hello **Enable Protection** action in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="616e2-116">Hello után **Védelemvéglegesítési** feladat befejeződik, hello kezdeti replikálás befejeződik, és hello virtuális gép készen áll a feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="616e2-116">After hello **Finalize Protection** job completes, hello initial replication is complete, and hello virtual machine is ready for failover.</span></span>

<span data-ttu-id="616e2-117">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="616e2-117">Note that:</span></span>

- <span data-ttu-id="616e2-118">Hello VMM-konzol virtuális gépek védelmét is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="616e2-118">You can also enable protection for virtual machines in hello VMM console.</span></span> <span data-ttu-id="616e2-119">Kattintson a **Védelemengedélyezési** hello eszköztár hello virtuális gép tulajdonságai > **Azure Site Recovery** lapon.</span><span class="sxs-lookup"><span data-stu-id="616e2-119">Click **Enable Protection** on hello toolbar in hello virtual machine properties > **Azure Site Recovery** tab.</span></span>
- <span data-ttu-id="616e2-120">Replikáció engedélyezése után megtekintheti a hello virtuális gép tulajdonságait a **replikált elemek**.</span><span class="sxs-lookup"><span data-stu-id="616e2-120">After you've enabled replication, you can view properties for hello VM in **Replicated Items**.</span></span> <span data-ttu-id="616e2-121">A hello **Essentials** az irányítópult hello replikációs házirend hello virtuális gép és annak állapotát kapcsolatos információkat tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="616e2-121">On hello **Essentials** dashboard, you can see information about hello replication policy for hello VM and its status.</span></span> <span data-ttu-id="616e2-122">Kattintson a **tulajdonságok** további részleteket.</span><span class="sxs-lookup"><span data-stu-id="616e2-122">Click **Properties** for more details.</span></span>

## <a name="onboard-existing-vms"></a><span data-ttu-id="616e2-123">A bevezetni meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="616e2-123">Onboard existing VMs</span></span>

<span data-ttu-id="616e2-124">Ha meglévő virtuális gépek a VMM Alkalmazásban – a Hyper-V replika replikálja, hogy discoveryt őket az Azure Site Recovery replikációs az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="616e2-124">If you have existing virtual machines in VMM that are replicating with Hyper-V Replica, you can onboard them for Azure Site Recovery replication as follows:</span></span>

1. <span data-ttu-id="616e2-125">Győződjön meg arról, hogy hello hello elsődleges felhőben található meglévő Virtuálisgép üzemeltetési hello Hyper-V kiszolgálón, és hello replika virtuális gépet szolgáltató hello Hyper-V kiszolgálón hello másodlagos felhőben található.</span><span class="sxs-lookup"><span data-stu-id="616e2-125">Ensure that hello Hyper-V server hosting hello existing VM is located in hello primary cloud, and that hello Hyper-V server hosting hello replica virtual machine is located in hello secondary cloud.</span></span>
2. <span data-ttu-id="616e2-126">Ellenőrizze, hogy a replikációs házirend hello elsődleges VMM-felhőben van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="616e2-126">Make sure a replication policy is configured for hello primary VMM cloud.</span></span>
3. <span data-ttu-id="616e2-127">Engedélyezze a hello elsődleges virtuális gép replikációját.</span><span class="sxs-lookup"><span data-stu-id="616e2-127">Enable replication for hello primary virtual machine.</span></span> <span data-ttu-id="616e2-128">Az Azure Site Recovery és a VMM győződjön meg arról, hogy hello azonos másodpéldány-állomás és a virtuális gép észlel, és Azure Site Recovery szeretné újrafelhasználni újra létrehozni a replikációs hello segítségével megadott beállítások.</span><span class="sxs-lookup"><span data-stu-id="616e2-128">Azure Site Recovery and VMM ensure that hello same replica host and virtual machine is detected, and Azure Site Recovery will reuse and reestablish replication using hello specified settings.</span></span>


## <a name="next-steps"></a><span data-ttu-id="616e2-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="616e2-129">Next steps</span></span>

<span data-ttu-id="616e2-130">Nyissa meg túl[10. lépés: feladatátvételi teszt futtatása](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="616e2-130">Go too[Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
