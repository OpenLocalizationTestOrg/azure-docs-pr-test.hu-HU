---
title: "Az Azure Stream Analytics: A feladat toouse adatfolyam-egységek hatékonyan optimalizálása |} Microsoft Docs"
description: "Gyakorlati tanácsok a méretezés és teljesítmény az Azure Stream Analytics lekérdezési."
keywords: "streamelési egység, teljesítmény-küszöbérték"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 5ad98b34d625190a879260f54c9eff0294e230cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-job-toouse-streaming-units-efficiently"></a><span data-ttu-id="19efd-104">A feladat toouse adatfolyam-egységek hatékonyan optimalizálása</span><span class="sxs-lookup"><span data-stu-id="19efd-104">Optimize your job toouse Streaming Units efficiently</span></span>

<span data-ttu-id="19efd-105">Az Azure Stream Analytics hello teljesítmény "weight" a folyamatos átviteli egységek (SUS-t) egy feladat futó összesíti.</span><span class="sxs-lookup"><span data-stu-id="19efd-105">Azure Stream Analytics aggregates hello performance "weight" of running a job into Streaming Units (SUs).</span></span> <span data-ttu-id="19efd-106">SUs hello számítási erőforrások, amelyek felhasznált tooexecute egy feladat jelölik.</span><span class="sxs-lookup"><span data-stu-id="19efd-106">SUs represent hello computing resources that are consumed tooexecute a job.</span></span> <span data-ttu-id="19efd-107">SUS-t adjon meg egy módon toodescribe hello relatív Eseményfeldolgozási Processzor, memória, kevert mértéke alapján kapacitás és és olvasási sebességet.</span><span class="sxs-lookup"><span data-stu-id="19efd-107">SUs provide a way toodescribe hello relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="19efd-108">Ez a kapacitás lehetővé hello lekérdezés programot, és eltávolítja, nem szükséges tooknow tárolási réteg teljesítménnyel kapcsolatos megfontolások memóriát lefoglalni a feladat manuálisan, illetve a feladat hello CPU szükséges core-száma toorun hozzávetőleges időben koncentrálhat.</span><span class="sxs-lookup"><span data-stu-id="19efd-108">This capacity lets you focus on hello query logic and removes you from needing tooknow storage tier performance considerations, allocate memory for your job manually, and approximate hello CPU core-count needed toorun your job in a timely manner.</span></span>

## <a name="how-many-sus-are-required-for-a-job"></a><span data-ttu-id="19efd-109">Hány SUs szükségesek egy feladatot?</span><span class="sxs-lookup"><span data-stu-id="19efd-109">How many SUs are required for a job?</span></span>

<span data-ttu-id="19efd-110">Kiválasztása szükséges SUS-t egy adott projekt hello száma attól függ, hogy hello partíció konfigurációját hello a be- és hello lekérdezés, amely a hello feladat van definiálva.</span><span class="sxs-lookup"><span data-stu-id="19efd-110">Choosing hello number of required SUs for a particular job depends on hello partition configuration for hello inputs and hello query that's defined within hello job.</span></span> <span data-ttu-id="19efd-111">Hello **méretezési** panel lehetővé teszi a tooset hello SUS-t jobb száma.</span><span class="sxs-lookup"><span data-stu-id="19efd-111">hello **Scale** blade allows you tooset hello right number of SUs.</span></span> <span data-ttu-id="19efd-112">Érdemes a bevált gyakorlat az tooallocate több SUs, mint amennyi szükséges.</span><span class="sxs-lookup"><span data-stu-id="19efd-112">It is a best practice tooallocate more SUs than needed.</span></span> <span data-ttu-id="19efd-113">hello Stream Analytics program optimalizálja a késés és a hello költségekkel további memória lefoglalása közben az átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="19efd-113">hello Stream Analytics processing engine optimizes for latency and throughput at hello cost of allocating additional memory.</span></span>

<span data-ttu-id="19efd-114">Ajánlott eljárás hello általában toostart 6 SUS-t a lekérdezések, amelyek nem használják a *PARTITION BY*.</span><span class="sxs-lookup"><span data-stu-id="19efd-114">In general, hello best practice is toostart with 6 SUs for queries that don't use *PARTITION BY*.</span></span> <span data-ttu-id="19efd-115">Majd megállapítja, hogy hello étkezési hellyel, amelyben módosítása SUs hello száma után fázis reprezentatív adatmennyiséget, és vizsgálja meg a hello SU % kihasználtsági metrika próbálkozást metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="19efd-115">Then determine hello sweet spot by using a trial and error method in which you modify hello number of SUs after you pass representative amounts of data and examine hello SU %Utilization metric.</span></span>

