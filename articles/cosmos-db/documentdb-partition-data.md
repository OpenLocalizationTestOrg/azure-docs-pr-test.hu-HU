---
title: "aaaPartitioning és az Azure Cosmos Adatbázisba skálázás |} Microsoft Docs"
description: "Ismerje meg, hogyan particionálási működését az Azure Cosmos Adatbázisba, hogyan tooconfigure particionálás és partíciókulcsok, és hogyan toopick jobb hello partícióazonosító kulcs az alkalmazáshoz."
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
ms.openlocfilehash: 30621d2ba0b89efb72005680d5f3a73998347514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-hello-documentdb-api"></a><span data-ttu-id="70986-103">Az Azure Cosmos Adatbázisba hello DocumentDB API használatával particionálás</span><span class="sxs-lookup"><span data-stu-id="70986-103">Partitioning in Azure Cosmos DB using hello DocumentDB API</span></span>

<span data-ttu-id="70986-104">[A Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) van a globális elosztott, több modellre adatbázis-szolgáltatás, amely toohelp elérni gyors és kiszámítható teljesítmény és méretezhetőség zökkenőmentesen együtt az alkalmazás növekedésével azt.</span><span class="sxs-lookup"><span data-stu-id="70986-104">[Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) is a global distributed, multi-model database service designed toohelp you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> 

