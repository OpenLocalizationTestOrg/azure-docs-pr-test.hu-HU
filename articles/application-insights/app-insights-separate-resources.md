---
title: "aaaSeparating telemetriai fejlesztési, tesztelése és Azure Application Insights megjelenése |} Microsoft Docs"
description: "Fejlesztői, tesztelési és éles bélyegzők közvetlen telemetriai toodifferent erőforrásait."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: a294c8c70f46d7c29b460461c3494c83e13a0cbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a><span data-ttu-id="5adc2-103">Telemetria mappától fejlesztői, tesztelési és éles</span><span class="sxs-lookup"><span data-stu-id="5adc2-103">Separating telemetry from Development, Test, and Production</span></span>

<span data-ttu-id="5adc2-104">Hello egy webes alkalmazás a következő verziójában fejleszt, ha nem szeretné, hogy másolatot hello toomix [Application Insights](app-insights-overview.md) hello új és hello már kiadott verziója a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="5adc2-104">When you are developing hello next version of a web application, you don't want toomix up hello [Application Insights](app-insights-overview.md) telemetry from hello new version and hello already released version.</span></span> <span data-ttu-id="5adc2-105">tooavoid zavart küldési hello telemetriai különböző fejlesztési tooseparate Application Insights-erőforrások, előkészítette a külön instrumentation kulcsokkal (ikeys).</span><span class="sxs-lookup"><span data-stu-id="5adc2-105">tooavoid confusion, send hello telemetry from different development stages tooseparate Application Insights resources, with separate instrumentation keys (ikeys).</span></span> <span data-ttu-id="5adc2-106">toomake azt könnyebb toochange hello instrumentation kulcs verzióként helyez át egy lépésben tooanother lehet hasznos tooset hello ikey kódban helyett hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="5adc2-106">toomake it easier toochange hello instrumentation key as a version moves from one stage tooanother, it can be useful tooset hello ikey in code instead of in hello configuration file.</span></span> 

<span data-ttu-id="5adc2-107">(Ha a rendszer egy Azure-Felhőszolgáltatásban nincs [más módszerrel kell beállítania külön ikeys](app-insights-cloudservices.md).)</span><span class="sxs-lookup"><span data-stu-id="5adc2-107">(If your system is an Azure Cloud Service, there's [another method of setting separate ikeys](app-insights-cloudservices.md).)</span></span>

## <a name="about-resources-and-instrumentation-keys"></a><span data-ttu-id="5adc2-108">Erőforrások és a rendszerállapot-kulcsok</span><span class="sxs-lookup"><span data-stu-id="5adc2-108">About resources and instrumentation keys</span></span>

<span data-ttu-id="5adc2-109">Az Application Insights a webalkalmazás figyelésének beállításakor hoz létre az Application Insights *erőforrás* a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5adc2-109">When you set up Application Insights monitoring for your web app, you create an Application Insights *resource* in Microsoft Azure.</span></span> <span data-ttu-id="5adc2-110">Nyissa meg az erőforrás hello Azure portálra az order toosee, és az alkalmazásban gyűjtött hello telemetriáját elemzése.</span><span class="sxs-lookup"><span data-stu-id="5adc2-110">You open this resource in hello Azure portal in order toosee and analyze hello telemetry collected from your app.</span></span> <span data-ttu-id="5adc2-111">hello erőforrás azonosít egy *instrumentation kulcs* (ikey).</span><span class="sxs-lookup"><span data-stu-id="5adc2-111">hello resource is identified by an *instrumentation key* (ikey).</span></span> <span data-ttu-id="5adc2-112">Hello telepítésekor az Application Insights csomagot toomonitor az alkalmazást, konfigurálta hello instrumentation kulccsal, így az tudni fogja, ahol toosend hello telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="5adc2-112">When you install hello Application Insights package toomonitor your app, you configure it with hello instrumentation key, so that it knows where toosend hello telemetry.</span></span>

<span data-ttu-id="5adc2-113">Általában választja toouse külön erőforrások vagy egy megosztott erőforrás különböző helyzetekben:</span><span class="sxs-lookup"><span data-stu-id="5adc2-113">You typically choose toouse separate resources or a single shared resource in different scenarios:</span></span>

* <span data-ttu-id="5adc2-114">Különböző, független alkalmazások - alkalmazásokra vonatkozó különálló erőforrás és ikey használata.</span><span class="sxs-lookup"><span data-stu-id="5adc2-114">Different, independent applications - Use a separate resource and ikey for each app.</span></span>
* <span data-ttu-id="5adc2-115">Több összetevő vagy egy üzleti alkalmazás - szerepkörök használja egy [egyetlen megosztott erőforrás](app-insights-monitor-multi-role-apps.md) az összes összetevő alkalmazások hello.</span><span class="sxs-lookup"><span data-stu-id="5adc2-115">Multiple components or roles of one business application - Use a [single shared resource](app-insights-monitor-multi-role-apps.md) for all hello component apps.</span></span> <span data-ttu-id="5adc2-116">Telemetriai adatok szűrhetők és szegmentált hello cloud_RoleName tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5adc2-116">Telemetry can be filtered or segmented by hello cloud_RoleName property.</span></span>
* <span data-ttu-id="5adc2-117">Fejlesztői, tesztelési és verzió - használata egy külön erőforrás és ikey verziók hello rendszer "stamp" vagy az éles szakasza.</span><span class="sxs-lookup"><span data-stu-id="5adc2-117">Development, Test, and Release - Use a separate resource and ikey for versions of hello system in 'stamp' or stage of production.</span></span>
* <span data-ttu-id="5adc2-118">A |} B tesztelés - azonosítónak egyetlen erőforrásra használja.</span><span class="sxs-lookup"><span data-stu-id="5adc2-118">A | B testing - Use a single resource.</span></span> <span data-ttu-id="5adc2-119">Hozzon létre egy TelemetryInitializer tooadd egy tulajdonság toohello telemetriai adat, amely azonosítja a hello Variant adattípusban.</span><span class="sxs-lookup"><span data-stu-id="5adc2-119">Create a TelemetryInitializer tooadd a property toohello telemetry that identifies hello variants.</span></span>


