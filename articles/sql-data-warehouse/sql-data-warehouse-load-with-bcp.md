---
title: aaaUse bcp tooload adatokat az SQL Data Warehouse |} Microsoft Docs
description: "Ismerje meg a bcp-t, és hogyan toouse az adatraktározási forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 09a2980585097644924c71899f9e74fb32fbc26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-bcp"></a><span data-ttu-id="ad801-103">Adatok betöltése a bcp használatával</span><span class="sxs-lookup"><span data-stu-id="ad801-103">Load data with bcp</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad801-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="ad801-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="ad801-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="ad801-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="ad801-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="ad801-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="ad801-107">BCP</span><span class="sxs-lookup"><span data-stu-id="ad801-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="ad801-108">**[BCP] [ bcp]**  parancssori tömeges betöltési segédprogram, amely lehetővé teszi a toocopy adatok SQL Server, az adatfájlok és az SQL Data Warehouse között.</span><span class="sxs-lookup"><span data-stu-id="ad801-108">**[bcp][bcp]** is a command-line bulk load utility that allows you toocopy data between SQL Server, data files, and SQL Data Warehouse.</span></span> <span data-ttu-id="ad801-109">Használja a bcp tooimport nagyszámú sorok SQL Data Warehouse tábláiba, vagy az SQL Server tábláiból adatfájlokba tooexport adatokat.</span><span class="sxs-lookup"><span data-stu-id="ad801-109">Use bcp tooimport large numbers of rows into SQL Data Warehouse tables or tooexport data from SQL Server tables into data files.</span></span> <span data-ttu-id="ad801-110">Amikor hello queryout kapcsolóval használja, kivéve a bcp nem ismeri a Transact-SQL van szükség.</span><span class="sxs-lookup"><span data-stu-id="ad801-110">Except when used with hello queryout option, bcp requires no knowledge of Transact-SQL.</span></span>

<span data-ttu-id="ad801-111">BCP-t egy gyorsan és egyszerűen elvégezhető toomove kisebb adatkészletek esetében bejövő és kimenő egy SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ad801-111">bcp is a quick and easy way toomove smaller data sets into and out of a SQL Data Warehouse database.</span></span> <span data-ttu-id="ad801-112">hello tooload/kinyerésre ajánlott adatok pontos mennyisége függvényében kapcsolat toohello Azure-adatközponthoz csatlakozó hálózati.</span><span class="sxs-lookup"><span data-stu-id="ad801-112">hello exact amount of data that is recommended tooload/extract via bcp will depend on you network connection toohello Azure data center.</span></span>  <span data-ttu-id="ad801-113">Általában a dimenziótáblák könnyen betölthetők és kinyerhetők, ennek ellenére a bcp nem ajánlott nagy adatmennyiségek betöltéséhez vagy kinyeréséhez.</span><span class="sxs-lookup"><span data-stu-id="ad801-113">Generally, dimension tables can be loaded and extracted readily with bcp, however, bcp is not recommended for loading or extracting large volumes of data.</span></span>  <span data-ttu-id="ad801-114">Polybase az ajánlott eszköz betöltéséhez és kinyeréséhez a nagy mennyiségű adat, mint az SQL Data Warehouse hello nagymértékben párhuzamos feldolgozási architektúráját, ami jobb munkát hello.</span><span class="sxs-lookup"><span data-stu-id="ad801-114">Polybase is hello recommended tool for loading and extracting large volumes of data as it does a better job leveraging hello massively parallel processing architecture of SQL Data Warehouse.</span></span>

<span data-ttu-id="ad801-115">A bcp-vel a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="ad801-115">With bcp you can:</span></span>

* <span data-ttu-id="ad801-116">Egy egyszerű parancssori segédprogrammal tooload adatokat használja az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ad801-116">Use a simple command-line utility tooload data into SQL Data Warehouse.</span></span>
* <span data-ttu-id="ad801-117">Az SQL Data Warehouse egy egyszerű parancssori segédprogrammal tooextract adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="ad801-117">Use a simple command-line utility tooextract data from SQL Data Warehouse.</span></span>

<span data-ttu-id="ad801-118">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="ad801-118">This tutorial will show you how to:</span></span>

