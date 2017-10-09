---
title: "aaaMonitor élő ASP.NET webalkalmazás az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "Megfigyelheti egy webhely teljesítményét annak ismételt üzembe helyezése nélkül. A helyszíni, valamint a virtuális gépeken, illetve az Azure-ban üzemeltetett ASP.NET-webappokhoz is használható."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: 0d53f0a59974f40767fae681bafc4f358d1283a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Webalkalmazások futásidejű kialakítása az Application Insights használatával


Egy élő webalkalmazását az Azure Application insights szolgáltatással állíthatnak be anélkül, hogy toomodify, vagy telepítse újra a kódot. Ha az alkalmazásokat egy helyszíni IIS-kiszolgáló működteti, telepítse az Állapotfigyelőt. Azure-webalkalmazásokban vagy egy Azure virtuális gép fut, az Application Insights általi figyelés hello Azure Vezérlőpultról válthat. (Külön cikkek érhetők el az [élő J2EE-webalkalmazások](app-insights-java-live.md) és az [Azure Cloud Services](app-insights-cloudservices.md) kialakításáról.) Ehhez [Microsoft Azure](http://azure.com)-előfizetésre van szükség.

![mintadiagramok](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

Megválaszthatja három útvonalak tooapply Application Insights tooyour .NET webes alkalmazások:

* **Build idő:** [Application Insights SDK hozzáadása hello] [ greenbrown] tooyour webes mintaalkalmazás kódját.
* **Futási idő:** állíthatnak be a webalkalmazás hello kiszolgálón, az alább ismertetett, újraépítését, és újbóli hello kód nélkül.
* **Mindkét:** hello SDK összeállítása a webes alkalmazás kódba, és hello futásidejű bővítmények verzióra is érvényes. Hello mindkét lehetőség a leghatékonyabb beolvasása.

Itt található egy összefoglaló az egyes módszerek eredményeiről:

|  | Felépítési idő | Futási idő |
| --- | --- | --- |
| Kérések és kivételek |Igen |Igen |
| [Részletes kivételek](app-insights-asp-net-exceptions.md) | |Igen |
| [Függőségek diagnosztikája](app-insights-asp-net-dependencies.md) |.NET 4.6+ esetén, kevésbé részletesen |Igen, teljes részletesség: eredménykódok, SQL-parancsszöveg, HTTP-parancsok|
| [Rendszerteljesítmény-számlálók](app-insights-performance-counters.md) |Igen |Igen |
| [API egyéni telemetriához][api] |Igen |Nem |
| [Nyomkövetési napló integrációja](app-insights-asp-net-trace-logs.md) |Igen |Nem |
| [Lapmegtekintések és felhasználói adatok](app-insights-javascript.md) |Igen |Nem |
| Toorebuild kód szükséges |Igen | Nem |


## <a name="monitor-a-live-azure-web-app"></a>Élő Azure-webapp figyelése

Ha az alkalmazás fut-e egy Azure webes szolgáltatás, itt, hogyan tooswitch figyeléséről:

* Válassza ki az Application Insights Vezérlőpultján hello alkalmazást az Azure-ban.

    ![Az Application Insights beállítása egy Azure-webapphoz](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* Hello Application Insights összefoglaló lap megnyitása után kattintson a hello hivatkozásra, hello alsó tooopen hello teljes Application Insights-erőforrást.

    ![Kattintson a tooApplication Insights](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

[Felhőben és virtuális gépeken futó alkalmazások figyelése](app-insights-azure.md).

### <a name="enable-client-side-monitoring-in-azure"></a>Az ügyféloldali figyelés engedélyezése az Azure-ban

Ha engedélyezte az Application Insights szolgáltatást az Azure-ban, felveheti a lapmegtekintéseket és a felhasználók telemetriai adatait.

1. Válassza a Beállítások > Alkalmazásbeállítások lehetőséget.
2.  Az alkalmazásbeállításoknál adjon meg egy új kulcs-érték párt: 
   
    Kulcs: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Érték: `true`
3. **Mentés** beállítások hello és **indítsa újra a** az alkalmazást.

Application Insights JavaScript SDK hello van most be a nézetmodellbe, minden weblapon.

## <a name="monitor-a-live-iis-web-app"></a>Élő IIS-webapp figyelése

Ha az alkalmazás egy IIS-kiszolgálón fut, engedélyezze az Application Insightst az Állapotfigyelő használatával.

1. Az IIS-webkiszolgálón jelentkezzen be rendszergazdai hitelesítő adatokkal.
2. Ha az Application Insights Állapotmonitor nincs telepítve, töltse le, és futtassa a hello [állapotfigyelő telepítő](http://go.microsoft.com/fwlink/?LinkId=506648) (vagy futtassa [Webplatform-telepítő](https://www.microsoft.com/web/downloads/platform.aspx) és a keresési azt az Application Insights állapotot A figyelő).
3. Állapotmonitorban válassza ki a telepített hello webes alkalmazás vagy webhely, amelyet az toomonitor. Jelentkezzen be az Azure-beli hitelesítő adataival.

    Konfigurálja a kívánt toosee hello eredmények hello Application Insights portál hello erőforrás. (Általában, akkor ajánlott toocreate egy új erőforrást. Meglévő erőforrást akkor válasszon, ha már rendelkezik [webes tesztekkel][availability] vagy [ügyfélfigyeléssel][client] az alkalmazáshoz.) 

    ![Válasszon egy alkalmazást és egy erőforrást.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. Indítsa újra az IIS-t.

    ![Válassza ki az újraindítás hello párbeszédpanel hello tetején.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    A webszolgáltatása rövid időre megszakad.

## <a name="customize-monitoring-options"></a>A megfigyelési beállítások testreszabása

Az Application Insights engedélyezése hozzáadja a dll-EK és ApplicationInsights.config tooyour webalkalmazás. Is [hello .config fájl szerkesztésével](app-insights-configuration-with-applicationinsights-config.md) toochange hello-beállítások egy része.

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a>Az Application Insights ismételt engedélyezése az alkalmazás ismételt közzétételekor

Mielőtt újra közzé az alkalmazást, érdemes lehet [a Visual Studio Application Insights toohello kód felvétele][greenbrown]. Részletes telemetria és hello képességét toowrite egyéni telemetriai adatokat fog kapni.

Ha azt szeretné, hogy toore-közzététele az Application Insights toohello kód hozzáadása nélkül, vegye figyelembe, hogy hello telepítési folyamatának törölhetik a hello dll-EK és hello az ApplicationInsights.config webhelyen közzétett. Ezért:

1. Ha szerkesztette az ApplicationInsights.config fájlt, készítsen róla egy másolatot, mielőtt újra közzéteszi az alkalmazást.
2. Tegye közzé újra az alkalmazást.
3. Engedélyezze újra az Application Insights-figyelést. (Használjon megfelelő módszert hello: hello Azure web app Vezérlőpult, vagy egy IIS állomáson állapotfigyelő hello.)
4. Végzett módosításokat végzett hello .config fájl visszaállítása.


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a>Az Application Insights futtatókörnyezet-konfigurációjának hibaelhárítása

### <a name="cant-connect-no-telemetry"></a>Nem tud csatlakozni? Nem működik a telemetria?

* Nyissa meg [szükséges kimenő portok hello](app-insights-ip-addresses.md#outgoing-ports) az a kiszolgáló tűzfal tooallow állapotfigyelő toowork.

* Nyissa meg az Állapotfigyelőt, és válassza ki az alkalmazását a bal oldali panelen. Ellenőrizze, hogy van-e az alkalmazás a hello "Konfigurációs értesítések" szakaszhoz diagnosztikai üzeneteket:

  ![Nyissa meg a hello teljesítmény panel toosee kérelem, válaszideje, függőség és egyéb adatok](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* Hello kiszolgálón Ha "nincs megfelelő engedélye" kapcsolatos egy üzenet jelenik meg, próbálkozzon hello következő:
  * Az IIS-kezelőben válassza ki az alkalmazáskészletet, nyissa meg a **speciális beállítások**, majd a **folyamatmodell** jegyezze fel hello identitását.
  * A számítógép-felügyeleti Vezérlőpult adja hozzá az identitás toohello Teljesítményfigyelő felhasználók csoportjába.
* Ha MMA/SCOM (Systems Center Operations Manager) van telepítve a kiszolgálón, néhány verzió esetében ütközés léphet fel. SCOM és a állapotfigyelő el, majd telepítse újra a hello legújabb verziói.
* Lásd: [Hibaelhárítás][qna].

## <a name="system-requirements"></a>Rendszerkövetelmények
Operációs rendszeri támogatás az Application Insights Állapotfigyelőhöz a kiszolgálón:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

a legújabb szervizcsomaggal és a .NET-keretrendszer 4.5-ös verziójával

Hello ügyféloldalon: Windows 7, 8, 8.1 és 10, újra a .NET-keretrendszer 4.5

IIS-támogatás: IIS 7, 7.5, 8, 8.5 (az IIS kötelező)

## <a name="automation-with-powershell"></a>Automatizálás a PowerShell használatával
A PowerShell a saját IIS-kiszolgálón való használatával elindíthatja és leállíthatja a figyelést.

Először importálni hello Application Insights modult:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Derítse ki, melyik alkalmazások állnak megfigyelés alatt:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name`A webes alkalmazás neve (nem kötelező) hello.
* Jeleníti meg az IIS-kiszolgálót az Application Insights figyelési állapotát az egyes web app (vagy alkalmazás nevű hello) hello.
* Az `ApplicationInsightsApplication` elemet adja vissza mindegyik alkalmazáshoz:

  * `SdkState==EnabledAfterDeployment`: Figyelt alkalmazás és a futási időben lett tagolva, vagy hello állapotfigyelő eszköz, vagy `Start-ApplicationInsightsMonitoring`.
  * `SdkState==Disabled`: hello app nem tagolva az Application insights szolgáltatással. Soha ne tagolva volt, vagy futásidejű figyelés le lett tiltva, hello állapotfigyelő eszközzel vagy `Stop-ApplicationInsightsMonitoring`.
  * `SdkState==EnabledByCodeInstrumentation`: hello alkalmazás lett tagolva hello SDK toohello forráskód hozzáadásával. Az SDK-ja nem frissíthető és nem állítható le.
  * `SdkVersion`Ez az alkalmazás figyeléséhez használja hello verzióját jelzi.
  * `LatestAvailableSdkVersion`jelenleg elérhető hello verzió látható hello NuGet gyűjteménye. tooupgrade hello toothis Alkalmazásverzió, használjon `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name`az IIS-ben hello alkalmazás hello nevét
* `-InstrumentationKey`az Application Insights-erőforrás hello eredmények toobe jelenik meg, ahová hello ikey hello.
* Ez a parancsmag csak olyan alkalmazásokra van hatással, amelyek még nincsenek kialakítva – vagyis amelyek esetében az SdkState==NotInstrumented.

    hello parancsmag nem befolyásolja az alkalmazások, amelyek már tagolva. Nem számít, hello app build időpontban, hello SDK toohello kódrészletet tagolva, és a futási időt egy előző Ez a parancsmag használatával.

    hello SDK használt verzió tooinstrument hello alkalmazás áll a legutóbb hello verzió letöltött toothis kiszolgáló.

    toodownload hello legújabb verziójára, használja a frissítés-ApplicationInsightsVersion.
* Siker esetén az `ApplicationInsightsApplication` elemet adja vissza. Ha nem sikerül, egy nyomkövetési toostderr naplózza.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name`az IIS alkalmazás hello neve
* `-All` Leállítja minden alkalmazás megfigyelését ezen az IIS-kiszolgálón, amely esetében az `SdkState==EnabledAfterDeployment`
* Leállítja a figyelést hello adott alkalmazást, és eltávolítja a instrumentation. Az alkalmazásokat, amelyek rendelkeznek lettek tagolva futásidejű az eszköz állapotának monitorozása vagy a Start-ApplicationInsightsApplication hello csak működik. (`SdkState==EnabledAfterDeployment`)
* Az ApplicationInsightsApplication elemet adja vissza.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: az IIS-ben a webes alkalmazás hello nevét.
* `-InstrumentationKey`(Választható.) Használja a toochange hello erőforrás toowhich hello alkalmazás telemetriai zajlik.
* Ez a parancsmag:
  * Frissítések hello nevű alkalmazás toohello verziója hello SDK legutóbb letöltött toothis gép. (Csak akkor működik, ha `SdkState==EnabledAfterDeployment`)
  * Ha megad egy rendszerállapot-kulcsot, nevű alkalmazás hello újra konfigurált toosend telemetriai toohello erőforrás ezzel a kulccsal. (Akkor működik, ha `SdkState != Disabled`)

`Update-ApplicationInsightsVersion`

* Letölti a legújabb Application Insights SDK toohello server hello.

## <a name="questions"></a>Kérdések az Állapotfigyelővel kapcsolatban

### <a name="what-is-status-monitor"></a>Mi az Állapotfigyelő?

Egy asztali alkalmazás, amelyet az IIS-webkiszolgálón kell telepítenie. Segít a webalkalmazások kialakításában és konfigurálásában. 

### <a name="when-do-i-use-status-monitor"></a>Mire használhatom az Állapotfigyelőt?

* a webalkalmazás, hogy fut az IIS-kiszolgáló - még akkor is, ha már fut. tooinstrument.
* tooenable további telemetriai adatokat is a web Apps [hello Application Insights SDK-val készült](app-insights-asp-net.md) fordítás során. 

### <a name="can-i-close-it-after-it-runs"></a>A futtatás után bezárhatom?

Igen. Ha az rendelkezik tagolva hello webhelyek választja, bezárhatja azt.

Az alkalmazás önmagától nem gyűjt telemetriai adatokat, Ebben az esetben hello webalkalmazások konfigurálja, és néhány engedélyeket állít be.

### <a name="what-does-status-monitor-do"></a>Milyen műveleteket hajt végre az Állapotfigyelő?

Ha bejelöli az állapotfigyelő tooinstrument webalkalmazás:

* Letölti és hello Application Insights szerelvények és .config fájl helyezi hello webes alkalmazás bináris fájljainak mappáját.
* Módosítja a `web.config` tooadd hello Application Insights HTTP követési modul.
* Lehetővé teszi, hogy a CLR-profilkészítés toocollect függőségi hívások esetében.

### <a name="do-i-need-toorun-status-monitor-whenever-i-update-hello-app"></a>Állapotfigyelő toorun amikor hello alkalmazást frissíteni kell?

Nem, ha az ismételt üzembe helyezés növekményesen történik. 

Ha hello "törölje a meglévő fájlokat" lehetőséget választja a hello folyamat közzétételéhez toore futtatási állapotfigyelő tooconfigure Application Insights kell.

### <a name="what-telemetry-is-collected"></a>A rendszer milyen telemetriai adatokat gyűjt?

Olyan alkalmazások esetén, amelyeket az Állapotfigyelővel kizárólag futásidőben állít be:

* HTTP-kérések
* Meghívja a toodependencies
* Kivételek
* Teljesítményszámlálók

A fordítási során már kiépített alkalmazások esetén:

 * Folyamatszámlálók.
 * Függőségi hívások (.NET 4.5); függőségi hívásokban visszaadott értékek (.NET 4.6).
 * Kivételhíváslánc-értékek.

[További információ](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next"></a>Következő lépések

A telemetriai adatok megtekintése:

* [Megismerkedhet a metrikák](app-insights-metrics-explorer.md) toomonitor teljesítmény- és használati
* [Keresést az események és a naplók] [ diagnostic] toodiagnose problémák
* [Elemzések](app-insights-analytics.md) az összetettebb lekérdezésekhez
* [Irányítópultok létrehozása](app-insights-dashboards.md)

További telemetriai funkciók hozzáadása:

* [Webteszt létrehozása] [ availability] toomake meg arról, hogy a hely élő marad.
* [Adja hozzá a webes ügyfél telemetriai] [ usage] weblap kódot és beszúrása toolet toosee kivételei nyomkövetési hívások.
* [Application Insights SDK tooyour kódot] [ greenbrown] , hogy helyezze be a nyomkövetést, és jelentkezzen hívások

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md
