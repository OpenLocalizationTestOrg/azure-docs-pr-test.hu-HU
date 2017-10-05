---
title: "Az SQL Data Warehouse az ideiglenes táblák |} Microsoft Docs"
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
ms.openlocfilehash: fd8c31a727dae3b011aa8294a81f005bad72a278
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a><span data-ttu-id="daeca-103">Az SQL Data Warehouse az ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="daeca-103">Temporary tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="daeca-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="daeca-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="daeca-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="daeca-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="daeca-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="daeca-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="daeca-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="daeca-107">[Index][Index]</span></span>
> * <span data-ttu-id="daeca-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="daeca-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="daeca-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="daeca-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="daeca-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="daeca-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="daeca-111">Az ideiglenes táblák nagyon hasznosak, ha az adatfeldolgozás - különösen ha a köztes eredmények átmeneti jellegűek átalakítása során.</span><span class="sxs-lookup"><span data-stu-id="daeca-111">Temporary tables are very useful when processing data - especially during transformation where the intermediate results are transient.</span></span> <span data-ttu-id="daeca-112">Az SQL Data Warehouse ideiglenes táblák léteznek, a munkamenet szintű.</span><span class="sxs-lookup"><span data-stu-id="daeca-112">In SQL Data Warehouse temporary tables exist at the session level.</span></span>  <span data-ttu-id="daeca-113">Csak azok a munkamenethez, amelyben hozta létre, és automatikusan eldobott munkamenetben kijelentkezésekor látható.</span><span class="sxs-lookup"><span data-stu-id="daeca-113">They are only visible to the session in which they were created and are automatically dropped when that session logs off.</span></span>  <span data-ttu-id="daeca-114">Az ideiglenes táblák teljesítmény előnyt kínálnak, mert az eredményeiket írt helyi ahelyett, hogy a távoli tároló.</span><span class="sxs-lookup"><span data-stu-id="daeca-114">Temporary tables offer a performance benefit because their results are written to local rather than remote storage.</span></span>  <span data-ttu-id="daeca-115">Az ideiglenes táblák azok is bárhonnan elérhetők a munkamenetben, beleértve belül és kívül tárolt eljárás belül amelyeket kis mértékben eltér az Azure SQL Data Warehouse az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="daeca-115">Temporary tables are slightly different in Azure SQL Data Warehouse than Azure SQL Database as they can be accessed from anywhere inside the session, including both inside and outside of a stored procedure.</span></span>

<span data-ttu-id="daeca-116">Ez a cikk alapvető útmutatást nyújt az ideiglenes táblák használata, és kiemeli a munkamenet szintű ideiglenes táblák ezeket az alapelveket.</span><span class="sxs-lookup"><span data-stu-id="daeca-116">This article contains essential guidance for using temporary tables and highlights the principles of session level temporary tables.</span></span> <span data-ttu-id="daeca-117">Ebben a cikkben szereplő információk segítségével segítséget nyújt a kódot, újrahasznosításának és a karbantartás a kód egyszerű javítása modularize.</span><span class="sxs-lookup"><span data-stu-id="daeca-117">Using the information in this article can help you modularize your code, improving both reusability and ease of maintenance of your code.</span></span>

## <a name="create-a-temporary-table"></a><span data-ttu-id="daeca-118">Hozzon létre egy ideiglenes táblát</span><span class="sxs-lookup"><span data-stu-id="daeca-118">Create a temporary table</span></span>
<span data-ttu-id="daeca-119">Ideiglenes táblázatok jönnek létre a tábla nevű egyszerűen illesztésével egy `#`.</span><span class="sxs-lookup"><span data-stu-id="daeca-119">Temporary tables are created by simply prefixing your table name with a `#`.</span></span>  <span data-ttu-id="daeca-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="daeca-120">For example:</span></span>

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

<span data-ttu-id="daeca-121">Az ideiglenes táblák is hozhatja létre a `CTAS` pontosan ugyanezt a megközelítést használ:</span><span class="sxs-lookup"><span data-stu-id="daeca-121">Temporary tables can also be created with a `CTAS` using exactly the same approach:</span></span>

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
> <span data-ttu-id="daeca-122">`CTAS`egy nagyon hatékony parancs és a hozzáadott előnye, hogy nagyon hatékony tranzakciónaplók helyhasználatáról használata során.</span><span class="sxs-lookup"><span data-stu-id="daeca-122">`CTAS` is a very powerful command and has the added advantage of being very efficient in its use of transaction log space.</span></span> 
> 
> 

## <a name="dropping-temporary-tables"></a><span data-ttu-id="daeca-123">Ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="daeca-123">Dropping temporary tables</span></span>
<span data-ttu-id="daeca-124">Amikor egy új munkamenetet hoz létre, nem ideiglenes táblák léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="daeca-124">When a new session is created, no temporary tables should exist.</span></span>  <span data-ttu-id="daeca-125">Azonban ha hívásakor az azonos tárolt eljárás, amely létrehoz egy ideiglenes ugyanazzal a névvel, annak érdekében, hogy a `CREATE TABLE` kimutatásai sikeres egy egyszerű előtti meglétének ellenőrzése a egy `DROP` is használható, mint a az alábbi példában:</span><span class="sxs-lookup"><span data-stu-id="daeca-125">However, if you are calling the same stored procedure, which creates a temporary with the same name, to ensure that your `CREATE TABLE` statements are successful a simple pre-existence check with a `DROP` can be used as in the below example:</span></span>

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

