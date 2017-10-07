---
title: "az SQL Data Warehouse aaaOptimizing tranzakciók |} Microsoft Docs"
description: "Az Azure SQL Data Warehouse hatékony tranzakció frissítések írásáról bevált gyakorlatokat tartalmazó útmutatóval"
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 6f326f26-8a54-49df-a482-9c96a58db371
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 1a821161711db9460b7e10d3cf7ba498d711448b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Az SQL Data Warehouse tranzakciók optimalizálása
Ez a cikk azt ismerteti, hogyan toooptimize hello teljesítmény, a tranzakciós kód, ugyanakkor minimalizálja a hosszú visszagörgetése kockázatát.

## <a name="transactions-and-logging"></a>Tranzakciók és a naplózás
-Tranzakciók le egy relációs adatbázis-kezelő fontos része. Az SQL Data Warehouse tranzakciók adatok módosítása során használ. Ezek a tranzakciók explicit vagy implicit lehet. Egyetlen `INSERT`, `UPDATE` és `DELETE` utasítások valamennyien implicit tranzakciók. Az explicit tranzakciók írták explicit módon a fejlesztők használatával `BEGIN TRAN`, `COMMIT TRAN` vagy `ROLLBACK TRAN` és általában használata, amikor több módosítási utasítást kell toobe egyetlen atomi egységben együtt kötődik. 

Az SQL Data Warehouse véglegesíti a módosításokat toohello adatbázis tranzakciós naplók segítségével. Minden terjesztési saját tranzakciónapló rendelkezik. Tranzakciós napló írása automatikus. Nincs szükség konfigurálásra. Bár ez a folyamat biztosítja, hogy az hello írási azt hello rendszer egy általános vezethet. A hatás minimalizálhatja tranzakciós úton hatékony kód megírásával. Tranzakciós úton hatékony kód nagyjából két kategóriába esik.

* Használja ki a minimális naplózás szerkezeteket intézkedéseinek lehetőség szerinti felhasználása
* Folyamat adatok hatókörön belüli kötegek tooavoid szinguláris hosszú ideig futó tranzakció használatával
* Egy adott partíció nagy módosítások tooa mintát a partícióváltás elfogadása

## <a name="minimal-vs-full-logging"></a>A minimális és a teljes körű naplózás
Ellentétben a teljes naplózott műveleteknek, amelyek hello tranzakciós napló tookeep nyomon minden sor változás, minimálisan naplózott műveleteknek nyomon követheti, mértékben kiosztásokat és csak a metaadatok módosításokat. Ezért minimális naplózás magában foglalja a csak a szükséges toorollback hello tranzakció hello eseményben hibát vagy egy explicit kérelem hello információk naplózása (`ROLLBACK TRAN`). Sokkal kevesebb adatot hello naplóban rögzíti, mert egy minimálisan naplózott műveletnek jobb, mint egy hasonlóképpen méretű teljesen naplózott műveletet hajt végre. Továbbá mert kevesebb írások hello tranzakciónapló halad, naplóadatokat sokkal kisebb mennyiségű jön létre és több, i/o hatékony.

hello tranzakció biztonsági a határértékeket toofully naplózott műveletek.

> [!NOTE]
> Minimálisan naplózott műveleteknek az explicit tranzakciók részt. Foglalási struktúrában összes változások nyomon követése, mivel már lehetséges tooroll vissza minimálisan műveletek a naplóba. Fontos, amely hello módosítása "minimálisan" toounderstand naplózza, akkor nincs nem naplózott.
> 
> 

## <a name="minimally-logged-operations"></a>Minimálisan naplózott műveleteknek
hello következő operations képesek minimálisan naplózott:

* HOZZON LÉTRE TABLE AS SELECT ([CTAS][CTAS])
* INSERT... VÁLASSZA KI
* INDEX LÉTREHOZÁSA
* ALTER INDEX REBUILD
* A DROP INDEX
* A TRUNCATE TABLE
* KÖZVETLEN TÁBLA
* ALTER TABLE SWITCH PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> Belső az adatátviteli műveletek (például `BROADCAST` és `SHUFFLE`) nem érinti a hello tranzakció biztonsági korlátot.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>A tömeges betöltés minimális naplózás
`CTAS`és `INSERT...SELECT` vannak mindkét tömeges betöltési műveletek. Azonban mind hello cél tábladefiníció befolyásolják, és hello terhelés forgatókönyv függenek. Az alábbiakban egy táblát, amely azt ismerteti, ha a csoportos művelet teljes mértékben vagy minimálisan naplóz a következő:  

| Elsődleges indexe | Betöltési forgatókönyv | A naplózási |
| --- | --- | --- |
| Halommemória |Bármelyik |**Minimális** |
| Fürtözött Index |Üres céltábla |**Minimális** |
| Fürtözött Index |Betöltött sorok nem lehetnek meglévő lapokon a célkiszolgálón |**Minimális** |
| Fürtözött Index |A betöltött sorok átfedésben vannak a meglévő lapokon a célkiszolgálón |Korlátlan |
| Fürtözött Oszlopcentrikus Index |Kötegméret > = 102,400 igazítva partíció eloszlása |**Minimális** |
| Fürtözött Oszlopcentrikus Index |A Batch-méret < 102,400 igazítva partíció eloszlása |Korlátlan |

Érdemes megjegyezni, hogy bármilyen tooupdate másodlagos vagy nem fürtözött indexek mindig lesz teljes mértékben írás a naplóba műveletek.

> [!IMPORTANT]
> Az SQL Data Warehouse 60 terjesztéseket rendelkezik. Ezért feltéve, hogy minden sor egyenletesen és üzenetsorokra egy olyan partíciót, a kötegelt kell toocontain 6,144,000 sort vagy nagyobb toobe minimálisan naplózott tooa fürtözött Oszlopcentrikus Index írásakor. Ha hello sort beszúrni span partícióhatárok hello tábla particionálva van, akkor szüksége lesz egy partíció határ, feltéve, hogy még az adatok terjesztési 6,144,000 sorok. Minden egyes partíciók az egyes terjesztési egymástól függetlenül haladhatja meg a hello insert toobe minimálisan bejelentkezik hello terjesztési hello 102 400 sor küszöbértéket.
> 
> 

Adatok betöltése az egy fürtözött index nem üres táblába gyakran tartalmaznak teljes mértékben naplózott és minimálisan naplózott sorok keverékével. Egy fürtözött index egy elosztott terhelésű fa (b-fa) lap. Ha hello lap írása tooalready egy másik tranzakció azon sorait tartalmazza, majd ezek írási műveletek teljes naplóz. Azonban ha hello lap üres majd hello írási toothat lap minimálisan naplóz.

## <a name="optimizing-deletes"></a>Törlések optimalizálása
`DELETE`egy olyan teljes naplózott művelet.  Ha egy nagy mennyiségű adatot táblázat vagy partíció toodelete van szüksége, gyakran érdemes további túl`SELECT` hello adatokat kívánja tookeep, ami minimálisan naplózott műveletet is lehet futtatni.  tooaccomplish, hozzon létre egy új tábla [CTAS][CTAS].  Létrehozása után használja [átnevezése] [ RENAME] tooswap el a régi táblázat és az újonnan létrehozott hello tábla.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only hello records we want tookep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename hello Tables tooreplace hello 
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] too[FactInternetSales];
```

## <a name="optimizing-updates"></a>Frissítések optimalizálása
`UPDATE`egy olyan teljes naplózott művelet.  Ha nagyszámú egy tábla sorainak tooupdate szüksége lehet egy vagy partíció gyakran sokkal hatékonyabb toouse, mint a minimálisan naplózott műveletnek [CTAS] [ CTAS] toodo így.

A hello a teljes táblázat frissítése az alábbi példában konvertált tooa új `CTAS` úgy, hogy minimális naplózás lehetséges.

Ebben az esetben azt utólag ad hozzá egy kedvezményes toohello értékesítés hello táblázatban:

```sql
--Step 01. Create a new table containing hello "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales]   too[FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] too[FactInternetSales];

