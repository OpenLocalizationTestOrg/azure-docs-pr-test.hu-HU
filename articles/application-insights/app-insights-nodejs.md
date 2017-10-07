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
# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>A Node.js szolgáltatások és appok figyelése az Application Insights segítségével

[Az Azure Application Insights](app-insights-overview.md) figyeli a háttér-szolgáltatások és az összetevők toohelp központi telepítésük után meg [felderíteni, és gyorsan a teljesítmény- és egyéb problémák diagnosztizálásához](app-insights-detect-triage-diagnose.md). Bármilyen Node.js szolgáltatáshoz használható, azok üzemeltetési helyétől: az adatközpontban, Azure-beli virtuális gépen vagy Web Apps-on, vagy akár nyilvános felhőkön.

tooreceive, tárolja, és áttekintheti a figyelési adatokat, kövesse az utasításokat tooinclude a kódban az ügynök a következő hello és állítsa be a megfelelő Application Insights-erőforrást az Azure-ban. hello ügynök küld adatokat toothat erőforrás további elemzés és kutatási funkciójával.

hello Node.js ügynök automatikusan képes figyelni a bejövő és kimenő HTTP kérelmeket, számos rendszer metrikákat és kivételeket. A 0.20 verziótó kezdve néhány gyakori külső csomag figyelésére is képes, mint például a `mongodb`, a `mysql` és a `redis`. Összes esemény kapcsolódó tooan bejövő HTTP-kérelem gyorsabban hibaelhárítási közötti kapcsolatot.

Az alkalmazás további aspektusait figyelheti és a rendszer által manuálisan tagolása azokat a későbbiekben olvashat hello-ügynök API használatával.

