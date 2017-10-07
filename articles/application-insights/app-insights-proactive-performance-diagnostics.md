---
title: "Észlelési - teljesítményanomáliákat aaaSmart |} Microsoft Docs"
description: "Az Application Insights hajtja végre az alkalmazás telemetriai intelligens elemzése és figyelmezteti, potenciális problémákat. Ez a funkció a telepítés nem kell."
services: application-insights
documentationcenter: windows
author: antonfrMSFT
manager: carmonm
ms.assetid: 6acd41b9-fbf0-45b8-b83b-117e19062dd2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: bwren
ms.openlocfilehash: 60f10612188920330030129f7464e2f398ef1f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="smart-detection---performance-anomalies"></a>Intelligens észlelési - Teljesítményanomáliákat

[Az Application Insights](app-insights-overview.md) automatikusan elemzi a webes alkalmazás hello teljesítményét, és figyelmeztetést küld, azokról a potenciális problémákról. Akkor lehet, hogy lehet olvassa, mert az intelligens észlelési értesítést kapott.

Ehhez a szolgáltatáshoz nem speciális beállítási kivételével az alkalmazás konfigurálását az Application Insights (a [ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), vagy [Node.js](app-insights-nodejs.md), majd a [weblap kód](app-insights-javascript.md)). Azt akkor aktív, ha az alkalmazás elég telemetriai adatokat állít elő.

## <a name="when-would-i-get-a-smart-detection-notification"></a>Ha visszajelzést kap egy intelligens észlelési értesítést?

Az Application Insights azt észlelte, hogy az alkalmazás teljesítményének hello segít az alábbi módszerek valamelyikével:

* **Válasz idő teljesítménycsökkenést** – az alkalmazás elindult, a használt lassabban válaszol toorequests. hello módosítás gyors, például megváltoztak, mert hiba történt egy regressziós a legújabb környezetben. Vagy volna fokozatos, talán memóriavesztés okozza. 
* **Függőség időtartama teljesítménycsökkenést** – az alkalmazás lehetővé teszi a hívások tooa REST API-n, adatbázis vagy más függőséget. a használt lassabban hello függőségi válaszol.
* **Lassú teljesítmény mintát** – az alkalmazás megjelenik a toohave a teljesítmény ki, hogy csak bizonyos kérelmek érinti. Például lapok betöltött lassabban egy típus a böngésző, mint a többire; vagy a kérelmek lassabban által kiszolgált egy adott kiszolgálóhoz. Jelenleg az algoritmusok tekintse meg lapbetöltési idők kérelem válaszidejét és függőségi válaszidejét.  

Intelligens észlelési legalább 8 nap, sorrendben tooestablish normál alapteljesítményének a alkalmazható kötet telemetriai adatot igényel. Igen után az alkalmazás adott ideig futott, bármely jelentős probléma okozza értesítést.


## <a name="does-my-app-definitely-have-a-problem"></a>Saját alkalmazás mindenképpen rendelkezik probléma?

Nem, egy értesítés nem jelenti azt, hogy az alkalmazás véglegesen rendelkezik-e probléma. Akkor egyszerűen valami jobban a(z) toolook érdemes vonatkozó javaslat.

## <a name="how-do-i-fix-it"></a>Hogyan tegye megjavítani?

hello értesítők diagnosztikai adatokat. Íme egy példa:


![Íme egy példa a kiszolgáló válasza idő teljesítménycsökkenést észlelés](./media/app-insights-proactive-diagnostics/server_response_time_degradation.png)

1. **Osztályozás**. hello értesítési elsajátíthatja, hogy hány felhasználó vagy az érintett hány művelet. Ez segít prioritás toohello probléma hozzárendelése.
2. **Hatókör**. Hello probléma érinti az összes forgalom, vagy csak bizonyos lapok? Az informatikai korlátozott tooparticular böngészők vagy helyek? Ez az információ hello értesítési lehet lekérni.
3. **Diagnosztizálja**. Gyakran hello diagnosztikai adatokat hello értesítés javasol hello probléma hello jellegét. Például ha válaszidő lelassul, ha nagy a kérések aránya, javasolja a kiszolgáló vagy a függőségek túlterhelt. 

    Ellenkező esetben nyissa meg az Application Insightsban hello teljesítmény panelen. Itt megtalálja [Profilkészítő](app-insights-profiler.md) adatokat. Ha a kivételek, is megpróbálhatja hello [pillanatkép hibakereső](app-insights-snapshot-debugger.md).



## <a name="configure-email-notifications"></a>E-mail értesítések beállítása

