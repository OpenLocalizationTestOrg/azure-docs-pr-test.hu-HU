---
title: "aaaSQL Server adatbázis áttelepítési tooAzure SQL-adatbázis |} Microsoft Docs"
description: "Ismerje meg, mi a helyzet az SQL Server adatbázis áttelepítési tooAzure hello felhőben SQL-adatbázis. Adatbázis áttelepítési eszközök tootest kompatibilitási előzetes toodatabase áttelepítést használ."
keywords: "adatbázis-áttelepítés,sql server-adatbázis áttelepítése,adatbázis-áttelepítési eszközök,adatbázis áttelepítése,sql database áttelepítése"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 02/08/2017
ms.author: carlrab
ms.openlocfilehash: 3a5e879404dd2da1dd5254a6134e3ee1517648db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-database-migration-toosql-database-in-hello-cloud"></a>SQL Server adatbázis áttelepítési tooSQL adatbázis hello felhőben
Ebből a cikkből megismerheti kapcsolatos hello áttelepítés egy SQL Server 2005 vagy újabb adatbázis tooAzure SQL adatbázis két elsődleges módszere. hello első módszer egyszerűbb, de igényel, valószínűleg jelentős állásidőt hello az áttelepítés során. második módszer hello összetettebb, de jelentősen megszünteti az állásidő hello az áttelepítés során.

