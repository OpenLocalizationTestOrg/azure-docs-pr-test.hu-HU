---
title: "aaaAzure Cosmos DB méretezés és teljesítmény tesztelése |} Microsoft Docs"
description: "Megtudhatja, hogyan tooperform méretezése és a teljesítmény tesztelése az Azure Cosmos DB"
keywords: "A teljesítmény tesztelése"
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: arramac
ms.openlocfilehash: 46d1217e11a39ee970a868de9a5c5dfcf52cedf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a><span data-ttu-id="36a65-104">Teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése</span><span class="sxs-lookup"><span data-stu-id="36a65-104">Performance and scale testing with Azure Cosmos DB</span></span>
<span data-ttu-id="36a65-105">Teljesítmény- és mérettesztelés az alkalmazásfejlesztés kulcsfontosságú lépés.</span><span class="sxs-lookup"><span data-stu-id="36a65-105">Performance and scale testing is a key step in application development.</span></span> <span data-ttu-id="36a65-106">Számos alkalmazás, az adatbázis-rétegből hello jelentős hatással van hello átfogó teljesítményét és méretezhetőségét, és ezért kritikus összetevője teljesítményét teszteli.</span><span class="sxs-lookup"><span data-stu-id="36a65-106">For many applications, hello database tier has a significant impact on hello overall performance and scalability, and is therefore a critical component of performance testing.</span></span> <span data-ttu-id="36a65-107">[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) erre a célra kialakított rugalmasan méretezhető és kiszámítható teljesítményt, és ezért egy nagyszerű méretezése nagy teljesítményű adatbázis-rétegből igénylő alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="36a65-107">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is purpose-built for elastic scale and predictable performance, and therefore a great fit for applications that need a high-performance database tier.</span></span> 

<span data-ttu-id="36a65-108">Ez a cikk a fejlesztők a Cosmos DB munkaterhelések teljesítményét teszt csomagok végrehajtási, vagy nagy teljesítményű alkalmazás forgatókönyvek Cosmos DB kiértékelése hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="36a65-108">This article is a reference for developers implementing performance test suites for their Cosmos DB workloads, or evaluating Cosmos DB for high-performance application scenarios.</span></span> <span data-ttu-id="36a65-109">Elsősorban hello adatbázis vizsgálati elkülönített teljesítmény összpontosít, de ajánlott eljárások az üzemi környezetben működő alkalmazásokat is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="36a65-109">It focuses primarily on isolated performance testing of hello database, but also includes best practices for production applications.</span></span>

<span data-ttu-id="36a65-110">A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="36a65-110">After reading this article, you will be able tooanswer hello following questions:</span></span>   

* <span data-ttu-id="36a65-111">Hol található a .NET ügyfél mintaalkalmazás Cosmos DB teljesítmény teszteléshez?</span><span class="sxs-lookup"><span data-stu-id="36a65-111">Where can I find a sample .NET client application for performance testing of Cosmos DB?</span></span> 
* <span data-ttu-id="36a65-112">Hogyan elérése a saját ügyfélalkalmazás magas teljesítmény szintek Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="36a65-112">How do I achieve high throughput levels with Cosmos DB from my client application?</span></span>

<span data-ttu-id="36a65-113">tooget használatába a kódot, töltse le a hello projekt [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="36a65-113">tooget started with code, please download hello project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span></span> 

> [!NOTE]
> <span data-ttu-id="36a65-114">hello az alkalmazás célja gyakorlati tanácsok toodemonstrate kívül Cosmos DB jobb teljesítményt kibontott ügyfélgépek kis számú.</span><span class="sxs-lookup"><span data-stu-id="36a65-114">hello goal of this application is toodemonstrate best practices for extracting better performance out of Cosmos DB with a small number of client machines.</span></span> <span data-ttu-id="36a65-115">Ez nem tették hello szolgáltatást, amely méretezhető korlátlanul bővíthető toodemonstrate hello maximális kapacitását.</span><span class="sxs-lookup"><span data-stu-id="36a65-115">This was not made toodemonstrate hello peak capacity of hello service, which can scale limitlessly.</span></span>
> 
> 

<span data-ttu-id="36a65-116">Ha az ügyféloldali konfigurációs beállítások tooimprove Cosmos DB teljesítmény keres, tekintse meg [Azure Cosmos DB teljesítmény tippek](performance-tips.md).</span><span class="sxs-lookup"><span data-stu-id="36a65-116">If you're looking for client-side configuration options tooimprove Cosmos DB performance, see [Azure Cosmos DB performance tips](performance-tips.md).</span></span>

## <a name="run-hello-performance-testing-application"></a><span data-ttu-id="36a65-117">Hello teljesítménytesztelési alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="36a65-117">Run hello performance testing application</span></span>
<span data-ttu-id="36a65-118">hello leggyorsabb módon tooget lépések van toocompile és futtatási hello .NET mintaalkalmazás az alábbi hello lépéseket leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="36a65-118">hello quickest way tooget started is toocompile and run hello .NET sample below, as described in hello steps below.</span></span> <span data-ttu-id="36a65-119">Is tekintse át a hello forráskódját és hasonló konfigurációkat tooyour saját ügyfélalkalmazások megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="36a65-119">You can also review hello source code and implement similar configurations tooyour own client applications.</span></span>

<span data-ttu-id="36a65-120">**1. lépés:** letöltési hello projektet a [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), vagy elágazás hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="36a65-120">**Step 1:** Download hello project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), or fork hello GitHub repository.</span></span>

