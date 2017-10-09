## <a name="repeatability-during-copy"></a><span data-ttu-id="6e5b0-101">Ismételhetőség másolása során</span><span class="sxs-lookup"><span data-stu-id="6e5b0-101">Repeatability during Copy</span></span>
<span data-ttu-id="6e5b0-102">Amikor adatok tooAzure SQL/SQL Server másolását más adatokat tárol egy igények tookeep ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-102">When copying data tooAzure SQL/SQL Server from other data stores one needs tookeep repeatability in mind tooavoid unintended outcomes.</span></span> 

<span data-ttu-id="6e5b0-103">SQL-/ SQL Server-adatbázis adatok tooAzure másolásakor másolási tevékenység fog alapértelmezett APPEND hello adatkészlet toohello fogadó tábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-103">When copying data tooAzure SQL/SQL Server Database, copy activity will by default APPEND hello data set toohello sink table by default.</span></span> <span data-ttu-id="6e5b0-104">Például az adatok másolása egy CSV (vesszővel tagolt értékek) fájlt tartalmazó adatforrást két tooAzure SQL/SQL Server-adatbázis rögzíti, amikor ez legyen milyen hello táblázat láthatóhoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="6e5b0-104">For example, when copying data from a CSV (comma separated values data) file source containing two records tooAzure SQL/SQL Server Database, this is what hello table looks like:</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="6e5b0-105">Tegyük fel, hogy a hibákat a forrásfájl és a frissített hello mennyiséget le cső 2 too4 hello forrásfájlban talált.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-105">Suppose you found errors in source file and updated hello quantity of Down Tube from 2 too4 in hello source file.</span></span> <span data-ttu-id="6e5b0-106">Ha újra futtatni az adott időszakra hello adatszelet, két új bejegyzést fűzött tooAzure SQL/SQL Server-adatbázis találhat.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-106">If you re-run hello data slice for that period, you’ll find two new records appended tooAzure SQL/SQL Server Database.</span></span> <span data-ttu-id="6e5b0-107">az alábbi hello azt feltételezi, hogy hello tábla oszlopainak hello egyik sincs hello elsődlegeskulcs-megkötés.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-107">hello below assumes none of hello columns in hello table have hello primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="6e5b0-108">tooavoid, toospecify UPSERT szemantikáját hello alatt alábbiakban említett 2 mechanizmusok használatával szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-108">tooavoid this, you will need toospecify UPSERT semantics by leveraging one of hello below 2 mechanisms stated below.</span></span>

> [!NOTE]
> <span data-ttu-id="6e5b0-109">A szelet futtatható újra automatikusan az Azure Data Factory megadott hello újrapróbálkozási házirend szerint.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-109">A slice can be re-run automatically in Azure Data Factory as per hello retry policy specified.</span></span>
> 
> 

### <a name="mechanism-1"></a><span data-ttu-id="6e5b0-110">1 mechanizmus</span><span class="sxs-lookup"><span data-stu-id="6e5b0-110">Mechanism 1</span></span>
<span data-ttu-id="6e5b0-111">Kihasználhatja **sqlWriterCleanupScript** tulajdonság toofirst törlési művelet végrehajtása a szelet futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-111">You can leverage **sqlWriterCleanupScript** property toofirst perform cleanup action when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="6e5b0-112">hello karbantartási parancsprogramot volna hajtható végre első példányát egy adott szelet, amely volna hello adatok törlése hello SQL táblázat megfelelő toothat szelet során.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-112">hello cleanup script would be executed first during copy for a given slice which would delete hello data from hello SQL Table corresponding toothat slice.</span></span> <span data-ttu-id="6e5b0-113">hello tevékenység később lesz hello adatok beszúrása hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-113">hello activity will subsequently insert hello data into hello SQL Table.</span></span> 

