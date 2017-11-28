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
# <a name="optimizing-transactions-for-sql-data-warehouse"></a><span data-ttu-id="62486-103">Az SQL Data Warehouse tranzakciók optimalizálása</span><span class="sxs-lookup"><span data-stu-id="62486-103">Optimizing transactions for SQL Data Warehouse</span></span>
<span data-ttu-id="62486-104">Ez a cikk azt ismerteti, hogyan toooptimize hello teljesítmény, a tranzakciós kód, ugyanakkor minimalizálja a hosszú visszagörgetése kockázatát.</span><span class="sxs-lookup"><span data-stu-id="62486-104">This article explains how toooptimize hello performance of your transactional code while minimizing risk for long rollbacks.</span></span>

## <a name="transactions-and-logging"></a><span data-ttu-id="62486-105">Tranzakciók és a naplózás</span><span class="sxs-lookup"><span data-stu-id="62486-105">Transactions and logging</span></span>
<span data-ttu-id="62486-106">-Tranzakciók le egy relációs adatbázis-kezelő fontos része.</span><span class="sxs-lookup"><span data-stu-id="62486-106">Transactions are an important component of a relational database engine.</span></span> <span data-ttu-id="62486-107">Az SQL Data Warehouse tranzakciók adatok módosítása során használ.</span><span class="sxs-lookup"><span data-stu-id="62486-107">SQL Data Warehouse uses transactions during data modification.</span></span> <span data-ttu-id="62486-108">Ezek a tranzakciók explicit vagy implicit lehet.</span><span class="sxs-lookup"><span data-stu-id="62486-108">These transactions can be explicit or implicit.</span></span> <span data-ttu-id="62486-109">Egyetlen `INSERT`, `UPDATE` és `DELETE` utasítások valamennyien implicit tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="62486-109">Single `INSERT`, `UPDATE` and `DELETE` statements are all examples of implicit transactions.</span></span> <span data-ttu-id="62486-110">Az explicit tranzakciók írták explicit módon a fejlesztők használatával `BEGIN TRAN`, `COMMIT TRAN` vagy `ROLLBACK TRAN` és általában használata, amikor több módosítási utasítást kell toobe egyetlen atomi egységben együtt kötődik.</span><span class="sxs-lookup"><span data-stu-id="62486-110">Explicit transactions are written explicitly by a developer using `BEGIN TRAN`, `COMMIT TRAN` or `ROLLBACK TRAN` and are typically used when multiple modification statements need toobe tied together in a single atomic unit.</span></span> 

<span data-ttu-id="62486-111">Az SQL Data Warehouse véglegesíti a módosításokat toohello adatbázis tranzakciós naplók segítségével.</span><span class="sxs-lookup"><span data-stu-id="62486-111">Azure SQL Data Warehouse commits changes toohello database using transaction logs.</span></span> <span data-ttu-id="62486-112">Minden terjesztési saját tranzakciónapló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="62486-112">Each distribution has its own transaction log.</span></span> <span data-ttu-id="62486-113">Tranzakciós napló írása automatikus.</span><span class="sxs-lookup"><span data-stu-id="62486-113">Transaction log writes are automatic.</span></span> <span data-ttu-id="62486-114">Nincs szükség konfigurálásra.</span><span class="sxs-lookup"><span data-stu-id="62486-114">There is no configuration required.</span></span> <span data-ttu-id="62486-115">Bár ez a folyamat biztosítja, hogy az hello írási azt hello rendszer egy általános vezethet.</span><span class="sxs-lookup"><span data-stu-id="62486-115">However, whilst this process guarantees hello write it does introduce an overhead in hello system.</span></span> <span data-ttu-id="62486-116">A hatás minimalizálhatja tranzakciós úton hatékony kód megírásával.</span><span class="sxs-lookup"><span data-stu-id="62486-116">You can minimize this impact by writing transactionally efficient code.</span></span> <span data-ttu-id="62486-117">Tranzakciós úton hatékony kód nagyjából két kategóriába esik.</span><span class="sxs-lookup"><span data-stu-id="62486-117">Transactionally efficient code broadly falls into two categories.</span></span>

