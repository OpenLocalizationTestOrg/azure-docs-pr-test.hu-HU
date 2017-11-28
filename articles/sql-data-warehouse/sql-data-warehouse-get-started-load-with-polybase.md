---
title: "az SQL Data Warehouse oktatóanyag aaaPolyBase |} Microsoft Docs"
description: "Ismerje meg a PolyBase-t, és hogyan toouse az adatraktározási forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 0a0103b4-ddd6-4d1e-87be-4965d6e99f3f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/01/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 3e680ec407c1d920dd59ea922b82c9208b5e9a84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a><span data-ttu-id="e18dc-103">Adatok betöltése a PolyBase-zel az SQL Data Warehouse-ba</span><span class="sxs-lookup"><span data-stu-id="e18dc-103">Load data with PolyBase in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e18dc-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="e18dc-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="e18dc-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="e18dc-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="e18dc-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="e18dc-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="e18dc-107">BCP</span><span class="sxs-lookup"><span data-stu-id="e18dc-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="e18dc-108">Ez az oktatóanyag bemutatja, hogyan tooload adatokat az SQL Data Warehouse-bA az AzCopy és a PolyBase használatával.</span><span class="sxs-lookup"><span data-stu-id="e18dc-108">This tutorial shows how tooload data into SQL Data Warehouse using AzCopy and PolyBase.</span></span> <span data-ttu-id="e18dc-109">Az oktatóanyag végére elsajátíthatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="e18dc-109">When finished, you will know how to:</span></span>

* <span data-ttu-id="e18dc-110">AzCopy toocopy adatok tooAzure blob storage használata</span><span class="sxs-lookup"><span data-stu-id="e18dc-110">Use AzCopy toocopy data tooAzure blob storage</span></span>
* <span data-ttu-id="e18dc-111">Adatbázis-objektumok toodefine hello adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="e18dc-111">Create database objects toodefine hello data</span></span>
* <span data-ttu-id="e18dc-112">Futtassa a T-SQL lekérdezés tooload hello adatok</span><span class="sxs-lookup"><span data-stu-id="e18dc-112">Run a T-SQL query tooload hello data</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="e18dc-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e18dc-113">Prerequisites</span></span>
<span data-ttu-id="e18dc-114">az oktatóanyag teljesítéséhez toostep, szüksége</span><span class="sxs-lookup"><span data-stu-id="e18dc-114">toostep through this tutorial, you need</span></span>

