---
title: az Application Insights az ASP.NET Core aaaAzure |} Microsoft Docs
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
ms.openlocfilehash: a7a27f9eef1daec5b0deae9fd88906e646980659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-aspnet-core"></a><span data-ttu-id="68ba4-103">ASP.Net Core-hoz készült Application Insights</span><span class="sxs-lookup"><span data-stu-id="68ba4-103">Application Insights for ASP.NET Core</span></span>
<span data-ttu-id="68ba4-104">[Az Application Insights](app-insights-overview.md) lehetővé teszi, hogy a webalkalmazás rendelkezésre állását, teljesítményét és használatának figyelése.</span><span class="sxs-lookup"><span data-stu-id="68ba4-104">[Application Insights](app-insights-overview.md) lets you monitor your web application for availability, performance and usage.</span></span> <span data-ttu-id="68ba4-105">Hello visszajelzést kap hello teljesítmény és az alkalmazást a hello hatékonyságát helyettesítő minden fejlesztési életciklus során hello tervezési hello irányának kapcsolatos megalapozott döntések tehet.</span><span class="sxs-lookup"><span data-stu-id="68ba4-105">With hello feedback you get about hello performance and effectiveness of your app in hello wild, you can make informed choices about hello direction of hello design in each development lifecycle.</span></span>

![Példa](./media/app-insights-asp-net-core/sample.png)

