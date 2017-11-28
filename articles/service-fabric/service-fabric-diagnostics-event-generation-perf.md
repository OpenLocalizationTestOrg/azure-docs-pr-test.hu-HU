---
title: "Service Fabric-figyelő aaaAzure |} Microsoft Docs"
description: "További információk a figyelési és az Azure Service Fabric-fürtök diagnostics teljesítményszámlálók."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 54d4c62b7250a1f70b0898ba07ae5a37716f4cf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-metrics"></a><span data-ttu-id="54e50-103">Teljesítménymértékeket</span><span class="sxs-lookup"><span data-stu-id="54e50-103">Performance metrics</span></span>

<span data-ttu-id="54e50-104">Metrikák kell kell a fürt összegyűjtött toounderstand hello teljesítmény, valamint hello az alkalmazások azt.</span><span class="sxs-lookup"><span data-stu-id="54e50-104">Metrics should be collected toounderstand hello performance of your cluster as well as hello applications running in it.</span></span> <span data-ttu-id="54e50-105">A Service Fabric-fürtök esetén ajánlott hello alábbi teljesítményszámlálók gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="54e50-105">For Service Fabric clusters, we recommend collecting hello following performance counters.</span></span>

## <a name="nodes"></a><span data-ttu-id="54e50-106">Csomópontok</span><span class="sxs-lookup"><span data-stu-id="54e50-106">Nodes</span></span>

<span data-ttu-id="54e50-107">Hello gépek a fürt fontolja meg a következő toobetter megérteni hello minden gép terhelését, és ellenőrizze a megfelelő döntések skálázás fürt teljesítményszámlálók hello gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="54e50-107">For hello machines in your cluster, consider collecting hello following performance counters toobetter understand hello load on each machine and make appropriate cluster scaling decisions.</span></span>

