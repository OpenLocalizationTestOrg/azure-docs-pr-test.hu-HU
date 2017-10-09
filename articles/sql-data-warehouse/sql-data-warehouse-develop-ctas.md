---
title: "Válasszon ki (CTAS) az SQL Data Warehouse aaaCreate tábla |} Microsoft Docs"
description: "Tippek a hello kódolási (CTAS) utasítás válasszon ki az Azure SQL Data Warehouse adattárházzal történő, megoldások tábla létrehozása."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 68ac9a94-09f9-424b-b536-06a125a653bd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 01/30/2017
ms.author: shigu;barbkess
ms.openlocfilehash: e381601a0a4d94e189d8f9115bf2e7593025410b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a><span data-ttu-id="befe1-103">Select (CTAS) tábla az SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="befe1-103">Create Table As Select (CTAS) in SQL Data Warehouse</span></span>
<span data-ttu-id="befe1-104">Válassza ki a táblázatban létrehozása vagy `CTAS` az hello egyik legfontosabb T-SQL funkciókat érhető el.</span><span class="sxs-lookup"><span data-stu-id="befe1-104">Create table as select or `CTAS` is one of hello most important T-SQL features available.</span></span> <span data-ttu-id="befe1-105">Egy teljes mértékben parallelized művelet, amely táblát hoz létre új hello kimeneti SELECT utasítás alapján.</span><span class="sxs-lookup"><span data-stu-id="befe1-105">It is a fully parallelized operation that creates a new table based on hello output of a SELECT statement.</span></span> <span data-ttu-id="befe1-106">`CTAS`hello legegyszerűbb módja toocreate van egy tábla egy példányát.</span><span class="sxs-lookup"><span data-stu-id="befe1-106">`CTAS` is hello simplest and fastest way toocreate a copy of a table.</span></span> <span data-ttu-id="befe1-107">Ez a dokumentum nyújt, példák és ajánlott eljárásai `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="befe1-107">This document provides both examples and best practices for `CTAS`.</span></span>

## <a name="selectinto-vs-ctas"></a><span data-ttu-id="befe1-108">VÁLASSZON... A Visual Studio. CTAS</span><span class="sxs-lookup"><span data-stu-id="befe1-108">SELECT..INTO vs. CTAS</span></span>
<span data-ttu-id="befe1-109">Érdemes lehet `CTAS` felső szinten megterhelni változata `SELECT..INTO`.</span><span class="sxs-lookup"><span data-stu-id="befe1-109">You can consider `CTAS` as a super-charged version of `SELECT..INTO`.</span></span>

<span data-ttu-id="befe1-110">Az alábbiakban látható egy példa egy egyszerű `SELECT..INTO` utasítást:</span><span class="sxs-lookup"><span data-stu-id="befe1-110">Below is an example of a simple `SELECT..INTO` statement:</span></span>

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

<span data-ttu-id="befe1-111">A fenti példában hello `[dbo].[FactInternetSales_new]` , ezek a hello tábla alapértelmezett beállításokat az Azure SQL Data Warehouse rajta a FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXSZEL rendelkező ROUND_ROBIN elosztott táblázatként létrehozott.</span><span class="sxs-lookup"><span data-stu-id="befe1-111">In hello example above `[dbo].[FactInternetSales_new]` would be created as ROUND_ROBIN distributed table with a CLUSTERED COLUMNSTORE INDEX on it as these are hello table defaults in Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="befe1-112">`SELECT..INTO`azonban nem teszi lehetővé toochange hello terjesztési metódus vagy hello index írja be a hello művelet részeként.</span><span class="sxs-lookup"><span data-stu-id="befe1-112">`SELECT..INTO` however does not allow you toochange either hello distribution method or hello index type as part of hello operation.</span></span> <span data-ttu-id="befe1-113">Ez akkor, ha `CTAS` érkezik.</span><span class="sxs-lookup"><span data-stu-id="befe1-113">This is where `CTAS` comes in.</span></span>

<span data-ttu-id="befe1-114">tooconvert túl hello fent`CTAS` meglehetősen egyszerű feladat:</span><span class="sxs-lookup"><span data-stu-id="befe1-114">tooconvert hello above too`CTAS` is quite straight-forward:</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<span data-ttu-id="befe1-115">A `CTAS` képes toochange áll mindkét hello terjesztési hello tábla adatai, valamint a hello táblatípus.</span><span class="sxs-lookup"><span data-stu-id="befe1-115">With `CTAS` you are able toochange both hello distribution of hello table data as well as hello table type.</span></span> 

> [!NOTE]
> <span data-ttu-id="befe1-116">Ha csak a toochange hello indexe a `CTAS` művelet és hello forrástábla elosztott kivonatoló, majd a `CTAS` műveletet hajt végre legjobb, ha a hello azonos terjesztési oszlop és adatok típusát.</span><span class="sxs-lookup"><span data-stu-id="befe1-116">If you are only trying toochange hello index in your `CTAS` operation and hello source table is hash distributed then your `CTAS` operation will perform best if you maintain hello same distribution column and data type.</span></span> <span data-ttu-id="befe1-117">Így elkerülhető, terjesztési adatmozgás közötti hatékonyabb hello művelet során.</span><span class="sxs-lookup"><span data-stu-id="befe1-117">This will avoid cross distribution data movement during hello operation which is more efficient.</span></span>
> 
> 

## <a name="using-ctas-toocopy-a-table"></a><span data-ttu-id="befe1-118">Egy tábla CTAS toocopy használatával</span><span class="sxs-lookup"><span data-stu-id="befe1-118">Using CTAS toocopy a table</span></span>
<span data-ttu-id="befe1-119">Lehet, hogy egyik leggyakoribb hello használja a `CTAS` hoz létre egy tábla egy példányát, hogy hello DDL bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="befe1-119">Perhaps one of hello most common uses of `CTAS` is creating a copy of a table so that you can change hello DDL.</span></span> <span data-ttu-id="befe1-120">Ha például eredetileg létrehozott a táblázatban `ROUND_ROBIN` és most kívánja megváltoztatni a tooa táblázat egy olyan oszloptól, terjesztés `CTAS` hogyan megváltozna hello terjesztési oszlop van.</span><span class="sxs-lookup"><span data-stu-id="befe1-120">If for example you originally created your table as `ROUND_ROBIN` and now want change it tooa table distributed on a column, `CTAS` is how you would change hello distribution column.</span></span> <span data-ttu-id="befe1-121">`CTAS`akkor is használt toochange particionálás, indexelő vagy oszlop.</span><span class="sxs-lookup"><span data-stu-id="befe1-121">`CTAS` can also be used toochange partitioning, indexing, or column types.</span></span>

<span data-ttu-id="befe1-122">Tegyük fel, ez a táblázat hello alapértelmezett eloszlási típusát használatával létrehozott `ROUND_ROBIN` elosztott, mivel nincs terjesztési oszlop van megadva a hello `CREATE TABLE`.</span><span class="sxs-lookup"><span data-stu-id="befe1-122">Let's say you created this table using hello default distribution type of `ROUND_ROBIN` distributed since no distribution column was specified in hello `CREATE TABLE`.</span></span>

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

<span data-ttu-id="befe1-123">Most szeretné ezt a táblázatot a fürtözött Oszlopcentrikus indexszel rendelkező új toocreate, hogy a fürtözött Oszloptárindexű táblákat hello teljesítményének előnyeit élvezheti.</span><span class="sxs-lookup"><span data-stu-id="befe1-123">Now you want toocreate a new copy of this table with a Clustered Columnstore Index so that you can take advantage of hello performance of Clustered Columnstore tables.</span></span> <span data-ttu-id="befe1-124">Szeretné toodistribute ezt a táblázatot a ProductKey vannak előrejelző illesztések ettől az oszloptól, és szeretné, hogy tooavoid adatátvitel során a ProductKey illesztések óta.</span><span class="sxs-lookup"><span data-stu-id="befe1-124">You also want toodistribute this table on ProductKey since you are anticipating joins on this column and want tooavoid data movement during joins on ProductKey.</span></span> <span data-ttu-id="befe1-125">Végül is érdemes tooadd particionálás OrderDateKey az, hogy a régi adatok régi partíciók ejtésével gyorsan törölhetők.</span><span class="sxs-lookup"><span data-stu-id="befe1-125">Lastly you also want tooadd partitioning on OrderDateKey so that you can quickly delete old data by dropping old partitions.</span></span> <span data-ttu-id="befe1-126">Itt található hello CTAS kimutatásban, amely a régi tábla szeretné másolni egy új táblába.</span><span class="sxs-lookup"><span data-stu-id="befe1-126">Here is hello CTAS statement which would copy your old table into a new table.</span></span>

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

<span data-ttu-id="befe1-127">Végül az új tábla nevezze át a táblák tooswap és majd dobja el a régi táblát.</span><span class="sxs-lookup"><span data-stu-id="befe1-127">Finally you can rename your tables tooswap in your new table and then drop your old table.</span></span>

```sql
RENAME OBJECT FactInternetSales tooFactInternetSales_old;
RENAME OBJECT FactInternetSales_new tooFactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> <span data-ttu-id="befe1-128">Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.</span><span class="sxs-lookup"><span data-stu-id="befe1-128">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="befe1-129">A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő.</span><span class="sxs-lookup"><span data-stu-id="befe1-129">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="befe1-130">A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében.</span><span class="sxs-lookup"><span data-stu-id="befe1-130">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span>
> 
> 

