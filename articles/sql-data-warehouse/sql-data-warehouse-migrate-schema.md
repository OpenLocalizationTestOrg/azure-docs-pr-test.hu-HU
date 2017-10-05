---
title: "A séma áttelepítése az SQL Data Warehouse |} Microsoft Docs"
description: "Tippek a séma áttelepítése az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a><span data-ttu-id="c083d-103">Az SQL Data Warehouse a sémák áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c083d-103">Migrate your schemas to SQL Data Warehouse</span></span>
<span data-ttu-id="c083d-104">Az SQL-sémák áttelepítéséhez az SQL Data Warehouse-útmutatót.</span><span class="sxs-lookup"><span data-stu-id="c083d-104">Guidance for migrating your SQL schemas to SQL Data Warehouse.</span></span> 

## <a name="plan-your-schema-migration"></a><span data-ttu-id="c083d-105">A séma áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="c083d-105">Plan your schema migration</span></span>

<span data-ttu-id="c083d-106">Áttelepítés tervezése közben, lásd: a [tábla áttekintése] [ table overview] megismerkedhet a táblázat kialakítási szempontok például statisztikáiról, terjesztési, particionálás, és az indexelés.</span><span class="sxs-lookup"><span data-stu-id="c083d-106">As you plan a migration, see the [table overview][table overview] to become familiar with table design considerations such as statistics, distribution, partitioning, and indexing.</span></span>  <span data-ttu-id="c083d-107">Azt is soroljuk [táblában funkciók nem támogatott] [ unsupported table features] és azok megoldását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c083d-107">It also lists some [unsupported table features][unsupported table features] and their workarounds.</span></span>

## <a name="use-user-defined-schemas-to-consolidate-databases"></a><span data-ttu-id="c083d-108">Felhasználói sémákkal segítségével adatbázisok összesítése</span><span class="sxs-lookup"><span data-stu-id="c083d-108">Use user-defined schemas to consolidate databases</span></span>

<span data-ttu-id="c083d-109">A meglévő alkalmazások és szolgáltatások valószínűleg tartalmazza az egynél több adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="c083d-109">Your existing workload probably has more than one database.</span></span> <span data-ttu-id="c083d-110">Egy SQL Server data warehouse tartalmazhat például egy átmeneti adatbázis, adatraktár-adatbázis és néhány adat adatközpont-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c083d-110">For example, a SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="c083d-111">Ebben a topológiában minden adatbázisnak külön biztonsági házirendek külön feladatként fut.</span><span class="sxs-lookup"><span data-stu-id="c083d-111">In this topology, each database runs as a separate workload with separate security policies.</span></span>

<span data-ttu-id="c083d-112">Az SQL Data Warehouse ellentétben a teljes adatraktározás számítási feladatáról egy adatbázison belül futtatja.</span><span class="sxs-lookup"><span data-stu-id="c083d-112">By contrast, SQL Data Warehouse runs the entire data warehouse workload within one database.</span></span> <span data-ttu-id="c083d-113">Adatbázis közötti illesztések nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="c083d-113">Cross database joins are not permitted.</span></span> <span data-ttu-id="c083d-114">Ezért az SQL Data Warehouse egy adatbázis tárolja az adatraktár által használt összes táblának vár.</span><span class="sxs-lookup"><span data-stu-id="c083d-114">Therefore, SQL Data Warehouse expects all tables used by the data warehouse to be stored within the one database.</span></span>

<span data-ttu-id="c083d-115">Azt javasoljuk, felhasználó által definiált sémák vonják össze a meglévő alkalmazások és szolgáltatások egy adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="c083d-115">We recommend using user-defined schemas to consolidate your existing workload into one database.</span></span> <span data-ttu-id="c083d-116">Tekintse meg a [felhasználói sémák](sql-data-warehouse-develop-user-defined-schemas.md)</span><span class="sxs-lookup"><span data-stu-id="c083d-116">For examples, see [User-defined schemas](sql-data-warehouse-develop-user-defined-schemas.md)</span></span>

