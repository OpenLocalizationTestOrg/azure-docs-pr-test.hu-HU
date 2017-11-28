---
title: "Particionálás és az Azure Cosmos Adatbázisba skálázás |} Microsoft Docs"
description: "Ismerje meg, hogyan particionálási működését Azure Cosmos DB, hogyan lehet konfigurálni a particionálás és kulcsok partícióazonosító és hogyan válassza ki a megfelelő partíciókulcs az alkalmazáshoz."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 702c39b4-1798-48dd-9993-4493a2f6df9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81010d91ac7fe8fa7149c52ed56af304cf4e83d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-the-documentdb-api"></a><span data-ttu-id="e56dc-103">Az Azure Cosmos Adatbázisba a DocumentDB API használatával particionálás</span><span class="sxs-lookup"><span data-stu-id="e56dc-103">Partitioning in Azure Cosmos DB using the DocumentDB API</span></span>

<span data-ttu-id="e56dc-104">[A Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) egy globális elosztott, több modellre adatbázis szolgáltatás célja, hogy gyors és kiszámítható teljesítmény és méretezhetőség zökkenőmentesen együtt az alkalmazás eléréséhez, növekedésével azt.</span><span class="sxs-lookup"><span data-stu-id="e56dc-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is a global distributed, multi-model database service designed to help you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> 

