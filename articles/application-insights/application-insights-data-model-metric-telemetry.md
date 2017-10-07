---
title: aaaAzure Application Insights Telemetria adatmodell - metrika Telemetriai |} Microsoft Docs
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
ms.openlocfilehash: 005e218a8451007458185f1e457a20cee93fa630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a><span data-ttu-id="dea3c-103">Metrika telemetriai: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="dea3c-103">Metric telemetry: Application Insights data model</span></span>

<span data-ttu-id="dea3c-104">Metrika telemetriai által támogatott két típusa van [Application Insights](app-insights-overview.md): egyszeri mérési és előre összesített mértéket.</span><span class="sxs-lookup"><span data-stu-id="dea3c-104">There are two types of metric telemetry supported by [Application Insights](app-insights-overview.md): single measurement and pre-aggregated metric.</span></span> <span data-ttu-id="dea3c-105">Egyetlen érték csak a nevét és értékét.</span><span class="sxs-lookup"><span data-stu-id="dea3c-105">Single measurement is just a name and value.</span></span> <span data-ttu-id="dea3c-106">Előre összesített metrika hello metrika minimális és maximális értékének hello az aggregációs időköznek és szórása azt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="dea3c-106">Pre-aggregated metric specifies minimum and maximum value of hello metric in hello aggregation interval and standard deviation of it.</span></span>

<span data-ttu-id="dea3c-107">Előre összesített metrika telemetriai azt feltételezi, hogy az összesítő időszak egy perc volt.</span><span class="sxs-lookup"><span data-stu-id="dea3c-107">Pre-aggregated metric telemetry assumes that aggregation period was one minute.</span></span>

<span data-ttu-id="dea3c-108">Nincsenek Application Insights által támogatott több jól ismert metrika neve.</span><span class="sxs-lookup"><span data-stu-id="dea3c-108">There are several well-known metric names supported by Application Insights.</span></span> 

<span data-ttu-id="dea3c-109">A mérőszám a rendszer és a folyamat számlálók képviselő:</span><span class="sxs-lookup"><span data-stu-id="dea3c-109">Metric representing system and process counters:</span></span>

