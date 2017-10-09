---
title: "aaaScale Stream Analytics-feladatok tooincrease átviteli |} Microsoft Docs"
description: "Ismerje meg, hogyan tooscale Stream Analytics-feladatok bemeneti partíciók, hangolása hello lekérdezés definícióját, és a beállítás által végrehajtott feladat folyamatos átviteli egységeket."
keywords: "adatfolyam, az adatfeldolgozás streaming hangolására analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeffstok
ms.openlocfilehash: 4ba8f6b2f8bfebd52cfa07696b501b42cda21f75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-stream-analytics-jobs-tooincrease-stream-data-processing-throughput"></a><span data-ttu-id="7ba20-104">Scale Azure Stream Analytics feladatok tooincrease adatfolyam adatfeldolgozási átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="7ba20-104">Scale Azure Stream Analytics jobs tooincrease stream data processing throughput</span></span>
<span data-ttu-id="7ba20-105">Ez a cikk bemutatja, hogyan tootune a Stream Analytics lekérdezési tooincrease átviteli Streaming Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="7ba20-105">This article shows you how tootune a Stream Analytics query tooincrease throughput for Streaming Analytics jobs.</span></span> <span data-ttu-id="7ba20-106">Megtudhatja, hogyan tooscale Stream Analytics feladatok bemeneti partíciók beállításával, hangolási hello analytics lekérdezésdefiníció és kiszámítása és beállítás feladat *adatfolyam-egységek* (SUS-t).</span><span class="sxs-lookup"><span data-stu-id="7ba20-106">You learn how tooscale Stream Analytics jobs by configuring input partitions, tuning hello analytics query definition, and calculating and setting job *streaming units* (SUs).</span></span> 

## <a name="what-are-hello-parts-of-a-stream-analytics-job"></a><span data-ttu-id="7ba20-107">Mik azok a hello részei a Stream Analytics-feladatot?</span><span class="sxs-lookup"><span data-stu-id="7ba20-107">What are hello parts of a Stream Analytics job?</span></span>
<span data-ttu-id="7ba20-108">A Stream Analytics-feladat definíciójának bemenet, a lekérdezés és a kimeneti tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ba20-108">A Stream Analytics job definition includes inputs, a query, and output.</span></span> <span data-ttu-id="7ba20-109">Bemenetek, ahol hello feladat beolvassa a hello adatfolyamban.</span><span class="sxs-lookup"><span data-stu-id="7ba20-109">Inputs are where hello job reads hello data stream from.</span></span> <span data-ttu-id="7ba20-110">hello lekérdezés használt tootransform hello bemeneti adatfolyamban, és hello kimeneti ahol hello feladat küldi hello feladat eredményét.</span><span class="sxs-lookup"><span data-stu-id="7ba20-110">hello query is used tootransform hello data input stream, and hello output is where hello job sends hello job results to.</span></span>  

<span data-ttu-id="7ba20-111">Egy feladat az adatok adatfolyamként történő legalább egy bemeneti forrást igényel.</span><span class="sxs-lookup"><span data-stu-id="7ba20-111">A job requires at least one input source for data streaming.</span></span> <span data-ttu-id="7ba20-112">hello adatforrás adatfolyam bemeneti tárolhatók az Azure event hubs vagy az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7ba20-112">hello data stream input source can be stored in an Azure event hub or in Azure blob storage.</span></span> <span data-ttu-id="7ba20-113">További információkért lásd: [Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md) és [Azure Stream Analytics használatának első](stream-analytics-real-time-fraud-detection.md).</span><span class="sxs-lookup"><span data-stu-id="7ba20-113">For more information, see [Introduction tooAzure Stream Analytics](stream-analytics-introduction.md) and [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).</span></span>

## <a name="partitions-in-event-hubs-and-azure-storage"></a><span data-ttu-id="7ba20-114">Az event hubs és az Azure storage a partíciók</span><span class="sxs-lookup"><span data-stu-id="7ba20-114">Partitions in event hubs and Azure storage</span></span>
<span data-ttu-id="7ba20-115">A Stream Analytics-feladat skálázás kihasználja a partíciók a bemeneti vagy kimeneti hello.</span><span class="sxs-lookup"><span data-stu-id="7ba20-115">Scaling a Stream Analytics job takes advantage of partitions in hello input or output.</span></span> <span data-ttu-id="7ba20-116">A particionáló lehetővé teszi, hogy részekre adatok partíciós kulcs alapján.</span><span class="sxs-lookup"><span data-stu-id="7ba20-116">Partitioning lets you divide data into subsets based on a partition key.</span></span> <span data-ttu-id="7ba20-117">A folyamat, amely akkor hello adatokat (például egy Streaming Analytics-feladat) is használnak, és különböző partíciók írási párhuzamosan, ami növeli az adatátviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="7ba20-117">A process that consumes hello data (such as a Streaming Analytics job) can consume and write different partitions in parallel, which increases throughput.</span></span> <span data-ttu-id="7ba20-118">Streaming Analytics használata, kihasználhatja a particionálás az event hubs és a Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7ba20-118">When you work with Streaming Analytics, you can take advantage of partitioning in event hubs and in Blob storage.</span></span> 

<span data-ttu-id="7ba20-119">Partíciók kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="7ba20-119">For more information about partitions, see hello following articles:</span></span>

