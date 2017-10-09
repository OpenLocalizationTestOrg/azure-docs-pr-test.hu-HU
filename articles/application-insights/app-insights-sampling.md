---
title: "az Azure Application Insights aaaTelemetry mintavételi |} Microsoft Docs"
description: "Hogyan tookeep hello telemetriai adatot vezérlése alatt."
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: e19c350d0a5f16736c100322a9922fcfbf5d7b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sampling-in-application-insights"></a>Application Insights-mintavétel


A mintavétel az szolgáltatása [Azure Application Insights](app-insights-overview.md). Hello ajánlott módja tooreduce telemetriai forgalom és a tárolás, az alkalmazásadatok statisztikailag megfelelő elemzési adatainak megőrzése mellett. hello szűrő elemek kapcsolódó, választja ki, így navigálhat diagnosztikai vizsgálatok során elemek között.
Metrika számok hello portálon tooyou mutatnak be, amikor azok renormalized mintavételi, bármilyen hatása hello statisztika toominimize hello tootake figyelembe.

Mintavételi csökkenti a forgalom és az adatok költségeket, és segít elkerülni a sávszélesség-szabályozás.

## <a name="in-brief"></a>Röviden:
* Mintavételi őrzi meg az 1  *n*  hello rest figyelmen kívül hagyása rögzíti. Például akkor lehet, hogy továbbra is 1: 5 események, a mintavételi ráta 20 %-os. 
* Mintavételi automatikusan történik, ha az alkalmazás nagy mennyiségű telemetriai adatokat küld az ASP.NET web server apps.
* Azt is beállíthatja, hogy manuálisan, vagy a hello mintavételi portált hello árképzést ismertető oldalra; vagy hello ASP.NET SDK hello .config kiterjesztésű fájl, a tooalso hello hálózati forgalom csökkentése érdekében.
* Ha szeretné, hogy, hogy egy, akár őrzi meg, illetve elvetett együtt toomake egyéni események naplózása, győződjön meg arról, hogy azok rendelkeznek hello OperationID azonosítójú ugyanazt az értéket.
* hello mintavételi osztó  *n*  hello tulajdonságban rekordokban levő jelentett `itemCount`, ami keresési hello rövid neve "kérelem száma" vagy "események száma" alatt jelenik meg. Ha a mintavételi nincs a művelet, `itemCount==1`.
* Írás Analytics lekérdezések, akkor [mintavételi figyelembe](app-insights-analytics-tour.md#counting-sampled-data). Különösen helyett egyszerűen számbavételi rekordok, használjon `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Mintavételi típusai
Alternatív mintavételi három módszer áll rendelkezésre:

* **Adaptív mintavételi** automatikusan beállítja az ASP.NET-alkalmazás az SDK hello küldött telemetriai hello mennyiségét. Alapértelmezés szerint az SDK v 2.0.0-beta3. Jelenleg csak az ASP.NET kiszolgálóoldali telemetriai adat esetén érhető el. 
* **Rögzített mintavételi** mindkét az ASP.NET kiszolgálóról, és a felhasználók böngészőjének továbbított telemetriai hello mennyiségét csökkenti. Hello sebességét beállíthatja. hello ügyfél- és a mintavételi fogja szinkronizálni, a keresés, navigálhat kapcsolódó Lapmegtekintések és kérések között.
* **Adatfeldolgozást mintavételi** hello Azure-portálon is működik. Törli az alkalmazásból, Ön által beállított ütemben érkező hello telemetriai adatok némelyike. Nem csökkenthető a telemetria-forgalom, de lehetővé teszi a havi kvótán belül. hello nagy adatfeldolgozást mintavételi előnye, hogy az alkalmazás üzembe helyezésével állíthatja be, és egységesen összes kiszolgálók és ügyfelek esetében működik. 

Ha az adaptív vagy rögzített arány mintavételi művelet, adatfeldolgozást mintavételi le van tiltva.

## <a name="ingestion-sampling"></a>Adatfeldolgozást mintavétel
Az űrlap mintavételi hello pont, ahol a webkiszolgáló böngészők és eszközök hello telemetriai eléri hello Application Insights szolgáltatás végpontjának működik. Bár ez nem csökkenthető hello telemetriai forgalom küldése az alkalmazásból, az csökkentheti hello feldolgozása és maradnak (majd díjat) az Application Insights által.

Az ilyen mintavételi típusú, ha az alkalmazás gyakran kerül felett a havi kvótát, és nem kell hello hello SDK-alapú típusú mintavételi bármelyikével lehetőséget. 

Állítsa be a hello kvóták hello mintavételi gyakoriság és az árazás panel:

![Hello alkalmazás áttekintése panelen kattintson a beállítások, a kvóta, a mintákat, majd válassza ki a mintavételi ráta, és kattintson a frissítés gombra.](./media/app-insights-sampling/04.png)

Mintavételi más típusú, mint a hello algoritmus megtartja a kapcsolódó telemetriai adat elemek. Például hello telemetriai Keresés most vizsgálatakor ellenőrizze, képes lesz toofind hello kérelem kapcsolódó tooa adott kivétel. A metrika számít például kérelmek aránya, és kivétel arány megfelelően megőrződnek.

Hogy a rendszer elveti a mintavétellel már nem érhetők el bármilyen Application Insights szolgáltatás például [a folyamatos exportálás](app-insights-export-telemetry.md).

Adatfeldolgozást mintavételi nem működik, amikor az SDK-alapú adaptív vagy rögzített mintavételi művelet van. Ha hello mintavételi ráta hello SDK 100 %-nál kisebb, hello adatfeldolgozást mintavételi ráta beállított figyelmen kívül hagyja.

> [!WARNING]
> hello hello csempén megjelenő érték hello adatfeldolgozást mintavételi beállított értékeket. Hello tényleges mintavételi ráta az nem jelent, ha SDK mintavételi művelet.
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a>A webkiszolgáló adaptív mintavétel
Adaptív mintavételi hello Application Insights SDK az ASP.NET v 2.0.0-beta3 és újabb verzióihoz érhető el, és alapértelmezés szerint engedélyezve van. 

Adaptív mintavételi hatással van a web server alkalmazás toohello Application Insights szolgáltatás által küldött telemetriai hello mennyiségét. hello kötet módosul automatikusan tookeep belül forgalom a megadott maximális értéket.

Azt nem működik, telemetriai adatokat kis mennyiségű úgy egy alkalmazást a hibakeresés, vagy nem befolyásolja az alacsony használat rendelkező webhely.

tooachieve hello célkötet, néhány hello telemetriai adatot generált a rendszer elveti. De mintavételi más típusú, például hello algoritmus kapcsolódó telemetriai adat elemek megőrzi. Például hello telemetriai Keresés most vizsgálatakor ellenőrizze, képes lesz toofind hello kérelem kapcsolódó tooa adott kivétel. 

A metrika álló, például a kérelmek gyakorisága és kivétel alapján a mintavételi ráta, hello módosított toocompensate, hogy azok körülbelül megfelelő értékek megjelenítése a metrika Intézőben.

**Frissítse a projekt NuGet** toohello legújabb csomagok *kiadás előtti* Application Insights verziója: kattintson a jobb gombbal a hello projektben a Megoldáskezelőre, válassza a NuGet-csomagok kezelése, ellenőrizze **belefoglalása előzetes** és Microsoft.ApplicationInsights.Web megkereséséhez. 

A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), módosíthatja a hello számos paraméter `AdaptiveSamplingTelemetryProcessor` csomópont. hello ábrán is látható hello alapértelmezett értékei lesznek:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    hello adaptív algoritmus hello cél sebességet célja a **minden kiszolgáló állomáson**. Ha sok gazdagép a webalkalmazás fut, csökkentse tooremain belül hello Application Insights portál-forgalmat a cél aránya, ezért ezt az értéket.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    hello időtartama, mely hello telemetriai jelenlegi sebessége újraértékelése is megtörténik. Kiértékelési mozgóátlaga szerint történik. Érdemes tooshorten Ez az időtartam alatt a telemetriai adatok azokért toosudden felszakadásáig esetén.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Ha mintavételi százalékos változásainak, hogy mikor után azt engedélyezett százalékos újra mintavételi toolower toocapture kevésbé adatok.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Ha mintavételi százalékos változásainak, hogy mikor után azt engedélyezett százalékos újra mintavételi tooincrease toocapture több adatot.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Mivel százalékos mintavételi változik, mi az hello minimális érték azt hogy jogosult tooset.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Mivel százalékos mintavételi változik, mi az hello maximális érték azt hogy jogosult tooset.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    Hello mozgóátlaga hello kiszámítása hello súly toohello legutóbbi értéket kapja. 1-nél kisebb érték egyenlő tooor használja. A kisebb értékek módosításokat hello algoritmus kevesebb reaktív toosudden.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Amikor hello app elkezdte hello érték. Nem csökkenti a akkor hibakeresése közben. 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Pontosvesszővel elválasztott felsorolása, amelyek nem szeretné, hogy toobe mintát venni. Felismert típusok a következők: függőségi, esemény, kivétel, PageView, kérelem, nyomkövetési. Hello összes példánya megadott típusok továbbításuk; Nincs megadva hello típusok lekérdező.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Egy pontosvesszővel tagolt listáját, amelyet a mintát toobe típusok. Felismert típusok a következők: függőségi, esemény, kivétel, PageView, kérelem, nyomkövetési. hello megadott típusok lekérdező; összes példányát a hello más mindig továbbítja.