<span data-ttu-id="6e5b0-114">Ha hello szelet most újra futtatni, akkor megkeresi hello mennyiség frissíteni, mivel a szükséges.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-114">If hello slice is now re-run, then you will find hello quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="6e5b0-115">Tegyük fel, hello strukturálatlan szélvédőmosó rekordot a rendszer eltávolítja a hello eredeti csv.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-115">Suppose hello Flat Washer record is removed from hello original csv.</span></span> <span data-ttu-id="6e5b0-116">Akkor majd az ismételt futtatásával hello szelet hello eredménye a következő eredmény:</span><span class="sxs-lookup"><span data-stu-id="6e5b0-116">Then re-running hello slice would produce hello following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
<span data-ttu-id="6e5b0-117">Újdonságunk kellett toobe történik.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-117">Nothing new had toobe done.</span></span> <span data-ttu-id="6e5b0-118">hello másolási tevékenység hello tisztítás parancsfájl toodelete hello megfelelő adatokat, hogy a szelet futott.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-118">hello copy activity ran hello cleanup script toodelete hello corresponding data for that slice.</span></span> <span data-ttu-id="6e5b0-119">Ezután hello bemeneti olvasni (amely majd tartalmazott mindössze 1 bejegyzés) hello csv és illeszteni a hello tábla.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-119">Then it read hello input from hello csv (which then contained only 1 record) and inserted it into hello Table.</span></span> 

### <a name="mechanism-2"></a><span data-ttu-id="6e5b0-120">2 mechanizmus</span><span class="sxs-lookup"><span data-stu-id="6e5b0-120">Mechanism 2</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6e5b0-121">sliceIdentifierColumnName jelenleg nem támogatott az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-121">sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse at this time.</span></span> 

<span data-ttu-id="6e5b0-122">Egy másik mechanizmust tooachieve ismételhetőség van azzal, hogy a kijelölt oszlop (**sliceIdentifierColumnName**) hello a cél a táblában.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-122">Another mechanism tooachieve repeatability is by having a dedicated column (**sliceIdentifierColumnName**) in hello target Table.</span></span> <span data-ttu-id="6e5b0-123">Ez az oszlop Azure Data Factory tooensure hello forrás és cél maradás szinkronizált használhatják.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-123">This column would be used by Azure Data Factory tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="6e5b0-124">Ezt a módszert akkor működik, ha módosításával vagy hello cél SQL tábla sémáját definiáló rugalmasságot.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-124">This approach works when there is flexibility in changing or defining hello destination SQL Table schema.</span></span> 

<span data-ttu-id="6e5b0-125">Ebben az oszlopban által az Azure Data Factory ismételhetőség célból használják, és hello folyamat Azure Data Factory nem teszi meg semmilyen sémát toohello tábla változik.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-125">This column would be used by Azure Data Factory for repeatability purposes and in hello process Azure Data Factory will not make any schema changes toohello Table.</span></span> <span data-ttu-id="6e5b0-126">Módon toouse ezt a módszert használja:</span><span class="sxs-lookup"><span data-stu-id="6e5b0-126">Way toouse this approach:</span></span>

1. <span data-ttu-id="6e5b0-127">Hello cél SQL tábla típusú oszlop bináris (32) ad meg.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-127">Define a column of type binary (32) in hello destination SQL Table.</span></span> <span data-ttu-id="6e5b0-128">Ebben az oszlopban megkötés kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-128">There should be no constraints on this column.</span></span> <span data-ttu-id="6e5b0-129">Ehhez a példához tegyük nevezze ebben az oszlopban "ColumnForADFuseOnly".</span><span class="sxs-lookup"><span data-stu-id="6e5b0-129">Let's name this column as ‘ColumnForADFuseOnly’ for this example.</span></span>
2. <span data-ttu-id="6e5b0-130">Használja az hello másolási tevékenység az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6e5b0-130">Use it in hello copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

<span data-ttu-id="6e5b0-131">Az Azure Data Factory feltölti ezt az oszlopot a szükség szerinti tooensure hello forrás és cél szinkronizált maradnak.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-131">Azure Data Factory will populate this column as per its need tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="6e5b0-132">az oszlop értékeinek hello nem használható ebben a környezetben kívül hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-132">hello values of this column should not be used outside of this context by hello user.</span></span> 

<span data-ttu-id="6e5b0-133">Hasonló toomechanism 1, a másolási tevékenység során lesz automatikusan első hello adatok számára megadott szelet a hello célhelyről SQL táblázat, és futtassa az hello másolási tevékenység során általában hello karbantartása az, hogy a szelet forrás toodestination tooinsert hello adatait.</span><span class="sxs-lookup"><span data-stu-id="6e5b0-133">Similar toomechanism 1, Copy Activity will automatically first clean up hello data for hello given slice from hello destination SQL Table and then run hello copy activity normally tooinsert hello data from source toodestination for that slice.</span></span> 

