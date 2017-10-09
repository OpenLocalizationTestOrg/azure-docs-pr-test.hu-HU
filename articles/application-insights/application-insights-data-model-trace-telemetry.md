---
title: "aaaAzure Application Insights Telemetria adatmodell - nyomkövetési Telemetria |} Microsoft Docs"
description: "Application Insights – nyomkövetési telemetria tartozó adatmodell"
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
ms.openlocfilehash: dfdee958e07d57448ff41abc5cd33bfd05dac090
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="trace-telemetry-application-insights-data-model"></a><span data-ttu-id="17f7f-103">Nyomkövetési telemetria: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="17f7f-103">Trace telemetry: Application Insights data model</span></span>

<span data-ttu-id="17f7f-104">Nyomkövetési telemetria (a [Application Insights](app-insights-overview.md)) jelöli `printf` stílus nyomkövetési utasítás szöveget keres.</span><span class="sxs-lookup"><span data-stu-id="17f7f-104">Trace telemetry (in [Application Insights](app-insights-overview.md)) represents `printf` style trace statements that are text-searched.</span></span> <span data-ttu-id="17f7f-105">`Log4Net`, `NLog`, és más szöveges naplófájl-bejegyzéseket az ilyen típusú példányok van lefordítva.</span><span class="sxs-lookup"><span data-stu-id="17f7f-105">`Log4Net`, `NLog`, and other text-based log file entries are translated into instances of this type.</span></span> <span data-ttu-id="17f7f-106">hello nyomkövetési nem lehet mérési egy bővíthetőségi.</span><span class="sxs-lookup"><span data-stu-id="17f7f-106">hello trace does not have measurements as an extensibility.</span></span>

## <a name="message"></a><span data-ttu-id="17f7f-107">Üzenet</span><span class="sxs-lookup"><span data-stu-id="17f7f-107">Message</span></span>

<span data-ttu-id="17f7f-108">Nyomkövetési üzenet.</span><span class="sxs-lookup"><span data-stu-id="17f7f-108">Trace message.</span></span>

<span data-ttu-id="17f7f-109">Maximális hossz: 32768 karakter</span><span class="sxs-lookup"><span data-stu-id="17f7f-109">Max length: 32768 characters</span></span>

## <a name="severity-level"></a><span data-ttu-id="17f7f-110">Súlyossági szint</span><span class="sxs-lookup"><span data-stu-id="17f7f-110">Severity level</span></span>

<span data-ttu-id="17f7f-111">Nyomkövetési súlyossági szint.</span><span class="sxs-lookup"><span data-stu-id="17f7f-111">Trace severity level.</span></span> <span data-ttu-id="17f7f-112">Az érték lehet `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span><span class="sxs-lookup"><span data-stu-id="17f7f-112">Value can be `Verbose`, `Information`, `Warning`, `Error`, `Critical`.</span></span>

## <a name="custom-properties"></a><span data-ttu-id="17f7f-113">Egyéni tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="17f7f-113">Custom properties</span></span>

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a><span data-ttu-id="17f7f-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17f7f-114">Next steps</span></span>

- <span data-ttu-id="17f7f-115">[.NET nyomkövetési naplók megtekintése az Application Insights felfedezés](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="17f7f-115">[Explore .NET trace logs in Application Insights](app-insights-asp-net-trace-logs.md).</span></span>
- <span data-ttu-id="17f7f-116">[Fedezze fel az Application Insights nyomkövetési naplók Java](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="17f7f-116">[Explore Java trace logs in Application Insights](app-insights-java-trace-logs.md).</span></span>
- <span data-ttu-id="17f7f-117">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="17f7f-117">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- [<span data-ttu-id="17f7f-118">Egyéni nyomkövetési telemetria írása</span><span class="sxs-lookup"><span data-stu-id="17f7f-118">Write custom trace telemetry</span></span>](app-insights-api-custom-events-metrics.md#tracktrace)
- <span data-ttu-id="17f7f-119">Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.</span><span class="sxs-lookup"><span data-stu-id="17f7f-119">Check out [platforms](app-insights-platforms.md) supported by Application Insights.</span></span>
