---
title: "egyéni események és metrikák Hirdetéselemző API-t aaaApplication |} Microsoft Docs"
description: "Helyezze be néhány sornyi kódot az eszköz vagy egy asztali alkalmazás, a weblap vagy egy szolgáltatást, tootrack használatának és problémák elemzéséhez."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: f3d207a47bb4825efda806a19dd0c26540db7bdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Application Insights API egyéni események és metrikák

Szúrja be néhány sornyi kódot az alkalmazás toofind ki a felhasználók tevékenységeit vele, vagy toohelp eseményadatokat. Eszköz- és asztali alkalmazások, a webes ügyfelekkel és a webkiszolgálók telemetriai adatokat küldhet. Használjon hello [Azure Application Insights](app-insights-overview.md) alapvető telemetriai API toosend egyéni események és metrikák és a saját szabványos telemetriai verziói. Ez az API van hello azonos API-t, hogy az Application Insights-adatgyűjtők használja hello szabvány.

## <a name="api-summary"></a>API összefoglaló
hello API egységes néhány kisebb eltérések mellett minden platformon.

| Módszer | A használt |
| --- | --- |
| [`TrackPageView`](#page-views) |Lapok, képernyők, paneleken vagy űrlap. |
| [`TrackEvent`](#trackevent) |Felhasználói műveletek és más események. Használt tootrack felhasználói viselkedés vagy toomonitor teljesítményét. |
| [`TrackMetric`](#trackmetric) |Például a várólista-hosszúságok meggátolják TELJESÍTMÉNYMÉRÉSEK toospecific események nem kapcsolódnak. |
| [`TrackException`](#trackexception) |Naplózási kivételek elemzés céljából. Nyomkövetés, ahonnan fordulhat elő, a kapcsolat tooother események és vizsgálja meg a híváslánc megjelenik. |
| [`TrackRequest`](#trackrequest) |A naplózás hello gyakorisága és időtartama a kiszolgálói kérelmek Teljesítményelemzési célokat. |
| [`TrackTrace`](#tracktrace) |Diagnosztikai naplózás. Külső naplókat is rögzítheti. |
| [`TrackDependency`](#trackdependency) |A naplózás hello időtartama és gyakorisága hívások tooexternal összetevők, amelyek az alkalmazás függ. |

Is [tulajdonságok és metrikákat](#properties) toomost az adott telemetriai hívások.

## <a name="prep"></a>Előkészületek
Ha egy hivatkozás még nem rendelkezik az Application Insights SDK:

* Hello Application Insights SDK tooyour projekt hozzáadása:

  * [ASP.NET-projekt](app-insights-asp-net.md)
  * [Java-projekt](app-insights-java-get-started.md)
  * [Az összes weboldal JavaScript](app-insights-javascript.md) 
* Az eszköz vagy a web server kódjában a következők:

    *C#:*`using Microsoft.ApplicationInsights;`

    *Visual Basic:*`Imports Microsoft.ApplicationInsights`

    *Java:*`import com.microsoft.applicationinsights.TelemetryClient;`

## <a name="constructing-a-telemetryclient-instance"></a>Egy TelemetryClient példány létrehozása
Példányának összeállításához `TelemetryClient` (csak a JavaScript weblapok):

*C#*

    private TelemetryClient telemetry = new TelemetryClient();

*Visual Basic*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();

TelemetryClient szálbiztos.

Azt javasoljuk, hogy az alkalmazás minden modul TelemetryClient-példány használata. Például előfordulhat, hogy egy TelemetryClient példány a webszolgáltatás tooreport bejövő HTTP-kérelmek és egy másik egy köztes osztály tooreport üzleti logika események. Beállítható például `TelemetryClient.Context.User.Id` tootrack felhasználók és a munkamenetek, vagy `TelemetryClient.Context.Device.Id` tooidentify hello gép. Ez az információ a csatolt tooall az eseményeket, amelyek hello példány küld.

## <a name="trackevent"></a>TrackEvent
Az Application Insights egy *egyéni esemény* a megjeleníthetők adatpont van [Metrikaböngésző](app-insights-metrics-explorer.md) egy összesített száma, és a [diagnosztikai keresési](app-insights-diagnostic-search.md) önálló események. (Ez nem a kapcsolódó tooMVC vagy más keretrendszer "események.")

Helyezze be `TrackEvent` hívja be a kódját toocount különféle eseményeket. Milyen gyakran válassza ki a felhasználók egy adott szolgáltatással, milyen gyakran azok meghatározott célok elérése, vagy lehet, hogy milyen gyakran akkor hibák adott típusú.

Például játék alkalmazásban elküldeni egy eseményt, amikor egy felhasználó wins hello játék:

*JavaScript*

    appInsights.trackEvent("WinGame");

*C#*

    telemetry.TrackEvent("WinGame");

*Visual Basic*

    telemetry.TrackEvent("WinGame")

*Java*

    telemetry.trackEvent("WinGame");

### <a name="view-your-events-in-hello-microsoft-azure-portal"></a>Az események megtekintése hello Microsoft Azure portálon
Nyissa meg a toosee az események száma egy [Metrikaböngésző](app-insights-metrics-explorer.md) panelen új diagram hozzáadása, és válassza ki **események**.  

![Tekintse meg az egyéni események száma](./media/app-insights-api-custom-events-metrics/01-custom.png)

különböző események számát toocompare hello beállítása hello diagramtípus túl**rács**, és az esemény neve:

![Hello diagramtípus és csoportosítási beállítása](./media/app-insights-api-custom-events-metrics/07-grid.png)

Hello rácson kattintson az esemény neve toosee egyedi adott esemény előfordulása. toosee több részletességi - kattintson bármely előfordulása hello listában.

![Részletezés hello események](./media/app-insights-api-custom-events-metrics/03-instances.png)

a meghatározott események keresési vagy a Metrikaböngésző, set hello panel szűrő toohello esemény nevét, amely kíváncsiak vagyunk toofocus:

![Nyissa meg a szűrők, bontsa ki az esemény nevét, és válasszon egy vagy több érték](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a>Egyéni események Analytics

hello telemetriai érhető el hello `customEvents` a tábla [Application Insights Analytics](app-insights-analytics.md). Minden egyes sorára túl jelenti. a hívás`trackEvent(..)` az alkalmazásban. 

Ha [mintavételi](app-insights-sampling.md) működik, hello elemek tulajdonság jeleníti meg egy értéket 1-nél nagyobb. Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackEvent() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét. egyéni események megfelelő száma tooget, kell használnia ezért használható kóddal például `customEvent | summarize sum(itemCount)`.


## <a name="trackmetric"></a>TrackMetric

Az Application Insights is diagram, amelyek nem csatlakoztatott tooparticular események metrikákat. Például felügyelheti a várólista hossza rendszeres időközönként. A metrika hello egyedi mérések kevésbé fontos, mint a hello változata és a trendeket, és így statisztikai diagramok hasznosak.

A rendezés toosend metrikák tooApplication Insights, használhatja a hello `TrackMetric(..)` API. Számos két módon toosend metrikát. 

* Egyetlen értéket. Minden alkalommal, amikor az alkalmazás hajt végre egy mérték, küldött megfelelő érték hello tooApplication Insights. Tegyük fel például, hogy rendelkezik-e a tárolóban lévő elemek száma hello leíró metrikát. Egy adott időszakon belül mindhárom elem put hello tárolóba, és két elem távolítsa el. Ennek megfelelően meghívta `TrackMetric` kétszer: először a hello értéket átadja `3` és majd hello érték `-2`. Az Application Insights mindkét értéket tárolja az Ön nevében. 

* Összesítést. Az metrikák használatakor minden egyetlen mérési ritkán érdekében áll. Ehelyett fontos Mi történt egy adott időszakon belül összegzését. Ilyen összegzését nevezik _összesítési_. A fenti példa hello, hello összesített metrika sum időszak a `1` , hello szám hello metrika értékek `2`. Hello összesítési módszer használata esetén csak indításakor `TrackMetric` időtartam és a küldési hello összesített időértékeket egy alkalommal. Ez az ajánlott megközelítést alkalmazva, mivel jelentősen csökkentheti a hello költség, és a teljesítmény szerint munkaterhelés kevesebb adatküldés tooApplication Insights pontok továbbra is az összes vonatkozó információk összegyűjtése közben hello.

### <a name="examples"></a>Példák:

#### <a name="single-values"></a>Egyetlen értékek

toosend a egyetlen metrika értékét:

*JavaScript*

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

*C#, Java*

```C#
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

#### <a name="aggregating-metrics"></a>Metrikák összesítése

Mielőtt elküldené a alkalmazást, a tooreduce sávszélesség, a költségek és tooimprove teljesítmény tooaggregate metrikák ajánlott.
Itt látható egy példa összesítése kódot:

*C#*

```C#
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends hello aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of hello aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap hello current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute hello actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct hello metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a>Egyéni metrikák a Metrikaböngészőben

toosee hello eredményeivel, nyissa meg a Metrikaböngésző, és új diagram hozzáadása. Hello diagram tooshow a metrika szerkesztéséhez.

> [!NOTE]
> Az egyéni metrika is igénybe vehet néhány percet tooappear elérhető hello listájában.
>

![Új diagram hozzáadása vagy a diagram az egyéni, válassza ki és a metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a>Egyéni metrikák Analytics

hello telemetriai érhető el hello `customMetrics` a tábla [Application Insights Analytics](app-insights-analytics.md). Minden egyes sorára túl jelenti. a hívás`trackMetric(..)` az alkalmazásban.
* `valueSum`-Ez az hello mérések hello összege. tooget hello átlagos értékének nullával `valueCount`.
* `valueCount`-Ez volt összesítése mérések számát hello `trackMetric(..)` hívható meg.

## <a name="page-views"></a>Lapmegtekintések
Egy eszköz vagy a weblap alkalmazásban lap nézet telemetriai küldi alapértelmezés szerint ha egyes képernyőit vagy lapon be van töltve. De további vagy különböző időpontokban, hogy tootrack Lapmegtekintések módosíthatja. Például megjeleníti a tabulátorokat vagy paneleken alkalmazásban, érdemes tootrack oldal amikor hello felhasználó megnyit egy új panelen.

![Használati fókuszban panelen – áttekintés](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Felhasználó és a munkamenet adatküldést, tulajdonságai lap nézetek, valamint úgy hello felhasználó és a munkamenet diagramok lap nézet telemetriai esetén életben származnak.

### <a name="custom-page-views"></a>Egyéni Lapmegtekintések
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*Visual Basic*

    telemetry.TrackPageView("GameReviewPage")


Ha különböző HTML-lapok belül több lap van, hello URL-címe túl is megadhatja:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a>Lapmegtekintések időzítése
Alapértelmezés szerint a hello idők jelentése szerint **lapmegtekintés betöltési ideje** vannak mért hello böngésző hello kérelmet küld, amíg hello böngésző lap betöltési esemény nevezik.

Ehelyett lehetőségek közül választhat:

* Az explicit időtartamot hello [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) hívja: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Hello oldalhívás nézet időzítési használja `startTrackPage` és `stopTrackPage`.

*JavaScript*

    // toostart timing a page:
    appInsights.startTrackPage("Page1");

...

    // toostop timing and log hello page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

hello hello első paraméter társítja hello start használható nevét, és állítsa le a hívások. Alapértelmezés szerint toohello aktuális lap neve.

hello eredményül kapott lapbetöltési jelenik meg a Metrikaböngészőben időtartamok vannak származtatva hello hello időközétől start, és állítsa le a hívásokat. Az működik tooyou milyen időköz ténylegesen idő.

### <a name="page-telemetry-in-analytics"></a>Az elemzés lap telemetria

A [Analytics](app-insights-analytics.md) két tábla browser műveletek adatait megjelenítése:

* Hello `pageViews` tábla hello URL-cím és a lap címét kapcsolatos adatokat tartalmaz.
* Hello `browserTimings` tábla ügyfél teljesítményét, kapcsolatos adatokat tartalmaz, például a hello igénybe vett idő tooprocess hello bejövő adatok

toofind mennyi ideig hello böngésző veszi tooprocess különböző oldalain:

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

a különböző böngészők toodiscover a hello popularities:

```
pageViews | summarize count() by client_Browser
```

tooassociate oldalhívás nézetek tooAJAX, csatlakozik a függőségek:

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest
hello server SDK TrackRequest toolog HTTP-kérelmek használja.

Akkor is is meghívhatja, ha azt szeretné, hogy toosimulate kérelmek környezetben ahol hello webes szolgáltatás modul fut nem rendelkezik.

Azonban hello ajánlott módja toosend – kéréstelemetria ahol hello kérelem úgy működik, mint egy <a href="#operation-context">műveletkörnyezetet</a>.

## <a name="operation-context"></a>A művelet a környezetben
Telemetria elemek együtt is társíthat, csatolásával toothem egy közös műveletet. hello normál kérés-nyomkövetési modul elvégzi ezt a kivételeket és eseményeket, amelyek küldött HTTP-kérelem feldolgozása közben. A [keresési](app-insights-diagnostic-search.md) és [Analytics](app-insights-analytics.md), hello azonosító tooeasily keresés használata hello kérelemhez társított eseményeket.

hello legegyszerűbb módja tooset hello azonosító tooset egy művelet környezet ebben a mintában használatával:

*C#*

```C#
// Establish an operation context and associated telemetry item:
using (var operation = telemetry.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use hello same operation ID.
    ...
    telemetry.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetry.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

Egy művelet környezet beállítása mellett `StartOperation` telemetriai megadott hello típusú elem létrehozása. Hello telemetriai meg eldobásakor hello műveletet, vagy ha explicit módon hívja elemet elküldi `StopOperation`. Ha `RequestTelemetry` hello telemetriai típusként időtartama túllépte az időkorlátot toohello időközétől kezdési és befejezési van beállítva.

Művelet környezetek nem ágyazhatók egymásba. Ha már van egy művelet környezetben, akkor minden tartalmazott hello elem, létre hello elem is tartozik Azonosítóval `StartOperation`.

A keresés hello műveletkörnyezetet használt toocreate hello **kapcsolódó elemek** listája:

![Kapcsolódó elemek](./media/app-insights-api-custom-events-metrics/21.png)

[Alkalmazás-insights – egyéni-műveletek-tracking.md] követési egyéni műveletek további információt talál.

### <a name="requests-in-analytics"></a>Az elemzés kérelmek 

A [Application Insights Analytics](app-insights-analytics.md), megjelenítése kéri fel a hello `requests` tábla.

Ha [mintavételi](app-insights-sampling.md) van a művelet hello elemek tulajdonság értékét fogja megjeleníteni a 1-nél nagyobb. Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackRequest() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét. tooget kérelmek és átlagos időtartama megfelelő száma szegmentált kérelem neve, a kódot használja, mint:

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a>TrackException
Kivételek tooApplication Insights küldeni:

* túl[megszámolni](app-insights-metrics-explorer.md), mint utalhat, hogy egy probléma hello gyakorisága.
* túl[vizsgálja meg az egyes előfordulások](app-insights-diagnostic-search.md).

hello jelentései tartalmazzák a hello híváslánc megjelenik.

*C#*

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

*JavaScript*

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }

hello SDK-k kivételeket sok automatikusan, így nem mindig toocall TrackException explicit módon.

* ASP.NET: [írhat kódot toocatch kivételek](app-insights-asp-net-exceptions.md).
* J2EE: [kivételek automatikusan észlelt](app-insights-java-get-started.md#exceptions-and-request-failures).
* JavaScript: Kivételek automatikusan észlelt. Ha azt szeretné, hogy toodisable automatikus gyűjtemény, adja hozzá egy sor toohello kódrészletet a weblapok beillesztett:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a>Az elemzés kivételek

A [Application Insights Analytics](app-insights-analytics.md), kivételek jelennek meg hello `exceptions` tábla.

Ha [mintavételi](app-insights-sampling.md) működik, hello `itemCount` tulajdonság jeleníti meg egy értéket 1-nél nagyobb. Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackException() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét. tooget kivételek megfelelő száma szegmentált által kivétel típusa, például a kód használata:

```
exceptions | summarize sum(itemCount) by type
```

Nagy része külön változók már kibontása verem adatokat, de lehet lekérni a egymástól hello fontos hello `details` struktúra tooget további. Mivel ez a struktúra dinamikus, akkor hello toohello eredménytípus várt kell konvertálni. Példa:

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

a kapcsolódó kérések tooassociate kivételek használjuk:

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a>TrackTrace
Használjon TrackTrace toohelp Insights egy "navigációs eljárást" tooApplication elküldésével problémák diagnosztizálásához. Adattömbök diagnosztikai adatok küldése, és vizsgálja meg azokat a [diagnosztikai keresési](app-insights-diagnostic-search.md).

[Naplófájl adapterek](app-insights-asp-net-trace-logs.md) ezen API toosend külső naplók toohello a portálon.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);


Üzenet tartalma kereshet, de (ellentétben a tulajdonságértékek) nem lehet szűrni rajta.

hello méretkorlátot `message` sokkal nagyobb, mint tulajdonságok hello vonatkozó korlátozást.
TrackTrace előnye, hogy viszonylag hosszú adatok helyezhetik hello üzenetben. Például lehet kódolása nincs POST-adatokat.  

Emellett egy súlyossági szint tooyour üzenetet is hozzáadhat. És egyéb telemetriai adatok, például tulajdonság értékek toohelp szűrheti vagy más-más részhalmazához nyomkövetési keresési adhat hozzá. Példa:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

A [keresési](app-insights-diagnostic-search.md), majd könnyen szűrhetők összes köszönőüzenetei egy adott súlyossági szint tooa adott adatbázis is.


### <a name="traces-in-analytics"></a>Nyomok Analytics

A [Application Insights Analytics](app-insights-analytics.md), tooTrackTrace megjelenítése hívja a hello `traces` tábla.

Ha [mintavételi](app-insights-sampling.md) működik, hello elemek tulajdonság jeleníti meg egy értéket 1-nél nagyobb. Példa az elemek száma a == 10 azt jelenti, hogy 10 meghívja túl`trackTrace()`, hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét. a megfelelő nyomkövetési indított hívások száma tooget, kell ezért kódot használja, mint `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency
Használjon hello TrackDependency hívja tootrack hello válaszidejét és sikerességi arányát hívások tooan külső kódnak küldött kódot. hello eredmények hello hello portál függőségi diagramjain jelennek meg.

```C#
var success = false;
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
try
{
    success = dependency.Call();
}
finally
{
    timer.Stop();
    telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
}
```

Ne feledje SDK tartalmazza hello kiszolgálón egy [függőségi modul](app-insights-asp-net-dependencies.md) , amely észleli, és bizonyos függőségi hívások automatikusan – például toodatabases és REST API-k nyomon követi. Lehetősége van egy ügynök a kiszolgáló toomake hello modul tooinstall működik. Ha automatikus követési hello tootrack hívások nem dolgozza fel, vagy ha nem szeretné, hogy tooinstall hello ügynök használhatja a hívást.

hello normál függőségi követése modul ki tooturn szerkesztése [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) és hello hivatkozás törlése túl`DependencyCollector.DependencyTrackingTelemetryModule`.

### <a name="dependencies-in-analytics"></a>Az elemzés függőségek

A [Application Insights Analytics](app-insights-analytics.md), hello megjelennek trackDependency hívások `dependencies` tábla.

Ha [mintavételi](app-insights-sampling.md) működik, hello elemek tulajdonság jeleníti meg egy értéket 1-nél nagyobb. Példa az elemek száma a == 10 azt jelenti, hogy a 10 hívások tootrackDependency() hello mintavételi folyamat csak akkor továbbítódnak, ezek egyikét. tooget függőségek megfelelő száma szegmentált cél összetevő, például a kód használata:

```
dependencies | summarize sum(itemCount) by target
```

a kapcsolódó kérések tooassociate függőségek használjuk:

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a>Könyvelési adatok
Általában a hello SDK néha kiválasztott toominimize hello gyakorolt hello felhasználói adatok küldése. Azonban bizonyos esetekben érdemes tooflush hello puffer – például egy alkalmazás, amely leállítja SDK hello használata.

*C#*

    telemetry.Flush();

    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(1000);

Vegye figyelembe, hogy hello függvény hello az aszinkron [server telemetriai csatorna](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

## <a name="authenticated-users"></a>Hitelesített felhasználók
A webes alkalmazás a felhasználók (alapértelmezés) azonosítja a cookie-k. Ha az alkalmazás hozzáférésük egy másik számítógép vagy a böngésző, vagy ha ezek a cookie-k törléséhez előfordulhat, hogy számba lehet vennni a felhasználó egynél többször.

A felhasználók bejelentkeznek tooyour alkalmazást, ha kaphat a pontosabb számát úgy, hogy hitelesített hello Felhasználóazonosító hello böngésző kódban:

*JavaScript*

```JS
// Called when my app has identified hello user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

Az ASP.NET-webalkalmazás MVC alkalmazás, például:

*RAZOR*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Nem szükséges toouse hello felhasználói tényleges bejelentkezési nevet. Csak rendelkezik toobe, amely egyedi toothat felhasználói Azonosítóját. Nem tartalmazhatnak szóközöket és hello karakterek `,;=|`.

hello felhasználói azonosító is állítsa be a munkamenet-cookie-ban, és toohello kiszolgáló küldött. Ha hello server SDK telepítve van, a hello hitelesített felhasználói azonosító hello környezeti tulajdonságok az ügyfél- és telemetria részeként küldött. Majd végezhet, és keresse meg azt.

Ha az alkalmazás felhasználók csoportosítja a fiókok, is átadhatja hello fiók azonosítója (a hello ugyanaz a karakter korlátozások).

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

A [Metrikaböngésző](app-insights-metrics-explorer.md), létrehozhat egy diagram, amely megjeleníti **hitelesített felhasználók,**, és **felhasználói fiókok**.

Emellett [keresési](app-insights-diagnostic-search.md) az ügyfél adatpontok adott felhasználói neveket és fiókok.

## <a name="properties"></a>Szűrés, keresést és az adatok példájaként tulajdonságok felhasználásával
Tulajdonságok és mérések tooyour események (és is toometrics, Lapmegtekintések, kivételeket és egyéb telemetriai adatokat) csatolhat.

*Tulajdonságok* használható toofilter a telemetriai adatokat a használati jelentésekben hello karakterlánc-értékek. Például ha az alkalmazás biztosít több játékok, csatolhat hello játék tooeach esemény hello neve, hogy láthassa, melyik játékok népszerűbbnek.

A karakterlánc hossza hello 8192 korlátozva van. (Ha azt szeretné, hogy toosend nagy méretű adattömböket írnak, használja a hello üzenet paramétert a [TrackTrace](#track-trace).)

*Metrikák* számértékek jelenítheti meg grafikusan. Érdemes például toosee fokozatos növekedése hello eredmények, amelyek a gamers elérése esetén. hello grafikonon is lehet szegmentált által küldött hello eseménnyel, hogy megkaphassa tulajdonságok külön hello, vagy a halmozott diagramok a különböző játékok számára.

A metrika értékek toobe megfelelően jelenik meg akkor nagyobb vagy egyenlő too0 kell lennie.

Van azonban néhány [tulajdonságait, a tulajdonságértékeket és a metrikák hello száma vonatkozó korlátozások](#limits) használható.

*JavaScript*

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


*C#*

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics);


*Visual Basic*

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send hello event:
    telemetry.TrackEvent("WinGame", properties, metrics)


*Java*

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> Mi gondoskodunk nem toolog személyazonosításra alkalmas adatok tulajdonságait.
>
>

*Ha követte a metrikák*, nyissa meg a Metrikaböngésző, majd válasszon hello metrika hello **egyéni** csoport:

![Nyissa meg a Metrikaböngésző, jelölje be hello diagram, és válassza ki a hello metrika](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> Ha a metrika nem jelenik meg, vagy ha hello **egyéni** fejléc nem létezik, Bezárás hello kijelölés panelt, és próbálkozzon újra később. Metrikák néha időigényes egy órával toobe összesítve hello-feldolgozási folyamaton keresztül.

*Ha követte a tulajdonságok és a metrikák*, hello metrika szegmentálja hello tulajdonság:

![Állítsa be a csoportosítás, és válassza a hello tulajdonság szerinti csoportosítás](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

*A diagnosztikai keresési*, megtekintheti a hello tulajdonságok és az esemény egyedi előfordulása.

![Válasszon ki egy példányt, és adja meg a "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Használjon hello **keresési** mezőben toosee esemény eseményeket, amelyek egy adott tulajdonság értéke.

![Írja be a keresőkifejezést keresési rendszerbe](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

[További információ a keresési kifejezések](app-insights-diagnostic-search.md).

### <a name="alternative-way-tooset-properties-and-metrics"></a>Alternatív módot tooset tulajdonságok és metrikák
Sokkal kényelmesebb, ha egy külön objektumban esemény hello paraméterek hozhatja létre:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> Ne használja fel a hello telemetriai elem példányt (`event` ebben a példában) toocall Track*() több alkalommal. Emiatt a helytelen konfiguráció küldött telemetriai toobe.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Egyéni mértékek és az elemzés tulajdonságok

A [Analytics](app-insights-analytics.md), egyéni metrikákkal és tulajdonságok megjelenítése hello `customMeasurements` és `customDimensions` telemetriai rekordokban attribútumait.

Például hozzáadta "játék" tooyour – kéréstelemetria nevű tulajdonságot, ha a lekérdezés különböző értékek "játék" hello előfordulását száma, és egyéni metrika "pontszám" hello hello átlaga megjelenítése:

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

Figyelje meg, hogy:

* Egy érték hello customDimensions vagy customMeasurements JSON kibontásakor dinamikus típust, és úgy kell alakítania azt `tostring` vagy `todouble`.
* hello lehetőségét figyelembe tootake [mintavételi](app-insights-sampling.md), használjon `sum(itemCount)`, nem `count()`.



## <a name="timed"></a>Időzítési események
Néha szükség toochart mennyi ideig tart tooperform a műveletet. Például érdemes tooknow mennyi ideig felhasználók el egy játékban tooconsider lehetőségeket. Ehhez használhatja hello mérési paraméter.

*C#*

    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform hello timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send hello event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);



## <a name="defaults"></a>Egyéni telemetria alapértelmezett tulajdonságai
Ha tooset alapértelmezett tulajdonságértékek egyes hello egyéni események írást, akkor TelemetryClient példányt teheti meg. Csatolt tooevery telemetriai elemet, hogy az ügyfél által küldött.

*C#*

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame");

*Visual Basic*

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with hello context property:
    gameTelemetry.TrackEvent("WinGame")

*Java*

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");



Egyéni telemetria hívások hello az alapértelmezett érték a tulajdonság szótár is felülírására.

*JavaScript a webes ügyfelek*, [JavaScript telemetriai inicializálók használja](#js-initializer).

*tooadd tulajdonságok tooall telemetriai*, beleértve a szabványos gyűjtési modulok hello adatait [megvalósítása `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>A mintavételi, szűrési és telemetriai adatainak feldolgozása
Írhat kódot tooprocess a telemetriai adatok hello SDK való továbbítás előtt. hello feldolgozási hello szabványos telemetriai modulok, például HTTP kérelem adatgyűjtési és -függőség gyűjtemény által küldött adatokat tartalmazza.

[Adja hozzá a Tulajdonságok](app-insights-api-filtering-sampling.md#add-properties) implementálásával tootelemetry `ITelemetryInitializer`. Például hozzáadhat verziószámok vagy számított értékeket más tulajdonságai közül.

[Szűrés](app-insights-api-filtering-sampling.md#filtering) módosíthatja vagy elveti a telemetriai adatok elküldés előtt a hello SDK implementálásával `ITelemetryProcesor`. Megadhatja, mi küldik vagy elvetett, de a metrikákat hello hatással a tooaccount rendelkezik. Attól függően, hogy hogyan elveti elemek elveszhetnek hello képességét toonavigate kapcsolódó elemek között.

[A mintavételi](app-insights-api-filtering-sampling.md) csomagolt megoldás tooreduce hello kötet az alkalmazás toohello portál által küldött adatokat. Igen, az nem befolyásolja a megjelenő hello metrikákat. És az hajtja végre, például kivételek, a kérelmek és az oldalmegtekintéseket kapcsolódó elemek közötti navigáljon a képes toodiagnose problémák befolyásolása nélkül.

[További információk](app-insights-api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Telemetria letiltása
túl*dinamikusan leállítására és elindítására* hello összegyűjtése és telemetriai adatok továbbítása:

*C#*

```C#

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

túl*tiltsa le a kiválasztott szabványos gyűjtők*– például teljesítményszámlálókat, HTTP-kérelmek vagy függőségek--törlése vagy hello megfelelő sorok megjegyzéssé [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Ehhez, például ha toosend saját TrackRequest adatokat.

## <a name="debug"></a>Fejlesztői mód
Hibakeresés során a rendszer hasznos toohave a telemetriai adatok sürgős hello-feldolgozási folyamaton keresztül, hogy az eredmények azonnal láthatók. Akkor is get további üzeneteket, amelyek segítségével nyomon követni a hello telemetriai problémákat. Kapcsolja ki a termelési, mert az alkalmazás lassíthatja.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*Visual Basic*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="ikey"></a>A kijelölt egyéni telemetria hello instrumentation kulcs beállítása
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="dynamic-ikey"></a>Dinamikus instrumentation kulcs
felfelé telemetriai fejlesztési, tesztelési és éles környezetben, a keverése tooavoid is [hozzon létre külön Application Insights-erőforrások](app-insights-create-new-resource.md) és azok kulcsait hello környezettől függően módosítsa.

Helyett hello instrumentation kulcs lekérése hello konfigurációs fájlt, beállíthatja a kódban. Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból hello kulcs meg:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

*JavaScript*

    appInsights.config.instrumentationKey = myKey;



A weboldalakon, érdemes lehet a tooset azt hello webkiszolgáló állapota, nem pedig szó kódolási hello parancsfájlba. Például a egy weblap ASP.NET alkalmazás hozott létre:

*JavaScript Razor*

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="telemetrycontext"></a>TelemetryContext
TelemetryClient rendelkezik egy környezeti tulajdonság, amely az összes telemetriai adatokkal együtt küldött értékeket tartalmaz. Általában állította hello szabványos telemetriai modulok, de is beállíthatja azokat saját maga. Példa:

    telemetry.Context.Operation.Name = "MyOperationName";

Ha ezek bármelyike saját magának, fontolja meg a hello a megfelelő sor eltávolítása [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), így a és hello szabványos értékek nem Zavarba.

* **Az összetevő**: hello alkalmazást és annak verzióját.
* **Eszköz**: hello alkalmazást futtató hello eszközzel kapcsolatos adatok. (A webalkalmazásokban, ez a hello vagy hello telemetriai által küldött ügyfél-eszközkövetelmények.)
* **InstrumentationKey**: hello Application Insights-erőforrást, ahol a hello telemetriai adatok jelennek meg az Azure-ban. Ez általában felvételre az ApplicationInsights.config.
* **Hely**: hello hello eszköz földrajzi helye.
* **A művelet**: A webalkalmazásokban, hello aktuális HTTP-kérelem. Más típusú alkalmazás állíthat be a toogroup események együtt.
  * **Azonosító**: egy generált érték különböző események hibához, így minden esetben a diagnosztikai keresési vizsgálja meg, ha található kapcsolódó elemeket.
  * **Név**: azonosítót, általában hello URL-cím hello HTTP-kérelem.
  * **SyntheticSource**: Ha nem null értékű vagy üres, karakterlánc, amely azt jelzi, hogy hello adatforrás hello kérelem néven azonosított egy robot vagy webes tesztet. Alapértelmezés szerint az ki lesz zárva a Metrikaböngészőben számításból.
* **Tulajdonságok**: az összes telemetriai adatokat küldött tulajdonságok. Az egyes követése * hívások felülbírálható.
* **Munkamenet**: hello felhasználói munkamenet. hello azonosító értéke tooa generált érték, amely megváltozik, miközben hello felhasználó nem volt aktív egy ideig.
* **Felhasználói**: felhasználói adatokat.

## <a name="limits"></a>Korlátok
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

Szerezze meg a hello adatok sávszélesség-korlátjának, használjon tooavoid [mintavételi](app-insights-sampling.md).

toodetermine hogyan tartják hosszú adatokat, lásd: [az adatmegőrzés és az adatvédelmi](app-insights-data-retention-privacy.md).

## <a name="reference-docs"></a>Segédanyagok
* [ASP.NET-hivatkozás](https://msdn.microsoft.com/library/dn817570.aspx)
* [Java dokumentáció](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [JavaScript-hivatkozás](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK-kód
* [Az ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [Windows Server csomagok](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)
* [Összes platform](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Kérdések
* *Milyen kivételek előfordulhat, hogy throw Track_() hívások?*

    nincs. Toowrap nem kell őket try-catch záradékban. Ha hello SDK problémákat tapasztal, üzenetek naplózza a hello hibakeresési konzol kimeneti és – ha hello üzenetek lekérése keresztül--diagnosztikai keresési.
* *Van egy REST API tooget adatok hello portálról?*

    Igen, hello [adatelérési API](https://dev.applicationinsights.io/). Más módokon tooextract adatok közé tartoznak a [Analytics tooPower BI exportálása](app-insights-export-power-bi.md) és [a folyamatos exportálás](app-insights-export-telemetry.md).

## <a name="next"></a>Következő lépések
* [Keresési események és a naplókat](app-insights-diagnostic-search.md)

* [hibaelhárítással](app-insights-troubleshoot-faq.md)


