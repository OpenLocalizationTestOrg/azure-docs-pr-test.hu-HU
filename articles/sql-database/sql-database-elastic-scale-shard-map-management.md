---
title: "Azure SQL-adatbázis kimenő aaaScale |} Microsoft Docs"
description: "Hogyan toouse hello ShardMapManager, elastic database ügyféloldali kódtár"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 4cf670d42ad7ded98fb8d6f0830154587dd2c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-out-databases-with-hello-shard-map-manager"></a><span data-ttu-id="ea638-103">Horizontális felskálázás hello shard térkép manager adatbázisokkal</span><span class="sxs-lookup"><span data-stu-id="ea638-103">Scale out databases with hello shard map manager</span></span>
<span data-ttu-id="ea638-104">a Azure SQL-adatbázisok tooeasily kibővítési-kezelővel a shard térkép.</span><span class="sxs-lookup"><span data-stu-id="ea638-104">tooeasily scale out databases on SQL Azure, use a shard map manager.</span></span> <span data-ttu-id="ea638-105">hello shard térkép manager shard csoportban lévő összes szilánkok (adatbázisok) globális hozzárendelés információt egy különleges adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ea638-105">hello shard map manager is a special database that maintains global mapping information about all shards (databases) in a shard set.</span></span> <span data-ttu-id="ea638-106">hello metaadatok lehetővé teszi, hogy az alkalmazás tooconnect toohello megfelelő adatbázisához hello hello értéke alapján **horizontális kulcs**.</span><span class="sxs-lookup"><span data-stu-id="ea638-106">hello metadata allows an application tooconnect toohello correct database based upon hello value of hello **sharding key**.</span></span> <span data-ttu-id="ea638-107">Emellett minden shard hello készlet tartalmazza, amelyek nyomon követik a hello helyi részekre bonthatók az adatok (úgynevezett **shardlets**).</span><span class="sxs-lookup"><span data-stu-id="ea638-107">In addition, every shard in hello set contains maps that track hello local shard data (known as **shardlets**).</span></span> 

![A shard leképezés kezelése](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

<span data-ttu-id="ea638-109">Alapvető tooshard térkép felügyeleti megértése, hogyan össze ezeket a leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="ea638-109">Understanding how these maps are constructed is essential tooshard map management.</span></span> <span data-ttu-id="ea638-110">Ebben az esetben hello segítségével [ShardMapManager osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), míg a talált a hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md) toomanage shard képezi le.</span><span class="sxs-lookup"><span data-stu-id="ea638-110">This is done using hello [ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), found in hello [Elastic Database client library](sql-database-elastic-database-client-library.md) toomanage shard maps.</span></span>  

## <a name="shard-maps-and-shard-mappings"></a><span data-ttu-id="ea638-111">A shard leképezések és shard hozzárendelések</span><span class="sxs-lookup"><span data-stu-id="ea638-111">Shard maps and shard mappings</span></span>
<span data-ttu-id="ea638-112">Az egyes shard shard térkép toocreate hello típusú kell választania.</span><span class="sxs-lookup"><span data-stu-id="ea638-112">For each shard, you must select hello type of shard map toocreate.</span></span> <span data-ttu-id="ea638-113">hello választáshoz hello adatbázis-architektúra:</span><span class="sxs-lookup"><span data-stu-id="ea638-113">hello choice depends on hello database architecture:</span></span> 

1. <span data-ttu-id="ea638-114">Adatbázisonként egybérlős</span><span class="sxs-lookup"><span data-stu-id="ea638-114">Single tenant per database</span></span>  
2. <span data-ttu-id="ea638-115">Több bérlő adatbázisonként (két típus):</span><span class="sxs-lookup"><span data-stu-id="ea638-115">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="ea638-116">Lista leképezése</span><span class="sxs-lookup"><span data-stu-id="ea638-116">List mapping</span></span>
   2. <span data-ttu-id="ea638-117">Tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="ea638-117">Range mapping</span></span>

<span data-ttu-id="ea638-118">Single-bérlő modell, hozzon létre egy **lista leképezési** shard leképezés.</span><span class="sxs-lookup"><span data-stu-id="ea638-118">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="ea638-119">hello single-bérlős modell bérlőnként egy adatbázis rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="ea638-119">hello single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="ea638-120">Ez megegyezik egy hatékony modell Szolgáltatottszoftver-fejlesztőknek egyszerűbbé teszi a felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="ea638-120">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Lista leképezése][1]

<span data-ttu-id="ea638-122">hello több-bérlős modell rendeli hozzá több bérlő tooa egyetlen adatbázis (és a bérlő csoportok szét több adatbázis).</span><span class="sxs-lookup"><span data-stu-id="ea638-122">hello multi-tenant model assigns several tenants tooa single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="ea638-123">Ezt a modellt használja, ha várhatóan mindegyik bérlő toohave kis adatokat.</span><span class="sxs-lookup"><span data-stu-id="ea638-123">Use this model when you expect each tenant toohave small data needs.</span></span> <span data-ttu-id="ea638-124">Ez a modell azt rendelje hozzá a bérlők tooa adatbázis használatával széles **tartomány leképezése**.</span><span class="sxs-lookup"><span data-stu-id="ea638-124">In this model, we assign a range of tenants tooa database using **range mapping**.</span></span> 

![Tartomány leképezése][2]

<span data-ttu-id="ea638-126">Megvalósíthat egy adatbázis több-bérlős modell használatával vagy egy *lista leképezési* tooassign egy adatbázis több bérlő tooa.</span><span class="sxs-lookup"><span data-stu-id="ea638-126">Or you can implement a multi-tenant database model using a *list mapping* tooassign multiple tenants tooa single database.</span></span> <span data-ttu-id="ea638-127">Például D1 bérlőazonosító 1 és 5 használt toostore információt, és DB2 eltárolja 7 bérlői és bérlői 10.</span><span class="sxs-lookup"><span data-stu-id="ea638-127">For example, DB1 is used toostore information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Az egyetlen DB Muliple bérlők][3] 

