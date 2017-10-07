---
title: "az Azure Application Insights keresztül Analytics aaaA bemutató |} Microsoft Docs"
description: "Az összes rövid minták hello Analytics, az Application Insights hello hatékony keresési eszköz fő lekérdezések."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/06/2017
ms.author: bwren
ms.openlocfilehash: c268e26c6bf93ac2ee2a9d5e83613150dcf90b04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Az Application Insightsban Analytics bemutatása
[Elemzés](app-insights-analytics.md) hello hatékony keresési funkciója [Application Insights](app-insights-overview.md). Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.

* **[Hello bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás még nem küld adatokat tooApplication Insights.
* **[SQL-felhasználók lap cheat](https://aka.ms/sql-analytics)**  hello leggyakoribb idioms lefordítja.

Vegyünk egy lépésein végighaladva néhány alapvető lekérdezések tooget-t elindította.

## <a name="connect-tooyour-application-insights-data"></a>Csatlakozás tooyour Application Insights adatainak
Nyissa meg az alkalmazás Analytics [áttekintése panel](app-insights-dashboards.md) az Application Insightsban:

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a>[Igénybe](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): n sorok megjelenítése
Felhasználói műveletek (általában HTTP-kérelmekre a webes alkalmazás által fogadott) jelentkezzen adatpontok tárolódnak a következő táblába `requests`. Minden egyes sorára az egy telemetriai adat hello Application Insights SDK az alkalmazás kapott.

Először néhány minta sorainak hello vizsgálata:

![eredmények](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Helyezze el hello kurzor valahol hello utasítás előtt Ugrás gombra. Egy utasítás is átnyúlik egynél több sort, de nem üres sorok be egy utasítást. Üres sorai egy kényelmes módszert arra tookeep több különálló lekérdezések hello ablakban.
>
>

Oszlopok kiválasztása, egérrel húzza őket, oszlop, csoport, és szűréséhez:

![Kattintson a felső jobb eredmények oszlop kiválasztása](./media/app-insights-analytics-tour/030.png)

Bármely elem toosee hello részleteinek kibontásához:

![Kattintson a táblázat, és használja az oszlopok konfigurálása](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Kattintson egy oszlop toore magasrendű hello érhetők el az eredmények hello webböngészőben hello vezetője. De vegye figyelembe, hogy a nagy eredményhalmazt sorok letöltött toohello böngésző hello száma korlátozott. Ezzel a módszerrel rendezés nem mindig mutatja hello tényleges legmagasabb vagy legalacsonyabb elemek. toosort elemek megbízhatóan, használja a hello `top` vagy `sort` operátor.
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a>[Felső](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) és [rendezés](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)
`take`hasznos tooget gyors mintát eredményt, de az nem meghatározott sorrendben hello tábla azon sorait jeleníti meg. egy rendezett nézet tooget használja `top` (a minta) vagy `sort` (keresztül hello teljes tábla).

Hello első n sorát, adott oszlop szerint rendezve jelenjenek meg:

```AIQL

    requests | top 10 by timestamp desc
```

* *Szintaxis:* legtöbb operátorok rendelkeznek kulcsszó paraméterek például `by`.
* `desc`csökkenő sorrendben = `asc` = növekvő.

![](./media/app-insights-analytics-tour/260.png)

`top...`További performant módszer megkapta a `sort ... | take...`. A Microsoft sikerült írt:

```AIQL

    requests | sort by timestamp desc | take 10
```

hello eredmény lenne hello azonos, de egy kicsit lassabban fog futni. (Is létrehozható `order`, ez az alias az `sort`.)

hello oszlopfejlécek hello tábla nézetben is használt toosort hello eredmények üdvözlő képernyőt. De működés során, ha már használta a `take` vagy `top` tooretrieve egy tábla csak egy részét, lesz csak sorrendjének hello rekordok már beolvasott.

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a>[Ha](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): szűrési feltétel

Nézzük meg, csak kérelmeket, amelyek egy adott eredmény, hibakód:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

Hello `where` operátor egy logikai kifejezés vesz igénybe. Az alábbiakban néhány kulcsfontosságú pont róluk:

* `and`, `or`: Logikai operátorok
* `==`, `<>`, `!=` : egyenlő, és nem egyenlő
* `=~`, `!~` : egyenlő és nem egyenlő azonban nem karakterlánc. Számos további karakterlánc-összehasonlítási operátorok.

<!---Read all about [scalar expressions]().--->

### <a name="getting-hello-right-type"></a>Hello megfelelő típusú beolvasása
Sikertelen kérelmek keresése:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Time

Alapértelmezés szerint a lekérdezések nincsenek korlátozott toohello utolsó 24 órában. Azonban módosíthatja ezt a tartományt:

![](./media/app-insights-analytics-tour/change-time-range.png)

Bírálja felül a hello időtartomány írásával, amely akkor említi lekérdezés `timestamp` a where záradékban. Példa:

```AIQL

    // What were hello slowest requests over hello past 3 days?
    requests
    | where timestamp > ago(3d)  // Override hello time range
    | top 5 by duration
```

hello idő tartomány szolgáltatása egyenértékű tooa "where" záradék után minden hashtagként hello forrástáblákból közül az egyik beszúrni.

`ago(3d)`azt jelenti, hogy a "három nappal ezelőtt". Más időegységekkel óra közé tartozik (`2h`, `2.5h`), a perc (`25m`), és a másodperc (`10s`).

További példák:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

```

[Dátumokat és időpontokat hivatkozás](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a>[Projekt](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): válassza ki, nevezze át és számítási oszlopok
Használjon [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick csak a kívánt oszlopok hello:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Nevezze át az oszlopok is, és újakat megadása:

```AIQL

    requests
    | top 10 by timestamp desc
    | project  
            name,
            response = resultCode,
            timestamp,
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![eredménye](./media/app-insights-analytics-tour/270.png)

* Oszlopnevek szóközöket is tartalmazhatnak, vagy azok vannak zárójeles Ha szimbólumokat, ez például: `['...']` vagy`["..."]`
* `%`hello szokásos moduló operátor van.
* `1d`(Ez egy számjegy, majd egy kellett ") literális timespan tehát az egy nap. Az alábbiakban néhány további timespan-szövegkonstans: `12h`, `30m`, `10s`, `0.01s`.
* `floor`(alias `bin`) kerekítése a legközelebbi többszörösére hello alapértéke megadta a toohello le értéket. Ezért `floor(aTime, 1s)` kerekítése a legközelebbi második toohello le egyszerre.

Kifejezések lehetnek összes hello szokásos operátorok (`+`, `-`,...), és számos különböző hasznos funkciók.

## <a name="extend"></a>Bővíthető
Ha csak tooadd oszlopok toohello meglévők, használja [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Használatával [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) kevésbé részletes, mint [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) Ha azt szeretné, hogy tookeep összes hello meglévő oszlopokat.

### <a name="convert-toolocal-time"></a>Átalakítás toolocal idő

Időbélyeg helyi időre mindig UTC formátumban vannak. Így ha Ön hello Velünk csendes-óceáni part és téli, akkor célszerű lehet ez:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a>[Összefoglalója](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): sorcsoportra összesítése
`Summarize`alkalmazza a megadott *aggregátumfüggvény* keresztül sorcsoportra.

Például a webes alkalmazás szükséges toorespond tooa kérelem hello idő hello mezőben van jelentett `duration`. Nézzük meg hello átlagos válasz tooall kérelmek idő:

![](./media/app-insights-analytics-tour/410.png)

Vagy a kérelmek különböző neveket azt sikerült külön hello eredmény:

![](./media/app-insights-analytics-tour/420.png)

`Summarize`gyűjti az adatpontok hello adatfolyam hello csoportokba tartozó mely hello `by` záradék egyaránt értékelődik ki. Minden érték hello `by` kifejezés - minden művelet neve a fenti példa hello - hello eredménytáblájában egy sort eredményez.

Vagy azt sikerült eredmények csoportosítás időpontja:

![](./media/app-insights-analytics-tour/430.png)

Figyelje meg, hogyan használunk hello `bin` funkciót (más néven `floor`). Ha éppen most használt `by timestamp`, mindegyik bemeneti sorában végül volna a saját kis csoport. Bármely folyamatos skaláris a hasonló alkalommal vagy számok tudunk toobreak hello folyamatos tartomány diszkrét értékei kezelhető számmá. `bin`-Ez csupán hello ismerős kerekítési lefelé `floor` működik – a hello legegyszerűbb módja toodo, amely.

Így azonos módszerrel tooreduce tartományok karakterláncok hello:

![](./media/app-insights-analytics-tour/440.png)

Figyelje meg, hogy használható `name=` olyan eredményoszlopot hello összesítő kifejezések vagy hello által záradék tooset hello nevét.

## <a name="counting-sampled-data"></a>Leltár adatminta
`sum(itemCount)`az ajánlott összesítési toocount események hello. Sok esetben az elemek száma 1, ==, így hello függvény egyszerűen hello számú hello csoport sorainak megszámlálása. De ha [mintavételi](app-insights-sampling.md) van műveletben, csak töredéke alatt hello eredeti események kerülnek, az Application Insights adatpontok úgy, hogy az egyes adatpontokban látja, `itemCount` események.

Például, ha a mintavételi elveti 75 %-a hello eredeti eseményeket, majd az elemek száma == 4 megőrzi hello rekordokban - Ez azt jelenti, hogy megtartott rekordot, volt négy eredeti rögzíti.

Adaptív mintavételi hatására az elemek száma toobe magasabb időszakban történjenek, amikor az alkalmazás fokozottan használatban van.

Ezért az elemek száma összegzése biztosít a helyes becslése hello eredeti események száma.

![](./media/app-insights-analytics-tour/510.png)

Szerepel továbbá egy `count()` összesítési (és a count művelet), olyan esetekben, ahol valóban szeretné, hogy a csoportban található sorok számát toocount hello.

Nincs számos [aggregátumfüggvényeket](https://docs.loganalytics.io/learn/tutorials/aggregations.html).

## <a name="charting-hello-results"></a>Diagramkészítési hello eredmények
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Alapértelmezés szerint eredmények táblázatos megjelenítéséhez:

![](./media/app-insights-analytics-tour/225.png)

Tudnánk mint hello tábla nézet. Nézzük hello eredmények hello diagram nézetben hello függőleges vonal beállítással:

![Diagram, kattintson, majd válassza ki a függőleges sávdiagram, és rendelje hozzá x és y tengely](./media/app-insights-analytics-tour/230.png)

Figyelje meg, hogy bár a Microsoft nem hello eredmények idő szerint (ahogy látja, a hello tábla megjelenítési) rendezéséhez hello diagram megjelenítési mindig látható időpontok megfelelő sorrendben.


## <a name="timecharts"></a>Timecharts
Események nincs mutatja óránként:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Hello diagram megjelenítési beállítást:

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Több sorozat
Több kifejezést hello `summarize` záradékban több oszlop hoz létre.

Több kifejezést hello `by` záradékban több sort, egy a minden értékkombinációja hoz létre.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tábla kérelmek óra és hely szerint](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>A diagram szegmentálja dimenziók
Ha a diagram táblának karakterlánc oszlopot és egy numerikus oszlopot, hello karakterlánc lehet használt toosplit hello numerikus adatokat külön sorozat pontok. Ha egynél több oszlophoz, dönthet úgy, mely oszlop toouse hello Diszkriminátor szerint.

![Egy diagramra szegmens](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Golflabda arány

Egy logikai tooa karakterlánc toouse alakítsa át azt egy Diszkriminátor:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Több metrikák megjelenítése
Ha a diagramtípus egynél több numerikus oszlopot, táblának a hozzáadása toohello timestamp, azok bármilyen kombinációját jelenítheti meg.

![Egy diagramra szegmens](./media/app-insights-analytics-tour/110.png)

Ki kell választania **nem osztott** több numerikus oszlop kiválasztása előtt. Meg nem osztható fel: hello karakterlánc oszlop szerint azonos időben is csak egy numerikus oszlopot megjelenítése.

## <a name="daily-average-cycle"></a>Napi átlagos ciklus
Hogyan változik a használati hello átlagos napon keresztül?

Count kérések hello idő moduló egy nap munkaidőre binned:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Az órát egy átlagos napon belül vonaldiagram](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Figyelje meg, hogy jelenleg tudunk tooconvert idő időtartamok toodatetimes a rendelés toodisplay a grafikont.
>
>

## <a name="compare-multiple-daily-series"></a>Több napi sorozat összehasonlítása
Hogyan használati változik a nap hello idővel a különböző országokban?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Vegyes által client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Egy terjesztési ábrázolása
Hány munkamenetek vannak-e különböző hosszúságú?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

hello utolsó sora szükség tooconvert toodatetime. Jelenleg egy diagram hello x tengelyének jelenik meg skaláris csak akkor, ha egy DateTime típusú érték.

Hello `where` záradék nem tartalmazza a végeredménye munkamenetek (sessionDuration == 0) és a készletek hello hello x tengely hosszát.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[Százalékos érték](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
Milyen tartományok időtartamok fedik le a munkamenetek különböző százalékának?

Lekérdezés fent hello használja, de hello utolsó sora cserélje le:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s)
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

Azt is eltávolítani hello felső korlátja hello, ahol a következő sorrendben tooget záradékában javítsa egynél több kérelemmel összes munkameneteket is beleértve:

![eredménye](./media/app-insights-analytics-tour/180.png)

Ahol láthatja, hogy:

* a munkamenetek 5 % rendelkezik a kevesebb mint 3 perc alatt 34s; időtartama
* 50 %-a munkamenetek utolsó 36 percnél kevesebb;
* az utolsó 7 napnál 5 %-a munkamenetek

külön részletes információkat az egyes országok tooget, csak tudunk toobring hello client_CountryOrRegion oszlop külön-külön is keresztül operátorok összegzése:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id, client_CountryOrRegion
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s), client_CountryOrRegion
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="join"></a>Csatlakozás
Hozzáférés tooseveral táblázatokat, beleértve a kérelmek és kivételek van.

toofind hello kivételek kapcsolódó tooa kérelem által visszaadott hibaválaszt, azt is csatlakoztatás hello táblák `session_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Akkor célszerű toouse `project` tooselect csak hello oszlopok végrehajtása előtt kell hello illesztési.
A hello azonos záradékban, a Microsoft hello Timestamp típusú oszlop átnevezése.

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-tooa-variable"></a>[Így](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): rendelni egy eredmény tooa változó

Használjon `let` tooseparate kimenő hello előző kifejezés hello részeit. hello eredmények ugyanazok maradtak, mint:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> Hello Analytics ügyfél put nem üres sorok hello lekérdezés hello részei között. Győződjön meg arról, hogy tooexecute minden elemét.
>

Használjon `toscalar` tooconvert egyetlen tábla cella tooa értéket:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>Functions

Használjon *segítségével* toodefine függvény:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a>Beágyazott objektumok elérése
A beágyazott objektumok könnyen elérhetők. Például a hello kivételek adatfolyam látható strukturált objektumok ehhez hasonló:

![eredménye](./media/app-insights-analytics-tour/520.png)

Kíváncsiak vagyunk hello tulajdonságok kiválasztásával konvertálhatók:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Ne feledje, hogy toocast hello toohello megfelelő típusú.


## <a name="custom-properties-and-measurements"></a>Egyéni tulajdonságok és mérések
Ha az alkalmazás csatol [egyéni dimenzió (Tulajdonságok) és egyéni mérték](app-insights-api-custom-events-metrics.md#properties) tooevents, akkor megtekintheti azokat a hello `customDimensions` és `customMeasurements` objektumok.

Ha például az alkalmazás tartalmazza:

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

tooextract Analytics ezeket az értékeket:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast tooexpected type

```

tooverify egy egyéni dimenzió egy adott típusú van-e:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a>Irányítópultok
Az eredmények tooa irányítópult rendelés toobring rögzíthető együtt minden a legfontosabb diagramok és táblák.

* [Az Azure megosztott irányítópult](app-insights-dashboards.md#share-dashboards): hello PIN-kód ikonra. Mielőtt ezt megtehetné, egy megosztott irányítópult kell rendelkeznie. Hello Azure-portálon, a nyissa meg, vagy hozzon létre egy irányítópultot, és kattintson a megosztás.
* [A Power BI-irányítópulton](app-insights-export-power-bi.md): kattintson az exportálni, a Power BI-lekérdezést. Ez a megoldás előnye, hogy a lekérdezés más a mellett számos információforrás jelenítheti meg.

## <a name="combine-with-imported-data"></a>Az importált adatok kombinálása

Elemzési jelentések remekül hello irányítópulton, de előfordulhat tootranslate hello adatok tooa további emészthető űrlap. Tegyük fel, hogy a hitelesített felhasználók alias hello telemetriai k azonosítását. A valós nevek tooshow szeretné az eredmények között. toodo, egy CSV-fájl, amely leképezhető a hello aliasok toohello valódi neve van szüksége.

Importálhatja egy adatfájlt és csakúgy, mint a hagyományos táblázatokban hello (kérelmeket, kivételek, és így tovább) használja. Vagy lekérdezni az önállóan, vagy más táblák csatlakozzon hozzá. Például, ha a usermap nevű tábla, és nem rendelkezik-e oszlopok `realName` és `userId`, akkor használhatja azt tootranslate hello `user_AuthenticatedId` hello – kéréstelemetria mezőbe:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get hello realName field from hello usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

egy tábla hello séma panelen tooimport alatt **egyéb adatforrások**, hello utasításokat tooadd új adatforrás, kövesse az adatok minta feltöltésével. Ezután a definíció tooupload táblák is használhatja.

hello importálás funkció jelenleg előzetes, ezért kezdetben megjelenik az "Egyéb adatforrások." ", lépjen velünk kapcsolatba" hivatkozás A toosign toohello preview program használja, és hello hivatkozás majd váltja fel az "Új adatforrás hozzáadása" gombra.


## <a name="tables"></a>Táblák
az alkalmazástól fogadott telemetriai hello adatfolyam több táblázatot keresztül érhető el. minden tábla rendelkezésre álló tulajdonságok hello sémája hello bal hello ablak akkor jelenik meg.

### <a name="requests-table"></a>Kérelmek tábla
Count HTTP-kérelmek tooyour webalkalmazás és a szegmens lapnév szerint:

![Count kérelmek szegmentált név szerint](./media/app-insights-analytics-tour/analytics-count-requests.png)

A legtöbb teljesítő hello kérelmek keresése:

![Count kérelmek szegmentált név szerint](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Egyéni események tábla
Ha [trackevent() függvény](app-insights-api-custom-events-metrics.md#trackevent) toosend saját események áttekintheti azokat ebből a táblázatból.

Vegyünk egy példát, amelyben az alkalmazás kódját tartalmazza a sorokat:

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Ezek az események gyakorisága hello megjelenítése:

![Egyéni események megjelenítése aránya](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Mérések és dimenziókat kinyerése hello események:

![Egyéni események megjelenítése aránya](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Egyéni metrikák tábla
Ha használ [trackmetric() függvény](app-insights-api-custom-events-metrics.md#trackmetric) toosend saját metrika értékeket, látni fogja az eredményeket a hello **customMetrics** adatfolyam. Példa:  

![Egyéni metrikákat az Application Insights elemzés](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> A [Metrikaböngésző](app-insights-metrics-explorer.md), telemetriai csatolt tooany típusú szerepelnek együtt hello metrikák panelről és metrikák használatával küldött összes egyéni mérték `TrackMetric()`. De Analytics, egyéni mértékek még csatolt toowhichever telemetriai azok zajlott - események vagy kéréseket, és így tovább -, amíg TrackMetric által küldött metrikák jelennek meg a saját adatfolyam típusú.
>
>

### <a name="performance-counters-table"></a>Teljesítmény számlálók tábla
[Teljesítményszámlálók](app-insights-performance-counters.md) láthat rendszer alapvető metrikákat az alkalmazás, például a Processzor, memória, és a hálózathasználat. Hello SDK toosend számlálókat, beleértve a saját egyéni számlálók konfigurálhatja.

Hello **performanceCounters** séma közzététele hello `category`, `counter` nevét, és `instance` minden teljesítményszámláló nevét. Teljesítményszámláló-példányok nevei csak a megfelelő toosome teljesítményszámlálókat, és általában azt jelzik, hello folyamatok toowhich hello számának hello neve vonatkozik. Hello telemetriai minden alkalmazáshoz az alkalmazás csak hello számlálók látható. Például toosee milyen számlálók érhetők el:

![Az Application Insights analytics teljesítményszámlálók](./media/app-insights-analytics-tour/analytics-performance-counters.png)

a kijelölt időszak tooget a diagram a hello keresztül a rendelkezésre álló memória:

![Az Application Insights Analytics memória timechart](./media/app-insights-analytics-tour/analytics-available-memory.png)

Egyéb telemetriai adatokat, például **performanceCounters** egy olyan oszlop is van `cloud_RoleInstance` azt jelzi, hogy hello hello gazdagépet, amelyen fut az alkalmazás identitását. Például toocompare hello az alkalmazás teljesítményével kapcsolatos különböző gépeken hello:

![Az Application Insights Analytics szerepkör példánya szegmentált teljesítmény](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Kivételek tábla
[Az alkalmazás által jelentett kivételek](app-insights-asp-net-exceptions.md) érhetők el ebben a táblában.

toofind hello az alkalmazás lett kezelése, ha hello kivételt észlelt HTTP-kérelem, operation_Id csatlakoztatás:

![Csatlakoztatás operation_Id kivételek kérések](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Időzítés tábla Browser
`browserTimings`a felhasználók böngészőjének gyűjtött lapbetöltési adatainak megjelenítése.

[Állítsa be az alkalmazás ügyféloldali telemetriai](app-insights-javascript.md) a rendezés toosee metrikákat.

hello séma tartalmaz [hello lap betöltési folyamat különböző szakaszaiban hello hosszának jelző metrikák](app-insights-javascript.md#page-load-performance). (A felhasználók, olvassa el a lap hello időtartamot nem utal.)  

Hello popularities különböző oldalak megjelenítése, és az összes lapon alkalommal betöltése:

![Lapbetöltési idők Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Rendelkezésre állási eredmények táblázatában
`availabilityResults`azt mutatja be hello eredményeit a [webalkalmazás-tesztek](app-insights-monitor-web-app-availability.md). Minden egyes teszt helyen a teszt futtatásakor a külön-külön jelenti.

![Lapbetöltési idők Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Függőségek tábla
Tartalmazza a hívás eredményeit, hogy az alkalmazás lehetővé teszi a toodatabases és a REST API-k és más tooTrackDependency() hívja. Hello böngészőből AJAX-hívások is tartalmaz.

AJAX-hívások hello böngészőből:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Hello kiszolgálóról függőségi hívások esetében:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Kiszolgálóoldali függőségi mindig eredményben `success==False` Ha hello Application Insights-ügynök nincs telepítve. Azonban hello más adatok megfelelőek-e.

### <a name="traces-table"></a>Nyomkövetések tábla
Tartalmazza az TrackTrace(), használó alkalmazás által küldött hello telemetriai vagy [más naplózási keretrendszer](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Videó 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Speciális lekérdezéseket:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Következő lépések
* [Elemzés nyelvi referencia](app-insights-analytics-reference.md)
* [SQL-felhasználók lap cheat](https://aka.ms/sql-analytics) hello leggyakoribb idioms lefordítja.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
