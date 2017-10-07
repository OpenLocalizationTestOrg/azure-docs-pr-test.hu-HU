---
title: "az Azure App Service alkalmazás aaaSecure"
description: "Megtudhatja, hogyan toosecure egy webalkalmazást, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazásba."
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
ms.openlocfilehash: fceef7963b29516f33602dcd50591d14309bf188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-app-in-azure-app-service"></a>Alkalmazás védelme az Azure App Service-ben
Ez a cikk segít Ismerkedés a webalkalmazást, mobil-háttéralkalmazás vagy az Azure App Service API-alkalmazásba való biztosításával. 

Az Azure App Service biztonsági két olyan szintje van: 

* **Infrastruktúra- és platform biztonsági** -megbízhatóság, az Azure toohave hello szolgáltatások, futtassa tooactually dologra biztonságosan hello felhőben van szükség.
* **Az alkalmazásbiztonság** -toodesign hello alkalmazás maga biztonságos helyen kell. Ez magában foglalja, hogy hogyan integrálja az Azure Active Directoryval, hogyan kezelheti a tanúsítványok és hogyan, győződjön meg arról, hogy toodifferent szolgáltatások biztonságos beszélgetés is. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktúra- és platform biztonsági
Mivel az App Service karbantartja hello Azure virtuális gépeken, tárolási, hálózati kapcsolatok, webes keretrendszerek, felügyeleti és integrációs szolgáltatások és még sok más aktívan védett, és megerősítve és erőteljes megfelelőségi végighalad és, hogy egy folyamatos toomake ellenőrzése amely:

* Az App Service-alkalmazásokhoz el különítve mindkét hello Internet, és a többi ügyfél Azure-erőforrások hello.
* A titkos kulcsokat (például kapcsolati karakterláncok) App Service-alkalmazás és más Azure-erőforrások (pl. SQL-adatbázis) erőforráscsoportban közötti kommunikáció Azure-ban marad, és nem telephelyek közötti hálózati határokat. Titkos kulcsok a rendszer mindig titkosítja.
* Az App Service-alkalmazást és a külső erőforrások, például a PowerShell-felügyeletet, a parancssori felület, a Azure SDK-k, a REST API-k és a hibrid kapcsolatok közötti összes kommunikáció megfelelően van titkosítva.
* 24 órás threat management megvédi az App Service-erőforrások a kártevő szoftverek, elosztott szolgáltatásmegtagadásos (DDoS-), man az-a-elkövetett (MITM), és más fenyegetésekkel szemben. 

Az Azure-infrastruktúra és a platform biztonsági további információkért lásd: [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/security/).

#### <a name="application-security"></a>Az alkalmazásbiztonság
Bár az Azure hello infrastruktúra és az alkalmazás fut, a platform történő biztonságba helyezéséért felelős, ez a beállítás a felelősség toosecure magának az alkalmazásnak. Más szóval szüksége toodevelop, telepítését és kezelését az alkalmazás kódjában és a tartalom biztonságos módon. E nélkül az alkalmazás kódja vagy a tartalom továbbra is történhet sebezhető toothreats, mint:

* SQL-injektálás
* Munkamenet-eltérítés
* Kereszt-helyközi
* Alkalmazásszintű MITM
* Alkalmazásszintű DDoS

