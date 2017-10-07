---
title: "aaaDatabase kompatibilitási szint 130 - Azure SQL Database |} Microsoft Docs"
description: "Ez a cikk azt megismerkedhet az Azure SQL Database 130 kompatibilitási szinten fut, és új lekérdezésoptimalizáló hello hello előnyeit kihasználva hello előnyei és lekérdezni a processzor funkcióit. Azt is eleget hello lehetséges mellékhatásokkal a lekérdezési teljesítmény hello hello meglévő SQL-alkalmazásokhoz."
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
ms.openlocfilehash: 25693c5f7b01405b7073fa7d4cc2833fbe125e2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a><span data-ttu-id="a0cda-104">Javult a lekérdezési teljesítmény kompatibilitási szint 130 az Azure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="a0cda-104">Improved query performance with compatibility Level 130 in Azure SQL Database</span></span>
<span data-ttu-id="a0cda-105">Az Azure SQL Database fut transzparens módon akár több ezer adatbázis több száz számos különböző kompatibilitási szinten megőrzi az és hello előző verziókkal való kompatibilitás toohello megfelelő verzióját a Microsoft SQL Server biztosítva az ügyfelek a!</span><span class="sxs-lookup"><span data-stu-id="a0cda-105">Azure SQL Database is running transparently hundreds of thousands of databases at many different compatibility levels, preserving and guaranteeing hello backward compatibility toohello corresponding version of Microsoft SQL Server for all its customers!</span></span>

<span data-ttu-id="a0cda-106">Ez a cikk azt megismerkedhet az Azure SQL adatbázismodell 130 kompatibilitási szinten fut, és új lekérdezésoptimalizáló hello hello előnyeit kihasználva hello előnyei és lekérdezni a processzor funkcióit.</span><span class="sxs-lookup"><span data-stu-id="a0cda-106">In this article, we explore hello benefits of running your Azure SQL Databse at compatibility level 130, and leveraging hello benefits of hello new query optimizer and query processor features.</span></span> <span data-ttu-id="a0cda-107">Azt is eleget hello lehetséges mellékhatásokkal a lekérdezési teljesítmény hello hello meglévő SQL-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a0cda-107">We also address hello possible side-effects on hello query performance for hello existing SQL applications.</span></span>

<span data-ttu-id="a0cda-108">Ne feledje az előzményeket SQL verziók toodefault kompatibilitási szintnek hello igazítását a következők:</span><span class="sxs-lookup"><span data-stu-id="a0cda-108">As a reminder of history, hello alignment of SQL versions toodefault compatibility levels are as follows:</span></span>

* <span data-ttu-id="a0cda-109">100: az SQL Server 2008 és az Azure SQL adatbázis-V11.</span><span class="sxs-lookup"><span data-stu-id="a0cda-109">100: in SQL Server 2008 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="a0cda-110">110: az SQL Server 2012 és az Azure SQL adatbázis-V11.</span><span class="sxs-lookup"><span data-stu-id="a0cda-110">110: in SQL Server 2012 and Azure SQL Database V11.</span></span>
* <span data-ttu-id="a0cda-111">120: az SQL Server 2014 és az Azure SQL-adatbázis 12-es verzió.</span><span class="sxs-lookup"><span data-stu-id="a0cda-111">120: in SQL Server 2014 and Azure SQL Database V12.</span></span>
* <span data-ttu-id="a0cda-112">130: az SQL Server 2016-os és az Azure SQL-adatbázis 12-es verzió.</span><span class="sxs-lookup"><span data-stu-id="a0cda-112">130: in SQL Server 2016 and Azure SQL Database V12.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0cda-113">Kezdve **2016. június mid**, az Azure SQL Database, a hello alapértelmezett kompatibilitási szintje lesz helyett 120 130 **az újonnan létrehozott** adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="a0cda-113">Starting in **mid-June 2016**, in Azure SQL Database, hello default compatibility level will be 130 instead of 120 for **newly created** databases.</span></span>
> 
> <span data-ttu-id="a0cda-114">2016. június mid előtt létrehozott adatbázisokat fogja *nem* érinti, és a jelenlegi kompatibilitási szint (100, 110-esre vagy 120) rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="a0cda-114">Databases created before mid-June 2016 will *not* be affected, and will maintain their current compatibility level (100, 110, or 120).</span></span> <span data-ttu-id="a0cda-115">Azure SQL Database V11 tooV12 verziójából áttelepített adatbázisok fog rendelkezni 100 vagy a 110-es kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="a0cda-115">Databases that migrated from Azure SQL Database version V11 tooV12 will have a compatibility level of either 100 or 110.</span></span> 
> 

## <a name="about-compatibility-level-130"></a><span data-ttu-id="a0cda-116">130 kompatibilitási szintre vonatkozó</span><span class="sxs-lookup"><span data-stu-id="a0cda-116">About compatibility level 130</span></span>
<span data-ttu-id="a0cda-117">Először Ha azt szeretné, hogy tooknow hello jelenlegi kompatibilitási szintjét az adatbázis, a következő Transact-SQL-utasítás hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="a0cda-117">First, if you want tooknow hello current compatibility level of your database, execute hello following Transact-SQL statement.</span></span>

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


<span data-ttu-id="a0cda-118">Ezen változtatás előtt toolevel 130 történik, a **újonnan** hozott létre adatbázisok, most áttekintheti az ezt a módosítást minden szól bizonyos nagyon alapszintű lekérdezés példákból, és tekintse meg, hogyan bárki is kihasználhatja le azt.</span><span class="sxs-lookup"><span data-stu-id="a0cda-118">Before this change toolevel 130 happens for **newly** created databases, let’s review what this change is all about through some very basic query examples, and see how anyone can benefit from it.</span></span>

<span data-ttu-id="a0cda-119">A lekérdezés feldolgozása a relációs adatbázisok nagyon összetett feladat lehet, és toolots számítógép tudományos és matematikai toounderstand hello rejlő tervezési döntések ütköznek azokkal és viselkedéshez vezethet.</span><span class="sxs-lookup"><span data-stu-id="a0cda-119">Query processing in relational databases can be very complex and can lead toolots of computer science and mathematics toounderstand hello inherent design choices and behaviors.</span></span> <span data-ttu-id="a0cda-120">Ebben a dokumentumban hello tartalom lett szándékosan egyszerűsített tooensure, hogy bárki, aki néhány minimális technikai háttér hello kompatibilitási szint módosítása hello hatásának megismerése, és határozza meg az alkalmazások előnyeiről.</span><span class="sxs-lookup"><span data-stu-id="a0cda-120">In this document, hello content has been intentionally simplified tooensure that anyone with some minimum technical background can understand hello impact of hello compatibility level change and determine how it can benefit applications.</span></span>

