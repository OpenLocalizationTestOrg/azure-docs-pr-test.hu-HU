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
# <a name="separating-telemetry-from-development-test-and-production"></a>Telemetria mappától fejlesztői, tesztelési és éles

Hello egy webes alkalmazás a következő verziójában fejleszt, ha nem szeretné, hogy másolatot hello toomix [Application Insights](app-insights-overview.md) hello új és hello már kiadott verziója a telemetriai adatokat. tooavoid zavart küldési hello telemetriai különböző fejlesztési tooseparate Application Insights-erőforrások, előkészítette a külön instrumentation kulcsokkal (ikeys). toomake azt könnyebb toochange hello instrumentation kulcs verzióként helyez át egy lépésben tooanother lehet hasznos tooset hello ikey kódban helyett hello konfigurációs fájlban. 

(Ha a rendszer egy Azure-Felhőszolgáltatásban nincs [más módszerrel kell beállítania külön ikeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Erőforrások és a rendszerállapot-kulcsok

Az Application Insights a webalkalmazás figyelésének beállításakor hoz létre az Application Insights *erőforrás* a Microsoft Azure-ban. Nyissa meg az erőforrás hello Azure portálra az order toosee, és az alkalmazásban gyűjtött hello telemetriáját elemzése. hello erőforrás azonosít egy *instrumentation kulcs* (ikey). Hello telepítésekor az Application Insights csomagot toomonitor az alkalmazást, konfigurálta hello instrumentation kulccsal, így az tudni fogja, ahol toosend hello telemetriai adatokat.

Általában választja toouse külön erőforrások vagy egy megosztott erőforrás különböző helyzetekben:

* Különböző, független alkalmazások - alkalmazásokra vonatkozó különálló erőforrás és ikey használata.
* Több összetevő vagy egy üzleti alkalmazás - szerepkörök használja egy [egyetlen megosztott erőforrás](app-insights-monitor-multi-role-apps.md) az összes összetevő alkalmazások hello. Telemetriai adatok szűrhetők és szegmentált hello cloud_RoleName tulajdonság.
* Fejlesztői, tesztelési és verzió - használata egy külön erőforrás és ikey verziók hello rendszer "stamp" vagy az éles szakasza.
* A |} B tesztelés - azonosítónak egyetlen erőforrásra használja. Hozzon létre egy TelemetryInitializer tooadd egy tulajdonság toohello telemetriai adat, amely azonosítja a hello Variant adattípusban.


## <a name="dynamic-ikey"></a>Dinamikus instrumentation kulcs

toomake adathalmazokban toochange hello ikey hello kódú helyezi át az éles, szakaszainak között állíthatja ezt be helyett kódban hello konfigurációs fájlban.

Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból hello kulcs meg:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

Ebben a példában a hello különböző erőforrásokat hello ikeys hello webes konfigurációs fájl különböző verzióinak kerülnek. Tárhellyel való felcserélése hello webes konfigurációs fájl – hello feloldási parancsprogram részeként teheti - fog felcserélése hello cél erőforráson.

### <a name="web-pages"></a>Weblapok
hello iKey is használatban van az alkalmazás weblapokat, hello [portáltól kapott hello – első lépések panelen parancsfájl](app-insights-javascript.md). Helyett megadás szó hello parancsfájlba, készítése hello állapotból. Például egy ASP.NET alkalmazásban:

*JavaScript Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>További Application Insights-erőforrások létrehozása
tooseparate telemetriai különböző alkalmazás-összetevők, vagy másik bélyegzők (fejlesztési/tesztelési/éles) hello azonos összetevő, akkor kell toocreate egy új Application Insights-erőforrást.

A hello [portal.azure.com](https://portal.azure.com), vegyen fel egy Application Insights-erőforrást:

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-separate-resources/01-new.png)

* **Az alkalmazástípus** elemnél mi jelenik hello áttekintése panel és hello tulajdonságok érhetők el [metrika explorer](app-insights-metrics-explorer.md). Ha nem látja a típusú alkalmazást, válasszon egyet az hello webes típusok weblapokra vonatkozóan.
* **Erőforráscsoport** kezeléséhez a tulajdonságokat, például a könnyebb van [hozzáférés-vezérlés](app-insights-resources-roles-access-control.md). Fejlesztési, tesztelési és éles külön erőforráscsoportok használható.
* **Előfizetés** a fizetési fiók az Azure-ban.
* **Hely** van, ahol azt megőrizni az adatokat. Jelenleg nem módosítható. 
* **Adja hozzá a toodashboard** helyezi az erőforrás egy gyors elérést csempe az Azure kezdőlapján. 

Hello erőforrás létrehozása néhány másodpercet vesz igénybe. Ha elkészült, akkor megjelenik egy riasztás.

(Írhat egy [PowerShell-parancsfájl](app-insights-powershell-script-create-resource.md) toocreate erőforrás automatikusan.)

### <a name="getting-hello-instrumentation-key"></a>Hello instrumentation kulcs beolvasása
hello instrumentation kulcs hello erőforrás létrehozott azonosítja. 

![Essentials kattintson, majd hello Instrumentation kulcs CTRL + C](./media/app-insights-separate-resources/02-props.png)

Hello instrumentation kulcsok van szüksége az összes hello erőforrások toowhich az alkalmazás elküldi az adatokat.

## <a name="filter-on-build-number"></a>Szűrhet buildszám
Ha közzéteszi a az alkalmazás egy új verziója, érdemes toobe képes tooseparate hello telemetriai adatokat a különböző környezetek.

Beállíthatja a hello Alkalmazásverzió tulajdonságát, hogy szűrheti [keresési](app-insights-diagnostic-search.md) és [metrika explorer](app-insights-metrics-explorer.md) eredmények.

![A tulajdonság szűrése](./media/app-insights-separate-resources/050-filter.png)

Többféleképpen különböző hello Alkalmazásverzió tulajdonság beállítását.

* Közvetlenül beállítani:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* A sor burkolása egy [telemetriai inicializáló](app-insights-api-custom-events-metrics.md#defaults) példányainak TelemetryClient következetesen beállított tooensure.
* [AZ ASP.NET] Hello verzió beállítása a `BuildInfo.config`. hello webmodul felveszi hello verzió hello BuildLabel csomópontból. Ez a fájl tartalmazza a projekt, és ne feledje tooset hello másolási mindig tulajdonság a Solution Explorer.

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
* [AZ ASP.NET] Automatikusan generálhat a BuildInfo.config MSBuild. toodo, adja hozzá a sor néhány tooyour `.csproj` fájlt:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Ezt követően nevű fájlba *com.example.baidutest*. BuildInfo.config. hello közzétételi folyamat tooBuildInfo.config átnevezi.

    hello build címke tartalmaz egy helyőrző (AutoGen_...), a Visual Studio összeállításakor. De MSBuild-val készült, ha a telepítéskor hello helyes verziószáma.

    MSBuild toogenerate verziószámok tooallow, például a hello verzió beállítása `1.0.*` a AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Verzió- és kiadáskövetés
tootrack hello Alkalmazásverzió, győződjön meg arról, hogy `buildinfo.config` a Microsoft Build motor folyamat által generált. A .csproj fájlban adja hozzá a következőt:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Ha rendelkezik hello build adatai, hello Application Insights webalkalmazás-modul automatikusan hozzáadja a **Alkalmazásverzió** telemetriai adatot tulajdonság tooevery elemként. Amely lehetővé teszi toofilter verziójával végrehajtásakor [diagnosztikai keresések](app-insights-diagnostic-search.md), vagy ha, [metrikák megismerkedhet](app-insights-metrics-explorer.md).

Figyelje meg, hogy hello buildszámának generálja csak a Visual Studio build Microsoft Build motor, nem a hello fejlesztői hello.

### <a name="release-annotations"></a>Kiadási jegyzetek
Ha használja a Visual Studio Team Services, akkor [egy jegyzet jelölő beolvasása](app-insights-annotations.md) tooyour diagramok hozzáadni, ha egy új kiadásból. hello a következő kép bemutatja, hogyan jelenjen meg ez a jelölő.

![Diagramon található példa kiadási jegyzet képernyőképe](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Következő lépések

* [Több szerepkör megosztott erőforrások](app-insights-monitor-multi-role-apps.md)
* [Hozzon létre egy Telemetriai inicializáló toodistinguish A |} B Variant típusú adatok](app-insights-api-filtering-sampling.md#add-properties)
