---
title: "az Azure blob tooAzure adatraktárból aaaLoad |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse PolyBase tooload adatok Azure blob-tároló az SQL Data Warehouse. A nyilvános adatok hello Contoso kereskedelmi adatraktár sémába néhány táblák betöltése."
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
ms.openlocfilehash: 4b4978ccefa4d55ff5c89fba84c5e705422ddbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a><span data-ttu-id="5c50d-104">Adatok betöltése az Azure blob storage az SQL Data warehouse-ba (PolyBase)</span><span class="sxs-lookup"><span data-stu-id="5c50d-104">Load data from Azure blob storage into SQL Data Warehouse (PolyBase)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5c50d-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="5c50d-105">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="5c50d-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="5c50d-106">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

<span data-ttu-id="5c50d-107">Az Azure SQL Data Warehouse PolyBase és T-SQL parancsokkal tooload adatokat az Azure blob storage használata</span><span class="sxs-lookup"><span data-stu-id="5c50d-107">Use PolyBase and T-SQL commands tooload data from Azure blob storage into Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="5c50d-108">egyszerű, ez az oktatóanyag betölti a két tábla az egy nyilvános Azure Storage-Blobból hello Contoso kereskedelmi adatraktár sémába tookeep.</span><span class="sxs-lookup"><span data-stu-id="5c50d-108">tookeep it simple, this tutorial loads two tables from a public Azure Storage Blob into hello Contoso Retail Data Warehouse schema.</span></span> <span data-ttu-id="5c50d-109">tooload hello teljes adatkészlet, futtassa a hello példa [terhelés hello teljes Contoso kereskedelmi adatraktár] [ Load hello full Contoso Retail Data Warehouse] adattárból hello Microsoft SQL Server minták.</span><span class="sxs-lookup"><span data-stu-id="5c50d-109">tooload hello full data set, run hello example [Load hello full Contoso Retail Data Warehouse][Load hello full Contoso Retail Data Warehouse] from hello Microsoft SQL Server Samples repository.</span></span>

<span data-ttu-id="5c50d-110">Az oktatóanyag tartalma:</span><span class="sxs-lookup"><span data-stu-id="5c50d-110">In this tutorial you will:</span></span>

1. <span data-ttu-id="5c50d-111">Az Azure blob storage PolyBase tooload konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c50d-111">Configure PolyBase tooload from Azure blob storage</span></span>
2. <span data-ttu-id="5c50d-112">Nyilvános adatok betöltése az adatbázisba</span><span class="sxs-lookup"><span data-stu-id="5c50d-112">Load public data into your database</span></span>
3. <span data-ttu-id="5c50d-113">Hajtsa végre a optimalizálásokat hello betöltése után.</span><span class="sxs-lookup"><span data-stu-id="5c50d-113">Perform optimizations after hello load is finished.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5c50d-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5c50d-114">Before you begin</span></span>
<span data-ttu-id="5c50d-115">toorun ebben az oktatóanyagban kell Azure-fiókot, amely már rendelkezik egy SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5c50d-115">toorun this tutorial, you need an Azure account that already has a SQL Data Warehouse database.</span></span> <span data-ttu-id="5c50d-116">Ha még nem rendelkezik ezzel, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="5c50d-116">If you don't already have this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>

## <a name="1-configure-hello-data-source"></a><span data-ttu-id="5c50d-117">1. Hello adatforrás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c50d-117">1. Configure hello data source</span></span>
<span data-ttu-id="5c50d-118">A polybase T-SQL külső objektumok toodefine hello helyét és hello külső adatokra vonatkozó attribútumok.</span><span class="sxs-lookup"><span data-stu-id="5c50d-118">PolyBase uses T-SQL external objects toodefine hello location and attributes of hello external data.</span></span> <span data-ttu-id="5c50d-119">hello külső objektum tárolja az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5c50d-119">hello external object definitions are stored in SQL Data Warehouse.</span></span> <span data-ttu-id="5c50d-120">hello maga tárolja kívülről.</span><span class="sxs-lookup"><span data-stu-id="5c50d-120">hello data itself is stored externally.</span></span>