* <span data-ttu-id="62486-118">Használja ki a minimális naplózás szerkezeteket intézkedéseinek lehetőség szerinti felhasználása</span><span class="sxs-lookup"><span data-stu-id="62486-118">Leverage minimal logging constructs where possible</span></span>
* <span data-ttu-id="62486-119">Folyamat adatok hatókörön belüli kötegek tooavoid szinguláris hosszú ideig futó tranzakció használatával</span><span class="sxs-lookup"><span data-stu-id="62486-119">Process data using scoped batches tooavoid singular long running transactions</span></span>
* <span data-ttu-id="62486-120">Egy adott partíció nagy módosítások tooa mintát a partícióváltás elfogadása</span><span class="sxs-lookup"><span data-stu-id="62486-120">Adopt a partition switching pattern for large modifications tooa given partition</span></span>

## <a name="minimal-vs-full-logging"></a><span data-ttu-id="62486-121">A minimális és a teljes körű naplózás</span><span class="sxs-lookup"><span data-stu-id="62486-121">Minimal vs. full logging</span></span>
<span data-ttu-id="62486-122">Ellentétben a teljes naplózott műveleteknek, amelyek hello tranzakciós napló tookeep nyomon minden sor változás, minimálisan naplózott műveleteknek nyomon követheti, mértékben kiosztásokat és csak a metaadatok módosításokat.</span><span class="sxs-lookup"><span data-stu-id="62486-122">Unlike fully logged operations, which use hello transaction log tookeep track of every row change, minimally logged operations keep track of extent allocations and meta-data changes only.</span></span> <span data-ttu-id="62486-123">Ezért minimális naplózás magában foglalja a csak a szükséges toorollback hello tranzakció hello eseményben hibát vagy egy explicit kérelem hello információk naplózása (`ROLLBACK TRAN`).</span><span class="sxs-lookup"><span data-stu-id="62486-123">Therefore, minimal logging involves logging only hello information that is required toorollback hello transaction in hello event of a failure or an explicit request (`ROLLBACK TRAN`).</span></span> <span data-ttu-id="62486-124">Sokkal kevesebb adatot hello naplóban rögzíti, mert egy minimálisan naplózott műveletnek jobb, mint egy hasonlóképpen méretű teljesen naplózott műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="62486-124">As much less information is tracked in hello transaction log, a minimally logged operation performs better than a similarly sized fully logged operation.</span></span> <span data-ttu-id="62486-125">Továbbá mert kevesebb írások hello tranzakciónapló halad, naplóadatokat sokkal kisebb mennyiségű jön létre és több, i/o hatékony.</span><span class="sxs-lookup"><span data-stu-id="62486-125">Furthermore, because fewer writes go hello transaction log, a much smaller amount of log data is generated and so is more I/O efficient.</span></span>

<span data-ttu-id="62486-126">hello tranzakció biztonsági a határértékeket toofully naplózott műveletek.</span><span class="sxs-lookup"><span data-stu-id="62486-126">hello transaction safety limits only apply toofully logged operations.</span></span>

> [!NOTE]
> <span data-ttu-id="62486-127">Minimálisan naplózott műveleteknek az explicit tranzakciók részt.</span><span class="sxs-lookup"><span data-stu-id="62486-127">Minimally logged operations can participate in explicit transactions.</span></span> <span data-ttu-id="62486-128">Foglalási struktúrában összes változások nyomon követése, mivel már lehetséges tooroll vissza minimálisan műveletek a naplóba.</span><span class="sxs-lookup"><span data-stu-id="62486-128">As all changes in allocation structures are tracked, it is possible tooroll back minimally logged operations.</span></span> <span data-ttu-id="62486-129">Fontos, amely hello módosítása "minimálisan" toounderstand naplózza, akkor nincs nem naplózott.</span><span class="sxs-lookup"><span data-stu-id="62486-129">It is important toounderstand that hello change is "minimally" logged it is not un-logged.</span></span>
> 
> 

## <a name="minimally-logged-operations"></a><span data-ttu-id="62486-130">Minimálisan naplózott műveleteknek</span><span class="sxs-lookup"><span data-stu-id="62486-130">Minimally logged operations</span></span>
<span data-ttu-id="62486-131">hello következő operations képesek minimálisan naplózott:</span><span class="sxs-lookup"><span data-stu-id="62486-131">hello following operations are capable of being minimally logged:</span></span>

