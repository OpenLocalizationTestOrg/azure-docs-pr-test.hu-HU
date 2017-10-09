---
title: "aaa Azure Stream Analytics adatvezérelt hibakeresést keresztül hello feladat diagram |} Microsoft Docs"
description: "A Stream Analytics-feladat hibaelhárítása hello feladat ábra és metrikák használatával."
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
ms.openlocfilehash: 1af884d485bebb06b034da01a13f7f8240516571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-driven-debugging-by-using-hello-job-diagram"></a><span data-ttu-id="275eb-103">Adatalapú hibakeresést keresztül hello feladat diagramja</span><span class="sxs-lookup"><span data-stu-id="275eb-103">Data-driven debugging by using hello job diagram</span></span>

<span data-ttu-id="275eb-104">hello feladat ábra: hello **figyelés** panel az Azure-portálon hello segítségével jelenítheti meg a feladat folyamat.</span><span class="sxs-lookup"><span data-stu-id="275eb-104">hello job diagram on hello **Monitoring** blade in hello Azure portal can help you visualize your job pipeline.</span></span> <span data-ttu-id="275eb-105">Azt illusztrálja, bemenetek, kimenetek és lekérdezések lépéseit.</span><span class="sxs-lookup"><span data-stu-id="275eb-105">It shows inputs, outputs, and query steps.</span></span> <span data-ttu-id="275eb-106">Egyes lépéseihez szükséges hello feladat ábra tooexamine hello metrikák is használhat, toomore gyorsan különítse el a probléma forrásának hello összefüggő problémák megoldásakor.</span><span class="sxs-lookup"><span data-stu-id="275eb-106">You can use hello job diagram tooexamine hello metrics for each step, toomore quickly isolate hello source of a problem when you troubleshoot issues.</span></span>

## <a name="using-hello-job-diagram"></a><span data-ttu-id="275eb-107">Hello feladat ábra használatával</span><span class="sxs-lookup"><span data-stu-id="275eb-107">Using hello job diagram</span></span>

<span data-ttu-id="275eb-108">Az Azure-portálon hello alatt a Stream Analytics-feladatot, miközben **támogatási + hibaelhárítás**, jelölje be **feladat ábra**:</span><span class="sxs-lookup"><span data-stu-id="275eb-108">In hello Azure portal, while in a Stream Analytics job, under **SUPPORT + TROUBLESHOOTING**, select **Job diagram**:</span></span>

