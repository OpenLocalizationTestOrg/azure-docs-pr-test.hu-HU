---
title: "Az Azure Application Insights tooSQL exportálása |} Microsoft Docs"
description: "Az Application Insights adatok tooSQL használja a Stream Analytics folyamatosan exportálja."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/06/2015
ms.author: bwren
ms.openlocfilehash: 58b579499113751a088dc7e66cbec71529773322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-export-toosql-from-application-insights-using-stream-analytics"></a><span data-ttu-id="e98c4-103">Forgatókönyv: A Stream Analytics használ az Application Insights tooSQL exportálása</span><span class="sxs-lookup"><span data-stu-id="e98c4-103">Walkthrough: Export tooSQL from Application Insights using Stream Analytics</span></span>
<span data-ttu-id="e98c4-104">Ez a cikk bemutatja, hogyan toomove a telemetriai adatokat a [Azure Application Insights] [ start] használatával az Azure SQL adatbázishoz [a folyamatos exportálás] [ export] és [az Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="e98c4-104">This article shows how toomove your telemetry data from [Azure Application Insights][start] into an Azure SQL database by using [Continuous Export][export] and [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> 

<span data-ttu-id="e98c4-105">A folyamatos exportálás a telemetriai adatok áthelyezése az Azure Storage JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="e98c4-105">Continuous export moves your telemetry data into Azure Storage in JSON format.</span></span> <span data-ttu-id="e98c4-106">Azt fogja hello JSON-objektumok használatával az Azure Stream Analytics elemzése és sorok az adatbázistábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e98c4-106">We'll parse hello JSON objects using Azure Stream Analytics and create rows in a database table.</span></span>

<span data-ttu-id="e98c4-107">(Általában a folyamatos exportálás hello módon toodo saját hello telemetriai adatok elemzését az alkalmazások küldése tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="e98c4-107">(More generally, Continuous Export is hello way toodo your own analysis of hello telemetry your apps send tooApplication Insights.</span></span> <span data-ttu-id="e98c4-108">Akkor lehetett igazítja a kód a minta toodo más exportált hello telemetriai adatok, például az adatok összesítése a műveleteket.)</span><span class="sxs-lookup"><span data-stu-id="e98c4-108">You could adapt this code sample toodo other things with hello exported telemetry, such as aggregation of data.)</span></span>

<span data-ttu-id="e98c4-109">Először foglalkozunk hello azt feltételezi, hogy már rendelkezik hello-alkalmazáshoz toomonitor.</span><span class="sxs-lookup"><span data-stu-id="e98c4-109">We'll start with hello assumption that you already have hello app you want toomonitor.</span></span>

<span data-ttu-id="e98c4-110">Ebben a példában a fogjuk hello Lapmegtekintések adatainak használni, de hello ugyanilyen mintájú egyszerűen bővíthető tooother adattípusok, például egyéni események és a kivételeket.</span><span class="sxs-lookup"><span data-stu-id="e98c4-110">In this example, we will be using hello page view data, but hello same pattern can easily be extended tooother data types such as custom events and exceptions.</span></span> 

## <a name="add-application-insights-tooyour-application"></a><span data-ttu-id="e98c4-111">Az Application Insights tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e98c4-111">Add Application Insights tooyour application</span></span>
<span data-ttu-id="e98c4-112">tooget lépések:</span><span class="sxs-lookup"><span data-stu-id="e98c4-112">tooget started:</span></span>

1. <span data-ttu-id="e98c4-113">[Az Application Insights beállítása a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="e98c4-113">[Set up Application Insights for your web pages](app-insights-javascript.md).</span></span> 
   
    <span data-ttu-id="e98c4-114">(A példában azt összpontosítson Lapmegtekintések adatainak hello ügyfél böngészőkkel történő feldolgozásáról, de a kiszolgálói oldalon hello is beállíthat az Application Insights a [Java](app-insights-java-get-started.md) vagy [ASP.NET](app-insights-asp-net.md) alkalmazás és a folyamat függőség és más kiszolgáló telemetriai adat.)</span><span class="sxs-lookup"><span data-stu-id="e98c4-114">(In this example, we'll focus on processing page view data from hello client browsers, but you could also set up Application Insights for hello server side of your [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md) app and process request, dependency and other server telemetry.)</span></span>
2. <span data-ttu-id="e98c4-115">Tegye közzé az alkalmazást, és tekintse meg a "telemetrikus" adatok jelennek meg az Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e98c4-115">Publish your app, and watch telemetry data appearing in your Application Insights resource.</span></span>

## <a name="create-storage-in-azure"></a><span data-ttu-id="e98c4-116">Tároló létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="e98c4-116">Create storage in Azure</span></span>
<span data-ttu-id="e98c4-117">A folyamatos exportálás adatok tooan Azure Storage-fiók esetén mindig kimenete, ezért először a toocreate hello tárolási kell.</span><span class="sxs-lookup"><span data-stu-id="e98c4-117">Continuous export always outputs data tooan Azure Storage account, so you need toocreate hello storage first.</span></span>

1. <span data-ttu-id="e98c4-118">Hozzon létre egy tárfiókot az előfizetésében szereplő hello [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="e98c4-118">Create a storage account in your subscription in hello [Azure portal][portal].</span></span>
   
    ![Azure-portálon válassza az új, az adatok, a tároló.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. <span data-ttu-id="e98c4-122">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e98c4-122">Create a container</span></span>
   
    ![Az új tárolási hello tárolók kiválasztása, hello tárolók csempére, majd a Hozzáadás gombra](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. <span data-ttu-id="e98c4-124">Hello tárelérési kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="e98c4-124">Copy hello storage access key</span></span>
   
    <span data-ttu-id="e98c4-125">Szüksége lehet rájuk hamarosan tooset hello bemeneti toohello stream analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e98c4-125">You'll need it soon tooset up hello input toohello stream analytics service.</span></span>
   
    ![Hello tárolóban nyissa meg a beállításokat, a kulcsokat, és másolatot hello elsődleges elérési kulcsot](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-tooazure-storage"></a><span data-ttu-id="e98c4-127">Indítsa el a folyamatos exportálás tooAzure tároló</span><span class="sxs-lookup"><span data-stu-id="e98c4-127">Start continuous export tooAzure storage</span></span>
1. <span data-ttu-id="e98c4-128">Hello Azure-portálon keresse meg az alkalmazáshoz létrehozott toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e98c4-128">In hello Azure portal, browse toohello Application Insights resource you created for your application.</span></span>
   
    ![A Tallózás, az Application Insights az alkalmazáshoz](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. <span data-ttu-id="e98c4-130">A folyamatos exportálás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e98c4-130">Create a continuous export.</span></span>
   
    ![Válassza a beállítások, folyamatos exportálási hozzáadása](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    <span data-ttu-id="e98c4-132">Válassza ki a korábban létrehozott hello tárfiókot:</span><span class="sxs-lookup"><span data-stu-id="e98c4-132">Select hello storage account you created earlier:</span></span>

    ![Hello exportálási cél beállítása](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    <span data-ttu-id="e98c4-134">Állítsa be a kívánt hello eseménytípusok toosee:</span><span class="sxs-lookup"><span data-stu-id="e98c4-134">Set hello event types you want toosee:</span></span>

    ![Válassza ki az esemény típusa](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. <span data-ttu-id="e98c4-136">Néhány adat gyűlik össze legyen.</span><span class="sxs-lookup"><span data-stu-id="e98c4-136">Let some data accumulate.</span></span> <span data-ttu-id="e98c4-137">Elhelyezkedik vissza, és a felhasználók használhassa az alkalmazást egy ideig.</span><span class="sxs-lookup"><span data-stu-id="e98c4-137">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="e98c4-138">Telemetria határozza meg, és látni fogja, statisztikai diagramok [metrika explorer](app-insights-metrics-explorer.md) és az egyes események [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="e98c4-138">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="e98c4-139">És is, hello adatok tooyour tárolási exportálja.</span><span class="sxs-lookup"><span data-stu-id="e98c4-139">And also, hello data will export tooyour storage.</span></span> 
2. <span data-ttu-id="e98c4-140">Vizsgálja meg az exportált hello adatok, vagy hello portal - kiválasztása **Tallózás**, válassza ki a tárfiókot, majd **tárolók** - vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e98c4-140">Inspect hello exported data, either in hello portal - choose **Browse**, select your storage account, and then **Containers** - or in Visual Studio.</span></span> <span data-ttu-id="e98c4-141">A Visual Studio felületén válassza **megtekintése / Cloud Explorer**, és nyissa meg az Azure / tárolási.</span><span class="sxs-lookup"><span data-stu-id="e98c4-141">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="e98c4-142">(Ha még nem rendelkezik a menüpont, tooinstall hello Azure SDK-t kell: hello új projekt párbeszédpanel megnyitásához, és nyissa meg a Visual C# / felhő / Microsoft Azure SDK beolvasása a .NET-hez.)</span><span class="sxs-lookup"><span data-stu-id="e98c4-142">(If you don't have this menu option, you need tooinstall hello Azure SDK: Open hello New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![Nyissa meg a Visual Studio Server Browser bővítményt, az Azure, a tároló](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    <span data-ttu-id="e98c4-144">Jegyezze fel a hello közös hello alkalmazás nevét és instrumentation kulcsból származtatott hello elérési út része.</span><span class="sxs-lookup"><span data-stu-id="e98c4-144">Make a note of hello common part of hello path name, which is derived from hello application name and instrumentation key.</span></span> 

<span data-ttu-id="e98c4-145">hello események tooblob fájlok írt JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="e98c4-145">hello events are written tooblob files in JSON format.</span></span> <span data-ttu-id="e98c4-146">Minden fájl tartalmazhat egy vagy több esemény.</span><span class="sxs-lookup"><span data-stu-id="e98c4-146">Each file may contain one or more events.</span></span> <span data-ttu-id="e98c4-147">Ezért szeretnénk tooread hello eseményadatok és szűrő ki szeretnénk hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="e98c4-147">So we'd like tooread hello event data and filter out hello fields we want.</span></span> <span data-ttu-id="e98c4-148">Azt sikerült műveletekből hello adatokkal az összes típusú léteznek, de a terv ma toouse Stream Analytics toomove hello az tooa SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e98c4-148">There are all kinds of things we could do with hello data, but our plan today is toouse Stream Analytics toomove hello data tooa SQL database.</span></span> <span data-ttu-id="e98c4-149">Amely megkönnyítő könnyen toorun nagy mennyiségű érdekes lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="e98c4-149">That will make it easy toorun lots of interesting queries.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="e98c4-150">Egy Azure SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="e98c4-150">Create an Azure SQL Database</span></span>
<span data-ttu-id="e98c4-151">Ismét az előfizetés-től kezdődő [Azure-portálon][portal], hello adatbázis létrehozása (és egy új kiszolgálót, kivéve, ha már van egy) toowhich fog írni hello adatok.</span><span class="sxs-lookup"><span data-stu-id="e98c4-151">Once again starting from your subscription in [Azure portal][portal], create hello database (and a new server, unless you've already got one) toowhich you'll write hello data.</span></span>

![Új adatok, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

<span data-ttu-id="e98c4-153">Győződjön meg arról, hogy hello adatbázis-kiszolgáló hozzáférést tooAzure szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="e98c4-153">Make sure that hello database server allows access tooAzure services:</span></span>

![Tallózással keresse meg, kiszolgálók, a kiszolgáló beállításait, a tűzfal, tooAzure hozzáférés engedélyezése](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a><span data-ttu-id="e98c4-155">Létrehoz egy táblát az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="e98c4-155">Create a table in Azure SQL DB</span></span>
<span data-ttu-id="e98c4-156">Csatlakoztassa az előnyben részesített felügyeleti eszközzel hello előző szakaszban létrehozott toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e98c4-156">Connect toohello database created in hello previous section with your preferred management tool.</span></span> <span data-ttu-id="e98c4-157">Ebben a bemutatóban fogjuk használni [az SQL Server felügyeleti eszközei](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="e98c4-157">In this walkthrough, we will be using [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

<span data-ttu-id="e98c4-158">Hozzon létre egy új lekérdezést, és hajtsa végre a következő T-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="e98c4-158">Create a new query, and execute hello following T-SQL:</span></span>

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

<span data-ttu-id="e98c4-159">Ez a példa Lapmegtekintések adatainak használunk.</span><span class="sxs-lookup"><span data-stu-id="e98c4-159">In this sample, we are using data from page views.</span></span> <span data-ttu-id="e98c4-160">toosee hello található egyéb adatok, vizsgálhatja meg a JSON-kimenetét, és tekintse meg a hello [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="e98c4-160">toosee hello other data available, inspect your JSON output, and see hello [export data model](app-insights-export-data-model.md).</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="e98c4-161">Hozzon létre egy Azure Stream Analytics-példányt</span><span class="sxs-lookup"><span data-stu-id="e98c4-161">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="e98c4-162">A hello [klasszikus Azure portálon](https://manage.windowsazure.com/), válassza ki a hello Azure Stream Analytics szolgáltatás, és hozzon létre egy új Stream Analytics-feladatot:</span><span class="sxs-lookup"><span data-stu-id="e98c4-162">From hello [Classic Azure Portal](https://manage.windowsazure.com/), select hello Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

<span data-ttu-id="e98c4-163">Amikor hello új feladatot hoz létre, bontsa ki a hozzá tartozó részletek:</span><span class="sxs-lookup"><span data-stu-id="e98c4-163">When hello new job is created, expand its details:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a><span data-ttu-id="e98c4-164">A blob hely beállítása</span><span class="sxs-lookup"><span data-stu-id="e98c4-164">Set blob location</span></span>
<span data-ttu-id="e98c4-165">A folyamatos exportálás blob tootake bemenetének állítsa be:</span><span class="sxs-lookup"><span data-stu-id="e98c4-165">Set it tootake input from your Continuous Export blob:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

<span data-ttu-id="e98c4-166">Most importáljon a Tárfiókba, amelyet korábban feljegyzett hello elsődleges elérési kulcsot lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="e98c4-166">Now you'll need hello Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="e98c4-167">Állítsa be a hello Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="e98c4-167">Set this as hello Storage Account Key.</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a><span data-ttu-id="e98c4-168">Set elérési út előtag mintája</span><span class="sxs-lookup"><span data-stu-id="e98c4-168">Set path prefix pattern</span></span>
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

<span data-ttu-id="e98c4-169">Túl lehet, hogy tooset hello dátumformátum**éééé-hh-nn** (a **kötőjelek**).</span><span class="sxs-lookup"><span data-stu-id="e98c4-169">Be sure tooset hello Date Format too**YYYY-MM-DD** (with **dashes**).</span></span>

<span data-ttu-id="e98c4-170">elérési út előtag mintája hello határozza meg, hogyan Stream Analytics keresse meg a bemeneti fájlok hello hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e98c4-170">hello Path Prefix Pattern specifies how Stream Analytics finds hello input files in hello storage.</span></span> <span data-ttu-id="e98c4-171">Tooset kell azt a folyamatos exportálás toocorrespond toohow hello adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="e98c4-171">You need tooset it toocorrespond toohow Continuous Export stores hello data.</span></span> <span data-ttu-id="e98c4-172">Állítsa be ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="e98c4-172">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="e98c4-173">Ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="e98c4-173">In this example:</span></span>

* <span data-ttu-id="e98c4-174">`webapplication27`hello Application Insights-erőforrás neve hello **minden a nagybetűs**.</span><span class="sxs-lookup"><span data-stu-id="e98c4-174">`webapplication27` is hello name of hello Application Insights resource, **all in lower case**.</span></span> 
* <span data-ttu-id="e98c4-175">`1234...`az Application Insights-erőforrás hello hello instrumentation kulcsa **az eltávolított kötőjelek**.</span><span class="sxs-lookup"><span data-stu-id="e98c4-175">`1234...` is hello instrumentation key of hello Application Insights resource **with dashes removed**.</span></span> 
* <span data-ttu-id="e98c4-176">`PageViews`hello típusú adatok tooanalyze szeretnénk.</span><span class="sxs-lookup"><span data-stu-id="e98c4-176">`PageViews` is hello type of data we want tooanalyze.</span></span> <span data-ttu-id="e98c4-177">rendelkezésre álló típusok hello hello szűrő, beállíthatja a folyamatos exportálás függ.</span><span class="sxs-lookup"><span data-stu-id="e98c4-177">hello available types depend on hello filter you set in Continuous Export.</span></span> <span data-ttu-id="e98c4-178">Vizsgálja meg a hello exportált adatok toosee hello más elérhető típusok, és tekintse meg a hello [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="e98c4-178">Examine hello exported data toosee hello other available types, and see hello [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="e98c4-179">`/{date}/{time}`a minta írt szó.</span><span class="sxs-lookup"><span data-stu-id="e98c4-179">`/{date}/{time}` is a pattern written literally.</span></span>

<span data-ttu-id="e98c4-180">tooget hello nevét és az Application Insights-erőforrás iKey Essentials az Áttekintés lap megnyitásához, vagy nyissa meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="e98c4-180">tooget hello name and iKey of your Application Insights resource, open Essentials on its overview page, or open Settings.</span></span>

#### <a name="finish-initial-setup"></a><span data-ttu-id="e98c4-181">Kezdeti telepítés befejezése</span><span class="sxs-lookup"><span data-stu-id="e98c4-181">Finish initial setup</span></span>
<span data-ttu-id="e98c4-182">Erősítse meg a hello szerializálási formátum:</span><span class="sxs-lookup"><span data-stu-id="e98c4-182">Confirm hello serialization format:</span></span>

![Erősítse meg, és zárja be a varázsló](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

<span data-ttu-id="e98c4-184">Hello bezárása, és várjon, amíg a telepítő toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="e98c4-184">Close hello wizard and wait for hello setup toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="e98c4-185">Hello minta függvény toocheck, hogy helyesen van beállítva hello bemeneti elérési utat használja.</span><span class="sxs-lookup"><span data-stu-id="e98c4-185">Use hello Sample function toocheck that you have set hello input path correctly.</span></span> <span data-ttu-id="e98c4-186">Ha a hiba: Ellenőrizze, hogy van-e adatok hello tárolási hello úgy döntött, hogy a minta az időtartomány.</span><span class="sxs-lookup"><span data-stu-id="e98c4-186">If it fails: Check that there is data in hello storage for hello sample time range you chose.</span></span> <span data-ttu-id="e98c4-187">Szerkessze a hello bemeneti definícióját, és ellenőrizze a hello tárfiókot, elérési út előtag beállítása és a dátum helyesen formátumban.</span><span class="sxs-lookup"><span data-stu-id="e98c4-187">Edit hello input definition and check you set hello storage account, path prefix and date format correctly.</span></span>
> 
> 

## <a name="set-query"></a><span data-ttu-id="e98c4-188">Set-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e98c4-188">Set query</span></span>
<span data-ttu-id="e98c4-189">Nyissa meg a hello lekérdezésszakaszt:</span><span class="sxs-lookup"><span data-stu-id="e98c4-189">Open hello query section:</span></span>

![A stream analytics válassza ki a lekérdezés](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

<span data-ttu-id="e98c4-191">Cserélje le a hello alapértelmezett lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="e98c4-191">Replace hello default query with:</span></span>

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

<span data-ttu-id="e98c4-192">Figyelje meg, hogy először hello néhány tulajdonságainak adott toopage adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e98c4-192">Notice that hello first few properties are specific toopage view data.</span></span> <span data-ttu-id="e98c4-193">Más telemetriai típusú kivitel más tulajdonságokkal fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="e98c4-193">Exports of other telemetry types will have different properties.</span></span> <span data-ttu-id="e98c4-194">Lásd: hello [részletes adatok modell hivatkozást hello Tulajdonságtípusok és értékeket.](app-insights-export-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="e98c4-194">See hello [detailed data model reference for hello property types and values.](app-insights-export-data-model.md)</span></span>

## <a name="set-up-output-toodatabase"></a><span data-ttu-id="e98c4-195">Kimeneti toodatabase beállítása</span><span class="sxs-lookup"><span data-stu-id="e98c4-195">Set up output toodatabase</span></span>
<span data-ttu-id="e98c4-196">Válassza ki az SQL hello output típusúként.</span><span class="sxs-lookup"><span data-stu-id="e98c4-196">Select SQL as hello output.</span></span>

![A stream analytics válassza ki a kimenetek](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

<span data-ttu-id="e98c4-198">Adja meg a hello SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e98c4-198">Specify hello SQL database.</span></span>

![Adja meg az adatbázis hello részletei](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

<span data-ttu-id="e98c4-200">Hello bezárása, és várja meg, egy értesítés, hogy hello kimeneti be van állítva.</span><span class="sxs-lookup"><span data-stu-id="e98c4-200">Close hello wizard and wait for a notification that hello output has been set up.</span></span>

## <a name="start-processing"></a><span data-ttu-id="e98c4-201">Indítsa el a feldolgozási</span><span class="sxs-lookup"><span data-stu-id="e98c4-201">Start processing</span></span>
<span data-ttu-id="e98c4-202">Hello feladat indításának hello műveletsávon:</span><span class="sxs-lookup"><span data-stu-id="e98c4-202">Start hello job from hello action bar:</span></span>

![A stream analytics kattintson a Start](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

<span data-ttu-id="e98c4-204">Kiválaszthatja, hogy toostart feldolgozási hello most vagy a korábbi adatok toostart kiindulva adatokat.</span><span class="sxs-lookup"><span data-stu-id="e98c4-204">You can choose whether toostart processing hello data starting from now, or toostart with earlier data.</span></span> <span data-ttu-id="e98c4-205">Ez utóbbi hello akkor hasznos, ha korábban a folyamatos exportálás már fut egy ideig.</span><span class="sxs-lookup"><span data-stu-id="e98c4-205">hello latter is useful if you have had Continuous Export already running for a while.</span></span>

![A stream analytics kattintson a Start](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

<span data-ttu-id="e98c4-207">Néhány perc elteltével tooSQL kiszolgálófelügyeleti visszaléphet, és tekintse meg a áramló adatok hello.</span><span class="sxs-lookup"><span data-stu-id="e98c4-207">After a few minutes, go back tooSQL Server Management Tools and watch hello data flowing in.</span></span> <span data-ttu-id="e98c4-208">Például lekérdezéssel ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="e98c4-208">For example, use a query like this:</span></span>

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a><span data-ttu-id="e98c4-209">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="e98c4-209">Related articles</span></span>
* [<span data-ttu-id="e98c4-210">Használja a Stream Analytics tooPowerBI exportálása</span><span class="sxs-lookup"><span data-stu-id="e98c4-210">Export tooPowerBI using Stream Analytics</span></span>](app-insights-export-power-bi.md)
* [<span data-ttu-id="e98c4-211">Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.</span><span class="sxs-lookup"><span data-stu-id="e98c4-211">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="e98c4-212">A folyamatos Exportálás az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="e98c4-212">Continuous Export in Application Insights</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="e98c4-213">Application Insights</span><span class="sxs-lookup"><span data-stu-id="e98c4-213">Application Insights</span></span>](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

