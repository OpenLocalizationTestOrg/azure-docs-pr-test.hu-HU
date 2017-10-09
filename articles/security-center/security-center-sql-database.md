---
title: "Biztonsági központ és az Azure SQL Database szolgáltatás aaaAzure |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan Security Center segítségével hozzájárulhat az adatbázisokat az Azure SQL-adatbázis."
services: sql-database
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 173590500f0ce64140f214ada24b9692e01dbd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Az Azure Security Center és az Azure SQL Database szolgáltatás
[Az Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/) megakadályozása, észlelésében és kezelésében toothreats segít. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

Ez a cikk bemutatja, hogyan Security Center segítségével hozzájárulhat az adatbázisokat az Azure SQL-adatbázis.

## <a name="why-use-security-center"></a>Miért használja a Security Centert?
A Security Center segít az SQL-adatbázis adatok azáltal, hogy a kiszolgálók és adatbázisok hello biztonságának láthatósága. A Security Center a következőket teheti:

* SQL adatbázis-titkosítás és naplózási házirendek meghatározása.
* Az előfizetések közötti hello biztonsági SQL adatbázis-erőforrások figyelése
* Gyorsan problémák azonosítása és javítása biztonsági.
* Riasztások integrálni [Azure SQL Database fenyegetésészlelés](../sql-database/sql-database-threat-detection.md).

Ezenkívül toohelping védeni az SQL-adatbázis-erőforrások, a Security Center is biztosít a biztonsági figyelés és az Azure virtuális gépeken, a Cloud Services, a App Service szolgáltatások, a virtuális hálózatok és több kezeléséhez. További tudnivalók a Security Center [Itt](security-center-intro.md).

