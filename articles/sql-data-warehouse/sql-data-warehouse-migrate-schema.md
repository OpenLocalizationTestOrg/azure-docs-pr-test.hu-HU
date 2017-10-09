---
title: "aaaMigrate a séma tooSQL adatraktár |} Microsoft Docs"
description: "Tippek a séma tooAzure SQL Data Warehouse adattárházzal történő, megoldások áttelepítéséhez."
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
ms.openlocfilehash: 1309b743b78564575695038a4856d9d25a2b18d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-schemas-toosql-data-warehouse"></a><span data-ttu-id="3fda9-103">Telepítse át a sémák tooSQL Data warehouse-bA</span><span class="sxs-lookup"><span data-stu-id="3fda9-103">Migrate your schemas tooSQL Data Warehouse</span></span>
<span data-ttu-id="3fda9-104">Az SQL-sémák tooSQL Data warehouse-ba történő áttelepítésére vonatkozó útmutatást.</span><span class="sxs-lookup"><span data-stu-id="3fda9-104">Guidance for migrating your SQL schemas tooSQL Data Warehouse.</span></span> 

## <a name="plan-your-schema-migration"></a><span data-ttu-id="3fda9-105">A séma áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="3fda9-105">Plan your schema migration</span></span>

<span data-ttu-id="3fda9-106">Áttelepítés tervezése közben, lásd: hello [tábla áttekintése] [ table overview] toobecome ismeri a táblázat kialakítási szempontok például statisztikáiról, terjesztési, particionálás, és az indexelés.</span><span class="sxs-lookup"><span data-stu-id="3fda9-106">As you plan a migration, see hello [table overview][table overview] toobecome familiar with table design considerations such as statistics, distribution, partitioning, and indexing.</span></span>  <span data-ttu-id="3fda9-107">Azt is soroljuk [táblában funkciók nem támogatott] [ unsupported table features] és azok megoldását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3fda9-107">It also lists some [unsupported table features][unsupported table features] and their workarounds.</span></span>

## <a name="use-user-defined-schemas-tooconsolidate-databases"></a><span data-ttu-id="3fda9-108">Felhasználó által definiált sémák tooconsolidate adatbázisok használata</span><span class="sxs-lookup"><span data-stu-id="3fda9-108">Use user-defined schemas tooconsolidate databases</span></span>

<span data-ttu-id="3fda9-109">A meglévő alkalmazások és szolgáltatások valószínűleg tartalmazza az egynél több adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="3fda9-109">Your existing workload probably has more than one database.</span></span> <span data-ttu-id="3fda9-110">Egy SQL Server data warehouse tartalmazhat például egy átmeneti adatbázis, adatraktár-adatbázis és néhány adat adatközpont-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3fda9-110">For example, a SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="3fda9-111">Ebben a topológiában minden adatbázisnak külön biztonsági házirendek külön feladatként fut.</span><span class="sxs-lookup"><span data-stu-id="3fda9-111">In this topology, each database runs as a separate workload with separate security policies.</span></span>

<span data-ttu-id="3fda9-112">Ezzel szemben a SQL Data Warehouse hello teljes adatraktározás számítási feladatáról egy adatbázison belül futtatja.</span><span class="sxs-lookup"><span data-stu-id="3fda9-112">By contrast, SQL Data Warehouse runs hello entire data warehouse workload within one database.</span></span> <span data-ttu-id="3fda9-113">Adatbázis közötti illesztések nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="3fda9-113">Cross database joins are not permitted.</span></span> <span data-ttu-id="3fda9-114">Ezért az SQL Data Warehouse hello data warehouse toobe hello egy adatbázisban tárolt által használt összes táblának vár.</span><span class="sxs-lookup"><span data-stu-id="3fda9-114">Therefore, SQL Data Warehouse expects all tables used by hello data warehouse toobe stored within hello one database.</span></span>

<span data-ttu-id="3fda9-115">Azt javasoljuk, felhasználó által definiált sémák tooconsolidate a meglévő alkalmazások és szolgáltatások egy adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="3fda9-115">We recommend using user-defined schemas tooconsolidate your existing workload into one database.</span></span> <span data-ttu-id="3fda9-116">Tekintse meg a [felhasználói sémák](sql-data-warehouse-develop-user-defined-schemas.md)</span><span class="sxs-lookup"><span data-stu-id="3fda9-116">For examples, see [User-defined schemas](sql-data-warehouse-develop-user-defined-schemas.md)</span></span>

