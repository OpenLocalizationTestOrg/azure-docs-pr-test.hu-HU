---
title: "Alkalmazás védelme az Azure App Service-ben"
description: "Megtudhatja, mennyire biztonságos a webalkalmazást, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazás."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 5ce560b4-42d7-4b20-935c-0445fd539e39
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: b70d74441f3d6d9793ae516b3f04e36e786a9a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-app-in-azure-app-service"></a>Alkalmazás védelme az Azure App Service-ben
Ez a cikk segít Ismerkedés a webalkalmazást, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazásba való biztosításával. 

Az Azure App Service biztonsági két olyan szintje van: 

* **Infrastruktúra- és platform biztonsági** -megbízható Azure biztonságosan a felhőben dolgot ténylegesen futtatásához szükséges szolgáltatást.
* **Az alkalmazásbiztonság** -kell terveznie biztonságosan maga az alkalmazás. Ez magában foglalja, hogy hogyan integrálja az Azure Active Directoryval, tanúsítványok kezelése, és hogyan, győződjön meg arról, hogy képes biztonságos beszélgetés különböző szolgáltatások. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktúra- és platform biztonsági
Mivel az App Service tart fenn az Azure virtuális gépeken, tárolási, hálózati kapcsolatok, webes keretrendszerek, felügyeleti és integrációs szolgáltatások és még sok más aktívan védett, és megerősítve és erőteljes megfelelőségi végighalad, és meggyőződni arról, hogy rendszeresen ellenőrzi:

* Az App Service-alkalmazásokhoz elszigetelt, mind az internetről és a többi ügyfél Azure-erőforrások.
* A titkos kulcsokat (például kapcsolati karakterláncok) App Service-alkalmazás és más Azure-erőforrások (pl. SQL-adatbázis) erőforráscsoportban közötti kommunikáció Azure-ban marad, és nem telephelyek közötti hálózati határokat. Titkos kulcsok a rendszer mindig titkosítja.
* Az App Service-alkalmazást és a külső erőforrások, például a PowerShell-felügyeletet, a parancssori felület, a Azure SDK-k, a REST API-k és a hibrid kapcsolatok közötti összes kommunikáció megfelelően van titkosítva.
* 24 órás threat management megvédi az App Service-erőforrások a kártevő szoftverek, elosztott szolgáltatásmegtagadásos (DDoS-), man az-a-elkövetett (MITM), és más fenyegetésekkel szemben. 

Az Azure-infrastruktúra és a platform biztonsági további információkért lásd: [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/security/).

#### <a name="application-security"></a>Az alkalmazásbiztonság
Az infrastruktúra és a platform, amely az alkalmazás fut, a történő biztonságba helyezéséért felelős Azure pedig feladata a biztonságos magának az alkalmazásnak. Más szóval kell történő fejlesztéséhez, telepítheti és kezelheti az alkalmazás kódjában és a tartalom biztonságos módon. E nélkül az alkalmazás kódja vagy a tartalom továbbra is történhet ki van téve a fenyegetéseket, mint:

* SQL-injektálás
* Munkamenet-eltérítés
* Kereszt-helyközi
* Alkalmazásszintű MITM
* Alkalmazásszintű DDoS

