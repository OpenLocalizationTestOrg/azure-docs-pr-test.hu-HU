---
title: "aaaMonitor Docker-alkalmazások Azure Application Insights |} Microsoft Docs"
description: "Docker teljesítményszámlálói, eseményeket és kivételeket is megjeleníthetők az Application Insights hello telemetriai indexelése hello alkalmazásokból együtt."
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
ms.openlocfilehash: 9aaf1076bae25485a396db1bb3dcd13bccd87c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a><span data-ttu-id="b2337-103">Az Application Insightsban Docker-alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="b2337-103">Monitor Docker applications in Application Insights</span></span>
<span data-ttu-id="b2337-104">A teljesítményszámlálók életciklus-események és a teljesítmény [Docker](https://www.docker.com/) tárolók is forrásadatok az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="b2337-104">Lifecycle events and performance counters from [Docker](https://www.docker.com/) containers can be charted on Application Insights.</span></span> <span data-ttu-id="b2337-105">Telepítse a hello [Application Insights](app-insights-overview.md) lemezkép a gazdagép és a tároló jelennek-teljesítményszámlálók hello állomás is megegyezik a hello más képek.</span><span class="sxs-lookup"><span data-stu-id="b2337-105">Install hello [Application Insights](app-insights-overview.md) image in a container in your host, and it will display performance counters for hello host, as well as for hello other images.</span></span>

<span data-ttu-id="b2337-106">Docker az egyszerűsített tárolók teljes összes függőségekkel rendelkező alkalmazások terjesztése.</span><span class="sxs-lookup"><span data-stu-id="b2337-106">With Docker, you distribute your apps in lightweight containers complete with all dependencies.</span></span> <span data-ttu-id="b2337-107">Ezek fogja futtatni, a gazdagépen, amelyen egy Docker-motorhoz.</span><span class="sxs-lookup"><span data-stu-id="b2337-107">They'll run on any host machine that runs a Docker Engine.</span></span>

<span data-ttu-id="b2337-108">Hello futtatásakor [Application Insights kép](https://hub.docker.com/r/microsoft/applicationinsights/) a Docker állomáson ilyen előnyt beolvasása:</span><span class="sxs-lookup"><span data-stu-id="b2337-108">When you run hello [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) on your Docker host, you get these benefits:</span></span>

* <span data-ttu-id="b2337-109">Életciklus telemetriai adatainak fut az összes hello tároló hello gazdagép - indítása, leállítása, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="b2337-109">Lifecycle telemetry about all hello containers running on hello host - start, stop, and so on.</span></span>
* <span data-ttu-id="b2337-110">Az összes hello tároló számlálói.</span><span class="sxs-lookup"><span data-stu-id="b2337-110">Performance counters for all hello containers.</span></span> <span data-ttu-id="b2337-111">Processzor, memória, hálózati használati és több.</span><span class="sxs-lookup"><span data-stu-id="b2337-111">CPU, memory, network usage, and more.</span></span>
* <span data-ttu-id="b2337-112">Ha Ön [Javához készült Application Insights SDK telepítve](app-insights-java-live.md) hello alkalmazások hello tárolókban lévő fut, az alkalmazások minden hello telemetriai hello tároló és a gazdagép további tulajdonságok rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="b2337-112">If you [installed Application Insights SDK for Java](app-insights-java-live.md) in hello apps running in hello containers, all hello telemetry of those apps will have additional properties identifying hello container and host machine.</span></span> <span data-ttu-id="b2337-113">Így például ha egy alkalmazás több gazdagépen fut, könnyen szűrheti az alkalmazás telemetriai állomás.</span><span class="sxs-lookup"><span data-stu-id="b2337-113">So for example, if you have instances of an app running in more than one host, you can easily filter your app telemetry by host.</span></span>

![Példa](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a><span data-ttu-id="b2337-115">Az Application Insights-erőforrás beállítása</span><span class="sxs-lookup"><span data-stu-id="b2337-115">Set up your Application Insights resource</span></span>
1. <span data-ttu-id="b2337-116">Jelentkezzen be a [Microsoft Azure-portálon](https://azure.com) , és nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz; vagy [hozzon létre egy újat](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="b2337-116">Sign into [Microsoft Azure portal](https://azure.com) and open hello Application Insights resource for your app; or [create a new one](app-insights-create-new-resource.md).</span></span> 
   
    <span data-ttu-id="b2337-117">*Erőforrás-érdemes használni?*</span><span class="sxs-lookup"><span data-stu-id="b2337-117">*Which resource should I use?*</span></span> <span data-ttu-id="b2337-118">Ha a gazdagépen futó alkalmazások hello fejlesztett valaki más, akkor meg kell túl[hozzon létre egy új Application Insights-erőforrást](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="b2337-118">If hello apps that you are running on your host were developed by someone else, then you need too[create a new Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="b2337-119">Ez az ahol megtekintése és elemzése hello telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="b2337-119">This is where you view and analyze hello telemetry.</span></span> <span data-ttu-id="b2337-120">(Válassza a "Általános" hello app típushoz.)</span><span class="sxs-lookup"><span data-stu-id="b2337-120">(Select 'General' for hello app type.)</span></span>
   
    <span data-ttu-id="b2337-121">De ha hello fejlesztői hello alkalmazásokat, majd Reméljük, hogy [Application Insights SDK hozzáadott](app-insights-java-live.md) tooeach közülük.</span><span class="sxs-lookup"><span data-stu-id="b2337-121">But if you're hello developer of hello apps, then we hope you [added Application Insights SDK](app-insights-java-live.md) tooeach of them.</span></span> <span data-ttu-id="b2337-122">Ha valóban az valamennyi összetevője egy egyetlen üzleti alkalmazást, majd konfigurálhatja az összes toosend telemetriai tooone erőforrás, és fogja használni, hogy ugyanazon erőforrás toodisplay hello Docker életciklust és a teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="b2337-122">If they're all really components of a single business application, then you might configure all of them toosend telemetry tooone resource, and you'll use that same resource toodisplay hello Docker lifecycle and performance data.</span></span> 
   
    <span data-ttu-id="b2337-123">Egy harmadik forgatókönyv, hello alkalmazások többsége kidolgozott, de használ külön erőforrások toodisplay a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="b2337-123">A third scenario is that you developed most of hello apps, but you are using separate resources toodisplay their telemetry.</span></span> <span data-ttu-id="b2337-124">Ebben az esetben akkor valószínűleg is kívánt toocreate hello Docker adatokat külön erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b2337-124">In that case, you probably also want toocreate a separate resource for hello Docker data.</span></span> 
2. <span data-ttu-id="b2337-125">Hozzáadás hello Docker csempe: válasszon **vegye fel a csempe**, húzza hello Docker csempe hello gyűjteményből, és kattintson a **kész**.</span><span class="sxs-lookup"><span data-stu-id="b2337-125">Add hello Docker tile: Choose **Add Tile**, drag hello Docker tile from hello gallery, and then click **Done**.</span></span> 
   
    ![Példa](./media/app-insights-docker/03.png)
3. <span data-ttu-id="b2337-127">Kattintson a hello **Essentials** legördülő és hello Instrumentation kulcs másolása.</span><span class="sxs-lookup"><span data-stu-id="b2337-127">Click hello **Essentials** drop-down and copy hello Instrumentation Key.</span></span> <span data-ttu-id="b2337-128">Használja a tootell hello SDK ahol toosend a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="b2337-128">You use this tootell hello SDK where toosend its telemetry.</span></span>

    ![Példa](./media/app-insights-docker/02-props.png)

<span data-ttu-id="b2337-130">Megőrizni böngészőablakot lesz szüksége, szerint, majd térjen vissza tooit hamarosan toolook a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="b2337-130">Keep that browser window handy, as you'll come back tooit soon toolook at your telemetry.</span></span>

## <a name="run-hello-application-insights-monitor-on-your-host"></a><span data-ttu-id="b2337-131">Hello Application Insights-figyelő futtatása a gazdagépen</span><span class="sxs-lookup"><span data-stu-id="b2337-131">Run hello Application Insights monitor on your host</span></span>
<span data-ttu-id="b2337-132">Most, hogy telepítve vannak-e valahol toodisplay hello telemetriai, hello indexelése alkalmazást, amely gyűjt, és elküldi a állíthat be.</span><span class="sxs-lookup"><span data-stu-id="b2337-132">Now that you've got somewhere toodisplay hello telemetry, you can set up hello containerized app that will collect and send it.</span></span>

1. <span data-ttu-id="b2337-133">Tooyour Docker gazdagép csatlakoztatása.</span><span class="sxs-lookup"><span data-stu-id="b2337-133">Connect tooyour Docker host.</span></span> 
2. <span data-ttu-id="b2337-134">Szerkesztheti a instrumentation be ezt a parancsot, és futtassa azt:</span><span class="sxs-lookup"><span data-stu-id="b2337-134">Edit your instrumentation key into this command, and then run it:</span></span>
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

<span data-ttu-id="b2337-135">Csak egy Application Insights-lemezkép szükség a Docker állomásonként.</span><span class="sxs-lookup"><span data-stu-id="b2337-135">Only one Application Insights image is required per Docker host.</span></span> <span data-ttu-id="b2337-136">Ha az alkalmazás több Docker gazdagépen van telepítve, majd ismételje meg a hello parancsot minden gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="b2337-136">If your application is deployed on multiple Docker hosts, then repeat hello command on every host.</span></span>

## <a name="update-your-app"></a><span data-ttu-id="b2337-137">Az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="b2337-137">Update your app</span></span>
<span data-ttu-id="b2337-138">Ha az alkalmazás a hello van tagolva [Javához készült Application Insights SDK](app-insights-java-get-started.md), adja hozzá a következő sor hello ApplicationInsights.xml fájlba a projekt alatt hello hello `<TelemetryInitializers>` elem:</span><span class="sxs-lookup"><span data-stu-id="b2337-138">If your application is instrumented with hello [Application Insights SDK for Java](app-insights-java-get-started.md), add hello following line into hello ApplicationInsights.xml file in your project, under hello `<TelemetryInitializers>` element:</span></span>

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

<span data-ttu-id="b2337-139">Ez biztosítja a Docker-információk, például a tároló és az állomás azonosítója tooevery telemetriai elem küldése az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="b2337-139">This adds Docker information such as container and host id tooevery telemetry item sent from your app.</span></span>

## <a name="view-your-telemetry"></a><span data-ttu-id="b2337-140">A telemetria megtekintése</span><span class="sxs-lookup"><span data-stu-id="b2337-140">View your telemetry</span></span>
<span data-ttu-id="b2337-141">Lépjen vissza az Azure-portálon hello tooyour Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b2337-141">Go back tooyour Application Insights resource in hello Azure portal.</span></span>

<span data-ttu-id="b2337-142">Kattintson a hello Docker csempe.</span><span class="sxs-lookup"><span data-stu-id="b2337-142">Click through hello Docker tile.</span></span>

<span data-ttu-id="b2337-143">Hello Docker alkalmazásból érkező adatok hamarosan megjelenik, különösen akkor, ha más tárolók futó a Docker-motorhoz.</span><span class="sxs-lookup"><span data-stu-id="b2337-143">You'll shortly see data arriving from hello Docker app, especially if you have other containers running on your Docker engine.</span></span>

<span data-ttu-id="b2337-144">Az alábbiakban néhány hello nézetek kaphat.</span><span class="sxs-lookup"><span data-stu-id="b2337-144">Here are some of hello views you can get.</span></span>

### <a name="perf-counters-by-host-activity-by-image"></a><span data-ttu-id="b2337-145">Állomás, kép tevékenység teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="b2337-145">Perf counters by host, activity by image</span></span>
![Példa](./media/app-insights-docker/10.png)

![Példa](./media/app-insights-docker/11.png)

<span data-ttu-id="b2337-148">Kattintson a további részletek gazdagép- vagy képfájl nevére.</span><span class="sxs-lookup"><span data-stu-id="b2337-148">Click any host or image name for more detail.</span></span>

<span data-ttu-id="b2337-149">toocustomize hello nézetet, kattintson a bármely diagram, hello rács fejléc, vagy adja hozzá a diagram.</span><span class="sxs-lookup"><span data-stu-id="b2337-149">toocustomize hello view, click any chart, hello grid heading, or use Add Chart.</span></span> 

<span data-ttu-id="b2337-150">[További információ a metrikaböngésző](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="b2337-150">[Learn more about metrics explorer](app-insights-metrics-explorer.md).</span></span>

### <a name="docker-container-events"></a><span data-ttu-id="b2337-151">Docker-tároló események</span><span class="sxs-lookup"><span data-stu-id="b2337-151">Docker container events</span></span>
![Példa](./media/app-insights-docker/13.png)

<span data-ttu-id="b2337-153">egyéni események tooinvestigate, kattintson a [keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="b2337-153">tooinvestigate individual events, click [Search](app-insights-diagnostic-search.md).</span></span> <span data-ttu-id="b2337-154">Keresés és szűrés toofind hello eseményeket.</span><span class="sxs-lookup"><span data-stu-id="b2337-154">Search and filter toofind hello events you want.</span></span> <span data-ttu-id="b2337-155">Kattintson a bármely esemény tooget további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="b2337-155">Click any event tooget more detail.</span></span>

### <a name="exceptions-by-container-name"></a><span data-ttu-id="b2337-156">Kivételek tároló neve</span><span class="sxs-lookup"><span data-stu-id="b2337-156">Exceptions by container name</span></span>
![Példa](./media/app-insights-docker/14.png)

### <a name="docker-context-added-tooapp-telemetry"></a><span data-ttu-id="b2337-158">Docker környezetben hozzáadott tooapp telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="b2337-158">Docker context added tooapp telemetry</span></span>
<span data-ttu-id="b2337-159">AI SDK-t és Docker környezetben növelést tagolva hello alkalmazásból küldött telemetriai kérelem:</span><span class="sxs-lookup"><span data-stu-id="b2337-159">Request telemetry sent from hello application instrumented with AI SDK, enriched with Docker context:</span></span>

![Példa](./media/app-insights-docker/16.png)

<span data-ttu-id="b2337-161">Processzor és memória teljesítményszámlálókat, dúsított, majd Docker-tároló neve szerint csoportosítva:</span><span class="sxs-lookup"><span data-stu-id="b2337-161">Processor time and available memory performance counters, enriched and grouped by Docker container name:</span></span>

![Példa](./media/app-insights-docker/15.png)

## <a name="q--a"></a><span data-ttu-id="b2337-163">Kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="b2337-163">Q & A</span></span>
<span data-ttu-id="b2337-164">*Mit nem az Application Insights engedi, hogy nem jelenik meg a Docker?*</span><span class="sxs-lookup"><span data-stu-id="b2337-164">*What does Application Insights give me that I can't get from Docker?*</span></span>

* <span data-ttu-id="b2337-165">Részletes információkat a tároló és a lemezkép teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="b2337-165">Detailed breakdown of performance counters by container and image.</span></span>
* <span data-ttu-id="b2337-166">Integrálják a tároló és az alkalmazás adatokat egy irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b2337-166">Integrate container and app data in one dashboard.</span></span>
* <span data-ttu-id="b2337-167">[Telemetriai adatok exportálása](app-insights-export-telemetry.md) további elemzés tooa adatbázishoz, a Power bi-ban vagy a más irányítópult.</span><span class="sxs-lookup"><span data-stu-id="b2337-167">[Export telemetry](app-insights-export-telemetry.md) for further analysis tooa database, Power BI or other dashboard.</span></span>

<span data-ttu-id="b2337-168">*Hogyan szerezhetek telemetriai maga hello alkalmazásból?*</span><span class="sxs-lookup"><span data-stu-id="b2337-168">*How do I get telemetry from hello app itself?*</span></span>

* <span data-ttu-id="b2337-169">Hello Application Insights SDK telepítése hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b2337-169">Install hello Application Insights SDK in hello app.</span></span> <span data-ttu-id="b2337-170">Megtudhatja, hogyan lehet a: [Java-webalkalmazások](app-insights-java-get-started.md), [Windows webalkalmazások](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="b2337-170">Learn how for: [Java web apps](app-insights-java-get-started.md), [Windows web apps](app-insights-asp-net.md).</span></span>

## <a name="video"></a><span data-ttu-id="b2337-171">Videó</span><span class="sxs-lookup"><span data-stu-id="b2337-171">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="b2337-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b2337-172">Next steps</span></span>

* [<span data-ttu-id="b2337-173">Javához készült Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b2337-173">Application Insights for Java</span></span>](app-insights-java-get-started.md)
* [<span data-ttu-id="b2337-174">A Node.js Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b2337-174">Application Insights for Node.js</span></span>](app-insights-nodejs.md)
* [<span data-ttu-id="b2337-175">Az ASP.NET Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b2337-175">Application Insights for ASP.NET</span></span>](app-insights-asp-net.md)
