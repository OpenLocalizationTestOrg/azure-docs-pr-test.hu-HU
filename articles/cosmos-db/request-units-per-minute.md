---
title: "Az Azure CosmosDB: Kérelem egység / perc (RU/m) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooreduce költség felügyelniük kérelem egység / perc."
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
ms.openlocfilehash: fcc3a92b9788750a2bfba361c3a9cebdb56eee05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a><span data-ttu-id="2f015-103">Az Azure Cosmos Adatbázisba percenként kérelemegység</span><span class="sxs-lookup"><span data-stu-id="2f015-103">Request units per minute in Azure Cosmos DB</span></span>

<span data-ttu-id="2f015-104">Azure Cosmos-adatbázis egy gyors és kiszámítható teljesítményt és a skála zökkenőmentesen együtt az alkalmazás növekedés elérése tervezett toohelp.</span><span class="sxs-lookup"><span data-stu-id="2f015-104">Azure Cosmos DB is designed toohelp you achieve a fast, predictable performance and scale seamlessly along with your application’s growth.</span></span> <span data-ttu-id="2f015-105">Megadhat egy Cosmos DB tároló mindkét, másodpercenként és perc (RU/m) Granularitás van az átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="2f015-105">You can provision throughput on a Cosmos DB container at both, per-second and at per-minute (RU/m) granularities.</span></span> <span data-ttu-id="2f015-106">hello esetén kiosztott átviteli sebesség perc részletességű használt toomanage váratlan igényeiben jelentkező másodpercenként részletességű előforduló hello terheléseknél engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="2f015-106">hello provisioned throughput at per-minute granularity is used toomanage unexpected spikes in hello workload occurring at a per-second granularity.</span></span> 

<span data-ttu-id="2f015-107">Ez a cikk áttekintést nyújt a kérelemegység (RU/m) percenkénti kiépítése hello működésével.</span><span class="sxs-lookup"><span data-stu-id="2f015-107">This article provides an overview of how hello provisioning of Request Unit per Minute (RU/m) works.</span></span> <span data-ttu-id="2f015-108">hello cél RU/m szolgáltatáskiépítéssel szem előtt, a kiszámítható teljesítmény (különösen akkor, ha van szüksége elemzésekre toorun fölött a működési adatok) előre nem látható igényeinek és spiky munkaterhelések tooprovide.</span><span class="sxs-lookup"><span data-stu-id="2f015-108">hello goal in mind with provisioning of RU/m is tooprovide a predictable performance around unpredictable needs (especially if you need toorun analytics on top of your operational data) and spiky workloads.</span></span> <span data-ttu-id="2f015-109">Azt szeretnénk, ha toohave ügyfeleink azok kiépítéséhez, így lehessen őket méretezni gyorsan nyugodt maradhat afelől, a további hello átviteli felhasználását.</span><span class="sxs-lookup"><span data-stu-id="2f015-109">We want toohave our customers consume more hello throughput they provision so they can scale quickly with peace of mind.</span></span>

<span data-ttu-id="2f015-110">A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="2f015-110">After reading this article, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="2f015-111">Hogyan működik a kérelemegység / perc?</span><span class="sxs-lookup"><span data-stu-id="2f015-111">How does a Request Unit per Minute work?</span></span>
* <span data-ttu-id="2f015-112">Mi az a kérelemegység / perc és a másodpercenként kérelemegység hello különbségének?</span><span class="sxs-lookup"><span data-stu-id="2f015-112">What is hello difference between Request Unit per Minute and Request Unit per Second?</span></span>
* <span data-ttu-id="2f015-113">Hogyan tooprovision RU/m?</span><span class="sxs-lookup"><span data-stu-id="2f015-113">How tooprovision RU/m?</span></span>
* <span data-ttu-id="2f015-114">A mely esetekben kell figyelembe venni kérelemegység / perc kiépítés?</span><span class="sxs-lookup"><span data-stu-id="2f015-114">Under which scenario shall I consider provisioning Request Unit per Minute?</span></span>
* <span data-ttu-id="2f015-115">Hogyan toouse hello portál metrikák toooptimize, a költség, és a teljesítmény?</span><span class="sxs-lookup"><span data-stu-id="2f015-115">How toouse hello portal metrics toooptimize my cost and performance?</span></span>
* <span data-ttu-id="2f015-116">Adja meg, milyen típusú kérelmek felhasználhat a RU/m költségvetést?</span><span class="sxs-lookup"><span data-stu-id="2f015-116">Define which type of request can consume your RU/m budget?</span></span>

