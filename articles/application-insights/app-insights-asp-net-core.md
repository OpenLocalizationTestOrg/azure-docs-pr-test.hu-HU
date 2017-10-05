---
title: Az Azure Application Insights for ASP.NET Core |} Microsoft Docs
description: "Webalkalmazások rendelkezésre állását, teljesítményét és használatának a figyelheti."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: d86495eea467977f6c079de72e2b49a2a1da2b60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-for-aspnet-core"></a><span data-ttu-id="5ce73-103">ASP.Net Core-hoz készült Application Insights</span><span class="sxs-lookup"><span data-stu-id="5ce73-103">Application Insights for ASP.NET Core</span></span>
<span data-ttu-id="5ce73-104">[Az Application Insights](app-insights-overview.md) lehetővé teszi, hogy a webalkalmazás rendelkezésre állását, teljesítményét és használatának figyelése.</span><span class="sxs-lookup"><span data-stu-id="5ce73-104">[Application Insights](app-insights-overview.md) lets you monitor your web application for availability, performance and usage.</span></span> <span data-ttu-id="5ce73-105">A széles körben elérhető módon működő alkalmazások teljesítményével és hatékonyságával kapcsolatos visszajelzések birtokában tájékozott döntéseket hozhat a fejlesztés irányát illetően az egyes fejlesztési fázisokban.</span><span class="sxs-lookup"><span data-stu-id="5ce73-105">With the feedback you get about the performance and effectiveness of your app in the wild, you can make informed choices about the direction of the design in each development lifecycle.</span></span>

![Példa](./media/app-insights-asp-net-core/sample.png)

