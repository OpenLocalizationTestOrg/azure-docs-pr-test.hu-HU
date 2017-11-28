---
title: "aaaMove adatok tooSQL Server Azure virtuális géphez |} Microsoft Docs"
description: "Adatok áthelyezése egybesimított fájlokba, illetve egy helyszíni SQL Server tooSQL Server Azure virtuális gépen."
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
ms.openlocfilehash: 63e02158f9c99f4478c4eb1cde62c877983dcf27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-toosql-server-on-an-azure-virtual-machine"></a><span data-ttu-id="ec88e-103">Helyezze át az adatokat tooSQL Server Azure virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="ec88e-103">Move data tooSQL Server on an Azure virtual machine</span></span>
<span data-ttu-id="ec88e-104">Ez a témakör bemutatja az adatok áthelyezése vagy strukturálatlan fájlból (CSV vagy TSV formátumban), vagy egy helyi SQL Server tooSQL Server Azure virtuális géphez hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="ec88e-104">This topic outlines hello options for moving data either from flat files (CSV or TSV formats) or from an on-premises SQL Server tooSQL Server on an Azure virtual machine.</span></span> <span data-ttu-id="ec88e-105">Ezek a feladatok áthelyezése adatok toohello felhő hello Team adatok tudományos folyamat részét képezik.</span><span class="sxs-lookup"><span data-stu-id="ec88e-105">These tasks for moving data toohello cloud are part of hello Team Data Science Process.</span></span>

