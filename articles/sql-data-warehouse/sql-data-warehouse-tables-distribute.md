---
title: "az SQL Data Warehouse aaaDistributing táblák |} Microsoft Docs"
description: "Első lépések az Azure SQL Data Warehouse táblák terjesztése."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 65093eeaeb00fef85aaa6070da2c976fed3f4bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a><span data-ttu-id="3a7ba-103">Az SQL Data Warehouse táblák terjesztése</span><span class="sxs-lookup"><span data-stu-id="3a7ba-103">Distributing tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="3a7ba-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="3a7ba-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="3a7ba-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="3a7ba-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-107">[Index][Index]</span></span>
> * <span data-ttu-id="3a7ba-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="3a7ba-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="3a7ba-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="3a7ba-110">[Temporary][Temporary]</span></span>
>
>

<span data-ttu-id="3a7ba-111">Az SQL Data Warehouse egy nagymértékben párhuzamos feldolgozási (MPP) elosztott adatbázisrendszerre épül.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-111">SQL Data Warehouse is a massively parallel processing (MPP) distributed database system.</span></span>  <span data-ttu-id="3a7ba-112">Az adatok és a feldolgozási képesség több csomópont közötti elosztásával az SQL Data Warehouse olyan rendkívüli méretezhetőséget kínál, amely messze meghaladja bármilyen különálló rendszer lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-112">By dividing data and processing capability across multiple nodes, SQL Data Warehouse can offer huge scalability - far beyond any single system.</span></span>  <span data-ttu-id="3a7ba-113">Annak eldöntése, hogyan toodistribute az adatokat az SQL Data warehouse az egyik legfontosabb hello tényezők tooachieving optimális teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-113">Deciding how toodistribute your data within your SQL Data Warehouse is one of hello most important factors tooachieving optimal performance.</span></span>   <span data-ttu-id="3a7ba-114">hello kulcs toooptimal teljesítmény van kis méretre adatmozgás, vagy pedig hello kulcs toominimizing adatmozgás hello megfelelő telepítési stratégia jelöl.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-114">hello key toooptimal performance is minimizing data movement and in turn hello key toominimizing data movement is selecting hello right distribution strategy.</span></span>

## <a name="understanding-data-movement"></a><span data-ttu-id="3a7ba-115">Adatátvitel ismertetése</span><span class="sxs-lookup"><span data-stu-id="3a7ba-115">Understanding data movement</span></span>
<span data-ttu-id="3a7ba-116">Minden tábla hello adatait az MPP rendszer több alapul szolgáló adatbázisok között oszlik meg.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-116">In an MPP system, hello data from each table is divided across several underlying databases.</span></span>  <span data-ttu-id="3a7ba-117">hello legtöbb optimalizált lekérdezések az MPP rendszeren is egyszerűen átadható tooexecute hello a beavatkozás nélküli egyes elosztott adatbázisok közötti hello más adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-117">hello most optimized queries on an MPP system can simply be passed through tooexecute on hello individual distributed databases with no interaction between hello other databases.</span></span>  <span data-ttu-id="3a7ba-118">Például tételezzük fel az értékesítési adatait, amely tartalmazza a két tábla, értékesítéssel és az ügyfelek olyan adatbázist.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-118">For example, let's say you have a database with sales data which contains two tables, sales and customers.</span></span>  <span data-ttu-id="3a7ba-119">Ha egy lekérdezést, amelyet a toojoin az értékesítési tooyour felhasználói tábla, és hogy nullával az értékesítési és a felhasználói táblák mentése ügyfélszámot, külön adatbázisban mindegyik ügyfél üzembe, a csatlakozás az értékesítés és a felhasználói lekérdezések belül minden megoldhatók nincsenek saját ismeretei az adatbázis hello más adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-119">If you have a query that needs toojoin your sales table tooyour customer table and you divide both your sales and customer tables up by customer number, putting each customer in a separate database, any queries which join sales and customer can be solved within each database with no knowledge of hello other databases.</span></span>  <span data-ttu-id="3a7ba-120">Ezzel szemben, ha az értékesítési adatok osztva számát és az ügyféladatok ügyfél száma szerint, majd bármely adott adatbázis nem lesz hello tartozó adatokat minden ügyfél esetében, ezért ha toojoin az értékesítési adatokat tooyour ügyféladatok, kellene tooget hello adatok a hello minden ügyfél esetében más adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-120">In contrast, if you divided your sales data by order number and your customer data by customer number, then any given database will not have hello corresponding data for each customer and thus if you wanted toojoin your sales data tooyour customer data, you would need tooget hello data for each customer from hello other databases.</span></span>  <span data-ttu-id="3a7ba-121">A második példában adatmozgás kellene toooccur toomove hello adatok toohello értékesítési ügyféladatok, úgy, hogy a két hello táblák társíthatók.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-121">In this second example, data movement would need toooccur toomove hello customer data toohello sales data, so that hello two tables can be joined.</span></span>  

