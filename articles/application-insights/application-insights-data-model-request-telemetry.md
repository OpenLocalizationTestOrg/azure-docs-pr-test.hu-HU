---
title: "Application Insights Telemetria adatmodell - aaaAzure Telemetriai kérelem |} Microsoft Docs"
description: "Application Insights – kéréstelemetria tartozó adatmodell"
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
ms.openlocfilehash: 6042975a35f5e672e5adb5390feecc63d0b284b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="request-telemetry-application-insights-data-model"></a>Telemetriai kérelem: az Application Insights adatmodell

A kérelem telemetriai elemet (a [Application Insights](app-insights-overview.md)) jelöli hello külső kérelem tooyour alkalmazás által indított végrehajtási logikai sorozata. Minden kérelem végrehajtása azonosítja egyedi `ID` és `url` tartalmazó összes hello végrehajtási paramétert. Logikai szerint csoportosíthatja kérelmek `name` , és adja meg a hello `source` a kérelem. Kód végrehajtása eredményezhet `success` vagy `fail` , és egy bizonyos `duration`. A sikeres és Sikertelen végrehajtások csoportosíthatók további korlátozásokat `resultCode`. Kezdő időpont hello – kéréstelemetria hello boríték szinten definiált.

Telemetria támogatja hello szabványos bővíthetőségi modell használatával egyéni kérelem `properties` és `measurements`.

## <a name="name"></a>Név

Hello kérelem neve útvonalán tooprocess hello kérés jelöli. Alacsony cardinality értéke tooallow kérelmek jobban csoportosítása. A HTTP-kérelmek azt jelöli hello HTTP-metódus és URL-cím elérési út sablont, például `GET /values/{id}` nélkül tényleges hello `id` érték.

Application Insights webes SDK kérelem neve "adott állapotban" tanúsítványinformációit tooletter esetben küld. A felhasználói felület a csoportosítás akkor kis-és nagybetűket, `GET /Home/Index` külön-külön számít a `GET /home/INDEX` annak ellenére, hogy gyakran eredményeznek hello azonos vezérlő és a művelet végrehajtását. Hello, amelyek oka, hogy vannak-e általában URL-címek [kis-és nagybetűket](http://www.w3.org/TR/WD-html40-970708/htmlweb.html). Érdemes lehet toosee, ha az összes `404` hello nagybetűs beírt URL-címek került sor. További a kérelem tenantnév-gyűjtemény ASP.Net webes SDK-ban a hello olvasható [blogbejegyzés](http://apmtips.com/blog/2015/02/23/request-name-and-url/).

Maximális hossz: 1024 karakter hosszú lehet

## <a name="id"></a>ID (Azonosító)

A kérelem hívás példány azonosítója. Kérelem és egyéb telemetriai adatok közötti korreláció használatos. Kell, hogy globálisan egyedi azonosítója. További információkért lásd: [korrelációs](application-insights-correlation.md) lap.

Maximális hossz: 128 karakter hosszú lehet

## <a name="url"></a>URL-cím

Kérelem URL-CÍMÉT és az összes lekérdezési karakterlánc paramétert.

Maximális hossz: 2048 karakter

## <a name="source"></a>Forrás

Hello kérelem forrását. Többek között az hello instrumentation kulcs hello hívó vagy hello hívó hello IP-címét. További információkért lásd: [korrelációs](application-insights-correlation.md) lap.

Maximális hossz: 1024 karakter hosszú lehet

## <a name="duration"></a>Időtartam

Időtartam formátumú kérelem: `DD.HH:MM:SS.MMMMMM`. Pozitív, és kisebbnek kell lennie mint `1000` nap. A mező kitöltése kötelező, mint – kéréstelemetria hello elején és végén hello hello műveletet jelöl.

## <a name="response-code"></a>Válaszkód

A kérelem végrehajtása eredményét. HTTP-állapotkód: a HTTP-kérelmekre. Lehet, hogy `HRESULT` más kéréstípusok értékre, vagy a kivétel típusát.

Maximális hossz: 1024 karakter hosszú lehet

## <a name="success"></a>Sikeres

Sikeres vagy sikertelen hívás megjelölése. A mező kitöltése kötelező. Ha nem állítja be túl`false` -kérelem toobe sikeresnek minősül. Az érték túl`false` Ha művelet kivétel miatt megszakadt vagy a eredmény hibakódot adott vissza.

Hello webes alkalmazásokhoz, a Application Insights kérelem megadása, ha hello válaszkód kevesebb hello sikertelenként `400` vagy túl`401`. Azonban előfordulhatnak olyan esetek, amikor az alapértelmezett leképezés nem felel meg a szemantikai hello alkalmazás hello. A válaszkód `404` jelezheti, hogy "nincs rekordok", amely rendszeres folyamat része lehet. Azt is jelezheti egy megszakadt hivatkozás. Hello hivatkozások hibás még akkor is alkalmazhat az összetettebb logikát. Csak akkor, ha ezeket a hivatkozásokat a ugyanaz a hely URL-cím hivatkozó elemzésével hello lévő hivatkozások is megjelölése hibák. Vagy hibák hello vállalati mobilalkalmazás történő megjelölése őket. Hasonlóképpen `301` és `302` hello ügyfélről, amely nem támogatja az átirányítási elérésekor hibát jelez.

Részlegesen elfogadta a tartalom `206` utalhat egy általános kérelem sikertelen. Az Application Insights végpont például egyetlen kérelemként kapja meg a kötegelt telemetriai elemek. Azt adja vissza `206` amikor bizonyos elemek hello kötegben fel nem dolgozott sikeresen megtörtént. Növekvő mértékű `206` , amelyet a vizsgált toobe hibáját jelzi. Hasonló logika vonatkozik túl`207` ahol hello sikeres lehet, hogy több állapot hello állapotösszegzés legrosszabb állapotán külön válaszkódot.

További a kérelem eredménye olvasható kód és a hello állapotkód [blogbejegyzés](http://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Egyéni tulajdonságok

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Egyéni mértékek

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Következő lépések

- [Egyéni – kéréstelemetria írása](app-insights-api-custom-events-metrics.md#trackrequest)
- Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- Ismerje meg, hogyan túl[konfigurálása az ASP.NET Core](app-insights-asp-net.md) alkalmazás az Application insights szolgáltatással.
- Tekintse meg [platformok](app-insights-platforms.md) Application Insights által támogatott.
