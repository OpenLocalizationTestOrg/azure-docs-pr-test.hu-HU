---
title: "az SQL Serverről az Azure SQL Data warehouse-ba (bcp) aaaLoad adatok |} Microsoft Docs"
description: "Kisebb adatméret esetében a bcp tooexport adatait az SQL Server tooflat fájlok és használja hello és adatokat importálhat közvetlenül az Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a03b5403d123e8814ae73a7cce8e6851c8b264a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a><span data-ttu-id="80461-103">Adatok betöltése az SQL Serverből az Azure SQL Data Warehouse-ba (egybesimított fájlok)</span><span class="sxs-lookup"><span data-stu-id="80461-103">Load data from SQL Server into Azure SQL Data Warehouse (flat files)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="80461-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="80461-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="80461-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="80461-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="80461-106">bcp</span><span class="sxs-lookup"><span data-stu-id="80461-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="80461-107">Kisebb adatkészletek használja az SQL Server hello bcp parancssori segédprogram tooexport adatait, és ezután töltse be tooAzure SQL Data Warehouse közvetlen.</span><span class="sxs-lookup"><span data-stu-id="80461-107">For small data sets, you can use hello bcp command-line utility tooexport data from SQL Server and then load it directly tooAzure SQL Data Warehouse.</span></span>

<span data-ttu-id="80461-108">Az oktatóanyag során a következőket hajtja végre a bcp segítségével:</span><span class="sxs-lookup"><span data-stu-id="80461-108">In this tutorial, you will use bcp to:</span></span>

* <span data-ttu-id="80461-109">Egy tábla exportálása az SQL Server hello bcp out paranccsal használatával (vagy hozzon létre egy egyszerű fájl)</span><span class="sxs-lookup"><span data-stu-id="80461-109">Export a table from from SQL Server by using hello bcp out command (or create a simple sample file)</span></span>
* <span data-ttu-id="80461-110">Hello tábla importálása egy egybesimított fájl tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="80461-110">Import hello table from a flat file tooSQL Data Warehouse.</span></span>
* <span data-ttu-id="80461-111">Statisztikák létrehozása hello betöltött adatokról.</span><span class="sxs-lookup"><span data-stu-id="80461-111">Create statistics on hello loaded data.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="80461-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="80461-112">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="80461-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="80461-113">Prerequisites</span></span>
<span data-ttu-id="80461-114">az oktatóanyag teljesítéséhez toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="80461-114">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="80461-115">Egy SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="80461-115">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="80461-116">hello telepített bcp parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="80461-116">hello bcp command-line utility installed</span></span>
* <span data-ttu-id="80461-117">hello telepített sqlcmd parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="80461-117">hello sqlcmd command-line utility installed</span></span>

<span data-ttu-id="80461-118">Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="80461-118">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="80461-119">Adatok ASCII vagy UTF-16 formátumban</span><span class="sxs-lookup"><span data-stu-id="80461-119">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="80461-120">Ha ez az oktatóanyag a saját adataival próbálja, az adatok kell toouse hello ASCII vagy UTF-16 kódolást, mert a bcp nem támogatja az UTF-8.</span><span class="sxs-lookup"><span data-stu-id="80461-120">If you are trying this tutorial with your own data, your data needs toouse hello ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

<span data-ttu-id="80461-121">A PolyBase támogatja az UTF-8 formátumot, de az UTF-16 formátumot még nem.</span><span class="sxs-lookup"><span data-stu-id="80461-121">PolyBase supports UTF-8 but doesn't yet support UTF-16.</span></span> <span data-ttu-id="80461-122">Vegye figyelembe, hogy ha azt szeretné, hogy a polybase-zel toocombine bcp szüksége lesz tootransform hello adatok tooUTF-8 után az SQL-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="80461-122">Note that if you want toocombine bcp with PolyBase you will need tootransform hello data tooUTF-8 after it is exported from SQL Server.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="80461-123">1. Céltábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="80461-123">1. Create a destination table</span></span>
<span data-ttu-id="80461-124">Adjon meg egy táblát az SQL Data Warehouse hello céltábla hello terhelést el.</span><span class="sxs-lookup"><span data-stu-id="80461-124">Define a table in SQL Data Warehouse that will be hello destination table for hello load.</span></span> <span data-ttu-id="80461-125">hello tábla oszlopainak hello toohello adatfájl egyes soraiban szereplő adatoknak kell megfelelnie.</span><span class="sxs-lookup"><span data-stu-id="80461-125">hello columns in hello table must correspond toohello data in each row of your data file.</span></span>

