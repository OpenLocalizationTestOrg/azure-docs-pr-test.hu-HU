---
title: "aaaAzure Application Insights – gyakori kérdések |} Microsoft Docs"
description: "Az Application Insights vonatkozó gyakran ismételt kérdések."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: bwren
ms.openlocfilehash: e27ee9b7d040a04828a9892865a6681b83f94326
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-frequently-asked-questions"></a>Az Application Insights: Gyakran ismételt kérdések

## <a name="configuration-problems"></a>Konfigurációs problémák
*Hiba történt a beállítás lépett a:*

* [.NET-alkalmazás](app-insights-asp-net-troubleshoot-no-data.md)
* [Egy már futó alkalmazás figyelése](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Az Azure diagnostics](app-insights-azure-diagnostics.md)
* [Java webalkalmazások](app-insights-java-troubleshoot.md)

*Adatot nem jelenik meg a kiszolgálóról*

* [Tűzfalkivételek beállítása](app-insights-ip-addresses.md)
* [ASP.NET-kiszolgáló beállítása](app-insights-monitor-performance-live-website-now.md)
* [A Java-kiszolgáló beállítása](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Használható az Application Insights...?

* [Webalkalmazások IIS kiszolgálón – a helyi vagy egy virtuális Gépre](app-insights-asp-net.md)
* [Java-webalkalmazások](app-insights-java-get-started.md)
* [Node.js-alkalmazások](app-insights-nodejs.md)
* [Az Azure Web Apps alkalmazások](app-insights-azure-web-apps.md)
* [Azure cloud Services csomag](app-insights-cloudservices.md)
* [A Docker alkalmazást futtató](app-insights-docker.md)
* [Egyoldalas webalkalmazások](app-insights-javascript.md)
* [SharePoint](app-insights-sharepoint.md)
* [Windows asztali alkalmazás](app-insights-windows-desktop.md)
* [Más platformok](app-insights-platforms.md)

## <a name="is-it-free"></a>Az ingyenes?

Igen, kísérleti használatra. Az alapszintű csomag árképzési hello az alkalmazás egy bizonyos támogatás az adatok minden hónapban díjmentesen is tud küldeni. hello szabad támogatás elég nagy toocover fejlesztési, de a felhasználók néhány alkalmazás közzététele. Beállíthat egy cap tooprevent több, mint a megadott méretű adatok feldolgozását.

Nagyobb mértékű telemetriai hello Gb számít fel. Tippek nyújtunk túl[a költségek korlátozására](app-insights-pricing.md).

hello vállalati terv a azt eredményezi, amelyet minden webkiszolgáló csomópontját telemetriai adatokat küld naponta díjat azok háromszorosa. Megfelelő, ha azt szeretné, hogy a nagy méretű a folyamatos exportálás toouse.

[Terv árképzési olvasási hello](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Mennyire van azt költségszámítás?

* Nyissa meg hello **szolgáltatások + díjszabás** lap az Application Insights-erőforrást. Legutóbbi használat diagram van. Egy adatok kötet kap, ha azt szeretné, állíthatja be.
* Nyissa meg hello [panel Azure számlázási](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) toosee a váltók közötti összes erőforrást.

## <a name="q14"></a>Mi az Application Insights módosítja a projektben?
hello részletek projekt hello típusától függ. Egy webes alkalmazáshoz:

* Ezen fájlok tooyour projekt hozzáadása:

  * ApplicationInsights.config.
  * AI.js
* A NuGet-csomagok telepíti:

  * *Application Insights API* – hello alapvető API
  * *Application Insights API-webalkalmazások számára történő* -használt toosend telemetriai hello kiszolgálóról
  * *Application Insights API-t a JavaScript alkalmazások* -használt toosend telemetriai hello ügyfélről

    hello csomagok ezek szerelvényeket tartalmazza:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Beszúrja azokat:

  * Web.config
  * Packages.config
* (Új projekt csak – ha meg [Application Insights tooan létező projekt hozzáadása][start], rendelkezik toodo ezt manuálisan.) Kódtöredékek szúr be hello ügyfél és kiszolgáló kód tooinitialize őket a hello Application Insights erőforrás-azonosító. Például egy MVC alkalmazás kód bekerülnek hello mesterlap Views/Shared/_Layout.cshtml

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Hogyan lehet frissíteni a régebbi SDK?
Lásd: hello [kibocsátási megjegyzéseket](app-insights-release-notes.md) hello SDK a megfelelő alkalmazás tooyour típusát.

## <a name="update"></a>Hogyan lehet módosítani a projekt adatokat küld az Azure erőforrás?
A Megoldáskezelőben kattintson a jobb gombbal `ApplicationInsights.config` válassza **frissítés az Application Insights**. Adatok tooan meglévő vagy új erőforrás hello küldhet az Azure-ban. hello frissítés varázsló módosítások hello instrumentation kulcs ApplicationInsights.config, amely megadja, hogy hol hello server SDK elküldi az adatokat. Kivéve, ha törli a "Frissítés all", ahol megjelenik a weblapok hello kulcs is megváltozik.

## <a name="what-is-status-monitor"></a>Mi az Állapotfigyelő?

Egy asztali alkalmazás, amely az IIS web server toohelp is használhatja a webalkalmazásokban konfigurálhatja az Application Insights. Azt nem gyűjthet: le lehet állítani, ha nem kívánja használni az alkalmazást. 

[További információk](app-insights-monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Milyen telemetriai adatokat gyűjti az Application Insights?

A kiszolgáló web apps:

* HTTP-kérések
* [Függőségek](app-insights-asp-net-dependencies.md). Hívások: SQL-adatbázisok; HTTP meghívja tooexternal szolgáltatások; Az Azure Cosmos DB, tábla, a blob storage és várólista. 
* [Kivételek](app-insights-asp-net-exceptions.md) és kivételeseményekhez megadhat veremkiíratásokat.
* [Teljesítményszámlálók](app-insights-performance-counters.md) - használatakor [állapotfigyelő](app-insights-monitor-performance-live-website-now.md), Azure monitoring(app-insights-azure-web-apps.md) vagy hello [Application Insights collectd író](app-insights-java-collectd.md).
* [Egyéni események és metrikák](app-insights-api-custom-events-metrics.md) , hogy kódaláírással.
* [Nyomkövetési naplók](app-insights-asp-net-trace-logs.md) hello megfelelő adatgyűjtő konfigurálásakor.

A [ügyfél weblapok](app-insights-javascript.md):

* [Lapmegtekintés száma](app-insights-web-track-usage.md)
* [AJAX-hívások](app-insights-asp-net-dependencies.md) futó parancsfájl kérések száma.
* Lapbetöltési adatainak megtekintése
* Felhasználó és a munkamenet számát
* [Hitelesített felhasználói azonosítók](app-insights-api-custom-events-metrics.md#authenticated-users)

Más forrásokból, ha konfigurálja őket:

* [Az Azure diagnostics](app-insights-azure-diagnostics.md)
* [Docker-tároló](app-insights-docker.md)
* [Importálás táblák tooAnalytics](app-insights-analytics-import.md)
* [OMS (Naplóelemzési)](https://azure.microsoft.com/blog/omssolutionforappinsightspublicpreview/)
* [Logstash](app-insights-analytics-import.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Kiszűrhetők vagy módosíthatja az egyes telemetriai?

Igen, a hello server írhat:

* Telemetria processzor toofilter, vagy adja hozzá a Tulajdonságok tooselected telemetriai elemek megkövetelése azok elküldéséhez az alkalmazásból.
* Telemetria inicializáló tooadd tulajdonságai tooall elemek telemetriai adatot.

A további [ASP.NET](app-insights-api-filtering-sampling.md) vagy [Java](app-insights-java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Hogyan város, az ország és az egyéb földrajzi hely adatok kiszámítása?

A Microsoft hello ügyfél IP-címe (IPv4 vagy IPv6) hello webes használatával kereshet [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/).

* Böngésző telemetriai: gyűjtjük hello feladó IP-címe.
* Kiszolgáló telemetriai: hello Application Insights modul gyűjt hello ügyfél IP-címét. Nem gyűjtenek, ha `X-Forwarded-For` van beállítva.

Konfigurálhatja a hello `ClientIpHeaderTelemetryInitializer` tootake hello IP-címet egy másik fejlécet. Az egyes rendszerek, például áthelyezés olyan proxy, és töltse be terheléselosztót vagy CDN túl`X-Originating-IP`. [További információk](http://apmtips.com/blog/2016/07/05/client-ip-address/).

Is [használja a Power BI](app-insights-export-power-bi.md) toodisplay a – kéréstelemetria a térképen.


## <a name="data"></a>Mennyi ideig adatok őrződnek meg hello portálon? Az biztonságos?
Vessen egy pillantást [az adatmegőrzés és az adatvédelmi][data].

## <a name="might-personally-identifiable-information-pii-be-sent-in-hello-telemetry"></a>Előfordulhat, hogy személyes azonosításra alkalmas adatokat (PII) kell küldi hello telemetriai?

Ez akkor lehetséges, ha a kód elküldi ezeket az adatokat. Akkor is előfordulhat, ha változók híváslánc megjelenik a személyhez köthető adatokat tartalmazza. A fejlesztői csapat kockázat értékelések tooensure, hogy a rendszer megfelelően kezelje személyhez köthető adatokat kell végezniük. [További információ az adatmegőrzés és adatvédelmi tudnivalók](app-insights-data-retention-privacy.md).

utolsó oktettje hello hello ügyfél webcímet értéke mindig too0 bevitel után hello portál.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>A iKey látható lesz a weblap forrása. 

* Ez a figyelési megoldásoknak a gyakori eljárása.
* Azt nem lehet használt toosteal adatait.
* Ez az adat- vagy eseményindító riasztásokat lehet használt tooskew.
* Igazolnia kell nem hallgassa meg, hogy minden ügyfél az ilyen problémák volt.

Sikerült:

* Két külön iKeys (külön Application Insights-erőforrások), használja az ügyfél és kiszolgáló adatok. Vagy
* Írni a proxy a kiszolgálón fut, és hello webes ügyféllel, hogy a proxyn keresztül történő adatküldés rendelkezik.

## <a name="post"></a>Hogyan tekinthető meg a diagnosztikai keresési POST-adatokat?
Automatikusan azt ne naplózza a POST-adatokat, de használhat TrackTrace hívás: hello adatok be hello üzenet paraméter. Ez méretkorlátja hosszabb hello vonatkozó korlátozások kapcsolatikarakterlánc-tulajdonságokat, mint abban az esetben, ha nem lehet szűrni rajta.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Egy vagy több Application Insights-erőforrások érdemes használni?

Egyetlen erőforrást használjanak minden hello összetevők vagy szerepkörök egyetlen üzleti rendszereken. Fejlesztői, tesztelési és verzióját, és független alkalmazások használjon külön erőforrások.

* [Itt hello leírását lásd:](app-insights-separate-resources.md)
* [Példa - munkavégző és a webes szerepkör felhőszolgáltatás](app-insights-cloudservices.md)

## <a name="how-do-i-dynamically-change-hello-instrumentation-key"></a>Hogyan dinamikusan módosítható hello instrumentation kulcsot?

* [Itt az ismertető](app-insights-separate-resources.md)
* [Példa - munkavégző és a webes szerepkör felhőszolgáltatás](app-insights-cloudservices.md)

## <a name="what-are-hello-user-and-session-counts"></a>Mik azok a felhasználói hello és a munkamenet száma?

* JavaScript SDK hello beállítása a felhasználói cookie-k hello webes ügyféllel, tooidentify visszatérő felhasználó és egy munkamenet-cookie-k toogroup tevékenységek.
* Ha a nem az ügyféloldali parancsprogram [hello kiszolgálón állítsa be a cookie-k](http://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Ha egy felhasználó a webhely különböző böngészők, vagy keresse meg a-private vagy incognito, vagy másik gép, majd többször beleszámítanak.
* a bejelentkezett felhasználó gép és a böngészők között tooidentify adjon hozzá egy túl[setAuthenticatedUserContect()](app-insights-api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a>Engedélyezte a I mindent az Application Insights?
| Mit kell megjelennie | Hogyan tooget azt | Miért érdemes |
| --- | --- | --- |
| Rendelkezésre állási diagramok |[Webteszt](app-insights-monitor-web-app-availability.md) |Tudja, hogy a webes alkalmazás mentése |
| Server app telj: válaszidejét,... |[Az Application Insights tooyour projekt hozzáadása](app-insights-asp-net.md) vagy [AI Állapotmonitor telepítése a kiszolgálón](app-insights-monitor-performance-live-website-now.md) (vagy a saját kód írása túl[függőségek nyomon](app-insights-api-custom-events-metrics.md#trackdependency)) |A Teljesítményfigyelő problémák észlelése |
| – Függőségi telemetria |[AI Állapotmonitor telepítése a kiszolgálón](app-insights-monitor-performance-live-website-now.md) |Az adatbázisok vagy más külső összetevők eseményadatokat |
| Kivételek híváslánc megjelenik beszerzése |[Helyezze be a kódot TrackException hívások](app-insights-asp-net-exceptions.md) (de néhány automatikusan jelenti) |Észlelheti és diagnosztizálhatja a kivételek |
| Keresési naplókivonatokat |[Naplózási adapter hozzáadása](app-insights-asp-net-trace-logs.md) |Ez alól kivétel a Teljesítményfigyelő problémák diagnosztizálása |
| Ügyfél használatának alapjai: Lapmegtekintések, munkamenetek,... |[JavaScript inicializáló a weblapok](app-insights-javascript.md) |Használatelemzés |
| Egyéni metrikák ügyfél |[Nyomkövetési meghívja a weblapok](app-insights-api-custom-events-metrics.md) |Felhasználói élmény javítása érdekében |
| Kiszolgáló egyéni metrikák |[A kiszolgáló nyomkövetési hívások](app-insights-api-custom-events-metrics.md) |Üzleti intelligencia |

## <a name="why-are-hello-counts-in-search-and-metrics-charts-unequal"></a>Miért van hello száma keresési és metrikák diagramokban egyenlőtlen?

[A mintavételi](app-insights-sampling.md) hello telemetriai elemek száma (kéréseket, egyéni események és így tovább) az alkalmazás toohello portálról ténylegesen elküldött csökkenti. A keresési lásd: hello elemszáma ténylegesen kapott. A metrika diagramokat, amely megjeleníti az események számát lásd: hello történt eredeti események száma. 

Minden elem, amely transmmitted végzi egy `itemCount` bemutatja, hogy hány eredeti események, hogy az elem tulajdonság jelöli. tooobserve mintavétel műveletben, futtathatja a lekérdezés Analytics:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Automatizálás

### <a name="configuring-application-insights"></a>Az Application Insights konfigurálása

Is [PowerShell-parancsfájlokat írhat](app-insights-powershell.md) az Azure erőforrás-figyelő segítségével:

* Hozzon létre, és frissítse az Application Insights-erőforrások.
* Terv árképzési hello beállítása.
* Hello instrumentation kulcs beszerzése.
* A metrika-riasztások hozzáadása.
* Elérhetőségi teszt hozzáadása.

Nem állítja be a metrika Explorer jelentést és a folyamatos exportálás beállítása.

### <a name="querying-hello-telemetry"></a>Hello telemetriai adatainak lekérdezése

Használjon hello [REST API](https://dev.applicationinsights.io/) toorun [Analytics](app-insights-analytics.md) lekérdezések.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Hogyan állíthatja be az esemény a riasztást?

Az Azure-riasztásokat csak metrikák esetén. Egy érték küszöbérték keverve használ az esemény akkor következik be, amikor egyéni metrika létrehozása. Majd hello metrika a riasztás beállításához. Vegye figyelembe a következőket: egy értesítést fog kapni, amikor hello metrika áthalad hello küszöbérték mindkét irányban; nem jelenik meg egy értesítés, amíg hello első metsző, függetlenül attól, hogy hello kezdeti értéke magas vagy alacsony; mindig legyen a késés néhány percig.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Vannak-e adatok adatátviteli díjakkal között egy Azure web app és az Application Insights?

* Ha az Azure-webalkalmazásban helyezkedik-e egy adott adatközpont Application Insights-gyűjtemény végpont esetén használata díjmentes. 
* Ha nincs figyelésgyűjtési végpont az állomás adatközpont van, akkor az alkalmazás telemetriai tájékozódnia [díjak kimenő Azure](https://azure.microsoft.com/pricing/details/bandwidth/).

Ez nem függő amelyen az Application Insights-erőforrás található. A végpontok hello terjesztési csak függ.

## <a name="can-i-send-telemetry-toohello-application-insights-portal"></a>Elküldheti a telemetriai adatok toohello Application Insights portál?

Javasoljuk, hogy használja az SDK-IT és hello SDK API-t (app-insights-api-custom-events-metrics.md) használja. Nincsenek hello SDK különböző változatai [platformok](app-insights-platforms.md). Ezek az SDK-k kezelni pufferelés, tömörítés, szabályozás, az újrapróbálkozások, és így tovább. Azonban hello [adatfeldolgozást séma](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) és [végpont protokoll](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) nyilvános.

## <a name="can-i-monitor-an-intranet-web-server"></a>Képes figyelni az intranet webkiszolgáló?

Az alábbiakban a két módszer:

### <a name="firewall-door"></a>Tűzfal ajtó

A web server toosend telemetriai tooour végpontok https://dc.services.visualstudio.com:443 és https://rt.services.visualstudio.com:443 engedélyezése. 

### <a name="proxy"></a>Proxy

Az intraneten, úgy, hogy ez az ApplicationInsights.config a kiszolgáló tooa átjáró irányíthatja a forgalmat:

```XML
<TelemetryChannel>
    <EndpointAddress>your gateway endpoint</EndpointAddress>
</TelemetryChannel>
```

Az átjáró kell továbbítani hello forgalom toohttps://dc.services.visualstudio.com:443/v2/nyomon követése

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Futtatható-e rendelkezésre állási webes tesztjeinek használatát egy intranetes kiszolgálóra?

A [webalkalmazás-tesztek](app-insights-monitor-web-app-availability.md) kapcsolódási pontok, körül hello földgömb elosztott futtatnak. Nincsenek két megoldások:

* Tűzfal-ajtó - kérések engedélyezése tooyour kiszolgáló [teszt ügynökök hello hosszú és webes módosítható listája](app-insights-ip-addresses.md).
* A saját kód toosend kérések tooyour kiszolgáló az intraneten belüli írni. Futtatható a Visual Studio webtesztjeivel erre a célra. hello tester küldhet hello eredmények tooApplication Insights hello TrackAvailability() API használatával.

## <a name="more-answers"></a>Adott további válaszokért
* [Application Insights fórum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