| <span data-ttu-id="dea3c-110">**.NET neve**</span><span class="sxs-lookup"><span data-stu-id="dea3c-110">**.NET name**</span></span>             | <span data-ttu-id="dea3c-111">**Platform független neve**</span><span class="sxs-lookup"><span data-stu-id="dea3c-111">**Platform agnostic name**</span></span> | <span data-ttu-id="dea3c-112">**REST API-név**</span><span class="sxs-lookup"><span data-stu-id="dea3c-112">**REST API name**</span></span> | <span data-ttu-id="dea3c-113">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="dea3c-113">**Description**</span></span>
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | <span data-ttu-id="dea3c-114">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-114">Work in progress...</span></span> | [<span data-ttu-id="dea3c-115">processorCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="dea3c-115">processorCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | <span data-ttu-id="dea3c-116">gép Processzorainak száma</span><span class="sxs-lookup"><span data-stu-id="dea3c-116">total machine CPU</span></span>
| `\Memory\Available Bytes`                 | <span data-ttu-id="dea3c-117">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-117">Work in progress...</span></span> | [<span data-ttu-id="dea3c-118">memoryAvailableBytes</span><span class="sxs-lookup"><span data-stu-id="dea3c-118">memoryAvailableBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | <span data-ttu-id="dea3c-119">a lemezen rendelkezésre álló memória</span><span class="sxs-lookup"><span data-stu-id="dea3c-119">memory available on disk</span></span>
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | <span data-ttu-id="dea3c-120">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-120">Work in progress...</span></span> | [<span data-ttu-id="dea3c-121">processCpuPercentage</span><span class="sxs-lookup"><span data-stu-id="dea3c-121">processCpuPercentage</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | <span data-ttu-id="dea3c-122">Hello alkalmazást hello folyamat CPU</span><span class="sxs-lookup"><span data-stu-id="dea3c-122">CPU of hello process hosting hello application</span></span>
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | <span data-ttu-id="dea3c-123">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-123">Work in progress...</span></span> | [<span data-ttu-id="dea3c-124">processPrivateBytes</span><span class="sxs-lookup"><span data-stu-id="dea3c-124">processPrivateBytes</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | <span data-ttu-id="dea3c-125">hello alkalmazást hello folyamat által használt memória</span><span class="sxs-lookup"><span data-stu-id="dea3c-125">memory used by hello process hosting hello application</span></span>
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | <span data-ttu-id="dea3c-126">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-126">Work in progress...</span></span> | [<span data-ttu-id="dea3c-127">processIOBytesPerSecond</span><span class="sxs-lookup"><span data-stu-id="dea3c-127">processIOBytesPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | <span data-ttu-id="dea3c-128">i/o-műveletek folyamat hello alkalmazást futtat</span><span class="sxs-lookup"><span data-stu-id="dea3c-128">rate of I/O operations runs by process hosting hello application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | <span data-ttu-id="dea3c-129">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-129">Work in progress...</span></span> | [<span data-ttu-id="dea3c-130">requestsPerSecond</span><span class="sxs-lookup"><span data-stu-id="dea3c-130">requestsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | <span data-ttu-id="dea3c-131">alkalmazás által feldolgozott kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="dea3c-131">rate of requests processed by application</span></span> 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | <span data-ttu-id="dea3c-132">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-132">Work in progress...</span></span> | [<span data-ttu-id="dea3c-133">exceptionsPerSecond</span><span class="sxs-lookup"><span data-stu-id="dea3c-133">exceptionsPerSecond</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | <span data-ttu-id="dea3c-134">alkalmazás által kiváltott kivételekre is ki aránya</span><span class="sxs-lookup"><span data-stu-id="dea3c-134">rate of exceptions thrown by application</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | <span data-ttu-id="dea3c-135">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-135">Work in progress...</span></span> | [<span data-ttu-id="dea3c-136">requestExecutionTime</span><span class="sxs-lookup"><span data-stu-id="dea3c-136">requestExecutionTime</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | <span data-ttu-id="dea3c-137">a kérelmek átlagos végrehajtási idő</span><span class="sxs-lookup"><span data-stu-id="dea3c-137">average requests execution time</span></span>
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | <span data-ttu-id="dea3c-138">Megoldás folyamatban...</span><span class="sxs-lookup"><span data-stu-id="dea3c-138">Work in progress...</span></span> | [<span data-ttu-id="dea3c-139">requestsInQueue</span><span class="sxs-lookup"><span data-stu-id="dea3c-139">requestsInQueue</span></span>](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | <span data-ttu-id="dea3c-140">a várólista feldolgozása hello várakozó kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="dea3c-140">number of requests waiting for hello processing in a queue</span></span>

## <a name="name"></a><span data-ttu-id="dea3c-141">Név</span><span class="sxs-lookup"><span data-stu-id="dea3c-141">Name</span></span>

<span data-ttu-id="dea3c-142">Neve az Application Insights portál és a felhasználói felület toosee milyen hello metrika.</span><span class="sxs-lookup"><span data-stu-id="dea3c-142">Name of hello metric you'd like toosee in Application Insights portal and UI.</span></span> 

## <a name="value"></a><span data-ttu-id="dea3c-143">Érték</span><span class="sxs-lookup"><span data-stu-id="dea3c-143">Value</span></span>

<span data-ttu-id="dea3c-144">Mérési egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="dea3c-144">Single value for measurement.</span></span> <span data-ttu-id="dea3c-145">Egyéni mértékek hello összesítés céljából összege.</span><span class="sxs-lookup"><span data-stu-id="dea3c-145">Sum of individual measurements for hello aggregation.</span></span>

## <a name="count"></a><span data-ttu-id="dea3c-146">Darabszám</span><span class="sxs-lookup"><span data-stu-id="dea3c-146">Count</span></span>

<span data-ttu-id="dea3c-147">Metrika súlyának összesítve hello metrika.</span><span class="sxs-lookup"><span data-stu-id="dea3c-147">Metric weight of hello aggregated metric.</span></span> <span data-ttu-id="dea3c-148">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="dea3c-148">Should not be set for a measurement.</span></span>

## <a name="min"></a><span data-ttu-id="dea3c-149">Perc</span><span class="sxs-lookup"><span data-stu-id="dea3c-149">Min</span></span>

<span data-ttu-id="dea3c-150">Összesítve hello metrika minimális értéke.</span><span class="sxs-lookup"><span data-stu-id="dea3c-150">Minimum value of hello aggregated metric.</span></span> <span data-ttu-id="dea3c-151">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="dea3c-151">Should not be set for a measurement.</span></span>

## <a name="max"></a><span data-ttu-id="dea3c-152">Maximum</span><span class="sxs-lookup"><span data-stu-id="dea3c-152">Max</span></span>

<span data-ttu-id="dea3c-153">Összesítve hello metrika maximális értékét.</span><span class="sxs-lookup"><span data-stu-id="dea3c-153">Maximum value of hello aggregated metric.</span></span> <span data-ttu-id="dea3c-154">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="dea3c-154">Should not be set for a measurement.</span></span>

## <a name="standard-deviation"></a><span data-ttu-id="dea3c-155">Szórás</span><span class="sxs-lookup"><span data-stu-id="dea3c-155">Standard deviation</span></span>

<span data-ttu-id="dea3c-156">Hello szórása metrika összesíteni.</span><span class="sxs-lookup"><span data-stu-id="dea3c-156">Standard deviation of hello aggregated metric.</span></span> <span data-ttu-id="dea3c-157">Nem lehet beállítani a mérés.</span><span class="sxs-lookup"><span data-stu-id="dea3c-157">Should not be set for a measurement.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="dea3c-158">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="dea3c-158">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="dea3c-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dea3c-159">Next steps</span></span>

- <span data-ttu-id="dea3c-160">Megtudhatja, hogyan toouse [Application Insights API egyéni események és metrikák](app-insights-api-custom-events-metrics.md#trackmetric).</span><span class="sxs-lookup"><span data-stu-id="dea3c-160">Learn how toouse [Application Insights API for custom events and metrics](app-insights-api-custom-events-metrics.md#trackmetric).</span></span>
- <span data-ttu-id="dea3c-161">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="dea3c-161">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="dea3c-162">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="dea3c-162">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
