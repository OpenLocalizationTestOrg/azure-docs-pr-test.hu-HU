---
title: "a webes alkalmazás aaaMonitor |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset fel a figyelés a webalkalmazásban"
services: App-Service
keywords: 
author: btardif
ms.author: byvinyal
ms.date: 04/04/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: c2f5e9842c732a804f1caee5d67e53dad24e190a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-app-service"></a>Az App Service figyelése
Ez az oktatóanyag végigvezeti az alkalmazás figyelése és hello beépített platform eszközök toosolve problémák használata, azok bekövetkezésekor.

Ez a dokumentum egyes részeinek kerül egy adott funkcióhoz keresztül. Hello szolgáltatások együttes használata lehetővé teszik, hogy:
- Az alkalmazás problémát azonosítása.
- Annak meghatározása, amikor hello a problémát a kódban, illetve hello platform okozta.
- Leszűkíthet hello probléma merült fel a kód a hello forrását.
- Hibakeresés és hello a probléma kijavítása.

## <a name="before-you-begin"></a>Előkészületek
- A webalkalmazás toomonitor kell, és hajtsa végre a hello leírt lépéseket.
    - Egy alkalmazás hello a hello ismertetett lépéseket követve hozhat létre [ASP.NET-alkalmazás létrehozása az Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) oktatóanyag.

- Ha szeretné tootry **távoli hibakeresés** az alkalmazás, a Visual Studio szüksége.
    - Ha még nincs telepítve a Visual Studio 2017, letöltheti és használható szabad hello [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).
    - Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.

## <a name="metrics"></a>1. lépés – nézet metrikák
**Metrikák** hasznos toounderstand vannak:
- Alkalmazás állapotának
- Alkalmazás teljesítménye
- Erőforrás-használat

Az alkalmazás problémát vizsgál, tekintse át a metrikákat esetén egy remek toostart. Azure portálon találhat egy vizsgálja meg az alkalmazás használatának hello metrikák gyorsan toovisually **Azure figyelő**.

Metrikákat biztosít az alkalmazás több kulcs összesítésben állapotelőzményeinek a nézetét. Bármely alkalmazás az app service-ben üzemeltetett, célszerű figyelemmel kísérni hello Web App és a hello App Service-csomag.

> [!NOTE]
> Az App Service-csomag a alkalmazásai használt fizikai erőforrások toohost hello gyűjteményét képviseli. Minden alkalmazás hozzárendelt tooan az alkalmazásszolgáltatási csomag hello erőforrások megosztása határozzák meg, hogy lehetővé teszi a toosave költség esetén több alkalmazás.
>
> Az App Service-csomagok a következőket határozzák meg:
> * Régió: Észak-Európa, USA keleti régiója, Délkelet-Ázsiában, stb.
> * Példány mérete: Kicsi, közepes, nagy, stb.
> * Méretezési száma: egy, kettő vagy három példányt, stb.
> * Termékváltozat: Ingyenes, a közös, Basic, Standard, Premium, stb.

a webalkalmazás, nyissa meg toohello tooreview metrikák **áttekintése** panel hello alkalmazás toomonitor szeretné. Itt megtekintheti, az alkalmazás metrikák diagramot egy **figyelés csempe**. Hello csempe tooedit kattintson, és milyen metrikák tooview konfigurálása, valamint idő tartomány toodisplay hello.