Biztonsági szempontok a web-alapú alkalmazások ismertetését nem ez a dokumentum hello terjed. Kiindulási pontként további útmutatást biztonságossá tétele az alkalmazás számára, lásd: hello [megnyitása webes alkalmazás biztonsági Project (OWASP)](https://www.owasp.org/index.php/Main_Page), kifejezetten hello [első 10 projekt.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), mely listák hello aktuális felső 10 kritikus webes alkalmazás biztonsági hiányosságokat, OWASP tagok alapján.

## <a name="perform-penetration-testing-on-your-app"></a>Hajtsa végre a behatolást vagy a biztonság a az alkalmazás tesztelése
A tesztelés hello legegyszerűbb módja tooget egyik használatába, az App Service-alkalmazás a biztonsági rések toouse hello [integráció a Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) tooperform egy kattintással biztonsági rés vizsgálja meg az alkalmazást. Könnyen érthető jelentésben hello teszteredmények megtekintéséhez, és megtudhatja, hogyan toofix részletes utasításokat tartalmaz az egyes biztonsági.

Ha jobban szeret tooperform saját tesztek vagy toouse kívánja, hogy egy másik képolvasó suite vagy szolgáltató, hajtsa végre az hello [jóváhagyási folyamat tesztelése az Azure behatolás](https://security-forms.azure.com/penetration-testing/terms) és előzetes tooperform szükséges hello behatolás beszerzése teszteli.

## <a name="https"></a>Az ügyfelek biztonságos kommunikáció
Hello használatakor  **\*. azurewebsites.net** App Service-alkalmazás létre tartománynevet is azonnal a HTTPS PROTOKOLLT használja, az SSL-tanúsítvány biztosított összes  **\*. azurewebsites.net** tartománynevek. Ha a hely által használt egy [egyéni tartománynév](app-service-web-tutorial-custom-domain.md), túl is SSL-tanúsítvány feltöltése[HTTPS engedélyezése](app-service-web-tutorial-custom-ssl.md) hello mutató egyéni tartományhoz.

Engedélyezés [HTTPS](https://en.wikipedia.org/wiki/HTTPS) segíthet az alkalmazás és a felhasználók hello kommunikációját támadások Kiküszöbölésében.

## <a name="secure-data-tier"></a>Biztonságos adatszint
App Service erősen integrálódik az SQL-adatbázis esetén az, hogy minden hello kapcsolati karakterláncok hello tábla között vannak titkosítva, és csak lesznek visszafejtve a virtuális gép hello hello alkalmazást *és* csak amikor hello alkalmazást futtat. Számos biztonsági szolgáltatások toohelp a számítógépes érintő fenyegetésekkel szemben, beleértve az alkalmazásadatok védelme az Azure SQL Database tartalmazza továbbá [nyugalmi titkosítási](https://msdn.microsoft.com/library/dn948096.aspx), [mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx), [ Dinamikus Adatmaszkolási](../sql-database/sql-database-dynamic-data-masking-get-started.md), és [veszélyforrások Detektálása](../sql-database/sql-database-threat-detection.md). Ha a bizalmas adatokat, vagy megfelelőségi követelménynek, olvassa el [SQL-adatbázisok védelme](../sql-database/sql-database-security-overview.md) további információt a toosecure adatait.

Ha egy külső adatbázis-szolgáltató, például a ClearDB, nézze át közvetlenül a bevált biztonsági gyakorlatok hello szolgáltató dokumentációját.  

## <a name="develop"></a>Biztonságos fejlesztési és üzembe helyezés
### <a name="publishing-profiles-and-publish-settings"></a>Profilok közzététele és a közzétételi beállítások
Amikor fejleszt alkalmazásokat, kezelési feladatainak végrehajtása, vagy segédprogramok használatával például a feladatok automatizálásának **Visual Studio**, **Web Matrix**, **Azure PowerShell** vagy hello **Azure parancssori felület (CLI)**, használhatja a *közzétételi beállítások* fájl vagy egy *közzétételi profil*. Mindkét fájltípusok hitelesíteni tudja Önt Azure-ral, és nem engedélyezett tooprevent hozzáférés titkosítani kell.

* A **közzétételi beállítások** fájl tartalmaz
  
  * Azure-előfizetése Azonosítóját
  * Egy felügyeleti tanúsítványt, amely lehetővé teszi az előfizetéshez tartozó tooperform felügyeleti feladatok *anélkül, hogy tooprovide felhasználónév és jelszó*.
* A **közzétételi profil** fájl tartalmaz
  
  * Információ tooyour alkalmazás-közzététel

Egy segédprogram, amely a közzétételi beállítások fájlja, vagy a közzétételi profil fájlja használatakor hello közzétételi beállításait tartalmazó hello fájl importálása vagy hello segédprogram a profilt, majd **törlése** hello fájlt. Ha a fájl hello kell, tooshare másokkal, például hello projekten dolgozik tárolja biztonságos helyen, mint egy *titkosított* címtárát a korlátozott engedélyekkel.

Emellett meg kell győződnie arról importált hello hitelesítő adatok biztonságáról. Például **Azure PowerShell** és hello **Azure parancssori felület (CLI)** mind az importált adatok tárolásához a **kezdőkönyvtár** ( *~*  a Linux vagy OS X rendszerben és */felhasználó/felhasználónév* Windows rendszeren.) A további biztonsági Kezdésként túl**titkosítása** ezeket a helyeket titkosítás által biztosított eszközökkel, az operációs rendszernek.

### <a name="configuration-settings-and-connection-strings"></a>A konfigurációs beállításokat és kapcsolati karakterláncok
Általános gyakorlat toostore kapcsolati karakterláncok, hitelesítő adatok és más bizalmas adatokat a konfigurációs fájlokban. Sajnos ezeket a fájlokat a webhelyen elérhetővé tett, vagy be van jelölve, egy nyilvános tárházat, ez az információ az ilyen be. Az egyszerű keresés [GitHub](https://github.com), például felfedhetők hello nyilvános adattárak kitett titkos kulcsainak számos konfigurációs fájlok.

ajánlott eljárás hello tookeep ezeket az információkat az alkalmazás konfigurációs fájlok kívül van. App Service lehetővé teszi a konfigurációs adatok tárolása, hello futtatókörnyezetben részeként **Alkalmazásbeállítások** és **kapcsolati karakterláncok**. hello értékei kitett tooyour alkalmazás futásidőben keresztül *környezeti változók* a legtöbb programozási nyelv. .NET-alkalmazások ezek az értékek vannak be a nézetmodellbe, a .NET-konfiguráció futásidőben. Ezekben a helyzetekben, leszámítva a konfigurációs beállítások is titkosítva maradnak megtekintéséhez, vagy állítsa be ezeket, kivéve hello segítségével [Azure Portal](https://portal.azure.com) vagy segédprogramok például a PowerShell vagy Azure CLI hello. 

Konfigurációs adatok tárolása az App Service-ben lehetővé teszi hello app rendszergazda toolock le hello termelési alkalmazások bizalmas adatokat. A fejlesztők olyan konfigurációs beállításokat külön készletét használható alkalmazások fejlesztéséhez és is hello beállítások automatikusan felváltotta az App Service-ben megadott hello beállításokat. Még akkor is, nem hello fejlesztők tooknow hello titkok hello éles alkalmazások konfigurálva kell. Az App Service Alkalmazásbeállítások és kapcsolati karakterláncok konfigurálásáról további információkért lásd: [webalkalmazások konfigurálása](web-sites-configure.md).

### <a name="ftps"></a>FTPS
Az Azure App Service biztonságos FTP hozzáférés toohello fájlrendszert biztosít az alkalmazás keresztül **ftps-t**. Ez lehetővé teszi toosecurely hozzáférés hello alkalmazáskód hello webalkalmazáshoz, valamint a diagnosztikai naplókat. Javasoljuk, hogy mindig használjon FTPS FTP helyett. 

az alkalmazás hivatkozását hello ftps-t az alábbi lépésekkel hello található:

1. Nyissa meg hello [Azure Portal](https://portal.azure.com).
2. Válassza ki **összes tallózása**.
3. A hello **Tallózás** panelen válassza **alkalmazásszolgáltatások**.
4. A hello **alkalmazásszolgáltatások** panelen, jelölje be hello kívánt alkalmazást.
5. Hello alkalmazás paneljén válassza **összes beállítás**.
6. A hello **beállítások** panelen válassza **tulajdonságok**.
7. hello FTP és FTPS hivatkozásokkal érhető el a hello **beállítások** panelen. 

Ftps-t a további információkért lásd: [File Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol).

## <a name="next-steps"></a>Következő lépések
Kapcsolatos további adatokat hello biztonsági hello Azure platformon, a jelentési adatokat egy **biztonsági incidens- vagy visszaélés**, vagy a Microsoft, amely kíván tooinform **behatolást vagy a biztonság tesztelése** a a helyrendszer, című rész hello biztonsági hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

További információ a **web.config** vagy **applicationhost.config** fájlok az App Service apps, lásd: [konfigurációs beállításokat az Azure App Service web apps nyitása](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/).

Naplózási információk az App Service-alkalmazások hasznosak lehetnek a támadások, információt [diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű alkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

