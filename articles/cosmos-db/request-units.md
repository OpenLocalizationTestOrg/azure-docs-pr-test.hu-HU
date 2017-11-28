---
title: "aaaRequest egységek és átviteli sebesség becslése - Azure Cosmos DB |} Microsoft Docs"
description: "További tudnivalók arról, hogyan toounderstand, adja meg, és az Azure Cosmos Adatbázisba kérelem egység követelményeinek becslése."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 13c4e7aeb6222fa14ef982e238716e15a0159fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-in-azure-cosmos-db"></a><span data-ttu-id="de613-103">Az Azure Cosmos DB egység kérése</span><span class="sxs-lookup"><span data-stu-id="de613-103">Request Units in Azure Cosmos DB</span></span>
<span data-ttu-id="de613-104">Most már hozzáférhető: Azure Cosmos DB [kérelem egység Számológép](https://www.documentdb.com/capacityplanner).</span><span class="sxs-lookup"><span data-stu-id="de613-104">Now available: Azure Cosmos DB [request unit calculator](https://www.documentdb.com/capacityplanner).</span></span> <span data-ttu-id="de613-105">További információ: [megbecsülheti, az átviteli sebesség kell](request-units.md#estimating-throughput-needs).</span><span class="sxs-lookup"><span data-stu-id="de613-105">Learn more in [Estimating your throughput needs](request-units.md#estimating-throughput-needs).</span></span>

![Átviteli sebesség Számológép][5]

## <a name="introduction"></a><span data-ttu-id="de613-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="de613-107">Introduction</span></span>
<span data-ttu-id="de613-108">[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Microsoft globálisan elosztott több modellre adatbázis.</span><span class="sxs-lookup"><span data-stu-id="de613-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is Microsoft's globally distributed multi-model database.</span></span> <span data-ttu-id="de613-109">Az Azure Cosmos DB akkor nem toorent olyan virtuális gépet, szoftver központi telepítése vagy adatbázisok figyelése.</span><span class="sxs-lookup"><span data-stu-id="de613-109">With Azure Cosmos DB, you don't have toorent virtual machines, deploy software, or monitor databases.</span></span> <span data-ttu-id="de613-110">Azure Cosmos DB üzemeltetésének és folyamatosan figyeli a Microsoft felső mérnökök toodeliver globális osztály rendelkezésre állását, teljesítményét és adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="de613-110">Azure Cosmos DB is operated and continuously monitored by Microsoft top engineers toodeliver world class availability, performance, and data protection.</span></span> <span data-ttu-id="de613-111">Az adatok az Ön által választott, API-k használatával végezheti el [a DocumentDB SQL](documentdb-sql-query.md) (dokumentumok) MongoDB (dokumentumok), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (kulcs-érték), és [Gremlin](https://tinkerpop.apache.org/gremlin.html) (diagramot)-e minden natív módon támogatottak.</span><span class="sxs-lookup"><span data-stu-id="de613-111">You can access your data using APIs of your choice, as [DocumentDB SQL](documentdb-sql-query.md) (document), MongoDB (document), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (key-value), and [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graph) are all natively supported.</span></span> <span data-ttu-id="de613-112">hello Azure Cosmos DB pénzneme hello kérelem egység (RU).</span><span class="sxs-lookup"><span data-stu-id="de613-112">hello currency of Azure Cosmos DB is hello Request Unit (RU).</span></span> <span data-ttu-id="de613-113">A RUs nem kell tooreserve olvasási/írási kapacitások vagy rendelkezés Processzor, memória és iops-érték.</span><span class="sxs-lookup"><span data-stu-id="de613-113">With RUs, you do not need tooreserve read/write capacities or provision CPU, Memory and IOPS.</span></span>

<span data-ttu-id="de613-114">Azure Cosmos-adatbázis API-k számos különböző közötti egyszerű olvasási műveletek támogat, és írja toocomplex graph lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="de613-114">Azure Cosmos DB supports a number of APIs with different operations ranging from simple reads and writes toocomplex graph queries.</span></span> <span data-ttu-id="de613-115">Mivel nem minden kérelemre egyenlő, hozzárendeli egy normalizált mennyisége **egységek kérelem** számítási szükséges tooserve hello kérelem hello mennyisége alapján.</span><span class="sxs-lookup"><span data-stu-id="de613-115">Since not all requests are equal, they are assigned a normalized quantity of **request units** based on hello amount of computation required tooserve hello request.</span></span> <span data-ttu-id="de613-116">hello száma kérelem művelet nem determinisztikus, és bármely Azure Cosmos DB egy válasz fejléce művelet által felhasznált kérelemegység hello száma követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="de613-116">hello number of request units for an operation is deterministic, and you can track hello number of request units consumed by any operation in Azure Cosmos DB via a response header.</span></span> 

<span data-ttu-id="de613-117">tooprovide kiszámítható teljesítmény, kell tooreserve átviteli egységekben 100 RU/másodperc.</span><span class="sxs-lookup"><span data-stu-id="de613-117">tooprovide predictable performance, you need tooreserve throughput in units of 100 RU/second.</span></span> 

<span data-ttu-id="de613-118">A cikk elolvasása után be fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="de613-118">After reading this article, you'll be able tooanswer hello following questions:</span></span>  

* <span data-ttu-id="de613-119">Mik azok a egységek kérése és díjak kérelem?</span><span class="sxs-lookup"><span data-stu-id="de613-119">What are request units and request charges?</span></span>
* <span data-ttu-id="de613-120">Hogyan adja meg a kérelem egység kapacitás gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="de613-120">How do I specify request unit capacity for a collection?</span></span>
* <span data-ttu-id="de613-121">Hogyan becsléséhez, az alkalmazás kérelem egység van szüksége?</span><span class="sxs-lookup"><span data-stu-id="de613-121">How do I estimate my application's request unit needs?</span></span>
* <span data-ttu-id="de613-122">Mi történik, ha szeretnék haladhatja meg a kérelem egység kapacitás gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="de613-122">What happens if I exceed request unit capacity for a collection?</span></span>

<span data-ttu-id="de613-123">Mivel Azure Cosmos DB több modellre adatbázis, akkor fontos, hogy a dokumentum API, egy grafikonon/csomópont egy grafikonon API és a tábla/entitás tábla API tooa gyűjtemény/dokumentum hivatkozik toonote.</span><span class="sxs-lookup"><span data-stu-id="de613-123">As Azure Cosmos DB is a multi-model database, it is important toonote that we will refer tooa collection/document for a document API, a graph/node for a graph API and a table/entity for table API.</span></span> <span data-ttu-id="de613-124">Átviteli sebesség a dokumentum azt fogja generalize tároló/elem toohello elveit.</span><span class="sxs-lookup"><span data-stu-id="de613-124">Throughput this document we will generalize toohello concepts of container/item.</span></span>

## <a name="request-units-and-request-charges"></a><span data-ttu-id="de613-125">Kérelemegység és kérelem díjak</span><span class="sxs-lookup"><span data-stu-id="de613-125">Request units and request charges</span></span>
<span data-ttu-id="de613-126">Azure Cosmos-adatbázis által gyors és kiszámítható teljesítményt nyújt *foglalása* erőforrások toosatisfy az alkalmazás átviteli sebességére kell.</span><span class="sxs-lookup"><span data-stu-id="de613-126">Azure Cosmos DB delivers fast, predictable performance by *reserving* resources toosatisfy your application's throughput needs.</span></span>  <span data-ttu-id="de613-127">Alkalmazás betölteni, és a hozzáférési minták módosítása adott idő alatt, mert az Azure Cosmos DB lehetővé teszi a tooeasily növelését, vagy csökkentse a fenntartott átviteli sebességet elérhető tooyour alkalmazás hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="de613-127">Because application load and access patterns change over time, Azure Cosmos DB allows you tooeasily increase or decrease hello amount of reserved throughput available tooyour application.</span></span>

<span data-ttu-id="de613-128">Az Azure Cosmos DB fenntartott átviteli kérelem egység / másodperc feldolgozása tekintetében van megadva.</span><span class="sxs-lookup"><span data-stu-id="de613-128">With Azure Cosmos DB, reserved throughput is specified in terms of request units processing per second.</span></span> <span data-ttu-id="de613-129">Az eltolásokat tekintheti kérelemegység átviteli pénznemként, amelyek akkor *lefoglalni* összege garantált kérelemegység másodpercenként elérhető tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="de613-129">You can think of request units as throughput currency, whereby you *reserve* an amount of guaranteed request units available tooyour application on per second basis.</span></span>  <span data-ttu-id="de613-130">Minden Azure Cosmos DB - dokumentum írása, frissítése egy dokumentumot a lekérdezés végrehajtása - műveletet igényel, Processzor, memória és iops-érték.</span><span class="sxs-lookup"><span data-stu-id="de613-130">Each operation in Azure Cosmos DB - writing a document, performing a query, updating a document - consumes CPU, memory, and IOPS.</span></span>  <span data-ttu-id="de613-131">Ez azt jelenti, hogy minden egyes művelet azt eredményezi azok háromszorosa egy *kell fizetni kérelem*, amelyhez van megadva *egységek kérelem*.</span><span class="sxs-lookup"><span data-stu-id="de613-131">That is, each operation incurs a *request charge*, which is expressed in *request units*.</span></span>  <span data-ttu-id="de613-132">Understanding hello tényező, amely hatással van a kérelem egység díjak, az alkalmazás átviteli követelményeket, valamint lehetővé teszi, hogy Ön toorun az alkalmazás a lehető leghatékonyabban költséggel.</span><span class="sxs-lookup"><span data-stu-id="de613-132">Understanding hello factors which impact request unit charges, along with your application's throughput requirements, enables you toorun your application as cost effectively as possible.</span></span> <span data-ttu-id="de613-133">a lekérdezés csodálatos eszköz tootest hello alapszintű hello lekérdezéskezelő is.</span><span class="sxs-lookup"><span data-stu-id="de613-133">hello query explorer is also a wonderful tool tootest hello core of a query.</span></span>

<span data-ttu-id="de613-134">Azt javasoljuk, hogy Kezdésként tekintse a következő videó, ahol Aravind Ramachandran ismerteti kérelemegység és kiszámítható teljesítményt az Azure Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="de613-134">We recommend getting started by watching hello following video, where Aravind Ramachandran explains request units and predictable performance with Azure Cosmos DB.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a><span data-ttu-id="de613-135">Adja meg a kérelem egység kapacitás az Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="de613-135">Specifying request unit capacity in Azure Cosmos DB</span></span>
<span data-ttu-id="de613-136">Egy új gyűjteményt, táblázat vagy graph indításakor adhatja meg hello számát a kérelemegység (RU / másodperc) másodpercenként kívánt foglalt.</span><span class="sxs-lookup"><span data-stu-id="de613-136">When starting a new collection, table or graph, you specify hello number of request units per second (RU per second) you want reserved.</span></span> <span data-ttu-id="de613-137">Hello kiosztott átviteli sebesség alapján, Azure Cosmos adatbázis lefoglalt fizikai partíciók toohost a gyűjtemény és az elágazások/rebalances adatok között partíciók növekedésével azt.</span><span class="sxs-lookup"><span data-stu-id="de613-137">Based on hello provisioned throughput, Azure Cosmos DB allocates physical partitions toohost your collection and splits/rebalances data across partitions as it grows.</span></span>

<span data-ttu-id="de613-138">Azure Cosmos-adatbázis szükséges, a partíciós kulcs toobe határozni, ha a gyűjtemény kiosztott 2500 kérést egységek vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="de613-138">Azure Cosmos DB requires a partition key toobe specified when a collection is provisioned with 2,500 request units or higher.</span></span> <span data-ttu-id="de613-139">A partíciós kulcs túl jövőbeli hello 2500 kérést egység a gyűjtemény átviteli sebességét szükséges tooscale is van.</span><span class="sxs-lookup"><span data-stu-id="de613-139">A partition key is also required tooscale your collection's throughput beyond 2,500 request units in hello future.</span></span> <span data-ttu-id="de613-140">Ezért erősen ajánlott tooconfigure egy [partíciókulcs](partition-data.md) függetlenül a kezdeti átviteli tárolója létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="de613-140">Therefore, it is highly recommended tooconfigure a [partition key](partition-data.md) when creating a container regardless of your initial throughput.</span></span> <span data-ttu-id="de613-141">Mivel az adatok előfordulhat, hogy rendelkezik-e osztani több partíciót toobe, célszerű szükséges toopick, amely rendelkezik egy nagy számosságot (100 toomillions pontos értékek), hogy a gyűjtemény/tábla/graph és a kérelmek is méretezhető egységesen Azure Cosmos DB partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="de613-141">Since your data might have toobe split across multiple partitions, it is necessary toopick a partition key that has a high cardinality (100 toomillions of distinct values) so that your collection/table/graph and requests can be scaled uniformly by Azure Cosmos DB.</span></span> 

> [!NOTE]
> <span data-ttu-id="de613-142">A partíciós kulcs, a logikai határ, és nem egy fizikai egy.</span><span class="sxs-lookup"><span data-stu-id="de613-142">A partition key is a logical boundary, and not a physical one.</span></span> <span data-ttu-id="de613-143">Emiatt nem kell különböző partíciókulcs-értékek toolimit hello száma.</span><span class="sxs-lookup"><span data-stu-id="de613-143">Therefore, you do not need toolimit hello number of distinct partition key values.</span></span> <span data-ttu-id="de613-144">Ez tulajdonképpen jobb toohave különböző kulcsértékei kisebb, mint partícióazonosító Azure Cosmos DB rendelkezik további terheléselosztási beállításai.</span><span class="sxs-lookup"><span data-stu-id="de613-144">It is in fact better toohave more distinct partition key values than less, as Azure Cosmos DB has more load balancing options.</span></span>

<span data-ttu-id="de613-145">Íme egy kódrészletet, egy gyűjtemény 3000 való létrehozásának egység / második használatával hello .NET SDK kérelem:</span><span class="sxs-lookup"><span data-stu-id="de613-145">Here is a code snippet for creating a collection with 3,000 request units per second using hello .NET SDK:</span></span>

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

<span data-ttu-id="de613-146">Azure Cosmos-adatbázis működik, a teljesítmény a Foglalás modellt.</span><span class="sxs-lookup"><span data-stu-id="de613-146">Azure Cosmos DB operates on a reservation model on throughput.</span></span> <span data-ttu-id="de613-147">Ez azt jelenti, hogy kell fizetni az átviteli sebesség hello mennyisége *fenntartott*, függetlenül attól, hogy átviteli mekkora aktívan *használt*.</span><span class="sxs-lookup"><span data-stu-id="de613-147">That is, you are billed for hello amount of throughput *reserved*, regardless of how much of that throughput is actively *used*.</span></span> <span data-ttu-id="de613-148">Mint az alkalmazás terhelés, adatok, és a használati minták módosítása könnyen méretezheti felfelé és lefelé hello SDK-k segítségével fenntartott RUs mennyisége vagy hello segítségével [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de613-148">As your application's load, data, and usage patterns change you can easily scale up and down hello amount of reserved RUs through SDKs or using hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="de613-149">Minden gyűjtemény/tábla/graph le vannak képezve tooan `Offer` Azure Cosmos DB, amelynek hello kiosztott átviteli sebesség metaadatainak erőforrás.</span><span class="sxs-lookup"><span data-stu-id="de613-149">Each collection/table/graph are mapped tooan `Offer` resource in Azure Cosmos DB, which has metadata about hello provisioned throughput.</span></span> <span data-ttu-id="de613-150">Egy tároló hello megfelelő ajánlat erőforrás keresése, akkor frissítette a hello új átviteli érték hello kiosztott átviteli sebesség módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="de613-150">You can change hello allocated throughput by looking up hello corresponding offer resource for a container, then updating it with hello new throughput value.</span></span> <span data-ttu-id="de613-151">Íme egy kódrészletet hello sebességét, a gyűjtemény too5 módosítására, 000 kérelemegység / második használatával hello .NET SDK-val:</span><span class="sxs-lookup"><span data-stu-id="de613-151">Here is a code snippet for changing hello throughput of a collection too5,000 request units per second using hello .NET SDK:</span></span>

```csharp
// Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set hello throughput too5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="de613-152">Nincs nem gyakorolt hatás toohello rendelkezésre állását a tároló hello átviteli módosításakor.</span><span class="sxs-lookup"><span data-stu-id="de613-152">There is no impact toohello availability of your container when you change hello throughput.</span></span> <span data-ttu-id="de613-153">Új fenntartott átviteli hello általában hatékony hello új átviteli kérelem másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="de613-153">Typically hello new reserved throughput is effective within seconds on application of hello new throughput.</span></span>

## <a name="request-unit-considerations"></a><span data-ttu-id="de613-154">Kérelem egység kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="de613-154">Request unit considerations</span></span>
<span data-ttu-id="de613-155">Az Azure Cosmos DB tároló kérelemben egységek tooreserve hello száma megbecsülheti, esetén fontos tootake hello változók figyelembe a következő:</span><span class="sxs-lookup"><span data-stu-id="de613-155">When estimating hello number of request units tooreserve for your Azure Cosmos DB container, it is important tootake hello following variables into consideration:</span></span>

* <span data-ttu-id="de613-156">**Konfigurációelem-méret**.</span><span class="sxs-lookup"><span data-stu-id="de613-156">**Item size**.</span></span> <span data-ttu-id="de613-157">Mérete nő, hello felhasználva tooread vagy adatok írása hello is növeli.</span><span class="sxs-lookup"><span data-stu-id="de613-157">As size increases hello units consumed tooread or write hello data will also increase.</span></span>
* <span data-ttu-id="de613-158">**Konfigurációelem-tulajdonság száma**.</span><span class="sxs-lookup"><span data-stu-id="de613-158">**Item property count**.</span></span> <span data-ttu-id="de613-159">Feltéve, hogy minden tulajdonságai, a dokumentum/csomópont/ntity növeli a hello tulajdonság számának növekedése hello felhasználva toowrite indexelése alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="de613-159">Assuming default indexing of all properties, hello units consumed toowrite a document/node/ntity will increase as hello property count increases.</span></span>
* <span data-ttu-id="de613-160">**Adatkonzisztencia**.</span><span class="sxs-lookup"><span data-stu-id="de613-160">**Data consistency**.</span></span> <span data-ttu-id="de613-161">Az erős, vagy a kötött elavulási konzisztencia szintek használata esetén további egységek lesz felhasznált tooread elemek.</span><span class="sxs-lookup"><span data-stu-id="de613-161">When using data consistency levels of Strong or Bounded Staleness, additional units will be consumed tooread items.</span></span>
* <span data-ttu-id="de613-162">**Indexelt tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="de613-162">**Indexed properties**.</span></span> <span data-ttu-id="de613-163">Egy index házirendet minden egyes tároló meghatározza, hogy mely tulajdonságok indexelt alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="de613-163">An index policy on each container determines which properties are indexed by default.</span></span> <span data-ttu-id="de613-164">Kérelem egység történő korlátozásával hello száma az indexelt tulajdonságok vagy Lusta indexelő engedélyezésével csökkentheti.</span><span class="sxs-lookup"><span data-stu-id="de613-164">You can reduce your request unit consumption by limiting hello number of indexed properties or by enabling lazy indexing.</span></span>
* <span data-ttu-id="de613-165">**A dokumentum indexelő**.</span><span class="sxs-lookup"><span data-stu-id="de613-165">**Document indexing**.</span></span> <span data-ttu-id="de613-166">Alapértelmezésben minden elem automatikusan indexelt kevesebb kérelemegység fog használni, ha úgy dönt, nem tooindex a elemek egy része.</span><span class="sxs-lookup"><span data-stu-id="de613-166">By default each item is automatically indexed, you will consume fewer request units if you choose not tooindex some of your items.</span></span>
* <span data-ttu-id="de613-167">**Lekérdezési minták**.</span><span class="sxs-lookup"><span data-stu-id="de613-167">**Query patterns**.</span></span> <span data-ttu-id="de613-168">a lekérdezés hello összetettsége hatással van kérelem egységek művelet végrehajtásánál.</span><span class="sxs-lookup"><span data-stu-id="de613-168">hello complexity of a query impacts how many Request Units are consumed for an operation.</span></span> <span data-ttu-id="de613-169">predikátumok hello száma, hello predikátumok, a leképezések, felhasználó által megadott függvények száma, illetve hello forrás adatkészlet összes befolyásolják hello költségét hello mérete jellege lekérdezési műveletek.</span><span class="sxs-lookup"><span data-stu-id="de613-169">hello number of predicates, nature of hello predicates, projections, number of UDFs, and hello size of hello source data set all influence hello cost of query operations.</span></span>
* <span data-ttu-id="de613-170">**Parancsfájl-használati**.</span><span class="sxs-lookup"><span data-stu-id="de613-170">**Script usage**.</span></span>  <span data-ttu-id="de613-171">Csakúgy, mint a lekérdezéseket, a tárolt eljárások és eseményindítók hello összetettsége hello végrehajtott műveletek alapján kérelemegység felhasználni.</span><span class="sxs-lookup"><span data-stu-id="de613-171">As with queries, stored procedures and triggers consume request units based on hello complexity of hello operations being performed.</span></span> <span data-ttu-id="de613-172">Mivel az alkalmazás fejlesztése, vizsgálja meg a hello kérelem kell fizetni fejléc toobetter megérteni, hogyan egyes műveletek nem használ-e kérelem egység kapacitás.</span><span class="sxs-lookup"><span data-stu-id="de613-172">As you develop your application, inspect hello request charge header toobetter understand how each operation is consuming request unit capacity.</span></span>

## <a name="estimating-throughput-needs"></a><span data-ttu-id="de613-173">Átviteli sebesség igények becslése</span><span class="sxs-lookup"><span data-stu-id="de613-173">Estimating throughput needs</span></span>
<span data-ttu-id="de613-174">A kérelem egység mérőszáma normalizált kérelemfeldolgozáshoz költség.</span><span class="sxs-lookup"><span data-stu-id="de613-174">A request unit is a normalized measure of request processing cost.</span></span> <span data-ttu-id="de613-175">Egy egyetlen kérelem egységet jelöli hello feldolgozási kapacitás szükséges tooread (keresztül self link vagy azonosítója) egy egyetlen 1KB cikk álló 10 egyedi tulajdonság értékével (kivéve a rendszer tulajdonságai).</span><span class="sxs-lookup"><span data-stu-id="de613-175">A single request unit represents hello processing capacity required tooread (via self link or id) a single 1KB item consisting of 10 unique property values (excluding system properties).</span></span> <span data-ttu-id="de613-176">A kérelem toocreate (insert), cserélje le, vagy azonos elem fogyaszt több hello szolgáltatás feldolgozása hello törlése, és ezáltal több kérelem egység.</span><span class="sxs-lookup"><span data-stu-id="de613-176">A request toocreate (insert), replace or delete hello same item will consume more processing from hello service and thereby more request units.</span></span>   

> [!NOTE]
> <span data-ttu-id="de613-177">hello Alapterv 1 kérelem egység az 1KB cikk megfelel-e egyszerű tooa HOZZA self link vagy hello elem azonosítója.</span><span class="sxs-lookup"><span data-stu-id="de613-177">hello baseline of 1 request unit for a 1KB item corresponds tooa simple GET by self link or id of hello item.</span></span>
> 
> 

<span data-ttu-id="de613-178">Itt például van egy táblázat mutatja be, hogy hány kérés egységek tooprovision három különböző elem mérete (1KB, 4 KB-os és 64 KB-os) és a két különböző teljesítményszintek (500 olvasás/másodperc + 100 írási műveletek másodpercenkénti száma és 500 olvasás/másodperc + 500 írás/másodperc).</span><span class="sxs-lookup"><span data-stu-id="de613-178">For example, here's a table that shows how many request units tooprovision at three different item sizes (1KB, 4KB, and 64KB) and at two different performance levels (500 reads/second + 100 writes/second and 500 reads/second + 500 writes/second).</span></span> <span data-ttu-id="de613-179">hello adatkonzisztencia munkamenetben lett konfigurálva, és házirend indexelő hello tooNone lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="de613-179">hello data consistency was configured at Session, and hello indexing policy was set tooNone.</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-180"><strong>Elem mérete</strong></span><span class="sxs-lookup"><span data-stu-id="de613-180"><strong>Item size</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-181"><strong>Olvasási/másodperc</strong></span><span class="sxs-lookup"><span data-stu-id="de613-181"><strong>Reads/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-182"><strong>Írási műveletek másodpercenkénti száma</strong></span><span class="sxs-lookup"><span data-stu-id="de613-182"><strong>Writes/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-183"><strong>Kérelemegységek</strong></span><span class="sxs-lookup"><span data-stu-id="de613-183"><strong>Request units</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-184">1 KB</span><span class="sxs-lookup"><span data-stu-id="de613-184">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-185">500</span><span class="sxs-lookup"><span data-stu-id="de613-185">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-186">100</span><span class="sxs-lookup"><span data-stu-id="de613-186">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-187">(500 * 1) + (100 * 5) = 1000 RU/mp</span><span class="sxs-lookup"><span data-stu-id="de613-187">(500 * 1) + (100 * 5) = 1,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-188">1 KB</span><span class="sxs-lookup"><span data-stu-id="de613-188">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-189">500</span><span class="sxs-lookup"><span data-stu-id="de613-189">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-190">500</span><span class="sxs-lookup"><span data-stu-id="de613-190">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-191">(500 * 1) + (500 * 5) = 3000 RU/mp</span><span class="sxs-lookup"><span data-stu-id="de613-191">(500 * 1) + (500 * 5) = 3,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-192">4 KB-OS</span><span class="sxs-lookup"><span data-stu-id="de613-192">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-193">500</span><span class="sxs-lookup"><span data-stu-id="de613-193">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-194">100</span><span class="sxs-lookup"><span data-stu-id="de613-194">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-195">(500 * 1,3) + (100 * 7) = 1,350 RU/mp</span><span class="sxs-lookup"><span data-stu-id="de613-195">(500 * 1.3) + (100 * 7) = 1,350 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-196">4 KB-OS</span><span class="sxs-lookup"><span data-stu-id="de613-196">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-197">500</span><span class="sxs-lookup"><span data-stu-id="de613-197">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-198">500</span><span class="sxs-lookup"><span data-stu-id="de613-198">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-199">(500 * 1,3) + (500 * 7) = 4,150 RU/mp</span><span class="sxs-lookup"><span data-stu-id="de613-199">(500 * 1.3) + (500 * 7) = 4,150 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-200">64 KB</span><span class="sxs-lookup"><span data-stu-id="de613-200">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-201">500</span><span class="sxs-lookup"><span data-stu-id="de613-201">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-202">100</span><span class="sxs-lookup"><span data-stu-id="de613-202">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-203">(500 * 10) + (100 * 48) = 9,800 RU/mp</span><span class="sxs-lookup"><span data-stu-id="de613-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="de613-204">64 KB</span><span class="sxs-lookup"><span data-stu-id="de613-204">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-205">500</span><span class="sxs-lookup"><span data-stu-id="de613-205">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-206">500</span><span class="sxs-lookup"><span data-stu-id="de613-206">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="de613-207">(500 * 10) + (500 * 48) = 29,000 RU/mp</span><span class="sxs-lookup"><span data-stu-id="de613-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span></span></p></td>
        </tr>
    </tbody>
</table>

### <a name="use-hello-request-unit-calculator"></a><span data-ttu-id="de613-208">Hello kérelem egység Számológép használata</span><span class="sxs-lookup"><span data-stu-id="de613-208">Use hello request unit calculator</span></span>
<span data-ttu-id="de613-209">toohelp ügyfelek finom hangolja az átviteli sebesség becsléseket, és van egy webalapú [kérelem egység Számológép](https://www.documentdb.com/capacityplanner) toohelp becsült hello kérelem egységekre vonatkozó követelményeket a tipikus műveleteket, köztük:</span><span class="sxs-lookup"><span data-stu-id="de613-209">toohelp customers fine tune their throughput estimations, there is a web based [request unit calculator](https://www.documentdb.com/capacityplanner) toohelp estimate hello request unit requirements for typical operations, including:</span></span>

* <span data-ttu-id="de613-210">Elem hoz létre (írás)</span><span class="sxs-lookup"><span data-stu-id="de613-210">Item creates (writes)</span></span>
* <span data-ttu-id="de613-211">Elem beolvasása</span><span class="sxs-lookup"><span data-stu-id="de613-211">Item reads</span></span>
* <span data-ttu-id="de613-212">Elem törlése</span><span class="sxs-lookup"><span data-stu-id="de613-212">Item deletes</span></span>
* <span data-ttu-id="de613-213">Elem frissítések</span><span class="sxs-lookup"><span data-stu-id="de613-213">Item updates</span></span>

<span data-ttu-id="de613-214">hello eszköz is támogatja az adattárolási igények kielégítésére megadta hello minta elemeken alapuló becslése.</span><span class="sxs-lookup"><span data-stu-id="de613-214">hello tool also includes support for estimating data storage needs based on hello sample items you provide.</span></span>

<span data-ttu-id="de613-215">Hello eszközzel felettébb egyszerű:</span><span class="sxs-lookup"><span data-stu-id="de613-215">Using hello tool is simple:</span></span>

1. <span data-ttu-id="de613-216">Töltsön fel egy vagy több jellemző elemet.</span><span class="sxs-lookup"><span data-stu-id="de613-216">Upload one or more representative items.</span></span>
   
    ![Elemek toohello kérelem egység Számológép feltöltése][2]
2. <span data-ttu-id="de613-218">adattárolási követelmények tooestimate, adja meg a hello száma elemek toostore várt.</span><span class="sxs-lookup"><span data-stu-id="de613-218">tooestimate data storage requirements, enter hello total number of items you expect toostore.</span></span>
3. <span data-ttu-id="de613-219">Adja meg hello elemszáma létrehozása, olvasása, frissítése és törlési műveletek (a másodpercenként alapján) van szüksége.</span><span class="sxs-lookup"><span data-stu-id="de613-219">Enter hello number of items create, read, update, and delete operations you require (on a per-second basis).</span></span> <span data-ttu-id="de613-220">tooestimate hello kérelem egység díjak elem frissítési műveletek, töltse fel a hello minta elem 1. lépésben a fenti tipikus mező frissítéseket tartalmazó másolatát.</span><span class="sxs-lookup"><span data-stu-id="de613-220">tooestimate hello request unit charges of item update operations, upload a copy of hello sample item from step 1 above that includes typical field updates.</span></span>  <span data-ttu-id="de613-221">Például ha elem frissítések általában a lastLogin és userVisits nevű két tulajdonságainak módosítása, akkor egyszerűen másolhatja hello minta elem, hello értékek ezek két tulajdonságok frissítése, és töltse fel a másolt hello elemet.</span><span class="sxs-lookup"><span data-stu-id="de613-221">For example, if item updates typically modify two properties named lastLogin and userVisits, then simply copy hello sample item, update hello values for those two properties, and upload hello copied item.</span></span>
   
    ![Adja meg a átviteli követelményeket hello kérelem egység Számológép][3]
4. <span data-ttu-id="de613-223">Kattintson kiszámításához, és vizsgálja meg hello eredményét.</span><span class="sxs-lookup"><span data-stu-id="de613-223">Click calculate and examine hello results.</span></span>
   
    ![Kérelem egység Számológép eredményei][4]

> [!NOTE]
> <span data-ttu-id="de613-225">Ha az indexelt tulajdonságok méretét és hello számát jelentősen eltérő típusú elemekre, majd töltse fel az egyes minta *típus* jellemző elem toohello az eszközt, és ezután hello számol.</span><span class="sxs-lookup"><span data-stu-id="de613-225">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then upload a sample of each *type* of typical item toohello tool and then calculate hello results.</span></span>
> 
> 

### <a name="use-hello-azure-cosmos-db-request-charge-response-header"></a><span data-ttu-id="de613-226">Hello Azure Cosmos DB kérelem kell fizetni válaszfejléc használata</span><span class="sxs-lookup"><span data-stu-id="de613-226">Use hello Azure Cosmos DB request charge response header</span></span>
<span data-ttu-id="de613-227">Minden hello Azure Cosmos DB szolgáltatás válasza tartalmaz egy egyéni fejlécet (`x-ms-request-charge`), amely tartalmazza a hello kérelem felhasznált hello kérelemegység.</span><span class="sxs-lookup"><span data-stu-id="de613-227">Every response from hello Azure Cosmos DB service includes a custom header (`x-ms-request-charge`) that contains hello request units consumed for hello request.</span></span> <span data-ttu-id="de613-228">Ezt a fejlécet hello Azure Cosmos DB SDK-k keresztül is érhető el.</span><span class="sxs-lookup"><span data-stu-id="de613-228">This header is also accessible through hello Azure Cosmos DB SDKs.</span></span> <span data-ttu-id="de613-229">A .NET SDK hello RequestCharge hello ResourceResponse objektum olyan osztályát.</span><span class="sxs-lookup"><span data-stu-id="de613-229">In hello .NET SDK, RequestCharge is a property of hello ResourceResponse object.</span></span>  <span data-ttu-id="de613-230">A lekérdezések hello Azure Cosmos DB lekérdezéskezelő hello Azure-portálon a információival kérelem kell fizetni végrehajtott lekérdezések számára.</span><span class="sxs-lookup"><span data-stu-id="de613-230">For queries, hello Azure Cosmos DB Query Explorer in hello Azure portal provides request charge information for executed queries.</span></span>

![RU költségeket az hello lekérdezéskezelő vizsgálata][1]

<span data-ttu-id="de613-232">Ennek tudatában, egy fenntartott átviteli sebességet, az alkalmazás által igényelt hello mennyisége becslése módja toorecord hello kérelem egység kell fizetni társított tipikus műveleteket futtatott egy reprezentatív elem, amelyet az alkalmazás, majd műveletek száma hello becslése várhatóan másodpercenként végez.</span><span class="sxs-lookup"><span data-stu-id="de613-232">With this in mind, one method for estimating hello amount of reserved throughput required by your application is toorecord hello request unit charge associated with running typical operations against a representative item used by your application and then estimating hello number of operations you anticipate performing each second.</span></span>  <span data-ttu-id="de613-233">Lehet, hogy toomeasure és tipikus lekérdezések és Azure Cosmos DB parancsfájl használati is.</span><span class="sxs-lookup"><span data-stu-id="de613-233">Be sure toomeasure and include typical queries and Azure Cosmos DB script usage as well.</span></span>

> [!NOTE]
> <span data-ttu-id="de613-234">Ha az indexelt tulajdonságok méretét és hello számát jelentősen eltérő típusú elemekre, majd rögzítse hello alkalmazandó művelet kérelem egység kell fizetni az összes kapcsolódó *típus* jellemző elem.</span><span class="sxs-lookup"><span data-stu-id="de613-234">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then record hello applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

<span data-ttu-id="de613-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="de613-235">For example:</span></span>

1. <span data-ttu-id="de613-236">Hello kérelem egység létrehozása (Beszúrás) kell fizetni rögzítéséhez egy jellemző elemet.</span><span class="sxs-lookup"><span data-stu-id="de613-236">Record hello request unit charge of creating (inserting) a typical item.</span></span> 
2. <span data-ttu-id="de613-237">Rekord hello kérelem egység kell fizetni egy tipikus elem olvasása.</span><span class="sxs-lookup"><span data-stu-id="de613-237">Record hello request unit charge of reading a typical item.</span></span>
3. <span data-ttu-id="de613-238">Rekord hello kérelem egység felelős egy tipikus elem frissítése.</span><span class="sxs-lookup"><span data-stu-id="de613-238">Record hello request unit charge of updating a typical item.</span></span>
4. <span data-ttu-id="de613-239">Rekord hello kérelem egység kell fizetni tipikus, közös elem lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="de613-239">Record hello request unit charge of typical, common item queries.</span></span>
5. <span data-ttu-id="de613-240">Rekord hello kérelem egység ingyenesen elérhető bármely egyéni parancsfájlok (tárolt eljárások, eseményindítók, felhasználó által definiált függvények) hello alkalmazás alkalmazhatók</span><span class="sxs-lookup"><span data-stu-id="de613-240">Record hello request unit charge of any custom scripts (stored procedures, triggers, user-defined functions) leveraged by hello application</span></span>
6. <span data-ttu-id="de613-241">Hello szükséges kérelem egységek megadott műveletek hello becsült száma, amelyek várhatóan toorun másodpercenként kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="de613-241">Calculate hello required request units given hello estimated number of operations you anticipate toorun each second.</span></span>

### <span data-ttu-id="de613-242"><a id="GetLastRequestStatistics"></a>A MongoDB GetLastRequestStatistics parancshoz használható API</span><span class="sxs-lookup"><span data-stu-id="de613-242"><a id="GetLastRequestStatistics"></a>Use API for MongoDB's GetLastRequestStatistics command</span></span>
<span data-ttu-id="de613-243">Mongodb-protokolltámogatással API támogatja egy egyéni parancs *getLastRequestStatistics*, hello kérelem kell fizetni a megadott műveletek beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="de613-243">API for MongoDB supports a custom command, *getLastRequestStatistics*, for retrieving hello request charge for specified operations.</span></span>

<span data-ttu-id="de613-244">A Mongo rendszerhéj hello, például azt szeretné, hogy tooverify hello kérelem díjat hello művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="de613-244">For example, in hello Mongo Shell, execute hello operation you want tooverify hello request charge for.</span></span>
```
> db.sample.find()
```

<span data-ttu-id="de613-245">Ezután hajtsa végre a hello parancsot *getLastRequestStatistics*.</span><span class="sxs-lookup"><span data-stu-id="de613-245">Next, execute hello command *getLastRequestStatistics*.</span></span>
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

<span data-ttu-id="de613-246">Ennek tudatában, egy fenntartott átviteli sebességet, az alkalmazás által igényelt hello mennyisége becslése módja toorecord hello kérelem egység kell fizetni társított tipikus műveleteket futtatott egy reprezentatív elem, amelyet az alkalmazás, majd műveletek száma hello becslése várhatóan másodpercenként végez.</span><span class="sxs-lookup"><span data-stu-id="de613-246">With this in mind, one method for estimating hello amount of reserved throughput required by your application is toorecord hello request unit charge associated with running typical operations against a representative item used by your application and then estimating hello number of operations you anticipate performing each second.</span></span>

> [!NOTE]
> <span data-ttu-id="de613-247">Ha az indexelt tulajdonságok méretét és hello számát jelentősen eltérő típusú elemekre, majd rögzítse hello alkalmazandó művelet kérelem egység kell fizetni az összes kapcsolódó *típus* jellemző elem.</span><span class="sxs-lookup"><span data-stu-id="de613-247">If you have item types which will differ dramatically in terms of size and hello number of indexed properties, then record hello applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a><span data-ttu-id="de613-248">MongoDB a portál metrikáihoz API használata</span><span class="sxs-lookup"><span data-stu-id="de613-248">Use API for MongoDB's portal metrics</span></span>
<span data-ttu-id="de613-249">egy jó felmérést kérelem egység a MongoDB adatbázis toouse hello díja az API-legegyszerűbb módja tooget hello [Azure-portálon](https://portal.azure.com) metrikákat.</span><span class="sxs-lookup"><span data-stu-id="de613-249">hello simplest way tooget a good estimation of request unit charges for your API for MongoDB database is toouse hello [Azure portal](https://portal.azure.com) metrics.</span></span> <span data-ttu-id="de613-250">A hello *kérések száma* és *kérelem kell fizetni* diagramok, kaphat a kérelem egységek minden művelet felmérését fel, és hány kérelemegység használják ki a relatív tooone egy másik.</span><span class="sxs-lookup"><span data-stu-id="de613-250">With hello *Number of requests* and *Request Charge* charts, you can get an estimation of how many request units each operation is consuming and how many request units they consume relative tooone another.</span></span>

![API-t a MongoDB portál metrikák][6]

## <a name="a-request-unit-estimation-example"></a><span data-ttu-id="de613-252">A kérelem egység becslés – példa</span><span class="sxs-lookup"><span data-stu-id="de613-252">A request unit estimation example</span></span>
<span data-ttu-id="de613-253">Vegye figyelembe a következő ~ 1 KB méretű dokumentum hello:</span><span class="sxs-lookup"><span data-stu-id="de613-253">Consider hello following ~1KB document:</span></span>

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> <span data-ttu-id="de613-254">Dokumentumok Azure Cosmos DB, a rendszer minified, hello rendszer által számított hello dokumentum fenti mérete valamivel kisebb, mint 1KB.</span><span class="sxs-lookup"><span data-stu-id="de613-254">Documents are minified in Azure Cosmos DB, so hello system calculated size of hello document above is slightly less than 1KB.</span></span>
> 
> 

<span data-ttu-id="de613-255">hello következő táblázatban a hozzávetőleges kérelem egység díjak ezt az elemet a tipikus műveleteihez (hello hozzávetőleges kérelem egység kell fizetni feltételezi, hogy hello fiók konzisztencia szintje túl "Munkamenet" és az összes elemet automatikusan indexelt):</span><span class="sxs-lookup"><span data-stu-id="de613-255">hello following table shows approximate request unit charges for typical operations on this item (hello approximate request unit charge assumes that hello account consistency level is set too“Session” and that all items are automatically indexed):</span></span>

| <span data-ttu-id="de613-256">Művelet</span><span class="sxs-lookup"><span data-stu-id="de613-256">Operation</span></span> | <span data-ttu-id="de613-257">Egységköltség kérése</span><span class="sxs-lookup"><span data-stu-id="de613-257">Request Unit Charge</span></span> |
| --- | --- |
| <span data-ttu-id="de613-258">Konfigurációelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="de613-258">Create item</span></span> |<span data-ttu-id="de613-259">~ 15 RU</span><span class="sxs-lookup"><span data-stu-id="de613-259">~15 RU</span></span> |
| <span data-ttu-id="de613-260">Elem olvasása</span><span class="sxs-lookup"><span data-stu-id="de613-260">Read item</span></span> |<span data-ttu-id="de613-261">~ 1 RU</span><span class="sxs-lookup"><span data-stu-id="de613-261">~1 RU</span></span> |
| <span data-ttu-id="de613-262">Lekérdezés elem azonosítója</span><span class="sxs-lookup"><span data-stu-id="de613-262">Query item by id</span></span> |<span data-ttu-id="de613-263">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="de613-263">~2.5 RU</span></span> |

<span data-ttu-id="de613-264">Emellett az alábbi táblázatban hozzávetőleges kérelem egység díjak hello alkalmazásban használt szokásos lekérdezések:</span><span class="sxs-lookup"><span data-stu-id="de613-264">Additionally, this table shows approximate request unit charges for typical queries used in hello application:</span></span>

| <span data-ttu-id="de613-265">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="de613-265">Query</span></span> | <span data-ttu-id="de613-266">Egységköltség kérése</span><span class="sxs-lookup"><span data-stu-id="de613-266">Request Unit Charge</span></span> | <span data-ttu-id="de613-267">Visszaadott elemek száma</span><span class="sxs-lookup"><span data-stu-id="de613-267"># of Returned Items</span></span> |
| --- | --- | --- |
| <span data-ttu-id="de613-268">Válassza ki a étele-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="de613-268">Select food by id</span></span> |<span data-ttu-id="de613-269">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="de613-269">~2.5 RU</span></span> |<span data-ttu-id="de613-270">1</span><span class="sxs-lookup"><span data-stu-id="de613-270">1</span></span> |
| <span data-ttu-id="de613-271">Válassza ki a gyártó által élelmiszerek</span><span class="sxs-lookup"><span data-stu-id="de613-271">Select foods by manufacturer</span></span> |<span data-ttu-id="de613-272">~ 7 RU</span><span class="sxs-lookup"><span data-stu-id="de613-272">~7 RU</span></span> |<span data-ttu-id="de613-273">7</span><span class="sxs-lookup"><span data-stu-id="de613-273">7</span></span> |
| <span data-ttu-id="de613-274">Jelöljön ki étele csoport és egy rendezési súlyozással</span><span class="sxs-lookup"><span data-stu-id="de613-274">Select by food group and order by weight</span></span> |<span data-ttu-id="de613-275">~ 70 RU</span><span class="sxs-lookup"><span data-stu-id="de613-275">~70 RU</span></span> |<span data-ttu-id="de613-276">100</span><span class="sxs-lookup"><span data-stu-id="de613-276">100</span></span> |
| <span data-ttu-id="de613-277">Válassza ki a felső 10 élelmiszerek étele csoportban</span><span class="sxs-lookup"><span data-stu-id="de613-277">Select top 10 foods in a food group</span></span> |<span data-ttu-id="de613-278">~ 10 RU</span><span class="sxs-lookup"><span data-stu-id="de613-278">~10 RU</span></span> |<span data-ttu-id="de613-279">10</span><span class="sxs-lookup"><span data-stu-id="de613-279">10</span></span> |

> [!NOTE]
> <span data-ttu-id="de613-280">RU díjak visszaküldött elemek száma hello függően változhat.</span><span class="sxs-lookup"><span data-stu-id="de613-280">RU charges vary based on hello number of items returned.</span></span>
> 
> 

<span data-ttu-id="de613-281">Az információ azt megbecsülhető a megadott alkalmazás hello műveletek és száma másodpercenként várhatóan lekérdezések hello RU követelményei:</span><span class="sxs-lookup"><span data-stu-id="de613-281">With this information, we can estimate hello RU requirements for this application given hello number of operations and queries we expect per second:</span></span>

| <span data-ttu-id="de613-282">A művelet/lekérdezés</span><span class="sxs-lookup"><span data-stu-id="de613-282">Operation/Query</span></span> | <span data-ttu-id="de613-283">Becsült száma másodpercenként</span><span class="sxs-lookup"><span data-stu-id="de613-283">Estimated number per second</span></span> | <span data-ttu-id="de613-284">Szükséges RUs</span><span class="sxs-lookup"><span data-stu-id="de613-284">Required RUs</span></span> |
| --- | --- | --- |
| <span data-ttu-id="de613-285">Konfigurációelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="de613-285">Create item</span></span> |<span data-ttu-id="de613-286">10</span><span class="sxs-lookup"><span data-stu-id="de613-286">10</span></span> |<span data-ttu-id="de613-287">150</span><span class="sxs-lookup"><span data-stu-id="de613-287">150</span></span> |
| <span data-ttu-id="de613-288">Elem olvasása</span><span class="sxs-lookup"><span data-stu-id="de613-288">Read item</span></span> |<span data-ttu-id="de613-289">100</span><span class="sxs-lookup"><span data-stu-id="de613-289">100</span></span> |<span data-ttu-id="de613-290">100</span><span class="sxs-lookup"><span data-stu-id="de613-290">100</span></span> |
| <span data-ttu-id="de613-291">Válassza ki a gyártó által élelmiszerek</span><span class="sxs-lookup"><span data-stu-id="de613-291">Select foods by manufacturer</span></span> |<span data-ttu-id="de613-292">25</span><span class="sxs-lookup"><span data-stu-id="de613-292">25</span></span> |<span data-ttu-id="de613-293">175</span><span class="sxs-lookup"><span data-stu-id="de613-293">175</span></span> |
| <span data-ttu-id="de613-294">Válassza ki a étele csoport szerint</span><span class="sxs-lookup"><span data-stu-id="de613-294">Select by food group</span></span> |<span data-ttu-id="de613-295">10</span><span class="sxs-lookup"><span data-stu-id="de613-295">10</span></span> |<span data-ttu-id="de613-296">700</span><span class="sxs-lookup"><span data-stu-id="de613-296">700</span></span> |
| <span data-ttu-id="de613-297">Válassza ki a felső 10</span><span class="sxs-lookup"><span data-stu-id="de613-297">Select top 10</span></span> |<span data-ttu-id="de613-298">15</span><span class="sxs-lookup"><span data-stu-id="de613-298">15</span></span> |<span data-ttu-id="de613-299">150 összesen</span><span class="sxs-lookup"><span data-stu-id="de613-299">150 Total</span></span> |

<span data-ttu-id="de613-300">Ebben az esetben egy 1,275 RU/s átlagos átviteli sebességgel követelmény várhatóan.</span><span class="sxs-lookup"><span data-stu-id="de613-300">In this case, we expect an average throughput requirement of 1,275 RU/s.</span></span>  <span data-ttu-id="de613-301">Felfelé toohello legközelebbi 100, azt kellene kiépítése 1300 RU/mp ezt az alkalmazást a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="de613-301">Rounding up toohello nearest 100, we would provision 1,300 RU/s for this application's collection.</span></span>

## <span data-ttu-id="de613-302"><a id="RequestRateTooLarge"></a>Az Azure Cosmos Adatbázisba meghaladó fenntartott átviteli sebességének korlátai</span><span class="sxs-lookup"><span data-stu-id="de613-302"><a id="RequestRateTooLarge"></a> Exceeding reserved throughput limits in Azure Cosmos DB</span></span>
<span data-ttu-id="de613-303">Visszahívása, hogy kérelem egység fogyasztás kiértékelhető legyen másodpercenkénti, ha hello költségvetési üres.</span><span class="sxs-lookup"><span data-stu-id="de613-303">Recall that request unit consumption is evaluated as a rate per second if hello budget is empty.</span></span> <span data-ttu-id="de613-304">Az alkalmazások, amelyek mérete meghaladja a hello kiosztott kérelem egység egy tároló-kérelmek száma másodpercenként toothat gyűjtemény halmozódni fog, amíg hello arány fenntartott hello szint alá csökken.</span><span class="sxs-lookup"><span data-stu-id="de613-304">For applications that exceed hello provisioned request unit rate for a container, requests toothat collection will be throttled until hello rate drops below hello reserved level.</span></span> <span data-ttu-id="de613-305">A szabályozási esetén hello server megelőző jelleggel véget ér hello kérelem RequestRateTooLargeException (HTTP-állapotkód: 429) és a visszatérési hello x-ms-újrapróbálkozási-után-ms-fejléc jelző idő, ezredmásodpercben, felhasználói hello hello mennyiségét kell várnia, mielőtt hello kérelem megoldódhat.</span><span class="sxs-lookup"><span data-stu-id="de613-305">When a throttle occurs, hello server will preemptively end hello request with RequestRateTooLargeException (HTTP status code 429) and return hello x-ms-retry-after-ms header indicating hello amount of time, in milliseconds, that hello user must wait before reattempting hello request.</span></span>

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

<span data-ttu-id="de613-306">Hello .NET SDK-ügyfél és a LINQ-lekérdezések, majd a legtöbbször ennek hello hello .NET SDK-ügyfél aktuális verziója hello implicit módon ki ezt a választ, soha nem kell ezt a kivételt a toodeal használatakor tekintetben hello kiszolgáló által megadott újrapróbálkozási után fejlécet, és Az újrapróbálkozások hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="de613-306">If you are using hello .NET Client SDK and LINQ queries, then most of hello time you never have toodeal with this exception, as hello current version of hello .NET Client SDK implicitly catches this response, respects hello server-specified retry-after header, and retries hello request.</span></span> <span data-ttu-id="de613-307">Kivéve, ha a fiók egyszerre több ügyfél által is hozzáférnek, hello legközelebbi újrapróbálkozás sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="de613-307">Unless your account is being accessed concurrently by multiple clients, hello next retry will succeed.</span></span>

<span data-ttu-id="de613-308">Ha egynél több ügyfél összesítve felett hello lekérdezési gyakorisága, működő hello alapértelmezett újrapróbálási viselkedése nem elegendők, és hello ügyfél kivételhibát egy DocumentClientException állapot kód 429-es jelű toohello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="de613-308">If you have more than one client cumulatively operating above hello request rate, hello default retry behavior may not suffice, and hello client will throw a DocumentClientException with status code 429 toohello application.</span></span> <span data-ttu-id="de613-309">Például ez esetben érdemes újrapróbálási viselkedése és rutinok kezelése vagy hello hello tároló fenntartott átviteli sebesség növelése az alkalmazás hibás programot.</span><span class="sxs-lookup"><span data-stu-id="de613-309">In cases such as this, you may consider handling retry behavior and logic in your application's error handling routines or increasing hello reserved throughput for hello container.</span></span>

## <span data-ttu-id="de613-310"><a id="RequestRateTooLargeAPIforMongoDB"></a>Mongodb-protokolltámogatással meghaladja a fenntartott átviteli sebességének korlátai API-ban.</span><span class="sxs-lookup"><span data-stu-id="de613-310"><a id="RequestRateTooLargeAPIforMongoDB"></a> Exceeding reserved throughput limits in API for MongoDB</span></span>
<span data-ttu-id="de613-311">Alkalmazások, mint a gyűjtemény kiépítése hello kérelemegység halmozódni fog, amíg hello arány fenntartott hello szint alá csökken.</span><span class="sxs-lookup"><span data-stu-id="de613-311">Applications that exceed hello provisioned request units for a collection will be throttled until hello rate drops below hello reserved level.</span></span> <span data-ttu-id="de613-312">A szabályozási esetén hello háttér megelőző jelleggel véget ér az hello kérelmek egy *16500* hibakód - *túl sok kérelem*.</span><span class="sxs-lookup"><span data-stu-id="de613-312">When a throttle occurs, hello backend will preemptively end hello request with a *16500* error code - *Too Many Requests*.</span></span> <span data-ttu-id="de613-313">Alapértelmezés szerint API-t a MongoDB automatikusan megpróbálja too10 alkalommal visszatérése előtt fel egy *túl sok kérelem* hibakód.</span><span class="sxs-lookup"><span data-stu-id="de613-313">By default, API for MongoDB will automatically retry up too10 times before returning a *Too Many Requests* error code.</span></span> <span data-ttu-id="de613-314">Ha sok kap *túl sok kérelem* hibakódok, akkor fontolja meg vagy hozzáadását újrapróbálási viselkedése a az alkalmazás hibakezelési rutinok vagy [hello hello gyűjteményfenntartottátvitelisebességnövelése](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="de613-314">If you are receiving many *Too Many Requests* error codes, you may consider either adding retry behavior in your application's error handling routines or [increasing hello reserved throughput for hello collection](set-throughput.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="de613-315">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de613-315">Next steps</span></span>
<span data-ttu-id="de613-316">További információ az Azure Cosmos DB adatbázisok fenntartott átviteli sebességet toolearn ismerheti meg ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="de613-316">toolearn more about reserved throughput with Azure Cosmos DB databases, explore these resources:</span></span>

* [<span data-ttu-id="de613-317">Azure-beli árakról Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="de613-317">Azure Cosmos DB pricing</span></span>](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [<span data-ttu-id="de613-318">Az Azure Cosmos Adatbázisba az adatok particionálása</span><span class="sxs-lookup"><span data-stu-id="de613-318">Partitioning data in Azure Cosmos DB</span></span>](partition-data.md)

<span data-ttu-id="de613-319">toolearn Azure Cosmos DB, kapcsolatos további információkért lásd: hello Azure Cosmos DB [dokumentáció](https://azure.microsoft.com/documentation/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="de613-319">toolearn more about Azure Cosmos DB, see hello Azure Cosmos DB [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/).</span></span> 

<span data-ttu-id="de613-320">Méretezés és teljesítmény Azure Cosmos DB, tesztelték használatába tooget lásd [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="de613-320">tooget started with scale and performance testing with Azure Cosmos DB, see [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md).</span></span>

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
