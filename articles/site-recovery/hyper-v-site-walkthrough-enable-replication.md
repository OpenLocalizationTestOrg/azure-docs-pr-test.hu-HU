---
title: "Hyper-V virtuális gépek replikálása az Azure Site Recovery (a System Center VMM nélkül) tooAzure aaaEnable replikációs |} Microsoft Docs"
description: "Összefoglalja a hello lépéseit tooenable replikációs tooAzure hello Azure Site Recovery szolgáltatással a Hyper-V virtuális gépek"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1589cb7aa1fe954e075cb7bf1f4a4ec199ed3ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-tooazure"></a><span data-ttu-id="772df-103">10. lépés: Hyper-V virtuális gépek replikálása tooAzure replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="772df-103">Step 10: Enable replication for Hyper-V VMs replicating tooAzure</span></span>


<span data-ttu-id="772df-104">Ez a cikk ismerteti, hogyan tooenable replikálása a helyszíni Hyper-V virtuális gépek (System Center VMM által nem felügyelt) tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="772df-104">This article describes how tooenable replication for on-premises Hyper-V virtual machines (not managed by System Center VMM) tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="772df-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="772df-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="before-you-start"></a><span data-ttu-id="772df-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="772df-106">Before you start</span></span>

<span data-ttu-id="772df-107">Mielőtt elkezdené, győződjön meg arról, hogy az Azure felhasználói fiók rendelkezik-e a szükséges hello [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy új virtuális gép tooAzure tooenable replikációját.</span><span class="sxs-lookup"><span data-stu-id="772df-107">Before you start, ensure that your Azure user account has hello required [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of a new virtual machine tooAzure.</span></span>

## <a name="exclude-disks-from-replication"></a><span data-ttu-id="772df-108">Lemezek kizárása a replikációból</span><span class="sxs-lookup"><span data-stu-id="772df-108">Exclude disks from replication</span></span>

<span data-ttu-id="772df-109">Alapértelmezés szerint az összes lemez gépen replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="772df-109">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="772df-110">Lemezek lehet kizárni a replikálásból.</span><span class="sxs-lookup"><span data-stu-id="772df-110">You can exclude disks from replication.</span></span> <span data-ttu-id="772df-111">Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb).</span><span class="sxs-lookup"><span data-stu-id="772df-111">For example you might not want tooreplicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="772df-112">További információ</span><span class="sxs-lookup"><span data-stu-id="772df-112">Learn more</span></span>](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a><span data-ttu-id="772df-113">Virtuális gépek replikálása</span><span class="sxs-lookup"><span data-stu-id="772df-113">Replicate VMs</span></span>

<span data-ttu-id="772df-114">Engedélyezze a virtuális gépek replikálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="772df-114">Enable replication for VMs as follows:</span></span>          

1. <span data-ttu-id="772df-115">Kattintson a **alkalmazás replikálása** > **forrás**.</span><span class="sxs-lookup"><span data-stu-id="772df-115">Click **Replicate application** > **Source**.</span></span> <span data-ttu-id="772df-116">Először hello replikációjának beállítása után kattintson **+ replikálás** a további gépek replikációjának tooenable.</span><span class="sxs-lookup"><span data-stu-id="772df-116">After you've set up replication for hello first time, you can click **+Replicate** tooenable replication for additional machines.</span></span>

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. <span data-ttu-id="772df-118">A **forrás**, jelölje be hello Hyper-V helyet.</span><span class="sxs-lookup"><span data-stu-id="772df-118">In **Source**, select hello Hyper-V site.</span></span> <span data-ttu-id="772df-119">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="772df-119">Then click **OK**.</span></span>
3. <span data-ttu-id="772df-120">A **cél**, válasszon hello tároló előfizetést és hello feladatátvételi típusú a toouse (klasszikus vagy erőforrás-kezelés) Azure-ban a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="772df-120">In **Target**, select hello vault subscription, and hello failover model you want toouse in Azure (classic or resource management) after failover.</span></span>
4. <span data-ttu-id="772df-121">Válassza ki a kívánt toouse hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="772df-121">Select hello storage account you want toouse.</span></span> <span data-ttu-id="772df-122">Ha azt szeretné, hogy toouse fiók nem rendelkezik, akkor [hozzon létre egyet](#set-up-an-azure-storage-account).</span><span class="sxs-lookup"><span data-stu-id="772df-122">If you don't have an account you want toouse, you can [create one](#set-up-an-azure-storage-account).</span></span> <span data-ttu-id="772df-123">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="772df-123">Then click **OK**.</span></span>
5. <span data-ttu-id="772df-124">SELECT hello Azure hálózati és alhálózati toowhich Azure virtuális gépeken fog csatlakozni, ha feladatátvételi jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="772df-124">Select hello Azure network and subnet toowhich Azure VMs will connect when they're created failover.</span></span>

    - <span data-ttu-id="772df-125">Válassza ki a tooapply hello hálózati beállítások tooall gépek engedélyezi a replikáció, **beállítás most a kijelölt gépekhez**.</span><span class="sxs-lookup"><span data-stu-id="772df-125">tooapply hello network settings tooall machines you enable for replication, select **Configure now for selected machines**.</span></span>
    - <span data-ttu-id="772df-126">Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="772df-126">Select **Configure later** tooselect hello Azure network per machine.</span></span>
    - <span data-ttu-id="772df-127">Ha azt szeretné, hogy toouse hálózati nincs, akkor [hozzon létre egyet](#set-up-an-azure-network).</span><span class="sxs-lookup"><span data-stu-id="772df-127">If you don't have a network you want toouse, you can [create one](#set-up-an-azure-network).</span></span> <span data-ttu-id="772df-128">Ha szükséges, válassza ki az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="772df-128">Select a subnet if applicable.</span></span> <span data-ttu-id="772df-129">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="772df-129">Then click **OK**.</span></span>

   ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. <span data-ttu-id="772df-131">A **virtuális gépek** > **válassza ki a virtuális gépek**kattintson, és válassza az egyes gépek tooreplicate szeretné.</span><span class="sxs-lookup"><span data-stu-id="772df-131">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="772df-132">Csak olyan gépeket választhat, amelyeken használható a replikáció funkció.</span><span class="sxs-lookup"><span data-stu-id="772df-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="772df-133">Végül kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="772df-133">Then click **OK**.</span></span>

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. <span data-ttu-id="772df-135">A **tulajdonságok** > **tulajdonságainak konfigurálása**hello operációs rendszert válasszon ki a kijelölt hello virtuális gépek és operációsrendszer-lemez hello.</span><span class="sxs-lookup"><span data-stu-id="772df-135">In **Properties** > **Configure properties**, select hello operating system for hello selected VMs, and hello OS disk.</span></span>
8. <span data-ttu-id="772df-136">Győződjön meg arról, hogy hello Azure virtuális gép nevét (cél neve) megfelel [Azure virtuális gép követelményeinek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="772df-136">Verify that hello Azure VM name (target name) complies with [Azure virtual machine requirements](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
9. <span data-ttu-id="772df-137">Alapértelmezés szerint összes hello lemeze hello virtuális gép ki van jelölve, a replikáció.</span><span class="sxs-lookup"><span data-stu-id="772df-137">By default all hello disks of hello VM are selected for replication.</span></span> <span data-ttu-id="772df-138">Törölje a jelet lemezek tooexclude őket.</span><span class="sxs-lookup"><span data-stu-id="772df-138">Clear disks tooexclude them.</span></span>
10. <span data-ttu-id="772df-139">Kattintson a **OK** toosave módosításokat.</span><span class="sxs-lookup"><span data-stu-id="772df-139">Click **OK** toosave changes.</span></span> <span data-ttu-id="772df-140">A további tulajdonságokat később is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="772df-140">You can set additional properties later.</span></span>

    ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. <span data-ttu-id="772df-142">A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, válassza ki a kívánt tooapply hello védett virtuális gépek hello replikációs házirend.</span><span class="sxs-lookup"><span data-stu-id="772df-142">In **Replication settings** > **Configure replication settings**, select hello replication policy you want tooapply for hello protected VMs.</span></span> <span data-ttu-id="772df-143">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="772df-143">Then click **OK**.</span></span> <span data-ttu-id="772df-144">Módosíthatja a hello replikációs házirendet a **replikációs házirendek** > házirend neve > **beállításainak szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="772df-144">You can modify hello replication policy in **Replication policies** > policy-name > **Edit Settings**.</span></span> <span data-ttu-id="772df-145">Az itt megadott módosítások a már replikálás alatt álló, illetve újonnan hozzáadott gépekre egyaránt érvényesek lesznek.</span><span class="sxs-lookup"><span data-stu-id="772df-145">Changes you apply will be used for machines that are already replicating, and new machines.</span></span>


   ![A replikáció engedélyezése](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

<span data-ttu-id="772df-147">Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="772df-147">You can track progress of hello **Enable Protection** job in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="772df-148">Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="772df-148">After hello **Finalize Protection** job runs hello machine is ready for failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="772df-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="772df-149">Next steps</span></span>


<span data-ttu-id="772df-150">Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="772df-150">Go too[Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
