---
title: "aaaMonitor Node.js szolgáltatások az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "Teljesítmény figyelése és problémák diagnosztizálása a Node.js szolgáltatásokban az Application Insights segítségével."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: bwren
ms.openlocfilehash: 0a7e66990cd4d3a2fcaf3fa779adb336c861f8ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a><span data-ttu-id="3ef47-103">A Node.js szolgáltatások és appok figyelése az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="3ef47-103">Monitor your Node.js services and apps with Application Insights</span></span>

<span data-ttu-id="3ef47-104">[Az Azure Application Insights](app-insights-overview.md) figyeli a háttér-szolgáltatások és az összetevők toohelp központi telepítésük után meg [felderíteni, és gyorsan a teljesítmény- és egyéb problémák diagnosztizálásához](app-insights-detect-triage-diagnose.md).</span><span class="sxs-lookup"><span data-stu-id="3ef47-104">[Azure Application Insights](app-insights-overview.md) monitors your backend services and components after you deploy them toohelp you [discover and rapidly diagnose performance and other issues](app-insights-detect-triage-diagnose.md).</span></span> <span data-ttu-id="3ef47-105">Bármilyen Node.js szolgáltatáshoz használható, azok üzemeltetési helyétől: az adatközpontban, Azure-beli virtuális gépen vagy Web Apps-on, vagy akár nyilvános felhőkön.</span><span class="sxs-lookup"><span data-stu-id="3ef47-105">Use it for Node.js services hosted anywhere: your datacenter, Azure VMs and Web Apps, and even other public clouds.</span></span>

<span data-ttu-id="3ef47-106">tooreceive, tárolja, és áttekintheti a figyelési adatokat, kövesse az utasításokat tooinclude a kódban az ügynök a következő hello és állítsa be a megfelelő Application Insights-erőforrást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3ef47-106">tooreceive, store, and explore your monitoring data, follow hello following instructions tooinclude an agent in your code and set up a corresponding Application Insights resource in Azure.</span></span> <span data-ttu-id="3ef47-107">hello ügynök küld adatokat toothat erőforrás további elemzés és kutatási funkciójával.</span><span class="sxs-lookup"><span data-stu-id="3ef47-107">hello agent sends data toothat resource for further analysis and exploration.</span></span>

<span data-ttu-id="3ef47-108">hello Node.js ügynök automatikusan képes figyelni a bejövő és kimenő HTTP kérelmeket, számos rendszer metrikákat és kivételeket.</span><span class="sxs-lookup"><span data-stu-id="3ef47-108">hello Node.js agent can automatically monitor incoming and outgoing HTTP requests, several system metrics, and exceptions.</span></span> <span data-ttu-id="3ef47-109">A 0.20 verziótó kezdve néhány gyakori külső csomag figyelésére is képes, mint például a `mongodb`, a `mysql` és a `redis`.</span><span class="sxs-lookup"><span data-stu-id="3ef47-109">Beginning in v0.20, it can also monitor some common third-party packages such as `mongodb`, `mysql`, and `redis`.</span></span> <span data-ttu-id="3ef47-110">Összes esemény kapcsolódó tooan bejövő HTTP-kérelem gyorsabban hibaelhárítási közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="3ef47-110">All events related tooan incoming HTTP request are correlated for faster troubleshooting.</span></span>

<span data-ttu-id="3ef47-111">Az alkalmazás további aspektusait figyelheti és a rendszer által manuálisan tagolása azokat a későbbiekben olvashat hello-ügynök API használatával.</span><span class="sxs-lookup"><span data-stu-id="3ef47-111">You can monitor more aspects of your app and system by manually instrumenting them using hello agent API described later.</span></span>

