---
title: "A Node.js szolgáltatások figyelése az Azure Application Insights segítségével | Microsoft Docs"
description: "Teljesítmény figyelése és problémák diagnosztizálása a Node.js szolgáltatásokban az Application Insights segítségével."
services: application-insights
documentationcenter: nodejs
author: mrbullwinkle
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: mbullwin
ms.openlocfilehash: 8f7a2344b6676a9067cf0adff04f49a73ce457fc
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/13/2017
---
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>A Node.js szolgáltatások és appok figyelése az Application Insights segítségével

Az üzembe helyezést követően a háttérszolgáltatásokat az [Azure Application Insights](app-insights-overview.md) felügyeli, hogy segítsen [felderíteni és gyorsan diagnosztizálni a teljesítménnyel kapcsolatos és más jellegű problémákat](app-insights-detect-triage-diagnose.md). Az Application Insights használható bármilyen Node.js-szolgáltatáshoz, amely futhat az adatközpontban, egy Azure-beli virtuális gépen vagy webalkalmazáson, vagy akár egy nyilvános felhőn is.

A megfigyelési adatok fogadásához, tárolásához és vizsgálatához építse be az SDK-t a programkódba, majd állítson be egy megfelelő Application Insights-erőforrást az Azure-ban. Az SDK ennek az erőforrásnak küldi az adatokat további elemzés és vizsgálat céljából.

A Node.js SDK automatikusan képes figyelni a bejövő és kimenő HTTP-kéréseket, kivételeket és bizonyos rendszermérőszámokat. A 0.20-as verziótól kezdve az SDK bizonyos gyakori külső eredetű csomagokat is képes monitorozni, pl. a MongoDB, a MySQL és a Redis csomagjait. Az egyes bejövő HTTP-kérésekhez kapcsolódó összes eseményt összekapcsolja a gyorsabb hibaelhárítás érdekében.

A TelemetryClient API használatával manuálisan beállíthatók és monitorozhatók az alkalmazás és a rendszer további részletei. A TelemetryClient API-t a jelen cikk egy későbbi részében részletesebben ismertetjük.

![Példa teljesítményfigyelő diagramokra](./media/app-insights-nodejs/10-perf.png)

## <a name="get-started"></a>Bevezetés

Egy alkalmazás vagy szolgáltatás monitorozásának beállításához a következő feladatokat kell elvégezni.

### <a name="prerequisites"></a>Előfeltételek

Mielőtt hozzákezd, győződjön meg róla, hogy rendelkezik Azure-előfizetéssel, vagy [igényeljen ingyenesen egy újat][azure-free-offer]. Ha szervezete már rendelkezik Azure-előfizetéssel, egy rendszergazda [az alábbi utasítások követésével][add-aad-user] Önt is hozzá tudja adni az előfizetéshez.

[azure-free-offer]: https://azure.microsoft.com/free/
[add-aad-user]: https://docs.microsoft.com/azure/active-directory/active-directory-users-create-azure-portal


### <a name="resource"></a> Application Insights-erőforrás beállítása


1. Jelentkezzen be az [Azure Portalra][portal].
2. Válassza az **Új** > **Fejlesztői eszközök** > **Application Insights** elemet. Az erőforrás tartalmaz egy végpontot a telemetriai adatok fogadására, valamint az érkező adatok, a mentett jelentések és irányítópultok, a szabály- és riasztási konfigurációk és továbbiak tárolására.

  ![Application Insights-erőforrás létrehozása](./media/app-insights-nodejs/03-new_appinsights_resource.png)

