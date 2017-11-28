---
title: "Az Azure Cosmos DB: A DocumentDB Java API, SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók a Java API és SDK, beleértve a kiadási dátum, használatból való kivonást dátumok és az Azure Cosmos DB DocumentDB Java SDK verziói között végrehajtott módosításokat."
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
ms.openlocfilehash: 15e3f7ef3bfd6b1f61fe6081a378bdb29e0a1aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-documentdb-java-sdk-release-notes-and-resources"></a><span data-ttu-id="909ae-103">Azure Cosmos DB: A DocumentDB Java SDK kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="909ae-103">Azure Cosmos DB: DocumentDB Java SDK release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="909ae-104">.NET</span><span class="sxs-lookup"><span data-stu-id="909ae-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="909ae-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="909ae-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="909ae-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="909ae-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="909ae-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="909ae-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="909ae-108">Java</span><span class="sxs-lookup"><span data-stu-id="909ae-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="909ae-109">Python</span><span class="sxs-lookup"><span data-stu-id="909ae-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="909ae-110">REST</span><span class="sxs-lookup"><span data-stu-id="909ae-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="909ae-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="909ae-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="909ae-112">SQL</span><span class="sxs-lookup"><span data-stu-id="909ae-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="909ae-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="909ae-113">**SDK Download**</span></span></td><td>[<span data-ttu-id="909ae-114">Maven 3</span><span class="sxs-lookup"><span data-stu-id="909ae-114">Maven</span></span>](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td><span data-ttu-id="909ae-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="909ae-115">**API documentation**</span></span></td><td>[<span data-ttu-id="909ae-116">Java API-referenciadokumentáció</span><span class="sxs-lookup"><span data-stu-id="909ae-116">Java API reference documentation</span></span>](/java/api/com.microsoft.azure.documentdb)</td></tr>

