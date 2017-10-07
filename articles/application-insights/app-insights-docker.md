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
# <a name="monitor-docker-applications-in-application-insights"></a>Az Application Insightsban Docker-alkalmazások figyelése
A teljesítményszámlálók életciklus-események és a teljesítmény [Docker](https://www.docker.com/) tárolók is forrásadatok az Application insights szolgáltatással. Telepítse a hello [Application Insights](app-insights-overview.md) lemezkép a gazdagép és a tároló jelennek-teljesítményszámlálók hello állomás is megegyezik a hello más képek.

Docker az egyszerűsített tárolók teljes összes függőségekkel rendelkező alkalmazások terjesztése. Ezek fogja futtatni, a gazdagépen, amelyen egy Docker-motorhoz.

Hello futtatásakor [Application Insights kép](https://hub.docker.com/r/microsoft/applicationinsights/) a Docker állomáson ilyen előnyt beolvasása:

* Életciklus telemetriai adatainak fut az összes hello tároló hello gazdagép - indítása, leállítása, és így tovább.
* Az összes hello tároló számlálói. Processzor, memória, hálózati használati és több.
* Ha Ön [Javához készült Application Insights SDK telepítve](app-insights-java-live.md) hello alkalmazások hello tárolókban lévő fut, az alkalmazások minden hello telemetriai hello tároló és a gazdagép további tulajdonságok rendelkeznek. Így például ha egy alkalmazás több gazdagépen fut, könnyen szűrheti az alkalmazás telemetriai állomás.

![Példa](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a>Az Application Insights-erőforrás beállítása
1. Jelentkezzen be a [Microsoft Azure-portálon](https://azure.com) , és nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz; vagy [hozzon létre egy újat](app-insights-create-new-resource.md). 
   
    *Erőforrás-érdemes használni?* Ha a gazdagépen futó alkalmazások hello fejlesztett valaki más, akkor meg kell túl[hozzon létre egy új Application Insights-erőforrást](app-insights-create-new-resource.md). Ez az ahol megtekintése és elemzése hello telemetriai adatokat. (Válassza a "Általános" hello app típushoz.)
   
    De ha hello fejlesztői hello alkalmazásokat, majd Reméljük, hogy [Application Insights SDK hozzáadott](app-insights-java-live.md) tooeach közülük. Ha valóban az valamennyi összetevője egy egyetlen üzleti alkalmazást, majd konfigurálhatja az összes toosend telemetriai tooone erőforrás, és fogja használni, hogy ugyanazon erőforrás toodisplay hello Docker életciklust és a teljesítmény. 
   
    Egy harmadik forgatókönyv, hello alkalmazások többsége kidolgozott, de használ külön erőforrások toodisplay a telemetriai adatokat. Ebben az esetben akkor valószínűleg is kívánt toocreate hello Docker adatokat külön erőforrás. 
2. Hozzáadás hello Docker csempe: válasszon **vegye fel a csempe**, húzza hello Docker csempe hello gyűjteményből, és kattintson a **kész**. 
   
    ![Példa](./media/app-insights-docker/03.png)
3. Kattintson a hello **Essentials** legördülő és hello Instrumentation kulcs másolása. Használja a tootell hello SDK ahol toosend a telemetriai adatokat.

    ![Példa](./media/app-insights-docker/02-props.png)

Megőrizni böngészőablakot lesz szüksége, szerint, majd térjen vissza tooit hamarosan toolook a telemetriai adatokat.

## <a name="run-hello-application-insights-monitor-on-your-host"></a>Hello Application Insights-figyelő futtatása a gazdagépen
Most, hogy telepítve vannak-e valahol toodisplay hello telemetriai, hello indexelése alkalmazást, amely gyűjt, és elküldi a állíthat be.

1. Tooyour Docker gazdagép csatlakoztatása. 
2. Szerkesztheti a instrumentation be ezt a parancsot, és futtassa azt:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Csak egy Application Insights-lemezkép szükség a Docker állomásonként. Ha az alkalmazás több Docker gazdagépen van telepítve, majd ismételje meg a hello parancsot minden gazdagépen.

## <a name="update-your-app"></a>Az alkalmazás frissítése
Ha az alkalmazás a hello van tagolva [Javához készült Application Insights SDK](app-insights-java-get-started.md), adja hozzá a következő sor hello ApplicationInsights.xml fájlba a projekt alatt hello hello `<TelemetryInitializers>` elem:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Ez biztosítja a Docker-információk, például a tároló és az állomás azonosítója tooevery telemetriai elem küldése az alkalmazásból.

## <a name="view-your-telemetry"></a>A telemetria megtekintése
Lépjen vissza az Azure-portálon hello tooyour Application Insights-erőforrást.

Kattintson a hello Docker csempe.

Hello Docker alkalmazásból érkező adatok hamarosan megjelenik, különösen akkor, ha más tárolók futó a Docker-motorhoz.

Az alábbiakban néhány hello nézetek kaphat.

### <a name="perf-counters-by-host-activity-by-image"></a>Állomás, kép tevékenység teljesítményszámlálói
![Példa](./media/app-insights-docker/10.png)

![Példa](./media/app-insights-docker/11.png)

Kattintson a további részletek gazdagép- vagy képfájl nevére.

toocustomize hello nézetet, kattintson a bármely diagram, hello rács fejléc, vagy adja hozzá a diagram. 

[További információ a metrikaböngésző](app-insights-metrics-explorer.md).

### <a name="docker-container-events"></a>Docker-tároló események
![Példa](./media/app-insights-docker/13.png)

egyéni események tooinvestigate, kattintson a [keresési](app-insights-diagnostic-search.md). Keresés és szűrés toofind hello eseményeket. Kattintson a bármely esemény tooget további információkhoz juthat.

### <a name="exceptions-by-container-name"></a>Kivételek tároló neve
![Példa](./media/app-insights-docker/14.png)

### <a name="docker-context-added-tooapp-telemetry"></a>Docker környezetben hozzáadott tooapp telemetriai adat
AI SDK-t és Docker környezetben növelést tagolva hello alkalmazásból küldött telemetriai kérelem:

![Példa](./media/app-insights-docker/16.png)

Processzor és memória teljesítményszámlálókat, dúsított, majd Docker-tároló neve szerint csoportosítva:

![Példa](./media/app-insights-docker/15.png)

## <a name="q--a"></a>Kérdések és válaszok
*Mit nem az Application Insights engedi, hogy nem jelenik meg a Docker?*

* Részletes információkat a tároló és a lemezkép teljesítményszámlálók.
* Integrálják a tároló és az alkalmazás adatokat egy irányítópulton.
* [Telemetriai adatok exportálása](app-insights-export-telemetry.md) további elemzés tooa adatbázishoz, a Power bi-ban vagy a más irányítópult.

*Hogyan szerezhetek telemetriai maga hello alkalmazásból?*

* Hello Application Insights SDK telepítése hello alkalmazásban. Megtudhatja, hogyan lehet a: [Java-webalkalmazások](app-insights-java-get-started.md), [Windows webalkalmazások](app-insights-asp-net.md).

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Következő lépések

* [Javához készült Application insights szolgáltatással](app-insights-java-get-started.md)
* [A Node.js Application insights szolgáltatással](app-insights-nodejs.md)
* [Az ASP.NET Application insights szolgáltatással](app-insights-asp-net.md)
