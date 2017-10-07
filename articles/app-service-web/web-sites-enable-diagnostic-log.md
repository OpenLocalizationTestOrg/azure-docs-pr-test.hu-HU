---
title: "az Azure App Service web Apps aaaEnable diagnosztikai naplózás"
description: "Megtudhatja, hogyan tooenable diagnosztikai naplózás és instrumentation tooyour-alkalmazások hozzáadásához, is, hogyan tooaccess hello Azure által naplózott adatokat."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b2903ff31cc93180552cf51196c33505ffbaf07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Az Azure App Service web Apps diagnosztikai naplózás engedélyezése
## <a name="overview"></a>Áttekintés
Azure biztosít a hibakeresés beépített diagnosztika tooassist egy [App Service webalkalmazásba](http://go.microsoft.com/fwlink/?LinkId=529714). Ebben a cikkben megtudhatja, hogyan tooenable diagnosztikai naplózás és instrumentation tooyour-alkalmazások hozzáadásához, is, hogyan tooaccess hello Azure által naplózott adatokat.

Ebben a cikkben az hello [Azure Portal](https://portal.azure.com), Azure PowerShell és hello Azure parancssori felület (CLI) toowork a diagnosztikai naplókat. Diagnosztikai naplók Visual Studio használatával használatával kapcsolatos információkért lásd: [Azure hibaelhárítása a Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web server diagnostics és az application diagnostics
App Service web apps naplóinformációk hello webkiszolgáló és a webes alkalmazás hello diagnosztikai funkciók biztosítanak. Ezek logikailag rétegekben **web server diagnosztika** és **alkalmazásdiagnosztika**.

### <a name="web-server-diagnostics"></a>Web server diagnosztika
Engedélyezheti vagy letilthatja a következő naplók típusú hello:

* **Részletes naplózás hiba** -részletes (állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hiba adatait. Ez tartalmazhat, amelyek segíthetnek meghatározni, miért hello server hello hibakódot adott vissza adatokat.
* **Sikertelen kérelmek nyomkövetésére vonatkozó** – részletes információk a sikertelen kérelmek nyomkövetési hello IIS használt összetevőknek tooprocess hello kérés és hello időn belül az egyes összetevők többek között. Ez akkor lehet hasznos, ha tooincrease helyteljesítmény próbált vagy különítse el, mi okozza-e egy adott HTTP hiba toobe adott vissza.
* **Webalkalmazás-kiszolgáló naplózza a** -információ a HTTP-tranzakciókat hello segítségével [W3C bővített naplófájlformátum](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Ez akkor hasznos, ha annak meghatározása, általános webhelymetrikák például hello kezelt kérelmek száma, és hogy hány kérésnek tartoznak, amely egy adott IP-cím.

### <a name="application-diagnostics"></a>Application diagnostics
Application diagnostics lehetővé teszi egy webes alkalmazás által létrehozott toocapture adatok. ASP.NET alkalmazások használhatják hello [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) osztály toolog információk toohello alkalmazás diagnosztikai naplófájlok. Példa:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Hibaelhárítás a naplók toohelp futásidőben kérheti le. További információkért lásd: [hibaelhárítási Azure web Apps alkalmazások, a Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

App Service web apps is naplózza az információkat, amikor a tartalom tooa webes alkalmazás közzététele. Ez automatikusan megtörténik, és nincsenek telepítési naplózás konfigurációs beállítások. Központi telepítés naplózási lehetővé teszi a központi telepítés sikertelenségének toodetermine. Például ha egy egyéni telepítési parancsfájl használ, miért hello parancsfájl nem működik a központi telepítés naplózási toodetermine használhatja.

## <a name="enablediag"></a>Hogyan tooenable diagnosztika
hello tooenable diagnosztika [Azure Portal](https://portal.azure.com), nyissa meg a webalkalmazás toohello panelen, és kattintson **beállítások > diagnosztikai naplók**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Naplók része](./media/web-sites-enable-diagnostic-log/logspart.png)

Amikor engedélyezi a **alkalmazásdiagnosztika** meg hello **szint**. Ez a beállítás lehetővé teszi toofilter hello információk túl rögzített**tájékoztató**, **figyelmeztetés** vagy **hiba** információkat. Beállítás túl**részletes** hello alkalmazás által létrehozott összes információkat naplózza.

> [!NOTE]
> Hello web.config módosításától fájl alkalmazásdiagnosztika engedélyezése vagy diagnosztikai naplózási szintek módosítása nem indul hello alkalmazás belül futó hello alkalmazástartományban.
>
>

A hello [klasszikus portál](https://manage.windowsazure.com) webalkalmazás **konfigurálása** lapon választhat **tárolási** vagy **fájlrendszer** a **webkiszolgáló naplózás**. Kiválasztása **tárolási** lehetővé teszi az tooselect egy tárfiókot, és egy blob-tároló, amely hello naplók lesz írva. A többi naplófájlt **diagnosztika hely** csak toohello fájlrendszer készültek.

Hello [klasszikus portál](https://manage.windowsazure.com) webalkalmazás **konfigurálása** lap is tartalmaz további beállításokat az application diagnostics:

* **Fájlrendszer** -tárolók hello alkalmazás diagnosztikai információk toohello web app fájlrendszer. Ezek a fájlok FTP érhető el, vagy letöltött Zip-archívum létrehozása, hello Azure PowerShell vagy Azure parancssori felület (CLI) használatával.
* **TABLE storage** -tárolók hello alkalmazás diagnosztikai információkat hello megadott Azure Storage-fiókot és a tábla neve.
* **BLOB-tároló** -tárolók hello alkalmazás diagnosztikai információkat hello megadott Azure-Tárfiókot és blob-tárolóban.
* **Megőrzési időtartam** -alapértelmezés szerint naplók nem automatikusan törlődjenek **blob-tároló**. Válassza ki **beállítani a megőrzési** , és írja be a hello napok számát, amelyek tookeep naplókat, ha tooautomatically naplók törlése.

> [!NOTE]
> Ha Ön [a tárfiók tárelérési kulcsok újragenerálása](../storage/common/storage-create-storage-account.md), hello megfelelő naplózási konfiguráció toouse frissített hello kulcsok kell visszaállítani. toodo ezt:
>
> 1. A hello **konfigurálása** lapján állítsa be a megfelelő hello szolgáltatás túl**ki**. A beállítás mentése.
> 2. Ismét engedélyezi a naplózást toohello tárolási fiók blob vagy tábla. A beállítás mentése.
>
>

Fájlrendszer, a table storage vagy a blob storage kombinációja hello azonos időben, és egyes naplózási szintekhez rendelkezik, akkor engedélyezhető. Például előfordulhat, hogy kívánja toolog hibák és figyelmeztetések tooblob tárolási hosszú távú naplózási megoldásként engedélyezésekor a rendszer fájlnaplózási részletes szintű.

Amíg az összes három tárolási helyét adja meg a hello naplózott események, ugyanazokat az alapvető információkat **table storage** és **blob-tároló** például hello Példányazonosító Szálazonosító vagy a még további információk naplózása részletes időbélyeg (osztásjelek formátum) mint naplózás túl**fájlrendszer**.

> [!NOTE]
> A tárolt információk **table storage** vagy **blob-tároló** csak segítségével férhetők el egy tárolási ügyfél vagy egy alkalmazás, amely közvetlenül a tárolórendszerek használható. Például a Visual Studio 2013 tartalmazza, amelyek lehetnek használt tooexplore tábla vagy a blob storage Tártallózóval, és HDInsight férhetnek hozzá a blob storage-ban tárolt adataihoz. Egy alkalmazás, aki hozzáfér az Azure Storage hello egyikének használatával is írhat [Azure SDK-k](/downloads/#).
>
> [!NOTE]
> Diagnosztika is engedélyezhető az Azure PowerShell használatával hello **Set-AzureWebsite** parancsmag. Ha nem telepítette az Azure PowerShell, vagy nem konfigurált azt toouse az Azure-előfizetéshez, lásd: [hogyan tooUse Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

## <a name="download"></a>Hogyan: naplók letöltése
Diagnosztikai adatok tárolt toohello web app fájlrendszer elérése közvetlenül az FTP használatával. Az Azure PowerShell vagy hello Azure parancssori felület használatával Zip-archívum létrehozása, is letölthetők.

hello naplója hello könyvtárstruktúrát a következőképpen történik:

* **Alkalmazásnaplók** -/LogFiles/alkalmazás /. Ez a mappa tartalmaz egy vagy több alkalmazásnaplózás által előállított adatokat tartalmazó szöveges fájlt.
* **Sikertelen kérelmek nyomkövetési** -/ naplófájlok/W3SVC ### /. Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat. Győződjön meg arról, a hello könyvtárába hello XML fájl, mert hello XSL-fájl formázás és szűrés hello XML hello tartalmát funkciókat kínál az Internet Explorer fájl hello XSL-fájl letöltése.
* **Részletes hibanaplókat** -/LogFiles/DetailedErrors /. Ez a mappa tartalmaz egy vagy több HTTP-hibaüzenetek történt széleskörű információkat biztosító .htm fájlt.
* **Webalkalmazás-naplói** -/LogFiles/http/RawLogs. Ez a mappa tartalmaz egy vagy több szövegfájlok hello fájlrendszerrel formázott [W3C bővített naplófájlformátum](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Telepítési naplói** -/ naplófájlok/Git. Ez a mappa hello belső folyamatok használják az Azure web Apps alkalmazások által létrehozott naplók tartalmazza, valamint a Git-telepítésekhez naplózza.

### <a name="ftp"></a>FTP
diagnosztikai adatok tooaccess FTP, látogasson el hello segítségével **irányítópult** hello a webalkalmazás [klasszikus portál](https://manage.windowsazure.com). A hello **gyors áttekintő** területen hello használata **FTP diagnosztikai naplók** tooaccess hello naplófájlok, FTP használatával hivatkozásra. Hello **telepítési/FTP-felhasználó** bejegyzést kell használt tooaccess hello FTP-hely hello felhasználónév sorolja fel.

> [!NOTE]
> Ha hello **telepítési/FTP-felhasználó** bejegyzés nincs megadva, vagy elfelejtette hello jelszót a felhasználónak, új felhasználói fiókot és jelszót hozhat létre hello segítségével **üzembe helyezési hitelesítő adatok alaphelyzetbe** hello hivatkozás**gyors áttekintő** hello szakasza **irányítópult**.
>
>

### <a name="download-with-azure-powershell"></a>Töltse le az Azure PowerShell használatával
toodownload hello naplófájlokat, indítsa el az Azure PowerShell egy új példányát, és a következő parancs hello használata:

    Save-AzureWebSiteLog -Name webappname

Hello naplók hello által megadott hello webalkalmazás megtakarít **-név** nevű tooa paraméterfájl **logs.zip** hello aktuális könyvtárban található.

> [!NOTE]
> Ha nem telepítette az Azure PowerShell, vagy nem konfigurált azt toouse az Azure-előfizetéshez, lásd: [hogyan tooUse Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="download-with-azure-command-line-interface"></a>Töltse le az Azure parancssori felülettel
toodownload hello naplófájlokat hello Azure parancssori felület, nyisson meg egy új parancssor, PowerShell, Bash vagy terminál-munkamenetet, és írja be a következő parancs hello:

    azure site log download webappname

Hello webes alkalmazás neve "webappname" tooa fájlt hello naplókat megtakarít **diagnostics.zip** hello aktuális könyvtárban található.

> [!NOTE]
> Ha még nem telepítette a hello Azure parancssori felület (CLI), vagy nem konfigurált azt toouse az Azure-előfizetéshez, lásd: [hogyan tooUse Azure CLI](../cli-install-nodejs.md).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Útmutató: az Application Insightsban naplók megtekintése
A Visual Studio Application Insights szűrési és keresési naplókat, valamint a válaszoknak az összekapcsolása hello naplók kérések és más eseményekkel eszközöket biztosít.

1. Hello Application Insights SDK tooyour projekt hozzáadása a Visual Studióban.
   * A Megoldáskezelőben kattintson a jobb gombbal a projekt, és válassza ki az Application Insights hozzáadása. Lesz, amely tartalmazza az Application Insights-erőforrás létrehozása lépések interaktív. [További információ](../application-insights/app-insights-asp-net.md)
2. Hello nyomkövetés-figyelő csomag tooyour projekt hozzáadása.
   * Kattintson a jobb gombbal a projekt, és válassza ki a NuGet-csomagok kezelése. Válassza ki `Microsoft.ApplicationInsights.TraceListener` [további](../application-insights/app-insights-asp-net-trace-logs.md)
3. Töltse fel a projektet, és futtassa toogenerate naplóadatait.
4. A hello [Azure Portal](https://portal.azure.com/), keresse meg a tooyour új Application Insights-erőforrást, és nyissa meg a **keresési**. Látni fogja, a naplóadatok kérelem, használatának és egyéb telemetriai együtt. Néhány telemetriai eltarthat néhány percig tooarrive: kattintson a frissítés parancsra. [További információ](../application-insights/app-insights-diagnostic-search.md)

[További információ a teljesítmény nyomon követése az Application insights szolgáltatással](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a>Hogyan: adatfolyam-naplók
Az alkalmazások fejlesztése során is gyakran hasznos toosee naplózási információk közel valós időben. A folyamatos naplózási információk tooyour fejlesztési környezetet az Azure PowerShell vagy hello Azure parancssori felület használatával lehet elvégezni.

> [!NOTE]
> Bizonyos típusú naplózási puffer írási toohello naplófájlt, amely a hello adatfolyam üzemen kívüli események eredményezhet. Például egy alkalmazás naplóbejegyzés, amely akkor fordul elő, amikor a felhasználó meglátogat egy lap esetleg jelennek meg hello adatfolyam hello megfelelő HTTP naplóbejegyzés hello lapkérelem előtt.
>
> [!NOTE]
> Adatfolyam-naplót is adatfolyam hello tárolt tooany szövegfájl írt adatok **D:\\otthoni\\naplófájlok\\**  mappa.
>
>

### <a name="streaming-with-azure-powershell"></a>Adatfolyam-Azure PowerShell használatával
toostream naplóinformációk, indítsa el az Azure PowerShell egy új példányát, és a következő parancs hello használata:

    Get-AzureWebSiteLog -Name webappname -Tail

Ezzel összeköti hello által megadott toohello webalkalmazás **-név** paraméter és újra kell kezdenie a streaming információ toohello PowerShell ablakban hello webalkalmazásban alkalmazásnapló-események bekövetkezésekor. Bármely toofiles .txt, .log vagy .htm befejezési hello (d:/otthoni/naplófájlok) /LogFiles könyvtárban tárolt írt adatok adatfolyamként továbbítva lesz toohello helyi konzolon.

adott események toofilter, például a hibák, használja a hello **-üzenet** paraméter. Példa:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

toofilter adott napló típusok, például HTTP, használja a hello **-elérési út** paraméter. Példa:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

toosee használata hello - ListPath paraméter elérhető elérési utak listája.

> [!NOTE]
> Ha nem telepítette az Azure PowerShell, vagy nem konfigurált azt toouse az Azure-előfizetéshez, lásd: [hogyan tooUse Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Adatfolyam-Azure parancssori felületével
toostream naplóinformációk, nyisson meg egy új parancssor, PowerShell, Bash vagy terminál-munkamenetet, és írja be a következő parancs hello:

    azure site log tail webappname

A "webappname" nevű toohello webes alkalmazás csatlakoztatása és kezdete streaming adatok toohello ablakban hello webalkalmazásban alkalmazásnapló-események bekövetkezésekor. Bármely toofiles .txt, .log vagy .htm befejezési hello (d:/otthoni/naplófájlok) /LogFiles könyvtárban tárolt írt adatok adatfolyamként továbbítva lesz toohello helyi konzolon.

adott események toofilter, például a hibák, használja a hello **--szűrő** paraméter. Példa:

    azure site log tail webappname --filter Error

toofilter adott napló típusok, például HTTP, használja a hello **--elérési** paraméter. Példa:

    azure site log tail webappname --path http

> [!NOTE]
> Ha nem telepített Azure parancssori felület hello, vagy nem konfigurálta az toouse az Azure-előfizetéshez, lásd: [hogyan tooUse Azure parancssori felület](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Hogyan: diagnosztikai naplók megértése
### <a name="application-diagnostics-logs"></a>Application diagnostics naplók
Application diagnostics a .NET-alkalmazások, attól függően, hogy a naplók toohello fájlrendszer, a table storage tárolja, és hogy a blob-tároló adott formátumú adatokat tartalmazza. hello alapkészlete tárolt adatok hello azonos összes három tárolási típusa – hello dátum és idő hello esemény történt, hello esemény, hello esemény típusa (információ, figyelmeztetés, hiba) és hello eseményüzenet előállított hello Folyamatazonosító.

**Fájlrendszer**

Minden egyes sorban naplózott toohello fájlrendszerrel, vagy használja a streaming kapott hello formátuma a következő lesz:

    {Date}  PID[{process id}] {event type/level} {message}

Például egy hibaesemény jelent hasonló toohello következő:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on hello page!

Toohello fájlrendszer naplózási információk hello legalapvetőbb hello három elérhető módszerek, így csak hello idő, folyamatazonosító, Eseményszint és üzenet.

**Table Storage**

Tootable tárolási bejelentkezéskor további tulajdonságok: használt toofacilitate keresés hello tábla, valamint a hello esemény részletesebb tájékoztatást a hello adataihoz. hello következő tulajdonságai (oszlop) használt minden egyes entitásnál (sor) hello tábla tárolja.

| Tulajdonság neve | / Formátumban |
| --- | --- |
| PartitionKey |Dátum és idő formátumban yyyyMMddHH hello esemény |
| RowKey |A GUID-érték, amely egyedileg azonosítja az ehhez az entitáshoz |
| időbélyeg |hello dátuma és időpontja hello esemény történt |
| EventTickCount |Osztásjelek formátumban (nagyobb pontosságú) hello dátuma és időpontja hello esemény történt |
| ApplicationName |hello webes alkalmazás neve |
| Szint |Eseményszint (pl. hiba, figyelmeztetés, információ) |
| Eseményazonosító |Ez az esemény hello esemény azonosítója<p><p>Ha nincs megadva az alapértelmezett érték too0 |
| instanceId |Történt még hello hello webalkalmazás példánya |
| Azonosítója (PID) |Folyamatazonosító |
| TID |hello Szálazonosító hello szál előállított hello esemény |
| Üzenet |Üzenet esemény részletei |

**Blob Storage**

Naplózás tooblob tárolási, adat tárolja vesszővel tagolt (CSV) formátumban. Hasonló tootable tárolási, szükség további mezőkre naplózott tooprovide hello esemény részletesebb információt. hello következő tulajdonságai szerepelnek hello CSV minden egyes sorára:

| Tulajdonság neve | / Formátumban |
| --- | --- |
| Dátum |hello dátuma és időpontja hello esemény történt |
| Szint |Eseményszint (pl. hiba, figyelmeztetés, információ) |
| ApplicationName |hello webes alkalmazás neve |
| instanceId |Történt az esemény hello hello webalkalmazás példánya |
| EventTickCount |Osztásjelek formátumban (nagyobb pontosságú) hello dátuma és időpontja hello esemény történt |
| Eseményazonosító |Ez az esemény hello esemény azonosítója<p><p>Ha nincs megadva az alapértelmezett érték too0 |
| Azonosítója (PID) |Folyamatazonosító |
| TID |hello Szálazonosító hello szál előállított hello esemény |
| Üzenet |Üzenet esemény részletei |

a blob hello adataihoz hasonló toohello következő lenne:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> első sor hello hello napló ebben a példában határozzák hello oszlopfejléceket tartalmazza.
>
>

### <a name="failed-request-traces"></a>Sikertelen kérelmek nyomkövetési
Sikertelen kérelmek nyomkövetési nevű XML-fájlok tárolják **fr ### .xml**. toomake azt könnyebb tooview hello naplózott információk, nevű XSL-stíluslap **freb.xsl** megadott hello azonos directory, az XML-fájlok hello. Az Internet Explorer hello XML-fájlok megnyitása hello XSL-stíluslap tooprovide formázott megjeleníti a hello nyomkövetési adatokat fogja használni. Ez hasonló toohello következő jelenik meg:

![sikertelen kérelmek hello böngészőben megtekintett](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Részletes hibanaplókat.
Részletes hibanaplókat a HTML-dokumentumok, amelyek részletes információkat biztosítanak történt a HTTP-hibák. Mivel azok csak a HTML-dokumentumok, azok megtekinthetők a webböngésző.

### <a name="web-server-logs"></a>Webkiszolgáló naplóinak
hello webkiszolgáló naplóinak használatával formázott hello [W3C bővített naplófájlformátum](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Ezeket az információkat egy szövegszerkesztővel olvasható, vagy a Delimiters segédprogramok például [napló elemző](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> hello Azure web Apps alkalmazások által létrehozott naplók nem támogatják a hello **s-computername**, **s-ip**, vagy **cs-version** mezőket.
>
>

## <a name="nextsteps"></a> Következő lépések
* [Hogyan tooMonitor webalkalmazások](http://docs.microsoft.com/en-us/azure/app-service-web/web-sites-monitor)
* [Hibaelhárítás a Visual Studio Azure-webalkalmazásokban](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Elemzése a Hdinsightban webalkalmazás-naplói](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
>
>

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)
* Egy útmutató toohello hello régi portál toohello új portál módosítását lásd: [navigáláshoz hivatkozás hello Azure-portálon](http://go.microsoft.com/fwlink/?LinkId=529715)
