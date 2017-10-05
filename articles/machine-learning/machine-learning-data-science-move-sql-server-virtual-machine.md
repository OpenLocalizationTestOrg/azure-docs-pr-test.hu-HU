---
title: "Adatok áthelyezése az SQL Server Azure virtuális géphez |} Microsoft Docs"
description: "Helyezze át adatok strukturálatlan fájlból, vagy egy helyi SQL Server-kiszolgáló SQL Server Azure virtuális gépen."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 2c9ef1d3-4f5c-4b1f-bf06-223646c8af06
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: aa0cbba6bdb4cfdfe6ceee50c94f706aa0974924
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a><span data-ttu-id="c1a69-103">Adatok áthelyezés SQL Server-kiszolgálóra Azure-beli virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="c1a69-103">Move data to SQL Server on an Azure virtual machine</span></span>
<span data-ttu-id="c1a69-104">Ez a témakör bemutatja a beállítások áthelyezése adatok strukturálatlan fájlból (CSV vagy TSV formátumban), vagy egy helyi SQL Server-kiszolgáló SQL Server Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="c1a69-104">This topic outlines the options for moving data either from flat files (CSV or TSV formats) or from an on-premises SQL Server to SQL Server on an Azure virtual machine.</span></span> <span data-ttu-id="c1a69-105">Ezeket a feladatokat az adatok áthelyezését a felhőbe az Team tudományos folyamat részét képezik.</span><span class="sxs-lookup"><span data-stu-id="c1a69-105">These tasks for moving data to the cloud are part of the Team Data Science Process.</span></span>

