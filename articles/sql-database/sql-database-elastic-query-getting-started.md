---
title: "A jelentés (vízszintes particionálás) kiterjesztett felhő az adatbázisok közötti |} Microsoft Docs"
description: "több adatbázis adatbázis-lekérdezések használata"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: 8eb56d44c3a261f6325d4fc91f169d09bf108160
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="1cc94-103">Jelentés közötti kiterjesztett felhő (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="1cc94-103">Report across scaled-out cloud databases (preview)</span></span>
<span data-ttu-id="1cc94-104">A több Azure SQL-adatbázisok egyetlen kapcsolódási pont használatával jelentéseket hozhat létre egy [rugalmas lekérdezési](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-104">You can create reports from multiple Azure SQL databases from a single connection point using an [elastic query](sql-database-elastic-query-overview.md).</span></span> <span data-ttu-id="1cc94-105">Az adatbázisok vízszintesen kell particionálni (más néven "szilánkos").</span><span class="sxs-lookup"><span data-stu-id="1cc94-105">The databases must be horizontally partitioned (also known as "sharded").</span></span>

<span data-ttu-id="1cc94-106">Ha egy meglévő adatbázist, olvassa el [kiterjesztett adatbázisok áttelepítése a meglévő adatbázisok](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-106">If you have an existing database, see [Migrating existing databases to scaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>

<span data-ttu-id="1cc94-107">A lekérdezni szükséges SQL objektumok ismertetése: [vízszintesen particionált az adatbázisok közötti lekérdezés](sql-database-elastic-query-horizontal-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-107">To understand the SQL objects needed to query, see [Query across horizontally partitioned databases](sql-database-elastic-query-horizontal-partitioning.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cc94-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1cc94-108">Prerequisites</span></span>
<span data-ttu-id="1cc94-109">Töltse le és futtassa a [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-109">Download and run the [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-the-sample-app"></a><span data-ttu-id="1cc94-110">Hozzon létre egy shard mintaalkalmazás térkép-kezelőt</span><span class="sxs-lookup"><span data-stu-id="1cc94-110">Create a shard map manager using the sample app</span></span>
<span data-ttu-id="1cc94-111">Itt hoz létre a shard térképre manager több szegmensben osztják, az adatok beszúrását követi azokat a szilánkok együtt.</span><span class="sxs-lookup"><span data-stu-id="1cc94-111">Here you will create a shard map manager along with several shards, followed by insertion of data into the shards.</span></span> <span data-ttu-id="1cc94-112">Ha már rendelkezik a bennük foglalt horizontálisan skálázott adatok szilánkok telepítése történhet meg, hagyja ki a következő lépéseket, és helyezze át a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1cc94-112">If you happen to already have shards setup with sharded data in them, you can skip the following steps and move to the next section.</span></span>

1. <span data-ttu-id="1cc94-113">Hozza létre, és futtassa a **Ismerkedés a rugalmas adatbáziseszközöket** mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1cc94-113">Build and run the **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="1cc94-114">Kövesse a lépéseket, amíg a szakasz a 7. lépés [töltse le és futtassa a mintaalkalmazást](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="1cc94-114">Follow the steps until step 7 in the section [Download and run the sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="1cc94-115">7. lépés végén jelenik meg a következő parancssort:</span><span class="sxs-lookup"><span data-stu-id="1cc94-115">At the end of Step 7, you will see the following command prompt:</span></span>

    ![parancssor][1]
2. <span data-ttu-id="1cc94-117">A parancsablakban írja be a "1", és nyomja le az ENTER **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1cc94-117">In the command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="1cc94-118">Létrehozza a shard térkép manager, és két szilánkok hozzáadása a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="1cc94-118">This creates the shard map manager, and adds two shards to the server.</span></span> <span data-ttu-id="1cc94-119">Ezután írja be a "3", és nyomja le az ENTER **Enter**; a művelet megismétlése négy alkalommal.</span><span class="sxs-lookup"><span data-stu-id="1cc94-119">Then type "3" and press **Enter**; repeat the action four times.</span></span> <span data-ttu-id="1cc94-120">Ez a szilánkok minta adatsorok szúrja be.</span><span class="sxs-lookup"><span data-stu-id="1cc94-120">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="1cc94-121">A [Azure-portálon](https://portal.azure.com) jelenítsen meg három új adatbázist a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="1cc94-121">The [Azure portal](https://portal.azure.com) should show three new databases in your server:</span></span>

   ![A Visual Studio-jóváhagyás][2]

   <span data-ttu-id="1cc94-123">Ezen a ponton adatbázisok közötti-lekérdezések használata támogatott a Elastic Database ügyféloldali kódtáraknál keresztül.</span><span class="sxs-lookup"><span data-stu-id="1cc94-123">At this point, cross-database queries are supported through the Elastic Database client libraries.</span></span> <span data-ttu-id="1cc94-124">Például használja a 4. lehetőséget a parancsablakban.</span><span class="sxs-lookup"><span data-stu-id="1cc94-124">For example, use option 4 in the command window.</span></span> <span data-ttu-id="1cc94-125">A lekérdezés eredményeként előálló egy több shard a rendszer mindig egy **UNION ALL** az összes szilánkok az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="1cc94-125">The results from a multi-shard query are always a **UNION ALL** of the results from all shards.</span></span>

   <span data-ttu-id="1cc94-126">A következő szakaszban létrehozhatunk egy minta adatbázis végponttal, amely támogatja a gazdagabb lekérdezése az adatok között szilánkok.</span><span class="sxs-lookup"><span data-stu-id="1cc94-126">In the next section, we create a sample database endpoint that supports richer querying of the data across shards.</span></span>

## <a name="create-an-elastic-query-database"></a><span data-ttu-id="1cc94-127">A lekérdezés rugalmas adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc94-127">Create an elastic query database</span></span>
1. <span data-ttu-id="1cc94-128">Nyissa meg a [Azure-portálon](https://portal.azure.com) , és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="1cc94-128">Open the [Azure portal](https://portal.azure.com) and log in.</span></span>
2. <span data-ttu-id="1cc94-129">Új Azure SQL-adatbázis létrehozása a shard beállításai ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="1cc94-129">Create a new Azure SQL database in the same server as your shard setup.</span></span> <span data-ttu-id="1cc94-130">Neve az adatbázis "ElasticDBQuery."</span><span class="sxs-lookup"><span data-stu-id="1cc94-130">Name the database "ElasticDBQuery."</span></span>

    ![Azure-portál és a tarifacsomag][3]

    > [!NOTE]
    > <span data-ttu-id="1cc94-132">használhat egy meglévő adatbázist.</span><span class="sxs-lookup"><span data-stu-id="1cc94-132">you can use an existing database.</span></span> <span data-ttu-id="1cc94-133">Ha így tesz, nem lehet egy a szilánkok, amelyeket szeretne, a-lekérdezéseket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="1cc94-133">If you can do so, it must not be one of the shards that you would like to execute your queries on.</span></span> <span data-ttu-id="1cc94-134">Ezt az adatbázist a metaadat-objektumok egy rugalmas adatbázis-lekérdezés létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="1cc94-134">This database will be used for creating the metadata objects for an elastic database query.</span></span>
    >

## <a name="create-database-objects"></a><span data-ttu-id="1cc94-135">Adatbázis-objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc94-135">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="1cc94-136">Adatbázis-hatókörű főkulcs és a hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="1cc94-136">Database-scoped master key and credentials</span></span>
<span data-ttu-id="1cc94-137">Ezek használhatók a shard térkép manager és a szilánkok való csatlakozáshoz:</span><span class="sxs-lookup"><span data-stu-id="1cc94-137">These are used to connect to the shard map manager and the shards:</span></span>

1. <span data-ttu-id="1cc94-138">Nyissa meg az SQL Server Management Studio vagy Visual Studio SQL Server Data Tools összetevővel.</span><span class="sxs-lookup"><span data-stu-id="1cc94-138">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="1cc94-139">Csatlakozzon az ElasticDBQuery adatbázisához, és a következő T-SQL-parancsok:</span><span class="sxs-lookup"><span data-stu-id="1cc94-139">Connect to ElasticDBQuery database and execute the following T-SQL commands:</span></span>

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    <span data-ttu-id="1cc94-140">"felhasználónév" és a "password" legyen ugyanaz, mint a 6. lépésében használt bejelentkezési adatok [töltse le és futtassa a mintaalkalmazást](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) a [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-140">"username" and "password" should be the same as login information used in step 6 of [Download and run the sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [Getting started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="1cc94-141">Külső adatforrások</span><span class="sxs-lookup"><span data-stu-id="1cc94-141">External data sources</span></span>
<span data-ttu-id="1cc94-142">A külső létrehozásához a ElasticDBQuery adatbázison végre az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="1cc94-142">To create an external data source, execute the following command on the ElasticDBQuery database:</span></span>

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 <span data-ttu-id="1cc94-143">"CustomerIDShardMap" shard leképezés neve esetén a shard térkép és shard térkép-kezelőt a rugalmas adatbázis eszközök minta létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1cc94-143">"CustomerIDShardMap" is the name of the shard map, if you created the shard map and shard map manager using the elastic database tools sample.</span></span> <span data-ttu-id="1cc94-144">Azonban ha az egyéni telepítő ezt a mintát használja, majd azt névnek kell lennie a shard térkép választja, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1cc94-144">However, if you used your custom setup for this sample, then it should be the shard map name you chose in your application.</span></span>

### <a name="external-tables"></a><span data-ttu-id="1cc94-145">Külső táblák</span><span class="sxs-lookup"><span data-stu-id="1cc94-145">External tables</span></span>
<span data-ttu-id="1cc94-146">Hozzon létre egy külső táblát, amely megfelel a szilánkok ügyfelek tábla a következő ElasticDBQuery adatbázis a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1cc94-146">Create an external table that matches the Customers table on the shards by executing the following command on ElasticDBQuery database:</span></span>

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="1cc94-147">Egy minta rugalmas adatbázis T-SQL-lekérdezés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="1cc94-147">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="1cc94-148">A külső adatforrást és a külső táblák meghatározása után a külső táblákon végrehajtott most már használhatja a teljes T-SQL.</span><span class="sxs-lookup"><span data-stu-id="1cc94-148">Once you have defined your external data source and your external tables you can now use full T-SQL over your external tables.</span></span>

<span data-ttu-id="1cc94-149">Hajtsa végre a lekérdezést a ElasticDBQuery adatbázison:</span><span class="sxs-lookup"><span data-stu-id="1cc94-149">Execute this query on the ElasticDBQuery database:</span></span>

    select count(CustomerId) from [dbo].[Customers]

<span data-ttu-id="1cc94-150">Megfigyelheti, hogy a lekérdezés eredményeit összesíti a szilánkok a, és lehetőséget ad a következő kimeneti:</span><span class="sxs-lookup"><span data-stu-id="1cc94-150">You will notice that the query aggregates results from all the shards and gives the following output:</span></span>

![Kimeneti részletei][4]

## <a name="import-elastic-database-query-results-to-excel"></a><span data-ttu-id="1cc94-152">A rugalmas adatbázis-lekérdezések eredményének Excel importálása</span><span class="sxs-lookup"><span data-stu-id="1cc94-152">Import elastic database query results to Excel</span></span>
 <span data-ttu-id="1cc94-153">Importálhatja az eredményeket a lekérdezés egy Excel-fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="1cc94-153">You can import the results from of a query to an Excel file.</span></span>

1. <span data-ttu-id="1cc94-154">Indítsa el az Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="1cc94-154">Launch Excel 2013.</span></span>
2. <span data-ttu-id="1cc94-155">Keresse meg a **adatok** menüszalagján.</span><span class="sxs-lookup"><span data-stu-id="1cc94-155">Navigate to the **Data** ribbon.</span></span>
3. <span data-ttu-id="1cc94-156">Kattintson a **egyéb forrásokból származó** kattintson **az SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1cc94-156">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Excel importálása más forrásokból][5]
4. <span data-ttu-id="1cc94-158">Az a **Adatkapcsolat varázsló** írja be a kiszolgáló nevét és a bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1cc94-158">In the **Data Connection Wizard** type the server name and login credentials.</span></span> <span data-ttu-id="1cc94-159">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="1cc94-159">Then click **Next**.</span></span>
5. <span data-ttu-id="1cc94-160">A párbeszédpanelen **válassza ki a kívánt adatokat tartalmazó adatbázis**, jelölje be a **ElasticDBQuery** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="1cc94-160">In the dialog box **Select the database that contains the data you want**, select the **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="1cc94-161">Válassza ki a **ügyfelek** táblázatban a lista nézetben, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="1cc94-161">Select the **Customers** table in the list view and click **Next**.</span></span> <span data-ttu-id="1cc94-162">Kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="1cc94-162">Then click **Finish**.</span></span>
7. <span data-ttu-id="1cc94-163">Az a **és adatokat importálhat** űrlap **válassza ki, hogy az adatok megtekintéséhez a munkafüzet**, jelölje be **tábla** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1cc94-163">In the **Import Data** form, under **Select how you want to view this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="1cc94-164">Összes sorát **ügyfelek** tábla különböző szilánkok tárolt feltölti az Excel-táblában.</span><span class="sxs-lookup"><span data-stu-id="1cc94-164">All the rows from **Customers** table, stored in different shards populate the Excel sheet.</span></span>

<span data-ttu-id="1cc94-165">Most már használhatja az Excel hatékony képi megjelenítés funkciók.</span><span class="sxs-lookup"><span data-stu-id="1cc94-165">You can now use Excel’s powerful data visualization functions.</span></span> <span data-ttu-id="1cc94-166">A kiszolgáló nevét, az adatbázis neve és a hitelesítő adatok adatbázishoz való kapcsolódáshoz a BI és az integráció eszközök a Rugalmas lekérdezési használhatja a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="1cc94-166">You can use the connection string with your server name, database name and credentials to connect your BI and data integration tools to the elastic query database.</span></span> <span data-ttu-id="1cc94-167">Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="1cc94-167">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="1cc94-168">A Rugalmas lekérdezési adatbázis és a külső táblák csakúgy, mint bármely más SQL Server-adatbázis és SQL Server-táblázatot, amely kíván csatlakozni, a eszközzel is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="1cc94-168">You can refer to the elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect to with your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="1cc94-169">Költségek</span><span class="sxs-lookup"><span data-stu-id="1cc94-169">Cost</span></span>
<span data-ttu-id="1cc94-170">Nem kell külön fizetni a rugalmas adatbázis-lekérdezés szolgáltatás használatára vonatkozó van.</span><span class="sxs-lookup"><span data-stu-id="1cc94-170">There is no additional charge for using the Elastic Database Query feature.</span></span>

<span data-ttu-id="1cc94-171">Díjszabási információkért lásd: [SQL adatbázis díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="1cc94-171">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1cc94-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cc94-172">Next steps</span></span>

* <span data-ttu-id="1cc94-173">Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-173">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="1cc94-174">Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="1cc94-174">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="1cc94-175">A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="1cc94-175">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="1cc94-176">A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="1cc94-176">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="1cc94-177">Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.</span><span class="sxs-lookup"><span data-stu-id="1cc94-177">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
