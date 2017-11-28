---
title: "Exportálja a Stream Analytics az Azure Application Insights segítségével |} Microsoft Docs"
description: "A Stream Analytics folyamatosan átalakíthatja, szűrésére és az adatok az Application Insights exportálnia útvonalát."
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
ms.openlocfilehash: 6a84d8ff67c420ce712de905ab1172632502a863
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a><span data-ttu-id="ae3cd-103">Az Application Insights exportált adatok feldolgozása a Stream Analytics segítségével</span><span class="sxs-lookup"><span data-stu-id="ae3cd-103">Use Stream Analytics to process exported data from Application Insights</span></span>
<span data-ttu-id="ae3cd-104">[Az Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) adatok feldolgozására ideális eszköz [Application Insights-ból exportált](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-104">[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) is the ideal tool for processing data [exported from Application Insights](app-insights-export-telemetry.md).</span></span> <span data-ttu-id="ae3cd-105">A Stream Analytics segítségével olvasnak be adatokat különböző forrásokból.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-105">Stream Analytics can pull data from a variety of sources.</span></span> <span data-ttu-id="ae3cd-106">Az átalakítási és szűrje az adatokat és mosdók számos irányítja.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-106">It can transform and filter the data, and then route it to a variety of sinks.</span></span>

<span data-ttu-id="ae3cd-107">Ebben a példában mi létrehozunk egy adaptert, amely beolvassa az Application Insights, átnevezése és feldolgozza az egyes mezőit és kiszolgálókészletéhez azt a Power BI-bA.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-107">In this example, we'll create an adaptor that takes data from Application Insights, renames and processes some of the fields, and pipes it into Power BI.</span></span>

> [!WARNING]
> <span data-ttu-id="ae3cd-108">Nincsenek sokkal hatékonyabb és könnyebben [javasolt módját Application Insights adatainak megjelenítése Power BI-ban](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-108">There are much better and easier [recommended ways to display Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="ae3cd-109">A bemutatott elérési út csak egy példa az exportált adatok feldolgozása mutatja be.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-109">The path illustrated here is just an example to illustrate how to process exported data.</span></span>
> 
> 

![Az exportált keresztül SA PBI blokk diagramja](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a><span data-ttu-id="ae3cd-111">Tároló létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ae3cd-111">Create storage in Azure</span></span>
<span data-ttu-id="ae3cd-112">A folyamatos exportálás mindig kimenete adatokat egy Azure Storage-fiók, ezért meg kell először hozza létre a tárolót.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-112">Continuous export always outputs data to an Azure Storage account, so you need to create the storage first.</span></span>

1. <span data-ttu-id="ae3cd-113">Az előfizetésben a "klasszikus" storage-fiók létrehozása a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-113">Create a "classic" storage account in your subscription in the [Azure portal](https://portal.azure.com).</span></span>
   
   ![Azure-portálon válassza ki az új, az adatok, a tárolás](./media/app-insights-export-stream-analytics/030.png)
2. <span data-ttu-id="ae3cd-115">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae3cd-115">Create a container</span></span>
   
    ![Az új tárterületen található tárolók kiválasztása, kattintson a tárolók csempe, majd a Hozzáadás](./media/app-insights-export-stream-analytics/040.png)
3. <span data-ttu-id="ae3cd-117">A tárelérési kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="ae3cd-117">Copy the storage access key</span></span>
   
    <span data-ttu-id="ae3cd-118">Kell, hogy hamarosan állítsa be a stream analytics szolgáltatás bemenete.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-118">You'll need it soon to set up the input to the stream analytics service.</span></span>
   
    ![A tárolóban nyissa meg a beállításokat, a kulcsokat, és az elsődleges elérési kulcsot másolatot](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a><span data-ttu-id="ae3cd-120">Indítsa el az Azure storage a folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="ae3cd-120">Start continuous export to Azure storage</span></span>
<span data-ttu-id="ae3cd-121">[A folyamatos exportálás](app-insights-export-telemetry.md) helyezi át az adatokat az Application Insights az Azure storage-be.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-121">[Continuous export](app-insights-export-telemetry.md) moves data from Application Insights into Azure storage.</span></span>

1. <span data-ttu-id="ae3cd-122">Az Azure portálon tallózással keresse meg az Application Insights-erőforrást az alkalmazáshoz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-122">In the Azure portal, browse to the Application Insights resource you created for your application.</span></span>
   
    ![A Tallózás, az Application Insights az alkalmazáshoz](./media/app-insights-export-stream-analytics/050.png)
2. <span data-ttu-id="ae3cd-124">A folyamatos exportálás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-124">Create a continuous export.</span></span>
   
    ![Válassza a beállítások, folyamatos exportálási hozzáadása](./media/app-insights-export-stream-analytics/060.png)

    <span data-ttu-id="ae3cd-126">Válassza ki a korábban létrehozott tárfiókot:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-126">Select the storage account you created earlier:</span></span>

    ![Exportálás céljának beállítása](./media/app-insights-export-stream-analytics/070.png)

    <span data-ttu-id="ae3cd-128">Állítsa be a megjeleníteni kívánt típusait:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-128">Set the event types you want to see:</span></span>

    ![Válassza ki az esemény típusa](./media/app-insights-export-stream-analytics/080.png)

1. <span data-ttu-id="ae3cd-130">Néhány adat gyűlik össze legyen.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-130">Let some data accumulate.</span></span> <span data-ttu-id="ae3cd-131">Elhelyezkedik vissza, és a felhasználók használhassa az alkalmazást egy ideig.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-131">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="ae3cd-132">Telemetria határozza meg, és látni fogja, statisztikai diagramok [metrika explorer](app-insights-metrics-explorer.md) és az egyes események [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-132">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="ae3cd-133">És is, exportálja az adatokat a tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-133">And also, the data will export to your storage.</span></span> 
2. <span data-ttu-id="ae3cd-134">Vizsgálja meg az exportált adatokat.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-134">Inspect the exported data.</span></span> <span data-ttu-id="ae3cd-135">A Visual Studio felületén válassza **megtekintése / Cloud Explorer**, és nyissa meg az Azure / tárolási.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-135">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="ae3cd-136">(Ha még nem rendelkezik a menüpont, szeretné-e az Azure SDK telepítése: az új projekt párbeszédpanel megnyitásához, és nyissa meg a Visual C# / felhő / Microsoft Azure SDK beolvasása a .NET-hez.)</span><span class="sxs-lookup"><span data-stu-id="ae3cd-136">(If you don't have this menu option, you need to install the Azure SDK: Open the New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    <span data-ttu-id="ae3cd-137">Jegyezze fel az elérési út, az alkalmazás nevét és instrumentation kulcsból származtatott közös részét.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-137">Make a note of the common part of the path name, which is derived from the application name and instrumentation key.</span></span> 

<span data-ttu-id="ae3cd-138">Az események a blob-JSON formátumú fájlok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-138">The events are written to blob files in JSON format.</span></span> <span data-ttu-id="ae3cd-139">Minden fájl tartalmazhat egy vagy több esemény.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-139">Each file may contain one or more events.</span></span> <span data-ttu-id="ae3cd-140">Ezért szeretnénk az esemény-adatok olvasása, és azt szeretnénk, ha a mezők szűrik.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-140">So we'd like to read the event data and filter out the fields we want.</span></span> <span data-ttu-id="ae3cd-141">Nincsenek a különböző dolgok, azt megteheti az adatokat, de a terv ma Stream Analytics segítségével átadhatja az adatokat a Power bi-bA.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-141">There are all kinds of things we could do with the data, but our plan today is to use Stream Analytics to pipe the data to Power BI.</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="ae3cd-142">Hozzon létre egy Azure Stream Analytics-példányt</span><span class="sxs-lookup"><span data-stu-id="ae3cd-142">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="ae3cd-143">Az a [klasszikus Azure portálon](https://manage.windowsazure.com/), válassza ki az Azure Stream Analytics szolgáltatás, és hozzon létre egy új Stream Analytics-feladatot:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-143">From the [Classic Azure Portal](https://manage.windowsazure.com/), select the Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

<span data-ttu-id="ae3cd-144">Ha az új feladat jön létre, bontsa ki a hozzá tartozó részletek:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-144">When the new job is created, expand its details:</span></span>

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a><span data-ttu-id="ae3cd-145">A blob hely beállítása</span><span class="sxs-lookup"><span data-stu-id="ae3cd-145">Set blob location</span></span>
<span data-ttu-id="ae3cd-146">Állítsa be úgy, hogy a folyamatos exportálás blobból bemeneti:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-146">Set it to take input from your Continuous Export blob:</span></span>

![](./media/app-insights-export-stream-analytics/120.png)

<span data-ttu-id="ae3cd-147">Most szüksége lesz az elsődleges elérési kulcsot importáljon a Tárfiókba, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-147">Now you'll need the Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="ae3cd-148">Állítsa be ezt a Tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-148">Set this as the Storage Account Key.</span></span>

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a><span data-ttu-id="ae3cd-149">Set elérési út előtag mintája</span><span class="sxs-lookup"><span data-stu-id="ae3cd-149">Set path prefix pattern</span></span>
![](./media/app-insights-export-stream-analytics/140.png)

<span data-ttu-id="ae3cd-150">**Győződjön meg arról, hogy beállítása a Date formátum éééé-hh-nn-(kötőjel).**</span><span class="sxs-lookup"><span data-stu-id="ae3cd-150">**Be sure to set the Date Format to YYYY-MM-DD (with dashes).**</span></span>

<span data-ttu-id="ae3cd-151">Az elérési út előtag mintája határozza meg, ahol a Stream Analytics talál a bemeneti fájlok a tároló.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-151">The Path Prefix Pattern specifies where Stream Analytics finds the input files in the storage.</span></span> <span data-ttu-id="ae3cd-152">Állítsa be úgy, hogy hogyan tárolja az adatokat folyamatos exportálni kell.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-152">You need to set it to correspond to how Continuous Export stores the data.</span></span> <span data-ttu-id="ae3cd-153">Állítsa be ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-153">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="ae3cd-154">Ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-154">In this example:</span></span>

* <span data-ttu-id="ae3cd-155">`webapplication27`az Application Insights-erőforrás neve **összes kisbetű**.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-155">`webapplication27` is the name of the Application Insights resource **all lower case**.</span></span>
* <span data-ttu-id="ae3cd-156">`1234...`az Application Insights-erőforrások instrumentation kulcsa **kötőjelek kihagyásával**.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-156">`1234...` is the instrumentation key of the Application Insights resource, **omitting dashes**.</span></span> 
* <span data-ttu-id="ae3cd-157">`PageViews`az elemezni kívánt adatok típusát.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-157">`PageViews` is the type of data you want to analyze.</span></span> <span data-ttu-id="ae3cd-158">A használható típusok a folyamatos exportálás beállítása szűrő függ.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-158">The available types depend on the filter you set in Continuous Export.</span></span> <span data-ttu-id="ae3cd-159">Vizsgálja meg az exportált adatok megtekintéséhez a használható típusok, és tekintse meg a [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-159">Examine the exported data to see the other available types, and see the [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="ae3cd-160">`/{date}/{time}`a minta írt szó.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-160">`/{date}/{time}` is a pattern written literally.</span></span>

> [!NOTE]
> <span data-ttu-id="ae3cd-161">Vizsgálja meg a tárolás ellenőrizze, hogy az elérési út jobb beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-161">Inspect the storage to make sure you get the path right.</span></span>
> 
> 

### <a name="finish-initial-setup"></a><span data-ttu-id="ae3cd-162">Kezdeti telepítés befejezése</span><span class="sxs-lookup"><span data-stu-id="ae3cd-162">Finish initial setup</span></span>
<span data-ttu-id="ae3cd-163">Erősítse meg a szerializálási formátum:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-163">Confirm the serialization format:</span></span>

![Erősítse meg, és zárja be a varázsló](./media/app-insights-export-stream-analytics/150.png)

<span data-ttu-id="ae3cd-165">Zárja be a varázslót, és várja meg, a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-165">Close the wizard and wait for the setup to complete.</span></span>

> [!TIP]
> <span data-ttu-id="ae3cd-166">A minta paranccsal bizonyos adatok letöltése.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-166">Use the Sample command to download some data.</span></span> <span data-ttu-id="ae3cd-167">Legyen a lekérdezés hibakeresési teszt mintaként.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-167">Keep it as a test sample to debug your query.</span></span>
> 
> 

## <a name="set-the-output"></a><span data-ttu-id="ae3cd-168">A kimeneti beállítása</span><span class="sxs-lookup"><span data-stu-id="ae3cd-168">Set the output</span></span>
<span data-ttu-id="ae3cd-169">Most jelölje ki a feladatot, és állítsa be a kimenetet.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-169">Now select your job and set the output.</span></span>

![Jelölje ki az új csatornát, kattintson a kimenetek, a Hozzáadás, a Power bi-ban](./media/app-insights-export-stream-analytics/160.png)

<span data-ttu-id="ae3cd-171">Adja meg a **munkahelyi vagy iskolai fiók** engedélyezése a Stream Analytics a Power BI erőforrás elérésére.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-171">Provide your **work or school account** to authorize Stream Analytics to access your Power BI resource.</span></span> <span data-ttu-id="ae3cd-172">Majd találjon ki a kimenetet, és a cél a Power BI DataSet adatkészlet és a tábla nevét.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-172">Then invent a name for the output, and for the target Power BI dataset and table.</span></span>

![A neveket készlet](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-the-query"></a><span data-ttu-id="ae3cd-174">A lekérdezés beállítása</span><span class="sxs-lookup"><span data-stu-id="ae3cd-174">Set the query</span></span>
<span data-ttu-id="ae3cd-175">A lekérdezés fordítása bemeneti, kimeneti szabályozza.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-175">The query governs the translation from input to output.</span></span>

![Jelölje ki a feladatot, és kattintson a lekérdezést.](./media/app-insights-export-stream-analytics/180.png)

<span data-ttu-id="ae3cd-178">A teszt funkció segítségével ellenőrizze, hogy a megfelelő kimeneti kap.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-178">Use the Test function to check that you get the right output.</span></span> <span data-ttu-id="ae3cd-179">A mintaadatok, amely a bemeneti adatok oldalról lejegyezte adjon neki.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-179">Give it the sample data that you took from the inputs page.</span></span> 

### <a name="query-to-display-counts-of-events"></a><span data-ttu-id="ae3cd-180">Megjelenítendő lekérdezések események száma</span><span class="sxs-lookup"><span data-stu-id="ae3cd-180">Query to display counts of events</span></span>
<span data-ttu-id="ae3cd-181">Illessze be a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="ae3cd-181">Paste this query:</span></span>

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

* <span data-ttu-id="ae3cd-182">export-bemeneti érték a jelenleg megadott, a bemeneti adatfolyam alias</span><span class="sxs-lookup"><span data-stu-id="ae3cd-182">export-input is the alias we gave to the stream input</span></span>
* <span data-ttu-id="ae3cd-183">o-pbi a kimeneti alias meghatározott</span><span class="sxs-lookup"><span data-stu-id="ae3cd-183">pbi-output is the output alias we defined</span></span>
* <span data-ttu-id="ae3cd-184">Használjuk [külső alkalmazása GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) mert egy beágyazott JSON arrray az esemény nevét.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-184">We use [OUTER APPLY GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) because the event name is in a nested JSON arrray.</span></span> <span data-ttu-id="ae3cd-185">A Select választja, majd az esemény-névvel, az adott időszakban ilyen nevű példányok számát.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-185">Then the Select picks the event name, together with a count of the number of instances with that name in the time period.</span></span> <span data-ttu-id="ae3cd-186">A [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) záradék csoportosítja az elemek 1 perces időszakokra.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-186">The [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) clause groups the elements into time periods of 1 minute.</span></span>

### <a name="query-to-display-metric-values"></a><span data-ttu-id="ae3cd-187">Lekérdezés metrika értékek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="ae3cd-187">Query to display metric values</span></span>
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

* <span data-ttu-id="ae3cd-188">Ez a lekérdezés eseményrögzítési azokat a metrikák telemetriai adat az esemény időpontja és a Átjárómetrika értékeként.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-188">This query drills into the metrics telemetry to get the event time and the metric value.</span></span> <span data-ttu-id="ae3cd-189">A metrika értékei egy tömb belül, a külső alkalmazása GetElements mintát használjuk a sor kibontásához.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-189">The metric values are inside an array, so we use the OUTER APPLY GetElements pattern to extract the rows.</span></span> <span data-ttu-id="ae3cd-190">"myMetric" Ebben az esetben a mérőszám a neve.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-190">"myMetric" is the name of the metric in this case.</span></span> 

### <a name="query-to-include-values-of-dimension-properties"></a><span data-ttu-id="ae3cd-191">Lekérdezés dimenzió a tulajdonságokat tartalmazza</span><span class="sxs-lookup"><span data-stu-id="ae3cd-191">Query to include values of dimension properties</span></span>
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

* <span data-ttu-id="ae3cd-192">Ez a lekérdezés a dimenzió tulajdonságok nélkül attól függően, hogy egy adott dimenzió alatt a dimenzió tömb rögzített indexnél értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-192">This query includes values of the dimension properties without depending on a particular dimension being at a fixed index in the dimension array.</span></span>

## <a name="run-the-job"></a><span data-ttu-id="ae3cd-193">A feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="ae3cd-193">Run the job</span></span>
<span data-ttu-id="ae3cd-194">A múltban elindítani a feladatot, kiválaszthatja a dátumot.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-194">You can select a date in the past to start the job from.</span></span> 

![Jelölje ki a feladatot, és kattintson a lekérdezést.](./media/app-insights-export-stream-analytics/190.png)

<span data-ttu-id="ae3cd-197">Várjon, amíg a feladat fut-e.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-197">Wait until the job is Running.</span></span>

## <a name="see-results-in-power-bi"></a><span data-ttu-id="ae3cd-198">A Power BI eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="ae3cd-198">See results in Power BI</span></span>
> [!WARNING]
> <span data-ttu-id="ae3cd-199">Nincsenek sokkal hatékonyabb és könnyebben [javasolt módját Application Insights adatainak megjelenítése Power BI-ban](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-199">There are much better and easier [recommended ways to display Application Insights data in Power BI](app-insights-export-power-bi.md).</span></span> <span data-ttu-id="ae3cd-200">A bemutatott elérési út csak egy példa az exportált adatok feldolgozása mutatja be.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-200">The path illustrated here is just an example to illustrate how to process exported data.</span></span>
> 
> 

<span data-ttu-id="ae3cd-201">Nyissa meg a Power BI a munkahelyi vagy iskolai fiókkal, majd válassza ki a DataSet adatkészlet és a tábla, amelyet a Stream Analytics-feladat eredményének.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-201">Open Power BI with your work or school account, and select the dataset and table that you defined as the output of the Stream Analytics job.</span></span>

![A Power bi-ban válassza ki a DataSet adatkészlet és a mezőket.](./media/app-insights-export-stream-analytics/200.png)

<span data-ttu-id="ae3cd-203">Most már használhat ez az adatkészlet a jelentések és irányítópultok a [Power BI](https://powerbi.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ae3cd-203">Now you can use this dataset in reports and dashboards in [Power BI](https://powerbi.microsoft.com).</span></span>

![A Power bi-ban válassza ki a DataSet adatkészlet és a mezőket.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a><span data-ttu-id="ae3cd-205">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="ae3cd-205">No data?</span></span>
* <span data-ttu-id="ae3cd-206">Ellenőrizze, hogy [a dátum formátum beállítása](#set-path-prefix-pattern) megfelelően éééé-hh-nn (a kötőjeleket) számára.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-206">Check that you [set the date format](#set-path-prefix-pattern) correctly to YYYY-MM-DD (with dashes).</span></span>

## <a name="video"></a><span data-ttu-id="ae3cd-207">Videó</span><span class="sxs-lookup"><span data-stu-id="ae3cd-207">Video</span></span>
<span data-ttu-id="ae3cd-208">Noam Ben Zeev bemutatja, hogyan használja a Stream Analytics exportált adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-208">Noam Ben Zeev shows how to process exported data using Stream Analytics.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ae3cd-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae3cd-209">Next steps</span></span>
* [<span data-ttu-id="ae3cd-210">Folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="ae3cd-210">Continuous export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="ae3cd-211">A részletes adatok modell útmutató a tulajdonság típusát és értékét.</span><span class="sxs-lookup"><span data-stu-id="ae3cd-211">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="ae3cd-212">Application Insights</span><span class="sxs-lookup"><span data-stu-id="ae3cd-212">Application Insights</span></span>](app-insights-overview.md)