Alapértelmezett hello erőforráspanelen biztosít egy nézet hello Alkalmazáskérelmeinek és hello elmúlt egy óra által jelzett hibákat.
![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

Hello példában látható, mert van egy alkalmazás, hogy a különböző **HTTP-kiszolgáló hibák**. hibák nagy mennyiségű hello hello első arra utal, hogy az alkalmazás kell tooinvestigate.

> [!TIP]
> További tudnivalók az Azure figyelő a következő hivatkozások hello:
> - [Ismerkedés az Azure-figyelő](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Az Azure metrikák](..\monitoring-and-diagnostics\monitoring-overview-metrics.md)
> - [Azure-figyelő támogatott metrikák](..\monitoring-and-diagnostics\monitoring-supported-metrics.md)
> - [Az Azure irányítópultok](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a>2. lépés - riasztások konfigurálása
**Riasztások** lehet konfigurált tootrigger az alkalmazás bizonyos feltételeknek.

A [1. lépés - nézet metrikák](#metrics), látott, hogy hello alkalmazás volt-e a hibák túl magas száma.

Lehetővé teszi, hogy olyan riasztást konfigurálhat tooautomatically értesítést kaphat, ha hiba lép fel. Ebben az esetben azt szeretné, hogy hello riasztási toosend és az e-mail minden alkalommal, amikor egy meghatározott küszöbérték feletti kerül hello HTTP 50 X hibák száma.

túl keresse meg az értesítés, toocreate**figyelés** > **riasztások** kattintson **[+] riasztás hozzáadása**.

![Riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

Adjon meg értékeket hello riasztás konfigurálásában:
- **Erőforrás:** hello hely toomonitor hello riasztáshoz.
- **Name:** egy nevet a riasztáshoz, ebben az esetben: *magas HTTP 50 X*.
- **Leírás:** egyszerű szöveges magyarázatát, mi van megnézi a riasztás.
- **Riasztás:** riasztások metrikák vagy események is megtekinthetik, ehhez a példához azt nézi metrikákat.
- **A metrika:** milyen metrika toomonitor, ebben az esetben: *HTTP-kiszolgáló hibák*.
- **Feltétel:** amikor tooalert, ebben az esetben válassza hello *nagyobb, mint* lehetőséget.
- **Küszöbérték:** érték toolook, újdonságok ebben az esetben: *400*.
- **Időszak:** riasztások hatnak hello átlagos értéke a metrikát. Kisebb időszakokra, yield érzékenyebb riasztásokat. Ebben az esetben azt nézi *5 perc*.
- **Tulajdonos és közreműködő szerepkörrel rendelkező személyek e-mail:** ebben az esetben: *engedélyezve*.

Most, hogy hello riasztást hoz létre egy e-mailt küld minden alkalommal, amikor hello app kerül konfigurált hello küszöbérték feletti. Aktív riasztások hello Azure-portálon is megtekinthető.

![A kiváltott riasztások](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)


> [!TIP]
> További tudnivalók az Azure riasztások a következő hivatkozások hello:
> - [Mik azok a riasztások a Microsoft Azure-ban](..\monitoring-and-diagnostics\monitoring-overview-alerts.md)
> - [Művelet végrehajtása a metrikák](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Riasztások metrika létrehozása](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="companion"></a>3. lépés – az App Service kiegészítő
**App Service kiegészítő** egy kényelmes módszert arra toomonitor kínál az alkalmazás (iOS vagy Android) egy mobileszközről natív nyújthassunk.

Az App Service kiegészítő használja:
- Tekintse át az alkalmazás metrikák
- Tekintse át és rájuk alkalmazás riasztások és javaslatok
- Alapszintű hibaelhárítás (Tallózás, start, stop, újraindítás hello alkalmazás)
- Leküldéses értesítések kritikus események lekérése.

![Az App Service kiegészítő](media/app-service-web-tutorial-monitoring/app-service-companion.png)

[![App Service kiegészítő App Store](media/app-service-web-tutorial-monitoring/app-service-companion-appStore.png)](https://itunes.apple.com/app/azure-app-service-companion/id1146659260)
[![alkalmazásszolgáltatási kiegészítő Google Play áruházból](media/app-service-web-tutorial-monitoring/app-service-companion-googlePlay.png)](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

Telepítheti az App Service kiegészítő hello [App Store](https://itunes.apple.com/app/azure-app-service-companion/id1146659260) vagy [Google Play áruházból](https://play.google.com/store/apps/details?id=azureApps.AzureApps)

## <a name="diagnose"></a>4. lépés: hibáinak diagnosztizálásához és elhárításához
**Hibáinak diagnosztizálásához és elhárításához** alkalmazással kapcsolatos problémák egymástól segít alkotnak szolgáltatásában. Azt is is javasolhatnak lehetséges megoldást tooget a webes alkalmazás hátsó toohealthy.

![Hibáinak diagnosztizálásához és elhárításához](media/app-service-web-tutorial-monitoring/app-service-monitor-diagnosis.png)

Folytatása hello példa űrlap előző lépéseket, láthatja, hogy hello alkalmazás rendelkezik lett elérhetőségéről problémák történtek. Hello platform rendelkezésre állása ellentétben nem helyezte 100 %-ból.

Ha hello alkalmazás hibát okoz, és hello platform marad be, egy tiszta jelzi, hogy a Microsoft alkalmazásproblémák foglalkoznak.

## <a name="logging"></a>– 5. lépés-naplózás
Most, hogy rendelkezik a Microsoft hello hibák tooan alkalmazásproblémák leszűkítheti, azt is megtekinthetik hello alkalmazás és a kiszolgálói naplók tooget további információt.

Naplózás lehetővé teszi mindkét toocollect **Application Diagnostics** és **Web Server diagnosztika** naplók a webalkalmazás.

### <a name="application-diagnostics"></a>Application Diagnostics
Application diagnostics lehetővé teszi, hogy Ön toocapture nyomkövetések futásidőben hello alkalmazás által létrehozott.

Nyomkövetés tooyour hozzáadása alkalmazás nagy mértékben csökkenti a képes toodebug és a PIN-kód-pont problémákat.

Az ASP.NET, az alkalmazások nyomkövetéseit használatával jelentkezhetnek [System.Diagnostics.Trace osztály](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) hello napló infrastruktúra által rögzített események toogenerate. Egyszerűbb szűréshez hello nyomkövetési hello súlyossága is megadható.

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
tooenable alkalmazásnaplózás lépjen túl**figyelés** > **diagnosztikai naplók** és alkalmazás naplózás engedélyezése hello váltógombok segítségével.

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

Alkalmazásnaplók tárolt tooyour Web App fájlrendszer vagy tooblob tárolási leküldött. Éles forgatókönyvek esetén ajánlott toouse blob-tároló.

> [!IMPORTANT]
> Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat. Éles környezetben a hibanaplókat használata ajánlott. Csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.

 ### <a name="web-server-diagnostics"></a>Web Server diagnosztika
Webkiszolgáló naplóinak akkor jönnek létre, akkor is, ha az alkalmazás nem tagolva.. App Service össze tudják gyűjteni a kiszolgáló naplóiban három különböző típusú:

- **Web Server naplózása**
    - Információ a HTTP-tranzakciókat hello segítségével [W3C bővített naplófájlformátum](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
    - Akkor hasznos, például a hello kezelt kérelmek száma, vagy hogy hány kérésnek egy adott IP-címről általános webhelymetrikák meghatározásakor.
- **Hibával kapcsolatos részletes naplózás**
    - (Állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hibával kapcsolatos részletes információk.
    - [További információ a hibával kapcsolatos részletes naplózás](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **Sikertelen kérelmek nyomkövetése**
    - Sikertelen kérelmek nyomkövetési hello IIS-összetevők többek között részletes információt tooprocess hello kérelem és hello igénybe vett idő minden összetevő használja.
    - Sikertelen kérelmek naplók hasznosak, mi okozza-e egy adott HTTP hiba tooisolate tett kísérlet során.
    - [További tudnivalók a sikertelen kérelmek nyomkövetése](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

Kiszolgáló naplózási tooenable:
- Nyissa meg túl**figyelés** > **diagnosztikai naplók**.
- Web Server diagnosztika hello váltógombok segítségével különböző típusú hello engedélyezése.

![Alkalmazás monitorozása](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> Naplózás engedélyezése hatással van az alkalmazás teljesítmény- és erőforrás-használat. Éles környezetben hibanaplókat ajánlott, csak akkor engedélyezze a részletesebb naplózás, ha problémákat vizsgálja.

### <a name="accessing-logs"></a>Naplók elérése
A blob Storage tárolóban tárolt naplók Azure Tártallózó hozzáférnek. Hello Web App filesystem tárolt naplók a következő elérési utak hello FTP keresztül érhetők el:

- **Alkalmazásnaplók** - `%HOME%/LogFiles/Application/`.
    - Ez a mappa tartalmaz egy vagy több alkalmazásnaplózás által előállított adatokat tartalmazó szöveges fájlt.
- **Sikertelen kérelmek nyomkövetési** - `%HOME%/LogFiles/W3SVC#########/`.
    - Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat.
- **Részletes hibanaplókat** - `%HOME%/LogFiles/DetailedErrors/`.
    - Ez a mappa tartalmaz egy vagy több, a HTTP-hibák az alkalmazás által létrehozott nagy mennyiségű vonatkozó .htm fájlt.
- **Webalkalmazás-naplói** - `%HOME%/LogFiles/http/RawLogs`.
    - Ez a mappa tartalmaz egy vagy több szövegfájlok formátuma hello W3C bővített naplófájlformátum használata.

## <a name="streaming"></a>Lépés 6 - adatfolyam-napló
Folyamatos átviteli naplók esetén kényelmesen használható alkalmazás hibakeresés, mivel túl képest időt takaríthat[hello naplók elérése](#Accessing-Logs) FTP keresztül.

App Service képes adatfolyam **alkalmazásnaplók** és **webkiszolgáló naplóinak** , akkor jönnek létre.

> [!TIP]
> Toostream naplók megkísérlése előtt ellenőrizze, hogy engedélyezte a gyűjtését naplók hello leírtak [naplózás](#logging) szakasz.

toostream naplókat, nyissa meg túl**figyelés**> **naplófolyamot**. Válassza ki **alkalmazásnaplók** vagy **webalkalmazás-naplói** attól függően, hogy milyen információkat keres. Itt is szüneteltetése programot, indítsa újra, és a hello puffer törölje.

![Folyamatos átviteli naplók](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> Naplók csak akkor jön létre, amikor nincs forgalom hello app, megnövelheti a naplók tooget hello részletesség is több eseményeket vagy adatokat.

## <a name="remote"></a>7. lépés - távoli hibakeresés
Ha PIN-kód jelölt hello forrás hello alkalmazások problémák rendelkezik, a **távoli hibakeresés** toowalk hello kód.

Távoli hibakeresés lehetővé csatlakoztatása a hibakereső tooyour webalkalmazás hello felhőben futó. Állítson be töréspontokat, kezelheti közvetlenül a memória, kód teljesítéséhez, és akkor is módosítani hello kódelérési-útja ugyanúgy, mint a helyileg futó alkalmazások.

tooattach hello hibakereső tooyour app hello felhőben futó:

- A Visual Studio 2017, nyissa meg hello megoldás hello alkalmazás segítségével szeretné a toodebug
- Néhány berendezés pont ugyanúgy, mint a helyi fejlesztési beállítása.
- Nyissa meg **cloud explorer** (parancsra + /, ctrl + x).
- Jelentkezzen be azure hitelesítő adataival, igény szerint.
- Keresés hello-alkalmazáshoz toodebug
- Válassza ki **csatolása hibakereső** űrlap hello **műveletek** ablaktáblán.

![Távoli hibakeresés](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

A Visual Studio konfigurálja a távoli hibakereséshez alkalmazást, és elindítja egy böngészőablakban, amely tooyour app navigál. Keresse meg az alkalmazás tootrigger break ponton keresztül, és hello kód lépéseit.

> [!WARNING]
> Éles hibakeresési módban nem ajánlott. Az éles alkalmazásban nem horizontálisan toomultiple kiszolgálópéldányok, ha hibakeresési megakadályozása hello webkiszolgáló válaszol tooother kérések. A termelési problémák elhárításához, a legjobb erőforrás túlságosan[naplózási](#logging) és [Application Insights](#insights).



## <a name="explorer"></a>8 - Process Explorert. lépés
Ha az alkalmazás egy példánya, mint toomore van horizontálisan **process Explorert** példány konkrét problémák azonosításához nyújt segítséget.

Használjon **Explorer feldolgozni** számára:

- Minden hello folyamat felsorolni az App Service-csomag különböző példányai között.
- Részletekbe menően tárhatják, és tekintse meg hello kezeli, és minden folyamathoz tartozó modulok.
- Hello nézet CPU, működik-e beállítva, és a szál darabszáma feldolgozni sorozatos folyamatok azonosította szintű toohelp
- A megnyitott fájlleíró keresse meg és még kill egy adott példányt.

Folyamat Explorer alatt található **figyelés** > **Process Explorer**.

![Process Explorer](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)


## <a name="insights"></a>9 - Application insights szolgáltatással. lépés
**Az Application Insights** alkalmazásprofil-készítési és speciális megfigyelési képességeket biztosít az alkalmazás.

Az Application Insights toodetect használni és kivételeket és a webalkalmazás a teljesítménnyel kapcsolatos problémák diagnosztizálásához.

A webalkalmazás alatt engedélyezheti az Application Insights **figyelés** > **Application insights szolgáltatással**

> [!NOTE]
> Az Application Insights kérhetik tooinstall hello Application Insights hely bővítmény toostart adatokat gyűjt. Hello hely bővítmény telepítése azt eredményezi, újra kell indítani az alkalmazást.

![Application Insights](media/app-service-web-tutorial-monitoring/app-service-monitor-appinsights.png)

Az Application Insights gazdag lehetőség van beállítva, toolearn több, hivatkozásokból hello hello szerepel [lépések](#next) szakasz.

## <a name="next"></a> Következő lépések

 - [Mi az Application Insights](..\application-insights\app-insights-overview.md)
 - [Az Application insights szolgáltatással Azure webes alkalmazás teljesítményének figyelése](..\application-insights\app-insights-azure-web-apps.md)
 - [Rendelkezésre állás figyelése és reakcióidőt, a webhely, az Application insights szolgáltatással](..\application-insights\app-insights-monitor-web-app-availability.md)
