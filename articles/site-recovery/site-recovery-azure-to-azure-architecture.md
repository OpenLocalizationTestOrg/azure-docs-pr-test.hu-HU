---
title: "aaaHow használ az Azure virtuális gép replikálása Azure-régiók munka az Azure Site Recovery közötti?  | Microsoft Docs"
description: "Ez a cikk a összetevők és használható, ha az Azure virtuális gépek replikálása Azure-régiók hello Azure Site Recovery szolgáltatással közötti architektúra áttekintése."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/29/2017
ms.author: sujayt
ms.openlocfilehash: 01eda83e490821f8afc8a2973c18a19453a2e84a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a><span data-ttu-id="4e5ae-104">Hogyan működik a Azure virtuális gép replikációs a Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="4e5ae-104">How does Azure VM replication work in Site Recovery?</span></span>


<span data-ttu-id="4e5ae-105">Ez a cikk ismerteti a hello összetevőiről és folyamataitól részt replikálásához és helyreállítása Azure virtuális gépek (VM) az egy régióban tooanother hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-105">This article describes hello components and processes involved in replicating and recovering Azure virtual machines (VMs) from one region tooanother by using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

>[!NOTE]
><span data-ttu-id="4e5ae-106">A Site Recovery szolgáltatás hello Azure Virtuálisgép-replikációt jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-106">Azure VM replication with hello Site Recovery service is currently in preview.</span></span>

<span data-ttu-id="4e5ae-107">Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4e5ae-107">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="architectural-components"></a><span data-ttu-id="4e5ae-108">Az architektúra összetevői</span><span class="sxs-lookup"><span data-stu-id="4e5ae-108">Architectural components</span></span>

<span data-ttu-id="4e5ae-109">a következő diagram hello Azure virtuális környezetben (a példában hello USA keleti régiója helye) egy adott régióban magas szintű áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-109">hello following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, hello East US location).</span></span> <span data-ttu-id="4e5ae-110">Egy Azure virtuális környezetben:</span><span class="sxs-lookup"><span data-stu-id="4e5ae-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="4e5ae-111">Storage-fiókok elosztva lemezek alkalmazásokat futtathat virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="4e5ae-112">virtuális gépek hello tartalmazhat egy vagy több alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-112">hello VMs can be included in one or more subnets within a virtual network.</span></span>

