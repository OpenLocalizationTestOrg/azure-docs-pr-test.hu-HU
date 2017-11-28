---
title: "Útmutató a aaaDesign replikált táblák - Azure SQL Data Warehouse |} Microsoft Docs"
description: "Tervezésével kapcsolatos ajánlások replikált táblák az Azure SQL Data Warehouse sémában."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 5d405b8c404c65177b387ba959126839c1cf8799
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a><span data-ttu-id="edae0-103">Útmutató a replikált táblák használata az Azure SQL Data Warehouse tervezése</span><span class="sxs-lookup"><span data-stu-id="edae0-103">Design guidance for using replicated tables in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="edae0-104">Ez a cikk tervezése során az SQL Data Warehouse-sémát a replikált táblák ajánlásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="edae0-104">This article gives recommendations for designing replicated tables in your SQL Data Warehouse schema.</span></span> <span data-ttu-id="edae0-105">A javaslatok tooimprove lekérdezési teljesítmény csökkenhet az adatok mozgása és lekérdezés összetettsége használatához.</span><span class="sxs-lookup"><span data-stu-id="edae0-105">Use these recommendations tooimprove query performance by reducing data movement and query complexity.</span></span>

> [!NOTE]
> <span data-ttu-id="edae0-106">hello replikált tábla funkció jelenleg nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="edae0-106">hello replicated table feature is currently in public preview.</span></span> <span data-ttu-id="edae0-107">Bizonyos viselkedéseinek tulajdonos toochange.</span><span class="sxs-lookup"><span data-stu-id="edae0-107">Some behaviors are subject toochange.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="edae0-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="edae0-108">Prerequisites</span></span>
<span data-ttu-id="edae0-109">Ez a cikk azt feltételezi, hogy az adatok terjesztési és az SQL Data Warehouse adatátviteli – fogalmak megismerése.</span><span class="sxs-lookup"><span data-stu-id="edae0-109">This article assumes you are familiar with data distribution and data movement concepts in SQL Data Warehouse.</span></span>  <span data-ttu-id="edae0-110">További információkért lásd: [adatok elosztott](sql-data-warehouse-distributed-data.md).</span><span class="sxs-lookup"><span data-stu-id="edae0-110">For more information, see [Distributed data](sql-data-warehouse-distributed-data.md).</span></span> 

<span data-ttu-id="edae0-111">Táblatervezés részeként megérteni a lehető legnagyobb mértékben az adatokat, és hogyan hello adatok le kell kérdezni.</span><span class="sxs-lookup"><span data-stu-id="edae0-111">As part of table design, understand as much as possible about your data and how hello data is queried.</span></span>  <span data-ttu-id="edae0-112">Tegyük fel ezeket a kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="edae0-112">For example, consider these questions:</span></span>

- <span data-ttu-id="edae0-113">Mekkora hello tábla?</span><span class="sxs-lookup"><span data-stu-id="edae0-113">How large is hello table?</span></span>   
- <span data-ttu-id="edae0-114">Milyen gyakran frissülnek, hello tábla?</span><span class="sxs-lookup"><span data-stu-id="edae0-114">How often is hello table refreshed?</span></span>   
- <span data-ttu-id="edae0-115">Van tény-és dimenziótáblák az adatraktárban?</span><span class="sxs-lookup"><span data-stu-id="edae0-115">Do I have fact and dimension tables in a data warehouse?</span></span>   

## <a name="what-is-a-replicated-table"></a><span data-ttu-id="edae0-116">Mi az a replikált tábla?</span><span class="sxs-lookup"><span data-stu-id="edae0-116">What is a replicated table?</span></span>
<span data-ttu-id="edae0-117">A replikált tábla rendelkezzen minden számítási csomópont hello tábla elérhető teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="edae0-117">A replicated table has a full copy of hello table accessible on each Compute node.</span></span> <span data-ttu-id="edae0-118">A tábla replikálása eltávolítja hello kell tootransfer join vagy összesítés előtt a számítási csomópontok között.</span><span class="sxs-lookup"><span data-stu-id="edae0-118">Replicating a table removes hello need tootransfer data among Compute nodes before a join or aggregation.</span></span> <span data-ttu-id="edae0-119">Mivel hello tábla csak különböző példányainak, replikált táblák legjobban, ha hello tábla mérete kisebb, mint 2 GB tömörített.</span><span class="sxs-lookup"><span data-stu-id="edae0-119">Since hello table has multiple copies, replicated tables work best when hello table size is less than 2 GB compressed.</span></span>

