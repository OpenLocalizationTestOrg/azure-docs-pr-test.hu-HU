---
title: "Az Azure Stream Analytics adatvezérelt hibakeresést a feladat ábra keresztül |} Microsoft Docs"
description: "A Stream Analytics-feladat hibaelhárítása a feladat ábra és metrikák használatával."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/01/2017
ms.author: jeffstok
ms.openlocfilehash: 4e5949232e8377b7697eaebf96eacdc31c4f5422
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="data-driven-debugging-by-using-the-job-diagram"></a><span data-ttu-id="47a37-103">Adatalapú hibakeresést keresztül a feladat ábra</span><span class="sxs-lookup"><span data-stu-id="47a37-103">Data-driven debugging by using the job diagram</span></span>

<span data-ttu-id="47a37-104">A feladat ábra: a **figyelés** panel az Azure portál segítségével jelenítheti meg a feladat folyamat.</span><span class="sxs-lookup"><span data-stu-id="47a37-104">The job diagram on the **Monitoring** blade in the Azure portal can help you visualize your job pipeline.</span></span> <span data-ttu-id="47a37-105">Azt illusztrálja, bemenetek, kimenetek és lekérdezések lépéseit.</span><span class="sxs-lookup"><span data-stu-id="47a37-105">It shows inputs, outputs, and query steps.</span></span> <span data-ttu-id="47a37-106">A feladat ábra segítségével vizsgálja meg a metrikák minden egyes lépést, a problémák elhárításakor gyorsabban elkülöníteni a probléma forrását.</span><span class="sxs-lookup"><span data-stu-id="47a37-106">You can use the job diagram to examine the metrics for each step, to more quickly isolate the source of a problem when you troubleshoot issues.</span></span>

## <a name="using-the-job-diagram"></a><span data-ttu-id="47a37-107">A feladat ábra használatával</span><span class="sxs-lookup"><span data-stu-id="47a37-107">Using the job diagram</span></span>

<span data-ttu-id="47a37-108">Az Azure-portálon alatt a Stream Analytics-feladatot, miközben **támogatási + hibaelhárítás**, jelölje be **feladat ábra**:</span><span class="sxs-lookup"><span data-stu-id="47a37-108">In the Azure portal, while in a Stream Analytics job, under **SUPPORT + TROUBLESHOOTING**, select **Job diagram**:</span></span>