| <span data-ttu-id="54e50-108">Teljesítményszámláló-kategóriája</span><span class="sxs-lookup"><span data-stu-id="54e50-108">Counter Category</span></span> | <span data-ttu-id="54e50-109">Számláló neve</span><span class="sxs-lookup"><span data-stu-id="54e50-109">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="54e50-110">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-110">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-111">Átlagos Olvasási Lemezvárólista hossza</span><span class="sxs-lookup"><span data-stu-id="54e50-111">Avg. Disk Read Queue Length</span></span> |
| <span data-ttu-id="54e50-112">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-112">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-113">Átlagos Írási Lemezvárólista hossza</span><span class="sxs-lookup"><span data-stu-id="54e50-113">Avg. Disk Write Queue Length</span></span> |
| <span data-ttu-id="54e50-114">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-114">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-115">Átlagos Lemez mp/Olvasás</span><span class="sxs-lookup"><span data-stu-id="54e50-115">Avg. Disk sec/Read</span></span> |
| <span data-ttu-id="54e50-116">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-116">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-117">Átlagos Lemez mp/írás</span><span class="sxs-lookup"><span data-stu-id="54e50-117">Avg. Disk sec/Write</span></span> |
| <span data-ttu-id="54e50-118">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-118">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-119">Lemezolvasások/mp</span><span class="sxs-lookup"><span data-stu-id="54e50-119">Disk Reads/sec</span></span> |
| <span data-ttu-id="54e50-120">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-120">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-121">Lemez sebessége olvasott bájt/mp</span><span class="sxs-lookup"><span data-stu-id="54e50-121">Disk Read Bytes/sec</span></span> |
| <span data-ttu-id="54e50-122">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-122">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-123">Lemezírás/mp</span><span class="sxs-lookup"><span data-stu-id="54e50-123">Disk Writes/sec</span></span> |
| <span data-ttu-id="54e50-124">Fizikai lemez (lemezenként)</span><span class="sxs-lookup"><span data-stu-id="54e50-124">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="54e50-125">Lemez írási bájtok/s</span><span class="sxs-lookup"><span data-stu-id="54e50-125">Disk Write Bytes/sec</span></span> |
| <span data-ttu-id="54e50-126">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="54e50-126">Memory</span></span> | <span data-ttu-id="54e50-127">Rendelkezésre álló memória (MB)</span><span class="sxs-lookup"><span data-stu-id="54e50-127">Available MBytes</span></span> |
| <span data-ttu-id="54e50-128">PagingFile</span><span class="sxs-lookup"><span data-stu-id="54e50-128">PagingFile</span></span> | <span data-ttu-id="54e50-129">Kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="54e50-129">% Usage</span></span> |
| <span data-ttu-id="54e50-130">Process(total)</span><span class="sxs-lookup"><span data-stu-id="54e50-130">Process(Total)</span></span> | <span data-ttu-id="54e50-131">Processzor kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="54e50-131">% Processor Time</span></span> |
| <span data-ttu-id="54e50-132">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-132">Process (per service)</span></span> | <span data-ttu-id="54e50-133">Processzor kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="54e50-133">% Processor Time</span></span> |
| <span data-ttu-id="54e50-134">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-134">Process (per service)</span></span> | <span data-ttu-id="54e50-135">A folyamatot</span><span class="sxs-lookup"><span data-stu-id="54e50-135">ID Process</span></span> |
| <span data-ttu-id="54e50-136">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-136">Process (per service)</span></span> | <span data-ttu-id="54e50-137">A saját memória</span><span class="sxs-lookup"><span data-stu-id="54e50-137">Private Bytes</span></span> |
| <span data-ttu-id="54e50-138">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-138">Process (per service)</span></span> | <span data-ttu-id="54e50-139">Szálak száma</span><span class="sxs-lookup"><span data-stu-id="54e50-139">Thread Count</span></span> |
| <span data-ttu-id="54e50-140">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-140">Process (per service)</span></span> | <span data-ttu-id="54e50-141">Virtuális memória</span><span class="sxs-lookup"><span data-stu-id="54e50-141">Virtual Bytes</span></span> |
| <span data-ttu-id="54e50-142">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-142">Process (per service)</span></span> | <span data-ttu-id="54e50-143">Munkakészlet</span><span class="sxs-lookup"><span data-stu-id="54e50-143">Working Set</span></span> |
| <span data-ttu-id="54e50-144">Folyamat száma (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-144">Process (per service)</span></span> | <span data-ttu-id="54e50-145">Munkakészlet - saját</span><span class="sxs-lookup"><span data-stu-id="54e50-145">Working Set - Private</span></span> |

## <a name="net-applications-and-services"></a><span data-ttu-id="54e50-146">.NET-alkalmazások és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="54e50-146">.NET applications and services</span></span>

<span data-ttu-id="54e50-147">.NET szolgáltatások tooyour fürt központi telepítése a következő számlálók hello gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="54e50-147">Collect hello following counters if you are deploying .NET services tooyour cluster.</span></span> 

| <span data-ttu-id="54e50-148">Teljesítményszámláló-kategóriája</span><span class="sxs-lookup"><span data-stu-id="54e50-148">Counter Category</span></span> | <span data-ttu-id="54e50-149">Számláló neve</span><span class="sxs-lookup"><span data-stu-id="54e50-149">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="54e50-150">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-150">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-151">Folyamatazonosító</span><span class="sxs-lookup"><span data-stu-id="54e50-151">Process ID</span></span> |
| <span data-ttu-id="54e50-152">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-152">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-153"># Összesen előjegyzett memória kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="54e50-153"># Total committed Bytes</span></span> |
| <span data-ttu-id="54e50-154">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-154">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-155"># Összesen fenntartott memória</span><span class="sxs-lookup"><span data-stu-id="54e50-155"># Total reserved Bytes</span></span> |
| <span data-ttu-id="54e50-156">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-156">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-157">(Bájt) összes halommemória</span><span class="sxs-lookup"><span data-stu-id="54e50-157"># Bytes in all Heaps</span></span> |
| <span data-ttu-id="54e50-158">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-158">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-159"># 0. generációs gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="54e50-159"># Gen 0 Collections</span></span> |
| <span data-ttu-id="54e50-160">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-160">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-161"># 1. generációból gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="54e50-161"># Gen 1 Collections</span></span> |
| <span data-ttu-id="54e50-162">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-162">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-163"># Generációból 2 gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="54e50-163"># Gen 2 Collections</span></span> |
| <span data-ttu-id="54e50-164">.NET CLR memória (szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="54e50-164">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="54e50-165">% Töltött idő</span><span class="sxs-lookup"><span data-stu-id="54e50-165">% Time in GC</span></span> |

### <a name="service-fabrics-custom-performance-counters"></a><span data-ttu-id="54e50-166">A Service Fabric egyéni teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="54e50-166">Service Fabric's custom performance counters</span></span>

<span data-ttu-id="54e50-167">A Service Fabric egyéni teljesítményszámlálóit jelentős mennyiségű állít elő.</span><span class="sxs-lookup"><span data-stu-id="54e50-167">Service Fabric generates a substantial amount of custom performance counters.</span></span> <span data-ttu-id="54e50-168">Ha hello SDK telepítve van, látható hello átfogó listáját a Windows-számítógépre az alkalmazás teljesítményének figyelése (Start > Teljesítményfigyelő).</span><span class="sxs-lookup"><span data-stu-id="54e50-168">If you have hello SDK installed, you can see hello comprehensive list on your Windows machine in your Performance Monitor application (Start > Performance Monitor).</span></span> 

<span data-ttu-id="54e50-169">Hello alkalmazások elérhetővé tett tooyour fürt, ha Reliable Actors használ, adja hozzá a countes `Service Fabric Actor` és `Service Fabric Actor Method` kategóriák (lásd: [Service Fabric megbízható szereplője diagnosztika](service-fabric-reliable-actors-diagnostics.md)).</span><span class="sxs-lookup"><span data-stu-id="54e50-169">In hello applications you are deploying tooyour cluster, if you are using Reliable Actors, add countes from `Service Fabric Actor` and `Service Fabric Actor Method` categories (see [Service Fabric Reliable Actors Diagnostics](service-fabric-reliable-actors-diagnostics.md)).</span></span>

<span data-ttu-id="54e50-170">Ha Reliable Services használatához hasonló módon kell `Service Fabric Service` és `Service Fabric Service Method` kell gyűjtése teljesítményszámláló-kategóriák a teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="54e50-170">If you use Reliable Services, we similarly have `Service Fabric Service` and `Service Fabric Service Method` counter categories that you should collect counters from.</span></span> 

<span data-ttu-id="54e50-171">Ha megbízható gyűjteményeket használ, azt javasoljuk, hogy hello `Avg. Transaction ms/Commit` a hello `Service Fabric Transactional Replicator` toocollect hello átlagos véglegesítési késés / tranzakció metrikát.</span><span class="sxs-lookup"><span data-stu-id="54e50-171">If you use Reliable Collections, we recommend adding hello `Avg. Transaction ms/Commit` from hello `Service Fabric Transactional Replicator` toocollect hello average commit latency per transaction metric.</span></span>


## <a name="next-steps"></a><span data-ttu-id="54e50-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54e50-172">Next steps</span></span>

* <span data-ttu-id="54e50-173">További információ [hello platform szintjén az eseménygenerálás](service-fabric-diagnostics-event-generation-infra.md) a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="54e50-173">Learn more about [event generation at hello platform level](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric</span></span>
* <span data-ttu-id="54e50-174">Teljesítményadatok gyűjtéséhez keresztül [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span><span class="sxs-lookup"><span data-stu-id="54e50-174">Collect performance metrics through [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span></span>
