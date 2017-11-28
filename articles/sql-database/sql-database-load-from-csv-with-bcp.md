---
title: "fürt megosztott kötetei szolgáltatás aaaLoad adatait fájlt az Azure SQL Database (bcp) |} Microsoft Docs"
description: "Kisebb adatméret esetében a bcp tooimport adatokat használja az Azure SQL-adatbázisba."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 9350e459aa844223820fbbd849a830cf0354d4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a><span data-ttu-id="e2847-103">Adatok betöltése CSV-fájlból az Azure SQL Database-be (egybesimított fájlok)</span><span class="sxs-lookup"><span data-stu-id="e2847-103">Load data from CSV into Azure SQL Database (flat files)</span></span>
<span data-ttu-id="e2847-104">Az Azure SQL-adatbázisba használható hello bcp parancssori segédprogram tooimport adatok CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="e2847-104">You can use hello bcp command-line utility tooimport data from a CSV file into Azure SQL Database.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e2847-105">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e2847-105">Before you begin</span></span>
### <a name="prerequisites"></a><span data-ttu-id="e2847-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e2847-106">Prerequisites</span></span>
<span data-ttu-id="e2847-107">toocomplete hello ebben a cikkben ismertetett visszaállítási lépésekkel, a szükséges:</span><span class="sxs-lookup"><span data-stu-id="e2847-107">toocomplete hello steps in this article, you need:</span></span>

* <span data-ttu-id="e2847-108">Egy Azure SQL Database logikai kiszolgáló és adatbázis</span><span class="sxs-lookup"><span data-stu-id="e2847-108">An Azure SQL Database logical server and database</span></span>
* <span data-ttu-id="e2847-109">hello telepített bcp parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="e2847-109">hello bcp command-line utility installed</span></span>
* <span data-ttu-id="e2847-110">hello telepített sqlcmd parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="e2847-110">hello sqlcmd command-line utility installed</span></span>

<span data-ttu-id="e2847-111">Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].</span><span class="sxs-lookup"><span data-stu-id="e2847-111">You can download hello bcp and sqlcmd utilities from hello [Microsoft Download Center][Microsoft Download Center].</span></span>

### <a name="data-in-ascii-or-utf-16-format"></a><span data-ttu-id="e2847-112">Adatok ASCII vagy UTF-16 formátumban</span><span class="sxs-lookup"><span data-stu-id="e2847-112">Data in ASCII or UTF-16 format</span></span>
<span data-ttu-id="e2847-113">Ha ez az oktatóanyag a saját adataival próbálja, az adatok kell toouse hello ASCII vagy UTF-16 kódolást, mert a bcp nem támogatja az UTF-8.</span><span class="sxs-lookup"><span data-stu-id="e2847-113">If you are trying this tutorial with your own data, your data needs toouse hello ASCII or UTF-16 encoding since bcp does not support UTF-8.</span></span> 

## <a name="1-create-a-destination-table"></a><span data-ttu-id="e2847-114">1. Céltábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2847-114">1. Create a destination table</span></span>
<span data-ttu-id="e2847-115">Adjon meg egy táblát az SQL-adatbázis hello cél táblázatként.</span><span class="sxs-lookup"><span data-stu-id="e2847-115">Define a table in SQL Database as hello destination table.</span></span> <span data-ttu-id="e2847-116">hello tábla oszlopainak hello toohello adatfájl egyes soraiban szereplő adatoknak kell megfelelnie.</span><span class="sxs-lookup"><span data-stu-id="e2847-116">hello columns in hello table must correspond toohello data in each row of your data file.</span></span>

