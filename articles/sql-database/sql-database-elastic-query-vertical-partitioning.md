---
title: "a különböző séma adatbázisok aaaQuery felhő között |} Microsoft Docs"
description: "Hogyan tooset adatbázisok közötti függőleges partíció a lekérdezések mentése"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 1134e2e608128b7a9cac47ff35a22a11e6e5bc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a><span data-ttu-id="8c02d-103">Más sémák (előzetes verzió) felhő adatbázisokkal átfogó lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8c02d-103">Query across cloud databases with different schemas (preview)</span></span>
![A különböző adatbázisokhoz táblák átfogó lekérdezése][1]

<span data-ttu-id="8c02d-105">Adatbázisok függőleges particionált táblák más-más részhalmazához különböző adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="8c02d-105">Vertically-partitioned databases use different sets of tables on different databases.</span></span> <span data-ttu-id="8c02d-106">Ez azt jelenti, hogy adott hello sémája nem egyezik meg a különböző adatbázisokhoz.</span><span class="sxs-lookup"><span data-stu-id="8c02d-106">That means that hello schema is different on different databases.</span></span> <span data-ttu-id="8c02d-107">Például a készlet összes tábla esetén egy adatbázis míg nyilvántartási kapcsolatos összes táblázatot egy második adatbázison.</span><span class="sxs-lookup"><span data-stu-id="8c02d-107">For instance, all tables for inventory are on one database while all accounting-related tables are on a second database.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8c02d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c02d-108">Prerequisites</span></span>
* <span data-ttu-id="8c02d-109">hello felhasználónak rendelkeznie kell ALTER ANY külső ADATFORRÁS engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="8c02d-109">hello user must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="8c02d-110">Ez az engedély megtalálható hello ALTER DATABASE engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="8c02d-110">This permission is included with hello ALTER DATABASE permission.</span></span>
* <span data-ttu-id="8c02d-111">Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.</span><span class="sxs-lookup"><span data-stu-id="8c02d-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="overview"></a><span data-ttu-id="8c02d-112">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8c02d-112">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="8c02d-113">Ellentétben a vízszintes particionálás, a DDL-utasításokban nem függ egy adatrétegbeli keresztül hello elastic database ügyféloldali kódtára a shard leképezést definiáló.</span><span class="sxs-lookup"><span data-stu-id="8c02d-113">Unlike with horizontal partitioning, these DDL statements do not depend on defining a data tier with a shard map through hello elastic database client library.</span></span>
>

