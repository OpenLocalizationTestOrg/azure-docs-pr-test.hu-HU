---
title: "Azure Cosmos DB méretezés és teljesítmény tesztelése |} Microsoft Docs"
description: "Ismerje meg, hogyan hajthat végre a méretezés és teljesítmény az Azure Cosmos DB tesztelése"
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
ms.openlocfilehash: b5a1edd08819e82437c5b22d8eb131665d7c9645
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a><span data-ttu-id="d53a3-104">Teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése</span><span class="sxs-lookup"><span data-stu-id="d53a3-104">Performance and scale testing with Azure Cosmos DB</span></span>
<span data-ttu-id="d53a3-105">Teljesítmény- és mérettesztelés az alkalmazásfejlesztés kulcsfontosságú lépés.</span><span class="sxs-lookup"><span data-stu-id="d53a3-105">Performance and scale testing is a key step in application development.</span></span> <span data-ttu-id="d53a3-106">Több alkalmazás az adatbázis-rétegből jelentős hatással az átfogó teljesítményét és méretezhetőségét, és ezért a teljesítmény tesztelése kritikus összetevője.</span><span class="sxs-lookup"><span data-stu-id="d53a3-106">For many applications, the database tier has a significant impact on the overall performance and scalability, and is therefore a critical component of performance testing.</span></span> <span data-ttu-id="d53a3-107">[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) erre a célra kialakított rugalmasan méretezhető és kiszámítható teljesítményt, és ezért egy nagyszerű méretezése nagy teljesítményű adatbázis-rétegből igénylő alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d53a3-107">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is purpose-built for elastic scale and predictable performance, and therefore a great fit for applications that need a high-performance database tier.</span></span> 

<span data-ttu-id="d53a3-108">Ez a cikk a fejlesztők a Cosmos DB munkaterhelések teljesítményét teszt csomagok végrehajtási, vagy nagy teljesítményű alkalmazás forgatókönyvek Cosmos DB kiértékelése hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="d53a3-108">This article is a reference for developers implementing performance test suites for their Cosmos DB workloads, or evaluating Cosmos DB for high-performance application scenarios.</span></span> <span data-ttu-id="d53a3-109">Elsősorban az adatbázis vizsgálati elkülönített teljesítmény összpontosít, de ajánlott eljárások az üzemi környezetben működő alkalmazásokat is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d53a3-109">It focuses primarily on isolated performance testing of the database, but also includes best practices for production applications.</span></span>

<span data-ttu-id="d53a3-110">A cikk elolvasása után lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="d53a3-110">After reading this article, you will be able to answer the following questions:</span></span>   

* <span data-ttu-id="d53a3-111">Hol található a .NET ügyfél mintaalkalmazás Cosmos DB teljesítmény teszteléshez?</span><span class="sxs-lookup"><span data-stu-id="d53a3-111">Where can I find a sample .NET client application for performance testing of Cosmos DB?</span></span> 
* <span data-ttu-id="d53a3-112">Hogyan elérése a saját ügyfélalkalmazás magas teljesítmény szintek Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="d53a3-112">How do I achieve high throughput levels with Cosmos DB from my client application?</span></span>

<span data-ttu-id="d53a3-113">Első lépésként kóddal, töltse le a projektet a [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span><span class="sxs-lookup"><span data-stu-id="d53a3-113">To get started with code, please download the project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark).</span></span> 

> [!NOTE]
> <span data-ttu-id="d53a3-114">Ez az alkalmazás célja a jobb teljesítmény érdekében kívül Cosmos DB kibontása ügyfélgépek kis számú ajánlott eljárásai bemutatják.</span><span class="sxs-lookup"><span data-stu-id="d53a3-114">The goal of this application is to demonstrate best practices for extracting better performance out of Cosmos DB with a small number of client machines.</span></span> <span data-ttu-id="d53a3-115">Ez nem tették bemutatása a szolgáltatást, amely méretezhető korlátlanul bővíthető maximális kapacitását.</span><span class="sxs-lookup"><span data-stu-id="d53a3-115">This was not made to demonstrate the peak capacity of the service, which can scale limitlessly.</span></span>
> 
> 

<span data-ttu-id="d53a3-116">Ha a keresett Cosmos DB teljesítmény javítása érdekében az ügyféloldali konfigurációs beállítások, lásd: [Azure Cosmos DB teljesítmény tippek](performance-tips.md).</span><span class="sxs-lookup"><span data-stu-id="d53a3-116">If you're looking for client-side configuration options to improve Cosmos DB performance, see [Azure Cosmos DB performance tips](performance-tips.md).</span></span>

## <a name="run-the-performance-testing-application"></a><span data-ttu-id="d53a3-117">Futtatható az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="d53a3-117">Run the performance testing application</span></span>
<span data-ttu-id="d53a3-118">Első lépésként a leggyorsabban fordításához és futtatásához a .NET-példa az alábbi az alábbi lépések elvégzésével.</span><span class="sxs-lookup"><span data-stu-id="d53a3-118">The quickest way to get started is to compile and run the .NET sample below, as described in the steps below.</span></span> <span data-ttu-id="d53a3-119">Is ajánlott felülvizsgálni a forráskódot, és a saját ügyfélalkalmazásait hasonló konfigurációkat valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="d53a3-119">You can also review the source code and implement similar configurations to your own client applications.</span></span>

<span data-ttu-id="d53a3-120">**1. lépés:** töltse le a projektet a [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), vagy oszthatja ketté a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="d53a3-120">**Step 1:** Download the project from [Azure Cosmos DB Performance Testing Sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), or fork the GitHub repository.</span></span>