### <a name="supported-net-types-for-sharding-keys"></a><span data-ttu-id="ea638-129">A horizontális kulcsok támogatott .net-típusok</span><span class="sxs-lookup"><span data-stu-id="ea638-129">Supported .Net types for sharding keys</span></span>
<span data-ttu-id="ea638-130">Rugalmas méretezési támogatási hello a következő .net Framework meg kell adnia horizontális kulcsként:</span><span class="sxs-lookup"><span data-stu-id="ea638-130">Elastic Scale support hello following .Net Framework types as sharding keys:</span></span>

* <span data-ttu-id="ea638-131">egész szám</span><span class="sxs-lookup"><span data-stu-id="ea638-131">integer</span></span>
* <span data-ttu-id="ea638-132">hosszú</span><span class="sxs-lookup"><span data-stu-id="ea638-132">long</span></span>
* <span data-ttu-id="ea638-133">GUID</span><span class="sxs-lookup"><span data-stu-id="ea638-133">guid</span></span>
* <span data-ttu-id="ea638-134">Byte]</span><span class="sxs-lookup"><span data-stu-id="ea638-134">byte[]</span></span>  
* <span data-ttu-id="ea638-135">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ea638-135">datetime</span></span>
* <span data-ttu-id="ea638-136">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ea638-136">timespan</span></span>
* <span data-ttu-id="ea638-137">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="ea638-137">datetimeoffset</span></span>

### <a name="list-and-range-shard-maps"></a><span data-ttu-id="ea638-138">Lista és a tartomány shard leképezések</span><span class="sxs-lookup"><span data-stu-id="ea638-138">List and range shard maps</span></span>
<span data-ttu-id="ea638-139">A shard maps használatával lehet létrehozni **listák az egyes horizontális kulcsértékek**, vagy azok lehet létrehozni használatával **horizontális tartományok kulcsértékek**.</span><span class="sxs-lookup"><span data-stu-id="ea638-139">Shard maps can be constructed using **lists of individual sharding key values**, or they can be constructed using **ranges of sharding key values**.</span></span> 

### <a name="list-shard-maps"></a><span data-ttu-id="ea638-140">Lista shard maps</span><span class="sxs-lookup"><span data-stu-id="ea638-140">List shard maps</span></span>
<span data-ttu-id="ea638-141">**Szilánkok** tartalmazhat **shardlets** és shardlets tooshards hello leképezése shard térképre tartja fenn.</span><span class="sxs-lookup"><span data-stu-id="ea638-141">**Shards** contain **shardlets** and hello mapping of shardlets tooshards is maintained by a shard map.</span></span> <span data-ttu-id="ea638-142">A **lista shard térkép** hello egyedi értékek hello shardlets azonosítására és szilánkok szolgálhat hello adatbázisok közötti társítás.</span><span class="sxs-lookup"><span data-stu-id="ea638-142">A **list shard map** is an association between hello individual key values that identify hello shardlets and hello databases that serve as shards.</span></span>  <span data-ttu-id="ea638-143">**Hozzárendelések listában** egyértelmű és különböző értékeit leképezett toohello lehet ugyanazt az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="ea638-143">**List mappings** are explicit and different key values can be mapped toohello same database.</span></span> <span data-ttu-id="ea638-144">Például kulcs 1 tooDatabase A maps, és 3. és 6 mindkét kulcsértékei hivatkozást adatbázis a b kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ea638-144">For example, key 1 maps tooDatabase A, and key values 3 and 6 both reference Database B.</span></span>

| <span data-ttu-id="ea638-145">Kulcs</span><span class="sxs-lookup"><span data-stu-id="ea638-145">Key</span></span> | <span data-ttu-id="ea638-146">A shard helye</span><span class="sxs-lookup"><span data-stu-id="ea638-146">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="ea638-147">1</span><span class="sxs-lookup"><span data-stu-id="ea638-147">1</span></span> |<span data-ttu-id="ea638-148">Database_A</span><span class="sxs-lookup"><span data-stu-id="ea638-148">Database_A</span></span> |
| <span data-ttu-id="ea638-149">3</span><span class="sxs-lookup"><span data-stu-id="ea638-149">3</span></span> |<span data-ttu-id="ea638-150">Database_B</span><span class="sxs-lookup"><span data-stu-id="ea638-150">Database_B</span></span> |
| <span data-ttu-id="ea638-151">4</span><span class="sxs-lookup"><span data-stu-id="ea638-151">4</span></span> |<span data-ttu-id="ea638-152">Database_C</span><span class="sxs-lookup"><span data-stu-id="ea638-152">Database_C</span></span> |
| <span data-ttu-id="ea638-153">6</span><span class="sxs-lookup"><span data-stu-id="ea638-153">6</span></span> |<span data-ttu-id="ea638-154">Database_B</span><span class="sxs-lookup"><span data-stu-id="ea638-154">Database_B</span></span> |
| <span data-ttu-id="ea638-155">...</span><span class="sxs-lookup"><span data-stu-id="ea638-155">...</span></span> |<span data-ttu-id="ea638-156">...</span><span class="sxs-lookup"><span data-stu-id="ea638-156">...</span></span> |

### <a name="range-shard-maps"></a><span data-ttu-id="ea638-157">Tartomány shard maps</span><span class="sxs-lookup"><span data-stu-id="ea638-157">Range shard maps</span></span>
<span data-ttu-id="ea638-158">Az egy **tartomány shard térkép**, hello kulcs tartomány leírt két **[érték alacsony, magas érték)** ahol hello *alacsony értéket* hello minimális kulcs a hello tartományt és hello *Értékes* hello első érték nagyobb, mint hello tartományon.</span><span class="sxs-lookup"><span data-stu-id="ea638-158">In a **range shard map**, hello key range is described by a pair **[Low Value, High Value)** where hello *Low Value* is hello minimum key in hello range, and hello *High Value* is hello first value higher than hello range.</span></span> 