<tr><td><span data-ttu-id="909ae-117">**Hozzájárul az SDK**</span><span class="sxs-lookup"><span data-stu-id="909ae-117">**Contribute to SDK**</span></span></td><td>[<span data-ttu-id="909ae-118">GitHub</span><span class="sxs-lookup"><span data-stu-id="909ae-118">GitHub</span></span>](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td><span data-ttu-id="909ae-119">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="909ae-119">**Get started**</span></span></td><td>[<span data-ttu-id="909ae-120">Ismerkedés a Java SDK-val</span><span class="sxs-lookup"><span data-stu-id="909ae-120">Get started with the Java SDK</span></span>](documentdb-java-get-started.md)</td></tr>

<tr><td><span data-ttu-id="909ae-121">**Webes alkalmazás oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="909ae-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="909ae-122">Webalkalmazás fejlesztése a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="909ae-122">Web application development with Azure Cosmos DB</span></span>](documentdb-java-application.md)</td></tr>

<tr><td><span data-ttu-id="909ae-123">**Aktuális támogatott futásidejű**</span><span class="sxs-lookup"><span data-stu-id="909ae-123">**Current supported runtime**</span></span></td><td>[<span data-ttu-id="909ae-124">JDK 7</span><span class="sxs-lookup"><span data-stu-id="909ae-124">JDK 7</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="909ae-125">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="909ae-125">Release Notes</span></span>

### <a name="a-name11201120"></a><span data-ttu-id="909ae-126"><a name="1.12.0"/>1.12.0</span><span class="sxs-lookup"><span data-stu-id="909ae-126"><a name="1.12.0"/>1.12.0</span></span>
* <span data-ttu-id="909ae-127">A kérelem feldolgozása során partíció elágazást kritikus hibajavítások.</span><span class="sxs-lookup"><span data-stu-id="909ae-127">Critical bug fixes to request processing during partition splits.</span></span>
* <span data-ttu-id="909ae-128">Az erős és BoundedStaleness konzisztenciaszintek rögzített kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="909ae-128">Fixed an issue with the Strong and BoundedStaleness consistency levels.</span></span>

### <a name="a-name11101110"></a><span data-ttu-id="909ae-129"><a name="1.11.0"/>1.11.0</span><span class="sxs-lookup"><span data-stu-id="909ae-129"><a name="1.11.0"/>1.11.0</span></span>
* <span data-ttu-id="909ae-130">Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="909ae-130">Added support for a new consistency level called ConsistentPrefix.</span></span>
* <span data-ttu-id="909ae-131">Rögzített munkamenetmód gyűjtemény olvasási hiba.</span><span class="sxs-lookup"><span data-stu-id="909ae-131">Fixed a bug in reading collection in session mode.</span></span>

### <a name="a-name11001100"></a><span data-ttu-id="909ae-132"><a name="1.10.0"/>1.10.0</span><span class="sxs-lookup"><span data-stu-id="909ae-132"><a name="1.10.0"/>1.10.0</span></span>
* <span data-ttu-id="909ae-133">Particionált gyűjtemény, mint az engedélyezett támogatása 2500 RU/mp, alacsony, a méretezés lépésekben 100 RU/mp.</span><span class="sxs-lookup"><span data-stu-id="909ae-133">Enabled support for partitioned collection with as low as 2,500 RU/sec and scale in increments of 100 RU/sec.</span></span>
* <span data-ttu-id="909ae-134">Rögzített, a hiba a natív szerelvény, ami azt eredményezheti NullRef kivétel néhány lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="909ae-134">Fixed a bug in the native assembly which can cause NullRef exception in some queries.</span></span>

### <a name="a-name196196"></a><span data-ttu-id="909ae-135"><a name="1.9.6"/>1.9.6</span><span class="sxs-lookup"><span data-stu-id="909ae-135"><a name="1.9.6"/>1.9.6</span></span>
* <span data-ttu-id="909ae-136">Programhiba rögzített, a lekérdezési motor konfigurációját, amelyek kivételek lekérdezések okozhatnak átjáró módban.</span><span class="sxs-lookup"><span data-stu-id="909ae-136">Fixed a bug in the query engine configuration that may cause exceptions for queries in Gateway mode.</span></span>
* <span data-ttu-id="909ae-137">A munkamenet-tárolóban, amelyek közvetlenül a webhelycsoport létrehozása után a kéréseket "Tulajdonosa erőforrás nem található" kivétel okozhatnak néhány hibák rögzített.</span><span class="sxs-lookup"><span data-stu-id="909ae-137">Fixed a few bugs in the session container that may cause an "Owner resource not found" exception for requests immediately after collection creation.</span></span>

### <a name="a-name195195"></a><span data-ttu-id="909ae-138"><a name="1.9.5"/>1.9.5</span><span class="sxs-lookup"><span data-stu-id="909ae-138"><a name="1.9.5"/>1.9.5</span></span>
* <span data-ttu-id="909ae-139">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-139">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="909ae-140">Lásd: [összesítési támogatási](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="909ae-140">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="909ae-141">Adatcsatorna módosítás támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-141">Added support for change feed.</span></span>
* <span data-ttu-id="909ae-142">Gyűjtemény kvótaadatok keresztül RequestOptions.setPopulateQuotaInfo támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-142">Added support for collection quota information through RequestOptions.setPopulateQuotaInfo.</span></span>
* <span data-ttu-id="909ae-143">Tárolt eljárás parancsfájl naplózást RequestOptions.setScriptLoggingEnabled támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-143">Added support for stored procedure script logging through RequestOptions.setScriptLoggingEnabled.</span></span>
* <span data-ttu-id="909ae-144">Rögzített programhiba, ahol DirectHttps módban lekérdezés lefagyhat, amikor adatátviteli hiba észlelése.</span><span class="sxs-lookup"><span data-stu-id="909ae-144">Fixed a bug where query in DirectHttps mode may hang when encountering throttle failures.</span></span>
* <span data-ttu-id="909ae-145">Programhiba rögzített munkamenet-konzisztencia módban.</span><span class="sxs-lookup"><span data-stu-id="909ae-145">Fixed a bug in session consistency mode.</span></span>
* <span data-ttu-id="909ae-146">Egy hiba miatt előfordulhat, hogy NullReferenceException a HttpContext Ha nagy a kérések aránya rögzített.</span><span class="sxs-lookup"><span data-stu-id="909ae-146">Fixed a bug which may cause NullReferenceException in HttpContext when request rate is high.</span></span>
* <span data-ttu-id="909ae-147">Javítja a teljesítményt a DirectHttps mód.</span><span class="sxs-lookup"><span data-stu-id="909ae-147">Improved performance of DirectHttps mode.</span></span>

### <a name="a-name194194"></a><span data-ttu-id="909ae-148"><a name="1.9.4"/>1.9.4</span><span class="sxs-lookup"><span data-stu-id="909ae-148"><a name="1.9.4"/>1.9.4</span></span>
* <span data-ttu-id="909ae-149">A hozzáadott egyszerű példány-alapú proxyk ügyféltámogatás ConnectionPolicy.setProxy() API-t.</span><span class="sxs-lookup"><span data-stu-id="909ae-149">Added simple client instance-based proxy support with ConnectionPolicy.setProxy() API.</span></span>
* <span data-ttu-id="909ae-150">A hozzáadott DocumentClient.close() API megfelelően leállítási DocumentClient példányra.</span><span class="sxs-lookup"><span data-stu-id="909ae-150">Added DocumentClient.close() API to properly shutdown DocumentClient instance.</span></span>
* <span data-ttu-id="909ae-151">Javítja a lekérdezési teljesítmény közvetlen kapcsolatot módban való származtatás a lekérdezésterv a natív szerelvény helyett az átjárót.</span><span class="sxs-lookup"><span data-stu-id="909ae-151">Improved query performance in direct connectivity mode by deriving the query plan from the native assembly instead of the Gateway.</span></span>
* <span data-ttu-id="909ae-152">Állítsa be a FAIL_ON_UNKNOWN_PROPERTIES = false, így a felhasználóknak nem kell a pojo-vá JsonIgnoreProperties ad meg.</span><span class="sxs-lookup"><span data-stu-id="909ae-152">Set FAIL_ON_UNKNOWN_PROPERTIES = false so users don't need to define JsonIgnoreProperties in their POJO.</span></span>
* <span data-ttu-id="909ae-153">Átkerült naplózási SLF4J használatára.</span><span class="sxs-lookup"><span data-stu-id="909ae-153">Refactored logging to use SLF4J.</span></span>
* <span data-ttu-id="909ae-154">Rögzített, néhány más hibák konzisztencia-olvasó.</span><span class="sxs-lookup"><span data-stu-id="909ae-154">Fixed a few other bugs in consistency reader.</span></span>

### <a name="a-name193193"></a><span data-ttu-id="909ae-155"><a name="1.9.3"/>1.9.3</span><span class="sxs-lookup"><span data-stu-id="909ae-155"><a name="1.9.3"/>1.9.3</span></span>
* <span data-ttu-id="909ae-156">A kapcsolat kezelése közvetlen kapcsolatot módban kapcsolat kiszivárgásának elkerülésére programhiba rögzített.</span><span class="sxs-lookup"><span data-stu-id="909ae-156">Fixed a bug in the connection management to prevent connection leaks in direct connectivity mode.</span></span>
* <span data-ttu-id="909ae-157">Rögzített programhiba a legfelső szintű lekérdezésben, ahol NullReferenece kivétel generálhat.</span><span class="sxs-lookup"><span data-stu-id="909ae-157">Fixed a bug in the TOP query where it may throw NullReferenece exception.</span></span>
* <span data-ttu-id="909ae-158">Javítja a teljesítményt a belső gyorsítótárat hálózati hívás számának csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="909ae-158">Improved performance by reducing the number of network call for the internal caches.</span></span>
* <span data-ttu-id="909ae-159">A hozzáadott állapotkód, tevékenységazonosító és a kérelem URI-azonosítója a DocumentClientException hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="909ae-159">Added status code, ActivityID and Request URI in DocumentClientException for better troubleshooting.</span></span>

### <a name="a-name192192"></a><span data-ttu-id="909ae-160"><a name="1.9.2"/>1.9.2</span><span class="sxs-lookup"><span data-stu-id="909ae-160"><a name="1.9.2"/>1.9.2</span></span>
* <span data-ttu-id="909ae-161">Megtörtént egy probléma javítása a kapcsolat kezelése stabilitását.</span><span class="sxs-lookup"><span data-stu-id="909ae-161">Fixed an issue in the connection management for stability.</span></span>

### <a name="a-name191191"></a><span data-ttu-id="909ae-162"><a name="1.9.1"/>1.9.1</span><span class="sxs-lookup"><span data-stu-id="909ae-162"><a name="1.9.1"/>1.9.1</span></span>
* <span data-ttu-id="909ae-163">BoundedStaleness konzisztenciaszint támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-163">Added support for BoundedStaleness consistency level.</span></span>
* <span data-ttu-id="909ae-164">Közvetlen kapcsolat CRUD műveleteihez particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-164">Added support for direct connectivity for CRUD operations for partitioned collections.</span></span>
* <span data-ttu-id="909ae-165">SQL adatbázis lekérdezése hiba kijavítva.</span><span class="sxs-lookup"><span data-stu-id="909ae-165">Fixed a bug in querying a database with SQL.</span></span>
* <span data-ttu-id="909ae-166">Programhiba rögzített, a munkamenet-gyorsítótár, ahol munkamenet-azonosító is lehet helytelenül van beállítva.</span><span class="sxs-lookup"><span data-stu-id="909ae-166">Fixed a bug in the session cache where session token may be set incorrectly.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="909ae-167"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="909ae-167"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="909ae-168">Támogatja a párhuzamos lekérdezések partíció közötti.</span><span class="sxs-lookup"><span data-stu-id="909ae-168">Added support for cross partition parallel queries.</span></span>
* <span data-ttu-id="909ae-169">FELSŐ/ORDER BY lekérdezések a particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-169">Added support for TOP/ORDER BY queries for partitioned collections.</span></span>
* <span data-ttu-id="909ae-170">Az erős konzisztencia támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-170">Added support for strong consistency.</span></span>
* <span data-ttu-id="909ae-171">Alapú kérések közvetlen kapcsolat használatakor támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-171">Added support for name based requests when using direct connectivity.</span></span>
* <span data-ttu-id="909ae-172">Ellenőrizze az összes kérelem újrapróbálkozások között konzisztens marad ActivityId rögzített.</span><span class="sxs-lookup"><span data-stu-id="909ae-172">Fixed to make ActivityId stay consistent across all request retries.</span></span>
* <span data-ttu-id="909ae-173">Rögzített egy azonos nevű gyűjtemény újbóli létrehozása során a munkamenet-gyorsítótár kapcsolatos hiba.</span><span class="sxs-lookup"><span data-stu-id="909ae-173">Fixed a bug related to the session cache when recreating a collection with the same name.</span></span>
* <span data-ttu-id="909ae-174">A hozzáadott sokszög és LineString adattípusok indexelő házirend geokerítések térbeli lekérdezéseket a gyűjtemény meg.</span><span class="sxs-lookup"><span data-stu-id="909ae-174">Added Polygon and LineString DataTypes while specifying collection indexing policy for geo-fencing spatial queries.</span></span>
* <span data-ttu-id="909ae-175">Java Doc Java 1.8 rögzített problémákat.</span><span class="sxs-lookup"><span data-stu-id="909ae-175">Fixed issues with Java Doc for Java 1.8.</span></span>

### <a name="a-name181181"></a><span data-ttu-id="909ae-176"><a name="1.8.1"/>1.8.1</span><span class="sxs-lookup"><span data-stu-id="909ae-176"><a name="1.8.1"/>1.8.1</span></span>
* <span data-ttu-id="909ae-177">Az egypartíciós gyűjtemények gyorsítótárba, és nem kulcs kérelmek partícióazonosító extra fetch PartitionKeyDefinitionMap programhiba rögzített.</span><span class="sxs-lookup"><span data-stu-id="909ae-177">Fixed a bug in PartitionKeyDefinitionMap to cache single partition collections and not make extra fetch partition key requests.</span></span>
* <span data-ttu-id="909ae-178">A hiba nem próbálja meg, amikor egy helytelen partíciókulcs-értékkel van megadva a rögzített.</span><span class="sxs-lookup"><span data-stu-id="909ae-178">Fixed a bug to not retry when an incorrect partition key value is provided.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="909ae-179"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="909ae-179"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="909ae-180">Több területi adatbázis fiókok támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-180">Added the support for multi-region database accounts.</span></span>
* <span data-ttu-id="909ae-181">A beállítások az újrapróbálkozások maximális és a várakozási idő maximális újrapróbálkozási testreszabásához szabályozottan halmozott kérelmek automatikus újrapróbálkozási támogatása.</span><span class="sxs-lookup"><span data-stu-id="909ae-181">Added support for automatic retry on throttled requests with options to customize the max retry attempts and max retry wait time.</span></span>  <span data-ttu-id="909ae-182">Lásd: RetryOptions és ConnectionPolicy.getRetryOptions().</span><span class="sxs-lookup"><span data-stu-id="909ae-182">See RetryOptions and ConnectionPolicy.getRetryOptions().</span></span>
* <span data-ttu-id="909ae-183">Elavult IPartitionResolver egyéni particionálási kód alapján.</span><span class="sxs-lookup"><span data-stu-id="909ae-183">Deprecated IPartitionResolver based custom partitioning code.</span></span> <span data-ttu-id="909ae-184">A particionált gyűjtemények használata magasabb tárolási és átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="909ae-184">Please use partitioned collections for higher storage and throughput.</span></span>

### <a name="a-name171171"></a><span data-ttu-id="909ae-185"><a name="1.7.1"/>1.7.1</span><span class="sxs-lookup"><span data-stu-id="909ae-185"><a name="1.7.1"/>1.7.1</span></span>
* <span data-ttu-id="909ae-186">A hozzáadott újrapróbálkozási házirend támogatása sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="909ae-186">Added retry policy support for throttling.</span></span>  

### <a name="a-name170170"></a><span data-ttu-id="909ae-187"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="909ae-187"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="909ae-188">A hozzáadott idő live (TTL)-támogatás a dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="909ae-188">Added time to live (TTL) support for documents.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="909ae-189"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="909ae-189"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="909ae-190">Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="909ae-190">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span>

### <a name="a-name151151"></a><span data-ttu-id="909ae-191"><a name="1.5.1"/>1.5.1</span><span class="sxs-lookup"><span data-stu-id="909ae-191"><a name="1.5.1"/>1.5.1</span></span>
* <span data-ttu-id="909ae-192">Programhiba rögzített HashPartitionResolver kivonatértékeket little endian Csomagjától konzisztens kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="909ae-192">Fixed a bug in HashPartitionResolver to generate hash values in little-endian to be consistent with other SDKs.</span></span>

### <a name="a-name150150"></a><span data-ttu-id="909ae-193"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="909ae-193"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="909ae-194">Kivonatoló & tartomány hozzáadása partícióazonosító feloldókat, elősegítve ezzel a horizontális alkalmazások között több partíciót.</span><span class="sxs-lookup"><span data-stu-id="909ae-194">Add Hash & Range partition resolvers to assist with sharding applications across multiple partitions.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="909ae-195"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="909ae-195"><a name="1.4.0"/>1.4.0</span></span>
* <span data-ttu-id="909ae-196">Upsert megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="909ae-196">Implement Upsert.</span></span> <span data-ttu-id="909ae-197">Új upsertXXX módszerek Upsert szolgáltatás támogatása érdekében adott hozzá.</span><span class="sxs-lookup"><span data-stu-id="909ae-197">New upsertXXX methods added to support Upsert feature.</span></span>
* <span data-ttu-id="909ae-198">Azonosító-alapú útválasztás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="909ae-198">Implement ID Based Routing.</span></span> <span data-ttu-id="909ae-199">Nincs nyilvános API-módosítás, belső összes módosítását.</span><span class="sxs-lookup"><span data-stu-id="909ae-199">No public API changes, all changes internal.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="909ae-200"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="909ae-200"><a name="1.3.0"/>1.3.0</span></span>
* <span data-ttu-id="909ae-201">Kiadás verziószáma átveendő Csomagjától összehangolás kihagyva</span><span class="sxs-lookup"><span data-stu-id="909ae-201">Release skipped to bring version number in alignment with other SDKs</span></span>

### <a name="a-name120120"></a><span data-ttu-id="909ae-202"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="909ae-202"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="909ae-203">Támogatja a földrajzi Index</span><span class="sxs-lookup"><span data-stu-id="909ae-203">Supports GeoSpatial Index</span></span>
* <span data-ttu-id="909ae-204">Ellenőrzi az összes erőforrás id tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="909ae-204">Validates id property for all resources.</span></span> <span data-ttu-id="909ae-205">Erőforrások azonosító nem tartalmazhat?, /, #, \, karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="909ae-205">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="909ae-206">Új fejléc "index átalakítása folyamatban" hozzáadása ResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="909ae-206">Adds new header "index transformation progress" to ResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="909ae-207"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="909ae-207"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="909ae-208">V2 indexelési házirendet alkalmazza</span><span class="sxs-lookup"><span data-stu-id="909ae-208">Implements V2 indexing policy</span></span>

### <a name="a-name100100"></a><span data-ttu-id="909ae-209"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="909ae-209"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="909ae-210">GA SDK</span><span class="sxs-lookup"><span data-stu-id="909ae-210">GA SDK</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="909ae-211">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="909ae-211">Release & Retirement Dates</span></span>
<span data-ttu-id="909ae-212">Microsoft legalább értesítést küldenek **12 hónapon keresztül** SDK eltávolítása érdekében vagy újabb támogatott verzióra való áttérés előtt.</span><span class="sxs-lookup"><span data-stu-id="909ae-212">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.</span></span>

<span data-ttu-id="909ae-213">Új szolgáltatásait és funkcióit és optimalizálás csak hozzá az aktuális SDK, így javasoljuk, hogy mindig a legújabb SDK verzióra frissít legkorábban lehető.</span><span class="sxs-lookup"><span data-stu-id="909ae-213">New features and functionality and optimizations are only added to the current SDK, as such it is  recommend that you always upgrade to the latest SDK version as early as possible.</span></span>

<span data-ttu-id="909ae-214">A Cosmos DB kivont SDK használatával fog kell elutasította a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="909ae-214">Any request to Cosmos DB using a retired SDK will be rejected by the service.</span></span>

> [!WARNING]
> <span data-ttu-id="909ae-215">A DocumentDB Java SDK-es vagy korábbi összes verziója **1.0.0** a rendszerből **2016. február 29-én**.</span><span class="sxs-lookup"><span data-stu-id="909ae-215">All versions of the DocumentDB SDK for Java prior to version **1.0.0** will be retired on **February 29, 2016**.</span></span>
> 
> 

<br/>

| <span data-ttu-id="909ae-216">Verzió</span><span class="sxs-lookup"><span data-stu-id="909ae-216">Version</span></span> | <span data-ttu-id="909ae-217">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="909ae-217">Release Date</span></span> | <span data-ttu-id="909ae-218">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="909ae-218">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="909ae-219">1.12.0</span><span class="sxs-lookup"><span data-stu-id="909ae-219">1.12.0</span></span>](#1.12.0) |<span data-ttu-id="909ae-220">2017. július 11.</span><span class="sxs-lookup"><span data-stu-id="909ae-220">July 11, 2017</span></span> |--- |
| [<span data-ttu-id="909ae-221">1.11.0</span><span class="sxs-lookup"><span data-stu-id="909ae-221">1.11.0</span></span>](#1.11.0) |<span data-ttu-id="909ae-222">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="909ae-222">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="909ae-223">1.10.0</span><span class="sxs-lookup"><span data-stu-id="909ae-223">1.10.0</span></span>](#1.10.0) |<span data-ttu-id="909ae-224">2017. március 11.</span><span class="sxs-lookup"><span data-stu-id="909ae-224">March 11, 2017</span></span> |--- |
| [<span data-ttu-id="909ae-225">1.9.6</span><span class="sxs-lookup"><span data-stu-id="909ae-225">1.9.6</span></span>](#1.9.6) |<span data-ttu-id="909ae-226">2017. február 21.</span><span class="sxs-lookup"><span data-stu-id="909ae-226">February 21, 2017</span></span> |--- |
| [<span data-ttu-id="909ae-227">1.9.5</span><span class="sxs-lookup"><span data-stu-id="909ae-227">1.9.5</span></span>](#1.9.5) |<span data-ttu-id="909ae-228">2017. január 31-ig.</span><span class="sxs-lookup"><span data-stu-id="909ae-228">January 31, 2017</span></span> |--- |
| [<span data-ttu-id="909ae-229">1.9.4</span><span class="sxs-lookup"><span data-stu-id="909ae-229">1.9.4</span></span>](#1.9.4) |<span data-ttu-id="909ae-230">2016. november 24.</span><span class="sxs-lookup"><span data-stu-id="909ae-230">November 24, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-231">1.9.3</span><span class="sxs-lookup"><span data-stu-id="909ae-231">1.9.3</span></span>](#1.9.3) |<span data-ttu-id="909ae-232">2016. október 30.</span><span class="sxs-lookup"><span data-stu-id="909ae-232">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-233">1.9.2</span><span class="sxs-lookup"><span data-stu-id="909ae-233">1.9.2</span></span>](#1.9.2) |<span data-ttu-id="909ae-234">2016. október 28.</span><span class="sxs-lookup"><span data-stu-id="909ae-234">October 28, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-235">1.9.1</span><span class="sxs-lookup"><span data-stu-id="909ae-235">1.9.1</span></span>](#1.9.1) |<span data-ttu-id="909ae-236">2016. október 26.</span><span class="sxs-lookup"><span data-stu-id="909ae-236">October 26, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-237">1.9.0</span><span class="sxs-lookup"><span data-stu-id="909ae-237">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="909ae-238">2016. október 03.</span><span class="sxs-lookup"><span data-stu-id="909ae-238">October 03, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-239">1.8.1</span><span class="sxs-lookup"><span data-stu-id="909ae-239">1.8.1</span></span>](#1.8.1) |<span data-ttu-id="909ae-240">2016. június 30.</span><span class="sxs-lookup"><span data-stu-id="909ae-240">June 30, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-241">1.8.0</span><span class="sxs-lookup"><span data-stu-id="909ae-241">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="909ae-242">2016. június 14.</span><span class="sxs-lookup"><span data-stu-id="909ae-242">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-243">1.7.1</span><span class="sxs-lookup"><span data-stu-id="909ae-243">1.7.1</span></span>](#1.7.1) |<span data-ttu-id="909ae-244">2016. április 30.</span><span class="sxs-lookup"><span data-stu-id="909ae-244">April 30, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-245">1.7.0</span><span class="sxs-lookup"><span data-stu-id="909ae-245">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="909ae-246">2016. április 27.</span><span class="sxs-lookup"><span data-stu-id="909ae-246">April 27, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-247">1.6.0</span><span class="sxs-lookup"><span data-stu-id="909ae-247">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="909ae-248">2016. március 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-248">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="909ae-249">1.5.1</span><span class="sxs-lookup"><span data-stu-id="909ae-249">1.5.1</span></span>](#1.5.1) |<span data-ttu-id="909ae-250">2015. december 31.</span><span class="sxs-lookup"><span data-stu-id="909ae-250">December 31, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-251">1.5.0</span><span class="sxs-lookup"><span data-stu-id="909ae-251">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="909ae-252">2015. december 04.</span><span class="sxs-lookup"><span data-stu-id="909ae-252">December 04, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-253">1.4.0</span><span class="sxs-lookup"><span data-stu-id="909ae-253">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="909ae-254">2015. október 05.</span><span class="sxs-lookup"><span data-stu-id="909ae-254">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-255">1.3.0</span><span class="sxs-lookup"><span data-stu-id="909ae-255">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="909ae-256">2015. október 05.</span><span class="sxs-lookup"><span data-stu-id="909ae-256">October 05, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-257">1.2.0</span><span class="sxs-lookup"><span data-stu-id="909ae-257">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="909ae-258">2015. augusztus 05.</span><span class="sxs-lookup"><span data-stu-id="909ae-258">August 05, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-259">1.1.0</span><span class="sxs-lookup"><span data-stu-id="909ae-259">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="909ae-260">2015. július 09.</span><span class="sxs-lookup"><span data-stu-id="909ae-260">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-261">1.0.1</span><span class="sxs-lookup"><span data-stu-id="909ae-261">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="909ae-262">2015. május 12.</span><span class="sxs-lookup"><span data-stu-id="909ae-262">May 12, 2015</span></span> |--- |
| [<span data-ttu-id="909ae-263">1.0.0</span><span class="sxs-lookup"><span data-stu-id="909ae-263">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="909ae-264">2015. április 07.</span><span class="sxs-lookup"><span data-stu-id="909ae-264">April 07, 2015</span></span> |--- |
| <span data-ttu-id="909ae-265">0.9.5-prelease</span><span class="sxs-lookup"><span data-stu-id="909ae-265">0.9.5-prelease</span></span> |<span data-ttu-id="909ae-266">2015. gyel 09.</span><span class="sxs-lookup"><span data-stu-id="909ae-266">Mar 09, 2015</span></span> |<span data-ttu-id="909ae-267">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-267">February 29, 2016</span></span> |
| <span data-ttu-id="909ae-268">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="909ae-268">0.9.4-prelease</span></span> |<span data-ttu-id="909ae-269">2015. február 17.</span><span class="sxs-lookup"><span data-stu-id="909ae-269">February 17, 2015</span></span> |<span data-ttu-id="909ae-270">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-270">February 29, 2016</span></span> |
| <span data-ttu-id="909ae-271">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="909ae-271">0.9.3-prelease</span></span> |<span data-ttu-id="909ae-272">2015. január 13.</span><span class="sxs-lookup"><span data-stu-id="909ae-272">January 13, 2015</span></span> |<span data-ttu-id="909ae-273">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-273">February 29, 2016</span></span> |
| <span data-ttu-id="909ae-274">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="909ae-274">0.9.2-prelease</span></span> |<span data-ttu-id="909ae-275">2014. decemberi 19</span><span class="sxs-lookup"><span data-stu-id="909ae-275">December 19, 2014</span></span> |<span data-ttu-id="909ae-276">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-276">February 29, 2016</span></span> |
| <span data-ttu-id="909ae-277">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="909ae-277">0.9.1-prelease</span></span> |<span data-ttu-id="909ae-278">2014. decemberi 19</span><span class="sxs-lookup"><span data-stu-id="909ae-278">December 19, 2014</span></span> |<span data-ttu-id="909ae-279">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-279">February 29, 2016</span></span> |
| <span data-ttu-id="909ae-280">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="909ae-280">0.9.0-prelease</span></span> |<span data-ttu-id="909ae-281">2014. december 10.</span><span class="sxs-lookup"><span data-stu-id="909ae-281">December 10, 2014</span></span> |<span data-ttu-id="909ae-282">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="909ae-282">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="909ae-283">GYIK</span><span class="sxs-lookup"><span data-stu-id="909ae-283">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="909ae-284">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="909ae-284">See Also</span></span>
<span data-ttu-id="909ae-285">A Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="909ae-285">To learn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span>