## <a name="using-ctas-toowork-around-unsupported-features"></a><span data-ttu-id="befe1-131">Nem támogatott szolgáltatások köré CTAS toowork használatával</span><span class="sxs-lookup"><span data-stu-id="befe1-131">Using CTAS toowork around unsupported features</span></span>
<span data-ttu-id="befe1-132">`CTAS`számos alább felsorolt nem támogatott hello szolgáltatást körül használt toowork is lehet.</span><span class="sxs-lookup"><span data-stu-id="befe1-132">`CTAS` can also be used toowork around a number of hello unsupported features listed below.</span></span> <span data-ttu-id="befe1-133">Ez gyakran bizonyítja toobe win/win helyzet nem csak a kód lesznek megfelelőek, de ez gyakran végrehajtja a gyorsabb SQL Data warehouse.</span><span class="sxs-lookup"><span data-stu-id="befe1-133">This can often prove toobe a win/win situation as not only will your code be compliant but it will often execute faster on SQL Data Warehouse.</span></span> <span data-ttu-id="befe1-134">Ez egy, a teljes parallelized kialakításakor miatt.</span><span class="sxs-lookup"><span data-stu-id="befe1-134">This is as a result of its fully parallelized design.</span></span> <span data-ttu-id="befe1-135">Dolgozhat körül CTAS a forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="befe1-135">Scenarios that can be worked around with CTAS include:</span></span>