--Step 03. Drop hello old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Hozza létre újra nagy táblák is előnyt az SQL Data Warehouse munkaterhelés felügyeleti funkciókat. A részletekért tekintse meg a toohello munkaterhelés felügyeleti szakasz hello [egyidejűségi] [ concurrency] cikk.
> 
> 

## <a name="optimizing-with-partition-switching"></a>Partíció váltás optimalizálása
Nagy méretű módosítások belül szemben egy [tábla partíciós][table partition], majd egy minta a partícióváltás lehetővé teszi nagy mennyiségű logika. Ha hello jelentős adatok módosítását, és több partíciót, majd egyszerűen léptetés hello partíciók keresztül éri el is lefedik hello ugyanazt az eredményt.

hello lépéseket tooperform táblaváltoztatásokat a következők:

1. Hozzon létre egy partíciót ki üres
2. Hajtsa végre a "hello"frissítés", a CTAS
3. Kimenő hello meglévő adatok toohello kimenő tábla Váltás
4. Új adatok hello kapcsoló
5. Hello adatok törlése

Azonban a toohelp hello partíciók tooswitch azt először engedélyeznie kell a toobuild például egy hello segítő eljárásról az alábbi azonosításához. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Ez az eljárás kód újbóli használata a lehető legnagyobbra növeli, és tartja hello partícióváltás tömörebb példa.

az alábbi hello kód bemutatja, tooachieve fent említett öt lépése hello teljes partícióváltás rutin.

```sql
--Create a partitioned aligned empty table tooswitch out hello data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update hello data in hello select portion of hello CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use hello helper procedure tooidentify hello partitions
--hello source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--hello "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--hello "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch hello partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' too[dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' too[dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform hello clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Kis kötegek naplózási minimalizálása érdekében
Módosítási műveletek nagy akkor teheti logika toodivide hello művelet adattömbök vagy kötegek tooscope hello egységbe munka.

Egy működő példa lejjebb tekinthetők meg. hello kötegméret tooa trivial számú toohighlight hello módszer van beállítva. A valóságban hello köteg mérete jelentősen nagyobb lenne. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Pause és a méretezésről útmutató
Az SQL Data Warehouse lehetővé teszi szüneteltetése, folytatása, és az adatraktár igény szerint méretezheti. Felfüggesztése vagy az SQL Data Warehouse méretezése esetén fontos, hogy bármilyen az üzenetsoroktól tranzakciók leállítása van azonnal; toounderstand bármely nyitott tranzakciók toobe okozó visszaállítása. Ha a terhelést állított ki egy hosszú ideig futó és a nem teljes adatokat módosítás előzetes toohello szüneteltetése vagy skálázási művelet, majd a munkahelyi kell toobe vonható vissza. Ez hatással lehet a hello időt toopause, vagy az Azure SQL Data Warehouse-adatbázis méretezése. 

> [!IMPORTANT]
> Mindkét `UPDATE` és `DELETE` teljesen naplózott művelet, így ezek visszavonási vagy visszaállítási műveletek megfelelője minimálisan naplózott műveletek jelentősen tovább. 
> 
> 

hello legjobb forgatókönyv toolet felé továbbított adatok módosítását tranzakciók teljes előzetes toopausing vagy méretezési SQL Data Warehouse. Előfordulhat azonban, ez nem mindig gyakorlati. toomitigate hello kockázat hosszú visszaállítás történt, érdemes lehet hello alábbi beállítások egyikét:

* Újra írási hosszú ideig futó műveletek [CTAS][CTAS]
* Hello művelet lebontva adattömbökbe; hello sorok működő

## <a name="next-steps"></a>Következő lépések
Lásd: [az SQL Data Warehouse tranzakciók] [ Transactions in SQL Data Warehouse] toolearn további elkülönítési szinten és tranzakciós korlátok.  Egyéb ajánlott eljárások áttekintését lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Transactions in SQL Data Warehouse]: ./sql-data-warehouse-develop-transactions.md
[table partition]: ./sql-data-warehouse-tables-partition.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

