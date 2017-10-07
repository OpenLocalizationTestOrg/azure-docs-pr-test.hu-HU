---
title: "aaaHow készíthetek... Azure Application insightsban |} Microsoft Docs"
description: "Az Application insights szolgáltatással kapcsolatos gyakori kérdések."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: 89294c3583b7c4e7998143be6d359f2deb3c8f49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-i--in-application-insights"></a>Hogyan tegyem... az Application Insights szolgáltatásban?
## <a name="get-an-email-when-"></a>Az e-mailek Ha...
### <a name="email-if-my-site-goes-down"></a>E-mailek, ha a hely leáll
Állítsa be az [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>Ha a hely túl van terhelve e-mail
Állítsa be az [riasztás](app-insights-alerts.md) a **kiszolgáló válaszideje**. A küszöbérték, 1 és 2 másodperc között kell működnie.

![](./media/app-insights-how-do-i/030-server.png)

Az alkalmazás is lehet, hogy megjelenítése a törzs jeleit hibát kódok vissza. Riasztást állíthat be **sikertelen kérelmek**.

Ha a kívánt riasztást tooset **kivételek**, toodo lehetséges, hogy [néhány további telepítési](app-insights-asp-net-exceptions.md) rendelés toosee adataiban.

### <a name="email-on-exceptions"></a>E-maileket az kivételek
1. [Kivételfigyelés beállítása](app-insights-asp-net-exceptions.md)
2. [A riasztások](app-insights-alerts.md) hello kivétel a metrika száma

### <a name="email-on-an-event-in-my-app"></a>E-maileket az alkalmazásom az esemény
Tegyük fel, hogy milyen tooget egy e-mailt egy adott esemény bekövetkezésekor. Az Application Insights közvetlenül ez a lehetőség nem biztosít, de azt is [riasztás küldése metrika ebbe a küszöbérték](app-insights-alerts.md).

Riasztások beállítható [egyéni metrikák](app-insights-api-custom-events-metrics.md#trackmetric), azonban nem egyéni események. Írási néhány kódot tooincrease metrika hello esemény bekövetkezésekor:

    telemetry.TrackMetric("Alarm", 10);

Vagy:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Riasztások kétállapotú van, mert rendelkezik toosend alacsony értéket toohave befejeződött hello riasztás meghatározásakor:

    telemetry.TrackMetric("Alarm", 0.5);

A diagram létrehozása [metrika explorer](app-insights-metrics-explorer.md) toosee a riasztás:

![](./media/app-insights-how-do-i/010-alarm.png)

Egy riasztás toofire most állítja, amikor egy rövid időre mid érték fölé megy hello metrika:

![](./media/app-insights-how-do-i/020-threshold.png)

Átlagolási időszak toohello minimális hello beállítása.

E-mailt fog kapni, amikor hello metrika fölé megy, mind hello küszöbérték alá.

Egyes pontok tooconsider:

* Riasztást két állapota ("figyelmeztetés" és "kifogástalan") van. hello állapot csak akkor, ha egy metrika érkezik értékeli.
* Az e-mail elküldésekor történik, csak akkor, ha hello állapota megváltozik. Ezért toosend rendelkezik magas és alacsony értékű.
* tooevaluate hello riasztás hello átlagos időtartam megelőző hello átvett kapott hello értékek. Ez mindig megtörténik egy metrika érkezik, e-mailek gyakrabban beállított hello időszakának küldhetők el.
* Mivel e-mailek küldése mind a "riasztás" és "kifogástalan", érdemes lehet újra végezni a egyszeri esemény kétállapotú feltételeként tooconsider. Például helyett a "feladata Befejezve" esemény "folyamatban lévő feladat" állapotba kerültek, ahol kap e-mailek hello kezdő és egy feladat végén.

### <a name="set-up-alerts-automatically"></a>Riasztások beállítása automatikusan
[PowerShell toocreate új riasztások](app-insights-alerts.md#automation)

## <a name="use-powershell-toomanage-application-insights"></a>PowerShell tooManage Application Insights használata
* [Hozzon létre új erőforrások](app-insights-powershell-script-create-resource.md)
* [Hozzon létre új riasztások](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Különböző verzióiból külön telemetria

* Egy alkalmazásban több szerepkört: egyetlen Application Insights-erőforrást használjon, és a szűrést végezni cloud_Rolename. [További információ](app-insights-monitor-multi-role-apps.md)
* Fejlesztői, tesztelési és verzió elválasztó: különböző Application Insights-erőforrások használatára. Vegyen fel hello instrumentation kulcsok a Web.config fájlból. [További információ](app-insights-separate-resources.md)
* Jelentéskészítési build verziók: a telemetriai adatok inicializáló használatával tulajdonság hozzáadása. [További információ](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>A figyelő háttérkiszolgálók, illetve az asztali alkalmazások
[Használjon hello Windows Server SDK modul](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Adatok megjelenítése
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>A metrikák a több alkalmazás irányítópult
* A [metrika Explorer](app-insights-metrics-explorer.md), a diagram testreszabásához, és mentse azokat a Kedvencek közé. Toohello Azure irányítópulton rögzítheti.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Más forrásokból és az Application Insights-adatokkal irányítópult
* [Exportálja a telemetriai adatok tooPower BI](app-insights-export-power-bi.md).

Vagy

* A SharePoint használata az irányítópulton megjelenő adatok SharePoint-kijelzők. [A folyamatos exportálás és a Stream Analytics tooexport tooSQL](app-insights-code-sample-export-sql-stream-analytics.md).  PowerView tooexamine hello adatbázist használnak, és hozzon létre egy SharePoint-kijelzőt PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Hitelesített vagy névtelen felhasználók szűrése
Ha a felhasználói bejelentkezés, beállíthatja azt a hello [hitelesített felhasználói azonosító](app-insights-api-custom-events-metrics.md#authenticated-users). (Ez nem automatikusan megtörténik.)

Ezek közül:

* A megadott felhasználói azonosítók keresése

![](./media/app-insights-how-do-i/110-search.png)

* Metrikák tooeither hitelesített vagy névtelen felhasználók szűrése

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Tulajdonság neve vagy értéke módosítása
Hozzon létre egy [szűrő](app-insights-api-filtering-sampling.md#filtering). Ez lehetővé teszi módosítása vagy telemetriai szűrő a app tooApplication Insights az elküldés előtt.

## <a name="list-specific-users-and-their-usage"></a>Adott felhasználók listázása és azok használata
Ha csak túl[bizonyos felhasználók keresése](#search-specific-users), beállíthatja a hello [hitelesített felhasználói azonosító](app-insights-api-custom-events-metrics.md#authenticated-users).

Ha azt szeretné, hogy az adatok, például az oldalak rendelkező felhasználók listáját, megtekintik vagy milyen gyakran jelentkeznek be, két lehetőség közül választhat:

* [Hitelesített felhasználó azonosítója](app-insights-api-custom-events-metrics.md#authenticated-users), [tooa adatbázis-exportálási](app-insights-code-sample-export-sql-stream-analytics.md) és használja a megfelelő eszközöket tooanalyze nincs felhasználói adatokat.
* Ha csak kisszámú felhasználók, küldése egyéni események és metrikák hello érdeklő adatok használatával, mint a metrika értékét vagy esemény és beállítás hello felhasználói azonosító tulajdonságként hello. Lapmegtekintések tooanalyze, cserélje le a hello standard JavaScript trackPageView hívás. tooanalyze kiszolgálóoldali telemetriai adatokat a telemetriai adatok inicializáló tooadd hello felhasználói azonosító tooall server telemetriai használja. Ezek közül és a szegmens metrikák és a keresés hello felhasználói azonosítóját.

## <a name="reduce-traffic-from-my-app-tooapplication-insights"></a>Saját alkalmazás tooApplication Insights a forgalom csökkentése
* A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), tiltsa le az ilyen hello teljesítmény Számláló adatgyűjtő nincs szüksége, modul.
* Használjon [mintavételi és a szűrés](app-insights-api-filtering-sampling.md) : hello SDK.
* A weblapok minden lapmegtekintés jelentett Ajax-hívások hello számának korlátozása. A hello parancsfájl részlet után `instrumentationKey:...` , beszúrása: `,maxAjaxCallsPerView:3` (vagy megfelelő számú).
* Ha használ [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), számítási hello összesítés metrika értékek kötegek hello eredmény elküldése előtt. A trackmetric() függvény, amely biztosítja, hogy egy túlterhelése van.

További információ [tarifa- és a kvóták](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Telemetria letiltása
túl**dinamikusan leállítására és elindítására** hello összegyűjtése és hello kiszolgálóról telemetriai adatok továbbítása:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



túl**tiltsa le a kiválasztott szabványos gyűjtők** – például teljesítményszámlálókat, HTTP-kérelmek vagy függőségek - törlése vagy hello megfelelő sorok megjegyzéssé [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Sikerült ehhez, például ha toosend saját TrackRequest adatokat.

## <a name="view-system-performance-counters"></a>Nézet rendszerteljesítmény-számlálók
Hello között is megjeleníthetők a metrikaböngészőben metrikák olyan rendszerteljesítmény-számlálók. Van egy előre meghatározott panel című **kiszolgálók** , amely megjeleníti, hogy ezek.

![Nyissa meg az Application Insights-erőforrást, és kattintson a kiszolgálók](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Ha nincs teljesítményszámláló-adatok
* **IIS-kiszolgáló** a saját számítógépén vagy a virtuális gép. [Állapotmonitor telepítése](app-insights-monitor-performance-live-website-now.md).
* **Azure-webhelyre** -teljesítményszámlálók még nem támogatott. Nincsenek több metrikákat kaphat a hello Azure webhelyére Vezérlőpult szabványos részeként.
* **UNIX kiszolgáló** - [collectd telepítése](app-insights-java-collectd.md)

### <a name="toodisplay-more-performance-counters"></a>toodisplay további teljesítményszámlálók
* Első, [új diagram hozzáadása](app-insights-metrics-explorer.md) , és ellenőrizze, hogy ha hello a számláló az alapszintű hello beállítása, hogy fel.
* Ha nem, [hello toohello számlálókészlet hello teljesítményszámláló modul által gyűjtött hozzáadása](app-insights-performance-counters.md).
