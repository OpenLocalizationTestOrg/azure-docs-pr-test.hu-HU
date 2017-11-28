---
title: "(vízszintes particionálás) kiterjesztett felhő az adatbázisok közötti aaaReport |} Microsoft Docs"
description: "Hogyan toouse kereszt-adatbázis adatbázis-lekérdezések"
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
ms.openlocfilehash: e34f398f8d408cffd91a70fc2cfbda73daec3550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a><span data-ttu-id="3fdf6-103">Jelentés közötti kiterjesztett felhő (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="3fdf6-103">Report across scaled-out cloud databases (preview)</span></span>
<span data-ttu-id="3fdf6-104">A több Azure SQL-adatbázisok egyetlen kapcsolódási pont használatával jelentéseket hozhat létre egy [rugalmas lekérdezési](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-104">You can create reports from multiple Azure SQL databases from a single connection point using an [elastic query](sql-database-elastic-query-overview.md).</span></span> <span data-ttu-id="3fdf6-105">hello adatbázisok vízszintesen kell particionálni (más néven "szilánkos").</span><span class="sxs-lookup"><span data-stu-id="3fdf6-105">hello databases must be horizontally partitioned (also known as "sharded").</span></span>

<span data-ttu-id="3fdf6-106">Ha egy meglévő adatbázist, olvassa el [áttelepítése a meglévő adatbázis tooscaled kibővített adatbázis](sql-database-elastic-convert-to-use-elastic-tools.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-106">If you have an existing database, see [Migrating existing databases tooscaled-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).</span></span>

<span data-ttu-id="3fdf6-107">toounderstand hello SQL objektumok szükséges tooquery című [vízszintesen particionált az adatbázisok közötti lekérdezés](sql-database-elastic-query-horizontal-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-107">toounderstand hello SQL objects needed tooquery, see [Query across horizontally partitioned databases](sql-database-elastic-query-horizontal-partitioning.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fdf6-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3fdf6-108">Prerequisites</span></span>
<span data-ttu-id="3fdf6-109">Töltse le és futtassa a hello [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-109">Download and run hello [Getting started with Elastic Database tools sample](sql-database-elastic-scale-get-started.md).</span></span>

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a><span data-ttu-id="3fdf6-110">A shard térkép létrehozásához-kezelőt hello mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="3fdf6-110">Create a shard map manager using hello sample app</span></span>
<span data-ttu-id="3fdf6-111">Itt hoz létre a shard térképre manager együtt több szilánkok hello szilánkok be az adatok beszúrását követ.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-111">Here you will create a shard map manager along with several shards, followed by insertion of data into hello shards.</span></span> <span data-ttu-id="3fdf6-112">Ha véletlenül tooalready szilánkok telepítő szilánkos adatokkal rendelkezik rajtuk, hello lépéseket kihagyhatja, és helyezze át a következő szakaszban toohello.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-112">If you happen tooalready have shards setup with sharded data in them, you can skip hello following steps and move toohello next section.</span></span>

1. <span data-ttu-id="3fdf6-113">Hozza létre, és futtassa a hello **Ismerkedés a rugalmas adatbáziseszközöket** mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-113">Build and run hello **Getting started with Elastic Database tools** sample application.</span></span> <span data-ttu-id="3fdf6-114">Amíg hello szakasz 7. lépés hello lépésekkel [hello minta alkalmazás letöltése és futtatása](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-114">Follow hello steps until step 7 in hello section [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app).</span></span> <span data-ttu-id="3fdf6-115">A 7. lépés hello végén jelenik meg a következő parancssor hello:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-115">At hello end of Step 7, you will see hello following command prompt:</span></span>

    ![parancssor][1]
2. <span data-ttu-id="3fdf6-117">Hello parancsablakot, írja be az "1", és nyomja le az ENTER **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-117">In hello command window, type "1" and press **Enter**.</span></span> <span data-ttu-id="3fdf6-118">Hello shard térkép manager hoz létre, és két szilánkok toohello kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-118">This creates hello shard map manager, and adds two shards toohello server.</span></span> <span data-ttu-id="3fdf6-119">Ezután írja be a "3", és nyomja le az ENTER **Enter**; hello művelet megismétlése négy alkalommal.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-119">Then type "3" and press **Enter**; repeat hello action four times.</span></span> <span data-ttu-id="3fdf6-120">Ez a szilánkok minta adatsorok szúrja be.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-120">This inserts sample data rows in your shards.</span></span>
3. <span data-ttu-id="3fdf6-121">Hello [Azure-portálon](https://portal.azure.com) jelenítsen meg három új adatbázist a kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-121">hello [Azure portal](https://portal.azure.com) should show three new databases in your server:</span></span>

   ![A Visual Studio-jóváhagyás][2]

   <span data-ttu-id="3fdf6-123">Ezen a ponton a kereszt-adatbázis-lekérdezések hello Elastic Database ügyféloldali kódtáraknál keresztül támogatottak.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-123">At this point, cross-database queries are supported through hello Elastic Database client libraries.</span></span> <span data-ttu-id="3fdf6-124">Például hello parancsablakban használja a 4. lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-124">For example, use option 4 in hello command window.</span></span> <span data-ttu-id="3fdf6-125">hello egy lekérdezés eredményeként előálló több shard a rendszer mindig egy **UNION ALL** összes szilánkok hello eredményeinek.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-125">hello results from a multi-shard query are always a **UNION ALL** of hello results from all shards.</span></span>

   <span data-ttu-id="3fdf6-126">Hello a következő szakaszban létrehozhatunk egy minta adatbázis végponttal, amely támogatja a gazdagabb lekérdezése hello adatok szilánkok között.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-126">In hello next section, we create a sample database endpoint that supports richer querying of hello data across shards.</span></span>

## <a name="create-an-elastic-query-database"></a><span data-ttu-id="3fdf6-127">A lekérdezés rugalmas adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fdf6-127">Create an elastic query database</span></span>
1. <span data-ttu-id="3fdf6-128">Nyissa meg hello [Azure-portálon](https://portal.azure.com) , és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-128">Open hello [Azure portal](https://portal.azure.com) and log in.</span></span>
2. <span data-ttu-id="3fdf6-129">Hozzon létre egy új Azure SQL database hello ugyanarra a kiszolgálóra a shard beállításai.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-129">Create a new Azure SQL database in hello same server as your shard setup.</span></span> <span data-ttu-id="3fdf6-130">Név hello adatbázis "ElasticDBQuery."</span><span class="sxs-lookup"><span data-stu-id="3fdf6-130">Name hello database "ElasticDBQuery."</span></span>

    ![Azure-portál és a tarifacsomag][3]

    > [!NOTE]
    > <span data-ttu-id="3fdf6-132">használhat egy meglévő adatbázist.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-132">you can use an existing database.</span></span> <span data-ttu-id="3fdf6-133">Ha így tesz, nem lehet, hogy szeretné-e tooexecute hello szilánkok egyikét a lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-133">If you can do so, it must not be one of hello shards that you would like tooexecute your queries on.</span></span> <span data-ttu-id="3fdf6-134">Ez az adatbázis hello metaadatok objektumok egy rugalmas adatbázis-lekérdezés létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-134">This database will be used for creating hello metadata objects for an elastic database query.</span></span>
    >

## <a name="create-database-objects"></a><span data-ttu-id="3fdf6-135">Adatbázis-objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fdf6-135">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="3fdf6-136">Adatbázis-hatókörű főkulcs és a hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="3fdf6-136">Database-scoped master key and credentials</span></span>
<span data-ttu-id="3fdf6-137">Ezek a kulcsok használt tooconnect toohello shard térkép manager és a hello szilánkok:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-137">These are used tooconnect toohello shard map manager and hello shards:</span></span>

1. <span data-ttu-id="3fdf6-138">Nyissa meg az SQL Server Management Studio vagy Visual Studio SQL Server Data Tools összetevővel.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-138">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="3fdf6-139">Csatlakozás tooElasticDBQuery adatbázis, és hajtsa végre a következő T-SQL parancsokkal hello:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-139">Connect tooElasticDBQuery database and execute hello following T-SQL commands:</span></span>

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    <span data-ttu-id="3fdf6-140">"felhasználónév" és a "password" kell kell hello ugyanaz, mint a 6. lépésében használt bejelentkezési adatok [hello minta alkalmazás letöltése és futtatása](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) a [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-140">"username" and "password" should be hello same as login information used in step 6 of [Download and run hello sample app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [Getting started with elastic database tools](sql-database-elastic-scale-get-started.md).</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="3fdf6-141">Külső adatforrások</span><span class="sxs-lookup"><span data-stu-id="3fdf6-141">External data sources</span></span>
<span data-ttu-id="3fdf6-142">toocreate egy külső adatforrásból parancs következő hello ElasticDBQuery adatbázison hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-142">toocreate an external data source, execute hello following command on hello ElasticDBQuery database:</span></span>

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 <span data-ttu-id="3fdf6-143">"CustomerIDShardMap" esetén hello hello shard leképezés, ha létrehozott hello shard térkép és shard térkép hello rugalmas adatbázis eszközök minta-kezelőt.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-143">"CustomerIDShardMap" is hello name of hello shard map, if you created hello shard map and shard map manager using hello elastic database tools sample.</span></span> <span data-ttu-id="3fdf6-144">Azonban ha az egyéni telepítő ezt a mintát használja, majd kell hello shard leképezésnév választja, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-144">However, if you used your custom setup for this sample, then it should be hello shard map name you chose in your application.</span></span>

### <a name="external-tables"></a><span data-ttu-id="3fdf6-145">Külső táblák</span><span class="sxs-lookup"><span data-stu-id="3fdf6-145">External tables</span></span>
<span data-ttu-id="3fdf6-146">Hozzon létre egy külső táblát, amely megfelel a hello ügyfelek tábla hello szilánkok a parancs következő ElasticDBQuery adatbázison hello végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-146">Create an external table that matches hello Customers table on hello shards by executing hello following command on ElasticDBQuery database:</span></span>

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="3fdf6-147">Egy minta rugalmas adatbázis T-SQL-lekérdezés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="3fdf6-147">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="3fdf6-148">A külső adatforrást és a külső táblák meghatározása után a külső táblákon végrehajtott most már használhatja a teljes T-SQL.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-148">Once you have defined your external data source and your external tables you can now use full T-SQL over your external tables.</span></span>

<span data-ttu-id="3fdf6-149">Hello ElasticDBQuery adatbázison hajtsa végre a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-149">Execute this query on hello ElasticDBQuery database:</span></span>

    select count(CustomerId) from [dbo].[Customers]

<span data-ttu-id="3fdf6-150">Megfigyelheti, hogy hello lekérdezés az összes hello szilánkok összesíti az eredményeket, és lehetőséget ad a következő kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="3fdf6-150">You will notice that hello query aggregates results from all hello shards and gives hello following output:</span></span>

![Kimeneti részletei][4]

## <a name="import-elastic-database-query-results-tooexcel"></a><span data-ttu-id="3fdf6-152">A rugalmas adatbázis-lekérdezés eredménye tooExcel importálása</span><span class="sxs-lookup"><span data-stu-id="3fdf6-152">Import elastic database query results tooExcel</span></span>
 <span data-ttu-id="3fdf6-153">A lekérdezés tooan Excel fájl eredményei hello importálhatja.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-153">You can import hello results from of a query tooan Excel file.</span></span>

1. <span data-ttu-id="3fdf6-154">Indítsa el az Excel 2013.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-154">Launch Excel 2013.</span></span>
2. <span data-ttu-id="3fdf6-155">Keresse meg a toohello **adatok** menüszalagján.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-155">Navigate toohello **Data** ribbon.</span></span>
3. <span data-ttu-id="3fdf6-156">Kattintson a **egyéb forrásokból származó** kattintson **az SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-156">Click **From Other Sources** and click **From SQL Server**.</span></span>

   ![Excel importálása más forrásokból][5]
4. <span data-ttu-id="3fdf6-158">A hello **Adatkapcsolat varázsló** írja be a hello kiszolgálónév és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-158">In hello **Data Connection Wizard** type hello server name and login credentials.</span></span> <span data-ttu-id="3fdf6-159">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-159">Then click **Next**.</span></span>
5. <span data-ttu-id="3fdf6-160">Hello párbeszédpanelen **hello adatokat tartalmazó adatbázis válassza hello**, jelölje be hello **ElasticDBQuery** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-160">In hello dialog box **Select hello database that contains hello data you want**, select hello **ElasticDBQuery** database.</span></span>
6. <span data-ttu-id="3fdf6-161">Jelölje be hello **ügyfelek** hello listanézetben táblázatban, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-161">Select hello **Customers** table in hello list view and click **Next**.</span></span> <span data-ttu-id="3fdf6-162">Kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-162">Then click **Finish**.</span></span>
7. <span data-ttu-id="3fdf6-163">A hello **és adatokat importálhat** űrlap **válassza ki, hogy tooview ezeket az adatokat a munkafüzet**, jelölje be **tábla** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-163">In hello **Import Data** form, under **Select how you want tooview this data in your workbook**, select **Table** and click **OK**.</span></span>

<span data-ttu-id="3fdf6-164">Az összes sorát hello **ügyfelek** a különböző szilánkok tárolt tábla feltöltéséhez hello Excel-táblában.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-164">All hello rows from **Customers** table, stored in different shards populate hello Excel sheet.</span></span>

<span data-ttu-id="3fdf6-165">Most már használhatja az Excel hatékony képi megjelenítés funkciók.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-165">You can now use Excel’s powerful data visualization functions.</span></span> <span data-ttu-id="3fdf6-166">A kiszolgáló nevét, az adatbázisnév hello kapcsolati karakterlánc használatával, és a hitelesítő adatok tooconnect a BI és az integráció eszközök toohello rugalmas adatbázis lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-166">You can use hello connection string with your server name, database name and credentials tooconnect your BI and data integration tools toohello elastic query database.</span></span> <span data-ttu-id="3fdf6-167">Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-167">Make sure that SQL Server is supported as a data source for your tool.</span></span> <span data-ttu-id="3fdf6-168">Olvassa el a toohello rugalmas lekérdezési adatbázis és a külső táblák csakúgy, mint bármely más SQL Server-adatbázis és, hogy Ön kapcsolódnának toowith az eszköz az SQL Server-táblákra.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-168">You can refer toohello elastic query database and external tables just like any other SQL Server database and SQL Server tables that you would connect toowith your tool.</span></span>

### <a name="cost"></a><span data-ttu-id="3fdf6-169">Költségek</span><span class="sxs-lookup"><span data-stu-id="3fdf6-169">Cost</span></span>
<span data-ttu-id="3fdf6-170">Nem kell külön fizetni hello rugalmas adatbázis-lekérdezés szolgáltatás használatára vonatkozó van.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-170">There is no additional charge for using hello Elastic Database Query feature.</span></span>

<span data-ttu-id="3fdf6-171">Díjszabási információkért lásd: [SQL adatbázis díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-171">For pricing information see [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3fdf6-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3fdf6-172">Next steps</span></span>

* <span data-ttu-id="3fdf6-173">Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-173">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="3fdf6-174">Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).</span><span class="sxs-lookup"><span data-stu-id="3fdf6-174">For a vertical partitioning tutorial, see [Getting started with cross-database query (vertical partitioning)](sql-database-elastic-query-getting-started-vertical.md).</span></span>
* <span data-ttu-id="3fdf6-175">A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="3fdf6-175">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="3fdf6-176">A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="3fdf6-176">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="3fdf6-177">Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.</span><span class="sxs-lookup"><span data-stu-id="3fdf6-177">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
