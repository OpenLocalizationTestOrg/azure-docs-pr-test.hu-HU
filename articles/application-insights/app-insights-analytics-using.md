---
title: "aaaUsing Analytics - hello hatékony keresési eszköz az Azure Application Insights |} Microsoft Docs"
description: "Hello elemzés, az Application Insights hello hatékony diagnosztikai keresési eszköz használatával. "
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6e0246848457db368c57d08c47b5bf73f4e5e3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-analytics-in-application-insights"></a><span data-ttu-id="b5117-103">Az Application Insightsban Analytics használatával</span><span class="sxs-lookup"><span data-stu-id="b5117-103">Using Analytics in Application Insights</span></span>
<span data-ttu-id="b5117-104">[Elemzés](app-insights-analytics.md) hello hatékony keresési funkciója [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b5117-104">[Analytics](app-insights-analytics.md) is hello powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="b5117-105">Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.</span><span class="sxs-lookup"><span data-stu-id="b5117-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="b5117-106">**[Hello bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="b5117-106">**[Watch hello introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="b5117-107">**[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás még nem küld adatokat tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="b5117-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data tooApplication Insights yet.</span></span>

## <a name="open-analytics"></a><span data-ttu-id="b5117-108">Nyissa meg elemzés</span><span class="sxs-lookup"><span data-stu-id="b5117-108">Open Analytics</span></span>
<span data-ttu-id="b5117-109">A az alkalmazás otthoni erőforrás az Application Insightsban kattintson az elemzés.</span><span class="sxs-lookup"><span data-stu-id="b5117-109">From your app's home resource in Application Insights, click Analytics.</span></span>

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics-using/001.png)

<span data-ttu-id="b5117-111">hello beágyazott oktatóanyag lehetővé teszi az elérhető lehetőségekkel ötleteket.</span><span class="sxs-lookup"><span data-stu-id="b5117-111">hello inline tutorial gives you some ideas about what you can do.</span></span>

<span data-ttu-id="b5117-112">Van egy [itt átfogóbb bemutatása](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="b5117-112">There's a [more extensive tour here](app-insights-analytics-tour.md).</span></span>

## <a name="query-your-telemetry"></a><span data-ttu-id="b5117-113">A telemetriai adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b5117-113">Query your telemetry</span></span>
### <a name="write-a-query"></a><span data-ttu-id="b5117-114">Lekérdezés írása</span><span class="sxs-lookup"><span data-stu-id="b5117-114">Write a query</span></span>
![Séma megjelenítése](./media/app-insights-analytics-using/150.png)

<span data-ttu-id="b5117-116">Hello nevéhez hello bal oldalon felsorolt hello táblák kezdődik (vagy hello [tartomány](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) vagy [Unió](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operátorok).</span><span class="sxs-lookup"><span data-stu-id="b5117-116">Begin with hello names of any of hello tables listed on hello left (or hello [range](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) or [union](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operators).</span></span> <span data-ttu-id="b5117-117">Használjon `|` az adatcsatorna egy toocreate [operátorok](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span><span class="sxs-lookup"><span data-stu-id="b5117-117">Use `|` toocreate a pipeline of [operators](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span></span> 

<span data-ttu-id="b5117-118">IntelliSense kéri hello operátorok és hello kifejezés elemek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="b5117-118">IntelliSense prompts you with hello operators and hello expression elements that you can use.</span></span> <span data-ttu-id="b5117-119">Kattintson a hello információs ikon (vagy nyomja le a CTRL + szóköz) tooget hosszabb leírást és példákat a toouse egyes elemei.</span><span class="sxs-lookup"><span data-stu-id="b5117-119">Click hello information icon (or press CTRL+Space) tooget a longer description and examples of how toouse each element.</span></span>

<span data-ttu-id="b5117-120">Lásd: hello [Analytics nyelvi bemutató](app-insights-analytics-tour.md) és [nyelvi referencia](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="b5117-120">See hello [Analytics language tour](app-insights-analytics-tour.md) and [language reference](app-insights-analytics-reference.md).</span></span>

### <a name="run-a-query"></a><span data-ttu-id="b5117-121">A lekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="b5117-121">Run a query</span></span>
![A lekérdezés futtatása](./media/app-insights-analytics-using/130.png)

1. <span data-ttu-id="b5117-123">Egyetlen sortörést használható egy lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="b5117-123">You can use single line breaks in a query.</span></span>
2. <span data-ttu-id="b5117-124">Helyezze el hello kurzor belül vagy hello lekérdezést toorun hello végén.</span><span class="sxs-lookup"><span data-stu-id="b5117-124">Put hello cursor inside or at hello end of hello query you want toorun.</span></span>
3. <span data-ttu-id="b5117-125">Ellenőrizze a lekérdezés hello időtartományát.</span><span class="sxs-lookup"><span data-stu-id="b5117-125">Check hello time range of your query.</span></span> <span data-ttu-id="b5117-126">(Módosíthatja, vagy bírálja felül az által, beleértve a saját [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) záradék a lekérdezés.)</span><span class="sxs-lookup"><span data-stu-id="b5117-126">(You can change it, or override it by including your own [`where...timestamp...`](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) clause in your query.)</span></span>
3. <span data-ttu-id="b5117-127">Kattintson az Ugrás toorun hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b5117-127">Click Go toorun hello query.</span></span>
4. <span data-ttu-id="b5117-128">A lekérdezés nem üres sorok be.</span><span class="sxs-lookup"><span data-stu-id="b5117-128">Don't put blank lines in your query.</span></span> <span data-ttu-id="b5117-129">Beállíthatja, hogy több elkülönített lekérdezések egy lekérdezés lap üres vonallal elválasztva őket.</span><span class="sxs-lookup"><span data-stu-id="b5117-129">You can keep several separated queries in one query tab by separating them with blank lines.</span></span> <span data-ttu-id="b5117-130">Csak az olyan hello lekérdezésekben, amelyekben hello kurzor fut.</span><span class="sxs-lookup"><span data-stu-id="b5117-130">Only hello query that has hello cursor runs.</span></span>

### <a name="save-a-query"></a><span data-ttu-id="b5117-131">Lekérdezés mentése</span><span class="sxs-lookup"><span data-stu-id="b5117-131">Save a query</span></span>
![A lekérdezés mentése](./media/app-insights-analytics-using/140.png)

1. <span data-ttu-id="b5117-133">Hello aktuális lekérdezés fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5117-133">Save hello current query file.</span></span>
2. <span data-ttu-id="b5117-134">Nyissa meg a korábban mentett lekérdezés fájlt.</span><span class="sxs-lookup"><span data-stu-id="b5117-134">Open a saved query file.</span></span>
3. <span data-ttu-id="b5117-135">Hozzon létre egy új lekérdezés fájlt.</span><span class="sxs-lookup"><span data-stu-id="b5117-135">Create a new query file.</span></span>

## <a name="see-hello-details"></a><span data-ttu-id="b5117-136">Hello részletek</span><span class="sxs-lookup"><span data-stu-id="b5117-136">See hello details</span></span>
<span data-ttu-id="b5117-137">Bontsa ki a hello eredmények toosee bármely sorára a tulajdonságok teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="b5117-137">Expand any row in hello results toosee its complete list of properties.</span></span> <span data-ttu-id="b5117-138">Ennél jobban is kibonthatja, bármely tulajdonság, amely strukturált érték – például, egyéni dimenziók vagy hello verem listázása egy kivételt.</span><span class="sxs-lookup"><span data-stu-id="b5117-138">You can further expand any property that is a structured value - for example, custom dimensions, or hello stack listing in an exception.</span></span>

![Bontsa ki a sor](./media/app-insights-analytics-using/070.png)

## <a name="arrange-hello-results"></a><span data-ttu-id="b5117-140">Hello eredmények rendezése</span><span class="sxs-lookup"><span data-stu-id="b5117-140">Arrange hello results</span></span>
<span data-ttu-id="b5117-141">Rendezési, szűréséhez, megjelenítheti és a lekérdezés által visszaadott hello eredmények csoportban.</span><span class="sxs-lookup"><span data-stu-id="b5117-141">You can sort, filter, paginate, and group hello results returned from your query.</span></span>

> [!NOTE]
> <span data-ttu-id="b5117-142">Rendezés, a csoportosítás és a szűrés hello böngészőben ne futtassa újból a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b5117-142">Sorting, grouping, and filtering in hello browser don't re-run your query.</span></span> <span data-ttu-id="b5117-143">Ezek csak a legutóbbi lekérdezés által visszaadott eredményobjektumokban tárolt hello eredmények átrendezését.</span><span class="sxs-lookup"><span data-stu-id="b5117-143">They only rearrange hello results that were returned by your last query.</span></span> 
> 
> <span data-ttu-id="b5117-144">tooperform hello kiszolgálóra, mielőtt hello eredményeinek, a feladatok írni a lekérdezés hello [rendezési](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [összefoglalója](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) és [ahol](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operátorok.</span><span class="sxs-lookup"><span data-stu-id="b5117-144">tooperform these tasks in hello server before hello results are returned, write your query with hello [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) and [where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operators.</span></span>
> 
> 

<span data-ttu-id="b5117-145">Válassza ki az üzembe helyezése hello oszlopokat toosee, például húzza oszlop fejlécek toorearrange, és átméretezési oszlopok a szegélyek húzásával.</span><span class="sxs-lookup"><span data-stu-id="b5117-145">Pick hello columns you'd like toosee, drag column headers toorearrange them, and resize columns by dragging their borders.</span></span>

![Oszlopok rendezése](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a><span data-ttu-id="b5117-147">Rendezésére és szűrésére elemek</span><span class="sxs-lookup"><span data-stu-id="b5117-147">Sort and filter items</span></span>
<span data-ttu-id="b5117-148">Egy oszlop hello vezetője kattintva rendezheti az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="b5117-148">Sort your results by clicking hello head of a column.</span></span> <span data-ttu-id="b5117-149">Kattintson ismét toosort hello más módon, és kattintson a harmadszor toorevert toohello eredeti rendezést a lekérdezés által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="b5117-149">Click again toosort hello other way, and click a third time toorevert toohello original ordering returned by your query.</span></span>

<span data-ttu-id="b5117-150">Használjon hello ikon toonarrow szűrheti a keresést.</span><span class="sxs-lookup"><span data-stu-id="b5117-150">Use hello filter icon toonarrow your search.</span></span>

![Rendezésére és szűrésére oszlopok](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a><span data-ttu-id="b5117-152">Csoport elemek</span><span class="sxs-lookup"><span data-stu-id="b5117-152">Group items</span></span>
<span data-ttu-id="b5117-153">toosort egynél több oszlopok szerinti csoportosítás használata.</span><span class="sxs-lookup"><span data-stu-id="b5117-153">toosort by more than one column, use grouping.</span></span> <span data-ttu-id="b5117-154">Először engedélyeznie, és húzza oszlopfejlécek felett hello tábla hello térbe kerülnek.</span><span class="sxs-lookup"><span data-stu-id="b5117-154">First enable it, and then drag column headers into hello space above hello table.</span></span>

![Csoport](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a><span data-ttu-id="b5117-156">Hiányzik az egyes eredmények?</span><span class="sxs-lookup"><span data-stu-id="b5117-156">Missing some results?</span></span>

<span data-ttu-id="b5117-157">Ha úgy gondolja, hogy nem várt összes hello eredményt is lát, van néhány lehetséges ok.</span><span class="sxs-lookup"><span data-stu-id="b5117-157">If you think you're not seeing all hello results you expected, there are a couple of possible reasons.</span></span>

* <span data-ttu-id="b5117-158">**Tartomány időszűrője**.</span><span class="sxs-lookup"><span data-stu-id="b5117-158">**Time range filter**.</span></span> <span data-ttu-id="b5117-159">Alapértelmezés szerint csak akkor jelenik meg hello eredményeinek utolsó 24 órában.</span><span class="sxs-lookup"><span data-stu-id="b5117-159">By default, you will only see results from hello last 24 hours.</span></span> <span data-ttu-id="b5117-160">Nincs olyan automatikus szűrő, amely korlátozza az eredmények, amelyek a rendszer beolvassa az hello forrástáblákból hello tartományán.</span><span class="sxs-lookup"><span data-stu-id="b5117-160">There is an automatic filter that limits hello range of results that are retrieved from hello source tables.</span></span> 

    <span data-ttu-id="b5117-161">Azonban módosíthatja a hello időtartomány szűrő hello legördülő menü használatával.</span><span class="sxs-lookup"><span data-stu-id="b5117-161">However, you can change hello time range filter by using hello drop-down menu.</span></span>

    <span data-ttu-id="b5117-162">A beállítás felülbírálható hello automatikus tartomány beleértve a saját vagy [ `where  ... timestamp ...` záradék](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) be a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b5117-162">Or you can override hello automatic range by including your own [`where  ... timestamp ...` clause](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) into your query.</span></span> <span data-ttu-id="b5117-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="b5117-163">For example:</span></span>

    `requests | where timestamp > ago('2d')`

* <span data-ttu-id="b5117-164">**Eredmények korlát**.</span><span class="sxs-lookup"><span data-stu-id="b5117-164">**Results limit**.</span></span> <span data-ttu-id="b5117-165">Hello portálról hello eredményének sorain körülbelül 10 KB-os korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="b5117-165">There's a limit of about 10k rows on hello results returned from hello portal.</span></span> <span data-ttu-id="b5117-166">Figyelmeztetés látható, ha meghaladja hello lépjen.</span><span class="sxs-lookup"><span data-stu-id="b5117-166">A warning shows if you go over hello limit.</span></span> <span data-ttu-id="b5117-167">Ebben az esetben, ha hello tábla az eredmények rendezéséhez nem mindig, minden hello tényleges első vagy utolsó eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b5117-167">If that happens, sorting your results in hello table won't always show you all hello actual first or last results.</span></span> 

    <span data-ttu-id="b5117-168">Akkor célszerű tooavoid elérte-e hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="b5117-168">It's good practice tooavoid hitting hello limit.</span></span> <span data-ttu-id="b5117-169">Hello idő a tartomány szűrővel, vagy használjon például operátorok:</span><span class="sxs-lookup"><span data-stu-id="b5117-169">Use hello time range filter, or use operators such as:</span></span>

  * [<span data-ttu-id="b5117-170">TOP 100 alapján időbélyeg</span><span class="sxs-lookup"><span data-stu-id="b5117-170">top 100 by timestamp</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [<span data-ttu-id="b5117-171">100 igénybe</span><span class="sxs-lookup"><span data-stu-id="b5117-171">take 100</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [<span data-ttu-id="b5117-172">összefoglalója</span><span class="sxs-lookup"><span data-stu-id="b5117-172">summarize </span></span>](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [<span data-ttu-id="b5117-173">Ha időbélyeg > ago(3d)</span><span class="sxs-lookup"><span data-stu-id="b5117-173">where timestamp > ago(3d)</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

<span data-ttu-id="b5117-174">(Több mint 10 KB-os sor szeretné?</span><span class="sxs-lookup"><span data-stu-id="b5117-174">(Want more than 10k rows?</span></span> <span data-ttu-id="b5117-175">Érdemes lehet [a folyamatos exportálás](app-insights-export-telemetry.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="b5117-175">Consider using [Continuous Export](app-insights-export-telemetry.md) instead.</span></span> <span data-ttu-id="b5117-176">Elemzés készült elemzés, nem pedig nyers adatok lekérése során.)</span><span class="sxs-lookup"><span data-stu-id="b5117-176">Analytics is designed for analysis, rather than retrieving raw data.)</span></span>

## <a name="diagrams"></a><span data-ttu-id="b5117-177">Diagramok</span><span class="sxs-lookup"><span data-stu-id="b5117-177">Diagrams</span></span>
<span data-ttu-id="b5117-178">Milyen diagram hello típusának kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="b5117-178">Select hello type of diagram you'd like:</span></span>

![Válassza ki a diagram típusát](./media/app-insights-analytics-using/230.png)

<span data-ttu-id="b5117-180">Ha több oszlopot hello megfelelő típusú, kiválaszthatja a hello x és y-tengelyek és dimenziók toosplit hello az eredmények oszlop.</span><span class="sxs-lookup"><span data-stu-id="b5117-180">If you have several columns of hello right types, you can choose hello x and y axes, and a column of dimensions toosplit hello results by.</span></span>

<span data-ttu-id="b5117-181">Alapértelmezés szerint eredmények kezdetben táblázatként jelenik meg, és hello diagram manuálisan válassza.</span><span class="sxs-lookup"><span data-stu-id="b5117-181">By default, results are initially displayed as a table, and you select hello diagram manually.</span></span> <span data-ttu-id="b5117-182">De használhat hello [irányelv leképezési](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) hello végén egy lekérdezés tooselect ábrázoló diagram.</span><span class="sxs-lookup"><span data-stu-id="b5117-182">But you can use hello [render directive](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) at hello end of a query tooselect a diagram.</span></span>

### <a name="analytics-diagnostics"></a><span data-ttu-id="b5117-183">Elemzés diagnosztika</span><span class="sxs-lookup"><span data-stu-id="b5117-183">Analytics diagnostics</span></span>


<span data-ttu-id="b5117-184">Egy timechart a hirtelen csúcs vagy lépése az adatokat, ha megjelenik a kijelölt pont hello sor.</span><span class="sxs-lookup"><span data-stu-id="b5117-184">On a timechart, if there is a sudden spike or step in your data, you may see a highlighted point on hello line.</span></span> <span data-ttu-id="b5117-185">Ez azt jelzi, hogy elemzés diagnosztika észlelt hello hirtelen változás kiszűrhetők tulajdonságok kombinációja.</span><span class="sxs-lookup"><span data-stu-id="b5117-185">This indicates that Analytics Diagnostics has identified a combination of properties that filter out hello sudden change.</span></span> <span data-ttu-id="b5117-186">Kattintson a hello pont tooget részletesebb hello szűrő és toosee hello szűrt verzióját.</span><span class="sxs-lookup"><span data-stu-id="b5117-186">Click hello point tooget more detail on hello filter, and toosee hello filtered version.</span></span> <span data-ttu-id="b5117-187">Ez lehet, hogy könnyebben azonosíthassa milyen okozta hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="b5117-187">This may help you identify what caused hello change.</span></span> 

[<span data-ttu-id="b5117-188">További információ az elemzés diagnosztika</span><span class="sxs-lookup"><span data-stu-id="b5117-188">Learn more about Analytics Diagnostics</span></span>](app-insights-analytics-diagnostics.md)


![Elemzés diagnosztika](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-toodashboard"></a><span data-ttu-id="b5117-190">PIN-kód toodashboard</span><span class="sxs-lookup"><span data-stu-id="b5117-190">Pin toodashboard</span></span>
<span data-ttu-id="b5117-191">A diagram- vagy tooone a rögzíthető a [irányítópultok megosztott](app-insights-dashboards.md) -kattintson hello PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="b5117-191">You can pin a diagram or table tooone of your [shared dashboards](app-insights-dashboards.md) - just click hello pin.</span></span> <span data-ttu-id="b5117-192">(Előfordulhat, hogy túl kell[frissítési az alkalmazás csomagazonosítóját alapú](app-insights-pricing.md) tooturn a szolgáltatást.)</span><span class="sxs-lookup"><span data-stu-id="b5117-192">(You might need too[upgrade your app's pricing package](app-insights-pricing.md) tooturn on this feature.)</span></span> 

![Kattintson a hello PIN-kód](./media/app-insights-analytics-using/pin-01.png)

<span data-ttu-id="b5117-194">Ez azt jelenti, hogy a figyelheti hello teljesítmény vagy a webszolgáltatások használatát irányítópult toohelp hozzáfoghat, amikor megadható elég bonyolult elemzés hello mellett más metrikákkal.</span><span class="sxs-lookup"><span data-stu-id="b5117-194">This means that, when you put together a dashboard toohelp you monitor hello performance or usage of your web services, you can include quite complex analysis alongside hello other metrics.</span></span> 

<span data-ttu-id="b5117-195">Egy tábla toohello irányítópult rögzíthető-e azt négy vagy kevesebb oszlopot.</span><span class="sxs-lookup"><span data-stu-id="b5117-195">You can pin a table toohello dashboard, if it has four or fewer columns.</span></span> <span data-ttu-id="b5117-196">Csak hello első hét sorok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b5117-196">Only hello top seven rows are displayed.</span></span>

### <a name="dashboard-refresh"></a><span data-ttu-id="b5117-197">Irányítópult frissítése</span><span class="sxs-lookup"><span data-stu-id="b5117-197">Dashboard refresh</span></span>
<span data-ttu-id="b5117-198">hello rögzítve diagram toohello irányítópult futtatásával újra hello lekérdezés körülbelül óránként automatikusan frissül.</span><span class="sxs-lookup"><span data-stu-id="b5117-198">hello chart pinned toohello dashboard is refreshed automatically by re-running hello query approximately every hours.</span></span> <span data-ttu-id="b5117-199">Hello frissítés gombra is kattinthat.</span><span class="sxs-lookup"><span data-stu-id="b5117-199">You can also click hello Refresh button.</span></span>

### <a name="automatic-simplifications"></a><span data-ttu-id="b5117-200">Automatikus egyszerűbb</span><span class="sxs-lookup"><span data-stu-id="b5117-200">Automatic simplifications</span></span>

<span data-ttu-id="b5117-201">Bizonyos egyszerűbb alkalmazott tooa diagram esetén tooa irányítópult rögzíti azt.</span><span class="sxs-lookup"><span data-stu-id="b5117-201">Certain simplifications are applied tooa chart when you pin it tooa dashboard.</span></span>

<span data-ttu-id="b5117-202">**Idő korlátozás:** lekérdezések automatikusan korlátozott toohello az elmúlt 14 napban.</span><span class="sxs-lookup"><span data-stu-id="b5117-202">**Time restriction:** Queries are automatically limited toohello past 14 days.</span></span> <span data-ttu-id="b5117-203">hello hatása van hello azonos módon, ha a lekérdezésben `where timestamp > ago(14d)`.</span><span class="sxs-lookup"><span data-stu-id="b5117-203">hello effect is hello same as if your query includes `where timestamp > ago(14d)`.</span></span>

<span data-ttu-id="b5117-204">**Count korlátozás bin:** bin Ha nagy mennyiségű diszkrét bins (általában a sávdiagram), kevesebb ki van töltve bins automatikusan sorolhatók egy egyetlen "egyéb" hello rendelkező diagram megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b5117-204">**Bin count restriction:** If you display a chart that has a lot of discrete bins (typically a bar chart), hello less populated bins are automatically grouped into a single "others" bin.</span></span> <span data-ttu-id="b5117-205">Ha például a lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="b5117-205">For example, this query:</span></span>

    requests | summarize count_search = count() by client_CountryOrRegion

<span data-ttu-id="b5117-206">így néz Analytics:</span><span class="sxs-lookup"><span data-stu-id="b5117-206">looks like this in Analytics:</span></span>

![A diagramon az hosszú utóhívás](./media/app-insights-analytics-using/pin-07.png)

<span data-ttu-id="b5117-208">de amikor azt tooa irányítópult, néz ki:</span><span class="sxs-lookup"><span data-stu-id="b5117-208">but when you pin it tooa dashboard, it looks like this:</span></span>

![A diagramon az korlátozott bins](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-tooexcel"></a><span data-ttu-id="b5117-210">TooExcel exportálása</span><span class="sxs-lookup"><span data-stu-id="b5117-210">Export tooExcel</span></span>
<span data-ttu-id="b5117-211">A lekérdezés futtatása után letölthet egy CSV-fájlt.</span><span class="sxs-lookup"><span data-stu-id="b5117-211">After you've run a query, you can download a .csv file.</span></span> <span data-ttu-id="b5117-212">Kattintson a **exportálása Excel**.</span><span class="sxs-lookup"><span data-stu-id="b5117-212">Click **Export,  Excel**.</span></span>

## <a name="export-toopower-bi"></a><span data-ttu-id="b5117-213">Exportálás tooPower BI</span><span class="sxs-lookup"><span data-stu-id="b5117-213">Export tooPower BI</span></span>
<span data-ttu-id="b5117-214">Hello kurzor elhelyezése egy lekérdezésben, és válassza a **exportálásához a Power BI**.</span><span class="sxs-lookup"><span data-stu-id="b5117-214">Put hello cursor in a query and choose **Export, Power BI**.</span></span>

![Elemzés tooPower BI exportálása](./media/app-insights-analytics-using/240.png)

<span data-ttu-id="b5117-216">Hello lekérdezés futtatása a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="b5117-216">You run hello query in Power BI.</span></span> <span data-ttu-id="b5117-217">Be lehet állítani toorefresh ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b5117-217">You can set it toorefresh on a schedule.</span></span>

<span data-ttu-id="b5117-218">A Power BI irányítópultjai összefogására olyan adatokat különböző forrásokból hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b5117-218">With Power BI, you can create dashboards that bring together data from a wide variety of sources.</span></span>

[<span data-ttu-id="b5117-219">További tudnivalók az Exportálás tooPower BI</span><span class="sxs-lookup"><span data-stu-id="b5117-219">Learn more about export tooPower BI</span></span>](app-insights-export-power-bi.md)

## <a name="deep-link"></a><span data-ttu-id="b5117-220">Áruházra mutató mélyhivatkozás</span><span class="sxs-lookup"><span data-stu-id="b5117-220">Deep link</span></span>

<span data-ttu-id="b5117-221">Szerezzen be egy hivatkozást a **exportálás megosztás hivatkozás** , hogy tooanother felhasználói küldhet.</span><span class="sxs-lookup"><span data-stu-id="b5117-221">Get a link under **Export, Share link** that you can send tooanother user.</span></span> <span data-ttu-id="b5117-222">Ha hello felhasználó rendelkezik [hozzáférés tooyour erőforráscsoport](app-insights-resources-roles-access-control.md), hello lekérdezés hello Analytics UI nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b5117-222">Provided hello user has [access tooyour resource group](app-insights-resources-roles-access-control.md), hello query will open in hello Analytics UI.</span></span>

<span data-ttu-id="b5117-223">(Hello hivatkozásban, hello lekérdezés szövegét után "? q =", gzip tömörített és base-64 kódolású.</span><span class="sxs-lookup"><span data-stu-id="b5117-223">(In hello link, hello query text appears after "?q=", gzip compressed and base-64 encoded.</span></span> <span data-ttu-id="b5117-224">Létrehozható kód toogenerate mélyhivatkozással toousers adni.</span><span class="sxs-lookup"><span data-stu-id="b5117-224">You could write code toogenerate deep links that you provide toousers.</span></span> <span data-ttu-id="b5117-225">Azonban hello ajánlott módja toorun Analytics kódból hello segítségével [REST API](https://dev.applicationinsights.io/).)</span><span class="sxs-lookup"><span data-stu-id="b5117-225">However, hello recommended way toorun Analytics from code is by using hello [REST API](https://dev.applicationinsights.io/).)</span></span>


## <a name="automation"></a><span data-ttu-id="b5117-226">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="b5117-226">Automation</span></span>

<span data-ttu-id="b5117-227">Használjon hello [Data Access REST API](https://dev.applicationinsights.io/) toorun elemzési lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="b5117-227">Use hello  [Data Access REST API](https://dev.applicationinsights.io/) toorun Analytics queries.</span></span> <span data-ttu-id="b5117-228">[Például](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (a PowerShell használatával):</span><span class="sxs-lookup"><span data-stu-id="b5117-228">[For example](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (using PowerShell):</span></span>

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

<span data-ttu-id="b5117-229">Hello Analytics felhasználói felület, eltérően hello REST API-t adja hozzá automatikusan korlátozás Timestamp típusú tooyour lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="b5117-229">Unlike hello Analytics UI, hello REST API does not automatically add any timestamp limitation tooyour queries.</span></span> <span data-ttu-id="b5117-230">Ne feledje tooadd saját where záradék, hatalmas válaszok első tooavoid.</span><span class="sxs-lookup"><span data-stu-id="b5117-230">Remember tooadd your own where-clause, tooavoid getting huge responses.</span></span>



## <a name="import-data"></a><span data-ttu-id="b5117-231">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="b5117-231">Import data</span></span>

<span data-ttu-id="b5117-232">Adatokat importálhat egy CSV-fájl.</span><span class="sxs-lookup"><span data-stu-id="b5117-232">You can import data from a CSV file.</span></span> <span data-ttu-id="b5117-233">A tipikus használati tooimport statikus adatok csatlakozhat a a telemetriai adatokból táblákkal.</span><span class="sxs-lookup"><span data-stu-id="b5117-233">A typical usage is tooimport static data that you can join with tables from your telemetry.</span></span> 

<span data-ttu-id="b5117-234">Hitelesített felhasználók a telemetriai adatokat egy aliast vagy rejtjelezett azonosító által azonosítható, ha például sikerült aliasok tooreal neveket le tábla importálni.</span><span class="sxs-lookup"><span data-stu-id="b5117-234">For example, if authenticated users are identified in your telemetry by an alias or obfuscated id, you could import a table that maps aliases tooreal names.</span></span> <span data-ttu-id="b5117-235">Hello – kéréstelemetria illesztés elvégzésével hello Analytics-jelentések a valódi névvel azonosítani a felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="b5117-235">By performing a join on hello request telemetry, you can identify users by their real names in hello Analytics reports.</span></span>

### <a name="define-your-data-schema"></a><span data-ttu-id="b5117-236">Adja meg a Adatséma</span><span class="sxs-lookup"><span data-stu-id="b5117-236">Define your data schema</span></span>

1. <span data-ttu-id="b5117-237">Kattintson a **beállítások** (a bal felső), majd **adatforrások**.</span><span class="sxs-lookup"><span data-stu-id="b5117-237">Click **Settings** (at top left) and then **Data Sources**.</span></span> 
2. <span data-ttu-id="b5117-238">Adja hozzá egy adatforrást, hello utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="b5117-238">Add a data source, following hello instructions.</span></span> <span data-ttu-id="b5117-239">Biztosan kéri toosupply hello adatokat, amelyek tartalmaznia kell legalább 10 sorok mintáját.</span><span class="sxs-lookup"><span data-stu-id="b5117-239">You are asked toosupply a sample of hello data, which should include at least ten rows.</span></span> <span data-ttu-id="b5117-240">Majd javítsa ki az hello séma.</span><span class="sxs-lookup"><span data-stu-id="b5117-240">You then correct hello schema.</span></span>

<span data-ttu-id="b5117-241">Ez határozza meg az adatforrás, amely követően tooimport egyedi táblázatot használnak.</span><span class="sxs-lookup"><span data-stu-id="b5117-241">This defines a data source, which you can then use tooimport individual tables.</span></span>

### <a name="import-a-table"></a><span data-ttu-id="b5117-242">A tábla importálása</span><span class="sxs-lookup"><span data-stu-id="b5117-242">Import a table</span></span>

1. <span data-ttu-id="b5117-243">Nyissa meg az adatforrása definíciója hello listából.</span><span class="sxs-lookup"><span data-stu-id="b5117-243">Open your data source definition from hello list.</span></span>
2. <span data-ttu-id="b5117-244">Kattintson a "Feltöltés lehetőségre", és kövesse a hello utasításokat tooupload hello tábla.</span><span class="sxs-lookup"><span data-stu-id="b5117-244">Click "Upload" and follow hello instructions tooupload hello table.</span></span> <span data-ttu-id="b5117-245">Ebbe beletartozik egy hívás tooa REST API-t, és így könnyen tooautomate.</span><span class="sxs-lookup"><span data-stu-id="b5117-245">This involves a call tooa REST API, and so it is easy tooautomate.</span></span> 

<span data-ttu-id="b5117-246">A tábla már elemzési lekérdezések használható.</span><span class="sxs-lookup"><span data-stu-id="b5117-246">Your table is now available for use in Analytics queries.</span></span> <span data-ttu-id="b5117-247">Megjelenik az elemzés</span><span class="sxs-lookup"><span data-stu-id="b5117-247">It will appear in Analytics</span></span> 

### <a name="use-hello-table"></a><span data-ttu-id="b5117-248">Hello táblázat</span><span class="sxs-lookup"><span data-stu-id="b5117-248">Use hello table</span></span>

<span data-ttu-id="b5117-249">Tegyük fel, az adatforrása definíciója nevezik `usermap`, és arról, hogy rendelkezik-e két mező `realName` és `user_AuthenticatedId`.</span><span class="sxs-lookup"><span data-stu-id="b5117-249">Let's suppose your data source definition is called `usermap`, and that it has two fields, `realName` and `user_AuthenticatedId`.</span></span> <span data-ttu-id="b5117-250">Hello `requests` is táblához nevű mező `user_AuthenticatedId`, így könnyen toojoin őket:</span><span class="sxs-lookup"><span data-stu-id="b5117-250">hello `requests` table also has a field named `user_AuthenticatedId`, so it's easy toojoin them:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
<span data-ttu-id="b5117-251">hello kérelmek eredményül kapott tábla tartalmaz egy olyan további oszlop `realName`.</span><span class="sxs-lookup"><span data-stu-id="b5117-251">hello resulting table of requests has an additional column, `realName`.</span></span>

### <a name="import-from-logstash"></a><span data-ttu-id="b5117-252">Importálás a LogStash</span><span class="sxs-lookup"><span data-stu-id="b5117-252">Import from LogStash</span></span>

<span data-ttu-id="b5117-253">Ha [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), elemzés tooquery a naplók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b5117-253">If you use [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), you can use Analytics tooquery your logs.</span></span> <span data-ttu-id="b5117-254">Használjon hello [beépülő modul, amely adatok csővezeték elemzés be](https://github.com/Microsoft/logstash-output-application-insights).</span><span class="sxs-lookup"><span data-stu-id="b5117-254">Use hello [plugin that pipes data into Analytics](https://github.com/Microsoft/logstash-output-application-insights).</span></span> 

## <a name="video"></a><span data-ttu-id="b5117-255">Videó</span><span class="sxs-lookup"><span data-stu-id="b5117-255">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

