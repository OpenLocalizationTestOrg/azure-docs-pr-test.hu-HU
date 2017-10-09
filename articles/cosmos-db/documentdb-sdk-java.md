---
title: "Az Azure Cosmos DB: A DocumentDB Java API, SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello Java API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB DocumentDB Java SDK verziói között."
services: cosmos-db
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 07/11/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ef43ebeb7ae1bfc55512c4a7489c1b7930122d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a><span data-ttu-id="5117c-103">Azure Cosmos DB: A DocumentDB Java SDK kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="5117c-103">Azure Cosmos DB: DocumentDB Java SDK release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5117c-104">.NET</span><span class="sxs-lookup"><span data-stu-id="5117c-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="5117c-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="5117c-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="5117c-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="5117c-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="5117c-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="5117c-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="5117c-108">Java</span><span class="sxs-lookup"><span data-stu-id="5117c-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="5117c-109">Python</span><span class="sxs-lookup"><span data-stu-id="5117c-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="5117c-110">REST</span><span class="sxs-lookup"><span data-stu-id="5117c-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="5117c-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="5117c-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="5117c-112">SQL</span><span class="sxs-lookup"><span data-stu-id="5117c-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="5117c-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="5117c-113">**SDK Download**</span></span></td><td>[<span data-ttu-id="5117c-114">Maven 3</span><span class="sxs-lookup"><span data-stu-id="5117c-114">Maven</span></span>](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td><span data-ttu-id="5117c-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="5117c-115">**API documentation**</span></span></td><td>[<span data-ttu-id="5117c-116">Java API-referenciadokumentáció</span><span class="sxs-lookup"><span data-stu-id="5117c-116">Java API reference documentation</span></span>](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td><span data-ttu-id="5117c-117">**Közreműködési lehetőségek tooSDK**</span><span class="sxs-lookup"><span data-stu-id="5117c-117">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="5117c-118">GitHub</span><span class="sxs-lookup"><span data-stu-id="5117c-118">GitHub</span></span>](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td><span data-ttu-id="5117c-119">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="5117c-119">**Get started**</span></span></td><td>[<span data-ttu-id="5117c-120">Ismerkedés a hello Java SDK</span><span class="sxs-lookup"><span data-stu-id="5117c-120">Get started with hello Java SDK</span></span>](documentdb-java-get-started.md)</td></tr>

<tr><td><span data-ttu-id="5117c-121">**Webes alkalmazás oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="5117c-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="5117c-122">Webalkalmazás fejlesztése a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5117c-122">Web application development with Azure Cosmos DB</span></span>](documentdb-java-application.md)</td></tr>