* <span data-ttu-id="befe1-136">A frissítések ANSI ILLESZTÉSEK</span><span class="sxs-lookup"><span data-stu-id="befe1-136">ANSI JOINS on UPDATEs</span></span>
* <span data-ttu-id="befe1-137">Törli a ANSI illesztések</span><span class="sxs-lookup"><span data-stu-id="befe1-137">ANSI JOINs on DELETEs</span></span>
* <span data-ttu-id="befe1-138">MERGE utasítást</span><span class="sxs-lookup"><span data-stu-id="befe1-138">MERGE statement</span></span>

> [!NOTE]
> <span data-ttu-id="befe1-139">Próbálja meg toothink "CTAS első".</span><span class="sxs-lookup"><span data-stu-id="befe1-139">Try toothink "CTAS first".</span></span> <span data-ttu-id="befe1-140">Ha úgy gondolja, hogy egy probléma segítségével megoldható `CTAS` akkor az általában hello legjobb módja tooapproach, - akkor is, ha emiatt több adatot ír.</span><span class="sxs-lookup"><span data-stu-id="befe1-140">If you think you can solve a problem using `CTAS` then that is generally hello best way tooapproach it - even if you are writing more data as a result.</span></span>
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a><span data-ttu-id="befe1-141">Update utasításokban ANSI illesztési váltja fel</span><span class="sxs-lookup"><span data-stu-id="befe1-141">ANSI join replacement for update statements</span></span>
<span data-ttu-id="befe1-142">Előfordulhat, hogy egy összetett frissítést, amelyhez csatlakozik, több mint két tábla ANSI való csatlakozás szintaxis tooperform hello használata a frissítés vagy törlés.</span><span class="sxs-lookup"><span data-stu-id="befe1-142">You may find you have a complex update that joins more than two tables together using ANSI joining syntax tooperform hello UPDATE or DELETE.</span></span>

