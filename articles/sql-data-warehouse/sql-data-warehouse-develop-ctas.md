---
title: "Válasszon ki (CTAS) az SQL Data Warehouse tábla létrehozása |} Microsoft Docs"
description: "Tippek a létrehozás táblával kódolás szerint select (CTAS) utasítás az Azure SQL Data Warehouse adattárházzal történő, megoldások."
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
ms.openlocfilehash: cb08313726e8135feaa9b413937c2197ea397f4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Select (CTAS) tábla az SQL Data Warehouse létrehozása
Válassza ki a táblázatban létrehozása vagy `CTAS` egyike a legfontosabb T-SQL funkciókat érhető el. Egy teljes mértékben parallelized művelet, amely új táblát hoz létre a kimeneti SELECT utasítás alapján. `CTAS`a rendszer a legegyszerűbb módja a tábla létrehozásához. Ez a dokumentum nyújt, példák és ajánlott eljárásai `CTAS`.

## <a name="selectinto-vs-ctas"></a>VÁLASSZON... A Visual Studio. CTAS
Érdemes lehet `CTAS` felső szinten megterhelni változata `SELECT..INTO`.

Az alábbiakban látható egy példa egy egyszerű `SELECT..INTO` utasítást:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

A fenti példában szereplő `[dbo].[FactInternetSales_new]` mivel ezek az Azure SQL Data warehouse tábla alapértelmezett rajta a FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXSZEL rendelkező ROUND_ROBIN elosztott táblázatként létrehozott.

`SELECT..INTO`azonban nem teszi lehetővé a művelet részeként a telepítési módszer vagy a Indextípus módosításához. Ez akkor, ha `CTAS` érkezik.

A fenti konvertálható `CTAS` meglehetősen egyszerű feladat:

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

A `CTAS` megváltoztatása a tábla adatai, valamint a táblatípus mindkét a terjesztési válik. 

> [!NOTE]
> Ha csak kívánt indexe, módosítsa a `CTAS` művelet és a forrástábla elosztott kivonatoló, majd a `CTAS` művelet legjobb hajtja végre, ha a terjesztési oszlop és adatok ugyanolyan. Így elkerülhető, közötti terjesztési adatátvitelt jelölik a hatékonyabb művelet során.
> 
> 

## <a name="using-ctas-to-copy-a-table"></a>Másolja át egy táblázatot a CTAS használatával
Lehet, hogy a leggyakoribb egyikét használja a `CTAS` hoz létre egy tábla egy példányát, hogy a DDL bármikor módosíthatja. Ha például eredetileg létrehozott a táblázatban `ROUND_ROBIN` és most kívánja módosítani egy táblához, egy olyan oszloptól, a terjesztés `CTAS` hogyan megváltozna a terjesztési oszlop van. `CTAS`is használható a particionálás, indexelő vagy oszlop típusának módosítása.

Tegyük fel, ez a táblázat alapértelmezett eloszlási típusát használatával létrehozott `ROUND_ROBIN` elosztott, mivel nincs terjesztési oszlop van megadva a a `CREATE TABLE`.

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

Most szeretne létrehozni a fürtözött Oszlopcentrikus indexszel rendelkező Ez a táblázat másolatát, így kihasználhatja a fürtözött Oszloptárindexű táblákat teljesítményének. Is szeretné, hogy ezt a táblázatot a ProductKey terjesztéséhez, mert meg vannak előrejelző illesztések ettől az oszloptól, és el szeretné kerülni az adatátvitel során a ProductKey illesztéseket. Végül is hozzáadandó OrderDateKey a particionálás, hogy a régi adatok régi partíciók ejtésével gyorsan törölhetők. Ez a CTAS kimutatásban, amely a régi tábla szeretné másolni egy új táblába.

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

Végül átnevezheti a táblákat a új tábla felcserélése, és majd dobja el a régi táblát.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.  A legjobb lekérdezési teljesítmény eléréséhez fontos létrehozni statisztikákat a táblák összes oszlopához az első betöltés után, illetve az adatok minden lényeges módosítását követően.  A statisztika részletes ismertetését a Fejlesztés témakörcsoport [Statisztika][Statistics] témakörében találja.
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a>CTAS használata nem támogatott funkciókat megoldása
`CTAS`is használható a nem támogatott funkciókat alább felsorolt számos megoldása. Ez gyakran bizonyítja, hogy egy Windows/win helyzet lehet, mert nem csak a kód lesznek megfelelőek, de ez gyakran végrehajtja a gyorsabb SQL Data warehouse. Ez egy, a teljes parallelized kialakításakor miatt. Dolgozhat körül CTAS a forgatókönyvek a következők:

* A frissítések ANSI ILLESZTÉSEK
* Törli a ANSI illesztések
* MERGE utasítást

> [!NOTE]
> Gondolja, hogy megpróbálja "CTAS első". Ha úgy gondolja, hogy egy probléma segítségével megoldható `CTAS` majd, amely általában a legjobb módszer az oldalról - akkor is, ha emiatt több adatot ír.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>Update utasításokban ANSI illesztési váltja fel
Előfordulhat, hogy egy összetett frissítést, amelyhez csatlakozik, több mint két tábla végrehajtani a frissítési vagy törlési ANSI való csatlakozás szintaxis használata.

Képzelje el ezt a táblázatot frissíteni kellett:

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

