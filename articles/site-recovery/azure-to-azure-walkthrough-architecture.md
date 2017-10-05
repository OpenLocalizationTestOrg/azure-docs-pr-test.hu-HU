---
title: "Tekintse át az Azure virtuális gépek Azure-régiók közötti replikáció architektúrája |} Microsoft Docs"
description: "Ez a cikk a összetevők és használható, ha az Azure virtuális gépek replikálása az Azure Site Recovery szolgáltatással Azure-régiók közötti architektúra áttekintése."
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
ms.openlocfilehash: f471add4f4dee26482e2820fb06d010d6c0c0e88
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-1-review-the-architecture-for-azure-vm-replication-between-azure-regions"></a><span data-ttu-id="037ae-103">1. lépés:, Tekintse át az Azure-régiók közötti Azure virtuális gép replikációs architektúrája</span><span class="sxs-lookup"><span data-stu-id="037ae-103">Step 1: Review the architecture for Azure VM replication between Azure regions</span></span>


<span data-ttu-id="037ae-104">Miután megtekintette a [áttekintése lépéseket](azure-to-azure-walkthrough-overview.md) ehhez a központi telepítéshez, a cikkből megtudhatja, hogy az összetevők és használható, ha a replikálása, és az Azure virtuális gépek (VM) helyreállítani egy Azure-régió, egy másik folyamat használatával [ Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="037ae-104">After reviewing the [overview steps](azure-to-azure-walkthrough-overview.md) for this deployment, read this article to understand the components and processes used when replicating and recovering Azure virtual machines (VMs) from one Azure region to another, using [Azure Site Recovery](site-recovery-overview.md).</span></span>

- <span data-ttu-id="037ae-105">A cikk befejezése után kell egy Azure virtuális gép replikációs egy másik régióban működése egyértelműen érthető.</span><span class="sxs-lookup"><span data-stu-id="037ae-105">When you finish the article, you should have a clear understanding of how Azure VM replication to another region works.</span></span>
- <span data-ttu-id="037ae-106">Megjegyzéseket a cikk alján tehet, kérdéseket pedig az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.</span><span class="sxs-lookup"><span data-stu-id="037ae-106">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

>[!NOTE]
><span data-ttu-id="037ae-107">A Site Recovery szolgáltatásban az Azure virtuális gép replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="037ae-107">Azure VM replication with the Site Recovery service is currently in preview.</span></span>



## <a name="architectural-components"></a><span data-ttu-id="037ae-108">Az architektúra összetevői</span><span class="sxs-lookup"><span data-stu-id="037ae-108">Architectural components</span></span>

<span data-ttu-id="037ae-109">Az alábbi ábrán egy Azure virtuális környezetben (a példában az USA keleti régiója helye) egy adott régióban magas szintű áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="037ae-109">The following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, the East US location).</span></span> <span data-ttu-id="037ae-110">Egy Azure virtuális környezetben:</span><span class="sxs-lookup"><span data-stu-id="037ae-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="037ae-111">Storage-fiókok elosztva lemezek alkalmazásokat futtathat virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="037ae-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="037ae-112">A virtuális gépeket tartalmazhat egy vagy több alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="037ae-112">The VMs can be included in one or more subnets within a virtual network.</span></span>

