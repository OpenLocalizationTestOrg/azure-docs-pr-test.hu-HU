---
title: "a webes alkalmazás aaaMonitor |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset fel a figyelés a webalkalmazásban"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: c2f5e9842c732a804f1caee5d67e53dad24e190a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-app-service"></a><span data-ttu-id="608e3-103">Az App Service figyelése</span><span class="sxs-lookup"><span data-stu-id="608e3-103">Monitor App Service</span></span>
<span data-ttu-id="608e3-104">Ez az oktatóanyag végigvezeti az alkalmazás figyelése és hello beépített platform eszközök toosolve problémák használata, azok bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="608e3-104">This tutorial walks you through monitoring your app and using hello built-in platform tools toosolve problems when they occur.</span></span>

<span data-ttu-id="608e3-105">Ez a dokumentum egyes részeinek kerül egy adott funkcióhoz keresztül.</span><span class="sxs-lookup"><span data-stu-id="608e3-105">Each section of this document goes over a specific feature.</span></span> <span data-ttu-id="608e3-106">Hello szolgáltatások együttes használata lehetővé teszik, hogy:</span><span class="sxs-lookup"><span data-stu-id="608e3-106">Using hello features together let you:</span></span>
- <span data-ttu-id="608e3-107">Az alkalmazás problémát azonosítása.</span><span class="sxs-lookup"><span data-stu-id="608e3-107">Identifying an issue in your app.</span></span>
- <span data-ttu-id="608e3-108">Annak meghatározása, amikor hello a problémát a kódban, illetve hello platform okozta.</span><span class="sxs-lookup"><span data-stu-id="608e3-108">Determining when hello issue is caused by your code or hello platform.</span></span>
- <span data-ttu-id="608e3-109">Leszűkíthet hello probléma merült fel a kód a hello forrását.</span><span class="sxs-lookup"><span data-stu-id="608e3-109">Narrow down hello source of hello problem in your code.</span></span>
- <span data-ttu-id="608e3-110">Hibakeresés és hello a probléma kijavítása.</span><span class="sxs-lookup"><span data-stu-id="608e3-110">Debugging and fixing hello issue.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="608e3-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="608e3-111">Before you begin</span></span>
- <span data-ttu-id="608e3-112">A webalkalmazás toomonitor kell, és hajtsa végre a hello leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="608e3-112">You need a Web App toomonitor and follow hello outlined steps.</span></span>
    - <span data-ttu-id="608e3-113">Egy alkalmazás hello a hello ismertetett lépéseket követve hozhat létre [ASP.NET-alkalmazás létrehozása az Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="608e3-113">You can create an application following hello steps described in hello [Create an ASP.NET app in Azure with SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) tutorial.</span></span>

- <span data-ttu-id="608e3-114">Ha szeretné tootry **távoli hibakeresés** az alkalmazás, a Visual Studio szüksége.</span><span class="sxs-lookup"><span data-stu-id="608e3-114">If you want tootry out **Remote Debugging** of your application, you need Visual Studio.</span></span>
    - <span data-ttu-id="608e3-115">Ha még nincs telepítve a Visual Studio 2017, letöltheti és használható szabad hello [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="608e3-115">If you don’t already have Visual Studio 2017 installed, you can download and use hello free [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span>
    - <span data-ttu-id="608e3-116">Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="608e3-116">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

## <span data-ttu-id="608e3-117"><a name="metrics"></a>1. lépés – nézet metrikák</span><span class="sxs-lookup"><span data-stu-id="608e3-117"><a name="metrics"></a> Step 1 - View metrics</span></span>
<span data-ttu-id="608e3-118">**Metrikák** hasznos toounderstand vannak:</span><span class="sxs-lookup"><span data-stu-id="608e3-118">**Metrics** are useful toounderstand:</span></span>
- <span data-ttu-id="608e3-119">Alkalmazás állapotának</span><span class="sxs-lookup"><span data-stu-id="608e3-119">Application health</span></span>
- <span data-ttu-id="608e3-120">Alkalmazás teljesítménye</span><span class="sxs-lookup"><span data-stu-id="608e3-120">App performance</span></span>
- <span data-ttu-id="608e3-121">Erőforrás-használat</span><span class="sxs-lookup"><span data-stu-id="608e3-121">Resource consumption</span></span>

<span data-ttu-id="608e3-122">Az alkalmazás problémát vizsgál, tekintse át a metrikákat esetén egy remek toostart.</span><span class="sxs-lookup"><span data-stu-id="608e3-122">When investigating an application issue, reviewing metrics is a good place toostart.</span></span> <span data-ttu-id="608e3-123">Azure portálon találhat egy vizsgálja meg az alkalmazás használatának hello metrikák gyorsan toovisually **Azure figyelő**.</span><span class="sxs-lookup"><span data-stu-id="608e3-123">Azure portal has a quick way toovisually inspect hello metrics of your app using **Azure Monitor**.</span></span>

<span data-ttu-id="608e3-124">Metrikákat biztosít az alkalmazás több kulcs összesítésben állapotelőzményeinek a nézetét.</span><span class="sxs-lookup"><span data-stu-id="608e3-124">Metrics provide a historical view across several key aggregations for your app.</span></span> <span data-ttu-id="608e3-125">Bármely alkalmazás az app service-ben üzemeltetett, célszerű figyelemmel kísérni hello Web App és a hello App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="608e3-125">For any app hosted in app service, you should monitor both hello Web App and hello App Service plan.</span></span>

> [!NOTE]
> <span data-ttu-id="608e3-126">Az App Service-csomag a alkalmazásai használt fizikai erőforrások toohost hello gyűjteményét képviseli.</span><span class="sxs-lookup"><span data-stu-id="608e3-126">An App Service plan represents hello collection of physical resources used toohost your apps.</span></span> <span data-ttu-id="608e3-127">Minden alkalmazás hozzárendelt tooan az alkalmazásszolgáltatási csomag hello erőforrások megosztása határozzák meg, hogy lehetővé teszi a toosave költség esetén több alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="608e3-127">All applications assigned tooan App Service plan share hello resources defined by it allowing you toosave cost when hosting multiple apps.</span></span>
>
> <span data-ttu-id="608e3-128">Az App Service-csomagok a következőket határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="608e3-128">App Service plans define:</span></span>
> * <span data-ttu-id="608e3-129">Régió: Észak-Európa, USA keleti régiója, Délkelet-Ázsiában, stb.</span><span class="sxs-lookup"><span data-stu-id="608e3-129">Region: North Europe, East US, Southeast Asia, etc.</span></span>
> * <span data-ttu-id="608e3-130">Példány mérete: Kicsi, közepes, nagy, stb.</span><span class="sxs-lookup"><span data-stu-id="608e3-130">Instance Size: Small, Medium, Large, etc.</span></span>
> * <span data-ttu-id="608e3-131">Méretezési száma: egy, kettő vagy három példányt, stb.</span><span class="sxs-lookup"><span data-stu-id="608e3-131">Scale Count: one, two, or three instances, etc.</span></span>
> * <span data-ttu-id="608e3-132">Termékváltozat: Ingyenes, a közös, Basic, Standard, Premium, stb.</span><span class="sxs-lookup"><span data-stu-id="608e3-132">SKU: Free, Shared, Basic, Standard, Premium, etc.</span></span>

<span data-ttu-id="608e3-133">a webalkalmazás, nyissa meg toohello tooreview metrikák **áttekintése** panel hello alkalmazás toomonitor szeretné.</span><span class="sxs-lookup"><span data-stu-id="608e3-133">tooreview metrics for your Web App, go toohello **Overview** blade of hello app you want toomonitor.</span></span> <span data-ttu-id="608e3-134">Itt megtekintheti, az alkalmazás metrikák diagramot egy **figyelés csempe**.</span><span class="sxs-lookup"><span data-stu-id="608e3-134">From here, you can view a chart for your app's metrics as a **Monitoring tile**.</span></span> <span data-ttu-id="608e3-135">Hello csempe tooedit kattintson, és milyen metrikák tooview konfigurálása, valamint idő tartomány toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="608e3-135">Click hello tile tooedit and configure what metrics tooview and hello time range toodisplay.</span></span>

<span data-ttu-id="608e3-136">Alapértelmezett hello erőforráspanelen biztosít egy nézet hello Alkalmazáskérelmeinek és hello elmúlt egy óra által jelzett hibákat.</span><span class="sxs-lookup"><span data-stu-id="608e3-136">By default hello resource blade provides a view for hello Application Requests and errors for hello last hour.</span></span>
<span data-ttu-id="608e3-137">![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span><span class="sxs-lookup"><span data-stu-id="608e3-137">![Monitor App](media/app-service-web-tutorial-monitoring/app-service-monitor.png)</span></span>

<span data-ttu-id="608e3-138">Hello példában látható, mert van egy alkalmazás, hogy a különböző **HTTP-kiszolgáló hibák**.</span><span class="sxs-lookup"><span data-stu-id="608e3-138">As you can see in hello example, we have an application that is generating many **HTTP Server Errors**.</span></span> <span data-ttu-id="608e3-139">hibák nagy mennyiségű hello hello első arra utal, hogy az alkalmazás kell tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="608e3-139">hello high volume of errors is hello first indication we need tooinvestigate this application.</span></span>

> [!TIP]
> <span data-ttu-id="608e3-140">További tudnivalók az Azure figyelő a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="608e3-140">Learn more about Azure Monitor with hello following links:</span></span>
> - [<span data-ttu-id="608e3-141">Ismerkedés az Azure-figyelő</span><span class="sxs-lookup"><span data-stu-id="608e3-141">Get started with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="608e3-142">Az Azure metrikák</span><span class="sxs-lookup"><span data-stu-id="608e3-142">Azure Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [<span data-ttu-id="608e3-143">Azure-figyelő támogatott metrikák</span><span class="sxs-lookup"><span data-stu-id="608e3-143">Supported metrics with Azure Monitor</span></span>](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [<span data-ttu-id="608e3-144">Az Azure irányítópultok</span><span class="sxs-lookup"><span data-stu-id="608e3-144">Azure Dashboards</span></span>](..\azure-portal\azure-portal-dashboards.md)

## <span data-ttu-id="608e3-145"><a name="alerts"></a>2. lépés - riasztások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="608e3-145"><a name="alerts"></a> Step 2 - Configure Alerts</span></span>
<span data-ttu-id="608e3-146">**Riasztások** lehet konfigurált tootrigger az alkalmazás bizonyos feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="608e3-146">**Alerts** can be configured tootrigger on specific conditions for your app.</span></span>

<span data-ttu-id="608e3-147">A [1. lépés - nézet metrikák](#metrics), látott, hogy hello alkalmazás volt-e a hibák túl magas száma.</span><span class="sxs-lookup"><span data-stu-id="608e3-147">In [Step 1 - View metrics](#metrics), we saw that hello application had a high number of errors.</span></span>

<span data-ttu-id="608e3-148">Lehetővé teszi, hogy olyan riasztást konfigurálhat tooautomatically értesítést kaphat, ha hiba lép fel.</span><span class="sxs-lookup"><span data-stu-id="608e3-148">Lets configure an alert tooautomatically get notified when errors occur.</span></span> <span data-ttu-id="608e3-149">Ebben az esetben azt szeretné, hogy hello riasztási toosend és az e-mail minden alkalommal, amikor egy meghatározott küszöbérték feletti kerül hello HTTP 50 X hibák száma.</span><span class="sxs-lookup"><span data-stu-id="608e3-149">In this case, we want hello alert toosend and e-mail every time hello number of HTTP 50X errors goes over a certain threshold.</span></span>

<span data-ttu-id="608e3-150">túl keresse meg az értesítés, toocreate**figyelés** > **riasztások** kattintson **[+] riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="608e3-150">toocreate an alert, navigate too**Monitoring** > **Alerts** and click **[+] Add Alert**.</span></span>

![Riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

<span data-ttu-id="608e3-152">Adjon meg értékeket hello riasztás konfigurálásában:</span><span class="sxs-lookup"><span data-stu-id="608e3-152">Provide values for hello Alert configuration:</span></span>
- <span data-ttu-id="608e3-153">**Erőforrás:** hello hely toomonitor hello riasztáshoz.</span><span class="sxs-lookup"><span data-stu-id="608e3-153">**Resource:** hello site toomonitor with hello alert.</span></span>
- <span data-ttu-id="608e3-154">**Name:** egy nevet a riasztáshoz, ebben az esetben: *magas HTTP 50 X*.</span><span class="sxs-lookup"><span data-stu-id="608e3-154">**Name:** A name for your alert, in this case: *High HTTP 50X*.</span></span>
- <span data-ttu-id="608e3-155">**Leírás:** egyszerű szöveges magyarázatát, mi van megnézi a riasztás.</span><span class="sxs-lookup"><span data-stu-id="608e3-155">**Description:** Plain text explanation of what this alert is looking at.</span></span>
- <span data-ttu-id="608e3-156">**Riasztás:** riasztások metrikák vagy események is megtekinthetik, ehhez a példához azt nézi metrikákat.</span><span class="sxs-lookup"><span data-stu-id="608e3-156">**Alert on:** Alerts can look at Metrics or Events, for this example we are looking at metrics.</span></span>
- <span data-ttu-id="608e3-157">**A metrika:** milyen metrika toomonitor, ebben az esetben: *HTTP-kiszolgáló hibák*.</span><span class="sxs-lookup"><span data-stu-id="608e3-157">**Metric:** What metric toomonitor, in this case: *HTTP Server Errors*.</span></span>
- <span data-ttu-id="608e3-158">**Feltétel:** amikor tooalert, ebben az esetben válassza hello *nagyobb, mint* lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="608e3-158">**Condition:** When tooalert, in this case select hello *greater than* option.</span></span>
- <span data-ttu-id="608e3-159">**Küszöbérték:** érték toolook, újdonságok ebben az esetben: *400*.</span><span class="sxs-lookup"><span data-stu-id="608e3-159">**Threshold:** What is value toolook for, in this case: *400*.</span></span>
- <span data-ttu-id="608e3-160">**Időszak:** riasztások hatnak hello átlagos értéke a metrikát.</span><span class="sxs-lookup"><span data-stu-id="608e3-160">**Period:** Alerts operate over hello average value of a metric.</span></span> <span data-ttu-id="608e3-161">Kisebb időszakokra, yield érzékenyebb riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="608e3-161">Smaller periods of time yield more sensitive alerts.</span></span> <span data-ttu-id="608e3-162">Ebben az esetben azt nézi *5 perc*.</span><span class="sxs-lookup"><span data-stu-id="608e3-162">in this case we are looking at *5 Minutes*.</span></span>
- <span data-ttu-id="608e3-163">**Tulajdonos és közreműködő szerepkörrel rendelkező személyek e-mail:** ebben az esetben: *engedélyezve*.</span><span class="sxs-lookup"><span data-stu-id="608e3-163">**Email Owners and contributors:** in this case: *Enabled*.</span></span>

<span data-ttu-id="608e3-164">Most, hogy hello riasztást hoz létre egy e-mailt küld minden alkalommal, amikor hello app kerül konfigurált hello küszöbérték feletti.</span><span class="sxs-lookup"><span data-stu-id="608e3-164">Now that hello alert is created an email is sent every time hello app goes over hello configured threshold.</span></span> <span data-ttu-id="608e3-165">Aktív riasztások hello Azure-portálon is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="608e3-165">Active alerts can also be reviewed in hello Azure portal.</span></span>

![A kiváltott riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> <span data-ttu-id="608e3-167">További tudnivalók az Azure riasztások a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="608e3-167">Learn more about Azure Alerts with hello following links:</span></span>
> - [<span data-ttu-id="608e3-168">Mik azok a riasztások a Microsoft Azure-ban</span><span class="sxs-lookup"><span data-stu-id="608e3-168">What are alerts in Microsoft Azure</span></span>](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [<span data-ttu-id="608e3-169">Művelet végrehajtása a metrikák</span><span class="sxs-lookup"><span data-stu-id="608e3-169">Take Action On Metrics</span></span>](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [<span data-ttu-id="608e3-170">Riasztások metrika létrehozása</span><span class="sxs-lookup"><span data-stu-id="608e3-170">Create metric alerts</span></span>](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <span data-ttu-id="608e3-171"><a name="companion"></a>3. lépés – az App Service kiegészítő</span><span class="sxs-lookup"><span data-stu-id="608e3-171"><a name="companion"></a> Step 3 - App Service Companion</span></span>
<span data-ttu-id="608e3-172">**App Service kiegészítő** egy kényelmes módszert arra toomonitor kínál az alkalmazás (iOS vagy Android) egy mobileszközről natív nyújthassunk.</span><span class="sxs-lookup"><span data-stu-id="608e3-172">**App Service companion** offers a convenient way toomonitor your app with a native experience in your mobile device (iOS or Android).</span></span>

<span data-ttu-id="608e3-173">Az App Service kiegészítő használja:</span><span class="sxs-lookup"><span data-stu-id="608e3-173">Use App Service Companion to:</span></span>
- <span data-ttu-id="608e3-174">Tekintse át az alkalmazás metrikák</span><span class="sxs-lookup"><span data-stu-id="608e3-174">Review application metrics</span></span>
- <span data-ttu-id="608e3-175">Tekintse át és rájuk alkalmazás riasztások és javaslatok</span><span class="sxs-lookup"><span data-stu-id="608e3-175">Review and act on application alerts and recommendations</span></span>
- <span data-ttu-id="608e3-176">Alapszintű hibaelhárítás (Tallózás, start, stop, újraindítás hello alkalmazás)</span><span class="sxs-lookup"><span data-stu-id="608e3-176">Perform basic troubleshooting (browse, start, stop, restart hello app)</span></span>
- <span data-ttu-id="608e3-177">Leküldéses értesítések kritikus események lekérése.</span><span class="sxs-lookup"><span data-stu-id="608e3-177">Get push notifications for critical events.</span></span>

![Az App Service kiegészítő](media/app-service-web-tutorial-monitoring/app-service-companion.png)

<span data-ttu-id="608e3-179">[![App Service kiegészítő App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![alkalmazásszolgáltatási kiegészítő Google Play áruházból](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="608e3-179">[![App Service Companion App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![App Service Companion Google Play](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

<span data-ttu-id="608e3-180">Telepítheti az App Service kiegészítő hello [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) vagy [Google Play áruházból](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span><span class="sxs-lookup"><span data-stu-id="608e3-180">You can install App Service companion from hello [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) or [Google Play](https://play.google.com/store/apps/details?id=azureApps.AzureApps)</span></span>

## <span data-ttu-id="608e3-181"><a name="diagnose"></a>4. lépés: hibáinak diagnosztizálásához és elhárításához</span><span class="sxs-lookup"><span data-stu-id="608e3-181"><a name="diagnose"></a> Step 4 - Diagnose and solve problems</span></span>
<span data-ttu-id="608e3-182">**Hibáinak diagnosztizálásához és elhárításához** alkalmazással kapcsolatos problémák egymástól segít alkotnak szolgáltatásában.</span><span class="sxs-lookup"><span data-stu-id="608e3-182">**Diagnose and solve problems** helps you separate application issues form platform issues.</span></span> <span data-ttu-id="608e3-183">Azt is is javasolhatnak lehetséges megoldást tooget a webes alkalmazás hátsó toohealthy.</span><span class="sxs-lookup"><span data-stu-id="608e3-183">It can also suggest possible mitigations tooget your Web App back toohealthy.</span></span>

![Hibáinak diagnosztizálásához és elhárításához](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

<span data-ttu-id="608e3-185">Folytatása hello példa űrlap előző lépéseket, láthatja, hogy hello alkalmazás rendelkezik lett elérhetőségéről problémák történtek.</span><span class="sxs-lookup"><span data-stu-id="608e3-185">Continuing with hello example form previous steps, we can see that hello application has been having availably issues.</span></span> <span data-ttu-id="608e3-186">Hello platform rendelkezésre állása ellentétben nem helyezte 100 %-ból.</span><span class="sxs-lookup"><span data-stu-id="608e3-186">In contrast, hello platform availability has not moved from 100%.</span></span>

<span data-ttu-id="608e3-187">Ha hello alkalmazás hibát okoz, és hello platform marad be, egy tiszta jelzi, hogy a Microsoft alkalmazásproblémák foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="608e3-187">When hello app is having issue and hello platform stays up, it's a clear indication that we are dealing with an application issue.</span></span>

## <span data-ttu-id="608e3-188"><a name="logging"></a>– 5. lépés-naplózás</span><span class="sxs-lookup"><span data-stu-id="608e3-188"><a name="logging"></a> Step 5 - Logging</span></span>
<span data-ttu-id="608e3-189">Most, hogy rendelkezik a Microsoft hello hibák tooan alkalmazásproblémák leszűkítheti, azt is megtekinthetik hello alkalmazás és a kiszolgálói naplók tooget további információt.</span><span class="sxs-lookup"><span data-stu-id="608e3-189">Now that we have narrowed down hello failures tooan application issue, we can look at hello application and server logs tooget more information.</span></span>

<span data-ttu-id="608e3-190">Naplózás lehetővé teszi mindkét toocollect **Application Diagnostics** és **Web Server diagnosztika** naplók a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="608e3-190">Logging allows you toocollect both **Application Diagnostics** and **Web Server Diagnostics** logs for your Web App.</span></span>

### <a name="application-diagnostics"></a><span data-ttu-id="608e3-191">Application Diagnostics</span><span class="sxs-lookup"><span data-stu-id="608e3-191">Application Diagnostics</span></span>
<span data-ttu-id="608e3-192">Application diagnostics lehetővé teszi, hogy Ön toocapture nyomkövetések futásidőben hello alkalmazás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="608e3-192">Application diagnostics allows you toocapture traces produced by hello application at runtime.</span></span>

<span data-ttu-id="608e3-193">Nyomkövetés tooyour hozzáadása alkalmazás nagy mértékben csökkenti a képes toodebug és a PIN-kód-pont problémákat.</span><span class="sxs-lookup"><span data-stu-id="608e3-193">Adding tracing tooyour application greatly improves your ability toodebug and pin-point issues.</span></span>

<span data-ttu-id="608e3-194">Az ASP.NET, az alkalmazások nyomkövetéseit használatával jelentkezhetnek [System.Diagnostics.Trace osztály](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) hello napló infrastruktúra által rögzített események toogenerate.</span><span class="sxs-lookup"><span data-stu-id="608e3-194">In ASP.NET, you can log application traces using [System.Diagnostics.Trace class](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) toogenerate events that are captured by hello log infrastructure.</span></span> <span data-ttu-id="608e3-195">Egyszerűbb szűréshez hello nyomkövetési hello súlyossága is megadható.</span><span class="sxs-lookup"><span data-stu-id="608e3-195">You can also specify hello severity of hello trace for easier filtering.</span></span>

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
<span data-ttu-id="608e3-196">tooenable alkalmazásnaplózás lépjen túl**figyelés** > **diagnosztikai naplók** és alkalmazás naplózás engedélyezése hello váltógombok segítségével.</span><span class="sxs-lookup"><span data-stu-id="608e3-196">tooenable Application logging Go too**Monitoring** > **Diagnostic Logs** and Enable Application Logging using hello toggles.</span></span>

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

<span data-ttu-id="608e3-198">Alkalmazásnaplók tárolt tooyour Web App fájlrendszer vagy tooblob tárolási leküldött.</span><span class="sxs-lookup"><span data-stu-id="608e3-198">Application logs can be stored tooyour Web App's file system or pushed out tooblob storage.</span></span> <span data-ttu-id="608e3-199">Éles forgatókönyvek esetén ajánlott toouse blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="608e3-199">For production scenarios, it's recommended toouse blob storage.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="608e3-200">Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="608e3-200">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="608e3-201">Éles környezetben a hibanaplókat használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="608e3-201">For production scenarios, error logs are recommended.</span></span> <span data-ttu-id="608e3-202">Csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.</span><span class="sxs-lookup"><span data-stu-id="608e3-202">Only enable more verbose logging when investigating issues.</span></span>

 ### <a name="web-server-diagnostics"></a><span data-ttu-id="608e3-203">Web Server diagnosztika</span><span class="sxs-lookup"><span data-stu-id="608e3-203">Web Server Diagnostics</span></span>
<span data-ttu-id="608e3-204">Webkiszolgáló naplóinak akkor jönnek létre, akkor is, ha az alkalmazás nem tagolva..</span><span class="sxs-lookup"><span data-stu-id="608e3-204">Web Server logs are generated even if your app is not instrumented.</span></span> <span data-ttu-id="608e3-205">App Service össze tudják gyűjteni a kiszolgáló naplóiban három különböző típusú:</span><span class="sxs-lookup"><span data-stu-id="608e3-205">App Service can collect three different types of server logs:</span></span>

- <span data-ttu-id="608e3-206">**Web Server naplózása**</span><span class="sxs-lookup"><span data-stu-id="608e3-206">**Web Server Logging**</span></span>
    - <span data-ttu-id="608e3-207">Információ a HTTP-tranzakciókat hello segítségével [W3C bővített naplófájlformátum](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span><span class="sxs-lookup"><span data-stu-id="608e3-207">Information about HTTP transactions using hello [W3C extended log file format](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).</span></span>
    - <span data-ttu-id="608e3-208">Akkor hasznos, például a hello kezelt kérelmek száma, vagy hogy hány kérésnek egy adott IP-címről általános webhelymetrikák meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="608e3-208">Useful when determining overall site metrics such as hello number of requests handled or how many requests are from a specific IP address.</span></span>
- <span data-ttu-id="608e3-209">**Hibával kapcsolatos részletes naplózás**</span><span class="sxs-lookup"><span data-stu-id="608e3-209">**Detailed Error Logging**</span></span>
    - <span data-ttu-id="608e3-210">(Állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hibával kapcsolatos részletes információk.</span><span class="sxs-lookup"><span data-stu-id="608e3-210">Detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).</span></span>
    - [<span data-ttu-id="608e3-211">További információ a hibával kapcsolatos részletes naplózás</span><span class="sxs-lookup"><span data-stu-id="608e3-211">Learn more about detailed error logging</span></span>](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- <span data-ttu-id="608e3-212">**Sikertelen kérelmek nyomkövetése**</span><span class="sxs-lookup"><span data-stu-id="608e3-212">**Failed Request Tracing**</span></span>
    - <span data-ttu-id="608e3-213">Sikertelen kérelmek nyomkövetési hello IIS-összetevők többek között részletes információt tooprocess hello kérelem és hello igénybe vett idő minden összetevő használja.</span><span class="sxs-lookup"><span data-stu-id="608e3-213">Detailed information on failed requests, including a trace of hello IIS components used tooprocess hello request and hello time taken in each component.</span></span>
    - <span data-ttu-id="608e3-214">Sikertelen kérelmek naplók hasznosak, mi okozza-e egy adott HTTP hiba tooisolate tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="608e3-214">Failed request logs are useful when trying tooisolate what is causing a specific HTTP error.</span></span>
    - [<span data-ttu-id="608e3-215">További tudnivalók a sikertelen kérelmek nyomkövetése</span><span class="sxs-lookup"><span data-stu-id="608e3-215">Learn more about failed request tracing</span></span>](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

<span data-ttu-id="608e3-216">Kiszolgáló naplózási tooenable:</span><span class="sxs-lookup"><span data-stu-id="608e3-216">tooenable Server logging:</span></span>
- <span data-ttu-id="608e3-217">Nyissa meg túl**figyelés** > **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="608e3-217">go too**Monitoring** > **Diagnostic Logs**.</span></span>
- <span data-ttu-id="608e3-218">Web Server diagnosztika hello váltógombok segítségével különböző típusú hello engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="608e3-218">Enable hello different types of Web Server Diagnostics using hello toggles.</span></span>

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> <span data-ttu-id="608e3-220">Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="608e3-220">Enabling logging has an impact on your application performance and resource utilization.</span></span> <span data-ttu-id="608e3-221">Éles környezetben hibanaplókat ajánlott, csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.</span><span class="sxs-lookup"><span data-stu-id="608e3-221">For Production Scenarios, Error logs are recommended, Only Enable more verbose logging when investigating issues.</span></span>

### <a name="accessing-logs"></a><span data-ttu-id="608e3-222">Naplók elérése</span><span class="sxs-lookup"><span data-stu-id="608e3-222">Accessing Logs</span></span>
<span data-ttu-id="608e3-223">A blob Storage tárolóban tárolt naplók Azure Tártallózó hozzáférnek.</span><span class="sxs-lookup"><span data-stu-id="608e3-223">Logs stored in blob storage are accessed using Azure Storage Explorer.</span></span> <span data-ttu-id="608e3-224">Hello Web App filesystem tárolt naplók a következő elérési utak hello FTP keresztül érhetők el:</span><span class="sxs-lookup"><span data-stu-id="608e3-224">Logs stored in hello Web App's filesystem are accessed through FTP under hello following paths:</span></span>

- <span data-ttu-id="608e3-225">**Alkalmazásnaplók** - `%HOME%/LogFiles/Application/`.</span><span class="sxs-lookup"><span data-stu-id="608e3-225">**Application logs** - `%HOME%/LogFiles/Application/`.</span></span>
    - <span data-ttu-id="608e3-226">Ez a mappa tartalmaz egy vagy több alkalmazásnaplózás által előállított adatokat tartalmazó szöveges fájlt.</span><span class="sxs-lookup"><span data-stu-id="608e3-226">This folder contains one or more text files containing information produced by application logging.</span></span>
- <span data-ttu-id="608e3-227">**Sikertelen kérelmek nyomkövetési** - `%HOME%/LogFiles/W3SVC#########/`.</span><span class="sxs-lookup"><span data-stu-id="608e3-227">**Failed Request Traces** - `%HOME%/LogFiles/W3SVC#########/`.</span></span>
    - <span data-ttu-id="608e3-228">Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="608e3-228">This folder contains an XSL file and one or more XML files.</span></span>
- <span data-ttu-id="608e3-229">**Részletes hibanaplókat** - `%HOME%/LogFiles/DetailedErrors/`.</span><span class="sxs-lookup"><span data-stu-id="608e3-229">**Detailed Error Logs** - `%HOME%/LogFiles/DetailedErrors/`.</span></span>
    - <span data-ttu-id="608e3-230">Ez a mappa tartalmaz egy vagy több, a HTTP-hibák az alkalmazás által létrehozott nagy mennyiségű vonatkozó .htm fájlt.</span><span class="sxs-lookup"><span data-stu-id="608e3-230">This folder contains one or more .htm files with extensive information on HTTP errors generated by your app.</span></span>
- <span data-ttu-id="608e3-231">**Webalkalmazás-naplói** - `%HOME%/LogFiles/http/RawLogs`.</span><span class="sxs-lookup"><span data-stu-id="608e3-231">**Web Server Logs** - `%HOME%/LogFiles/http/RawLogs`.</span></span>
    - <span data-ttu-id="608e3-232">Ez a mappa tartalmaz egy vagy több szövegfájlok formátuma hello W3C bővített naplófájlformátum használata.</span><span class="sxs-lookup"><span data-stu-id="608e3-232">This folder contains one or more text files formatted using hello W3C extended log file format.</span></span>

## <span data-ttu-id="608e3-233"><a name="streaming"></a>Lépés 6 - adatfolyam-napló</span><span class="sxs-lookup"><span data-stu-id="608e3-233"><a name="streaming"></a> Step 6 - Log Streaming</span></span>
<span data-ttu-id="608e3-234">Folyamatos átviteli naplók esetén kényelmesen használható alkalmazás hibakeresés, mivel túl képest időt takaríthat[hello naplók elérése](#Accessing-Logs) FTP keresztül.</span><span class="sxs-lookup"><span data-stu-id="608e3-234">Streaming logs are convenient when debugging an application since it saves time compared too[accessing hello logs](#Accessing-Logs) through FTP.</span></span>

<span data-ttu-id="608e3-235">App Service képes adatfolyam **alkalmazásnaplók** és **webkiszolgáló naplóinak** , akkor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="608e3-235">App Service can stream **Application Logs** and **Web Server Logs** as they are generated.</span></span>

> [!TIP]
> <span data-ttu-id="608e3-236">Toostream naplók megkísérlése előtt ellenőrizze, hogy engedélyezte a gyűjtését naplók hello leírtak [naplózás](#logging) szakasz.</span><span class="sxs-lookup"><span data-stu-id="608e3-236">Before trying toostream logs, make sure you have enabled collecting logs as described in hello [Logging](#logging) section.</span></span>

<span data-ttu-id="608e3-237">toostream naplókat, nyissa meg túl**figyelés**> **naplófolyamot**.</span><span class="sxs-lookup"><span data-stu-id="608e3-237">toostream logs, go too**Monitoring**> **Log Stream**.</span></span> <span data-ttu-id="608e3-238">Válassza ki **alkalmazásnaplók** vagy **webalkalmazás-naplói** attól függően, hogy milyen információkat keres.</span><span class="sxs-lookup"><span data-stu-id="608e3-238">Select **Application Logs** or **Web server logs** depending what information you are looking for.</span></span> <span data-ttu-id="608e3-239">Itt is szüneteltetése programot, indítsa újra, és a hello puffer törölje.</span><span class="sxs-lookup"><span data-stu-id="608e3-239">From here, you can also pause, restart, and clear hello buffer.</span></span>

![Folyamatos átviteli naplók](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> <span data-ttu-id="608e3-241">Naplók csak akkor jön létre, amikor nincs forgalom hello app, megnövelheti a naplók tooget hello részletesség is több eseményeket vagy adatokat.</span><span class="sxs-lookup"><span data-stu-id="608e3-241">Logs are only generated when there is traffic on hello app, you can also increase hello verbosity of logs tooget more events or information.</span></span>

## <span data-ttu-id="608e3-242"><a name="remote"></a>7. lépés - távoli hibakeresés</span><span class="sxs-lookup"><span data-stu-id="608e3-242"><a name="remote"></a> Step 7 - Remote Debugging</span></span>
<span data-ttu-id="608e3-243">Ha PIN-kód jelölt hello forrás hello alkalmazások problémák rendelkezik, a **távoli hibakeresés** toowalk hello kód.</span><span class="sxs-lookup"><span data-stu-id="608e3-243">Once you have pin-pointed hello source of hello applications problems, use **Remote Debugging** toowalk through hello code.</span></span>

<span data-ttu-id="608e3-244">Távoli hibakeresés lehetővé csatlakoztatása a hibakereső tooyour webalkalmazás hello felhőben futó.</span><span class="sxs-lookup"><span data-stu-id="608e3-244">Remote debugging lets you attach a debugger tooyour Web App running in hello cloud.</span></span> <span data-ttu-id="608e3-245">Állítson be töréspontokat, kezelheti közvetlenül a memória, kód teljesítéséhez, és akkor is módosítani hello kódelérési-útja ugyanúgy, mint a helyileg futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="608e3-245">You can set breakpoints, manipulate memory directly, step through code, and even change hello code path just like you do for an app running locally.</span></span>

<span data-ttu-id="608e3-246">tooattach hello hibakereső tooyour app hello felhőben futó:</span><span class="sxs-lookup"><span data-stu-id="608e3-246">tooattach hello debugger tooyour app running in hello cloud:</span></span>

- <span data-ttu-id="608e3-247">A Visual Studio 2017, nyissa meg hello megoldás hello alkalmazás segítségével szeretné a toodebug</span><span class="sxs-lookup"><span data-stu-id="608e3-247">Using Visual Studio 2017, open hello solution for hello app you want toodebug</span></span>
- <span data-ttu-id="608e3-248">Néhány berendezés pont ugyanúgy, mint a helyi fejlesztési beállítása.</span><span class="sxs-lookup"><span data-stu-id="608e3-248">Set some brake points just like you would for local development.</span></span>
- <span data-ttu-id="608e3-249">Nyissa meg **cloud explorer** (parancsra + /, ctrl + x).</span><span class="sxs-lookup"><span data-stu-id="608e3-249">Open **cloud explorer** (ctr + /, ctrl + x).</span></span>
- <span data-ttu-id="608e3-250">Jelentkezzen be azure hitelesítő adataival, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="608e3-250">Log in with your azure credentials as needed.</span></span>
- <span data-ttu-id="608e3-251">Keresés hello-alkalmazáshoz toodebug</span><span class="sxs-lookup"><span data-stu-id="608e3-251">Find hello app you want toodebug</span></span>
- <span data-ttu-id="608e3-252">Válassza ki **csatolása hibakereső** űrlap hello **műveletek** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="608e3-252">Select **Attach Debugger** form hello **Actions** pane.</span></span>

![Távoli hibakeresés](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

<span data-ttu-id="608e3-254">A Visual Studio konfigurálja a távoli hibakereséshez alkalmazást, és elindítja egy böngészőablakban, amely tooyour app navigál.</span><span class="sxs-lookup"><span data-stu-id="608e3-254">Visual Studio configures your application for remote debugging and launches a browser window that navigates tooyour app.</span></span> <span data-ttu-id="608e3-255">Keresse meg az alkalmazás tootrigger break ponton keresztül, és hello kód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="608e3-255">Browse through your app tootrigger break points and step through hello code.</span></span>

> [!WARNING]
> <span data-ttu-id="608e3-256">Éles hibakeresési módban nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="608e3-256">Running in debug mode in production is not recommended.</span></span> <span data-ttu-id="608e3-257">Az éles alkalmazásban nem horizontálisan toomultiple kiszolgálópéldányok, ha hibakeresési megakadályozása hello webkiszolgáló válaszol tooother kérések.</span><span class="sxs-lookup"><span data-stu-id="608e3-257">If your production app is not scaled out toomultiple server instances, debugging prevent hello web server from responding tooother requests.</span></span> <span data-ttu-id="608e3-258">A termelési problémák elhárításához, a legjobb erőforrás túlságosan[naplózási](#logging) és [Application Insights](#insights).</span><span class="sxs-lookup"><span data-stu-id="608e3-258">For troubleshooting production problems, your best resource is too[configure logging](#logging) and [Application Insights](#insights).</span></span>



## <span data-ttu-id="608e3-259"><a name="explorer"></a>8 - Process Explorert. lépés</span><span class="sxs-lookup"><span data-stu-id="608e3-259"><a name="explorer"></a> Step 8 - Process Explorer</span></span>
<span data-ttu-id="608e3-260">Ha az alkalmazás egy példánya, mint toomore van horizontálisan **process Explorert** példány konkrét problémák azonosításához nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="608e3-260">When your application is scaled out toomore than one instance, **process explorer** can help you identify instance specific problems.</span></span>

<span data-ttu-id="608e3-261">Használjon **Explorer feldolgozni** számára:</span><span class="sxs-lookup"><span data-stu-id="608e3-261">Use **Process Explorer** to:</span></span>

- <span data-ttu-id="608e3-262">Minden hello folyamat felsorolni az App Service-csomag különböző példányai között.</span><span class="sxs-lookup"><span data-stu-id="608e3-262">Enumerate all hello processes across different instances of your App Service plan.</span></span>
- <span data-ttu-id="608e3-263">Részletekbe menően tárhatják, és tekintse meg hello kezeli, és minden folyamathoz tartozó modulok.</span><span class="sxs-lookup"><span data-stu-id="608e3-263">Drill down and view hello handles and modules associated with each process.</span></span>
- <span data-ttu-id="608e3-264">Hello nézet CPU, működik-e beállítva, és a szál darabszáma feldolgozni sorozatos folyamatok azonosította szintű toohelp</span><span class="sxs-lookup"><span data-stu-id="608e3-264">View CPU, Working Set, and Thread count at hello process level toohelp you identify runaway processes</span></span>
- <span data-ttu-id="608e3-265">A megnyitott fájlleíró keresse meg és még kill egy adott példányt.</span><span class="sxs-lookup"><span data-stu-id="608e3-265">Find open file handles, and even kill a specific process instance.</span></span>

<span data-ttu-id="608e3-266">Folyamat Explorer alatt található **figyelés** > **Process Explorer**.</span><span class="sxs-lookup"><span data-stu-id="608e3-266">Process Explorer can be found under **Monitoring** > **Process Explorer**.</span></span>

![Process Explorer](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <span data-ttu-id="608e3-268"><a name="insights"></a>9 - Application insights szolgáltatással. lépés</span><span class="sxs-lookup"><span data-stu-id="608e3-268"><a name="insights"></a> Step 9 - Application Insights</span></span>
<span data-ttu-id="608e3-269">**Az Application Insights** alkalmazásprofil-készítési és speciális megfigyelési képességeket biztosít az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="608e3-269">**Application Insights** provides application profiling and advanced monitoring capabilities for your app.</span></span>

<span data-ttu-id="608e3-270">Az Application Insights toodetect használni és kivételeket és a webalkalmazás a teljesítménnyel kapcsolatos problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="608e3-270">Use Application Insights toodetect and diagnose exceptions and performance issues in your Web App.</span></span>

<span data-ttu-id="608e3-271">A webalkalmazás alatt engedélyezheti az Application Insights **figyelés** > **Application insights szolgáltatással**</span><span class="sxs-lookup"><span data-stu-id="608e3-271">You can enable Application Insights for your Web App under **Monitoring** > **Application Insights**</span></span>

> [!NOTE]
> <span data-ttu-id="608e3-272">Az Application Insights kérhetik tooinstall hello Application Insights hely bővítmény toostart adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="608e3-272">Application Insights might prompt you tooinstall hello Application Insights site extension toostart collecting data.</span></span> <span data-ttu-id="608e3-273">Hello hely bővítmény telepítése azt eredményezi, újra kell indítani az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="608e3-273">Installing hello site extension causes an application restart.</span></span>

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

<span data-ttu-id="608e3-275">Az Application Insights gazdag lehetőség van beállítva, toolearn több, hivatkozásokból hello hello szerepel [lépések](#next) szakasz.</span><span class="sxs-lookup"><span data-stu-id="608e3-275">Application Insights has a rich feature set, toolearn more, follow hello links included in hello [Next Steps](#next) section.</span></span>

## <span data-ttu-id="608e3-276"><a name="next"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="608e3-276"><a name="next"></a> Next steps</span></span>

 - [<span data-ttu-id="608e3-277">Mi az Application Insights</span><span class="sxs-lookup"><span data-stu-id="608e3-277">What is Application Insights</span></span>](..\application-insights\app-insights-overview.md)
 - [<span data-ttu-id="608e3-278">Az Application insights szolgáltatással Azure webes alkalmazás teljesítményének figyelése</span><span class="sxs-lookup"><span data-stu-id="608e3-278">Monitor Azure web app performance with Application Insights</span></span>](..\application-insights\app-insights-azure-web-apps.md)
 - [<span data-ttu-id="608e3-279">Rendelkezésre állás figyelése és reakcióidőt, a webhely, az Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="608e3-279">Monitor availability and responsiveness of any web site with Application Insights</span></span>](..\application-insights\app-insights-monitor-web-app-availability.md)
