---
title: "aaaLoad adatokat az SQL Serverről az Azure SQL Data Warehouse-(ba PolyBase) |} Microsoft Docs"
description: "Az Azure SQL Data Warehouse bcp tooexport adatokat SQL Server tooflat fájlok, az AZCopy tooimport adatok tooAzure blob-tároló és a PolyBase tooingest hello adatokat használ."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 89e2a91bc97642e9fc18545cb802b42d8dc4ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a><span data-ttu-id="1503f-103">Adatok betöltése az SQL Serverről az Azure SQL Data Warehouse-ba (AZCopy)</span><span class="sxs-lookup"><span data-stu-id="1503f-103">Load data from SQL Server into Azure SQL Data Warehouse (AZCopy)</span></span>
<span data-ttu-id="1503f-104">Használja a bcp és az AZCopy parancssori segédprogrammal tooload adatokat az SQL Server tooAzure blob-tárolóból.</span><span class="sxs-lookup"><span data-stu-id="1503f-104">Use bcp and AZCopy command-line utilities tooload data from SQL Server tooAzure blob storage.</span></span> <span data-ttu-id="1503f-105">Az Azure SQL Data Warehouse PolyBase vagy az Azure Data Factory tooload hello adatok használni.</span><span class="sxs-lookup"><span data-stu-id="1503f-105">Then use PolyBase or Azure Data Factory tooload hello data into Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1503f-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1503f-106">Prerequisites</span></span>
<span data-ttu-id="1503f-107">az oktatóanyag teljesítéséhez toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="1503f-107">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="1503f-108">Egy SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="1503f-108">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="1503f-109">hello telepített bcp parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="1503f-109">hello bcp command line utility installed</span></span>
* <span data-ttu-id="1503f-110">hello telepített SQLCMD parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="1503f-110">hello SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="1503f-111">Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="1503f-111">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="1503f-112">Adatok importálása az SQL Data Warehouse-ba</span><span class="sxs-lookup"><span data-stu-id="1503f-112">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="1503f-113">Ebben az oktatóanyagban létrehoz egy táblát az Azure SQL Data warehouse-ba, és adatimportálás hello táblába.</span><span class="sxs-lookup"><span data-stu-id="1503f-113">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into hello table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="1503f-114">1. lépés: Tábla létrehozása az Azure SQL Data Warehouse-ban</span><span class="sxs-lookup"><span data-stu-id="1503f-114">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="1503f-115">A parancssorból használja az sqlcmd toorun hello lekérdezés toocreate egy táblát a példányon a következő:</span><span class="sxs-lookup"><span data-stu-id="1503f-115">From a command prompt, use sqlcmd toorun hello following query toocreate a table on your instance:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

> [!NOTE]
> <span data-ttu-id="1503f-116">Lásd: [tábla áttekintése] [ Table Overview] vagy [CREATE TABLE szintaxis] [ CREATE TABLE syntax] a táblázatok létrehozásáról az SQL Data Warehouse és hello további információ  hello WITH záradékkal használható lehetőségekről.</span><span class="sxs-lookup"><span data-stu-id="1503f-116">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and hello  options available in hello WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="1503f-117">2. lépés: Forrásadatfájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1503f-117">Step 2: Create a source data file</span></span>
<span data-ttu-id="1503f-118">Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új szöveges fájlba, és mentse a fájlt tooyour helyi ideiglenes könyvtárba C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="1503f-118">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span>

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

> [!NOTE]
> <span data-ttu-id="1503f-119">Fontos tooremember, hogy a bcp.exe nem támogatja a hello UTF-8 kódolása.</span><span class="sxs-lookup"><span data-stu-id="1503f-119">It is important tooremember that bcp.exe does not support hello UTF-8 file encoding.</span></span> <span data-ttu-id="1503f-120">A bcp.exe használatakor használjon ASCII fájlokat vagy UTF-16 kódolást.</span><span class="sxs-lookup"><span data-stu-id="1503f-120">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a><span data-ttu-id="1503f-121">3. lépés: Csatlakozás és hello adatok importálása</span><span class="sxs-lookup"><span data-stu-id="1503f-121">Step 3: Connect and import hello data</span></span>
<span data-ttu-id="1503f-122">A bcp csatlakozhat, és használja a következő parancs tagjára hello értékeket szükség szerint hello hello adatok importálása:</span><span class="sxs-lookup"><span data-stu-id="1503f-122">Using bcp, you can connect and import hello data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="1503f-123">Hello adatok lett betöltve, a következő lekérdezést az sqlcmd segítségével hello futtatásával ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="1503f-123">You can verify hello data was loaded by running hello following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="1503f-124">Ez a következő eredmények hello kell visszaadnia:</span><span class="sxs-lookup"><span data-stu-id="1503f-124">This should return hello following results:</span></span>