## <a name="prerequisites"></a>Előfeltételek
a Security Center használatába tooget, rendelkeznie kell egy előfizetési tooMicrosoft Azure. Ingyenes szint hello a Security Center engedélyezve van az előfizetéshez. A Security Center szabad és a szabványos rétegek további információkért lásd: [Security Center árképzési](https://azure.microsoft.com/pricing/details/security-center/).

Biztonsági központ szerepkörön alapuló hozzáférés támogatja. További információ a szerepköralapú hozzáférés-vezérlést (RBAC), az Azure-ban toolearn lásd [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md). hello Security Center: GYIK információt nyújt a [engedélyek kezelésének módja a biztonsági központban](security-center-faq.md#permissions).

## <a name="access-security-center"></a>A Security Center elérése
A Security Center elérje hello [Azure-portálon](https://azure.microsoft.com/features/azure-portal/). [Jelentkezzen be toohello portal](https://portal.azure.com/) és select hello **Security Center beállítás**.

![A Security Center lehetőséget][1]

Hello **Security Center** panel nyílik meg.
![A Security Center panel][2]

## <a name="set-security-policy"></a>Biztonsági házirend beállítása
A biztonsági házirend vezérlőelemek hello megadott előfizetés vagy az erőforrás-csoporton belüli erőforrások ajánljuk, hogy hello csoportját határozza meg. A biztonsági központban állíthatja be a szabályzatokat az előfizetések vagy erőforráscsoportok tooyour vállalat biztonsági igényeinek és hello szereplő alkalmazások típusának vagy az egyes előfizetések hello adatok érzékenységének megfelelően.

Egy házirend tooshow ajánlások az SQL-naplózás és az SQL transzparens adattitkosítás (TDE) állíthatja be.

* Ha bekapcsolja a **SQL-naplózás és a fenyegetések észlelésére**, a Security Center javasolja, hogy hozzáférési tooAzure adatbázis naplózását engedélyezni a megfelelőség szempontjából, felderítését és a nyomozás érdekében speciális.
* Ha bekapcsolja a **SQL transzparens adattitkosítás**, a Security Center javasolja, hogy a titkosítás aktívan engedélyezhető az Azure SQL Database, a társított biztonsági másolatok és a tranzakciós naplófájlok.

tooset egy olyan biztonsági házirendet, válassza hello **házirend** hello Security Center panel csempét. A hello **biztonsági házirend** panelen, jelölje be hello előfizetés kívánja tooenable hello biztonsági házirend. Válassza ki **megakadályozási szabályzat** tehetik **a** hello biztonsági javaslatok, amelyet az toouse ebben az előfizetésben.
![Biztonsági szabályzat][3]

több, lásd: toolearn [biztonsági házirendek beállítása](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Biztonsági ajánlás kezelése
A Security Center rendszeresen elemzi az Azure-erőforrások biztonsági állapotának hello. A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel. hello javaslatok végigvezetik hello hello szükséges vezérlők konfigurálásának lépésein.

Miután beállította a biztonsági házirendet, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonsági állapotát. hello javaslatok ahol mindegyik sor jelenti-e egy adott javaslat táblázatos formátumban jelennek meg. A következő táblázat, egy hivatkozás toohelp hello elérhető javaslatok az Azure SQL Database, és milyen minden javaslat amelyen alkalmazza, ha megismeri hello használja. Az ajánlás kiválasztása viszi tooan cikk azt ismerteti, hogyan tooimplement hello érték a Security Center.

| Ajánlás | Leírás |
| --- | --- |
| [SQL-kiszolgálón a naplózás és a fenyegetések észlelésére engedélyezése](security-center-enable-auditing-on-sql-servers.md) |Javasolja, hogy kapcsolja be az SQL adatbázis-kiszolgálók naplózás és a fenyegetések észlelésére. (Csak az SQL Database szolgáltatásban. Nem tartalmazza a Microsoft SQL Server, a virtuális gépeken futó.) |
| [A naplózás és a fenyegetések észlelésére az SQL-adatbázisok engedélyezése](security-center-enable-auditing-on-sql-databases.md) |Javasolja, hogy kapcsolja be az SQL Database-adatbázisok naplózásának és a fenyegetések észlelésére. (Csak az SQL Database szolgáltatásban. Nem tartalmazza a Microsoft SQL Server, a virtuális gépeken futó.) |
| [Transzparens adattitkosítás engedélyezése](security-center-enable-transparent-data-encryption.md) |Az SQL-adatbázisok titkosítási engedélyezését javasolja. (SQL Database szolgáltatás csak.) |

az Azure-erőforrások, jelölje be hello toosee javaslatok **javaslatok** hello Security Center panel csempét. A hello **javaslatok** panelen válassza ki a javaslat toosee részletei. Ebben a példában most válasszon **naplózás engedélyezését & Threat detection SQL-kiszolgálón**.

![Javaslatok][4]

Ahogy azt az alábbi, a Security Center, ahol a naplózás és a fenyegetések észlelésére nem engedélyezettek az SQL-kiszolgálók hello. Naplózás bekapcsolása után Fenyegetésészlelés beállításokat konfigurálja, és e-mail-beállítások tooreceive biztonsági riasztásokat. A Fenyegetésészlelés figyelmezteti, ha azt észleli, hogy adatbázist érintő rendellenes tevékenységeket, amelyek esetleges biztonsági fenyegetéseket toohello adatbázis jelzik. a Security Center irányítópultjának hello hello riasztások jelennek meg.
![Naplózás és a fenyegetések észlelése][5]

Hello kövesse [hello Azure-portálon az SQL-adatbázis fenyegetésészlelés](../sql-database/sql-database-threat-detection-portal.md) tooturn a fenyegetések észlelése és tooconfigure hello listáját, amelyek megkapják a biztonsági riasztásokat, a rendellenes tevékenységek észlelésekor e-mailek és konfigurálása.

toolearn vonatkozó javaslatokkal kapcsolatban bővebben lásd: [biztonsági javaslatok kezelése](security-center-recommendations.md).

## <a name="monitor-security-health"></a>A biztonsági állapot figyelése
Miután engedélyezte a [biztonsági házirendek](security-center-policies.md) az előfizetéshez tartozó erőforrásokra, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonságát.  Megtekintheti az erőforrások biztonsági állapotát hello hello **erőforrás biztonsági állapota** csempére. Elemre **adatok** a hello **erőforrás biztonsági állapota** csempére, hello **erőforrásokat** panel nyílik meg problémák, például a naplózás és átlátható SQL javaslatok nem engedélyezett az adatok titkosítását. Általános állapotát hello hello adatbázis javaslatok is rendelkezik.
![Erőforrás biztonsági állapota][6]

több, lásd: toolearn [biztonsági állapotfigyelés](security-center-monitoring.md).

## <a name="manage-and-respond-toosecurity-alerts"></a>Kezelésének és megoldásának toosecurity riasztások
A Security Center automatikusan gyűjti, elemzi és integrálja naplóadatait [Azure SQL Fenyegetésészlelés](../sql-database/sql-database-threat-detection.md), valamint az egyéb Azure-erőforrások, toodetect valós fenyegetések és a vakriasztások számának csökkentése érdekében. A rangsorolt biztonsági riasztások listája látható a Security Center együtt hello tooquickly szükséges információk vizsgálata hello probléma és módjára vonatkozó javaslatokkal tooremediate támadás.

toosee riasztások, jelölje be hello **biztonsági riasztások** hello Security Center panel csempét. A hello **biztonsági riasztások** panelen válassza egy riasztási toolearn több indított hello riasztást, és milyen, ha vannak ilyenek, lépésről lépésre hello eseményekkel kapcsolatban kell tootake tooremediate támadás. Ebben a példában most válasszon **lehetséges SQL-injektálás**.
![Biztonsági riasztások][7]

Alább látható módon a Security Center Itt további részleteket által biztosított milyen kiváltott hello riasztás betekintést hello célerőforrás, ha alkalmazható hello forrás IP-címet, és a fenyegetés elhárítására vonatkozó javaslatokról tooremediate.
![Lehetséges SQL-injektálás][8]

több, lásd: toolearn [toosecurity riasztások kezelése és válaszol](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Következő lépések
* [Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [A Security Center tervezésével és műveletek útmutató](security-center-planning-and-operations-guide.md) - kövesse az ismertetett lépések és feladatok toooptimize alapján a vállalat biztonsági igényeinek és felhőfelügyeleti modelljének Security Center használatát.
* [Biztonsági központ adatbiztonság](security-center-data-security.md) – megtudhatja, hogyan gyűjti a Security Center, és feldolgozza az Azure-erőforrások, például konfigurációs adatokat, metaadatokat, eseménynaplók, összeomlási memóriaképek és egyéb adatait.
* [Biztonsági incidensek kezelése](security-center-incident.md) -megtudhatja, hogyan toouse hello biztonsági riasztást a Security Center tooassist terén a biztonsági incidensek kezelése.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
