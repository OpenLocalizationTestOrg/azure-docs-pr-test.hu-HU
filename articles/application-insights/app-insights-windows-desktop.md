---
title: "aaaMonitoring használati és teljesítményadatokat Windows asztali alkalmazások esetén"
description: "Windowsos asztali alkalmazások használatát és teljesítményét elemezheti a HockeyApp és az Application Insights segítségével."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2016
ms.author: bwren
ms.openlocfilehash: 73806885a6f0ed3896c0e43308c90ba087007887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Windowsos asztali alkalmazások használatának és teljesítményének figyelése


Az [Azure Application Insights](app-insights-overview.md) és a [HockeyApp](https://hockeyapp.net) a telepített alkalmazások használatának és teljesítményének figyelését teszi lehetővé.

> [!IMPORTANT]
> Ajánlott [Hockeyappra](https://hockeyapp.net) toodistribute és a figyelő asztali és az eszköz alkalmazások. A HockeyApp segítségével a terjesztést, a működés közbeni tesztelést és a felhasználói visszajelzéseket felügyelheti, valamint a használatot és az összeomlási jelentéseket figyelheti. Ezen túlmenően [telemetriai adatokat exportálhat és kérdezhet le az Analytics használatával](app-insights-hockeyapp-bridge-app.md).
> 
> Telemetria küldhetők tooApplication Insights egy asztali alkalmazás, bár ez elsősorban az hibakeresési és kísérleti használható.
> 
> 

## <a name="toosend-telemetry-tooapplication-insights-from-a-windows-application"></a>toosend telemetriai tooApplication Insights Windows-alkalmazás
1. A hello [Azure-portálon](https://portal.azure.com), [Application Insights-erőforrás létrehozása](app-insights-create-new-resource.md). Az alkalmazás típusánál válassza az ASP.NET alkalmazás lehetőséget.
2. Tegye meg a hello Instrumentation kulcs másolatát. Hello kulcs található hello Essentials legördülő a most létrehozott hello új erőforrást. 
3. A Visual Studio NuGet-csomagok hello a alkalmazás projekt szerkesztése, és vegye fel a Microsoft.ApplicationInsights.WindowsServer. (Vagy Microsoft.ApplicationInsights válassza, ha csak operációs rendszer API nélkül hello szabványos telemetriai gyűjtemény modulok hello.)
4. Állítsa be hello instrumentation kulcs vagy a kódban:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "`*az Ön kulcsa*`";` 
   
    vagy applicationinsights.config (Ha telepítette az egyik hello szabványos telemetriai csomagok):
   
    `<InstrumentationKey>`*az Ön kulcsa*`</InstrumentationKey>` 
   
    ApplicationInsights.config használatakor győződjön róla túl van-e beállítva a tulajdonságait a Solution Explorer**Build művelet = tartalom másolása tooOutput Directory másolási =**.
5. [Hello API-t használó](app-insights-api-custom-events-metrics.md) toosend telemetriai adatokat.
6. Az alkalmazás futtatása, és tekintse meg a hello telemetriai hello Azure-portálon létrehozott hello erőforrás.

## <a name="telemetry"></a>Mintakód
```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative toosetting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Következő lépések
* [Irányítópult létrehozása](app-insights-dashboards.md)
* [Diagnosztikai keresés](app-insights-diagnostic-search.md)
* [Metrikák böngészése](app-insights-metrics-explorer.md)
* [Analytics-lekérdezések](app-insights-analytics.md)

