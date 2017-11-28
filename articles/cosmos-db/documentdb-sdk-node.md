---
title: "aaaAzure Cosmos DB Node.js API SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello Node.js API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB Node.js SDK verziói között."
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
ms.openlocfilehash: d450b9a9ea7b0f4717ddae8940121fc458ea3744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-nodejs-sdk-release-notes-and-resources"></a><span data-ttu-id="06bf7-103">Azure Cosmos DB Node.js SDK: A kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="06bf7-103">Azure Cosmos DB Node.js SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="06bf7-104">.NET</span><span class="sxs-lookup"><span data-stu-id="06bf7-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="06bf7-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="06bf7-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="06bf7-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="06bf7-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="06bf7-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="06bf7-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="06bf7-108">Java</span><span class="sxs-lookup"><span data-stu-id="06bf7-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="06bf7-109">Python</span><span class="sxs-lookup"><span data-stu-id="06bf7-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="06bf7-110">REST</span><span class="sxs-lookup"><span data-stu-id="06bf7-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="06bf7-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="06bf7-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="06bf7-112">SQL</span><span class="sxs-lookup"><span data-stu-id="06bf7-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="06bf7-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="06bf7-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="06bf7-114">NPM</span><span class="sxs-lookup"><span data-stu-id="06bf7-114">NPM</span></span>](https://www.npmjs.com/package/documentdb)</td></tr>

<tr><td><span data-ttu-id="06bf7-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="06bf7-115">**API documentation**</span></span></td><td>[<span data-ttu-id="06bf7-116">NODE.js API referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="06bf7-116">Node.js API reference documentation</span></span>](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>

<tr><td><span data-ttu-id="06bf7-117">**SDK telepítési utasításokat**</span><span class="sxs-lookup"><span data-stu-id="06bf7-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="06bf7-118">Telepítési utasításokat</span><span class="sxs-lookup"><span data-stu-id="06bf7-118">Installation instructions</span></span>](http://azure.github.io/azure-documentdb-node/)</td></tr>

<tr><td><span data-ttu-id="06bf7-119">**Közreműködési lehetőségek tooSDK**</span><span class="sxs-lookup"><span data-stu-id="06bf7-119">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="06bf7-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="06bf7-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>

<tr><td><span data-ttu-id="06bf7-121">**Példák**</span><span class="sxs-lookup"><span data-stu-id="06bf7-121">**Samples**</span></span></td><td>[<span data-ttu-id="06bf7-122">NODE.js-Kódminták</span><span class="sxs-lookup"><span data-stu-id="06bf7-122">Node.js code samples</span></span>](documentdb-nodejs-samples.md)</td></tr>

<tr><td><span data-ttu-id="06bf7-123">**Első lépéseket ismertető oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="06bf7-123">**Get started tutorial**</span></span></td><td>[<span data-ttu-id="06bf7-124">Ismerkedés a Node.js SDK hello</span><span class="sxs-lookup"><span data-stu-id="06bf7-124">Get started with hello Node.js SDK</span></span>](documentdb-nodejs-get-started.md)</td></tr>

<tr><td><span data-ttu-id="06bf7-125">**Webes alkalmazás oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="06bf7-125">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="06bf7-126">Azure Cosmos DB használata Node.js-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="06bf7-126">Build a Node.js web application using Azure Cosmos DB</span></span>](documentdb-nodejs-application.md)</td></tr>

