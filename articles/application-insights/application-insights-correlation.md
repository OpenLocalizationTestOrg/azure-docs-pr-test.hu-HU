---
title: "Application Insights Telemetria korrelációs aaaAzure |} Microsoft Docs"
description: "Application Insights telemetria korrelációs"
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
ms.openlocfilehash: 3ed8c589d237cac5daceac939ca893b7d81a2967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-correlation-in-application-insights"></a>Az Application Insights telemetria korrelációs

Hello world micro szolgáltatások, az összes logikai művelethez hello szolgáltatás különböző összetevői végzett munkát. Ezeket az összetevőket külön felügyelhesse [Application Insights](app-insights-overview.md). hello web app összetevő hitelesítési szolgáltató összetevő toovalidate felhasználói hitelesítő adatokkal, és adatokkal hello API összetevő tooget a képi megjelenítéshez tartozó kommunikál. hello API összetevő a kapcsolja más szolgáltatások adatok lekérdezése és gyorsítótár-szolgáltató összetevőket használnak és hello számlázási összetevő a hívás kapcsolatos értesítés. Application Insights által támogatott elosztott telemetriai korrelációs. Melyik összetevő felelős a hibák vagy a teljesítmény romlását toodetect lehetővé teszi.

Ez a cikk ismerteti, hogy több összetevő által küldött hello használják az Application Insights toocorrelate telemetriai adatokat az adatmodellbe. Hello környezetben propagálás módszerek és protokollok magában foglalja. Hello korrelációs fogalmak a különböző nyelvekhez és platformokhoz hello végrehajtását is magában foglalja.

## <a name="telemetry-correlation-data-model"></a>Telemetria korrelációs adatmodell

Az Application Insights definiál egy [adatmodell](application-insights-data-model.md) elosztott telemetriai korrelációhoz. tooassociate telemetriai hello logikai művelettel, minden telemetriai elemnek egy nevű környezet mező `operation_Id`. Ez az azonosító az elosztott hello nyomkövetési minden telemetriai elem által közösen. Egy rétegben a telemetriai adatok elvesztését használatát, így még továbbra is társíthat más összetevők által küldött telemetriai adatokat.

Elosztott logikai műveletet rendszerint kisebb műveletkészlet - hello összetevői által feldolgozott kérelmek áll. Ezek a műveletek által meghatározott [telemetriai kérelem](application-insights-data-model-request-telemetry.md). Minden – kéréstelemetria rendelkezik saját `id` , amely globálisan egyedi módon azonosítja. Minden telemetriai adat - nyomkövetéseket, kivételek, stb. Ehhez a kérelemhez társított hello kell beállítania és `operation_parentId` toohello érték hello kérelem `id`.

Minden kimenő művelet, például http hívás tooanother összetevő által képviselt [– függőségi telemetria](application-insights-data-model-dependency-telemetry.md). – Függőségi telemetria is határozza meg a saját `id` globálisan egyedi. – Kéréstelemetria, ez a függőségi hívás által kezdeményezett használja ezt a szolgáltatást, `operation_parentId`.

Hello nézet elosztott logikai művelet használatával hozhat létre `operation_Id`, `operation_parentId`, és `request.id` rendelkező `dependency.id`. Ezek a mezők telemetriai hívások sorrendjét hello okozatiság is meghatározhat.

Összetevők nyomkövetéseit micro szolgáltatások környezetben előfordulhat, hogy nyissa meg a toohello különböző tárolók. Minden összetevő saját instrumentation kulcs lehet az Application Insights. tooget telemetriai logikai hello a művelethez, kell minden egyes tárolási tooquery adatait. Ha a tárolók száma túl nagy, végre kell toohave mutató hol toolook tovább.

Az Application Insights adatmodell határozza meg két mezők toosolve probléma: `request.source` és `dependency.target`. hello első mező azonosítja, hello függőségi kérelmet kezdeményeztek hello összetevő, és hello második azonosító melyik összetevőnél hello függőségi hívás hello választ adott vissza.


## <a name="example"></a>Példa

Vegyünk például egy alkalmazás készlet árak hello aktuális piaci ár egy készlet hello külső API hívása készletek API használatával. hello készlet árak alkalmazás is rendelkezik `Stock page` hello ügyfél webes böngésző használni `GET /Home/Stock`. hello alkalmazás lekérdezések hello készlet API használatával egy HTTP-hívás `GET /api/stock/value`.

Lekérdezés eredményeként kapott telemetriai elemezheti:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

A hello eredmény nézet vegye figyelembe, hogy az összes telemetriai elemet közös hello legfelső szintű `operation_Id`. Ajax hívás esetén készült hello lap – új egyedi azonosító `qJSXU` hozzárendelt toohello – függőségi telemetria és pageView tartozó azonosító használt `operation_ParentId`. Viszont a kiszolgálói kérelem használja ajax tartozó azonosítója `operation_ParentId`stb.

