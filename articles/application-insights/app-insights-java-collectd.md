---
title: "Linux - Azure Java web app teljesítményére aaaMonitor |} Microsoft Docs"
description: "Bővített alkalmazásteljesítmény-figyelés a Java-webhely hello CollectD beépülő modul az Application insights szolgáltatással."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: bwren
ms.openlocfilehash: f783e8607a83b2b43f67d3a2fc20f100aa2f75ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a><span data-ttu-id="714bf-103">collectd: Linux teljesítménymutatók az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="714bf-103">collectd: Linux performance metrics in Application Insights</span></span>


<span data-ttu-id="714bf-104">tooexplore Linux rendszer metrikák a [Application Insights](app-insights-overview.md), telepítse [collectd](http://collectd.org/), valamint azokat az Application Insights beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="714bf-104">tooexplore Linux system performance metrics in [Application Insights](app-insights-overview.md), install [collectd](http://collectd.org/), together with its Application Insights plug-in.</span></span> <span data-ttu-id="714bf-105">A nyílt forráskódú megoldás különböző rendszer és a hálózati statisztikákat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="714bf-105">This open-source solution gathers various system and network statistics.</span></span>

<span data-ttu-id="714bf-106">Általában fogja használni collectd, ha már rendelkezik [a Java webes szolgáltatás az Application insights szolgáltatással tagolva][java].</span><span class="sxs-lookup"><span data-stu-id="714bf-106">Typically you'll use collectd if you have already [instrumented your Java web service with Application Insights][java].</span></span> <span data-ttu-id="714bf-107">Biztosít további adatok toohelp meg tooenhance az alkalmazás teljesítmény vagy a problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="714bf-107">It gives you more data toohelp you tooenhance your app's performance or diagnose problems.</span></span> 

![a minta diagramok](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a><span data-ttu-id="714bf-109">A rendszerállapot-kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="714bf-109">Get your instrumentation key</span></span>
<span data-ttu-id="714bf-110">A hello [Microsoft Azure-portálon](https://portal.azure.com), nyissa meg hello [Application Insights](app-insights-overview.md) hello adatok tooappear kívánt erőforrás.</span><span class="sxs-lookup"><span data-stu-id="714bf-110">In hello [Microsoft Azure portal](https://portal.azure.com), open hello [Application Insights](app-insights-overview.md) resource where you want hello data tooappear.</span></span> <span data-ttu-id="714bf-111">(Vagy [hozzon létre egy új erőforrást](app-insights-create-new-resource.md).)</span><span class="sxs-lookup"><span data-stu-id="714bf-111">(Or [create a new resource](app-insights-create-new-resource.md).)</span></span>

<span data-ttu-id="714bf-112">Tegye meg a hello erőforrás azonosítja hello instrumentation kulcs másolatával.</span><span class="sxs-lookup"><span data-stu-id="714bf-112">Take a copy of hello instrumentation key, which identifies hello resource.</span></span>

![Keresse meg az összes, nyissa meg az erőforrás és majd a hello Essentials legördülő listán, válassza ki, és másolja hello Instrumentation kulcs](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-hello-plug-in"></a><span data-ttu-id="714bf-114">Collectd és hello beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="714bf-114">Install collectd and hello plug-in</span></span>
<span data-ttu-id="714bf-115">A Linux server gépeken:</span><span class="sxs-lookup"><span data-stu-id="714bf-115">On your Linux server machines:</span></span>

1. <span data-ttu-id="714bf-116">Telepítés [collectd](http://collectd.org/) 5.4.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="714bf-116">Install [collectd](http://collectd.org/) version 5.4.0 or later.</span></span>
2. <span data-ttu-id="714bf-117">Töltse le a hello [Application Insights collectd író beépülő modul](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="714bf-117">Download hello [Application Insights collectd writer plugin](https://aka.ms/aijavasdk).</span></span> <span data-ttu-id="714bf-118">Vegye figyelembe a hello verziószáma.</span><span class="sxs-lookup"><span data-stu-id="714bf-118">Note hello version number.</span></span>
3. <span data-ttu-id="714bf-119">Hello beépülő modul JAR másolja be `/usr/share/collectd/java`.</span><span class="sxs-lookup"><span data-stu-id="714bf-119">Copy hello plugin JAR into `/usr/share/collectd/java`.</span></span>
4. <span data-ttu-id="714bf-120">Szerkesztés `/etc/collectd/collectd.conf`:</span><span class="sxs-lookup"><span data-stu-id="714bf-120">Edit `/etc/collectd/collectd.conf`:</span></span>
   * <span data-ttu-id="714bf-121">Győződjön meg arról, hogy [Java beépülő modul hello](https://collectd.org/wiki/index.php/Plugin:Java) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="714bf-121">Ensure that [hello Java plugin](https://collectd.org/wiki/index.php/Plugin:Java) is enabled.</span></span>
   * <span data-ttu-id="714bf-122">Frissítés hello JVMArg hello java.class.path tooinclude hello JAR követően.</span><span class="sxs-lookup"><span data-stu-id="714bf-122">Update hello JVMArg for hello java.class.path tooinclude hello following JAR.</span></span> <span data-ttu-id="714bf-123">Frissítés hello verzió száma toomatch hello egy letöltött:</span><span class="sxs-lookup"><span data-stu-id="714bf-123">Update hello version number toomatch hello one you downloaded:</span></span>
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * <span data-ttu-id="714bf-124">Adja hozzá a kódrészletet, az erőforrás Instrumentation kulcs hello használata:</span><span class="sxs-lookup"><span data-stu-id="714bf-124">Add this snippet, using hello Instrumentation Key from your resource:</span></span>

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

<span data-ttu-id="714bf-125">Íme egy példa konfigurációs fájl részeként:</span><span class="sxs-lookup"><span data-stu-id="714bf-125">Here's part of a sample configuration file:</span></span>

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

<span data-ttu-id="714bf-126">Konfigurálja az egyéb [collectd beépülő modulok](https://collectd.org/wiki/index.php/Table_of_Plugins), amelyek különböző adatokat gyűjthet különböző forrásokból származó.</span><span class="sxs-lookup"><span data-stu-id="714bf-126">Configure other [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), which can collect various data from different sources.</span></span>

<span data-ttu-id="714bf-127">Indítsa újra a megfelelő tooits collectd [manuális](https://collectd.org/wiki/index.php/First_steps).</span><span class="sxs-lookup"><span data-stu-id="714bf-127">Restart collectd according tooits [manual](https://collectd.org/wiki/index.php/First_steps).</span></span>

## <a name="view-hello-data-in-application-insights"></a><span data-ttu-id="714bf-128">Az Application Insightsban hello adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="714bf-128">View hello data in Application Insights</span></span>
<span data-ttu-id="714bf-129">Nyissa meg az Application Insights-erőforrás [Metrikaböngésző, és adja hozzá a diagramok][metrics], kívánt hello metrikák kiválasztásával hello egyéni kategóriából toosee.</span><span class="sxs-lookup"><span data-stu-id="714bf-129">In your Application Insights resource, open [Metrics Explorer and add charts][metrics], selecting hello metrics you want toosee from hello Custom category.</span></span>

![](./media/app-insights-java-collectd/result.png)

<span data-ttu-id="714bf-130">Alapértelmezés szerint hello metrikák összesítése, amelyből hello metrikák gyűjtött összes állomás számítógépével.</span><span class="sxs-lookup"><span data-stu-id="714bf-130">By default, hello metrics are aggregated across all host machines from which hello metrics were collected.</span></span> <span data-ttu-id="714bf-131">tooview hello metrikák állomásonként hello diagram részletei panelen kapcsolja be a csoportosítás, és válassza a toogroup CollectD-állomás.</span><span class="sxs-lookup"><span data-stu-id="714bf-131">tooview hello metrics per host, in hello Chart details blade, turn on Grouping and then choose toogroup by CollectD-Host.</span></span>

## <a name="tooexclude-upload-of-specific-statistics"></a><span data-ttu-id="714bf-132">az adott statisztika tooexclude feltöltése a</span><span class="sxs-lookup"><span data-stu-id="714bf-132">tooexclude upload of specific statistics</span></span>
<span data-ttu-id="714bf-133">Alapértelmezés szerint a hello Application Insights beépülő modult minden engedélyezett hello collectd által gyűjtött összes hello adatok "read" beépülő modulok küld.</span><span class="sxs-lookup"><span data-stu-id="714bf-133">By default, hello Application Insights plugin sends all hello data collected by all hello enabled collectd 'read' plugins.</span></span> 

<span data-ttu-id="714bf-134">adott beépülő modulok, illetve az adatforrások tooexclude adatait:</span><span class="sxs-lookup"><span data-stu-id="714bf-134">tooexclude data from specific plugins or data sources:</span></span>

* <span data-ttu-id="714bf-135">Szerkessze a hello konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="714bf-135">Edit hello configuration file.</span></span> 
* <span data-ttu-id="714bf-136">A `<Plugin ApplicationInsightsWriter>`, adja hozzá a direktíva sorokat:</span><span class="sxs-lookup"><span data-stu-id="714bf-136">In `<Plugin ApplicationInsightsWriter>`, add directive lines like this:</span></span>

| <span data-ttu-id="714bf-137">Irányelv</span><span class="sxs-lookup"><span data-stu-id="714bf-137">Directive</span></span> | <span data-ttu-id="714bf-138">Következmény</span><span class="sxs-lookup"><span data-stu-id="714bf-138">Effect</span></span> |
| --- | --- |
| `Exclude disk` |<span data-ttu-id="714bf-139">Hello által gyűjtött összes adat kizárása `disk` beépülő modul</span><span class="sxs-lookup"><span data-stu-id="714bf-139">Exclude all data collected by hello `disk` plugin</span></span> |
| `Exclude disk:read,write` |<span data-ttu-id="714bf-140">Nevű kizárása hello `read` és `write` a hello `disk` beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="714bf-140">Exclude hello sources named `read` and `write` from hello `disk` plugin.</span></span> |

<span data-ttu-id="714bf-141">Külön irányelvek és egy új sor.</span><span class="sxs-lookup"><span data-stu-id="714bf-141">Separate directives with a newline.</span></span>

## <a name="problems"></a><span data-ttu-id="714bf-142">Problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="714bf-142">Problems?</span></span>
<span data-ttu-id="714bf-143">*Nem szerepel az adatok hello portálon*</span><span class="sxs-lookup"><span data-stu-id="714bf-143">*I don't see data in hello portal*</span></span>

* <span data-ttu-id="714bf-144">Nyissa meg [keresési] [ diagnostic] toosee Ha hello nyers események érkezett.</span><span class="sxs-lookup"><span data-stu-id="714bf-144">Open [Search][diagnostic] toosee if hello raw events have arrived.</span></span> <span data-ttu-id="714bf-145">Hosszabb tooappear néha a metrikaböngészőben tartanak.</span><span class="sxs-lookup"><span data-stu-id="714bf-145">Sometimes they take longer tooappear in metrics explorer.</span></span>
* <span data-ttu-id="714bf-146">Szükség lehet túl[tűzfalkivételeket a kimenő adatok beállítása](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="714bf-146">You might need too[set firewall exceptions for outgoing data](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="714bf-147">Nyomkövetés engedélyezése a hello Application Insights beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="714bf-147">Enable tracing in hello Application Insights plugin.</span></span> <span data-ttu-id="714bf-148">Adja hozzá a sort belül `<Plugin ApplicationInsightsWriter>`:</span><span class="sxs-lookup"><span data-stu-id="714bf-148">Add this line within `<Plugin ApplicationInsightsWriter>`:</span></span>
  * `SDKLogger true`
* <span data-ttu-id="714bf-149">Nyisson meg egy terminált, és collectd részletes üzemmódban toosee indítsa el a probléma merül fel jelentések:</span><span class="sxs-lookup"><span data-stu-id="714bf-149">Open a terminal and start collectd in verbose mode, toosee any issues it is reporting:</span></span>
  * `sudo collectd -f`

## <a name="known-issue"></a><span data-ttu-id="714bf-150">Ismert hiba</span><span class="sxs-lookup"><span data-stu-id="714bf-150">Known issue</span></span>

<span data-ttu-id="714bf-151">hello Application Insights írási beépülő modul nem kompatibilis a bizonyos olvasási beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="714bf-151">hello Application Insights Write plugin is incompatible with certain Read plugins.</span></span> <span data-ttu-id="714bf-152">Néhány beépülő modulok néha küldése "NaN" ahol hello Application Insights beépülő modul egy lebegőpontos számot vár.</span><span class="sxs-lookup"><span data-stu-id="714bf-152">Some plugins sometimes send "NaN" where hello Application Insights plugin expects a floating-point number.</span></span>

<span data-ttu-id="714bf-153">Jelenség: hello collectd napló mutatja, amely tartalmazza az "Eszközintelligencia:... SyntaxError: váratlan lexikális elem N ".</span><span class="sxs-lookup"><span data-stu-id="714bf-153">Symptom: hello collectd log shows errors that include "AI: ... SyntaxError: Unexpected token N".</span></span>

<span data-ttu-id="714bf-154">Megkerülő megoldás: Hello probléma írási beépülő modulok által gyűjtött adatok kihagyása.</span><span class="sxs-lookup"><span data-stu-id="714bf-154">Workaround: Exclude data collected by hello problem Write plugins.</span></span> 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


