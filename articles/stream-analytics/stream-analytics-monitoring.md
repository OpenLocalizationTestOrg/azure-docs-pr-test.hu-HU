---
title: "Stream Analytics feladat figyelési aaaUnderstanding |} Microsoft Docs"
description: "Figyelés a Stream Analytics-feladat ismertetése"
keywords: "lekérdezés-figyelő"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 36819025c7b2ddbf4b9694522f1b4820407ca5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-toomonitor-queries"></a><span data-ttu-id="f64d2-104">A Stream Analytics-feladat figyelés megértése, hogyan toomonitor lekérdezések</span><span class="sxs-lookup"><span data-stu-id="f64d2-104">Understand Stream Analytics job monitoring and how toomonitor queries</span></span>

## <a name="introduction-hello-monitor-page"></a><span data-ttu-id="f64d2-105">Bemutató: hello figyelő lapja</span><span class="sxs-lookup"><span data-stu-id="f64d2-105">Introduction: hello monitor page</span></span>
<span data-ttu-id="f64d2-106">hello Azure-portálon is surface fontos teljesítménymutatókat, melyek használt toomonitor és a lekérdezés és a feladat elhárítása.</span><span class="sxs-lookup"><span data-stu-id="f64d2-106">hello Azure portal both surface key performance metrics that can be used toomonitor and troubleshoot your query and job performance.</span></span> <span data-ttu-id="f64d2-107">toosee metrikákat, keresse meg a toohello Stream Analytics-feladat érdekli metrikáit és a nézet hello **figyelés** szakasz hello áttekintése lapon.</span><span class="sxs-lookup"><span data-stu-id="f64d2-107">toosee these metrics, browse toohello Stream Analytics job you are interested in seeing metrics for and view hello **Monitoring** section on hello Overview page.</span></span>  