![A metrika - hely feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

<span data-ttu-id="47a37-110">Válassza ki az összes lekérdezés lépést a lekérdezés-szerkesztő ablakban megfelelő szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="47a37-110">Select each query step to see the corresponding section in a query editing pane.</span></span> <span data-ttu-id="47a37-111">A lépés metrika diagramot az oldalon egy alsó ablaktáblán jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="47a37-111">A metric chart for the step is displayed in a lower pane on the page.</span></span>

![A metrika - alapvető feladat feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

<span data-ttu-id="47a37-113">A partíciók az Azure Event Hubs bemeneti megtekintéséhez válasszon **...**</span><span class="sxs-lookup"><span data-stu-id="47a37-113">To see the partitions of the Azure Event Hubs input, select **. . .**</span></span> <span data-ttu-id="47a37-114">A helyi menü megjelenik.</span><span class="sxs-lookup"><span data-stu-id="47a37-114">A context menu appears.</span></span> <span data-ttu-id="47a37-115">A bemeneti egyesülés is látható.</span><span class="sxs-lookup"><span data-stu-id="47a37-115">You also can see the input merger.</span></span>

![A metrika - feladat ábra bontsa ki a partíció](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

<span data-ttu-id="47a37-117">A metrika csak egyetlen partícióra diagramot, jelölje ki a partíció csomópont.</span><span class="sxs-lookup"><span data-stu-id="47a37-117">To see the metric chart for only a single partition, select the partition node.</span></span> <span data-ttu-id="47a37-118">A metrikák jelennek meg a lap alján.</span><span class="sxs-lookup"><span data-stu-id="47a37-118">The metrics are shown at the bottom of the page.</span></span>

![A metrika - további metrikák feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

<span data-ttu-id="47a37-120">A metrikák diagramot egyesülés megtekintéséhez jelölje ki az egyesülés csomópontot.</span><span class="sxs-lookup"><span data-stu-id="47a37-120">To see the metrics chart for a merger, select the merger node.</span></span> <span data-ttu-id="47a37-121">Az alábbi ábra mutatja, hogy események nem eldobni vagy módosítani.</span><span class="sxs-lookup"><span data-stu-id="47a37-121">The following chart shows that no events were dropped or adjusted.</span></span>

![A metrika - feladat ábra rács](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

<span data-ttu-id="47a37-123">A metrika értékét, és az idő a részletek megtekintéséhez, majd a diagram.</span><span class="sxs-lookup"><span data-stu-id="47a37-123">To see the details of the metric value and time, point to the chart.</span></span>

![Diagram metrikákat a sikertelen feladat - vigye](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a><span data-ttu-id="47a37-125">Hibaelhárítása mérőszámok segítségével</span><span class="sxs-lookup"><span data-stu-id="47a37-125">Troubleshoot by using metrics</span></span>

<span data-ttu-id="47a37-126">A **QueryLastProcessedTime** metrika azt jelzi, ha egy adott lépésre adatokat kapott.</span><span class="sxs-lookup"><span data-stu-id="47a37-126">The **QueryLastProcessedTime** metric indicates when a specific step received data.</span></span> <span data-ttu-id="47a37-127">A topológia megtekintésével is dolgozhat visszafelé a kimeneti processzor, hogy mely lépés nem kapja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="47a37-127">By looking at the topology, you can work backward from the output processor to see which step is not receiving data.</span></span> <span data-ttu-id="47a37-128">Ha a lépés nem sikerül adatokat, ugorjon a lekérdezés azt megelőzően.</span><span class="sxs-lookup"><span data-stu-id="47a37-128">If a step is not getting data, go to the query step just before it.</span></span> <span data-ttu-id="47a37-129">Ellenőrizze, hogy rendelkezik-e az előző lekérdezés lépés egy olyan időkeretet, és ha elegendő idő az megfelelt kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="47a37-129">Check whether the preceding query step has a time window, and if enough time has passed for it to output data.</span></span> <span data-ttu-id="47a37-130">(Vegye figyelembe, időpont windows rendszer diagramobjektum a óra.)</span><span class="sxs-lookup"><span data-stu-id="47a37-130">(Note that time windows are snapped to the hour.)</span></span>
 
<span data-ttu-id="47a37-131">Ha az előző lekérdezés lépés egy bemeneti processzor, használja a bemeneti metrikák amely választ adhat ezekre a következő célzott kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="47a37-131">If the preceding query step is an input processor, use the input metrics to help answer the following targeted questions.</span></span> <span data-ttu-id="47a37-132">Segít meghatározni, hogy egy feladat van adat lekérésekor a bemeneti forrásból.</span><span class="sxs-lookup"><span data-stu-id="47a37-132">They can help you determine whether a job is getting data from its input sources.</span></span> <span data-ttu-id="47a37-133">Ha a lekérdezés particionálva van, vizsgálja meg az összes partíciót.</span><span class="sxs-lookup"><span data-stu-id="47a37-133">If the query is partitioned, examine each partition.</span></span>
 
### <a name="how-much-data-is-being-read"></a><span data-ttu-id="47a37-134">Mennyi adatot olvasása?</span><span class="sxs-lookup"><span data-stu-id="47a37-134">How much data is being read?</span></span>

*   <span data-ttu-id="47a37-135">**InputEventsSourcesTotal** olvasható adatok egységek száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-135">**InputEventsSourcesTotal** is the number of data units read.</span></span> <span data-ttu-id="47a37-136">Ha például a blobok száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-136">For example, the number of blobs.</span></span>
*   <span data-ttu-id="47a37-137">**InputEventsTotal** olvasási események száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-137">**InputEventsTotal** is the number of events read.</span></span> <span data-ttu-id="47a37-138">Ez a metrika partíciónként érhető el.</span><span class="sxs-lookup"><span data-stu-id="47a37-138">This metric is available per partition.</span></span>
*   <span data-ttu-id="47a37-139">**InputEventsInBytesTotal** olvasott bájtok száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-139">**InputEventsInBytesTotal** is the number of bytes read.</span></span>
*   <span data-ttu-id="47a37-140">**InputEventsLastArrivalTime** minden fogadott események a várólistában levő időpontja frissül.</span><span class="sxs-lookup"><span data-stu-id="47a37-140">**InputEventsLastArrivalTime** is updated with every received event's enqueued time.</span></span>
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a><span data-ttu-id="47a37-141">Az idő soron?</span><span class="sxs-lookup"><span data-stu-id="47a37-141">Is time moving forward?</span></span> <span data-ttu-id="47a37-142">Ha tényleges események olvasható, előfordulhat, hogy nem adható ki absztrakt.</span><span class="sxs-lookup"><span data-stu-id="47a37-142">If actual events are read, punctuation might not be issued.</span></span>

*   <span data-ttu-id="47a37-143">Az **InputEventsLastPunctuationTime** megadja, hogy mikor adtak ki írásjelek, az idő előrehaladása érdekében.</span><span class="sxs-lookup"><span data-stu-id="47a37-143">**InputEventsLastPunctuationTime** indicates when a punctuation was issued to keep time moving forward.</span></span> <span data-ttu-id="47a37-144">Ha nem jelenik meg absztrakt, adatfolyama is blokkolnánk a hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="47a37-144">If punctuation is not issued, data flow can get blocked.</span></span>
 
### <a name="are-there-any-errors-in-the-input"></a><span data-ttu-id="47a37-145">Vannak-e hibák a bemeneti adatok?</span><span class="sxs-lookup"><span data-stu-id="47a37-145">Are there any errors in the input?</span></span>

*   <span data-ttu-id="47a37-146">**InputEventsEventDataNullTotal** null adattal rendelkező események száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-146">**InputEventsEventDataNullTotal** is a count of events that have null data.</span></span>
*   <span data-ttu-id="47a37-147">**InputEventsSerializerErrorsTotal** nem deszerializálhatók megfelelően események száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-147">**InputEventsSerializerErrorsTotal** is a count of events that could not be deserialized correctly.</span></span>
*   <span data-ttu-id="47a37-148">**InputEventsDegradedTotal** a szerializálás megszüntetése nem problémába ütközött események száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-148">**InputEventsDegradedTotal** is a count of events that had an issue other than with deserialization.</span></span>
 
### <a name="are-events-being-dropped-or-adjusted"></a><span data-ttu-id="47a37-149">Események eldobott vagy igazítva?</span><span class="sxs-lookup"><span data-stu-id="47a37-149">Are events being dropped or adjusted?</span></span>

*   <span data-ttu-id="47a37-150">**InputEventsEarlyTotal** eseményeket, amelyek egy alkalmazás időbélyeg előtt a magas vízjel száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-150">**InputEventsEarlyTotal** is the number of events that have an application timestamp before the high watermark.</span></span>
*   <span data-ttu-id="47a37-151">**InputEventsLateTotal** eseményeket, amelyek egy alkalmazás időbélyeg után a magas vízjel száma.</span><span class="sxs-lookup"><span data-stu-id="47a37-151">**InputEventsLateTotal** is the number of events that have an application timestamp after the high watermark.</span></span>
*   <span data-ttu-id="47a37-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** a szám események eldobása előtt a feladat kezdési időpont.</span><span class="sxs-lookup"><span data-stu-id="47a37-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** is the number events dropped before the job start time.</span></span>
 
### <a name="are-we-falling-behind-in-reading-data"></a><span data-ttu-id="47a37-153">Amelyek azt alá tartozó az adatok olvasása közben?</span><span class="sxs-lookup"><span data-stu-id="47a37-153">Are we falling behind in reading data?</span></span>

*   <span data-ttu-id="47a37-154">**InputEventsSourcesBackloggedTotal** jelzi, hogy hány több üzenetet kell az Event Hubs és Azure IoT Hub bemeneti adatok olvasását.</span><span class="sxs-lookup"><span data-stu-id="47a37-154">**InputEventsSourcesBackloggedTotal** tells you how many more messages need to be read for Event Hubs and Azure IoT Hub inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="47a37-155">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="47a37-155">Get help</span></span>
<span data-ttu-id="47a37-156">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="47a37-156">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47a37-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47a37-157">Next steps</span></span>
* [<span data-ttu-id="47a37-158">A Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="47a37-158">Introduction to Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="47a37-159">A Stream Analytics használatába</span><span class="sxs-lookup"><span data-stu-id="47a37-159">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="47a37-160">Stream Analytics-feladatok méretezése</span><span class="sxs-lookup"><span data-stu-id="47a37-160">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="47a37-161">Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="47a37-161">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="47a37-162">A Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="47a37-162">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
