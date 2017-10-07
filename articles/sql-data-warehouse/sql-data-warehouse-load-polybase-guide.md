---
title: az SQL Data Warehouse PolyBase az aaaGuide |} Microsoft Docs
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
ms.openlocfilehash: b05e4c5d528f2fe1c60d6855b5333065f0c908ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a><span data-ttu-id="60794-103">Útmutató az SQL Data Warehouse PolyBase használatával</span><span class="sxs-lookup"><span data-stu-id="60794-103">Guide for using PolyBase in SQL Data Warehouse</span></span>
<span data-ttu-id="60794-104">Ez az útmutató az SQL Data Warehouse PolyBase használatára vonatkozó gyakorlati információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="60794-104">This guide gives practical information for using PolyBase in SQL Data Warehouse.</span></span>

<span data-ttu-id="60794-105">tooget indult el, lásd: hello [adatok betöltése a PolyBase] [ Load data with PolyBase] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="60794-105">tooget started, see hello [Load data with PolyBase][Load data with PolyBase] tutorial.</span></span>

## <a name="rotating-storage-keys"></a><span data-ttu-id="60794-106">Tárolási kulcsok elforgatása</span><span class="sxs-lookup"><span data-stu-id="60794-106">Rotating storage keys</span></span>
<span data-ttu-id="60794-107">Az idő tootime érdemes biztonsági okokból toochange hello hozzáférési kulcs tooyour blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="60794-107">From time tootime you will want toochange hello access key tooyour blob storage for security reasons.</span></span>

<span data-ttu-id="60794-108">Ez a feladat a "hello kulcsok elforgatása" néven ismert folyamat toofollow legtöbb elegáns módon tooperform hello.</span><span class="sxs-lookup"><span data-stu-id="60794-108">hello most elegant way tooperform this task is toofollow a process known as "rotating hello keys".</span></span> <span data-ttu-id="60794-109">Talán észrevette, hogy két kulccsal is rendelkezik tárolási a blob storage-fiók esetében.</span><span class="sxs-lookup"><span data-stu-id="60794-109">You may have noticed that you have two storage keys for your blob storage account.</span></span> <span data-ttu-id="60794-110">Ez azért, hogy, hogy térjen át</span><span class="sxs-lookup"><span data-stu-id="60794-110">This is so that you can transition</span></span>

<span data-ttu-id="60794-111">Az Azure tárfiókkulcsok elforgatása rendkívül egyszerű három lépést folyamat</span><span class="sxs-lookup"><span data-stu-id="60794-111">Rotating your Azure storage account keys is a simple three step process</span></span>

1. <span data-ttu-id="60794-112">Második adatbázishoz kötődő hitelesítő adatok alapján hello másodlagos tárelérési kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="60794-112">Create second database scoped credential based on hello secondary storage access key</span></span>
2. <span data-ttu-id="60794-113">Ki az új hitelesítőadat-alapú második külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="60794-113">Create second external data source based off this new credential</span></span>
3. <span data-ttu-id="60794-114">Dobja el és létrehozni a hello külső toohello új külső adatforrás mutat</span><span class="sxs-lookup"><span data-stu-id="60794-114">Drop and create hello external table(s) pointing toohello new external data source</span></span>

<span data-ttu-id="60794-115">Ha áttelepítette az összes külső táblák toohello új külső adatforrás majd végezhet hello feladatok törlése:</span><span class="sxs-lookup"><span data-stu-id="60794-115">When you have migrated all your external tables toohello new external data source then you can perform hello clean up tasks:</span></span>

1. <span data-ttu-id="60794-116">Első külső adatforrásból eldobási</span><span class="sxs-lookup"><span data-stu-id="60794-116">Drop first external data source</span></span>
2. <span data-ttu-id="60794-117">Első adatbázis kötődő hitelesítő adatok hello elsődleges tárelérési kulcs alapján</span><span class="sxs-lookup"><span data-stu-id="60794-117">Drop first database scoped credential based on hello primary storage access key</span></span>
3. <span data-ttu-id="60794-118">Jelentkezzen be Azure, majd újra létrehozza a készen áll a hello hello elsődleges elérési kulcsot következő indításakor</span><span class="sxs-lookup"><span data-stu-id="60794-118">Log into Azure and regenerate hello primary access key ready for hello next time</span></span>

