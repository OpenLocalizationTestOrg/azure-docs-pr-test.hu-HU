---
title: "Az Azure Data Factory ismételhető másolása |} Microsoft Docs"
description: "Megtanulhatja, hogyan ismétlődések annak ellenére, hogy a szelet, amely másolja az adatokat csak egyszer fut-e."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 5b88a235915bf35fec701eee4a5f80beb4a67632
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a><span data-ttu-id="fceb8-103">Az Azure Data Factory ismételhető másolása</span><span class="sxs-lookup"><span data-stu-id="fceb8-103">Repeatable copy in Azure Data Factory</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="fceb8-104">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="fceb8-104">Repeatable read from relational sources</span></span>
<span data-ttu-id="fceb8-105">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="fceb8-105">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="fceb8-106">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="fceb8-106">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="fceb8-107">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="fceb8-107">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="fceb8-108">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="fceb8-108">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span>  
 
> [!NOTE]
> <span data-ttu-id="fceb8-109">A következő mintákat az Azure SQL, de bármilyen adattároló, amely támogatja a téglalap alakú adatkészletek vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="fceb8-109">The following samples are for Azure SQL but are applicable to any data store that supports rectangular datasets.</span></span> <span data-ttu-id="fceb8-110">Előfordulhat, hogy úgy, hogy a **típus** forrás- és a **lekérdezés** tulajdonság (például: lekérdezés helyett sqlReaderQuery) az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="fceb8-110">You may have to adjust the **type** of source and the **query** property (for example: query instead of sqlReaderQuery) for the data store.</span></span>   