<span data-ttu-id="d53a3-121">**2. lépés:** módosítsa a beállításokat a végponti URL-cím, AuthorizationKey, CollectionThroughput és DocumentTemplate az App.config fájlban (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="d53a3-121">**Step 2:** Modify the settings for EndpointUrl, AuthorizationKey, CollectionThroughput and DocumentTemplate (optional) in App.config.</span></span>

> [!NOTE]
> <span data-ttu-id="d53a3-122">Gyűjtemények nagy átviteli sebességgel üzembe helyezése, előtt tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/) becsléséhez gyűjteményenként költségeket.</span><span class="sxs-lookup"><span data-stu-id="d53a3-122">Before provisioning collections with high throughput, please refer to the [Pricing Page](https://azure.microsoft.com/pricing/details/cosmos-db/) to estimate the costs per collection.</span></span> <span data-ttu-id="d53a3-123">Az Azure Cosmos DB váltók tárolási és átviteli sebességet egymástól függetlenül óránként, így költségeket takaríthat törlése, és így csökkenti az átviteli sebessége a Azure Cosmos DB gyűjtemények tesztelés után.</span><span class="sxs-lookup"><span data-stu-id="d53a3-123">Azure Cosmos DB bills storage and throughput independently on an hourly basis, so you can save costs by deleting or lowering the throughput of your Azure Cosmos DB collections after testing.</span></span>
> 
> 

<span data-ttu-id="d53a3-124">**3. lépés:** fordítsa le és futtassa a konzolalkalmazást a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="d53a3-124">**Step 3:** Compile and run the console app from the command line.</span></span> <span data-ttu-id="d53a3-125">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="d53a3-125">You should see output like the following:</span></span>

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


<span data-ttu-id="d53a3-126">**(Ha szükséges), a 4. lépés:** az eszköze (RU/mp) jelentett átviteli azonosnak kell lennie, vagy nagyobb, mint a gyűjtemény a létesített átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="d53a3-126">**Step 4 (if necessary):** The throughput reported (RU/s) from the tool should be the same or higher than the provisioned throughput of the collection.</span></span> <span data-ttu-id="d53a3-127">Ha nem, a kis lépésekkel DegreeOfParallelism növelése segítenek eléri a határértéket.</span><span class="sxs-lookup"><span data-stu-id="d53a3-127">If not, increasing the DegreeOfParallelism in small increments may help you reach the limit.</span></span> <span data-ttu-id="d53a3-128">Ha az ügyfél alkalmazásból átviteli trületek elindítani az alkalmazást az azonos vagy azoktól eltérő gépeken több példánya segítséget a különböző példányai között létesített eléri.</span><span class="sxs-lookup"><span data-stu-id="d53a3-128">If the throughput from your client app plateaus, launching multiple instances of the app on the same or different machines will help you reach the provisioned limit across the different instances.</span></span> <span data-ttu-id="d53a3-129">Ha ebben a lépésben segítségre van szüksége, kérjük, írjon egy e-mailek askcosmosdb@microsoft.com vagy a támogatási jegy fájl a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d53a3-129">If you need help with this step, please, write an email to askcosmosdb@microsoft.com or file a support ticket from the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="d53a3-130">Miután az alkalmazás futását, próbálkozzon másik [házirendek indexelő](indexing-policies.md) és [konzisztenciaszintek](consistency-levels.md) átviteli sebesség és a késleltetés gyakorolt hatásuk megértéséhez.</span><span class="sxs-lookup"><span data-stu-id="d53a3-130">Once you have the app running, you can try different [Indexing policies](indexing-policies.md) and [Consistency levels](consistency-levels.md) to understand their impact on throughput and latency.</span></span> <span data-ttu-id="d53a3-131">Is ajánlott felülvizsgálni a forráskódot, és a saját tesztelési programcsomagok vagy üzemi környezetben működő alkalmazásokhoz hasonló konfigurációkat valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="d53a3-131">You can also review the source code and implement similar configurations to your own test suites or production applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d53a3-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d53a3-132">Next steps</span></span>
<span data-ttu-id="d53a3-133">Ebben a cikkben azt venni, hogyan hajthat végre a teljesítmény és méretezhetőség tesztelték Cosmos Adatbázisba egy .NET-Konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="d53a3-133">In this article, we looked at how you can perform performance and scale testing with Cosmos DB using a .NET console app.</span></span> <span data-ttu-id="d53a3-134">Tekintse meg az Azure Cosmos DB használatáról további információt az alábbi hivatkozásokat követve.</span><span class="sxs-lookup"><span data-stu-id="d53a3-134">Please refer to the links below for additional information on working with Azure Cosmos DB.</span></span>

* [<span data-ttu-id="d53a3-135">Az Azure Cosmos adatbázis teljesítményének tesztelésekor minta</span><span class="sxs-lookup"><span data-stu-id="d53a3-135">Azure Cosmos DB performance testing sample</span></span>](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [<span data-ttu-id="d53a3-136">Ügyfél-konfigurációs beállítások Azure Cosmos DB teljesítmény javítása érdekében</span><span class="sxs-lookup"><span data-stu-id="d53a3-136">Client configuration options to improve Azure Cosmos DB performance</span></span>](performance-tips.md)
* [<span data-ttu-id="d53a3-137">Kiszolgálóoldali particionálás az Azure Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="d53a3-137">Server-side partitioning in Azure Cosmos DB</span></span>](partition-data.md)


