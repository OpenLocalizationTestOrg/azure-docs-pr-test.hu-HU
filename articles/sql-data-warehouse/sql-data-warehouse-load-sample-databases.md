---
title: az SQL Data Warehouse aaaLoad mintaadatok |} Microsoft Docs
description: "Mintaadatok betöltése az SQL Data Warehouse adatbázisba"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 3459c42f3aae51c27fd35db7874faf99e1e577e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a><span data-ttu-id="444e0-103">Mintaadatok betöltése az SQL Data Warehouse adatbázisba</span><span class="sxs-lookup"><span data-stu-id="444e0-103">Load sample data into SQL Data Warehouse</span></span>
<span data-ttu-id="444e0-104">Kövesse ezeket a egyszerű lépéseket tooload és hello Adventure Works vállalat adatbázis lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="444e0-104">Follow these simple steps tooload and query hello Adventure Works Sample database.</span></span> <span data-ttu-id="444e0-105">Ezek a parancsfájlok először az sqlcmd toorun SQL, amely táblák és nézetek fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="444e0-105">These scripts first use sqlcmd toorun SQL which will create tables and views.</span></span> <span data-ttu-id="444e0-106">Táblák létrehozása után, hello parancsfájlok bcp tooload adatokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="444e0-106">Once tables have been created, hello scripts will use bcp tooload data.</span></span>  <span data-ttu-id="444e0-107">Ha még nem rendelkezik sqlcmd és telepített bcp, az alábbi hivatkozásokat követve túl[bcp telepítése] [ install bcp] és túl[sqlcmd telepítése][install sqlcmd].</span><span class="sxs-lookup"><span data-stu-id="444e0-107">If you don't already have sqlcmd and bcp installed, follow these links too[install bcp][install bcp] and too[install sqlcmd][install sqlcmd].</span></span>

