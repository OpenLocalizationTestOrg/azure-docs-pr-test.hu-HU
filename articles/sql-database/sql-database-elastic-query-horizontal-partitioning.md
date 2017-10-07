---
title: "kiterjesztett felhő az adatbázisok közötti aaaReporting |} Microsoft Docs"
description: "Hogyan tooset vízszintes partíciók a rugalmas lekérdezések mentése"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: f86eccb8-6323-4ba7-8559-8a7c039049f3
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: mlandzic
ms.openlocfilehash: 78986c2040bf308195bf7c77e64d4f37273fcf36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="41f62-103">Jelentéskészítési közötti kiterjesztett felhő (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="41f62-103">Reporting across scaled-out cloud databases (preview)</span></span>
![Átfogó szilánkok lekérdezése][1]

<span data-ttu-id="41f62-105">Horizontálisan skálázott adatbázisok sorok szét méretezett kimenő adatok réteg.</span><span class="sxs-lookup"><span data-stu-id="41f62-105">Sharded databases distribute rows across a scaled out data tier.</span></span> <span data-ttu-id="41f62-106">hello séma részt vevő adatbázisok, más néven vízszintes particionálás megegyezik.</span><span class="sxs-lookup"><span data-stu-id="41f62-106">hello schema is identical on all participating databases, also known as horizontal partitioning.</span></span> <span data-ttu-id="41f62-107">Rugalmas lekérdezéssel, jelentéseket is létrehozhat, amelyek több összes adatbázis szilánkos-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="41f62-107">Using an elastic query, you can create reports that span all databases in a sharded database.</span></span>

<span data-ttu-id="41f62-108">Tekintse meg a gyors üzembe helyezési [kiterjesztett felhő az adatbázisok közötti Reporting](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-108">For a quick start, see [Reporting across scaled-out cloud databases](sql-database-elastic-query-getting-started.md).</span></span>

<span data-ttu-id="41f62-109">Nem szilánkos adatbázisok, lásd: [eltérő sémával felhő az adatbázisok közötti lekérdezés](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-109">For non-sharded databases, see [Query across cloud databases with different schemas](sql-database-elastic-query-vertical-partitioning.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="41f62-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="41f62-110">Prerequisites</span></span>
* <span data-ttu-id="41f62-111">Hello elastic database ügyféloldali kódtár segítségével shard térkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="41f62-111">Create a shard map using hello elastic database client library.</span></span> <span data-ttu-id="41f62-112">Lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-112">see [Shard map management](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="41f62-113">Vagy használja a hello mintaalkalmazás [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-113">Or use hello sample app in [Get started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>
* <span data-ttu-id="41f62-114">Azt is megteheti, lásd: [áttelepítése a meglévő adatbázis tooscaled kibővített adatbázis](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-114">Alternatively, see [Migrate existing databases tooscaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>
* <span data-ttu-id="41f62-115">hello felhasználónak rendelkeznie kell ALTER ANY külső ADATFORRÁS engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="41f62-115">hello user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="41f62-116">Ez az engedély megtalálható hello ALTER DATABASE engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="41f62-116">This permission is included with hello ALTER DATABASE permission.</span></span>
* <span data-ttu-id="41f62-117">Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.</span><span class="sxs-lookup"><span data-stu-id="41f62-117">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="41f62-118">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="41f62-118">Overview</span></span>
<span data-ttu-id="41f62-119">Ezekre az utasításokra metaadatok alakot hello a szilánkos adatrétegbeli hello rugalmas lekérdezési adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="41f62-119">These statements create hello metadata representation of your sharded data tier in hello elastic query database.</span></span> 

1. [<span data-ttu-id="41f62-120">A FŐKULCS LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="41f62-120">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="41f62-121">HOZZON LÉTRE AZ ADATBÁZISHOZ KÖTŐDŐ HITELESÍTŐ ADATOK</span><span class="sxs-lookup"><span data-stu-id="41f62-121">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="41f62-122">KÜLSŐ ADATFORRÁS LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="41f62-122">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="41f62-123">KÜLSŐ TÁBLA LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="41f62-123">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="41f62-124">1.1 adatbázis hatókörű főkulcs és hitelesítő adatainak létrehozása</span><span class="sxs-lookup"><span data-stu-id="41f62-124">1.1 Create database scoped master key and credentials</span></span>
<span data-ttu-id="41f62-125">hello rugalmas lekérdezési tooconnect tooyour távoli adatbázisok hello hitelesítő adatot használja.</span><span class="sxs-lookup"><span data-stu-id="41f62-125">hello credential is used by hello elastic query tooconnect tooyour remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="41f62-126">Győződjön meg arról, hogy hello *"\<felhasználónév\>"* nem vonatkozik a *"@servername"* utótag.</span><span class="sxs-lookup"><span data-stu-id="41f62-126">Make sure that hello *"\<username\>"* does not include any *"@servername"* suffix.</span></span> 
> 
> 

## <a name="12-create-external-data-sources"></a><span data-ttu-id="41f62-127">1.2 külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="41f62-127">1.2 Create external data sources</span></span>
<span data-ttu-id="41f62-128">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="41f62-128">Syntax:</span></span>

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a><span data-ttu-id="41f62-129">Példa</span><span class="sxs-lookup"><span data-stu-id="41f62-129">Example</span></span>
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

<span data-ttu-id="41f62-130">Lekéri az aktuális külső adatforrások hello listája:</span><span class="sxs-lookup"><span data-stu-id="41f62-130">Retrieve hello list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

<span data-ttu-id="41f62-131">hello külső adatforráshoz a shard térképre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="41f62-131">hello external data source references your shard map.</span></span> <span data-ttu-id="41f62-132">Egy rugalmas lekérdezési ezután hello külső adatforrást és az alapul szolgáló shard leképezési tooenumerate hello adatbázisok, amelyek hello adatrétegbeli hello.</span><span class="sxs-lookup"><span data-stu-id="41f62-132">An elastic query then uses hello external data source and hello underlying shard map tooenumerate hello databases that participate in hello data tier.</span></span> <span data-ttu-id="41f62-133">hello ugyanazokkal a hitelesítő adatokkal használt tooread hello shard térkép és tooaccess hello szilánkok adatok hello rugalmas lekérdezés hello feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="41f62-133">hello same credentials are used tooread hello shard map and tooaccess hello data on hello shards during hello processing of an elastic query.</span></span> 

## <a name="13-create-external-tables"></a><span data-ttu-id="41f62-134">1.3 külső táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="41f62-134">1.3 Create external tables</span></span>
<span data-ttu-id="41f62-135">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="41f62-135">Syntax:</span></span>  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

<span data-ttu-id="41f62-136">**Példa**</span><span class="sxs-lookup"><span data-stu-id="41f62-136">**Example**</span></span>

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 

    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
         SCHEMA_NAME = 'orders', 
         OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

<span data-ttu-id="41f62-137">Lekéri a külső táblák listáját hello hello jelenlegi adatbázisból:</span><span class="sxs-lookup"><span data-stu-id="41f62-137">Retrieve hello list of external tables from hello current database:</span></span> 

    SELECT * from sys.external_tables; 

<span data-ttu-id="41f62-138">toodrop külső táblák:</span><span class="sxs-lookup"><span data-stu-id="41f62-138">toodrop external tables:</span></span>

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a><span data-ttu-id="41f62-139">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="41f62-139">Remarks</span></span>
<span data-ttu-id="41f62-140">hello adatok\_forrás záradék hello külső adatforrás (szilánkok térképre) hello külső tábla használt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="41f62-140">hello DATA\_SOURCE clause defines hello external data source (a shard map) that is used for hello external table.</span></span>  

<span data-ttu-id="41f62-141">hello SÉMA\_nevét és objektum\_neve záradékok hello külső tábla definíciójának tooa tábla egy másik séma hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="41f62-141">hello SCHEMA\_NAME and OBJECT\_NAME clauses map hello external table definition tooa table in a different schema.</span></span> <span data-ttu-id="41f62-142">Ha nincs megadva, hello séma hello távoli objektum toobe "dbo" feltételezi, és felhasználónevében feltételezi múltbeli toobe azonos toohello külső tábla neve.</span><span class="sxs-lookup"><span data-stu-id="41f62-142">If omitted, hello schema of hello remote object is assumed toobe “dbo” and its name is assumed toobe identical toohello external table name being defined.</span></span> <span data-ttu-id="41f62-143">Ez akkor hasznos, ha a távoli tábla hello neve már használatban van hello adatbázisban, amelyben szeretné toocreate hello külső tábla.</span><span class="sxs-lookup"><span data-stu-id="41f62-143">This is useful if hello name of your remote table is already taken in hello database where you want toocreate hello external table.</span></span> <span data-ttu-id="41f62-144">Például azt szeretné, hogy egy külső tábla tooget egy összesítő nézetét katalógusnézetekre toodefine, vagy a kimenő méretezett adatai dinamikus felügyeleti nézetek réteg.</span><span class="sxs-lookup"><span data-stu-id="41f62-144">For  example, you want toodefine an external table tooget an aggregate view of catalog views or DMVs on your scaled out data tier.</span></span> <span data-ttu-id="41f62-145">Katalógusnézetekre és dinamikus felügyeleti nézetek létezik helyileg, mivel a nevek hello külső tábla definíciójának nem használható.</span><span class="sxs-lookup"><span data-stu-id="41f62-145">Since catalog views and DMVs already exist locally, you cannot use their names for hello external table definition.</span></span> <span data-ttu-id="41f62-146">Ehelyett használjon egy másik nevet és hello katalógus nézetben vagy DMV meg nevet a SÉMA hello hello\_név és/vagy az objektum\_záradékok neve.</span><span class="sxs-lookup"><span data-stu-id="41f62-146">Instead, use a different name and use hello catalog view’s or hello DMV’s name in hello SCHEMA\_NAME and/or OBJECT\_NAME clauses.</span></span> <span data-ttu-id="41f62-147">(Lásd az alábbi példa hello.)</span><span class="sxs-lookup"><span data-stu-id="41f62-147">(See hello example below.)</span></span> 

<span data-ttu-id="41f62-148">hello TERJESZTÉSI záradékban hello adatok terjesztési használt ehhez a táblához.</span><span class="sxs-lookup"><span data-stu-id="41f62-148">hello DISTRIBUTION clause specifies hello data distribution used for this table.</span></span> <span data-ttu-id="41f62-149">hello lekérdezésfeldolgozó található hello TERJESZTÉSI záradék toobuild hello leghatékonyabb lekérdezésterveket hello információk használja.</span><span class="sxs-lookup"><span data-stu-id="41f62-149">hello query processor utilizes hello information provided in hello DISTRIBUTION clause toobuild hello most efficient query plans.</span></span>  

1. <span data-ttu-id="41f62-150">**Horizontálisan SKÁLÁZOTT** azt jelenti, hogy adatok vízszintesen particionálása hello adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="41f62-150">**SHARDED** means data is horizontally partitioned across hello databases.</span></span> <span data-ttu-id="41f62-151">Particionálás kulcs hello adatok terjesztési: hello hello **< sharding_column_name >** paraméter.</span><span class="sxs-lookup"><span data-stu-id="41f62-151">hello partitioning key for hello data distribution is hello **<sharding_column_name>** parameter.</span></span>
2. <span data-ttu-id="41f62-152">**REPLIKÁLT** azt jelenti, hogy az egyes adatbázisok jelen-e hello azonos példányát.</span><span class="sxs-lookup"><span data-stu-id="41f62-152">**REPLICATED** means that identical copies of hello table are present on each database.</span></span> <span data-ttu-id="41f62-153">A felelősség tooensure, hogy hello replikák megegyeznek hello adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="41f62-153">It is your responsibility tooensure that hello replicas are identical across hello databases.</span></span>
3. <span data-ttu-id="41f62-154">**KEREK\_MULTIPLEXELÉS** azt jelenti, hogy hello tábla vízszintesen particionálva van, az alkalmazás-függő elosztási módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="41f62-154">**ROUND\_ROBIN** means that hello table is horizontally partitioned using an application-dependent distribution method.</span></span> 

<span data-ttu-id="41f62-155">**Adatok réteg hivatkozás**: hello külső DDL a táblázat tooan külső adatforráshoz.</span><span class="sxs-lookup"><span data-stu-id="41f62-155">**Data tier reference**: hello external table DDL refers tooan external data source.</span></span> <span data-ttu-id="41f62-156">hello külső adatforrás határozza meg, amely hello információ szükséges toolocate hello külső tábla biztosít az adatréteg összes hello adatbázisának shard térképet.</span><span class="sxs-lookup"><span data-stu-id="41f62-156">hello external data source specifies a shard map which provides hello external table with hello information necessary toolocate all hello databases in your data tier.</span></span> 

### <a name="security-considerations"></a><span data-ttu-id="41f62-157">Biztonsági szempontok</span><span class="sxs-lookup"><span data-stu-id="41f62-157">Security considerations</span></span>
<span data-ttu-id="41f62-158">Felhasználók és a hozzáférés toohello külső tábla automatikusan hozzáférést toohello alapul szolgáló távoli táblák hello külső adatforrása definíciója megadott hello hitelesítő adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="41f62-158">Users with access toohello external table automatically gain access toohello underlying remote tables under hello credential given in hello external data source definition.</span></span> <span data-ttu-id="41f62-159">Hello hello külső adatforrás hitelesítő adatai segítségével nemkívánatos jogok kiterjesztésének elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="41f62-159">Avoid undesired elevation of privileges through hello credential of hello external data source.</span></span> <span data-ttu-id="41f62-160">Használja GRANT vagy VISSZAVONNI egy külső tábla ugyanúgy, mintha az lenne normál táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="41f62-160">Use GRANT or REVOKE for an external table just as though it were a regular table.</span></span>  

<span data-ttu-id="41f62-161">Miután meghatározta a külső adatforrást és a külső táblák, a külső táblákon végrehajtott most már használhatja a teljes T-SQL.</span><span class="sxs-lookup"><span data-stu-id="41f62-161">Once you have defined your external data source and your external tables, you can now use full T-SQL over your external tables.</span></span>

## <a name="example-querying-horizontal-partitioned-databases"></a><span data-ttu-id="41f62-162">Példa: vízszintes particionált adatbázisok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="41f62-162">Example: querying horizontal partitioned databases</span></span>
<span data-ttu-id="41f62-163">hello következő lekérdezés során, a rendeléseket és a sorok háromutas illesztést végez és több összesítések és szelektív szűrőt.</span><span class="sxs-lookup"><span data-stu-id="41f62-163">hello following query performs a three-way join between warehouses, orders and order lines and uses several aggregates and a selective filter.</span></span> <span data-ttu-id="41f62-164">Azt feltételezi, hogy (1) vízszintes particionálási (horizontális skálázási) és (2), hogy adatraktárakra, rendeléseket és sorok szilánkos hello adatraktár azonosító oszlop szerint, és hello rugalmas lekérdezés közös elhelyezése a hello szilánkok hello illesztéseket, és a hello hello lekérdezés költséges részét hello feldolgozni a szilánkok párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="41f62-164">It assumes (1) horizontal partitioning (sharding) and (2) that warehouses, orders and order lines are sharded by hello warehouse id column, and that hello elastic query can co-locate hello joins on hello shards and process hello expensive part of hello query on hello shards in parallel.</span></span> 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="41f62-165">Tárolt eljárás távoli T-SQL-végrehajtásra: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="41f62-165">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="41f62-166">Rugalmas lekérdezési is vezet be, amely közvetlen hozzáférést biztosít a toohello szilánkok tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="41f62-166">Elastic query also introduces a stored procedure that provides direct access toohello shards.</span></span> <span data-ttu-id="41f62-167">hello tárolt eljárás neve [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) és használt tooexecute távoli tárolt eljárások és a T-SQL kód a hello távoli adatbázis is lehet.</span><span class="sxs-lookup"><span data-stu-id="41f62-167">hello stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used tooexecute remote stored procedures or T-SQL code on hello remote databases.</span></span> <span data-ttu-id="41f62-168">A következő paraméterek hello tart:</span><span class="sxs-lookup"><span data-stu-id="41f62-168">It takes hello following parameters:</span></span> 

* <span data-ttu-id="41f62-169">Adatforrás neve (nvarchar): hello külső adatforrás RDBMS típusú hello neve.</span><span class="sxs-lookup"><span data-stu-id="41f62-169">Data source name (nvarchar): hello name of hello external data source of type RDBMS.</span></span> 
* <span data-ttu-id="41f62-170">Lekérdezés (nvarchar): hello T-SQL lekérdezés toobe minden shard végre.</span><span class="sxs-lookup"><span data-stu-id="41f62-170">Query (nvarchar): hello T-SQL query toobe executed on each shard.</span></span> 
* <span data-ttu-id="41f62-171">Paraméterdeklarációhoz (nvarchar) – nem kötelező: adatok típusdefiníciók hello paraméterek hello lekérdezési paraméter (például sp_executesql) használt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="41f62-171">Parameter declaration (nvarchar) - optional: String with data type definitions for hello parameters used in hello Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="41f62-172">Paraméter értéklistát - választható: paraméterértékek (például sp_executesql) vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="41f62-172">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="41f62-173">hello sp\_hajtható végre\_távoli használ hello külső adatforrás hello Meghívási paraméterek tooexecute hello T-SQL-utasításban megadott hello távoli adatbázisok találhatók.</span><span class="sxs-lookup"><span data-stu-id="41f62-173">hello sp\_execute\_remote uses hello external data source provided in hello invocation parameters tooexecute hello given T-SQL statement on hello remote databases.</span></span> <span data-ttu-id="41f62-174">Hello hitelesítő adatai hello külső adatok forrás tooconnect toohello shardmap manager-adatbázis és a távoli adatbázisokhoz hello használ.</span><span class="sxs-lookup"><span data-stu-id="41f62-174">It uses hello credential of hello external data source tooconnect toohello shardmap manager database and hello remote databases.</span></span>  

<span data-ttu-id="41f62-175">Példa:</span><span class="sxs-lookup"><span data-stu-id="41f62-175">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a><span data-ttu-id="41f62-176">Eszközök kapcsolattal</span><span class="sxs-lookup"><span data-stu-id="41f62-176">Connectivity for tools</span></span>
<span data-ttu-id="41f62-177">Rendszeres SQL Server kapcsolati karakterláncok tooconnect használja az alkalmazást, a BI és az integráció eszközök toohello adatbázis a külső tábla-definíciók.</span><span class="sxs-lookup"><span data-stu-id="41f62-177">Use regular SQL Server connection strings tooconnect your application, your BI and data integration tools toohello database with your external table definitions.</span></span> <span data-ttu-id="41f62-178">Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="41f62-178">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="41f62-179">Majd hivatkozhat hello rugalmas lekérdezési adatbázis, mint bármely más SQL Server adatbázis csatlakoztatva toohello eszközt, és a külső táblákhoz az eszköz vagy alkalmazás használja, helyi táblákkal, mintha.</span><span class="sxs-lookup"><span data-stu-id="41f62-179">Then reference hello elastic query database like any other SQL Server database connected toohello tool, and use external tables from your tool or application as if they were local tables.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="41f62-180">Ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="41f62-180">Best practices</span></span>
* <span data-ttu-id="41f62-181">Győződjön meg arról, hello rugalmas lekérdezési végpont adatbázis megkapta-e hozzáférési toohello shardmap adatbázis és SQL-adatbázis a tűzfalak hello keresztül minden szilánkok.</span><span class="sxs-lookup"><span data-stu-id="41f62-181">Ensure that hello elastic query endpoint database has been given access toohello shardmap database and all shards through hello SQL DB firewalls.</span></span>  
* <span data-ttu-id="41f62-182">Ellenőrzi, vagy hello adatok terjesztési hello külső tábla által meghatározott kényszerítéséhez.</span><span class="sxs-lookup"><span data-stu-id="41f62-182">Validate or enforce hello data distribution defined by hello external table.</span></span> <span data-ttu-id="41f62-183">Ha a tényleges adatok terjesztési eltér a tábla definíciójában megadott hello terjesztési, a lekérdezések előfordulhat, hogy eredményt várt.</span><span class="sxs-lookup"><span data-stu-id="41f62-183">If your actual data distribution is different from hello distribution specified in your table definition, your queries may yield unexpected results.</span></span> 
* <span data-ttu-id="41f62-184">Rugalmas lekérdezés jelenleg nem végeznek eltávolítási szilánkok predikátumok hello horizontális kulcs keresztül lehetővé tenné, toosafely kizárási bizonyos szilánkok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="41f62-184">Elastic query currently does not perform shard elimination when predicates over hello sharding key would allow it toosafely exclude certain shards from processing.</span></span>
* <span data-ttu-id="41f62-185">Rugalmas lekérdezési működik a legjobban az lekérdezések ahol hello szilánkok hello számítási többsége végezhető.</span><span class="sxs-lookup"><span data-stu-id="41f62-185">Elastic query works best for queries where most of hello computation can be done on hello shards.</span></span> <span data-ttu-id="41f62-186">Általában beolvasása a legjobb lekérdezési teljesítményt hello szelektív szűrő predikátumok kiértékelhető hello szilánkok vagy illesztések keresztül az összes szilánkok partíciójához módon végrehajtható kulcsok particionálás hello.</span><span class="sxs-lookup"><span data-stu-id="41f62-186">You typically get hello best query performance with selective filter predicates that can be evaluated on hello shards or joins over hello partitioning keys that can be performed in a partition-aligned way on all shards.</span></span> <span data-ttu-id="41f62-187">Más lekérdezési mintáinak esetleg tooload nagy mennyiségű hello szilánkok toohello átjárócsomópont adatait, és rosszul hajthatja végre</span><span class="sxs-lookup"><span data-stu-id="41f62-187">Other query patterns may need tooload large amounts of data from hello shards toohello head node and may perform poorly</span></span>

## <a name="next-steps"></a><span data-ttu-id="41f62-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41f62-188">Next steps</span></span>

* <span data-ttu-id="41f62-189">Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-189">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="41f62-190">Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-190">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="41f62-191">A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="41f62-191">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="41f62-192">Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="41f62-192">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="41f62-193">Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.</span><span class="sxs-lookup"><span data-stu-id="41f62-193">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
