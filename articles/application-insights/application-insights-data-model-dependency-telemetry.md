---
title: "Azure Application Insights Telemetria-adatmodell - függőségi Telemetria |} Microsoft Docs"
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
ms.openlocfilehash: 2e97c3f951f46c32802aea543b93d5ab1bb76228
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a><span data-ttu-id="45b52-103">– Függőségi telemetria: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="45b52-103">Dependency telemetry: Application Insights data model</span></span>

<span data-ttu-id="45b52-104">– Függőségi Telemetria (a [Application Insights](app-insights-overview.md)) a figyelt összetevő egy távoli összetevő, például az SQL- vagy HTTP-végponttal való jelöli.</span><span class="sxs-lookup"><span data-stu-id="45b52-104">Dependency Telemetry (in [Application Insights](app-insights-overview.md)) represents an interaction of the monitored component with a remote component such as SQL or an HTTP endpoint.</span></span>

## <a name="name"></a><span data-ttu-id="45b52-105">Név</span><span class="sxs-lookup"><span data-stu-id="45b52-105">Name</span></span>

<span data-ttu-id="45b52-106">Ez a függőségi hívás értékkel kezdeményezték a parancs nevét.</span><span class="sxs-lookup"><span data-stu-id="45b52-106">Name of the command initiated with this dependency call.</span></span> <span data-ttu-id="45b52-107">Alacsony cardinality értéke.</span><span class="sxs-lookup"><span data-stu-id="45b52-107">Low cardinality value.</span></span> <span data-ttu-id="45b52-108">Többek között a tárolt eljárás nevét és URL-cím elérési út sablont.</span><span class="sxs-lookup"><span data-stu-id="45b52-108">Examples are stored procedure name and URL path template.</span></span>

## <a name="id"></a><span data-ttu-id="45b52-109">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="45b52-109">ID</span></span>

<span data-ttu-id="45b52-110">A függőségi hívás példány azonosítója.</span><span class="sxs-lookup"><span data-stu-id="45b52-110">Identifier of a dependency call instance.</span></span> <span data-ttu-id="45b52-111">Ez a függőségi hívás megfelelő kérelem telemetriai elemmel használható korrelációhoz.</span><span class="sxs-lookup"><span data-stu-id="45b52-111">Used for correlation with the request telemetry item corresponding to this dependency call.</span></span> <span data-ttu-id="45b52-112">További információkért lásd: [korrelációs](application-insights-correlation.md) lap.</span><span class="sxs-lookup"><span data-stu-id="45b52-112">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="data"></a><span data-ttu-id="45b52-113">Adatok</span><span class="sxs-lookup"><span data-stu-id="45b52-113">Data</span></span>

<span data-ttu-id="45b52-114">Ez a függőségi hívás által kezdeményezett parancsot.</span><span class="sxs-lookup"><span data-stu-id="45b52-114">Command initiated by this dependency call.</span></span> <span data-ttu-id="45b52-115">Többek között az SQL-utasítást, és HTTP URL-cím és az összes lekérdezési paramétert.</span><span class="sxs-lookup"><span data-stu-id="45b52-115">Examples are SQL statement and HTTP URL with all query parameters.</span></span>

## <a name="type"></a><span data-ttu-id="45b52-116">Típus</span><span class="sxs-lookup"><span data-stu-id="45b52-116">Type</span></span>

<span data-ttu-id="45b52-117">A függőségi típusnév.</span><span class="sxs-lookup"><span data-stu-id="45b52-117">Dependency type name.</span></span> <span data-ttu-id="45b52-118">A függőségek logikai jellegű csoportosítását és más mezők, például a commandName és resultCode értelmezése alacsony cardinality értéke.</span><span class="sxs-lookup"><span data-stu-id="45b52-118">Low cardinality value for logical grouping of dependencies and interpretation of other fields like commandName and resultCode.</span></span> <span data-ttu-id="45b52-119">Többek között az SQL Azure-tábla és a HTTP.</span><span class="sxs-lookup"><span data-stu-id="45b52-119">Examples are SQL, Azure table, and HTTP.</span></span>

## <a name="target"></a><span data-ttu-id="45b52-120">cél</span><span class="sxs-lookup"><span data-stu-id="45b52-120">Target</span></span>

<span data-ttu-id="45b52-121">A függőségi hívás célhelyre.</span><span class="sxs-lookup"><span data-stu-id="45b52-121">Target site of a dependency call.</span></span> <span data-ttu-id="45b52-122">Például a kiszolgáló neve, a gazdagép címe.</span><span class="sxs-lookup"><span data-stu-id="45b52-122">Examples are server name, host address.</span></span> <span data-ttu-id="45b52-123">További információkért lásd: [korrelációs](application-insights-correlation.md) lap.</span><span class="sxs-lookup"><span data-stu-id="45b52-123">For more information, see [correlation](application-insights-correlation.md) page.</span></span>

## <a name="duration"></a><span data-ttu-id="45b52-124">Időtartam</span><span class="sxs-lookup"><span data-stu-id="45b52-124">Duration</span></span>

<span data-ttu-id="45b52-125">Időtartam formátumú kérelem: `DD.HH:MM:SS.MMMMMM`.</span><span class="sxs-lookup"><span data-stu-id="45b52-125">Request duration in format: `DD.HH:MM:SS.MMMMMM`.</span></span> <span data-ttu-id="45b52-126">Lehet kisebb, mint `1000` nap.</span><span class="sxs-lookup"><span data-stu-id="45b52-126">Must be less than `1000` days.</span></span>

## <a name="result-code"></a><span data-ttu-id="45b52-127">Eredmény kódja</span><span class="sxs-lookup"><span data-stu-id="45b52-127">Result code</span></span>

<span data-ttu-id="45b52-128">A függőségi hívás eredménykódja.</span><span class="sxs-lookup"><span data-stu-id="45b52-128">Result code of a dependency call.</span></span> <span data-ttu-id="45b52-129">Többek között az SQL-hibakód és a HTTP-állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="45b52-129">Examples are SQL error code and HTTP status code.</span></span>

## <a name="success"></a><span data-ttu-id="45b52-130">Sikeres</span><span class="sxs-lookup"><span data-stu-id="45b52-130">Success</span></span>

<span data-ttu-id="45b52-131">Sikeres vagy sikertelen hívás megjelölése.</span><span class="sxs-lookup"><span data-stu-id="45b52-131">Indication of successful or unsuccessful call.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="45b52-132">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="45b52-132">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="45b52-133">Egyéni mértékek</span><span class="sxs-lookup"><span data-stu-id="45b52-133">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a><span data-ttu-id="45b52-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45b52-134">Next steps</span></span>

- <span data-ttu-id="45b52-135">Állítsa be a követési függőségi [.NET](app-insights-asp-net-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="45b52-135">Set up dependency tracking for [.NET](app-insights-asp-net-dependencies.md).</span></span>
- <span data-ttu-id="45b52-136">Állítsa be a követési függőségi [Java](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="45b52-136">Set up dependency tracking for [Java](app-insights-java-agent.md).</span></span>
- [<span data-ttu-id="45b52-137">Egyéni függőségi telemetria írása</span><span class="sxs-lookup"><span data-stu-id="45b52-137">Write custom dependency telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackdependency)
- <span data-ttu-id="45b52-138">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="45b52-138">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="45b52-139">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="45b52-139">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