* <span data-ttu-id="62486-132">HOZZON LÉTRE TABLE AS SELECT ([CTAS][CTAS])</span><span class="sxs-lookup"><span data-stu-id="62486-132">CREATE TABLE AS SELECT ([CTAS][CTAS])</span></span>
* <span data-ttu-id="62486-133">INSERT... VÁLASSZA KI</span><span class="sxs-lookup"><span data-stu-id="62486-133">INSERT..SELECT</span></span>
* <span data-ttu-id="62486-134">INDEX LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="62486-134">CREATE INDEX</span></span>
* <span data-ttu-id="62486-135">ALTER INDEX REBUILD</span><span class="sxs-lookup"><span data-stu-id="62486-135">ALTER INDEX REBUILD</span></span>
* <span data-ttu-id="62486-136">A DROP INDEX</span><span class="sxs-lookup"><span data-stu-id="62486-136">DROP INDEX</span></span>
* <span data-ttu-id="62486-137">A TRUNCATE TABLE</span><span class="sxs-lookup"><span data-stu-id="62486-137">TRUNCATE TABLE</span></span>
* <span data-ttu-id="62486-138">KÖZVETLEN TÁBLA</span><span class="sxs-lookup"><span data-stu-id="62486-138">DROP TABLE</span></span>
* <span data-ttu-id="62486-139">ALTER TABLE SWITCH PARTITION</span><span class="sxs-lookup"><span data-stu-id="62486-139">ALTER TABLE SWITCH PARTITION</span></span>

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> <span data-ttu-id="62486-140">Belső az adatátviteli műveletek (például `BROADCAST` és `SHUFFLE`) nem érinti a hello tranzakció biztonsági korlátot.</span><span class="sxs-lookup"><span data-stu-id="62486-140">Internal data movement operations (such as `BROADCAST` and `SHUFFLE`) are not affected by hello transaction safety limit.</span></span>
> 
> 

## <a name="minimal-logging-with-bulk-load"></a><span data-ttu-id="62486-141">A tömeges betöltés minimális naplózás</span><span class="sxs-lookup"><span data-stu-id="62486-141">Minimal logging with bulk load</span></span>
<span data-ttu-id="62486-142">`CTAS`és `INSERT...SELECT` vannak mindkét tömeges betöltési műveletek.</span><span class="sxs-lookup"><span data-stu-id="62486-142">`CTAS` and `INSERT...SELECT` are both bulk load operations.</span></span> <span data-ttu-id="62486-143">Azonban mind hello cél tábladefiníció befolyásolják, és hello terhelés forgatókönyv függenek.</span><span class="sxs-lookup"><span data-stu-id="62486-143">However, both are influenced by hello target table definition and depend on hello load scenario.</span></span> <span data-ttu-id="62486-144">Az alábbiakban egy táblát, amely azt ismerteti, ha a csoportos művelet teljes mértékben vagy minimálisan naplóz a következő:</span><span class="sxs-lookup"><span data-stu-id="62486-144">Below is a table that explains if your bulk operation will be fully or minimally logged:</span></span>  