3. Az erőforrás-létrehozási oldalon az **Alkalmazás típusa** mezőben válassza a **Node.js-alkalmazás** elemet. Az alkalmazástípus határozza meg, hogy milyen alapértelmezett irányítópultokat és jelentéseket hoz létre a rendszer. (Bármely Application Insights-erőforrás képes bármilyen nyelvből és platformból adatot gyűjteni.)

  ![Új Application Insights-erőforrás űrlap](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="sdk"></a> A Node.js SDK beállítása

Építse be az SDK-t az alkalmazásba, hogy az adatokat tudjon gyűjteni. 

1. Másolja át az erőforrás rendszerállapotkulcsát (más néven: *ikey*) az Azure Portalról. Az Application Insights a rendszerállapotkulcs segítségével rendeli hozzá az adatokat az Azure-erőforráshoz. Ahhoz, hogy az SDK használni tudja a rendszerállapotkulcsot, meg kell azt adni a programkód egy környezeti változójában.  

  ![Rendszerállapotkulcs másolása](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

2. Adja hozzá a Node.js SDK-kódtárat az alkalmazás függőségeihez a package.json lapon. Futtassa az app gyökérkönyvtárából az alábbi parancsot:

  ```bash
  npm install applicationinsights --save
  ```

3. Explicit módon töltse be a kódtárat a programkódba. Mivel az SDK a rendszerállapotot több más kódtárba is beépíti, a kódtárat a lehető leghamarabb be kell betölteni, akár más `require`-utasítások előtt. 

  Adja hozzá az első .js fájl elejéhez a következő kódot. A `setup` metódus konfigurálja a rendszerállapotkulcsot (és így az Azure-erőforrást), hogy minden követett elemhez alapértelmezetten azt használja a rendszer.

  ```javascript
  const appInsights = require("applicationinsights");
  appInsights.setup("<instrumentation_key>");
  appInsights.start();
  ```
   
  A rendszerállapotkulcsot az APPINSIGHTS\_INSTRUMENTATIONKEY környezeti változón keresztül is megadhatja ahelyett, hogy kézzel adná azt át a `setup()` vagy a `new appInsights.TelemetryClient()` függvénynek. Ez az eljárás lehetővé teszi, hogy ne írja be az erőforráskulcsot a jóváhagyott forráskódba, és eltérő erőforráskulcsot adjon meg az eltérő környezeteknél.

  További konfigurációs részletekért lásd a következő szakaszokat.

  Az SDK-t telemetria küldése nélkül is kipróbálhatja az `appInsights.defaultClient.config.disableAppInsights = true` beállításával.

### <a name="monitor"></a> Figyelje alkalmazását

Az SDK automatikusan gyűjti a telemetriaadatokat a Node.js-futtatókörnyezetről és néhány gyakori külső modulról. Az alkalmazás használatával gyűjtsön össze néhányat ezekből az adatokból.

Ezután az [Azure Portalon][portal] lépjen a korábban létrehozott Application Insights-erőforráshoz. Az **Áttekintő idővonalon** tekintse meg az első néhány adatpontot. Részletesebb adatokért válasszon a diagramok különböző összetevői közül.

![Első adatpontok](./media/app-insights-nodejs/12-first-perf.png)

Az alkalmazáshoz felderített topológia megtekintéséhez kattintson az **Alkalmazás-hozzárendelés** gombra. Részletesebb adatokért válasszon a diagram különböző összetevői közül.

![Egyszerű apphozzárendelés](./media/app-insights-nodejs/06-appinsights_appmap.png)

Az alkalmazás részletesebb megismeréséhez és a problémák elhárításához válassza ki a **VIZSGÁLAT** szakasz további elérhető nézeteit.

![Vizsgálat szakasz](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Nincs adat?

Mivel az SDK kötegeli az adatokat az elküldéshez, az elemek késve jelenhetnek meg a portálon. Ha nem lát adatokat az erőforrásában, próbálja elvégezni valamelyik javítást az alábbiak közül:

* Folytassa az alkalmazás használatát. Végezzen el több műveletet, hogy több telemetria jöjjön létre.
* Kattintson a **Frissítés** gombra a portál erőforrásnézetében. A diagramok adott időközönként maguktól frissülnek, de manuálisan azonnal is frissíthetők.
* Ellenőrizze, hogy a [szükséges kimenő portok](app-insights-ip-addresses.md) nyitva vannak-e.
* [Keressen rá](app-insights-diagnostic-search.md) bizonyos eseményekre.
* Tekintse meg a [gyakori kérdésekkel foglalkozó részt][FAQ].


## <a name="sdk-configuration"></a>SDK konfigurálása

Az SDK konfigurálásának módját és az alapértelmezett értékeket a következő példakód sorolja fel.

Ahhoz, hogy teljes körűen megállapíthassa egy szolgáltatás eseményeinek korrelációját, mindenképpen be kell állítania a `.setAutoDependencyCorrelation(true)` értéket. E beállítás lehetővé teszi, hogy a Node.js-ben az SDK kövesse a kontextust az aszinkron visszahívások során.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(true)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .setAutoCollectConsole(true)
    .setUseDiskRetryCaching(true)
    .start();
```

## <a name="telemetryclient-api"></a>TelemetryClient API

A TelemetryClient API részletes leírásával kapcsolatban lásd az [egyéni eseményekhez és a mérőszámokhoz rendelkezésre álló Application Insights API-t](app-insights-api-custom-events-metrics.md).

Az Application Insights Node.js SDK használatával bármilyen kérést, eseményt, mérőszámot vagy kivételt követhet. Az alábbi példakód a használható API-k közül mutat be néhányat.

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey is in env var
let client = appInsights.defaultClient;

client.trackEvent({name: "my custom event", properties: {customProperty: "custom property value"}});
client.trackException({exception: new Error("handled exceptions can be logged with this method")});
client.trackMetric({name: "custom metric", value: 3});
client.trackTrace({message: "trace message"});
client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL"});
client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true});

let http = require("http");
http.createServer( (req, res) => {
  client.trackNodeHttpRequest({request: req, response: res}); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Függőségek követése

A következő kóddal követheti a függőségeket:

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.defaultClient;

var success = false;
let startTime = Date.now();
// Execute dependency call here...
let duration = Date.now() - startTime;
success = true;

client.trackDependency({dependencyTypeName: "dependency name", name: "command name", duration: duration, success: success});
```

### <a name="add-a-custom-property-to-all-events"></a>Egyéni tulajdonság hozzáadása eseményekhez

A következő kóddal adhat hozzá egyéni tulajdonságot minden eseményhez:

```javascript
appInsights.defaultClient.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>HTTP GET-kérések követése

A következő kóddal követheti a HTTP GET-kéréseket:

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.defaultClient.trackNodeHttpRequest({request: req, response: res});
    }
    // Other work here...
    res.end();
});
```

### <a name="track-server-startup-time"></a>Kiszolgáló indítási idejének követése

A következő kóddal követheti a kiszolgáló indítási idejét:

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.defaultClient.trackMetric({name: "server startup time", value: duration});
});
```

## <a name="next-steps"></a>Következő lépések

* [A telemetria figyelése a portálon](app-insights-dashboards.md)
* [Analytics-lekérdezések írása a telemetriai adatokhoz](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[FAQ]: app-insights-troubleshoot-faq.md

