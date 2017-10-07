---
title: "az SQL Data Warehouse aaaTemporary táblák |} Microsoft Docs"
description: "Ismerkedés az Azure SQL Data Warehouse ideiglenes táblákat."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 2e8b122eb6d71d5bc0a99ce8a2ecab5dbe2d1b49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse az ideiglenes táblák
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

Az ideiglenes táblák nagyon hasznosak, ahol hello köztes eredmények átmeneti jellegűek átalakítása során különösen - adatok feldolgozásakor. Az SQL Data Warehouse az ideiglenes táblák hello munkamenet szinten található.  Csak látható toohello munkamenet, amelyben hozta létre, és automatikusan eldobott munkamenetben kijelentkezésekor.  Az ideiglenes táblák teljesítmény előnyt kínálnak, mert az eredményeiket írt távtároló helyett toolocal.  Az ideiglenes táblák azok elérhetők tetszőleges belül hello munkamenet, beleértve a belüli és kívüli tárolt eljárás amelyeket kis mértékben eltér az Azure SQL Data Warehouse az Azure SQL Database.

Ez a cikk alapvető útmutatást nyújt az ideiglenes táblák használata, és kiemeli a munkamenet szintű ideiglenes táblák hello elveit. Hello információk alapján ez a cikk segítséget nyújt a kódot, újrahasznosításának és a karbantartás a kód egyszerű javítása modularize.

## <a name="create-a-temporary-table"></a>Hozzon létre egy ideiglenes táblát
Ideiglenes táblázatok jönnek létre a tábla nevű egyszerűen illesztésével egy `#`.  Példa:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Az ideiglenes táblák is hozhatja létre a `CTAS` használatával pontosan ugyanezt a megközelítést hello:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS`egy nagyon hatékony parancs, és hozzáadta hello előnye, hogy nagyon hatékony tranzakciónaplók helyhasználatáról használata során. 
> 
> 

## <a name="dropping-temporary-tables"></a>Ideiglenes táblák
Amikor egy új munkamenetet hoz létre, nem ideiglenes táblák léteznie kell.  Azonban hívásakor hello azonos tárolt eljárást, amely hoz létre egy ideiglenes hello ugyanazzal a névvel, tooensure, amely a `CREATE TABLE` kimutatásai sikeres egy egyszerű előtti meglétének ellenőrzése a egy `DROP` is használható, ahogy az alábbi példában hello:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

A kódolási konzisztencia, esetén jó gyakorlat toouse ebben a mintában a táblák és a ideiglenes táblákra.  Egyúttal egy jó ötlet toouse `DROP TABLE` tooremove ideiglenes táblák befejezése után, a kódban.  A tárolt eljárás fejlesztési elég általános toosee hello drop utasítást egyetlen kötegbe foglalnak egy eljárás tooensure hello végén ezek az objektumok megtisztítva.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kód
Az ideiglenes táblák felhasználói munkamenet bármely részén látható, mivel Ön az alkalmazás kódjában modularize kihasznált toohelp is lehet.  Például hello tárolt eljárás az alábbi összegyűjti az ajánlott eljárások promptjai toogenerate DDL, amely frissíti az összes statisztika hello adatbázis statisztika név szerint hello.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Ebben a szakaszban hello egyetlen művelet történt hello létrehozása egy tárolt eljárás, amely a rendszer egyszerűen csak generált ideiglenes táblából, #stats_ddl a DDL-utasításokban.  Ez a tárolt eljárás eldobja #stats_ddl, ha nem sikerül a Ha-munkameneten belül csak egyszer futtatnia tooensure már létezik.  Azonban mivel nincs `DROP TABLE` hello tárolt eljárás hello végén hello tárolt eljárás befejezése után bízza hello létrehozott tábla így hello tárolt eljárás kívül tudja olvasni.  Az SQL Data Warehouse szemben a többi SQL Server-adatbázis is lehetséges toouse hello ideiglenes tábla kívül hello eljárás, amely létrehozta.  Az SQL Data Warehouse az ideiglenes táblák használható **bárhol** hello munkamenet belül. Ennek eredményeképpen előfordulhat, ahogy az alábbi példában hello moduláris és kezelhető is legyen kód toomore:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Ideiglenes tábla korlátozásai
Az SQL Data Warehouse ugyanazok a korlátozások néhány, az ideiglenes táblák végrehajtása során.  Jelenleg csak a munkamenet hatókörű ideiglenes táblák támogatottak.  Globális ideiglenes táblák nem támogatottak.  Emellett nézetek nem hozható létre ideiglenes táblákra.

## <a name="next-steps"></a>Következő lépések
toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla indexelő][Index], [tábla particionáló] [ Partition] és [ Tábla statisztikai adatainak karbantartása][Statistics].  Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
