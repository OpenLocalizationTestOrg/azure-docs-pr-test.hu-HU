---
title: "A webes alkalmazás figyelése |} Microsoft Docs"
description: "Útmutató: a webalkalmazás a figyelés beállítása"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: 29df824062d00e01b786533033097948c008588f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-app-service"></a>Az App Service figyelése
Ez az oktatóanyag végigvezeti az alkalmazás figyelése és a beépített platform eszközökkel kapcsolatos problémák megoldásához, azok bekövetkezésekor.

Ez a dokumentum egyes részeinek kerül egy adott funkcióhoz keresztül. A szolgáltatások együttes használata lehetővé teszik, hogy:
- Az alkalmazás problémát azonosítása.
- Annak meghatározása, ha a hibát az okozza a kódban, illetve a platform.
- Szűkítse le a kódban a probléma forrása.
- Hibakeresés és a probléma kijavítása.

## <a name="before-you-begin"></a>Előkészületek
- Szüksége van a webes alkalmazás figyelésére, és kövesse az itt ismertetett lépéseket.
    - Létrehozhat egy alkalmazást a következő helyen ismertetett lépések a [ASP.NET-alkalmazás létrehozása az Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) oktatóanyag.

- Ha azt szeretné, hogy próbálja ki **távoli hibakeresés** az alkalmazás, a Visual Studio szüksége.
    - Ha még nincs telepítve a Visual Studio 2017, töltse le, és az ingyenes [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).
    - Ügyeljen arra, hogy engedélyezze az **Azure Development** használatát a Visual Studio telepítése során.

## <a name="metrics"></a>1. lépés – nézet metrikák
**Metrikák** működésének megértése:
- Alkalmazás állapotának
- Alkalmazás teljesítménye
- Erőforrás-használat

Az alkalmazás problémát vizsgál, tekintse át a metrikákat esetén remek kezdőpont. Azure-portálon gyorsan vizuálisan vizsgálja meg az alkalmazás használatával metrikáinak rendelkezik **Azure figyelő**.

Metrikákat biztosít az alkalmazás több kulcs összesítésben állapotelőzményeinek a nézetét. Bármely alkalmazás az app service-ben üzemeltetett figyelje, hogy mind a Web App és az App Service-csomag.

> [!NOTE]
> Az App Service-csomagok az alkalmazások üzemeltetéséhez használt fizikai erőforrások gyűjteményét jelölik. Egy App Service-csomaghoz rendelt összes alkalmazás ugyanazokat az általa meghatározott erőforrásokat használja, így több alkalmazás üzemeltetése esetén is csökkenthetők a költségek.
>
> Az App Service-csomagok a következőket határozzák meg:
> * Régió: Észak-Európa, USA keleti régiója, Délkelet-Ázsiában, stb.
> * Példány mérete: Kicsi, közepes, nagy, stb.
> * Méretezési száma: egy, kettő vagy három példányt, stb.
> * Termékváltozat: Ingyenes, a közös, Basic, Standard, Premium, stb.

Tekintse át a metrikákat a webalkalmazás, keresse fel a **áttekintése** a figyelni kívánt alkalmazás panelen. Itt megtekintheti, az alkalmazás metrikák diagramot egy **figyelés csempe**. Kattintson a csempére kattintva szerkesszék és konfigurálják milyen mérőszámok alapján megtekintheti és a megjelenítendő időtartományát.

