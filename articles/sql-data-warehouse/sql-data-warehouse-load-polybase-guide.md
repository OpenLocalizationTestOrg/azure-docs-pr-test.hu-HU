---
title: "A PolyBase használatával az SQL Data Warehouse útmutatója |} Microsoft Docs"
description: "Irányelvek és javaslatok a PolyBase az SQL Data Warehouse forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 6/5/2016
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: 6938b92d8e5b46d908dc5b2155bdfdc89bb1dc8c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a><span data-ttu-id="6400c-103">Útmutató az SQL Data Warehouse PolyBase használatával</span><span class="sxs-lookup"><span data-stu-id="6400c-103">Guide for using PolyBase in SQL Data Warehouse</span></span>
<span data-ttu-id="6400c-104">Ez az útmutató az SQL Data Warehouse PolyBase használatára vonatkozó gyakorlati információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="6400c-104">This guide gives practical information for using PolyBase in SQL Data Warehouse.</span></span>

<span data-ttu-id="6400c-105">Első lépésként tekintse meg a [adatok betöltése a PolyBase] [ Load data with PolyBase] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6400c-105">To get started, see the [Load data with PolyBase][Load data with PolyBase] tutorial.</span></span>

## <a name="rotating-storage-keys"></a><span data-ttu-id="6400c-106">Tárolási kulcsok elforgatása</span><span class="sxs-lookup"><span data-stu-id="6400c-106">Rotating storage keys</span></span>
<span data-ttu-id="6400c-107">Időről időre lesz módosítani szeretné a hozzáférési kulcsot a blob Storage biztonsági okokból.</span><span class="sxs-lookup"><span data-stu-id="6400c-107">From time to time you will want to change the access key to your blob storage for security reasons.</span></span>

<span data-ttu-id="6400c-108">A legtöbb elegáns a feladat végrehajtásához módja kövesse "a kulcsok elforgatása" néven ismert folyamat.</span><span class="sxs-lookup"><span data-stu-id="6400c-108">The most elegant way to perform this task is to follow a process known as "rotating the keys".</span></span> <span data-ttu-id="6400c-109">Talán észrevette, hogy két kulccsal is rendelkezik tárolási a blob storage-fiók esetében.</span><span class="sxs-lookup"><span data-stu-id="6400c-109">You may have noticed that you have two storage keys for your blob storage account.</span></span> <span data-ttu-id="6400c-110">Ez azért, hogy, hogy térjen át</span><span class="sxs-lookup"><span data-stu-id="6400c-110">This is so that you can transition</span></span>

<span data-ttu-id="6400c-111">Az Azure tárfiókkulcsok elforgatása rendkívül egyszerű három lépést folyamat</span><span class="sxs-lookup"><span data-stu-id="6400c-111">Rotating your Azure storage account keys is a simple three step process</span></span>

1. <span data-ttu-id="6400c-112">A másodlagos tárelérési kulcs alapján második adatbázishoz kötődő hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6400c-112">Create second database scoped credential based on the secondary storage access key</span></span>
2. <span data-ttu-id="6400c-113">Ki az új hitelesítőadat-alapú második külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6400c-113">Create second external data source based off this new credential</span></span>
3. <span data-ttu-id="6400c-114">Dobja el, és a külső táblák, mutasson az új külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6400c-114">Drop and create the external table(s) pointing to the new external data source</span></span>

<span data-ttu-id="6400c-115">Ha áttelepítette a külső táblák az új külső adatforráshoz, majd az eltávolítási feladatokat hajthat végre:</span><span class="sxs-lookup"><span data-stu-id="6400c-115">When you have migrated all your external tables to the new external data source then you can perform the clean up tasks:</span></span>

1. <span data-ttu-id="6400c-116">Első külső adatforrásból eldobási</span><span class="sxs-lookup"><span data-stu-id="6400c-116">Drop first external data source</span></span>
2. <span data-ttu-id="6400c-117">Első adatbázis kötődő hitelesítő adatok az elsődleges tárelérési kulcs alapján</span><span class="sxs-lookup"><span data-stu-id="6400c-117">Drop first database scoped credential based on the primary storage access key</span></span>
3. <span data-ttu-id="6400c-118">Jelentkezzen be Azure és az készen áll a következő elsődleges elérési kulcs újragenerálása</span><span class="sxs-lookup"><span data-stu-id="6400c-118">Log into Azure and regenerate the primary access key ready for the next time</span></span>

