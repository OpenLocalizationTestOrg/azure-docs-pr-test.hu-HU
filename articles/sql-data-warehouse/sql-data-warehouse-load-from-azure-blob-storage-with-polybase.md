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
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Adatok betöltése az Azure blob storage az SQL Data warehouse-ba (PolyBase)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

Az Azure SQL Data Warehouse PolyBase és T-SQL parancsokkal tooload adatokat az Azure blob storage használata 

egyszerű, ez az oktatóanyag betölti a két tábla az egy nyilvános Azure Storage-Blobból hello Contoso kereskedelmi adatraktár sémába tookeep. tooload hello teljes adatkészlet, futtassa a hello példa [terhelés hello teljes Contoso kereskedelmi adatraktár] [ Load hello full Contoso Retail Data Warehouse] adattárból hello Microsoft SQL Server minták.

Az oktatóanyag tartalma:

1. Az Azure blob storage PolyBase tooload konfigurálása
2. Nyilvános adatok betöltése az adatbázisba
3. Hajtsa végre a optimalizálásokat hello betöltése után.

## <a name="before-you-begin"></a>Előkészületek
toorun ebben az oktatóanyagban kell Azure-fiókot, amely már rendelkezik egy SQL Data Warehouse-adatbázis. Ha még nem rendelkezik ezzel, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].

## <a name="1-configure-hello-data-source"></a>1. Hello adatforrás konfigurálása
A polybase T-SQL külső objektumok toodefine hello helyét és hello külső adatokra vonatkozó attribútumok. hello külső objektum tárolja az SQL Data Warehouse. hello maga tárolja kívülről.

### <a name="11-create-a-credential"></a>1.1. Hitelesítő adatok létrehozása
**Kihagyhatja ezt a lépést** Ha hello Contoso nyilvános adatokat tölt be. Biztonságos hozzáférés toohello nyilvános adatok nem szükséges, mert az már elérhető tooanyone.

**Ne hagyja ki ezt a lépést** használata ebben az oktatóanyagban sablonként a saját adatok feltöltését. hitelesítő adatokat, a következő használatát hello tooaccess adatokat parancsfájl toocreate egy adatbázishoz kötődő hitelesítő adatok, és használja azt hello adatforrás hello helyét meghatározásakor.

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

Kihagyás toostep 2.

### <a name="12-create-hello-external-data-source"></a>1.2. Hello külső adatforrás létrehozása
Ezzel [külső ADATFORRÁS létrehozása] [ CREATE EXTERNAL DATA SOURCE] toostore hello helyét, valamint hello adatok hello típusú adatok parancsot. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> Ha úgy dönt, toomake az azure blob storage tárolók nyilvános, ne feledje, hogy hello adatok tulajdonosaként fizetnie kell adatok kilépő díjak adatok elhagyásakor hello adatközpont. 
> 
> 

## <a name="2-configure-data-format"></a>2. Az adatformátum konfigurálása
hello adatok tárolódnak az Azure blob storage szövegfájlok, és minden mező elválasztóval választja el egymástól. Ez [külső FÁJLFORMÁTUM létrehozása] [ CREATE EXTERNAL FILE FORMAT] hello adatok hello szövegfájlok, és a parancs toospecify hello formátuma. hello Contoso adatok tömörítetlen, és az adatcsatorna tagolt.

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

## <a name="3-create-hello-external-tables"></a>3. Hello külső táblák létrehozása
Most, hogy a megadott hello forrás- és a fájl formátuma, készen áll a toocreate hello külső táblák áll. 

### <a name="31-create-a-schema-for-hello-data"></a>3.1. Hozzon létre egy séma hello adatok.
toocreate egy hely toostore hello Contoso adatokat az adatbázisban séma létrehozása.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-hello-external-tables"></a>3.2. Hello külső táblák létrehozása.
Futtassa a parancsfájl toocreate hello DimProduct és FactOnlineSales külső táblákon. Csak azt is itt az oszlopneveket és adattípusokat meghatározása, és kötelező toohello helyét és hello Azure blob storage fájlok formátuma. hello definition SQL Data Warehouse tárolja és hello adatok továbbra is a hello Azure Storage-Blobba.