Biztonsági szempontok a web-alapú alkalmazások ismertetését nem a jelen dokumentum terjed. Kiindulási pontként további útmutatást a biztonságossá tétele az alkalmazás számára, lásd: a [megnyitása webes alkalmazás biztonsági Project (OWASP)](https://www.owasp.org/index.php/Main_Page), kifejezetten a [első 10 projekt.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), amely tartalmazza az aktuális első 10 kritikus webes alkalmazás biztonsági hibái, OWASP tagok alapján.

## <a name="perform-penetration-testing-on-your-app"></a>Hajtsa végre a behatolást vagy a biztonság a az alkalmazás tesztelése
Ismerkedjen meg a biztonsági réseket az App Service-alkalmazás tesztelése a legegyszerűbben egyik használja a [integráció a Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) vizsgálja meg az alkalmazást egy kattintással biztonsági rés végrehajtásához. A teszteredmények megtekintéséhez könnyen érthető jelentésben, és részletes utasításokat tartalmaz az egyes biztonsági kijavításához.

Ha inkább a saját behatolást vagy a biztonság tesztek kerülnek végrehajtásra, vagy egy másik képolvasó suite vagy szolgáltatót szeretne használni, kövesse a [jóváhagyási folyamat tesztelése az Azure behatolás](https://security-forms.azure.com/penetration-testing/terms) és előzetes jóváhagyásának a kívánt behatolás tesztek végrehajtásához.

## <a name="https"></a>Az ügyfelek biztonságos kommunikáció
Használatakor a  **\*. azurewebsites.net** App Service-alkalmazás létre tartománynevet is azonnal a HTTPS PROTOKOLLT használja, az SSL-tanúsítvány biztosított összes  **\*. azurewebsites.net** tartománynevek. Ha a hely által használt egy [egyéni tartománynév](app-service-web-tutorial-custom-domain.md), egy SSL-tanúsítványt feltöltheti [HTTPS engedélyezése](app-service-web-tutorial-custom-ssl.md) az egyéni tartomány.

Engedélyezés [HTTPS](https://en.wikipedia.org/wiki/HTTPS) segíthet a közbeékelődéses támadások Kiküszöbölésében a az alkalmazás és a felhasználók közötti kommunikáció védelméhez.

## <a name="secure-data-tier"></a>Biztonságos adatszint
App Service erősen integrálódik az SQL-adatbázis, hogy a kapcsolati karakterláncok titkosítása a tábla között, és csak lesznek visszafejtve, amely az alkalmazás fut a virtuális gépen *és* csak az alkalmazás futtatásakor. Ezenkívül az Azure SQL Database sok biztonsági szolgáltatásai segítségével biztosíthatja a számítógépes érintő fenyegetésekkel szemben, beleértve az alkalmazásadatok [nyugalmi titkosítási](https://msdn.microsoft.com/library/dn948096.aspx), [mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx), [dinamikus Adatmaszkolási](../sql-database/sql-database-dynamic-data-masking-get-started.md), és [Fenyegetésészlelés](../sql-database/sql-database-threat-detection.md). Ha a bizalmas adatokat, vagy megfelelőségi követelménynek, olvassa el [SQL-adatbázisok védelme](../sql-database/sql-database-security-overview.md) olvashat az adatok védelme.

Ha használ egy külső adatbázis-szolgáltató, például a ClearDB, forduljon közvetlenül az ajánlott biztonsági eljárások a szolgáltató dokumentációját.  

## <a name="develop"></a>Biztonságos fejlesztési és üzembe helyezés
### <a name="publishing-profiles-and-publish-settings"></a>Profilok közzététele és a közzétételi beállítások
Amikor fejleszt alkalmazásokat, kezelési feladatainak végrehajtása, vagy segédprogramok használatával például a feladatok automatizálásának **Visual Studio**, **Web Matrix**, **Azure PowerShell** vagy a **Azure parancssori felület (CLI)**, választhat egy *közzétételi beállítások* fájl vagy egy *közzétételi profil*. Mindkét fájltípusok hitelesíteni tudja Önt Azure-ral, és történő jogosulatlan hozzáférés elkerülése titkosítani kell.

* A **közzétételi beállítások** fájl tartalmaz
  
  * Azure-előfizetése Azonosítóját
  * Egy felügyeleti tanúsítványt, amely lehetővé teszi az előfizetéshez tartozó felügyeleti feladatok elvégzésére *felhasználónév és jelszó megadása nélkül*.
* A **közzétételi profil** fájl tartalmaz
  
  * Az alkalmazás-közzététel információi

Ha egy segédprogram, amely a közzétételi beállítások fájlja vagy a közzétételi profil fájlt használja, importálja a fájlt, a segédprogram profil vagy a közzétételi beállításokat tartalmazó, majd **törlése** a fájlt. Ha a fájl megosztása másokkal, például a projekten dolgozik mindig tárolja biztonságos helyen, mint egy *titkosított* címtárát a korlátozott engedélyekkel.

Emellett meg kell győződnie arról az importált hitelesítő adatok biztonságáról. Például **Azure PowerShell** és a **Azure parancssori felület (CLI)** mind az importált adatok tárolásához a **kezdőkönyvtár** ( *~*  a Linux vagy OS X rendszerben és */felhasználó/felhasználónév* Windows rendszeren.) További biztonsági okokból érdemes lehet **titkosítása** ezeket a helyeket titkosítás által biztosított eszközökkel, az operációs rendszernek.

### <a name="configuration-settings-and-connection-strings"></a>A konfigurációs beállításokat és kapcsolati karakterláncok
Általános gyakorlat, hogy a konfigurációs fájlban tárolhatja a kapcsolati karakterláncokat, hitelesítő adatok és más bizalmas adatokat is. Sajnos ezeket a fájlokat a webhelyen elérhetővé tett, vagy be van jelölve, egy nyilvános tárházat, ez az információ az ilyen be. Az egyszerű keresés [GitHub](https://github.com), például felfedhetők a nyilvános adattárak kitett titkos kulcsainak számos konfigurációs fájlok.

Az ajánlott eljárás az adatok megőrzése ezeket az információkat az alkalmazás konfigurációs fájlok kívül. App Service lehetővé teszi a konfigurációs adatok tárolásához, a futtatókörnyezet részeként **Alkalmazásbeállítások** és **kapcsolati karakterláncok**. Az alkalmazás futásidőben keresztül érhetők el az értékeket *környezeti változók* a legtöbb programozási nyelv. .NET-alkalmazások ezek az értékek vannak be a nézetmodellbe, a .NET-konfiguráció futásidőben. Ezekben a helyzetekben, leszámítva a konfigurációs beállítások is titkosítva maradnak kivéve, ha a megtekintése, vagy állítsa be ezeket, használja a [Azure Portal](https://portal.azure.com) vagy segédprogramok például a PowerShell vagy az Azure parancssori felület. 

Konfigurációs adatok tárolása az App Service-ben lehetővé teszi az alkalmazás-rendszergazda az üzemi alkalmazások bizalmas adatokat zárolását. A fejlesztők olyan konfigurációs beállításokat külön készletét használható alkalmazások fejlesztéséhez és a beállítások automatikusan felülírják az App Service-ben konfigurált beállítások szerint. Még a fejlesztők a titkos kulcsok beállítást az éles alkalmazások tudnia kell. Az App Service Alkalmazásbeállítások és kapcsolati karakterláncok konfigurálásáról további információkért lásd: [webalkalmazások konfigurálása](web-sites-configure.md).

### <a name="ftps"></a>FTPS
Az Azure App Service biztonságos FTP hozzáférést biztosít a fájlrendszer az alkalmazás keresztül **ftps-t**. Ez lehetővé teszi, hogy biztonságosan férhetnek hozzá az alkalmazás kódjának a webalkalmazáshoz, valamint a diagnosztikai naplókat. Javasoljuk, hogy mindig használjon FTPS FTP helyett. 

Az alkalmazás a FTPS hivatkozás található a következő lépések:

1. Nyissa meg a [Azure-portálon](https://portal.azure.com).
2. Válassza ki **összes tallózása**.
3. Az a **Tallózás** panelen válassza **alkalmazásszolgáltatások**.
4. Az a **alkalmazásszolgáltatások** panelen válassza ki a kívánt alkalmazást.
5. Válassza ki az alkalmazás paneljén **összes beállítás**.
6. Az a **beállítások** panelen válassza **tulajdonságok**.
7. Az FTP és FTPS hivatkozásokkal érhető el a a **beállítások** panelen. 

Ftps-t a további információkért lásd: [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Következő lépések
Kapcsolatos további adatokat az Azure platformon, a jelentési adatokat a biztonsági egy **biztonsági incidens- vagy visszaélés**, vagy az információt, amely kíván Microsoft **behatolást vagy a biztonság tesztelés** a webhely biztonsági című szakaszában talál a [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

További információ a **web.config** vagy **applicationhost.config** fájlok az App Service apps, lásd: [konfigurációs beállításokat az Azure App Service web apps nyitása](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Naplózási információk az App Service-alkalmazások hasznosak lehetnek a támadások, információt [diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).

> [!NOTE]
> Ha az Azure App Service első lépései az Azure-fiók regisztrálása előtt szeretné, folytassa a [App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű alkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