| <span data-ttu-id="1503f-125">DateId</span><span class="sxs-lookup"><span data-stu-id="1503f-125">DateId</span></span> | <span data-ttu-id="1503f-126">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="1503f-126">CalendarQuarter</span></span> | <span data-ttu-id="1503f-127">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="1503f-127">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1503f-128">20150101</span><span class="sxs-lookup"><span data-stu-id="1503f-128">20150101</span></span> |<span data-ttu-id="1503f-129">1</span><span class="sxs-lookup"><span data-stu-id="1503f-129">1</span></span> |<span data-ttu-id="1503f-130">3</span><span class="sxs-lookup"><span data-stu-id="1503f-130">3</span></span> |
| <span data-ttu-id="1503f-131">20150201</span><span class="sxs-lookup"><span data-stu-id="1503f-131">20150201</span></span> |<span data-ttu-id="1503f-132">1</span><span class="sxs-lookup"><span data-stu-id="1503f-132">1</span></span> |<span data-ttu-id="1503f-133">3</span><span class="sxs-lookup"><span data-stu-id="1503f-133">3</span></span> |
| <span data-ttu-id="1503f-134">20150301</span><span class="sxs-lookup"><span data-stu-id="1503f-134">20150301</span></span> |<span data-ttu-id="1503f-135">1</span><span class="sxs-lookup"><span data-stu-id="1503f-135">1</span></span> |<span data-ttu-id="1503f-136">3</span><span class="sxs-lookup"><span data-stu-id="1503f-136">3</span></span> |
| <span data-ttu-id="1503f-137">20150401</span><span class="sxs-lookup"><span data-stu-id="1503f-137">20150401</span></span> |<span data-ttu-id="1503f-138">2</span><span class="sxs-lookup"><span data-stu-id="1503f-138">2</span></span> |<span data-ttu-id="1503f-139">4</span><span class="sxs-lookup"><span data-stu-id="1503f-139">4</span></span> |
| <span data-ttu-id="1503f-140">20150501</span><span class="sxs-lookup"><span data-stu-id="1503f-140">20150501</span></span> |<span data-ttu-id="1503f-141">2</span><span class="sxs-lookup"><span data-stu-id="1503f-141">2</span></span> |<span data-ttu-id="1503f-142">4</span><span class="sxs-lookup"><span data-stu-id="1503f-142">4</span></span> |
| <span data-ttu-id="1503f-143">20150601</span><span class="sxs-lookup"><span data-stu-id="1503f-143">20150601</span></span> |<span data-ttu-id="1503f-144">2</span><span class="sxs-lookup"><span data-stu-id="1503f-144">2</span></span> |<span data-ttu-id="1503f-145">4</span><span class="sxs-lookup"><span data-stu-id="1503f-145">4</span></span> |
| <span data-ttu-id="1503f-146">20150701</span><span class="sxs-lookup"><span data-stu-id="1503f-146">20150701</span></span> |<span data-ttu-id="1503f-147">3</span><span class="sxs-lookup"><span data-stu-id="1503f-147">3</span></span> |<span data-ttu-id="1503f-148">1</span><span class="sxs-lookup"><span data-stu-id="1503f-148">1</span></span> |
| <span data-ttu-id="1503f-149">20150801</span><span class="sxs-lookup"><span data-stu-id="1503f-149">20150801</span></span> |<span data-ttu-id="1503f-150">3</span><span class="sxs-lookup"><span data-stu-id="1503f-150">3</span></span> |<span data-ttu-id="1503f-151">1</span><span class="sxs-lookup"><span data-stu-id="1503f-151">1</span></span> |
| <span data-ttu-id="1503f-152">20150801</span><span class="sxs-lookup"><span data-stu-id="1503f-152">20150801</span></span> |<span data-ttu-id="1503f-153">3</span><span class="sxs-lookup"><span data-stu-id="1503f-153">3</span></span> |<span data-ttu-id="1503f-154">1</span><span class="sxs-lookup"><span data-stu-id="1503f-154">1</span></span> |
| <span data-ttu-id="1503f-155">20151001</span><span class="sxs-lookup"><span data-stu-id="1503f-155">20151001</span></span> |<span data-ttu-id="1503f-156">4</span><span class="sxs-lookup"><span data-stu-id="1503f-156">4</span></span> |<span data-ttu-id="1503f-157">2</span><span class="sxs-lookup"><span data-stu-id="1503f-157">2</span></span> |
| <span data-ttu-id="1503f-158">20151101</span><span class="sxs-lookup"><span data-stu-id="1503f-158">20151101</span></span> |<span data-ttu-id="1503f-159">4</span><span class="sxs-lookup"><span data-stu-id="1503f-159">4</span></span> |<span data-ttu-id="1503f-160">2</span><span class="sxs-lookup"><span data-stu-id="1503f-160">2</span></span> |
| <span data-ttu-id="1503f-161">20151201</span><span class="sxs-lookup"><span data-stu-id="1503f-161">20151201</span></span> |<span data-ttu-id="1503f-162">4</span><span class="sxs-lookup"><span data-stu-id="1503f-162">4</span></span> |<span data-ttu-id="1503f-163">2</span><span class="sxs-lookup"><span data-stu-id="1503f-163">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="1503f-164">4. lépés: Statisztikák létrehozása az újonnan betöltött adatokról</span><span class="sxs-lookup"><span data-stu-id="1503f-164">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="1503f-165">Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.</span><span class="sxs-lookup"><span data-stu-id="1503f-165">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="1503f-166">A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő.</span><span class="sxs-lookup"><span data-stu-id="1503f-166">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span> <span data-ttu-id="1503f-167">A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében.</span><span class="sxs-lookup"><span data-stu-id="1503f-167">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span> <span data-ttu-id="1503f-168">Az alábbiakban látható egy gyors példa hogyan előterjesztett hello toocreate statisztikák ebben a példában betöltött</span><span class="sxs-lookup"><span data-stu-id="1503f-168">Below is a quick example of how toocreate statistics on hello tabled loaded in this example</span></span>