## <a name="use-compatible-data-types"></a><span data-ttu-id="3fda9-117">Kompatibilis adattípusok használata</span><span class="sxs-lookup"><span data-stu-id="3fda9-117">Use compatible data types</span></span>
<span data-ttu-id="3fda9-118">Módosítsa az adatok típusok toobe az SQL Data Warehouse szolgáltatással kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="3fda9-118">Modify your data types toobe compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="3fda9-119">Támogatott és nem támogatott adattípusú listájáért lásd: [adattípusok][data types].</span><span class="sxs-lookup"><span data-stu-id="3fda9-119">For a list of supported and unsupported data types, see [data types][data types].</span></span> <span data-ttu-id="3fda9-120">Témakör biztosít a lehetséges megoldások hello nem támogatott típusú.</span><span class="sxs-lookup"><span data-stu-id="3fda9-120">That topic gives workarounds for hello unsupported types.</span></span> <span data-ttu-id="3fda9-121">Emellett biztosítja a lekérdezés tooidentify meglévő típusok nem támogatottak az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3fda9-121">It also provides a query tooidentify existing types that are not supported in SQL Data Warehouse.</span></span>

## <a name="minimize-row-size"></a><span data-ttu-id="3fda9-122">Sor mérete minimalizálása érdekében</span><span class="sxs-lookup"><span data-stu-id="3fda9-122">Minimize row size</span></span>
<span data-ttu-id="3fda9-123">A legjobb teljesítmény érdekében minimalizálása érdekében a táblák hello sorhosszt.</span><span class="sxs-lookup"><span data-stu-id="3fda9-123">For best performance, minimize hello row length of your tables.</span></span> <span data-ttu-id="3fda9-124">Rövidebb sor hosszának toobetter teljesítmény vezethet, mivel hello legkisebb adattípusok használatát, amelyek működnek az adatok.</span><span class="sxs-lookup"><span data-stu-id="3fda9-124">Since shorter row lengths lead toobetter performance, use hello smallest data types that work for your data.</span></span> 

<span data-ttu-id="3fda9-125">A táblázat sor szélessége a PolyBase van a 1 Megabájtos korlátot.</span><span class="sxs-lookup"><span data-stu-id="3fda9-125">For table row width, PolyBase has a 1 MB limit.</span></span>  <span data-ttu-id="3fda9-126">Ha tooload adatokat az SQL Data Warehouse PolyBase rendelkező, frissítse a táblák toohave sor maximális szélessége 1 MB-nál kevesebb.</span><span class="sxs-lookup"><span data-stu-id="3fda9-126">If you plan tooload data into SQL Data Warehouse with PolyBase, update your tables toohave maximum row widths of less than 1 MB.</span></span> 

<!--
- For example, this table uses variable length data but hello largest possible size of hello row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and hello defined row width is less than one MB. When loading rows, PolyBase allocates hello full length of hello variable-length data. hello full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-hello-distribution-option"></a><span data-ttu-id="3fda9-127">Hello terjesztési beállítást adja meg</span><span class="sxs-lookup"><span data-stu-id="3fda9-127">Specify hello distribution option</span></span>
<span data-ttu-id="3fda9-128">Az SQL Data Warehouse egy olyan elosztott adatbázis rendszer.</span><span class="sxs-lookup"><span data-stu-id="3fda9-128">SQL Data Warehouse is a distributed database system.</span></span> <span data-ttu-id="3fda9-129">Minden tábla elosztott vagy replikált hello számítási csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="3fda9-129">Each table is distributed or replicated across hello Compute nodes.</span></span> <span data-ttu-id="3fda9-130">Nincs olyan tábla beállítás, amely lehetővé teszi, hogy adja meg, hogyan toodistribute hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="3fda9-130">There's a table option that lets you specify how toodistribute hello data.</span></span> <span data-ttu-id="3fda9-131">hello használhatók ciklikus multiplexelés replikálva, vagy kivonatoló elosztott.</span><span class="sxs-lookup"><span data-stu-id="3fda9-131">hello choices are  round-robin, replicated, or hash distributed.</span></span> <span data-ttu-id="3fda9-132">Minden egyes vannak előnyei és hátrányai.</span><span class="sxs-lookup"><span data-stu-id="3fda9-132">Each has pros and cons.</span></span> <span data-ttu-id="3fda9-133">Ha nem ad meg hello terjesztő beállítást, az SQL Data Warehouse használja ciklikus multiplexelés hello alapértelmezettként.</span><span class="sxs-lookup"><span data-stu-id="3fda9-133">If you don't specify hello distribution option, SQL Data Warehouse will use round-robin as hello default.</span></span>

