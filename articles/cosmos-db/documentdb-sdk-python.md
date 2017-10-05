---
title: "Azure Cosmos DB Python API, az SDK és az erőforrások |} Microsoft Docs"
description: "Tudnivalók a Python API és SDK, beleértve a kiadási dátum, használatból való kivonást dátumok és az Azure Cosmos DB Python SDK verziói között végrehajtott módosításokat."
services: cosmos-db
documentationcenter: python
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 3ac344a9-b2fa-4a3f-a4cc-02d287e05469
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/24/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70d2550f713ff0e9daed235eb8053589b8682633
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a><span data-ttu-id="c1571-103">Az Azure Cosmos DB Python SDK: A kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="c1571-103">Azure Cosmos DB Python SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c1571-104">.NET</span><span class="sxs-lookup"><span data-stu-id="c1571-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="c1571-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="c1571-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="c1571-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1571-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="c1571-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="c1571-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="c1571-108">Java</span><span class="sxs-lookup"><span data-stu-id="c1571-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="c1571-109">Python</span><span class="sxs-lookup"><span data-stu-id="c1571-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="c1571-110">REST</span><span class="sxs-lookup"><span data-stu-id="c1571-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="c1571-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="c1571-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="c1571-112">SQL</span><span class="sxs-lookup"><span data-stu-id="c1571-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="c1571-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="c1571-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="c1571-114">PyPI</span><span class="sxs-lookup"><span data-stu-id="c1571-114">PyPI</span></span>](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td><span data-ttu-id="c1571-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="c1571-115">**API documentation**</span></span></td><td>[<span data-ttu-id="c1571-116">Python API referenciadokumentációt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="c1571-116">Python API reference documentation</span></span>](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td><span data-ttu-id="c1571-117">**SDK telepítési utasításokat**</span><span class="sxs-lookup"><span data-stu-id="c1571-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="c1571-118">Python SDK telepítési utasításokat</span><span class="sxs-lookup"><span data-stu-id="c1571-118">Python SDK installation instructions</span></span>](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td><span data-ttu-id="c1571-119">**Hozzájárul az SDK**</span><span class="sxs-lookup"><span data-stu-id="c1571-119">**Contribute to SDK**</span></span></td><td>[<span data-ttu-id="c1571-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="c1571-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td><span data-ttu-id="c1571-121">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="c1571-121">**Get started**</span></span></td><td>[<span data-ttu-id="c1571-122">Ismerkedés a Python SDK-val</span><span class="sxs-lookup"><span data-stu-id="c1571-122">Get started with the Python SDK</span></span>](documentdb-python-application.md)</td></tr>

<tr><td><span data-ttu-id="c1571-123">**Aktuális támogatott platform**</span><span class="sxs-lookup"><span data-stu-id="c1571-123">**Current supported platform**</span></span></td><td><span data-ttu-id="c1571-124">[Python 2.7](https://www.python.org/downloads/) és [Python 3.5](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="c1571-124">[Python 2.7](https://www.python.org/downloads/) and [Python 3.5](https://www.python.org/downloads/)</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="c1571-125">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c1571-125">Release notes</span></span>
### <a name="a-name220220"></a><span data-ttu-id="c1571-126"><a name="2.2.0"/>2.2.0</span><span class="sxs-lookup"><span data-stu-id="c1571-126"><a name="2.2.0"/>2.2.0</span></span>
* <span data-ttu-id="c1571-127">Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="c1571-127">Added support for a new consistency level called ConsistentPrefix.</span></span>


### <a name="a-name210210"></a><span data-ttu-id="c1571-128"><a name="2.1.0"/>2.1.0</span><span class="sxs-lookup"><span data-stu-id="c1571-128"><a name="2.1.0"/>2.1.0</span></span>
* <span data-ttu-id="c1571-129">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-129">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="c1571-130">A beállítás a letiltás SSL ellenőrzési Cosmos DB emulatorban futtatásakor hozzá.</span><span class="sxs-lookup"><span data-stu-id="c1571-130">Added an option for disabling SSL verification when running against Cosmos DB Emulator.</span></span>
* <span data-ttu-id="c1571-131">Függő kérések modul kell lennie, pontosan 2.10.0 korlátozásának eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="c1571-131">Removed the restriction of dependent requests module to be exactly 2.10.0.</span></span>
* <span data-ttu-id="c1571-132">Minimális átviteli sebességet 2500 RU/mp 10,100 RU/mp a particionált gyűjtemények szintűre csökkent.</span><span class="sxs-lookup"><span data-stu-id="c1571-132">Lowered minimum throughput on partitioned collections from 10,100 RU/s to 2500 RU/s.</span></span>
* <span data-ttu-id="c1571-133">Támogatja a tárolt eljárás végrehajtása során parancsfájl naplózásának engedélyezéséről.</span><span class="sxs-lookup"><span data-stu-id="c1571-133">Added support for enabling script logging during stored procedure execution.</span></span>
* <span data-ttu-id="c1571-134">REST API-verziót a "2017 bumped-01-19' ebben a kiadásban.</span><span class="sxs-lookup"><span data-stu-id="c1571-134">REST API version bumped to '2017-01-19' with this release.</span></span>

### <a name="a-name201201"></a><span data-ttu-id="c1571-135"><a name="2.0.1"/>2.0.1</span><span class="sxs-lookup"><span data-stu-id="c1571-135"><a name="2.0.1"/>2.0.1</span></span>
* <span data-ttu-id="c1571-136">A dokumentációs célú Szerkesztői módosítja.</span><span class="sxs-lookup"><span data-stu-id="c1571-136">Made editorial changes to documentation comments.</span></span>

### <a name="a-name200200"></a><span data-ttu-id="c1571-137"><a name="2.0.0"/>2.0.0</span><span class="sxs-lookup"><span data-stu-id="c1571-137"><a name="2.0.0"/>2.0.0</span></span>
* <span data-ttu-id="c1571-138">Python 3.5 támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-138">Added support for Python 3.5.</span></span>
* <span data-ttu-id="c1571-139">Támogatja a kapcsolatkészletezést kérelmek modul használatával.</span><span class="sxs-lookup"><span data-stu-id="c1571-139">Added support for connection pooling using a requests module.</span></span>
* <span data-ttu-id="c1571-140">A munkamenet-konzisztencia támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-140">Added support for session consistency.</span></span>
* <span data-ttu-id="c1571-141">FELSŐ/ORDERBY lekérdezések particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-141">Added support for TOP/ORDERBY queries for partitioned collections.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="c1571-142"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="c1571-142"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="c1571-143">Szabályozottan halmozott kérelmek hozzáadott újrapróbálkozási házirend támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-143">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="c1571-144">(A szabályozottan halmozott kérelmek egy kérelem aránya túl nagy kivétel, hibakód 429 kapnak.) Alapértelmezés szerint Azure Cosmos DB újrapróbálja kilenc alkalommal az egyes kérelmek 429. Hibakód: a rendszer észlelt, amikor az retryAfter idő a válaszfejlécet érvényesítenie.</span><span class="sxs-lookup"><span data-stu-id="c1571-144">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring the retryAfter time in the response header.</span></span> <span data-ttu-id="c1571-145">A rögzített újrapróbálkozási időköz most már beállítható a RetryOptions tulajdonság részeként a ConnectionPolicy objektum Ha azt szeretné, figyelmen kívül hagyja a próbálkozások közötti kiszolgáló által visszaadott retryAfter idő.</span><span class="sxs-lookup"><span data-stu-id="c1571-145">A fixed retry interval time can now be set as part of the RetryOptions property on the ConnectionPolicy object if you want to ignore the retryAfter time returned by server between the retries.</span></span> <span data-ttu-id="c1571-146">Azure Cosmos-adatbázis most megvárja, legfeljebb 30 másodpercig (függetlenül újrapróbálkozások száma) leszabályozta, és visszaadja a választ 429. Hibakód: minden egyes kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="c1571-146">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns the response with error code 429.</span></span> <span data-ttu-id="c1571-147">Most is lehet a RetryOptions tulajdonságban ConnectionPolicy objektumon.</span><span class="sxs-lookup"><span data-stu-id="c1571-147">This time can also be overriden in the RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="c1571-148">Cosmos DB most adja vissza x-ms-szabályozási--újrapróbálkozások és x-ms-throttle-retry-wait-time-ms a válaszfejlécek minden kérelemben a késleltetési jelöléséhez újra számát és a próbálkozások közötti várta a kérelem cummulative idő szerint.</span><span class="sxs-lookup"><span data-stu-id="c1571-148">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as the response headers in every request to denote the throttle retry count and the cummulative time the request waited between the retries.</span></span>
* <span data-ttu-id="c1571-149">Eltávolította a RetryPolicy osztály és a megfelelő tulajdonságával (retry_policy), a document_client osztály kitett, és ehelyett bevezetett teszi ki a ConnectionPolicy osztály, amely felülbírálhatja az alapértelmezett beállítások némelyike használható RetryOptions tulajdonsága RetryOptions osztály.</span><span class="sxs-lookup"><span data-stu-id="c1571-149">Removed the RetryPolicy class and the corresponding property (retry_policy) exposed on the document_client class and instead introduced a RetryOptions class exposing the RetryOptions property on ConnectionPolicy class that can be used to override some of the default retry options.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="c1571-150"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="c1571-150"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="c1571-151">Több területi adatbázis fiókok támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-151">Added the support for multi-region database accounts.</span></span>

### <a name="a-name170170"></a><span data-ttu-id="c1571-152"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="c1571-152"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="c1571-153">Idő a Live(TTL) szolgáltatás dokumentumok támogatása.</span><span class="sxs-lookup"><span data-stu-id="c1571-153">Added the support for Time To Live(TTL) feature for documents.</span></span>

### <a name="a-name161161"></a><span data-ttu-id="c1571-154"><a name="1.6.1"/>1.6.1</span><span class="sxs-lookup"><span data-stu-id="c1571-154"><a name="1.6.1"/>1.6.1</span></span>
* <span data-ttu-id="c1571-155">Kiszolgáló oldalán particionálás a különleges karakterek engedélyezéséhez partitionkey elérési kapcsolatos hibajavítások.</span><span class="sxs-lookup"><span data-stu-id="c1571-155">Bug fixes related to server side partitioning to allow special characters in partitionkey path.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="c1571-156"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="c1571-156"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="c1571-157">Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="c1571-157">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span> 

### <a name="a-name150150"></a><span data-ttu-id="c1571-158"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="c1571-158"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="c1571-159">Kivonatoló & tartomány hozzáadása partícióazonosító feloldókat, elősegítve ezzel a horizontális alkalmazások között több partíciót.</span><span class="sxs-lookup"><span data-stu-id="c1571-159">Add Hash & Range partition resolvers to assist with sharding applications across multiple partitions.</span></span>

### <a name="a-name142142"></a><span data-ttu-id="c1571-160"><a name="1.4.2"/>1.4.2</span><span class="sxs-lookup"><span data-stu-id="c1571-160"><a name="1.4.2"/>1.4.2</span></span>
* <span data-ttu-id="c1571-161">Upsert megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="c1571-161">Implement Upsert.</span></span> <span data-ttu-id="c1571-162">Új UpsertXXX módszerek Upsert szolgáltatás támogatása érdekében adott hozzá.</span><span class="sxs-lookup"><span data-stu-id="c1571-162">New UpsertXXX methods added to support Upsert feature.</span></span>
* <span data-ttu-id="c1571-163">Azonosító-alapú útválasztás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="c1571-163">Implement ID Based Routing.</span></span> <span data-ttu-id="c1571-164">Nincs nyilvános API-módosítás, belső összes módosítását.</span><span class="sxs-lookup"><span data-stu-id="c1571-164">No public API changes, all changes internal.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="c1571-165"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="c1571-165"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="c1571-166">Támogatja a földrajzi index.</span><span class="sxs-lookup"><span data-stu-id="c1571-166">Supports GeoSpatial index.</span></span>
* <span data-ttu-id="c1571-167">Ellenőrzi az összes erőforrás id tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c1571-167">Validates id property for all resources.</span></span> <span data-ttu-id="c1571-168">Erőforrások azonosító nem tartalmazhat?, /, #, \, karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="c1571-168">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="c1571-169">Új fejléc "index átalakítása folyamatban" hozzáadása ResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="c1571-169">Adds new header "index transformation progress" to ResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="c1571-170"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="c1571-170"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="c1571-171">V2 indexelési házirendet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1571-171">Implements V2 indexing policy.</span></span>

### <a name="a-name101101"></a><span data-ttu-id="c1571-172"><a name="1.0.1"/>1.0.1</span><span class="sxs-lookup"><span data-stu-id="c1571-172"><a name="1.0.1"/>1.0.1</span></span>
* <span data-ttu-id="c1571-173">Proxy kapcsolatot támogat.</span><span class="sxs-lookup"><span data-stu-id="c1571-173">Supports proxy connection.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="c1571-174"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="c1571-174"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="c1571-175">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="c1571-175">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="c1571-176">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="c1571-176">Release & retirement dates</span></span>
<span data-ttu-id="c1571-177">Microsoft legalább értesítést küldenek **12 hónapon keresztül** SDK eltávolítása érdekében vagy újabb támogatott verzióra való áttérés előtt.</span><span class="sxs-lookup"><span data-stu-id="c1571-177">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="c1571-178">Új szolgáltatásait és funkcióit és optimalizálás csak hozzá az aktuális SDK, így javasoljuk, hogy mindig a legújabb SDK verzióra frissít legkorábban lehető.</span><span class="sxs-lookup"><span data-stu-id="c1571-178">New features and functionality and optimizations are only added to the current SDK, as such it is  recommend that you always upgrade to the latest SDK version as early as possible.</span></span> 

<span data-ttu-id="c1571-179">A Cosmos DB kivont SDK használatával fog kell elutasította a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c1571-179">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

> [!WARNING]
> <span data-ttu-id="c1571-180">Az Azure DocumentDB Python SDK-es vagy korábbi összes verziója **1.0.0** a rendszerből **2016. február 29-én**.</span><span class="sxs-lookup"><span data-stu-id="c1571-180">All versions of the Azure DocumentDB SDK for Python prior to version **1.0.0** will be retired on **February 29, 2016**.</span></span> 
> 
> 

<br/>

| <span data-ttu-id="c1571-181">Verzió</span><span class="sxs-lookup"><span data-stu-id="c1571-181">Version</span></span> | <span data-ttu-id="c1571-182">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="c1571-182">Release Date</span></span> | <span data-ttu-id="c1571-183">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="c1571-183">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="c1571-184">2.2.0</span><span class="sxs-lookup"><span data-stu-id="c1571-184">2.2.0</span></span>](#2.2.0) |<span data-ttu-id="c1571-185">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="c1571-185">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="c1571-186">2.1.0</span><span class="sxs-lookup"><span data-stu-id="c1571-186">2.1.0</span></span>](#2.1.0) |<span data-ttu-id="c1571-187">2017. Előfordulhat, hogy 01.</span><span class="sxs-lookup"><span data-stu-id="c1571-187">May 01, 2017</span></span> |--- |
| [<span data-ttu-id="c1571-188">2.0.1</span><span class="sxs-lookup"><span data-stu-id="c1571-188">2.0.1</span></span>](#2.0.1) |<span data-ttu-id="c1571-189">2016. október 30.</span><span class="sxs-lookup"><span data-stu-id="c1571-189">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-190">2.0.0</span><span class="sxs-lookup"><span data-stu-id="c1571-190">2.0.0</span></span>](#2.0.0) |<span data-ttu-id="c1571-191">2016. Szeptembertől 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-191">September 29, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-192">1.9.0</span><span class="sxs-lookup"><span data-stu-id="c1571-192">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="c1571-193">2016. július 07.</span><span class="sxs-lookup"><span data-stu-id="c1571-193">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-194">1.8.0</span><span class="sxs-lookup"><span data-stu-id="c1571-194">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="c1571-195">2016. június 14.</span><span class="sxs-lookup"><span data-stu-id="c1571-195">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-196">1.7.0</span><span class="sxs-lookup"><span data-stu-id="c1571-196">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="c1571-197">2016. április 26.</span><span class="sxs-lookup"><span data-stu-id="c1571-197">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-198">1.6.1</span><span class="sxs-lookup"><span data-stu-id="c1571-198">1.6.1</span></span>](#1.6.1) |<span data-ttu-id="c1571-199">2016. április 08.</span><span class="sxs-lookup"><span data-stu-id="c1571-199">April 08, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-200">1.6.0</span><span class="sxs-lookup"><span data-stu-id="c1571-200">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="c1571-201">2016. március 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-201">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-202">1.5.0</span><span class="sxs-lookup"><span data-stu-id="c1571-202">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="c1571-203">2016. január 03.</span><span class="sxs-lookup"><span data-stu-id="c1571-203">January 03, 2016</span></span> |--- |
| [<span data-ttu-id="c1571-204">1.4.2</span><span class="sxs-lookup"><span data-stu-id="c1571-204">1.4.2</span></span>](#1.4.2) |<span data-ttu-id="c1571-205">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="c1571-205">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="c1571-206">1.4.1</span><span class="sxs-lookup"><span data-stu-id="c1571-206">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="c1571-207">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="c1571-207">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="c1571-208">1.2.0</span><span class="sxs-lookup"><span data-stu-id="c1571-208">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="c1571-209">2015. augusztus 06.</span><span class="sxs-lookup"><span data-stu-id="c1571-209">August 06, 2015</span></span> |--- |
| [<span data-ttu-id="c1571-210">1.1.0</span><span class="sxs-lookup"><span data-stu-id="c1571-210">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="c1571-211">2015. július 09.</span><span class="sxs-lookup"><span data-stu-id="c1571-211">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="c1571-212">1.0.1</span><span class="sxs-lookup"><span data-stu-id="c1571-212">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="c1571-213">2015. május 25.</span><span class="sxs-lookup"><span data-stu-id="c1571-213">May 25, 2015</span></span> |--- |
| [<span data-ttu-id="c1571-214">1.0.0</span><span class="sxs-lookup"><span data-stu-id="c1571-214">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="c1571-215">2015. április 07.</span><span class="sxs-lookup"><span data-stu-id="c1571-215">April 07, 2015</span></span> |--- |
| <span data-ttu-id="c1571-216">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="c1571-216">0.9.4-prelease</span></span> |<span data-ttu-id="c1571-217">2015. január 14.</span><span class="sxs-lookup"><span data-stu-id="c1571-217">January 14, 2015</span></span> |<span data-ttu-id="c1571-218">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-218">February 29, 2016</span></span> |
| <span data-ttu-id="c1571-219">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="c1571-219">0.9.3-prelease</span></span> |<span data-ttu-id="c1571-220">2014. decemberi 09</span><span class="sxs-lookup"><span data-stu-id="c1571-220">December 09, 2014</span></span> |<span data-ttu-id="c1571-221">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-221">February 29, 2016</span></span> |
| <span data-ttu-id="c1571-222">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="c1571-222">0.9.2-prelease</span></span> |<span data-ttu-id="c1571-223">2014. november 25.</span><span class="sxs-lookup"><span data-stu-id="c1571-223">November 25, 2014</span></span> |<span data-ttu-id="c1571-224">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-224">February 29, 2016</span></span> |
| <span data-ttu-id="c1571-225">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="c1571-225">0.9.1-prelease</span></span> |<span data-ttu-id="c1571-226">2014. szeptember 23.</span><span class="sxs-lookup"><span data-stu-id="c1571-226">September 23, 2014</span></span> |<span data-ttu-id="c1571-227">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-227">February 29, 2016</span></span> |
| <span data-ttu-id="c1571-228">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="c1571-228">0.9.0-prelease</span></span> |<span data-ttu-id="c1571-229">2014. augusztus 21.</span><span class="sxs-lookup"><span data-stu-id="c1571-229">August 21, 2014</span></span> |<span data-ttu-id="c1571-230">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="c1571-230">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="c1571-231">GYIK</span><span class="sxs-lookup"><span data-stu-id="c1571-231">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="c1571-232">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c1571-232">See also</span></span>
<span data-ttu-id="c1571-233">A Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="c1571-233">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

