---
title: az Application Insights aaaTroubleshoot Java webes projekt
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
ms.openlocfilehash: c274c01b1992971fae194c3e510512ca06ab76b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a><span data-ttu-id="9cf24-103">Hibaelhárítás, kérdések és válaszok: Application Insights Java-hoz</span><span class="sxs-lookup"><span data-stu-id="9cf24-103">Troubleshooting and Q and A for Application Insights for Java</span></span>
<span data-ttu-id="9cf24-104">Kérdések és problémák [Azure Application Insights Java nyelven][java]?</span><span class="sxs-lookup"><span data-stu-id="9cf24-104">Questions or problems with [Azure Application Insights in Java][java]?</span></span> <span data-ttu-id="9cf24-105">Az alábbiakban néhány tipp.</span><span class="sxs-lookup"><span data-stu-id="9cf24-105">Here are some tips.</span></span>

## <a name="build-errors"></a><span data-ttu-id="9cf24-106">Build hibák</span><span class="sxs-lookup"><span data-stu-id="9cf24-106">Build errors</span></span>
<span data-ttu-id="9cf24-107">**Az eclipse-ben Maven vagy a gradle-lel, Application Insights SDK hozzáadása hello kattintáskor build vagy ellenőrzőösszeg érvényesítési hiba.**</span><span class="sxs-lookup"><span data-stu-id="9cf24-107">**In Eclipse, when adding hello Application Insights SDK via Maven or Gradle, I get build or checksum validation errors.**</span></span>