<span data-ttu-id="ea638-159">Például **[0, 100)** nagyobb vagy egyenlő 0 és 100-nál kisebb összes egész számokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ea638-159">For example, **[0, 100)** includes all integers greater than or equal 0 and less than 100.</span></span> <span data-ttu-id="ea638-160">Vegye figyelembe, hogy a több tartomány is pont toohello azonos adatbázisából, és a tartományok különálló támogatottak-e (pl. [100,200) és a [400,600) mindkét pont tooDatabase C az alábbi példa hello.)</span><span class="sxs-lookup"><span data-stu-id="ea638-160">Note that multiple ranges can point toohello same database, and disjoint ranges are supported (e.g., [100,200) and [400,600) both point tooDatabase C in hello example below.)</span></span>

| <span data-ttu-id="ea638-161">Kulcs</span><span class="sxs-lookup"><span data-stu-id="ea638-161">Key</span></span> | <span data-ttu-id="ea638-162">A shard helye</span><span class="sxs-lookup"><span data-stu-id="ea638-162">Shard Location</span></span> |
| --- | --- |
| <span data-ttu-id="ea638-163">[1,50)</span><span class="sxs-lookup"><span data-stu-id="ea638-163">[1,50)</span></span> |<span data-ttu-id="ea638-164">Database_A</span><span class="sxs-lookup"><span data-stu-id="ea638-164">Database_A</span></span> |
| <span data-ttu-id="ea638-165">[50,100)</span><span class="sxs-lookup"><span data-stu-id="ea638-165">[50,100)</span></span> |<span data-ttu-id="ea638-166">Database_B</span><span class="sxs-lookup"><span data-stu-id="ea638-166">Database_B</span></span> |
| <span data-ttu-id="ea638-167">[100,200)</span><span class="sxs-lookup"><span data-stu-id="ea638-167">[100,200)</span></span> |<span data-ttu-id="ea638-168">Database_C</span><span class="sxs-lookup"><span data-stu-id="ea638-168">Database_C</span></span> |
| <span data-ttu-id="ea638-169">[400,600)</span><span class="sxs-lookup"><span data-stu-id="ea638-169">[400,600)</span></span> |<span data-ttu-id="ea638-170">Database_C</span><span class="sxs-lookup"><span data-stu-id="ea638-170">Database_C</span></span> |
| <span data-ttu-id="ea638-171">...</span><span class="sxs-lookup"><span data-stu-id="ea638-171">...</span></span> |<span data-ttu-id="ea638-172">...</span><span class="sxs-lookup"><span data-stu-id="ea638-172">...</span></span> |

<span data-ttu-id="ea638-173">A fenti hello táblák egy általános példát egy **ShardMap** objektum.</span><span class="sxs-lookup"><span data-stu-id="ea638-173">Each of hello tables shown above is a conceptual example of a **ShardMap** object.</span></span> <span data-ttu-id="ea638-174">Minden egyes sorára egy adott egyszerűsített példát **PointMapping** (az hello lista shard leképezés) vagy **RangeMapping** (a tartomány shard térkép hello) objektum.</span><span class="sxs-lookup"><span data-stu-id="ea638-174">Each row is a simplified example of an individual **PointMapping** (for hello list shard map) or **RangeMapping** (for hello range shard map) object.</span></span>

## <a name="shard-map-manager"></a><span data-ttu-id="ea638-175">A shard térkép manager</span><span class="sxs-lookup"><span data-stu-id="ea638-175">Shard map manager</span></span>
<span data-ttu-id="ea638-176">Hello ügyfél könyvtárban hello shard térkép manager gyűjteménye shard leképezések.</span><span class="sxs-lookup"><span data-stu-id="ea638-176">In hello client library, hello shard map  manager is a collection of shard maps.</span></span> <span data-ttu-id="ea638-177">hello által kezelt egy **ShardMapManager** példány tartják három helyen:</span><span class="sxs-lookup"><span data-stu-id="ea638-177">hello data managed by a **ShardMapManager** instance is kept in three places:</span></span> 