<span data-ttu-id="80461-126">toocreate tábla, nyisson meg egy parancssort, és az sqlcmd.exe toorun hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="80461-126">toocreate a table, open a command prompt and use sqlcmd.exe toorun hello following command:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="80461-127">2. Forrásadatfájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="80461-127">2. Create a source data file</span></span>
<span data-ttu-id="80461-128">Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új szöveges fájlba, és mentse a fájlt tooyour helyi ideiglenes könyvtárba C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="80461-128">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="80461-129">Ezek az adatok ASCII formátumban vannak.</span><span class="sxs-lookup"><span data-stu-id="80461-129">This data is in ASCII format.</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

<span data-ttu-id="80461-130">(Választható) tooexport a saját adatait egy SQL Server-adatbázisból, nyisson meg egy parancssort, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="80461-130">(Optional) tooexport your own data from a SQL Server database, open a command prompt and run hello following command.</span></span> <span data-ttu-id="80461-131">Cserélje le a TableName, ServerName, DatabaseName, Username és Password adatokat a saját adataira.</span><span class="sxs-lookup"><span data-stu-id="80461-131">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-hello-data"></a><span data-ttu-id="80461-132">3. Hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="80461-132">3. Load hello data</span></span>
<span data-ttu-id="80461-133">tooload hello adatok, nyisson meg egy parancssort, és futtassa a következő parancsot, a kiszolgáló nevét, a adatbázis nevét, a felhasználónév és a jelszó hello értékeket a saját cserélje le hello.</span><span class="sxs-lookup"><span data-stu-id="80461-133">tooload hello data, open a command prompt and run hello following command, replacing hello values for Server Name, Database name, Username, and Password with your own information.</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="80461-134">A parancs tooverify hello megfelelően be adatok használata</span><span class="sxs-lookup"><span data-stu-id="80461-134">Use this command tooverify hello data was loaded properly</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="80461-135">hello eredményeket kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="80461-135">hello results should look like this:</span></span>

