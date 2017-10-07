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
# <a name="exception-telemetry-application-insights-data-model"></a>– Kivételtelemetria: Application Insights adatmodell

A [Application Insights](app-insights-overview.md), kivétel példányának jelöli egy figyelt hello alkalmazás végrehajtása közben történt kezelt vagy nem kezelt kivétel.

## <a name="problem-id"></a>Problémaazonosító

Azonosítója, ahol kód hello kivétel lépett fel. Csoportosítás kivételek használatos. Általában kombinációja kivétel típusát és a függvényt a hello hívási verem.

Maximális hossz: 1024 karakter hosszú lehet

## <a name="severity-level"></a>Súlyossági szint

Nyomkövetési súlyossági szint. Az érték lehet `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>A kivétel részletei

(a kiterjesztett toobe)

## <a name="custom-properties"></a>Egyéni tulajdonságok

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Egyéni mértékek

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Következő lépések

- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- Ismerje meg, hogyan túl[kivételek az Application insights szolgáltatással a webalkalmazások diagnosztizálásához](app-insights-asp-net-exceptions.md).
- Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.
