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
# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Adatok betöltése a PolyBase-zel az SQL Data Warehouse-ba
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Ez az oktatóanyag bemutatja, hogyan tooload adatokat az SQL Data Warehouse-bA az AzCopy és a PolyBase használatával. Az oktatóanyag végére elsajátíthatja a következőket:

* AzCopy toocopy adatok tooAzure blob storage használata
* Adatbázis-objektumok toodefine hello adatok létrehozása
* Futtassa a T-SQL lekérdezés tooload hello adatok

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-with-PolyBase-in-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Előfeltételek
az oktatóanyag teljesítéséhez toostep, szüksége

* Egy SQL Data Warehouse-adatbázis.
* Egy standard helyileg redundáns tárolás (Standard-LRS), standard georedundáns tárolás (Standard-GRS) vagy standard írásvédett georedundáns tárolás (Standard-RAGRS) típusú Azure Storage-fiók.
* AzCopy parancssori segédprogram Töltse le és telepítse a hello [az AzCopy legújabb verzióját] [ latest version of AzCopy] amely hello Microsoft Azure Storage-eszközökkel együtt települ.
  
    ![Azure Storage-eszközök](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)

## <a name="step-1-add-sample-data-tooazure-blob-storage"></a>1. lépés: A minta adatok tooAzure blob-tároló hozzáadása
Rendelés tooload adatok igazolnia kell tooput néhány adatot az Azure blob storage-be. Ebben a lépésben feltöltünk egy Azure Storage-blobot mintaadatokkal. Később használjuk a PolyBase tooload ezeket a mintaadatokat az SQL Data Warehouse-adatbázisba.

### <a name="a-prepare-a-sample-text-file"></a>A. Minta szöveges fájl előkészítése
minta szöveges fájl tooprepare:

1. Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új fájlba. % Temp%\DimDate2.txt a tooyour helyi ideiglenes könyvtárba menteni.

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

### <a name="b-find-your-blob-service-endpoint"></a>B. A Blob-szolgáltatásvégpont megkeresése
toofind a blob-szolgáltatásvégpont:

1. Hello Azure portálon válassza **Tallózás** > **Tárfiókok**.
2. Kattintson a kívánt toouse hello tárfiókra.
3. Hello Storage-fiók panelen kattintson a Blobok elemre
   
    ![Kattintson a Blobok elemre](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)