<span data-ttu-id="3a7ba-122">Adatátvitel nem mindig egy rossz dolgot tegyenek, egyes esetekben szükséges toosolve lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-122">Data movement isn't always a bad thing, sometimes it's necessary toosolve a query.</span></span>  <span data-ttu-id="3a7ba-123">De ha ezt a kiegészítő lépést el kell kerülni, természetesen a lekérdezés fog futni gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-123">But when this extra step can be avoided, naturally your query will run faster.</span></span>  <span data-ttu-id="3a7ba-124">Adatátvitel leggyakrabban akkor keletkezik, ha a táblák tartományhoz csatlakoztatott vagy Összesítés hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-124">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="3a7ba-125">Gyakran kell toodo mindkét, így bár bizonyára tudni toooptimize egy forgatókönyvet, például a csatlakozzon, Ön továbbra is igényel, oldja meg az adatok mozgása toohelp hello más forgatókönyv, például egy összesítő.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-125">Often you need toodo both, so while you may be able toooptimize for one scenario, like a join, you still need data movement toohelp you solve for hello other scenario, like an aggregation.</span></span>  <span data-ttu-id="3a7ba-126">hello trükk rendszer tudja, amelyek értéke kevesebb munka.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-126">hello trick is figuring out which is less work.</span></span>  <span data-ttu-id="3a7ba-127">A legtöbb esetben nagy ténytáblák általában illesztett oszlopon kiosztása van hello hello csökkenti a leghatékonyabb módszer legtöbb adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-127">In most cases, distributing large fact tables on a commonly joined column is hello most effective method for reducing hello most data movement.</span></span>  <span data-ttu-id="3a7ba-128">Az illesztési oszlopok adatok terjesztése egy sokkal közös metódus tooreduce adatmozgás mint összesítést részt vevő oszlopokat adatok terjesztése.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-128">Distributing data on join columns is a much more common method tooreduce data movement than distributing data on columns involved in an aggregation.</span></span>

## <a name="select-distribution-method"></a><span data-ttu-id="3a7ba-129">Elosztási módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3a7ba-129">Select distribution method</span></span>
<span data-ttu-id="3a7ba-130">Hello háttérben SQL Data Warehouse felosztja az adatok 60 adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-130">Behind hello scenes, SQL Data Warehouse divides your data into 60 databases.</span></span>  <span data-ttu-id="3a7ba-131">Minden egyes adatbázis hivatkozott tooas egy **terjesztési**.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-131">Each individual database is referred tooas a **distribution**.</span></span>  <span data-ttu-id="3a7ba-132">Adatok betöltése az egyes táblanevek, az SQL Data Warehouse rendelkezik tooknow hogyan toodivide az adatok így vannak elrendezve a 60 terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-132">When data is loaded into each table, SQL Data Warehouse has tooknow how toodivide your data across these 60 distributions.</span></span>  

<span data-ttu-id="3a7ba-133">hello elosztási módszer hello tábla szintjén van meghatározva, és jelenleg két választási lehetőség:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-133">hello distribution method is defined at hello table level and currently there are two choices:</span></span>

1. <span data-ttu-id="3a7ba-134">**Ciklikus multiplexelés** egyenletesen, de véletlenszerűen elosztják adatokat.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-134">**Round robin** which distribute data evenly but randomly.</span></span>
2. <span data-ttu-id="3a7ba-135">**Elosztott kivonatoló** osztja el a kivonatolás csak egy oszlop értékeinek alapján</span><span class="sxs-lookup"><span data-stu-id="3a7ba-135">**Hash Distributed** which distributes data based on hashing values from a single column</span></span>

<span data-ttu-id="3a7ba-136">Alapértelmezés szerint nem határoznak meg a telepítési módszert, ha a tábla eloszlik a hello segítségével **ciklikus multiplexelés** elosztási módszer.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-136">By default, when you do not define a data distribution method, your table will be distributed using hello **round robin** distribution method.</span></span>  <span data-ttu-id="3a7ba-137">Azonban ha már kifinomultabb a megvalósításában, érdemes tooconsider használatával **elosztott kivonatoló** táblák toominimize adatmozgás, ami viszont optimalizálja a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-137">However, as you become more sophisticated in your implementation, you will want tooconsider using **hash distributed** tables toominimize data movement which will in turn optimize query performance.</span></span>