| <span data-ttu-id="62486-145">Elsődleges indexe</span><span class="sxs-lookup"><span data-stu-id="62486-145">Primary Index</span></span> | <span data-ttu-id="62486-146">Betöltési forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="62486-146">Load Scenario</span></span> | <span data-ttu-id="62486-147">A naplózási</span><span class="sxs-lookup"><span data-stu-id="62486-147">Logging Mode</span></span> |
| --- | --- | --- |
| <span data-ttu-id="62486-148">Halommemória</span><span class="sxs-lookup"><span data-stu-id="62486-148">Heap</span></span> |<span data-ttu-id="62486-149">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="62486-149">Any</span></span> |<span data-ttu-id="62486-150">**Minimális**</span><span class="sxs-lookup"><span data-stu-id="62486-150">**Minimal**</span></span> |
| <span data-ttu-id="62486-151">Fürtözött Index</span><span class="sxs-lookup"><span data-stu-id="62486-151">Clustered Index</span></span> |<span data-ttu-id="62486-152">Üres céltábla</span><span class="sxs-lookup"><span data-stu-id="62486-152">Empty target table</span></span> |<span data-ttu-id="62486-153">**Minimális**</span><span class="sxs-lookup"><span data-stu-id="62486-153">**Minimal**</span></span> |
| <span data-ttu-id="62486-154">Fürtözött Index</span><span class="sxs-lookup"><span data-stu-id="62486-154">Clustered Index</span></span> |<span data-ttu-id="62486-155">Betöltött sorok nem lehetnek meglévő lapokon a célkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="62486-155">Loaded rows do not overlap with existing pages in target</span></span> |<span data-ttu-id="62486-156">**Minimális**</span><span class="sxs-lookup"><span data-stu-id="62486-156">**Minimal**</span></span> |
| <span data-ttu-id="62486-157">Fürtözött Index</span><span class="sxs-lookup"><span data-stu-id="62486-157">Clustered Index</span></span> |<span data-ttu-id="62486-158">A betöltött sorok átfedésben vannak a meglévő lapokon a célkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="62486-158">Loaded rows overlap with existing pages in target</span></span> |<span data-ttu-id="62486-159">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="62486-159">Full</span></span> |
| <span data-ttu-id="62486-160">Fürtözött Oszlopcentrikus Index</span><span class="sxs-lookup"><span data-stu-id="62486-160">Clustered Columnstore Index</span></span> |<span data-ttu-id="62486-161">Kötegméret > = 102,400 igazítva partíció eloszlása</span><span class="sxs-lookup"><span data-stu-id="62486-161">Batch size >= 102,400 per partition aligned distribution</span></span> |<span data-ttu-id="62486-162">**Minimális**</span><span class="sxs-lookup"><span data-stu-id="62486-162">**Minimal**</span></span> |
| <span data-ttu-id="62486-163">Fürtözött Oszlopcentrikus Index</span><span class="sxs-lookup"><span data-stu-id="62486-163">Clustered Columnstore Index</span></span> |<span data-ttu-id="62486-164">A Batch-méret < 102,400 igazítva partíció eloszlása</span><span class="sxs-lookup"><span data-stu-id="62486-164">Batch size < 102,400 per partition aligned distribution</span></span> |<span data-ttu-id="62486-165">Korlátlan</span><span class="sxs-lookup"><span data-stu-id="62486-165">Full</span></span> |

<span data-ttu-id="62486-166">Érdemes megjegyezni, hogy bármilyen tooupdate másodlagos vagy nem fürtözött indexek mindig lesz teljes mértékben írás a naplóba műveletek.</span><span class="sxs-lookup"><span data-stu-id="62486-166">It is worth noting that any writes tooupdate secondary or non-clustered indexes will always be fully logged operations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62486-167">Az SQL Data Warehouse 60 terjesztéseket rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="62486-167">SQL Data Warehouse has 60 distributions.</span></span> <span data-ttu-id="62486-168">Ezért feltéve, hogy minden sor egyenletesen és üzenetsorokra egy olyan partíciót, a kötegelt kell toocontain 6,144,000 sort vagy nagyobb toobe minimálisan naplózott tooa fürtözött Oszlopcentrikus Index írásakor.</span><span class="sxs-lookup"><span data-stu-id="62486-168">Therefore, assuming all rows are evenly distributed and landing in a single partition, your batch will need toocontain 6,144,000 rows or larger toobe minimally logged when writing tooa Clustered Columnstore Index.</span></span> <span data-ttu-id="62486-169">Ha hello sort beszúrni span partícióhatárok hello tábla particionálva van, akkor szüksége lesz egy partíció határ, feltéve, hogy még az adatok terjesztési 6,144,000 sorok.</span><span class="sxs-lookup"><span data-stu-id="62486-169">If hello table is partitioned and hello rows being inserted span partition boundaries, then you will need 6,144,000 rows per partition boundary assuming even data distribution.</span></span> <span data-ttu-id="62486-170">Minden egyes partíciók az egyes terjesztési egymástól függetlenül haladhatja meg a hello insert toobe minimálisan bejelentkezik hello terjesztési hello 102 400 sor küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="62486-170">Each partition in each distribution must independently exceed hello 102,400 row threshold for hello insert toobe minimally logged into hello distribution.</span></span>
> 
> 

