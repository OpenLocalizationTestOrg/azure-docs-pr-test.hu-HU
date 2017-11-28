---
title: "aaaAzure Cosmos DB Python API SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello Python API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB Python SDK verziói között."
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
ms.openlocfilehash: 1a164b72d2bd819de87df0229357b82e2177af2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-sdk-release-notes-and-resources"></a><span data-ttu-id="0fa9e-103">Az Azure Cosmos DB Python SDK: A kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="0fa9e-103">Azure Cosmos DB Python SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0fa9e-104">.NET</span><span class="sxs-lookup"><span data-stu-id="0fa9e-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="0fa9e-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="0fa9e-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="0fa9e-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0fa9e-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="0fa9e-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="0fa9e-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="0fa9e-108">Java</span><span class="sxs-lookup"><span data-stu-id="0fa9e-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="0fa9e-109">Python</span><span class="sxs-lookup"><span data-stu-id="0fa9e-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="0fa9e-110">REST</span><span class="sxs-lookup"><span data-stu-id="0fa9e-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="0fa9e-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="0fa9e-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="0fa9e-112">SQL</span><span class="sxs-lookup"><span data-stu-id="0fa9e-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="0fa9e-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="0fa9e-113">**Download SDK**</span></span></td><td>[<span data-ttu-id="0fa9e-114">PyPI</span><span class="sxs-lookup"><span data-stu-id="0fa9e-114">PyPI</span></span>](https://pypi.python.org/pypi/pydocumentdb)</td></tr>

<tr><td><span data-ttu-id="0fa9e-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="0fa9e-115">**API documentation**</span></span></td><td>[<span data-ttu-id="0fa9e-116">Python API referenciadokumentációt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="0fa9e-116">Python API reference documentation</span></span>](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>

