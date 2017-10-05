---
title: Azure Application Insights Telemetria-adatmodell - metrika Telemetriai |} Microsoft Docs
description: Application Insights adatmodell metrika telemetriai adat
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: bwren
ms.openlocfilehash: 42e55db4f932de85ee1a71c408b889e0ff9fe3e1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a><span data-ttu-id="0d106-103">Metrika telemetriai: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="0d106-103">Metric telemetry: Application Insights data model</span></span>

<span data-ttu-id="0d106-104">Metrika telemetriai által támogatott két típusa van [Application Insights](app-insights-overview.md): egyszeri mérési és előre összesített mértéket.</span><span class="sxs-lookup"><span data-stu-id="0d106-104">There are two types of metric telemetry supported by [Application Insights](app-insights-overview.md): single measurement and pre-aggregated metric.</span></span> <span data-ttu-id="0d106-105">Egyetlen érték csak a nevét és értékét.</span><span class="sxs-lookup"><span data-stu-id="0d106-105">Single measurement is just a name and value.</span></span> <span data-ttu-id="0d106-106">Előre összesített metrika legkisebb és legnagyobb értéke a metrika az aggregációs időköznek és szórása azt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0d106-106">Pre-aggregated metric specifies minimum and maximum value of the metric in the aggregation interval and standard deviation of it.</span></span>

<span data-ttu-id="0d106-107">Előre összesített metrika telemetriai azt feltételezi, hogy az összesítő időszak egy perc volt.</span><span class="sxs-lookup"><span data-stu-id="0d106-107">Pre-aggregated metric telemetry assumes that aggregation period was one minute.</span></span>

<span data-ttu-id="0d106-108">Nincsenek Application Insights által támogatott több jól ismert metrika neve.</span><span class="sxs-lookup"><span data-stu-id="0d106-108">There are several well-known metric names supported by Application Insights.</span></span> 

<span data-ttu-id="0d106-109">A mérőszám a rendszer és a folyamat számlálók képviselő:</span><span class="sxs-lookup"><span data-stu-id="0d106-109">Metric representing system and process counters:</span></span>

