---
title: "aaaAzure adatbázis ajánlott biztonsági eljárások |} Microsoft Docs"
description: "Ez a cikk ajánlott eljárások az Azure-adatbázis biztonsági biztosít."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: tomsh
ms.openlocfilehash: 78984291e8f74879c7f738e2ce3176c4be18d154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-best-practices"></a>Azure-adatbázishoz ajánlott biztonsági eljárások

Biztonsági elsődleges szempont kezeléséhez, adatbázisok, és az Azure SQL Database prioritást mindig lett. Az adatbázisok lehet szorosan biztonságos toohelp elégíti ki a szabályozási vagy biztonsági követelmények, például a HIPAA, ISO 27001/27002 és PCI DSS szintjét 1, többek között. A biztonsági megfelelőség tanúsítványok aktuális listáját érhető el: hello [Microsoft Trust Center webhely](http://azure.microsoft.com/support/trust-center/services/). Az adatbázisok az adott Azure adatközpontjaiban szabályozási követelmények alapján tooplace is használhat.

Ez a cikk ismertetik Azure-adatbázishoz ajánlott biztonsági eljárások gyűjteménye. Az alábbi gyakorlati tanácsok származó tapasztalatunk az Azure-adatbázis biztonsági és hello véleményeket ügyfelek, például saját maga.

A minden ajánlott eljárás az hogy ismertetik:

-   Milyen hello ajánlott eljárás
-   Miért érdemes tooenable, hogy a legjobb
-   Mi lehet hello eredmény, ha Ön nem tooenable hello ajánlott
-   Hogyan szerezhet tooenable hello ajánlott

Azure adatbázis ajánlott biztonsági eljárások a cikkben egy együttműködési véleményt és az Azure platform képességei alapján, és a szolgáltatás állítja be, ez a cikk írásának hello időpontban léteznek. Vélemények és technológiák változnak az idők, és ez a cikk lesznek frissítve egy rendszeresen tooreflect ezeket a módosításokat.

Azure-adatbázishoz ajánlott biztonsági eljárások cikkben említett a következők:

-   Tűzfal szabályok toorestrict adatbázis hozzáféréssel
-   Adatbázis-hitelesítés engedélyezése
-   Adatok védelme a titkosítás
-   Adatok védelmére átvitel
-   Adatbázis naplózásának engedélyezése
-   Adatbázis fenyegetésészlelés engedélyezéséhez


## <a name="use-firewall-rules-toorestrict-database-access"></a>Tűzfal szabályok toorestrict adatbázis hozzáféréssel

A Microsoft Azure SQL Database egy relációs adatbázis-szolgáltatást nyújt az Azure és egyéb internetalapú alkalmazások számára. tooprovide hozzáférés-biztonságot, SQL-adatbázis szabályozza a hozzáférést igénylő felhasználók tooprove hitelesítési mechanizmusok korlátozása kapcsolat IP-címe, tűzfal-szabályokat a személyazonosságukat, és korlátozza a felhasználók toospecific műveletek és az adatok engedélyezési mechanizmusokat. Tűzfalak tooyour adatbázis-kiszolgáló minden hozzáférés tiltása, amíg megadhatja, hogy mely számítógépek rendelkeznek engedéllyel. hello tűzfal engedélyezi a hozzáférést toodatabases származó IP-cím az egyes kérelmek hello alapján.

![Tűzfalszabályok](./media/azure-database-security-best-practices/azure-database-security-best-practices-Fig1.png)

hello Azure SQL Database szolgáltatásban csak az 1433-as TCP-porton keresztül érhető el. a számítógép egy SQL-adatbázis tooaccess győződjön meg arról, hogy az ügyfél számítógép tűzfal engedélyezi-e az 1433-as TCP-port kimenő TCP-kommunikáció. Ha nincs szükség más alkalmazások, bejövő kapcsolat tiltása a 1433-as TCP-port tűzfalszabályok használatával.

Hello csatlakozási folyamat részeként az Azure virtuális gépek közötti kapcsolatok átirányított tooa másik IP-címet és portot, egyedi minden feldolgozói szerepkör esetében. hello portszám van 11000 too11999 hello közé. További információ a TCP-portok: [kívüli ADO.NET 4.5 és SQL Database2 1433-as portokon](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12).

> [!Note]
> További információ az SQL Database tűzfalszabályaival kapcsolatban: [SQL Database tűzfalszabályok](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="enable-database-authentication"></a>Adatbázis-hitelesítés engedélyezése
SQL adatbázis két típusú hitelesítés, SQL-hitelesítést és az Azure Active Directory-hitelesítés (az Azure AD-alapú hitelesítés) támogatja.

**SQL-hitelesítés** a következő esetekben ajánlott:

-   Lehetővé teszi SQL Azure toosupport környezetekben, ahol vegyes operációs rendszerek, ahol minden felhasználó nem hitelesíti a Windows-tartománynak.
-   Lehetővé teszi, hogy az SQL Azure toosupport régebbi alkalmazásokat és SQL Server-hitelesítést igénylő harmadik felek által nyújtott.
-   Lehetővé teszi, hogy a felhasználók tooconnect ismeretlen vagy nem megbízható tartományokból. Például egy adott csatlakozás meghatározott felhasználók alkalmazás SQL Server bejelentkezési tooreceive hello állapot hozzárendelve a rendelések.
-   Lehetővé teszi, hogy az SQL Azure toosupport webes alkalmazás mely felhasználók létre saját identitással.
-   Lehetővé teszi, hogy a szoftver az alkalmazások egy összetett engedély hierarchia használatával alapján fejlesztők toodistribute ismert, az adott néven beállítás SQL Server bejelentkezési azonosítók.

> [!Note]
> Azonban az SQL Server-hitelesítés Kerberos biztonsági protokoll nem használható.

Ha **SQL-hitelesítés** kell:

-   Hello erős hitelesítő adatok kezelése magát.
-   Hello kapcsolati karakterláncban a hello hitelesítő adatok védelme.
-   Hello Web server toohello adatbázisból hello hálózaton keresztül átadott hello hitelesítő (vélhetően) védelmét. További információ: [hogyan: csatlakozás tooSQL Server használatával SQL-hitelesítés az ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx).

**Az Azure Active Directory authentication** egy mechanizmus, amely tooMicrosoft Azure SQL Database és [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) identitások az Azure Active Directory (Azure AD) segítségével. Az Azure AD-alapú hitelesítés az adatbázis-felhasználók hello identitások és más Microsoft-szolgáltatásokban egyetlen központi helyen központilag kezelheti. Központi azonosítófelügyeleti itt egyetlen toomanage adatbázis-felhasználók és egyszerűbbé teszi a jogosultság kezelése. Előnyöket hello alábbiakat foglalja magába:

-   Egy alternatív tooSQL kiszolgálóhitelesítés biztosít.
-   Segít hello elterjedése tartozó felhasználói azonosítók leállítása adatbázis-kiszolgáló között.
-   Lehetővé teszi, hogy a jelszó Elforgatás egyetlen helyen.
-   Az ügyfelek adatbázis-engedélyek külső (AAD) csoportok használatával kezelheti.
-   Integrált Windows-hitelesítés és egyéb Azure Active Directory által támogatott hitelesítési engedélyezésével azt megnövelésével kiküszöbölheti jelszavak tárolását.
-   Az Azure AD authentication használja a tartalmazott adatbázis felhasználók tooauthenticate identitások hello adatbázis szintjén.
-   Az Azure AD Kapcsolódás adatbázis tooSQL alkalmazások jogkivonat-alapú hitelesítést támogatja.
-   Az Azure AD authentication az ADFS (tartomány-összevonás) vagy a natív felhasználói/jelszó-hitelesítés támogat egy helyi Azure Active Directory tartományi szinkronizálás nélkül.
-   Az Azure AD-példányokhoz SQL Server Management Studio eszközt, amely az Active Directory univerzális hitelesítés használatára, amely magában foglalja a többtényezős hitelesítés (MFA). Többtényezős hitelesítés tartalmazza, könnyen ellenőrzési lehetőségek széles erős hitelesítés – telefonhívás, szöveges üzenetet, intelligens kártyák PIN-kód és az értesítést a mobilalkalmazásban. További információkért lásd: [SSMS támogatása az Azure AD MFA az SQL-adatbázis és az SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication).

hello konfigurációs lépések közé tartozik a következő eljárások tooconfigure hello és Azure Active Directory-hitelesítés használatára.

-   Létrehozása és feltöltése az Azure AD.
-   Választható lehetőség: Hozzárendelése vagy hello active directory jelenleg az Azure-előfizetéshez társított módosítása.
-   Hozzon létre egy Azure Active Directory-rendszergazda az Azure SQL server vagy [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
- Állítsa be az ügyfélszámítógépen.
-   Felhasználók létrehozása a tartalmazott adatbázis az adatbázis leképezve tooAzure AD identitásokat.
-   Csatlakozás tooyour adatbázis az Azure AD-identitások segítségével.

Részletek információt [Itt](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

## <a name="protect-your-data-using-encryption"></a>Adatok védelme a titkosítás

[Az Azure SQL Database átlátható adattitkosítás (TDE)](https://msdn.microsoft.com/library/dn948096.aspx) segít valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok, kártevő szándékú tevékenységek hello fenyegetés elleni védelmében. és a tranzakciós naplófájlok nélkül aktívan igénylő módosításokat toohello alkalmazás. TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja.

Még ha hello teljes tárolási titkosítva van, akkor nagyon fontos tooalso maga az adatbázis titkosítása. Ez az üdvözlő védelmi mélysége megközelítéssel a data protection megvalósítását. Ha használja az Azure SQL Database, és tooprotect érzékeny adatok, például a hitelkártya vagy társadalombiztosítási számot kívánja, a FIPS 140-2 érvényesítve 256 bites AES titkosítást és meg kell felelnie a hello követelményeinek sok ipari szabványok (például a HIPAA, adatbázisok titkosíthatja PCI).

Fontos, hogy a fájlok toounderstand kapcsolódó túl[(BPE) kiterjesztés puffer](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) amikor adatbázis titkosítása a TDE nincs titkosítva. A fájlszintű titkosítás Rendszereszközök fájl például kell használnia [BitLocker](https://technet.microsoft.com/library/cc732774) vagy hello [titkosított fájlrendszer (EFS)](https://technet.microsoft.com/library/cc700811.aspx) BPE a kapcsolódó fájlokat.

Hitelesített felhasználó óta, mint például a biztonsági rendszergazda vagy egy adatbázis-rendszergazda hozzáférhessen hello adatokat, akkor is, ha a TDE, hello adatbázis van titkosítva is kövesse az alábbi hello ajánlásokat:

-   Engedélyezi az SQL-hitelesítést hello adatbázis szintjén.
-   Használja az Azure AD használatával [RBAC-szerepkörök](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is).
-   Felhasználók és az alkalmazások tooauthenticate külön fiókot kell használnia. Ezzel a módszerrel toousers és az alkalmazások adott hello engedélyek korlátozása, és a kártékony tevékenység hello kockázatok csökkentése.
-   Alkalmazzon adatbázis szintű biztonsági rögzített adatbázis-szerepkörök (például a db_datareader vagy db_datawriter), vagy az alkalmazás toogrant az egyéni szerepköröket hozhatnak létre a kifejezett engedélyek tooselected adatbázis-objektumokat.

A más módon tooencrypt adatait, vegye figyelembe:

-   [Cellaszintű titkosítását alkalmazza](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt adott oszlopok vagy még akkor is, cellák az adatok különböző titkosítási kulcsokat.
-   Használ, mindig titkosítja a titkosítási: [mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx) lehetővé teszi, hogy az ügyfelek tooencrypt bizalmas adatok ügyfélalkalmazások belül, és soha nem fedi fel hello titkosítási kulcsok toohello adatbázis-kezelő (SQL-adatbázis vagy SQL Server). Ennek eredményeképpen Always Encrypted funkciója saját hello adatok (és megtekinthesse) elkülönítése és azokra, akik hello adatok kezelése (de nem hozzáféréssel kell rendelkeznie).
-   Használata a sorszintű biztonság: sorszintű biztonság lehetővé teszi, hogy az ügyfelek toocontrol hozzáférés toorows egy adatbázis tábláinak hello jellemzők hello felhasználó (pl. csoport tagsági vagy a végrehajtási környezet) lekérdezése alapján. További információkat a [sorszintű biztonsággal kapcsolatos](https://msdn.microsoft.com/library/dn765131) részben találhat.

## <a name="protect-data-in-transit"></a>Adatok védelmére átvitel
Az átvitel során az adatok védelme a data protection stratégia nagyon fontos részét kell lennie. Oda-vissza adatokat fog áthelyezése több helyről, mivel a hello általános javaslat, hogy mindig használjon SSL/TLS protokollok tooexchange adatok különböző helyek között. Bizonyos esetekben érdemes lehet tooisolate hello teljes kommunikációs csatornát a helyszíni és a felhő közötti virtuális magánhálózati (VPN) segítségével infrastruktúra.

Az adatok áthelyezése a helyszíni infrastruktúra és az Azure között megfelelő védelmi funkciók, például a HTTPS- vagy VPN-érdemes lehet.

Használja a szervezet számára több munkaállomások helyszíni tooAzure toosecure hozzáférést igénylő [Azure telephelyek közötti VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Olyan szervezeteknek, amelyek toosecure kell a hozzáférést a található egyéni munkaállomások helyszíni vagy irodán kívüli tooAzure, érdemes lehet [pont-pont VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Nagyobb, mint egy dedikált nagy sebességű WAN-kapcsolaton keresztül anélkül áthelyezhetők [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Ha toouse ExpressRoute, hello adatok hello alkalmazásszintű használatával is titkosíthatók [SSL/TLS](https://support.microsoft.com/kb/257591) vagy egyéb protokollok hozzáadott védelemre.

Interakció Azure Storage hello Azure portálon keresztül, ha minden tranzakciója történik, HTTPS használatával. [Storage REST API felülete](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS-KAPCSOLATON keresztül is lehet a használt toointeract [Azure Storage](https://azure.microsoft.com/services/storage/) és [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

A szervezeteknek, amelyek sikertelen tooprotect adatokat átvitel közben is jobban ki vannak téve a [-átjárójának](https://technet.microsoft.com/library/gg195821.aspx), [lehallgatás](https://technet.microsoft.com/library/gg195641.aspx) és munkamenet-eltérítés. Ilyen támadások hello első lépéseként való hozzáférés tooconfidential adatok is lehetnek.

További információk az Azure VPN beállítást hello cikk elolvasása toolearn [tervezése és kialakítása VPN-átjáró](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-plan-design).

## <a name="enable-database-auditing"></a>Adatbázis naplózásának engedélyezése
SQL Server adatbázismotor hello vagy egyedi adatbázis naplózásának magában foglalja a nyomon követése és hello adatbázismotor események naplózása. SQL Server-naplózás lehetővé teszi a kiszolgáló felülvizsgálatát, amely a kiszolgálói szintű események naplózási specifikációk server és adatbázis szolgáltatásiszint-események naplózási specifikációk adatbázis létrehozása. Naplózott események toohello eseménynaplók vagy tooaudit fájlok írhatók.

Nincsenek attól függően, hogy kormányzati vagy a telepítési szabványok követelményeinek SQL Server-naplózás több szintet. Az SQL Server Audit biztosít hello eszközeivel és eljárásaival különböző server és adatbázis-objektumok tooenable, a tároló és a nézet naplózással kell rendelkeznie.

[Az Azure SQL Database Auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) követi az adatbázisban, majd beírja őket az Azure Storage-fiókban tooan audit napló.

A naplózás segíthet a jogszabályi megfelelőség fenntartásában és az adatbázison végzett tevékenység megértésében, valamint az esetleg üzleti veszélyeket vagy biztonsági problémákat jelző rendellenességek feltárásában.

Naplózás lehetővé teszi, hogy és betartása toocompliance szabványok elősegíti, de nem biztosítja a megfelelőség.

adatbázis naplózásával kapcsolatos további toolearn és hogyan tooenable, olvassa el a cikk hello [engedélyezése az Azure Security Centerben SQL-kiszolgálón a naplózás és a fenyegetések észlelésére](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="enable-database-threat-detection"></a>Adatbázis fenyegetésészlelés engedélyezéséhez
SQL Fenyegetésészlelés toodetect lehetővé teszi, és válaszoljon toopotential fenyegetéseket, adja meg a biztonsági riasztások a rendellenes tevékenységek előforduló. Adatbázis gyanús tevékenységeket, a potenciális biztonsági réseket, és a SQL injektálási támadások, valamint rendellenes adatbázis hozzáférési minták riasztást fog kapni. SQL Fenyegetésészlelés riasztásokat gyanús tevékenység részleteinek megadása, és hogyan művelet javasolja tooinvestigate és hello fenyegetést.

Például SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások. A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások alkalmazás mezőkbe megsértése vagy hello adatbázis adatainak módosítása.

arról, hogyan toolearn tooset fel az adatbázis hello Azure portál lásd: a fenyegetések észlelése [SQL adatbázis Fenyegetésészlelés](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).

Ezenkívül integrálható az SQL Fenyegetésészlelés a riasztások [az Azure Security Center](https://azure.microsoft.com/services/security-center/). Szeretnénk felhívni a tootry azt a 60 napig ingyenes.

További információk a Fenyegetésészlelés adatbázis toolearn és hogyan tooenable, olvassa el a cikk hello [engedélyezése az Azure Security Centerben SQL-kiszolgálón a naplózás és a fenyegetések észlelésére](https://docs.microsoft.com/azure/security-center/security-center-enable-auditing-on-sql-servers).

## <a name="conclusion"></a>Összegzés
Azure-adatbázis egy robusztus adatbázis platform, amely sok szervezeti és szabályozási megfelelőségi követelményeknek funkciókat teljes tartománnyal. Hello fizikai hozzáférés tooyour adatok vezérlése, és számos lehetőséget használ az adatok biztonsági hello fájl-, oszlop-vagy sor szintjén átlátható adattitkosítási, cellaszintű titkosítását alkalmazza vagy sorszintű biztonság is adatok védelme. Mindig titkosított is lehetővé teszi a titkosított adatok műveleteket az alkalmazás frissítései hello folyamat egyszerűsítése. Viszont belépési tooauditing naplók SQL adatbázis-tevékenység biztosít hello szükséges információkat, hogy hogyan és mikor adatokhoz tooknow lehetővé teszi.

## <a name="next-steps"></a>Következő lépések
- toolearn tűzfalszabályok, bővebben lásd: [tűzfal-szabályok](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).
- bejelentkezések, és a felhasználók toolearn lásd [bejelentkezések kezelése](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).
- Az oktatóanyagok esetén lásd: [az Azure SQL Database biztonságos](https://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial).
