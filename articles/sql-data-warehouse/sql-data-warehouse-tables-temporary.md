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
# <a name="temporary-tables-in-sql-data-warehouse"></a><span data-ttu-id="2fc37-103">Az SQL Data Warehouse az ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="2fc37-103">Temporary tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="2fc37-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="2fc37-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="2fc37-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="2fc37-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="2fc37-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="2fc37-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="2fc37-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="2fc37-107">[Index][Index]</span></span>
> * <span data-ttu-id="2fc37-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="2fc37-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="2fc37-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="2fc37-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="2fc37-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="2fc37-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="2fc37-111">Az ideiglenes táblák nagyon hasznosak, ahol hello köztes eredmények átmeneti jellegűek átalakítása során különösen - adatok feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="2fc37-111">Temporary tables are very useful when processing data - especially during transformation where hello intermediate results are transient.</span></span> <span data-ttu-id="2fc37-112">Az SQL Data Warehouse az ideiglenes táblák hello munkamenet szinten található.</span><span class="sxs-lookup"><span data-stu-id="2fc37-112">In SQL Data Warehouse temporary tables exist at hello session level.</span></span>  <span data-ttu-id="2fc37-113">Csak látható toohello munkamenet, amelyben hozta létre, és automatikusan eldobott munkamenetben kijelentkezésekor.</span><span class="sxs-lookup"><span data-stu-id="2fc37-113">They are only visible toohello session in which they were created and are automatically dropped when that session logs off.</span></span>  <span data-ttu-id="2fc37-114">Az ideiglenes táblák teljesítmény előnyt kínálnak, mert az eredményeiket írt távtároló helyett toolocal.</span><span class="sxs-lookup"><span data-stu-id="2fc37-114">Temporary tables offer a performance benefit because their results are written toolocal rather than remote storage.</span></span>  <span data-ttu-id="2fc37-115">Az ideiglenes táblák azok elérhetők tetszőleges belül hello munkamenet, beleértve a belüli és kívüli tárolt eljárás amelyeket kis mértékben eltér az Azure SQL Data Warehouse az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2fc37-115">Temporary tables are slightly different in Azure SQL Data Warehouse than Azure SQL Database as they can be accessed from anywhere inside hello session, including both inside and outside of a stored procedure.</span></span>

<span data-ttu-id="2fc37-116">Ez a cikk alapvető útmutatást nyújt az ideiglenes táblák használata, és kiemeli a munkamenet szintű ideiglenes táblák hello elveit.</span><span class="sxs-lookup"><span data-stu-id="2fc37-116">This article contains essential guidance for using temporary tables and highlights hello principles of session level temporary tables.</span></span> <span data-ttu-id="2fc37-117">Hello információk alapján ez a cikk segítséget nyújt a kódot, újrahasznosításának és a karbantartás a kód egyszerű javítása modularize.</span><span class="sxs-lookup"><span data-stu-id="2fc37-117">Using hello information in this article can help you modularize your code, improving both reusability and ease of maintenance of your code.</span></span>

## <a name="create-a-temporary-table"></a><span data-ttu-id="2fc37-118">Hozzon létre egy ideiglenes táblát</span><span class="sxs-lookup"><span data-stu-id="2fc37-118">Create a temporary table</span></span>
<span data-ttu-id="2fc37-119">Ideiglenes táblázatok jönnek létre a tábla nevű egyszerűen illesztésével egy `#`.</span><span class="sxs-lookup"><span data-stu-id="2fc37-119">Temporary tables are created by simply prefixing your table name with a `#`.</span></span>  <span data-ttu-id="2fc37-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="2fc37-120">For example:</span></span>

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

<span data-ttu-id="2fc37-121">Az ideiglenes táblák is hozhatja létre a `CTAS` használatával pontosan ugyanezt a megközelítést hello:</span><span class="sxs-lookup"><span data-stu-id="2fc37-121">Temporary tables can also be created with a `CTAS` using exactly hello same approach:</span></span>

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
> <span data-ttu-id="2fc37-122">`CTAS`egy nagyon hatékony parancs, és hozzáadta hello előnye, hogy nagyon hatékony tranzakciónaplók helyhasználatáról használata során.</span><span class="sxs-lookup"><span data-stu-id="2fc37-122">`CTAS` is a very powerful command and has hello added advantage of being very efficient in its use of transaction log space.</span></span> 
> 
> 

## <a name="dropping-temporary-tables"></a><span data-ttu-id="2fc37-123">Ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="2fc37-123">Dropping temporary tables</span></span>
<span data-ttu-id="2fc37-124">Amikor egy új munkamenetet hoz létre, nem ideiglenes táblák léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="2fc37-124">When a new session is created, no temporary tables should exist.</span></span>  <span data-ttu-id="2fc37-125">Azonban hívásakor hello azonos tárolt eljárást, amely hoz létre egy ideiglenes hello ugyanazzal a névvel, tooensure, amely a `CREATE TABLE` kimutatásai sikeres egy egyszerű előtti meglétének ellenőrzése a egy `DROP` is használható, ahogy az alábbi példában hello:</span><span class="sxs-lookup"><span data-stu-id="2fc37-125">However, if you are calling hello same stored procedure, which creates a temporary with hello same name, tooensure that your `CREATE TABLE` statements are successful a simple pre-existence check with a `DROP` can be used as in hello below example:</span></span>

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

