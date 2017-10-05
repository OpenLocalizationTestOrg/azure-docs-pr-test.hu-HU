---
title: "Az Azure-ból betöltése az Azure data warehouse-blob |} Microsoft Docs"
description: "Útmutató az Azure blob storage adatok betöltése az SQL Data Warehouse PolyBase segítségével. A nyilvános adatok a Contoso kereskedelmi adatraktár sémába néhány táblák betöltése."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: faca0fe7-62e7-4e1f-a86f-032b4ffcb06e
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 2859c1144f72fd685af89f83024df1409902ab0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a><span data-ttu-id="b31b0-104">Adatok betöltése az Azure blob storage az SQL Data warehouse-ba (PolyBase)</span><span class="sxs-lookup"><span data-stu-id="b31b0-104">Load data from Azure blob storage into SQL Data Warehouse (PolyBase)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b31b0-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="b31b0-105">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="b31b0-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="b31b0-106">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

<span data-ttu-id="b31b0-107">Adatok betöltése az Azure blob storage az Azure SQL Data Warehouse PolyBase és T-SQL-parancsok segítségével</span><span class="sxs-lookup"><span data-stu-id="b31b0-107">Use PolyBase and T-SQL commands to load data from Azure blob storage into Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="b31b0-108">Legyen egyszerű, hogy ez az oktatóanyag tölti be a két tábla az egy nyilvános Azure Storage-Blobból a Contoso kereskedelmi adatraktár sémába.</span><span class="sxs-lookup"><span data-stu-id="b31b0-108">To keep it simple, this tutorial loads two tables from a public Azure Storage Blob into the Contoso Retail Data Warehouse schema.</span></span> <span data-ttu-id="b31b0-109">A teljes adatkészlet betöltéséhez futtassa a példa [betölteni a teljes Contoso kereskedelmi adatraktár] [ Load the full Contoso Retail Data Warehouse] a Microsoft SQL Server minták tárházból.</span><span class="sxs-lookup"><span data-stu-id="b31b0-109">To load the full data set, run the example [Load the full Contoso Retail Data Warehouse][Load the full Contoso Retail Data Warehouse] from the Microsoft SQL Server Samples repository.</span></span>

<span data-ttu-id="b31b0-110">Az oktatóanyag tartalma:</span><span class="sxs-lookup"><span data-stu-id="b31b0-110">In this tutorial you will:</span></span>

1. <span data-ttu-id="b31b0-111">Az Azure blob storage betöltése a PolyBase konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b31b0-111">Configure PolyBase to load from Azure blob storage</span></span>
2. <span data-ttu-id="b31b0-112">Nyilvános adatok betöltése az adatbázisba</span><span class="sxs-lookup"><span data-stu-id="b31b0-112">Load public data into your database</span></span>
3. <span data-ttu-id="b31b0-113">Hajtsa végre a optimalizálást, a betöltés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="b31b0-113">Perform optimizations after the load is finished.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b31b0-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b31b0-114">Before you begin</span></span>
<span data-ttu-id="b31b0-115">Ez az oktatóanyag elindításához Azure-fiókot, amely már rendelkezik egy SQL Data Warehouse-adatbázis kell.</span><span class="sxs-lookup"><span data-stu-id="b31b0-115">To run this tutorial, you need an Azure account that already has a SQL Data Warehouse database.</span></span> <span data-ttu-id="b31b0-116">Ha még nem rendelkezik ezzel, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="b31b0-116">If you don't already have this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>

## <a name="1-configure-the-data-source"></a><span data-ttu-id="b31b0-117">1. Konfigurálja az adatforrást</span><span class="sxs-lookup"><span data-stu-id="b31b0-117">1. Configure the data source</span></span>
<span data-ttu-id="b31b0-118">A polybase külső T-SQL-objektumok a hely és a külső adatokra vonatkozó attribútumok megadhatók.</span><span class="sxs-lookup"><span data-stu-id="b31b0-118">PolyBase uses T-SQL external objects to define the location and attributes of the external data.</span></span> <span data-ttu-id="b31b0-119">A külső objektum az SQL Data Warehouse tárolja.</span><span class="sxs-lookup"><span data-stu-id="b31b0-119">The external object definitions are stored in SQL Data Warehouse.</span></span> <span data-ttu-id="b31b0-120">Maga az adat a külsőleg tárolódik.</span><span class="sxs-lookup"><span data-stu-id="b31b0-120">The data itself is stored externally.</span></span>