### <a name="11-create-a-credential"></a><span data-ttu-id="5c50d-121">1.1.</span><span class="sxs-lookup"><span data-stu-id="5c50d-121">1.1.</span></span> <span data-ttu-id="5c50d-122">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c50d-122">Create a credential</span></span>
<span data-ttu-id="5c50d-123">**Kihagyhatja ezt a lépést** Ha hello Contoso nyilvános adatokat tölt be.</span><span class="sxs-lookup"><span data-stu-id="5c50d-123">**Skip this step** if you are loading hello Contoso public data.</span></span> <span data-ttu-id="5c50d-124">Biztonságos hozzáférés toohello nyilvános adatok nem szükséges, mert az már elérhető tooanyone.</span><span class="sxs-lookup"><span data-stu-id="5c50d-124">You don't need secure access toohello public data since it is already accessible tooanyone.</span></span>

<span data-ttu-id="5c50d-125">**Ne hagyja ki ezt a lépést** használata ebben az oktatóanyagban sablonként a saját adatok feltöltését.</span><span class="sxs-lookup"><span data-stu-id="5c50d-125">**Don't skip this step** if you are using this tutorial as a template for loading your own data.</span></span> <span data-ttu-id="5c50d-126">hitelesítő adatokat, a következő használatát hello tooaccess adatokat parancsfájl toocreate egy adatbázishoz kötődő hitelesítő adatok, és használja azt hello adatforrás hello helyét meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="5c50d-126">tooaccess data through a credential, use hello following script toocreate a database-scoped credential, and then use it when defining hello location of hello data source.</span></span>

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
```

<span data-ttu-id="5c50d-127">Kihagyás toostep 2.</span><span class="sxs-lookup"><span data-stu-id="5c50d-127">Skip toostep 2.</span></span>

### <a name="12-create-hello-external-data-source"></a><span data-ttu-id="5c50d-128">1.2.</span><span class="sxs-lookup"><span data-stu-id="5c50d-128">1.2.</span></span> <span data-ttu-id="5c50d-129">Hello külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c50d-129">Create hello external data source</span></span>
<span data-ttu-id="5c50d-130">Ezzel [külső ADATFORRÁS létrehozása] [ CREATE EXTERNAL DATA SOURCE] toostore hello helyét, valamint hello adatok hello típusú adatok parancsot.</span><span class="sxs-lookup"><span data-stu-id="5c50d-130">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command toostore hello location of hello data, and hello type of data.</span></span> 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> <span data-ttu-id="5c50d-131">Ha úgy dönt, toomake az azure blob storage tárolók nyilvános, ne feledje, hogy hello adatok tulajdonosaként fizetnie kell adatok kilépő díjak adatok elhagyásakor hello adatközpont.</span><span class="sxs-lookup"><span data-stu-id="5c50d-131">If you choose toomake your azure blob storage containers public, remember that as hello data owner you will be charged for data egress charges when data leaves hello data center.</span></span> 
> 
> 

## <a name="2-configure-data-format"></a><span data-ttu-id="5c50d-132">2. Az adatformátum konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c50d-132">2. Configure data format</span></span>
<span data-ttu-id="5c50d-133">hello adatok tárolódnak az Azure blob storage szövegfájlok, és minden mező elválasztóval választja el egymástól.</span><span class="sxs-lookup"><span data-stu-id="5c50d-133">hello data is stored in text files in Azure blob storage, and each field is separated with a delimiter.</span></span> <span data-ttu-id="5c50d-134">Ez [külső FÁJLFORMÁTUM létrehozása] [ CREATE EXTERNAL FILE FORMAT] hello adatok hello szövegfájlok, és a parancs toospecify hello formátuma.</span><span class="sxs-lookup"><span data-stu-id="5c50d-134">Run this [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] command toospecify hello format of hello data in hello text files.</span></span> <span data-ttu-id="5c50d-135">hello Contoso adatok tömörítetlen, és az adatcsatorna tagolt.</span><span class="sxs-lookup"><span data-stu-id="5c50d-135">hello Contoso data is uncompressed and pipe delimited.</span></span>

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

## <a name="3-create-hello-external-tables"></a><span data-ttu-id="5c50d-136">3. Hello külső táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c50d-136">3. Create hello external tables</span></span>
<span data-ttu-id="5c50d-137">Most, hogy a megadott hello forrás- és a fájl formátuma, készen áll a toocreate hello külső táblák áll.</span><span class="sxs-lookup"><span data-stu-id="5c50d-137">Now that you have specified hello data source and file format, you are ready toocreate hello external tables.</span></span> 

### <a name="31-create-a-schema-for-hello-data"></a><span data-ttu-id="5c50d-138">3.1.</span><span class="sxs-lookup"><span data-stu-id="5c50d-138">3.1.</span></span> <span data-ttu-id="5c50d-139">Hozzon létre egy séma hello adatok.</span><span class="sxs-lookup"><span data-stu-id="5c50d-139">Create a schema for hello data.</span></span>
<span data-ttu-id="5c50d-140">toocreate egy hely toostore hello Contoso adatokat az adatbázisban séma létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5c50d-140">toocreate a place toostore hello Contoso data in your database, create a schema.</span></span>

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-hello-external-tables"></a><span data-ttu-id="5c50d-141">3.2.</span><span class="sxs-lookup"><span data-stu-id="5c50d-141">3.2.</span></span> <span data-ttu-id="5c50d-142">Hello külső táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5c50d-142">Create hello external tables.</span></span>
<span data-ttu-id="5c50d-143">Futtassa a parancsfájl toocreate hello DimProduct és FactOnlineSales külső táblákon.</span><span class="sxs-lookup"><span data-stu-id="5c50d-143">Run this script toocreate hello DimProduct and FactOnlineSales external tables.</span></span> <span data-ttu-id="5c50d-144">Csak azt is itt az oszlopneveket és adattípusokat meghatározása, és kötelező toohello helyét és hello Azure blob storage fájlok formátuma.</span><span class="sxs-lookup"><span data-stu-id="5c50d-144">All we are doing here is defining column names and data types, and binding them toohello location and format of hello Azure blob storage files.</span></span> <span data-ttu-id="5c50d-145">hello definition SQL Data Warehouse tárolja és hello adatok továbbra is a hello Azure Storage-Blobba.</span><span class="sxs-lookup"><span data-stu-id="5c50d-145">hello definition is stored in SQL Data Warehouse and hello data is still in hello Azure Storage Blob.</span></span>

<span data-ttu-id="5c50d-146">Hello **hely** paraméter hello gyökérmappáját hello Azure Storage-Blobba hello mappát.</span><span class="sxs-lookup"><span data-stu-id="5c50d-146">hello  **LOCATION** parameter is hello folder under hello root folder in hello Azure Storage Blob.</span></span> <span data-ttu-id="5c50d-147">Minden tábla másik mappában van.</span><span class="sxs-lookup"><span data-stu-id="5c50d-147">Each table is in a different folder.</span></span>

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

## <a name="4-load-hello-data"></a><span data-ttu-id="5c50d-148">4. Hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="5c50d-148">4. Load hello data</span></span>
<span data-ttu-id="5c50d-149">Nincs elérhető különböző módokon tooaccess külső adat.</span><span class="sxs-lookup"><span data-stu-id="5c50d-149">There's different ways tooaccess external data.</span></span>  <span data-ttu-id="5c50d-150">Kérdezhet le adatokat közvetlenül a hello külső tábla, hello adatok betöltése az új adatbázistáblák, vagy vegye fel a külső adatokat tooexisting adatbázistáblák.</span><span class="sxs-lookup"><span data-stu-id="5c50d-150">You can query data directly from hello external table, load hello data into new database tables, or add external data tooexisting database tables.</span></span>  

### <a name="41-create-a-new-schema"></a><span data-ttu-id="5c50d-151">4.1.</span><span class="sxs-lookup"><span data-stu-id="5c50d-151">4.1.</span></span> <span data-ttu-id="5c50d-152">Hozzon létre egy új sémát</span><span class="sxs-lookup"><span data-stu-id="5c50d-152">Create a new schema</span></span>
<span data-ttu-id="5c50d-153">CTAS új táblát hoz létre, amely adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5c50d-153">CTAS creates a new table that contains data.</span></span>  <span data-ttu-id="5c50d-154">Először hozzon létre egy séma hello contoso adatok.</span><span class="sxs-lookup"><span data-stu-id="5c50d-154">First, create a schema for hello contoso data.</span></span>

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-hello-data-into-new-tables"></a><span data-ttu-id="5c50d-155">4.2.</span><span class="sxs-lookup"><span data-stu-id="5c50d-155">4.2.</span></span> <span data-ttu-id="5c50d-156">Új hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="5c50d-156">Load hello data into new tables</span></span>
<span data-ttu-id="5c50d-157">tooload adatokat az Azure blob-tároló, és mentse azt egy tábla belül az adatbázis, használja a hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást.</span><span class="sxs-lookup"><span data-stu-id="5c50d-157">tooload data from Azure blob storage and save it in a table inside of your database, use hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="5c50d-158">A CTAS betöltése használja hello erős típusmegadású külső táblák csak created.tooload hello adatok új táblába, használjon egy [CTAS] [ CTAS] táblánként utasítást.</span><span class="sxs-lookup"><span data-stu-id="5c50d-158">Loading with CTAS leverages hello strongly typed external tables you have just created.tooload hello data into new tables, use one [CTAS][CTAS] statement per table.</span></span> 
 
<span data-ttu-id="5c50d-159">CTAS új táblát hoz létre, és feltölti a select utasítás hello eredményekkel.</span><span class="sxs-lookup"><span data-stu-id="5c50d-159">CTAS creates a new table and populates it with hello results of a select statement.</span></span> <span data-ttu-id="5c50d-160">CTAS meghatározása hello új tábla toohave azonos oszlopok és adattípusok hello hello hello eredményeit válasszon ki az utasítást.</span><span class="sxs-lookup"><span data-stu-id="5c50d-160">CTAS defines hello new table toohave hello same columns and data types as hello results of hello select statement.</span></span> <span data-ttu-id="5c50d-161">Ha minden hello oszlop egy külső tábla, hello új tábla hello oszlopok és típusok adatok replikáját hello külső tábla lesz.</span><span class="sxs-lookup"><span data-stu-id="5c50d-161">If you select all hello columns from an external table, hello new table will be a replica of hello columns and data types in hello external table.</span></span>

<span data-ttu-id="5c50d-162">Ebben a példában létrehozhatunk hello dimenzió és a hello ténytábla, elosztott táblák kivonat.</span><span class="sxs-lookup"><span data-stu-id="5c50d-162">In this example, we create both hello dimension and hello fact table as hash distributed tables.</span></span> 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-hello-load-progress"></a><span data-ttu-id="5c50d-163">4.3 nyomon hello betöltése folyamatban</span><span class="sxs-lookup"><span data-stu-id="5c50d-163">4.3 Track hello load progress</span></span>
<span data-ttu-id="5c50d-164">Nyomon követheti a dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek) betöltési hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="5c50d-164">You can track hello progress of your load using dynamic management views (DMVs).</span></span> 

```sql
-- toosee all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- toosee a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- tootrack bytes and files
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