| <span data-ttu-id="80461-136">DateId</span><span class="sxs-lookup"><span data-stu-id="80461-136">DateId</span></span> | <span data-ttu-id="80461-137">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="80461-137">CalendarQuarter</span></span> | <span data-ttu-id="80461-138">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="80461-138">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="80461-139">20150101</span><span class="sxs-lookup"><span data-stu-id="80461-139">20150101</span></span> |<span data-ttu-id="80461-140">1</span><span class="sxs-lookup"><span data-stu-id="80461-140">1</span></span> |<span data-ttu-id="80461-141">3</span><span class="sxs-lookup"><span data-stu-id="80461-141">3</span></span> |
| <span data-ttu-id="80461-142">20150201</span><span class="sxs-lookup"><span data-stu-id="80461-142">20150201</span></span> |<span data-ttu-id="80461-143">1</span><span class="sxs-lookup"><span data-stu-id="80461-143">1</span></span> |<span data-ttu-id="80461-144">3</span><span class="sxs-lookup"><span data-stu-id="80461-144">3</span></span> |
| <span data-ttu-id="80461-145">20150301</span><span class="sxs-lookup"><span data-stu-id="80461-145">20150301</span></span> |<span data-ttu-id="80461-146">1</span><span class="sxs-lookup"><span data-stu-id="80461-146">1</span></span> |<span data-ttu-id="80461-147">3</span><span class="sxs-lookup"><span data-stu-id="80461-147">3</span></span> |
| <span data-ttu-id="80461-148">20150401</span><span class="sxs-lookup"><span data-stu-id="80461-148">20150401</span></span> |<span data-ttu-id="80461-149">2</span><span class="sxs-lookup"><span data-stu-id="80461-149">2</span></span> |<span data-ttu-id="80461-150">4</span><span class="sxs-lookup"><span data-stu-id="80461-150">4</span></span> |
| <span data-ttu-id="80461-151">20150501</span><span class="sxs-lookup"><span data-stu-id="80461-151">20150501</span></span> |<span data-ttu-id="80461-152">2</span><span class="sxs-lookup"><span data-stu-id="80461-152">2</span></span> |<span data-ttu-id="80461-153">4</span><span class="sxs-lookup"><span data-stu-id="80461-153">4</span></span> |
| <span data-ttu-id="80461-154">20150601</span><span class="sxs-lookup"><span data-stu-id="80461-154">20150601</span></span> |<span data-ttu-id="80461-155">2</span><span class="sxs-lookup"><span data-stu-id="80461-155">2</span></span> |<span data-ttu-id="80461-156">4</span><span class="sxs-lookup"><span data-stu-id="80461-156">4</span></span> |
| <span data-ttu-id="80461-157">20150701</span><span class="sxs-lookup"><span data-stu-id="80461-157">20150701</span></span> |<span data-ttu-id="80461-158">3</span><span class="sxs-lookup"><span data-stu-id="80461-158">3</span></span> |<span data-ttu-id="80461-159">1</span><span class="sxs-lookup"><span data-stu-id="80461-159">1</span></span> |
| <span data-ttu-id="80461-160">20150801</span><span class="sxs-lookup"><span data-stu-id="80461-160">20150801</span></span> |<span data-ttu-id="80461-161">3</span><span class="sxs-lookup"><span data-stu-id="80461-161">3</span></span> |<span data-ttu-id="80461-162">1</span><span class="sxs-lookup"><span data-stu-id="80461-162">1</span></span> |
| <span data-ttu-id="80461-163">20150801</span><span class="sxs-lookup"><span data-stu-id="80461-163">20150801</span></span> |<span data-ttu-id="80461-164">3</span><span class="sxs-lookup"><span data-stu-id="80461-164">3</span></span> |<span data-ttu-id="80461-165">1</span><span class="sxs-lookup"><span data-stu-id="80461-165">1</span></span> |
| <span data-ttu-id="80461-166">20151001</span><span class="sxs-lookup"><span data-stu-id="80461-166">20151001</span></span> |<span data-ttu-id="80461-167">4</span><span class="sxs-lookup"><span data-stu-id="80461-167">4</span></span> |<span data-ttu-id="80461-168">2</span><span class="sxs-lookup"><span data-stu-id="80461-168">2</span></span> |
| <span data-ttu-id="80461-169">20151101</span><span class="sxs-lookup"><span data-stu-id="80461-169">20151101</span></span> |<span data-ttu-id="80461-170">4</span><span class="sxs-lookup"><span data-stu-id="80461-170">4</span></span> |<span data-ttu-id="80461-171">2</span><span class="sxs-lookup"><span data-stu-id="80461-171">2</span></span> |
| <span data-ttu-id="80461-172">20151201</span><span class="sxs-lookup"><span data-stu-id="80461-172">20151201</span></span> |<span data-ttu-id="80461-173">4</span><span class="sxs-lookup"><span data-stu-id="80461-173">4</span></span> |<span data-ttu-id="80461-174">2</span><span class="sxs-lookup"><span data-stu-id="80461-174">2</span></span> |

## <a name="4-create-statistics"></a><span data-ttu-id="80461-175">4. Statisztika létrehozása</span><span class="sxs-lookup"><span data-stu-id="80461-175">4. Create statistics</span></span>
<span data-ttu-id="80461-176">Az SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását vagy automatikus frissítését.</span><span class="sxs-lookup"><span data-stu-id="80461-176">SQL Data Warehouse does not yet support auto-create or auto-update statistics.</span></span> <span data-ttu-id="80461-177">tooget hello legjobb lekérdezési teljesítményt, akkor fontos toocreate statisztikákat a táblák összes oszlopához hello első betöltés után, vagy minden lényeges módosítását követően hello adataiban.</span><span class="sxs-lookup"><span data-stu-id="80461-177">tooget hello best query performance, it's important toocreate statistics on all columns of all tables after hello first load or after any substantial changes occur in hello data.</span></span> <span data-ttu-id="80461-178">A statisztika részletes ismertetése, [statisztika][Statistics].</span><span class="sxs-lookup"><span data-stu-id="80461-178">For a detailed explanation of statistics, see [Statistics][Statistics].</span></span> 