* <span data-ttu-id="ad801-119">Adatok importálása egy táblába a bcp hello parancsban</span><span class="sxs-lookup"><span data-stu-id="ad801-119">Import data into a table using hello bcp in command</span></span>
* <span data-ttu-id="ad801-120">Adatok exportálása egy tábla előrejelzése hello bcp out paranccsal</span><span class="sxs-lookup"><span data-stu-id="ad801-120">Export data from a table uisng hello bcp out command</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ad801-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ad801-121">Prerequisites</span></span>
<span data-ttu-id="ad801-122">az oktatóanyag teljesítéséhez toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="ad801-122">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="ad801-123">Egy SQL Data Warehouse-adatbázis</span><span class="sxs-lookup"><span data-stu-id="ad801-123">A SQL Data Warehouse database</span></span>
* <span data-ttu-id="ad801-124">hello telepített bcp parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="ad801-124">hello bcp command line utility installed</span></span>
* <span data-ttu-id="ad801-125">hello telepített SQLCMD parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="ad801-125">hello SQLCMD command line utility installed</span></span>

> [!NOTE]
> <span data-ttu-id="ad801-126">Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="ad801-126">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a><span data-ttu-id="ad801-127">Adatok importálása az SQL Data Warehouse-ba</span><span class="sxs-lookup"><span data-stu-id="ad801-127">Import data into SQL Data Warehouse</span></span>
<span data-ttu-id="ad801-128">Ebben az oktatóanyagban létrehoz egy táblát az Azure SQL Data warehouse-ba, és adatimportálás hello táblába.</span><span class="sxs-lookup"><span data-stu-id="ad801-128">In this tutorial, you will create a table in Azure SQL Data Warehouse and import data into hello table.</span></span>

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a><span data-ttu-id="ad801-129">1. lépés: Tábla létrehozása az Azure SQL Data Warehouse-ban</span><span class="sxs-lookup"><span data-stu-id="ad801-129">Step 1: Create a table in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="ad801-130">A parancssorból használja az sqlcmd toorun hello lekérdezés toocreate egy táblát a példányon a következő:</span><span class="sxs-lookup"><span data-stu-id="ad801-130">From a command prompt, use sqlcmd toorun hello following query toocreate a table on your instance:</span></span>

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
> <span data-ttu-id="ad801-131">Lásd: [tábla áttekintése] [ Table Overview] vagy [CREATE TABLE szintaxis] [ CREATE TABLE syntax] a táblázatok létrehozásáról az SQL Data Warehouse és hello további információ  hello WITH záradékkal használható lehetőségekről.</span><span class="sxs-lookup"><span data-stu-id="ad801-131">See [Table Overview][Table Overview] or [CREATE TABLE syntax][CREATE TABLE syntax] for more information about creating a table on SQL Data Warehouse and hello  options available in hello WITH clause.</span></span>
> 
> 

### <a name="step-2-create-a-source-data-file"></a><span data-ttu-id="ad801-132">2. lépés: Forrásadatfájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad801-132">Step 2: Create a source data file</span></span>
<span data-ttu-id="ad801-133">Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új szöveges fájlba, és mentse a fájlt tooyour helyi ideiglenes könyvtárba C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="ad801-133">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span>

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
> <span data-ttu-id="ad801-134">Fontos tooremember, hogy a bcp.exe nem támogatja a hello UTF-8 kódolása.</span><span class="sxs-lookup"><span data-stu-id="ad801-134">It is important tooremember that bcp.exe does not support hello UTF-8 file encoding.</span></span> <span data-ttu-id="ad801-135">A bcp.exe használatakor használjon ASCII fájlokat vagy UTF-16 kódolást.</span><span class="sxs-lookup"><span data-stu-id="ad801-135">Please use ASCII files or UTF-16 encoded files when using bcp.exe.</span></span>
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a><span data-ttu-id="ad801-136">3. lépés: Csatlakozás és hello adatok importálása</span><span class="sxs-lookup"><span data-stu-id="ad801-136">Step 3: Connect and import hello data</span></span>
<span data-ttu-id="ad801-137">A bcp csatlakozhat, és használja a következő parancs tagjára hello értékeket szükség szerint hello hello adatok importálása:</span><span class="sxs-lookup"><span data-stu-id="ad801-137">Using bcp, you can connect and import hello data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

<span data-ttu-id="ad801-138">Hello adatok lett betöltve, a következő lekérdezést az sqlcmd segítségével hello futtatásával ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="ad801-138">You can verify hello data was loaded by running hello following query using sqlcmd:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="ad801-139">Ez a következő eredmények hello kell visszaadnia:</span><span class="sxs-lookup"><span data-stu-id="ad801-139">This should return hello following results:</span></span>

