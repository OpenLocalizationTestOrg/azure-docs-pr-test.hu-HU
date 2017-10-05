---
title: "Azure Application Insights Telemetria-adatmodell - Kivételtelemetria |} Microsoft Docs"
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
ms.openlocfilehash: 6b220b0cb6719bac606f599d657d08ab847c7590
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a><span data-ttu-id="2bf31-103">– Kivételtelemetria: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="2bf31-103">Exception telemetry: Application Insights data model</span></span>

<span data-ttu-id="2bf31-104">A [Application Insights](app-insights-overview.md), kivétel példányának jelöli egy kezelt vagy nem kezelt kivétel a figyelt alkalmazás végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="2bf31-104">In [Application Insights](app-insights-overview.md), an instance of Exception represents a handled or unhandled exception that occurred during execution of the monitored application.</span></span>

## <a name="problem-id"></a><span data-ttu-id="2bf31-105">Problémaazonosító</span><span class="sxs-lookup"><span data-stu-id="2bf31-105">Problem Id</span></span>

<span data-ttu-id="2bf31-106">Ahol a kivétel a kód azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2bf31-106">Identifier of where the exception was thrown in code.</span></span> <span data-ttu-id="2bf31-107">Csoportosítás kivételek használatos.</span><span class="sxs-lookup"><span data-stu-id="2bf31-107">Used for exceptions grouping.</span></span> <span data-ttu-id="2bf31-108">Általában kombinációja kivétel típusát és egy, a hívási verem származó függvényt.</span><span class="sxs-lookup"><span data-stu-id="2bf31-108">Typically a combination of exception type and a function from the call stack.</span></span>

<span data-ttu-id="2bf31-109">Maximális hossz: 1024 karakter hosszú lehet</span><span class="sxs-lookup"><span data-stu-id="2bf31-109">Max length: 1024 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="2bf31-110">Súlyossági szint</span><span class="sxs-lookup"><span data-stu-id="2bf31-110">Severity level</span></span>

<span data-ttu-id="2bf31-111">Nyomkövetési súlyossági szint.</span><span class="sxs-lookup"><span data-stu-id="2bf31-111">Trace severity level.</span></span> <span data-ttu-id="2bf31-112">Az érték lehet `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="2bf31-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="exception-details"></a><span data-ttu-id="2bf31-113">A kivétel részletei</span><span class="sxs-lookup"><span data-stu-id="2bf31-113">Exception details</span></span>

<span data-ttu-id="2bf31-114">(A kiterjesztett)</span><span class="sxs-lookup"><span data-stu-id="2bf31-114">(To be extended)</span></span>

## <a name="custom-properties"></a><span data-ttu-id="2bf31-115">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="2bf31-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="2bf31-116">Egyéni mértékek</span><span class="sxs-lookup"><span data-stu-id="2bf31-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="2bf31-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2bf31-117">Next steps</span></span>

- <span data-ttu-id="2bf31-118">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="2bf31-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="2bf31-119">Megtudhatja, hogyan [kivételek az Application insights szolgáltatással a webalkalmazások diagnosztizálásához](app-insights-asp-net-exceptions.md).</span><span class="sxs-lookup"><span data-stu-id="2bf31-119">Learn how to [diagnose exceptions in your web apps with Application Insights](app-insights-asp-net-exceptions.md).</span></span>
- <span data-ttu-id="2bf31-120">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="2bf31-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
