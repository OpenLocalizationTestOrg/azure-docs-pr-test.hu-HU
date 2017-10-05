---
title: "Understanding Stream Analytics-feladat figyelése |} Microsoft Docs"
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
ms.openlocfilehash: 13d96807a5591ec88deda19ea73cfedc07078433
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a><span data-ttu-id="5666b-104">A Stream Analytics-feladat megfigyelés és a lekérdezések figyelése</span><span class="sxs-lookup"><span data-stu-id="5666b-104">Understand Stream Analytics job monitoring and how to monitor queries</span></span>

## <a name="introduction-the-monitor-page"></a><span data-ttu-id="5666b-105">Bemutató: A figyelő lapja</span><span class="sxs-lookup"><span data-stu-id="5666b-105">Introduction: The monitor page</span></span>
<span data-ttu-id="5666b-106">Az Azure portál mindkét surface fontos teljesítménymutatókat, és a lekérdezés és a feladat teljesítmény hibaelhárítása használható.</span><span class="sxs-lookup"><span data-stu-id="5666b-106">The Azure portal both surface key performance metrics that can be used to monitor and troubleshoot your query and job performance.</span></span> <span data-ttu-id="5666b-107">A metrikák megtekintéséhez keresse meg a Stream Analytics-feladat metrikáját szükség, és megtekintheti a **figyelés** szakaszban Áttekintés lap.</span><span class="sxs-lookup"><span data-stu-id="5666b-107">To see these metrics, browse to the Stream Analytics job you are interested in seeing metrics for and view the **Monitoring** section on the Overview page.</span></span>  

