---
title: "aaaMigrate meglévő adatbázisok tooscale kibővített |} Microsoft Docs"
description: "Hozzon létre egy shard térkép manager szilánkos adatbázisok toouse rugalmas adatbáziseszközöket átalakítása"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: fa2c9e3699f30667cf547d1faadf4504609199be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-existing-databases-tooscale-out"></a><span data-ttu-id="4ca1c-103">Telepítse át a meglévő adatbázisok tooscale kimenő</span><span class="sxs-lookup"><span data-stu-id="4ca1c-103">Migrate existing databases tooscale-out</span></span>
<span data-ttu-id="4ca1c-104">Könnyedén felügyelheti a meglévő szilánkos kiterjesztett adatbázisok Azure SQL Database-adatbázis eszközökkel (például hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md)).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-104">Easily manage your existing scaled-out sharded databases using Azure SQL Database database tools (such as hello [Elastic Database client library](sql-database-elastic-database-client-library.md)).</span></span> <span data-ttu-id="4ca1c-105">Először alakítsa át egy meglévő adatbázisok toouse hello-készletből [shard térkép manager](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-105">You must first convert an existing set of databases toouse hello [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="4ca1c-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4ca1c-106">Overview</span></span>
<span data-ttu-id="4ca1c-107">meglévő szilánkos adatbázis toomigrate:</span><span class="sxs-lookup"><span data-stu-id="4ca1c-107">toomigrate an existing sharded database:</span></span> 

1. <span data-ttu-id="4ca1c-108">Készítse elő a hello [shard manager adatbázist](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-108">Prepare hello [shard map manager database](sql-database-elastic-scale-shard-map-management.md).</span></span>
2. <span data-ttu-id="4ca1c-109">Hello shard térkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-109">Create hello shard map.</span></span>
3. <span data-ttu-id="4ca1c-110">Készítse elő a hello egyedi szilánkok.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-110">Prepare hello individual shards.</span></span>  
4. <span data-ttu-id="4ca1c-111">Hozzárendelések toohello shard-társítás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-111">Add mappings toohello shard map.</span></span>

<span data-ttu-id="4ca1c-112">Ezek a technológiák vagy hello valósítható [.NET-keretrendszer ügyféloldali kódtár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), vagy a PowerShell-parancsfájlok hello a következő címen található [Azure SQL Database - rugalmas adatbázis eszközök parancsfájlok](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-112">These techniques can be implemented using either hello [.NET Framework client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), or hello PowerShell scripts found at [Azure SQL DB - Elastic Database tools scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span> <span data-ttu-id="4ca1c-113">Itt hello példák hello PowerShell-parancsfájlok használata.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-113">hello examples here use hello PowerShell scripts.</span></span>

<span data-ttu-id="4ca1c-114">Hello ShardMapManager kapcsolatos további információkért lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-114">For more information about hello ShardMapManager, see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="4ca1c-115">Hello rugalmas adatbáziseszközöket áttekintését lásd: [rugalmas adatbázis-szolgáltatások áttekintése](sql-database-elastic-scale-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-115">For an overview of hello elastic database tools, see [Elastic Database features overview](sql-database-elastic-scale-introduction.md).</span></span>

## <a name="prepare-hello-shard-map-manager-database"></a><span data-ttu-id="4ca1c-116">Hello shard térkép manager-adatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-116">Prepare hello shard map manager database</span></span>
<span data-ttu-id="4ca1c-117">hello shard térkép manager különleges adatbázis hello adatok toomanage kiterjesztett adatbázisokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-117">hello shard map manager is a special database that contains hello data toomanage scaled-out databases.</span></span> <span data-ttu-id="4ca1c-118">Használjon egy meglévő adatbázist, vagy hozzon létre egy új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-118">You can use an existing database, or create a new database.</span></span> <span data-ttu-id="4ca1c-119">Vegye figyelembe, hogy egy adatbázis forráscsoportként shard térkép manager hello nem lehet azonos adatbázis, a szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-119">Note that a database acting as shard map manager should not be hello same database as a shard.</span></span> <span data-ttu-id="4ca1c-120">Ne feledje, hogy hello PowerShell-parancsfájl nem hoz létre hello adatbázis meg.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-120">Also note that hello PowerShell script does not create hello database for you.</span></span> 

## <a name="step-1-create-a-shard-map-manager"></a><span data-ttu-id="4ca1c-121">1. lépés: hozzon létre egy shard térkép manager</span><span class="sxs-lookup"><span data-stu-id="4ca1c-121">Step 1: create a shard map manager</span></span>
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are hello server name and database name 
    # for hello new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="tooretrieve-hello-shard-map-manager"></a><span data-ttu-id="4ca1c-122">tooretrieve hello shard térkép manager</span><span class="sxs-lookup"><span data-stu-id="4ca1c-122">tooretrieve hello shard map manager</span></span>
<span data-ttu-id="4ca1c-123">A létrehozás után hello shard térkép manager ezzel a parancsmaggal kérheti le.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-123">After creation, you can retrieve hello shard map manager with this cmdlet.</span></span> <span data-ttu-id="4ca1c-124">Ez a lépés akkor szükséges, minden alkalommal, amikor toouse hello ShardMapManager objektumot kell.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-124">This step is needed every time you need toouse hello ShardMapManager object.</span></span>

    # Try tooget a reference toohello Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-hello-shard-map"></a><span data-ttu-id="4ca1c-125">2. lépés: hello shard térkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-125">Step 2: create hello shard map</span></span>
<span data-ttu-id="4ca1c-126">Választania kell a shard térkép toocreate hello típusú.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-126">You must select hello type of shard map toocreate.</span></span> <span data-ttu-id="4ca1c-127">hello választáshoz hello adatbázis-architektúra:</span><span class="sxs-lookup"><span data-stu-id="4ca1c-127">hello choice depends on hello database architecture:</span></span> 

1. <span data-ttu-id="4ca1c-128">Adatbázisonként egybérlős (feltételei, lásd: hello [szószedet](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="4ca1c-128">Single tenant per database (For terms, see hello [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
2. <span data-ttu-id="4ca1c-129">Több bérlő adatbázisonként (két típus):</span><span class="sxs-lookup"><span data-stu-id="4ca1c-129">Multiple tenants per database (two types):</span></span>
   1. <span data-ttu-id="4ca1c-130">Lista leképezése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-130">List mapping</span></span>
   2. <span data-ttu-id="4ca1c-131">Tartomány leképezése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-131">Range mapping</span></span>

<span data-ttu-id="4ca1c-132">Single-bérlő modell, hozzon létre egy **lista leképezési** shard leképezés.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-132">For a single-tenant model, create a **list mapping** shard map.</span></span> <span data-ttu-id="4ca1c-133">hello single-bérlős modell bérlőnként egy adatbázis rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-133">hello single-tenant model assigns one database per tenant.</span></span> <span data-ttu-id="4ca1c-134">Ez megegyezik egy hatékony modell Szolgáltatottszoftver-fejlesztőknek egyszerűbbé teszi a felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-134">This is an effective model for SaaS developers as it simplifies management.</span></span>

![Lista leképezése][1]

<span data-ttu-id="4ca1c-136">hello több-bérlős modell rendeli hozzá több bérlő tooa egyetlen adatbázis (és a bérlő csoportok szét több adatbázis).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-136">hello multi-tenant model assigns several tenants tooa single database (and you can distribute groups of tenants across multiple databases).</span></span> <span data-ttu-id="4ca1c-137">Ezt a modellt használja, ha várhatóan mindegyik bérlő toohave kis adatokat.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-137">Use this model when you expect each tenant toohave small data needs.</span></span> <span data-ttu-id="4ca1c-138">Ez a modell azt rendelje hozzá a bérlők tooa adatbázis használatával széles **tartomány leképezése**.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-138">In this model, we assign a range of tenants tooa database using **range mapping**.</span></span> 

![Tartomány leképezése][2]

<span data-ttu-id="4ca1c-140">Megvalósíthat egy adatbázis több-bérlős modell használatával vagy egy *lista leképezési* tooassign egy adatbázis több bérlő tooa.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-140">Or you can implement a multi-tenant database model using a *list mapping* tooassign multiple tenants tooa single database.</span></span> <span data-ttu-id="4ca1c-141">Például D1 bérlőazonosító 1 és 5 használt toostore információt, és DB2 eltárolja 7 bérlői és bérlői 10.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-141">For example, DB1 is used toostore information about tenant id 1 and 5, and DB2 stores data for tenant 7 and tenant 10.</span></span> 

![Az egyetlen DB Muliple bérlők][3] 

<span data-ttu-id="4ca1c-143">**A kiválasztott alapján, válassza ki az alábbi lehetőségek egyikét:**</span><span class="sxs-lookup"><span data-stu-id="4ca1c-143">**Based on your choice, choose one of these options:**</span></span>

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a><span data-ttu-id="4ca1c-144">1. lehetőség: a lista hozzárendelés shard térkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-144">Option 1: create a shard map for a list mapping</span></span>
<span data-ttu-id="4ca1c-145">A hello ShardMapManager objektummal shard térkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-145">Create a shard map using hello ShardMapManager object.</span></span> 

    # $ShardMapManager is hello shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a><span data-ttu-id="4ca1c-146">2. lehetőség: a tartomány hozzárendelés shard térkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-146">Option 2: create a shard map for a range mapping</span></span>
<span data-ttu-id="4ca1c-147">Vegye figyelembe, hogy tooutilize ebben a leképezési mintában, bérlői azonosító értékekkel kell toobe folyamatos tartományokat, és elfogadható toohave résnek hello tartományok átugrásával egyszerűen hello tartomány hello adatbázisok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-147">Note that tooutilize this mapping pattern, tenant id values needs toobe continuous ranges, and it is acceptable toohave gap in hello ranges by simply skipping hello range when creating hello databases.</span></span>

    # $ShardMapManager is hello shard map manager object 
    # 'RangeShardMap' is hello unique identifier for hello range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a><span data-ttu-id="4ca1c-148">3. lehetőség: Lista leképezése egy önálló adatbázis</span><span class="sxs-lookup"><span data-stu-id="4ca1c-148">Option 3: List mappings on a single database</span></span>
<span data-ttu-id="4ca1c-149">A lista leképezés létrehozása is ebben a mintában beállítása igényel, ahogy az 1. lehetőség 2. lépést.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-149">Setting up this pattern also requires creation of a list map as shown in step 2, option 1.</span></span>

## <a name="step-3-prepare-individual-shards"></a><span data-ttu-id="4ca1c-150">3. lépés: Felkészülés az egyes szilánkok</span><span class="sxs-lookup"><span data-stu-id="4ca1c-150">Step 3: Prepare individual shards</span></span>
<span data-ttu-id="4ca1c-151">Minden egyes shard (adatbázis) toohello shard térkép-kezelő hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-151">Add each shard (database) toohello shard map manager.</span></span> <span data-ttu-id="4ca1c-152">Ez felkészíti hello egyedi adatbázisok identitásleképezési információk tárolására.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-152">This prepares hello individual databases for storing mapping information.</span></span> <span data-ttu-id="4ca1c-153">Ez a módszer minden shard hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-153">Execute this method on each shard.</span></span>

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # hello $ShardMap is hello shard map created in step 2.


## <a name="step-4-add-mappings"></a><span data-ttu-id="4ca1c-154">4. lépés: Hozzárendelések hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4ca1c-154">Step 4: Add mappings</span></span>
<span data-ttu-id="4ca1c-155">hozzárendelések hello hozzáadása a shard térkép létrehozott hello típusú függ.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-155">hello addition of mappings depends on hello kind of shard map you created.</span></span> <span data-ttu-id="4ca1c-156">Ha létrehozott egy lista térkép, hozzá kell adnia hozzárendelések listáját.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-156">If you created a list map, you add list mappings.</span></span> <span data-ttu-id="4ca1c-157">Ha létrehozott egy tartomány-társítást, hozzá kell adnia tartomány leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-157">If you created a range map, you add range mappings.</span></span>

### <a name="option-1-map-hello-data-for-a-list-mapping"></a><span data-ttu-id="4ca1c-158">1. lehetőség: a lista hozzárendelés hello adatok leképezése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-158">Option 1: map hello data for a list mapping</span></span>
<span data-ttu-id="4ca1c-159">Hello adatok leképezése egy lista leképezése hozzáadásával az egyes bérlők számára.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-159">Map hello data by adding a list mapping for each tenant.</span></span>  

    # Create hello mappings and associate it with hello new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-hello-data-for-a-range-mapping"></a><span data-ttu-id="4ca1c-160">2. lehetőség: a tartomány hozzárendelés hello adatok leképezése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-160">Option 2: map hello data for a range mapping</span></span>
<span data-ttu-id="4ca1c-161">Minden hello bérlői azonosító tartomány - adatbázis társítások leképezéseit hello tartomány hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="4ca1c-161">Add hello range mappings for all hello tenant id range - database associations:</span></span>

    # Create hello mappings and associate it with hello new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-hello-data-for-multiple-tenants-on-a-single-database"></a><span data-ttu-id="4ca1c-162">4. lépés: 3. lehetőség: egy adatbázist a több bérlő hello adatok leképezése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-162">Step 4 option 3: map hello data for multiple tenants on a single database</span></span>
<span data-ttu-id="4ca1c-163">Az egyes bérlők, futtassa az Add-ListMapping hello (1, lehetőséget fent).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-163">For each tenant, run hello Add-ListMapping (option 1, above).</span></span> 

## <a name="checking-hello-mappings"></a><span data-ttu-id="4ca1c-164">Hello hozzárendelések ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4ca1c-164">Checking hello mappings</span></span>
<span data-ttu-id="4ca1c-165">Hello meglévő szilánkok és a hozzájuk társított runbookokat hello hozzárendelések kapcsolatos információk lekérdezhetők, következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="4ca1c-165">Information about hello existing shards and hello mappings associated with them can be queried using following commands:</span></span>  

    # List hello shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a><span data-ttu-id="4ca1c-166">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4ca1c-166">Summary</span></span>
<span data-ttu-id="4ca1c-167">Hello telepítő befejezése után megkezdheti a toouse hello Elastic Database ügyféloldali kódtárára.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-167">Once you have completed hello setup, you can begin toouse hello Elastic Database client library.</span></span> <span data-ttu-id="4ca1c-168">Is [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) és [több shard lekérdezés](sql-database-elastic-scale-multishard-querying.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-168">You can also use [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and [multi-shard query](sql-database-elastic-scale-multishard-querying.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4ca1c-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ca1c-169">Next steps</span></span>
<span data-ttu-id="4ca1c-170">PowerShell-parancsfájlok hello az beszerzése [Azure SQL DB-rugalmas adatbázis eszközök sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-170">Get hello PowerShell scripts from [Azure SQL DB-Elastic Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

<span data-ttu-id="4ca1c-171">hello eszközök megtalálhatók a Githubon: [Azure/rugalmas-db-eszközök](https://github.com/Azure/elastic-db-tools).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-171">hello tools are also on GitHub: [Azure/elastic-db-tools](https://github.com/Azure/elastic-db-tools).</span></span>

<span data-ttu-id="4ca1c-172">Hello vegyes egyesítéses eszköz toomove adatok tooor egy több-bérlős modell tooa egybérlős modellből használja.</span><span class="sxs-lookup"><span data-stu-id="4ca1c-172">Use hello split-merge tool toomove data tooor from a multi-tenant model tooa single tenant model.</span></span> <span data-ttu-id="4ca1c-173">Lásd: [vegyes egyesítő eszköz](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-173">See [Split merge tool](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ca1c-174">További források</span><span class="sxs-lookup"><span data-stu-id="4ca1c-174">Additional resources</span></span>
<span data-ttu-id="4ca1c-175">A több bérlős szoftverszolgáltatás (SaaS) típusú adatbázis-alkalmazások általános adatarchitektúra-mintázataival kapcsolatos információk: [Tervminták több-bérlős SaaS-alkalmazásokhoz Azure SQL Database esetén](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-175">For information on common data architecture patterns of multi-tenant software-as-a-service (SaaS) database applications, see [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).</span></span>

## <a name="questions-and-feature-requests"></a><span data-ttu-id="4ca1c-176">Kérdések és funkciókra vonatkozó kérések</span><span class="sxs-lookup"><span data-stu-id="4ca1c-176">Questions and Feature Requests</span></span>
<span data-ttu-id="4ca1c-177">A kérdésekhez, lépjen kapcsolatba a hello toous [SQL-adatbázis fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a funkciókra vonatkozó kérések, adjon hozzá toohello [SQL adatbázis-visszajelzési fórumon](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="4ca1c-177">For questions, please reach out toous on hello [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them toohello [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

