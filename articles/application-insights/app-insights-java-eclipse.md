---
title: "aaaGet Azure Application Insights az eclipse-ben Java használatába |} Microsoft docs"
description: "Hello Eclipse beépülő modul tooadd teljesítmény és a használat figyelés tooyour Java webhely az Application insights szolgáltatással"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: 3142a26a9e2d14c2c433882e3d337f2a8c8f2247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a><span data-ttu-id="2aa5a-103">Ismerkedés az Application Insights Java az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="2aa5a-103">Get started with Application Insights with Java in Eclipse</span></span>
<span data-ttu-id="2aa5a-104">hello Application Insights SDK, hogy elemezheti a használati és teljesítményadatokat küld a Java-webalkalmazások telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-104">hello Application Insights SDK sends telemetry from your Java web application so that you can analyze usage and performance.</span></span> <span data-ttu-id="2aa5a-105">hello eclipse-ben az Application Insights beépülő modult úgy, hogy be hello mezőben telemetriai adatokat, valamint az API-k használható toowrite egyéni telemetria kívül automatikusan telepíti hello SDK a projektben.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-105">hello Eclipse plug-in for Application Insights automatically installs hello SDK in your project so that you get out of hello box telemetry, plus an API that you can use toowrite custom telemetry.</span></span>   

## <a name="prerequisites"></a><span data-ttu-id="2aa5a-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2aa5a-106">Prerequisites</span></span>
<span data-ttu-id="2aa5a-107">Hello beépülő modul jelenleg működik Maven-projektek és dinamikus webes projektek az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-107">Currently hello plug-in works for Maven projects and Dynamic Web Projects in Eclipse.</span></span>
<span data-ttu-id="2aa5a-108">([Java-projektet az Application Insights hozzáadása tooother típusú][java].)</span><span class="sxs-lookup"><span data-stu-id="2aa5a-108">([Add Application Insights tooother types of Java project][java].)</span></span>

<span data-ttu-id="2aa5a-109">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-109">You'll need:</span></span>

