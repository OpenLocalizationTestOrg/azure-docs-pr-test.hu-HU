---
title: "SQL adatbázis erőforrás korlátok aaaAzure |} Microsoft Docs"
description: "Ez a lap néhány általános erőforrás-korlátozások az Azure SQL Database ismerteti."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2017
ms.author: carlrab
ms.openlocfilehash: 3e7be4fdc74e802d37274690531caaf138bcedb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-resource-limits"></a>Az Azure SQL Database erőforrás korlátok
## <a name="overview"></a>Áttekintés
Az Azure SQL Database kezeli hello erőforrások elérhető tooa adatbázist két különböző mechanizmusok alapján: **erőforrások irányítás** és **vonatkozó korlátok kényszerítési**. Ez a témakör azt ismerteti, ezek két fő területet az erőforrás-kezelés.

## <a name="resource-governance"></a>Erőforrás-irányítás
A hello Basic, Standard, Premium és prémium RS szolgáltatási szinteket hello céljai egyike az Azure SQL Database toobehave, mintha a saját számítógépen, különítve az egyéb adatbázisok fut-e a hello adatbázis. Erőforrás-irányítás emulálja ezt a viselkedést. Ha hello összesített erőforrás-használat eléri hello maximális rendelkezésre álló CPU, a memória, a napló i/o és az adatok i/o-erőforrások hozzárendelt toohello adatbázis erőforrás irányítás várólisták lekérdezések végrehajtása az erőforrások és hozzárendelése toohello várólistára azok lekérdezések szabadítson fel.

Mint dedikált gépre, jelenleg feldolgozás alatt álló lekérdezések, hosszabb végrehajtásának eredményez az összes rendelkezésre álló erőforrások okhoz ami eredményezhet hello ügyfél időtúllépése parancsot. Alkalmazások agresszív újrapróbálkozási logika és alkalmazásokat, amelyek nagy gyakorisággal hello adatbázis-lekérdezéseinek végrehajtására is előforduló hibák üzenetek tooexecute új lekérdezések közben, amikor elérte az egyidejű kérelmek hello korlátozását.

### <a name="recommendations"></a>Javaslatok:
Ha egy adatbázis-hello maximális kihasználtságot hamarosan, figyelheti a hello erőforrások kihasználtságát és a hello átlagos válaszideje a lekérdezések. Amikor észlelése magasabb lekérdezés késések általában három lehetősége van:

1. Csökkentse a bejövő kérések toohello adatbázis tooprevent időtúllépés és hello most be kérelmek hello számát.
2. Rendeljen hozzá egy magasabb teljesítmény szintű toohello adatbázis.
3. Lekérdezések tooreduce hello erőforrás-használat minden egyes lekérdezés optimalizálására. További információkért lásd: hello lekérdezés hangolása/Hinting szakasz hello Azure SQL adatbázis teljesítményének útmutatást cikkben.

## <a name="enforcement-of-limits"></a>A korlátozások érvényesítése
Eltérő Processzor, memória, napló i/o és adatok i/o erőforrások új kérelmek megtagadása, korlátok elérésekor érvényesíti. Ha egy adatbázis eléri a konfigurált hello maximális mérete, a beszúrások és a frissítések, amelyek növelik sikertelen adatok mérete, választja ki, és a törlések leállítaná toowork. Elküld az ügyfélgépeknek egy [hibaüzenet](sql-database-develop-error-messages.md) attól függően, hogy hello korlát, amely el lett érve.

Például hello kapcsolatok tooa SQL-adatbázis és hello számát, amelyek dolgozhatók egyidejű kérelmek korlátozódnak. SQL-adatbázis lehetővé teszi, hogy kapcsolatok toohello adatbázis toobe hello száma nagyobb, mint az egyidejű kérelmek toosupport kapcsolatkészletezést hello száma. Során rendelkezésre álló kapcsolatok száma hello hello alkalmazás könnyen szabályozhatók, hello száma párhuzamos kérelmek gyakran alkalommal nehezebben tooestimate és toocontrol. Különösen során csúcs terhelések hello alkalmazás vagy túl sok kérelmet küld, vagy hello eléri az erőforrás-korlátozások és munkaszál miatt toolonger futó lekérdezések felhalmozódjanak elindul, hibák is fel.

## <a name="service-tiers-and-performance-levels"></a>Szolgáltatásszintek és teljesítményszintek
Szolgáltatásszintek és teljesítményszintek az önálló adatbázisok és a rugalmas készletek van.

### <a name="single-databases"></a>Önálló adatbázisok
Egy adatbázist, az adatbázis hello korlátai által megszabott hello adatbázis és teljesítményszintet szolgáltatásszint határozzák meg. a következő táblázat hello különböző teljesítményszintek Basic, Standard, Premium és prémium RS adatbázisaiban hello jellemzőit írja le.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> P11 és P15 teljesítményszintet használó ügyfelek is elhasználja too4 TB-nyi belefoglalt storage használatáért nem kell külön fizetni. A 4 TB-os beállítás érhető el jelenleg a következő régiókban hello: US East2, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Dél Kelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.
>

### <a name="elastic-pools"></a>Rugalmas készletek
[Rugalmas készletek](sql-database-elastic-pool.md) osztozik az erőforrásokon hello készletben adatbázisok között. a következő táblázat hello Basic, Standard, Premium és prémium RS rugalmas készletek hello jellemzőit írja le.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Bővített meghatározását az egyes erőforrások hello előző táblázatban, lásd: hello leírásokat [szolgáltatásszintek lehetőségei és korlátai](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Szolgáltatásszintek áttekintését lásd: [Azure SQL adatbázis szolgáltatásszintjei és Teljesítményszintjei](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Más SQL-adatbázis korlátok
| Terület | Korlát | Leírás |
| --- | --- | --- |
| Az automatikus exportálás előfizetésenként adatbázisok |10 |Automatikus lehetővé teszi egy egyéni ütemezést toocreate az SQL-adatbázisok biztonsági mentésére. Ez a szolgáltatás hello előnézete 2017. március 1. a véget ér.  |
| Adatbázis kiszolgálónként |Too5000 mentése |Másolatot too5000 adatbázisok kiszolgálónként használata engedélyezett. |
| Dtu-k kiszolgálónként |45000 |45000 dtu-k engedélyezettek kiszolgálónként önálló adatbázisok és rugalmas készletek történő üzembe helyezéséhez. hello száma önálló adatbázisok és a készletek engedélyezett-e a kiszolgáló csak a kiszolgáló dtu-inak száma hello korlátozza.  

> [!IMPORTANT]
> Az Azure SQL adatbázis automatikus exportálása jelenleg előzetes verzióban érhetők és 2017. március 1. a rendszerből. Kezdési 2016. December 1. már nem lesz képes tooconfigure automatikus Exportálás bármely SQL-adatbázison. A meglévő automatizált exportálási feladat 2017. március 1. amíg toowork továbbra is. 2016. December 1. után használhatja [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md) vagy [Azure Automation](../automation/automation-intro.md) tooarchive SQL-adatbázisainak rendszeres időközönként a powershellel rendszeres időközönként a az Ön által választott tooa ütemezés szerint. Egy minta parancsfájlt a hello letölthető [mintaparancsfájl a Githubról](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export).
>


## <a name="resources"></a>Erőforrások
[Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md)

[Az Azure SQL adatbázis szolgáltatásszintjei és Teljesítményszintjei](sql-database-service-tiers.md)

[Az SQL Database-ügyfélprogramok konfigurálása](sql-database-develop-error-messages.md)
