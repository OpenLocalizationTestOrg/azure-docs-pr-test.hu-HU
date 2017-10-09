---
title: "Azure virtuális gépek tooanother Azure-régió, az Azure Site Recovery aaaEnable replikálása |} Microsoft Docs"
description: "Azure virtuális gépek, hello Azure Site Recovery szolgáltatással tooenable replikációs tooanother Azure-régióban kell hello lépéseket foglalja"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.openlocfilehash: 2fa03db45a18ccb8b9f31ed05589be0dd6d5f031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a><span data-ttu-id="61ffd-103">5. lépés: Azure virtuális gépek replikációjának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="61ffd-103">Step 5: Enable replication for Azure VMs</span></span>


<span data-ttu-id="61ffd-104">Miután beállította a [Recovery Services-tároló](azure-to-azure-walkthrough-vault.md), ez a cikk tooenable replikáció a virtuális gépek (VM), az Azure-régió, tooanother használata [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="61ffd-104">After setting up a [Recovery Services vault](azure-to-azure-walkthrough-vault.md), use this article tooenable replication of virtual machines (VMs), tooanother Azure region, with [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="61ffd-105">tooenable replikációs, akkor állítsa be a forrás és cél beállításai, ellenőrizze a hello replikációs házirend, és válassza ki a kívánt tooreplicate virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="61ffd-105">tooenable replication, you set up source and target settings, verify hello replication policy, and select VMs you want tooreplicate.</span></span>

- <span data-ttu-id="61ffd-106">Amikor befejezi a hello a cikkben az Azure virtuális gépek kell kell replikálni toohello másodlagos Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="61ffd-106">When you finish hello article, your Azure VMs should be replicating toohello secondary Azure region.</span></span>
- <span data-ttu-id="61ffd-107">Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="61ffd-107">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

>[!NOTE]
>
> <span data-ttu-id="61ffd-108">Az Azure virtuális gép replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="61ffd-108">Azure VM replication is currently in preview.</span></span>


## <a name="select-hello-source"></a><span data-ttu-id="61ffd-109">Hello forrás kiválasztása</span><span class="sxs-lookup"><span data-stu-id="61ffd-109">Select hello source</span></span> 

1. <span data-ttu-id="61ffd-110">A Recovery Services-tárolók, kattintson a hello tároló neve > **+ replikálás**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-110">In Recovery Services vaults, click hello vault name > **+Replicate**.</span></span>
2. <span data-ttu-id="61ffd-111">A **forrás**, jelölje be **Azure - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-111">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="61ffd-112">A **adatforrásról**, jelölje be hello forrás Azure-régió, ahol a virtuális gépek futnak.</span><span class="sxs-lookup"><span data-stu-id="61ffd-112">In **Source location**, select hello source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="61ffd-113">Jelölje be hello **Azure virtuális gép üzembe helyezési modellel** virtuális gépek: **erőforrás-kezelő** vagy **klasszikus**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-113">Select hello **Azure virtual machine deployment model** for VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="61ffd-114">Jelölje be hello **forrás-erőforráscsoporton** erőforrás-kezelő virtuális gépekhez, vagy **felhőalapú szolgáltatás** klasszikus virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="61ffd-114">Select hello **Source resource group** for Resource Manager VMs, or **cloud service** for classic VMs.</span></span>
5. <span data-ttu-id="61ffd-115">Kattintson a **OK** toosave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="61ffd-115">Click **OK** toosave hello settings.</span></span>

    ![Állítson be hello forrás](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-hello-vms"></a><span data-ttu-id="61ffd-117">Hello virtuális gépek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="61ffd-117">Select hello VMs</span></span>

<span data-ttu-id="61ffd-118">A Site Recovery lekéri azon virtuális gépek társított hello előfizetés és az erőforrás/felhőszolgáltatás hello listáját.</span><span class="sxs-lookup"><span data-stu-id="61ffd-118">Site Recovery retrieves a list of hello VMs associated with hello subscription and resource group/cloud service.</span></span>

1. <span data-ttu-id="61ffd-119">A **virtuális gépek**, válassza ki a kívánt tooreplicate hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="61ffd-119">In **Virtual Machines**, select hello VMs you want tooreplicate.</span></span>
2. <span data-ttu-id="61ffd-120">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="61ffd-120">Click **OK**.</span></span>

    ![Válassza ki a virtuális gépek](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a><span data-ttu-id="61ffd-122">Beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="61ffd-122">Configure settings</span></span>

<span data-ttu-id="61ffd-123">A Site Recovery kiépítését hello cél régió (hello forrásbeállítások régió alapján), az alapértelmezett beállításait és hello replikációs házirendhez:</span><span class="sxs-lookup"><span data-stu-id="61ffd-123">Site Recovery provisions default settings for hello target region (based on hello source region settings), and hello replication policy:</span></span>

   - <span data-ttu-id="61ffd-124">**Célhelye**: hello cél régió vész-helyreállítási toouse használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="61ffd-124">**Target location**: hello target region you want toouse for disaster recovery.</span></span> <span data-ttu-id="61ffd-125">Azt javasoljuk, hogy hello célhelye megegyezik-e a Site Recovery-tároló hello hello helyét.</span><span class="sxs-lookup"><span data-stu-id="61ffd-125">We recommend that hello target location matches hello location of hello Site Recovery vault.</span></span>
   - <span data-ttu-id="61ffd-126">**Célként megadott erőforráscsoportja**: erőforrás csoport toowhich hello cél régióban Azure virtuális gépek a feladatátvételt követően fog tartozni.</span><span class="sxs-lookup"><span data-stu-id="61ffd-126">**Target resource group**: Resource group toowhich Azure VMs in hello target region will belong after failover.</span></span> <span data-ttu-id="61ffd-127">Alapértelmezés szerint a Site Recovery hello cél régióban az "automatikus" utótaggal rendelkező egy új erőforráscsoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="61ffd-127">By default, Site Recovery creates a new resource group in hello target region with an "asr" suffix.</span></span> 
   - <span data-ttu-id="61ffd-128">**Cél virtuális hálózati**: hello hálózati az Azure virtuális gépek mely hello a cél régióban található feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="61ffd-128">**Target virtual network**: hello network in which Azure VMs in hello target region will be located after failover.</span></span> <span data-ttu-id="61ffd-129">Alapértelmezés szerint a Site Recovery hoz létre egy új virtuális hálózat (és alhálózatok) az "automatikus" utótaggal rendelkező hello cél régióban.</span><span class="sxs-lookup"><span data-stu-id="61ffd-129">By default, Site Recovery creates a new virtual network (and subnets) in hello target region with an "asr" suffix.</span></span> <span data-ttu-id="61ffd-130">Ehhez a hálózathoz csatlakoztatott tooyour Forráshálózat.</span><span class="sxs-lookup"><span data-stu-id="61ffd-130">This network is mapped tooyour source network.</span></span> <span data-ttu-id="61ffd-131">Vegye figyelembe, hogy a virtuális gépek a feladatátvételt követően rendelhet egy adott IP-cím, ha szüksége tooretain hello azonos IP-cím hello forrás- és a célként megadott helyen.</span><span class="sxs-lookup"><span data-stu-id="61ffd-131">Note that you can assign a specific IP address after failover of a VM, if you need tooretain hello same IP address in hello source and target locations.</span></span> 
   - <span data-ttu-id="61ffd-132">**Storage-fiókok gyorsítótár**: a Site Recovery storage-fiókot használ hello forrás régióban.</span><span class="sxs-lookup"><span data-stu-id="61ffd-132">**Cache storage accounts**: Site Recovery uses a storage account in hello source region.</span></span> <span data-ttu-id="61ffd-133">A forrás virtuális gépeken módosítások előtt replikációs toohello célhelyet küldött toothis fiók.</span><span class="sxs-lookup"><span data-stu-id="61ffd-133">Changes on source VMs are sent toothis account, before replication toohello target location.</span></span> 
   - <span data-ttu-id="61ffd-134">**Storage-fiókok cél**: alapértelmezés szerint a Site Recovery hoz létre egy új tárfiókot hello cél régióban, toomirror hello forrás virtuális gép tárfiók.</span><span class="sxs-lookup"><span data-stu-id="61ffd-134">**Target storage accounts**: By default, Site Recovery creates a new storage account in hello target region, toomirror hello source VM storage account.</span></span>
   -  <span data-ttu-id="61ffd-135">**Rendelkezésre állási készletek cél**: alapértelmezés szerint a Site Recovery hoz létre egy új rendelkezésre állási csoportban, hello cél régióban, hello "automatikus" utótaggal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="61ffd-135">**Target availability sets**: By default, Site Recovery creates a new availability set in hello target region, with hello "asr" suffix.</span></span> 
   - <span data-ttu-id="61ffd-136">**Replikációs házirend neve**: a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="61ffd-136">**Replication policy name**: Policy name.</span></span>
   - <span data-ttu-id="61ffd-137">**Helyreállítási pontok megőrzésének ideje**: alapértelmezés szerint a Site Recovery helyreállítási pontok tartja 24 órán át.</span><span class="sxs-lookup"><span data-stu-id="61ffd-137">**Recovery point retention**: By default Site Recovery keeps recovery points for 24 hours.</span></span> <span data-ttu-id="61ffd-138">Beállíthatja, hogy egy 1 és 72 óra közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="61ffd-138">You can configure a value between 1 and 72 hours.</span></span>
   - <span data-ttu-id="61ffd-139">**Alkalmazáskonzisztens pillanatkép gyakorisága**: alapértelmezés szerint a Site Recovery pillanatfelvételt egy alkalmazáskonzisztens 4 óránként.</span><span class="sxs-lookup"><span data-stu-id="61ffd-139">**App-consistent snapshot frequency**: By default Site Recovery takes an app-consistent snapshot every 4 hours.</span></span> <span data-ttu-id="61ffd-140">Beállíthatja, hogy bármely érték 1 és 12 óra között.</span><span class="sxs-lookup"><span data-stu-id="61ffd-140">You can configure any value between 1 and 12 hours.</span></span> <span data-ttu-id="61ffd-141">Folyamatosan replikált adatokat:</span><span class="sxs-lookup"><span data-stu-id="61ffd-141">Data is replicated continuously:</span></span>
    - <span data-ttu-id="61ffd-142">Összeomlás-konzisztens helyreállítási pontot karbantartása azonos adatokat írási magasrendű létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="61ffd-142">Crash-consistent recovery points maintain consistent data write-order when created.</span></span> <span data-ttu-id="61ffd-143">Ez a helyreállítási pont típus általában elegendő, ha az alkalmazás egy összeomlási adatok inkonzisztenciát nélkül a tervezett toorecover</span><span class="sxs-lookup"><span data-stu-id="61ffd-143">This type of recovery point is usually sufficient if your app is designed toorecover from a crash without data inconsistencies</span></span>
    - <span data-ttu-id="61ffd-144">Összeomlás-konzisztens helyreállítási pontjai akkor jönnek létre, néhány percenként.</span><span class="sxs-lookup"><span data-stu-id="61ffd-144">Crash-consistent recovery points are generated every few minutes.</span></span> <span data-ttu-id="61ffd-145">A helyreállítási pontok toofail protokollt használó, és állítsa a virtuális gépek biztosít a helyreállítási pont célkitűzés (RPO) hello sorrendben perc alatt.</span><span class="sxs-lookup"><span data-stu-id="61ffd-145">Using these recovery points toofail over and recover your VMs provides a Recovery Point Objective (RPO) in hello order of minutes.</span></span>
    - <span data-ttu-id="61ffd-146">Alkalmazáskonzisztens helyreállítási pontokat (a hozzáadása toowrite magasrendű konzisztencia) Győződjön meg arról, hogy futó alkalmazások végrehajtani minden műveletnél ürítése pufferek toodisk (alkalmazás leépítése).</span><span class="sxs-lookup"><span data-stu-id="61ffd-146">App-consistent recovery points (in addition toowrite-order consistency) ensure that running apps complete all operations and flush buffers toodisk (application quiescing).</span></span> <span data-ttu-id="61ffd-147">Azt javasoljuk, hogy a helyreállítási pontok adatbázis-alkalmazások, például Exchange, SQL Server és Oracle használja.</span><span class="sxs-lookup"><span data-stu-id="61ffd-147">We recommend you use these recovery points for database apps such as SQL Server, Oracle, and Exchange.</span></span>
        
    ![Beállítások konfigurálása](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a><span data-ttu-id="61ffd-149">Beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="61ffd-149">Modify settings</span></span>

<span data-ttu-id="61ffd-150">Ha azt szeretné, hogy toomodify cél- és a replikációs házirend-beállítások, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="61ffd-150">If you want toomodify target and replication policy settings, do hello following:</span></span>

1. <span data-ttu-id="61ffd-151">tooview vagy a célként megadott beállítások módosításához kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-151">tooview or modify target settings, click **Settings**.</span></span>
2. <span data-ttu-id="61ffd-152">Kattintson a toooverride hello alapértelmezett tárolóbeállítások **Testreszabás**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-152">toooverride hello default target settings, click **Customize**.</span></span> <span data-ttu-id="61ffd-153">A célként megadott erőforráscsoportja, a virtuális hálózat, a rendelkezésre állási csoport és a cél tárfiók is megadhat.</span><span class="sxs-lookup"><span data-stu-id="61ffd-153">You can specify a target resource group, virtual network, availability set, and target storage account.</span></span> <span data-ttu-id="61ffd-154">Rendelkezésre állási csoportok csak virtuális gépek részei egy készlet hello forrás régióban adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="61ffd-154">You can only add availability sets if VMs are part of a set in hello source region.</span></span>

    ![Beállítások konfigurálása](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. <span data-ttu-id="61ffd-156">Kattintson a helyreállítási pontokhoz, illetve az alkalmazáskonzisztens pillanatképek toooverride replikációs beállításainak **Testreszabás** következő túl**replikációs házirend**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-156">toooverride replication settings for recovery points and app-consistent snapshots, click **Customize** next too**Replication Policy**.</span></span>
 
    ![Beállítások konfigurálása](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. <span data-ttu-id="61ffd-158">Kattintson a toostart létesítési hello tároló erőforrásait, **célerőforrások létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-158">toostart provisioning hello target resources, click **Create target resources**.</span></span> <span data-ttu-id="61ffd-159">Kiépítés egy percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="61ffd-159">Provisioning takes a minute or so.</span></span> <span data-ttu-id="61ffd-160">Ne zárja be a panelt hello kiépítése során, vagy konfigurálnia kell toostart keresztül.</span><span class="sxs-lookup"><span data-stu-id="61ffd-160">Don't close hello blade during provisioning, or you'll have toostart over.</span></span>




## <a name="enable-replication"></a><span data-ttu-id="61ffd-161">A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="61ffd-161">Enable replication</span></span>

1. <span data-ttu-id="61ffd-162">A **beállítások**, kattintson a **engedélyezze a replikálást**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-162">In **Settings**, click **Enable replication**.</span></span> <span data-ttu-id="61ffd-163">Ez lehetővé teszi a kiválasztott virtuális gépek hello kezdeti replikálása.</span><span class="sxs-lookup"><span data-stu-id="61ffd-163">This enables initial replication of hello VMs you selected.</span></span> <span data-ttu-id="61ffd-164">Kezdeti replikáció állapotát is igénybe vehet néhány alkalommal toorefresh.</span><span class="sxs-lookup"><span data-stu-id="61ffd-164">Initial replication status might take some time toorefresh.</span></span> <span data-ttu-id="61ffd-165">Kattintson a **frissítése** tooget hello legutóbbi állapota.</span><span class="sxs-lookup"><span data-stu-id="61ffd-165">Click **Refresh** tooget hello latest status.</span></span>

2. <span data-ttu-id="61ffd-166">Hello állapotának nyomon követheti **engedélyezni a védelmet** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="61ffd-166">You can track progress of hello **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

3. <span data-ttu-id="61ffd-167">A **beállítások** > **replikált elemek**, virtuális gépek hello állapotát tekintheti meg és hello a kezdeti replikáció folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="61ffd-167">In **Settings** > **Replicated Items**, you can view hello status of VMs and hello initial replication progress.</span></span> <span data-ttu-id="61ffd-168">Kattintson a hello VM toodrill le azokat a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="61ffd-168">Click hello VM toodrill down into its settings.</span></span>



## <a name="next-steps"></a><span data-ttu-id="61ffd-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61ffd-169">Next steps</span></span>

<span data-ttu-id="61ffd-170">Nyissa meg túl[6. lépés: feladatátvételi teszt futtatása](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="61ffd-170">Go too[Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>
