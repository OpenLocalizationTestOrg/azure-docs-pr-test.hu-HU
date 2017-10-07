---
title: "az SQL Data Warehouse aaaPartitioning táblák |} Microsoft Docs"
description: "Első lépések az Azure SQL Data Warehouse táblaparticionálást."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: aa63c51562f3e6f83063320860b195e135a721e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse Táblák particionálása
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Adattípusok][Data Types]
> * [Terjesztése][Distribute]
> * [Index][Index]
> * [Partíció][Partition]
> * [Statisztika][Statistics]
> * [Ideiglenes][Temporary]
> 
> 

Particionálás támogatott az SQL Data Warehouse tábla összes olyan; beleértve a fürtözött oszlopcentrikus, a fürtözött index és a halommemória.  Minden terjesztési típuson, beleértve a kivonatoló vagy a ciklikus multiplexelés elosztott a particionálás is támogatott.  Particionálás lehetővé teszi a dátum oszlop történik az adatok particionálása kisebb csoportokra és a legtöbb esetben az adatok toodivide.

## <a name="benefits-of-partitioning"></a>Particionálás előnyei
Particionálás adatok karbantartása és lekérdezéseivel kapcsolatos teljesítményt is kihasználhatja.  E előnyöket mindegyikét vagy csak egy függ, hogyan történik az adatok betöltése, és hogy hello ugyanarra az oszlopra használhatók mindkét célokra, mivel particionálás csak akkor lehet elvégezni egy oszlop.

### <a name="benefits-tooloads"></a>Előnyöket tooloads
hello elsődleges előnye, hogy az SQL Data Warehouse particionálás hello hatékonyságát és a partíció törlése használatával az adatok betöltéséhez, váltás és egyesítése teljesítményének van javítása.  A legtöbb esetben adatok particionálása napon oszloptól szorosan toohello feladatütemezési hello adatok betöltött toohello adatbázis társítva.  Az egyik legnagyobb előnyei hello a partíciók toomaintain adatokat a tranzakció naplózási elkerülő hello azt.  Egyszerűen beszúrni, frissítése vagy törlése az adatok lehetnek hello legegyszerűbb módszer, egy kis gondolat és erőfeszítést, használja a betöltési folyamat során particionálás jelentősen javíthatja a teljesítményt.

Partíció váltás használt tooquickly távolítsa el vagy cserélje le a szakasz egy tábla.  Például egy értékesítési ténytábla tartalmazhat csak az adatokat a hello 36 elmúlt hónap.  Minden hónap végén hello hello legrégebbi hónapja ábrázolhatja az értékesítési adatokat töröltek, amelyek hello tábla.  Ezek az adatok használatával egy utasítás toodelete hello adatok hello legrégebbi hónap sikerült törölni.  Azonban nagy mennyiségű adat--soronként egy delete utasítás törlése is nagyon hosszú időt vehet igénybe, valamint hello veszéllyel nagy tranzakciók, amelyek egy hosszú ideig toorollback is igénybe vehet, ha valamilyen hiba.  A több optimális megközelítés toosimply eldobási hello legrégebbi partíció adatok.  Ha hello egyedi sorok törléséhez órába is telhet, egy teljes partíció törlése sikerült másodpercre.

### <a name="benefits-tooqueries"></a>Előnyöket tooqueries
Particionálás használt tooimprove lekérdezési teljesítmény is lehet.  Ha a lekérdezés egy particionált oszlop szűrőt alkalmaz, ez korlátozhatja hello vizsgálat tooonly hello esetleg hello adatok, elkerülve a teljes táblázat vizsgálat sokkal kisebb részhalmazát partíciók jogosult.  A fürtözött oszlopcentrikus indexek hello bevezetése hello predikátum eltávolítási teljesítménybeli előnyökben kevesebb előnyös, de egyes esetekben lehet egy juttatás tooqueries.  Például ha hello értékesítési ténytábla hello értékesítési dátum mező 36 hónapokra particionálva van, majd hello pénztári dátum szűrő lekérdezéseket ugorjon hello szűrő nem egyező partíciók megkeresése.

