---
title: "aaaAzure SQL Database biztonsági áttekintése |} Microsoft Docs"
description: "További tudnivalók az Azure SQL Database és SQL Server biztonsági, többek között a hello felhőalapú és helyszíni SQL Server hello különbségei tooauthentication, az engedélyezés, a kapcsolat biztonságát, a titkosítási és a megfelelőségi ismét."
services: sql-database
documentationcenter: 
author: tmullaney
manager: jhubbard
editor: 
ms.assetid: a012bb85-7fb4-4fde-a2fc-cf426c0a56bb
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/05/2017
ms.author: thmullan;jackr
ms.openlocfilehash: 7b0b0d1b59ec4018d9fb668bc8b6b56c9907e982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-your-sql-database"></a>Az SQL Database-adatbázis védelme

Ez a cikk végigvezeti a védelmét biztosító adatszint hello Azure SQL Database használata az alkalmazások hello alapjait. A cikk ismerteti az adatok védelméhez, a hozzáférés szabályozásához és a proaktív figyeléshez szükséges erőforrások az első lépéseit is. 

A biztonsági szolgáltatások elérhető összes verziója az SQL teljes áttekintése, lásd: hello [Security Center az SQL Server adatbázismotor és Azure SQL Database](https://msdn.microsoft.com/library/bb510589). További információk is rendelkezésre áll, a hello [biztonsági és az Azure SQL Database műszaki dokumentáció](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>Adatok védelme
Az SQL Database a mozgásban lévő adatokat a [Transport Layer Security](https://support.microsoft.com/kb/3135244) protokol, az inaktív adatokat [transzparens adattitkosítás](http://go.microsoft.com/fwlink/?LinkId=526242), a használatban lévő adatokat pedig az [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) protokoll használatával titkosítja az adatok védelme érdekében. 

> [!IMPORTANT]
>Minden kapcsolatok tooAzure SQL-adatbázis titkosításának megkövetelése (SSL/TLS) minden alkalommal "az átvitel során" tooand hello adatbázisból közben. Az alkalmazás a kapcsolati karakterláncot, meg kell adnia paraméterek tooencrypt hello kapcsolat és *nem* tootrust hello kiszolgálói tanúsítványt (ebben az esetben az, ha a kapcsolati karakterlánc kívüli másolja hello klasszikus Azure portálon) Ellenkező esetben hello kapcsolat hello kiszolgáló identitásának hello nem ellenőrzi, és ki vannak téve túl "man-az-a-middle" támadások. Hello ADO.NET illesztőprogram, például a kapcsolati karakterlánc paraméterei **Encrypt = True** és **TrustServerCertificate = False**. 

A más módon tooencrypt adatait, vegye figyelembe:

* [Cellaszintű titkosítását alkalmazza](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt adott oszlopok vagy még akkor is, cellák az adatok különböző titkosítási kulcsokat.
* Ha hardveres biztonsági modulra vagy a titkosítási kulcshierarchia központi kezelésére van szüksége, fontolja meg az [Azure Key Vault és az SQL Server együttes használatát egy Azure virtuális gépen](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).

## <a name="control-access"></a>Vezérlési hozzáférés
SQL-adatbázis adatait korlátozza a hozzáférést tooyour adatbázis tűzfalszabályokat igénylő felhasználók tooprove azok identitáskezelési és engedélyezési toodata szerepköralapú tagságát és engedélyeket, valamint a hitelesítési mechanizmusok használatával biztonságossá tételére. a sorszintű biztonsági és dinamikus adatmaszkolási. Az SQL-adatbázis hozzáférés-vezérlő szolgáltatások hello használata tárgyalását lásd: [hozzáférés szabályozása](sql-database-control-access.md).

> [!IMPORTANT]
> Az adatbázisok és logikai kiszolgálók az Azure-ban való kezelését a portál felhasználói fiókjának szerepkör-hozzárendelése szabályozza. A témával kapcsolatos további információk az [Szerepköralapú hozzáférés-vezérlés az Azure Portalon](../active-directory/role-based-access-control-what-is.md) című cikkben találhatók.
>

### <a name="firewall-and-firewall-rules"></a>Tűzfal és tűzfalszabályok
az adatok védelméhez toohelp, tűzfalak összes tooyour adatbáziskiszolgáló tiltása, amíg megadhatja, hogy mely számítógépek rendelkeznek engedéllyel használó [tűzfal-szabályok](sql-database-firewall-configure.md). hello tűzfal engedélyezi a hozzáférést toodatabases származó IP-cím az egyes kérelmek hello alapján.

### <a name="authentication"></a>Authentication
SQL-adatbázis hitelesítési toohow, akkor a személyazonosság toohello adatbázishoz kapcsolódáskor hivatkozik. Az SQL Database két hitelesítési típust támogat:

* A felhasználónévvel és jelszóval végzett **SQL-hitelesítést**. Hello logikai kiszolgáló létrehozása után az adatbázis, a "kiszolgáló-rendszergazda" Bejelentkezés a felhasználónevet és jelszót adott meg. Ezeket a hitelesítő adatokat használva hitelesítheti tooany adatbázis a kiszolgálón hello adatbázis tulajdonosa vagy a "dbo." 
* Az **Azure Active Directory-alapú hitelesítést**, amely az Azure Active Directory által felügyelt identitásokat használ, és a felügyelt és integrált tartományok is támogatják. [Amikor csak lehet](https://msdn.microsoft.com/library/ms144284.aspx), használja az Active Directory-hitelesítést (beépített biztonság). Ha azt szeretné, hogy toouse Azure Active Directory-hitelesítéssel, létre kell hoznia egy másik kiszolgáló rendszergazdája hello "Azure AD admin," tooadminister az Azure Active Directory-felhasználók és csoportok engedélyezett nevű. Ez a rendszergazda a normál kiszolgálói rendszergazdák által elvégezhető összes műveletet is végrehajthatja. Lásd: [Kapcsolódás adatbázis által használata Azure Active Directory-hitelesítéssel tooSQL](sql-database-aad-authentication.md) a forgatókönyv bemutatja, hogyan toocreate az Azure AD rendszergazdai tooenable Azure Active Directory-hitelesítéssel.

### <a name="authorization"></a>Engedélyezés
Engedély a felhasználó egy Azure SQL Database belül végrehajtható toowhat hivatkozik, és ezek ellenőrzik a felhasználói fiók adatbázis szerepkörtagságok és objektumszintű engedélyeket. Ajánlott eljárásként akkor adja meg az felhasználók hello szükséges legalacsonyabb jogosultságok. hello server rendszergazdai fiók csatlakozik a db_owner, melynek hatóság toodo hello adatbázison belül minden tagja. Tartsa meg ezt a fiókot a sémafrissítések üzembe helyezéséhez és egyéb felügyeleti műveletekhez. Az alkalmazás toohello adatbázisból hello a korlátozott engedélyekkel tooconnect hello "ApplicationUser" fiók használata az alkalmazás számára szükséges legalacsonyabb jogosultságok.

### <a name="row-level-security"></a>Sorszintű biztonság
A sorszintű biztonsági lehetővé teszi, hogy az ügyfelek toocontrol hozzáférés toorows egy adatbázis tábláinak hello jellemzők hello felhasználó (pl. csoport tagsági vagy a végrehajtási környezet) lekérdezése alapján. További információkat a [sorszintű biztonsággal kapcsolatos](https://msdn.microsoft.com/library/dn765131) részben találhat.

### <a name="data-masking"></a>Adatmaszkolás 
SQL-adatbázis dinamikus adatmaszkolási maszkolás azt toonon jogosultságú felhasználók által bizalmas adatok veszélyeztetettségének korlátozása. Dinamikus adatmaszkolási automatikusan felderíti az Azure SQL Database potenciálisan bizalmas adatokat, és a hello alkalmazásréteg gyakorolt minimális hatás mellett ezeket a mezőket végrehajthatóként javaslatok toomask nyújt. Működését tekintve a hello bizalmas adatok hello eredménykészletben lekérdezés obfuscating keresztül kijelölt mezőt, amíg hello adatbázis hello adatai nem változik. További információkért lásd: [Ismerkedés az SQL-adatbázis dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md) használt toolimit a bizalmas adatok felfedésnek lehet.

## <a name="proactive-monitoring"></a>Proaktív figyelés
Az SQL Database naplózási és fenyegetésészlelési képességek biztosításával védi az adatokat. 

### <a name="auditing"></a>Naplózás
SQL Database Auditing követi nyomon az adatbázis-tevékenységek, és segít toomaintain előírásoknak való megfelelés, adatbázis események tooan napló rögzíti az Azure Storage-fiókban. Naplózás toounderstand adatbázis folyamatban lévő tevékenységek, lehetővé teszi, valamint elemezheti és vizsgálja meg a szoftverközpontban végzett tevékenység tooidentify potenciális fenyegetések vagy gyanús visszaélés és a biztonság megsértése. További információk: [Ismerkedés az SQL Database naplózási szolgáltatásával](sql-database-auditing.md).  

### <a name="threat-detection"></a>Fenyegetések észlelése
A Fenyegetésészlelés kiegészíti a naplózás esetén által épített hello Azure SQL Database szolgáltatás által észlelt szokatlan és potenciálisan káros kísérletek tooaccess vagy biztonsági rés adatbázisok biztonsági eszközintelligencia további réteget biztosít. A gyanús tevékenységeket, a potenciális biztonsági réseket és a SQL injektálási támadások, valamint rendellenes adatbázis hozzáférési minták kapcsolatos fog kapni. Figyelmeztetések-ból is megtekinthetők [az Azure Security Center](https://azure.microsoft.com/services/security-center/) és a gyanús tevékenység részleteinek megadása, és hogyan művelet javasolja tooinvestigate és hello fenyegetést. A Fenyegetésészlelés $15/kiszolgáló/hó költségek. Az ingyenes hello az első 60 nap lesz. További információk: [Ismerkedés az SQL Database fenyegetések észlelése szolgáltatásával](sql-database-threat-detection.md).
 
### <a name="data-masking"></a>Adatmaszkolás 
SQL-adatbázis dinamikus adatmaszkolási maszkolás azt toonon jogosultságú felhasználók által bizalmas adatok veszélyeztetettségének korlátozása. Dinamikus adatmaszkolási automatikusan felderíti az Azure SQL Database potenciálisan bizalmas adatokat, és a hello alkalmazásréteg gyakorolt minimális hatás mellett ezeket a mezőket végrehajthatóként javaslatok toomask nyújt. Működését tekintve a hello bizalmas adatok hello eredménykészletben lekérdezés obfuscating keresztül kijelölt mezőt, amíg hello adatbázis hello adatai nem változik. További információkért lásd: Ismerkedés a [SQL-adatbázis dinamikus adatmaszkolási](sql-database-dynamic-data-masking-get-started.md)
 
## <a name="compliance"></a>Megfelelőség
Továbbá fent szolgáltatásait és funkcióit, amelyek segítségével az alkalmazás különböző biztonsági követelmények, az Azure SQL Database is teljesítéséhez toohello vesz részt a rendszeres ellenőrzéseket, és megfelelőségi követelményeket számos elleni hitelesített. További információkért lásd: hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), ahol hello legfrissebb listáját megtalálja [SQL-adatbázis megfelelőségi minősítései közül](https://azure.microsoft.com/support/trust-center/services/).

## <a name="next-steps"></a>Következő lépések

- Az SQL-adatbázis hozzáférés-vezérlő szolgáltatások hello használata tárgyalását lásd: [hozzáférés szabályozása](sql-database-control-access.md).
- Adatbázis naplózási tárgyalását lásd: [SQL Database auditing](sql-database-auditing.md).
- A fenyegetésészlelés tárgyalását lásd: [SQL-adatbázis fenyegetésészlelés](sql-database-threat-detection.md).
