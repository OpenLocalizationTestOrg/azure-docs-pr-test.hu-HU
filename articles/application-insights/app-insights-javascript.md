---
title: "a JavaScript aaaAzure Application Insights webalkalmazás-alkalmazások |} Microsoft Docs"
description: "Lekérheti a lapmegtekintések és a munkamenetek számát, a webes ügyfél adatait, és nyomon követheti a használati mintákat. Kivételeket és teljesítményproblémákat észlelhet a JavaScript weblapokon."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 986db3c3776471f9f8556f4e09f2d02aad022549
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-web-pages"></a>Application Insights weblapokhoz
Tájékozódhat hello teljesítmény és a weblap vagy az alkalmazás használatát. Ha ad hozzá [Application Insights](app-insights-overview.md) tooyour lap parancsfájl, kapott időzítés lap terhelések és AJAX hívások, számok és böngésző kivételeket és a AJAX hibák, valamint a felhasználók és a munkamenet számok részleteit. Ezek mindegyike szegmentálható lap, ügyfél operációs rendszere és böngészőverziója, földrajzi hely és más dimenziók alapján. Beállíthat riasztásokat a hibaszámokról és a lassú lapbetöltésekről. És a JavaScript-kód nyomkövetési hívások beszúrásával nyomon követheti a weblap alkalmazás különböző funkcióit hello használata.

Az Application Insights bármely weblappal használható – csak egy rövid JavaScript-kódot kell hozzáadnia. Ha a webszolgáltatása [Java](app-insights-java-get-started.md) vagy [ASP.NET](app-insights-asp-net.md), telemetriát integrálhat a kiszolgálóról és az ügyfelekről.

![A portal.azure.com címen nyissa meg az alkalmazás erőforrását, és kattintson a Böngésző elemre](./media/app-insights-javascript/03.png)

