---
title: "aaaDependency az Azure Application Insights nyomkövetési |} Microsoft Docs"
description: "Az Application Insights segítségével elemezheti a használati adatokat, a rendelkezésre állást és a teljesítményt a helyszíni vagy Microsoft Azure webalkalmazásán."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: bwren
ms.openlocfilehash: e72f5465462ae8e64363cbbaa62911aff636c504
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-dependency-tracking"></a><span data-ttu-id="51670-103">Az Application Insights beállítása: függőségi nyomon követése</span><span class="sxs-lookup"><span data-stu-id="51670-103">Set up Application Insights: Dependency tracking</span></span>
<span data-ttu-id="51670-104">A *függőségi* az alkalmazás által külső összetevője.</span><span class="sxs-lookup"><span data-stu-id="51670-104">A *dependency* is an external component that is called by your app.</span></span> <span data-ttu-id="51670-105">Általában le egy HTTP, vagy egy adatbázist vagy egy fájlrendszer nevű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="51670-105">It's typically a service called using HTTP, or a database, or a file system.</span></span> <span data-ttu-id="51670-106">[Az Application Insights](app-insights-overview.md) méri, hogy az alkalmazás meddig függőségek, és milyen gyakran függőségi hívás sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="51670-106">[Application Insights](app-insights-overview.md) measures how long your application waits for dependencies and how often a dependency call fails.</span></span> <span data-ttu-id="51670-107">Vizsgálja meg az adott hívások, és összekapcsolhatja őket toorequests és kivételek.</span><span class="sxs-lookup"><span data-stu-id="51670-107">You can investigate specific calls, and relate them toorequests and exceptions.</span></span>

![mintadiagramok](./media/app-insights-asp-net-dependencies/10-intro.png)

<span data-ttu-id="51670-109">hello out-of-az-box függőségfigyelő jelenleg jelentések hívások toothese típusú függőségek:</span><span class="sxs-lookup"><span data-stu-id="51670-109">hello out-of-the-box dependency monitor currently reports calls toothese  types of dependencies:</span></span>

* <span data-ttu-id="51670-110">Kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="51670-110">Server</span></span>
  * <span data-ttu-id="51670-111">SQL Database-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="51670-111">SQL databases</span></span>
  * <span data-ttu-id="51670-112">ASP.NET webes és WCF-szolgáltatások HTTP-alapú kötések használó</span><span class="sxs-lookup"><span data-stu-id="51670-112">ASP.NET web and WCF services that use HTTP-based bindings</span></span>
  * <span data-ttu-id="51670-113">Helyi vagy távoli HTTP-hívások</span><span class="sxs-lookup"><span data-stu-id="51670-113">Local or remote HTTP calls</span></span>
  * <span data-ttu-id="51670-114">Az Azure Cosmos DB, a tábla, a blob-tároló és a várólista</span><span class="sxs-lookup"><span data-stu-id="51670-114">Azure Cosmos DB, table, blob storage, and queue</span></span>
* <span data-ttu-id="51670-115">Weblapok</span><span class="sxs-lookup"><span data-stu-id="51670-115">Web pages</span></span>
  * <span data-ttu-id="51670-116">AJAX-hívások</span><span class="sxs-lookup"><span data-stu-id="51670-116">AJAX calls</span></span>

<span data-ttu-id="51670-117">Figyelés works segítségével [bájt kód instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) kijelölt módszerek köré.</span><span class="sxs-lookup"><span data-stu-id="51670-117">Monitoring works by using [byte code instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) around selected methods.</span></span> <span data-ttu-id="51670-118">Teljesítményigény minimális.</span><span class="sxs-lookup"><span data-stu-id="51670-118">Performance overhead is minimal.</span></span>

