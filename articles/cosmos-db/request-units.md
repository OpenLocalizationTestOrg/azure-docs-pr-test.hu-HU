---
title: "Kérelem egységek & átviteli - Azure Cosmos DB becslése |} Microsoft Docs"
description: "További tudnivalók ismertetése, adja meg, és az Azure Cosmos Adatbázisba kérelem egység követelményeinek becslése."
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
ms.openlocfilehash: 7a4efc0fb9b3855b9dbbe445768ceb2a9940d0b2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="request-units-in-azure-cosmos-db"></a><span data-ttu-id="8d0e8-103">Az Azure Cosmos DB egység kérése</span><span class="sxs-lookup"><span data-stu-id="8d0e8-103">Request Units in Azure Cosmos DB</span></span>
<span data-ttu-id="8d0e8-104">Most már hozzáférhető: Azure Cosmos DB [kérelem egység Számológép](https://www.documentdb.com/capacityplanner).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-104">Now available: Azure Cosmos DB [request unit calculator](https://www.documentdb.com/capacityplanner).</span></span> <span data-ttu-id="8d0e8-105">További információ: [megbecsülheti, az átviteli sebesség kell](request-units.md#estimating-throughput-needs).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-105">Learn more in [Estimating your throughput needs](request-units.md#estimating-throughput-needs).</span></span>

![Átviteli sebesség Számológép][5]

## <a name="introduction"></a><span data-ttu-id="8d0e8-107">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8d0e8-107">Introduction</span></span>
<span data-ttu-id="8d0e8-108">[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Microsoft globálisan elosztott több modellre adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-108">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is Microsoft's globally distributed multi-model database.</span></span> <span data-ttu-id="8d0e8-109">Az Azure Cosmos DB nem kell virtuális gépek kölcsönbe, szoftver központi telepítése vagy adatbázisok figyelése.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-109">With Azure Cosmos DB, you don't have to rent virtual machines, deploy software, or monitor databases.</span></span> <span data-ttu-id="8d0e8-110">Azure Cosmos DB üzemeltetett, és képes biztosítani a világ osztály rendelkezésre állását, teljesítményét és adatok védelme a Microsoft felső mérnökök folyamatosan figyeli.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-110">Azure Cosmos DB is operated and continuously monitored by Microsoft top engineers to deliver world class availability, performance, and data protection.</span></span> <span data-ttu-id="8d0e8-111">Az adatok az Ön által választott, API-k használatával végezheti el [a DocumentDB SQL](documentdb-sql-query.md) (dokumentumok) MongoDB (dokumentumok), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (kulcs-érték), és [Gremlin](https://tinkerpop.apache.org/gremlin.html) (diagramot)-e minden natív módon támogatottak.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-111">You can access your data using APIs of your choice, as [DocumentDB SQL](documentdb-sql-query.md) (document), MongoDB (document), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (key-value), and [Gremlin](https://tinkerpop.apache.org/gremlin.html) (graph) are all natively supported.</span></span> <span data-ttu-id="8d0e8-112">Azure Cosmos DB pénzneme kérelem egység (RU).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-112">The currency of Azure Cosmos DB is the Request Unit (RU).</span></span> <span data-ttu-id="8d0e8-113">A RUs nem kell olvasási/írási kapacitások vagy rendelkezés Processzor, memória, és iops-érték.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-113">With RUs, you do not need to reserve read/write capacities or provision CPU, Memory and IOPS.</span></span>

<span data-ttu-id="8d0e8-114">Azure Cosmos DB alkalmazásprogramozási támogatja a különböző műveletekkel, egyszerű olvasási műveletek közötti, és összetett graph lekérdezések írja.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-114">Azure Cosmos DB supports a number of APIs with different operations ranging from simple reads and writes to complex graph queries.</span></span> <span data-ttu-id="8d0e8-115">Mivel nem minden kérelemre egyenlő, hozzárendeli egy normalizált mennyisége **egységek kérelem** a kérelem kiszolgálásához szükséges számítási mennyisége alapján.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-115">Since not all requests are equal, they are assigned a normalized quantity of **request units** based on the amount of computation required to serve the request.</span></span> <span data-ttu-id="8d0e8-116">A száma kérelem művelet nem determinisztikus, és nyomon követheti a válasz fejléce Azure Cosmos DB bármely művelet által felhasznált kérelem egységek száma.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-116">The number of request units for an operation is deterministic, and you can track the number of request units consumed by any operation in Azure Cosmos DB via a response header.</span></span> 

<span data-ttu-id="8d0e8-117">Kiszámítható teljesítmény elérése érdekében kell lefoglalni átviteli egységekben 100 RU/másodperc.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-117">To provide predictable performance, you need to reserve throughput in units of 100 RU/second.</span></span> 

<span data-ttu-id="8d0e8-118">A cikk elolvasása után képes lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-118">After reading this article, you'll be able to answer the following questions:</span></span>  

* <span data-ttu-id="8d0e8-119">Mik azok a egységek kérése és díjak kérelem?</span><span class="sxs-lookup"><span data-stu-id="8d0e8-119">What are request units and request charges?</span></span>
* <span data-ttu-id="8d0e8-120">Hogyan adja meg a kérelem egység kapacitás gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="8d0e8-120">How do I specify request unit capacity for a collection?</span></span>
* <span data-ttu-id="8d0e8-121">Hogyan becsléséhez, az alkalmazás kérelem egység van szüksége?</span><span class="sxs-lookup"><span data-stu-id="8d0e8-121">How do I estimate my application's request unit needs?</span></span>
* <span data-ttu-id="8d0e8-122">Mi történik, ha szeretnék haladhatja meg a kérelem egység kapacitás gyűjtemény?</span><span class="sxs-lookup"><span data-stu-id="8d0e8-122">What happens if I exceed request unit capacity for a collection?</span></span>

<span data-ttu-id="8d0e8-123">Mivel Azure Cosmos DB több modellre adatbázis, fontos megjegyezni, hogy egy gyűjtemény/dokumentum API dokumentum, egy grafikonon/csomópont egy grafikonon API és a tábla/entitás tábla API hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-123">As Azure Cosmos DB is a multi-model database, it is important to note that we will refer to a collection/document for a document API, a graph/node for a graph API and a table/entity for table API.</span></span> <span data-ttu-id="8d0e8-124">Átviteli sebesség a dokumentum azt fogja generalize tároló/elem fogalmakra.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-124">Throughput this document we will generalize to the concepts of container/item.</span></span>

## <a name="request-units-and-request-charges"></a><span data-ttu-id="8d0e8-125">Kérelemegység és kérelem díjak</span><span class="sxs-lookup"><span data-stu-id="8d0e8-125">Request units and request charges</span></span>
<span data-ttu-id="8d0e8-126">Azure Cosmos-adatbázis által gyors és kiszámítható teljesítményt nyújt *foglalása* erőforrások teljesíteni kell az alkalmazás átviteli sebességére.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-126">Azure Cosmos DB delivers fast, predictable performance by *reserving* resources to satisfy your application's throughput needs.</span></span>  <span data-ttu-id="8d0e8-127">Alkalmazás betölteni, és a hozzáférési minták módosítása adott idő alatt, mert az Azure Cosmos DB lehetővé teszi könnyen növeléséhez vagy csökkentéséhez fenntartott átviteli sebesség érhető el, hogy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-127">Because application load and access patterns change over time, Azure Cosmos DB allows you to easily increase or decrease the amount of reserved throughput available to your application.</span></span>

<span data-ttu-id="8d0e8-128">Az Azure Cosmos DB fenntartott átviteli kérelem egység / másodperc feldolgozása tekintetében van megadva.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-128">With Azure Cosmos DB, reserved throughput is specified in terms of request units processing per second.</span></span> <span data-ttu-id="8d0e8-129">Az eltolásokat tekintheti kérelemegység átviteli pénznemként, amellyel meg *lefoglalni* az alkalmazás számára elérhető garantált kérelemegység másodpercenként egy mennyisége.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-129">You can think of request units as throughput currency, whereby you *reserve* an amount of guaranteed request units available to your application on per second basis.</span></span>  <span data-ttu-id="8d0e8-130">Minden Azure Cosmos DB - dokumentum írása, frissítése egy dokumentumot a lekérdezés végrehajtása - műveletet igényel, Processzor, memória és iops-érték.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-130">Each operation in Azure Cosmos DB - writing a document, performing a query, updating a document - consumes CPU, memory, and IOPS.</span></span>  <span data-ttu-id="8d0e8-131">Ez azt jelenti, hogy minden egyes művelet azt eredményezi azok háromszorosa egy *kell fizetni kérelem*, amelyhez van megadva *egységek kérelem*.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-131">That is, each operation incurs a *request charge*, which is expressed in *request units*.</span></span>  <span data-ttu-id="8d0e8-132">Ismertetése a tényezőket, amely hatással van a kérelem egység díjak, az alkalmazás átviteli követelményeket, valamint lehetővé teszi az alkalmazás futtatása a lehető leghatékonyabban költség.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-132">Understanding the factors which impact request unit charges, along with your application's throughput requirements, enables you to run your application as cost effectively as possible.</span></span> <span data-ttu-id="8d0e8-133">A lekérdezés explorer egyben a core lekérdezés teszteléséhez csodálatos eszköz.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-133">The query explorer is also a wonderful tool to test the core of a query.</span></span>

<span data-ttu-id="8d0e8-134">Azt javasoljuk, hogy Kezdésként tekintse meg az alábbi videót, ahol Aravind Ramachandran kérelemegység és a kiszámítható teljesítmény Azure Cosmos DB mutatja.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-134">We recommend getting started by watching the following video, where Aravind Ramachandran explains request units and predictable performance with Azure Cosmos DB.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a><span data-ttu-id="8d0e8-135">Adja meg a kérelem egység kapacitás az Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8d0e8-135">Specifying request unit capacity in Azure Cosmos DB</span></span>
<span data-ttu-id="8d0e8-136">Egy új gyűjteményt, táblázat vagy graph indításakor, akkor az itt megadott kérelemegység (RU / másodperc) másodpercenként kívánt foglalt.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-136">When starting a new collection, table or graph, you specify the number of request units per second (RU per second) you want reserved.</span></span> <span data-ttu-id="8d0e8-137">A létesített átviteli sebesség alapján, Azure Cosmos DB foglal le, a gyűjtemény üzemeltetésére fizikai partíciók és elágazást/rebalances adatok között partíciók növekedésével azt.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-137">Based on the provisioned throughput, Azure Cosmos DB allocates physical partitions to host your collection and splits/rebalances data across partitions as it grows.</span></span>

<span data-ttu-id="8d0e8-138">Azure Cosmos-adatbázis egy partíciókulcsot kell határozni, ha a gyűjtemény kiépítése 2500 kérést egységek vagy újabb szükséges.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-138">Azure Cosmos DB requires a partition key to be specified when a collection is provisioned with 2,500 request units or higher.</span></span> <span data-ttu-id="8d0e8-139">A partíciós kulcs is szükség van a gyűjtemény átviteli sebességét túl 2500 kérést egységek méretezése a jövőben.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-139">A partition key is also required to scale your collection's throughput beyond 2,500 request units in the future.</span></span> <span data-ttu-id="8d0e8-140">Ezért erősen ajánlott konfigurálása egy [partíciókulcs](partition-data.md) függetlenül a kezdeti átviteli tárolója létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-140">Therefore, it is highly recommended to configure a [partition key](partition-data.md) when creating a container regardless of your initial throughput.</span></span> <span data-ttu-id="8d0e8-141">Az adatok kell kell-e osztani több partíciót, szükség egy partíciós kulcs, amely rendelkezik egy nagy számosságot (több millió különböző értékeket 100), hogy a gyűjtemény/tábla/graph és a kérelmek is méretezhető egységesen Azure Cosmos DB kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-141">Since your data might have to be split across multiple partitions, it is necessary to pick a partition key that has a high cardinality (100 to millions of distinct values) so that your collection/table/graph and requests can be scaled uniformly by Azure Cosmos DB.</span></span> 

> [!NOTE]
> <span data-ttu-id="8d0e8-142">A partíciós kulcs, a logikai határ, és nem egy fizikai egy.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-142">A partition key is a logical boundary, and not a physical one.</span></span> <span data-ttu-id="8d0e8-143">Emiatt nem kell külön partíciókulcs-értékek számának korlátozása.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-143">Therefore, you do not need to limit the number of distinct partition key values.</span></span> <span data-ttu-id="8d0e8-144">Valójában célszerűbb értékűeknek több partíciós kulcs kisebb, mint Azure Cosmos DB rendelkezik további terheléselosztási beállításai.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-144">It is in fact better to have more distinct partition key values than less, as Azure Cosmos DB has more load balancing options.</span></span>

<span data-ttu-id="8d0e8-145">Íme egy kódrészletet 3000 kérelem egység / második .NET SDK használatával a gyűjtemény létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-145">Here is a code snippet for creating a collection with 3,000 request units per second using the .NET SDK:</span></span>

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

<span data-ttu-id="8d0e8-146">Azure Cosmos-adatbázis működik, a teljesítmény a Foglalás modellt.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-146">Azure Cosmos DB operates on a reservation model on throughput.</span></span> <span data-ttu-id="8d0e8-147">Ez azt jelenti, hogy kell fizetni az átviteli sebesség *fenntartott*, függetlenül attól, hogy átviteli mekkora aktívan *használt*.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-147">That is, you are billed for the amount of throughput *reserved*, regardless of how much of that throughput is actively *used*.</span></span> <span data-ttu-id="8d0e8-148">Az alkalmazás által könnyen méretezheti mennyisége fel és le terhelés, az adatok és a használati minták módosítása fenntartott RUs SDK-k, vagy használja a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-148">As your application's load, data, and usage patterns change you can easily scale up and down the amount of reserved RUs through SDKs or using the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="8d0e8-149">Minden gyűjtemény/tábla/graph hozzá vannak rendelve egy `Offer` Azure Cosmos DB, amelynek metaadatait a létesített átviteli sebesség erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-149">Each collection/table/graph are mapped to an `Offer` resource in Azure Cosmos DB, which has metadata about the provisioned throughput.</span></span> <span data-ttu-id="8d0e8-150">A megfelelő ajánlat erőforrás egy tároló keresése, akkor új átviteli értékű frissítése módosíthatja a kiosztott átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-150">You can change the allocated throughput by looking up the corresponding offer resource for a container, then updating it with the new throughput value.</span></span> <span data-ttu-id="8d0e8-151">Íme egy kódrészletet a módosítása a gyűjtemény átviteli 5 000 kérelemegység / második .NET SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-151">Here is a code snippet for changing the throughput of a collection to 5,000 request units per second using the .NET SDK:</span></span>

```csharp
// Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

<span data-ttu-id="8d0e8-152">Ha megváltoztatja az átviteli sebesség, nincs hatással a következő rendelkezésre állási, a tároló.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-152">There is no impact to the availability of your container when you change the throughput.</span></span> <span data-ttu-id="8d0e8-153">Az új fenntartott átviteli sebességet általában hatékony alkalmazásra, az új átviteli másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-153">Typically the new reserved throughput is effective within seconds on application of the new throughput.</span></span>

## <a name="request-unit-considerations"></a><span data-ttu-id="8d0e8-154">Kérelem egység kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="8d0e8-154">Request unit considerations</span></span>
<span data-ttu-id="8d0e8-155">Az Azure Cosmos DB tároló foglalása a kérelem egységek számának becslése, fontos figyelembe venni a következő változókat:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-155">When estimating the number of request units to reserve for your Azure Cosmos DB container, it is important to take the following variables into consideration:</span></span>

* <span data-ttu-id="8d0e8-156">**Konfigurációelem-méret**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-156">**Item size**.</span></span> <span data-ttu-id="8d0e8-157">Mivel mérete nő, olvasására vagy írására, növeli az adatok használt mértékegységet.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-157">As size increases the units consumed to read or write the data will also increase.</span></span>
* <span data-ttu-id="8d0e8-158">**Konfigurációelem-tulajdonság száma**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-158">**Item property count**.</span></span> <span data-ttu-id="8d0e8-159">A tulajdonság száma növekszik, megnöveli a feltételezve alapértelmezett indexelő levő összes tulajdonság írása egy dokumentum/csomópont/ntity használt mértékegységet.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-159">Assuming default indexing of all properties, the units consumed to write a document/node/ntity will increase as the property count increases.</span></span>
* <span data-ttu-id="8d0e8-160">**Adatkonzisztencia**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-160">**Data consistency**.</span></span> <span data-ttu-id="8d0e8-161">Az erős, vagy a kötött elavulási konzisztencia szintek használata esetén további egységek cikkek elolvasására fognak használni.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-161">When using data consistency levels of Strong or Bounded Staleness, additional units will be consumed to read items.</span></span>
* <span data-ttu-id="8d0e8-162">**Indexelt tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-162">**Indexed properties**.</span></span> <span data-ttu-id="8d0e8-163">Egy index házirendet minden egyes tároló meghatározza, hogy mely tulajdonságok indexelt alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-163">An index policy on each container determines which properties are indexed by default.</span></span> <span data-ttu-id="8d0e8-164">Az indexelt tulajdonságok számának korlátozása vagy engedélyezése a Lusta indexelő csökkentése érdekében a kérelem egység fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-164">You can reduce your request unit consumption by limiting the number of indexed properties or by enabling lazy indexing.</span></span>
* <span data-ttu-id="8d0e8-165">**A dokumentum indexelő**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-165">**Document indexing**.</span></span> <span data-ttu-id="8d0e8-166">Alapértelmezésben minden elem automatikusan indexelt kevesebb kérelemegység fog használni, ha nem kíván a elemek egy része az index.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-166">By default each item is automatically indexed, you will consume fewer request units if you choose not to index some of your items.</span></span>
* <span data-ttu-id="8d0e8-167">**Lekérdezési minták**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-167">**Query patterns**.</span></span> <span data-ttu-id="8d0e8-168">A lekérdezés összetettsége hatással van kérelem egységek művelet végrehajtásánál.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-168">The complexity of a query impacts how many Request Units are consumed for an operation.</span></span> <span data-ttu-id="8d0e8-169">A predikátum száma, a predikátum, leképezések, felhasználó által megadott függvények száma és mérete a forrás adatkészlet összes jellege befolyásolják a lekérdezési műveletek költségét.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-169">The number of predicates, nature of the predicates, projections, number of UDFs, and the size of the source data set all influence the cost of query operations.</span></span>
* <span data-ttu-id="8d0e8-170">**Parancsfájl-használati**.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-170">**Script usage**.</span></span>  <span data-ttu-id="8d0e8-171">Csakúgy, mint a lekérdezéseket, a tárolt eljárások és eseményindítók végrehajtott műveletek összetettsége alapján kérelemegység felhasználni.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-171">As with queries, stored procedures and triggers consume request units based on the complexity of the operations being performed.</span></span> <span data-ttu-id="8d0e8-172">Az alkalmazás elkészítéséhez, vizsgálja meg jobb megértése érdekében hogyan egyes műveletek nem használ-e kérelem egység kapacitás kérelemfejléc kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-172">As you develop your application, inspect the request charge header to better understand how each operation is consuming request unit capacity.</span></span>

## <a name="estimating-throughput-needs"></a><span data-ttu-id="8d0e8-173">Átviteli sebesség igények becslése</span><span class="sxs-lookup"><span data-stu-id="8d0e8-173">Estimating throughput needs</span></span>
<span data-ttu-id="8d0e8-174">A kérelem egység mérőszáma normalizált kérelemfeldolgozáshoz költség.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-174">A request unit is a normalized measure of request processing cost.</span></span> <span data-ttu-id="8d0e8-175">Egy egyetlen kérelem egységet jelöli a feldolgozási kapacitás egy egyetlen 1KB cikk álló (kivéve a rendszer tulajdonságai) 10 egyedi tulajdonságértékek (keresztül self link vagy azonosítója) olvasásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-175">A single request unit represents the processing capacity required to read (via self link or id) a single 1KB item consisting of 10 unique property values (excluding system properties).</span></span> <span data-ttu-id="8d0e8-176">A kérelem létrehozása (insert), cseréje vagy azonos elem törlése fogyaszt, több folyamatot, a szolgáltatásból, és ezáltal több kérelemegység.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-176">A request to create (insert), replace or delete the same item will consume more processing from the service and thereby more request units.</span></span>   

> [!NOTE]
> <span data-ttu-id="8d0e8-177">Az alapkonfiguráció 1 kérelem egység egy 1 KB cikk megfelel egy egyszerű GET self link vagy az elem azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-177">The baseline of 1 request unit for a 1KB item corresponds to a simple GET by self link or id of the item.</span></span>
> 
> 

<span data-ttu-id="8d0e8-178">Például egy táblázat következik hány kérelemegység kiépítését, három különböző elem mérete (1KB, 4 KB-os és 64 KB-os) és két különböző teljesítményszintek az itt található (500 olvasás/másodperc + 100 írási műveletek másodpercenkénti száma és 500 olvasás/másodperc + 500 írás/másodperc).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-178">For example, here's a table that shows how many request units to provision at three different item sizes (1KB, 4KB, and 64KB) and at two different performance levels (500 reads/second + 100 writes/second and 500 reads/second + 500 writes/second).</span></span> <span data-ttu-id="8d0e8-179">Az adatok konzisztenciájának a munkamenet lett konfigurálva, és az indexelési házirendet értékre lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-179">The data consistency was configured at Session, and the indexing policy was set to None.</span></span>

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-180"><strong>Elem mérete</strong></span><span class="sxs-lookup"><span data-stu-id="8d0e8-180"><strong>Item size</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-181"><strong>Olvasási/másodperc</strong></span><span class="sxs-lookup"><span data-stu-id="8d0e8-181"><strong>Reads/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-182"><strong>Írási műveletek másodpercenkénti száma</strong></span><span class="sxs-lookup"><span data-stu-id="8d0e8-182"><strong>Writes/second</strong></span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-183"><strong>Kérelemegységek</strong></span><span class="sxs-lookup"><span data-stu-id="8d0e8-183"><strong>Request units</strong></span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-184">1 KB</span><span class="sxs-lookup"><span data-stu-id="8d0e8-184">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-185">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-185">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-186">100</span><span class="sxs-lookup"><span data-stu-id="8d0e8-186">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-187">(500 * 1) + (100 * 5) = 1000 RU/mp</span><span class="sxs-lookup"><span data-stu-id="8d0e8-187">(500 * 1) + (100 * 5) = 1,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-188">1 KB</span><span class="sxs-lookup"><span data-stu-id="8d0e8-188">1 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-189">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-189">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-190">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-190">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-191">(500 * 1) + (500 * 5) = 3000 RU/mp</span><span class="sxs-lookup"><span data-stu-id="8d0e8-191">(500 * 1) + (500 * 5) = 3,000 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-192">4 KB-OS</span><span class="sxs-lookup"><span data-stu-id="8d0e8-192">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-193">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-193">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-194">100</span><span class="sxs-lookup"><span data-stu-id="8d0e8-194">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-195">(500 * 1,3) + (100 * 7) = 1,350 RU/mp</span><span class="sxs-lookup"><span data-stu-id="8d0e8-195">(500 * 1.3) + (100 * 7) = 1,350 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-196">4 KB-OS</span><span class="sxs-lookup"><span data-stu-id="8d0e8-196">4 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-197">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-197">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-198">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-198">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-199">(500 * 1,3) + (500 * 7) = 4,150 RU/mp</span><span class="sxs-lookup"><span data-stu-id="8d0e8-199">(500 * 1.3) + (500 * 7) = 4,150 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-200">64 KB</span><span class="sxs-lookup"><span data-stu-id="8d0e8-200">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-201">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-201">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-202">100</span><span class="sxs-lookup"><span data-stu-id="8d0e8-202">100</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-203">(500 * 10) + (100 * 48) = 9,800 RU/mp</span><span class="sxs-lookup"><span data-stu-id="8d0e8-203">(500 * 10) + (100 * 48) = 9,800 RU/s</span></span></p></td>
        </tr>
        <tr>
            <td valign="top"><p><span data-ttu-id="8d0e8-204">64 KB</span><span class="sxs-lookup"><span data-stu-id="8d0e8-204">64 KB</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-205">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-205">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-206">500</span><span class="sxs-lookup"><span data-stu-id="8d0e8-206">500</span></span></p></td>
            <td valign="top"><p><span data-ttu-id="8d0e8-207">(500 * 10) + (500 * 48) = 29,000 RU/mp</span><span class="sxs-lookup"><span data-stu-id="8d0e8-207">(500 * 10) + (500 * 48) = 29,000 RU/s</span></span></p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a><span data-ttu-id="8d0e8-208">A kérelem egység Számológép használata</span><span class="sxs-lookup"><span data-stu-id="8d0e8-208">Use the request unit calculator</span></span>
<span data-ttu-id="8d0e8-209">Vékony ügyfelek hangolja az átviteli sebesség becsléseket, és van egy webalapú [kérelem egység Számológép](https://www.documentdb.com/capacityplanner) használatával megbecsülheti a kérelem egységekre vonatkozó követelményeket a tipikus műveleteket, köztük:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-209">To help customers fine tune their throughput estimations, there is a web based [request unit calculator](https://www.documentdb.com/capacityplanner) to help estimate the request unit requirements for typical operations, including:</span></span>

* <span data-ttu-id="8d0e8-210">Elem hoz létre (írás)</span><span class="sxs-lookup"><span data-stu-id="8d0e8-210">Item creates (writes)</span></span>
* <span data-ttu-id="8d0e8-211">Elem beolvasása</span><span class="sxs-lookup"><span data-stu-id="8d0e8-211">Item reads</span></span>
* <span data-ttu-id="8d0e8-212">Elem törlése</span><span class="sxs-lookup"><span data-stu-id="8d0e8-212">Item deletes</span></span>
* <span data-ttu-id="8d0e8-213">Elem frissítések</span><span class="sxs-lookup"><span data-stu-id="8d0e8-213">Item updates</span></span>

<span data-ttu-id="8d0e8-214">Az eszköz az adattárolási igények kielégítésére megadta minta cikkek alapján megbecsülheti támogatását is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-214">The tool also includes support for estimating data storage needs based on the sample items you provide.</span></span>

<span data-ttu-id="8d0e8-215">Az eszköz használata egyszerű:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-215">Using the tool is simple:</span></span>

1. <span data-ttu-id="8d0e8-216">Töltsön fel egy vagy több jellemző elemet.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-216">Upload one or more representative items.</span></span>
   
    ![Töltse fel a kérelem egység Számológép elemek][2]
2. <span data-ttu-id="8d0e8-218">Adatok tárolási követelményeinek becslése, adja meg a várhatóan tárolni elemek összesített száma.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-218">To estimate data storage requirements, enter the total number of items you expect to store.</span></span>
3. <span data-ttu-id="8d0e8-219">Adja meg az elemek száma létrehozása, olvasása, frissítése és törlési műveletek (a másodpercenként alapján) van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-219">Enter the number of items create, read, update, and delete operations you require (on a per-second basis).</span></span> <span data-ttu-id="8d0e8-220">A kérelem egység díjak elem frissítési műveletek becsléséhez, töltse fel az 1. lépés a fenti tipikus mező frissítéseket tartalmazó minta elem másolatának.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-220">To estimate the request unit charges of item update operations, upload a copy of the sample item from step 1 above that includes typical field updates.</span></span>  <span data-ttu-id="8d0e8-221">Például ha elem frissítések általában két tulajdonságainak módosítása lastLogin és userVisits nevű majd egyszerűen a minta elem másolása, e két tulajdonság értéket, és töltse fel a másolt elem.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-221">For example, if item updates typically modify two properties named lastLogin and userVisits, then simply copy the sample item, update the values for those two properties, and upload the copied item.</span></span>
   
    ![Adja meg a kérelem egység Számológép átviteli követelmények][3]
4. <span data-ttu-id="8d0e8-223">Kattintson a kiszámításához, és vizsgálja meg az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-223">Click calculate and examine the results.</span></span>
   
    ![Kérelem egység Számológép eredményei][4]

> [!NOTE]
> <span data-ttu-id="8d0e8-225">Ha méretét és az indexelt tulajdonságok száma jelentősen eltérő típusú elemekre, majd töltse fel az egyes minta *típus* tipikus az eszköz elemet, és majd kiszámítja az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-225">If you have item types which will differ dramatically in terms of size and the number of indexed properties, then upload a sample of each *type* of typical item to the tool and then calculate the results.</span></span>
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a><span data-ttu-id="8d0e8-226">Használja az Azure Cosmos DB kérelem kell fizetni válaszfejléc</span><span class="sxs-lookup"><span data-stu-id="8d0e8-226">Use the Azure Cosmos DB request charge response header</span></span>
<span data-ttu-id="8d0e8-227">Minden válasz az Azure Cosmos DB szolgáltatástól tartalmaz egy egyéni fejlécet (`x-ms-request-charge`), amely tartalmazza a kéréshez használt kérelemegység.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-227">Every response from the Azure Cosmos DB service includes a custom header (`x-ms-request-charge`) that contains the request units consumed for the request.</span></span> <span data-ttu-id="8d0e8-228">Ezt a fejlécet az Azure Cosmos DB SDK-k keresztül is érhető el.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-228">This header is also accessible through the Azure Cosmos DB SDKs.</span></span> <span data-ttu-id="8d0e8-229">A .NET SDK RequestCharge a ResourceResponse objektum olyan osztályát.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-229">In the .NET SDK, RequestCharge is a property of the ResourceResponse object.</span></span>  <span data-ttu-id="8d0e8-230">A lekérdezések az Azure Cosmos DB lekérdezéskezelő az Azure portálon információival kérelem kell fizetni végrehajtott lekérdezések számára.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-230">For queries, the Azure Cosmos DB Query Explorer in the Azure portal provides request charge information for executed queries.</span></span>

![A lekérdezés Explorer RU díjak vizsgálata][1]

<span data-ttu-id="8d0e8-232">Ennek a szem előtt, megbecsülheti a fenntartott átviteli sebességet, az alkalmazás által igényelt mérete, jegyezze fel a kérelem egység kell fizetni társított tipikus műveleteket futtatott egy reprezentatív elem, amelyet az alkalmazás, és ezután műveletek számának becslése egy módszert, amelyek várhatóan másodpercenként végez.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-232">With this in mind, one method for estimating the amount of reserved throughput required by your application is to record the request unit charge associated with running typical operations against a representative item used by your application and then estimating the number of operations you anticipate performing each second.</span></span>  <span data-ttu-id="8d0e8-233">Győződjön meg arról, mérése és tipikus lekérdezések és Azure Cosmos DB parancsfájl használata is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-233">Be sure to measure and include typical queries and Azure Cosmos DB script usage as well.</span></span>

> [!NOTE]
> <span data-ttu-id="8d0e8-234">Ha méretét és az indexelt tulajdonságok száma jelentősen eltérő típusú elemekre, majd jegyezze fel a megfelelő műveletet kérelem egység kell fizetni társított minden egyes *típus* jellemző elem.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-234">If you have item types which will differ dramatically in terms of size and the number of indexed properties, then record the applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

<span data-ttu-id="8d0e8-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-235">For example:</span></span>

1. <span data-ttu-id="8d0e8-236">Jegyezze fel a kérelem egység költség (Beszúrás) létrehozásához egy jellemző elemet.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-236">Record the request unit charge of creating (inserting) a typical item.</span></span> 
2. <span data-ttu-id="8d0e8-237">Jegyezze fel a kérelem egység kell fizetni egy tipikus elem olvasása.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-237">Record the request unit charge of reading a typical item.</span></span>
3. <span data-ttu-id="8d0e8-238">Jegyezze fel a kérelem egység felelős egy tipikus elem frissítése.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-238">Record the request unit charge of updating a typical item.</span></span>
4. <span data-ttu-id="8d0e8-239">Jegyezze fel a kérelem egység kell fizetni tipikus, közös elem lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-239">Record the request unit charge of typical, common item queries.</span></span>
5. <span data-ttu-id="8d0e8-240">Jegyezze fel a kérelem egység ingyenesen elérhető bármely egyéni parancsfájlok (tárolt eljárások, eseményindítók, felhasználó által definiált függvények) használja ki az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="8d0e8-240">Record the request unit charge of any custom scripts (stored procedures, triggers, user-defined functions) leveraged by the application</span></span>
6. <span data-ttu-id="8d0e8-241">A megadott műveletek másodpercenkénti futtatásához várhatóan becsült száma szükséges kérelemegység kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-241">Calculate the required request units given the estimated number of operations you anticipate to run each second.</span></span>

### <span data-ttu-id="8d0e8-242"><a id="GetLastRequestStatistics"></a>A MongoDB GetLastRequestStatistics parancshoz használható API</span><span class="sxs-lookup"><span data-stu-id="8d0e8-242"><a id="GetLastRequestStatistics"></a>Use API for MongoDB's GetLastRequestStatistics command</span></span>
<span data-ttu-id="8d0e8-243">Mongodb-protokolltámogatással API támogatja egy egyéni parancs *getLastRequestStatistics*, a kérelem kell fizetni a megadott műveletek beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-243">API for MongoDB supports a custom command, *getLastRequestStatistics*, for retrieving the request charge for specified operations.</span></span>

<span data-ttu-id="8d0e8-244">Például a Mongo rendszerhéj hajtható végre a kérelem díjat ellenőrizni szeretné a műveletet.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-244">For example, in the Mongo Shell, execute the operation you want to verify the request charge for.</span></span>
```
> db.sample.find()
```

<span data-ttu-id="8d0e8-245">Ezután hajtsa végre a parancsot *getLastRequestStatistics*.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-245">Next, execute the command *getLastRequestStatistics*.</span></span>
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

<span data-ttu-id="8d0e8-246">Ennek a szem előtt, megbecsülheti a fenntartott átviteli sebességet, az alkalmazás által igényelt mérete, jegyezze fel a kérelem egység kell fizetni társított tipikus műveleteket futtatott egy reprezentatív elem, amelyet az alkalmazás, és ezután műveletek számának becslése egy módszert, amelyek várhatóan másodpercenként végez.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-246">With this in mind, one method for estimating the amount of reserved throughput required by your application is to record the request unit charge associated with running typical operations against a representative item used by your application and then estimating the number of operations you anticipate performing each second.</span></span>

> [!NOTE]
> <span data-ttu-id="8d0e8-247">Ha méretét és az indexelt tulajdonságok száma jelentősen eltérő típusú elemekre, majd jegyezze fel a megfelelő műveletet kérelem egység kell fizetni társított minden egyes *típus* jellemző elem.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-247">If you have item types which will differ dramatically in terms of size and the number of indexed properties, then record the applicable operation request unit charge associated with each *type* of typical item.</span></span>
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a><span data-ttu-id="8d0e8-248">MongoDB a portál metrikáihoz API használata</span><span class="sxs-lookup"><span data-stu-id="8d0e8-248">Use API for MongoDB's portal metrics</span></span>
<span data-ttu-id="8d0e8-249">A legegyszerűbben úgy beszerezni kérelem jó becslése egység költségek az API-MongoDB-adatbázist, hogy használja a [Azure-portálon](https://portal.azure.com) metrikákat.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-249">The simplest way to get a good estimation of request unit charges for your API for MongoDB database is to use the [Azure portal](https://portal.azure.com) metrics.</span></span> <span data-ttu-id="8d0e8-250">Az a *kérelem* és *kérelem kell fizetni* diagramokat, a kérelem egységek minden művelet nem használ-e, és hány kérelemegység használják ki egy másik viszonyítva felmérését kaphat.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-250">With the *Number of requests* and *Request Charge* charts, you can get an estimation of how many request units each operation is consuming and how many request units they consume relative to one another.</span></span>

![API-t a MongoDB portál metrikák][6]

## <a name="a-request-unit-estimation-example"></a><span data-ttu-id="8d0e8-252">A kérelem egység becslés – példa</span><span class="sxs-lookup"><span data-stu-id="8d0e8-252">A request unit estimation example</span></span>
<span data-ttu-id="8d0e8-253">Vegye figyelembe a következő ~ 1 KB méretű dokumentum:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-253">Consider the following ~1KB document:</span></span>

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
> <span data-ttu-id="8d0e8-254">Dokumentumok Azure Cosmos DB, a rendszer minified szűrést, a rendszer a dokumentum fenti mérete valamivel kisebb, mint 1KB.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-254">Documents are minified in Azure Cosmos DB, so the system calculated size of the document above is slightly less than 1KB.</span></span>
> 
> 

<span data-ttu-id="8d0e8-255">Az alábbi táblázat hozzávetőleges kérelem egység költségekkel ezt az elemet (a hozzávetőleges kérelem egység kell fizetni feltételezi, hogy a fiók konzisztencia szintje "Munkamenet", és minden elem automatikusan indexelt) jellemző műveleteket:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-255">The following table shows approximate request unit charges for typical operations on this item (the approximate request unit charge assumes that the account consistency level is set to “Session” and that all items are automatically indexed):</span></span>

| <span data-ttu-id="8d0e8-256">Művelet</span><span class="sxs-lookup"><span data-stu-id="8d0e8-256">Operation</span></span> | <span data-ttu-id="8d0e8-257">Egységköltség kérése</span><span class="sxs-lookup"><span data-stu-id="8d0e8-257">Request Unit Charge</span></span> |
| --- | --- |
| <span data-ttu-id="8d0e8-258">Konfigurációelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d0e8-258">Create item</span></span> |<span data-ttu-id="8d0e8-259">~ 15 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-259">~15 RU</span></span> |
| <span data-ttu-id="8d0e8-260">Elem olvasása</span><span class="sxs-lookup"><span data-stu-id="8d0e8-260">Read item</span></span> |<span data-ttu-id="8d0e8-261">~ 1 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-261">~1 RU</span></span> |
| <span data-ttu-id="8d0e8-262">Lekérdezés elem azonosítója</span><span class="sxs-lookup"><span data-stu-id="8d0e8-262">Query item by id</span></span> |<span data-ttu-id="8d0e8-263">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-263">~2.5 RU</span></span> |

<span data-ttu-id="8d0e8-264">Emellett az alábbi táblázatban hozzávetőleges kérést az alkalmazásban használt szokásos lekérdezések egység díjak:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-264">Additionally, this table shows approximate request unit charges for typical queries used in the application:</span></span>

| <span data-ttu-id="8d0e8-265">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="8d0e8-265">Query</span></span> | <span data-ttu-id="8d0e8-266">Egységköltség kérése</span><span class="sxs-lookup"><span data-stu-id="8d0e8-266">Request Unit Charge</span></span> | <span data-ttu-id="8d0e8-267">Visszaadott elemek száma</span><span class="sxs-lookup"><span data-stu-id="8d0e8-267"># of Returned Items</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8d0e8-268">Válassza ki a étele-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="8d0e8-268">Select food by id</span></span> |<span data-ttu-id="8d0e8-269">~2.5 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-269">~2.5 RU</span></span> |<span data-ttu-id="8d0e8-270">1</span><span class="sxs-lookup"><span data-stu-id="8d0e8-270">1</span></span> |
| <span data-ttu-id="8d0e8-271">Válassza ki a gyártó által élelmiszerek</span><span class="sxs-lookup"><span data-stu-id="8d0e8-271">Select foods by manufacturer</span></span> |<span data-ttu-id="8d0e8-272">~ 7 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-272">~7 RU</span></span> |<span data-ttu-id="8d0e8-273">7</span><span class="sxs-lookup"><span data-stu-id="8d0e8-273">7</span></span> |
| <span data-ttu-id="8d0e8-274">Jelöljön ki étele csoport és egy rendezési súlyozással</span><span class="sxs-lookup"><span data-stu-id="8d0e8-274">Select by food group and order by weight</span></span> |<span data-ttu-id="8d0e8-275">~ 70 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-275">~70 RU</span></span> |<span data-ttu-id="8d0e8-276">100</span><span class="sxs-lookup"><span data-stu-id="8d0e8-276">100</span></span> |
| <span data-ttu-id="8d0e8-277">Válassza ki a felső 10 élelmiszerek étele csoportban</span><span class="sxs-lookup"><span data-stu-id="8d0e8-277">Select top 10 foods in a food group</span></span> |<span data-ttu-id="8d0e8-278">~ 10 RU</span><span class="sxs-lookup"><span data-stu-id="8d0e8-278">~10 RU</span></span> |<span data-ttu-id="8d0e8-279">10</span><span class="sxs-lookup"><span data-stu-id="8d0e8-279">10</span></span> |

> [!NOTE]
> <span data-ttu-id="8d0e8-280">RU díjak visszaküldött elemek száma függően változhat.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-280">RU charges vary based on the number of items returned.</span></span>
> 
> 

<span data-ttu-id="8d0e8-281">Az információ azt megbecsülhető a RU-műveletek és a másodpercenként várhatóan lekérdezések száma alkalmazáshoz szükséges követelmények:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-281">With this information, we can estimate the RU requirements for this application given the number of operations and queries we expect per second:</span></span>

| <span data-ttu-id="8d0e8-282">A művelet/lekérdezés</span><span class="sxs-lookup"><span data-stu-id="8d0e8-282">Operation/Query</span></span> | <span data-ttu-id="8d0e8-283">Becsült száma másodpercenként</span><span class="sxs-lookup"><span data-stu-id="8d0e8-283">Estimated number per second</span></span> | <span data-ttu-id="8d0e8-284">Szükséges RUs</span><span class="sxs-lookup"><span data-stu-id="8d0e8-284">Required RUs</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8d0e8-285">Konfigurációelem létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d0e8-285">Create item</span></span> |<span data-ttu-id="8d0e8-286">10</span><span class="sxs-lookup"><span data-stu-id="8d0e8-286">10</span></span> |<span data-ttu-id="8d0e8-287">150</span><span class="sxs-lookup"><span data-stu-id="8d0e8-287">150</span></span> |
| <span data-ttu-id="8d0e8-288">Elem olvasása</span><span class="sxs-lookup"><span data-stu-id="8d0e8-288">Read item</span></span> |<span data-ttu-id="8d0e8-289">100</span><span class="sxs-lookup"><span data-stu-id="8d0e8-289">100</span></span> |<span data-ttu-id="8d0e8-290">100</span><span class="sxs-lookup"><span data-stu-id="8d0e8-290">100</span></span> |
| <span data-ttu-id="8d0e8-291">Válassza ki a gyártó által élelmiszerek</span><span class="sxs-lookup"><span data-stu-id="8d0e8-291">Select foods by manufacturer</span></span> |<span data-ttu-id="8d0e8-292">25</span><span class="sxs-lookup"><span data-stu-id="8d0e8-292">25</span></span> |<span data-ttu-id="8d0e8-293">175</span><span class="sxs-lookup"><span data-stu-id="8d0e8-293">175</span></span> |
| <span data-ttu-id="8d0e8-294">Válassza ki a étele csoport szerint</span><span class="sxs-lookup"><span data-stu-id="8d0e8-294">Select by food group</span></span> |<span data-ttu-id="8d0e8-295">10</span><span class="sxs-lookup"><span data-stu-id="8d0e8-295">10</span></span> |<span data-ttu-id="8d0e8-296">700</span><span class="sxs-lookup"><span data-stu-id="8d0e8-296">700</span></span> |
| <span data-ttu-id="8d0e8-297">Válassza ki a felső 10</span><span class="sxs-lookup"><span data-stu-id="8d0e8-297">Select top 10</span></span> |<span data-ttu-id="8d0e8-298">15</span><span class="sxs-lookup"><span data-stu-id="8d0e8-298">15</span></span> |<span data-ttu-id="8d0e8-299">150 összesen</span><span class="sxs-lookup"><span data-stu-id="8d0e8-299">150 Total</span></span> |

<span data-ttu-id="8d0e8-300">Ebben az esetben egy 1,275 RU/s átlagos átviteli sebességgel követelmény várhatóan.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-300">In this case, we expect an average throughput requirement of 1,275 RU/s.</span></span>  <span data-ttu-id="8d0e8-301">Kerekítése a legközelebbi 100, akár azt szeretné kiépítése 1300 RU/mp ezt az alkalmazást a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-301">Rounding up to the nearest 100, we would provision 1,300 RU/s for this application's collection.</span></span>

## <span data-ttu-id="8d0e8-302"><a id="RequestRateTooLarge"></a>Az Azure Cosmos Adatbázisba meghaladó fenntartott átviteli sebességének korlátai</span><span class="sxs-lookup"><span data-stu-id="8d0e8-302"><a id="RequestRateTooLarge"></a> Exceeding reserved throughput limits in Azure Cosmos DB</span></span>
<span data-ttu-id="8d0e8-303">Visszahívása, hogy kérelem egység fogyasztás kiértékelhető legyen másodpercenkénti, ha a keret üres.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-303">Recall that request unit consumption is evaluated as a rate per second if the budget is empty.</span></span> <span data-ttu-id="8d0e8-304">Olyan alkalmazások, amelyek mérete meghaladja a kiépített kérelmek egység aránya a tárolóhoz gyűjteményhez kérelmek kell halmozódni fog, amíg nem foglalt szint alatt esik száma másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-304">For applications that exceed the provisioned request unit rate for a container, requests to that collection will be throttled until the rate drops below the reserved level.</span></span> <span data-ttu-id="8d0e8-305">A szabályozási következik be, amikor a kiszolgáló megelőző jelleggel end RequestRateTooLargeException (HTTP-állapotkód: 429) a kérelmet, és térjen vissza a idő ezredmásodpercben, amely a felhasználó kell várnia, mielőtt megoldódhat jelző x-ms-újrapróbálkozási-után-ms-fejléc a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-305">When a throttle occurs, the server will preemptively end the request with RequestRateTooLargeException (HTTP status code 429) and return the x-ms-retry-after-ms header indicating the amount of time, in milliseconds, that the user must wait before reattempting the request.</span></span>

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

<span data-ttu-id="8d0e8-306">Ha a .NET SDK-ügyfél és a LINQ-lekérdezések, majd a legtöbbször ennek soha nem kell a .NET SDK-ügyfél aktuális verziója implicit módon ezt a választ ki, ez a kivétel kezelésére használ, tiszteletben tartja a kiszolgáló által megadott újrapróbálkozási után fejlécet, és újrapróbálkozik a a kérést.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-306">If you are using the .NET Client SDK and LINQ queries, then most of the time you never have to deal with this exception, as the current version of the .NET Client SDK implicitly catches this response, respects the server-specified retry-after header, and retries the request.</span></span> <span data-ttu-id="8d0e8-307">Kivéve, ha a fiók több ügyfélnek egyszerre használja, a következő újrapróbálkozási sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-307">Unless your account is being accessed concurrently by multiple clients, the next retry will succeed.</span></span>

<span data-ttu-id="8d0e8-308">Ha egynél több ügyfél összesítve felett a kérelmek aránya működő esetleg nem elegendő az alapértelmezett újrapróbálási viselkedése, és az ügyfél egy DocumentClientException állapotkóddal 429 kivételhibát az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-308">If you have more than one client cumulatively operating above the request rate, the default retry behavior may not suffice, and the client will throw a DocumentClientException with status code 429 to the application.</span></span> <span data-ttu-id="8d0e8-309">Például ez esetben érdemes újrapróbálási viselkedése és rutinok kezelése vagy a tároló a fenntartott átviteli sebesség növelése az alkalmazás hibás programot.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-309">In cases such as this, you may consider handling retry behavior and logic in your application's error handling routines or increasing the reserved throughput for the container.</span></span>

## <span data-ttu-id="8d0e8-310"><a id="RequestRateTooLargeAPIforMongoDB"></a>Mongodb-protokolltámogatással meghaladja a fenntartott átviteli sebességének korlátai API-ban.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-310"><a id="RequestRateTooLargeAPIforMongoDB"></a> Exceeding reserved throughput limits in API for MongoDB</span></span>
<span data-ttu-id="8d0e8-311">Alkalmazások, amelyek mérete meghaladja a kiosztott kérelemegység gyűjtemény halmozódni fog, amíg a sebesség esik fenntartott szint alatt.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-311">Applications that exceed the provisioned request units for a collection will be throttled until the rate drops below the reserved level.</span></span> <span data-ttu-id="8d0e8-312">A szabályozási esetén a háttér megelőző jelleggel véget ér a kérelmet egy *16500* hibakód - *túl sok kérelem*.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-312">When a throttle occurs, the backend will preemptively end the request with a *16500* error code - *Too Many Requests*.</span></span> <span data-ttu-id="8d0e8-313">Alapértelmezés szerint API-t a MongoDB automatikusan megpróbálja legfeljebb 10-szer visszatérése előtt egy *túl sok kérelem* hibakód.</span><span class="sxs-lookup"><span data-stu-id="8d0e8-313">By default, API for MongoDB will automatically retry up to 10 times before returning a *Too Many Requests* error code.</span></span> <span data-ttu-id="8d0e8-314">Ha sok kap *túl sok kérelem* hibakódok, akkor fontolja meg vagy hozzáadását újrapróbálási viselkedése a az alkalmazás hibakezelési rutinok vagy [a gyűjteményafenntartottátvitelisebességnövelése](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-314">If you are receiving many *Too Many Requests* error codes, you may consider either adding retry behavior in your application's error handling routines or [increasing the reserved throughput for the collection](set-throughput.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d0e8-315">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d0e8-315">Next steps</span></span>
<span data-ttu-id="8d0e8-316">További információt az Azure Cosmos DB adatbázisok fenntartott átviteli sebességet, ismerheti meg ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="8d0e8-316">To learn more about reserved throughput with Azure Cosmos DB databases, explore these resources:</span></span>

* [<span data-ttu-id="8d0e8-317">Azure-beli árakról Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8d0e8-317">Azure Cosmos DB pricing</span></span>](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [<span data-ttu-id="8d0e8-318">Az Azure Cosmos Adatbázisba az adatok particionálása</span><span class="sxs-lookup"><span data-stu-id="8d0e8-318">Partitioning data in Azure Cosmos DB</span></span>](partition-data.md)

<span data-ttu-id="8d0e8-319">Azure Cosmos DB kapcsolatos további tudnivalókért tekintse meg az Azure Cosmos DB [dokumentáció](https://azure.microsoft.com/documentation/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-319">To learn more about Azure Cosmos DB, see the Azure Cosmos DB [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/).</span></span> 

<span data-ttu-id="8d0e8-320">Első lépésként a méretezés és teljesítmény Azure Cosmos DB tesztelést, lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md).</span><span class="sxs-lookup"><span data-stu-id="8d0e8-320">To get started with scale and performance testing with Azure Cosmos DB, see [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md).</span></span>

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
