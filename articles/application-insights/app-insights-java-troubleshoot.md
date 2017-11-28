---
title: "Hibaelhárítás az Application Insights Java webes projekt"
description: "Hibaelhárítási útmutató - figyelési élő Java-alkalmazásokhoz az Application insights szolgáltatással."
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: ce46a4f561a273dc340b090a5bf0c8932a308722
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a><span data-ttu-id="2d228-103">Hibaelhárítás, kérdések és válaszok: Application Insights Java-hoz</span><span class="sxs-lookup"><span data-stu-id="2d228-103">Troubleshooting and Q and A for Application Insights for Java</span></span>
<span data-ttu-id="2d228-104">Kérdések és problémák [Azure Application Insights Java nyelven][java]?</span><span class="sxs-lookup"><span data-stu-id="2d228-104">Questions or problems with [Azure Application Insights in Java][java]?</span></span> <span data-ttu-id="2d228-105">Az alábbiakban néhány tipp.</span><span class="sxs-lookup"><span data-stu-id="2d228-105">Here are some tips.</span></span>

## <a name="build-errors"></a><span data-ttu-id="2d228-106">Build hibák</span><span class="sxs-lookup"><span data-stu-id="2d228-106">Build errors</span></span>
<span data-ttu-id="2d228-107">**Az eclipse-ben, az Application Insights SDK Maven vagy a gradle-lel hozzáadásakor build vagy ellenőrzőösszeg érvényesítési hiba jelenik meg.**</span><span class="sxs-lookup"><span data-stu-id="2d228-107">**In Eclipse, when adding the Application Insights SDK via Maven or Gradle, I get build or checksum validation errors.**</span></span>