* <span data-ttu-id="2aa5a-110">Oracle JRE 1.6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="2aa5a-110">Oracle JRE 1.6 or later</span></span>
* <span data-ttu-id="2aa5a-111">Előfizetés túl[Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="2aa5a-111">A subscription too[Microsoft Azure](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="2aa5a-112">[Java EE-fejlesztőknek IDE Holdas](http://www.eclipse.org/downloads/), Indigo vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-112">[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/), Indigo or later.</span></span>
* <span data-ttu-id="2aa5a-113">Windows 7 vagy újabb, vagy a Windows Server 2008 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="2aa5a-113">Windows 7 or later, or Windows Server 2008 or later</span></span>

## <a name="install-hello-sdk-on-eclipse-one-time"></a><span data-ttu-id="2aa5a-114">Az eclipse-ben (egyszer) hello SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="2aa5a-114">Install hello SDK on Eclipse (one time)</span></span>
<span data-ttu-id="2aa5a-115">Csak akkor kell toodo gépenként egyetlen most.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-115">You only have toodo this one time per machine.</span></span> <span data-ttu-id="2aa5a-116">Ez a lépés telepíti egy eszközkészlet, amelyet ezután felvehet hello SDK tooeach dinamikus webes projekt.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-116">This step installs a toolkit which can then add hello SDK tooeach Dynamic Web Project.</span></span>

1. <span data-ttu-id="2aa5a-117">Az eclipse-ben kattintson a Súgó, új szoftverek telepítése.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-117">In Eclipse, click Help, Install New Software.</span></span>

    ![Súgó, új szoftverek telepítése](./media/app-insights-java-eclipse/0-plugin.png)
2. <span data-ttu-id="2aa5a-119">hello SDK http://dl.microsoft.com/eclipse Azure eszközkészlet alatt van.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-119">hello SDK is in http://dl.microsoft.com/eclipse, under Azure Toolkit.</span></span>
3. <span data-ttu-id="2aa5a-120">Törölje a jelet **lépjen kapcsolatba az összes frissítés hely...**</span><span class="sxs-lookup"><span data-stu-id="2aa5a-120">Uncheck **Contact all update sites...**</span></span>

    ![Application Insights SDK törölje lépjen kapcsolatba az összes frissítés helyek](./media/app-insights-java-eclipse/1-plugin.png)

<span data-ttu-id="2aa5a-122">Kövesse a hátralévő lépéseket minden olyan projekthez Java hello.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-122">Follow hello remaining steps for each Java project.</span></span>

## <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="2aa5a-123">Az Application Insights-erőforrás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="2aa5a-123">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="2aa5a-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2aa5a-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2aa5a-125">Hozzon létre egy új Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-125">Create a new Application Insights resource.</span></span> <span data-ttu-id="2aa5a-126">Állítsa be a hello alkalmazás típusú tooJava webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-126">Set hello application type tooJava web application.</span></span>  

    ![Kattintson a + gombra, és válassza az Application Insights lehetőséget.](./media/app-insights-java-eclipse/01-create.png)  

4. <span data-ttu-id="2aa5a-128">Hello instrumentation kulcs hello új erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-128">Find hello instrumentation key of hello new resource.</span></span> <span data-ttu-id="2aa5a-129">Szüksége lesz toopaste Ez a kód projektben hamarosan.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-129">You'll need toopaste this into your code project shortly.</span></span>  

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-tooyour-project"></a><span data-ttu-id="2aa5a-131">Az Application Insights tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2aa5a-131">Add Application Insights tooyour project</span></span>
1. <span data-ttu-id="2aa5a-132">Az Application Insights hozzáadása a Java webes projekt hello helyi menüből.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-132">Add Application Insights from hello context menu of your Java web project.</span></span>

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-eclipse/02-context-menu.png)
2. <span data-ttu-id="2aa5a-134">Beillesztés hello instrumentation kulcs portáltól kapott hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-134">Paste hello instrumentation key that you got from hello Azure portal.</span></span>

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-eclipse/03-ikey.png)

<span data-ttu-id="2aa5a-136">minden telemetriai tétel együtt küldött hello kulcs, és közli az Application Insights toodisplay legyen az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-136">hello key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>

## <a name="run-hello-application-and-see-metrics"></a><span data-ttu-id="2aa5a-137">Hello alkalmazás futtatásához, és tekintse meg a metrikák</span><span class="sxs-lookup"><span data-stu-id="2aa5a-137">Run hello application and see metrics</span></span>
<span data-ttu-id="2aa5a-138">Futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-138">Run your application.</span></span>

<span data-ttu-id="2aa5a-139">A Microsoft Azure Application Insights-erőforrás tooyour adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-139">Return tooyour Application Insights resource in Microsoft Azure.</span></span>

<span data-ttu-id="2aa5a-140">HTTP-kérelmek adatai hello áttekintése panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-140">HTTP requests data will appear on hello overview blade.</span></span> <span data-ttu-id="2aa5a-141">(Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)</span><span class="sxs-lookup"><span data-stu-id="2aa5a-141">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![<span data-ttu-id="2aa5a-142">Kiszolgáló válasza, az ügyfélkérelmek számát és a hibák</span><span class="sxs-lookup"><span data-stu-id="2aa5a-142">Server response, request counts, and failures</span></span> ](./media/app-insights-java-eclipse/5-results.png)

<span data-ttu-id="2aa5a-143">Kattintson a diagram toosee keresztül metrikák részletes.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-143">Click through any chart toosee more detailed metrics.</span></span>

![A kérelmek számát név szerint](./media/app-insights-java-eclipse/6-barchart.png)

<span data-ttu-id="2aa5a-145">[További információk a metrikákról.][metrics]</span><span class="sxs-lookup"><span data-stu-id="2aa5a-145">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="2aa5a-146">És egy kérelem hello tulajdonságainak megtekintésekor láthatja hello telemetriai események például a kérelmek és kivételek társítva.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-146">And when viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![A kérelemhez tartozó összes adat](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a><span data-ttu-id="2aa5a-148">Ügyféloldali telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="2aa5a-148">Client-side telemetry</span></span>
<span data-ttu-id="2aa5a-149">Hello gyors üzembe helyezés panel kattintson a weblapokat Get kód toomonitor:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-149">From hello QuickStart blade, click Get code toomonitor my web pages:</span></span>

![Az alkalmazás áttekintése paneljén válassza a gyors üzembe helyezés, majd kód toomonitor a weblapokat.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

<span data-ttu-id="2aa5a-152">Helyezze be a HTML-fájlok hello vezetője hello kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-152">Insert hello code snippet in hello head of your HTML files.</span></span>

#### <a name="view-client-side-data"></a><span data-ttu-id="2aa5a-153">Az ügyféloldali adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="2aa5a-153">View client-side data</span></span>
<span data-ttu-id="2aa5a-154">Nyissa meg a frissített weblapok és használhatják azokat.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-154">Open your updated web pages and use them.</span></span> <span data-ttu-id="2aa5a-155">Várjon, amíg egy-két percen, majd térjen vissza a tooApplication elemzések és a nyitott hello használati panelen.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-155">Wait a minute or two, then return tooApplication Insights and open hello usage blade.</span></span> <span data-ttu-id="2aa5a-156">(Hello áttekintése panelen görgessen lefelé, és kattintson használati.)</span><span class="sxs-lookup"><span data-stu-id="2aa5a-156">(From hello Overview blade, scroll down and click Usage.)</span></span>

<span data-ttu-id="2aa5a-157">Lap nézet, a felhasználó és a munkamenet metrikák hello használati panelen jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-157">Page view, user, and session metrics will appear on hello usage blade:</span></span>

![Munkamenetek, a felhasználók és az oldalmegtekintéseket](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

<span data-ttu-id="2aa5a-159">[További információ az ügyféloldali telemetriai beállítása.][usage]</span><span class="sxs-lookup"><span data-stu-id="2aa5a-159">[Learn more about setting up client-side telemetry.][usage]</span></span>

## <a name="publish-your-application"></a><span data-ttu-id="2aa5a-160">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="2aa5a-160">Publish your application</span></span>
<span data-ttu-id="2aa5a-161">Most már a toohello alkalmazáskiszolgálóra közzétételéhez a felhasználók használni, és tekintse meg a hello telemetriai hello portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-161">Now publish your app toohello server, let people use it, and watch hello telemetry show up on hello portal.</span></span>

* <span data-ttu-id="2aa5a-162">Győződjön meg arról, hogy a tűzfala lehetővé teszi, hogy az alkalmazás toosend telemetriai toothese portok:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-162">Make sure your firewall allows your application toosend telemetry toothese ports:</span></span>

  * <span data-ttu-id="2aa5a-163">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="2aa5a-163">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="2aa5a-164">dc.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="2aa5a-164">dc.services.visualstudio.com:80</span></span>
  * <span data-ttu-id="2aa5a-165">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="2aa5a-165">f5.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="2aa5a-166">f5.services.visualstudio.com:80</span><span class="sxs-lookup"><span data-stu-id="2aa5a-166">f5.services.visualstudio.com:80</span></span>
* <span data-ttu-id="2aa5a-167">Windows-kiszolgálókon telepítse a következőt:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-167">On Windows servers, install:</span></span>

  * [<span data-ttu-id="2aa5a-168">Microsoft Visual C++ újraterjeszthető csomag</span><span class="sxs-lookup"><span data-stu-id="2aa5a-168">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="2aa5a-169">(Ez lehetővé teszi a teljesítményszámlálókat.)</span><span class="sxs-lookup"><span data-stu-id="2aa5a-169">(This enables performance counters.)</span></span>

## <a name="exceptions-and-request-failures"></a><span data-ttu-id="2aa5a-170">Kivételek és kérelemhibák</span><span class="sxs-lookup"><span data-stu-id="2aa5a-170">Exceptions and request failures</span></span>
<span data-ttu-id="2aa5a-171">A rendszer a nem kezelt kivételeket automatikusan begyűjti:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-171">Unhandled exceptions are automatically collected:</span></span>

![](./media/app-insights-java-eclipse/21-exceptions.png)

<span data-ttu-id="2aa5a-172">más kivételek toocollect adatok, két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-172">toocollect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="2aa5a-173">[INSERT tooTrackException meghívja a kódban](app-insights-api-custom-events-metrics.md#trackexception).</span><span class="sxs-lookup"><span data-stu-id="2aa5a-173">[Insert calls tooTrackException in your code](app-insights-api-custom-events-metrics.md#trackexception).</span></span>
* <span data-ttu-id="2aa5a-174">[Hello Java-ügynök telepítése a kiszolgálón](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="2aa5a-174">[Install hello Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="2aa5a-175">Megadhatja a kívánt toowatch hello módszerek.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-175">You specify hello methods you want toowatch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="2aa5a-176">Metódushívások és külső függőségek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="2aa5a-176">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="2aa5a-177">[Hello Java-ügynök telepítése](app-insights-java-agent.md) toolog megadott belső módszerek és adatokkal időzítése keresztül JDBC, felé indított hívások.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-177">[Install hello Java Agent](app-insights-java-agent.md) toolog specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="2aa5a-178">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="2aa5a-178">Performance counters</span></span>
<span data-ttu-id="2aa5a-179">Az Áttekintés panelen görgessen lefelé, és kattintson a hello **kiszolgálók** csempére.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-179">On your Overview blade, scroll down and click hello  **Servers** tile.</span></span> <span data-ttu-id="2aa5a-180">Teljesítményszámlálók számos láthatja.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-180">You'll see a range of performance counters.</span></span>

![Görgessen lefelé tooclick hello kiszolgálók csempéje](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="2aa5a-182">Teljesítményszámláló-gyűjtemény testreszabása</span><span class="sxs-lookup"><span data-stu-id="2aa5a-182">Customize performance counter collection</span></span>
<span data-ttu-id="2aa5a-183">hello szabványos készletét, a teljesítményszámlálók gyűjteményét toodisable adja hozzá a következő kódot a gyökércsomópont hello hello ApplicationInsights.xml fájl hello:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-183">toodisable collection of hello standard set of performance counters, add hello following code under hello root node of hello ApplicationInsights.xml file:</span></span>

```XML

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="2aa5a-184">További teljesítményszámlálók gyűjtése</span><span class="sxs-lookup"><span data-stu-id="2aa5a-184">Collect additional performance counters</span></span>
<span data-ttu-id="2aa5a-185">További teljesítmény számlálók toobe összegyűjtött adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-185">You can specify additional performance counters toobe collected.</span></span>

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a><span data-ttu-id="2aa5a-186">JMX számlálók (hello Java virtuális gép által elérhetővé tett)</span><span class="sxs-lookup"><span data-stu-id="2aa5a-186">JMX counters (exposed by hello Java Virtual Machine)</span></span>

```XML

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="2aa5a-187">`displayName`– hello Application Insights portálon megjelenő hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-187">`displayName` – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="2aa5a-188">`objectName`– hello JMX objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-188">`objectName` – hello JMX object name.</span></span>
* <span data-ttu-id="2aa5a-189">`attribute`– hello JMX objektum neve toofetch hello attribútuma</span><span class="sxs-lookup"><span data-stu-id="2aa5a-189">`attribute` – hello attribute of hello JMX object name toofetch</span></span>
* <span data-ttu-id="2aa5a-190">`type`(nem kötelező) – hello JMX objektum attribútum típusa:</span><span class="sxs-lookup"><span data-stu-id="2aa5a-190">`type` (optional) - hello type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="2aa5a-191">Alapértelmezett: egyszerű típus, például int vagy long.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-191">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="2aa5a-192">`composite`: a "Attribute.Data" hello formátumban van hello teljesítményszámláló adatait</span><span class="sxs-lookup"><span data-stu-id="2aa5a-192">`composite`: hello perf counter data is in hello format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="2aa5a-193">`tabular`: a tábla sorának hello formátumban van hello teljesítményszámláló adatait</span><span class="sxs-lookup"><span data-stu-id="2aa5a-193">`tabular`: hello perf counter data is in hello format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="2aa5a-194">Windows-teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="2aa5a-194">Windows performance counters</span></span>
<span data-ttu-id="2aa5a-195">Minden egyes [Windows teljesítményszámláló](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kategória tagja (a hello azonos módon, hogy egy mező osztály tagjai).</span><span class="sxs-lookup"><span data-stu-id="2aa5a-195">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in hello same way that a field is a member of a class).</span></span> <span data-ttu-id="2aa5a-196">A kategóriák globálisak lehetnek, vagy számozott vagy elnevezett példányokkal rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-196">Categories can either be global, or can have numbered or named instances.</span></span>

```XML

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="2aa5a-197">displayName – hello Application Insights portálon megjelenő hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-197">displayName – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="2aa5a-198">categoryName – hello teljesítményszámláló-kategória (teljesítményobjektum), amelyhez ez a teljesítményszámláló társítva.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-198">categoryName – hello performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="2aa5a-199">counterName – hello teljesítményszámláló hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-199">counterName – hello name of hello performance counter.</span></span>
* <span data-ttu-id="2aa5a-200">a példánynév – hello hello teljesítményszámláló-kategória példány nevét, vagy üres karakterlánc (""), ha hello kategória egyetlen példányt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-200">instanceName – hello name of hello performance counter category instance, or an empty string (""), if hello category contains a single instance.</span></span> <span data-ttu-id="2aa5a-201">Ha hello categoryName folyamat, és azt szeretné, hogy toocollect hello teljesítményszámláló hello aktuális JVM folyamat során a, amely az alkalmazás fut, adja meg `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-201">If hello categoryName is Process, and hello performance counter you'd like toocollect is from hello current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="2aa5a-202">A teljesítményszámlálói egyéni mérőszámokként láthatók a [Metrikaböngészőben][metrics].</span><span class="sxs-lookup"><span data-stu-id="2aa5a-202">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="2aa5a-203">Unix-teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="2aa5a-203">Unix performance counters</span></span>
* <span data-ttu-id="2aa5a-204">[Hello Application Insights beépülő modul telepítése collectd](app-insights-java-collectd.md) tooget széles körének a rendszer és a hálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-204">[Install collectd with hello Application Insights plugin](app-insights-java-collectd.md) tooget a wide variety of system and network data.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="2aa5a-205">Rendelkezésre állási webes tesztek</span><span class="sxs-lookup"><span data-stu-id="2aa5a-205">Availability web tests</span></span>
<span data-ttu-id="2aa5a-206">Az Application Insights tesztelheti, hogy működik-e rendszeres időközönként toocheck és is válaszol a webhelyen.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-206">Application Insights can test your website at regular intervals toocheck that it's up and responding well.</span></span> <span data-ttu-id="2aa5a-207">[másolatot tooset][availability], tooclick rendelkezésre állási görgetve.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-207">[tooset up][availability], scroll down tooclick Availability.</span></span>

![Görgessen lefelé, kattintson a Rendelkezésre állás elemre, majd a Webes teszt hozzáadása elemre](./media/app-insights-java-eclipse/31-config-web-test.png)

<span data-ttu-id="2aa5a-209">Megkapja a válaszidők diagramjait, valamint e-mailes értesítéseket kap, ha a webhely leáll.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-209">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Példa webes tesztre](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

<span data-ttu-id="2aa5a-211">[További információk a rendelkezésre állási webes tesztekről.][availability]</span><span class="sxs-lookup"><span data-stu-id="2aa5a-211">[Learn more about availability web tests.][availability]</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="2aa5a-212">Diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="2aa5a-212">Diagnostic logs</span></span>
<span data-ttu-id="2aa5a-213">Ha a Log4J vagy a Logback használata (1.2-es verzió vagy 2.0-s) a nyomkövetés, akkor a nyomkövetési naplók automatikusan elküld a tooApplication Insights, ahol vizsgálatát, és keresse meg őket.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-213">If you're using Logback or Log4J (v1.2 or v2.0) for tracing, you can have your trace logs sent automatically tooApplication Insights where you can explore and search on them.</span></span>

<span data-ttu-id="2aa5a-214">[További információ a diagnosztikai naplók][javalogs]</span><span class="sxs-lookup"><span data-stu-id="2aa5a-214">[Learn more about diagnostic logs][javalogs]</span></span>

## <a name="custom-telemetry"></a><span data-ttu-id="2aa5a-215">Egyéni telemetria</span><span class="sxs-lookup"><span data-stu-id="2aa5a-215">Custom telemetry</span></span>
<span data-ttu-id="2aa5a-216">Helyezze be néhány sornyi kód a Java webes alkalmazás toofind ki a felhasználók tevékenységeit vele, vagy toohelp problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-216">Insert a few lines of code in your Java web application toofind out what users are doing with it or toohelp diagnose problems.</span></span>

<span data-ttu-id="2aa5a-217">Kód weblapon JavaScript és hello kiszolgálóoldali Java beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-217">You can insert code both in web page JavaScript and in hello server-side Java.</span></span>

<span data-ttu-id="2aa5a-218">[További tudnivalók az egyéni telemetria][track]</span><span class="sxs-lookup"><span data-stu-id="2aa5a-218">[Learn about custom telemetry][track]</span></span>

## <a name="next-steps"></a><span data-ttu-id="2aa5a-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2aa5a-219">Next steps</span></span>
#### <a name="detect-and-diagnose-issues"></a><span data-ttu-id="2aa5a-220">Észlelheti és diagnosztizálhatja a problémákat</span><span class="sxs-lookup"><span data-stu-id="2aa5a-220">Detect and diagnose issues</span></span>
* <span data-ttu-id="2aa5a-221">[Adja hozzá a webes ügyfél telemetriai] [ usage] tooget teljesítmény telemetriai hello webes ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-221">[Add web client telemetry][usage] tooget performance telemetry from hello web client.</span></span>
* <span data-ttu-id="2aa5a-222">[Webalkalmazás-tesztek beállítása] [ availability] toomake meg arról, hogy az alkalmazás élő és rugalmas marad.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-222">[Set up web tests][availability] toomake sure your application stays live and responsive.</span></span>
* <span data-ttu-id="2aa5a-223">[Keresést az események és a naplók] [ diagnostic] toohelp problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-223">[Search events and logs][diagnostic] toohelp diagnose problems.</span></span>
* <span data-ttu-id="2aa5a-224">[Log4J vagy a Logback nyomkövetés rögzítése][javalogs]</span><span class="sxs-lookup"><span data-stu-id="2aa5a-224">[Capture Log4J or Logback traces][javalogs]</span></span>

#### <a name="track-usage"></a><span data-ttu-id="2aa5a-225">Használat követése</span><span class="sxs-lookup"><span data-stu-id="2aa5a-225">Track usage</span></span>
* <span data-ttu-id="2aa5a-226">[Adja hozzá a webes ügyfél telemetriai] [ usage] toomonitor lapon nézetek és az alapszintű felhasználói metrikákat.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-226">[Add web client telemetry][usage] toomonitor page views and basic user metrics.</span></span>
* <span data-ttu-id="2aa5a-227">[Egyéni események és metrikák nyomon](app-insights-web-track-usage.md) módjáról az alkalmazás, mind a hello ügyfélen és kiszolgálón hello toolearn.</span><span class="sxs-lookup"><span data-stu-id="2aa5a-227">[Track custom events and metrics](app-insights-web-track-usage.md) toolearn about how your application is used, both at hello client and hello server.</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