- <span data-ttu-id="3fda9-134">Ciklikus multiplexelés hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="3fda9-134">Round-robin is hello default.</span></span> <span data-ttu-id="3fda9-135">Hello legegyszerűbb toouse, és lehetséges gyorsaságának hello adatokat tölt, de illesztések adatmozgás, ami megnöveli a lekérdezési teljesítmény szükséges.</span><span class="sxs-lookup"><span data-stu-id="3fda9-135">It is hello simplest toouse, and loads hello data as fast as possible, but joins will require data movement which slows query performance.</span></span>
- <span data-ttu-id="3fda9-136">A replikált tároló egy-egy példányát minden számítási csomóponton hello tábla.</span><span class="sxs-lookup"><span data-stu-id="3fda9-136">Replicated stores a copy of hello table on each Compute node.</span></span> <span data-ttu-id="3fda9-137">Replikált táblák nincsenek performant, mert az összekapcsolásokhoz és az összesítések nem igényelnek adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="3fda9-137">Replicated tables are performant because they do not require data movement for joins and aggregations.</span></span> <span data-ttu-id="3fda9-138">Ezek – megnövelt tárhely szükséges, és ezért működhet a legjobban a kisebb táblákat.</span><span class="sxs-lookup"><span data-stu-id="3fda9-138">They do require extra storage, and therefore work best for smaller tables.</span></span>
- <span data-ttu-id="3fda9-139">Elosztott kivonatoló hello sorok elosztása az összes hello csomópontok keresztül kivonatoló függvényt.</span><span class="sxs-lookup"><span data-stu-id="3fda9-139">Hash distributed distributes hello rows across all hello nodes via a hash function.</span></span> <span data-ttu-id="3fda9-140">A kivonattáblákkal elosztott hello szív az SQL Data Warehouse, mert azok tervezett tooprovide a lekérdezési teljesítmény nagy táblákon.</span><span class="sxs-lookup"><span data-stu-id="3fda9-140">Hash distributed tables are hello heart of SQL Data Warehouse since they are designed tooprovide high query performance on large tables.</span></span> <span data-ttu-id="3fda9-141">Ez a beállítás megköveteli néhány tervezési tooselect hello legjobb oszlop toodistribute hello adatokról.</span><span class="sxs-lookup"><span data-stu-id="3fda9-141">This option requires some planning tooselect hello best column on which toodistribute hello data.</span></span> <span data-ttu-id="3fda9-142">Azonban ha nem választja hello legjobb oszlop hello először, könnyen újra terjesztheti hello adatokat egy másik oszlop alapján.</span><span class="sxs-lookup"><span data-stu-id="3fda9-142">However, if you don't choose hello best column hello first time, you can easily re-distribute hello data on a different column.</span></span> 

<span data-ttu-id="3fda9-143">toochoose hello ajánlott terjesztési beállítás mindegyikhez, lásd: [táblák elosztott](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="3fda9-143">toochoose hello best distribution option for each table, see [Distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="3fda9-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fda9-144">Next steps</span></span>
<span data-ttu-id="3fda9-145">Miután sikeresen áttelepítette az adatbázis-séma tooSQL Data warehouse-ba, folytassa a következő cikkek hello tooone:</span><span class="sxs-lookup"><span data-stu-id="3fda9-145">Once you have successfully migrated your database schema tooSQL Data Warehouse, proceed tooone of hello following articles:</span></span>

* <span data-ttu-id="3fda9-146">[Adatok áttelepítése][Migrate your data]</span><span class="sxs-lookup"><span data-stu-id="3fda9-146">[Migrate your data][Migrate your data]</span></span>
* <span data-ttu-id="3fda9-147">[Telepítse át a kódot][Migrate your code]</span><span class="sxs-lookup"><span data-stu-id="3fda9-147">[Migrate your code][Migrate your code]</span></span>

<span data-ttu-id="3fda9-148">Az SQL Data Warehouse gyakorlati tanácsokat kapcsolatban bővebben lásd: hello [ajánlott eljárások] [ best practices] cikk.</span><span class="sxs-lookup"><span data-stu-id="3fda9-148">For more about SQL Data Warehouse best practices, see hello [best practices][best practices] article.</span></span>

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
