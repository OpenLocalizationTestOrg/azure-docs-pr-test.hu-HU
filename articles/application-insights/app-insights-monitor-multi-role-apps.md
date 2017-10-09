---
title: "a több összetevők, a mikroszolgáltatások létrehozására és a tárolók támogatása az Application Insights aaaAzure |} Microsoft Docs"
description: "Figyelési alkalmazásokat, amelyek több összetevők vagy a teljesítmény- és használati szerepkörök állnak."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: 6185eedf32ec450d7541603b94de6c3dcdf64a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a><span data-ttu-id="18f1e-103">Az Application Insights (előzetes verzió) több összetevőt alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="18f1e-103">Monitor multi-component applications with Application Insights (preview)</span></span>

<span data-ttu-id="18f1e-104">Figyelheti az alkalmazások, amelyek több kiszolgáló-összetevők, szerepkörök vagy szolgáltatások állnak [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="18f1e-104">You can monitor apps that consist of multiple server components, roles, or services with [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="18f1e-105">egyetlen alkalmazás térképként hello összetevők és a közöttük hello kapcsolatok hello állapotának jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="18f1e-105">hello health of hello components and hello relationships between them are displayed on a single Application Map.</span></span> <span data-ttu-id="18f1e-106">Egyes műveletek – több összetevő automatikus HTTP korrelációkereséssel vezethető vissza.</span><span class="sxs-lookup"><span data-stu-id="18f1e-106">You can trace individual operations through multiple components with automatic HTTP correlation.</span></span> <span data-ttu-id="18f1e-107">Tároló diagnosztika integrált, és szorosan összefügg az alkalmazás telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="18f1e-107">Container diagnostics can be integrated and correlated with application telemetry.</span></span> <span data-ttu-id="18f1e-108">Az alkalmazás összes hello összetevő egyetlen Application Insights-erőforrást használjon.</span><span class="sxs-lookup"><span data-stu-id="18f1e-108">Use a single Application Insights resource for all hello components of your application.</span></span> 

![Több összetevőt alkalmazás-hozzárendelés](./media/app-insights-monitor-multi-role-apps/app-map.png)

<span data-ttu-id="18f1e-110">"Összetevő" használjuk ide toomean bármely nagy alkalmazás működő részét.</span><span class="sxs-lookup"><span data-stu-id="18f1e-110">We use 'component' here toomean any functioning part of a large application.</span></span> <span data-ttu-id="18f1e-111">Például a szokásos üzleti alkalmazások állhat Ügyfélkód futó webböngészők tooone van szó, vagy további web app, használó szolgáltatások pedig vissza záró szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="18f1e-111">For example, a typical business application may consist of client code running in web browsers, talking tooone or more web app services, which in turn use back end services.</span></span> <span data-ttu-id="18f1e-112">Kiszolgáló-összetevők üzemeltethető a helyszínen a hello felhő, vagy lehet, hogy az Azure webes és feldolgozói szerepkörök, esetleg szereplő tárolókhoz, például Docker vagy a Service Fabric futtathatnak.</span><span class="sxs-lookup"><span data-stu-id="18f1e-112">Server components may be hosted on-premises on in hello cloud, or may be Azure web and worker roles, or may run in containers such as Docker or Service Fabric.</span></span> 

### <a name="sharing-a-single-application-insights-resource"></a><span data-ttu-id="18f1e-113">Egyetlen Application Insights-erőforrás megosztása</span><span class="sxs-lookup"><span data-stu-id="18f1e-113">Sharing a single Application Insights resource</span></span> 

<span data-ttu-id="18f1e-114">hello Itt a fő módszer minden összetevő a az alkalmazás toohello toosend telemetriai azonos Application Insights-erőforrást, de használja hello `cloud_RoleName` tulajdonság toodistinguish összetevők szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="18f1e-114">hello key technique here is toosend telemetry from every component in your application toohello same Application Insights resource, but use hello `cloud_RoleName` property toodistinguish components when necessary.</span></span> <span data-ttu-id="18f1e-115">hello Application Insights SDK hozzáadása hello `cloud_RoleName` tulajdonság toohello telemetriai összetevők hozható létre.</span><span class="sxs-lookup"><span data-stu-id="18f1e-115">hello Application Insights SDK adds hello `cloud_RoleName` property toohello telemetry components emit.</span></span> <span data-ttu-id="18f1e-116">Például hello SDK felveszi egy webhely neve, vagy a szolgáltatás-szerepkör nevét toohello `cloud_RoleName` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="18f1e-116">For example, hello SDK will add a web site name, or service role name toohello `cloud_RoleName` property.</span></span> <span data-ttu-id="18f1e-117">Ezt az értéket egy telemetryinitializer felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="18f1e-117">You can override this value with a telemetryinitializer.</span></span> <span data-ttu-id="18f1e-118">Alkalmazás-hozzárendelés hello használ hello `cloud_RoleName` tulajdonság tooidentify hello összetevők hello térképen.</span><span class="sxs-lookup"><span data-stu-id="18f1e-118">hello Application Map uses hello `cloud_RoleName` property tooidentify hello components on hello map.</span></span>

<span data-ttu-id="18f1e-119">További információ a felülírják hello `cloud_RoleName` tulajdonság lásd [tulajdonságok hozzáadása: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span><span class="sxs-lookup"><span data-stu-id="18f1e-119">For more information about how do override hello `cloud_RoleName` property see [Add properties: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span></span>  

<span data-ttu-id="18f1e-120">Néhány esetben ez nem lehet megfelelő, és célszerű toouse külön erőforrások összetevők különböző csoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="18f1e-120">In some cases, this may not be appropriate, and you may prefer toouse separate resources for different groups of components.</span></span> <span data-ttu-id="18f1e-121">Például szükség lehet különböző erőforrások toouse vagy más célra.</span><span class="sxs-lookup"><span data-stu-id="18f1e-121">For example, you might need toouse different resources for management or billing purposes.</span></span> <span data-ttu-id="18f1e-122">Külön erőforrásokat használó azt jelenti, hogy egyetlen alkalmazás térképen; összetevők hello nem jelenik meg és hogy Ön nem tudja lekérdezni a összetevői között [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="18f1e-122">Using separate resources means that you don't see all hello components displayed on a single Application Map; and that you can't query across components in [Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="18f1e-123">Akkor is tooset hello külön erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="18f1e-123">You also have tooset up hello separate resources.</span></span>

<span data-ttu-id="18f1e-124">Az adott ismeret feltételezzük, ez a dokumentum többi hello, amelyet több összetevők tooone Application Insights-erőforrás toosend adatait.</span><span class="sxs-lookup"><span data-stu-id="18f1e-124">With that caveat, we'll assume in hello rest of this document that you want toosend data from multiple components tooone Application Insights resource.</span></span>

## <a name="configure-multi-component-applications"></a><span data-ttu-id="18f1e-125">Több összetevőt alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="18f1e-125">Configure multi-component applications</span></span>

<span data-ttu-id="18f1e-126">tooget egy több összetevőt alkalmazás hozzárendelését, akkor kell tooachieve ezen célok:</span><span class="sxs-lookup"><span data-stu-id="18f1e-126">tooget a multi-component application map, you need tooachieve these goals:</span></span>

* <span data-ttu-id="18f1e-127">**Telepítse a legújabb előzetes hello** Application Insights csomagot minden hello alkalmazás-összetevője.</span><span class="sxs-lookup"><span data-stu-id="18f1e-127">**Install hello latest pre-release** Application Insights package in each component of hello application.</span></span> 
* <span data-ttu-id="18f1e-128">**Egyetlen Application Insights-erőforrás megosztása** az összes hello az alkalmazás összetevői.</span><span class="sxs-lookup"><span data-stu-id="18f1e-128">**Share a single Application Insights resource** for all hello components of your application.</span></span>
* <span data-ttu-id="18f1e-129">**Engedélyezze az alkalmazás több szerepkör-hozzárendelés** hello az előzetes verziójú funkciók a panelen.</span><span class="sxs-lookup"><span data-stu-id="18f1e-129">**Enable Multi-role Application Map** in hello Previews blade.</span></span>

<span data-ttu-id="18f1e-130">Minden ehhez a típushoz hello megfelelő módszer segítségével az alkalmazás-összetevő konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="18f1e-130">Configure each component of your application using hello appropriate method for its type.</span></span> <span data-ttu-id="18f1e-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span><span class="sxs-lookup"><span data-stu-id="18f1e-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span></span>

### <a name="1-install-hello-latest-pre-release-package"></a><span data-ttu-id="18f1e-132">1. Hello legújabb kiadás előtti csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="18f1e-132">1. Install hello latest pre-release package</span></span>

<span data-ttu-id="18f1e-133">Frissítés, vagy hello portot Insights csomagok telepítése minden egyes kiszolgáló-összetevő hello projektben.</span><span class="sxs-lookup"><span data-stu-id="18f1e-133">Update or install hello Appication Insights packages in hello project for each server component.</span></span> <span data-ttu-id="18f1e-134">Visual Studio használata:</span><span class="sxs-lookup"><span data-stu-id="18f1e-134">If you're using Visual Studio:</span></span>

1. <span data-ttu-id="18f1e-135">Kattintson jobb gombbal a projektre, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="18f1e-135">Right-click a project and select **Manage NuGet Packages**.</span></span> 
2. <span data-ttu-id="18f1e-136">Válassza ki **közé tartoznak az előzetes**.</span><span class="sxs-lookup"><span data-stu-id="18f1e-136">Select **Include prerelease**.</span></span>
3. <span data-ttu-id="18f1e-137">Ha az Application Insights csomagok frissítések jelenik meg, jelölje ki őket.</span><span class="sxs-lookup"><span data-stu-id="18f1e-137">If Application Insights packages appear in Updates, select them.</span></span> 

    <span data-ttu-id="18f1e-138">Ellenkező esetben és telepítésére hello megfelelő csomag:</span><span class="sxs-lookup"><span data-stu-id="18f1e-138">Otherwise, browse for and install hello appropriate package:</span></span>
    
    * <span data-ttu-id="18f1e-139">Microsoft.ApplicationInsights.WindowsServer</span><span class="sxs-lookup"><span data-stu-id="18f1e-139">Microsoft.ApplicationInsights.WindowsServer</span></span>
    * <span data-ttu-id="18f1e-140">Microsoft.ApplicationInsights.ServiceFabric - összetevők futtatásához használt Vendég végrehajtható fájlok és a Docker-tárolók a Service Fabric-alkalmazás fut</span><span class="sxs-lookup"><span data-stu-id="18f1e-140">Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application</span></span>
    * <span data-ttu-id="18f1e-141">Microsoft.ApplicationInsights.ServiceFabric.Native - ServiceFabric alkalmazások megbízható szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="18f1e-141">Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications</span></span>
    * <span data-ttu-id="18f1e-142">Microsoft.ApplicationInsights.Kubernetes futó Kubernetes Docker-összetevők</span><span class="sxs-lookup"><span data-stu-id="18f1e-142">Microsoft.ApplicationInsights.Kubernetes for components running in Docker on Kubernetes</span></span>

### <a name="2-share-a-single-application-insights-resource"></a><span data-ttu-id="18f1e-143">2. Egyetlen Application Insights-erőforrás megosztása</span><span class="sxs-lookup"><span data-stu-id="18f1e-143">2. Share a single Application Insights resource</span></span>

* <span data-ttu-id="18f1e-144">A Visual Studióban, kattintson jobb gombbal a projektre, és válassza ki **konfigurálja az Application Insights**, vagy **Application Insights > konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="18f1e-144">In Visual Studio, right-click a project and select **Configure Application Insights**, or **Application Insights > Configure**.</span></span> <span data-ttu-id="18f1e-145">Hello első projekt használja a hello varázsló toocreate Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="18f1e-145">For hello first project, use hello wizard toocreate an Application Insights resource.</span></span> <span data-ttu-id="18f1e-146">Az ezt követő projektek esetében válassza hello ugyanazt az erőforrást.</span><span class="sxs-lookup"><span data-stu-id="18f1e-146">For subsequent projects, select hello same resource.</span></span>
* <span data-ttu-id="18f1e-147">Ha nincs Application Insights menü, kézi konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="18f1e-147">If there is no Application Insights menu, configure manually:</span></span>

   1. <span data-ttu-id="18f1e-148">A [Azure-portálon](https://portal,azure.com), nyisson meg egy másik összetevő már létrehozott hello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="18f1e-148">In [Azure portal](https://portal,azure.com), open hello Application Insights resource you already created for another component.</span></span>
   2. <span data-ttu-id="18f1e-149">A hello áttekintése panelen, a nyitott hello Essentials legördülő lapon és a másolási hello **Instrumentation kulcs.**</span><span class="sxs-lookup"><span data-stu-id="18f1e-149">In hello Overview blade, open hello Essentials drop-down tab, and copy hello **Instrumentation Key.**</span></span>
   3. <span data-ttu-id="18f1e-150">A projektben nyissa meg az ApplicationInsights.config, és helyezze be:`<InstrumentationKey>your copied key</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="18f1e-150">In your project, open ApplicationInsights.config and insert: `<InstrumentationKey>your copied key</InstrumentationKey>`</span></span>

![Hello instrumentation kulcs toohello .config fájl másolása](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a><span data-ttu-id="18f1e-152">3. Alkalmazás több szerepkör-hozzárendelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="18f1e-152">3. Enable multi-role Application Map</span></span>

<span data-ttu-id="18f1e-153">Hello Azure-portálon nyissa meg az alkalmazás hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="18f1e-153">In hello Azure portal, open hello resource for your application.</span></span> <span data-ttu-id="18f1e-154">Hello előzetes panelen engedélyezése *alkalmazás több szerepkör-hozzárendelés*.</span><span class="sxs-lookup"><span data-stu-id="18f1e-154">In hello Previews blade, enable *Multi-role Application Map*.</span></span>

### <a name="4-enable-docker-metrics-optional"></a><span data-ttu-id="18f1e-155">4. Engedélyezze a Docker-metrikák (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="18f1e-155">4. Enable Docker metrics (Optional)</span></span> 

<span data-ttu-id="18f1e-156">Egy összetevő egy Docker egy Windows Azure virtuális gépen futó fut, ha további metrikák hello tárolóból hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="18f1e-156">If a component runs in a Docker hosted on an Azure Windows VM, you can collect additional metrics from hello container.</span></span> <span data-ttu-id="18f1e-157">Ezt a Beszúrás a [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="18f1e-157">Insert this in your [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuration file:</span></span>

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-tooseparate-components"></a><span data-ttu-id="18f1e-158">Cloud_RoleName tooseparate összetevőket használnak</span><span class="sxs-lookup"><span data-stu-id="18f1e-158">Use cloud_RoleName tooseparate components</span></span>

<span data-ttu-id="18f1e-159">Hello `cloud_RoleName` tulajdonsága csatolt tooall telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="18f1e-159">hello `cloud_RoleName` property is attached tooall telemetry.</span></span> <span data-ttu-id="18f1e-160">Hello összetevő - hello szerepkör vagy szolgáltatás - létrehozó hello telemetriai azonosítja.</span><span class="sxs-lookup"><span data-stu-id="18f1e-160">It identifies hello component - hello role or service - that originates hello telemetry.</span></span> <span data-ttu-id="18f1e-161">(Fontos nem hello ugyanaz, mint a cloud_RoleInstance, amely elválasztja az azonos több kiszolgáló folyamatok vagy gépek párhuzamosan futó szerepkörök.)</span><span class="sxs-lookup"><span data-stu-id="18f1e-161">(It is not hello same as cloud_RoleInstance, which separates identical roles that are running in parallel on multiple server processes or machines.)</span></span>

<span data-ttu-id="18f1e-162">Hello portálon kiszűrhetik vagy szegmentálja a telemetriai adatok e tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="18f1e-162">In hello portal, you can filter or segment your telemetry using this property.</span></span> <span data-ttu-id="18f1e-163">Ebben a példában a hello hibák panel szűrt tooshow csak információk hello előtér-webkiszolgáló szolgáltatás hello CRM API háttérrendszerből hibák kiszűrése:</span><span class="sxs-lookup"><span data-stu-id="18f1e-163">In this example, hello Failures blade is filtered tooshow just information from hello front-end web service, filtering out failures from hello CRM API backend:</span></span>

![Felhő szerepkör neve szegmentált metrika diagram](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a><span data-ttu-id="18f1e-165">Összetevők közötti nyomkövetési műveletek</span><span class="sxs-lookup"><span data-stu-id="18f1e-165">Trace operations between components</span></span>

<span data-ttu-id="18f1e-166">Nyomon követhetők a egy összetevő tooanother, hello hívások egy egyéni művelet feldolgozása közben.</span><span class="sxs-lookup"><span data-stu-id="18f1e-166">You can trace from one component tooanother, hello calls made while processing an individual operation.</span></span>


![Telemetria művelet megjelenítése](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

<span data-ttu-id="18f1e-168">Kattintson a telemetriai adat ehhez a művelethez kapcsolódó listája tooa hello előtér-webkiszolgáló és a háttér-API hello:</span><span class="sxs-lookup"><span data-stu-id="18f1e-168">Click through tooa correlated list of telemetry for this operation across hello front-end web server and hello back-end API:</span></span>

![Keresés összetevői között](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a><span data-ttu-id="18f1e-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18f1e-170">Next steps</span></span>

* [<span data-ttu-id="18f1e-171">A fejlesztői, tesztelési és éles külön telemetria</span><span class="sxs-lookup"><span data-stu-id="18f1e-171">Separate telemetry from Development, Test, and Production</span></span>](app-insights-separate-resources.md)
