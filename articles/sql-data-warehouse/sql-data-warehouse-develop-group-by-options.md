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
# <a name="group-by-options-in-sql-data-warehouse"></a>Az SQL Data Warehouse beállítások szerint kell csoportosítani
Hello [GROUP BY] [ GROUP BY] záradék tooaggregate tooa összefoglaló adatkészlet sorok. Azt is, amelyek kibővítik annak funkcióit, hogy szükség toobe körül működött, nem közvetlenül támogatja őket Azure SQL Data Warehouse néhány lehetőség.

Ezek a beállítások

* A GROUP BY összesítő
* GROUPING SETS
* A GROUP BY ADATKOCKA

## <a name="rollup-and-grouping-sets-options"></a>Rollup és grouping sets beállítások
hello Itt a legegyszerűbb lehetőség toouse `UNION ALL` helyette hagyatkoznia helyett tooperform hello összesítő hello explicit szintaxist. hello eredmény van pontosan hello azonos

Az alábbiakban látható egy példa egy csoport hello segítségével utasítással `ROLLUP` lehetőséget:

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

ÖSSZESÍTŐ használatával azt kért a következő összesítések hello:

* Ország és régió
* Ország
* Végösszeg

tooreplace ez toouse kell `UNION ALL`; adja meg a szükséges explicit módon hello összesítések tooreturn hello ugyanazokat az eredményeket:

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

Csak meg kell toodo elfogadják GROUPING SETS hello azonos egyszerű, de csak hozzon létre az hello UNION ALL szakaszaiban összesítési szintek szeretnénk toosee

## <a name="cube-options"></a>Adatkocka-beállítások
Már lehetséges toocreate csoport által a KOCKA hello UNION ALL módszer használatával. hello probléma az, hogy hello kód nehézkes és kezelése nehézkessé vehet. toomitigate ez ezzel fejlettebb módszert.

Most használja a fenti hello példát.

hello első lépéseként toodefine hello "adatkocka", amely meghatározza, hogy szeretnénk toocreate összesítési minden hello szintjén. Fontos tootake tudomásul hello két származtatott táblák CROSS JOIN hello. Ezt követően az összes hello szint nekünk. hello kód hello részeinek van valóban formázásához.

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

hello hello eredményeit CTAS látható alatt:

![][1]

hello második lépése annak az eredménye egy cél tábla toostore ideiglenes toospecify:

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

hello harmadik lépése tooloop végrehajtása hello összesítő oszlopok a kocka felett. hello lekérdezés egyszer futnak le minden egyes sorára hello #Cube ideiglenes tábla és hello eredmények tárolása hello #Results ideiglenes tábla

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

Végül azt is eredményeket hello hello #Results ideiglenes táblából egyszerűen beolvasásával

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Hello kód összeállításának részre, és létrehozzon egy ismétlési szerkezet hello kód lesz kezelhető, és fenntarthatóvá.

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