**ki tooswitch** adaptív mintavételi, eltávolítás hello AdaptiveSamplingTelemetryProcessor csomópont a applicationinsights-konfiguráció.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternatív: Adaptív mintavétel konfigurálása a kódban
Mintavételi hello .config kiterjesztésű fájl helyett, a kódot is használhatja. Ez lehetővé teszi a visszahívási függvény, amelyet mindig hello mintavételi ráta újraértékelése is megtörténik, amikor toospecify. Ez használhat, például milyen mintavételi ráta kimenő toofind használatban van.

Távolítsa el a hello `AdaptiveSamplingTelemetryProcessor` csomópontot hello .config fájlból.

*C#*

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust hello settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report hello sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([További információ a telemetriai adatok processzorok](app-insights-api-filtering-sampling.md#filtering).)

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a>Mintavételi JavaScript weblapok
Konfigurálhatja a weblapok rögzített mintavételi tetszőleges kiszolgálóról. 

Ha Ön [hello weblapok konfigurálja az Application Insights](app-insights-javascript.md), hello részlet kapott hello Application Insights portál módosítása. (Az ASP.NET alkalmazásokban hello részlet általában kerül a _Layout.cshtml.)  Például a sor beszúrása `samplingPercentage: 10,` hello instrumentation kulcs előtt:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Hello mintavételi arány válassza a Bezárás too100/N van, ahol N az egész érték.  Mintavételi jelenleg nem támogatja a más értékek.

Ha rögzített mintavételi hello kiszolgálón is engedélyeznie hello ügyfelek és a kiszolgáló szinkronizálja, az adott, a keresési navigálhat kapcsolódó Lapmegtekintések és kérések között.

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a>Az ASP.NET-webhelyeket rögzített mintavétel
Rögzített alapján mintavételi csökkenti a webkiszolgáló és a böngészők által küldött hello forgalmat. Adaptív mintavételi eltérően csökkenti telemetriai rögzített kulcs határoz meg. Azt is szinkronizálja hello ügyfél és kiszolgáló mintavételi, hogy a kapcsolódó elemek megmaradnak – például, hogy a nézet a keresési tekinti meg, ha a kapcsolódó kérelemre található.

hello mintavételi algoritmus megtartja a kapcsolódó elemeket. Az egyes HTTP-kérelmek esemény, ezért a kapcsolódó események vagy elvetett vagy továbbítani. 

A Metrikaböngészőben díjszabás kérelem és a kivétel számát, például program megszorozza egy tényező toocompensate hello mintavételi ráta, az, hogy azok körülbelül helyességét.

1. **A projekt NuGet-csomagok frissítése** toohello legújabb *kiadás előtti* Application Insights verzióját. Kattintson a jobb gombbal a hello projektben a Megoldáskezelőre, válassza a NuGet-csomagok kezelése, ellenőrizze **közé tartoznak az előzetes** és Microsoft.ApplicationInsights.Web megkereséséhez. 
2. **Tiltsa le a adaptív mintavételi**: A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), távolítsa el vagy hello megjegyzéssé `AdaptiveSamplingTelemetryProcessor` csomópont.
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. **Hello rögzített mintavételi modul engedélyezése.** Adja hozzá ezt a kódrészletet túl[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> Hello mintavételi arány válassza a Bezárás too100/N van, ahol N az egész érték.  Mintavételi jelenleg nem támogatja a más értékek.
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Másik megoldás: a kiszolgáló kódjában található rögzített mintavételi engedélyezése
Hello mintavételi paraméter beállításhoz hello .config fájl helyett a kód is használhatja. 

*C#*

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([További információ a telemetriai adatok processzorok](app-insights-api-filtering-sampling.md#filtering).)

## <a name="when-toouse-sampling"></a>Amikor toouse mintavételi?
Adaptív mintavételi automatikusan engedélyezett, ha az ASP.NET SDK verzió 2.0.0-beta3 hello használata vagy újabb. Függetlenül attól, milyen SDK verzióját használja használhat adatfeldolgozást mintavételi (a kiszolgáló).

Mintavételi legtöbb kis és közepes méretű alkalmazásokhoz nem szükséges. hello legfontosabb diagnosztikai adatokat és a legpontosabb statisztika nyerhetők adatokat gyűjt a felhasználói tevékenységekről. 

mintavételi hello fő előnyei a következők:

* Application Insights szolgáltatás elhagyta a(z) ("szabályozások") adatpontok rövid üzenetet küld a nagyon nagy mértékben telemetriai adatot az alkalmazás időintervallum. 
* hello belül tookeep [kvóta](app-insights-pricing.md) az adatokat a tarifacsomagot. 
* tooreduce hálózati forgalmat a telemetriai adatok hello gyűjteménye. 

### <a name="which-type-of-sampling-should-i-use"></a>Milyen típusú mintavételi érdemes használni?
**Használja az adatfeldolgozást mintavételi, ha:**

* Gyakran halad át a havi kvóta telemetriai adatot.
* Akkor egy verzióját használja, amely nem támogatja például mintavételi - SDK hello, Java SDK hello vagy ASP.NET verzió 2-nél korábbi.
* A felhasználók webböngészővel kap nagy mennyiségű telemetriai adatokat.

**Használjon rögzített mintavételi, ha:**

* Application Insights SDK hello használata esetén az ASP.NET web services, verziószám: 2.0.0 vagy újabb verzióját, és
* Ügyfél és kiszolgáló közötti szinkronizált mintavételi kívánt, az, hogy ha éppen vizsgálja a események [keresési](app-insights-diagnostic-search.md), kapcsolódó események hello ügyfélen és a kiszolgáló, például a lapmegtekintések és a http-kérelmek között válthat.
* Biztos abban az hello megfelelő mintavételi arány az alkalmazásra vonatkozóan. Az legyen elég magas tooget pontos mérőszámokat, de az alábbiakban hello arány, amely meghaladja a árképzési kvótát, és szabályozás korlátok hello. 

**Adaptív mintavételi használja:**

Ellenkező esetben ajánlott adaptív mintavételi. Ez a hello ASP.NET server SDK verzió 2.0.0-beta3 alapértelmezés szerint engedélyezve van, vagy később. Azt nem csökkenthető a forgalom csak egy bizonyos minimális sebessége, így nem lesz hatással a alacsony használható hely.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Hogyan állapítható meg, hogy a mintavétel van-e művelet?
a mintavételi arány nem számít, ha az telepítve van, használja a tényleges toodiscover hello egy [Analytics lekérdezési](app-insights-analytics.md) Ez például:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

Az egyes megőrzi a rekord, `itemCount` hello eredeti rekordok száma, amely azt jelöli, annál too1 + hello előző elvetett rekordok számát jelzi. 

## <a name="how-does-sampling-work"></a>Hogyan működik a mintavételi?
Rögzített és adaptív mintavételi egyik újdonsága az SDK hello 2.0.0 és újabb verziók esetében az ASP.NET-verziókban. Adatfeldolgozást mintavételi hello Application Insights szolgáltatás egyik szolgáltatása, és lehet a műveletet, ha hello SDK nem hajt végre mintavétel. 

hello mintavételi algoritmus úgy dönt, mely telemetriai elemek toodrop, és melyik kiépítettektől eltérő tookeep (hogy az hello SDK vagy van hello Application Insights szolgáltatásban). hello mintavételi döntési különböző szabályok, amelyek célja toopreserve összes egymáshoz adatpontok változatlanok maradnak, egy diagnosztikai megoldást vezet be, hogy végrehajthatóként és megbízhatóbb, még akkor is csökkentett adatkészlet az Application insightsban karbantartása alapul. Például ha a sikertelen kérelmek az alkalmazás további telemetriai elemeket (például kivétel és naplózza a kérést a nyomkövetések) küld, mintavételi nem elválasztja az ehhez a kérelemhez és egyéb telemetriai adatokat. Az tartja vagy együtt elutasítja azokat. Ennek eredményeképpen ha hello kérelemrészletek az Application Insightsban tekinti meg, bármikor megtekintheti hello kérelem és a kapcsolódó telemetriai elemek. 

Az alkalmazások, amelyek meghatározzák a "user" (Ez azt jelenti, hogy a legjellemzőbb webalkalmazások), hello mintavételi döntési hello felhasználói azonosítóját, ami azt jelenti, hogy bármely adott felhasználó összes telemetriai adat vagy megőrzi, vagy kihagyott hello kivonatának alapul. Alkalmazások felhasználók számára (például a web services) nem meghatározó hello típusú hello mintavételi döntési hello műveletazonosító hello kérelem alapul. Végezetül cikkeknél hello telemetriai adat sem be (például telemetriai elemek nem http-környezetben az aszinkron szál jelentett) és a felhasználó nem művelet azonosítója mintavételi egyszerűen rögzíti a rendelkezésre álló telemetriai elemek százalékaként. 

Ha telemetriai hátsó tooyou bemutató, hello Application Insights szolgáltatás hello mérőszámok alapján beállítja hello azonos mintavételi arány, amely szerepel a gyűjteményben, a hiányzó adatpontokat hello toocompensate hello időpontjában. Emiatt az Application Insightsban hello telemetriai megtekintve hello felhasználók azért jelent meg statisztikailag megfelelő becsléseket, amelyek rendkívül szoros toohello valós szám.

hello pontossága hello közelítés nagymértékben függ a konfigurált hello mintavételi arány. Hello pontossága is, az alkalmazások, amelyek kezelik a felhasználók sok általában hasonló kérelmek nagy mennyiségű növeli. A hello ugyanakkor, az alkalmazások, amelyek nem működnek a jelentős terhelés, mintavételi nem szükséges, mivel ezek az alkalmazások általában küldhet a telemetriai adatok maradva hello keretén belül a sávszélesség-szabályozás adatvesztés nélkül. 

Vegye figyelembe, hogy az Application Insights nem minta metrikák és a munkamenetek telemetriai típusok óta ezekhez, hello pontosság csökkenésével magas nemkívánatos lehet. 

### <a name="adaptive-sampling"></a>Adaptív mintavétel
Adaptív mintavételi hozzáadása egy összetevő figyelők hello adott jelenlegi átviteli hello SDK, és beállítja hello mintavételi százalékos tootry toostay hello cél maximális sebesség belül. hello módosítás rendszeres időközönként újraszámítja, és átviteli sebességet kimenő hello mozgóátlaga alapul.

## <a name="sampling-and-hello-javascript-sdk"></a>Mintavételi és hello JavaScript SDK
hello ügyféloldali (JavaScript) SDK vesz részt a rögzített mintavételi hello kiszolgálóoldali együtt SDK. hello tagolva lapok csak küldése ügyféloldali telemetriai az azonos felhasználók melyik hello kiszolgálóoldali arról döntése túl "sample a." hello Ez a módszer tervezett toomaintain integritási felhasználói munkamenet között - és kiszolgáló-ügyféloldalon. Ennek eredményeképpen bármely Application Insights telemetria adott eleménél minden egyéb telemetriai elemek találhatók a felhasználó vagy a munkamenet. 

*Az ügyfél és a kiszolgálóoldali telemetriai ne jelenjen meg koordinált minták fent leírtak.*

* Győződjön meg arról, hogy a rögzített mintavételi mind a kiszolgáló és az ügyfél engedélyezve.
* Győződjön meg arról, hogy hello SDK-verzió: 2.0-s vagy újabb.
* Ellenőrizze, hogy a beállított hello azonos mintavételi hello ügyfél és kiszolgáló százalékos aránya.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
*Miért nem mintavételi egy egyszerű "collect X valamennyi telemetriai típusú százaléka"?*

* A mintavételi megközelítést metrika becsléseket a nagyon nagy pontosságú volna meg, amíg képes toocorrelate diagnosztikai adatok felhasználó, a munkamenet és a kérést, ami kritikus diagnosztikai megszűnését. Ezért mintavételi works jobban az "összes telemetriai elemek összegyűjtése az X százalék az alkalmazás felhasználók" vagy "app kérelmek százalékos X az összes telemetriai adat gyűjtése" logikát. Hello telemetriai elemek nem hello kérések (például a háttérben történő aszinkron feldolgozás) társított, hello alá esik biztonsági mentés akkor túl "collect X összes elemet az egyes telemetriai százalékát." 

*Hello mintavételi arány idővel megváltozhat?*

* Igen, adaptív mintavételi fokozatosan módosítja hello mintavételi arány alapján hello jelenleg megfigyelt hello telemetriai adatot.

*Ha rögzített mintavételi használatához honnan tudhatom, hogy mely mintavételi százalékos működnek hello ajánlott az alkalmazásom?*

* Egyik módja az adaptív mintavételi toostart, megtudhatja, mi értékelés rendezi a (lásd fent kérdés hello), és ezután kapcsoló toofixed sebesség mintavételi adott alapján. 
  
    Ellenkező esetben tooguess rendelkezik. Az aktuális telemetriai használatának a AI elemzése, tekintse át az esetleges szabályozási, vagyis lépett fel, és becsült hello kötet hello gyűjtött telemetriai adatot. Ezen három bemeneti adatokat, és a választott tarifacsomaghoz javasoljuk, hogy milyen mértékű érdemes tooreduce hello kötet hello gyűjtött telemetriai adatot. Azonban a felhasználók hello számának növekedése vagy néhány más shift hello kötet telemetriai adatot előfordulhat, hogy érvénytelenné válnak a becsült.

*Mi történik, ha a mintavételi arány túl alacsony konfigurálni?*

* A túl alacsony mintavételi arány (over-aggressive mintavételi) kevesebb hello becsléseket hello pontosságát, amikor az Application Insights kísérletek toocompensate hello képi megjelenítés hello adatok hello adatok kötet csökkentése érdekében. Is diagnosztikai élmény előfordulhat, hogy csökkenéséhez vezet, rendszereivel hello ritkán sikertelen vagy lassú kérelmek lehet mintát venni ki.

*Mi történik, ha a mintavételi arány túl magas konfigurálni?*

* Hello hello mennyiségű elegendő csökkenést konfigurálása túl magas mintavételi százalékos aránya (nem agresszív elég) eredmények gyűjtött telemetriai adatokat. Továbbra is problémákat tapasztalhat a telemetriai adatok elvesztését kapcsolódó toothrottling, és az Application Insights hello költségeinek lehet magasabb, mint amennyi lejáró költségek toooverage tervezett.

*Milyen platformokon használható mintavételi?*

* Adatfeldolgozást mintavételi automatikusan történik az összes telemetriai fent egy bizonyos köteten, ha hello SDK nem hajt végre mintavétel. Ez akkor működik, például ha az alkalmazás egy Java-kiszolgálót használ, vagy ha az ASP.NET SDK hello egy régebbi verzióját használja.
* ASP.NET SDK verziók 2.0.0 használata vagy újabb (az Azure-ban vagy a saját kiszolgáló üzemelteti), adaptív alapértelmezés szerint a mintavételi kap, de toofixed sebesség átválthatja a fent leírt módon. A rögzített mintavételi hello böngésző SDK automatikusan szinkronizálja toosample kapcsolatos eseményeket. 

*Nincsenek az egyes ritka események toosee mindig szeretné. Hogyan kaphatok őket túli hello mintavételi modul?*

* Egy új TelemetryConfiguration (nem hello alapértelmezett aktív) rendelkező TelemetryClient külön példányt inicializálni. Használja az adott toosend a ritka eseményeket.

## <a name="next-steps"></a>Következő lépések
* [Szűrés](app-insights-api-filtering-sampling.md) biztosíthat további szigorú ellenőrzése az SDK küld.

