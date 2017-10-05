---
title: "Azure Cosmos DB Node.js API, az SDK és az erőforrások |} Microsoft Docs"
description: "Tudnivalók a Node.js API és SDK, beleértve a kiadási dátum, használatból való kivonást dátumok és az Azure Cosmos DB Node.js SDK verziói között végrehajtott módosításokat."
services: cosmos-db
documentationcenter: nodejs
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 9d5621fa-0e11-4619-a28b-a19d872bcf37
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4376a5c07b5f00311ce0fe3c0056efdf79c273f9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a><span data-ttu-id="6575e-103">Azure Cosmos DB Node.js SDK: A kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="6575e-103">Azure Cosmos DB Node.js SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6575e-104">.NET</span><span class="sxs-lookup"><span data-stu-id="6575e-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="6575e-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="6575e-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="6575e-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6575e-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="6575e-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="6575e-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="6575e-108">Java</span><span class="sxs-lookup"><span data-stu-id="6575e-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="6575e-109">Python</span><span class="sxs-lookup"><span data-stu-id="6575e-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="6575e-110">REST</span><span class="sxs-lookup"><span data-stu-id="6575e-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="6575e-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="6575e-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="6575e-112">SQL</span><span class="sxs-lookup"><span data-stu-id="6575e-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="6575e-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="6575e-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="6575e-114">NPM</span><span class="sxs-lookup"><span data-stu-id="6575e-114">NPM</span></span>](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td><span data-ttu-id="6575e-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="6575e-115">**API documentation**</span></span></td><td>[<span data-ttu-id="6575e-116">NODE.js API referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="6575e-116">Node.js API reference documentation</span></span>](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td><span data-ttu-id="6575e-117">**SDK telepítési utasításokat**</span><span class="sxs-lookup"><span data-stu-id="6575e-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="6575e-118">Telepítési utasításokat</span><span class="sxs-lookup"><span data-stu-id="6575e-118">Installation instructions</span></span>](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td><span data-ttu-id="6575e-119">**Hozzájárul az SDK**</span><span class="sxs-lookup"><span data-stu-id="6575e-119">**Contribute to SDK**</span></span></td><td>[<span data-ttu-id="6575e-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="6575e-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td><span data-ttu-id="6575e-121">**Példák**</span><span class="sxs-lookup"><span data-stu-id="6575e-121">**Samples**</span></span></td><td>[<span data-ttu-id="6575e-122">NODE.js-Kódminták</span><span class="sxs-lookup"><span data-stu-id="6575e-122">Node.js code samples</span></span>](documentdb-nodejs-samples.md)</td></tr>

<tr><td><span data-ttu-id="6575e-123">**Első lépéseket ismertető oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="6575e-123">**Get started tutorial**</span></span></td><td>[<span data-ttu-id="6575e-124">Ismerkedés a Node.js SDK-val</span><span class="sxs-lookup"><span data-stu-id="6575e-124">Get started with the Node.js SDK</span></span>](documentdb-nodejs-get-started.md)</td></tr>

<tr><td><span data-ttu-id="6575e-125">**Webes alkalmazás oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="6575e-125">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="6575e-126">Azure Cosmos DB használata Node.js-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6575e-126">Build a Node.js web application using Azure Cosmos DB</span></span>](documentdb-nodejs-application.md)</td></tr>

<tr><td><span data-ttu-id="6575e-127">**Aktuális támogatott platform**</span><span class="sxs-lookup"><span data-stu-id="6575e-127">**Current supported platform**</span></span></td><td> 
[<span data-ttu-id="6575e-128">NODE.js 6.x</span><span class="sxs-lookup"><span data-stu-id="6575e-128">Node.js v6.x</span></span>](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[<span data-ttu-id="6575e-129">NODE.js v4.2.0</span><span class="sxs-lookup"><span data-stu-id="6575e-129">Node.js v4.2.0</span></span>](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[<span data-ttu-id="6575e-130">NODE.js v0.12</span><span class="sxs-lookup"><span data-stu-id="6575e-130">Node.js v0.12</span></span>](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
<span data-ttu-id="6575e-131">[NODE.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span><span class="sxs-lookup"><span data-stu-id="6575e-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="6575e-132">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6575e-132">Release notes</span></span>

### <span data-ttu-id="6575e-133"><a name="1.12.2"/>1.12.2</a></span><span class="sxs-lookup"><span data-stu-id="6575e-133"><a name="1.12.2"/>1.12.2</a></span></span>
*   <span data-ttu-id="6575e-134">rögzített npm dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6575e-134">npm documentation fixed.</span></span>

### <span data-ttu-id="6575e-135"><a name="1.12.1"/>1.12.1</a></span><span class="sxs-lookup"><span data-stu-id="6575e-135"><a name="1.12.1"/>1.12.1</a></span></span>
* <span data-ttu-id="6575e-136">Rögzített programhiba executeStoredProcedure ahol érintett dokumentumok kellett speciális Unicode-karaktereket (LS, PS).</span><span class="sxs-lookup"><span data-stu-id="6575e-136">Fixed a bug in executeStoredProcedure where documents involved had special Unicode characters (LS, PS).</span></span>
* <span data-ttu-id="6575e-137">A partíciókulcs az Unicode karaktereket tartalmazó dokumentumok kezelése hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="6575e-137">Fixed a bug in handling documents with Unicode characters in the partition key.</span></span>
* <span data-ttu-id="6575e-138">Gyűjtemények létrehozása a név adathordozó rögzített támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-138">Fixed support for creating collections with the name media.</span></span> <span data-ttu-id="6575e-139">Github probléma #114.</span><span class="sxs-lookup"><span data-stu-id="6575e-139">Github issue #114.</span></span>
* <span data-ttu-id="6575e-140">Engedély engedélyezési jogkivonat rögzített támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-140">Fixed support for permission authorization token.</span></span> <span data-ttu-id="6575e-141">Github probléma #178.</span><span class="sxs-lookup"><span data-stu-id="6575e-141">Github issue #178.</span></span>

### <span data-ttu-id="6575e-142"><a name="1.12.0"/>1.12.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-142"><a name="1.12.0"/>1.12.0</a></span></span>
* <span data-ttu-id="6575e-143">Támogatása egy új [konzisztenciaszint](consistency-levels.md) ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="6575e-143">Added support for a new [consistency level](consistency-levels.md) called ConsistentPrefix.</span></span>
* <span data-ttu-id="6575e-144">UriFactory támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-144">Added support for UriFactory.</span></span>
* <span data-ttu-id="6575e-145">Egy Unicode támogatási hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="6575e-145">Fixed a Unicode support bug.</span></span> <span data-ttu-id="6575e-146">GitHub probléma #171.</span><span class="sxs-lookup"><span data-stu-id="6575e-146">GitHub issue #171.</span></span>

### <span data-ttu-id="6575e-147"><a name="1.11.0"/>1.11.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-147"><a name="1.11.0"/>1.11.0</a></span></span>
* <span data-ttu-id="6575e-148">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-148">Added the support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="6575e-149">A beállítás párhuzamossági a partíció lekérdezések közötti mértékű vezérlése hozzá.</span><span class="sxs-lookup"><span data-stu-id="6575e-149">Added the option for controlling degree of parallelism for cross partition queries.</span></span>
* <span data-ttu-id="6575e-150">A kívánt beállítást SSL ellenőrzési letiltása Azure Cosmos DB emulatorban futtatásakor hozzá.</span><span class="sxs-lookup"><span data-stu-id="6575e-150">Added the option for disabling SSL verification when running against Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="6575e-151">Minimális átviteli sebességet 2500 RU/mp 10,100 RU/mp a particionált gyűjtemények szintűre csökkent.</span><span class="sxs-lookup"><span data-stu-id="6575e-151">Lowered minimum throughput on partitioned collections from 10,100 RU/s to 2500 RU/s.</span></span>
* <span data-ttu-id="6575e-152">A folytatási egypartíciós gyűjtemény token hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="6575e-152">Fixed the continuation token bug for single partition collection.</span></span> <span data-ttu-id="6575e-153">Github probléma #107.</span><span class="sxs-lookup"><span data-stu-id="6575e-153">Github issue #107.</span></span>
* <span data-ttu-id="6575e-154">Rögzített 0 kezelése, egyetlen param executeStoredProcedure hibát.</span><span class="sxs-lookup"><span data-stu-id="6575e-154">Fixed the executeStoredProcedure bug in handling 0 as single param.</span></span> <span data-ttu-id="6575e-155">Github probléma #155.</span><span class="sxs-lookup"><span data-stu-id="6575e-155">Github issue #155.</span></span>

### <span data-ttu-id="6575e-156"><a name="1.10.2"/>1.10.2</a></span><span class="sxs-lookup"><span data-stu-id="6575e-156"><a name="1.10.2"/>1.10.2</a></span></span>
* <span data-ttu-id="6575e-157">Rögzített felhasználói ügynök fejléc az SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="6575e-157">Fixed user-agent header to include the SDK version.</span></span>
* <span data-ttu-id="6575e-158">Kisebb kód karbantartása.</span><span class="sxs-lookup"><span data-stu-id="6575e-158">Minor code cleanup.</span></span>

### <span data-ttu-id="6575e-159"><a name="1.10.1"/>1.10.1</a></span><span class="sxs-lookup"><span data-stu-id="6575e-159"><a name="1.10.1"/>1.10.1</a></span></span>
* <span data-ttu-id="6575e-160">SSL-ellenőrzés letiltása, ha az SDK használatával a emulator(hostname=localhost) céloz.</span><span class="sxs-lookup"><span data-stu-id="6575e-160">Disabling SSL verification when using the SDK to target the emulator(hostname=localhost).</span></span>
* <span data-ttu-id="6575e-161">Támogatja a tárolt eljárás végrehajtása során parancsfájl naplózásának engedélyezéséről.</span><span class="sxs-lookup"><span data-stu-id="6575e-161">Added support for enabling script logging during stored procedure execution.</span></span>

### <span data-ttu-id="6575e-162"><a name="1.10.0"/>1.10.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-162"><a name="1.10.0"/>1.10.0</a></span></span>
* <span data-ttu-id="6575e-163">Támogatja a párhuzamos lekérdezések partíció közötti.</span><span class="sxs-lookup"><span data-stu-id="6575e-163">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="6575e-164">FELSŐ/ORDER BY lekérdezések a particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-164">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>

### <span data-ttu-id="6575e-165"><a name="1.9.0"/>1.9.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-165"><a name="1.9.0"/>1.9.0</a></span></span>
* <span data-ttu-id="6575e-166">Szabályozottan halmozott kérelmek hozzáadott újrapróbálkozási házirend támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-166">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="6575e-167">(A szabályozottan halmozott kérelmek egy kérelem aránya túl nagy kivétel, hibakód 429 kapnak.) Alapértelmezés szerint Azure Cosmos DB újrapróbálja kilenc alkalommal az egyes kérelmek 429. Hibakód: a rendszer észlelt, amikor az retryAfter idő a válaszfejlécet érvényesítenie.</span><span class="sxs-lookup"><span data-stu-id="6575e-167">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring the retryAfter time in the response header.</span></span> <span data-ttu-id="6575e-168">A rögzített újrapróbálkozási időköz most már beállítható a RetryOptions tulajdonság részeként a ConnectionPolicy objektum Ha azt szeretné, figyelmen kívül hagyja a próbálkozások közötti kiszolgáló által visszaadott retryAfter idő.</span><span class="sxs-lookup"><span data-stu-id="6575e-168">A fixed retry interval time can now be set as part of the RetryOptions property on the ConnectionPolicy object if you want to ignore the retryAfter time returned by server between the retries.</span></span> <span data-ttu-id="6575e-169">Azure Cosmos-adatbázis most megvárja, legfeljebb 30 másodpercig (függetlenül újrapróbálkozások száma) leszabályozta, és visszaadja a választ 429. Hibakód: minden egyes kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="6575e-169">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns the response with error code 429.</span></span> <span data-ttu-id="6575e-170">Most is felülbírálható lesz a RetryOptions tulajdonságban ConnectionPolicy objektumon.</span><span class="sxs-lookup"><span data-stu-id="6575e-170">This time can also be overridden in the RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="6575e-171">Cosmos DB most adja vissza x-ms-szabályozási--újrapróbálkozások és x-ms-throttle-retry-wait-time-ms a válaszfejlécek minden kérelemben a késleltetési jelöléséhez újra számát és a próbálkozások közötti várta a kérelem összesített idő szerint.</span><span class="sxs-lookup"><span data-stu-id="6575e-171">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as the response headers in every request to denote the throttle retry count and the cumulative time the request waited between the retries.</span></span>
* <span data-ttu-id="6575e-172">A RetryOptions osztály adott, az ilyen a ConnectionPolicy osztály, amely felülbírálhatja az alapértelmezett beállítások némelyike használható a RetryOptions tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="6575e-172">The RetryOptions class was added, exposing the RetryOptions property on the ConnectionPolicy class that can be used to override some of the default retry options.</span></span>

### <span data-ttu-id="6575e-173"><a name="1.8.0"/>1.8.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-173"><a name="1.8.0"/>1.8.0</a></span></span>
* <span data-ttu-id="6575e-174">Több területi adatbázis fiókok támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-174">Added the support for multi-region database accounts.</span></span>

### <span data-ttu-id="6575e-175"><a name="1.7.0"/>1.7.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-175"><a name="1.7.0"/>1.7.0</a></span></span>
* <span data-ttu-id="6575e-176">Idő a Live(TTL) szolgáltatás dokumentumok támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-176">Added the support for Time To Live(TTL) feature for documents.</span></span>

### <span data-ttu-id="6575e-177"><a name="1.6.0"/>1.6.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-177"><a name="1.6.0"/>1.6.0</a></span></span>
* <span data-ttu-id="6575e-178">Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="6575e-178">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <span data-ttu-id="6575e-179"><a name="1.5.6"/>1.5.6</a></span><span class="sxs-lookup"><span data-stu-id="6575e-179"><a name="1.5.6"/>1.5.6</a></span></span>
* <span data-ttu-id="6575e-180">Javított RangePartitionResolver.resolveForRead hibát, ahol azt lett nem ad vissza hivatkozások eredmények rossz concat miatt.</span><span class="sxs-lookup"><span data-stu-id="6575e-180">Fixed RangePartitionResolver.resolveForRead bug where it was not returning links due to a bad concat of results.</span></span>

### <span data-ttu-id="6575e-181"><a name="1.5.5"/>1.5.5</a></span><span class="sxs-lookup"><span data-stu-id="6575e-181"><a name="1.5.5"/>1.5.5</a></span></span>
* <span data-ttu-id="6575e-182">Rögzített hashParitionResolver resolveForRead(): Ha nincs megadva partíciókulcs kivétel, helyett sorolja fel az összes regisztrált hivatkozások kiváltása volt.</span><span class="sxs-lookup"><span data-stu-id="6575e-182">Fixed hashParitionResolver resolveForRead(): When no partition key supplied was throwing exception, instead of returning a list of all registered links.</span></span>

### <span data-ttu-id="6575e-183"><a name="1.5.4"/>1.5.4</a></span><span class="sxs-lookup"><span data-stu-id="6575e-183"><a name="1.5.4"/>1.5.4</a></span></span>
* <span data-ttu-id="6575e-184">Kijavítja a hibát [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -HTTPS-ügynök dedikált: elkerülése érdekében a globális ügynök Azure Cosmos DB célokra módosítása.</span><span class="sxs-lookup"><span data-stu-id="6575e-184">Fixes issue [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - Dedicated HTTPS Agent: Avoid modifying the global agent for Azure Cosmos DB purposes.</span></span> <span data-ttu-id="6575e-185">Használja a dedikált ügynököt az összes a lib kérelmek.</span><span class="sxs-lookup"><span data-stu-id="6575e-185">Use a dedicated agent for all of the lib’s requests.</span></span>

### <span data-ttu-id="6575e-186"><a name="1.5.3"/>1.5.3</a></span><span class="sxs-lookup"><span data-stu-id="6575e-186"><a name="1.5.3"/>1.5.3</a></span></span>
* <span data-ttu-id="6575e-187">Kijavítja a hibát [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - megfelelően kötőjelek az adathordozó-azonosítók kezelésére.</span><span class="sxs-lookup"><span data-stu-id="6575e-187">Fixes issue [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - Properly handle dashes in media ids.</span></span>

### <span data-ttu-id="6575e-188"><a name="1.5.2"/>1.5.2</a></span><span class="sxs-lookup"><span data-stu-id="6575e-188"><a name="1.5.2"/>1.5.2</a></span></span>
* <span data-ttu-id="6575e-189">Kijavítja a hibát [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter figyelő szivárgás lépett fel, figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="6575e-189">Fixes issue [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter listener leak warning.</span></span>

### <span data-ttu-id="6575e-190"><a name="1.5.1"/>1.5.1</a></span><span class="sxs-lookup"><span data-stu-id="6575e-190"><a name="1.5.1"/>1.5.1</a></span></span>
* <span data-ttu-id="6575e-191">Kijavítja a hibát [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -nevezze át a mappát kivonat kivonatoló rendszerek kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="6575e-191">Fixes issue [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - rename folder Hash to hash for case-sensitive systems.</span></span>

### <span data-ttu-id="6575e-192"><a name="1.5.0"/>1.5.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-192"><a name="1.5.0"/>1.5.0</a></span></span>
* <span data-ttu-id="6575e-193">Horizontális segítséget nyújt kivonatoló & tartomány partíció feloldókat hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="6575e-193">Implement sharding support by adding hash & range partition resolvers.</span></span>

### <span data-ttu-id="6575e-194"><a name="1.4.0"/>1.4.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-194"><a name="1.4.0"/>1.4.0</a></span></span>
* <span data-ttu-id="6575e-195">Upsert megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="6575e-195">Implement Upsert.</span></span> <span data-ttu-id="6575e-196">Új upsertXXX metódusai documentClient.</span><span class="sxs-lookup"><span data-stu-id="6575e-196">New upsertXXX methods on documentClient.</span></span>

### <span data-ttu-id="6575e-197"><a name="1.3.0"/>1.3.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-197"><a name="1.3.0"/>1.3.0</a></span></span>
* <span data-ttu-id="6575e-198">Kihagyott verziószámok átveendő Csomagjától összehangolás.</span><span class="sxs-lookup"><span data-stu-id="6575e-198">Skipped to bring version numbers in alignment with other SDKs.</span></span>

### <span data-ttu-id="6575e-199"><a name="1.2.2"/>1.2.2</a></span><span class="sxs-lookup"><span data-stu-id="6575e-199"><a name="1.2.2"/>1.2.2</a></span></span>
* <span data-ttu-id="6575e-200">Vegyes Q kihasználásának köszönhetően akár még a burkoló új tárházba.</span><span class="sxs-lookup"><span data-stu-id="6575e-200">Split Q promises wrapper to new repository.</span></span>
* <span data-ttu-id="6575e-201">Frissítse az npm beállításjegyzék csomagfájlt.</span><span class="sxs-lookup"><span data-stu-id="6575e-201">Update to package file for npm registry.</span></span>

### <span data-ttu-id="6575e-202"><a name="1.2.1"/>1.2.1</a></span><span class="sxs-lookup"><span data-stu-id="6575e-202"><a name="1.2.1"/>1.2.1</a></span></span>
* <span data-ttu-id="6575e-203">Megvalósítja-alapú útválasztási azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6575e-203">Implements ID Based Routing.</span></span>
* <span data-ttu-id="6575e-204">Kijavítja a hibát [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuális tulajdonság metódus current() ütközik.</span><span class="sxs-lookup"><span data-stu-id="6575e-204">Fixes Issue [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - current property conflicts with method current().</span></span>

### <span data-ttu-id="6575e-205"><a name="1.2.0"/>1.2.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-205"><a name="1.2.0"/>1.2.0</a></span></span>
* <span data-ttu-id="6575e-206">A földrajzi index támogatása.</span><span class="sxs-lookup"><span data-stu-id="6575e-206">Added support for GeoSpatial index.</span></span>
* <span data-ttu-id="6575e-207">Ellenőrzi az összes erőforrás id tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="6575e-207">Validates id property for all resources.</span></span> <span data-ttu-id="6575e-208">Erőforrások azonosító nem tartalmazhat?, /, #, &#47; &#47; karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="6575e-208">Ids for resources cannot contain ?, /, #, &#47;&#47;, characters or end with a space.</span></span>
* <span data-ttu-id="6575e-209">Új fejléc "index átalakítása folyamatban" hozzáadása ResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="6575e-209">Adds new header "index transformation progress" to ResourceResponse.</span></span>

### <span data-ttu-id="6575e-210"><a name="1.1.0"/>1.1.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-210"><a name="1.1.0"/>1.1.0</a></span></span>
* <span data-ttu-id="6575e-211">V2 indexelési házirendet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="6575e-211">Implements V2 indexing policy.</span></span>

### <span data-ttu-id="6575e-212"><a name="1.0.3"/>1.0.3</a></span><span class="sxs-lookup"><span data-stu-id="6575e-212"><a name="1.0.3"/>1.0.3</a></span></span>
* <span data-ttu-id="6575e-213">A probléma [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - eslint megvalósítása és a core-konfigurációja grunt és SDK készletéről.</span><span class="sxs-lookup"><span data-stu-id="6575e-213">Issue [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - Implemented eslint and grunt configurations in the core and promise SDK.</span></span>

### <span data-ttu-id="6575e-214"><a name="1.0.2"/>1.0.2</a></span><span class="sxs-lookup"><span data-stu-id="6575e-214"><a name="1.0.2"/>1.0.2</a></span></span>
* <span data-ttu-id="6575e-215">A probléma [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -tett burkoló nem tartalmazza a hiba: a fejlécben.</span><span class="sxs-lookup"><span data-stu-id="6575e-215">Issue [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises wrapper does not include header with error.</span></span>

### <span data-ttu-id="6575e-216"><a name="1.0.1"/>1.0.1</a></span><span class="sxs-lookup"><span data-stu-id="6575e-216"><a name="1.0.1"/>1.0.1</a></span></span>
* <span data-ttu-id="6575e-217">Az ütközések readConflicts, readConflictAsync és queryConflicts hozzáadásával lekérdezés megvalósított képessége.</span><span class="sxs-lookup"><span data-stu-id="6575e-217">Implemented ability to query for conflicts by adding readConflicts, readConflictAsync, and queryConflicts.</span></span>
* <span data-ttu-id="6575e-218">Frissített API dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6575e-218">Updated API documentation.</span></span>
* <span data-ttu-id="6575e-219">A probléma [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync hiba.</span><span class="sxs-lookup"><span data-stu-id="6575e-219">Issue [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync error.</span></span>

### <span data-ttu-id="6575e-220"><a name="1.0.0"/>1.0.0</a></span><span class="sxs-lookup"><span data-stu-id="6575e-220"><a name="1.0.0"/>1.0.0</a></span></span>
* <span data-ttu-id="6575e-221">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="6575e-221">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="6575e-222">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="6575e-222">Release & Retirement Dates</span></span>
<span data-ttu-id="6575e-223">A Microsoft biztosít értesítési legalább **12 hónapon keresztül** SDK eltávolítása érdekében vagy újabb támogatott verzióra való áttérés előtt.</span><span class="sxs-lookup"><span data-stu-id="6575e-223">Microsoft provides notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="6575e-224">Új szolgáltatásait és funkcióit és optimalizálás csak hozzá az aktuális SDK, így javasoljuk, hogy mindig a legújabb SDK verzióra frissít legkorábban lehető.</span><span class="sxs-lookup"><span data-stu-id="6575e-224">New features and functionality and optimizations are only added to the current SDK, as such it is  recommended that you always upgrade to the latest SDK version as early as possible.</span></span>

<span data-ttu-id="6575e-225">A Cosmos DB használatával kivont SDK kell elutasította a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6575e-225">Any request to Cosmos DB using a retired SDK is be rejected by the service.</span></span>

<br/>

| <span data-ttu-id="6575e-226">Verzió</span><span class="sxs-lookup"><span data-stu-id="6575e-226">Version</span></span> | <span data-ttu-id="6575e-227">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="6575e-227">Release Date</span></span> | <span data-ttu-id="6575e-228">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="6575e-228">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="6575e-229">1.12.2</span><span class="sxs-lookup"><span data-stu-id="6575e-229">1.12.2</span></span>](#1.12.2) |<span data-ttu-id="6575e-230">2017. augusztus 10.</span><span class="sxs-lookup"><span data-stu-id="6575e-230">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="6575e-231">1.12.1</span><span class="sxs-lookup"><span data-stu-id="6575e-231">1.12.1</span></span>](#1.12.1) |<span data-ttu-id="6575e-232">2017. augusztus 10.</span><span class="sxs-lookup"><span data-stu-id="6575e-232">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="6575e-233">1.12.0</span><span class="sxs-lookup"><span data-stu-id="6575e-233">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="6575e-234">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="6575e-234">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="6575e-235">1.11.0</span><span class="sxs-lookup"><span data-stu-id="6575e-235">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="6575e-236">2017. március 16.</span><span class="sxs-lookup"><span data-stu-id="6575e-236">March 16, 2017</span></span> |--- |
| [<span data-ttu-id="6575e-237">1.10.2</span><span class="sxs-lookup"><span data-stu-id="6575e-237">1.10.2</span></span>](#1.10.2) |<span data-ttu-id="6575e-238">2017. január 27.</span><span class="sxs-lookup"><span data-stu-id="6575e-238">January 27, 2017</span></span> |--- |
| [<span data-ttu-id="6575e-239">1.10.1</span><span class="sxs-lookup"><span data-stu-id="6575e-239">1.10.1</span></span>](#1.10.1) |<span data-ttu-id="6575e-240">2016. december 22.</span><span class="sxs-lookup"><span data-stu-id="6575e-240">December 22, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-241">1.10.0</span><span class="sxs-lookup"><span data-stu-id="6575e-241">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="6575e-242">2016. október 03.</span><span class="sxs-lookup"><span data-stu-id="6575e-242">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-243">1.9.0</span><span class="sxs-lookup"><span data-stu-id="6575e-243">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="6575e-244">2016. július 07.</span><span class="sxs-lookup"><span data-stu-id="6575e-244">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-245">1.8.0</span><span class="sxs-lookup"><span data-stu-id="6575e-245">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="6575e-246">2016. június 14.</span><span class="sxs-lookup"><span data-stu-id="6575e-246">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-247">1.7.0</span><span class="sxs-lookup"><span data-stu-id="6575e-247">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="6575e-248">2016. április 26.</span><span class="sxs-lookup"><span data-stu-id="6575e-248">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-249">1.6.0</span><span class="sxs-lookup"><span data-stu-id="6575e-249">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="6575e-250">2016. március 29.</span><span class="sxs-lookup"><span data-stu-id="6575e-250">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-251">1.5.6</span><span class="sxs-lookup"><span data-stu-id="6575e-251">1.5.6</span></span>](#1.5.6) |<span data-ttu-id="6575e-252">2016. március 08.</span><span class="sxs-lookup"><span data-stu-id="6575e-252">March 08, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-253">1.5.5</span><span class="sxs-lookup"><span data-stu-id="6575e-253">1.5.5</span></span>](#1.5.5) |<span data-ttu-id="6575e-254">2016. február 02.</span><span class="sxs-lookup"><span data-stu-id="6575e-254">February 02, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-255">1.5.4</span><span class="sxs-lookup"><span data-stu-id="6575e-255">1.5.4</span></span>](#1.5.4) |<span data-ttu-id="6575e-256">2016. február 01.</span><span class="sxs-lookup"><span data-stu-id="6575e-256">February 01, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-257">1.5.2</span><span class="sxs-lookup"><span data-stu-id="6575e-257">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="6575e-258">2016. január 26.</span><span class="sxs-lookup"><span data-stu-id="6575e-258">January 26, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-259">1.5.2</span><span class="sxs-lookup"><span data-stu-id="6575e-259">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="6575e-260">2016. január 22.</span><span class="sxs-lookup"><span data-stu-id="6575e-260">January 22, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-261">1.5.1</span><span class="sxs-lookup"><span data-stu-id="6575e-261">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="6575e-262">2016. január 4.</span><span class="sxs-lookup"><span data-stu-id="6575e-262">January 4, 2016</span></span> |--- |
| [<span data-ttu-id="6575e-263">1.5.0</span><span class="sxs-lookup"><span data-stu-id="6575e-263">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="6575e-264">2015. december 31.</span><span class="sxs-lookup"><span data-stu-id="6575e-264">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-265">1.4.0</span><span class="sxs-lookup"><span data-stu-id="6575e-265">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="6575e-266">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="6575e-266">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-267">1.3.0</span><span class="sxs-lookup"><span data-stu-id="6575e-267">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="6575e-268">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="6575e-268">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-269">1.2.2</span><span class="sxs-lookup"><span data-stu-id="6575e-269">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="6575e-270">2015. szeptember 10.</span><span class="sxs-lookup"><span data-stu-id="6575e-270">September 10, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-271">1.2.1</span><span class="sxs-lookup"><span data-stu-id="6575e-271">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="6575e-272">2015. augusztus 15.</span><span class="sxs-lookup"><span data-stu-id="6575e-272">August 15, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-273">1.2.0</span><span class="sxs-lookup"><span data-stu-id="6575e-273">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="6575e-274">2015. augusztus 05.</span><span class="sxs-lookup"><span data-stu-id="6575e-274">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-275">1.1.0</span><span class="sxs-lookup"><span data-stu-id="6575e-275">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="6575e-276">2015. július 09.</span><span class="sxs-lookup"><span data-stu-id="6575e-276">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-277">1.0.3</span><span class="sxs-lookup"><span data-stu-id="6575e-277">1.0.3</span></span>](#1.0.3) |<span data-ttu-id="6575e-278">2015. június 04.</span><span class="sxs-lookup"><span data-stu-id="6575e-278">June 04, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-279">1.0.2</span><span class="sxs-lookup"><span data-stu-id="6575e-279">1.0.2</span></span>](#1.0.2) |<span data-ttu-id="6575e-280">2015. május 23.</span><span class="sxs-lookup"><span data-stu-id="6575e-280">May 23, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-281">1.0.1</span><span class="sxs-lookup"><span data-stu-id="6575e-281">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="6575e-282">2015. május 15.</span><span class="sxs-lookup"><span data-stu-id="6575e-282">May 15, 2015</span></span> |--- |
| [<span data-ttu-id="6575e-283">1.0.0</span><span class="sxs-lookup"><span data-stu-id="6575e-283">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="6575e-284">2015. áprilisi 08</span><span class="sxs-lookup"><span data-stu-id="6575e-284">April 08, 2015</span></span> |--- |

## <a name="faq"></a><span data-ttu-id="6575e-285">GYIK</span><span class="sxs-lookup"><span data-stu-id="6575e-285">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="6575e-286">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6575e-286">See also</span></span>
<span data-ttu-id="6575e-287">A Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="6575e-287">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