Az eredeti lekérdezés előfordulhat, hogy rendelkezik kikeresi például ehhez hasonló:

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

Mivel az SQL Data Warehouse nem támogatja az ANSI társítások (JOIN) az `FROM` záradékában egy `UPDATE` utasítás, nem lehet másolni a kódot keresztül némileg módosítás nélkül.

A kombinációját is használja a `CTAS` és egy implicit illesztési, ha ezt a kódot:

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

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI illesztési helyettesíti a törlési utasítást
Néha adatok törléséhez a legjobb módszer az, hogy használjon `CTAS`. Ahelyett, hogy az adatok törlése egyszerűen jelölje ki a megőrizni kívánt adatokat. Ez különösen igaz `DELETE` ansi csatlakozó szintaxis, mert az SQL Data Warehouse nem támogatja az ANSI társítások (JOIN) használó utasítások a `FROM` záradékában egy `DELETE` utasítást.

A konvertált DELETE utasításban példát alább érhető el:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Cserélje le a merge utasítások
Merge utasításokban cserélhetősége, legalább a részben a `CTAS`. Képes egyesíteni a `INSERT` és a `UPDATE` be egy utasítás. Minden olyan törölt rekord kellene le kell zárni a második utasításban.

Például egy `UPSERT` alatt érhető el:

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

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS javaslat: explicit módon a adattípus és a kimeneti nullázhatóságának állapot
Kód áttelepítésekor bizonyára hasznosnak találja, az ilyen típusú kódolási mintát keresztül futtatja:

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

Instinctively érdemes ezt a kódot át kell telepíteni a CTAS és lenne megfelelő. Van azonban egy rejtett probléma.

A következő kódot a ugyanazt az eredményt nem fed fel:

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

Figyelje meg, hogy az oszlop "eredménye" hordoz magában, ha előre az adatok típusát és nullázhatóságának a kifejezésben szereplő érték. Finom eltérések az értékek ez is vezethet, ha még nem óvatos.

Próbálkozzon a következő példa:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

A tárolt eredményt értéke eltérő. A megőrzött a eredményoszlop értékét használja a többi kifejezésében hiba még jelentősebb válik.

![][1]

Ez különösen fontos az adatok áttelepítése. Annak ellenére, hogy a második lekérdezés késései pontosabb probléma van. Az adatokat eltérő lenne a forrásrendszerben képest, és sértetlenségének áttelepítésének kérdésekre eredményezi. Ez az egyik ezen bizonyos ritkán előforduló esetekben, amikor a "hibás" válasz ténylegesen a megfelelőt.

Ez a két eredményt eltérései látható oka implicit típuskonverzió adattípusokról le. Az első példában a tábla határozza meg az oszlop definíciójában. Az implicit típuskonverzió átalakítás akkor fordul elő, amikor a sor kerül. A második példában nincs nincs implicit konvertálása, a kifejezés határozza meg az oszlop adattípusát. Is vegye figyelembe, hogy a második példában az oszlop definiálva van egy nullázható oszlopot, mivel az első nem. A táblázat létrehozásának a példa első oszlop nullázhatóságával explicit módon van definiálva. A második példában az lett csak bal oldali kifejezésnek és alapértelmezés szerint ez NULL definícióját eredményezne.  

A problémák megoldásához explicit módon be kell a típuskonverziós és a nullázhatósági a `SELECT` része a `CTAS` utasítást. Create table részében ezen tulajdonságai nem állíthatók be.

Az alábbi példa bemutatja, hogyan javítsa ki a kódot:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Vegye figyelembe a következőket:

* CAST vagy CONVERT sikerült fel lett használva
* ISNULL nullázhatósági nem COALESCE kötelező használhatja
* ISNULL, akkor a legkülső függvény
* A ISNULL második része egy állandó például 0

> [!NOTE]
> A nullázhatósági kell megfelelően állítsa be a létfontosságú használandó `ISNULL` és nem `COALESCE`. `COALESCE`nem determinisztikus-tól, és ezért a kifejezés eredménye mindig lesz NULLable. `ISNULL`nem egyezik. Determinisztikus. Ezért ha második része a `ISNULL` függvény egy állandó vagy -szövegkonstans, akkor az eredményül kapott érték lesz nem null értékű.
> 
> 

Ez tipp nincs csak akkor hasznos, a számítások integritásának biztosításához. Fontos továbbá tábla partíciós átállításához. Képzelje el ezt a táblázatot a tény definiálva van:

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

Azonban az érték mező értéke már nem a forrásadatok része számított kifejezés.

A particionált adatkészlet-érdemes ehhez létrehozása:

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

A lekérdezés tökéletesen finom fog futni. A probléma hajtsa végre a partíció kapcsolójának próbál származik. A tábla definícióit nem egyeznek. Ellenőrizze a felel meg a CTAS definíciói kell módosítani.

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

Látható ezért, hogy típus konzisztencia és karbantartása a CTAS nullázhatósági tulajdonságainak jó mérnöki ajánlott. Segít a számítások egységének fenntartására szolgáló módszert, és is biztosítja, hogy a partíció váltás lehetséges.

További információk az MSDN tekintse meg [CTAS][CTAS]. A legfontosabb utasításokat az Azure SQL Data Warehouse egyike. Győződjön meg arról, hogy alaposan ismertetése.

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
