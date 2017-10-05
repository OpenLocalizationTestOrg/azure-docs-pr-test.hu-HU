---
title: "Hogyan működik az Azure virtuális gép replikálása Azure-régiók közötti az Azure Site Recovery?  | Microsoft Docs"
description: "Ez a cikk a összetevők és használható, ha az Azure virtuális gépek replikálása az Azure Site Recovery szolgáltatás használatával Azure-régiók közötti architektúra áttekintése."
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
ms.openlocfilehash: ec397eaeda963f257d1bd996f1f57189bcde17ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a><span data-ttu-id="1e3c4-104">Hogyan működik a Azure virtuális gép replikációs a Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="1e3c4-104">How does Azure VM replication work in Site Recovery?</span></span>


<span data-ttu-id="1e3c4-105">Ez a cikk ismerteti a összetevőiről és folyamataitól részt replikálásához és helyreállítása Azure virtuális gépek (VM) több régióban másik használatával a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-105">This article describes the components and processes involved in replicating and recovering Azure virtual machines (VMs) from one region to another by using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

>[!NOTE]
><span data-ttu-id="1e3c4-106">A Site Recovery szolgáltatásban az Azure virtuális gép replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-106">Azure VM replication with the Site Recovery service is currently in preview.</span></span>

<span data-ttu-id="1e3c4-107">Megjegyzéseket a cikk alján tehet, kérdéseket pedig az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-107">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="architectural-components"></a><span data-ttu-id="1e3c4-108">Az architektúra összetevői</span><span class="sxs-lookup"><span data-stu-id="1e3c4-108">Architectural components</span></span>

<span data-ttu-id="1e3c4-109">Az alábbi ábrán egy Azure virtuális környezetben (a példában az USA keleti régiója helye) egy adott régióban magas szintű áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-109">The following diagram provides a high-level view of an Azure VM environment in a specific region (in this example, the East US location).</span></span> <span data-ttu-id="1e3c4-110">Egy Azure virtuális környezetben:</span><span class="sxs-lookup"><span data-stu-id="1e3c4-110">In an Azure VM environment:</span></span>
- <span data-ttu-id="1e3c4-111">Storage-fiókok elosztva lemezek alkalmazásokat futtathat virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-111">Apps can be running on VMs with disks spread across storage accounts.</span></span>
- <span data-ttu-id="1e3c4-112">A virtuális gépeket tartalmazhat egy vagy több alhálózatot a virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-112">The VMs can be included in one or more subnets within a virtual network.</span></span>