4. Mentse a Blob-szolgáltatásvégpontot későbbi használatra.
   
    ![Blob-szolgáltatásvégpont](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Az Azure Storage-kulcs megkeresése
toofind az Azure storage-kulcs:

1. Hello Azure portált, válassza ki **Tallózás** > **Tárfiókok**.
2. Kattintson a kívánt toouse hello tárfiók.
3. Válassza az **Összes beállítás** > **Hívóbetűk** lehetőséget.
4. Kattintson a hello másolási mezőben toocopy a hozzáférési kulcsok toohello vágólapra egyikét.
   
    ![Az Azure Storage-kulcs másolása](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-hello-sample-file-tooazure-blob-storage"></a>D. Másolja a hello minta fájl tooAzure blob-tároló
toocopy az adatok tooAzure blob-tároló:

1. Nyisson meg egy parancssort, és módosítsa a könyvtárakat toohello AzCopy telepítési könyvtárára. Ez a parancs módosítja egy 64 bites Windows-ügyfelén toohello alapértelmezett telepítési könyvtárra.
   
    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```
2. Futtassa a következő parancs tooupload hello fájl hello. Adja meg a(z) <blob service endpoint URL> Blob-szolgáltatásvégpont URL-jét és az <azure_storage_account_key> Azure Storage-fiók kulcsát.
   
    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Lásd még: [Ismerkedés az AzCopy parancssori segédprogram hello][Getting Started with hello AzCopy Command-Line Utility].

### <a name="e-explore-your-blob-storage-container"></a>E. A Blob Storage-tároló áttekintése
toosee hello tooblob tárolási feltöltött fájlban:

1. Lépjen vissza a tooyour Blob szolgáltatás panelre.
2. A Tárolók területen kattintson duplán a **datacontainer** elemre.
3. tooexplore hello elérési tooyour adatokat, kattintson a hello mappa **datedimension** és látni fogja a feltöltött fájl **DimDate2.txt**.
4. tooview tulajdonságait, kattintson a **DimDate2.txt**.
5. Vegye figyelembe, hogy a hello Blob tulajdonságai panelen letöltheti és hello fájl törlése.
   
    ![Az Azure Storage-blob megtekintése](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)

## <a name="step-2-create-an-external-table-for-hello-sample-data"></a>2. lépés: Mintaadatok hello a külső tábla létrehozása
Ebben a szakaszban létrehozhatunk egy külső táblát, amely meghatározza a hello mintaadatok.

A polybase külső táblák tooaccess adatok Azure blob Storage tárolóban. Hello adatok nem tárolja az SQL Data Warehouse, mivel a PolyBase kezeli a hitelesítési toohello külső adatokat egy adatbázishoz kötődő hitelesítő adatok használatával.

hello példa ebben a lépésben a Transact-SQL utasítás toocreate a külső tábla használja.

* [Hozzon létre Master Key (Transact-SQL)] [ Create Master Key (Transact-SQL)] tooencrypt hello titka az adatbázishoz kötődő hitelesítő adatok.
* [Hozzon létre Database Scoped Credential (Transact-SQL)] [ Create Database Scoped Credential (Transact-SQL)] toospecify hitelesítési adatainak megadása az Azure storage-fiók.
* [Külső adatforrás (Transact-SQL) létrehozása] [ Create External Data Source (Transact-SQL)] az Azure blob storage toospecify hello helyét.
* [Hozzon létre a külső fájlformátumot (Transact-SQL)] [ Create External File Format (Transact-SQL)] toospecify hello adatformátum.
* [Hozzon létre a külső tábla (Transact-SQL)] [ Create External Table (Transact-SQL)] toospecify hello tábladefiníció és helyének hello adatokat.

Futtassa ezt a lekérdezést az SQL Data Warehouse-adatbázison. Egy külső táblát dimdate2external néven hello dbo sémában, amely toohello DimDate2.txt mintaadataira mutat hello Azure blob Storage tárolóban hoz létre.

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


Az SQL Server Object Explorerben a Visual Studio láthatja, hogy hello külső fájlformátumot, a külső adatforrást és a hello DimDate2External táblát.

![Külső tábla megtekintése](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>3. lépés: Adatok betöltése az SQL Data Warehouse-ba
Hello külső tábla létrehozása után hello adatok betöltése az új tábla, vagy szúrja be egy meglévő táblába.

* tooload hello adatok új táblába, futtassa a hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást. hello új tábla tartalmazza hello hello lekérdezésben szereplő oszlopokat. hello oszlopok adattípusai hello fog egyezni hello adattípusok a hello külső tábla definíciójában.
* tooload hello adatok meglévő táblába, használja a hello [INSERT... SELECT (Transact-SQL)] [ INSERT...SELECT (Transact-SQL)] utasítást.

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

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>4. lépés: Statisztikák létrehozása az újonnan betöltött adatokról
Az SQL Data Warehouse nem tudja automatikus létrehozni és frissíteni a statisztikákat. Ezért tooachieve a lekérdezési teljesítmény, fontos toocreate statisztika hello után minden tábla minden oszlopához először betölteni. Célszerű is fontos tooupdate statisztika hello adatok lényeges módosításai után.

Ez a példa egyoszlopos statisztikát hoz létre hello új DimDate2 táblához.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

több, lásd: toolearn [statisztika][Statistics].  

## <a name="next-steps"></a>Következő lépések
Lásd: hello [PolyBase-útmutatóban] [ PolyBase guide] -t a PolyBase használó megoldások fejlesztéséről további információkat.

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