### <a name="round-robin-tables"></a><span data-ttu-id="3a7ba-138">Ciklikus multiplexelés táblák</span><span class="sxs-lookup"><span data-stu-id="3a7ba-138">Round Robin Tables</span></span>
<span data-ttu-id="3a7ba-139">Ciklikus multiplexelés metódus adatok elosztása hello használata túlságosan azok hogyan úgy tűnik, hogy.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-139">Using hello Round Robin method of distributing data is very much how it sounds.</span></span>  <span data-ttu-id="3a7ba-140">Ahogy az az adatok be van töltve, minden egyes sorára egyszerűen toohello következő terjesztési küld.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-140">As your data is loaded, each row is simply sent toohello next distribution.</span></span>  <span data-ttu-id="3a7ba-141">Ez a módszer elosztása hello adatokat fog mindig véletlenszerűen hello adatok nagyon egyenletes elosztása összes hello terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-141">This method of distributing hello data will always randomly distribute hello data very evenly across all of hello distributions.</span></span>  <span data-ttu-id="3a7ba-142">Ez azt jelenti, hogy nincs rendezés kész során hello round robin folyamattal, amely az adatokat helyezi.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-142">That is, there is no sorting done during hello round robin process which places your data.</span></span>  <span data-ttu-id="3a7ba-143">Ciklikus multiplexelés terjesztési ezért nevezik véletlenszerű kivonatát.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-143">A round robin distribution is sometimes called a random hash for this reason.</span></span>  <span data-ttu-id="3a7ba-144">Ciklikus multiplexelés elosztott tábla nincs szükség toounderstand hello adat.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-144">With a round-robin distributed table there is no need toounderstand hello data.</span></span>  <span data-ttu-id="3a7ba-145">Emiatt ciklikus multiplexelés gyakran táblázatokkal jó betöltése célokat.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-145">For this reason, Round-Robin tables often make good loading targets.</span></span>

<span data-ttu-id="3a7ba-146">Alapértelmezés szerint nincs elosztási módszer választása esetén hello ciklikus multiplexelés elosztási módszer használható.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-146">By default, if no distribution method is chosen, hello round robin distribution method will be used.</span></span>  <span data-ttu-id="3a7ba-147">Azonban amíg ciklikus multiplexelés táblák könnyen toouse, mert az adatok véletlenszerűen hello rendszer, az azt jelenti, hogy a rendszer hello nem garantálható, hogy mely terjesztési pontjain. mindegyik sor van.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-147">However, while round robin tables are easy toouse, because data is randomly distributed across hello system it means that hello system can't guarantee which distribution each row is on.</span></span>  <span data-ttu-id="3a7ba-148">Eredményeként hello rendszer néha igények tooinvoke egy adatok adatátviteli művelet toobetter rendszerezi az adatokat ahhoz, hogy feloldja egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-148">As a result, hello system sometimes needs tooinvoke a data movement operation toobetter organize your data before it can resolve a query.</span></span>  <span data-ttu-id="3a7ba-149">Ezt a kiegészítő lépést lelassíthatja a lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-149">This extra step can slow down your queries.</span></span>

<span data-ttu-id="3a7ba-150">Vegye figyelembe, hogy a táblázat a forgatókönyv a következő hello ciklikus multiplexelés eloszlás használatával:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-150">Consider using Round Robin distribution for your table in hello following scenarios:</span></span>

* <span data-ttu-id="3a7ba-151">Ha egyszerű kiindulási pontként első lépések</span><span class="sxs-lookup"><span data-stu-id="3a7ba-151">When getting started as a simple starting point</span></span>
* <span data-ttu-id="3a7ba-152">Ha nincs egyértelmű csatlakozó kulcs</span><span class="sxs-lookup"><span data-stu-id="3a7ba-152">If there is no obvious joining key</span></span>
* <span data-ttu-id="3a7ba-153">Ha nem jó jelölt oszlop a kivonat hello tábla terjesztése</span><span class="sxs-lookup"><span data-stu-id="3a7ba-153">If there is not good candidate column for hash distributing hello table</span></span>
* <span data-ttu-id="3a7ba-154">Ha hello tábla nem osztja meg egy közös illesztési kulcs más táblák</span><span class="sxs-lookup"><span data-stu-id="3a7ba-154">If hello table does not share a common join key with other tables</span></span>
* <span data-ttu-id="3a7ba-155">Ha hello illesztési kevésbé fontos, mint más illesztések hello lekérdezés</span><span class="sxs-lookup"><span data-stu-id="3a7ba-155">If hello join is less significant than other joins in hello query</span></span>
* <span data-ttu-id="3a7ba-156">Egy ideiglenes átmeneti tárolási tábla hello tábla esetén</span><span class="sxs-lookup"><span data-stu-id="3a7ba-156">When hello table is a temporary staging table</span></span>

<span data-ttu-id="3a7ba-157">A példák is létrehoz egy ciklikus multiplexelés táblát:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-157">Both of these examples will create a Round Robin Table:</span></span>

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> <span data-ttu-id="3a7ba-158">Ciklikus multiplexelés pedig hello alapértelmezett táblatípus alatt a DDL-t a explicit tekinthető az ajánlott eljárás, hogy a table elrendezést hello céljaira egyértelmű tooothers.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-158">While round robin is hello default table type being explicit in your DDL is considered a best practice so that hello intentions of your table layout are clear tooothers.</span></span>
>
>

