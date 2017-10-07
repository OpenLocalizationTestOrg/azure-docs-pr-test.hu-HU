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
# <a name="set-up-application-insights-dependency-tracking"></a>Az Application Insights beállítása: függőségi nyomon követése
A *függőségi* az alkalmazás által külső összetevője. Általában le egy HTTP, vagy egy adatbázist vagy egy fájlrendszer nevű szolgáltatás. [Az Application Insights](app-insights-overview.md) méri, hogy az alkalmazás meddig függőségek, és milyen gyakran függőségi hívás sikertelen lesz. Vizsgálja meg az adott hívások, és összekapcsolhatja őket toorequests és kivételek.

![mintadiagramok](./media/app-insights-asp-net-dependencies/10-intro.png)

hello out-of-az-box függőségfigyelő jelenleg jelentések hívások toothese típusú függőségek:

* Kiszolgáló
  * SQL Database-adatbázisok
  * ASP.NET webes és WCF-szolgáltatások HTTP-alapú kötések használó
  * Helyi vagy távoli HTTP-hívások
  * Az Azure Cosmos DB, a tábla, a blob-tároló és a várólista
* Weblapok
  * AJAX-hívások

Figyelés works segítségével [bájt kód instrumentation](https://msdn.microsoft.com/library/z9z62c29.aspx) kijelölt módszerek köré. Teljesítményigény minimális.

A saját SDK meghívja toomonitor más függőségek, mindkettő hello ügyfél és kiszolgáló-kódban, hello segítségével is kiírhatja [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

## <a name="set-up-dependency-monitoring"></a>A függőségi figyelés beállítása
Részleges függőségi gyűjt adatokat automatikusan hello [Application Insights SDK](app-insights-asp-net.md). teljes körű adatok tooget, ügynököt hello megfelelő hello kiszolgálóhoz.

| Platform | Telepítés |
| --- | --- |
| IIS-kiszolgálón |Vagy [Állapotmonitor telepítése a kiszolgálón](app-insights-monitor-performance-live-website-now.md) vagy [frissítse az alkalmazás too.NET keretrendszer 4.6-os vagy újabb](http://go.microsoft.com/fwlink/?LinkId=528259) és hello telepítése [Application Insights SDK](app-insights-asp-net.md) az alkalmazásban. |
| Azure Web App |A webes alkalmazás Vezérlőpult [hello nyissa meg az Application Insights panel a webes alkalmazás Vezérlőpult](app-insights-azure-web-apps.md) , és válassza a telepítés, ha a rendszer kéri. |
| Azure-Felhőszolgáltatás |[Használjon indítási tevékenységhez](app-insights-cloudservices.md) vagy [telepítse a .NET-keretrendszer 4.6 +](../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-toofind-dependency-data"></a>Ha toofind függőségi adatokat
* [Alkalmazás-hozzárendelés](#application-map) visualizes az alkalmazás és a szomszédos összetevők közötti függőségek.
* [Teljesítmény, a böngésző és a hiba paneleken](#performance-and-blades) kiszolgáló függőségi adatainak megjelenítése.
* [Böngészők panel](#ajax-calls) jeleníti meg a felhasználók böngészőjének az AJAX-hívások.
* [A lassú és a sikertelen kérelmek átkattintással](#diagnose-slow-requests) toocheck azok függőségi hívás.
* [Elemzés](#analytics) használt tooquery függőségi adatokat is lehet.

## <a name="application-map"></a>Alkalmazástérkép
Alkalmazás-hozzárendelés úgy működik, mint a visual támogatás toodiscovering függőségek hello az alkalmazások összetevői között. Automatikusan létrejön a hello telemetriai adatokból az alkalmazásból. Ez a példa bemutatja AJAX-hívások hello böngésző parancsfájlok és a REST-hívások hello kiszolgálóról alkalmazásszolgáltatások tootwo külső.

![Alkalmazástérkép](./media/app-insights-asp-net-dependencies/08.png)

* **Keresse meg a hello mezőkben** toorelevant függőségi és más típusú diagramokkal.
* **PIN-kód hello térkép** toohello [irányítópult](app-insights-dashboards.md), ahol azt teljesen működőképes lesz.

[További információk](app-insights-app-map.md).

## <a name="performance-and-failure-blades"></a>Teljesítmény és hibák paneleken
hello teljesítmény panelen látható hello hello server alkalmazás függőségi hívások időtartama. Létrejön egy összegző diagram és hívása szegmentált tábla.

![Teljesítmény panel függőségi diagramjain](./media/app-insights-asp-net-dependencies/dependencies-in-performance-blade.png)

Kattintson a hello összefoglaló diagramok vagy hello tábla elemek toosearch nyers előfordulását hívásokat.

![Függőségi hívás példányok](./media/app-insights-asp-net-dependencies/dependency-call-instance.png)

**Hiba száma** látható a hello **hibák** panelen. Hiba az összes visszatérési kódot, amely nincs hello tartomány 200-399, vagy ismeretlen.

> [!NOTE]
> **100 %-os hibák?** -Ez valószínűleg azt jelzi, hogy csak kap a részleges függőségi adatokat. Túl kell[állítsa be a függőségi figyelő megfelelő tooyour platform](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>AJAX-hívások
hello böngészők panel hello időtartam látható és AJAX sikertelenségének arányát hívásait [JavaScript a weblapok](app-insights-javascript.md). Szerint függőségeinek megtekinthető.

## <a name="diagnosis"></a>Lassú kérelmek diagnosztizálására
Minden egyes kérelem esemény társított hello függőségi hívások esetében, kivételeket és eseményeket, amelyek az alkalmazás feldolgozása közben a rendszer nyomon hello kérelem. Így bizonyos kérelmek rosszul hajtja végre, ha azt megtudhatja, hogy van-e tooslow válaszainak függőség miatt.

Bemutatjuk, például, hogy keresztül.

### <a name="tracing-from-requests-toodependencies"></a>Kérelmek toodependencies nyomkövetés
Hello teljesítmény panel megnyitásához, és nézze meg a kérelmek hello rács:

![Lista átlagok és száma](./media/app-insights-asp-net-dependencies/02-reqs.png)

hello felső egy nagyon hosszú tart. Nézzük meg, ha azt is megtudhatja, ahol hello idő telik.

Kattintson az adott sor toosee egyes események:

![A kérelem események listája](./media/app-insights-asp-net-dependencies/03-instances.png)

Kattintson a hosszan futó példány tooinspect tovább, és görgessen lefelé toohello távoli függőségi hívások kapcsolódó toothis kérelem:

![Hívások tooRemote függőségek található, és keresse meg a szokatlan időtartama](./media/app-insights-asp-net-dependencies/04-dependencies.png)

Úgy tűnik a legtöbb hello ideje karbantartási ehhez a kérelemhez telt egy hívás tooa helyi szolgáltatás.

Válassza ki, hogy sor tooget további információt:

![Kattintson az adott távoli függőség tooidentify hello hibát okozó keresztül](./media/app-insights-asp-net-dependencies/05-detail.png)

A jelek szerint ez az hello probléma esetén. Azt is pinpointed hello probléma, így most csak azt kell toofind, miért adott hívás tart sokáig.

### <a name="request-timeline"></a>Az idősor kérése
Más esetben nem függőségi hívás különösen hosszú van. De toohello idősor nézetének megnyitására váltásával láthatja Ha hello késedelem a saját belső feldolgozása során:

![Hívások tooRemote függőségek található, és keresse meg a szokatlan időtartama](./media/app-insights-asp-net-dependencies/04-1.png)

Úgy tűnik, nagy hiány toobe hello első függőségi hívás után, azt kell kinéznie: a kód toosee, ezért ez azt jelenti.

### <a name="profile-your-live-site"></a>Élő webhelyét profilját

Nem tudja, hol hello idő kerül? Hello [Application Insights Profilkészítő](app-insights-profiler.md) nyomkövetések HTTP meghívja a tooyour élő webhelyet, és megjeleníti a funkciókról a kódban hello leghosszabb időt vett.

## <a name="failed-requests"></a>Sikertelen kérelmek
Lehet, hogy a sikertelen kérelmek is sikertelen hívás toodependencies társítva. Ebben az esetben azt átkattintással is tootrack hello probléma le.

![Kattintson a hello sikertelen kérelmek diagram](./media/app-insights-asp-net-dependencies/06-fail.png)

Haladjon végig a sikertelen kérelmek tooan előfordulása, és tekintse meg a kapcsolódó események.

![Kattintson egy kérelemtípus, kattintson a hello példány tooget tooa különböző ábrázolása hello példányt, kattintson rá tooget kivétel részletes adatai.](./media/app-insights-asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Elemzés
Nyomon követheti a hello függőségek [Log Analytics lekérdezési nyelv](https://docs.loganalytics.io/). Néhány példa:

* Keresse meg a sikertelen függőségi hívások esetében:

```

    dependencies | where success != "True" | take 10
```

* AJAX-hívások keresése:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Keresse meg a kérelmek társított függőségi hívások esetében:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Lapmegtekintések társított AJAX-hívások keresése:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Egyéni függőségi nyomon követése
hello szabványos függőségi követése modul automatikusan észleli a külső függőségei, például adatbázisok és a REST API-k. De érdemes lehet néhány további összetevők toobe kezelni hello azonos módon.

Írhat kódot, amely a függőségi adatokat küldi hello segítségével azonos [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency) hello szabványos modulok által használt.

Például ha használjon a kód egy szerelvény, hogy saját maga nem írhatók, minden hello hívások tooit sikerült idő, milyen hozzájárulás teszi tooyour választ ki toofind alkalommal fordult elő. toohave Application Insights hello függőségi diagramjain jelennek meg ezek az adatok küldése használatával `TrackDependency`.

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

Hello szabványos függőségi követési modul ki tooswitch, szüntesse meg a hello hivatkozás tooDependencyTrackingTelemetryModule [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>Hibaelhárítás
*Függőségi sikerességét jelző mindig igaz vagy hamis értéket jeleníti meg.*

*Nem jelenik meg a teljes SQL-lekérdezésben.*

* Toohello hello SDK legújabb verziójára frissítse. Ha a .NET verziószáma kisebb, mint 4.6:
  * IIS-állomás: telepítése [Application Insights Agent](app-insights-monitor-performance-live-website-now.md) hello gazdagép-kiszolgálón.
  * Azure-webalkalmazásban: Nyissa meg az Application Insights hello web app Vezérlőpult fülre, és telepítse az Application insights szolgáltatással.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Következő lépések
* [Kivételek](app-insights-asp-net-exceptions.md)
* [Felhasználók és Lapadatok](app-insights-javascript.md)
* [Rendelkezésre állás](app-insights-monitor-web-app-availability.md)
