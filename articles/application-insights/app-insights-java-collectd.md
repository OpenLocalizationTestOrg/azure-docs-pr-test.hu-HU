---
title: "Linux - Azure Java web app teljesítményére figyelése |} Microsoft Docs"
description: "Bővített alkalmazásteljesítmény-figyelés a Java-webhely beépülő modul CollectD az Application insights szolgáltatással."
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
ms.openlocfilehash: 4ea917b068e0242bfb88d7357eca032607a43a3f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a><span data-ttu-id="d1034-103">collectd: Linux teljesítménymutatók az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="d1034-103">collectd: Linux performance metrics in Application Insights</span></span>


<span data-ttu-id="d1034-104">Linux rendszer teljesítménymutatók a felfedezése [Application Insights](app-insights-overview.md), telepítse [collectd](http://collectd.org/)együtt az Application Insights beépülő modult,.</span><span class="sxs-lookup"><span data-stu-id="d1034-104">To explore Linux system performance metrics in [Application Insights](app-insights-overview.md), install [collectd](http://collectd.org/), together with its Application Insights plug-in.</span></span> <span data-ttu-id="d1034-105">A nyílt forráskódú megoldás különböző rendszer és a hálózati statisztikákat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="d1034-105">This open-source solution gathers various system and network statistics.</span></span>

<span data-ttu-id="d1034-106">Általában fogja használni collectd, ha már rendelkezik [a Java webes szolgáltatás az Application insights szolgáltatással tagolva][java].</span><span class="sxs-lookup"><span data-stu-id="d1034-106">Typically you'll use collectd if you have already [instrumented your Java web service with Application Insights][java].</span></span> <span data-ttu-id="d1034-107">Ez lehetővé teszi több adat segítséget az alkalmazás a teljesítmény növelése és a problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="d1034-107">It gives you more data to help you to enhance your app's performance or diagnose problems.</span></span> 

![a minta diagramok](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a><span data-ttu-id="d1034-109">A rendszerállapot-kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="d1034-109">Get your instrumentation key</span></span>
<span data-ttu-id="d1034-110">Az a [Microsoft Azure-portálon](https://portal.azure.com), nyissa meg a [Application Insights](app-insights-overview.md) erőforrás, ahol azt szeretné, hogy az adatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d1034-110">In the [Microsoft Azure portal](https://portal.azure.com), open the [Application Insights](app-insights-overview.md) resource where you want the data to appear.</span></span> <span data-ttu-id="d1034-111">(Vagy [hozzon létre egy új erőforrást](app-insights-create-new-resource.md).)</span><span class="sxs-lookup"><span data-stu-id="d1034-111">(Or [create a new resource](app-insights-create-new-resource.md).)</span></span>

<span data-ttu-id="d1034-112">Igénybe vehet a instrumentation kulcs, amely azonosítja az erőforrás egy példányát.</span><span class="sxs-lookup"><span data-stu-id="d1034-112">Take a copy of the instrumentation key, which identifies the resource.</span></span>

![Keresse meg az összes, nyissa meg az erőforrást, majd az Essentials legördülő válassza ki, és a Instrumentation kulcs másolása](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a><span data-ttu-id="d1034-114">Collectd és a beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="d1034-114">Install collectd and the plug-in</span></span>
<span data-ttu-id="d1034-115">A Linux server gépeken:</span><span class="sxs-lookup"><span data-stu-id="d1034-115">On your Linux server machines:</span></span>

1. <span data-ttu-id="d1034-116">Telepítés [collectd](http://collectd.org/) 5.4.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d1034-116">Install [collectd](http://collectd.org/) version 5.4.0 or later.</span></span>
2. <span data-ttu-id="d1034-117">Töltse le a [Application Insights collectd író beépülő modul](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="d1034-117">Download the [Application Insights collectd writer plugin](https://aka.ms/aijavasdk).</span></span> <span data-ttu-id="d1034-118">Megjegyzés: a verziószám.</span><span class="sxs-lookup"><span data-stu-id="d1034-118">Note the version number.</span></span>
3. <span data-ttu-id="d1034-119">A beépülő modul JAR történő másolás `/usr/share/collectd/java`.</span><span class="sxs-lookup"><span data-stu-id="d1034-119">Copy the plugin JAR into `/usr/share/collectd/java`.</span></span>
4. <span data-ttu-id="d1034-120">Szerkesztés `/etc/collectd/collectd.conf`:</span><span class="sxs-lookup"><span data-stu-id="d1034-120">Edit `/etc/collectd/collectd.conf`:</span></span>
   * <span data-ttu-id="d1034-121">Győződjön meg arról, hogy [a Java beépülő modul](https://collectd.org/wiki/index.php/Plugin:Java) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d1034-121">Ensure that [the Java plugin](https://collectd.org/wiki/index.php/Plugin:Java) is enabled.</span></span>
   * <span data-ttu-id="d1034-122">Frissítse a JVMArg a java.class.path számára a következő JAR felvenni.</span><span class="sxs-lookup"><span data-stu-id="d1034-122">Update the JVMArg for the java.class.path to include the following JAR.</span></span> <span data-ttu-id="d1034-123">A letöltött egyeznie a verziószám frissítéséhez:</span><span class="sxs-lookup"><span data-stu-id="d1034-123">Update the version number to match the one you downloaded:</span></span>
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * <span data-ttu-id="d1034-124">Adja hozzá ezt a kódrészletet, az erőforrás Instrumentation kulccsal:</span><span class="sxs-lookup"><span data-stu-id="d1034-124">Add this snippet, using the Instrumentation Key from your resource:</span></span>

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

<span data-ttu-id="d1034-125">Íme egy példa konfigurációs fájl részeként:</span><span class="sxs-lookup"><span data-stu-id="d1034-125">Here's part of a sample configuration file:</span></span>

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

<span data-ttu-id="d1034-126">Konfigurálja az egyéb [collectd beépülő modulok](https://collectd.org/wiki/index.php/Table_of_Plugins), amelyek különböző adatokat gyűjthet különböző forrásokból származó.</span><span class="sxs-lookup"><span data-stu-id="d1034-126">Configure other [collectd plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), which can collect various data from different sources.</span></span>

<span data-ttu-id="d1034-127">Indítsa újra a következők szerint collectd a [manuális](https://collectd.org/wiki/index.php/First_steps).</span><span class="sxs-lookup"><span data-stu-id="d1034-127">Restart collectd according to its [manual](https://collectd.org/wiki/index.php/First_steps).</span></span>

## <a name="view-the-data-in-application-insights"></a><span data-ttu-id="d1034-128">Az adatok megtekintése az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="d1034-128">View the data in Application Insights</span></span>
<span data-ttu-id="d1034-129">Nyissa meg az Application Insights-erőforrás [Metrikaböngésző, és adja hozzá a diagramok][metrics], a metrikák meg szeretné tekinteni a egyéni kategóriából kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="d1034-129">In your Application Insights resource, open [Metrics Explorer and add charts][metrics], selecting the metrics you want to see from the Custom category.</span></span>

![](./media/app-insights-java-collectd/result.png)

<span data-ttu-id="d1034-130">Alapértelmezés szerint a metrikák összesítése, amelyből a metrikák gyűjtött összes állomás számítógépével.</span><span class="sxs-lookup"><span data-stu-id="d1034-130">By default, the metrics are aggregated across all host machines from which the metrics were collected.</span></span> <span data-ttu-id="d1034-131">A metrikák állomásonként, a diagram részletei panelen megtekintéséhez kapcsolja be a csoportosítás, és válassza a CollectD-állomás szerint kell csoportosítani.</span><span class="sxs-lookup"><span data-stu-id="d1034-131">To view the metrics per host, in the Chart details blade, turn on Grouping and then choose to group by CollectD-Host.</span></span>

## <a name="to-exclude-upload-of-specific-statistics"></a><span data-ttu-id="d1034-132">Az adott statisztika feltöltése kizárása</span><span class="sxs-lookup"><span data-stu-id="d1034-132">To exclude upload of specific statistics</span></span>
<span data-ttu-id="d1034-133">Alapértelmezés szerint az Application Insights beépülő modul elküldi az engedélyezett collectd "read" beépülő modulok által gyűjtött összes adat.</span><span class="sxs-lookup"><span data-stu-id="d1034-133">By default, the Application Insights plugin sends all the data collected by all the enabled collectd 'read' plugins.</span></span> 

<span data-ttu-id="d1034-134">Adatok kizárandó konkrét beépülő modulok, illetve az adatforrások:</span><span class="sxs-lookup"><span data-stu-id="d1034-134">To exclude data from specific plugins or data sources:</span></span>

* <span data-ttu-id="d1034-135">Szerkessze a konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="d1034-135">Edit the configuration file.</span></span> 
* <span data-ttu-id="d1034-136">A `<Plugin ApplicationInsightsWriter>`, adja hozzá a direktíva sorokat:</span><span class="sxs-lookup"><span data-stu-id="d1034-136">In `<Plugin ApplicationInsightsWriter>`, add directive lines like this:</span></span>

| <span data-ttu-id="d1034-137">Irányelv</span><span class="sxs-lookup"><span data-stu-id="d1034-137">Directive</span></span> | <span data-ttu-id="d1034-138">Következmény</span><span class="sxs-lookup"><span data-stu-id="d1034-138">Effect</span></span> |
| --- | --- |
| `Exclude disk` |<span data-ttu-id="d1034-139">Zárja ki által gyűjtött összes adat a `disk` beépülő modul</span><span class="sxs-lookup"><span data-stu-id="d1034-139">Exclude all data collected by the `disk` plugin</span></span> |
| `Exclude disk:read,write` |<span data-ttu-id="d1034-140">Kizárása a nevű `read` és `write` a a `disk` beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="d1034-140">Exclude the sources named `read` and `write` from the `disk` plugin.</span></span> |

<span data-ttu-id="d1034-141">Külön irányelvek és egy új sor.</span><span class="sxs-lookup"><span data-stu-id="d1034-141">Separate directives with a newline.</span></span>

## <a name="problems"></a><span data-ttu-id="d1034-142">Problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="d1034-142">Problems?</span></span>
<span data-ttu-id="d1034-143">*Nem szerepel az adatok a portálon*</span><span class="sxs-lookup"><span data-stu-id="d1034-143">*I don't see data in the portal*</span></span>

* <span data-ttu-id="d1034-144">Nyissa meg [keresési] [ diagnostic] érkezett-e a nyers események megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d1034-144">Open [Search][diagnostic] to see if the raw events have arrived.</span></span> <span data-ttu-id="d1034-145">Egyes esetekben tovább tartanak metrikaböngésző jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d1034-145">Sometimes they take longer to appear in metrics explorer.</span></span>
* <span data-ttu-id="d1034-146">Szükség lehet [tűzfalkivételeket a kimenő adatok beállítása](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="d1034-146">You might need to [set firewall exceptions for outgoing data](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="d1034-147">Nyomkövetés engedélyezése a az Application Insights beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="d1034-147">Enable tracing in the Application Insights plugin.</span></span> <span data-ttu-id="d1034-148">Adja hozzá a sort belül `<Plugin ApplicationInsightsWriter>`:</span><span class="sxs-lookup"><span data-stu-id="d1034-148">Add this line within `<Plugin ApplicationInsightsWriter>`:</span></span>
  * `SDKLogger true`
* <span data-ttu-id="d1034-149">Nyissa meg egy terminált, és indítsa el a collectd a kapcsoló ütközik a jelentések megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="d1034-149">Open a terminal and start collectd in verbose mode, to see any issues it is reporting:</span></span>
  * `sudo collectd -f`

## <a name="known-issue"></a><span data-ttu-id="d1034-150">Ismert hiba</span><span class="sxs-lookup"><span data-stu-id="d1034-150">Known issue</span></span>

<span data-ttu-id="d1034-151">Az Application Insights írási beépülő modul nem kompatibilis a bizonyos olvasási beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="d1034-151">The Application Insights Write plugin is incompatible with certain Read plugins.</span></span> <span data-ttu-id="d1034-152">Néhány beépülő modulok néha küldése "NaN", ahol az Application Insights beépülő modul egy lebegőpontos számot vár.</span><span class="sxs-lookup"><span data-stu-id="d1034-152">Some plugins sometimes send "NaN" where the Application Insights plugin expects a floating-point number.</span></span>

<span data-ttu-id="d1034-153">Jelenség: A collectd napló mutatja, amely tartalmazza az "Eszközintelligencia:... SyntaxError: váratlan lexikális elem N ".</span><span class="sxs-lookup"><span data-stu-id="d1034-153">Symptom: The collectd log shows errors that include "AI: ... SyntaxError: Unexpected token N".</span></span>

<span data-ttu-id="d1034-154">Megkerülő megoldás: A probléma írási beépülő modulok által gyűjtött adatok kihagyása.</span><span class="sxs-lookup"><span data-stu-id="d1034-154">Workaround: Exclude data collected by the problem Write plugins.</span></span> 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