![Példa teljesítményfigyelő diagramokra](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Első lépések

Tekintsük át, hogyan állíthatja be a felügyeletet egy apphoz vagy egy szolgáltatáshoz.

### <a name="resource"></a> Egy App Insights-erőforrás beállítása

**Mielőtt hozzákezd**, győződjön meg róla, hogy rendelkezik Azure-előfizetéssel vagy [igényeljen ingyenesen egy újat][azure-free-offer]. Ha a szervezete rendelkezik Azure-előfizetéssel, kövesse a rendszergazda [ezeket az utasításokat] [ add-aad-user] tooadd meg tooit.

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Most jelentkezzen be toohello [Azure-portálon] [ portal] és az Application Insights-erőforrás létrehozása hello következő ismertetett módon – kattintson az "Új" > "Fejlesztői eszközök" > "Application Insights". hello erőforrás végpont a telemetriai adatokat, ezek az adatok, jelentések és irányítópultok, a szabály riasztás konfigurálásában és még mentett-tároló tartalmazza.

![App Insights-erőforrás létrehozása](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Hello erőforrás létrehozása lapon válassza ki a hello alkalmazás típusa legördülő "Node.js-alkalmazás". hello alkalmazás típusa határozza meg a hello alapértelmezett készletét irányítópultokat és jelentéseket hozott létre. Aggodalomra azonban semmi ok, bármely App Insights-erőforrás képes bármilyen nyelvből és platformból adatot gyűjteni.

![Új App Insights-erőforrás űrlap](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a>Hello Node.js ügynök beállítása

Most már idő tooinclude hello ügynök az alkalmazásban, adatokat gyűjthessen.
Indítsa el az erőforrás Instrumentation kulcsának (továbbiakban említett tooas a `ikey`) hello portálról alább látható módon. hello App Insights rendszer e kulcs toomap adatok tooyour Azure-erőforrás használ, így a toospecify van szüksége, egy környezeti változó vagy a hello ügynök toouse kódját.  

![Rendszerállapotkulcs másolása](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Ezután adja hozzá a package.json keresztül hello Node.js ügynök könyvtár tooyour alkalmazás függőségeit. Az alkalmazás hello gyökérmappájában futtassa:

```bash
npm install applicationinsights --save
```

Most tooexplicitly hello-könyvtár betöltése a kódban. Hello ügynök esetében instrumentation sok más szalagtárak be, mert be kell tölteni azt más azelőtt a lehető leghamarabb `require` utasításokat. tooget elindítva, az első .js fájl hello tetején adja hozzá:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

Hello `setup` metódus hello instrumentation kulcs (és így Azure-erőforrás) konfigurálja az összes nyomon követett elemek alapértelmezés szerint használt toobe. Hívás `start` után konfigurációs végzett toobegin összegyűjtése és a telemetriai adatok küldését.

Is megadhatja egy ikey keresztül hello környezeti változó appinsights által biztosított\_ahelyett, hogy át azt manuálisan túl INSTRUMENTATIONKEY `setup()` vagy `getClient()`. Ez az eljárás lehetővé teszi a véglegesített forráskód kívül ikeys és toospecify különböző ikeys különböző környezetekben.

A további konfigurációs lehetőségek az alábbi dokumentációban érhetőek el.

Megpróbálhatja hello ügynök úgy, hogy hello instrumentation tooany kulcs nem üres karakterláncnak telemetriai adatok elküldése nélkül.

### <a name="monitor"></a> Figyelje alkalmazását

hello ügynök automatikusan összegyűjti a telemetriai adatainak hello Node.js futásidejű és néhány gyakori külső modulok. Az alkalmazás most már toogenerate használja az adatok egy részét.

Ezt követően a hello [Azure-portálon] [ portal] keresse meg a korábban létrehozott toohello Application Insights-erőforrást, és keresse meg az első néhány adatpont hello áttekintése idővonalon, mint a következő kép hello. Kattintson a további részletekért hello diagramokat.

![Első adatpontok](./media/app-insights-nodejs/12-first-perf.png)

Kattintson a hello alkalmazás térkép gomb tooview hello topológia felderítése az alkalmazások, mint a következő kép hello. Kattintson a további részletekért hello leképezés összetevők.

![Egyszerű apphozzárendelés](./media/app-insights-nodejs/06-appinsights_appmap.png)

További tudnivalók az alkalmazása és elhárítása hello segítségével más hello "Vizsgálat" szakasz alatt elérhető nézeteket.

![Vizsgálat szakasz](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Nincs adat?

Mert hello ügynök kötegek adatainak beküldése lehet késleltetést elemek hello portálon megjelenése előtt. Ha nem látja az erőforrás adatainak próbáljon ki a következő javítások hello:

* Hello alkalmazásával néhány további; További műveletek toogenerate tegye meg a további telemetriai adatokat.
* Kattintson a **frissítése** hello portál erőforrás nézetben. Diagram automatikus frissítése maguk rendszeres időközönként, de azonnal frissítése arra kényszeríti a toohappen.
* Ellenőrizze, hogy a [szükséges kimenő portok](app-insights-ip-addresses.md) nyitva vannak-e.
* Nyissa meg hello [keresési](app-insights-diagnostic-search.md) csempén, és keresse meg az egyes események.
* Ellenőrizze a hello [gyakran ismételt kérdések][].


## <a name="agent-configuration"></a>Ügynökkonfiguráció

Az alábbiakban hello ügynök konfigurációs módszerek és az alapértelmezett értékekre.

toofully összefüggésbe események egy szolgáltatás, lehet, hogy tooset `.setAutoDependencyCorrelation(true)`. Ez lehetővé teszi, hogy hello ügynök tootrack környezetben a node.js aszinkron visszahívások között.

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

## <a name="agent-api"></a>Ügynök API

<!-- TODO: Fully document agent API. -->

.NET-ügynök API hello teljes leírása [Itt](app-insights-api-custom-events-metrics.md).

A kérelem, esemény, metrika vagy hello Application Insights Node.js-ügyfélprogrammal kivétel követheti nyomon. hello következő példa bemutatja, néhány hello elérhető API-kat.

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

### <a name="track-your-dependencies"></a>Függőségek követése

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

### <a name="add-a-custom-property-tooall-events"></a>Egy egyéni tulajdonság tooall események hozzáadása

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>HTTP GET-kérések követése

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Kiszolgáló indítási idejének követése

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>További erőforrások

* [A telemetriai hello portálon figyelése](app-insights-dashboards.md)
* [Analytics-lekérdezések írása a telemetriai adatokhoz](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[gyakran ismételt kérdések]: app-insights-troubleshoot-faq.md