| <span data-ttu-id="ad801-140">DateId</span><span class="sxs-lookup"><span data-stu-id="ad801-140">DateId</span></span> | <span data-ttu-id="ad801-141">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="ad801-141">CalendarQuarter</span></span> | <span data-ttu-id="ad801-142">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="ad801-142">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad801-143">20150101</span><span class="sxs-lookup"><span data-stu-id="ad801-143">20150101</span></span> |<span data-ttu-id="ad801-144">1</span><span class="sxs-lookup"><span data-stu-id="ad801-144">1</span></span> |<span data-ttu-id="ad801-145">3</span><span class="sxs-lookup"><span data-stu-id="ad801-145">3</span></span> |
| <span data-ttu-id="ad801-146">20150201</span><span class="sxs-lookup"><span data-stu-id="ad801-146">20150201</span></span> |<span data-ttu-id="ad801-147">1</span><span class="sxs-lookup"><span data-stu-id="ad801-147">1</span></span> |<span data-ttu-id="ad801-148">3</span><span class="sxs-lookup"><span data-stu-id="ad801-148">3</span></span> |
| <span data-ttu-id="ad801-149">20150301</span><span class="sxs-lookup"><span data-stu-id="ad801-149">20150301</span></span> |<span data-ttu-id="ad801-150">1</span><span class="sxs-lookup"><span data-stu-id="ad801-150">1</span></span> |<span data-ttu-id="ad801-151">3</span><span class="sxs-lookup"><span data-stu-id="ad801-151">3</span></span> |
| <span data-ttu-id="ad801-152">20150401</span><span class="sxs-lookup"><span data-stu-id="ad801-152">20150401</span></span> |<span data-ttu-id="ad801-153">2</span><span class="sxs-lookup"><span data-stu-id="ad801-153">2</span></span> |<span data-ttu-id="ad801-154">4</span><span class="sxs-lookup"><span data-stu-id="ad801-154">4</span></span> |
| <span data-ttu-id="ad801-155">20150501</span><span class="sxs-lookup"><span data-stu-id="ad801-155">20150501</span></span> |<span data-ttu-id="ad801-156">2</span><span class="sxs-lookup"><span data-stu-id="ad801-156">2</span></span> |<span data-ttu-id="ad801-157">4</span><span class="sxs-lookup"><span data-stu-id="ad801-157">4</span></span> |
| <span data-ttu-id="ad801-158">20150601</span><span class="sxs-lookup"><span data-stu-id="ad801-158">20150601</span></span> |<span data-ttu-id="ad801-159">2</span><span class="sxs-lookup"><span data-stu-id="ad801-159">2</span></span> |<span data-ttu-id="ad801-160">4</span><span class="sxs-lookup"><span data-stu-id="ad801-160">4</span></span> |
| <span data-ttu-id="ad801-161">20150701</span><span class="sxs-lookup"><span data-stu-id="ad801-161">20150701</span></span> |<span data-ttu-id="ad801-162">3</span><span class="sxs-lookup"><span data-stu-id="ad801-162">3</span></span> |<span data-ttu-id="ad801-163">1</span><span class="sxs-lookup"><span data-stu-id="ad801-163">1</span></span> |
| <span data-ttu-id="ad801-164">20150801</span><span class="sxs-lookup"><span data-stu-id="ad801-164">20150801</span></span> |<span data-ttu-id="ad801-165">3</span><span class="sxs-lookup"><span data-stu-id="ad801-165">3</span></span> |<span data-ttu-id="ad801-166">1</span><span class="sxs-lookup"><span data-stu-id="ad801-166">1</span></span> |
| <span data-ttu-id="ad801-167">20150801</span><span class="sxs-lookup"><span data-stu-id="ad801-167">20150801</span></span> |<span data-ttu-id="ad801-168">3</span><span class="sxs-lookup"><span data-stu-id="ad801-168">3</span></span> |<span data-ttu-id="ad801-169">1</span><span class="sxs-lookup"><span data-stu-id="ad801-169">1</span></span> |
| <span data-ttu-id="ad801-170">20151001</span><span class="sxs-lookup"><span data-stu-id="ad801-170">20151001</span></span> |<span data-ttu-id="ad801-171">4</span><span class="sxs-lookup"><span data-stu-id="ad801-171">4</span></span> |<span data-ttu-id="ad801-172">2</span><span class="sxs-lookup"><span data-stu-id="ad801-172">2</span></span> |
| <span data-ttu-id="ad801-173">20151101</span><span class="sxs-lookup"><span data-stu-id="ad801-173">20151101</span></span> |<span data-ttu-id="ad801-174">4</span><span class="sxs-lookup"><span data-stu-id="ad801-174">4</span></span> |<span data-ttu-id="ad801-175">2</span><span class="sxs-lookup"><span data-stu-id="ad801-175">2</span></span> |
| <span data-ttu-id="ad801-176">20151201</span><span class="sxs-lookup"><span data-stu-id="ad801-176">20151201</span></span> |<span data-ttu-id="ad801-177">4</span><span class="sxs-lookup"><span data-stu-id="ad801-177">4</span></span> |<span data-ttu-id="ad801-178">2</span><span class="sxs-lookup"><span data-stu-id="ad801-178">2</span></span> |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a><span data-ttu-id="ad801-179">4. lépés: Statisztikák létrehozása az újonnan betöltött adatokról</span><span class="sxs-lookup"><span data-stu-id="ad801-179">Step 4: Create Statistics on your newly loaded data</span></span>
<span data-ttu-id="ad801-180">Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.</span><span class="sxs-lookup"><span data-stu-id="ad801-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span> <span data-ttu-id="ad801-181">A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő.</span><span class="sxs-lookup"><span data-stu-id="ad801-181">In order tooget hello best performance from your queries, it's important that statistics be created on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span> <span data-ttu-id="ad801-182">A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében.</span><span class="sxs-lookup"><span data-stu-id="ad801-182">For a detailed explanation of statistics, see hello [Statistics][Statistics] topic in hello Develop group of topics.</span></span> <span data-ttu-id="ad801-183">Az alábbiakban látható egy gyors példa hogyan előterjesztett hello toocreate statisztikák ebben a példában betöltött</span><span class="sxs-lookup"><span data-stu-id="ad801-183">Below is a quick example of how toocreate statistics on hello tabled loaded in this example</span></span>