<span data-ttu-id="1503f-169">Hajtsa végre az alábbi CREATE STATISTICS utasításokat egy sqlcmd parancssorból hello:</span><span class="sxs-lookup"><span data-stu-id="1503f-169">Execute hello following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="1503f-170">Adatok exportálása az SQL Data Warehouse-ból</span><span class="sxs-lookup"><span data-stu-id="1503f-170">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="1503f-171">Ebben az oktatóanyagban létrehoz egy adatfájlt egy SQL Data Warehouse-táblából.</span><span class="sxs-lookup"><span data-stu-id="1503f-171">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="1503f-172">A fenti tooa új, dimdate2_export.txt adatfájlba létrehozott hello adatokat fog exportálása.</span><span class="sxs-lookup"><span data-stu-id="1503f-172">We will export hello data we created above tooa new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-hello-data"></a><span data-ttu-id="1503f-173">1. lépés: Hello adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="1503f-173">Step 1: Export hello data</span></span>
<span data-ttu-id="1503f-174">Hello bcp segédprogram használatával csatlakozhat és exportálhatja az adatokat a következő parancs tagjára hello értékeket szükség szerint hello használata:</span><span class="sxs-lookup"><span data-stu-id="1503f-174">Using hello bcp utility, you can connect and export data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="1503f-175">Hello adatok helyes exportálását hello új fájl megnyitásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="1503f-175">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="1503f-176">hello fájl hello adatait meg kell felelnie az alábbi hello szöveg:</span><span class="sxs-lookup"><span data-stu-id="1503f-176">hello data in hello file should match hello text below:</span></span>

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

> [!NOTE]
> <span data-ttu-id="1503f-177">Miatt elosztott rendszerek jellemzői toohello hello adatok sorrendje nem lehet azonos az SQL Data Warehouse az adatbázisok közötti hello.</span><span class="sxs-lookup"><span data-stu-id="1503f-177">Due toohello nature of distributed systems, hello data order may not be hello same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="1503f-178">Másik lehetőség is toouse hello **queryout** függvény a bcp toowrite lekérdezés kibontása helyett hello egész tábla exportálása.</span><span class="sxs-lookup"><span data-stu-id="1503f-178">Another option is toouse hello **queryout** function of bcp toowrite a query extract rather than export hello entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1503f-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1503f-179">Next steps</span></span>
<span data-ttu-id="1503f-180">A betöltés áttekintése: [Adatok betöltése az SQL Data Warehouse-ba][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="1503f-180">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="1503f-181">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="1503f-181">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
