---
title: "Service Fabric esemény elemzése az Application insights szolgáltatással aaaAzure |} Microsoft Docs"
description: "Információ megjelenítése és eseményeket az Application Insights figyelési és az Azure Service Fabric-fürtök diagnosztika elemzése."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 59bb5a409f2951e5b2034049e782dd0da67f933c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Esemény elemzése és a képi megjelenítés az Application insights szolgáltatással

Az Azure Application Insights egy bővíthető platform, az alkalmazás figyelése és a diagnosztika. Ez magában foglalja egy hatékony elemzések és eszköz, a testre szabható irányítópult és a képi megjelenítések lekérdezése, és további beállításokat, többek között automatikus lehetőséget. Figyelés platform és a Service Fabric-alkalmazások és szolgáltatások diagnosztikai ajánlott hello.

## <a name="setting-up-application-insights"></a>Az Application Insights beállítása

### <a name="creating-an-ai-resource"></a>Az Eszközintelligencia-erőforrás létrehozása

központi toohello Azure piactéren, és keresse meg az "Application Insights" toocreate egy AI erőforrás. Akkor kell jelenik meg (már a "Web + mobil" kategória) hello első megoldásaként. Kattintson a **létrehozása** amikor nézi hello jobb erőforrás (Győződjön meg arról, hogy az elérési út megegyezik-e az alábbi képen hello).