* <span data-ttu-id="2d228-108">Ha a függőség <version> elem minta használatával helyettesítő karaktereket is tartalmazó (pl. (Maven) `<version>[1.0,)</version>` vagy (Gradle) `version:'1.0.+'`), próbáljon meg egy adott verziójához helyette például `1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="2d228-108">If the dependency <version> element is using a pattern with wildcard characters (e.g. (Maven) `<version>[1.0,)</version>` or (Gradle) `version:'1.0.+'`), try specifying a specific version instead like `1.0.2`.</span></span> <span data-ttu-id="2d228-109">Tekintse meg a [kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) legújabb verziójának.</span><span class="sxs-lookup"><span data-stu-id="2d228-109">See the [release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) for the latest version.</span></span>

## <a name="no-data"></a><span data-ttu-id="2d228-110">Nincs adat</span><span class="sxs-lookup"><span data-stu-id="2d228-110">No data</span></span>
<span data-ttu-id="2d228-111">**I Application Insights hozzáadása sikeresen befejeződött, és saját alkalmazás futott, de soha nem láthatta, szeretnék adatokat a portálon.**</span><span class="sxs-lookup"><span data-stu-id="2d228-111">**I added Application Insights successfully and ran my app, but I've never seen data in the portal.**</span></span>

* <span data-ttu-id="2d228-112">Várjon egy percet, és kattintson a frissítés parancsra.</span><span class="sxs-lookup"><span data-stu-id="2d228-112">Wait a minute and click Refresh.</span></span> <span data-ttu-id="2d228-113">A két diagramot rendszeresen frissíteni magukat, de manuálisan is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="2d228-113">The charts refresh themselves periodically, but you can also refresh manually.</span></span> <span data-ttu-id="2d228-114">A frissítési időköz az időtartományt a diagram függ.</span><span class="sxs-lookup"><span data-stu-id="2d228-114">The refresh interval depends on the time range of the chart.</span></span>
* <span data-ttu-id="2d228-115">Ellenőrizze, hogy van-e egy instrumentation kulcs van megadva az ApplicationInsights.xml fájlban (a Projekt erőforrások mappa)</span><span class="sxs-lookup"><span data-stu-id="2d228-115">Check that you have an instrumentation key defined in the ApplicationInsights.xml file (in the resources folder in your project)</span></span>
* <span data-ttu-id="2d228-116">Győződjön meg arról, hogy nincs `<DisableTelemetry>true</DisableTelemetry>` csomópont az XML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="2d228-116">Verify that there is no `<DisableTelemetry>true</DisableTelemetry>` node in the xml file.</span></span>
* <span data-ttu-id="2d228-117">A tűzfal akkor előfordulhat, hogy nyissa meg a 80-as és a kimenő forgalmat dc.services.visualstudio.com 443-as TCP-portot. Tekintse meg a [tűzfalkivételeket teljes listája](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="2d228-117">In your firewall, you might have to open TCP ports 80 and 443 for outgoing traffic to dc.services.visualstudio.com. See the [full list of firewall exceptions](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="2d228-118">A Microsoft Azure-ban indítsa el a tábla, és nézze meg a szolgáltatástérkép állapotát.</span><span class="sxs-lookup"><span data-stu-id="2d228-118">In the Microsoft Azure start board, look at the service status map.</span></span> <span data-ttu-id="2d228-119">Ha néhány riasztási jelzések, várjon, amíg azok vissza kellett volna OK majd zárja be és nyissa meg újra az Application Insights-alkalmazás paneljének.</span><span class="sxs-lookup"><span data-stu-id="2d228-119">If there are some alert indications, wait until they have returned to OK and then close and re-open your Application Insights application blade.</span></span>
* <span data-ttu-id="2d228-120">Engedélyezze a naplózást az IDE konzolablak hozzáadásával egy `<SDKLogger />` elem a ApplicationInsights.xml fájlban (a erőforrások a projekt), és a végrehajtásával kerüli meg a [hiba] bejegyzések legfelső szintű csomópontja alatt.</span><span class="sxs-lookup"><span data-stu-id="2d228-120">Turn on logging to the IDE console window, by adding an `<SDKLogger />` element under the root node in the ApplicationInsights.xml file (in the resources folder in your project), and check for entries prefaced with [Error].</span></span>
* <span data-ttu-id="2d228-121">Győződjön meg arról, hogy a megfelelő ApplicationInsights.xml fájl sikeresen betöltötte a Java SDK-t, ha megnézi a konzol kimeneti üzenetek "konfigurációs fájl sikeresen talált" utasítás.</span><span class="sxs-lookup"><span data-stu-id="2d228-121">Make sure that the correct ApplicationInsights.xml file has been successfully loaded by the Java SDK, by looking at the console's output messages for a "Configuration file has been successfully found" statement.</span></span>
* <span data-ttu-id="2d228-122">Ha a konfigurációs fájl nem található, ellenőrizze a kimenet megtekintéséhez, ahol a konfigurációs fájl keres a, és győződjön meg arról, hogy a ApplicationInsights.xml azokat a keresési helyek egyikén található.</span><span class="sxs-lookup"><span data-stu-id="2d228-122">If the config file is not found, check the output messages to see where the config file is being searched for, and make sure that the ApplicationInsights.xml is located in one of those search locations.</span></span> <span data-ttu-id="2d228-123">A szokásos megoldás, mint a konfigurációs fájl elhelyezheti az Application Insights SDK JARs közelében.</span><span class="sxs-lookup"><span data-stu-id="2d228-123">As a rule of thumb, you can place the config file near the Application Insights SDK JARs.</span></span> <span data-ttu-id="2d228-124">Például: a Tomcat, ez azt jelentené a WEBALKALMAZÁS-INF/lib mappát.</span><span class="sxs-lookup"><span data-stu-id="2d228-124">For example: in Tomcat, this would mean the WEB-INF/lib folder.</span></span>

#### <a name="i-used-to-see-data-but-it-has-stopped"></a><span data-ttu-id="2d228-125">Adatok megjelenítéséhez használt, de leállt</span><span class="sxs-lookup"><span data-stu-id="2d228-125">I used to see data, but it has stopped</span></span>
* <span data-ttu-id="2d228-126">Ellenőrizze a [állapot blog](http://blogs.msdn.com/b/applicationinsights-status/).</span><span class="sxs-lookup"><span data-stu-id="2d228-126">Check the [status blog](http://blogs.msdn.com/b/applicationinsights-status/).</span></span>
* <span data-ttu-id="2d228-127">Elérte a havi kvótát, az adatokat?</span><span class="sxs-lookup"><span data-stu-id="2d228-127">Have you hit your monthly quota of data points?</span></span> <span data-ttu-id="2d228-128">Nyissa meg a beállítások/kvóta és az árazás a regisztrációval. Ha igen, frissítse a csomagot, vagy további kapacitást kell fizetnie.</span><span class="sxs-lookup"><span data-stu-id="2d228-128">Open Settings/Quota and Pricing to find out. If so, you can upgrade your plan, or pay for additional capacity.</span></span> <span data-ttu-id="2d228-129">Tekintse meg a [séma árképzési](https://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="2d228-129">See the [pricing scheme](https://azure.microsoft.com/pricing/details/application-insights/).</span></span>

#### <a name="i-dont-see-all-the-data-im-expecting"></a><span data-ttu-id="2d228-130">I vagyok várt adatok nem látható</span><span class="sxs-lookup"><span data-stu-id="2d228-130">I don't see all the data I'm expecting</span></span>
* <span data-ttu-id="2d228-131">Nyissa meg a kvóták és panel megnyitásához, és ellenőrizze e árképzési [mintavételi](app-insights-sampling.md) működik.</span><span class="sxs-lookup"><span data-stu-id="2d228-131">Open the Quotas and Pricing blade and check whether [sampling](app-insights-sampling.md) is in operation.</span></span> <span data-ttu-id="2d228-132">(100 %-os átviteli azt jelenti, hogy a mintavételi műveletben nem.) Az Application Insights szolgáltatás csak a azon részét, az alkalmazásból érkező telemetriai adatok fogadásához állítható be.</span><span class="sxs-lookup"><span data-stu-id="2d228-132">(100% transmission means that sampling isn't in operation.) The Application Insights service can be set to accept only a fraction of the telemetry that arrives from your app.</span></span> <span data-ttu-id="2d228-133">Ez segítséget nyújt tartásához telemetriai adatot a havi kvótán belül.</span><span class="sxs-lookup"><span data-stu-id="2d228-133">This helps you keep within your monthly quota of telemetry.</span></span> 

## <a name="no-usage-data"></a><span data-ttu-id="2d228-134">Nincs használati adatok</span><span class="sxs-lookup"><span data-stu-id="2d228-134">No usage data</span></span>
<span data-ttu-id="2d228-135">**Kérelmek és válaszidejét, de nincs lapmegtekintés, böngésző vagy felhasználói adatok láthatók.**</span><span class="sxs-lookup"><span data-stu-id="2d228-135">**I see data about requests and response times, but no page view, browser, or user data.**</span></span>

<span data-ttu-id="2d228-136">Sikeresen állít be az alkalmazás telemetriai adatokat küldhet a kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="2d228-136">You successfully set up your app to send telemetry from the server.</span></span> <span data-ttu-id="2d228-137">Most a következő lépés [telemetriai adatokat küldhet a webböngészőből beállítása a weblapok][usage].</span><span class="sxs-lookup"><span data-stu-id="2d228-137">Now your next step is to [set up your web pages to send telemetry from the web browser][usage].</span></span>

<span data-ttu-id="2d228-138">Azt is megteheti Ha az ügyfél az alkalmazásban egy [telefon vagy más eszköz][platforms], ott telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="2d228-138">Alternatively, if your client is an app in a [phone or other device][platforms], you can send telemetry from there.</span></span> 

<span data-ttu-id="2d228-139">Az ügyfél- és telemetria létrehozásához használja a instrumentation ugyanazzal a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="2d228-139">Use the same instrumentation key to set up both your client and server telemetry.</span></span> <span data-ttu-id="2d228-140">Az adatok jelennek meg az ugyanazon az Application Insights-erőforrás, és az ügyfél és kiszolgáló események összefüggéseket képes lesz.</span><span class="sxs-lookup"><span data-stu-id="2d228-140">The data will appear in the same Application Insights resource, and you'll be able to correlate events from client and server.</span></span>


## <a name="disabling-telemetry"></a><span data-ttu-id="2d228-141">Telemetria letiltása</span><span class="sxs-lookup"><span data-stu-id="2d228-141">Disabling telemetry</span></span>
<span data-ttu-id="2d228-142">**Hogyan letilthatja a telemetriai adatok gyűjtemény?**</span><span class="sxs-lookup"><span data-stu-id="2d228-142">**How can I disable telemetry collection?**</span></span>

<span data-ttu-id="2d228-143">A kódban:</span><span class="sxs-lookup"><span data-stu-id="2d228-143">In code:</span></span>

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

<span data-ttu-id="2d228-144">**Vagy**</span><span class="sxs-lookup"><span data-stu-id="2d228-144">**Or**</span></span> 

<span data-ttu-id="2d228-145">Frissítés ApplicationInsights.xml (a a Projekt erőforrások mappában).</span><span class="sxs-lookup"><span data-stu-id="2d228-145">Update ApplicationInsights.xml (in the resources folder in your project).</span></span> <span data-ttu-id="2d228-146">Adja hozzá a következő, a legfelső szintű csomópontja alatt:</span><span class="sxs-lookup"><span data-stu-id="2d228-146">Add the following under the root node:</span></span>

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

<span data-ttu-id="2d228-147">XML módszert használ, akkor az alkalmazás újraindítása, ha megváltoztatja az értéket.</span><span class="sxs-lookup"><span data-stu-id="2d228-147">Using the XML method, you have to restart the application when you change the value.</span></span>

## <a name="changing-the-target"></a><span data-ttu-id="2d228-148">A tároló módosítása</span><span class="sxs-lookup"><span data-stu-id="2d228-148">Changing the target</span></span>
<span data-ttu-id="2d228-149">**Hogyan lehet módosítani a projekt adatokat küld az Azure erőforrás?**</span><span class="sxs-lookup"><span data-stu-id="2d228-149">**How can I change which Azure resource my project sends data to?**</span></span>

* <span data-ttu-id="2d228-150">[Az új erőforrás instrumentation kulcs lekérése.][java]</span><span class="sxs-lookup"><span data-stu-id="2d228-150">[Get the instrumentation key of the new resource.][java]</span></span>
* <span data-ttu-id="2d228-151">Az Application Insights hozzáadni a projekthez az Azure-eszközkészlet használata az eclipse-ben, ha a webes projekt kattintson a jobb gombbal, válassza ki **Azure**, **konfigurálja az Application Insights**, és módosítsa a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2d228-151">If you added Application Insights to your project using the Azure Toolkit for Eclipse, right click your web project, select **Azure**, **Configure Application Insights**, and change the key.</span></span>
* <span data-ttu-id="2d228-152">Ellenkező esetben frissítése a kulcsot a ApplicationInsights.xml a Projekt erőforrások mappában.</span><span class="sxs-lookup"><span data-stu-id="2d228-152">Otherwise, update the key in ApplicationInsights.xml in the resources folder in your project.</span></span>

## <a name="debug-data-from-the-sdk"></a><span data-ttu-id="2d228-153">A hibakeresési adatokat az SDK-ból</span><span class="sxs-lookup"><span data-stu-id="2d228-153">Debug data from the SDK</span></span>

<span data-ttu-id="2d228-154">**Hogyan tudhatom meg, hogy az SDK tevékenységeit?**</span><span class="sxs-lookup"><span data-stu-id="2d228-154">**How can I find out what the SDK is doing?**</span></span>

<span data-ttu-id="2d228-155">Mi történik az API-t a kapcsolatos további információk beszerzéséhez vegye fel a `<SDKLogger/>` ApplicationInsights.xml konfigurációs fájljának legfelső szintű csomópontja alatt.</span><span class="sxs-lookup"><span data-stu-id="2d228-155">To get more information about what's happening in the API, add `<SDKLogger/>` under the root node of the ApplicationInsights.xml configuration file.</span></span>

<span data-ttu-id="2d228-156">Azt is beállíthatja, hogy a kimeneti fájlba naplózó:</span><span class="sxs-lookup"><span data-stu-id="2d228-156">You can also instruct the logger to output to a file:</span></span>

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

<span data-ttu-id="2d228-157">A fájlok alatt található `%temp%\javasdklogs` vagy `java.io.tmpdir` esetén Tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="2d228-157">The files can be found under `%temp%\javasdklogs` or `java.io.tmpdir` in case of Tomcat server.</span></span>


## <a name="the-azure-start-screen"></a><span data-ttu-id="2d228-158">Az Azure kezdőképernyőn</span><span class="sxs-lookup"><span data-stu-id="2d228-158">The Azure start screen</span></span>
<span data-ttu-id="2d228-159">**Vagyok megnézi [az Azure-portálon](https://portal.azure.com). Nem a térkép arról, hogy valami kapcsolatos alkalmazásom?**</span><span class="sxs-lookup"><span data-stu-id="2d228-159">**I'm looking at [the Azure portal](https://portal.azure.com). Does the map tell me something about my app?**</span></span>

<span data-ttu-id="2d228-160">Azure-kiszolgálók állapotának nem, a világ különböző mutatja.</span><span class="sxs-lookup"><span data-stu-id="2d228-160">No, it shows the health of Azure servers around the world.</span></span>

<span data-ttu-id="2d228-161">*Az Azure start tábláról (kezdőképernyő) hogyan található adatok az alkalmazással kapcsolatos?*</span><span class="sxs-lookup"><span data-stu-id="2d228-161">*From the Azure start board (home screen), how do I find data about my app?*</span></span>

<span data-ttu-id="2d228-162">Feltéve, hogy [az alkalmazás beállítása az Application Insights][java], kattintson a Tallózás gombra, válassza ki az Application Insights, és válassza ki az alkalmazáshoz létrehozott alkalmazás-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2d228-162">Assuming you [set up your app for Application Insights][java], click Browse, select Application Insights, and select the app resource you created for your app.</span></span> <span data-ttu-id="2d228-163">A beolvasandó nincs gyorsabb a jövőben, rögzíthető az alkalmazást a start táblán.</span><span class="sxs-lookup"><span data-stu-id="2d228-163">To get there faster in future, you can pin your app to the start board.</span></span>

## <a name="intranet-servers"></a><span data-ttu-id="2d228-164">Intranetes kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="2d228-164">Intranet servers</span></span>
<span data-ttu-id="2d228-165">**Képes a kiszolgáló is figyelése intranetről?**</span><span class="sxs-lookup"><span data-stu-id="2d228-165">**Can I monitor a server on my intranet?**</span></span>

<span data-ttu-id="2d228-166">Igen, megadva, a kiszolgáló telemetriai adatokat küldhet az Application Insights portálon találja meg a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="2d228-166">Yes, provided your server can send telemetry to the Application Insights portal through the public internet.</span></span> 

<span data-ttu-id="2d228-167">A tűzfal akkor előfordulhat, hogy nyissa meg a 80-as és a kimenő forgalom dc.services.visualstudio.com és f5.services.visualstudio.com 443-as TCP-portot.</span><span class="sxs-lookup"><span data-stu-id="2d228-167">In your firewall, you might have to open TCP ports 80 and 443 for outgoing traffic to dc.services.visualstudio.com and f5.services.visualstudio.com.</span></span>

## <a name="data-retention"></a><span data-ttu-id="2d228-168">Adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="2d228-168">Data retention</span></span>
<span data-ttu-id="2d228-169">**Mennyi ideig adatok őrződnek meg a portálon? Az biztonságos?**</span><span class="sxs-lookup"><span data-stu-id="2d228-169">**How long is data retained in the portal? Is it secure?**</span></span>

<span data-ttu-id="2d228-170">Lásd: [az adatmegőrzés és az adatvédelmi][data].</span><span class="sxs-lookup"><span data-stu-id="2d228-170">See [Data retention and privacy][data].</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d228-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d228-171">Next steps</span></span>
<span data-ttu-id="2d228-172">**Az Application Insights saját Java server alkalmazás beállítása I. Mit tehetek?**</span><span class="sxs-lookup"><span data-stu-id="2d228-172">**I set up Application Insights for my Java server app. What else can I do?**</span></span>

* <span data-ttu-id="2d228-173">[A weblapok rendelkezésre állásának figyelése][availability]</span><span class="sxs-lookup"><span data-stu-id="2d228-173">[Monitor availability of your web pages][availability]</span></span>
* <span data-ttu-id="2d228-174">[Weblap megfigyeléséhez][usage]</span><span class="sxs-lookup"><span data-stu-id="2d228-174">[Monitor web page usage][usage]</span></span>
* <span data-ttu-id="2d228-175">[Használat követése és az eszköz alkalmazások problémák diagnosztizálása][platforms]</span><span class="sxs-lookup"><span data-stu-id="2d228-175">[Track usage and diagnose issues in your device apps][platforms]</span></span>
* <span data-ttu-id="2d228-176">[Kód írása az alkalmazás használati nyomkövetéséhez][track]</span><span class="sxs-lookup"><span data-stu-id="2d228-176">[Write code to track usage of your app][track]</span></span>
* <span data-ttu-id="2d228-177">[Diagnosztikai naplók rögzítése][javalogs]</span><span class="sxs-lookup"><span data-stu-id="2d228-177">[Capture diagnostic logs][javalogs]</span></span>

## <a name="get-help"></a><span data-ttu-id="2d228-178">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="2d228-178">Get help</span></span>
* [<span data-ttu-id="2d228-179">Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="2d228-179">Stack Overflow</span></span>](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