## <span data-ttu-id="5adc2-120"><a name="dynamic-ikey"></a>Dinamikus instrumentation kulcs</span><span class="sxs-lookup"><span data-stu-id="5adc2-120"><a name="dynamic-ikey"></a> Dynamic instrumentation key</span></span>

<span data-ttu-id="5adc2-121">toomake adathalmazokban toochange hello ikey hello kódú helyezi át az éles, szakaszainak között állíthatja ezt be helyett kódban hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="5adc2-121">toomake it easier toochange hello ikey as hello code moves between stages of production, set it in code instead of in hello configuration file.</span></span>

<span data-ttu-id="5adc2-122">Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból hello kulcs meg:</span><span class="sxs-lookup"><span data-stu-id="5adc2-122">Set hello key in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

<span data-ttu-id="5adc2-123">*C#*</span><span class="sxs-lookup"><span data-stu-id="5adc2-123">*C#*</span></span>

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

<span data-ttu-id="5adc2-124">Ebben a példában a hello különböző erőforrásokat hello ikeys hello webes konfigurációs fájl különböző verzióinak kerülnek.</span><span class="sxs-lookup"><span data-stu-id="5adc2-124">In this example, hello ikeys for hello different resources are placed in different versions of hello web configuration file.</span></span> <span data-ttu-id="5adc2-125">Tárhellyel való felcserélése hello webes konfigurációs fájl – hello feloldási parancsprogram részeként teheti - fog felcserélése hello cél erőforráson.</span><span class="sxs-lookup"><span data-stu-id="5adc2-125">Swapping hello web configuration file - which you can do as part of hello release script - will swap hello target resource.</span></span>

