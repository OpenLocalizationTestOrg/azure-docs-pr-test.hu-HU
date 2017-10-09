---
title: "aaaSet mentése web app analytics az ASP.NET az Azure Application insights szolgáltatással |} Microsoft Docs"
description: "Konfigurálhatja a helyszínen vagy az Azure-ban üzemeltetett ASP.NET-webhely teljesítményét, rendelkezésre állását és használatelemzését."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: 61a3cdce68da48bfb9450b1d296acc1535f50a38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Az Application Insights beállítása az ASP.NET-webhelyhez

Ezzel az eljárással az ASP.NET web app toosend telemetriai toohello [Azure Application Insights](app-insights-overview.md) szolgáltatás. A saját IIS-kiszolgálót vagy hello felhőben üzemeltetett ASP.NET alkalmazások esetében működik. Diagramok kap, és egy hatékony lekérdező nyelv, amelyek segítenek megérteni hello az alkalmazás teljesítményével kapcsolatos és a felhasználók hogyan használják, valamint automatikus riasztások hibák vagy teljesítményproblémák. Sok fejlesztők található ezeket a funkciókat kiváló, –, de is kiterjesztheti és testre szabhatja hello telemetriai adatokat, ha szeretné.

A telepítés mindössze néhány kattintással végrehajtható a Visual Studióban. Telemetriai adatok mennyisége hello korlátozásával hello beállítás tooavoid díjak rendelkezik. Ez lehetővé teszi, hogy tooexperiment és hibakeresési, vagy a hely nem sok felhasználóval rendelkező toomonitor. Ha mégis ezt szeretné azokat, amelyek toogo, és figyelheti a munkakörnyezeti helyet, később könnyen tooraise hello korlátot.

## <a name="before-you-start"></a>Előkészületek
A következők szükségesek:

* Visual Studio 2013 3. frissítés vagy újabb. Az újabb jobb.
* Előfizetés túl[Microsoft Azure](http://azure.com). Ha a csapat vagy szervezet Azure-előfizetéssel, hello tulajdonos használatával adhat hozzá, tooit, a [Microsoft-fiók](http://live.com).

Ha érdekli, számos alternatív témakörök toolook:.

* [Webalkalmazás beállítása futási időben](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a>1. lépés: Hello Application Insights SDK hozzáadása

Kattintson a jobb gombbal a webalkalmazás-projektre a Solution Explorer (Megoldáskezelő) területén, és válassza az **Add** (Hozzáadás),  > **Application Insights Telemetry** (Application Insights-telemetria) vagy a **Configure Application Insights** (Application Insights konfigurálása) elemet.

![A Solution Explorer (Megoldáskezelő) képernyőképe, a kiemelt Add Application Insights Telemetry (Application Insights telemetria hozzáadása) elemmel](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(A Visual Studio 2015, van továbbá egy beállítás tooadd Application Insights hello új projekt párbeszédpanelje.)

Továbbra is az Application Insights-konfiguráció lapon toohello:

![Képernyőkép az alkalmazásregisztrációs szakaszról az Application Insights oldalon](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** Válassza ki a hello fiókja és -előfizetése tooaccess Azure használatát.

**b.** Válassza ki a hello erőforrás ahová toosee hello adatokat az alkalmazásból az Azure-ban. Általában:

* [Egyetlen erőforrást használjon](app-insights-monitor-multi-role-apps.md) egy alkalmazás különböző összetevőihez. 
* Különálló erőforrásokhoz hozzon létre az egymáshoz nem kapcsolódó alkalmazásokhoz.
 
Ha azt szeretné, hogy tooset hello erőforrás csoport vagy hello az adatok tárolási helye, kattintson a **beállításainak**. Erőforráscsoportok használt toocontrol hozzáférés toodata. Például, ha több alkalmazást részét képező hello azonos rendszer, akkor előfordulhat, hogy be az Application Insights adatainak hello ugyanabban az erőforráscsoportban.

**c.** Állítsa be a cap hello szabad adatok mennyiség korlátozása, tooavoid díjakat. Az Application Insights szabadítson fel tooa telemetriai adatok mennyiségét. Hello erőforrás létrehozása után módosíthatja a kiválasztott hello portálon megnyitása **szolgáltatások + díjszabás** > **adatok kötetkezelés** > **napi kötet cap**.

**d.** Kattintson a **regisztrálása** azokat, amelyek toogo és konfigurálhatja az Application Insights a webalkalmazás. Telemetriai adatokat küld toohello [Azure-portálon](https://portal.azure.com), hibakeresési és az alkalmazás közzétételét követően.

**e.** Ha toosend telemetriai toohello portál nem szeretné, hogy hibakeresése közben, csak adja hozzá a hello Application Insights SDK tooyour app, de ne állítson be egy erőforrás hello portálon. A Visual Studio képes toosee telemetriai közben a rendszer hibakeresési lesz. Később, toothis konfigurálása lapon lépjen vissza, vagy sikerült megvárni az alkalmazás telepítése után, és [futási időben a telemetria kapcsoló](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> 2. lépés: Az alkalmazás futtatása
Futtassa az alkalmazást az F5 billentyűvel. Nyissa meg a különböző oldalakhoz toogenerate néhány telemetriai adatokat.

A Visual Studio alkalmazásban lásd: hello naplózott események számát.

![A Visual Studio képernyőképe. hello Application Insights gombon hibakeresés során.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>3. lépés: A telemetria megtekintése
A telemetriai adatokat, a Visual Studio vagy hello Application Insights webes portálon tekintheti meg. Telemetriai adatok keresése az alkalmazás hibakeresése a Visual Studio toohelp. Ha a rendszer élő, figyelheti a teljesítmény- és használati hello web portal alkalmazásban. 

### <a name="see-your-telemetry-in-visual-studio"></a>Telemetria megtekintése a Visual Studióban

A Visual Studióban nyissa meg a hello Application Insights ablakot. Vagy kattintson a hello **Application Insights** gombra, vagy kattintson a jobb gombbal a projektre a Megoldáskezelőben, válassza ki **Application Insights**, és kattintson a **keresési élő Telemetriai**.

Hello Visual Studio Application Insights keresési ablak, tekintse meg a hello **hibakeresési munkamenet adatait** hello kiszolgálóoldali az alkalmazás hozott létre telemetriai nézetet. Hello szűrők kísérletezhet, és kattintson a bármely esemény toosee további információkhoz juthat.

![Képernyőfelvétel a hello hibakeresési munkamenetben adatnézet hello Application Insights ablakban.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Ha nem lát adatokat, győződjön meg arról, hello időtartomány helyességéről, majd kattintson a hello keresés ikonra.

[További tudnivalók az Application Insights-eszközökről a Visual Studióban](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Telemetria megtekintése a webportálon

Telemetria hello Application Insights webes portálon megtekintheti (kivéve, ha úgy döntött, hogy tooinstall csak hello SDK) is. hello portal további diagramokat, elemzési eszközök és kereszt-összetevő nézetek, mint a Visual Studio rendelkezik. hello portal riasztást küld.

Nyissa meg az Application Insights-erőforrást. Vagy bejelentkezés toohello [Azure-portálon](https://portal.azure.com/) és a Visual Studio van, vagy kattintson a jobb gombbal hello projekt keresse meg és azt nem léphet.

![Képernyőfelvétel a Visual Studio, hogyan tooopen hello Application Insights portál megjelenítése](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Ha a hozzáférési hibaüzenetet kap: rendelkezik egynél több Microsoft hitelesítőadat-készletet, és van jelentkezett be hello megfelelő készletet? Hello portálon jelentkezzen ki, és jelentkezzen be újra.

hello portál megnyitja egy nézeten hello telemetriai adatot az alkalmazásból.

![Az Application Insights áttekintési oldalának képernyőképe](./media/app-insights-asp-net/66.png)

Hello portálon kattintson a csempe vagy diagram toosee további információkhoz juthat.

[További információ az Application Insights az Azure-portálon hello](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>4. lépés: Az alkalmazás közzététele
Az alkalmazás tooyour IIS-kiszolgálót vagy tooAzure közzététele. Figyelési [metrikák élő adatfolyam](app-insights-metrics-explorer.md#live-metrics-stream) toomake, minden fut-e zökkenőmentesen működjön.

A telemetriai adatok épít fel hello Application Insights portálon, ahol metrikát, a telemetriai adatok keresése és állítson be [irányítópultok](app-insights-dashboards.md). Is használhatja hello hatékony [Log Analytics lekérdezési nyelv](https://docs.loganalytics.io/) tooanalyze használati és a teljesítmény vagy a toofind adott események.

Is folytathatja a tooanalyze az a telemetriai [Visual Studio](app-insights-visual-studio.md), az eszközöket, például a diagnosztikai keresési és [trendek](app-insights-visual-studio-trends.md).

> [!NOTE]
> Ha az alkalmazás küld elég telemetriai tooapproach hello [szabályozás korlátok](app-insights-pricing.md#limits-summary), az automatikus [mintavételi](app-insights-sampling.md) kapcsolókat. Mintavételi diagnosztikai célokra kapcsolódó adatok megőrzésével az alkalmazásból küldött telemetriai hello mennyiségét csökkenti.
>
>

## <a name="land"></a>Készen is van.

Gratulálunk! Hello Application Insights csomag telepítése az alkalmazásban, és konfigurálta az Azure toosend telemetriai toohello Application Insights szolgáltatást.

![A telemetria mozgásának ábrája](./media/app-insights-asp-net/01-scheme.png)

hello Azure-erőforrás, amely megkapja az alkalmazás telemetriai azonosít egy *instrumentation kulcs*. Ez a kulcs hello ApplicationInsights.config fájlban talál.


## <a name="upgrade-toofuture-sdk-versions"></a>Toofuture SDK verziók frissítése
tooupgrade tooa [hello SDK új kiadása](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), nyissa meg hello **NuGet-Csomagkezelő** újra, és a telepített csomagok szűrőt. Jelölje ki a **Microsoft.ApplicationInsights.Web** lehetőséget, és válassza a **Frissítés** elemet.

Ha végzett a testreszabások tooApplicationInsights.config, mentse egy példányát frissítése előtt. A módosításokat, majd egyesítése hello új verziója.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Következő lépések

### <a name="more-telemetry"></a>További telemetria

* **[Böngésző és oldalbetöltési adatok](app-insights-javascript.md)** – Kódrészlet beszúrása a weboldalakra.
* **[Részletesebb függőség- és kivételfigyelés](app-insights-monitor-performance-live-website-now.md)** – Állapotfigyelő telepítése a kiszolgálón.
* **[Egyéni események Code](app-insights-api-custom-events-metrics.md)**  toocount, time, vagy felhasználói műveletek mérését.
* **[Naplóadatok lekérése](app-insights-asp-net-trace-logs.md)** – Naplóadatok összevetése a telemetriával.

### <a name="analysis"></a>Elemzés

* **[Az Application Insights használata a Visual Studióban](app-insights-visual-studio.md)**<br/>Telemetriai adatokat a hibakeresési információkat tartalmaz diagnosztikai keresési, valamint lefúrhat toocode keresztül.
* **[Hello Application Insights portál használata](app-insights-dashboards.md)**<br/> Az irányítópultokkal, a hatékony diagnosztikai és elemzési eszközökkel, riasztásokkal, az alkalmazás élő függőségi térképével, valamint a telemetria exportálásával kapcsolatos információkat tartalmaz.
* **[Elemzés](app-insights-analytics-tour.md)**  -hello hatékony lekérdezési nyelv.

### <a name="alerts"></a>Riasztások

* [Rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md): tesztek létrehozása, hogy a hely látható hello Web toomake.
* [Diagnosztika intelligens](app-insights-proactive-diagnostics.md): ezek a tesztek automatikusan fut, így nem kell toodo semmi tooset be őket. Értesítést kap, ha az alkalmazásában szokatlanul magas a meghiúsult kérelmek száma.
* [Metrika riasztások](app-insights-alerts.md): állítsa be ezeket a toowarn, ha egy metrika a küszöbérték keverve használ. Az alkalmazás kódjába beépített egyedi metrikákhoz is állíthat be riasztásokat.

### <a name="automation"></a>Automatizálás

* [Application Insights-erőforrások létrehozásának automatizálása](app-insights-powershell.md)