<tr><td><span data-ttu-id="0fa9e-117">**SDK telepítési utasításokat**</span><span class="sxs-lookup"><span data-stu-id="0fa9e-117">**SDK installation instructions**</span></span></td><td>[<span data-ttu-id="0fa9e-118">Python SDK telepítési utasításokat</span><span class="sxs-lookup"><span data-stu-id="0fa9e-118">Python SDK installation instructions</span></span>](http://azure.github.io/azure-documentdb-python/)</td></tr>

<tr><td><span data-ttu-id="0fa9e-119">**Közreműködési lehetőségek tooSDK**</span><span class="sxs-lookup"><span data-stu-id="0fa9e-119">**Contribute tooSDK**</span></span></td><td>[<span data-ttu-id="0fa9e-120">GitHub</span><span class="sxs-lookup"><span data-stu-id="0fa9e-120">GitHub</span></span>](https://github.com/Azure/azure-documentdb-python)</td></tr>

<tr><td><span data-ttu-id="0fa9e-121">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="0fa9e-121">**Get started**</span></span></td><td>[<span data-ttu-id="0fa9e-122">Ismerkedés a hello Python SDK</span><span class="sxs-lookup"><span data-stu-id="0fa9e-122">Get started with hello Python SDK</span></span>](documentdb-python-application.md)</td></tr>

<tr><td><span data-ttu-id="0fa9e-123">**Aktuális támogatott platform**</span><span class="sxs-lookup"><span data-stu-id="0fa9e-123">**Current supported platform**</span></span></td><td><span data-ttu-id="0fa9e-124">[Python 2.7](https://www.python.org/downloads/) és [Python 3.5](https://www.python.org/downloads/)</span><span class="sxs-lookup"><span data-stu-id="0fa9e-124">[Python 2.7](https://www.python.org/downloads/) and [Python 3.5](https://www.python.org/downloads/)</span></span></td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="0fa9e-125">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0fa9e-125">Release notes</span></span>
### <a name="a-name220220"></a><span data-ttu-id="0fa9e-126"><a name="2.2.0"/>2.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-126"><a name="2.2.0"/>2.2.0</span></span>
* <span data-ttu-id="0fa9e-127">Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-127">Added support for a new consistency level called ConsistentPrefix.</span></span>


### <a name="a-name210210"></a><span data-ttu-id="0fa9e-128"><a name="2.1.0"/>2.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-128"><a name="2.1.0"/>2.1.0</span></span>
* <span data-ttu-id="0fa9e-129">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-129">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="0fa9e-130">A beállítás a letiltás SSL ellenőrzési Cosmos DB emulatorban futtatásakor hozzá.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-130">Added an option for disabling SSL verification when running against Cosmos DB Emulator.</span></span>
* <span data-ttu-id="0fa9e-131">Függő kérések modul toobe pontosan 2.10.0 hello korlátozása eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-131">Removed hello restriction of dependent requests module toobe exactly 2.10.0.</span></span>
* <span data-ttu-id="0fa9e-132">A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-132">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>
* <span data-ttu-id="0fa9e-133">Támogatja a tárolt eljárás végrehajtása során parancsfájl naplózásának engedélyezéséről.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-133">Added support for enabling script logging during stored procedure execution.</span></span>
* <span data-ttu-id="0fa9e-134">REST API-verzió bumped túl "2017-01-19' ebben a kiadásban.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-134">REST API version bumped too'2017-01-19' with this release.</span></span>

### <a name="a-name201201"></a><span data-ttu-id="0fa9e-135"><a name="2.0.1"/>2.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-135"><a name="2.0.1"/>2.0.1</span></span>
* <span data-ttu-id="0fa9e-136">A Szerkesztői módosítások végrehajtott toodocumentation megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-136">Made editorial changes toodocumentation comments.</span></span>

### <a name="a-name200200"></a><span data-ttu-id="0fa9e-137"><a name="2.0.0"/>2.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-137"><a name="2.0.0"/>2.0.0</span></span>
* <span data-ttu-id="0fa9e-138">Python 3.5 támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-138">Added support for Python 3.5.</span></span>
* <span data-ttu-id="0fa9e-139">Támogatja a kapcsolatkészletezést kérelmek modul használatával.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-139">Added support for connection pooling using a requests module.</span></span>
* <span data-ttu-id="0fa9e-140">A munkamenet-konzisztencia támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-140">Added support for session consistency.</span></span>
* <span data-ttu-id="0fa9e-141">FELSŐ/ORDERBY lekérdezések particionált gyűjtemények támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-141">Added support for TOP/ORDERBY queries for partitioned collections.</span></span>

### <a name="a-name190190"></a><span data-ttu-id="0fa9e-142"><a name="1.9.0"/>1.9.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-142"><a name="1.9.0"/>1.9.0</span></span>
* <span data-ttu-id="0fa9e-143">Szabályozottan halmozott kérelmek hozzáadott újrapróbálkozási házirend támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-143">Added retry policy support for throttled requests.</span></span> <span data-ttu-id="0fa9e-144">(A szabályozottan halmozott kérelmek egy kérelem aránya túl nagy kivétel, hibakód 429 kapnak.) Alapértelmezés szerint Azure Cosmos DB újrapróbálja kilenc alkalommal az egyes kérelmek 429. Hibakód: a rendszer észlelt, amikor hello retryAfter idő hello válaszfejléc érvényesítenie.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-144">(Throttled requests receive a request rate too large exception, error code 429.) By default, Azure Cosmos DB retries nine times for each request when error code 429 is encountered, honoring hello retryAfter time in hello response header.</span></span> <span data-ttu-id="0fa9e-145">A rögzített újrapróbálkozási időköz most már beállítható hello RetryOptions tulajdonság részeként hello ConnectionPolicy objektum Ha azt szeretné, hogy a kiszolgáló által visszaadott hello újrapróbálkozások között tooignore hello retryAfter idő.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-145">A fixed retry interval time can now be set as part of hello RetryOptions property on hello ConnectionPolicy object if you want tooignore hello retryAfter time returned by server between hello retries.</span></span> <span data-ttu-id="0fa9e-146">Azure Cosmos-adatbázis most megvárja, legfeljebb 30 másodpercig minden egyes kérelemhez (függetlenül újrapróbálkozások száma) leszabályozta, és hibakód 429 hello választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-146">Azure Cosmos DB now waits for a maximum of 30 seconds for each request that is being throttled (irrespective of retry count) and returns hello response with error code 429.</span></span> <span data-ttu-id="0fa9e-147">Most is lehet a hello RetryOptions tulajdonság ConnectionPolicy objektumon.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-147">This time can also be overriden in hello RetryOptions property on ConnectionPolicy object.</span></span>
* <span data-ttu-id="0fa9e-148">Cosmos DB most adja vissza x-ms-szabályozási--újrapróbálkozások és x-ms-throttle-retry-wait-time-ms, minden kérelem toodenote hello késleltetési hello válaszfejlécek újrapróbálkozás száma és hello cummulative időpontja hello kérelem hello újrapróbálkozások között várt.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-148">Cosmos DB now returns x-ms-throttle-retry-count and x-ms-throttle-retry-wait-time-ms as hello response headers in every request toodenote hello throttle retry count and hello cummulative time hello request waited between hello retries.</span></span>
* <span data-ttu-id="0fa9e-149">Eltávolított hello RetryPolicy osztály és hello megfelelő tulajdonságával (retry_policy) kikerültek a hello document_client osztály, és ehelyett bevezetett ConnectionPolicy osztály, amely használt toooverride hello RetryOptions tulajdonsága kitettségének RetryOptions osztály Néhány hello alapértelmezett ismételje meg a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-149">Removed hello RetryPolicy class and hello corresponding property (retry_policy) exposed on hello document_client class and instead introduced a RetryOptions class exposing hello RetryOptions property on ConnectionPolicy class that can be used toooverride some of hello default retry options.</span></span>

### <a name="a-name180180"></a><span data-ttu-id="0fa9e-150"><a name="1.8.0"/>1.8.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-150"><a name="1.8.0"/>1.8.0</span></span>
* <span data-ttu-id="0fa9e-151">Több területi adatbázis fiókok hozzáadott hello támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-151">Added hello support for multi-region database accounts.</span></span>

### <a name="a-name170170"></a><span data-ttu-id="0fa9e-152"><a name="1.7.0"/>1.7.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-152"><a name="1.7.0"/>1.7.0</span></span>
* <span data-ttu-id="0fa9e-153">Hozzáadott hello idő tooLive(TTL) szolgáltatás dokumentumok támogatása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-153">Added hello support for Time tooLive(TTL) feature for documents.</span></span>

### <a name="a-name161161"></a><span data-ttu-id="0fa9e-154"><a name="1.6.1"/>1.6.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-154"><a name="1.6.1"/>1.6.1</span></span>
* <span data-ttu-id="0fa9e-155">Hibajavítások kapcsolódó tooserver ügyféloldali particionálás partitionkey elérési út tooallow különleges karaktereket.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-155">Bug fixes related tooserver side partitioning tooallow special characters in partitionkey path.</span></span>

### <a name="a-name160160"></a><span data-ttu-id="0fa9e-156"><a name="1.6.0"/>1.6.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-156"><a name="1.6.0"/>1.6.0</span></span>
* <span data-ttu-id="0fa9e-157">Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="0fa9e-157">Implemented [partitioned collections](partition-data.md) and [user-defined performance levels](performance-levels.md).</span></span> 

### <a name="a-name150150"></a><span data-ttu-id="0fa9e-158"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-158"><a name="1.5.0"/>1.5.0</span></span>
* <span data-ttu-id="0fa9e-159">Kivonatoló & tartomány hozzáadása partíció feloldókat tooassist a horizontális az alkalmazások több partíciót.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-159">Add Hash & Range partition resolvers tooassist with sharding applications across multiple partitions.</span></span>

### <a name="a-name142142"></a><span data-ttu-id="0fa9e-160"><a name="1.4.2"/>1.4.2</span><span class="sxs-lookup"><span data-stu-id="0fa9e-160"><a name="1.4.2"/>1.4.2</span></span>
* <span data-ttu-id="0fa9e-161">Upsert megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-161">Implement Upsert.</span></span> <span data-ttu-id="0fa9e-162">Új UpsertXXX módszerek toosupport Upsert szolgáltatás hozzá.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-162">New UpsertXXX methods added toosupport Upsert feature.</span></span>
* <span data-ttu-id="0fa9e-163">Azonosító-alapú útválasztás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-163">Implement ID Based Routing.</span></span> <span data-ttu-id="0fa9e-164">Nincs nyilvános API-módosítás, belső összes módosítását.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-164">No public API changes, all changes internal.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="0fa9e-165"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-165"><a name="1.2.0"/>1.2.0</span></span>
* <span data-ttu-id="0fa9e-166">Támogatja a földrajzi index.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-166">Supports GeoSpatial index.</span></span>
* <span data-ttu-id="0fa9e-167">Ellenőrzi az összes erőforrás id tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-167">Validates id property for all resources.</span></span> <span data-ttu-id="0fa9e-168">Erőforrások azonosító nem tartalmazhat?, /, #, \, karaktereket vagy záró szóközt.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-168">Ids for resources cannot contain ?, /, #, \, characters or end with a space.</span></span>
* <span data-ttu-id="0fa9e-169">Hozzáadja az új fejléc "index átalakítása folyamatban" tooResourceResponse.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-169">Adds new header "index transformation progress" tooResourceResponse.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="0fa9e-170"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-170"><a name="1.1.0"/>1.1.0</span></span>
* <span data-ttu-id="0fa9e-171">V2 indexelési házirendet alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-171">Implements V2 indexing policy.</span></span>

### <a name="a-name101101"></a><span data-ttu-id="0fa9e-172"><a name="1.0.1"/>1.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-172"><a name="1.0.1"/>1.0.1</span></span>
* <span data-ttu-id="0fa9e-173">Proxy kapcsolatot támogat.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-173">Supports proxy connection.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="0fa9e-174"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-174"><a name="1.0.0"/>1.0.0</span></span>
* <span data-ttu-id="0fa9e-175">GA SDK.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-175">GA SDK.</span></span>

## <a name="release--retirement-dates"></a><span data-ttu-id="0fa9e-176">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="0fa9e-176">Release & retirement dates</span></span>
<span data-ttu-id="0fa9e-177">Microsoft legalább értesítést küldenek **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-177">Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order toosmooth hello transition tooa newer/supported version.</span></span>

<span data-ttu-id="0fa9e-178">Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így célszerű a lehető leghamarabb javasoljuk, hogy Ön mindig frissítési toohello SDK letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-178">New features and functionality and optimizations are only added toohello current SDK, as such it is  recommend that you always upgrade toohello latest SDK version as early as possible.</span></span> 

<span data-ttu-id="0fa9e-179">A kérelem tooCosmos DB kivont SDK használatával a program elutasítja hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-179">Any request tooCosmos DB using a retired SDK will be rejected by hello service.</span></span>

> [!WARNING]
> <span data-ttu-id="0fa9e-180">Hello Azure DocumentDB SDK-t a Python előzetes tooversion összes verziója **1.0.0** a rendszerből **2016. február 29-én**.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-180">All versions of hello Azure DocumentDB SDK for Python prior tooversion **1.0.0** will be retired on **February 29, 2016**.</span></span> 
> 
> 

<br/>

| <span data-ttu-id="0fa9e-181">Verzió</span><span class="sxs-lookup"><span data-stu-id="0fa9e-181">Version</span></span> | <span data-ttu-id="0fa9e-182">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="0fa9e-182">Release Date</span></span> | <span data-ttu-id="0fa9e-183">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="0fa9e-183">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="0fa9e-184">2.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-184">2.2.0</span></span>](#2.2.0) |<span data-ttu-id="0fa9e-185">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-185">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="0fa9e-186">2.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-186">2.1.0</span></span>](#2.1.0) |<span data-ttu-id="0fa9e-187">2017. Előfordulhat, hogy 01.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-187">May 01, 2017</span></span> |--- |
| [<span data-ttu-id="0fa9e-188">2.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-188">2.0.1</span></span>](#2.0.1) |<span data-ttu-id="0fa9e-189">2016. október 30.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-189">October 30, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-190">2.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-190">2.0.0</span></span>](#2.0.0) |<span data-ttu-id="0fa9e-191">2016. Szeptembertől 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-191">September 29, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-192">1.9.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-192">1.9.0</span></span>](#1.9.0) |<span data-ttu-id="0fa9e-193">2016. július 07.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-193">July 07, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-194">1.8.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-194">1.8.0</span></span>](#1.8.0) |<span data-ttu-id="0fa9e-195">2016. június 14.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-195">June 14, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-196">1.7.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-196">1.7.0</span></span>](#1.7.0) |<span data-ttu-id="0fa9e-197">2016. április 26.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-197">April 26, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-198">1.6.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-198">1.6.1</span></span>](#1.6.1) |<span data-ttu-id="0fa9e-199">2016. április 08.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-199">April 08, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-200">1.6.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-200">1.6.0</span></span>](#1.6.0) |<span data-ttu-id="0fa9e-201">2016. március 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-201">March 29, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-202">1.5.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-202">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="0fa9e-203">2016. január 03.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-203">January 03, 2016</span></span> |--- |
| [<span data-ttu-id="0fa9e-204">1.4.2</span><span class="sxs-lookup"><span data-stu-id="0fa9e-204">1.4.2</span></span>](#1.4.2) |<span data-ttu-id="0fa9e-205">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-205">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="0fa9e-206">1.4.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-206">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="0fa9e-207">2015. október 06.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-207">October 06, 2015</span></span> |--- |
| [<span data-ttu-id="0fa9e-208">1.2.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-208">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="0fa9e-209">2015. augusztus 06.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-209">August 06, 2015</span></span> |--- |
| [<span data-ttu-id="0fa9e-210">1.1.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-210">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="0fa9e-211">2015. július 09.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-211">July 09, 2015</span></span> |--- |
| [<span data-ttu-id="0fa9e-212">1.0.1</span><span class="sxs-lookup"><span data-stu-id="0fa9e-212">1.0.1</span></span>](#1.0.1) |<span data-ttu-id="0fa9e-213">2015. május 25.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-213">May 25, 2015</span></span> |--- |
| [<span data-ttu-id="0fa9e-214">1.0.0</span><span class="sxs-lookup"><span data-stu-id="0fa9e-214">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="0fa9e-215">2015. április 07.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-215">April 07, 2015</span></span> |--- |
| <span data-ttu-id="0fa9e-216">0.9.4-prelease</span><span class="sxs-lookup"><span data-stu-id="0fa9e-216">0.9.4-prelease</span></span> |<span data-ttu-id="0fa9e-217">2015. január 14.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-217">January 14, 2015</span></span> |<span data-ttu-id="0fa9e-218">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-218">February 29, 2016</span></span> |
| <span data-ttu-id="0fa9e-219">0.9.3-prelease</span><span class="sxs-lookup"><span data-stu-id="0fa9e-219">0.9.3-prelease</span></span> |<span data-ttu-id="0fa9e-220">2014. decemberi 09</span><span class="sxs-lookup"><span data-stu-id="0fa9e-220">December 09, 2014</span></span> |<span data-ttu-id="0fa9e-221">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-221">February 29, 2016</span></span> |
| <span data-ttu-id="0fa9e-222">0.9.2-prelease</span><span class="sxs-lookup"><span data-stu-id="0fa9e-222">0.9.2-prelease</span></span> |<span data-ttu-id="0fa9e-223">2014. november 25.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-223">November 25, 2014</span></span> |<span data-ttu-id="0fa9e-224">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-224">February 29, 2016</span></span> |
| <span data-ttu-id="0fa9e-225">0.9.1-prelease</span><span class="sxs-lookup"><span data-stu-id="0fa9e-225">0.9.1-prelease</span></span> |<span data-ttu-id="0fa9e-226">2014. szeptember 23.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-226">September 23, 2014</span></span> |<span data-ttu-id="0fa9e-227">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-227">February 29, 2016</span></span> |
| <span data-ttu-id="0fa9e-228">0.9.0-prelease</span><span class="sxs-lookup"><span data-stu-id="0fa9e-228">0.9.0-prelease</span></span> |<span data-ttu-id="0fa9e-229">2014. augusztus 21.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-229">August 21, 2014</span></span> |<span data-ttu-id="0fa9e-230">2016. február 29.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-230">February 29, 2016</span></span> |

## <a name="faq"></a><span data-ttu-id="0fa9e-231">GYIK</span><span class="sxs-lookup"><span data-stu-id="0fa9e-231">FAQ</span></span>
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a><span data-ttu-id="0fa9e-232">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0fa9e-232">See also</span></span>
<span data-ttu-id="0fa9e-233">toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="0fa9e-233">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

