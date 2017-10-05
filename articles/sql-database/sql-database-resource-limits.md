---
title: "Az Azure SQL adatbázis erőforrás-korlátozások |} Microsoft Docs"
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
ms.openlocfilehash: e75458db79e6c15f8d5155b71f04a20fa21818ff
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-sql-database-resource-limits"></a>Az Azure SQL Database erőforrás korlátok
## <a name="overview"></a>Áttekintés
Az Azure SQL Database kezeli egy adatbázist két különböző mechanizmusok számára elérhető erőforrások: **erőforrások irányítás** és **vonatkozó korlátok kényszerítési**. Ez a témakör azt ismerteti, ezek két fő területet az erőforrás-kezelés.

## <a name="resource-governance"></a>Erőforrás-irányítás
A Basic, Standard, Premium és prémium RS szolgáltatási szinteket a céljai egyik úgy kezd viselkedni, mintha az adatbázis fut a saját gép különítve az egyéb adatbázisok Azure SQL-adatbázis. Erőforrás-irányítás emulálja ezt a viselkedést. Ha az összesített erőforrás-használat eléri a maximális rendelkezésre álló Processzor, memória, a napló i/o és az adatok i/o erőforrások rendelt adatbázis, erőforrás irányítás várólisták lekérdezések végrehajtása, és erőforrások hozzárendelése a várólistán lévő lekérdezéseket, mivel azok szabadítson fel.

Mint dedikált gépre, jelenleg feldolgozás alatt álló lekérdezések, hosszabb végrehajtásának eredményez az összes rendelkezésre álló erőforrások okhoz ami eredményezhet parancs időtúllépések az ügyfélen. Az alkalmazásoknak agresszív újrapróbálkozási logika és alkalmazásokat, amelyek nagy gyakorisággal adatbázis-lekérdezéseinek végrehajtására találkozhat hibák üzenetek új-lekérdezéseket hajt végre, amikor elérte az egyidejű kérelmek maximális közben.

### <a name="recommendations"></a>Javaslatok:
Figyelése az erőforrás-használat és az átlagos válaszideje a lekérdezések, ha az adatbázis maximális kihasználtsági hamarosan. Amikor észlelése magasabb lekérdezés késések általában három lehetősége van:

1. Adjon meg kevesebb bejövő kérelmek időtúllépési és mentése a kérelmek halom megelőzése érdekében az adatbázisba.
2. Rendelje hozzá egy magasabb teljesítményszintre az adatbázisban.
3. Minden egyes lekérdezés az erőforrás-használat csökkentésére lekérdezések optimalizálását. További információ az Azure SQL adatbázis teljesítményének útmutatást cikk lekérdezés hangolása/Hinting szakaszában talál.

## <a name="enforcement-of-limits"></a>A korlátozások érvényesítése
Eltérő Processzor, memória, napló i/o és adatok i/o erőforrások új kérelmek megtagadása, korlátok elérésekor érvényesíti. Amikor adatbázis eléri a beállított maximális méretkorlátot, beszúrások és a frissítések, növelheti az adatok mérete sikertelen, amíg választja ki, és a törlések továbbra is működnek. Elküld az ügyfélgépeknek egy [hibaüzenet](sql-database-develop-error-messages.md) attól függően, hogy a rendszer elérte a korlátot.

Például az SQL-adatbázis kapcsolatok számát és a feldolgozható egyidejű kérelmek száma korlátozva. SQL-adatbázis lehetővé teszi, hogy az adatbázis kapcsolatkészletezést támogatásához egyidejű kérelmek száma nagyobb kapcsolatok száma. Során rendelkezésre álló kapcsolatok száma az alkalmazások könnyen szabályozhatók, párhuzamos kérések száma gyakran alkalommal nehezebben becsléséhez és szabályozására. Csúcsidőszak terhelések során különösen ha az alkalmazás túl sok kéréssel vagy az adatbázis eléri az erőforrást küld vagy korlátozza, és elindítja a munkaszálak hosszabb futó lekérdezések, mert felhalmozódjanak hibák is kell fel.

## <a name="service-tiers-and-performance-levels"></a>Szolgáltatásszintek és teljesítményszintek
Szolgáltatásszintek és teljesítményszintek az önálló adatbázisok és a rugalmas készletek van.

### <a name="single-databases"></a>Önálló adatbázisok
A egy adatbázist az adatbázis-szolgáltatás és teljesítményszintet szint határozza meg egy adatbázis határain. A következő táblázat ismerteti a különböző teljesítményszintek Basic, Standard, Premium és prémium RS adatbázisaiban jellemzőit.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!IMPORTANT]
> P11 és P15 teljesítményszintet használó ügyfelek használhatják a belefoglalt tárolási 4 TB-os használatáért nem kell külön fizetni. A 4 TB-os beállítás érhető el jelenleg a következő régióban: US East2, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Dél Kelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.
>

### <a name="elastic-pools"></a>Rugalmas készletek
[Rugalmas készletek](sql-database-elastic-pool.md) osztozik az erőforrásokon a készletben lévő adatbázisok között. A következő táblázat ismerteti a Basic, Standard, Premium és prémium RS rugalmas készletek jellemzőit.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Az egyes erőforrások, az előző táblázatban felsorolt egy bővített meghatározása: a leírásokat [szolgáltatásszintek lehetőségei és korlátai](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Szolgáltatásszintek áttekintését lásd: [Azure SQL adatbázis szolgáltatásszintjei és Teljesítményszintjei](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Más SQL-adatbázis korlátok
| Terület | Korlát | Leírás |
| --- | --- | --- |
| Az automatikus exportálás előfizetésenként adatbázisok |10 |Az automatikus exportálás lehetővé teszi, hogy hozzon létre egy egyénit az SQL-adatbázisok biztonsági mentésére. Ez a funkció előzetes 2017. március 1. a véget ér.  |
| Adatbázis kiszolgálónként |Legfeljebb 5000 |Legfeljebb 5000 adatbázisok kiszolgálónként engedélyezettek. |
| Dtu-k kiszolgálónként |45000 |45000 dtu-k engedélyezettek kiszolgálónként önálló adatbázisok és rugalmas készletek történő üzembe helyezéséhez. Önálló adatbázisok és a készletek kiszolgálónként engedélyezett száma csak a kiszolgáló dtu-inak száma korlátozza.  

> [!IMPORTANT]
> Az Azure SQL adatbázis automatikus exportálása jelenleg előzetes verzióban érhetők és 2017. március 1. a rendszerből. Kezdési 2016. December 1., pedig már nem fogják tudni automatikus konfigurálása a bármely SQL-adatbázis. Exportálási feladatok továbbra is használhatók, amíg 2017. március 1. az összes meglévő automatikus. 2016. December 1. után használhatja [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md) vagy [Azure Automation](../automation/automation-intro.md) archiválására SQL-adatbázisok segítségével rendszeres időközönként PowerShell rendszeres időközönként a kiválasztott ütemezés szerint. Egy parancsfájlt, a programot letöltheti a [mintaparancsfájl a Githubról](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export).
>


## <a name="resources"></a>Erőforrások
[Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md)

[Az Azure SQL adatbázis szolgáltatásszintjei és Teljesítményszintjei](sql-database-service-tiers.md)

[Az SQL Database-ügyfélprogramok konfigurálása](sql-database-develop-error-messages.md)