<span data-ttu-id="19efd-116">Az Azure Stream Analytics események tartja hello "újrarendelési puffer" néven, bármely feldolgozásának megkezdése előtt ablakban.</span><span class="sxs-lookup"><span data-stu-id="19efd-116">Azure Stream Analytics keeps events in a window called hello “reorder buffer” before it starts any processing.</span></span> <span data-ttu-id="19efd-117">Események alapján vannak rendezve hello újrarendelési időszakban idő, és a hello ideiglenesen rendezett események utólagos műveleteket.</span><span class="sxs-lookup"><span data-stu-id="19efd-117">Events are sorted within hello reorder window by time, and subsequent operations are performed on hello temporally sorted events.</span></span> <span data-ttu-id="19efd-118">Események idő szerinti újrarendezése biztosítja, hogy hello kezelőnek megjelenítési lehetőségeinek összes hello hello események időkeretre kötni.</span><span class="sxs-lookup"><span data-stu-id="19efd-118">Reordering events by time ensures that hello operator has visibility into all hello events in hello stipulated timeframe.</span></span> <span data-ttu-id="19efd-119">Azt is lehetővé teszi, hogy hajtsa végre a szükséges hello feldolgozási, majd előállít egy kimeneti hello operátor.</span><span class="sxs-lookup"><span data-stu-id="19efd-119">It also lets hello operator perform hello requisite processing and produce an output.</span></span> <span data-ttu-id="19efd-120">A mechanizmus egyik mellékhatása, hogy feldolgozás késleltetett által hello újrarendelési időszak hello időtartama.</span><span class="sxs-lookup"><span data-stu-id="19efd-120">A side effect of this mechanism is that processing is delayed by hello duration of hello reorder window.</span></span> <span data-ttu-id="19efd-121">hello memóriaigény hello feladat (amely hatással van a SU fogyasztás) hello méretét, a újrarendelési ablakot, és hello lévő események feladata.</span><span class="sxs-lookup"><span data-stu-id="19efd-121">hello memory footprint of hello job (which affects SU consumption) is a function of hello size of this reorder window and hello number of events contained within it.</span></span>

> [!NOTE]
> <span data-ttu-id="19efd-122">Hello olvasók számát megváltozásakor feladat frissítéskor átmeneti kapcsolatos figyelmeztetések írása tooaudit naplókat.</span><span class="sxs-lookup"><span data-stu-id="19efd-122">When hello number of readers changes during job upgrades, transient warnings are written tooaudit logs.</span></span> <span data-ttu-id="19efd-123">Stream Analytics-feladatok automatikusan helyreállni ezek átmeneti probléma.</span><span class="sxs-lookup"><span data-stu-id="19efd-123">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a><span data-ttu-id="19efd-124">Magas SU használatát a futó feladatok nagy memória okairól</span><span class="sxs-lookup"><span data-stu-id="19efd-124">Common high-memory causes for high SU usage for running jobs</span></span>

### <a name="high-cardinality-for-group-by"></a><span data-ttu-id="19efd-125">A GROUP BY nagy számosságot</span><span class="sxs-lookup"><span data-stu-id="19efd-125">High cardinality for GROUP BY</span></span>

<span data-ttu-id="19efd-126">bejövő események hello számossága határozzák meg, hogy a memóriahasználat hello feladat.</span><span class="sxs-lookup"><span data-stu-id="19efd-126">hello cardinality of incoming events dictates memory usage for hello job.</span></span>

<span data-ttu-id="19efd-127">Például a `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, társított szám hello **fürtözött** hello lekérdezés hello számossága van.</span><span class="sxs-lookup"><span data-stu-id="19efd-127">For example, in `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, hello number associated with **clustered** is hello cardinality of hello query.</span></span>

<span data-ttu-id="19efd-128">nagy számosságot által okozott problémák toomitigate használó partíciókkal növelésével kiterjesztése hello lekérdezés **PARTITION BY**.</span><span class="sxs-lookup"><span data-stu-id="19efd-128">toomitigate issues that are caused by high cardinality, scale out hello query by increasing partitions using **PARTITION BY**.</span></span>

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

<span data-ttu-id="19efd-129">hello száma *fürtözött* hello számossága alapján a GROUP BY itt van.</span><span class="sxs-lookup"><span data-stu-id="19efd-129">hello number of *clustered* is hello cardinality of GROUP BY here.</span></span>

<span data-ttu-id="19efd-130">Hello lekérdezés particionálása, után azt között oszlik több csomópont.</span><span class="sxs-lookup"><span data-stu-id="19efd-130">After hello query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="19efd-131">Ennek eredményeképpen minden csomópontjába érkező események hello száma csökken, amely hello hello újrarendelési puffer mérete pedig csökkenti.</span><span class="sxs-lookup"><span data-stu-id="19efd-131">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span> <span data-ttu-id="19efd-132">PartitionID is kell particionálni az event hub partíciókat.</span><span class="sxs-lookup"><span data-stu-id="19efd-132">You should also partition event hub partitions by partitionid.</span></span>