<span data-ttu-id="ad801-184">Hajtsa végre az alábbi CREATE STATISTICS utasításokat egy sqlcmd parancssorból hello:</span><span class="sxs-lookup"><span data-stu-id="ad801-184">Execute hello following CREATE STATISTICS statements from a sqlcmd prompt:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a><span data-ttu-id="ad801-185">Adatok exportálása az SQL Data Warehouse-ból</span><span class="sxs-lookup"><span data-stu-id="ad801-185">Export data from SQL Data Warehouse</span></span>
<span data-ttu-id="ad801-186">Ebben az oktatóanyagban létrehoz egy adatfájlt egy SQL Data Warehouse-táblából.</span><span class="sxs-lookup"><span data-stu-id="ad801-186">In this tutorial, you will create a data file from a table in SQL Data Warehouse.</span></span> <span data-ttu-id="ad801-187">A fenti tooa új, dimdate2_export.txt adatfájlba létrehozott hello adatokat fog exportálása.</span><span class="sxs-lookup"><span data-stu-id="ad801-187">We will export hello data we created above tooa new data file called DimDate2_export.txt.</span></span>

### <a name="step-1-export-hello-data"></a><span data-ttu-id="ad801-188">1. lépés: Hello adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="ad801-188">Step 1: Export hello data</span></span>
<span data-ttu-id="ad801-189">Hello bcp segédprogram használatával csatlakozhat és exportálhatja az adatokat a következő parancs tagjára hello értékeket szükség szerint hello használata:</span><span class="sxs-lookup"><span data-stu-id="ad801-189">Using hello bcp utility, you can connect and export data using hello following command replacing hello values as appropriate:</span></span>

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
<span data-ttu-id="ad801-190">Hello adatok helyes exportálását hello új fájl megnyitásával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="ad801-190">You can verify hello data was exported correctly by opening hello new file.</span></span> <span data-ttu-id="ad801-191">hello fájl hello adatait meg kell felelnie az alábbi hello szöveg:</span><span class="sxs-lookup"><span data-stu-id="ad801-191">hello data in hello file should match hello text below:</span></span>

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
> <span data-ttu-id="ad801-192">Miatt elosztott rendszerek jellemzői toohello hello adatok sorrendje nem lehet azonos az SQL Data Warehouse az adatbázisok közötti hello.</span><span class="sxs-lookup"><span data-stu-id="ad801-192">Due toohello nature of distributed systems, hello data order may not be hello same across SQL Data Warehouse databases.</span></span> <span data-ttu-id="ad801-193">Másik lehetőség is toouse hello **queryout** függvény a bcp toowrite lekérdezés kibontása helyett hello egész tábla exportálása.</span><span class="sxs-lookup"><span data-stu-id="ad801-193">Another option is toouse hello **queryout** function of bcp toowrite a query extract rather than export hello entire table.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ad801-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad801-194">Next steps</span></span>
<span data-ttu-id="ad801-195">A betöltés áttekintése: [Adatok betöltése az SQL Data Warehouse-ba][Load data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ad801-195">For an overview of loading, see [Load data into SQL Data Warehouse][Load data into SQL Data Warehouse].</span></span>
<span data-ttu-id="ad801-196">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="ad801-196">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
