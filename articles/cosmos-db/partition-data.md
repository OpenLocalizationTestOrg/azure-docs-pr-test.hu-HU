---
title: "Particionálás és Azure Cosmos DB horizontális skálázás |} Microsoft Docs"
description: "Ismerje meg, hogyan particionálási működését Azure Cosmos DB, hogyan lehet konfigurálni a particionálás és kulcsok partícióazonosító és hogyan válassza ki a megfelelő partíciókulcs az alkalmazáshoz."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2d2847276e553d7511241ff323c3e00aad8e5c9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-partition-and-scale-in-azure-cosmos-db"></a><span data-ttu-id="7a585-103">Partíció és a skála Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7a585-103">How to partition and scale in Azure Cosmos DB</span></span>

<span data-ttu-id="7a585-104">[A Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) egy globális elosztott, több modellre adatbázis szolgáltatás célja, hogy gyors és kiszámítható teljesítmény és méretezhetőség zökkenőmentesen együtt az alkalmazás eléréséhez, növekedésével azt.</span><span class="sxs-lookup"><span data-stu-id="7a585-104">[Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a global distributed, multi-model database service designed to help you achieve fast, predictable performance and scale seamlessly along with your application as it grows.</span></span> <span data-ttu-id="7a585-105">Ez a cikk áttekintése Azure Cosmos DB az adatok modellek hogyan particionálási működik, és ismerteti, hogyan konfigurálhatja a hatékony méretezést az alkalmazások Azure Cosmos DB tárolók.</span><span class="sxs-lookup"><span data-stu-id="7a585-105">This article provides an overview of how partitioning works for all the data models in Azure Cosmos DB, and describes how you can configure Azure Cosmos DB containers to effectively scale your applications.</span></span>

<span data-ttu-id="7a585-106">Particionálás és partíciókulcsok is tartoznak az Azure-ban a Scott Hanselman és Azure Cosmos DB egyszerű mérnöki Manager, Shireesh Thota videó péntekig.</span><span class="sxs-lookup"><span data-stu-id="7a585-106">Partitioning and partition keys are also covered in this Azure Friday video with Scott Hanselman and Azure Cosmos DB Principal Engineering Manager, Shireesh Thota.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a><span data-ttu-id="7a585-107">Az Azure Cosmos DB particionálás</span><span class="sxs-lookup"><span data-stu-id="7a585-107">Partitioning in Azure Cosmos DB</span></span>
<span data-ttu-id="7a585-108">Az Azure Cosmos Adatbázisba tárolására és az ahhoz az ezredmásodperces válaszidők bármilyen léptékben séma nélküli adatait.</span><span class="sxs-lookup"><span data-stu-id="7a585-108">In Azure Cosmos DB, you can store and query schema-less data with order-of-millisecond response times at any scale.</span></span> <span data-ttu-id="7a585-109">Adattárolás nevű nyújt tárolók cosmos DB **gyűjtemények (a dokumentum), a diagramok és a táblázatok**.</span><span class="sxs-lookup"><span data-stu-id="7a585-109">Cosmos DB provides containers for storing data called **collections (for document), graphs, or tables**.</span></span> <span data-ttu-id="7a585-110">Tárolók logikai erőforrások, és egy vagy több fizikai partíciók, sem kiszolgálók is kiterjedhetnek.</span><span class="sxs-lookup"><span data-stu-id="7a585-110">Containers are logical resources and can span one or more physical partitions or servers.</span></span> <span data-ttu-id="7a585-111">A partíciók számának Cosmos DB a tárhely méretét és a létesített átviteli sebesség a tároló alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="7a585-111">The number of partitions is determined by Cosmos DB based on the storage size and the provisioned throughput of the container.</span></span> <span data-ttu-id="7a585-112">Minden partíció Cosmos DB SSD-biztonsági tárolási társítva a rögzített méretű rendelkezik, és a magas rendelkezésre állású replikálódik.</span><span class="sxs-lookup"><span data-stu-id="7a585-112">Every partition in Cosmos DB has a fixed amount of SSD-backed storage associated with it, and is replicated for high availability.</span></span> <span data-ttu-id="7a585-113">Partíció felügyeleti teljes mértékben felügyelt Azure Cosmos DB, és komplex kódot írnia, vagy a partíciók kezelésére nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="7a585-113">Partition management is fully managed by Azure Cosmos DB, and you do not have to write complex code or manage your partitions.</span></span> <span data-ttu-id="7a585-114">Cosmos DB tárolók olyan tárolási és átviteli korlátlan.</span><span class="sxs-lookup"><span data-stu-id="7a585-114">Cosmos DB containers are unlimited in terms of storage and throughput.</span></span> 

![vízszintes](./media/introduction/azure-cosmos-db-partitioning.png) 

<span data-ttu-id="7a585-116">Particionálás átlátható az alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="7a585-116">Partitioning is transparent to your application.</span></span> <span data-ttu-id="7a585-117">Cosmos DB támogatja a gyors olvasást és írási műveleteket, lekérdezések, tranzakciós logikát, konzisztenciaszintek, és minden részletre kiterjedő hozzáférés-vezérlés keresztül módszerek/API-k egyetlen tároló-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7a585-117">Cosmos DB supports fast reads and writes, queries, transactional logic, consistency levels, and fine-grained access control via methods/APIs to a single container resource.</span></span> <span data-ttu-id="7a585-118">A szolgáltatás kezeli a terjesztés adatokat partíciókat és a megfelelő partíció útválasztási lekérdezési kérelmek.</span><span class="sxs-lookup"><span data-stu-id="7a585-118">The service handles distributing data across partitions and routing query requests to the right partition.</span></span> 

<span data-ttu-id="7a585-119">Particionálás működése</span><span class="sxs-lookup"><span data-stu-id="7a585-119">How does partitioning work?</span></span> <span data-ttu-id="7a585-120">Minden elem rendelkeznie kell egy partíció és sor kulcsot, amely egyedileg azonosítja.</span><span class="sxs-lookup"><span data-stu-id="7a585-120">Each item must have a partition key and a row key, which uniquely identify it.</span></span> <span data-ttu-id="7a585-121">A partíciós kulcs úgy működik, mint az adatok logikai partíció, és a természetes határ Cosmos DB biztosít osztja el az adatok között partíciókat.</span><span class="sxs-lookup"><span data-stu-id="7a585-121">Your partition key acts as a logical partition for your data, and provides Cosmos DB with a natural boundary for distributing data across partitions.</span></span> <span data-ttu-id="7a585-122">Röviden itt található particionálási hogyan működik az Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="7a585-122">In brief, here is how partitioning works in Azure Cosmos DB:</span></span>

* <span data-ttu-id="7a585-123">A Cosmos DB tárolóhoz kiépítése `T` kérelmek/s átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="7a585-123">You provision a Cosmos DB container with `T` requests/s throughput</span></span>
* <span data-ttu-id="7a585-124">A háttérben Cosmos DB kiépítését kiszolgálásához szükséges partíciókat `T` kérelmek/s.</span><span class="sxs-lookup"><span data-stu-id="7a585-124">Behind the scenes, Cosmos DB provisions partitions needed to serve `T` requests/s.</span></span> <span data-ttu-id="7a585-125">Ha `T` magasabb, mint a maximális átviteli sebesség partíciónként `t`, majd Cosmos DB rendelkezések `N`  =  `T/t` partíciók</span><span class="sxs-lookup"><span data-stu-id="7a585-125">If `T` is higher than the maximum throughput per partition `t`, then Cosmos DB provisions `N` = `T/t` partitions</span></span>
* <span data-ttu-id="7a585-126">Cosmos DB foglal le a kulcsfontosságú terület partíció kulcs kivonatok egyenlő vízszintes a `N` partíciókat.</span><span class="sxs-lookup"><span data-stu-id="7a585-126">Cosmos DB allocates the key space of partition key hashes evenly across the `N` partitions.</span></span> <span data-ttu-id="7a585-127">Igen minden partíció (fizikai partíció) tároló 1-N partíciókulcs-értékek (logikai partíciót)</span><span class="sxs-lookup"><span data-stu-id="7a585-127">So, each partition (physical partition) hosts 1-N partition key values (logical partitions)</span></span>
* <span data-ttu-id="7a585-128">Ha egy fizikai partíció `p` eléri a tárolási korlátját, Cosmos DB zökkenőmentesen felosztja a `p` két új partíciókra `p1` és `p2` , majd az egyes partíciók körülbelül fél kulcsainak megfelelő értékeket továbbítja.</span><span class="sxs-lookup"><span data-stu-id="7a585-128">When a physical partition `p` reaches its storage limit, Cosmos DB seamlessly splits `p` into two new partitions `p1` and `p2` and distributes values corresponding to roughly half the keys to each of the partitions.</span></span> <span data-ttu-id="7a585-129">Ez a művelet vágási nem látható, hogy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7a585-129">This split operation is invisible to your application.</span></span>
* <span data-ttu-id="7a585-130">Ha ehhez hasonlóan a nagyobb átviteli sebesség kiépítése `t*N` átviteli sebességet Cosmos DB felosztja egy vagy több, a nagyobb átviteli sebesség támogatásához a partíciók</span><span class="sxs-lookup"><span data-stu-id="7a585-130">Similarly, when you provision throughput higher than `t*N` throughput, Cosmos DB splits one or more of your partitions to support the higher throughput</span></span>

<span data-ttu-id="7a585-131">A partíciós kulcsok szemantikája némileg eltérő minden API szemantikáját megfelelően, az alábbi táblázatban látható módon:</span><span class="sxs-lookup"><span data-stu-id="7a585-131">The semantics for partition keys are slightly different to match the semantics of each API, as shown in the following table:</span></span>

| <span data-ttu-id="7a585-132">API</span><span class="sxs-lookup"><span data-stu-id="7a585-132">API</span></span> | <span data-ttu-id="7a585-133">Partíciókulcs</span><span class="sxs-lookup"><span data-stu-id="7a585-133">Partition Key</span></span> | <span data-ttu-id="7a585-134">Sorkulcsa</span><span class="sxs-lookup"><span data-stu-id="7a585-134">Row Key</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7a585-135">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="7a585-135">DocumentDB</span></span> | <span data-ttu-id="7a585-136">egyéni partíciós kulcs elérési útja</span><span class="sxs-lookup"><span data-stu-id="7a585-136">custom partition key path</span></span> | <span data-ttu-id="7a585-137">rögzített`id`</span><span class="sxs-lookup"><span data-stu-id="7a585-137">fixed `id`</span></span> | 
| <span data-ttu-id="7a585-138">MongoDB</span><span class="sxs-lookup"><span data-stu-id="7a585-138">MongoDB</span></span> | <span data-ttu-id="7a585-139">egyéni shard kulcs</span><span class="sxs-lookup"><span data-stu-id="7a585-139">custom shard key</span></span>  | <span data-ttu-id="7a585-140">rögzített`_id`</span><span class="sxs-lookup"><span data-stu-id="7a585-140">fixed `_id`</span></span> | 
| <span data-ttu-id="7a585-141">Graph</span><span class="sxs-lookup"><span data-stu-id="7a585-141">Graph</span></span> | <span data-ttu-id="7a585-142">egyéni partíció kulcstulajdonság</span><span class="sxs-lookup"><span data-stu-id="7a585-142">custom partition key property</span></span> | <span data-ttu-id="7a585-143">rögzített`id`</span><span class="sxs-lookup"><span data-stu-id="7a585-143">fixed `id`</span></span> | 
| <span data-ttu-id="7a585-144">Tábla</span><span class="sxs-lookup"><span data-stu-id="7a585-144">Table</span></span> | <span data-ttu-id="7a585-145">rögzített`PartitionKey`</span><span class="sxs-lookup"><span data-stu-id="7a585-145">fixed `PartitionKey`</span></span> | <span data-ttu-id="7a585-146">rögzített`RowKey`</span><span class="sxs-lookup"><span data-stu-id="7a585-146">fixed `RowKey`</span></span> | 

<span data-ttu-id="7a585-147">Cosmos DB használ, a particionálás kivonat-alapú.</span><span class="sxs-lookup"><span data-stu-id="7a585-147">Cosmos DB uses hash-based partitioning.</span></span> <span data-ttu-id="7a585-148">Egy cikk írásakor Cosmos DB csak a partíciós kulcs értékét, és a kivonatolt eredmény segítségével határozhatja meg, melyik partíció-elem tárolására.</span><span class="sxs-lookup"><span data-stu-id="7a585-148">When you write an item, Cosmos DB hashes the partition key value and use the hashed result to determine which partition to store the item in.</span></span> <span data-ttu-id="7a585-149">Cosmos DB ugyanazon fizikai partícióján azonos partíciókulcsú tárolja az összes elemet.</span><span class="sxs-lookup"><span data-stu-id="7a585-149">Cosmos DB stores all items with the same partition key in the same physical partition.</span></span> <span data-ttu-id="7a585-150">A partíciós kulcs választott egy fontos döntés, hogy módosítania kell a tervezés során nem.</span><span class="sxs-lookup"><span data-stu-id="7a585-150">The choice of the partition key is an important decision that you have to make at design time.</span></span> <span data-ttu-id="7a585-151">Válasszon ki egy tulajdonság neve, amely számos különböző értékeket tartalmaz, és nem is memóriahozzáférési mintáitól.</span><span class="sxs-lookup"><span data-stu-id="7a585-151">You must pick a property name that has a wide range of values and has even access patterns.</span></span>

> [!NOTE]
> <span data-ttu-id="7a585-152">Ajánlott eljárás, számos különböző értékeket (100 db-1000 egység minimális) tartalmazó partíció kulcsa.</span><span class="sxs-lookup"><span data-stu-id="7a585-152">It is a best practice to have a partition key with many distinct values (100s-1000s at a minimum).</span></span>
>

<span data-ttu-id="7a585-153">Az Azure Cosmos DB tárolók hozhatók létre "fixed" vagy "korlátlan."</span><span class="sxs-lookup"><span data-stu-id="7a585-153">Azure Cosmos DB containers can be created as "fixed" or "unlimited."</span></span> <span data-ttu-id="7a585-154">Rögzített méretű tárolók rendelkezik egy legfeljebb 10 GB és 10000 RU/s átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="7a585-154">Fixed-size containers have a maximum limit of 10 GB and 10,000 RU/s throughput.</span></span> <span data-ttu-id="7a585-155">Egyes API-k a rögzített méretű tárolók nem kell a partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="7a585-155">Some APIs allow the partition key to be omitted for fixed-size containers.</span></span> <span data-ttu-id="7a585-156">Egy tároló korlátlan mint létrehozásához meg kell adnia minimális átviteli sebességgel 2500 RU/mp.</span><span class="sxs-lookup"><span data-stu-id="7a585-156">To create a container as unlimited, you must specify a minimum throughput of 2500 RU/s.</span></span>

## <a name="partitioning-and-provisioned-throughput"></a><span data-ttu-id="7a585-157">Particionálási és kiosztott átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="7a585-157">Partitioning and provisioned throughput</span></span>
<span data-ttu-id="7a585-158">Kiszámítható teljesítmény cosmos DB készült.</span><span class="sxs-lookup"><span data-stu-id="7a585-158">Cosmos DB is designed for predictable performance.</span></span> <span data-ttu-id="7a585-159">Amikor létrehoz egy tárolót, a teljesítményt a lefoglalni  **[egységek kérelem](request-units.md) (RU) percenkénti RU egy potenciális bővítmény másodpercenként**.</span><span class="sxs-lookup"><span data-stu-id="7a585-159">When you create a container, you reserve throughput in terms of **[request units](request-units.md) (RU) per second with a potential add-on for RU per minute**.</span></span> <span data-ttu-id="7a585-160">Minden egyes kérelem hozzá van rendelve egy kérelem egység kell fizetni, például a Processzor, memória és I/O művelethez rendszererőforrások mennyiségét arányos.</span><span class="sxs-lookup"><span data-stu-id="7a585-160">Each request is assigned a request unit charge that is proportionate to the amount of system resources like CPU, Memory, and IO consumed by the operation.</span></span> <span data-ttu-id="7a585-161">Egy 1 KB-os dokumentum munkamenet-konzisztencia olvasási egy kérelem egységet használ fel.</span><span class="sxs-lookup"><span data-stu-id="7a585-161">A read of a 1-KB document with Session consistency consumes one request unit.</span></span> <span data-ttu-id="7a585-162">Egy olvasási 1 RU függetlenül tárolt elemek számát vagy egy időben futó egyidejű kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="7a585-162">A read is 1 RU regardless of the number of items stored or the number of concurrent requests running at the same time.</span></span> <span data-ttu-id="7a585-163">Nagyobb elemek szükség magasabb kérelemegység méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="7a585-163">Larger items require higher request units depending on the size.</span></span> <span data-ttu-id="7a585-164">Ha ismeri az entitások és az alkalmazás támogatásához szükséges olvasások száma, az alkalmazás csomagazonosítóját kell olvasni a szükséges átviteli pontos mennyisége oszthat meg.</span><span class="sxs-lookup"><span data-stu-id="7a585-164">If you know the size of your entities and the number of reads you need to support for your application, you can provision the exact amount of throughput required for your application's read needs.</span></span> 

> [!NOTE]
> <span data-ttu-id="7a585-165">A teljes átviteli képessége – a tároló elérése érdekében ki kell választania egy partíciós kulcs, amely lehetővé teszi a segítségével egyenlően osztható el a kérelmek néhány különböző partíciókulcs-értékek között.</span><span class="sxs-lookup"><span data-stu-id="7a585-165">To achieve the full throughput of the container, you must choose a partition key that allows you to evenly distribute requests among some distinct partition key values.</span></span>
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-the-azure-cosmos-db-apis"></a><span data-ttu-id="7a585-166">Az Azure Cosmos DB API-k használata</span><span class="sxs-lookup"><span data-stu-id="7a585-166">Working with the Azure Cosmos DB APIs</span></span>
<span data-ttu-id="7a585-167">Az Azure portálon vagy az Azure CLI segítségével tárolók létrehozása, és bármikor skálázni őket.</span><span class="sxs-lookup"><span data-stu-id="7a585-167">You can use the Azure portal or Azure CLI to create containers and scale them at any time.</span></span> <span data-ttu-id="7a585-168">Ez a szakasz bemutatja, hogyan tárolók létrehozása, és adja meg az átviteli sebesség és a partíciós kulcs definíciójában minden támogatott API-k.</span><span class="sxs-lookup"><span data-stu-id="7a585-168">This section shows how to create containers and specify the throughput and partition key definition in each of the supported APIs.</span></span>

### <a name="documentdb-api"></a><span data-ttu-id="7a585-169">DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="7a585-169">DocumentDB API</span></span>
<span data-ttu-id="7a585-170">A következő példa bemutatja, hogyan hozhat létre a DocumentDB API-val (gyűjtemény) tárolót.</span><span class="sxs-lookup"><span data-stu-id="7a585-170">The following sample shows how to create a container (collection) using the DocumentDB API.</span></span> <span data-ttu-id="7a585-171">További részletek találhatók [DocumentDB API-val particionálás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="7a585-171">You can find more details in [Partitioning with DocumentDB API](partition-data.md).</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

<span data-ttu-id="7a585-172">Egy elem (dokumentumok) segítségével elolvashatja a `GET` metódust a REST API vagy használatával `ReadDocumentAsync` az SDK-k egyikén.</span><span class="sxs-lookup"><span data-stu-id="7a585-172">You can read an item (document) using the `GET` method in the REST API or using `ReadDocumentAsync` in one of the SDKs.</span></span>

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a><span data-ttu-id="7a585-173">MongoDB API</span><span class="sxs-lookup"><span data-stu-id="7a585-173">MongoDB API</span></span>
<span data-ttu-id="7a585-174">A MongoDB API-t a kedvenc eszköz, az illesztőprogram, vagy az SDK szilánkos gyűjtemény hozható létre.</span><span class="sxs-lookup"><span data-stu-id="7a585-174">With the MongoDB API, you can create a sharded collection through your favorite tool, driver, or SDK.</span></span> <span data-ttu-id="7a585-175">Ebben a példában a Mongo rendszerhéj használjuk a gyűjtemény létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7a585-175">In this example, we use the Mongo Shell for the collection creation.</span></span>

<span data-ttu-id="7a585-176">A Mongo rendszerhéj:</span><span class="sxs-lookup"><span data-stu-id="7a585-176">In the Mongo Shell:</span></span>

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
<span data-ttu-id="7a585-177">Eredmények:</span><span class="sxs-lookup"><span data-stu-id="7a585-177">Results:</span></span>

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a><span data-ttu-id="7a585-178">Tábla API</span><span class="sxs-lookup"><span data-stu-id="7a585-178">Table API</span></span>

<span data-ttu-id="7a585-179">A tábla API-t megadja a táblák adatátviteli sebességét az appSettings konfigurációs az alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="7a585-179">With the Table API, you specify the throughput for tables in the appSettings configuration for your application:</span></span>

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

<span data-ttu-id="7a585-180">Ezután hozzon létre egy táblát az Azure Table storage SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="7a585-180">Then you create a table using the Azure Table storage SDK.</span></span> <span data-ttu-id="7a585-181">A partíciós kulcs implicit módon jön létre a `PartitionKey` érték.</span><span class="sxs-lookup"><span data-stu-id="7a585-181">The partition key is implicitly created as the `PartitionKey` value.</span></span> 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

<span data-ttu-id="7a585-182">Az alábbi kódrészletben használatával egyetlen entitás le:</span><span class="sxs-lookup"><span data-stu-id="7a585-182">You can retrieve a single entity using the following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
<span data-ttu-id="7a585-183">Lásd: [tábla API-val fejlesztése](tutorial-develop-table-dotnet.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="7a585-183">See [Developing with the Table API](tutorial-develop-table-dotnet.md) for more details.</span></span>

### <a name="graph-api"></a><span data-ttu-id="7a585-184">Graph API</span><span class="sxs-lookup"><span data-stu-id="7a585-184">Graph API</span></span>

<span data-ttu-id="7a585-185">A Graph API-t kell használnia az Azure portálon vagy a CLI tárolók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7a585-185">With the Graph API, you must use the Azure portal or CLI to create containers.</span></span> <span data-ttu-id="7a585-186">Azt is megteheti mivel Azure Cosmos DB több modellre, segítségével egyik más modell létrehozása és a graph tároló méretezni.</span><span class="sxs-lookup"><span data-stu-id="7a585-186">Alternatively, since Azure Cosmos DB is multi-model, you can use one of the other models to create and scale your graph container.</span></span>

<span data-ttu-id="7a585-187">Bármely csúcspont vagy edge Gremlin a partíciókulcs és azonosító használatával érheti el.</span><span class="sxs-lookup"><span data-stu-id="7a585-187">You can read any vertex or edge using the partition key and id in Gremlin.</span></span> <span data-ttu-id="7a585-188">Például a régióban ("USA) a partíciós kulcs, és a"Seattle"sor kulcsként van grafikon, is megtalálhatja a következő szintaxis használatával csúcspont:</span><span class="sxs-lookup"><span data-stu-id="7a585-188">For example, for a graph with region ("USA") as the partition key, and "Seattle" as the row key, you can find a vertex using the following syntax:</span></span>

```
g.V(['USA', 'Seattle'])
```

<span data-ttu-id="7a585-189">Azonos olvasáskor, melyeket referenciaként használhat, a partíciós kulcs és a sorkulcs él.</span><span class="sxs-lookup"><span data-stu-id="7a585-189">Same with edges, you can reference an edge using the partition key and row key.</span></span>

```
g.E(['USA', 'I5'])
```

<span data-ttu-id="7a585-190">Lásd: [Cosmos DB Gremlin támogatása](gremlin-support.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="7a585-190">See [Gremlin support for Cosmos DB](gremlin-support.md) for more details.</span></span>


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a><span data-ttu-id="7a585-191">A particionálás tervezése</span><span class="sxs-lookup"><span data-stu-id="7a585-191">Designing for partitioning</span></span>
<span data-ttu-id="7a585-192">Hatékony méretezést Azure Cosmos DB, a tároló létrehozásakor válassza ki a remek partíciókulcs kell.</span><span class="sxs-lookup"><span data-stu-id="7a585-192">To scale effectively with Azure Cosmos DB, you need to pick a good partition key when you create your container.</span></span> <span data-ttu-id="7a585-193">Nincsenek a partíciós kulcs két fő szempontjait:</span><span class="sxs-lookup"><span data-stu-id="7a585-193">There are two key considerations for choosing a partition key:</span></span>

* <span data-ttu-id="7a585-194">**A lekérdezés és a tranzakciókért határ**: A partíciós kulcs választott kell terheléselosztást tranzakciók a követelménnyel szemben az entitások szét több partíciós kulcsok méretezhető megoldás biztosításához az engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="7a585-194">**Boundary for query and transactions**: Your choice of partition key should balance the need to enable the use of transactions against the requirement to distribute your entities across multiple partition keys to ensure a scalable solution.</span></span> <span data-ttu-id="7a585-195">Az elemek ugyanazzal a partíciókulccsal beállíthat egy rendkívüli, de ez korlátozottá teheti méretezhetőségét a megoldás.</span><span class="sxs-lookup"><span data-stu-id="7a585-195">At one extreme, you could set the same partition key for all your items, but this may limit the scalability of your solution.</span></span> <span data-ttu-id="7a585-196">Vagyis a rendelheti minden elem, amely magas szinten méretezhető lenne, de akadályozzák meg közötti dokumentum tranzakciók tárolt eljárások és eseményindítók használata az egyedi partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="7a585-196">At the other extreme, you could assign a unique partition key for each item, which would be highly scalable but would prevent you from using cross document transactions via stored procedures and triggers.</span></span> <span data-ttu-id="7a585-197">Az ideális partíciókulcs az egyik, amely lehetővé teszi, hogy használni lehessen a hatékony lekérdezések és annak biztosítása érdekében, a megoldás méretezhető elegendő számossága áll.</span><span class="sxs-lookup"><span data-stu-id="7a585-197">An ideal partition key is one that enables you to use efficient queries and that has sufficient cardinality to ensure your solution is scalable.</span></span> 
* <span data-ttu-id="7a585-198">**Nincs tárterületi és teljesítménybeli szűk keresztmetszetek**: fontos, hogy egy tulajdonság, amely lehetővé teszi az írások legyen elosztva a különböző különböző értékeket.</span><span class="sxs-lookup"><span data-stu-id="7a585-198">**No storage and performance bottlenecks**: It is important to pick a property that allows writes to be distributed across various distinct values.</span></span> <span data-ttu-id="7a585-199">Ugyanazzal a partíciókulccsal kérelmek nem lehet hosszabb az átviteli sebessége egy olyan partíciót, és szabályozott.</span><span class="sxs-lookup"><span data-stu-id="7a585-199">Requests to the same partition key cannot exceed the throughput of a single partition, and are throttled.</span></span> <span data-ttu-id="7a585-200">Ezért fontos válassza ki a partíciós kulcs, amely nem eredményez "interaktív területek" az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="7a585-200">So it is important to pick a partition key that does not result in "hot spots" within your application.</span></span> <span data-ttu-id="7a585-201">Mivel egyetlen partíciókulcs tartozó összes adatot a partíción belül kell tárolni, is javasolt, amelyek nagy mennyiségű adatot ugyanolyan értékéhez partíciókulcsok elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7a585-201">Since all the data for a single partition key must be stored within a partition, it is also recommended to avoid partition keys that have high volumes of data for the same value.</span></span> 

<span data-ttu-id="7a585-202">Vizsgáljuk meg néhány valós forgatókönyv, és jó partíciós kulcsok minden egyes:</span><span class="sxs-lookup"><span data-stu-id="7a585-202">Let's look at a few real-world scenarios, and good partition keys for each:</span></span>
* <span data-ttu-id="7a585-203">A felhasználói profil backend megvalósításához, ha a felhasználói azonosító partíciós kulcs érdemes választani.</span><span class="sxs-lookup"><span data-stu-id="7a585-203">If you’re implementing a user profile backend, then the user ID is a good choice for partition key.</span></span>
* <span data-ttu-id="7a585-204">Ha például az eszköz állapotát az IoT-adatokat tárolja, áll jól funkcionálnak a partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="7a585-204">If you’re storing IoT data for example, device state, a device ID is a good choice for partition key.</span></span>
* <span data-ttu-id="7a585-205">Azure Cosmos DB-idősoros adatok naplózásának használata, az állomásnév vagy a folyamat azonosítója partíciós kulcs érdemes választani.</span><span class="sxs-lookup"><span data-stu-id="7a585-205">If you’re using Azure Cosmos DB for logging time-series data, then the hostname or process ID is a good choice for partition key.</span></span>
* <span data-ttu-id="7a585-206">Ha egy több-bérlős architektúrák, a bérlő azonosítója partíciós kulcs érdemes választani.</span><span class="sxs-lookup"><span data-stu-id="7a585-206">If you have a multi-tenant architecture, the tenant ID is a good choice for partition key.</span></span>

<span data-ttu-id="7a585-207">Egyes esetekben az IoT- és felhasználói profilok használatához a partíciókulcs lehet ugyanaz, mint a azonosítója (a dokumentum kulcs).</span><span class="sxs-lookup"><span data-stu-id="7a585-207">In some use cases like IoT and user profiles, the partition key might be the same as your id (document key).</span></span> <span data-ttu-id="7a585-208">Más esetekben az idő adatsor hasonlóan lehetséges, hogy a partíciós kulcs, amely eltér attól az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7a585-208">In others like the time series data, you might have a partition key that’s different than the id.</span></span>

### <a name="partitioning-and-loggingtime-series-data"></a><span data-ttu-id="7a585-209">A particionáló és naplózási/idősorozat adatok</span><span class="sxs-lookup"><span data-stu-id="7a585-209">Partitioning and logging/time-series data</span></span>
<span data-ttu-id="7a585-210">A gyakori alkalmazási esetei Cosmos-adatbázis egyik naplózási és telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="7a585-210">One of the common use cases of Cosmos DB is for logging and telemetry.</span></span> <span data-ttu-id="7a585-211">Fontos válasszon remek partíciókulcs, mivel az olvasási/írási hatalmas mennyiségű adatot kell.</span><span class="sxs-lookup"><span data-stu-id="7a585-211">It is important to pick a good partition key since you might need to read/write vast volumes of data.</span></span> <span data-ttu-id="7a585-212">A függ az olvasási és írási sebesség és lekérdezések futtatásához várt típusú.</span><span class="sxs-lookup"><span data-stu-id="7a585-212">The choice depends on your read and write rates and kinds of queries you expect to run.</span></span> <span data-ttu-id="7a585-213">Az alábbiakban néhány tipp az remek partíciókulcs kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="7a585-213">Here are some tips on how to choose a good partition key.</span></span>

* <span data-ttu-id="7a585-214">Ha a használati eset szerint kis mértékét írja az idő, és a tartományok időbélyegeket a lekérdezésre kell és a többi szűrőt, hosszú távon gyűlik az használata a Timestamp, például egy összesítő dátumánál, mert a partíciós kulcs egy jó módszer.</span><span class="sxs-lookup"><span data-stu-id="7a585-214">If your use case involves a small rate of writes accumulating over a long period of time, and need to query by ranges of timestamps and other filters, then using a rollup of the timestamp, for example,  date as a partition key is a good approach.</span></span> <span data-ttu-id="7a585-215">Ez lehetővé teszi a lekérdezés keresztül az adatok dátum az egyetlen partícióra.</span><span class="sxs-lookup"><span data-stu-id="7a585-215">This allows you to query over all the data for a date from a single partition.</span></span> 
* <span data-ttu-id="7a585-216">Ha a számítási feladatok gyakori, amely több előfordul, a partíciós kulcs nem alapuló timestamp, hogy Cosmos DB is szét írások egyenletesen különféle partíciók kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7a585-216">If your workload is written heavy, which is more common, you should use a partition key that’s not based on timestamp so that Cosmos DB can distribute writes evenly across various partitions.</span></span> <span data-ttu-id="7a585-217">Itt egy állomásnevet, Folyamatazonosító, Tevékenységazonosító vagy nagy számosságot egy másik tulajdonság akkor hasznos.</span><span class="sxs-lookup"><span data-stu-id="7a585-217">Here a hostname, process ID, activity ID, or another property with high cardinality is a good choice.</span></span> 
* <span data-ttu-id="7a585-218">Harmadik módszer egy hibrid egy ahol egy napi vagy havi több tároló van, és a partíciós kulcs például állomásnév részletes tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="7a585-218">A third approach is a hybrid one where you have multiple containers, one for each day/month and the partition key is a granular property like hostname.</span></span> <span data-ttu-id="7a585-219">Ennek a beállítható az időszak alapján különböző átviteli előnye van, például az alkalmazása tárolója az aktuális hónap ki van építve nagyobb átviteli sebességgel mert ez olvasási és írási, mivel az előző hónap alacsonyabb átviteli, mert csak olvasási műveletek kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="7a585-219">This has the benefit that you can set different throughput based on the time window, for example, the container for the current month is provisioned with higher throughput since it serves reads and writes, whereas previous months with lower throughput since they only serve reads.</span></span>

### <a name="partitioning-and-multi-tenancy"></a><span data-ttu-id="7a585-220">Particionálás és a több-bérlős</span><span class="sxs-lookup"><span data-stu-id="7a585-220">Partitioning and multi-tenancy</span></span>
<span data-ttu-id="7a585-221">Egy több-bérlős alkalmazás Cosmos DB használatával valósít meg, ha nincsenek két népszerű minták – bérlőnként egy partíciókulcsot, és bérlőnként több tároló.</span><span class="sxs-lookup"><span data-stu-id="7a585-221">If you are implementing a multi-tenant application using Cosmos DB, there are two popular patterns – one partition key per tenant, and one container per tenant.</span></span> <span data-ttu-id="7a585-222">Az alábbiakban és az egyes:</span><span class="sxs-lookup"><span data-stu-id="7a585-222">Here are the pros and cons for each:</span></span>

* <span data-ttu-id="7a585-223">Bérlőnként egy Partíciókulcsot: Ebben a modellben bérlők közös a elhelyezésű belül egyetlen tárolót.</span><span class="sxs-lookup"><span data-stu-id="7a585-223">One Partition Key per tenant: In this model, tenants are collocated within a single container.</span></span> <span data-ttu-id="7a585-224">De lekérdezések és egy egybérlős belül elemek beszúrása Indexalapú egyetlen partícióra.</span><span class="sxs-lookup"><span data-stu-id="7a585-224">But queries and inserts for items within a single tenant can be performed against a single partition.</span></span> <span data-ttu-id="7a585-225">Tranzakciós logika összes eleme belül a bérlők között is megvalósíthatja.</span><span class="sxs-lookup"><span data-stu-id="7a585-225">You can also implement transactional logic across all items within a tenant.</span></span> <span data-ttu-id="7a585-226">Több bérlő egy tároló megosztás, mivel tárolási és átviteli költségeket takaríthat készletezési erőforrásokat a bérlők számára belül egyetlen tárolót, nem pedig további belső magasságnak kiépítés az egyes bérlők számára.</span><span class="sxs-lookup"><span data-stu-id="7a585-226">Since multiple tenants share a container, you can save storage and throughput costs by pooling resources for tenants within a single container rather than provisioning extra headroom for each tenant.</span></span> <span data-ttu-id="7a585-227">A hátránya, hogy nem rendelkezik bérlőnként teljesítmény-elszigetelés érdekében.</span><span class="sxs-lookup"><span data-stu-id="7a585-227">The drawback is that you do not have performance isolation per tenant.</span></span> <span data-ttu-id="7a585-228">Teljesítmény/átviteli sebesség növekedése alkalmazni a teljes tárolóhoz vs célzott növeli a bérlők számára.</span><span class="sxs-lookup"><span data-stu-id="7a585-228">Performance/throughput increases apply to the entire container vs targeted increases for tenants.</span></span>
* <span data-ttu-id="7a585-229">Egy tároló bérlőnként: mindegyik bérlő saját tároló rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7a585-229">One Container per tenant: Each tenant has its own container.</span></span> <span data-ttu-id="7a585-230">Ebben a modellben bérlőnként teljesítmény foglalhat.</span><span class="sxs-lookup"><span data-stu-id="7a585-230">In this model, you can reserve performance per tenant.</span></span> <span data-ttu-id="7a585-231">Cosmos DB új szolgáltatáskiépítéssel árképzési modellt, költséghatékonyabb, több-bérlős alkalmazásokhoz a néhány bérlőkkel ebben a modellben.</span><span class="sxs-lookup"><span data-stu-id="7a585-231">With Cosmos DB's new provisioning pricing model, this model is more cost-effective for multi-tenant applications with a few tenants.</span></span>

<span data-ttu-id="7a585-232">Egy kombináció/rétegzett megközelítés, amely kis bérlők collocates és áttelepíti a nagyobb bérlők saját tárolót is használható.</span><span class="sxs-lookup"><span data-stu-id="7a585-232">You can also use a combination/tiered approach that collocates small tenants and migrates larger tenants to their own container.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a585-233">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a585-233">Next steps</span></span>
<span data-ttu-id="7a585-234">Ez a cikk azt előírt áttekintése fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintése.</span><span class="sxs-lookup"><span data-stu-id="7a585-234">In this article, we provided an overview for an overview of concepts and best practices for partitioning with any Azure Cosmos DB API.</span></span> 

* <span data-ttu-id="7a585-235">További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md)</span><span class="sxs-lookup"><span data-stu-id="7a585-235">Learn about [provisioned throughput in Azure Cosmos DB](request-units.md)</span></span>
* <span data-ttu-id="7a585-236">További tudnivalók [globális terjesztési az Azure Cosmos-Adatbázisba](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="7a585-236">Learn about [global distribution in Azure Cosmos DB](distribute-data-globally.md)</span></span>



