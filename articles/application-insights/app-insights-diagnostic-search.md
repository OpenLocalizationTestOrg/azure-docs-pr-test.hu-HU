---
title: "Keresés a Azure Application Insights aaaUsing |} Microsoft Docs"
description: "Keresés és szűrés nyers telemetriaadatok küldött a webes alkalmazást."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: df2b0eb017ad48bcdc6ef57d8dff207d120143b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-search-in-application-insights"></a>Az Application Insightsban keresés használata
Keresés csak a [Application Insights](app-insights-overview.md) alkalmazni toofind és egyéni telemetriai elemek, például Lapmegtekintések, kivételek, így megismerkedhet vagy webalkalmazás-kérelmeket. És naplókivonatokat és eseményeket, amelyek rendelkeznek a kódolt megtekintéséhez.

(Az adatok a összetettebb lekérdezések, használjon [Analytics](app-insights-analytics-tour.md).)

## <a name="where-do-you-see-search"></a>Ha látja keresési?
### <a name="in-hello-azure-portal"></a>A hello Azure-portálon
Diagnosztikai keresési explicit módon megnyithatja az alkalmazás hello Application Insights – áttekintés paneljéről:

![Diagnosztikai keresés megnyitása](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)

Néhány diagramok és a rács elemek kattintva is megnyílik. Ebben az esetben a szűrők vannak előre beállított toofocus hello típusú kijelölt elemhez. 

Például a hello áttekintése panelen nincs kérelmek válaszideje által besorolt sávdiagram. Kattintson a teljesítmény tartomány toosee tartalmazó lista egyes abban a válaszidő tartomány:

![Kattintson a kérelem teljesítmény](./media/app-insights-diagnostic-search/07-open-from-filters.png)

hello fő diagnosztikai keresési szövegtörzse listáját telemetriai elemek - kiszolgálói kérelmek lapon a nézetek, egyéni események, amelyek rendelkeznek a kódolt és így tovább. Hello hello lista elejére egy összefoglaló táblázat számát is események adott idő alatt.

Kattintson a frissítés tooget új eseményeket.

### <a name="in-visual-studio"></a>Visual Studióban

A Visual Studio esetében is az Application Insights keresési ablak. A leghasznosabb, amely akkor hibakeresése hello alkalmazás által létrehozott telemetriai események megjelenítése. Azonban akkor is megjelenhet hello események gyűjtött hello Azure-portálon a közzétett alkalmazást.

Visual Studio hello keresési ablak megnyitása:

![A Visual Studio Application Insights keresés megnyitása](./media/app-insights-diagnostic-search/32.png)

hello keresési ablak szolgáltatások hasonló toohello webes portál esetében:

![Visual Studio Application Insights – keresési ablak](./media/app-insights-diagnostic-search/34.png)

hello követése művelet lapon érhető el egy kérelem vagy egy nézet megnyitásakor. Egy "művelet" tooa egyetlen kérelemmel vagy a lap nézetben kapcsolódó események sorozata. Például függőségi hívások esetében, kivételek, nyomkövetési naplókat és egyéni események része lehet egyetlen művelettel. hello követése művelet lapon megjelenik a grafikusan hello ütemezése és ezek az események kapcsolat toohello kérelemmel vagy a lap nézetben időtartama. 

## <a name="inspect-individual-items"></a>Vizsgálja meg az egyes elemek
Válassza ki a telemetriai elem toosee kulcsmezők és a kapcsolódó elemek. Ha azt szeretné, hogy toosee hello teljes mezők halmaza, kattintson a "...". 

![Kattintson az új munkaelemre vonatkozóan, hello mezők szerkesztéséhez, és kattintson az OK gombra.](./media/app-insights-diagnostic-search/10-detail.png)

## <a name="filter-event-types"></a>Szűrő eseménytípusok
Nyissa meg a hello szűrő panelre, és válassza a hello esemény meg kell adnia toosee szeretné. (, Később, amellyel hello panelen megnyitott toorestore hello szűrők, kattintson a alaphelyzetbe állítása.)

![Kattintson a szűrő és telemetriai kijelölve](./media/app-insights-diagnostic-search/02-filter-req.png)

hello esemény típusok a következők:

* **Nyomkövetési** - [diagnosztikai naplók](app-insights-asp-net-trace-logs.md) TrackTrace, log4Net, NLog és System.Diagnostic.Trace hívások beleértve.
* **Kérelem** -HTTP-kérések a kiszolgálóalkalmazás, beleértve a lapok, parancsfájlok, képek, stílus fájlokat és adatokat fogadja. Ezek az események használt toocreate hello kérelem-válasz áttekintő diagramok.
* **Lapmegtekintés** - [hello webes ügyfél által küldött Telemetriai](app-insights-javascript.md), használt toocreate lap jelentések megtekintése. 
* **Egyéni esemény** – ha beszúrt hívások tooTrackEvent() sorrendben túl[megfigyeléséhez](app-insights-api-custom-events-metrics.md), Itt kereshet bennük.
* **Kivétel** – fellépő nem kezelt [kivételek hello Server](app-insights-asp-net-exceptions.md), valamint azokat, amelyeket a TrackException() használatával jelentkezik.
* **A függőségi** - [a kiszolgálóalkalmazás hívásait](app-insights-asp-net-dependencies.md) tooother szolgáltatások, például REST API-k vagy az adatbázisok, és az AJAX-hívásai a [Ügyfélkód](app-insights-javascript.md).
* **Rendelkezésre állási** -eredményeinek [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>A tulajdonságértékek szűrése
A tulajdonságok értékeit hello események jeleníthetők meg. rendelkezésre álló tulajdonságok hello kiválasztott hello eseménytípusok függ. 

Válasszon például adott válaszkód kéréseket. 

![Bontsa ki a tulajdonságot, és adjon meg értéket](./media/app-insights-diagnostic-search/03-response500.png)

Ugyanaz, mint minden értékek érvényesíti hello kiválasztása nem egy adott tulajdonság értékének rendelkezik. Vált ki a a tulajdonságon alapuló szűrőt.

### <a name="narrow-your-search"></a>A keresés szűkítéséhez
Figyelje meg, hogy hello száma sarkában hello szűrőértékek toohello megjelenítése hány előfordulások nem szerepelnek a hello aktuális szűrt készletében. 

Az ebben a példában is egyértelmű hello Rpt/alkalmazottak kérés hello "500" hibák többségét eredményezi:

![Bontsa ki a tulajdonságot, és adjon meg értéket](./media/app-insights-diagnostic-search/04-failingReq.png)




## <a name="find-events-with-hello-same-property"></a>Esemény megkeresése, a hello ugyanahhoz a tulajdonsághoz
Összes hello található elemek hello azonos tulajdonság értéke:

![Kattintson a jobb gombbal egy tulajdonság](./media/app-insights-diagnostic-search/12-samevalue.png)


## <a name="search-hello-data"></a>Keresési hello adatok

> [!NOTE]
> toowrite összetettebb lekérdezések, nyissa meg [ **Analytics** ](app-insights-analytics-tour.md) a hello felső hello keresési panelről.
> 

Bármely hello tulajdonságértékek feltételeket is kereshet. Ez különösen akkor hasznos, ha írt az [egyéni események](app-insights-api-custom-events-metrics.md) tulajdonság értékekkel. 

Érdemes lehet egy időtartományt, mint rövidebb tartományban keresések gyorsabbak tooset. 

![Diagnosztikai keresés megnyitása](./media/app-insights-diagnostic-search/appinsights-311search.png)

Teljes szavak, nem karakterláncrész keresése. Idézőjelek tooenclose különleges karakterek használhatók.

| Karakterlánc | van *nem* által talált | Ezek találja |
| --- | --- | --- |
| HomeController.About |otthoni<br/>Tartományvezérlő<br/>Kimenő | homecontroller<br/>tudnivalók<br/>"homecontroller.about"|
|Egyesült Államok|UNI<br/>TED|Egyesült<br/>állapotok<br/>Egyesült Államok és<br/>"az Amerikai Egyesült Államok"

Az alábbiakban hello keresési kifejezéseket is használhat:

| Mintalekérdezés | Következmény |
| --- | --- |
| `apple` |Összes esemény található egyéb mezőjének tartalmaznia hello word "apple" hello időtartomány |
| `apple AND banana` |Megkeresése, amelynek mindkét szavakat tartalmaznak. A tőkéhez "és", nem használható "és". |
| `apple OR banana`<br/>`apple banana` |Megkeresése, amelynek vagy szót tartalmaz. "Vagy", nem használható "vagy".<br/>Rövid alak. |
| `apple NOT banana` |Esemény megkeresése, amelyek tartalmaznak egy szót, de nem hello más. |



## <a name="sampling"></a>Mintavételezés
Ha az alkalmazás nagy mennyiségű telemetriai adatokat hoz létre (hello ASP.NET SDK verzió 2.0.0-beta3 használ, és vagy újabb), hello adaptív mintavételi modul automatikusan csökkenti toohello portal reprezentatív része események küldése által küldött hello kötet. Azonban események, amelyek kapcsolódó toohello kérésben kiválasztva, vagy nincs kijelölve csoportosan, hogy a kapcsolódó események közti léphet. 

[Ismerkedés a mintavételezéssel](app-insights-sampling.md).



## <a name="create-work-item"></a>Munkaelem létrehozása
A Githubból vagy a Visual Studio Team Services programhiba bármely telemetriai elemből hello adatokkal hozhat létre. 

![Kattintson az új munkaelemre vonatkozóan, hello mezők szerkesztéséhez, és kattintson az OK gombra.](./media/app-insights-diagnostic-search/42.png)

hello ehhez, a rendszer felkéri tooconfigure először egy hivatkozás tooyour Team Services fiókját, és a projekt.

![Töltse ki a hello URL-CÍMÉT a Team Services-kiszolgáló és a hello projekt nevét, majd kattintson az Engedélyezés parancsra](./media/app-insights-diagnostic-search/41.png)

(Beállíthatja úgy is hello hivatkozás hello munkaelemek panelen.)

## <a name="save-your-search"></a>A Keresés mentése
Miután beállított összes hello szűrők azt szeretné, hello keresési mentheti a Kedvencek közé. Egy szervezeti fiók dolgozik, dönthet úgy, hogy tooshare, a többi csoport tagjaival.

![Kattintson a kedvenc, állítson be hello nevet és kattintson a Mentés gombra](./media/app-insights-diagnostic-search/08-favorite-save.png)

Ebben az esetben a toosee hello keresési **lépjen toohello áttekintése panel** , és nyissa meg a Kedvencek:

![Kedvencek csempe](./media/app-insights-diagnostic-search/09-favorite-get.png)

Ha mentett relatív időtartomány, a hello újra megnyitni panel hello legfrissebb adatokat tartalmaz. Ha mentette az abszolút időtartomány, hello látható minden egyes ugyanazokat az adatokat. ("Relatív" érhető el, ha azt szeretné, hogy toosave kedvenc, ha az időtartományt hello fejlécben kattintson, és a beállítása egy időtartományt, amely nem egy egyéni tartományt.)

## <a name="send-more-telemetry-tooapplication-insights"></a>További telemetriai adatokat küldhet tooApplication Insights
Továbbá toohello out-of-az-box telemetriai Application Insights SDK által küldött, a következőket teheti:

* A kedvenc naplózási keretrendszer a naplózási nyomkövetés rögzítése a [.NET](app-insights-asp-net-trace-logs.md) vagy [Java](app-insights-java-trace-logs.md). Ez azt jelenti, hogy a naplókivonatokat közötti keresésre, és a kivizsgált Lapmegtekintések, kivételeket és eseményeket. 
* [Kód írása](app-insights-api-custom-events-metrics.md) toosend egyéni események Lapmegtekintések és kivételeket. 

[Ismerje meg, hogy miként naplózza az toosend és egyéni telemetriai tooApplication Insights](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>A KÉRDÉSEK ÉS VÁLASZOK
### <a name="limits"></a>Mennyi adatot megmarad?

Lásd: hello [korlátok összegzés](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Honnan látom POST-adatokat a saját kiszolgáló kérések?
Automatikusan azt ne naplózza a hello POST-adatokat, de használhat [TrackTrace vagy a napló hívások](app-insights-asp-net-trace-logs.md). Helyezze el hello üzenet paraméter hello POST-adatokat. Meg nem lehet szűrést végezni a hello üdvözlőüzenetére azonos módon szűrheti a tulajdonságok, de már hello méretkorlátját.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Következő lépések
* [Összetett lekérdezések írás Analytics](app-insights-analytics-tour.md)
* [Naplók és egyéni telemetriai adatokat küldeni tooApplication Insights](app-insights-asp-net-trace-logs.md)
* [Rendelkezésre állási és reakcióidőt tesztek beállítása](app-insights-monitor-web-app-availability.md)
* [hibaelhárítással](app-insights-troubleshoot-faq.md)