### <a name="11-create-a-credential"></a><span data-ttu-id="b31b0-121">1.1.</span><span class="sxs-lookup"><span data-stu-id="b31b0-121">1.1.</span></span> <span data-ttu-id="b31b0-122">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b31b0-122">Create a credential</span></span>
<span data-ttu-id="b31b0-123">**Kihagyhatja ezt a lépést** Ha a Contoso nyilvános adatokat tölt be.</span><span class="sxs-lookup"><span data-stu-id="b31b0-123">**Skip this step** if you are loading the Contoso public data.</span></span> <span data-ttu-id="b31b0-124">A nyilvános adataihoz való biztonságos hozzáférés nem szükséges, mert már bárki hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="b31b0-124">You don't need secure access to the public data since it is already accessible to anyone.</span></span>

<span data-ttu-id="b31b0-125">**Ne hagyja ki ezt a lépést** használata ebben az oktatóanyagban sablonként a saját adatok feltöltését.</span><span class="sxs-lookup"><span data-stu-id="b31b0-125">**Don't skip this step** if you are using this tutorial as a template for loading your own data.</span></span> <span data-ttu-id="b31b0-126">Adatok eléréséhez a hitelesítő adatokat, használja a következő parancsfájl egy adatbázishoz kötődő hitelesítő adatok létrehozása, és majd használatra, ha az adatforrás helyének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b31b0-126">To access data through a credential, use the following script to create a database-scoped credential, and then use it when defining the location of the data source.</span></span>

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

<span data-ttu-id="b31b0-127">Ugorjon a 2.</span><span class="sxs-lookup"><span data-stu-id="b31b0-127">Skip to step 2.</span></span>

### <a name="12-create-the-external-data-source"></a><span data-ttu-id="b31b0-128">1.2.</span><span class="sxs-lookup"><span data-stu-id="b31b0-128">1.2.</span></span> <span data-ttu-id="b31b0-129">A külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b31b0-129">Create the external data source</span></span>
<span data-ttu-id="b31b0-130">Ezzel [külső ADATFORRÁS létrehozása] [ CREATE EXTERNAL DATA SOURCE] parancs a helyét, valamint az adatok, milyen típusú adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="b31b0-130">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command to store the location of the data, and the type of data.</span></span> 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> <span data-ttu-id="b31b0-131">Ha arra, hogy az azure blob storage tárolók nyilvánosságra választja, ne feledje, hogy az adatok tulajdonosa fizetnie kell adatok kilépő díjak amikor adatok elhagyja az Adatközpont.</span><span class="sxs-lookup"><span data-stu-id="b31b0-131">If you choose to make your azure blob storage containers public, remember that as the data owner you will be charged for data egress charges when data leaves the data center.</span></span> 
> 
> 

## <a name="2-configure-data-format"></a><span data-ttu-id="b31b0-132">2. Az adatformátum konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b31b0-132">2. Configure data format</span></span>
<span data-ttu-id="b31b0-133">Az adatok szöveg fájlok az Azure blob Storage tárolóban tárolják, és minden mező elválasztóval választja el egymástól.</span><span class="sxs-lookup"><span data-stu-id="b31b0-133">The data is stored in text files in Azure blob storage, and each field is separated with a delimiter.</span></span> <span data-ttu-id="b31b0-134">Ez [külső FÁJLFORMÁTUM létrehozása] [ CREATE EXTERNAL FILE FORMAT] parancs használatával adja meg az adatok formátumának a szövegfájlok.</span><span class="sxs-lookup"><span data-stu-id="b31b0-134">Run this [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] command to specify the format of the data in the text files.</span></span> <span data-ttu-id="b31b0-135">A Contoso adatok tömörítetlen és az adatcsatorna tagolt.</span><span class="sxs-lookup"><span data-stu-id="b31b0-135">The Contoso data is uncompressed and pipe delimited.</span></span>

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a><span data-ttu-id="b31b0-136">3. A külső táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="b31b0-136">3. Create the external tables</span></span>
<span data-ttu-id="b31b0-137">Most, hogy a megadott forrás- és a fájl adatformátum, készen áll a külső táblák létrehozására.</span><span class="sxs-lookup"><span data-stu-id="b31b0-137">Now that you have specified the data source and file format, you are ready to create the external tables.</span></span> 