<span data-ttu-id="edae0-120">hello következő diagramon láthatók a replikált tábla által elérhető minden számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="edae0-120">hello following diagram shows a replicated table that is accessible on each Compute node.</span></span> <span data-ttu-id="edae0-121">Az SQL Data Warehouse hello replikált tábla teljesen másolt tooa terjesztési adatbázis minden számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="edae0-121">In SQL Data Warehouse, hello replicated table is fully copied tooa distribution database on each Compute node.</span></span> 

<span data-ttu-id="edae0-122">![Replikált tábla](media/guidance-for-using-replicated-tables/replicated-table.png "replikált tábla")</span><span class="sxs-lookup"><span data-stu-id="edae0-122">![Replicated table](media/guidance-for-using-replicated-tables/replicated-table.png "Replicated table")</span></span>  

<span data-ttu-id="edae0-123">Replikált táblák munkahelyi csillagséma kis dimenzió táblák esetén.</span><span class="sxs-lookup"><span data-stu-id="edae0-123">Replicated tables work well for small dimension tables in a star schema.</span></span> <span data-ttu-id="edae0-124">Dimenziótáblák általában, amely megkönnyíti a megvalósítható toostore méretű és különböző példányainak karbantartása.</span><span class="sxs-lookup"><span data-stu-id="edae0-124">Dimension tables are usually of a size that makes it feasible toostore and maintain multiple copies.</span></span> <span data-ttu-id="edae0-125">Dimenziók adattárolásra leíró megváltoztató lassan, például a felhasználói nevét és címét és a termék részletei.</span><span class="sxs-lookup"><span data-stu-id="edae0-125">Dimensions store descriptive data that changes slowly, such as customer name and address, and product details.</span></span> <span data-ttu-id="edae0-126">hello lassan módosítása hello adatok jellege vezet toofewer újraépíti hello replikált tábla.</span><span class="sxs-lookup"><span data-stu-id="edae0-126">hello slowly changing nature of hello data leads toofewer rebuilds of hello replicated table.</span></span> 

<span data-ttu-id="edae0-127">Fontolja meg a replikált tábla esetén:</span><span class="sxs-lookup"><span data-stu-id="edae0-127">Consider using a replicated table when:</span></span>

