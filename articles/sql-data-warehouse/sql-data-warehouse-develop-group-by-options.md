---
title: "az SQL Data Warehouse beállítások aaaGroup |} Microsoft Docs"
description: "Ötletek a csoport beállításai az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: cc443c2af4e3ef2babd74d78aa6fb57bb3c1c7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a><span data-ttu-id="a558f-103">Az SQL Data Warehouse beállítások szerint kell csoportosítani</span><span class="sxs-lookup"><span data-stu-id="a558f-103">Group by options in SQL Data Warehouse</span></span>
<span data-ttu-id="a558f-104">Hello [GROUP BY] [ GROUP BY] záradék tooaggregate tooa összefoglaló adatkészlet sorok.</span><span class="sxs-lookup"><span data-stu-id="a558f-104">hello [GROUP BY][GROUP BY] clause is used tooaggregate data tooa summary set of rows.</span></span> <span data-ttu-id="a558f-105">Azt is, amelyek kibővítik annak funkcióit, hogy szükség toobe körül működött, nem közvetlenül támogatja őket Azure SQL Data Warehouse néhány lehetőség.</span><span class="sxs-lookup"><span data-stu-id="a558f-105">It also has a few options that extend it's functionality that need toobe worked around as they are not directly supported by Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="a558f-106">Ezek a beállítások</span><span class="sxs-lookup"><span data-stu-id="a558f-106">These options are</span></span>

* <span data-ttu-id="a558f-107">A GROUP BY összesítő</span><span class="sxs-lookup"><span data-stu-id="a558f-107">GROUP BY with ROLLUP</span></span>
* <span data-ttu-id="a558f-108">GROUPING SETS</span><span class="sxs-lookup"><span data-stu-id="a558f-108">GROUPING SETS</span></span>
* <span data-ttu-id="a558f-109">A GROUP BY ADATKOCKA</span><span class="sxs-lookup"><span data-stu-id="a558f-109">GROUP BY with CUBE</span></span>

## <a name="rollup-and-grouping-sets-options"></a><span data-ttu-id="a558f-110">Rollup és grouping sets beállítások</span><span class="sxs-lookup"><span data-stu-id="a558f-110">Rollup and grouping sets options</span></span>
<span data-ttu-id="a558f-111">hello Itt a legegyszerűbb lehetőség toouse `UNION ALL` helyette hagyatkoznia helyett tooperform hello összesítő hello explicit szintaxist.</span><span class="sxs-lookup"><span data-stu-id="a558f-111">hello simplest option here is toouse `UNION ALL` instead tooperform hello rollup rather than relying on hello explicit syntax.</span></span> <span data-ttu-id="a558f-112">hello eredmény van pontosan hello azonos</span><span class="sxs-lookup"><span data-stu-id="a558f-112">hello result is exactly hello same</span></span>

<span data-ttu-id="a558f-113">Az alábbiakban látható egy példa egy csoport hello segítségével utasítással `ROLLUP` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="a558f-113">Below is an example of a group by statement using hello `ROLLUP` option:</span></span>

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

<span data-ttu-id="a558f-114">ÖSSZESÍTŐ használatával azt kért a következő összesítések hello:</span><span class="sxs-lookup"><span data-stu-id="a558f-114">By using ROLLUP we have requested hello following aggregations:</span></span>

* <span data-ttu-id="a558f-115">Ország és régió</span><span class="sxs-lookup"><span data-stu-id="a558f-115">Country and Region</span></span>
* <span data-ttu-id="a558f-116">Ország</span><span class="sxs-lookup"><span data-stu-id="a558f-116">Country</span></span>
* <span data-ttu-id="a558f-117">Végösszeg</span><span class="sxs-lookup"><span data-stu-id="a558f-117">Grand Total</span></span>

<span data-ttu-id="a558f-118">tooreplace ez toouse kell `UNION ALL`; adja meg a szükséges explicit módon hello összesítések tooreturn hello ugyanazokat az eredményeket:</span><span class="sxs-lookup"><span data-stu-id="a558f-118">tooreplace this you will need toouse `UNION ALL`; specifying hello aggregations required explicitly tooreturn hello same results:</span></span>

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

