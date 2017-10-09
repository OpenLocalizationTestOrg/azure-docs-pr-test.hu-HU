---
title: "aaaDiagnose hibákat és kivételeket a webes alkalmazásokat az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "ASP.NET alkalmazások együtt – kéréstelemetria kivételei rögzíti."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d1e98390-3ce4-4d04-9351-144314a42aa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 8930e6d2b29f83ea635c4ecb7afd11fc1d97d085
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a><span data-ttu-id="d9523-103">Kivételek az Application insights szolgáltatással a webalkalmazások diagnosztizálásához</span><span class="sxs-lookup"><span data-stu-id="d9523-103">Diagnose exceptions in your web apps with Application Insights</span></span>
<span data-ttu-id="d9523-104">Az élő webalkalmazását kivételek által jelentett [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d9523-104">Exceptions in your live web app are reported by [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="d9523-105">Sikertelen kérelmek hozhatók kivételeket és hello ügyfél és a kiszolgáló, az eseményeket, így gyorsan diagnosztizálhatja a hello okok.</span><span class="sxs-lookup"><span data-stu-id="d9523-105">You can correlate failed requests with exceptions and other events at both hello client and server, so that you can quickly diagnose hello causes.</span></span>

## <a name="set-up-exception-reporting"></a><span data-ttu-id="d9523-106">Kivétel jelentéskészítés beállítása</span><span class="sxs-lookup"><span data-stu-id="d9523-106">Set up exception reporting</span></span>
* <span data-ttu-id="d9523-107">a kiszolgáló alkalmazás jelentett toohave kivételek:</span><span class="sxs-lookup"><span data-stu-id="d9523-107">toohave exceptions reported from your server app:</span></span>
  * <span data-ttu-id="d9523-108">Telepítés [Application Insights SDK](app-insights-asp-net.md) az alkalmazás kódjában, vagy</span><span class="sxs-lookup"><span data-stu-id="d9523-108">Install [Application Insights SDK](app-insights-asp-net.md) in your app code, or</span></span>
  * <span data-ttu-id="d9523-109">Az IIS webkiszolgálón: futtatása [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); vagy</span><span class="sxs-lookup"><span data-stu-id="d9523-109">IIS web servers: Run [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); or</span></span>
  * <span data-ttu-id="d9523-110">Azure-webalkalmazásokban: hello hozzáadása [Application Insights-bővítmény](app-insights-azure-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="d9523-110">Azure web apps: Add hello [Application Insights Extension](app-insights-azure-web-apps.md)</span></span>
  * <span data-ttu-id="d9523-111">Java-webalkalmazások: telepítés hello [Java-ügynök](app-insights-java-agent.md)</span><span class="sxs-lookup"><span data-stu-id="d9523-111">Java web apps: Install hello [Java agent](app-insights-java-agent.md)</span></span>
* <span data-ttu-id="d9523-112">Telepítse a hello [JavaScript részlet](app-insights-javascript.md) a weblapok toocatch böngésző kivételek a.</span><span class="sxs-lookup"><span data-stu-id="d9523-112">Install hello [JavaScript snippet](app-insights-javascript.md) in your web pages toocatch browser exceptions.</span></span>
* <span data-ttu-id="d9523-113">Az egyes alkalmazás-keretrendszerbeli vagy néhány beállításokkal, szükség tootake néhány további lépést toocatch további kivételek:</span><span class="sxs-lookup"><span data-stu-id="d9523-113">In some application frameworks or with some settings, you need tootake some extra steps toocatch more exceptions:</span></span>
  * [<span data-ttu-id="d9523-114">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="d9523-114">Web forms</span></span>](#web-forms)
  * [<span data-ttu-id="d9523-115">MVC</span><span class="sxs-lookup"><span data-stu-id="d9523-115">MVC</span></span>](#mvc)
  * [<span data-ttu-id="d9523-116">Webes API-1.*</span><span class="sxs-lookup"><span data-stu-id="d9523-116">Web API 1.*</span></span>](#web-api-1)
  * [<span data-ttu-id="d9523-117">Webes API-k 2.*</span><span class="sxs-lookup"><span data-stu-id="d9523-117">Web API 2.*</span></span>](#web-api-2)
  * [<span data-ttu-id="d9523-118">WCF</span><span class="sxs-lookup"><span data-stu-id="d9523-118">WCF</span></span>](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a><span data-ttu-id="d9523-119">Visual Studio használatával kivételek diagnosztizálásáról</span><span class="sxs-lookup"><span data-stu-id="d9523-119">Diagnosing exceptions using Visual Studio</span></span>
<span data-ttu-id="d9523-120">Nyissa meg a hello app megoldást a Visual Studio toohelp a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="d9523-120">Open hello app solution in Visual Studio toohelp with debugging.</span></span>

<span data-ttu-id="d9523-121">A kiszolgálón vagy a fejlesztői gépen F5 használatával hello alkalmazás fut.</span><span class="sxs-lookup"><span data-stu-id="d9523-121">Run hello app, either on your server or on your development machine by using F5.</span></span>

<span data-ttu-id="d9523-122">A Visual Studio hello Application Insights keresési ablak megnyitásához, majd állítsa be toodisplay események az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="d9523-122">Open hello Application Insights Search window in Visual Studio, and set it toodisplay events from your app.</span></span> <span data-ttu-id="d9523-123">Hibakeresése, közben ehhez csak hello Application Insights gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="d9523-123">While you're debugging, you can do this just by clicking hello Application Insights button.</span></span>

![Kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights részére, nyissa meg.](./media/app-insights-asp-net-exceptions/34.png)

<span data-ttu-id="d9523-125">Figyelje meg, hogy hello jelentés tooshow csak kivételek szűrheti.</span><span class="sxs-lookup"><span data-stu-id="d9523-125">Notice that you can filter hello report tooshow just exceptions.</span></span>

<span data-ttu-id="d9523-126">*Nincsenek kivételek megjelenítő? Lásd: [kivételek rögzítése](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="d9523-126">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>

<span data-ttu-id="d9523-127">Kattintson egy kivétel jelentés tooshow a Veremkivonat.</span><span class="sxs-lookup"><span data-stu-id="d9523-127">Click an exception report tooshow its stack trace.</span></span>
<span data-ttu-id="d9523-128">Kattintson egy sor mutató hivatkozás megadásával hello Veremkivonat, tooopen hello vonatkozó forráskód fájlja.</span><span class="sxs-lookup"><span data-stu-id="d9523-128">Click a line reference in hello stack trace, tooopen hello relevant code file.</span></span>  

<span data-ttu-id="d9523-129">Hello kódban figyelje meg, hogy a CodeLens hello kivételek adatait jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="d9523-129">In hello code, notice that CodeLens shows data about hello exceptions:</span></span>

![Kivételek CodeLens értesítés.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-hello-azure-portal"></a><span data-ttu-id="d9523-131">Hello Azure-portál használatával hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="d9523-131">Diagnosing failures using hello Azure portal</span></span>
<span data-ttu-id="d9523-132">Hello Application Insights az alkalmazás áttekintésében hello hibák csempe elsajátíthatja, hogy a kivételek diagramok és sikertelen HTTP-kérelmekre együtt hello listája kérelem URL-címek, amelyek hello leggyakoribb hibákhoz vezethet.</span><span class="sxs-lookup"><span data-stu-id="d9523-132">From hello Application Insights overview of your app, hello Failures tile shows you charts of exceptions and failed HTTP requests, together with a list of hello request URLs that cause hello most frequent failures.</span></span>

![Válassza ki a beállítások, hibák](./media/app-insights-asp-net-exceptions/012-start.png)

<span data-ttu-id="d9523-134">Kattintson valamelyik hello keresztül nem sikerült a kivétel típusú hello lista tooget tooindividual eseményeket a hello kivétel, ahol hello részleteinek és Veremkivonat:</span><span class="sxs-lookup"><span data-stu-id="d9523-134">Click through one of hello failed exception types in hello list tooget tooindividual occurrences of hello exception, where you can see hello details and stack trace:</span></span>

![Válasszon ki egy példányt a sikertelen kérelmek, valamint a kivétel részletes adatai alapján, majd a tooinstances hello kivétel.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

<span data-ttu-id="d9523-136">**Másik lehetőségként** hello lista-tól kezdődnek, és kivételeket kapcsolódó tooit található.</span><span class="sxs-lookup"><span data-stu-id="d9523-136">**Alternatively,** you can start from hello list of requests and find exceptions related tooit.</span></span>

<span data-ttu-id="d9523-137">*Nincsenek kivételek megjelenítő? Lásd: [kivételek rögzítése](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="d9523-137">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>


## <a name="custom-tracing-and-log-data"></a><span data-ttu-id="d9523-138">Egyéni nyomkövetés és naplóadatok</span><span class="sxs-lookup"><span data-stu-id="d9523-138">Custom tracing and log data</span></span>
<span data-ttu-id="d9523-139">diagnosztikai adatok adott tooyour app tooget, kód toosend beszúrásához saját telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="d9523-139">tooget diagnostic data specific tooyour app, you can insert code toosend your own telemetry data.</span></span> <span data-ttu-id="d9523-140">Ez a diagnosztikai keresési hello kérelem, nézet és egyéb automatikusan gyűjtött adatok mellett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d9523-140">This displayed in diagnostic search alongside hello request, page view and other automatically-collected data.</span></span>

<span data-ttu-id="d9523-141">Több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="d9523-141">You have several options:</span></span>

* <span data-ttu-id="d9523-142">[A trackevent() függvény](app-insights-api-custom-events-metrics.md#trackevent) tipikus felhasználási területe a használati szokásokat, de is küld jelenik meg adat az egyéni események diagnosztikai keresési hello figyelése.</span><span class="sxs-lookup"><span data-stu-id="d9523-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) is typically used for monitoring usage patterns, but hello data it sends also appears under Custom Events in diagnostic search.</span></span> <span data-ttu-id="d9523-143">Események megnevezett, és képes továbbítani az kapcsolatikarakterlánc-tulajdonságokat és amelyen is numerikus metrikák [szűrheti a diagnosztikai keresések](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="d9523-143">Events are named, and can carry string properties and numeric metrics on which you can [filter your diagnostic searches](app-insights-diagnostic-search.md).</span></span>
* <span data-ttu-id="d9523-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lehetővé teszi, hogy hosszabb adatküldés például a POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="d9523-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lets you send longer data such as POST information.</span></span>
* <span data-ttu-id="d9523-145">[TrackException()](#exceptions) küld kivételeseményekhez megadhat veremkiíratásokat.</span><span class="sxs-lookup"><span data-stu-id="d9523-145">[TrackException()](#exceptions) sends stack traces.</span></span> <span data-ttu-id="d9523-146">[További tudnivalók a kivételek](#exceptions).</span><span class="sxs-lookup"><span data-stu-id="d9523-146">[More about exceptions](#exceptions).</span></span>
* <span data-ttu-id="d9523-147">Ha már használ egy naplózási keretrendszert, például a Log4Net vagy NLog, akkor [lesznek a naplók rögzítése](app-insights-asp-net-trace-logs.md) és lássák a diagnosztikai keresési kérelem és a kivétel adatai mellett.</span><span class="sxs-lookup"><span data-stu-id="d9523-147">If you already use a logging framework like Log4Net or NLog, you can [capture those logs](app-insights-asp-net-trace-logs.md) and see them in diagnostic search alongside request and exception data.</span></span>

<span data-ttu-id="d9523-148">toosee ezeket az eseményeket, nyissa meg a [keresési](app-insights-diagnostic-search.md), nyissa meg a szűrő, és válassza a egyéni esemény, nyomkövetésre, vagy kivétel.</span><span class="sxs-lookup"><span data-stu-id="d9523-148">toosee these events, open [Search](app-insights-diagnostic-search.md), open Filter, and then choose Custom Event, Trace, or Exception.</span></span>

![Részletezés](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> <span data-ttu-id="d9523-150">Az alkalmazás nagy mennyiségű telemetriai adatokat állít elő, ha hello adaptív mintavételi modul automatikusan toohello portal reprezentatív része események küldése által küldött hello kötet csökkenti.</span><span class="sxs-lookup"><span data-stu-id="d9523-150">If your app generates a lot of telemetry, hello adaptive sampling module will automatically reduce hello volume that is sent toohello portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="d9523-151">Események eszköz részét képező hello kell kiválasztott vagy nincs kijelölve csoportosan ugyanazt a műveletet úgy, hogy a kapcsolódó események közti léphet.</span><span class="sxs-lookup"><span data-stu-id="d9523-151">Events that are part of hello same operation will be selected or deselected as a group, so that you can navigate between related events.</span></span> [<span data-ttu-id="d9523-152">További információk a mintavétel.</span><span class="sxs-lookup"><span data-stu-id="d9523-152">Learn about sampling.</span></span>](app-insights-sampling.md)
>
>

### <a name="how-toosee-request-post-data"></a><span data-ttu-id="d9523-153">Hogyan toosee POST-adatokat kér</span><span class="sxs-lookup"><span data-stu-id="d9523-153">How toosee request POST data</span></span>
<span data-ttu-id="d9523-154">Szolgáltatáskérés részleteinek nem tartalmazhat tooyour app elküldve a POST híváson hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="d9523-154">Request details don't include hello data sent tooyour app in a POST call.</span></span> <span data-ttu-id="d9523-155">Ezek az adatok jelentett toohave:</span><span class="sxs-lookup"><span data-stu-id="d9523-155">toohave this data reported:</span></span>

* <span data-ttu-id="d9523-156">[Hello SDK telepítése](app-insights-asp-net.md) a alkalmazás projektben.</span><span class="sxs-lookup"><span data-stu-id="d9523-156">[Install hello SDK](app-insights-asp-net.md) in your application project.</span></span>
* <span data-ttu-id="d9523-157">Helyezze be a kódot az alkalmazás toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span><span class="sxs-lookup"><span data-stu-id="d9523-157">Insert code in your application toocall [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span></span> <span data-ttu-id="d9523-158">Hello POST-adatokat küldenek hello üzenet paraméter.</span><span class="sxs-lookup"><span data-stu-id="d9523-158">Send hello POST data in hello message parameter.</span></span> <span data-ttu-id="d9523-159">Nincs mérete engedélyezett toohello, t érdemes kipróbálni! toosend csak hello lényeges adat.</span><span class="sxs-lookup"><span data-stu-id="d9523-159">There is a limit toohello permitted size, so you should try toosend just hello essential data.</span></span>
* <span data-ttu-id="d9523-160">A sikertelen kérelmek vizsgálatánál található hello tartozó nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="d9523-160">When you investigate a failed request, find hello associated traces.</span></span>  

![Részletezés](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <span data-ttu-id="d9523-162"><a name="exceptions"></a>Kivételeket és a kapcsolódó diagnosztikai adatokat rögzítése</span><span class="sxs-lookup"><span data-stu-id="d9523-162"><a name="exceptions"></a> Capturing exceptions and related diagnostic data</span></span>
<span data-ttu-id="d9523-163">Először nem jelenik meg a portál hello hello hibákhoz vezethet az alkalmazás összes kivétel.</span><span class="sxs-lookup"><span data-stu-id="d9523-163">At first, you won't see in hello portal all hello exceptions that cause failures in your app.</span></span> <span data-ttu-id="d9523-164">Látni fogja, a böngésző kivételek (hello használata [JavaScript SDK](app-insights-javascript.md) a weblapok).</span><span class="sxs-lookup"><span data-stu-id="d9523-164">You'll see any browser exceptions (if you're using hello [JavaScript SDK](app-insights-javascript.md) in your web pages).</span></span> <span data-ttu-id="d9523-165">A legtöbb kivételek az IIS által észlelt és toowrite kód toosee bit van, de azokat.</span><span class="sxs-lookup"><span data-stu-id="d9523-165">But most server exceptions are caught by IIS and you have toowrite a bit of code toosee them.</span></span>

<span data-ttu-id="d9523-166">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="d9523-166">You can:</span></span>

* <span data-ttu-id="d9523-167">**Kivételek explicit módon jelentkezzen** úgy kód kivétel kezelők tooreport hello kivételek.</span><span class="sxs-lookup"><span data-stu-id="d9523-167">**Log exceptions explicitly** by inserting code in exception handlers tooreport hello exceptions.</span></span>
* <span data-ttu-id="d9523-168">**Kivételek automatikusan rögzítése** úgy konfigurálja az ASP.NET keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="d9523-168">**Capture exceptions automatically** by configuring your ASP.NET framework.</span></span> <span data-ttu-id="d9523-169">hello szükséges kiegészítés eltérőek a keretrendszer különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="d9523-169">hello necessary additions are different for different types of framework.</span></span>

## <a name="reporting-exceptions-explicitly"></a><span data-ttu-id="d9523-170">Explicit módon Reporting kivételek</span><span class="sxs-lookup"><span data-stu-id="d9523-170">Reporting exceptions explicitly</span></span>
<span data-ttu-id="d9523-171">hello legegyszerűbb módja a tooinsert egy hívás tooTrackException() a egy kivételkezelőbe.</span><span class="sxs-lookup"><span data-stu-id="d9523-171">hello simplest way is tooinsert a call tooTrackException() in an exception handler.</span></span>

<span data-ttu-id="d9523-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d9523-172">JavaScript</span></span>

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

<span data-ttu-id="d9523-173">C#</span><span class="sxs-lookup"><span data-stu-id="d9523-173">C#</span></span>

    var telemetry = new TelemetryClient();
    ...
    try
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string>
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send hello exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

<span data-ttu-id="d9523-174">VISUAL BASIC</span><span class="sxs-lookup"><span data-stu-id="d9523-174">VB</span></span>

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send hello exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

<span data-ttu-id="d9523-175">hello tulajdonságok és mérések paraméterek megadása nem kötelező, de hasznos a [szűrést és a hozzáadása](app-insights-diagnostic-search.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="d9523-175">hello properties and measurements parameters are optional, but are useful for [filtering and adding](app-insights-diagnostic-search.md) extra information.</span></span> <span data-ttu-id="d9523-176">Például ha egy alkalmazás futtatható több játékok, találta az összes hello kivétel jelentések kapcsolódó tooa játék.</span><span class="sxs-lookup"><span data-stu-id="d9523-176">For example, if you have an app that can run several games, you could find all hello exception reports related tooa particular game.</span></span> <span data-ttu-id="d9523-177">Hozzáadáshoz annyi, ha Ön például tooeach szótárban.</span><span class="sxs-lookup"><span data-stu-id="d9523-177">You can add as many items as you like tooeach dictionary.</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="d9523-178">Böngészőkivételek</span><span class="sxs-lookup"><span data-stu-id="d9523-178">Browser exceptions</span></span>
<span data-ttu-id="d9523-179">A legtöbb webböngésző kivételek jelenti.</span><span class="sxs-lookup"><span data-stu-id="d9523-179">Most browser exceptions are reported.</span></span>

<span data-ttu-id="d9523-180">Ha a weblap tartalmazza a parancsfájlok tartalomkézbesítési hálózat vagy a más tartományokban, győződjön meg arról, a script kód hello attribútummal rendelkezik ```crossorigin="anonymous"```, és adott hello a kiszolgáló elküldi [CORS fejlécek](http://enable-cors.org/).</span><span class="sxs-lookup"><span data-stu-id="d9523-180">If your web page includes script files from content delivery networks or other domains, ensure your script tag has hello attribute ```crossorigin="anonymous"```,  and that hello server sends [CORS headers](http://enable-cors.org/).</span></span> <span data-ttu-id="d9523-181">Ez lehetővé teszi egy Veremkivonat tooget és ezeket az erőforrásokat a nem kezelt JavaScript-kivételek adatai.</span><span class="sxs-lookup"><span data-stu-id="d9523-181">This will allow you tooget a stack trace and detail for unhandled JavaScript exceptions from these resources.</span></span>

## <a name="web-forms"></a><span data-ttu-id="d9523-182">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="d9523-182">Web forms</span></span>
<span data-ttu-id="d9523-183">Web Forms keretrendszerre hello HTTP-modul is képes toocollect hello kivételek nincs konfigurálva a CustomErrors átirányítások esetén.</span><span class="sxs-lookup"><span data-stu-id="d9523-183">For web forms, hello HTTP Module will be able toocollect hello exceptions when there are no redirects configured with CustomErrors.</span></span>

<span data-ttu-id="d9523-184">De ha aktív átirányításokat, adja hozzá a következő sorokat toohello Application_Error függvényt Global.asax.cs hello.</span><span class="sxs-lookup"><span data-stu-id="d9523-184">But if you have active redirects, add hello following lines toohello Application_Error function in Global.asax.cs.</span></span> <span data-ttu-id="d9523-185">(Ha adja hozzá a Global.asax fájl még nem rendelkezik.)</span><span class="sxs-lookup"><span data-stu-id="d9523-185">(Add a Global.asax file if you don't already have one.)</span></span>

<span data-ttu-id="d9523-186">*C#*</span><span class="sxs-lookup"><span data-stu-id="d9523-186">*C#*</span></span>

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a><span data-ttu-id="d9523-187">MVC</span><span class="sxs-lookup"><span data-stu-id="d9523-187">MVC</span></span>
<span data-ttu-id="d9523-188">Ha hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfiguráció `Off`, majd kivételeket lesz elérhető a hello [HTTP-modulja](https://msdn.microsoft.com/library/ms178468.aspx) toocollect.</span><span class="sxs-lookup"><span data-stu-id="d9523-188">If hello [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuration is `Off`, then exceptions will be available for hello [HTTP Module](https://msdn.microsoft.com/library/ms178468.aspx) toocollect.</span></span> <span data-ttu-id="d9523-189">Azonban ha `RemoteOnly` (alapértelmezett), vagy `On`, majd hello kivétel törlődik, és nem érhető el, az Application Insights tooautomatically gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="d9523-189">However, if it is `RemoteOnly` (default), or `On`, then hello exception will be cleared and not available for Application Insights tooautomatically collect.</span></span> <span data-ttu-id="d9523-190">Ezt úgy javíthatja ki, amely hello felülbírálásával [System.Web.Mvc.HandleErrorAttribute osztály](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), és felül hello osztály alkalmazása hello különböző verziójához MVC alább látható módon ([github forrás](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span><span class="sxs-lookup"><span data-stu-id="d9523-190">You can fix that by overriding hello [System.Web.Mvc.HandleErrorAttribute class](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), and applying hello overridden class as shown for hello different MVC versions below ([github source](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span></span>

    using System;
    using System.Web.Mvc;
    using Microsoft.ApplicationInsights;

    namespace MVC2App.Controllers
    {
      [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
      public class AiHandleErrorAttribute : HandleErrorAttribute
      {
        public override void OnException(ExceptionContext filterContext)
        {
            if (filterContext != null && filterContext.HttpContext != null && filterContext.Exception != null)
            {
                //If customError is Off, then AI HTTPModule will report hello exception
                if (filterContext.HttpContext.IsCustomErrorEnabled)
                {   //or reuse instance (recommended!). see note above  
                    var ai = new TelemetryClient();
                    ai.TrackException(filterContext.Exception);
                }
            }
            base.OnException(filterContext);
        }
      }
    }

#### <a name="mvc-2"></a><span data-ttu-id="d9523-191">MVC 2</span><span class="sxs-lookup"><span data-stu-id="d9523-191">MVC 2</span></span>
<span data-ttu-id="d9523-192">Hello HandleError attribútum cserélje le a tartományvezérlőket az új attribútumot.</span><span class="sxs-lookup"><span data-stu-id="d9523-192">Replace hello HandleError attribute with your new attribute in your controllers.</span></span>

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[<span data-ttu-id="d9523-193">Minta</span><span class="sxs-lookup"><span data-stu-id="d9523-193">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a><span data-ttu-id="d9523-194">MVC 3</span><span class="sxs-lookup"><span data-stu-id="d9523-194">MVC 3</span></span>
<span data-ttu-id="d9523-195">Regisztrálni `AiHandleErrorAttribute` Global.asax.cs globális szűrőként:</span><span class="sxs-lookup"><span data-stu-id="d9523-195">Register `AiHandleErrorAttribute` as a global filter in Global.asax.cs:</span></span>

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[<span data-ttu-id="d9523-196">Minta</span><span class="sxs-lookup"><span data-stu-id="d9523-196">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a><span data-ttu-id="d9523-197">MVC 4, MVC5</span><span class="sxs-lookup"><span data-stu-id="d9523-197">MVC 4, MVC5</span></span>
<span data-ttu-id="d9523-198">FilterConfig.cs globális szűrőként regisztrálja a AiHandleErrorAttribute:</span><span class="sxs-lookup"><span data-stu-id="d9523-198">Register AiHandleErrorAttribute as a global filter in FilterConfig.cs:</span></span>

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with hello override tootrack unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[<span data-ttu-id="d9523-199">Minta</span><span class="sxs-lookup"><span data-stu-id="d9523-199">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a><span data-ttu-id="d9523-200">Webes API-t 1.x</span><span class="sxs-lookup"><span data-stu-id="d9523-200">Web API 1.x</span></span>
<span data-ttu-id="d9523-201">Bírálja felül System.Web.Http.Filters.ExceptionFilterAttribute:</span><span class="sxs-lookup"><span data-stu-id="d9523-201">Override System.Web.Http.Filters.ExceptionFilterAttribute:</span></span>

    using System.Web.Http.Filters;
    using Microsoft.ApplicationInsights;

    namespace WebAPI.App_Start
    {
      public class AiExceptionFilterAttribute : ExceptionFilterAttribute
      {
        public override void OnException(HttpActionExecutedContext actionExecutedContext)
        {
            if (actionExecutedContext != null && actionExecutedContext.Exception != null)
            {  //or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(actionExecutedContext.Exception);    
            }
            base.OnException(actionExecutedContext);
        }
      }
    }

<span data-ttu-id="d9523-202">Adja hozzá a felülbírált attribútum toospecific tartományvezérlők, vagy vegye fel globális szűrőkonfigurációt toohello hello register osztályban:</span><span class="sxs-lookup"><span data-stu-id="d9523-202">You could add this overridden attribute toospecific controllers, or add it toohello global filter configuration in hello WebApiConfig class:</span></span>

    using System.Web.Http;
    using WebApi1.x.App_Start;

    namespace WebApi1.x
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            config.Routes.MapHttpRoute(name: "DefaultApi", routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional });
            ...
            config.EnableSystemDiagnosticsTracing();

            // Capture exceptions for Application Insights:
            config.Filters.Add(new AiExceptionFilterAttribute());
        }
      }
    }

[<span data-ttu-id="d9523-203">Minta</span><span class="sxs-lookup"><span data-stu-id="d9523-203">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

<span data-ttu-id="d9523-204">Nincsenek olyan esetek számát, amelyet hello kivételszűrők nem tudja kezelni.</span><span class="sxs-lookup"><span data-stu-id="d9523-204">There are a number of cases that hello exception filters cannot handle.</span></span> <span data-ttu-id="d9523-205">Példa:</span><span class="sxs-lookup"><span data-stu-id="d9523-205">For example:</span></span>

* <span data-ttu-id="d9523-206">A vezérlő konstruktorok kivételek.</span><span class="sxs-lookup"><span data-stu-id="d9523-206">Exceptions thrown from controller constructors.</span></span>
* <span data-ttu-id="d9523-207">Az üzenet kezelők kivételek.</span><span class="sxs-lookup"><span data-stu-id="d9523-207">Exceptions thrown from message handlers.</span></span>
* <span data-ttu-id="d9523-208">Útválasztás során okozott kivételeket.</span><span class="sxs-lookup"><span data-stu-id="d9523-208">Exceptions thrown during routing.</span></span>
* <span data-ttu-id="d9523-209">Válasz a tartalom szerializálás során okozott kivételeket.</span><span class="sxs-lookup"><span data-stu-id="d9523-209">Exceptions thrown during response content serialization.</span></span>

## <a name="web-api-2x"></a><span data-ttu-id="d9523-210">Web API 2.x</span><span class="sxs-lookup"><span data-stu-id="d9523-210">Web API 2.x</span></span>
<span data-ttu-id="d9523-211">Iexceptionlogger felület egy megvalósításának hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d9523-211">Add an implementation of IExceptionLogger:</span></span>

    using System.Web.Http.ExceptionHandling;
    using Microsoft.ApplicationInsights;

    namespace ProductsAppPureWebAPI.App_Start
    {
      public class AiExceptionLogger : ExceptionLogger
      {
        public override void Log(ExceptionLoggerContext context)
        {
            if (context !=null && context.Exception != null)
            {//or reuse instance (recommended!). see note above
                var ai = new TelemetryClient();
                ai.TrackException(context.Exception);
            }
            base.Log(context);
        }
      }
    }

<span data-ttu-id="d9523-212">Adja hozzá a toohello szolgáltatások register:</span><span class="sxs-lookup"><span data-stu-id="d9523-212">Add this toohello services in WebApiConfig:</span></span>

    using System.Web.Http;
    using System.Web.Http.ExceptionHandling;
    using ProductsAppPureWebAPI.App_Start;

    namespace WebApi2WithMVC
    {
      public static class WebApiConfig
      {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
            config.Services.Add(typeof(IExceptionLogger), new AiExceptionLogger());
        }
      }
  <span data-ttu-id="d9523-213">}</span><span class="sxs-lookup"><span data-stu-id="d9523-213">}</span></span>

[<span data-ttu-id="d9523-214">Minta</span><span class="sxs-lookup"><span data-stu-id="d9523-214">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

<span data-ttu-id="d9523-215">Alternatív megoldásként sikerült:</span><span class="sxs-lookup"><span data-stu-id="d9523-215">As alternatives, you could:</span></span>

1. <span data-ttu-id="d9523-216">Cserélje le csak egy egyéni végrehajtásának IExceptionHandler ExceptionHandler hello.</span><span class="sxs-lookup"><span data-stu-id="d9523-216">Replace hello only ExceptionHandler with a custom implementation of IExceptionHandler.</span></span> <span data-ttu-id="d9523-217">Csak ennek meghívása az hello keretrendszer esetén is képes toochoose mely válasz üzenet toosend (amikor például hello kapcsolat megszakadt. nem)</span><span class="sxs-lookup"><span data-stu-id="d9523-217">This is only called when hello framework is still able toochoose which response message toosend (not when hello connection is aborted for instance)</span></span>
2. <span data-ttu-id="d9523-218">Kivétel szűrők (leírtak hello szakasz a webes API 1.x tartományvezérlőkön a fenti) - hívása nem minden esetben.</span><span class="sxs-lookup"><span data-stu-id="d9523-218">Exception Filters (as described in hello section on Web API 1.x controllers above) - not called in all cases.</span></span>

## <a name="wcf"></a><span data-ttu-id="d9523-219">WCF</span><span class="sxs-lookup"><span data-stu-id="d9523-219">WCF</span></span>
<span data-ttu-id="d9523-220">Adjon hozzá egy osztály, amely kibővíti az attribútumot, és megvalósítja az IErrorHandler és az IServiceBehavior.</span><span class="sxs-lookup"><span data-stu-id="d9523-220">Add a class that extends Attribute and implements IErrorHandler and IServiceBehavior.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.ServiceModel.Description;
    using System.ServiceModel.Dispatcher;
    using System.Web;
    using Microsoft.ApplicationInsights;

    namespace WcfService4.ErrorHandling
    {
      public class AiLogExceptionAttribute : Attribute, IErrorHandler, IServiceBehavior
      {
        public void AddBindingParameters(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase,
            System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints,
            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)
        {
        }

        public void ApplyDispatchBehavior(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
            foreach (ChannelDispatcher disp in serviceHostBase.ChannelDispatchers)
            {
                disp.ErrorHandlers.Add(this);
            }
        }

        public void Validate(ServiceDescription serviceDescription,
            System.ServiceModel.ServiceHostBase serviceHostBase)
        {
        }

        bool IErrorHandler.HandleError(Exception error)
        {//or reuse instance (recommended!). see note above
            var ai = new TelemetryClient();

            ai.TrackException(error);
            return false;
        }

        void IErrorHandler.ProvideFault(Exception error,
            System.ServiceModel.Channels.MessageVersion version,
            ref System.ServiceModel.Channels.Message fault)
        {
        }
      }
    }

<span data-ttu-id="d9523-221">Hello attribútum toohello szolgáltatás megvalósítások hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="d9523-221">Add hello attribute toohello service implementations:</span></span>

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[<span data-ttu-id="d9523-222">Minta</span><span class="sxs-lookup"><span data-stu-id="d9523-222">Sample</span></span>](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a><span data-ttu-id="d9523-223">Kivétel teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="d9523-223">Exception performance counters</span></span>
<span data-ttu-id="d9523-224">Ha rendelkezik [hello Application Insights-ügynök telepítve](app-insights-monitor-performance-live-website-now.md) a kiszolgálón, és hello kivételek, .NET mérhető diagram kaphat.</span><span class="sxs-lookup"><span data-stu-id="d9523-224">If you have [installed hello Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on your server, you can get a chart of hello exceptions rate, measured by .NET.</span></span> <span data-ttu-id="d9523-225">Ez magában foglalja a kezelt, mind a nem kezelt .NET-kivételek.</span><span class="sxs-lookup"><span data-stu-id="d9523-225">This includes both handled and unhandled .NET exceptions.</span></span>

<span data-ttu-id="d9523-226">Nyissa meg a metrika Explorer panelt, új diagram hozzáadása, és válassza ki **kivétel arány**, a teljesítményszámlálók felsorolt.</span><span class="sxs-lookup"><span data-stu-id="d9523-226">Open a Metric Explorer blade, add a new chart, and select **Exception rate**, listed under Performance Counters.</span></span>

<span data-ttu-id="d9523-227">hello .NET-keretrendszer hello arány kivételek száma hello leltár időközönkénti és hello időköz hello hosszát elosztjuk számítja ki.</span><span class="sxs-lookup"><span data-stu-id="d9523-227">hello .NET framework calculates hello rate by counting hello number of exceptions in an interval and dividing by hello length of hello interval.</span></span>

<span data-ttu-id="d9523-228">Vegye figyelembe, hogy az eltérő lesz hello "kivétel" count hello Application Insights portál TrackException jelentések alapján számítja ki.</span><span class="sxs-lookup"><span data-stu-id="d9523-228">Note that it will be different from hello 'Exceptions' count calculated by hello Application Insights portal by counting TrackException reports.</span></span> <span data-ttu-id="d9523-229">hello mintavételi időköze különböző, és hello SDK nem küldje minden kezelt és kezeletlen kivételek TrackException a jelentésekre.</span><span class="sxs-lookup"><span data-stu-id="d9523-229">hello sampling intervals are different, and hello SDK doesn't send TrackException reports for all handled and unhandled exceptions.</span></span>

## <a name="video"></a><span data-ttu-id="d9523-230">Videó</span><span class="sxs-lookup"><span data-stu-id="d9523-230">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a><span data-ttu-id="d9523-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9523-231">Next steps</span></span>
* [<span data-ttu-id="d9523-232">REST, SQL és egyéb hívások toodependencies figyelése</span><span class="sxs-lookup"><span data-stu-id="d9523-232">Monitor REST, SQL and other calls toodependencies</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="d9523-233">Lapbetöltési idők, a böngésző kivételeket és a AJAX-hívások figyelése</span><span class="sxs-lookup"><span data-stu-id="d9523-233">Monitor page load times, browser exceptions, and AJAX calls</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="d9523-234">A figyelő teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="d9523-234">Monitor performance counters</span></span>](app-insights-performance-counters.md)