## <a name="provisioning-request-units-per-minute-rum"></a><span data-ttu-id="2f015-117">Üzembe helyezési kérelemegység / perc (RU/m)</span><span class="sxs-lookup"><span data-stu-id="2f015-117">Provisioning request units per minute (RU/m)</span></span>

<span data-ttu-id="2f015-118">Ha Azure Cosmos DB hello második részletességű (RU/mp), a kérelmet egy kis késés: sikeres, ha az átviteli sebesség nem lépte hello kapacitás kiosztása belül, hogy a második hello garancia kap.</span><span class="sxs-lookup"><span data-stu-id="2f015-118">When you provision Azure Cosmos DB at hello second granularity (RU/s), you get hello guarantee that your request succeeds at a low latency if your throughput has not exceeded hello capacity provisioned within that second.</span></span> <span data-ttu-id="2f015-119">A RU/m hello granularitási jelenleg hello percen hello garancia a kérelem sikeres adott percen belül.</span><span class="sxs-lookup"><span data-stu-id="2f015-119">With RU/m, hello granularity is at hello minute with hello guarantee that your request succeeds within that minute.</span></span> <span data-ttu-id="2f015-120">Toobursting rendszerek, összehasonlítva azt győződjön meg arról, hogy elérhetővé hello teljesítmény előre jelezhető, és a rajta is tervezhet.</span><span class="sxs-lookup"><span data-stu-id="2f015-120">Compared toobursting systems, we make sure that hello performance you get is predictable and you can plan on it.</span></span>

<span data-ttu-id="2f015-121">hello works kiépítés percenként módja egyszerű:</span><span class="sxs-lookup"><span data-stu-id="2f015-121">hello way per minute provisioning works is simple:</span></span>