<span data-ttu-id="e2847-117">toocreate tábla, nyisson meg egy parancssort, és az sqlcmd.exe toorun hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="e2847-117">toocreate a table, open a command prompt and use sqlcmd.exe toorun hello following command:</span></span>

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## <a name="2-create-a-source-data-file"></a><span data-ttu-id="e2847-118">2. Forrásadatfájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2847-118">2. Create a source data file</span></span>
<span data-ttu-id="e2847-119">Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új szöveges fájlba, és mentse a fájlt tooyour helyi ideiglenes könyvtárba C:\Temp\DimDate2.txt.</span><span class="sxs-lookup"><span data-stu-id="e2847-119">Open Notepad and copy hello following lines of data into a new text file and then save this file tooyour local temp directory, C:\Temp\DimDate2.txt.</span></span> <span data-ttu-id="e2847-120">Ezek az adatok ASCII formátumban vannak.</span><span class="sxs-lookup"><span data-stu-id="e2847-120">This data is in ASCII format.</span></span>

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

<span data-ttu-id="e2847-121">(Választható) tooexport a saját adatait egy SQL Server-adatbázisból, nyisson meg egy parancssort, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="e2847-121">(Optional) tooexport your own data from a SQL Server database, open a command prompt and run hello following command.</span></span> <span data-ttu-id="e2847-122">Cserélje le a TableName, ServerName, DatabaseName, Username és Password adatokat a saját adataira.</span><span class="sxs-lookup"><span data-stu-id="e2847-122">Replace TableName, ServerName, DatabaseName, Username, and Password with your own information.</span></span>

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-hello-data"></a><span data-ttu-id="e2847-123">3. Hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="e2847-123">3. Load hello data</span></span>
<span data-ttu-id="e2847-124">tooload hello adatok, nyisson meg egy parancssort, és futtassa a következő parancsot, a kiszolgáló nevét, a adatbázis nevét, a felhasználónév és a jelszó hello értékeket a saját cserélje le hello.</span><span class="sxs-lookup"><span data-stu-id="e2847-124">tooload hello data, open a command prompt and run hello following command, replacing hello values for Server Name, Database name, Username, and Password with your own information.</span></span>

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

<span data-ttu-id="e2847-125">A parancs tooverify hello megfelelően be adatok használata</span><span class="sxs-lookup"><span data-stu-id="e2847-125">Use this command tooverify hello data was loaded properly</span></span>