### <a name="hash-distributed-tables"></a><span data-ttu-id="3a7ba-159">Elosztott táblák kivonatoló</span><span class="sxs-lookup"><span data-stu-id="3a7ba-159">Hash Distributed Tables</span></span>
<span data-ttu-id="3a7ba-160">Használja a **elosztott kivonatoló** algoritmus toodistribute a táblák a jobb teljesítmény érdekében számos forgatókönyv az adatmozgás csökkentése a lekérdezés idejét.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-160">Using a **Hash distributed** algorithm toodistribute your tables can improve performance for many scenarios by reducing data movement at query time.</span></span>  <span data-ttu-id="3a7ba-161">Elosztott táblák listáját táblákat, amelyek között hello kivonatoló elosztott adatbázisok csak egy oszlop, amely választ a kivonatolási algoritmust használja.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-161">Hash distributed tables are tables which are divided between hello distributed databases using a hashing algorithm on a single column which you select.</span></span>  <span data-ttu-id="3a7ba-162">hello terjesztési oszlop, mi határozza meg, hogyan hello adatok felosztásának az elosztott adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-162">hello distribution column is what determines how hello data is divided across your distributed databases.</span></span>  <span data-ttu-id="3a7ba-163">hello kivonatoló funkció hello terjesztési oszlop tooassign sorok toodistributions.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-163">hello hash function uses hello distribution column tooassign rows toodistributions.</span></span>  <span data-ttu-id="3a7ba-164">hello kivonatoló algoritmust, és eredményül kapott terjesztési determinisztikus.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-164">hello hashing algorithm and resulting distribution is deterministic.</span></span>  <span data-ttu-id="3a7ba-165">Vagyis hello hello ugyanannál az adattípusnál lesz mindig azonos értéket tartalmaz toohello azonos terjesztési.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-165">That is hello same value with hello same data type will always has toohello same distribution.</span></span>    

<span data-ttu-id="3a7ba-166">Ez a példa létrehoz egy táblát, a terjesztés azonosítója:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-166">This example will create a table distributed on id:</span></span>

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a><span data-ttu-id="3a7ba-167">Válassza ki a terjesztési oszlop</span><span class="sxs-lookup"><span data-stu-id="3a7ba-167">Select distribution column</span></span>
<span data-ttu-id="3a7ba-168">Ha úgy dönt, túl**kivonatoló terjesztése** egy tábla tooselect egyetlen terjesztési oszlop szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-168">When you choose too**hash distribute** a table, you will need tooselect a single distribution column.</span></span>  <span data-ttu-id="3a7ba-169">Jelöljön ki egy terjesztési oszlopot, amikor nincsenek három fő tényezők tooconsider.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-169">When selecting a distribution column, there are three major factors tooconsider.</span></span>  

<span data-ttu-id="3a7ba-170">Válassza ki a csak egy oszlop, amelyek:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-170">Select a single column which will:</span></span>

1. <span data-ttu-id="3a7ba-171">Nem frissíthető</span><span class="sxs-lookup"><span data-stu-id="3a7ba-171">Not be updated</span></span>
2. <span data-ttu-id="3a7ba-172">Adatok, egyenletes elosztása elkerülhető az a döntés adatok</span><span class="sxs-lookup"><span data-stu-id="3a7ba-172">Distribute data evenly, avoiding data skew</span></span>
3. <span data-ttu-id="3a7ba-173">Adatátvitel minimalizálása érdekében</span><span class="sxs-lookup"><span data-stu-id="3a7ba-173">Minimize data movement</span></span>

### <a name="select-distribution-column-which-will-not-be-updated"></a><span data-ttu-id="3a7ba-174">Válassza ki a terjesztési oszlop, amely nem frissíthető</span><span class="sxs-lookup"><span data-stu-id="3a7ba-174">Select distribution column which will not be updated</span></span>
<span data-ttu-id="3a7ba-175">Terjesztési oszlopok nem frissíthető, ezért, válasszon statikus értékeket tartalmazó oszlop.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-175">Distribution columns are not updatable, therefore, select a column with static values.</span></span>  <span data-ttu-id="3a7ba-176">Egy oszlop toobe frissíteni kell, esetén általában nem a megfelelő terjesztési jelölt.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-176">If a column will need toobe updated, it is generally not a good distribution candidate.</span></span>  <span data-ttu-id="3a7ba-177">Ha egy olyan esetben, ha frissítenie kell egy terjesztési oszlop, ez végezhető először a hello sor törlése, és ezután az új sorok beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-177">If there is a case where you must update a distribution column, this can be done by first deleting hello row and then inserting a new row.</span></span>

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a><span data-ttu-id="3a7ba-178">Válassza ki a terjesztési oszlop, amely egyenletes elosztása adatok</span><span class="sxs-lookup"><span data-stu-id="3a7ba-178">Select distribution column which will distribute data evenly</span></span>
<span data-ttu-id="3a7ba-179">Elosztott rendszer csak olyan gyorsan, mint a leglassabb kiosztás hajt végre, mert azt nem fontos toodivide hello munkahelyi egyenletesen hello terjesztéseket rendelés elosztott terhelésű tooachieve végrehajtása hello rendszer között.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-179">Since a distributed system performs only as fast as its slowest distribution, it is important toodivide hello work evenly across hello distributions in order tooachieve balanced execution across hello system.</span></span>  <span data-ttu-id="3a7ba-180">hello munkahelyi oszlik elosztott rendszerek hello módon ahol hello adatokat az egyes terjesztési él alapul.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-180">hello way hello work is divided on a distributed system is based on where hello data for each distribution lives.</span></span>  <span data-ttu-id="3a7ba-181">Ennek köszönhetően nagyon fontos tooselect hello megfelelő terjesztési oszlop hello adatok terjesztéséhez, hogy minden terjesztési egyenlő munkahelyi és a rendszer hajtsa végre a megfelelő hello azonos idő toocomplete hello munkahelyi részét.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-181">This makes it very important tooselect hello right distribution column for distributing hello data so that each distribution has equal work and will take hello same time toocomplete its portion of hello work.</span></span>  <span data-ttu-id="3a7ba-182">Munkahelyi jól oszlik hello rendszer között, amikor hello adatok kiegyensúlyozott hello terjesztéseket között.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-182">When work is well divided across hello system, hello data is balanced across hello distributions.</span></span>  <span data-ttu-id="3a7ba-183">Adatok nem egyenletesen eloszlik, amikor ez nevezzük **adatok döntés**.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-183">When data is not evenly balanced, we call this **data skew**.</span></span>  

