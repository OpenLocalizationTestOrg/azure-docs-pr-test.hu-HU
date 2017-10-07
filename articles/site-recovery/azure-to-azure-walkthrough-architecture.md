---
title: "Azure virtuális gépek Azure-régiók közötti replikáció aaaReview hello architektúra |} Microsoft Docs"
description: "Ez a cikk áttekintése összetevők és architektúra használható, ha az Azure virtuális gépek replikálása Azure-régiók hello Azure Site Recovery szolgáltatás között."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/12/2017
ms.author: raynew
ms.openlocfilehash: 4caab4e7a764040f317201d1345c40c73f836d81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-azure-vm-replication-between-azure-regions"></a><span data-ttu-id="061e8-103">1. lépés:, Tekintse át az Azure-régiók közötti Azure virtuális gép replikációs hello architektúrája</span><span class="sxs-lookup"><span data-stu-id="061e8-103">Step 1: Review hello architecture for Azure VM replication between Azure regions</span></span>


<span data-ttu-id="061e8-104">Hello áttekintése után [áttekintése lépéseket](azure-to-azure-walkthrough-overview.md) ehhez a központi telepítéshez, olvassa el a cikk toounderstand hello összetevők és a folyamatok használható, ha a replikálása, és végezze el az Azure-régió tooanother egy, a Azure virtuális gépek (VM) használatával [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="061e8-104">After reviewing hello [overview steps](azure-to-azure-walkthrough-overview.md) for this deployment, read this article toounderstand hello components and processes used when replicating and recovering Azure virtual machines (VMs) from one Azure region tooanother, using [Azure Site Recovery](site-recovery-overview.md).</span></span>

- <span data-ttu-id="061e8-105">Hello cikk befejezése után kell egy Azure virtuális gép replikációs tooanother régió működése egyértelműen érthető.</span><span class="sxs-lookup"><span data-stu-id="061e8-105">When you finish hello article, you should have a clear understanding of how Azure VM replication tooanother region works.</span></span>
- <span data-ttu-id="061e8-106">Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="061e8-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

>[!NOTE]
><span data-ttu-id="061e8-107">A Site Recovery szolgáltatás hello Azure Virtuálisgép-replikációt jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="061e8-107">Azure VM replication with hello Site Recovery service is currently in preview.</span></span>



## <a name="architectural-components"></a><span data-ttu-id="061e8-108">Az architektúra összetevői</span><span class="sxs-lookup"><span data-stu-id="061e8-108">Architectural components</span></span>

<span data-ttu-id="061e8-109">a következő diagram hello Azure virtuális környezetben (a példában hello USA keleti régiója helye) egy adott régióban magas szintű áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="061e8-109">hello following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, hello East US location).</span></span> <span data-ttu-id="061e8-110">Egy Azure virtuális környezetben:</span><span class="sxs-lookup"><span data-stu-id="061e8-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="061e8-111">Storage-fiókok elosztva lemezek alkalmazásokat futtathat virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="061e8-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="061e8-112">virtuális gépek hello tartalmazhat egy vagy több alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="061e8-112">hello VMs can be included in one or more subnets within a virtual network.</span></span>