## <a name="use-compatible-data-types"></a><span data-ttu-id="c083d-117">Kompatibilis adattípusok használata</span><span class="sxs-lookup"><span data-stu-id="c083d-117">Use compatible data types</span></span>
<span data-ttu-id="c083d-118">Ahhoz, hogy az SQL Data Warehouse szolgáltatással kompatibilis az adattípusok módosításával.</span><span class="sxs-lookup"><span data-stu-id="c083d-118">Modify your data types to be compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="c083d-119">Támogatott és nem támogatott adattípusú listájáért lásd: [adattípusok][data types].</span><span class="sxs-lookup"><span data-stu-id="c083d-119">For a list of supported and unsupported data types, see [data types][data types].</span></span> <span data-ttu-id="c083d-120">Témakör lehetséges megoldások ad a nem támogatott típusú.</span><span class="sxs-lookup"><span data-stu-id="c083d-120">That topic gives workarounds for the unsupported types.</span></span> <span data-ttu-id="c083d-121">A lekérdezés nem támogatott az SQL Data Warehouse típusú létező azonosítására is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c083d-121">It also provides a query to identify existing types that are not supported in SQL Data Warehouse.</span></span>

## <a name="minimize-row-size"></a><span data-ttu-id="c083d-122">Sor mérete minimalizálása érdekében</span><span class="sxs-lookup"><span data-stu-id="c083d-122">Minimize row size</span></span>
<span data-ttu-id="c083d-123">A legjobb teljesítmény érdekében minimalizálása érdekében a táblák sor hossza.</span><span class="sxs-lookup"><span data-stu-id="c083d-123">For best performance, minimize the row length of your tables.</span></span> <span data-ttu-id="c083d-124">Mivel rövidebb sor hosszának előfordulhat, hogy jobb teljesítményt, használja a legkisebb adatok, amelyek működnek az adatok.</span><span class="sxs-lookup"><span data-stu-id="c083d-124">Since shorter row lengths lead to better performance, use the smallest data types that work for your data.</span></span> 

<span data-ttu-id="c083d-125">A táblázat sor szélessége a PolyBase van a 1 Megabájtos korlátot.</span><span class="sxs-lookup"><span data-stu-id="c083d-125">For table row width, PolyBase has a 1 MB limit.</span></span>  <span data-ttu-id="c083d-126">Adatok betöltése az SQL Data Warehouse PolyBase tervez, ha rendelkezik a legnagyobb sor vonalainak 1 MB-nál kevesebb a táblák frissítésével.</span><span class="sxs-lookup"><span data-stu-id="c083d-126">If you plan to load data into SQL Data Warehouse with PolyBase, update your tables to have maximum row widths of less than 1 MB.</span></span> 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a><span data-ttu-id="c083d-127">Adja meg a terjesztési beállítást</span><span class="sxs-lookup"><span data-stu-id="c083d-127">Specify the distribution option</span></span>
<span data-ttu-id="c083d-128">Az SQL Data Warehouse egy olyan elosztott adatbázis rendszer.</span><span class="sxs-lookup"><span data-stu-id="c083d-128">SQL Data Warehouse is a distributed database system.</span></span> <span data-ttu-id="c083d-129">Minden tábla elosztott vagy a számítási csomópontok replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="c083d-129">Each table is distributed or replicated across the Compute nodes.</span></span> <span data-ttu-id="c083d-130">Nincs olyan tábla beállítás, amely lehetővé teszi, hogy miként ossza el az adatokat adja meg.</span><span class="sxs-lookup"><span data-stu-id="c083d-130">There's a table option that lets you specify how to distribute the data.</span></span> <span data-ttu-id="c083d-131">Az alábbiak közül választhat ciklikus multiplexelés replikálva, vagy a kivonatoló elosztott.</span><span class="sxs-lookup"><span data-stu-id="c083d-131">The choices are  round-robin, replicated, or hash distributed.</span></span> <span data-ttu-id="c083d-132">Minden egyes vannak előnyei és hátrányai.</span><span class="sxs-lookup"><span data-stu-id="c083d-132">Each has pros and cons.</span></span> <span data-ttu-id="c083d-133">Ha nem adja meg a terjesztő beállítást, az SQL Data Warehouse ciklikus multiplexelés fogja használni az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="c083d-133">If you don't specify the distribution option, SQL Data Warehouse will use round-robin as the default.</span></span>