- <span data-ttu-id="edae0-128">hello tábla lemez mérete 2 GB-nál kevesebb, függetlenül hello sorok száma.</span><span class="sxs-lookup"><span data-stu-id="edae0-128">hello table size on disk is less than 2 GB, regardless of hello number of rows.</span></span> <span data-ttu-id="edae0-129">egy tábla toofind hello méretét, hello használható [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) parancs: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span><span class="sxs-lookup"><span data-stu-id="edae0-129">toofind hello size of a table, you can use hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) command: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`.</span></span> 
- <span data-ttu-id="edae0-130">hello tábla szolgál, amelyek egyébként adatmozgás illesztésekben.</span><span class="sxs-lookup"><span data-stu-id="edae0-130">hello table is used in joins that would otherwise require data movement.</span></span> <span data-ttu-id="edae0-131">Például az illesztés kivonatoló elosztott táblákon adatátvitelt jelölik a szükséges, ha hello csatlakozó oszlopok nem vannak hello azonos terjesztési oszlop.</span><span class="sxs-lookup"><span data-stu-id="edae0-131">For example, a join on hash-distributed tables requires data movement when hello joining columns are not hello same distribution column.</span></span> <span data-ttu-id="edae0-132">Ha hello kivonatoló elosztott tábla kicsi, fontolja meg a replikált tábla.</span><span class="sxs-lookup"><span data-stu-id="edae0-132">If one of hello hash-distributed tables is small, consider a replicated table.</span></span> <span data-ttu-id="edae0-133">Ciklikus multiplexelés táblán illesztés adatmozgás igényel.</span><span class="sxs-lookup"><span data-stu-id="edae0-133">A join on a round-robin table requires data movement.</span></span> <span data-ttu-id="edae0-134">Replikált táblák helyett a legtöbb esetben ciklikus multiplexelés táblák használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="edae0-134">We recommend using replicated tables instead of round-robin tables in most cases.</span></span> 


<span data-ttu-id="edae0-135">Vegye figyelembe a replikált tábla tooa konvertálása a meglévő elosztott táblából:</span><span class="sxs-lookup"><span data-stu-id="edae0-135">Consider converting an existing distributed table tooa replicated table when:</span></span>

- <span data-ttu-id="edae0-136">Lekérdezés-használja az adatátviteli műveletek hello adatok tooall hello számítási csomópontok szórási csomagok.</span><span class="sxs-lookup"><span data-stu-id="edae0-136">Query plans use data movement operations that broadcast hello data tooall hello Compute nodes.</span></span> <span data-ttu-id="edae0-137">hello BroadcastMoveOperation drága, és csökkenti a lekérdezés teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="edae0-137">hello BroadcastMoveOperation is expensive and slows query performance.</span></span> <span data-ttu-id="edae0-138">tooview az adatátviteli műveletek a lekérdezésterveket, használjon [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="edae0-138">tooview data movement operations in query plans, use [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).</span></span>
 
<span data-ttu-id="edae0-139">Replikált táblák nem fed fel hello legjobb lekérdezési teljesítmény során:</span><span class="sxs-lookup"><span data-stu-id="edae0-139">Replicated tables may not yield hello best query performance when:</span></span>

- <span data-ttu-id="edae0-140">hello táblához gyakori beszúrási, frissítési és törlési műveletek.</span><span class="sxs-lookup"><span data-stu-id="edae0-140">hello table has frequent insert, update, and delete operations.</span></span> <span data-ttu-id="edae0-141">Ezen adatok adatkezelési nyelv működéséhez szükségesek egy újjáépítését hello replikált tábla.</span><span class="sxs-lookup"><span data-stu-id="edae0-141">These data manipulation language (DML) operations require a rebuild of hello replicated table.</span></span> <span data-ttu-id="edae0-142">Újraépítése gyakran okozhat lassabb teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="edae0-142">Rebuilding frequently can cause slower performance.</span></span>
- <span data-ttu-id="edae0-143">hello adatraktár gyakran méretezett.</span><span class="sxs-lookup"><span data-stu-id="edae0-143">hello data warehouse is scaled frequently.</span></span> <span data-ttu-id="edae0-144">Adatraktár skálázás módosítja a hello számítási csomópontok száma, ami azt eredményezi azok háromszorosa egy Újraépítés.</span><span class="sxs-lookup"><span data-stu-id="edae0-144">Scaling a data warehouse changes hello number of Compute nodes, which incurs a rebuild.</span></span>
- <span data-ttu-id="edae0-145">hello tábla nagy számú oszlopot tartalmaz, de az műveleteket általában hozzáférni csak kis számú oszlopot.</span><span class="sxs-lookup"><span data-stu-id="edae0-145">hello table has a large number of columns, but data operations typically access only a small number of columns.</span></span> <span data-ttu-id="edae0-146">Ebben a forgatókönyvben, ahelyett, hogy a teljes táblázat hello, előfordulhat, hogy hatékonyabb toohash hello tábla terjesztése, majd hozza létre az index a gyakran használt hello oszlopokra.</span><span class="sxs-lookup"><span data-stu-id="edae0-146">In this scenario, instead of replicating hello entire table, it might be more effective toohash distribute hello table, and then create an index on hello frequently accessed columns.</span></span> <span data-ttu-id="edae0-147">Ha egy lekérdezéshez adatmozgás, az SQL Data Warehouse csak hello helyezi át adatokat kért oszlopok.</span><span class="sxs-lookup"><span data-stu-id="edae0-147">When a query requires data movement, SQL Data Warehouse only moves data in hello requested columns.</span></span> 



## <a name="use-replicated-tables-with-simple-query-predicates"></a><span data-ttu-id="edae0-148">Replikált táblák használata egyszerű lekérdezés predikátumok</span><span class="sxs-lookup"><span data-stu-id="edae0-148">Use replicated tables with simple query predicates</span></span>
<span data-ttu-id="edae0-149">Mielőtt toodistribute válassza vagy a tábla replikálása, gondolja át azt tervezi, hogy hello táblázaton toorun hello lekérdezéstípusok.</span><span class="sxs-lookup"><span data-stu-id="edae0-149">Before you choose toodistribute or replicate a table, think about hello types of queries you plan toorun against hello table.</span></span> <span data-ttu-id="edae0-150">Amikor csak lehetséges –</span><span class="sxs-lookup"><span data-stu-id="edae0-150">Whenever possible,</span></span>

- <span data-ttu-id="edae0-151">Az egyszerű lekérdezés predikátumok, például egyenlőség vagy egyenlőtlen Lekérdezések replikált táblák használja.</span><span class="sxs-lookup"><span data-stu-id="edae0-151">Use replicated tables for queries with simple query predicates, such as equality or inequality.</span></span>
- <span data-ttu-id="edae0-152">Az összetett lekérdezések predikátumok, például a hasonló lekérdezések elosztott táblázatot használni, vagy nincs MEGELÉGEDVE.</span><span class="sxs-lookup"><span data-stu-id="edae0-152">Use distributed tables for queries with complex query predicates, such as LIKE or NOT LIKE.</span></span>

<span data-ttu-id="edae0-153">Processzorigényes lekérdezések legjobb elvégezni, amikor hello munkahelyi oszlik el minden hello számítási csomópont.</span><span class="sxs-lookup"><span data-stu-id="edae0-153">CPU-intensive queries perform best when hello work is distributed across all of hello Compute nodes.</span></span> <span data-ttu-id="edae0-154">Például egy táblázat minden egyes sorára számítások futó lekérdezések végezzen jobban, mint a replikált táblák elosztott táblákon.</span><span class="sxs-lookup"><span data-stu-id="edae0-154">For example, queries that run computations on each row of a table perform better on distributed tables than replicated tables.</span></span> <span data-ttu-id="edae0-155">Mivel a replikált tábla rendszer teljes minden számítási csomóponton, processzorigényes replikált táblákon futása hello teljes táblázaton minden számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="edae0-155">Since a replicated table is stored in full on each Compute node, a CPU-intensive query against a replicated table runs against hello entire table on every Compute node.</span></span> <span data-ttu-id="edae0-156">hello további számítási is csökkentheti a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="edae0-156">hello extra computation can slow query performance.</span></span>

<span data-ttu-id="edae0-157">Például ez a lekérdezés tartalmaz egy összetett predikátum.</span><span class="sxs-lookup"><span data-stu-id="edae0-157">For example, this query has a complex predicate.</span></span>  <span data-ttu-id="edae0-158">Az elosztott táblázat helyett egy replikált tábla esetén a szállító gyorsabban futtatja.</span><span class="sxs-lookup"><span data-stu-id="edae0-158">It runs faster when supplier is a distributed table instead of a replicated table.</span></span> <span data-ttu-id="edae0-159">Ebben a példában a szállító kivonatoló elosztott vagy ciklikus multiplexelés elosztott.</span><span class="sxs-lookup"><span data-stu-id="edae0-159">In this example, supplier can be hash-distributed or round-robin distributed.</span></span>

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-tooreplicated-tables"></a><span data-ttu-id="edae0-160">Alakítsa át a létező táblák ciklikus multiplexelés tooreplicated táblák</span><span class="sxs-lookup"><span data-stu-id="edae0-160">Convert existing round-robin tables tooreplicated tables</span></span>
<span data-ttu-id="edae0-161">Ha már rendelkezik ciklikus multiplexelés táblák, javasoljuk a konvertálás tooreplicated táblák azokat a cikkben ismertetett feltételekkel, amelyek megfelelnek.</span><span class="sxs-lookup"><span data-stu-id="edae0-161">If you already have round-robin tables, we recommend converting them tooreplicated tables if they meet with criteria outlined in this article.</span></span> <span data-ttu-id="edae0-162">Replikált táblák teljesítmény javítása a ciklikus multiplexelés táblák keresztül, mert azok kiküszöbölik adatmozgás hello szükségességét.</span><span class="sxs-lookup"><span data-stu-id="edae0-162">Replicated tables improve performance over round-robin tables because they eliminate hello need for data movement.</span></span>  <span data-ttu-id="edae0-163">Ciklikus multiplexelés tábla mindig szükséges, adatátvitelt jelölik a táblákra.</span><span class="sxs-lookup"><span data-stu-id="edae0-163">A round-robin table always requires data movement for joins.</span></span> 

<span data-ttu-id="edae0-164">Ez a példa [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory tábla tooa replikált tábla.</span><span class="sxs-lookup"><span data-stu-id="edae0-164">This example uses [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory table tooa replicated table.</span></span> <span data-ttu-id="edae0-165">Ez a példa függetlenül attól, hogy DimSalesTerritory kivonatoló elosztott vagy a ciklikus multiplexelési működik.</span><span class="sxs-lookup"><span data-stu-id="edae0-165">This example works regardless of whether DimSalesTerritory is hash-distributed or round-robin.</span></span>

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] too[DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] too[DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a><span data-ttu-id="edae0-166">Ciklikus multiplexelés és a lekérdezési teljesítmény példa replikálása</span><span class="sxs-lookup"><span data-stu-id="edae0-166">Query performance example for round-robin versus replicated</span></span> 

<span data-ttu-id="edae0-167">A replikált tábla nem szükséges minden adatátvitelt jelölik a táblákra mivel hello teljes tábla már létezik minden számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="edae0-167">A replicated table does not require any data movement for joins because hello entire table is already present on each Compute node.</span></span> <span data-ttu-id="edae0-168">Ha hello dimenziótáblák ciklikus multiplexelés elosztott, illesztés átmásolja a terjesztendő hello dimenziótáblában teljes tooeach számítási csomópont.</span><span class="sxs-lookup"><span data-stu-id="edae0-168">If hello dimension tables are round-robin distributed, a join copies hello dimension table in full tooeach Compute node.</span></span> <span data-ttu-id="edae0-169">toomove hello adat, hello lekérdezésterv tartalmaz egy BroadcastMoveOperation nevű művelet.</span><span class="sxs-lookup"><span data-stu-id="edae0-169">toomove hello data, hello query plan contains an operation called BroadcastMoveOperation.</span></span> <span data-ttu-id="edae0-170">Ilyen adatok adatátviteli művelet csökkenti a lekérdezés teljesítményét, és kiiktatja a replikált táblák használata.</span><span class="sxs-lookup"><span data-stu-id="edae0-170">This type of data movement operation slows query performance and is eliminated by using replicated tables.</span></span> <span data-ttu-id="edae0-171">tooview lekérdezés terv lépései, használja a hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) rendszer katalógusnézet használatával derítheti ki.</span><span class="sxs-lookup"><span data-stu-id="edae0-171">tooview query plan steps, use hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) system catalog view.</span></span> 

<span data-ttu-id="edae0-172">Például a következő lekérdezés hello AdventureWorks sémának, hello ` FactInternetSales` táblát kell kivonatoló elosztott.</span><span class="sxs-lookup"><span data-stu-id="edae0-172">For example, in following query against hello AdventureWorks schema, hello ` FactInternetSales` table is hash-distributed.</span></span> <span data-ttu-id="edae0-173">Hello `DimDate` és `DimSalesTerritory` kisebb dimenzió táblák.</span><span class="sxs-lookup"><span data-stu-id="edae0-173">hello `DimDate` and `DimSalesTerritory` tables are smaller dimension tables.</span></span> <span data-ttu-id="edae0-174">A lekérdezés által visszaadott, pénzügyi év 2004 hello összes értékesítés Észak-Amerikában:</span><span class="sxs-lookup"><span data-stu-id="edae0-174">This query returns hello total sales in North America for fiscal year 2004:</span></span>
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
<span data-ttu-id="edae0-175">Újból létrehozza `DimDate` és `DimSalesTerritory` ciklikus multiplexelés táblák.</span><span class="sxs-lookup"><span data-stu-id="edae0-175">We re-created `DimDate` and `DimSalesTerritory` as round-robin tables.</span></span> <span data-ttu-id="edae0-176">Ennek eredményeképpen hello lekérdezés bemutatta, a következő műveletek áthelyezése több szórás rendelkező lekérdezésterv hello:</span><span class="sxs-lookup"><span data-stu-id="edae0-176">As a result, hello query showed hello following query plan, which has multiple broadcast move operations:</span></span> 
 
![Ciklikus lekérdezés terv](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

<span data-ttu-id="edae0-178">Újból létrehozza `DimDate` és `DimSalesTerritory` regisztrációja, mivel a replikált táblák és a hello lekérdezés újra futott.</span><span class="sxs-lookup"><span data-stu-id="edae0-178">We re-created `DimDate` and `DimSalesTerritory` as replicated tables, and ran hello query again.</span></span> <span data-ttu-id="edae0-179">hello eredményül kapott lekérdezésterv sokkal rövidebb, és nem rendelkezik a küldi helyezi át.</span><span class="sxs-lookup"><span data-stu-id="edae0-179">hello resulting query plan is much shorter and does not have any broadcast moves.</span></span>

![Replikált lekérdezésterv](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a><span data-ttu-id="edae0-181">A replikált táblák módosítását a teljesítménnyel kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="edae0-181">Performance considerations for modifying replicated tables</span></span>
<span data-ttu-id="edae0-182">Az SQL Data Warehouse egy replikált tábla fölött: hello tábla főverzió valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="edae0-182">SQL Data Warehouse implements a replicated table by maintaining a master version of hello table.</span></span> <span data-ttu-id="edae0-183">Másolja át hello főverzió tooone terjesztési adatbázis minden számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="edae0-183">It copies hello master version tooone distribution database on each Compute node.</span></span> <span data-ttu-id="edae0-184">Változás történik, amikor az SQL Data Warehouse először frissíti hello fő tábla.</span><span class="sxs-lookup"><span data-stu-id="edae0-184">When there is a change, SQL Data Warehouse first updates hello master table.</span></span> <span data-ttu-id="edae0-185">Majd minden számítási csomóponton hello táblák egy újjáépítését igényel.</span><span class="sxs-lookup"><span data-stu-id="edae0-185">Then it requires a rebuild of hello tables on each Compute node.</span></span> <span data-ttu-id="edae0-186">A replikált tábla újjáépítését hello tábla tooeach számítási csomópont másolása és majd a hello indexek újraépítése tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="edae0-186">A rebuild of a replicated table includes copying hello table tooeach Compute node and then rebuilding hello indexes.</span></span>

<span data-ttu-id="edae0-187">Újraépíti szükségesek után:</span><span class="sxs-lookup"><span data-stu-id="edae0-187">Rebuilds are required after:</span></span>
- <span data-ttu-id="edae0-188">Adatok betöltése vagy módosította</span><span class="sxs-lookup"><span data-stu-id="edae0-188">Data is loaded or modified</span></span>
- <span data-ttu-id="edae0-189">hello adatraktár az méretezett tooa különböző DWU-beállítására</span><span class="sxs-lookup"><span data-stu-id="edae0-189">hello data warehouse is scaled tooa different DWU setting</span></span>
- <span data-ttu-id="edae0-190">Tábla definíciójában frissül.</span><span class="sxs-lookup"><span data-stu-id="edae0-190">Table definition is updated</span></span>

<span data-ttu-id="edae0-191">Újraépíti esetén nincs szükség után:</span><span class="sxs-lookup"><span data-stu-id="edae0-191">Rebuilds are not required after:</span></span>
- <span data-ttu-id="edae0-192">Szüneteltetési művelete</span><span class="sxs-lookup"><span data-stu-id="edae0-192">Pause operation</span></span>
- <span data-ttu-id="edae0-193">A művelet folytatása</span><span class="sxs-lookup"><span data-stu-id="edae0-193">Resume operation</span></span>

<span data-ttu-id="edae0-194">hello rebuild történik meg azonnal adatok módosítása után.</span><span class="sxs-lookup"><span data-stu-id="edae0-194">hello rebuild does not happen immediately after data is modified.</span></span> <span data-ttu-id="edae0-195">Ehelyett hello rebuild kiváltásáról hello hello táblából kiválasztja a lekérdezés első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="edae0-195">Instead, hello rebuild is triggered hello first time a query selects from hello table.</span></span>  <span data-ttu-id="edae0-196">Hello kezdeti select utasítás hello táblából van lépéseket toorebuild hello replikált tábla.</span><span class="sxs-lookup"><span data-stu-id="edae0-196">Within hello initial select statement from hello table are steps toorebuild hello replicated table.</span></span>  <span data-ttu-id="edae0-197">Mivel hello rebuild hello lekérdezésen belül történik, hello hatás toohello kezdeti select utasítás hello tábla hello méretétől függően jelentős lehet.</span><span class="sxs-lookup"><span data-stu-id="edae0-197">Because hello rebuild is done within hello query, hello impact toohello initial select statement could be significant depending on hello size of hello table.</span></span>  <span data-ttu-id="edae0-198">Ha több replikált táblák egy rebuild igénylő van szó, minden egyes példányra újraépítésekor Feladattervek lépések hello utasításon belül.</span><span class="sxs-lookup"><span data-stu-id="edae0-198">If multiple replicated tables are involved that need a rebuild, each copy is rebuilt serially as steps within hello statement.</span></span>  <span data-ttu-id="edae0-199">toomaintain adatok konzisztenciájának hello hello újjáépítését során replikált tábla kizárólagos zárolást hello tábla lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="edae0-199">toomaintain data consistency during hello rebuild of hello replicated table an exclusive lock is taken on hello table.</span></span>  <span data-ttu-id="edae0-200">hello zárolási megakadályozza, hogy a minden hozzáférési toohello tábla hello hello Újraépítés időtartama.</span><span class="sxs-lookup"><span data-stu-id="edae0-200">hello lock prevents all access toohello table for hello duration of hello rebuild.</span></span> 

### <a name="use-indexes-conservatively"></a><span data-ttu-id="edae0-201">Konzervatív módon indexek használata</span><span class="sxs-lookup"><span data-stu-id="edae0-201">Use indexes conservatively</span></span>
<span data-ttu-id="edae0-202">Standard indexelési eljárások tooreplicated táblák alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="edae0-202">Standard indexing practices apply tooreplicated tables.</span></span> <span data-ttu-id="edae0-203">Az SQL Data Warehouse újraépíti minden replikált tábla index hello rebuild részeként.</span><span class="sxs-lookup"><span data-stu-id="edae0-203">SQL Data Warehouse rebuilds each replicated table index as part of hello rebuild.</span></span> <span data-ttu-id="edae0-204">Indexek csak akkor alkalmazza, ha hello jobb teljesítménye ez fontosabb, mint a hello indexek újraépítése hello költségét.</span><span class="sxs-lookup"><span data-stu-id="edae0-204">Only use indexes when hello performance gain outweighs hello cost of rebuilding hello indexes.</span></span>  
 
### <a name="batch-data-loads"></a><span data-ttu-id="edae0-205">Kötegelt adatbetöltés</span><span class="sxs-lookup"><span data-stu-id="edae0-205">Batch data loads</span></span>
<span data-ttu-id="edae0-206">Ha az adatok betöltését replikált táblák, próbálkozna toominimize újraépíti terhelések kötegelés együtt.</span><span class="sxs-lookup"><span data-stu-id="edae0-206">When loading data into replicated tables, try toominimize rebuilds by batching loads together.</span></span> <span data-ttu-id="edae0-207">Hajtsa végre az összes kötegelni hello terhelések select utasítás futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="edae0-207">Perform all hello batched loads before running select statements.</span></span>

<span data-ttu-id="edae0-208">Például a terhelés mintát négy forrásból származó adatokat tölt, és meghívja a négy újraépíti.</span><span class="sxs-lookup"><span data-stu-id="edae0-208">For example, this load pattern loads data from four sources and invokes four rebuilds.</span></span> 

- <span data-ttu-id="edae0-209">Forrás 1 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-209">Load from source 1.</span></span>
- <span data-ttu-id="edae0-210">SELECT utasítás eseményindítók építse újra az 1.</span><span class="sxs-lookup"><span data-stu-id="edae0-210">Select statement triggers rebuild 1.</span></span>
- <span data-ttu-id="edae0-211">Forrás 2 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-211">Load from source 2.</span></span>
- <span data-ttu-id="edae0-212">SELECT utasítás eseményindítók építse újra a 2.</span><span class="sxs-lookup"><span data-stu-id="edae0-212">Select statement triggers rebuild 2.</span></span>
- <span data-ttu-id="edae0-213">Forrás 3 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-213">Load from source 3.</span></span>
- <span data-ttu-id="edae0-214">SELECT utasítás eseményindítók építse újra a 3.</span><span class="sxs-lookup"><span data-stu-id="edae0-214">Select statement triggers rebuild 3.</span></span>
- <span data-ttu-id="edae0-215">Adatforrás 4 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-215">Load from source 4.</span></span>
- <span data-ttu-id="edae0-216">SELECT utasítás eseményindítók építse újra a 4.</span><span class="sxs-lookup"><span data-stu-id="edae0-216">Select statement triggers rebuild 4.</span></span>

<span data-ttu-id="edae0-217">Például a terhelés mintát négy forrásból származó adatokat tölt, de csak hív meg, egy Újraépítés.</span><span class="sxs-lookup"><span data-stu-id="edae0-217">For example, this load pattern loads data from four sources, but only invokes one rebuild.</span></span>

- <span data-ttu-id="edae0-218">Forrás 1 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-218">Load from source 1.</span></span>
- <span data-ttu-id="edae0-219">Forrás 2 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-219">Load from source 2.</span></span>
- <span data-ttu-id="edae0-220">Forrás 3 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-220">Load from source 3.</span></span>
- <span data-ttu-id="edae0-221">Adatforrás 4 betöltése.</span><span class="sxs-lookup"><span data-stu-id="edae0-221">Load from source 4.</span></span>
- <span data-ttu-id="edae0-222">SELECT utasítás eseményindítók építse újra.</span><span class="sxs-lookup"><span data-stu-id="edae0-222">Select statement triggers rebuild.</span></span>


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a><span data-ttu-id="edae0-223">A replikált tábla helyreállítása egy kötegelt betöltés után</span><span class="sxs-lookup"><span data-stu-id="edae0-223">Rebuild a replicated table after a batch load</span></span>
<span data-ttu-id="edae0-224">tooensure lekérdezés konzisztens végrehajtásának lassúságát, ajánlott kényszerített hello replikált táblák frissítését egy kötegelt betöltés után.</span><span class="sxs-lookup"><span data-stu-id="edae0-224">tooensure consistent query execution times, we recommend forcing a refresh of hello replicated tables after a batch load.</span></span> <span data-ttu-id="edae0-225">Ellenkező esetben hello első lekérdezés hello táblák toorefresh, amely tartalmazza a hello indexek újraépítése kell várnia.</span><span class="sxs-lookup"><span data-stu-id="edae0-225">Otherwise, hello first query must wait for hello tables toorefresh, which includes rebuilding hello indexes.</span></span> <span data-ttu-id="edae0-226">Attól függően, hogy hello méretét és az érintett replikált táblák számát hello teljesítményre gyakorolt hatás jelentős lehet.</span><span class="sxs-lookup"><span data-stu-id="edae0-226">Depending on hello size and number of replicated tables affected, hello performance impact can be significant.</span></span>  

<span data-ttu-id="edae0-227">Ez a lekérdezés használ hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replikált módosított, de nem úgy táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="edae0-227">This query uses hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replicated tables that have been modified, but not rebuilt.</span></span>

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
<span data-ttu-id="edae0-228">egy rebuild tooforce futtassa a következő utasítás minden táblában a kimeneti megelőző hello hello.</span><span class="sxs-lookup"><span data-stu-id="edae0-228">tooforce a rebuild, run hello following statement on each table in hello preceding output.</span></span> 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a><span data-ttu-id="edae0-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="edae0-229">Next steps</span></span> 
<span data-ttu-id="edae0-230">a replikált tábla toocreate ezekre az utasításokra egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="edae0-230">toocreate a replicated table, use one of these statements:</span></span>

- [<span data-ttu-id="edae0-231">(Az Azure SQL Data Warehouse) tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="edae0-231">CREATE TABLE (Azure SQL Data Warehouse)</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [<span data-ttu-id="edae0-232">TABLE AS SELECT (Azure SQL Data Warehouse létrehozása</span><span class="sxs-lookup"><span data-stu-id="edae0-232">CREATE TABLE AS SELECT (Azure SQL Data Warehouse</span></span>](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

<span data-ttu-id="edae0-233">Elosztott táblák áttekintését lásd: [táblák elosztott](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="edae0-233">For an overview of distributed tables, see [distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>