![Figyelési hivatkozás](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

<span data-ttu-id="5666b-109">Az ablak módon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="5666b-109">The window will appear as shown:</span></span>

![Figyelési feladat irányítópult](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a><span data-ttu-id="5666b-111">A Stream Analytics elérhető metrikák</span><span class="sxs-lookup"><span data-stu-id="5666b-111">Metrics available for Stream Analytics</span></span>
| <span data-ttu-id="5666b-112">Metrika</span><span class="sxs-lookup"><span data-stu-id="5666b-112">Metric</span></span>                 | <span data-ttu-id="5666b-113">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="5666b-113">Definition</span></span>                               |
| ---------------------- | ---------------------------------------- |
| <span data-ttu-id="5666b-114">SU kihasználtsága (%)</span><span class="sxs-lookup"><span data-stu-id="5666b-114">SU % Utilization</span></span>       | <span data-ttu-id="5666b-115">A folyamatos átviteli egység (ek) használata az egy feladathoz hozzárendelt a skála lapot a feladat.</span><span class="sxs-lookup"><span data-stu-id="5666b-115">The utilization of the Streaming Unit(s) assigned to a job from the Scale tab of the job.</span></span> <span data-ttu-id="5666b-116">Érje ezen mutató 80 %-át, vagy a fenti nincs nagy valószínűséggel, hogy az esemény feldolgozása később, vagy leállt, így a folyamat.</span><span class="sxs-lookup"><span data-stu-id="5666b-116">Should this indicator reach 80%, or above, there is high probability that event processing may be delayed or stopped making progress.</span></span> |
| <span data-ttu-id="5666b-117">A bemeneti események</span><span class="sxs-lookup"><span data-stu-id="5666b-117">Input Events</span></span>           | <span data-ttu-id="5666b-118">A Stream Analytics-feladat, az események által fogadott adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="5666b-118">Amount of data received by the Stream Analytics job, in number of events.</span></span> <span data-ttu-id="5666b-119">Ennek segítségével ellenőrizze, hogy a bemeneti forrás küldött események.</span><span class="sxs-lookup"><span data-stu-id="5666b-119">This can be used to validate that events are being sent to the input source.</span></span> |
| <span data-ttu-id="5666b-120">A kimeneti eseményekben</span><span class="sxs-lookup"><span data-stu-id="5666b-120">Output Events</span></span>          | <span data-ttu-id="5666b-121">A kimeneti cél események száma a Stream Analytics-feladat által küldött adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="5666b-121">Amount of data sent by the Stream Analytics job to the output target, in number of events.</span></span> |
| <span data-ttu-id="5666b-122">Soron események</span><span class="sxs-lookup"><span data-stu-id="5666b-122">Out-of-Order Events</span></span>    | <span data-ttu-id="5666b-123">Lettek dobva, vagy egy módosított Timestamp értéket, az esemény rendelési házirend alapján megadott sorrendje nem fogadott események száma.</span><span class="sxs-lookup"><span data-stu-id="5666b-123">Number of events received out of order that were either dropped or given an adjusted timestamp, based on the Event Ordering Policy.</span></span> <span data-ttu-id="5666b-124">Ez is negatív hatással lehet a soron of tűrési beállítás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="5666b-124">This can be impacted by the configuration of the Out of Order Tolerance Window setting.</span></span> |
| <span data-ttu-id="5666b-125">Adatok konvertálási hibák</span><span class="sxs-lookup"><span data-stu-id="5666b-125">Data Conversion Errors</span></span> | <span data-ttu-id="5666b-126">A Stream Analytics-feladat felmerült adatok átalakítás hibák száma.</span><span class="sxs-lookup"><span data-stu-id="5666b-126">Number of data conversion errors incurred by a Stream Analytics job.</span></span> |
| <span data-ttu-id="5666b-127">Futásidejű hibák</span><span class="sxs-lookup"><span data-stu-id="5666b-127">Runtime Errors</span></span>         | <span data-ttu-id="5666b-128">A Stream Analytics-feladat végrehajtása során előforduló hibák teljes száma.</span><span class="sxs-lookup"><span data-stu-id="5666b-128">The total number of errors that happen during execution of a Stream Analytics job.</span></span> |
| <span data-ttu-id="5666b-129">A későn érkező bemeneti események</span><span class="sxs-lookup"><span data-stu-id="5666b-129">Late Input Events</span></span>      | <span data-ttu-id="5666b-130">A forrás, amely vagy el lett dobva vagy időbélyegzőik későn érkező események száma be van állítva, az esemény rendelési házirend konfiguráció késő érkezés tűrési beállítás alapján.</span><span class="sxs-lookup"><span data-stu-id="5666b-130">Number of events arriving late from the source which have either been dropped or their timestamp has been adjusted, based on the Event Ordering Policy configuration of the Late Arrival Tolerance Window setting.</span></span> |
| <span data-ttu-id="5666b-131">Függvény kérelmek</span><span class="sxs-lookup"><span data-stu-id="5666b-131">Function Requests</span></span>      | <span data-ttu-id="5666b-132">Az Azure Machine Learning-függvény (ha van ilyen) hívások száma.</span><span class="sxs-lookup"><span data-stu-id="5666b-132">Number of calls to the Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="5666b-133">Sikertelen függvény kérelmek</span><span class="sxs-lookup"><span data-stu-id="5666b-133">Failed Function Requests</span></span> | <span data-ttu-id="5666b-134">Sikertelen Azure Machine Learning függvényhívások (ha van ilyen) száma.</span><span class="sxs-lookup"><span data-stu-id="5666b-134">Number of failed Azure Machine Learning function calls (if present).</span></span> |
| <span data-ttu-id="5666b-135">Függvény események</span><span class="sxs-lookup"><span data-stu-id="5666b-135">Function Events</span></span>        | <span data-ttu-id="5666b-136">(Ha van ilyen) az Azure Machine Learning-függvény küldött események száma.</span><span class="sxs-lookup"><span data-stu-id="5666b-136">Number of events sent to the Azure Machine Learning function (if present).</span></span> |
| <span data-ttu-id="5666b-137">A bemeneti esemény bájt</span><span class="sxs-lookup"><span data-stu-id="5666b-137">Input Event Bytes</span></span>      | <span data-ttu-id="5666b-138">A Stream Analytics-feladat, bájtban által fogadott adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="5666b-138">Amount of data received by the Stream Analytics job, in bytes.</span></span> <span data-ttu-id="5666b-139">Ennek segítségével ellenőrizze, hogy a bemeneti forrás küldött események.</span><span class="sxs-lookup"><span data-stu-id="5666b-139">This can be used to validate that events are being sent to the input source.</span></span> |


## <a name="customizing-monitoring-in-the-azure-portal"></a><span data-ttu-id="5666b-140">Figyelés testreszabása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5666b-140">Customizing Monitoring in the Azure portal</span></span>
<span data-ttu-id="5666b-141">Módosíthatja a típusú diagramra, a feltüntetett, metrikákat és időtartománynak a diagram szerkesztése lehetőséget beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="5666b-141">You can adjust the type of chart, metrics shown, and time range in the Edit Chart settings.</span></span> <span data-ttu-id="5666b-142">További információkért lásd: [hogyan testreszabása figyelési](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="5666b-142">For details, see [How to Customize Monitoring](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

  ![Lekérdezési idő figyelése diagramhoz](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a><span data-ttu-id="5666b-144">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="5666b-144">Get help</span></span>
<span data-ttu-id="5666b-145">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="5666b-145">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5666b-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5666b-146">Next steps</span></span>
* [<span data-ttu-id="5666b-147">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="5666b-147">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="5666b-148">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="5666b-148">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="5666b-149">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="5666b-149">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="5666b-150">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="5666b-150">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="5666b-151">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="5666b-151">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

