---
title: "Elemzés - a hatékony keresési eszköz az Azure Application Insights segítségével |} Microsoft Docs"
description: "Az elemzés, az Application Insights a hatékony diagnosztikai keresési eszköz használatával. "
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
ms.openlocfilehash: 9f7c1744db52b1c2f374fd5655e2c66b5f61bacf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-analytics-in-application-insights"></a><span data-ttu-id="59f48-103">Az Application Insightsban Analytics használatával</span><span class="sxs-lookup"><span data-stu-id="59f48-103">Using Analytics in Application Insights</span></span>
<span data-ttu-id="59f48-104">[Elemzés](app-insights-analytics.md) a hatékony keresési funkciója [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="59f48-104">[Analytics](app-insights-analytics.md) is the powerful search feature of [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="59f48-105">Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.</span><span class="sxs-lookup"><span data-stu-id="59f48-105">These pages describe the Log Analytics query language.</span></span>

* <span data-ttu-id="59f48-106">**[A bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span><span class="sxs-lookup"><span data-stu-id="59f48-106">**[Watch the introductory video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.</span></span>
* <span data-ttu-id="59f48-107">**[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás nem adatok küldése az Application Insights még.</span><span class="sxs-lookup"><span data-stu-id="59f48-107">**[Test drive Analytics on our simulated data](https://analytics.applicationinsights.io/demo)** if your app isn't sending data to Application Insights yet.</span></span>

## <a name="open-analytics"></a><span data-ttu-id="59f48-108">Nyissa meg elemzés</span><span class="sxs-lookup"><span data-stu-id="59f48-108">Open Analytics</span></span>
<span data-ttu-id="59f48-109">A az alkalmazás otthoni erőforrás az Application Insightsban kattintson az elemzés.</span><span class="sxs-lookup"><span data-stu-id="59f48-109">From your app's home resource in Application Insights, click Analytics.</span></span>

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics-using/001.png)

<span data-ttu-id="59f48-111">A beágyazott oktatóanyag lehetővé teszi az elérhető lehetőségekkel ötleteket.</span><span class="sxs-lookup"><span data-stu-id="59f48-111">The inline tutorial gives you some ideas about what you can do.</span></span>

<span data-ttu-id="59f48-112">Van egy [itt átfogóbb bemutatása](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="59f48-112">There's a [more extensive tour here](app-insights-analytics-tour.md).</span></span>

## <a name="query-your-telemetry"></a><span data-ttu-id="59f48-113">A telemetriai adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="59f48-113">Query your telemetry</span></span>
### <a name="write-a-query"></a><span data-ttu-id="59f48-114">Lekérdezés írása</span><span class="sxs-lookup"><span data-stu-id="59f48-114">Write a query</span></span>
![Séma megjelenítése](./media/app-insights-analytics-using/150.png)

<span data-ttu-id="59f48-116">A bal oldalon felsorolt táblákat nevei kezdődnie (vagy a [tartomány](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) vagy [Unió](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operátorok).</span><span class="sxs-lookup"><span data-stu-id="59f48-116">Begin with the names of any of the tables listed on the left (or the [range](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) or [union](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operators).</span></span> <span data-ttu-id="59f48-117">Használjon `|` a folyamatokat létrehozni [operátorok](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span><span class="sxs-lookup"><span data-stu-id="59f48-117">Use `|` to create a pipeline of [operators](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html).</span></span> 

<span data-ttu-id="59f48-118">IntelliSense kéri az operátorok és a kifejezés elemek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="59f48-118">IntelliSense prompts you with the operators and the expression elements that you can use.</span></span> <span data-ttu-id="59f48-119">Kattintson a ikonra (vagy nyomja meg a CTRL + szóköz) hosszabb leírást és a példákat egyes elemeinek a használatára.</span><span class="sxs-lookup"><span data-stu-id="59f48-119">Click the information icon (or press CTRL+Space) to get a longer description and examples of how to use each element.</span></span>

<span data-ttu-id="59f48-120">Tekintse meg a [Analytics nyelvi bemutató](app-insights-analytics-tour.md) és [nyelvi referencia](app-insights-analytics-reference.md).</span><span class="sxs-lookup"><span data-stu-id="59f48-120">See the [Analytics language tour](app-insights-analytics-tour.md) and [language reference](app-insights-analytics-reference.md).</span></span>

### <a name="run-a-query"></a><span data-ttu-id="59f48-121">A lekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="59f48-121">Run a query</span></span>
![A lekérdezés futtatása](./media/app-insights-analytics-using/130.png)

1. <span data-ttu-id="59f48-123">Egyetlen sortörést használható egy lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="59f48-123">You can use single line breaks in a query.</span></span>
2. <span data-ttu-id="59f48-124">A kurzor belül, vagy a futtatni kívánt lekérdezés végén elhelyezése.</span><span class="sxs-lookup"><span data-stu-id="59f48-124">Put the cursor inside or at the end of the query you want to run.</span></span>
3. <span data-ttu-id="59f48-125">Ellenőrizze a lekérdezés az időtartományt.</span><span class="sxs-lookup"><span data-stu-id="59f48-125">Check the time range of your query.</span></span> <span data-ttu-id="59f48-126">(Módosíthatja, vagy bírálja felül az által, beleértve a saját [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) záradék a lekérdezés.)</span><span class="sxs-lookup"><span data-stu-id="59f48-126">(You can change it, or override it by including your own [`where...timestamp...`](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) clause in your query.)</span></span>
3. <span data-ttu-id="59f48-127">A lekérdezés futtatásához az Ugrás gombra.</span><span class="sxs-lookup"><span data-stu-id="59f48-127">Click Go to run the query.</span></span>
4. <span data-ttu-id="59f48-128">A lekérdezés nem üres sorok be.</span><span class="sxs-lookup"><span data-stu-id="59f48-128">Don't put blank lines in your query.</span></span> <span data-ttu-id="59f48-129">Beállíthatja, hogy több elkülönített lekérdezések egy lekérdezés lap üres vonallal elválasztva őket.</span><span class="sxs-lookup"><span data-stu-id="59f48-129">You can keep several separated queries in one query tab by separating them with blank lines.</span></span> <span data-ttu-id="59f48-130">Csak a lekérdezés, amely rendelkezik a kurzor fut.</span><span class="sxs-lookup"><span data-stu-id="59f48-130">Only the query that has the cursor runs.</span></span>

### <a name="save-a-query"></a><span data-ttu-id="59f48-131">Lekérdezés mentése</span><span class="sxs-lookup"><span data-stu-id="59f48-131">Save a query</span></span>
![A lekérdezés mentése](./media/app-insights-analytics-using/140.png)

1. <span data-ttu-id="59f48-133">Az aktuális lekérdezés fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="59f48-133">Save the current query file.</span></span>
2. <span data-ttu-id="59f48-134">Nyissa meg a korábban mentett lekérdezés fájlt.</span><span class="sxs-lookup"><span data-stu-id="59f48-134">Open a saved query file.</span></span>
3. <span data-ttu-id="59f48-135">Hozzon létre egy új lekérdezés fájlt.</span><span class="sxs-lookup"><span data-stu-id="59f48-135">Create a new query file.</span></span>

## <a name="see-the-details"></a><span data-ttu-id="59f48-136">Részletek</span><span class="sxs-lookup"><span data-stu-id="59f48-136">See the details</span></span>
<span data-ttu-id="59f48-137">Bontsa ki az összes sort az eredmények között, a tulajdonságok teljes listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="59f48-137">Expand any row in the results to see its complete list of properties.</span></span> <span data-ttu-id="59f48-138">Ennél jobban is kibonthatja, bármely tulajdonsága strukturált érték – például, egyéni dimenziók vagy a veremben egy kivételt listázása.</span><span class="sxs-lookup"><span data-stu-id="59f48-138">You can further expand any property that is a structured value - for example, custom dimensions, or the stack listing in an exception.</span></span>

![Bontsa ki a sor](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a><span data-ttu-id="59f48-140">Az eredmények rendezése</span><span class="sxs-lookup"><span data-stu-id="59f48-140">Arrange the results</span></span>
<span data-ttu-id="59f48-141">Rendezési, szűréséhez, megjelenítheti és az a lekérdezés által visszaadott eredmények csoportban.</span><span class="sxs-lookup"><span data-stu-id="59f48-141">You can sort, filter, paginate, and group the results returned from your query.</span></span>

> [!NOTE]
> <span data-ttu-id="59f48-142">Rendezés, a csoportosítás és a szűrés a böngészőben ne futtassa újból a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="59f48-142">Sorting, grouping, and filtering in the browser don't re-run your query.</span></span> <span data-ttu-id="59f48-143">Ezek csak az eredményeket a legutóbbi lekérdezés által visszaadott eredményobjektumokban tárolt átrendezését.</span><span class="sxs-lookup"><span data-stu-id="59f48-143">They only rearrange the results that were returned by your last query.</span></span> 
> 
> <span data-ttu-id="59f48-144">Ezen feladatok végrehajtásával a kiszolgálón, mielőtt a rendszer visszairányítja az eredményeket, a lekérdezés írása a [rendezési](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [összefoglalója](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) és [ahol](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operátorok.</span><span class="sxs-lookup"><span data-stu-id="59f48-144">To perform these tasks in the server before the results are returned, write your query with the [sort](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [summarize](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) and [where](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operators.</span></span>
> 
> 

<span data-ttu-id="59f48-145">Válasszon az annak megtekintéséhez, húzza az oszlopfejlécek átrendezés, húzza a határok automatikus oszlopok.</span><span class="sxs-lookup"><span data-stu-id="59f48-145">Pick the columns you'd like to see, drag column headers to rearrange them, and resize columns by dragging their borders.</span></span>

![Oszlopok rendezése](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a><span data-ttu-id="59f48-147">Rendezésére és szűrésére elemek</span><span class="sxs-lookup"><span data-stu-id="59f48-147">Sort and filter items</span></span>
<span data-ttu-id="59f48-148">Egy oszlop vezetője kattintva rendezheti az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="59f48-148">Sort your results by clicking the head of a column.</span></span> <span data-ttu-id="59f48-149">Kattintson ismét a más módon rendezheti, majd kattintson a harmadik pedig visszatérhet az eredeti rendezést a lekérdezés által visszaadott idő.</span><span class="sxs-lookup"><span data-stu-id="59f48-149">Click again to sort the other way, and click a third time to revert to the original ordering returned by your query.</span></span>

<span data-ttu-id="59f48-150">A szűrő ikon segítségével szűkítse a keresést.</span><span class="sxs-lookup"><span data-stu-id="59f48-150">Use the filter icon to narrow your search.</span></span>

![Rendezésére és szűrésére oszlopok](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a><span data-ttu-id="59f48-152">Csoport elemek</span><span class="sxs-lookup"><span data-stu-id="59f48-152">Group items</span></span>
<span data-ttu-id="59f48-153">Több oszlop szerinti rendezéshez, csoportosítás használata.</span><span class="sxs-lookup"><span data-stu-id="59f48-153">To sort by more than one column, use grouping.</span></span> <span data-ttu-id="59f48-154">Először engedélyeznie, és húzza oszlopfejlécek a táblázat felett térbe kerülnek.</span><span class="sxs-lookup"><span data-stu-id="59f48-154">First enable it, and then drag column headers into the space above the table.</span></span>

![Csoport](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a><span data-ttu-id="59f48-156">Hiányzik az egyes eredmények?</span><span class="sxs-lookup"><span data-stu-id="59f48-156">Missing some results?</span></span>

<span data-ttu-id="59f48-157">Ha úgy gondolja, hogy nem minden, a várt eredményt is lát, van néhány lehetséges ok.</span><span class="sxs-lookup"><span data-stu-id="59f48-157">If you think you're not seeing all the results you expected, there are a couple of possible reasons.</span></span>

* <span data-ttu-id="59f48-158">**Tartomány időszűrője**.</span><span class="sxs-lookup"><span data-stu-id="59f48-158">**Time range filter**.</span></span> <span data-ttu-id="59f48-159">Alapértelmezés szerint az elmúlt 24 órában származó eredmények csak akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="59f48-159">By default, you will only see results from the last 24 hours.</span></span> <span data-ttu-id="59f48-160">Az automatikus szűrő, amely korlátozza az eredmények, amelyek a forrástáblákból lekérése tartományon van.</span><span class="sxs-lookup"><span data-stu-id="59f48-160">There is an automatic filter that limits the range of results that are retrieved from the source tables.</span></span> 

    <span data-ttu-id="59f48-161">Azonban módosíthatja az időtartományt a legördülő menü használatával szűrő.</span><span class="sxs-lookup"><span data-stu-id="59f48-161">However, you can change the time range filter by using the drop-down menu.</span></span>

    <span data-ttu-id="59f48-162">A beállítás felülbírálható az automatikus tartomány beleértve a saját vagy [ `where  ... timestamp ...` záradék](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) be a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="59f48-162">Or you can override the automatic range by including your own [`where  ... timestamp ...` clause](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) into your query.</span></span> <span data-ttu-id="59f48-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="59f48-163">For example:</span></span>

    `requests | where timestamp > ago('2d')`

* <span data-ttu-id="59f48-164">**Eredmények korlát**.</span><span class="sxs-lookup"><span data-stu-id="59f48-164">**Results limit**.</span></span> <span data-ttu-id="59f48-165">A portál által visszaadott eredmények körülbelül 10 KB-os sorok korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="59f48-165">There's a limit of about 10k rows on the results returned from the portal.</span></span> <span data-ttu-id="59f48-166">Figyelmeztetést jeleníti meg, ha meghaladja a lépjen.</span><span class="sxs-lookup"><span data-stu-id="59f48-166">A warning shows if you go over the limit.</span></span> <span data-ttu-id="59f48-167">Ha ez előfordul, a tábla az eredmények rendezéséhez nem mindig, minden a tényleges első vagy utolsó eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="59f48-167">If that happens, sorting your results in the table won't always show you all the actual first or last results.</span></span> 

    <span data-ttu-id="59f48-168">Tanácsos elérte-e a korlát elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="59f48-168">It's good practice to avoid hitting the limit.</span></span> <span data-ttu-id="59f48-169">Az idő a tartomány szűrővel, vagy használjon operátorok például:</span><span class="sxs-lookup"><span data-stu-id="59f48-169">Use the time range filter, or use operators such as:</span></span>

  * [<span data-ttu-id="59f48-170">TOP 100 alapján időbélyeg</span><span class="sxs-lookup"><span data-stu-id="59f48-170">top 100 by timestamp</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [<span data-ttu-id="59f48-171">100 igénybe</span><span class="sxs-lookup"><span data-stu-id="59f48-171">take 100</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [<span data-ttu-id="59f48-172">összefoglalója</span><span class="sxs-lookup"><span data-stu-id="59f48-172">summarize </span></span>](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [<span data-ttu-id="59f48-173">Ha időbélyeg > ago(3d)</span><span class="sxs-lookup"><span data-stu-id="59f48-173">where timestamp > ago(3d)</span></span>](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

<span data-ttu-id="59f48-174">(Több mint 10 KB-os sor szeretné?</span><span class="sxs-lookup"><span data-stu-id="59f48-174">(Want more than 10k rows?</span></span> <span data-ttu-id="59f48-175">Érdemes lehet [a folyamatos exportálás](app-insights-export-telemetry.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="59f48-175">Consider using [Continuous Export](app-insights-export-telemetry.md) instead.</span></span> <span data-ttu-id="59f48-176">Elemzés készült elemzés, nem pedig nyers adatok lekérése során.)</span><span class="sxs-lookup"><span data-stu-id="59f48-176">Analytics is designed for analysis, rather than retrieving raw data.)</span></span>

## <a name="diagrams"></a><span data-ttu-id="59f48-177">Diagramok</span><span class="sxs-lookup"><span data-stu-id="59f48-177">Diagrams</span></span>
<span data-ttu-id="59f48-178">Válassza ki a kívánt diagram típusát:</span><span class="sxs-lookup"><span data-stu-id="59f48-178">Select the type of diagram you'd like:</span></span>

![Válassza ki a diagram típusát](./media/app-insights-analytics-using/230.png)

<span data-ttu-id="59f48-180">Ha rendelkezik a megfelelő típusú több oszlopot, kiválaszthatja az x és y-tengelyek és dimenziókat osztani az eredményeket tartalmazó oszlop.</span><span class="sxs-lookup"><span data-stu-id="59f48-180">If you have several columns of the right types, you can choose the x and y axes, and a column of dimensions to split the results by.</span></span>

<span data-ttu-id="59f48-181">Alapértelmezés szerint eredmények kezdetben táblázatként jelenik meg, és manuálisan jelölje ki a diagramot.</span><span class="sxs-lookup"><span data-stu-id="59f48-181">By default, results are initially displayed as a table, and you select the diagram manually.</span></span> <span data-ttu-id="59f48-182">De használhatja a [irányelv leképezési](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) egy lekérdezést a diagram kiválasztásához végén.</span><span class="sxs-lookup"><span data-stu-id="59f48-182">But you can use the [render directive](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) at the end of a query to select a diagram.</span></span>

### <a name="analytics-diagnostics"></a><span data-ttu-id="59f48-183">Elemzés diagnosztika</span><span class="sxs-lookup"><span data-stu-id="59f48-183">Analytics diagnostics</span></span>


<span data-ttu-id="59f48-184">Egy timechart a hirtelen csúcs vagy lépése az adatokat, ha megjelenik a kijelölt pont a sor.</span><span class="sxs-lookup"><span data-stu-id="59f48-184">On a timechart, if there is a sudden spike or step in your data, you may see a highlighted point on the line.</span></span> <span data-ttu-id="59f48-185">Ez azt jelzi, hogy elemzés diagnosztika észlelt a hirtelen módosítás kiszűrhetők tulajdonságok kombinációja.</span><span class="sxs-lookup"><span data-stu-id="59f48-185">This indicates that Analytics Diagnostics has identified a combination of properties that filter out the sudden change.</span></span> <span data-ttu-id="59f48-186">Kattintson a további információkhoz juthat a szűrőre, illetve a jelenik meg a szűrt verzió pontra.</span><span class="sxs-lookup"><span data-stu-id="59f48-186">Click the point to get more detail on the filter, and to see the filtered version.</span></span> <span data-ttu-id="59f48-187">Ez lehet, hogy könnyebben azonosíthassa a módosítás okáról.</span><span class="sxs-lookup"><span data-stu-id="59f48-187">This may help you identify what caused the change.</span></span> 

[<span data-ttu-id="59f48-188">További információ az elemzés diagnosztika</span><span class="sxs-lookup"><span data-stu-id="59f48-188">Learn more about Analytics Diagnostics</span></span>](app-insights-analytics-diagnostics.md)


![Elemzés diagnosztika](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-to-dashboard"></a><span data-ttu-id="59f48-190">Rögzítés az irányítópulton</span><span class="sxs-lookup"><span data-stu-id="59f48-190">Pin to dashboard</span></span>
<span data-ttu-id="59f48-191">A diagram vagy a tábla egyik rögzíthető a [irányítópultok megosztott](app-insights-dashboards.md) -kattintson a PIN-kódot.</span><span class="sxs-lookup"><span data-stu-id="59f48-191">You can pin a diagram or table to one of your [shared dashboards](app-insights-dashboards.md) - just click the pin.</span></span> <span data-ttu-id="59f48-192">(Szükség lehet [frissítési az alkalmazás csomagazonosítóját alapú](app-insights-pricing.md) kapcsolja be a szolgáltatást.)</span><span class="sxs-lookup"><span data-stu-id="59f48-192">(You might need to [upgrade your app's pricing package](app-insights-pricing.md) to turn on this feature.)</span></span> 

![Kattintson a PIN-kód](./media/app-insights-analytics-using/pin-01.png)

<span data-ttu-id="59f48-194">Ez azt jelenti, hogy elhelyezett együtt segítenek a teljesítmény vagy a webszolgáltatások használatát az irányítópultot, megadható a más metrikákkal együtt túl összetett elemzést.</span><span class="sxs-lookup"><span data-stu-id="59f48-194">This means that, when you put together a dashboard to help you monitor the performance or usage of your web services, you can include quite complex analysis alongside the other metrics.</span></span> 

<span data-ttu-id="59f48-195">Rögzítheti az irányítópulton, tábla-e azt négy vagy kevesebb oszlopot.</span><span class="sxs-lookup"><span data-stu-id="59f48-195">You can pin a table to the dashboard, if it has four or fewer columns.</span></span> <span data-ttu-id="59f48-196">Csak az első hét sorok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="59f48-196">Only the top seven rows are displayed.</span></span>

### <a name="dashboard-refresh"></a><span data-ttu-id="59f48-197">Irányítópult frissítése</span><span class="sxs-lookup"><span data-stu-id="59f48-197">Dashboard refresh</span></span>
<span data-ttu-id="59f48-198">A diagram az irányítópulton rögzítette által futtatja újból a lekérdezést körülbelül óránként automatikusan frissül.</span><span class="sxs-lookup"><span data-stu-id="59f48-198">The chart pinned to the dashboard is refreshed automatically by re-running the query approximately every hours.</span></span> <span data-ttu-id="59f48-199">A frissítés gombra is kattinthat.</span><span class="sxs-lookup"><span data-stu-id="59f48-199">You can also click the Refresh button.</span></span>

### <a name="automatic-simplifications"></a><span data-ttu-id="59f48-200">Automatikus egyszerűbb</span><span class="sxs-lookup"><span data-stu-id="59f48-200">Automatic simplifications</span></span>

<span data-ttu-id="59f48-201">Bizonyos egyszerűbb diagram is vonatkozik, amikor irányítópulton rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="59f48-201">Certain simplifications are applied to a chart when you pin it to a dashboard.</span></span>

<span data-ttu-id="59f48-202">**Idő korlátozás:** lekérdezések korlátozódnak automatikusan az elmúlt 14 napban.</span><span class="sxs-lookup"><span data-stu-id="59f48-202">**Time restriction:** Queries are automatically limited to the past 14 days.</span></span> <span data-ttu-id="59f48-203">A hatás megegyezik, ha a lekérdezésben `where timestamp > ago(14d)`.</span><span class="sxs-lookup"><span data-stu-id="59f48-203">The effect is the same as if your query includes `where timestamp > ago(14d)`.</span></span>

<span data-ttu-id="59f48-204">**Count korlátozás bin:** nagy mennyiségű diszkrét bins (általában a sávdiagram) rendelkező diagram megjelenítése, ha a kisebb ki van töltve bins automatikusan vannak csoportosítva pedig egyetlen "egyéb" bin.</span><span class="sxs-lookup"><span data-stu-id="59f48-204">**Bin count restriction:** If you display a chart that has a lot of discrete bins (typically a bar chart), the less populated bins are automatically grouped into a single "others" bin.</span></span> <span data-ttu-id="59f48-205">Ha például a lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="59f48-205">For example, this query:</span></span>

    requests | summarize count_search = count() by client_CountryOrRegion

<span data-ttu-id="59f48-206">így néz Analytics:</span><span class="sxs-lookup"><span data-stu-id="59f48-206">looks like this in Analytics:</span></span>

![A diagramon az hosszú utóhívás](./media/app-insights-analytics-using/pin-07.png)

<span data-ttu-id="59f48-208">de ha irányítópulton rögzítheti, így néz:</span><span class="sxs-lookup"><span data-stu-id="59f48-208">but when you pin it to a dashboard, it looks like this:</span></span>

![A diagramon az korlátozott bins](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a><span data-ttu-id="59f48-210">Exportálása az Excelbe</span><span class="sxs-lookup"><span data-stu-id="59f48-210">Export to Excel</span></span>
<span data-ttu-id="59f48-211">A lekérdezés futtatása után letölthet egy CSV-fájlt.</span><span class="sxs-lookup"><span data-stu-id="59f48-211">After you've run a query, you can download a .csv file.</span></span> <span data-ttu-id="59f48-212">Kattintson a **exportálása Excel**.</span><span class="sxs-lookup"><span data-stu-id="59f48-212">Click **Export,  Excel**.</span></span>

## <a name="export-to-power-bi"></a><span data-ttu-id="59f48-213">Power BI-exportálás</span><span class="sxs-lookup"><span data-stu-id="59f48-213">Export to Power BI</span></span>
<span data-ttu-id="59f48-214">A kurzor elhelyezése egy lekérdezésben, és válassza a **exportálásához a Power BI**.</span><span class="sxs-lookup"><span data-stu-id="59f48-214">Put the cursor in a query and choose **Export, Power BI**.</span></span>

![A Power bi-bA Analytics exportálása](./media/app-insights-analytics-using/240.png)

<span data-ttu-id="59f48-216">A lekérdezés futtatása a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="59f48-216">You run the query in Power BI.</span></span> <span data-ttu-id="59f48-217">Beállíthatja úgy, hogy az ütemezés szerint frissítse.</span><span class="sxs-lookup"><span data-stu-id="59f48-217">You can set it to refresh on a schedule.</span></span>

<span data-ttu-id="59f48-218">A Power BI irányítópultjai összefogására olyan adatokat különböző forrásokból hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="59f48-218">With Power BI, you can create dashboards that bring together data from a wide variety of sources.</span></span>

[<span data-ttu-id="59f48-219">További információ a Power bi-ban való exportáláshoz</span><span class="sxs-lookup"><span data-stu-id="59f48-219">Learn more about export to Power BI</span></span>](app-insights-export-power-bi.md)

## <a name="deep-link"></a><span data-ttu-id="59f48-220">Áruházra mutató mélyhivatkozás</span><span class="sxs-lookup"><span data-stu-id="59f48-220">Deep link</span></span>

<span data-ttu-id="59f48-221">Szerezzen be egy hivatkozást a **exportálás megosztás hivatkozás** , amely egy másik felhasználó is küldhet.</span><span class="sxs-lookup"><span data-stu-id="59f48-221">Get a link under **Export, Share link** that you can send to another user.</span></span> <span data-ttu-id="59f48-222">Ha a felhasználó rendelkezik [hozzáférés az erőforráscsoportba](app-insights-resources-roles-access-control.md), a lekérdezés megnyílik az elemzés felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="59f48-222">Provided the user has [access to your resource group](app-insights-resources-roles-access-control.md), the query will open in the Analytics UI.</span></span>

<span data-ttu-id="59f48-223">(A hivatkozás a lekérdezés szövegének után jelenik meg "? q =", gzip tömörített és base-64 kódolású.</span><span class="sxs-lookup"><span data-stu-id="59f48-223">(In the link, the query text appears after "?q=", gzip compressed and base-64 encoded.</span></span> <span data-ttu-id="59f48-224">Kód létrehozásához adja meg a felhasználóknak mélyhivatkozással lehet írni.</span><span class="sxs-lookup"><span data-stu-id="59f48-224">You could write code to generate deep links that you provide to users.</span></span> <span data-ttu-id="59f48-225">Azonban az ajánlott módszer kódból Analytics futtatásához segítségével el a [REST API](https://dev.applicationinsights.io/).)</span><span class="sxs-lookup"><span data-stu-id="59f48-225">However, the recommended way to run Analytics from code is by using the [REST API](https://dev.applicationinsights.io/).)</span></span>


## <a name="automation"></a><span data-ttu-id="59f48-226">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="59f48-226">Automation</span></span>

<span data-ttu-id="59f48-227">Használja a [Data Access REST API](https://dev.applicationinsights.io/) elemzési lekérdezések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="59f48-227">Use the  [Data Access REST API](https://dev.applicationinsights.io/) to run Analytics queries.</span></span> <span data-ttu-id="59f48-228">[Például](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (a PowerShell használatával):</span><span class="sxs-lookup"><span data-stu-id="59f48-228">[For example](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (using PowerShell):</span></span>

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

<span data-ttu-id="59f48-229">Ellentétben a Analytics felhasználói felületén a REST API nem automatikusan hozzáadják a Timestamp típusú korlátozás a lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="59f48-229">Unlike the Analytics UI, the REST API does not automatically add any timestamp limitation to your queries.</span></span> <span data-ttu-id="59f48-230">Ne felejtse el, adja hozzá a saját where záradék, hatalmas válaszok megakadályozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="59f48-230">Remember to add your own where-clause, to avoid getting huge responses.</span></span>



## <a name="import-data"></a><span data-ttu-id="59f48-231">Adatok importálása</span><span class="sxs-lookup"><span data-stu-id="59f48-231">Import data</span></span>

<span data-ttu-id="59f48-232">Adatokat importálhat egy CSV-fájl.</span><span class="sxs-lookup"><span data-stu-id="59f48-232">You can import data from a CSV file.</span></span> <span data-ttu-id="59f48-233">A rendszer egy tipikus használati csatlakozhat a a telemetriai adatokból táblákkal statikus adatok.</span><span class="sxs-lookup"><span data-stu-id="59f48-233">A typical usage is to import static data that you can join with tables from your telemetry.</span></span> 

<span data-ttu-id="59f48-234">Hitelesített felhasználók a telemetriai adatokat egy aliast vagy rejtjelezett azonosító által azonosítható, ha például sikerült egy táblázat, amely leképezhető aliasok valódi nevét importálni.</span><span class="sxs-lookup"><span data-stu-id="59f48-234">For example, if authenticated users are identified in your telemetry by an alias or obfuscated id, you could import a table that maps aliases to real names.</span></span> <span data-ttu-id="59f48-235">A – kéréstelemetria illesztés elvégzésével felhasználók azonosíthatja az Analytics-jelentések a valódi névvel.</span><span class="sxs-lookup"><span data-stu-id="59f48-235">By performing a join on the request telemetry, you can identify users by their real names in the Analytics reports.</span></span>

### <a name="define-your-data-schema"></a><span data-ttu-id="59f48-236">Adja meg a Adatséma</span><span class="sxs-lookup"><span data-stu-id="59f48-236">Define your data schema</span></span>

1. <span data-ttu-id="59f48-237">Kattintson a **beállítások** (a bal felső), majd **adatforrások**.</span><span class="sxs-lookup"><span data-stu-id="59f48-237">Click **Settings** (at top left) and then **Data Sources**.</span></span> 
2. <span data-ttu-id="59f48-238">Adjon hozzá egy adatforrást, a következő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="59f48-238">Add a data source, following the instructions.</span></span> <span data-ttu-id="59f48-239">A rendszer felkéri adja meg az adatokat, amelyek tartalmaznia kell legalább 10 sorok mintáját.</span><span class="sxs-lookup"><span data-stu-id="59f48-239">You are asked to supply a sample of the data, which should include at least ten rows.</span></span> <span data-ttu-id="59f48-240">Javítsa a sémát.</span><span class="sxs-lookup"><span data-stu-id="59f48-240">You then correct the schema.</span></span>

<span data-ttu-id="59f48-241">Ez határozza meg az adatforrás, amelyet a későbbiekben is használhat egyes táblák importálásához.</span><span class="sxs-lookup"><span data-stu-id="59f48-241">This defines a data source, which you can then use to import individual tables.</span></span>

### <a name="import-a-table"></a><span data-ttu-id="59f48-242">A tábla importálása</span><span class="sxs-lookup"><span data-stu-id="59f48-242">Import a table</span></span>

1. <span data-ttu-id="59f48-243">Nyissa meg az adatforrása definíciója a listából.</span><span class="sxs-lookup"><span data-stu-id="59f48-243">Open your data source definition from the list.</span></span>
2. <span data-ttu-id="59f48-244">Kattintson a "Feltöltés lehetőségre", és kövesse az utasításokat a táblázat feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="59f48-244">Click "Upload" and follow the instructions to upload the table.</span></span> <span data-ttu-id="59f48-245">Ebbe beletartozik a REST API hívása, és így könnyen automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="59f48-245">This involves a call to a REST API, and so it is easy to automate.</span></span> 

<span data-ttu-id="59f48-246">A tábla már elemzési lekérdezések használható.</span><span class="sxs-lookup"><span data-stu-id="59f48-246">Your table is now available for use in Analytics queries.</span></span> <span data-ttu-id="59f48-247">Megjelenik az elemzés</span><span class="sxs-lookup"><span data-stu-id="59f48-247">It will appear in Analytics</span></span> 

### <a name="use-the-table"></a><span data-ttu-id="59f48-248">A táblázat</span><span class="sxs-lookup"><span data-stu-id="59f48-248">Use the table</span></span>

<span data-ttu-id="59f48-249">Tegyük fel, az adatforrása definíciója nevezik `usermap`, és arról, hogy rendelkezik-e két mező `realName` és `user_AuthenticatedId`.</span><span class="sxs-lookup"><span data-stu-id="59f48-249">Let's suppose your data source definition is called `usermap`, and that it has two fields, `realName` and `user_AuthenticatedId`.</span></span> <span data-ttu-id="59f48-250">A `requests` is táblához nevű mező `user_AuthenticatedId`, így egyszerűen csatlakoztassa azokat:</span><span class="sxs-lookup"><span data-stu-id="59f48-250">The `requests` table also has a field named `user_AuthenticatedId`, so it's easy to join them:</span></span>

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
<span data-ttu-id="59f48-251">Az eredményül kapott tábla kérelmek rendelkezik egy olyan további oszlop `realName`.</span><span class="sxs-lookup"><span data-stu-id="59f48-251">The resulting table of requests has an additional column, `realName`.</span></span>

### <a name="import-from-logstash"></a><span data-ttu-id="59f48-252">Importálás a LogStash</span><span class="sxs-lookup"><span data-stu-id="59f48-252">Import from LogStash</span></span>

<span data-ttu-id="59f48-253">Ha [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), Analytics segítségével lekérdezni a naplókat.</span><span class="sxs-lookup"><span data-stu-id="59f48-253">If you use [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), you can use Analytics to query your logs.</span></span> <span data-ttu-id="59f48-254">Használja a [beépülő modul, amely adatok csővezeték elemzés be](https://github.com/Microsoft/logstash-output-application-insights).</span><span class="sxs-lookup"><span data-stu-id="59f48-254">Use the [plugin that pipes data into Analytics](https://github.com/Microsoft/logstash-output-application-insights).</span></span> 

## <a name="video"></a><span data-ttu-id="59f48-255">Videó</span><span class="sxs-lookup"><span data-stu-id="59f48-255">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

