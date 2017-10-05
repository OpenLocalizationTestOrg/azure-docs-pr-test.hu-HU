---
title: "Azure Application Insights Docker alkalmazások figyelésének |} Microsoft Docs"
description: "Docker teljesítményszámlálói, eseményeket és kivételeket is megjeleníthetők az Application Insights együtt a telemetriai adatok indexelése alkalmazásokból."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: b082e345ca1bb3b12c548e05e699474d3aa9306c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a><span data-ttu-id="60bf2-103">Az Application Insightsban Docker-alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="60bf2-103">Monitor Docker applications in Application Insights</span></span>
<span data-ttu-id="60bf2-104">A teljesítményszámlálók életciklus-események és a teljesítmény [Docker](https://www.docker.com/) tárolók is forrásadatok az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="60bf2-104">Lifecycle events and performance counters from [Docker](https://www.docker.com/) containers can be charted on Application Insights.</span></span> <span data-ttu-id="60bf2-105">Telepítse a [Application Insights](app-insights-overview.md) lemezkép a gazdagép és a tároló teljesítményszámlálók megjeleníti a gazdagép, valamint a többi képet.</span><span class="sxs-lookup"><span data-stu-id="60bf2-105">Install the [Application Insights](app-insights-overview.md) image in a container in your host, and it will display performance counters for the host, as well as for the other images.</span></span>

<span data-ttu-id="60bf2-106">Docker az egyszerűsített tárolók teljes összes függőségekkel rendelkező alkalmazások terjesztése.</span><span class="sxs-lookup"><span data-stu-id="60bf2-106">With Docker, you distribute your apps in lightweight containers complete with all dependencies.</span></span> <span data-ttu-id="60bf2-107">Ezek fogja futtatni, a gazdagépen, amelyen egy Docker-motorhoz.</span><span class="sxs-lookup"><span data-stu-id="60bf2-107">They'll run on any host machine that runs a Docker Engine.</span></span>

<span data-ttu-id="60bf2-108">Futtatásakor a [Application Insights kép](https://hub.docker.com/r/microsoft/applicationinsights/) a Docker állomáson ilyen előnyt beolvasása:</span><span class="sxs-lookup"><span data-stu-id="60bf2-108">When you run the [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) on your Docker host, you get these benefits:</span></span>

* <span data-ttu-id="60bf2-109">Életciklus telemetriai adatainak fut az összes tároló a gazdagép - indítása, leállítása, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="60bf2-109">Lifecycle telemetry about all the containers running on the host - start, stop, and so on.</span></span>
* <span data-ttu-id="60bf2-110">A tárolók teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="60bf2-110">Performance counters for all the containers.</span></span> <span data-ttu-id="60bf2-111">Processzor, memória, hálózati használati és több.</span><span class="sxs-lookup"><span data-stu-id="60bf2-111">CPU, memory, network usage, and more.</span></span>
* <span data-ttu-id="60bf2-112">Ha Ön [Javához készült Application Insights SDK telepítve](app-insights-java-live.md) a tárolókban lévő futó alkalmazások, az alkalmazások telemetriai adatok azonosítása a tároló és a fogadó számítógép további tulajdonságainak rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="60bf2-112">If you [installed Application Insights SDK for Java](app-insights-java-live.md) in the apps running in the containers, all the telemetry of those apps will have additional properties identifying the container and host machine.</span></span> <span data-ttu-id="60bf2-113">Így például ha egy alkalmazás több gazdagépen fut, könnyen szűrheti az alkalmazás telemetriai állomás.</span><span class="sxs-lookup"><span data-stu-id="60bf2-113">So for example, if you have instances of an app running in more than one host, you can easily filter your app telemetry by host.</span></span>

![Példa](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a><span data-ttu-id="60bf2-115">Az Application Insights-erőforrás beállítása</span><span class="sxs-lookup"><span data-stu-id="60bf2-115">Set up your Application Insights resource</span></span>
1. <span data-ttu-id="60bf2-116">Jelentkezzen be a [Microsoft Azure-portálon](https://azure.com) , és nyissa meg az Application Insights-erőforrást az alkalmazáshoz; vagy [hozzon létre egy újat](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="60bf2-116">Sign into [Microsoft Azure portal](https://azure.com) and open the Application Insights resource for your app; or [create a new one](app-insights-create-new-resource.md).</span></span> 
   
    <span data-ttu-id="60bf2-117">*Erőforrás-érdemes használni?*</span><span class="sxs-lookup"><span data-stu-id="60bf2-117">*Which resource should I use?*</span></span> <span data-ttu-id="60bf2-118">Ha az alkalmazások, a gazdagépen futó fejlesztett valaki más, akkor kell [hozzon létre egy új Application Insights-erőforrást](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="60bf2-118">If the apps that you are running on your host were developed by someone else, then you need to [create a new Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="60bf2-119">Ez az megtekintése, és ahol a telemetriai adatok elemzése.</span><span class="sxs-lookup"><span data-stu-id="60bf2-119">This is where you view and analyze the telemetry.</span></span> <span data-ttu-id="60bf2-120">(Válassza ki az alkalmazás típusa az "általános".)</span><span class="sxs-lookup"><span data-stu-id="60bf2-120">(Select 'General' for the app type.)</span></span>
   
    <span data-ttu-id="60bf2-121">De ha Ön az alkalmazásokat a fejlesztői, majd Reméljük, hogy [Application Insights SDK hozzáadott](app-insights-java-live.md) hozzájuk.</span><span class="sxs-lookup"><span data-stu-id="60bf2-121">But if you're the developer of the apps, then we hope you [added Application Insights SDK](app-insights-java-live.md) to each of them.</span></span> <span data-ttu-id="60bf2-122">Ha minden valóban az egyetlen üzleti alkalmazások összetevői, akkor előfordulhat, hogy konfigurálja az összes telemetriai adatokat küldhet egy erőforrást, és azt ismertetjük, hogy ugyanaz az erőforrás a Docker életciklust és a teljesítmény adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="60bf2-122">If they're all really components of a single business application, then you might configure all of them to send telemetry to one resource, and you'll use that same resource to display the Docker lifecycle and performance data.</span></span> 
   
    <span data-ttu-id="60bf2-123">Harmadik forgatókönyv, hogy az alkalmazások a legtöbb kidolgozott, de külön erőforrások használata a telemetriai adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="60bf2-123">A third scenario is that you developed most of the apps, but you are using separate resources to display their telemetry.</span></span> <span data-ttu-id="60bf2-124">Ebben az esetben akkor valószínűleg is szeretné a Docker adatokat külön erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="60bf2-124">In that case, you probably also want to create a separate resource for the Docker data.</span></span> 
2. <span data-ttu-id="60bf2-125">Adja hozzá a Docker csempe: válasszon **vegye fel a csempe**, húzza a Docker csempe a gyűjteményből, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="60bf2-125">Add the Docker tile: Choose **Add Tile**, drag the Docker tile from the gallery, and then click **Done**.</span></span> 
   
    ![Példa](./media/app-insights-docker/03.png)
3. <span data-ttu-id="60bf2-127">Kattintson a **Essentials** legördülő lista és a rendszerállapot-kulcs másolása.</span><span class="sxs-lookup"><span data-stu-id="60bf2-127">Click the **Essentials** drop-down and copy the Instrumentation Key.</span></span> <span data-ttu-id="60bf2-128">Ezzel, hogy az SDK helyét a telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="60bf2-128">You use this to tell the SDK where to send its telemetry.</span></span>

    ![Példa](./media/app-insights-docker/02-props.png)

<span data-ttu-id="60bf2-130">Tartsa böngészőablakot lesz szüksége, akkor lesz térjen vissza úgy, hogy hamarosan nézze meg a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="60bf2-130">Keep that browser window handy, as you'll come back to it soon to look at your telemetry.</span></span>

## <a name="run-the-application-insights-monitor-on-your-host"></a><span data-ttu-id="60bf2-131">A gazdagépen az Application Insights-figyelő futtatása</span><span class="sxs-lookup"><span data-stu-id="60bf2-131">Run the Application Insights monitor on your host</span></span>
<span data-ttu-id="60bf2-132">Most, hogy telepítve vannak-e valahol telemetriai adatok megjelenítéséhez, a tárolóalapú alkalmazást, amely gyűjt, és elküldi a állíthat be.</span><span class="sxs-lookup"><span data-stu-id="60bf2-132">Now that you've got somewhere to display the telemetry, you can set up the containerized app that will collect and send it.</span></span>

1. <span data-ttu-id="60bf2-133">A Docker-állomással lépnek kapcsolatba.</span><span class="sxs-lookup"><span data-stu-id="60bf2-133">Connect to your Docker host.</span></span> 
2. <span data-ttu-id="60bf2-134">Szerkesztheti a instrumentation be ezt a parancsot, és futtassa azt:</span><span class="sxs-lookup"><span data-stu-id="60bf2-134">Edit your instrumentation key into this command, and then run it:</span></span>
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

<span data-ttu-id="60bf2-135">Csak egy Application Insights-lemezkép szükség a Docker állomásonként.</span><span class="sxs-lookup"><span data-stu-id="60bf2-135">Only one Application Insights image is required per Docker host.</span></span> <span data-ttu-id="60bf2-136">Ha az alkalmazás több Docker gazdagépen van telepítve, majd ismételje meg a parancs minden gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="60bf2-136">If your application is deployed on multiple Docker hosts, then repeat the command on every host.</span></span>

## <a name="update-your-app"></a><span data-ttu-id="60bf2-137">Az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="60bf2-137">Update your app</span></span>
<span data-ttu-id="60bf2-138">Ha az alkalmazást a rendszer tagolva a [Javához készült Application Insights SDK](app-insights-java-get-started.md), adja meg a következő sort a ApplicationInsights.xml fájlba a projekt alatt a `<TelemetryInitializers>` elem:</span><span class="sxs-lookup"><span data-stu-id="60bf2-138">If your application is instrumented with the [Application Insights SDK for Java](app-insights-java-get-started.md), add the following line into the ApplicationInsights.xml file in your project, under the `<TelemetryInitializers>` element:</span></span>

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

<span data-ttu-id="60bf2-139">Minden az alkalmazásból küldött telemetriai elem hozzáadása Docker információkat, például a tároló és az állomás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="60bf2-139">This adds Docker information such as container and host id to every telemetry item sent from your app.</span></span>

## <a name="view-your-telemetry"></a><span data-ttu-id="60bf2-140">A telemetria megtekintése</span><span class="sxs-lookup"><span data-stu-id="60bf2-140">View your telemetry</span></span>
<span data-ttu-id="60bf2-141">Térjen vissza az Application Insights-erőforrást az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="60bf2-141">Go back to your Application Insights resource in the Azure portal.</span></span>

<span data-ttu-id="60bf2-142">Kattintson a Docker csempén.</span><span class="sxs-lookup"><span data-stu-id="60bf2-142">Click through the Docker tile.</span></span>

<span data-ttu-id="60bf2-143">A Docker alkalmazásból érkező adatok hamarosan megjelenik, különösen akkor, ha más tárolók futó a Docker-motorhoz.</span><span class="sxs-lookup"><span data-stu-id="60bf2-143">You'll shortly see data arriving from the Docker app, especially if you have other containers running on your Docker engine.</span></span>

<span data-ttu-id="60bf2-144">Íme néhány a nézetek kaphat.</span><span class="sxs-lookup"><span data-stu-id="60bf2-144">Here are some of the views you can get.</span></span>

### <a name="perf-counters-by-host-activity-by-image"></a><span data-ttu-id="60bf2-145">Állomás, kép tevékenység teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="60bf2-145">Perf counters by host, activity by image</span></span>
![Példa](./media/app-insights-docker/10.png)

![Példa](./media/app-insights-docker/11.png)

<span data-ttu-id="60bf2-148">Kattintson a további részletek gazdagép- vagy képfájl nevére.</span><span class="sxs-lookup"><span data-stu-id="60bf2-148">Click any host or image name for more detail.</span></span>

<span data-ttu-id="60bf2-149">A nézet testreszabásához kattintson bármelyik olyan diagram, a rács fejléc, vagy adja hozzá a diagram.</span><span class="sxs-lookup"><span data-stu-id="60bf2-149">To customize the view, click any chart, the grid heading, or use Add Chart.</span></span> 

<span data-ttu-id="60bf2-150">[További információ a metrikaböngésző](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="60bf2-150">[Learn more about metrics explorer](app-insights-metrics-explorer.md).</span></span>

### <a name="docker-container-events"></a><span data-ttu-id="60bf2-151">Docker-tároló események</span><span class="sxs-lookup"><span data-stu-id="60bf2-151">Docker container events</span></span>
![Példa](./media/app-insights-docker/13.png)

<span data-ttu-id="60bf2-153">Vizsgálja meg az egyes események, kattintson a [keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="60bf2-153">To investigate individual events, click [Search](app-insights-diagnostic-search.md).</span></span> <span data-ttu-id="60bf2-154">Keresés és szűrés kívánt események kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="60bf2-154">Search and filter to find the events you want.</span></span> <span data-ttu-id="60bf2-155">Kattintson azon események segítségével további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="60bf2-155">Click any event to get more detail.</span></span>

### <a name="exceptions-by-container-name"></a><span data-ttu-id="60bf2-156">Kivételek tároló neve</span><span class="sxs-lookup"><span data-stu-id="60bf2-156">Exceptions by container name</span></span>
![Példa](./media/app-insights-docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a><span data-ttu-id="60bf2-158">Docker környezet hozzá lett adva, az alkalmazás telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="60bf2-158">Docker context added to app telemetry</span></span>
<span data-ttu-id="60bf2-159">AI SDK-t és Docker környezetben növelést tagolva az alkalmazásból küldött telemetriai kérelem:</span><span class="sxs-lookup"><span data-stu-id="60bf2-159">Request telemetry sent from the application instrumented with AI SDK, enriched with Docker context:</span></span>

![Példa](./media/app-insights-docker/16.png)

<span data-ttu-id="60bf2-161">Processzor és memória teljesítményszámlálókat, dúsított, majd Docker-tároló neve szerint csoportosítva:</span><span class="sxs-lookup"><span data-stu-id="60bf2-161">Processor time and available memory performance counters, enriched and grouped by Docker container name:</span></span>

![Példa](./media/app-insights-docker/15.png)

## <a name="q--a"></a><span data-ttu-id="60bf2-163">Kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="60bf2-163">Q & A</span></span>
<span data-ttu-id="60bf2-164">*Mit nem az Application Insights engedi, hogy nem jelenik meg a Docker?*</span><span class="sxs-lookup"><span data-stu-id="60bf2-164">*What does Application Insights give me that I can't get from Docker?*</span></span>

* <span data-ttu-id="60bf2-165">Részletes információkat a tároló és a lemezkép teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="60bf2-165">Detailed breakdown of performance counters by container and image.</span></span>
* <span data-ttu-id="60bf2-166">Integrálják a tároló és az alkalmazás adatokat egy irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="60bf2-166">Integrate container and app data in one dashboard.</span></span>
* <span data-ttu-id="60bf2-167">[Telemetriai adatok exportálása](app-insights-export-telemetry.md) adatbázis, a Power bi-ban vagy más irányítópult további elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="60bf2-167">[Export telemetry](app-insights-export-telemetry.md) for further analysis to a database, Power BI or other dashboard.</span></span>

<span data-ttu-id="60bf2-168">*Hogyan szerezhetek telemetriai magát az alkalmazásból?*</span><span class="sxs-lookup"><span data-stu-id="60bf2-168">*How do I get telemetry from the app itself?*</span></span>

* <span data-ttu-id="60bf2-169">Telepítse az Application Insights SDK az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="60bf2-169">Install the Application Insights SDK in the app.</span></span> <span data-ttu-id="60bf2-170">Megtudhatja, hogyan lehet a: [Java-webalkalmazások](app-insights-java-get-started.md), [Windows webalkalmazások](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="60bf2-170">Learn how for: [Java web apps](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).</span></span>

## <a name="video"></a><span data-ttu-id="60bf2-171">Videó</span><span class="sxs-lookup"><span data-stu-id="60bf2-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="60bf2-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60bf2-172">Next steps</span></span>

* [<span data-ttu-id="60bf2-173">Javához készült Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="60bf2-173">Application Insights for Java</span></span>](app-insights-java-get-started.md)
* [<span data-ttu-id="60bf2-174">A Node.js Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="60bf2-174">Application Insights for Node.js</span></span>](app-insights-nodejs.md)
* [<span data-ttu-id="60bf2-175">Az ASP.NET Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="60bf2-175">Application Insights for ASP.NET</span></span>](app-insights-asp-net.md)
