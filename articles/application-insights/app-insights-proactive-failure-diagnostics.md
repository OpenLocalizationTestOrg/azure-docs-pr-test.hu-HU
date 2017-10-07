---
title: "Észlelési - hiba rendellenességek észlelését, és az Application Insightsban aaaSmart |} Microsoft Docs"
description: "Riasztást küld, a sikertelen kérelmek tooyour webalkalmazás hello aránya toounusual módosításokat, és diagnosztikai elemzés biztosít. Nincs a konfigurációra nincs szükség."
services: application-insights
documentationcenter: 
author: yorac
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: bwren
ms.openlocfilehash: dfd178ca9546294be91f27b0c1b846d519539b77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection---failure-anomalies"></a>Intelligens észlelési - hiba rendellenességek észlelését
[Az Application Insights](app-insights-overview.md) automatikusan értesíti a felhasználót közel valós idejű Ha a webalkalmazás szokatlan mértékben megnőtt a sikertelen kérelmek hello aránya. Azt észleli, hogy egy szokatlan megnövekedhet a HTTP-kérelmek vagy sikertelenként jelentett függőségi hívások hello aránya. A kéréseket a sikertelen kérelmek rendszerint rendelkező válaszkódot 400 vagy magasabb. toohelp osztályozhatja és diagnosztizálhatja hello probléma, hello jellemzői hello hibák és a kapcsolódó telemetriai adatok elemzését hello értesítési megtalálható. Van még hivatkozások toohello Application Insights portál további elemzés céljából. hello szolgáltatást kell nincs beállításról, sem a konfigurációban, mert gépi tanulási algoritmusok toopredict hello normál hibaaránya.

