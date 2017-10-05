---
title: "Adatbázis-kompatibilitási szint 130 - Azure SQL Database |} Microsoft Docs"
description: "Ez a cikk azt előnyei az Azure SQL Database 130 kompatibilitási szinten fut, és hogy az új lekérdezésoptimalizáló és a lekérdezés processzorfunkciókat előnyeit kihasználva. Azt is eleget a lehetséges-hatással a lekérdezési teljesítményt, a meglévő SQL-alkalmazásokhoz."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 08/08/2016
ms.author: alainl
ms.openlocfilehash: c08c0690df4f389416e4ed2e2df2dbb72d6fd567
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a><span data-ttu-id="43102-104">Javult a lekérdezési teljesítmény kompatibilitási szint 130 az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="43102-104">Improved query performance with compatibility Level 130 in Azure SQL Database</span></span>
<span data-ttu-id="43102-105">Azure SQL-adatbázis fut transzparens módon akár több ezer adatbázis több száz számos különböző kompatibilitási szinten megőrzi, és az összes ügyfél számára a megfelelő verzióra, a Microsoft SQL Server az előző verziókkal való kompatibilitás biztosítása!</span><span class="sxs-lookup"><span data-stu-id="43102-105">Azure SQL Database is running transparently hundreds of thousands of databases at many different compatibility levels, preserving and guaranteeing the backward compatibility to the corresponding version of Microsoft SQL Server for all its customers!</span></span>

<span data-ttu-id="43102-106">Ez a cikk azt előnyei az Azure SQL adatbázismodell 130 kompatibilitási szinten fut, és az új lekérdezésoptimalizáló és a lekérdezés processzorfunkciókat előnyeit kihasználva.</span><span class="sxs-lookup"><span data-stu-id="43102-106">In this article, we explore the benefits of running your Azure SQL Databse at compatibility level 130, and leveraging the benefits of the new query optimizer and query processor features.</span></span> <span data-ttu-id="43102-107">Azt is eleget a lehetséges-hatással a lekérdezési teljesítményt, a meglévő SQL-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="43102-107">We also address the possible side-effects on the query performance for the existing SQL applications.</span></span>

<span data-ttu-id="43102-108">Ne feledje az előzményeket SQL-verziók alapértelmezett kompatibilitási szintre igazítása a következők:</span><span class="sxs-lookup"><span data-stu-id="43102-108">As a reminder of history, the alignment of SQL versions to default compatibility levels are as follows:</span></span>

* <span data-ttu-id="43102-109">100: az SQL Server 2008 és az Azure SQL adatbázis-V11.</span><span class="sxs-lookup"><span data-stu-id="43102-109">100: in SQL Server 2008 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="43102-110">110: az SQL Server 2012 és az Azure SQL adatbázis-V11.</span><span class="sxs-lookup"><span data-stu-id="43102-110">110: in SQL Server 2012 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="43102-111">120: az SQL Server 2014 és az Azure SQL-adatbázis 12-es verzió.</span><span class="sxs-lookup"><span data-stu-id="43102-111">120: in SQL Server 2014 and Azure SQL Database V12.</span></span>
* <span data-ttu-id="43102-112">130: az SQL Server 2016-os és az Azure SQL-adatbázis 12-es verzió.</span><span class="sxs-lookup"><span data-stu-id="43102-112">130: in SQL Server 2016 and Azure SQL Database V12.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="43102-113">Kezdve **2016. június mid**, az Azure SQL-adatbázis, az alapértelmezett kompatibilitási szintje lesz helyett 120 130 **az újonnan létrehozott** adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="43102-113">Starting in **mid-June 2016**, in Azure SQL Database, the default compatibility level will be 130 instead of 120 for **newly created** databases.</span></span>
> 
> <span data-ttu-id="43102-114">2016. június mid előtt létrehozott adatbázisokat fogja *nem* érinti, és a jelenlegi kompatibilitási szint (100, 110-esre vagy 120) rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="43102-114">Databases created before mid-June 2016 will *not* be affected, and will maintain their current compatibility level (100, 110, or 120).</span></span> <span data-ttu-id="43102-115">V12 Azure SQL Database V11-es verziójából áttelepített adatbázisok fog rendelkezni 100 vagy a 110-es kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="43102-115">Databases that migrated from Azure SQL Database version V11 to V12 will have a compatibility level of either 100 or 110.</span></span> 
> 

## <a name="about-compatibility-level-130"></a><span data-ttu-id="43102-116">130 kompatibilitási szintre vonatkozó</span><span class="sxs-lookup"><span data-stu-id="43102-116">About compatibility level 130</span></span>
<span data-ttu-id="43102-117">Ha szeretné tudni, hogy az adatbázis jelenlegi kompatibilitási szintjét, először a következő Transact-SQL-utasítás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="43102-117">First, if you want to know the current compatibility level of your database, execute the following Transact-SQL statement.</span></span>

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


<span data-ttu-id="43102-118">Mielőtt szint 130 a módosítás történik az **újonnan** hozott létre adatbázisok, most áttekintheti az ezt a módosítást minden szól bizonyos nagyon alapszintű lekérdezés példákból, és tekintse meg, hogyan bárki is kihasználhatja le azt.</span><span class="sxs-lookup"><span data-stu-id="43102-118">Before this change to level 130 happens for **newly** created databases, let’s review what this change is all about through some very basic query examples, and see how anyone can benefit from it.</span></span>

<span data-ttu-id="43102-119">A lekérdezés feldolgozása a relációs adatbázisok nagyon összetett feladat lehet, és nagy mennyiségű számítástechnikai és matematikai megtudhatja, hogy a rejlő tervezési döntések ütköznek azokkal és viselkedéshez vezethet.</span><span class="sxs-lookup"><span data-stu-id="43102-119">Query processing in relational databases can be very complex and can lead to lots of computer science and mathematics to understand the inherent design choices and behaviors.</span></span> <span data-ttu-id="43102-120">Ebben a dokumentumban a tartalom szándékosan egyszerűsített, győződjön meg arról, hogy bárki, aki néhány minimális technikai háttér megismerheti a hatását a kompatibilitási szint változás, és határozza meg az alkalmazások használatának előnyei.</span><span class="sxs-lookup"><span data-stu-id="43102-120">In this document, the content has been intentionally simplified to ensure that anyone with some minimum technical background can understand the impact of the compatibility level change and determine how it can benefit applications.</span></span>