<span data-ttu-id="3a7ba-184">toodivide adatok egyenletesen és eltérésére adatok elkerüléséhez hello következő megfontolandó a terjesztési oszlop kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-184">toodivide data evenly and avoid data skew, consider hello following when selecting your distribution column:</span></span>

1. <span data-ttu-id="3a7ba-185">Válasszon ki egy oszlopot, amely számos különböző értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-185">Select a column which contains a significant number of distinct values.</span></span>
2. <span data-ttu-id="3a7ba-186">Ne elosztása néhány különböző értékeket tartalmazó oszlopok adatokat.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-186">Avoid distributing data on columns with a few distinct values.</span></span>
3. <span data-ttu-id="3a7ba-187">Kerülje a NULL értékek nagy gyakorisággal oszlopokon adatok terjesztése.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-187">Avoid distributing data on columns with a high frequency of nulls.</span></span>
4. <span data-ttu-id="3a7ba-188">Kerülje a dátumoszlopot adatok terjesztése.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-188">Avoid distributing data on date columns.</span></span>

<span data-ttu-id="3a7ba-189">Mivel minden egyes érték 60 azokat a terjesztéseket, kivonatolt too1, még akkor is, terjesztési tooachieve érdemes tooselect egy oszlopot, amely magas egyedi, és több mint 60 egyedi értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-189">Since each value is hashed too1 of 60 distributions, tooachieve even distribution you will want tooselect a column that is highly unique and contains more than 60 unique values.</span></span>  <span data-ttu-id="3a7ba-190">tooillustrate, fontolja meg egy olyan esetben, ahol egy oszlop csak rendelkezik 40 egyedi értékeket.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-190">tooillustrate, consider a case where a column only has 40 unique values.</span></span>  <span data-ttu-id="3a7ba-191">Ebben az oszlopban hello terjesztési kulcs volt kiválasztva, ha, hogy a tábla adatai hello volna mindkét 40 azokat a terjesztéseket, legfeljebb 20 terjesztéseket nincsenek adatok, és nem feldolgozási toodo hagyja.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-191">If this column was selected as hello distribution key, hello data for that table would land on 40 distributions at most, leaving 20 distributions with no data and no processing toodo.</span></span>  <span data-ttu-id="3a7ba-192">Ezzel ellentétben hello más 40 terjesztéseket kellene, hogy ha hello adatok lett egyenlően elosztva több mint 60 terjesztéseket további munkahelyi toodo.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-192">Conversely, hello other 40 distributions would have more work toodo that if hello data was evenly spread over 60 distributions.</span></span>  <span data-ttu-id="3a7ba-193">Ebben a forgatókönyvben az adatok eltérésére egy példája.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-193">This scenario is an example of data skew.</span></span>

<span data-ttu-id="3a7ba-194">MPP rendszerben minden egyes lekérdezés lépés megvárja-e a minden terjesztéseket toocomplete hello munka a megosztást.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-194">In MPP system, each query step waits for all distributions toocomplete their share of hello work.</span></span>  <span data-ttu-id="3a7ba-195">Ha egy terjesztési végez műveletet több mint mások hello, akkor a hello erőforrás hello más terjesztéseket vannak lényegében kihasználatlan kell hello foglalt terjesztési csak vár.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-195">If one distribution is doing more work than hello others, then hello resource of hello other distributions are essentially wasted just waiting on hello busy distribution.</span></span>  <span data-ttu-id="3a7ba-196">Ha munkahelyi nem egyenlően elosztva összes azokat a terjesztéseket, ez nevezzük **eltérésére feldolgozási**.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-196">When work is not evenly spread across all distributions, we call this **processing skew**.</span></span>  <span data-ttu-id="3a7ba-197">Feldolgozási döntés lekérdezések toorun lassabb, mint ha hello munkaterhelés is lehet egyenlően elosztva hello terjesztéseket miatt.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-197">Processing skew will cause queries toorun slower than if hello workload can be evenly spread across hello distributions.</span></span>  <span data-ttu-id="3a7ba-198">Adatok döntés eltérésére tooprocessing irányítja.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-198">Data skew will lead tooprocessing skew.</span></span>