<span data-ttu-id="ec88e-106">Ez a témakör bemutatja a Machine Learning hello áthelyezése adatok tooan Azure SQL Database beállítások, lásd: [adatok tooan Azure SQL-adatbázis áthelyezése az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="ec88e-106">For a topic that outlines hello options for moving data tooan Azure SQL Database for Machine Learning, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

<span data-ttu-id="ec88e-107">Hello **menü** hivatkozások tootopics, amelyek ismertetik, hogyan tooingest adatok más cél környezetekben, ahol hello adatok is tárolhatók és feldolgozhatók, során hello Team adatok tudományos folyamat (TDSP) alatt.</span><span class="sxs-lookup"><span data-stu-id="ec88e-107">hello **menu** below links tootopics that describe how tooingest data into other target environments where hello data can be stored and processed during hello Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="ec88e-108">hello következő táblázat összefoglalja hello beállítások áthelyezése adatok tooSQL Server Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="ec88e-108">hello following table summarizes hello options for moving data tooSQL Server on an Azure virtual machine.</span></span>

| <span data-ttu-id="ec88e-109"><b>FORRÁS</b></span><span class="sxs-lookup"><span data-stu-id="ec88e-109"><b>SOURCE</b></span></span> | <span data-ttu-id="ec88e-110"><b>CÉL: SQL Server Azure virtuális gépen</b></span><span class="sxs-lookup"><span data-stu-id="ec88e-110"><b>DESTINATION: SQL Server on Azure VM</b></span></span> |
| --- | --- |
| <span data-ttu-id="ec88e-111"><b>Egybesimított fájl</b></span><span class="sxs-lookup"><span data-stu-id="ec88e-111"><b>Flat File</b></span></span> |<span data-ttu-id="ec88e-112">1. <a href="#insert-tables-bcp">Parancssori tömeges másolási segédprogram (BCP)</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-112">1. <a href="#insert-tables-bcp">Command-line bulk copy utility (BCP) </a></span></span><br> <span data-ttu-id="ec88e-113">2. <a href="#insert-tables-bulkquery">A tömeges beszúrás SQL-lekérdezés</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-113">2. <a href="#insert-tables-bulkquery">Bulk Insert SQL Query </a></span></span><br> <span data-ttu-id="ec88e-114">3. <a href="#sql-builtin-utilities">Az SQL Server grafikus beépített segédprogramok</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-114">3. <a href="#sql-builtin-utilities">Graphical Built-in Utilities in SQL Server</a></span></span> |
| <span data-ttu-id="ec88e-115"><b>A helyszíni SQL Server</b></span><span class="sxs-lookup"><span data-stu-id="ec88e-115"><b>On-Premises SQL Server</b></span></span> |<span data-ttu-id="ec88e-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-116">1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</a></span></span><br> <span data-ttu-id="ec88e-117">2. <a href="#export-flat-file">Exportálás tooa lapos fájl</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-117">2. <a href="#export-flat-file">Export tooa flat File </a></span></span><br> <span data-ttu-id="ec88e-118">3. <a href="#sql-migration">SQL-adatbázis áttelepítése varázsló</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-118">3. <a href="#sql-migration">SQL Database Migration Wizard </a></span></span> <br> <span data-ttu-id="ec88e-119">4. <a href="#sql-backup">Adatbázis biztonsági mentése és visszaállítása</a></span><span class="sxs-lookup"><span data-stu-id="ec88e-119">4. <a href="#sql-backup">Database back up and restore </a></span></span><br> |

<span data-ttu-id="ec88e-120">Figyelje meg, hogy a jelen dokumentum céljából feltételezzük, hogy az SQL-parancsok végrehajtott SQL Server Management Studio vagy Visual Studio adatbázis-kezelő.</span><span class="sxs-lookup"><span data-stu-id="ec88e-120">Note that this document assumes that SQL commands are executed from SQL Server Management Studio or Visual Studio Database Explorer.</span></span>

> [!TIP]
> <span data-ttu-id="ec88e-121">Alternatív megoldásként használható [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate és ütemezés szerinti egy folyamatot, amely áthelyezi adatok tooa SQL Server virtuális gép az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="ec88e-121">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) toocreate and schedule a pipeline that will move data tooa SQL Server VM on Azure.</span></span> <span data-ttu-id="ec88e-122">További információkért lásd: [Azure Data Factory (másolási tevékenység) adatok másolása](../data-factory/data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ec88e-122">For more information, see [Copy data with Azure Data Factory (Copy Activity)](../data-factory/data-factory-data-movement-activities.md).</span></span>
>
>

## <span data-ttu-id="ec88e-123"><a name="prereqs"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ec88e-123"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="ec88e-124">Ez az oktatóanyag feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="ec88e-124">This tutorial assumes you have:</span></span>

* <span data-ttu-id="ec88e-125">Egy **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="ec88e-125">An **Azure subscription**.</span></span> <span data-ttu-id="ec88e-126">Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ec88e-126">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ec88e-127">Egy **Azure storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="ec88e-127">An **Azure storage account**.</span></span> <span data-ttu-id="ec88e-128">Szüksége lesz egy Azure storage-fiók ebben az oktatóanyagban hello adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="ec88e-128">You will use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="ec88e-129">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk.</span><span class="sxs-lookup"><span data-stu-id="ec88e-129">If you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="ec88e-130">Hello storage-fiók létrehozása után kell tooobtain hello fiók kulcs tooaccess hello tárterületet használja.</span><span class="sxs-lookup"><span data-stu-id="ec88e-130">After you have created hello storage account, you will need tooobtain hello account key used tooaccess hello storage.</span></span> <span data-ttu-id="ec88e-131">Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="ec88e-131">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="ec88e-132">Kiépített **egy Azure virtuális Gépen található SQL-kiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="ec88e-132">Provisioned **SQL Server on an Azure VM**.</span></span> <span data-ttu-id="ec88e-133">Útmutatásért lásd: [állítsa be az Azure SQL Server virtuális gép speciális elemzésekre IPython Notebook kiszolgálóként](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="ec88e-133">For instructions, see [Set up an Azure SQL Server virtual machine as an IPython Notebook server for advanced analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md).</span></span>
* <span data-ttu-id="ec88e-134">Telepített és konfigurált **Azure PowerShell** helyileg.</span><span class="sxs-lookup"><span data-stu-id="ec88e-134">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="ec88e-135">Útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ec88e-135">For instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="ec88e-136"><a name="filesource_to_sqlonazurevm"></a>Adatok áthelyezése egy egybesimított fájl forrás tooSQL kiszolgálót egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="ec88e-136"><a name="filesource_to_sqlonazurevm"></a> Moving data from a flat file source tooSQL Server on an Azure VM</span></span>
<span data-ttu-id="ec88e-137">Ha az adatok egy egyszerű fájlban (rendezett sorhoz/oszlophoz formátumban), azok áthelyezett tooSQL Server Azure virtuális gép a következő módszerek hello keresztül:</span><span class="sxs-lookup"><span data-stu-id="ec88e-137">If your data is in a flat file (arranged in a row/column format), it can be moved tooSQL Server VM on Azure via hello following methods:</span></span>

1. [<span data-ttu-id="ec88e-138">Parancssori tömeges másolási segédprogram (BCP)</span><span class="sxs-lookup"><span data-stu-id="ec88e-138">Command-line bulk copy utility (BCP)</span></span>](#insert-tables-bcp)
2. [<span data-ttu-id="ec88e-139">A tömeges beszúrás SQL-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="ec88e-139">Bulk Insert SQL Query </span></span>](#insert-tables-bulkquery)
3. [<span data-ttu-id="ec88e-140">Az SQL Server (importálási/exportálási, SSIS) grafikus beépített segédprogramok</span><span class="sxs-lookup"><span data-stu-id="ec88e-140">Graphical Built-in Utilities in SQL Server (Import/Export, SSIS)</span></span>](#sql-builtin-utilities)

### <span data-ttu-id="ec88e-141"><a name="insert-tables-bcp"></a>Parancssori tömeges másolási segédprogram (BCP)</span><span class="sxs-lookup"><span data-stu-id="ec88e-141"><a name="insert-tables-bcp"></a>Command-line bulk copy utility (BCP)</span></span>
<span data-ttu-id="ec88e-142">BCP parancssori segédprogram SQL Server telepített, és egyik hello leggyorsabb módon toomove adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec88e-142">BCP is a command-line utility installed with SQL Server and is one of hello quickest ways toomove data.</span></span> <span data-ttu-id="ec88e-143">Minden három SQL Server változat (a helyszíni SQL Server, SQL Azure és SQL Server Azure virtuális gép) is használható.</span><span class="sxs-lookup"><span data-stu-id="ec88e-143">It works across all three SQL Server variants (On-premises SQL Server, SQL Azure and SQL Server VM on Azure).</span></span>

> [!NOTE]
> <span data-ttu-id="ec88e-144">**Az adatok hol kell lennie a BCP?**</span><span class="sxs-lookup"><span data-stu-id="ec88e-144">**Where should my data be for BCP?**</span></span>  
> <span data-ttu-id="ec88e-145">Bár ez nem szükséges, hogy hello hello célként SQL Server egyazon számítógépen lehetővé teszi, hogy gyorsabb átvitelt (hálózati sebesség és a helyi lemez IO sebesség) található forrás adatokat tartalmazó fájlok.</span><span class="sxs-lookup"><span data-stu-id="ec88e-145">While it is not required, having files containing source data located on hello same machine as hello target SQL Server allows for faster transfers (network speed vs local disk IO speed).</span></span> <span data-ttu-id="ec88e-146">Hello egybesimított fájlok tartalmazó adatok toohello gépet áthelyezheti adott SQL Server használatával van telepítve, mint eszközök különböző fájlok másolása [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Tártallózó](http://storageexplorer.com/) vagy a windows másolja és illessze be távoli keresztül Asztal protokoll (RDP).</span><span class="sxs-lookup"><span data-stu-id="ec88e-146">You can move hello flat files containing data toohello machine where SQL Server is installed using various file copying tools such as [AZCopy](../storage/common/storage-use-azcopy.md), [Azure Storage Explorer](http://storageexplorer.com/) or windows copy/paste via Remote Desktop Protocol (RDP).</span></span>
>
>

1. <span data-ttu-id="ec88e-147">Győződjön meg arról, hogy a hello tároló SQL Server-adatbázis hello adatbázis és hello táblák jöttek létre.</span><span class="sxs-lookup"><span data-stu-id="ec88e-147">Ensure that hello database and hello tables are created on hello target SQL Server database.</span></span> <span data-ttu-id="ec88e-148">Íme egy példa bemutatja, hogyan toodo adott használatával hello `Create Database` és `Create Table` parancsokat:</span><span class="sxs-lookup"><span data-stu-id="ec88e-148">Here is an example of how toodo that using hello `Create Database` and `Create Table` commands:</span></span>

        CREATE DATABASE <database_name>

        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        )
2. <span data-ttu-id="ec88e-149">Hello meghatározó formátumfájl séma hello hello tábla parancs követően – hello gép parancssori hello hello kiállításával bcp futtató készítése.</span><span class="sxs-lookup"><span data-stu-id="ec88e-149">Generate hello format file that describes hello schema for hello table by issuing hello following command from hello command-line of hello machine where bcp is installed.</span></span>

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n`
3. <span data-ttu-id="ec88e-150">Hello adatok beszúrása hello adatbázis hello bcp parancs használatával az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="ec88e-150">Insert hello data into hello database using hello bcp command as follows.</span></span> <span data-ttu-id="ec88e-151">Feltételezve, hogy hello SQL Server telepítve van ugyanezen a gépen ezt kell működnie a parancssori hello:</span><span class="sxs-lookup"><span data-stu-id="ec88e-151">This should work from hello command-line assuming that hello SQL Server is installed on same machine:</span></span>

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> <span data-ttu-id="ec88e-152">**Optimalizálja a BCP beszúrása** tekintse meg a következő cikket hello ["Tömeges importálással optimalizálása útmutatás"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) ilyen toooptimize szúrja be.</span><span class="sxs-lookup"><span data-stu-id="ec88e-152">**Optimizing BCP Inserts** Please refer hello following article ['Guidelines for Optimizing Bulk Import'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) toooptimize such inserts.</span></span>
>
>

### <span data-ttu-id="ec88e-153"><a name="insert-tables-bulkquery-parallel"></a>Gyorsabb adatátvitelt jelölik a Beszúrás parallelizing</span><span class="sxs-lookup"><span data-stu-id="ec88e-153"><a name="insert-tables-bulkquery-parallel"></a>Parallelizing Inserts for Faster Data Movement</span></span>
<span data-ttu-id="ec88e-154">Ha nagy hello adatokat helyez át, felgyorsíthatja dolgot egy PowerShell-parancsfájlt párhuzamosan több BCP parancsok egyidejűleg végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="ec88e-154">If hello data you are moving is large, you can speed things up by simultaneously executing multiple BCP commands in parallel in a PowerShell Script.</span></span>

> [!NOTE]
> <span data-ttu-id="ec88e-155">**Big Data típusú adatok adatfeldolgozást** nagy és nagyon nagy adatkészletek betöltése toooptimize adatok particionálása a logikai és fizikai adatbázis táblák fájlcsoportok és a partíció táblákat.</span><span class="sxs-lookup"><span data-stu-id="ec88e-155">**Big data Ingestion** toooptimize data loading for large and very large datasets, partition your logical and physical database tables using multiple filegroups and partition tables.</span></span> <span data-ttu-id="ec88e-156">Létrehozás és adattáblák toopartition betöltés kapcsolatos további információkért lásd: [párhuzamos terhelés SQL partíciós táblákba](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="ec88e-156">For more information about creating and loading data toopartition tables, see [Parallel Load SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
>
>

<span data-ttu-id="ec88e-157">hello PowerShell-parancsfájlpélda az alábbi bemutatása párhuzamos Beszúrás a bcp segítségével:</span><span class="sxs-lookup"><span data-stu-id="ec88e-157">hello sample PowerShell script below demonstrate parallel inserts using bcp:</span></span>

    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for hello script tooexecute
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


    # Wait for it all toocomplete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }

    # Getting hello information back from hello jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset hello execution policy


### <span data-ttu-id="ec88e-158"><a name="insert-tables-bulkquery"></a>A tömeges beszúrás SQL-lekérdezés</span><span class="sxs-lookup"><span data-stu-id="ec88e-158"><a name="insert-tables-bulkquery"></a>Bulk Insert SQL Query</span></span>
<span data-ttu-id="ec88e-159">[Tömeges beszúrás SQL-lekérdezés](https://msdn.microsoft.com/library/ms188365) lehet a sorhoz/oszlophoz alapú fájlokban hello adatbázisba használt tooimport adatok (hello támogatott típusok ismertetnek a[tömeges exportálása és importálása (SQL Server) előkészítése adatait](https://msdn.microsoft.com/library/ms188609)) témakör.</span><span class="sxs-lookup"><span data-stu-id="ec88e-159">[Bulk Insert SQL Query](https://msdn.microsoft.com/library/ms188365) can be used tooimport data into hello database from row/column based files (hello supported types are covered in the[Prepare Data for Bulk Export or Import (SQL Server)](https://msdn.microsoft.com/library/ms188609)) topic.</span></span>

<span data-ttu-id="ec88e-160">Az alábbiakban néhány Példaparancsok tömeges Beszúrás az alábbiak szerint vannak:</span><span class="sxs-lookup"><span data-stu-id="ec88e-160">Here are some sample commands for Bulk Insert are as below:</span></span>  

1. <span data-ttu-id="ec88e-161">Az adatok elemzéséhez, és állítsa be az egyéni beállításokat arról, hogy hello az SQL Server adatbázis azt feltételezi, hogy a dátumok például semmilyen különleges mezők formátuma azonos hello toomake importálása előtt.</span><span class="sxs-lookup"><span data-stu-id="ec88e-161">Analyze your data and set any custom options before importing toomake sure that hello SQL Server database assumes hello same format for any special fields such as dates.</span></span> <span data-ttu-id="ec88e-162">Íme egy példa hogyan tooset hello dátum formátuma év, hónap-nap (ha az adatok hello dátum év, hónap-nap formátumban):</span><span class="sxs-lookup"><span data-stu-id="ec88e-162">Here is an example of how tooset hello date format as year-month-day (if your data contains hello date in year-month-day format):</span></span>

        SET DATEFORMAT ymd;    
2. <span data-ttu-id="ec88e-163">Importálja az adatokat, és tömeges importálási utasítást:</span><span class="sxs-lookup"><span data-stu-id="ec88e-163">Import data using bulk import statements:</span></span>

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be hello row separator in your data
        )

### <span data-ttu-id="ec88e-164"><a name="sql-builtin-utilities"></a>Az SQL Server beépített segédprogramok</span><span class="sxs-lookup"><span data-stu-id="ec88e-164"><a name="sql-builtin-utilities"></a>Built-in Utilities in SQL Server</span></span>
<span data-ttu-id="ec88e-165">SQL Server integrációja Services (SSIS) tooimport adatok SQL Server egy egybesimított fájlból Azure virtuális gép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ec88e-165">You can use SQL Server Integrations Services (SSIS) tooimport data into SQL Server VM on Azure from a flat file.</span></span>
<span data-ttu-id="ec88e-166">SSIS két studio környezetekben érhető el.</span><span class="sxs-lookup"><span data-stu-id="ec88e-166">SSIS is available in two studio environments.</span></span> <span data-ttu-id="ec88e-167">További információkért lásd: [Integration Services (SSIS) és a Studio környezetek](https://technet.microsoft.com/library/ms140028.aspx):</span><span class="sxs-lookup"><span data-stu-id="ec88e-167">For details, see [Integration Services (SSIS) and Studio Environments](https://technet.microsoft.com/library/ms140028.aspx):</span></span>

* <span data-ttu-id="ec88e-168">További részletek az SQL Server Data Tools: [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span><span class="sxs-lookup"><span data-stu-id="ec88e-168">For details on SQL Server Data Tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)</span></span>  
* <span data-ttu-id="ec88e-169">További részletek az Import/Export varázsló hello: [SQL Server importálása és exportálása varázsló](https://msdn.microsoft.com/library/ms141209.aspx)</span><span class="sxs-lookup"><span data-stu-id="ec88e-169">For details on hello Import/Export Wizard, see [SQL Server Import and Export Wizard](https://msdn.microsoft.com/library/ms141209.aspx)</span></span>

## <span data-ttu-id="ec88e-170"><a name="sqlonprem_to_sqlonazurevm"></a>Adatok áthelyezése a helyszíni SQL Server tooSQL kiszolgálót egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="ec88e-170"><a name="sqlonprem_to_sqlonazurevm"></a>Moving Data from on-premises SQL Server tooSQL Server on an Azure VM</span></span>
<span data-ttu-id="ec88e-171">A következő áttelepítési stratégiák hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="ec88e-171">You can also use hello following migration strategies:</span></span>

1. [<span data-ttu-id="ec88e-172">Központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló</span><span class="sxs-lookup"><span data-stu-id="ec88e-172">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</span></span>](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [<span data-ttu-id="ec88e-173">TooFlat fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="ec88e-173">Export tooFlat File</span></span>](#export-flat-file)
3. [<span data-ttu-id="ec88e-174">SQL-adatbázis áttelepítése varázsló</span><span class="sxs-lookup"><span data-stu-id="ec88e-174">SQL Database Migration Wizard</span></span>](#sql-migration)
4. [<span data-ttu-id="ec88e-175">Adatbázis biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ec88e-175">Database back up and restore</span></span>](#sql-backup)

<span data-ttu-id="ec88e-176">Azt mutatják be ezen az alábbi:</span><span class="sxs-lookup"><span data-stu-id="ec88e-176">We describe each of these below:</span></span>

### <a name="deploy-a-sql-server-database-tooa-microsoft-azure-vm-wizard"></a><span data-ttu-id="ec88e-177">Központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló</span><span class="sxs-lookup"><span data-stu-id="ec88e-177">Deploy a SQL Server Database tooa Microsoft Azure VM wizard</span></span>
<span data-ttu-id="ec88e-178">Hello **központi telepítése egy SQL Server-adatbázis tooa Microsoft Azure virtuális gép varázsló** egy helyi SQL Server példány tooSQL kiszolgálót egy Azure virtuális gépen egy egyszerű és ajánlott módja toomove adatokat jelzi.</span><span class="sxs-lookup"><span data-stu-id="ec88e-178">hello **Deploy a SQL Server Database tooa Microsoft Azure VM wizard** is a simple and recommended way toomove data from an on-premises SQL Server instance tooSQL Server on an Azure VM.</span></span> <span data-ttu-id="ec88e-179">Részletes lépéseit, valamint az egyéb alternatívák döntéseken, lásd: [egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ec88e-179">For detailed steps as well as a discussion of other alternatives, see [Migrate a database tooSQL Server on an Azure VM](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md).</span></span>

### <span data-ttu-id="ec88e-180"><a name="export-flat-file"></a>TooFlat fájl exportálása</span><span class="sxs-lookup"><span data-stu-id="ec88e-180"><a name="export-flat-file"></a>Export tooFlat File</span></span>
<span data-ttu-id="ec88e-181">Különböző módszereket lehet használt toobulk adatok exportálása egy helyszíni SQL Server kiszolgáló hello leírtak [tömeges adatok importálása és exportálása a (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="ec88e-181">Various methods can be used toobulk export data from an On-Premises SQL Server as documented in hello [Bulk Import and Export of Data (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) topic.</span></span> <span data-ttu-id="ec88e-182">Ez a dokumentum példaként hello tömeges másolási Program (BCP) szakaszában.</span><span class="sxs-lookup"><span data-stu-id="ec88e-182">This document will cover hello Bulk Copy Program (BCP) as an example.</span></span> <span data-ttu-id="ec88e-183">Adatok exportálása egy egyszerű fájlba, után azok importált tooanother SQL server tömeges importálással.</span><span class="sxs-lookup"><span data-stu-id="ec88e-183">Once data is exported into a flat file, it can be imported tooanother SQL server using bulk import.</span></span>

1. <span data-ttu-id="ec88e-184">A helyszíni SQL Server tooa fájl hello adatok exportálása az alábbiak szerint hello bcp segédprogram használatával</span><span class="sxs-lookup"><span data-stu-id="ec88e-184">Export hello data from on-premises SQL Server tooa File using hello bcp utility as follows</span></span>

    `bcp dbname..tablename out datafile.tsv -S    servername\sqlinstancename -T -t \t -t \n -c`
2. <span data-ttu-id="ec88e-185">Hello adatbázis és hello tábla létrehozása az SQL Server virtuális gép Azure-ban hello `create database` és `create table` az 1. lépésben exportált hello tábla sémáját.</span><span class="sxs-lookup"><span data-stu-id="ec88e-185">Create hello database and hello table on SQL Server VM on Azure using hello `create database` and `create table` for hello table schema exported in step 1.</span></span>
3. <span data-ttu-id="ec88e-186">Hozzon létre egy formátumfájlt leíró hello táblaséma hello adatok exportálása/importálása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="ec88e-186">Create a format file for describing hello table schema of hello data being exported/imported.</span></span> <span data-ttu-id="ec88e-187">Hello formátumfájl részleteit ismerteti a [hozzon létre egy Formátumfájlt (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec88e-187">Details of hello format file are described in [Create a Format File (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).</span></span>

    <span data-ttu-id="ec88e-188">Ha SQL Server-számítógépen fut a BCP hello formátumú fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec88e-188">Format file generation when running BCP from hello SQL Server machine</span></span>

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n

    <span data-ttu-id="ec88e-189">Formázza a fájl létrehozása a jelentés futtatásakor BCP távoli SQL-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="ec88e-189">Format file generation when running BCP remotely against a SQL Server</span></span>

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
4. <span data-ttu-id="ec88e-190">A szakaszban leírt hello módszerekkel [áthelyezése adatforrásból származó fájl](#filesource_to_sqlonazurevm) toomove hello adatok egybesimított fájlokba tooa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ec88e-190">Use any of hello methods described in section [Moving Data from File Source](#filesource_to_sqlonazurevm) toomove hello data in flat files tooa SQL Server.</span></span>

### <span data-ttu-id="ec88e-191"><a name="sql-migration"></a>SQL-adatbázis áttelepítése varázsló</span><span class="sxs-lookup"><span data-stu-id="ec88e-191"><a name="sql-migration"></a>SQL Database Migration Wizard</span></span>
<span data-ttu-id="ec88e-192">[SQL Server adatbázis áttelepítése varázsló](http://sqlazuremw.codeplex.com/) adja meg egy felhasználóbarát módon toomove között két SQL server-példányok adatait.</span><span class="sxs-lookup"><span data-stu-id="ec88e-192">[SQL Server Database Migration Wizard](http://sqlazuremw.codeplex.com/) provides a user-friendly way toomove data between two SQL server instances.</span></span> <span data-ttu-id="ec88e-193">Lehetővé teszi a hello felhasználói toomap hello Adatséma források és a cél táblák között, típusú oszlopokat és a különböző egyéb funkciók kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="ec88e-193">It allows hello user toomap hello data schema between sources and destination tables, choose column types and various other functionalities.</span></span> <span data-ttu-id="ec88e-194">Hello színfalak tömeges másolási (BCP) használja.</span><span class="sxs-lookup"><span data-stu-id="ec88e-194">It uses bulk copy (BCP) under hello covers.</span></span> <span data-ttu-id="ec88e-195">A képernyőfelvétel a hello hello SQL-adatbázis áttelepítése varázsló alább az üdvözlőképernyő.</span><span class="sxs-lookup"><span data-stu-id="ec88e-195">A screenshot of hello welcome screen for hello SQL Database Migration wizard is shown below.</span></span>  

![SQL Server varázsló][2]

### <span data-ttu-id="ec88e-197"><a name="sql-backup"></a>Adatbázis biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ec88e-197"><a name="sql-backup"></a>Database back up and restore</span></span>
<span data-ttu-id="ec88e-198">SQL Server támogatja:</span><span class="sxs-lookup"><span data-stu-id="ec88e-198">SQL Server supports:</span></span>

1. <span data-ttu-id="ec88e-199">[Adatbázis biztonsági mentése és visszaállítása funkció](https://msdn.microsoft.com/library/ms187048.aspx) (mindkét tooa helyi fájl vagy bacpac exportálás tooblob) és [adatok rétegből álló alkalmazások](https://msdn.microsoft.com/library/ee210546.aspx) (bacpac használatával).</span><span class="sxs-lookup"><span data-stu-id="ec88e-199">[Database back up and restore functionality](https://msdn.microsoft.com/library/ms187048.aspx) (both tooa local file or bacpac export tooblob) and [Data Tier Applications](https://msdn.microsoft.com/library/ee210546.aspx) (using bacpac).</span></span>
2. <span data-ttu-id="ec88e-200">Képes toodirectly SQL Server virtuális gépek létrehozása az Azure másolt adatbázis vagy példány tooan meglévő SQL Azure-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="ec88e-200">Ability toodirectly create SQL Server VMs on Azure with a copied database or copy tooan existing SQL Azure database.</span></span> <span data-ttu-id="ec88e-201">További részletekért lásd: [használata hello másolása adatbázis varázsló](https://msdn.microsoft.com/library/ms188664.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec88e-201">For more details, see [Use hello Copy Database Wizard](https://msdn.microsoft.com/library/ms188664.aspx).</span></span>

<span data-ttu-id="ec88e-202">Fel/helyreállítani a egy képernyőfelvétel a hello adatbázis biztonsági beállításokat az SQL Server Management Studio alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="ec88e-202">A screenshot of hello Database back up/restore options from SQL Server Management Studio is shown below.</span></span>

![SQL Server Import eszközt][1]

## <a name="resources"></a><span data-ttu-id="ec88e-204">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="ec88e-204">Resources</span></span>
[<span data-ttu-id="ec88e-205">Egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése</span><span class="sxs-lookup"><span data-stu-id="ec88e-205">Migrate a Database tooSQL Server on an Azure VM</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-migrate-sql.md)

[<span data-ttu-id="ec88e-206">Az SQL Server használata Azure virtuális gépeken – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ec88e-206">SQL Server on Azure Virtual Machines overview</span></span>](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