Hello **hely** paraméter hello gyökérmappáját hello Azure Storage-Blobba hello mappát. Minden tábla másik mappában van.

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

## <a name="4-load-hello-data"></a>4. Hello adatok betöltése
Nincs elérhető különböző módokon tooaccess külső adat.  Kérdezhet le adatokat közvetlenül a hello külső tábla, hello adatok betöltése az új adatbázistáblák, vagy vegye fel a külső adatokat tooexisting adatbázistáblák.  

### <a name="41-create-a-new-schema"></a>4.1. Hozzon létre egy új sémát
CTAS új táblát hoz létre, amely adatokat tartalmaz.  Először hozzon létre egy séma hello contoso adatok.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-hello-data-into-new-tables"></a>4.2. Új hello adatok betöltése
tooload adatokat az Azure blob-tároló, és mentse azt egy tábla belül az adatbázis, használja a hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást. A CTAS betöltése használja hello erős típusmegadású külső táblák csak created.tooload hello adatok új táblába, használjon egy [CTAS] [ CTAS] táblánként utasítást. 
 
CTAS új táblát hoz létre, és feltölti a select utasítás hello eredményekkel. CTAS meghatározása hello új tábla toohave azonos oszlopok és adattípusok hello hello hello eredményeit válasszon ki az utasítást. Ha minden hello oszlop egy külső tábla, hello új tábla hello oszlopok és típusok adatok replikáját hello külső tábla lesz.

Ebben a példában létrehozhatunk hello dimenzió és a hello ténytábla, elosztott táblák kivonat. 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-hello-load-progress"></a>4.3 nyomon hello betöltése folyamatban
Nyomon követheti a dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek) betöltési hello előrehaladását. 

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

## <a name="5-optimize-columnstore-compression"></a>5. Oszlopcentrikus tömörítés optimalizálása
Alapértelmezés szerint az SQL Data Warehouse tárolja hello tábla fürtözött oszlopcentrikus index. A betöltés befejezése után néhány hello adatsorok előfordulhat, hogy nem lehet tömörített hello oszlopcentrikus.  Miért Ez akkor fordulhat elő, ennek több van. több, lásd: toolearn [oszlopcentrikus Indexek kezelése][manage columnstore indexes].

toooptimize lekérdezési teljesítmény és a betöltés után oszlopcentrikus tömörítési építse újra hello tábla tooforce hello oszlopcentrikus index toocompress hello összes sort. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Az oszlopcentrikus indexek fenntartása További információkért lásd: hello [oszlopcentrikus Indexek kezelése] [ manage columnstore indexes] cikk.

## <a name="6-optimize-statistics"></a>6. Statisztika optimalizálása
A betöltés után azonnal is ajánlott toocreate egyoszlopos statisztikát. Nincsenek néhány statisztikai lehetőségeit. Például ha egyoszlopos statisztikát hoz létre minden egyes oszlophoz eltarthat egy hosszú ideig toorebuild összes hello statisztikáját. Ha ismeri az egyes oszlopok nem fog az lekérdezés predikátumokban toobe, kihagyhatja a statisztikák létrehozása az ilyen oszlopokat.

Ha úgy dönt, hogy minden tábla minden egyes oszlophoz a toocreate egyoszlopos statisztikát, használhatja a hello tárolt eljárás kódminta `prc_sqldw_create_stats` a hello [statisztika] [ statistics] cikk.

a következő példa hello statisztikák létrehozása az jó kiindulási pont. Egyoszlopos statisztikát hoz hello dimenzió tábla összes oszlopa, és minden hello ténytáblák csatlakozó oszlopában. Bármikor hozzáadhat egy vagy több oszlop statisztika tooother tény táblaoszlopok később.

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

## <a name="achievement-unlocked"></a>Zárolása feloldva elérésének!
Az Azure SQL Data Warehouse nyilvános adatok sikeresen töltött. Remek munka!

Most elindíthatja a lekérdezésekkel hasonló hello hello táblák lekérdezése:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Következő lépések
tooload hello teljes Contoso kereskedelmi Data Warehouse-adatok, parancsfájllal hello a további fejlesztési tippek című [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].

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