Intelligens észlelési értesítések alapértelmezés szerint engedélyezve van, és rendelkező toothose küldött [tulajdonosait, a munkatársak és a olvasók hozzáférés toohello Application Insights-erőforrás](app-insights-resources-roles-access-control.md). toochange ez, vagy kattintson **konfigurálása** e-mailben értesítést hello, vagy nyissa meg az Application Insights intelligens észlelési beállítások. 
  
  ![Intelligens észlelési beállítások](./media/app-insights-proactive-diagnostics/smart_detection_configuration.png)
  
  * Használhatja a hello **leiratkozhat** hello intelligens észlelési e-mailek toostop hello értesítő e-mailjeire hivatkozásra.

E-mailek kapcsolatos intelligens észlelések teljesítményanomáliákat korlátozott tooone e-mail / nap / Application Insights-erőforrást. hello e-mailt küldünk csak, ha legalább egy új problémát észlelt az adott napon. Ismétlődik minden üzenet nem jelenik meg. 

## <a name="faq"></a>GYIK

* *Igen guys megnézzük adataimat?*
  * Nem. hello szolgáltatás egy teljesen automatikus. Csak hello értesítéseket kap. Az adatok [titkos](app-insights-data-retention-privacy.md).
* *Elemezze az Application Insights által gyűjtött összes hello adatokat?*
  * Jelenleg nem. Jelenleg a Microsoft elemzése kérelmek válaszideje, a függőségi válaszidő és a lap betöltési ideje. További metrikák elemzését a várakozó fájlok – Reméljük be van kapcsolva.

