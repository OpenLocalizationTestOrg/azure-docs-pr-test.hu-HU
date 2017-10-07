---
title: "aaaMonitor az alkalmazás állapotának és használatának az Application insights szolgáltatással"
description: "Ismerkedés az Application insights szolgáltatással. Használati, rendelkezésre állásának és a helyszíni vagy a Microsoft Azure-alkalmazások teljesítményének elemzése."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/25/2015
ms.author: bwren
ms.openlocfilehash: 9230a6e65e5afb00c36122ff1d1de01ba19cd7f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-in-web-applications"></a>Webalkalmazások teljesítményének monitorozása


Győződjön meg arról, hogy az alkalmazás teljesítménye, és tájékozódhat a gyorsan esetleges hibákat. [Az Application Insights] [ start] adja meg a teljesítménybeli problémák és kivételek, és keresse meg és diagnosztizálásához hello súgó kiváltó okok.

Az Application Insights figyelheti a Java és az ASP.NET webalkalmazások és szolgáltatások, WCF-szolgáltatások. Ezek lehetnek üzemeltethető a helyszínen, a virtuális gépek, illetve a Microsoft Azure-webhelyekre. 

Hello ügyféloldalon az Application Insights telemetria a weblapok és az eszközt, beleértve az iOS, Android és Windows Áruházbeli alkalmazások számos vehet igénybe.