```bcp
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

<span data-ttu-id="e2847-126">hello eredményeket kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="e2847-126">hello results should look like this:</span></span>

| <span data-ttu-id="e2847-127">DateId</span><span class="sxs-lookup"><span data-stu-id="e2847-127">DateId</span></span> | <span data-ttu-id="e2847-128">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="e2847-128">CalendarQuarter</span></span> | <span data-ttu-id="e2847-129">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="e2847-129">FiscalQuarter</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e2847-130">20150101</span><span class="sxs-lookup"><span data-stu-id="e2847-130">20150101</span></span> |<span data-ttu-id="e2847-131">1</span><span class="sxs-lookup"><span data-stu-id="e2847-131">1</span></span> |<span data-ttu-id="e2847-132">3</span><span class="sxs-lookup"><span data-stu-id="e2847-132">3</span></span> |
| <span data-ttu-id="e2847-133">20150201</span><span class="sxs-lookup"><span data-stu-id="e2847-133">20150201</span></span> |<span data-ttu-id="e2847-134">1</span><span class="sxs-lookup"><span data-stu-id="e2847-134">1</span></span> |<span data-ttu-id="e2847-135">3</span><span class="sxs-lookup"><span data-stu-id="e2847-135">3</span></span> |
| <span data-ttu-id="e2847-136">20150301</span><span class="sxs-lookup"><span data-stu-id="e2847-136">20150301</span></span> |<span data-ttu-id="e2847-137">1</span><span class="sxs-lookup"><span data-stu-id="e2847-137">1</span></span> |<span data-ttu-id="e2847-138">3</span><span class="sxs-lookup"><span data-stu-id="e2847-138">3</span></span> |
| <span data-ttu-id="e2847-139">20150401</span><span class="sxs-lookup"><span data-stu-id="e2847-139">20150401</span></span> |<span data-ttu-id="e2847-140">2</span><span class="sxs-lookup"><span data-stu-id="e2847-140">2</span></span> |<span data-ttu-id="e2847-141">4</span><span class="sxs-lookup"><span data-stu-id="e2847-141">4</span></span> |
| <span data-ttu-id="e2847-142">20150501</span><span class="sxs-lookup"><span data-stu-id="e2847-142">20150501</span></span> |<span data-ttu-id="e2847-143">2</span><span class="sxs-lookup"><span data-stu-id="e2847-143">2</span></span> |<span data-ttu-id="e2847-144">4</span><span class="sxs-lookup"><span data-stu-id="e2847-144">4</span></span> |
| <span data-ttu-id="e2847-145">20150601</span><span class="sxs-lookup"><span data-stu-id="e2847-145">20150601</span></span> |<span data-ttu-id="e2847-146">2</span><span class="sxs-lookup"><span data-stu-id="e2847-146">2</span></span> |<span data-ttu-id="e2847-147">4</span><span class="sxs-lookup"><span data-stu-id="e2847-147">4</span></span> |
| <span data-ttu-id="e2847-148">20150701</span><span class="sxs-lookup"><span data-stu-id="e2847-148">20150701</span></span> |<span data-ttu-id="e2847-149">3</span><span class="sxs-lookup"><span data-stu-id="e2847-149">3</span></span> |<span data-ttu-id="e2847-150">1</span><span class="sxs-lookup"><span data-stu-id="e2847-150">1</span></span> |
| <span data-ttu-id="e2847-151">20150801</span><span class="sxs-lookup"><span data-stu-id="e2847-151">20150801</span></span> |<span data-ttu-id="e2847-152">3</span><span class="sxs-lookup"><span data-stu-id="e2847-152">3</span></span> |<span data-ttu-id="e2847-153">1</span><span class="sxs-lookup"><span data-stu-id="e2847-153">1</span></span> |
| <span data-ttu-id="e2847-154">20150801</span><span class="sxs-lookup"><span data-stu-id="e2847-154">20150801</span></span> |<span data-ttu-id="e2847-155">3</span><span class="sxs-lookup"><span data-stu-id="e2847-155">3</span></span> |<span data-ttu-id="e2847-156">1</span><span class="sxs-lookup"><span data-stu-id="e2847-156">1</span></span> |
| <span data-ttu-id="e2847-157">20151001</span><span class="sxs-lookup"><span data-stu-id="e2847-157">20151001</span></span> |<span data-ttu-id="e2847-158">4</span><span class="sxs-lookup"><span data-stu-id="e2847-158">4</span></span> |<span data-ttu-id="e2847-159">2</span><span class="sxs-lookup"><span data-stu-id="e2847-159">2</span></span> |
| <span data-ttu-id="e2847-160">20151101</span><span class="sxs-lookup"><span data-stu-id="e2847-160">20151101</span></span> |<span data-ttu-id="e2847-161">4</span><span class="sxs-lookup"><span data-stu-id="e2847-161">4</span></span> |<span data-ttu-id="e2847-162">2</span><span class="sxs-lookup"><span data-stu-id="e2847-162">2</span></span> |
| <span data-ttu-id="e2847-163">20151201</span><span class="sxs-lookup"><span data-stu-id="e2847-163">20151201</span></span> |<span data-ttu-id="e2847-164">4</span><span class="sxs-lookup"><span data-stu-id="e2847-164">4</span></span> |<span data-ttu-id="e2847-165">2</span><span class="sxs-lookup"><span data-stu-id="e2847-165">2</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e2847-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2847-166">Next steps</span></span>
<span data-ttu-id="e2847-167">egy SQL Server-adatbázis toomigrate lásd [SQL Server-adatbázis áttelepítése](sql-database-cloud-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="e2847-167">toomigrate a SQL Server database, see [SQL Server database migration](sql-database-cloud-migrate.md).</span></span>

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
