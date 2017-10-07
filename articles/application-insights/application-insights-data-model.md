---
title: Application Insights Telemetria adatmodell aaaAzure |} Microsoft Docs
description: "Application Insights az modell áttekintése"
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
ms.openlocfilehash: 5610519c3db8ec68d6cf787639204fb79724f511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetria adatmodell

[Az Azure Application Insights](app-insights-overview.md) telemetriai küldi a webes alkalmazás toohello Azure-portálon, így hello teljesítmény- és az alkalmazás használati elemzése. hello telemetriai modell szabványosított, hogy lehetséges toocreate platform és nyelvfüggetlen figyelését. 

Az Application Insights által gyűjtött adatokat a tipikus végrehajtási alkalmazásminta modellek:

![Application Insights Alkalmazásmodellt.](./media/application-insights-data-model/application-insights-data-model.png)

hello következő telemetriai használt toomonitor hello az alkalmazás végrehajtása során. hello következő háromféle általában automatikusan begyűjti a hello Application Insights SDK által hello webes alkalmazás-keretrendszer:

* [**Kérelem** ](application-insights-data-model-request-telemetry.md) -toolog egy kérelmet, az alkalmazás által fogadott jön létre. Például hello Application Insights webes SDK automatikusan hoz létre, amely megkapja a webes alkalmazás egyes HTTP-kérelem a kérelem telemetriai elem. 

    Egy **művelet** van, amely feldolgozza a kérést végrehajtó hello szálát. Emellett [írhat kódot](app-insights-api-custom-events-metrics.md#trackrequest) toomonitor más típusú műveletet, mint például a "ébresztésére" egy a web feladat, vagy nem működik, amely rendszeres időközönként adatokat dolgozza fel.  Egyes műveletek rendelkezik azonosítóval. Ezt az Azonosítót, amely használt too[group]((application-insights-correlation.md) a összes telemetriai adat jelentkezik, míg az alkalmazás hello kérelem feldolgozása. Egyes műveletek vagy sikeres vagy sikertelen lesz, és idő időtartama.
* [**Kivétel** ](application-insights-data-model-exception-telemetry.md) -általában az egy művelet toofail okozó kivételt jelenti.
* [**A függőségi** ](application-insights-data-model-dependency-telemetry.md) -hívás jelöli az alkalmazás tooan külső szolgáltatás vagy a tárhelyen, többek között a REST API-t vagy az SQL. Az ASP.NET, függőségi hívásokat tooSQL határozzák `System.Data`. Hívások tooHTTP végpontokat `System.Net`. 

Az Application Insights három további adattípusok egyéni telemetriai adatokat ismerteti:

* [Nyomkövetési](application-insights-data-model-trace-telemetry.md) - használt vagy közvetlenül, vagy egy adapter tooimplement diagnosztikai naplózás keresztül egy instrumentation keretrendszer, amely jól ismert tooyou, például használatával `Log4Net` vagy `System.Diagnostics`.
* [Esemény](application-insights-data-model-event-telemetry.md) -jellemzően a szolgáltatással, tooanalyze használati minták toocapture felhasználói beavatkozást.
* [A metrika](application-insights-data-model-metric-telemetry.md) -tooreport rendszeres skaláris mérések használt.

Minden telemetriai elem definiálhat hello [kontextusadatok](application-insights-data-model-context.md) például alkalmazás verziója vagy a felhasználói munkamenet-azonosító. A környezetben a mezők halmaza alapján szigorú típusmeghatározású, amely bizonyos forgatókönyvekben feloldja. Alkalmazásverzió megfelelően van inicializálva, az Application Insights képes észlelni új minták szorosan összefügg az újratelepítés alkalmazás viselkedésében. Munkamenet-azonosító lehet használt toocalculate hello kimaradás vagy egy problémát okozzon a felhasználóknak. Függőség munkamenet-azonosító értékek eltérők száma az egyes kiszámítása nem sikerült, hiba nyomkövetési vagy kritikus kivétel hatással beható ismerete biztosítja.

Az Application Insights telemetria modell meghatározása úgy túl[összefüggéseket](application-insights-correlation.md) telemetriai toohello művelet, amely egy részét képezi. Például egy kérelem egy SQL-adatbázis hívás, és diagnosztikai adatait rögzíti. Hello korrelációs környezetben összekötését azt vissza toohello – kéréstelemetria telemetriai elemeket állíthatja be.

## <a name="schema-improvements"></a>Séma fejlesztései

Application Insights adatmodell egy egyszerű és egyszerű, mégis hatékony módon toomodel a telemetriai van. A Microsoft szükség tookeep hello modell egyszerű és slim toosupport alapvető alkalmazási és speciális használatra tooextend hello séma engedélyezése.

tooreport adatok modell és séma problémák és javaslatokat használja a Githubon [ApplicationInsights-Home](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) tárházba.

## <a name="next-steps"></a>Következő lépések

- [Egyéni telemetriai adatok írása](app-insights-api-custom-events-metrics.md)
- Ismerje meg, hogyan túl[kiterjesztése és a telemetriai adatok szűrése](app-insights-api-filtering-sampling.md).
- Használjon [mintavételi](app-insights-sampling.md) adatmodellen alapuló toominimize telemetriai adatok mennyiségét.
- Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.