<span data-ttu-id="43102-121">Mi a kompatibilitási szint 130 számos lehetőséget kínál a táblát gyorsan át most rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="43102-121">Let’s have a quick look at what the compatibility level 130 brings at the table.</span></span>  <span data-ttu-id="43102-122">További részletei: [ALTER adatbázis kompatibilitási szintje (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), de rövid összegzése:</span><span class="sxs-lookup"><span data-stu-id="43102-122">You can find more details at [ALTER DATABASE Compatibility Level (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), but here is a short summary:</span></span>

* <span data-ttu-id="43102-123">Az Insert művelet Insert választás utasítás többszálas lehetnek, vagy hogy a párhuzamos terv, mielőtt ez a művelet lett egyszálas során.</span><span class="sxs-lookup"><span data-stu-id="43102-123">The Insert operation of an Insert-select statement can be multi-threaded or can have a parallel plan, while before this operation was single-threaded.</span></span>
* <span data-ttu-id="43102-124">Memória optimalizált tábla és a tábla változók lekérdezések most lehet párhuzamos terveket, mielőtt ez a művelet lett is egyszálas során.</span><span class="sxs-lookup"><span data-stu-id="43102-124">Memory Optimized table and table variables queries can now have parallel plans, while before this operation was also single-threaded .</span></span>
* <span data-ttu-id="43102-125">A Memóriaoptimalizált tábla statisztikája most mintát vehetnek és automatikus frissítése.</span><span class="sxs-lookup"><span data-stu-id="43102-125">Statistics for Memory Optimized table can now be sampled and are auto-updated.</span></span> <span data-ttu-id="43102-126">Lásd: [az adatbázis-kezelő újdonságai: memórián belüli online Tranzakciófeldolgozási](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="43102-126">See [What's New in Database Engine: In-Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) for more details.</span></span>
* <span data-ttu-id="43102-127">Az oszlop az Oszlopcentrikus indexek módosítja sor módú kötegelt módban v/s</span><span class="sxs-lookup"><span data-stu-id="43102-127">Batch mode v/s Row Mode changes with Column Store indexes</span></span>
  * <span data-ttu-id="43102-128">Egy oszlop Oszlopcentrikus indexszel rendelkező táblán rendezi most kötegelt módban van.</span><span class="sxs-lookup"><span data-stu-id="43102-128">Sorts on a table with a Column Store index are now in batch mode.</span></span>
  * <span data-ttu-id="43102-129">Leképezési összesítések kötegelt módban, például a TSQL LAG/ÁTFUTÁSI utasítások működni.</span><span class="sxs-lookup"><span data-stu-id="43102-129">Windowing aggregates now operate in batch mode such as TSQL LAG/LEAD statements.</span></span>
  * <span data-ttu-id="43102-130">A distinct záradékban több oszlop tárolási táblákon lekérdezések kötegelt módban működik.</span><span class="sxs-lookup"><span data-stu-id="43102-130">Queries on Column Store tables with Multiple distinct clauses operate in Batch mode.</span></span>
  * <span data-ttu-id="43102-131">DOP mellett futó lekérdezésekhez = 1 vagy soros csomagot is hajtsa végre a kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="43102-131">Queries running under DOP=1 or with a serial plan also execute in Batch Mode.</span></span>
* <span data-ttu-id="43102-132">Utolsó számossága becslés fejlesztései ténylegesen érkeznek-es kompatibilitási szintű 120, de azokat (azaz 100 vagy 110), alacsonyabb kompatibilitási szintű futtatásával az áthelyezés kompatibilitási szint 130 is megjelenik a fejlesztéseknek, és ezeket is a lekérdezési teljesítményt, az alkalmazások kihasználják.</span><span class="sxs-lookup"><span data-stu-id="43102-132">Last, Cardinality Estimation improvements are actually coming with compatibility level 120, but for those of you running at a lower Compatibility level (i.e. 100, or 110), the move to compatibility level 130 will also bring these improvements, and these can also benefit the query performance of your applications.</span></span>

## <a name="practicing-compatibility-level-130"></a><span data-ttu-id="43102-133">Kompatibilitási szint 130 gyakorlás</span><span class="sxs-lookup"><span data-stu-id="43102-133">Practicing compatibility level 130</span></span>
<span data-ttu-id="43102-134">Első folytassuk az egyes táblák, indexek és az új lehetőségekhez némelyike gyakorlata létrehozott véletlenszerű adatokat.</span><span class="sxs-lookup"><span data-stu-id="43102-134">First let’s get some tables, indexes and random data created to practice some of these new capabilities.</span></span> <span data-ttu-id="43102-135">A TSQL-parancsfájl például SQL Server 2016-os, illetve az Azure SQL Database hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="43102-135">The TSQL script examples can be executed under SQL Server 2016, or under Azure SQL Database.</span></span> <span data-ttu-id="43102-136">Azure SQL-adatbázis létrehozása esetén ellenőrizze, hogy választja, a legalább P2 adatbázis, mivel szüksége legalább néhány többszálas engedélyezése, és ezért ezek a funkciók kihasználhassa magszámra vonatkozó követelmény.</span><span class="sxs-lookup"><span data-stu-id="43102-136">However, when creating an Azure SQL database, make sure you choose at the minimum a P2 database because you need at least a couple of cores to allow multi-threading and therefore benefit from these features.</span></span>

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


<span data-ttu-id="43102-137">Most tegyük a tekintse meg a lekérdezés feldolgozása funkciók hamarosan-es kompatibilitási szintű 130 egyes rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="43102-137">Now, let’s have a look to some of the Query Processing features coming with compatibility level 130.</span></span>

## <a name="parallel-insert"></a><span data-ttu-id="43102-138">Párhuzamos BESZÚRÁSA</span><span class="sxs-lookup"><span data-stu-id="43102-138">Parallel INSERT</span></span>
<span data-ttu-id="43102-139">Az INSERT művelet a kompatibilitási szint 120 és 130, melyik rendre egyetlen összefűzött modellben (120), és a többszálas modellben (130) hajtja végre a beszúrási művelet végrehajtása a TSQL-utasításokat az alábbi hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="43102-139">Executing the TSQL statements below executes the INSERT operation under compatibility level 120 and 130, which respectively executes the INSERT operation in a single threaded model (120), and in a multi-threaded model (130).</span></span>

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-140">Kérésével a tényleges a lekérdezésterv, és megnézi a grafikus ábrázolása vagy az XML-tartalmat, mely számossága becslés függvény jelenleg play azt is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="43102-140">By requesting the actual the query plan, looking at its graphical representation or its XML content, you can determine which Cardinality Estimation function is at play.</span></span> <span data-ttu-id="43102-141">1. ábra a megnézi a tervek-mellé, egyértelműen láthatja, hogy az oszlop tároló BESZÚRÁSA végrehajtási tartalma a 120 soros párhuzamos 130.</span><span class="sxs-lookup"><span data-stu-id="43102-141">Looking at the plans side-by-side on figure 1, we can clearly see that the Column Store INSERT execution goes from serial in 120 to parallel in 130.</span></span> <span data-ttu-id="43102-142">Emellett vegye figyelembe, hogy a módosítás a iterátor ikon megjelenítése két párhuzamos nyilat ábrázoló azt a tényt, ami már iterátor végrehajtása 130 tervben valóban párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="43102-142">Also, note that the change of the iterator icon in the 130 plan showing two parallel arrows, illustrating the fact that now the iterator execution is indeed parallel.</span></span> <span data-ttu-id="43102-143">Ha nagy a beszúrási műveletek végrehajtásához, csatolva van az adatbázis, a rendelkezésére core száma párhuzamos végrehajtása javítja a teljesítményt; legfeljebb 100 alkalommal, amely gyorsabb a helyzettől függően!</span><span class="sxs-lookup"><span data-stu-id="43102-143">If you have large INSERT operations to complete, the parallel execution, linked to the number of core you have at your disposal for the database, will perform better; up to a 100 times faster depending your situation!</span></span>

<span data-ttu-id="43102-144">*1. ábra: INSERT művelet állapotról soros párhuzamos kompatibilitási szint 130.*</span><span class="sxs-lookup"><span data-stu-id="43102-144">*Figure 1: INSERT operation changes from serial to parallel with compatibility level 130.*</span></span>

![1. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a><span data-ttu-id="43102-146">SOROS kötegelt módban</span><span class="sxs-lookup"><span data-stu-id="43102-146">SERIAL Batch Mode</span></span>
<span data-ttu-id="43102-147">Hasonlóképpen a áthelyezését a kompatibilitási szintet 130 sornyi adatot feldolgozásakor engedélyezi kötegfeldolgozási mód.</span><span class="sxs-lookup"><span data-stu-id="43102-147">Similarly, moving to compatibility level 130 when processing rows of data enables batch mode processing.</span></span> <span data-ttu-id="43102-148">Először kötegelt módban futó műveletekhez esetén csak érhetők el egy oszlopcentrikus indexszel érvényben van.</span><span class="sxs-lookup"><span data-stu-id="43102-148">First, batch mode operations  are only available when you have a column store index in place.</span></span> <span data-ttu-id="43102-149">A kötegelt, általában jelöl ~ 900 sorok, és használja a kód logika multicore Processzor, memória nagyobb átviteli optimalizálva, és közvetlenül lehetővé teszi a tömörített adatok az oszlop-tároló, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="43102-149">Second, a batch typically represents ~900 rows, and uses a code logic optimized for multicore CPU, higher memory throughput and directly leverages the compressed data of the Column Store whenever possible.</span></span> <span data-ttu-id="43102-150">Ebben az SQL Server 2016 is feldolgozni ~ 900 sorok egyszerre, helyett 1 sor időpontjában, és következtében a művelet a teljes általános költség most közösen az egész köteget, csökkenti a költségeket, soronként teljes.</span><span class="sxs-lookup"><span data-stu-id="43102-150">Under these conditions, SQL Server 2016 can process ~900 rows at once, instead of 1 row at the time, and as a consequence, the overall overhead cost of the operation is now shared by the entire batch, reducing the overall cost by row.</span></span> <span data-ttu-id="43102-151">A műveletek a oszlop tároló tömörítés alapvetően együtt megosztott mennyisége egy SELECT kötegelt módban műveletben érintett késését csökkentő.</span><span class="sxs-lookup"><span data-stu-id="43102-151">This shared amount of operations combined with the column store compression basically reduces the latency involved in a SELECT batch mode operation.</span></span> <span data-ttu-id="43102-152">Az oszlop-áruházzal kapcsolatos további részletekért található, és kötegelt módban, [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="43102-152">You can find more details about the column store and batch mode at [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-153">Alább látható, mint betartásával a lekérdezés 2. ábra tervek egymás mellett, láthatja, hogy a feldolgozás módját kompatibilitási szintű megváltozott, és ennek következtében, ha a lekérdezések végrehajtása mindkét kompatibilitási szint regisztrálását, láthatja, hogy a legtöbb a feldolgozási időt töltött sor módú (86 %) képest kötegelt módtól (14 %), ahol 2 kötegek dolgozott.</span><span class="sxs-lookup"><span data-stu-id="43102-153">As visible below, by observing the query plans side-by-side on figure 2, we can see that the processing mode has changed with the compatibility level, and as a consequence, when executing the queries in both compatibility level altogether, we can see that most of the processing time is spent in row mode (86%) compared to the batch mode (14%), where 2 batches have been processed.</span></span> <span data-ttu-id="43102-154">A dataset növeléséhez a juttatás növeli.</span><span class="sxs-lookup"><span data-stu-id="43102-154">Increase the dataset, the benefit will increase.</span></span>

<span data-ttu-id="43102-155">*2. ábra: A soros művelet módosítások BEJELÖLÉSÉVEL-es kompatibilitási szintű 130 kötegelt módban.*</span><span class="sxs-lookup"><span data-stu-id="43102-155">*Figure 2: SELECT operation changes from serial to batch mode with compatibility level 130.*</span></span>

![2. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a><span data-ttu-id="43102-157">A rendezési végrehajtási kötegelt módban</span><span class="sxs-lookup"><span data-stu-id="43102-157">Batch mode on Sort Execution</span></span>
<span data-ttu-id="43102-158">Hasonló a fenti, de a rendezési művelet életbe lép az átmenet kötegelt módban (kompatibilitási szint 130) sor üzemmódról (120 kompatibilitási szint) javítja a okok miatt a rendezési művelet.</span><span class="sxs-lookup"><span data-stu-id="43102-158">Similar to the above, but applied to a sort operation, the transition from row mode (compatibility level 120) to batch mode (compatibility level 130) improves the performance of the SORT operation for the same reasons.</span></span>

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-159">Látható-mellé a 3. ábra, láthatja, hogy a rendezési művelet sor módú jelöli 81-es %-át, amíg a kötegelt módban csak a 19 %-át (illetve 81-56 % meg magát a rendezés) jelöli.</span><span class="sxs-lookup"><span data-stu-id="43102-159">Visible side-by-side on figure 3, we can see that the sort operation in row mode represents 81% of the cost, while the batch mode only represents 19% of the cost (respectively 81% and 56% on the sort itself).</span></span>

<span data-ttu-id="43102-160">*3. ábra: Rendezési művelet módosítja sor-es kompatibilitási szintű 130 kötegelt módban.*</span><span class="sxs-lookup"><span data-stu-id="43102-160">*Figure 3: SORT operation changes from row to batch mode with compatibility level 130.*</span></span>

![3. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

<span data-ttu-id="43102-162">Természetesen ezeket a mintákat csak tartalmazhat tízezreit sorok, vagyis semmi kezelőkonzoljából nézve a legtöbb SQL-kiszolgáló elérhető adatok ezek a napok.</span><span class="sxs-lookup"><span data-stu-id="43102-162">Obviously, these samples only contain tens of thousands of rows, which is nothing when looking at the data available in most SQL Servers these days.</span></span> <span data-ttu-id="43102-163">Csak ezek alapján több millió sort helyette projektre, és ezt tudja lefordítani a függőben lévő a számítási feladatok jellege naponta kímélni végrehajtás néhány percig.</span><span class="sxs-lookup"><span data-stu-id="43102-163">Just project these against millions of rows instead, and this can translate in several minutes of execution spared every day pending the nature of your workload.</span></span>

## <a name="cardinality-estimation-ce-improvements"></a><span data-ttu-id="43102-164">Számossága becslése (CE) fejlesztései</span><span class="sxs-lookup"><span data-stu-id="43102-164">Cardinality Estimation (CE) improvements</span></span>
<span data-ttu-id="43102-165">Az SQL Server 2014-ban bevezetett, bármilyen adatbázis-kompatibilitási szinten 120 vagy újabb rendszeren futó igénybe veszik a új számossága a vizsgálat funkciót.</span><span class="sxs-lookup"><span data-stu-id="43102-165">Introduced with SQL Server 2014, any database running at a compatibility level 120 or above will make use of the new Cardinality Estimation functionality.</span></span> <span data-ttu-id="43102-166">Számossága becslése lényegében az határozza meg, hogyan hajtható végre SQL server egy lekérdezést a becsült költség alapuló logikai.</span><span class="sxs-lookup"><span data-stu-id="43102-166">Essentially, cardinality estimation is the logic used to determine how SQL server will execute a query based on its estimated cost.</span></span> <span data-ttu-id="43102-167">A becslés lekérdezés érintett objektumok statisztika bemenetének használatával történik.</span><span class="sxs-lookup"><span data-stu-id="43102-167">The estimation is calculated using input from statistics associated with objects involved in that query.</span></span> <span data-ttu-id="43102-168">Gyakorlatilag, egy magas szintű számossága becslés funkciók hozzávetőlegesek sor száma együtt a különböző érték számát, az értékek kapcsolatos információkat és ismétlődő száma szerepel a táblákat és a lekérdezésben hivatkozott objektumok.</span><span class="sxs-lookup"><span data-stu-id="43102-168">Practically, at a high-level, Cardinality Estimation functions are row count estimates along with information about the distribution of the values, distinct value counts, and duplicate counts contained in the tables and objects referenced in the query.</span></span> <span data-ttu-id="43102-169">Ezek a becslések megfelelő első, vezethet szükségtelen lemez i/o miatt nincs elegendő memória biztosít (azaz a TempDB kijutás), vagy egy soros terv végrehajtási kijelölt keresztül egy párhuzamos terv végrehajtási néhányat említsünk.</span><span class="sxs-lookup"><span data-stu-id="43102-169">Getting these estimates wrong, can lead to unnecessary disk I/O due to insufficient memory grants (i.e. TempDB spills), or to a selection of a serial plan execution over a parallel plan execution, to name a few.</span></span> <span data-ttu-id="43102-170">Megkötését, helytelen becslése egy átfogó teljesítménycsökkenés a lekérdezés-végrehajtás vezethet.</span><span class="sxs-lookup"><span data-stu-id="43102-170">Conclusion, incorrect estimates can lead to an overall performance degradation of the query execution.</span></span> <span data-ttu-id="43102-171">A másik oldalon jobb becsléseket, pontosabb becslést vezet jobb lekérdezés végrehajtások!</span><span class="sxs-lookup"><span data-stu-id="43102-171">On the other side, better estimates, more accurate estimates, leads to better query executions!</span></span>

<span data-ttu-id="43102-172">Ahogy korábban említettük, lekérdezés optimalizálása és becslése egy összetett függetlenül attól, hogy, de ha azt szeretné, további információt a lekérdezésterveket és számosságbecslő, a dokumentum hivatkozhat [a lekérdezésterveket optimalizálja az SQL Server 2014 kulcsattribútumával Négyzetgyökének](https://msdn.microsoft.com/library/dn673537.aspx) mélyebb bemutatója a.</span><span class="sxs-lookup"><span data-stu-id="43102-172">As mentioned before, query optimizations and estimates are a complex matter, but if you want to learn more about query plans and cardinality estimator, you can refer to the document at [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) for a deeper dive.</span></span>

## <a name="which-cardinality-estimation-do-you-currently-use"></a><span data-ttu-id="43102-173">Mely számossága becslés jelenleg használ?</span><span class="sxs-lookup"><span data-stu-id="43102-173">Which Cardinality Estimation do you currently use?</span></span>
<span data-ttu-id="43102-174">Határozza meg, hogy mely számossága becslése a lekérdezéseket futtat, a most használja az alábbi lekérdezés minták.</span><span class="sxs-lookup"><span data-stu-id="43102-174">To determine under which Cardinality Estimation your queries are running, let’s just use the query samples below.</span></span> <span data-ttu-id="43102-175">Vegye figyelembe, hogy ez a példa első kompatibilitási szintjét 110-re, a régi számossága becslés funkciók használatát fognak futni.</span><span class="sxs-lookup"><span data-stu-id="43102-175">Note that this first example will run under compatibility level 110, implying the use of the old Cardinality Estimation functions.</span></span>

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-176">Végrehajtási végrehajtása után XML hivatkozásra, és nézze meg az első iterátor tulajdonságainak alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="43102-176">Once execution is complete, click on the XML link, and look at the properties of the first iterator as shown below.</span></span> <span data-ttu-id="43102-177">Vegye figyelembe a beállított 70 CardinalityEstimationModelVersion nevű tulajdonságnév.</span><span class="sxs-lookup"><span data-stu-id="43102-177">Note the property name called CardinalityEstimationModelVersion currently set on 70.</span></span> <span data-ttu-id="43102-178">Ez nem jelenti azt, hogy az adatbázis kompatibilitási szintje van beállítva az SQL Server 7.0 verziójára (van állítva, akkor a 110-esre látható a fenti TSQL utasításokban, mint), de az érték 70 egyszerűen jelöli a örökölt számossága becslés funkció érhető el az SQL Server 7.0 óta Nincs fő verziók amely addig az SQL Server 2014 (amely 120 kompatibilitási szintje).</span><span class="sxs-lookup"><span data-stu-id="43102-178">It does not mean that the database compatibility level is set to the SQL Server 7.0 version (it is set on 110 as visible in the TSQL statements above), but the value 70 simply represents the legacy Cardinality Estimation functionality available since SQL Server 7.0, which had no major revisions until SQL Server 2014 (which comes with a compatibility level of 120).</span></span>

<span data-ttu-id="43102-179">*4. ábra: A CardinalityEstimationModelVersion értéke 70 110-esre vagy alatti kompatibilitási szintje használatakor.*</span><span class="sxs-lookup"><span data-stu-id="43102-179">*Figure 4: The CardinalityEstimationModelVersion is set to 70 when using a compatibility level of 110 or below.*</span></span>

![4. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

<span data-ttu-id="43102-181">Azt is megteheti, módosítsa a kompatibilitási szintjét 130, és az új számossága becslés függvény használatának letiltása a LEGACY_CARDINALITY_ESTIMATION beállítása ON a használatával [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://msdn.microsoft.com/library/mt629158.aspx).</span><span class="sxs-lookup"><span data-stu-id="43102-181">Alternatively, you can change the compatibility level to 130, and disable the use of the new Cardinality Estimation function by using the LEGACY_CARDINALITY_ESTIMATION set to ON with [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span></span> <span data-ttu-id="43102-182">Ez lesz pontosan megegyezik a 110-re a számosság becslés függvény szempontjából, a legújabb lekérdezés feldolgozása a kompatibilitási szint használata során.</span><span class="sxs-lookup"><span data-stu-id="43102-182">This will be exactly the same as using 110 from a Cardinality Estimation function point of view, while using the latest query processing compatibility level.</span></span> <span data-ttu-id="43102-183">Ezáltal kihasználhassa az új lekérdezés feldolgozása a funkciók hamarosan legfrissebb kompatibilitási szintű (azaz kötegelt módban), de továbbra is a régi számossága becslés funkció támaszkodnak szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="43102-183">Doing so, you can benefit from the new query processing features coming with the latest compatibility level (i.e. batch mode), but still rely on the old Cardinality Estimation functionality if necessary.</span></span>

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-184">A kompatibilitási szint 120 vagy 130 egyszerűen áthelyezése lehetővé teszi, hogy az új számossága becslés funkciókat.</span><span class="sxs-lookup"><span data-stu-id="43102-184">Simply moving to the compatibility level 120 or 130 enables the new Cardinality Estimation functionality.</span></span> <span data-ttu-id="43102-185">Ebben az esetben az alapértelmezett CardinalityEstimationModelVersion lesz beállítva, ennek megfelelően 120 vagy 130, az alább látható.</span><span class="sxs-lookup"><span data-stu-id="43102-185">In such a case, the default CardinalityEstimationModelVersion will be set accordingly to 120 or 130 as visible below.</span></span>

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-186">*5. ábra: A CardinalityEstimationModelVersion értéke 130 130 kompatibilitási szintje használatakor.*</span><span class="sxs-lookup"><span data-stu-id="43102-186">*Figure 5: The CardinalityEstimationModelVersion is set to 130 when using a compatibility level of 130.*</span></span>

![5. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a><span data-ttu-id="43102-188">A számosság becslés különbségek aláírnia</span><span class="sxs-lookup"><span data-stu-id="43102-188">Witnessing the Cardinality Estimation differences</span></span>
<span data-ttu-id="43102-189">Most tegyük futtassa kissé összetettebb érintő INNER JOIN néhány predikátumok a WHERE záradék a lekérdezés, és vizsgáljuk meg a sor számának becslése a régi számossága becslés függvény első.</span><span class="sxs-lookup"><span data-stu-id="43102-189">Now, let’s run a slightly more complex query involving an INNER JOIN with a WHERE clause with some predicates, and let’s look at the row count estimate from the old Cardinality Estimation function first.</span></span>

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-190">A lekérdezés végrehajtása hatékonyan sorát adja vissza 200,704, amíg a régi számossága becslés funkciójú sor becsült jogcímek 194,284 sorok.</span><span class="sxs-lookup"><span data-stu-id="43102-190">Executing this query effectively returns 200,704 rows, while the row estimate with the old Cardinality Estimation functionality claims 194,284 rows.</span></span> <span data-ttu-id="43102-191">Természetesen, az említett előtt, sor száma keresztillesztésével is függ, milyen gyakran futtatta a korábbi mintákat, amelyek tölti fel a minta táblák minden egyes futtatásához, és újra.</span><span class="sxs-lookup"><span data-stu-id="43102-191">Obviously, as said before, these row count results will also depend how often you ran the previous samples, which populates the sample tables over and over again at each run.</span></span> <span data-ttu-id="43102-192">Természetesen a lekérdezés predikátumok is vezérelt a tábla alakzat adattartalmat, a tényleges becslés hatással, és hogyan ezek az adatok tényleges összefüggéseket egymással.</span><span class="sxs-lookup"><span data-stu-id="43102-192">Obviously, the predicates in your query will also have an influence on the actual estimation aside from the table shape, data content, and how this data actually correlate with each other.</span></span>

<span data-ttu-id="43102-193">*6. ábra: A sor számának becslése a rendszer nem 194,284 vagy 6000 sor a várt 200,704 sorokat.*</span><span class="sxs-lookup"><span data-stu-id="43102-193">*Figure 6: The row count estimate is 194,284 or 6,000 rows off from the 200,704 rows expected.*</span></span>

![6. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

<span data-ttu-id="43102-195">Ugyanúgy most most végrehajtani ugyanabban a lekérdezésben új számossága becslés funkció.</span><span class="sxs-lookup"><span data-stu-id="43102-195">In the same way, let’s now execute the same query with the new Cardinality Estimation functionality.</span></span>

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="43102-196">Megnézi a lent most látható, hogy a sorok becsült 202,877, vagy sokkal szorosabb és magasabb, mint a régi számossága becslés-e.</span><span class="sxs-lookup"><span data-stu-id="43102-196">Looking at the below, we now see that the row estimate is 202,877, or much closer and higher than the old Cardinality Estimation.</span></span>

<span data-ttu-id="43102-197">*7. ábra: A sor számának becslése már 202,877 194,284 helyett.*</span><span class="sxs-lookup"><span data-stu-id="43102-197">*Figure 7: The row count estimate is now 202,877, instead of 194,284.*</span></span>

![7. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

<span data-ttu-id="43102-199">A valóságban eredménykészlet 200,704 sorok (de az összes függ, milyen gyakran az előző minták lekérdezések futott, de ennél is fontosabb, a TSQL a RAND() utasítást használja, mert a visszaadott tényleges értékek eltérőek lehetnek egy futni, hanem a következő).</span><span class="sxs-lookup"><span data-stu-id="43102-199">In reality, the result set is 200,704 rows (but all of it depends how often you did run the queries of the previous samples, but more importantly, because the TSQL uses the RAND() statement, the actual values returned can vary from one run to the next).</span></span> <span data-ttu-id="43102-200">A konkrét példában az új számossága becslés nem, mert 202,877 sokkal közelebb 200,704, mint 194,284 sorok számának becslése jobb munkát!</span><span class="sxs-lookup"><span data-stu-id="43102-200">Therefore, in this particular example, the new Cardinality Estimation does a better job at estimating the number of rows because 202,877 is much closer to 200,704, than 194,284!</span></span> <span data-ttu-id="43102-201">Az utolsó, ha módosítja a WHERE záradékban predikátumokat az egyenlő (helyett ">" példány), ez teheti a becslések között a régi és új számossága függvény még több különböző, attól függően, hogy hány megegyezik kérheti le.</span><span class="sxs-lookup"><span data-stu-id="43102-201">Last, if you change the WHERE clause predicates to equality (rather than “>” for instance), this could make the estimates between the old and new Cardinality function even more different, depending on how many matches you can get.</span></span>

<span data-ttu-id="43102-202">Természetesen ebben az esetben ~ 6000 sort éppen ki a tényleges száma nem felel meg nagy mennyiségű adat bizonyos esetekben.</span><span class="sxs-lookup"><span data-stu-id="43102-202">Obviously, in this case, being ~6000 rows off from actual count does not represent a lot of data in some situations.</span></span> <span data-ttu-id="43102-203">Most, ez több millió sort több tábla és összetett lekérdezések, és esetenként a becslés lehet ki több millió sort, és ezért a megfelelő végrehajtási terv fel-vagy a TempDB vezető nincs elég memória biztosít a kért Transzponálás kijutás, és így további i/o sokkal nagyobb.</span><span class="sxs-lookup"><span data-stu-id="43102-203">Now, transpose this to millions of rows across several tables and more complex queries, and at times the estimate can be off by millions of rows , and therefore, the risk of picking-up the wrong execution plan, or requesting insufficient memory grants leading to TempDB spills, and so more I/O, are much higher.</span></span>

<span data-ttu-id="43102-204">Ha a lehetőség, ez az összehasonlítás a leggyakoribb lekérdezések és adatkészletek gyakorlat, és tapasztalja meg, mennyi által a régi és új becslések némelyike érint, amíg csak néhány volt válnak a valóságban ki további vagy néhány másoknak közelebb egyszerűen csak az aktuális sor száma, az eredménykészlet ténylegesen vissza.</span><span class="sxs-lookup"><span data-stu-id="43102-204">If you have the opportunity, practice this comparison with your most typical queries and datasets, and see for yourself by how much some of the old and new estimates are affected, while some could just become more off from the reality, or some others just simply closer to the actual row counts actually returned in the result sets.</span></span> <span data-ttu-id="43102-205">Az összes az alakzat a lekérdezéseket, az Azure SQL adatbázis jellemzőit, típusát és az adatkészleteket és a rájuk vonatkozó statisztikát méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="43102-205">All of it will depend of the shape of your queries, the Azure SQL database characteristics, the nature and the size of your datasets, and the statistics available about them.</span></span> <span data-ttu-id="43102-206">Most létrehozott az Azure SQL Database-példányt, ha a lekérdezésoptimalizáló kell teljesen az előző lekérdezés futtatásakor készült statisztika újbóli felhasználása helyett a Tudásbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="43102-206">If you just created your Azure SQL Database instance, the query optimizer will have to build its knowledge from scratch instead of reusing statistics made of the previous query runs.</span></span> <span data-ttu-id="43102-207">Igen a becslések nagyon környezetfüggő és majdnem adott minden kiszolgáló- és alkalmazástelepítési követelmények állnak.</span><span class="sxs-lookup"><span data-stu-id="43102-207">So, the estimates are very contextual and almost specific to every server and application situation.</span></span> <span data-ttu-id="43102-208">Fontos szem előtt tartani fontos eleme!</span><span class="sxs-lookup"><span data-stu-id="43102-208">It is an important aspect to keep in mind!</span></span>

## <a name="some-considerations-to-take-into-account"></a><span data-ttu-id="43102-209">Néhány szempontot figyelembe kell venni</span><span class="sxs-lookup"><span data-stu-id="43102-209">Some considerations to take into account</span></span>
<span data-ttu-id="43102-210">Bár a legtöbb alkalmazás és szolgáltatás előnyös a kompatibilitási szint 130, mielőtt az éles környezetben a kompatibilitási szintje bevezetése alapvetően 3 lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="43102-210">Although most workloads would benefit from the compatibility level 130, before you adopting the compatibility level for your production environment, you basically have 3 options:</span></span>

1. <span data-ttu-id="43102-211">Kompatibilitási szint 130 áthelyezése, és tekintse meg, hogyan dolgot végre.</span><span class="sxs-lookup"><span data-stu-id="43102-211">You move to compatibility level 130, and see how things perform.</span></span> <span data-ttu-id="43102-212">Ha azt észleli, hogy egyes hátrányait, akkor egyszerűen csak a kompatibilitási szint állítsa vissza az eredeti szintre, vagy hagyja 130 és az örökölt üzemmód csak fordított számossága becslése biztonsági (fentiekben leírtak szerint ez nem sikerült a problémát).</span><span class="sxs-lookup"><span data-stu-id="43102-212">In case you notice some regressions, you just simply set the compatibility level back to its original level, or keep 130, and only reverse the Cardinality Estimation back to the legacy mode (As explained above, this alone could address the issue).</span></span>
2. <span data-ttu-id="43102-213">Alaposan tesztelje a meglévő alkalmazások hasonló üzemi terhelés, finomhangolásra és a teljesítményt, mielőtt a termelési ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="43102-213">You thoroughly test your existing applications under similar production load, fine tune, and validate the performance before going to production.</span></span> <span data-ttu-id="43102-214">Esetlegesen felmerülő problémák ugyanaz, mint a fenti, is mindig lépjen vissza az eredeti kompatibilitási szintjét, vagy egyszerűen megfordíthatja a számosság becslése az örökölt módba.</span><span class="sxs-lookup"><span data-stu-id="43102-214">In case of issues, same as above, you can always go back to the original compatibility level, or simply reverse the Cardinality Estimation back to the legacy mode.</span></span>
3. <span data-ttu-id="43102-215">Az utolsó lehetőség, megoldásának ezeket a kérdéseket a legutóbbi módja a Lekérdezéstár ki.</span><span class="sxs-lookup"><span data-stu-id="43102-215">As a final option, and the most recent way to address these questions, is to leverage the Query Store.</span></span> <span data-ttu-id="43102-216">Ez az ajánlott beállítás az aktuális!</span><span class="sxs-lookup"><span data-stu-id="43102-216">That’s today’s recommended option!</span></span> <span data-ttu-id="43102-217">Segít az elemzést, a lekérdezések a kompatibilitási szint 120 vagy alatt 130, és nem javasoljuk, ahhoz, hogy a Lekérdezéstár használja.</span><span class="sxs-lookup"><span data-stu-id="43102-217">To assist the analysis of your queries under compatibility level 120 or below versus 130, we cannot encourage you enough to use Query Store.</span></span> <span data-ttu-id="43102-218">A Lekérdezéstár érhető el az Azure SQL Database 12-es verziójú legújabb verzióját, és úgy van kialakítva, a lekérdezési teljesítmény hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="43102-218">Query Store is available with the latest version of Azure SQL Database V12, and it’s designed to help you with query performance troubleshooting.</span></span> <span data-ttu-id="43102-219">A Lekérdezéstár gondol, a felhőszolgáltató közötti átviteléhez rögzítő összegyűjtése és minden lekérdezést előzménymodell részletesen bemutatja az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="43102-219">Think of the Query Store as a flight data recorder for your database collecting and presenting detailed historic information about all queries.</span></span> <span data-ttu-id="43102-220">Ez jelentősen egyszerűbb teljesítmény Törvényszéki diagnosztizálhatja és problémák megoldására idő csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="43102-220">This greatly simplifies performance forensics by reducing the time to diagnose and resolve issues.</span></span> <span data-ttu-id="43102-221">További információt a [Lekérdezéstár: az adatbázis egy felé továbbított adatok író](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span><span class="sxs-lookup"><span data-stu-id="43102-221">You can find more information at [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span></span>

<span data-ttu-id="43102-222">A magas szintű, ha már fut, vagy az alatti kompatibilitási szinttel rendelkező 120 adatbázisok készleteit, és tervezze át őket 130, vagy mert a számítási feladatok automatikusan létesítsen új adatbázisokat, hogy hamarosan beállítani 130 alapértelmezés szerint, fontolja meg a a következőket:</span><span class="sxs-lookup"><span data-stu-id="43102-222">At the high-level, if you already have a set of databases running at compatibility level 120 or below, and plan to move some of them to 130, or because your workload automatically provision new databases that will be soon be set by default to 130, please consider the followings:</span></span>

* <span data-ttu-id="43102-223">Mielőtt éles környezetben az új kompatibilitási szint módosítása, engedélyezze a Lekérdezéstárat.</span><span class="sxs-lookup"><span data-stu-id="43102-223">Before changing to the new compatibility level in production, enable Query Store.</span></span> <span data-ttu-id="43102-224">Olvassa el a [módosítsa az adatbázis kompatibilitási módja, és használja a Lekérdezéstár](https://msdn.microsoft.com/library/bb895281.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="43102-224">You can refer to [Change the Database Compatibility Mode and Use the Query Store](https://msdn.microsoft.com/library/bb895281.aspx) for more information.</span></span>
* <span data-ttu-id="43102-225">Ezt követően tesztelje jellemző adatok és a lekérdezések egy hasonló környezetet, és hasonlítsa össze a tapasztalt teljesítmény és a Lekérdezéstár által jelentett összes fontos munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="43102-225">Next, test all critical workloads using representative data and queries of a production-like environment, and compare the performance experienced and as reported by Query Store.</span></span> <span data-ttu-id="43102-226">Egyes hátrányait tapasztal, az a Lekérdezéstár regressed lekérdezések azonosításához, és a terv kényszerített lehetőséget a Lekérdezéstár használata (más néven csomag rögzítéshez).</span><span class="sxs-lookup"><span data-stu-id="43102-226">If you experience some regressions, you can identify the regressed queries with the Query Store and use the plan forcing option from Query Store (aka plan pinning).</span></span> <span data-ttu-id="43102-227">Ebben az esetben akkor véglegesen a kompatibilitási szint 130 marad, és használja a korábbi lekérdezésterv a Lekérdezéstár által javasolt.</span><span class="sxs-lookup"><span data-stu-id="43102-227">In such a case, you definitively stay with the compatibility level 130, and use the former query plan as suggested by the Query Store.</span></span>
* <span data-ttu-id="43102-228">Ha szeretnék kihasználni az új funkciók és képességek az Azure SQL Database (amelyen az SQL Server 2016-os), de a kompatibilitási szint 130, utolsó lehetőségként által indított módosítások érzékeny érdemes lehet a kompatibilitási szint biztonsági szint kényszerítése az ALTER DATABASE utasítás segítségével, amely megfelel a számítási feladatok.</span><span class="sxs-lookup"><span data-stu-id="43102-228">If you want to leverage new features and capabilities of Azure SQL Database (which is running SQL Server 2016), but are sensitive to changes brought by the compatibility level 130, as a last resort, you could consider forcing the compatibility level back to the level that suits your workload by using an ALTER DATABASE statement.</span></span> <span data-ttu-id="43102-229">De először, vegye figyelembe, hogy a Lekérdezéstár terv rögzítése beállítás-e a legjobb lehetőség, mivel nem használ 130 alapvetően marad az SQL Server régebbi verziójú működési szinten.</span><span class="sxs-lookup"><span data-stu-id="43102-229">But first, be aware that the Query Store plan pinning option is your best option because not using 130 is basically staying at the functionality level of an older SQL Server version.</span></span>
* <span data-ttu-id="43102-230">Ha több adatbázis átfedés több-bérlős alkalmazásokhoz, frissítenie kell az adatbázis konzisztens kompatibilitási szintjének biztosítása közötti összes; létesítési logikai lehet régi és az újonnan telepített néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="43102-230">If you have multitenant applications spanning multiple databases, it may be necessary to update the provisioning logic of your databases to ensure a consistent compatibility level across all databases; old and newly provisioned ones.</span></span> <span data-ttu-id="43102-231">Lehet, hogy az alkalmazás munkateljesítményt néhány adatbázis különböző kompatibilitási szinten futnak tény-és nagybetűket, ezért bármely adatbázis kompatibilitási szintjét egységességének lehet szükség ahhoz, hogy ugyanazt a felhasználói élményt biztosít az ügyfelek számára a tábla összes között.</span><span class="sxs-lookup"><span data-stu-id="43102-231">Your application workload performance could be sensitive to the fact that some databases are running at different compatibility levels, and therefore, compatibility level consistency across any database could be required in order to provide the same experience to your customers all across the board.</span></span> <span data-ttu-id="43102-232">Vegye figyelembe, hogy nincs megbízás, valóban attól függ, milyen hatással van az alkalmazás által a kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="43102-232">Note that it is not a mandate, it really depends on how your application is affected by the compatibility level.</span></span>
* <span data-ttu-id="43102-233">Utolsó, számossága becslése kapcsolatban, és hasonlóan, éles, a folytatás előtt a kompatibilitási szint módosítása javasoljuk, hogy a termelési számítási feladatok határozza meg, ha az alkalmazás nyújtotta új feltételek tesztelése a Számossága becslés fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="43102-233">Last, regarding the Cardinality Estimation, and just like changing the compatibility level, before proceeding in production, it is recommended to test your production workload under the new conditions to determine if your application benefits from the Cardinality Estimation improvements.</span></span>

## <a name="conclusion"></a><span data-ttu-id="43102-234">Összegzés</span><span class="sxs-lookup"><span data-stu-id="43102-234">Conclusion</span></span>
<span data-ttu-id="43102-235">Azure SQL Database használata az összes SQL Server 2016 fejlesztések kihasználják a lekérdezés végrehajtások egyértelműen javítja.</span><span class="sxs-lookup"><span data-stu-id="43102-235">Using Azure SQL Database to benefit from all SQL Server 2016 enhancements can clearly improve your query executions.</span></span> <span data-ttu-id="43102-236">Ahogy-van!</span><span class="sxs-lookup"><span data-stu-id="43102-236">Just as-is!</span></span> <span data-ttu-id="43102-237">Természetesen mint bármely új szolgáltatás a megfelelő próbaverzióra kell elvégezni a pontos feltételek, amelyek alapján az adatbázis munkaterhelés működik, a legjobb meghatározására.</span><span class="sxs-lookup"><span data-stu-id="43102-237">Of course, like any new feature, a proper evaluation must be done to determine the exact conditions under which your database workload operates the best.</span></span> <span data-ttu-id="43102-238">A tapasztalat, hogy a legtöbb munkaterhelés várhatóan legalább futni transzparens módon kompatibilitási szint 130, miközben feldolgozási funkciók és új számossága becslés új lekérdezés használhatja.</span><span class="sxs-lookup"><span data-stu-id="43102-238">Experience shows that most workload are expected to at least run transparently under compatibility level 130, while leveraging new query processing functions, and new Cardinality Estimation.</span></span> <span data-ttu-id="43102-239">Amely említett reálisan nézve mindig van néhány kivétel, valamint arról, hogy megfelelő megfelelő gondossággal annak meghatározásához, hogy milyen mértékben kihasználhassa a bővítések fontos értékelését.</span><span class="sxs-lookup"><span data-stu-id="43102-239">That said, realistically, there are always some exceptions and doing proper due diligence is an important assessment to determine how much you can benefit from these enhancements.</span></span> <span data-ttu-id="43102-240">És ebben az esetben a Lekérdezéstár a munka során a nagy Súgó!</span><span class="sxs-lookup"><span data-stu-id="43102-240">And again, the Query Store can be of a great help in doing this work!</span></span>

<span data-ttu-id="43102-241">Az SQL Azure fejlődésének a jövőben számíthat 140 kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="43102-241">As SQL Azure evolves, you can expect a compatibility level 140 in the future.</span></span> <span data-ttu-id="43102-242">Megfelelő idő esetén először fog van szó Mi a későbbi kompatibilitási szint 140 be fogja hozni, ugyanúgy, mint a röviden ismertettük itt milyen kompatibilitási szint 130 ma mihamarabb elérhetővé tenni.</span><span class="sxs-lookup"><span data-stu-id="43102-242">When time is appropriate, we will start talking about what this future compatibility level 140 will bring, just as we briefly discussed here what compatibility level 130 is bringing today.</span></span>

<span data-ttu-id="43102-243">Most tegyük feledkezzen, 2016. június-től kezdődően az Azure SQL Database módosul az alapértelmezett kompatibilitási szintje 120 130 az újonnan létrehozott adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="43102-243">For now, let’s not forget, starting June 2016, Azure SQL Database will change the default compatibility level from 120 to 130 for newly created databases.</span></span> <span data-ttu-id="43102-244">Ne feledje!</span><span class="sxs-lookup"><span data-stu-id="43102-244">Be aware!</span></span>

## <a name="references"></a><span data-ttu-id="43102-245">Referencia</span><span class="sxs-lookup"><span data-stu-id="43102-245">References</span></span>
* [<span data-ttu-id="43102-246">Adatbázis-kezelő újdonságai</span><span class="sxs-lookup"><span data-stu-id="43102-246">What’s New in Database Engine</span></span>](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [<span data-ttu-id="43102-247">Blog: A Lekérdezéstár: egy felé továbbított adatok író Borko Novakovic, 8 2016. június az adatbázis számára</span><span class="sxs-lookup"><span data-stu-id="43102-247">Blog: Query Store: A flight data recorder for your database, by Borko Novakovic, June 8 2016</span></span>](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [<span data-ttu-id="43102-248">ALTER adatbázis kompatibilitási szintje (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="43102-248">ALTER DATABASE Compatibility Level (Transact-SQL)</span></span>](https://msdn.microsoft.com/library/bb510680.aspx)
* [<span data-ttu-id="43102-249">ALTER DATABASE HATÓKÖRŰ KONFIGURÁCIÓ</span><span class="sxs-lookup"><span data-stu-id="43102-249">ALTER DATABASE SCOPED CONFIGURATION</span></span>](https://msdn.microsoft.com/library/mt629158.aspx)
* [<span data-ttu-id="43102-250">Az Azure SQL Database 12-es kompatibilitási szintje 130</span><span class="sxs-lookup"><span data-stu-id="43102-250">Compatibility Level 130 for Azure SQL Database V12</span></span>](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [<span data-ttu-id="43102-251">A lekérdezés optimalizálása-csomagok és az SQL Server 2014 Számosságbecslő</span><span class="sxs-lookup"><span data-stu-id="43102-251">Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator</span></span>](https://msdn.microsoft.com/library/dn673537.aspx)
* [<span data-ttu-id="43102-252">Oszlopcentrikus indexek útmutató</span><span class="sxs-lookup"><span data-stu-id="43102-252">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
* [<span data-ttu-id="43102-253">Blog: Jobb teljesítmény-küszöbérték-es kompatibilitási szintű 130 az Azure SQL-adatbázis által Alain Lissoir 6 2016. május</span><span class="sxs-lookup"><span data-stu-id="43102-253">Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database, by Alain Lissoir, May 6 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