1. <span data-ttu-id="ea638-178">**Globális Shard térkép (GSM)**: egy adatbázis tooserve hello tárháza összes shard leképezéseket és leképezéseket ad meg.</span><span class="sxs-lookup"><span data-stu-id="ea638-178">**Global Shard Map (GSM)**: You specify a database tooserve as hello repository for all of its shard maps and mappings.</span></span> <span data-ttu-id="ea638-179">Különleges táblák és tárolt eljárások automatikusan létrejönnek toomanage hello információkat.</span><span class="sxs-lookup"><span data-stu-id="ea638-179">Special tables and stored procedures are automatically created toomanage hello information.</span></span> <span data-ttu-id="ea638-180">Ez általában egy kis adatbázist, és könnyebb érhető el, és azt nem használható más hello alkalmazás igényeinek.</span><span class="sxs-lookup"><span data-stu-id="ea638-180">This is typically a small database and lightly accessed, and it should not be used for other needs of hello application.</span></span> <span data-ttu-id="ea638-181">hello táblák vannak különleges a séma nevű **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="ea638-181">hello tables are in a special schema named **__ShardManagement**.</span></span> 
2. <span data-ttu-id="ea638-182">**Helyi Shard térkép (LSM)**: minden adatbázis egy shard van toobe módosított toocontain több kis táblák és speciális tárolt eljárások, melyek és shard térkép információt adott toothat shard kezelése.</span><span class="sxs-lookup"><span data-stu-id="ea638-182">**Local Shard Map (LSM)**: Every database that you specify toobe a shard is modified toocontain several small tables and special stored procedures that contain and manage shard map information specific toothat shard.</span></span> <span data-ttu-id="ea638-183">Ezekkel az információkkal már redundáns és hello GSM hello adatait, és anélkül, hogy a terhelés a hello GSM; lehetővé teszi hello alkalmazás gyorsítótárazott toovalidate shard leképezési adatai hello alkalmazás hello LSM toodetermine használja, ha egy gyorsítótárazott leképezés továbbra is érvényes.</span><span class="sxs-lookup"><span data-stu-id="ea638-183">This information is redundant with hello information in hello GSM, and it allows hello application toovalidate cached shard map information without placing any load on hello GSM; hello application uses hello LSM toodetermine if a cached mapping is still valid.</span></span> <span data-ttu-id="ea638-184">hello táblázatot is hello séma vannak az egyes shard LSM megfelelő toohello **__ShardManagement**.</span><span class="sxs-lookup"><span data-stu-id="ea638-184">hello tables corresponding toohello LSM on each shard are also in hello schema **__ShardManagement**.</span></span>
3. <span data-ttu-id="ea638-185">**Alkalmazás-gyorsítótárának**: minden egyes alkalmazás példány elérése a **ShardMapManager** objektum a leképezéseket egy helyi memóriabeli gyorsítótárában megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="ea638-185">**Application cache**: Each application instance accessing a **ShardMapManager** object maintains a local in-memory cache of its mappings.</span></span> <span data-ttu-id="ea638-186">Útvonal-információkat, amelyek a legutóbb beolvasott tárol.</span><span class="sxs-lookup"><span data-stu-id="ea638-186">It stores routing information that has recently been retrieved.</span></span> 

## <a name="constructing-a-shardmapmanager"></a><span data-ttu-id="ea638-187">Egy ShardMapManager létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea638-187">Constructing a ShardMapManager</span></span>
<span data-ttu-id="ea638-188">A **ShardMapManager** objektum segítségével jön létre egy [gyári](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) mintát.</span><span class="sxs-lookup"><span data-stu-id="ea638-188">A **ShardMapManager** object is constructed using a [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) pattern.</span></span> <span data-ttu-id="ea638-189">Hello  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metódus hitelesítő adatait (beleértve a hello kiszolgáló és adatbázis nevét okozó hello GSM) formájában hello időt vesz igénybe. a  **ConnectionString** és egy példányát adja vissza egy **ShardMapManager**.</span><span class="sxs-lookup"><span data-stu-id="ea638-189">hello **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)** method takes credentials (including hello server name and database name holding hello GSM) in hello form of a **ConnectionString** and returns an instance of a **ShardMapManager**.</span></span>  

<span data-ttu-id="ea638-190">**Ne feledje:** hello **ShardMapManager** kell példányosítani csak egyszer app tartományonként hello inicializálási kód egy alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ea638-190">**Please Note:** hello **ShardMapManager** should be instantiated only once per app domain, within hello initialization code for an application.</span></span> <span data-ttu-id="ea638-191">Létrehozása a hello ShardMapManager további példányának egyazon appdomain eredményez nagyobb memória és CPU-felhasználás hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ea638-191">Creation of additional instances of ShardMapManager in hello same appdomain, will result in increased memory and CPU utilization of hello application.</span></span> <span data-ttu-id="ea638-192">A **ShardMapManager** shard maps tetszőleges számú tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ea638-192">A **ShardMapManager** can contain any number of shard maps.</span></span> <span data-ttu-id="ea638-193">Lehet, hogy elegendő-e számos alkalmazás egyetlen shard térképre, amíg nincsenek alkalommal, ha az adatbázisok más-más részhalmazához használt különböző séma vagy egyedi célokra; azokban az esetekben érdemes lehet több shard maps.</span><span class="sxs-lookup"><span data-stu-id="ea638-193">While a single shard map may be sufficient for many applications, there are times when different sets of databases are used for different schema or for unique purposes; in those cases multiple shard maps may be preferable.</span></span> 

<span data-ttu-id="ea638-194">Ebben a kódban, egy alkalmazás egy meglévő tooopen megpróbál **ShardMapManager** a hello [TryGetSqlShardMapManager metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea638-194">In this code, an application tries tooopen an existing **ShardMapManager** with hello [TryGetSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).</span></span>  <span data-ttu-id="ea638-195">Ha egy globális objektumokból **ShardMapManager** (GSM) létezhet hello adatbázis még nem, hello ügyféloldali kódtár létrehozásuk nincs hello segítségével [CreateSqlShardMapManager metódus](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea638-195">If objects representing a Global **ShardMapManager** (GSM) do not yet exist inside hello database, hello client library creates them there using hello [CreateSqlShardMapManager method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).</span></span>

    // Try tooget a reference toohello Shard Map Manager 
     // via hello Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create hello Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // hello connectionString contains server name, database name, and admin credentials 
        // for privileges on both hello GSM and hello shards themselves.
    } 

<span data-ttu-id="ea638-196">Alternatív megoldásként használhatja a Powershell toocreate egy új Shard térkép Manager.</span><span class="sxs-lookup"><span data-stu-id="ea638-196">As an alternative, you can use Powershell toocreate a new Shard Map Manager.</span></span> <span data-ttu-id="ea638-197">Példa: elérhető [Itt](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="ea638-197">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="get-a-rangeshardmap-or-listshardmap"></a><span data-ttu-id="ea638-198">Szerezze be a RangeShardMap vagy ListShardMap</span><span class="sxs-lookup"><span data-stu-id="ea638-198">Get a RangeShardMap or ListShardMap</span></span>
<span data-ttu-id="ea638-199">Miután létrehozta a shard térkép kezelő, kaphat a hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) vagy [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) hello segítségével [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), vagy hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metódust.</span><span class="sxs-lookup"><span data-stu-id="ea638-199">After creating a shard map manager, you can get hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) or [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) using hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), or hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) method.</span></span>

    /// <summary>
    /// Creates a new Range Shard Map with hello specified name, or gets hello Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try tooget a reference toohello Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // hello Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a><span data-ttu-id="ea638-200">A shard térkép felügyeleti hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="ea638-200">Shard map administration credentials</span></span>
