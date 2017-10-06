---
title: "Azure-szolgáltatásokhoz Insights aaaApplication |} Microsoft Docs"
description: "Webes és feldolgozói szerepkörök hatékony figyelése az Application Insightsszal"
services: application-insights
documentationcenter: 
keywords: WAD2AI, Azure Diagnostics
author: CFreemanwa
manager: carmonm
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: get-started-article
ms.workload: tbd
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 6956ce423eea1e2cf387bd98250bae32d9501ed0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights az Azure Cloud Servicesben
Az [Application Insightsszal][start]monitorozható a [Microsoft Azure felhőszolgáltatásbeli alkalmazások](https://azure.microsoft.com/services/cloud-services/) rendelkezésre állása, teljesítménye, hibái és használata az Application Insights SDK-iból származó adatok és a felhőszolgáltatások [Azure Diagnostics](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics)-adatainak kombinálása révén. Hello visszajelzést kap hello teljesítmény és az alkalmazást a hello hatékonyságát helyettesítő minden fejlesztési életciklus során hello tervezési hello irányának kapcsolatos megalapozott döntések tehet.

![Példa](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Előkészületek
A következők szükségesek:

* Egy [Microsoft Azure](http://azure.com)-előfizetés. Jelentkezzen be egy Microsoft-fiókkal – ez tartozhat a Windowshoz, az XBox Live-hoz vagy egyéb Microsoft felhőszolgáltatásokhoz. 
* A Microsoft Azure eszközök 2.9-es vagy újabb verziója
* A Developer Analytics Tools 7.10-es vagy újabb verziója

## <a name="quick-start"></a>Első lépések
a felhőalapú szolgáltatás, az Application insights szolgáltatással, amely a szolgáltatás tooAzure közzétételekor lehetőséget toochoose leggyorsabb és legegyszerűbb módja toomonitor hello.

![Példa](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Ez az alkalmazás futásidőben, felkínálva összes hello telemetriai adat toomonitor kéréseket, kivételeket és a webes szerepkör, valamint a teljesítmény függőségek van szüksége a feldolgozói szerepkörök a teljesítményszámlálók beállítás eszközök. Az alkalmazás által generált bármely diagnosztikai nyomkövetési tooApplication Insights küld.

Ha csak ennyire van szüksége, már kész is van! A következő lépések [az alkalmazás mérőszámainak megtekintése](app-insights-metrics-explorer.md), [az adatok lekérdezése az Analytics használatával](app-insights-analytics.md), valamint esetleg egy [irányítópult](app-insights-dashboards.md) beállítása. Érdemes tooset mentése [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md) és [adja hozzá a kódot tooyour weblapok](app-insights-javascript.md) toomonitor teljesítmény hello böngészőben.

Azonban további lehetőségeket is elérhet:

* Adatok küldése a különböző összetevőkből, és hozzon létre konfigurációk tooseparate erőforrásokat.
* Egyéni telemetriát adhat hozzá az alkalmazásból.

Ha ezek a lehetőségek érdeklődési tooyou, olvassa el a.

## <a name="sample-application-instrumented-with-application-insights"></a>Az Application Insights révén utasított Alkalmazás minta
Vessen egy pillantást, ez [mintaalkalmazás](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) , amelyben az Application Insights hozzá tooa felhőszolgáltatás rendelkező Azure-ban üzemeltetett két feldolgozói szerepköröket. 

Mi a következő megtudhatja, hogyan tooadapt a saját felhőalapú szolgáltatás projekt hello a megszokott módon.

## <a name="plan-resources-and-resource-groups"></a>Erőforrások és erőforráscsoportok tervezése
az alkalmazásból hello telemetriai tárolt, elemzése, és megjelenik egy Azure típus Application Insights-erőforrás. 

Minden erőforrás tooa erőforráscsoporthoz tartozik. Erőforráscsoportok tooteam tagok hozzáférés biztosításához a költségek kezelésére használt, és toodeploy frissíti egy koordinált tranzakción belül. Például sikerült [írása egy parancsfájl toodeploy](../azure-resource-manager/resource-group-template-deploy.md) Azure Cloud Service és az Application Insights figyelési erőforrások egy műveletben.

### <a name="resources-for-components"></a>Az összetevők erőforrásai
hello ajánlott séma toocreate az egyes összetevők az alkalmazás - Ez azt jelenti, hogy minden webes és feldolgozói szerepkör külön erőforrás. Minden összetevőt külön-külön elemezni tudja, de létrehozhat egy [irányítópult](app-insights-dashboards.md) , amely egyesíti hello kulcs diagramok összes hello összetevői, így összehasonlítása, és figyelheti azokat együtt. 

Egy másik program a több szerepkört toohello toosend hello telemetriai ugyanarra az erőforrásra, de [dimenzió tulajdonság tooeach telemetriai elem hozzáadása](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer) , amely azonosítja a forrás szerepkör. A séma metrika diagramokat, például a kivételek rendszerint megjelenítése hello számát a hello összesítését különböző szerepkörök, de szükség esetén hello szerepkör-azonosító akkor is szegmentálja hello diagram. A keresések szűrni hello is lehet ugyanazt a dimenziót. Ez a megoldás lehetővé teszi egy kicsit könnyebb tooview mindent: hello azonos időben, de is vezethet toosome zavart hello szerepkörök között.

Böngésző telemetriai általában megtalálható hello ugyanazt az erőforrást a kiszolgálóoldali webes szerepkörben.

Application Insights-erőforrások hello hello különböző összetevők be egy erőforráscsoport. Így könnyen toomanage együtt. 

### <a name="separating-development-test-and-production"></a>A fejlesztési, tesztelési és éles környezetek elkülönítése
Fejlesztői egyéni események a következő szolgáltatás élő hello előző verzió pedig, ha azt szeretné, toosend hello fejlesztési telemetriai tooa külön Application Insights-erőforrást. Rögzített toofind lesz ellenkező esetben a teszt telemetriai közötti összes forgalom hello élő helyről hello.

tooavoid ebben a helyzetben minden build konfigurációját vagy a "bélyegző" (fejlesztési, tesztelési, éles,...) a rendszer külön erőforrásokat létrehozni. Erőforrásaikat hello minden build konfigurációját egy külön erőforráscsoportban. 

toosend hello telemetriai toohello megfelelő erőforrásokkal, állíthat be hello Application Insights SDK, hogy azt szerzi be egy másik instrumentation kulcs hello build konfigurációjától függően. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Application Insights-erőforrás létrehozása mindegyik szerepkörhöz
Ha úgy döntött, hogy az egyes szerepkörökhöz - egy külön erőforrás toocreate és lehet, hogy egy külön minden build konfigurációját - állítsa be, majd legegyszerűbb toocreate minden a hello Application Insights portál őket. (Ha erőforrások sokkal hoz létre, akkor [hello folyamat automatizálása](app-insights-powershell.md).

1. A hello [Azure-portálon][portal], hozzon létre egy új Application Insights-erőforrást. Az alkalmazás típusánál válassza az ASP.NET alkalmazás lehetőséget. 

    ![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-cloudservices/01-new.png)
2. Fontos észrevenni, hogy mindegyik erőforrást egy kialakítási kulcs azonosítja. Előfordulhat, hogy később szüksége Ha azt szeretné, hogy toomanually konfigurálja vagy ellenőrizze a hello SDK hello konfigurációját.

    ![Kattintson a Tulajdonságok parancsra, válassza ki a hello kulcs, használja a ctrl + C](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Azure Diagnostics beállítása az egyes szerepkörökhöz
Ez a beállítás toomonitor állítsa be az alkalmazás az Application insights szolgáltatással. Webes szerepkörök esetében ez biztosítja a teljesítményfigyelést, a riasztásokat és a diagnosztikát, valamint a használat elemzését. Más szerepköreivel kapcsolatban megkeresheti és az Azure diagnostics például újraindítása, a teljesítményszámlálók és a hívások tooSystem.Diagnostics.Trace figyelése. 

1. A Visual Studio Solution Explorer alatt &lt;YourCloudService&gt;, szerepkörök, nyissa meg az egyes szerepkörökhöz hello tulajdonságait.
2. A **konfigurációs**, beállíthatja **elküldeni a diagnosztikai adatok tooApplication Insights** , és válassza ki a korábban létrehozott megfelelő Application Insights-erőforrás hello.

Ha úgy döntött toouse minden build konfigurációját egy külön Application Insights-erőforrást, először válassza a hello konfigurálását.

![Minden Azure szerepkör hello tulajdonságainak konfigurálja az Application Insights](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Ennek hatása hello az Application Insights instrumentation kulcsok beszúrása hello nevű fájlt a `ServiceConfiguration.*.cscfg`. ([Mintakód](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

Ha diagnosztikai adatokat küld tooApplication Insights toovary hello szintjét, ehhez [hello szerkesztésével `.cscfg` közvetlenül fájlok](app-insights-azure-diagnostics.md).

## <a name="sdk"></a>Minden olyan projekthez hello SDK telepítése
Ezt a beállítást hozzáad hello képességét tooadd egyéni üzleti telemetriai tooany szerepkört, hogyan az alkalmazás használja, és végez szorosabb elemzése.

A Visual Studio hello Application Insights SDK minden felhő alkalmazás projekt konfigurálása

1. **Webalkalmazás-szerepkörök**: kattintson a jobb gombbal a hello projektet, és válassza a **konfigurálja az Application Insights** vagy **Hozzáadás > Application Insights telemetria**.

2. **Feldolgozói szerepkörök**: 
 * Kattintson a jobb gombbal a hello projektet, és válassza ki **Nuget-csomagok kezelése**.
 * Adja hozzá az [Application Insights a Windows-kiszolgálókon](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) modult.

    ![Az „Application Insights” kifejezés keresése](./media/app-insights-cloudservices/04-ai-nuget.png)

3. Hello SDK toosend adatok toohello Application Insights-erőforrás konfigurálása.

    A megfelelő indítási függvényben, hello instrumentation kulcsát állítsa a hello konfigurációs beállítás hello .cscfg fájlban:
 
    ```C#
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Ezt tegye meg az alkalmazás minden szerepköre esetében. Hello példát látható:
   
   * [Webes szerepkör](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Feldolgozói szerepkör](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [Weblapok esetében](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 
4. Set hello ApplicationInsights.config fájl toobe másolt mindig toohello kimeneti könyvtár. 
   
    (Hello .config kiterjesztésű fájl, látni fogja, üzenetben kéri az érintett meg tooplace hello instrumentation kulcs van. Azonban a felhőalapú alkalmazások esetén jobb tooset azt hello .cscfg fájlból. Ez biztosítja, hogy hello szerepkör helyesen azonosítani hello portálon.)

#### <a name="run-and-publish-hello-app"></a>Futtassa a és hello alkalmazás közzététele
Futtassa az alkalmazást, és jelentkezzen be az Azure-ba. Megnyitás hello Application Insights-erőforrások hozott létre, és látni fogja, az egyes adatpontok szereplő [keresési](app-insights-diagnostic-search.md), majd összesíti azokat az adatokat [metrika Explorer](app-insights-metrics-explorer.md). 

Adja hozzá a további telemetria - alábbi - hello szakaszban talál, és tegye közzé az alkalmazást tooget élő diagnosztikai és használati visszajelzését. 

#### <a name="no-data"></a>Nincs adat?
* Nyissa meg hello [keresési] [ diagnostic] csempére, toosee események.
* Hello alkalmazást, a különböző oldalakhoz megnyitása, hogy néhány telemetriai generál használni.
* Várjon néhány másodpercet, és kattintson a Frissítés lehetőségre.
* Lásd: [Hibaelhárítás][qna].

## <a name="view-azure-diagnostic-events"></a>Azure diagnosztikai események megtekintése
Ha toofind hello [Azure Diagnostics](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics) az Application Insightsban információkat:

* A teljesítményszámlálók egyéni mérőszámokként jelennek meg. 
* A Windows eseménynaplók nyomkövetésekként és egyéni eseményekként jelennek meg.
* Az alkalmazásnaplók, ETW-naplók és egyéb diagnosztikai infrastruktúra-naplók nyomkövetésként jelennek meg.

toosee teljesítményszámlálók és számát is események, nyissa meg a [Metrikaböngésző](app-insights-metrics-explorer.md) és új diagram hozzáadása:

![Az Azure diagnosztikai adatai](./media/app-insights-cloudservices/23-wad.png)

Használjon [keresési](app-insights-diagnostic-search.md) vagy egy [Analytics lekérdezési](app-insights-analytics-tour.md) toosearch különböző nyomkövetési naplók Azure Diagnostics által küldött hello között. Tegyük fel például, egy szerepkör toocrash és újrahasznosítást okozó nem kezelt kivétel. Ezt az információt akkor jelenik meg a hello alkalmazások csatorna a Windows eseménynaplójában. Használja a keresési toolook hello Windows eseménynaplóban, és teljes Veremkivonat hello lekérése hello kivétel. Amely segít hello a probléma okának hello található.

![Azure Diagnostics-keresés](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>További telemetria
hello megjelenítése szakaszokat hogyan tooget az alkalmazás különböző aspektusainak a további telemetriai adatokat.

## <a name="track-requests-from-worker-roles"></a>Kérések nyomon követése a feldolgozói szerepkörökből
A webes szerepkörök hello kérelmek modul automatikusan HTTP-kérelmek adatokat gyűjt. Lásd: hello [MVCWebRole minta](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) példákat arra, hogyan lehet felülbírálni hello alapértelmezett gyűjtemény viselkedését. 

Hello teljesítmény hívások tooworker szerepkörök rögzítést hello a nyomon követés ugyanúgy, mint a HTTP-kérelmekre. Az Application Insights telemetria kérelemtípus hello munkaegység elnevezett kiszolgálóoldali is időtúllépés miatt megszakadt, és egymástól függetlenül sikeres vagy sikertelen méri. HTTP-kérelmek hello SDK által automatikusan rögzített, amíg a saját kód tootrack kérelmek tooworker szerepkörök is beszúrhat.

Lásd: hello két minta feldolgozói szerepkörök felműszerezett tooreport kérelmek: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) és [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Kivételek
A nem kezelt kivételek a különféle webalkalmazás-típusokból való gyűjtésével kapcsolatban lásd: [Kivételek figyelése az Application Insightsban](app-insights-asp-net-exceptions.md).

hello minta webalkalmazás szerepkörhöz MVC5-és a Web API 2. hello hello két a nem kezelt kivételek a rendszer rögzíti a kezelők a következő hello:

* Az MVC5-vezérlőkhöz az [itt](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) beállított [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs)
* A Web API 2-vezérlőkhöz az [itt](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) beállított [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs)

A feldolgozói szerepkörök kétféleképpen tootrack kivételek:

* TrackException(ex)
* Ha hozzáadta hello Application Insights nyomkövetési figyelő NuGet-csomagot, **System.Diagnostics.Trace** toolog kivételek. [Mintakód.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## <a name="performance-counters"></a>Teljesítményszámlálók
a következő számlálók hello alapértelmezés szerint gyűjtött a program:

    * \Process(??APP_WIN32_PROC??)\% A processzor kihasználtsága
    * \Memory\Available Bytes
    * \.NET CLR-kivételek (??APP_CLR_PROC??)\# az összes kivétel közül másodpercenként
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\% Processor Time

Webes szerepkörök esetében a rendszer az alábbi számlálókat is gyűjti:

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

Megadhat további egyéni vagy egyéb Windows-teljesítményszámlálókat is az ApplicationInsights.config szerkesztésével, [ahogy ebben a példában is](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14).

  ![Teljesítményszámlálók](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>A feldolgozói szerepkörök korrelált telemetriája
Az esetén sorával gazdag diagnosztikai élményt, láthatja, milyen sikertelen következtében tooa vagy nagy késleltetésű kérelem. A webes szerepkörök hello SDK automatikusan beállítja a két kapcsolódó telemetriai. A feldolgozói szerepkörök ezzel egy egyéni telemetria inicializáló tooset egy közös Operation.Id context attribútum az összes hello telemetriai tooachieve. Ez lehetővé teszi toosee e hello késést és sikertelen probléma miatt tooa függőségeknek vagy a kód egy pillantással okozott! 

Ezt a következőképpen teheti meg:

* Hello korrelációs azonosító egy CallContext történő beállítása látható [Itt](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36). Ebben az esetben használjuk hello Kérelemazonosító hello korrelációs azonosító
* Adjon hozzá egy egyéni TelemetryInitializer megvalósításban tooset hello Operation.Id toohello correlationId fent. Íme egy példa: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Adja hozzá a hello egyéni telemetria inicializálásakor. Ön tehetné hello ApplicationInsights.config fájlban, vagy a kódban látható módon [Itt](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

Ennyi az egész! hello portál élmény van már vezetékes toohelp másolatot minden kapcsolódó telemetriai egy pillanat alatt látható:

![Korrelált telemetria](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>Ügyfél-telemetria
[Adja hozzá a JavaScript SDK tooyour weblapok hello] [ client] tooget böngészőalapú telemetriai például nézet oldalszám, lapbetöltési idők, parancsprogram-kivételei és egyéni telemetriai adatokat ír az oldal parancsfájlok toolet.

## <a name="availability-tests"></a>Rendelkezésre állási tesztek
[Webalkalmazás-tesztek beállítása] [ availability] toomake meg arról, hogy az alkalmazás élő és rugalmas marad.

## <a name="display-everything-together"></a>Az összes elem együttes megjelenítése
a rendszer átfogó képet tooget, a figyelési diagramok együtt egy hello kulcs helyezheti [irányítópult](app-insights-dashboards.md). Például hello kérést sikerült rögzíteni, és hiba száma az egyes szerepkörökhöz. 

Ha a rendszer egyéb Azure-szolgáltatásokat (például Stream Analytics) is tartalmaz, ezeknek a figyelési diagramjait is beillesztheti. 

Ha egy ügyfél mobilalkalmazást, néhány kódot toosend egyéni események legfontosabb felhasználói műveletek beszúrása, és hozzon létre egy [Hockeyappra híd](app-insights-hockeyapp-bridge-app.md). Létrehozhat olyan lekérdezéseket, a [Analytics](app-insights-analytics.md) toodisplay hello esemény számát, és toohello irányítópulton rögzítheti őket.

## <a name="example"></a>Példa
[hello példa](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) olyan szolgáltatás, amely rendelkezik a webes szerepkör és a két feldolgozói szerepkörök figyeli.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>„A metódus nem található” kivétel az Azure Cloud Servicesben futó rendszeren
A .NET 4.6-os verziójára készítette el az alkalmazást? Az Azure Cloud Services szerepkörei nem támogatják automatikusan a 4.6-os verziót. [Telepítse a 4.6-os verziót mindegyik szerepkörön](../cloud-services/cloud-services-dotnet-install-dotnet.md), mielőtt futtatná az alkalmazást.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Következő lépések
* [Konfigurálja az Azure Diagnostics tooApplication Insights küldése](app-insights-azure-diagnostics.md)
* [Application Insights-erőforrások létrehozásának automatizálása](app-insights-powershell.md)
* [Az Azure Diagnostics-diagnosztikák automatizálása](app-insights-powershell-azure-diagnostics.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 
