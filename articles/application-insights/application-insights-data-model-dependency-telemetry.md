---
title: "aaaAzure Application Insights Telemetria adatmodell - függőségi Telemetria |} Microsoft Docs"
description: "Application Insights – függőségi telemetria tartozó adatmodell"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: bwren
ms.openlocfilehash: cd5ab7c61d3498e4aa2a0aa0c8b0d106a92912e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a><span data-ttu-id="4d3a1-103">– Függőségi telemetria: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="4d3a1-103">Dependency telemetry: Application Insights data model</span></span>

<span data-ttu-id="4d3a1-104">– Függőségi Telemetria (a [Application Insights](app-insights-overview.md)) hello figyelt összetevők egy távoli összetevő, például az SQL- vagy HTTP-végponttal való jelöli.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-104">Dependency Telemetry (in [Application Insights](app-insights-overview.md)) represents an interaction of hello monitored component with a remote component such as SQL or an HTTP endpoint.</span></span>

## <a name="name"></a><span data-ttu-id="4d3a1-105">Név</span><span class="sxs-lookup"><span data-stu-id="4d3a1-105">Name</span></span>

<span data-ttu-id="4d3a1-106">Ez a függőségi hívás értékkel kezdeményezték hello parancs nevét.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-106">Name of hello command initiated with this dependency call.</span></span> <span data-ttu-id="4d3a1-107">Alacsony cardinality értéke.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-107">Low cardinality value.</span></span> <span data-ttu-id="4d3a1-108">Többek között a tárolt eljárás nevét és URL-cím elérési út sablont.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-108">Examples are stored procedure name and URL path template.</span></span>

## <a name="id"></a><span data-ttu-id="4d3a1-109">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="4d3a1-109">ID</span></span>

<span data-ttu-id="4d3a1-110">A függőségi hívás példány azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-110">Identifier of a dependency call instance.</span></span> <span data-ttu-id="4d3a1-111">Hello kérelem telemetriai elemével toothis függőségi hívás megfelelő használható korrelációhoz.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-111">Used for correlation with hello request telemetry item corresponding toothis dependency call.</span></span> <span data-ttu-id="4d3a1-112">További információkért lásd: [korrelációs](application-insights-correlation.md) lap.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-112">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="data"></a><span data-ttu-id="4d3a1-113">Adatok</span><span class="sxs-lookup"><span data-stu-id="4d3a1-113">Data</span></span>

<span data-ttu-id="4d3a1-114">Ez a függőségi hívás által kezdeményezett parancsot.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-114">Command initiated by this dependency call.</span></span> <span data-ttu-id="4d3a1-115">Többek között az SQL-utasítást, és HTTP URL-cím és az összes lekérdezési paramétert.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-115">Examples are SQL statement and HTTP URL with all query parameters.</span></span>

## <a name="type"></a><span data-ttu-id="4d3a1-116">Típus</span><span class="sxs-lookup"><span data-stu-id="4d3a1-116">Type</span></span>

<span data-ttu-id="4d3a1-117">A függőségi típusnév.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-117">Dependency type name.</span></span> <span data-ttu-id="4d3a1-118">A függőségek logikai jellegű csoportosítását és más mezők, például a commandName és resultCode értelmezése alacsony cardinality értéke.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-118">Low cardinality value for logical grouping of dependencies and interpretation of other fields like commandName and resultCode.</span></span> <span data-ttu-id="4d3a1-119">Többek között az SQL Azure-tábla és a HTTP.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-119">Examples are SQL, Azure table, and HTTP.</span></span>

## <a name="target"></a><span data-ttu-id="4d3a1-120">cél</span><span class="sxs-lookup"><span data-stu-id="4d3a1-120">Target</span></span>

<span data-ttu-id="4d3a1-121">A függőségi hívás célhelyre.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-121">Target site of a dependency call.</span></span> <span data-ttu-id="4d3a1-122">Például a kiszolgáló neve, a gazdagép címe.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-122">Examples are server name, host address.</span></span> <span data-ttu-id="4d3a1-123">További információkért lásd: [korrelációs](application-insights-correlation.md) lap.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-123">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="duration"></a><span data-ttu-id="4d3a1-124">Időtartam</span><span class="sxs-lookup"><span data-stu-id="4d3a1-124">Duration</span></span>

<span data-ttu-id="4d3a1-125">Időtartam formátumú kérelem: `DD.HH:MM:SS.MMMMMM`.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-125">Request duration in format: `DD.HH:MM:SS.MMMMMM`.</span></span> <span data-ttu-id="4d3a1-126">Lehet kisebb, mint `1000` nap.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-126">Must be less than `1000` days.</span></span>

## <a name="result-code"></a><span data-ttu-id="4d3a1-127">Eredmény kódja</span><span class="sxs-lookup"><span data-stu-id="4d3a1-127">Result code</span></span>

<span data-ttu-id="4d3a1-128">A függőségi hívás eredménykódja.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-128">Result code of a dependency call.</span></span> <span data-ttu-id="4d3a1-129">Többek között az SQL-hibakód és a HTTP-állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-129">Examples are SQL error code and HTTP status code.</span></span>

## <a name="success"></a><span data-ttu-id="4d3a1-130">Sikeres</span><span class="sxs-lookup"><span data-stu-id="4d3a1-130">Success</span></span>

<span data-ttu-id="4d3a1-131">Sikeres vagy sikertelen hívás megjelölése.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-131">Indication of successful or unsuccessful call.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="4d3a1-132">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="4d3a1-132">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="4d3a1-133">Egyéni mértékek</span><span class="sxs-lookup"><span data-stu-id="4d3a1-133">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a><span data-ttu-id="4d3a1-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d3a1-134">Next steps</span></span>

- <span data-ttu-id="4d3a1-135">Állítsa be a követési függőségi [.NET](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="4d3a1-135">Set up dependency tracking for [.NET](app-insights-asp-net-dependencies.md).</span></span>
- <span data-ttu-id="4d3a1-136">Állítsa be a követési függőségi [Java](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="4d3a1-136">Set up dependency tracking for [Java](app-insights-java-agent.md).</span></span>
- [<span data-ttu-id="4d3a1-137">Egyéni függőségi telemetria írása</span><span class="sxs-lookup"><span data-stu-id="4d3a1-137">Write custom dependency telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackdependency)
- <span data-ttu-id="4d3a1-138">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-138">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="4d3a1-139">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="4d3a1-139">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
