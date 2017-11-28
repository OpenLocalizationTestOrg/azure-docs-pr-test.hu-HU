---
title: "Azure Application Insights támogatja a több összetevőt, a mikroszolgáltatások létrehozására és a tárolók |} Microsoft Docs"
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
ms.openlocfilehash: ca1bb8ee886c4b4e69be9dd653d6a52b874e1f5a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a><span data-ttu-id="7e5fa-103">Az Application Insights (előzetes verzió) több összetevőt alkalmazások figyelése</span><span class="sxs-lookup"><span data-stu-id="7e5fa-103">Monitor multi-component applications with Application Insights (preview)</span></span>

<span data-ttu-id="7e5fa-104">Figyelheti az alkalmazások, amelyek több kiszolgáló-összetevők, szerepkörök vagy szolgáltatások állnak [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e5fa-104">You can monitor apps that consist of multiple server components, roles, or services with [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="7e5fa-105">Az összetevők és a köztük lévő viszonyt is egyetlen alkalmazás térképként jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-105">The health of the components and the relationships between them are displayed on a single Application Map.</span></span> <span data-ttu-id="7e5fa-106">Egyes műveletek – több összetevő automatikus HTTP korrelációkereséssel vezethető vissza.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-106">You can trace individual operations through multiple components with automatic HTTP correlation.</span></span> <span data-ttu-id="7e5fa-107">Tároló diagnosztika integrált, és szorosan összefügg az alkalmazás telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-107">Container diagnostics can be integrated and correlated with application telemetry.</span></span> <span data-ttu-id="7e5fa-108">Az alkalmazás összetevőit egyetlen Application Insights-erőforrást használjon.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-108">Use a single Application Insights resource for all the components of your application.</span></span> 

![Több összetevőt alkalmazás-hozzárendelés](./media/app-insights-monitor-multi-role-apps/app-map.png)

<span data-ttu-id="7e5fa-110">Használjuk "összetevő" ide: a nagyméretű alkalmazások bármely működő része.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-110">We use 'component' here to mean any functioning part of a large application.</span></span> <span data-ttu-id="7e5fa-111">Például böngészők, ha egy futó Ügyfélkód állhat, a szokásos üzleti alkalmazások, vagy további web app, használó szolgáltatások pedig vissza záró szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-111">For example, a typical business application may consist of client code running in web browsers, talking to one or more web app services, which in turn use back end services.</span></span> <span data-ttu-id="7e5fa-112">Kiszolgáló-összetevők lehet üzemeltethető a helyszínen a felhőben, lehet, hogy az Azure webes és feldolgozói szerepkörök, vagy a tárolókhoz, például Docker vagy a Service Fabric futtathatnak.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-112">Server components may be hosted on-premises on in the cloud, or may be Azure web and worker roles, or may run in containers such as Docker or Service Fabric.</span></span> 

### <a name="sharing-a-single-application-insights-resource"></a><span data-ttu-id="7e5fa-113">Egyetlen Application Insights-erőforrás megosztása</span><span class="sxs-lookup"><span data-stu-id="7e5fa-113">Sharing a single Application Insights resource</span></span> 

<span data-ttu-id="7e5fa-114">A fő módszer telemetriai adatokat küldhet a minden összetevő ugyanahhoz az Application Insights-erőforráshoz az alkalmazásban, de a `cloud_RoleName` tulajdonság segítségével különböztetheti meg egymástól a szükséges összetevőket.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-114">The key technique here is to send telemetry from every component in your application to the same Application Insights resource, but use the `cloud_RoleName` property to distinguish components when necessary.</span></span> <span data-ttu-id="7e5fa-115">Az Application Insights SDK hozzáadása a `cloud_RoleName` tulajdonság a telemetria-összetevőkkel hozható létre.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-115">The Application Insights SDK adds the `cloud_RoleName` property to the telemetry components emit.</span></span> <span data-ttu-id="7e5fa-116">Például adja hozzá az SDK-t egy webhely neve, vagy a szerepkör nevét a `cloud_RoleName` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-116">For example, the SDK will add a web site name, or service role name to the `cloud_RoleName` property.</span></span> <span data-ttu-id="7e5fa-117">Ezt az értéket egy telemetryinitializer felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-117">You can override this value with a telemetryinitializer.</span></span> <span data-ttu-id="7e5fa-118">Az alkalmazás-hozzárendelés használja a `cloud_RoleName` tulajdonság a térképen összetevők azonosításához.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-118">The Application Map uses the `cloud_RoleName` property to identify the components on the map.</span></span>

<span data-ttu-id="7e5fa-119">További információ a felülírják a `cloud_RoleName` tulajdonság lásd [tulajdonságok hozzáadása: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span><span class="sxs-lookup"><span data-stu-id="7e5fa-119">For more information about how do override the `cloud_RoleName` property see [Add properties: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).</span></span>  

<span data-ttu-id="7e5fa-120">Néhány esetben ez nem lehet megfelelő, és célszerű külön erőforrásainak használatához összetevők különböző csoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-120">In some cases, this may not be appropriate, and you may prefer to use separate resources for different groups of components.</span></span> <span data-ttu-id="7e5fa-121">Például előfordulhat, hogy kell használni a különböző erőforrások felügyeleti vagy számlázási célokra.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-121">For example, you might need to use different resources for management or billing purposes.</span></span> <span data-ttu-id="7e5fa-122">Külön erőforrásokat használó azt jelenti, hogy egyetlen alkalmazás térképen; összetevőit nem jelenik meg és hogy Ön nem tudja lekérdezni a összetevői között [Analytics](app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="7e5fa-122">Using separate resources means that you don't see all the components displayed on a single Application Map; and that you can't query across components in [Analytics](app-insights-analytics.md).</span></span> <span data-ttu-id="7e5fa-123">Akkor is, a különböző erőforrások beállítása.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-123">You also have to set up the separate resources.</span></span>

<span data-ttu-id="7e5fa-124">Az adott ismeret feltételezzük, ez a dokumentum több összetevő adatokat küldeni egy Application Insights-erőforrás kívánt további részében.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-124">With that caveat, we'll assume in the rest of this document that you want to send data from multiple components to one Application Insights resource.</span></span>

## <a name="configure-multi-component-applications"></a><span data-ttu-id="7e5fa-125">Több összetevőt alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e5fa-125">Configure multi-component applications</span></span>

<span data-ttu-id="7e5fa-126">Ahhoz, hogy egy több összetevőt alkalmazás-hozzárendelés, szüksége ezen célok eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-126">To get a multi-component application map, you need to achieve these goals:</span></span>

* <span data-ttu-id="7e5fa-127">**Telepítse a legújabb előzetes** az alkalmazás minden összetevője az Application Insights-csomagot.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-127">**Install the latest pre-release** Application Insights package in each component of the application.</span></span> 
* <span data-ttu-id="7e5fa-128">**Egyetlen Application Insights-erőforrás megosztása** az alkalmazás valamennyi összetevőnél.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-128">**Share a single Application Insights resource** for all the components of your application.</span></span>
* <span data-ttu-id="7e5fa-129">**Engedélyezze az alkalmazás több szerepkör-hozzárendelés** az előzetes verziójú funkciók a panelen.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-129">**Enable Multi-role Application Map** in the Previews blade.</span></span>

<span data-ttu-id="7e5fa-130">Minden ehhez a típushoz a megfelelő módszer segítségével az alkalmazás-összetevő konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-130">Configure each component of your application using the appropriate method for its type.</span></span> <span data-ttu-id="7e5fa-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span><span class="sxs-lookup"><span data-stu-id="7e5fa-131">([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)</span></span>

### <a name="1-install-the-latest-pre-release-package"></a><span data-ttu-id="7e5fa-132">1. Telepítse a legújabb előzetes csomagot</span><span class="sxs-lookup"><span data-stu-id="7e5fa-132">1. Install the latest pre-release package</span></span>

<span data-ttu-id="7e5fa-133">Frissítés, vagy a portot Insights csomagok telepítése minden egyes kiszolgáló-összetevő a projektben.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-133">Update or install the Appication Insights packages in the project for each server component.</span></span> <span data-ttu-id="7e5fa-134">Visual Studio használata:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-134">If you're using Visual Studio:</span></span>

1. <span data-ttu-id="7e5fa-135">Kattintson jobb gombbal a projektre, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-135">Right-click a project and select **Manage NuGet Packages**.</span></span> 
2. <span data-ttu-id="7e5fa-136">Válassza ki **közé tartoznak az előzetes**.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-136">Select **Include prerelease**.</span></span>
3. <span data-ttu-id="7e5fa-137">Ha az Application Insights csomagok frissítések jelenik meg, jelölje ki őket.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-137">If Application Insights packages appear in Updates, select them.</span></span> 

    <span data-ttu-id="7e5fa-138">Ha nem tallózással keresse meg, és a megfelelő telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-138">Otherwise, browse for and install the appropriate package:</span></span>
    
    * <span data-ttu-id="7e5fa-139">Microsoft.ApplicationInsights.WindowsServer</span><span class="sxs-lookup"><span data-stu-id="7e5fa-139">Microsoft.ApplicationInsights.WindowsServer</span></span>
    * <span data-ttu-id="7e5fa-140">Microsoft.ApplicationInsights.ServiceFabric - összetevők futtatásához használt Vendég végrehajtható fájlok és a Docker-tárolók a Service Fabric-alkalmazás fut</span><span class="sxs-lookup"><span data-stu-id="7e5fa-140">Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application</span></span>
    * <span data-ttu-id="7e5fa-141">Microsoft.ApplicationInsights.ServiceFabric.Native - ServiceFabric alkalmazások megbízható szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="7e5fa-141">Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications</span></span>
    * <span data-ttu-id="7e5fa-142">Microsoft.ApplicationInsights.Kubernetes futó Kubernetes Docker-összetevők</span><span class="sxs-lookup"><span data-stu-id="7e5fa-142">Microsoft.ApplicationInsights.Kubernetes for components running in Docker on Kubernetes</span></span>

### <a name="2-share-a-single-application-insights-resource"></a><span data-ttu-id="7e5fa-143">2. Egyetlen Application Insights-erőforrás megosztása</span><span class="sxs-lookup"><span data-stu-id="7e5fa-143">2. Share a single Application Insights resource</span></span>

* <span data-ttu-id="7e5fa-144">A Visual Studióban, kattintson jobb gombbal a projektre, és válassza ki **konfigurálja az Application Insights**, vagy **Application Insights > konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-144">In Visual Studio, right-click a project and select **Configure Application Insights**, or **Application Insights > Configure**.</span></span> <span data-ttu-id="7e5fa-145">Az első projekt a varázsló segítségével hozzon létre egy Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-145">For the first project, use the wizard to create an Application Insights resource.</span></span> <span data-ttu-id="7e5fa-146">Az ezt követő projektek esetében válassza ki az ugyanazon erőforrást.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-146">For subsequent projects, select the same resource.</span></span>
* <span data-ttu-id="7e5fa-147">Ha nincs Application Insights menü, kézi konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-147">If there is no Application Insights menu, configure manually:</span></span>

   1. <span data-ttu-id="7e5fa-148">A [Azure-portálon](https://portal,azure.com), nyissa meg az Application Insights-erőforrást egy másik összetevő már létrehozott.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-148">In [Azure portal](https://portal,azure.com), open the Application Insights resource you already created for another component.</span></span>
   2. <span data-ttu-id="7e5fa-149">A panel áttekintése, nyissa meg az Essentials legördülő fülre, és másolja a **Instrumentation kulcs.**</span><span class="sxs-lookup"><span data-stu-id="7e5fa-149">In the Overview blade, open the Essentials drop-down tab, and copy the **Instrumentation Key.**</span></span>
   3. <span data-ttu-id="7e5fa-150">A projektben nyissa meg az ApplicationInsights.config, és helyezze be:`<InstrumentationKey>your copied key</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="7e5fa-150">In your project, open ApplicationInsights.config and insert: `<InstrumentationKey>your copied key</InstrumentationKey>`</span></span>

![Instrumentation kulcsának az átmásolása a .config-fájlhoz](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a><span data-ttu-id="7e5fa-152">3. Alkalmazás több szerepkör-hozzárendelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7e5fa-152">3. Enable multi-role Application Map</span></span>

<span data-ttu-id="7e5fa-153">Nyissa meg az alkalmazás erőforrást az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-153">In the Azure portal, open the resource for your application.</span></span> <span data-ttu-id="7e5fa-154">Az előzetes verziójú funkciók panelen engedélyezése *alkalmazás több szerepkör-hozzárendelés*.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-154">In the Previews blade, enable *Multi-role Application Map*.</span></span>

### <a name="4-enable-docker-metrics-optional"></a><span data-ttu-id="7e5fa-155">4. Engedélyezze a Docker-metrikák (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="7e5fa-155">4. Enable Docker metrics (Optional)</span></span> 

<span data-ttu-id="7e5fa-156">Ha egy összetevő egy Docker egy Windows Azure virtuális gépen futó fut, további metrikák gyűjtheti a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-156">If a component runs in a Docker hosted on an Azure Windows VM, you can collect additional metrics from the container.</span></span> <span data-ttu-id="7e5fa-157">Ezt a Beszúrás a [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-157">Insert this in your [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) configuration file:</span></span>

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

## <a name="use-cloudrolename-to-separate-components"></a><span data-ttu-id="7e5fa-158">Összetevők a cloud_RoleName használatával</span><span class="sxs-lookup"><span data-stu-id="7e5fa-158">Use cloud_RoleName to separate components</span></span>

<span data-ttu-id="7e5fa-159">A `cloud_RoleName` összes telemetriai adat csatolt tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-159">The `cloud_RoleName` property is attached to all telemetry.</span></span> <span data-ttu-id="7e5fa-160">A telemetriai származó összetevő - szerepkör vagy szolgáltatás - azonosítja.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-160">It identifies the component - the role or service - that originates the telemetry.</span></span> <span data-ttu-id="7e5fa-161">(Nincs azonos cloud_RoleInstance, amely elválasztja az azonos több kiszolgáló folyamatok vagy gépek párhuzamosan futó szerepkörök.)</span><span class="sxs-lookup"><span data-stu-id="7e5fa-161">(It is not the same as cloud_RoleInstance, which separates identical roles that are running in parallel on multiple server processes or machines.)</span></span>

<span data-ttu-id="7e5fa-162">A portál kiszűrhetik vagy szegmentálja a telemetriai adatok e tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-162">In the portal, you can filter or segment your telemetry using this property.</span></span> <span data-ttu-id="7e5fa-163">Ebben a példában a hibák panel úgy szűri, hogy az előtér-webkiszolgáló szolgáltatás, a CRM API háttérrendszerből hibák kiszűrése csak adatainak megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-163">In this example, the Failures blade is filtered to show just information from the front-end web service, filtering out failures from the CRM API backend:</span></span>

![Felhő szerepkör neve szegmentált metrika diagram](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a><span data-ttu-id="7e5fa-165">Összetevők közötti nyomkövetési műveletek</span><span class="sxs-lookup"><span data-stu-id="7e5fa-165">Trace operations between components</span></span>

<span data-ttu-id="7e5fa-166">Az egyik összetevő a másikra, az egyedi művelet feldolgozása közben felé indított hívások vezethető vissza.</span><span class="sxs-lookup"><span data-stu-id="7e5fa-166">You can trace from one component to another, the calls made while processing an individual operation.</span></span>


![Telemetria művelet megjelenítése](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

<span data-ttu-id="7e5fa-168">Kattintson a telemetriai adat ehhez a művelethez kapcsolódó listáját az előtér-webkiszolgáló és a háttér-API:</span><span class="sxs-lookup"><span data-stu-id="7e5fa-168">Click through to a correlated list of telemetry for this operation across the front-end web server and the back-end API:</span></span>

![Keresés összetevői között](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a><span data-ttu-id="7e5fa-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e5fa-170">Next steps</span></span>

* [<span data-ttu-id="7e5fa-171">A fejlesztői, tesztelési és éles külön telemetria</span><span class="sxs-lookup"><span data-stu-id="7e5fa-171">Separate telemetry from Development, Test, and Production</span></span>](app-insights-separate-resources.md)