![Új Application Insights-erőforrás](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

Kimenő néhány információt tooprovision hello erőforrás toofill megfelelően kell. A hello *alkalmazástípus* mezőjét, használja a "ASP.NET web application" Ha fog használni a Service Fabric programozási modellek vagy toohello fürt .NET alkalmazás közzététele. Használja az "Általános", ha a Vendég végrehajtható fájlok és a tárolók meg fog telepítéséhez. Általánosságban elmondható alapértelmezett toousing "ASP.NET web application" tookeep a beállítások nyissa meg a jövőbeli hello. hello neve tooyour beállítás mentése, és hello erőforráscsoport és az előfizetés hello erőforrás telepítés utáni módosítható. Azt javasoljuk, hogy a AI erőforrás hello van a fürt ugyanabban az erőforráscsoportban. Ha további tájékoztatásra van szüksége, tekintse át [Application Insights-erőforrás létrehozása](../application-insights/app-insights-create-new-resource.md)

Az esemény összesítési eszközzel hello AI Instrumentation kulcs tooconfigure AI van szüksége. Miután az Eszközintelligencia-erőforrás (hello telepítési érvényesítését követően néhány percet vesz igénybe) be van állítva, nyissa meg a tooit, és megkeresi hello **tulajdonságok** szakasz hello bal oldali navigációs sávon. Egy új panel nyílik meg, amely egy *INSTRUMENTATION kulcs*. Ha toochange hello előfizetéshez vagy erőforráscsoporthoz hello erőforrás van szüksége, azt is megteheti itt is.

### <a name="configuring-ai-with-wad"></a>ÜVEGVATTA AI konfigurálása

Két módon elsődleges toosend adatait ÜVEGVATTA tooAzure AI, amely az érhető el egy AI fogadó toohello ÜVEGVATTA konfiguráció hozzáadása a [Ez a cikk](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Adja hozzá egy AI Instrumentation kulcsot, ha a fürt létrehozása az Azure-portálon

![Egy AIKey hozzáadása](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Ha a fürt létrehozása, amikor engedélyezve van a diagnosztika "On", egy nem kötelező mező tooenter az Application Insights Instrumentation kulcs jelennek meg. Ha itt a AI IKey beillesztéséhez hello AI fogadó lesz automatikusan konfigurálva az Ön által használt toodeploy hello Resource Manager-sablon a fürt.

#### <a name="add-hello-ai-sink-toohello-resource-manager-template"></a>Hello AI fogadó toohello Resource Manager-sablon hozzáadása

A Resource Manager-sablon hello "WadCfg" hello vegye fel a "Fogadó" szerint többek között a következő két módosítások hello:

1. Hello fogadó konfiguráció hozzáadása:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Adja hozzá a következő sort a "DiagnosticMonitorConfiguration" hello "WadCfg" hello, hello DiagnosticMonitorConfiguration hello fogadó közé tartoznak:

    ```json
    "sinks": "applicationInsights"
    ```

A fenti hello mindkét hello kódrészletek neve "applicationInsights" használt toodescribe hello fogadó volt. Ez nem követelmény, és mindaddig, amíg "mosdók" hello fogadó hello nevét tartalmazza, hello tooany karakterlánc állíthatja be.

Jelenleg hello fürtből naplók állapotúként jelenik meg AI tartozó naplófájl-megjelenítő a nyomkövetési adatokat. Mivel a legtöbb hello platform érkező hello nyomkövetési szint "Tájékoztató", akkor is fontolja meg is hello fogadó konfigurációs tooonly küldési logs "Kritikus" vagy "Error" típusú. Ehhez adja hozzá a "Csatornák" tooyour fogadó, ahogyan az [Ez a cikk](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Portál vagy a Resource Manager-sablon használatakor egy helytelen AI IKey, hogy toomanually hello kulcs módosítása és hello fürt frissítése / újratelepítése. 

### <a name="configuring-ai-with-eventflow"></a>EventFlow AI konfigurálása

Ha EventFlow tooaggregate eseményeket használ, győződjön meg arról, hogy tooimport hello `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet-csomagot. hello következő rendelkezik hello szereplő toobe *kimenete* hello szakasza *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace hello following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Meg arról, hogy toomake hello szükséges módosításokat a szűrőket, valamint bármely más bemeneti (valamint a megfelelő NuGet-csomagok) tartalmazza.

## <a name="aisdk"></a>AI. SDK

Általában javasolt toouse EventFlow és mivel lehetővé teszik a több moduláris megközelítés toodiagnostics és figyelést, vagyis ha azt szeretné, toochange EventFlow a kimeneteinek, szükség van nincs változás tooyour tényleges ÜVEGVATTA összesítési megoldásokat, mint Instrumentation, csak egy egyszerű módosítása tooyour konfigurációs fájlt. Ha azonban az Application Insights segítségével tooinvest döntse el, és nincsenek valószínűleg toochange tooa különböző platform, az események összesítése, és elküldi őket tooAI AI tartozó új SDK használatával kell keresnie. Ez azt jelenti, hogy az adatok tooAI már nem kell tooconfigure EventFlow toosend, de ehelyett hello ApplicationInsight Service Fabric NuGet csomag telepíti. Hello csomag a részletek megtalálhatók [Itt](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Az Application Insights mikroszolgáltatások létrehozására és a tárolók támogatása](https://azure.microsoft.com/app-insights-microservices/) elsajátíthatja, hogy néhány hello új funkciók a (jelenleg továbbra is a bétaverzió) működő, amelyek lehetővé teszik toohave gazdagabb out-of-az-box figyelési beállítások AI. Ezek közé tartozik a függőségi követési (a szolgáltatások és alkalmazások egy fürt és hello kommunikáció közöttük egy AppMap létrehozásakor használt), és a szolgáltatások (segítséget nyújt a jobb felügyelő hello hibáját származó nyomkövetési jobb korrelációs a munkafolyamat egy alkalmazás vagy szolgáltatás).

Ha a .NET fejleszt, és valószínűleg fog kell néhány, a Service Fabric programozási modell használatával, és kész toouse AI megjelenítése és elemzéséhez esemény és a naplófájlok, akkor azt javasoljuk, hogy módba keresztül hello AI SDK útvonal, mint a figyelést a platformhoz, és diagnosztikai munkafolyamat. Olvasási [ez](../application-insights/app-insights-asp-net-more.md) és [ez](../application-insights/app-insights-asp-net-trace-logs.md) tooget AI toocollect használatával, és megjeleníti a naplókat.

## <a name="navigating-hello-ai-resource-in-azure-portal"></a>A Navigálás hello AI erőforrás Azure-portálon

Miután konfigurálta a AI kimenetként az események és a naplókat, információkat kell elindításához tooshow a AI erőforrás néhány perc múlva. Keresse meg a toohello AI erőforrás, amely akkor toohello AI erőforrás irányítópult. Kattintson a **keresési** hello AI tálca toosee hello legújabb nyomokat kapott, és képes toofilter toobe révén azokat.

*Metrikaböngésző* az eszköz lehet, hogy az alkalmazások, szolgáltatások és a fürt reporting metrikák alapján egyéni irányítópultok létrehozásához. Lásd: [felfedezése metrikákat az Application Insightsban](../application-insights/app-insights-metrics-explorer.md) tooset fel saját maga néhány diagramok hello gyűjtött adatok alapján.

Kattintson a **Analytics** léphet vissza toohello Application Insights Analytics portál, ahol, eseményeket és nagyobb hatókörrel és a lehetőség a nyomkövetéseket lekérdezheti-e. További információk a következő [az Application Insightsban Analytics](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Következő lépések

* [Állítson be riasztásokat a AI](../application-insights/app-insights-alerts.md) toobe teljesítmény vagy a használati változásaira vonatkozó értesítés
* [Az Application Insights az észlelést intelligens](../application-insights/app-insights-proactive-diagnostics.md) tooAI toowarn küldi hello telemetriai proaktív elemzésének hajt végre, mert ez teljesítményproblémákat okozhat
