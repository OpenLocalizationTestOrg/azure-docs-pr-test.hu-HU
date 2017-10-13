## <a name="what-is-table-storage"></a><span data-ttu-id="14a53-101">A Table Storage ismertetése</span><span class="sxs-lookup"><span data-stu-id="14a53-101">What is Table storage</span></span>
<span data-ttu-id="14a53-102">Az Azure Table Storage nagy mennyiségű strukturált adat tárolására alkalmas.</span><span class="sxs-lookup"><span data-stu-id="14a53-102">Azure Table storage stores large amounts of structured data.</span></span> <span data-ttu-id="14a53-103">hello szolgáltatás áll, egy NoSQL-adattár, amely elfogadja érkező hitelesített hívásokat belüli és kívüli hello Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="14a53-103">hello service is a NoSQL datastore which accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="14a53-104">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="14a53-104">Azure tables are ideal for storing structured, non-relational data.</span></span> <span data-ttu-id="14a53-105">A Table Storage gyakori használati módjai:</span><span class="sxs-lookup"><span data-stu-id="14a53-105">Common uses of Table storage include:</span></span>

* <span data-ttu-id="14a53-106">Több TB-nyi, webes méretű alkalmazások kiszolgálására alkalmas strukturált adat tárolása</span><span class="sxs-lookup"><span data-stu-id="14a53-106">Storing TBs of structured data capable of serving web scale applications</span></span>
* <span data-ttu-id="14a53-107">Olyan adatkészletek tárolása, amelyekhez nincs szükség bonyolult illesztésekre, külső kulcsokra vagy tárolt eljárásokra, és a gyors hozzáférés érdekében denormalizálhatók</span><span class="sxs-lookup"><span data-stu-id="14a53-107">Storing datasets that don't require complex joins, foreign keys, or stored procedures and can be denormalized for fast access</span></span>
* <span data-ttu-id="14a53-108">Adatok gyors lekérdezése fürtözött indexszel</span><span class="sxs-lookup"><span data-stu-id="14a53-108">Quickly querying data using a clustered index</span></span>
* <span data-ttu-id="14a53-109">A WCF Data Service .NET-kódtárakra hello OData protokoll és a LINQ-lekérdezések használja adatok elérése</span><span class="sxs-lookup"><span data-stu-id="14a53-109">Accessing data using hello OData protocol and LINQ queries with WCF Data Service .NET Libraries</span></span>

<span data-ttu-id="14a53-110">Használhatja a Table storage toostore és lekérdezés hatalmas strukturált, nem relációs adatokat, és a táblák a igény szerinti növekedése lehessen méretezni.</span><span class="sxs-lookup"><span data-stu-id="14a53-110">You can use Table storage toostore and query huge sets of structured, non-relational data, and your tables will scale as demand increases.</span></span>

## <a name="table-storage-concepts"></a><span data-ttu-id="14a53-111">A Table Storage szolgáltatással kapcsolatos alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="14a53-111">Table storage concepts</span></span>
<span data-ttu-id="14a53-112">A TABLE storage a következő összetevők hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="14a53-112">Table storage  contains hello following components:</span></span>

![Table Storage-összetevők ábrája][Table1]

* <span data-ttu-id="14a53-114">**URL-címformátum:** A kód egy fiók tábláira hivatkozik az alábbi címformátummal:</span><span class="sxs-lookup"><span data-stu-id="14a53-114">**URL format:** Code addresses tables in an account using this address format:</span></span>   
  <span data-ttu-id="14a53-115">http://`<storage account>`.table.core.windows.net/`<table>`</span><span class="sxs-lookup"><span data-stu-id="14a53-115">http://`<storage account>`.table.core.windows.net/`<table>`</span></span>  
  
  <span data-ttu-id="14a53-116">Meg lehet oldani az Azure-táblákban közvetlenül hello OData protokoll fenti címet használja.</span><span class="sxs-lookup"><span data-stu-id="14a53-116">You can address Azure tables directly using this address with hello OData protocol.</span></span> <span data-ttu-id="14a53-117">További információk az [OData.org][OData.org] webhelyen találhatók.</span><span class="sxs-lookup"><span data-stu-id="14a53-117">For more information, see [OData.org][OData.org].</span></span>
* <span data-ttu-id="14a53-118">**Tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="14a53-118">**Storage Account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="14a53-119">A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Az Azure Storage méretezhetőségi és teljesítménycéljai).</span><span class="sxs-lookup"><span data-stu-id="14a53-119">See [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) for details about storage account capacity.</span></span>
* <span data-ttu-id="14a53-120">**Tábla:** A tábla az entitások gyűjteményét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="14a53-120">**Table**: A table is a collection of entities.</span></span> <span data-ttu-id="14a53-121">A táblák nem kényszerítenek sémát az entitásokra, ami azt jelenti, hogy egyetlen tábla különböző tulajdonságkészletekkel rendelkező entitásokat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="14a53-121">Tables don't enforce a schema on entities, which means a single table can contain entities that have different sets of properties.</span></span> <span data-ttu-id="14a53-122">a tárfiók tartalmazó táblák hello száma csak hello tárolási kapacitási határa korlátozza korlátozza.</span><span class="sxs-lookup"><span data-stu-id="14a53-122">hello number of tables that a storage account can contain is limited only by hello storage account capacity limit.</span></span>
* <span data-ttu-id="14a53-123">**Entitás**: entitás tulajdonságait, tooa adatbázissorhoz hasonló.</span><span class="sxs-lookup"><span data-stu-id="14a53-123">**Entity**: An entity is a set of properties, similar tooa database row.</span></span> <span data-ttu-id="14a53-124">Egy entitás mentése too1MB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="14a53-124">An entity can be up too1MB in size.</span></span>
* <span data-ttu-id="14a53-125">**Tulajdonságok:** A tulajdonság egy név-érték pár.</span><span class="sxs-lookup"><span data-stu-id="14a53-125">**Properties**: A property is a name-value pair.</span></span> <span data-ttu-id="14a53-126">Minden entitás too252 tulajdonságok toostore adatokat tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="14a53-126">Each entity can include up too252 properties toostore data.</span></span> <span data-ttu-id="14a53-127">Minden entitás három rendszertulajdonsággal rendelkezik, amelyek egy partíciókulcsot, egy sorkulcsot és egy időbélyegzőt adnak meg.</span><span class="sxs-lookup"><span data-stu-id="14a53-127">Each entity also has three system properties that specify a partition key, a row key, and a timestamp.</span></span> <span data-ttu-id="14a53-128">Hello ugyanazzal a partíciókulccsal kell gyorsabban lekérdezhetők, és beállíthatja, szűrhatók be/frissíthetők atomi műveletek rendelkező entitások.</span><span class="sxs-lookup"><span data-stu-id="14a53-128">Entities with hello same partition key can be queried more quickly, and inserted/updated in atomic operations.</span></span> <span data-ttu-id="14a53-129">Egy entitás sorkulcsa a partíción belüli azonosítója.</span><span class="sxs-lookup"><span data-stu-id="14a53-129">An entity's row key is its unique identifier within a partition.</span></span>

<span data-ttu-id="14a53-130">Táblák és tulajdonságok elnevezésével kapcsolatos részletekért lásd: [ismertetése hello tábla szolgáltatás adatmodell](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).</span><span class="sxs-lookup"><span data-stu-id="14a53-130">For details about naming tables and properties, see [Understanding hello Table Service Data Model](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).</span></span>

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/