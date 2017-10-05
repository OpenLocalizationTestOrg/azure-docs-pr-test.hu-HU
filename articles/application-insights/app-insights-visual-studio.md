---
title: "Az Azure Application insights szolgáltatással, a Visual Studio-alkalmazások hibakeresését |} Microsoft Docs"
description: "Webalkalmazások teljesítményelemzése és diagnosztikája hibakeresés közben és éles környezetben."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/7/2017
ms.author: bwren
ms.openlocfilehash: e0ac2bf01992520cdbea22a232dc42d678d77c7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a><span data-ttu-id="064cf-103">Az alkalmazások az Azure Application insights szolgáltatással, a Visual Studio hibakeresési</span><span class="sxs-lookup"><span data-stu-id="064cf-103">Debug your applications with Azure Application Insights in Visual Studio</span></span>
<span data-ttu-id="064cf-104">A Visual Studio 2015-ös és újabb verzióiban elemezheti az ASP.NET-webalkalmazások teljesítményét és diagnosztizálhatja a problémákat hibakeresés közben és éles környezetben is az [Azure Application Insights](app-insights-overview.md) telemetriájával.</span><span class="sxs-lookup"><span data-stu-id="064cf-104">In Visual Studio (2015 and later), you can analyze performance and diagnose issues in your ASP.NET web app both in debugging and in production, using telemetry from [Azure Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="064cf-105">Ha az ASP.NET-webalkalmazást a Visual Studio 2017-es vagy újabb verziójával hozta létre, már a részét képezi az Application Insights SDK.</span><span class="sxs-lookup"><span data-stu-id="064cf-105">If you created your ASP.NET web app using Visual Studio 2017 or later, it already has the Application Insights SDK.</span></span> <span data-ttu-id="064cf-106">Korábbi verziók esetén, ha még nem tette meg, [adja hozzá az alkalmazáshoz az Application Insights-t](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="064cf-106">Otherwise, if you haven't done so already, [add Application Insights to your app](app-insights-asp-net.md).</span></span>

<span data-ttu-id="064cf-107">Az alkalmazás éles környezetben végzett megfigyeléséhez általában az [Azure Portalon](https://portal.azure.com) tekintheti meg az Application Insights Telemetriát, ahol riasztásokat állíthat be és hatékony megfigyelő eszközöket alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="064cf-107">To monitor your app when it's in live production, you normally view the Application Insights telemetry in the [Azure portal](https://portal.azure.com), where you can set alerts and apply powerful monitoring tools.</span></span> <span data-ttu-id="064cf-108">Hibakereséshez azonban a Visual Studióban is megkeresheti és elemezheti a telemetriát.</span><span class="sxs-lookup"><span data-stu-id="064cf-108">But for debugging, you can also search and analyze the telemetry in Visual Studio.</span></span> <span data-ttu-id="064cf-109">Visual Studio segítségével telemetriai adatok elemzése a munkakörnyezeti helyet a pedig a hibakeresés fut a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="064cf-109">You can use Visual Studio to analyze telemetry both from your production site and from debugging runs on your development machine.</span></span> <span data-ttu-id="064cf-110">Az utóbbi esetben akkor is elemezhet hibakeresési futtatásokat, ha még nem konfigurálta az SDK-t, hogy a telemetriát az Azure Portalra továbbítsa.</span><span class="sxs-lookup"><span data-stu-id="064cf-110">In the latter case, you can analyze debugging runs even if you haven't yet configured the SDK to send telemetry to the Azure portal.</span></span> 

## <span data-ttu-id="064cf-111"><a name="run"></a> Hibakeresés a projektben</span><span class="sxs-lookup"><span data-stu-id="064cf-111"><a name="run"></a> Debug your project</span></span>
<span data-ttu-id="064cf-112">Futtassa a webalkalmazást helyi hibakeresési módban az F5 billentyűvel.</span><span class="sxs-lookup"><span data-stu-id="064cf-112">Run your web app in local debug mode by using F5.</span></span> <span data-ttu-id="064cf-113">Nyisson meg több lapot, hogy létrejöjjön valamennyi telemetria.</span><span class="sxs-lookup"><span data-stu-id="064cf-113">Open different pages to generate some telemetry.</span></span>

<span data-ttu-id="064cf-114">A Visual Studióban tekintse meg a projekt Application Insights-modul által naplózott eseményeket számát.</span><span class="sxs-lookup"><span data-stu-id="064cf-114">In Visual Studio, you see a count of the events that have been logged by the Application Insights module in your project.</span></span>

![A Visual Studióban megjelenik az Application Insights gomb a hibakeresés alatt.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

<span data-ttu-id="064cf-116">Erre a gombra kattintva kereshet a telemetriában.</span><span class="sxs-lookup"><span data-stu-id="064cf-116">Click this button to search your telemetry.</span></span> 

## <a name="application-insights-search"></a><span data-ttu-id="064cf-117">Application Insights-keresés</span><span class="sxs-lookup"><span data-stu-id="064cf-117">Application Insights search</span></span>
<span data-ttu-id="064cf-118">Az Application Insights Keresés ablaka megjeleníti a naplózott eseményeket.</span><span class="sxs-lookup"><span data-stu-id="064cf-118">The Application Insights Search window shows events that have been logged.</span></span> <span data-ttu-id="064cf-119">(Ha regisztrált Azure Application Insights beállításakor, kereshet a azonos események az Azure portálon.)</span><span class="sxs-lookup"><span data-stu-id="064cf-119">(If you signed in to Azure when you set up Application Insights, you can search the same events in the Azure portal.)</span></span>

![Kattintson a jobb gombbal a projektre, és válassza az Application Insights, Keresés lehetőséget.](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> <span data-ttu-id="064cf-121">A szűrők bejelölése vagy kijelölésük törlése után kattintson a szöveges keresőmező végénél található Keresés gombra.</span><span class="sxs-lookup"><span data-stu-id="064cf-121">After you select or deselect filters, click the Search button at the end of the text search field.</span></span>
>

<span data-ttu-id="064cf-122">A szabad szöveges keresés az események minden mezőjén használható.</span><span class="sxs-lookup"><span data-stu-id="064cf-122">The free text search works on any fields in the events.</span></span> <span data-ttu-id="064cf-123">Kereshet például egy oldal URL-jének egy részére, egy tulajdonság egy értékére (pl. az ügyfél városa) vagy egy nyomkövetési napló adott szavaira.</span><span class="sxs-lookup"><span data-stu-id="064cf-123">For example, search for part of the URL of a page; or the value of a property such as client city; or specific words in a trace log.</span></span>

<span data-ttu-id="064cf-124">A részletes tulajdonságok megtekintéséhez kattintson egy eseményre.</span><span class="sxs-lookup"><span data-stu-id="064cf-124">Click any event to see its detailed properties.</span></span>

<span data-ttu-id="064cf-125">A webalkalmazáshoz tartozó kérelmekhez végigkattinthat a kódon.</span><span class="sxs-lookup"><span data-stu-id="064cf-125">For requests to your web app, you can click through to the code.</span></span>

![A Request Details (Adatok kérése) alatt kattintson végig a kódon](./media/app-insights-visual-studio/31.png)

<span data-ttu-id="064cf-127">A kapcsolódó elemeket is megnyithatja a sikertelen kérések vagy a kivételek diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="064cf-127">You can also open related items to help diagnose failed requests or exceptions.</span></span>

![A Request Details (Adatok kérése) alatt görgessen le a vonatkozó elemekhez](./media/app-insights-visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a><span data-ttu-id="064cf-129">Kivételek megtekintése és a sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="064cf-129">View exceptions and failed requests</span></span>
<span data-ttu-id="064cf-130">A keresési ablakban megjelennek a kivételekről szóló jelentések.</span><span class="sxs-lookup"><span data-stu-id="064cf-130">Exception reports show in the Search window.</span></span> <span data-ttu-id="064cf-131">(Az ASP.NET alkalmazás néhány régebbi típusában [be kell állítania a kivételfigyelést](app-insights-asp-net-exceptions.md) a keretrendszer által kezelt kivételek megtekintéséhez.)</span><span class="sxs-lookup"><span data-stu-id="064cf-131">(In some older types of ASP.NET application, you have to [set up exception monitoring](app-insights-asp-net-exceptions.md) to see exceptions that are handled by the framework.)</span></span>

<span data-ttu-id="064cf-132">Híváslánc lekéréséhez kattintson egy kivételre.</span><span class="sxs-lookup"><span data-stu-id="064cf-132">Click an exception to get a stack trace.</span></span> <span data-ttu-id="064cf-133">Ha az alkalmazás kódja meg van nyitva a Visual Studióban, a hívásláncból végigkattinthat a kód megfelelő soráig.</span><span class="sxs-lookup"><span data-stu-id="064cf-133">If the code of the app is open in Visual Studio, you can click through from the stack trace to the relevant line of the code.</span></span>

![Kivétel híváslánca](./media/app-insights-visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-the-code"></a><span data-ttu-id="064cf-135">A kód a kérelem és a kivétel összesítéseket</span><span class="sxs-lookup"><span data-stu-id="064cf-135">View request and exception summaries in the code</span></span>
<span data-ttu-id="064cf-136">A kód fókuszban sorban fölött mindegyik eseménykezelő metódus tekintse meg a kérelmek és az Application Insights az elmúlt 24 órás naplózásáról kivételek számát.</span><span class="sxs-lookup"><span data-stu-id="064cf-136">In the Code Lens line above each handler method, you see a count of the requests and exceptions logged by Application Insights in the past 24 h.</span></span>

![Kivétel híváslánca](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> <span data-ttu-id="064cf-138">A Code Lens csak akkor jeleníti meg az Application Insights adatait, ha [úgy konfigurálta az alkalmazást, hogy telemetriát küldjön az Application Insights portálra](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="064cf-138">Code Lens shows Application Insights data only if you have [configured your app to send telemetry to the Application Insights portal](app-insights-asp-net.md).</span></span>
>

[<span data-ttu-id="064cf-139">További információk az Application Insights-telemetriáról a Code Lensben</span><span class="sxs-lookup"><span data-stu-id="064cf-139">More about Application Insights in Code Lens</span></span>](app-insights-visual-studio-codelens.md)

## <a name="trends"></a><span data-ttu-id="064cf-140">Trendek</span><span class="sxs-lookup"><span data-stu-id="064cf-140">Trends</span></span>
<span data-ttu-id="064cf-141">A Trendek használatával megjelenítheti az alkalmazás időbeni működésének a módját.</span><span class="sxs-lookup"><span data-stu-id="064cf-141">Trends is a tool for visualizing how your app behaves over time.</span></span> 

<span data-ttu-id="064cf-142">Válassza az **Explore Telemetry Trends** (Telemetriatrendek megtekintése) elemet az Application Insights eszköztárgombjáról vagy az Application Insights Keresés ablakában.</span><span class="sxs-lookup"><span data-stu-id="064cf-142">Choose **Explore Telemetry Trends** from the Application Insights toolbar button or Application Insights Search window.</span></span> <span data-ttu-id="064cf-143">A kezdéshez válasszon egyet az öt gyakori lekérdezés közül.</span><span class="sxs-lookup"><span data-stu-id="064cf-143">Choose one of five common queries to get started.</span></span> <span data-ttu-id="064cf-144">A különböző adatkészleteket telemetriatípusok, időintervallumok és egyéb tulajdonságok szerint elemezheti.</span><span class="sxs-lookup"><span data-stu-id="064cf-144">You can analyze different datasets based on telemetry types, time ranges, and other properties.</span></span> 

<span data-ttu-id="064cf-145">Az adatokban előforduló rendellenességek felderítéséhez válassza valamelyik rendellenességi lehetőséget a „View Type” (Nézettípus) legördülő menüben.</span><span class="sxs-lookup"><span data-stu-id="064cf-145">To find anomalies in your data, choose one of the anomaly options under the "View Type" dropdown.</span></span> <span data-ttu-id="064cf-146">Az ablak alján található szűrőbeállítások megkönnyítik a telemetria bizonyos részhalmazainak alaposabb vizsgálatát.</span><span class="sxs-lookup"><span data-stu-id="064cf-146">The filtering options at the bottom of the window make it easy to hone in on specific subsets of your telemetry.</span></span>

![Trendek](./media/app-insights-visual-studio/51.png)

<span data-ttu-id="064cf-148">[További információ a Trendekről](app-insights-visual-studio-trends.md).</span><span class="sxs-lookup"><span data-stu-id="064cf-148">[More about Trends](app-insights-visual-studio-trends.md).</span></span>

## <a name="local-monitoring"></a><span data-ttu-id="064cf-149">Helyi figyelés</span><span class="sxs-lookup"><span data-stu-id="064cf-149">Local monitoring</span></span>
<span data-ttu-id="064cf-150">(A Visual Studio 2015 Update 2) Ha még nincs konfigurálva a telemetriai adatokat küldhet az Application Insights portáljáról (úgy, hogy nincs instrumentation kulcs applicationinsights.config) SDK a diagnosztika ablak a legújabb hibakeresési munkamenetben telemetriai adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="064cf-150">(From Visual Studio 2015 Update 2) If you haven't configured the SDK to send telemetry to the Application Insights portal (so that there is no instrumentation key in ApplicationInsights.config) then the diagnostics window displays telemetry from your latest debugging session.</span></span> 

<span data-ttu-id="064cf-151">Ez akkor javasolt, ha már közzétette az alkalmazás korábbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="064cf-151">This is desirable if you have already published a previous version of your app.</span></span> <span data-ttu-id="064cf-152">Az nem lenne szerencsés, ha a hibakeresési munkamenetek telemetriája összekeveredjen a közzétett alkalmazás Application Insights portálján lévő telemetriával.</span><span class="sxs-lookup"><span data-stu-id="064cf-152">You don't want the telemetry from your debugging sessions to be mixed up with the telemetry on the Application Insights portal from the published app.</span></span>

<span data-ttu-id="064cf-153">Az is hasznos, ha van [egyéni telemetriája](app-insights-api-custom-events-metrics.md), amelyen hibakeresést végez, mielőtt elküldené a telemetriát a portálra.</span><span class="sxs-lookup"><span data-stu-id="064cf-153">It's also useful if you have some [custom telemetry](app-insights-api-custom-events-metrics.md) that you want to debug before sending telemetry to the portal.</span></span>

* <span data-ttu-id="064cf-154">*Először teljes körűen konfiguráltam az Application Insights szolgáltatást, hogy küldjön telemetriát a portálra. Most azonban azt szeretném, hogy a telemetria csak a Visual Studióban jelenjen meg.*</span><span class="sxs-lookup"><span data-stu-id="064cf-154">*At first, I fully configured Application Insights to send telemetry to the portal. But now I'd like to see the telemetry only in Visual Studio.*</span></span>
  
  * <span data-ttu-id="064cf-155">A Keresés ablak Beállításai között lehetősége van a helyi diagnosztika keresésére még akkor is, ha az alkalmazás elküldi a telemetriát a portálra.</span><span class="sxs-lookup"><span data-stu-id="064cf-155">In the Search window's Settings, there's an option to search local diagnostics even if your app sends telemetry to the portal.</span></span>
  * <span data-ttu-id="064cf-156">Ha nem akarja, hogy a rendszer elküldje a telemetriát a portálra, tegye megjegyzésbe az `<instrumentationkey>...` sort az ApplicationInsights.config fájlban. Ha azt szeretné, hogy a rendszer megint elküldje a telemetriát a portálra, állítsa vissza a kódot.</span><span class="sxs-lookup"><span data-stu-id="064cf-156">To stop telemetry being sent to the portal, comment out the line `<instrumentationkey>...` from ApplicationInsights.config. When you're ready to send telemetry to the portal again, uncomment it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="064cf-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="064cf-157">Next steps</span></span>
|  |  |
| --- | --- |
| <span data-ttu-id="064cf-158">**[További adatok hozzáadása](app-insights-asp-net-more.md)**</span><span class="sxs-lookup"><span data-stu-id="064cf-158">**[Add more data](app-insights-asp-net-more.md)**</span></span><br/><span data-ttu-id="064cf-159">Figyelheti a használatot, az elérhetőséget, a függőségeket és a kivételeket.</span><span class="sxs-lookup"><span data-stu-id="064cf-159">Monitor usage, availability, dependencies, exceptions.</span></span> <span data-ttu-id="064cf-160">Integrálhatja a nyomkövetéseket naplózási keretrendszerekből.</span><span class="sxs-lookup"><span data-stu-id="064cf-160">Integrate traces from logging frameworks.</span></span> <span data-ttu-id="064cf-161">Egyéni telemetriát írhat.</span><span class="sxs-lookup"><span data-stu-id="064cf-161">Write custom telemetry.</span></span> |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| <span data-ttu-id="064cf-163">**[Az Application Insights-portál használata](app-insights-dashboards.md)**</span><span class="sxs-lookup"><span data-stu-id="064cf-163">**[Working with the Application Insights portal](app-insights-dashboards.md)**</span></span><br/><span data-ttu-id="064cf-164">Irányítópultok, hatékony elemzési és diagnosztikai eszközöket, riasztások, egy élő kapcsolatfüggőségi térképének megjelenítéséhez az alkalmazás, és exportált telemetriai adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="064cf-164">View dashboards, powerful diagnostic and analytic tools, alerts, a live dependency map of your application, and exported telemetry data.</span></span> |![Visual Studio](./media/app-insights-visual-studio/62.png) |