>[!Note]
> Hajtottunk egy új felhasználói élmény keresése lassú lapok végrehajtása a webalkalmazásban érhető el. Ha nem rendelkezik hozzáféréssel tooit, engedélyezheti a hello a kép beállításainak konfigurálásával [előnézeti panelen](app-insights-previews.md). Olvassa el az új felhasználói élmény a [keresse meg és hárítsa el szűk keresztmetszetek hello interaktív teljesítményt vizsgálat](#Find-and-fix-performance-bottlenecks-with-an-interactive-Performance-investigation).

## <a name="setup"></a>Teljesítmény figyelése
Ha még nem vett Application Insights tooyour (Ez azt jelenti, ha nincs beállítva a ApplicationInsights.config) projektre, válassza ki az alábbi módszerek valamelyikével tooget lépések:

* [ASP.NET-webalkalmazások](app-insights-asp-net.md)
  * [Kivételfigyelés hozzáadása](app-insights-asp-net-exceptions.md)
  * [A függőségi figyelés felvétele](app-insights-monitor-performance-live-website-now.md)
* [J2EE webalkalmazások](app-insights-java-get-started.md)
  * [A függőségi figyelés felvétele](app-insights-java-agent.md)

## <a name="view"></a>Teljesítménymértékeket felfedezése
A [hello Azure-portálon](https://portal.azure.com), keresse meg az alkalmazáshoz beállított toohello Application Insights-erőforrást. hello áttekintése panel alapvető teljesítményadatait jeleníti meg:

Kattintson a bármely diagram toosee további információkhoz juthat, és toosee eredmények hosszabb ideig. Kattintson például a hello kérések csempe, és válassza a időtartomány:

![Kattintson a toomore adatok között, és válasszon egy időtartományt](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Kattintson a diagram toochoose mely metrikák azt jeleníti meg, vagy új diagram hozzáadása, és válassza ki a metrikák:

![Kattintson egy grafikonon toochoose metrikák](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Törölje az összes hello metrikát** toosee hello teljes kijelölés elérhető. hello metrikák sorolhatók csoportok; Ha egy csoport minden tagja van kijelölve, csak hello csoport többi tagjára jelennek meg.

## <a name="metrics"></a>Mire azt minden középérték? Teljesítmény csempék és jelentések
Nincsenek is elérhetővé teljesítménymutatók. Kezdjük az alábbiakhoz alapértelmezés szerint a hello alkalmazás panelen jelennek meg.

### <a name="requests"></a>Kérelmek
egy megadott időszakban fogadott HTTP-kérelmek száma hello. Hasonlítsa össze a más jelentések toosee, hogyan viselkedik az alkalmazás, mivel változik a terhelés hello hello eredményt.

HTTP-kérések összes GET vagy POST kérelem lapok, adatok és lemezképek tartalmazzák.

Kattintson a hello csempe tooget érintett adott URL-címek.

### <a name="average-response-time"></a>Átlagos válaszidő
Intézkedések hello között eltelt idő egy webes kéréssel, írja be az alkalmazás- és hello választ ad vissza.

hello pontok megjelenítése mozgó átlagos. Ha nagy mennyiségű kérést, előfordulhat, egyes hello átlagos nélkül egy nyilvánvaló csúcs eltérnek vagy hello graph mártsuk.

Szokatlan csúcsait keresi. Általában várt válasz idő toorise rendelkező kérelmek növekedését. Ha hello megnövekedhet le aránytalanul nagy, előfordulhat, hogy az alkalmazás elérés például a Processzor- vagy hello kapacitás használ szolgáltatás erőforrás korlátozni.

Kattintson a hello csempe tooget többször megadott URL-címek.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>A leghosszabb kérelmek
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Jeleníti meg, mely kérelmek teljesítményhangolás kell.

### <a name="failed-requests"></a>Sikertelen kérelmek
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Kérelmek kivételt váltott ki a nem kezelt kivételek számát.

Kattintson a hello csempe toosee hello részleteit vonatkozó hibákat, és válassza ki az egyes toosee a részletek. 

Egyéni ellenőrzési hibák csak egy reprezentatív minta megmarad.

### <a name="other-metrics"></a>Más metrikákkal
toosee milyen más metrikákkal jeleníti meg, kattintson egy grafikonon, és törölje a jelölést minden hello metrikák toosee hello teljes elérhető beállítás. Kattintson (i) toosee mindegyik metrikát definíciója.

![Kijelölésének megszüntetése az összes metrikák toosee hello teljes](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

A metrika letiltja a hello kiválasztása mások számára, amelyek nem szerepelnek a hello ugyanabban a diagramban.

## <a name="set-alerts"></a>Riasztások beállítása
rendkívüli értékek a metrika az e-mailben értesítést toobe adja hozzá a riasztást. Kiválaszthatja a toosend hello e-mail toohello a rendszergazdák vagy toospecific e-mail címet.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Mielőtt hello hello erőforrás más tulajdonságainak beállítása. Ne válassza ki a hello webtesztben erőforrásokat, ha azt szeretné, hogy a teljesítmény vagy a szoftverhasználati mérési adatok tooset riasztásokat.

Lehet, amelyben kéri tooenter hello küszöbérték gondos toonote hello egység.

*Hello Hozzáadás riasztási gomb nem látható.* -Van ez a csoport fiók toowhich csak olvasási hozzáféréssel rendelkezik? Kérje meg a fiókadminisztrátort hello.

## <a name="diagnosis"></a>Problémák diagnosztizálása
Az alábbiakban néhány tippek megkereséséhez és teljesítménnyel kapcsolatos problémák diagnosztizálásához használható:

* Állítson be [webalkalmazás-tesztek] [ availability] toobe riasztást kapni, ha a webhely nem működik, vagy helytelenül vagy lassan válaszol. 
* Hasonlítsa össze a más metrikák toosee hello kérelmek száma, ha sikertelen vagy lassú válasz a kapcsolódó tooload.
* [INSERT, és nyomkövetési utasítások keresési] [ diagnostic] a kód toohelp a rögzítési ponthoz problémákat.
* A webalkalmazás műveletet a figyelheti [metrikák adatfolyamot][livestream].
* A .net alkalmazás hello állapot rögzítése [pillanatkép hibakereső][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-an-interactive-performance-investigation"></a>Keresse meg és hárítsa el szűk keresztmetszetek interaktív teljesítményt vizsgálat

Hello új Application Insights interaktív teljesítményt vizsgálat toolocate területeit webalkalmazás lassítja általános teljesítménye is használhatja. Is gyorsan keresés adott lapok, amelyek lassítja, és használja a hello [profilkészítési eszköz](app-insights-profiler.md) toosee, ha az adott lapok közötti összefüggés.

### <a name="create-a-list-of-slow-performing-pages"></a>Lassú teljesítő oldalak listájának létrehozása 

hello első lépése a teljesítményproblémák kereséshez tooget hello lassan válaszol a lapok listája. alább hello képernyőfelvétel mutatja be, hello teljesítmény panel tooget lehetséges oldalak tooinvestigate további listájának használatával. Gyorsan megtekintheti ezen a lapon, hogy történt egy slow-down hello válaszidő hello alkalmazás körülbelül 6:00 PM és újra körülbelül 10 óra. Láthatja, hogy hello ügyfél-részletek művelet volt, néhány hosszú futású műveleteket és a egy közepes válaszidő 507.05 ideje (MS). 

![Alkalmazásteljesítmény elemzések interaktív](./media/app-insights-web-monitor-performance/performance1.png)

### <a name="drill-down-on-specific-pages"></a>Bizonyos lapokon részletekbe menően tárhatják

Miután az alkalmazás teljesítményének pillanatképet, kaphat további részleteket a lassú végez műveleteket. Kattintson az összes műveletet hello lista toosee hello részletei alább látható módon. Hello diagram láthatja, ha hello teljesítmény a függőség alapszik. Hány felhasználó tapasztalt hello különböző válaszidejét is látható. 

![Application Insights műveletek panel](./media/app-insights-web-monitor-performance/performance5.png)

### <a name="drill-down-on-a-specific-time-period"></a>Egy adott időszakra vonatkozóan a részletekbe menően tárhatják

Ha azonosított egy adott idő tooinvestigate ponthoz, részletekbe menően tárhatják hello hello teljesítmény slow-down oka lehet, hogy bizonyos műveletek, akár további toolook. Az adott időben kattint hello részleteit hello oldalon lent látható módon nyílik meg. Hello az alábbi példa látható egy adott időszakban, valamint hello server válaszkódot és hello művelet időtartama felsorolt hello műveletek. A TFS munkaelem megnyitása, ha az információ tooyour fejlesztői csapat toosend szüksége van hello URL-címét is.

![Application Insights időszelet](./media/app-insights-web-monitor-performance/performance2.png)

### <a name="drill-down-on-a-specific-operation"></a>Egy bizonyos művelet részletekbe menően tárhatják

Ha azonosított egy adott idő tooinvestigate ponthoz, részletekbe menően tárhatják hello hello teljesítmény slow-down oka lehet, hogy bizonyos műveletek, akár további toolook. Kattintson az egy művelet hello lista toosee hello adatait hello művelet alább látható módon. Ebben a példában láthatja, hogy hello művelet végrehajtása sikertelen volt, és az Application Insights nyújtott hello hello részleteit kivétel hello alkalmazás kivételt váltott ki. Ezen a panelen újra, a TFS munkaelem könnyedén létrehozhat.

![Application Insights művelet panel](./media/app-insights-web-monitor-performance/performance3.png)


## <a name="next"></a>Következő lépések
[Webalkalmazás-tesztek] [ availability] -webes kérelmek elküldött tooyour alkalmazás hello világ a rendszeres időközönként.

[Rögzítése, és diagnosztikai nyomkövetési keresési] [ diagnostic] - nyomkövetési hívások beszúrása és hello eredmények toopinpoint problémák keletkezik.

[Használat nyomon követése] [ usage] -megtudhatja, hogyan személyek használhassa az alkalmazást.

[Hibaelhárítási] [ qna] - és a kérdések és válaszok



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