## <a name="query-azure-blob-storage-data"></a><span data-ttu-id="6400c-119">Az Azure blob storage-adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6400c-119">Query Azure blob storage data</span></span>
<span data-ttu-id="6400c-120">Külső táblák lekérdezéseket egyszerűen használja a táblázat nevét, mintha egy relációs tábla volt.</span><span class="sxs-lookup"><span data-stu-id="6400c-120">Queries against external tables simply use the table name as though it was a relational table.</span></span>

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> <span data-ttu-id="6400c-121">A külső tábla a lekérdezés sikertelen lehet a hibával *"lekérdezés végrehajtása megszakadt – a maximális elutasítás küszöbérték elérésekor külső forrásból történő beolvasás során"*.</span><span class="sxs-lookup"><span data-stu-id="6400c-121">A query on an external table can fail with the error *"Query aborted-- the maximum reject threshold was reached while reading from an external source"*.</span></span> <span data-ttu-id="6400c-122">Ez azt jelzi, hogy a külső adatokat tartalmaz *inkonzisztencia* rögzíti.</span><span class="sxs-lookup"><span data-stu-id="6400c-122">This indicates that your external data contains *dirty* records.</span></span> <span data-ttu-id="6400c-123">Rekord "hibás" tekintendő, ha a tényleges adatok típusok/oszlopok száma nem egyezik az oszlopdefiníciók a külső tábla, vagy ha az adatok nem felelnek meg a megadott külső fájlformátum.</span><span class="sxs-lookup"><span data-stu-id="6400c-123">A data record is considered 'dirty' if the actual data types/number of columns do not match the column definitions of the external table or if the data doesn't conform to the specified external file format.</span></span> <span data-ttu-id="6400c-124">A javítás érdekében győződjön meg arról, hogy a külső tábla és a külső fájlformátum-meghatározások helyességéről, valamint a külső adatokat megfelel e definíciókat.</span><span class="sxs-lookup"><span data-stu-id="6400c-124">To fix this, ensure that your external table and external file format definitions are correct and your external data conforms to these definitions.</span></span> <span data-ttu-id="6400c-125">Abban az esetben, ha egy külső rekordok részét inkonzisztencia, dönthet úgy, hogy ezeket a rekordokat, a lekérdezések utasítsa el az alkalmazást a külső tábla létrehozása DDL.</span><span class="sxs-lookup"><span data-stu-id="6400c-125">In case a subset of external data records are dirty, you can choose to reject these records for your queries by using the reject options in CREATE EXTERNAL TABLE DDL.</span></span>
> 
> 

## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="6400c-126">Adatok betöltése az Azure Blob Storage-ből</span><span class="sxs-lookup"><span data-stu-id="6400c-126">Load data from Azure blob storage</span></span>
<span data-ttu-id="6400c-127">Ebben a példában az Azure blob storage adatokat tölt az SQL Data Warehouse-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="6400c-127">This example loads data from Azure blob storage to SQL Data Warehouse database.</span></span>

<span data-ttu-id="6400c-128">Az adatátviteli idő lekérdezések adattárolás közvetlenül eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="6400c-128">Storing data directly removes the data transfer time for queries.</span></span> <span data-ttu-id="6400c-129">Adattárolás egy oszloptárindexet tartalmazó növeli a lekérdezési teljesítményt, ha a akár 10 x elemzési lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="6400c-129">Storing data with a columnstore index improves query performance for analysis queries by up to 10x.</span></span>

<span data-ttu-id="6400c-130">Ebben a példában a CREATE TABLE AS SELECT utasítást használja az adatok betöltése.</span><span class="sxs-lookup"><span data-stu-id="6400c-130">This example uses the CREATE TABLE AS SELECT statement to load data.</span></span> <span data-ttu-id="6400c-131">Az új táblázat örökli a lekérdezésben szereplő oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="6400c-131">The new table inherits the columns named in the query.</span></span> <span data-ttu-id="6400c-132">Ezek az oszlopok adattípusai az örökli a külső tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="6400c-132">It inherits the data types of those columns from the external table definition.</span></span>

<span data-ttu-id="6400c-133">CREATE TABLE AS SELECT a magas performant Transact-SQL-utasítást az SQL Data Warehouse összes számítási csomópont párhuzamosan az adatok betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="6400c-133">CREATE TABLE AS SELECT is a highly performant Transact-SQL statement that loads the data in parallel to all the compute nodes of your SQL Data Warehouse.</span></span>  <span data-ttu-id="6400c-134">A nagymértékben párhuzamos feldolgozási (MPP) motor az Analytics Platform System eredetileg, és jelenleg az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6400c-134">It was originally developed for  the massively parallel processing (MPP) engine in Analytics Platform System and is now in SQL Data Warehouse.</span></span>

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

