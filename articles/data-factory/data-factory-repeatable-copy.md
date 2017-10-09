---
title: "az Azure Data Factory aaaRepeatable másolása |} Microsoft Docs"
description: "Ismerje meg, hogyan tooavoid másolatot készít, annak ellenére, hogy a szelet, amely másolja az adatokat csak egyszer fut-e."
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
ms.openlocfilehash: 79a3fde2b700bf0a0e167479d6a86c5bee1bf7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a><span data-ttu-id="0d011-103">Az Azure Data Factory ismételhető másolása</span><span class="sxs-lookup"><span data-stu-id="0d011-103">Repeatable copy in Azure Data Factory</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="0d011-104">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="0d011-104">Repeatable read from relational sources</span></span>
<span data-ttu-id="0d011-105">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="0d011-105">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="0d011-106">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="0d011-106">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="0d011-107">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="0d011-107">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="0d011-108">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="0d011-108">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span>  
 
> [!NOTE]
> <span data-ttu-id="0d011-109">hello következő mintákat az Azure SQL azonban találhatók, amely támogatja a téglalap alakú adatkészletek alkalmazható tooany adattár.</span><span class="sxs-lookup"><span data-stu-id="0d011-109">hello following samples are for Azure SQL but are applicable tooany data store that supports rectangular datasets.</span></span> <span data-ttu-id="0d011-110">Előfordulhat, hogy tooadjust hello **típus** a forrás- és hello **lekérdezés** tulajdonság (például: lekérdezés helyett sqlReaderQuery) hello adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="0d011-110">You may have tooadjust hello **type** of source and hello **query** property (for example: query instead of sqlReaderQuery) for hello data store.</span></span>   

<span data-ttu-id="0d011-111">Általában relációs áruházakból olvasásakor érdemes tooread toothat szelet megfelelő csak hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d011-111">Usually, when reading from relational stores, you want tooread only hello data corresponding toothat slice.</span></span> <span data-ttu-id="0d011-112">Egy módon toodo így lenne hello WindowStart és WindowEnd rendszerváltozók érhető el az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="0d011-112">A way toodo so would be by using hello WindowStart and WindowEnd system variables available in Azure Data Factory.</span></span> <span data-ttu-id="0d011-113">További információ a hello változók és függvényeket az Azure Data Factory itt hello [Azure Data Factory - funkciók és rendszerváltozók](data-factory-functions-variables.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="0d011-113">Read about hello variables and functions in Azure Data Factory here in hello [Azure Data Factory - Functions and System Variables](data-factory-functions-variables.md) article.</span></span> <span data-ttu-id="0d011-114">Példa:</span><span class="sxs-lookup"><span data-stu-id="0d011-114">Example:</span></span> 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
<span data-ttu-id="0d011-115">Ez a lekérdezés MyTable hello táblából (WindowStart -> WindowEnd) hello szelet időtartama közé eső adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0d011-115">This query reads data that falls in hello slice duration range (WindowStart -> WindowEnd) from hello table MyTable.</span></span> <span data-ttu-id="0d011-116">Futtassa újból a szeletet a is minden esetben gondoskodjon arról, hogy adott hello olvasható ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d011-116">Rerun of this slice would also always ensure that hello same data is read.</span></span> 

<span data-ttu-id="0d011-117">Más esetekben lehet szeretnének tooread hello teljes táblázat és a következőképpen határozhatnak meg hello sqlReaderQuery:</span><span class="sxs-lookup"><span data-stu-id="0d011-117">In other cases, you may wish tooread hello entire table and may define hello sqlReaderQuery as follows:</span></span>

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-toosqlsink"></a><span data-ttu-id="0d011-118">Ismételhető írási tooSqlSink</span><span class="sxs-lookup"><span data-stu-id="0d011-118">Repeatable write tooSqlSink</span></span>
<span data-ttu-id="0d011-119">Ha az adatok másolásának túl**Azure SQL-/ SQL Server** az egyéb adattárakhoz tookeep ismételhetőség a szem előtt tartva tooavoid kell nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="0d011-119">When copying data too**Azure SQL/SQL Server** from other data stores, you need tookeep repeatability in mind tooavoid unintended outcomes.</span></span> 

