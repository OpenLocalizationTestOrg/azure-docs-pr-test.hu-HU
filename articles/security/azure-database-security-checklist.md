---
title: "aaaAzure adatbázis biztonsági ellenőrzőlista |} Microsoft Docs"
description: "Ez a cikk ellenőrzőlista biztosít az Azure-adatbázis biztonsági."
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
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: 5e3a69591df3c8508a8707a2d47068342863ce93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-security-checklist"></a>Azure-adatbázis biztonsági ellenőrzőlista

biztonsági okokból azt ajánljuk toohelp, Azure-adatbázis számos olyan beépített biztonsági ellenőrzéseket, amelyeket toolimit használja, és hozzáférés szabályozása.

Ezek a következők:

-   A tűzfal, amely lehetővé teszi toocreate [tűzfal-szabályok](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) kapcsolat korlátozza az IP-címe
-   Kiszolgálószintű tűzfal érhető el a hello Azure-portálon
-   Adatbázis-szintű tűzfalszabályok érhető el a szolgáltatáshoz az SSMS
-   Biztonságos kapcsolati karakterláncok használatával biztonságos kapcsolatot tooyour adatbázis
-   Hozzáférés-kezelés
-   Adattitkosítás
-   SQL Database auditing
-   SQL-adatbázis fenyegetések észlelése

## <a name="introduction"></a>Bevezetés
A felhőalapú informatika új biztonsági mintáknak, amelyek ismeretlen toomany az alkalmazás felhasználóinak, adatbázis-rendszergazdák és programozók igényel. Ennek eredményeképpen egyes szervezetek olyan határozatlanok tooimplement adatkezelés tooperceived biztonsági kockázatok miatt a felhőalapú infrastruktúra. Azonban nagy részét ezen probléma enyhíthetők keresztül hello biztonsági funkciói a Microsoft Azure és a Microsoft Azure SQL Database beépített jobb megértése.

## <a name="checklist"></a>Feladatlista
Azt javasoljuk, hogy olvassa el a hello [Azure adatbázis ajánlott biztonsági eljárások](https://docs.microsoft.com/en-us/azure/security/azure-database-security-best-practices) cikk előzetes tooreviewing ezt az ellenőrző listát. Ezt az ellenőrző listát hatékonyabb működtetését képes tooget hello hello gyakorlati tanácsok megismerését követően lesz. Használhatja a feladatlista toomake meg arról, hogy az Azure-adatbázis biztonsági már foglalkozott hello a fontos problémákat.


|Feladatlista kategória| Leírás|
| ------------ | -------- |
|**Adatok védelme**||
| <br> Mozgásérzékelő – / átvitel titkosítás| <ul><li>[Transport Layer Security](https://docs.microsoft.com/en-us/windows-server/security/tls/transport-layer-security-protocol), az adatok titkosítását, ha adatok átvitele pedig toohello hálózatok.</li><li>Adatbázis szükséges a biztonságos kommunikációt olyan ügyfelektől, hello alapján [(tabulált adatfolyamban) TDS](https://msdn.microsoft.com/en-in/library/dd357628.aspx) keresztüli TLS (Transport Layer Security).</li></ul> |
|<br>Titkosítás inaktív állapotban| <ul><li>[Az átlátható adattitkosítási](http://go.microsoft.com/fwlink/?LinkId=526242), ha inaktív adatok fizikailag bármely digitális formában van tárolva.</li></ul>|
|**Elérés**||  
|<br> Adatbázis-hozzáférés | <ul><li>[Hitelesítési](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) (Azure Active Directory-hitelesítés) AD hitelesítési használja az Azure Active Directory által kezelt identitások.</li><li>[Engedélyezési](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) engedélyezése a felhasználók hello szükséges legalacsonyabb jogosultságok.</li></ul> |
|<br>Alkalmazás-hozzáférés| <ul><li>[Biztonsági szint sor](https://msdn.microsoft.com/library/dn765131) (használatával biztonsági szabályzatot, hello ugyanannyi időt vesz igénybe, sorszintű hozzáférés korlátozása a felhasználói identitás, a szerepkör vagy a végrehajtási környezet alapján).</li><li>[A dinamikus Adatmaszkolás](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) (használatával engedély & házirend, korlátozza a bizalmas adatok veszélyeztetettségének maszkolás azt toonon jogosultságú felhasználók által)</li></ul>|
|**Az előrejelzéses figyeléssel**||  
| <br>Követés & észlelése| <ul><li>[Naplózás](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing) követi az adatbázisban, majd beírja őket tooan napló / tevékenység jelentkezzen be a [Azure Storage-fiók](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account).</li><li>Track Azure adatbázis állapotfigyelő segítségével [Azure figyelő tevékenységi naplóit](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Veszélyforrások Detektálása](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection) adatbázist érintő rendellenes tevékenységeket utaló esetleges biztonsági fenyegetéseket toohello adatbázis észleli. </li></ul> |
|<br>Azure Security Center| <ul><li>[Adatok figyelés](https://docs.microsoft.com/en-us/azure/security-center/security-center-enable-auditing-on-sql-databases) használja az Azure Security Center biztonsági központi felügyeleti megoldás az SQL és más Azure-szolgáltatásokkal.</li></ul>|     

## <a name="conclusion"></a>Összegzés
Azure-adatbázis egy robusztus adatbázis platform, amely sok szervezeti és szabályozási megfelelőségi követelményeknek funkciókat teljes tartománnyal. Védheti meg egyszerűen adatok hello fizikai hozzáférés tooyour adatok vezérlése, és számos lehetőséget használ az adatok biztonsági hello fájl-, oszlop-vagy sorszintű átlátható adattitkosítási, cellaszintű titkosítását alkalmazza vagy sorszintű biztonság. Mindig titkosított is lehetővé teszi a titkosított adatok műveleteket az alkalmazás frissítései hello folyamat egyszerűsítése. Viszont belépési tooauditing naplók SQL adatbázis-tevékenység biztosít hello szükséges információkat, hogy hogyan és mikor adatokhoz tooknow lehetővé teszi.

## <a name="next-steps"></a>Következő lépések
Az adatbázis rosszindulatú felhasználók vagy néhány egyszerű lépésben jogosulatlan hozzáférés elleni védelmének hello növelhető. Ebben az oktatóanyagban elsajátíthatja, hogy:

- Állítson be [tűzfal-szabályok](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) adatbázis és a kiszolgálón.
- Az adatok védelméhez [titkosítási](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/sql-server-encryption).
- Engedélyezése [SQL Database auditing](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing).

