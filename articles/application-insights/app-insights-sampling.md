---
title: "az Azure Application Insights aaaTelemetry mintavételi |} Microsoft Docs"
description: "Hogyan tookeep hello telemetriai adatot vezérlése alatt."
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
ms.openlocfilehash: e19c350d0a5f16736c100322a9922fcfbf5d7b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sampling-in-application-insights"></a><span data-ttu-id="25618-103">Application Insights-mintavétel</span><span class="sxs-lookup"><span data-stu-id="25618-103">Sampling in Application Insights</span></span>


<span data-ttu-id="25618-104">A mintavétel az szolgáltatása [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="25618-104">Sampling is a feature in [Azure Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="25618-105">Hello ajánlott módja tooreduce telemetriai forgalom és a tárolás, az alkalmazásadatok statisztikailag megfelelő elemzési adatainak megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="25618-105">It is hello recommended way tooreduce telemetry traffic and storage, while preserving  a statistically correct analysis of application data.</span></span> <span data-ttu-id="25618-106">hello szűrő elemek kapcsolódó, választja ki, így navigálhat diagnosztikai vizsgálatok során elemek között.</span><span class="sxs-lookup"><span data-stu-id="25618-106">hello filter selects items that are related, so that you can navigate between items when you are doing diagnostic investigations.</span></span>
<span data-ttu-id="25618-107">Metrika számok hello portálon tooyou mutatnak be, amikor azok renormalized mintavételi, bármilyen hatása hello statisztika toominimize hello tootake figyelembe.</span><span class="sxs-lookup"><span data-stu-id="25618-107">When metric counts are presented tooyou in hello portal, they are renormalized tootake account of hello sampling, toominimize any effect on hello statistics.</span></span>

<span data-ttu-id="25618-108">Mintavételi csökkenti a forgalom és az adatok költségeket, és segít elkerülni a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="25618-108">Sampling reduces traffic and data costs, and helps you avoid throttling.</span></span>

## <a name="in-brief"></a><span data-ttu-id="25618-109">Röviden:</span><span class="sxs-lookup"><span data-stu-id="25618-109">In brief:</span></span>
* <span data-ttu-id="25618-110">Mintavételi őrzi meg az 1  *n*  hello rest figyelmen kívül hagyása rögzíti.</span><span class="sxs-lookup"><span data-stu-id="25618-110">Sampling retains 1 in *n* records and discards hello rest.</span></span> <span data-ttu-id="25618-111">Például akkor lehet, hogy továbbra is 1: 5 események, a mintavételi ráta 20 %-os.</span><span class="sxs-lookup"><span data-stu-id="25618-111">For example, it might retain 1 in 5 events, a sampling rate of 20%.</span></span> 
* <span data-ttu-id="25618-112">Mintavételi automatikusan történik, ha az alkalmazás nagy mennyiségű telemetriai adatokat küld az ASP.NET web server apps.</span><span class="sxs-lookup"><span data-stu-id="25618-112">Sampling happens automatically if your application sends a lot of telemetry, in ASP.NET web server apps.</span></span>
* <span data-ttu-id="25618-113">Azt is beállíthatja, hogy manuálisan, vagy a hello mintavételi portált hello árképzést ismertető oldalra; vagy hello ASP.NET SDK hello .config kiterjesztésű fájl, a tooalso hello hálózati forgalom csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="25618-113">You can also set sampling manually, either in hello portal on hello pricing page; or in hello ASP.NET SDK in hello .config file, tooalso reduce hello network traffic.</span></span>
* <span data-ttu-id="25618-114">Ha szeretné, hogy, hogy egy, akár őrzi meg, illetve elvetett együtt toomake egyéni események naplózása, győződjön meg arról, hogy azok rendelkeznek hello OperationID azonosítójú ugyanazt az értéket.</span><span class="sxs-lookup"><span data-stu-id="25618-114">If you log custom events and you want toomake sure that a set of events is either retained or discarded together, make sure that they have hello same OperationId value.</span></span>
* <span data-ttu-id="25618-115">hello mintavételi osztó  *n*  hello tulajdonságban rekordokban levő jelentett `itemCount`, ami keresési hello rövid neve "kérelem száma" vagy "események száma" alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="25618-115">hello sampling divisor *n* is reported in each record in hello property `itemCount`, which in Search appears under hello friendly name "request count" or "event count".</span></span> <span data-ttu-id="25618-116">Ha a mintavételi nincs a művelet, `itemCount==1`.</span><span class="sxs-lookup"><span data-stu-id="25618-116">When sampling is not in operation, `itemCount==1`.</span></span>
* <span data-ttu-id="25618-117">Írás Analytics lekérdezések, akkor [mintavételi figyelembe](app-insights-analytics-tour.md#counting-sampled-data).</span><span class="sxs-lookup"><span data-stu-id="25618-117">If you write Analytics queries, you should [take account of sampling](app-insights-analytics-tour.md#counting-sampled-data).</span></span> <span data-ttu-id="25618-118">Különösen helyett egyszerűen számbavételi rekordok, használjon `summarize sum(itemCount)`.</span><span class="sxs-lookup"><span data-stu-id="25618-118">In particular, instead of simply counting records, you should use `summarize sum(itemCount)`.</span></span>

## <a name="types-of-sampling"></a><span data-ttu-id="25618-119">Mintavételi típusai</span><span class="sxs-lookup"><span data-stu-id="25618-119">Types of sampling</span></span>
<span data-ttu-id="25618-120">Alternatív mintavételi három módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="25618-120">There are three alternative sampling methods:</span></span>

* <span data-ttu-id="25618-121">**Adaptív mintavételi** automatikusan beállítja az ASP.NET-alkalmazás az SDK hello küldött telemetriai hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="25618-121">**Adaptive sampling** automatically adjusts hello volume of telemetry sent from hello SDK in your ASP.NET app.</span></span> <span data-ttu-id="25618-122">Alapértelmezés szerint az SDK v 2.0.0-beta3.</span><span class="sxs-lookup"><span data-stu-id="25618-122">Default from SDK v 2.0.0-beta3.</span></span> <span data-ttu-id="25618-123">Jelenleg csak az ASP.NET kiszolgálóoldali telemetriai adat esetén érhető el.</span><span class="sxs-lookup"><span data-stu-id="25618-123">Currently available for ASP.NET server-side telemetry only.</span></span> 
* <span data-ttu-id="25618-124">**Rögzített mintavételi** mindkét az ASP.NET kiszolgálóról, és a felhasználók böngészőjének továbbított telemetriai hello mennyiségét csökkenti.</span><span class="sxs-lookup"><span data-stu-id="25618-124">**Fixed-rate sampling** reduces hello volume of telemetry sent from both your ASP.NET server and from your users' browsers.</span></span> <span data-ttu-id="25618-125">Hello sebességét beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="25618-125">You set hello rate.</span></span> <span data-ttu-id="25618-126">hello ügyfél- és a mintavételi fogja szinkronizálni, a keresés, navigálhat kapcsolódó Lapmegtekintések és kérések között.</span><span class="sxs-lookup"><span data-stu-id="25618-126">hello client and server will synchronize their sampling so that, in Search, you can navigate between related page views and requests.</span></span>
* <span data-ttu-id="25618-127">**Adatfeldolgozást mintavételi** hello Azure-portálon is működik.</span><span class="sxs-lookup"><span data-stu-id="25618-127">**Ingestion sampling** works in hello Azure portal.</span></span> <span data-ttu-id="25618-128">Törli az alkalmazásból, Ön által beállított ütemben érkező hello telemetriai adatok némelyike.</span><span class="sxs-lookup"><span data-stu-id="25618-128">It discards some of hello telemetry that arrives from your app, at a rate that you set.</span></span> <span data-ttu-id="25618-129">Nem csökkenthető a telemetria-forgalom, de lehetővé teszi a havi kvótán belül.</span><span class="sxs-lookup"><span data-stu-id="25618-129">It doesn't reduce telemetry traffic, but helps you keep within your monthly quota.</span></span> <span data-ttu-id="25618-130">hello nagy adatfeldolgozást mintavételi előnye, hogy az alkalmazás üzembe helyezésével állíthatja be, és egységesen összes kiszolgálók és ügyfelek esetében működik.</span><span class="sxs-lookup"><span data-stu-id="25618-130">hello big advantage of ingestion sampling is that you can set it without redeploying your app, and it works uniformly for all servers and clients.</span></span> 

<span data-ttu-id="25618-131">Ha az adaptív vagy rögzített arány mintavételi művelet, adatfeldolgozást mintavételi le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="25618-131">If Adaptive or Fixed rate sampling are in operation, Ingestion sampling is disabled.</span></span>

## <a name="ingestion-sampling"></a><span data-ttu-id="25618-132">Adatfeldolgozást mintavétel</span><span class="sxs-lookup"><span data-stu-id="25618-132">Ingestion sampling</span></span>
<span data-ttu-id="25618-133">Az űrlap mintavételi hello pont, ahol a webkiszolgáló böngészők és eszközök hello telemetriai eléri hello Application Insights szolgáltatás végpontjának működik.</span><span class="sxs-lookup"><span data-stu-id="25618-133">This form of sampling operates at hello point where hello telemetry from your web server, browsers, and devices reaches hello Application Insights service endpoint.</span></span> <span data-ttu-id="25618-134">Bár ez nem csökkenthető hello telemetriai forgalom küldése az alkalmazásból, az csökkentheti hello feldolgozása és maradnak (majd díjat) az Application Insights által.</span><span class="sxs-lookup"><span data-stu-id="25618-134">Although it doesn't reduce hello telemetry traffic sent from your app, it does reduce hello amount processed and retained (and charged for) by Application Insights.</span></span>

<span data-ttu-id="25618-135">Az ilyen mintavételi típusú, ha az alkalmazás gyakran kerül felett a havi kvótát, és nem kell hello hello SDK-alapú típusú mintavételi bármelyikével lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="25618-135">Use this type of sampling if your app often goes over its monthly quota and you don't have hello option of using either of hello SDK-based types of sampling.</span></span> 

<span data-ttu-id="25618-136">Állítsa be a hello kvóták hello mintavételi gyakoriság és az árazás panel:</span><span class="sxs-lookup"><span data-stu-id="25618-136">Set hello sampling rate in hello Quotas and Pricing blade:</span></span>

![Hello alkalmazás áttekintése panelen kattintson a beállítások, a kvóta, a mintákat, majd válassza ki a mintavételi ráta, és kattintson a frissítés gombra.](./media/app-insights-sampling/04.png)

<span data-ttu-id="25618-138">Mintavételi más típusú, mint a hello algoritmus megtartja a kapcsolódó telemetriai adat elemek.</span><span class="sxs-lookup"><span data-stu-id="25618-138">Like other types of sampling, hello algorithm retains related telemetry items.</span></span> <span data-ttu-id="25618-139">Például hello telemetriai Keresés most vizsgálatakor ellenőrizze, képes lesz toofind hello kérelem kapcsolódó tooa adott kivétel.</span><span class="sxs-lookup"><span data-stu-id="25618-139">For example, when you're inspecting hello telemetry in Search, you'll be able toofind hello request related tooa particular exception.</span></span> <span data-ttu-id="25618-140">A metrika számít például kérelmek aránya, és kivétel arány megfelelően megőrződnek.</span><span class="sxs-lookup"><span data-stu-id="25618-140">Metric counts such as request rate and exception rate are correctly retained.</span></span>

<span data-ttu-id="25618-141">Hogy a rendszer elveti a mintavétellel már nem érhetők el bármilyen Application Insights szolgáltatás például [a folyamatos exportálás](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="25618-141">Data points that are discarded by sampling are not available in any Application Insights feature such as [Continuous Export](app-insights-export-telemetry.md).</span></span>

<span data-ttu-id="25618-142">Adatfeldolgozást mintavételi nem működik, amikor az SDK-alapú adaptív vagy rögzített mintavételi művelet van.</span><span class="sxs-lookup"><span data-stu-id="25618-142">Ingestion sampling doesn't operate while SDK-based adaptive or fixed-rate sampling is in operation.</span></span> <span data-ttu-id="25618-143">Ha hello mintavételi ráta hello SDK 100 %-nál kisebb, hello adatfeldolgozást mintavételi ráta beállított figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="25618-143">If hello sampling rate at hello SDK is less than 100%, then hello ingestion sampling rate that you set is ignored.</span></span>

> [!WARNING]
> <span data-ttu-id="25618-144">hello hello csempén megjelenő érték hello adatfeldolgozást mintavételi beállított értékeket.</span><span class="sxs-lookup"><span data-stu-id="25618-144">hello value shown on hello tile indicates hello value that you set for ingestion sampling.</span></span> <span data-ttu-id="25618-145">Hello tényleges mintavételi ráta az nem jelent, ha SDK mintavételi művelet.</span><span class="sxs-lookup"><span data-stu-id="25618-145">It doesn't represent hello actual sampling rate if SDK sampling is in operation.</span></span>
> 
> 

## <a name="adaptive-sampling-at-your-web-server"></a><span data-ttu-id="25618-146">A webkiszolgáló adaptív mintavétel</span><span class="sxs-lookup"><span data-stu-id="25618-146">Adaptive sampling at your web server</span></span>
<span data-ttu-id="25618-147">Adaptív mintavételi hello Application Insights SDK az ASP.NET v 2.0.0-beta3 és újabb verzióihoz érhető el, és alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="25618-147">Adaptive sampling is available for hello Application Insights SDK for ASP.NET v 2.0.0-beta3 and later, and is enabled by default.</span></span> 

<span data-ttu-id="25618-148">Adaptív mintavételi hatással van a web server alkalmazás toohello Application Insights szolgáltatás által küldött telemetriai hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="25618-148">Adaptive sampling affects hello volume of telemetry sent from your web server app toohello Application Insights service.</span></span> <span data-ttu-id="25618-149">hello kötet módosul automatikusan tookeep belül forgalom a megadott maximális értéket.</span><span class="sxs-lookup"><span data-stu-id="25618-149">hello volume is adjusted automatically tookeep within a specified maximum rate of traffic.</span></span>

<span data-ttu-id="25618-150">Azt nem működik, telemetriai adatokat kis mennyiségű úgy egy alkalmazást a hibakeresés, vagy nem befolyásolja az alacsony használat rendelkező webhely.</span><span class="sxs-lookup"><span data-stu-id="25618-150">It doesn't operate at low volumes of telemetry, so an app in debugging or a website with low usage won't be affected.</span></span>

<span data-ttu-id="25618-151">tooachieve hello célkötet, néhány hello telemetriai adatot generált a rendszer elveti.</span><span class="sxs-lookup"><span data-stu-id="25618-151">tooachieve hello target volume, some of hello telemetry generated is discarded.</span></span> <span data-ttu-id="25618-152">De mintavételi más típusú, például hello algoritmus kapcsolódó telemetriai adat elemek megőrzi.</span><span class="sxs-lookup"><span data-stu-id="25618-152">But like other types of sampling, hello algorithm retains related telemetry items.</span></span> <span data-ttu-id="25618-153">Például hello telemetriai Keresés most vizsgálatakor ellenőrizze, képes lesz toofind hello kérelem kapcsolódó tooa adott kivétel.</span><span class="sxs-lookup"><span data-stu-id="25618-153">For example, when you're inspecting hello telemetry in Search, you'll be able toofind hello request related tooa particular exception.</span></span> 

<span data-ttu-id="25618-154">A metrika álló, például a kérelmek gyakorisága és kivétel alapján a mintavételi ráta, hello módosított toocompensate, hogy azok körülbelül megfelelő értékek megjelenítése a metrika Intézőben.</span><span class="sxs-lookup"><span data-stu-id="25618-154">Metric counts such as request rate and exception rate are adjusted toocompensate for hello sampling rate, so that they show approximately correct values in Metric Explorer.</span></span>

<span data-ttu-id="25618-155">**Frissítse a projekt NuGet** toohello legújabb csomagok *kiadás előtti* Application Insights verziója: kattintson a jobb gombbal a hello projektben a Megoldáskezelőre, válassza a NuGet-csomagok kezelése, ellenőrizze **belefoglalása előzetes** és Microsoft.ApplicationInsights.Web megkereséséhez.</span><span class="sxs-lookup"><span data-stu-id="25618-155">**Update your project's NuGet** packages toohello latest *pre-release* version of Application Insights: Right-click hello project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 

<span data-ttu-id="25618-156">A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), módosíthatja a hello számos paraméter `AdaptiveSamplingTelemetryProcessor` csomópont.</span><span class="sxs-lookup"><span data-stu-id="25618-156">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), you can adjust several parameters in hello `AdaptiveSamplingTelemetryProcessor` node.</span></span> <span data-ttu-id="25618-157">hello ábrán is látható hello alapértelmezett értékei lesznek:</span><span class="sxs-lookup"><span data-stu-id="25618-157">hello figures shown are hello default values:</span></span>

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    <span data-ttu-id="25618-158">hello adaptív algoritmus hello cél sebességet célja a **minden kiszolgáló állomáson**.</span><span class="sxs-lookup"><span data-stu-id="25618-158">hello target rate that hello adaptive algorithm aims for **on each server host**.</span></span> <span data-ttu-id="25618-159">Ha sok gazdagép a webalkalmazás fut, csökkentse tooremain belül hello Application Insights portál-forgalmat a cél aránya, ezért ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="25618-159">If your web app runs on many hosts, reduce this value so as tooremain within your target rate of traffic at hello Application Insights portal.</span></span>
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    <span data-ttu-id="25618-160">hello időtartama, mely hello telemetriai jelenlegi sebessége újraértékelése is megtörténik.</span><span class="sxs-lookup"><span data-stu-id="25618-160">hello interval at which hello current rate of telemetry is re-evaluated.</span></span> <span data-ttu-id="25618-161">Kiértékelési mozgóátlaga szerint történik.</span><span class="sxs-lookup"><span data-stu-id="25618-161">Evaluation is performed as a moving average.</span></span> <span data-ttu-id="25618-162">Érdemes tooshorten Ez az időtartam alatt a telemetriai adatok azokért toosudden felszakadásáig esetén.</span><span class="sxs-lookup"><span data-stu-id="25618-162">You might want tooshorten this interval if your telemetry is liable toosudden bursts.</span></span>
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    <span data-ttu-id="25618-163">Ha mintavételi százalékos változásainak, hogy mikor után azt engedélyezett százalékos újra mintavételi toolower toocapture kevésbé adatok.</span><span class="sxs-lookup"><span data-stu-id="25618-163">When sampling percentage value changes, how soon after are we allowed toolower sampling percentage again toocapture less data.</span></span>
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    <span data-ttu-id="25618-164">Ha mintavételi százalékos változásainak, hogy mikor után azt engedélyezett százalékos újra mintavételi tooincrease toocapture több adatot.</span><span class="sxs-lookup"><span data-stu-id="25618-164">When sampling percentage value changes, how soon after are we allowed tooincrease sampling percentage again toocapture more data.</span></span>
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    <span data-ttu-id="25618-165">Mivel százalékos mintavételi változik, mi az hello minimális érték azt hogy jogosult tooset.</span><span class="sxs-lookup"><span data-stu-id="25618-165">As sampling percentage varies, what is hello minimum value we're allowed tooset.</span></span>
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    <span data-ttu-id="25618-166">Mivel százalékos mintavételi változik, mi az hello maximális érték azt hogy jogosult tooset.</span><span class="sxs-lookup"><span data-stu-id="25618-166">As sampling percentage varies, what is hello maximum value we're allowed tooset.</span></span>
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    <span data-ttu-id="25618-167">Hello mozgóátlaga hello kiszámítása hello súly toohello legutóbbi értéket kapja.</span><span class="sxs-lookup"><span data-stu-id="25618-167">In hello calculation of hello moving average, hello weight assigned toohello most recent value.</span></span> <span data-ttu-id="25618-168">1-nél kisebb érték egyenlő tooor használja.</span><span class="sxs-lookup"><span data-stu-id="25618-168">Use a value equal tooor less than 1.</span></span> <span data-ttu-id="25618-169">A kisebb értékek módosításokat hello algoritmus kevesebb reaktív toosudden.</span><span class="sxs-lookup"><span data-stu-id="25618-169">Smaller values make hello algorithm less reactive toosudden changes.</span></span>
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    <span data-ttu-id="25618-170">Amikor hello app elkezdte hello érték.</span><span class="sxs-lookup"><span data-stu-id="25618-170">hello value assigned when hello app has just started.</span></span> <span data-ttu-id="25618-171">Nem csökkenti a akkor hibakeresése közben.</span><span class="sxs-lookup"><span data-stu-id="25618-171">Don't reduce this while you're debugging.</span></span> 

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    <span data-ttu-id="25618-172">Pontosvesszővel elválasztott felsorolása, amelyek nem szeretné, hogy toobe mintát venni.</span><span class="sxs-lookup"><span data-stu-id="25618-172">A semi-colon delimited list of types that you do not want toobe sampled.</span></span> <span data-ttu-id="25618-173">Felismert típusok a következők: függőségi, esemény, kivétel, PageView, kérelem, nyomkövetési.</span><span class="sxs-lookup"><span data-stu-id="25618-173">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="25618-174">Hello összes példánya megadott típusok továbbításuk; Nincs megadva hello típusok lekérdező.</span><span class="sxs-lookup"><span data-stu-id="25618-174">All instances of hello specified types are transmitted; hello types that are not specified are sampled.</span></span>

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    <span data-ttu-id="25618-175">Egy pontosvesszővel tagolt listáját, amelyet a mintát toobe típusok.</span><span class="sxs-lookup"><span data-stu-id="25618-175">A semi-colon delimited list of types that you want toobe sampled.</span></span> <span data-ttu-id="25618-176">Felismert típusok a következők: függőségi, esemény, kivétel, PageView, kérelem, nyomkövetési.</span><span class="sxs-lookup"><span data-stu-id="25618-176">Recognized types are: Dependency, Event, Exception, PageView, Request, Trace.</span></span> <span data-ttu-id="25618-177">hello megadott típusok lekérdező; összes példányát a hello más mindig továbbítja.</span><span class="sxs-lookup"><span data-stu-id="25618-177">hello specified types are sampled; all instances of hello other types will always be transmitted.</span></span>


<span data-ttu-id="25618-178">**ki tooswitch** adaptív mintavételi, eltávolítás hello AdaptiveSamplingTelemetryProcessor csomópont a applicationinsights-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="25618-178">**tooswitch off** adaptive sampling, remove hello AdaptiveSamplingTelemetryProcessor node from applicationinsights-config.</span></span>

### <a name="alternative-configure-adaptive-sampling-in-code"></a><span data-ttu-id="25618-179">Alternatív: Adaptív mintavétel konfigurálása a kódban</span><span class="sxs-lookup"><span data-stu-id="25618-179">Alternative: configure adaptive sampling in code</span></span>
<span data-ttu-id="25618-180">Mintavételi hello .config kiterjesztésű fájl helyett, a kódot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="25618-180">Instead of adjusting sampling in hello .config file, you can use code.</span></span> <span data-ttu-id="25618-181">Ez lehetővé teszi a visszahívási függvény, amelyet mindig hello mintavételi ráta újraértékelése is megtörténik, amikor toospecify.</span><span class="sxs-lookup"><span data-stu-id="25618-181">This allows you toospecify a callback function that is invoked whenever hello sampling rate is re-evaluated.</span></span> <span data-ttu-id="25618-182">Ez használhat, például milyen mintavételi ráta kimenő toofind használatban van.</span><span class="sxs-lookup"><span data-stu-id="25618-182">You could use this, for example, toofind out what sampling rate is being used.</span></span>

<span data-ttu-id="25618-183">Távolítsa el a hello `AdaptiveSamplingTelemetryProcessor` csomópontot hello .config fájlból.</span><span class="sxs-lookup"><span data-stu-id="25618-183">Remove hello `AdaptiveSamplingTelemetryProcessor` node from hello .config file.</span></span>

<span data-ttu-id="25618-184">*C#*</span><span class="sxs-lookup"><span data-stu-id="25618-184">*C#*</span></span>

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust hello settings from their defaults.

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
             // Report hello sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="25618-185">([További információ a telemetriai adatok processzorok](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="25618-185">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

<a name="other-web-pages"></a>

## <a name="sampling-for-web-pages-with-javascript"></a><span data-ttu-id="25618-186">Mintavételi JavaScript weblapok</span><span class="sxs-lookup"><span data-stu-id="25618-186">Sampling for web pages with JavaScript</span></span>
<span data-ttu-id="25618-187">Konfigurálhatja a weblapok rögzített mintavételi tetszőleges kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="25618-187">You can configure web pages for fixed-rate sampling from any server.</span></span> 

<span data-ttu-id="25618-188">Ha Ön [hello weblapok konfigurálja az Application Insights](app-insights-javascript.md), hello részlet kapott hello Application Insights portál módosítása.</span><span class="sxs-lookup"><span data-stu-id="25618-188">When you [configure hello web pages for Application Insights](app-insights-javascript.md), modify hello snippet that you get from hello Application Insights portal.</span></span> <span data-ttu-id="25618-189">(Az ASP.NET alkalmazásokban hello részlet általában kerül a _Layout.cshtml.)  Például a sor beszúrása `samplingPercentage: 10,` hello instrumentation kulcs előtt:</span><span class="sxs-lookup"><span data-stu-id="25618-189">(In ASP.NET apps, hello snippet typically goes in _Layout.cshtml.)  Insert a line like `samplingPercentage: 10,` before hello instrumentation key:</span></span>

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

<span data-ttu-id="25618-190">Hello mintavételi arány válassza a Bezárás too100/N van, ahol N az egész érték.</span><span class="sxs-lookup"><span data-stu-id="25618-190">For hello sampling percentage, choose a percentage that is close too100/N where N is an integer.</span></span>  <span data-ttu-id="25618-191">Mintavételi jelenleg nem támogatja a más értékek.</span><span class="sxs-lookup"><span data-stu-id="25618-191">Currently sampling doesn't support other values.</span></span>

<span data-ttu-id="25618-192">Ha rögzített mintavételi hello kiszolgálón is engedélyeznie hello ügyfelek és a kiszolgáló szinkronizálja, az adott, a keresési navigálhat kapcsolódó Lapmegtekintések és kérések között.</span><span class="sxs-lookup"><span data-stu-id="25618-192">If you also enable fixed-rate sampling at hello server, hello clients and server will synchronize so that, in Search, you can  navigate between related page views and requests.</span></span>

## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a><span data-ttu-id="25618-193">Az ASP.NET-webhelyeket rögzített mintavétel</span><span class="sxs-lookup"><span data-stu-id="25618-193">Fixed-rate sampling for ASP.NET web sites</span></span>
<span data-ttu-id="25618-194">Rögzített alapján mintavételi csökkenti a webkiszolgáló és a böngészők által küldött hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="25618-194">Fixed rate sampling reduces hello traffic sent from your web server and web browsers.</span></span> <span data-ttu-id="25618-195">Adaptív mintavételi eltérően csökkenti telemetriai rögzített kulcs határoz meg.</span><span class="sxs-lookup"><span data-stu-id="25618-195">Unlike adaptive sampling, it reduces telemetry at a fixed rate decided by you.</span></span> <span data-ttu-id="25618-196">Azt is szinkronizálja hello ügyfél és kiszolgáló mintavételi, hogy a kapcsolódó elemek megmaradnak – például, hogy a nézet a keresési tekinti meg, ha a kapcsolódó kérelemre található.</span><span class="sxs-lookup"><span data-stu-id="25618-196">It also synchronizes hello client and server sampling so that related items are retained - for example, so that if you look at a page view in Search, you can find its related request.</span></span>

<span data-ttu-id="25618-197">hello mintavételi algoritmus megtartja a kapcsolódó elemeket.</span><span class="sxs-lookup"><span data-stu-id="25618-197">hello sampling algorithm retains related items.</span></span> <span data-ttu-id="25618-198">Az egyes HTTP-kérelmek esemény, ezért a kapcsolódó események vagy elvetett vagy továbbítani.</span><span class="sxs-lookup"><span data-stu-id="25618-198">For each HTTP request event, it and its related events are either discarded or transmitted.</span></span> 

<span data-ttu-id="25618-199">A Metrikaböngészőben díjszabás kérelem és a kivétel számát, például program megszorozza egy tényező toocompensate hello mintavételi ráta, az, hogy azok körülbelül helyességét.</span><span class="sxs-lookup"><span data-stu-id="25618-199">In Metrics Explorer, rates such as request and exception counts are multiplied by a factor toocompensate for hello sampling rate, so that they are approximately correct.</span></span>

1. <span data-ttu-id="25618-200">**A projekt NuGet-csomagok frissítése** toohello legújabb *kiadás előtti* Application Insights verzióját.</span><span class="sxs-lookup"><span data-stu-id="25618-200">**Update your project's NuGet packages** toohello latest *pre-release* version of Application Insights.</span></span> <span data-ttu-id="25618-201">Kattintson a jobb gombbal a hello projektben a Megoldáskezelőre, válassza a NuGet-csomagok kezelése, ellenőrizze **közé tartoznak az előzetes** és Microsoft.ApplicationInsights.Web megkereséséhez.</span><span class="sxs-lookup"><span data-stu-id="25618-201">Right-click hello project in Solution Explorer, choose Manage NuGet Packages, check **Include prerelease** and search for Microsoft.ApplicationInsights.Web.</span></span> 
2. <span data-ttu-id="25618-202">**Tiltsa le a adaptív mintavételi**: A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), távolítsa el vagy hello megjegyzéssé `AdaptiveSamplingTelemetryProcessor` csomópont.</span><span class="sxs-lookup"><span data-stu-id="25618-202">**Disable adaptive sampling**: In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), remove or comment out hello `AdaptiveSamplingTelemetryProcessor` node.</span></span>
   
    ```xml
   
    <TelemetryProcessors>
   
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->

    ```