<span data-ttu-id="0d011-120">Adatok tooAzure SQL/SQL Server-adatbázis másolásakor hello másolási tevékenység hozzáfűzi toohello fogadó adattábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="0d011-120">When copying data tooAzure SQL/SQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="0d011-121">Mondja ki másolja át adatokat a egy CSV (vesszővel tagolt) fájlt tartalmazó két rekordok toohello a következő táblázat egy Azure SQL-/ SQL Server-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="0d011-121">Say, you are copying data from a CSV (comma-separated values) file containing two records toohello following table in an Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="0d011-122">A szelet futtatásakor hello két rekordok olyan másolt toohello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="0d011-122">When a slice runs, hello two records are copied toohello SQL table.</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="0d011-123">Tegyük fel, hogy hiba található a forrásfájl és hello mennyiséget le cső 2 too4 frissítése.</span><span class="sxs-lookup"><span data-stu-id="0d011-123">Suppose you found errors in source file and updated hello quantity of Down Tube from 2 too4.</span></span> <span data-ttu-id="0d011-124">Ha hello adatszelet manuálisan adott időszakra Újrafuttatja, két új bejegyzést fűzött tooAzure SQL/SQL Server-adatbázis találhat.</span><span class="sxs-lookup"><span data-stu-id="0d011-124">If you rerun hello data slice for that period manually, you’ll find two new records appended tooAzure SQL/SQL Server Database.</span></span> <span data-ttu-id="0d011-125">Ez a példa feltételezi, hogy nincs hello tábla oszlopainak hello rendelkezik-e hello elsődlegeskulcs-megkötés.</span><span class="sxs-lookup"><span data-stu-id="0d011-125">This example assumes that none of hello columns in hello table has hello primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="0d011-126">tooavoid ezt a viselkedést kell toospecify UPSERT szemantikáját használja a következő két mechanizmus hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="0d011-126">tooavoid this behavior, you need toospecify UPSERT semantics by using one of hello following two mechanisms:</span></span>

### <a name="mechanism-1-using-sqlwritercleanupscript"></a><span data-ttu-id="0d011-127">1. módszer: sqlWriterCleanupScript használatával</span><span class="sxs-lookup"><span data-stu-id="0d011-127">Mechanism 1: using sqlWriterCleanupScript</span></span>
<span data-ttu-id="0d011-128">Használhatja a hello **sqlWriterCleanupScript** tulajdonság tooclean mentése előtt hello adatok beszúrása szelet futtatásakor hello fogadó tábla adatait.</span><span class="sxs-lookup"><span data-stu-id="0d011-128">You can use hello **sqlWriterCleanupScript** property tooclean up data from hello sink table before inserting hello data when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="0d011-129">A szelet futtatásakor hello tisztítás parancsfájl futtatása első toodelete adat, amely megfelel a toohello szelet hello SQL-táblából.</span><span class="sxs-lookup"><span data-stu-id="0d011-129">When a slice runs, hello cleanup script is run first toodelete data that corresponds toohello slice from hello SQL table.</span></span> <span data-ttu-id="0d011-130">hello másolási tevékenység majd adatbeszúrást hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="0d011-130">hello copy activity then inserts data into hello SQL Table.</span></span> <span data-ttu-id="0d011-131">Hello szelet akkor fut újra, ha hello mennyiség frissül igény szerint.</span><span class="sxs-lookup"><span data-stu-id="0d011-131">If hello slice is rerun, hello quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="0d011-132">Tegyük fel, hello strukturálatlan szélvédőmosó rekordot a rendszer eltávolítja a hello eredeti csv.</span><span class="sxs-lookup"><span data-stu-id="0d011-132">Suppose hello Flat Washer record is removed from hello original csv.</span></span> <span data-ttu-id="0d011-133">Majd újrafuttatásakor valószínűleg megtörténik a hello szelet hozna hello eredménye a következő:</span><span class="sxs-lookup"><span data-stu-id="0d011-133">Then rerunning hello slice would produce hello following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="0d011-134">hello másolási tevékenység hello tisztítás parancsfájl toodelete hello megfelelő adatokat, hogy a szelet futott.</span><span class="sxs-lookup"><span data-stu-id="0d011-134">hello copy activity ran hello cleanup script toodelete hello corresponding data for that slice.</span></span> <span data-ttu-id="0d011-135">Ezután hello bemeneti olvasni (amely csak egy bejegyzés majd tartalmazott) hello csv és illeszteni a hello tábla.</span><span class="sxs-lookup"><span data-stu-id="0d011-135">Then it read hello input from hello csv (which then contained only one record) and inserted it into hello Table.</span></span> 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a><span data-ttu-id="0d011-136">2. módszer: sliceIdentifierColumnName használatával</span><span class="sxs-lookup"><span data-stu-id="0d011-136">Mechanism 2: using sliceIdentifierColumnName</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0d011-137">SliceIdentifierColumnName az Azure SQL Data Warehouse nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="0d011-137">Currently, sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="0d011-138">hello második mechanizmus tooachieve ismételhetőség van azzal, hogy a kijelölt oszlop (sliceIdentifierColumnName) hello cél tábla.</span><span class="sxs-lookup"><span data-stu-id="0d011-138">hello second mechanism tooachieve repeatability is by having a dedicated column (sliceIdentifierColumnName) in hello target Table.</span></span> <span data-ttu-id="0d011-139">Ez az oszlop Azure Data Factory tooensure hello forrás és cél maradás szinkronizált használhatják.</span><span class="sxs-lookup"><span data-stu-id="0d011-139">This column would be used by Azure Data Factory tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="0d011-140">Ezt a módszert akkor működik, ha módosításával vagy hello cél SQL tábla sémáját definiáló rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="0d011-140">This approach works when there is flexibility in changing or defining hello destination SQL Table schema.</span></span> 