<span data-ttu-id="fceb8-111">Általában relációs áruházakból olvasásakor szeretne olvasni a csak az, hogy a szelet megfelelő adatokat.</span><span class="sxs-lookup"><span data-stu-id="fceb8-111">Usually, when reading from relational stores, you want to read only the data corresponding to that slice.</span></span> <span data-ttu-id="fceb8-112">Ehhez úgy lenne a WindowStart és WindowEnd rendszerváltozók érhető el az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="fceb8-112">A way to do so would be by using the WindowStart and WindowEnd system variables available in Azure Data Factory.</span></span> <span data-ttu-id="fceb8-113">Olvassa el a változók és az Azure Data Factory Itt a függvényt a [Azure Data Factory - funkciók és rendszerváltozók](data-factory-functions-variables.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="fceb8-113">Read about the variables and functions in Azure Data Factory here in the [Azure Data Factory - Functions and System Variables](data-factory-functions-variables.md) article.</span></span> <span data-ttu-id="fceb8-114">Példa:</span><span class="sxs-lookup"><span data-stu-id="fceb8-114">Example:</span></span> 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
<span data-ttu-id="fceb8-115">Ez a lekérdezés a táblából MyTable (WindowStart -> WindowEnd) szelet időtartama tartományba eső adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="fceb8-115">This query reads data that falls in the slice duration range (WindowStart -> WindowEnd) from the table MyTable.</span></span> <span data-ttu-id="fceb8-116">Futtassa újból a szelet mindig is biztosítja, hogy ugyanazokat az adatokat olvasható.</span><span class="sxs-lookup"><span data-stu-id="fceb8-116">Rerun of this slice would also always ensure that the same data is read.</span></span> 

<span data-ttu-id="fceb8-117">Más esetekben előfordulhat, hogy a teljes táblázat olvasni kíván, és az alábbiak szerint határozhatnak meg a sqlReaderQuery:</span><span class="sxs-lookup"><span data-stu-id="fceb8-117">In other cases, you may wish to read the entire table and may define the sqlReaderQuery as follows:</span></span>

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a><span data-ttu-id="fceb8-118">SqlSink ismételhető írása</span><span class="sxs-lookup"><span data-stu-id="fceb8-118">Repeatable write to SqlSink</span></span>
<span data-ttu-id="fceb8-119">Az adatok másolásakor **Azure SQL-/ SQL Server** az egyéb adattárakhoz ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében szükséges.</span><span class="sxs-lookup"><span data-stu-id="fceb8-119">When copying data to **Azure SQL/SQL Server** from other data stores, you need to keep repeatability in mind to avoid unintended outcomes.</span></span> 

<span data-ttu-id="fceb8-120">Amikor adat másolása az Azure SQL-/ SQL Server-adatbázis, a másolási tevékenység hozzáfűzi adatokat a fogadó tábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="fceb8-120">When copying data to Azure SQL/SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="fceb8-121">Tegyük fel például, adatokat másol egy Azure SQL-/ SQL Server-adatbázisban a következő táblázat két rekordokat tartalmazó CSV (vesszővel tagolt) fájlból.</span><span class="sxs-lookup"><span data-stu-id="fceb8-121">Say, you are copying data from a CSV (comma-separated values) file containing two records to the following table in an Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="fceb8-122">A szelet futtatásakor a két bejegyzés az SQL-tábla lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="fceb8-122">When a slice runs, the two records are copied to the SQL table.</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="fceb8-123">Tegyük fel, hogy hiba található a forrásfájl és a mennyiséget le cső 2 4 frissítése.</span><span class="sxs-lookup"><span data-stu-id="fceb8-123">Suppose you found errors in source file and updated the quantity of Down Tube from 2 to 4.</span></span> <span data-ttu-id="fceb8-124">Az adatszelet manuálisan adott időszakra futtassa, ha talál fűzött Azure SQL-/ SQL Server-adatbázis két új bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="fceb8-124">If you rerun the data slice for that period manually, you’ll find two new records appended to Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="fceb8-125">Ez a példa feltételezi, hogy rendelkezik-e az oszlopok a tábla egyik elsődleges kulcs megszorítását.</span><span class="sxs-lookup"><span data-stu-id="fceb8-125">This example assumes that none of the columns in the table has the primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="fceb8-126">Ez a viselkedés elkerülése érdekében meg kell adnia a UPSERT szemantikáját használja az alábbi két módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="fceb8-126">To avoid this behavior, you need to specify UPSERT semantics by using one of the following two mechanisms:</span></span>

### <a name="mechanism-1-using-sqlwritercleanupscript"></a><span data-ttu-id="fceb8-127">1. módszer: sqlWriterCleanupScript használatával</span><span class="sxs-lookup"><span data-stu-id="fceb8-127">Mechanism 1: using sqlWriterCleanupScript</span></span>
<span data-ttu-id="fceb8-128">Használhatja a **sqlWriterCleanupScript** tulajdonság előtt a beillesztéssel szelet futtatásakor a fogadó tábla adatainak karbantartását.</span><span class="sxs-lookup"><span data-stu-id="fceb8-128">You can use the **sqlWriterCleanupScript** property to clean up data from the sink table before inserting the data when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="fceb8-129">A szelet futtatásakor a tisztítás parancsfájl futtatása először törli az adatokat, amely megfelel a szelet az SQL-táblából.</span><span class="sxs-lookup"><span data-stu-id="fceb8-129">When a slice runs, the cleanup script is run first to delete data that corresponds to the slice from the SQL table.</span></span> <span data-ttu-id="fceb8-130">A másolási tevékenység majd adatbeszúrást az SQL-tábla.</span><span class="sxs-lookup"><span data-stu-id="fceb8-130">The copy activity then inserts data into the SQL Table.</span></span> <span data-ttu-id="fceb8-131">A szelet akkor fut újra, ha a mennyiség frissül igény szerint.</span><span class="sxs-lookup"><span data-stu-id="fceb8-131">If the slice is rerun, the quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="fceb8-132">Tegyük fel, a strukturálatlan szélvédőmosó rekordot a rendszer eltávolítja az eredeti csv a.</span><span class="sxs-lookup"><span data-stu-id="fceb8-132">Suppose the Flat Washer record is removed from the original csv.</span></span> <span data-ttu-id="fceb8-133">Majd futtassa újból a szeletet volna eredményt a következő:</span><span class="sxs-lookup"><span data-stu-id="fceb8-133">Then rerunning the slice would produce the following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="fceb8-134">A másolási tevékenység futott a karbantartási parancsprogramot a megfelelő adatokat, hogy a szelet törlése.</span><span class="sxs-lookup"><span data-stu-id="fceb8-134">The copy activity ran the cleanup script to delete the corresponding data for that slice.</span></span> <span data-ttu-id="fceb8-135">Ezt követően a bemeneti olvasni a fürt megosztott kötetei szolgáltatás (amelyeket majd csak egy rekord) és a táblába beszúrt.</span><span class="sxs-lookup"><span data-stu-id="fceb8-135">Then it read the input from the csv (which then contained only one record) and inserted it into the Table.</span></span> 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a><span data-ttu-id="fceb8-136">2. módszer: sliceIdentifierColumnName használatával</span><span class="sxs-lookup"><span data-stu-id="fceb8-136">Mechanism 2: using sliceIdentifierColumnName</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fceb8-137">SliceIdentifierColumnName az Azure SQL Data Warehouse nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fceb8-137">Currently, sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="fceb8-138">A második mechanizmus ismételhetőség eléréséhez van azzal, hogy a kijelölt oszlop (sliceIdentifierColumnName) a cél tábla.</span><span class="sxs-lookup"><span data-stu-id="fceb8-138">The second mechanism to achieve repeatability is by having a dedicated column (sliceIdentifierColumnName) in the target Table.</span></span> <span data-ttu-id="fceb8-139">Ebben az oszlopban szeretne győződjön meg arról, a forrás és cél maradnak szinkronizált Azure Data Factory által használni.</span><span class="sxs-lookup"><span data-stu-id="fceb8-139">This column would be used by Azure Data Factory to ensure the source and destination stay synchronized.</span></span> <span data-ttu-id="fceb8-140">Ezt a módszert akkor működik, ha módosításával vagy a cél SQL tábla sémáját definiáló rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="fceb8-140">This approach works when there is flexibility in changing or defining the destination SQL Table schema.</span></span> 

<span data-ttu-id="fceb8-141">Ebben az oszlopban Azure Data Factory ismételhetőség célra használja, és a folyamat az Azure Data Factory nem módosítja séma a táblában.</span><span class="sxs-lookup"><span data-stu-id="fceb8-141">This column is used by Azure Data Factory for repeatability purposes and in the process Azure Data Factory does not make any schema changes to the Table.</span></span> <span data-ttu-id="fceb8-142">Ez a megközelítés használatának módja:</span><span class="sxs-lookup"><span data-stu-id="fceb8-142">Way to use this approach:</span></span>

1. <span data-ttu-id="fceb8-143">Adja meg a típusú oszlop **bináris (32)** a célként megadott SQL-tábla.</span><span class="sxs-lookup"><span data-stu-id="fceb8-143">Define a column of type **binary (32)** in the destination SQL Table.</span></span> <span data-ttu-id="fceb8-144">Ebben az oszlopban megkötés kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fceb8-144">There should be no constraints on this column.</span></span> <span data-ttu-id="fceb8-145">Ehhez a példához tegyük nevezze ebben az oszlopban AdfSliceIdentifier.</span><span class="sxs-lookup"><span data-stu-id="fceb8-145">Let's name this column as AdfSliceIdentifier for this example.</span></span>


    <span data-ttu-id="fceb8-146">Forrástábla:</span><span class="sxs-lookup"><span data-stu-id="fceb8-146">Source table:</span></span>

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    <span data-ttu-id="fceb8-147">Céltábla:</span><span class="sxs-lookup"><span data-stu-id="fceb8-147">Destination table:</span></span> 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. <span data-ttu-id="fceb8-148">Használhatja a másolási tevékenység az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fceb8-148">Use it in the copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

<span data-ttu-id="fceb8-149">Az Azure Data Factory tölti fel. Ez az oszlop szerint annak érdekében, hogy a forrás és cél maradnak szinkronizált igényét.</span><span class="sxs-lookup"><span data-stu-id="fceb8-149">Azure Data Factory populates this column as per its need to ensure the source and destination stay synchronized.</span></span> <span data-ttu-id="fceb8-150">Az oszlop értékeinek kívül ebben a környezetben nem használható.</span><span class="sxs-lookup"><span data-stu-id="fceb8-150">The values of this column should not be used outside of this context.</span></span> 

<span data-ttu-id="fceb8-151">Hasonló mechanizmus 1, másolási tevékenység során automatikusan törli a szükségtelenné érkező SQL táblázat megadott szeletre vonatkozó adatokat.</span><span class="sxs-lookup"><span data-stu-id="fceb8-151">Similar to mechanism 1, Copy Activity automatically cleans up the data for the given slice from the destination SQL Table.</span></span> <span data-ttu-id="fceb8-152">Majd adatok beszúrása a forráskiszolgálóról a céltábla.</span><span class="sxs-lookup"><span data-stu-id="fceb8-152">It then inserts data from source in to the destination table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fceb8-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fceb8-153">Next steps</span></span>
<span data-ttu-id="fceb8-154">Tekintse át a következő összekötő, amely annak teljes JSON-példák:</span><span class="sxs-lookup"><span data-stu-id="fceb8-154">Review the following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="fceb8-155">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="fceb8-155">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="fceb8-156">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fceb8-156">Azure SQL Data Warehouse</span></span>](data-factory-azure-sql-data-warehouse-connector.md)
- [<span data-ttu-id="fceb8-157">SQL Server</span><span class="sxs-lookup"><span data-stu-id="fceb8-157">SQL Server</span></span>](data-factory-sqlserver-connector.md)