<span data-ttu-id="a558f-119">Csak meg kell toodo elfogadják GROUPING SETS hello azonos egyszerű, de csak hozzon létre az hello UNION ALL szakaszaiban összesítési szintek szeretnénk toosee</span><span class="sxs-lookup"><span data-stu-id="a558f-119">For GROUPING SETS all we need toodo is adopt hello same principal but only create UNION ALL sections for hello aggregation levels we want toosee</span></span>

## <a name="cube-options"></a><span data-ttu-id="a558f-120">Adatkocka-beállítások</span><span class="sxs-lookup"><span data-stu-id="a558f-120">Cube options</span></span>
<span data-ttu-id="a558f-121">Már lehetséges toocreate csoport által a KOCKA hello UNION ALL módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="a558f-121">It is possible toocreate a GROUP BY WITH CUBE using hello UNION ALL approach.</span></span> <span data-ttu-id="a558f-122">hello probléma az, hogy hello kód nehézkes és kezelése nehézkessé vehet.</span><span class="sxs-lookup"><span data-stu-id="a558f-122">hello problem is that hello code can quickly become cumbersome and unwieldy.</span></span> <span data-ttu-id="a558f-123">toomitigate ez ezzel fejlettebb módszert.</span><span class="sxs-lookup"><span data-stu-id="a558f-123">toomitigate this you can use this more advanced approach.</span></span>

<span data-ttu-id="a558f-124">Most használja a fenti hello példát.</span><span class="sxs-lookup"><span data-stu-id="a558f-124">Let's use hello example above.</span></span>

<span data-ttu-id="a558f-125">hello első lépéseként toodefine hello "adatkocka", amely meghatározza, hogy szeretnénk toocreate összesítési minden hello szintjén.</span><span class="sxs-lookup"><span data-stu-id="a558f-125">hello first step is toodefine hello 'cube' that defines all hello levels of aggregation that we want toocreate.</span></span> <span data-ttu-id="a558f-126">Fontos tootake tudomásul hello két származtatott táblák CROSS JOIN hello.</span><span class="sxs-lookup"><span data-stu-id="a558f-126">It is important tootake note of hello CROSS JOIN of hello two derived tables.</span></span> <span data-ttu-id="a558f-127">Ezt követően az összes hello szint nekünk.</span><span class="sxs-lookup"><span data-stu-id="a558f-127">This generates all hello levels for us.</span></span> <span data-ttu-id="a558f-128">hello kód hello részeinek van valóban formázásához.</span><span class="sxs-lookup"><span data-stu-id="a558f-128">hello rest of hello code is really there for formatting.</span></span>

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

<span data-ttu-id="a558f-129">hello hello eredményeit CTAS látható alatt:</span><span class="sxs-lookup"><span data-stu-id="a558f-129">hello results of hello CTAS can be seen below:</span></span>

![][1]

<span data-ttu-id="a558f-130">hello második lépése annak az eredménye egy cél tábla toostore ideiglenes toospecify:</span><span class="sxs-lookup"><span data-stu-id="a558f-130">hello second step is toospecify a target table toostore interim results:</span></span>

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

<span data-ttu-id="a558f-131">hello harmadik lépése tooloop végrehajtása hello összesítő oszlopok a kocka felett.</span><span class="sxs-lookup"><span data-stu-id="a558f-131">hello third step is tooloop over our cube of columns performing hello aggregation.</span></span> <span data-ttu-id="a558f-132">hello lekérdezés egyszer futnak le minden egyes sorára hello #Cube ideiglenes tábla és hello eredmények tárolása hello #Results ideiglenes tábla</span><span class="sxs-lookup"><span data-stu-id="a558f-132">hello query will run once for every row in hello #Cube temporary table and store hello results in hello #Results temp table</span></span>

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

<span data-ttu-id="a558f-133">Végül azt is eredményeket hello hello #Results ideiglenes táblából egyszerűen beolvasásával</span><span class="sxs-lookup"><span data-stu-id="a558f-133">Lastly we can return hello results by simply reading from hello #Results temporary table</span></span>

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

<span data-ttu-id="a558f-134">Hello kód összeállításának részre, és létrehozzon egy ismétlési szerkezet hello kód lesz kezelhető, és fenntarthatóvá.</span><span class="sxs-lookup"><span data-stu-id="a558f-134">By breaking hello code up into sections and generating a looping construct hello code becomes more manageable and maintainable.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a558f-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a558f-135">Next steps</span></span>
<span data-ttu-id="a558f-136">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="a558f-136">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