<span data-ttu-id="0d011-141">Ez az oszlop Azure Data Factory ismételhetőség célra használja, és hello folyamat Azure Data Factory nem végzi a séma toohello tábla változik.</span><span class="sxs-lookup"><span data-stu-id="0d011-141">This column is used by Azure Data Factory for repeatability purposes and in hello process Azure Data Factory does not make any schema changes toohello Table.</span></span> <span data-ttu-id="0d011-142">Módon toouse ezt a módszert használja:</span><span class="sxs-lookup"><span data-stu-id="0d011-142">Way toouse this approach:</span></span>

1. <span data-ttu-id="0d011-143">Adja meg a típusú oszlop **bináris (32)** hello cél SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="0d011-143">Define a column of type **binary (32)** in hello destination SQL Table.</span></span> <span data-ttu-id="0d011-144">Ebben az oszlopban megkötés kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0d011-144">There should be no constraints on this column.</span></span> <span data-ttu-id="0d011-145">Ehhez a példához tegyük nevezze ebben az oszlopban AdfSliceIdentifier.</span><span class="sxs-lookup"><span data-stu-id="0d011-145">Let's name this column as AdfSliceIdentifier for this example.</span></span>


    <span data-ttu-id="0d011-146">Forrástábla:</span><span class="sxs-lookup"><span data-stu-id="0d011-146">Source table:</span></span>

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    <span data-ttu-id="0d011-147">Céltábla:</span><span class="sxs-lookup"><span data-stu-id="0d011-147">Destination table:</span></span> 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. <span data-ttu-id="0d011-148">Használja az hello másolási tevékenység az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0d011-148">Use it in hello copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

<span data-ttu-id="0d011-149">Az Azure Data Factory tölti fel ezt az oszlopot a szükség szerinti tooensure hello forrás és cél szinkronizált maradnak.</span><span class="sxs-lookup"><span data-stu-id="0d011-149">Azure Data Factory populates this column as per its need tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="0d011-150">az oszlop értékeinek hello kívül ebben a környezetben nem használható.</span><span class="sxs-lookup"><span data-stu-id="0d011-150">hello values of this column should not be used outside of this context.</span></span> 

<span data-ttu-id="0d011-151">Hasonló toomechanism 1, a másolási tevékenység során automatikusan törli a szükségtelenné megadott szelet a hello célhelyről SQL táblázat hello hello adatait.</span><span class="sxs-lookup"><span data-stu-id="0d011-151">Similar toomechanism 1, Copy Activity automatically cleans up hello data for hello given slice from hello destination SQL Table.</span></span> <span data-ttu-id="0d011-152">Toohello céltáblázatban forrásból származó adatok majd beszúrása.</span><span class="sxs-lookup"><span data-stu-id="0d011-152">It then inserts data from source in toohello destination table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0d011-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d011-153">Next steps</span></span>
<span data-ttu-id="0d011-154">Tekintse át a következő összekötő cikket, amely a JSON-példák befejezéséhez hello:</span><span class="sxs-lookup"><span data-stu-id="0d011-154">Review hello following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="0d011-155">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0d011-155">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="0d011-156">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0d011-156">Azure SQL Data Warehouse</span></span>](data-factory-azure-sql-data-warehouse-connector.md)
- [<span data-ttu-id="0d011-157">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0d011-157">SQL Server</span></span>](data-factory-sqlserver-connector.md)