## <a name="query-azure-blob-storage-data"></a><span data-ttu-id="60794-119">Az Azure blob storage-adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="60794-119">Query Azure blob storage data</span></span>
<span data-ttu-id="60794-120">Külső táblák lekérdezéseket egyszerűen használja hello táblanév, mintha egy relációs tábla volt.</span><span class="sxs-lookup"><span data-stu-id="60794-120">Queries against external tables simply use hello table name as though it was a relational table.</span></span>

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> <span data-ttu-id="60794-121">A külső tábla is sikertelenek hello hiba *"lekérdezés végrehajtása megszakadt – hello maximális elutasítás küszöbérték elérte a külső forrásból történő beolvasás során"*.</span><span class="sxs-lookup"><span data-stu-id="60794-121">A query on an external table can fail with hello error *"Query aborted-- hello maximum reject threshold was reached while reading from an external source"*.</span></span> <span data-ttu-id="60794-122">Ez azt jelzi, hogy a külső adatokat tartalmaz *inkonzisztencia* rögzíti.</span><span class="sxs-lookup"><span data-stu-id="60794-122">This indicates that your external data contains *dirty* records.</span></span> <span data-ttu-id="60794-123">Rekord "hibás" tekintendő, ha hello tényleges adatokat típusok/oszlopok száma nem egyezik a hello oszlopdefiníciók hello külső tábla, vagy ha hello adatai nem felelnek meg a megadott külső fájlformátum toohello.</span><span class="sxs-lookup"><span data-stu-id="60794-123">A data record is considered 'dirty' if hello actual data types/number of columns do not match hello column definitions of hello external table or if hello data doesn't conform toohello specified external file format.</span></span> <span data-ttu-id="60794-124">toofix, győződjön meg arról, hogy a külső tábla és a külső fájlformátum-meghatározások helyességéről, valamint a külső adatokat megfelel toothese definíciókat.</span><span class="sxs-lookup"><span data-stu-id="60794-124">toofix this, ensure that your external table and external file format definitions are correct and your external data conforms toothese definitions.</span></span> <span data-ttu-id="60794-125">Abban az esetben, ha egy külső rekordok részét inkonzisztencia, kiválaszthatja tooreject ezeket a rekordokat a lekérdezések a külső tábla létrehozása DDL hello elutasítás beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="60794-125">In case a subset of external data records are dirty, you can choose tooreject these records for your queries by using hello reject options in CREATE EXTERNAL TABLE DDL.</span></span>
> 
> 

## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="60794-126">Adatok betöltése az Azure Blob Storage-ből</span><span class="sxs-lookup"><span data-stu-id="60794-126">Load data from Azure blob storage</span></span>
<span data-ttu-id="60794-127">Ez a példa adatokat tölt az Azure blob storage tooSQL az operatív adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="60794-127">This example loads data from Azure blob storage tooSQL Data Warehouse database.</span></span>

<span data-ttu-id="60794-128">Adattárolás közvetlenül eltávolítja hello adatátviteli idő lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="60794-128">Storing data directly removes hello data transfer time for queries.</span></span> <span data-ttu-id="60794-129">Adattárolás egy oszloptárindexet tartalmazó növeli a lekérdezési teljesítmény elemzési lekérdezések által too10x fel.</span><span class="sxs-lookup"><span data-stu-id="60794-129">Storing data with a columnstore index improves query performance for analysis queries by up too10x.</span></span>

<span data-ttu-id="60794-130">A példa hello CREATE TABLE AS SELECT utasítás tooload adatokat.</span><span class="sxs-lookup"><span data-stu-id="60794-130">This example uses hello CREATE TABLE AS SELECT statement tooload data.</span></span> <span data-ttu-id="60794-131">Új tábla hello örökli hello hello lekérdezésben szereplő oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="60794-131">hello new table inherits hello columns named in hello query.</span></span> <span data-ttu-id="60794-132">Ezek az oszlopok adattípusai hello örököl hello külső tábla definíciójában.</span><span class="sxs-lookup"><span data-stu-id="60794-132">It inherits hello data types of those columns from hello external table definition.</span></span>

<span data-ttu-id="60794-133">CREATE TABLE AS SELECT a magas performant Transact-SQL utasítást, amely a párhuzamos tooall hello hello adatokat tölt számítási csomópontok az SQL Data warehouse.</span><span class="sxs-lookup"><span data-stu-id="60794-133">CREATE TABLE AS SELECT is a highly performant Transact-SQL statement that loads hello data in parallel tooall hello compute nodes of your SQL Data Warehouse.</span></span>  <span data-ttu-id="60794-134">A hello nagymértékben párhuzamos feldolgozási (MPP) motor az Analytics Platform System eredetileg, és jelenleg az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="60794-134">It was originally developed for  hello massively parallel processing (MPP) engine in Analytics Platform System and is now in SQL Data Warehouse.</span></span>