<span data-ttu-id="3a7ba-199">Elkerülése hello null értékek összes megnyílik a hello azonos módon magas nullázható oszlopában terjesztése terjesztési.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-199">Avoid distributing on highly nullable column as hello null values will all land on hello same distribution.</span></span> <span data-ttu-id="3a7ba-200">A dátum oszlop terjesztése is eredményezheti, hogy eltérésére feldolgozása, mert egy adott dátumhoz tartozó összes adatot megnyílik a hello azonos terjesztési.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-200">Distributing on a date column can also cause processing skew because all data for a given date will land on hello same distribution.</span></span> <span data-ttu-id="3a7ba-201">Ha több felhasználó is feldolgozás alatt álló összes szűrési lekérdezések hello azonos dátum, akkor csak a hello 60 terjesztéseket 1 lesz aki összes hello munkahelyi mivel egy adott időpontban csak egy terjesztési.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-201">If several users are executing queries all filtering on hello same date, then only 1 of hello 60 distributions will be doing all of hello work since a given date will only be on one distribution.</span></span> <span data-ttu-id="3a7ba-202">Ebben a forgatókönyvben hello lekérdezések valószínűleg lassabb, mint ha hello adatok lettek egyaránt eloszlatva összes hello terjesztéseket 60 alkalommal futtathatja.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-202">In this scenario, hello queries will likely run 60 times slower than if hello data were equally spread over all of hello distributions.</span></span>

<span data-ttu-id="3a7ba-203">Ha nem jó jelölt oszlop létezik, majd vegye fontolóra a ciklikus multiplexelés hello elosztási módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-203">When no good candidate columns exist, then consider using round robin as hello distribution method.</span></span>

### <a name="select-distribution-column-which-will-minimize-data-movement"></a><span data-ttu-id="3a7ba-204">Válassza ki a terjesztési oszlop, ami minimálisra csökkentheti adatátvitel</span><span class="sxs-lookup"><span data-stu-id="3a7ba-204">Select distribution column which will minimize data movement</span></span>
<span data-ttu-id="3a7ba-205">Adatátvitel minimalizálja a hello megfelelő terjesztési oszlop kiválasztásával egyike hello legfontosabb stratégiák az SQL Data Warehouse teljesítményének optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-205">Minimizing data movement by selecting hello right distribution column is one of hello most important strategies for optimizing performance of your SQL Data Warehouse.</span></span>  <span data-ttu-id="3a7ba-206">Adatátvitel leggyakrabban akkor keletkezik, ha a táblák tartományhoz csatlakoztatott vagy Összesítés hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-206">Data Movement most commonly arises when tables are joined or aggregations are performed.</span></span>  <span data-ttu-id="3a7ba-207">Használt az oszlopok `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` és `HAVING` készítése az összes záradékok **jó** terjesztési jelöltek kivonat.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-207">Columns used in `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` and `HAVING` clauses all make for **good** hash distribution candidates.</span></span>

<span data-ttu-id="3a7ba-208">Ugyanakkor, hello oszlopai a hello `WHERE` záradék do **nem** gondoskodjon megfelelő kivonatoló oszlop jelöltek mert azok korlátozza, mely azokat a terjesztéseket hello lekérdezés feldolgozása, amely részt döntés.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-208">On hello other hand, columns in hello `WHERE` clause do **not** make for good hash column candidates because they limit which distributions participate in hello query, causing processing skew.</span></span>  <span data-ttu-id="3a7ba-209">Egy oszlop, amely lehet, tempting toodistribute, de gyakran okozhat a feldolgozási eltérésére jó példa a dátum oszlop.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-209">A good example of a column which might be tempting toodistribute on, but often can cause this processing skew is a date column.</span></span>

<span data-ttu-id="3a7ba-210">Általánosan fogalmazva Ha két nagy ténytáblák gyakran érintett szerepel, érheti el hello legtöbb teljesítményét azáltal, hogy mindkét tábla egyik hello illesztési oszlop.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-210">Generally speaking, if you have two large fact tables frequently involved in a join, you will gain hello most performance by distributing both tables on one of hello join columns.</span></span>  <span data-ttu-id="3a7ba-211">Ha egy táblázatot, amely soha nem csatlakoztatott tooanother nagy ténytábla, majd keresse meg, amelyek gyakran hello toocolumns `GROUP BY` záradékban.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-211">If you have a table that is never joined tooanother large fact table, then look toocolumns that are frequently in hello `GROUP BY` clause.</span></span>

<span data-ttu-id="3a7ba-212">Van néhány főbb feltételeket, amelyek az illesztés során fennállnak tooavoid adatmozgás kell lennie:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-212">There are a few key criteria which must be met tooavoid data movement during a join:</span></span>