<span data-ttu-id="c1a69-106">Ez a témakör ismerteti az adatok áthelyezése a Machine Learning Azure SQL adatbázis beállításait, lásd: [adatok áthelyezése az Azure SQL Database az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c1a69-106">For a topic that outlines the options for moving data to an Azure SQL Database for Machine Learning, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

<span data-ttu-id="c1a69-107">A **menü** alatti leíró betöltik az adatokat más cél környezetekben, ahol az adatok is tárolhatók és feldolgozhatók, a csapat adatok tudományos folyamat (TDSP) során témakörökre mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1a69-107">The **menu** below links to topics that describe how to ingest data into other target environments where the data can be stored and processed during the Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="c1a69-108">A következő táblázat összefoglalja a beállítások érhetők el adatok áthelyezése az SQL Server Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="c1a69-108">The following table summarizes the options for moving data to SQL Server on an Azure virtual machine.</span></span>

| <span data-ttu-id="c1a69-109"><b>FORRÁS</b></span><span class="sxs-lookup"><span data-stu-id="c1a69-109"><b>SOURCE</b></span></span> | <span data-ttu-id="c1a69-110"><b>CÉL: SQL Server Azure virtuális gépen</b></span><span class="sxs-lookup"><span data-stu-id="c1a69-110"><b>DESTINATION: SQL Server on Azure VM</b></span></span> |
| --- | --- |
| <span data-ttu-id="c1a69-111"><b>Egybesimított fájl</b></span><span class="sxs-lookup"><span data-stu-id="c1a69-111"><b>Flat File</b></span></span> |<span data-ttu-id="c1a69-112">1. <a href="#insert-tables-bcp">Parancssori tömeges másolási segédprogram (BCP)</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-112">1. <a href="#insert-tables-bcp">Command-line bulk copy utility (BCP) </a></span></span><br> <span data-ttu-id="c1a69-113">2. <a href="#insert-tables-bulkquery">A tömeges beszúrás SQL-lekérdezés</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-113">2. <a href="#insert-tables-bulkquery">Bulk Insert SQL Query </a></span></span><br> <span data-ttu-id="c1a69-114">3. <a href="#sql-builtin-utilities">Az SQL Server grafikus beépített segédprogramok</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-114">3. <a href="#sql-builtin-utilities">Graphical Built-in Utilities in SQL Server</a></span></span> |
| <span data-ttu-id="c1a69-115"><b>A helyszíni SQL Server</b></span><span class="sxs-lookup"><span data-stu-id="c1a69-115"><b>On-Premises SQL Server</b></span></span> |<span data-ttu-id="c1a69-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">SQL Server-adatbázis telepítése a Microsoft Azure virtuális gép varázsló</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy a SQL Server Database to a Microsoft Azure VM wizard</a></span></span><br> <span data-ttu-id="c1a69-117">2. <a href="#export-flat-file">Egybesimított fájl exportálása</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-117">2. <a href="#export-flat-file">Export to a flat File </a></span></span><br> <span data-ttu-id="c1a69-118">3. <a href="#sql-migration">SQL-adatbázis áttelepítése varázsló</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-118">3. <a href="#sql-migration">SQL Database Migration Wizard </a></span></span> <br> <span data-ttu-id="c1a69-119">4. <a href="#sql-backup">Adatbázis biztonsági mentése és visszaállítása</a></span><span class="sxs-lookup"><span data-stu-id="c1a69-119">4. <a href="#sql-backup">Database back up and restore </a></span></span><br> |

<span data-ttu-id="c1a69-120">Figyelje meg, hogy a jelen dokumentum céljából feltételezzük, hogy az SQL-parancsok végrehajtott SQL Server Management Studio vagy Visual Studio adatbázis-kezelő.</span><span class="sxs-lookup"><span data-stu-id="c1a69-120">Note that this document assumes that SQL commands are executed from SQL Server Management Studio or Visual Studio Database Explorer.</span></span>

> [!TIP]
> <span data-ttu-id="c1a69-121">Alternatív megoldásként használható [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) lehet létrehozni és ütemezni egy folyamatot, amely az Azure data áthelyezi egy SQL Server virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="c1a69-121">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to create and schedule a pipeline that will move data to a SQL Server VM on Azure.</span></span> <span data-ttu-id="c1a69-122">További információkért lásd: [Azure Data Factory (másolási tevékenység) adatok másolása](../data-factory/data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c1a69-122">For more information, see [Copy data with Azure Data Factory (Copy Activity)](../data-factory/data-factory-data-movement-activities.md).</span></span>
>
>

## <span data-ttu-id="c1a69-123"><a name="prereqs"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1a69-123"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="c1a69-124">Ez az oktatóanyag feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="c1a69-124">This tutorial assumes you have:</span></span>

* <span data-ttu-id="c1a69-125">Egy **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c1a69-125">An **Azure subscription**.</span></span> <span data-ttu-id="c1a69-126">Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1a69-126">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c1a69-127">Egy **Azure storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="c1a69-127">An **Azure storage account**.</span></span> <span data-ttu-id="c1a69-128">Szüksége lesz egy Azure storage-fiók ebben az oktatóanyagban az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="c1a69-128">You will use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="c1a69-129">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk.</span><span class="sxs-lookup"><span data-stu-id="c1a69-129">If you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="c1a69-130">Miután létrehozta a tárfiókot, szüksége lesz a tároló elérésére használt fiókot kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="c1a69-130">After you have created the storage account, you will need to obtain the account key used to access the storage.</span></span> <span data-ttu-id="c1a69-131">Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="c1a69-131">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="c1a69-132">Kiépített **egy Azure virtuális Gépen található SQL-kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="c1a69-132">Provisioned **SQL Server on an Azure VM**.</span></span> <span data-ttu-id="c1a69-133">Útmutatásért lásd: [állítsa be az Azure SQL Server virtuális gép speciális elemzésekre IPython Notebook kiszolgálóként](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="c1a69-133">For instructions, see [Set up an Azure SQL Server virtual machine as an IPython Notebook server for advanced analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span></span>
* <span data-ttu-id="c1a69-134">Telepített és konfigurált **Azure PowerShell** helyileg.</span><span class="sxs-lookup"><span data-stu-id="c1a69-134">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="c1a69-135">Útmutatásért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c1a69-135">For instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="c1a69-136"><a name="filesource_to_sqlonazurevm"></a>Adatok áthelyezése egy egybesimított fájl forrásból SQL Server egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="c1a69-136"><a name="filesource_to_sqlonazurevm"></a> Moving data from a flat file source to SQL Server on an Azure VM</span></span>
<span data-ttu-id="c1a69-137">Ha az adatok egy egyszerű fájlban (rendezett sorhoz/oszlophoz formátumban), akkor helyezheti át SQL Server rendszerű virtuális az Azure-on keresztül az alábbi módszerek:</span><span class="sxs-lookup"><span data-stu-id="c1a69-137">If your data is in a flat file (arranged in a row/column format), it can be moved to SQL Server VM on Azure via the following methods:</span></span>

1. [<span data-ttu-id="c1a69-138">Parancssori tömeges másolási segédprogram (BCP)</span><span class="sxs-lookup"><span data-stu-id="c1a69-138">Command-line bulk copy utility (BCP)</span></span>](#insert-tables-bcp)
2. [<span data-ttu-id="c1a69-139">A tömeges beszúrás SQL-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="c1a69-139">Bulk Insert SQL Query </span></span>](#insert-tables-bulkquery)
3. [<span data-ttu-id="c1a69-140">Az SQL Server (importálási/exportálási, SSIS) grafikus beépített segédprogramok</span><span class="sxs-lookup"><span data-stu-id="c1a69-140">Graphical Built-in Utilities in SQL Server (Import/Export, SSIS)</span></span>](#sql-builtin-utilities)

### <span data-ttu-id="c1a69-141"><a name="insert-tables-bcp"></a>Parancssori tömeges másolási segédprogram (BCP)</span><span class="sxs-lookup"><span data-stu-id="c1a69-141"><a name="insert-tables-bcp"></a>Command-line bulk copy utility (BCP)</span></span>
<span data-ttu-id="c1a69-142">BCP parancssori segédprogram SQL Server telepített, és egyik áthelyezni az adatokat a leggyorsabb módja.</span><span class="sxs-lookup"><span data-stu-id="c1a69-142">BCP is a command-line utility installed with SQL Server and is one of the quickest ways to move data.</span></span> <span data-ttu-id="c1a69-143">Minden három SQL Server változat (a helyszíni SQL Server, SQL Azure és SQL Server Azure virtuális gép) is használható.</span><span class="sxs-lookup"><span data-stu-id="c1a69-143">It works across all three SQL Server variants (On-premises SQL Server, SQL Azure and SQL Server VM on Azure).</span></span>

> [!NOTE]
> <span data-ttu-id="c1a69-144">**Az adatok hol kell lennie a BCP?**</span><span class="sxs-lookup"><span data-stu-id="c1a69-144">**Where should my data be for BCP?**</span></span>  
> <span data-ttu-id="c1a69-145">Bár ez nem kötelező, és a cél az SQL Server ugyanazon a számítógépen található forrás adatokat tartalmazó fájlok segítségével gyorsabb átvitelek (hálózati sebesség és a helyi lemez IO sebessége).</span><span class="sxs-lookup"><span data-stu-id="c1a69-145">While it is not required, having files containing source data located on the same machine as the target SQL Server allows for faster transfers (network speed vs local disk IO speed).</span></span> <span data-ttu-id="c1a69-146">Áthelyezheti a egybesimított fájlokba, a gép adatokat tartalmazó adott SQL Server használatával van telepítve, mint eszközök különböző fájlok másolása [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Tártallózó](http://storageexplorer.com/) vagy a windows távoli asztalon keresztül, másolási és beillesztési Protokoll (RDP).</span><span class="sxs-lookup"><span data-stu-id="c1a69-146">You can move the flat files containing data to the machine where SQL Server is installed using various file copying tools such as [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) or windows copy/paste via Remote Desktop Protocol (RDP).</span></span>
>
>

1. <span data-ttu-id="c1a69-147">Győződjön meg arról, hogy a célként megadott SQL Server-adatbázis az adatbázis és a táblázatok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="c1a69-147">Ensure that the database and the tables are created on the target SQL Server database.</span></span> <span data-ttu-id="c1a69-148">Nem adott használatával például a `Create Database` és `Create Table` parancsokat:</span><span class="sxs-lookup"><span data-stu-id="c1a69-148">Here is an example of how to do that using the `Create Database` and `Create Table` commands:</span></span>

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. <span data-ttu-id="c1a69-149">Hozza létre a formátumú fájlt, amely leírja a séma a következő táblázatban a következő parancsot a parancssorból a gép bcp futtató.</span><span class="sxs-lookup"><span data-stu-id="c1a69-149">Generate the format file that describes the schema for the table by issuing the following command from the command-line of the machine where bcp is installed.</span></span>

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. <span data-ttu-id="c1a69-150">Helyezze be az adatokat az adatbázisba, az alábbiak szerint a bcp paranccsal.</span><span class="sxs-lookup"><span data-stu-id="c1a69-150">Insert the data into the database using the bcp command as follows.</span></span> <span data-ttu-id="c1a69-151">Feltételezve, hogy az SQL Server ugyanezen a gépen telepítve van a kell működnie parancsot a parancssorból:</span><span class="sxs-lookup"><span data-stu-id="c1a69-151">This should work from the command-line assuming that the SQL Server is installed on same machine:</span></span>

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> <span data-ttu-id="c1a69-152">**Optimalizálja a BCP beszúrása** tekintse meg a következő cikk ["Tömeges importálással optimalizálása útmutatás"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) ilyen Beszúrások optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="c1a69-152">**Optimizing BCP Inserts** Please refer the following article ['Guidelines for Optimizing Bulk Import'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) to optimize such inserts.</span></span>
>
>

### <span data-ttu-id="c1a69-153"><a name="insert-tables-bulkquery-parallel"></a>Gyorsabb adatátvitelt jelölik a Beszúrás parallelizing</span><span class="sxs-lookup"><span data-stu-id="c1a69-153"><a name="insert-tables-bulkquery-parallel"></a>Parallelizing Inserts for Faster Data Movement</span></span>
<span data-ttu-id="c1a69-154">Ha az adatokat helyez át nagy, felgyorsíthatja dolgot egy PowerShell-parancsfájlt párhuzamosan több BCP parancsok egyidejűleg végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="c1a69-154">If the data you are moving is large, you can speed things up by simultaneously executing multiple BCP commands in parallel in a PowerShell Script.</span></span>

> [!NOTE]
> <span data-ttu-id="c1a69-155">**Big Data típusú adatok adatfeldolgozást** nagy és nagyon nagy adatkészletek betöltése adatok optimalizálása érdekében partícióazonosító a logikai és fizikai adatbázis táblák fájlcsoportok és a partíció táblákat.</span><span class="sxs-lookup"><span data-stu-id="c1a69-155">**Big data Ingestion** To optimize data loading for large and very large datasets, partition your logical and physical database tables using multiple filegroups and partition tables.</span></span> <span data-ttu-id="c1a69-156">Létrehozásával és a partíciós táblákba adatok betöltése kapcsolatos további információkért lásd: [párhuzamos terhelés SQL partíciós táblákba](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="c1a69-156">For more information about creating and loading data to partition tables, see [Parallel Load SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
>
>

<span data-ttu-id="c1a69-157">A PowerShell-parancsfájlpélda az alábbi párhuzamos Beszúrás a bcp bemutatása:</span><span class="sxs-lookup"><span data-stu-id="c1a69-157">The sample PowerShell script below demonstrate parallel inserts using bcp:</span></span>

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n
      }


    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }


    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <span data-ttu-id="c1a69-158"><a name="insert-tables-bulkquery"></a>A tömeges beszúrás SQL-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="c1a69-158"><a name="insert-tables-bulkquery"></a>Bulk Insert SQL Query</span></span>
<span data-ttu-id="c1a69-159">[Tömeges beszúrás SQL-lekérdezés](https://msdn.microsoft.com/library/ms188365) is használható, hogy adatokat importáljon belőlük az adatbázist a sorhoz/oszlophoz alapú fájlok (lásd a támogatott típusok: a[tömeges exportálása és importálása (SQL Server) előkészítése adatait](https://msdn.microsoft.com/library/ms188609)) témakör.</span><span class="sxs-lookup"><span data-stu-id="c1a69-159">[Bulk Insert SQL Query](https://msdn.microsoft.com/library/ms188365) can be used to import data into the database from row/column based files (the supported types are covered in the[Prepare Data for Bulk Export or Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)) topic.</span></span>

<span data-ttu-id="c1a69-160">Az alábbiakban néhány Példaparancsok tömeges Beszúrás az alábbiak szerint vannak:</span><span class="sxs-lookup"><span data-stu-id="c1a69-160">Here are some sample commands for Bulk Insert are as below:</span></span>  

1. <span data-ttu-id="c1a69-161">Az adatok elemzéséhez, és győződjön meg arról, hogy az SQL Server-adatbázis azt feltételezi, hogy ugyanazt a formátumot dátumok például különleges mezőket az importálás előtt minden egyéni beállítások megadása.</span><span class="sxs-lookup"><span data-stu-id="c1a69-161">Analyze your data and set any custom options before importing to make sure that the SQL Server database assumes the same format for any special fields such as dates.</span></span> <span data-ttu-id="c1a69-162">Íme egy példa bemutatja, hogyan beállítani a dátumformátum év, hónap-nap (ha az adatok a dátum az év, hónap-nap formátumban):</span><span class="sxs-lookup"><span data-stu-id="c1a69-162">Here is an example of how to set the date format as year-month-day (if your data contains the date in year-month-day format):</span></span>

        SET DATEFORMAT ymd;    
2. <span data-ttu-id="c1a69-163">Importálja az adatokat, és tömeges importálási utasítást:</span><span class="sxs-lookup"><span data-stu-id="c1a69-163">Import data using bulk import statements:</span></span>

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )

### <span data-ttu-id="c1a69-164"><a name="sql-builtin-utilities"></a>Az SQL Server beépített segédprogramok</span><span class="sxs-lookup"><span data-stu-id="c1a69-164"><a name="sql-builtin-utilities"></a>Built-in Utilities in SQL Server</span></span>
<span data-ttu-id="c1a69-165">SQL Server integrációja Services (SSIS) segítségével az adatok importálása egy egybesimított fájlból Azure SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c1a69-165">You can use SQL Server Integrations Services (SSIS) to import data into SQL Server VM on Azure from a flat file.</span></span>
<span data-ttu-id="c1a69-166">SSIS két studio környezetekben érhető el.</span><span class="sxs-lookup"><span data-stu-id="c1a69-166">SSIS is available in two studio environments.</span></span> <span data-ttu-id="c1a69-167">További információkért lásd: [Integration Services (SSIS) és a Studio környezetek](https://technet.microsoft.com/library/ms140028.aspx):</span><span class="sxs-lookup"><span data-stu-id="c1a69-167">For details, see [Integration Services (SSIS) and Studio Environments](https://technet.microsoft.com/library/ms140028.aspx):</span></span>

* <span data-ttu-id="c1a69-168">További részletek az SQL Server Data Tools: [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1a69-168">For details on SQL Server Data Tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span></span>  
* <span data-ttu-id="c1a69-169">További részletek az Import/Export varázsló: [SQL Server importálása és exportálása varázsló](https://msdn.microsoft.com/library/ms141209.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1a69-169">For details on the Import/Export Wizard, see [SQL Server Import and Export Wizard](https://msdn.microsoft.com/library/ms141209.aspx)</span></span>

## <span data-ttu-id="c1a69-170"><a name="sqlonprem_to_sqlonazurevm"></a>Adatok áthelyezése a helyszíni SQL Server SQL Server egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="c1a69-170"><a name="sqlonprem_to_sqlonazurevm"></a>Moving Data from on-premises SQL Server to SQL Server on an Azure VM</span></span>
<span data-ttu-id="c1a69-171">A következő áttelepítési stratégia is használhatja:</span><span class="sxs-lookup"><span data-stu-id="c1a69-171">You can also use the following migration strategies:</span></span>

1. [<span data-ttu-id="c1a69-172">SQL Server-adatbázis telepítése a Microsoft Azure virtuális gép varázsló</span><span class="sxs-lookup"><span data-stu-id="c1a69-172">Deploy a SQL Server Database to a Microsoft Azure VM wizard</span></span>](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [<span data-ttu-id="c1a69-173">Egybesimított fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="c1a69-173">Export to Flat File</span></span>](#export-flat-file)
3. [<span data-ttu-id="c1a69-174">SQL-adatbázis áttelepítése varázsló</span><span class="sxs-lookup"><span data-stu-id="c1a69-174">SQL Database Migration Wizard</span></span>](#sql-migration)
4. [<span data-ttu-id="c1a69-175">Adatbázis biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="c1a69-175">Database back up and restore</span></span>](#sql-backup)

<span data-ttu-id="c1a69-176">Azt mutatják be ezen az alábbi:</span><span class="sxs-lookup"><span data-stu-id="c1a69-176">We describe each of these below:</span></span>

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a><span data-ttu-id="c1a69-177">SQL Server-adatbázis telepítése a Microsoft Azure virtuális gép varázsló</span><span class="sxs-lookup"><span data-stu-id="c1a69-177">Deploy a SQL Server Database to a Microsoft Azure VM wizard</span></span>
<span data-ttu-id="c1a69-178">A **központi telepítése az SQL Server-adatbázis a Microsoft Azure virtuális gép varázsló** egy egyszerű és ajánlott módja a tárolt adatok mozgatása a helyszíni SQL Server-példány SQL Server egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c1a69-178">The **Deploy a SQL Server Database to a Microsoft Azure VM wizard** is a simple and recommended way to move data from an on-premises SQL Server instance to SQL Server on an Azure VM.</span></span> <span data-ttu-id="c1a69-179">Részletes lépéseit, valamint az egyéb alternatívák döntéseken, lásd: [adatbázis áttelepítése SQL Server egy Azure virtuális gépen](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span><span class="sxs-lookup"><span data-stu-id="c1a69-179">For detailed steps as well as a discussion of other alternatives, see [Migrate a database to SQL Server on an Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span></span>

### <span data-ttu-id="c1a69-180"><a name="export-flat-file"></a>Egybesimított fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="c1a69-180"><a name="export-flat-file"></a>Export to Flat File</span></span>
<span data-ttu-id="c1a69-181">Különböző módszer használható tömeges adatok exportálása egy helyszíni SQL Server, ahogy a [tömeges adatok importálása és exportálása a (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="c1a69-181">Various methods can be used to bulk export data from an On-Premises SQL Server as documented in the [Bulk Import and Export of Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) topic.</span></span> <span data-ttu-id="c1a69-182">Ez a dokumentum a tömeges másolási Program (BCP) szakaszában példaként.</span><span class="sxs-lookup"><span data-stu-id="c1a69-182">This document will cover the Bulk Copy Program (BCP) as an example.</span></span> <span data-ttu-id="c1a69-183">Amennyiben az adatok strukturálatlan fájlba exportálása, hogy importálni lehessen egy másik SQL Server tömeges importálással.</span><span class="sxs-lookup"><span data-stu-id="c1a69-183">Once data is exported into a flat file, it can be imported to another SQL server using bulk import.</span></span>

1. <span data-ttu-id="c1a69-184">A helyszíni SQL Server a bcp segédprogram használatával az alábbiak szerint fájlba az adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="c1a69-184">Export the data from on-premises SQL Server to a File using the bcp utility as follows</span></span>

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. <span data-ttu-id="c1a69-185">Az adatbázis és a tábla az SQL Server virtuális gép létrehozása az Azure használatával a `create database` és `create table` a következő tábla sémáját exportálja a rendszer az 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="c1a69-185">Create the database and the table on SQL Server VM on Azure using the `create database` and `create table` for the table schema exported in step 1.</span></span>
3. <span data-ttu-id="c1a69-186">Hozzon létre egy formátumfájlt keresztül mutatja az adatok exportálása/importálása folyamatban a következő tábla sémáját.</span><span class="sxs-lookup"><span data-stu-id="c1a69-186">Create a format file for describing the table schema of the data being exported/imported.</span></span> <span data-ttu-id="c1a69-187">A formátumfájl részleteit ismerteti a [hozzon létre egy Formátumfájlt (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1a69-187">Details of the format file are described in [Create a Format File (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span></span>

    <span data-ttu-id="c1a69-188">Formázza a fájl létrehozása a jelentés futtatásakor BCP az SQL Server-számítógépről</span><span class="sxs-lookup"><span data-stu-id="c1a69-188">Format file generation when running BCP from the SQL Server machine</span></span>

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    <span data-ttu-id="c1a69-189">Formázza a fájl létrehozása a jelentés futtatásakor BCP távoli SQL-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c1a69-189">Format file generation when running BCP remotely against a SQL Server</span></span>

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. <span data-ttu-id="c1a69-190">A szakaszban ismertetett módszerekkel [áthelyezése adatforrásból származó fájl](#filesource_to_sqlonazurevm) az adatok áthelyezése egy SQL Server a egybesimított fájlokba.</span><span class="sxs-lookup"><span data-stu-id="c1a69-190">Use any of the methods described in section [Moving Data from File Source](#filesource_to_sqlonazurevm) to move the data in flat files to a SQL Server.</span></span>

### <span data-ttu-id="c1a69-191"><a name="sql-migration"></a>SQL-adatbázis áttelepítése varázsló</span><span class="sxs-lookup"><span data-stu-id="c1a69-191"><a name="sql-migration"></a>SQL Database Migration Wizard</span></span>
<span data-ttu-id="c1a69-192">[SQL Server adatbázis áttelepítése varázsló](http://sqlazuremw.codeplex.com/) felhasználóbarát biztosítja az adatok áthelyezése két SQL server példányai között.</span><span class="sxs-lookup"><span data-stu-id="c1a69-192">[SQL Server Database Migration Wizard](http://sqlazuremw.codeplex.com/) provides a user-friendly way to move data between two SQL server instances.</span></span> <span data-ttu-id="c1a69-193">A felhasználó a képezi le a forrás és a céltábla közötti adatkulcsokat típusú oszlopokat és a különböző egyéb funkciók válasszon.</span><span class="sxs-lookup"><span data-stu-id="c1a69-193">It allows the user to map the data schema between sources and destination tables, choose column types and various other functionalities.</span></span> <span data-ttu-id="c1a69-194">A tömeges másolási (BCP) a színfalak használ.</span><span class="sxs-lookup"><span data-stu-id="c1a69-194">It uses bulk copy (BCP) under the covers.</span></span> <span data-ttu-id="c1a69-195">Az alábbiakban látható egy Képernyőkép az SQL-adatbázis áttelepítése varázsló az üdvözlőképernyő.</span><span class="sxs-lookup"><span data-stu-id="c1a69-195">A screenshot of the welcome screen for the SQL Database Migration wizard is shown below.</span></span>  

![SQL Server varázsló][2]

### <span data-ttu-id="c1a69-197"><a name="sql-backup"></a>Adatbázis biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="c1a69-197"><a name="sql-backup"></a>Database back up and restore</span></span>
<span data-ttu-id="c1a69-198">SQL Server támogatja:</span><span class="sxs-lookup"><span data-stu-id="c1a69-198">SQL Server supports:</span></span>

1. <span data-ttu-id="c1a69-199">[Adatbázis biztonsági mentése és visszaállítása funkció](https://msdn.microsoft.com/library/ms187048.aspx) (mind a helyi fájl vagy bacpac exportálása blob) és [adatok rétegből álló alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac használatával).</span><span class="sxs-lookup"><span data-stu-id="c1a69-199">[Database back up and restore functionality](https://msdn.microsoft.com/library/ms187048.aspx) (both to a local file or bacpac export to blob) and [Data Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx) (using bacpac).</span></span>
2. <span data-ttu-id="c1a69-200">Képes közvetlenül létrehozása a másolt adatbázis vagy egy meglévő SQL Azure Database másolása az Azure SQL Server virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c1a69-200">Ability to directly create SQL Server VMs on Azure with a copied database or copy to an existing SQL Azure database.</span></span> <span data-ttu-id="c1a69-201">További részletekért lásd: [adatbázis másolása varázsló használatával](https://msdn.microsoft.com/library/ms188664.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1a69-201">For more details, see [Use the Copy Database Wizard](https://msdn.microsoft.com/library/ms188664.aspx).</span></span>

<span data-ttu-id="c1a69-202">Fel/helyreállítani a képernyőképen az adatbázis biztonsági beállításokat az SQL Server Management Studio alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="c1a69-202">A screenshot of the Database back up/restore options from SQL Server Management Studio is shown below.</span></span>

![SQL Server Import eszközt][1]

## <a name="resources"></a><span data-ttu-id="c1a69-204">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="c1a69-204">Resources</span></span>
[<span data-ttu-id="c1a69-205">Egy Azure virtuális Gépen lévő SQL Server adatbázis áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c1a69-205">Migrate a Database to SQL Server on an Azure VM</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[<span data-ttu-id="c1a69-206">Az SQL Server használata Azure virtuális gépeken – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c1a69-206">SQL Server on Azure Virtual Machines overview</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