* <span data-ttu-id="9cf24-108">Ha függőségi hello <version> elem minta használatával helyettesítő karaktereket is tartalmazó (pl. (Maven) `<version>[1.0,)</version>` vagy (Gradle) `version:'1.0.+'`), próbáljon meg egy adott verziójához helyette például `1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="9cf24-108">If hello dependency <version> element is using a pattern with wildcard characters (e.g. (Maven) `<version>[1.0,)</version>` or (Gradle) `version:'1.0.+'`), try specifying a specific version instead like `1.0.2`.</span></span> <span data-ttu-id="9cf24-109">Lásd: hello [kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) hello legújabb verziójához.</span><span class="sxs-lookup"><span data-stu-id="9cf24-109">See hello [release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) for hello latest version.</span></span>

## <a name="no-data"></a><span data-ttu-id="9cf24-110">Nincs adat</span><span class="sxs-lookup"><span data-stu-id="9cf24-110">No data</span></span>
<span data-ttu-id="9cf24-111">**I Application Insights hozzáadása sikeresen befejeződött, és saját alkalmazás lefutott, de soha nem láthatta, szeretnék adatokat hello portálon.**</span><span class="sxs-lookup"><span data-stu-id="9cf24-111">**I added Application Insights successfully and ran my app, but I've never seen data in hello portal.**</span></span>

* <span data-ttu-id="9cf24-112">Várjon egy percet, és kattintson a frissítés parancsra.</span><span class="sxs-lookup"><span data-stu-id="9cf24-112">Wait a minute and click Refresh.</span></span> <span data-ttu-id="9cf24-113">hello diagramok rendszeresen frissítse magát, de manuálisan is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="9cf24-113">hello charts refresh themselves periodically, but you can also refresh manually.</span></span> <span data-ttu-id="9cf24-114">hello frissítési időköze hello időtartománya hello diagram függ.</span><span class="sxs-lookup"><span data-stu-id="9cf24-114">hello refresh interval depends on hello time range of hello chart.</span></span>
* <span data-ttu-id="9cf24-115">Ellenőrizze, hogy van-e egy rendszerállapot-kulcsot a hello ApplicationInsights.xml fájlban (a hello erőforrások mappában a projekt)</span><span class="sxs-lookup"><span data-stu-id="9cf24-115">Check that you have an instrumentation key defined in hello ApplicationInsights.xml file (in hello resources folder in your project)</span></span>
* <span data-ttu-id="9cf24-116">Győződjön meg arról, hogy nincs `<DisableTelemetry>true</DisableTelemetry>` csomópont hello XML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="9cf24-116">Verify that there is no `<DisableTelemetry>true</DisableTelemetry>` node in hello xml file.</span></span>
* <span data-ttu-id="9cf24-117">A tűzfal lehetséges, hogy 80-as és 443-as kimenő forgalom toodc.services.visualstudio.com tooopen TCP-portok. Lásd: hello [tűzfalkivételeket teljes listája](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="9cf24-117">In your firewall, you might have tooopen TCP ports 80 and 443 for outgoing traffic toodc.services.visualstudio.com. See hello [full list of firewall exceptions](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="9cf24-118">A Microsoft Azure hello board indításához hello állapot szolgáltatástérkép tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="9cf24-118">In hello Microsoft Azure start board, look at hello service status map.</span></span> <span data-ttu-id="9cf24-119">Ha néhány riasztási jelzések, várjon, amíg azok vissza kellett volna tooOK majd zárja be és nyissa meg újra az Application Insights-alkalmazás paneljének.</span><span class="sxs-lookup"><span data-stu-id="9cf24-119">If there are some alert indications, wait until they have returned tooOK and then close and re-open your Application Insights application blade.</span></span>
* <span data-ttu-id="9cf24-120">Kapcsolja be a naplózás toohello IDE konzolablakban hozzáadásával egy `<SDKLogger />` elem hello ApplicationInsights.xml fájlban (a hello erőforrások mappában a projekt), és ellenőrzés végrehajtásával kerüli meg a [hiba] bejegyzések hello legfelső szintű csomópontja alatt.</span><span class="sxs-lookup"><span data-stu-id="9cf24-120">Turn on logging toohello IDE console window, by adding an `<SDKLogger />` element under hello root node in hello ApplicationInsights.xml file (in hello resources folder in your project), and check for entries prefaced with [Error].</span></span>
* <span data-ttu-id="9cf24-121">Győződjön meg arról, hogy helyes-e ApplicationInsights.xml fájl sikeresen betöltötte hello Java SDK-t, ha megnézi a hello a konzol kimeneti üzenetek "konfigurációs fájl sikeresen talált" utasítás hello.</span><span class="sxs-lookup"><span data-stu-id="9cf24-121">Make sure that hello correct ApplicationInsights.xml file has been successfully loaded by hello Java SDK, by looking at hello console's output messages for a "Configuration file has been successfully found" statement.</span></span>
* <span data-ttu-id="9cf24-122">Ha hello konfigurációs fájl nem található, ellenőrizze a hello kimeneti üzenetek toosee ahol keresett hello konfigurációs fájlban, és győződjön meg arról, hogy azokat a keresési helyek egyikén található ApplicationInsights.xml hello.</span><span class="sxs-lookup"><span data-stu-id="9cf24-122">If hello config file is not found, check hello output messages toosee where hello config file is being searched for, and make sure that hello ApplicationInsights.xml is located in one of those search locations.</span></span> <span data-ttu-id="9cf24-123">A szokásos megoldás, mint közelében hello Application Insights SDK JARs elhelyezhet hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="9cf24-123">As a rule of thumb, you can place hello config file near hello Application Insights SDK JARs.</span></span> <span data-ttu-id="9cf24-124">Például: a Tomcat, ez azt jelentené hello WEB-INF/lib mappát.</span><span class="sxs-lookup"><span data-stu-id="9cf24-124">For example: in Tomcat, this would mean hello WEB-INF/lib folder.</span></span>

#### <a name="i-used-toosee-data-but-it-has-stopped"></a><span data-ttu-id="9cf24-125">Használt toosee adatokat, de leállt</span><span class="sxs-lookup"><span data-stu-id="9cf24-125">I used toosee data, but it has stopped</span></span>
* <span data-ttu-id="9cf24-126">Ellenőrizze a hello [állapot blog](http://blogs.msdn.com/b/applicationinsights-status/).</span><span class="sxs-lookup"><span data-stu-id="9cf24-126">Check hello [status blog](http://blogs.msdn.com/b/applicationinsights-status/).</span></span>
* <span data-ttu-id="9cf24-127">Elérte a havi kvótát, az adatokat?</span><span class="sxs-lookup"><span data-stu-id="9cf24-127">Have you hit your monthly quota of data points?</span></span> <span data-ttu-id="9cf24-128">Nyissa meg a beállítások/kvóta és az árazás toofind ki. Ha igen, frissítse a csomagot, vagy további kapacitást kell fizetnie.</span><span class="sxs-lookup"><span data-stu-id="9cf24-128">Open Settings/Quota and Pricing toofind out. If so, you can upgrade your plan, or pay for additional capacity.</span></span> <span data-ttu-id="9cf24-129">Lásd: hello [séma árképzési](https://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="9cf24-129">See hello [pricing scheme](https://azure.microsoft.com/pricing/details/application-insights/).</span></span>

#### <a name="i-dont-see-all-hello-data-im-expecting"></a><span data-ttu-id="9cf24-130">Nem szerepel az összes hello adatok I vagyok vár</span><span class="sxs-lookup"><span data-stu-id="9cf24-130">I don't see all hello data I'm expecting</span></span>
* <span data-ttu-id="9cf24-131">Nyissa meg a hello kvóták és a panel megnyitásához, és ellenőrizze e árképzési [mintavételi](app-insights-sampling.md) működik.</span><span class="sxs-lookup"><span data-stu-id="9cf24-131">Open hello Quotas and Pricing blade and check whether [sampling](app-insights-sampling.md) is in operation.</span></span> <span data-ttu-id="9cf24-132">(100 %-os átviteli azt jelenti, hogy a mintavételi műveletben nem.) az Application Insights szolgáltatás hello lehet set tooaccept csak töredéke alatt hello telemetriai adatot az alkalmazás nem érkezik.</span><span class="sxs-lookup"><span data-stu-id="9cf24-132">(100% transmission means that sampling isn't in operation.) hello Application Insights service can be set tooaccept only a fraction of hello telemetry that arrives from your app.</span></span> <span data-ttu-id="9cf24-133">Ez segítséget nyújt tartásához telemetriai adatot a havi kvótán belül.</span><span class="sxs-lookup"><span data-stu-id="9cf24-133">This helps you keep within your monthly quota of telemetry.</span></span> 

## <a name="no-usage-data"></a><span data-ttu-id="9cf24-134">Nincs használati adatok</span><span class="sxs-lookup"><span data-stu-id="9cf24-134">No usage data</span></span>
<span data-ttu-id="9cf24-135">**Kérelmek és válaszidejét, de nincs lapmegtekintés, böngésző vagy felhasználói adatok láthatók.**</span><span class="sxs-lookup"><span data-stu-id="9cf24-135">**I see data about requests and response times, but no page view, browser, or user data.**</span></span>

<span data-ttu-id="9cf24-136">Sikeresen beállította az alkalmazás toosend telemetriai hello kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="9cf24-136">You successfully set up your app toosend telemetry from hello server.</span></span> <span data-ttu-id="9cf24-137">Mostantól a következő lépés túl[állítsa be a weblapok toosend telemetriai hello webböngészőből][usage].</span><span class="sxs-lookup"><span data-stu-id="9cf24-137">Now your next step is too[set up your web pages toosend telemetry from hello web browser][usage].</span></span>

<span data-ttu-id="9cf24-138">Azt is megteheti Ha az ügyfél az alkalmazásban egy [telefon vagy más eszköz][platforms], ott telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="9cf24-138">Alternatively, if your client is an app in a [phone or other device][platforms], you can send telemetry from there.</span></span> 

<span data-ttu-id="9cf24-139">Használja az azonos hello instrumentation kulcs tooset fel az ügyfél- és telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="9cf24-139">Use hello same instrumentation key tooset up both your client and server telemetry.</span></span> <span data-ttu-id="9cf24-140">hello adatok megjelennek, hello azonos Application Insights-erőforrást, és képes toocorrelate események az ügyfél és kiszolgáló lesz.</span><span class="sxs-lookup"><span data-stu-id="9cf24-140">hello data will appear in hello same Application Insights resource, and you'll be able toocorrelate events from client and server.</span></span>


## <a name="disabling-telemetry"></a><span data-ttu-id="9cf24-141">Telemetria letiltása</span><span class="sxs-lookup"><span data-stu-id="9cf24-141">Disabling telemetry</span></span>
<span data-ttu-id="9cf24-142">**Hogyan letilthatja a telemetriai adatok gyűjtemény?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-142">**How can I disable telemetry collection?**</span></span>

<span data-ttu-id="9cf24-143">A kódban:</span><span class="sxs-lookup"><span data-stu-id="9cf24-143">In code:</span></span>

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

<span data-ttu-id="9cf24-144">**Vagy**</span><span class="sxs-lookup"><span data-stu-id="9cf24-144">**Or**</span></span> 

<span data-ttu-id="9cf24-145">Frissítés ApplicationInsights.xml (mappában hello erőforrások a projekt).</span><span class="sxs-lookup"><span data-stu-id="9cf24-145">Update ApplicationInsights.xml (in hello resources folder in your project).</span></span> <span data-ttu-id="9cf24-146">Adja hozzá a hello következő hello legfelső szintű csomópontja alatt:</span><span class="sxs-lookup"><span data-stu-id="9cf24-146">Add hello following under hello root node:</span></span>

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

<span data-ttu-id="9cf24-147">Hello XML metódussal rendelkezik toorestart hello alkalmazás hello érték módosításakor.</span><span class="sxs-lookup"><span data-stu-id="9cf24-147">Using hello XML method, you have toorestart hello application when you change hello value.</span></span>

## <a name="changing-hello-target"></a><span data-ttu-id="9cf24-148">Hello cél módosítása</span><span class="sxs-lookup"><span data-stu-id="9cf24-148">Changing hello target</span></span>
<span data-ttu-id="9cf24-149">**Hogyan lehet módosítani a projekt adatokat küld az Azure erőforrás?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-149">**How can I change which Azure resource my project sends data to?**</span></span>

* <span data-ttu-id="9cf24-150">[Hello instrumentation kulcs hello új erőforrás lekérése.][java]</span><span class="sxs-lookup"><span data-stu-id="9cf24-150">[Get hello instrumentation key of hello new resource.][java]</span></span>
* <span data-ttu-id="9cf24-151">Ha hozzáadta az Application Insights tooyour projekt hello Azure eszközkészlet használata az eclipse-ben, kattintson a jobb gombbal a webes projekthez, jelölje be **Azure**, **konfigurálja az Application Insights**, és módosítsa a hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="9cf24-151">If you added Application Insights tooyour project using hello Azure Toolkit for Eclipse, right click your web project, select **Azure**, **Configure Application Insights**, and change hello key.</span></span>
* <span data-ttu-id="9cf24-152">Ellenkező esetben frissíteni a hello erőforrások mappában a projekt ApplicationInsights.xml hello kulcsot.</span><span class="sxs-lookup"><span data-stu-id="9cf24-152">Otherwise, update hello key in ApplicationInsights.xml in hello resources folder in your project.</span></span>

## <a name="debug-data-from-hello-sdk"></a><span data-ttu-id="9cf24-153">Debug hello SDK adatait</span><span class="sxs-lookup"><span data-stu-id="9cf24-153">Debug data from hello SDK</span></span>

<span data-ttu-id="9cf24-154">**Hogyan tudhatom meg, milyen hello SDK végez műveletet?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-154">**How can I find out what hello SDK is doing?**</span></span>

<span data-ttu-id="9cf24-155">tooget mi történik a hello API-val kapcsolatos további információk hozzáadása `<SDKLogger/>` hello gyökércsomópont hello ApplicationInsights.xml konfigurációs fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="9cf24-155">tooget more information about what's happening in hello API, add `<SDKLogger/>` under hello root node of hello ApplicationInsights.xml configuration file.</span></span>

<span data-ttu-id="9cf24-156">Azt is beállíthatja, hogy hello naplózó toooutput tooa fájlt:</span><span class="sxs-lookup"><span data-stu-id="9cf24-156">You can also instruct hello logger toooutput tooa file:</span></span>

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

<span data-ttu-id="9cf24-157">hello fájlok alatt található `%temp%\javasdklogs` vagy `java.io.tmpdir` esetén Tomcat kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="9cf24-157">hello files can be found under `%temp%\javasdklogs` or `java.io.tmpdir` in case of Tomcat server.</span></span>


## <a name="hello-azure-start-screen"></a><span data-ttu-id="9cf24-158">hello Azure kezdőképernyőn</span><span class="sxs-lookup"><span data-stu-id="9cf24-158">hello Azure start screen</span></span>
<span data-ttu-id="9cf24-159">**Vagyok megnézi [hello Azure-portálon](https://portal.azure.com). Nem hello térkép arról, hogy valami kapcsolatos alkalmazásom?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-159">**I'm looking at [hello Azure portal](https://portal.azure.com). Does hello map tell me something about my app?**</span></span>

<span data-ttu-id="9cf24-160">Azure-kiszolgálók állapotának hello nem, hello világ mutatja.</span><span class="sxs-lookup"><span data-stu-id="9cf24-160">No, it shows hello health of Azure servers around hello world.</span></span>

<span data-ttu-id="9cf24-161">*Hello Azure start tábláról (kezdőképernyő) hogyan található adatok kapcsolatos alkalmazásom?*</span><span class="sxs-lookup"><span data-stu-id="9cf24-161">*From hello Azure start board (home screen), how do I find data about my app?*</span></span>

<span data-ttu-id="9cf24-162">Feltéve, hogy [az alkalmazás beállítása az Application Insights][java], kattintson a Tallózás gombra, válassza ki az Application Insights és válassza ki az alkalmazáshoz létrehozott hello alkalmazás-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9cf24-162">Assuming you [set up your app for Application Insights][java], click Browse, select Application Insights, and select hello app resource you created for your app.</span></span> <span data-ttu-id="9cf24-163">tooget nincs gyorsabb a későbbiekben rögzíthető az alkalmazás toohello start tábla.</span><span class="sxs-lookup"><span data-stu-id="9cf24-163">tooget there faster in future, you can pin your app toohello start board.</span></span>

## <a name="intranet-servers"></a><span data-ttu-id="9cf24-164">Intranetes kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="9cf24-164">Intranet servers</span></span>
<span data-ttu-id="9cf24-165">**Képes a kiszolgáló is figyelése intranetről?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-165">**Can I monitor a server on my intranet?**</span></span>

<span data-ttu-id="9cf24-166">Igen, a kiszolgáló elküldheti telemetriai toohello Application Insights portálon keresztül megadott hello a nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="9cf24-166">Yes, provided your server can send telemetry toohello Application Insights portal through hello public internet.</span></span> 

<span data-ttu-id="9cf24-167">A tűzfal lehetséges, hogy 80-as és 443-as kimenő forgalom toodc.services.visualstudio.com és f5.services.visualstudio.com tooopen TCP-portok.</span><span class="sxs-lookup"><span data-stu-id="9cf24-167">In your firewall, you might have tooopen TCP ports 80 and 443 for outgoing traffic toodc.services.visualstudio.com and f5.services.visualstudio.com.</span></span>

## <a name="data-retention"></a><span data-ttu-id="9cf24-168">Adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="9cf24-168">Data retention</span></span>
<span data-ttu-id="9cf24-169">**Mennyi ideig adatok őrződnek meg hello portálon? Az biztonságos?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-169">**How long is data retained in hello portal? Is it secure?**</span></span>

<span data-ttu-id="9cf24-170">Lásd: [az adatmegőrzés és az adatvédelmi][data].</span><span class="sxs-lookup"><span data-stu-id="9cf24-170">See [Data retention and privacy][data].</span></span>

## <a name="next-steps"></a><span data-ttu-id="9cf24-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9cf24-171">Next steps</span></span>
<span data-ttu-id="9cf24-172">**Az Application Insights saját Java server alkalmazás beállítása I. Mit tehetek?**</span><span class="sxs-lookup"><span data-stu-id="9cf24-172">**I set up Application Insights for my Java server app. What else can I do?**</span></span>

* <span data-ttu-id="9cf24-173">[A weblapok rendelkezésre állásának figyelése][availability]</span><span class="sxs-lookup"><span data-stu-id="9cf24-173">[Monitor availability of your web pages][availability]</span></span>
* <span data-ttu-id="9cf24-174">[Weblap megfigyeléséhez][usage]</span><span class="sxs-lookup"><span data-stu-id="9cf24-174">[Monitor web page usage][usage]</span></span>
* <span data-ttu-id="9cf24-175">[Használat követése és az eszköz alkalmazások problémák diagnosztizálása][platforms]</span><span class="sxs-lookup"><span data-stu-id="9cf24-175">[Track usage and diagnose issues in your device apps][platforms]</span></span>
* <span data-ttu-id="9cf24-176">[Az alkalmazás tootrack kódhasználat írása][track]</span><span class="sxs-lookup"><span data-stu-id="9cf24-176">[Write code tootrack usage of your app][track]</span></span>
* <span data-ttu-id="9cf24-177">[Diagnosztikai naplók rögzítése][javalogs]</span><span class="sxs-lookup"><span data-stu-id="9cf24-177">[Capture diagnostic logs][javalogs]</span></span>

## <a name="get-help"></a><span data-ttu-id="9cf24-178">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="9cf24-178">Get help</span></span>
* [<span data-ttu-id="9cf24-179">Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="9cf24-179">Stack Overflow</span></span>](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

