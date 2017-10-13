---
title: "a fizikai kiszolgálók replikálása az Azure Site Recovery tooAzure aaaEnable replikációs |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket kell tooenable replikációs tooAzure fizikai kiszolgálók hello Azure Site Recovery szolgáltatással"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: dde4b1463023d2ccefa498f72bb51e57d60ac0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-physical-servers-tooazure"></a><span data-ttu-id="1ca21-103">10. lépés: A fizikai kiszolgálók tooAzure replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1ca21-103">Step 10: Enable replication for physical servers tooAzure</span></span>


<span data-ttu-id="1ca21-104">Ez a cikk ismerteti, hogyan tooenable replikálása a helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1ca21-104">This article describes how tooenable replication for on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="1ca21-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1ca21-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="1ca21-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="1ca21-106">Before you start</span></span>

- <span data-ttu-id="1ca21-107">Kiszolgálóknak rendelkezniük kell hello [telepíteni a mobilitási szolgáltatás összetevőt](physical-walkthrough-install-mobility.md).</span><span class="sxs-lookup"><span data-stu-id="1ca21-107">Servers must have hello [Mobility service component installed](physical-walkthrough-install-mobility.md).</span></span>
- <span data-ttu-id="1ca21-108">A virtuális gépek kész az ügyfélleküldéses telepítést, ha a hello folyamatkiszolgáló automatikusan telepíti az hello mobilitási szolgáltatás, amikor a replikáció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1ca21-108">If a VM is prepared for push installation, hello process server automatically installs hello Mobility service when you enable replication.</span></span>
- <span data-ttu-id="1ca21-109">Ha engedélyezi a gép a replikáció, a Azure felhasználói fiókot kell adott [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooensure képes toouse az Azure storage, és hozzon létre az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="1ca21-109">When you enable a machine for replication, your Azure user account needs specific [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooensure you're able toouse Azure storage, and create Azure VMs.</span></span>
- <span data-ttu-id="1ca21-110">Amikor fel vagy módosít a kiszolgálók IP-címeit, is igénybe vehet fel too15 perc vagy már módosítások tootake hatása, valamint azokat a tooappear hello portálon.</span><span class="sxs-lookup"><span data-stu-id="1ca21-110">When you add or modify IP addresses for servers, it can take up too15 minutes or longer for changes tootake effect, and for them tooappear in hello portal.</span></span>


## <a name="exclude-disks-from-replication"></a><span data-ttu-id="1ca21-111">Lemezek kizárása a replikációból</span><span class="sxs-lookup"><span data-stu-id="1ca21-111">Exclude disks from replication</span></span>

<span data-ttu-id="1ca21-112">Alapértelmezés szerint az összes lemez gépen replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="1ca21-112">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="1ca21-113">Lemezek lehet kizárni a replikálásból.</span><span class="sxs-lookup"><span data-stu-id="1ca21-113">You can exclude disks from replication.</span></span> <span data-ttu-id="1ca21-114">Például előfordulhat, hogy nem kívánja tooreplicate lemezek, amelyek ideiglenes vagy frissítette minden alkalommal, amikor olyan gép, adatok, vagy alkalmazás újraindul, (például pagefile.sys vagy SQL Server tempdb).</span><span class="sxs-lookup"><span data-stu-id="1ca21-114">For example you might not want tooreplicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="1ca21-115">További információ</span><span class="sxs-lookup"><span data-stu-id="1ca21-115">Learn more</span></span>](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a><span data-ttu-id="1ca21-116">Kiszolgálók replikálása</span><span class="sxs-lookup"><span data-stu-id="1ca21-116">Replicate servers</span></span>

1. <span data-ttu-id="1ca21-117">Kattintson **2. lépés: Az alkalmazás replikálása** > **Forrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="1ca21-117">Click **Step 2: Replicate application** > **Source**.</span></span>
2. <span data-ttu-id="1ca21-118">A **forrás**, jelölje be hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="1ca21-118">In **Source**, select hello configuration server.</span></span>
3. <span data-ttu-id="1ca21-119">A **számítógép típusú**, jelölje be **fizikai gépek**.</span><span class="sxs-lookup"><span data-stu-id="1ca21-119">In **Machine type**, select **Physical Machines**.</span></span>
4. <span data-ttu-id="1ca21-120">Hello folyamatkiszolgáló kijelölése.</span><span class="sxs-lookup"><span data-stu-id="1ca21-120">Select hello process server.</span></span> <span data-ttu-id="1ca21-121">Ha még nem hozott létre minden olyan további folyamat kiszolgálóhoz ez lesz hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="1ca21-121">If you haven't created any additional process servers this will be hello configuration server.</span></span> <span data-ttu-id="1ca21-122">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ca21-122">Then click **OK**.</span></span>
5. <span data-ttu-id="1ca21-123">A **cél**válassza ki a hello előfizetés, hello virtuális gépek a feladatátvételt toocreate hello használni kívánt erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="1ca21-123">In **Target**, select hello subscription and hello resource group in which you want toocreate hello failed over VMs.</span></span> <span data-ttu-id="1ca21-124">Válassza ki a beállítani kívánt toouse (klasszikus vagy erőforrás-kezelés), Azure virtuális gépek a feladatátvételt hello hello telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="1ca21-124">Choose hello deployment model that you want toouse in Azure (classic or resource management), for hello failed over VMs.</span></span>
6. <span data-ttu-id="1ca21-125">Válassza ki a kívánt toouse adatok replikálásához hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="1ca21-125">Select hello Azure storage account you want toouse for replicating data.</span></span> <span data-ttu-id="1ca21-126">Ha már beállította fiók toouse nem szeretné, létrehozhat egy újat.</span><span class="sxs-lookup"><span data-stu-id="1ca21-126">If you don't want toouse an account you've already set up, you can create a new one.</span></span>
7. <span data-ttu-id="1ca21-127">Jelölje be hello **Azure hálózati** és **alhálózati** toowhich Azure virtuális gépek a feladatátvételt követően csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="1ca21-127">Select hello **Azure network** and **Subnet** toowhich Azure VMs connect after failover.</span></span> <span data-ttu-id="1ca21-128">Válassza ki **beállítás most a kijelölt gépekhez** tooapply hello beállítás tooall számítógépek választja a védelem.</span><span class="sxs-lookup"><span data-stu-id="1ca21-128">Select **Configure now for selected machines** tooapply hello network setting tooall machines you select for protection.</span></span> <span data-ttu-id="1ca21-129">Válassza ki **konfigurálását később** tooselect hello gépenként Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="1ca21-129">Select **Configure later** tooselect hello Azure network per machine.</span></span> <span data-ttu-id="1ca21-130">Ha egy meglévő hálózati toouse nem szeretné, akkor hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="1ca21-130">If you don't want toouse an existing network, you can create one.</span></span>

    ![A replikáció engedélyezése](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. <span data-ttu-id="1ca21-132">A **fizikai gépek**, kattintson a **+ a fizikai gép** , és írja be a hello **neve** és **IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="1ca21-132">In **Physical machines**, click **+Physical machine** and enter hello **Name** and **IP address**.</span></span> <span data-ttu-id="1ca21-133">Válassza ki az operációs rendszer hello hello gép tooreplicate szeretné.</span><span class="sxs-lookup"><span data-stu-id="1ca21-133">Choose hello operating system of hello machine you want tooreplicate.</span></span> <span data-ttu-id="1ca21-134">Amíg a számítógépek felderítése és hello listán látható néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1ca21-134">It takes a few minutes until machines are discovered and shown in hello list.</span></span>
9. <span data-ttu-id="1ca21-135">A **tulajdonságok** > **tulajdonságainak konfigurálása**, jelölje be hello folyamat server tooautomatically által használt fiók hello hello mobilitási szolgáltatás telepítése hello gépen.</span><span class="sxs-lookup"><span data-stu-id="1ca21-135">In **Properties** > **Configure properties**, select hello account that will be used by hello process server tooautomatically install hello Mobility service on hello machine.</span></span>
10. <span data-ttu-id="1ca21-136">Alapértelmezés szerint a rendszer replikálja az összes lemez.</span><span class="sxs-lookup"><span data-stu-id="1ca21-136">By default all disks are replicated.</span></span> <span data-ttu-id="1ca21-137">Kattintson a **összes lemez** , és törölje a nem kívánt tooreplicate lemezzel.</span><span class="sxs-lookup"><span data-stu-id="1ca21-137">Click **All Disks** and clear any disks you don't want tooreplicate.</span></span> <span data-ttu-id="1ca21-138">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ca21-138">Then click **OK**.</span></span> <span data-ttu-id="1ca21-139">Virtuális gép további tulajdonságokat később állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="1ca21-139">You can set additional VM properties later.</span></span>

    ![A replikáció engedélyezése](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. <span data-ttu-id="1ca21-141">A **replikációs beállítások** > **replikáció beállításainak konfigurálása**, győződjön meg arról, hogy helyes-e replikációs házirend be van jelölve hello.</span><span class="sxs-lookup"><span data-stu-id="1ca21-141">In **Replication settings** > **Configure replication settings**, verify that hello correct replication policy is selected.</span></span> <span data-ttu-id="1ca21-142">Ha módosít egy házirendet, a változások lesznek alkalmazott tooreplicating gép és toonew gépek.</span><span class="sxs-lookup"><span data-stu-id="1ca21-142">If you modify a policy, changes will be applied tooreplicating machine, and toonew machines.</span></span>
12. <span data-ttu-id="1ca21-143">Engedélyezése **virtuális Gépre kiterjedő konzisztencia** Ha toogather gépeihez egy replikációs csoporthoz, és adjon meg egy nevet hello csoportnak.</span><span class="sxs-lookup"><span data-stu-id="1ca21-143">Enable **Multi-VM consistency** if you want toogather machines into a replication group, and specify a name for hello group.</span></span> <span data-ttu-id="1ca21-144">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ca21-144">Then click **OK**.</span></span> <span data-ttu-id="1ca21-145">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="1ca21-145">Note that:</span></span>

    * <span data-ttu-id="1ca21-146">A replikációs csoportok gépek replikálása együtt, és megosztott összeomlás-konzisztens és alkalmazáskonzisztens helyreállítási pontokat, amikor azok a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="1ca21-146">Machines in replication groups replicate together, and have shared crash-consistent and app-consistent recovery points when they fail over.</span></span>
    * <span data-ttu-id="1ca21-147">Azt javasoljuk, hogy gyűjtse össze a fizikai kiszolgálók együtt, hogy látni lehessen a munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="1ca21-147">We recommend that you gather physical servers together so that they mirror your workloads.</span></span> <span data-ttu-id="1ca21-148">Több virtuális Gépre kiterjedő konzisztencia engedélyezése befolyásolhatja a teljesítményt a munkaterhelés, és csak akkor használja, ha a gépek is működnek hello azonos munkaterhelés, és konzisztenciára van szükség.</span><span class="sxs-lookup"><span data-stu-id="1ca21-148">Enabling multi-VM consistency can impact workload performance, and should only be used if machines are running hello same workload and you need consistency.</span></span>

13. <span data-ttu-id="1ca21-149">Kattintson a **engedélyezze a replikálást**.</span><span class="sxs-lookup"><span data-stu-id="1ca21-149">Click **Enable Replication**.</span></span> <span data-ttu-id="1ca21-150">Hello állapotának nyomon követheti **Védelemengedélyezési** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="1ca21-150">You can track progress of hello **Enable Protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span> <span data-ttu-id="1ca21-151">Hello után **Védelemvéglegesítési** feladat futtatása hello gép készen áll a feladatátvételre.</span><span class="sxs-lookup"><span data-stu-id="1ca21-151">After hello **Finalize Protection** job runs hello machine is ready for failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ca21-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ca21-152">Next steps</span></span>

<span data-ttu-id="1ca21-153">Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](physical-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="1ca21-153">Go too[Step 11: Run a test failover](physical-walkthrough-test-failover.md)</span></span>