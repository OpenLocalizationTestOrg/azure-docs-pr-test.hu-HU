---
title: "az Azure SQL Database Transact-SQL aaaConfigure-georeplikációt |} Microsoft Docs"
description: "A georeplikáció konfigurálása az Azure SQL Database Transact-SQL használatával"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d94d89a6-3234-46c5-8279-5eb8daad10ac
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/14/2017
ms.author: carlrab
ms.openlocfilehash: 295b6b12f257dfb15131d5ee28fbe65c6476352d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-with-transact-sql"></a>Aktív georeplikáció konfigurálása az Azure SQL Database Transact-SQL

Ez a cikk bemutatja, hogyan tooconfigure aktív georeplikáció Azure SQL-adatbázis a Transact-SQL.

tooinitiate feladatátvételi Transact-SQL használatával lásd: [kezdeményezzen tervezett vagy nem tervezett feladatátvételt az Azure SQL Database Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

> [!NOTE]
> Aktív georeplikáció (olvasható másodlagos adatbázis) vész-helyreállítási használatakor, konfigurálnia kell egy feladatátvételi csoport az összes olyan adatbázis egy alkalmazás tooenable automatikus és transzparens feladatátvétellel belül. A funkció jelenleg előzetes verzió. További információ: [automatikus feladatátvételt csoportok és georeplikáció](sql-database-geo-replication-overview.md).
> 
> 

tooconfigure aktív georeplikáció Transact-SQL használatával, hello következő szükséges:

* Azure-előfizetés
* Egy Azure SQL Database logikai kiszolgáló <MyLocalServer> és SQL-adatbázis <MyDB> -hello elsődleges adatbázis, amelyet az tooreplicate
* Egy vagy több logikai Azure SQL Database kiszolgáló < MySecondaryServer(n) > - hello lesz hello partner kiszolgálók másodlagos adatbázisok hoz létre logikai kiszolgáló
* A bejelentkezési azonosítót, amely a DBManager az elsődleges hello
* Hello helyi adatbázis földrajzi-replikálás fogja végrehajtani db_ownership rendelkezik
* A georeplikáció konfigurálja a DBManager lehet hello partner kiszolgáló(k) toowhich
* hello legújabb verziója az SQL Server Management Studio (SSMS)

> [!IMPORTANT]
> Javasoljuk, hogy mindig használjon hello Azure és az SQL-adatbázis a frissítések tooMicrosoft szinkronizálva tooremain Management Studio legújabb verzióját. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="add-secondary-database"></a>Másodlagos adatbázis hozzáadása
Használhatja a hello **ALTER DATABASE** utasítás toocreate georeplikált másodlagos adatbázis egy partner-kiszolgálón. A jelen nyilatkozat hello hello server tartalmazó hello adatbázis toobe replikálja a főadatbázison hajtható végre. hello georeplikált adatbázis (hello "elsődleges adatbázis") lesz azonos nevet replikált hello adatbázis és a rendszer alapértelmezés szerint rendelkezik hello hello azonos szintű szolgáltatási hello elsődleges adatbázisként. hello másodlagos adatbázis olvasható vagy nem olvasható, és lehet egy adatbázist vagy a rugalmas készletekben található. További információkért lásd: [(Transact-SQL) adatbázis módosítása](https://msdn.microsoft.com/library/mt574871.aspx) és [szolgáltatásszintek](sql-database-service-tiers.md).
Miután hello másodlagos adatbázis létrehozták, és a rendezés, adatok megkezdődik aszinkron módon replikál hello elsődleges adatbázisból. hello következő lépések leírják, hogyan tooconfigure georeplikáció Management Studio használatával. Lépéseket toocreate olvashatók és nem olvasható másodlagos adatbázis, a rugalmas készlethez, vagy egy önálló adatbázis találhatók.

> [!NOTE]
> Ha létezik egy adatbázis-kiszolgálón hello megadott partner hello azonos nevet, amint hello elsődleges adatbázis hello parancs sikertelen lesz.
> 

### <a name="add-readable-secondary-single-database"></a>Adja hozzá az olvasható másodlagos (egyetlen adatbázisban)
A következő lépéseket toocreate egyetlen adatbázisként olvasható másodlagos hello használata.

1. A Management Studio eszközben csatlakozzon a tooyour Azure SQL Database logikai kiszolgálóhoz.
2. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **fő**, és kattintson a **új lekérdezés**.
3. Hello következő **ALTER DATABASE** utasítás toomake a másodlagos kiszolgálón olvasható másodlagos adatbázis georeplikáció elsődleges be egy helyi adatbázist.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);
4. Kattintson a **Execute** toorun hello lekérdezés.

### <a name="add-readable-secondary-elastic-pool"></a>Adja hozzá az olvasható másodlagos (rugalmas készlet)
A következő lépéseket toocreate rugalmas készlethez olvasható másodlagos hello használata.

1. A Management Studio eszközben csatlakozzon a tooyour Azure SQL Database logikai kiszolgálóhoz.
2. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **fő**, és kattintson a **új lekérdezés**.
3. Hello következő **ALTER DATABASE** utasítás toomake a másodlagos kiszolgálón a rugalmas készletekben található olvasható másodlagos adatbázis georeplikáció elsődleges be egy helyi adatbázist.
   
        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));