### <a name="high-unmatched-event-count-for-join"></a><span data-ttu-id="19efd-133">Az ILLESZTÉSI magas páratlan események száma</span><span class="sxs-lookup"><span data-stu-id="19efd-133">High unmatched event count for JOIN</span></span>

<span data-ttu-id="19efd-134">hello ILLESZTÉS nem egyező események száma hello memóriahasználata hello lekérdezés hatással van.</span><span class="sxs-lookup"><span data-stu-id="19efd-134">hello number of unmatched events in a JOIN affects hello memory utilization of hello query.</span></span> <span data-ttu-id="19efd-135">Vegyük például, egy lekérdezést, amely toofind hello ad megjelenítések száma kattintással generáló keresi:</span><span class="sxs-lookup"><span data-stu-id="19efd-135">For example, take a query that is looking toofind hello number of ad impressions that generates clicks:</span></span>

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

<span data-ttu-id="19efd-136">Az ebben az esetben lehetséges, hogy sok ads is látható, és akkor jönnek létre, néhány kattintással is.</span><span class="sxs-lookup"><span data-stu-id="19efd-136">In this scenario, it is possible that many ads are shown and few clicks are generated.</span></span> <span data-ttu-id="19efd-137">Ilyen eredményt igényelnének hello feladat tookeep hello időszak alatt az összes hello esemény.</span><span class="sxs-lookup"><span data-stu-id="19efd-137">Such a result would require hello job tookeep all hello events within hello time window.</span></span> <span data-ttu-id="19efd-138">hello felhasznált memória mérete arányos toohello ablak méretének és esemény sebessége.</span><span class="sxs-lookup"><span data-stu-id="19efd-138">hello amount of memory consumed is proportional toohello window size and event rate.</span></span> 

<span data-ttu-id="19efd-139">toomitigate ebben az esetben hello lekérdezés növelésével kibővítési particionálja a PARTITION BY használatával.</span><span class="sxs-lookup"><span data-stu-id="19efd-139">toomitigate this situation, scale out hello query by increasing partitions by using PARTITION BY.</span></span> 

<span data-ttu-id="19efd-140">Hello lekérdezés particionálása, után azt között oszlik több feldolgozási csomópont.</span><span class="sxs-lookup"><span data-stu-id="19efd-140">After hello query is partitioned, it is spread out over multiple processing nodes.</span></span> <span data-ttu-id="19efd-141">Ennek eredményeképpen minden csomópontjába érkező események hello száma csökken, amely hello hello újrarendelési puffer mérete pedig csökkenti.</span><span class="sxs-lookup"><span data-stu-id="19efd-141">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span>

### <a name="large-number-of-out-of-order-events"></a><span data-ttu-id="19efd-142">Üzemen kívüli események nagy száma</span><span class="sxs-lookup"><span data-stu-id="19efd-142">Large number of out of order events</span></span> 

<span data-ttu-id="19efd-143">Üzemen kívüli események belül egy olyan nagy időkeretet nagy számú hello "átrendezése puffer" toobe nagyobb hello méretének okoz.</span><span class="sxs-lookup"><span data-stu-id="19efd-143">A large number of out of order events within a large time window causes hello size of hello "reorder buffer" toobe larger.</span></span> <span data-ttu-id="19efd-144">toomitigate ebben az esetben méretezési hello lekérdezés növelésével particionálja a PARTITION BY használatával.</span><span class="sxs-lookup"><span data-stu-id="19efd-144">toomitigate this situation, scale hello query by increasing partitions by using PARTITION BY.</span></span> <span data-ttu-id="19efd-145">Hello lekérdezés particionálása, után azt között oszlik több csomópont.</span><span class="sxs-lookup"><span data-stu-id="19efd-145">After hello query is partitioned, it is spread out over multiple nodes.</span></span> <span data-ttu-id="19efd-146">Ennek eredményeképpen minden csomópontjába érkező események hello száma csökken, amely hello hello újrarendelési puffer mérete pedig csökkenti.</span><span class="sxs-lookup"><span data-stu-id="19efd-146">As a result, hello number of events coming into each node is reduced, which in turn reduces hello size of hello reorder buffer.</span></span> 


## <a name="get-help"></a><span data-ttu-id="19efd-147">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="19efd-147">Get help</span></span>
<span data-ttu-id="19efd-148">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="19efd-148">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="19efd-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19efd-149">Next steps</span></span>
* [<span data-ttu-id="19efd-150">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="19efd-150">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="19efd-151">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="19efd-151">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="19efd-152">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="19efd-152">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* [<span data-ttu-id="19efd-153">Az Azure Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="19efd-153">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="19efd-154">Az Azure Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="19efd-154">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