```sql
-- Load data from Azure blob storage tooSQL Data Warehouse

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

<span data-ttu-id="60794-135">Lásd: [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span><span class="sxs-lookup"><span data-stu-id="60794-135">See [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].</span></span>

## <a name="create-statistics-on-newly-loaded-data"></a><span data-ttu-id="60794-136">Statisztika létrehozása újonnan betöltött adatokról</span><span class="sxs-lookup"><span data-stu-id="60794-136">Create Statistics on newly loaded data</span></span>
<span data-ttu-id="60794-137">Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.</span><span class="sxs-lookup"><span data-stu-id="60794-137">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="60794-138">A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő.</span><span class="sxs-lookup"><span data-stu-id="60794-138">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="60794-139">A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében.</span><span class="sxs-lookup"><span data-stu-id="60794-139">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span>  <span data-ttu-id="60794-140">Az alábbiakban látható egy gyors példa hogyan előterjesztett hello toocreate statisztikák ebben a példában betöltött.</span><span class="sxs-lookup"><span data-stu-id="60794-140">Below is a quick example of how toocreate statistics on hello tabled loaded in this example.</span></span>

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-tooazure-blob-storage"></a><span data-ttu-id="60794-141">Adatok tooAzure blob-tároló exportálása</span><span class="sxs-lookup"><span data-stu-id="60794-141">Export data tooAzure blob storage</span></span>
<span data-ttu-id="60794-142">Ez a szakasz bemutatja, hogyan tooexport adatait az SQL Data Warehouse tooAzure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="60794-142">This section shows how tooexport data from SQL Data Warehouse tooAzure blob storage.</span></span> <span data-ttu-id="60794-143">A példa létrehozása külső tábla AS válassza ki azt a magas performant Transact-SQL utasítás tooexport hello adatokat az összes hello számítási csomópont párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="60794-143">This example uses CREATE EXTERNAL TABLE AS SELECT which is a highly performant Transact-SQL statement tooexport hello data in parallel from all hello compute nodes.</span></span>

<span data-ttu-id="60794-144">hello alábbi példa létrehoz egy külső tábla Weblogs2014 oszlopdefiníciók és dbo adatait. Webes naplók tábla.</span><span class="sxs-lookup"><span data-stu-id="60794-144">hello following example creates an external table Weblogs2014 using column definitions and data from dbo.Weblogs table.</span></span> <span data-ttu-id="60794-145">hello külső tábla definíciójának SQL Data Warehouse tárolja, és hello hello SELECT utasítás eredményei exportált toohello hello adatforrás által meghatározott hello blob tároló "/ / log2014/archiválására" könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="60794-145">hello external table definition is stored in SQL Data Warehouse and hello results of hello SELECT statement are exported toohello "/archive/log2014/" directory under hello blob container specified by hello data source.</span></span> <span data-ttu-id="60794-146">hello adatok exportálása hello megadott szöveg formátumban.</span><span class="sxs-lookup"><span data-stu-id="60794-146">hello data is exported in hello specified text file format.</span></span>

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
## <a name="isolate-loading-users"></a><span data-ttu-id="60794-147">Különítse el a felhasználók betöltésekor</span><span class="sxs-lookup"><span data-stu-id="60794-147">Isolate Loading Users</span></span>
<span data-ttu-id="60794-148">Nincs gyakran egy szükséges toohave adatok betölthetnek egy SQL DW több felhasználó.</span><span class="sxs-lookup"><span data-stu-id="60794-148">There is often a need toohave multiple users that can load data into a SQL DW.</span></span> <span data-ttu-id="60794-149">Mivel hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] VEZÉRLÉSI engedélyekkel kell rendelkeznie a hello adatbázis kat, az elérés több felhasználóval rendelkező összes keresztül.</span><span class="sxs-lookup"><span data-stu-id="60794-149">Because hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] requires CONTROL permissions of hello database, you will end up with multiple users with control access over all schemas.</span></span> <span data-ttu-id="60794-150">toolimit, hello ellenőrzési MEGTAGADÁSI utasítással.</span><span class="sxs-lookup"><span data-stu-id="60794-150">toolimit this, you can use hello DENY CONTROL statement.</span></span>

<span data-ttu-id="60794-151">Példa: Fontolja meg adatbázis sémák schema_A az osztály A, és a részleg B segítségével adatbázis felhasználók user_A schema_B és user_B felhasználóknak PolyBase betöltése az osztály A és B, illetve a.</span><span class="sxs-lookup"><span data-stu-id="60794-151">Example: Consider database schemas schema_A for dept A, and schema_B for dept B Let database users user_A and user_B be users for PolyBase loading in dept A and B, respectively.</span></span> <span data-ttu-id="60794-152">Mindkettő rendelkezik vezérlő adatbázis-engedélyek.</span><span class="sxs-lookup"><span data-stu-id="60794-152">They both have been granted CONTROL database permissions.</span></span>
<span data-ttu-id="60794-153">hello creators séma A és B most zárolását, hogy a sémáikat MEGTAGADÁS használatával:</span><span class="sxs-lookup"><span data-stu-id="60794-153">hello creators of schema A and B now lock down their schemas using DENY:</span></span>

```sql
   DENY CONTROL ON SCHEMA :: schema_A toouser_B;
   DENY CONTROL ON SCHEMA :: schema_B toouser_A;
```   
 <span data-ttu-id="60794-154">Ennek, user_A és user_B most zárolja a hello más osztály sémáját.</span><span class="sxs-lookup"><span data-stu-id="60794-154">With this, user_A and user_B should now be locked out from hello other dept’s schema.</span></span>
 


## <a name="next-steps"></a><span data-ttu-id="60794-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60794-155">Next steps</span></span>
<span data-ttu-id="60794-156">toolearn áthelyezése adatok tooSQL Data Warehouse kapcsolatos további információkért lásd: hello [adatok áttelepítése – áttekintés][data migration overview].</span><span class="sxs-lookup"><span data-stu-id="60794-156">toolearn more about moving data tooSQL Data Warehouse, see hello [data migration overview][data migration overview].</span></span>

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