4. Kattintson a **Execute** toorun hello lekérdezés.

## <a name="remove-secondary-database"></a>Távolítsa el a másodlagos adatbázis
Használhatja a hello **ALTER DATABASE** utasítás toopermanently hello replikációs partnerség másodlagos adatbázis és az elsődleges leáll. Az utasítás végrehajtása hello master adatbázis elsődleges adatbázis melyik hello találhatók. Hello kapcsolat megszüntetése, után hello másodlagos adatbázis rendszeres olvasási és írási adatbázis válik. Ha megszakad a hello kapcsolat toosecondary adatbázis hello parancs sikeres, de másodlagos hello lesz írható-olvasható kapcsolat helyreállítását követő rögzítésénél. További információkért lásd: [(Transact-SQL) adatbázis módosítása](https://msdn.microsoft.com/library/mt574871.aspx) és [szolgáltatásszintek](sql-database-service-tiers.md).

Georeplikálási partnerség lépéseket tooremove georeplikált másodlagos követően hello használja.

1. A Management Studio eszközben csatlakozzon a tooyour Azure SQL Database logikai kiszolgálóhoz.
2. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **fő**, és kattintson a **új lekérdezés**.
3. Hello következő **ALTER DATABASE** utasítás tooremove egy georeplikált másodlagos.
   
        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;
4. Kattintson a **Execute** toorun hello lekérdezés.

## <a name="monitor-active-geo-replication-configuration-and-health"></a>Aktív georeplikáció konfigurációs és a rendszerállapot figyelése

A megfigyelési feladatok közé tartozik a hello georeplikáció konfigurációt megfigyelését és az adatokat replikálás állapotának figyelését.  Használhatja a hello **sys.dm_geo_replication_links** hello főadatbázis tooreturn adatokat az összes létező replikációs hivatkozások az egyes adatbázisok hello Azure SQL Database logikai kiszolgáló dinamikus kezelési nézetet. Ez a nézet az egyes elsődleges és másodlagos adatbázisok közötti hello-replikációs hivatkozást tartalmaz egy sort. Használhatja a hello **sys.dm_replication_link_status** dinamikus felügyeleti szeretné megtekinteni a tooreturn egy sor minden Azure SQL-adatbázist jelenleg egy replikációs replikációs hivatkozás foglalkozik. Ez magában foglalja az elsődleges és másodlagos adatbázisok. Ha egynél több folyamatos replikálási hivatkozás egy adott elsődleges adatbázis létezik, ez a táblázat az egyes hello kapcsolatok tartalmaz egy sort. hello nézet az összes adatbázis, beleértve a logikai master hello jön létre. Azonban ez a nézet a logikai master hello lekérdezése készletet ad vissza egy üres. Használhatja a hello **sys.dm_operation_status** dinamikus felügyeleti tooshow hello állapot megtekintése, az összes adatbázis-műveletek, beleértve hello hello replikációs hivatkozások állapotát. További információkért lásd: [sys.geo_replication_links (az Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (az Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx), és [sys.dm_operation_status (az Azure SQL Database)](https://msdn.microsoft.com/library/dn270022.aspx).

A következő lépéseket egy aktív georeplikáció toomonitor partnerség hello használata.

1. A Management Studio eszközben csatlakozzon a tooyour Azure SQL Database logikai kiszolgálóhoz.
2. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **fő**, és kattintson a **új lekérdezés**.
3. Használja a következő utasítás tooshow hello összes adatbázis georeplikáció mutató hivatkozásokat tartalmaz.
   
        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM sys.geo_replication_links;
4. Kattintson a **Execute** toorun hello lekérdezés.
5. Nyissa meg a hello adatbázismappát, és bontsa ki a hello **Rendszeradatbázisokban** mappát, kattintson a jobb gombbal a **MyDB**, és kattintson a **új lekérdezés**.
6. A következő utasítás tooshow hello replikációs használata hello késik és utolsó replikációs ideje a másodlagos adatbázisok a MyDB.
   
        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status
7. Kattintson a **Execute** toorun hello lekérdezés.
8. A következő utasítás tooshow hello legutóbbi georeplikáció műveletek MyDB adatbázishoz tartozó hello használata.
   
        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC
9. Kattintson a **Execute** toorun hello lekérdezés.

## <a name="next-steps"></a>Következő lépések
* További információ a feladatátvételi csoportok és aktív georeplikáció, toolearn lásd - [feladatátvételi csoportok](sql-database-geo-replication-overview.md)
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md)