<span data-ttu-id="a0cda-121">Most rendelkeznie milyen hello kompatibilitási szint 130 számos lehetőséget kínál, hello táblát gyorsan át.</span><span class="sxs-lookup"><span data-stu-id="a0cda-121">Let’s have a quick look at what hello compatibility level 130 brings at hello table.</span></span>  <span data-ttu-id="a0cda-122">További részletei: [ALTER adatbázis kompatibilitási szintje (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), de rövid összegzése:</span><span class="sxs-lookup"><span data-stu-id="a0cda-122">You can find more details at [ALTER DATABASE Compatibility Level (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), but here is a short summary:</span></span>

* <span data-ttu-id="a0cda-123">hello Insert választás utasítás beszúrási művelet lehet többszálas vagy, hogy a párhuzamos terv, mielőtt ez a művelet lett egyszálas során.</span><span class="sxs-lookup"><span data-stu-id="a0cda-123">hello Insert operation of an Insert-select statement can be multi-threaded or can have a parallel plan, while before this operation was single-threaded.</span></span>
* <span data-ttu-id="a0cda-124">Memória optimalizált tábla és a tábla változók lekérdezések most lehet párhuzamos terveket, mielőtt ez a művelet lett is egyszálas során.</span><span class="sxs-lookup"><span data-stu-id="a0cda-124">Memory Optimized table and table variables queries can now have parallel plans, while before this operation was also single-threaded .</span></span>
* <span data-ttu-id="a0cda-125">A Memóriaoptimalizált tábla statisztikája most mintát vehetnek és automatikus frissítése.</span><span class="sxs-lookup"><span data-stu-id="a0cda-125">Statistics for Memory Optimized table can now be sampled and are auto-updated.</span></span> <span data-ttu-id="a0cda-126">Lásd: [az adatbázis-kezelő újdonságai: memórián belüli online Tranzakciófeldolgozási](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="a0cda-126">See [What's New in Database Engine: In-Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) for more details.</span></span>
* <span data-ttu-id="a0cda-127">Az oszlop az Oszlopcentrikus indexek módosítja sor módú kötegelt módban v/s</span><span class="sxs-lookup"><span data-stu-id="a0cda-127">Batch mode v/s Row Mode changes with Column Store indexes</span></span>
  * <span data-ttu-id="a0cda-128">Egy oszlop Oszlopcentrikus indexszel rendelkező táblán rendezi most kötegelt módban van.</span><span class="sxs-lookup"><span data-stu-id="a0cda-128">Sorts on a table with a Column Store index are now in batch mode.</span></span>
  * <span data-ttu-id="a0cda-129">Leképezési összesítések kötegelt módban, például a TSQL LAG/ÁTFUTÁSI utasítások működni.</span><span class="sxs-lookup"><span data-stu-id="a0cda-129">Windowing aggregates now operate in batch mode such as TSQL LAG/LEAD statements.</span></span>
  * <span data-ttu-id="a0cda-130">A distinct záradékban több oszlop tárolási táblákon lekérdezések kötegelt módban működik.</span><span class="sxs-lookup"><span data-stu-id="a0cda-130">Queries on Column Store tables with Multiple distinct clauses operate in Batch mode.</span></span>
  * <span data-ttu-id="a0cda-131">DOP mellett futó lekérdezésekhez = 1 vagy soros csomagot is hajtsa végre a kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="a0cda-131">Queries running under DOP=1 or with a serial plan also execute in Batch Mode.</span></span>
* <span data-ttu-id="a0cda-132">Utolsó, számossága becslés fejlesztései ténylegesen érkeznek-es kompatibilitási szintű 120, de azok az Ön alacsonyabb kompatibilitási szintű fut (azaz 100 vagy 110), hello áthelyezés toocompatibility szint 130 fog is visszaállítja ezek a fejlesztések, és ezeket is az alkalmazások teljesítményének hello lekérdezés előnyeit.</span><span class="sxs-lookup"><span data-stu-id="a0cda-132">Last, Cardinality Estimation improvements are actually coming with compatibility level 120, but for those of you running at a lower Compatibility level (i.e. 100, or 110), hello move toocompatibility level 130 will also bring these improvements, and these can also benefit hello query performance of your applications.</span></span>

## <a name="practicing-compatibility-level-130"></a><span data-ttu-id="a0cda-133">Kompatibilitási szint 130 gyakorlás</span><span class="sxs-lookup"><span data-stu-id="a0cda-133">Practicing compatibility level 130</span></span>
<span data-ttu-id="a0cda-134">Első folytassuk egyes táblák, indexek és létrehozott véletlenszerű adatokat toopractice néhány az új lehetőségekhez.</span><span class="sxs-lookup"><span data-stu-id="a0cda-134">First let’s get some tables, indexes and random data created toopractice some of these new capabilities.</span></span> <span data-ttu-id="a0cda-135">SQL Server 2016-os, illetve az Azure SQL Database hello TSQL parancsfájl példák hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="a0cda-135">hello TSQL script examples can be executed under SQL Server 2016, or under Azure SQL Database.</span></span> <span data-ttu-id="a0cda-136">Azonban ha egy Azure SQL-adatbázis létrehozása győződjön meg arról, hogy adjon meg egy P2 adatbázist, mert a szükséges minimális hello többszálú magok tooallow legalább néhány, és ezért kihasználhatja le ezeket a szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="a0cda-136">However, when creating an Azure SQL database, make sure you choose at hello minimum a P2 database because you need at least a couple of cores tooallow multi-threading and therefore benefit from these features.</span></span>

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- hello second one (only available on Premium databases)

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


<span data-ttu-id="a0cda-137">Most tegyük rendelkezik egy hely toosome hello lekérdezés feldolgozása funkciók hamarosan-es kompatibilitási szintű 130.</span><span class="sxs-lookup"><span data-stu-id="a0cda-137">Now, let’s have a look toosome of hello Query Processing features coming with compatibility level 130.</span></span>

## <a name="parallel-insert"></a><span data-ttu-id="a0cda-138">Párhuzamos BESZÚRÁSA</span><span class="sxs-lookup"><span data-stu-id="a0cda-138">Parallel INSERT</span></span>
<span data-ttu-id="a0cda-139">Hello kompatibilitási szintje 120 és 130, amely egyetlen összefűzött modellben (120), és a többszálas modellben (130) rendre végrehajtja a beszúrási művelet hello alapján végrehajtott beszúrási művelet végrehajtása hello TSQL utasításokat az alábbi hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="a0cda-139">Executing hello TSQL statements below executes hello INSERT operation under compatibility level 120 and 130, which respectively executes hello INSERT operation in a single threaded model (120), and in a multi-threaded model (130).</span></span>

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- hello INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- hello INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


<span data-ttu-id="a0cda-140">Hello tényleges hello lekérdezésterv, a grafikus ábrázolása és az XML-tartalom kérésével mely számossága becslés függvény jelenleg play azt is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="a0cda-140">By requesting hello actual hello query plan, looking at its graphical representation or its XML content, you can determine which Cardinality Estimation function is at play.</span></span> <span data-ttu-id="a0cda-141">Az 1. ábra megnézi hello csomagok-mellé, egyértelműen láthatja, hogy hello oszlop tároló BESZÚRÁSA végrehajtási kerül a soros a 120 tooparallel 130.</span><span class="sxs-lookup"><span data-stu-id="a0cda-141">Looking at hello plans side-by-side on figure 1, we can clearly see that hello Column Store INSERT execution goes from serial in 120 tooparallel in 130.</span></span> <span data-ttu-id="a0cda-142">Megjegyzés: Ez hello a módosítás hello iterátor ikon hello 130 terv, amely mutatja, két párhuzamos nyilat ábrázoló hello tényt, hogy most hello iterátor végrehajtási valóban párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="a0cda-142">Also, note that hello change of hello iterator icon in hello 130 plan showing two parallel arrows, illustrating hello fact that now hello iterator execution is indeed parallel.</span></span> <span data-ttu-id="a0cda-143">Ha nagy beszúrási műveletek toocomplete, hello párhuzamos futtatáshoz, Ön rendelkezésére a hello adatbázis core csatolt toohello száma javítja a teljesítményt; 100 alkalommal gyorsabb a helyzettől függően tooa mentése!</span><span class="sxs-lookup"><span data-stu-id="a0cda-143">If you have large INSERT operations toocomplete, hello parallel execution, linked toohello number of core you have at your disposal for hello database, will perform better; up tooa 100 times faster depending your situation!</span></span>

<span data-ttu-id="a0cda-144">*1. ábra: BESZÚRÁSA soros tooparallel-es kompatibilitási szintű 130 művelet módosításokat.*</span><span class="sxs-lookup"><span data-stu-id="a0cda-144">*Figure 1: INSERT operation changes from serial tooparallel with compatibility level 130.*</span></span>

![1. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a><span data-ttu-id="a0cda-146">SOROS kötegelt módban</span><span class="sxs-lookup"><span data-stu-id="a0cda-146">SERIAL Batch Mode</span></span>
<span data-ttu-id="a0cda-147">Hasonlóképpen a toocompatibility szint 130 áthelyezése sornyi adatot feldolgozásakor engedélyezi kötegfeldolgozási mód.</span><span class="sxs-lookup"><span data-stu-id="a0cda-147">Similarly, moving toocompatibility level 130 when processing rows of data enables batch mode processing.</span></span> <span data-ttu-id="a0cda-148">Először kötegelt módban futó műveletekhez esetén csak érhetők el egy oszlopcentrikus indexszel érvényben van.</span><span class="sxs-lookup"><span data-stu-id="a0cda-148">First, batch mode operations  are only available when you have a column store index in place.</span></span> <span data-ttu-id="a0cda-149">Ezután egy kötegelt általában ~ 900 sorok jelöli, és használja a kód logika multicore Processzor, memória nagyobb átviteli optimalizálva és közvetlenül használja hello tömörített adatai hello oszlop tároló, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a0cda-149">Second, a batch typically represents ~900 rows, and uses a code logic optimized for multicore CPU, higher memory throughput and directly leverages hello compressed data of hello Column Store whenever possible.</span></span> <span data-ttu-id="a0cda-150">Ebben az SQL Server 2016 tud feldolgozni ~ 900 sorok egyszerre, 1 sor hello időpontban helyett, valamint következtében hello hello művelet teljes általános költség most megosztott hello teljes kötegelt hello általános soronként költségeket csökkenti.</span><span class="sxs-lookup"><span data-stu-id="a0cda-150">Under these conditions, SQL Server 2016 can process ~900 rows at once, instead of 1 row at hello time, and as a consequence, hello overall overhead cost of hello operation is now shared by hello entire batch, reducing hello overall cost by row.</span></span> <span data-ttu-id="a0cda-151">Alapvetően együtt hello oszlop tároló tömörítési műveletek megosztott ennyi csökkenti a hello késést részt vesz egy SELECT kötegelt módban működik.</span><span class="sxs-lookup"><span data-stu-id="a0cda-151">This shared amount of operations combined with hello column store compression basically reduces hello latency involved in a SELECT batch mode operation.</span></span> <span data-ttu-id="a0cda-152">Hello oszlop-áruházzal kapcsolatos további részletekért található, és kötegelt módban, [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0cda-152">You can find more details about hello column store and batch mode at [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="a0cda-153">Alább látható, mint betartásával hello lekérdezés tervek-mellé a 2. ábra azt mutatják, hogy hello feldolgozási mód hello kompatibilitási szinttel rendelkező megváltozott, és következtében végrehajtásakor hello lekérdezések mindkét kompatibilitási szint regisztrálását, láthatja, hogy a legtöbb hello feldolgozási időt töltött sor mód (86 %) képest toohello kötegelt módban (14 %), ahol 2 kötegek feldolgozott.</span><span class="sxs-lookup"><span data-stu-id="a0cda-153">As visible below, by observing hello query plans side-by-side on figure 2, we can see that hello processing mode has changed with hello compatibility level, and as a consequence, when executing hello queries in both compatibility level altogether, we can see that most of hello processing time is spent in row mode (86%) compared toohello batch mode (14%), where 2 batches have been processed.</span></span> <span data-ttu-id="a0cda-154">Hello dataset növeléséhez hello juttatás növeli.</span><span class="sxs-lookup"><span data-stu-id="a0cda-154">Increase hello dataset, hello benefit will increase.</span></span>

<span data-ttu-id="a0cda-155">*2. ábra: Művelet módosítások-es kompatibilitási szintű 130 soros toobatch mód kiválasztása*</span><span class="sxs-lookup"><span data-stu-id="a0cda-155">*Figure 2: SELECT operation changes from serial toobatch mode with compatibility level 130.*</span></span>

![2. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a><span data-ttu-id="a0cda-157">A rendezési végrehajtási kötegelt módban</span><span class="sxs-lookup"><span data-stu-id="a0cda-157">Batch mode on Sort Execution</span></span>
<span data-ttu-id="a0cda-158">A fenti hasonló toohello, de a rendezési művelet alkalmazott tooa, hello való áttérés sor mód (kompatibilitási szint 120) toobatch mód (kompatibilitási szint 130) javítja hello hello a rendezési művelet hello teljesítményét okok miatt.</span><span class="sxs-lookup"><span data-stu-id="a0cda-158">Similar toohello above, but applied tooa sort operation, hello transition from row mode (compatibility level 120) toobatch mode (compatibility level 130) improves hello performance of hello SORT operation for hello same reasons.</span></span>

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


<span data-ttu-id="a0cda-159">Látható-mellé a 3. ábra, láthatja, hogy a hello rendezési művelet sor módú 81-es %-a költség, amíg hello kötegelt módban csak jelöli 19 % hello költség (illetve 81-56 % hello rendezési magát a) hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="a0cda-159">Visible side-by-side on figure 3, we can see that hello sort operation in row mode represents 81% of hello cost, while hello batch mode only represents 19% of hello cost (respectively 81% and 56% on hello sort itself).</span></span>

<span data-ttu-id="a0cda-160">*3. ábra: A sor toobatch módú-es kompatibilitási szintű 130 változik rendezési művelet.*</span><span class="sxs-lookup"><span data-stu-id="a0cda-160">*Figure 3: SORT operation changes from row toobatch mode with compatibility level 130.*</span></span>

![3. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

<span data-ttu-id="a0cda-162">Természetesen ezeket a mintákat csak tartalmazhat tízezreit sorok, vagyis semmi kezelőkonzoljából nézve legtöbb SQL-kiszolgáló elérhető hello adatok ezek a napok.</span><span class="sxs-lookup"><span data-stu-id="a0cda-162">Obviously, these samples only contain tens of thousands of rows, which is nothing when looking at hello data available in most SQL Servers these days.</span></span> <span data-ttu-id="a0cda-163">Csak ezek alapján több millió sort helyette projektre, és ez kímélni minden nap függőben lévő hello jellegét a számítási feladatok végrehajtása több percig is fordítása.</span><span class="sxs-lookup"><span data-stu-id="a0cda-163">Just project these against millions of rows instead, and this can translate in several minutes of execution spared every day pending hello nature of your workload.</span></span>

## <a name="cardinality-estimation-ce-improvements"></a><span data-ttu-id="a0cda-164">Számossága becslése (CE) fejlesztései</span><span class="sxs-lookup"><span data-stu-id="a0cda-164">Cardinality Estimation (CE) improvements</span></span>
<span data-ttu-id="a0cda-165">Az SQL Server 2014-ban bevezetett, bármilyen adatbázis-kompatibilitási szinten 120 vagy újabb rendszeren futó megkönnyítő hello új számossága becslés funkciók használatára.</span><span class="sxs-lookup"><span data-stu-id="a0cda-165">Introduced with SQL Server 2014, any database running at a compatibility level 120 or above will make use of hello new Cardinality Estimation functionality.</span></span> <span data-ttu-id="a0cda-166">Alapvetően számossága becslés hello logika toodetermine SQL server hogyan hajtható végre a lekérdezés a becsült költség alapul.</span><span class="sxs-lookup"><span data-stu-id="a0cda-166">Essentially, cardinality estimation is hello logic used toodetermine how SQL server will execute a query based on its estimated cost.</span></span> <span data-ttu-id="a0cda-167">hello becslés lekérdezés érintett objektumok statisztika bemenetének használatával történik.</span><span class="sxs-lookup"><span data-stu-id="a0cda-167">hello estimation is calculated using input from statistics associated with objects involved in that query.</span></span> <span data-ttu-id="a0cda-168">Gyakorlatilag, egy magas szintű számossága becslés funkciók hozzávetőlegesek sor száma információ hello terjesztési hello értékek eltérő érték számát, valamint és ismétlődő számok lévő táblák és hello lekérdezésben hivatkozott objektumok hello.</span><span class="sxs-lookup"><span data-stu-id="a0cda-168">Practically, at a high-level, Cardinality Estimation functions are row count estimates along with information about hello distribution of hello values, distinct value counts, and duplicate counts contained in hello tables and objects referenced in hello query.</span></span> <span data-ttu-id="a0cda-169">Első hibás, ezek a becslések vezethet toounnecessary lemez i/o tooinsufficient memória biztosít (azaz a TempDB kijutás) miatt, vagy tooa kiválasztását párhuzamos keresztül egy soros terv végrehajtási terv végrehajtási, tooname néhány.</span><span class="sxs-lookup"><span data-stu-id="a0cda-169">Getting these estimates wrong, can lead toounnecessary disk I/O due tooinsufficient memory grants (i.e. TempDB spills), or tooa selection of a serial plan execution over a parallel plan execution, tooname a few.</span></span> <span data-ttu-id="a0cda-170">Megkötését, helytelen becslése vezethet tooan hello lekérdezés-végrehajtás általános teljesítménycsökkenést.</span><span class="sxs-lookup"><span data-stu-id="a0cda-170">Conclusion, incorrect estimates can lead tooan overall performance degradation of hello query execution.</span></span> <span data-ttu-id="a0cda-171">Hello a másik oldalon pontosabb becslést, pontosabb becslést, érdeklődők toobetter lekérdezés végrehajtások!</span><span class="sxs-lookup"><span data-stu-id="a0cda-171">On hello other side, better estimates, more accurate estimates, leads toobetter query executions!</span></span>

<span data-ttu-id="a0cda-172">Ahogy korábban említettük, lekérdezés optimalizálása és becslése egy összetett függetlenül attól, hogy, de ha azt szeretné, hogy további terveket és számosságbecslő toolearn, olvassa el a dokumentumot toohello [optimalizálása a lekérdezés terveket hello SQL Server 2014 Számosságbecslő](https://msdn.microsoft.com/library/dn673537.aspx) mélyebb bemutatója a.</span><span class="sxs-lookup"><span data-stu-id="a0cda-172">As mentioned before, query optimizations and estimates are a complex matter, but if you want toolearn more about query plans and cardinality estimator, you can refer toohello document at [Optimizing Your Query Plans with hello SQL Server 2014 Cardinality Estimator](https://msdn.microsoft.com/library/dn673537.aspx) for a deeper dive.</span></span>

## <a name="which-cardinality-estimation-do-you-currently-use"></a><span data-ttu-id="a0cda-173">Mely számossága becslés jelenleg használ?</span><span class="sxs-lookup"><span data-stu-id="a0cda-173">Which Cardinality Estimation do you currently use?</span></span>
<span data-ttu-id="a0cda-174">mely számossága becslése a lekérdezéseket futtat, a most csak használata hello lekérdezés minták alábbi toodetermine.</span><span class="sxs-lookup"><span data-stu-id="a0cda-174">toodetermine under which Cardinality Estimation your queries are running, let’s just use hello query samples below.</span></span> <span data-ttu-id="a0cda-175">Vegye figyelembe, hogy az első példában kompatibilitási szintjét 110-esre, hello hello régi számossága becslés funkciók használatát úgy fognak futni.</span><span class="sxs-lookup"><span data-stu-id="a0cda-175">Note that this first example will run under compatibility level 110, implying hello use of hello old Cardinality Estimation functions.</span></span>

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


<span data-ttu-id="a0cda-176">Ha végrehajtása befejeződött, kattintson a hello XML-hivatkozás, és hello első iterátor hello tulajdonságainak tekintse meg alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="a0cda-176">Once execution is complete, click on hello XML link, and look at hello properties of hello first iterator as shown below.</span></span> <span data-ttu-id="a0cda-177">Megjegyzés: a beállított 70 CardinalityEstimationModelVersion nevű hello tulajdonság neve.</span><span class="sxs-lookup"><span data-stu-id="a0cda-177">Note hello property name called CardinalityEstimationModelVersion currently set on 70.</span></span> <span data-ttu-id="a0cda-178">Azt nem jelenti azt, hogy hello adatbázis kompatibilitási szintje toohello SQL Server 7.0 verzió (van állítva, akkor a 110-re, mint látható a fenti hello TSQL-utasításokban), de hello érték 70 egyszerűen annak hello örökölt számossága becslés funkció érhető el SQL óta Server 7.0, amelyet nem fő verziók addig az SQL Server 2014 (amely 120 kompatibilitási szintje).</span><span class="sxs-lookup"><span data-stu-id="a0cda-178">It does not mean that hello database compatibility level is set toohello SQL Server 7.0 version (it is set on 110 as visible in hello TSQL statements above), but hello value 70 simply represents hello legacy Cardinality Estimation functionality available since SQL Server 7.0, which had no major revisions until SQL Server 2014 (which comes with a compatibility level of 120).</span></span>

<span data-ttu-id="a0cda-179">*4. ábra: hello CardinalityEstimationModelVersion értéke too70 110-esre vagy alatti kompatibilitási szintje használatakor.*</span><span class="sxs-lookup"><span data-stu-id="a0cda-179">*Figure 4: hello CardinalityEstimationModelVersion is set too70 when using a compatibility level of 110 or below.*</span></span>

![4. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

<span data-ttu-id="a0cda-181">Azt is megteheti, hello kompatibilitási szint too130 módosítsa, majd tiltsa le a hello új számossága becslés függvény hello használata LEGACY_CARDINALITY_ESTIMATION beállítása a tooON hello segítségével [ALTER DATABASE HATÓKÖRŰ CONFIGURATION utasítás](https://msdn.microsoft.com/library/mt629158.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0cda-181">Alternatively, you can change hello compatibility level too130, and disable hello use of hello new Cardinality Estimation function by using hello LEGACY_CARDINALITY_ESTIMATION set tooON with [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).</span></span> <span data-ttu-id="a0cda-182">Ez fogja kell pontosan hello ugyanaz, mint a Cardinality becslés függvény szempontjából, a 110 használatával hello legújabb lekérdezés feldolgozása a kompatibilitási szint használata során.</span><span class="sxs-lookup"><span data-stu-id="a0cda-182">This will be exactly hello same as using 110 from a Cardinality Estimation function point of view, while using hello latest query processing compatibility level.</span></span> <span data-ttu-id="a0cda-183">Így igénybe vehesse az hello új lekérdezés feldolgozása a funkciók hamarosan hello legfrissebb kompatibilitási szintű (azaz kötegelt módban), de továbbra is támaszkodjon hello régi számossága becslés funkció szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="a0cda-183">Doing so, you can benefit from hello new query processing features coming with hello latest compatibility level (i.e. batch mode), but still rely on hello old Cardinality Estimation functionality if necessary.</span></span>

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


<span data-ttu-id="a0cda-184">Egyszerűen áthelyezése toohello kompatibilitási szint 120 vagy 130 lehetővé teszi, hogy a hello új számossága becslés funkciót.</span><span class="sxs-lookup"><span data-stu-id="a0cda-184">Simply moving toohello compatibility level 120 or 130 enables hello new Cardinality Estimation functionality.</span></span> <span data-ttu-id="a0cda-185">Ebben az esetben hello alapértelmezett CardinalityEstimationModelVersion lesz beállítva, ennek megfelelően too120 vagy 130 látható, az alábbi.</span><span class="sxs-lookup"><span data-stu-id="a0cda-185">In such a case, hello default CardinalityEstimationModelVersion will be set accordingly too120 or 130 as visible below.</span></span>

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


<span data-ttu-id="a0cda-186">*5. ábra: hello CardinalityEstimationModelVersion értéke too130 130 kompatibilitási szintje használatakor.*</span><span class="sxs-lookup"><span data-stu-id="a0cda-186">*Figure 5: hello CardinalityEstimationModelVersion is set too130 when using a compatibility level of 130.*</span></span>

![5. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-hello-cardinality-estimation-differences"></a><span data-ttu-id="a0cda-188">Aláírnia hello számossága becslés különbségek</span><span class="sxs-lookup"><span data-stu-id="a0cda-188">Witnessing hello Cardinality Estimation differences</span></span>
<span data-ttu-id="a0cda-189">Most tegyük futtassa kissé összetettebb érintő INNER JOIN néhány predikátumok a WHERE záradék a lekérdezés, és nézzük hello sor számának becslése hello régi számossága becslés függvény első.</span><span class="sxs-lookup"><span data-stu-id="a0cda-189">Now, let’s run a slightly more complex query involving an INNER JOIN with a WHERE clause with some predicates, and let’s look at hello row count estimate from hello old Cardinality Estimation function first.</span></span>

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


<span data-ttu-id="a0cda-190">Hello sor becsült funkciójú hello régi számossága becslés jogcímek 194,284 sorok, 200,704 sorok, a lekérdezés végrehajtása hatékonyan adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0cda-190">Executing this query effectively returns 200,704 rows, while hello row estimate with hello old Cardinality Estimation functionality claims 194,284 rows.</span></span> <span data-ttu-id="a0cda-191">Természetesen, az említett előtt, sor száma keresztillesztésével is függ, milyen gyakran futtatta hello előző mintát, amely feltölti hello minta táblák minden egyes futtatásához, és újra.</span><span class="sxs-lookup"><span data-stu-id="a0cda-191">Obviously, as said before, these row count results will also depend how often you ran hello previous samples, which populates hello sample tables over and over again at each run.</span></span> <span data-ttu-id="a0cda-192">A lekérdezés hello predikátumok természetesen is hello tényleges becslés vezérelt hello tábla alakzat, az adatok tartalmának és hogyan ezek az adatok tényleges összefüggéseket egymással hatással.</span><span class="sxs-lookup"><span data-stu-id="a0cda-192">Obviously, hello predicates in your query will also have an influence on hello actual estimation aside from hello table shape, data content, and how this data actually correlate with each other.</span></span>

<span data-ttu-id="a0cda-193">*6. ábra: hello sor számának becslése a rendszer nem 194,284 vagy 6000 sor a hello 200,704 sorok várt.*</span><span class="sxs-lookup"><span data-stu-id="a0cda-193">*Figure 6: hello row count estimate is 194,284 or 6,000 rows off from hello 200,704 rows expected.*</span></span>

![6. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

<span data-ttu-id="a0cda-195">A hello ugyanúgy, most végrehajtani a hello azonos funkciójú hello új számossága becslés lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="a0cda-195">In hello same way, let’s now execute hello same query with hello new Cardinality Estimation functionality.</span></span>

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


<span data-ttu-id="a0cda-196">Az alábbi hello megnézi, azt most meg hello sor becslést 202,877, vagy sokkal szorosabb és magasabb, mint a régi számossága becslés hello.</span><span class="sxs-lookup"><span data-stu-id="a0cda-196">Looking at hello below, we now see that hello row estimate is 202,877, or much closer and higher than hello old Cardinality Estimation.</span></span>

<span data-ttu-id="a0cda-197">*7. ábra: hello sor számának becslése már 202,877 194,284 helyett.*</span><span class="sxs-lookup"><span data-stu-id="a0cda-197">*Figure 7: hello row count estimate is now 202,877, instead of 194,284.*</span></span>

![7. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

<span data-ttu-id="a0cda-199">A valóságban hello eredménykészlet 200,704 sorok (de hello hello lekérdezések futott milyen gyakran az előző minták függ az összes, de ennél is fontosabb, hello TSQL hello RAND() utasítást használja, mert hello visszaadott tényleges értékek eltérhetnek egy futtatási toohello mellett).</span><span class="sxs-lookup"><span data-stu-id="a0cda-199">In reality, hello result set is 200,704 rows (but all of it depends how often you did run hello queries of hello previous samples, but more importantly, because hello TSQL uses hello RAND() statement, hello actual values returned can vary from one run toohello next).</span></span> <span data-ttu-id="a0cda-200">A konkrét példában hello új számossága becslés nem: hello sorok számának becslése, mert 202,877 sokkal szorosabb too200, 704, mint 194,284 jobb munkát!</span><span class="sxs-lookup"><span data-stu-id="a0cda-200">Therefore, in this particular example, hello new Cardinality Estimation does a better job at estimating hello number of rows because 202,877 is much closer too200,704, than 194,284!</span></span> <span data-ttu-id="a0cda-201">Az utolsó, ha módosítja a WHERE záradék predikátumokat tooequality hello (helyett ">" példány), ez akár több különböző sikerült hello becslése hello régi és új számossága függvény között, attól függően, hogy hány megegyezik kaphat.</span><span class="sxs-lookup"><span data-stu-id="a0cda-201">Last, if you change hello WHERE clause predicates tooequality (rather than “>” for instance), this could make hello estimates between hello old and new Cardinality function even more different, depending on how many matches you can get.</span></span>

<span data-ttu-id="a0cda-202">Természetesen ebben az esetben ~ 6000 sort éppen ki a tényleges száma nem felel meg nagy mennyiségű adat bizonyos esetekben.</span><span class="sxs-lookup"><span data-stu-id="a0cda-202">Obviously, in this case, being ~6000 rows off from actual count does not represent a lot of data in some situations.</span></span> <span data-ttu-id="a0cda-203">Most, tenni a sorok toomillions több táblák és összetett lekérdezések között, és esetenként hello becsült sorok milliói ki kell, és ezért a fel-hello helytelen végrehajtási terv, vagy nincs elég memória a kért biztosít bevezető kockázatát hello tooTempDB kijutás, és így további i/o sokkal nagyobb.</span><span class="sxs-lookup"><span data-stu-id="a0cda-203">Now, transpose this toomillions of rows across several tables and more complex queries, and at times hello estimate can be off by millions of rows , and therefore, hello risk of picking-up hello wrong execution plan, or requesting insufficient memory grants leading tooTempDB spills, and so more I/O, are much higher.</span></span>

<span data-ttu-id="a0cda-204">Ha hello lehetőség van, ez az összehasonlítás a leggyakoribb lekérdezések és adatkészletek konfigurációkat, és ismerje által mennyi néhány hello régi és új becslése érint, míg néhány csak válhat a hello valóságban ki további vagy néhány mások csak egyszerűen szorosabb toohello tényleges sorban álló hello eredményhalmazt ténylegesen vissza.</span><span class="sxs-lookup"><span data-stu-id="a0cda-204">If you have hello opportunity, practice this comparison with your most typical queries and datasets, and see for yourself by how much some of hello old and new estimates are affected, while some could just become more off from hello reality, or some others just simply closer toohello actual row counts actually returned in hello result sets.</span></span> <span data-ttu-id="a0cda-205">Az összes hello alakzat lekérdezések, hello Azure SQL adatbázis jellemzőit, hello természetét és a adatkészleteket, és rendelkezésére hello statisztika hello méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="a0cda-205">All of it will depend of hello shape of your queries, hello Azure SQL database characteristics, hello nature and hello size of your datasets, and hello statistics available about them.</span></span> <span data-ttu-id="a0cda-206">Ha az imént létrehozott az Azure SQL Database-példányt, hello lekérdezés optimalizáló toobuild kell a Tudásbázis helyett hello előző lekérdezés tett újbóli felhasználás statisztika teljesen új fut.</span><span class="sxs-lookup"><span data-stu-id="a0cda-206">If you just created your Azure SQL Database instance, hello query optimizer will have toobuild its knowledge from scratch instead of reusing statistics made of hello previous query runs.</span></span> <span data-ttu-id="a0cda-207">Igen hello becslése nagyon környezetfüggő és majdnem adott tooevery kiszolgáló és az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a0cda-207">So, hello estimates are very contextual and almost specific tooevery server and application situation.</span></span> <span data-ttu-id="a0cda-208">Fontos szem előtt egy fontos tookeep!</span><span class="sxs-lookup"><span data-stu-id="a0cda-208">It is an important aspect tookeep in mind!</span></span>

## <a name="some-considerations-tootake-into-account"></a><span data-ttu-id="a0cda-209">Bizonyos szempontok tootake figyelembe</span><span class="sxs-lookup"><span data-stu-id="a0cda-209">Some considerations tootake into account</span></span>
<span data-ttu-id="a0cda-210">Bár a legtöbb alkalmazás és szolgáltatás hello kompatibilitási szint 130, mielőtt az éles környezetben hello kompatibilitási szintje bevezetése előnyös, alapvetően 3 lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="a0cda-210">Although most workloads would benefit from hello compatibility level 130, before you adopting hello compatibility level for your production environment, you basically have 3 options:</span></span>

1. <span data-ttu-id="a0cda-211">Helyezze át a toocompatibility szint 130, és tekintse meg, hogyan dolgot végre.</span><span class="sxs-lookup"><span data-stu-id="a0cda-211">You move toocompatibility level 130, and see how things perform.</span></span> <span data-ttu-id="a0cda-212">Abban az esetben, ha egyes hátrányait figyelje meg, akkor egyszerűen csak hello kompatibilitási szint hátsó tooits eredeti szintje, vagy tartsa 130, és csak fordított hello számossága becslés hátsó toohello örökölt üzemmódú (fentiekben Elmagyarázott, ez nem sikerült hiba megoldása érdekében hello).</span><span class="sxs-lookup"><span data-stu-id="a0cda-212">In case you notice some regressions, you just simply set hello compatibility level back tooits original level, or keep 130, and only reverse hello Cardinality Estimation back toohello legacy mode (As explained above, this alone could address hello issue).</span></span>
2. <span data-ttu-id="a0cda-213">Alaposan tesztelje a meglévő alkalmazások hasonló üzemi terhelés, finomhangolásra és hello teljesítményének folyamatos tooproduction előtt ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="a0cda-213">You thoroughly test your existing applications under similar production load, fine tune, and validate hello performance before going tooproduction.</span></span> <span data-ttu-id="a0cda-214">Esetlegesen felmerülő problémák ugyanaz, mint a fenti, is mindig vissza toohello eredeti kompatibilitási szintet, vagy egyszerűen megfordíthatja hello számossága becslés hátsó toohello örökölt üzemmódú.</span><span class="sxs-lookup"><span data-stu-id="a0cda-214">In case of issues, same as above, you can always go back toohello original compatibility level, or simply reverse hello Cardinality Estimation back toohello legacy mode.</span></span>
3. <span data-ttu-id="a0cda-215">A végső lehetőséget, és hello legutóbbi módon tooaddress tooleverage hello Lekérdezéstár ezek kérdésekre, lesz.</span><span class="sxs-lookup"><span data-stu-id="a0cda-215">As a final option, and hello most recent way tooaddress these questions, is tooleverage hello Query Store.</span></span> <span data-ttu-id="a0cda-216">Ez az ajánlott beállítás az aktuális!</span><span class="sxs-lookup"><span data-stu-id="a0cda-216">That’s today’s recommended option!</span></span> <span data-ttu-id="a0cda-217">tooassist hello elemzése a lekérdezéseket a kompatibilitási szint 120 vagy alatt 130, és nem javasoljuk, elég toouse Lekérdezéstár.</span><span class="sxs-lookup"><span data-stu-id="a0cda-217">tooassist hello analysis of your queries under compatibility level 120 or below versus 130, we cannot encourage you enough toouse Query Store.</span></span> <span data-ttu-id="a0cda-218">A Lekérdezéstár hello legújabb Azure SQL Database 12-es verziójában elérhető, és úgy van kialakítva, toohelp, a lekérdezési teljesítmény hibaelhárításban.</span><span class="sxs-lookup"><span data-stu-id="a0cda-218">Query Store is available with hello latest version of Azure SQL Database V12, and it’s designed toohelp you with query performance troubleshooting.</span></span> <span data-ttu-id="a0cda-219">A Lekérdezéstár hello gondol a felhőszolgáltató közötti átviteléhez rögzítő az adatbázis összegyűjtése és minden lekérdezést előzménymodell részletesen bemutatja.</span><span class="sxs-lookup"><span data-stu-id="a0cda-219">Think of hello Query Store as a flight data recorder for your database collecting and presenting detailed historic information about all queries.</span></span> <span data-ttu-id="a0cda-220">Ez jelentősen leegyszerűsíti a teljesítmény Törvényszéki hello idő toodiagnose csökkentésével, és problémák megoldására.</span><span class="sxs-lookup"><span data-stu-id="a0cda-220">This greatly simplifies performance forensics by reducing hello time toodiagnose and resolve issues.</span></span> <span data-ttu-id="a0cda-221">További információt a [Lekérdezéstár: az adatbázis egy felé továbbított adatok író](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span><span class="sxs-lookup"><span data-stu-id="a0cda-221">You can find more information at [Query Store: A flight data recorder for your database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).</span></span>

<span data-ttu-id="a0cda-222">A magas szintű, ha már fut, vagy az alatti kompatibilitási szinttel rendelkező 120 adatbázisok készleteit, és tervezze meg toomove hello némelyikük too130, vagy mert a számítási feladatok automatikusan létesítsen új adatbázisokat, hogy rövidesen lehetőség alapértelmezett too130 által beállított, fontolja meg az hello a következőket:</span><span class="sxs-lookup"><span data-stu-id="a0cda-222">At hello high-level, if you already have a set of databases running at compatibility level 120 or below, and plan toomove some of them too130, or because your workload automatically provision new databases that will be soon be set by default too130, please consider hello followings:</span></span>

* <span data-ttu-id="a0cda-223">Toohello új kompatibilitási szintjének éles környezetben módosítása előtt engedélyezze a Lekérdezéstárat.</span><span class="sxs-lookup"><span data-stu-id="a0cda-223">Before changing toohello new compatibility level in production, enable Query Store.</span></span> <span data-ttu-id="a0cda-224">Olvassa el a túl[hello adatbázis kompatibilitási módja és -felhasználási hello Lekérdezéstár módosítása](https://msdn.microsoft.com/library/bb895281.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="a0cda-224">You can refer too[Change hello Database Compatibility Mode and Use hello Query Store](https://msdn.microsoft.com/library/bb895281.aspx) for more information.</span></span>
* <span data-ttu-id="a0cda-225">Ezt követően tesztelje összes fontos munkaterhelés jellemző adatok és a lekérdezések egy hasonló környezetet, és hasonlítsa össze hello teljesítmény észlelt, és a Lekérdezéstár jelentett adatokat.</span><span class="sxs-lookup"><span data-stu-id="a0cda-225">Next, test all critical workloads using representative data and queries of a production-like environment, and compare hello performance experienced and as reported by Query Store.</span></span> <span data-ttu-id="a0cda-226">Egyes hátrányait tapasztal, azonosíthatja a Lekérdezéstár hello közleményében szerepelt lekérdezések hello és kényszerített lehetőséget a Lekérdezéstár hello csomag használata (más néven csomag rögzítéshez).</span><span class="sxs-lookup"><span data-stu-id="a0cda-226">If you experience some regressions, you can identify hello regressed queries with hello Query Store and use hello plan forcing option from Query Store (aka plan pinning).</span></span> <span data-ttu-id="a0cda-227">Ebben az esetben akkor véglegesen hello kompatibilitási szintű 130 maradnak, és használja hello korábbi lekérdezésterv hello a Lekérdezéstár által javasolt.</span><span class="sxs-lookup"><span data-stu-id="a0cda-227">In such a case, you definitively stay with hello compatibility level 130, and use hello former query plan as suggested by hello Query Store.</span></span>
* <span data-ttu-id="a0cda-228">Ha szeretné, hogy az Azure SQL Database (amelyen az SQL Server 2016) tooleverage új funkcióira és képességeire, de bizalmas toochanges alá hello kompatibilitási szint 130, utolsó lehetőségként, érdemes lehet hello kompatibilitási szint vissza kényszerítése toohello szintje, amely megfelel a számítási feladatok ALTER DATABASE utasítás segítségével.</span><span class="sxs-lookup"><span data-stu-id="a0cda-228">If you want tooleverage new features and capabilities of Azure SQL Database (which is running SQL Server 2016), but are sensitive toochanges brought by hello compatibility level 130, as a last resort, you could consider forcing hello compatibility level back toohello level that suits your workload by using an ALTER DATABASE statement.</span></span> <span data-ttu-id="a0cda-229">De először, vegye figyelembe beállítás rögzítési hello Lekérdezéstár terv a legjobb lehetőség, mert nem használ 130 alapvetően marad az SQL Server régebbi verziójú hello működési szinten.</span><span class="sxs-lookup"><span data-stu-id="a0cda-229">But first, be aware that hello Query Store plan pinning option is your best option because not using 130 is basically staying at hello functionality level of an older SQL Server version.</span></span>
* <span data-ttu-id="a0cda-230">Ha több adatbázis átfedés több-bérlős alkalmazásokhoz, lehet szükséges tooupdate hello az adatbázisok tooensure egy egységes kompatibilitási szint elvét kiépítés közötti összes; régi és az újonnan telepített néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="a0cda-230">If you have multitenant applications spanning multiple databases, it may be necessary tooupdate hello provisioning logic of your databases tooensure a consistent compatibility level across all databases; old and newly provisioned ones.</span></span> <span data-ttu-id="a0cda-231">Lehet, hogy a munkaterhelés alkalmazásteljesítmény bizalmas toohello tényt, hogy az egyes adatbázisok különböző kompatibilitási szinten futnak, ezért bármely adatbázis kompatibilitási szintjét egységességének szükség lehet ahhoz tooprovide hello azonos felhasználói élmény tooyour ügyfelek összes hello tábla között.</span><span class="sxs-lookup"><span data-stu-id="a0cda-231">Your application workload performance could be sensitive toohello fact that some databases are running at different compatibility levels, and therefore, compatibility level consistency across any database could be required in order tooprovide hello same experience tooyour customers all across hello board.</span></span> <span data-ttu-id="a0cda-232">Vegye figyelembe, hogy nincs megbízás, valóban attól függ, milyen hatással van az alkalmazás által hello kompatibilitási szintet.</span><span class="sxs-lookup"><span data-stu-id="a0cda-232">Note that it is not a mandate, it really depends on how your application is affected by hello compatibility level.</span></span>
* <span data-ttu-id="a0cda-233">Utolsó, hello számossága becslés kapcsolatban, és hasonlóan hello kompatibilitási szint, éles, a folytatás előtt célszerű ajánlott a hello új feltételek toodetermine alatt a termelési számítási feladatok tootest Ha az alkalmazás nyújtotta hello számossága becslés fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="a0cda-233">Last, regarding hello Cardinality Estimation, and just like changing hello compatibility level, before proceeding in production, it is recommended tootest your production workload under hello new conditions toodetermine if your application benefits from hello Cardinality Estimation improvements.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a0cda-234">Összegzés</span><span class="sxs-lookup"><span data-stu-id="a0cda-234">Conclusion</span></span>
<span data-ttu-id="a0cda-235">Azure SQL Database használata az összes SQL Server 2016 fejlesztések toobenefit egyértelműen javíthatja a lekérdezés végrehajtások.</span><span class="sxs-lookup"><span data-stu-id="a0cda-235">Using Azure SQL Database toobenefit from all SQL Server 2016 enhancements can clearly improve your query executions.</span></span> <span data-ttu-id="a0cda-236">Ahogy-van!</span><span class="sxs-lookup"><span data-stu-id="a0cda-236">Just as-is!</span></span> <span data-ttu-id="a0cda-237">Természetesen mint bármely új szolgáltatás a megfelelő próbaverzióra elvégzendő toodetermine hello pontos feltételek alapján, amely az adatbázis munkaterhelés működik hello legjobban.</span><span class="sxs-lookup"><span data-stu-id="a0cda-237">Of course, like any new feature, a proper evaluation must be done toodetermine hello exact conditions under which your database workload operates hello best.</span></span> <span data-ttu-id="a0cda-238">Felhasználói élmény azt mutatja, hogy a legtöbb munkaterhelés várt tooat legalább futni transzparens módon kompatibilitási szint 130, miközben feldolgozási funkciók és új számossága becslés új lekérdezés használhatja.</span><span class="sxs-lookup"><span data-stu-id="a0cda-238">Experience shows that most workload are expected tooat least run transparently under compatibility level 130, while leveraging new query processing functions, and new Cardinality Estimation.</span></span> <span data-ttu-id="a0cda-239">Amely említett reálisan nézve mindig van néhány kivétel, valamint arról, hogy megfelelő megfelelő gondossággal egy fontos assessment toodetermine milyen mértékben kihasználhassa a bővítések.</span><span class="sxs-lookup"><span data-stu-id="a0cda-239">That said, realistically, there are always some exceptions and doing proper due diligence is an important assessment toodetermine how much you can benefit from these enhancements.</span></span> <span data-ttu-id="a0cda-240">És ebben az esetben hasznos lehet a hello Lekérdezéstár kiváló segít a munka során!</span><span class="sxs-lookup"><span data-stu-id="a0cda-240">And again, hello Query Store can be of a great help in doing this work!</span></span>

<span data-ttu-id="a0cda-241">Az SQL Azure fejlődésének számíthat jövőbeli hello a kompatibilitási szintet 140.</span><span class="sxs-lookup"><span data-stu-id="a0cda-241">As SQL Azure evolves, you can expect a compatibility level 140 in hello future.</span></span> <span data-ttu-id="a0cda-242">Megfelelő idő esetén először fog van szó Mi a későbbi kompatibilitási szint 140 be fogja hozni, ugyanúgy, mint a röviden ismertettük itt milyen kompatibilitási szint 130 ma mihamarabb elérhetővé tenni.</span><span class="sxs-lookup"><span data-stu-id="a0cda-242">When time is appropriate, we will start talking about what this future compatibility level 140 will bring, just as we briefly discussed here what compatibility level 130 is bringing today.</span></span>

<span data-ttu-id="a0cda-243">Most tegyük feledkezzen, 2016. június-től kezdődően az Azure SQL Database állapotúról hello alapértelmezett kompatibilitási szintje 120 too130 az újonnan létrehozott adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="a0cda-243">For now, let’s not forget, starting June 2016, Azure SQL Database will change hello default compatibility level from 120 too130 for newly created databases.</span></span> <span data-ttu-id="a0cda-244">Ne feledje!</span><span class="sxs-lookup"><span data-stu-id="a0cda-244">Be aware!</span></span>

## <a name="references"></a><span data-ttu-id="a0cda-245">Referencia</span><span class="sxs-lookup"><span data-stu-id="a0cda-245">References</span></span>
* [<span data-ttu-id="a0cda-246">Adatbázis-kezelő újdonságai</span><span class="sxs-lookup"><span data-stu-id="a0cda-246">What’s New in Database Engine</span></span>](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [<span data-ttu-id="a0cda-247">Blog: A Lekérdezéstár: egy felé továbbított adatok író Borko Novakovic, 8 2016. június az adatbázis számára</span><span class="sxs-lookup"><span data-stu-id="a0cda-247">Blog: Query Store: A flight data recorder for your database, by Borko Novakovic, June 8 2016</span></span>](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [<span data-ttu-id="a0cda-248">ALTER adatbázis kompatibilitási szintje (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="a0cda-248">ALTER DATABASE Compatibility Level (Transact-SQL)</span></span>](https://msdn.microsoft.com/library/bb510680.aspx)
* [<span data-ttu-id="a0cda-249">ALTER DATABASE HATÓKÖRŰ KONFIGURÁCIÓ</span><span class="sxs-lookup"><span data-stu-id="a0cda-249">ALTER DATABASE SCOPED CONFIGURATION</span></span>](https://msdn.microsoft.com/library/mt629158.aspx)
* [<span data-ttu-id="a0cda-250">Az Azure SQL Database 12-es kompatibilitási szintje 130</span><span class="sxs-lookup"><span data-stu-id="a0cda-250">Compatibility Level 130 for Azure SQL Database V12</span></span>](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [<span data-ttu-id="a0cda-251">A lekérdezés tervek optimalizálja az SQL Server 2014 Számosságbecslő hello</span><span class="sxs-lookup"><span data-stu-id="a0cda-251">Optimizing Your Query Plans with hello SQL Server 2014 Cardinality Estimator</span></span>](https://msdn.microsoft.com/library/dn673537.aspx)
* [<span data-ttu-id="a0cda-252">Oszlopcentrikus indexek útmutató</span><span class="sxs-lookup"><span data-stu-id="a0cda-252">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
* [<span data-ttu-id="a0cda-253">Blog: Jobb teljesítmény-küszöbérték-es kompatibilitási szintű 130 az Azure SQL-adatbázis által Alain Lissoir 6 2016. május</span><span class="sxs-lookup"><span data-stu-id="a0cda-253">Blog: Improved Query Performance with Compatibility Level 130 in Azure SQL Database, by Alain Lissoir, May 6 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

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