### <a name="31-create-a-schema-for-the-data"></a><span data-ttu-id="b31b0-138">3.1.</span><span class="sxs-lookup"><span data-stu-id="b31b0-138">3.1.</span></span> <span data-ttu-id="b31b0-139">Hozzon létre egy sémát.</span><span class="sxs-lookup"><span data-stu-id="b31b0-139">Create a schema for the data.</span></span>
<span data-ttu-id="b31b0-140">A hely, a Contoso adatokat az adatbázisban tárolni, hozzon létre egy séma.</span><span class="sxs-lookup"><span data-stu-id="b31b0-140">To create a place to store the Contoso data in your database, create a schema.</span></span>

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a><span data-ttu-id="b31b0-141">3.2.</span><span class="sxs-lookup"><span data-stu-id="b31b0-141">3.2.</span></span> <span data-ttu-id="b31b0-142">Hozza létre a külső táblákat.</span><span class="sxs-lookup"><span data-stu-id="b31b0-142">Create the external tables.</span></span>
<span data-ttu-id="b31b0-143">Futtassa ezt a parancsfájlt a DimProduct és FactOnlineSales külső tábla létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b31b0-143">Run this script to create the DimProduct and FactOnlineSales external tables.</span></span> <span data-ttu-id="b31b0-144">Csak azt is itt az oszlopneveket és adattípusokat meghatározása, és a kötése a hely és az Azure blob storage fájlok formátuma.</span><span class="sxs-lookup"><span data-stu-id="b31b0-144">All we are doing here is defining column names and data types, and binding them to the location and format of the Azure blob storage files.</span></span> <span data-ttu-id="b31b0-145">A definíció SQL Data Warehouse tárolja, és az adatok továbbra is megtalálható az Azure Storage-Blobba.</span><span class="sxs-lookup"><span data-stu-id="b31b0-145">The definition is stored in SQL Data Warehouse and the data is still in the Azure Storage Blob.</span></span>

<span data-ttu-id="b31b0-146">A **hely** paraméter megadása a mappa alatt az Azure Storage-Blobba gyökérmappáját.</span><span class="sxs-lookup"><span data-stu-id="b31b0-146">The  **LOCATION** parameter is the folder under the root folder in the Azure Storage Blob.</span></span> <span data-ttu-id="b31b0-147">Minden tábla másik mappában van.</span><span class="sxs-lookup"><span data-stu-id="b31b0-147">Each table is in a different folder.</span></span>