![ügyfél-környezet](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

<span data-ttu-id="1e3c4-114">Tudnivalók a telepítési előfeltételek és a követelmények a [támogatási mátrix](site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="1e3c4-114">Learn about the deployment prerequisites and requirements in the [support matrix](site-recovery-support-matrix-azure-to-azure.md).</span></span>

## <a name="replication-process"></a><span data-ttu-id="1e3c4-115">Replikációs folyamat</span><span class="sxs-lookup"><span data-stu-id="1e3c4-115">Replication process</span></span>

### <a name="step-1"></a><span data-ttu-id="1e3c4-116">1. lépés</span><span class="sxs-lookup"><span data-stu-id="1e3c4-116">Step 1</span></span>

<span data-ttu-id="1e3c4-117">Ha engedélyezi az Azure virtuális gép replikációs az Azure portálon, a következő ábra és táblázat megjelenített erőforrások automatikusan létrejönnek a cél régióban.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-117">When you enable Azure VM replication in the Azure portal, the resources shown in the following diagram and table are automatically created in the target region.</span></span> <span data-ttu-id="1e3c4-118">Alapértelmezés szerint erőforrások forrásbeállítások régió alapján hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-118">By default, resources are created based on source region settings.</span></span> <span data-ttu-id="1e3c4-119">Testre szabhatja a célként megadott beállításokat, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-119">You can customize the target settings as required.</span></span> <span data-ttu-id="1e3c4-120">[További információk](site-recovery-replicate-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="1e3c4-120">[Learn more](site-recovery-replicate-azure-to-azure.md).</span></span>

![Engedélyezze a replikálási folyamat, 1. lépés](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

<span data-ttu-id="1e3c4-122">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-122">**Resource**</span></span> | <span data-ttu-id="1e3c4-123">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-123">**Details**</span></span>
--- | ---
<span data-ttu-id="1e3c4-124">**Cél-erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-124">**Target resource group**</span></span> | <span data-ttu-id="1e3c4-125">Az erőforráscsoport, amelybe a replikált virtuális gépek a feladatátvételt követően tartoznak.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-125">The resource group to which replicated VMs belong after failover.</span></span>
<span data-ttu-id="1e3c4-126">**Virtuális hálózati cél**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-126">**Target virtual network**</span></span> | <span data-ttu-id="1e3c4-127">A virtuális hálózatot, amelyben replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-127">The virtual network in which replicated VMs are located after failover.</span></span> <span data-ttu-id="1e3c4-128">A hálózatleképezés jön létre a forrás és cél virtuális hálózatok között, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-128">A network mapping is created between source and target virtual networks, and vice versa.</span></span>
<span data-ttu-id="1e3c4-129">**Gyorsítótár-storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-129">**Cache storage accounts**</span></span> | <span data-ttu-id="1e3c4-130">A forrás virtuális gépeken változásai replikálódnak a cél tárfiókkal, mielőtt azok nyomon vagy a gyorsítótár storage-fiókot a célhely számára küldött.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-130">Before changes on source VMs are replicated to the target storage account, they are tracked and sent to the cache storage account in the target location.</span></span> <span data-ttu-id="1e3c4-131">Ez biztosítja, hogy a virtuális gépen az üzemi alkalmazások gyakorolt minimális hatás mellett.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-131">This ensures minimal impact on production apps running on the VM.</span></span>
<span data-ttu-id="1e3c4-132">**Cél storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-132">**Target storage accounts**</span></span>  | <span data-ttu-id="1e3c4-133">Storage-fiók, amelyhez a rendszer replikálja az adatokat a célhelyre.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-133">Storage accounts in the target location to which the data is replicated.</span></span>
<span data-ttu-id="1e3c4-134">**Cél rendelkezésre állási csoportok**</span><span class="sxs-lookup"><span data-stu-id="1e3c4-134">**Target availability sets**</span></span>  | <span data-ttu-id="1e3c4-135">Rendelkezésre állási készletek, amelyek a replikált virtuális gépek a feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-135">Availability sets in which the replicated VMs are located after failover.</span></span>

### <a name="step-2"></a><span data-ttu-id="1e3c4-136">2. lépés</span><span class="sxs-lookup"><span data-stu-id="1e3c4-136">Step 2</span></span>

<span data-ttu-id="1e3c4-137">Replikálás engedélyezve van, mint a Site Recovery bővítmény mobilitási szolgáltatás automatikusan települ a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-137">As replication is enabled, the Site Recovery extension Mobility service is automatically installed on the VM.</span></span> <span data-ttu-id="1e3c4-138">A következő történik:</span><span class="sxs-lookup"><span data-stu-id="1e3c4-138">The following occurs:</span></span>

1. <span data-ttu-id="1e3c4-139">A virtuális gép regisztrálva van a Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-139">The VM is registered with Site Recovery.</span></span>

2. <span data-ttu-id="1e3c4-140">A virtuális gép folyamatos replikálásra van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-140">Continuous replication is configured for the VM.</span></span> <span data-ttu-id="1e3c4-141">A virtuális gép lemezeken adatírás folyamatosan átkerülnek a forráshely a gyorsítótár storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-141">Data writes on the VM disks are continuously transferred to the cache storage account in the source location.</span></span>

   ![Engedélyezze a replikálási folyamat, 2. lépés](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > <span data-ttu-id="1e3c4-143">A Site Recovery soha nem kell a VM bejövő kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-143">Site Recovery never needs inbound connectivity to the VM.</span></span> <span data-ttu-id="1e3c4-144">A virtuális gép van szüksége a Site Recovery szolgáltatás URL-címek vagy IP-címek, az Office 365 authentication URL-címek vagy IP-címek és a gyorsítótár tárolási fiók IP-címek csak a kimenő kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-144">The VM needs only outbound connectivity to Site Recovery service URLs/IP addresses, Office 365 authentication URLs/IP addresses, and cache storage account IP addresses.</span></span> <span data-ttu-id="1e3c4-145">További információkért lásd: a [hálózati útmutatót az Azure virtuális gépek replikálásához](site-recovery-azure-to-azure-networking-guidance.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-145">For more information, see the [Networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md) article.</span></span>

## <a name="continuous-replication-process"></a><span data-ttu-id="1e3c4-146">Folyamatos replikálási folyamat</span><span class="sxs-lookup"><span data-stu-id="1e3c4-146">Continuous replication process</span></span>

<span data-ttu-id="1e3c4-147">Miután folyamatos replikálás működik-e, folyamat a lemezírásokat azonnal átkerülnek a gyorsítótár tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-147">After continuous replication is working, disk writes are immediately transferred to the cache storage account.</span></span> <span data-ttu-id="1e3c4-148">A Site Recovery feldolgozza az adatokat, és elküldi a céloldali tárfiók.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-148">Site Recovery processes the data and sends it to the target storage account.</span></span> <span data-ttu-id="1e3c4-149">Az adatok feldolgozása után helyreállítási pontok létrejönnek a céloldali tárfiók néhány percenként.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-149">After the data is processed, recovery points are generated in the target storage account every few minutes.</span></span>

## <a name="failover-process"></a><span data-ttu-id="1e3c4-150">Feladatátvételi folyamat</span><span class="sxs-lookup"><span data-stu-id="1e3c4-150">Failover process</span></span>

<span data-ttu-id="1e3c4-151">Kezdeményezzen feladatátvételt, ha a célerőforrás-csoport virtuális célhálózat, célként megadott alhálózat, a virtuális gépek jönnek létre, és a célként megadott rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-151">When you initiate a failover, the VMs are created in the target resource group, target virtual network, target subnet, and the target availability set.</span></span> <span data-ttu-id="1e3c4-152">A feladatátvétel során bármely helyreállítási pontot is használhat.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-152">During a failover, you can use any recovery point.</span></span>

![Feladatátvételi folyamat](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a><span data-ttu-id="1e3c4-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e3c4-154">Next steps</span></span>

- <span data-ttu-id="1e3c4-155">További tudnivalók [hálózati](site-recovery-azure-to-azure-networking-guidance.md) az Azure Virtuálisgép-replikációt.</span><span class="sxs-lookup"><span data-stu-id="1e3c4-155">Learn about [networking](site-recovery-azure-to-azure-networking-guidance.md) for Azure VM replication.</span></span>
- <span data-ttu-id="1e3c4-156">A bemutató lépéseit követve [Azure virtuális gépek replikálása.](site-recovery-azure-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="1e3c4-156">Follow a walkthrough to [replicate Azure VMs.](site-recovery-azure-to-azure.md)</span></span>