| ItemType   | név                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | A rendszer lap                |              | STYz               | STYz         |
| függőség | GET/Home/Stock           | qJSXU        | STYz               | STYz         |
| Kérelem    | GET otthoni/Stock            | KqKwlrSt9PA = | qJSXU              | STYz         |
| függőség | /Api/stock/value beolvasása      | bBrf2L7mm2g = | KqKwlrSt9PA =       | STYz         |

Ha most hello hívás `GET /api/stock/value` tooan külső szolgáltatás végrehajtott azt szeretné, hogy a kiszolgáló tooknow hello identitásának. Amelyen beállíthatja `dependency.target` megfelelően mezőben. Ha a külső hello szolgáltatás nem támogatja a figyelési - `target` toohello állomásnév például hello szolgáltatás beállítása `stock-prices-api.com`. Azonban ha a szolgáltatás azonosítja magát vissza egy előre meghatározott HTTP-fejléc - `target` hello szolgáltatás identitást, amely lehetővé teszi, hogy az Application Insights elosztott toobuild nyomkövetési telemetria, hogy a szolgáltatás lekérdezésével tartalmaz. 

## <a name="correlation-headers"></a>Korrelációs fejlécek

Jelenleg is dolgozunk hello RFC javaslatot [korrelációs HTTP protokollt](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Ez a javaslat meghatározza, hogy két fejléc:

- `Request-Id`hello globálisan egyedi azonosító hello hívás végrehajtása
- `Correlation-Context`-portprofil hello name érték párok gyűjteménye elosztott hello nyomkövetési tulajdonságai

hello standard is meghatároz két sémái `Request-Id` generációs - és a hierarchikus. Hello strukturálatlan sémával, nincs jól ismert `Id` hello definiálva kulcs `Correlation-Context` gyűjtemény.

Az Application Insights meghatározása hello [bővítmény](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) hello korreláció HTTP protokollt. Használja `Request-Context` name érték párok hello közvetlen hívónak vagy a hívott fél által használt tulajdonságok toopropagate hello gyűjteménye. Application Insights SDK használja a fejléc tooset `dependency.target` és `request.source` mezőket.

## <a name="open-tracing-and-application-insights"></a>Nyissa meg a nyomkövetés és az Application Insights

[Nyissa meg a nyomkövetési](http://opentracing.io/) és az Application Insights adatok modellek keresi 

- `request`, `pageView` túl leképezhető**Span** rendelkező`span.kind = server`
- `dependency`túl leképezhető**Span** rendelkező`span.kind = client`
- `id`az egy `request` és `dependency` túl leképezhető**Span.Id**
- `operation_Id`túl leképezhető**TraceId**
- `operation_ParentId`túl leképezhető**hivatkozás** típusa`ChileOf`

Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.

Lásd: [specification](https://github.com/opentracing/specification/blob/master/specification.md) és [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) nyitott nyomkövetés fogalmak definícióját.


## <a name="telemetry-correlation-in-net"></a>A .NET telemetriai korrelációs

Időbeli .NET meghatározott módon toocorrelate telemetriai és diagnosztikai naplókat. Nincs `System.Diagnostics.CorrelationManager` tootrack így [LogicalOperationStack és ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource`Windows ETW hello módszer meghatározásához és [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger`használja a [napló hatókörök](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF és a Http átvitel közbeni "aktuális" környezetben propagálás fel.

Azokat a módszereket azonban automatikus elosztott nyomkövetés támogatásának engedélyezése nem. `DiagnosticsSource`van olyan módon toosupport gép korrelációs közötti automatikus. .NET-kódtárakra diagnosztika forrás támogatja, és lehetővé teszi az alhálózatok közötti gépek automatikus propagálás hello korrelációs környezet keresztül hello átviteli például a http.

Hello [tooActivities útmutató](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) diagnosztika forrás tevékenységek nyomon követése hello alapjait ismerteti. 

Az ASP.NET Core 2.0 támogatja a HTTP-fejlécek kinyerési, és új tevékenység kezdési hello. 

`System.Net.HttpClient`Kezdő verzió `<fill in>` hello korrelációs HTTP-fejlécek és követési hello http hívás tevékenységként automatikus injektálási támogatja.

Nincs új Http-modulja [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) a hello ASP.NET klasszikus. Ebben a modulban telemetriai korrelációs DiagnosticsSource használatával. A bejövő kérelem fejlécek alapján tevékenység elindul. Emellett a hello különböző szakaszaiban a kérelem feldolgozása telemetriai ad eredményül. Még a hello esetekben, amikor fut az IIS feldolgozó minden szakasza egy másik kezelése szálak.

Application Insights SDK kezdési verzió `2.4.0-beta1` DiagnosticsSource és tevékenység toocollect telemetriai adatokat használ, és társítsa azt az aktuális tevékenység hello. 

## <a name="next-steps"></a>Következő lépések

- [Egyéni telemetriai adatok írása](app-insights-api-custom-events-metrics.md)
- A bevezetni a micro Application Insights szolgáltatás összetevők. Tekintse meg [által támogatott platformok](app-insights-platforms.md).
- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- Ismerje meg, hogyan túl[kiterjesztése és a telemetriai adatok szűrése](app-insights-api-filtering-sampling.md).
