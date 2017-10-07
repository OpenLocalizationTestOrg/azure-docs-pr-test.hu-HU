---
title: "kiterjesztett felhő az adatbázisok közötti aaaReporting |} Microsoft Docs"
description: "Hogyan tooset vízszintes partíciók a rugalmas lekérdezések mentése"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: f86eccb8-6323-4ba7-8559-8a7c039049f3
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: mlandzic
ms.openlocfilehash: 78986c2040bf308195bf7c77e64d4f37273fcf36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Jelentéskészítési közötti kiterjesztett felhő (előzetes verzió)
![Átfogó szilánkok lekérdezése][1]

Horizontálisan skálázott adatbázisok sorok szét méretezett kimenő adatok réteg. hello séma részt vevő adatbázisok, más néven vízszintes particionálás megegyezik. Rugalmas lekérdezéssel, jelentéseket is létrehozhat, amelyek több összes adatbázis szilánkos-adatbázisban.

Tekintse meg a gyors üzembe helyezési [kiterjesztett felhő az adatbázisok közötti Reporting](sql-database-elastic-query-getting-started.md).

Nem szilánkos adatbázisok, lásd: [eltérő sémával felhő az adatbázisok közötti lekérdezés](sql-database-elastic-query-vertical-partitioning.md). 

## <a name="prerequisites"></a>Előfeltételek
* Hello elastic database ügyféloldali kódtár segítségével shard térkép létrehozásához. Lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md). Vagy használja a hello mintaalkalmazás [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).
* Azt is megteheti, lásd: [áttelepítése a meglévő adatbázis tooscaled kibővített adatbázis](sql-database-elastic-convert-to-use-elastic-tools.md).
* hello felhasználónak rendelkeznie kell ALTER ANY külső ADATFORRÁS engedéllyel. Ez az engedély megtalálható hello ALTER DATABASE engedéllyel.
* Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.

## <a name="overview"></a>Áttekintés
Ezekre az utasításokra metaadatok alakot hello a szilánkos adatrétegbeli hello rugalmas lekérdezési adatbázis létrehozása. 