<span data-ttu-id="62486-171">Adatok betöltése az egy fürtözött index nem üres táblába gyakran tartalmaznak teljes mértékben naplózott és minimálisan naplózott sorok keverékével.</span><span class="sxs-lookup"><span data-stu-id="62486-171">Loading data into a non-empty table with a clustered index can often contain a mixture of fully logged and minimally logged rows.</span></span> <span data-ttu-id="62486-172">Egy fürtözött index egy elosztott terhelésű fa (b-fa) lap.</span><span class="sxs-lookup"><span data-stu-id="62486-172">A clustered index is a balanced tree (b-tree) of pages.</span></span> <span data-ttu-id="62486-173">Ha hello lap írása tooalready egy másik tranzakció azon sorait tartalmazza, majd ezek írási műveletek teljes naplóz.</span><span class="sxs-lookup"><span data-stu-id="62486-173">If hello page being written tooalready contains rows from another transaction, then these writes will be fully logged.</span></span> <span data-ttu-id="62486-174">Azonban ha hello lap üres majd hello írási toothat lap minimálisan naplóz.</span><span class="sxs-lookup"><span data-stu-id="62486-174">However, if hello page is empty then hello write toothat page will be minimally logged.</span></span>

## <a name="optimizing-deletes"></a><span data-ttu-id="62486-175">Törlések optimalizálása</span><span class="sxs-lookup"><span data-stu-id="62486-175">Optimizing deletes</span></span>
<span data-ttu-id="62486-176">`DELETE`egy olyan teljes naplózott művelet.</span><span class="sxs-lookup"><span data-stu-id="62486-176">`DELETE` is a fully logged operation.</span></span>  <span data-ttu-id="62486-177">Ha egy nagy mennyiségű adatot táblázat vagy partíció toodelete van szüksége, gyakran érdemes további túl`SELECT` hello adatokat kívánja tookeep, ami minimálisan naplózott műveletet is lehet futtatni.</span><span class="sxs-lookup"><span data-stu-id="62486-177">If you need toodelete a large amount of data in a table or a partition, it often makes more sense too`SELECT` hello data you wish tookeep, which can be run as a minimally logged operation.</span></span>  <span data-ttu-id="62486-178">tooaccomplish, hozzon létre egy új tábla [CTAS][CTAS].</span><span class="sxs-lookup"><span data-stu-id="62486-178">tooaccomplish this, create a new table with [CTAS][CTAS].</span></span>  <span data-ttu-id="62486-179">Létrehozása után használja [átnevezése] [ RENAME] tooswap el a régi táblázat és az újonnan létrehozott hello tábla.</span><span class="sxs-lookup"><span data-stu-id="62486-179">Once created, use [RENAME][RENAME] tooswap out your old table with hello newly created table.</span></span>

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

## <a name="optimizing-updates"></a><span data-ttu-id="62486-180">Frissítések optimalizálása</span><span class="sxs-lookup"><span data-stu-id="62486-180">Optimizing updates</span></span>
<span data-ttu-id="62486-181">`UPDATE`egy olyan teljes naplózott művelet.</span><span class="sxs-lookup"><span data-stu-id="62486-181">`UPDATE` is a fully logged operation.</span></span>  <span data-ttu-id="62486-182">Ha nagyszámú egy tábla sorainak tooupdate szüksége lehet egy vagy partíció gyakran sokkal hatékonyabb toouse, mint a minimálisan naplózott műveletnek [CTAS] [ CTAS] toodo így.</span><span class="sxs-lookup"><span data-stu-id="62486-182">If you need tooupdate a large number of rows in a table or a partition it can often be far more efficient toouse a minimally logged operation such as [CTAS][CTAS] toodo so.</span></span>

<span data-ttu-id="62486-183">A hello a teljes táblázat frissítése az alábbi példában konvertált tooa új `CTAS` úgy, hogy minimális naplózás lehetséges.</span><span class="sxs-lookup"><span data-stu-id="62486-183">In hello example below a full table update has been converted tooa `CTAS` so that minimal logging is possible.</span></span>

<span data-ttu-id="62486-184">Ebben az esetben azt utólag ad hozzá egy kedvezményes toohello értékesítés hello táblázatban:</span><span class="sxs-lookup"><span data-stu-id="62486-184">In this case we are retrospectively adding a discount amount toohello sales in hello table:</span></span>

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
> <span data-ttu-id="62486-185">Hozza létre újra nagy táblák is előnyt az SQL Data Warehouse munkaterhelés felügyeleti funkciókat.</span><span class="sxs-lookup"><span data-stu-id="62486-185">Re-creating large tables can benefit from using SQL Data Warehouse workload management features.</span></span> <span data-ttu-id="62486-186">A részletekért tekintse meg a toohello munkaterhelés felügyeleti szakasz hello [egyidejűségi] [ concurrency] cikk.</span><span class="sxs-lookup"><span data-stu-id="62486-186">For more details please refer toohello workload management section in hello [concurrency][concurrency] article.</span></span>
> 
> 