<tr><td><span data-ttu-id="06bf7-127">**Aktuális támogatott platform**</span><span class="sxs-lookup"><span data-stu-id="06bf7-127">**Current supported platform**</span></span></td><td> 
[<span data-ttu-id="06bf7-128">NODE.js 6.x</span><span class="sxs-lookup"><span data-stu-id="06bf7-128">Node.js v6.x</span></span>](https://nodejs.org/en/blog/release/v6.10.3/)<br/> 
[<span data-ttu-id="06bf7-129">NODE.js v4.2.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-129">Node.js v4.2.0</span></span>](https://nodejs.org/en/blog/release/v4.2.0/)<br/> 
[<span data-ttu-id="06bf7-130">NODE.js v0.12</span><span class="sxs-lookup"><span data-stu-id="06bf7-130">Node.js v0.12</span></span>](https://nodejs.org/en/blog/release/v0.12.0/)<br/> 
<span data-ttu-id="06bf7-131">[NODE.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span><span class="sxs-lookup"><span data-stu-id="06bf7-131">[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/) 
</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="06bf7-132">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="06bf7-132">Release notes</span></span>

### <span data-ttu-id="06bf7-133"><a name="1.12.2"/>1.12.2</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-133"><a name="1.12.2"/>1.12.2</a></span></span>
*   <span data-ttu-id="06bf7-134">rögzített npm dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="06bf7-134">npm documentation fixed.</span></span>

### <span data-ttu-id="06bf7-135"><a name="1.12.1"/>1.12.1</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-135"><a name="1.12.1"/>1.12.1</a></span></span>
* <span data-ttu-id="06bf7-136">Rögzített programhiba executeStoredProcedure ahol érintett dokumentumok kellett speciális Unicode-karaktereket (LS, PS).</span><span class="sxs-lookup"><span data-stu-id="06bf7-136">Fixed a bug in executeStoredProcedure where documents involved had special Unicode characters (LS, PS).</span></span>
* <span data-ttu-id="06bf7-137">Unicode-karaktereket a hello partíciókulcs-dokumentumok kezelése hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="06bf7-137">Fixed a bug in handling documents with Unicode characters in hello partition key.</span></span>
* <span data-ttu-id="06bf7-138">Gyűjtemények létrehozása hello neve adathordozó rögzített támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-138">Fixed support for creating collections with hello name media.</span></span> <span data-ttu-id="06bf7-139">Github probléma #114.</span><span class="sxs-lookup"><span data-stu-id="06bf7-139">Github issue #114.</span></span>
* <span data-ttu-id="06bf7-140">Engedély engedélyezési jogkivonat rögzített támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-140">Fixed support for permission authorization token.</span></span> <span data-ttu-id="06bf7-141">Github probléma #178.</span><span class="sxs-lookup"><span data-stu-id="06bf7-141">Github issue #178.</span></span>

### <span data-ttu-id="06bf7-142"><a name="1.12.0"/>1.12.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-142"><a name="1.12.0"/>1.12.0</a></span></span>
* <span data-ttu-id="06bf7-143">Támogatása egy új [konzisztenciaszint](consistency-levels.md) ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="06bf7-143">Added support for a new [consistency level](consistency-levels.md) called ConsistentPrefix.</span></span>
* <span data-ttu-id="06bf7-144">UriFactory támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-144">Added support for UriFactory.</span></span>
* <span data-ttu-id="06bf7-145">Egy Unicode támogatási hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="06bf7-145">Fixed a Unicode support bug.</span></span> <span data-ttu-id="06bf7-146">GitHub probléma #171.</span><span class="sxs-lookup"><span data-stu-id="06bf7-146">GitHub issue #171.</span></span>

### <span data-ttu-id="06bf7-147"><a name="1.11.0"/>1.11.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-147"><a name="1.11.0"/>1.11.0</a></span></span>
* <span data-ttu-id="06bf7-148">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) hozzáadott hello támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-148">Added hello support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="06bf7-149">A hozzáadott hello beállítás párhuzamossági a partíció lekérdezések közötti mértékű vezérlése.</span><span class="sxs-lookup"><span data-stu-id="06bf7-149">Added hello option for controlling degree of parallelism for cross partition queries.</span></span>
* <span data-ttu-id="06bf7-150">Hozzáadott hello beállítás SSL ellenőrzési letiltását Azure Cosmos DB emulatorban futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="06bf7-150">Added hello option for disabling SSL verification when running against Azure Cosmos DB Emulator.</span></span>
* <span data-ttu-id="06bf7-151">A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.</span><span class="sxs-lookup"><span data-stu-id="06bf7-151">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>
* <span data-ttu-id="06bf7-152">Rögzített hello folytatási token hiba egypartíciós gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="06bf7-152">Fixed hello continuation token bug for single partition collection.</span></span> <span data-ttu-id="06bf7-153">Github probléma #107.</span><span class="sxs-lookup"><span data-stu-id="06bf7-153">Github issue #107.</span></span>
* <span data-ttu-id="06bf7-154">Rögzített hello executeStoredProcedure hiba egyetlen param 0 foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="06bf7-154">Fixed hello executeStoredProcedure bug in handling 0 as single param.</span></span> <span data-ttu-id="06bf7-155">Github probléma #155.</span><span class="sxs-lookup"><span data-stu-id="06bf7-155">Github issue #155.</span></span>

### <span data-ttu-id="06bf7-156"><a name="1.10.2"/>1.10.2</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-156"><a name="1.10.2"/>1.10.2</a></span></span>
* <span data-ttu-id="06bf7-157">Rögzített felhasználói ügynök fejléc tooinclude hello SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="06bf7-157">Fixed user-agent header tooinclude hello SDK version.</span></span>
* <span data-ttu-id="06bf7-158">Kisebb kód karbantartása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-158">Minor code cleanup.</span></span>

### <span data-ttu-id="06bf7-159"><a name="1.10.1"/>1.10.1</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-159"><a name="1.10.1"/>1.10.1</a></span></span>
* <span data-ttu-id="06bf7-160">SSL-ellenőrzés letiltása, hello SDK tootarget hello emulator(hostname=localhost) használatakor.</span><span class="sxs-lookup"><span data-stu-id="06bf7-160">Disabling SSL verification when using hello SDK tootarget hello emulator(hostname=localhost).</span></span>
* <span data-ttu-id="06bf7-161">Támogatja a tárolt eljárás végrehajtása során parancsfájl naplózásának engedélyezéséről.</span><span class="sxs-lookup"><span data-stu-id="06bf7-161">Added support for enabling script logging during stored procedure execution.</span></span>

### <span data-ttu-id="06bf7-162"><a name="1.10.0"/>1.10.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-162"><a name="1.10.0"/>1.10.0</a></span></span>
* <span data-ttu-id="06bf7-163">Támogatja a párhuzamos lekérdezések partíció közötti.</span><span class="sxs-lookup"><span data-stu-id="06bf7-163">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="06bf7-164">FELSŐ/ORDER BY lekérdezések a particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-164">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>

### <span data-ttu-id="06bf7-165"><a name="1.9.0"/>1.9.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-165"><a name="1.9.0"/>1.9.0</a></span></span>
* <span data-ttu-id="06bf7-166">Szabályozottan halmozott kérelmek hozzáadott újrapróbálkozási házirend támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-166">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="06bf7-167">(A szabályozottan halmozott kérelmek egy kérelem aránya túl nagy kivétel, hibakód 429 kapnak.) Alapértelmezés szerint Azure Cosmos DB újrapróbálja kilenc alkalommal az egyes kérelmek 429. Hibakód: a rendszer észlelt, amikor hello retryAfter idő hello válaszfejléc érvényesítenie.</span><span class="sxs-lookup"><span data-stu-id="06bf7-167">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring hello retryAfter time in hello response header.</span></span> <span data-ttu-id="06bf7-168">A rögzített újrapróbálkozási időköz most már beállítható hello RetryOptions tulajdonság részeként hello ConnectionPolicy objektum Ha azt szeretné, hogy a kiszolgáló által visszaadott hello újrapróbálkozások között tooignore hello retryAfter idő.</span><span class="sxs-lookup"><span data-stu-id="06bf7-168">A fixed retry interval time can now be set as part of hello RetryOptions property on hello ConnectionPolicy object if you want tooignore hello retryAfter time returned by server between hello retries.</span></span> <span data-ttu-id="06bf7-169">Azure Cosmos-adatbázis most megvárja, legfeljebb 30 másodpercig minden egyes kérelemhez (függetlenül újrapróbálkozások száma) leszabályozta, és hibakód 429 hello választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="06bf7-169">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns hello response with error code 429.</span></span> <span data-ttu-id="06bf7-170">Most is a hello RetryOptions tulajdonság azon objektumhoz, ConnectionPolicy felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="06bf7-170">This time can also be overridden in hello RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="06bf7-171">Cosmos DB most adja vissza x-ms-szabályozási--újrapróbálkozások és x-ms-throttle-retry-wait-time-ms, hello válaszfejlécek minden kérelem toodenote hello késleltetési száma és hello összesített ideje hello kérelem hello újrapróbálkozások között várt próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="06bf7-171">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as hello response headers in every request toodenote hello throttle retry count and hello cumulative time hello request waited between hello retries.</span></span>
* <span data-ttu-id="06bf7-172">hello RetryOptions osztály adott, az ilyen hello RetryOptions tulajdonság hello ConnectionPolicy osztály, amely használt toooverride néhány hello alapértelmezett újrapróbálkozási beállítások.</span><span class="sxs-lookup"><span data-stu-id="06bf7-172">hello RetryOptions class was added, exposing hello RetryOptions property on hello ConnectionPolicy class that can be used toooverride some of hello default retry options.</span></span>

### <span data-ttu-id="06bf7-173"><a name="1.8.0"/>1.8.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-173"><a name="1.8.0"/>1.8.0</a></span></span>
* <span data-ttu-id="06bf7-174">Több területi adatbázis fiókok hozzáadott hello támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-174">Added hello support for multi-region database accounts.</span></span>

### <span data-ttu-id="06bf7-175"><a name="1.7.0"/>1.7.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-175"><a name="1.7.0"/>1.7.0</a></span></span>
* <span data-ttu-id="06bf7-176">Hozzáadott hello idő tooLive(TTL) szolgáltatás dokumentumok támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-176">Added hello support for Time tooLive(TTL) feature for documents.</span></span>

### <span data-ttu-id="06bf7-177"><a name="1.6.0"/>1.6.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-177"><a name="1.6.0"/>1.6.0</a></span></span>
* <span data-ttu-id="06bf7-178">Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="06bf7-178">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <span data-ttu-id="06bf7-179"><a name="1.5.6"/>1.5.6</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-179"><a name="1.5.6"/>1.5.6</a></span></span>
* <span data-ttu-id="06bf7-180">Rögzített RangePartitionResolver.resolveForRead hiba, ha azt lett nem ad vissza hivatkozások miatt hibás concat tooa eredmények.</span><span class="sxs-lookup"><span data-stu-id="06bf7-180">Fixed RangePartitionResolver.resolveForRead bug where it was not returning links due tooa bad concat of results.</span></span>

### <span data-ttu-id="06bf7-181"><a name="1.5.5"/>1.5.5</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-181"><a name="1.5.5"/>1.5.5</a></span></span>
* <span data-ttu-id="06bf7-182">Rögzített hashParitionResolver resolveForRead(): Ha nincs megadva partíciókulcs kivétel, helyett sorolja fel az összes regisztrált hivatkozások kiváltása volt.</span><span class="sxs-lookup"><span data-stu-id="06bf7-182">Fixed hashParitionResolver resolveForRead(): When no partition key supplied was throwing exception, instead of returning a list of all registered links.</span></span>

### <span data-ttu-id="06bf7-183"><a name="1.5.4"/>1.5.4</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-183"><a name="1.5.4"/>1.5.4</a></span></span>
* <span data-ttu-id="06bf7-184">Kijavítja a hibát [#100](https://github.com/Azure/azure-documentdb-node/issues/100) -HTTPS-ügynök dedikált: elkerülése hello globális ügynök Azure Cosmos DB célokra módosítása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-184">Fixes issue [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - Dedicated HTTPS Agent: Avoid modifying hello global agent for Azure Cosmos DB purposes.</span></span> <span data-ttu-id="06bf7-185">Használja a dedikált ügynököt az összes hello lib kérelmek.</span><span class="sxs-lookup"><span data-stu-id="06bf7-185">Use a dedicated agent for all of hello lib’s requests.</span></span>

### <span data-ttu-id="06bf7-186"><a name="1.5.3"/>1.5.3</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-186"><a name="1.5.3"/>1.5.3</a></span></span>
* <span data-ttu-id="06bf7-187">Kijavítja a hibát [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - megfelelően kötőjelek az adathordozó-azonosítók kezelésére.</span><span class="sxs-lookup"><span data-stu-id="06bf7-187">Fixes issue [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - Properly handle dashes in media ids.</span></span>

### <span data-ttu-id="06bf7-188"><a name="1.5.2"/>1.5.2</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-188"><a name="1.5.2"/>1.5.2</a></span></span>
* <span data-ttu-id="06bf7-189">Kijavítja a hibát [#95](https://github.com/Azure/azure-documentdb-node/issues/95) -EventEmitter figyelő szivárgás lépett fel, figyelmeztetés.</span><span class="sxs-lookup"><span data-stu-id="06bf7-189">Fixes issue [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - EventEmitter listener leak warning.</span></span>

### <span data-ttu-id="06bf7-190"><a name="1.5.1"/>1.5.1</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-190"><a name="1.5.1"/>1.5.1</a></span></span>
* <span data-ttu-id="06bf7-191">Kijavítja a hibát [#92](https://github.com/Azure/azure-documentdb-node/issues/90) -nevezze át a mappát kivonatoló toohash rendszerek kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="06bf7-191">Fixes issue [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - rename folder Hash toohash for case-sensitive systems.</span></span>

### <span data-ttu-id="06bf7-192"><a name="1.5.0"/>1.5.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-192"><a name="1.5.0"/>1.5.0</a></span></span>
* <span data-ttu-id="06bf7-193">Horizontális segítséget nyújt kivonatoló & tartomány partíció feloldókat hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="06bf7-193">Implement sharding support by adding hash & range partition resolvers.</span></span>

### <span data-ttu-id="06bf7-194"><a name="1.4.0"/>1.4.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-194"><a name="1.4.0"/>1.4.0</a></span></span>
* <span data-ttu-id="06bf7-195">Upsert megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="06bf7-195">Implement Upsert.</span></span> <span data-ttu-id="06bf7-196">Új upsertXXX metódusai documentClient.</span><span class="sxs-lookup"><span data-stu-id="06bf7-196">New upsertXXX methods on documentClient.</span></span>

### <span data-ttu-id="06bf7-197"><a name="1.3.0"/>1.3.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-197"><a name="1.3.0"/>1.3.0</a></span></span>
* <span data-ttu-id="06bf7-198">Kihagyott toobring verziószámok Csomagjától az igazítását.</span><span class="sxs-lookup"><span data-stu-id="06bf7-198">Skipped toobring version numbers in alignment with other SDKs.</span></span>

### <span data-ttu-id="06bf7-199"><a name="1.2.2"/>1.2.2</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-199"><a name="1.2.2"/>1.2.2</a></span></span>
* <span data-ttu-id="06bf7-200">Vegyes Q kihasználásának köszönhetően akár még a burkoló toonew tárházba.</span><span class="sxs-lookup"><span data-stu-id="06bf7-200">Split Q promises wrapper toonew repository.</span></span>
* <span data-ttu-id="06bf7-201">Frissítse az npm beállításjegyzék toopackage fájlt.</span><span class="sxs-lookup"><span data-stu-id="06bf7-201">Update toopackage file for npm registry.</span></span>

### <span data-ttu-id="06bf7-202"><a name="1.2.1"/>1.2.1</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-202"><a name="1.2.1"/>1.2.1</a></span></span>
* <span data-ttu-id="06bf7-203">Megvalósítja-alapú útválasztási azonosítója.</span><span class="sxs-lookup"><span data-stu-id="06bf7-203">Implements ID Based Routing.</span></span>
* <span data-ttu-id="06bf7-204">Kijavítja a hibát [#49](https://github.com/Azure/azure-documentdb-node/issues/49) -aktuális tulajdonság metódus current() ütközik.</span><span class="sxs-lookup"><span data-stu-id="06bf7-204">Fixes Issue [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - current property conflicts with method current().</span></span>

### <span data-ttu-id="06bf7-205"><a name="1.2.0"/>1.2.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-205"><a name="1.2.0"/>1.2.0</a></span></span>
* <span data-ttu-id="06bf7-206">A földrajzi index támogatása.</span><span class="sxs-lookup"><span data-stu-id="06bf7-206">Added support for GeoSpatial index.</span></span>
* <span data-ttu-id="06bf7-207">Ellenőrzi az összes erőforrás id tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="06bf7-207">Validates id property for all resources.</span></span> <span data-ttu-id="06bf7-208">Erőforrások azonosító nem tartalmazhat?, /, #, & #47; & #47; karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="06bf7-208">Ids for resources cannot contain ?, /, #, &#47;&#47;, characters or end with a space.</span></span>
* <span data-ttu-id="06bf7-209">Hozzáadja az új fejléc "index átalakítása folyamatban" tooResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="06bf7-209">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <span data-ttu-id="06bf7-210"><a name="1.1.0"/>1.1.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-210"><a name="1.1.0"/>1.1.0</a></span></span>
* <span data-ttu-id="06bf7-211">V2 indexelési házirendet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="06bf7-211">Implements V2 indexing policy.</span></span>

### <span data-ttu-id="06bf7-212"><a name="1.0.3"/>1.0.3</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-212"><a name="1.0.3"/>1.0.3</a></span></span>
* <span data-ttu-id="06bf7-213">A probléma [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - eslint megvalósítása és grunt hello core-konfigurációja és SDK készletéről.</span><span class="sxs-lookup"><span data-stu-id="06bf7-213">Issue [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - Implemented eslint and grunt configurations in hello core and promise SDK.</span></span>

### <span data-ttu-id="06bf7-214"><a name="1.0.2"/>1.0.2</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-214"><a name="1.0.2"/>1.0.2</a></span></span>
* <span data-ttu-id="06bf7-215">A probléma [#45](https://github.com/Azure/azure-documentdb-node/issues/45) -tett burkoló nem tartalmazza a hiba: a fejlécben.</span><span class="sxs-lookup"><span data-stu-id="06bf7-215">Issue [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises wrapper does not include header with error.</span></span>

### <span data-ttu-id="06bf7-216"><a name="1.0.1"/>1.0.1</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-216"><a name="1.0.1"/>1.0.1</a></span></span>
* <span data-ttu-id="06bf7-217">Az ütközések kezelésére képes tooquery megvalósított readConflicts, readConflictAsync és queryConflicts hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="06bf7-217">Implemented ability tooquery for conflicts by adding readConflicts, readConflictAsync, and queryConflicts.</span></span>
* <span data-ttu-id="06bf7-218">Frissített API dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="06bf7-218">Updated API documentation.</span></span>
* <span data-ttu-id="06bf7-219">A probléma [#41](https://github.com/Azure/azure-documentdb-node/issues/41) -client.createDocumentAsync hiba.</span><span class="sxs-lookup"><span data-stu-id="06bf7-219">Issue [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync error.</span></span>

### <span data-ttu-id="06bf7-220"><a name="1.0.0"/>1.0.0</a></span><span class="sxs-lookup"><span data-stu-id="06bf7-220"><a name="1.0.0"/>1.0.0</a></span></span>
* <span data-ttu-id="06bf7-221">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="06bf7-221">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="06bf7-222">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="06bf7-222">Release & Retirement Dates</span></span>
<span data-ttu-id="06bf7-223">A Microsoft biztosít értesítési legalább **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.</span><span class="sxs-lookup"><span data-stu-id="06bf7-223">Microsoft provides notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="06bf7-224">Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így javasolt, hogy Ön mindig frissítési toohello SDK legújabb lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="06bf7-224">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommended that you always upgrade toohello latest SDK version as early as possible.</span></span>

<span data-ttu-id="06bf7-225">TooCosmos DB kivont SDK használatával van kérelmet vissza kell utasítani, hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="06bf7-225">Any request tooCosmos DB using a retired SDK is be rejected by hello service.</span></span>

<br/>

| <span data-ttu-id="06bf7-226">Verzió</span><span class="sxs-lookup"><span data-stu-id="06bf7-226">Version</span></span> | <span data-ttu-id="06bf7-227">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="06bf7-227">Release Date</span></span> | <span data-ttu-id="06bf7-228">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="06bf7-228">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="06bf7-229">1.12.2</span><span class="sxs-lookup"><span data-stu-id="06bf7-229">1.12.2</span></span>](#1.12.2) |<span data-ttu-id="06bf7-230">2017. augusztus 10.</span><span class="sxs-lookup"><span data-stu-id="06bf7-230">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="06bf7-231">1.12.1</span><span class="sxs-lookup"><span data-stu-id="06bf7-231">1.12.1</span></span>](#1.12.1) |<span data-ttu-id="06bf7-232">2017. augusztus 10.</span><span class="sxs-lookup"><span data-stu-id="06bf7-232">August 10, 2017</span></span> |--- |
| [<span data-ttu-id="06bf7-233">1.12.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-233">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="06bf7-234">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="06bf7-234">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="06bf7-235">1.11.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-235">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="06bf7-236">2017. március 16.</span><span class="sxs-lookup"><span data-stu-id="06bf7-236">March 16, 2017</span></span> |--- |
| [<span data-ttu-id="06bf7-237">1.10.2</span><span class="sxs-lookup"><span data-stu-id="06bf7-237">1.10.2</span></span>](#1.10.2) |<span data-ttu-id="06bf7-238">2017. január 27.</span><span class="sxs-lookup"><span data-stu-id="06bf7-238">January 27, 2017</span></span> |--- |
| [<span data-ttu-id="06bf7-239">1.10.1</span><span class="sxs-lookup"><span data-stu-id="06bf7-239">1.10.1</span></span>](#1.10.1) |<span data-ttu-id="06bf7-240">2016. december 22.</span><span class="sxs-lookup"><span data-stu-id="06bf7-240">December 22, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-241">1.10.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-241">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="06bf7-242">2016. október 03.</span><span class="sxs-lookup"><span data-stu-id="06bf7-242">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-243">1.9.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-243">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="06bf7-244">2016. július 07.</span><span class="sxs-lookup"><span data-stu-id="06bf7-244">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-245">1.8.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-245">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="06bf7-246">2016. június 14.</span><span class="sxs-lookup"><span data-stu-id="06bf7-246">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-247">1.7.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-247">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="06bf7-248">2016. április 26.</span><span class="sxs-lookup"><span data-stu-id="06bf7-248">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-249">1.6.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-249">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="06bf7-250">2016. március 29.</span><span class="sxs-lookup"><span data-stu-id="06bf7-250">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-251">1.5.6</span><span class="sxs-lookup"><span data-stu-id="06bf7-251">1.5.6</span></span>](#1.5.6) |<span data-ttu-id="06bf7-252">2016. március 08.</span><span class="sxs-lookup"><span data-stu-id="06bf7-252">March 08, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-253">1.5.5</span><span class="sxs-lookup"><span data-stu-id="06bf7-253">1.5.5</span></span>](#1.5.5) |<span data-ttu-id="06bf7-254">2016. február 02.</span><span class="sxs-lookup"><span data-stu-id="06bf7-254">February 02, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-255">1.5.4</span><span class="sxs-lookup"><span data-stu-id="06bf7-255">1.5.4</span></span>](#1.5.4) |<span data-ttu-id="06bf7-256">2016. február 01.</span><span class="sxs-lookup"><span data-stu-id="06bf7-256">February 01, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-257">1.5.2</span><span class="sxs-lookup"><span data-stu-id="06bf7-257">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="06bf7-258">2016. január 26.</span><span class="sxs-lookup"><span data-stu-id="06bf7-258">January 26, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-259">1.5.2</span><span class="sxs-lookup"><span data-stu-id="06bf7-259">1.5.2</span></span>](#1.5.2) |<span data-ttu-id="06bf7-260">2016. január 22.</span><span class="sxs-lookup"><span data-stu-id="06bf7-260">January 22, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-261">1.5.1</span><span class="sxs-lookup"><span data-stu-id="06bf7-261">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="06bf7-262">2016. január 4.</span><span class="sxs-lookup"><span data-stu-id="06bf7-262">January 4, 2016</span></span> |--- |
| [<span data-ttu-id="06bf7-263">1.5.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-263">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="06bf7-264">2015. december 31.</span><span class="sxs-lookup"><span data-stu-id="06bf7-264">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-265">1.4.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-265">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="06bf7-266">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="06bf7-266">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-267">1.3.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-267">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="06bf7-268">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="06bf7-268">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-269">1.2.2</span><span class="sxs-lookup"><span data-stu-id="06bf7-269">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="06bf7-270">2015. szeptember 10.</span><span class="sxs-lookup"><span data-stu-id="06bf7-270">September 10, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-271">1.2.1</span><span class="sxs-lookup"><span data-stu-id="06bf7-271">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="06bf7-272">2015. augusztus 15.</span><span class="sxs-lookup"><span data-stu-id="06bf7-272">August 15, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-273">1.2.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-273">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="06bf7-274">2015. augusztus 05.</span><span class="sxs-lookup"><span data-stu-id="06bf7-274">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-275">1.1.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-275">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="06bf7-276">2015. július 09.</span><span class="sxs-lookup"><span data-stu-id="06bf7-276">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-277">1.0.3</span><span class="sxs-lookup"><span data-stu-id="06bf7-277">1.0.3</span></span>](#1.0.3) |<span data-ttu-id="06bf7-278">2015. június 04.</span><span class="sxs-lookup"><span data-stu-id="06bf7-278">June 04, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-279">1.0.2</span><span class="sxs-lookup"><span data-stu-id="06bf7-279">1.0.2</span></span>](#1.0.2) |<span data-ttu-id="06bf7-280">2015. május 23.</span><span class="sxs-lookup"><span data-stu-id="06bf7-280">May 23, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-281">1.0.1</span><span class="sxs-lookup"><span data-stu-id="06bf7-281">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="06bf7-282">2015. május 15.</span><span class="sxs-lookup"><span data-stu-id="06bf7-282">May 15, 2015</span></span> |--- |
| [<span data-ttu-id="06bf7-283">1.0.0</span><span class="sxs-lookup"><span data-stu-id="06bf7-283">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="06bf7-284">2015. áprilisi 08</span><span class="sxs-lookup"><span data-stu-id="06bf7-284">April 08, 2015</span></span> |--- |

## <a name="faq"></a><span data-ttu-id="06bf7-285">GYIK</span><span class="sxs-lookup"><span data-stu-id="06bf7-285">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="06bf7-286">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="06bf7-286">See also</span></span>
<span data-ttu-id="06bf7-287">toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="06bf7-287">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

