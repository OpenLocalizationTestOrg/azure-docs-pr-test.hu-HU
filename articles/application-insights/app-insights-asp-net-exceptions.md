---
title: "Diagnosztizálja a hibákat és kivételeket a webalkalmazásokban az Azure Application insights szolgáltatással |} Microsoft Docs"
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
ms.openlocfilehash: 7eeacdc6677ccdebb1653e94a163ecb47090b7ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="diagnose-exceptions-in-your-web-apps-with-application-insights"></a><span data-ttu-id="45ecc-103">Kivételek az Application insights szolgáltatással a webalkalmazások diagnosztizálásához</span><span class="sxs-lookup"><span data-stu-id="45ecc-103">Diagnose exceptions in your web apps with Application Insights</span></span>
<span data-ttu-id="45ecc-104">Az élő webalkalmazását kivételek által jelentett [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="45ecc-104">Exceptions in your live web app are reported by [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="45ecc-105">Sikertelen kérelmek hozhatók kivételeket és az ügyfél és a kiszolgáló, az eseményeket, így gyorsan felderítheti az az oka.</span><span class="sxs-lookup"><span data-stu-id="45ecc-105">You can correlate failed requests with exceptions and other events at both the client and server, so that you can quickly diagnose the causes.</span></span>

## <a name="set-up-exception-reporting"></a><span data-ttu-id="45ecc-106">Kivétel jelentéskészítés beállítása</span><span class="sxs-lookup"><span data-stu-id="45ecc-106">Set up exception reporting</span></span>
* <span data-ttu-id="45ecc-107">Szeretné, hogy a kiszolgáló alkalmazás jelentett kivételek:</span><span class="sxs-lookup"><span data-stu-id="45ecc-107">To have exceptions reported from your server app:</span></span>
  * <span data-ttu-id="45ecc-108">Telepítés [Application Insights SDK](app-insights-asp-net.md) az alkalmazás kódjában, vagy</span><span class="sxs-lookup"><span data-stu-id="45ecc-108">Install [Application Insights SDK](app-insights-asp-net.md) in your app code, or</span></span>
  * <span data-ttu-id="45ecc-109">Az IIS webkiszolgálón: futtatása [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); vagy</span><span class="sxs-lookup"><span data-stu-id="45ecc-109">IIS web servers: Run [Application Insights Agent](app-insights-monitor-performance-live-website-now.md); or</span></span>
  * <span data-ttu-id="45ecc-110">Azure-webalkalmazásokban: vegye fel a [Application Insights-bővítmény](app-insights-azure-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="45ecc-110">Azure web apps: Add the [Application Insights Extension](app-insights-azure-web-apps.md)</span></span>
  * <span data-ttu-id="45ecc-111">Java-webalkalmazások: telepítse a [Java-ügynök](app-insights-java-agent.md)</span><span class="sxs-lookup"><span data-stu-id="45ecc-111">Java web apps: Install the [Java agent](app-insights-java-agent.md)</span></span>
* <span data-ttu-id="45ecc-112">Telepítse a [JavaScript részlet](app-insights-javascript.md) a weblapok a böngésző kivételeket.</span><span class="sxs-lookup"><span data-stu-id="45ecc-112">Install the [JavaScript snippet](app-insights-javascript.md) in your web pages to catch browser exceptions.</span></span>
* <span data-ttu-id="45ecc-113">Egyes alkalmazás-keretrendszerbeli vagy az egyes beállítások néhány további lépések elvégzésével további kivételeket kell:</span><span class="sxs-lookup"><span data-stu-id="45ecc-113">In some application frameworks or with some settings, you need to take some extra steps to catch more exceptions:</span></span>
  * [<span data-ttu-id="45ecc-114">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="45ecc-114">Web forms</span></span>](#web-forms)
  * [<span data-ttu-id="45ecc-115">MVC</span><span class="sxs-lookup"><span data-stu-id="45ecc-115">MVC</span></span>](#mvc)
  * [<span data-ttu-id="45ecc-116">Webes API-1.*</span><span class="sxs-lookup"><span data-stu-id="45ecc-116">Web API 1.*</span></span>](#web-api-1)
  * [<span data-ttu-id="45ecc-117">Webes API-k 2.*</span><span class="sxs-lookup"><span data-stu-id="45ecc-117">Web API 2.*</span></span>](#web-api-2)
  * [<span data-ttu-id="45ecc-118">WCF</span><span class="sxs-lookup"><span data-stu-id="45ecc-118">WCF</span></span>](#wcf)

## <a name="diagnosing-exceptions-using-visual-studio"></a><span data-ttu-id="45ecc-119">Visual Studio használatával kivételek diagnosztizálásáról</span><span class="sxs-lookup"><span data-stu-id="45ecc-119">Diagnosing exceptions using Visual Studio</span></span>
<span data-ttu-id="45ecc-120">Nyissa meg az alkalmazás megoldást a Visual Studio, a hibakeresés érdekében.</span><span class="sxs-lookup"><span data-stu-id="45ecc-120">Open the app solution in Visual Studio to help with debugging.</span></span>

<span data-ttu-id="45ecc-121">Futtassa az alkalmazást, és a kiszolgálón vagy a fejlesztői gépen F5 használatával.</span><span class="sxs-lookup"><span data-stu-id="45ecc-121">Run the app, either on your server or on your development machine by using F5.</span></span>

<span data-ttu-id="45ecc-122">Nyissa meg az Application Insights keresési ablak a Visual Studio, majd állítsa be az alkalmazás származó események megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="45ecc-122">Open the Application Insights Search window in Visual Studio, and set it to display events from your app.</span></span> <span data-ttu-id="45ecc-123">Hibakeresése, közben ehhez csak az Application Insights gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="45ecc-123">While you're debugging, you can do this just by clicking the Application Insights button.</span></span>

![Kattintson jobb gombbal a projektre, és válassza ki az Application Insights, nyissa meg.](./media/app-insights-asp-net-exceptions/34.png)

<span data-ttu-id="45ecc-125">Figyelje meg, hogy a jelentésben csak kivételek szűrheti.</span><span class="sxs-lookup"><span data-stu-id="45ecc-125">Notice that you can filter the report to show just exceptions.</span></span>

<span data-ttu-id="45ecc-126">*Nincsenek kivételek megjelenítő? Lásd: [kivételek rögzítése](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="45ecc-126">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>

<span data-ttu-id="45ecc-127">Kattintson a Veremkivonat megjelenítendő kivételjelentést.</span><span class="sxs-lookup"><span data-stu-id="45ecc-127">Click an exception report to show its stack trace.</span></span>
<span data-ttu-id="45ecc-128">Kattintson az alábbi veremkivonatban, a megfelelő kódot fájl megnyitásához egy sor mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="45ecc-128">Click a line reference in the stack trace, to open the relevant code file.</span></span>  

<span data-ttu-id="45ecc-129">A kódban figyelje meg, hogy a CodeLens a kivételek adatait jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="45ecc-129">In the code, notice that CodeLens shows data about the exceptions:</span></span>

![Kivételek CodeLens értesítés.](./media/app-insights-asp-net-exceptions/35.png)

## <a name="diagnosing-failures-using-the-azure-portal"></a><span data-ttu-id="45ecc-131">Az Azure portál használatával hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="45ecc-131">Diagnosing failures using the Azure portal</span></span>
<span data-ttu-id="45ecc-132">Az alkalmazás az Application Insights áttekintésében, a hibák képet jeleníti meg a kivételek diagramok, és nem sikerült a HTTP-kérelmekre, és a kérelmek listája URL-címek, amelyek a leggyakoribb ügyfélellenőrzési hibák okozzák.</span><span class="sxs-lookup"><span data-stu-id="45ecc-132">From the Application Insights overview of your app, the Failures tile shows you charts of exceptions and failed HTTP requests, together with a list of the request URLs that cause the most frequent failures.</span></span>

![Válassza ki a beállítások, hibák](./media/app-insights-asp-net-exceptions/012-start.png)

<span data-ttu-id="45ecc-134">Kattintson a kivételt, ahol a részletek megtekintéséhez és Veremkiíratás egyedi előfordulása eléréséhez a listán sikertelen kivétel típusok egyike:</span><span class="sxs-lookup"><span data-stu-id="45ecc-134">Click through one of the failed exception types in the list to get to individual occurrences of the exception, where you can see the details and stack trace:</span></span>

![Válasszon ki egy példányt a sikertelen kérelmek és a kivétel részletei, a kivétel-példányokban kívánja beolvasni.](./media/app-insights-asp-net-exceptions/030-req-drill.png)

<span data-ttu-id="45ecc-136">**Másik lehetőségként** indítsa el a kérelmek listája és a hozzá kapcsolódó kivételeket található.</span><span class="sxs-lookup"><span data-stu-id="45ecc-136">**Alternatively,** you can start from the list of requests and find exceptions related to it.</span></span>

<span data-ttu-id="45ecc-137">*Nincsenek kivételek megjelenítő? Lásd: [kivételek rögzítése](#exceptions).*</span><span class="sxs-lookup"><span data-stu-id="45ecc-137">*No exceptions showing? See [Capture exceptions](#exceptions).*</span></span>


## <a name="custom-tracing-and-log-data"></a><span data-ttu-id="45ecc-138">Egyéni nyomkövetés és naplóadatok</span><span class="sxs-lookup"><span data-stu-id="45ecc-138">Custom tracing and log data</span></span>
<span data-ttu-id="45ecc-139">Diagnosztikai adatok lekérése az alkalmazáshoz megadott, szúrhat be a saját telemetriai adatokat küldeni a kódot.</span><span class="sxs-lookup"><span data-stu-id="45ecc-139">To get diagnostic data specific to your app, you can insert code to send your own telemetry data.</span></span> <span data-ttu-id="45ecc-140">Ez a kérelem, nézet és egyéb automatikusan gyűjtött adatok mellett diagnosztikai keresési jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="45ecc-140">This displayed in diagnostic search alongside the request, page view and other automatically-collected data.</span></span>

<span data-ttu-id="45ecc-141">Több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="45ecc-141">You have several options:</span></span>

* <span data-ttu-id="45ecc-142">[A trackevent() függvény](app-insights-api-custom-events-metrics.md#trackevent) tipikus felhasználási területe a használati szokásokat, figyelés, de az is elküldi az adatokat az egyéni események diagnosztikai keresésben.</span><span class="sxs-lookup"><span data-stu-id="45ecc-142">[TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) is typically used for monitoring usage patterns, but the data it sends also appears under Custom Events in diagnostic search.</span></span> <span data-ttu-id="45ecc-143">Események megnevezett, és képes továbbítani az kapcsolatikarakterlánc-tulajdonságokat és amelyen is numerikus metrikák [szűrheti a diagnosztikai keresések](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="45ecc-143">Events are named, and can carry string properties and numeric metrics on which you can [filter your diagnostic searches](app-insights-diagnostic-search.md).</span></span>
* <span data-ttu-id="45ecc-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lehetővé teszi, hogy hosszabb adatküldés például a POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="45ecc-144">[TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) lets you send longer data such as POST information.</span></span>
* <span data-ttu-id="45ecc-145">[TrackException()](#exceptions) küld kivételeseményekhez megadhat veremkiíratásokat.</span><span class="sxs-lookup"><span data-stu-id="45ecc-145">[TrackException()](#exceptions) sends stack traces.</span></span> <span data-ttu-id="45ecc-146">[További tudnivalók a kivételek](#exceptions).</span><span class="sxs-lookup"><span data-stu-id="45ecc-146">[More about exceptions](#exceptions).</span></span>
* <span data-ttu-id="45ecc-147">Ha már használ egy naplózási keretrendszert, például a Log4Net vagy NLog, akkor [lesznek a naplók rögzítése](app-insights-asp-net-trace-logs.md) és lássák a diagnosztikai keresési kérelem és a kivétel adatai mellett.</span><span class="sxs-lookup"><span data-stu-id="45ecc-147">If you already use a logging framework like Log4Net or NLog, you can [capture those logs](app-insights-asp-net-trace-logs.md) and see them in diagnostic search alongside request and exception data.</span></span>

<span data-ttu-id="45ecc-148">Ezek az események megtekintéséhez nyissa meg a [keresési](app-insights-diagnostic-search.md), nyissa meg a szűrő, és válassza a egyéni esemény, nyomkövetésre, vagy kivétel.</span><span class="sxs-lookup"><span data-stu-id="45ecc-148">To see these events, open [Search](app-insights-diagnostic-search.md), open Filter, and then choose Custom Event, Trace, or Exception.</span></span>

![Részletezés](./media/app-insights-asp-net-exceptions/viewCustomEvents.png)

> [!NOTE]
> <span data-ttu-id="45ecc-150">Ha az alkalmazása sok telemetriát hoz létre, az adaptív mintavételezési modul automatikusan csökkenti a portálra küldött mennyiséget, és csupán az eseményeket megjelenítő töredékeket küld.</span><span class="sxs-lookup"><span data-stu-id="45ecc-150">If your app generates a lot of telemetry, the adaptive sampling module will automatically reduce the volume that is sent to the portal by sending only a representative fraction of events.</span></span> <span data-ttu-id="45ecc-151">Események azonos művelet részét képező fog kell vagy legyen kiválasztva csoportként, úgy, hogy a kapcsolódó események közti léphet.</span><span class="sxs-lookup"><span data-stu-id="45ecc-151">Events that are part of the same operation will be selected or deselected as a group, so that you can navigate between related events.</span></span> [<span data-ttu-id="45ecc-152">További információk a mintavétel.</span><span class="sxs-lookup"><span data-stu-id="45ecc-152">Learn about sampling.</span></span>](app-insights-sampling.md)
>
>

### <a name="how-to-see-request-post-data"></a><span data-ttu-id="45ecc-153">Kérelem POST adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="45ecc-153">How to see request POST data</span></span>
<span data-ttu-id="45ecc-154">Szolgáltatáskérés részleteinek nem tartalmazza az alkalmazást a POST híváson a küldött adatokat.</span><span class="sxs-lookup"><span data-stu-id="45ecc-154">Request details don't include the data sent to your app in a POST call.</span></span> <span data-ttu-id="45ecc-155">Az adatok jelentette:</span><span class="sxs-lookup"><span data-stu-id="45ecc-155">To have this data reported:</span></span>

* <span data-ttu-id="45ecc-156">[Az SDK telepítése](app-insights-asp-net.md) a alkalmazás projektben.</span><span class="sxs-lookup"><span data-stu-id="45ecc-156">[Install the SDK](app-insights-asp-net.md) in your application project.</span></span>
* <span data-ttu-id="45ecc-157">Helyezze be a kódot az alkalmazás hívása [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span><span class="sxs-lookup"><span data-stu-id="45ecc-157">Insert code in your application to call [Microsoft.ApplicationInsights.TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace).</span></span> <span data-ttu-id="45ecc-158">A POST-adatokat küldenek a üzenet paraméter.</span><span class="sxs-lookup"><span data-stu-id="45ecc-158">Send the POST data in the message parameter.</span></span> <span data-ttu-id="45ecc-159">Nincs a megengedett méretet, a megadott korlát, meg kell próbálnia úgy, hogy az alapvető adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="45ecc-159">There is a limit to the permitted size, so you should try to send just the essential data.</span></span>
* <span data-ttu-id="45ecc-160">A sikertelen kérelmek vizsgálatánál található a társított nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="45ecc-160">When you investigate a failed request, find the associated traces.</span></span>  

![Részletezés](./media/app-insights-asp-net-exceptions/060-req-related.png)

## <span data-ttu-id="45ecc-162"><a name="exceptions"></a>Kivételeket és a kapcsolódó diagnosztikai adatokat rögzítése</span><span class="sxs-lookup"><span data-stu-id="45ecc-162"><a name="exceptions"></a> Capturing exceptions and related diagnostic data</span></span>
<span data-ttu-id="45ecc-163">Először nem jelenik meg a portálon a kivételeket, amelyek az alkalmazás hibákhoz vezethet.</span><span class="sxs-lookup"><span data-stu-id="45ecc-163">At first, you won't see in the portal all the exceptions that cause failures in your app.</span></span> <span data-ttu-id="45ecc-164">Látni fogja, a böngésző kivételek (használata a [JavaScript SDK](app-insights-javascript.md) a weblapok).</span><span class="sxs-lookup"><span data-stu-id="45ecc-164">You'll see any browser exceptions (if you're using the [JavaScript SDK](app-insights-javascript.md) in your web pages).</span></span> <span data-ttu-id="45ecc-165">De a legtöbb kivételek az IIS által észlelt, és el kell írni a kód azok bit.</span><span class="sxs-lookup"><span data-stu-id="45ecc-165">But most server exceptions are caught by IIS and you have to write a bit of code to see them.</span></span>

<span data-ttu-id="45ecc-166">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="45ecc-166">You can:</span></span>

* <span data-ttu-id="45ecc-167">**Kivételek explicit módon jelentkezzen** kód beszúrásával a kivételkezelők jelentheti a kivételeket.</span><span class="sxs-lookup"><span data-stu-id="45ecc-167">**Log exceptions explicitly** by inserting code in exception handlers to report the exceptions.</span></span>
* <span data-ttu-id="45ecc-168">**Kivételek automatikusan rögzítése** úgy konfigurálja az ASP.NET keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="45ecc-168">**Capture exceptions automatically** by configuring your ASP.NET framework.</span></span> <span data-ttu-id="45ecc-169">A szükséges kiegészítés eltérőek a keretrendszer különböző típusú.</span><span class="sxs-lookup"><span data-stu-id="45ecc-169">The necessary additions are different for different types of framework.</span></span>

## <a name="reporting-exceptions-explicitly"></a><span data-ttu-id="45ecc-170">Explicit módon Reporting kivételek</span><span class="sxs-lookup"><span data-stu-id="45ecc-170">Reporting exceptions explicitly</span></span>
<span data-ttu-id="45ecc-171">A legegyszerűbb módja TrackException() hívásakor be egy kivételkezelőbe.</span><span class="sxs-lookup"><span data-stu-id="45ecc-171">The simplest way is to insert a call to TrackException() in an exception handler.</span></span>

<span data-ttu-id="45ecc-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="45ecc-172">JavaScript</span></span>

    try
    { ...
    }
    catch (ex)
    {
      appInsights.trackException(ex, "handler loc",
        {Game: currentGame.Name,
         State: currentGame.State.ToString()});
    }

<span data-ttu-id="45ecc-173">C#</span><span class="sxs-lookup"><span data-stu-id="45ecc-173">C#</span></span>

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

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

<span data-ttu-id="45ecc-174">VISUAL BASIC</span><span class="sxs-lookup"><span data-stu-id="45ecc-174">VB</span></span>

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

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

<span data-ttu-id="45ecc-175">A tulajdonságok és a mérési paraméterek megadása nem kötelező, de hasznos a [szűrést és a hozzáadása](app-insights-diagnostic-search.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="45ecc-175">The properties and measurements parameters are optional, but are useful for [filtering and adding](app-insights-diagnostic-search.md) extra information.</span></span> <span data-ttu-id="45ecc-176">Például ha egy alkalmazás futtatható több játékok, találta egy adott játékkal kapcsolatos összes kivétel jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="45ecc-176">For example, if you have an app that can run several games, you could find all the exception reports related to a particular game.</span></span> <span data-ttu-id="45ecc-177">Szeretné, hogy mindegyik szótár elemek is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="45ecc-177">You can add as many items as you like to each dictionary.</span></span>

## <a name="browser-exceptions"></a><span data-ttu-id="45ecc-178">Böngészőkivételek</span><span class="sxs-lookup"><span data-stu-id="45ecc-178">Browser exceptions</span></span>
<span data-ttu-id="45ecc-179">A legtöbb webböngésző kivételek jelenti.</span><span class="sxs-lookup"><span data-stu-id="45ecc-179">Most browser exceptions are reported.</span></span>

<span data-ttu-id="45ecc-180">Ha a weblap tartalmazza a parancsfájlok tartalomkézbesítési hálózat vagy a más tartományokban, győződjön meg arról, a script kód attribútuma ```crossorigin="anonymous"```, és a kiszolgáló által küldött [CORS fejlécek](http://enable-cors.org/).</span><span class="sxs-lookup"><span data-stu-id="45ecc-180">If your web page includes script files from content delivery networks or other domains, ensure your script tag has the attribute ```crossorigin="anonymous"```,  and that the server sends [CORS headers](http://enable-cors.org/).</span></span> <span data-ttu-id="45ecc-181">Ez lehetővé teszi a veremkivonatot és részletes lekérése nem kezelt JavaScript-kivételeknek az ezekhez az erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="45ecc-181">This will allow you to get a stack trace and detail for unhandled JavaScript exceptions from these resources.</span></span>

## <a name="web-forms"></a><span data-ttu-id="45ecc-182">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="45ecc-182">Web forms</span></span>
<span data-ttu-id="45ecc-183">Web Forms keretrendszerre a HTTP-modulja lesznek képesek a kivételek gyűjtése, ha nincs átirányítja a CustomErrors konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="45ecc-183">For web forms, the HTTP Module will be able to collect the exceptions when there are no redirects configured with CustomErrors.</span></span>

<span data-ttu-id="45ecc-184">De ha rendelkezik active átirányításokat, adja hozzá a következő sorokat Global.asax.cs Application_Error funkciójának.</span><span class="sxs-lookup"><span data-stu-id="45ecc-184">But if you have active redirects, add the following lines to the Application_Error function in Global.asax.cs.</span></span> <span data-ttu-id="45ecc-185">(Ha adja hozzá a Global.asax fájl még nem rendelkezik.)</span><span class="sxs-lookup"><span data-stu-id="45ecc-185">(Add a Global.asax file if you don't already have one.)</span></span>

<span data-ttu-id="45ecc-186">*C#*</span><span class="sxs-lookup"><span data-stu-id="45ecc-186">*C#*</span></span>

    void Application_Error(object sender, EventArgs e)
    {
      if (HttpContext.Current.IsCustomErrorEnabled && Server.GetLastError  () != null)
      {
         var ai = new TelemetryClient(); // or re-use an existing instance

         ai.TrackException(Server.GetLastError());
      }
    }


## <a name="mvc"></a><span data-ttu-id="45ecc-187">MVC</span><span class="sxs-lookup"><span data-stu-id="45ecc-187">MVC</span></span>
<span data-ttu-id="45ecc-188">Ha a [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) konfiguráció `Off`, akkor kivételeket elérhető lesz a [HTTP-modulja](https://msdn.microsoft.com/library/ms178468.aspx) gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="45ecc-188">If the [CustomErrors](https://msdn.microsoft.com/library/h0hfz6fc.aspx) configuration is `Off`, then exceptions will be available for the [HTTP Module](https://msdn.microsoft.com/library/ms178468.aspx) to collect.</span></span> <span data-ttu-id="45ecc-189">Azonban ha `RemoteOnly` (alapértelmezett), vagy `On`, akkor a kivétel törlődik, és nem érhető el az Application Insights segítségével automatikusan begyűjtik a rendszer.</span><span class="sxs-lookup"><span data-stu-id="45ecc-189">However, if it is `RemoteOnly` (default), or `On`, then the exception will be cleared and not available for Application Insights to automatically collect.</span></span> <span data-ttu-id="45ecc-190">Ezt úgy javíthatja ki, amely felülbírálásával a [System.Web.Mvc.HandleErrorAttribute osztály](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), és a felülbírált osztály alkalmazása, ahogy az alábbi különböző MVC verzióihoz ([github forrás](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span><span class="sxs-lookup"><span data-stu-id="45ecc-190">You can fix that by overriding the [System.Web.Mvc.HandleErrorAttribute class](http://msdn.microsoft.com/library/system.web.mvc.handleerrorattribute.aspx), and applying the overridden class as shown for the different MVC versions below ([github source](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions/blob/master/MVC2App/Controllers/AiHandleErrorAttribute.cs)):</span></span>

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
                //If customError is Off, then AI HTTPModule will report the exception
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

#### <a name="mvc-2"></a><span data-ttu-id="45ecc-191">MVC 2</span><span class="sxs-lookup"><span data-stu-id="45ecc-191">MVC 2</span></span>
<span data-ttu-id="45ecc-192">A HandleError attribútum cserélje le a tartományvezérlőket az új attribútumot.</span><span class="sxs-lookup"><span data-stu-id="45ecc-192">Replace the HandleError attribute with your new attribute in your controllers.</span></span>

    namespace MVC2App.Controllers
    {
       [AiHandleError]
       public class HomeController : Controller
       {
    ...

[<span data-ttu-id="45ecc-193">Minta</span><span class="sxs-lookup"><span data-stu-id="45ecc-193">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc2UnhandledExceptions)

#### <a name="mvc-3"></a><span data-ttu-id="45ecc-194">MVC 3</span><span class="sxs-lookup"><span data-stu-id="45ecc-194">MVC 3</span></span>
<span data-ttu-id="45ecc-195">Regisztrálni `AiHandleErrorAttribute` Global.asax.cs globális szűrőként:</span><span class="sxs-lookup"><span data-stu-id="45ecc-195">Register `AiHandleErrorAttribute` as a global filter in Global.asax.cs:</span></span>

    public class MyMvcApplication : System.Web.HttpApplication
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
         filters.Add(new AiHandleErrorAttribute());
      }
     ...

[<span data-ttu-id="45ecc-196">Minta</span><span class="sxs-lookup"><span data-stu-id="45ecc-196">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc3UnhandledExceptionTelemetry)

#### <a name="mvc-4-mvc5"></a><span data-ttu-id="45ecc-197">MVC 4, MVC5</span><span class="sxs-lookup"><span data-stu-id="45ecc-197">MVC 4, MVC5</span></span>
<span data-ttu-id="45ecc-198">FilterConfig.cs globális szűrőként regisztrálja a AiHandleErrorAttribute:</span><span class="sxs-lookup"><span data-stu-id="45ecc-198">Register AiHandleErrorAttribute as a global filter in FilterConfig.cs:</span></span>

    public class FilterConfig
    {
      public static void RegisterGlobalFilters(GlobalFilterCollection filters)
      {
        // Default replaced with the override to track unhandled exceptions
        filters.Add(new AiHandleErrorAttribute());
      }
    }

[<span data-ttu-id="45ecc-199">Minta</span><span class="sxs-lookup"><span data-stu-id="45ecc-199">Sample</span></span>](https://github.com/AppInsightsSamples/Mvc5UnhandledExceptionTelemetry)

## <a name="web-api-1x"></a><span data-ttu-id="45ecc-200">Webes API-t 1.x</span><span class="sxs-lookup"><span data-stu-id="45ecc-200">Web API 1.x</span></span>
<span data-ttu-id="45ecc-201">Bírálja felül System.Web.Http.Filters.ExceptionFilterAttribute:</span><span class="sxs-lookup"><span data-stu-id="45ecc-201">Override System.Web.Http.Filters.ExceptionFilterAttribute:</span></span>

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

<span data-ttu-id="45ecc-202">Felülbírált attribútum hozzáadása adott tartományvezérlők, vagy vegye fel a globális szűrőkonfigurációt a register osztályban:</span><span class="sxs-lookup"><span data-stu-id="45ecc-202">You could add this overridden attribute to specific controllers, or add it to the global filter configuration in the WebApiConfig class:</span></span>

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

[<span data-ttu-id="45ecc-203">Minta</span><span class="sxs-lookup"><span data-stu-id="45ecc-203">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_1.x_UnhandledExceptions)

<span data-ttu-id="45ecc-204">Nincsenek olyan esetek számát, amelyet a kivételszűrők nem tudja kezelni.</span><span class="sxs-lookup"><span data-stu-id="45ecc-204">There are a number of cases that the exception filters cannot handle.</span></span> <span data-ttu-id="45ecc-205">Példa:</span><span class="sxs-lookup"><span data-stu-id="45ecc-205">For example:</span></span>

* <span data-ttu-id="45ecc-206">A vezérlő konstruktorok kivételek.</span><span class="sxs-lookup"><span data-stu-id="45ecc-206">Exceptions thrown from controller constructors.</span></span>
* <span data-ttu-id="45ecc-207">Az üzenet kezelők kivételek.</span><span class="sxs-lookup"><span data-stu-id="45ecc-207">Exceptions thrown from message handlers.</span></span>
* <span data-ttu-id="45ecc-208">Útválasztás során okozott kivételeket.</span><span class="sxs-lookup"><span data-stu-id="45ecc-208">Exceptions thrown during routing.</span></span>
* <span data-ttu-id="45ecc-209">Válasz a tartalom szerializálás során okozott kivételeket.</span><span class="sxs-lookup"><span data-stu-id="45ecc-209">Exceptions thrown during response content serialization.</span></span>

## <a name="web-api-2x"></a><span data-ttu-id="45ecc-210">Web API 2.x</span><span class="sxs-lookup"><span data-stu-id="45ecc-210">Web API 2.x</span></span>
<span data-ttu-id="45ecc-211">Iexceptionlogger felület egy megvalósításának hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="45ecc-211">Add an implementation of IExceptionLogger:</span></span>

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

<span data-ttu-id="45ecc-212">Adja hozzá a register-szolgáltatásaira ezt:</span><span class="sxs-lookup"><span data-stu-id="45ecc-212">Add this to the services in WebApiConfig:</span></span>

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
  <span data-ttu-id="45ecc-213">}</span><span class="sxs-lookup"><span data-stu-id="45ecc-213">}</span></span>

[<span data-ttu-id="45ecc-214">Minta</span><span class="sxs-lookup"><span data-stu-id="45ecc-214">Sample</span></span>](https://github.com/AppInsightsSamples/WebApi_2.x_UnhandledExceptions)

<span data-ttu-id="45ecc-215">Alternatív megoldásként sikerült:</span><span class="sxs-lookup"><span data-stu-id="45ecc-215">As alternatives, you could:</span></span>

1. <span data-ttu-id="45ecc-216">Cserélje le a csak ExceptionHandler IExceptionHandler egyéni végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="45ecc-216">Replace the only ExceptionHandler with a custom implementation of IExceptionHandler.</span></span> <span data-ttu-id="45ecc-217">Csak ezt nevezik, amikor a keretrendszer továbbra is képes, mely válaszüzenetet küldeni (nem amikor a kapcsolat megszakadt példányhoz) kiválasztása</span><span class="sxs-lookup"><span data-stu-id="45ecc-217">This is only called when the framework is still able to choose which response message to send (not when the connection is aborted for instance)</span></span>
2. <span data-ttu-id="45ecc-218">Kivétel szűrők (leírtak a Web API 1.x tartományvezérlőkön a fenti szakaszban) - hívása nem minden esetben.</span><span class="sxs-lookup"><span data-stu-id="45ecc-218">Exception Filters (as described in the section on Web API 1.x controllers above) - not called in all cases.</span></span>

## <a name="wcf"></a><span data-ttu-id="45ecc-219">WCF</span><span class="sxs-lookup"><span data-stu-id="45ecc-219">WCF</span></span>
<span data-ttu-id="45ecc-220">Adjon hozzá egy osztály, amely kibővíti az attribútumot, és megvalósítja az IErrorHandler és az IServiceBehavior.</span><span class="sxs-lookup"><span data-stu-id="45ecc-220">Add a class that extends Attribute and implements IErrorHandler and IServiceBehavior.</span></span>

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

<span data-ttu-id="45ecc-221">Az attribútum hozzáadása a szolgáltatás hitelesítés megvalósításához:</span><span class="sxs-lookup"><span data-stu-id="45ecc-221">Add the attribute to the service implementations:</span></span>

    namespace WcfService4
    {
        [AiLogException]
        public class Service1 : IService1
        {
         ...

[<span data-ttu-id="45ecc-222">Minta</span><span class="sxs-lookup"><span data-stu-id="45ecc-222">Sample</span></span>](https://github.com/AppInsightsSamples/WCFUnhandledExceptions)

## <a name="exception-performance-counters"></a><span data-ttu-id="45ecc-223">Kivétel teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="45ecc-223">Exception performance counters</span></span>
<span data-ttu-id="45ecc-224">Ha rendelkezik [az Application Insights-ügynök telepítése](app-insights-monitor-performance-live-website-now.md) a kiszolgálón, és a kivételeket, .NET mérhető diagram kaphat.</span><span class="sxs-lookup"><span data-stu-id="45ecc-224">If you have [installed the Application Insights Agent](app-insights-monitor-performance-live-website-now.md) on your server, you can get a chart of the exceptions rate, measured by .NET.</span></span> <span data-ttu-id="45ecc-225">Ez magában foglalja a kezelt, mind a nem kezelt .NET-kivételek.</span><span class="sxs-lookup"><span data-stu-id="45ecc-225">This includes both handled and unhandled .NET exceptions.</span></span>

<span data-ttu-id="45ecc-226">Nyissa meg a metrika Explorer panelt, új diagram hozzáadása, és válassza ki **kivétel arány**, a teljesítményszámlálók felsorolt.</span><span class="sxs-lookup"><span data-stu-id="45ecc-226">Open a Metric Explorer blade, add a new chart, and select **Exception rate**, listed under Performance Counters.</span></span>

<span data-ttu-id="45ecc-227">A .NET-keretrendszer sebessége alapján kiszámítja kivételek száma számbavételi időközönként történik, és elosztja a időköz hosszát.</span><span class="sxs-lookup"><span data-stu-id="45ecc-227">The .NET framework calculates the rate by counting the number of exceptions in an interval and dividing by the length of the interval.</span></span>

<span data-ttu-id="45ecc-228">Vegye figyelembe, hogy a "Kivételek" count TrackException jelentések alapján számítja ki az Application Insights portáljáról eltérő lesz.</span><span class="sxs-lookup"><span data-stu-id="45ecc-228">Note that it will be different from the 'Exceptions' count calculated by the Application Insights portal by counting TrackException reports.</span></span> <span data-ttu-id="45ecc-229">A mintavételi időköze különböző, és az SDK nem küldje TrackException jelentések összes kezelt és kezeletlen kivételek.</span><span class="sxs-lookup"><span data-stu-id="45ecc-229">The sampling intervals are different, and the SDK doesn't send TrackException reports for all handled and unhandled exceptions.</span></span>

## <a name="video"></a><span data-ttu-id="45ecc-230">Videó</span><span class="sxs-lookup"><span data-stu-id="45ecc-230">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="next-steps"></a><span data-ttu-id="45ecc-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45ecc-231">Next steps</span></span>
* [<span data-ttu-id="45ecc-232">REST, SQL és más függőségek hívásainak figyelése</span><span class="sxs-lookup"><span data-stu-id="45ecc-232">Monitor REST, SQL and other calls to dependencies</span></span>](app-insights-asp-net-dependencies.md)
* [<span data-ttu-id="45ecc-233">Lapbetöltési idők, a böngésző kivételeket és a AJAX-hívások figyelése</span><span class="sxs-lookup"><span data-stu-id="45ecc-233">Monitor page load times, browser exceptions, and AJAX calls</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="45ecc-234">A figyelő teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="45ecc-234">Monitor performance counters</span></span>](app-insights-performance-counters.md)