<span data-ttu-id="daeca-126">A kódolási konzisztencia, célszerű használni ezt a mintát táblák és az ideiglenes táblák esetén.</span><span class="sxs-lookup"><span data-stu-id="daeca-126">For coding consistency, it is a good practice to use this pattern for both tables and temporary tables.</span></span>  <span data-ttu-id="daeca-127">Akkor is érdemes használni `DROP TABLE` eltávolítása az ideiglenes táblák befejezése után, a kódban.</span><span class="sxs-lookup"><span data-stu-id="daeca-127">It is also a good idea to use `DROP TABLE` to remove temporary tables when you have finished with them in your code.</span></span>  <span data-ttu-id="daeca-128">A tárolt eljárás megtisztítva fejlesztési elég általános a drop utasítást egybe kötegelik ahhoz, hogy ezek az objektumok eljárás végén megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="daeca-128">In stored procedure development it is quite common to see the drop commands bundled together at the end of a procedure to ensure these objects are cleaned up.</span></span>

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a><span data-ttu-id="daeca-129">Modularizing kód</span><span class="sxs-lookup"><span data-stu-id="daeca-129">Modularizing code</span></span>
<span data-ttu-id="daeca-130">Az ideiglenes táblák felhasználói munkamenet bármely részén látható, mert ez az alkalmazás kódjában modularize segítséget is kihasználható.</span><span class="sxs-lookup"><span data-stu-id="daeca-130">Since temporary tables can be seen anywhere in a user session, this can be exploited to help you modularize your application code.</span></span>  <span data-ttu-id="daeca-131">Például az alábbi tárolt eljárás egyesíti az ajánlott eljárásokat, a fenti DDL, amely frissíti az adatbázis összes statisztika statisztika nevű létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="daeca-131">For example, the stored procedure below brings together the recommended practices from above to generate DDL which will update all statistics in the database by statistic name.</span></span>

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

<span data-ttu-id="daeca-132">Ebben a szakaszban az egyetlen művelet történt feladata a tárolt eljárás, amely egyszerűen fog generált ideiglenes táblából, #stats_ddl a DDL-utasításokban.</span><span class="sxs-lookup"><span data-stu-id="daeca-132">At this stage the only action that has occurred is the creation of a stored procedure which will simply generated a temporary table, #stats_ddl, with DDL statements.</span></span>  <span data-ttu-id="daeca-133">Ez a tárolt eljárás eldobja #stats_ddl, ha már létezik annak érdekében, hogy nem sikerül a Ha-munkameneten belül csak egyszer futtatnia.</span><span class="sxs-lookup"><span data-stu-id="daeca-133">This stored procedure will drop #stats_ddl if it already exists to ensure it does not fail if run more than once within a session.</span></span>  <span data-ttu-id="daeca-134">Azonban mivel nincs `DROP TABLE` a tárolt eljárás végén a tárolt eljárás befejeződésekor bízza a létrehozott tábla, hogy a tárolt eljárás kívül tudja olvasni.</span><span class="sxs-lookup"><span data-stu-id="daeca-134">However, since there is no `DROP TABLE` at the end of the stored procedure, when the stored procedure completes, it will leave the created table so that it can be read outside of the stored procedure.</span></span>  <span data-ttu-id="daeca-135">Az SQL Data Warehouse szemben a többi SQL Server-adatbázis is lehet az átmeneti táblázat kívül az eljárást, amely létrehozta.</span><span class="sxs-lookup"><span data-stu-id="daeca-135">In SQL Data Warehouse, unlike other SQL Server databases, it is possible to use the temporary table outside of the procedure that created it.</span></span>  <span data-ttu-id="daeca-136">Az SQL Data Warehouse az ideiglenes táblák használható **bárhol** a munkamenet belül.</span><span class="sxs-lookup"><span data-stu-id="daeca-136">SQL Data Warehouse temporary tables can be used **anywhere** inside the session.</span></span> <span data-ttu-id="daeca-137">Ez további moduláris és kezelhető is legyen kódot, mint a vezethet az alábbi példában:</span><span class="sxs-lookup"><span data-stu-id="daeca-137">This can lead to more modular and manageable code as in the below example:</span></span>

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

## <a name="temporary-table-limitations"></a><span data-ttu-id="daeca-138">Ideiglenes tábla korlátozásai</span><span class="sxs-lookup"><span data-stu-id="daeca-138">Temporary table limitations</span></span>
<span data-ttu-id="daeca-139">Az SQL Data Warehouse ugyanazok a korlátozások néhány, az ideiglenes táblák végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="daeca-139">SQL Data Warehouse does impose a couple of limitations when implementing temporary tables.</span></span>  <span data-ttu-id="daeca-140">Jelenleg csak a munkamenet hatókörű ideiglenes táblák támogatottak.</span><span class="sxs-lookup"><span data-stu-id="daeca-140">Currently, only session scoped temporary tables are supported.</span></span>  <span data-ttu-id="daeca-141">Globális ideiglenes táblák nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="daeca-141">Global Temporary Tables are not supported.</span></span>  <span data-ttu-id="daeca-142">Emellett nézetek nem hozható létre ideiglenes táblákra.</span><span class="sxs-lookup"><span data-stu-id="daeca-142">In addition, views cannot be created on temporary tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="daeca-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="daeca-143">Next steps</span></span>
<span data-ttu-id="daeca-144">További tudnivalókért tekintse meg a cikkek [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [terjesztése egy tábla] [ Distribute], [Tábla indexelő][Index], [tábla particionáló] [ Partition] és [Fenntartása a tábla statisztikai adatainak][Statistics].</span><span class="sxs-lookup"><span data-stu-id="daeca-144">To learn more, see the articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Maintaining Table Statistics][Statistics].</span></span>  <span data-ttu-id="daeca-145">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="daeca-145">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