1. <span data-ttu-id="3a7ba-213">hello hello illesztési érintett táblákhoz kell lennie a terjesztés kivonatoló **egy** hello oszlopok hello illesztési részt.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-213">hello tables involved in hello join must be hash distributed on **one** of hello columns participating in hello join.</span></span>
2. <span data-ttu-id="3a7ba-214">hello illesztési oszlopok adattípusai hello meg kell egyeznie a két tábla között.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-214">hello data types of hello join columns must match between both tables.</span></span>
3. <span data-ttu-id="3a7ba-215">az egyenlő operátor szerinti szűrése hello oszlopok kell csatlakoznia.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-215">hello columns must be joined with an equals operator.</span></span>
4. <span data-ttu-id="3a7ba-216">hello illesztési típus nem lehet egy `CROSS JOIN`.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-216">hello join type may not be a `CROSS JOIN`.</span></span>

## <a name="troubleshooting-data-skew"></a><span data-ttu-id="3a7ba-217">Hibaelhárítási adatokat eltérésére</span><span class="sxs-lookup"><span data-stu-id="3a7ba-217">Troubleshooting data skew</span></span>
<span data-ttu-id="3a7ba-218">Amikor a tábla adatai hello kivonatoló telepítési módszerrel van egy lehetőségét, hogy az egyes terjesztési lesz toohave aránytalanul több adat, mint a többire válik egyenetlenné.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-218">When table data is distributed using hello hash distribution method there is a chance that some distributions will be skewed toohave disproportionately more data than others.</span></span> <span data-ttu-id="3a7ba-219">Eltérésére túl sok adatot befolyásolhatja a lekérdezési teljesítményt, mert hello végső eredménye egy elosztott lekérdezést kell várnia, hogy hello leghosszabb futó terjesztési toofinish.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-219">Excessive data skew can impact query performance because hello final result of a distributed query must wait for hello longest running distribution toofinish.</span></span> <span data-ttu-id="3a7ba-220">Attól függően, hello adatok eltérésére szükség lehet a tooaddress hello fokú azt.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-220">Depending on hello degree of hello data skew you might need tooaddress it.</span></span>

### <a name="identifying-skew"></a><span data-ttu-id="3a7ba-221">Döntés azonosítása</span><span class="sxs-lookup"><span data-stu-id="3a7ba-221">Identifying skew</span></span>
<span data-ttu-id="3a7ba-222">Egy egyszerű módon tooidentify válik egyenetlenné táblázattá toouse `DBCC PDW_SHOWSPACEUSED`.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-222">A simple way tooidentify a table as skewed is toouse `DBCC PDW_SHOWSPACEUSED`.</span></span>  <span data-ttu-id="3a7ba-223">Ez az egy igen gyors és egyszerű módon toosee hello minden hello 60 disztribúciók az adatbázis tárolt tábla sorok száma.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-223">This is a very quick and simple way toosee hello number of table rows that are stored in each of hello 60 distributions of your database.</span></span>  <span data-ttu-id="3a7ba-224">Ne feledje, hogy a legtöbb kiegyensúlyozott hello teljesítmény érdekében az elosztott tábla sorainak hello kell kell elosztva egyenletesen valamennyi hello felosztást.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-224">Remember that for hello most balanced performance, hello rows in your distributed table should be spread evenly across all hello distributions.</span></span>

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

<span data-ttu-id="3a7ba-225">Azonban ha lekérdezést hajt végre hello Azure SQL Data Warehouse dinamikus felügyeleti nézetekkel (DMV) végezhet részletesebb elemzését.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-225">However, if you query hello Azure SQL Data Warehouse dynamic management views (DMV) you can perform a more detailed analysis.</span></span>  <span data-ttu-id="3a7ba-226">toostart, hello nézet létrehozása [dbo.vTableSizes] [ dbo.vTableSizes] használatával megtekintheti az SQL hello [tábla áttekintése] [ Overview] cikk.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-226">toostart, create hello view [dbo.vTableSizes][dbo.vTableSizes] view using hello SQL from [Table Overview][Overview] article.</span></span>  <span data-ttu-id="3a7ba-227">Hello nézet létrehozása után futtassa a lekérdezés tooidentify táblák rendelkezhet több mint 10 % adatok döntés.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-227">Once hello view is created, run this query tooidentify which tables have more than 10% data skew.</span></span>

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a><span data-ttu-id="3a7ba-228">Adatok eltérésére feloldása</span><span class="sxs-lookup"><span data-stu-id="3a7ba-228">Resolving data skew</span></span>
<span data-ttu-id="3a7ba-229">Nem minden eltérésére van elegendő toowarrant egy javítást.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-229">Not all skew is enough toowarrant a fix.</span></span>  <span data-ttu-id="3a7ba-230">Bizonyos esetekben néhány lekérdezést egy olyan táblázatában hello teljesítményét is járó hello kárt eltérésére adatok.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-230">In some cases, hello performance of a table in some queries can outweigh hello harm of data skew.</span></span>  <span data-ttu-id="3a7ba-231">Ha oldja fel az adatok toodecide döntés egy tábla tisztában kell lennie hello adatok mennyiségéről és lekérdezések lehetőség szerint a terhelést a.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-231">toodecide if you should resolve data skew in a table, you should understand as much as possible about hello data volumes and queries in your workload.</span></span>   <span data-ttu-id="3a7ba-232">Döntés hello hatását a egyirányú toolook toouse hello lépéseit hello [lekérdezés figyelési] [ Query Monitoring] toomonitor hello hatását a lekérdezési teljesítmény eltérésére a következő cikket, és kifejezetten hello hatás toohow hosszú lekérdezések hello egyedi terjesztéseket toocomplete igénybe.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-232">One way toolook at hello impact of skew is toouse hello steps in hello [Query Monitoring][Query Monitoring] article toomonitor hello impact of skew on query performance and specifically hello impact toohow long queries take toocomplete on hello individual distributions.</span></span>