<span data-ttu-id="befe1-143">Képzelje el ezt a táblázatot kellett tooupdate:</span><span class="sxs-lookup"><span data-stu-id="befe1-143">Imagine you had tooupdate this table:</span></span>

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

<span data-ttu-id="befe1-144">hello eredeti lekérdezés előfordulhat, hogy rendelkezik kikeresi például ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="befe1-144">hello original query might have looked something like this:</span></span>

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

<span data-ttu-id="befe1-145">Mivel az SQL Data Warehouse nem támogatja az ANSI társítások (JOIN) hello `FROM` záradékában egy `UPDATE` utasítás, nem lehet másolni a kódot keresztül némileg módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="befe1-145">Since SQL Data Warehouse does not support ANSI joins in hello `FROM` clause of an `UPDATE` statement, you cannot copy this code over without changing it slightly.</span></span>

<span data-ttu-id="befe1-146">A kombinációját is használja a `CTAS` és egy implicit csatlakozás tooreplace ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="befe1-146">You can use a combination of a `CTAS` and an implicit join tooreplace this code:</span></span>

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join tooperform hello update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop hello interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a><span data-ttu-id="befe1-147">ANSI illesztési helyettesíti a törlési utasítást</span><span class="sxs-lookup"><span data-stu-id="befe1-147">ANSI join replacement for delete statements</span></span>
<span data-ttu-id="befe1-148">Egyes esetekben hello legjobb módszer az adatok törlése az toouse `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="befe1-148">Sometimes hello best approach for deleting data is toouse `CTAS`.</span></span> <span data-ttu-id="befe1-149">Ahelyett, hogy törli a hello adatot egyszerűen jelölje ki a kívánt tookeep hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="befe1-149">Rather than deleting hello data simply select hello data you want tookeep.</span></span> <span data-ttu-id="befe1-150">Ez különösen igaz `DELETE` utasítás szintaxisa csatlakoztatása, mivel az SQL Data Warehouse nem támogatja az ANSI illesztések hello ansi használó `FROM` záradékában egy `DELETE` utasítást.</span><span class="sxs-lookup"><span data-stu-id="befe1-150">This especially true for `DELETE` statements that use ansi joining syntax since SQL Data Warehouse does not support ANSI joins in hello `FROM` clause of a `DELETE` statement.</span></span>

<span data-ttu-id="befe1-151">A konvertált DELETE utasításban példát alább érhető el:</span><span class="sxs-lookup"><span data-stu-id="befe1-151">An example of a converted DELETE statement is available below:</span></span>

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish tookeep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        tooDimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert tooDimProduct;
```

## <a name="replace-merge-statements"></a><span data-ttu-id="befe1-152">Cserélje le a merge utasítások</span><span class="sxs-lookup"><span data-stu-id="befe1-152">Replace merge statements</span></span>
<span data-ttu-id="befe1-153">Merge utasításokban cserélhetősége, legalább a részben a `CTAS`.</span><span class="sxs-lookup"><span data-stu-id="befe1-153">Merge statements can be replaced, at least in part, by using `CTAS`.</span></span> <span data-ttu-id="befe1-154">A konszolidáció hello `INSERT` és hello `UPDATE` be egy utasítás.</span><span class="sxs-lookup"><span data-stu-id="befe1-154">You can consolidate hello `INSERT` and hello `UPDATE` into a single statement.</span></span> <span data-ttu-id="befe1-155">Minden olyan törölt rekord kellene zárni egy második utasításban toobe.</span><span class="sxs-lookup"><span data-stu-id="befe1-155">Any deleted records would need toobe closed off in a second statement.</span></span>