```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a><span data-ttu-id="b31b0-148">4. Az adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="b31b0-148">4. Load the data</span></span>
<span data-ttu-id="b31b0-149">Külső adatok eléréséhez különböző módon van.</span><span class="sxs-lookup"><span data-stu-id="b31b0-149">There's different ways to access external data.</span></span>  <span data-ttu-id="b31b0-150">Adatok közvetlenül a külső tábla lekérdezése, az adatok betöltése az új adatbázistáblák vagy külső adatok hozzáadása a meglévő táblák.</span><span class="sxs-lookup"><span data-stu-id="b31b0-150">You can query data directly from the external table, load the data into new database tables, or add external data to existing database tables.</span></span>  

### <a name="41-create-a-new-schema"></a><span data-ttu-id="b31b0-151">4.1.</span><span class="sxs-lookup"><span data-stu-id="b31b0-151">4.1.</span></span> <span data-ttu-id="b31b0-152">Hozzon létre egy új sémát</span><span class="sxs-lookup"><span data-stu-id="b31b0-152">Create a new schema</span></span>
<span data-ttu-id="b31b0-153">CTAS új táblát hoz létre, amely adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b31b0-153">CTAS creates a new table that contains data.</span></span>  <span data-ttu-id="b31b0-154">Először hozzon létre egy contoso sémát.</span><span class="sxs-lookup"><span data-stu-id="b31b0-154">First, create a schema for the contoso data.</span></span>

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a><span data-ttu-id="b31b0-155">4.2.</span><span class="sxs-lookup"><span data-stu-id="b31b0-155">4.2.</span></span> <span data-ttu-id="b31b0-156">Az adatok betöltése az új táblák</span><span class="sxs-lookup"><span data-stu-id="b31b0-156">Load the data into new tables</span></span>
<span data-ttu-id="b31b0-157">Adatok betöltése az Azure blob storage és mentse a munkafüzetet egy táblát az adatbázis belül használja a [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást.</span><span class="sxs-lookup"><span data-stu-id="b31b0-157">To load data from Azure blob storage and save it in a table inside of your database, use the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="b31b0-158">A CTAS betöltése kihasználja az imént létrehozott szigorú típusmegadású külső táblákra. Adatok betöltése az új táblák, segítségével [CTAS] [ CTAS] táblánként utasítást.</span><span class="sxs-lookup"><span data-stu-id="b31b0-158">Loading with CTAS leverages the strongly typed external tables you have just created.To load the data into new tables, use one [CTAS][CTAS] statement per table.</span></span> 
 
<span data-ttu-id="b31b0-159">CTAS új táblát hoz létre, és feltölti a select utasítás a eredményekkel.</span><span class="sxs-lookup"><span data-stu-id="b31b0-159">CTAS creates a new table and populates it with the results of a select statement.</span></span> <span data-ttu-id="b31b0-160">CTAS határozza meg az új tábla ugyanazon oszlopok és adattípusok rendelkezik, mint a select utasítás eredményét.</span><span class="sxs-lookup"><span data-stu-id="b31b0-160">CTAS defines the new table to have the same columns and data types as the results of the select statement.</span></span> <span data-ttu-id="b31b0-161">Ha minden oszlop egy külső tábla, az új táblázat lesz a külső tábla az adattípusokat és az oszlopok egy replikát.</span><span class="sxs-lookup"><span data-stu-id="b31b0-161">If you select all the columns from an external table, the new table will be a replica of the columns and data types in the external table.</span></span>

<span data-ttu-id="b31b0-162">Ebben a példában azt hozni a dimenzió, illetve a ténytábla, elosztott táblák kivonat.</span><span class="sxs-lookup"><span data-stu-id="b31b0-162">In this example, we create both the dimension and the fact table as hash distributed tables.</span></span> 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a><span data-ttu-id="b31b0-163">4.3 a folyamat előrehaladását betöltése</span><span class="sxs-lookup"><span data-stu-id="b31b0-163">4.3 Track the load progress</span></span>
<span data-ttu-id="b31b0-164">A dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek) betöltési állapotának nyomon követheti.</span><span class="sxs-lookup"><span data-stu-id="b31b0-164">You can track the progress of your load using dynamic management views (DMVs).</span></span> 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a><span data-ttu-id="b31b0-165">5. Oszlopcentrikus tömörítés optimalizálása</span><span class="sxs-lookup"><span data-stu-id="b31b0-165">5. Optimize columnstore compression</span></span>
<span data-ttu-id="b31b0-166">Alapértelmezés szerint az SQL Data Warehouse tárolja a tábla fürtözött oszlopcentrikus index.</span><span class="sxs-lookup"><span data-stu-id="b31b0-166">By default, SQL Data Warehouse stores the table as a clustered columnstore index.</span></span> <span data-ttu-id="b31b0-167">A betöltés befejezése után az adatsorok némelyike esetleg nem tömörítse össze az oszlopcentrikus.</span><span class="sxs-lookup"><span data-stu-id="b31b0-167">After a load completes, some of the data rows might not be compressed into the columnstore.</span></span>  <span data-ttu-id="b31b0-168">Miért Ez akkor fordulhat elő, ennek több van.</span><span class="sxs-lookup"><span data-stu-id="b31b0-168">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="b31b0-169">További tudnivalókért lásd: [oszlopcentrikus Indexek kezelése][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="b31b0-169">To learn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="b31b0-170">Optimalizálható a teljesítmény-küszöbérték és a betöltés után oszlopcentrikus tömörítés, építse újra az oszloptárindexet az összes sor tömörítendő kényszerítése a tábla.</span><span class="sxs-lookup"><span data-stu-id="b31b0-170">To optimize query performance and columnstore compression after a load, rebuild the table to force the columnstore index to compress all the rows.</span></span> 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

<span data-ttu-id="b31b0-171">Az oszlopcentrikus indexek fenntartása További információkért lásd: a [oszlopcentrikus Indexek kezelése] [ manage columnstore indexes] cikk.</span><span class="sxs-lookup"><span data-stu-id="b31b0-171">For more information on maintaining columnstore indexes, see the [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="6-optimize-statistics"></a><span data-ttu-id="b31b0-172">6. Statisztika optimalizálása</span><span class="sxs-lookup"><span data-stu-id="b31b0-172">6. Optimize statistics</span></span>
<span data-ttu-id="b31b0-173">Érdemes a betöltés után azonnal létrehozza a egyoszlopos statisztikát.</span><span class="sxs-lookup"><span data-stu-id="b31b0-173">It is best to create single-column statistics immediately after a load.</span></span> <span data-ttu-id="b31b0-174">Nincsenek néhány statisztikai lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="b31b0-174">There are some choices for statistics.</span></span> <span data-ttu-id="b31b0-175">Például ha egyoszlopos statisztikát hoz létre minden egyes oszlophoz a statisztika, hogy hosszú ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="b31b0-175">For example, if you create single-column statistics on every column it might take a long time to rebuild all the statistics.</span></span> <span data-ttu-id="b31b0-176">Ha ismeri az egyes oszlopok nem tervezi, hogy a lekérdezés predikátumok kell, kihagyhatja a statisztikák létrehozása az ilyen oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="b31b0-176">If you know certain columns are not going to be in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="b31b0-177">Ha egy oszlop statisztikákat létrehozni minden tábla minden egyes oszlophoz, használhatja a tárolt eljárás kódminta `prc_sqldw_create_stats` a a [statisztika] [ statistics] cikk.</span><span class="sxs-lookup"><span data-stu-id="b31b0-177">If you decide to create single-column statistics on every column of every table, you can use the stored procedure code sample `prc_sqldw_create_stats` in the [statistics][statistics] article.</span></span>

<span data-ttu-id="b31b0-178">A következő példa: statisztikák létrehozása az jó kiindulási pont.</span><span class="sxs-lookup"><span data-stu-id="b31b0-178">The following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="b31b0-179">Minden egyes oszlopának a dimenziótáblában és a ténytáblák csatlakozó oszlopainak egyoszlopos statisztikát hoz.</span><span class="sxs-lookup"><span data-stu-id="b31b0-179">It creates single-column statistics on each column in the dimension table, and on each joining column in the fact tables.</span></span> <span data-ttu-id="b31b0-180">Mindig hozzáadhat egy vagy több oszlop statisztikai más tény táblaoszlopok később.</span><span class="sxs-lookup"><span data-stu-id="b31b0-180">You can always add single or multi-column statistics to other fact table columns later on.</span></span>

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a><span data-ttu-id="b31b0-181">Zárolása feloldva elérésének!</span><span class="sxs-lookup"><span data-stu-id="b31b0-181">Achievement unlocked!</span></span>
<span data-ttu-id="b31b0-182">Az Azure SQL Data Warehouse nyilvános adatok sikeresen töltött.</span><span class="sxs-lookup"><span data-stu-id="b31b0-182">You have successfully loaded public data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="b31b0-183">Remek munka!</span><span class="sxs-lookup"><span data-stu-id="b31b0-183">Great job!</span></span>

<span data-ttu-id="b31b0-184">Most elindíthatja a lekérdezésekkel például a következő táblázatok lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="b31b0-184">You can now start querying the tables using queries like the following:</span></span>

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a><span data-ttu-id="b31b0-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b31b0-185">Next steps</span></span>
<span data-ttu-id="b31b0-186">A teljes Contoso kereskedelmi Data Warehouse-adatok betöltése, a parancsfájl használata további fejlesztési tippek. További információ: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="b31b0-186">To load the full Contoso Retail Data Warehouse data, use the script in For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
