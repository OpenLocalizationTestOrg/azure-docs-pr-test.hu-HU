---
title: "Azure virtuális gépek replikálása Azure-régiók közötti, a vész helyreállítási igényeket: Azure az Azure-bA |} Microsoft Docs"
description: "Azure virtuális gépek replikálása Azure-régiók (Azure-Azure) az Azure Site Recovery szolgáltatásban, a vész-helyreállítási igényekre lépéseket foglalja össze."
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
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="4f38b-103">Azure virtuális gépek replikálása az Azure Site Recovery régiók között</span><span class="sxs-lookup"><span data-stu-id="4f38b-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

>[!NOTE]
>
> <span data-ttu-id="4f38b-104">Az Azure virtuális gépek (VM) az Azure Site Recovery replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="4f38b-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

<span data-ttu-id="4f38b-105">Ez a cikk ismerteti, hogyan Azure virtuális gépek replikálása Azure-régiók használatával a [Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4f38b-105">This article describes how to replicate Azure VMs between Azure regions by using the [Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="4f38b-106">Ez a cikk vagy a alsó megjegyzések és kérdések utáni a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4f38b-106">Post comments and questions at the bottom of this article or on the [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="disaster-recovery-in-azure"></a><span data-ttu-id="4f38b-107">Az Azure-ban katasztrófa utáni helyreállítás</span><span class="sxs-lookup"><span data-stu-id="4f38b-107">Disaster recovery in Azure</span></span>

<span data-ttu-id="4f38b-108">Beépített Azure infrastruktúra-szolgáltatásait és funkcióit hozzájárul az Azure virtuális gépeken futó számítási feladatokat egy hatékony és rugalmas rendelkezésre állási stratégiát.</span><span class="sxs-lookup"><span data-stu-id="4f38b-108">Built-in Azure infrastructure capabilities and features contribute to a robust and resilient availability strategy for workloads that run on Azure VMs.</span></span> <span data-ttu-id="4f38b-109">Vannak azonban miért meg kell terveznie az Azure-régiók közötti vész-helyreállítási saját kezűleg számos oka:</span><span class="sxs-lookup"><span data-stu-id="4f38b-109">However, there are many reasons why you need to plan for disaster recovery between Azure regions yourself:</span></span>

- <span data-ttu-id="4f38b-110">Bizonyos alkalmazások és munkafolyamatok, amelyek üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia kérése megfelelőségi irányelveinek teljesítéséhez szüksége.</span><span class="sxs-lookup"><span data-stu-id="4f38b-110">You need to meet compliance guidelines for specific apps and workloads that require a business continuity and disaster recovery (BCDR) strategy.</span></span>
- <span data-ttu-id="4f38b-111">Azt szeretnénk, ha védelme és helyreállítása Azure virtuális gépeken, az üzleti döntések alapján, és nem csak a beépített Azure funkciókon alapulnak.</span><span class="sxs-lookup"><span data-stu-id="4f38b-111">You want the ability to protect and recover Azure VMs based on your business decisions, and not only based on inbuilt Azure functionality.</span></span>
- <span data-ttu-id="4f38b-112">Feladatátvételét és helyreállítását az üzleti és megfelelőségi igényeinek érintő forgalomkiesés nélkül éles megfelelően tesztelni kell.</span><span class="sxs-lookup"><span data-stu-id="4f38b-112">You need to test failover and recovery in accordance with your business and compliance needs, with no impact on production.</span></span>
- <span data-ttu-id="4f38b-113">Feladatok átadása a helyreállítási régió katasztrófa esetén, és zökkenőmentesen visszaadják feladataikat az eredeti forrás régióban kell.</span><span class="sxs-lookup"><span data-stu-id="4f38b-113">You need to fail over to the recovery region in the event of a disaster and fail back to the original source region seamlessly.</span></span>

<span data-ttu-id="4f38b-114">Az Azure-Azure virtuális gép replikációs Site Recovery segítségével ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="4f38b-114">Use Site Recovery for Azure-to-Azure VM replication to help you do all these tasks.</span></span>


## <a name="why-use-site-recovery"></a><span data-ttu-id="4f38b-115">Miért előnyös a Site Recovery használata?</span><span class="sxs-lookup"><span data-stu-id="4f38b-115">Why use Site Recovery?</span></span>      

<span data-ttu-id="4f38b-116">A Site Recovery Azure virtuális gépek replikálása régiók közötti egyszerű módszert kínál:</span><span class="sxs-lookup"><span data-stu-id="4f38b-116">Site Recovery provides a simple way to replicate Azure VMs between regions:</span></span>

- <span data-ttu-id="4f38b-117">**Automatikus központi telepítési**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-117">**Automatic deployment**.</span></span> <span data-ttu-id="4f38b-118">Az aktív-aktív replikációs modell eltérően nincs szükség egy összetett és költséges infrastruktúra másodlagos régióban.</span><span class="sxs-lookup"><span data-stu-id="4f38b-118">Unlike an active-active replication model, there's no need for an expensive and complex infrastructure in the secondary region.</span></span> <span data-ttu-id="4f38b-119">Ha engedélyezi a replikációt, Site Recovery automatikusan létrehozza a szükséges erőforrások a cél régióban forrás régió beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="4f38b-119">When you enable replication, Site Recovery automatically creates the required resources in the target region, based on source region settings.</span></span>
- <span data-ttu-id="4f38b-120">**Szabályozza a régiók**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-120">**Control regions**.</span></span> <span data-ttu-id="4f38b-121">A Site Recovery segítségével replikálhatja bármely régióban a kontinensen belül bármely régióban.</span><span class="sxs-lookup"><span data-stu-id="4f38b-121">With Site Recovery, you can replicate from any region to any region within a continent.</span></span> <span data-ttu-id="4f38b-122">Hasonlítsa ezt össze írásvédett georedundáns tárolás, amelyek szabványos közötti aszinkron módon replikál [régiók párosítva](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) csak.</span><span class="sxs-lookup"><span data-stu-id="4f38b-122">Compare this with read-access geo-redundant storage, which replicates asynchronously between standard [paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) only.</span></span> <span data-ttu-id="4f38b-123">Írásvédett georedundáns tárolás az adatokat a cél régióban olvasási hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="4f38b-123">Read-access geo-redundant storage provides read-only access to the data in the target region.</span></span>
- <span data-ttu-id="4f38b-124">**Replikációs automatikus**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-124">**Automated replication**.</span></span> <span data-ttu-id="4f38b-125">A Site Recovery automatizált folyamatos replikálásra biztosít.</span><span class="sxs-lookup"><span data-stu-id="4f38b-125">Site Recovery provides automated continuous replication.</span></span> <span data-ttu-id="4f38b-126">A feladatátvételi és a feladat-visszavétel egyetlen kattintással is elindítható.</span><span class="sxs-lookup"><span data-stu-id="4f38b-126">Failover and failback can be triggered with a single click.</span></span>
- <span data-ttu-id="4f38b-127">**RTO és a helyreállítási Időkorlát**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-127">**RTO and RPO**.</span></span> <span data-ttu-id="4f38b-128">A Site Recovery kihasználja az Azure hálózati infrastruktúra, amely a régiók RTO és a helyreállítási Időkorlát nagyon alacsony tartani.</span><span class="sxs-lookup"><span data-stu-id="4f38b-128">Site Recovery takes advantage of the Azure network infrastructure that connects regions to keep RTO and RPO very low.</span></span>
- <span data-ttu-id="4f38b-129">**Tesztelési**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-129">**Testing**.</span></span> <span data-ttu-id="4f38b-130">Az igény szerinti feladatátvételi teszteket, szükséges, hogy az nem befolyásolja a termelési számítási feladatokhoz vagy a folyamatban lévő replikáció vész-helyreállítási gyakorlatokat is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="4f38b-130">You can run disaster-recovery drills with on-demand test failovers, as and when needed, without affecting your production workloads or ongoing replication.</span></span>
- <span data-ttu-id="4f38b-131">**A helyreállítási terv**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-131">**Recovery plans**.</span></span> <span data-ttu-id="4f38b-132">A helyreállítási tervek segítségével levezényelni a feladatátvétel és a teljes alkalmazás több virtuális gépeken futó feladat-visszavétel.</span><span class="sxs-lookup"><span data-stu-id="4f38b-132">You can use recovery plans to orchestrate failover and failback of the entire application running on multiple VMs.</span></span> <span data-ttu-id="4f38b-133">A helyreállítási terv funkció Azure automation-forgatókönyveket gazdag első osztályú integrálva van.</span><span class="sxs-lookup"><span data-stu-id="4f38b-133">The recovery plan feature has rich first-class integration with Azure automation runbooks.</span></span>


## <a name="deployment-summary"></a><span data-ttu-id="4f38b-134">Központi telepítési összefoglaló</span><span class="sxs-lookup"><span data-stu-id="4f38b-134">Deployment summary</span></span>

<span data-ttu-id="4f38b-135">Itt található egy Összegzés kell tennie a virtuális gépek Azure-régiók közötti replikáció beállításához:</span><span class="sxs-lookup"><span data-stu-id="4f38b-135">Here's a summary of what you need to do to set up replication of VMs between Azure regions:</span></span>

1. <span data-ttu-id="4f38b-136">Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4f38b-136">Create a Recovery Services vault.</span></span> <span data-ttu-id="4f38b-137">A tároló-konfigurációs beállításait tartalmazza, és koordinálja a replikációt.</span><span class="sxs-lookup"><span data-stu-id="4f38b-137">The vault contains configuration settings and orchestrates replication.</span></span>
2. <span data-ttu-id="4f38b-138">Engedélyezze az Azure virtuális gépek replikálását.</span><span class="sxs-lookup"><span data-stu-id="4f38b-138">Enable replication for the Azure VMs.</span></span>
3. <span data-ttu-id="4f38b-139">Győződjön meg arról, hogy minden a várt módon működik feladatátvételi teszt futtatása.</span><span class="sxs-lookup"><span data-stu-id="4f38b-139">Run a test failover to make sure that everything's working as expected.</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="4f38b-140">Ellenőrizheti a [támogatási mátrixot az Azure virtuális gép replikációs](./site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4f38b-140">You can check the [support matrix for Azure VM replication](./site-recovery-support-matrix-azure-to-azure.md).</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="4f38b-141">A szükséges hálózati kimenő kapcsolat konfigurálása Azure virtuális gépek esetén a Site Recovery replikáció információkért lásd: a [hálózati útmutató](./site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="4f38b-141">For information on how to configure the required network outbound connectivity for Azure VMs for Site Recovery replication, see the [networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md).</span></span>

### <a name="before-you-start"></a><span data-ttu-id="4f38b-142">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4f38b-142">Before you start</span></span>

* <span data-ttu-id="4f38b-143">Egyes rendelkeznie kell Azure felhasználói fiókja [engedélyek](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) egy Azure virtuális gép replikációjának engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f38b-143">Your Azure user account needs to have certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) to enable replication of an Azure virtual machine.</span></span>
* <span data-ttu-id="4f38b-144">Az Azure-előfizetéshez engedélyezni kell a célként megadott helyen, a vész-helyreállítási régió használni kívánt virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4f38b-144">Your Azure subscription should be enabled to create VMs in the target location that you want to use as the disaster recovery region.</span></span> <span data-ttu-id="4f38b-145">A szükséges kvóta engedélyezéséhez forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="4f38b-145">Contact support to enable the required quota.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="4f38b-146">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f38b-146">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="4f38b-147">Azt javasoljuk, hogy a helyen, ahol azt szeretné, hogy a virtuális gépek replikálásához hozzon létre a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="4f38b-147">We recommend that you create the Recovery Services vault in the location where you want your VMs to replicate.</span></span> <span data-ttu-id="4f38b-148">Például, ha a cél elérési útja a központi USA, hozzon létre a tárolót, a **USA középső RÉGIÓJA**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-148">For example, if your target location is the central US, create the vault in **Central US**.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="4f38b-149">A replikáció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4f38b-149">Enable replication</span></span>

<span data-ttu-id="4f38b-150">A **Recovery Services-tárolók**, kattintson a tároló nevére.</span><span class="sxs-lookup"><span data-stu-id="4f38b-150">In **Recovery Services vaults**, click the vault name.</span></span> <span data-ttu-id="4f38b-151">A tárolóban, kattintson a **+ replikálás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4f38b-151">In the vault, click the **+Replicate** button.</span></span>

### <a name="step-1-configure-the-source"></a><span data-ttu-id="4f38b-152">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4f38b-152">Step 1.</span></span> <span data-ttu-id="4f38b-153">Konfigurálja a forrás</span><span class="sxs-lookup"><span data-stu-id="4f38b-153">Configure the source</span></span>
1. <span data-ttu-id="4f38b-154">A **forrás**, jelölje be **Azure - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-154">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="4f38b-155">A **adatforrásról**, válasszon ki forrást az Azure-régió, ahol a virtuális gépek futnak.</span><span class="sxs-lookup"><span data-stu-id="4f38b-155">In **Source location**, select the source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="4f38b-156">A virtuális gépek telepítési modell kiválasztása: **erőforrás-kezelő** vagy **klasszikus**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-156">Select the deployment model of your VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="4f38b-157">Válassza ki a **forrás-erőforráscsoporton** erőforrás-kezelő virtuális gépek vagy **felhőalapú szolgáltatás** klasszikus virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="4f38b-157">Select the **Source resource group** for Resource Manager VMs or **cloud service** for classic VMs.</span></span>

    ![Konfigurálja a forrás](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a><span data-ttu-id="4f38b-159">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4f38b-159">Step 2.</span></span> <span data-ttu-id="4f38b-160">Válassza ki a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="4f38b-160">Select virtual machines</span></span>

* <span data-ttu-id="4f38b-161">Válassza ki a virtuális gépek replikálása, és kattintson a kívánt **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-161">Select the VMs you want to replicate, and then click **OK**.</span></span>

    ![Válassza ki a virtuális gépek](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a><span data-ttu-id="4f38b-163">3. lépés</span><span class="sxs-lookup"><span data-stu-id="4f38b-163">Step 3.</span></span> <span data-ttu-id="4f38b-164">Beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4f38b-164">Configure settings</span></span>

1. <span data-ttu-id="4f38b-165">Felülírják az alapértelmezett cél beállításokat, és adja meg a beállításokat az Ön által választott, kattintson a **Testreszabás**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-165">To override the default target settings and specify the settings of your choice, click **Customize**.</span></span> <span data-ttu-id="4f38b-166">További információkért lásd: [célerőforrások testreszabása](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span><span class="sxs-lookup"><span data-stu-id="4f38b-166">For more information, see [Customize target resources](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span></span>

    ![Beállítások konfigurálása](./media/site-recovery-azure-to-azure/settings.png)


2. <span data-ttu-id="4f38b-168">Alapértelmezés szerint a Site Recovery veszi alkalmazáskonzisztens pillanatképek 4 óránként, és megtartja a helyreállítási pont 24 órán át replikációs házirendet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f38b-168">By default, Site Recovery creates a replication policy that takes app-consistent snapshots every 4 hours and retains recovery points for 24 hours.</span></span> <span data-ttu-id="4f38b-169">Különböző beállításokkal házirend létrehozásához kattintson a **Testreszabás** melletti **replikációs házirend**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-169">To create a policy with different settings, click **Customize** next to **Replication Policy**.</span></span>

    ![Házirend testreszabása](./media/site-recovery-azure-to-azure/customize-policy.png)

3. <span data-ttu-id="4f38b-171">A tároló erőforrásait kiépítés elindításához kattintson **célerőforrások létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-171">To start provisioning the target resources, click **Create target resources**.</span></span> <span data-ttu-id="4f38b-172">Kiépítés egy percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="4f38b-172">Provisioning takes a minute or so.</span></span> <span data-ttu-id="4f38b-173">Ne zárja be a panelt kiépítése során, vagy kezdje újra a folyamatot kell.</span><span class="sxs-lookup"><span data-stu-id="4f38b-173">Don't close the blade during provisioning, or you need to start over.</span></span>

4. <span data-ttu-id="4f38b-174">A kijelölt virtuális gép replikálását indításához kattintson **engedélyezze a replikálást**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-174">To trigger replication of the selected VM, click **Enable replication**.</span></span>

5. <span data-ttu-id="4f38b-175">Előrehaladásának nyomon követheti a **engedélyezni a védelmet** feladat **beállítások** > **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-175">You can track progress of the **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

6. <span data-ttu-id="4f38b-176">A **beállítások** > **replikált elemek**, megtekintheti az állapotát, a virtuális gépek és a kezdeti replikáció folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="4f38b-176">In **Settings** > **Replicated Items**, you can view the status of VMs and the initial replication progress.</span></span> <span data-ttu-id="4f38b-177">Kattintson a virtuális gépek a részletekbe menően tárhatják fel annak beállításait.</span><span class="sxs-lookup"><span data-stu-id="4f38b-177">Click the VM to drill down into its settings.</span></span>

## <a name="run-a-test-failover"></a><span data-ttu-id="4f38b-178">Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="4f38b-178">Run a test failover</span></span>

<span data-ttu-id="4f38b-179">Miután beállította mindent, győződjön meg arról, hogy minden a várt módon működik a feladatátvételi teszt futtatása:</span><span class="sxs-lookup"><span data-stu-id="4f38b-179">After you set up everything, run a test failover to make sure everything's working as expected:</span></span>

1. <span data-ttu-id="4f38b-180">Az egyetlen számítógépen, feladatátvételt **beállítások** > **replikált elemek**, kattintson a virtuális gép **+ feladatátvételi teszt** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4f38b-180">To fail over a single machine, in **Settings** > **Replicated Items**, click the VM **+Test Failover** icon.</span></span>

2. <span data-ttu-id="4f38b-181">A helyreállítási tervben feladatátvételt a **beállítások** > **helyreállítási tervek**, kattintson a jobb gombbal a terv **feladatátvételi teszt**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-181">To fail over a recovery plan, in **Settings** > **Recovery Plans**, right-click the plan **Test Failover**.</span></span> <span data-ttu-id="4f38b-182">Helyreállítási terv létrehozásához [kövesse ezeket az utasításokat](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="4f38b-182">To create a recovery plan, [follow these instructions](site-recovery-create-recovery-plans.md).</span></span> 

3. <span data-ttu-id="4f38b-183">A **feladatátvételi teszt**, jelölje ki a cél Azure-beli virtuális hálózat, amely az Azure virtuális gépek a feladatátvételt követően csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="4f38b-183">In **Test Failover**, select the target Azure virtual network to which Azure VMs are connected after the failover occurs.</span></span>

4. <span data-ttu-id="4f38b-184">A feladatátvételi elindításához kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-184">To start the failover, click **OK**.</span></span> <span data-ttu-id="4f38b-185">Nyomon követni, kattintson a virtuális gép tulajdonságainak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="4f38b-185">To track progress, click the VM to open its properties.</span></span> <span data-ttu-id="4f38b-186">Vagy kattintson a **feladatátvételi teszt** feladat a tároló neve > **beállítások** > **feladatok** > **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-186">Or you can click the **Test Failover** job in the vault name > **Settings** > **Jobs** > **Site Recovery jobs**.</span></span>

5. <span data-ttu-id="4f38b-187">A feladatátvétel befejezése után a replika Azure gép megjelenik-e az Azure portálon > **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="4f38b-187">After the failover finishes, the replica Azure machine appears in the Azure portal > **Virtual Machines**.</span></span> <span data-ttu-id="4f38b-188">Győződjön meg arról, hogy a virtuális gép a megfelelő, csatlakozik-e a megfelelő hálózati méretét, és fut-e.</span><span class="sxs-lookup"><span data-stu-id="4f38b-188">Make sure that the VM is the appropriate size, that it's connected to the appropriate network, and that it's running.</span></span>

6. <span data-ttu-id="4f38b-189">A virtuális gép feladatátvételi tesztje során létrehozott törléséhez kattintson **karbantartása a feladatátvételi teszt** a replikált cikk vagy a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="4f38b-189">To delete the VMs that were created during the test failover, click **Cleanup test failover** on the replicated item or the recovery plan.</span></span> <span data-ttu-id="4f38b-190">A **megjegyzések**, és a feladatátvételi teszttel kapcsolatos megfigyelések feljegyzéséhez mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f38b-190">In **Notes**, record and save any observations associated with the test failover.</span></span> 

<span data-ttu-id="4f38b-191">[További](site-recovery-test-failover-to-azure.md) vonatkozó feladatátvételi tesztet.</span><span class="sxs-lookup"><span data-stu-id="4f38b-191">[Learn more](site-recovery-test-failover-to-azure.md) about test failovers.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4f38b-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f38b-192">Next steps</span></span>

<span data-ttu-id="4f38b-193">Miután a telepítés tesztelésére:</span><span class="sxs-lookup"><span data-stu-id="4f38b-193">After you test the deployment:</span></span>

- <span data-ttu-id="4f38b-194">[További információk](site-recovery-failover.md) a feladatátvételek különféle típusaival és a futtatásukkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4f38b-194">[Learn more](site-recovery-failover.md) about different types of failovers and how to run them.</span></span>
- <span data-ttu-id="4f38b-195">További információ [helyreállítási tervek segítségével](site-recovery-create-recovery-plans.md) RTO csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="4f38b-195">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) to reduce RTO.</span></span>
