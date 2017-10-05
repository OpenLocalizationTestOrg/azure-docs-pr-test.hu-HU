---
title: "Az Azure CosmosDB: Kérelem egység / perc (RU/m) |} Microsoft Docs"
description: "Útmutató a költségek csökkentése a kérelemegység / perc felügyelniük."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 72d60d5656ad664e42a848fc9b372cb09e49b888
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a><span data-ttu-id="d440f-103">Az Azure Cosmos Adatbázisba percenként kérelemegység</span><span class="sxs-lookup"><span data-stu-id="d440f-103">Request units per minute in Azure Cosmos DB</span></span>

<span data-ttu-id="d440f-104">Azure Cosmos DB célja segítséget nyújtanak a gyors és kiszámítható teljesítményt és a skála zökkenőmentesen együtt az alkalmazás növekedés elérése.</span><span class="sxs-lookup"><span data-stu-id="d440f-104">Azure Cosmos DB is designed to help you achieve a fast, predictable performance and scale seamlessly along with your application’s growth.</span></span> <span data-ttu-id="d440f-105">Megadhat egy Cosmos DB tároló mindkét, másodpercenként és perc (RU/m) Granularitás van az átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="d440f-105">You can provision throughput on a Cosmos DB container at both, per-second and at per-minute (RU/m) granularities.</span></span> <span data-ttu-id="d440f-106">A percalapú granularitással kiosztott átviteli sebességet a számítási feladatban történő váratlan, másodpercalapú granularitású kiugrások kezelésére használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d440f-106">The provisioned throughput at per-minute granularity is used to manage unexpected spikes in the workload occurring at a per-second granularity.</span></span> 

<span data-ttu-id="d440f-107">Ez a cikk áttekintést nyújt a kérelemegység (RU/m) percenkénti létesítését működését.</span><span class="sxs-lookup"><span data-stu-id="d440f-107">This article provides an overview of how the provisioning of Request Unit per Minute (RU/m) works.</span></span> <span data-ttu-id="d440f-108">A szolgáltatáskiépítéssel RU/m figyelembe célja a kiszámítható teljesítményt biztosít, előre nem látható igényeinek (különösen akkor, ha a működési adatok felett analytics futtatásához szükséges) és spiky munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="d440f-108">The goal in mind with provisioning of RU/m is to provide a predictable performance around unpredictable needs (especially if you need to run analytics on top of your operational data) and spiky workloads.</span></span> <span data-ttu-id="d440f-109">Azt szeretné ügyfeleink azok kiépítéséhez, így lehessen őket méretezni gyorsan nyugodt maradhat afelől, a további átviteli felhasználását.</span><span class="sxs-lookup"><span data-stu-id="d440f-109">We want to have our customers consume more the throughput they provision so they can scale quickly with peace of mind.</span></span>

<span data-ttu-id="d440f-110">A cikk elolvasása után lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="d440f-110">After reading this article, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="d440f-111">Hogyan működik a kérelemegység / perc?</span><span class="sxs-lookup"><span data-stu-id="d440f-111">How does a Request Unit per Minute work?</span></span>
* <span data-ttu-id="d440f-112">Mi a különbség a kérelemegység / perc és kérelemegység / másodperc között?</span><span class="sxs-lookup"><span data-stu-id="d440f-112">What is the difference between Request Unit per Minute and Request Unit per Second?</span></span>
* <span data-ttu-id="d440f-113">Hogyan lehet kiépíteni RU/m?</span><span class="sxs-lookup"><span data-stu-id="d440f-113">How to provision RU/m?</span></span>
* <span data-ttu-id="d440f-114">A mely esetekben kell figyelembe venni kérelemegység / perc kiépítés?</span><span class="sxs-lookup"><span data-stu-id="d440f-114">Under which scenario shall I consider provisioning Request Unit per Minute?</span></span>
* <span data-ttu-id="d440f-115">Hogyan használható a portál metrikákat a költségeket és a teljesítmény optimalizálása érdekében?</span><span class="sxs-lookup"><span data-stu-id="d440f-115">How to use the portal metrics to optimize my cost and performance?</span></span>
* <span data-ttu-id="d440f-116">Adja meg, milyen típusú kérelmek felhasználhat a RU/m költségvetést?</span><span class="sxs-lookup"><span data-stu-id="d440f-116">Define which type of request can consume your RU/m budget?</span></span>

