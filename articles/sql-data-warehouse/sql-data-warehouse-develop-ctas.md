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
# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Select (CTAS) tábla az SQL Data Warehouse létrehozása
Válassza ki a táblázatban létrehozása vagy `CTAS` az hello egyik legfontosabb T-SQL funkciókat érhető el. Egy teljes mértékben parallelized művelet, amely táblát hoz létre új hello kimeneti SELECT utasítás alapján. `CTAS`hello legegyszerűbb módja toocreate van egy tábla egy példányát. Ez a dokumentum nyújt, példák és ajánlott eljárásai `CTAS`.

## <a name="selectinto-vs-ctas"></a>VÁLASSZON... A Visual Studio. CTAS
Érdemes lehet `CTAS` felső szinten megterhelni változata `SELECT..INTO`.

Az alábbiakban látható egy példa egy egyszerű `SELECT..INTO` utasítást:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

A fenti példában hello `[dbo].[FactInternetSales_new]` , ezek a hello tábla alapértelmezett beállításokat az Azure SQL Data Warehouse rajta a FÜRTÖZÖTT OSZLOPCENTRIKUS INDEXSZEL rendelkező ROUND_ROBIN elosztott táblázatként létrehozott.

`SELECT..INTO`azonban nem teszi lehetővé toochange hello terjesztési metódus vagy hello index írja be a hello művelet részeként. Ez akkor, ha `CTAS` érkezik.

tooconvert túl hello fent`CTAS` meglehetősen egyszerű feladat:

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

A `CTAS` képes toochange áll mindkét hello terjesztési hello tábla adatai, valamint a hello táblatípus. 

> [!NOTE]
> Ha csak a toochange hello indexe a `CTAS` művelet és hello forrástábla elosztott kivonatoló, majd a `CTAS` műveletet hajt végre legjobb, ha a hello azonos terjesztési oszlop és adatok típusát. Így elkerülhető, terjesztési adatmozgás közötti hatékonyabb hello művelet során.
> 
> 

## <a name="using-ctas-toocopy-a-table"></a>Egy tábla CTAS toocopy használatával
Lehet, hogy egyik leggyakoribb hello használja a `CTAS` hoz létre egy tábla egy példányát, hogy hello DDL bármikor módosíthatja. Ha például eredetileg létrehozott a táblázatban `ROUND_ROBIN` és most kívánja megváltoztatni a tooa táblázat egy olyan oszloptól, terjesztés `CTAS` hogyan megváltozna hello terjesztési oszlop van. `CTAS`akkor is használt toochange particionálás, indexelő vagy oszlop.

Tegyük fel, ez a táblázat hello alapértelmezett eloszlási típusát használatával létrehozott `ROUND_ROBIN` elosztott, mivel nincs terjesztési oszlop van megadva a hello `CREATE TABLE`.

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

Most szeretné ezt a táblázatot a fürtözött Oszlopcentrikus indexszel rendelkező új toocreate, hogy a fürtözött Oszloptárindexű táblákat hello teljesítményének előnyeit élvezheti. Szeretné toodistribute ezt a táblázatot a ProductKey vannak előrejelző illesztések ettől az oszloptól, és szeretné, hogy tooavoid adatátvitel során a ProductKey illesztések óta. Végül is érdemes tooadd particionálás OrderDateKey az, hogy a régi adatok régi partíciók ejtésével gyorsan törölhetők. Itt található hello CTAS kimutatásban, amely a régi tábla szeretné másolni egy új táblába.

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

Végül az új tábla nevezze át a táblák tooswap és majd dobja el a régi táblát.