<span data-ttu-id="2fc37-126">A kódolási konzisztencia, esetén jó gyakorlat toouse ebben a mintában a táblák és a ideiglenes táblákra.</span><span class="sxs-lookup"><span data-stu-id="2fc37-126">For coding consistency, it is a good practice toouse this pattern for both tables and temporary tables.</span></span>  <span data-ttu-id="2fc37-127">Egyúttal egy jó ötlet toouse `DROP TABLE` tooremove ideiglenes táblák befejezése után, a kódban.</span><span class="sxs-lookup"><span data-stu-id="2fc37-127">It is also a good idea toouse `DROP TABLE` tooremove temporary tables when you have finished with them in your code.</span></span>  <span data-ttu-id="2fc37-128">A tárolt eljárás fejlesztési elég általános toosee hello drop utasítást egyetlen kötegbe foglalnak egy eljárás tooensure hello végén ezek az objektumok megtisztítva.</span><span class="sxs-lookup"><span data-stu-id="2fc37-128">In stored procedure development it is quite common toosee hello drop commands bundled together at hello end of a procedure tooensure these objects are cleaned up.</span></span>

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a><span data-ttu-id="2fc37-129">Modularizing kód</span><span class="sxs-lookup"><span data-stu-id="2fc37-129">Modularizing code</span></span>
<span data-ttu-id="2fc37-130">Az ideiglenes táblák felhasználói munkamenet bármely részén látható, mivel Ön az alkalmazás kódjában modularize kihasznált toohelp is lehet.</span><span class="sxs-lookup"><span data-stu-id="2fc37-130">Since temporary tables can be seen anywhere in a user session, this can be exploited toohelp you modularize your application code.</span></span>  <span data-ttu-id="2fc37-131">Például hello tárolt eljárás az alábbi összegyűjti az ajánlott eljárások promptjai toogenerate DDL, amely frissíti az összes statisztika hello adatbázis statisztika név szerint hello.</span><span class="sxs-lookup"><span data-stu-id="2fc37-131">For example, hello stored procedure below brings together hello recommended practices from above toogenerate DDL which will update all statistics in hello database by statistic name.</span></span>

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

<span data-ttu-id="2fc37-132">Ebben a szakaszban hello egyetlen művelet történt hello létrehozása egy tárolt eljárás, amely a rendszer egyszerűen csak generált ideiglenes táblából, #stats_ddl a DDL-utasításokban.</span><span class="sxs-lookup"><span data-stu-id="2fc37-132">At this stage hello only action that has occurred is hello creation of a stored procedure which will simply generated a temporary table, #stats_ddl, with DDL statements.</span></span>  <span data-ttu-id="2fc37-133">Ez a tárolt eljárás eldobja #stats_ddl, ha nem sikerül a Ha-munkameneten belül csak egyszer futtatnia tooensure már létezik.</span><span class="sxs-lookup"><span data-stu-id="2fc37-133">This stored procedure will drop #stats_ddl if it already exists tooensure it does not fail if run more than once within a session.</span></span>  <span data-ttu-id="2fc37-134">Azonban mivel nincs `DROP TABLE` hello tárolt eljárás hello végén hello tárolt eljárás befejezése után bízza hello létrehozott tábla így hello tárolt eljárás kívül tudja olvasni.</span><span class="sxs-lookup"><span data-stu-id="2fc37-134">However, since there is no `DROP TABLE` at hello end of hello stored procedure, when hello stored procedure completes, it will leave hello created table so that it can be read outside of hello stored procedure.</span></span>  <span data-ttu-id="2fc37-135">Az SQL Data Warehouse szemben a többi SQL Server-adatbázis is lehetséges toouse hello ideiglenes tábla kívül hello eljárás, amely létrehozta.</span><span class="sxs-lookup"><span data-stu-id="2fc37-135">In SQL Data Warehouse, unlike other SQL Server databases, it is possible toouse hello temporary table outside of hello procedure that created it.</span></span>  <span data-ttu-id="2fc37-136">Az SQL Data Warehouse az ideiglenes táblák használható **bárhol** hello munkamenet belül.</span><span class="sxs-lookup"><span data-stu-id="2fc37-136">SQL Data Warehouse temporary tables can be used **anywhere** inside hello session.</span></span> <span data-ttu-id="2fc37-137">Ennek eredményeképpen előfordulhat, ahogy az alábbi példában hello moduláris és kezelhető is legyen kód toomore:</span><span class="sxs-lookup"><span data-stu-id="2fc37-137">This can lead toomore modular and manageable code as in hello below example:</span></span>

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

## <a name="temporary-table-limitations"></a><span data-ttu-id="2fc37-138">Ideiglenes tábla korlátozásai</span><span class="sxs-lookup"><span data-stu-id="2fc37-138">Temporary table limitations</span></span>
<span data-ttu-id="2fc37-139">Az SQL Data Warehouse ugyanazok a korlátozások néhány, az ideiglenes táblák végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="2fc37-139">SQL Data Warehouse does impose a couple of limitations when implementing temporary tables.</span></span>  <span data-ttu-id="2fc37-140">Jelenleg csak a munkamenet hatókörű ideiglenes táblák támogatottak.</span><span class="sxs-lookup"><span data-stu-id="2fc37-140">Currently, only session scoped temporary tables are supported.</span></span>  <span data-ttu-id="2fc37-141">Globális ideiglenes táblák nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="2fc37-141">Global Temporary Tables are not supported.</span></span>  <span data-ttu-id="2fc37-142">Emellett nézetek nem hozható létre ideiglenes táblákra.</span><span class="sxs-lookup"><span data-stu-id="2fc37-142">In addition, views cannot be created on temporary tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2fc37-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2fc37-143">Next steps</span></span>
<span data-ttu-id="2fc37-144">toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla indexelő][Index], [tábla particionáló] [ Partition] és [ Tábla statisztikai adatainak karbantartása][Statistics].</span><span class="sxs-lookup"><span data-stu-id="2fc37-144">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Maintaining Table Statistics][Statistics].</span></span>  <span data-ttu-id="2fc37-145">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="2fc37-145">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
