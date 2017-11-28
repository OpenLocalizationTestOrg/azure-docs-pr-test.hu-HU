---
title: az Application Insights a BI aaaExport tooPower |} Microsoft Docs
description: "A Power BI elemzési lekérdezések olvasható."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 6668cd7f4e0fbf41695972617f5f8ec207356659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="feed-power-bi-from-application-insights"></a><span data-ttu-id="7ca4d-103">Az Application Insights hírcsatorna a Power bi-ban</span><span class="sxs-lookup"><span data-stu-id="7ca4d-103">Feed Power BI from Application Insights</span></span>
<span data-ttu-id="7ca4d-104">[A Power BI](http://www.powerbi.com/) üzleti analytics eszközöket tartalmazza, amelyek segítséget nyújtanak a csomagok elemzéséhez és insights megosztani.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-104">[Power BI](http://www.powerbi.com/) is a suite of business analytics tools that help you analyze data and share insights.</span></span> <span data-ttu-id="7ca4d-105">Gazdag az irányítópultok olyan rendelkezésre álljanak minden eszközön.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-105">Rich dashboards are available on every device.</span></span> <span data-ttu-id="7ca4d-106">Számos más forrásból, beleértve az elemzési lekérdezések adatok kombinálhatja [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7ca4d-106">You can combine data from many sources, including Analytics queries from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="7ca4d-107">Az Application Insights adatok tooPower BI exportáló három javasolt módszer áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-107">There are three recommended methods of exporting Application Insights data tooPower BI.</span></span> <span data-ttu-id="7ca4d-108">Használhatja őket külön-külön vagy együtt.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-108">You can use them separately or together.</span></span>

* <span data-ttu-id="7ca4d-109">[**A Power BI adapter** ](#power-pi-adapter) -állítson be egy teljes irányítópult telemetriai adatot az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-109">[**Power BI adapter**](#power-pi-adapter) - set up a complete dashboard of telemetry from your app.</span></span> <span data-ttu-id="7ca4d-110">előre definiált diagramok hello készletét, de más forrásból is hozzáadhat a saját lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-110">hello set of charts is predefined, but you can add your own queries from any other sources.</span></span>
* <span data-ttu-id="7ca4d-111">[**Elemzési lekérdezések exportálásáról** ](#export-analytics-queries) -bármely írás Analytics segítségével, és exportálni onnan tooPower BI lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-111">[**Export Analytics queries**](#export-analytics-queries) - write any query you want using Analytics, and export it tooPower BI.</span></span> <span data-ttu-id="7ca4d-112">Ez a lekérdezés elhelyezheti az egyéb adatokat irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-112">You can place this query on a dashboard along with any other data.</span></span>
* <span data-ttu-id="7ca4d-113">[**A folyamatos exportálás és a Stream Analytics** ](app-insights-export-stream-analytics.md) -Ez magában foglalja a további munkahelyi tooset fel.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-113">[**Continuous export and Stream Analytics**](app-insights-export-stream-analytics.md) - This involves more work tooset up.</span></span> <span data-ttu-id="7ca4d-114">Ez akkor hasznos, ha azt szeretné, tookeep az adatokat hosszú ideig.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-114">It is useful if you want tookeep your data for long periods.</span></span> <span data-ttu-id="7ca4d-115">Ellenkező esetben hello többi módszer használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-115">Otherwise, hello other methods are recommended.</span></span>

## <a name="power-bi-adapter"></a><span data-ttu-id="7ca4d-116">A Power BI-adapter</span><span class="sxs-lookup"><span data-stu-id="7ca4d-116">Power BI adapter</span></span>
<span data-ttu-id="7ca4d-117">Ez a módszer létrehoz egy teljes irányítópult telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-117">This method creates a complete dashboard of telemetry for you.</span></span> <span data-ttu-id="7ca4d-118">hello kezdeti adatkészlet előre definiált, de további adatok tooit is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-118">hello initial data set is predefined, but you can add more data tooit.</span></span>

### <a name="get-hello-adapter"></a><span data-ttu-id="7ca4d-119">Hello adapter beolvasása</span><span class="sxs-lookup"><span data-stu-id="7ca4d-119">Get hello adapter</span></span>
1. <span data-ttu-id="7ca4d-120">Jelentkezzen be a túl[Power BI](https://app.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="7ca4d-120">Sign in too[Power BI](https://app.powerbi.com/).</span></span>
2. <span data-ttu-id="7ca4d-121">Nyissa meg **adatok**, **szolgáltatások**, **Application insights szolgáltatással**</span><span class="sxs-lookup"><span data-stu-id="7ca4d-121">Open **Get Data**, **Services**, **Application Insights**</span></span>
   
    ![Az Application Insights adatforrás beszerzése](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. <span data-ttu-id="7ca4d-123">Adja meg az Application Insights-erőforrás hello adatait.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-123">Provide hello details of your Application Insights resource.</span></span>
   
    ![Az Application Insights adatforrás beszerzése](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. <span data-ttu-id="7ca4d-125">Várjon egy percet, amíg hello adatok toobe importálva.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-125">Wait a minute or two for hello data toobe imported.</span></span>
   
    ![A Power BI-adapter](./media/app-insights-export-power-bi/010.png)

<span data-ttu-id="7ca4d-127">Hello irányítópult hello Application Insights diagramok kombinálásának más forrásokból, valamint elemzési lekérdezések szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-127">You can edit hello dashboard, combining hello Application Insights charts with those of other sources, and with Analytics queries.</span></span> <span data-ttu-id="7ca4d-128">Nincs a képi megjelenítés gyűjteménye, ahol kaphat további diagramokat, és minden egyes diagram egy beállítható paraméterrel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-128">There's a visualization gallery where you can get more charts, and each chart has a parameters you can set.</span></span>

<span data-ttu-id="7ca4d-129">Hello kezdeti importálása, hello irányítópult és jelentések hello után továbbra is tooupdate naponta.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-129">After hello initial import, hello dashboard and hello reports continue tooupdate daily.</span></span> <span data-ttu-id="7ca4d-130">Azt is szabályozhatja hello frissítési ütemezés hello adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-130">You can control hello refresh schedule on hello dataset.</span></span>

## <a name="export-analytics-queries"></a><span data-ttu-id="7ca4d-131">Elemzési lekérdezések exportálása</span><span class="sxs-lookup"><span data-stu-id="7ca4d-131">Export Analytics queries</span></span>
<span data-ttu-id="7ca4d-132">Ez az útvonal toowrite lehetővé teszi bármely Analytics lekérdezési Önnek, majd exportálja a adott tooa Power BI-irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-132">This route allows you toowrite any Analytics query you like, and then export that tooa Power BI dashboard.</span></span> <span data-ttu-id="7ca4d-133">(Hello csatoló által létrehozott toohello irányítópult is hozzáadhat.)</span><span class="sxs-lookup"><span data-stu-id="7ca4d-133">(You can add toohello dashboard created by hello adapter.)</span></span>

### <a name="one-time-install-power-bi-desktop"></a><span data-ttu-id="7ca4d-134">Egy alkalommal: telepítse a Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="7ca4d-134">One time: install Power BI Desktop</span></span>
<span data-ttu-id="7ca4d-135">tooimport az Application Insights lekérdezés hello a Power BI asztali verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-135">tooimport your Application Insights query, you use hello desktop version of Power BI.</span></span> <span data-ttu-id="7ca4d-136">De majd közzéteheti azt toohello web vagy tooyour Power BI felhő munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-136">But then you can publish it toohello web or tooyour Power BI cloud workspace.</span></span> 

<span data-ttu-id="7ca4d-137">Telepítés [a Power BI Desktopban](https://powerbi.microsoft.com/en-us/desktop/).</span><span class="sxs-lookup"><span data-stu-id="7ca4d-137">Install [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).</span></span>

### <a name="export-an-analytics-query"></a><span data-ttu-id="7ca4d-138">Az elemzési lekérdezések exportálása</span><span class="sxs-lookup"><span data-stu-id="7ca4d-138">Export an Analytics query</span></span>
1. <span data-ttu-id="7ca4d-139">[Nyissa meg a elemzés és a lekérdezés írása](app-insights-analytics-tour.md).</span><span class="sxs-lookup"><span data-stu-id="7ca4d-139">[Open Analytics and write your query](app-insights-analytics-tour.md).</span></span>
2. <span data-ttu-id="7ca4d-140">Tesztelje, pontosítsa a hello lekérdezés, amíg hello eredmények megfelelőnek találja.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-140">Test and refine hello query until you're happy with hello results.</span></span>

   <span data-ttu-id="7ca4d-141">**Győződjön meg arról, hogy hello lekérdezés futtatása megfelelően Analytics előtt exportálni kell.**</span><span class="sxs-lookup"><span data-stu-id="7ca4d-141">**Make sure that hello query runs correctly in Analytics before you export it.**</span></span>
3. <span data-ttu-id="7ca4d-142">A hello **exportálása** menüben válasszon **Power BI (M)**.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-142">On hello **Export** menu, choose **Power BI (M)**.</span></span> <span data-ttu-id="7ca4d-143">Hello szöveges fájlt mentse.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-143">Save hello text file.</span></span>
   
    ![A Power BI lekérdezés exportálása](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. <span data-ttu-id="7ca4d-145">A Power BI Desktop select **adatok beolvasása, üres lekérdezés** és majd hello a lekérdezés-szerkesztő a **nézet** válasszon **speciális lekérdezés-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-145">In Power BI Desktop select **Get Data, Blank Query** and then in hello query editor, under **View** select **Advanced Query Editor**.</span></span>

    <span data-ttu-id="7ca4d-146">Beillesztés exportált hello M nyelvi parancsfájlt hello speciális lekérdezés-szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-146">Paste hello exported M Language script into hello Advanced Query Editor.</span></span>

    ![Speciális lekérdezés-szerkesztő](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. <span data-ttu-id="7ca4d-148">Lehetséges, hogy tooprovide hitelesítő adatok tooallow Power BI tooaccess Azure.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-148">You might have tooprovide credentials tooallow Power BI tooaccess Azure.</span></span> <span data-ttu-id="7ca4d-149">Használja a "szervezeti fiók" toosign be a Microsoft-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-149">Use 'organizational account' toosign in with your Microsoft account.</span></span>
   
    ![Adja meg Azure hitelesítő adataival tooenable Power BI toorun az Application Insights-lekérdezés](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    <span data-ttu-id="7ca4d-151">(Ha tooverify hello hitelesítő adatok szükségesek, paranccsal hello adatforrás-beállítások menü a Lekérdezésszerkesztő hello.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-151">(If you need tooverify hello credentials, use hello Data Source Settings menu command in hello Query Editor.</span></span> <span data-ttu-id="7ca4d-152">Gondossággal járjanak el a toospecify hello hitelesítő adatokkal kell az Azure-ba, elképzelhető, hogy a hitelesítő adatait a Power BI eltér.)</span><span class="sxs-lookup"><span data-stu-id="7ca4d-152">Take care toospecify hello credentials you use for Azure, which might be different from your credentials for Power BI.)</span></span>
2. <span data-ttu-id="7ca4d-153">Válassza ki a lekérdezést a képi megjelenítés, és válassza ki azokat az x tengely, y tengely és szegmentálja a dimenzió hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-153">Choose a visualization for your query and select hello fields for x-axis, y-axis, and segmenting dimension.</span></span>
   
    ![Válassza ki a képi megjelenítés](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. <span data-ttu-id="7ca4d-155">A jelentés tooyour Power BI felhő munkaterület közzététele.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-155">Publish your report tooyour Power BI cloud workspace.</span></span> <span data-ttu-id="7ca4d-156">Ott egy szinkronizált verziót is beágyazása más weblapokat.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-156">From there, you can embed a synchronized version into other web pages.</span></span>
   
    ![Válassza ki a képi megjelenítés](./media/app-insights-export-power-bi/publish-power-bi.png)
4. <span data-ttu-id="7ca4d-158">Frissítse manuálisan a hello jelentés időközönként, vagy állítson be egy ütemezett frissítés hello-beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-158">Refresh hello report manually at intervals, or set up a scheduled refresh on hello options page.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7ca4d-159">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="7ca4d-159">Troubleshooting</span></span>

### <a name="401-or-403-unauthorized"></a><span data-ttu-id="7ca4d-160">401-es vagy 403-as nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="7ca4d-160">401 or 403 Unauthorized</span></span> 
<span data-ttu-id="7ca4d-161">Ez akkor fordulhat elő, ha a refesh token nem lett frissítve.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-161">This can happen if your refesh token has not been updated.</span></span> <span data-ttu-id="7ca4d-162">Próbálja meg a lépéseket tooensure továbbra is hozzáfér.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-162">Try these steps tooensure you still have access.</span></span> <span data-ttu-id="7ca4d-163">Ha Ön rendelkezik hozzáféréssel, és refershing hello hitelesítő adatok nem működik, nyisson egy támogatási jegy.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-163">If you do have access and refershing hello credentials does not work, please open a support ticket.</span></span>

1. <span data-ttu-id="7ca4d-164">Jelentkezzen be hello Azure portálon, és győződjön meg arról, hogy van-e hozzáférési hello erőforrás</span><span class="sxs-lookup"><span data-stu-id="7ca4d-164">Log into hello Azure Portal and make sure you can access hello resource</span></span>
2. <span data-ttu-id="7ca4d-165">Próbálja meg hello irányítópult toorefresh hello hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="7ca4d-165">Try toorefresh hello credentials for hello Dashboard</span></span>

### <a name="502-bad-gateway"></a><span data-ttu-id="7ca4d-166">502 Hibás átjáró</span><span class="sxs-lookup"><span data-stu-id="7ca4d-166">502 Bad Gateway</span></span>
<span data-ttu-id="7ca4d-167">Ezt általában az Analytics-lekérdezés túl sok adat visszaadó okozza.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-167">This is usually caused by an Analytics query that returns too much data.</span></span> <span data-ttu-id="7ca4d-168">Meg kell egy kisebb időtartományt használatával vagy hello segítségével [ezelőtt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) vagy [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) funkciót csak [projekt](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello szükséges mezők.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-168">You should try using a smaller time range or by using hello [ago](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) or [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) functions only [project](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello fields you need.</span></span>

<span data-ttu-id="7ca4d-169">Ha hello Analytics lekérdezési érkező hello dataset csökkentése nem felel meg a követelményeknek érdemes hello [API](https://dev.applicationinsights.io/documentation/overview) toopull nagyobb adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-169">If reducing hello dataset coming from hello Analytics query doesn't meet your requirements you should consider using hello [API](https://dev.applicationinsights.io/documentation/overview) toopull a larger dataset.</span></span> <span data-ttu-id="7ca4d-170">Az alábbiakban a hogyan tooconvert hello M-lekérdezés exportálja toouse hello API utasításokat.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-170">Here are instructions on how tooconvert hello M-Query export toouse hello API.</span></span>

1. <span data-ttu-id="7ca4d-171">Hozzon létre egy [API-kulcs](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span><span class="sxs-lookup"><span data-stu-id="7ca4d-171">Create an [API Key](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)</span></span>
2. <span data-ttu-id="7ca4d-172">Frissítés hello Power BI M parancsfájl tartományvezérlőkkel történő lecserélésével Analytics exportált hello ARM URL-cím AI API-hoz (lásd az alábbi példa)</span><span class="sxs-lookup"><span data-stu-id="7ca4d-172">Update hello Power BI M script that you exported from Analytics by replacing hello ARM URL with AI API (see example below)</span></span>
   * <span data-ttu-id="7ca4d-173">Cserélje le **https://management.azure.com/subscriptions/...**</span><span class="sxs-lookup"><span data-stu-id="7ca4d-173">Replace **https://management.azure.com/subscriptions/...**</span></span>
   * <span data-ttu-id="7ca4d-174">a **https://api.applicationinsights.io/beta/apps/...**</span><span class="sxs-lookup"><span data-stu-id="7ca4d-174">with, **https://api.applicationinsights.io/beta/apps/...**</span></span>
3. <span data-ttu-id="7ca4d-175">Végül frissítse a hitelesítő adatok toobasic, és az API-kulcs használata</span><span class="sxs-lookup"><span data-stu-id="7ca4d-175">Finally, update credentials toobasic, and use your API Key</span></span>
  

<span data-ttu-id="7ca4d-176">**Meglévő parancsfájl**</span><span class="sxs-lookup"><span data-stu-id="7ca4d-176">**Existing Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
<span data-ttu-id="7ca4d-177">**Frissített parancsfájl**</span><span class="sxs-lookup"><span data-stu-id="7ca4d-177">**Updated Script**</span></span>
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a><span data-ttu-id="7ca4d-178">Mintavételi kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="7ca4d-178">About sampling</span></span>
<span data-ttu-id="7ca4d-179">Az alkalmazás nagy mennyiségű adatot küld, ha hello adaptív mintavételi szolgáltatás működik, és csak százalékaként a telemetriai adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-179">If your application sends a lot of data, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> <span data-ttu-id="7ca4d-180">hello is igaz, ha manuálisan mintavételi hello SDK vagy adatfeldolgozást.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-180">hello same is true if you have manually set sampling either in hello SDK or on ingestion.</span></span> [<span data-ttu-id="7ca4d-181">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="7ca4d-181">Learn more about sampling.</span></span>](app-insights-sampling.md)


## <a name="next-steps"></a><span data-ttu-id="7ca4d-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ca4d-182">Next steps</span></span>
* [<span data-ttu-id="7ca4d-183">A Power BI - információ</span><span class="sxs-lookup"><span data-stu-id="7ca4d-183">Power BI - Learn</span></span>](http://www.powerbi.com/learning/)
* [<span data-ttu-id="7ca4d-184">Elemzés oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="7ca4d-184">Analytics tutorial</span></span>](app-insights-analytics-tour.md)