<tr><td><span data-ttu-id="5117c-123">**Aktuális támogatott futásidejű**</span><span class="sxs-lookup"><span data-stu-id="5117c-123">**Current supported runtime**</span></span></td><td>[<span data-ttu-id="5117c-124">JDK 7</span><span class="sxs-lookup"><span data-stu-id="5117c-124">JDK 7</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="5117c-125">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5117c-125">Release Notes</span></span>

### <a name="a-name11201120"></a><span data-ttu-id="5117c-126"><a name="1.12.0"/>1.12.0</span><span class="sxs-lookup"><span data-stu-id="5117c-126"><a name="1.12.0"/>1.12.0</span></span>
* <span data-ttu-id="5117c-127">Felosztja a kritikus hibajavítások toorequest során partíció feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="5117c-127">Critical bug fixes toorequest processing during partition splits.</span></span>
* <span data-ttu-id="5117c-128">Megtörtént egy probléma javítása hello erős és BoundedStaleness konzisztenciaszintek.</span><span class="sxs-lookup"><span data-stu-id="5117c-128">Fixed an issue with hello Strong and BoundedStaleness consistency levels.</span></span>

### <a name="a-name11101110"></a><span data-ttu-id="5117c-129"><a name="1.11.0"/>1.11.0</span><span class="sxs-lookup"><span data-stu-id="5117c-129"><a name="1.11.0"/>1.11.0</span></span>
* <span data-ttu-id="5117c-130">Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="5117c-130">Added support for a new consistency level called ConsistentPrefix.</span></span>
* <span data-ttu-id="5117c-131">Rögzített munkamenetmód gyűjtemény olvasási hiba.</span><span class="sxs-lookup"><span data-stu-id="5117c-131">Fixed a bug in reading collection in session mode.</span></span>

### <a name="a-name11001100"></a><span data-ttu-id="5117c-132"><a name="1.10.0"/>1.10.0</span><span class="sxs-lookup"><span data-stu-id="5117c-132"><a name="1.10.0"/>1.10.0</span></span>
* <span data-ttu-id="5117c-133">Particionált gyűjtemény, mint az engedélyezett támogatása 2500 RU/mp, alacsony, a méretezés lépésekben 100 RU/mp.</span><span class="sxs-lookup"><span data-stu-id="5117c-133">Enabled support for partitioned collection with as low as 2,500 RU/sec and scale in increments of 100 RU/sec.</span></span>
* <span data-ttu-id="5117c-134">A hiba kijavítva a hello natív szerelvény, ami azt eredményezheti NullRef kivétel néhány lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="5117c-134">Fixed a bug in hello native assembly which can cause NullRef exception in some queries.</span></span>

### <a name="a-name196196"></a><span data-ttu-id="5117c-135"><a name="1.9.6"/>1.9.6</span><span class="sxs-lookup"><span data-stu-id="5117c-135"><a name="1.9.6"/>1.9.6</span></span>
* <span data-ttu-id="5117c-136">Programhiba rögzített hello lekérdezési motor konfigurációját, amelyek kivételek lekérdezések okozhatnak átjáró módban.</span><span class="sxs-lookup"><span data-stu-id="5117c-136">Fixed a bug in hello query engine configuration that may cause exceptions for queries in Gateway mode.</span></span>
* <span data-ttu-id="5117c-137">Rögzített néhány hibák hello munkamenet tárolóban, amelyek közvetlenül a webhelycsoport létrehozása után a kéréseket "Tulajdonosa erőforrás nem található" kivétel okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="5117c-137">Fixed a few bugs in hello session container that may cause an "Owner resource not found" exception for requests immediately after collection creation.</span></span>

### <a name="a-name195195"></a><span data-ttu-id="5117c-138"><a name="1.9.5"/>1.9.5</span><span class="sxs-lookup"><span data-stu-id="5117c-138"><a name="1.9.5"/>1.9.5</span></span>
* <span data-ttu-id="5117c-139">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-139">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="5117c-140">Lásd: [összesítési támogatási](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="5117c-140">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="5117c-141">Adatcsatorna módosítás támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-141">Added support for change feed.</span></span>
* <span data-ttu-id="5117c-142">Gyűjtemény kvótaadatok keresztül RequestOptions.setPopulateQuotaInfo támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-142">Added support for collection quota information through RequestOptions.setPopulateQuotaInfo.</span></span>
* <span data-ttu-id="5117c-143">Tárolt eljárás parancsfájl naplózást RequestOptions.setScriptLoggingEnabled támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-143">Added support for stored procedure script logging through RequestOptions.setScriptLoggingEnabled.</span></span>
* <span data-ttu-id="5117c-144">Rögzített programhiba, ahol DirectHttps módban lekérdezés lefagyhat, amikor adatátviteli hiba észlelése.</span><span class="sxs-lookup"><span data-stu-id="5117c-144">Fixed a bug where query in DirectHttps mode may hang when encountering throttle failures.</span></span>
* <span data-ttu-id="5117c-145">Programhiba rögzített munkamenet-konzisztencia módban.</span><span class="sxs-lookup"><span data-stu-id="5117c-145">Fixed a bug in session consistency mode.</span></span>
* <span data-ttu-id="5117c-146">Egy hiba miatt előfordulhat, hogy NullReferenceException a HttpContext Ha nagy a kérések aránya rögzített.</span><span class="sxs-lookup"><span data-stu-id="5117c-146">Fixed a bug which may cause NullReferenceException in HttpContext when request rate is high.</span></span>
* <span data-ttu-id="5117c-147">Javítja a teljesítményt a DirectHttps mód.</span><span class="sxs-lookup"><span data-stu-id="5117c-147">Improved performance of DirectHttps mode.</span></span>

### <a name="a-name194194"></a><span data-ttu-id="5117c-148"><a name="1.9.4"/>1.9.4</span><span class="sxs-lookup"><span data-stu-id="5117c-148"><a name="1.9.4"/>1.9.4</span></span>
* <span data-ttu-id="5117c-149">A hozzáadott egyszerű példány-alapú proxyk ügyféltámogatás ConnectionPolicy.setProxy() API-t.</span><span class="sxs-lookup"><span data-stu-id="5117c-149">Added simple client instance-based proxy support with ConnectionPolicy.setProxy() API.</span></span>
* <span data-ttu-id="5117c-150">A hozzáadott DocumentClient.close() API tooproperly leállítási DocumentClient-példány.</span><span class="sxs-lookup"><span data-stu-id="5117c-150">Added DocumentClient.close() API tooproperly shutdown DocumentClient instance.</span></span>
* <span data-ttu-id="5117c-151">Továbbfejlesztett lekérdezési teljesítmény szerint hello lekérdezésterv származó hello natív szerelvény helyett átjáró hello közvetlen kapcsolatot módban.</span><span class="sxs-lookup"><span data-stu-id="5117c-151">Improved query performance in direct connectivity mode by deriving hello query plan from hello native assembly instead of hello Gateway.</span></span>
* <span data-ttu-id="5117c-152">Állítsa be a FAIL_ON_UNKNOWN_PROPERTIES = false, így a felhasználók a saját pojo-vá JsonIgnoreProperties toodefine nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5117c-152">Set FAIL_ON_UNKNOWN_PROPERTIES = false so users don't need toodefine JsonIgnoreProperties in their POJO.</span></span>
* <span data-ttu-id="5117c-153">Átkerült naplózási toouse SLF4J.</span><span class="sxs-lookup"><span data-stu-id="5117c-153">Refactored logging toouse SLF4J.</span></span>
* <span data-ttu-id="5117c-154">Rögzített, néhány más hibák konzisztencia-olvasó.</span><span class="sxs-lookup"><span data-stu-id="5117c-154">Fixed a few other bugs in consistency reader.</span></span>

### <a name="a-name193193"></a><span data-ttu-id="5117c-155"><a name="1.9.3"/>1.9.3</span><span class="sxs-lookup"><span data-stu-id="5117c-155"><a name="1.9.3"/>1.9.3</span></span>
* <span data-ttu-id="5117c-156">Programhiba rögzített hello kapcsolat felügyeleti tooprevent kapcsolat kiszivárgásának közvetlen kapcsolatot módban.</span><span class="sxs-lookup"><span data-stu-id="5117c-156">Fixed a bug in hello connection management tooprevent connection leaks in direct connectivity mode.</span></span>
* <span data-ttu-id="5117c-157">Rögzített programhiba hello legfelső szintű lekérdezésben, ahol NullReferenece kivétel generálhat.</span><span class="sxs-lookup"><span data-stu-id="5117c-157">Fixed a bug in hello TOP query where it may throw NullReferenece exception.</span></span>
* <span data-ttu-id="5117c-158">Javítja a teljesítményt, csökkenti a hálózati hívás hello belső gyorsítótárak esetében hello száma.</span><span class="sxs-lookup"><span data-stu-id="5117c-158">Improved performance by reducing hello number of network call for hello internal caches.</span></span>
* <span data-ttu-id="5117c-159">A hozzáadott állapotkód, tevékenységazonosító és a kérelem URI-azonosítója a DocumentClientException hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="5117c-159">Added status code, ActivityID and Request URI in DocumentClientException for better troubleshooting.</span></span>

### <a name="a-name192192"></a><span data-ttu-id="5117c-160"><a name="1.9.2"/>1.9.2</span><span class="sxs-lookup"><span data-stu-id="5117c-160"><a name="1.9.2"/>1.9.2</span></span>
* <span data-ttu-id="5117c-161">Megtörtént egy probléma javítása hello kapcsolat felügyeleti stabilitását.</span><span class="sxs-lookup"><span data-stu-id="5117c-161">Fixed an issue in hello connection management for stability.</span></span>

### <a name="a-name191191"></a><span data-ttu-id="5117c-162"><a name="1.9.1"/>1.9.1</span><span class="sxs-lookup"><span data-stu-id="5117c-162"><a name="1.9.1"/>1.9.1</span></span>
* <span data-ttu-id="5117c-163">BoundedStaleness konzisztenciaszint támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-163">Added support for BoundedStaleness consistency level.</span></span>
* <span data-ttu-id="5117c-164">Közvetlen kapcsolat CRUD műveleteihez particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-164">Added support for direct connectivity for CRUD operations for partitioned collections.</span></span>
* <span data-ttu-id="5117c-165">SQL adatbázis lekérdezése hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="5117c-165">Fixed a bug in querying a database with SQL.</span></span>
* <span data-ttu-id="5117c-166">Programhiba rögzített hello gyorsítótárában, ahol munkamenet-azonosító is lehet helytelenül van beállítva.</span><span class="sxs-lookup"><span data-stu-id="5117c-166">Fixed a bug in hello session cache where session token may be set incorrectly.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="5117c-167"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="5117c-167"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="5117c-168">Támogatja a párhuzamos lekérdezések partíció közötti.</span><span class="sxs-lookup"><span data-stu-id="5117c-168">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="5117c-169">FELSŐ/ORDER BY lekérdezések a particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-169">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>
* <span data-ttu-id="5117c-170">Az erős konzisztencia támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-170">Added support for strong consistency.</span></span>
* <span data-ttu-id="5117c-171">Alapú kérések közvetlen kapcsolat használatakor támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-171">Added support for name based requests when using direct connectivity.</span></span>
* <span data-ttu-id="5117c-172">Rögzített toomake ActivityId maradnak konzisztens összes kérelem újrapróbálkozások között.</span><span class="sxs-lookup"><span data-stu-id="5117c-172">Fixed toomake ActivityId stay consistent across all request retries.</span></span>
* <span data-ttu-id="5117c-173">Programhiba rögzített során egy gyűjteménybe hello és újbóli létrehozása a munkamenet-gyorsítótár toohello kapcsolatos ugyanazzal a névvel.</span><span class="sxs-lookup"><span data-stu-id="5117c-173">Fixed a bug related toohello session cache when recreating a collection with hello same name.</span></span>
* <span data-ttu-id="5117c-174">A hozzáadott sokszög és LineString adattípusok indexelő házirend geokerítések térbeli lekérdezéseket a gyűjtemény meg.</span><span class="sxs-lookup"><span data-stu-id="5117c-174">Added Polygon and LineString DataTypes while specifying collection indexing policy for geo-fencing spatial queries.</span></span>
* <span data-ttu-id="5117c-175">Java Doc Java 1.8 rögzített problémákat.</span><span class="sxs-lookup"><span data-stu-id="5117c-175">Fixed issues with Java Doc for Java 1.8.</span></span>

### <a name="a-name181181"></a><span data-ttu-id="5117c-176"><a name="1.8.1"/>1.8.1</span><span class="sxs-lookup"><span data-stu-id="5117c-176"><a name="1.8.1"/>1.8.1</span></span>
* <span data-ttu-id="5117c-177">Programhiba rögzített PartitionKeyDefinitionMap toocache az egypartíciós gyűjtemények és extra nem később fetch partíció kulcs kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="5117c-177">Fixed a bug in PartitionKeyDefinitionMap toocache single partition collections and not make extra fetch partition key requests.</span></span>
* <span data-ttu-id="5117c-178">Rögzített egy hiba toonot újra, amikor egy helytelen partíciókulcs-értékkel van megadva.</span><span class="sxs-lookup"><span data-stu-id="5117c-178">Fixed a bug toonot retry when an incorrect partition key value is provided.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="5117c-179"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="5117c-179"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="5117c-180">Több területi adatbázis fiókok hozzáadott hello támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-180">Added hello support for multi-region database accounts.</span></span>
* <span data-ttu-id="5117c-181">A beállítások toocustomize hello maximális szabályozottan halmozott kérelmek automatikus újrapróbálkozási támogatása ismétlési kísérletek száma és max várakozási idő.</span><span class="sxs-lookup"><span data-stu-id="5117c-181">Added support for automatic retry on throttled requests with options toocustomize hello max retry attempts and max retry wait time.</span></span>  <span data-ttu-id="5117c-182">Lásd: RetryOptions és ConnectionPolicy.getRetryOptions().</span><span class="sxs-lookup"><span data-stu-id="5117c-182">See RetryOptions and ConnectionPolicy.getRetryOptions().</span></span>
* <span data-ttu-id="5117c-183">Elavult IPartitionResolver egyéni particionálási kód alapján.</span><span class="sxs-lookup"><span data-stu-id="5117c-183">Deprecated IPartitionResolver based custom partitioning code.</span></span> <span data-ttu-id="5117c-184">A particionált gyűjtemények használata magasabb tárolási és átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="5117c-184">Please use partitioned collections for higher storage and throughput.</span></span>

### <a name="a-name171171"></a><span data-ttu-id="5117c-185"><a name="1.7.1"/>1.7.1</span><span class="sxs-lookup"><span data-stu-id="5117c-185"><a name="1.7.1"/>1.7.1</span></span>
* <span data-ttu-id="5117c-186">A hozzáadott újrapróbálkozási házirend támogatása sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5117c-186">Added retry policy support for throttling.</span></span>  

### <a name="a-name170170"></a><span data-ttu-id="5117c-187"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="5117c-187"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="5117c-188">Időigényessé toolive (TTL) dokumentumok támogatása.</span><span class="sxs-lookup"><span data-stu-id="5117c-188">Added time toolive (TTL) support for documents.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="5117c-189"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="5117c-189"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="5117c-190">Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="5117c-190">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <a name="a-name151151"></a><span data-ttu-id="5117c-191"><a name="1.5.1"/>1.5.1</span><span class="sxs-lookup"><span data-stu-id="5117c-191"><a name="1.5.1"/>1.5.1</span></span>
* <span data-ttu-id="5117c-192">Programhiba rögzített HashPartitionResolver toogenerate kivonatértékeket más SDK konzisztens little endian toobe.</span><span class="sxs-lookup"><span data-stu-id="5117c-192">Fixed a bug in HashPartitionResolver toogenerate hash values in little-endian toobe consistent with other SDKs.</span></span>

### <a name="a-name150150"></a><span data-ttu-id="5117c-193"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="5117c-193"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="5117c-194">Kivonatoló & tartomány hozzáadása partíció feloldókat tooassist a horizontális az alkalmazások több partíciót.</span><span class="sxs-lookup"><span data-stu-id="5117c-194">Add Hash & Range partition resolvers tooassist with sharding applications across multiple partitions.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="5117c-195"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="5117c-195"><a name="1.4.0"/>1.4.0</span></span>
* <span data-ttu-id="5117c-196">Upsert megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="5117c-196">Implement Upsert.</span></span> <span data-ttu-id="5117c-197">Új upsertXXX módszerek toosupport Upsert szolgáltatás hozzá.</span><span class="sxs-lookup"><span data-stu-id="5117c-197">New upsertXXX methods added toosupport Upsert feature.</span></span>
* <span data-ttu-id="5117c-198">Azonosító-alapú útválasztás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="5117c-198">Implement ID Based Routing.</span></span> <span data-ttu-id="5117c-199">Nincs nyilvános API-módosítás, belső összes módosítását.</span><span class="sxs-lookup"><span data-stu-id="5117c-199">No public API changes, all changes internal.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="5117c-200"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="5117c-200"><a name="1.3.0"/>1.3.0</span></span>
* <span data-ttu-id="5117c-201">Kiadás kihagyva Csomagjától összehangolás toobring verziószám</span><span class="sxs-lookup"><span data-stu-id="5117c-201">Release skipped toobring version number in alignment with other SDKs</span></span>

### <a name="a-name120120"></a><span data-ttu-id="5117c-202"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="5117c-202"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="5117c-203">Támogatja a földrajzi Index</span><span class="sxs-lookup"><span data-stu-id="5117c-203">Supports GeoSpatial Index</span></span>
* <span data-ttu-id="5117c-204">Ellenőrzi az összes erőforrás id tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5117c-204">Validates id property for all resources.</span></span> <span data-ttu-id="5117c-205">Erőforrások azonosító nem tartalmazhat?, /, #, \, karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="5117c-205">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="5117c-206">Hozzáadja az új fejléc "index átalakítása folyamatban" tooResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="5117c-206">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="5117c-207"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="5117c-207"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="5117c-208">V2 indexelési házirendet alkalmazza</span><span class="sxs-lookup"><span data-stu-id="5117c-208">Implements V2 indexing policy</span></span>

### <a name="a-name100100"></a><span data-ttu-id="5117c-209"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="5117c-209"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="5117c-210">GA SDK</span><span class="sxs-lookup"><span data-stu-id="5117c-210">GA SDK</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="5117c-211">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="5117c-211">Release & Retirement Dates</span></span>
<span data-ttu-id="5117c-212">Microsoft legalább értesítést küldenek **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.</span><span class="sxs-lookup"><span data-stu-id="5117c-212">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="5117c-213">Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így célszerű a lehető leghamarabb javasoljuk, hogy Ön mindig frissítési toohello SDK letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="5117c-213">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommend that you always upgrade toohello latest SDK version as early as possible.</span></span>

<span data-ttu-id="5117c-214">A kérelem tooCosmos DB kivont SDK használatával a program elutasítja hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5117c-214">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

> [!WARNING]
> <span data-ttu-id="5117c-215">Hello DocumentDB SDK-t a Java előzetes tooversion összes verziója **1.0.0** a rendszerből **2016. február 29-én**.</span><span class="sxs-lookup"><span data-stu-id="5117c-215">All versions of hello DocumentDB SDK for Java prior tooversion **1.0.0** will be retired on **February 29, 2016**.</span></span>
> 
> 

<br/>

| <span data-ttu-id="5117c-216">Verzió</span><span class="sxs-lookup"><span data-stu-id="5117c-216">Version</span></span> | <span data-ttu-id="5117c-217">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="5117c-217">Release Date</span></span> | <span data-ttu-id="5117c-218">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="5117c-218">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="5117c-219">1.12.0</span><span class="sxs-lookup"><span data-stu-id="5117c-219">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="5117c-220">2017. július 11.</span><span class="sxs-lookup"><span data-stu-id="5117c-220">July 11, 2017</span></span> |--- |
| [<span data-ttu-id="5117c-221">1.11.0</span><span class="sxs-lookup"><span data-stu-id="5117c-221">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="5117c-222">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="5117c-222">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="5117c-223">1.10.0</span><span class="sxs-lookup"><span data-stu-id="5117c-223">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="5117c-224">2017. március 11.</span><span class="sxs-lookup"><span data-stu-id="5117c-224">March 11, 2017</span></span> |--- |
| [<span data-ttu-id="5117c-225">1.9.6</span><span class="sxs-lookup"><span data-stu-id="5117c-225">1.9.6</span></span>](#1.9.6) |<span data-ttu-id="5117c-226">2017. február 21.</span><span class="sxs-lookup"><span data-stu-id="5117c-226">February 21, 2017</span></span> |--- |
| [<span data-ttu-id="5117c-227">1.9.5</span><span class="sxs-lookup"><span data-stu-id="5117c-227">1.9.5</span></span>](#1.9.5) |<span data-ttu-id="5117c-228">2017. január 31-ig.</span><span class="sxs-lookup"><span data-stu-id="5117c-228">January 31, 2017</span></span> |--- |
| [<span data-ttu-id="5117c-229">1.9.4</span><span class="sxs-lookup"><span data-stu-id="5117c-229">1.9.4</span></span>](#1.9.4) |<span data-ttu-id="5117c-230">2016. november 24.</span><span class="sxs-lookup"><span data-stu-id="5117c-230">November 24, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-231">1.9.3</span><span class="sxs-lookup"><span data-stu-id="5117c-231">1.9.3</span></span>](#1.9.3) |<span data-ttu-id="5117c-232">2016. október 30.</span><span class="sxs-lookup"><span data-stu-id="5117c-232">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-233">1.9.2</span><span class="sxs-lookup"><span data-stu-id="5117c-233">1.9.2</span></span>](#1.9.2) |<span data-ttu-id="5117c-234">2016. október 28.</span><span class="sxs-lookup"><span data-stu-id="5117c-234">October 28, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-235">1.9.1</span><span class="sxs-lookup"><span data-stu-id="5117c-235">1.9.1</span></span>](#1.9.1) |<span data-ttu-id="5117c-236">2016. október 26.</span><span class="sxs-lookup"><span data-stu-id="5117c-236">October 26, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-237">1.9.0</span><span class="sxs-lookup"><span data-stu-id="5117c-237">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="5117c-238">2016. október 03.</span><span class="sxs-lookup"><span data-stu-id="5117c-238">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-239">1.8.1</span><span class="sxs-lookup"><span data-stu-id="5117c-239">1.8.1</span></span>](#1.8.1) |<span data-ttu-id="5117c-240">2016. június 30.</span><span class="sxs-lookup"><span data-stu-id="5117c-240">June 30, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-241">1.8.0</span><span class="sxs-lookup"><span data-stu-id="5117c-241">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="5117c-242">2016. június 14.</span><span class="sxs-lookup"><span data-stu-id="5117c-242">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-243">1.7.1</span><span class="sxs-lookup"><span data-stu-id="5117c-243">1.7.1</span></span>](#1.7.1) |<span data-ttu-id="5117c-244">2016. április 30.</span><span class="sxs-lookup"><span data-stu-id="5117c-244">April 30, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-245">1.7.0</span><span class="sxs-lookup"><span data-stu-id="5117c-245">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="5117c-246">2016. április 27.</span><span class="sxs-lookup"><span data-stu-id="5117c-246">April 27, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-247">1.6.0</span><span class="sxs-lookup"><span data-stu-id="5117c-247">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="5117c-248">2016. március 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-248">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="5117c-249">1.5.1</span><span class="sxs-lookup"><span data-stu-id="5117c-249">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="5117c-250">2015. december 31.</span><span class="sxs-lookup"><span data-stu-id="5117c-250">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-251">1.5.0</span><span class="sxs-lookup"><span data-stu-id="5117c-251">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="5117c-252">2015. december 04.</span><span class="sxs-lookup"><span data-stu-id="5117c-252">December 04, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-253">1.4.0</span><span class="sxs-lookup"><span data-stu-id="5117c-253">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="5117c-254">2015. október 05.</span><span class="sxs-lookup"><span data-stu-id="5117c-254">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-255">1.3.0</span><span class="sxs-lookup"><span data-stu-id="5117c-255">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="5117c-256">2015. október 05.</span><span class="sxs-lookup"><span data-stu-id="5117c-256">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-257">1.2.0</span><span class="sxs-lookup"><span data-stu-id="5117c-257">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="5117c-258">2015. augusztus 05.</span><span class="sxs-lookup"><span data-stu-id="5117c-258">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-259">1.1.0</span><span class="sxs-lookup"><span data-stu-id="5117c-259">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="5117c-260">2015. július 09.</span><span class="sxs-lookup"><span data-stu-id="5117c-260">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-261">1.0.1</span><span class="sxs-lookup"><span data-stu-id="5117c-261">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="5117c-262">2015. május 12.</span><span class="sxs-lookup"><span data-stu-id="5117c-262">May 12, 2015</span></span> |--- |
| [<span data-ttu-id="5117c-263">1.0.0</span><span class="sxs-lookup"><span data-stu-id="5117c-263">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="5117c-264">2015. április 07.</span><span class="sxs-lookup"><span data-stu-id="5117c-264">April 07, 2015</span></span> |--- |
| <span data-ttu-id="5117c-265">0.9.5-prelease</span><span class="sxs-lookup"><span data-stu-id="5117c-265">0.9.5-prelease</span></span> |<span data-ttu-id="5117c-266">2015. gyel 09.</span><span class="sxs-lookup"><span data-stu-id="5117c-266">Mar 09, 2015</span></span> |<span data-ttu-id="5117c-267">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-267">February 29, 2016</span></span> |
| <span data-ttu-id="5117c-268">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="5117c-268">0.9.4-prelease</span></span> |<span data-ttu-id="5117c-269">2015. február 17.</span><span class="sxs-lookup"><span data-stu-id="5117c-269">February 17, 2015</span></span> |<span data-ttu-id="5117c-270">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-270">February 29, 2016</span></span> |
| <span data-ttu-id="5117c-271">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="5117c-271">0.9.3-prelease</span></span> |<span data-ttu-id="5117c-272">2015. január 13.</span><span class="sxs-lookup"><span data-stu-id="5117c-272">January 13, 2015</span></span> |<span data-ttu-id="5117c-273">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-273">February 29, 2016</span></span> |
| <span data-ttu-id="5117c-274">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="5117c-274">0.9.2-prelease</span></span> |<span data-ttu-id="5117c-275">2014. decemberi 19</span><span class="sxs-lookup"><span data-stu-id="5117c-275">December 19, 2014</span></span> |<span data-ttu-id="5117c-276">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-276">February 29, 2016</span></span> |
| <span data-ttu-id="5117c-277">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="5117c-277">0.9.1-prelease</span></span> |<span data-ttu-id="5117c-278">2014. decemberi 19</span><span class="sxs-lookup"><span data-stu-id="5117c-278">December 19, 2014</span></span> |<span data-ttu-id="5117c-279">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-279">February 29, 2016</span></span> |
| <span data-ttu-id="5117c-280">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="5117c-280">0.9.0-prelease</span></span> |<span data-ttu-id="5117c-281">2014. december 10.</span><span class="sxs-lookup"><span data-stu-id="5117c-281">December 10, 2014</span></span> |<span data-ttu-id="5117c-282">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="5117c-282">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="5117c-283">GYIK</span><span class="sxs-lookup"><span data-stu-id="5117c-283">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="5117c-284">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5117c-284">See Also</span></span>
<span data-ttu-id="5117c-285">toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="5117c-285">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

