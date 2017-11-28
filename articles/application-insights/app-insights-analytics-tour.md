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
# <a name="a-tour-of-analytics-in-application-insights"></a><span data-ttu-id="79be6-103">Az Application Insightsban Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="79be6-103">A tour of Analytics in Application Insights</span></span>
<span data-ttu-id="79be6-104">[Elemzés](app-insights-analytics.md) hello hatékony keresési funkciója [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79be6-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="79be6-105">Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.</span><span class="sxs-lookup"><span data-stu-id="79be6-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="79be6-106">**[Hello bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="79be6-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="79be6-107">**[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás még nem küld adatokat tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="79be6-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>
* <span data-ttu-id="79be6-108">**[SQL-felhasználók lap cheat](https://aka.ms/sql-analytics)**  hello leggyakoribb idioms lefordítja.</span><span class="sxs-lookup"><span data-stu-id="79be6-108">**[SQL-users' cheat sheet](https://aka.ms/sql-analytics)** translates hello most common idioms.</span></span>

<span data-ttu-id="79be6-109">Vegyünk egy lépésein végighaladva néhány alapvető lekérdezések tooget-t elindította.</span><span class="sxs-lookup"><span data-stu-id="79be6-109">Let's take a walk through some basic queries tooget you started.</span></span>

## <a name="connect-tooyour-application-insights-data"></a><span data-ttu-id="79be6-110">Csatlakozás tooyour Application Insights adatainak</span><span class="sxs-lookup"><span data-stu-id="79be6-110">Connect tooyour Application Insights data</span></span>
<span data-ttu-id="79be6-111">Nyissa meg az alkalmazás Analytics [áttekintése panel](app-insights-dashboards.md) az Application Insightsban:</span><span class="sxs-lookup"><span data-stu-id="79be6-111">Open Analytics from your app's [overview blade](app-insights-dashboards.md) in Application Insights:</span></span>

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a><span data-ttu-id="79be6-113">[Igénybe](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): n sorok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="79be6-113">[Take](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): show me n rows</span></span>
<span data-ttu-id="79be6-114">Felhasználói műveletek (általában HTTP-kérelmekre a webes alkalmazás által fogadott) jelentkezzen adatpontok tárolódnak a következő táblába `requests`.</span><span class="sxs-lookup"><span data-stu-id="79be6-114">Data points that log user operations (typically HTTP requests received by your web app) are stored in a table called `requests`.</span></span> <span data-ttu-id="79be6-115">Minden egyes sorára az egy telemetriai adat hello Application Insights SDK az alkalmazás kapott.</span><span class="sxs-lookup"><span data-stu-id="79be6-115">Each row is a telemetry data point received from hello Application Insights SDK in your app.</span></span>

<span data-ttu-id="79be6-116">Először néhány minta sorainak hello vizsgálata:</span><span class="sxs-lookup"><span data-stu-id="79be6-116">Let's start by examining a few sample rows of hello table:</span></span>

![eredmények](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> <span data-ttu-id="79be6-118">Helyezze el hello kurzor valahol hello utasítás előtt Ugrás gombra.</span><span class="sxs-lookup"><span data-stu-id="79be6-118">Put hello cursor somewhere in hello statement before you click Go.</span></span> <span data-ttu-id="79be6-119">Egy utasítás is átnyúlik egynél több sort, de nem üres sorok be egy utasítást.</span><span class="sxs-lookup"><span data-stu-id="79be6-119">You can split a statement over more than one line, but don't put blank lines in a statement.</span></span> <span data-ttu-id="79be6-120">Üres sorai egy kényelmes módszert arra tookeep több különálló lekérdezések hello ablakban.</span><span class="sxs-lookup"><span data-stu-id="79be6-120">Blank lines are a convenient way tookeep several separate queries in hello window.</span></span>
>
>

<span data-ttu-id="79be6-121">Oszlopok kiválasztása, egérrel húzza őket, oszlop, csoport, és szűréséhez:</span><span class="sxs-lookup"><span data-stu-id="79be6-121">Choose columns, drag them, group by columns, and filter:</span></span>

![Kattintson a felső jobb eredmények oszlop kiválasztása](./media/app-insights-analytics-tour/030.png)

<span data-ttu-id="79be6-123">Bármely elem toosee hello részleteinek kibontásához:</span><span class="sxs-lookup"><span data-stu-id="79be6-123">Expand any item toosee hello detail:</span></span>

![Kattintson a táblázat, és használja az oszlopok konfigurálása](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> <span data-ttu-id="79be6-125">Kattintson egy oszlop toore magasrendű hello érhetők el az eredmények hello webböngészőben hello vezetője.</span><span class="sxs-lookup"><span data-stu-id="79be6-125">Click hello head of a column toore-order hello results available in hello web browser.</span></span> <span data-ttu-id="79be6-126">De vegye figyelembe, hogy a nagy eredményhalmazt sorok letöltött toohello böngésző hello száma korlátozott.</span><span class="sxs-lookup"><span data-stu-id="79be6-126">But be aware that for a large result set, hello number of rows downloaded toohello browser is limited.</span></span> <span data-ttu-id="79be6-127">Ezzel a módszerrel rendezés nem mindig mutatja hello tényleges legmagasabb vagy legalacsonyabb elemek.</span><span class="sxs-lookup"><span data-stu-id="79be6-127">Sorting this way doesn't always show you hello actual highest or lowest items.</span></span> <span data-ttu-id="79be6-128">toosort elemek megbízhatóan, használja a hello `top` vagy `sort` operátor.</span><span class="sxs-lookup"><span data-stu-id="79be6-128">toosort items reliably, use hello `top` or `sort` operator.</span></span>
>
>

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a><span data-ttu-id="79be6-129">[Felső](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) és [rendezés](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span><span class="sxs-lookup"><span data-stu-id="79be6-129">[Top](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) and [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)</span></span>
<span data-ttu-id="79be6-130">`take`hasznos tooget gyors mintát eredményt, de az nem meghatározott sorrendben hello tábla azon sorait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="79be6-130">`take` is useful tooget a quick sample of a result, but it shows rows from hello table in no particular order.</span></span> <span data-ttu-id="79be6-131">egy rendezett nézet tooget használja `top` (a minta) vagy `sort` (keresztül hello teljes tábla).</span><span class="sxs-lookup"><span data-stu-id="79be6-131">tooget an ordered view, use `top` (for a sample) or `sort` (over hello whole table).</span></span>

<span data-ttu-id="79be6-132">Hello első n sorát, adott oszlop szerint rendezve jelenjenek meg:</span><span class="sxs-lookup"><span data-stu-id="79be6-132">Show me hello first n rows, ordered by a particular column:</span></span>

```AIQL

    requests | top 10 by timestamp desc
```

* <span data-ttu-id="79be6-133">*Szintaxis:* legtöbb operátorok rendelkeznek kulcsszó paraméterek például `by`.</span><span class="sxs-lookup"><span data-stu-id="79be6-133">*Syntax:* Most operators have keyword parameters such as `by`.</span></span>
* <span data-ttu-id="79be6-134">`desc`csökkenő sorrendben = `asc` = növekvő.</span><span class="sxs-lookup"><span data-stu-id="79be6-134">`desc` = descending order, `asc` = ascending.</span></span>

![](./media/app-insights-analytics-tour/260.png)

<span data-ttu-id="79be6-135">`top...`További performant módszer megkapta a `sort ... | take...`.</span><span class="sxs-lookup"><span data-stu-id="79be6-135">`top...` is a more performant way of saying `sort ... | take...`.</span></span> <span data-ttu-id="79be6-136">A Microsoft sikerült írt:</span><span class="sxs-lookup"><span data-stu-id="79be6-136">We could have written:</span></span>

```AIQL

    requests | sort by timestamp desc | take 10
```

<span data-ttu-id="79be6-137">hello eredmény lenne hello azonos, de egy kicsit lassabban fog futni.</span><span class="sxs-lookup"><span data-stu-id="79be6-137">hello result would be hello same, but it would run a bit more slowly.</span></span> <span data-ttu-id="79be6-138">(Is létrehozható `order`, ez az alias az `sort`.)</span><span class="sxs-lookup"><span data-stu-id="79be6-138">(You could also write `order`, which is an alias of `sort`.)</span></span>

<span data-ttu-id="79be6-139">hello oszlopfejlécek hello tábla nézetben is használt toosort hello eredmények üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="79be6-139">hello column headers in hello table view can also be used toosort hello results on hello screen.</span></span> <span data-ttu-id="79be6-140">De működés során, ha már használta a `take` vagy `top` tooretrieve egy tábla csak egy részét, lesz csak sorrendjének hello rekordok már beolvasott.</span><span class="sxs-lookup"><span data-stu-id="79be6-140">But of course, if you've used `take` or `top` tooretrieve just part of a table, you'll only re-order hello records you've retrieved.</span></span>

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a><span data-ttu-id="79be6-141">[Ha](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): szűrési feltétel</span><span class="sxs-lookup"><span data-stu-id="79be6-141">[Where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): filtering on a condition</span></span>

<span data-ttu-id="79be6-142">Nézzük meg, csak kérelmeket, amelyek egy adott eredmény, hibakód:</span><span class="sxs-lookup"><span data-stu-id="79be6-142">Let's see just requests that returned a particular result code:</span></span>

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

<span data-ttu-id="79be6-143">Hello `where` operátor egy logikai kifejezés vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="79be6-143">hello `where` operator takes a Boolean expression.</span></span> <span data-ttu-id="79be6-144">Az alábbiakban néhány kulcsfontosságú pont róluk:</span><span class="sxs-lookup"><span data-stu-id="79be6-144">Here are some key points about them:</span></span>

* <span data-ttu-id="79be6-145">`and`, `or`: Logikai operátorok</span><span class="sxs-lookup"><span data-stu-id="79be6-145">`and`, `or`: Boolean operators</span></span>
* <span data-ttu-id="79be6-146">`==`, `<>`, `!=` : egyenlő, és nem egyenlő</span><span class="sxs-lookup"><span data-stu-id="79be6-146">`==`, `<>`, `!=` : equal and not equal</span></span>
* <span data-ttu-id="79be6-147">`=~`, `!~` : egyenlő és nem egyenlő azonban nem karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="79be6-147">`=~`, `!~` : case-insensitive string equal and not equal.</span></span> <span data-ttu-id="79be6-148">Számos további karakterlánc-összehasonlítási operátorok.</span><span class="sxs-lookup"><span data-stu-id="79be6-148">There are lots more string comparison operators.</span></span>

<!---Read all about [scalar expressions]().--->

### <a name="getting-hello-right-type"></a><span data-ttu-id="79be6-149">Hello megfelelő típusú beolvasása</span><span class="sxs-lookup"><span data-stu-id="79be6-149">Getting hello right type</span></span>
<span data-ttu-id="79be6-150">Sikertelen kérelmek keresése:</span><span class="sxs-lookup"><span data-stu-id="79be6-150">Find unsuccessful requests:</span></span>

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a><span data-ttu-id="79be6-151">Time</span><span class="sxs-lookup"><span data-stu-id="79be6-151">Time</span></span>

<span data-ttu-id="79be6-152">Alapértelmezés szerint a lekérdezések nincsenek korlátozott toohello utolsó 24 órában.</span><span class="sxs-lookup"><span data-stu-id="79be6-152">By default, your queries are restricted toohello last 24 hours.</span></span> <span data-ttu-id="79be6-153">Azonban módosíthatja ezt a tartományt:</span><span class="sxs-lookup"><span data-stu-id="79be6-153">But you can change this range:</span></span>

![](./media/app-insights-analytics-tour/change-time-range.png)

<span data-ttu-id="79be6-154">Bírálja felül a hello időtartomány írásával, amely akkor említi lekérdezés `timestamp` a where záradékban.</span><span class="sxs-lookup"><span data-stu-id="79be6-154">Override hello time range by writing any query that mentions `timestamp` in a where-clause.</span></span> <span data-ttu-id="79be6-155">Példa:</span><span class="sxs-lookup"><span data-stu-id="79be6-155">For example:</span></span>

```AIQL

    // What were hello slowest requests over hello past 3 days?
    requests
    | where timestamp > ago(3d)  // Override hello time range
    | top 5 by duration
```

<span data-ttu-id="79be6-156">hello idő tartomány szolgáltatása egyenértékű tooa "where" záradék után minden hashtagként hello forrástáblákból közül az egyik beszúrni.</span><span class="sxs-lookup"><span data-stu-id="79be6-156">hello time range feature is equivalent tooa 'where' clause inserted after each mention of one of hello source tables.</span></span>

<span data-ttu-id="79be6-157">`ago(3d)`azt jelenti, hogy a "három nappal ezelőtt".</span><span class="sxs-lookup"><span data-stu-id="79be6-157">`ago(3d)` means 'three days ago'.</span></span> <span data-ttu-id="79be6-158">Más időegységekkel óra közé tartozik (`2h`, `2.5h`), a perc (`25m`), és a másodperc (`10s`).</span><span class="sxs-lookup"><span data-stu-id="79be6-158">Other units of time include hours (`2h`, `2.5h`), minutes (`25m`), and seconds (`10s`).</span></span>

<span data-ttu-id="79be6-159">További példák:</span><span class="sxs-lookup"><span data-stu-id="79be6-159">Other examples:</span></span>

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

<span data-ttu-id="79be6-160">[Dátumokat és időpontokat hivatkozás](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span><span class="sxs-lookup"><span data-stu-id="79be6-160">[Dates and times reference](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).</span></span>


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a><span data-ttu-id="79be6-161">[Projekt](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): válassza ki, nevezze át és számítási oszlopok</span><span class="sxs-lookup"><span data-stu-id="79be6-161">[Project](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): select, rename, and compute columns</span></span>
<span data-ttu-id="79be6-162">Használjon [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick csak a kívánt oszlopok hello:</span><span class="sxs-lookup"><span data-stu-id="79be6-162">Use [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) toopick out just hello columns you want:</span></span>

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

<span data-ttu-id="79be6-163">Nevezze át az oszlopok is, és újakat megadása:</span><span class="sxs-lookup"><span data-stu-id="79be6-163">You can also rename columns and define new ones:</span></span>

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

* <span data-ttu-id="79be6-165">Oszlopnevek szóközöket is tartalmazhatnak, vagy azok vannak zárójeles Ha szimbólumokat, ez például: `['...']` vagy`["..."]`</span><span class="sxs-lookup"><span data-stu-id="79be6-165">Column names can include spaces or symbols if they are bracketed like this: `['...']` or `["..."]`</span></span>
* <span data-ttu-id="79be6-166">`%`hello szokásos moduló operátor van.</span><span class="sxs-lookup"><span data-stu-id="79be6-166">`%` is hello usual modulo operator.</span></span>
* <span data-ttu-id="79be6-167">`1d`(Ez egy számjegy, majd egy kellett ") literális timespan tehát az egy nap.</span><span class="sxs-lookup"><span data-stu-id="79be6-167">`1d` (that's a digit one, then a 'd') is a timespan literal meaning one day.</span></span> <span data-ttu-id="79be6-168">Az alábbiakban néhány további timespan-szövegkonstans: `12h`, `30m`, `10s`, `0.01s`.</span><span class="sxs-lookup"><span data-stu-id="79be6-168">Here are some more timespan literals: `12h`, `30m`, `10s`, `0.01s`.</span></span>
* <span data-ttu-id="79be6-169">`floor`(alias `bin`) kerekítése a legközelebbi többszörösére hello alapértéke megadta a toohello le értéket.</span><span class="sxs-lookup"><span data-stu-id="79be6-169">`floor` (alias `bin`) rounds a value down toohello nearest multiple of hello base value you provide.</span></span> <span data-ttu-id="79be6-170">Ezért `floor(aTime, 1s)` kerekítése a legközelebbi második toohello le egyszerre.</span><span class="sxs-lookup"><span data-stu-id="79be6-170">So `floor(aTime, 1s)` rounds a time down toohello nearest second.</span></span>

<span data-ttu-id="79be6-171">Kifejezések lehetnek összes hello szokásos operátorok (`+`, `-`,...), és számos különböző hasznos funkciók.</span><span class="sxs-lookup"><span data-stu-id="79be6-171">Expressions can include all hello usual operators (`+`, `-`, ...), and there's a range of useful functions.</span></span>

## <a name="extend"></a><span data-ttu-id="79be6-172">Bővíthető</span><span class="sxs-lookup"><span data-stu-id="79be6-172">Extend</span></span>
<span data-ttu-id="79be6-173">Ha csak tooadd oszlopok toohello meglévők, használja [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span><span class="sxs-lookup"><span data-stu-id="79be6-173">If you just want tooadd columns toohello existing ones, use [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

<span data-ttu-id="79be6-174">Használatával [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) kevésbé részletes, mint [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) Ha azt szeretné, hogy tookeep összes hello meglévő oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="79be6-174">Using [`extend`](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) is less verbose than [`project`](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) if you want tookeep all hello existing columns.</span></span>

### <a name="convert-toolocal-time"></a><span data-ttu-id="79be6-175">Átalakítás toolocal idő</span><span class="sxs-lookup"><span data-stu-id="79be6-175">Convert toolocal time</span></span>

<span data-ttu-id="79be6-176">Időbélyeg helyi időre mindig UTC formátumban vannak.</span><span class="sxs-lookup"><span data-stu-id="79be6-176">Timestamps are always in UTC.</span></span> <span data-ttu-id="79be6-177">Így ha Ön hello Velünk csendes-óceáni part és téli, akkor célszerű lehet ez:</span><span class="sxs-lookup"><span data-stu-id="79be6-177">So if you're on hello US Pacific coast and it's winter, you might like this:</span></span>

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a><span data-ttu-id="79be6-178">[Összefoglalója](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): sorcsoportra összesítése</span><span class="sxs-lookup"><span data-stu-id="79be6-178">[Summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): aggregate groups of rows</span></span>
<span data-ttu-id="79be6-179">`Summarize`alkalmazza a megadott *aggregátumfüggvény* keresztül sorcsoportra.</span><span class="sxs-lookup"><span data-stu-id="79be6-179">`Summarize` applies a specified *aggregation function* over groups of rows.</span></span>

<span data-ttu-id="79be6-180">Például a webes alkalmazás szükséges toorespond tooa kérelem hello idő hello mezőben van jelentett `duration`.</span><span class="sxs-lookup"><span data-stu-id="79be6-180">For example, hello time your web app takes toorespond tooa request is reported in hello field `duration`.</span></span> <span data-ttu-id="79be6-181">Nézzük meg hello átlagos válasz tooall kérelmek idő:</span><span class="sxs-lookup"><span data-stu-id="79be6-181">Let's see hello average response time tooall requests:</span></span>

![](./media/app-insights-analytics-tour/410.png)

<span data-ttu-id="79be6-182">Vagy a kérelmek különböző neveket azt sikerült külön hello eredmény:</span><span class="sxs-lookup"><span data-stu-id="79be6-182">Or we could separate hello result into requests of different names:</span></span>

![](./media/app-insights-analytics-tour/420.png)

<span data-ttu-id="79be6-183">`Summarize`gyűjti az adatpontok hello adatfolyam hello csoportokba tartozó mely hello `by` záradék egyaránt értékelődik ki.</span><span class="sxs-lookup"><span data-stu-id="79be6-183">`Summarize` collects hello data points in hello stream into groups for which hello `by` clause evaluates equally.</span></span> <span data-ttu-id="79be6-184">Minden érték hello `by` kifejezés - minden művelet neve a fenti példa hello - hello eredménytáblájában egy sort eredményez.</span><span class="sxs-lookup"><span data-stu-id="79be6-184">Each value in hello `by` expression - each operation name in hello above example - results in a row in hello result table.</span></span>

<span data-ttu-id="79be6-185">Vagy azt sikerült eredmények csoportosítás időpontja:</span><span class="sxs-lookup"><span data-stu-id="79be6-185">Or we could group results by time of day:</span></span>

![](./media/app-insights-analytics-tour/430.png)

<span data-ttu-id="79be6-186">Figyelje meg, hogyan használunk hello `bin` funkciót (más néven `floor`).</span><span class="sxs-lookup"><span data-stu-id="79be6-186">Notice how we're using hello `bin` function (aka `floor`).</span></span> <span data-ttu-id="79be6-187">Ha éppen most használt `by timestamp`, mindegyik bemeneti sorában végül volna a saját kis csoport.</span><span class="sxs-lookup"><span data-stu-id="79be6-187">If we just used `by timestamp`, every input row would end up in its own little group.</span></span> <span data-ttu-id="79be6-188">Bármely folyamatos skaláris a hasonló alkalommal vagy számok tudunk toobreak hello folyamatos tartomány diszkrét értékei kezelhető számmá.</span><span class="sxs-lookup"><span data-stu-id="79be6-188">For any continuous scalar like times or numbers, we have toobreak hello continuous range into a manageable number of discrete values.</span></span> <span data-ttu-id="79be6-189">`bin`-Ez csupán hello ismerős kerekítési lefelé `floor` működik – a hello legegyszerűbb módja toodo, amely.</span><span class="sxs-lookup"><span data-stu-id="79be6-189">`bin` - which is just hello familiar rounding-down `floor` function - is hello easiest way toodo that.</span></span>

<span data-ttu-id="79be6-190">Így azonos módszerrel tooreduce tartományok karakterláncok hello:</span><span class="sxs-lookup"><span data-stu-id="79be6-190">We can use hello same technique tooreduce ranges of strings:</span></span>

![](./media/app-insights-analytics-tour/440.png)

<span data-ttu-id="79be6-191">Figyelje meg, hogy használható `name=` olyan eredményoszlopot hello összesítő kifejezések vagy hello által záradék tooset hello nevét.</span><span class="sxs-lookup"><span data-stu-id="79be6-191">Notice that you can use `name=` tooset hello name of a result column, either in hello aggregation expressions or hello by-clause.</span></span>

## <a name="counting-sampled-data"></a><span data-ttu-id="79be6-192">Leltár adatminta</span><span class="sxs-lookup"><span data-stu-id="79be6-192">Counting sampled data</span></span>
<span data-ttu-id="79be6-193">`sum(itemCount)`az ajánlott összesítési toocount események hello.</span><span class="sxs-lookup"><span data-stu-id="79be6-193">`sum(itemCount)` is hello recommended aggregation toocount events.</span></span> <span data-ttu-id="79be6-194">Sok esetben az elemek száma 1, ==, így hello függvény egyszerűen hello számú hello csoport sorainak megszámlálása.</span><span class="sxs-lookup"><span data-stu-id="79be6-194">In many cases, itemCount==1, so hello function simply counts up hello number of rows in hello group.</span></span> <span data-ttu-id="79be6-195">De ha [mintavételi](app-insights-sampling.md) van műveletben, csak töredéke alatt hello eredeti események kerülnek, az Application Insights adatpontok úgy, hogy az egyes adatpontokban látja, `itemCount` események.</span><span class="sxs-lookup"><span data-stu-id="79be6-195">But when [sampling](app-insights-sampling.md) is in operation, only a fraction of hello original events are retained as data points in Application Insights, so that for each data point you see, there are `itemCount` events.</span></span>

<span data-ttu-id="79be6-196">Például, ha a mintavételi elveti 75 %-a hello eredeti eseményeket, majd az elemek száma == 4 megőrzi hello rekordokban - Ez azt jelenti, hogy megtartott rekordot, volt négy eredeti rögzíti.</span><span class="sxs-lookup"><span data-stu-id="79be6-196">For example, if sampling discards 75% of hello original events, then itemCount==4 in hello retained records - that is, for every retained record, there were four original records.</span></span>

<span data-ttu-id="79be6-197">Adaptív mintavételi hatására az elemek száma toobe magasabb időszakban történjenek, amikor az alkalmazás fokozottan használatban van.</span><span class="sxs-lookup"><span data-stu-id="79be6-197">Adaptive sampling causes itemCount toobe higher during periods when your application is being heavily used.</span></span>

<span data-ttu-id="79be6-198">Ezért az elemek száma összegzése biztosít a helyes becslése hello eredeti események száma.</span><span class="sxs-lookup"><span data-stu-id="79be6-198">Summing up itemCount therefore gives a good estimate of hello original number of events.</span></span>

![](./media/app-insights-analytics-tour/510.png)

<span data-ttu-id="79be6-199">Szerepel továbbá egy `count()` összesítési (és a count művelet), olyan esetekben, ahol valóban szeretné, hogy a csoportban található sorok számát toocount hello.</span><span class="sxs-lookup"><span data-stu-id="79be6-199">There's also a `count()` aggregation (and a count operation), for cases where you really do want toocount hello number of rows in a group.</span></span>

<span data-ttu-id="79be6-200">Nincs számos [aggregátumfüggvényeket](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span><span class="sxs-lookup"><span data-stu-id="79be6-200">There's a range of [aggregation functions](https://docs.loganalytics.io/learn/tutorials/aggregations.html).</span></span>

## <a name="charting-hello-results"></a><span data-ttu-id="79be6-201">Diagramkészítési hello eredmények</span><span class="sxs-lookup"><span data-stu-id="79be6-201">Charting hello results</span></span>
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

<span data-ttu-id="79be6-202">Alapértelmezés szerint eredmények táblázatos megjelenítéséhez:</span><span class="sxs-lookup"><span data-stu-id="79be6-202">By default, results display as a table:</span></span>

![](./media/app-insights-analytics-tour/225.png)

<span data-ttu-id="79be6-203">Tudnánk mint hello tábla nézet.</span><span class="sxs-lookup"><span data-stu-id="79be6-203">We can do better than hello table view.</span></span> <span data-ttu-id="79be6-204">Nézzük hello eredmények hello diagram nézetben hello függőleges vonal beállítással:</span><span class="sxs-lookup"><span data-stu-id="79be6-204">Let's look at hello results in hello chart view with hello vertical bar option:</span></span>

![Diagram, kattintson, majd válassza ki a függőleges sávdiagram, és rendelje hozzá x és y tengely](./media/app-insights-analytics-tour/230.png)

<span data-ttu-id="79be6-206">Figyelje meg, hogy bár a Microsoft nem hello eredmények idő szerint (ahogy látja, a hello tábla megjelenítési) rendezéséhez hello diagram megjelenítési mindig látható időpontok megfelelő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="79be6-206">Notice that although we didn't sort hello results by time (as you can see in hello table display), hello chart display always shows datetimes in correct order.</span></span>


## <a name="timecharts"></a><span data-ttu-id="79be6-207">Timecharts</span><span class="sxs-lookup"><span data-stu-id="79be6-207">Timecharts</span></span>
<span data-ttu-id="79be6-208">Események nincs mutatja óránként:</span><span class="sxs-lookup"><span data-stu-id="79be6-208">Show how many events there are each hour:</span></span>

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

<span data-ttu-id="79be6-209">Hello diagram megjelenítési beállítást:</span><span class="sxs-lookup"><span data-stu-id="79be6-209">Select hello Chart display option:</span></span>

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a><span data-ttu-id="79be6-211">Több sorozat</span><span class="sxs-lookup"><span data-stu-id="79be6-211">Multiple series</span></span>
<span data-ttu-id="79be6-212">Több kifejezést hello `summarize` záradékban több oszlop hoz létre.</span><span class="sxs-lookup"><span data-stu-id="79be6-212">Multiple expressions in hello `summarize` clause creates multiple columns.</span></span>

<span data-ttu-id="79be6-213">Több kifejezést hello `by` záradékban több sort, egy a minden értékkombinációja hoz létre.</span><span class="sxs-lookup"><span data-stu-id="79be6-213">Multiple expressions in hello `by` clause creates multiple rows, one for each combination of values.</span></span>

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tábla kérelmek óra és hely szerint](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a><span data-ttu-id="79be6-215">A diagram szegmentálja dimenziók</span><span class="sxs-lookup"><span data-stu-id="79be6-215">Segment a chart by dimensions</span></span>
<span data-ttu-id="79be6-216">Ha a diagram táblának karakterlánc oszlopot és egy numerikus oszlopot, hello karakterlánc lehet használt toosplit hello numerikus adatokat külön sorozat pontok.</span><span class="sxs-lookup"><span data-stu-id="79be6-216">If you chart a table that has a string column and a numeric column, hello string can be used toosplit hello numeric data into separate series of points.</span></span> <span data-ttu-id="79be6-217">Ha egynél több oszlophoz, dönthet úgy, mely oszlop toouse hello Diszkriminátor szerint.</span><span class="sxs-lookup"><span data-stu-id="79be6-217">If there's more than one string column, you can choose which column toouse as hello discriminator.</span></span>

![Egy diagramra szegmens](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a><span data-ttu-id="79be6-219">Golflabda arány</span><span class="sxs-lookup"><span data-stu-id="79be6-219">Bounce rate</span></span>

<span data-ttu-id="79be6-220">Egy logikai tooa karakterlánc toouse alakítsa át azt egy Diszkriminátor:</span><span class="sxs-lookup"><span data-stu-id="79be6-220">Convert a boolean tooa string toouse it as a discriminator:</span></span>

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

### <a name="display-multiple-metrics"></a><span data-ttu-id="79be6-221">Több metrikák megjelenítése</span><span class="sxs-lookup"><span data-stu-id="79be6-221">Display multiple metrics</span></span>
<span data-ttu-id="79be6-222">Ha a diagramtípus egynél több numerikus oszlopot, táblának a hozzáadása toohello timestamp, azok bármilyen kombinációját jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="79be6-222">If you chart a table that has more than one numeric column, in addition toohello timestamp, you can display any combination of them.</span></span>

![Egy diagramra szegmens](./media/app-insights-analytics-tour/110.png)

<span data-ttu-id="79be6-224">Ki kell választania **nem osztott** több numerikus oszlop kiválasztása előtt.</span><span class="sxs-lookup"><span data-stu-id="79be6-224">You must select **Don't Split** before you can select multiple numeric columns.</span></span> <span data-ttu-id="79be6-225">Meg nem osztható fel: hello karakterlánc oszlop szerint azonos időben is csak egy numerikus oszlopot megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="79be6-225">You can't split by a string column at hello same time as displaying more than one numeric column.</span></span>

## <a name="daily-average-cycle"></a><span data-ttu-id="79be6-226">Napi átlagos ciklus</span><span class="sxs-lookup"><span data-stu-id="79be6-226">Daily average cycle</span></span>
<span data-ttu-id="79be6-227">Hogyan változik a használati hello átlagos napon keresztül?</span><span class="sxs-lookup"><span data-stu-id="79be6-227">How does usage vary over hello average day?</span></span>

<span data-ttu-id="79be6-228">Count kérések hello idő moduló egy nap munkaidőre binned:</span><span class="sxs-lookup"><span data-stu-id="79be6-228">Count requests by hello time modulo one day, binned into hours:</span></span>

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
> <span data-ttu-id="79be6-230">Figyelje meg, hogy jelenleg tudunk tooconvert idő időtartamok toodatetimes a rendelés toodisplay a grafikont.</span><span class="sxs-lookup"><span data-stu-id="79be6-230">Notice that we currently have tooconvert time durations toodatetimes in order toodisplay on a line chart.</span></span>
>
>

## <a name="compare-multiple-daily-series"></a><span data-ttu-id="79be6-231">Több napi sorozat összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="79be6-231">Compare multiple daily series</span></span>
<span data-ttu-id="79be6-232">Hogyan használati változik a nap hello idővel a különböző országokban?</span><span class="sxs-lookup"><span data-stu-id="79be6-232">How does usage vary over hello time of day in different countries?</span></span>

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

## <a name="plot-a-distribution"></a><span data-ttu-id="79be6-234">Egy terjesztési ábrázolása</span><span class="sxs-lookup"><span data-stu-id="79be6-234">Plot a distribution</span></span>
<span data-ttu-id="79be6-235">Hány munkamenetek vannak-e különböző hosszúságú?</span><span class="sxs-lookup"><span data-stu-id="79be6-235">How many sessions are there of different lengths?</span></span>

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

<span data-ttu-id="79be6-236">hello utolsó sora szükség tooconvert toodatetime.</span><span class="sxs-lookup"><span data-stu-id="79be6-236">hello last line is required tooconvert toodatetime.</span></span> <span data-ttu-id="79be6-237">Jelenleg egy diagram hello x tengelyének jelenik meg skaláris csak akkor, ha egy DateTime típusú érték.</span><span class="sxs-lookup"><span data-stu-id="79be6-237">Currently hello x axis of a chart is displayed as a scalar only if it is a datetime.</span></span>

<span data-ttu-id="79be6-238">Hello `where` záradék nem tartalmazza a végeredménye munkamenetek (sessionDuration == 0) és a készletek hello hello x tengely hosszát.</span><span class="sxs-lookup"><span data-stu-id="79be6-238">hello `where` clause excludes one-shot sessions (sessionDuration==0) and sets hello length of hello x-axis.</span></span>

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[<span data-ttu-id="79be6-239">Százalékos érték</span><span class="sxs-lookup"><span data-stu-id="79be6-239">Percentiles</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
<span data-ttu-id="79be6-240">Milyen tartományok időtartamok fedik le a munkamenetek különböző százalékának?</span><span class="sxs-lookup"><span data-stu-id="79be6-240">What ranges of durations cover different percentages of sessions?</span></span>

<span data-ttu-id="79be6-241">Lekérdezés fent hello használja, de hello utolsó sora cserélje le:</span><span class="sxs-lookup"><span data-stu-id="79be6-241">Use hello above query, but replace hello last line:</span></span>

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

<span data-ttu-id="79be6-242">Azt is eltávolítani hello felső korlátja hello, ahol a következő sorrendben tooget záradékában javítsa egynél több kérelemmel összes munkameneteket is beleértve:</span><span class="sxs-lookup"><span data-stu-id="79be6-242">We also removed hello upper limit in hello where clause, in order tooget correct figures including all sessions with more than one request:</span></span>

![eredménye](./media/app-insights-analytics-tour/180.png)

<span data-ttu-id="79be6-244">Ahol láthatja, hogy:</span><span class="sxs-lookup"><span data-stu-id="79be6-244">From which we can see that:</span></span>

* <span data-ttu-id="79be6-245">a munkamenetek 5 % rendelkezik a kevesebb mint 3 perc alatt 34s; időtartama</span><span class="sxs-lookup"><span data-stu-id="79be6-245">5% of sessions have a duration of less than 3 minutes 34s;</span></span>
* <span data-ttu-id="79be6-246">50 %-a munkamenetek utolsó 36 percnél kevesebb;</span><span class="sxs-lookup"><span data-stu-id="79be6-246">50% of sessions last less than 36 minutes;</span></span>
* <span data-ttu-id="79be6-247">az utolsó 7 napnál 5 %-a munkamenetek</span><span class="sxs-lookup"><span data-stu-id="79be6-247">5% of sessions last more than 7 days</span></span>

<span data-ttu-id="79be6-248">külön részletes információkat az egyes országok tooget, csak tudunk toobring hello client_CountryOrRegion oszlop külön-külön is keresztül operátorok összegzése:</span><span class="sxs-lookup"><span data-stu-id="79be6-248">tooget a separate breakdown for each country, we just have toobring hello client_CountryOrRegion column separately through both summarize operators:</span></span>

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

## <a name="join"></a><span data-ttu-id="79be6-249">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="79be6-249">Join</span></span>
<span data-ttu-id="79be6-250">Hozzáférés tooseveral táblázatokat, beleértve a kérelmek és kivételek van.</span><span class="sxs-lookup"><span data-stu-id="79be6-250">We have access tooseveral tables, including requests and exceptions.</span></span>

<span data-ttu-id="79be6-251">toofind hello kivételek kapcsolódó tooa kérelem által visszaadott hibaválaszt, azt is csatlakoztatás hello táblák `session_Id`:</span><span class="sxs-lookup"><span data-stu-id="79be6-251">toofind hello exceptions related tooa request that returned a failure response, we can join hello tables on `session_Id`:</span></span>

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


<span data-ttu-id="79be6-252">Akkor célszerű toouse `project` tooselect csak hello oszlopok végrehajtása előtt kell hello illesztési.</span><span class="sxs-lookup"><span data-stu-id="79be6-252">It's good practice toouse `project` tooselect just hello columns we need before performing hello join.</span></span>
<span data-ttu-id="79be6-253">A hello azonos záradékban, a Microsoft hello Timestamp típusú oszlop átnevezése.</span><span class="sxs-lookup"><span data-stu-id="79be6-253">In hello same clauses, we rename hello timestamp column.</span></span>

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-tooa-variable"></a><span data-ttu-id="79be6-254">[Így](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): rendelni egy eredmény tooa változó</span><span class="sxs-lookup"><span data-stu-id="79be6-254">[Let](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): Assign a result tooa variable</span></span>

<span data-ttu-id="79be6-255">Használjon `let` tooseparate kimenő hello előző kifejezés hello részeit.</span><span class="sxs-lookup"><span data-stu-id="79be6-255">Use `let` tooseparate out hello parts of hello previous expression.</span></span> <span data-ttu-id="79be6-256">hello eredmények ugyanazok maradtak, mint:</span><span class="sxs-lookup"><span data-stu-id="79be6-256">hello results are unchanged:</span></span>

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> <span data-ttu-id="79be6-257">Hello Analytics ügyfél put nem üres sorok hello lekérdezés hello részei között.</span><span class="sxs-lookup"><span data-stu-id="79be6-257">In hello Analytics client, don't put blank lines between hello parts of hello query.</span></span> <span data-ttu-id="79be6-258">Győződjön meg arról, hogy tooexecute minden elemét.</span><span class="sxs-lookup"><span data-stu-id="79be6-258">Make sure tooexecute all of it.</span></span>
>

<span data-ttu-id="79be6-259">Használjon `toscalar` tooconvert egyetlen tábla cella tooa értéket:</span><span class="sxs-lookup"><span data-stu-id="79be6-259">Use `toscalar` tooconvert a single table cell tooa value:</span></span>

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


### <a name="functions"></a><span data-ttu-id="79be6-260">Functions</span><span class="sxs-lookup"><span data-stu-id="79be6-260">Functions</span></span>

<span data-ttu-id="79be6-261">Használjon *segítségével* toodefine függvény:</span><span class="sxs-lookup"><span data-stu-id="79be6-261">Use *Let* toodefine a function:</span></span>

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a><span data-ttu-id="79be6-262">Beágyazott objektumok elérése</span><span class="sxs-lookup"><span data-stu-id="79be6-262">Accessing nested objects</span></span>
<span data-ttu-id="79be6-263">A beágyazott objektumok könnyen elérhetők.</span><span class="sxs-lookup"><span data-stu-id="79be6-263">Nested objects can be accessed easily.</span></span> <span data-ttu-id="79be6-264">Például a hello kivételek adatfolyam látható strukturált objektumok ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="79be6-264">For example, in hello exceptions stream you can see structured objects like this:</span></span>

![eredménye](./media/app-insights-analytics-tour/520.png)

<span data-ttu-id="79be6-266">Kíváncsiak vagyunk hello tulajdonságok kiválasztásával konvertálhatók:</span><span class="sxs-lookup"><span data-stu-id="79be6-266">You can flatten it by choosing hello properties you're interested in:</span></span>

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

<span data-ttu-id="79be6-267">Ne feledje, hogy toocast hello toohello megfelelő típusú.</span><span class="sxs-lookup"><span data-stu-id="79be6-267">Note that you need toocast hello result toohello appropriate type.</span></span>


## <a name="custom-properties-and-measurements"></a><span data-ttu-id="79be6-268">Egyéni tulajdonságok és mérések</span><span class="sxs-lookup"><span data-stu-id="79be6-268">Custom properties and measurements</span></span>
<span data-ttu-id="79be6-269">Ha az alkalmazás csatol [egyéni dimenzió (Tulajdonságok) és egyéni mérték](app-insights-api-custom-events-metrics.md#properties) tooevents, akkor megtekintheti azokat a hello `customDimensions` és `customMeasurements` objektumok.</span><span class="sxs-lookup"><span data-stu-id="79be6-269">If your application attaches [custom dimensions (properties) and custom measurements](app-insights-api-custom-events-metrics.md#properties) tooevents, then you will see them in hello `customDimensions` and `customMeasurements` objects.</span></span>

<span data-ttu-id="79be6-270">Ha például az alkalmazás tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="79be6-270">For example, if your app includes:</span></span>

```C#

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

<span data-ttu-id="79be6-271">tooextract Analytics ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="79be6-271">tooextract these values in Analytics:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast tooexpected type

```

<span data-ttu-id="79be6-272">tooverify egy egyéni dimenzió egy adott típusú van-e:</span><span class="sxs-lookup"><span data-stu-id="79be6-272">tooverify whether a custom dimension is of a particular type:</span></span>

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a><span data-ttu-id="79be6-273">Irányítópultok</span><span class="sxs-lookup"><span data-stu-id="79be6-273">Dashboards</span></span>
<span data-ttu-id="79be6-274">Az eredmények tooa irányítópult rendelés toobring rögzíthető együtt minden a legfontosabb diagramok és táblák.</span><span class="sxs-lookup"><span data-stu-id="79be6-274">You can pin your results tooa dashboard in order toobring together all your most important charts and tables.</span></span>

* <span data-ttu-id="79be6-275">[Az Azure megosztott irányítópult](app-insights-dashboards.md#share-dashboards): hello PIN-kód ikonra.</span><span class="sxs-lookup"><span data-stu-id="79be6-275">[Azure shared dashboard](app-insights-dashboards.md#share-dashboards): Click hello pin icon.</span></span> <span data-ttu-id="79be6-276">Mielőtt ezt megtehetné, egy megosztott irányítópult kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="79be6-276">Before you do this, you must have a shared dashboard.</span></span> <span data-ttu-id="79be6-277">Hello Azure-portálon, a nyissa meg, vagy hozzon létre egy irányítópultot, és kattintson a megosztás.</span><span class="sxs-lookup"><span data-stu-id="79be6-277">In hello Azure portal, open or create a dashboard and click Share.</span></span>
* <span data-ttu-id="79be6-278">[A Power BI-irányítópulton](app-insights-export-power-bi.md): kattintson az exportálni, a Power BI-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="79be6-278">[Power BI dashboard](app-insights-export-power-bi.md): Click Export, Power BI Query.</span></span> <span data-ttu-id="79be6-279">Ez a megoldás előnye, hogy a lekérdezés más a mellett számos információforrás jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="79be6-279">An advantage of this alternative is that you can display your query alongside other results from a wide range of sources.</span></span>

## <a name="combine-with-imported-data"></a><span data-ttu-id="79be6-280">Az importált adatok kombinálása</span><span class="sxs-lookup"><span data-stu-id="79be6-280">Combine with imported data</span></span>

<span data-ttu-id="79be6-281">Elemzési jelentések remekül hello irányítópulton, de előfordulhat tootranslate hello adatok tooa további emészthető űrlap.</span><span class="sxs-lookup"><span data-stu-id="79be6-281">Analytics reports look great on hello dashboard, but sometimes you want tootranslate hello data tooa more digestible form.</span></span> <span data-ttu-id="79be6-282">Tegyük fel, hogy a hitelesített felhasználók alias hello telemetriai k azonosítását.</span><span class="sxs-lookup"><span data-stu-id="79be6-282">For example, suppose your authenticated users are identified in hello telemetry by an alias.</span></span> <span data-ttu-id="79be6-283">A valós nevek tooshow szeretné az eredmények között.</span><span class="sxs-lookup"><span data-stu-id="79be6-283">You'd like tooshow their real names in your results.</span></span> <span data-ttu-id="79be6-284">toodo, egy CSV-fájl, amely leképezhető a hello aliasok toohello valódi neve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="79be6-284">toodo this, you need a CSV file that maps from hello aliases toohello real names.</span></span>

<span data-ttu-id="79be6-285">Importálhatja egy adatfájlt és csakúgy, mint a hagyományos táblázatokban hello (kérelmeket, kivételek, és így tovább) használja.</span><span class="sxs-lookup"><span data-stu-id="79be6-285">You can import a data file and use it just like any of hello standard tables (requests, exceptions, and so on).</span></span> <span data-ttu-id="79be6-286">Vagy lekérdezni az önállóan, vagy más táblák csatlakozzon hozzá.</span><span class="sxs-lookup"><span data-stu-id="79be6-286">Either query it on its own, or join it with other tables.</span></span> <span data-ttu-id="79be6-287">Például, ha a usermap nevű tábla, és nem rendelkezik-e oszlopok `realName` és `userId`, akkor használhatja azt tootranslate hello `user_AuthenticatedId` hello – kéréstelemetria mezőbe:</span><span class="sxs-lookup"><span data-stu-id="79be6-287">For example, if you have a table named usermap, and it has columns `realName` and `userId`, then you can use it tootranslate hello `user_AuthenticatedId` field in hello request telemetry:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get hello realName field from hello usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

<span data-ttu-id="79be6-288">egy tábla hello séma panelen tooimport alatt **egyéb adatforrások**, hello utasításokat tooadd új adatforrás, kövesse az adatok minta feltöltésével.</span><span class="sxs-lookup"><span data-stu-id="79be6-288">tooimport a table, in hello Schema blade, under **Other Data Sources**, follow hello instructions tooadd a new data source, by uploading a sample of your data.</span></span> <span data-ttu-id="79be6-289">Ezután a definíció tooupload táblák is használhatja.</span><span class="sxs-lookup"><span data-stu-id="79be6-289">Then you can use this definition tooupload tables.</span></span>

<span data-ttu-id="79be6-290">hello importálás funkció jelenleg előzetes, ezért kezdetben megjelenik az "Egyéb adatforrások." ", lépjen velünk kapcsolatba" hivatkozás</span><span class="sxs-lookup"><span data-stu-id="79be6-290">hello import feature is currently in preview, so you will initially see a "Contact us" link under "Other data sources."</span></span> <span data-ttu-id="79be6-291">A toosign toohello preview program használja, és hello hivatkozás majd váltja fel az "Új adatforrás hozzáadása" gombra.</span><span class="sxs-lookup"><span data-stu-id="79be6-291">Use this toosign up toohello preview program, and hello link will then be replaced by an "Add new data source" button.</span></span>


## <a name="tables"></a><span data-ttu-id="79be6-292">Táblák</span><span class="sxs-lookup"><span data-stu-id="79be6-292">Tables</span></span>
<span data-ttu-id="79be6-293">az alkalmazástól fogadott telemetriai hello adatfolyam több táblázatot keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="79be6-293">hello stream of telemetry received from your app is accessible through several tables.</span></span> <span data-ttu-id="79be6-294">minden tábla rendelkezésre álló tulajdonságok hello sémája hello bal hello ablak akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="79be6-294">hello schema of properties available for each table is visible at hello left of hello window.</span></span>

### <a name="requests-table"></a><span data-ttu-id="79be6-295">Kérelmek tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-295">Requests table</span></span>
<span data-ttu-id="79be6-296">Count HTTP-kérelmek tooyour webalkalmazás és a szegmens lapnév szerint:</span><span class="sxs-lookup"><span data-stu-id="79be6-296">Count HTTP requests tooyour web app and segment by page name:</span></span>

![Count kérelmek szegmentált név szerint](./media/app-insights-analytics-tour/analytics-count-requests.png)

<span data-ttu-id="79be6-298">A legtöbb teljesítő hello kérelmek keresése:</span><span class="sxs-lookup"><span data-stu-id="79be6-298">Find hello requests that fail most:</span></span>

![Count kérelmek szegmentált név szerint](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a><span data-ttu-id="79be6-300">Egyéni események tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-300">Custom events table</span></span>
<span data-ttu-id="79be6-301">Ha [trackevent() függvény](app-insights-api-custom-events-metrics.md#trackevent) toosend saját események áttekintheti azokat ebből a táblázatból.</span><span class="sxs-lookup"><span data-stu-id="79be6-301">If you use [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) toosend your own events, you can read them from this table.</span></span>

<span data-ttu-id="79be6-302">Vegyünk egy példát, amelyben az alkalmazás kódját tartalmazza a sorokat:</span><span class="sxs-lookup"><span data-stu-id="79be6-302">Let's take an example where your app code contains these lines:</span></span>

```C#

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

<span data-ttu-id="79be6-303">Ezek az események gyakorisága hello megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="79be6-303">Display hello frequency of these events:</span></span>

![Egyéni események megjelenítése aránya](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

<span data-ttu-id="79be6-305">Mérések és dimenziókat kinyerése hello események:</span><span class="sxs-lookup"><span data-stu-id="79be6-305">Extract measurements and dimensions from hello events:</span></span>

![Egyéni események megjelenítése aránya](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a><span data-ttu-id="79be6-307">Egyéni metrikák tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-307">Custom metrics table</span></span>
<span data-ttu-id="79be6-308">Ha használ [trackmetric() függvény](app-insights-api-custom-events-metrics.md#trackmetric) toosend saját metrika értékeket, látni fogja az eredményeket a hello **customMetrics** adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="79be6-308">If you are using [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) toosend your own metric values, you’ll find its results in hello **customMetrics** stream.</span></span> <span data-ttu-id="79be6-309">Példa:</span><span class="sxs-lookup"><span data-stu-id="79be6-309">For example:</span></span>  

![Egyéni metrikákat az Application Insights elemzés](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> <span data-ttu-id="79be6-311">A [Metrikaböngésző](app-insights-metrics-explorer.md), telemetriai csatolt tooany típusú szerepelnek együtt hello metrikák panelről és metrikák használatával küldött összes egyéni mérték `TrackMetric()`.</span><span class="sxs-lookup"><span data-stu-id="79be6-311">In [Metrics Explorer](app-insights-metrics-explorer.md), all custom measurements attached tooany type of telemetry appear together in hello metrics blade along with metrics sent using `TrackMetric()`.</span></span> <span data-ttu-id="79be6-312">De Analytics, egyéni mértékek még csatolt toowhichever telemetriai azok zajlott - események vagy kéréseket, és így tovább -, amíg TrackMetric által küldött metrikák jelennek meg a saját adatfolyam típusú.</span><span class="sxs-lookup"><span data-stu-id="79be6-312">But in Analytics, custom measurements are still attached toowhichever type of telemetry they were carried on - events or requests, and so on - while metrics sent by TrackMetric appear in their own stream.</span></span>
>
>

### <a name="performance-counters-table"></a><span data-ttu-id="79be6-313">Teljesítmény számlálók tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-313">Performance counters table</span></span>
<span data-ttu-id="79be6-314">[Teljesítményszámlálók](app-insights-performance-counters.md) láthat rendszer alapvető metrikákat az alkalmazás, például a Processzor, memória, és a hálózathasználat.</span><span class="sxs-lookup"><span data-stu-id="79be6-314">[Performance counters](app-insights-performance-counters.md) show you basic system metrics for your app, such as CPU, memory, and network utilization.</span></span> <span data-ttu-id="79be6-315">Hello SDK toosend számlálókat, beleértve a saját egyéni számlálók konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="79be6-315">You can configure hello SDK toosend additional counters, including your own custom counters.</span></span>

<span data-ttu-id="79be6-316">Hello **performanceCounters** séma közzététele hello `category`, `counter` nevét, és `instance` minden teljesítményszámláló nevét.</span><span class="sxs-lookup"><span data-stu-id="79be6-316">hello **performanceCounters** schema exposes hello `category`, `counter` name, and `instance` name of each performance counter.</span></span> <span data-ttu-id="79be6-317">Teljesítményszámláló-példányok nevei csak a megfelelő toosome teljesítményszámlálókat, és általában azt jelzik, hello folyamatok toowhich hello számának hello neve vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="79be6-317">Counter instance names are only applicable toosome performance counters, and typically indicate hello name of hello process toowhich hello count relates.</span></span> <span data-ttu-id="79be6-318">Hello telemetriai minden alkalmazáshoz az alkalmazás csak hello számlálók látható.</span><span class="sxs-lookup"><span data-stu-id="79be6-318">In hello telemetry for each application, you’ll see only hello counters for that application.</span></span> <span data-ttu-id="79be6-319">Például toosee milyen számlálók érhetők el:</span><span class="sxs-lookup"><span data-stu-id="79be6-319">For example, toosee what counters are available:</span></span>

![Az Application Insights analytics teljesítményszámlálók](./media/app-insights-analytics-tour/analytics-performance-counters.png)

<span data-ttu-id="79be6-321">a kijelölt időszak tooget a diagram a hello keresztül a rendelkezésre álló memória:</span><span class="sxs-lookup"><span data-stu-id="79be6-321">tooget a chart of available memory over hello selected period:</span></span>

![Az Application Insights Analytics memória timechart](./media/app-insights-analytics-tour/analytics-available-memory.png)

<span data-ttu-id="79be6-323">Egyéb telemetriai adatokat, például **performanceCounters** egy olyan oszlop is van `cloud_RoleInstance` azt jelzi, hogy hello hello gazdagépet, amelyen fut az alkalmazás identitását.</span><span class="sxs-lookup"><span data-stu-id="79be6-323">Like other telemetry, **performanceCounters** also has a column `cloud_RoleInstance` that indicates hello identity of hello host machine on which your app is running.</span></span> <span data-ttu-id="79be6-324">Például toocompare hello az alkalmazás teljesítményével kapcsolatos különböző gépeken hello:</span><span class="sxs-lookup"><span data-stu-id="79be6-324">For example, toocompare hello performance of your app on hello different machines:</span></span>

![Az Application Insights Analytics szerepkör példánya szegmentált teljesítmény](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a><span data-ttu-id="79be6-326">Kivételek tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-326">Exceptions table</span></span>
<span data-ttu-id="79be6-327">[Az alkalmazás által jelentett kivételek](app-insights-asp-net-exceptions.md) érhetők el ebben a táblában.</span><span class="sxs-lookup"><span data-stu-id="79be6-327">[Exceptions reported by your app](app-insights-asp-net-exceptions.md) are available in this table.</span></span>

<span data-ttu-id="79be6-328">toofind hello az alkalmazás lett kezelése, ha hello kivételt észlelt HTTP-kérelem, operation_Id csatlakoztatás:</span><span class="sxs-lookup"><span data-stu-id="79be6-328">toofind hello HTTP request that your app was handling when hello exception was raised, join on operation_Id:</span></span>

![Csatlakoztatás operation_Id kivételek kérések](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a><span data-ttu-id="79be6-330">Időzítés tábla Browser</span><span class="sxs-lookup"><span data-stu-id="79be6-330">Browser timings table</span></span>
<span data-ttu-id="79be6-331">`browserTimings`a felhasználók böngészőjének gyűjtött lapbetöltési adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="79be6-331">`browserTimings` shows page load data collected in your users' browsers.</span></span>

<span data-ttu-id="79be6-332">[Állítsa be az alkalmazás ügyféloldali telemetriai](app-insights-javascript.md) a rendezés toosee metrikákat.</span><span class="sxs-lookup"><span data-stu-id="79be6-332">[Set up your app for client-side telemetry](app-insights-javascript.md) in order toosee these metrics.</span></span>

<span data-ttu-id="79be6-333">hello séma tartalmaz [hello lap betöltési folyamat különböző szakaszaiban hello hosszának jelző metrikák](app-insights-javascript.md#page-load-performance).</span><span class="sxs-lookup"><span data-stu-id="79be6-333">hello schema includes [metrics indicating hello lengths of different stages of hello page loading process](app-insights-javascript.md#page-load-performance).</span></span> <span data-ttu-id="79be6-334">(A felhasználók, olvassa el a lap hello időtartamot nem utal.)</span><span class="sxs-lookup"><span data-stu-id="79be6-334">(They don’t indicate hello length of time your users read a page.)</span></span>  

<span data-ttu-id="79be6-335">Hello popularities különböző oldalak megjelenítése, és az összes lapon alkalommal betöltése:</span><span class="sxs-lookup"><span data-stu-id="79be6-335">Show hello popularities of different pages, and load times for each page:</span></span>

![Lapbetöltési idők Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a><span data-ttu-id="79be6-337">Rendelkezésre állási eredmények táblázatában</span><span class="sxs-lookup"><span data-stu-id="79be6-337">Availability results table</span></span>
<span data-ttu-id="79be6-338">`availabilityResults`azt mutatja be hello eredményeit a [webalkalmazás-tesztek](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="79be6-338">`availabilityResults` shows hello results of your [web tests](app-insights-monitor-web-app-availability.md).</span></span> <span data-ttu-id="79be6-339">Minden egyes teszt helyen a teszt futtatásakor a külön-külön jelenti.</span><span class="sxs-lookup"><span data-stu-id="79be6-339">Each run of your tests from each test location is reported separately.</span></span>

![Lapbetöltési idők Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a><span data-ttu-id="79be6-341">Függőségek tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-341">Dependencies table</span></span>
<span data-ttu-id="79be6-342">Tartalmazza a hívás eredményeit, hogy az alkalmazás lehetővé teszi a toodatabases és a REST API-k és más tooTrackDependency() hívja.</span><span class="sxs-lookup"><span data-stu-id="79be6-342">Contains results of calls that your app makes toodatabases and REST APIs, and other calls tooTrackDependency().</span></span> <span data-ttu-id="79be6-343">Hello böngészőből AJAX-hívások is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="79be6-343">Also includes AJAX calls made from hello browser.</span></span>

<span data-ttu-id="79be6-344">AJAX-hívások hello böngészőből:</span><span class="sxs-lookup"><span data-stu-id="79be6-344">AJAX calls from hello browser:</span></span>

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

<span data-ttu-id="79be6-345">Hello kiszolgálóról függőségi hívások esetében:</span><span class="sxs-lookup"><span data-stu-id="79be6-345">Dependency calls from hello server:</span></span>

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

<span data-ttu-id="79be6-346">Kiszolgálóoldali függőségi mindig eredményben `success==False` Ha hello Application Insights-ügynök nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="79be6-346">Server-side dependency results always show `success==False` if hello Application Insights Agent is not installed.</span></span> <span data-ttu-id="79be6-347">Azonban hello más adatok megfelelőek-e.</span><span class="sxs-lookup"><span data-stu-id="79be6-347">However, hello other data are correct.</span></span>

### <a name="traces-table"></a><span data-ttu-id="79be6-348">Nyomkövetések tábla</span><span class="sxs-lookup"><span data-stu-id="79be6-348">Traces table</span></span>
<span data-ttu-id="79be6-349">Tartalmazza az TrackTrace(), használó alkalmazás által küldött hello telemetriai vagy [más naplózási keretrendszer](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="79be6-349">Contains hello telemetry sent by your app using TrackTrace(), or [other logging frameworks](app-insights-asp-net-trace-logs.md).</span></span>

## <a name="video"></a><span data-ttu-id="79be6-350">Videó</span><span class="sxs-lookup"><span data-stu-id="79be6-350">Video</span></span> 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

<span data-ttu-id="79be6-351">Speciális lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="79be6-351">Advanced queries:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a><span data-ttu-id="79be6-352">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="79be6-352">Next steps</span></span>
* [<span data-ttu-id="79be6-353">Elemzés nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="79be6-353">Analytics language reference</span></span>](app-insights-analytics-reference.md)
* <span data-ttu-id="79be6-354">[SQL-felhasználók lap cheat](https://aka.ms/sql-analytics) hello leggyakoribb idioms lefordítja.</span><span class="sxs-lookup"><span data-stu-id="79be6-354">[SQL-users' cheat sheet](https://aka.ms/sql-analytics) translates hello most common idioms.</span></span>

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
