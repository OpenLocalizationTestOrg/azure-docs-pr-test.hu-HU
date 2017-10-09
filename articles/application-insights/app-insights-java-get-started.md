---
title: "aaaJava web app analytics az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "Alkalmazásteljesítmény-figyelés Java-webalkalmazásokhoz az Application Insights használatával. "
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6555ee53a44f937350e4fa296080f7dce4f45226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a><span data-ttu-id="4166f-103">Ismerkedés az Application Insights szolgáltatással Java webes projektben</span><span class="sxs-lookup"><span data-stu-id="4166f-103">Get started with Application Insights in a Java web project</span></span>


<span data-ttu-id="4166f-104">[Az Application Insights](https://azure.microsoft.com/services/application-insights/) van egy bővíthető elemzési szolgáltatás, a webfejlesztőknek, amely segít megérteni a hello teljesítmény- és az élő alkalmazás használati.</span><span class="sxs-lookup"><span data-stu-id="4166f-104">[Application Insights](https://azure.microsoft.com/services/application-insights/) is an extensible analytics service for web developers that helps you understand hello performance and usage of your live application.</span></span> <span data-ttu-id="4166f-105">Ezzel túl[észlelheti és diagnosztizálhatja a teljesítménybeli problémák és kivételek](app-insights-detect-triage-diagnose.md), és [írhat kódot] [ api] tootrack az alkalmazás a felhasználók milyen elvégezni.</span><span class="sxs-lookup"><span data-stu-id="4166f-105">Use it too[detect and diagnose performance issues and exceptions](app-insights-detect-triage-diagnose.md), and [write code][api] tootrack what users do with your app.</span></span>

![mintaadatok](./media/app-insights-java-get-started/5-results.png)

<span data-ttu-id="4166f-107">Az Application Insights a Linux, Unix vagy Windows rendszeren futó Java alkalmazásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="4166f-107">Application Insights supports Java apps running on Linux, Unix, or Windows.</span></span>

<span data-ttu-id="4166f-108">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="4166f-108">You need:</span></span>

* <span data-ttu-id="4166f-109">Oracle JRE 1.6 vagy újabb, vagy Zulu JRE 1.6 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="4166f-109">Oracle JRE 1.6 or later, or Zulu JRE 1.6 or later</span></span>
* <span data-ttu-id="4166f-110">Előfizetés túl[Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="4166f-110">A subscription too[Microsoft Azure](https://azure.microsoft.com/).</span></span>

<span data-ttu-id="4166f-111">*Ha a webes alkalmazás, amely már élő, követhető hello alternatív eljárás túl[hello SDK hozzáadása hello webkiszolgálón futásidőben](app-insights-java-live.md). Adott alternatív elkerülhető hello kód újraépítését, de hello beállítás toowrite kód tootrack felhasználói tevékenység nem jelenik.*</span><span class="sxs-lookup"><span data-stu-id="4166f-111">*If you have a web app that's already live, you could follow hello alternative procedure too[add hello SDK at runtime in hello web server](app-insights-java-live.md). That alternative avoids rebuilding hello code, but you don't get hello option toowrite code tootrack user activity.*</span></span>

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="4166f-112">1. Application Insights-kialakítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="4166f-112">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="4166f-113">Jelentkezzen be toohello [Microsoft Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4166f-113">Sign in toohello [Microsoft Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="4166f-114">Hozzon létre egy Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="4166f-114">Create an Application Insights resource.</span></span> <span data-ttu-id="4166f-115">Állítsa be a hello alkalmazás típusú tooJava webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4166f-115">Set hello application type tooJava web application.</span></span>

    ![Adjon meg egy nevet, válassza ki a Java webalkalmazást, és kattintson a Létrehozás gombra.](./media/app-insights-java-get-started/02-create.png)
3. <span data-ttu-id="4166f-117">Hello instrumentation kulcs hello új erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="4166f-117">Find hello instrumentation key of hello new resource.</span></span> <span data-ttu-id="4166f-118">Szüksége lesz toopaste ezt a kulcsot a kód projektben hamarosan.</span><span class="sxs-lookup"><span data-stu-id="4166f-118">You'll need toopaste this key into your code project shortly.</span></span>

    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-get-started/03-key.png)

## <a name="2-add-hello-application-insights-sdk-for-java-tooyour-project"></a><span data-ttu-id="4166f-120">2. Application Insights SDK hello Java tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4166f-120">2. Add hello Application Insights SDK for Java tooyour project</span></span>
<span data-ttu-id="4166f-121">*Válassza ki a projekt hello megfelelő megoldást.*</span><span class="sxs-lookup"><span data-stu-id="4166f-121">*Choose hello appropriate way for your project.*</span></span>

#### <a name="if-youre-using-eclipse-toocreate-a-maven-or-dynamic-web-project-"></a><span data-ttu-id="4166f-122">Ha használ Holdas Maven vagy dinamikus webes projekt toocreate...</span><span class="sxs-lookup"><span data-stu-id="4166f-122">If you're using Eclipse toocreate a Maven or Dynamic Web project ...</span></span>
<span data-ttu-id="4166f-123">Használjon hello [beépülő modul Javához készült Application Insights SDK][eclipse].</span><span class="sxs-lookup"><span data-stu-id="4166f-123">Use hello [Application Insights SDK for Java plug-in][eclipse].</span></span>

#### <a name="if-youre-using-maven"></a><span data-ttu-id="4166f-124">Ha Mavent használ...</span><span class="sxs-lookup"><span data-stu-id="4166f-124">If you're using Maven...</span></span>
<span data-ttu-id="4166f-125">Ha a projekt toouse Maven build a már be van állítva, az egyesítési hello következő kódot tooyour pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="4166f-125">If your project is already set up toouse Maven for build, merge hello following code tooyour pom.xml file.</span></span>

<span data-ttu-id="4166f-126">Ezt követően frissítési hello projekt függőségek tooget hello bináris fájljainak letöltése.</span><span class="sxs-lookup"><span data-stu-id="4166f-126">Then, refresh hello project dependencies tooget hello binaries downloaded.</span></span>

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>
```

* <span data-ttu-id="4166f-127">*Build- vagy ellenőrzőösszeg-érvényesítési hibák?*</span><span class="sxs-lookup"><span data-stu-id="4166f-127">*Build or checksum validation errors?*</span></span> <span data-ttu-id="4166f-128">Próbáljon egy adott verziót használni, például a következőt: `<version>1.0.n</version>`.</span><span class="sxs-lookup"><span data-stu-id="4166f-128">Try using a specific version, such as: `<version>1.0.n</version>`.</span></span> <span data-ttu-id="4166f-129">Hello hello legújabb verziója található [SDK kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) vagy a [Maven összetevők](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).</span><span class="sxs-lookup"><span data-stu-id="4166f-129">You'll find hello latest version in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) or in our [Maven artifacts](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).</span></span>
* <span data-ttu-id="4166f-130">*Tooupdate tooa kell új SDK?*</span><span class="sxs-lookup"><span data-stu-id="4166f-130">*Need tooupdate tooa new SDK?*</span></span> <span data-ttu-id="4166f-131">Frissítse a projekt függőségeit.</span><span class="sxs-lookup"><span data-stu-id="4166f-131">Refresh your project's dependencies.</span></span>

#### <a name="if-youre-using-gradle"></a><span data-ttu-id="4166f-132">Ha Gradle-t használ...</span><span class="sxs-lookup"><span data-stu-id="4166f-132">If you're using Gradle...</span></span>
<span data-ttu-id="4166f-133">A projekt buildjénél a gradle-lel toouse már be van állítva, ha egyesítése a következő kód tooyour build.gradle fájl hello.</span><span class="sxs-lookup"><span data-stu-id="4166f-133">If your project is already set up toouse Gradle for build, merge hello following code tooyour build.gradle file.</span></span>

<span data-ttu-id="4166f-134">Majd frissítési hello projekt függőségek tooget hello bináris fájljainak letöltése.</span><span class="sxs-lookup"><span data-stu-id="4166f-134">Then refresh hello project dependencies tooget hello binaries downloaded.</span></span>

```JSON

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }
```

* <span data-ttu-id="4166f-135">*Build- vagy ellenőrzőösszeg-érvényesítési hibák? Próbáljon adott verziót használni, például a következőt:* `version:'1.0.n'`.</span><span class="sxs-lookup"><span data-stu-id="4166f-135">*Build or checksum validation errors? Try using a specific version, such as:* `version:'1.0.n'`.</span></span> <span data-ttu-id="4166f-136">*Hello hello legújabb verziója található [SDK kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*</span><span class="sxs-lookup"><span data-stu-id="4166f-136">*You'll find hello latest version in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*</span></span>
* <span data-ttu-id="4166f-137">*tooupdate tooa új SDK*</span><span class="sxs-lookup"><span data-stu-id="4166f-137">*tooupdate tooa new SDK*</span></span>
  * <span data-ttu-id="4166f-138">Frissítse a projekt függőségeit.</span><span class="sxs-lookup"><span data-stu-id="4166f-138">Refresh your project's dependencies.</span></span>

#### <a name="otherwise-"></a><span data-ttu-id="4166f-139">Egyéb esetben...</span><span class="sxs-lookup"><span data-stu-id="4166f-139">Otherwise ...</span></span>
<span data-ttu-id="4166f-140">Hello SDK manuális hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="4166f-140">Manually add hello SDK:</span></span>

1. <span data-ttu-id="4166f-141">Töltse le a hello [Javához készült Application Insights SDK](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="4166f-141">Download hello [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span>
2. <span data-ttu-id="4166f-142">Hello bináris kinyerése hello zip-fájl, és tooyour projektben adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="4166f-142">Extract hello binaries from hello zip file and add them tooyour project.</span></span>

### <a name="questions"></a><span data-ttu-id="4166f-143">Kérdések...</span><span class="sxs-lookup"><span data-stu-id="4166f-143">Questions...</span></span>
* <span data-ttu-id="4166f-144">*Mi az az hello hello kapcsolatát `-core` és `-web` hello zip összetevőket?*</span><span class="sxs-lookup"><span data-stu-id="4166f-144">*What's hello relationship between hello `-core` and `-web` components in hello zip?*</span></span>

  * <span data-ttu-id="4166f-145">`applicationinsights-core`operációs rendszer API hello akkor biztosít.</span><span class="sxs-lookup"><span data-stu-id="4166f-145">`applicationinsights-core` gives you hello bare API.</span></span> <span data-ttu-id="4166f-146">Erre az összetevőre mindig szüksége van.</span><span class="sxs-lookup"><span data-stu-id="4166f-146">You always need this component.</span></span>
  * <span data-ttu-id="4166f-147">`applicationinsights-web`– olyan mérőszámokat biztosít, amelyek nyomon követik a HTTP-kérések számát és a válaszidőket.</span><span class="sxs-lookup"><span data-stu-id="4166f-147">`applicationinsights-web` gives you metrics that track HTTP request counts and response times.</span></span> <span data-ttu-id="4166f-148">Ezt az összetevőt kihagyhatja, ha nem szeretné automatikusan gyűjteni ezt a telemetriát.</span><span class="sxs-lookup"><span data-stu-id="4166f-148">You can omit this component if you don't want this telemetry automatically collected.</span></span> <span data-ttu-id="4166f-149">Ha például azt szeretné, toowrite saját.</span><span class="sxs-lookup"><span data-stu-id="4166f-149">For example, if you want toowrite your own.</span></span>
* <span data-ttu-id="4166f-150">*tooupdate hello SDK jelenleg a változtatások közzétételekor*</span><span class="sxs-lookup"><span data-stu-id="4166f-150">*tooupdate hello SDK when we publish changes*</span></span>

  * <span data-ttu-id="4166f-151">Töltse le a hello legújabb [Javához készült Application Insights SDK](https://aka.ms/qqkaq6) és a régiek hello cseréje.</span><span class="sxs-lookup"><span data-stu-id="4166f-151">Download hello latest [Application Insights SDK for Java](https://aka.ms/qqkaq6) and replace hello old ones.</span></span>
  * <span data-ttu-id="4166f-152">Hello ismerteti a módosítások [SDK kibocsátási megjegyzéseket](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).</span><span class="sxs-lookup"><span data-stu-id="4166f-152">Changes are described in hello [SDK release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="4166f-153">3. Application Insights .xml fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4166f-153">3. Add an Application Insights .xml file</span></span>
<span data-ttu-id="4166f-154">ApplicationInsights.xml toohello erőforrások mappa hozzáadása a projekthez a, vagy ellenőrizze, hogy tooyour projekt telepítési osztály útvonala kerül.</span><span class="sxs-lookup"><span data-stu-id="4166f-154">Add ApplicationInsights.xml toohello resources folder in your project, or make sure it is added tooyour project’s deployment class path.</span></span> <span data-ttu-id="4166f-155">Másolás hello XML következő bele.</span><span class="sxs-lookup"><span data-stu-id="4166f-155">Copy hello following XML into it.</span></span>

<span data-ttu-id="4166f-156">Helyettesítő hello instrumentation kulcs portáltól kapott hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4166f-156">Substitute hello instrumentation key that you got from hello Azure portal.</span></span>

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```


* <span data-ttu-id="4166f-157">minden telemetriai tétel együtt küldött hello instrumentation kulcs, és közli az Application Insights toodisplay legyen az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="4166f-157">hello instrumentation key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>
* <span data-ttu-id="4166f-158">hello HTTP-kérelem összetevő nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="4166f-158">hello HTTP Request component is optional.</span></span> <span data-ttu-id="4166f-159">Automatikusan elküldi a telemetriai kérelem és válasz alkalommal toohello portál.</span><span class="sxs-lookup"><span data-stu-id="4166f-159">It automatically sends telemetry about requests and response times toohello portal.</span></span>
* <span data-ttu-id="4166f-160">Korrelációs események egy hozzáadása toohello HTTP-kérelem összetevő.</span><span class="sxs-lookup"><span data-stu-id="4166f-160">Events correlation is an addition toohello HTTP request component.</span></span> <span data-ttu-id="4166f-161">Hozzárendel egy azonosító tooeach kérelem hello kiszolgáló által fogadott, és felveszi ezt az azonosítót telemetriai adatot tulajdonság tooevery elemként hello tulajdonság "Operation.Id".</span><span class="sxs-lookup"><span data-stu-id="4166f-161">It assigns an identifier tooeach request received by hello server, and adds this identifier as a property tooevery item of telemetry as hello property 'Operation.Id'.</span></span> <span data-ttu-id="4166f-162">Állítsa be a szűrőt minden kérelemhez társított toocorrelate hello telemetriai lehetővé teszi [diagnosztikai keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="4166f-162">It allows you toocorrelate hello telemetry associated with each request by setting a filter in [diagnostic search][diagnostic].</span></span>
* <span data-ttu-id="4166f-163">hello Application Insights kulcs dinamikusan hello az Azure-portálon argumentumként átadhatók a rendszer tulajdonság (-DAPPLICATION_INSIGHTS_IKEY = your_ikey).</span><span class="sxs-lookup"><span data-stu-id="4166f-163">hello Application Insights key can be passed dynamically from hello Azure portal as a system property (-DAPPLICATION_INSIGHTS_IKEY=your_ikey).</span></span> <span data-ttu-id="4166f-164">Ha nincs tulajdonság meghatározva, környezeti változót (APPLICATION_INSIGHTS_IKEY) keres az Azure App-beállításokban.</span><span class="sxs-lookup"><span data-stu-id="4166f-164">If there is no property defined, it checks for environment variable (APPLICATION_INSIGHTS_IKEY) in Azure App Settings.</span></span> <span data-ttu-id="4166f-165">Ha mindkét hello tulajdonság nincs megadva, hello alapértelmezett InstrumentationKey ApplicationInsights.xml használják.</span><span class="sxs-lookup"><span data-stu-id="4166f-165">If both hello properties are undefined, hello default InstrumentationKey is used from ApplicationInsights.xml.</span></span> <span data-ttu-id="4166f-166">Ebben a sorozatban toomanage nyújt segítséget a különböző környezetekhez tartozó különböző InstrumentationKeys dinamikusan.</span><span class="sxs-lookup"><span data-stu-id="4166f-166">This sequence helps you toomanage different InstrumentationKeys for different environments dynamically.</span></span>

### <a name="alternative-ways-tooset-hello-instrumentation-key"></a><span data-ttu-id="4166f-167">Alternatív módszereket tooset hello instrumentation kulcs</span><span class="sxs-lookup"><span data-stu-id="4166f-167">Alternative ways tooset hello instrumentation key</span></span>
<span data-ttu-id="4166f-168">Application Insights SDK keresi hello kulcs az itt megadott sorrendben:</span><span class="sxs-lookup"><span data-stu-id="4166f-168">Application Insights SDK looks for hello key in this order:</span></span>

1. <span data-ttu-id="4166f-169">Rendszertulajdonság: -DAPPLICATION_INSIGHTS_IKEY=saját_kialakítási_kulcs</span><span class="sxs-lookup"><span data-stu-id="4166f-169">System property: -DAPPLICATION_INSIGHTS_IKEY=your_ikey</span></span>
2. <span data-ttu-id="4166f-170">Környezeti változó: APPLICATION_INSIGHTS_IKEY</span><span class="sxs-lookup"><span data-stu-id="4166f-170">Environment variable: APPLICATION_INSIGHTS_IKEY</span></span>
3. <span data-ttu-id="4166f-171">Konfigurációs fájl: ApplicationInsights.xml</span><span class="sxs-lookup"><span data-stu-id="4166f-171">Configuration file: ApplicationInsights.xml</span></span>

<span data-ttu-id="4166f-172">[Beállíthatja a programkódban](app-insights-api-custom-events-metrics.md#ikey) is:</span><span class="sxs-lookup"><span data-stu-id="4166f-172">You can also [set it in code](app-insights-api-custom-events-metrics.md#ikey):</span></span>

```Java

    telemetryClient.InstrumentationKey = "...";
```

## <a name="4-add-an-http-filter"></a><span data-ttu-id="4166f-173">4. HTTP-szűrő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4166f-173">4. Add an HTTP filter</span></span>
<span data-ttu-id="4166f-174">hello utolsó konfigurációs lépés lehetővé teszi, hogy hello HTTP kérelem összetevő toolog minden webes kérelem.</span><span class="sxs-lookup"><span data-stu-id="4166f-174">hello last configuration step allows hello HTTP request component toolog each web request.</span></span> <span data-ttu-id="4166f-175">(Nem kötelező, ha csak hello operációs rendszer API.)</span><span class="sxs-lookup"><span data-stu-id="4166f-175">(Not required if you just want hello bare API.)</span></span>

<span data-ttu-id="4166f-176">Keresse meg, és nyissa meg a projekt, és a következő kód hello webalkalmazás csomópont alatt, ahol az alkalmazás szűrőit egyesítési hello hello web.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="4166f-176">Locate and open hello web.xml file in your project, and merge hello following code under hello web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="4166f-177">hello szűrő tooget hello legpontosabb eredmények leképezéshez előtt az összes többi szűrőt.</span><span class="sxs-lookup"><span data-stu-id="4166f-177">tooget hello most accurate results, hello filter should be mapped before all other filters.</span></span>

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a><span data-ttu-id="4166f-178">Ha a Spring Web MVC 3.1-es vagy újabb verzióját használja</span><span class="sxs-lookup"><span data-stu-id="4166f-178">If you're using Spring Web MVC 3.1 or later</span></span>
<span data-ttu-id="4166f-179">Ezeket az elemeket a Szerkesztés *-servlet.xml tooinclude hello Application Insights csomagot:</span><span class="sxs-lookup"><span data-stu-id="4166f-179">Edit these elements in *-servlet.xml tooinclude hello Application Insights package:</span></span>

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a><span data-ttu-id="4166f-180">Ha Struts 2-t használ</span><span class="sxs-lookup"><span data-stu-id="4166f-180">If you're using Struts 2</span></span>
<span data-ttu-id="4166f-181">Adja hozzá a cikk toohello merevítőket vagy konfigurációs fájl (általában elnevezett struts.xml merevítőket-default.xml):</span><span class="sxs-lookup"><span data-stu-id="4166f-181">Add this item toohello Struts configuration file (usually named struts.xml or struts-default.xml):</span></span>

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

<span data-ttu-id="4166f-182">(Ha van egy alapértelmezett verem definiált elfogókat, hello elfogó egyszerűen adhatók hozzá toothat verem.)</span><span class="sxs-lookup"><span data-stu-id="4166f-182">(If you have interceptors defined in a default stack, hello interceptor can simply be added toothat stack.)</span></span>

## <a name="5-run-your-application"></a><span data-ttu-id="4166f-183">5. Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4166f-183">5. Run your application</span></span>
<span data-ttu-id="4166f-184">A fejlesztői gépen hibakeresési módban futtatni, vagy tooyour-kiszolgáló közzététele.</span><span class="sxs-lookup"><span data-stu-id="4166f-184">Either run it in debug mode on your development machine, or publish tooyour server.</span></span>

## <a name="6-view-your-telemetry-in-application-insights"></a><span data-ttu-id="4166f-185">6. A telemetria megtekintése az Application Insights szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="4166f-185">6. View your telemetry in Application Insights</span></span>
<span data-ttu-id="4166f-186">Térjen vissza az Application Insights-erőforrás tooyour [Microsoft Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4166f-186">Return tooyour Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="4166f-187">HTTP-kérelmek adatai hello áttekintése panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4166f-187">HTTP requests data appears on hello overview blade.</span></span> <span data-ttu-id="4166f-188">(Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)</span><span class="sxs-lookup"><span data-stu-id="4166f-188">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![mintaadatok](./media/app-insights-java-get-started/5-results.png)

<span data-ttu-id="4166f-190">[További információk a metrikákról.][metrics]</span><span class="sxs-lookup"><span data-stu-id="4166f-190">[Learn more about metrics.][metrics]</span></span>

<span data-ttu-id="4166f-191">Kattintson a diagram toosee részletesebb keresztül mérőszámok összesített értéket.</span><span class="sxs-lookup"><span data-stu-id="4166f-191">Click through any chart toosee more detailed aggregated metrics.</span></span>

![](./media/app-insights-java-get-started/6-barchart.png)

> <span data-ttu-id="4166f-192">Az Application Insights azt feltételezi, hogy hello MVC-alkalmazások HTTP-kérelmek formátuma: `VERB controller/action`.</span><span class="sxs-lookup"><span data-stu-id="4166f-192">Application Insights assumes hello format of HTTP requests for MVC applications is: `VERB controller/action`.</span></span> <span data-ttu-id="4166f-193">Például a `GET Home/Product/f9anuh81`, a `GET Home/Product/2dffwrf5` és a `GET Home/Product/sdf96vws` a következőbe van csoportosítva: `GET Home/Product`.</span><span class="sxs-lookup"><span data-stu-id="4166f-193">For example, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` and `GET Home/Product/sdf96vws` are grouped into `GET Home/Product`.</span></span> <span data-ttu-id="4166f-194">Ez a csoportosítás lehetővé teszi a kérelmek fontos információkat biztosító összesítéseit, például a kérelmek számának és a kérelmek átlagos végrehajtási idejének meghatározását.</span><span class="sxs-lookup"><span data-stu-id="4166f-194">This grouping enables meaningful aggregations of requests, such as number of requests and average execution time for requests.</span></span>
>
>

### <a name="instance-data"></a><span data-ttu-id="4166f-195">Példányadatok</span><span class="sxs-lookup"><span data-stu-id="4166f-195">Instance data</span></span>
<span data-ttu-id="4166f-196">Kattintson egy adott kérelem toosee egyedi példányaira.</span><span class="sxs-lookup"><span data-stu-id="4166f-196">Click through a specific request type toosee individual instances.</span></span>

<span data-ttu-id="4166f-197">Két adattípus jelenik meg az Application Insights szolgáltatásban: összesített adatok, tárolva és átlagokként, számokként és összegekként megjelenítve; és példányadatok – a HTTP-kérelmek, kivételek, lapmegtekintések vagy egyéni események egyedi jelentései.</span><span class="sxs-lookup"><span data-stu-id="4166f-197">Two kinds of data are displayed in Application Insights: aggregated data, stored and displayed as averages, counts, and sums; and instance data - individual reports of HTTP requests, exceptions, page views, or custom events.</span></span>

<span data-ttu-id="4166f-198">Egy kérelem hello tulajdonságainak megtekintésekor láthatja hello telemetriai események például a kérelmek és kivételek társítva.</span><span class="sxs-lookup"><span data-stu-id="4166f-198">When viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a><span data-ttu-id="4166f-199">Elemzés: Erőteljes lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="4166f-199">Analytics: Powerful query language</span></span>
<span data-ttu-id="4166f-200">Több adat gyűlik össze, mert mindkét tooaggregate adatok és toofind egyes példányok lekérdezéseket is futtathat.</span><span class="sxs-lookup"><span data-stu-id="4166f-200">As you accumulate more data, you can run queries both tooaggregate data and toofind individual instances.</span></span>  <span data-ttu-id="4166f-201">Az [elemzés](app-insights-analytics.md) erőteljes eszköz a teljesítmény és a használat megértéséhez és diagnosztikai célokra is.</span><span class="sxs-lookup"><span data-stu-id="4166f-201">[Analytics](app-insights-analytics.md) is a powerful tool for both for understanding performance and usage, and for diagnostic purposes.</span></span>

![Példa elemzésre](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-hello-server"></a><span data-ttu-id="4166f-203">7. Az alkalmazás telepítése hello kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="4166f-203">7. Install your app on hello server</span></span>
<span data-ttu-id="4166f-204">Most már a toohello alkalmazáskiszolgálóra közzétételéhez a felhasználók használni, és tekintse meg a hello telemetriai hello portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4166f-204">Now publish your app toohello server, let people use it, and watch hello telemetry show up on hello portal.</span></span>

* <span data-ttu-id="4166f-205">Győződjön meg arról, hogy a tűzfala lehetővé teszi, hogy az alkalmazás toosend telemetriai toothese portok:</span><span class="sxs-lookup"><span data-stu-id="4166f-205">Make sure your firewall allows your application toosend telemetry toothese ports:</span></span>

  * <span data-ttu-id="4166f-206">dc.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="4166f-206">dc.services.visualstudio.com:443</span></span>
  * <span data-ttu-id="4166f-207">f5.services.visualstudio.com:443</span><span class="sxs-lookup"><span data-stu-id="4166f-207">f5.services.visualstudio.com:443</span></span>

* <span data-ttu-id="4166f-208">Ha a kimenő forgalmat át kell irányítani egy tűzfalon, adja meg a `http.proxyHost` és a `http.proxyPort` rendszertulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="4166f-208">If outgoing traffic must be routed through a firewall, define system properties `http.proxyHost` and `http.proxyPort`.</span></span>

* <span data-ttu-id="4166f-209">Windows-kiszolgálókon telepítse a következőt:</span><span class="sxs-lookup"><span data-stu-id="4166f-209">On Windows servers, install:</span></span>

  * [<span data-ttu-id="4166f-210">Microsoft Visual C++ újraterjeszthető csomag</span><span class="sxs-lookup"><span data-stu-id="4166f-210">Microsoft Visual C++ Redistributable</span></span>](http://www.microsoft.com/download/details.aspx?id=40784)

    <span data-ttu-id="4166f-211">(Ez az összetevő lehetővé teszi a teljesítményszámlálókat.)</span><span class="sxs-lookup"><span data-stu-id="4166f-211">(This component enables performance counters.)</span></span>


## <a name="exceptions-and-request-failures"></a><span data-ttu-id="4166f-212">Kivételek és kérelemhibák</span><span class="sxs-lookup"><span data-stu-id="4166f-212">Exceptions and request failures</span></span>
<span data-ttu-id="4166f-213">A rendszer a nem kezelt kivételeket automatikusan begyűjti:</span><span class="sxs-lookup"><span data-stu-id="4166f-213">Unhandled exceptions are automatically collected:</span></span>

![Beállítások megnyitása, Hibák](./media/app-insights-java-get-started/21-exceptions.png)

<span data-ttu-id="4166f-215">más kivételek toocollect adatok, két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="4166f-215">toocollect data on other exceptions, you have two options:</span></span>

* <span data-ttu-id="4166f-216">[INSERT tootrackException() meghívja a kódban][apiexceptions].</span><span class="sxs-lookup"><span data-stu-id="4166f-216">[Insert calls tootrackException() in your code][apiexceptions].</span></span>
* <span data-ttu-id="4166f-217">[Hello Java-ügynök telepítése a kiszolgálón](app-insights-java-agent.md).</span><span class="sxs-lookup"><span data-stu-id="4166f-217">[Install hello Java Agent on your server](app-insights-java-agent.md).</span></span> <span data-ttu-id="4166f-218">Megadhatja a kívánt toowatch hello módszerek.</span><span class="sxs-lookup"><span data-stu-id="4166f-218">You specify hello methods you want toowatch.</span></span>

## <a name="monitor-method-calls-and-external-dependencies"></a><span data-ttu-id="4166f-219">Metódushívások és külső függőségek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="4166f-219">Monitor method calls and external dependencies</span></span>
<span data-ttu-id="4166f-220">[Hello Java-ügynök telepítése](app-insights-java-agent.md) toolog megadott belső módszerek és adatokkal időzítése keresztül JDBC, felé indított hívások.</span><span class="sxs-lookup"><span data-stu-id="4166f-220">[Install hello Java Agent](app-insights-java-agent.md) toolog specified internal methods and calls made through JDBC, with timing data.</span></span>

## <a name="performance-counters"></a><span data-ttu-id="4166f-221">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="4166f-221">Performance counters</span></span>
<span data-ttu-id="4166f-222">Nyissa meg **beállítások**, **kiszolgálók**, toosee teljesítményszámlálók számos.</span><span class="sxs-lookup"><span data-stu-id="4166f-222">Open **Settings**, **Servers**, toosee a range of performance counters.</span></span>

![](./media/app-insights-java-get-started/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a><span data-ttu-id="4166f-223">Teljesítményszámláló-gyűjtemény testreszabása</span><span class="sxs-lookup"><span data-stu-id="4166f-223">Customize performance counter collection</span></span>
<span data-ttu-id="4166f-224">hello szabványos készletét, a teljesítményszámlálók gyűjteményét toodisable adja hozzá a következő kódot a gyökércsomópont hello hello ApplicationInsights.xml fájl hello:</span><span class="sxs-lookup"><span data-stu-id="4166f-224">toodisable collection of hello standard set of performance counters, add hello following code under hello root node of hello ApplicationInsights.xml file:</span></span>

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a><span data-ttu-id="4166f-225">További teljesítményszámlálók gyűjtése</span><span class="sxs-lookup"><span data-stu-id="4166f-225">Collect additional performance counters</span></span>
<span data-ttu-id="4166f-226">További teljesítmény számlálók toobe összegyűjtött adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="4166f-226">You can specify additional performance counters toobe collected.</span></span>

#### <a name="jmx-counters-exposed-by-hello-java-virtual-machine"></a><span data-ttu-id="4166f-227">JMX számlálók (hello Java virtuális gép által elérhetővé tett)</span><span class="sxs-lookup"><span data-stu-id="4166f-227">JMX counters (exposed by hello Java Virtual Machine)</span></span>

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* <span data-ttu-id="4166f-228">`displayName`– hello Application Insights portálon megjelenő hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4166f-228">`displayName` – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="4166f-229">`objectName`– hello JMX objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="4166f-229">`objectName` – hello JMX object name.</span></span>
* <span data-ttu-id="4166f-230">`attribute`– hello JMX objektum neve toofetch hello attribútuma</span><span class="sxs-lookup"><span data-stu-id="4166f-230">`attribute` – hello attribute of hello JMX object name toofetch</span></span>
* <span data-ttu-id="4166f-231">`type`(nem kötelező) – hello JMX objektum attribútum típusa:</span><span class="sxs-lookup"><span data-stu-id="4166f-231">`type` (optional) - hello type of JMX object’s attribute:</span></span>
  * <span data-ttu-id="4166f-232">Alapértelmezett: egyszerű típus, például int vagy long.</span><span class="sxs-lookup"><span data-stu-id="4166f-232">Default: a simple type such as int or long.</span></span>
  * <span data-ttu-id="4166f-233">`composite`: a "Attribute.Data" hello formátumban van hello teljesítményszámláló adatait</span><span class="sxs-lookup"><span data-stu-id="4166f-233">`composite`: hello perf counter data is in hello format of 'Attribute.Data'</span></span>
  * <span data-ttu-id="4166f-234">`tabular`: a tábla sorának hello formátumban van hello teljesítményszámláló adatait</span><span class="sxs-lookup"><span data-stu-id="4166f-234">`tabular`: hello perf counter data is in hello format of a table row</span></span>

#### <a name="windows-performance-counters"></a><span data-ttu-id="4166f-235">Windows-teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="4166f-235">Windows performance counters</span></span>
<span data-ttu-id="4166f-236">Minden egyes [Windows teljesítményszámláló](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kategória tagja (a hello azonos módon, hogy egy mező osztály tagjai).</span><span class="sxs-lookup"><span data-stu-id="4166f-236">Each [Windows performance counter](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) is a member of a category (in hello same way that a field is a member of a class).</span></span> <span data-ttu-id="4166f-237">A kategóriák globálisak lehetnek, vagy számozott vagy elnevezett példányokkal rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="4166f-237">Categories can either be global, or can have numbered or named instances.</span></span>

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* <span data-ttu-id="4166f-238">displayName – hello Application Insights portálon megjelenő hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4166f-238">displayName – hello name displayed in hello Application Insights portal.</span></span>
* <span data-ttu-id="4166f-239">categoryName – hello teljesítményszámláló-kategória (teljesítményobjektum), amelyhez ez a teljesítményszámláló társítva.</span><span class="sxs-lookup"><span data-stu-id="4166f-239">categoryName – hello performance counter category (performance object) with which this performance counter is associated.</span></span>
* <span data-ttu-id="4166f-240">counterName – hello teljesítményszámláló hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4166f-240">counterName – hello name of hello performance counter.</span></span>
* <span data-ttu-id="4166f-241">a példánynév – hello hello teljesítményszámláló-kategória példány nevét, vagy üres karakterlánc (""), ha hello kategória egyetlen példányt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4166f-241">instanceName – hello name of hello performance counter category instance, or an empty string (""), if hello category contains a single instance.</span></span> <span data-ttu-id="4166f-242">Ha hello categoryName folyamat, és azt szeretné, hogy toocollect hello teljesítményszámláló hello aktuális JVM folyamat során a, amely az alkalmazás fut, adja meg `"__SELF__"`.</span><span class="sxs-lookup"><span data-stu-id="4166f-242">If hello categoryName is Process, and hello performance counter you'd like toocollect is from hello current JVM process on which your app is running, specify `"__SELF__"`.</span></span>

<span data-ttu-id="4166f-243">A teljesítményszámlálói egyéni mérőszámokként láthatók a [Metrikaböngészőben][metrics].</span><span class="sxs-lookup"><span data-stu-id="4166f-243">Your performance counters are visible as custom metrics in [Metrics Explorer][metrics].</span></span>

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a><span data-ttu-id="4166f-244">Unix-teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="4166f-244">Unix performance counters</span></span>
* <span data-ttu-id="4166f-245">[Hello Application Insights beépülő modul telepítése collectd](app-insights-java-collectd.md) tooget széles körének a rendszer és a hálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="4166f-245">[Install collectd with hello Application Insights plugin](app-insights-java-collectd.md) tooget a wide variety of system and network data.</span></span>

## <a name="get-user-and-session-data"></a><span data-ttu-id="4166f-246">Felhasználói és munkamenetadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="4166f-246">Get user and session data</span></span>
<span data-ttu-id="4166f-247">Telemetriát küld a webkiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="4166f-247">OK, you're sending telemetry from your web server.</span></span> <span data-ttu-id="4166f-248">Most tooget hello 360 fok nézet az alkalmazás, hozzáadhat további figyelési:</span><span class="sxs-lookup"><span data-stu-id="4166f-248">Now tooget hello full 360-degree view of your application, you can add more monitoring:</span></span>

* <span data-ttu-id="4166f-249">[Adja hozzá a telemetriai adatok tooyour weblapok] [ usage] toomonitor nézetek és a felhasználó metrikák lapon.</span><span class="sxs-lookup"><span data-stu-id="4166f-249">[Add telemetry tooyour web pages][usage] toomonitor page views and user metrics.</span></span>
* <span data-ttu-id="4166f-250">[Webalkalmazás-tesztek beállítása] [ availability] toomake meg arról, hogy az alkalmazás élő és rugalmas marad.</span><span class="sxs-lookup"><span data-stu-id="4166f-250">[Set up web tests][availability] toomake sure your application stays live and responsive.</span></span>

## <a name="capture-log-traces"></a><span data-ttu-id="4166f-251">Naplónyomkövetések rögzítése</span><span class="sxs-lookup"><span data-stu-id="4166f-251">Capture log traces</span></span>
<span data-ttu-id="4166f-252">Az Application Insights tooslice használ, és ezután Log4J, Logback vagy más naplózási keretrendszer naplók.</span><span class="sxs-lookup"><span data-stu-id="4166f-252">You can use Application Insights tooslice and dice logs from Log4J, Logback, or other logging frameworks.</span></span> <span data-ttu-id="4166f-253">Hello naplók kivizsgált HTTP-kérelmek és egyéb telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="4166f-253">You can correlate hello logs with HTTP requests and other telemetry.</span></span> <span data-ttu-id="4166f-254">[Itt megismerkedhet az erre vonatkozó részletekkel][javalogs].</span><span class="sxs-lookup"><span data-stu-id="4166f-254">[Learn how][javalogs].</span></span>

## <a name="send-your-own-telemetry"></a><span data-ttu-id="4166f-255">Saját telemetria küldése</span><span class="sxs-lookup"><span data-stu-id="4166f-255">Send your own telemetry</span></span>
<span data-ttu-id="4166f-256">Most, hogy hello SDK telepítése, használhatja a saját telemetriai hello API toosend.</span><span class="sxs-lookup"><span data-stu-id="4166f-256">Now that you've installed hello SDK, you can use hello API toosend your own telemetry.</span></span>

* <span data-ttu-id="4166f-257">[Egyéni események és metrikák nyomon] [ api] toolearn teljesítenek a felhasználók számára az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="4166f-257">[Track custom events and metrics][api] toolearn what users are doing with your application.</span></span>
* <span data-ttu-id="4166f-258">[Keresést az események és a naplók] [ diagnostic] toohelp problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="4166f-258">[Search events and logs][diagnostic] toohelp diagnose problems.</span></span>

## <a name="availability-web-tests"></a><span data-ttu-id="4166f-259">Rendelkezésre állási webes tesztek</span><span class="sxs-lookup"><span data-stu-id="4166f-259">Availability web tests</span></span>
<span data-ttu-id="4166f-260">Az Application Insights tesztelheti, hogy működik-e rendszeres időközönként toocheck és is válaszol a webhelyen.</span><span class="sxs-lookup"><span data-stu-id="4166f-260">Application Insights can test your website at regular intervals toocheck that it's up and responding well.</span></span> <span data-ttu-id="4166f-261">[másolatot tooset][availability], kattintson a webes tesztjeinek használatát.</span><span class="sxs-lookup"><span data-stu-id="4166f-261">[tooset up][availability], click Web tests.</span></span>

![Kattintson a Webes tesztek elemre, majd a Webes teszt hozzáadása elemre.](./media/app-insights-java-get-started/31-config-web-test.png)

<span data-ttu-id="4166f-263">Megkapja a válaszidők diagramjait, valamint e-mailes értesítéseket kap, ha a webhely leáll.</span><span class="sxs-lookup"><span data-stu-id="4166f-263">You'll get charts of response times, plus email notifications if your site goes down.</span></span>

![Példa webes tesztre](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

<span data-ttu-id="4166f-265">[További információk a rendelkezésre állási webes tesztekről.][availability]</span><span class="sxs-lookup"><span data-stu-id="4166f-265">[Learn more about availability web tests.][availability]</span></span>

## <a name="questions-problems"></a><span data-ttu-id="4166f-266">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="4166f-266">Questions?</span></span> <span data-ttu-id="4166f-267">Problémákat tapasztal?</span><span class="sxs-lookup"><span data-stu-id="4166f-267">Problems?</span></span>
[<span data-ttu-id="4166f-268">A Java hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="4166f-268">Troubleshooting Java</span></span>](app-insights-java-troubleshoot.md)

## <a name="video"></a><span data-ttu-id="4166f-269">Videó</span><span class="sxs-lookup"><span data-stu-id="4166f-269">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a><span data-ttu-id="4166f-270">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4166f-270">Next steps</span></span>
* [<span data-ttu-id="4166f-271">Függőségi hívások figyelése</span><span class="sxs-lookup"><span data-stu-id="4166f-271">Monitor dependency calls</span></span>](app-insights-java-agent.md)
* [<span data-ttu-id="4166f-272">Unix-teljesítményszámlálók figyelése</span><span class="sxs-lookup"><span data-stu-id="4166f-272">Monitor Unix performance counters</span></span>](app-insights-java-collectd.md)
* <span data-ttu-id="4166f-273">Adja hozzá [tooyour weblapok figyelési](app-insights-javascript.md) toomonitor betöltési időkorlátja, AJAX-hívások, böngésző kivételek.</span><span class="sxs-lookup"><span data-stu-id="4166f-273">Add [monitoring tooyour web pages](app-insights-javascript.md) toomonitor page load times, AJAX calls, browser exceptions.</span></span>
* <span data-ttu-id="4166f-274">Írási [egyéni telemetria](app-insights-api-custom-events-metrics.md) tootrack használati hello böngészőben vagy hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="4166f-274">Write [custom telemetry](app-insights-api-custom-events-metrics.md) tootrack usage in hello browser or at hello server.</span></span>
* <span data-ttu-id="4166f-275">Hozzon létre [irányítópultok](app-insights-dashboards.md) toobring együtt hello kulcs diagramok esetében a rendszer figyelése.</span><span class="sxs-lookup"><span data-stu-id="4166f-275">Create [dashboards](app-insights-dashboards.md) toobring together hello key charts for monitoring your system.</span></span>
* <span data-ttu-id="4166f-276">Az [Elemzések](app-insights-analytics.md) használatával hatékony telemetriai lekérdezéseket hajthat végre az alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="4166f-276">Use  [Analytics](app-insights-analytics.md) for powerful queries over telemetry from your app</span></span>
* <span data-ttu-id="4166f-277">További információ: [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="4166f-277">For more information, visit [Azure for Java developers](/java/azure).</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