<span data-ttu-id="ea638-201">Alkalmazások, amelyek felügyelheti és kezelheti a shard maps különböznek hello shard maps tooroute kapcsolatok használó.</span><span class="sxs-lookup"><span data-stu-id="ea638-201">Applications that administer and manipulate shard maps are different from those that use hello shard maps tooroute connections.</span></span> 

<span data-ttu-id="ea638-202">tooadminister shard leképezi (hozzáadása vagy módosítása a szilánkok, shard maps, shard hozzárendelések, stb.) példányosítania kell hello **ShardMapManager** használatával **hitelesítő adatokat, amelyek olvasási/írási jogosultsággal mindkét hello GSM adatbázison, és minden egyes adatbázis, amely egy shard funkcionál**.</span><span class="sxs-lookup"><span data-stu-id="ea638-202">tooadminister shard maps (add or change shards, shard maps, shard mappings, etc.) you must instantiate hello **ShardMapManager** using **credentials that have read/write privileges on both hello GSM database and on each database that serves as a shard**.</span></span> <span data-ttu-id="ea638-203">hello hitelesítő adatok lehetővé kell tennie a mindkét hello hello táblák írására GSM és LSM shard megfeleltetési adatokat a megadott, vagy megváltozott, valamint LSM-táblázatok létrehozása az új szilánkok esetében.</span><span class="sxs-lookup"><span data-stu-id="ea638-203">hello credentials must allow for writes against hello tables in both hello GSM and LSM as shard map information is entered or changed, as well as for creating LSM tables on new shards.</span></span>  

