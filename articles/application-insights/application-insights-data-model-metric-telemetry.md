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
# <a name="metric-telemetry-application-insights-data-model"></a>Metrika telemetriai: Application Insights adatmodell

Metrika telemetriai által támogatott két típusa van [Application Insights](app-insights-overview.md): egyszeri mérési és előre összesített mértéket. Egyetlen érték csak a nevét és értékét. Előre összesített metrika hello metrika minimális és maximális értékének hello az aggregációs időköznek és szórása azt határozza meg.

Előre összesített metrika telemetriai azt feltételezi, hogy az összesítő időszak egy perc volt.

Nincsenek Application Insights által támogatott több jól ismert metrika neve. 

A mérőszám a rendszer és a folyamat számlálók képviselő:

| **.NET neve**             | **Platform független neve** | **REST API-név** | **Leírás**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | Megoldás folyamatban... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | gép Processzorainak száma
| `\Memory\Available Bytes`                 | Megoldás folyamatban... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | a lemezen rendelkezésre álló memória
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | Megoldás folyamatban... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | Hello alkalmazást hello folyamat CPU
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | Megoldás folyamatban... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | hello alkalmazást hello folyamat által használt memória
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | Megoldás folyamatban... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | i/o-műveletek folyamat hello alkalmazást futtat
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | Megoldás folyamatban... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | alkalmazás által feldolgozott kérelmek száma 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | Megoldás folyamatban... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | alkalmazás által kiváltott kivételekre is ki aránya
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | Megoldás folyamatban... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | a kérelmek átlagos végrehajtási idő
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | Megoldás folyamatban... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | a várólista feldolgozása hello várakozó kérelmek száma

## <a name="name"></a>Név

Neve az Application Insights portál és a felhasználói felület toosee milyen hello metrika. 

## <a name="value"></a>Érték

Mérési egyetlen értéket. Egyéni mértékek hello összesítés céljából összege.

## <a name="count"></a>Darabszám

Metrika súlyának összesítve hello metrika. Nem lehet beállítani a mérés.

## <a name="min"></a>Perc

Összesítve hello metrika minimális értéke. Nem lehet beállítani a mérés.

## <a name="max"></a>Maximum

Összesítve hello metrika maximális értékét. Nem lehet beállítani a mérés.

## <a name="standard-deviation"></a>Szórás

Hello szórása metrika összesíteni. Nem lehet beállítani a mérés.

## <a name="custom-properties"></a>Egyéni tulajdonságok

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan toouse [Application Insights API egyéni események és metrikák](app-insights-api-custom-events-metrics.md#trackmetric).
- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.
