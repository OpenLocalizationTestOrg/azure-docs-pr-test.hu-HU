---
title: "az Azure Application Insights Stream Analytics segítségével aaaExport |} Microsoft Docs"
description: "A Stream Analytics folyamatosan alakíthatja át, és a útvonal hello adatok exportálása az Application Insights."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: fda9b64f588c520833b2669eafdf650efc3de6be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-stream-analytics-tooprocess-exported-data-from-application-insights"></a><span data-ttu-id="e9b2c-103">Használja a Stream Analytics tooprocess az exportált adatok az Application Insights</span><span class="sxs-lookup"><span data-stu-id="e9b2c-103">Use Stream Analytics tooprocess exported data from Application Insights</span></span>
<span data-ttu-id="e9b2c-104">[Az Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) hello ideális eszköz adatfeldolgozás [Application Insights-ból exportált](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is hello ideal tool for processing data [exported from Application Insights](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="e9b2c-105">A Stream Analytics segítségével olvasnak be adatokat különböző forrásokból.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-105">Stream Analytics can pull data from a variety of sources.</span></span> <span data-ttu-id="e9b2c-106">Az átalakítási és hello adatok szűrése és tooa különböző nyelő irányításához.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-106">It can transform and filter hello data, and then route it tooa variety of sinks.</span></span>

<span data-ttu-id="e9b2c-107">Ebben a példában mi létrehozunk egy adaptert, amely beolvassa az Application Insights, átnevezése és dolgozza fel néhány hello mező, és kiszolgálókészletéhez azt a Power BI-bA.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-107">In this example, we'll create an adaptor that takes data from Application Insights, renames and processes some of hello fields, and pipes it into Power BI.</span></span>

> [!WARNING]
> <span data-ttu-id="e9b2c-108">Nincsenek sokkal hatékonyabb és könnyebben [javasolt módját toodisplay Application Insights adatokat a Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-108">There are much better and easier [recommended ways toodisplay Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="e9b2c-109">hello bemutatott elérési út csak egy példa tooillustrate hogyan tooprocess exportált adatok.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-109">hello path illustrated here is just an example tooillustrate how tooprocess exported data.</span></span>
> 
> 

![SA tooPBI keresztül exportálás letiltása diagramja](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a><span data-ttu-id="e9b2c-111">Tároló létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e9b2c-111">Create storage in Azure</span></span>
<span data-ttu-id="e9b2c-112">A folyamatos exportálás adatok tooan Azure Storage-fiók esetén mindig kimenete, ezért először a toocreate hello tárolási kell.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-112">Continuous export always outputs data tooan Azure Storage account, so you need toocreate hello storage first.</span></span>

1. <span data-ttu-id="e9b2c-113">Hozzon létre egy "klasszikus" tárfiókot az előfizetésében szereplő hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-113">Create a "classic" storage account in your subscription in hello [Azure portal](https://portal.azure.com).</span></span>
   
   ![Azure-portálon válassza ki az új, az adatok, a tárolás](./media/app-insights-export-stream-analytics/030.png)
2. <span data-ttu-id="e9b2c-115">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e9b2c-115">Create a container</span></span>
   
    ![Az új tárolási hello tárolók kiválasztása, hello tárolók csempére, majd a Hozzáadás gombra](./media/app-insights-export-stream-analytics/040.png)
3. <span data-ttu-id="e9b2c-117">Hello tárelérési kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="e9b2c-117">Copy hello storage access key</span></span>
   
    <span data-ttu-id="e9b2c-118">Szüksége lehet rájuk hamarosan tooset hello bemeneti toohello stream analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-118">You'll need it soon tooset up hello input toohello stream analytics service.</span></span>
   
    ![Hello tárolóban nyissa meg a beállításokat, a kulcsokat, és másolatot hello elsődleges elérési kulcsot](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-tooazure-storage"></a><span data-ttu-id="e9b2c-120">Indítsa el a folyamatos exportálás tooAzure tároló</span><span class="sxs-lookup"><span data-stu-id="e9b2c-120">Start continuous export tooAzure storage</span></span>
<span data-ttu-id="e9b2c-121">[A folyamatos exportálás](app-insights-export-telemetry.md) helyezi át az adatokat az Application Insights az Azure storage-be.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-121">[Continuous export](app-insights-export-telemetry.md) moves data from Application Insights into Azure storage.</span></span>

1. <span data-ttu-id="e9b2c-122">Hello Azure-portálon keresse meg az alkalmazáshoz létrehozott toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-122">In hello Azure portal, browse toohello Application Insights resource you created for your application.</span></span>
   
    ![A Tallózás, az Application Insights az alkalmazáshoz](./media/app-insights-export-stream-analytics/050.png)
2. <span data-ttu-id="e9b2c-124">A folyamatos exportálás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-124">Create a continuous export.</span></span>
   
    ![Válassza a beállítások, folyamatos exportálási hozzáadása](./media/app-insights-export-stream-analytics/060.png)

    <span data-ttu-id="e9b2c-126">Válassza ki a korábban létrehozott hello tárfiókot:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-126">Select hello storage account you created earlier:</span></span>

    ![Hello exportálási cél beállítása](./media/app-insights-export-stream-analytics/070.png)

    <span data-ttu-id="e9b2c-128">Állítsa be a kívánt hello eseménytípusok toosee:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-128">Set hello event types you want toosee:</span></span>

    ![Válassza ki az esemény típusa](./media/app-insights-export-stream-analytics/080.png)

1. <span data-ttu-id="e9b2c-130">Néhány adat gyűlik össze legyen.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-130">Let some data accumulate.</span></span> <span data-ttu-id="e9b2c-131">Elhelyezkedik vissza, és a felhasználók használhassa az alkalmazást egy ideig.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-131">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="e9b2c-132">Telemetria határozza meg, és látni fogja, statisztikai diagramok [metrika explorer](app-insights-metrics-explorer.md) és az egyes események [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-132">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="e9b2c-133">És is, hello adatok tooyour tárolási exportálja.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-133">And also, hello data will export tooyour storage.</span></span> 
2. <span data-ttu-id="e9b2c-134">Vizsgálja meg az exportált hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-134">Inspect hello exported data.</span></span> <span data-ttu-id="e9b2c-135">A Visual Studio felületén válassza **megtekintése / Cloud Explorer**, és nyissa meg az Azure / tárolási.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-135">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="e9b2c-136">(Ha még nem rendelkezik a menüpont, tooinstall hello Azure SDK-t kell: hello új projekt párbeszédpanel megnyitásához, és nyissa meg a Visual C# / felhő / Microsoft Azure SDK beolvasása a .NET-hez.)</span><span class="sxs-lookup"><span data-stu-id="e9b2c-136">(If you don't have this menu option, you need tooinstall hello Azure SDK: Open hello New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    <span data-ttu-id="e9b2c-137">Jegyezze fel a hello közös hello alkalmazás nevét és instrumentation kulcsból származtatott hello elérési út része.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-137">Make a note of hello common part of hello path name, which is derived from hello application name and instrumentation key.</span></span> 

<span data-ttu-id="e9b2c-138">hello események tooblob fájlok írt JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-138">hello events are written tooblob files in JSON format.</span></span> <span data-ttu-id="e9b2c-139">Minden fájl tartalmazhat egy vagy több esemény.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-139">Each file may contain one or more events.</span></span> <span data-ttu-id="e9b2c-140">Ezért szeretnénk tooread hello eseményadatok és szűrő ki szeretnénk hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-140">So we'd like tooread hello event data and filter out hello fields we want.</span></span> <span data-ttu-id="e9b2c-141">Azt sikerült műveletekből hello adatokkal az összes típusú léteznek, de a terv ma toouse Stream Analytics toopipe hello adatok tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-141">There are all kinds of things we could do with hello data, but our plan today is toouse Stream Analytics toopipe hello data tooPower BI.</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="e9b2c-142">Hozzon létre egy Azure Stream Analytics-példányt</span><span class="sxs-lookup"><span data-stu-id="e9b2c-142">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="e9b2c-143">A hello [klasszikus Azure portálon](https://manage.windowsazure.com/), válassza ki a hello Azure Stream Analytics szolgáltatás, és hozzon létre egy új Stream Analytics-feladatot:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-143">From hello [Classic Azure Portal](https://manage.windowsazure.com/), select hello Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

<span data-ttu-id="e9b2c-144">Amikor hello új feladatot hoz létre, bontsa ki a hozzá tartozó részletek:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-144">When hello new job is created, expand its details:</span></span>

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a><span data-ttu-id="e9b2c-145">A blob hely beállítása</span><span class="sxs-lookup"><span data-stu-id="e9b2c-145">Set blob location</span></span>
<span data-ttu-id="e9b2c-146">A folyamatos exportálás blob tootake bemenetének állítsa be:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-146">Set it tootake input from your Continuous Export blob:</span></span>

![](./media/app-insights-export-stream-analytics/120.png)

<span data-ttu-id="e9b2c-147">Most importáljon a Tárfiókba, amelyet korábban feljegyzett hello elsődleges elérési kulcsot lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-147">Now you'll need hello Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="e9b2c-148">Állítsa be a hello Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-148">Set this as hello Storage Account Key.</span></span>

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a><span data-ttu-id="e9b2c-149">Set elérési út előtag mintája</span><span class="sxs-lookup"><span data-stu-id="e9b2c-149">Set path prefix pattern</span></span>
![](./media/app-insights-export-stream-analytics/140.png)

<span data-ttu-id="e9b2c-150">**Lehet, hogy tooset hello dátumformátum tooYYYY-hh-nn (a szaggatott vonal).**</span><span class="sxs-lookup"><span data-stu-id="e9b2c-150">**Be sure tooset hello Date Format tooYYYY-MM-DD (with dashes).**</span></span>

<span data-ttu-id="e9b2c-151">elérési út előtag mintája hello határozza meg, ahol a Stream Analytics hello tárolási hello bemeneti fájlok talál.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-151">hello Path Prefix Pattern specifies where Stream Analytics finds hello input files in hello storage.</span></span> <span data-ttu-id="e9b2c-152">Tooset kell azt a folyamatos exportálás toocorrespond toohow hello adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-152">You need tooset it toocorrespond toohow Continuous Export stores hello data.</span></span> <span data-ttu-id="e9b2c-153">Állítsa be ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-153">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="e9b2c-154">Ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-154">In this example:</span></span>

* <span data-ttu-id="e9b2c-155">`webapplication27`hello Application Insights-erőforrás neve hello **összes kisbetű**.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-155">`webapplication27` is hello name of hello Application Insights resource **all lower case**.</span></span>
* <span data-ttu-id="e9b2c-156">`1234...`az Application Insights-erőforrást, hello hello instrumentation kulcsa **kötőjelek kihagyásával**.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-156">`1234...` is hello instrumentation key of hello Application Insights resource, **omitting dashes**.</span></span> 
* <span data-ttu-id="e9b2c-157">`PageViews`hello típusú adatok tooanalyze szeretné.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-157">`PageViews` is hello type of data you want tooanalyze.</span></span> <span data-ttu-id="e9b2c-158">rendelkezésre álló típusok hello hello szűrő, beállíthatja a folyamatos exportálás függ.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-158">hello available types depend on hello filter you set in Continuous Export.</span></span> <span data-ttu-id="e9b2c-159">Vizsgálja meg a hello exportált adatok toosee hello más elérhető típusok, és tekintse meg a hello [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-159">Examine hello exported data toosee hello other available types, and see hello [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="e9b2c-160">`/{date}/{time}`a minta írt szó.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-160">`/{date}/{time}` is a pattern written literally.</span></span>

> [!NOTE]
> <span data-ttu-id="e9b2c-161">Vizsgálja meg a hello tárolási toomake meg arról, hogy a megfelelő beolvasni hello elérési útját.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-161">Inspect hello storage toomake sure you get hello path right.</span></span>
> 
> 

### <a name="finish-initial-setup"></a><span data-ttu-id="e9b2c-162">Kezdeti telepítés befejezése</span><span class="sxs-lookup"><span data-stu-id="e9b2c-162">Finish initial setup</span></span>
<span data-ttu-id="e9b2c-163">Erősítse meg a hello szerializálási formátum:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-163">Confirm hello serialization format:</span></span>

![Erősítse meg, és zárja be a varázsló](./media/app-insights-export-stream-analytics/150.png)

<span data-ttu-id="e9b2c-165">Hello bezárása, és várjon, amíg a telepítő toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-165">Close hello wizard and wait for hello setup toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="e9b2c-166">Hello minta parancs toodownload bizonyos adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-166">Use hello Sample command toodownload some data.</span></span> <span data-ttu-id="e9b2c-167">Legyen, a teszt minta toodebug a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-167">Keep it as a test sample toodebug your query.</span></span>
> 
> 

## <a name="set-hello-output"></a><span data-ttu-id="e9b2c-168">Set hello kimeneti</span><span class="sxs-lookup"><span data-stu-id="e9b2c-168">Set hello output</span></span>
<span data-ttu-id="e9b2c-169">Most jelölje ki a feladatot, és állítsa be a hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-169">Now select your job and set hello output.</span></span>

![Válassza ki a hello új csatorna, kattintson a kimenetek, a Hozzáadás, a Power bi-ban](./media/app-insights-export-stream-analytics/160.png)

<span data-ttu-id="e9b2c-171">Adja meg a **munkahelyi vagy iskolai fiók** tooauthorize Stream Analytics tooaccess a Power BI-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-171">Provide your **work or school account** tooauthorize Stream Analytics tooaccess your Power BI resource.</span></span> <span data-ttu-id="e9b2c-172">Majd készlet hello kimeneti, és hello cél Power BI DataSet adatkészlet és a tábla nevét.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-172">Then invent a name for hello output, and for hello target Power BI dataset and table.</span></span>

![A neveket készlet](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-hello-query"></a><span data-ttu-id="e9b2c-174">Set hello lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e9b2c-174">Set hello query</span></span>
<span data-ttu-id="e9b2c-175">hello lekérdezés hello fordítási a bemeneti toooutput szabályozza.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-175">hello query governs hello translation from input toooutput.</span></span>

![Jelölje ki a hello feladat, majd kattintson a lekérdezést.](./media/app-insights-export-stream-analytics/180.png)

<span data-ttu-id="e9b2c-178">Hello teszt függvény toocheck, hogy elérhetővé hello jobb oldali kimeneti használja.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-178">Use hello Test function toocheck that you get hello right output.</span></span> <span data-ttu-id="e9b2c-179">Adjon meg hozzá hello bemenetek oldalról lejegyezte hello mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-179">Give it hello sample data that you took from hello inputs page.</span></span> 

### <a name="query-toodisplay-counts-of-events"></a><span data-ttu-id="e9b2c-180">Lekérdezés toodisplay számát is események</span><span class="sxs-lookup"><span data-stu-id="e9b2c-180">Query toodisplay counts of events</span></span>
<span data-ttu-id="e9b2c-181">Illessze be a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="e9b2c-181">Paste this query:</span></span>

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* <span data-ttu-id="e9b2c-182">export-bemeneti érték azt a nevet adott toohello adatfolyam-bemenet hello alias</span><span class="sxs-lookup"><span data-stu-id="e9b2c-182">export-input is hello alias we gave toohello stream input</span></span>
* <span data-ttu-id="e9b2c-183">pbi-kimeneti rendszer hello kimeneti alias meghatározott</span><span class="sxs-lookup"><span data-stu-id="e9b2c-183">pbi-output is hello output alias we defined</span></span>
* <span data-ttu-id="e9b2c-184">Használjuk [külső alkalmazása GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) mert egy beágyazott JSON arrray hello esemény neve.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-184">We use [OUTER APPLY GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) because hello event name is in a nested JSON arrray.</span></span> <span data-ttu-id="e9b2c-185">Majd hello válassza kivételezések hello esemény-névvel, hello több példányban található ilyen nevű hello időszak számát.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-185">Then hello Select picks hello event name, together with a count of hello number of instances with that name in hello time period.</span></span> <span data-ttu-id="e9b2c-186">Hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) záradék hello elemek csoportok 1 perces időszakokra.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-186">hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) clause groups hello elements into time periods of 1 minute.</span></span>

### <a name="query-toodisplay-metric-values"></a><span data-ttu-id="e9b2c-187">Lekérdezés toodisplay metrika értékek</span><span class="sxs-lookup"><span data-stu-id="e9b2c-187">Query toodisplay metric values</span></span>
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* <span data-ttu-id="e9b2c-188">Ez a lekérdezés eseményrögzítési hello metrikák telemetriai tooget hello esemény időpontja és hello Átjárómetrika értékeként.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-188">This query drills into hello metrics telemetry tooget hello event time and hello metric value.</span></span> <span data-ttu-id="e9b2c-189">hello metrika értékei belül tömb, így hello külső alkalmazása GetElements mintát tooextract hello sorok használjuk.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-189">hello metric values are inside an array, so we use hello OUTER APPLY GetElements pattern tooextract hello rows.</span></span> <span data-ttu-id="e9b2c-190">"myMetric" Ebben az esetben az hello metrika hello név.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-190">"myMetric" is hello name of hello metric in this case.</span></span> 

### <a name="query-tooinclude-values-of-dimension-properties"></a><span data-ttu-id="e9b2c-191">Lekérdezés tooinclude értékek dimenzió tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e9b2c-191">Query tooinclude values of dimension properties</span></span>
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* <span data-ttu-id="e9b2c-192">Ez a lekérdezés hello dimenziótulajdonságok nélkül attól függően, hogy egy adott dimenzió hello dimenzió tömb rögzített indexnél alatt az értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-192">This query includes values of hello dimension properties without depending on a particular dimension being at a fixed index in hello dimension array.</span></span>

## <a name="run-hello-job"></a><span data-ttu-id="e9b2c-193">Hello feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="e9b2c-193">Run hello job</span></span>
<span data-ttu-id="e9b2c-194">A múltbeli toostart hello feladatot hello kiválaszthatja a dátumot.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-194">You can select a date in hello past toostart hello job from.</span></span> 

![Jelölje ki a hello feladat, majd kattintson a lekérdezést.](./media/app-insights-export-stream-analytics/190.png)

<span data-ttu-id="e9b2c-197">Várjon, amíg hello feladat fut-e.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-197">Wait until hello job is Running.</span></span>

## <a name="see-results-in-power-bi"></a><span data-ttu-id="e9b2c-198">A Power BI eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="e9b2c-198">See results in Power BI</span></span>
> [!WARNING]
> <span data-ttu-id="e9b2c-199">Nincsenek sokkal hatékonyabb és könnyebben [javasolt módját toodisplay Application Insights adatokat a Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-199">There are much better and easier [recommended ways toodisplay Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="e9b2c-200">hello bemutatott elérési út csak egy példa tooillustrate hogyan tooprocess exportált adatok.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-200">hello path illustrated here is just an example tooillustrate how tooprocess exported data.</span></span>
> 
> 

<span data-ttu-id="e9b2c-201">Nyissa meg a munkahelyi vagy iskolai fiókját, és jelölje be hello dataset és tábla, amelyet hello Stream Analytics-feladat eredményének hello Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-201">Open Power BI with your work or school account, and select hello dataset and table that you defined as hello output of hello Stream Analytics job.</span></span>

![A Power bi-ban válassza ki a DataSet adatkészlet és a mezőket.](./media/app-insights-export-stream-analytics/200.png)

<span data-ttu-id="e9b2c-203">Most már használhat ez az adatkészlet a jelentések és irányítópultok a [Power BI](https://powerbi.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-203">Now you can use this dataset in reports and dashboards in [Power BI](https://powerbi.microsoft.com).</span></span>

![A Power bi-ban válassza ki a DataSet adatkészlet és a mezőket.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a><span data-ttu-id="e9b2c-205">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="e9b2c-205">No data?</span></span>
* <span data-ttu-id="e9b2c-206">Ellenőrizze, hogy [set hello dátumformátum](#set-path-prefix-pattern) megfelelően tooYYYY-hh-nn (a szaggatott vonal).</span><span class="sxs-lookup"><span data-stu-id="e9b2c-206">Check that you [set hello date format](#set-path-prefix-pattern) correctly tooYYYY-MM-DD (with dashes).</span></span>

## <a name="video"></a><span data-ttu-id="e9b2c-207">Videó</span><span class="sxs-lookup"><span data-stu-id="e9b2c-207">Video</span></span>
<span data-ttu-id="e9b2c-208">Noam Ben Zeev bemutatja, hogyan tooprocess exportált adatok használatával a Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-208">Noam Ben Zeev shows how tooprocess exported data using Stream Analytics.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a><span data-ttu-id="e9b2c-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9b2c-209">Next steps</span></span>
* [<span data-ttu-id="e9b2c-210">Folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="e9b2c-210">Continuous export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="e9b2c-211">Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.</span><span class="sxs-lookup"><span data-stu-id="e9b2c-211">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="e9b2c-212">Application Insights</span><span class="sxs-lookup"><span data-stu-id="e9b2c-212">Application Insights</span></span>](app-insights-overview.md)