## <a name="provisioning-request-units-per-minute-rum"></a><span data-ttu-id="d440f-117">Üzembe helyezési kérelemegység / perc (RU/m)</span><span class="sxs-lookup"><span data-stu-id="d440f-117">Provisioning request units per minute (RU/m)</span></span>

<span data-ttu-id="d440f-118">Ha Azure Cosmos DB a második részletességű (RU/mp), a garancia a kérelmet egy kis késés: sikeres, ha az átviteli sebesség nem haladja meg a kapacitás kiosztása belül, hogy a második kap.</span><span class="sxs-lookup"><span data-stu-id="d440f-118">When you provision Azure Cosmos DB at the second granularity (RU/s), you get the guarantee that your request succeeds at a low latency if your throughput has not exceeded the capacity provisioned within that second.</span></span> <span data-ttu-id="d440f-119">A granularitási RU/m, a perc, a garancia a kérelem sikeres adott percen belül van.</span><span class="sxs-lookup"><span data-stu-id="d440f-119">With RU/m, the granularity is at the minute with the guarantee that your request succeeds within that minute.</span></span> <span data-ttu-id="d440f-120">Teljesítménynövelés érdekében rendszerek képest, azt győződjön meg arról, hogy a kapott teljesítmény kiszámítható és megtervezheti rajta.</span><span class="sxs-lookup"><span data-stu-id="d440f-120">Compared to bursting systems, we make sure that the performance you get is predictable and you can plan on it.</span></span>

<span data-ttu-id="d440f-121">Kiépítés works percenként módja egyszerű:</span><span class="sxs-lookup"><span data-stu-id="d440f-121">The way per minute provisioning works is simple:</span></span>