## <a name="optimizing-with-partition-switching"></a><span data-ttu-id="62486-187">Partíció váltás optimalizálása</span><span class="sxs-lookup"><span data-stu-id="62486-187">Optimizing with partition switching</span></span>
<span data-ttu-id="62486-188">Nagy méretű módosítások belül szemben egy [tábla partíciós][table partition], majd egy minta a partícióváltás lehetővé teszi nagy mennyiségű logika.</span><span class="sxs-lookup"><span data-stu-id="62486-188">When faced with large scale modifications inside a [table partition][table partition], then a partition switching pattern makes a lot of sense.</span></span> <span data-ttu-id="62486-189">Ha hello jelentős adatok módosítását, és több partíciót, majd egyszerűen léptetés hello partíciók keresztül éri el is lefedik hello ugyanazt az eredményt.</span><span class="sxs-lookup"><span data-stu-id="62486-189">If hello data modification is significant and spans multiple partitions, then simply iterating over hello partitions achieves hello same result.</span></span>

<span data-ttu-id="62486-190">hello lépéseket tooperform táblaváltoztatásokat a következők:</span><span class="sxs-lookup"><span data-stu-id="62486-190">hello steps tooperform a partition switch are as follows:</span></span>

1. <span data-ttu-id="62486-191">Hozzon létre egy partíciót ki üres</span><span class="sxs-lookup"><span data-stu-id="62486-191">Create an empty out partition</span></span>
2. <span data-ttu-id="62486-192">Hajtsa végre a "hello"frissítés", a CTAS</span><span class="sxs-lookup"><span data-stu-id="62486-192">Perform hello 'update' as a CTAS</span></span>
3. <span data-ttu-id="62486-193">Kimenő hello meglévő adatok toohello kimenő tábla Váltás</span><span class="sxs-lookup"><span data-stu-id="62486-193">Switch out hello existing data toohello out table</span></span>
4. <span data-ttu-id="62486-194">Új adatok hello kapcsoló</span><span class="sxs-lookup"><span data-stu-id="62486-194">Switch in hello new data</span></span>
5. <span data-ttu-id="62486-195">Hello adatok törlése</span><span class="sxs-lookup"><span data-stu-id="62486-195">Clean up hello data</span></span>

<span data-ttu-id="62486-196">Azonban a toohelp hello partíciók tooswitch azt először engedélyeznie kell a toobuild például egy hello segítő eljárásról az alábbi azonosításához.</span><span class="sxs-lookup"><span data-stu-id="62486-196">However, toohelp identify hello partitions tooswitch we will first need toobuild a helper procedure such as hello one below.</span></span> 

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

<span data-ttu-id="62486-197">Ez az eljárás kód újbóli használata a lehető legnagyobbra növeli, és tartja hello partícióváltás tömörebb példa.</span><span class="sxs-lookup"><span data-stu-id="62486-197">This procedure maximizes code re-use and keeps hello partition switching example more compact.</span></span>

<span data-ttu-id="62486-198">az alábbi hello kód bemutatja, tooachieve fent említett öt lépése hello teljes partícióváltás rutin.</span><span class="sxs-lookup"><span data-stu-id="62486-198">hello code below demonstrates hello five steps mentioned above tooachieve a full partition switching routine.</span></span>

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

## <a name="minimize-logging-with-small-batches"></a><span data-ttu-id="62486-199">Kis kötegek naplózási minimalizálása érdekében</span><span class="sxs-lookup"><span data-stu-id="62486-199">Minimize logging with small batches</span></span>
<span data-ttu-id="62486-200">Módosítási műveletek nagy akkor teheti logika toodivide hello művelet adattömbök vagy kötegek tooscope hello egységbe munka.</span><span class="sxs-lookup"><span data-stu-id="62486-200">For large data modification operations, it may make sense toodivide hello operation into chunks or batches tooscope hello unit of work.</span></span>

<span data-ttu-id="62486-201">Egy működő példa lejjebb tekinthetők meg.</span><span class="sxs-lookup"><span data-stu-id="62486-201">A working example is provided below.</span></span> <span data-ttu-id="62486-202">hello kötegméret tooa trivial számú toohighlight hello módszer van beállítva.</span><span class="sxs-lookup"><span data-stu-id="62486-202">hello batch size has been set tooa trivial number toohighlight hello technique.</span></span> <span data-ttu-id="62486-203">A valóságban hello köteg mérete jelentősen nagyobb lenne.</span><span class="sxs-lookup"><span data-stu-id="62486-203">In reality hello batch size would be significantly larger.</span></span> 

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