<span data-ttu-id="befe1-156">Például egy `UPSERT` alatt érhető el:</span><span class="sxs-lookup"><span data-stu-id="befe1-156">An example of an `UPSERT` is available below:</span></span>

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          too[DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  too[DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a><span data-ttu-id="befe1-157">CTAS javaslat: explicit módon a adattípus és a kimeneti nullázhatóságának állapot</span><span class="sxs-lookup"><span data-stu-id="befe1-157">CTAS recommendation: Explicitly state data type and nullability of output</span></span>
<span data-ttu-id="befe1-158">Kód áttelepítésekor bizonyára hasznosnak találja, az ilyen típusú kódolási mintát keresztül futtatja:</span><span class="sxs-lookup"><span data-stu-id="befe1-158">When migrating code you might find you run across this type of coding pattern:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

<span data-ttu-id="befe1-159">Érdemes instinctively át kell telepítenie a kód tooa CTAS és lenne megfelelő.</span><span class="sxs-lookup"><span data-stu-id="befe1-159">Instinctively you might think you should migrate this code tooa CTAS and you would be correct.</span></span> <span data-ttu-id="befe1-160">Van azonban egy rejtett probléma.</span><span class="sxs-lookup"><span data-stu-id="befe1-160">However, there is a hidden issue here.</span></span>

<span data-ttu-id="befe1-161">hello alábbira nem fed fel ugyanazt az eredményt hello:</span><span class="sxs-lookup"><span data-stu-id="befe1-161">hello following code does NOT yield hello same result:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

<span data-ttu-id="befe1-162">Figyelje meg, győződjön meg arról, hogy hello oszlop "eredménye" előre hello adatok típusát és nullázhatóságának hello kifejezésben szereplő végzi.</span><span class="sxs-lookup"><span data-stu-id="befe1-162">Notice that hello column "result" carries forward hello data type and nullability values of hello expression.</span></span> <span data-ttu-id="befe1-163">Ha még nem gondosan meg kell, ez is járhat toosubtle eltérésekre értékeket.</span><span class="sxs-lookup"><span data-stu-id="befe1-163">This can lead toosubtle variances in values if you aren't careful.</span></span>

<span data-ttu-id="befe1-164">Próbálkozzon hello következő egy példa:</span><span class="sxs-lookup"><span data-stu-id="befe1-164">Try hello following as an example:</span></span>

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

<span data-ttu-id="befe1-165">hello tárolt eredmény értéke eltérő.</span><span class="sxs-lookup"><span data-stu-id="befe1-165">hello value stored for result is different.</span></span> <span data-ttu-id="befe1-166">Hello megőrzött értékét hello eredményoszlop használja más kifejezések hello hiba még jelentősebb válik.</span><span class="sxs-lookup"><span data-stu-id="befe1-166">As hello persisted value in hello result column is used in other expressions hello error becomes even more significant.</span></span>

![][1]

<span data-ttu-id="befe1-167">Ez különösen fontos az adatok áttelepítése.</span><span class="sxs-lookup"><span data-stu-id="befe1-167">This is particularly important for data migrations.</span></span> <span data-ttu-id="befe1-168">Annak ellenére, hogy hello második lekérdezés késései pontosabb probléma van.</span><span class="sxs-lookup"><span data-stu-id="befe1-168">Even though hello second query is arguably more accurate there is a problem.</span></span> <span data-ttu-id="befe1-169">hello adatok különböző összehasonlított toohello forrásrendszerben és integritásának tooquestions hello áttelepítés eredményezi.</span><span class="sxs-lookup"><span data-stu-id="befe1-169">hello data would be different compared toohello source system and that leads tooquestions of integrity in hello migration.</span></span> <span data-ttu-id="befe1-170">Ez az egyik ezeket bizonyos ritkán előforduló esetekben, amikor hello "hibás" válasz ténylegesen hello jobbról egy!</span><span class="sxs-lookup"><span data-stu-id="befe1-170">This is one of those rare cases where hello "wrong" answer is actually hello right one!</span></span>

<span data-ttu-id="befe1-171">hello oka a hello két eredmények eltérései látható, lefelé tooimplicit adattípusokról írja be.</span><span class="sxs-lookup"><span data-stu-id="befe1-171">hello reason we see this disparity between hello two results is down tooimplicit type casting.</span></span> <span data-ttu-id="befe1-172">Első példa hello tábla hello határozza meg a hello oszlop definíciójában.</span><span class="sxs-lookup"><span data-stu-id="befe1-172">In hello first example hello table defines hello column definition.</span></span> <span data-ttu-id="befe1-173">Az implicit típuskonverzió átalakítás akkor fordul elő, amikor hello sor kerül.</span><span class="sxs-lookup"><span data-stu-id="befe1-173">When hello row is inserted an implicit type conversion occurs.</span></span> <span data-ttu-id="befe1-174">Hello második példában nincs nincs implicit konvertálása, hello kifejezés határozza meg a hello oszlop adattípusát.</span><span class="sxs-lookup"><span data-stu-id="befe1-174">In hello second example there is no implicit type conversion as hello expression defines data type of hello column.</span></span> <span data-ttu-id="befe1-175">Figyelje meg is, hogy hello oszlop hello második példában definiálva van nullázható oszlopot, mivel az első példában hello nem.</span><span class="sxs-lookup"><span data-stu-id="befe1-175">Notice also that hello column in hello second example has been defined as a NULLable column whereas in hello first example it has not.</span></span> <span data-ttu-id="befe1-176">Hello tábla létrehozásakor hello első példa oszlop nullázhatósági explicit módon van definiálva.</span><span class="sxs-lookup"><span data-stu-id="befe1-176">When hello table was created in hello first example column nullability was explicitly defined.</span></span> <span data-ttu-id="befe1-177">Hello második példában az lett csak elhagyta toohello kifejezés, és alapértelmezés szerint ez eredményeznének NULL definícióját.</span><span class="sxs-lookup"><span data-stu-id="befe1-177">In hello second example it was just left toohello expression and by default this would result in a NULL definition.</span></span>  

<span data-ttu-id="befe1-178">Ezek kapcsolatban, hogy tooresolve explicit módon be kell hello konvertálásához és nullázhatóságának hello `SELECT` hello része `CTAS` utasítást.</span><span class="sxs-lookup"><span data-stu-id="befe1-178">tooresolve these issues you must explicitly set hello type conversion and nullability in hello `SELECT` portion of hello `CTAS` statement.</span></span> <span data-ttu-id="befe1-179">Nem állítható be a tulajdonságokat az hello létrehozni tábla.</span><span class="sxs-lookup"><span data-stu-id="befe1-179">You cannot set these properties in hello create table part.</span></span>

<span data-ttu-id="befe1-180">hello az alábbi példa bemutatja, hogyan toofix hello kódot:</span><span class="sxs-lookup"><span data-stu-id="befe1-180">hello example below demonstrates how toofix hello code:</span></span>

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

<span data-ttu-id="befe1-181">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="befe1-181">Note hello following:</span></span>

* <span data-ttu-id="befe1-182">CAST vagy CONVERT sikerült fel lett használva</span><span class="sxs-lookup"><span data-stu-id="befe1-182">CAST or CONVERT could have been used</span></span>
* <span data-ttu-id="befe1-183">ISNULL használt tooforce nullázhatósági nem COALESCE</span><span class="sxs-lookup"><span data-stu-id="befe1-183">ISNULL is used tooforce NULLability not COALESCE</span></span>
* <span data-ttu-id="befe1-184">ISNULL hello legkülső függvény</span><span class="sxs-lookup"><span data-stu-id="befe1-184">ISNULL is hello outermost function</span></span>
* <span data-ttu-id="befe1-185">hello második hello ISNULL része egy állandó például 0</span><span class="sxs-lookup"><span data-stu-id="befe1-185">hello second part of hello ISNULL is a constant i.e. 0</span></span>

> [!NOTE]
> <span data-ttu-id="befe1-186">Hello nullázhatósági toobe megfelelően beállítani az létfontosságú toouse `ISNULL` és nem `COALESCE`.</span><span class="sxs-lookup"><span data-stu-id="befe1-186">For hello nullability toobe correctly set it is vital toouse `ISNULL` and not `COALESCE`.</span></span> <span data-ttu-id="befe1-187">`COALESCE`nem determinisztikus-tól, és így hello hello kifejezés eredménye mindig lesz NULLable.</span><span class="sxs-lookup"><span data-stu-id="befe1-187">`COALESCE` is not a deterministic function and so hello result of hello expression will always be NULLable.</span></span> <span data-ttu-id="befe1-188">`ISNULL`nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="befe1-188">`ISNULL` is different.</span></span> <span data-ttu-id="befe1-189">Determinisztikus.</span><span class="sxs-lookup"><span data-stu-id="befe1-189">It is deterministic.</span></span> <span data-ttu-id="befe1-190">Ezért ha hello hello második része `ISNULL` függvény egy állandó vagy -szövegkonstans, akkor a hello az eredményül kapott érték nem null értékű.</span><span class="sxs-lookup"><span data-stu-id="befe1-190">Therefore when hello second part of hello `ISNULL` function is a constant or a literal then hello resulting value will be NOT NULL.</span></span>
> 
> 

<span data-ttu-id="befe1-191">Ez tipp nincs csak akkor hasznos, a számítások hello integritásának biztosításához.</span><span class="sxs-lookup"><span data-stu-id="befe1-191">This tip is not just useful for ensuring hello integrity of your calculations.</span></span> <span data-ttu-id="befe1-192">Fontos továbbá tábla partíciós átállításához.</span><span class="sxs-lookup"><span data-stu-id="befe1-192">It is also important for table partition switching.</span></span> <span data-ttu-id="befe1-193">Képzelje el ezt a táblázatot a tény definiálva van:</span><span class="sxs-lookup"><span data-stu-id="befe1-193">Imagine you have this table defined as your fact:</span></span>

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

<span data-ttu-id="befe1-194">Azonban hello érték mező nem egy része nem hello forrásadatok számított kifejezés.</span><span class="sxs-lookup"><span data-stu-id="befe1-194">However, hello value field is a calculated expression it is not part of hello source data.</span></span>

<span data-ttu-id="befe1-195">toocreate a particionált adatkészlet-érdemes toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="befe1-195">toocreate your partitioned dataset you might want toodo this:</span></span>

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

<span data-ttu-id="befe1-196">hello lekérdezés tökéletesen finom fog futni.</span><span class="sxs-lookup"><span data-stu-id="befe1-196">hello query would run perfectly fine.</span></span> <span data-ttu-id="befe1-197">hello probléma tooperform hello partíció kapcsolójának meg előre.</span><span class="sxs-lookup"><span data-stu-id="befe1-197">hello problem comes when you try tooperform hello partition switch.</span></span> <span data-ttu-id="befe1-198">hello definíciói nem egyeznek.</span><span class="sxs-lookup"><span data-stu-id="befe1-198">hello table definitions do not match.</span></span> <span data-ttu-id="befe1-199">toomake hello definíciói hello CTAS toobe módosítani kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="befe1-199">toomake hello table definitions match hello CTAS needs toobe modified.</span></span>

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

<span data-ttu-id="befe1-200">Látható ezért, hogy típus konzisztencia és karbantartása a CTAS nullázhatósági tulajdonságainak jó mérnöki ajánlott.</span><span class="sxs-lookup"><span data-stu-id="befe1-200">You can see therefore that type consistency and maintaining nullability properties on a CTAS is a good engineering best practice.</span></span> <span data-ttu-id="befe1-201">Segít a számítások toomaintain integritását, és is biztosítja, hogy a partíció váltás lehetséges.</span><span class="sxs-lookup"><span data-stu-id="befe1-201">It helps toomaintain integrity in your calculations and also ensures that partition switching is possible.</span></span>

<span data-ttu-id="befe1-202">Tekintse meg a további információk az tooMSDN [CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="befe1-202">Please refer tooMSDN for more information on using [CTAS][CTAS].</span></span> <span data-ttu-id="befe1-203">Hello legfontosabb utasítások az Azure SQL Data Warehouse egyike.</span><span class="sxs-lookup"><span data-stu-id="befe1-203">It is one of hello most important statements in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="befe1-204">Győződjön meg arról, hogy alaposan ismertetése.</span><span class="sxs-lookup"><span data-stu-id="befe1-204">Make sure you thoroughly understand it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="befe1-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="befe1-205">Next steps</span></span>
<span data-ttu-id="befe1-206">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="befe1-206">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