1. <span data-ttu-id="25618-203">**Hello rögzített mintavételi modul engedélyezése.**</span><span class="sxs-lookup"><span data-stu-id="25618-203">**Enable hello fixed-rate sampling module.**</span></span> <span data-ttu-id="25618-204">Adja hozzá ezt a kódrészletet túl[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="25618-204">Add this snippet too[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```

> [!NOTE]
> <span data-ttu-id="25618-205">Hello mintavételi arány válassza a Bezárás too100/N van, ahol N az egész érték.</span><span class="sxs-lookup"><span data-stu-id="25618-205">For hello sampling percentage, choose a percentage that is close too100/N where N is an integer.</span></span>  <span data-ttu-id="25618-206">Mintavételi jelenleg nem támogatja a más értékek.</span><span class="sxs-lookup"><span data-stu-id="25618-206">Currently sampling doesn't support other values.</span></span>
> 
> 

### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a><span data-ttu-id="25618-207">Másik megoldás: a kiszolgáló kódjában található rögzített mintavételi engedélyezése</span><span class="sxs-lookup"><span data-stu-id="25618-207">Alternative: enable fixed-rate sampling in your server code</span></span>
<span data-ttu-id="25618-208">Hello mintavételi paraméter beállításhoz hello .config fájl helyett a kód is használhatja.</span><span class="sxs-lookup"><span data-stu-id="25618-208">Instead of setting hello sampling parameter in hello .config file, you can use code.</span></span> 

<span data-ttu-id="25618-209">*C#*</span><span class="sxs-lookup"><span data-stu-id="25618-209">*C#*</span></span>

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

<span data-ttu-id="25618-210">([További információ a telemetriai adatok processzorok](app-insights-api-filtering-sampling.md#filtering).)</span><span class="sxs-lookup"><span data-stu-id="25618-210">([Learn about telemetry processors](app-insights-api-filtering-sampling.md#filtering).)</span></span>

## <a name="when-toouse-sampling"></a><span data-ttu-id="25618-211">Amikor toouse mintavételi?</span><span class="sxs-lookup"><span data-stu-id="25618-211">When toouse sampling?</span></span>
<span data-ttu-id="25618-212">Adaptív mintavételi automatikusan engedélyezett, ha az ASP.NET SDK verzió 2.0.0-beta3 hello használata vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="25618-212">Adaptive sampling is automatically enabled if you use hello ASP.NET SDK version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="25618-213">Függetlenül attól, milyen SDK verzióját használja használhat adatfeldolgozást mintavételi (a kiszolgáló).</span><span class="sxs-lookup"><span data-stu-id="25618-213">No matter what SDK version you use, you can use ingestion sampling (at our server).</span></span>

<span data-ttu-id="25618-214">Mintavételi legtöbb kis és közepes méretű alkalmazásokhoz nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="25618-214">You don’t need sampling for most small and medium size applications.</span></span> <span data-ttu-id="25618-215">hello legfontosabb diagnosztikai adatokat és a legpontosabb statisztika nyerhetők adatokat gyűjt a felhasználói tevékenységekről.</span><span class="sxs-lookup"><span data-stu-id="25618-215">hello most useful diagnostic information and most accurate statistics are obtained by collecting data on all your user activities.</span></span> 

<span data-ttu-id="25618-216">mintavételi hello fő előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="25618-216">hello main advantages of sampling are:</span></span>

* <span data-ttu-id="25618-217">Application Insights szolgáltatás elhagyta a(z) ("szabályozások") adatpontok rövid üzenetet küld a nagyon nagy mértékben telemetriai adatot az alkalmazás időintervallum.</span><span class="sxs-lookup"><span data-stu-id="25618-217">Application Insights service drops ("throttles") data points when your app sends a very high rate of telemetry in short time interval.</span></span> 
* <span data-ttu-id="25618-218">hello belül tookeep [kvóta](app-insights-pricing.md) az adatokat a tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="25618-218">tookeep within hello [quota](app-insights-pricing.md) of data points for your pricing tier.</span></span> 
* <span data-ttu-id="25618-219">tooreduce hálózati forgalmat a telemetriai adatok hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="25618-219">tooreduce network traffic from hello collection of telemetry.</span></span> 

### <a name="which-type-of-sampling-should-i-use"></a><span data-ttu-id="25618-220">Milyen típusú mintavételi érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="25618-220">Which type of sampling should I use?</span></span>
<span data-ttu-id="25618-221">**Használja az adatfeldolgozást mintavételi, ha:**</span><span class="sxs-lookup"><span data-stu-id="25618-221">**Use ingestion sampling if:**</span></span>

* <span data-ttu-id="25618-222">Gyakran halad át a havi kvóta telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="25618-222">You often go through your monthly quota of telemetry.</span></span>
* <span data-ttu-id="25618-223">Akkor egy verzióját használja, amely nem támogatja például mintavételi - SDK hello, Java SDK hello vagy ASP.NET verzió 2-nél korábbi.</span><span class="sxs-lookup"><span data-stu-id="25618-223">You're using a version of hello SDK that doesn't support sampling - for example, hello Java SDK or ASP.NET versions earlier than 2.</span></span>
* <span data-ttu-id="25618-224">A felhasználók webböngészővel kap nagy mennyiségű telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="25618-224">You're getting a lot of telemetry from your users' web browsers.</span></span>

<span data-ttu-id="25618-225">**Használjon rögzített mintavételi, ha:**</span><span class="sxs-lookup"><span data-stu-id="25618-225">**Use fixed-rate sampling if:**</span></span>

* <span data-ttu-id="25618-226">Application Insights SDK hello használata esetén az ASP.NET web services, verziószám: 2.0.0 vagy újabb verzióját, és</span><span class="sxs-lookup"><span data-stu-id="25618-226">You're using hello Application Insights SDK for ASP.NET web services version 2.0.0 or later, and</span></span>
* <span data-ttu-id="25618-227">Ügyfél és kiszolgáló közötti szinkronizált mintavételi kívánt, az, hogy ha éppen vizsgálja a események [keresési](app-insights-diagnostic-search.md), kapcsolódó események hello ügyfélen és a kiszolgáló, például a lapmegtekintések és a http-kérelmek között válthat.</span><span class="sxs-lookup"><span data-stu-id="25618-227">You want synchronized sampling between client and server, so that, when you're investigating events in [Search](app-insights-diagnostic-search.md), you can navigate between related events on hello client and server, such as page views and http requests.</span></span>
* <span data-ttu-id="25618-228">Biztos abban az hello megfelelő mintavételi arány az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="25618-228">You are confident of hello appropriate sampling percentage for your app.</span></span> <span data-ttu-id="25618-229">Az legyen elég magas tooget pontos mérőszámokat, de az alábbiakban hello arány, amely meghaladja a árképzési kvótát, és szabályozás korlátok hello.</span><span class="sxs-lookup"><span data-stu-id="25618-229">It should be high enough tooget accurate metrics, but below hello rate that exceeds your pricing quota and hello throttling limits.</span></span> 

<span data-ttu-id="25618-230">**Adaptív mintavételi használja:**</span><span class="sxs-lookup"><span data-stu-id="25618-230">**Use adaptive sampling:**</span></span>

<span data-ttu-id="25618-231">Ellenkező esetben ajánlott adaptív mintavételi.</span><span class="sxs-lookup"><span data-stu-id="25618-231">Otherwise, we recommend adaptive sampling.</span></span> <span data-ttu-id="25618-232">Ez a hello ASP.NET server SDK verzió 2.0.0-beta3 alapértelmezés szerint engedélyezve van, vagy később.</span><span class="sxs-lookup"><span data-stu-id="25618-232">This is enabled by default in hello ASP.NET server SDK, version 2.0.0-beta3 or later.</span></span> <span data-ttu-id="25618-233">Azt nem csökkenthető a forgalom csak egy bizonyos minimális sebessége, így nem lesz hatással a alacsony használható hely.</span><span class="sxs-lookup"><span data-stu-id="25618-233">It doesn't reduce traffic until a certain minimum rate, so it won't affect a low-use site.</span></span>

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a><span data-ttu-id="25618-234">Hogyan állapítható meg, hogy a mintavétel van-e művelet?</span><span class="sxs-lookup"><span data-stu-id="25618-234">How do I know whether sampling is in operation?</span></span>
<span data-ttu-id="25618-235">a mintavételi arány nem számít, ha az telepítve van, használja a tényleges toodiscover hello egy [Analytics lekérdezési](app-insights-analytics.md) Ez például:</span><span class="sxs-lookup"><span data-stu-id="25618-235">toodiscover hello actual sampling rate no matter where it has been applied, use an [Analytics query](app-insights-analytics.md) such as this:</span></span>

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

<span data-ttu-id="25618-236">Az egyes megőrzi a rekord, `itemCount` hello eredeti rekordok száma, amely azt jelöli, annál too1 + hello előző elvetett rekordok számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="25618-236">In each retained record, `itemCount` indicates hello number of original records that it represents, equal too1 + hello number of previous discarded records.</span></span> 

## <a name="how-does-sampling-work"></a><span data-ttu-id="25618-237">Hogyan működik a mintavételi?</span><span class="sxs-lookup"><span data-stu-id="25618-237">How does sampling work?</span></span>
<span data-ttu-id="25618-238">Rögzített és adaptív mintavételi egyik újdonsága az SDK hello 2.0.0 és újabb verziók esetében az ASP.NET-verziókban.</span><span class="sxs-lookup"><span data-stu-id="25618-238">Fixed-rate and adaptive sampling are a feature of hello SDK in ASP.NET versions from 2.0.0 onwards.</span></span> <span data-ttu-id="25618-239">Adatfeldolgozást mintavételi hello Application Insights szolgáltatás egyik szolgáltatása, és lehet a műveletet, ha hello SDK nem hajt végre mintavétel.</span><span class="sxs-lookup"><span data-stu-id="25618-239">Ingestion sampling is a feature of hello Application Insights service, and can be in operation if hello SDK is not performing sampling.</span></span> 

<span data-ttu-id="25618-240">hello mintavételi algoritmus úgy dönt, mely telemetriai elemek toodrop, és melyik kiépítettektől eltérő tookeep (hogy az hello SDK vagy van hello Application Insights szolgáltatásban).</span><span class="sxs-lookup"><span data-stu-id="25618-240">hello sampling algorithm decides which telemetry items toodrop, and which ones tookeep (whether it's in hello SDK or in hello Application Insights service).</span></span> <span data-ttu-id="25618-241">hello mintavételi döntési különböző szabályok, amelyek célja toopreserve összes egymáshoz adatpontok változatlanok maradnak, egy diagnosztikai megoldást vezet be, hogy végrehajthatóként és megbízhatóbb, még akkor is csökkentett adatkészlet az Application insightsban karbantartása alapul.</span><span class="sxs-lookup"><span data-stu-id="25618-241">hello sampling decision is based on several rules that aim toopreserve all interrelated data points intact, maintaining a diagnostic experience in Application Insights that is actionable and reliable even with a reduced data set.</span></span> <span data-ttu-id="25618-242">Például ha a sikertelen kérelmek az alkalmazás további telemetriai elemeket (például kivétel és naplózza a kérést a nyomkövetések) küld, mintavételi nem elválasztja az ehhez a kérelemhez és egyéb telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="25618-242">For example, if for a failed request your app sends additional telemetry items (such as exception and traces logged from this request), sampling will not split this request and other telemetry.</span></span> <span data-ttu-id="25618-243">Az tartja vagy együtt elutasítja azokat.</span><span class="sxs-lookup"><span data-stu-id="25618-243">It either keeps or drops them all together.</span></span> <span data-ttu-id="25618-244">Ennek eredményeképpen ha hello kérelemrészletek az Application Insightsban tekinti meg, bármikor megtekintheti hello kérelem és a kapcsolódó telemetriai elemek.</span><span class="sxs-lookup"><span data-stu-id="25618-244">As a result, when you look at hello request details in Application Insights, you can always see hello request along with its associated telemetry items.</span></span> 

<span data-ttu-id="25618-245">Az alkalmazások, amelyek meghatározzák a "user" (Ez azt jelenti, hogy a legjellemzőbb webalkalmazások), hello mintavételi döntési hello felhasználói azonosítóját, ami azt jelenti, hogy bármely adott felhasználó összes telemetriai adat vagy megőrzi, vagy kihagyott hello kivonatának alapul.</span><span class="sxs-lookup"><span data-stu-id="25618-245">For applications that define "user" (that is, most typical web applications), hello sampling decision is based on hello hash of hello user id, which means that all telemetry for any particular user is either preserved or dropped.</span></span> <span data-ttu-id="25618-246">Alkalmazások felhasználók számára (például a web services) nem meghatározó hello típusú hello mintavételi döntési hello műveletazonosító hello kérelem alapul.</span><span class="sxs-lookup"><span data-stu-id="25618-246">For hello types of applications that don't define users (such as web services) hello sampling decision is based on hello operation id of hello request.</span></span> <span data-ttu-id="25618-247">Végezetül cikkeknél hello telemetriai adat sem be (például telemetriai elemek nem http-környezetben az aszinkron szál jelentett) és a felhasználó nem művelet azonosítója mintavételi egyszerűen rögzíti a rendelkezésre álló telemetriai elemek százalékaként.</span><span class="sxs-lookup"><span data-stu-id="25618-247">Finally, for hello telemetry items that neither have user nor operation id set (for example telemetry items reported from asynchronous threads with no http context) sampling simply captures a percentage of telemetry items of each type.</span></span> 

<span data-ttu-id="25618-248">Ha telemetriai hátsó tooyou bemutató, hello Application Insights szolgáltatás hello mérőszámok alapján beállítja hello azonos mintavételi arány, amely szerepel a gyűjteményben, a hiányzó adatpontokat hello toocompensate hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="25618-248">When presenting telemetry back tooyou, hello Application Insights service adjusts hello metrics by hello same sampling percentage that was used at hello time of collection, toocompensate for hello missing data points.</span></span> <span data-ttu-id="25618-249">Emiatt az Application Insightsban hello telemetriai megtekintve hello felhasználók azért jelent meg statisztikailag megfelelő becsléseket, amelyek rendkívül szoros toohello valós szám.</span><span class="sxs-lookup"><span data-stu-id="25618-249">Hence, when looking at hello telemetry in Application Insights, hello users are seeing statistically correct approximations that are very close toohello real numbers.</span></span>

<span data-ttu-id="25618-250">hello pontossága hello közelítés nagymértékben függ a konfigurált hello mintavételi arány.</span><span class="sxs-lookup"><span data-stu-id="25618-250">hello accuracy of hello approximation largely depends on hello configured sampling percentage.</span></span> <span data-ttu-id="25618-251">Hello pontossága is, az alkalmazások, amelyek kezelik a felhasználók sok általában hasonló kérelmek nagy mennyiségű növeli.</span><span class="sxs-lookup"><span data-stu-id="25618-251">Also, hello accuracy increases for applications that handle a large volume of generally similar requests from lots of users.</span></span> <span data-ttu-id="25618-252">A hello ugyanakkor, az alkalmazások, amelyek nem működnek a jelentős terhelés, mintavételi nem szükséges, mivel ezek az alkalmazások általában küldhet a telemetriai adatok maradva hello keretén belül a sávszélesség-szabályozás adatvesztés nélkül.</span><span class="sxs-lookup"><span data-stu-id="25618-252">On hello other hand, for applications that don't work with a significant load, sampling is not needed as these applications can usually send all their telemetry while staying within hello quota, without causing data loss from throttling.</span></span> 

<span data-ttu-id="25618-253">Vegye figyelembe, hogy az Application Insights nem minta metrikák és a munkamenetek telemetriai típusok óta ezekhez, hello pontosság csökkenésével magas nemkívánatos lehet.</span><span class="sxs-lookup"><span data-stu-id="25618-253">Note that Application Insights does not sample Metrics and Sessions telemetry types, since for these types, reduction in hello precision can be highly undesirable.</span></span> 

### <a name="adaptive-sampling"></a><span data-ttu-id="25618-254">Adaptív mintavétel</span><span class="sxs-lookup"><span data-stu-id="25618-254">Adaptive sampling</span></span>
<span data-ttu-id="25618-255">Adaptív mintavételi hozzáadása egy összetevő figyelők hello adott jelenlegi átviteli hello SDK, és beállítja hello mintavételi százalékos tootry toostay hello cél maximális sebesség belül.</span><span class="sxs-lookup"><span data-stu-id="25618-255">Adaptive sampling adds a component that monitors hello current rate of transmission from hello SDK, and adjusts hello sampling percentage tootry toostay within hello target maximum rate.</span></span> <span data-ttu-id="25618-256">hello módosítás rendszeres időközönként újraszámítja, és átviteli sebességet kimenő hello mozgóátlaga alapul.</span><span class="sxs-lookup"><span data-stu-id="25618-256">hello adjustment is recalculated at regular intervals, and is based on a moving average of hello outgoing transmission rate.</span></span>

## <a name="sampling-and-hello-javascript-sdk"></a><span data-ttu-id="25618-257">Mintavételi és hello JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="25618-257">Sampling and hello JavaScript SDK</span></span>
<span data-ttu-id="25618-258">hello ügyféloldali (JavaScript) SDK vesz részt a rögzített mintavételi hello kiszolgálóoldali együtt SDK.</span><span class="sxs-lookup"><span data-stu-id="25618-258">hello client-side (JavaScript) SDK participates in fixed-rate sampling in conjunction with hello server-side SDK.</span></span> <span data-ttu-id="25618-259">hello tagolva lapok csak küldése ügyféloldali telemetriai az azonos felhasználók melyik hello kiszolgálóoldali arról döntése túl "sample a." hello</span><span class="sxs-lookup"><span data-stu-id="25618-259">hello instrumented pages will only send client-side telemetry from hello same users for which hello server-side made its decision too"sample in."</span></span> <span data-ttu-id="25618-260">Ez a módszer tervezett toomaintain integritási felhasználói munkamenet között - és kiszolgáló-ügyféloldalon.</span><span class="sxs-lookup"><span data-stu-id="25618-260">This logic is designed toomaintain integrity of user session across client- and server-sides.</span></span> <span data-ttu-id="25618-261">Ennek eredményeképpen bármely Application Insights telemetria adott eleménél minden egyéb telemetriai elemek találhatók a felhasználó vagy a munkamenet.</span><span class="sxs-lookup"><span data-stu-id="25618-261">As a result, from any particular telemetry item in Application Insights you can find all other telemetry items for this user or session.</span></span> 

<span data-ttu-id="25618-262">*Az ügyfél és a kiszolgálóoldali telemetriai ne jelenjen meg koordinált minták fent leírtak.*</span><span class="sxs-lookup"><span data-stu-id="25618-262">*My client and server-side telemetry don't show coordinated samples as you describe above.*</span></span>

* <span data-ttu-id="25618-263">Győződjön meg arról, hogy a rögzített mintavételi mind a kiszolgáló és az ügyfél engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="25618-263">Verify that you enabled fixed-rate sampling both on server and client.</span></span>
* <span data-ttu-id="25618-264">Győződjön meg arról, hogy hello SDK-verzió: 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="25618-264">Make sure that hello SDK version is 2.0 or above.</span></span>
* <span data-ttu-id="25618-265">Ellenőrizze, hogy a beállított hello azonos mintavételi hello ügyfél és kiszolgáló százalékos aránya.</span><span class="sxs-lookup"><span data-stu-id="25618-265">Check that you set hello same sampling percentage in both hello client and server.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="25618-266">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="25618-266">Frequently Asked Questions</span></span>
<span data-ttu-id="25618-267">*Miért nem mintavételi egy egyszerű "collect X valamennyi telemetriai típusú százaléka"?*</span><span class="sxs-lookup"><span data-stu-id="25618-267">*Why isn't sampling a simple "collect X percent of each telemetry type"?*</span></span>

* <span data-ttu-id="25618-268">A mintavételi megközelítést metrika becsléseket a nagyon nagy pontosságú volna meg, amíg képes toocorrelate diagnosztikai adatok felhasználó, a munkamenet és a kérést, ami kritikus diagnosztikai megszűnését.</span><span class="sxs-lookup"><span data-stu-id="25618-268">While this sampling approach would provide with a very high precision in metric approximations, it would break ability toocorrelate diagnostic data per user, session, and request, which is critical for diagnostics.</span></span> <span data-ttu-id="25618-269">Ezért mintavételi works jobban az "összes telemetriai elemek összegyűjtése az X százalék az alkalmazás felhasználók" vagy "app kérelmek százalékos X az összes telemetriai adat gyűjtése" logikát.</span><span class="sxs-lookup"><span data-stu-id="25618-269">Therefore, sampling works better with "collect all telemetry items for X percent of app users", or "collect all telemetry for X percent of app requests" logic.</span></span> <span data-ttu-id="25618-270">Hello telemetriai elemek nem hello kérések (például a háttérben történő aszinkron feldolgozás) társított, hello alá esik biztonsági mentés akkor túl "collect X összes elemet az egyes telemetriai százalékát."</span><span class="sxs-lookup"><span data-stu-id="25618-270">For hello telemetry items not associated with hello requests (such as background asynchronous processing), hello fall back is too"collect X percent of all items for each telemetry type."</span></span> 

<span data-ttu-id="25618-271">*Hello mintavételi arány idővel megváltozhat?*</span><span class="sxs-lookup"><span data-stu-id="25618-271">*Can hello sampling percentage change over time?*</span></span>

* <span data-ttu-id="25618-272">Igen, adaptív mintavételi fokozatosan módosítja hello mintavételi arány alapján hello jelenleg megfigyelt hello telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="25618-272">Yes, adaptive sampling gradually changes hello sampling percentage, based on hello currently observed volume of hello telemetry.</span></span>

<span data-ttu-id="25618-273">*Ha rögzített mintavételi használatához honnan tudhatom, hogy mely mintavételi százalékos működnek hello ajánlott az alkalmazásom?*</span><span class="sxs-lookup"><span data-stu-id="25618-273">*If I use fixed-rate sampling, how do I know which sampling percentage will work hello best for my app?*</span></span>

* <span data-ttu-id="25618-274">Egyik módja az adaptív mintavételi toostart, megtudhatja, mi értékelés rendezi a (lásd fent kérdés hello), és ezután kapcsoló toofixed sebesség mintavételi adott alapján.</span><span class="sxs-lookup"><span data-stu-id="25618-274">One way is toostart with adaptive sampling, find out what rate it settles on (see hello above question), and then switch toofixed-rate sampling using that rate.</span></span> 
  
    <span data-ttu-id="25618-275">Ellenkező esetben tooguess rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="25618-275">Otherwise, you have tooguess.</span></span> <span data-ttu-id="25618-276">Az aktuális telemetriai használatának a AI elemzése, tekintse át az esetleges szabályozási, vagyis lépett fel, és becsült hello kötet hello gyűjtött telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="25618-276">Analyze your current telemetry usage in AI, observe any throttling that is occurring, and estimate hello volume of hello collected telemetry.</span></span> <span data-ttu-id="25618-277">Ezen három bemeneti adatokat, és a választott tarifacsomaghoz javasoljuk, hogy milyen mértékű érdemes tooreduce hello kötet hello gyűjtött telemetriai adatot.</span><span class="sxs-lookup"><span data-stu-id="25618-277">These three inputs, together with your selected pricing tier, suggest how much you might want tooreduce hello volume of hello collected telemetry.</span></span> <span data-ttu-id="25618-278">Azonban a felhasználók hello számának növekedése vagy néhány más shift hello kötet telemetriai adatot előfordulhat, hogy érvénytelenné válnak a becsült.</span><span class="sxs-lookup"><span data-stu-id="25618-278">However, an increase in hello number of your users or some other shift in hello volume of telemetry might invalidate your estimate.</span></span>

<span data-ttu-id="25618-279">*Mi történik, ha a mintavételi arány túl alacsony konfigurálni?*</span><span class="sxs-lookup"><span data-stu-id="25618-279">*What happens if I configure sampling percentage too low?*</span></span>

* <span data-ttu-id="25618-280">A túl alacsony mintavételi arány (over-aggressive mintavételi) kevesebb hello becsléseket hello pontosságát, amikor az Application Insights kísérletek toocompensate hello képi megjelenítés hello adatok hello adatok kötet csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="25618-280">Excessively low sampling percentage (over-aggressive sampling) reduces hello accuracy of hello approximations, when Application Insights attempts toocompensate hello visualization of hello data for hello data volume reduction.</span></span> <span data-ttu-id="25618-281">Is diagnosztikai élmény előfordulhat, hogy csökkenéséhez vezet, rendszereivel hello ritkán sikertelen vagy lassú kérelmek lehet mintát venni ki.</span><span class="sxs-lookup"><span data-stu-id="25618-281">Also, diagnostic experience might be negatively impacted, as some of hello infrequently failing or slow requests may be sampled out.</span></span>

<span data-ttu-id="25618-282">*Mi történik, ha a mintavételi arány túl magas konfigurálni?*</span><span class="sxs-lookup"><span data-stu-id="25618-282">*What happens if I configure sampling percentage too high?*</span></span>

* <span data-ttu-id="25618-283">Hello hello mennyiségű elegendő csökkenést konfigurálása túl magas mintavételi százalékos aránya (nem agresszív elég) eredmények gyűjtött telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="25618-283">Configuring too high sampling percentage (not aggressive enough) results in an insufficient reduction in hello volume of hello collected telemetry.</span></span> <span data-ttu-id="25618-284">Továbbra is problémákat tapasztalhat a telemetriai adatok elvesztését kapcsolódó toothrottling, és az Application Insights hello költségeinek lehet magasabb, mint amennyi lejáró költségek toooverage tervezett.</span><span class="sxs-lookup"><span data-stu-id="25618-284">You may still experience telemetry data loss related toothrottling, and hello cost of using Application Insights might be higher than you planned due toooverage charges.</span></span>

<span data-ttu-id="25618-285">*Milyen platformokon használható mintavételi?*</span><span class="sxs-lookup"><span data-stu-id="25618-285">*On what platforms can I use sampling?*</span></span>

* <span data-ttu-id="25618-286">Adatfeldolgozást mintavételi automatikusan történik az összes telemetriai fent egy bizonyos köteten, ha hello SDK nem hajt végre mintavétel.</span><span class="sxs-lookup"><span data-stu-id="25618-286">Ingestion sampling can occur automatically for any telemetry above a certain volume, if hello SDK is not performing sampling.</span></span> <span data-ttu-id="25618-287">Ez akkor működik, például ha az alkalmazás egy Java-kiszolgálót használ, vagy ha az ASP.NET SDK hello egy régebbi verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="25618-287">This would work, for example, if your app uses a Java server, or if you are using an older version of hello ASP.NET SDK.</span></span>
* <span data-ttu-id="25618-288">ASP.NET SDK verziók 2.0.0 használata vagy újabb (az Azure-ban vagy a saját kiszolgáló üzemelteti), adaptív alapértelmezés szerint a mintavételi kap, de toofixed sebesség átválthatja a fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="25618-288">If you're using ASP.NET SDK versions 2.0.0 and above (hosted either in Azure or on your own server), you get adaptive sampling by default, but you can switch toofixed-rate as described above.</span></span> <span data-ttu-id="25618-289">A rögzített mintavételi hello böngésző SDK automatikusan szinkronizálja toosample kapcsolatos eseményeket.</span><span class="sxs-lookup"><span data-stu-id="25618-289">With fixed-rate sampling, hello browser SDK automatically synchronizes toosample related events.</span></span> 

<span data-ttu-id="25618-290">*Nincsenek az egyes ritka események toosee mindig szeretné. Hogyan kaphatok őket túli hello mintavételi modul?*</span><span class="sxs-lookup"><span data-stu-id="25618-290">*There are certain rare events I always want toosee. How can I get them past hello sampling module?*</span></span>

* <span data-ttu-id="25618-291">Egy új TelemetryConfiguration (nem hello alapértelmezett aktív) rendelkező TelemetryClient külön példányt inicializálni.</span><span class="sxs-lookup"><span data-stu-id="25618-291">Initialize a separate instance of TelemetryClient with a new TelemetryConfiguration (not hello default Active one).</span></span> <span data-ttu-id="25618-292">Használja az adott toosend a ritka eseményeket.</span><span class="sxs-lookup"><span data-stu-id="25618-292">Use that toosend your rare events.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25618-293">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25618-293">Next steps</span></span>
* <span data-ttu-id="25618-294">[Szűrés](app-insights-api-filtering-sampling.md) biztosíthat további szigorú ellenőrzése az SDK küld.</span><span class="sxs-lookup"><span data-stu-id="25618-294">[Filtering](app-insights-api-filtering-sampling.md) can provide more strict control of what your SDK sends.</span></span>