![ügyfél-környezet](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a><span data-ttu-id="061e8-114">Replikációs folyamat</span><span class="sxs-lookup"><span data-stu-id="061e8-114">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="061e8-115">1. lépés</span><span class="sxs-lookup"><span data-stu-id="061e8-115">Step 1</span></span>

<span data-ttu-id="061e8-116">Ha engedélyezi az Azure virtuális gép replikálása az Azure-portálon hello, hello erőforrások látható hello a következő ábra és táblázat automatikusan létrejönnek hello cél régióban.</span><span class="sxs-lookup"><span data-stu-id="061e8-116">When you enable Azure VM replication in hello Azure portal, hello resources shown in hello following diagram and table are automatically created in hello target region.</span></span> <span data-ttu-id="061e8-117">Alapértelmezés szerint erőforrások forrásbeállítások régió alapján hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="061e8-117">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="061e8-118">Hello tárolóbeállítások szükség szerint testre.</span><span class="sxs-lookup"><span data-stu-id="061e8-118">You can customize hello target settings as required.</span></span> <span data-ttu-id="061e8-119">[További információk](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="061e8-119">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Engedélyezze a replikálási folyamat, 1. lépés](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

<span data-ttu-id="061e8-121">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="061e8-121">**Resource**</span></span> | <span data-ttu-id="061e8-122">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="061e8-122">**Details**</span></span>
--- | ---
<span data-ttu-id="061e8-123">**Cél-erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="061e8-123">**Target resource group**</span></span> | <span data-ttu-id="061e8-124">hello erőforrás csoport toowhich replikált virtuális gépek a feladatátvételt követően tartozik.</span><span class="sxs-lookup"><span data-stu-id="061e8-124">hello resource group toowhich replicated VMs belong after failover.</span></span>
<span data-ttu-id="061e8-125">**Virtuális hálózati cél**</span><span class="sxs-lookup"><span data-stu-id="061e8-125">**Target virtual network**</span></span> | <span data-ttu-id="061e8-126">hello virtuális hálózatot, amelyben a replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="061e8-126">hello virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="061e8-127">A hálózatleképezés jön létre a forrás és cél virtuális hálózatok között, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="061e8-127">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="061e8-128">**Gyorsítótár-storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="061e8-128">**Cache storage accounts**</span></span> | <span data-ttu-id="061e8-129">A forrás virtuális gépeken változásai replikálódnak a céloldali tárfiók toohello, mielőtt ezeket nyomon követheti és toohello gyorsítótár tárfiók küldi hello célhelyet.</span><span class="sxs-lookup"><span data-stu-id="061e8-129">Before changes on source VMs are replicated toohello target storage account, they are tracked and sent toohello cache storage account in hello target location.</span></span> <span data-ttu-id="061e8-130">Ez biztosítja, hogy a futó virtuális gép hello éles alkalmazásokra gyakorolt minimális hatás mellett.</span><span class="sxs-lookup"><span data-stu-id="061e8-130">This ensures minimal impact on production apps running on hello VM.</span></span>
<span data-ttu-id="061e8-131">**Cél storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="061e8-131">**Target storage accounts**</span></span>  | <span data-ttu-id="061e8-132">Storage-fiók hello cél toowhich hello helyadatok replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="061e8-132">Storage accounts in hello target location toowhich hello data is replicated.</span></span>
<span data-ttu-id="061e8-133">**Cél rendelkezésre állási csoportok**</span><span class="sxs-lookup"><span data-stu-id="061e8-133">**Target availability sets**</span></span>  | <span data-ttu-id="061e8-134">Rendelkezésre állási készletek a mely hello replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="061e8-134">Availability sets in which hello replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="061e8-135">2. lépés</span><span class="sxs-lookup"><span data-stu-id="061e8-135">Step 2</span></span>

<span data-ttu-id="061e8-136">Replikálás engedélyezve van, mert hello Site Recovery bővítmény mobilitási szolgáltatás automatikusan települ azon hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="061e8-136">As replication is enabled, hello Site Recovery extension Mobility service is automatically installed on hello VM.</span></span> <span data-ttu-id="061e8-137">hello következő történik:</span><span class="sxs-lookup"><span data-stu-id="061e8-137">hello following occurs:</span></span>

1. <span data-ttu-id="061e8-138">hello VM regisztrálva van a Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="061e8-138">hello VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="061e8-139">Virtuális gép hello folyamatos replikálásra van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="061e8-139">Continuous replication is configured for hello VM.</span></span> <span data-ttu-id="061e8-140">Adatok írása hello méretű lemezek vannak folyamatosan toohello gyorsítótár tárfiók hello forráshely át.</span><span class="sxs-lookup"><span data-stu-id="061e8-140">Data writes on hello VM disks are continuously transferred toohello cache storage account in hello source location.</span></span>

   ![Engedélyezze a replikálási folyamat, 2. lépés](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  <span data-ttu-id="061e8-142">Vegye figyelembe, hogy a Site Recovery soha nem kell kapcsolatot toohello VM bejövő.</span><span class="sxs-lookup"><span data-stu-id="061e8-142">Note that Site Recovery never needs inbound connectivity toohello VM.</span></span> <span data-ttu-id="061e8-143">Csak kimenő kapcsolat tooSite helyreállítási szolgáltatás URL-címek vagy IP-címek, Office 365 portál URL-címek vagy IP-címek és gyorsítótár tárolási fiók IP-címek van szükség.</span><span class="sxs-lookup"><span data-stu-id="061e8-143">Only outbound connectivity tooSite Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses is needed.</span></span> 

## <a name="continuous-replication-process"></a><span data-ttu-id="061e8-144">Folyamatos replikálási folyamat</span><span class="sxs-lookup"><span data-stu-id="061e8-144">Continuous replication process</span></span>

<span data-ttu-id="061e8-145">Után folyamatos replikálás működik-e, a lemez írási műveleteket azonnal lépnek át toohello gyorsítótár tárfiók.</span><span class="sxs-lookup"><span data-stu-id="061e8-145">After continuous replication is working, disk writes are immediately transferred toohello cache storage account.</span></span> <span data-ttu-id="061e8-146">A Site Recovery hello adatokat dolgozza fel, és elküldi azt toohello cél tárfiók.</span><span class="sxs-lookup"><span data-stu-id="061e8-146">Site Recovery processes hello data and sends it toohello target storage account.</span></span> <span data-ttu-id="061e8-147">Hello adatok feldolgozása után helyreállítási pontok jönnek létre a céloldali tárfiók hello néhány percenként.</span><span class="sxs-lookup"><span data-stu-id="061e8-147">After hello data is processed, recovery points are generated in hello target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="061e8-148">Feladatátvételi folyamat</span><span class="sxs-lookup"><span data-stu-id="061e8-148">Failover process</span></span>

<span data-ttu-id="061e8-149">Kezdeményezzen feladatátvételt, virtuális gépek jönnek létre hello célerőforrás-csoport, a cél virtuális hálózat, a cél alhálózathoz hello és hello rendelkezésre állási csoportot célozza.</span><span class="sxs-lookup"><span data-stu-id="061e8-149">When you initiate a failover, hello VMs are created in hello target resource group, target virtual network, target subnet, and hello target availability set.</span></span> <span data-ttu-id="061e8-150">A feladatátvétel során bármely helyreállítási pontot is használhat.</span><span class="sxs-lookup"><span data-stu-id="061e8-150">During a failover, you can use any recovery point.</span></span>

![Feladatátvételi folyamat](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="061e8-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="061e8-152">Next steps</span></span>

<span data-ttu-id="061e8-153">Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="061e8-153">Go too[Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>