Mindkét esetben szüksége tooensure hello source adatbázis összeegyeztethető Azure SQL Database szolgáltatáshoz hello [adatok áttelepítési Segéd (DMA-)](https://www.microsoft.com/download/details.aspx?id=53595). SQL Database 12-es közeledik [szolgáltatásparitás](sql-database-features.md) az SQL Server eltérő problémák kapcsolódó tooserver szintű és az adatbázisok közötti műveletek. Adatbázisok és az alkalmazások [részben támogatott vagy nem támogatott funkciók](sql-database-transact-sql-information.md) kell néhány [átalakításának toofix Ezen inkompatibilitások](sql-database-cloud-migrate.md#resolving-database-migration-compatibility-issues) előtt hello SQL Server adatbázis telepíthetők át.

> [!NOTE]
> toomigrate nem SQL Server-adatbázisok, beleértve a Microsoft Access, Sybase, MySQL Oracle és DB2 tooAzure SQL-adatbázis, lásd: [SQL Server áttelepítés Segéd](https://blogs.msdn.microsoft.com/datamigration/2016/12/22/released-sql-server-migration-assistant-ssma-v7-2/).
> 

## <a name="method-1-migration-with-downtime-during-hello-migration"></a>1. módszer: Áttelepítés állásidővel hello az áttelepítés során

 Akkor használja ezt a módszert, ha megengedhet valamennyi állásidő, vagy ha tesztelési céllal migrál egy éles adatbázist a későbbi migráláshoz. Az oktatóanyagok esetén lásd: [egy SQL Server-adatbázis áttelepítése](sql-database-migrate-your-sql-server-database.md).

hello alábbi lista hello ezzel a módszerrel az SQL Server adatbázis áttelepítés általános munkafolyamata.

  ![VSSSDT áttelepítési ábra](./media/sql-database-cloud-migrate/azure-sql-migration-sql-db.png)

1. Hello adatbázis hello legújabb verzióját használja kompatibilitás ellenőrzéséhez [adatok áttelepítési Segéd (DMA-)](https://www.microsoft.com/download/details.aspx?id=53595).
2. A szükséges javításokat Transact-SQL szkriptekként készítse elő.
3. Hello forrásadatbázis tranzakciós úton konzisztens másolat készítése az áttelepítendő -, és győződjön meg arról nincs további módosul toohello forrásadatbázis (vagy manuálisan alkalmazhat az ilyen jellegű változtatásokat a hello az áttelepítés befejezése után). Egy adatbázis, az ügyfél-csatlakozási toocreating letiltása sok módszerek tooquiesce vannak egy [adatbázis-pillanatkép](https://msdn.microsoft.com/library/ms175876.aspx).
4. Hello Transact-SQL parancsfájlok tooapply hello javítások toohello adatbázis-másolat telepítéséhez.
5. [Exportálás](sql-database-export.md) adatbázis másolása tooa hello. Egy helyi meghajtó BACPAC-fájl.
6. [Importálás](sql-database-import.md) hello. Bármely több BACPAC használatával új Azure SQL-adatbázis BACPAC fájlt importálja a eszközök, az eszköz a legjobb teljesítmény érdekében ajánlott hello alatt SQLPackage.exe.

### <a name="optimizing-data-transfer-performance-during-migration"></a>Az adatátviteli teljesítmény optimalizálása migrálás közben 

a következő lista hello hello importálási folyamat során a legjobb teljesítmény érdekében ajánlásokat tartalmazza.

* Válassza ki a hello legmagasabb szolgáltatási szint és a teljesítmény, a költségvetési lehetővé teszi az toomaximize hello átviteli teljesítmény réteg. Hello áttelepítési toosave pénz befejezése után is csökkentheti. 
* Hello közötti távolság minimalizálása érdekében a. BACPAC fájl- és hello cél adatközpont.
* Tiltsa le az automatikus statisztikákat a migrálás alatt.
* Particionálja a táblákat és az indexeket.
* Vesse el, majd a folyamat befejezése után hozza létre újra az indexelt nézeteket.
* Ritkán lekérdezett előzményadatokat tooanother adatbázis eltávolítása, és telepítse át az előzményadatok tooa külön Azure SQL-adatbázis. Ezután lekérdezheti ezeket az előzményadatokat a [rugalmas lekérdezések](sql-database-elastic-query-overview.md) használatával.

### <a name="optimize-performance-after-hello-migration-completes"></a>A teljesítmény optimalizálása hello az áttelepítés befejezése után

[Statisztika frissítése](https://msdn.microsoft.com/library/ms187348.aspx) a teljes vizsgálat hello áttelepítés befejezése után.

## <a name="method-2-use-transactional-replication"></a>2. módszer: Tranzakciós replikáció használata

Ha Ön nem biztosít tooremove az SQL Server-adatbázis nem éles környezetben hello áttelepítés során, a tranzakciós replikáció az SQL Server használata áttelepítési megoldásként is használhatja. Ezzel a módszerrel hello forrásadatbázis kell toouse igazodhat hello [követelmények, a tranzakciós replikáció](https://msdn.microsoft.com/library/mt589530.aspx) és az Azure SQL Database kompatibilis. Az AlwaysOn az SQL-replikációval kapcsolatos információkért lásd: [replikáció konfigurálása az Always On rendelkezésre állási csoportok (SQL Server)](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server).

toouse Ez a megoldás konfigurálása az Azure SQL Database előfizető toohello SQL Server-példányt, hogy kívánja-e toomigrate. hello tranzakciós replikáció terjesztő szinkronizálja az adatokat a hello adatbázis toobe szinkronizálva (hello publisher) új tranzakciók leállítaná fordulhat elő. 

Tranzakciós replikáció, az összes módosítások tooyour adatok vagy séma jelenik meg az Azure SQL Database adatbázishoz. Hello szinkronizálása befejeződött, és készen áll a toomigrate, módosíthatja az alkalmazások toopoint hello kapcsolati karakterlánca őket az Azure SQL Database tooyour. Miután tranzakciós replikáció kiüríti a bal oldali módosításokat a forrás-adatbázis és az alkalmazások terjesztésipont-tooAzure DB, a tranzakciós replikáció eltávolíthatja. Mostantól az Azure SQL Database az éles rendszer.

 ![SeedCloudTR-diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

> [!TIP]
> Használhatja a tranzakciós replikáció toomigrate a forrás-adatbázis egy részét is. hello kiadványt, hogy a tooAzure SQL-adatbázis replikálása replikált hello adatbázis tábláit hello korlátozott tooa részhalmaza lehet. Replikált táblák korlátozhatja hello adatok tooa részhalmazát hello sorok és/vagy az hello oszlopok egy részét.
>

### <a name="migration-toosql-database-using-transaction-replication-workflow"></a>Áttelepítési tooSQL adatbázis használatával. a tranzakció replikációs munkafolyamat

> [!IMPORTANT]
> Használjon hello legújabb verziójára SQL Server Management Studio tooremain szinkronizálva frissítések tooMicrosoft Azure és az SQL-adatbázis. Az SQL Server Management Studio régebbi verzióiban az SQL Database nem állítható be előfizetőként. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).
> 

1. Terjesztés beállítása
   -  [SQL Server Management Studio (SSMS) használatával](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   -  [Transact-SQL használatával](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)
2. Kiadvány létrehozása
   -  [SQL Server Management Studio (SSMS) használatával](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   -  [Transact-SQL használatával](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Előfizetés létrehozása
   -  [SQL Server Management Studio (SSMS) használatával](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   -  [Transact-SQL használatával](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

### <a name="some-tips-and-differences-for-migrating-toosql-database"></a>Néhány tipp és különbségek áttelepítéséhez tooSQL adatbázis

1. Helyi terjesztő használata 
   - Ennek hatására a teljesítményre gyakorolt hatás hello kiszolgálón. 
   - Hello teljesítményre gyakorolt hatás nem fogadható el, ha egy másik kiszolgálót is használhat, de összetettsége hozzáadja a felügyelet és kezelés.
2. Ha a pillanatkép mappája, ellenőrizze választja meg arról, hogy hello mappa kiválasztásával elég nagy toohold a BCP minden tábla tooreplicate szeretné. 
3. Pillanatkép létrehozása zárolások hello társított táblázatokat, amíg nem fejeződik be, úgy ütemezze megfelelően a pillanatkép. 
4. Az Azure SQL Database csak a leküldéses előfizetéseket támogatja. Hello forrásadatbázisból csak előfizetők adhat hozzá.

## <a name="resolving-database-migration-compatibility-issues"></a>Adatbázis-migrálás kompatibilitási problémáinak megoldása
Nincsenek széles körének a kompatibilitási problémák léphetnek fel, attól függően, mind az SQL Server verziójának hello hello source adatbázis és hello összetettsége hello adatbázis végzi az áttelepítést. Az SQL Server korábbi verziói több kompatibilitási problémával rendelkeznek. A következő erőforrások hello használatához továbbá tooa céloz internetes keresés a választási lehetőségek keresőmotor segítségével:

* [Az Azure SQL Database-ben nem támogatott SQL Server-adatbázisfunkciók](sql-database-transact-sql-information.md)
* [Megszűnt adatbázismotor-funkció az SQL Server 2016-ban](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Megszűnt adatbázismotor-funkció az SQL Server 2014-ben](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Megszűnt adatbázismotor-funkció az SQL Server 2012-ben](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Megszűnt adatbázismotor-funkció az SQL Server 2008 R2-ben](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Megszűnt adatbázismotor-funkció az SQL Server 2005-ben](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Továbbá a toosearching hello Internet, és ezeket az erőforrásokat, a használatával hello [MSDN SQL Server közösségi fórumok](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) vagy [StackOverflow](http://stackoverflow.com/).

## <a name="next-steps"></a>Következő lépések
* Parancsfájllal hello hello Azure SQL EMEA mérnökök blogjában túl[tempdb megfigyeléséhez áttelepítéskor](https://blogs.msdn.microsoft.com/azuresqlemea/2016/12/28/lesson-learned-10-monitoring-tempdb-usage/).
* Parancsfájllal hello hello Azure SQL EMEA mérnökök blogjában túl[hello tranzakciónaplók helyhasználatáról az adatbázis figyelése az áttelepítés során](https://blogs.msdn.microsoft.com/azuresqlemea/2016/10/31/lesson-learned-7-monitoring-the-transaction-log-space-of-my-database/0).
* Egy SQL Server Ügyféltanácsadói csapatának blogja áttelepítéssel kapcsolatos BACPAC-fájlokkal, lásd: [áttelepítése az SQL Server tooAzure SQL-adatbázis BACPAC-fájlokat használ](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Az áttelepítés után az UTC-idő használatáról információkért lásd: [módosítása hello alapértelmezett időzónát a helyi időzóna](https://blogs.msdn.microsoft.com/azuresqlemea/2016/07/27/lesson-learned-4-modifying-the-default-time-zone-for-your-local-time-zone/).
* Az áttelepítés után egy adatbázis alapértelmezett nyelve hello módosításával kapcsolatban bővebben lásd: [hogyan toochange hello Azure SQL-adatbázis alapértelmezett nyelve](https://blogs.msdn.microsoft.com/azuresqlemea/2017/01/13/lesson-learned-16-how-to-change-the-default-language-of-azure-sql-database/).