* <span data-ttu-id="e18dc-115">Egy SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e18dc-115">A SQL Data Warehouse database.</span></span>
* <span data-ttu-id="e18dc-116">Egy standard helyileg redundáns tárolás (Standard-LRS), standard georedundáns tárolás (Standard-GRS) vagy standard írásvédett georedundáns tárolás (Standard-RAGRS) típusú Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="e18dc-116">An Azure storage account of type Standard Locally Redundant Storage (Standard-LRS), Standard Geo-Redundant Storage (Standard-GRS), or Standard Read-Access Geo-Redundant Storage (Standard-RAGRS).</span></span>
* <span data-ttu-id="e18dc-117">AzCopy parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="e18dc-117">AzCopy Command-Line Utility.</span></span> <span data-ttu-id="e18dc-118">Töltse le és telepítse a hello [az AzCopy legújabb verzióját] [ latest version of AzCopy] amely hello Microsoft Azure Storage-eszközökkel együtt települ.</span><span class="sxs-lookup"><span data-stu-id="e18dc-118">Download and install hello [latest version of AzCopy][latest version of AzCopy] which is installed with hello Microsoft Azure Storage Tools.</span></span>
  
    ![Azure Storage-eszközök](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-tooazure-blob-storage"></a><span data-ttu-id="e18dc-120">1. lépés: A minta adatok tooAzure blob-tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e18dc-120">Step 1: Add sample data tooAzure blob storage</span></span>
<span data-ttu-id="e18dc-121">Rendelés tooload adatok igazolnia kell tooput néhány adatot az Azure blob storage-be.</span><span class="sxs-lookup"><span data-stu-id="e18dc-121">In order tooload data, we need tooput some sample data into an Azure blob storage.</span></span> <span data-ttu-id="e18dc-122">Ebben a lépésben feltöltünk egy Azure Storage-blobot mintaadatokkal.</span><span class="sxs-lookup"><span data-stu-id="e18dc-122">In this step we populate an Azure Storage blob with sample data.</span></span> <span data-ttu-id="e18dc-123">Később használjuk a PolyBase tooload ezeket a mintaadatokat az SQL Data Warehouse-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="e18dc-123">Later, we will use PolyBase tooload this sample data into your SQL Data Warehouse database.</span></span>

### <a name="a-prepare-a-sample-text-file"></a><span data-ttu-id="e18dc-124">A.</span><span class="sxs-lookup"><span data-stu-id="e18dc-124">A.</span></span> <span data-ttu-id="e18dc-125">Minta szöveges fájl előkészítése</span><span class="sxs-lookup"><span data-stu-id="e18dc-125">Prepare a sample text file</span></span>
<span data-ttu-id="e18dc-126">minta szöveges fájl tooprepare:</span><span class="sxs-lookup"><span data-stu-id="e18dc-126">tooprepare a sample text file:</span></span>

1. <span data-ttu-id="e18dc-127">Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új fájlba.</span><span class="sxs-lookup"><span data-stu-id="e18dc-127">Open Notepad and copy hello following lines of data into a new file.</span></span> <span data-ttu-id="e18dc-128">% Temp%\DimDate2.txt a tooyour helyi ideiglenes könyvtárba menteni.</span><span class="sxs-lookup"><span data-stu-id="e18dc-128">Save this tooyour local temp directory as %temp%\DimDate2.txt.</span></span>

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

### <a name="b-find-your-blob-service-endpoint"></a><span data-ttu-id="e18dc-129">B.</span><span class="sxs-lookup"><span data-stu-id="e18dc-129">B.</span></span> <span data-ttu-id="e18dc-130">A Blob-szolgáltatásvégpont megkeresése</span><span class="sxs-lookup"><span data-stu-id="e18dc-130">Find your blob service endpoint</span></span>
<span data-ttu-id="e18dc-131">toofind a blob-szolgáltatásvégpont:</span><span class="sxs-lookup"><span data-stu-id="e18dc-131">toofind your blob service endpoint:</span></span>

1. <span data-ttu-id="e18dc-132">Hello Azure portálon válassza **Tallózás** > **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="e18dc-132">From hello Azure Portal select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="e18dc-133">Kattintson a kívánt toouse hello tárfiókra.</span><span class="sxs-lookup"><span data-stu-id="e18dc-133">Click hello storage account you want toouse.</span></span>
3. <span data-ttu-id="e18dc-134">Hello Storage-fiók panelen kattintson a Blobok elemre</span><span class="sxs-lookup"><span data-stu-id="e18dc-134">In hello Storage account blade, click Blobs</span></span>
   
    ![Kattintson a Blobok elemre](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. <span data-ttu-id="e18dc-136">Mentse a Blob-szolgáltatásvégpontot későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="e18dc-136">Save your blob service endpoint URL for later.</span></span>
   
    ![Blob-szolgáltatásvégpont](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a><span data-ttu-id="e18dc-138">C.</span><span class="sxs-lookup"><span data-stu-id="e18dc-138">C.</span></span> <span data-ttu-id="e18dc-139">Az Azure Storage-kulcs megkeresése</span><span class="sxs-lookup"><span data-stu-id="e18dc-139">Find your Azure storage key</span></span>
<span data-ttu-id="e18dc-140">toofind az Azure storage-kulcs:</span><span class="sxs-lookup"><span data-stu-id="e18dc-140">toofind your Azure storage key:</span></span>

1. <span data-ttu-id="e18dc-141">Hello Azure portált, válassza ki **Tallózás** > **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="e18dc-141">From hello Azure Portal, select **Browse** > **Storage Accounts**.</span></span>
2. <span data-ttu-id="e18dc-142">Kattintson a kívánt toouse hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="e18dc-142">Click on hello storage account you want toouse.</span></span>
3. <span data-ttu-id="e18dc-143">Válassza az **Összes beállítás** > **Hívóbetűk** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e18dc-143">Select **All settings** > **Access keys**.</span></span>
4. <span data-ttu-id="e18dc-144">Kattintson a hello másolási mezőben toocopy a hozzáférési kulcsok toohello vágólapra egyikét.</span><span class="sxs-lookup"><span data-stu-id="e18dc-144">Click hello copy box toocopy one of your access keys toohello clipboard.</span></span>
   
    ![Az Azure Storage-kulcs másolása](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-hello-sample-file-tooazure-blob-storage"></a><span data-ttu-id="e18dc-146">D.</span><span class="sxs-lookup"><span data-stu-id="e18dc-146">D.</span></span> <span data-ttu-id="e18dc-147">Másolja a hello minta fájl tooAzure blob-tároló</span><span class="sxs-lookup"><span data-stu-id="e18dc-147">Copy hello sample file tooAzure blob storage</span></span>
<span data-ttu-id="e18dc-148">toocopy az adatok tooAzure blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="e18dc-148">toocopy your data tooAzure blob storage:</span></span>

1. <span data-ttu-id="e18dc-149">Nyisson meg egy parancssort, és módosítsa a könyvtárakat toohello AzCopy telepítési könyvtárára.</span><span class="sxs-lookup"><span data-stu-id="e18dc-149">Open a command prompt, and change directories toohello AzCopy installation directory.</span></span> <span data-ttu-id="e18dc-150">Ez a parancs módosítja egy 64 bites Windows-ügyfelén toohello alapértelmezett telepítési könyvtárra.</span><span class="sxs-lookup"><span data-stu-id="e18dc-150">This command changes toohello default installation directory on a 64-bit Windows client.</span></span>
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. <span data-ttu-id="e18dc-151">Futtassa a következő parancs tooupload hello fájl hello.</span><span class="sxs-lookup"><span data-stu-id="e18dc-151">Run hello following command tooupload hello file.</span></span> <span data-ttu-id="e18dc-152">Adja meg a(z) <blob service endpoint URL> Blob-szolgáltatásvégpont URL-jét és az <azure_storage_account_key> Azure Storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="e18dc-152">Specify your blob service endpoint URL for <blob service endpoint URL> and your Azure storage account key for <azure_storage_account_key>.</span></span>
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

<span data-ttu-id="e18dc-153">Lásd még: [Ismerkedés az AzCopy parancssori segédprogram hello][Getting Started with hello AzCopy Command-Line Utility].</span><span class="sxs-lookup"><span data-stu-id="e18dc-153">See also [Getting Started with hello AzCopy Command-Line Utility][Getting Started with hello AzCopy Command-Line Utility].</span></span>

### <a name="e-explore-your-blob-storage-container"></a><span data-ttu-id="e18dc-154">E.</span><span class="sxs-lookup"><span data-stu-id="e18dc-154">E.</span></span> <span data-ttu-id="e18dc-155">A Blob Storage-tároló áttekintése</span><span class="sxs-lookup"><span data-stu-id="e18dc-155">Explore your blob storage container</span></span>
<span data-ttu-id="e18dc-156">toosee hello tooblob tárolási feltöltött fájlban:</span><span class="sxs-lookup"><span data-stu-id="e18dc-156">toosee hello file you uploaded tooblob storage:</span></span>

1. <span data-ttu-id="e18dc-157">Lépjen vissza a tooyour Blob szolgáltatás panelre.</span><span class="sxs-lookup"><span data-stu-id="e18dc-157">Go back tooyour Blob service blade.</span></span>
2. <span data-ttu-id="e18dc-158">A Tárolók területen kattintson duplán a **datacontainer** elemre.</span><span class="sxs-lookup"><span data-stu-id="e18dc-158">Under Containers, double-click **datacontainer**.</span></span>
3. <span data-ttu-id="e18dc-159">tooexplore hello elérési tooyour adatokat, kattintson a hello mappa **datedimension** és látni fogja a feltöltött fájl **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="e18dc-159">tooexplore hello path tooyour data, click hello folder **datedimension** and you will see your uploaded file **DimDate2.txt**.</span></span>
4. <span data-ttu-id="e18dc-160">tooview tulajdonságait, kattintson a **DimDate2.txt**.</span><span class="sxs-lookup"><span data-stu-id="e18dc-160">tooview properties, click **DimDate2.txt**.</span></span>
5. <span data-ttu-id="e18dc-161">Vegye figyelembe, hogy a hello Blob tulajdonságai panelen letöltheti és hello fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="e18dc-161">Note that in hello Blob properties blade, you can download or delete hello file.</span></span>
   
    ![Az Azure Storage-blob megtekintése](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-hello-sample-data"></a><span data-ttu-id="e18dc-163">2. lépés: Mintaadatok hello a külső tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="e18dc-163">Step 2: Create an external table for hello sample data</span></span>
<span data-ttu-id="e18dc-164">Ebben a szakaszban létrehozhatunk egy külső táblát, amely meghatározza a hello mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="e18dc-164">In this section we create an external table that defines hello sample data.</span></span>

<span data-ttu-id="e18dc-165">A polybase külső táblák tooaccess adatok Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e18dc-165">PolyBase uses external tables tooaccess data in Azure blob storage.</span></span> <span data-ttu-id="e18dc-166">Hello adatok nem tárolja az SQL Data Warehouse, mivel a PolyBase kezeli a hitelesítési toohello külső adatokat egy adatbázishoz kötődő hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="e18dc-166">Since hello data is not stored within SQL Data Warehouse, PolyBase handles authentication toohello external data by using a database-scoped credential.</span></span>

<span data-ttu-id="e18dc-167">hello példa ebben a lépésben a Transact-SQL utasítás toocreate a külső tábla használja.</span><span class="sxs-lookup"><span data-stu-id="e18dc-167">hello example in this step uses these Transact-SQL statements toocreate an external table.</span></span>

* <span data-ttu-id="e18dc-168">[Hozzon létre Master Key (Transact-SQL)] [ Create Master Key (Transact-SQL)] tooencrypt hello titka az adatbázishoz kötődő hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="e18dc-168">[Create Master Key (Transact-SQL)][Create Master Key (Transact-SQL)] tooencrypt hello secret of your database scoped credential.</span></span>
* <span data-ttu-id="e18dc-169">[Hozzon létre Database Scoped Credential (Transact-SQL)] [ Create Database Scoped Credential (Transact-SQL)] toospecify hitelesítési adatainak megadása az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="e18dc-169">[Create Database Scoped Credential (Transact-SQL)][Create Database Scoped Credential (Transact-SQL)] toospecify authentication information for your Azure storage account.</span></span>
* <span data-ttu-id="e18dc-170">[Külső adatforrás (Transact-SQL) létrehozása] [ Create External Data Source (Transact-SQL)] az Azure blob storage toospecify hello helyét.</span><span class="sxs-lookup"><span data-stu-id="e18dc-170">[Create External Data Source (Transact-SQL)][Create External Data Source (Transact-SQL)] toospecify hello location of your Azure blob storage.</span></span>
* <span data-ttu-id="e18dc-171">[Hozzon létre a külső fájlformátumot (Transact-SQL)] [ Create External File Format (Transact-SQL)] toospecify hello adatformátum.</span><span class="sxs-lookup"><span data-stu-id="e18dc-171">[Create External File Format (Transact-SQL)][Create External File Format (Transact-SQL)] toospecify hello format of your data.</span></span>
* <span data-ttu-id="e18dc-172">[Hozzon létre a külső tábla (Transact-SQL)] [ Create External Table (Transact-SQL)] toospecify hello tábladefiníció és helyének hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="e18dc-172">[Create External Table (Transact-SQL)][Create External Table (Transact-SQL)] toospecify hello table definition and location of hello data.</span></span>

<span data-ttu-id="e18dc-173">Futtassa ezt a lekérdezést az SQL Data Warehouse-adatbázison.</span><span class="sxs-lookup"><span data-stu-id="e18dc-173">Run this query against your SQL Data Warehouse database.</span></span> <span data-ttu-id="e18dc-174">Egy külső táblát dimdate2external néven hello dbo sémában, amely toohello DimDate2.txt mintaadataira mutat hello Azure blob Storage tárolóban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e18dc-174">It will create an external table named DimDate2External in hello dbo schema that points toohello DimDate2.txt sample data in hello Azure blob storage.</span></span>

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication tooAzure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create hello external table
-- Specify column names and data types. This needs toomatch hello data in hello sample file.
-- LOCATION: Specify path toofile or directory that contains hello data (relative toohello blob container).
-- toopoint tooall files under hello blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on hello external table

SELECT count(*) FROM dbo.DimDate2External;

```


<span data-ttu-id="e18dc-175">Az SQL Server Object Explorerben a Visual Studio láthatja, hogy hello külső fájlformátumot, a külső adatforrást és a hello DimDate2External táblát.</span><span class="sxs-lookup"><span data-stu-id="e18dc-175">In SQL Server Object Explorer in Visual Studio, you can see hello external file format, external data source, and hello DimDate2External table.</span></span>

![Külső tábla megtekintése](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a><span data-ttu-id="e18dc-177">3. lépés: Adatok betöltése az SQL Data Warehouse-ba</span><span class="sxs-lookup"><span data-stu-id="e18dc-177">Step 3: Load data into SQL Data Warehouse</span></span>
<span data-ttu-id="e18dc-178">Hello külső tábla létrehozása után hello adatok betöltése az új tábla, vagy szúrja be egy meglévő táblába.</span><span class="sxs-lookup"><span data-stu-id="e18dc-178">Once hello external table is created, you can either load hello data into a new table or insert it into an existing table.</span></span>

* <span data-ttu-id="e18dc-179">tooload hello adatok új táblába, futtassa a hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást.</span><span class="sxs-lookup"><span data-stu-id="e18dc-179">tooload hello data into a new table, run hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="e18dc-180">hello új tábla tartalmazza hello hello lekérdezésben szereplő oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="e18dc-180">hello new table will have hello columns named in hello query.</span></span> <span data-ttu-id="e18dc-181">hello oszlopok adattípusai hello fog egyezni hello adattípusok a hello külső tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="e18dc-181">hello data types of hello columns will match hello data types in hello external table definition.</span></span>
* <span data-ttu-id="e18dc-182">tooload hello adatok meglévő táblába, használja a hello [INSERT... SELECT (Transact-SQL)] [ INSERT...SELECT (Transact-SQL)] utasítást.</span><span class="sxs-lookup"><span data-stu-id="e18dc-182">tooload hello data into an existing table, use hello [INSERT...SELECT (Transact-SQL)][INSERT...SELECT (Transact-SQL)] statement.</span></span>

```sql
-- Load hello data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="e18dc-183">4. lépés: Statisztikák létrehozása az újonnan betöltött adatokról</span><span class="sxs-lookup"><span data-stu-id="e18dc-183">Step 4: Create statistics on your newly loaded data</span></span>
<span data-ttu-id="e18dc-184">Az SQL Data Warehouse nem tudja automatikus létrehozni és frissíteni a statisztikákat.</span><span class="sxs-lookup"><span data-stu-id="e18dc-184">SQL Data Warehouse does not auto-create or auto-update statistics.</span></span> <span data-ttu-id="e18dc-185">Ezért tooachieve a lekérdezési teljesítmény, fontos toocreate statisztika hello után minden tábla minden oszlopához először betölteni.</span><span class="sxs-lookup"><span data-stu-id="e18dc-185">Therefore, tooachieve high query performance, it's important toocreate statistics on each column of each table after hello first load.</span></span> <span data-ttu-id="e18dc-186">Célszerű is fontos tooupdate statisztika hello adatok lényeges módosításai után.</span><span class="sxs-lookup"><span data-stu-id="e18dc-186">It's also important tooupdate statistics after substantial changes in hello data.</span></span>

<span data-ttu-id="e18dc-187">Ez a példa egyoszlopos statisztikát hoz létre hello új DimDate2 táblához.</span><span class="sxs-lookup"><span data-stu-id="e18dc-187">This example creates single-column statistics on hello new DimDate2 table.</span></span>

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

<span data-ttu-id="e18dc-188">több, lásd: toolearn [statisztika][Statistics].</span><span class="sxs-lookup"><span data-stu-id="e18dc-188">toolearn more, see [Statistics][Statistics].</span></span>  

## <a name="next-steps"></a><span data-ttu-id="e18dc-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e18dc-189">Next steps</span></span>
<span data-ttu-id="e18dc-190">Lásd: hello [PolyBase-útmutatóban] [ PolyBase guide] -t a PolyBase használó megoldások fejlesztéséről további információkat.</span><span class="sxs-lookup"><span data-stu-id="e18dc-190">See hello [PolyBase guide][PolyBase guide] for further information you should know as you develop a solution that uses PolyBase.</span></span>

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[PolyBase guide]: ./sql-data-warehouse-load-polybase-guide.md
[Getting Started with hello AzCopy Command-Line Utility]:../storage/common/storage-use-azcopy.md
[latest version of AzCopy]:../storage/common/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