<span data-ttu-id="36a65-121">**2. lépés:** végponti URL-cím, AuthorizationKey, CollectionThroughput és DocumentTemplate (opcionális) az App.config fájlban hello beállításainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="36a65-121">**Step 2:** Modify hello settings for EndpointUrl, AuthorizationKey, CollectionThroughput and DocumentTemplate (optional) in App.config.</span></span>

> [!NOTE]
> <span data-ttu-id="36a65-122">Gyűjtemények nagy átviteli sebességgel üzembe helyezése, előtt tekintse meg az toohello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello költségek gyűjteményenként.</span><span class="sxs-lookup"><span data-stu-id="36a65-122">Before provisioning collections with high throughput, please refer toohello [Pricing Page](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello costs per collection.</span></span> <span data-ttu-id="36a65-123">Az Azure Cosmos DB váltók tárolási és átviteli sebességet egymástól függetlenül óránként, így költségeket takaríthat törlése vagy a Azure Cosmos DB gyűjtemények hello átviteli lowering tesztelés után.</span><span class="sxs-lookup"><span data-stu-id="36a65-123">Azure Cosmos DB bills storage and throughput independently on an hourly basis, so you can save costs by deleting or lowering hello throughput of your Azure Cosmos DB collections after testing.</span></span>
> 
> 

<span data-ttu-id="36a65-124">**3. lépés:** összegzik és hello Konzolalkalmazás hello parancssorból futtassa.</span><span class="sxs-lookup"><span data-stu-id="36a65-124">**Step 3:** Compile and run hello console app from hello command line.</span></span> <span data-ttu-id="36a65-125">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="36a65-125">You should see output like hello following:</span></span>

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


<span data-ttu-id="36a65-126">**(Ha szükséges), a 4. lépés:** hello átviteli jelentett (RU/mp) hello eszközből hello azonos vagy nagyobb, mint hello kiosztott átviteli sebesség hello gyűjtemény kell lennie.</span><span class="sxs-lookup"><span data-stu-id="36a65-126">**Step 4 (if necessary):** hello throughput reported (RU/s) from hello tool should be hello same or higher than hello provisioned throughput of hello collection.</span></span> <span data-ttu-id="36a65-127">Ha nem, növekvő hello DegreeOfParallelism kis lépésekkel segítenek elérni hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="36a65-127">If not, increasing hello DegreeOfParallelism in small increments may help you reach hello limit.</span></span> <span data-ttu-id="36a65-128">Ha az ügyfél alkalmazásból hello átviteli trületek hello alkalmazás több példánya indítása a hello azonos vagy különböző példányok eléri a határértéket kiépített hello hello között különböző gépek nyújtanak segítséget.</span><span class="sxs-lookup"><span data-stu-id="36a65-128">If hello throughput from your client app plateaus, launching multiple instances of hello app on hello same or different machines will help you reach hello provisioned limit across hello different instances.</span></span> <span data-ttu-id="36a65-129">Ha ebben a lépésben segítségre van szüksége, kérjük, írjon e-mailt tooaskcosmosdb@microsoft.com vagy a fájl egy támogatási jegy a hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36a65-129">If you need help with this step, please, write an email tooaskcosmosdb@microsoft.com or file a support ticket from hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="36a65-130">Miután hello alkalmazás fut, próbálja meg különböző [házirendek indexelő](indexing-policies.md) és [konzisztenciaszintek](consistency-levels.md) toounderstand átviteli sebesség és a késleltetés gyakorolt hatásának.</span><span class="sxs-lookup"><span data-stu-id="36a65-130">Once you have hello app running, you can try different [Indexing policies](indexing-policies.md) and [Consistency levels](consistency-levels.md) toounderstand their impact on throughput and latency.</span></span> <span data-ttu-id="36a65-131">Hello forráskód áttekintheti és hasonló konfigurációkat tooyour saját tesztelési programcsomagok vagy üzemi környezetben működő alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="36a65-131">You can also review hello source code and implement similar configurations tooyour own test suites or production applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36a65-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36a65-132">Next steps</span></span>
<span data-ttu-id="36a65-133">Ebben a cikkben azt venni, hogyan hajthat végre a teljesítmény és méretezhetőség tesztelték Cosmos Adatbázisba egy .NET-Konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="36a65-133">In this article, we looked at how you can perform performance and scale testing with Cosmos DB using a .NET console app.</span></span> <span data-ttu-id="36a65-134">Azure Cosmos DB használatával kapcsolatos további információkért tekintse meg az alábbi toohello hivatkozásokat követve.</span><span class="sxs-lookup"><span data-stu-id="36a65-134">Please refer toohello links below for additional information on working with Azure Cosmos DB.</span></span>

* [<span data-ttu-id="36a65-135">Az Azure Cosmos adatbázis teljesítményének tesztelésekor minta</span><span class="sxs-lookup"><span data-stu-id="36a65-135">Azure Cosmos DB performance testing sample</span></span>](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [<span data-ttu-id="36a65-136">Ügyfél konfigurációs beállítások tooimprove Azure Cosmos DB teljesítmény</span><span class="sxs-lookup"><span data-stu-id="36a65-136">Client configuration options tooimprove Azure Cosmos DB performance</span></span>](performance-tips.md)
* [<span data-ttu-id="36a65-137">Kiszolgálóoldali particionálás az Azure Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="36a65-137">Server-side partitioning in Azure Cosmos DB</span></span>](partition-data.md)


