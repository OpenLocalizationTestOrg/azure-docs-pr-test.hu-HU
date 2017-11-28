---
title: "aaaAzure Application Insights Telemetria adatmodell - esemény Telemetriai |} Microsoft Docs"
description: "Application Insights adatmodell esemény telemetriai adat"
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
ms.openlocfilehash: cd7dc3c5f4f3df22b7a52ee79fcad566a27a9f4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-telemetry-application-insights-data-model"></a><span data-ttu-id="5d9f5-103">Esemény telemetriai: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="5d9f5-103">Event telemetry: Application Insights data model</span></span>

<span data-ttu-id="5d9f5-104">Esemény telemetriai elemek is létrehozhat (a [Application Insights](app-insights-overview.md)) toorepresent esemény történt az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-104">You can create event telemetry items (in [Application Insights](app-insights-overview.md)) toorepresent an event that occurred in your application.</span></span> <span data-ttu-id="5d9f5-105">Az általában egy felhasználói beavatkozást például kattintson vagy sorrend kivételt.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-105">Typically it is a user interaction such as button click or order checkout.</span></span> <span data-ttu-id="5d9f5-106">Például az inicializálási vagy a konfiguráció módosítása alkalmazás életciklusa esemény is lehet.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-106">It can also be an application life cycle event like initialization or configuration update.</span></span> 

<span data-ttu-id="5d9f5-107">Események szemantikailag, előfordulhat, hogy, vagy nem lehet korrelált toorequests.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-107">Semantically, events may or may not be correlated toorequests.</span></span> <span data-ttu-id="5d9f5-108">Azonban ha megfelelően használják, esemény telemetriai fontosabb, mint a kéréseket, és a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-108">However, if used properly, event telemetry is more important than requests or traces.</span></span> <span data-ttu-id="5d9f5-109">Események üzleti telemetriai jelöl, és legyen a tulajdonos tooseparate, kevesebb agresszív [mintavételi](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="5d9f5-109">Events represent business telemetry and should be a subject tooseparate, less aggressive [sampling](app-insights-api-filtering-sampling.md).</span></span>

## <a name="name"></a><span data-ttu-id="5d9f5-110">Név</span><span class="sxs-lookup"><span data-stu-id="5d9f5-110">Name</span></span>

<span data-ttu-id="5d9f5-111">Az esemény neve.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-111">Event name.</span></span> <span data-ttu-id="5d9f5-112">tooallow megfelelő csoportosítás és hasznos metrikákat, korlátozhatja az alkalmazás így külön esemény nevek kis számú generál.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-112">tooallow proper grouping and useful metrics, restrict your application so that it generates a small number of separate event names.</span></span> <span data-ttu-id="5d9f5-113">Például ne használja az esemény minden egyes létrehozott példány külön nevét.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-113">For example, don't use a separate name for each generated instance of an event.</span></span>

<span data-ttu-id="5d9f5-114">Maximális hossz: 512 karakter</span><span class="sxs-lookup"><span data-stu-id="5d9f5-114">Max length: 512 characters</span></span>

## <a name="custom-properties"></a><span data-ttu-id="5d9f5-115">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="5d9f5-115">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a><span data-ttu-id="5d9f5-116">Egyéni mértékek</span><span class="sxs-lookup"><span data-stu-id="5d9f5-116">Custom measurements</span></span>

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a><span data-ttu-id="5d9f5-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d9f5-117">Next steps</span></span>

- <span data-ttu-id="5d9f5-118">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-118">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="5d9f5-119">Egyéni esemény telemetriai adatok írása</span><span class="sxs-lookup"><span data-stu-id="5d9f5-119">Write custom event telemetry</span></span>](app-insights-api-custom-events-metrics.md#trackevent)
- <span data-ttu-id="5d9f5-120">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="5d9f5-120">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