- <span data-ttu-id="c083d-134">Ciklikus multiplexelés az alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="c083d-134">Round-robin is the default.</span></span> <span data-ttu-id="c083d-135">A legegyszerűbb használja, és a terhelések lassabbak, de illesztések adatok adatmozgás, ami megnöveli a lekérdezési teljesítmény szükséges.</span><span class="sxs-lookup"><span data-stu-id="c083d-135">It is the simplest to use, and loads the data as fast as possible, but joins will require data movement which slows query performance.</span></span>
- <span data-ttu-id="c083d-136">A replikált tároló egy-egy példányát a tábla minden számítási csomóponton.</span><span class="sxs-lookup"><span data-stu-id="c083d-136">Replicated stores a copy of the table on each Compute node.</span></span> <span data-ttu-id="c083d-137">Replikált táblák nincsenek performant, mert az összekapcsolásokhoz és az összesítések nem igényelnek adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="c083d-137">Replicated tables are performant because they do not require data movement for joins and aggregations.</span></span> <span data-ttu-id="c083d-138">Ezek – megnövelt tárhely szükséges, és ezért működhet a legjobban a kisebb táblákat.</span><span class="sxs-lookup"><span data-stu-id="c083d-138">They do require extra storage, and therefore work best for smaller tables.</span></span>
- <span data-ttu-id="c083d-139">Elosztott kivonat kivonatoló függvényt keresztül a csomópontok a sorok elosztása.</span><span class="sxs-lookup"><span data-stu-id="c083d-139">Hash distributed distributes the rows across all the nodes via a hash function.</span></span> <span data-ttu-id="c083d-140">A kivonattáblákkal elosztott SQL Data Warehouse lelke, mivel úgy vannak kialakítva, hogy a lekérdezési teljesítmény biztosítható a nagy táblák.</span><span class="sxs-lookup"><span data-stu-id="c083d-140">Hash distributed tables are the heart of SQL Data Warehouse since they are designed to provide high query performance on large tables.</span></span> <span data-ttu-id="c083d-141">Ez a beállítás megköveteli néhány jelölje ki a legjobb oszlopot, amelyen az adatok terjeszteni tervezi.</span><span class="sxs-lookup"><span data-stu-id="c083d-141">This option requires some planning to select the best column on which to distribute the data.</span></span> <span data-ttu-id="c083d-142">Azonban ha nem a legjobb oszlop először, könnyen újra terjesztheti az adatokat egy másik oszlop alapján.</span><span class="sxs-lookup"><span data-stu-id="c083d-142">However, if you don't choose the best column the first time, you can easily re-distribute the data on a different column.</span></span> 

<span data-ttu-id="c083d-143">A legjobb telepítési lehetőség minden táblához, lásd: [táblák elosztott](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="c083d-143">To choose the best distribution option for each table, see [Distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="c083d-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c083d-144">Next steps</span></span>
<span data-ttu-id="c083d-145">Miután sikeresen áttelepítette az adatbázissémát az SQL Data Warehouse, folytassa a következő cikkekben:</span><span class="sxs-lookup"><span data-stu-id="c083d-145">Once you have successfully migrated your database schema to SQL Data Warehouse, proceed to one of the following articles:</span></span>

* <span data-ttu-id="c083d-146">[Adatok áttelepítése][Migrate your data]</span><span class="sxs-lookup"><span data-stu-id="c083d-146">[Migrate your data][Migrate your data]</span></span>
* <span data-ttu-id="c083d-147">[Telepítse át a kódot][Migrate your code]</span><span class="sxs-lookup"><span data-stu-id="c083d-147">[Migrate your code][Migrate your code]</span></span>

<span data-ttu-id="c083d-148">Az SQL Data Warehouse gyakorlati tanácsokat kapcsolatban bővebben lásd: a [ajánlott eljárások] [ best practices] cikk.</span><span class="sxs-lookup"><span data-stu-id="c083d-148">For more about SQL Data Warehouse best practices, see the [best practices][best practices] article.</span></span>

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