Előfizetés túl kell[Microsoft Azure](https://azure.com). Ha olyan szervezeti előfizetéssel rendelkezik, kérje a Microsoft Account tooit hello tulajdonos tooadd. A fejlesztés és a kis mértékű használat nem kerül semmibe.

## <a name="set-up-application-insights-for-your-web-page"></a>Az Application Insights beállítása a weboldalához
Adjon hozzá hello betöltő kód részlet tooyour weblapok, az alábbiak szerint.

### <a name="open-or-create-application-insights-resource"></a>Application Insights-erőforrás megnyitása vagy létrehozása
hello Application Insights-erőforrás, ahol megjelenik a lapon teljesítmény- és használati adatait. 

Jelentkezzen be az [Azure Portalra](https://portal.azure.com).

Ha már beállította az alkalmazás hello kiszolgálóoldali figyelést, már rendelkezik egy erőforrás:

![Válassza a Tallózás, Fejlesztői szolgáltatások, Application Insights lehetőséget.](./media/app-insights-javascript/01-find.png)

Ha nincs erőforrása, hozza létre azt:

![Válassza az Új, Fejlesztői szolgáltatások, Application Insights lehetőséget.](./media/app-insights-javascript/01-create.png)

*Máris kérdései vannak?* [További információk erőforrások létrehozásáról](app-insights-create-new-resource.md).

### <a name="add-hello-sdk-script-tooyour-app-or-web-pages"></a>Hello SDK parancsfájl tooyour alkalmazás vagy a weblapok hozzáadása
A gyors üzembe helyezés hello parancsfájl weblapok érhető el:

![Az alkalmazás áttekintése paneljén válassza a gyors üzembe helyezés, majd kód toomonitor a weblapokat. Másolja a hello parancsfájlt.](./media/app-insights-javascript/02-monitor-web-page.png)

Hello előtt hello parancsprogram beszúrása `</head>` címke tootrack kívánt minden oldalon. Ha a webhely mesterlapra, nem adhat meg hello parancsfájl. Példa:

* Egy ASP.NET MVC-projektben a következő helyre helyezné a szkriptet: `View\Shared\_Layout.cshtml`
* Nyissa meg a SharePoint-webhely hello Vezérlőpultján, [helybeállításokat / mesterlap](app-insights-sharepoint.md).

hello hello instrumentation kulcs, amely arra utasítja a hello adatok tooyour Application Insights-erőforrást tartalmaz. 

([Hello parancsfájl ismerteti. ](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))

*(Ha jól ismert weblapkeretrendszert használ, keressen Application Insights-adaptereket. Például van [egy AngularJS modul](http://ngmodules.org/modules/angular-appinsights).)*

## <a name="detailed-configuration"></a>Részletes konfiguráció
Több [paramétert](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) beállíthat, de a legtöbb esetben erre nincs szükség. Például tiltsa le vagy hello / lapmegtekintés (tooreduce forgalom) jelentett Ajax-hívások száma. Vagy a hibakeresési mód toohave telemetriai áthelyezés gyorsan hello-feldolgozási folyamaton keresztül nélkül kötegelni alatt állíthatja be.

tooset ezeket a paramétereket, keresse meg a sort a hello kódrészletet, és adja meg vesszővel tagolt több elemet:

    })({
      instrumentationKey: "..."
      // Insert here
    });

Hello [felhasználható paraméterek](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) tartalmazza:

    // Send telemetry immediately without batching.
    // Remember tooremove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, tooreduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up tooexecution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <a name="run"></a>Az alkalmazás futtatása
A webes alkalmazás futtatását, használja a toogenerate telemetriai adatokat, és várjon egy pár másodpercen közben. Továbbra is fut hello segítségével **F5** billentyűt a fejlesztői számítógépén vagy közzététele és teheti a felhasználóknak össze azt.

Ha azt szeretné, hogy a webes alkalmazás küld tooApplication Insights toocheck hello telemetriai, a böngésző hibakeresési eszközök használata (**F12** sok böngészőkben). Toodc.services.visualstudio.com adatokat küldi el.

## <a name="explore-your-browser-performance-data"></a>A böngésző teljesítményadatainak feltárása
Megnyitás hello böngésző panel tooshow összesíteni a teljesítményadatokat a felhasználók böngészőjének.

![A portal.azure.com címen nyissa meg az alkalmazás erőforrását, és kattintson a Beállítások, Böngésző lehetőségre](./media/app-insights-javascript/03.png)

*Még nincsenek adatok? Kattintson a **frissítése** hello oldal hello tetején. Még mindig semmi? Lásd: [Hibaelhárítás](app-insights-troubleshoot-faq.md).*

hello böngésző panel van egy [Metrikaböngésző panel](app-insights-metrics-explorer.md) előre beállított szűrők és diagram beállításokat. Ha szeretné, és menti a hello eredmény kedvencként szerkesztheti hello időtartományt, szűrők és diagram konfigurációs. Kattintson a **visszaállítása** tooget hátsó toohello eredeti panel konfigurációt.

## <a name="page-load-performance"></a>Lapbetöltés teljesítménye
Hello felső a szegmentált lapbetöltési idők-diagram. hello hello diagram magassága az alkalmazást a a felhasználók böngészőjének hello átlagos idő tooload és megjelenítendő lapok jelöli. hello idő mérése a hello böngésző hello kezdeti HTTP-kérelem küld, amíg az összes szinkron betöltési események dolgozott, beleértve a elrendezés és a parancsfájlok futtatását. Ebben nem tartoznak bele az aszinkron feladatok, például a kijelzők betöltése az AJAX hívásokból.

hello diagram szegmensek hello összes lap betöltési ideje az hello [W3C határozza szabvány időzítés](http://www.w3.org/TR/navigation-timing/#processing-model). 

![](./media/app-insights-javascript/08-client-split.png)

Vegye figyelembe, hogy hello *hálózati csatlakozás* idő gyakran alacsonyabb, mint a várt, akkor az átlagos keresztül elérhető hello toohello összes kérelem. Sok egyes kérelmeket 0 connect időt igénybe, mert már van egy aktív kapcsolat toohello kiszolgáló.

### <a name="slow-loading"></a>Lassú a betöltés?
A lassú lapbetöltések a felhasználók elégedetlenségének fő forrásai. Hello diagram lassúlap betölti azt jelzi, ha a rendszer egyszerű toodo néhány diagnosztikai kutatási.

hello a diagram összes lap terhelések hello átlaga mutatja az alkalmazás. Ha hello probléma toosee korlátozódik tooparticular lapok, megjelenését további hello panel le, ahol a rács által szegmentált lap URL-cím:

![](./media/app-insights-javascript/09-page-perf.png)

Figyelje meg hello nézet oldalszám és a szórást. Ha hello oldalszám nagyon alacsony, majd hello a probléma nem befolyásolja a felhasználók sokkal. A magas szórás (a saját magát összehasonlítható toohello átlagos) azt jelzi, hogy nagy mennyiségű egyedi mérések közötti eltérés.

**Nagyítsa ki az egyik URL-címet és az egyik lapmegtekintést.** Kattintson a lap neve toosee egy panel böngésző diagramok szűrt csak toothat URL-címének; majd a nézet egy példánya.

![](./media/app-insights-javascript/35.png)

Kattintson a `...` , hogy az esemény tulajdonságainak teljes listáját, vagy vizsgálja meg a hello Ajax-hívások, illetve a kapcsolódó eseményeket. Lassú Ajax-hívások hatással hello betöltési ideje Általános lapon, ha azok szinkron. Kapcsolódó események tartalmazzák a hello kiszolgálói kérelmek azonos URL-CÍMMEL (Ha a webkiszolgálón állított be az Application Insights).

**Lapteljesítmény az idő függvényében.** Eközben hello böngészők panelen átalakítása hello lapmegtekintés betöltési ideje rács egy sor diagram toosee Ha csúcsait adott időpontokban:

![Kattintson a hello head hello rács, és egy új diagram típusának kiválasztása](./media/app-insights-javascript/10-page-perf-area.png)

**Szegmentáljon más dimenziók alapján.** Lehet, hogy a lapok lassabb tooload szerepelnek a böngésző, az ügyfél operációs rendszer vagy a felhasználó adott nyelvterület? Új diagram hozzáadása és hello kísérletezhet **csoportosítási** dimenzió.

![](./media/app-insights-javascript/21.png)

## <a name="ajax-performance"></a>AJAX teljesítménye
Győződjön meg arról, hogy a weblapjain lévő összes AJAX hívás teljesítménye megfelelő. Ezek gyakran használt toofill részét képezik a lap aszinkron módon történik. Bár hello Általános lap töltődik be azonnal, a felhasználók sikerült kell további javulás is meghiúsult: üres a kijelzők indítás őket az adatok tooappear vár.

A weblap AJAX-hívások szerint függőségeinek hello böngészők panelen jelennek meg.

Nincsenek összefoglaló diagramok hello hello panel felső részén:

![](./media/app-insights-javascript/31.png)

és részletes rácsok lejjebb:

![](./media/app-insights-javascript/33.png)

Kattintson valamelyik sorra a részletekért.

> [!NOTE]
> Ha törli a hello böngészők szűrő hello panelen, a kiszolgáló és az AJAX-függőségek szerepelnek a diagramokat. Kattintson az alapértelmezett értékek visszaállítása tooreconfigure hello szűrő.
> 
> 

**a sikertelen Ajax-hívások toodrill** görgessen lefelé toohello függőségi hibák rács, és kattintson az egy sor toosee olyan specifikus példányai.

![](./media/app-insights-javascript/37.png)


Kattintson a `...` az Ajax-hívások hello teljes telemetriai adatokat.

### <a name="no-ajax-calls-reported"></a>Nincsenek Ajax hívások jelentve?
AJAX-hívások bármely hello parancsfájlt a weblapot HTTP/HTTPS hívásoknak tartalmazza. Ha nem látja ezeket a jelentett, ellenőrizze, hogy hello kódrészletet nem állítja be a hello `disableAjaxTracking` vagy `maxAjaxCallsPerView` [paraméterek](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="browser-exceptions"></a>Böngészőkivételek
Hello böngészők paneljén van egy kivételek összegző diagram, és a kivétel típusok további hello panel le.

![](./media/app-insights-javascript/39.png)

Böngésző kivételek jelentett nem látható, ha ellenőrizze, hogy hello kódrészletet nem állítja be a hello `disableExceptionTracking` [paraméter](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config).

## <a name="inspect-individual-page-view-events"></a>Egyéni lapmegtekintési események vizsgálata

Általában az Application Insights elemzi a lapmegtekintési telemetriát, és Ön csak összegző jelentéseket tekinthet meg, amelyek az összes felhasználó átlagát tartalmazzák. De a hibakereséshez az egyes lapmegtekintési eseményeket is megtekintheti.

Hello diagnosztikai Search paneljét állítson be szűrőt tooPage nézet.

![](./media/app-insights-javascript/12-search-pages.png)

Válassza ki a bármely esemény toosee további információkhoz juthat. Hello részleteit megjelenítő oldalon kattintson "..." toosee is további információkhoz juthat.

> [!NOTE]
> Ha [keresési](app-insights-diagnostic-search.md), figyelje meg, hogy rendelkezik-e toomatch teljes szó: "Abou" és "vebben" nem egyeznek meg "Kapcsolatos".
> 
> 

Is használhatja hello hatékony [Log Analytics lekérdezési nyelv](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour#browser-timings-table) toosearch lapmegtekintéseiről.

### <a name="page-view-properties"></a>Lapmegtekintési tulajdonságok
* **Lapmegtekintés időtartama** 
  
  * Alapértelmezés szerint hello alkalommal vesz tooload hello lap, és az ügyfél kérelem toofull töltse be a (beleértve a kiegészítő fájlokat, de kivételével aszinkron feladatokhoz, mint az Ajax-hívások). 
  * Ha `overridePageViewDuration` a hello [lap konfigurációs](#detailed-configuration), hello első ügyfél kérést tooexecution a hello közötti távolság `trackPageView`. Ha trackPageView szokásos pozícióját hello parancsfájl hello inicializálás után helyezi át, akkor egy másik értéket fogja tartalmazni.
  * Ha `overridePageViewDuration` be van, egy argumentum hello megadott időtartamot `trackPageView()` hívja, majd hello argumentum értékét használja. 

## <a name="custom-page-counts"></a>Egyéni lapok száma
Lapok alapértelmezés szerint minden alkalommal, amikor egy új lap betölti a hello az ügyfél böngészője történik.  De érdemes lehet további Lapmegtekintések toocount. Például egy lap lapok megjelenítheti benne lévő tartalom, és toocount oldal szüksége, amikor hello felhasználói lapok vált. Vagy JavaScript-kód hello lap előfordulhat, hogy új tartalom betöltése hello böngésző URL-cím megváltoztatása nélkül.

Szúrja be a JavaScript hívás pontján hello megfelelő az Ügyfélkód:

    appInsights.trackPageView(myPageName);

hello lapnév tartalmazhat hello azonos karaktert tartalmaz, mint egy URL-címet, de semmi után "#" vagy "?" figyelmen kívül hagyja.

## <a name="usage-tracking"></a>Használatkövetés
Szeretné, hogy ki a felhasználók mit alkalmazása toofind?

* [Információk a használatkövetésről](app-insights-web-track-usage.md)
* [Információk az egyéni eseményekről és a mérőszám API-ról](app-insights-api-custom-events-metrics.md).

## <a name="video"></a>Videó


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <a name="next"></a> Következő lépések
* [Használat követése](app-insights-web-track-usage.md)
* [Egyéni események és a mérőszámok](app-insights-api-custom-events-metrics.md)
* [Összeállítás, mérés, tanulás](app-insights-web-track-usage.md)