## <a name="5-optimize-columnstore-compression"></a><span data-ttu-id="5c50d-165">5. Oszlopcentrikus tömörítés optimalizálása</span><span class="sxs-lookup"><span data-stu-id="5c50d-165">5. Optimize columnstore compression</span></span>
<span data-ttu-id="5c50d-166">Alapértelmezés szerint az SQL Data Warehouse tárolja hello tábla fürtözött oszlopcentrikus index.</span><span class="sxs-lookup"><span data-stu-id="5c50d-166">By default, SQL Data Warehouse stores hello table as a clustered columnstore index.</span></span> <span data-ttu-id="5c50d-167">A betöltés befejezése után néhány hello adatsorok előfordulhat, hogy nem lehet tömörített hello oszlopcentrikus.</span><span class="sxs-lookup"><span data-stu-id="5c50d-167">After a load completes, some of hello data rows might not be compressed into hello columnstore.</span></span>  <span data-ttu-id="5c50d-168">Miért Ez akkor fordulhat elő, ennek több van.</span><span class="sxs-lookup"><span data-stu-id="5c50d-168">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="5c50d-169">több, lásd: toolearn [oszlopcentrikus Indexek kezelése][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="5c50d-169">toolearn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="5c50d-170">toooptimize lekérdezési teljesítmény és a betöltés után oszlopcentrikus tömörítési építse újra hello tábla tooforce hello oszlopcentrikus index toocompress hello összes sort.</span><span class="sxs-lookup"><span data-stu-id="5c50d-170">toooptimize query performance and columnstore compression after a load, rebuild hello table tooforce hello columnstore index toocompress all hello rows.</span></span> 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

<span data-ttu-id="5c50d-171">Az oszlopcentrikus indexek fenntartása További információkért lásd: hello [oszlopcentrikus Indexek kezelése] [ manage columnstore indexes] cikk.</span><span class="sxs-lookup"><span data-stu-id="5c50d-171">For more information on maintaining columnstore indexes, see hello [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="6-optimize-statistics"></a><span data-ttu-id="5c50d-172">6. Statisztika optimalizálása</span><span class="sxs-lookup"><span data-stu-id="5c50d-172">6. Optimize statistics</span></span>
<span data-ttu-id="5c50d-173">A betöltés után azonnal is ajánlott toocreate egyoszlopos statisztikát.</span><span class="sxs-lookup"><span data-stu-id="5c50d-173">It is best toocreate single-column statistics immediately after a load.</span></span> <span data-ttu-id="5c50d-174">Nincsenek néhány statisztikai lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="5c50d-174">There are some choices for statistics.</span></span> <span data-ttu-id="5c50d-175">Például ha egyoszlopos statisztikát hoz létre minden egyes oszlophoz eltarthat egy hosszú ideig toorebuild összes hello statisztikáját.</span><span class="sxs-lookup"><span data-stu-id="5c50d-175">For example, if you create single-column statistics on every column it might take a long time toorebuild all hello statistics.</span></span> <span data-ttu-id="5c50d-176">Ha ismeri az egyes oszlopok nem fog az lekérdezés predikátumokban toobe, kihagyhatja a statisztikák létrehozása az ilyen oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="5c50d-176">If you know certain columns are not going toobe in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="5c50d-177">Ha úgy dönt, hogy minden tábla minden egyes oszlophoz a toocreate egyoszlopos statisztikát, használhatja a hello tárolt eljárás kódminta `prc_sqldw_create_stats` a hello [statisztika] [ statistics] cikk.</span><span class="sxs-lookup"><span data-stu-id="5c50d-177">If you decide toocreate single-column statistics on every column of every table, you can use hello stored procedure code sample `prc_sqldw_create_stats` in hello [statistics][statistics] article.</span></span>

<span data-ttu-id="5c50d-178">a következő példa hello statisztikák létrehozása az jó kiindulási pont.</span><span class="sxs-lookup"><span data-stu-id="5c50d-178">hello following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="5c50d-179">Egyoszlopos statisztikát hoz hello dimenzió tábla összes oszlopa, és minden hello ténytáblák csatlakozó oszlopában.</span><span class="sxs-lookup"><span data-stu-id="5c50d-179">It creates single-column statistics on each column in hello dimension table, and on each joining column in hello fact tables.</span></span> <span data-ttu-id="5c50d-180">Bármikor hozzáadhat egy vagy több oszlop statisztika tooother tény táblaoszlopok később.</span><span class="sxs-lookup"><span data-stu-id="5c50d-180">You can always add single or multi-column statistics tooother fact table columns later on.</span></span>

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

## <a name="achievement-unlocked"></a><span data-ttu-id="5c50d-181">Zárolása feloldva elérésének!</span><span class="sxs-lookup"><span data-stu-id="5c50d-181">Achievement unlocked!</span></span>
<span data-ttu-id="5c50d-182">Az Azure SQL Data Warehouse nyilvános adatok sikeresen töltött.</span><span class="sxs-lookup"><span data-stu-id="5c50d-182">You have successfully loaded public data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5c50d-183">Remek munka!</span><span class="sxs-lookup"><span data-stu-id="5c50d-183">Great job!</span></span>

<span data-ttu-id="5c50d-184">Most elindíthatja a lekérdezésekkel hasonló hello hello táblák lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="5c50d-184">You can now start querying hello tables using queries like hello following:</span></span>

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a><span data-ttu-id="5c50d-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c50d-185">Next steps</span></span>
<span data-ttu-id="5c50d-186">tooload hello teljes Contoso kereskedelmi Data Warehouse-adatok, parancsfájllal hello a további fejlesztési tippek című [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="5c50d-186">tooload hello full Contoso Retail Data Warehouse data, use hello script in For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
[Load hello full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