```sql
RENAME OBJECT FactInternetSales tooFactInternetSales_old;
RENAME OBJECT FactInternetSales_new tooFactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.  A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő.  A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében.
> 
> 

## <a name="using-ctas-toowork-around-unsupported-features"></a>Nem támogatott szolgáltatások köré CTAS toowork használatával
`CTAS`számos alább felsorolt nem támogatott hello szolgáltatást körül használt toowork is lehet. Ez gyakran bizonyítja toobe win/win helyzet nem csak a kód lesznek megfelelőek, de ez gyakran végrehajtja a gyorsabb SQL Data warehouse. Ez egy, a teljes parallelized kialakításakor miatt. Dolgozhat körül CTAS a forgatókönyvek a következők:

* A frissítések ANSI ILLESZTÉSEK
* Törli a ANSI illesztések
* MERGE utasítást

> [!NOTE]
> Próbálja meg toothink "CTAS első". Ha úgy gondolja, hogy egy probléma segítségével megoldható `CTAS` akkor az általában hello legjobb módja tooapproach, - akkor is, ha emiatt több adatot ír.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>Update utasításokban ANSI illesztési váltja fel
Előfordulhat, hogy egy összetett frissítést, amelyhez csatlakozik, több mint két tábla ANSI való csatlakozás szintaxis tooperform hello használata a frissítés vagy törlés.

Képzelje el ezt a táblázatot kellett tooupdate:

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

hello eredeti lekérdezés előfordulhat, hogy rendelkezik kikeresi például ehhez hasonló:

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

Mivel az SQL Data Warehouse nem támogatja az ANSI társítások (JOIN) hello `FROM` záradékában egy `UPDATE` utasítás, nem lehet másolni a kódot keresztül némileg módosítás nélkül.

A kombinációját is használja a `CTAS` és egy implicit csatlakozás tooreplace ezt a kódot:

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

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI illesztési helyettesíti a törlési utasítást
Egyes esetekben hello legjobb módszer az adatok törlése az toouse `CTAS`. Ahelyett, hogy törli a hello adatot egyszerűen jelölje ki a kívánt tookeep hello adatokat. Ez különösen igaz `DELETE` utasítás szintaxisa csatlakoztatása, mivel az SQL Data Warehouse nem támogatja az ANSI illesztések hello ansi használó `FROM` záradékában egy `DELETE` utasítást.

A konvertált DELETE utasításban példát alább érhető el:

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

## <a name="replace-merge-statements"></a>Cserélje le a merge utasítások
Merge utasításokban cserélhetősége, legalább a részben a `CTAS`. A konszolidáció hello `INSERT` és hello `UPDATE` be egy utasítás. Minden olyan törölt rekord kellene zárni egy második utasításban toobe.

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

RENAME OBJECT dbo.[DimProduct]          too[DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  too[DimProduct];

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

Érdemes instinctively át kell telepítenie a kód tooa CTAS és lenne megfelelő. Van azonban egy rejtett probléma.

hello alábbira nem fed fel ugyanazt az eredményt hello:

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

Figyelje meg, győződjön meg arról, hogy hello oszlop "eredménye" előre hello adatok típusát és nullázhatóságának hello kifejezésben szereplő végzi. Ha még nem gondosan meg kell, ez is járhat toosubtle eltérésekre értékeket.

Próbálkozzon hello következő egy példa:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

hello tárolt eredmény értéke eltérő. Hello megőrzött értékét hello eredményoszlop használja más kifejezések hello hiba még jelentősebb válik.

![][1]

Ez különösen fontos az adatok áttelepítése. Annak ellenére, hogy hello második lekérdezés késései pontosabb probléma van. hello adatok különböző összehasonlított toohello forrásrendszerben és integritásának tooquestions hello áttelepítés eredményezi. Ez az egyik ezeket bizonyos ritkán előforduló esetekben, amikor hello "hibás" válasz ténylegesen hello jobbról egy!

hello oka a hello két eredmények eltérései látható, lefelé tooimplicit adattípusokról írja be. Első példa hello tábla hello határozza meg a hello oszlop definíciójában. Az implicit típuskonverzió átalakítás akkor fordul elő, amikor hello sor kerül. Hello második példában nincs nincs implicit konvertálása, hello kifejezés határozza meg a hello oszlop adattípusát. Figyelje meg is, hogy hello oszlop hello második példában definiálva van nullázható oszlopot, mivel az első példában hello nem. Hello tábla létrehozásakor hello első példa oszlop nullázhatósági explicit módon van definiálva. Hello második példában az lett csak elhagyta toohello kifejezés, és alapértelmezés szerint ez eredményeznének NULL definícióját.  

Ezek kapcsolatban, hogy tooresolve explicit módon be kell hello konvertálásához és nullázhatóságának hello `SELECT` hello része `CTAS` utasítást. Nem állítható be a tulajdonságokat az hello létrehozni tábla.

hello az alábbi példa bemutatja, hogyan toofix hello kódot:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Vegye figyelembe a következőket hello:

* CAST vagy CONVERT sikerült fel lett használva
* ISNULL használt tooforce nullázhatósági nem COALESCE
* ISNULL hello legkülső függvény
* hello második hello ISNULL része egy állandó például 0

> [!NOTE]
> Hello nullázhatósági toobe megfelelően beállítani az létfontosságú toouse `ISNULL` és nem `COALESCE`. `COALESCE`nem determinisztikus-tól, és így hello hello kifejezés eredménye mindig lesz NULLable. `ISNULL`nem egyezik. Determinisztikus. Ezért ha hello hello második része `ISNULL` függvény egy állandó vagy -szövegkonstans, akkor a hello az eredményül kapott érték nem null értékű.
> 
> 

Ez tipp nincs csak akkor hasznos, a számítások hello integritásának biztosításához. Fontos továbbá tábla partíciós átállításához. Képzelje el ezt a táblázatot a tény definiálva van:

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

Azonban hello érték mező nem egy része nem hello forrásadatok számított kifejezés.

toocreate a particionált adatkészlet-érdemes toodo ezt:

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

hello lekérdezés tökéletesen finom fog futni. hello probléma tooperform hello partíció kapcsolójának meg előre. hello definíciói nem egyeznek. toomake hello definíciói hello CTAS toobe módosítani kell egyeznie.

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

Látható ezért, hogy típus konzisztencia és karbantartása a CTAS nullázhatósági tulajdonságainak jó mérnöki ajánlott. Segít a számítások toomaintain integritását, és is biztosítja, hogy a partíció váltás lehetséges.

Tekintse meg a további információk az tooMSDN [CTAS][CTAS]. Hello legfontosabb utasítások az Azure SQL Data Warehouse egyike. Győződjön meg arról, hogy alaposan ismertetése.

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
