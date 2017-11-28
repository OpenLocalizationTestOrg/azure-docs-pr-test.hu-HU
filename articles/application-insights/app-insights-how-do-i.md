---
title: "aaaHow készíthetek... Azure Application insightsban |} Microsoft Docs"
description: "Az Application insights szolgáltatással kapcsolatos gyakori kérdések."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: 89294c3583b7c4e7998143be6d359f2deb3c8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-i--in-application-insights"></a><span data-ttu-id="37cb7-103">Hogyan tegyem... az Application Insights szolgáltatásban?</span><span class="sxs-lookup"><span data-stu-id="37cb7-103">How do I ... in Application Insights?</span></span>
## <a name="get-an-email-when-"></a><span data-ttu-id="37cb7-104">Az e-mailek Ha...</span><span class="sxs-lookup"><span data-stu-id="37cb7-104">Get an email when ...</span></span>
### <a name="email-if-my-site-goes-down"></a><span data-ttu-id="37cb7-105">E-mailek, ha a hely leáll</span><span class="sxs-lookup"><span data-stu-id="37cb7-105">Email if my site goes down</span></span>
<span data-ttu-id="37cb7-106">Állítsa be az [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-106">Set an [availability web test](app-insights-monitor-web-app-availability.md).</span></span>

### <a name="email-if-my-site-is-overloaded"></a><span data-ttu-id="37cb7-107">Ha a hely túl van terhelve e-mail</span><span class="sxs-lookup"><span data-stu-id="37cb7-107">Email if my site is overloaded</span></span>
<span data-ttu-id="37cb7-108">Állítsa be az [riasztás](app-insights-alerts.md) a **kiszolgáló válaszideje**.</span><span class="sxs-lookup"><span data-stu-id="37cb7-108">Set an [alert](app-insights-alerts.md) on **Server response time**.</span></span> <span data-ttu-id="37cb7-109">A küszöbérték, 1 és 2 másodperc között kell működnie.</span><span class="sxs-lookup"><span data-stu-id="37cb7-109">A threshold between 1 and 2 seconds should work.</span></span>

![](./media/app-insights-how-do-i/030-server.png)

<span data-ttu-id="37cb7-110">Az alkalmazás is lehet, hogy megjelenítése a törzs jeleit hibát kódok vissza.</span><span class="sxs-lookup"><span data-stu-id="37cb7-110">Your app might also show signs of strain by returning failure codes.</span></span> <span data-ttu-id="37cb7-111">Riasztást állíthat be **sikertelen kérelmek**.</span><span class="sxs-lookup"><span data-stu-id="37cb7-111">Set an alert on **Failed requests**.</span></span>

<span data-ttu-id="37cb7-112">Ha a kívánt riasztást tooset **kivételek**, toodo lehetséges, hogy [néhány további telepítési](app-insights-asp-net-exceptions.md) rendelés toosee adataiban.</span><span class="sxs-lookup"><span data-stu-id="37cb7-112">If you want tooset an alert on **Server exceptions**, you might have toodo [some additional setup](app-insights-asp-net-exceptions.md) in order toosee data.</span></span>

### <a name="email-on-exceptions"></a><span data-ttu-id="37cb7-113">E-maileket az kivételek</span><span class="sxs-lookup"><span data-stu-id="37cb7-113">Email on exceptions</span></span>
1. [<span data-ttu-id="37cb7-114">Kivételfigyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="37cb7-114">Set up exception monitoring</span></span>](app-insights-asp-net-exceptions.md)
2. <span data-ttu-id="37cb7-115">[A riasztások](app-insights-alerts.md) hello kivétel a metrika száma</span><span class="sxs-lookup"><span data-stu-id="37cb7-115">[Set an alert](app-insights-alerts.md) on hello Exception count metric</span></span>

### <a name="email-on-an-event-in-my-app"></a><span data-ttu-id="37cb7-116">E-maileket az alkalmazásom az esemény</span><span class="sxs-lookup"><span data-stu-id="37cb7-116">Email on an event in my app</span></span>
<span data-ttu-id="37cb7-117">Tegyük fel, hogy milyen tooget egy e-mailt egy adott esemény bekövetkezésekor.</span><span class="sxs-lookup"><span data-stu-id="37cb7-117">Let's suppose you'd like tooget an email when a specific event occurs.</span></span> <span data-ttu-id="37cb7-118">Az Application Insights közvetlenül ez a lehetőség nem biztosít, de azt is [riasztás küldése metrika ebbe a küszöbérték](app-insights-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-118">Application Insights doesn't provide this facility directly, but it can [send an alert when a metric crosses a threshold](app-insights-alerts.md).</span></span>

<span data-ttu-id="37cb7-119">Riasztások beállítható [egyéni metrikák](app-insights-api-custom-events-metrics.md#trackmetric), azonban nem egyéni események.</span><span class="sxs-lookup"><span data-stu-id="37cb7-119">Alerts can be set on [custom metrics](app-insights-api-custom-events-metrics.md#trackmetric), though not custom events.</span></span> <span data-ttu-id="37cb7-120">Írási néhány kódot tooincrease metrika hello esemény bekövetkezésekor:</span><span class="sxs-lookup"><span data-stu-id="37cb7-120">Write some code tooincrease a metric when hello event occurs:</span></span>

    telemetry.TrackMetric("Alarm", 10);

<span data-ttu-id="37cb7-121">Vagy:</span><span class="sxs-lookup"><span data-stu-id="37cb7-121">or:</span></span>

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

<span data-ttu-id="37cb7-122">Riasztások kétállapotú van, mert rendelkezik toosend alacsony értéket toohave befejeződött hello riasztás meghatározásakor:</span><span class="sxs-lookup"><span data-stu-id="37cb7-122">Because alerts have two states, you have toosend a low value when you consider hello alert toohave ended:</span></span>

    telemetry.TrackMetric("Alarm", 0.5);

<span data-ttu-id="37cb7-123">A diagram létrehozása [metrika explorer](app-insights-metrics-explorer.md) toosee a riasztás:</span><span class="sxs-lookup"><span data-stu-id="37cb7-123">Create a chart in [metric explorer](app-insights-metrics-explorer.md) toosee your alarm:</span></span>

![](./media/app-insights-how-do-i/010-alarm.png)

<span data-ttu-id="37cb7-124">Egy riasztás toofire most állítja, amikor egy rövid időre mid érték fölé megy hello metrika:</span><span class="sxs-lookup"><span data-stu-id="37cb7-124">Now set an alert toofire when hello metric goes above a mid value for a short period:</span></span>

![](./media/app-insights-how-do-i/020-threshold.png)

<span data-ttu-id="37cb7-125">Átlagolási időszak toohello minimális hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="37cb7-125">Set hello averaging period toohello minimum.</span></span>

<span data-ttu-id="37cb7-126">E-mailt fog kapni, amikor hello metrika fölé megy, mind hello küszöbérték alá.</span><span class="sxs-lookup"><span data-stu-id="37cb7-126">You'll get emails both when hello metric goes above and below hello threshold.</span></span>

<span data-ttu-id="37cb7-127">Egyes pontok tooconsider:</span><span class="sxs-lookup"><span data-stu-id="37cb7-127">Some points tooconsider:</span></span>

* <span data-ttu-id="37cb7-128">Riasztást két állapota ("figyelmeztetés" és "kifogástalan") van.</span><span class="sxs-lookup"><span data-stu-id="37cb7-128">An alert has two states ("alert" and "healthy").</span></span> <span data-ttu-id="37cb7-129">hello állapot csak akkor, ha egy metrika érkezik értékeli.</span><span class="sxs-lookup"><span data-stu-id="37cb7-129">hello state is evaluated only when a metric is received.</span></span>
* <span data-ttu-id="37cb7-130">Az e-mail elküldésekor történik, csak akkor, ha hello állapota megváltozik.</span><span class="sxs-lookup"><span data-stu-id="37cb7-130">An email is sent only when hello state changes.</span></span> <span data-ttu-id="37cb7-131">Ezért toosend rendelkezik magas és alacsony értékű.</span><span class="sxs-lookup"><span data-stu-id="37cb7-131">This is why you have toosend both high and low-value metrics.</span></span>
* <span data-ttu-id="37cb7-132">tooevaluate hello riasztás hello átlagos időtartam megelőző hello átvett kapott hello értékek.</span><span class="sxs-lookup"><span data-stu-id="37cb7-132">tooevaluate hello alert, hello average is taken of hello received values over hello preceding period.</span></span> <span data-ttu-id="37cb7-133">Ez mindig megtörténik egy metrika érkezik, e-mailek gyakrabban beállított hello időszakának küldhetők el.</span><span class="sxs-lookup"><span data-stu-id="37cb7-133">This occurs every time a metric is received, so emails can be sent more frequently than hello period you set.</span></span>
* <span data-ttu-id="37cb7-134">Mivel e-mailek küldése mind a "riasztás" és "kifogástalan", érdemes lehet újra végezni a egyszeri esemény kétállapotú feltételeként tooconsider.</span><span class="sxs-lookup"><span data-stu-id="37cb7-134">Since emails are sent both on "alert" and "healthy", you might want tooconsider re-thinking your one-shot event as a two-state condition.</span></span> <span data-ttu-id="37cb7-135">Például helyett a "feladata Befejezve" esemény "folyamatban lévő feladat" állapotba kerültek, ahol kap e-mailek hello kezdő és egy feladat végén.</span><span class="sxs-lookup"><span data-stu-id="37cb7-135">For example, instead of a "job completed" event, have a "job in progress" condition, where you get emails at hello start and end of a job.</span></span>

### <a name="set-up-alerts-automatically"></a><span data-ttu-id="37cb7-136">Riasztások beállítása automatikusan</span><span class="sxs-lookup"><span data-stu-id="37cb7-136">Set up alerts automatically</span></span>
[<span data-ttu-id="37cb7-137">PowerShell toocreate új riasztások</span><span class="sxs-lookup"><span data-stu-id="37cb7-137">Use PowerShell toocreate new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="use-powershell-toomanage-application-insights"></a><span data-ttu-id="37cb7-138">PowerShell tooManage Application Insights használata</span><span class="sxs-lookup"><span data-stu-id="37cb7-138">Use PowerShell tooManage Application Insights</span></span>
* [<span data-ttu-id="37cb7-139">Hozzon létre új erőforrások</span><span class="sxs-lookup"><span data-stu-id="37cb7-139">Create new resources</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="37cb7-140">Hozzon létre új riasztások</span><span class="sxs-lookup"><span data-stu-id="37cb7-140">Create new alerts</span></span>](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a><span data-ttu-id="37cb7-141">Különböző verzióiból külön telemetria</span><span class="sxs-lookup"><span data-stu-id="37cb7-141">Separate telemetry from different versions</span></span>

* <span data-ttu-id="37cb7-142">Egy alkalmazásban több szerepkört: egyetlen Application Insights-erőforrást használjon, és a szűrést végezni cloud_Rolename.</span><span class="sxs-lookup"><span data-stu-id="37cb7-142">Multiple roles in an app: Use a single Application Insights resource, and filter on cloud_Rolename.</span></span> [<span data-ttu-id="37cb7-143">További információ</span><span class="sxs-lookup"><span data-stu-id="37cb7-143">Learn more</span></span>](app-insights-monitor-multi-role-apps.md)
* <span data-ttu-id="37cb7-144">Fejlesztői, tesztelési és verzió elválasztó: különböző Application Insights-erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="37cb7-144">Separating development, test, and release versions: Use different Application Insights resources.</span></span> <span data-ttu-id="37cb7-145">Vegyen fel hello instrumentation kulcsok a Web.config fájlból. [További információ](app-insights-separate-resources.md)</span><span class="sxs-lookup"><span data-stu-id="37cb7-145">Pick up hello instrumentation keys from web.config. [Learn more](app-insights-separate-resources.md)</span></span>
* <span data-ttu-id="37cb7-146">Jelentéskészítési build verziók: a telemetriai adatok inicializáló használatával tulajdonság hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="37cb7-146">Reporting build versions: Add a property using a telemetry initializer.</span></span> [<span data-ttu-id="37cb7-147">További információ</span><span class="sxs-lookup"><span data-stu-id="37cb7-147">Learn more</span></span>](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a><span data-ttu-id="37cb7-148">A figyelő háttérkiszolgálók, illetve az asztali alkalmazások</span><span class="sxs-lookup"><span data-stu-id="37cb7-148">Monitor backend servers and desktop apps</span></span>
<span data-ttu-id="37cb7-149">[Használjon hello Windows Server SDK modul](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-149">[Use hello Windows Server SDK module](app-insights-windows-desktop.md).</span></span>

## <a name="visualize-data"></a><span data-ttu-id="37cb7-150">Adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="37cb7-150">Visualize data</span></span>
#### <a name="dashboard-with-metrics-from-multiple-apps"></a><span data-ttu-id="37cb7-151">A metrikák a több alkalmazás irányítópult</span><span class="sxs-lookup"><span data-stu-id="37cb7-151">Dashboard with metrics from multiple apps</span></span>
* <span data-ttu-id="37cb7-152">A [metrika Explorer](app-insights-metrics-explorer.md), a diagram testreszabásához, és mentse azokat a Kedvencek közé.</span><span class="sxs-lookup"><span data-stu-id="37cb7-152">In [Metric Explorer](app-insights-metrics-explorer.md), customize your chart and save it as a favorite.</span></span> <span data-ttu-id="37cb7-153">Toohello Azure irányítópulton rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="37cb7-153">Pin it toohello Azure dashboard.</span></span>

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a><span data-ttu-id="37cb7-154">Más forrásokból és az Application Insights-adatokkal irányítópult</span><span class="sxs-lookup"><span data-stu-id="37cb7-154">Dashboard with data from other sources and Application Insights</span></span>
* <span data-ttu-id="37cb7-155">[Exportálja a telemetriai adatok tooPower BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-155">[Export telemetry tooPower BI](app-insights-export-power-bi.md).</span></span>

<span data-ttu-id="37cb7-156">Vagy</span><span class="sxs-lookup"><span data-stu-id="37cb7-156">Or</span></span>

* <span data-ttu-id="37cb7-157">A SharePoint használata az irányítópulton megjelenő adatok SharePoint-kijelzők.</span><span class="sxs-lookup"><span data-stu-id="37cb7-157">Use SharePoint as your dashboard, displaying data in SharePoint web parts.</span></span> <span data-ttu-id="37cb7-158">[A folyamatos exportálás és a Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-158">[Use continuous export and Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).</span></span>  <span data-ttu-id="37cb7-159">PowerView tooexamine hello adatbázist használnak, és hozzon létre egy SharePoint-kijelzőt PowerView.</span><span class="sxs-lookup"><span data-stu-id="37cb7-159">Use PowerView tooexamine hello database, and create a SharePoint web part for PowerView.</span></span>

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a><span data-ttu-id="37cb7-160">Hitelesített vagy névtelen felhasználók szűrése</span><span class="sxs-lookup"><span data-stu-id="37cb7-160">Filter out anonymous or authenticated users</span></span>
<span data-ttu-id="37cb7-161">Ha a felhasználói bejelentkezés, beállíthatja azt a hello [hitelesített felhasználói azonosító](app-insights-api-custom-events-metrics.md#authenticated-users). (Ez nem automatikusan megtörténik.)</span><span class="sxs-lookup"><span data-stu-id="37cb7-161">If your users sign in, you can set hello [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users). (It doesn't happen automatically.)</span></span>

<span data-ttu-id="37cb7-162">Ezek közül:</span><span class="sxs-lookup"><span data-stu-id="37cb7-162">You can then:</span></span>

* <span data-ttu-id="37cb7-163">A megadott felhasználói azonosítók keresése</span><span class="sxs-lookup"><span data-stu-id="37cb7-163">Search on specific user ids</span></span>

![](./media/app-insights-how-do-i/110-search.png)

* <span data-ttu-id="37cb7-164">Metrikák tooeither hitelesített vagy névtelen felhasználók szűrése</span><span class="sxs-lookup"><span data-stu-id="37cb7-164">Filter metrics tooeither anonymous or authenticated users</span></span>

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a><span data-ttu-id="37cb7-165">Tulajdonság neve vagy értéke módosítása</span><span class="sxs-lookup"><span data-stu-id="37cb7-165">Modify property names or values</span></span>
<span data-ttu-id="37cb7-166">Hozzon létre egy [szűrő](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="37cb7-166">Create a [filter](app-insights-api-filtering-sampling.md#filtering).</span></span> <span data-ttu-id="37cb7-167">Ez lehetővé teszi módosítása vagy telemetriai szűrő a app tooApplication Insights az elküldés előtt.</span><span class="sxs-lookup"><span data-stu-id="37cb7-167">This lets you modify or filter telemetry before it is sent from your app tooApplication Insights.</span></span>

## <a name="list-specific-users-and-their-usage"></a><span data-ttu-id="37cb7-168">Adott felhasználók listázása és azok használata</span><span class="sxs-lookup"><span data-stu-id="37cb7-168">List specific users and their usage</span></span>
<span data-ttu-id="37cb7-169">Ha csak túl[bizonyos felhasználók keresése](#search-specific-users), beállíthatja a hello [hitelesített felhasználói azonosító](app-insights-api-custom-events-metrics.md#authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="37cb7-169">If you just want too[search for specific users](#search-specific-users), you can set hello [authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users).</span></span>

<span data-ttu-id="37cb7-170">Ha azt szeretné, hogy az adatok, például az oldalak rendelkező felhasználók listáját, megtekintik vagy milyen gyakran jelentkeznek be, két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="37cb7-170">If you want a list of users with data such as what pages they look at or how often they log in, you have two options:</span></span>

* <span data-ttu-id="37cb7-171">[Hitelesített felhasználó azonosítója](app-insights-api-custom-events-metrics.md#authenticated-users), [tooa adatbázis-exportálási](app-insights-code-sample-export-sql-stream-analytics.md) és használja a megfelelő eszközöket tooanalyze nincs felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="37cb7-171">[Set authenticated user id](app-insights-api-custom-events-metrics.md#authenticated-users), [export tooa database](app-insights-code-sample-export-sql-stream-analytics.md) and use suitable tools tooanalyze your user data there.</span></span>
* <span data-ttu-id="37cb7-172">Ha csak kisszámú felhasználók, küldése egyéni események és metrikák hello érdeklő adatok használatával, mint a metrika értékét vagy esemény és beállítás hello felhasználói azonosító tulajdonságként hello.</span><span class="sxs-lookup"><span data-stu-id="37cb7-172">If you have only a small number of users, send custom events or metrics, using hello data of interest as hello metric value or event name, and setting hello user id as a property.</span></span> <span data-ttu-id="37cb7-173">Lapmegtekintések tooanalyze, cserélje le a hello standard JavaScript trackPageView hívás.</span><span class="sxs-lookup"><span data-stu-id="37cb7-173">tooanalyze page views, replace hello standard JavaScript trackPageView call.</span></span> <span data-ttu-id="37cb7-174">tooanalyze kiszolgálóoldali telemetriai adatokat a telemetriai adatok inicializáló tooadd hello felhasználói azonosító tooall server telemetriai használja.</span><span class="sxs-lookup"><span data-stu-id="37cb7-174">tooanalyze server-side telemetry, use a telemetry initializer tooadd hello user id tooall server telemetry.</span></span> <span data-ttu-id="37cb7-175">Ezek közül és a szegmens metrikák és a keresés hello felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="37cb7-175">You can then filter and segment metrics and searches on hello user id.</span></span>

## <a name="reduce-traffic-from-my-app-tooapplication-insights"></a><span data-ttu-id="37cb7-176">Saját alkalmazás tooApplication Insights a forgalom csökkentése</span><span class="sxs-lookup"><span data-stu-id="37cb7-176">Reduce traffic from my app tooApplication Insights</span></span>
* <span data-ttu-id="37cb7-177">A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), tiltsa le az ilyen hello teljesítmény Számláló adatgyűjtő nincs szüksége, modul.</span><span class="sxs-lookup"><span data-stu-id="37cb7-177">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), disable any modules you don't need, such hello performance counter collector.</span></span>
* <span data-ttu-id="37cb7-178">Használjon [mintavételi és a szűrés](app-insights-api-filtering-sampling.md) : hello SDK.</span><span class="sxs-lookup"><span data-stu-id="37cb7-178">Use [Sampling and filtering](app-insights-api-filtering-sampling.md) at hello SDK.</span></span>
* <span data-ttu-id="37cb7-179">A weblapok minden lapmegtekintés jelentett Ajax-hívások hello számának korlátozása.</span><span class="sxs-lookup"><span data-stu-id="37cb7-179">In your web pages, Limit hello number of Ajax calls reported for every page view.</span></span> <span data-ttu-id="37cb7-180">A hello parancsfájl részlet után `instrumentationKey:...` , beszúrása: `,maxAjaxCallsPerView:3` (vagy megfelelő számú).</span><span class="sxs-lookup"><span data-stu-id="37cb7-180">In hello script snippet after `instrumentationKey:...` , insert: `,maxAjaxCallsPerView:3` (or a suitable number).</span></span>
* <span data-ttu-id="37cb7-181">Ha használ [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), számítási hello összesítés metrika értékek kötegek hello eredmény elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="37cb7-181">If you're using [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), compute hello aggregate of batches of metric values before sending hello result.</span></span> <span data-ttu-id="37cb7-182">A trackmetric() függvény, amely biztosítja, hogy egy túlterhelése van.</span><span class="sxs-lookup"><span data-stu-id="37cb7-182">There's an overload of TrackMetric() that provides for that.</span></span>

<span data-ttu-id="37cb7-183">További információ [tarifa- és a kvóták](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-183">Learn more about [pricing and quotas](app-insights-pricing.md).</span></span>

## <a name="disable-telemetry"></a><span data-ttu-id="37cb7-184">Telemetria letiltása</span><span class="sxs-lookup"><span data-stu-id="37cb7-184">Disable telemetry</span></span>
<span data-ttu-id="37cb7-185">túl**dinamikusan leállítására és elindítására** hello összegyűjtése és hello kiszolgálóról telemetriai adatok továbbítása:</span><span class="sxs-lookup"><span data-stu-id="37cb7-185">too**dynamically stop and start** hello collection and transmission of telemetry from hello server:</span></span>

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



<span data-ttu-id="37cb7-186">túl**tiltsa le a kiválasztott szabványos gyűjtők** – például teljesítményszámlálókat, HTTP-kérelmek vagy függőségek - törlése vagy hello megfelelő sorok megjegyzéssé [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Sikerült ehhez, például ha toosend saját TrackRequest adatokat.</span><span class="sxs-lookup"><span data-stu-id="37cb7-186">too**disable selected standard collectors** - for example, performance counters, HTTP requests, or dependencies - delete or comment out hello relevant lines in [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). You could do this, for example, if you want toosend your own TrackRequest data.</span></span>

## <a name="view-system-performance-counters"></a><span data-ttu-id="37cb7-187">Nézet rendszerteljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="37cb7-187">View system performance counters</span></span>
<span data-ttu-id="37cb7-188">Hello között is megjeleníthetők a metrikaböngészőben metrikák olyan rendszerteljesítmény-számlálók.</span><span class="sxs-lookup"><span data-stu-id="37cb7-188">Among hello metrics you can show in metrics explorer are a set of system performance counters.</span></span> <span data-ttu-id="37cb7-189">Van egy előre meghatározott panel című **kiszolgálók** , amely megjeleníti, hogy ezek.</span><span class="sxs-lookup"><span data-stu-id="37cb7-189">There's a predefined blade titled **Servers** that displays several of them.</span></span>

![Nyissa meg az Application Insights-erőforrást, és kattintson a kiszolgálók](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a><span data-ttu-id="37cb7-191">Ha nincs teljesítményszámláló-adatok</span><span class="sxs-lookup"><span data-stu-id="37cb7-191">If you see no performance counter data</span></span>
* <span data-ttu-id="37cb7-192">**IIS-kiszolgáló** a saját számítógépén vagy a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="37cb7-192">**IIS server** on your own machine or on a VM.</span></span> <span data-ttu-id="37cb7-193">[Állapotmonitor telepítése](app-insights-monitor-performance-live-website-now.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-193">[Install Status Monitor](app-insights-monitor-performance-live-website-now.md).</span></span>
* <span data-ttu-id="37cb7-194">**Azure-webhelyre** -teljesítményszámlálók még nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="37cb7-194">**Azure web site** - we don't support performance counters yet.</span></span> <span data-ttu-id="37cb7-195">Nincsenek több metrikákat kaphat a hello Azure webhelyére Vezérlőpult szabványos részeként.</span><span class="sxs-lookup"><span data-stu-id="37cb7-195">There are several metrics you can get as a standard part of hello Azure web site control panel.</span></span>
* <span data-ttu-id="37cb7-196">**UNIX kiszolgáló** - [collectd telepítése](app-insights-java-collectd.md)</span><span class="sxs-lookup"><span data-stu-id="37cb7-196">**Unix server** - [Install collectd](app-insights-java-collectd.md)</span></span>

### <a name="toodisplay-more-performance-counters"></a><span data-ttu-id="37cb7-197">toodisplay további teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="37cb7-197">toodisplay more performance counters</span></span>
* <span data-ttu-id="37cb7-198">Első, [új diagram hozzáadása](app-insights-metrics-explorer.md) , és ellenőrizze, hogy ha hello a számláló az alapszintű hello beállítása, hogy fel.</span><span class="sxs-lookup"><span data-stu-id="37cb7-198">First, [add a new chart](app-insights-metrics-explorer.md) and see if hello counter is in hello basic set that we offer.</span></span>
* <span data-ttu-id="37cb7-199">Ha nem, [hello toohello számlálókészlet hello teljesítményszámláló modul által gyűjtött hozzáadása](app-insights-performance-counters.md).</span><span class="sxs-lookup"><span data-stu-id="37cb7-199">If not, [add hello counter toohello set collected by hello performance counter module](app-insights-performance-counters.md).</span></span>