## <a name="load-sample-data"></a><span data-ttu-id="444e0-108">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="444e0-108">Load sample data</span></span>
1. <span data-ttu-id="444e0-109">Töltse le a hello [Adventure Works mintaparancsfájlok az SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="444e0-109">Download hello [Adventure Works Sample Scripts for SQL Data Warehouse][Adventure Works Sample Scripts for SQL Data Warehouse] zip file.</span></span>
2. <span data-ttu-id="444e0-110">Hello fájlt csomagolja ki a helyi számítógépen a letöltött zip tooa könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="444e0-110">Extract hello files from downloaded zip tooa directory on your local machine.</span></span>
3. <span data-ttu-id="444e0-111">Kibontott hello fájl aw_create.bat szerkesztése, és állítsa be a következő változók hello fájl hello tetején találhatók hello.</span><span class="sxs-lookup"><span data-stu-id="444e0-111">Edit hello extracted file aw_create.bat and set hello following variables found at hello top of hello file.</span></span>  <span data-ttu-id="444e0-112">Lehet, hogy tooleave hello "=" és hello paraméter között nem állhat csak szóközökből.</span><span class="sxs-lookup"><span data-stu-id="444e0-112">Be sure tooleave no whitespace between hello "=" and hello parameter.</span></span>  <span data-ttu-id="444e0-113">Az alábbiakban példákat hogyan nézhet ki a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="444e0-113">Below are examples of how your edits might look.</span></span>
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. <span data-ttu-id="444e0-114">A Windows parancssort szerkeszteni hello aw_create.bat futtassa.</span><span class="sxs-lookup"><span data-stu-id="444e0-114">From a Windows cmd prompt, run hello edited aw_create.bat.</span></span>  <span data-ttu-id="444e0-115">Győződjön meg arról, hogy hello könyvtárban, ahová menteni szeretné a aw_create.bat módosított verzióját.</span><span class="sxs-lookup"><span data-stu-id="444e0-115">Be sure you are in hello directory where you saved your edited version of aw_create.bat.</span></span>
   <span data-ttu-id="444e0-116">Ezt a parancsfájlt a rendszer...</span><span class="sxs-lookup"><span data-stu-id="444e0-116">This script will...</span></span>
   
   * <span data-ttu-id="444e0-117">Az Adventure Works táblát és nézetet, az adatbázisban már meglévő DROP</span><span class="sxs-lookup"><span data-stu-id="444e0-117">Drop any Adventure Works tables or views that already exist in your database</span></span>
   * <span data-ttu-id="444e0-118">Hello Adventure Works táblák és nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="444e0-118">Create hello Adventure Works tables and views</span></span>
   * <span data-ttu-id="444e0-119">A bcp Adventure Works táblákra betöltése</span><span class="sxs-lookup"><span data-stu-id="444e0-119">Load each Adventure Works table using bcp</span></span>
   * <span data-ttu-id="444e0-120">Ellenőrizze az Adventure Works mindegyikhez hello sorok száma</span><span class="sxs-lookup"><span data-stu-id="444e0-120">Validate hello row counts for each Adventure Works table</span></span>
   * <span data-ttu-id="444e0-121">Statisztika gyűjtése az Adventure Works mindegyikhez minden egyes oszlophoz</span><span class="sxs-lookup"><span data-stu-id="444e0-121">Collect statistics on every column for each Adventure Works table</span></span>

## <a name="query-sample-data"></a><span data-ttu-id="444e0-122">Lekérdezés mintaadatokat</span><span class="sxs-lookup"><span data-stu-id="444e0-122">Query sample data</span></span>
<span data-ttu-id="444e0-123">Ha néhány adatot az SQL Data Warehouse-bA már betöltött, gyorsan néhány lekérdezéseket is futtathat.</span><span class="sxs-lookup"><span data-stu-id="444e0-123">Once you've loaded some sample data into your SQL Data Warehouse, you can quickly run a few queries.</span></span>  <span data-ttu-id="444e0-124">toorun lekérdezés, csatlakoztassa az újonnan létrehozott tooyour Adventure Works adatbázisa a Visual Studio és az SSDT, Azure SQL DW, hello leírtaknak [lekérdezése a Visual Studio] [ query with Visual Studio] dokumentum.</span><span class="sxs-lookup"><span data-stu-id="444e0-124">toorun a query, connect tooyour newly created Adventure Works database in Azure SQL DW using Visual Studio and SSDT, as described in hello [query with Visual Studio][query with Visual Studio] document.</span></span>

<span data-ttu-id="444e0-125">Az egyszerű példában válassza ki a utasítás tooget összes hello adatai hello alkalmazottak:</span><span class="sxs-lookup"><span data-stu-id="444e0-125">Example of simple select statement tooget all hello info of hello employees:</span></span>

```sql
SELECT * FROM DimEmployee;
```

<span data-ttu-id="444e0-126">Egy összetettebb lekérdezés használata szerkezetek GROUP BY toolook például a teljes összeg hello az összes értékesítés naponta példát:</span><span class="sxs-lookup"><span data-stu-id="444e0-126">Example of a more complex query using constructs such as GROUP BY toolook at hello total amount for all sales on each day:</span></span>

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="444e0-127">A WHERE záradék toofilter ki egy bizonyos időpont előtt rendelést a SELECT példát:</span><span class="sxs-lookup"><span data-stu-id="444e0-127">Example of a SELECT with a WHERE clause toofilter out orders from before a certain date:</span></span>

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="444e0-128">Az SQL Data Warehouse szinte minden, amely támogatja az SQL Server T-SQL szerkezetek támogatja.</span><span class="sxs-lookup"><span data-stu-id="444e0-128">SQL Data Warehouse supports almost all T-SQL constructs which SQL Server supports.</span></span>  <span data-ttu-id="444e0-129">Bármely különbségek vannak dokumentálva a [telepítse át a kódot] [ migrate code] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="444e0-129">Any differences are documented in our [migrate code][migrate code] documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="444e0-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="444e0-130">Next steps</span></span>
<span data-ttu-id="444e0-131">Most, hogy egy alkalommal tootry már volt néhány lekérdezést mintaadatokkal, ellenőrizze, hogyan túl[fejlesztése][develop], [betölteni][load], vagy [ Telepítse át] [ migrate] tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="444e0-131">Now that you've had a chance tootry some queries with sample data, check out how too[develop][develop], [load][load], or [migrate][migrate] tooSQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