![ügyfél-környezet](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a><span data-ttu-id="037ae-114">Replikációs folyamat</span><span class="sxs-lookup"><span data-stu-id="037ae-114">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="037ae-115">1. lépés</span><span class="sxs-lookup"><span data-stu-id="037ae-115">Step 1</span></span>

<span data-ttu-id="037ae-116">Ha engedélyezi az Azure virtuális gép replikációs az Azure portálon, a következő ábra és táblázat megjelenített erőforrások automatikusan létrejönnek a cél régióban.</span><span class="sxs-lookup"><span data-stu-id="037ae-116">When you enable Azure VM replication in the Azure portal, the resources shown in the following diagram and table are automatically created in the target region.</span></span> <span data-ttu-id="037ae-117">Alapértelmezés szerint erőforrások forrásbeállítások régió alapján hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="037ae-117">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="037ae-118">Testre szabhatja a célként megadott beállításokat, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="037ae-118">You can customize the target settings as required.</span></span> <span data-ttu-id="037ae-119">[További információk](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="037ae-119">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Engedélyezze a replikálási folyamat, 1. lépés](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

<span data-ttu-id="037ae-121">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="037ae-121">**Resource**</span></span> | <span data-ttu-id="037ae-122">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="037ae-122">**Details**</span></span>
--- | ---
<span data-ttu-id="037ae-123">**Cél-erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="037ae-123">**Target resource group**</span></span> | <span data-ttu-id="037ae-124">Az erőforráscsoport, amelybe a replikált virtuális gépek a feladatátvételt követően tartoznak.</span><span class="sxs-lookup"><span data-stu-id="037ae-124">The resource group to which replicated VMs belong after failover.</span></span>
<span data-ttu-id="037ae-125">**Virtuális hálózati cél**</span><span class="sxs-lookup"><span data-stu-id="037ae-125">**Target virtual network**</span></span> | <span data-ttu-id="037ae-126">A virtuális hálózatot, amelyben replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="037ae-126">The virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="037ae-127">A hálózatleképezés jön létre a forrás és cél virtuális hálózatok között, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="037ae-127">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="037ae-128">**Gyorsítótár-storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="037ae-128">**Cache storage accounts**</span></span> | <span data-ttu-id="037ae-129">A forrás virtuális gépeken változásai replikálódnak a cél tárfiókkal, mielőtt azok nyomon vagy a gyorsítótár storage-fiókot a célhely számára küldött.</span><span class="sxs-lookup"><span data-stu-id="037ae-129">Before changes on source VMs are replicated to the target storage account, they are tracked and sent to the cache storage account in the target location.</span></span> <span data-ttu-id="037ae-130">Ez biztosítja, hogy a virtuális gépen az üzemi alkalmazások gyakorolt minimális hatás mellett.</span><span class="sxs-lookup"><span data-stu-id="037ae-130">This ensures minimal impact on production apps running on the VM.</span></span>
<span data-ttu-id="037ae-131">**Cél storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="037ae-131">**Target storage accounts**</span></span>  | <span data-ttu-id="037ae-132">Storage-fiók, amelyhez a rendszer replikálja az adatokat a célhelyre.</span><span class="sxs-lookup"><span data-stu-id="037ae-132">Storage accounts in the target location to which the data is replicated.</span></span>
<span data-ttu-id="037ae-133">**Cél rendelkezésre állási csoportok**</span><span class="sxs-lookup"><span data-stu-id="037ae-133">**Target availability sets**</span></span>  | <span data-ttu-id="037ae-134">Rendelkezésre állási készletek, amelyek a replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="037ae-134">Availability sets in which the replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="037ae-135">2. lépés</span><span class="sxs-lookup"><span data-stu-id="037ae-135">Step 2</span></span>

<span data-ttu-id="037ae-136">Replikálás engedélyezve van, mint a Site Recovery bővítmény mobilitási szolgáltatás automatikusan települ a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="037ae-136">As replication is enabled, the Site Recovery extension Mobility service is automatically installed on the VM.</span></span> <span data-ttu-id="037ae-137">A következő történik:</span><span class="sxs-lookup"><span data-stu-id="037ae-137">The following occurs:</span></span>

1. <span data-ttu-id="037ae-138">A virtuális gép regisztrálva van a Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="037ae-138">The VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="037ae-139">A virtuális gép folyamatos replikálásra van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="037ae-139">Continuous replication is configured for the VM.</span></span> <span data-ttu-id="037ae-140">A virtuális gép lemezeken adatírás folyamatosan átkerülnek a forráshely a gyorsítótár storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="037ae-140">Data writes on the VM disks are continuously transferred to the cache storage account in the source location.</span></span>

   ![Engedélyezze a replikálási folyamat, 2. lépés](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  <span data-ttu-id="037ae-142">Vegye figyelembe, hogy a Site Recovery soha nem kell-e a bejövő kapcsolatot a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="037ae-142">Note that Site Recovery never needs inbound connectivity to the VM.</span></span> <span data-ttu-id="037ae-143">Csak a Site Recovery szolgáltatás URL-címek vagy IP-címek, az Office 365 authentication URL-címek vagy IP-címek és a gyorsítótár tárolási fiók IP-címek kimenő kapcsolatra van szükség.</span><span class="sxs-lookup"><span data-stu-id="037ae-143">Only outbound connectivity to Site Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses is needed.</span></span> 

## <a name="continuous-replication-process"></a><span data-ttu-id="037ae-144">Folyamatos replikálási folyamat</span><span class="sxs-lookup"><span data-stu-id="037ae-144">Continuous replication process</span></span>

<span data-ttu-id="037ae-145">Miután folyamatos replikálás működik-e, folyamat a lemezírásokat azonnal átkerülnek a gyorsítótár tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="037ae-145">After continuous replication is working, disk writes are immediately transferred to the cache storage account.</span></span> <span data-ttu-id="037ae-146">A Site Recovery feldolgozza az adatokat, és elküldi a céloldali tárfiók.</span><span class="sxs-lookup"><span data-stu-id="037ae-146">Site Recovery processes the data and sends it to the target storage account.</span></span> <span data-ttu-id="037ae-147">Az adatok feldolgozása után helyreállítási pontok létrejönnek a céloldali tárfiók néhány percenként.</span><span class="sxs-lookup"><span data-stu-id="037ae-147">After the data is processed, recovery points are generated in the target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="037ae-148">Feladatátvételi folyamat</span><span class="sxs-lookup"><span data-stu-id="037ae-148">Failover process</span></span>

<span data-ttu-id="037ae-149">Kezdeményezzen feladatátvételt, ha a célerőforrás-csoport virtuális célhálózat, célként megadott alhálózat, a virtuális gépek jönnek létre, és a célként megadott rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="037ae-149">When you initiate a failover, the VMs are created in the target resource group, target virtual network, target subnet, and the target availability set.</span></span> <span data-ttu-id="037ae-150">A feladatátvétel során bármely helyreállítási pontot is használhat.</span><span class="sxs-lookup"><span data-stu-id="037ae-150">During a failover, you can use any recovery point.</span></span>

![Feladatátvételi folyamat](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="037ae-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="037ae-152">Next steps</span></span>

<span data-ttu-id="037ae-153">Ugrás a [2. lépés: Ellenőrizze az Előfeltételek és korlátozások](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="037ae-153">Go to [Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>