Ez a szolgáltatás működik, Java és az ASP.NET web Apps, vagy a saját kiszolgálóit egy hello felhőben. Azt is működik, bármely alkalmazás, amely hoz létre a kérelem vagy függőségi telemetria - például, ha a feldolgozói szerepkör, amely behívja [TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) vagy [TrackDependency()](app-insights-api-custom-events-metrics.md#trackdependency).

Beállítása után [a projekthez az Application Insights](app-insights-overview.md), és az alkalmazás egyes minimális telemetriai adatokat állít elő, ha intelligens tegye a hiba rendellenességek észlelését tart, 24 óra toolearn hello normál az alkalmazás viselkedését, előtte bekapcsolt és riasztásokat küldhet.

Íme egy minta riasztás.

![A minta intelligens észleléséről szóló figyelmeztetés a fürt elemzési hiba körül megjelenítő](./media/app-insights-proactive-failure-diagnostics/013.png)

> [!NOTE]
> Alapértelmezés szerint egy rövidebb, mint ebben a példában formátum mail kap. Azonban úgy is [kapcsoló toothis részletes formátumban](#configure-alerts).
>
>

Figyelje meg, amely jelzi, hogy:

* hello os Hibaarány, szemben toonormal alkalmazása működését.
* Érintett felhasználók számát – így megtudhatja, hogy mekkora tooworry.
* Hello hibák társított jellemző mintát. Ebben a példában van egy adott válaszkód, kérés neve (művelet) és az alkalmazás verziója. Mely azonnal jelzi, hogy hol toostart keresése a kódban. Egyéb lehetőségek egy megadott böngésző vagy az ügyfél operációs rendszer lehet.
* hello kivétel, naplókivonatokat és függőségi hiba (adatbázisok vagy más külső összetevő) hello társított toobe megjelenő jellemző hibák.
* Az Application Insightsban hello telemetriai toorelevant keresés közvetlenül hivatkozik.

## <a name="benefits-of-smart-detection"></a>Intelligens észlelési előnyei
A szokványos [metrika riasztások](app-insights-alerts.md) jelzi, hogy probléma lehet. De intelligens észlelési hello diagnosztikai megfelelőek Önnek, nagy mennyiségű hello elemzés, amelyeket egyébként külön toodo saját kezűleg végrehajtása kezdődik. Eredményt el hello szépen csomagolása, így tooget gyorsan hello probléma toohello gyökérmappájában.

## <a name="how-it-works"></a>Működés
Intelligens észlelési figyeli az alkalmazásból, és az adott hello hiba díjszabás hello telemetriai adatokat. Ez a szabály melyik hello kérelmek száma hello száma `Successful request` tulajdonság értéke HAMIS, és melyik hello hello száma függőségi hívásokat `Successful call` tulajdonság értéke "false". A kéréseket, alapértelmezés szerint `Successful request == (resultCode < 400)` (kivéve, ha túl írt egyéni kód[szűrő](app-insights-api-filtering-sampling.md#filtering) , vagy a saját [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest) hívások). 

Az alkalmazás teljesítményének rendelkezik egy tipikus mintája működését. Néhány kérések vagy a függőségi hívásokra lesz jobban toofailure, mint a többire; és hello általános hibaaránya előfordulhat, hogy lépjen a terhelés növekedése. Intelligens észlelési használ a gépi tanulási toofind a rendellenességeket.

Telemetriai adatokat a webalkalmazás az Application Insightsban való származik, intelligens észlelési hello jelenlegi viselkedése hello keresztül látható az elmúlt néhány nap múlva hello mintákat hasonlítja össze. Ha egy szokatlan mértékben megnőtt a sikertelen korábbi teljesítmény összehasonlítva, elemzés működésbe lép.

Elemzés kiváltásakor hello szolgáltatást egy fürt analysis végez hello sikertelen kérelmek, tootry tooidentify hello hibák jellemző értékek egy mintát. Hello a fenti példában a hello elemzés észlelte, hogy a legtöbb hibák készül-e egy adott eredménykódja, a kérelem neve, a kiszolgáló URL-címe gazdagépet és a szerepkör példánya. Ezzel szemben hello analysis észlelte, hogy hello ügyfél operációs rendszer tulajdonsághoz több érték van elosztva, és ezért nem szerepel.

Ha a szolgáltatás a telemetriai adatok hívások van tagolva, hello elemző megkeresi a kivételt, és a kérelmek hello fürt állapított, együtt minden nyomkövetési napló társított azokat, például társított függőségi hiba kérelmek száma.

hello eredményül kapott elemzési ként legyen elküldve tooyou, riasztás, kivéve, ha nem konfigurálta.

Például a hello [riasztást manuálisan állítsa](app-insights-alerts.md), hello riasztás hello állapotának vizsgálata, és konfigurálja az Application Insights-erőforrás hello a riasztások panelen. De egyéb értesítések eltérően nem tooset be kell, vagy intelligens észlelési konfigurálása. Ha azt szeretné, tiltsa le, vagy módosítsa a cél e-mail címe.

## <a name="configure-alerts"></a>Riasztások konfigurálása
Letilthatja a intelligens észlelését, hello e-mailek címzettjeinek módosítása, a webhook létrehozása, vagy részt vevő toomore részletes üzenetek a riasztásra.

Nyissa meg a hello figyelmeztetések lapját. Hiba rendellenességeket megtalálható együtt minden riasztást, manuálisan van beállítva, és láthatja, hogy jelenleg hello figyelmeztetési állapotban van.

![Hello áttekintése lapon kattintson a riasztások csempén. Vagy bármely metrikák lapján kattintson a riasztások gombra.](./media/app-insights-proactive-failure-diagnostics/021.png)

Kattintson a riasztás tooconfigure hello azt.

![Konfiguráció](./media/app-insights-proactive-failure-diagnostics/032.png)

Figyelje meg, hogy az intelligens észlelési letilthatja, de nem törli (vagy hozzon létre egy újat).

#### <a name="detailed-alerts"></a>Részletes riasztások
Ha bejelöli a "Get részletesebb diagnosztikai" hello e-mail további diagnosztikai adatokat fogja tartalmazni. Egyes esetekben csak hello adatokból hello e-mailben képes toodiagnose hello probléma lesz.

Nincs enyhe kockázata, hogy részletesebb riasztás hello tartalmazhatnak bizalmas adatokat, mert a kivétel- és nyomkövetési üzeneteket tartalmaz. Azonban ez csak történne a kód lehetővé teheti a bizalmas információk be azokat az üzeneteket.

## <a name="triaging-and-diagnosing-an-alert"></a>Triaging és figyelmeztetés diagnosztizálásával
Egy riasztás azt jelzi, hogy szokatlan mértékben megnőtt a sikertelen kérések aránya hello észlelhető. Valószínű, hogy nincs-e az alkalmazás vagy a környezet kapcsolatos problémára.

A hello százalékos kérelmek és az érintett felhasználók számát mutatja, eldöntheti, hogyan sürgős hello probléma van. Hello a fenti példában hello sikertelenségének arányát 22,5 % 1 % a normál értéket összehasonlítja, azt jelzi, hogy valami rossz van folyamatban. A hello ugyanakkor, csak 11 felhasználók is hatással volt. Amennyiben az alkalmazást, akkor képes tooassess hogyan súlyos, amely lenne.

Sok esetben fogja tudni toodiagnose hello probléma gyorsan a hello kérés nevét, a kivétel megadott függőségi hiba és a nyomkövetési adatokat.

Léteznek bizonyos más keresik. Például hello függőségi hibaaránya ebben a példában az hello azonos a hello kivétel gyakorisága (89.3 %). Ez azt sugallja, hogy hello kivétel ered közvetlenül hello függőségi hiba - felkínálva egy tiszta meghatározni, hogy hol toostart keresése a kódban.

tooinvestigate további, hello egyes szakaszokban szereplő hivatkozásokkal egyenes tooa [keresőoldalt](app-insights-diagnostic-search.md) szűrt toohello vonatkozó kérések, a kivételt, a függőségekkel vagy a nyomkövetési adatokat. Vagy megnyithatja a hello [Azure-portálon](https://portal.azure.com), keresse meg az alkalmazás toohello Application Insights-erőforrás és hello hibák panel megnyitásához.

Ebben a példában a gombra kattintva hello "függőség hibák részleteinek megtekintése" a hivatkozás megnyitja hello Application Insights search paneljét. Azt mutatja, hogy hello SQL-utasításban, amely rendelkezik egy példát hello alapvető ok: NULL érték lett megadva a kötelező mezőket, és nem felelt meg az érvényesítési hello mentési művelet során.

![Diagnosztikai keresés](./media/app-insights-proactive-failure-diagnostics/051.png)

## <a name="review-recent-alerts"></a>Tekintse át a legújabb riasztások

Kattintson a **intelligens észlelési** tooget toohello legutóbbi riasztás:

![Riasztások szerint](./media/app-insights-proactive-failure-diagnostics/070.png)


## <a name="whats-hello-difference-"></a>Mi az különbség a hello...
Intelligens tegye a hiba rendellenességek észlelését más hasonló kiegészíti az Application Insights de különböző funkcióit.

* [Metrika riasztások](app-insights-alerts.md) -beállításokat, és figyelheti azokat a metrikák például CPU Foglaltság kérelem díjszabás, lapbetöltési idők vagy stb. Használhatja őket toowarn, például, ha tooadd több erőforrást igényelnek. Ezzel szemben intelligens tegye a hiba rendellenességek észlelését hozzá van rendelve egy kis számos fontos metrikák (jelenleg csak a sikertelen kérelmek aránya), a tervezett toonotify tooweb app képest, a közel valós idejű módon után jelentősen növeli a webalkalmazásban futó sikertelen kérések aránya Normál viselkedése.

    Intelligens észlelési automatikusan beállítja az küszöböt válasz tooprevailing feltételek.

    Intelligens észlelési hello diagnosztikai megfelelőek Önnek kezdődik.
* [A teljesítményanomáliákat észlelése intelligens](app-insights-proactive-performance-diagnostics.md) is használt számítógéphez eszközintelligencia toodiscover szokatlan minták a metrikákat, és Ön beállításokra nincs is szükség. De eltérően intelligens tegye a hiba rendellenességek észlelését, hello teljesítményanomáliákat intelligens észlelése célja a használat gyűjtőcső, előfordulhat, hogy rosszul szolgáltatott – például adott lap a böngészőben egy adott típusú toofind szegmensek. hello elemzés naponta történik, és az eredményt található, akkor valószínűleg toobe kevesebb sokkal sürgetőbb, mint a riasztást. Ezzel szemben a hiba rendellenességeket hello elemzés a bejövő telemetria folyamatosan történik, és értesítést fog kapni percen belül a vártnál nagyobb server hiba sebesség esetén.

## <a name="if-you-receive-a-smart-detection-alert"></a>Ha egy intelligens észleléséről szóló figyelmeztetés jelenik meg
*Miért megkapta a riasztást?*

* Szokatlan mértékben megnőtt a sikertelen kérelmek képest toohello normál alapterv az időszak megelőző hello észleltük. Az elemzést hello hibák és kapcsolódó telemetriai úgy tűnik, hogy probléma merül fel, hogy be kell keresnie.

*Hello értesítés jelent problémát mindenképpen rendelkezik?*

* Az alkalmazás megszakítása vagy teljesítménycsökkenése tooalert próbálja azt, de csak is teljes mértékben tisztában hello szemantikáját és hello gyakorolt hatása hello alkalmazás vagy a felhasználók.

*Igen guys megnézzük adataimat?*

* Nem. hello szolgáltatás egy teljesen automatikus. Csak hello értesítéseket kap. Az adatok [titkos](app-insights-data-retention-privacy.md).

*Toosubscribe toothis riasztás van?*

* Nem. Minden egyes alkalmazás, hogy a küld telemetriai kérelem hello intelligens észlelési riasztási szabályt tartalmaz.

*Leiratkozhat vagy toomy, munkatársakat helyette küldött hello értesítéseket?*

* Igen, a riasztási szabályok kattintson hello intelligens észlelési szabály tooconfigure azt. Hello értesítések letiltása vagy módosítása hello értesítés címzettjeit.

*Hello e-mail elvész. Hol találok hello értesítések hello portálon?*

* A hello tevékenységi naplóit. Az Azure nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz, majd válassza ki a tevékenységi naplóit.

*Néhány hello riasztásokat is ismert problémákról, és most nem kívánok tooreceive őket.*

* A várakozó fájlok számát a riasztás letiltását van.

## <a name="next-steps"></a>Következő lépések
A diagnosztikai eszközök segítségével vizsgálja meg az alkalmazásból hello telemetriai:

* [Metrika explorer](app-insights-metrics-explorer.md)
* [Keresési ablak](app-insights-diagnostic-search.md)
* [Elemzés - hatékony lekérdezési nyelv](app-insights-analytics-tour.md)

Intelligens észlelések rendszer teljesen automatikus. De lehet, hogy milyen tooset fel néhány további riasztások?

* [Manuálisan konfigurált metrika riasztások](app-insights-alerts.md)
* [A webteszt rendelkezésre állása](app-insights-monitor-web-app-availability.md)
