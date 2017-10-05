---
title: "Hozza létre, és az adatok gyors párhuzamos importálásakor táblák optimalizálása egy SQL-kiszolgálóra, egy Azure virtuális gépen való |} Microsoft Docs"
description: "Párhuzamos tömeges adatimportálás SQL partíciós táblák használatával"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ff90fdb0-5bc7-49e8-aee7-678b54f901c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: aae4e4f59e76bf48b00a2ee92aedd7d5643ba91a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a><span data-ttu-id="ecba0-103">Párhuzamos tömeges adatimportálás SQL partíciós táblák használatával</span><span class="sxs-lookup"><span data-stu-id="ecba0-103">Parallel Bulk Data Import Using SQL Partition Tables</span></span>
<span data-ttu-id="ecba0-104">Ez a dokumentum ismerteti, hogyan hozhat létre az SQL Server-adatbázis adatok gyors párhuzamos tömeges importálását a particionált táblákat.</span><span class="sxs-lookup"><span data-stu-id="ecba0-104">This document describes how to build partitioned tables for fast parallel bulk importing of data to a SQL Server database.</span></span> <span data-ttu-id="ecba0-105">A big Data típusú adatok betöltését/átvitel SQL-adatbázishoz, az adatok importálása az SQL-adatbázis és a lekérdezések növelhető a *particionált táblák és nézetek*.</span><span class="sxs-lookup"><span data-stu-id="ecba0-105">For big data loading/transfer to a SQL database, importing data to the SQL DB and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a><span data-ttu-id="ecba0-106">Hozzon létre egy új adatbázist és a fájlcsoport készlete</span><span class="sxs-lookup"><span data-stu-id="ecba0-106">Create a new database and a set of filegroups</span></span>
* <span data-ttu-id="ecba0-107">[Hozzon létre egy új adatbázist](https://technet.microsoft.com/library/ms176061.aspx), ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ecba0-107">[Create a new database](https://technet.microsoft.com/library/ms176061.aspx), if it doesn't exist already.</span></span>
* <span data-ttu-id="ecba0-108">Adatbázis fájlcsoport hozzáadása az adatbázisban, ami a particionált fizikai fájlok tárolására. Ezt megteheti a [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) Ha új vagy [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) az adatbázis már létezik.</span><span class="sxs-lookup"><span data-stu-id="ecba0-108">Add database filegroups to the database which will hold the partitioned physical files.This can be done with [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) if new or [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) if the database exists already.</span></span>
* <span data-ttu-id="ecba0-109">Fájlokat adjon hozzá egy vagy több (szükség szerint) minden egyes adatbázis fájlcsoport.</span><span class="sxs-lookup"><span data-stu-id="ecba0-109">Add one or more files (as needed) to each database filegroup.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="ecba0-110">Adja meg a cél fájlcsoportban ehhez a partícióhoz adatokat tároló és a fizikai adatbázis fájl vagy fájlcsoport adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="ecba0-110">Specify the target filegroup which holds data for this partition and the physical database file name(s) where the filegroup data will be stored.</span></span>
  > 
  > 

<span data-ttu-id="ecba0-111">A következő példa egy új adatbázist hoz létre három kívül az elsődleges fájlcsoport és naplófájlcsoportok, az egyes egy fizikai fájlt tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="ecba0-111">The following example creates a new database with three filegroups other than the primary and log groups, containing one physical file in each.</span></span> <span data-ttu-id="ecba0-112">Az adatbázisfájlok be az SQL Server-példányt az alapértelmezett SQL Server-adatok mappa jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ecba0-112">The database files are created in the default SQL Server Data folder, as configured in the SQL Server instance.</span></span> <span data-ttu-id="ecba0-113">Az alapértelmezett fájlhelyek kapcsolatos további információkért lásd: [fájlhelyek alapértelmezett és az SQL Server példány nevű](https://msdn.microsoft.com/library/ms143547.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecba0-113">For more information about the default file locations, see [File Locations for Default and Named Instances of SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).</span></span>

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a><span data-ttu-id="ecba0-114">Particionált tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecba0-114">Create a partitioned table</span></span>
<span data-ttu-id="ecba0-115">Az adatok séma, a az előző lépésben létrehozott adatbázis fájlcsoportokat leképezve megfelelően particionált táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ecba0-115">Create partitioned table(s) according to the data schema, mapped to the database filegroups created in the previous step.</span></span> <span data-ttu-id="ecba0-116">Adatok importálása a particionált táblák tömeges esetén rekordok közötti elosztása a fájlcsoportokat megfelelően partícióséma, az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="ecba0-116">When data is bulk imported to the partitioned table(s), records will be distributed among the filegroups according to a partition scheme, as described below.</span></span>

<span data-ttu-id="ecba0-117">**A partíciós tábla létrehozásához kell:**</span><span class="sxs-lookup"><span data-stu-id="ecba0-117">**To create a partition table, you need to:**</span></span>

* <span data-ttu-id="ecba0-118">[A partíciós függvények létrehozása](https://msdn.microsoft.com/library/ms187802.aspx) amely megadja, hogy az értékek/határok szereplő minden egyes partíciós tábla, pl., partíciók korlátozása havonta tartományán (néhány\_datetime\_mező) 2013-ben:</span><span class="sxs-lookup"><span data-stu-id="ecba0-118">[Create a partition function](https://msdn.microsoft.com/library/ms187802.aspx) which defines the range of values/boundaries to be included in each individual partition table, e.g., to limit partitions by month(some\_datetime\_field) in the year 2013:</span></span>
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* <span data-ttu-id="ecba0-119">[Hozzon létre egy partícióséma](https://msdn.microsoft.com/library/ms179854.aspx) amely van leképezve a partíciós függvény minden egyes partíciótartomány fizikai fájlcsoport, pl.:</span><span class="sxs-lookup"><span data-stu-id="ecba0-119">[Create a partition scheme](https://msdn.microsoft.com/library/ms179854.aspx) which maps each partition range in the partition function to a physical filegroup, e.g.:</span></span>
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  <span data-ttu-id="ecba0-120">Mindegyik partíció, a függvény séma szerint érvényben lévő tartományok ellenőrzéséhez futtassa az alábbi lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="ecba0-120">To verify the ranges in effect in each partition according to the function/scheme, run the following query:</span></span>
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* <span data-ttu-id="ecba0-121">[Particionált tábla létrehozása](https://msdn.microsoft.com/library/ms174979.aspx)(s) az adatok séma alapján történik, és adja meg a tábla, pl. particionálásához használt partíciós séma és a korlátozás mező:</span><span class="sxs-lookup"><span data-stu-id="ecba0-121">[Create partitioned table](https://msdn.microsoft.com/library/ms174979.aspx)(s) according to your data schema, and specify the partition scheme and constraint field used to partition the table, e.g.:</span></span>
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

<span data-ttu-id="ecba0-122">További információkért lásd: [particionált táblák létrehozása és indexek](https://msdn.microsoft.com/library/ms188730.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecba0-122">For more information, see [Create Partitioned Tables and Indexes](https://msdn.microsoft.com/library/ms188730.aspx).</span></span>

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a><span data-ttu-id="ecba0-123">Tömeges importálásához minden egyes partíció tábla adatai</span><span class="sxs-lookup"><span data-stu-id="ecba0-123">Bulk import the data for each individual partition table</span></span>
* <span data-ttu-id="ecba0-124">Előfordulhat, hogy a BCP, TÖMEGES Beszúrás vagy más módszerrel például [SQL Server varázsló](http://sqlazuremw.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="ecba0-124">You may use BCP, BULK INSERT, or other methods such as [SQL Server Migration Wizard](http://sqlazuremw.codeplex.com/).</span></span> <span data-ttu-id="ecba0-125">A megadott példa a BCP módszert használja.</span><span class="sxs-lookup"><span data-stu-id="ecba0-125">The example provided uses the BCP method.</span></span>
* <span data-ttu-id="ecba0-126">[Az adatbázis módosítására](https://msdn.microsoft.com/library/bb522682.aspx) tranzakció naplózási séma átállítása tömegesen_naplózott naplózás, pl. terhek minimalizálása érdekében:</span><span class="sxs-lookup"><span data-stu-id="ecba0-126">[Alter the database](https://msdn.microsoft.com/library/bb522682.aspx) to change transaction logging scheme to BULK_LOGGED to minimize overhead of logging, e.g.:</span></span>
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* <span data-ttu-id="ecba0-127">Adatbetöltési elősegítésére, nyissa meg a tömeges importálási műveletek párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="ecba0-127">To expedite data loading, launch the bulk import operations in parallel.</span></span> <span data-ttu-id="ecba0-128">A tömeges meggyorsítása tippek a big Data típusú adatok importálása az SQL Server-adatbázisok, lásd: [1 TB-os betölteni a kevesebb mint 1 óra](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).</span><span class="sxs-lookup"><span data-stu-id="ecba0-128">For tips on expediting bulk importing of big data into SQL Server databases, see [Load 1TB in less than 1 hour](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).</span></span>

<span data-ttu-id="ecba0-129">A következő PowerShell-parancsfájl párhuzamos adatok betöltése a BCP segítségével példája.</span><span class="sxs-lookup"><span data-stu-id="ecba0-129">The following PowerShell script is an example of parallel data loading using BCP.</span></span>

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a><span data-ttu-id="ecba0-130">Illesztések és a lekérdezési teljesítmény optimalizálása érdekében indexek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecba0-130">Create indexes to optimize joins and query performance</span></span>
* <span data-ttu-id="ecba0-131">Ha több tábla rendszer kinyeri az adatokat a modellezési, indexek létrehozása az illesztés kulcsok illesztési teljesítményének javítása.</span><span class="sxs-lookup"><span data-stu-id="ecba0-131">If you will extract data for modeling from multiple tables, create indexes on the join keys to improve the join performance.</span></span>
* <span data-ttu-id="ecba0-132">[Indexek létrehozása](https://technet.microsoft.com/library/ms188783.aspx) (fürtözött vagy nem fürtözött) célcsoport-kezelési minden partíció esetében azonos fájlcsoport a pl.:</span><span class="sxs-lookup"><span data-stu-id="ecba0-132">[Create indexes](https://technet.microsoft.com/library/ms188783.aspx) (clustered or non-clustered) targeting the same filegroup for each partition, for e.g.:</span></span>
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  <span data-ttu-id="ecba0-133">Vagy,</span><span class="sxs-lookup"><span data-stu-id="ecba0-133">or,</span></span>
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > <span data-ttu-id="ecba0-134">Előfordulhat, hogy az adatok importálása tömeges előtt indexek létrehozása mellett dönt.</span><span class="sxs-lookup"><span data-stu-id="ecba0-134">You may choose to create the indexes before bulk importing the data.</span></span> <span data-ttu-id="ecba0-135">Az index létrehozása előtt tömeges importálását lelassítja az adatok betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="ecba0-135">Index creation before bulk importing will slow down the data loading.</span></span>
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a><span data-ttu-id="ecba0-136">Speciális elemzésekre folyamat és a technológia művelet példa</span><span class="sxs-lookup"><span data-stu-id="ecba0-136">Advanced Analytics Process and Technology in Action Example</span></span>
<span data-ttu-id="ecba0-137">A Cortana Analytics folyamat használata a nyilvános dataset végpont forgatókönyv példáért lásd: [Cortana Analytics folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="ecba0-137">For an end-to-end walkthrough example using the Cortana Analytics Process with a public dataset, see [Cortana Analytics Process in Action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

