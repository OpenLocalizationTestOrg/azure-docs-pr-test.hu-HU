---
title: "Application Insights Telemetria adatmodell - aaaAzure Kivételtelemetria |} Microsoft Docs"
description: "Application Insights – kivételtelemetria tartozó adatmodell"
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
ms.openlocfilehash: 4c2b7d1ac3816d5623db9a35819a48a68a13a9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a><span data-ttu-id="143d1-103">– Kivételtelemetria: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="143d1-103">Exception telemetry: Application Insights data model</span></span>

<span data-ttu-id="143d1-104">A [Application Insights](app-insights-overview.md), kivétel példányának jelöli egy figyelt hello alkalmazás végrehajtása közben történt kezelt vagy nem kezelt kivétel.</span><span class="sxs-lookup"><span data-stu-id="143d1-104">In [Application Insights](app-insights-overview.md), an instance of Exception represents a handled or unhandled exception that occurred during execution of hello monitored application.</span></span>

## <a name="problem-id"></a><span data-ttu-id="143d1-105">Problémaazonosító</span><span class="sxs-lookup"><span data-stu-id="143d1-105">Problem Id</span></span>

<span data-ttu-id="143d1-106">Azonosítója, ahol kód hello kivétel lépett fel.</span><span class="sxs-lookup"><span data-stu-id="143d1-106">Identifier of where hello exception was thrown in code.</span></span> <span data-ttu-id="143d1-107">Csoportosítás kivételek használatos.</span><span class="sxs-lookup"><span data-stu-id="143d1-107">Used for exceptions grouping.</span></span> <span data-ttu-id="143d1-108">Általában kombinációja kivétel típusát és a függvényt a hello hívási verem.</span><span class="sxs-lookup"><span data-stu-id="143d1-108">Typically a combination of exception type and a function from hello call stack.</span></span>

<span data-ttu-id="143d1-109">Maximális hossz: 1024 karakter hosszú lehet</span><span class="sxs-lookup"><span data-stu-id="143d1-109">Max length: 1024 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="143d1-110">Súlyossági szint</span><span class="sxs-lookup"><span data-stu-id="143d1-110">Severity level</span></span>

<span data-ttu-id="143d1-111">Nyomkövetési súlyossági szint.</span><span class="sxs-lookup"><span data-stu-id="143d1-111">Trace severity level.</span></span> <span data-ttu-id="143d1-112">Az érték lehet `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="143d1-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="exception-details"></a><span data-ttu-id="143d1-113">A kivétel részletei</span><span class="sxs-lookup"><span data-stu-id="143d1-113">Exception details</span></span>

<span data-ttu-id="143d1-114">(a kiterjesztett toobe)</span><span class="sxs-lookup"><span data-stu-id="143d1-114">(toobe extended)</span></span>

## <a name="custom-properties"></a><span data-ttu-id="143d1-115">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="143d1-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="143d1-116">Egyéni mértékek</span><span class="sxs-lookup"><span data-stu-id="143d1-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="143d1-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="143d1-117">Next steps</span></span>

- <span data-ttu-id="143d1-118">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="143d1-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="143d1-119">Ismerje meg, hogyan túl[kivételek az Application insights szolgáltatással a webalkalmazások diagnosztizálásához](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="143d1-119">Learn how too[diagnose exceptions in your web apps with Application Insights](app-insights-asp-net-exceptions.md).</span></span>
- <span data-ttu-id="143d1-120">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="143d1-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