1. [<span data-ttu-id="8c02d-114">A FŐKULCS LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="8c02d-114">CREATE MASTER KEY</span></span>](https://msdn.microsoft.com/library/ms174382.aspx)
2. [<span data-ttu-id="8c02d-115">HOZZON LÉTRE AZ ADATBÁZISHOZ KÖTŐDŐ HITELESÍTŐ ADATOK</span><span class="sxs-lookup"><span data-stu-id="8c02d-115">CREATE DATABASE SCOPED CREDENTIAL</span></span>](https://msdn.microsoft.com/library/mt270260.aspx)
3. [<span data-ttu-id="8c02d-116">KÜLSŐ ADATFORRÁS LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="8c02d-116">CREATE EXTERNAL DATA SOURCE</span></span>](https://msdn.microsoft.com/library/dn935022.aspx)
4. [<span data-ttu-id="8c02d-117">KÜLSŐ TÁBLA LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="8c02d-117">CREATE EXTERNAL TABLE</span></span>](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a><span data-ttu-id="8c02d-118">Adatbázis hatókörű főkulcs és hitelesítő adatainak létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c02d-118">Create database scoped master key and credentials</span></span>
<span data-ttu-id="8c02d-119">hello rugalmas lekérdezési tooconnect tooyour távoli adatbázisok hello hitelesítő adatot használja.</span><span class="sxs-lookup"><span data-stu-id="8c02d-119">hello credential is used by hello elastic query tooconnect tooyour remote databases.</span></span>  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> <span data-ttu-id="8c02d-120">Győződjön meg arról, hogy hello `<username>` nem vonatkozik a **"@servername"** utótag.</span><span class="sxs-lookup"><span data-stu-id="8c02d-120">Ensure that hello `<username>` does not include any **"@servername"** suffix.</span></span> 
>

## <a name="create-external-data-sources"></a><span data-ttu-id="8c02d-121">Külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c02d-121">Create external data sources</span></span>
<span data-ttu-id="8c02d-122">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="8c02d-122">Syntax:</span></span>

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> <span data-ttu-id="8c02d-123">hello típusparaméter túl be kell állítani**RDBMS**.</span><span class="sxs-lookup"><span data-stu-id="8c02d-123">hello TYPE parameter must be set too**RDBMS**.</span></span> 
>

### <a name="example"></a><span data-ttu-id="8c02d-124">Példa</span><span class="sxs-lookup"><span data-stu-id="8c02d-124">Example</span></span>
<span data-ttu-id="8c02d-125">hello alábbi példa azt mutatja be a külső adatforrások hello hello CREATE utasítás használatát.</span><span class="sxs-lookup"><span data-stu-id="8c02d-125">hello following example illustrates hello use of hello CREATE statement for external data sources.</span></span> 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

<span data-ttu-id="8c02d-126">aktuális külső adatforrások tooretrieve hello listája:</span><span class="sxs-lookup"><span data-stu-id="8c02d-126">tooretrieve hello list of current external data sources:</span></span> 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a><span data-ttu-id="8c02d-127">Külső táblák</span><span class="sxs-lookup"><span data-stu-id="8c02d-127">External Tables</span></span>
<span data-ttu-id="8c02d-128">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="8c02d-128">Syntax:</span></span>

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a><span data-ttu-id="8c02d-129">Példa</span><span class="sxs-lookup"><span data-stu-id="8c02d-129">Example</span></span>
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

<span data-ttu-id="8c02d-130">hello a következő példa bemutatja, hogyan tooretrieve hello hello jelenlegi adatbázisból külső táblák listáját:</span><span class="sxs-lookup"><span data-stu-id="8c02d-130">hello following example shows how tooretrieve hello list of external tables from hello current database:</span></span> 

    select * from sys.external_tables; 

### <a name="remarks"></a><span data-ttu-id="8c02d-131">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8c02d-131">Remarks</span></span>
<span data-ttu-id="8c02d-132">Rugalmas lekérdezési hello meglévő külső tábla szintaxis toodefine külső táblák külső adatforrások RDBMS típusú használó terjeszti ki.</span><span class="sxs-lookup"><span data-stu-id="8c02d-132">Elastic query extends hello existing external table syntax toodefine external tables that use external data sources of type RDBMS.</span></span> <span data-ttu-id="8c02d-133">Egy külső tábla definícióját a vertikális particionálás hello a következő szempontokat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="8c02d-133">An external table definition for vertical partitioning covers hello following aspects:</span></span> 

* <span data-ttu-id="8c02d-134">**Séma**: hello külső táblára vonatkozó DDL definiálja a séma, a lekérdezéseket használhat.</span><span class="sxs-lookup"><span data-stu-id="8c02d-134">**Schema**: hello external table DDL defines a schema that your queries can use.</span></span> <span data-ttu-id="8c02d-135">a külső tábla definíciójában megadott hello séma kell toomatch hello séma hello hello távoli adatbázis hello tényleges adatokat tároló tábla.</span><span class="sxs-lookup"><span data-stu-id="8c02d-135">hello schema provided in your external table definition needs toomatch hello schema of hello tables in hello remote database where hello actual data is stored.</span></span> 
* <span data-ttu-id="8c02d-136">**Távoli adatbázis hivatkozás**: hello külső DDL a táblázat tooan külső adatforráshoz.</span><span class="sxs-lookup"><span data-stu-id="8c02d-136">**Remote database reference**: hello external table DDL refers tooan external data source.</span></span> <span data-ttu-id="8c02d-137">hello külső adatforrás hello logikai kiszolgáló nevét és a hello távoli adatbázis hello tábla tényleges adatokat tároló adatbázis nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="8c02d-137">hello external data source specifies hello logical server name and database name of hello remote database where hello actual table data is stored.</span></span> 

<span data-ttu-id="8c02d-138">A külső hello előző szakaszokban foglaltaknak, hello szintaxis toocreate külső táblák használata az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="8c02d-138">Using an external data source as outlined in hello previous section, hello syntax toocreate external tables is as follows:</span></span> 

<span data-ttu-id="8c02d-139">hello DATA_SOURCE záradék hello külső adatforrás (azaz hello távoli adatbázis esetén a vertikális particionálás) hello külső tábla használt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8c02d-139">hello DATA_SOURCE clause defines hello external data source (i.e. hello remote database in case of vertical partitioning) that is used for hello external table.</span></span>  

<span data-ttu-id="8c02d-140">hello SCHEMA_NAME és OBJECT_NAME záradékok biztosítanak hello képességét toomap hello külső tábla definíciójának tooa tábla egy másik séma hello távoli adatbázis, vagy egy eltérő nevű tooa tábla kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="8c02d-140">hello SCHEMA_NAME and OBJECT_NAME clauses provide hello ability toomap hello external table definition tooa table in a different schema on hello remote database, or tooa table with a different name, respectively.</span></span> <span data-ttu-id="8c02d-141">Ez akkor hasznos, ha azt szeretné, toodefine egy külső tábla tooa katalógusnézet használatával derítheti ki vagy DMV a távoli adatbázis - vagy bármely más olyan esetben, ha hello távoli tábla neve már használatban van helyileg.</span><span class="sxs-lookup"><span data-stu-id="8c02d-141">This is useful if you want toodefine an external table tooa catalog view or DMV on your remote database - or any other situation where hello remote table name is already taken locally.</span></span>  

<span data-ttu-id="8c02d-142">hello következő DDL-utasítás elutasítja azokat egy meglévő külső tábla definíciójának hello helyi katalógus.</span><span class="sxs-lookup"><span data-stu-id="8c02d-142">hello following DDL statement drops an existing external table definition from hello local catalog.</span></span> <span data-ttu-id="8c02d-143">Hello távoli adatbázis nem befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="8c02d-143">It does not impact hello remote database.</span></span> 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

<span data-ttu-id="8c02d-144">**A külső tábla létrehozása/DROP engedélyeinek**: ALTER ANY külső ADATFORRÁS engedélyekre van szükség, a külső tábla DDL, amely egyben szükséges toorefer toohello alapul szolgáló adatforrásban.</span><span class="sxs-lookup"><span data-stu-id="8c02d-144">**Permissions for CREATE/DROP EXTERNAL TABLE**: ALTER ANY EXTERNAL DATA SOURCE permissions are needed for external table DDL which is also needed toorefer toohello underlying data source.</span></span>  

## <a name="security-considerations"></a><span data-ttu-id="8c02d-145">Biztonsági szempontok</span><span class="sxs-lookup"><span data-stu-id="8c02d-145">Security considerations</span></span>
<span data-ttu-id="8c02d-146">Felhasználók és a hozzáférés toohello külső tábla automatikusan hozzáférést toohello alapul szolgáló távoli táblák hello külső adatforrása definíciója megadott hello hitelesítő adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="8c02d-146">Users with access toohello external table automatically gain access toohello underlying remote tables under hello credential given in hello external data source definition.</span></span> <span data-ttu-id="8c02d-147">Gondosan hozzáférés toohello külső tábla rendelés nemkívánatos tooavoid jogok kiterjesztése hello hello külső adatforrás hitelesítő adatai segítségével kezelje.</span><span class="sxs-lookup"><span data-stu-id="8c02d-147">You should carefully manage access toohello external table in order tooavoid undesired elevation of privileges through hello credential of hello external data source.</span></span> <span data-ttu-id="8c02d-148">Rendszeres SQL lehet engedélyeket használt tooGRANT vagy a REVOKE access tooan külső tábla ugyanúgy, mintha az lenne normál táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="8c02d-148">Regular SQL permissions can be used tooGRANT or REVOKE access tooan external table just as though it were a regular table.</span></span>  

## <a name="example-querying-vertically-partitioned-databases"></a><span data-ttu-id="8c02d-149">Példa: adatbázisok függőleges lekérdezése particionálva.</span><span class="sxs-lookup"><span data-stu-id="8c02d-149">Example: querying vertically partitioned databases</span></span>
<span data-ttu-id="8c02d-150">hello következő lekérdezés illesztést végez a háromutas hello két helyi táblázatokhoz rendelések és a sorok és hello távoli ügyfelek esetén.</span><span class="sxs-lookup"><span data-stu-id="8c02d-150">hello following query performs a three-way join between hello two local tables for orders and order lines and hello remote table for customers.</span></span> <span data-ttu-id="8c02d-151">Ez az hello hivatkozás adatok használati eset rugalmas lekérdezési példát:</span><span class="sxs-lookup"><span data-stu-id="8c02d-151">This is an example of hello reference data use case for elastic query:</span></span> 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a><span data-ttu-id="8c02d-152">Tárolt eljárás távoli T-SQL-végrehajtásra: sp\_execute_remote</span><span class="sxs-lookup"><span data-stu-id="8c02d-152">Stored procedure for remote T-SQL execution: sp\_execute_remote</span></span>
<span data-ttu-id="8c02d-153">Rugalmas lekérdezési is vezet be, amely közvetlen hozzáférést biztosít a toohello szilánkok tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="8c02d-153">Elastic query also introduces a stored procedure that provides direct access toohello shards.</span></span> <span data-ttu-id="8c02d-154">hello tárolt eljárás neve [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) és használt tooexecute távoli tárolt eljárások és a T-SQL kód a hello távoli adatbázis is lehet.</span><span class="sxs-lookup"><span data-stu-id="8c02d-154">hello stored procedure is called [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) and can be used tooexecute remote stored procedures or T-SQL code on hello remote databases.</span></span> <span data-ttu-id="8c02d-155">A következő paraméterek hello tart:</span><span class="sxs-lookup"><span data-stu-id="8c02d-155">It takes hello following parameters:</span></span> 

* <span data-ttu-id="8c02d-156">Adatforrás neve (nvarchar): hello külső adatforrás RDBMS típusú hello neve.</span><span class="sxs-lookup"><span data-stu-id="8c02d-156">Data source name (nvarchar): hello name of hello external data source of type RDBMS.</span></span> 
* <span data-ttu-id="8c02d-157">Lekérdezés (nvarchar): hello T-SQL lekérdezés toobe minden shard végre.</span><span class="sxs-lookup"><span data-stu-id="8c02d-157">Query (nvarchar): hello T-SQL query toobe executed on each shard.</span></span> 
* <span data-ttu-id="8c02d-158">Paraméterdeklarációhoz (nvarchar) – nem kötelező: adatok típusdefiníciók hello paraméterek hello lekérdezési paraméter (például sp_executesql) használt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8c02d-158">Parameter declaration (nvarchar) - optional: String with data type definitions for hello parameters used in hello Query parameter (like sp_executesql).</span></span> 
* <span data-ttu-id="8c02d-159">Paraméter értéklistát - választható: paraméterértékek (például sp_executesql) vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="8c02d-159">Parameter value list - optional: Comma-separated list of parameter values (like sp_executesql).</span></span>

<span data-ttu-id="8c02d-160">hello sp\_hajtható végre\_távoli használ hello külső adatforrás hello Meghívási paraméterek tooexecute hello T-SQL-utasításban megadott hello távoli adatbázisok találhatók.</span><span class="sxs-lookup"><span data-stu-id="8c02d-160">hello sp\_execute\_remote uses hello external data source provided in hello invocation parameters tooexecute hello given T-SQL statement on hello remote databases.</span></span> <span data-ttu-id="8c02d-161">Hello hitelesítő adatai hello külső adatok forrás tooconnect toohello shardmap manager-adatbázis és a távoli adatbázisokhoz hello használ.</span><span class="sxs-lookup"><span data-stu-id="8c02d-161">It uses hello credential of hello external data source tooconnect toohello shardmap manager database and hello remote databases.</span></span>  

<span data-ttu-id="8c02d-162">Példa:</span><span class="sxs-lookup"><span data-stu-id="8c02d-162">Example:</span></span> 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 



## <a name="connectivity-for-tools"></a><span data-ttu-id="8c02d-163">Eszközök kapcsolattal</span><span class="sxs-lookup"><span data-stu-id="8c02d-163">Connectivity for tools</span></span>
<span data-ttu-id="8c02d-164">Használhat rendszeres SQL Server kapcsolati karakterláncok tooconnect a BI és az integráció eszközök toodatabases hello SQL-adatbázis a kiszolgálón, amelyen engedélyezve van a Rugalmas lekérdezési és definiált külső táblák.</span><span class="sxs-lookup"><span data-stu-id="8c02d-164">You can use regular SQL Server connection strings tooconnect your BI and data integration tools toodatabases on hello SQL DB server that has elastic query enabled and external tables defined.</span></span> <span data-ttu-id="8c02d-165">Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="8c02d-165">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="8c02d-166">Ezután tekintse meg a toohello rugalmas lekérdezési adatbázis és a külső táblákra csakúgy, mint bármely más SQL Server-adatbázist, hogy Ön kapcsolódnának toowith az eszköz.</span><span class="sxs-lookup"><span data-stu-id="8c02d-166">Then refer toohello elastic query database and its external tables just like any other SQL Server database that you would connect toowith your tool.</span></span> 

## <a name="best-practices"></a><span data-ttu-id="8c02d-167">Ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="8c02d-167">Best practices</span></span>
* <span data-ttu-id="8c02d-168">Győződjön meg arról, hogy hello rugalmas lekérdezési végpont az adatbázis adott adatbázist toohello távoli Azure-szolgáltatások hozzáférésének engedélyezéséhez az SQL-adatbázis a tűzfal-konfigurációjában.</span><span class="sxs-lookup"><span data-stu-id="8c02d-168">Ensure that hello elastic query endpoint database has been given access toohello remote database by enabling access for Azure Services in its SQL DB firewall configuration.</span></span> <span data-ttu-id="8c02d-169">Bizonyosodjon meg arról, hogy hello hitelesítő adat hello külső adatok adatforrása definíciója sikeresen be tud jelentkezni a hello távoli adatbázis és a hello engedélyek tooaccess hello távoli táblájában.</span><span class="sxs-lookup"><span data-stu-id="8c02d-169">Also ensure that hello credential provided in hello external data source definition can successfully log into hello remote database and has hello permissions tooaccess hello remote table.</span></span>  
* <span data-ttu-id="8c02d-170">Rugalmas lekérdezési működik a legjobban az lekérdezések ahol hello számítási többsége a távoli adatbázis hello végezhető.</span><span class="sxs-lookup"><span data-stu-id="8c02d-170">Elastic query works best for queries where most of hello computation can be done on hello remote databases.</span></span> <span data-ttu-id="8c02d-171">Általában legjobb lekérdezési teljesítményt hello szelektív szűrő predikátumok hello távoli adatbázisokhoz vagy végrehajtható teljesen hello távoli adatbázis illesztések kiértékelhető kap.</span><span class="sxs-lookup"><span data-stu-id="8c02d-171">You typically get hello best query performance with selective filter predicates that can be evaluated on hello remote databases or joins that can be performed completely on hello remote database.</span></span> <span data-ttu-id="8c02d-172">Más lekérdezési mintáinak esetleg tooload nagy mennyiségű hello távoli adatbázis adatait, és rosszul hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="8c02d-172">Other query patterns may need tooload large amounts of data from hello remote database and may perform poorly.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8c02d-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c02d-173">Next steps</span></span>

* <span data-ttu-id="8c02d-174">Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8c02d-174">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="8c02d-175">Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="8c02d-175">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="8c02d-176">Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8c02d-176">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="8c02d-177">A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="8c02d-177">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="8c02d-178">Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.</span><span class="sxs-lookup"><span data-stu-id="8c02d-178">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