<span data-ttu-id="70986-105">Ez a cikk áttekintést hogyan toowork a Cosmos DB tárolók a particionálás hello DocumentDB API.</span><span class="sxs-lookup"><span data-stu-id="70986-105">This article provides an overview of how toowork with partitioning of Cosmos DB containers with hello DocumentDB API.</span></span> <span data-ttu-id="70986-106">Lásd: [particionálás és horizontális skálázás](../cosmos-db/partition-data.md) fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintését.</span><span class="sxs-lookup"><span data-stu-id="70986-106">See [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

<span data-ttu-id="70986-107">tooget kód használatába, töltse le a hello projekt [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="70986-107">tooget started with code, download hello project from [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<span data-ttu-id="70986-108">A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="70986-108">After reading this article, you will be able tooanswer hello following questions:</span></span>   

* <span data-ttu-id="70986-109">Hogyan működik az Azure Cosmos Adatbázisba particionálási munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="70986-109">How does partitioning work in Azure Cosmos DB?</span></span>
* <span data-ttu-id="70986-110">Hogyan konfigurálhatók a particionálás az Azure Cosmos Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="70986-110">How do I configure partitioning in Azure Cosmos DB</span></span>
* <span data-ttu-id="70986-111">Mik azok a partíciós kulcsok, és hogyan tegye én választom ki hello jobb partíciókulcs az alkalmazáshoz?</span><span class="sxs-lookup"><span data-stu-id="70986-111">What are partition keys, and how do I pick hello right partition key for my application?</span></span>

<span data-ttu-id="70986-112">tooget kód használatába, töltse le a hello projekt [Azure Cosmos DB teljesítményének tesztelése illesztőprogram minta](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="70986-112">tooget started with code, download hello project from [Azure Cosmos DB Performance Testing Driver Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark).</span></span> 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a><span data-ttu-id="70986-113">Partíciós kulcsok</span><span class="sxs-lookup"><span data-stu-id="70986-113">Partition keys</span></span>

<span data-ttu-id="70986-114">Hello DocumentDB API hello partíciós kulcs definíciójában egy JSON-útvonal hello formában kell megadni.</span><span class="sxs-lookup"><span data-stu-id="70986-114">In hello DocumentDB API, you specify hello partition key definition in hello form of a JSON path.</span></span> <span data-ttu-id="70986-115">hello következő táblázatban példák partíció fontos definíciókat és tooeach hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="70986-115">hello following table shows examples of partition key definitions and hello values corresponding tooeach.</span></span> <span data-ttu-id="70986-116">hello partíciós kulcs van megadva egy elérési utat, mint pl. `/department` jelöli hello tulajdonság részleg.</span><span class="sxs-lookup"><span data-stu-id="70986-116">hello partition key is specified as a path, e.g. `/department` represents hello property department.</span></span> 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="70986-117"><strong>Partíciókulcs</strong></span><span class="sxs-lookup"><span data-stu-id="70986-117"><strong>Partition Key</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="70986-118"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="70986-118"><strong>Description</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="70986-119">/Department</span><span class="sxs-lookup"><span data-stu-id="70986-119">/department</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="70986-120">Ahol doc az hello elem doc.department toohello értéke megfelel.</span><span class="sxs-lookup"><span data-stu-id="70986-120">Corresponds toohello value of doc.department where doc is hello item.</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="70986-121">/ tulajdonságok/neve</span><span class="sxs-lookup"><span data-stu-id="70986-121">/properties/name</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="70986-122">Ha doc elem hello (beágyazott tulajdonság) doc.properties.name toohello értékének felel meg.</span><span class="sxs-lookup"><span data-stu-id="70986-122">Corresponds toohello value of doc.properties.name where doc is hello item (nested property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="70986-123">/ID</span><span class="sxs-lookup"><span data-stu-id="70986-123">/id</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="70986-124">Doc.id toohello értéke megfelel (azonosító és a partíciós kulcs vannak hello azonos tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="70986-124">Corresponds toohello value of doc.id (id and partition key are hello same property).</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="70986-125">/ "részleg neve"</span><span class="sxs-lookup"><span data-stu-id="70986-125">/"department name"</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="70986-126">Megfelel a toohello doc ["részleg neve"], ahol doc az hello elem értékét.</span><span class="sxs-lookup"><span data-stu-id="70986-126">Corresponds toohello value of doc["department name"] where doc is hello item.</span></span></p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> <span data-ttu-id="70986-127">partíciókulcs hello szintaxisának hasonló toohello elérési utat az indexelés házirend elérési utak hello legfontosabb különbség, hogy az elérési út hello felel meg a toohello tulajdonság hello érték helyett, azaz van nem helyettesítő karakter hello végén.</span><span class="sxs-lookup"><span data-stu-id="70986-127">hello syntax for partition key is similar toohello path specification for indexing policy paths with hello key difference that hello path corresponds toohello property instead of hello value, i.e. there is no wild card at hello end.</span></span> <span data-ttu-id="70986-128">Például lehet megadni/részleg /?</span><span class="sxs-lookup"><span data-stu-id="70986-128">For example, you would specify /department/?</span></span> <span data-ttu-id="70986-129">tooindex hello értékének részleg, de adja meg a /department hello partíciós kulcs definíciójában.</span><span class="sxs-lookup"><span data-stu-id="70986-129">tooindex hello values under department, but specify /department as hello partition key definition.</span></span> <span data-ttu-id="70986-130">hello partíciókulcs implicit módon indexelt, és nem zárható ki indexelő indexelési házirend felülbírálások használatával.</span><span class="sxs-lookup"><span data-stu-id="70986-130">hello partition key is implicitly indexed and cannot be excluded from indexing using indexing policy overrides.</span></span>
> 
> 

<span data-ttu-id="70986-131">Nézzük milyen hatással van a partíciós kulcs hello választott hello az alkalmazás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="70986-131">Let's look at how hello choice of partition key impacts hello performance of your application.</span></span>

## <a name="working-with-hello-azure-cosmos-db-sdks"></a><span data-ttu-id="70986-132">Hello Azure Cosmos DB SDK-k használata</span><span class="sxs-lookup"><span data-stu-id="70986-132">Working with hello Azure Cosmos DB SDKs</span></span>
<span data-ttu-id="70986-133">Az Azure Cosmos DB támogatása az automatikus particionálási [REST API verziója 2015-12-16](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="70986-133">Azure Cosmos DB added support for automatic partitioning with [REST API version 2015-12-16](/rest/api/documentdb/).</span></span> <span data-ttu-id="70986-134">Rendelés particionálva toocreate tárolókban, le kell töltenie 1.6.0 SDK-verzió vagy újabb hello valamelyik támogatott SDK-platform (.NET, Node.js, Java, Python, MongoDB).</span><span class="sxs-lookup"><span data-stu-id="70986-134">In order toocreate partitioned containers, you must download SDK versions 1.6.0 or newer in one of hello supported SDK platforms (.NET, Node.js, Java, Python, MongoDB).</span></span> 

### <a name="creating-containers"></a><span data-ttu-id="70986-135">Tárolók létrehozása</span><span class="sxs-lookup"><span data-stu-id="70986-135">Creating containers</span></span>
<span data-ttu-id="70986-136">hello következő példa bemutatja a .NET-részlet toocreate egy tároló toostore eszköz telemetriai adatokat a 20 000 kérelemegység / s átviteli.</span><span class="sxs-lookup"><span data-stu-id="70986-136">hello following sample shows a .NET snippet toocreate a container toostore device telemetry data of 20,000 request units per second of throughput.</span></span> <span data-ttu-id="70986-137">hello SDK egy hello OfferThroughput értéket (amelyek viszont beállítja hello `x-ms-offer-throughput` hello REST API-t a kérelem fejlécében).</span><span class="sxs-lookup"><span data-stu-id="70986-137">hello SDK sets hello OfferThroughput value (which in turn sets hello `x-ms-offer-throughput` request header in hello REST API).</span></span> <span data-ttu-id="70986-138">Itt hivatott hello `/deviceId` hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="70986-138">Here we set hello `/deviceId` as hello partition key.</span></span> <span data-ttu-id="70986-139">partíciókulcs hello választott együtt hello részeinek hello tároló metaadatait, például a nevét, és az indexelési házirendet a rendszer menti.</span><span class="sxs-lookup"><span data-stu-id="70986-139">hello choice of partition key is saved along with hello rest of hello container metadata like name and indexing policy.</span></span>

<span data-ttu-id="70986-140">Az ebben a példában azt kivételezett `deviceId` tudjuk, hogy (a) óta eszközök nagy számú, mivel a írások terjeszthető partíciók között egyenlően, és így velünk tooscale hello adatbázis tooingest nagy mennyiségű adatok és (b) számos hello kérések, például hello legújabb olvasási beolvasása eszköz hatókörön belüli tooa egyetlen deviceId, és egyetlen partícióra lekérhetők.</span><span class="sxs-lookup"><span data-stu-id="70986-140">For this sample, we picked `deviceId` since we know that (a) since there are a large number of devices, writes can be distributed across partitions evenly and allowing us tooscale hello database tooingest massive volumes of data and (b) many of hello requests like fetching hello latest reading for a device are scoped tooa single deviceId and can be retrieved from a single partition.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here hello property deviceId will be used as hello partition key too
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

<span data-ttu-id="70986-141">Ez a módszer lehetővé teszi a REST API hívása tooCosmos DB, és hello szolgáltatás kiépíti a hello kért átviteli sebesség alapján partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="70986-141">This method makes a REST API call tooCosmos DB, and hello service will provision a number of partitions based on hello requested throughput.</span></span> <span data-ttu-id="70986-142">A teljesítmény kell fejleszteni a tároló hello átviteli tudja módosítani.</span><span class="sxs-lookup"><span data-stu-id="70986-142">You can change hello throughput of a container as your performance needs evolve.</span></span> 

### <a name="reading-and-writing-items"></a><span data-ttu-id="70986-143">Olvasott és írt elemek</span><span class="sxs-lookup"><span data-stu-id="70986-143">Reading and writing items</span></span>
<span data-ttu-id="70986-144">Most tegyük beszúrni Cosmos DB adatokat.</span><span class="sxs-lookup"><span data-stu-id="70986-144">Now, let's insert data into Cosmos DB.</span></span> <span data-ttu-id="70986-145">Íme egy minta tartalmazó osztály egy eszköz olvasásra, és egy hívás tooCreateDocumentAsync tooinsert egy új eszközt egy tárolóba olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="70986-145">Here's a sample class containing a device reading, and a call tooCreateDocumentAsync tooinsert a new device reading into a container.</span></span> <span data-ttu-id="70986-146">Íme egy kihasználva hello DocumentDB API:</span><span class="sxs-lookup"><span data-stu-id="70986-146">This is an example leveraging hello DocumentDB API:</span></span>

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

// Create a document. Here hello partition key is extracted as "XMS-0001" based on hello collection definition
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

<span data-ttu-id="70986-147">Most hello elem partíciókulcs és azonosító által olvasását, a frissítést, és utolsó lépésként, törölje azt partíciókulcs és azonosító. Vegye figyelembe, hogy a hello olvasások PartitionKey értéket tartalmazza (megfelelő toohello `x-ms-documentdb-partitionkey` hello REST API-t a kérelem fejlécében).</span><span class="sxs-lookup"><span data-stu-id="70986-147">Let's read hello item by its partition key and id, update it, and then as a final step, delete it by partition key and id. Note that hello reads include a PartitionKey value (corresponding toohello `x-ms-documentdb-partitionkey` request header in hello REST API).</span></span>

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update hello document. Partition key is not required, again extracted from hello document
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

### <a name="querying-partitioned-containers"></a><span data-ttu-id="70986-148">A particionált tárolók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="70986-148">Querying partitioned containers</span></span>
<span data-ttu-id="70986-149">Amikor a adatait particionált-tárolókban, Cosmos DB automatikusan útvonalak hello lekérdezési toohello partíciók megfelelő toohello partíció megadott kulcsértékek hello szűrő (ha vannak ilyenek).</span><span class="sxs-lookup"><span data-stu-id="70986-149">When you query data in partitioned containers, Cosmos DB automatically routes hello query toohello partitions corresponding toohello partition key values specified in hello filter (if there are any).</span></span> <span data-ttu-id="70986-150">Például egy, a lekérdezés nem útválasztásos toojust hello partíció tartalmazó hello partíciós kulcs "XMS-0001".</span><span class="sxs-lookup"><span data-stu-id="70986-150">For example, this query is routed toojust hello partition containing hello partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="70986-151">hello következő lekérdezés nincs szűrő hello partíciókulcs (DeviceId) és tooall partíciók hol végre hello partíció index alapján van rendezve.</span><span class="sxs-lookup"><span data-stu-id="70986-151">hello following query does not have a filter on hello partition key (DeviceId) and is fanned out tooall partitions where it is executed against hello partition's index.</span></span> <span data-ttu-id="70986-152">Vegye figyelembe, hogy rendelkezik-e toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` a hello REST API-t) toohave hello SDK tooexecute közötti partíciók lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="70986-152">Note that you have toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in hello REST API) toohave hello SDK tooexecute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

<span data-ttu-id="70986-153">Támogatja a cosmos DB [aggregátumfüggvények](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` és `AVG` over particionálva tárolók indítása az SDK-k 1.12.0 és újabb SQL használatával.</span><span class="sxs-lookup"><span data-stu-id="70986-153">Cosmos DB supports [aggregate functions](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` and `AVG` over partitioned containers using SQL starting with SDKs 1.12.0 and above.</span></span> <span data-ttu-id="70986-154">Lekérdezések tartalmaznia kell egy egyetlen összesítő operátor és hello projekcióban szereplő egyetlen értéket kell adni.</span><span class="sxs-lookup"><span data-stu-id="70986-154">Queries must include a single aggregate operator, and must include a single value in hello projection.</span></span>

### <a name="parallel-query-execution"></a><span data-ttu-id="70986-155">Párhuzamos lekérdezés-végrehajtás</span><span class="sxs-lookup"><span data-stu-id="70986-155">Parallel query execution</span></span>
<span data-ttu-id="70986-156">hello Cosmos DB SDK-k 1.9.0 és támogatási fent párhuzamos lekérdezés végrehajtási beállítások, melyek lehetővé teszik tooperform kis késleltetésű lekérdezi a particionált gyűjtemények szemben még akkor is, ha szükségük van tootouch partíciók nagy számú.</span><span class="sxs-lookup"><span data-stu-id="70986-156">hello Cosmos DB SDKs 1.9.0 and above support parallel query execution options, which allow you tooperform low latency queries against partitioned collections, even when they need tootouch a large number of partitions.</span></span> <span data-ttu-id="70986-157">Például a következő lekérdezés hello partíciók nem konfigurált toorun párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="70986-157">For example, hello following query is configured toorun in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="70986-158">A következő paraméterek hello hangolása kezelheti párhuzamos lekérdezés-végrehajtás:</span><span class="sxs-lookup"><span data-stu-id="70986-158">You can manage parallel query execution by tuning hello following parameters:</span></span>

* <span data-ttu-id="70986-159">Úgy, hogy `MaxDegreeOfParallelism`, azaz hello egyidejű hálózati kapcsolatok toohello tároló partíciók száma párhuzamos hello fokú szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="70986-159">By setting `MaxDegreeOfParallelism`, you can control hello degree of parallelism i.e., hello maximum number of simultaneous network connections toohello container's partitions.</span></span> <span data-ttu-id="70986-160">Ha ez túl-1, párhuzamossági hello fokát hello SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="70986-160">If you set this too-1, hello degree of parallelism is managed by hello SDK.</span></span> <span data-ttu-id="70986-161">Ha hello `MaxDegreeOfParallelism` értéke nincs megadva, illetve too0, amely hello alapértelmezett érték, egy egyetlen hálózati kapcsolat toohello tároló partíciók lesz.</span><span class="sxs-lookup"><span data-stu-id="70986-161">If hello `MaxDegreeOfParallelism` is not specified or set too0, which is hello default value, there will be a single network connection toohello container's partitions.</span></span>
* <span data-ttu-id="70986-162">Úgy, hogy `MaxBufferedItemCount`, akkor is kompromisszumot lekérdezés-késleltetés és ügyféloldali memóriahasználata.</span><span class="sxs-lookup"><span data-stu-id="70986-162">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="70986-163">Ha kihagyja ezt a paramétert, vagy állítsa 1 túl, hello száma párhuzamos lekérdezés-végrehajtás során pufferelt elemek hello SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="70986-163">If you omit this parameter or set this too-1, hello number of items buffered during parallel query execution is managed by hello SDK.</span></span>

<span data-ttu-id="70986-164">Hello megadott hello gyűjtemény olyan állapotban, a párhuzamos lekérdezések lesz az eredményeket hello ugyanaz, mint soros végrehajtási sorrendben.</span><span class="sxs-lookup"><span data-stu-id="70986-164">Given hello same state of hello collection, a parallel query will return results in hello same order as in serial execution.</span></span> <span data-ttu-id="70986-165">A kereszt-partíció lekérdezést, amely tartalmazza a rendezés (ORDER BY és/vagy felső) végrehajtásakor hello Azure Cosmos DB SDK problémák hello párhuzamos lekérdezés partíciók és összevonása részben rendezett eredményez hello ügyfél oldalán tooproduce globálisan rendezett az eredmények között.</span><span class="sxs-lookup"><span data-stu-id="70986-165">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), hello Azure Cosmos DB SDK issues hello query in parallel across partitions and merges partially sorted results in hello client side tooproduce globally ordered results.</span></span>

### <a name="executing-stored-procedures"></a><span data-ttu-id="70986-166">Tárolt eljárások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="70986-166">Executing stored procedures</span></span>
<span data-ttu-id="70986-167">Is végre lehet hajtani a dokumentumokon végzett atomi tranzakciók pl. hello ugyanazon Eszközazonosítót, ha éppen karbantartása összesítések, vagy hello csak egy elemet az eszközök aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="70986-167">You can also execute atomic transactions against documents with hello same device ID, e.g. if you're maintaining aggregates or hello latest state of a device in a single item.</span></span> 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
<span data-ttu-id="70986-168">A következő szakaszban hello úgy tekintünk, hogyan léphet toopartitioned tárolók egypartíciós tárolók.</span><span class="sxs-lookup"><span data-stu-id="70986-168">In hello next section, we look at how you can move toopartitioned containers from single-partition containers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="70986-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70986-169">Next steps</span></span>
<span data-ttu-id="70986-170">Ez a cikk azt megadott hogyan rendelkező Azure Cosmos DB tárolók a particionálás toowork hello DocumentDB API áttekintése.</span><span class="sxs-lookup"><span data-stu-id="70986-170">In this article, we provided an overview of how toowork with partitioning of Azure Cosmos DB containers with hello DocumentDB API.</span></span> <span data-ttu-id="70986-171">Lásd még: [particionálás és horizontális skálázás](../cosmos-db/partition-data.md) fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintését.</span><span class="sxs-lookup"><span data-stu-id="70986-171">Also see [partitioning and horizontal scaling](../cosmos-db/partition-data.md) for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="70986-172">Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték.</span><span class="sxs-lookup"><span data-stu-id="70986-172">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="70986-173">Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.</span><span class="sxs-lookup"><span data-stu-id="70986-173">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="70986-174">Ismerkedés a hello kódolási [SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API-n](/rest/api/documentdb/)</span><span class="sxs-lookup"><span data-stu-id="70986-174">Get started coding with hello [SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/)</span></span>
* <span data-ttu-id="70986-175">További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="70986-175">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>