* [<span data-ttu-id="7ba20-120">Event Hubs szolgáltatások – áttekintés</span><span class="sxs-lookup"><span data-stu-id="7ba20-120">Event Hubs features overview</span></span>](../event-hubs/event-hubs-features.md#partitions)
* [<span data-ttu-id="7ba20-121">Az adatok particionálása</span><span class="sxs-lookup"><span data-stu-id="7ba20-121">Data partitioning</span></span>](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a><span data-ttu-id="7ba20-122">Adatfolyam-egységek (SUS-t)</span><span class="sxs-lookup"><span data-stu-id="7ba20-122">Streaming units (SUs)</span></span>
<span data-ttu-id="7ba20-123">Adatfolyam-egységek (SUs) jelentik hello erőforrások power, amelyek szükségesek a számítási rendelés és tooexecute egy Azure Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="7ba20-123">Streaming units (SUs) represent hello resources and computing power that are required in order tooexecute an Azure Stream Analytics job.</span></span> <span data-ttu-id="7ba20-124">SUS-t adjon meg egy módon toodescribe hello relatív Eseményfeldolgozási Processzor, memória, kevert mértéke alapján kapacitás és és olvasási sebességet.</span><span class="sxs-lookup"><span data-stu-id="7ba20-124">SUs provide a way toodescribe hello relative event processing capacity based on a blended measure of CPU, memory, and read and write rates.</span></span> <span data-ttu-id="7ba20-125">Minden egyes SU megfelel tooroughly 1 MB/s átviteli.</span><span class="sxs-lookup"><span data-stu-id="7ba20-125">Each SU corresponds tooroughly 1 MB/second of throughput.</span></span> 

<span data-ttu-id="7ba20-126">Hány SUs kiválasztása egy adott feladat függ hello partíció konfigurációját hello bemenetek hello feladat definiált hello lekérdezés szükség.</span><span class="sxs-lookup"><span data-stu-id="7ba20-126">Choosing how many SUs are required for a particular job depends on hello partition configuration for hello inputs and on hello query defined for hello job.</span></span> <span data-ttu-id="7ba20-127">Tooyour kvóta SUS-t a feladat másolatot választhat.</span><span class="sxs-lookup"><span data-stu-id="7ba20-127">You can select up tooyour quota in SUs for a job.</span></span> <span data-ttu-id="7ba20-128">Alapértelmezés szerint minden Azure-előfizetéssel rendelkezik tartozó kvóta másolatot az összes hello analytics-feladat too50 SUS-t egy adott régióban.</span><span class="sxs-lookup"><span data-stu-id="7ba20-128">By default, each Azure subscription has a quota of up too50 SUs for all hello analytics jobs in a specific region.</span></span> <span data-ttu-id="7ba20-129">SUs tooincrease meghaladja a kvótát, lépjen kapcsolatba az előfizetésekhez [Microsoft Support](http://support.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="7ba20-129">tooincrease SUs for your subscriptions beyond this quota, contact [Microsoft Support](http://support.microsoft.com).</span></span> <span data-ttu-id="7ba20-130">Érvényes SUs Feladatonkénti értékei 1, 3, 6, és a 6 lépésekben.</span><span class="sxs-lookup"><span data-stu-id="7ba20-130">Valid values for SUs per job are 1, 3, 6, and up in increments of 6.</span></span>

## <a name="embarrassingly-parallel-jobs"></a><span data-ttu-id="7ba20-131">Embarrassingly párhuzamos feladat</span><span class="sxs-lookup"><span data-stu-id="7ba20-131">Embarrassingly parallel jobs</span></span>
<span data-ttu-id="7ba20-132">Egy *embarrassingly párhuzamos* feladat a hello legjobban méretezhető lehetőséget az Azure Stream Analytics van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-132">An *embarrassingly parallel* job is hello most scalable scenario we have in Azure Stream Analytics.</span></span> <span data-ttu-id="7ba20-133">Egy partíció hello bemeneti tooone példány hello lekérdezés tooone partíció hello kimeneti csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="7ba20-133">It connects one partition of hello input tooone instance of hello query tooone partition of hello output.</span></span> <span data-ttu-id="7ba20-134">A párhuzamos végrehajtás rendelkezik hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="7ba20-134">This parallelism has hello following requirements:</span></span>

1. <span data-ttu-id="7ba20-135">Ha ugyanaz a kulcs feldolgozott hello függ a lekérdezés logika szerint hello azonos példány lekérdezéséhez meg kell győződnie arról, hogy hello események toohello nyissa meg a bemeneti partícióra.</span><span class="sxs-lookup"><span data-stu-id="7ba20-135">If your query logic depends on hello same key being processed by hello same query instance, you must make sure that hello events go toohello same partition of your input.</span></span> <span data-ttu-id="7ba20-136">Az event hubs számára ez azt jelenti, hogy hello eseményadatok kell hello **PartitionKey** érték beállítása.</span><span class="sxs-lookup"><span data-stu-id="7ba20-136">For event hubs, this means that hello event data must have hello **PartitionKey** value set.</span></span> <span data-ttu-id="7ba20-137">Másik lehetőségként particionált feladók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7ba20-137">Alternatively, you can use partitioned senders.</span></span> <span data-ttu-id="7ba20-138">A blob-tároló, ez azt jelenti, hogy hello események küldött toohello partíció mappában.</span><span class="sxs-lookup"><span data-stu-id="7ba20-138">For blob storage, this means that hello events are sent toohello same partition folder.</span></span> <span data-ttu-id="7ba20-139">Ha a lekérdezés logika nem követeli meg ugyanaz a kulcs feldolgozott toobe hello által hello azonos példány lekérdezéséhez, figyelmen kívül hagyhatja ezt a követelményt.</span><span class="sxs-lookup"><span data-stu-id="7ba20-139">If your query logic does not require hello same key toobe processed by hello same query instance, you can ignore this requirement.</span></span> <span data-ttu-id="7ba20-140">A logikai erre példa lehet egy egyszerű válassza-projekt-szűrő lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7ba20-140">An example of this logic would be a simple select-project-filter query.</span></span>  

2. <span data-ttu-id="7ba20-141">Miután hello adatok hello bemeneti oldalon elrendezését, meg kell győződnie arról, hogy a lekérdezés particionálva van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-141">Once hello data is laid out on hello input side, you must make sure that your query is partitioned.</span></span> <span data-ttu-id="7ba20-142">Ehhez toouse **Partition By** összes hello lépésben.</span><span class="sxs-lookup"><span data-stu-id="7ba20-142">This requires you toouse **Partition By** in all hello steps.</span></span> <span data-ttu-id="7ba20-143">Több lépést használhat, de mindegyikük hello kell particionálható ugyanazzal a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="7ba20-143">Multiple steps are allowed, but they all must be partitioned by hello same key.</span></span> <span data-ttu-id="7ba20-144">Jelenleg particionálási kulcs hello be kell állítani túl**PartitionId** ahhoz, hogy teljesen párhuzamos hello feladat toobe.</span><span class="sxs-lookup"><span data-stu-id="7ba20-144">Currently, hello partitioning key must be set too**PartitionId** in order for hello job toobe fully parallel.</span></span>  

3. <span data-ttu-id="7ba20-145">Jelenleg csak az event hubs és a blob storage támogatja a particionált kimeneti.</span><span class="sxs-lookup"><span data-stu-id="7ba20-145">Currently only event hubs and blob storage support partitioned output.</span></span> <span data-ttu-id="7ba20-146">Az event hub kimeneti, konfigurálnia kell a hello partíciós kulcs toobe **PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="7ba20-146">For event hub output, you must configure hello partition key toobe **PartitionId**.</span></span> <span data-ttu-id="7ba20-147">A blob storage kimenet toodo semmi sincs.</span><span class="sxs-lookup"><span data-stu-id="7ba20-147">For blob storage output, you don't have toodo anything.</span></span>  

4. <span data-ttu-id="7ba20-148">hello bemeneti partíciók száma hello kimeneti partíciók számának egyenlőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7ba20-148">hello number of input partitions must equal hello number of output partitions.</span></span> <span data-ttu-id="7ba20-149">BLOB storage-kimenet jelenleg nem támogatja a partíciókat.</span><span class="sxs-lookup"><span data-stu-id="7ba20-149">Blob storage output doesn't currently support partitions.</span></span> <span data-ttu-id="7ba20-150">Azonban ez nem probléma, mert a particionálási sémát hello fölérendelt lekérdezés hello örökli.</span><span class="sxs-lookup"><span data-stu-id="7ba20-150">But that's okay, because it inherits hello partitioning scheme of hello upstream query.</span></span> <span data-ttu-id="7ba20-151">Az alábbiakban néhány lehetséges, amelyek lehetővé teszik a teljes párhuzamos feladat partíció érték:</span><span class="sxs-lookup"><span data-stu-id="7ba20-151">Here are examples of partition values that allow a fully parallel job:</span></span>  

   * <span data-ttu-id="7ba20-152">8 event hub bemeneti partíciók és 8 eseményközpont kimeneti partíciók</span><span class="sxs-lookup"><span data-stu-id="7ba20-152">8 event hub input partitions and 8 event hub output partitions</span></span>
   * <span data-ttu-id="7ba20-153">8 event hub bemeneti partíciókat és a blob storage kimeneti</span><span class="sxs-lookup"><span data-stu-id="7ba20-153">8 event hub input partitions and blob storage output</span></span>  
   * <span data-ttu-id="7ba20-154">bemeneti partíciók 8 blob storage és a blob storage kimeneti</span><span class="sxs-lookup"><span data-stu-id="7ba20-154">8 blob storage input partitions and blob storage output</span></span>  
   * <span data-ttu-id="7ba20-155">8 blob-tároló bemeneti partíciók és 8 event hub kimeneti partíciók</span><span class="sxs-lookup"><span data-stu-id="7ba20-155">8 blob storage input partitions and 8 event hub output partitions</span></span>  

<span data-ttu-id="7ba20-156">hello alábbi szakaszok ismertetik, amelyek embarrassingly párhuzamos néhány példát.</span><span class="sxs-lookup"><span data-stu-id="7ba20-156">hello following sections discuss some example scenarios that are embarrassingly parallel.</span></span>

### <a name="simple-query"></a><span data-ttu-id="7ba20-157">Egyszerű lekérdezés</span><span class="sxs-lookup"><span data-stu-id="7ba20-157">Simple query</span></span>

* <span data-ttu-id="7ba20-158">Bemenet: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-158">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="7ba20-159">Kimeneti: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-159">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="7ba20-160">Lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="7ba20-160">Query:</span></span>

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

<span data-ttu-id="7ba20-161">Ez a lekérdezés egy egyszerű szűrési.</span><span class="sxs-lookup"><span data-stu-id="7ba20-161">This query is a simple filter.</span></span> <span data-ttu-id="7ba20-162">Ezért toohello eseményközpont elküldött hello bemeneti partícionálásra vonatkozó tooworry nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7ba20-162">Therefore, we don't need tooworry about partitioning hello input that is being sent toohello event hub.</span></span> <span data-ttu-id="7ba20-163">Figyelje meg, hogy hello lekérdezés tartalmazza **partíció által PartitionId**, így a korábbi #2 követelmény teljesít.</span><span class="sxs-lookup"><span data-stu-id="7ba20-163">Notice that hello query includes **Partition By PartitionId**, so it fulfills requirement #2 from earlier.</span></span> <span data-ttu-id="7ba20-164">Hello kimeneti, el kell tooconfigure hello event hub kimenetet a hello feladat toohave hello particionáló kulcskészlet túl**PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="7ba20-164">For hello output, we need tooconfigure hello event hub output in hello job toohave hello parition key set too**PartitionId**.</span></span> <span data-ttu-id="7ba20-165">Egy legutóbbi ellenőrzés arra, hogy a bemeneti partíciók száma hello kimeneti partíciók száma egyenlő toohello toomake.</span><span class="sxs-lookup"><span data-stu-id="7ba20-165">One last check is toomake sure that hello number of input partitions is equal toohello number of output partitions.</span></span>

### <a name="query-with-a-grouping-key"></a><span data-ttu-id="7ba20-166">Csoportosítás kulccsal lekérdezése</span><span class="sxs-lookup"><span data-stu-id="7ba20-166">Query with a grouping key</span></span>

* <span data-ttu-id="7ba20-167">Bemenet: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-167">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="7ba20-168">A kimenetre: A Blob storage</span><span class="sxs-lookup"><span data-stu-id="7ba20-168">Output: Blob storage</span></span>

<span data-ttu-id="7ba20-169">Lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="7ba20-169">Query:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="7ba20-170">Ez a lekérdezés csoportosítási kulccsal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7ba20-170">This query has a grouping key.</span></span> <span data-ttu-id="7ba20-171">Ezért hello azonos lekérdezése példányt, ami azt jelenti, hogy események küldött jogcímet kell elküldeni toohello event hubs egy particionált módon hello által feldolgozott azonos kulcsú igényeinek toobe.</span><span class="sxs-lookup"><span data-stu-id="7ba20-171">Therefore, hello same key needs toobe processed by hello same query instance, which means that events must be sent toohello event hub in a partitioned manner.</span></span> <span data-ttu-id="7ba20-172">De mely kulcsot kell használni?</span><span class="sxs-lookup"><span data-stu-id="7ba20-172">But which key should be used?</span></span> <span data-ttu-id="7ba20-173">**PartitionId** feladat-logika fogalom.</span><span class="sxs-lookup"><span data-stu-id="7ba20-173">**PartitionId** is a job-logic concept.</span></span> <span data-ttu-id="7ba20-174">hello kulcs ténylegesen tartjuk **TollBoothId**, így hello **PartitionKey** hello eseményadatok értékének meg kell **TollBoothId**.</span><span class="sxs-lookup"><span data-stu-id="7ba20-174">hello key we actually care about is **TollBoothId**, so hello **PartitionKey** value of hello event data should be **TollBoothId**.</span></span> <span data-ttu-id="7ba20-175">A Microsoft ehhez a hello lekérdezés **Partition By** túl**PartitionId**.</span><span class="sxs-lookup"><span data-stu-id="7ba20-175">We do this in hello query by setting **Partition By** too**PartitionId**.</span></span> <span data-ttu-id="7ba20-176">Mivel hello kimeneti blob-tároló, tooworry konfigurálásáról a partíciós kulcs értékre, követelmény #4 nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7ba20-176">Since hello output is blob storage, we don't need tooworry about configuring a partition key value, as per requirement #4.</span></span>

### <a name="multi-step-query-with-a-grouping-key"></a><span data-ttu-id="7ba20-177">Több lekérdezés csoportosítási kulccsal</span><span class="sxs-lookup"><span data-stu-id="7ba20-177">Multi-step query with a grouping key</span></span>
* <span data-ttu-id="7ba20-178">Bemenet: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-178">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="7ba20-179">Kimenet: Event hub példány 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-179">Output: Event hub instance with 8 partitions</span></span>

<span data-ttu-id="7ba20-180">Lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="7ba20-180">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="7ba20-181">Ez a lekérdezés tartalmaz egy csoportosítási kulcsot, így az azonos kulcsú igények toobe hello által feldolgozott hello lekérdezés ugyanezen példányában.</span><span class="sxs-lookup"><span data-stu-id="7ba20-181">This query has a grouping key, so hello same key needs toobe processed by hello same query instance.</span></span> <span data-ttu-id="7ba20-182">Használhatunk hello azonos stratégia hello előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="7ba20-182">We can use hello same strategy as in hello previous example.</span></span> <span data-ttu-id="7ba20-183">Ebben az esetben a hello a lekérdezés több lépést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7ba20-183">In this case, hello query has multiple steps.</span></span> <span data-ttu-id="7ba20-184">Az egyes lépések rendelkezik **partíció által PartitionId**?</span><span class="sxs-lookup"><span data-stu-id="7ba20-184">Does each step have **Partition By PartitionId**?</span></span> <span data-ttu-id="7ba20-185">Igen, így hello lekérdezés célkitűzéseinek #3 követelményt.</span><span class="sxs-lookup"><span data-stu-id="7ba20-185">Yes, so hello query fulfills requirement #3.</span></span> <span data-ttu-id="7ba20-186">Hello kimeneti, el kell tooset hello partíciós kulcs túl**PartitionId**, a fentiekben taglaltak.</span><span class="sxs-lookup"><span data-stu-id="7ba20-186">For hello output, we need tooset hello partition key too**PartitionId**, as discussed earlier.</span></span> <span data-ttu-id="7ba20-187">Azt is láthatja, hogy rendelkezik-e hello azonos számú partíciót hello bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="7ba20-187">We can also see that it has hello same number of partitions as hello input.</span></span>

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a><span data-ttu-id="7ba20-188">Példák, amelyek *nem* embarrassingly párhuzamos</span><span class="sxs-lookup"><span data-stu-id="7ba20-188">Example scenarios that are *not* embarrassingly parallel</span></span>

<span data-ttu-id="7ba20-189">Hello előző szakaszban néhány embarrassingly párhuzamos forgatókönyv bemutatta azt.</span><span class="sxs-lookup"><span data-stu-id="7ba20-189">In hello previous section, we showed some embarrassingly parallel scenarios.</span></span> <span data-ttu-id="7ba20-190">Ebben a szakaszban arról lesz szó, amelyek nem felelnek meg az összes hello követelmények toobe embarrassingly párhuzamos forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="7ba20-190">In this section, we discuss scenarios that don't meet all hello requirements toobe embarrassingly parallel.</span></span> 

### <a name="mismatched-partition-count"></a><span data-ttu-id="7ba20-191">Nem egyező partíciók száma</span><span class="sxs-lookup"><span data-stu-id="7ba20-191">Mismatched partition count</span></span>
* <span data-ttu-id="7ba20-192">Bemenet: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-192">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="7ba20-193">Kimenet: Eseményközpont 32 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-193">Output: Event hub with 32 partitions</span></span>

<span data-ttu-id="7ba20-194">Ebben az esetben nem számít, milyen hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7ba20-194">In this case, it doesn't matter what hello query is.</span></span> <span data-ttu-id="7ba20-195">Ha hello bemeneti partíciók száma nem egyezik meg a hello kimeneti partíciók száma, hello topológia embarrassingly párhuzamos nem.</span><span class="sxs-lookup"><span data-stu-id="7ba20-195">If hello input partition count doesn't match hello output partition count, hello topology isn't embarrassingly parallel.</span></span>

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a><span data-ttu-id="7ba20-196">Az event hubs vagy a blob-tároló nem használ kimenetként</span><span class="sxs-lookup"><span data-stu-id="7ba20-196">Not using event hubs or blob storage as output</span></span>
* <span data-ttu-id="7ba20-197">Bemenet: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-197">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="7ba20-198">Kimenet: Power BI</span><span class="sxs-lookup"><span data-stu-id="7ba20-198">Output: PowerBI</span></span>

<span data-ttu-id="7ba20-199">Power bi-kimenet jelenleg nem támogatja a particionálást.</span><span class="sxs-lookup"><span data-stu-id="7ba20-199">PowerBI output doesn't currently support partitioning.</span></span> <span data-ttu-id="7ba20-200">Emiatt az ebben a forgatókönyvben nincs embarrassingly párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="7ba20-200">Therefore, this scenario is not embarrassingly parallel.</span></span>

### <a name="multi-step-query-with-different-partition-by-values"></a><span data-ttu-id="7ba20-201">Több lekérdezés eltérő értékű Partition By</span><span class="sxs-lookup"><span data-stu-id="7ba20-201">Multi-step query with different Partition By values</span></span>
* <span data-ttu-id="7ba20-202">Bemenet: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-202">Input: Event hub with 8 partitions</span></span>
* <span data-ttu-id="7ba20-203">Kimeneti: Eseményközpont 8 partíciókkal rendelkező</span><span class="sxs-lookup"><span data-stu-id="7ba20-203">Output: Event hub with 8 partitions</span></span>

<span data-ttu-id="7ba20-204">Lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="7ba20-204">Query:</span></span>

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="7ba20-205">Ahogy látja, használja a hello második lépésben **TollBoothId** particionálási kulcs hello szerint.</span><span class="sxs-lookup"><span data-stu-id="7ba20-205">As you can see, hello second step uses **TollBoothId** as hello partitioning key.</span></span> <span data-ttu-id="7ba20-206">Ezt a lépést nem van hello azonos hello első lépéseként, és ezért szükség van, egy véletlen toodo.</span><span class="sxs-lookup"><span data-stu-id="7ba20-206">This step is not hello same as hello first step, and it therefore requires us toodo a shuffle.</span></span> 

<span data-ttu-id="7ba20-207">hello előző példák azt szemléltetik, néhány Stream Analytics-feladatok egy embarrassingly párhuzamos topológia megfelelő túl (és nem).</span><span class="sxs-lookup"><span data-stu-id="7ba20-207">hello preceding examples show some Stream Analytics jobs that conform too(or don't) an embarrassingly parallel topology.</span></span> <span data-ttu-id="7ba20-208">Felelnek meg, ha rendelkeznek hello lehetséges, hogy a maximális skálája.</span><span class="sxs-lookup"><span data-stu-id="7ba20-208">If they do conform, they have hello potential for maximum scale.</span></span> <span data-ttu-id="7ba20-209">Feladatok, amelyek nem felelnek meg ezeket a profilokat, útmutatást skálázás egyik le lesz elérhető a jövőben frissíti.</span><span class="sxs-lookup"><span data-stu-id="7ba20-209">For jobs that don't fit one of these profiles, scaling guidance will be available in future updates.</span></span> <span data-ttu-id="7ba20-210">Most használja a következő részekben hello hello általános útmutatást.</span><span class="sxs-lookup"><span data-stu-id="7ba20-210">For now, use hello general guidance in hello following sections.</span></span>

## <a name="calculate-hello-maximum-streaming-units-of-a-job"></a><span data-ttu-id="7ba20-211">A feladatok streamelési egységek maximális hello kiszámítása</span><span class="sxs-lookup"><span data-stu-id="7ba20-211">Calculate hello maximum streaming units of a job</span></span>
<span data-ttu-id="7ba20-212">hello adatfolyam-továbbítási egységek száma, amelyek segítségével a Stream Analytics-feladat hello számos lépés hello lekérdezésben definiált hello feladat, és minden egyes partíciók száma hello függ.</span><span class="sxs-lookup"><span data-stu-id="7ba20-212">hello total number of streaming units that can be used by a Stream Analytics job depends on hello number of steps in hello query defined for hello job and hello number of partitions for each step.</span></span>

### <a name="steps-in-a-query"></a><span data-ttu-id="7ba20-213">A lekérdezésben lépései</span><span class="sxs-lookup"><span data-stu-id="7ba20-213">Steps in a query</span></span>
<span data-ttu-id="7ba20-214">A lekérdezés egy vagy több lépéseket rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="7ba20-214">A query can have one or many steps.</span></span> <span data-ttu-id="7ba20-215">Az egyes lépések hello által meghatározott segédlekérdezésben **WITH** kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="7ba20-215">Each step is a subquery defined by hello **WITH** keyword.</span></span> <span data-ttu-id="7ba20-216">hello kívül hello-lekérdezést **WITH** kulcsszó (csak egy lekérdezést) is számít egy lépést, például a hello **VÁLASSZA** utasítás a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="7ba20-216">hello query that is outside hello **WITH** keyword (one query only) is also counted as a step, such as hello **SELECT** statement in hello following query:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

<span data-ttu-id="7ba20-217">Ez a lekérdezés két lépést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7ba20-217">This query has two steps.</span></span>

> [!NOTE]
> <span data-ttu-id="7ba20-218">Ez a lekérdezés tárgyalt hello cikk későbbi részében részletesebben.</span><span class="sxs-lookup"><span data-stu-id="7ba20-218">This query is discussed in more detail later in hello article.</span></span>
>  

### <a name="partition-a-step"></a><span data-ttu-id="7ba20-219">A lépés particionálása</span><span class="sxs-lookup"><span data-stu-id="7ba20-219">Partition a step</span></span>
<span data-ttu-id="7ba20-220">A következő feltételek hello particionálás egy lépés szükséges:</span><span class="sxs-lookup"><span data-stu-id="7ba20-220">Partitioning a step requires hello following conditions:</span></span>

* <span data-ttu-id="7ba20-221">hello bemeneti forrás kell particionálni.</span><span class="sxs-lookup"><span data-stu-id="7ba20-221">hello input source must be partitioned.</span></span> 
* <span data-ttu-id="7ba20-222">Hello **válasszon** hello lekérdezési utasítás particionált bemeneti forrásból kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="7ba20-222">hello **SELECT** statement of hello query must read from a partitioned input source.</span></span>
* <span data-ttu-id="7ba20-223">hello lekérdezés hello lépés belül rendelkeznie kell hello **Partition By** kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="7ba20-223">hello query within hello step must have hello **Partition By** keyword.</span></span>

<span data-ttu-id="7ba20-224">A lekérdezés particionálva van, amikor hello bemeneti események feldolgozott és az összesített külön partíciócsoportok, és kimenetek az események az egyes hello csoportok hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="7ba20-224">When a query is partitioned, hello input events are processed and aggregated in separate partition groups, and outputs events are generated for each of hello groups.</span></span> <span data-ttu-id="7ba20-225">Ha azt szeretné, hogy egy kombinált összesítést, létre kell hoznia egy második lépésben nem particionált tooaggregate.</span><span class="sxs-lookup"><span data-stu-id="7ba20-225">If you want a combined aggregate, you must create a second non-partitioned step tooaggregate.</span></span>

### <a name="calculate-hello-max-streaming-units-for-a-job"></a><span data-ttu-id="7ba20-226">Adatfolyam-egységek feladat hello maximális kiszámítása</span><span class="sxs-lookup"><span data-stu-id="7ba20-226">Calculate hello max streaming units for a job</span></span>
<span data-ttu-id="7ba20-227">Minden nem particionált lépést együtt költenie toosix adatfolyam-egységek (SUS-t) egy Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="7ba20-227">All non-partitioned steps together can scale up toosix streaming units (SUs) for a Stream Analytics job.</span></span> <span data-ttu-id="7ba20-228">kell particionálni a tooadd SUS-t, a lépést.</span><span class="sxs-lookup"><span data-stu-id="7ba20-228">tooadd SUs, a step must be partitioned.</span></span> <span data-ttu-id="7ba20-229">Mindegyik partíció lehet hat SUS-t.</span><span class="sxs-lookup"><span data-stu-id="7ba20-229">Each partition can have six SUs.</span></span>

<table border="1">
<tr><th><span data-ttu-id="7ba20-230">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="7ba20-230">Query</span></span></th><th><span data-ttu-id="7ba20-231">Maximális SUs hello feladat</span><span class="sxs-lookup"><span data-stu-id="7ba20-231">Max SUs for hello job</span></span></th></td>

<tr><td>
<ul>
<li><span data-ttu-id="7ba20-232">hello lekérdezés tartalmaz egy lépésben.</span><span class="sxs-lookup"><span data-stu-id="7ba20-232">hello query contains one step.</span></span></li>
<li><span data-ttu-id="7ba20-233">hello lépés nincs particionálva.</span><span class="sxs-lookup"><span data-stu-id="7ba20-233">hello step is not partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="7ba20-234">6</span><span class="sxs-lookup"><span data-stu-id="7ba20-234">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="7ba20-235">a bemeneti adatfolyam hello 3 particionálva van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-235">hello input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="7ba20-236">hello lekérdezés tartalmaz egy lépésben.</span><span class="sxs-lookup"><span data-stu-id="7ba20-236">hello query contains one step.</span></span></li>
<li><span data-ttu-id="7ba20-237">hello lépés particionálva van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-237">hello step is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="7ba20-238">18</span><span class="sxs-lookup"><span data-stu-id="7ba20-238">18</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="7ba20-239">hello lekérdezés két lépést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7ba20-239">hello query contains two steps.</span></span></li>
<li><span data-ttu-id="7ba20-240">Hello lépések egyike sem particionálva van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-240">Neither of hello steps is partitioned.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="7ba20-241">6</span><span class="sxs-lookup"><span data-stu-id="7ba20-241">6</span></span></td></tr>

<tr><td>
<ul>
<li><span data-ttu-id="7ba20-242">a bemeneti adatfolyam hello 3 particionálva van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-242">hello input data stream is partitioned by 3.</span></span></li>
<li><span data-ttu-id="7ba20-243">hello lekérdezés két lépést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7ba20-243">hello query contains two steps.</span></span> <span data-ttu-id="7ba20-244">hello bemeneti lépés particionálva van, és hello a második lépés.</span><span class="sxs-lookup"><span data-stu-id="7ba20-244">hello input step is partitioned and hello second step is not.</span></span></li>
<li><span data-ttu-id="7ba20-245">Hello <strong>válasszon</strong> particionálva hello bemeneti olvassa be az utasítást.</span><span class="sxs-lookup"><span data-stu-id="7ba20-245">hello <strong>SELECT</strong> statement reads from hello partitioned input.</span></span></li>
</ul>
</td>
<td><span data-ttu-id="7ba20-246">(a particionált lépéseket 18) + 6. lépéseket nem particionált 24</span><span class="sxs-lookup"><span data-stu-id="7ba20-246">24 (18 for partitioned steps + 6 for non-partitioned steps)</span></span></td></tr>
</table>

### <a name="examples-of-scaling"></a><span data-ttu-id="7ba20-247">Példák a méretezés</span><span class="sxs-lookup"><span data-stu-id="7ba20-247">Examples of scaling</span></span>

<span data-ttu-id="7ba20-248">hello következő lekérdezés számítja ki, amely rendelkezik a három tollbooths téren állomás áthaladás három perces ablak autók hello száma.</span><span class="sxs-lookup"><span data-stu-id="7ba20-248">hello following query calculates hello number of cars within a three-minute window going through a toll station that has three tollbooths.</span></span> <span data-ttu-id="7ba20-249">Ez a lekérdezés akár toosix SUS-t is méretezhető.</span><span class="sxs-lookup"><span data-stu-id="7ba20-249">This query can be scaled up toosix SUs.</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="7ba20-250">További toouse SUS-t a hello lekérdezés, mind a bemeneti adatfolyam hello és kell particionálni a hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7ba20-250">toouse more SUs for hello query, both hello input data stream and hello query must be partitioned.</span></span> <span data-ttu-id="7ba20-251">Mivel hello adatok adatfolyam partíció too3 van állítva, hello következő módosított lekérdezés akár is méretezhető too18 SUs:</span><span class="sxs-lookup"><span data-stu-id="7ba20-251">Since hello data stream partition is set too3, hello following modified query can be scaled up too18 SUs:</span></span>

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

<span data-ttu-id="7ba20-252">Amikor lekérdezést particionálva van, a hello bemeneti események feldolgozása, külön partíciócsoportok összesíteni.</span><span class="sxs-lookup"><span data-stu-id="7ba20-252">When a query is partitioned, hello input events are processed and aggregated in separate partition groups.</span></span> <span data-ttu-id="7ba20-253">A kimeneti eseményekben is vonatkozóan hello csoportok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="7ba20-253">Output events are also generated for each of hello groups.</span></span> <span data-ttu-id="7ba20-254">Particionálás okozhat, ha hello néhány váratlan eredményekhez **GROUP BY** mező nincs a bemeneti adatfolyam hello hello partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="7ba20-254">Partitioning can cause some unexpected results when hello **GROUP BY** field is not hello partition key in hello input data stream.</span></span> <span data-ttu-id="7ba20-255">Például hello **TollBoothId** hello előző lekérdezés mezője nincs hello partíciós kulcs a **Input1**.</span><span class="sxs-lookup"><span data-stu-id="7ba20-255">For example, hello **TollBoothId** field in hello previous query is not hello partition key of **Input1**.</span></span> <span data-ttu-id="7ba20-256">hello eredménye, hogy hello őrbódét #1 adatait a több partíciót kell terjed.</span><span class="sxs-lookup"><span data-stu-id="7ba20-256">hello result is that hello data from TollBooth #1 can be spread in multiple partitions.</span></span>

<span data-ttu-id="7ba20-257">Egyes hello **Input1** partíciók dolgoz fel külön Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="7ba20-257">Each of hello **Input1** partitions will be processed separately by Stream Analytics.</span></span> <span data-ttu-id="7ba20-258">Ennek eredményeképpen hello car száma több rekordnyi hello hello azonos Átfedésmentes ablak jön létre a azonos őrbódét.</span><span class="sxs-lookup"><span data-stu-id="7ba20-258">As a result, multiple records of hello car count for hello same tollbooth in hello same Tumbling window will be created.</span></span> <span data-ttu-id="7ba20-259">Hello bemeneti partíciós kulcs nem módosítható, ha a probléma egy partíción kívüli lépés, mint például a következő hello hozzáadásával lehet meghatározni:</span><span class="sxs-lookup"><span data-stu-id="7ba20-259">If hello input partition key can't be changed, this problem can be fixed by adding a non-partition step, as in hello following example:</span></span>

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

<span data-ttu-id="7ba20-260">Ez a lekérdezés lehet méretezett too24 SUS-t.</span><span class="sxs-lookup"><span data-stu-id="7ba20-260">This query can be scaled too24 SUs.</span></span>

> [!NOTE]
> <span data-ttu-id="7ba20-261">Ha a két adatfolyam csatlakozik, győződjön meg arról, hogy hello adatfolyamok particionáltak hello partíciós kulcs hello oszlop által használt toocreate hello illesztéseket.</span><span class="sxs-lookup"><span data-stu-id="7ba20-261">If you are joining two streams, make sure that hello streams are partitioned by hello partition key of hello column that you use toocreate hello joins.</span></span> <span data-ttu-id="7ba20-262">Győződjön meg arról, hogy rendelkezik-e hello is azonos számú mindkét adatfolyamokat a partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="7ba20-262">Also make sure that you have hello same number of partitions in both streams.</span></span>
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a><span data-ttu-id="7ba20-263">Konfigurálja a Stream Analytics adatfolyam-egységek</span><span class="sxs-lookup"><span data-stu-id="7ba20-263">Configure Stream Analytics streaming units</span></span>

1. <span data-ttu-id="7ba20-264">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7ba20-264">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7ba20-265">Az erőforrások listájához hello keresse meg hello Stream Analytics-feladat tooscale szeretne, és nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="7ba20-265">In hello list of resources, find hello Stream Analytics job that you want tooscale and then open it.</span></span>
3. <span data-ttu-id="7ba20-266">A hello feladat panelen a **konfigurálása**, kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="7ba20-266">In hello job blade, under **Configure**, click **Scale**.</span></span>

    ![Az Azure Stream Analytics feladat beállítása][img.stream.analytics.preview.portal.settings.scale]

4. <span data-ttu-id="7ba20-268">Hello feladat hello csúszkát tooset hello SUS-t használja.</span><span class="sxs-lookup"><span data-stu-id="7ba20-268">Use hello slider tooset hello SUs for hello job.</span></span> <span data-ttu-id="7ba20-269">Figyelje meg, hogy-e korlátozott toospecific SU beállításait.</span><span class="sxs-lookup"><span data-stu-id="7ba20-269">Notice that you are limited toospecific SU settings.</span></span>


## <a name="monitor-job-performance"></a><span data-ttu-id="7ba20-270">Feladat teljesítmény figyelése</span><span class="sxs-lookup"><span data-stu-id="7ba20-270">Monitor job performance</span></span>
<span data-ttu-id="7ba20-271">Hello Azure-portál használatával, nyomon követheti a feladatok hello átviteli:</span><span class="sxs-lookup"><span data-stu-id="7ba20-271">Using hello Azure portal, you can track hello throughput of a job:</span></span>

![Az Azure Stream Analytics feladatok figyelése][img.stream.analytics.monitor.job]

<span data-ttu-id="7ba20-273">Várt hello munkaterhelések átviteli sebessége hello kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="7ba20-273">Calculate hello expected throughput of hello workload.</span></span> <span data-ttu-id="7ba20-274">Ha hello átviteli kisebb a vártnál, hello bemeneti partíció hangolására, hello lekérdezés hangolása és SUs tooyour feladat hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7ba20-274">If hello throughput is less than expected, tune hello input partition, tune hello query, and add SUs tooyour job.</span></span>


## <a name="visualize-stream-analytics-throughput-at-scale-hello-raspberry-pi-scenario"></a><span data-ttu-id="7ba20-275">A Stream Analytics átviteli léptékű megjelenítése: hello málna Pi forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="7ba20-275">Visualize Stream Analytics throughput at scale: hello Raspberry Pi scenario</span></span>
<span data-ttu-id="7ba20-276">megismerte a Stream Analytics-feladatok méretezése hogyan toohelp, azt végre kísérlet málna Pi eszköz visszajelzései alapján.</span><span class="sxs-lookup"><span data-stu-id="7ba20-276">toohelp you understand how Stream Analytics jobs scale, we performed an experiment based on input from a Raspberry Pi device.</span></span> <span data-ttu-id="7ba20-277">Ehhez a kísérlethez tudassa velünk látni több adatfolyam-továbbítási egységek és partíciók átviteli hello hatását.</span><span class="sxs-lookup"><span data-stu-id="7ba20-277">This experiment let us see hello effect on throughput of multiple streaming units and partitions.</span></span>

<span data-ttu-id="7ba20-278">Ebben a forgatókönyvben a hello eszköz érzékelő adatokat (ügyfelek) tooan eseményközpont küld.</span><span class="sxs-lookup"><span data-stu-id="7ba20-278">In this scenario, hello device sends sensor data (clients) tooan event hub.</span></span> <span data-ttu-id="7ba20-279">Streaming Analytics hello adatokat dolgozza fel, és riasztást vagy statisztika küld kimeneti tooanother eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="7ba20-279">Streaming Analytics processes hello data and sends an alert or statistics as an output tooanother event hub.</span></span> 

<span data-ttu-id="7ba20-280">hello ügyfél érzékelő adatokat küld a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="7ba20-280">hello client sends sensor data in JSON format.</span></span> <span data-ttu-id="7ba20-281">hello adatokat tartalmazó kimenetével is JSON formátumban van.</span><span class="sxs-lookup"><span data-stu-id="7ba20-281">hello data output is also in JSON format.</span></span> <span data-ttu-id="7ba20-282">hello adatok így néz ki:</span><span class="sxs-lookup"><span data-stu-id="7ba20-282">hello data looks like this:</span></span>

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

<span data-ttu-id="7ba20-283">a következő lekérdezés hello használt toosend riasztás esetén egy világos ki kell kapcsolni:</span><span class="sxs-lookup"><span data-stu-id="7ba20-283">hello following query is used toosend an alert when a light is switched off:</span></span>

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a><span data-ttu-id="7ba20-284">Mérték átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="7ba20-284">Measure throughput</span></span>

<span data-ttu-id="7ba20-285">Ebben a környezetben átviteli érték hello bemeneti adatfeldolgozási Stream Analytics egy rögzített időn belül.</span><span class="sxs-lookup"><span data-stu-id="7ba20-285">In this context, throughput is hello amount of input data processed by Stream Analytics in a fixed amount of time.</span></span> <span data-ttu-id="7ba20-286">(Jelenleg mért 10 perc.) tooachieve hello legjobb feldolgozási adatátviteli sebességét hello bemeneti adatai, mindkét hello adatfolyam bemeneti és hello lekérdezés volt particionálva.</span><span class="sxs-lookup"><span data-stu-id="7ba20-286">(We measured for 10 minutes.) tooachieve hello best processing throughput for hello input data, both hello data stream input and hello query were  partitioned.</span></span> <span data-ttu-id="7ba20-287">Azt a részét **COUNT()** hello lekérdezés toomeasure a bemeneti események feldolgozása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="7ba20-287">We included **COUNT()** in hello query toomeasure how many input events were processed.</span></span> <span data-ttu-id="7ba20-288">a bemeneti események toocome nem egyszerűen várakozási toomake meg arról, hogy hello feladat, mindegyik partíciójához hello bemeneti eseményközpont által előre betöltött bemeneti adatokat körülbelül 300 MB.</span><span class="sxs-lookup"><span data-stu-id="7ba20-288">toomake sure hello job was not simply waiting for input events toocome, each partition of hello input event hub was preloaded with about 300 MB of input data.</span></span>

<span data-ttu-id="7ba20-289">hello következő táblázat azt növelni a streamelési egységek számának hello és hello megfelelő partíció található, az event hubs látott hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="7ba20-289">hello following table shows hello results we saw when we increased hello number of streaming units and hello corresponding partition counts in event hubs.</span></span>  

<table border="1">
<tr><th><span data-ttu-id="7ba20-290">Bemeneti partíciók</span><span class="sxs-lookup"><span data-stu-id="7ba20-290">Input Partitions</span></span></th><th><span data-ttu-id="7ba20-291">Kimeneti partíciók</span><span class="sxs-lookup"><span data-stu-id="7ba20-291">Output Partitions</span></span></th><th><span data-ttu-id="7ba20-292">Folyamatos átviteli egységek</span><span class="sxs-lookup"><span data-stu-id="7ba20-292">Streaming Units</span></span></th><th><span data-ttu-id="7ba20-293">Fenntartható</span><span class="sxs-lookup"><span data-stu-id="7ba20-293">Sustained Throughput</span></span>
</th></td>

<tr><td><span data-ttu-id="7ba20-294">12</span><span class="sxs-lookup"><span data-stu-id="7ba20-294">12</span></span></td>
<td><span data-ttu-id="7ba20-295">12</span><span class="sxs-lookup"><span data-stu-id="7ba20-295">12</span></span></td>
<td><span data-ttu-id="7ba20-296">6</span><span class="sxs-lookup"><span data-stu-id="7ba20-296">6</span></span></td>
<td><span data-ttu-id="7ba20-297">4.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="7ba20-297">4.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="7ba20-298">12</span><span class="sxs-lookup"><span data-stu-id="7ba20-298">12</span></span></td>
<td><span data-ttu-id="7ba20-299">12</span><span class="sxs-lookup"><span data-stu-id="7ba20-299">12</span></span></td>
<td><span data-ttu-id="7ba20-300">12</span><span class="sxs-lookup"><span data-stu-id="7ba20-300">12</span></span></td>
<td><span data-ttu-id="7ba20-301">8.06 MB/s</span><span class="sxs-lookup"><span data-stu-id="7ba20-301">8.06 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="7ba20-302">48</span><span class="sxs-lookup"><span data-stu-id="7ba20-302">48</span></span></td>
<td><span data-ttu-id="7ba20-303">48</span><span class="sxs-lookup"><span data-stu-id="7ba20-303">48</span></span></td>
<td><span data-ttu-id="7ba20-304">48</span><span class="sxs-lookup"><span data-stu-id="7ba20-304">48</span></span></td>
<td><span data-ttu-id="7ba20-305">38.32 MB/s</span><span class="sxs-lookup"><span data-stu-id="7ba20-305">38.32 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="7ba20-306">192</span><span class="sxs-lookup"><span data-stu-id="7ba20-306">192</span></span></td>
<td><span data-ttu-id="7ba20-307">192</span><span class="sxs-lookup"><span data-stu-id="7ba20-307">192</span></span></td>
<td><span data-ttu-id="7ba20-308">192</span><span class="sxs-lookup"><span data-stu-id="7ba20-308">192</span></span></td>
<td><span data-ttu-id="7ba20-309">172.67 MB/s</span><span class="sxs-lookup"><span data-stu-id="7ba20-309">172.67 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="7ba20-310">480</span><span class="sxs-lookup"><span data-stu-id="7ba20-310">480</span></span></td>
<td><span data-ttu-id="7ba20-311">480</span><span class="sxs-lookup"><span data-stu-id="7ba20-311">480</span></span></td>
<td><span data-ttu-id="7ba20-312">480</span><span class="sxs-lookup"><span data-stu-id="7ba20-312">480</span></span></td>
<td><span data-ttu-id="7ba20-313">454.27 MB/s</span><span class="sxs-lookup"><span data-stu-id="7ba20-313">454.27 MB/s</span></span></td>
</tr>

<tr><td><span data-ttu-id="7ba20-314">720</span><span class="sxs-lookup"><span data-stu-id="7ba20-314">720</span></span></td>
<td><span data-ttu-id="7ba20-315">720</span><span class="sxs-lookup"><span data-stu-id="7ba20-315">720</span></span></td>
<td><span data-ttu-id="7ba20-316">720</span><span class="sxs-lookup"><span data-stu-id="7ba20-316">720</span></span></td>
<td><span data-ttu-id="7ba20-317">609.69 MB/s</span><span class="sxs-lookup"><span data-stu-id="7ba20-317">609.69 MB/s</span></span></td>
</tr>
</table>

<span data-ttu-id="7ba20-318">És hello következő grafikon azt ábrázolja, SUS-t és átviteli hello kapcsolatát a képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="7ba20-318">And hello following graph shows a visualization of hello relationship between SUs and throughput.</span></span>

![img.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a><span data-ttu-id="7ba20-320">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="7ba20-320">Get help</span></span>
<span data-ttu-id="7ba20-321">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="7ba20-321">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ba20-322">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ba20-322">Next steps</span></span>
* [<span data-ttu-id="7ba20-323">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="7ba20-323">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="7ba20-324">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="7ba20-324">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="7ba20-325">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="7ba20-325">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="7ba20-326">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="7ba20-326">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