<span data-ttu-id="80461-179">Futtatási hello következő parancsot a toocreate statisztikák az újonnan betöltött táblákon.</span><span class="sxs-lookup"><span data-stu-id="80461-179">Run hello following command toocreate statistics on your newly loaded table.</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a><span data-ttu-id="80461-180">5. Adatok exportálása az SQL Data Warehouse-ból</span><span class="sxs-lookup"><span data-stu-id="80461-180">5. Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="80461-181">A Szórakozás exportálhatja a hello adatokat, vissza az SQL Data Warehouse kívül betöltött.</span><span class="sxs-lookup"><span data-stu-id="80461-181">For fun, you can export hello data that you just loaded back out of SQL Data Warehouse.</span></span>  <span data-ttu-id="80461-182">hello parancs tooexport pontosan hello ugyanaz, mint az SQL Server exportálása.</span><span class="sxs-lookup"><span data-stu-id="80461-182">hello command tooexport is exactly hello same as exporting from SQL Server.</span></span>

<span data-ttu-id="80461-183">Van azonban hello eredmények eltérőek lesznek.</span><span class="sxs-lookup"><span data-stu-id="80461-183">However, there is a difference in hello results.</span></span> <span data-ttu-id="80461-184">Mivel a hello adatok rendszer elosztott helyeken az SQL Data Warehouse, az adatok exportálásakor minden számítási csomópont böngészőbe írja azt toohello kimeneti adatfájlt.</span><span class="sxs-lookup"><span data-stu-id="80461-184">Since hello data is stored in distributed locations within SQL Data Warehouse, when you export data each Compute node writes it data toohello output file.</span></span> <span data-ttu-id="80461-185">hello hello hello kimeneti fájl adatainak sorrendje valószínűleg toobe hello hello hello bemeneti fájl adatainak sorrendje eltér.</span><span class="sxs-lookup"><span data-stu-id="80461-185">hello order of hello data in hello output file is likely toobe different than hello order of hello data in hello input file.</span></span>

### <a name="export-a-table-and-compare-exported-results"></a><span data-ttu-id="80461-186">Tábla exportálása és az exportált eredmények összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="80461-186">Export a table and compare exported results</span></span>
<span data-ttu-id="80461-187">toosee hello tegye elérhetővé az adatokat, nyisson meg egy parancssort, és futtassa a parancsot a saját paramétereivel.</span><span class="sxs-lookup"><span data-stu-id="80461-187">toosee hello exported data, open a command prompt and run this command using your own parameters.</span></span> <span data-ttu-id="80461-188">A kiszolgálónév annak az Azure logikai SQL Server hello neve.</span><span class="sxs-lookup"><span data-stu-id="80461-188">ServerName is hello name of your Azure logical SQL Server.</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="80461-189">Hello adatok helyes exportálását hello új fájl megnyitásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="80461-189">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="80461-190">hello fájl hello adatait meg kell felelnie az alábbi hello szöveg, de valószínűleg rendezhető más sorrendben:</span><span class="sxs-lookup"><span data-stu-id="80461-190">hello data in hello file should match hello text below, but will likely be sorted in a different order:</span></span>

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-hello-results-of-a-query"></a><span data-ttu-id="80461-191">A lekérdezés eredményeinek hello exportálása</span><span class="sxs-lookup"><span data-stu-id="80461-191">Export hello results of a query</span></span>
<span data-ttu-id="80461-192">Használhatja a hello **queryout** bcp tooexport hello hello egész tábla exportálása helyett a lekérdezés eredményeinek funkcióját.</span><span class="sxs-lookup"><span data-stu-id="80461-192">You can use hello **queryout** function of bcp tooexport hello results of a query instead of exporting hello entire table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="80461-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80461-193">Next steps</span></span>
<span data-ttu-id="80461-194">A betöltés áttekintése: [Adatok betöltése az SQL Data Warehouse-ba][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="80461-194">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="80461-195">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="80461-195">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="80461-196">Lásd: [tábla áttekintése] [ Table Overview] vagy [CREATE TABLE szintaxis] [ CREATE TABLE syntax] SQL Data Warehouse a táblázatok létrehozásáról további információt.</span><span class="sxs-lookup"><span data-stu-id="80461-196">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