## <a name="partition-sizing-guidance"></a>Partíció olvasható méretezési útmutató
Particionálás közben lehet használt tooimprove teljesítmény bizonyos esetekben a tábla létrehozása **túl sok** partíciók hátrányosan befolyásolhatja a teljesítményt bizonyos körülmények között.  A problémák különösen akkor igaz, a fürtözött oszloptárindexű táblákat.  Fontos toounderstand esetén particionálás toobe hasznos, ha toouse particionálás és hello száma partíciók toocreate.  Nincs toohow nem rögzített gyors szabályát sok partíció található, a túl sok, attól függ, az adatokat, és hány particionálja toosimultaneously tölt be.  Általános tapasztalatok, mint gondolja, hogy 10 egység összeadásának, de a partíciók, nem 1000 egység too100s.

A particionálás létrehozásakor **fürtözött oszlopcentrikus** táblák, akkor fontos, hogy hány sort megnyílik a minden partíció tooconsider.  Az optimális tömörítés és a fürtözött oszloptárindexű táblákat teljesítményétől legalább 1 millió sort foglalnak terjesztési és partíciónként szükséges.  Mielőtt partíciók jönnek létre, az SQL Data Warehouse már felosztja minden tábla 60 elosztott adatbázisok.  Bármely particionálási hozzáadott tooa tábla továbbá hello háttérben létrehozott toohello terjesztéseket.  Ebben a példában használata, ha hello értékesítési ténytábla 36 havi partíció található, és fényében, hogy az SQL Data Warehouse 60 azokat a terjesztéseket, majd hello értékesítési tény tábla kell tartalmaznia 60 millió sort foglalnak havonta vagy 2.1 egymilliárd sort, ha minden hónap fel van töltve.  Ha a tábla lényegesen kevesebb mint hello ajánlott minimális száma sorszám sorokat tartalmaz, érdemes kevesebb partíciók rendelés toomake növekedése hello kötegenkénti sorok száma partíció.  Lásd még: hello [indexelő] [ Index] cikket, amely tartalmazza az SQL Data Warehouse tooassess hello minőségének fürt oszlopcentrikus indexek futó lekérdezések.

## <a name="syntax-difference-from-sql-server"></a>Az SQL Server szintaktikai különbség
SQL Data Warehouse bevezeti a partíciók egy egyszerűsített definíciót, amely az SQL Server kis mértékben eltér.  Particionálási függvény és sémák nem használják az SQL Data Warehouse szerint az SQL Server.  Ehelyett toodo szüksége a particionált oszlop és hello határ pontok azonosításához.  Bár a particionálás hello szintaxisát némileg eltérő SQL Server, hello főbb fogalmait és kifejezéseit vannak hello azonos.  SQL Server és az SQL Data Warehouse támogatja táblánként, amely címkiosztási partíció lehet egy partícióoszlop.  toolearn particionálás, bővebben lásd: [particionált táblák és -indexek][Partitioned Tables and Indexes].

hello az alábbi példában egy SQL Data warehouse particionálva [CREATE TABLE] [ CREATE TABLE] utasítás, partíciók hello FactInternetSales táblának hello OrderDateKey oszlop alapján:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Az SQL Server particionálás áttelepítése
toomigrate SQL Server egyszerűen partícióazonosító definíciók tooSQL Data warehouse-bA:

* SQL Server hello kiküszöbölése [partícióséma][partition scheme].
* Adja hozzá a hello [partíciós függvényei] [ partition function] definition tooyour tábla létrehozása.

Ha egy SQL Server-példány hello alatt SQL telepít egy particionált táblához segítségével is szerepelnek, mindegyik partíció sorok számának toointerrogate hello.  Ne feledje, hogy ha hello olyan particionálási részletességgel használatban van az SQL Data Warehouse, hello partíciónként sorok száma csökken 60 tényezővel.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Terheléskezelés
Egy utolsó darabja szempont toofactor a toohello tábla partíciós döntési van [munkaterhelés felügyeleti][workload management].  Munkaterhelés-kezelés az SQL Data Warehouse, a memória és a párhuzamosság elsősorban hello kezelése.  Az SQL Data Warehouse hello tooeach terjesztési lekérdezés végrehajtása közben lefoglalt maximális memória szabályoz erőforrás osztály.  Ideális esetben a partíciók méretezi más tényezőktől, például a fürtözött oszlopcentrikus indexek felépítése hello memóriaigényét figyelembe véve.  Nagy mértékben fürtözött oszlopcentrikus indexek juttatásra, amikor azok több memóriát foglal le.  Ezért érdemes, amely egy partíció index újraépítése tooensure nem függeszteni memória. Más szerepkörök, például largerc hello memóriamennyiség elérhető tooyour lekérdezés elérhető átvált a hello alapértelmezett szerepkör, smallrc, a tooone növelése hello.

