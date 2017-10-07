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
# <a name="event-telemetry-application-insights-data-model"></a>Esemény telemetriai: Application Insights adatmodell

Esemény telemetriai elemek is létrehozhat (a [Application Insights](app-insights-overview.md)) toorepresent esemény történt az alkalmazásban. Az általában egy felhasználói beavatkozást például kattintson vagy sorrend kivételt. Például az inicializálási vagy a konfiguráció módosítása alkalmazás életciklusa esemény is lehet. 

Események szemantikailag, előfordulhat, hogy, vagy nem lehet korrelált toorequests. Azonban ha megfelelően használják, esemény telemetriai fontosabb, mint a kéréseket, és a nyomkövetési adatokat. Események üzleti telemetriai jelöl, és legyen a tulajdonos tooseparate, kevesebb agresszív [mintavételi](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Név

Az esemény neve. tooallow megfelelő csoportosítás és hasznos metrikákat, korlátozhatja az alkalmazás így külön esemény nevek kis számú generál. Például ne használja az esemény minden egyes létrehozott példány külön nevét.

Maximális hossz: 512 karakter

## <a name="custom-properties"></a>Egyéni tulajdonságok

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Egyéni mértékek

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Következő lépések

- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- [Egyéni esemény telemetriai adatok írása](app-insights-api-custom-events-metrics.md#trackevent)
- Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.