* Milyen típusú alkalmazás ez működik?
  * Ezek degradations bármely alkalmazás hozza létre a megfelelő telemetriai hello észlelt. Ha telepítette, akkor az Application Insights a webes alkalmazást, majd kérelmek és függőségei automatikusan követi. Azonban a háttér-szolgáltatások vagy alkalmazások, ha túl beszúrt hívások[TrackRequest()](app-insights-api-custom-events-metrics.md#trackrequest) vagy [TrackDependency](app-insights-api-custom-events-metrics.md#trackdependency), majd intelligens észlelési fog működni hello azonos módon.

* *Saját anomáliadetektálási észlelési szabályok létrehozása vagy meglévő szabályok testreszabása?*

  * Még nem, de a következőket teheti:
    * [Riasztások beállítása](app-insights-alerts.md) , amely közli, ha egy metrika keverve használ a küszöbértéket.
    * [Telemetriai adatok exportálása](app-insights-export-telemetry.md) tooa [adatbázis](app-insights-code-sample-export-sql-stream-analytics.md) vagy [tooPowerBI](app-insights-export-power-bi.md), ahol elemezhetjük magát.
* *Milyen gyakran történik a hello elemzés?*

  * A Microsoft futtassa a hello elemzés naponta hello telemetriai hello előző napi (UTC időzónában teljes nap).
* *Ezáltal ez a név felülírandó [metrika riasztások](app-insights-alerts.md)?*
  * Nem.  Jelenleg nem véglegesíthető toodetecting minden, érdemes lehet rendellenes viselkedés.


* *Ha nem tesz semmit, a válasz tooa értesítési, I kap emlékeztető?*
  * Nem, egy üzenetet kap minden problémával kapcsolatos csak egyszer. Ha hello a probléma továbbra is fennáll, az intelligens észlelési hírcsatorna panel hello frissíti.
* *Hello e-mail elvész. Hol találok hello értesítések hello portálon?*
  * A hello Application Insights – áttekintés az alkalmazás, kattintson a hello **intelligens észlelési** csempére. Nem lesz képes toofind too90 nap fel minden értesítést biztonsági.

## <a name="how-can-i-improve-performance"></a>Hogyan javíthatja a teljesítményt?
Lassú és sikertelen válaszok rendszer egyik hello legnagyobb frustrations webhely számára a saját tapasztalatunkat is tudja módon. Ezért fontos tooaddress hello problémák.

### <a name="triage"></a>Osztályozás
Első lépésként számít? Ha egy lap mindig lassú tooload, de csak 1 %-át a hely felhasználóinak legalább egyszer állandóan toolook azt, lehet, hogy van több fontos dolgok toothink kapcsolatban. A hello ugyanakkor, ha csak a felhasználók 1 % megnyitásához, de lehet, hogy érdemes vizsgál, minden egyes jelez kivételt.

Általános útmutatóként hello hatás utasítás (érintett felhasználók vagy forgalom %) használható, de vegye figyelembe, hogy nincs-e hello lényeg. Gyűjtse össze a többi bizonyító adatok tooconfirm.

Vegye figyelembe a hello probléma hello paramétereit. Ha a földrajzi-függő, állítsa be [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) többek között az adott régióban: egyszerűen hálózati problémák adódhatnak az adott területre.

### <a name="diagnose-slow-page-loads"></a>Lassúlap terhelések diagnosztizálása
Hol található hello probléma? Hello server lassú toorespond, hello lap nagyon hosszú, vagy nem hello böngésző rendelkezik toodo munkahelyi toodisplay számos azt?

Hello böngészők metrika panel megnyitásához. hello szegmentált böngésző lap betöltési ideje mutat be ahol hello idő állapotra vált, megjelenítését. 

* Ha **küldési kérelem ideje** értéke magas, vagy hello kiszolgáló válaszol lassan, vagy hello kérelme, mert az egy post a nagy mennyiségű adat. Nézze meg hello [teljesítménymutatók](app-insights-web-monitor-performance.md#metrics) tooinvestigate válaszidejét.
* Állítson be [követési függőségi](app-insights-asp-net-dependencies.md) toosee hello lassúsága megfelelő tooexternal services vagy az adatbázis van-e.
* Ha **válasz fogadása** elsődleges, a lap és a függő részek - JavaScript, CSS, és így tovább (de nem aszinkron módon betöltött adatokról) lemezképek hosszúak. Állítson be egy [elérhetőségi teszt](app-insights-monitor-web-app-availability.md), és lehet, hogy tooset hello beállítás tooload függő részek. Amikor bizonyos adatokat, nyissa meg az eredményt hello részletei és toosee hello bontsa ki másik fájljainak betöltése.
* Magas **ügyfél feldolgozási idejének** lassan futnak a parancsfájlok javasol. Ha hello oka nem egyértelmű, fontolja meg néhány időzítési kódot, és küldjön hello alkalommal trackMetric hívások.

### <a name="improve-slow-pages"></a>Lassú lapok javítása
Nincs olyan teljes tanácsadás javítása a kiszolgáló válaszainak és lapbetöltési idők, ezért többször nem fognak toorepeat a webhely összes itt. Néhány tipp kapcsolatos csak tooget valószínűleg már tudja, hogy végezni:

* Lassú betöltése nagy fájlok miatt: hello parancsfájlok és egyéb részeinek aszinkron módon betöltése. Ezen parancsfájl kötegelése. Hello főoldala felosztása widgeteket, amely az adatok külön-külön betöltése. Ne küldjön egyszerű régi HTML hosszú táblázatok: egy parancsfájl toorequest hello adatokat használjon JSON vagy más kompakt formátumú, majd töltse ki a hello tábla helyen. Nincsenek minden ennek kiváló keretrendszerek toohelp. (Ezek maguk után nagy parancsfájlok természetesen.)
* Lassú server függőségek: fontolja meg a hello az összetevők földrajzi elhelyezkedését. Például Azure használata, győződjön meg arról, hello hello webkiszolgáló és a hello adatbázis is ugyanabban a régióban. Hajtsa végre a lekérdezések több információt kell olvassák be? Gyorsítótárazás vagy súgó kötegelés volna?
* A kapacitás problémák: hello kiszolgálói metrikák válaszidejének tekintse meg, és a kérelmek számát. Ha válaszidők aránytalanul csúcsait ügyfélkérelmek számát az a maximális, valószínű, hogy a kiszolgálók úgy módosítja a program.


## <a name="server-response-time-degradation"></a>Kiszolgáló válasza idő teljesítménycsökkenése

hello válasz idő teljesítménycsökkenést értesítés, miszerint:

* hello válaszidő képest toonormal válaszidő ehhez a művelethez.
* Érintett felhasználók számát.
* Átlagos válaszidő és 90 PERCENTILIS válaszidő ehhez a művelethez hello napon hello észlelési és előtt 7 nap. 
* Ez a művelet száma kérelmek hello hello észlelés és 7 nap előtt.
* Ebben a műveletben teljesítménycsökkenést és a kapcsolódó függőségek degradations közötti korreláció. 
* Hivatkozások toohelp hello probléma diagnosztizálása.
  * Profilkészítő hívásláncainak megtekintheti az adott művelet töltött idő, az toohelp (hello mutató hivatkozás áll rendelkezésre, ha ehhez a művelethez hello észlelési időszak során összegyűjtött Profilkészítő nyomkövetési példák). 
  * Teljesítmény-jelentések a metrika Intézőben, ahol Ön is részletekbe menően idő tartományszűrő ehhez a művelethez.
  * Keresse meg a tooview meghívja az adott hívások tulajdonságok.
  * Hiba – Ha jelent ez azt jelenti, hogy hiba történt a művelet, előfordulhat, hogy kívánják tooperformance teljesítménycsökkenést > 1 száma.

## <a name="dependency-duration-degradation"></a>A függőségi időtartama teljesítménycsökkenése

A modern alkalmazásnak nagyobb micro szolgáltatások tervezett módszert alkalmaz, amely sok esetben tooheavy megbízhatóság külső szolgáltatások fogad el. Például, ha az alkalmazás hipervizorplatformra támaszkodva bizonyos adatokat, vagy ha a készít a saját botot szolgáltatás akkor lesz valószínűleg továbbítják az egyes kognitív szolgáltatások szolgáltató tooenable a botok toointeract több emberi módon és néhány adat tárolására botot toopull hello szolgáltatást a válaszokat.  

Példa függőségi teljesítménycsökkenést értesítés:

![Függőség időtartama teljesítménycsökkenést észlelés példa](./media/app-insights-proactive-diagnostics/dependency_duration_degradation.png)

Figyelje meg, amely jelzi, hogy:

* hello időtartama képest toonormal válaszidő ehhez a művelethez
* Hány felhasználó érintett
* Átlagos és 90 PERCENTILIS időtartama a függőség a hello hello észlelés és 7 nap előtt
* A függőségi száma meghívja a hello hello észlelés és 7 nap előtt
* Hivatkozások toohelp hello probléma diagnosztizálása
  * Teljesítmény jelentéseinek a függőség metrika Explorerben
  * Keresse meg a függőségi hívások tooview hívások tulajdonságai
  * Hiba jelentések – Ha ezt az átlagos, hogy történtek-e sikertelen függőségi hívások során száma > 1 hello észlelési időszakban, előfordulhat, hogy kívánják tooduration teljesítménycsökkenést. 
  * Nyissa meg a függőségi időtartamát és a count számító lekérdezéseket elemzés  

## <a name="smart-detection-of-slow-performing-patterns"></a>A lassú teljesítő minták intelligens észlelése 

Az Application Insights megkeresése, amelyek csak hatással vannak a felhasználók egy részét, vagy csak az egyes esetekben felhasználókat érintő teljesítményproblémákat. Például a lapok betöltési értesítést más típusú böngészők, mint a böngésző egyik típusú slowler, vagy ha a kérések lassabban rendelkezésre az adott kiszolgálóhoz. Feltérképezésére is alkalmas kombinációk, kapcsolódó problémák, mint például adott operációs rendszert használó ügyfelek számára egy földrajzi terület lassúlap tölti be.  

Hasonló rendellenességeket nagyon nehéz toodetect csak hello adatok vizsgálatával, de több közös, mint érdemes. Gyakran azok csak felület, amikor a felhasználók panaszkodnak mert. Addigra már túl későn: hello érintett felhasználók már vált tooyour versenytársak!

Jelenleg az algoritmusok tekintse meg lapbetöltési idők, a kérelem válaszidejének hello kiszolgálón és a függőségi válaszidejét.  

Nem rendelkezik tooset bármely küszöbértékek vagy szabályok konfigurálása. Gépi tanulás és adatbányászati algoritmusokat már nem használt toodetect rendellenes minták.

![Az üdvözlő e-mail riasztást kattintson a hello hivatkozás tooopen hello diagnosztikai jelentés az Azure-ban](./media/app-insights-proactive-performance-diagnostics/03.png)

* **Amikor** hello idő hello problémát észlelt mutatja.
* **Mi** ismerteti:

  * hello problémát észlelt;
  * hello hello jellemzői készlet eseményeket, amelyek észleltünk a hello probléma viselkedés jelenik meg.
* hello táblázat hello rosszul működő készlet az összes esemény átlagos viselkedését hello hasonlítja össze.

Kattintson a hello hivatkozások tooopen metrika Explorer, és keresse a megfelelő jelentésekben hello idő-és tulajdonságainak hello lassú set végrehajtása szűrt.

Módosítsa a hello idő tartomány és a szűrők tooexplore hello telemetriai adatokat.

## <a name="next-steps"></a>Következő lépések
A diagnosztikai eszközök segítségével vizsgálja meg az alkalmazásból hello telemetriai:

* [Profilkészítő](app-insights-profiler.md) 
* [Pillanatkép hibakereső](app-insights-snapshot-debugger.md)
* [Elemzés](app-insights-analytics-tour.md)
* [Elemzés intelligens diagnosztika](app-insights-analytics-diagnostics.md)

Intelligens észlelések rendszer teljesen automatikus. De lehet, hogy milyen tooset fel néhány további riasztások?

* [Manuálisan konfigurált metrika riasztások](app-insights-alerts.md)
* [A webteszt rendelkezésre állása](app-insights-monitor-web-app-availability.md)