| <span data-ttu-id="0d106-110">**.NET neve**</span><span class="sxs-lookup"><span data-stu-id="0d106-110">**.NET name**</span></span>             | <span data-ttu-id="0d106-111">**Platform független neve**</span><span class="sxs-lookup"><span data-stu-id="0d106-111">**Platform agnostic name**</span></span> | <span data-ttu-id="0d106-112">**REST API-név**</span><span class="sxs-lookup"><span data-stu-id="0d106-112">**REST API name**</span></span> | <span data-ttu-id="0d106-113">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="0d106-113">**Description**</span></span>
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | <span data-ttu-id="0d106-114">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-114">Work in progress...</span></span> | [<span data-ttu-id="0d106-115">processorCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="0d106-115">processorCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | <span data-ttu-id="0d106-116">gép Processzorainak száma</span><span class="sxs-lookup"><span data-stu-id="0d106-116">total machine CPU</span></span>
| `\Memory\Available Bytes`                 | <span data-ttu-id="0d106-117">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-117">Work in progress...</span></span> | [<span data-ttu-id="0d106-118">memoryAvailableBytes</span><span class="sxs-lookup"><span data-stu-id="0d106-118">memoryAvailableBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | <span data-ttu-id="0d106-119">a lemezen rendelkezésre álló memória</span><span class="sxs-lookup"><span data-stu-id="0d106-119">memory available on disk</span></span>
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | <span data-ttu-id="0d106-120">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-120">Work in progress...</span></span> | [<span data-ttu-id="0d106-121">processCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="0d106-121">processCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | <span data-ttu-id="0d106-122">A folyamat az alkalmazást futtató Processzor</span><span class="sxs-lookup"><span data-stu-id="0d106-122">CPU of the process hosting the application</span></span>
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | <span data-ttu-id="0d106-123">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-123">Work in progress...</span></span> | [<span data-ttu-id="0d106-124">processPrivateBytes</span><span class="sxs-lookup"><span data-stu-id="0d106-124">processPrivateBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | <span data-ttu-id="0d106-125">az alkalmazást a folyamat által használt memória</span><span class="sxs-lookup"><span data-stu-id="0d106-125">memory used by the process hosting the application</span></span>
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | <span data-ttu-id="0d106-126">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-126">Work in progress...</span></span> | [<span data-ttu-id="0d106-127">processIOBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="0d106-127">processIOBytesPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | <span data-ttu-id="0d106-128">i/o-műveletek futtatja az alkalmazást futtató folyamat</span><span class="sxs-lookup"><span data-stu-id="0d106-128">rate of I/O operations runs by process hosting the application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | <span data-ttu-id="0d106-129">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-129">Work in progress...</span></span> | [<span data-ttu-id="0d106-130">requestsPerSecond</span><span class="sxs-lookup"><span data-stu-id="0d106-130">requestsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | <span data-ttu-id="0d106-131">alkalmazás által feldolgozott kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="0d106-131">rate of requests processed by application</span></span> 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | <span data-ttu-id="0d106-132">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-132">Work in progress...</span></span> | [<span data-ttu-id="0d106-133">exceptionsPerSecond</span><span class="sxs-lookup"><span data-stu-id="0d106-133">exceptionsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | <span data-ttu-id="0d106-134">alkalmazás által kiváltott kivételekre is ki aránya</span><span class="sxs-lookup"><span data-stu-id="0d106-134">rate of exceptions thrown by application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | <span data-ttu-id="0d106-135">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-135">Work in progress...</span></span> | [<span data-ttu-id="0d106-136">requestExecutionTime</span><span class="sxs-lookup"><span data-stu-id="0d106-136">requestExecutionTime</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | <span data-ttu-id="0d106-137">a kérelmek átlagos végrehajtási idő</span><span class="sxs-lookup"><span data-stu-id="0d106-137">average requests execution time</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | <span data-ttu-id="0d106-138">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="0d106-138">Work in progress...</span></span> | [<span data-ttu-id="0d106-139">requestsInQueue</span><span class="sxs-lookup"><span data-stu-id="0d106-139">requestsInQueue</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | <span data-ttu-id="0d106-140">a sorhoz feldolgozásra váró kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="0d106-140">number of requests waiting for the processing in a queue</span></span>

## <a name="name"></a><span data-ttu-id="0d106-141">Név</span><span class="sxs-lookup"><span data-stu-id="0d106-141">Name</span></span>

<span data-ttu-id="0d106-142">Az Application Insights portál és a felhasználói felületen szeretné metrika neve.</span><span class="sxs-lookup"><span data-stu-id="0d106-142">Name of the metric you'd like to see in Application Insights portal and UI.</span></span> 

## <a name="value"></a><span data-ttu-id="0d106-143">Érték</span><span class="sxs-lookup"><span data-stu-id="0d106-143">Value</span></span>

<span data-ttu-id="0d106-144">Mérési egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="0d106-144">Single value for measurement.</span></span> <span data-ttu-id="0d106-145">Az összesítés egyéni mértékek összege.</span><span class="sxs-lookup"><span data-stu-id="0d106-145">Sum of individual measurements for the aggregation.</span></span>

## <a name="count"></a><span data-ttu-id="0d106-146">Darabszám</span><span class="sxs-lookup"><span data-stu-id="0d106-146">Count</span></span>

<span data-ttu-id="0d106-147">Metrika súlyának a összesített metrika.</span><span class="sxs-lookup"><span data-stu-id="0d106-147">Metric weight of the aggregated metric.</span></span> <span data-ttu-id="0d106-148">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="0d106-148">Should not be set for a measurement.</span></span>

## <a name="min"></a><span data-ttu-id="0d106-149">Perc</span><span class="sxs-lookup"><span data-stu-id="0d106-149">Min</span></span>

<span data-ttu-id="0d106-150">Az összesített metrika minimális értéke.</span><span class="sxs-lookup"><span data-stu-id="0d106-150">Minimum value of the aggregated metric.</span></span> <span data-ttu-id="0d106-151">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="0d106-151">Should not be set for a measurement.</span></span>

## <a name="max"></a><span data-ttu-id="0d106-152">Maximum</span><span class="sxs-lookup"><span data-stu-id="0d106-152">Max</span></span>

<span data-ttu-id="0d106-153">Az összesített metrika maximális értékét.</span><span class="sxs-lookup"><span data-stu-id="0d106-153">Maximum value of the aggregated metric.</span></span> <span data-ttu-id="0d106-154">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="0d106-154">Should not be set for a measurement.</span></span>

## <a name="standard-deviation"></a><span data-ttu-id="0d106-155">Szórás</span><span class="sxs-lookup"><span data-stu-id="0d106-155">Standard deviation</span></span>

<span data-ttu-id="0d106-156">Az összesített metrika szórása.</span><span class="sxs-lookup"><span data-stu-id="0d106-156">Standard deviation of the aggregated metric.</span></span> <span data-ttu-id="0d106-157">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="0d106-157">Should not be set for a measurement.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="0d106-158">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="0d106-158">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="0d106-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d106-159">Next steps</span></span>

- <span data-ttu-id="0d106-160">Ismerje meg, hogyan használható [Application Insights API egyéni események és metrikák](app-insights-api-custom-events-metrics.md#trackmetric).</span><span class="sxs-lookup"><span data-stu-id="0d106-160">Learn how to use [Application Insights API for custom events and metrics](app-insights-api-custom-events-metrics.md#trackmetric).</span></span>
- <span data-ttu-id="0d106-161">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="0d106-161">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="0d106-162">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="0d106-162">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