<span data-ttu-id="68ba4-107">Szüksége lesz egy előfizetés [Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="68ba4-107">You'll need a subscription with [Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="68ba4-108">Jelentkezzen be egy Microsoft-fiókkal – ez tartozhat a Windowshoz, az XBox Live-hoz vagy egyéb Microsoft felhőszolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="68ba4-108">Sign in with a Microsoft account, which you might have for Windows, XBox Live, or other Microsoft cloud services.</span></span> <span data-ttu-id="68ba4-109">Előfordulhat, hogy a csoport egy szervezeti előfizetés tooAzure: kérje meg a hello tulajdonos tooadd tooit, a Microsoft-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="68ba4-109">Your team might have an organizational subscription tooAzure: ask hello owner tooadd you tooit using your Microsoft account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="68ba4-110">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="68ba4-110">Getting started</span></span>

* <span data-ttu-id="68ba4-111">A Visual Studio Solution Explorerben kattintson jobb gombbal a projektre, és válassza ki **konfigurálja az Application Insights**, vagy **Hozzáadás > Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="68ba4-111">In Visual Studio Solution Explorer, right-click your project and select **Configure Application Insights**, or **Add > Application Insights**.</span></span> <span data-ttu-id="68ba4-112">[További információk](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="68ba4-112">[Learn more](app-insights-asp-net.md).</span></span>
* <span data-ttu-id="68ba4-113">Ha nem látja ezeket menüparancsai, hajtsa végre a hello [manuális első lépések útmutató](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span><span class="sxs-lookup"><span data-stu-id="68ba4-113">If you don't see those menu commands, follow hello [manual getting Started guide](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).</span></span> <span data-ttu-id="68ba4-114">Ha akkor esetleg toodo Ez a projekt egy Visual Studio verziója előtt 2017 hozták létre.</span><span class="sxs-lookup"><span data-stu-id="68ba4-114">You may need toodo this if your project was created with a version of Visual Studio before 2017.</span></span>

## <a name="using-application-insights"></a><span data-ttu-id="68ba4-115">Az Application Insights használata</span><span class="sxs-lookup"><span data-stu-id="68ba4-115">Using Application Insights</span></span>
<span data-ttu-id="68ba4-116">Jelentkezzen be a hello [Microsoft Azure-portálon](https://portal.azure.com), jelölje be **összes erőforrás** vagy **Application Insights**, majd válassza ki az alkalmazáshoz létrehozott toomonitor hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="68ba4-116">Sign into hello [Microsoft Azure portal](https://portal.azure.com), select **All Resources** or **Application Insights**, and then select hello resource you created toomonitor your app.</span></span>

<span data-ttu-id="68ba4-117">Egy külön böngészőablakban használja az alkalmazást egy ideig.</span><span class="sxs-lookup"><span data-stu-id="68ba4-117">In a separate browser window, use your app for a while.</span></span> <span data-ttu-id="68ba4-118">Látni fogja, hello Application Insights diagramok szereplő adatok.</span><span class="sxs-lookup"><span data-stu-id="68ba4-118">You'll see data appearing in hello Application Insights charts.</span></span> <span data-ttu-id="68ba4-119">(Lehetséges, hogy frissítési tooclick.) Nem lesznek csak kisebb mennyiségű adatot közben kidolgozása, de a diagramokat valóban származnak életben tegye közzé az alkalmazást, és a sok felhasználóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="68ba4-119">(You might have tooclick Refresh.) There will be only a small amount of data while you're developing, but these charts really come alive when you publish your app and have many users.</span></span> 

<span data-ttu-id="68ba4-120">hello – Áttekintés lapon látható legfontosabb teljesítményi diagramok: kiszolgáló válaszideje, lapbetöltési idő és a sikertelen kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="68ba4-120">hello overview page shows key performance charts: server response time,  page load time, and counts of failed requests.</span></span> <span data-ttu-id="68ba4-121">Kattintson a diagram toosee további diagramokat és adatokat.</span><span class="sxs-lookup"><span data-stu-id="68ba4-121">Click any chart toosee more charts and data.</span></span>

<span data-ttu-id="68ba4-122">Nézetek hello portálon három fő kategóriába sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="68ba4-122">Views in hello portal fall into three main categories:</span></span>

* <span data-ttu-id="68ba4-123">[Metrikaböngésző](app-insights-metrics-explorer.md) mutatja, diagramok és a metrikák és számát, például a válaszidőt, hiba díjszabás vagy metrikák Ön hozza létre a hello [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="68ba4-123">[Metrics Explorer](app-insights-metrics-explorer.md) shows graphs and tables of metrics and counts, such as response times, failure rates, or metrics you create yourself with hello [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="68ba4-124">Szűrő és a szegmens hello adatait az alkalmazás és a felhasználóknak szeretné jobban megismerni a tulajdonság értékek tooget.</span><span class="sxs-lookup"><span data-stu-id="68ba4-124">Filter and segment hello data by property values tooget a better understanding of your app and its users.</span></span>
* <span data-ttu-id="68ba4-125">[Keresés Explorer](app-insights-diagnostic-search.md) felsorolja az egyes események, például bizonyos kérésekre, kivételek, naplókivonatokat vagy saját kezűleg hello segítségével létrehozott események [API](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="68ba4-125">[Search Explorer](app-insights-diagnostic-search.md) lists individual events, such as specific requests, exceptions, log traces, or events you created yourself with hello [API](app-insights-api-custom-events-metrics.md).</span></span> <span data-ttu-id="68ba4-126">Szűrje és hello események kereséséhez, és keresse meg a kapcsolódó események tooinvestigate problémák között.</span><span class="sxs-lookup"><span data-stu-id="68ba4-126">Filter and search in hello events, and navigate among related events tooinvestigate issues.</span></span>
* <span data-ttu-id="68ba4-127">[Elemzés](app-insights-analytics.md) lehetővé teszi, hogy az SQL-szerű lekérdezéseket futtatnia a telemetriai adatokat, és a hatékony analitikai és diagnosztikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="68ba4-127">[Analytics](app-insights-analytics.md) lets you run SQL-like queries over your telemetry, and is a powerful analytical and diagnostic tool.</span></span>

## <a name="alerts"></a><span data-ttu-id="68ba4-128">Riasztások</span><span class="sxs-lookup"><span data-stu-id="68ba4-128">Alerts</span></span>
* <span data-ttu-id="68ba4-129">Automatikusan elérhetővé [proaktív diagnosztikai riasztások](app-insights-proactive-diagnostics.md) , amely közli, hiba sebességét és más metrikákkal rendellenes változásaira vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="68ba4-129">You automatically get [proactive diagnostic alerts](app-insights-proactive-diagnostics.md) that tell you about anomalous changes in failure rates and other metrics.</span></span>
* <span data-ttu-id="68ba4-130">Állítson be [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) tootest a webhely folyamatosan világszerte helyeket, és az e-mailek, ahogy bármely tesztelése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="68ba4-130">Set up [availability tests](app-insights-monitor-web-app-availability.md) tootest your website continually from locations worldwide, and get emails as soon as any test fails.</span></span>
* <span data-ttu-id="68ba4-131">Állítson be [metrika riasztások](app-insights-monitor-web-app-availability.md) tooknow, ha például a válaszidők vagy kivétel díjszabás metrikák külső elfogadható határértékeket.</span><span class="sxs-lookup"><span data-stu-id="68ba4-131">Set up [metric alerts](app-insights-monitor-web-app-availability.md) tooknow if metrics such as response times or exception rates go outside acceptable limits.</span></span>

## <a name="video"></a><span data-ttu-id="68ba4-132">Videó</span><span class="sxs-lookup"><span data-stu-id="68ba4-132">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a><span data-ttu-id="68ba4-133">Nyílt forráskód</span><span class="sxs-lookup"><span data-stu-id="68ba4-133">Open source</span></span>
[<span data-ttu-id="68ba4-134">Olvassa el és toohello kód közreműködési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="68ba4-134">Read and contribute toohello code</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a><span data-ttu-id="68ba4-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="68ba4-135">Next steps</span></span>
* <span data-ttu-id="68ba4-136">[Adja hozzá a telemetriai adatok tooyour weblapok](app-insights-javascript.md) toomonitor lapon használati és teljesítményadatokat.</span><span class="sxs-lookup"><span data-stu-id="68ba4-136">[Add telemetry tooyour web pages](app-insights-javascript.md) toomonitor page usage and performance.</span></span>
* <span data-ttu-id="68ba4-137">[Függőségek figyelése](app-insights-asp-net-dependencies.md) toosee, ha a többi, SQL vagy más külső erőforrások lassítja meg.</span><span class="sxs-lookup"><span data-stu-id="68ba4-137">[Monitor dependencies](app-insights-asp-net-dependencies.md) toosee if REST, SQL or other external resources are slowing you down.</span></span>
* <span data-ttu-id="68ba4-138">[Hello API-t használó](app-insights-api-custom-events-metrics.md) toosend saját eseményeket és metrikákat az alkalmazás teljesítményét és használatának részletes nézet.</span><span class="sxs-lookup"><span data-stu-id="68ba4-138">[Use hello API](app-insights-api-custom-events-metrics.md) toosend your own events and metrics for a more detailed view of your app's performance and usage.</span></span>
* <span data-ttu-id="68ba4-139">[Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) ellenőrizze az alkalmazását folyamatosan hello world körül.</span><span class="sxs-lookup"><span data-stu-id="68ba4-139">[Availability tests](app-insights-monitor-web-app-availability.md) check your app constantly from around hello world.</span></span> 