<span data-ttu-id="6400c-135">Lásd: [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="6400c-135">See [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span></span>

## <a name="create-statistics-on-newly-loaded-data"></a><span data-ttu-id="6400c-136">Statisztika létrehozása újonnan betöltött adatokról</span><span class="sxs-lookup"><span data-stu-id="6400c-136">Create Statistics on newly loaded data</span></span>
<span data-ttu-id="6400c-137">Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.</span><span class="sxs-lookup"><span data-stu-id="6400c-137">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="6400c-138">A legjobb lekérdezési teljesítmény eléréséhez fontos létrehozni statisztikákat a táblák összes oszlopához az első betöltés után, illetve az adatok minden lényeges módosítását követően.</span><span class="sxs-lookup"><span data-stu-id="6400c-138">In order to get the best performance from your queries, it's important that statistics be created on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="6400c-139">A statisztika részletes ismertetését a Fejlesztés témakörcsoport [Statisztika][Statistics] témakörében találja.</span><span class="sxs-lookup"><span data-stu-id="6400c-139">For a detailed explanation of statistics, see the [Statistics][Statistics] topic in the Develop group of topics.</span></span>  <span data-ttu-id="6400c-140">Alább egy gyors példát létrehozására a táblázatos ebben a példában betöltött van.</span><span class="sxs-lookup"><span data-stu-id="6400c-140">Below is a quick example of how to create statistics on the tabled loaded in this example.</span></span>

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a><span data-ttu-id="6400c-141">Exportálja az adatokat az Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="6400c-141">Export data to Azure blob storage</span></span>
<span data-ttu-id="6400c-142">Ez a szakasz bemutatja, hogyan exportál adatokat az SQL Data Warehouse az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6400c-142">This section shows how to export data from SQL Data Warehouse to Azure blob storage.</span></span> <span data-ttu-id="6400c-143">A példa létrehozása külső TABLE AS SELECT egy magas performant Transact-SQL-utasítást az adatok párhuzamos exportálása a számítási csomópontok egyben.</span><span class="sxs-lookup"><span data-stu-id="6400c-143">This example uses CREATE EXTERNAL TABLE AS SELECT which is a highly performant Transact-SQL statement to export the data in parallel from all the compute nodes.</span></span>

<span data-ttu-id="6400c-144">Az alábbi példa létrehoz egy külső tábla Weblogs2014 oszlopdefiníciók és dbo adatait. Webes naplók tábla.</span><span class="sxs-lookup"><span data-stu-id="6400c-144">The following example creates an external table Weblogs2014 using column definitions and data from dbo.Weblogs table.</span></span> <span data-ttu-id="6400c-145">A külső tábla definíciójának SQL Data Warehouse tárolja, és a SELECT utasítás eredményét exportálják a "/ / log2014/archiválására" az adatforrás által megadott blob tároló könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="6400c-145">The external table definition is stored in SQL Data Warehouse and the results of the SELECT statement are exported to the "/archive/log2014/" directory under the blob container specified by the data source.</span></span> <span data-ttu-id="6400c-146">Az adatok exportálása a megadott szöveg formátumban.</span><span class="sxs-lookup"><span data-stu-id="6400c-146">The data is exported in the specified text file format.</span></span>

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a><span data-ttu-id="6400c-147">Különítse el a felhasználók betöltésekor</span><span class="sxs-lookup"><span data-stu-id="6400c-147">Isolate Loading Users</span></span>
<span data-ttu-id="6400c-148">Gyakran szükség van egy SQL DW adatok betölthetnek több felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="6400c-148">There is often a need to have multiple users that can load data into a SQL DW.</span></span> <span data-ttu-id="6400c-149">Mivel a [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] VEZÉRLÉSI engedélyekkel kell rendelkeznie az adatbázis kat, az elérés több felhasználóval rendelkező összes keresztül.</span><span class="sxs-lookup"><span data-stu-id="6400c-149">Because the [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] requires CONTROL permissions of the database, you will end up with multiple users with control access over all schemas.</span></span> <span data-ttu-id="6400c-150">Ez korlátozza, a MEGTAGADÁSI vezérlő utasítás is használhat.</span><span class="sxs-lookup"><span data-stu-id="6400c-150">To limit this, you can use the DENY CONTROL statement.</span></span>

<span data-ttu-id="6400c-151">Példa: Fontolja meg adatbázis sémák schema_A az osztály A, és a részleg B segítségével adatbázis felhasználók user_A schema_B és user_B felhasználóknak PolyBase betöltése az osztály A és B, illetve a.</span><span class="sxs-lookup"><span data-stu-id="6400c-151">Example: Consider database schemas schema_A for dept A, and schema_B for dept B Let database users user_A and user_B be users for PolyBase loading in dept A and B, respectively.</span></span> <span data-ttu-id="6400c-152">Mindkettő rendelkezik vezérlő adatbázis-engedélyek.</span><span class="sxs-lookup"><span data-stu-id="6400c-152">They both have been granted CONTROL database permissions.</span></span>
<span data-ttu-id="6400c-153">Séma A és B most zárolás creators le, hogy a sémáikat MEGTAGADÁS használatával:</span><span class="sxs-lookup"><span data-stu-id="6400c-153">The creators of schema A and B now lock down their schemas using DENY:</span></span>

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   
 <span data-ttu-id="6400c-154">Ennek user_A és user_B most zárolja a más osztály séma.</span><span class="sxs-lookup"><span data-stu-id="6400c-154">With this, user_A and user_B should now be locked out from the other dept’s schema.</span></span>
 


## <a name="next-steps"></a><span data-ttu-id="6400c-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6400c-155">Next steps</span></span>
<span data-ttu-id="6400c-156">Adatok áthelyezése az SQL Data Warehouse kapcsolatos további tudnivalókért tekintse meg a [adatok áttelepítése – áttekintés][data migration overview].</span><span class="sxs-lookup"><span data-stu-id="6400c-156">To learn more about moving data to SQL Data Warehouse, see the [data migration overview][data migration overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