<span data-ttu-id="e56dc-105">Ez a cikk áttekintést Cosmos DB tároló DocumentDB API-val particionálás használata.</span><span class="sxs-lookup"><span data-stu-id="e56dc-105">This article provides an overview of how to work with partitioning of Cosmos DB containers with the DocumentDB API.</span></span> <span data-ttu-id="e56dc-106">Lásd: [particionálás és horizontális skálázás](../cosmos-db/partition-data.md) fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintését.</span><span class="sxs-lookup"><span data-stu-id="e56dc-106">See [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

<span data-ttu-id="e56dc-107">Első lépésként kóddal, töltse le a projektet a [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="e56dc-107">To get started with code, download the project from [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<span data-ttu-id="e56dc-108">A cikk elolvasása után lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="e56dc-108">After reading this article, you will be able to answer the following questions:</span></span>   

* <span data-ttu-id="e56dc-109">Hogyan működik az Azure Cosmos Adatbázisba particionálási munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="e56dc-109">How does partitioning work in Azure Cosmos DB?</span></span>
* <span data-ttu-id="e56dc-110">Hogyan konfigurálhatók a particionálás az Azure Cosmos Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="e56dc-110">How do I configure partitioning in Azure Cosmos DB</span></span>
* <span data-ttu-id="e56dc-111">Mik azok a partíciós kulcsok, és hogyan tegye én választom ki a megfelelő partíciókulcs az alkalmazáshoz?</span><span class="sxs-lookup"><span data-stu-id="e56dc-111">What are partition keys, and how do I pick the right partition key for my application?</span></span>

<span data-ttu-id="e56dc-112">Első lépésként kóddal, töltse le a projektet a [Azure Cosmos DB teljesítményének tesztelése illesztőprogram minta](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="e56dc-112">To get started with code, download the project from [Azure Cosmos DB Performance Testing Driver Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a><span data-ttu-id="e56dc-113">Partíciós kulcsok</span><span class="sxs-lookup"><span data-stu-id="e56dc-113">Partition keys</span></span>

<span data-ttu-id="e56dc-114">A DocumentDB API-t a partíciós kulcs definíciójában egy JSON-útvonal formájában kell megadni.</span><span class="sxs-lookup"><span data-stu-id="e56dc-114">In the DocumentDB API, you specify the partition key definition in the form of a JSON path.</span></span> <span data-ttu-id="e56dc-115">Az alábbi táblázat példákat partíció fontos definíciókat és a megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="e56dc-115">The following table shows examples of partition key definitions and the values corresponding to each.</span></span> <span data-ttu-id="e56dc-116">A partíciós kulcs van megadva egy elérési utat, mint pl. `/department` jelenti. a tulajdonság részleg.</span><span class="sxs-lookup"><span data-stu-id="e56dc-116">The partition key is specified as a path, e.g. `/department` represents the property department.</span></span> 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="e56dc-117"><strong>Partíciókulcs</strong></span><span class="sxs-lookup"><span data-stu-id="e56dc-117"><strong>Partition Key</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e56dc-118"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="e56dc-118"><strong>Description</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e56dc-119">/Department</span><span class="sxs-lookup"><span data-stu-id="e56dc-119">/department</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e56dc-120">A cikk esetén a doc doc.department értékének felel meg.</span><span class="sxs-lookup"><span data-stu-id="e56dc-120">Corresponds to the value of doc.department where doc is the item.</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e56dc-121">/ tulajdonságok/neve</span><span class="sxs-lookup"><span data-stu-id="e56dc-121">/properties/name</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e56dc-122">Ahol doc-e az elem (beágyazott tulajdonság) doc.properties.name értékének felel meg.</span><span class="sxs-lookup"><span data-stu-id="e56dc-122">Corresponds to the value of doc.properties.name where doc is the item (nested property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e56dc-123">/ID</span><span class="sxs-lookup"><span data-stu-id="e56dc-123">/id</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e56dc-124">Doc.id értéke megfelel (azonosító és a partíciós kulcs, amelyek ugyanahhoz a tulajdonsághoz).</span><span class="sxs-lookup"><span data-stu-id="e56dc-124">Corresponds to the value of doc.id (id and partition key are the same property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="e56dc-125">/ "részleg neve"</span><span class="sxs-lookup"><span data-stu-id="e56dc-125">/"department name"</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="e56dc-126">Megfelel doc ["részleg neve"], ahol doc-e az elem értékét.</span><span class="sxs-lookup"><span data-stu-id="e56dc-126">Corresponds to the value of doc["department name"] where doc is the item.</span></span></p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> <span data-ttu-id="e56dc-127">A partíciós kulcs szintaxisa a következő elérési útja a legfontosabb különbség, hogy az elérési út a tulajdonság értéke nem felel meg házirend elérési utak indexelő hasonló, azaz nincs nem helyettesítő karakter végén.</span><span class="sxs-lookup"><span data-stu-id="e56dc-127">The syntax for partition key is similar to the path specification for indexing policy paths with the key difference that the path corresponds to the property instead of the value, i.e. there is no wild card at the end.</span></span> <span data-ttu-id="e56dc-128">Például lehet megadni/részleg /?</span><span class="sxs-lookup"><span data-stu-id="e56dc-128">For example, you would specify /department/?</span></span> <span data-ttu-id="e56dc-129">az index részleg alatt az értékeket, de adja meg a /department a partíciós kulcs definíciójában.</span><span class="sxs-lookup"><span data-stu-id="e56dc-129">to index the values under department, but specify /department as the partition key definition.</span></span> <span data-ttu-id="e56dc-130">A partíciós kulcs implicit módon indexelt, és nem zárható ki indexelő indexelési házirend felülbírálások használatával.</span><span class="sxs-lookup"><span data-stu-id="e56dc-130">The partition key is implicitly indexed and cannot be excluded from indexing using indexing policy overrides.</span></span>
> 
> 

<span data-ttu-id="e56dc-131">Nézzük milyen hatással van a partíciós kulcs kiválasztásakor az alkalmazás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="e56dc-131">Let's look at how the choice of partition key impacts the performance of your application.</span></span>

## <a name="working-with-the-azure-cosmos-db-sdks"></a><span data-ttu-id="e56dc-132">Az Azure Cosmos DB SDK-k használata</span><span class="sxs-lookup"><span data-stu-id="e56dc-132">Working with the Azure Cosmos DB SDKs</span></span>
<span data-ttu-id="e56dc-133">Az Azure Cosmos DB támogatása az automatikus particionálási [REST API verziója 2015-12-16](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="e56dc-133">Azure Cosmos DB added support for automatic partitioning with [REST API version 2015-12-16](/rest/api/documentdb/).</span></span> <span data-ttu-id="e56dc-134">Particionált tárolók létrehozásához le kell töltenie 1.6.0 SDK verzió vagy újabb valamelyik támogatott SDK platformon (.NET, Node.js, Java, Python, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="e56dc-134">In order to create partitioned containers, you must download SDK versions 1.6.0 or newer in one of the supported SDK platforms (.NET, Node.js, Java, Python, MongoDB).</span></span> 

### <a name="creating-containers"></a><span data-ttu-id="e56dc-135">Tárolók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e56dc-135">Creating containers</span></span>
<span data-ttu-id="e56dc-136">A következő példában egy .NET-részlet létrehozni egy tárolót a 20 000 kérelemegység / s átviteli eszköz telemetriai adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="e56dc-136">The following sample shows a .NET snippet to create a container to store device telemetry data of 20,000 request units per second of throughput.</span></span> <span data-ttu-id="e56dc-137">Az SDK-t állítja be a OfferThroughput (amelyek viszont beállítja a `x-ms-offer-throughput` kérelem fejléce a REST API-ban).</span><span class="sxs-lookup"><span data-stu-id="e56dc-137">The SDK sets the OfferThroughput value (which in turn sets the `x-ms-offer-throughput` request header in the REST API).</span></span> <span data-ttu-id="e56dc-138">Itt be van állítva a `/deviceId` partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="e56dc-138">Here we set the `/deviceId` as the partition key.</span></span> <span data-ttu-id="e56dc-139">A partíciós kulcs választott mentett a tároló metaadatait, például a nevét, és az indexelési házirendet a többi mellett.</span><span class="sxs-lookup"><span data-stu-id="e56dc-139">The choice of partition key is saved along with the rest of the container metadata like name and indexing policy.</span></span>

<span data-ttu-id="e56dc-140">Az ebben a példában azt kivételezett `deviceId` tudjuk, hogy (a) óta eszközök nagy számú, mivel a írások terjeszthető partíciók között egyenlően, és lehetővé téve, hogy a nagy mennyiségű adatot betöltési adatbázis méretezése és (b) számos a kérések, például egy eszköz a legújabb olvasási beolvasása egyetlen deviceId hatóköre, és egyetlen partícióra lekérhetők.</span><span class="sxs-lookup"><span data-stu-id="e56dc-140">For this sample, we picked `deviceId` since we know that (a) since there are a large number of devices, writes can be distributed across partitions evenly and allowing us to scale the database to ingest massive volumes of data and (b) many of the requests like fetching the latest reading for a device are scoped to a single deviceId and can be retrieved from a single partition.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here the property deviceId will be used as the partition key to 
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

<span data-ttu-id="e56dc-141">Ez a módszer lehetővé teszi a REST API hívása Cosmos DB, és a szolgáltatás kiépíti a kért átviteli sebesség alapján létrehozott partícióknak számos.</span><span class="sxs-lookup"><span data-stu-id="e56dc-141">This method makes a REST API call to Cosmos DB, and the service will provision a number of partitions based on the requested throughput.</span></span> <span data-ttu-id="e56dc-142">A teljesítmény kell fejlődnek tudja módosítani az átviteli sebesség a tároló.</span><span class="sxs-lookup"><span data-stu-id="e56dc-142">You can change the throughput of a container as your performance needs evolve.</span></span> 

### <a name="reading-and-writing-items"></a><span data-ttu-id="e56dc-143">Olvasott és írt elemek</span><span class="sxs-lookup"><span data-stu-id="e56dc-143">Reading and writing items</span></span>
<span data-ttu-id="e56dc-144">Most tegyük beszúrni Cosmos DB adatokat.</span><span class="sxs-lookup"><span data-stu-id="e56dc-144">Now, let's insert data into Cosmos DB.</span></span> <span data-ttu-id="e56dc-145">Íme egy minta osztály, amely tartalmazza egy eszközt, olvasása, és egy tárolóba olvasása új eszköz beszúrása Documentclient hívásakor.</span><span class="sxs-lookup"><span data-stu-id="e56dc-145">Here's a sample class containing a device reading, and a call to CreateDocumentAsync to insert a new device reading into a container.</span></span> <span data-ttu-id="e56dc-146">Íme egy kihasználva a DocumentDB API:</span><span class="sxs-lookup"><span data-stu-id="e56dc-146">This is an example leveraging the DocumentDB API:</span></span>

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

<span data-ttu-id="e56dc-147">Most olvassa el a cikk a partíciókulcs és azonosító, a frissítést, és utolsó lépésként, törölje azt partíciókulcs és azonosítója. Vegye figyelembe, hogy az olvasások adni egy PartitionKey (a megfelelő a `x-ms-documentdb-partitionkey` kérelem fejléce a REST API-ban).</span><span class="sxs-lookup"><span data-stu-id="e56dc-147">Let's read the item by its partition key and id, update it, and then as a final step, delete it by partition key and id. Note that the reads include a PartitionKey value (corresponding to the `x-ms-documentdb-partitionkey` request header in the REST API).</span></span>

```csharp
// Read document. Needs the partition key and the ID to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a><span data-ttu-id="e56dc-148">A particionált tárolók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e56dc-148">Querying partitioned containers</span></span>
<span data-ttu-id="e56dc-149">A particionált tárolókban lévő adatok lekérdezésekor Cosmos DB automatikusan továbbítja a lekérdezés a partíciókat a partíciókulcs-értékek a szűrőben megadott (ha vannak ilyenek) megfelelő.</span><span class="sxs-lookup"><span data-stu-id="e56dc-149">When you query data in partitioned containers, Cosmos DB automatically routes the query to the partitions corresponding to the partition key values specified in the filter (if there are any).</span></span> <span data-ttu-id="e56dc-150">Ez a lekérdezés például csak a partíciós kulcs "XMS-0001" tartalmazó partíció van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="e56dc-150">For example, this query is routed to just the partition containing the partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="e56dc-151">A következő lekérdezés nem rendelkezik egy szűrőt a partíciós kulcs (DeviceId), és minden olyan partíciónak, ahol hajtotta végre a partíció index alapján történő van rendezve.</span><span class="sxs-lookup"><span data-stu-id="e56dc-151">The following query does not have a filter on the partition key (DeviceId) and is fanned out to all partitions where it is executed against the partition's index.</span></span> <span data-ttu-id="e56dc-152">Vegye figyelembe, hogy meg kell adnia a EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` REST API-ja) kell rendelkeznie az SDK partíciók között a lekérdezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="e56dc-152">Note that you have to specify the EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in the REST API) to have the SDK to execute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

<span data-ttu-id="e56dc-153">Támogatja a cosmos DB [aggregátumfüggvények](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` és `AVG` over particionálva tárolók indítása az SDK-k 1.12.0 és újabb SQL használatával.</span><span class="sxs-lookup"><span data-stu-id="e56dc-153">Cosmos DB supports [aggregate functions](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` and `AVG` over partitioned containers using SQL starting with SDKs 1.12.0 and above.</span></span> <span data-ttu-id="e56dc-154">Lekérdezések tartalmaznia kell egy egyetlen összesítő operátor, és egyetlen értéket kell adni a leképezésben.</span><span class="sxs-lookup"><span data-stu-id="e56dc-154">Queries must include a single aggregate operator, and must include a single value in the projection.</span></span>

### <a name="parallel-query-execution"></a><span data-ttu-id="e56dc-155">Párhuzamos lekérdezés-végrehajtás</span><span class="sxs-lookup"><span data-stu-id="e56dc-155">Parallel query execution</span></span>
<span data-ttu-id="e56dc-156">A Cosmos DB SDK-k 1.9.0 és hajthat végre a particionált gyűjtemények, lekérdezések kis késés, még akkor is, amikor sok partíciók touch kell támogatási párhuzamos lekérdezés végrehajtási beállítások fent.</span><span class="sxs-lookup"><span data-stu-id="e56dc-156">The Cosmos DB SDKs 1.9.0 and above support parallel query execution options, which allow you to perform low latency queries against partitioned collections, even when they need to touch a large number of partitions.</span></span> <span data-ttu-id="e56dc-157">A következő lekérdezés például több partíció párhuzamosan futó van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e56dc-157">For example, the following query is configured to run in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="e56dc-158">A következő paraméterek hangolása kezelheti párhuzamos lekérdezés-végrehajtás:</span><span class="sxs-lookup"><span data-stu-id="e56dc-158">You can manage parallel query execution by tuning the following parameters:</span></span>

* <span data-ttu-id="e56dc-159">Úgy, hogy `MaxDegreeOfParallelism`, például a tároló partíciók egyidejű hálózati kapcsolatok maximális száma párhuzamos fokának szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="e56dc-159">By setting `MaxDegreeOfParallelism`, you can control the degree of parallelism i.e., the maximum number of simultaneous network connections to the container's partitions.</span></span> <span data-ttu-id="e56dc-160">Ha a-1, milyen párhuzamossági az SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="e56dc-160">If you set this to -1, the degree of parallelism is managed by the SDK.</span></span> <span data-ttu-id="e56dc-161">Ha a `MaxDegreeOfParallelism` nem megadott vagy kell állítani, 0, amely az alapértelmezett érték, a tároló partíciók egyetlen hálózati kapcsolattal lesz.</span><span class="sxs-lookup"><span data-stu-id="e56dc-161">If the `MaxDegreeOfParallelism` is not specified or set to 0, which is the default value, there will be a single network connection to the container's partitions.</span></span>
* <span data-ttu-id="e56dc-162">Úgy, hogy `MaxBufferedItemCount`, akkor is kompromisszumot lekérdezés-késleltetés és ügyféloldali memóriahasználata.</span><span class="sxs-lookup"><span data-stu-id="e56dc-162">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="e56dc-163">Ha kihagyja ezt a paramétert, vagy állítsa-1, párhuzamos lekérdezés-végrehajtás során pufferelt elemek száma. az SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="e56dc-163">If you omit this parameter or set this to -1, the number of items buffered during parallel query execution is managed by the SDK.</span></span>

<span data-ttu-id="e56dc-164">Megadott gyűjtemény olyan állapotban, a párhuzamos lekérdezések lesz az eredményeket ugyanabban a sorrendben, ahogy soros végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e56dc-164">Given the same state of the collection, a parallel query will return results in the same order as in serial execution.</span></span> <span data-ttu-id="e56dc-165">Rendezés (ORDER BY és/vagy felső) tartalmazó kereszt-partíció lekérdezés végrehajtásakor a az Azure Cosmos DB SDK állít ki a párhuzamos lekérdezés partíciók között, és egyesíti globálisan rendezett eredmények eredményezett ügyféloldali részben rendezett eredményez.</span><span class="sxs-lookup"><span data-stu-id="e56dc-165">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), the Azure Cosmos DB SDK issues the query in parallel across partitions and merges partially sorted results in the client side to produce globally ordered results.</span></span>

### <a name="executing-stored-procedures"></a><span data-ttu-id="e56dc-166">Tárolt eljárások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="e56dc-166">Executing stored procedures</span></span>
<span data-ttu-id="e56dc-167">Ilyen azonosítójú eszköz,-dokumentumokon végzett atomi tranzakciók is futtathat, például ha összesítések vagy csak egy elemet az eszközök aktuális állapotát most karbantartása.</span><span class="sxs-lookup"><span data-stu-id="e56dc-167">You can also execute atomic transactions against documents with the same device ID, e.g. if you're maintaining aggregates or the latest state of a device in a single item.</span></span> 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
<span data-ttu-id="e56dc-168">A következő szakaszban úgy tekintünk, hogyan viheti át a particionált tárolók egypartíciós tárolókból.</span><span class="sxs-lookup"><span data-stu-id="e56dc-168">In the next section, we look at how you can move to partitioned containers from single-partition containers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e56dc-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e56dc-169">Next steps</span></span>
<span data-ttu-id="e56dc-170">Ez a cikk azt megadott használata Azure Cosmos DB tároló DocumentDB API-val particionálás áttekintése.</span><span class="sxs-lookup"><span data-stu-id="e56dc-170">In this article, we provided an overview of how to work with partitioning of Azure Cosmos DB containers with the DocumentDB API.</span></span> <span data-ttu-id="e56dc-171">Lásd még: [particionálás és horizontális skálázás](../cosmos-db/partition-data.md) fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintését.</span><span class="sxs-lookup"><span data-stu-id="e56dc-171">Also see [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="e56dc-172">Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték.</span><span class="sxs-lookup"><span data-stu-id="e56dc-172">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="e56dc-173">Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.</span><span class="sxs-lookup"><span data-stu-id="e56dc-173">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="e56dc-174">Ismerkedés a kódolási a [SDK-k](documentdb-sdk-dotnet.md) vagy a [REST API-n](/rest/api/documentdb/)</span><span class="sxs-lookup"><span data-stu-id="e56dc-174">Get started coding with the [SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/)</span></span>
* <span data-ttu-id="e56dc-175">További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="e56dc-175">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>