![ügyfél-környezet](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

<span data-ttu-id="4e5ae-114">Hello üzembehelyezési Előfeltételek és a hello követelményeiről további [támogatási mátrix](site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4e5ae-114">Learn about hello deployment prerequisites and requirements in hello [support matrix](site-recovery-support-matrix-azure-to-azure.md).</span></span>

## <a name="replication-process"></a><span data-ttu-id="4e5ae-115">Replikációs folyamat</span><span class="sxs-lookup"><span data-stu-id="4e5ae-115">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="4e5ae-116">1. lépés</span><span class="sxs-lookup"><span data-stu-id="4e5ae-116">Step 1</span></span>

<span data-ttu-id="4e5ae-117">Ha engedélyezi az Azure virtuális gép replikálása az Azure-portálon hello, hello erőforrások látható hello a következő ábra és táblázat automatikusan létrejönnek hello cél régióban.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-117">When you enable Azure VM replication in hello Azure portal, hello resources shown in hello following diagram and table are automatically created in hello target region.</span></span> <span data-ttu-id="4e5ae-118">Alapértelmezés szerint erőforrások forrásbeállítások régió alapján hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-118">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="4e5ae-119">Hello tárolóbeállítások szükség szerint testre.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-119">You can customize hello target settings as required.</span></span> <span data-ttu-id="4e5ae-120">[További információk](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4e5ae-120">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Engedélyezze a replikálási folyamat, 1. lépés](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

<span data-ttu-id="4e5ae-122">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-122">**Resource**</span></span> | <span data-ttu-id="4e5ae-123">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-123">**Details**</span></span>
--- | ---
<span data-ttu-id="4e5ae-124">**Cél-erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-124">**Target resource group**</span></span> | <span data-ttu-id="4e5ae-125">hello erőforrás csoport toowhich replikált virtuális gépek a feladatátvételt követően tartozik.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-125">hello resource group toowhich replicated VMs belong after failover.</span></span>
<span data-ttu-id="4e5ae-126">**Virtuális hálózati cél**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-126">**Target virtual network**</span></span> | <span data-ttu-id="4e5ae-127">hello virtuális hálózatot, amelyben a replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-127">hello virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="4e5ae-128">A hálózatleképezés jön létre a forrás és cél virtuális hálózatok között, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-128">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="4e5ae-129">**Gyorsítótár-storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-129">**Cache storage accounts**</span></span> | <span data-ttu-id="4e5ae-130">A forrás virtuális gépeken változásai replikálódnak a céloldali tárfiók toohello, mielőtt ezeket nyomon követheti és toohello gyorsítótár tárfiók küldi hello célhelyet.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-130">Before changes on source VMs are replicated toohello target storage account, they are tracked and sent toohello cache storage account in hello target location.</span></span> <span data-ttu-id="4e5ae-131">Ez biztosítja, hogy a futó virtuális gép hello éles alkalmazásokra gyakorolt minimális hatás mellett.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-131">This ensures minimal impact on production apps running on hello VM.</span></span>
<span data-ttu-id="4e5ae-132">**Cél storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-132">**Target storage accounts**</span></span>  | <span data-ttu-id="4e5ae-133">Storage-fiók hello cél toowhich hello helyadatok replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-133">Storage accounts in hello target location toowhich hello data is replicated.</span></span>
<span data-ttu-id="4e5ae-134">**Cél rendelkezésre állási csoportok**</span><span class="sxs-lookup"><span data-stu-id="4e5ae-134">**Target availability sets**</span></span>  | <span data-ttu-id="4e5ae-135">Rendelkezésre állási készletek a mely hello replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-135">Availability sets in which hello replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="4e5ae-136">2. lépés</span><span class="sxs-lookup"><span data-stu-id="4e5ae-136">Step 2</span></span>

<span data-ttu-id="4e5ae-137">Replikálás engedélyezve van, mert hello Site Recovery bővítmény mobilitási szolgáltatás automatikusan települ azon hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-137">As replication is enabled, hello Site Recovery extension Mobility service is automatically installed on hello VM.</span></span> <span data-ttu-id="4e5ae-138">hello következő történik:</span><span class="sxs-lookup"><span data-stu-id="4e5ae-138">hello following occurs:</span></span>

1. <span data-ttu-id="4e5ae-139">hello VM regisztrálva van a Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-139">hello VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="4e5ae-140">Virtuális gép hello folyamatos replikálásra van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-140">Continuous replication is configured for hello VM.</span></span> <span data-ttu-id="4e5ae-141">Adatok írása hello méretű lemezek vannak folyamatosan toohello gyorsítótár tárfiók hello forráshely át.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-141">Data writes on hello VM disks are continuously transferred toohello cache storage account in hello source location.</span></span>

   ![Engedélyezze a replikálási folyamat, 2. lépés](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > <span data-ttu-id="4e5ae-143">A Site Recovery soha nem kell a VM bejövő kapcsolatot toohello.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-143">Site Recovery never needs inbound connectivity toohello VM.</span></span> <span data-ttu-id="4e5ae-144">virtuális gép hello van szüksége, csak a kimenő kapcsolat tooSite helyreállítási szolgáltatás URL-címek vagy IP-címek, a Office 365 authentication URL-címek vagy IP-címek és a gyorsítótár tárolási fiók IP-címek.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-144">hello VM needs only outbound connectivity tooSite Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses.</span></span> <span data-ttu-id="4e5ae-145">További információkért lásd: hello [hálózati útmutatót az Azure virtuális gépek replikálásához](site-recovery-azure-to-azure-networking-guidance.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-145">For more information, see hello [Networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md) article.</span></span>

## <a name="continuous-replication-process"></a><span data-ttu-id="4e5ae-146">Folyamatos replikálási folyamat</span><span class="sxs-lookup"><span data-stu-id="4e5ae-146">Continuous replication process</span></span>

<span data-ttu-id="4e5ae-147">Után folyamatos replikálás működik-e, a lemez írási műveleteket azonnal lépnek át toohello gyorsítótár tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-147">After continuous replication is working, disk writes are immediately transferred toohello cache storage account.</span></span> <span data-ttu-id="4e5ae-148">A Site Recovery hello adatokat dolgozza fel, és elküldi azt toohello cél tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-148">Site Recovery processes hello data and sends it toohello target storage account.</span></span> <span data-ttu-id="4e5ae-149">Hello adatok feldolgozása után helyreállítási pontok jönnek létre a céloldali tárfiók hello néhány percenként.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-149">After hello data is processed, recovery points are generated in hello target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="4e5ae-150">Feladatátvételi folyamat</span><span class="sxs-lookup"><span data-stu-id="4e5ae-150">Failover process</span></span>

<span data-ttu-id="4e5ae-151">Kezdeményezzen feladatátvételt, virtuális gépek jönnek létre hello célerőforrás-csoport, a cél virtuális hálózat, a cél alhálózathoz hello és hello rendelkezésre állási csoportot célozza.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-151">When you initiate a failover, hello VMs are created in hello target resource group, target virtual network, target subnet, and hello target availability set.</span></span> <span data-ttu-id="4e5ae-152">A feladatátvétel során bármely helyreállítási pontot is használhat.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-152">During a failover, you can use any recovery point.</span></span>

![Feladatátvételi folyamat](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="4e5ae-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e5ae-154">Next steps</span></span>

- <span data-ttu-id="4e5ae-155">További tudnivalók [hálózati](site-recovery-azure-to-azure-networking-guidance.md) az Azure Virtuálisgép-replikációt.</span><span class="sxs-lookup"><span data-stu-id="4e5ae-155">Learn about [networking](site-recovery-azure-to-azure-networking-guidance.md) for Azure VM replication.</span></span>
- <span data-ttu-id="4e5ae-156">Hajtsa végre a forgatókönyv túl[Azure virtuális gépek replikálása.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="4e5ae-156">Follow a walkthrough too[replicate Azure VMs.](site-recovery-azure-to-azure.md)</span></span>
