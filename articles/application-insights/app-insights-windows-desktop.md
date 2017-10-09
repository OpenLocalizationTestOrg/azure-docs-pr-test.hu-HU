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
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a><span data-ttu-id="93f2b-103">Windowsos asztali alkalmazások használatának és teljesítményének figyelése</span><span class="sxs-lookup"><span data-stu-id="93f2b-103">Monitoring usage and performance in Windows Desktop apps</span></span>


<span data-ttu-id="93f2b-104">Az [Azure Application Insights](app-insights-overview.md) és a [HockeyApp](https://hockeyapp.net) a telepített alkalmazások használatának és teljesítményének figyelését teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="93f2b-104">[Azure Application Insights](app-insights-overview.md) and [HockeyApp](https://hockeyapp.net) let you monitor your deployed application for usage and performance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93f2b-105">Ajánlott [Hockeyappra](https://hockeyapp.net) toodistribute és a figyelő asztali és az eszköz alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="93f2b-105">We recommend [HockeyApp](https://hockeyapp.net) toodistribute and monitor desktop and device apps.</span></span> <span data-ttu-id="93f2b-106">A HockeyApp segítségével a terjesztést, a működés közbeni tesztelést és a felhasználói visszajelzéseket felügyelheti, valamint a használatot és az összeomlási jelentéseket figyelheti.</span><span class="sxs-lookup"><span data-stu-id="93f2b-106">With HockeyApp, you can manage distribution, live testing, and user feedback, as well as monitor usage and crash reports.</span></span> <span data-ttu-id="93f2b-107">Ezen túlmenően [telemetriai adatokat exportálhat és kérdezhet le az Analytics használatával](app-insights-hockeyapp-bridge-app.md).</span><span class="sxs-lookup"><span data-stu-id="93f2b-107">You can also [export and query your telemetry with Analytics](app-insights-hockeyapp-bridge-app.md).</span></span>
> 
> <span data-ttu-id="93f2b-108">Telemetria küldhetők tooApplication Insights egy asztali alkalmazás, bár ez elsősorban az hibakeresési és kísérleti használható.</span><span class="sxs-lookup"><span data-stu-id="93f2b-108">Although telemetry can be sent tooApplication Insights from a desktop application, this is chiefly useful for debugging and experimental purposes.</span></span>
> 
> 

## <a name="toosend-telemetry-tooapplication-insights-from-a-windows-application"></a><span data-ttu-id="93f2b-109">toosend telemetriai tooApplication Insights Windows-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="93f2b-109">toosend telemetry tooApplication Insights from a Windows application</span></span>
1. <span data-ttu-id="93f2b-110">A hello [Azure-portálon](https://portal.azure.com), [Application Insights-erőforrás létrehozása](app-insights-create-new-resource.md).</span><span class="sxs-lookup"><span data-stu-id="93f2b-110">In hello [Azure portal](https://portal.azure.com), [create an Application Insights resource](app-insights-create-new-resource.md).</span></span> <span data-ttu-id="93f2b-111">Az alkalmazás típusánál válassza az ASP.NET alkalmazás lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="93f2b-111">For application type, choose ASP.NET app.</span></span>
2. <span data-ttu-id="93f2b-112">Tegye meg a hello Instrumentation kulcs másolatát.</span><span class="sxs-lookup"><span data-stu-id="93f2b-112">Take a copy of hello Instrumentation Key.</span></span> <span data-ttu-id="93f2b-113">Hello kulcs található hello Essentials legördülő a most létrehozott hello új erőforrást.</span><span class="sxs-lookup"><span data-stu-id="93f2b-113">Find hello key in hello Essentials drop-down of hello new resource you just created.</span></span> 
3. <span data-ttu-id="93f2b-114">A Visual Studio NuGet-csomagok hello a alkalmazás projekt szerkesztése, és vegye fel a Microsoft.ApplicationInsights.WindowsServer.</span><span class="sxs-lookup"><span data-stu-id="93f2b-114">In Visual Studio, edit hello NuGet packages of your app project, and add Microsoft.ApplicationInsights.WindowsServer.</span></span> <span data-ttu-id="93f2b-115">(Vagy Microsoft.ApplicationInsights válassza, ha csak operációs rendszer API nélkül hello szabványos telemetriai gyűjtemény modulok hello.)</span><span class="sxs-lookup"><span data-stu-id="93f2b-115">(Or choose Microsoft.ApplicationInsights if you just want hello bare API, without hello standard telemetry collection modules.)</span></span>
4. <span data-ttu-id="93f2b-116">Állítsa be hello instrumentation kulcs vagy a kódban:</span><span class="sxs-lookup"><span data-stu-id="93f2b-116">Set hello instrumentation key either in your code:</span></span>
   
    <span data-ttu-id="93f2b-117">`TelemetryConfiguration.Active.InstrumentationKey = "`*az Ön kulcsa*`";`</span><span class="sxs-lookup"><span data-stu-id="93f2b-117">`TelemetryConfiguration.Active.InstrumentationKey = "` *your key* `";`</span></span> 
   
    <span data-ttu-id="93f2b-118">vagy applicationinsights.config (Ha telepítette az egyik hello szabványos telemetriai csomagok):</span><span class="sxs-lookup"><span data-stu-id="93f2b-118">or in ApplicationInsights.config (if you installed one of hello standard telemetry packages):</span></span>
   
    <span data-ttu-id="93f2b-119">`<InstrumentationKey>`*az Ön kulcsa*`</InstrumentationKey>`</span><span class="sxs-lookup"><span data-stu-id="93f2b-119">`<InstrumentationKey>`*your key*`</InstrumentationKey>`</span></span> 
   
    <span data-ttu-id="93f2b-120">ApplicationInsights.config használatakor győződjön róla túl van-e beállítva a tulajdonságait a Solution Explorer**Build művelet = tartalom másolása tooOutput Directory másolási =**.</span><span class="sxs-lookup"><span data-stu-id="93f2b-120">If you use ApplicationInsights.config, make sure its properties in Solution Explorer are set too**Build Action = Content, Copy tooOutput Directory = Copy**.</span></span>
5. <span data-ttu-id="93f2b-121">[Hello API-t használó](app-insights-api-custom-events-metrics.md) toosend telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="93f2b-121">[Use hello API](app-insights-api-custom-events-metrics.md) toosend telemetry.</span></span>
6. <span data-ttu-id="93f2b-122">Az alkalmazás futtatása, és tekintse meg a hello telemetriai hello Azure-portálon létrehozott hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="93f2b-122">Run your app, and see hello telemetry in hello resource you created in hello Azure Portal.</span></span>

## <span data-ttu-id="93f2b-123"><a name="telemetry"></a>Mintakód</span><span class="sxs-lookup"><span data-stu-id="93f2b-123"><a name="telemetry"></a>Example code</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="93f2b-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="93f2b-124">Next steps</span></span>
* [<span data-ttu-id="93f2b-125">Irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="93f2b-125">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="93f2b-126">Diagnosztikai keresés</span><span class="sxs-lookup"><span data-stu-id="93f2b-126">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="93f2b-127">Metrikák böngészése</span><span class="sxs-lookup"><span data-stu-id="93f2b-127">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="93f2b-128">Analytics-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="93f2b-128">Write Analytics queries</span></span>](app-insights-analytics.md)

