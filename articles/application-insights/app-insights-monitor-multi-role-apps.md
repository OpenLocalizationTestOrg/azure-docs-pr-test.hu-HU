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
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Az Application Insights (előzetes verzió) több összetevőt alkalmazások figyelése

Figyelheti az alkalmazások, amelyek több kiszolgáló-összetevők, szerepkörök vagy szolgáltatások állnak [Azure Application Insights](app-insights-overview.md). egyetlen alkalmazás térképként hello összetevők és a közöttük hello kapcsolatok hello állapotának jelennek meg. Egyes műveletek – több összetevő automatikus HTTP korrelációkereséssel vezethető vissza. Tároló diagnosztika integrált, és szorosan összefügg az alkalmazás telemetriai adatokat. Az alkalmazás összes hello összetevő egyetlen Application Insights-erőforrást használjon. 

![Több összetevőt alkalmazás-hozzárendelés](./media/app-insights-monitor-multi-role-apps/app-map.png)

"Összetevő" használjuk ide toomean bármely nagy alkalmazás működő részét. Például a szokásos üzleti alkalmazások állhat Ügyfélkód futó webböngészők tooone van szó, vagy további web app, használó szolgáltatások pedig vissza záró szolgáltatások. Kiszolgáló-összetevők üzemeltethető a helyszínen a hello felhő, vagy lehet, hogy az Azure webes és feldolgozói szerepkörök, esetleg szereplő tárolókhoz, például Docker vagy a Service Fabric futtathatnak. 

### <a name="sharing-a-single-application-insights-resource"></a>Egyetlen Application Insights-erőforrás megosztása 

hello Itt a fő módszer minden összetevő a az alkalmazás toohello toosend telemetriai azonos Application Insights-erőforrást, de használja hello `cloud_RoleName` tulajdonság toodistinguish összetevők szükség esetén. hello Application Insights SDK hozzáadása hello `cloud_RoleName` tulajdonság toohello telemetriai összetevők hozható létre. Például hello SDK felveszi egy webhely neve, vagy a szolgáltatás-szerepkör nevét toohello `cloud_RoleName` tulajdonság. Ezt az értéket egy telemetryinitializer felülbírálható. Alkalmazás-hozzárendelés hello használ hello `cloud_RoleName` tulajdonság tooidentify hello összetevők hello térképen.

További információ a felülírják hello `cloud_RoleName` tulajdonság lásd [tulajdonságok hozzáadása: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

Néhány esetben ez nem lehet megfelelő, és célszerű toouse külön erőforrások összetevők különböző csoportjai számára. Például szükség lehet különböző erőforrások toouse vagy más célra. Külön erőforrásokat használó azt jelenti, hogy egyetlen alkalmazás térképen; összetevők hello nem jelenik meg és hogy Ön nem tudja lekérdezni a összetevői között [Analytics](app-insights-analytics.md). Akkor is tooset hello külön erőforrásait.

Az adott ismeret feltételezzük, ez a dokumentum többi hello, amelyet több összetevők tooone Application Insights-erőforrás toosend adatait.

## <a name="configure-multi-component-applications"></a>Több összetevőt alkalmazások konfigurálása

tooget egy több összetevőt alkalmazás hozzárendelését, akkor kell tooachieve ezen célok:

* **Telepítse a legújabb előzetes hello** Application Insights csomagot minden hello alkalmazás-összetevője. 
* **Egyetlen Application Insights-erőforrás megosztása** az összes hello az alkalmazás összetevői.
* **Engedélyezze az alkalmazás több szerepkör-hozzárendelés** hello az előzetes verziójú funkciók a panelen.

Minden ehhez a típushoz hello megfelelő módszer segítségével az alkalmazás-összetevő konfigurálása. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-hello-latest-pre-release-package"></a>1. Hello legújabb kiadás előtti csomag telepítése

Frissítés, vagy hello portot Insights csomagok telepítése minden egyes kiszolgáló-összetevő hello projektben. Visual Studio használata:

1. Kattintson jobb gombbal a projektre, és válassza ki **NuGet-csomagok kezelése**. 
2. Válassza ki **közé tartoznak az előzetes**.
3. Ha az Application Insights csomagok frissítések jelenik meg, jelölje ki őket. 

    Ellenkező esetben és telepítésére hello megfelelő csomag:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - összetevők futtatásához használt Vendég végrehajtható fájlok és a Docker-tárolók a Service Fabric-alkalmazás fut
    * Microsoft.ApplicationInsights.ServiceFabric.Native - ServiceFabric alkalmazások megbízható szolgáltatásokhoz
    * Microsoft.ApplicationInsights.Kubernetes futó Kubernetes Docker-összetevők

### <a name="2-share-a-single-application-insights-resource"></a>2. Egyetlen Application Insights-erőforrás megosztása

* A Visual Studióban, kattintson jobb gombbal a projektre, és válassza ki **konfigurálja az Application Insights**, vagy **Application Insights > konfigurálása**. Hello első projekt használja a hello varázsló toocreate Application Insights-erőforrást. Az ezt követő projektek esetében válassza hello ugyanazt az erőforrást.
* Ha nincs Application Insights menü, kézi konfigurálása:

   1. A [Azure-portálon](https://portal,azure.com), nyisson meg egy másik összetevő már létrehozott hello Application Insights-erőforrást.
   2. A hello áttekintése panelen, a nyitott hello Essentials legördülő lapon és a másolási hello **Instrumentation kulcs.**
   3. A projektben nyissa meg az ApplicationInsights.config, és helyezze be:`<InstrumentationKey>your copied key</InstrumentationKey>`

![Hello instrumentation kulcs toohello .config fájl másolása](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a>3. Alkalmazás több szerepkör-hozzárendelés engedélyezése

Hello Azure-portálon nyissa meg az alkalmazás hello erőforrás. Hello előzetes panelen engedélyezése *alkalmazás több szerepkör-hozzárendelés*.

### <a name="4-enable-docker-metrics-optional"></a>4. Engedélyezze a Docker-metrikák (nem kötelező) 

Egy összetevő egy Docker egy Windows Azure virtuális gépen futó fut, ha további metrikák hello tárolóból hozhatja létre. Ezt a Beszúrás a [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurációs fájlban:

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

## <a name="use-cloudrolename-tooseparate-components"></a>Cloud_RoleName tooseparate összetevőket használnak

Hello `cloud_RoleName` tulajdonsága csatolt tooall telemetriai adatokat. Hello összetevő - hello szerepkör vagy szolgáltatás - létrehozó hello telemetriai azonosítja. (Fontos nem hello ugyanaz, mint a cloud_RoleInstance, amely elválasztja az azonos több kiszolgáló folyamatok vagy gépek párhuzamosan futó szerepkörök.)

Hello portálon kiszűrhetik vagy szegmentálja a telemetriai adatok e tulajdonság használatával. Ebben a példában a hello hibák panel szűrt tooshow csak információk hello előtér-webkiszolgáló szolgáltatás hello CRM API háttérrendszerből hibák kiszűrése:

![Felhő szerepkör neve szegmentált metrika diagram](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Összetevők közötti nyomkövetési műveletek

Nyomon követhetők a egy összetevő tooanother, hello hívások egy egyéni művelet feldolgozása közben.


![Telemetria művelet megjelenítése](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Kattintson a telemetriai adat ehhez a művelethez kapcsolódó listája tooa hello előtér-webkiszolgáló és a háttér-API hello:

![Keresés összetevői között](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Következő lépések

* [A fejlesztői, tesztelési és éles külön telemetria](app-insights-separate-resources.md)