![Példa teljesítményfigyelő diagramokra](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a><span data-ttu-id="3ef47-113">Első lépések</span><span class="sxs-lookup"><span data-stu-id="3ef47-113">Getting Started</span></span>

<span data-ttu-id="3ef47-114">Tekintsük át, hogyan állíthatja be a felügyeletet egy apphoz vagy egy szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="3ef47-114">Let's step through setting up monitoring for an app or service.</span></span>

### <span data-ttu-id="3ef47-115"><a name="resource"></a> Egy App Insights-erőforrás beállítása</span><span class="sxs-lookup"><span data-stu-id="3ef47-115"><a name="resource"></a> Set up an App Insights resource</span></span>

<span data-ttu-id="3ef47-116">**Mielőtt hozzákezd**, győződjön meg róla, hogy rendelkezik Azure-előfizetéssel vagy [igényeljen ingyenesen egy újat][azure-free-offer].</span><span class="sxs-lookup"><span data-stu-id="3ef47-116">**Before you start**, make sure you have an Azure subscription or [get a new one for free][azure-free-offer].</span></span> <span data-ttu-id="3ef47-117">Ha a szervezete rendelkezik Azure-előfizetéssel, kövesse a rendszergazda [ezeket az utasításokat] [ add-aad-user] tooadd meg tooit.</span><span class="sxs-lookup"><span data-stu-id="3ef47-117">If your organization already has an Azure subscription, an administrator can follow [these instructions][add-aad-user] tooadd you tooit.</span></span>

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

<span data-ttu-id="3ef47-118">Most jelentkezzen be toohello [Azure-portálon] [ portal] és az Application Insights-erőforrás létrehozása hello következő ismertetett módon – kattintson az "Új" > "Fejlesztői eszközök" > "Application Insights".</span><span class="sxs-lookup"><span data-stu-id="3ef47-118">Now log in toohello [Azure portal][portal] and create an Application Insights resource as illustrated in hello following - click "New" > "Developer tools" > "Application Insights".</span></span> <span data-ttu-id="3ef47-119">hello erőforrás végpont a telemetriai adatokat, ezek az adatok, jelentések és irányítópultok, a szabály riasztás konfigurálásában és még mentett-tároló tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3ef47-119">hello resource includes an endpoint for receiving telemetry data, storage for this data, saved reports and dashboards, rule and alert configuration, and more.</span></span>

![App Insights-erőforrás létrehozása](./media/app-insights-nodejs/03-new_appinsights_resource.png)

<span data-ttu-id="3ef47-121">Hello erőforrás létrehozása lapon válassza ki a hello alkalmazás típusa legördülő "Node.js-alkalmazás".</span><span class="sxs-lookup"><span data-stu-id="3ef47-121">On hello resource creation page, choose "Node.js Application" from hello application type drop-down.</span></span> <span data-ttu-id="3ef47-122">hello alkalmazás típusa határozza meg a hello alapértelmezett készletét irányítópultokat és jelentéseket hozott létre.</span><span class="sxs-lookup"><span data-stu-id="3ef47-122">hello app type determines hello default set of dashboards and reports created for you.</span></span> <span data-ttu-id="3ef47-123">Aggodalomra azonban semmi ok, bármely App Insights-erőforrás képes bármilyen nyelvből és platformból adatot gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="3ef47-123">Don't worry though, any App Insights resource can in fact collect data from any language and platform.</span></span>

![Új App Insights-erőforrás űrlap](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <span data-ttu-id="3ef47-125"><a name="agent"></a>Hello Node.js ügynök beállítása</span><span class="sxs-lookup"><span data-stu-id="3ef47-125"><a name="agent"></a> Set up hello Node.js agent</span></span>

<span data-ttu-id="3ef47-126">Most már idő tooinclude hello ügynök az alkalmazásban, adatokat gyűjthessen.</span><span class="sxs-lookup"><span data-stu-id="3ef47-126">Now it's time tooinclude hello agent in your app so it can gather data.</span></span>
<span data-ttu-id="3ef47-127">Indítsa el az erőforrás Instrumentation kulcsának (továbbiakban említett tooas a `ikey`) hello portálról alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="3ef47-127">Start by copying your resource's Instrumentation Key (hereinafter referred tooas your `ikey`) from hello portal as shown below.</span></span> <span data-ttu-id="3ef47-128">hello App Insights rendszer e kulcs toomap adatok tooyour Azure-erőforrás használ, így a toospecify van szüksége, egy környezeti változó vagy a hello ügynök toouse kódját.</span><span class="sxs-lookup"><span data-stu-id="3ef47-128">hello App Insights system uses this key toomap data tooyour Azure resource so you need toospecify it in an environment variable or your code for hello agent toouse.</span></span>  

![Rendszerállapotkulcs másolása](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

<span data-ttu-id="3ef47-130">Ezután adja hozzá a package.json keresztül hello Node.js ügynök könyvtár tooyour alkalmazás függőségeit.</span><span class="sxs-lookup"><span data-stu-id="3ef47-130">Next, add hello Node.js agent library tooyour app's dependencies via package.json.</span></span> <span data-ttu-id="3ef47-131">Az alkalmazás hello gyökérmappájában futtassa:</span><span class="sxs-lookup"><span data-stu-id="3ef47-131">From hello root folder of your app, run:</span></span>

```bash
npm install applicationinsights --save
```

<span data-ttu-id="3ef47-132">Most tooexplicitly hello-könyvtár betöltése a kódban.</span><span class="sxs-lookup"><span data-stu-id="3ef47-132">Now you need tooexplicitly load hello library in your code.</span></span> <span data-ttu-id="3ef47-133">Hello ügynök esetében instrumentation sok más szalagtárak be, mert be kell tölteni azt más azelőtt a lehető leghamarabb `require` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="3ef47-133">Because hello agent injects instrumentation into many other libraries, you should load it as early as possible, even before other `require` statements.</span></span> <span data-ttu-id="3ef47-134">tooget elindítva, az első .js fájl hello tetején adja hozzá:</span><span class="sxs-lookup"><span data-stu-id="3ef47-134">tooget started, at hello top of your first .js file add:</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

<span data-ttu-id="3ef47-135">Hello `setup` metódus hello instrumentation kulcs (és így Azure-erőforrás) konfigurálja az összes nyomon követett elemek alapértelmezés szerint használt toobe.</span><span class="sxs-lookup"><span data-stu-id="3ef47-135">hello `setup` method configures hello instrumentation key (and thus Azure resource) toobe used by default for all tracked items.</span></span> <span data-ttu-id="3ef47-136">Hívás `start` után konfigurációs végzett toobegin összegyűjtése és a telemetriai adatok küldését.</span><span class="sxs-lookup"><span data-stu-id="3ef47-136">Call `start` after configuration is finished toobegin gathering and sending telemetry data.</span></span>

<span data-ttu-id="3ef47-137">Is megadhatja egy ikey keresztül hello környezeti változó appinsights által biztosított\_ahelyett, hogy át azt manuálisan túl INSTRUMENTATIONKEY `setup()` vagy `getClient()`.</span><span class="sxs-lookup"><span data-stu-id="3ef47-137">You can also provide an ikey via hello environment variable APPINSIGHTS\_INSTRUMENTATIONKEY instead of passing it manually too `setup()` or `getClient()`.</span></span> <span data-ttu-id="3ef47-138">Ez az eljárás lehetővé teszi a véglegesített forráskód kívül ikeys és toospecify különböző ikeys különböző környezetekben.</span><span class="sxs-lookup"><span data-stu-id="3ef47-138">This practice lets you keep ikeys out of committed source code and toospecify different ikeys for different environments.</span></span>

<span data-ttu-id="3ef47-139">A további konfigurációs lehetőségek az alábbi dokumentációban érhetőek el.</span><span class="sxs-lookup"><span data-stu-id="3ef47-139">Additional configuration options are documented below.</span></span>

<span data-ttu-id="3ef47-140">Megpróbálhatja hello ügynök úgy, hogy hello instrumentation tooany kulcs nem üres karakterláncnak telemetriai adatok elküldése nélkül.</span><span class="sxs-lookup"><span data-stu-id="3ef47-140">You can try hello agent without sending telemetry by setting hello instrumentation key tooany non-empty string.</span></span>

### <span data-ttu-id="3ef47-141"><a name="monitor"></a> Figyelje alkalmazását</span><span class="sxs-lookup"><span data-stu-id="3ef47-141"><a name="monitor"></a> Monitor your app</span></span>

<span data-ttu-id="3ef47-142">hello ügynök automatikusan összegyűjti a telemetriai adatainak hello Node.js futásidejű és néhány gyakori külső modulok.</span><span class="sxs-lookup"><span data-stu-id="3ef47-142">hello agent automatically gathers telemetry about hello Node.js runtime and some common third-party modules.</span></span> <span data-ttu-id="3ef47-143">Az alkalmazás most már toogenerate használja az adatok egy részét.</span><span class="sxs-lookup"><span data-stu-id="3ef47-143">Use your application now toogenerate some of this data.</span></span>

<span data-ttu-id="3ef47-144">Ezt követően a hello [Azure-portálon] [ portal] keresse meg a korábban létrehozott toohello Application Insights-erőforrást, és keresse meg az első néhány adatpont hello áttekintése idővonalon, mint a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-144">Then, in hello [Azure portal][portal] browse toohello Application Insights resource you created earlier and look for your first few data points in hello Overview timeline, as in hello following image.</span></span> <span data-ttu-id="3ef47-145">Kattintson a további részletekért hello diagramokat.</span><span class="sxs-lookup"><span data-stu-id="3ef47-145">Click through hello charts for more details.</span></span>

![Első adatpontok](./media/app-insights-nodejs/12-first-perf.png)

<span data-ttu-id="3ef47-147">Kattintson a hello alkalmazás térkép gomb tooview hello topológia felderítése az alkalmazások, mint a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="3ef47-147">Click hello Application map button tooview hello topology discovered for your app, as in hello following image.</span></span> <span data-ttu-id="3ef47-148">Kattintson a további részletekért hello leképezés összetevők.</span><span class="sxs-lookup"><span data-stu-id="3ef47-148">Click through components in hello map for more details.</span></span>

![Egyszerű apphozzárendelés](./media/app-insights-nodejs/06-appinsights_appmap.png)

<span data-ttu-id="3ef47-150">További tudnivalók az alkalmazása és elhárítása hello segítségével más hello "Vizsgálat" szakasz alatt elérhető nézeteket.</span><span class="sxs-lookup"><span data-stu-id="3ef47-150">Learn more about your app and troubleshoot problems using hello other views available under hello "Investigate" section.</span></span>

![Vizsgálat szakasz](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a><span data-ttu-id="3ef47-152">Nincs adat?</span><span class="sxs-lookup"><span data-stu-id="3ef47-152">No data?</span></span>

<span data-ttu-id="3ef47-153">Mert hello ügynök kötegek adatainak beküldése lehet késleltetést elemek hello portálon megjelenése előtt.</span><span class="sxs-lookup"><span data-stu-id="3ef47-153">Because hello agent batches data for submission there may be a delay before items are displayed in hello portal.</span></span> <span data-ttu-id="3ef47-154">Ha nem látja az erőforrás adatainak próbáljon ki a következő javítások hello:</span><span class="sxs-lookup"><span data-stu-id="3ef47-154">If you don't see data in your resource try some of hello following fixes:</span></span>

* <span data-ttu-id="3ef47-155">Hello alkalmazásával néhány további; További műveletek toogenerate tegye meg a további telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="3ef47-155">Use hello application some more; take more actions toogenerate more telemetry.</span></span>
* <span data-ttu-id="3ef47-156">Kattintson a **frissítése** hello portál erőforrás nézetben.</span><span class="sxs-lookup"><span data-stu-id="3ef47-156">Click **Refresh** in hello portal resource view.</span></span> <span data-ttu-id="3ef47-157">Diagram automatikus frissítése maguk rendszeres időközönként, de azonnal frissítése arra kényszeríti a toohappen.</span><span class="sxs-lookup"><span data-stu-id="3ef47-157">Charts automatically refresh themselves periodically but refreshing forces this toohappen immediately.</span></span>
* <span data-ttu-id="3ef47-158">Ellenőrizze, hogy a [szükséges kimenő portok](app-insights-ip-addresses.md) nyitva vannak-e.</span><span class="sxs-lookup"><span data-stu-id="3ef47-158">Verify that [needed outgoing ports](app-insights-ip-addresses.md) are open.</span></span>
* <span data-ttu-id="3ef47-159">Nyissa meg hello [keresési](app-insights-diagnostic-search.md) csempén, és keresse meg az egyes események.</span><span class="sxs-lookup"><span data-stu-id="3ef47-159">Open hello [Search](app-insights-diagnostic-search.md) tile and look for individual events.</span></span>
* <span data-ttu-id="3ef47-160">Ellenőrizze a hello [gyakran ismételt kérdések][].</span><span class="sxs-lookup"><span data-stu-id="3ef47-160">Check hello [FAQ][].</span></span>


## <a name="agent-configuration"></a><span data-ttu-id="3ef47-161">Ügynökkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="3ef47-161">Agent Configuration</span></span>

<span data-ttu-id="3ef47-162">Az alábbiakban hello ügynök konfigurációs módszerek és az alapértelmezett értékekre.</span><span class="sxs-lookup"><span data-stu-id="3ef47-162">Following are hello agent's configuration methods and their default values.</span></span>

<span data-ttu-id="3ef47-163">toofully összefüggésbe események egy szolgáltatás, lehet, hogy tooset `.setAutoDependencyCorrelation(true)`.</span><span class="sxs-lookup"><span data-stu-id="3ef47-163">toofully correlate events in a service, be sure tooset `.setAutoDependencyCorrelation(true)`.</span></span> <span data-ttu-id="3ef47-164">Ez lehetővé teszi, hogy hello ügynök tootrack környezetben a node.js aszinkron visszahívások között.</span><span class="sxs-lookup"><span data-stu-id="3ef47-164">This allows hello agent tootrack context across asynchronous callbacks in Node.js.</span></span>

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a><span data-ttu-id="3ef47-165">Ügynök API</span><span class="sxs-lookup"><span data-stu-id="3ef47-165">Agent API</span></span>

<!-- TODO: Fully document agent API. -->

<span data-ttu-id="3ef47-166">.NET-ügynök API hello teljes leírása [Itt](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="3ef47-166">hello .NET agent API is fully described [here](app-insights-api-custom-events-metrics.md).</span></span>

<span data-ttu-id="3ef47-167">A kérelem, esemény, metrika vagy hello Application Insights Node.js-ügyfélprogrammal kivétel követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="3ef47-167">You can track any request, event, metric, or exception using hello Application Insights Node.js client.</span></span> <span data-ttu-id="3ef47-168">hello következő példa bemutatja, néhány hello elérhető API-kat.</span><span class="sxs-lookup"><span data-stu-id="3ef47-168">hello following example demonstrates some of hello available APIs.</span></span>

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at hello beginning of your request handler
});
```

### <a name="track-your-dependencies"></a><span data-ttu-id="3ef47-169">Függőségek követése</span><span class="sxs-lookup"><span data-stu-id="3ef47-169">Track your dependencies</span></span>

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-tooall-events"></a><span data-ttu-id="3ef47-170">Egy egyéni tulajdonság tooall események hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3ef47-170">Add a custom property tooall events</span></span>

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a><span data-ttu-id="3ef47-171">HTTP GET-kérések követése</span><span class="sxs-lookup"><span data-stu-id="3ef47-171">Track HTTP GET requests</span></span>

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a><span data-ttu-id="3ef47-172">Kiszolgáló indítási idejének követése</span><span class="sxs-lookup"><span data-stu-id="3ef47-172">Track server startup time</span></span>

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a><span data-ttu-id="3ef47-173">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="3ef47-173">More resources</span></span>

* [<span data-ttu-id="3ef47-174">A telemetriai hello portálon figyelése</span><span class="sxs-lookup"><span data-stu-id="3ef47-174">Monitor your telemetry in hello portal</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="3ef47-175">Analytics-lekérdezések írása a telemetriai adatokhoz</span><span class="sxs-lookup"><span data-stu-id="3ef47-175">Write Analytics queries over your telemetry</span></span>](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[gyakran ismételt kérdések]: app-insights-troubleshoot-faq.md
