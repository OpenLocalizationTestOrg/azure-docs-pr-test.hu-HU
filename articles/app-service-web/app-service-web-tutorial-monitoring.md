---
title: "A webes alkalmazás figyelése |} Microsoft Docs"
description: "Útmutató: a webalkalmazás a figyelés beállítása"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: 29df824062d00e01b786533033097948c008588f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-app-service"></a><span data-ttu-id="a2d44-103">Az App Service figyelése</span><span class="sxs-lookup"><span data-stu-id="a2d44-103">Monitor App Service</span></span>
<span data-ttu-id="a2d44-104">Ez az oktatóanyag végigvezeti az alkalmazás figyelése és a beépített platform eszközökkel kapcsolatos problémák megoldásához, azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="a2d44-104">This tutorial walks you through monitoring your app and using the built-in platform tools to solve problems when they occur.</span></span>

<span data-ttu-id="a2d44-105">Ez a dokumentum egyes részeinek kerül egy adott funkcióhoz keresztül.</span><span class="sxs-lookup"><span data-stu-id="a2d44-105">Each section of this document goes over a specific feature.</span></span> <span data-ttu-id="a2d44-106">A szolgáltatások együttes használata lehetővé teszik, hogy:</span><span class="sxs-lookup"><span data-stu-id="a2d44-106">Using the features together let you:</span></span>
- <span data-ttu-id="a2d44-107">Az alkalmazás problémát azonosítása.</span><span class="sxs-lookup"><span data-stu-id="a2d44-107">Identifying an issue in your app.</span></span>
- <span data-ttu-id="a2d44-108">Annak meghatározása, ha a hibát az okozza a kódban, illetve a platform.</span><span class="sxs-lookup"><span data-stu-id="a2d44-108">Determining when the issue is caused by your code or the platform.</span></span>
- <span data-ttu-id="a2d44-109">Szűkítse le a kódban a probléma forrása.</span><span class="sxs-lookup"><span data-stu-id="a2d44-109">Narrow down the source of the problem in your code.</span></span>
- <span data-ttu-id="a2d44-110">Hibakeresés és a probléma kijavítása.</span><span class="sxs-lookup"><span data-stu-id="a2d44-110">Debugging and fixing the issue.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a2d44-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="a2d44-111">Before you begin</span></span>
- <span data-ttu-id="a2d44-112">Szüksége van a webes alkalmazás figyelésére, és kövesse az itt ismertetett lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a2d44-112">You need a Web App to monitor and follow the outlined steps.</span></span>
    - <span data-ttu-id="a2d44-113">Létrehozhat egy alkalmazást a következő helyen ismertetett lépések a [ASP.NET-alkalmazás létrehozása az Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a2d44-113">You can create an application following the steps described in the [Create an ASP.NET app in Azure with SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) tutorial.</span></span>

- <span data-ttu-id="a2d44-114">Ha azt szeretné, hogy próbálja ki **távoli hibakeresés** az alkalmazás, a Visual Studio szüksége.</span><span class="sxs-lookup"><span data-stu-id="a2d44-114">If you want to try out **Remote Debugging** of your application, you need Visual Studio.</span></span>
    - <span data-ttu-id="a2d44-115">Ha még nincs telepítve a Visual Studio 2017, töltse le, és az ingyenes [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a2d44-115">If you don’t already have Visual Studio 2017 installed, you can download and use the free [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span>
    - <span data-ttu-id="a2d44-116">Ügyeljen arra, hogy engedélyezze az **Azure Development** használatát a Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="a2d44-116">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

## <span data-ttu-id="a2d44-117"><a name="metrics"></a>1. lépés – nézet metrikák</span><span class="sxs-lookup"><span data-stu-id="a2d44-117"><a name="metrics"></a> Step 1 - View metrics</span></span>
<span data-ttu-id="a2d44-118">**Metrikák** működésének megértése:</span><span class="sxs-lookup"><span data-stu-id="a2d44-118">**Metrics** are useful to understand:</span></span>
- <span data-ttu-id="a2d44-119">Alkalmazás állapotának</span><span class="sxs-lookup"><span data-stu-id="a2d44-119">Application health</span></span>
- <span data-ttu-id="a2d44-120">Alkalmazás teljesítménye</span><span class="sxs-lookup"><span data-stu-id="a2d44-120">App performance</span></span>
- <span data-ttu-id="a2d44-121">Erőforrás-használat</span><span class="sxs-lookup"><span data-stu-id="a2d44-121">Resource consumption</span></span>

<span data-ttu-id="a2d44-122">Az alkalmazás problémát vizsgál, tekintse át a metrikákat esetén remek kezdőpont.</span><span class="sxs-lookup"><span data-stu-id="a2d44-122">When investigating an application issue, reviewing metrics is a good place to start.</span></span> <span data-ttu-id="a2d44-123">Azure-portálon gyorsan vizuálisan vizsgálja meg az alkalmazás használatával metrikáinak rendelkezik **Azure figyelő**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-123">Azure portal has a quick way to visually inspect the metrics of your app using **Azure Monitor**.</span></span>

<span data-ttu-id="a2d44-124">Metrikákat biztosít az alkalmazás több kulcs összesítésben állapotelőzményeinek a nézetét.</span><span class="sxs-lookup"><span data-stu-id="a2d44-124">Metrics provide a historical view across several key aggregations for your app.</span></span> <span data-ttu-id="a2d44-125">Bármely alkalmazás az app service-ben üzemeltetett figyelje, hogy mind a Web App és az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="a2d44-125">For any app hosted in app service, you should monitor both the Web App and the App Service plan.</span></span>

> [!NOTE]
> <span data-ttu-id="a2d44-126">Az App Service-csomagok az alkalmazások üzemeltetéséhez használt fizikai erőforrások gyűjteményét jelölik.</span><span class="sxs-lookup"><span data-stu-id="a2d44-126">An App Service plan represents the collection of physical resources used to host your apps.</span></span> <span data-ttu-id="a2d44-127">Egy App Service-csomaghoz rendelt összes alkalmazás ugyanazokat az általa meghatározott erőforrásokat használja, így több alkalmazás üzemeltetése esetén is csökkenthetők a költségek.</span><span class="sxs-lookup"><span data-stu-id="a2d44-127">All applications assigned to an App Service plan share the resources defined by it allowing you to save cost when hosting multiple apps.</span></span>
>
> <span data-ttu-id="a2d44-128">Az App Service-csomagok a következőket határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="a2d44-128">App Service plans define:</span></span>
> * <span data-ttu-id="a2d44-129">Régió: Észak-Európa, USA keleti régiója, Délkelet-Ázsiában, stb.</span><span class="sxs-lookup"><span data-stu-id="a2d44-129">Region: North Europe, East US, Southeast Asia, etc.</span></span>
> * <span data-ttu-id="a2d44-130">Példány mérete: Kicsi, közepes, nagy, stb.</span><span class="sxs-lookup"><span data-stu-id="a2d44-130">Instance Size: Small, Medium, Large, etc.</span></span>
> * <span data-ttu-id="a2d44-131">Méretezési száma: egy, kettő vagy három példányt, stb.</span><span class="sxs-lookup"><span data-stu-id="a2d44-131">Scale Count: one, two, or three instances, etc.</span></span>
> * <span data-ttu-id="a2d44-132">Termékváltozat: Ingyenes, a közös, Basic, Standard, Premium, stb.</span><span class="sxs-lookup"><span data-stu-id="a2d44-132">SKU: Free, Shared, Basic, Standard, Premium, etc.</span></span>

<span data-ttu-id="a2d44-133">Tekintse át a metrikákat a webalkalmazás, keresse fel a **áttekintése** a figyelni kívánt alkalmazás panelen.</span><span class="sxs-lookup"><span data-stu-id="a2d44-133">To review metrics for your Web App, go to the **Overview** blade of the app you want to monitor.</span></span> <span data-ttu-id="a2d44-134">Itt megtekintheti, az alkalmazás metrikák diagramot egy **figyelés csempe**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-134">From here, you can view a chart for your app's metrics as a **Monitoring tile**.</span></span> <span data-ttu-id="a2d44-135">Kattintson a csempére kattintva szerkesszék és konfigurálják milyen mérőszámok alapján megtekintheti és a megjelenítendő időtartományát.</span><span class="sxs-lookup"><span data-stu-id="a2d44-135">Click the tile to edit and configure what metrics to view and the time range to display.</span></span>

<span data-ttu-id="a2d44-136">Alapértelmezés szerint az erőforrás panel az tartalmazza az alkalmazás kérések és hibák követése az elmúlt egy óra.</span><span class="sxs-lookup"><span data-stu-id="a2d44-136">By default the resource blade provides a view for the Application Requests and errors for the last hour.</span></span>
<span data-ttu-id="a2d44-137">![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span><span class="sxs-lookup"><span data-stu-id="a2d44-137">![Monitor App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span></span>

<span data-ttu-id="a2d44-138">Ahogy látja, a példában, kell, hogy a számos alkalmazás **HTTP-kiszolgáló hibák**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-138">As you can see in the example, we have an application that is generating many **HTTP Server Errors**.</span></span> <span data-ttu-id="a2d44-139">A nagy mennyiségű hibák igazolnia kell a vizsgálja meg az alkalmazás első jelzi.</span><span class="sxs-lookup"><span data-stu-id="a2d44-139">The high volume of errors is the first indication we need to investigate this application.</span></span>

> [!TIP]
> <span data-ttu-id="a2d44-140">További tudnivalók az Azure-figyelő a következő hivatkozásokkal:</span><span class="sxs-lookup"><span data-stu-id="a2d44-140">Learn more about Azure Monitor with the following links:</span></span>
> - [<span data-ttu-id="a2d44-141">Ismerkedés az Azure-figyelő</span><span class="sxs-lookup"><span data-stu-id="a2d44-141">Get started with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="a2d44-142">Az Azure metrikák</span><span class="sxs-lookup"><span data-stu-id="a2d44-142">Azure Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [<span data-ttu-id="a2d44-143">Azure-figyelő támogatott metrikák</span><span class="sxs-lookup"><span data-stu-id="a2d44-143">Supported metrics with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [<span data-ttu-id="a2d44-144">Az Azure irányítópultok</span><span class="sxs-lookup"><span data-stu-id="a2d44-144">Azure Dashboards</span></span>](..\azure-portal\azure-portal-dashboards.md)

## <span data-ttu-id="a2d44-145"><a name="alerts"></a>2. lépés - riasztások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a2d44-145"><a name="alerts"></a> Step 2 - Configure Alerts</span></span>
<span data-ttu-id="a2d44-146">**Riasztások** konfigurálható elindítani az alkalmazás bizonyos feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="a2d44-146">**Alerts** can be configured to trigger on specific conditions for your app.</span></span>

<span data-ttu-id="a2d44-147">A [1. lépés - nézet metrikák](#metrics), látott, hogy az alkalmazás volt-e a hibák túl magas száma.</span><span class="sxs-lookup"><span data-stu-id="a2d44-147">In [Step 1 - View metrics](#metrics), we saw that the application had a high number of errors.</span></span>

<span data-ttu-id="a2d44-148">Lehetővé teszi, hogy automatikusan tájékoztatást kaphat a hibák esetén olyan riasztást konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-148">Lets configure an alert to automatically get notified when errors occur.</span></span> <span data-ttu-id="a2d44-149">Ebben az esetben azt szeretnénk, ha a riasztás küldése és minden alkalommal, amikor egy meghatározott küszöbérték feletti kerül X HTTP 50-hibák száma.</span><span class="sxs-lookup"><span data-stu-id="a2d44-149">In this case, we want the alert to send and e-mail every time the number of HTTP 50X errors goes over a certain threshold.</span></span>

<span data-ttu-id="a2d44-150">Riasztás létrehozása, navigáljon a **figyelés** > **riasztások** kattintson **[+] riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-150">To create an alert, navigate to **Monitoring** > **Alerts** and click **[+] Add Alert**.</span></span>

![Riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

<span data-ttu-id="a2d44-152">Adjon meg értékeket a riasztás konfigurálásában:</span><span class="sxs-lookup"><span data-stu-id="a2d44-152">Provide values for the Alert configuration:</span></span>
- <span data-ttu-id="a2d44-153">**Erőforrás:** a helyet, hogy a riasztást figyelő.</span><span class="sxs-lookup"><span data-stu-id="a2d44-153">**Resource:** The site to monitor with the alert.</span></span>
- <span data-ttu-id="a2d44-154">**Name:** egy nevet a riasztáshoz, ebben az esetben: *magas HTTP 50 X*.</span><span class="sxs-lookup"><span data-stu-id="a2d44-154">**Name:** A name for your alert, in this case: *High HTTP 50X*.</span></span>
- <span data-ttu-id="a2d44-155">**Leírás:** egyszerű szöveges magyarázatát, mi van megnézi a riasztás.</span><span class="sxs-lookup"><span data-stu-id="a2d44-155">**Description:** Plain text explanation of what this alert is looking at.</span></span>
- <span data-ttu-id="a2d44-156">**Riasztás:** riasztások metrikák vagy események is megtekinthetik, ehhez a példához azt nézi metrikákat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-156">**Alert on:** Alerts can look at Metrics or Events, for this example we are looking at metrics.</span></span>
- <span data-ttu-id="a2d44-157">**A metrika:** milyen metrika figyeléséhez, ebben az esetben: *HTTP-kiszolgáló hibák*.</span><span class="sxs-lookup"><span data-stu-id="a2d44-157">**Metric:** What metric to monitor, in this case: *HTTP Server Errors*.</span></span>
- <span data-ttu-id="a2d44-158">**Feltétel:** mikor kell riasztást, ebben az esetben válassza a *nagyobb, mint* lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a2d44-158">**Condition:** When to alert, in this case select the *greater than* option.</span></span>
- <span data-ttu-id="a2d44-159">**Küszöbérték:** Mi az érték, amelyet meg kíván keresni, ebben az esetben: *400*.</span><span class="sxs-lookup"><span data-stu-id="a2d44-159">**Threshold:** What is value to look for, in this case: *400*.</span></span>
- <span data-ttu-id="a2d44-160">**Időszak:** riasztások hatnak a metrika az átlagos értéke.</span><span class="sxs-lookup"><span data-stu-id="a2d44-160">**Period:** Alerts operate over the average value of a metric.</span></span> <span data-ttu-id="a2d44-161">Kisebb időszakokra, yield érzékenyebb riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-161">Smaller periods of time yield more sensitive alerts.</span></span> <span data-ttu-id="a2d44-162">Ebben az esetben azt nézi *5 perc*.</span><span class="sxs-lookup"><span data-stu-id="a2d44-162">in this case we are looking at *5 Minutes*.</span></span>
- <span data-ttu-id="a2d44-163">**Tulajdonos és közreműködő szerepkörrel rendelkező személyek e-mail:** ebben az esetben: *engedélyezve*.</span><span class="sxs-lookup"><span data-stu-id="a2d44-163">**Email Owners and contributors:** in this case: *Enabled*.</span></span>

<span data-ttu-id="a2d44-164">Most, hogy a riasztást hoz létre az e-mail elküldésekor történik, minden alkalommal, amikor az alkalmazás halad keresztül a beállított küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="a2d44-164">Now that the alert is created an email is sent every time the app goes over the configured threshold.</span></span> <span data-ttu-id="a2d44-165">Aktív riasztások is áttekintheti az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="a2d44-165">Active alerts can also be reviewed in the Azure portal.</span></span>

![A kiváltott riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> <span data-ttu-id="a2d44-167">További tudnivalók az Azure riasztások a következő hivatkozásokkal:</span><span class="sxs-lookup"><span data-stu-id="a2d44-167">Learn more about Azure Alerts with the following links:</span></span>
> - [<span data-ttu-id="a2d44-168">Mik azok a riasztások a Microsoft Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a2d44-168">What are alerts in Microsoft Azure</span></span>](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [<span data-ttu-id="a2d44-169">Művelet végrehajtása a metrikák</span><span class="sxs-lookup"><span data-stu-id="a2d44-169">Take Action On Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="a2d44-170">Riasztások metrika létrehozása</span><span class="sxs-lookup"><span data-stu-id="a2d44-170">Create metric alerts</span></span>](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <span data-ttu-id="a2d44-171"><a name="companion"></a>3. lépés – az App Service kiegészítő</span><span class="sxs-lookup"><span data-stu-id="a2d44-171"><a name="companion"></a> Step 3 - App Service Companion</span></span>
<span data-ttu-id="a2d44-172">**App Service kiegészítő** kényelmesen a figyelheti az alkalmazást egy natív tapasztalattal a mobil eszköz (iOS vagy Android).</span><span class="sxs-lookup"><span data-stu-id="a2d44-172">**App Service companion** offers a convenient way to monitor your app with a native experience in your mobile device (iOS or Android).</span></span>

<span data-ttu-id="a2d44-173">Az App Service kiegészítő használja:</span><span class="sxs-lookup"><span data-stu-id="a2d44-173">Use App Service Companion to:</span></span>
- <span data-ttu-id="a2d44-174">Tekintse át az alkalmazás metrikák</span><span class="sxs-lookup"><span data-stu-id="a2d44-174">Review application metrics</span></span>
- <span data-ttu-id="a2d44-175">Tekintse át és rájuk alkalmazás riasztások és javaslatok</span><span class="sxs-lookup"><span data-stu-id="a2d44-175">Review and act on application alerts and recommendations</span></span>
- <span data-ttu-id="a2d44-176">Alapszintű hibaelhárítás (Tallózás, elindíthatja, leállíthatja, indítsa újra az alkalmazást)</span><span class="sxs-lookup"><span data-stu-id="a2d44-176">Perform basic troubleshooting (browse, start, stop, restart the app)</span></span>
- <span data-ttu-id="a2d44-177">Leküldéses értesítések kritikus események lekérése.</span><span class="sxs-lookup"><span data-stu-id="a2d44-177">Get push notifications for critical events.</span></span>

![Az App Service kiegészítő](media/app-service-web-tutorial-monitoring/app-service-companion.png)

<span data-ttu-id="a2d44-179">[![App Service kiegészítő App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![alkalmazásszolgáltatási kiegészítő Google Play áruházból](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="a2d44-179">[![App Service Companion App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![App Service Companion Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

<span data-ttu-id="a2d44-180">Az App Service kiegészítő telepítése a [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) vagy [Google Play áruházból](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="a2d44-180">You can install App Service companion from the [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) or [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

## <span data-ttu-id="a2d44-181"><a name="diagnose"></a>4. lépés: hibáinak diagnosztizálásához és elhárításához</span><span class="sxs-lookup"><span data-stu-id="a2d44-181"><a name="diagnose"></a> Step 4 - Diagnose and solve problems</span></span>
<span data-ttu-id="a2d44-182">**Hibáinak diagnosztizálásához és elhárításához** alkalmazással kapcsolatos problémák egymástól segít alkotnak szolgáltatásában.</span><span class="sxs-lookup"><span data-stu-id="a2d44-182">**Diagnose and solve problems** helps you separate application issues form platform issues.</span></span> <span data-ttu-id="a2d44-183">Azt is javasoljuk, hogy lehetséges megoldást lekérni a webalkalmazás ismét működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a2d44-183">It can also suggest possible mitigations to get your Web App back to healthy.</span></span>

![Hibáinak diagnosztizálásához és elhárításához](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

<span data-ttu-id="a2d44-185">A példa űrlap előző lépést folytatva, láthatja, hogy az alkalmazás rendelkezik lett elérhetőségéről problémák történtek.</span><span class="sxs-lookup"><span data-stu-id="a2d44-185">Continuing with the example form previous steps, we can see that the application has been having availably issues.</span></span> <span data-ttu-id="a2d44-186">A platform rendelkezésre állása ellentétben nem áthelyezte 100 %-ból.</span><span class="sxs-lookup"><span data-stu-id="a2d44-186">In contrast, the platform availability has not moved from 100%.</span></span>

<span data-ttu-id="a2d44-187">Az alkalmazás okoz problémát, és mentése a platform marad, esetén egy tiszta jelzi, hogy a Microsoft alkalmazásproblémák foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="a2d44-187">When the app is having issue and the platform stays up, it's a clear indication that we are dealing with an application issue.</span></span>

## <span data-ttu-id="a2d44-188"><a name="logging"></a>– 5. lépés-naplózás</span><span class="sxs-lookup"><span data-stu-id="a2d44-188"><a name="logging"></a> Step 5 - Logging</span></span>
<span data-ttu-id="a2d44-189">Most, hogy azt rendelkezik leszűkítheti a alkalmazásproblémák sikertelen, azt is meg az alkalmazás és a kiszolgáló talál további információkat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-189">Now that we have narrowed down the failures to an application issue, we can look at the application and server logs to get more information.</span></span>

<span data-ttu-id="a2d44-190">Naplózás lehetővé teszi, hogy mindkét gyűjthet **Application Diagnostics** és **Web Server diagnosztika** naplók a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a2d44-190">Logging allows you to collect both **Application Diagnostics** and **Web Server Diagnostics** logs for your Web App.</span></span>

### <a name="application-diagnostics"></a><span data-ttu-id="a2d44-191">Application Diagnostics</span><span class="sxs-lookup"><span data-stu-id="a2d44-191">Application Diagnostics</span></span>
<span data-ttu-id="a2d44-192">Application diagnostics lehetővé teszi az alkalmazás futásidőben által előállított nyomok rögzítését.</span><span class="sxs-lookup"><span data-stu-id="a2d44-192">Application diagnostics allows you to capture traces produced by the application at runtime.</span></span>

<span data-ttu-id="a2d44-193">Nyomkövetés nagy mértékben az alkalmazás hozzáadása növeli a hibakeresési és a PIN-kód-pont problémákat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-193">Adding tracing to your application greatly improves your ability to debug and pin-point issues.</span></span>

<span data-ttu-id="a2d44-194">Az ASP.NET, az alkalmazások nyomkövetéseit használatával jelentkezhetnek [System.Diagnostics.Trace osztály](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) a napló infrastruktúra által rögzített események generálásához.</span><span class="sxs-lookup"><span data-stu-id="a2d44-194">In ASP.NET, you can log application traces using [System.Diagnostics.Trace class](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) to generate events that are captured by the log infrastructure.</span></span> <span data-ttu-id="a2d44-195">A nyomkövetés megkönnyíti szűréshez súlyossága is megadható.</span><span class="sxs-lookup"><span data-stu-id="a2d44-195">You can also specify the severity of the trace for easier filtering.</span></span>

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
<span data-ttu-id="a2d44-196">Ahhoz, hogy alkalmazásnaplózás Ugrás **figyelés** > **diagnosztikai naplók** és alkalmazás naplózás engedélyezése váltógombok segítségével.</span><span class="sxs-lookup"><span data-stu-id="a2d44-196">To enable Application logging Go to **Monitoring** > **Diagnostic Logs** and Enable Application Logging using the toggles.</span></span>

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

<span data-ttu-id="a2d44-198">Alkalmazásnaplók tárolja a webalkalmazás fájlrendszerre, vagy leküldött blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="a2d44-198">Application logs can be stored to your Web App's file system or pushed out to blob storage.</span></span> <span data-ttu-id="a2d44-199">Éles környezetben javasoljuk a blob storage használata.</span><span class="sxs-lookup"><span data-stu-id="a2d44-199">For production scenarios, it's recommended to use blob storage.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2d44-200">Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-200">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="a2d44-201">Éles környezetben a hibanaplókat használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a2d44-201">For production scenarios, error logs are recommended.</span></span> <span data-ttu-id="a2d44-202">Csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.</span><span class="sxs-lookup"><span data-stu-id="a2d44-202">Only enable more verbose logging when investigating issues.</span></span>

 ### <a name="web-server-diagnostics"></a><span data-ttu-id="a2d44-203">Web Server diagnosztika</span><span class="sxs-lookup"><span data-stu-id="a2d44-203">Web Server Diagnostics</span></span>
<span data-ttu-id="a2d44-204">Webkiszolgáló naplóinak akkor jönnek létre, akkor is, ha az alkalmazás nem tagolva..</span><span class="sxs-lookup"><span data-stu-id="a2d44-204">Web Server logs are generated even if your app is not instrumented.</span></span> <span data-ttu-id="a2d44-205">App Service össze tudják gyűjteni a kiszolgáló naplóiban három különböző típusú:</span><span class="sxs-lookup"><span data-stu-id="a2d44-205">App Service can collect three different types of server logs:</span></span>

- <span data-ttu-id="a2d44-206">**Web Server naplózása**</span><span class="sxs-lookup"><span data-stu-id="a2d44-206">**Web Server Logging**</span></span>
    - <span data-ttu-id="a2d44-207">HTTP-tranzakciók használatával kapcsolatos információkat a [W3C bővített naplófájlformátum](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span><span class="sxs-lookup"><span data-stu-id="a2d44-207">Information about HTTP transactions using the [W3C extended log file format](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span></span>
    - <span data-ttu-id="a2d44-208">Akkor hasznos, például a kezelt kéréseket, és hogy hány kérésnek egy adott IP-címről van a teljes webhelymetrikák meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="a2d44-208">Useful when determining overall site metrics such as the number of requests handled or how many requests are from a specific IP address.</span></span>
- <span data-ttu-id="a2d44-209">**Hibával kapcsolatos részletes naplózás**</span><span class="sxs-lookup"><span data-stu-id="a2d44-209">**Detailed Error Logging**</span></span>
    - <span data-ttu-id="a2d44-210">(Állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hibával kapcsolatos részletes információk.</span><span class="sxs-lookup"><span data-stu-id="a2d44-210">Detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).</span></span>
    - [<span data-ttu-id="a2d44-211">További információ a hibával kapcsolatos részletes naplózás</span><span class="sxs-lookup"><span data-stu-id="a2d44-211">Learn more about detailed error logging</span></span>](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- <span data-ttu-id="a2d44-212">**Sikertelen kérelmek nyomkövetése**</span><span class="sxs-lookup"><span data-stu-id="a2d44-212">**Failed Request Tracing**</span></span>
    - <span data-ttu-id="a2d44-213">Sikertelen kérelmek nyomkövetési segítségével dolgozza fel a kérelmet, és minden egyes összetevő szükséges idő az IIS-összetevők többek között részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="a2d44-213">Detailed information on failed requests, including a trace of the IIS components used to process the request and the time taken in each component.</span></span>
    - <span data-ttu-id="a2d44-214">Nem sikerült a kérelem naplók hasznosak, ha egy adott HTTP hiba próbál különítse el, mi okozza.</span><span class="sxs-lookup"><span data-stu-id="a2d44-214">Failed request logs are useful when trying to isolate what is causing a specific HTTP error.</span></span>
    - [<span data-ttu-id="a2d44-215">További tudnivalók a sikertelen kérelmek nyomkövetése</span><span class="sxs-lookup"><span data-stu-id="a2d44-215">Learn more about failed request tracing</span></span>](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

<span data-ttu-id="a2d44-216">Kiszolgáló naplózásának engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="a2d44-216">To enable Server logging:</span></span>
- <span data-ttu-id="a2d44-217">Ugrás a **figyelés** > **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-217">go to **Monitoring** > **Diagnostic Logs**.</span></span>
- <span data-ttu-id="a2d44-218">A Web Server diagnosztika váltógombok segítségével különböző típusú engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a2d44-218">Enable the different types of Web Server Diagnostics using the toggles.</span></span>

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> <span data-ttu-id="a2d44-220">Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-220">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="a2d44-221">Éles környezetben hibanaplókat ajánlott, csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.</span><span class="sxs-lookup"><span data-stu-id="a2d44-221">For Production Scenarios, Error logs are recommended, Only Enable more verbose logging when investigating issues.</span></span>

### <a name="accessing-logs"></a><span data-ttu-id="a2d44-222">Naplók elérése</span><span class="sxs-lookup"><span data-stu-id="a2d44-222">Accessing Logs</span></span>
<span data-ttu-id="a2d44-223">A blob Storage tárolóban tárolt naplók Azure Tártallózó hozzáférnek.</span><span class="sxs-lookup"><span data-stu-id="a2d44-223">Logs stored in blob storage are accessed using Azure Storage Explorer.</span></span> <span data-ttu-id="a2d44-224">A webalkalmazás filesystem tárolt naplók az a következő elérési útján FTP keresztül érhetők el:</span><span class="sxs-lookup"><span data-stu-id="a2d44-224">Logs stored in the Web App's filesystem are accessed through FTP under the following paths:</span></span>

- <span data-ttu-id="a2d44-225">**Alkalmazásnaplók** - `%HOME%/LogFiles/Application/`.</span><span class="sxs-lookup"><span data-stu-id="a2d44-225">**Application logs** - `%HOME%/LogFiles/Application/`.</span></span>
    - <span data-ttu-id="a2d44-226">Ez a mappa tartalmaz egy vagy több alkalmazásnaplózás által előállított adatokat tartalmazó szöveges fájlt.</span><span class="sxs-lookup"><span data-stu-id="a2d44-226">This folder contains one or more text files containing information produced by application logging.</span></span>
- <span data-ttu-id="a2d44-227">**Sikertelen kérelmek nyomkövetési** - `%HOME%/LogFiles/W3SVC#########/`.</span><span class="sxs-lookup"><span data-stu-id="a2d44-227">**Failed Request Traces** - `%HOME%/LogFiles/W3SVC#########/`.</span></span>
    - <span data-ttu-id="a2d44-228">Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-228">This folder contains an XSL file and one or more XML files.</span></span>
- <span data-ttu-id="a2d44-229">**Részletes hibanaplókat** - `%HOME%/LogFiles/DetailedErrors/`.</span><span class="sxs-lookup"><span data-stu-id="a2d44-229">**Detailed Error Logs** - `%HOME%/LogFiles/DetailedErrors/`.</span></span>
    - <span data-ttu-id="a2d44-230">Ez a mappa tartalmaz egy vagy több, a HTTP-hibák az alkalmazás által létrehozott nagy mennyiségű vonatkozó .htm fájlt.</span><span class="sxs-lookup"><span data-stu-id="a2d44-230">This folder contains one or more .htm files with extensive information on HTTP errors generated by your app.</span></span>
- <span data-ttu-id="a2d44-231">**Webalkalmazás-naplói** - `%HOME%/LogFiles/http/RawLogs`.</span><span class="sxs-lookup"><span data-stu-id="a2d44-231">**Web Server Logs** - `%HOME%/LogFiles/http/RawLogs`.</span></span>
    - <span data-ttu-id="a2d44-232">Ez a mappa tartalmaz egy vagy több szövegfájlok formátuma a W3C bővített naplófájlformátum használata.</span><span class="sxs-lookup"><span data-stu-id="a2d44-232">This folder contains one or more text files formatted using the W3C extended log file format.</span></span>

## <span data-ttu-id="a2d44-233"><a name="streaming"></a>Lépés 6 - adatfolyam-napló</span><span class="sxs-lookup"><span data-stu-id="a2d44-233"><a name="streaming"></a> Step 6 - Log Streaming</span></span>
<span data-ttu-id="a2d44-234">Folyamatos átviteli naplók esetén kényelmesen használható alkalmazás hibakeresés, mivel képest időt takaríthat [a naplók elérése](#Accessing-Logs) FTP keresztül.</span><span class="sxs-lookup"><span data-stu-id="a2d44-234">Streaming logs are convenient when debugging an application since it saves time compared to [accessing the logs](#Accessing-Logs) through FTP.</span></span>

<span data-ttu-id="a2d44-235">App Service képes adatfolyam **alkalmazásnaplók** és **webkiszolgáló naplóinak** , akkor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="a2d44-235">App Service can stream **Application Logs** and **Web Server Logs** as they are generated.</span></span>

> [!TIP]
> <span data-ttu-id="a2d44-236">Mielőtt az adatfolyam-naplókat, győződjön meg arról, hogy engedélyezte a gyűjtését naplók leírtak szerint a [naplózás](#logging) szakasz.</span><span class="sxs-lookup"><span data-stu-id="a2d44-236">Before trying to stream logs, make sure you have enabled collecting logs as described in the [Logging](#logging) section.</span></span>

<span data-ttu-id="a2d44-237">Az adatfolyam-naplók lépjen **figyelés**> **naplófolyamot**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-237">To stream logs, go to **Monitoring**> **Log Stream**.</span></span> <span data-ttu-id="a2d44-238">Válassza ki **alkalmazásnaplók** vagy **webalkalmazás-naplói** attól függően, hogy milyen információkat keres.</span><span class="sxs-lookup"><span data-stu-id="a2d44-238">Select **Application Logs** or **Web server logs** depending what information you are looking for.</span></span> <span data-ttu-id="a2d44-239">Itt meg is szüneteltetése, indítsa újra, és törölje a puffer.</span><span class="sxs-lookup"><span data-stu-id="a2d44-239">From here, you can also pause, restart, and clear the buffer.</span></span>

![Folyamatos átviteli naplók](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> <span data-ttu-id="a2d44-241">Naplók csak akkor jön létre, amikor az alkalmazás forgalom van, megnövelheti a naplók további események és információk részletességi is.</span><span class="sxs-lookup"><span data-stu-id="a2d44-241">Logs are only generated when there is traffic on the app, you can also increase the verbosity of logs to get more events or information.</span></span>

## <span data-ttu-id="a2d44-242"><a name="remote"></a>7. lépés - távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="a2d44-242"><a name="remote"></a> Step 7 - Remote Debugging</span></span>
<span data-ttu-id="a2d44-243">Ha Ön rendelkezik PIN-kód-jelölt alkalmazások problémák forrását, a **távoli hibakeresés** bízná a kódot.</span><span class="sxs-lookup"><span data-stu-id="a2d44-243">Once you have pin-pointed the source of the applications problems, use **Remote Debugging** to walk through the code.</span></span>

<span data-ttu-id="a2d44-244">Távoli hibakeresés lehetővé csatol hibakereső a webalkalmazás fut a felhőben.</span><span class="sxs-lookup"><span data-stu-id="a2d44-244">Remote debugging lets you attach a debugger to your Web App running in the cloud.</span></span> <span data-ttu-id="a2d44-245">Állítson be töréspontokat, kezelheti közvetlenül a memória, kód teljesítéséhez és még a kód elérési utat módosítsa, ugyanúgy, mint a helyileg futó alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a2d44-245">You can set breakpoints, manipulate memory directly, step through code, and even change the code path just like you do for an app running locally.</span></span>

<span data-ttu-id="a2d44-246">A hibakereső csatolása a felhőben futó alkalmazásnak:</span><span class="sxs-lookup"><span data-stu-id="a2d44-246">To attach the debugger to your app running in the cloud:</span></span>

- <span data-ttu-id="a2d44-247">A Visual Studio 2017 nyissa meg a megoldást a hibakeresési kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="a2d44-247">Using Visual Studio 2017, open the solution for the app you want to debug</span></span>
- <span data-ttu-id="a2d44-248">Néhány berendezés pont ugyanúgy, mint a helyi fejlesztési beállítása.</span><span class="sxs-lookup"><span data-stu-id="a2d44-248">Set some brake points just like you would for local development.</span></span>
- <span data-ttu-id="a2d44-249">Nyissa meg **cloud explorer** (parancsra + /, ctrl + x).</span><span class="sxs-lookup"><span data-stu-id="a2d44-249">Open **cloud explorer** (ctr + /, ctrl + x).</span></span>
- <span data-ttu-id="a2d44-250">Jelentkezzen be azure hitelesítő adataival, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="a2d44-250">Log in with your azure credentials as needed.</span></span>
- <span data-ttu-id="a2d44-251">Megkeresheti az alkalmazást hibakeresési kívánt</span><span class="sxs-lookup"><span data-stu-id="a2d44-251">Find the app you want to debug</span></span>
- <span data-ttu-id="a2d44-252">Válassza ki **csatolása hibakereső** űrlap a **műveletek** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="a2d44-252">Select **Attach Debugger** form the **Actions** pane.</span></span>

![Távoli hibakeresés](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

<span data-ttu-id="a2d44-254">A Visual Studio konfigurálja a távoli hibakereséshez alkalmazást, és elindítja egy böngészőablakban, amely az alkalmazás navigál.</span><span class="sxs-lookup"><span data-stu-id="a2d44-254">Visual Studio configures your application for remote debugging and launches a browser window that navigates to your app.</span></span> <span data-ttu-id="a2d44-255">Tallózzon a break pontok és a kód lépésenkénti elindítani az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a2d44-255">Browse through your app to trigger break points and step through the code.</span></span>

> [!WARNING]
> <span data-ttu-id="a2d44-256">Éles hibakeresési módban nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a2d44-256">Running in debug mode in production is not recommended.</span></span> <span data-ttu-id="a2d44-257">Ha az éles alkalmazásban nem horizontálisan több kiszolgálópéldány, hibakeresés megakadályozni a webkiszolgáló egyéb kérésekre válaszol.</span><span class="sxs-lookup"><span data-stu-id="a2d44-257">If your production app is not scaled out to multiple server instances, debugging prevent the web server from responding to other requests.</span></span> <span data-ttu-id="a2d44-258">A termelési problémák elhárításához, a legjobb erőforrás-hoz [naplózási](#logging) és [Application Insights](#insights).</span><span class="sxs-lookup"><span data-stu-id="a2d44-258">For troubleshooting production problems, your best resource is to [configure logging](#logging) and [Application Insights](#insights).</span></span>



## <span data-ttu-id="a2d44-259"><a name="explorer"></a>8 - Process Explorert. lépés</span><span class="sxs-lookup"><span data-stu-id="a2d44-259"><a name="explorer"></a> Step 8 - Process Explorer</span></span>
<span data-ttu-id="a2d44-260">Ha az alkalmazás csak egy példányhoz, horizontálisan **process Explorert** példány konkrét problémák azonosításához nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="a2d44-260">When your application is scaled out to more than one instance, **process explorer** can help you identify instance specific problems.</span></span>

<span data-ttu-id="a2d44-261">Használjon **Explorer feldolgozni** számára:</span><span class="sxs-lookup"><span data-stu-id="a2d44-261">Use **Process Explorer** to:</span></span>

- <span data-ttu-id="a2d44-262">A folyamatok felsorolni az App Service-csomag különböző példányai között.</span><span class="sxs-lookup"><span data-stu-id="a2d44-262">Enumerate all the processes across different instances of your App Service plan.</span></span>
- <span data-ttu-id="a2d44-263">Részletekbe menően tárhatják, és tekintse meg a leírók és minden folyamathoz tartozó modulok.</span><span class="sxs-lookup"><span data-stu-id="a2d44-263">Drill down and view the handles and modules associated with each process.</span></span>
- <span data-ttu-id="a2d44-264">Megtekintheti a CPU, működik-e beállítva, és szálak száma a folyamat szintjén sorozatos folyamatok azonosítása</span><span class="sxs-lookup"><span data-stu-id="a2d44-264">View CPU, Working Set, and Thread count at the process level to help you identify runaway processes</span></span>
- <span data-ttu-id="a2d44-265">A megnyitott fájlleíró keresse meg és még kill egy adott példányt.</span><span class="sxs-lookup"><span data-stu-id="a2d44-265">Find open file handles, and even kill a specific process instance.</span></span>

<span data-ttu-id="a2d44-266">Folyamat Explorer alatt található **figyelés** > **Process Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a2d44-266">Process Explorer can be found under **Monitoring** > **Process Explorer**.</span></span>

![Process Explorer](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <span data-ttu-id="a2d44-268"><a name="insights"></a>9 - Application insights szolgáltatással. lépés</span><span class="sxs-lookup"><span data-stu-id="a2d44-268"><a name="insights"></a> Step 9 - Application Insights</span></span>
<span data-ttu-id="a2d44-269">**Az Application Insights** alkalmazásprofil-készítési és speciális megfigyelési képességeket biztosít az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a2d44-269">**Application Insights** provides application profiling and advanced monitoring capabilities for your app.</span></span>

<span data-ttu-id="a2d44-270">Az Application Insights segítségével észlelheti és diagnosztizálhatja a kivételeket és a webes alkalmazás teljesítményével kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="a2d44-270">Use Application Insights to detect and diagnose exceptions and performance issues in your Web App.</span></span>

<span data-ttu-id="a2d44-271">A webalkalmazás alatt engedélyezheti az Application Insights **figyelés** > **Application insights szolgáltatással**</span><span class="sxs-lookup"><span data-stu-id="a2d44-271">You can enable Application Insights for your Web App under **Monitoring** > **Application Insights**</span></span>

> [!NOTE]
> <span data-ttu-id="a2d44-272">Az Application Insights kérheti, hogy telepítse az Application Insights hely bővítmény adatgyűjtés elindításához.</span><span class="sxs-lookup"><span data-stu-id="a2d44-272">Application Insights might prompt you to install the Application Insights site extension to start collecting data.</span></span> <span data-ttu-id="a2d44-273">A webhely-bővítmény telepítése azt eredményezi, újra kell indítani az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a2d44-273">Installing the site extension causes an application restart.</span></span>

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

<span data-ttu-id="a2d44-275">Az Application Insights beállítása, a további szerepel a hivatkozások követése gazdag lehetőség van a [lépések](#next) szakasz.</span><span class="sxs-lookup"><span data-stu-id="a2d44-275">Application Insights has a rich feature set, to learn more, follow the links included in the [Next Steps](#next) section.</span></span>

## <span data-ttu-id="a2d44-276"><a name="next"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a2d44-276"><a name="next"></a> Next steps</span></span>

 - [<span data-ttu-id="a2d44-277">Mi az Application Insights</span><span class="sxs-lookup"><span data-stu-id="a2d44-277">What is Application Insights</span></span>](..\application-insights\app-insights-overview.md)
 - [<span data-ttu-id="a2d44-278">Az Application insights szolgáltatással Azure webes alkalmazás teljesítményének figyelése</span><span class="sxs-lookup"><span data-stu-id="a2d44-278">Monitor Azure web app performance with Application Insights</span></span>](..\application-insights\app-insights-azure-web-apps.md)
 - [<span data-ttu-id="a2d44-279">Rendelkezésre állás figyelése és reakcióidőt, a webhely, az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="a2d44-279">Monitor availability and responsiveness of any web site with Application Insights</span></span>](..\application-insights\app-insights-monitor-web-app-availability.md)