* <span data-ttu-id="2f015-122">RU/m lesz számlázva, óránként és tooRU/s-hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="2f015-122">RU/m is billed hourly and in addition tooRU/s.</span></span> <span data-ttu-id="2f015-123">További részletekért tekintse meg az Azure Cosmos DB [árképzést ismertető oldalra](https://aka.ms/acdbpricing).</span><span class="sxs-lookup"><span data-stu-id="2f015-123">For more details, please visit Azure Cosmos DB [pricing page](https://aka.ms/acdbpricing).</span></span>
* <span data-ttu-id="2f015-124">RU/m a gyűjtemény szintjén engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="2f015-124">RU/m can be enabled at collection level.</span></span> <span data-ttu-id="2f015-125">Amely a hello SDK-k (Node.js, Java vagy .net) vagy hello portálon keresztül végezhető (a MongoDB API munkaterhelések is között)</span><span class="sxs-lookup"><span data-stu-id="2f015-125">That can be done through hello SDKs (Node.js, Java, or .Net) or through hello portal (also include MongoDB API workloads)</span></span>
* <span data-ttu-id="2f015-126">Ha RU/m engedélyezve van, minden 100 RU/mp üzembe helyezve, is elérhetővé 1000 RU/m kiépítve (hello arány 10 x)</span><span class="sxs-lookup"><span data-stu-id="2f015-126">When RU/m is enabled, for every 100 RU/s provisioned, you also get 1,000 RU/m provisioned (hello ratio is 10x)</span></span>
* <span data-ttu-id="2f015-127">Egy adott második, egy kérelem egységet használ fel a RU/m kiosztást, csak ha túllépte a belül, hogy a második második létesítési</span><span class="sxs-lookup"><span data-stu-id="2f015-127">At a given second, a request unit consumes your RU/m provisioning only if you have exceeded your per second provisioning within that second</span></span>
* <span data-ttu-id="2f015-128">Egyszer hello 60 másodperc idő (szerint UTC) végződik, percben létesítési hello van-e tölteni</span><span class="sxs-lookup"><span data-stu-id="2f015-128">Once hello 60-second period (UTC) ends, hello per minute provisioning is refilled</span></span>
* <span data-ttu-id="2f015-129">RU/m csak a gyűjteményeket, amelyek partíciónként 5000 RU/mp maximális létesítési engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="2f015-129">RU/m can be enabled only for collections with a maximum provisioning of 5,000 RU/s per partition.</span></span> <span data-ttu-id="2f015-130">Ha méretezhető átviteli igényeinek, és ilyen magas szintű partíciónként kiépítés, egy figyelmeztető üzenetet fog kapni</span><span class="sxs-lookup"><span data-stu-id="2f015-130">If you scale your throughput needs and have such a high level of provisioning per partition, you will get a warning message</span></span>

<span data-ttu-id="2f015-131">Az alábbiakban egy konkrét példában, amelyben az ügyfél építhető ki 10kRU/s-100kRU/m, menti az 73 % elleni RU/mp és 100 000 RU/m kiépített csúcsidőre (a 50kRU/mp) keresztül egy 90 másodperc után egy gyűjteményt, amely 10 000-re a kiépítés költséggel jár:</span><span class="sxs-lookup"><span data-stu-id="2f015-131">Below is a concrete example, in which a customer can provision 10kRU/s with 100kRU/m, saving 73% in cost against provisioning for peak (at 50kRU/sec) through a 90-second period on a collection that has 10,000 RU/s and 100,000 RU/m provisioned:</span></span>

* <span data-ttu-id="2f015-132">1. a második: 100 000 hello RU/m költségvetésnek megfelelően van beállítva</span><span class="sxs-lookup"><span data-stu-id="2f015-132">1st second: hello RU/m budget is set at 100,000</span></span>
* <span data-ttu-id="2f015-133">3. második: során a második hello kérelemegység fogyasztásának lett 11,010 RUs, a fenti hello RU/mp-kiépítés 1,010 RUs.</span><span class="sxs-lookup"><span data-stu-id="2f015-133">3rd second: During that second hello consumption of Request Unit was 11,010 RUs, 1,010 RUs above hello RU/s provisioning.</span></span> <span data-ttu-id="2f015-134">Ezért 1,010 RUs hello RU/m költségvetési vonni.</span><span class="sxs-lookup"><span data-stu-id="2f015-134">Therefore, 1,010 RUs are deducted from hello RU/m budget.</span></span> <span data-ttu-id="2f015-135">98,990 RUs elérhetők hello hello RU/m költségvetés következő 57 másodpercben</span><span class="sxs-lookup"><span data-stu-id="2f015-135">98,990 RUs are available for hello next 57 seconds in hello RU/m budget</span></span>
* <span data-ttu-id="2f015-136">29 második: adott másodpercen belül nagy csúcs történt (> 4 x nagyobb, mint a kiosztás, másodpercenként) és a kérelemegység hello fogyasztásának 46,920 RUs volt.</span><span class="sxs-lookup"><span data-stu-id="2f015-136">29th second: During that second, a large spike happened (>4x higher than provisioning per second) and hello consumption of Request Unit was 46,920 RUs.</span></span> <span data-ttu-id="2f015-137">36,920 RUs vonni hello RU/m költségvetést, eldobni a 92,323 RUs (28 másodperc) too55, a 403-as RUs (29 másodperc)</span><span class="sxs-lookup"><span data-stu-id="2f015-137">36,920 RUs are deducted from hello RU/m budget that dropped from 92,323 RUs (28th second) too55,403 RUs (29th second)</span></span>
* <span data-ttu-id="2f015-138">61st második: RU/m költségvetési vissza too100, 000 RUs van beállítva.</span><span class="sxs-lookup"><span data-stu-id="2f015-138">61st second: RU/m budget is set back too100,000 RUs.</span></span>
 
![Graph hello fogyasztás megjelenítő és az Azure Cosmos DB kiépítése](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a><span data-ttu-id="2f015-140">Kérelem egység kapacitás RU/m megadása</span><span class="sxs-lookup"><span data-stu-id="2f015-140">Specifying request unit capacity with RU/m</span></span>

<span data-ttu-id="2f015-141">Egy Azure Cosmos DB gyűjtemény létrehozásakor megadott hello szám kérelemegység (RU / másodperc) másodpercenként kívánt hello gyűjtemény számára.</span><span class="sxs-lookup"><span data-stu-id="2f015-141">When creating an Azure Cosmos DB collection, you specify hello number of request units per second (RU per second) you want reserved for hello collection.</span></span> <span data-ttu-id="2f015-142">Eldöntheti azt is, ha azt szeretné, tooadd RU / perc.</span><span class="sxs-lookup"><span data-stu-id="2f015-142">You can also decide if you want tooadd RU per minute.</span></span> <span data-ttu-id="2f015-143">Ezt megteheti hello Portal vagy hello SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="2f015-143">This can be done through hello Portal or hello SDK.</span></span> 

### <a name="through-hello-portal"></a><span data-ttu-id="2f015-144">Hello portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="2f015-144">Through hello Portal</span></span>

<span data-ttu-id="2f015-145">Engedélyezés vagy letiltás RU / perc egyszerűen igényel a kattintson egy gyűjtemény létesítésekor.</span><span class="sxs-lookup"><span data-stu-id="2f015-145">Enabling or disabling RU per minute simply requires a click when provisioning a collection.</span></span> 

 ![Képernyőfelvétel: hogyan tooset RU/m a hello Azure-portálon](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-hello-sdk"></a><span data-ttu-id="2f015-147">Hello SDK használatával</span><span class="sxs-lookup"><span data-stu-id="2f015-147">Through hello SDK</span></span>
<span data-ttu-id="2f015-148">Először ami fontos, hogy RU/m lehetőség csak a következő SDK-k hello toonote:</span><span class="sxs-lookup"><span data-stu-id="2f015-148">First, this is important toonote that RU/m is only available for hello following SDKs:</span></span>

* <span data-ttu-id="2f015-149">.NET 1.14.0</span><span class="sxs-lookup"><span data-stu-id="2f015-149">.Net 1.14.0</span></span>
* <span data-ttu-id="2f015-150">Java 1.11.0</span><span class="sxs-lookup"><span data-stu-id="2f015-150">Java 1.11.0</span></span>
* <span data-ttu-id="2f015-151">NODE.js 1.12.0</span><span class="sxs-lookup"><span data-stu-id="2f015-151">Node.js 1.12.0</span></span>
* <span data-ttu-id="2f015-152">Python 2.2.0</span><span class="sxs-lookup"><span data-stu-id="2f015-152">Python 2.2.0</span></span>

<span data-ttu-id="2f015-153">Íme egy kódrészletet 3000 kérelem egység / második és 30 000 kérelemegység / perc hello .NET SDK használatával a gyűjtemény létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="2f015-153">Here is a code snippet for creating a collection with 3,000 request units per second and 30,000 request units per minute using hello .NET SDK:</span></span>

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set hello throughput too3,000 request units per second which will give you 30,000 request units per minute as hello RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

<span data-ttu-id="2f015-154">Itt egy kódrészletet hello sebességét, a gyűjtemény too5 módosítására, a kiépítés RU / perc használata nélkül másodpercenként 000 kérelemegység hello .NET SDK-val:</span><span class="sxs-lookup"><span data-stu-id="2f015-154">Here is a code snippet for changing hello throughput of a collection too5,000 request units per second without provisioning RU per minute using hello .NET SDK:</span></span>

```csharp
// Get hello current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput too5000 request units per second without RU/m enabled (hello last parameter tooOfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a><span data-ttu-id="2f015-155">Jó elférjen forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="2f015-155">Good fit scenarios</span></span>

<span data-ttu-id="2f015-156">Ebben a szakaszban vannak remekül beválik, ha engedélyezi a kérelemegység / perc forgatókönyvek áttekintése a Microsoft biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2f015-156">In this section, we provide an overview of scenarios that are a good fit for enabling request units per minute.</span></span>

<span data-ttu-id="2f015-157">**Fejlesztési/tesztelési környezetben:** remekül beválik.</span><span class="sxs-lookup"><span data-stu-id="2f015-157">**Dev/Test environment:** Good fit.</span></span> <span data-ttu-id="2f015-158">Hello fejlesztési szakaszban, ha teszteli az alkalmazás a különböző terhelésekhez RU/m hello rugalmasságot biztosítanak ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2f015-158">During hello development stage, if you are testing your application with different workloads, RU/m can provide hello flexibility at this stage.</span></span> <span data-ttu-id="2f015-159">Hello közben [emulátor](local-emulator.md) egy nagyszerű szabad eszköz tootest Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2f015-159">While hello [emulator](local-emulator.md) is a great free tool tootest Azure Cosmos DB.</span></span> <span data-ttu-id="2f015-160">Azonban ha azt szeretné, hogy egy felhőalapú környezetben toostart, akkor egy rugalmas lehetőségeket biztosítanak a RU/m az ad hoc teljesítmény igényeinek.</span><span class="sxs-lookup"><span data-stu-id="2f015-160">However if you want toostart in a cloud environment, you will have a great flexibility with RU/m for your adhoc performance needs.</span></span> <span data-ttu-id="2f015-161">Rendszer időt további fejleszt, először a teljesítményigény kevesebb vagyonának.</span><span class="sxs-lookup"><span data-stu-id="2f015-161">You will spend more time developing, less worrying about performance needs at first.</span></span> <span data-ttu-id="2f015-162">Azt javasoljuk hello minimális RU/mp kiépítés kezdve és RU/m engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2f015-162">We recommend starting with hello minimum RU/s provisioning and enable RU/m.</span></span>

<span data-ttu-id="2f015-163">**Előre nem látható, spiky, percben granularitási kell:** jó elférjen – megtakarítások: 25-75 %-át.</span><span class="sxs-lookup"><span data-stu-id="2f015-163">**Unpredictable, spiky, minute granularity needs:** Good fit – Savings: 25-75%.</span></span> <span data-ttu-id="2f015-164">Úgy találtuk, hogy a csoportba vannak nagy fokozása RU/m és a legtöbb éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2f015-164">We have seen large improvement from RU/m and most production scenarios are into that group.</span></span> <span data-ttu-id="2f015-165">Ha egy IoT-terhelést, csúcs rendelkezik néhány alkalommal be egy perc alatt, ha futó lekérdezésekhez amikor a rendszer teszi tömeges beszúrást hello azonos időben, plusz kapacitást kell a handeling spiky kell.</span><span class="sxs-lookup"><span data-stu-id="2f015-165">If you have an IoT workload that has spike a few times in a minute, if you have queries running when your system makes mass insert at hello same time, you will need extra capacity for handeling spiky needs.</span></span> <span data-ttu-id="2f015-166">Azt javasoljuk, hogy az erőforrás igényeinek optimalizálás az alábbi részletes módszer alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="2f015-166">We recommend optimizing your resource needs by applying our step by step approach below.</span></span>

 ![5 perc granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 <span data-ttu-id="2f015-168">*Ábra - RU fogyasztás teljesítményteszt*</span><span class="sxs-lookup"><span data-stu-id="2f015-168">*Figure - RU consumption benchmark*</span></span>

<span data-ttu-id="2f015-169">**Nyugodt maradhat afelől,:** jó elférjen – megtakarítások: 10-20 %-át.</span><span class="sxs-lookup"><span data-stu-id="2f015-169">**Peace of mind:** Good fit – Savings: 10-20%.</span></span> <span data-ttu-id="2f015-170">Egyes esetekben csak szeretné, hogy toohave nyugodt maradhat afelől, és nem kell foglalkoznia lehetséges csúcsait és sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="2f015-170">Sometimes, you just want toohave peace of mind and not worry about potential peaks and throttling.</span></span> <span data-ttu-id="2f015-171">Ez a szolgáltatás hello jobbról egy meg.</span><span class="sxs-lookup"><span data-stu-id="2f015-171">This feature is hello right one for you.</span></span> <span data-ttu-id="2f015-172">Ebben az esetben ajánlott, RU/m engedélyezése és némileg csökken a második létesítési.</span><span class="sxs-lookup"><span data-stu-id="2f015-172">In that case, we recommend enabling RU/m and slightly lower your per second provisioning.</span></span> <span data-ttu-id="2f015-173">Ebben az esetben eltér hello fent, akkor nem próbálkozik toooptimize agresszív a kiépítés.</span><span class="sxs-lookup"><span data-stu-id="2f015-173">This case is different from hello above as you will not try toooptimize aggressively your provisioning.</span></span> <span data-ttu-id="2f015-174">Ez az a "Nulla sávszélesség-szabályozás" alaposan több.</span><span class="sxs-lookup"><span data-stu-id="2f015-174">This is more of a “Zero Throttling” mindset you are in.</span></span>

<span data-ttu-id="2f015-175">Ad hoc igényekkel végrehajtott kulcsfontosságú műveleteket: néha ajánlott tooonly segítségével végrehajtott kulcsfontosságú műveleteket RU/m költségvetést, így nem hello költségvetési hozzáférést ad hoc vagy kevésbé fontos operations felhasználását.</span><span class="sxs-lookup"><span data-stu-id="2f015-175">Critical operations with adhoc needs: We sometimes recommend tooonly let critical operations access RU/m budget so hello budget doesn’t get consume by adhoc or less important operations.</span></span> <span data-ttu-id="2f015-176">Amely könnyen határozható meg az alábbi hello részben.</span><span class="sxs-lookup"><span data-stu-id="2f015-176">That can be easily defined in hello section below.</span></span>

## <a name="using-hello-portal-metrics-toooptimize-cost-and-performance"></a><span data-ttu-id="2f015-177">Hello portál metrikák toooptimize költség és teljesítmény</span><span class="sxs-lookup"><span data-stu-id="2f015-177">Using hello portal metrics toooptimize cost and performance</span></span>

<span data-ttu-id="2f015-178">**Hello elkövetkező hetektől azt fogja továbbfejlesztése hello tartalma körüli RUs perces használat toooptimize kell a teljesítmény figyelése.**</span><span class="sxs-lookup"><span data-stu-id="2f015-178">**In hello coming weeks, we will further develop hello content around monitoring RUs minute consumption toooptimize your throughput needs.**</span></span>

<span data-ttu-id="2f015-179">Keresztül hello portál metrika láthatja, mennyi RU és felhasznált rendszeres RU másodperc perc.</span><span class="sxs-lookup"><span data-stu-id="2f015-179">Through hello portal metrics, you can see how much of regular RU seconds you consume versus RU minutes.</span></span> <span data-ttu-id="2f015-180">Figyelési metrikákat segítségül a kiépítés optimalizálását.</span><span class="sxs-lookup"><span data-stu-id="2f015-180">Monitoring these metrics should help you optimize your provisioning.</span></span> 

<span data-ttu-id="2f015-181">Azt javasoljuk, hogy a részletes megközelítés hogyan toouse RU/m tooyour kihasználásához.</span><span class="sxs-lookup"><span data-stu-id="2f015-181">We recommend a step by step approach on how toouse RU/m tooyour advantage.</span></span> <span data-ttu-id="2f015-182">Minden egyes lépést, rendelkeznie kell egy teljes ciklus a számítási feladatok (lehet óra, nap, vagy akár hét) jelölő hello RU fogyasztás áttekintése és elemzések lekérése a hello kihasználtságot mi kiépítése.</span><span class="sxs-lookup"><span data-stu-id="2f015-182">For each step, you should have an overview of hello RU consumption representing a full cycle of your workload (it could be hours, days, or even weeks) and get insights on hello utilization of what you provision.</span></span>

<span data-ttu-id="2f015-183">hello elve mögött ezt a módszert használja az átviteli sebesség kiépítés zárja be, a teljesítmény az alábbi feltételeknek megfelelő pontot kiépítés lehetséges tooa toomake.</span><span class="sxs-lookup"><span data-stu-id="2f015-183">hello principle behind this approach is toomake your throughput provisioning as close as possible tooa provisioning point that matches your performance criteria below.</span></span> 

![5 perces granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
<span data-ttu-id="2f015-185">toounderstand hello optimális üzembe helyezési pont a munkaterhelés, toounderstand kell:</span><span class="sxs-lookup"><span data-stu-id="2f015-185">toounderstand hello optimal provisioning point for your workload, you need toounderstand:</span></span>

* <span data-ttu-id="2f015-186">Felhasználási minták: nincs, alkalomszerű vagy tartós igényeiben jelentkező?</span><span class="sxs-lookup"><span data-stu-id="2f015-186">Consumption patterns: no, infrequent or sustained spikes?</span></span> <span data-ttu-id="2f015-187">(2 x átlagos) kis, közepes vagy nagy (> 10 x átlagos) igényeiben jelentkező?</span><span class="sxs-lookup"><span data-stu-id="2f015-187">Small (2x average), medium, or large (>10x average) spikes?</span></span>
* <span data-ttu-id="2f015-188">A szabályozottan halmozott kérelmek százalékos: tegye úgy érzi, hogy kényelmes Ha szabályozáshoz bit van?</span><span class="sxs-lookup"><span data-stu-id="2f015-188">Percent of throttled requests: do you feel comfortable if you have a bit of throttling?</span></span> <span data-ttu-id="2f015-189">Ha igen, hogy mekkora által?</span><span class="sxs-lookup"><span data-stu-id="2f015-189">If so, by how much?</span></span> 

<span data-ttu-id="2f015-190">Ha azonosított Mik azok a célokat, képes tooget szorosabb toohello optimális kiépítés lesz.</span><span class="sxs-lookup"><span data-stu-id="2f015-190">Once you have identified what your goals are, you will be able tooget closer toohello optimal provisioning.</span></span>

<span data-ttu-id="2f015-191">tooassist meg, hogyan toooptimize a kiépítés a fogyasztás alapján történik a RU/m általános útmutatást tooprovide szeretnénk.</span><span class="sxs-lookup"><span data-stu-id="2f015-191">tooassist you, we want tooprovide an overall guidance on how toooptimize your provisioning based on your RU/m consumption.</span></span> <span data-ttu-id="2f015-192">Ez az útmutató tooall jellegű munkaterheléseket nem alkalmazható, de hello private Preview verziójára Tudásbázis alapul.</span><span class="sxs-lookup"><span data-stu-id="2f015-192">This guidance doesn’t apply tooall kind of workloads but is based on hello private preview knowledge.</span></span> <span data-ttu-id="2f015-193">Ilyen alaptervek előfordulhat, hogy módosítjuk, hogy további:</span><span class="sxs-lookup"><span data-stu-id="2f015-193">We might change such baselines as we learn more:</span></span>

|<span data-ttu-id="2f015-194">RU/m kihasználtsága (%)</span><span class="sxs-lookup"><span data-stu-id="2f015-194">RU/m % utilization</span></span>|<span data-ttu-id="2f015-195">Bizonyos fokú RU/m kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="2f015-195">Degree of utilization of RU/m</span></span>|<span data-ttu-id="2f015-196">Javasolt művelet történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="2f015-196">Recommended actions for provisioning</span></span>|
|---|---|---|
|<span data-ttu-id="2f015-197">0-1%</span><span class="sxs-lookup"><span data-stu-id="2f015-197">0-1%</span></span>|<span data-ttu-id="2f015-198">A kihasználtság</span><span class="sxs-lookup"><span data-stu-id="2f015-198">Under utilization</span></span>|<span data-ttu-id="2f015-199">Alacsonyabb RU/mp tooconsume további RU/m</span><span class="sxs-lookup"><span data-stu-id="2f015-199">Lower RU/s tooconsume more RU/m</span></span>|
|<span data-ttu-id="2f015-200">1-10%</span><span class="sxs-lookup"><span data-stu-id="2f015-200">1-10%</span></span>|<span data-ttu-id="2f015-201">Kifogástalan állapot használata</span><span class="sxs-lookup"><span data-stu-id="2f015-201">Healthy use</span></span>|<span data-ttu-id="2f015-202">Tartsa hello azonos kiépítés szint</span><span class="sxs-lookup"><span data-stu-id="2f015-202">Keep hello same provisioning level</span></span>|
|<span data-ttu-id="2f015-203">10 % felett</span><span class="sxs-lookup"><span data-stu-id="2f015-203">Above 10%</span></span>|<span data-ttu-id="2f015-204">Túlterhelés</span><span class="sxs-lookup"><span data-stu-id="2f015-204">Over utilization</span></span>|<span data-ttu-id="2f015-205">Kevésbé a RU/m RU/mp toorely növelése</span><span class="sxs-lookup"><span data-stu-id="2f015-205">Increase RU/s toorely less on RU/m</span></span>|

## <a name="select-which-operations-can-consume-hello-rum-budget"></a><span data-ttu-id="2f015-206">Válassza ki, milyen műveleteket is felhasználhatnak hello RU/m költségvetés</span><span class="sxs-lookup"><span data-stu-id="2f015-206">Select which operations can consume hello RU/m budget</span></span>

<span data-ttu-id="2f015-207">Kérelem szinten akkor is is engedélyezését vagy letiltását RU/m költségvetési tooserve hello kérelem függetlenül művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="2f015-207">At request level, you can also enable/disable RU/m budget tooserve hello request irrespective of operation type.</span></span> <span data-ttu-id="2f015-208">Ha hello kérelmet nem lehet felhasználni hello RU/m költségvetési rendszeres kiosztott RUs másodpercenkénti költségvetési felhasznált, a kérelem halmozódni fog.</span><span class="sxs-lookup"><span data-stu-id="2f015-208">If regular provisioned RUs/sec budget is consumed and hello request cannot consume hello RU/m budget, this request will be throttled.</span></span> <span data-ttu-id="2f015-209">Alapértelmezés szerint minden kérelemben által kiszolgált RU/m költségvetési RU/m átviteli költségvetési aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="2f015-209">By default, any request is served by RU/m budget if RU/m throughput budget is activated.</span></span> 

<span data-ttu-id="2f015-210">Íme egy kódrészletet hello DocumentDB API használata CRUD és lekérdezési műveletek RU/m költségvetési letiltását.</span><span class="sxs-lookup"><span data-stu-id="2f015-210">Here is a code snippet for disabling RU/m budget using hello DocumentDB API for CRUD and query operations.</span></span>

```csharp
// In order toodisable any CRUD request for RU/m, set DisableRUPerMinuteUsage tootrue in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order toodisable any query request for RU/m, set DisableRUPerMinuteOnRequest tootrue in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a><span data-ttu-id="2f015-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f015-211">Next steps</span></span>

<span data-ttu-id="2f015-212">Ebben a cikkben azt korábban leírt Azure Cosmos DB hogyan particionálási működik, hogyan hozhat létre a particionált gyűjtemények, és hogyan választhatja ki a remek partíciókulcs az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2f015-212">In this article, we've described how partitioning works in Azure Cosmos DB, how you can create partitioned collections, and how you can pick a good partition key for your application.</span></span>

* <span data-ttu-id="2f015-213">Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték.</span><span class="sxs-lookup"><span data-stu-id="2f015-213">Perform scale and performance testing with Azure Cosmos DB.</span></span> <span data-ttu-id="2f015-214">Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.</span><span class="sxs-lookup"><span data-stu-id="2f015-214">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) for a sample.</span></span>
* <span data-ttu-id="2f015-215">Ismerkedés a hello kódolási [SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="2f015-215">Get started coding with hello [SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/).</span></span>
* <span data-ttu-id="2f015-216">További tudnivalók [kiosztott átviteli sebesség](request-units.md) az Azure Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="2f015-216">Learn about [provisioned throughput](request-units.md) in Azure Cosmos DB</span></span> 