* <span data-ttu-id="d440f-122">RU/m lesz számlázva, óránként és RU/mp mellett.</span><span class="sxs-lookup"><span data-stu-id="d440f-122">RU/m is billed hourly and in addition to RU/s.</span></span> <span data-ttu-id="d440f-123">További részletekért tekintse meg az Azure Cosmos DB [árképzést ismertető oldalra](https://aka.ms/acdbpricing).</span><span class="sxs-lookup"><span data-stu-id="d440f-123">For more details, please visit Azure Cosmos DB [pricing page](https://aka.ms/acdbpricing).</span></span>
* <span data-ttu-id="d440f-124">RU/m a gyűjtemény szintjén engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="d440f-124">RU/m can be enabled at collection level.</span></span> <span data-ttu-id="d440f-125">Amely a keresztül az SDK-k (Node.js, Java vagy .net) vagy a portálon keresztül végezhető (a MongoDB API munkaterhelések is között)</span><span class="sxs-lookup"><span data-stu-id="d440f-125">That can be done through the SDKs (Node.js, Java, or .Net) or through the portal (also include MongoDB API workloads)</span></span>
* <span data-ttu-id="d440f-126">Ha RU/m engedélyezve van, minden 100 RU/mp üzembe helyezve, is elérhetővé 1000 RU/m kiépítve (arány, 10 x)</span><span class="sxs-lookup"><span data-stu-id="d440f-126">When RU/m is enabled, for every 100 RU/s provisioned, you also get 1,000 RU/m provisioned (the ratio is 10x)</span></span>
* <span data-ttu-id="d440f-127">Egy adott második, egy kérelem egységet használ fel a RU/m kiosztást, csak ha túllépte a belül, hogy a második második létesítési</span><span class="sxs-lookup"><span data-stu-id="d440f-127">At a given second, a request unit consumes your RU/m provisioning only if you have exceeded your per second provisioning within that second</span></span>
* <span data-ttu-id="d440f-128">A 60 másodperc időszak (szerint UTC) leteltével a percenként kiépítés van tölteni</span><span class="sxs-lookup"><span data-stu-id="d440f-128">Once the 60-second period (UTC) ends, the per minute provisioning is refilled</span></span>
* <span data-ttu-id="d440f-129">RU/m csak a gyűjteményeket, amelyek partíciónként 5000 RU/mp maximális létesítési engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="d440f-129">RU/m can be enabled only for collections with a maximum provisioning of 5,000 RU/s per partition.</span></span> <span data-ttu-id="d440f-130">Ha méretezhető átviteli igényeinek, és ilyen magas szintű partíciónként kiépítés, egy figyelmeztető üzenetet fog kapni</span><span class="sxs-lookup"><span data-stu-id="d440f-130">If you scale your throughput needs and have such a high level of provisioning per partition, you will get a warning message</span></span>

<span data-ttu-id="d440f-131">Az alábbiakban egy konkrét példában, amelyben az ügyfél építhető ki 10kRU/s-100kRU/m, menti az 73 % elleni RU/mp és 100 000 RU/m kiépített csúcsidőre (a 50kRU/mp) keresztül egy 90 másodperc után egy gyűjteményt, amely 10 000-re a kiépítés költséggel jár:</span><span class="sxs-lookup"><span data-stu-id="d440f-131">Below is a concrete example, in which a customer can provision 10kRU/s with 100kRU/m, saving 73% in cost against provisioning for peak (at 50kRU/sec) through a 90-second period on a collection that has 10,000 RU/s and 100,000 RU/m provisioned:</span></span>

* <span data-ttu-id="d440f-132">1. a második: 100 000 a RU/m költségvetésnek megfelelően van beállítva</span><span class="sxs-lookup"><span data-stu-id="d440f-132">1st second: The RU/m budget is set at 100,000</span></span>
* <span data-ttu-id="d440f-133">3. második: adott másodpercen belül a kérelemegység fogyasztását lett 11,010 RUs, 1,010 RUs kiépítési RU/mp feletti.</span><span class="sxs-lookup"><span data-stu-id="d440f-133">3rd second: During that second the consumption of Request Unit was 11,010 RUs, 1,010 RUs above the RU/s provisioning.</span></span> <span data-ttu-id="d440f-134">Ezért 1,010 RUs a RU/m költségvetés vonni.</span><span class="sxs-lookup"><span data-stu-id="d440f-134">Therefore, 1,010 RUs are deducted from the RU/m budget.</span></span> <span data-ttu-id="d440f-135">A következő 57 másodpercig RU/m költségvetési 98,990 RUs érhetők el</span><span class="sxs-lookup"><span data-stu-id="d440f-135">98,990 RUs are available for the next 57 seconds in the RU/m budget</span></span>
* <span data-ttu-id="d440f-136">29 második: adott másodpercen belül nagy csúcs történt (> 4 x nagyobb, mint a kiosztás, másodpercenként) és a kérelemegység fogyasztását 46,920 RUs volt.</span><span class="sxs-lookup"><span data-stu-id="d440f-136">29th second: During that second, a large spike happened (>4x higher than provisioning per second) and the consumption of Request Unit was 46,920 RUs.</span></span> <span data-ttu-id="d440f-137">36,920 RUs vonni a RU/m költségvetést, eldobni a 92,323 RUs (28 másodperc) való 55,403 RUs (29 másodperc)</span><span class="sxs-lookup"><span data-stu-id="d440f-137">36,920 RUs are deducted from the RU/m budget that dropped from 92,323 RUs (28th second) to 55,403 RUs (29th second)</span></span>
* <span data-ttu-id="d440f-138">61st második: RU/m költségvetési visszavált 100 000 RUs.</span><span class="sxs-lookup"><span data-stu-id="d440f-138">61st second: RU/m budget is set back to 100,000 RUs.</span></span>
 
![Megjeleníti a felhasználás és Azure Cosmos DB kiépítését diagramhoz](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a><span data-ttu-id="d440f-140">Kérelem egység kapacitás RU/m megadása</span><span class="sxs-lookup"><span data-stu-id="d440f-140">Specifying request unit capacity with RU/m</span></span>

<span data-ttu-id="d440f-141">Amikor egy Azure Cosmos DB gyűjteményt hoz létre, akkor az itt megadott kérelemegység (RU / másodperc) másodpercenként kívánt fenntartott ahhoz a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="d440f-141">When creating an Azure Cosmos DB collection, you specify the number of request units per second (RU per second) you want reserved for the collection.</span></span> <span data-ttu-id="d440f-142">Eldöntheti azt is, ha hozzáadandó RU / perc.</span><span class="sxs-lookup"><span data-stu-id="d440f-142">You can also decide if you want to add RU per minute.</span></span> <span data-ttu-id="d440f-143">Ezt megteheti a portál vagy az SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="d440f-143">This can be done through the Portal or the SDK.</span></span> 

### <a name="through-the-portal"></a><span data-ttu-id="d440f-144">A portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="d440f-144">Through the Portal</span></span>

<span data-ttu-id="d440f-145">Engedélyezés vagy letiltás RU / perc egyszerűen igényel a kattintson egy gyűjtemény létesítésekor.</span><span class="sxs-lookup"><span data-stu-id="d440f-145">Enabling or disabling RU per minute simply requires a click when provisioning a collection.</span></span> 

 ![Képernyőfelvétel: RU/m beállítása az Azure-portálon](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a><span data-ttu-id="d440f-147">Az SDK használatával</span><span class="sxs-lookup"><span data-stu-id="d440f-147">Through the SDK</span></span>
<span data-ttu-id="d440f-148">Először Ez azért fontos, vegye figyelembe, hogy RU/m csak a következő SDK-k érhető el:</span><span class="sxs-lookup"><span data-stu-id="d440f-148">First, this is important to note that RU/m is only available for the following SDKs:</span></span>

* <span data-ttu-id="d440f-149">.NET 1.14.0</span><span class="sxs-lookup"><span data-stu-id="d440f-149">.Net 1.14.0</span></span>
* <span data-ttu-id="d440f-150">Java 1.11.0</span><span class="sxs-lookup"><span data-stu-id="d440f-150">Java 1.11.0</span></span>
* <span data-ttu-id="d440f-151">NODE.js 1.12.0</span><span class="sxs-lookup"><span data-stu-id="d440f-151">Node.js 1.12.0</span></span>
* <span data-ttu-id="d440f-152">Python 2.2.0</span><span class="sxs-lookup"><span data-stu-id="d440f-152">Python 2.2.0</span></span>

<span data-ttu-id="d440f-153">Íme egy kódrészletet 3000 kérelem egység / második és 30 000 kérelemegység / perc, a .NET SDK használatával a gyűjtemény létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="d440f-153">Here is a code snippet for creating a collection with 3,000 request units per second and 30,000 request units per minute using the .NET SDK:</span></span>

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

<span data-ttu-id="d440f-154">Íme egy kódrészletet a gyűjtemény átviteli módosítása a 5000 kérelem egység / másodperc nélkül létesítési RU / perc, a .NET SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="d440f-154">Here is a code snippet for changing the throughput of a collection to 5,000 request units per second without provisioning RU per minute using the .NET SDK:</span></span>

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a><span data-ttu-id="d440f-155">Jó elférjen forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d440f-155">Good fit scenarios</span></span>

<span data-ttu-id="d440f-156">Ebben a szakaszban vannak remekül beválik, ha engedélyezi a kérelemegység / perc forgatókönyvek áttekintése a Microsoft biztosítja.</span><span class="sxs-lookup"><span data-stu-id="d440f-156">In this section, we provide an overview of scenarios that are a good fit for enabling request units per minute.</span></span>

<span data-ttu-id="d440f-157">**Fejlesztési/tesztelési környezetben:** remekül beválik.</span><span class="sxs-lookup"><span data-stu-id="d440f-157">**Dev/Test environment:** Good fit.</span></span> <span data-ttu-id="d440f-158">A fejlesztési szakasza alatt Ha teszteli az alkalmazás a különböző terhelésekhez RU/m a rugalmasságot biztosítanak ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d440f-158">During the development stage, if you are testing your application with different workloads, RU/m can provide the flexibility at this stage.</span></span> <span data-ttu-id="d440f-159">Amíg a [emulátor](local-emulator.md) teszteléséhez Azure Cosmos DB szabad kiváló eszköz.</span><span class="sxs-lookup"><span data-stu-id="d440f-159">While the [emulator](local-emulator.md) is a great free tool to test Azure Cosmos DB.</span></span> <span data-ttu-id="d440f-160">Azonban ha azt szeretné, a egy felhőalapú környezetben indul el, hogy egy rugalmas lehetőségeket biztosítanak a RU/m az ad hoc teljesítmény igényeinek.</span><span class="sxs-lookup"><span data-stu-id="d440f-160">However if you want to start in a cloud environment, you will have a great flexibility with RU/m for your adhoc performance needs.</span></span> <span data-ttu-id="d440f-161">Rendszer időt további fejleszt, először a teljesítményigény kevesebb vagyonának.</span><span class="sxs-lookup"><span data-stu-id="d440f-161">You will spend more time developing, less worrying about performance needs at first.</span></span> <span data-ttu-id="d440f-162">Azt javasoljuk, hogy a minimális RU/mp-kiépítés kezdve és RU/m engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d440f-162">We recommend starting with the minimum RU/s provisioning and enable RU/m.</span></span>

<span data-ttu-id="d440f-163">**Előre nem látható, spiky, percben granularitási kell:** jó elférjen – megtakarítások: 25-75 %-át.</span><span class="sxs-lookup"><span data-stu-id="d440f-163">**Unpredictable, spiky, minute granularity needs:** Good fit – Savings: 25-75%.</span></span> <span data-ttu-id="d440f-164">Úgy találtuk, hogy a csoportba vannak nagy fokozása RU/m és a legtöbb éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d440f-164">We have seen large improvement from RU/m and most production scenarios are into that group.</span></span> <span data-ttu-id="d440f-165">Ha egy IoT-terhelést, csúcs rendelkezik néhány alkalommal be egy perc alatt, ha még ha a rendszer tömeges beszúrás hajt végre egy időben futó lekérdezésekhez, szüksége lesz az handeling spiky igényeinek megfelelően további kapacitást.</span><span class="sxs-lookup"><span data-stu-id="d440f-165">If you have an IoT workload that has spike a few times in a minute, if you have queries running when your system makes mass insert at the same time, you will need extra capacity for handeling spiky needs.</span></span> <span data-ttu-id="d440f-166">Azt javasoljuk, hogy az erőforrás igényeinek optimalizálás az alábbi részletes módszer alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="d440f-166">We recommend optimizing your resource needs by applying our step by step approach below.</span></span>

 ![5 perc granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 <span data-ttu-id="d440f-168">*Ábra - RU fogyasztás teljesítményteszt*</span><span class="sxs-lookup"><span data-stu-id="d440f-168">*Figure - RU consumption benchmark*</span></span>

<span data-ttu-id="d440f-169">**Nyugodt maradhat afelől,:** jó elférjen – megtakarítások: 10-20 %-át.</span><span class="sxs-lookup"><span data-stu-id="d440f-169">**Peace of mind:** Good fit – Savings: 10-20%.</span></span> <span data-ttu-id="d440f-170">Egyes esetekben csak szeretné nyugodt maradhat afelől, és nem kell foglalkoznia lehetséges csúcsait és sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="d440f-170">Sometimes, you just want to have peace of mind and not worry about potential peaks and throttling.</span></span> <span data-ttu-id="d440f-171">Ez a funkció meg a megfelelőt.</span><span class="sxs-lookup"><span data-stu-id="d440f-171">This feature is the right one for you.</span></span> <span data-ttu-id="d440f-172">Ebben az esetben ajánlott, RU/m engedélyezése és némileg csökken a második létesítési.</span><span class="sxs-lookup"><span data-stu-id="d440f-172">In that case, we recommend enabling RU/m and slightly lower your per second provisioning.</span></span> <span data-ttu-id="d440f-173">Ebben az esetben eltér a fent, nem próbálkozik agresszív a kiépítés optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d440f-173">This case is different from the above as you will not try to optimize aggressively your provisioning.</span></span> <span data-ttu-id="d440f-174">Ez az a "Nulla sávszélesség-szabályozás" alaposan több.</span><span class="sxs-lookup"><span data-stu-id="d440f-174">This is more of a “Zero Throttling” mindset you are in.</span></span>

<span data-ttu-id="d440f-175">Ad hoc igényekkel végrehajtott kulcsfontosságú műveleteket: néha javasoljuk, hogy csak a kritikus műveletek RU/m költségvetést, így a keret nem hozzáférést ad hoc vagy kevésbé fontos operations felhasználását.</span><span class="sxs-lookup"><span data-stu-id="d440f-175">Critical operations with adhoc needs: We sometimes recommend to only let critical operations access RU/m budget so the budget doesn’t get consume by adhoc or less important operations.</span></span> <span data-ttu-id="d440f-176">Amely könnyen határozható meg az alábbi részben.</span><span class="sxs-lookup"><span data-stu-id="d440f-176">That can be easily defined in the section below.</span></span>

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a><span data-ttu-id="d440f-177">A portál metrikák segítségével optimalizálhatja a költségeket és a teljesítmény</span><span class="sxs-lookup"><span data-stu-id="d440f-177">Using the portal metrics to optimize cost and performance</span></span>

<span data-ttu-id="d440f-178">**Az elkövetkező hetektől azt továbbfejleszti a tartalom körül figyelési RUs perces használat optimalizálása átviteli igényeinek.**</span><span class="sxs-lookup"><span data-stu-id="d440f-178">**In the coming weeks, we will further develop the content around monitoring RUs minute consumption to optimize your throughput needs.**</span></span>

<span data-ttu-id="d440f-179">A portál metrikák keresztül tekintheti meg mennyi RU és felhasznált rendszeres RU másodperc perc.</span><span class="sxs-lookup"><span data-stu-id="d440f-179">Through the portal metrics, you can see how much of regular RU seconds you consume versus RU minutes.</span></span> <span data-ttu-id="d440f-180">Figyelési metrikákat segítségül a kiépítés optimalizálását.</span><span class="sxs-lookup"><span data-stu-id="d440f-180">Monitoring these metrics should help you optimize your provisioning.</span></span> 

<span data-ttu-id="d440f-181">Azt javasoljuk, hogy a részletes megközelítés előnyére RU/m használatával.</span><span class="sxs-lookup"><span data-stu-id="d440f-181">We recommend a step by step approach on how to use RU/m to your advantage.</span></span> <span data-ttu-id="d440f-182">Minden egyes lépést, rendelkeznie kell a a teljes ciklus a számítási feladatok (lehet óra, nap, vagy akár hét) jelölő RU fogyasztás áttekintése és elemzések lekérése mi kiépítése felhasználásáról.</span><span class="sxs-lookup"><span data-stu-id="d440f-182">For each step, you should have an overview of the RU consumption representing a full cycle of your workload (it could be hours, days, or even weeks) and get insights on the utilization of what you provision.</span></span>

<span data-ttu-id="d440f-183">Ez a megközelítés mögött lényege az átviteli sebesség kiépítés egy üzembe helyezési pont, amely megfelel az alábbi teljesítmény feltételek a lehető legközelebb végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d440f-183">The principle behind this approach is to make your throughput provisioning as close as possible to a provisioning point that matches your performance criteria below.</span></span> 

![5 perces granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
<span data-ttu-id="d440f-185">Szeretné megtudni, a munkaterhelés optimális üzembe helyezési pont, tisztában kell:</span><span class="sxs-lookup"><span data-stu-id="d440f-185">To understand the optimal provisioning point for your workload, you need to understand:</span></span>

* <span data-ttu-id="d440f-186">Felhasználási minták: nincs, alkalomszerű vagy tartós igényeiben jelentkező?</span><span class="sxs-lookup"><span data-stu-id="d440f-186">Consumption patterns: no, infrequent or sustained spikes?</span></span> <span data-ttu-id="d440f-187">(2 x átlagos) kis, közepes vagy nagy (> 10 x átlagos) igényeiben jelentkező?</span><span class="sxs-lookup"><span data-stu-id="d440f-187">Small (2x average), medium, or large (>10x average) spikes?</span></span>
* <span data-ttu-id="d440f-188">A szabályozottan halmozott kérelmek százalékos: tegye úgy érzi, hogy kényelmes Ha szabályozáshoz bit van?</span><span class="sxs-lookup"><span data-stu-id="d440f-188">Percent of throttled requests: do you feel comfortable if you have a bit of throttling?</span></span> <span data-ttu-id="d440f-189">Ha igen, hogy mekkora által?</span><span class="sxs-lookup"><span data-stu-id="d440f-189">If so, by how much?</span></span> 

<span data-ttu-id="d440f-190">Miután azonosította a célok van, lesz optimális kiépítési esélyét.</span><span class="sxs-lookup"><span data-stu-id="d440f-190">Once you have identified what your goals are, you will be able to get closer to the optimal provisioning.</span></span>

<span data-ttu-id="d440f-191">Segít, azt szeretnénk, hogy általános útmutatást a kiépítés a RU/m fogyasztás alapján optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d440f-191">To assist you, we want to provide an overall guidance on how to optimize your provisioning based on your RU/m consumption.</span></span> <span data-ttu-id="d440f-192">Ez az útmutató minden jellegű munkaterheléseket nem alkalmazható, de a private Preview verziójára Tudásbázis alapul.</span><span class="sxs-lookup"><span data-stu-id="d440f-192">This guidance doesn’t apply to all kind of workloads but is based on the private preview knowledge.</span></span> <span data-ttu-id="d440f-193">Ilyen alaptervek előfordulhat, hogy módosítjuk, hogy további:</span><span class="sxs-lookup"><span data-stu-id="d440f-193">We might change such baselines as we learn more:</span></span>

|<span data-ttu-id="d440f-194">RU/m kihasználtsága (%)</span><span class="sxs-lookup"><span data-stu-id="d440f-194">RU/m % utilization</span></span>|<span data-ttu-id="d440f-195">Bizonyos fokú RU/m kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="d440f-195">Degree of utilization of RU/m</span></span>|<span data-ttu-id="d440f-196">Javasolt művelet történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="d440f-196">Recommended actions for provisioning</span></span>|
|---|---|---|
|<span data-ttu-id="d440f-197">0-1%</span><span class="sxs-lookup"><span data-stu-id="d440f-197">0-1%</span></span>|<span data-ttu-id="d440f-198">A kihasználtság</span><span class="sxs-lookup"><span data-stu-id="d440f-198">Under utilization</span></span>|<span data-ttu-id="d440f-199">További RU/m felhasználásához alacsonyabb RU/mp</span><span class="sxs-lookup"><span data-stu-id="d440f-199">Lower RU/s to consume more RU/m</span></span>|
|<span data-ttu-id="d440f-200">1-10%</span><span class="sxs-lookup"><span data-stu-id="d440f-200">1-10%</span></span>|<span data-ttu-id="d440f-201">Kifogástalan állapot használata</span><span class="sxs-lookup"><span data-stu-id="d440f-201">Healthy use</span></span>|<span data-ttu-id="d440f-202">Üzembe helyezési azonos szinten tartása</span><span class="sxs-lookup"><span data-stu-id="d440f-202">Keep the same provisioning level</span></span>|
|<span data-ttu-id="d440f-203">10 % felett</span><span class="sxs-lookup"><span data-stu-id="d440f-203">Above 10%</span></span>|<span data-ttu-id="d440f-204">Túlterhelés</span><span class="sxs-lookup"><span data-stu-id="d440f-204">Over utilization</span></span>|<span data-ttu-id="d440f-205">Növelje a használják a kisebb a RU/m RU/mp</span><span class="sxs-lookup"><span data-stu-id="d440f-205">Increase RU/s to rely less on RU/m</span></span>|

## <a name="select-which-operations-can-consume-the-rum-budget"></a><span data-ttu-id="d440f-206">Válassza ki, milyen műveletek felhasználhat az RU/m keret</span><span class="sxs-lookup"><span data-stu-id="d440f-206">Select which operations can consume the RU/m budget</span></span>

<span data-ttu-id="d440f-207">Kérelem szinten akkor is is engedélyezését vagy letiltását RU/m költségvetés teljesíteni a kérést, függetlenül a művelet típusát.</span><span class="sxs-lookup"><span data-stu-id="d440f-207">At request level, you can also enable/disable RU/m budget to serve the request irrespective of operation type.</span></span> <span data-ttu-id="d440f-208">Ha a kérelem nem lehet felhasználni a RU/m költségvetés rendszeres kiosztott RUs másodpercenkénti költségvetési felhasznált, a kérelem halmozódni fog.</span><span class="sxs-lookup"><span data-stu-id="d440f-208">If regular provisioned RUs/sec budget is consumed and the request cannot consume the RU/m budget, this request will be throttled.</span></span> <span data-ttu-id="d440f-209">Alapértelmezés szerint minden kérelemben által kiszolgált RU/m költségvetési RU/m átviteli költségvetési aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="d440f-209">By default, any request is served by RU/m budget if RU/m throughput budget is activated.</span></span> 

<span data-ttu-id="d440f-210">Íme egy kódrészletet a DocumentDB API használata CRUD és lekérdezési műveletek RU/m költségvetési letiltását.</span><span class="sxs-lookup"><span data-stu-id="d440f-210">Here is a code snippet for disabling RU/m budget using the DocumentDB API for CRUD and query operations.</span></span>

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a><span data-ttu-id="d440f-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d440f-211">Next steps</span></span>

<span data-ttu-id="d440f-212">Ebben a cikkben azt korábban leírt Azure Cosmos DB hogyan particionálási működik, hogyan hozhat létre a particionált gyűjtemények, és hogyan választhatja ki a remek partíciókulcs az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d440f-212">In this article, we've described how partitioning works in Azure Cosmos DB, how you can create partitioned collections, and how you can pick a good partition key for your application.</span></span>

* <span data-ttu-id="d440f-213">Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték.</span><span class="sxs-lookup"><span data-stu-id="d440f-213">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="d440f-214">Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.</span><span class="sxs-lookup"><span data-stu-id="d440f-214">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="d440f-215">Ismerkedés a kódolási a [SDK-k](documentdb-sdk-dotnet.md) vagy a [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="d440f-215">Get started coding with the [SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/).</span></span>
* <span data-ttu-id="d440f-216">További tudnivalók [kiosztott átviteli sebesség](request-units.md) az Azure Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="d440f-216">Learn about [provisioned throughput](request-units.md) in Azure Cosmos DB</span></span> 