![Figyelési hivatkozás](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

<span data-ttu-id="f64d2-109">hello ablakban látható módon jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="f64d2-109">hello window will appear as shown:</span></span>

![Figyelési feladat irányítópult](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a><span data-ttu-id="f64d2-111">A Stream Analytics elérhető metrikák</span><span class="sxs-lookup"><span data-stu-id="f64d2-111">Metrics available for Stream Analytics</span></span>
| <span data-ttu-id="f64d2-112">Metrika</span><span class="sxs-lookup"><span data-stu-id="f64d2-112">Metric</span></span>                 | <span data-ttu-id="f64d2-113">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="f64d2-113">Definition</span></span>                               |
| ---------------------- | ---------------------------------------- |
| <span data-ttu-id="f64d2-114">SU kihasználtsága (%)</span><span class="sxs-lookup"><span data-stu-id="f64d2-114">SU % Utilization</span></span>       | <span data-ttu-id="f64d2-115">hello hello felhasználásának folyamatos átviteli egység (ek) hozzárendelt tooa feladat hello méretezési lapon hello feladat.</span><span class="sxs-lookup"><span data-stu-id="f64d2-115">hello utilization of hello Streaming Unit(s) assigned tooa job from hello Scale tab of hello job.</span></span> <span data-ttu-id="f64d2-116">Érje ezen mutató 80 %-át, vagy a fenti nincs nagy valószínűséggel, hogy az esemény feldolgozása később, vagy leállt, így a folyamat.</span><span class="sxs-lookup"><span data-stu-id="f64d2-116">Should this indicator reach 80%, or above, there is high probability that event processing may be delayed or stopped making progress.</span></span> |
| <span data-ttu-id="f64d2-117">A bemeneti események</span><span class="sxs-lookup"><span data-stu-id="f64d2-117">Input Events</span></span>           | <span data-ttu-id="f64d2-118">Hello Stream Analytics-feladat az események által fogadott adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="f64d2-118">Amount of data received by hello Stream Analytics job, in number of events.</span></span> <span data-ttu-id="f64d2-119">Ez akkor lehet, hogy események küldése toohello bemeneti forrás használt toovalidate.</span><span class="sxs-lookup"><span data-stu-id="f64d2-119">This can be used toovalidate that events are being sent toohello input source.</span></span> |
| <span data-ttu-id="f64d2-120">A kimeneti eseményekben</span><span class="sxs-lookup"><span data-stu-id="f64d2-120">Output Events</span></span>          | <span data-ttu-id="f64d2-121">Hello Stream Analytics feladat toohello kimeneti tároló, az események által küldött adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="f64d2-121">Amount of data sent by hello Stream Analytics job toohello output target, in number of events.</span></span> |
| <span data-ttu-id="f64d2-122">Soron események</span><span class="sxs-lookup"><span data-stu-id="f64d2-122">Out-of-Order Events</span></span>    | <span data-ttu-id="f64d2-123">Lettek dobva, vagy egy módosított timestamp, hello esemény rendelési házirend alapján megadott sorrendje nem fogadott események száma.</span><span class="sxs-lookup"><span data-stu-id="f64d2-123">Number of events received out of order that were either dropped or given an adjusted timestamp, based on hello Event Ordering Policy.</span></span> <span data-ttu-id="f64d2-124">Ez is negatív hatással lehet hello of soron tűrési beállítás hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f64d2-124">This can be impacted by hello configuration of hello Out of Order Tolerance Window setting.</span></span> |
| <span data-ttu-id="f64d2-125">Adatok konvertálási hibák</span><span class="sxs-lookup"><span data-stu-id="f64d2-125">Data Conversion Errors</span></span> | <span data-ttu-id="f64d2-126">A Stream Analytics-feladat felmerült adatok átalakítás hibák száma.</span><span class="sxs-lookup"><span data-stu-id="f64d2-126">Number of data conversion errors incurred by a Stream Analytics job.</span></span> |
| <span data-ttu-id="f64d2-127">Futásidejű hibák</span><span class="sxs-lookup"><span data-stu-id="f64d2-127">Runtime Errors</span></span>         | <span data-ttu-id="f64d2-128">hello hibák összesített számát, amely a Stream Analytics-feladat végrehajtása során kerül sor.</span><span class="sxs-lookup"><span data-stu-id="f64d2-128">hello total number of errors that happen during execution of a Stream Analytics job.</span></span> |
| <span data-ttu-id="f64d2-129">A későn érkező bemeneti események</span><span class="sxs-lookup"><span data-stu-id="f64d2-129">Late Input Events</span></span>      | <span data-ttu-id="f64d2-130">Hello forrás, amely vagy el lett dobva vagy időbélyegzőik későn érkező események száma be van állítva, a hello esemény rendelési házirend konfigurációs hello késő érkezés tűrési beállítás alapján.</span><span class="sxs-lookup"><span data-stu-id="f64d2-130">Number of events arriving late from hello source which have either been dropped or their timestamp has been adjusted, based on hello Event Ordering Policy configuration of hello Late Arrival Tolerance Window setting.</span></span> |
| <span data-ttu-id="f64d2-131">Függvény kérelmek</span><span class="sxs-lookup"><span data-stu-id="f64d2-131">Function Requests</span></span>      | <span data-ttu-id="f64d2-132">Hívások toohello Azure Machine Learning-függvény (ha van ilyen) száma.</span><span class="sxs-lookup"><span data-stu-id="f64d2-132">Number of calls toohello Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="f64d2-133">Sikertelen függvény kérelmek</span><span class="sxs-lookup"><span data-stu-id="f64d2-133">Failed Function Requests</span></span> | <span data-ttu-id="f64d2-134">Sikertelen Azure Machine Learning függvényhívások (ha van ilyen) száma.</span><span class="sxs-lookup"><span data-stu-id="f64d2-134">Number of failed Azure Machine Learning function calls (if present).</span></span> |
| <span data-ttu-id="f64d2-135">Függvény események</span><span class="sxs-lookup"><span data-stu-id="f64d2-135">Function Events</span></span>        | <span data-ttu-id="f64d2-136">Elküldött toohello Azure Machine Learning-függvény események száma (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="f64d2-136">Number of events sent toohello Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="f64d2-137">A bemeneti esemény bájt</span><span class="sxs-lookup"><span data-stu-id="f64d2-137">Input Event Bytes</span></span>      | <span data-ttu-id="f64d2-138">Hello Stream Analytics-feladat bájtban által fogadott adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="f64d2-138">Amount of data received by hello Stream Analytics job, in bytes.</span></span> <span data-ttu-id="f64d2-139">Ez akkor lehet, hogy események küldése toohello bemeneti forrás használt toovalidate.</span><span class="sxs-lookup"><span data-stu-id="f64d2-139">This can be used toovalidate that events are being sent toohello input source.</span></span> |


## <a name="customizing-monitoring-in-hello-azure-portal"></a><span data-ttu-id="f64d2-140">Figyelés a hello Azure-portál testreszabása</span><span class="sxs-lookup"><span data-stu-id="f64d2-140">Customizing Monitoring in hello Azure portal</span></span>
<span data-ttu-id="f64d2-141">Módosítsa a hello típusú diagramra, a feltüntetett metrikákat, és időtartománynak hello diagram szerkesztése lehetőséget beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="f64d2-141">You can adjust hello type of chart, metrics shown, and time range in hello Edit Chart settings.</span></span> <span data-ttu-id="f64d2-142">További információkért lásd: [hogyan tooCustomize figyelés](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="f64d2-142">For details, see [How tooCustomize Monitoring](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

  ![Lekérdezési idő figyelése diagramhoz](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a><span data-ttu-id="f64d2-144">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="f64d2-144">Get help</span></span>
<span data-ttu-id="f64d2-145">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="f64d2-145">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f64d2-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f64d2-146">Next steps</span></span>
* [<span data-ttu-id="f64d2-147">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="f64d2-147">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="f64d2-148">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="f64d2-148">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="f64d2-149">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="f64d2-149">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="f64d2-150">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="f64d2-150">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="f64d2-151">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="f64d2-151">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