<span data-ttu-id="ea638-204">Lásd: [a hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="ea638-204">See [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

### <a name="only-metadata-affected"></a><span data-ttu-id="ea638-205">Csak az érintett metaadatok</span><span class="sxs-lookup"><span data-stu-id="ea638-205">Only metadata affected</span></span>
<span data-ttu-id="ea638-206">Feltöltése vagy módosítása hello használt módszerek **ShardMapManager** adatok nem módosítja a hello szilánkok maguk tárolt hello felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="ea638-206">Methods used for populating or changing hello **ShardMapManager** data do not alter hello user data stored in hello shards themselves.</span></span> <span data-ttu-id="ea638-207">Többek között például módszerek **CreateShard**, **DeleteShard**, **UpdateMapping**stb hello shard ábrázolási metaadatainak csak hatással.</span><span class="sxs-lookup"><span data-stu-id="ea638-207">For example, methods such as **CreateShard**, **DeleteShard**, **UpdateMapping**, etc. affect hello shard map metadata only.</span></span> <span data-ttu-id="ea638-208">Ezek nem távolítható el, adja hozzá vagy alter hello szilánkok szereplő felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="ea638-208">They do not remove, add, or alter user data contained in hello shards.</span></span> <span data-ttu-id="ea638-209">Ehelyett az alábbi módszerek egyike használt tervezett toobe külön műveletek együtt toocreate végrehajtani, vagy távolítsa el a tényleges adatbázisok, vagy ez a lépés sor egy shard tooanother toorebalance innen a szilánkos környezetekben.</span><span class="sxs-lookup"><span data-stu-id="ea638-209">Instead, these methods are designed toobe used in conjunction with separate operations you perform toocreate or remove actual databases, or that move rows from one shard tooanother toorebalance a sharded environment.</span></span>  <span data-ttu-id="ea638-210">(hello **vegyes egyesítéses** rugalmas adatbáziseszközöket eszköz lehetővé teszi, hogy ezek API-k között szilánkok adatok tényleges mozgatását koordinálása együtt használja.) Lásd: [méretezési hello rugalmas adatbázis vegyes egyesítéses eszközzel](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="ea638-210">(hello **split-merge** tool included with elastic database tools makes use of these APIs along with orchestrating actual data movement between shards.) See [Scaling using hello Elastic Database split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md).</span></span>

## <a name="populating-a-shard-map-example"></a><span data-ttu-id="ea638-211">A szilánkok térkép példa feltöltése</span><span class="sxs-lookup"><span data-stu-id="ea638-211">Populating a shard map example</span></span>
<span data-ttu-id="ea638-212">Egy parancssorozat-példa műveletek toopopulate egy adott shard térkép alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="ea638-212">An example sequence of operations toopopulate a specific shard map is shown below.</span></span> <span data-ttu-id="ea638-213">hello kód végrehajtja ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ea638-213">hello code performs these steps:</span></span> 

1. <span data-ttu-id="ea638-214">Új shard leképezés egy shard térkép manager belül jön létre.</span><span class="sxs-lookup"><span data-stu-id="ea638-214">A new shard map is created within a shard map manager.</span></span> 
2. <span data-ttu-id="ea638-215">két különböző szilánkok hello metaadatai toohello shard térkép kerül.</span><span class="sxs-lookup"><span data-stu-id="ea638-215">hello metadata for two different shards is added toohello shard map.</span></span> 
3. <span data-ttu-id="ea638-216">Kulcs tartomány hozzárendelések számos ad hozzá, és hello teljes tartalma hello shard térkép jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ea638-216">A variety of key range mappings are added, and hello overall contents of hello shard map are displayed.</span></span> 

<span data-ttu-id="ea638-217">hello kód írása, hogy hello metódus is futtatható, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="ea638-217">hello code is written so that hello method can be rerun if an error occurs.</span></span> <span data-ttu-id="ea638-218">Minden egyes kérelem teszteli, hogy a shard vagy a hozzárendelés már létezik, mielőtt megpróbálná toocreate azt.</span><span class="sxs-lookup"><span data-stu-id="ea638-218">Each request tests whether a shard or mapping already exists, before attempting toocreate it.</span></span> <span data-ttu-id="ea638-219">hello kód azt feltételezi, hogy az adatbázisok nevű **sample_shard_0**, **sample_shard_1** és **sample_shard_2** létre lett hozva a hello server karakterlánc hivatkozik**shardServer**.</span><span class="sxs-lookup"><span data-stu-id="ea638-219">hello code assumes that databases named **sample_shard_0**, **sample_shard_1** and **sample_shard_2** have already been created in hello server referenced by string **shardServer**.</span></span> 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List hello shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

<span data-ttu-id="ea638-220">Mivel helyett használhatja a PowerShell parancsfájlok tooachieve hello azonos vezethet.</span><span class="sxs-lookup"><span data-stu-id="ea638-220">As an alternative you can use PowerShell scripts tooachieve hello same result.</span></span> <span data-ttu-id="ea638-221">Hello minta PowerShell-példák némelyike elérhető [Itt](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="ea638-221">Some of hello sample PowerShell examples are available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>     

<span data-ttu-id="ea638-222">Után shard maps vannak-e töltve, az adatok alkalmazásokat hozhatók létre, vagy a hello Maps Közösséggel toowork leírtakon.</span><span class="sxs-lookup"><span data-stu-id="ea638-222">Once shard maps have been populated, data access applications can be created or adapted toowork with hello maps.</span></span> <span data-ttu-id="ea638-223">Feltöltése vagy módosítása hello maps kell nem fordulhat elő, amíg újra nem **térkép elrendezés** toochange kell.</span><span class="sxs-lookup"><span data-stu-id="ea638-223">Populating or manipulating hello maps need not occur again until **map layout** needs toochange.</span></span>  

## <a name="data-dependent-routing"></a><span data-ttu-id="ea638-224">Adatfüggő útválasztás</span><span class="sxs-lookup"><span data-stu-id="ea638-224">Data dependent routing</span></span>
<span data-ttu-id="ea638-225">hello shard térkép manager legtöbb használandó adatbázis-kapcsolatok tooperform hello alkalmazás-specifikus adatok műveletek igénylő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="ea638-225">hello shard map manager will be most used in applications that require database connections tooperform hello app-specific data operations.</span></span> <span data-ttu-id="ea638-226">Ezeket a kapcsolatokat hello megfelelő adatbázis társítani kell.</span><span class="sxs-lookup"><span data-stu-id="ea638-226">Those connections must be associated with hello correct database.</span></span> <span data-ttu-id="ea638-227">Ez más néven **adatok függő útválasztási**.</span><span class="sxs-lookup"><span data-stu-id="ea638-227">This is known as **Data Dependent Routing**.</span></span> <span data-ttu-id="ea638-228">Az alkalmazás hozható létre a shard leképezési manager objektum, amely csak olvasási hozzáféréssel rendelkezik a hello GSM adatbázis hitelesítő adataival hello gyárból származó.</span><span class="sxs-lookup"><span data-stu-id="ea638-228">For these applications, instantiate a shard map manager object from hello factory using credentials that have read-only access on hello GSM database.</span></span> <span data-ttu-id="ea638-229">Egyes kérelmeket a későbbi kapcsolatok toohello megfelelő shard adatbázis csatlakozáshoz szükséges hitelesítő adatokat fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="ea638-229">Individual requests for later connections supply credentials necessary for connecting toohello appropriate shard database.</span></span>

<span data-ttu-id="ea638-230">Vegye figyelembe, hogy ezek az alkalmazások (használatával **ShardMapManager** olvasási jogosultságokkal megnyitni) nem lehet módosítani toohello leképezéseket és leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="ea638-230">Note that these applications (using **ShardMapManager** opened with read-only credentials) cannot make changes toohello maps or mappings.</span></span> <span data-ttu-id="ea638-231">Hozzon létre felügyeleti-specifikus alkalmazások vagy a PowerShell-parancsfájlok, amelyek magasabb jogosultsági szintű hitelesítő adatokat a fentiekben taglaltak ezeket az igényeket.</span><span class="sxs-lookup"><span data-stu-id="ea638-231">For those needs, create administrative-specific applications or PowerShell scripts that supply higher-privileged credentials as discussed earlier.</span></span> <span data-ttu-id="ea638-232">Lásd: [a hitelesítő adatokat használni tooaccess hello Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="ea638-232">See [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span>

<span data-ttu-id="ea638-233">További részletekért lásd: [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="ea638-233">For more details, see [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> 

## <a name="modifying-a-shard-map"></a><span data-ttu-id="ea638-234">A szilánkok térkép módosítása</span><span class="sxs-lookup"><span data-stu-id="ea638-234">Modifying a shard map</span></span>
<span data-ttu-id="ea638-235">A shard térképre különböző módokon lehet megváltoztatni.</span><span class="sxs-lookup"><span data-stu-id="ea638-235">A shard map can be changed in different ways.</span></span> <span data-ttu-id="ea638-236">A következő módszerek hello mindegyikét: hello szilánkok és a hozzájuk tartozó leképezések hello metaadatok módosításához, de hello szilánkok adatainak fizikailag nem módosíthatják, és nem tegye azokat létrehozása vagy törlése hello tényleges adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="ea638-236">All of hello following methods modify hello metadata describing hello shards and their mappings, but they do not physically modify data within hello shards, nor do they create or delete hello actual databases.</span></span>  <span data-ttu-id="ea638-237">Az alábbiakban hello shard térképen hello műveleteket esetleg toobe koordinált, amely fizikailag helyezi át adatokat, vagy hozzáadására és eltávolítására szolgáló szilánkok adatbázisok, amelyek a felügyeleti műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="ea638-237">Some of hello operations on hello shard map described below may need toobe coordinated with administrative actions that physically move data or that add and remove databases serving as shards.</span></span>

<span data-ttu-id="ea638-238">Ezek a módszerek működnek együtt a módosítását a rendelkezésre álló hello építőelemeiként hello a szilánkos adatbázis környezetében adatok teljes terjesztési.</span><span class="sxs-lookup"><span data-stu-id="ea638-238">These methods work together as hello building blocks available for modifying hello overall distribution of data in your sharded database environment.</span></span>  

* <span data-ttu-id="ea638-239">tooadd vagy az eltávolítási szilánkok: használja  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  és  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  a hello [Shardmap osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea638-239">tooadd or remove shards: use **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)** and **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)** of hello [Shardmap class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx).</span></span> 
  
    <span data-ttu-id="ea638-240">hello kiszolgáló és az adatbázis hello cél shard képviselő már léteznie kell a műveletek tooexecute.</span><span class="sxs-lookup"><span data-stu-id="ea638-240">hello server and database representing hello target shard must already exist for these operations tooexecute.</span></span> <span data-ttu-id="ea638-241">Ezek a módszerek nem rendelkezik gyakorolt hatás hello adatbázisok, csak a metaadatok hello shard leképezés.</span><span class="sxs-lookup"><span data-stu-id="ea638-241">These methods do not have any impact on hello databases themselves, only on metadata in hello shard map.</span></span>
* <span data-ttu-id="ea638-242">pontok toocreate, vagy távolítsa el vagy tartományokat toohello szilánkok leképezése: használja  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  a hello [RangeShardMapping osztály](https://msdn.microsoft.com/library/azure/dn807318.aspx), és  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  a hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span><span class="sxs-lookup"><span data-stu-id="ea638-242">toocreate or remove points or ranges that are mapped toohello shards: use **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**, **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)** of hello [RangeShardMapping class](https://msdn.microsoft.com/library/azure/dn807318.aspx), and **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)** of hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)</span></span>
  
    <span data-ttu-id="ea638-243">Számos különböző pontok vagy tartományok lehet csatlakoztatott toohello ugyanazt a shard.</span><span class="sxs-lookup"><span data-stu-id="ea638-243">Many different points or ranges can be mapped toohello same shard.</span></span> <span data-ttu-id="ea638-244">Ezek a módszerek csak hatással vannak a metaadat - adatokat, amelyek esetleg már szerepel a szilánkok nem befolyásolják.</span><span class="sxs-lookup"><span data-stu-id="ea638-244">These methods only affect metadata - they do not affect any data that may already be present in shards.</span></span> <span data-ttu-id="ea638-245">Ha adatokat kell-e eltávolítva a rendelés toobe konzisztens hello adatbázisból toobe **DeleteMapping** műveletek, szüksége lesz tooperform ezeket a műveleteket külön-külön, de ezekkel a módszerekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="ea638-245">If data needs toobe removed from hello database in order toobe consistent with **DeleteMapping** operations, you will need tooperform those operations separately but in conjunction with using these methods.</span></span>  
* <span data-ttu-id="ea638-246">létező tartományok toosplit kettő vagy egyesítés szomszédos tartományok valamelyikébe: használja  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  és  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ea638-246">toosplit existing ranges into two, or merge adjacent ranges into one: use **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)** and **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.</span></span>  
  
    <span data-ttu-id="ea638-247">Vegye figyelembe, hogy ossza fel, és egyesíti az műveletek **ne változtassa meg hello shard toowhich kulcs képezi le**.</span><span class="sxs-lookup"><span data-stu-id="ea638-247">Note that split and merge operations **do not change hello shard toowhich key values are mapped**.</span></span> <span data-ttu-id="ea638-248">Valószínűségét egy meglévő tartományt bontja két részből áll, de hagyja mindkét csatlakoztatott toohello, ugyanazt a shard.</span><span class="sxs-lookup"><span data-stu-id="ea638-248">A split breaks an existing range into two parts, but leaves both as mapped toohello same shard.</span></span> <span data-ttu-id="ea638-249">Két szomszédos tartományokat, amelyek már csatlakoztatott toohello működik egyesítésével ugyanazt a shard, egyesítése őket egy egyetlen tartományba.</span><span class="sxs-lookup"><span data-stu-id="ea638-249">A merge operates on two adjacent ranges that are already mapped toohello same shard, coalescing them into a single range.</span></span>  <span data-ttu-id="ea638-250">hello pontok vagy közötti szilánkok maguk tartományok kell használatával koordinált toobe **UpdateMapping** adatok tényleges mozgatását együtt.</span><span class="sxs-lookup"><span data-stu-id="ea638-250">hello movement of points or ranges themselves between shards needs toobe coordinated by using **UpdateMapping** in conjunction with actual data movement.</span></span>  <span data-ttu-id="ea638-251">Használhatja a hello **vegyes/Egyesítés** rugalmas adatbázis részét képező szolgáltatás eszközei toocoordinate shard térkép módosításokat adatmozgás, amikor szükség van a mozgás.</span><span class="sxs-lookup"><span data-stu-id="ea638-251">You can use hello **Split/Merge** service that is part of elastic database tools toocoordinate shard map changes with data movement, when movement is needed.</span></span> 
* <span data-ttu-id="ea638-252">toore-társítási (vagy áthelyezése) egyes pontok vagy tartományok toodifferent szilánkok: használja  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ea638-252">toore-map (or move) individual points or ranges toodifferent shards: use **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.</span></span>  
  
    <span data-ttu-id="ea638-253">Mivel adatok esetleg egy shard tooanother rendelés toobe konzisztensek legyenek az áthelyezett toobe **UpdateMapping** műveletek, szüksége lesz tooperform, hogy a mozgás külön-külön, de ezekkel a módszerekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="ea638-253">Since data may need toobe moved from one shard tooanother in order toobe consistent with **UpdateMapping** operations, you will need tooperform that movement separately but in conjunction with using these methods.</span></span>
* <span data-ttu-id="ea638-254">online és offline tootake leképezéseket: használja  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  és  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  toocontrol hello online állapotát egy leképezés.</span><span class="sxs-lookup"><span data-stu-id="ea638-254">tootake mappings online and offline: use **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)** and **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)** toocontrol hello online state of a mapping.</span></span> 
  
    <span data-ttu-id="ea638-255">A shard hozzárendelések bizonyos műveletek csak vannak engedélyezve, ha a leképezés nem "offline" állapotú, beleértve a **UpdateMapping** és **DeleteMapping**.</span><span class="sxs-lookup"><span data-stu-id="ea638-255">Certain operations on shard mappings are only allowed when a mapping is in an “offline” state, including **UpdateMapping** and **DeleteMapping**.</span></span> <span data-ttu-id="ea638-256">Ha a leképezés offline állapotban, a kulcs szerepel a leképezéseket alapuló adatok függő kérelem hibát adnak vissza.</span><span class="sxs-lookup"><span data-stu-id="ea638-256">When a mapping is offline, a data-dependent request based on a key included in that mapping will return an error.</span></span> <span data-ttu-id="ea638-257">Emellett egy tartomány első offline állapotba kerül, ha minden kapcsolatok hatással toohello shard vannak automatikusan levágni tooprevent inkonzisztens vagy hiányos eredményeket lekérdezésekhez az módosítás alatt tartományok szemben.</span><span class="sxs-lookup"><span data-stu-id="ea638-257">In addition, when a range is first taken offline, all connections toohello affected shard are automatically killed in order tooprevent inconsistent or incomplete results for queries directed against ranges being changed.</span></span> 

<span data-ttu-id="ea638-258">Hozzárendelések nem módosítható objektumokat a .net.</span><span class="sxs-lookup"><span data-stu-id="ea638-258">Mappings are immutable objects in .Net.</span></span>  <span data-ttu-id="ea638-259">Az összes hello módszerek fenti leképezések módosításához bármely hivatkozások toothem a kódban is érvénytelenné válnak.</span><span class="sxs-lookup"><span data-stu-id="ea638-259">All of hello methods above that change mappings also invalidate any references toothem in your code.</span></span> <span data-ttu-id="ea638-260">toomake állapotváltozáshoz a leképezést, minden módosítása, leképezés hello módszerek műveletek könnyebb tooperform sorozatát adja vissza egy új informatikai leképezési hivatkozást, így műveletek elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ea638-260">toomake it easier tooperform sequences of operations that change a mapping’s state, all of hello methods that change a mapping return a new mapping reference, so operations can be chained.</span></span> <span data-ttu-id="ea638-261">Például egy meglévő leképezés az shardmap sm 25 hello kulcsot tartalmazó toodelete, hajthat végre a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ea638-261">For example, toodelete an existing mapping in shardmap sm that contains hello key 25, you can execute hello following:</span></span> 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a><span data-ttu-id="ea638-262">A szilánkok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ea638-262">Adding a shard</span></span>
<span data-ttu-id="ea638-263">Alkalmazások gyakran kell toosimply új szilánkok toohandle adatok hozzáadása új kulcsokat vagy kulcstartományokkal, várt shard térképre már létezik.</span><span class="sxs-lookup"><span data-stu-id="ea638-263">Applications often need toosimply add new shards toohandle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="ea638-264">Például a bérlő-azonosító szerint szilánkos alkalmazás egy új shard tooprovision szükséges egy új bérlőt, vagy adatok szilánkos havi esetleg egy új shard kiépítése előtt minden új hónap elején hello.</span><span class="sxs-lookup"><span data-stu-id="ea638-264">For example, an application sharded by Tenant ID may need tooprovision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before hello start of each new month.</span></span> 

<span data-ttu-id="ea638-265">Ha hello új értékek tartománya nem már része egy meglévő hozzárendelést és nélküli adatátvitelt jelölik a nagyon egyszerű tooadd hello új shard szükség, társítson hello kulcsot vagy tartomány toothat szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="ea638-265">If hello new range of key values is not already part of an existing mapping and no data movement is necessary, it is very simple tooadd hello new shard and associate hello new key or range toothat shard.</span></span> <span data-ttu-id="ea638-266">Új szilánkok hozzáadásával kapcsolatos részletekért lásd: [hozzáadása egy új shard](sql-database-elastic-scale-add-a-shard.md).</span><span class="sxs-lookup"><span data-stu-id="ea638-266">For details on adding new shards, see [Adding a new shard](sql-database-elastic-scale-add-a-shard.md).</span></span>

<span data-ttu-id="ea638-267">Adatátvitel igénylő forgatókönyvek esetén azonban hello vegyes egyesítéses eszköze szükséges tooorchestrate hello adatátvitelt jelölik a szilánkok hello szükséges shard leképezési frissítések együtt között.</span><span class="sxs-lookup"><span data-stu-id="ea638-267">For scenarios that require data movement, however, hello split-merge tool is needed tooorchestrate hello data movement between shards in combination with hello necessary shard map updates.</span></span> <span data-ttu-id="ea638-268">Információk hello vegyes egyesítéses yool, lásd: [vegyes egyesítéses áttekintése](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="ea638-268">For details on using hello split-merge yool, see [Overview of split-merge](sql-database-elastic-scale-overview-split-and-merge.md)</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