<span data-ttu-id="51670-119">A saját SDK meghívja toomonitor más függőségek, mindkettő hello ügyfél és kiszolgáló-kódban, hello segítségével is kiírhatja [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="51670-119">You can also write your own SDK calls toomonitor other dependencies, both in hello client and server code, using hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

## <a name="set-up-dependency-monitoring"></a><span data-ttu-id="51670-120">A függőségi figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="51670-120">Set up dependency monitoring</span></span>
<span data-ttu-id="51670-121">Részleges függőségi gyűjt adatokat automatikusan hello [Application Insights SDK](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="51670-121">Partial dependency information is collected automatically by hello [Application Insights SDK](app-insights-asp-net.md).</span></span> <span data-ttu-id="51670-122">teljes körű adatok tooget, ügynököt hello megfelelő hello kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="51670-122">tooget complete data, install hello appropriate agent for hello host server.</span></span>

| <span data-ttu-id="51670-123">Platform</span><span class="sxs-lookup"><span data-stu-id="51670-123">Platform</span></span> | <span data-ttu-id="51670-124">Telepítés</span><span class="sxs-lookup"><span data-stu-id="51670-124">Install</span></span> |
| --- | --- |
| <span data-ttu-id="51670-125">IIS-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="51670-125">IIS Server</span></span> |<span data-ttu-id="51670-126">Vagy [Állapotmonitor telepítése a kiszolgálón](app-insights-monitor-performance-live-website-now.md) vagy [frissítse az alkalmazás too.NET keretrendszer 4.6-os vagy újabb](http://go.microsoft.com/fwlink/?LinkId=528259) és hello telepítése [Application Insights SDK](app-insights-asp-net.md) az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="51670-126">Either [install Status Monitor on your server](app-insights-monitor-performance-live-website-now.md) or [Upgrade your application too.NET framework 4.6 or later](http://go.microsoft.com/fwlink/?LinkId=528259) and install hello [Application Insights SDK](app-insights-asp-net.md)  in your app.</span></span> |
| <span data-ttu-id="51670-127">Azure Web App</span><span class="sxs-lookup"><span data-stu-id="51670-127">Azure Web App</span></span> |<span data-ttu-id="51670-128">A webes alkalmazás Vezérlőpult [hello nyissa meg az Application Insights panel a webes alkalmazás Vezérlőpult](app-insights-azure-web-apps.md) , és válassza a telepítés, ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="51670-128">In your web app control panel, [open hello Application Insights blade in your web app control panel](app-insights-azure-web-apps.md) and choose Install if prompted.</span></span> |
| <span data-ttu-id="51670-129">Azure-Felhőszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="51670-129">Azure Cloud Service</span></span> |<span data-ttu-id="51670-130">[Használjon indítási tevékenységhez](app-insights-cloudservices.md) vagy [telepítse a .NET-keretrendszer 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="51670-130">[Use startup task](app-insights-cloudservices.md) or [Install .NET framework 4.6+](../cloud-services/cloud-services-dotnet-install-dotnet.md)</span></span> |

## <a name="where-toofind-dependency-data"></a><span data-ttu-id="51670-131">Ha toofind függőségi adatokat</span><span class="sxs-lookup"><span data-stu-id="51670-131">Where toofind dependency data</span></span>
* <span data-ttu-id="51670-132">[Alkalmazás-hozzárendelés](#application-map) visualizes az alkalmazás és a szomszédos összetevők közötti függőségek.</span><span class="sxs-lookup"><span data-stu-id="51670-132">[Application Map](#application-map) visualizes dependencies between your app and neighbouring components.</span></span>
* <span data-ttu-id="51670-133">[Teljesítmény, a böngésző és a hiba paneleken](#performance-and-blades) kiszolgáló függőségi adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="51670-133">[Performance, browser, and failure blades](#performance-and-blades) show server dependency data.</span></span>
* <span data-ttu-id="51670-134">[Böngészők panel](#ajax-calls) jeleníti meg a felhasználók böngészőjének az AJAX-hívások.</span><span class="sxs-lookup"><span data-stu-id="51670-134">[Browsers blade](#ajax-calls) shows AJAX calls from your users' browsers.</span></span>
* <span data-ttu-id="51670-135">[A lassú és a sikertelen kérelmek átkattintással](#diagnose-slow-requests) toocheck azok függőségi hívás.</span><span class="sxs-lookup"><span data-stu-id="51670-135">[Click through from slow or failed requests](#diagnose-slow-requests) toocheck their dependency calls.</span></span>
* <span data-ttu-id="51670-136">[Elemzés](#analytics) használt tooquery függőségi adatokat is lehet.</span><span class="sxs-lookup"><span data-stu-id="51670-136">[Analytics](#analytics) can be used tooquery dependency data.</span></span>

## <a name="application-map"></a><span data-ttu-id="51670-137">Alkalmazástérkép</span><span class="sxs-lookup"><span data-stu-id="51670-137">Application Map</span></span>
<span data-ttu-id="51670-138">Alkalmazás-hozzárendelés úgy működik, mint a visual támogatás toodiscovering függőségek hello az alkalmazások összetevői között.</span><span class="sxs-lookup"><span data-stu-id="51670-138">Application Map acts as a visual aid toodiscovering dependencies between hello components of your application.</span></span> <span data-ttu-id="51670-139">Automatikusan létrejön a hello telemetriai adatokból az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="51670-139">It is automatically generated from hello telemetry from your app.</span></span> <span data-ttu-id="51670-140">Ez a példa bemutatja AJAX-hívások hello böngésző parancsfájlok és a REST-hívások hello kiszolgálóról alkalmazásszolgáltatások tootwo külső.</span><span class="sxs-lookup"><span data-stu-id="51670-140">This example shows AJAX calls from hello browser scripts and REST calls from hello server app tootwo external services.</span></span>

![Alkalmazástérkép](./media/app-insights-asp-net-dependencies/08.png)

* <span data-ttu-id="51670-142">**Keresse meg a hello mezőkben** toorelevant függőségi és más típusú diagramokkal.</span><span class="sxs-lookup"><span data-stu-id="51670-142">**Navigate from hello boxes** toorelevant dependency and other charts.</span></span>
* <span data-ttu-id="51670-143">**PIN-kód hello térkép** toohello [irányítópult](app-insights-dashboards.md), ahol azt teljesen működőképes lesz.</span><span class="sxs-lookup"><span data-stu-id="51670-143">**Pin hello map** toohello [dashboard](app-insights-dashboards.md), where it will be fully functional.</span></span>

<span data-ttu-id="51670-144">[További információk](app-insights-app-map.md).</span><span class="sxs-lookup"><span data-stu-id="51670-144">[Learn more](app-insights-app-map.md).</span></span>

## <a name="performance-and-failure-blades"></a><span data-ttu-id="51670-145">Teljesítmény és hibák paneleken</span><span class="sxs-lookup"><span data-stu-id="51670-145">Performance and failure blades</span></span>
<span data-ttu-id="51670-146">hello teljesítmény panelen látható hello hello server alkalmazás függőségi hívások időtartama.</span><span class="sxs-lookup"><span data-stu-id="51670-146">hello performance blade shows hello duration of dependency calls made by hello server app.</span></span> <span data-ttu-id="51670-147">Létrejön egy összegző diagram és hívása szegmentált tábla.</span><span class="sxs-lookup"><span data-stu-id="51670-147">There's a summary chart and a table segmented by call.</span></span>

![Teljesítmény panel függőségi diagramjain](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

<span data-ttu-id="51670-149">Kattintson a hello összefoglaló diagramok vagy hello tábla elemek toosearch nyers előfordulását hívásokat.</span><span class="sxs-lookup"><span data-stu-id="51670-149">Click through hello summary charts or hello table items toosearch raw occurrences of these calls.</span></span>

![Függőségi hívás példányok](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

<span data-ttu-id="51670-151">**Hiba száma** látható a hello **hibák** panelen.</span><span class="sxs-lookup"><span data-stu-id="51670-151">**Failure counts** are shown on hello **Failures** blade.</span></span> <span data-ttu-id="51670-152">Hiba az összes visszatérési kódot, amely nincs hello tartomány 200-399, vagy ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="51670-152">A failure is any return code that is not in hello range 200-399, or unknown.</span></span>

> [!NOTE]
> <span data-ttu-id="51670-153">**100 %-os hibák?**</span><span class="sxs-lookup"><span data-stu-id="51670-153">**100% failures?**</span></span> <span data-ttu-id="51670-154">-Ez valószínűleg azt jelzi, hogy csak kap a részleges függőségi adatokat.</span><span class="sxs-lookup"><span data-stu-id="51670-154">- This probably indicates that you are only getting partial dependency data.</span></span> <span data-ttu-id="51670-155">Túl kell[állítsa be a függőségi figyelő megfelelő tooyour platform](#set-up-dependency-monitoring).</span><span class="sxs-lookup"><span data-stu-id="51670-155">You need too[set up dependency monitoring appropriate tooyour platform](#set-up-dependency-monitoring).</span></span>
>
>

## <a name="ajax-calls"></a><span data-ttu-id="51670-156">AJAX-hívások</span><span class="sxs-lookup"><span data-stu-id="51670-156">AJAX Calls</span></span>
<span data-ttu-id="51670-157">hello böngészők panel hello időtartam látható és AJAX sikertelenségének arányát hívásait [JavaScript a weblapok](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="51670-157">hello Browsers blade shows hello duration and failure rate of AJAX calls from [JavaScript in your web pages](app-insights-javascript.md).</span></span> <span data-ttu-id="51670-158">Szerint függőségeinek megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="51670-158">They are shown as Dependencies.</span></span>

## <span data-ttu-id="51670-159"><a name="diagnosis"></a>Lassú kérelmek diagnosztizálására</span><span class="sxs-lookup"><span data-stu-id="51670-159"><a name="diagnosis"></a> Diagnose slow requests</span></span>
<span data-ttu-id="51670-160">Minden egyes kérelem esemény társított hello függőségi hívások esetében, kivételeket és eseményeket, amelyek az alkalmazás feldolgozása közben a rendszer nyomon hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="51670-160">Each request event is associated with hello dependency calls, exceptions and other events that are tracked while your app is processing hello request.</span></span> <span data-ttu-id="51670-161">Így bizonyos kérelmek rosszul hajtja végre, ha azt megtudhatja, hogy van-e tooslow válaszainak függőség miatt.</span><span class="sxs-lookup"><span data-stu-id="51670-161">So if some requests are performing badly, you can find out whether it's due tooslow responses from a dependency.</span></span>

<span data-ttu-id="51670-162">Bemutatjuk, például, hogy keresztül.</span><span class="sxs-lookup"><span data-stu-id="51670-162">Let's walk through an example of that.</span></span>

### <a name="tracing-from-requests-toodependencies"></a><span data-ttu-id="51670-163">Kérelmek toodependencies nyomkövetés</span><span class="sxs-lookup"><span data-stu-id="51670-163">Tracing from requests toodependencies</span></span>
<span data-ttu-id="51670-164">Hello teljesítmény panel megnyitásához, és nézze meg a kérelmek hello rács:</span><span class="sxs-lookup"><span data-stu-id="51670-164">Open hello Performance blade, and look at hello grid of requests:</span></span>

![Lista átlagok és száma](./media/app-insights-asp-net-dependencies/02-reqs.png)

<span data-ttu-id="51670-166">hello felső egy nagyon hosszú tart.</span><span class="sxs-lookup"><span data-stu-id="51670-166">hello top one is taking very long.</span></span> <span data-ttu-id="51670-167">Nézzük meg, ha azt is megtudhatja, ahol hello idő telik.</span><span class="sxs-lookup"><span data-stu-id="51670-167">Let's see if we can find out where hello time is spent.</span></span>

<span data-ttu-id="51670-168">Kattintson az adott sor toosee egyes események:</span><span class="sxs-lookup"><span data-stu-id="51670-168">Click that row toosee individual request events:</span></span>

![A kérelem események listája](./media/app-insights-asp-net-dependencies/03-instances.png)

<span data-ttu-id="51670-170">Kattintson a hosszan futó példány tooinspect tovább, és görgessen lefelé toohello távoli függőségi hívások kapcsolódó toothis kérelem:</span><span class="sxs-lookup"><span data-stu-id="51670-170">Click any long-running instance tooinspect it further, and scroll down toohello remote dependency calls related toothis request:</span></span>

![Hívások tooRemote függőségek található, és keresse meg a szokatlan időtartama](./media/app-insights-asp-net-dependencies/04-dependencies.png)

<span data-ttu-id="51670-172">Úgy tűnik a legtöbb hello ideje karbantartási ehhez a kérelemhez telt egy hívás tooa helyi szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="51670-172">It looks like most of hello time servicing this request was spent in a call tooa local service.</span></span>

<span data-ttu-id="51670-173">Válassza ki, hogy sor tooget további információt:</span><span class="sxs-lookup"><span data-stu-id="51670-173">Select that row tooget more information:</span></span>

![Kattintson az adott távoli függőség tooidentify hello hibát okozó keresztül](./media/app-insights-asp-net-dependencies/05-detail.png)

<span data-ttu-id="51670-175">A jelek szerint ez az hello probléma esetén.</span><span class="sxs-lookup"><span data-stu-id="51670-175">Looks like this is where hello problem is.</span></span> <span data-ttu-id="51670-176">Azt is pinpointed hello probléma, így most csak azt kell toofind, miért adott hívás tart sokáig.</span><span class="sxs-lookup"><span data-stu-id="51670-176">We've pinpointed hello problem, so now we just need toofind out why that call is taking so long.</span></span>

### <a name="request-timeline"></a><span data-ttu-id="51670-177">Az idősor kérése</span><span class="sxs-lookup"><span data-stu-id="51670-177">Request timeline</span></span>
<span data-ttu-id="51670-178">Más esetben nem függőségi hívás különösen hosszú van.</span><span class="sxs-lookup"><span data-stu-id="51670-178">In a different case, there is no dependency call that is particularly long.</span></span> <span data-ttu-id="51670-179">De toohello idősor nézetének megnyitására váltásával láthatja Ha hello késedelem a saját belső feldolgozása során:</span><span class="sxs-lookup"><span data-stu-id="51670-179">But by switching toohello timeline view, we can see where hello delay occurred in our internal processing:</span></span>

![Hívások tooRemote függőségek található, és keresse meg a szokatlan időtartama](./media/app-insights-asp-net-dependencies/04-1.png)

<span data-ttu-id="51670-181">Úgy tűnik, nagy hiány toobe hello első függőségi hívás után, azt kell kinéznie: a kód toosee, ezért ez azt jelenti.</span><span class="sxs-lookup"><span data-stu-id="51670-181">There seems toobe a big gap after hello first dependency call, so we should look at our code toosee why that is.</span></span>

### <a name="profile-your-live-site"></a><span data-ttu-id="51670-182">Élő webhelyét profilját</span><span class="sxs-lookup"><span data-stu-id="51670-182">Profile your live site</span></span>

<span data-ttu-id="51670-183">Nem tudja, hol hello idő kerül? Hello [Application Insights Profilkészítő](app-insights-profiler.md) nyomkövetések HTTP meghívja a tooyour élő webhelyet, és megjeleníti a funkciókról a kódban hello leghosszabb időt vett.</span><span class="sxs-lookup"><span data-stu-id="51670-183">No idea where hello time goes? hello [Application Insights profiler](app-insights-profiler.md) traces HTTP calls tooyour live site and shows you which functions in your code took hello longest time.</span></span>

## <a name="failed-requests"></a><span data-ttu-id="51670-184">Sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="51670-184">Failed requests</span></span>
<span data-ttu-id="51670-185">Lehet, hogy a sikertelen kérelmek is sikertelen hívás toodependencies társítva.</span><span class="sxs-lookup"><span data-stu-id="51670-185">Failed requests might also be associated with failed calls toodependencies.</span></span> <span data-ttu-id="51670-186">Ebben az esetben azt átkattintással is tootrack hello probléma le.</span><span class="sxs-lookup"><span data-stu-id="51670-186">Again, we can click through tootrack down hello problem.</span></span>

![Kattintson a hello sikertelen kérelmek diagram](./media/app-insights-asp-net-dependencies/06-fail.png)

<span data-ttu-id="51670-188">Haladjon végig a sikertelen kérelmek tooan előfordulása, és tekintse meg a kapcsolódó események.</span><span class="sxs-lookup"><span data-stu-id="51670-188">Click through tooan occurrence of a failed request, and look at its associated events.</span></span>

![Kattintson egy kérelemtípus, kattintson a hello példány tooget tooa különböző ábrázolása hello példányt, kattintson rá tooget kivétel részletes adatai.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a><span data-ttu-id="51670-190">Elemzés</span><span class="sxs-lookup"><span data-stu-id="51670-190">Analytics</span></span>
<span data-ttu-id="51670-191">Nyomon követheti a hello függőségek [Log Analytics lekérdezési nyelv](https://docs.loganalytics.io/).</span><span class="sxs-lookup"><span data-stu-id="51670-191">You can track dependencies in hello [Log Analytics query language](https://docs.loganalytics.io/).</span></span> <span data-ttu-id="51670-192">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="51670-192">Here are some examples.</span></span>

* <span data-ttu-id="51670-193">Keresse meg a sikertelen függőségi hívások esetében:</span><span class="sxs-lookup"><span data-stu-id="51670-193">Find any failed dependency calls:</span></span>

```

    dependencies | where success != "True" | take 10
```

* <span data-ttu-id="51670-194">AJAX-hívások keresése:</span><span class="sxs-lookup"><span data-stu-id="51670-194">Find AJAX calls:</span></span>

```

    dependencies | where client_Type == "Browser" | take 10
```

* <span data-ttu-id="51670-195">Keresse meg a kérelmek társított függőségi hívások esetében:</span><span class="sxs-lookup"><span data-stu-id="51670-195">Find dependency calls associated with requests:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* <span data-ttu-id="51670-196">Lapmegtekintések társított AJAX-hívások keresése:</span><span class="sxs-lookup"><span data-stu-id="51670-196">Find AJAX calls associated with page views:</span></span>

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a><span data-ttu-id="51670-197">Egyéni függőségi nyomon követése</span><span class="sxs-lookup"><span data-stu-id="51670-197">Custom dependency tracking</span></span>
<span data-ttu-id="51670-198">hello szabványos függőségi követése modul automatikusan észleli a külső függőségei, például adatbázisok és a REST API-k.</span><span class="sxs-lookup"><span data-stu-id="51670-198">hello standard dependency-tracking module automatically discovers external dependencies such as databases and REST APIs.</span></span> <span data-ttu-id="51670-199">De érdemes lehet néhány további összetevők toobe kezelni hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="51670-199">But you might want some additional components toobe treated in hello same way.</span></span>

<span data-ttu-id="51670-200">Írhat kódot, amely a függőségi adatokat küldi hello segítségével azonos [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) hello szabványos modulok által használt.</span><span class="sxs-lookup"><span data-stu-id="51670-200">You can write code that sends dependency information, using hello same [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) that is used by hello standard modules.</span></span>

<span data-ttu-id="51670-201">Például ha használjon a kód egy szerelvény, hogy saját maga nem írhatók, minden hello hívások tooit sikerült idő, milyen hozzájárulás teszi tooyour választ ki toofind alkalommal fordult elő.</span><span class="sxs-lookup"><span data-stu-id="51670-201">For example, if you build your code with an assembly that you didn't write yourself, you could time all hello calls tooit, toofind out what contribution it makes tooyour response times.</span></span> <span data-ttu-id="51670-202">toohave Application Insights hello függőségi diagramjain jelennek meg ezek az adatok küldése használatával `TrackDependency`.</span><span class="sxs-lookup"><span data-stu-id="51670-202">toohave this data displayed in hello dependency charts in Application Insights, send it using `TrackDependency`.</span></span>

```C#

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
            }
```

<span data-ttu-id="51670-203">Hello szabványos függőségi követési modul ki tooswitch, szüntesse meg a hello hivatkozás tooDependencyTrackingTelemetryModule [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span><span class="sxs-lookup"><span data-stu-id="51670-203">If you want tooswitch off hello standard dependency tracking module, remove hello reference tooDependencyTrackingTelemetryModule in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="51670-204">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="51670-204">Troubleshooting</span></span>
<span data-ttu-id="51670-205">*Függőségi sikerességét jelző mindig igaz vagy hamis értéket jeleníti meg.*</span><span class="sxs-lookup"><span data-stu-id="51670-205">*Dependency success flag always shows either true or false.*</span></span>

<span data-ttu-id="51670-206">*Nem jelenik meg a teljes SQL-lekérdezésben.*</span><span class="sxs-lookup"><span data-stu-id="51670-206">*SQL query not shown in full.*</span></span>

* <span data-ttu-id="51670-207">Toohello hello SDK legújabb verziójára frissítse.</span><span class="sxs-lookup"><span data-stu-id="51670-207">Upgrade toohello latest version of hello SDK.</span></span> <span data-ttu-id="51670-208">Ha a .NET verziószáma kisebb, mint 4.6:</span><span class="sxs-lookup"><span data-stu-id="51670-208">If your .NET version is less than 4.6:</span></span>
  * <span data-ttu-id="51670-209">IIS-állomás: telepítése [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) hello gazdagép-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="51670-209">IIS host: Install [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on hello host servers.</span></span>
  * <span data-ttu-id="51670-210">Azure-webalkalmazásban: Nyissa meg az Application Insights hello web app Vezérlőpult fülre, és telepítse az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="51670-210">Azure web app: Open Application Insights tab in hello web app control panel, and install Application Insights.</span></span>

## <a name="video"></a><span data-ttu-id="51670-211">Videó</span><span class="sxs-lookup"><span data-stu-id="51670-211">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="51670-212">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51670-212">Next steps</span></span>
* [<span data-ttu-id="51670-213">Kivételek</span><span class="sxs-lookup"><span data-stu-id="51670-213">Exceptions</span></span>](app-insights-asp-net-exceptions.md)
* [<span data-ttu-id="51670-214">Felhasználók és Lapadatok</span><span class="sxs-lookup"><span data-stu-id="51670-214">User & page data</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="51670-215">Rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="51670-215">Availability</span></span>](app-insights-monitor-web-app-availability.md)
