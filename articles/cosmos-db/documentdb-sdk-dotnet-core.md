---
title: "aaaAzure Cosmos DB .NET Core API SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello .NET Core API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB .NET Core SDK verziói között."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1269cafe0ea1caaa871404d507b12632dbb3ed82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a><span data-ttu-id="318ff-103">Az Azure Cosmos DB .NET Core SDK: A kibocsátási megjegyzések és erőforrások</span><span class="sxs-lookup"><span data-stu-id="318ff-103">Azure Cosmos DB .NET Core SDK: Release notes and resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="318ff-104">.NET</span><span class="sxs-lookup"><span data-stu-id="318ff-104">.NET</span></span>](documentdb-sdk-dotnet.md)
> * [<span data-ttu-id="318ff-105">.NET-módosítás adatcsatorna</span><span class="sxs-lookup"><span data-stu-id="318ff-105">.NET Change Feed</span></span>](documentdb-sdk-dotnet-changefeed.md)
> * [<span data-ttu-id="318ff-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="318ff-106">.NET Core</span></span>](documentdb-sdk-dotnet-core.md)
> * [<span data-ttu-id="318ff-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="318ff-107">Node.js</span></span>](documentdb-sdk-node.md)
> * [<span data-ttu-id="318ff-108">Java</span><span class="sxs-lookup"><span data-stu-id="318ff-108">Java</span></span>](documentdb-sdk-java.md)
> * [<span data-ttu-id="318ff-109">Python</span><span class="sxs-lookup"><span data-stu-id="318ff-109">Python</span></span>](documentdb-sdk-python.md)
> * [<span data-ttu-id="318ff-110">REST</span><span class="sxs-lookup"><span data-stu-id="318ff-110">REST</span></span>](https://docs.microsoft.com/rest/api/documentdb/)
> * [<span data-ttu-id="318ff-111">REST erőforrás-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="318ff-111">REST Resource Provider</span></span>](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [<span data-ttu-id="318ff-112">SQL</span><span class="sxs-lookup"><span data-stu-id="318ff-112">SQL</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td><span data-ttu-id="318ff-113">**SDK letöltése**</span><span class="sxs-lookup"><span data-stu-id="318ff-113">**SDK download**</span></span></td><td>[<span data-ttu-id="318ff-114">NuGet</span><span class="sxs-lookup"><span data-stu-id="318ff-114">NuGet</span></span>](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td><span data-ttu-id="318ff-115">**API-JÁNAK dokumentációja**</span><span class="sxs-lookup"><span data-stu-id="318ff-115">**API documentation**</span></span></td><td>[<span data-ttu-id="318ff-116">.NET API-referenciadokumentáció</span><span class="sxs-lookup"><span data-stu-id="318ff-116">.NET API reference documentation</span></span>](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td><span data-ttu-id="318ff-117">**Példák**</span><span class="sxs-lookup"><span data-stu-id="318ff-117">**Samples**</span></span></td><td>[<span data-ttu-id="318ff-118">.NET-Kódminták</span><span class="sxs-lookup"><span data-stu-id="318ff-118">.NET code samples</span></span>](documentdb-dotnet-samples.md)</td></tr>

<tr><td><span data-ttu-id="318ff-119">**Első lépések**</span><span class="sxs-lookup"><span data-stu-id="318ff-119">**Get started**</span></span></td><td>[<span data-ttu-id="318ff-120">Ismerkedés az Azure Cosmos DB .NET Core SDK hello</span><span class="sxs-lookup"><span data-stu-id="318ff-120">Get started with hello Azure Cosmos DB .NET Core SDK</span></span>](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td><span data-ttu-id="318ff-121">**Webes alkalmazás oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="318ff-121">**Web app tutorial**</span></span></td><td>[<span data-ttu-id="318ff-122">Webalkalmazás fejlesztése a Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="318ff-122">Web application development with Azure Cosmos DB</span></span>](documentdb-dotnet-application.md)</td></tr>

<tr><td><span data-ttu-id="318ff-123">**Aktuális támogatott keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="318ff-123">**Current supported framework**</span></span></td><td>[<span data-ttu-id="318ff-124">.NET szabványos 1.6-os és .NET szabványos 1.5</span><span class="sxs-lookup"><span data-stu-id="318ff-124">.NET Standard 1.6 and .NET Standard 1.5</span></span>](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a><span data-ttu-id="318ff-125">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="318ff-125">Release Notes</span></span>

<span data-ttu-id="318ff-126">hello Azure Cosmos DB .NET Core SDK legújabb verziójával hello hello szolgáltatásparitást rendelkezik [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="318ff-126">hello Azure Cosmos DB .NET Core SDK has feature parity with hello latest version of hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="318ff-127">hello Azure Cosmos DB .NET Core SDK még nem kompatibilis az univerzális Windows Platform (UWP-) alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="318ff-127">hello Azure Cosmos DB .NET Core SDK is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="318ff-128">Ha érdekli hello .NET Core SDK-t támogató UWP-alkalmazások, e-mailek küldése túl[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="318ff-128">If you are interested in hello .NET Core SDK that does support UWP apps, send email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

### <a name="a-name150150"></a><span data-ttu-id="318ff-129"><a name="1.5.0"/>1.5.0</span><span class="sxs-lookup"><span data-stu-id="318ff-129"><a name="1.5.0"/>1.5.0</span></span> 

* <span data-ttu-id="318ff-130">Mint a lekérdezési eredmények tooa adott partíciótartomány kulcs-érték hatókörének egy FeedOption PartitionKeyRangeId támogatása.</span><span class="sxs-lookup"><span data-stu-id="318ff-130">Added support for PartitionKeyRangeId as a FeedOption for scoping query results tooa specific partition key range value.</span></span> 
* <span data-ttu-id="318ff-131">Egy ChangeFeedOption toostart időnek az elteltével hello módosítások keres, StartTime támogatása.</span><span class="sxs-lookup"><span data-stu-id="318ff-131">Added support for StartTime as a ChangeFeedOption toostart looking for hello changes after that time.</span></span> 

### <a name="a-name141141"></a><span data-ttu-id="318ff-132"><a name="1.4.1"/>1.4.1</span><span class="sxs-lookup"><span data-stu-id="318ff-132"><a name="1.4.1"/>1.4.1</span></span>

*   <span data-ttu-id="318ff-133">Rögzített hello JsonSerializable osztály, amely a verem túlcsordulás kivételt okozhat problémát.</span><span class="sxs-lookup"><span data-stu-id="318ff-133">Fixed an issue in hello JsonSerializable class that may cause a stack overflow exception.</span></span>

### <a name="a-name140140"></a><span data-ttu-id="318ff-134"><a name="1.4.0"/>1.4.0</span><span class="sxs-lookup"><span data-stu-id="318ff-134"><a name="1.4.0"/>1.4.0</span></span>

*   <span data-ttu-id="318ff-135">Egyéni JsonSerializerSettings megadása közben példányának támogatása egy [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) példány.</span><span class="sxs-lookup"><span data-stu-id="318ff-135">Added support for specifying custom JsonSerializerSettings while instantiating a [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.</span></span>

### <a name="a-name132132"></a><span data-ttu-id="318ff-136"><a name="1.3.2"/>1.3.2</span><span class="sxs-lookup"><span data-stu-id="318ff-136"><a name="1.3.2"/>1.3.2</span></span>

*   <span data-ttu-id="318ff-137">.NET-szabvány 1.5 támogató hello cél keretrendszerek egyik.</span><span class="sxs-lookup"><span data-stu-id="318ff-137">Supporting .NET Standard 1.5 as one of hello target frameworks.</span></span>

### <a name="a-name131131"></a><span data-ttu-id="318ff-138"><a name="1.3.1"/>1.3.1</span><span class="sxs-lookup"><span data-stu-id="318ff-138"><a name="1.3.1"/>1.3.1</span></span>

*   <span data-ttu-id="318ff-139">Rögzített x64 érintő problémát gépek, amelyek nem támogatják a SSE4 utasítást, és SEHException throw Azure Cosmos DB lekérdezések futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="318ff-139">Fixed an issue that affected x64 machines that don’t support SSE4 instruction and throw SEHException when running Azure Cosmos DB queries.</span></span>

### <a name="a-name130130"></a><span data-ttu-id="318ff-140"><a name="1.3.0"/>1.3.0</span><span class="sxs-lookup"><span data-stu-id="318ff-140"><a name="1.3.0"/>1.3.0</span></span>

*   <span data-ttu-id="318ff-141">Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.</span><span class="sxs-lookup"><span data-stu-id="318ff-141">Added support for a new consistency level called ConsistentPrefix.</span></span>
*   <span data-ttu-id="318ff-142">Az egyes partíciók lekérdezési metrikák támogatása.</span><span class="sxs-lookup"><span data-stu-id="318ff-142">Added support for query metrics for individual partitions.</span></span>
*   <span data-ttu-id="318ff-143">Támogatja a hello folytatási kód lekérdezések hello mérete korlátozza.</span><span class="sxs-lookup"><span data-stu-id="318ff-143">Added support for limiting hello size of hello continuation token for queries.</span></span>
*   <span data-ttu-id="318ff-144">Sikertelen kérelmek nyomkövetése részletesebb támogatása.</span><span class="sxs-lookup"><span data-stu-id="318ff-144">Added support for more detailed tracing for failed requests.</span></span>
*   <span data-ttu-id="318ff-145">Hello SDK végzett néhány teljesítménynövekedést.</span><span class="sxs-lookup"><span data-stu-id="318ff-145">Made some performance improvements in hello SDK.</span></span>

### <a name="a-name122122"></a><span data-ttu-id="318ff-146"><a name="1.2.2"/>1.2.2</span><span class="sxs-lookup"><span data-stu-id="318ff-146"><a name="1.2.2"/>1.2.2</span></span>

* <span data-ttu-id="318ff-147">Megtörtént egy probléma javítása, amelyik mellőzve hello PartitionKey értéket összesítő lekérdezések FeedOptions megadott.</span><span class="sxs-lookup"><span data-stu-id="318ff-147">Fixed an issue that ignored hello PartitionKey value provided in FeedOptions for aggregate queries.</span></span>
* <span data-ttu-id="318ff-148">Megtörtént egy probléma javítása átlátszó kezelésében, partíció felügyeleti közepes repülési kereszt-partíció Order By lekérdezés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="318ff-148">Fixed an issue in transparent handling of partition management during mid-flight cross-partition Order By query execution.</span></span>

### <a name="a-name121121"></a><span data-ttu-id="318ff-149"><a name="1.2.1"/>1.2.1</span><span class="sxs-lookup"><span data-stu-id="318ff-149"><a name="1.2.1"/>1.2.1</span></span>

* <span data-ttu-id="318ff-150">Rögzített egyes hello aszinkron API-k használatakor az ASP.NET környezet belül holtpont okozó problémát.</span><span class="sxs-lookup"><span data-stu-id="318ff-150">Fixed an issue which caused deadlocks in some of hello async APIs when used inside ASP.NET context.</span></span>

### <a name="a-name120120"></a><span data-ttu-id="318ff-151"><a name="1.2.0"/>1.2.0</span><span class="sxs-lookup"><span data-stu-id="318ff-151"><a name="1.2.0"/>1.2.0</span></span>

* <span data-ttu-id="318ff-152">Kijavítja a toomake SDK több rugalmas tooautomatic feladatátvételi bizonyos körülmények között.</span><span class="sxs-lookup"><span data-stu-id="318ff-152">Fixes toomake SDK more resilient tooautomatic failover under certain conditions.</span></span>

### <a name="a-name112112"></a><span data-ttu-id="318ff-153"><a name="1.1.2"/>1.1.2</span><span class="sxs-lookup"><span data-stu-id="318ff-153"><a name="1.1.2"/>1.1.2</span></span>

* <span data-ttu-id="318ff-154">Javítsa ki a WebException alkalmanként okozó hibát: hello távoli név nem sikerült feloldani.</span><span class="sxs-lookup"><span data-stu-id="318ff-154">Fix for an issue that occasionally causes a WebException: hello remote name could not be resolved.</span></span>
* <span data-ttu-id="318ff-155">Hozzáadott hello támogatása típusos dokumentumok közvetlenül olvasásakor új túlterhelések tooReadDocumentAsync API hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="318ff-155">Added hello support for directly reading a typed document by adding new overloads tooReadDocumentAsync API.</span></span>

### <a name="a-name111111"></a><span data-ttu-id="318ff-156"><a name="1.1.1"/>1.1.1</span><span class="sxs-lookup"><span data-stu-id="318ff-156"><a name="1.1.1"/>1.1.1</span></span>

* <span data-ttu-id="318ff-157">LINQ támogatása (COUNT, MIN, MAX, SUM és átlagos) összesítési lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="318ff-157">Added LINQ support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span>
* <span data-ttu-id="318ff-158">Javítsa ki a hello ConnectionPolicy objektum eseménykezelő hello használata okozza a problémát.</span><span class="sxs-lookup"><span data-stu-id="318ff-158">Fix for a memory leak issue for hello ConnectionPolicy object caused by hello use of event handler.</span></span>
* <span data-ttu-id="318ff-159">Javítsa ki a hibát, amelynek UpsertAttachmentAsync lett nem működik ETag használatakor.</span><span class="sxs-lookup"><span data-stu-id="318ff-159">Fix for an issue wherein UpsertAttachmentAsync was not working when ETag was used.</span></span>
* <span data-ttu-id="318ff-160">Javítsa ki a hibát, amelynek több partíciót az order by lekérdezés folytatási nem dolgozott rendezéskor karakterláncmező.</span><span class="sxs-lookup"><span data-stu-id="318ff-160">Fix for an issue wherein cross partition order-by query continuation was not working when sorting on string field.</span></span>

### <a name="a-name110110"></a><span data-ttu-id="318ff-161"><a name="1.1.0"/>1.1.0</span><span class="sxs-lookup"><span data-stu-id="318ff-161"><a name="1.1.0"/>1.1.0</span></span>

* <span data-ttu-id="318ff-162">Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása.</span><span class="sxs-lookup"><span data-stu-id="318ff-162">Added support for aggregation queries (COUNT, MIN, MAX, SUM, and AVG).</span></span> <span data-ttu-id="318ff-163">Lásd: [összesítési támogatási](documentdb-sql-query.md#Aggregates).</span><span class="sxs-lookup"><span data-stu-id="318ff-163">See [Aggregation support](documentdb-sql-query.md#Aggregates).</span></span>
* <span data-ttu-id="318ff-164">A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.</span><span class="sxs-lookup"><span data-stu-id="318ff-164">Lowered minimum throughput on partitioned collections from 10,100 RU/s too2500 RU/s.</span></span>

### <a name="a-name100100"></a><span data-ttu-id="318ff-165"><a name="1.0.0"/>1.0.0</span><span class="sxs-lookup"><span data-stu-id="318ff-165"><a name="1.0.0"/>1.0.0</span></span>

<span data-ttu-id="318ff-166">hello Azure Cosmos DB .NET Core SDK lehetővé teszi a toobuild gyors, platformfüggetlen [ASP.NET Core](https://www.asp.net/core) és [.NET Core](https://www.microsoft.com/net/core#windows) alkalmazások toorun Windows, Mac és Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="318ff-166">hello Azure Cosmos DB .NET Core SDK enables you toobuild fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps toorun on Windows, Mac, and Linux.</span></span> <span data-ttu-id="318ff-167">hello hello Azure Cosmos DB .NET Core SDK legújabb kiadása van teljesen [Xamarin](https://www.xamarin.com) kompatibilis és használt toobuild alkalmazások, amelyek monó (Linux), iOS és Android.</span><span class="sxs-lookup"><span data-stu-id="318ff-167">hello latest release of hello Azure Cosmos DB .NET Core SDK is fully [Xamarin](https://www.xamarin.com) compatible and be used toobuild applications that target iOS, Android, and Mono (Linux).</span></span>  

### <a name="a-name010-preview010-preview"></a><span data-ttu-id="318ff-168"><a name="0.1.0-preview"/>0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="318ff-168"><a name="0.1.0-preview"/>0.1.0-preview</span></span>

<span data-ttu-id="318ff-169">hello Azure Cosmos DB .NET Core Preview SDK lehetővé teszi a toobuild gyors, platformfüggetlen [ASP.NET Core](https://www.asp.net/core) és [.NET Core](https://www.microsoft.com/net/core#windows) alkalmazások toorun Windows, Mac és Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="318ff-169">hello Azure Cosmos DB .NET Core Preview SDK enables you toobuild fast, cross-platform [ASP.NET Core](https://www.asp.net/core) and [.NET Core](https://www.microsoft.com/net/core#windows) apps toorun on Windows, Mac, and Linux.</span></span>

<span data-ttu-id="318ff-170">hello Azure Cosmos DB .NET Core Preview SDK legújabb verziójával hello hello szolgáltatásparitást rendelkezik [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) és hello következőket támogatja:</span><span class="sxs-lookup"><span data-stu-id="318ff-170">hello Azure Cosmos DB .NET Core Preview SDK has feature parity with hello latest version of hello [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) and supports hello following:</span></span>
* <span data-ttu-id="318ff-171">Minden [csatlakozási mód](performance-tips.md#networking): átjáró módban, a közvetlen TCP és a közvetlen HTTPs.</span><span class="sxs-lookup"><span data-stu-id="318ff-171">All [connection modes](performance-tips.md#networking): Gateway mode, Direct TCP, and Direct HTTPs.</span></span> 
* <span data-ttu-id="318ff-172">Minden [konzisztenciaszintek](consistency-levels.md): erős, munkamenet, amelyet az elavulási és Eventual.</span><span class="sxs-lookup"><span data-stu-id="318ff-172">All [consistency levels](consistency-levels.md): Strong, Session, Bounded Staleness, and Eventual.</span></span>
* <span data-ttu-id="318ff-173">[A particionált gyűjtemények](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="318ff-173">[Partitioned collections](partition-data.md).</span></span> 
* <span data-ttu-id="318ff-174">[Több területi adatbázis fiókok és georeplikáció](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="318ff-174">[Multi-region database accounts and geo-replication](distribute-data-globally.md).</span></span>

<span data-ttu-id="318ff-175">Ha kérdése kapcsolódó toothis SDK, post túl[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), vagy probléma fájlt hello [github-tárházban](https://github.com/Azure/azure-documentdb-dotnet/issues).</span><span class="sxs-lookup"><span data-stu-id="318ff-175">If you have questions related toothis SDK, post too[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), or file an issue in hello [github repository](https://github.com/Azure/azure-documentdb-dotnet/issues).</span></span> 

## <a name="release--retirement-dates"></a><span data-ttu-id="318ff-176">Kiadás & használatból való kivonást dátumok</span><span class="sxs-lookup"><span data-stu-id="318ff-176">Release & Retirement Dates</span></span>

| <span data-ttu-id="318ff-177">Verzió</span><span class="sxs-lookup"><span data-stu-id="318ff-177">Version</span></span> | <span data-ttu-id="318ff-178">Kiadás dátuma</span><span class="sxs-lookup"><span data-stu-id="318ff-178">Release Date</span></span> | <span data-ttu-id="318ff-179">Kivezetési dátum</span><span class="sxs-lookup"><span data-stu-id="318ff-179">Retirement Date</span></span> |
| --- | --- | --- |
| [<span data-ttu-id="318ff-180">1.5.0</span><span class="sxs-lookup"><span data-stu-id="318ff-180">1.5.0</span></span>](#1.5.0) |<span data-ttu-id="318ff-181">2017. augusztus 10.</span><span class="sxs-lookup"><span data-stu-id="318ff-181">August 10, 2017</span></span> |--- | 
| [<span data-ttu-id="318ff-182">1.4.1</span><span class="sxs-lookup"><span data-stu-id="318ff-182">1.4.1</span></span>](#1.4.1) |<span data-ttu-id="318ff-183">2017. augusztus 07.</span><span class="sxs-lookup"><span data-stu-id="318ff-183">August 07, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-184">1.4.0</span><span class="sxs-lookup"><span data-stu-id="318ff-184">1.4.0</span></span>](#1.4.0) |<span data-ttu-id="318ff-185">2017. augusztus 02.</span><span class="sxs-lookup"><span data-stu-id="318ff-185">August 02, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-186">1.3.2</span><span class="sxs-lookup"><span data-stu-id="318ff-186">1.3.2</span></span>](#1.3.2) |<span data-ttu-id="318ff-187">2017. június 12.</span><span class="sxs-lookup"><span data-stu-id="318ff-187">June 12, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-188">1.3.1</span><span class="sxs-lookup"><span data-stu-id="318ff-188">1.3.1</span></span>](#1.3.1) |<span data-ttu-id="318ff-189">2017. május 23.</span><span class="sxs-lookup"><span data-stu-id="318ff-189">May 23, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-190">1.3.0</span><span class="sxs-lookup"><span data-stu-id="318ff-190">1.3.0</span></span>](#1.3.0) |<span data-ttu-id="318ff-191">2017. május 10.</span><span class="sxs-lookup"><span data-stu-id="318ff-191">May 10, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-192">1.2.2</span><span class="sxs-lookup"><span data-stu-id="318ff-192">1.2.2</span></span>](#1.2.2) |<span data-ttu-id="318ff-193">2017. április 19.</span><span class="sxs-lookup"><span data-stu-id="318ff-193">April 19, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-194">1.2.1</span><span class="sxs-lookup"><span data-stu-id="318ff-194">1.2.1</span></span>](#1.2.1) |<span data-ttu-id="318ff-195">2017. március 29.</span><span class="sxs-lookup"><span data-stu-id="318ff-195">March 29, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-196">1.2.0</span><span class="sxs-lookup"><span data-stu-id="318ff-196">1.2.0</span></span>](#1.2.0) |<span data-ttu-id="318ff-197">2017. március 25.</span><span class="sxs-lookup"><span data-stu-id="318ff-197">March 25, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-198">1.1.2</span><span class="sxs-lookup"><span data-stu-id="318ff-198">1.1.2</span></span>](#1.1.2) |<span data-ttu-id="318ff-199">2017. március 20.</span><span class="sxs-lookup"><span data-stu-id="318ff-199">March 20, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-200">1.1.1</span><span class="sxs-lookup"><span data-stu-id="318ff-200">1.1.1</span></span>](#1.1.1) |<span data-ttu-id="318ff-201">2017. március 14.</span><span class="sxs-lookup"><span data-stu-id="318ff-201">March 14, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-202">1.1.0</span><span class="sxs-lookup"><span data-stu-id="318ff-202">1.1.0</span></span>](#1.1.0) |<span data-ttu-id="318ff-203">2017. február 16.</span><span class="sxs-lookup"><span data-stu-id="318ff-203">February 16, 2017</span></span> |--- |
| [<span data-ttu-id="318ff-204">1.0.0</span><span class="sxs-lookup"><span data-stu-id="318ff-204">1.0.0</span></span>](#1.0.0) |<span data-ttu-id="318ff-205">2016. december 21.</span><span class="sxs-lookup"><span data-stu-id="318ff-205">December 21, 2016</span></span> |--- |
| [<span data-ttu-id="318ff-206">0.1.0-Preview</span><span class="sxs-lookup"><span data-stu-id="318ff-206">0.1.0-preview</span></span>](#0.1.0-preview) |<span data-ttu-id="318ff-207">2016. november 15.</span><span class="sxs-lookup"><span data-stu-id="318ff-207">November 15, 2016</span></span> |<span data-ttu-id="318ff-208">2016. december 31.</span><span class="sxs-lookup"><span data-stu-id="318ff-208">December 31, 2016</span></span> |

## <a name="see-also"></a><span data-ttu-id="318ff-209">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="318ff-209">See Also</span></span>
<span data-ttu-id="318ff-210">toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján.</span><span class="sxs-lookup"><span data-stu-id="318ff-210">toolearn more about Cosmos DB, see [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) service page.</span></span> 