Alapértelmezés szerint az erőforrás panel az tartalmazza az alkalmazás kérések és hibák követése az elmúlt egy óra.
![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

Ahogy látja, a példában, kell, hogy a számos alkalmazás **HTTP-kiszolgáló hibák**. A nagy mennyiségű hibák igazolnia kell a vizsgálja meg az alkalmazás első jelzi.

> [!TIP]
> További tudnivalók az Azure-figyelő a következő hivatkozásokkal:
> - [Ismerkedés az Azure-figyelő](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Az Azure metrikák](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Azure-figyelő támogatott metrikák](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [Az Azure irányítópultok](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a>2. lépés - riasztások konfigurálása
**Riasztások** konfigurálható elindítani az alkalmazás bizonyos feltételeknek.

A [1. lépés - nézet metrikák](#metrics), látott, hogy az alkalmazás volt-e a hibák túl magas száma.

Lehetővé teszi, hogy automatikusan tájékoztatást kaphat a hibák esetén olyan riasztást konfigurálhat. Ebben az esetben azt szeretnénk, ha a riasztás küldése és minden alkalommal, amikor egy meghatározott küszöbérték feletti kerül X HTTP 50-hibák száma.

Riasztás létrehozása, navigáljon a **figyelés** > **riasztások** kattintson **[+] riasztás hozzáadása**.

![Riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

Adjon meg értékeket a riasztás konfigurálásában:
- **Erőforrás:** a helyet, hogy a riasztást figyelő.
- **Name:** egy nevet a riasztáshoz, ebben az esetben: *magas HTTP 50 X*.
- **Leírás:** egyszerű szöveges magyarázatát, mi van megnézi a riasztás.
- **Riasztás:** riasztások metrikák vagy események is megtekinthetik, ehhez a példához azt nézi metrikákat.
- **A metrika:** milyen metrika figyeléséhez, ebben az esetben: *HTTP-kiszolgáló hibák*.
- **Feltétel:** mikor kell riasztást, ebben az esetben válassza a *nagyobb, mint* lehetőséget.
- **Küszöbérték:** Mi az érték, amelyet meg kíván keresni, ebben az esetben: *400*.
- **Időszak:** riasztások hatnak a metrika az átlagos értéke. Kisebb időszakokra, yield érzékenyebb riasztásokat. Ebben az esetben azt nézi *5 perc*.
- **Tulajdonos és közreműködő szerepkörrel rendelkező személyek e-mail:** ebben az esetben: *engedélyezve*.

Most, hogy a riasztást hoz létre az e-mail elküldésekor történik, minden alkalommal, amikor az alkalmazás halad keresztül a beállított küszöbértéket. Aktív riasztások is áttekintheti az Azure portálon.

![A kiváltott riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> További tudnivalók az Azure riasztások a következő hivatkozásokkal:
> - [Mik azok a riasztások a Microsoft Azure-ban](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Művelet végrehajtása a metrikák](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Riasztások metrika létrehozása](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="companion"></a>3. lépés – az App Service kiegészítő
**App Service kiegészítő** kényelmesen a figyelheti az alkalmazást egy natív tapasztalattal a mobil eszköz (iOS vagy Android).

Az App Service kiegészítő használja:
- Tekintse át az alkalmazás metrikák
- Tekintse át és rájuk alkalmazás riasztások és javaslatok
- Alapszintű hibaelhárítás (Tallózás, elindíthatja, leállíthatja, indítsa újra az alkalmazást)
- Leküldéses értesítések kritikus események lekérése.

![Az App Service kiegészítő](media/app-service-web-tutorial-monitoring/app-service-companion.png)

[![App Service kiegészítő App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![alkalmazásszolgáltatási kiegészítő Google Play áruházból](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

Az App Service kiegészítő telepítése a [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) vagy [Google Play áruházból](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

## <a name="diagnose"></a>4. lépés: hibáinak diagnosztizálásához és elhárításához
**Hibáinak diagnosztizálásához és elhárításához** alkalmazással kapcsolatos problémák egymástól segít alkotnak szolgáltatásában. Azt is javasoljuk, hogy lehetséges megoldást lekérni a webalkalmazás ismét működik megfelelően.

![Hibáinak diagnosztizálásához és elhárításához](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

A példa űrlap előző lépést folytatva, láthatja, hogy az alkalmazás rendelkezik lett elérhetőségéről problémák történtek. A platform rendelkezésre állása ellentétben nem áthelyezte 100 %-ból.

Az alkalmazás okoz problémát, és mentése a platform marad, esetén egy tiszta jelzi, hogy a Microsoft alkalmazásproblémák foglalkoznak.

## <a name="logging"></a>– 5. lépés-naplózás
Most, hogy azt rendelkezik leszűkítheti a alkalmazásproblémák sikertelen, azt is meg az alkalmazás és a kiszolgáló talál további információkat.

Naplózás lehetővé teszi, hogy mindkét gyűjthet **Application Diagnostics** és **Web Server diagnosztika** naplók a webalkalmazás.

### <a name="application-diagnostics"></a>Application Diagnostics
Application diagnostics lehetővé teszi az alkalmazás futásidőben által előállított nyomok rögzítését.

Nyomkövetés nagy mértékben az alkalmazás hozzáadása növeli a hibakeresési és a PIN-kód-pont problémákat.

Az ASP.NET, az alkalmazások nyomkövetéseit használatával jelentkezhetnek [System.Diagnostics.Trace osztály](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) a napló infrastruktúra által rögzített események generálásához. A nyomkövetés megkönnyíti szűréshez súlyossága is megadható.

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
Ahhoz, hogy alkalmazásnaplózás Ugrás **figyelés** > **diagnosztikai naplók** és alkalmazás naplózás engedélyezése váltógombok segítségével.

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

Alkalmazásnaplók tárolja a webalkalmazás fájlrendszerre, vagy leküldött blob-tároló. Éles környezetben javasoljuk a blob storage használata.

> [!IMPORTANT]
> Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat. Éles környezetben a hibanaplókat használata ajánlott. Csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.

 ### <a name="web-server-diagnostics"></a>Web Server diagnosztika
Webkiszolgáló naplóinak akkor jönnek létre, akkor is, ha az alkalmazás nem tagolva.. App Service össze tudják gyűjteni a kiszolgáló naplóiban három különböző típusú:

- **Web Server naplózása**
    - HTTP-tranzakciók használatával kapcsolatos információkat a [W3C bővített naplófájlformátum](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
    - Akkor hasznos, például a kezelt kéréseket, és hogy hány kérésnek egy adott IP-címről van a teljes webhelymetrikák meghatározásakor.
- **Hibával kapcsolatos részletes naplózás**
    - (Állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hibával kapcsolatos részletes információk.
    - [További információ a hibával kapcsolatos részletes naplózás](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **Sikertelen kérelmek nyomkövetése**
    - Sikertelen kérelmek nyomkövetési segítségével dolgozza fel a kérelmet, és minden egyes összetevő szükséges idő az IIS-összetevők többek között részletes tájékoztatást.
    - Nem sikerült a kérelem naplók hasznosak, ha egy adott HTTP hiba próbál különítse el, mi okozza.
    - [További tudnivalók a sikertelen kérelmek nyomkövetése](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

Kiszolgáló naplózásának engedélyezése:
- Ugrás a **figyelés** > **diagnosztikai naplók**.
- A Web Server diagnosztika váltógombok segítségével különböző típusú engedélyezése.

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat. Éles környezetben hibanaplókat ajánlott, csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.

### <a name="accessing-logs"></a>Naplók elérése
A blob Storage tárolóban tárolt naplók Azure Tártallózó hozzáférnek. A webalkalmazás filesystem tárolt naplók az a következő elérési útján FTP keresztül érhetők el:

- **Alkalmazásnaplók** - `%HOME%/LogFiles/Application/`.
    - Ez a mappa tartalmaz egy vagy több alkalmazásnaplózás által előállított adatokat tartalmazó szöveges fájlt.
- **Sikertelen kérelmek nyomkövetési** - `%HOME%/LogFiles/W3SVC#########/`.
    - Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat.
- **Részletes hibanaplókat** - `%HOME%/LogFiles/DetailedErrors/`.
    - Ez a mappa tartalmaz egy vagy több, a HTTP-hibák az alkalmazás által létrehozott nagy mennyiségű vonatkozó .htm fájlt.
- **Webalkalmazás-naplói** - `%HOME%/LogFiles/http/RawLogs`.
    - Ez a mappa tartalmaz egy vagy több szövegfájlok formátuma a W3C bővített naplófájlformátum használata.

## <a name="streaming"></a>Lépés 6 - adatfolyam-napló
Folyamatos átviteli naplók esetén kényelmesen használható alkalmazás hibakeresés, mivel képest időt takaríthat [a naplók elérése](#Accessing-Logs) FTP keresztül.

App Service képes adatfolyam **alkalmazásnaplók** és **webkiszolgáló naplóinak** , akkor jönnek létre.

> [!TIP]
> Mielőtt az adatfolyam-naplókat, győződjön meg arról, hogy engedélyezte a gyűjtését naplók leírtak szerint a [naplózás](#logging) szakasz.

Az adatfolyam-naplók lépjen **figyelés**> **naplófolyamot**. Válassza ki **alkalmazásnaplók** vagy **webalkalmazás-naplói** attól függően, hogy milyen információkat keres. Itt meg is szüneteltetése, indítsa újra, és törölje a puffer.

![Folyamatos átviteli naplók](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Naplók csak akkor jön létre, amikor az alkalmazás forgalom van, megnövelheti a naplók további események és információk részletességi is.

## <a name="remote"></a>7. lépés - távoli hibakeresés
Ha Ön rendelkezik PIN-kód-jelölt alkalmazások problémák forrását, a **távoli hibakeresés** bízná a kódot.

Távoli hibakeresés lehetővé csatol hibakereső a webalkalmazás fut a felhőben. Állítson be töréspontokat, kezelheti közvetlenül a memória, kód teljesítéséhez és még a kód elérési utat módosítsa, ugyanúgy, mint a helyileg futó alkalmazást.

A hibakereső csatolása a felhőben futó alkalmazásnak:

- A Visual Studio 2017 nyissa meg a megoldást a hibakeresési kívánt alkalmazást
- Néhány berendezés pont ugyanúgy, mint a helyi fejlesztési beállítása.
- Nyissa meg **cloud explorer** (parancsra + /, ctrl + x).
- Jelentkezzen be azure hitelesítő adataival, igény szerint.
- Megkeresheti az alkalmazást hibakeresési kívánt
- Válassza ki **csatolása hibakereső** űrlap a **műveletek** ablaktáblán.

![Távoli hibakeresés](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

A Visual Studio konfigurálja a távoli hibakereséshez alkalmazást, és elindítja egy böngészőablakban, amely az alkalmazás navigál. Tallózzon a break pontok és a kód lépésenkénti elindítani az alkalmazást.

> [!WARNING]
> Éles hibakeresési módban nem ajánlott. Ha az éles alkalmazásban nem horizontálisan több kiszolgálópéldány, hibakeresés megakadályozni a webkiszolgáló egyéb kérésekre válaszol. A termelési problémák elhárításához, a legjobb erőforrás-hoz [naplózási](#logging) és [Application Insights](#insights).



## <a name="explorer"></a>8 - Process Explorert. lépés
Ha az alkalmazás csak egy példányhoz, horizontálisan **process Explorert** példány konkrét problémák azonosításához nyújt segítséget.

Használjon **Explorer feldolgozni** számára:

- A folyamatok felsorolni az App Service-csomag különböző példányai között.
- Részletekbe menően tárhatják, és tekintse meg a leírók és minden folyamathoz tartozó modulok.
- Megtekintheti a CPU, működik-e beállítva, és szálak száma a folyamat szintjén sorozatos folyamatok azonosítása
- A megnyitott fájlleíró keresse meg és még kill egy adott példányt.

Folyamat Explorer alatt található **figyelés** > **Process Explorer**.

![Process Explorer](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <a name="insights"></a>9 - Application insights szolgáltatással. lépés
**Az Application Insights** alkalmazásprofil-készítési és speciális megfigyelési képességeket biztosít az alkalmazás.

Az Application Insights segítségével észlelheti és diagnosztizálhatja a kivételeket és a webes alkalmazás teljesítményével kapcsolatos problémákat.

A webalkalmazás alatt engedélyezheti az Application Insights **figyelés** > **Application insights szolgáltatással**

> [!NOTE]
> Az Application Insights kérheti, hogy telepítse az Application Insights hely bővítmény adatgyűjtés elindításához. A webhely-bővítmény telepítése azt eredményezi, újra kell indítani az alkalmazást.

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

Az Application Insights beállítása, a további szerepel a hivatkozások követése gazdag lehetőség van a [lépések](#next) szakasz.

## <a name="next"></a> Következő lépések

 - [Mi az Application Insights](..\application-insights\app-insights-overview.md)
 - [Az Application insights szolgáltatással Azure webes alkalmazás teljesítményének figyelése](..\application-insights\app-insights-azure-web-apps.md)
 - [Rendelkezésre állás figyelése és reakcióidőt, a webhely, az Application insights szolgáltatással](..\application-insights\app-insights-monitor-web-app-availability.md)