1. [A FŐKULCS LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms174382.aspx)
2. [HOZZON LÉTRE AZ ADATBÁZISHOZ KÖTŐDŐ HITELESÍTŐ ADATOK](https://msdn.microsoft.com/library/mt270260.aspx)
3. [KÜLSŐ ADATFORRÁS LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [KÜLSŐ TÁBLA LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 adatbázis hatókörű főkulcs és hitelesítő adatainak létrehozása
hello rugalmas lekérdezési tooconnect tooyour távoli adatbázisok hello hitelesítő adatot használja.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Győződjön meg arról, hogy hello *"\<felhasználónév\>"* nem vonatkozik a *"@servername"* utótag. 
> 
> 

## <a name="12-create-external-data-sources"></a>1.2 külső adatforrás létrehozása
Szintaxis:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Példa
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

Lekéri az aktuális külső adatforrások hello listája: 

    select * from sys.external_data_sources; 

hello külső adatforráshoz a shard térképre hivatkozik. Egy rugalmas lekérdezési ezután hello külső adatforrást és az alapul szolgáló shard leképezési tooenumerate hello adatbázisok, amelyek hello adatrétegbeli hello. hello ugyanazokkal a hitelesítő adatokkal használt tooread hello shard térkép és tooaccess hello szilánkok adatok hello rugalmas lekérdezés hello feldolgozása során. 

## <a name="13-create-external-tables"></a>1.3 külső táblák létrehozása
Szintaxis:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Példa**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 

    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
         SCHEMA_NAME = 'orders', 
         OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Lekéri a külső táblák listáját hello hello jelenlegi adatbázisból: 

    SELECT * from sys.external_tables; 

toodrop külső táblák:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Megjegyzések
hello adatok\_forrás záradék hello külső adatforrás (szilánkok térképre) hello külső tábla használt határozza meg.  

hello SÉMA\_nevét és objektum\_neve záradékok hello külső tábla definíciójának tooa tábla egy másik séma hozzárendelését. Ha nincs megadva, hello séma hello távoli objektum toobe "dbo" feltételezi, és felhasználónevében feltételezi múltbeli toobe azonos toohello külső tábla neve. Ez akkor hasznos, ha a távoli tábla hello neve már használatban van hello adatbázisban, amelyben szeretné toocreate hello külső tábla. Például azt szeretné, hogy egy külső tábla tooget egy összesítő nézetét katalógusnézetekre toodefine, vagy a kimenő méretezett adatai dinamikus felügyeleti nézetek réteg. Katalógusnézetekre és dinamikus felügyeleti nézetek létezik helyileg, mivel a nevek hello külső tábla definíciójának nem használható. Ehelyett használjon egy másik nevet és hello katalógus nézetben vagy DMV meg nevet a SÉMA hello hello\_név és/vagy az objektum\_záradékok neve. (Lásd az alábbi példa hello.) 

hello TERJESZTÉSI záradékban hello adatok terjesztési használt ehhez a táblához. hello lekérdezésfeldolgozó található hello TERJESZTÉSI záradék toobuild hello leghatékonyabb lekérdezésterveket hello információk használja.  

1. **Horizontálisan SKÁLÁZOTT** azt jelenti, hogy adatok vízszintesen particionálása hello adatbázisok között. Particionálás kulcs hello adatok terjesztési: hello hello **< sharding_column_name >** paraméter.
2. **REPLIKÁLT** azt jelenti, hogy az egyes adatbázisok jelen-e hello azonos példányát. A felelősség tooensure, hogy hello replikák megegyeznek hello adatbázisok között.
3. **KEREK\_MULTIPLEXELÉS** azt jelenti, hogy hello tábla vízszintesen particionálva van, az alkalmazás-függő elosztási módszer használatával. 

**Adatok réteg hivatkozás**: hello külső DDL a táblázat tooan külső adatforráshoz. hello külső adatforrás határozza meg, amely hello információ szükséges toolocate hello külső tábla biztosít az adatréteg összes hello adatbázisának shard térképet. 

### <a name="security-considerations"></a>Biztonsági szempontok
Felhasználók és a hozzáférés toohello külső tábla automatikusan hozzáférést toohello alapul szolgáló távoli táblák hello külső adatforrása definíciója megadott hello hitelesítő adatok alapján. Hello hello külső adatforrás hitelesítő adatai segítségével nemkívánatos jogok kiterjesztésének elkerülése érdekében. Használja GRANT vagy VISSZAVONNI egy külső tábla ugyanúgy, mintha az lenne normál táblákhoz.  

Miután meghatározta a külső adatforrást és a külső táblák, a külső táblákon végrehajtott most már használhatja a teljes T-SQL.

## <a name="example-querying-horizontal-partitioned-databases"></a>Példa: vízszintes particionált adatbázisok lekérdezése
hello következő lekérdezés során, a rendeléseket és a sorok háromutas illesztést végez és több összesítések és szelektív szűrőt. Azt feltételezi, hogy (1) vízszintes particionálási (horizontális skálázási) és (2), hogy adatraktárakra, rendeléseket és sorok szilánkos hello adatraktár azonosító oszlop szerint, és hello rugalmas lekérdezés közös elhelyezése a hello szilánkok hello illesztéseket, és a hello hello lekérdezés költséges részét hello feldolgozni a szilánkok párhuzamosan. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Tárolt eljárás távoli T-SQL-végrehajtásra: sp\_execute_remote
Rugalmas lekérdezési is vezet be, amely közvetlen hozzáférést biztosít a toohello szilánkok tárolt eljárást. hello tárolt eljárás neve [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) és használt tooexecute távoli tárolt eljárások és a T-SQL kód a hello távoli adatbázis is lehet. A következő paraméterek hello tart: 

* Adatforrás neve (nvarchar): hello külső adatforrás RDBMS típusú hello neve. 
* Lekérdezés (nvarchar): hello T-SQL lekérdezés toobe minden shard végre. 
* Paraméterdeklarációhoz (nvarchar) – nem kötelező: adatok típusdefiníciók hello paraméterek hello lekérdezési paraméter (például sp_executesql) használt karakterlánc. 
* Paraméter értéklistát - választható: paraméterértékek (például sp_executesql) vesszővel tagolt listája.

hello sp\_hajtható végre\_távoli használ hello külső adatforrás hello Meghívási paraméterek tooexecute hello T-SQL-utasításban megadott hello távoli adatbázisok találhatók. Hello hitelesítő adatai hello külső adatok forrás tooconnect toohello shardmap manager-adatbázis és a távoli adatbázisokhoz hello használ.  

Példa: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Eszközök kapcsolattal
Rendszeres SQL Server kapcsolati karakterláncok tooconnect használja az alkalmazást, a BI és az integráció eszközök toohello adatbázis a külső tábla-definíciók. Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként. Majd hivatkozhat hello rugalmas lekérdezési adatbázis, mint bármely más SQL Server adatbázis csatlakoztatva toohello eszközt, és a külső táblákhoz az eszköz vagy alkalmazás használja, helyi táblákkal, mintha. 

## <a name="best-practices"></a>Ajánlott eljárások
* Győződjön meg arról, hello rugalmas lekérdezési végpont adatbázis megkapta-e hozzáférési toohello shardmap adatbázis és SQL-adatbázis a tűzfalak hello keresztül minden szilánkok.  
* Ellenőrzi, vagy hello adatok terjesztési hello külső tábla által meghatározott kényszerítéséhez. Ha a tényleges adatok terjesztési eltér a tábla definíciójában megadott hello terjesztési, a lekérdezések előfordulhat, hogy eredményt várt. 
* Rugalmas lekérdezés jelenleg nem végeznek eltávolítási szilánkok predikátumok hello horizontális kulcs keresztül lehetővé tenné, toosafely kizárási bizonyos szilánkok feldolgozása.
* Rugalmas lekérdezési működik a legjobban az lekérdezések ahol hello szilánkok hello számítási többsége végezhető. Általában beolvasása a legjobb lekérdezési teljesítményt hello szelektív szűrő predikátumok kiértékelhető hello szilánkok vagy illesztések keresztül az összes szilánkok partíciójához módon végrehajtható kulcsok particionálás hello. Más lekérdezési mintáinak esetleg tooload nagy mennyiségű hello szilánkok toohello átjárócsomópont adatait, és rosszul hajthatja végre

## <a name="next-steps"></a>Következő lépések

* Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).
* Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).
* A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)
* Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).
* Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