![A metrika - hely feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

<span data-ttu-id="275eb-110">A lekérdezés-szerkesztő ablakban jelölje ki minden egyes lekérdezés lépés toosee hello megfelelő szakaszához.</span><span class="sxs-lookup"><span data-stu-id="275eb-110">Select each query step toosee hello corresponding section in a query editing pane.</span></span> <span data-ttu-id="275eb-111">Hello lépés metrika diagramot az alsó ablaktáblában hello lapon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="275eb-111">A metric chart for hello step is displayed in a lower pane on hello page.</span></span>

![A metrika - alapvető feladat feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

<span data-ttu-id="275eb-113">Válassza ki a toosee hello partíciók hello Azure Event Hubs bemeneti, **...**</span><span class="sxs-lookup"><span data-stu-id="275eb-113">toosee hello partitions of hello Azure Event Hubs input, select **. . .**</span></span> <span data-ttu-id="275eb-114">A helyi menü megjelenik.</span><span class="sxs-lookup"><span data-stu-id="275eb-114">A context menu appears.</span></span> <span data-ttu-id="275eb-115">Hello bemeneti egyesülés is látható.</span><span class="sxs-lookup"><span data-stu-id="275eb-115">You also can see hello input merger.</span></span>

![A metrika - feladat ábra bontsa ki a partíció](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

<span data-ttu-id="275eb-117">toosee hello metrika diagramot csak egy olyan partíciót, jelölje be hello partíció csomópont.</span><span class="sxs-lookup"><span data-stu-id="275eb-117">toosee hello metric chart for only a single partition, select hello partition node.</span></span> <span data-ttu-id="275eb-118">hello metrikák hello lap hello alján jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="275eb-118">hello metrics are shown at hello bottom of hello page.</span></span>

![A metrika - további metrikák feladat diagramja](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

<span data-ttu-id="275eb-120">toosee hello metrikák diagramot egyesülés, jelölje be hello egyesülés csomópont.</span><span class="sxs-lookup"><span data-stu-id="275eb-120">toosee hello metrics chart for a merger, select hello merger node.</span></span> <span data-ttu-id="275eb-121">a következő diagram hello látható, hogy egyetlen esemény sem volt eldobni módosul.</span><span class="sxs-lookup"><span data-stu-id="275eb-121">hello following chart shows that no events were dropped or adjusted.</span></span>

![A metrika - feladat ábra rács](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

<span data-ttu-id="275eb-123">toosee hello részletei hello metrika értékét, és az idő, pont toohello diagram.</span><span class="sxs-lookup"><span data-stu-id="275eb-123">toosee hello details of hello metric value and time, point toohello chart.</span></span>

![Diagram metrikákat a sikertelen feladat - vigye](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a><span data-ttu-id="275eb-125">Hibaelhárítása mérőszámok segítségével</span><span class="sxs-lookup"><span data-stu-id="275eb-125">Troubleshoot by using metrics</span></span>

<span data-ttu-id="275eb-126">Hello **QueryLastProcessedTime** metrika azt jelzi, ha egy adott lépésre adatokat kapott.</span><span class="sxs-lookup"><span data-stu-id="275eb-126">hello **QueryLastProcessedTime** metric indicates when a specific step received data.</span></span> <span data-ttu-id="275eb-127">Hello topológia megtekintésével is dolgozhat visszafelé hello kimeneti processzor toosee mely lépés nem kapja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="275eb-127">By looking at hello topology, you can work backward from hello output processor toosee which step is not receiving data.</span></span> <span data-ttu-id="275eb-128">Ha a lépés nem sikerül adatokat, nyissa meg azt megelőzően toohello lekérdezés lépést.</span><span class="sxs-lookup"><span data-stu-id="275eb-128">If a step is not getting data, go toohello query step just before it.</span></span> <span data-ttu-id="275eb-129">Ellenőrizze, hogy hello előző lekérdezés lépést tartalmaz egy olyan időkeretet, és hogy elég idő telt meg toooutput adatokat.</span><span class="sxs-lookup"><span data-stu-id="275eb-129">Check whether hello preceding query step has a time window, and if enough time has passed for it toooutput data.</span></span> <span data-ttu-id="275eb-130">(Megjegyzés: Ez idő windows illesztett toohello óra.)</span><span class="sxs-lookup"><span data-stu-id="275eb-130">(Note that time windows are snapped toohello hour.)</span></span>
 
<span data-ttu-id="275eb-131">Ha hello előző lekérdezés lépés egy bemeneti processzor, akkor hello bemeneti metrikák toohelp válasz hello célzott kérdések a következő.</span><span class="sxs-lookup"><span data-stu-id="275eb-131">If hello preceding query step is an input processor, use hello input metrics toohelp answer hello following targeted questions.</span></span> <span data-ttu-id="275eb-132">Segít meghatározni, hogy egy feladat van adat lekérésekor a bemeneti forrásból.</span><span class="sxs-lookup"><span data-stu-id="275eb-132">They can help you determine whether a job is getting data from its input sources.</span></span> <span data-ttu-id="275eb-133">Ha hello lekérdezés particionálása, vizsgálja meg az egyes partíciók.</span><span class="sxs-lookup"><span data-stu-id="275eb-133">If hello query is partitioned, examine each partition.</span></span>
 
### <a name="how-much-data-is-being-read"></a><span data-ttu-id="275eb-134">Mennyi adatot olvasása?</span><span class="sxs-lookup"><span data-stu-id="275eb-134">How much data is being read?</span></span>

*   <span data-ttu-id="275eb-135">**InputEventsSourcesTotal** hello olvasható adatok egységek száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-135">**InputEventsSourcesTotal** is hello number of data units read.</span></span> <span data-ttu-id="275eb-136">Például hello számú blobot.</span><span class="sxs-lookup"><span data-stu-id="275eb-136">For example, hello number of blobs.</span></span>
*   <span data-ttu-id="275eb-137">**InputEventsTotal** események olvasása hello száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-137">**InputEventsTotal** is hello number of events read.</span></span> <span data-ttu-id="275eb-138">Ez a metrika partíciónként érhető el.</span><span class="sxs-lookup"><span data-stu-id="275eb-138">This metric is available per partition.</span></span>
*   <span data-ttu-id="275eb-139">**InputEventsInBytesTotal** hello olvasott bájtok száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-139">**InputEventsInBytesTotal** is hello number of bytes read.</span></span>
*   <span data-ttu-id="275eb-140">**InputEventsLastArrivalTime** minden fogadott események a várólistában levő időpontja frissül.</span><span class="sxs-lookup"><span data-stu-id="275eb-140">**InputEventsLastArrivalTime** is updated with every received event's enqueued time.</span></span>
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a><span data-ttu-id="275eb-141">Az idő soron?</span><span class="sxs-lookup"><span data-stu-id="275eb-141">Is time moving forward?</span></span> <span data-ttu-id="275eb-142">Ha tényleges események olvasható, előfordulhat, hogy nem adható ki absztrakt.</span><span class="sxs-lookup"><span data-stu-id="275eb-142">If actual events are read, punctuation might not be issued.</span></span>

*   <span data-ttu-id="275eb-143">**InputEventsLastPunctuationTime** azt jelzi, ha egy absztrakt bocsátotta tookeep idő áthelyezése előre.</span><span class="sxs-lookup"><span data-stu-id="275eb-143">**InputEventsLastPunctuationTime** indicates when a punctuation was issued tookeep time moving forward.</span></span> <span data-ttu-id="275eb-144">Ha nem jelenik meg absztrakt, adatfolyama is blokkolnánk a hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="275eb-144">If punctuation is not issued, data flow can get blocked.</span></span>
 
### <a name="are-there-any-errors-in-hello-input"></a><span data-ttu-id="275eb-145">Vannak-e hibák hello bemeneti?</span><span class="sxs-lookup"><span data-stu-id="275eb-145">Are there any errors in hello input?</span></span>

*   <span data-ttu-id="275eb-146">**InputEventsEventDataNullTotal** null adattal rendelkező események száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-146">**InputEventsEventDataNullTotal** is a count of events that have null data.</span></span>
*   <span data-ttu-id="275eb-147">**InputEventsSerializerErrorsTotal** nem deszerializálhatók megfelelően események száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-147">**InputEventsSerializerErrorsTotal** is a count of events that could not be deserialized correctly.</span></span>
*   <span data-ttu-id="275eb-148">**InputEventsDegradedTotal** a szerializálás megszüntetése nem problémába ütközött események száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-148">**InputEventsDegradedTotal** is a count of events that had an issue other than with deserialization.</span></span>
 
### <a name="are-events-being-dropped-or-adjusted"></a><span data-ttu-id="275eb-149">Események eldobott vagy igazítva?</span><span class="sxs-lookup"><span data-stu-id="275eb-149">Are events being dropped or adjusted?</span></span>

*   <span data-ttu-id="275eb-150">**InputEventsEarlyTotal** hello eseményeket, amelyek egy alkalmazás időbélyeg előtt hello magas vízjel száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-150">**InputEventsEarlyTotal** is hello number of events that have an application timestamp before hello high watermark.</span></span>
*   <span data-ttu-id="275eb-151">**InputEventsLateTotal** hello eseményeket, amelyek egy alkalmazás időbélyeg után hello magas vízjel száma.</span><span class="sxs-lookup"><span data-stu-id="275eb-151">**InputEventsLateTotal** is hello number of events that have an application timestamp after hello high watermark.</span></span>
*   <span data-ttu-id="275eb-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** hello számú események eldobott hello feladat kezdési időpont előtt.</span><span class="sxs-lookup"><span data-stu-id="275eb-152">**InputEventsDroppedBeforeApplicationStartTimeTotal** is hello number events dropped before hello job start time.</span></span>
 
### <a name="are-we-falling-behind-in-reading-data"></a><span data-ttu-id="275eb-153">Amelyek azt alá tartozó az adatok olvasása közben?</span><span class="sxs-lookup"><span data-stu-id="275eb-153">Are we falling behind in reading data?</span></span>

*   <span data-ttu-id="275eb-154">**InputEventsSourcesBackloggedTotal** jelzi, hogy hány több üzenetet kell toobe Event Hubs és Azure IoT Hub bemeneti című témakörben találhat.</span><span class="sxs-lookup"><span data-stu-id="275eb-154">**InputEventsSourcesBackloggedTotal** tells you how many more messages need toobe read for Event Hubs and Azure IoT Hub inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="275eb-155">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="275eb-155">Get help</span></span>
<span data-ttu-id="275eb-156">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="275eb-156">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="275eb-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="275eb-157">Next steps</span></span>
* [<span data-ttu-id="275eb-158">Bevezetés tooStream elemzés</span><span class="sxs-lookup"><span data-stu-id="275eb-158">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="275eb-159">A Stream Analytics használatába</span><span class="sxs-lookup"><span data-stu-id="275eb-159">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="275eb-160">Stream Analytics-feladatok méretezése</span><span class="sxs-lookup"><span data-stu-id="275eb-160">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="275eb-161">Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="275eb-161">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="275eb-162">A Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="275eb-162">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
