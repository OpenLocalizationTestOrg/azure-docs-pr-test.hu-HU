---
title: "Az Azure Application Insights SQL exportálása |} Microsoft Docs"
description: "Application Insights adatainak folyamatos exportálása Stream Analytics használ SQL."
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
ms.openlocfilehash: d51e80509ffb63cef0d01133a2295d58757d5b1a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a><span data-ttu-id="a646c-103">Forgatókönyv: Az Application Insights Stream Analytics használ SQL exportálása</span><span class="sxs-lookup"><span data-stu-id="a646c-103">Walkthrough: Export to SQL from Application Insights using Stream Analytics</span></span>
<span data-ttu-id="a646c-104">Ez a cikk bemutatja, hogyan kívánja áthelyezni a telemetriai adatokat a [Azure Application Insights] [ start] használatával az Azure SQL adatbázishoz [a folyamatos exportálás] [ export] és [az Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="a646c-104">This article shows how to move your telemetry data from [Azure Application Insights][start] into an Azure SQL database by using [Continuous Export][export] and [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> 

<span data-ttu-id="a646c-105">A folyamatos exportálás a telemetriai adatok áthelyezése az Azure Storage JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="a646c-105">Continuous export moves your telemetry data into Azure Storage in JSON format.</span></span> <span data-ttu-id="a646c-106">Azt fogja elemezni a JSON-objektumok használatával az Azure Stream Analytics és sorok az adatbázistábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a646c-106">We'll parse the JSON objects using Azure Stream Analytics and create rows in a database table.</span></span>

<span data-ttu-id="a646c-107">(Általában a folyamatos exportálás ennek a módja a saját Application Insights küldeni az alkalmazások telemetriai adatok elemzését.</span><span class="sxs-lookup"><span data-stu-id="a646c-107">(More generally, Continuous Export is the way to do your own analysis of the telemetry your apps send to Application Insights.</span></span> <span data-ttu-id="a646c-108">Akkor lehetett igazítja más műveleteket az exportált telemetriai adatok, például az adatok összesítő kódmintában.)</span><span class="sxs-lookup"><span data-stu-id="a646c-108">You could adapt this code sample to do other things with the exported telemetry, such as aggregation of data.)</span></span>

<span data-ttu-id="a646c-109">Először foglalkozunk azt feltételezi, hogy már rendelkezik a figyelni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a646c-109">We'll start with the assumption that you already have the app you want to monitor.</span></span>

<span data-ttu-id="a646c-110">Ebben a példában a fogjuk a lapmegtekintések adatainak használni, de ugyanilyen mintájú könnyen más adattípusok, például az egyéni események és kivételek kell terjeszteni.</span><span class="sxs-lookup"><span data-stu-id="a646c-110">In this example, we will be using the page view data, but the same pattern can easily be extended to other data types such as custom events and exceptions.</span></span> 

## <a name="add-application-insights-to-your-application"></a><span data-ttu-id="a646c-111">Az Application Insights hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a646c-111">Add Application Insights to your application</span></span>
<span data-ttu-id="a646c-112">Első lépések:</span><span class="sxs-lookup"><span data-stu-id="a646c-112">To get started:</span></span>

1. <span data-ttu-id="a646c-113">[Az Application Insights beállítása a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="a646c-113">[Set up Application Insights for your web pages](app-insights-javascript.md).</span></span> 
   
    <span data-ttu-id="a646c-114">(A példában azt összpontosítson az ügyfélböngészők a lapmegtekintések adatainak feldolgozási, de a kiszolgáló oldalán is beállíthat az Application Insights a [Java](app-insights-java-get-started.md) vagy [ASP.NET](app-insights-asp-net.md) alkalmazás és a folyamat kérelem függőségi és más kiszolgáló telemetriai adat.)</span><span class="sxs-lookup"><span data-stu-id="a646c-114">(In this example, we'll focus on processing page view data from the client browsers, but you could also set up Application Insights for the server side of your [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md) app and process request, dependency and other server telemetry.)</span></span>
2. <span data-ttu-id="a646c-115">Tegye közzé az alkalmazást, és tekintse meg a "telemetrikus" adatok jelennek meg az Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a646c-115">Publish your app, and watch telemetry data appearing in your Application Insights resource.</span></span>

## <a name="create-storage-in-azure"></a><span data-ttu-id="a646c-116">Tároló létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a646c-116">Create storage in Azure</span></span>
<span data-ttu-id="a646c-117">A folyamatos exportálás mindig kimenete adatokat egy Azure Storage-fiók, ezért meg kell először hozza létre a tárolót.</span><span class="sxs-lookup"><span data-stu-id="a646c-117">Continuous export always outputs data to an Azure Storage account, so you need to create the storage first.</span></span>

1. <span data-ttu-id="a646c-118">Az előfizetésben a storage-fiók létrehozása a [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="a646c-118">Create a storage account in your subscription in the [Azure portal][portal].</span></span>
   
    ![Azure-portálon válassza az új, az adatok, a tároló.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. <span data-ttu-id="a646c-122">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a646c-122">Create a container</span></span>
   
    ![Az új tárterületen található tárolók kiválasztása, kattintson a tárolók csempe, majd a Hozzáadás](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. <span data-ttu-id="a646c-124">A tárelérési kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="a646c-124">Copy the storage access key</span></span>
   
    <span data-ttu-id="a646c-125">Kell, hogy hamarosan állítsa be a stream analytics szolgáltatás bemenete.</span><span class="sxs-lookup"><span data-stu-id="a646c-125">You'll need it soon to set up the input to the stream analytics service.</span></span>
   
    ![A tárolóban nyissa meg a beállításokat, a kulcsokat, és az elsődleges elérési kulcsot másolatot](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a><span data-ttu-id="a646c-127">Indítsa el az Azure storage a folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="a646c-127">Start continuous export to Azure storage</span></span>
1. <span data-ttu-id="a646c-128">Az Azure portálon tallózással keresse meg az Application Insights-erőforrást az alkalmazáshoz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a646c-128">In the Azure portal, browse to the Application Insights resource you created for your application.</span></span>
   
    ![A Tallózás, az Application Insights az alkalmazáshoz](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. <span data-ttu-id="a646c-130">A folyamatos exportálás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a646c-130">Create a continuous export.</span></span>
   
    ![Válassza a beállítások, folyamatos exportálási hozzáadása](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    <span data-ttu-id="a646c-132">Válassza ki a korábban létrehozott tárfiókot:</span><span class="sxs-lookup"><span data-stu-id="a646c-132">Select the storage account you created earlier:</span></span>

    ![Exportálás céljának beállítása](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    <span data-ttu-id="a646c-134">Állítsa be a megjeleníteni kívánt típusait:</span><span class="sxs-lookup"><span data-stu-id="a646c-134">Set the event types you want to see:</span></span>

    ![Válassza ki az esemény típusa](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. <span data-ttu-id="a646c-136">Néhány adat gyűlik össze legyen.</span><span class="sxs-lookup"><span data-stu-id="a646c-136">Let some data accumulate.</span></span> <span data-ttu-id="a646c-137">Elhelyezkedik vissza, és a felhasználók használhassa az alkalmazást egy ideig.</span><span class="sxs-lookup"><span data-stu-id="a646c-137">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="a646c-138">Telemetria határozza meg, és látni fogja, statisztikai diagramok [metrika explorer](app-insights-metrics-explorer.md) és az egyes események [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="a646c-138">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="a646c-139">És is, exportálja az adatokat a tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="a646c-139">And also, the data will export to your storage.</span></span> 
2. <span data-ttu-id="a646c-140">Vizsgálja meg az exportált adatok, vagy a portál - kiválasztása **Tallózás**, válassza ki a tárfiókot, majd **tárolók** - vagy a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="a646c-140">Inspect the exported data, either in the portal - choose **Browse**, select your storage account, and then **Containers** - or in Visual Studio.</span></span> <span data-ttu-id="a646c-141">A Visual Studio felületén válassza **megtekintése / Cloud Explorer**, és nyissa meg az Azure / tárolási.</span><span class="sxs-lookup"><span data-stu-id="a646c-141">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="a646c-142">(Ha még nem rendelkezik a menüpont, szeretné-e az Azure SDK telepítése: az új projekt párbeszédpanel megnyitásához, és nyissa meg a Visual C# / felhő / Microsoft Azure SDK beolvasása a .NET-hez.)</span><span class="sxs-lookup"><span data-stu-id="a646c-142">(If you don't have this menu option, you need to install the Azure SDK: Open the New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![Nyissa meg a Visual Studio Server Browser bővítményt, az Azure, a tároló](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    <span data-ttu-id="a646c-144">Jegyezze fel az elérési út, az alkalmazás nevét és instrumentation kulcsból származtatott közös részét.</span><span class="sxs-lookup"><span data-stu-id="a646c-144">Make a note of the common part of the path name, which is derived from the application name and instrumentation key.</span></span> 

<span data-ttu-id="a646c-145">Az események a blob-JSON formátumú fájlok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="a646c-145">The events are written to blob files in JSON format.</span></span> <span data-ttu-id="a646c-146">Minden fájl tartalmazhat egy vagy több esemény.</span><span class="sxs-lookup"><span data-stu-id="a646c-146">Each file may contain one or more events.</span></span> <span data-ttu-id="a646c-147">Ezért szeretnénk az esemény-adatok olvasása, és azt szeretnénk, ha a mezők szűrik.</span><span class="sxs-lookup"><span data-stu-id="a646c-147">So we'd like to read the event data and filter out the fields we want.</span></span> <span data-ttu-id="a646c-148">Nincsenek a különböző dolgok, azt megteheti az adatokat, de a terv ma Stream Analytics segítségével az adatok áthelyezése az SQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="a646c-148">There are all kinds of things we could do with the data, but our plan today is to use Stream Analytics to move the data to a SQL database.</span></span> <span data-ttu-id="a646c-149">Amely könnyen nagy mennyiségű érdekes lekérdezések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a646c-149">That will make it easy to run lots of interesting queries.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="a646c-150">Egy Azure SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="a646c-150">Create an Azure SQL Database</span></span>
<span data-ttu-id="a646c-151">Még egyszer az előfizetés-től kezdődő [Azure-portálon][portal], hozza létre az adatbázist (és az új kiszolgáló kivéve, ha már van egy) amely meg fog írni.</span><span class="sxs-lookup"><span data-stu-id="a646c-151">Once again starting from your subscription in [Azure portal][portal], create the database (and a new server, unless you've already got one) to which you'll write the data.</span></span>

![Új adatok, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

<span data-ttu-id="a646c-153">Győződjön meg arról, hogy az adatbázis-kiszolgáló Azure-szolgáltatásokhoz való hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="a646c-153">Make sure that the database server allows access to Azure services:</span></span>

![Tallózással keresse meg, kiszolgálók, a kiszolgáló beállításait, a tűzfal, a hozzáférés engedélyezése az Azure-bA](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a><span data-ttu-id="a646c-155">Létrehoz egy táblát az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="a646c-155">Create a table in Azure SQL DB</span></span>
<span data-ttu-id="a646c-156">Csatlakozás az előnyben részesített felügyeleti eszközzel az előző szakaszban létrehozott adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="a646c-156">Connect to the database created in the previous section with your preferred management tool.</span></span> <span data-ttu-id="a646c-157">Ebben a bemutatóban fogjuk használni [az SQL Server felügyeleti eszközei](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="a646c-157">In this walkthrough, we will be using [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

<span data-ttu-id="a646c-158">Hozzon létre egy új lekérdezést, és hajtsa végre a következő T-SQL:</span><span class="sxs-lookup"><span data-stu-id="a646c-158">Create a new query, and execute the following T-SQL:</span></span>

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

<span data-ttu-id="a646c-159">Ez a példa Lapmegtekintések adatainak használunk.</span><span class="sxs-lookup"><span data-stu-id="a646c-159">In this sample, we are using data from page views.</span></span> <span data-ttu-id="a646c-160">A rendelkezésre álló adatok megtekintéséhez vizsgálhatja meg a JSON-kimenetét, és tekintse meg a [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="a646c-160">To see the other data available, inspect your JSON output, and see the [export data model](app-insights-export-data-model.md).</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="a646c-161">Hozzon létre egy Azure Stream Analytics-példányt</span><span class="sxs-lookup"><span data-stu-id="a646c-161">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="a646c-162">Az a [klasszikus Azure portálon](https://manage.windowsazure.com/), válassza ki az Azure Stream Analytics szolgáltatás, és hozzon létre egy új Stream Analytics-feladatot:</span><span class="sxs-lookup"><span data-stu-id="a646c-162">From the [Classic Azure Portal](https://manage.windowsazure.com/), select the Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

<span data-ttu-id="a646c-163">Ha az új feladat jön létre, bontsa ki a hozzá tartozó részletek:</span><span class="sxs-lookup"><span data-stu-id="a646c-163">When the new job is created, expand its details:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a><span data-ttu-id="a646c-164">A blob hely beállítása</span><span class="sxs-lookup"><span data-stu-id="a646c-164">Set blob location</span></span>
<span data-ttu-id="a646c-165">Állítsa be úgy, hogy a folyamatos exportálás blobból bemeneti:</span><span class="sxs-lookup"><span data-stu-id="a646c-165">Set it to take input from your Continuous Export blob:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

<span data-ttu-id="a646c-166">Most szüksége lesz az elsődleges elérési kulcsot importáljon a Tárfiókba, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="a646c-166">Now you'll need the Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="a646c-167">Állítsa be ezt a Tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="a646c-167">Set this as the Storage Account Key.</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a><span data-ttu-id="a646c-168">Set elérési út előtag mintája</span><span class="sxs-lookup"><span data-stu-id="a646c-168">Set path prefix pattern</span></span>
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

<span data-ttu-id="a646c-169">Ügyeljen arra, hogy a dátum formátum beállítása **éééé-hh-nn** (a **kötőjelek**).</span><span class="sxs-lookup"><span data-stu-id="a646c-169">Be sure to set the Date Format to **YYYY-MM-DD** (with **dashes**).</span></span>

<span data-ttu-id="a646c-170">Az elérési út előtag mintája határozza meg, hogyan Stream Analytics keresse meg a bemeneti fájlokat a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a646c-170">The Path Prefix Pattern specifies how Stream Analytics finds the input files in the storage.</span></span> <span data-ttu-id="a646c-171">Állítsa be úgy, hogy hogyan tárolja az adatokat folyamatos exportálni kell.</span><span class="sxs-lookup"><span data-stu-id="a646c-171">You need to set it to correspond to how Continuous Export stores the data.</span></span> <span data-ttu-id="a646c-172">Állítsa be ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="a646c-172">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="a646c-173">Ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="a646c-173">In this example:</span></span>

* <span data-ttu-id="a646c-174">`webapplication27`Application Insights-erőforrások, neve **minden a nagybetűs**.</span><span class="sxs-lookup"><span data-stu-id="a646c-174">`webapplication27` is the name of the Application Insights resource, **all in lower case**.</span></span> 
* <span data-ttu-id="a646c-175">`1234...`az Application Insights-erőforrások instrumentation kulcsa **az eltávolított kötőjelek**.</span><span class="sxs-lookup"><span data-stu-id="a646c-175">`1234...` is the instrumentation key of the Application Insights resource **with dashes removed**.</span></span> 
* <span data-ttu-id="a646c-176">`PageViews`az adatok elemzéséhez szeretnénk típusa van.</span><span class="sxs-lookup"><span data-stu-id="a646c-176">`PageViews` is the type of data we want to analyze.</span></span> <span data-ttu-id="a646c-177">A használható típusok a folyamatos exportálás beállítása szűrő függ.</span><span class="sxs-lookup"><span data-stu-id="a646c-177">The available types depend on the filter you set in Continuous Export.</span></span> <span data-ttu-id="a646c-178">Vizsgálja meg az exportált adatok megtekintéséhez a használható típusok, és tekintse meg a [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="a646c-178">Examine the exported data to see the other available types, and see the [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="a646c-179">`/{date}/{time}`a minta írt szó.</span><span class="sxs-lookup"><span data-stu-id="a646c-179">`/{date}/{time}` is a pattern written literally.</span></span>

<span data-ttu-id="a646c-180">Ahhoz, hogy a név és az Application Insights-erőforrás iKey, nyissa meg az Essentials az Áttekintés lap, vagy nyissa meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a646c-180">To get the name and iKey of your Application Insights resource, open Essentials on its overview page, or open Settings.</span></span>

#### <a name="finish-initial-setup"></a><span data-ttu-id="a646c-181">Kezdeti telepítés befejezése</span><span class="sxs-lookup"><span data-stu-id="a646c-181">Finish initial setup</span></span>
<span data-ttu-id="a646c-182">Erősítse meg a szerializálási formátum:</span><span class="sxs-lookup"><span data-stu-id="a646c-182">Confirm the serialization format:</span></span>

![Erősítse meg, és zárja be a varázsló](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

<span data-ttu-id="a646c-184">Zárja be a varázslót, és várja meg, a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="a646c-184">Close the wizard and wait for the setup to complete.</span></span>

> [!TIP]
> <span data-ttu-id="a646c-185">A Sample függvény segítségével ellenőrizze, hogy a bemeneti elérési utat helyesen van beállítva.</span><span class="sxs-lookup"><span data-stu-id="a646c-185">Use the Sample function to check that you have set the input path correctly.</span></span> <span data-ttu-id="a646c-186">Ha a hiba: Ellenőrizze, hogy van-e adatokat a minta időtartományát úgy döntött, hogy a tároló.</span><span class="sxs-lookup"><span data-stu-id="a646c-186">If it fails: Check that there is data in the storage for the sample time range you chose.</span></span> <span data-ttu-id="a646c-187">A bemeneti definition szerkessze, és ellenőrizze, állítsa be a tárfiók, elérési út előtag és a dátum helyesen formátumban.</span><span class="sxs-lookup"><span data-stu-id="a646c-187">Edit the input definition and check you set the storage account, path prefix and date format correctly.</span></span>
> 
> 

## <a name="set-query"></a><span data-ttu-id="a646c-188">Set-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="a646c-188">Set query</span></span>
<span data-ttu-id="a646c-189">Nyissa meg a lekérdezés szakaszt:</span><span class="sxs-lookup"><span data-stu-id="a646c-189">Open the query section:</span></span>

![A stream analytics válassza ki a lekérdezés](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

<span data-ttu-id="a646c-191">Cserélje le az alapértelmezett lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="a646c-191">Replace the default query with:</span></span>

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

<span data-ttu-id="a646c-192">Figyelje meg, hogy az első néhány tulajdonságok Lapmegtekintések adatainak vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="a646c-192">Notice that the first few properties are specific to page view data.</span></span> <span data-ttu-id="a646c-193">Más telemetriai típusú kivitel más tulajdonságokkal fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="a646c-193">Exports of other telemetry types will have different properties.</span></span> <span data-ttu-id="a646c-194">Tekintse meg a [részletes adatok modell hivatkozást a tulajdonság típusát és értékét.](app-insights-export-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="a646c-194">See the [detailed data model reference for the property types and values.](app-insights-export-data-model.md)</span></span>

## <a name="set-up-output-to-database"></a><span data-ttu-id="a646c-195">Kimeneti adatbázis beállítása</span><span class="sxs-lookup"><span data-stu-id="a646c-195">Set up output to database</span></span>
<span data-ttu-id="a646c-196">Válassza ki az SQL kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="a646c-196">Select SQL as the output.</span></span>

![A stream analytics válassza ki a kimenetek](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

<span data-ttu-id="a646c-198">Adja meg az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a646c-198">Specify the SQL database.</span></span>

![Adja meg az adatbázis részletei](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

<span data-ttu-id="a646c-200">Zárja be a varázslót, és várja meg, egy értesítés, hogy a kimeneti be van állítva.</span><span class="sxs-lookup"><span data-stu-id="a646c-200">Close the wizard and wait for a notification that the output has been set up.</span></span>

## <a name="start-processing"></a><span data-ttu-id="a646c-201">Indítsa el a feldolgozási</span><span class="sxs-lookup"><span data-stu-id="a646c-201">Start processing</span></span>
<span data-ttu-id="a646c-202">Indítsa el a feladatot az műveletsávon:</span><span class="sxs-lookup"><span data-stu-id="a646c-202">Start the job from the action bar:</span></span>

![A stream analytics kattintson a Start](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

<span data-ttu-id="a646c-204">Dönthet úgy, hogy-e el most, vagy kezdje korábbi adatok kiindulva adatainak feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="a646c-204">You can choose whether to start processing the data starting from now, or to start with earlier data.</span></span> <span data-ttu-id="a646c-205">Az utóbbi akkor hasznos, ha korábban a folyamatos exportálás már fut egy ideig.</span><span class="sxs-lookup"><span data-stu-id="a646c-205">The latter is useful if you have had Continuous Export already running for a while.</span></span>

![A stream analytics kattintson a Start](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

<span data-ttu-id="a646c-207">Néhány perc elteltével lépjen vissza az SQL Server felügyeleti eszközei, és tekintse meg a áramló adatokat.</span><span class="sxs-lookup"><span data-stu-id="a646c-207">After a few minutes, go back to SQL Server Management Tools and watch the data flowing in.</span></span> <span data-ttu-id="a646c-208">Például lekérdezéssel ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="a646c-208">For example, use a query like this:</span></span>

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a><span data-ttu-id="a646c-209">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="a646c-209">Related articles</span></span>
* [<span data-ttu-id="a646c-210">A Power bi szolgáltatásba, használja a Stream Analytics exportálása</span><span class="sxs-lookup"><span data-stu-id="a646c-210">Export to PowerBI using Stream Analytics</span></span>](app-insights-export-power-bi.md)
* [<span data-ttu-id="a646c-211">A részletes adatok modell útmutató a tulajdonság típusát és értékét.</span><span class="sxs-lookup"><span data-stu-id="a646c-211">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="a646c-212">A folyamatos Exportálás az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="a646c-212">Continuous Export in Application Insights</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="a646c-213">Application Insights</span><span class="sxs-lookup"><span data-stu-id="a646c-213">Application Insights</span></span>](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