<span data-ttu-id="5ce73-107">Szüksége lesz egy előfizetés [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="5ce73-107">You'll need a subscription with [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="5ce73-108">Jelentkezzen be egy Microsoft-fiókkal – ez tartozhat a Windowshoz, az XBox Live-hoz vagy egyéb Microsoft felhőszolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5ce73-108">Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services.</span></span> <span data-ttu-id="5ce73-109">A csoport lehet egy szervezeti Azure-előfizetéssel: a nyilvános venni azt a Microsoft-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="5ce73-109">Your team might have an organizational subscription to Azure: ask the owner to add you to it using your Microsoft account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="5ce73-110">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5ce73-110">Getting started</span></span>

* <span data-ttu-id="5ce73-111">A Visual Studio Solution Explorerben kattintson jobb gombbal a projektre, és válassza ki **konfigurálja az Application Insights**, vagy **Hozzáadás > Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="5ce73-111">In Visual Studio Solution Explorer, right-click your project and select **Configure Application Insights**, or **Add > Application Insights**.</span></span> <span data-ttu-id="5ce73-112">[További információk](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="5ce73-112">[Learn more](app-insights-asp-net.md).</span></span>
* <span data-ttu-id="5ce73-113">Ha nem látja ezeket menüparancsai, kövesse a [manuális első lépések útmutató](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span><span class="sxs-lookup"><span data-stu-id="5ce73-113">If you don't see those menu commands, follow the [manual getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span></span> <span data-ttu-id="5ce73-114">Erre a projekt egy Visual Studio verziója előtt 2017 hozták létre, ha szeretne.</span><span class="sxs-lookup"><span data-stu-id="5ce73-114">You may need to do this if your project was created with a version of Visual Studio before 2017.</span></span>

## <a name="using-application-insights"></a><span data-ttu-id="5ce73-115">Az Application Insights használata</span><span class="sxs-lookup"><span data-stu-id="5ce73-115">Using Application Insights</span></span>
<span data-ttu-id="5ce73-116">Jelentkezzen be a [Microsoft Azure-portálon](https://portal.azure.com), jelölje be **összes erőforrás** vagy **Application Insights**, majd válassza ki a figyelheti az alkalmazást a létrehozott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5ce73-116">Sign into the [Microsoft Azure portal](https://portal.azure.com), select **All Resources** or **Application Insights**, and then select the resource you created to monitor your app.</span></span>

<span data-ttu-id="5ce73-117">Egy külön böngészőablakban használja az alkalmazást egy ideig.</span><span class="sxs-lookup"><span data-stu-id="5ce73-117">In a separate browser window, use your app for a while.</span></span> <span data-ttu-id="5ce73-118">Láthatja, hogy az Application Insights diagramok szereplő adatok.</span><span class="sxs-lookup"><span data-stu-id="5ce73-118">You'll see data appearing in the Application Insights charts.</span></span> <span data-ttu-id="5ce73-119">(Lehetséges, hogy a frissítés gombra.) Nem lesznek csak kisebb mennyiségű adatot közben kidolgozása, de a diagramokat valóban származnak életben tegye közzé az alkalmazást, és a sok felhasználóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5ce73-119">(You might have to click Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users.</span></span> 

<span data-ttu-id="5ce73-120">A – Áttekintés lapon látható legfontosabb teljesítményi diagramok: kiszolgáló válaszideje, lapbetöltési idő és a sikertelen kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="5ce73-120">The overview page shows key performance charts: server response time,  page load time, and counts of failed requests.</span></span> <span data-ttu-id="5ce73-121">Kattintson bármelyik olyan diagram további diagramok és adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5ce73-121">Click any chart to see more charts and data.</span></span>

<span data-ttu-id="5ce73-122">A portál nézetek három fő kategóriába sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="5ce73-122">Views in the portal fall into three main categories:</span></span>

* <span data-ttu-id="5ce73-123">[Metrikaböngésző](app-insights-metrics-explorer.md) diagramjait és a metrikák és számát, például válaszidejét, hiba díjszabás vagy a saját metrikákat jeleníti meg a [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="5ce73-123">[Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with the [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="5ce73-124">Szűrés, és szegmentálja a data tulajdonság értékével jobban megismerheti a felhasználók és az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5ce73-124">Filter and segment the data by property values to get a better understanding of your app and its users.</span></span>
* <span data-ttu-id="5ce73-125">[Keresés Explorer](app-insights-diagnostic-search.md) események, például bizonyos kérésekre, kivételek, naplókivonatokat vagy magának a létrehozott események felsorolja a [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="5ce73-125">[Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with the [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="5ce73-126">Szűrje és az események kereséséhez, és keresse meg kell vizsgálni a problémákat a kapcsolódó események között.</span><span class="sxs-lookup"><span data-stu-id="5ce73-126">Filter and search in the events, and navigate among related events to investigate issues.</span></span>
* <span data-ttu-id="5ce73-127">[Elemzés](app-insights-analytics.md) lehetővé teszi, hogy az SQL-szerű lekérdezéseket futtatnia a telemetriai adatokat, és a hatékony analitikai és diagnosztikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="5ce73-127">[Analytics](app-insights-analytics.md) lets you run SQL-like queries over your telemetry, and is a powerful analytical and diagnostic tool.</span></span>

## <a name="alerts"></a><span data-ttu-id="5ce73-128">Riasztások</span><span class="sxs-lookup"><span data-stu-id="5ce73-128">Alerts</span></span>
* <span data-ttu-id="5ce73-129">Automatikusan elérhetővé [proaktív diagnosztikai riasztások](app-insights-proactive-diagnostics.md) , amely közli, hiba sebességét és más metrikákkal rendellenes változásaira vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="5ce73-129">You automatically get [proactive diagnostic alerts](app-insights-proactive-diagnostics.md) that tell you about anomalous changes in failure rates and other metrics.</span></span>
* <span data-ttu-id="5ce73-130">Állítson be [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) tesztelje a webhelyet, folyamatosan helyekről világszerte, és e-mailek, ahogy bármely tesztelése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="5ce73-130">Set up [availability tests](app-insights-monitor-web-app-availability.md) to test your website continually from locations worldwide, and get emails as soon as any test fails.</span></span>
* <span data-ttu-id="5ce73-131">Állítson be [metrika riasztások](app-insights-monitor-web-app-availability.md) tudni, hogy ha például a válaszidők vagy kivétel díjszabás metrikák nyissa meg külső elfogadható határértékeket.</span><span class="sxs-lookup"><span data-stu-id="5ce73-131">Set up [metric alerts](app-insights-monitor-web-app-availability.md) to know if metrics such as response times or exception rates go outside acceptable limits.</span></span>

## <a name="video"></a><span data-ttu-id="5ce73-132">Videó</span><span class="sxs-lookup"><span data-stu-id="5ce73-132">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a><span data-ttu-id="5ce73-133">Nyílt forráskód</span><span class="sxs-lookup"><span data-stu-id="5ce73-133">Open source</span></span>
[<span data-ttu-id="5ce73-134">Olvassa el, és hozzájárulnak a kódot</span><span class="sxs-lookup"><span data-stu-id="5ce73-134">Read and contribute to the code</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a><span data-ttu-id="5ce73-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ce73-135">Next steps</span></span>
* <span data-ttu-id="5ce73-136">[Telemetriai adatok felvétele a weblapok](app-insights-javascript.md) a figyelő lap használati és teljesítményadatokat.</span><span class="sxs-lookup"><span data-stu-id="5ce73-136">[Add telemetry to your web pages](app-insights-javascript.md) to monitor page usage and performance.</span></span>
* <span data-ttu-id="5ce73-137">[Függőségek figyelése](app-insights-asp-net-dependencies.md) megjelenítéséhez, ha a többi, SQL vagy más külső erőforrások lassítja meg.</span><span class="sxs-lookup"><span data-stu-id="5ce73-137">[Monitor dependencies](app-insights-asp-net-dependencies.md) to see if REST, SQL or other external resources are slowing you down.</span></span>
* <span data-ttu-id="5ce73-138">[Az API-val](app-insights-api-custom-events-metrics.md) a saját eseményeket és metrikákat az alkalmazás teljesítményét és használatának részletes nézet küldését.</span><span class="sxs-lookup"><span data-stu-id="5ce73-138">[Use the API](app-insights-api-custom-events-metrics.md) to send your own events and metrics for a more detailed view of your app's performance and usage.</span></span>
* <span data-ttu-id="5ce73-139">[Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) ellenőrizze a világszerte folyamatosan az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5ce73-139">[Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around the world.</span></span> 