## <a name="pause-and-scaling-guidance"></a><span data-ttu-id="62486-204">Pause és a méretezésről útmutató</span><span class="sxs-lookup"><span data-stu-id="62486-204">Pause and scaling guidance</span></span>
<span data-ttu-id="62486-205">Az SQL Data Warehouse lehetővé teszi szüneteltetése, folytatása, és az adatraktár igény szerint méretezheti.</span><span class="sxs-lookup"><span data-stu-id="62486-205">Azure SQL Data Warehouse lets you pause, resume and scale your data warehouse on demand.</span></span> <span data-ttu-id="62486-206">Felfüggesztése vagy az SQL Data Warehouse méretezése esetén fontos, hogy bármilyen az üzenetsoroktól tranzakciók leállítása van azonnal; toounderstand bármely nyitott tranzakciók toobe okozó visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="62486-206">When you pause or scale your SQL Data Warehouse it is important toounderstand that any in-flight transactions are terminated immediately; causing any open transactions toobe rolled back.</span></span> <span data-ttu-id="62486-207">Ha a terhelést állított ki egy hosszú ideig futó és a nem teljes adatokat módosítás előzetes toohello szüneteltetése vagy skálázási művelet, majd a munkahelyi kell toobe vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="62486-207">If your workload had issued a long running and incomplete data modification prior toohello pause or scale operation, then this work will need toobe undone.</span></span> <span data-ttu-id="62486-208">Ez hatással lehet a hello időt toopause, vagy az Azure SQL Data Warehouse-adatbázis méretezése.</span><span class="sxs-lookup"><span data-stu-id="62486-208">This may impact hello time it takes toopause or scale your Azure SQL Data Warehouse database.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="62486-209">Mindkét `UPDATE` és `DELETE` teljesen naplózott művelet, így ezek visszavonási vagy visszaállítási műveletek megfelelője minimálisan naplózott műveletek jelentősen tovább.</span><span class="sxs-lookup"><span data-stu-id="62486-209">Both `UPDATE` and `DELETE` are fully logged operations and so these undo/redo operations can take significantly longer than equivalent minimally logged operations.</span></span> 
> 
> 

<span data-ttu-id="62486-210">hello legjobb forgatókönyv toolet felé továbbított adatok módosítását tranzakciók teljes előzetes toopausing vagy méretezési SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="62486-210">hello best scenario is toolet in flight data modification transactions complete prior toopausing or scaling SQL Data Warehouse.</span></span> <span data-ttu-id="62486-211">Előfordulhat azonban, ez nem mindig gyakorlati.</span><span class="sxs-lookup"><span data-stu-id="62486-211">However, this may not always be practical.</span></span> <span data-ttu-id="62486-212">toomitigate hello kockázat hosszú visszaállítás történt, érdemes lehet hello alábbi beállítások egyikét:</span><span class="sxs-lookup"><span data-stu-id="62486-212">toomitigate hello risk of a long rollback, consider one of hello following options:</span></span>

* <span data-ttu-id="62486-213">Újra írási hosszú ideig futó műveletek [CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="62486-213">Re-write long running operations using [CTAS][CTAS]</span></span>
* <span data-ttu-id="62486-214">Hello művelet lebontva adattömbökbe; hello sorok működő</span><span class="sxs-lookup"><span data-stu-id="62486-214">Break hello operation down into chunks; operating on a subset of hello rows</span></span>

## <a name="next-steps"></a><span data-ttu-id="62486-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62486-215">Next steps</span></span>
<span data-ttu-id="62486-216">Lásd: [az SQL Data Warehouse tranzakciók] [ Transactions in SQL Data Warehouse] toolearn további elkülönítési szinten és tranzakciós korlátok.</span><span class="sxs-lookup"><span data-stu-id="62486-216">See [Transactions in SQL Data Warehouse][Transactions in SQL Data Warehouse] toolearn more about isolation levels and transactional limits.</span></span>  <span data-ttu-id="62486-217">Egyéb ajánlott eljárások áttekintését lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="62486-217">For an overview of other Best Practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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

