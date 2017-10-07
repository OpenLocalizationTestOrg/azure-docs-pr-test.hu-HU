---
title: "aaaBuild és optimalizálják az adatok gyors párhuzamos importálásakor táblák SQL-kiszolgálót egy Azure virtuális gépen |} Microsoft Docs"
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
ms.openlocfilehash: ab748c47348ec6ca3b98ba39e27181bba5d36fc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Párhuzamos tömeges adatimportálás SQL partíciós táblák használatával
Ez a dokumentum azt ismerteti, hogyan toobuild particionált táblák gyors párhuzamos tömeges importálásához adatok tooa SQL Server-adatbázis. Big Data típusú adatok betöltését/átvitel tooa SQL-adatbázis, az adatok toohello SQL-adatbázis és a lekérdezések importálása növelhető a használatával *particionált táblák és nézetek*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Hozzon létre egy új adatbázist és a fájlcsoport készlete
* [Hozzon létre egy új adatbázist](https://technet.microsoft.com/library/ms176061.aspx), ha már nem létezik.
* Adja hozzá az adatbázis fájlcsoportok toohello adatbázisban, ami particionálva hello fizikai fájlok tárolására. Ezt megteheti a [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) Ha új vagy [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) hello adatbázis már létezik.
* Adjon hozzá egy vagy több (szükség szerint) fájlok tooeach adatbázis fájlcsoport.
  
  > [!NOTE]
  > Adja meg a hello cél fájlcsoportban a partíció és hello fizikai adatbázis fájlnevek adatokat tároló hello fájlcsoport adatok tárolásához.
  > 
  > 

hello alábbi példa létrehoz egy új adatbázist az elsődleges hello és naplófájlcsoportok, az egyes egy fizikai fájlt tartalmazó három fájlcsoport. hello adatbázisfájlok hello alapértelmezett SQL Server-adatok mappában jönnek létre, a hello SQL Server-példány. További információ a hello alapértelmezett tárolási helyeit: [fájlhelyek az alapértelmezett és elnevezett példány az SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

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

## <a name="create-a-partitioned-table"></a>Particionált tábla létrehozása
Létrehozni a particionált toohello adatkulcsokat, hello előző lépésben létrehozott csatlakoztatott toohello adatbázis fájlcsoport szerint. Adatok importálása tömeges toohello particionált táblák, rekordok közötti elosztása hello fájlcsoport szerint tooa partícióséma, az alább ismertetett.

**a partíciós tábla toocreate, kell:**

* [A partíciós függvények létrehozása](https://msdn.microsoft.com/library/ms187802.aspx) értékek/határok toobe hello számos határozza meg, amely táblázatban szereplő minden egyes partíció, például toolimit partíciót hónaponként (néhány\_datetime\_mező) hello évben 2013:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )
* [Hozzon létre egy partícióséma](https://msdn.microsoft.com/library/ms179854.aspx) amely leképezhető minden partíciótartomány hello partíciós függvény tooa fizikai fájlcsoportban, pl.:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> too(
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  tooverify hello tartományok érvényben az egyes partícióazonosító függően toohello függvény/sémát, futtassa a következő lekérdezés hello:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>
* [Particionált tábla létrehozása](https://msdn.microsoft.com/library/ms174979.aspx)(s) szerint tooyour adatkulcsokat, és adja meg, hello partíciós séma és a korlátozás mező használt toopartition hello tábla, pl.:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

További információkért lásd: [particionált táblák létrehozása és indexek](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-hello-data-for-each-individual-partition-table"></a>Tömeges hello adatokat importálhat minden egyes partíció táblához
* Előfordulhat, hogy a BCP, TÖMEGES Beszúrás vagy más módszerrel például [SQL Server varázsló](http://sqlazuremw.codeplex.com/). a megadott hello példa hello BCP módszert használja.
* [Az ALTER hello database](https://msdn.microsoft.com/library/bb522682.aspx) toochange tranzakciók naplózása a rendszer tooBULK_LOGGED toominimize növeli a naplózás, pl.:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* tooexpedite Adatbetöltési, indítsa el a hello tömeges importálási műveletek párhuzamosan. A tömeges meggyorsítása tippek a big Data típusú adatok importálása az SQL Server-adatbázisok, lásd: [1 TB-os betölteni a kevesebb mint 1 óra](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

hello következő PowerShell-parancsfájl példája párhuzamos adatok betöltése a BCP segítségével.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # hello example assumes hello partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes hello input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set hello server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match hello number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name toobe loaded, basename of input data files, input format file, and number of partitions
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


## <a name="create-indexes-toooptimize-joins-and-query-performance"></a>Hozzon létre indexeket toooptimize illesztések és a lekérdezési teljesítmény
* Ha több tábla rendszer kinyeri az adatokat a modellezési, indexek létrehozása hello illesztési kulcsok tooimprove hello illesztési teljesítmény.
* [Indexek létrehozása](https://technet.microsoft.com/library/ms188783.aspx) (fürtözött vagy nem fürtözött) célcsoport-kezelési hello minden partíció esetében azonos fájlcsoport pl.:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  Vagy,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Úgy is dönthet, toocreate hello indexek tömeges hello adatok importálása előtt. Az index létrehozása előtt tömeges importálását lelassulnak hello adatok betöltésekor.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Speciális elemzésekre folyamat és a technológia művelet példa
Cortana Analytics folyamat hello használata a nyilvános dataset végpont forgatókönyv példáért lásd: [Cortana Analytics folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).

