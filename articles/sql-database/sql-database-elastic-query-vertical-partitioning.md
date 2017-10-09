---
title: "a különböző séma adatbázisok aaaQuery felhő között |} Microsoft Docs"
description: "Hogyan tooset adatbázisok közötti függőleges partíció a lekérdezések mentése"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 1134e2e608128b7a9cac47ff35a22a11e6e5bc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Más sémák (előzetes verzió) felhő adatbázisokkal átfogó lekérdezése
![A különböző adatbázisokhoz táblák átfogó lekérdezése][1]

Adatbázisok függőleges particionált táblák más-más részhalmazához különböző adatbázist használja. Ez azt jelenti, hogy adott hello sémája nem egyezik meg a különböző adatbázisokhoz. Például a készlet összes tábla esetén egy adatbázis míg nyilvántartási kapcsolatos összes táblázatot egy második adatbázison. 

## <a name="prerequisites"></a>Előfeltételek
* hello felhasználónak rendelkeznie kell ALTER ANY külső ADATFORRÁS engedéllyel. Ez az engedély megtalálható hello ALTER DATABASE engedéllyel.
* Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.

## <a name="overview"></a>Áttekintés

> [!NOTE]
> Ellentétben a vízszintes particionálás, a DDL-utasításokban nem függ egy adatrétegbeli keresztül hello elastic database ügyféloldali kódtára a shard leképezést definiáló.
>

1. [A FŐKULCS LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms174382.aspx)
2. [HOZZON LÉTRE AZ ADATBÁZISHOZ KÖTŐDŐ HITELESÍTŐ ADATOK](https://msdn.microsoft.com/library/mt270260.aspx)
3. [KÜLSŐ ADATFORRÁS LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935022.aspx)
4. [KÜLSŐ TÁBLA LÉTREHOZÁSA](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Adatbázis hatókörű főkulcs és hitelesítő adatainak létrehozása
hello rugalmas lekérdezési tooconnect tooyour távoli adatbázisok hello hitelesítő adatot használja.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Győződjön meg arról, hogy hello `<username>` nem vonatkozik a **"@servername"** utótag. 
>

## <a name="create-external-data-sources"></a>Külső adatforrás létrehozása
Szintaxis:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> hello típusparaméter túl be kell állítani**RDBMS**. 
>

### <a name="example"></a>Példa
hello alábbi példa azt mutatja be a külső adatforrások hello hello CREATE utasítás használatát. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

aktuális külső adatforrások tooretrieve hello listája: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Külső táblák
Szintaxis:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Példa
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

hello a következő példa bemutatja, hogyan tooretrieve hello hello jelenlegi adatbázisból külső táblák listáját: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Megjegyzések
Rugalmas lekérdezési hello meglévő külső tábla szintaxis toodefine külső táblák külső adatforrások RDBMS típusú használó terjeszti ki. Egy külső tábla definícióját a vertikális particionálás hello a következő szempontokat ismerteti: 

* **Séma**: hello külső táblára vonatkozó DDL definiálja a séma, a lekérdezéseket használhat. a külső tábla definíciójában megadott hello séma kell toomatch hello séma hello hello távoli adatbázis hello tényleges adatokat tároló tábla. 
* **Távoli adatbázis hivatkozás**: hello külső DDL a táblázat tooan külső adatforráshoz. hello külső adatforrás hello logikai kiszolgáló nevét és a hello távoli adatbázis hello tábla tényleges adatokat tároló adatbázis nevét adja meg. 

A külső hello előző szakaszokban foglaltaknak, hello szintaxis toocreate külső táblák használata az alábbiak szerint: 

hello DATA_SOURCE záradék hello külső adatforrás (azaz hello távoli adatbázis esetén a vertikális particionálás) hello külső tábla használt határozza meg.  

hello SCHEMA_NAME és OBJECT_NAME záradékok biztosítanak hello képességét toomap hello külső tábla definíciójának tooa tábla egy másik séma hello távoli adatbázis, vagy egy eltérő nevű tooa tábla kulcsattribútumokkal. Ez akkor hasznos, ha azt szeretné, toodefine egy külső tábla tooa katalógusnézet használatával derítheti ki vagy DMV a távoli adatbázis - vagy bármely más olyan esetben, ha hello távoli tábla neve már használatban van helyileg.  

hello következő DDL-utasítás elutasítja azokat egy meglévő külső tábla definíciójának hello helyi katalógus. Hello távoli adatbázis nem befolyásolja. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**A külső tábla létrehozása/DROP engedélyeinek**: ALTER ANY külső ADATFORRÁS engedélyekre van szükség, a külső tábla DDL, amely egyben szükséges toorefer toohello alapul szolgáló adatforrásban.  

## <a name="security-considerations"></a>Biztonsági szempontok
Felhasználók és a hozzáférés toohello külső tábla automatikusan hozzáférést toohello alapul szolgáló távoli táblák hello külső adatforrása definíciója megadott hello hitelesítő adatok alapján. Gondosan hozzáférés toohello külső tábla rendelés nemkívánatos tooavoid jogok kiterjesztése hello hello külső adatforrás hitelesítő adatai segítségével kezelje. Rendszeres SQL lehet engedélyeket használt tooGRANT vagy a REVOKE access tooan külső tábla ugyanúgy, mintha az lenne normál táblákhoz.  

## <a name="example-querying-vertically-partitioned-databases"></a>Példa: adatbázisok függőleges lekérdezése particionálva.
hello következő lekérdezés illesztést végez a háromutas hello két helyi táblázatokhoz rendelések és a sorok és hello távoli ügyfelek esetén. Ez az hello hivatkozás adatok használati eset rugalmas lekérdezési példát: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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
Használhat rendszeres SQL Server kapcsolati karakterláncok tooconnect a BI és az integráció eszközök toodatabases hello SQL-adatbázis a kiszolgálón, amelyen engedélyezve van a Rugalmas lekérdezési és definiált külső táblák. Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként. Ezután tekintse meg a toohello rugalmas lekérdezési adatbázis és a külső táblákra csakúgy, mint bármely más SQL Server-adatbázist, hogy Ön kapcsolódnának toowith az eszköz. 

## <a name="best-practices"></a>Ajánlott eljárások
* Győződjön meg arról, hogy hello rugalmas lekérdezési végpont az adatbázis adott adatbázist toohello távoli Azure-szolgáltatások hozzáférésének engedélyezéséhez az SQL-adatbázis a tűzfal-konfigurációjában. Bizonyosodjon meg arról, hogy hello hitelesítő adat hello külső adatok adatforrása definíciója sikeresen be tud jelentkezni a hello távoli adatbázis és a hello engedélyek tooaccess hello távoli táblájában.  
* Rugalmas lekérdezési működik a legjobban az lekérdezések ahol hello számítási többsége a távoli adatbázis hello végezhető. Általában legjobb lekérdezési teljesítményt hello szelektív szűrő predikátumok hello távoli adatbázisokhoz vagy végrehajtható teljesen hello távoli adatbázis illesztések kiértékelhető kap. Más lekérdezési mintáinak esetleg tooload nagy mennyiségű hello távoli adatbázis adatait, és rosszul hajthatja végre. 

## <a name="next-steps"></a>Következő lépések

* Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).
* Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).
* Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).
* A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)
* Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
