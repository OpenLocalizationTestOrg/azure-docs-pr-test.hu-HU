---
title: "Azure virtuális gépek replikálása Azure-régiók közötti, a vész helyreállítási igényeket: Azure tooAzure |} Microsoft Docs"
description: "Tooreplicate Azure virtuális gépek Azure-régiók (Azure-Azure) hello Azure Site Recovery szolgáltatásban a vész-helyreállítási igényekre között kell hello lépéseket foglalja össze."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: c4c425af260a9bcc3dd4dcc13da26109e20f03bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="89526-103">Azure virtuális gépek replikálása az Azure Site Recovery régiók között</span><span class="sxs-lookup"><span data-stu-id="89526-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

>[!NOTE]
>
> <span data-ttu-id="89526-104">Az Azure virtuális gépek (VM) az Azure Site Recovery replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="89526-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

<span data-ttu-id="89526-105">Ez a cikk ismerteti, hogyan tooreplicate Azure virtuális gépek használatával Azure-régiók közötti hello [Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="89526-105">This article describes how tooreplicate Azure VMs between Azure regions by using hello [Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="89526-106">Ez a cikk vagy hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="89526-106">Post comments and questions at hello bottom of this article or on hello [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="disaster-recovery-in-azure"></a><span data-ttu-id="89526-107">Az Azure-ban katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="89526-107">Disaster recovery in Azure</span></span>

<span data-ttu-id="89526-108">Beépített Azure infrastruktúra-szolgáltatásait és funkcióit hozzájárul az Azure virtuális gépeken futó alkalmazások és szolgáltatások tooa hatékony és rugalmas rendelkezésre állási stratégiát.</span><span class="sxs-lookup"><span data-stu-id="89526-108">Built-in Azure infrastructure capabilities and features contribute tooa robust and resilient availability strategy for workloads that run on Azure VMs.</span></span> <span data-ttu-id="89526-109">Vannak azonban számos oka lehet, miért van szüksége tooplan vész-helyreállítási Azure-régiók közötti saját kezűleg is:</span><span class="sxs-lookup"><span data-stu-id="89526-109">However, there are many reasons why you need tooplan for disaster recovery between Azure regions yourself:</span></span>

- <span data-ttu-id="89526-110">Bizonyos alkalmazások és munkafolyamatok, amelyek üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia kérése toomeet megfelelőségi irányelveinek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="89526-110">You need toomeet compliance guidelines for specific apps and workloads that require a business continuity and disaster recovery (BCDR) strategy.</span></span>
- <span data-ttu-id="89526-111">Szeretné, hogy hello képességét tooprotect és helyreállítása Azure virtuális gépeken, az üzleti döntések alapján, és nem csak a beépített Azure funkciókon alapulnak.</span><span class="sxs-lookup"><span data-stu-id="89526-111">You want hello ability tooprotect and recover Azure VMs based on your business decisions, and not only based on inbuilt Azure functionality.</span></span>
- <span data-ttu-id="89526-112">Az üzleti és megfelelőségi igényeinek érintő forgalomkiesés nélkül éles megfelelően kell tootest feladatátvételét és helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="89526-112">You need tootest failover and recovery in accordance with your business and compliance needs, with no impact on production.</span></span>
- <span data-ttu-id="89526-113">Toofail toohello helyreállítási régió egy olyan vészhelyzet esetén a hello eseményben van szükségük, és hátsó toohello eredeti forrás régió zökkenőmentesen sikertelen.</span><span class="sxs-lookup"><span data-stu-id="89526-113">You need toofail over toohello recovery region in hello event of a disaster and fail back toohello original source region seamlessly.</span></span>

<span data-ttu-id="89526-114">A Site Recovery használata az Azure-Azure virtuális gép replikációs toohelp végre ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="89526-114">Use Site Recovery for Azure-to-Azure VM replication toohelp you do all these tasks.</span></span>


## <a name="why-use-site-recovery"></a><span data-ttu-id="89526-115">Miért előnyös a Site Recovery használata?</span><span class="sxs-lookup"><span data-stu-id="89526-115">Why use Site Recovery?</span></span>      

<span data-ttu-id="89526-116">A Site Recovery Azure virtuális gépek biztosít egy egyszerű módon tooreplicate régiók között:</span><span class="sxs-lookup"><span data-stu-id="89526-116">Site Recovery provides a simple way tooreplicate Azure VMs between regions:</span></span>

- <span data-ttu-id="89526-117">**Automatikus központi telepítési**.</span><span class="sxs-lookup"><span data-stu-id="89526-117">**Automatic deployment**.</span></span> <span data-ttu-id="89526-118">Az aktív-aktív replikációs modell eltérően nincs szükség egy összetett és költséges infrastruktúra hello másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="89526-118">Unlike an active-active replication model, there's no need for an expensive and complex infrastructure in hello secondary region.</span></span> <span data-ttu-id="89526-119">Ha engedélyezi a replikációt, Site Recovery automatikusan létrehoz hello szükséges erőforrások hello cél régióban, forrás régió beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="89526-119">When you enable replication, Site Recovery automatically creates hello required resources in hello target region, based on source region settings.</span></span>
- <span data-ttu-id="89526-120">**Szabályozza a régiók**.</span><span class="sxs-lookup"><span data-stu-id="89526-120">**Control regions**.</span></span> <span data-ttu-id="89526-121">A Site Recovery replikálhatja a kontinensen belül bármely régióban tooany régióban.</span><span class="sxs-lookup"><span data-stu-id="89526-121">With Site Recovery, you can replicate from any region tooany region within a continent.</span></span> <span data-ttu-id="89526-122">Hasonlítsa ezt össze írásvédett georedundáns tárolás, amelyek szabványos közötti aszinkron módon replikál [régiók párosítva](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) csak.</span><span class="sxs-lookup"><span data-stu-id="89526-122">Compare this with read-access geo-redundant storage, which replicates asynchronously between standard [paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) only.</span></span> <span data-ttu-id="89526-123">Írásvédett georedundáns tárolás adja meg a csak olvasási hozzáféréssel toohello adatait hello cél régióban.</span><span class="sxs-lookup"><span data-stu-id="89526-123">Read-access geo-redundant storage provides read-only access toohello data in hello target region.</span></span>
- <span data-ttu-id="89526-124">**Replikációs automatikus**.</span><span class="sxs-lookup"><span data-stu-id="89526-124">**Automated replication**.</span></span> <span data-ttu-id="89526-125">A Site Recovery automatizált folyamatos replikálásra biztosít.</span><span class="sxs-lookup"><span data-stu-id="89526-125">Site Recovery provides automated continuous replication.</span></span> <span data-ttu-id="89526-126">A feladatátvételi és a feladat-visszavétel egyetlen kattintással is elindítható.</span><span class="sxs-lookup"><span data-stu-id="89526-126">Failover and failback can be triggered with a single click.</span></span>
- <span data-ttu-id="89526-127">**RTO és a helyreállítási Időkorlát**.</span><span class="sxs-lookup"><span data-stu-id="89526-127">**RTO and RPO**.</span></span> <span data-ttu-id="89526-128">A Site Recovery kihasználja hello Azure hálózati infrastruktúra, amely a régiók tookeep RTO és a helyreállítási Időkorlát nagyon alacsony.</span><span class="sxs-lookup"><span data-stu-id="89526-128">Site Recovery takes advantage of hello Azure network infrastructure that connects regions tookeep RTO and RPO very low.</span></span>
- <span data-ttu-id="89526-129">**Tesztelési**.</span><span class="sxs-lookup"><span data-stu-id="89526-129">**Testing**.</span></span> <span data-ttu-id="89526-130">Az igény szerinti feladatátvételi teszteket, szükséges, hogy az nem befolyásolja a termelési számítási feladatokhoz vagy a folyamatban lévő replikáció vész-helyreállítási gyakorlatokat is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="89526-130">You can run disaster-recovery drills with on-demand test failovers, as and when needed, without affecting your production workloads or ongoing replication.</span></span>
- <span data-ttu-id="89526-131">**A helyreállítási terv**.</span><span class="sxs-lookup"><span data-stu-id="89526-131">**Recovery plans**.</span></span> <span data-ttu-id="89526-132">Helyreállítási tervek tooorchestrate feladatátvétel és a feladat-visszavétel hello teljes alkalmazás több virtuális gépeken futó is használhatja.</span><span class="sxs-lookup"><span data-stu-id="89526-132">You can use recovery plans tooorchestrate failover and failback of hello entire application running on multiple VMs.</span></span> <span data-ttu-id="89526-133">hello helyreállítási terv szolgáltatás gazdag első osztályú integrálása az Azure automation-runbook rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="89526-133">hello recovery plan feature has rich first-class integration with Azure automation runbooks.</span></span>


## <a name="deployment-summary"></a><span data-ttu-id="89526-134">Központi telepítési összefoglaló</span><span class="sxs-lookup"><span data-stu-id="89526-134">Deployment summary</span></span>

<span data-ttu-id="89526-135">Mire van szüksége a összegzése toodo tooset virtuális gépek Azure-régiók közötti replikáció:</span><span class="sxs-lookup"><span data-stu-id="89526-135">Here's a summary of what you need toodo tooset up replication of VMs between Azure regions:</span></span>

1. <span data-ttu-id="89526-136">Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89526-136">Create a Recovery Services vault.</span></span> <span data-ttu-id="89526-137">hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.</span><span class="sxs-lookup"><span data-stu-id="89526-137">hello vault contains configuration settings and orchestrates replication.</span></span>
2. <span data-ttu-id="89526-138">Engedélyezze a hello Azure virtuális gépek replikációját.</span><span class="sxs-lookup"><span data-stu-id="89526-138">Enable replication for hello Azure VMs.</span></span>
3. <span data-ttu-id="89526-139">Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="89526-139">Run a test failover toomake sure that everything's working as expected.</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="89526-140">Ellenőrizheti a hello [támogatási mátrixot az Azure virtuális gép replikációs](./site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="89526-140">You can check hello [support matrix for Azure VM replication](./site-recovery-support-matrix-azure-to-azure.md).</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="89526-141">Hogyan tooconfigure hello virtuális gépekhez szükséges hálózati kimenő kapcsolat Azure Site Recovery-replikációhoz információkért lásd: hello [hálózati útmutató](./site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="89526-141">For information on how tooconfigure hello required network outbound connectivity for Azure VMs for Site Recovery replication, see hello [networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md).</span></span>

### <a name="before-you-start"></a><span data-ttu-id="89526-142">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="89526-142">Before you start</span></span>

* <span data-ttu-id="89526-143">Az Azure felhasználói fiókot kell toohave bizonyos [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable Azure virtuális gép replikációját.</span><span class="sxs-lookup"><span data-stu-id="89526-143">Your Azure user account needs toohave certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of an Azure virtual machine.</span></span>
* <span data-ttu-id="89526-144">Az Azure-előfizetéssel kell engedélyezett toocreate virtuális gépe, amelyet az hello vész-helyreállítási régió szerint toouse hello célhelyet.</span><span class="sxs-lookup"><span data-stu-id="89526-144">Your Azure subscription should be enabled toocreate VMs in hello target location that you want toouse as hello disaster recovery region.</span></span> <span data-ttu-id="89526-145">Forduljon a támogatási szolgálathoz tooenable hello szükséges kvótát.</span><span class="sxs-lookup"><span data-stu-id="89526-145">Contact support tooenable hello required quota.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="89526-146">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="89526-146">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="89526-147">Azt javasoljuk, hogy a hello helyre, ahol a virtuális gépek tooreplicate hozzon létre hello Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="89526-147">We recommend that you create hello Recovery Services vault in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="89526-148">Például ha a célhely van hello központi VELÜNK, hozzon létre hello tárolót a **USA középső RÉGIÓJA**.</span><span class="sxs-lookup"><span data-stu-id="89526-148">For example, if your target location is hello central US, create hello vault in **Central US**.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="89526-149">A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="89526-149">Enable replication</span></span>

<span data-ttu-id="89526-150">A **Recovery Services-tárolók**, kattintson a hello tároló nevére.</span><span class="sxs-lookup"><span data-stu-id="89526-150">In **Recovery Services vaults**, click hello vault name.</span></span> <span data-ttu-id="89526-151">Hello tárolóban, kattintson a hello **+ replikálás** gombra.</span><span class="sxs-lookup"><span data-stu-id="89526-151">In hello vault, click hello **+Replicate** button.</span></span>

### <a name="step-1-configure-hello-source"></a><span data-ttu-id="89526-152">1. lépés</span><span class="sxs-lookup"><span data-stu-id="89526-152">Step 1.</span></span> <span data-ttu-id="89526-153">Állítson be hello forrás</span><span class="sxs-lookup"><span data-stu-id="89526-153">Configure hello source</span></span>
1. <span data-ttu-id="89526-154">A **forrás**, jelölje be **Azure - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="89526-154">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="89526-155">A **adatforrásról**, jelölje be hello forrás Azure-régió, ahol a virtuális gépek futnak.</span><span class="sxs-lookup"><span data-stu-id="89526-155">In **Source location**, select hello source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="89526-156">A virtuális gépek válassza hello telepítési modell: **erőforrás-kezelő** vagy **klasszikus**.</span><span class="sxs-lookup"><span data-stu-id="89526-156">Select hello deployment model of your VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="89526-157">Jelölje be hello **forrás-erőforráscsoporton** erőforrás-kezelő virtuális gépek vagy **felhőalapú szolgáltatás** klasszikus virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="89526-157">Select hello **Source resource group** for Resource Manager VMs or **cloud service** for classic VMs.</span></span>

    ![Állítson be hello forrás](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a><span data-ttu-id="89526-159">2. lépés</span><span class="sxs-lookup"><span data-stu-id="89526-159">Step 2.</span></span> <span data-ttu-id="89526-160">Válassza ki a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="89526-160">Select virtual machines</span></span>

* <span data-ttu-id="89526-161">Válassza ki a hello virtuális gépek tooreplicate szeretne, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="89526-161">Select hello VMs you want tooreplicate, and then click **OK**.</span></span>

    ![Válassza ki a virtuális gépek](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a><span data-ttu-id="89526-163">3. lépés</span><span class="sxs-lookup"><span data-stu-id="89526-163">Step 3.</span></span> <span data-ttu-id="89526-164">Beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="89526-164">Configure settings</span></span>

1. <span data-ttu-id="89526-165">toooverride hello alapértelmezett beállítások célozza, és hello beállításainak megadása az Ön által választott, kattintson a **Testreszabás**.</span><span class="sxs-lookup"><span data-stu-id="89526-165">toooverride hello default target settings and specify hello settings of your choice, click **Customize**.</span></span> <span data-ttu-id="89526-166">További információkért lásd: [célerőforrások testreszabása](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span><span class="sxs-lookup"><span data-stu-id="89526-166">For more information, see [Customize target resources](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span></span>

    ![Beállítások konfigurálása](./media/site-recovery-azure-to-azure/settings.png)


2. <span data-ttu-id="89526-168">Alapértelmezés szerint a Site Recovery veszi alkalmazáskonzisztens pillanatképek 4 óránként, és megtartja a helyreállítási pont 24 órán át replikációs házirendet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="89526-168">By default, Site Recovery creates a replication policy that takes app-consistent snapshots every 4 hours and retains recovery points for 24 hours.</span></span> <span data-ttu-id="89526-169">egy házirend különböző beállításokkal toocreate kattintson **Testreszabás** következő túl**replikációs házirend**.</span><span class="sxs-lookup"><span data-stu-id="89526-169">toocreate a policy with different settings, click **Customize** next too**Replication Policy**.</span></span>

    ![Házirend testreszabása](./media/site-recovery-azure-to-azure/customize-policy.png)

3. <span data-ttu-id="89526-171">Kattintson a toostart létesítési hello tároló erőforrásait, **célerőforrások létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="89526-171">toostart provisioning hello target resources, click **Create target resources**.</span></span> <span data-ttu-id="89526-172">Kiépítés egy percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="89526-172">Provisioning takes a minute or so.</span></span> <span data-ttu-id="89526-173">Ne zárja be a panelt hello kiépítése során, vagy toostart keresztül kell.</span><span class="sxs-lookup"><span data-stu-id="89526-173">Don't close hello blade during provisioning, or you need toostart over.</span></span>

4. <span data-ttu-id="89526-174">hello tootrigger replikálása kijelölt virtuális gép, kattintson a **engedélyezze a replikálást**.</span><span class="sxs-lookup"><span data-stu-id="89526-174">tootrigger replication of hello selected VM, click **Enable replication**.</span></span>

5. <span data-ttu-id="89526-175">Hello állapotának nyomon követheti **engedélyezni a védelmet** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="89526-175">You can track progress of hello **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

6. <span data-ttu-id="89526-176">A **beállítások** > **replikált elemek**, virtuális gépek hello állapotát tekintheti meg és hello a kezdeti replikáció folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="89526-176">In **Settings** > **Replicated Items**, you can view hello status of VMs and hello initial replication progress.</span></span> <span data-ttu-id="89526-177">Kattintson a hello VM toodrill le azokat a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="89526-177">Click hello VM toodrill down into its settings.</span></span>

## <a name="run-a-test-failover"></a><span data-ttu-id="89526-178">Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="89526-178">Run a test failover</span></span>

<span data-ttu-id="89526-179">Telepítése után minden, futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik:</span><span class="sxs-lookup"><span data-stu-id="89526-179">After you set up everything, run a test failover toomake sure everything's working as expected:</span></span>

1. <span data-ttu-id="89526-180">toofail keresztül egyetlen számítógépen, a **beállítások** > **replikált elemek**, kattintson a virtuális gép hello **+ feladatátvételi teszt** ikonra.</span><span class="sxs-lookup"><span data-stu-id="89526-180">toofail over a single machine, in **Settings** > **Replicated Items**, click hello VM **+Test Failover** icon.</span></span>

2. <span data-ttu-id="89526-181">a helyreállítás alatt toofail tervez, a **beállítások** > **helyreállítási tervek**, kattintson a jobb gombbal hello terv **feladatátvételi teszt**.</span><span class="sxs-lookup"><span data-stu-id="89526-181">toofail over a recovery plan, in **Settings** > **Recovery Plans**, right-click hello plan **Test Failover**.</span></span> <span data-ttu-id="89526-182">a helyreállítási terv toocreate [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="89526-182">toocreate a recovery plan, [follow these instructions](site-recovery-create-recovery-plans.md).</span></span> 

3. <span data-ttu-id="89526-183">A **feladatátvételi teszt**, jelölje be hello cél Azure-beli virtuális hálózat toowhich Azure virtuális gépek csatlakoznak hello feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="89526-183">In **Test Failover**, select hello target Azure virtual network toowhich Azure VMs are connected after hello failover occurs.</span></span>

4. <span data-ttu-id="89526-184">toostart hello feladatátvételi, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="89526-184">toostart hello failover, click **OK**.</span></span> <span data-ttu-id="89526-185">tootrack előrehaladás, kattintson a hello VM tooopen tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="89526-185">tootrack progress, click hello VM tooopen its properties.</span></span> <span data-ttu-id="89526-186">Hello kattintva **feladatátvételi teszt** feladat hello tároló neve > **beállítások** > **feladatok** > **SiteRecovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="89526-186">Or you can click hello **Test Failover** job in hello vault name > **Settings** > **Jobs** > **Site Recovery jobs**.</span></span>

5. <span data-ttu-id="89526-187">Feladatátvétel befejezését követően hello, hello replika Azure machine hello Azure-portálon jelenik meg > **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="89526-187">After hello failover finishes, hello replica Azure machine appears in hello Azure portal > **Virtual Machines**.</span></span> <span data-ttu-id="89526-188">Győződjön meg arról, hogy hello VM hello megfelelő méretét, csatlakoztatva van a megfelelő hálózati toohello, és fut-e.</span><span class="sxs-lookup"><span data-stu-id="89526-188">Make sure that hello VM is hello appropriate size, that it's connected toohello appropriate network, and that it's running.</span></span>

6. <span data-ttu-id="89526-189">toodelete hello hello feladatátvételi teszt során létrehozott virtuális gépek kattintson **karbantartása a feladatátvételi teszt** hello a replikált elemek vagy hello helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="89526-189">toodelete hello VMs that were created during hello test failover, click **Cleanup test failover** on hello replicated item or hello recovery plan.</span></span> <span data-ttu-id="89526-190">A **megjegyzések**, és a feladatátvételi teszt hello kapcsolatos megfigyelések feljegyzéséhez mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="89526-190">In **Notes**, record and save any observations associated with hello test failover.</span></span> 

<span data-ttu-id="89526-191">[További](site-recovery-test-failover-to-azure.md) vonatkozó feladatátvételi tesztet.</span><span class="sxs-lookup"><span data-stu-id="89526-191">[Learn more](site-recovery-test-failover-to-azure.md) about test failovers.</span></span>


## <a name="next-steps"></a><span data-ttu-id="89526-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89526-192">Next steps</span></span>

<span data-ttu-id="89526-193">Miután hello központi telepítés tesztelése:</span><span class="sxs-lookup"><span data-stu-id="89526-193">After you test hello deployment:</span></span>

- <span data-ttu-id="89526-194">[További](site-recovery-failover.md) feladatátvételek különböző típusú, és hogyan toorun őket.</span><span class="sxs-lookup"><span data-stu-id="89526-194">[Learn more](site-recovery-failover.md) about different types of failovers and how toorun them.</span></span>
- <span data-ttu-id="89526-195">További információ [helyreállítási tervek segítségével](site-recovery-create-recovery-plans.md) tooreduce RTO.</span><span class="sxs-lookup"><span data-stu-id="89526-195">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) tooreduce RTO.</span></span>