### <a name="web-pages"></a><span data-ttu-id="5adc2-126">Weblapok</span><span class="sxs-lookup"><span data-stu-id="5adc2-126">Web pages</span></span>
<span data-ttu-id="5adc2-127">hello iKey is használatban van az alkalmazás weblapokat, hello [portáltól kapott hello – első lépések panelen parancsfájl](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="5adc2-127">hello iKey is also used in your app's web pages, in hello [script that you got from hello quick start blade](app-insights-javascript.md).</span></span> <span data-ttu-id="5adc2-128">Helyett megadás szó hello parancsfájlba, készítése hello állapotból.</span><span class="sxs-lookup"><span data-stu-id="5adc2-128">Instead of coding it literally into hello script, generate it from hello server state.</span></span> <span data-ttu-id="5adc2-129">Például egy ASP.NET alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="5adc2-129">For example, in an ASP.NET app:</span></span>

<span data-ttu-id="5adc2-130">*JavaScript Razor*</span><span class="sxs-lookup"><span data-stu-id="5adc2-130">*JavaScript in Razor*</span></span>

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a><span data-ttu-id="5adc2-131">További Application Insights-erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="5adc2-131">Create additional Application Insights resources</span></span>
<span data-ttu-id="5adc2-132">tooseparate telemetriai különböző alkalmazás-összetevők, vagy másik bélyegzők (fejlesztési/tesztelési/éles) hello azonos összetevő, akkor kell toocreate egy új Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="5adc2-132">tooseparate telemetry for different application components, or for different stamps (dev/test/production) of hello same component, then you'll have toocreate a new Application Insights resource.</span></span>

<span data-ttu-id="5adc2-133">A hello [portal.azure.com](https://portal.azure.com), vegyen fel egy Application Insights-erőforrást:</span><span class="sxs-lookup"><span data-stu-id="5adc2-133">In hello [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-separate-resources/01-new.png)

* <span data-ttu-id="5adc2-135">**Az alkalmazástípus** elemnél mi jelenik hello áttekintése panel és hello tulajdonságok érhetők el [metrika explorer](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="5adc2-135">**Application type** affects what you see on hello overview blade and hello properties available in [metric explorer](app-insights-metrics-explorer.md).</span></span> <span data-ttu-id="5adc2-136">Ha nem látja a típusú alkalmazást, válasszon egyet az hello webes típusok weblapokra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="5adc2-136">If you don't see your type of app, choose one of hello web types for web pages.</span></span>
* <span data-ttu-id="5adc2-137">**Erőforráscsoport** kezeléséhez a tulajdonságokat, például a könnyebb van [hozzáférés-vezérlés](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5adc2-137">**Resource group** is a convenience for managing properties like [access control](app-insights-resources-roles-access-control.md).</span></span> <span data-ttu-id="5adc2-138">Fejlesztési, tesztelési és éles külön erőforráscsoportok használható.</span><span class="sxs-lookup"><span data-stu-id="5adc2-138">You could use separate resource groups for development, test, and production.</span></span>
* <span data-ttu-id="5adc2-139">**Előfizetés** a fizetési fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5adc2-139">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="5adc2-140">**Hely** van, ahol azt megőrizni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5adc2-140">**Location** is where we keep your data.</span></span> <span data-ttu-id="5adc2-141">Jelenleg nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="5adc2-141">Currently it can't be changed.</span></span> 
* <span data-ttu-id="5adc2-142">**Adja hozzá a toodashboard** helyezi az erőforrás egy gyors elérést csempe az Azure kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="5adc2-142">**Add toodashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> 

<span data-ttu-id="5adc2-143">Hello erőforrás létrehozása néhány másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5adc2-143">Creating hello resource takes a few seconds.</span></span> <span data-ttu-id="5adc2-144">Ha elkészült, akkor megjelenik egy riasztás.</span><span class="sxs-lookup"><span data-stu-id="5adc2-144">You'll see an alert when it's done.</span></span>

<span data-ttu-id="5adc2-145">(Írhat egy [PowerShell-parancsfájl](app-insights-powershell-script-create-resource.md) toocreate erőforrás automatikusan.)</span><span class="sxs-lookup"><span data-stu-id="5adc2-145">(You can write a [PowerShell script](app-insights-powershell-script-create-resource.md) toocreate a resource automatically.)</span></span>

### <a name="getting-hello-instrumentation-key"></a><span data-ttu-id="5adc2-146">Hello instrumentation kulcs beolvasása</span><span class="sxs-lookup"><span data-stu-id="5adc2-146">Getting hello instrumentation key</span></span>
<span data-ttu-id="5adc2-147">hello instrumentation kulcs hello erőforrás létrehozott azonosítja.</span><span class="sxs-lookup"><span data-stu-id="5adc2-147">hello instrumentation key identifies hello resource that you created.</span></span> 

![Essentials kattintson, majd hello Instrumentation kulcs CTRL + C](./media/app-insights-separate-resources/02-props.png)

<span data-ttu-id="5adc2-149">Hello instrumentation kulcsok van szüksége az összes hello erőforrások toowhich az alkalmazás elküldi az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5adc2-149">You need hello instrumentation keys of all hello resources toowhich your app will send data.</span></span>

## <a name="filter-on-build-number"></a><span data-ttu-id="5adc2-150">Szűrhet buildszám</span><span class="sxs-lookup"><span data-stu-id="5adc2-150">Filter on build number</span></span>
<span data-ttu-id="5adc2-151">Ha közzéteszi a az alkalmazás egy új verziója, érdemes toobe képes tooseparate hello telemetriai adatokat a különböző környezetek.</span><span class="sxs-lookup"><span data-stu-id="5adc2-151">When you publish a new version of your app, you'll want toobe able tooseparate hello telemetry from different builds.</span></span>

<span data-ttu-id="5adc2-152">Beállíthatja a hello Alkalmazásverzió tulajdonságát, hogy szűrheti [keresési](app-insights-diagnostic-search.md) és [metrika explorer](app-insights-metrics-explorer.md) eredmények.</span><span class="sxs-lookup"><span data-stu-id="5adc2-152">You can set hello Application Version property so that you can filter [search](app-insights-diagnostic-search.md) and [metric explorer](app-insights-metrics-explorer.md) results.</span></span>

![A tulajdonság szűrése](./media/app-insights-separate-resources/050-filter.png)

<span data-ttu-id="5adc2-154">Többféleképpen különböző hello Alkalmazásverzió tulajdonság beállítását.</span><span class="sxs-lookup"><span data-stu-id="5adc2-154">There are several different methods of setting hello Application Version property.</span></span>

* <span data-ttu-id="5adc2-155">Közvetlenül beállítani:</span><span class="sxs-lookup"><span data-stu-id="5adc2-155">Set directly:</span></span>

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* <span data-ttu-id="5adc2-156">A sor burkolása egy [telemetriai inicializáló](app-insights-api-custom-events-metrics.md#defaults) példányainak TelemetryClient következetesen beállított tooensure.</span><span class="sxs-lookup"><span data-stu-id="5adc2-156">Wrap that line in a [telemetry initializer](app-insights-api-custom-events-metrics.md#defaults) tooensure that all TelemetryClient instances are set consistently.</span></span>
* <span data-ttu-id="5adc2-157">[AZ ASP.NET] Hello verzió beállítása a `BuildInfo.config`.</span><span class="sxs-lookup"><span data-stu-id="5adc2-157">[ASP.NET] Set hello version in `BuildInfo.config`.</span></span> <span data-ttu-id="5adc2-158">hello webmodul felveszi hello verzió hello BuildLabel csomópontból.</span><span class="sxs-lookup"><span data-stu-id="5adc2-158">hello web module will pick up hello version from hello BuildLabel node.</span></span> <span data-ttu-id="5adc2-159">Ez a fájl tartalmazza a projekt, és ne feledje tooset hello másolási mindig tulajdonság a Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="5adc2-159">Include this file in your project and remember tooset hello Copy Always property in Solution Explorer.</span></span>

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* <span data-ttu-id="5adc2-160">[AZ ASP.NET] Automatikusan generálhat a BuildInfo.config MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5adc2-160">[ASP.NET] Generate BuildInfo.config automatically in MSBuild.</span></span> <span data-ttu-id="5adc2-161">toodo, adja hozzá a sor néhány tooyour `.csproj` fájlt:</span><span class="sxs-lookup"><span data-stu-id="5adc2-161">toodo this, add a few lines tooyour `.csproj` file:</span></span>

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    <span data-ttu-id="5adc2-162">Ezt követően nevű fájlba *com.example.baidutest*. BuildInfo.config. hello közzétételi folyamat tooBuildInfo.config átnevezi.</span><span class="sxs-lookup"><span data-stu-id="5adc2-162">This generates a file called *yourProjectName*.BuildInfo.config. hello Publish process renames it tooBuildInfo.config.</span></span>

    <span data-ttu-id="5adc2-163">hello build címke tartalmaz egy helyőrző (AutoGen_...), a Visual Studio összeállításakor.</span><span class="sxs-lookup"><span data-stu-id="5adc2-163">hello build label contains a placeholder (AutoGen_...) when you build with Visual Studio.</span></span> <span data-ttu-id="5adc2-164">De MSBuild-val készült, ha a telepítéskor hello helyes verziószáma.</span><span class="sxs-lookup"><span data-stu-id="5adc2-164">But when built with MSBuild, it is populated with hello correct version number.</span></span>

    <span data-ttu-id="5adc2-165">MSBuild toogenerate verziószámok tooallow, például a hello verzió beállítása `1.0.*` a AssemblyReference.cs</span><span class="sxs-lookup"><span data-stu-id="5adc2-165">tooallow MSBuild toogenerate version numbers, set hello version like `1.0.*` in AssemblyReference.cs</span></span>

## <a name="version-and-release-tracking"></a><span data-ttu-id="5adc2-166">Verzió- és kiadáskövetés</span><span class="sxs-lookup"><span data-stu-id="5adc2-166">Version and release tracking</span></span>
<span data-ttu-id="5adc2-167">tootrack hello Alkalmazásverzió, győződjön meg arról, hogy `buildinfo.config` a Microsoft Build motor folyamat által generált.</span><span class="sxs-lookup"><span data-stu-id="5adc2-167">tootrack hello application version, make sure `buildinfo.config` is generated by your Microsoft Build Engine process.</span></span> <span data-ttu-id="5adc2-168">A .csproj fájlban adja hozzá a következőt:</span><span class="sxs-lookup"><span data-stu-id="5adc2-168">In your .csproj file, add:</span></span>  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

<span data-ttu-id="5adc2-169">Ha rendelkezik hello build adatai, hello Application Insights webalkalmazás-modul automatikusan hozzáadja a **Alkalmazásverzió** telemetriai adatot tulajdonság tooevery elemként.</span><span class="sxs-lookup"><span data-stu-id="5adc2-169">When it has hello build info, hello Application Insights web module automatically adds **Application version** as a property tooevery item of telemetry.</span></span> <span data-ttu-id="5adc2-170">Amely lehetővé teszi toofilter verziójával végrehajtásakor [diagnosztikai keresések](app-insights-diagnostic-search.md), vagy ha, [metrikák megismerkedhet](app-insights-metrics-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="5adc2-170">That allows you toofilter by version when you perform [diagnostic searches](app-insights-diagnostic-search.md), or when you [explore metrics](app-insights-metrics-explorer.md).</span></span>

<span data-ttu-id="5adc2-171">Figyelje meg, hogy hello buildszámának generálja csak a Visual Studio build Microsoft Build motor, nem a hello fejlesztői hello.</span><span class="sxs-lookup"><span data-stu-id="5adc2-171">However, notice that hello build version number is generated only by hello Microsoft Build Engine, not by hello developer build in Visual Studio.</span></span>

### <a name="release-annotations"></a><span data-ttu-id="5adc2-172">Kiadási jegyzetek</span><span class="sxs-lookup"><span data-stu-id="5adc2-172">Release annotations</span></span>
<span data-ttu-id="5adc2-173">Ha használja a Visual Studio Team Services, akkor [egy jegyzet jelölő beolvasása](app-insights-annotations.md) tooyour diagramok hozzáadni, ha egy új kiadásból.</span><span class="sxs-lookup"><span data-stu-id="5adc2-173">If you use Visual Studio Team Services, you can [get an annotation marker](app-insights-annotations.md) added tooyour charts whenever you release a new version.</span></span> <span data-ttu-id="5adc2-174">hello a következő kép bemutatja, hogyan jelenjen meg ez a jelölő.</span><span class="sxs-lookup"><span data-stu-id="5adc2-174">hello following image shows how this marker appears.</span></span>

![Diagramon található példa kiadási jegyzet képernyőképe](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a><span data-ttu-id="5adc2-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5adc2-176">Next steps</span></span>

* [<span data-ttu-id="5adc2-177">Több szerepkör megosztott erőforrások</span><span class="sxs-lookup"><span data-stu-id="5adc2-177">Shared resources for multiple roles</span></span>](app-insights-monitor-multi-role-apps.md)
* [<span data-ttu-id="5adc2-178">Hozzon létre egy Telemetriai inicializáló toodistinguish A |} B Variant típusú adatok</span><span class="sxs-lookup"><span data-stu-id="5adc2-178">Create a Telemetry Initializer toodistinguish A|B variants</span></span>](app-insights-api-filtering-sampling.md#add-properties)
