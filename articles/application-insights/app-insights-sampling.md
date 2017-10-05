---
title: "Az Azure Application Insights telemetria mintavételi |} Microsoft Docs"
description: "Hogyan kell fenntartani a vezérlése alatt telemetriai adatok mennyiségét."
services: application-insights
documentationcenter: windows
author: vgorbenko
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bwren
ms.openlocfilehash: ceaeced414c9c302fba335b4578bcdcbfaef0410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="sampling-in-application-insights"></a><span data-ttu-id="90d20-103">Application Insights-mintavétel</span><span class="sxs-lookup"><span data-stu-id="90d20-103">Sampling in Application Insights</span></span>


<span data-ttu-id="90d20-104">A mintavétel az szolgáltatása [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="90d20-104">Sampling is a feature in [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="90d20-105">Az ajánlott módszer a telemetriai adatok és a tárolás, az alkalmazásadatok statisztikailag megfelelő elemzési adatainak megőrzése mellett csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90d20-105">It is the recommended way to reduce telemetry traffic and storage, while preserving  a statistically correct analysis of application data.</span></span> <span data-ttu-id="90d20-106">A szűrő elemek kapcsolódó, választja ki, így navigálhat diagnosztikai vizsgálatok során elemek között.</span><span class="sxs-lookup"><span data-stu-id="90d20-106">The filter selects items that are related, so that you can navigate between items when you are doing diagnostic investigations.</span></span>
<span data-ttu-id="90d20-107">Metrika számát a portálon jelennek meg, amikor azok figyelembe véve a mintavételi minimalizálása érdekében a hatása, ha a statisztikákat a rendszer renormalized.</span><span class="sxs-lookup"><span data-stu-id="90d20-107">When metric counts are presented to you in the portal, they are renormalized to take account of the sampling, to minimize any effect on the statistics.</span></span>

<span data-ttu-id="90d20-108">Mintavételi csökkenti a forgalom és az adatok költségeket, és segít elkerülni a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="90d20-108">Sampling reduces traffic and data costs, and helps you avoid throttling.</span></span>

## <a name="in-brief"></a><span data-ttu-id="90d20-109">Röviden:</span><span class="sxs-lookup"><span data-stu-id="90d20-109">In brief:</span></span>
* <span data-ttu-id="90d20-110">Mintavételi őrzi meg az 1  *n*  rögzíti, és elveti a többi.</span><span class="sxs-lookup"><span data-stu-id="90d20-110">Sampling retains 1 in *n* records and discards the rest.</span></span> <span data-ttu-id="90d20-111">Például akkor lehet, hogy továbbra is 1: 5 események, a mintavételi ráta 20 %-os.</span><span class="sxs-lookup"><span data-stu-id="90d20-111">For example, it might retain 1 in 5 events, a sampling rate of 20%.</span></span> 
* <span data-ttu-id="90d20-112">Mintavételi automatikusan történik, ha az alkalmazás nagy mennyiségű telemetriai adatokat küld az ASP.NET web server apps.</span><span class="sxs-lookup"><span data-stu-id="90d20-112">Sampling happens automatically if your application sends a lot of telemetry, in ASP.NET web server apps.</span></span>
* <span data-ttu-id="90d20-113">Azt is beállíthatja, hogy a mintavételi manuálisan, vagy a portál az árképzést ismertető oldalra; vagy az ASP.NET SDK .config fájl is a a hálózati forgalom csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90d20-113">You can also set sampling manually, either in the portal on the pricing page; or in the ASP.NET SDK in the .config file, to also reduce the network traffic.</span></span>
* <span data-ttu-id="90d20-114">Ha egyéni események naplózása, és győződjön meg arról, hogy egy megőrzött vagy elvetett együtt szeretné, győződjön meg arról, hogy rendelkeznek-e az azonos OperationID azonosítójú érték.</span><span class="sxs-lookup"><span data-stu-id="90d20-114">If you log custom events and you want to make sure that a set of events is either retained or discarded together, make sure that they have the same OperationId value.</span></span>
* <span data-ttu-id="90d20-115">A mintavételi osztó  *n*  tulajdonságában rekordokban levő jelentett `itemCount`, amelyen a Keresés a rövid név "kérelem száma" vagy "események száma" alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="90d20-115">The sampling divisor *n* is reported in each record in the property `itemCount`, which in Search appears under the friendly name "request count" or "event count".</span></span> <span data-ttu-id="90d20-116">Ha a mintavételi nincs a művelet, `itemCount==1`.</span><span class="sxs-lookup"><span data-stu-id="90d20-116">When sampling is not in operation, `itemCount==1`.</span></span>
* <span data-ttu-id="90d20-117">Írás Analytics lekérdezések, akkor [mintavételi figyelembe](app-insights-analytics-tour.md#counting-sampled-data).</span><span class="sxs-lookup"><span data-stu-id="90d20-117">If you write Analytics queries, you should [take account of sampling](app-insights-analytics-tour.md#counting-sampled-data).</span></span> <span data-ttu-id="90d20-118">Különösen helyett egyszerűen számbavételi rekordok, használjon `summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="90d20-118">In particular, instead of simply counting records, you should use `summarize sum(itemCount)`.</span></span>

## <a name="types-of-sampling"></a><span data-ttu-id="90d20-119">Mintavételi típusai</span><span class="sxs-lookup"><span data-stu-id="90d20-119">Types of sampling</span></span>
<span data-ttu-id="90d20-120">Alternatív mintavételi három módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="90d20-120">There are three alternative sampling methods:</span></span>

* <span data-ttu-id="90d20-121">**Adaptív mintavételi** automatikusan beállítja az SDK-t az ASP.NET-alkalmazás által küldött telemetriai adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="90d20-121">**Adaptive sampling** automatically adjusts the volume of telemetry sent from the SDK in your ASP.NET app.</span></span> <span data-ttu-id="90d20-122">Alapértelmezés szerint az SDK v 2.0.0-beta3.</span><span class="sxs-lookup"><span data-stu-id="90d20-122">Default from SDK v 2.0.0-beta3.</span></span> <span data-ttu-id="90d20-123">Jelenleg csak az ASP.NET kiszolgálóoldali telemetriai adat esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="90d20-123">Currently available for ASP.NET server-side telemetry only.</span></span> 
* <span data-ttu-id="90d20-124">**Rögzített mintavételi** mindkét az ASP.NET kiszolgálóról, és a felhasználók böngészőjének a telemetriai adatok mennyiségét csökkenti.</span><span class="sxs-lookup"><span data-stu-id="90d20-124">**Fixed-rate sampling** reduces the volume of telemetry sent from both your ASP.NET server and from your users' browsers.</span></span> <span data-ttu-id="90d20-125">A sebesség beállítása.</span><span class="sxs-lookup"><span data-stu-id="90d20-125">You set the rate.</span></span> <span data-ttu-id="90d20-126">Az ügyfél és kiszolgáló szinkronizálja a mintavételi, a keresés, navigálhat kapcsolódó Lapmegtekintések és kérések között.</span><span class="sxs-lookup"><span data-stu-id="90d20-126">The client and server will synchronize their sampling so that, in Search, you can navigate between related page views and requests.</span></span>
* <span data-ttu-id="90d20-127">**Adatfeldolgozást mintavételi** működik az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="90d20-127">**Ingestion sampling** works in the Azure portal.</span></span> <span data-ttu-id="90d20-128">Törli az alkalmazásból, Ön által beállított ütemben érkező telemetriai adatok némelyike.</span><span class="sxs-lookup"><span data-stu-id="90d20-128">It discards some of the telemetry that arrives from your app, at a rate that you set.</span></span> <span data-ttu-id="90d20-129">Nem csökkenthető a telemetria-forgalom, de lehetővé teszi a havi kvótán belül.</span><span class="sxs-lookup"><span data-stu-id="90d20-129">It doesn't reduce telemetry traffic, but helps you keep within your monthly quota.</span></span> <span data-ttu-id="90d20-130">A nagy adatfeldolgozást mintavételi előnye, hogy az alkalmazás üzembe helyezésével állíthatja be, és egységesen összes kiszolgálók és ügyfelek esetében működik.</span><span class="sxs-lookup"><span data-stu-id="90d20-130">The big advantage of ingestion sampling is that you can set it without redeploying your app, and it works uniformly for all servers and clients.</span></span> 

<span data-ttu-id="90d20-131">Ha az adaptív vagy rögzített arány mintavételi művelet, adatfeldolgozást mintavételi le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="90d20-131">If Adaptive or Fixed rate sampling are in operation, Ingestion sampling is disabled.</span></span>

## <a name="ingestion-sampling"></a><span data-ttu-id="90d20-132">Adatfeldolgozást mintavétel</span><span class="sxs-lookup"><span data-stu-id="90d20-132">Ingestion sampling</span></span>
<span data-ttu-id="90d20-133">Ezt az űrlapot mintavételi a pont, ahol a webkiszolgáló böngészők és eszközök telemetriai eléri az Application Insights szolgáltatás végpont működik.</span><span class="sxs-lookup"><span data-stu-id="90d20-133">This form of sampling operates at the point where the telemetry from your web server, browsers, and devices reaches the Application Insights service endpoint.</span></span> <span data-ttu-id="90d20-134">Ezzel nem csökkenthető az alkalmazásból küldött telemetriai-forgalom, bár ez csökkentheti a feldolgozott és maradnak (majd díjat) az Application Insights által.</span><span class="sxs-lookup"><span data-stu-id="90d20-134">Although it doesn't reduce the telemetry traffic sent from your app, it does reduce the amount processed and retained (and charged for) by Application Insights.</span></span>

<span data-ttu-id="90d20-135">Az ilyen mintavételi típusú, ha az alkalmazás gyakran kerül felett a havi kvótát, és nem kell a mintavételi SDK-alapú típusú bármelyikével lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="90d20-135">Use this type of sampling if your app often goes over its monthly quota and you don't have the option of using either of the SDK-based types of sampling.</span></span> 

<span data-ttu-id="90d20-136">A mintavételi ráta beállítását a kvóták és árképzési panel:</span><span class="sxs-lookup"><span data-stu-id="90d20-136">Set the sampling rate in the Quotas and Pricing blade:</span></span>

![Alkalmazás – áttekintés paneljén kattintson a beállítások, a kvóta, a mintákat, majd válassza ki a mintavételi ráta, és kattintson a frissítés gombra.](./media/app-insights-sampling/04.png)

<span data-ttu-id="90d20-138">Mintavételi más típusú, mint az algoritmus megtartja a kapcsolódó telemetriai elemeket.</span><span class="sxs-lookup"><span data-stu-id="90d20-138">Like other types of sampling, the algorithm retains related telemetry items.</span></span> <span data-ttu-id="90d20-139">Például a telemetriai adatokat, a Keresés most vizsgálatakor ellenőrizze, hogy képes lesz megkereshető egy adott kivételhez kapcsolódó a kérelem.</span><span class="sxs-lookup"><span data-stu-id="90d20-139">For example, when you're inspecting the telemetry in Search, you'll be able to find the request related to a particular exception.</span></span> <span data-ttu-id="90d20-140">A metrika számít például kérelmek aránya, és kivétel arány megfelelően megőrződnek.</span><span class="sxs-lookup"><span data-stu-id="90d20-140">Metric counts such as request rate and exception rate are correctly retained.</span></span>

<span data-ttu-id="90d20-141">Hogy a rendszer elveti a mintavétellel már nem érhetők el bármilyen Application Insights szolgáltatás például [a folyamatos exportálás](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="90d20-141">Data points that are discarded by sampling are not available in any Application Insights feature such as [Continuous Export](app-insights-export-telemetry.md).</span></span>

<span data-ttu-id="90d20-142">Adatfeldolgozást mintavételi nem működik, amikor az SDK-alapú adaptív vagy rögzített mintavételi művelet van.</span><span class="sxs-lookup"><span data-stu-id="90d20-142">Ingestion sampling doesn't operate while SDK-based adaptive or fixed-rate sampling is in operation.</span></span> <span data-ttu-id="90d20-143">Ha a mintavételi ráta az SDK-t a 100 %-nál kisebb, az Ön által beállított adatfeldolgozást mintavételi ráta figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="90d20-143">If the sampling rate at the SDK is less than 100%, then the ingestion sampling rate that you set is ignored.</span></span>

> [!WARNING]
> <span data-ttu-id="90d20-144">A csempén megjelenő érték adatfeldolgozást mintavételi beállított értékeket.</span><span class="sxs-lookup"><span data-stu-id="90d20-144">The value shown on the tile indicates the value that you set for ingestion sampling.</span></span> <span data-ttu-id="90d20-145">A tényleges mintavételi ráta az nem jelent, ha SDK mintavételi művelet.</span><span class="sxs-lookup"><span data-stu-id="90d20-145">It doesn't represent the actual sampling rate if SDK sampling is in operation.</span></span>
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a><span data-ttu-id="90d20-146">A webkiszolgáló adaptív mintavétel</span><span class="sxs-lookup"><span data-stu-id="90d20-146">Adaptive sampling at your web server</span></span>
<span data-ttu-id="90d20-147">Adaptív mintavételi az Application Insights SDK az ASP.NET v 2.0.0-beta3 és újabb verzióihoz érhető el, és alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="90d20-147">Adaptive sampling is available for the Application Insights SDK for ASP.NET v 2.0.0-beta3 and later, and is enabled by default.</span></span> 

<span data-ttu-id="90d20-148">Adaptív mintavételi hatással van az Application Insights szolgáltatás a webalkalmazás-kiszolgáló által küldött telemetriai adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="90d20-148">Adaptive sampling affects the volume of telemetry sent from your web server app to the Application Insights service.</span></span> <span data-ttu-id="90d20-149">A kötet tartani a megadott legnagyobb mértékben forgalom belül automatikusan módosul.</span><span class="sxs-lookup"><span data-stu-id="90d20-149">The volume is adjusted automatically to keep within a specified maximum rate of traffic.</span></span>

<span data-ttu-id="90d20-150">Azt nem működik, telemetriai adatokat kis mennyiségű úgy egy alkalmazást a hibakeresés, vagy nem befolyásolja az alacsony használat rendelkező webhely.</span><span class="sxs-lookup"><span data-stu-id="90d20-150">It doesn't operate at low volumes of telemetry, so an app in debugging or a website with low usage won't be affected.</span></span>

<span data-ttu-id="90d20-151">A célkötet eléréséhez a generált telemetriai adatok némelyike program elveti.</span><span class="sxs-lookup"><span data-stu-id="90d20-151">To achieve the target volume, some of the telemetry generated is discarded.</span></span> <span data-ttu-id="90d20-152">De mintavételi más típusú, mint az algoritmus megtartja a kapcsolódó telemetriai elemeket.</span><span class="sxs-lookup"><span data-stu-id="90d20-152">But like other types of sampling, the algorithm retains related telemetry items.</span></span> <span data-ttu-id="90d20-153">Például a telemetriai adatokat, a Keresés most vizsgálatakor ellenőrizze, hogy képes lesz megkereshető egy adott kivételhez kapcsolódó a kérelem.</span><span class="sxs-lookup"><span data-stu-id="90d20-153">For example, when you're inspecting the telemetry in Search, you'll be able to find the request related to a particular exception.</span></span> 

<span data-ttu-id="90d20-154">A metrika számít például kérelmek aránya, és kivétel arány igazítva kiegyensúlyozása érdekében a mintavételi ráta, hogy azok körülbelül megfelelő értékek megjelenítése a metrika Intézőben.</span><span class="sxs-lookup"><span data-stu-id="90d20-154">Metric counts such as request rate and exception rate are adjusted to compensate for the sampling rate, so that they show approximately correct values in Metric Explorer.</span></span>

<span data-ttu-id="90d20-155">**Frissítse a projekt NuGet** csomagok a legújabb *kiadás előtti* Application Insights verziója: kattintson a jobb gombbal a projektre a Megoldáskezelőben, válassza a NuGet-csomagok kezelése, ellenőrizze **közé tartoznak az előzetes** , és keressen a Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="90d20-155">**Update your project's NuGet** packages to the latest *pre-release* version of Application Insights: Right-click the project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 

<span data-ttu-id="90d20-156">A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), a több paraméterrel beállíthatja a `AdaptiveSamplingTelemetryProcessor` csomópont.</span><span class="sxs-lookup"><span data-stu-id="90d20-156">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), you can adjust several parameters in the `AdaptiveSamplingTelemetryProcessor` node.</span></span> <span data-ttu-id="90d20-157">Az ábrán látható az alapértelmezett értékeket:</span><span class="sxs-lookup"><span data-stu-id="90d20-157">The figures shown are the default values:</span></span>

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    <span data-ttu-id="90d20-158">Az adaptív algoritmus célja a cél gyakorisága **minden kiszolgáló állomáson**.</span><span class="sxs-lookup"><span data-stu-id="90d20-158">The target rate that the adaptive algorithm aims for **on each server host**.</span></span> <span data-ttu-id="90d20-159">Ha sok gazdagép a webalkalmazás fut, csökkentse a ezt az értéket, hogy az Application Insights portáljáról-forgalmat a cél aránya belül.</span><span class="sxs-lookup"><span data-stu-id="90d20-159">If your web app runs on many hosts, reduce this value so as to remain within your target rate of traffic at the Application Insights portal.</span></span>
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    <span data-ttu-id="90d20-160">Az időköz, ahol az aktuális telemetriai mérték újraértékelése is megtörténik.</span><span class="sxs-lookup"><span data-stu-id="90d20-160">The interval at which the current rate of telemetry is re-evaluated.</span></span> <span data-ttu-id="90d20-161">Kiértékelési mozgóátlaga szerint történik.</span><span class="sxs-lookup"><span data-stu-id="90d20-161">Evaluation is performed as a moving average.</span></span> <span data-ttu-id="90d20-162">Rövidítse le ezt az időközt, ha a telemetriai adatok hirtelen felszakadásáig kívánt.</span><span class="sxs-lookup"><span data-stu-id="90d20-162">You might want to shorten this interval if your telemetry is liable to sudden bursts.</span></span>
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    <span data-ttu-id="90d20-163">Mintavételi százalékos érték módosításokat, ha hamarosan hogyan után azt engedélyezettek csökkenthető a mintavételi arány újra kevesebb adatot rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="90d20-163">When sampling percentage value changes, how soon after are we allowed to lower sampling percentage again to capture less data.</span></span>
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    <span data-ttu-id="90d20-164">Mintavételi százalékos érték módosításokat, ha hamarosan hogyan után azt engedélyezettek újra az adatok rögzítéséhez mintavételi arány növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90d20-164">When sampling percentage value changes, how soon after are we allowed to increase sampling percentage again to capture more data.</span></span>
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    <span data-ttu-id="90d20-165">Mivel százalékos mintavételi változik, mi az a minimális értéke azt hogy jogosult-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="90d20-165">As sampling percentage varies, what is the minimum value we're allowed to set.</span></span>
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    <span data-ttu-id="90d20-166">Mivel százalékos mintavételi változik, mi az a maximális érték azt hogy jogosult-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="90d20-166">As sampling percentage varies, what is the maximum value we're allowed to set.</span></span>
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    <span data-ttu-id="90d20-167">A mozgóátlag számítás a súlyozás a legfrissebb értéket hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="90d20-167">In the calculation of the moving average, the weight assigned to the most recent value.</span></span> <span data-ttu-id="90d20-168">Az 1-nél kisebb vagy egyenlő, érték.</span><span class="sxs-lookup"><span data-stu-id="90d20-168">Use a value equal to or less than 1.</span></span> <span data-ttu-id="90d20-169">A kisebb értékek módosításokat az algoritmus kevesebb reaktív hirtelen számára.</span><span class="sxs-lookup"><span data-stu-id="90d20-169">Smaller values make the algorithm less reactive to sudden changes.</span></span>
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    <span data-ttu-id="90d20-170">Ha az alkalmazás elkezdte rendelt értéket.</span><span class="sxs-lookup"><span data-stu-id="90d20-170">The value assigned when the app has just started.</span></span> <span data-ttu-id="90d20-171">Nem csökkenti a akkor hibakeresése közben.</span><span class="sxs-lookup"><span data-stu-id="90d20-171">Don't reduce this while you're debugging.</span></span> 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    <span data-ttu-id="90d20-172">Egy pontosvesszővel tagolt listáját, amelyek nem kívánt mintát venni.</span><span class="sxs-lookup"><span data-stu-id="90d20-172">A semi-colon delimited list of types that you do not want to be sampled.</span></span> <span data-ttu-id="90d20-173">Felismert típusok a következők: függőségi, esemény, kivétel, PageView, kérelem, nyomkövetési.</span><span class="sxs-lookup"><span data-stu-id="90d20-173">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="90d20-174">A megadott típusú példányainak továbbításuk; a nem megadott típusok lekérdező.</span><span class="sxs-lookup"><span data-stu-id="90d20-174">All instances of the specified types are transmitted; the types that are not specified are sampled.</span></span>

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    <span data-ttu-id="90d20-175">Egy pontosvesszővel tagolt listáját kell mintát venni kívánt típusokat.</span><span class="sxs-lookup"><span data-stu-id="90d20-175">A semi-colon delimited list of types that you want to be sampled.</span></span> <span data-ttu-id="90d20-176">Felismert típusok a következők: függőségi, esemény, kivétel, PageView, kérelem, nyomkövetési.</span><span class="sxs-lookup"><span data-stu-id="90d20-176">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="90d20-177">A megadott típusoknak lekérdező; a más típusú példányainak mindig továbbítja.</span><span class="sxs-lookup"><span data-stu-id="90d20-177">The specified types are sampled; all instances of the other types will always be transmitted.</span></span>


<span data-ttu-id="90d20-178">**Kapcsolja ki a** adaptív mintavételi, távolítsa el a AdaptiveSamplingTelemetryProcessor csomópont applicationinsights-config.</span><span class="sxs-lookup"><span data-stu-id="90d20-178">**To switch off** adaptive sampling, remove the AdaptiveSamplingTelemetryProcessor node from applicationinsights-config.</span></span>

### <a name="alternative-configure-adaptive-sampling-in-code"></a><span data-ttu-id="90d20-179">Alternatív: Adaptív mintavétel konfigurálása a kódban</span><span class="sxs-lookup"><span data-stu-id="90d20-179">Alternative: configure adaptive sampling in code</span></span>
<span data-ttu-id="90d20-180">Mintavétel a .config kiterjesztésű fájl helyett, a kódot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="90d20-180">Instead of adjusting sampling in the .config file, you can use code.</span></span> <span data-ttu-id="90d20-181">Ez lehetővé teszi, hogy adjon meg egy visszahívási függvény, amelyet mindig, amikor a mintavételi ráta újraértékelése is megtörténik.</span><span class="sxs-lookup"><span data-stu-id="90d20-181">This allows you to specify a callback function that is invoked whenever the sampling rate is re-evaluated.</span></span> <span data-ttu-id="90d20-182">Használhat, például, hogy megtudja, milyen mintavételi ráta használatban van.</span><span class="sxs-lookup"><span data-stu-id="90d20-182">You could use this, for example, to find out what sampling rate is being used.</span></span>

<span data-ttu-id="90d20-183">Távolítsa el a `AdaptiveSamplingTelemetryProcessor` csomópontot a .config fájlból.</span><span class="sxs-lookup"><span data-stu-id="90d20-183">Remove the `AdaptiveSamplingTelemetryProcessor` node from the .config file.</span></span>

<span data-ttu-id="90d20-184">*C#*</span><span class="sxs-lookup"><span data-stu-id="90d20-184">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust the settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;

    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report the sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="90d20-185">([További információ a telemetriai adatok processzorok](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="90d20-185">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a><span data-ttu-id="90d20-186">Mintavételi JavaScript weblapok</span><span class="sxs-lookup"><span data-stu-id="90d20-186">Sampling for web pages with JavaScript</span></span>
<span data-ttu-id="90d20-187">Konfigurálhatja a weblapok rögzített mintavételi tetszőleges kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="90d20-187">You can configure web pages for fixed-rate sampling from any server.</span></span> 

<span data-ttu-id="90d20-188">Ha Ön [a weblapok konfigurálja az Application Insights](app-insights-javascript.md), módosítsa a kódrészletet, hogy az Application Insights portáljáról.</span><span class="sxs-lookup"><span data-stu-id="90d20-188">When you [configure the web pages for Application Insights](app-insights-javascript.md), modify the snippet that you get from the Application Insights portal.</span></span> <span data-ttu-id="90d20-189">(Az ASP.NET alkalmazásokban, a részlet általában kerül a _Layout.cshtml.)  Például a sor beszúrása `samplingPercentage: 10,` előtt a instrumentation kulcs:</span><span class="sxs-lookup"><span data-stu-id="90d20-189">(In ASP.NET apps, the snippet typically goes in _Layout.cshtml.)  Insert a line like `samplingPercentage: 10,` before the instrumentation key:</span></span>

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

<span data-ttu-id="90d20-190">A mintavételi arány válassza százalékában, amelynek mérete megközelítőleg 100/N, ahol N az egész szám.</span><span class="sxs-lookup"><span data-stu-id="90d20-190">For the sampling percentage, choose a percentage that is close to 100/N where N is an integer.</span></span>  <span data-ttu-id="90d20-191">Mintavételi jelenleg nem támogatja a más értékek.</span><span class="sxs-lookup"><span data-stu-id="90d20-191">Currently sampling doesn't support other values.</span></span>

<span data-ttu-id="90d20-192">Ha rögzített mintavételi a kiszolgálón is engedélyezi, az ügyfelek és a kiszolgáló szinkronizálja, a keresés, navigálhat kapcsolódó Lapmegtekintések és kérések között.</span><span class="sxs-lookup"><span data-stu-id="90d20-192">If you also enable fixed-rate sampling at the server, the clients and server will synchronize so that, in Search, you can  navigate between related page views and requests.</span></span>

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a><span data-ttu-id="90d20-193">Az ASP.NET-webhelyeket rögzített mintavétel</span><span class="sxs-lookup"><span data-stu-id="90d20-193">Fixed-rate sampling for ASP.NET web sites</span></span>
<span data-ttu-id="90d20-194">Rögzített alapján mintavételi csökkenti a webkiszolgálót és a böngészők által küldött forgalmat.</span><span class="sxs-lookup"><span data-stu-id="90d20-194">Fixed rate sampling reduces the traffic sent from your web server and web browsers.</span></span> <span data-ttu-id="90d20-195">Adaptív mintavételi eltérően csökkenti telemetriai rögzített kulcs határoz meg.</span><span class="sxs-lookup"><span data-stu-id="90d20-195">Unlike adaptive sampling, it reduces telemetry at a fixed rate decided by you.</span></span> <span data-ttu-id="90d20-196">Azt is szinkronizálja az ügyfél és kiszolgáló mintavételi, hogy a kapcsolódó elemek megmaradnak – például, hogy a nézet a keresési tekinti meg, ha a kapcsolódó kérelemre található.</span><span class="sxs-lookup"><span data-stu-id="90d20-196">It also synchronizes the client and server sampling so that related items are retained - for example, so that if you look at a page view in Search, you can find its related request.</span></span>

<span data-ttu-id="90d20-197">A mintavételi algoritmus megtartja a kapcsolódó elemeket.</span><span class="sxs-lookup"><span data-stu-id="90d20-197">The sampling algorithm retains related items.</span></span> <span data-ttu-id="90d20-198">Az egyes HTTP-kérelmek esemény, ezért a kapcsolódó események vagy elvetett vagy továbbítani.</span><span class="sxs-lookup"><span data-stu-id="90d20-198">For each HTTP request event, it and its related events are either discarded or transmitted.</span></span> 

<span data-ttu-id="90d20-199">A Metrikaböngészőben például a kérelem és a kivételesemények számát díjszabás vannak faktor szorzata kiegyensúlyozása érdekében a mintavételi ráta, hogy azok körülbelül helyességét.</span><span class="sxs-lookup"><span data-stu-id="90d20-199">In Metrics Explorer, rates such as request and exception counts are multiplied by a factor to compensate for the sampling rate, so that they are approximately correct.</span></span>

1. <span data-ttu-id="90d20-200">**A projekt NuGet-csomagok frissítése** a legújabb *kiadás előtti* Application Insights verzióját.</span><span class="sxs-lookup"><span data-stu-id="90d20-200">**Update your project's NuGet packages** to the latest *pre-release* version of Application Insights.</span></span> <span data-ttu-id="90d20-201">Kattintson a jobb gombbal a projektre a Megoldáskezelőben, válassza a NuGet-csomagok kezelése, ellenőrizze **közé tartoznak az előzetes** és Microsoft.ApplicationInsights.Web megkereséséhez.</span><span class="sxs-lookup"><span data-stu-id="90d20-201">Right-click the project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 
2. <span data-ttu-id="90d20-202">**Tiltsa le a adaptív mintavételi**: A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), távolítsa el vagy megjegyzéssé a `AdaptiveSamplingTelemetryProcessor` csomópont.</span><span class="sxs-lookup"><span data-stu-id="90d20-202">**Disable adaptive sampling**: In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), remove or comment out the `AdaptiveSamplingTelemetryProcessor` node.</span></span>
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. <span data-ttu-id="90d20-203">**A rögzített mintavételi modul engedélyezése.**</span><span class="sxs-lookup"><span data-stu-id="90d20-203">**Enable the fixed-rate sampling module.**</span></span> <span data-ttu-id="90d20-204">Adja hozzá ezt a kódrészletet [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="90d20-204">Add this snippet to [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> <span data-ttu-id="90d20-205">A mintavételi arány válassza százalékában, amelynek mérete megközelítőleg 100/N, ahol N az egész szám.</span><span class="sxs-lookup"><span data-stu-id="90d20-205">For the sampling percentage, choose a percentage that is close to 100/N where N is an integer.</span></span>  <span data-ttu-id="90d20-206">Mintavételi jelenleg nem támogatja a más értékek.</span><span class="sxs-lookup"><span data-stu-id="90d20-206">Currently sampling doesn't support other values.</span></span>
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a><span data-ttu-id="90d20-207">Másik megoldás: a kiszolgáló kódjában található rögzített mintavételi engedélyezése</span><span class="sxs-lookup"><span data-stu-id="90d20-207">Alternative: enable fixed-rate sampling in your server code</span></span>
<span data-ttu-id="90d20-208">Helyett, ha a mintavételi paramétert a .config kiterjesztésű fájl, a kódot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="90d20-208">Instead of setting the sampling parameter in the .config file, you can use code.</span></span> 

<span data-ttu-id="90d20-209">*C#*</span><span class="sxs-lookup"><span data-stu-id="90d20-209">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="90d20-210">([További információ a telemetriai adatok processzorok](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="90d20-210">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

## <a name="when-to-use-sampling"></a><span data-ttu-id="90d20-211">Mikor érdemes használni a mintavételi?</span><span class="sxs-lookup"><span data-stu-id="90d20-211">When to use sampling?</span></span>
<span data-ttu-id="90d20-212">Adaptív mintavételi automatikusan engedélyezi az ASP.NET SDK verzió 2.0.0-beta3 használatakor vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="90d20-212">Adaptive sampling is automatically enabled if you use the ASP.NET SDK version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="90d20-213">Függetlenül attól, milyen SDK verzióját használja használhat adatfeldolgozást mintavételi (a kiszolgáló).</span><span class="sxs-lookup"><span data-stu-id="90d20-213">No matter what SDK version you use, you can use ingestion sampling (at our server).</span></span>

<span data-ttu-id="90d20-214">Mintavételi legtöbb kis és közepes méretű alkalmazásokhoz nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="90d20-214">You don’t need sampling for most small and medium size applications.</span></span> <span data-ttu-id="90d20-215">A leghasznosabb információkat a diagnosztikai és a legpontosabb statisztika nyerhetők adatokat gyűjt a felhasználói tevékenységekről.</span><span class="sxs-lookup"><span data-stu-id="90d20-215">The most useful diagnostic information and most accurate statistics are obtained by collecting data on all your user activities.</span></span> 

<span data-ttu-id="90d20-216">A mintavételi fő előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="90d20-216">The main advantages of sampling are:</span></span>

* <span data-ttu-id="90d20-217">Application Insights szolgáltatás elhagyta a(z) ("szabályozások") adatpontok rövid üzenetet küld a nagyon nagy mértékben telemetriai adatot az alkalmazás időintervallum.</span><span class="sxs-lookup"><span data-stu-id="90d20-217">Application Insights service drops ("throttles") data points when your app sends a very high rate of telemetry in short time interval.</span></span> 
* <span data-ttu-id="90d20-218">Tartsa belül a [kvóta](app-insights-pricing.md) az adatokat a tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="90d20-218">To keep within the [quota](app-insights-pricing.md) of data points for your pricing tier.</span></span> 
* <span data-ttu-id="90d20-219">Az Alkalmazáshasználati adatok gyűjtése hálózati forgalom csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90d20-219">To reduce network traffic from the collection of telemetry.</span></span> 

### <a name="which-type-of-sampling-should-i-use"></a><span data-ttu-id="90d20-220">Milyen típusú mintavételi érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="90d20-220">Which type of sampling should I use?</span></span>
<span data-ttu-id="90d20-221">**Használja az adatfeldolgozást mintavételi, ha:**</span><span class="sxs-lookup"><span data-stu-id="90d20-221">**Use ingestion sampling if:**</span></span>

* <span data-ttu-id="90d20-222">Gyakran halad át a havi kvóta telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="90d20-222">You often go through your monthly quota of telemetry.</span></span>
* <span data-ttu-id="90d20-223">Azt a verzióját használja az SDK-t nem támogató mintavételi – például, a Java SDK vagy az ASP.NET-verziók 2-nél korábbi.</span><span class="sxs-lookup"><span data-stu-id="90d20-223">You're using a version of the SDK that doesn't support sampling - for example, the Java SDK or ASP.NET versions earlier than 2.</span></span>
* <span data-ttu-id="90d20-224">A felhasználók webböngészővel kap nagy mennyiségű telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="90d20-224">You're getting a lot of telemetry from your users' web browsers.</span></span>

<span data-ttu-id="90d20-225">**Használjon rögzített mintavételi, ha:**</span><span class="sxs-lookup"><span data-stu-id="90d20-225">**Use fixed-rate sampling if:**</span></span>

* <span data-ttu-id="90d20-226">Az Application Insights SDK használ az ASP.NET web services, verziószám: 2.0.0 vagy újabb verzióját, és</span><span class="sxs-lookup"><span data-stu-id="90d20-226">You're using the Application Insights SDK for ASP.NET web services version 2.0.0 or later, and</span></span>
* <span data-ttu-id="90d20-227">Ügyfél és kiszolgáló közötti szinkronizált mintavételi kívánt, az, hogy ha éppen vizsgálja a események [keresési](app-insights-diagnostic-search.md), az ügyfél és kiszolgáló, például a lapmegtekintések és a http-kérelmek kapcsolódó események közti léphet.</span><span class="sxs-lookup"><span data-stu-id="90d20-227">You want synchronized sampling between client and server, so that, when you're investigating events in [Search](app-insights-diagnostic-search.md), you can navigate between related events on the client and server, such as page views and http requests.</span></span>
* <span data-ttu-id="90d20-228">Biztos abban, a megfelelő mintavételi arány az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="90d20-228">You are confident of the appropriate sampling percentage for your app.</span></span> <span data-ttu-id="90d20-229">Elég nagy az beszerzése pontos mérőszámokat kell lennie, de alatt, amely meghaladja a árképzési kvóta és a sávszélesség-szabályozási korlátok.</span><span class="sxs-lookup"><span data-stu-id="90d20-229">It should be high enough to get accurate metrics, but below the rate that exceeds your pricing quota and the throttling limits.</span></span> 

<span data-ttu-id="90d20-230">**Adaptív mintavételi használja:**</span><span class="sxs-lookup"><span data-stu-id="90d20-230">**Use adaptive sampling:**</span></span>

<span data-ttu-id="90d20-231">Ellenkező esetben ajánlott adaptív mintavételi.</span><span class="sxs-lookup"><span data-stu-id="90d20-231">Otherwise, we recommend adaptive sampling.</span></span> <span data-ttu-id="90d20-232">Ez az ASP.NET Server SDK verzió 2.0.0-beta3 alapértelmezés szerint engedélyezve van, vagy később.</span><span class="sxs-lookup"><span data-stu-id="90d20-232">This is enabled by default in the ASP.NET server SDK, version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="90d20-233">Azt nem csökkenthető a forgalom csak egy bizonyos minimális sebessége, így nem lesz hatással a alacsony használható hely.</span><span class="sxs-lookup"><span data-stu-id="90d20-233">It doesn't reduce traffic until a certain minimum rate, so it won't affect a low-use site.</span></span>

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a><span data-ttu-id="90d20-234">Hogyan állapítható meg, hogy a mintavétel van-e művelet?</span><span class="sxs-lookup"><span data-stu-id="90d20-234">How do I know whether sampling is in operation?</span></span>
<span data-ttu-id="90d20-235">Annak megállapításához, a tényleges mintavételi ráta, függetlenül attól, hol van érvényben, használjon egy [Analytics lekérdezési](app-insights-analytics.md) Ez például:</span><span class="sxs-lookup"><span data-stu-id="90d20-235">To discover the actual sampling rate no matter where it has been applied, use an [Analytics query](app-insights-analytics.md) such as this:</span></span>

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

<span data-ttu-id="90d20-236">Az egyes megőrzi a rekord, `itemCount` azt jelzi, amely azt jelöli, eredeti rekordok száma egyenlő 1 + az előző elvetett rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="90d20-236">In each retained record, `itemCount` indicates the number of original records that it represents, equal to 1 + the number of previous discarded records.</span></span> 

## <a name="how-does-sampling-work"></a><span data-ttu-id="90d20-237">Hogyan működik a mintavételi?</span><span class="sxs-lookup"><span data-stu-id="90d20-237">How does sampling work?</span></span>
<span data-ttu-id="90d20-238">Rögzített és adaptív mintavételi egyik újdonsága az SDK 2.0.0 és újabb verziók esetében az ASP.NET-verziókban.</span><span class="sxs-lookup"><span data-stu-id="90d20-238">Fixed-rate and adaptive sampling are a feature of the SDK in ASP.NET versions from 2.0.0 onwards.</span></span> <span data-ttu-id="90d20-239">Mintavételi adatfeldolgozást az Application Insights-szolgáltatásának egy funkciója, és lehet a műveletet, ha az SDK nem hajt végre mintavétel.</span><span class="sxs-lookup"><span data-stu-id="90d20-239">Ingestion sampling is a feature of the Application Insights service, and can be in operation if the SDK is not performing sampling.</span></span> 

<span data-ttu-id="90d20-240">A mintavételi algoritmus úgy dönt, mely telemetriai elemek eldobása, és melyeket kívánja megtartani (az SDK-ban vagy akár az Application Insights szolgáltatásban).</span><span class="sxs-lookup"><span data-stu-id="90d20-240">The sampling algorithm decides which telemetry items to drop, and which ones to keep (whether it's in the SDK or in the Application Insights service).</span></span> <span data-ttu-id="90d20-241">A mintavételi döntési különböző szabályok, célja, hogy minden egymáshoz adatpontok változatlanok maradnak, egy diagnosztikai megoldást vezet be, hogy végrehajthatóként és megbízhatóbb, még akkor is csökkentett adatkészlet az Application insightsban karbantartása megőrzése alapul.</span><span class="sxs-lookup"><span data-stu-id="90d20-241">The sampling decision is based on several rules that aim to preserve all interrelated data points intact, maintaining a diagnostic experience in Application Insights that is actionable and reliable even with a reduced data set.</span></span> <span data-ttu-id="90d20-242">Például ha a sikertelen kérelmek az alkalmazás további telemetriai elemeket (például kivétel és naplózza a kérést a nyomkövetések) küld, mintavételi nem elválasztja az ehhez a kérelemhez és egyéb telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="90d20-242">For example, if for a failed request your app sends additional telemetry items (such as exception and traces logged from this request), sampling will not split this request and other telemetry.</span></span> <span data-ttu-id="90d20-243">Az tartja vagy együtt elutasítja azokat.</span><span class="sxs-lookup"><span data-stu-id="90d20-243">It either keeps or drops them all together.</span></span> <span data-ttu-id="90d20-244">Emiatt ha a kérelem részletes adatait az Application Insightsban tekinti meg, bármikor megtekintheti a kérelem és a kapcsolódó telemetriai elemek.</span><span class="sxs-lookup"><span data-stu-id="90d20-244">As a result, when you look at the request details in Application Insights, you can always see the request along with its associated telemetry items.</span></span> 

<span data-ttu-id="90d20-245">Az alkalmazások, amelyek meghatározzák a "user" (Ez azt jelenti, hogy a legjellemzőbb webalkalmazások), a mintavételi döntést a felhasználói azonosítóját, ami azt jelenti, hogy bármely adott felhasználó összes telemetriai adat vagy megőrzi, vagy kihagyott kivonatának alapul.</span><span class="sxs-lookup"><span data-stu-id="90d20-245">For applications that define "user" (that is, most typical web applications), the sampling decision is based on the hash of the user id, which means that all telemetry for any particular user is either preserved or dropped.</span></span> <span data-ttu-id="90d20-246">Milyen típusú alkalmazások felhasználók számára (például a web services) nem meghatározó a mintavételi döntési alapul a Műveletazonosító a kérelem.</span><span class="sxs-lookup"><span data-stu-id="90d20-246">For the types of applications that don't define users (such as web services) the sampling decision is based on the operation id of the request.</span></span> <span data-ttu-id="90d20-247">Végezetül cikkeknél a telemetriai adatok sem be (például telemetriai elemek nem http-környezetben az aszinkron szál jelentett) és a felhasználó nem művelet azonosítója mintavételi egyszerűen rögzíti a rendelkezésre álló telemetriai elemek százalékaként.</span><span class="sxs-lookup"><span data-stu-id="90d20-247">Finally, for the telemetry items that neither have user nor operation id set (for example telemetry items reported from asynchronous threads with no http context) sampling simply captures a percentage of telemetry items of each type.</span></span> 

<span data-ttu-id="90d20-248">Telemetria bemutató vissza, ha az Application Insights szolgáltatás a metrikák a azonos mintavételi arány kiegyensúlyozása érdekében a hiányzó adatpontokat gyűjtemény, idején használt állítja be.</span><span class="sxs-lookup"><span data-stu-id="90d20-248">When presenting telemetry back to you, the Application Insights service adjusts the metrics by the same sampling percentage that was used at the time of collection, to compensate for the missing data points.</span></span> <span data-ttu-id="90d20-249">Ezért ha az Application Insights telemetria, a felhasználók azért jelent meg, statisztikailag megfelelő becsléseket, amelyek nagyon közel valós számok.</span><span class="sxs-lookup"><span data-stu-id="90d20-249">Hence, when looking at the telemetry in Application Insights, the users are seeing statistically correct approximations that are very close to the real numbers.</span></span>

<span data-ttu-id="90d20-250">A közelítés a pontosság nagymértékben függ a konfigurált mintavételi arány.</span><span class="sxs-lookup"><span data-stu-id="90d20-250">The accuracy of the approximation largely depends on the configured sampling percentage.</span></span> <span data-ttu-id="90d20-251">A pontosság is, az alkalmazások, amelyek kezelik a felhasználók sok általában hasonló kérelmek nagy mennyiségű növeli.</span><span class="sxs-lookup"><span data-stu-id="90d20-251">Also, the accuracy increases for applications that handle a large volume of generally similar requests from lots of users.</span></span> <span data-ttu-id="90d20-252">Másrészről olyan alkalmazások, amelyek nem működnek a jelentős terhelés esetén mintavételi nem szükséges, mivel ezek az alkalmazások általában küldhet a telemetriai adatok maradva a kvótán belül anélkül, hogy ez adatvesztést a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="90d20-252">On the other hand, for applications that don't work with a significant load, sampling is not needed as these applications can usually send all their telemetry while staying within the quota, without causing data loss from throttling.</span></span> 

<span data-ttu-id="90d20-253">Vegye figyelembe, hogy az Application Insights nem mintavételre metrikák és a munkamenetek telemetriai típusok óta ezek a típusok, a pontosság csökkenésével magas nemkívánatos lehet.</span><span class="sxs-lookup"><span data-stu-id="90d20-253">Note that Application Insights does not sample Metrics and Sessions telemetry types, since for these types, reduction in the precision can be highly undesirable.</span></span> 

### <a name="adaptive-sampling"></a><span data-ttu-id="90d20-254">Adaptív mintavétel</span><span class="sxs-lookup"><span data-stu-id="90d20-254">Adaptive sampling</span></span>
<span data-ttu-id="90d20-255">Adaptív mintavételi ad hozzá egy összetevő, amely figyeli a jelenlegi átviteli az SDK-ból, és beállítja a mintavételi arány ahhoz, hogy a cél maximális sebesség belül maradjanak kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="90d20-255">Adaptive sampling adds a component that monitors the current rate of transmission from the SDK, and adjusts the sampling percentage to try to stay within the target maximum rate.</span></span> <span data-ttu-id="90d20-256">A módosítás rendszeres időközönként újraszámítja, és a kimenő átviteli sebességet mozgóátlaga alapul.</span><span class="sxs-lookup"><span data-stu-id="90d20-256">The adjustment is recalculated at regular intervals, and is based on a moving average of the outgoing transmission rate.</span></span>

## <a name="sampling-and-the-javascript-sdk"></a><span data-ttu-id="90d20-257">Mintavételi és a JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="90d20-257">Sampling and the JavaScript SDK</span></span>
<span data-ttu-id="90d20-258">Az ügyféloldali (JavaScript) SDK vesz részt a rögzített mintavételi a kiszolgálóoldali SDK-val együtt.</span><span class="sxs-lookup"><span data-stu-id="90d20-258">The client-side (JavaScript) SDK participates in fixed-rate sampling in conjunction with the server-side SDK.</span></span> <span data-ttu-id="90d20-259">A felműszerezett lapok csak küldése ügyféloldali telemetriai, amelynek a kiszolgálóoldali "mintát." a döntésről végrehajtott azonos felhasználói</span><span class="sxs-lookup"><span data-stu-id="90d20-259">The instrumented pages will only send client-side telemetry from the same users for which the server-side made its decision to "sample in."</span></span> <span data-ttu-id="90d20-260">A logikai egységének fenntartására szolgáló felhasználói munkamenet között - és kiszolgáló-ügyféloldalon tervezték.</span><span class="sxs-lookup"><span data-stu-id="90d20-260">This logic is designed to maintain integrity of user session across client- and server-sides.</span></span> <span data-ttu-id="90d20-261">Ennek eredményeképpen bármely Application Insights telemetria adott eleménél minden egyéb telemetriai elemek találhatók a felhasználó vagy a munkamenet.</span><span class="sxs-lookup"><span data-stu-id="90d20-261">As a result, from any particular telemetry item in Application Insights you can find all other telemetry items for this user or session.</span></span> 

<span data-ttu-id="90d20-262">*Az ügyfél és a kiszolgálóoldali telemetriai ne jelenjen meg koordinált minták fent leírtak.*</span><span class="sxs-lookup"><span data-stu-id="90d20-262">*My client and server-side telemetry don't show coordinated samples as you describe above.*</span></span>

* <span data-ttu-id="90d20-263">Győződjön meg arról, hogy a rögzített mintavételi mind a kiszolgáló és az ügyfél engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="90d20-263">Verify that you enabled fixed-rate sampling both on server and client.</span></span>
* <span data-ttu-id="90d20-264">Győződjön meg arról, hogy az SDK-verzió-e a 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="90d20-264">Make sure that the SDK version is 2.0 or above.</span></span>
* <span data-ttu-id="90d20-265">Ellenőrizze, hogy az azonos mintavételi arány beállíthatja az ügyfél és kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="90d20-265">Check that you set the same sampling percentage in both the client and server.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="90d20-266">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="90d20-266">Frequently Asked Questions</span></span>
<span data-ttu-id="90d20-267">*Miért nem mintavételi egy egyszerű "collect X valamennyi telemetriai típusú százaléka"?*</span><span class="sxs-lookup"><span data-stu-id="90d20-267">*Why isn't sampling a simple "collect X percent of each telemetry type"?*</span></span>

* <span data-ttu-id="90d20-268">A mintavételi megközelítést metrika becsléseket a nagyon nagy pontosságú volna meg, amíg megszűnését összehangolhatók a felhasználó, a munkamenet és a kérést, ami kritikus diagnosztikai diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="90d20-268">While this sampling approach would provide with a very high precision in metric approximations, it would break ability to correlate diagnostic data per user, session, and request, which is critical for diagnostics.</span></span> <span data-ttu-id="90d20-269">Ezért mintavételi works jobban az "összes telemetriai elemek összegyűjtése az X százalék az alkalmazás felhasználók" vagy "app kérelmek százalékos X az összes telemetriai adat gyűjtése" logikát.</span><span class="sxs-lookup"><span data-stu-id="90d20-269">Therefore, sampling works better with "collect all telemetry items for X percent of app users", or "collect all telemetry for X percent of app requests" logic.</span></span> <span data-ttu-id="90d20-270">A telemetriai elemek nem a kérések (például a háttérben történő aszinkron feldolgozás) társított, ismét alá esik, hogy "a gyűjtés X összes elemet az egyes telemetriai százalékát."</span><span class="sxs-lookup"><span data-stu-id="90d20-270">For the telemetry items not associated with the requests (such as background asynchronous processing), the fall back is to "collect X percent of all items for each telemetry type."</span></span> 

<span data-ttu-id="90d20-271">*A mintavételi arány idővel megváltozhat?*</span><span class="sxs-lookup"><span data-stu-id="90d20-271">*Can the sampling percentage change over time?*</span></span>

* <span data-ttu-id="90d20-272">Igen, adaptív mintavételi fokozatosan módosítja a mintavételi arány, telemetriai adatok jelenleg megfigyelt mennyisége alapján.</span><span class="sxs-lookup"><span data-stu-id="90d20-272">Yes, adaptive sampling gradually changes the sampling percentage, based on the currently observed volume of the telemetry.</span></span>

<span data-ttu-id="90d20-273">*Ha rögzített mintavételi használatához honnan tudhatom, hogy mely mintavételi százalékos fog működni a legjobb az alkalmazásom?*</span><span class="sxs-lookup"><span data-stu-id="90d20-273">*If I use fixed-rate sampling, how do I know which sampling percentage will work the best for my app?*</span></span>

* <span data-ttu-id="90d20-274">Egyirányú kezdődnie adaptív mintavételi, megtudhatja, mi értékelés rendezi a (lásd a fenti kérdés), és rögzített majd átváltása mintát vesz adott alapján.</span><span class="sxs-lookup"><span data-stu-id="90d20-274">One way is to start with adaptive sampling, find out what rate it settles on (see the above question), and then switch to fixed-rate sampling using that rate.</span></span> 
  
    <span data-ttu-id="90d20-275">Ellenkező esetben van kitalálni.</span><span class="sxs-lookup"><span data-stu-id="90d20-275">Otherwise, you have to guess.</span></span> <span data-ttu-id="90d20-276">Az aktuális telemetriai használatának a AI elemzése, megfigyelheti, hogy a sávszélesség-szabályozás, hogy folyamatban van és becslése gyűjtött telemetriai adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="90d20-276">Analyze your current telemetry usage in AI, observe any throttling that is occurring, and estimate the volume of the collected telemetry.</span></span> <span data-ttu-id="90d20-277">Ezen három bemeneti adatokat, és a választott tarifacsomaghoz javasoljuk, hogy mennyire érdemes a gyűjtött telemetriai adatok mennyiségének csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90d20-277">These three inputs, together with your selected pricing tier, suggest how much you might want to reduce the volume of the collected telemetry.</span></span> <span data-ttu-id="90d20-278">Azonban a felhasználók számának növelését vagy valamilyen más shift telemetriai adatok mennyiségének esetleg érvénytelenné válnak a becsült.</span><span class="sxs-lookup"><span data-stu-id="90d20-278">However, an increase in the number of your users or some other shift in the volume of telemetry might invalidate your estimate.</span></span>

<span data-ttu-id="90d20-279">*Mi történik, ha a mintavételi arány túl alacsony konfigurálni?*</span><span class="sxs-lookup"><span data-stu-id="90d20-279">*What happens if I configure sampling percentage too low?*</span></span>

* <span data-ttu-id="90d20-280">A túl alacsony mintavételi arány (over-aggressive mintavételi) csökkenti a becsléseket pontosságának, amikor az Application Insights megkísérli az adatok az adatok kötet csökkentése érdekében a képi megjelenítés kártalanítja.</span><span class="sxs-lookup"><span data-stu-id="90d20-280">Excessively low sampling percentage (over-aggressive sampling) reduces the accuracy of the approximations, when Application Insights attempts to compensate the visualization of the data for the data volume reduction.</span></span> <span data-ttu-id="90d20-281">Emellett diagnosztikai élmény előfordulhat, hogy csökkenéséhez vezet, a ritkán meghibásodott vagy lassú kérelmek egy részénél lehet mintát venni ki.</span><span class="sxs-lookup"><span data-stu-id="90d20-281">Also, diagnostic experience might be negatively impacted, as some of the infrequently failing or slow requests may be sampled out.</span></span>

<span data-ttu-id="90d20-282">*Mi történik, ha a mintavételi arány túl magas konfigurálni?*</span><span class="sxs-lookup"><span data-stu-id="90d20-282">*What happens if I configure sampling percentage too high?*</span></span>

* <span data-ttu-id="90d20-283">A gyűjtött telemetriai adatok mennyiségének elegendő csökkenést konfigurálása (nem agresszív elég) túl magas mintavételi arány eredménye.</span><span class="sxs-lookup"><span data-stu-id="90d20-283">Configuring too high sampling percentage (not aggressive enough) results in an insufficient reduction in the volume of the collected telemetry.</span></span> <span data-ttu-id="90d20-284">Továbbra is problémákat tapasztalhat a sávszélesség-szabályozás kapcsolatos telemetriai adatok elvesztését, és lehet, hogy az Application Insights költségeinek magasabb, mint amennyi tervezett keretét költségek miatt.</span><span class="sxs-lookup"><span data-stu-id="90d20-284">You may still experience telemetry data loss related to throttling, and the cost of using Application Insights might be higher than you planned due to overage charges.</span></span>

<span data-ttu-id="90d20-285">*Milyen platformokon használható mintavételi?*</span><span class="sxs-lookup"><span data-stu-id="90d20-285">*On what platforms can I use sampling?*</span></span>

* <span data-ttu-id="90d20-286">Adatfeldolgozást mintavételi automatikusan történik az összes telemetriai fent egy bizonyos köteten, ha az SDK nem hajt végre mintavétel.</span><span class="sxs-lookup"><span data-stu-id="90d20-286">Ingestion sampling can occur automatically for any telemetry above a certain volume, if the SDK is not performing sampling.</span></span> <span data-ttu-id="90d20-287">Ez akkor működik, például ha az alkalmazás egy Java-kiszolgálót használ, vagy ha az ASP.NET SDK egy régebbi verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="90d20-287">This would work, for example, if your app uses a Java server, or if you are using an older version of the ASP.NET SDK.</span></span>
* <span data-ttu-id="90d20-288">ASP.NET SDK verziók 2.0.0 használata vagy újabb (az Azure-ban vagy a saját kiszolgáló üzemelteti), adaptív alapértelmezés szerint a mintavételi kap, de lehet váltani rögzített fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="90d20-288">If you're using ASP.NET SDK versions 2.0.0 and above (hosted either in Azure or on your own server), you get adaptive sampling by default, but you can switch to fixed-rate as described above.</span></span> <span data-ttu-id="90d20-289">A rögzített mintavételi, a böngésző SDK automatikusan szinkronizálja a kapcsolódó események mintavételhez.</span><span class="sxs-lookup"><span data-stu-id="90d20-289">With fixed-rate sampling, the browser SDK automatically synchronizes to sample related events.</span></span> 

<span data-ttu-id="90d20-290">*Nincsenek az egyes ritka események, hogy mindig szeretné. Hogyan szerezhető be őket a mintavételi modul túli?*</span><span class="sxs-lookup"><span data-stu-id="90d20-290">*There are certain rare events I always want to see. How can I get them past the sampling module?*</span></span>

* <span data-ttu-id="90d20-291">Egy új TelemetryConfiguration (nem az alapértelmezett Active) rendelkező TelemetryClient külön példányt inicializálni.</span><span class="sxs-lookup"><span data-stu-id="90d20-291">Initialize a separate instance of TelemetryClient with a new TelemetryConfiguration (not the default Active one).</span></span> <span data-ttu-id="90d20-292">Használja a ritka események küldésére.</span><span class="sxs-lookup"><span data-stu-id="90d20-292">Use that to send your rare events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90d20-293">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90d20-293">Next steps</span></span>
* <span data-ttu-id="90d20-294">[Szűrés](app-insights-api-filtering-sampling.md) biztosíthat további szigorú ellenőrzése az SDK küld.</span><span class="sxs-lookup"><span data-stu-id="90d20-294">[Filtering](app-insights-api-filtering-sampling.md) can provide more strict control of what your SDK sends.</span></span>