<span data-ttu-id="3a7ba-233">Adatok terjesztése, hogy a rendszer hello egyensúlyt a minimalizálja a adatok döntés és minimalizálja az adatmozgás között.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-233">Distributing data is a matter of finding hello right balance between minimizing data skew and minimizing data movement.</span></span> <span data-ttu-id="3a7ba-234">Ezek is ellentétes célok, és egyes esetekben érdemes tookeep adatok rendelés tooreduce adatátvitelt jelölik a döntés.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-234">These can be opposing goals, and sometimes you will want tookeep data skew in order tooreduce data movement.</span></span> <span data-ttu-id="3a7ba-235">Például ha hello terjesztési oszlop gyakran összekapcsolásokhoz és az aggregációhoz megosztott oszlopa hello, akkor lesz kell minimalizálása adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-235">For example, when hello distribution column is frequently hello shared column in joins and aggregations, you will be minimizing data movement.</span></span> <span data-ttu-id="3a7ba-236">hello hello minimális adatmozgás előnyös lehet, hogy járó döntés adatok hello hatását.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-236">hello benefit of having hello minimal data movement might outweigh hello impact of having data skew.</span></span>

<span data-ttu-id="3a7ba-237">hello jellemző módon tooresolve adatok döntés toore-hello tábla létrehozása egy másik terjesztési oszloppal.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-237">hello typical way tooresolve data skew is toore-create hello table with a different distribution column.</span></span> <span data-ttu-id="3a7ba-238">Mivel nincs mód toochange hello terjesztési oszlop egy olyan tábla, hello módon toochange hello terjesztési táblázat azt toorecreate azt egy [CTAS] [].</span><span class="sxs-lookup"><span data-stu-id="3a7ba-238">Since there is no way toochange hello distribution column on an existing table, hello way toochange hello distribution of a table it toorecreate it with a [CTAS][].</span></span>  <span data-ttu-id="3a7ba-239">Az alábbiakban a két példa bemutatja, hogyan oldhatók fel az adatok eltérésére:</span><span class="sxs-lookup"><span data-stu-id="3a7ba-239">Here are two examples of how resolve data skew:</span></span>

### <a name="example-1-re-create-hello-table-with-a-new-distribution-column"></a><span data-ttu-id="3a7ba-240">1. példa: Hozza létre egy új terjesztési oszlop hello tábla</span><span class="sxs-lookup"><span data-stu-id="3a7ba-240">Example 1: Re-create hello table with a new distribution column</span></span>
<span data-ttu-id="3a7ba-241">Ez a példa [CTAS] [] toore-hozzon létre egy táblát egy másik kivonatoló terjesztési oszlop.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-241">This example uses [CTAS][] toore-create a table with a different hash distribution column.</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] too[FactInternetSales];
```

### <a name="example-2-re-create-hello-table-using-round-robin-distribution"></a><span data-ttu-id="3a7ba-242">2. példa: Hozza létre a ciklikus multiplexelés terjesztés hello tábla</span><span class="sxs-lookup"><span data-stu-id="3a7ba-242">Example 2: Re-create hello table using round robin distribution</span></span>
<span data-ttu-id="3a7ba-243">Ez a példa [CTAS] [] toore-ciklikus multiplexelés kivonatoló terjesztési helyett hozzon létre egy táblát.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-243">This example uses [CTAS][] toore-create a table with round robin instead of a hash distribution.</span></span> <span data-ttu-id="3a7ba-244">Ez a változás a művelet létrehoz még akkor is, adatok terjesztési hello költségekkel megnövekedett adat mozgása.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-244">This change will produce even data distribution at hello cost of increased data movement.</span></span>

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] too[FactInternetSales];
```

## <a name="next-steps"></a><span data-ttu-id="3a7ba-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a7ba-245">Next steps</span></span>
<span data-ttu-id="3a7ba-246">toolearn Táblatervezés, kapcsolatos további információkért lásd: hello [elosztás][Distribute], [Index][Index], [partíció] [ Partition], [Adattípusok][Data Types], [statisztika] [ Statistics] és [ideiglenes táblák] [ Temporary] cikkeket.</span><span class="sxs-lookup"><span data-stu-id="3a7ba-246">toolearn more about table design, see hello [Distribute][Distribute], [Index][Index], [Partition][Partition], [Data Types][Data Types], [Statistics][Statistics] and [Temporary Tables][Temporary] articles.</span></span>

<span data-ttu-id="3a7ba-247">Ajánlott eljárások áttekintését lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="3a7ba-247">For an overview of best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>

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
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