Hello kiosztását) eloszlása feladatonként (memória áll rendelkezésre információ hello erőforrás vezérlő dinamikus felügyeleti nézetekkel lekérdezésével. A valóságban a memóriabeli ideiglenes kisebb, mint az alábbi ábrán hello lesz. Azonban ez biztosít, amelyekkel a partíciók felügyeleti műveletek osztályozás útmutatás.  Próbálja meg a partíciók túl hello extra nagy erőforrásosztály által biztosított hello memóriaengedély méretezése tooavoid. Ha ez a szám nagyobb legyen a partíciók futtatnia Memóriaterhelést, ami viszont tooless optimális tömörítési hello kockázatát.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Váltás a partíció
Az SQL Data Warehouse a felosztás egyesítése és váltás partíció támogatja. Ezek segítségével hello excuted [az ALTER TABLE] [ ALTER TABLE] utasítást.

Győződjön meg arról, hogy hello partíciók igazodnak a megfelelő határokon belül, és, hogy megfelel-e hello definíciói két tábla között tooswitch partíciókat. Nem érhetők el ellenőrzési korlátozásokban, egy tábla hello forrástáblában értékek tartományán tooenforce hello tartalmaznia kell hello azonos határok partícióazonosító hello cél táblázatként. Ha ez nem hello helyzet, majd hello partíció kapcsolójának sikertelen lesz, mivel nem fognak szinkronizálódni hello partíciós metaadatok.

### <a name="how-toosplit-a-partition-that-contains-data"></a>Hogyan toosplit adatokat tartalmazza.
hello leghatékonyabb módszer toosplit már tartalmazza az adatokat az toouse egy `CTAS` utasítást. Ha hello particionált tábla fürtözött oszlopcentrikus majd hello tábla partíciós üresnek kell lennie ahhoz, lehessen.

Alább egy sor minden partíció tartalmazó minta particionált oszlopcentrikus tábla lesz:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> Creating hello statisztika objektum által jelenleg győződjön meg arról, hogy a tábla metaadatainak pontosabb. Jelenleg nincs megadva statisztikák létrehozása, ha az SQL Data Warehouse alapértelmezett értékeket fogja használni. A statisztika részletes tekintse át [statisztika][statistics].
> 
> 

Azt is majd kereshet hello sorok számát hello segítségével `sys.partitions` katalógus megtekintése:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Ha ez a táblázat azt próbálja toosplit, azt egy hiba jelenik meg:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Üzenet 35346, szint 15 állapot 1, az ALTER PARTITION utasítás sor 44 SPLIT záradékának sikertelen volt, mert hello partíció nem üres.  Csak üres partíciók oszthatók fel egy oszloptárindex hello tábla létezik. Vegye figyelembe, hogy hello oszloptárindex letiltását előtt hello az ALTER PARTITION utasítás kiadása, majd hello oszloptárindex újraépítését az ALTER PARTITION utasítás befejeződése után.

Azonban használhatjuk `CTAS` toocreate egy új tábla toohold az adatok.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Hello partícióhatárok vannak rendezve, mivel a kapcsoló megengedett. Így marad, hogy a forrástábla hello egy üres partícióból, azt később fel.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 too FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Toodo marad csak az adatok toohello új partíció határokat a tooalign `CTAS` és az adatok váltson vissza a toohello főtáblát

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 toodbo.FactInternetSales PARTITION 2;
```

Hello adatok mozgása hello befejezése után egy jó ötlet toorefresh hello statisztika a hello cél tábla tooensure pontosan tükrözik hello új terjesztési hello adatok a megfelelő partícióikba:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>A verziókövetési rendszerrel particionálás tábla
tooavoid a tábladefiníció a **Rozsdás** a forrásrendszerben vezérlő érdemes lehet a következő megközelítés tooconsider hello:

1. Hello tábla létrehozása egy particionált táblához megegyezik, azonban nincsenek partíció értékek

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

1. `SPLIT`hello tábla hello központi telepítési folyamat részeként:

```sql
-- Create a table containing hello partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over hello partition boundaries and split hello table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Ez a megközelítés hello a verziókövetési kód statikus marad, és a hello particionálási tartományhatár-értékek használata engedélyezett toobe dinamikus; fejlesztik hello adatraktár az idő múlásával.

## <a name="next-steps"></a>Következő lépések
toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla indexelő][Index], [fenntartása a tábla statisztikai adatainak] [ Statistics] és [ Az ideiglenes táblák][Temporary].  Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
