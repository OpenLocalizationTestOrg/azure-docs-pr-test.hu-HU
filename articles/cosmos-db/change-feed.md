---
title: "hello módosítását aaaWorking adatcsatorna-támogatás az Azure Cosmos Adatbázisba |} Microsoft Docs"
description: "Use Azure Cosmos adatbázis módosítása a dokumentumok adatcsatorna támogatási tootrack változásairól, és végezze el az esemény-alapú feldolgozási például eseményindítók és gyorsítótárak és elemzési rendszerek frissítése."
keywords: "Adatcsatorna módosítása"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: a4dcf4ceb476e3e08266dbcdcbee1d75e1d1eed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-hello-change-feed-support-in-azure-cosmos-db"></a><span data-ttu-id="6ef39-104">Hello módosítás adatcsatorna támogatást Azure Cosmos DB használata</span><span class="sxs-lookup"><span data-stu-id="6ef39-104">Working with hello change feed support in Azure Cosmos DB</span></span>
<span data-ttu-id="6ef39-105">[Az Azure Cosmos DB](../cosmos-db/introduction.md) gyors és rugalmas globálisan replikálni, amely nagy mennyiségű tranzakciós és működési adatok tárolására, előre jelezhető egyjegyű ezredmásodperces késést olvasása szolgál, és írja az adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ef39-105">[Azure Cosmos DB](../cosmos-db/introduction.md) is a fast and flexible globally replicated database service that is used for storing high-volume transactional and operational data with predictable single-digit millisecond latency for reads and writes.</span></span> <span data-ttu-id="6ef39-106">Így jól alkalmazható IoT, játékok, kiskereskedelmi, és működési naplózási alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="6ef39-106">This makes it well-suited for IoT, gaming, retail, and operational logging applications.</span></span> <span data-ttu-id="6ef39-107">Ezeket az alkalmazásokat egy közös tervezési minta tootrack módosításokat végzett tooAzure Cosmos DB adatokat, és materializált nézet frissítése, valós idejű elemzés, archivált adatok toocold adatok és eseményindító értesítések végre ezeket a módosításokat bizonyos eseményekről.</span><span class="sxs-lookup"><span data-stu-id="6ef39-107">A common design pattern in these applications is tootrack changes made tooAzure Cosmos DB data, and update materialized views, perform real-time analytics, archive data toocold storage, and trigger notifications on certain events based on these changes.</span></span> <span data-ttu-id="6ef39-108">Hello **módosítás hírcsatorna támogatási** az Azure Cosmos Adatbázisba lehetővé teszi toobuild méretezhető és hatékony megoldások az egyes ezeket a mintákat.</span><span class="sxs-lookup"><span data-stu-id="6ef39-108">hello **change feed support** in Azure Cosmos DB enables you toobuild efficient and scalable solutions for each of these patterns.</span></span>

<span data-ttu-id="6ef39-109">Támogatási hírcsatorna módosítását Azure Cosmos DB biztosít egy dokumentumot egy Azure Cosmos DB gyűjteményt, amelyben a módosítás hello sorrendben rendezett listát.</span><span class="sxs-lookup"><span data-stu-id="6ef39-109">With change feed support, Azure Cosmos DB provides a sorted list of documents within an Azure Cosmos DB collection in hello order in which they were modified.</span></span> <span data-ttu-id="6ef39-110">Ez az adatcsatorna is a módosítások toodata hello gyűjteményen belül használt toolisten és műveleteket, mint:</span><span class="sxs-lookup"><span data-stu-id="6ef39-110">This feed can be used toolisten for modifications toodata within hello collection and perform actions such as:</span></span>

* <span data-ttu-id="6ef39-111">Indul el egy hívás tooan API, ha egy dokumentum hozzáadásakor vagy módosításakor</span><span class="sxs-lookup"><span data-stu-id="6ef39-111">Trigger a call tooan API when a document is inserted or modified</span></span>
* <span data-ttu-id="6ef39-112">Valós idejű (stream) feldolgozási végre frissítések</span><span class="sxs-lookup"><span data-stu-id="6ef39-112">Perform real-time (stream) processing on updates</span></span>
* <span data-ttu-id="6ef39-113">Szinkronizálja az adatokat a gyorsítótár, a keresőmotor vagy a data warehouse-bA</span><span class="sxs-lookup"><span data-stu-id="6ef39-113">Synchronize data with a cache, search engine, or data warehouse</span></span>

<span data-ttu-id="6ef39-114">Azure Cosmos DB változásai megmaradnak és dolgozható fel aszinkron módon történik, és egy vagy több felhasználói párhuzamos feldolgozás pontjain.</span><span class="sxs-lookup"><span data-stu-id="6ef39-114">Changes in Azure Cosmos DB are persisted and can be processed asynchronously, and distributed across one or more consumers for parallel processing.</span></span> <span data-ttu-id="6ef39-115">A módosítás adatcsatorna és használatát őket toobuild méretezhető valós idejű alkalmazások API-k hello vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="6ef39-115">Let's look at hello APIs for change feed and how you can use them toobuild scalable real-time applications.</span></span> <span data-ttu-id="6ef39-116">Ez a cikk bemutatja, hogyan toowork Azure Cosmos DB az adatcsatorna és hello DocumentDB API módosítása.</span><span class="sxs-lookup"><span data-stu-id="6ef39-116">This article shows how toowork with Azure Cosmos DB change feed and hello DocumentDB API.</span></span> 

![Azure Cosmos DB módosítás használatával hírcsatorna toopower valós idejű elemzési és eseményvezérelt, számítási forgatókönyvek](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> <span data-ttu-id="6ef39-118">Támogatási hírcsatorna módosítása csak előírt hello DocumentDB API jelenleg; hello Graph API és a tábla API jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6ef39-118">Change feed support is only provided for hello DocumentDB API at this time; hello Graph API and Table API are not currently supported.</span></span>

## <a name="use-cases-and-scenarios"></a><span data-ttu-id="6ef39-119">Használati esetek és forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="6ef39-119">Use cases and scenarios</span></span>
<span data-ttu-id="6ef39-120">Módosítás adatcsatorna lehetővé teszi, hogy a nagy mennyiségű írási műveletek nagy adatkészletek hatékony feldolgozás, és ez biztosítja az alternatív tooquerying teljes adatkészletek tooidentify változásai.</span><span class="sxs-lookup"><span data-stu-id="6ef39-120">Change feed allows for efficient processing of large datasets with a high volume of writes, and offers an alternative tooquerying entire datasets tooidentify what has changed.</span></span> <span data-ttu-id="6ef39-121">Például hello hatékonyan a következő feladatokat végezheti el:</span><span class="sxs-lookup"><span data-stu-id="6ef39-121">For example, you can perform hello following tasks efficiently:</span></span>

* <span data-ttu-id="6ef39-122">Frissítse a gyorsítótárat, search-index vagy adatraktár Azure Cosmos DB-ben tárolt adatokkal.</span><span class="sxs-lookup"><span data-stu-id="6ef39-122">Update a cache, search index, or a data warehouse with data stored in Azure Cosmos DB.</span></span>
* <span data-ttu-id="6ef39-123">Alkalmazásszintű adatok rétegezési és archiválási alkalmazására, ez azt jelenti, hogy Azure Cosmos DB "gyakran használt adatokkal" tárolja, és elavulnak, "ritkán használt adatok" túl[Azure Blob Storage](../storage/common/storage-introduction.md) vagy [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ef39-123">Implement application-level data tiering and archival, that is, store "hot data" in Azure Cosmos DB, and age out "cold data" too[Azure Blob Storage](../storage/common/storage-introduction.md) or [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span></span>
* <span data-ttu-id="6ef39-124">Kötegelt elemzés megvalósítása az adatok használatával [Apache Hadoop](run-hadoop-with-hdinsight.md).</span><span class="sxs-lookup"><span data-stu-id="6ef39-124">Implement batch analytics on data using [Apache Hadoop](run-hadoop-with-hdinsight.md).</span></span>
* <span data-ttu-id="6ef39-125">Alkalmazzon [lambda folyamatok Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) rendelkező Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6ef39-125">Implement [lambda pipelines on Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) with Azure Cosmos DB.</span></span> <span data-ttu-id="6ef39-126">Azure Cosmos-adatbázis, amely kezeli az adatfeldolgozást és a lekérdezés, és az alacsony TCO lambda architektúrák megvalósítása adatbázis méretezhető megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="6ef39-126">Azure Cosmos DB provides a scalable database solution that can handle both ingestion and query, and implement lambda architectures with low TCO.</span></span> 
* <span data-ttu-id="6ef39-127">Hajtsa végre a nulla várakozási-ideje áttelepítések tooanother Azure Cosmos adatbázis fiók egy másik particionálási sémát.</span><span class="sxs-lookup"><span data-stu-id="6ef39-127">Perform zero down-time migrations tooanother Azure Cosmos DB account with a different partitioning scheme.</span></span>

<span data-ttu-id="6ef39-128">**Az Azure Cosmos DB adatfeldolgozást és lekérdezés lambda folyamatok:**</span><span class="sxs-lookup"><span data-stu-id="6ef39-128">**Lambda Pipelines with Azure Cosmos DB for ingestion and query:**</span></span>

![Azure-alapú Cosmos DB lambda folyamat adatfeldolgozást és lekérdezés](./media/change-feed/lambda.png)

<span data-ttu-id="6ef39-130">Azure Cosmos DB tooreceive használja és az eszközök, érzékelőket, infrastruktúra és az alkalmazások eseményadatok tárolására, és ezek az események feldolgozása valós időben [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [alatt futó Apache Storm](../hdinsight/hdinsight-storm-overview.md), vagy [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ef39-130">You can use Azure Cosmos DB tooreceive and store event data from devices, sensors, infrastructure, and applications, and process these events in real-time with [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), or [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span></span> 

<span data-ttu-id="6ef39-131">Belül a [kiszolgáló nélküli](http://azure.com/serverless) webes és mobilalkalmazások, például a változások tooyour felhasználói profilt, a beállítások vagy a hely tootrigger nyomon követheti az eseményeket bizonyos műveletek, például a leküldéses értesítések tootheir eszközök is[Az azure Functions](../azure-functions/functions-bindings-documentdb.md) vagy [alkalmazásszolgáltatások](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="6ef39-131">Within your [serverless](http://azure.com/serverless) web and mobile apps, you can track events such as changes tooyour customer's profile, preferences, or location tootrigger certain actions like sending push notifications tootheir devices using [Azure Functions](../azure-functions/functions-bindings-documentdb.md) or [App Services](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="6ef39-132">Azure Cosmos DB toobuild játék használata, például használhatja módosítás adatcsatorna tooimplement valós idejű ranglisták befejezett játékok származó eredmények alapján.</span><span class="sxs-lookup"><span data-stu-id="6ef39-132">If you're using Azure Cosmos DB toobuild a game, you can, for example, use change feed tooimplement real-time leaderboards based on scores from completed games.</span></span>

## <a name="how-change-feed-works-in-azure-cosmos-db"></a><span data-ttu-id="6ef39-133">Az Azure Cosmos Adatbázisba módosítás adatcsatorna működése</span><span class="sxs-lookup"><span data-stu-id="6ef39-133">How change feed works in Azure Cosmos DB</span></span>
<span data-ttu-id="6ef39-134">Azure Cosmos DB biztosít hello képességét tooincrementally végrehajtott frissítések tooan Azure Cosmos DB gyűjtemény olvasása.</span><span class="sxs-lookup"><span data-stu-id="6ef39-134">Azure Cosmos DB provides hello ability tooincrementally read updates made tooan Azure Cosmos DB collection.</span></span> <span data-ttu-id="6ef39-135">Ez a változás-hírcsatorna rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="6ef39-135">This change feed has hello following properties:</span></span>

* <span data-ttu-id="6ef39-136">Módosítások állandó az Azure Cosmos Adatbázisba, és aszinkron módon képes lehet feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="6ef39-136">Changes are persistent in Azure Cosmos DB and can be processed asynchronously.</span></span>
* <span data-ttu-id="6ef39-137">A gyűjteményen belül módosítások toodocuments azonnal hello módosítás adatcsatorna érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6ef39-137">Changes toodocuments within a collection are available immediately in hello change feed.</span></span>
* <span data-ttu-id="6ef39-138">Minden módosítás tooa dokumentum pontosan egyszer jelenik meg hello módosítás adatcsatorna, és ügyfelek kezelése az ellenőrzőpontok használata logikát.</span><span class="sxs-lookup"><span data-stu-id="6ef39-138">Each change tooa document appears exactly once in hello change feed, and clients manage their checkpointing logic.</span></span> <span data-ttu-id="6ef39-139">hello változáskezelési adatcsatorna processzor könyvtár biztosít automatikus ellenőrzőpont-készítés és a "legalább egyszeri" szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="6ef39-139">hello change feed processor library provides automatic checkpointing and "at least once" semantics.</span></span>
* <span data-ttu-id="6ef39-140">Hello változásnapló csak hello legutóbbi módosítása egy adott dokumentum tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ef39-140">Only hello most recent change for a given document is included in hello change log.</span></span> <span data-ttu-id="6ef39-141">Előfordulhat, hogy köztes módosítások nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="6ef39-141">Intermediate changes may not be available.</span></span>
* <span data-ttu-id="6ef39-142">hello módosítás adatcsatorna szolgáló belül minden partíciókulcs-értékkel módosítást van rendezve.</span><span class="sxs-lookup"><span data-stu-id="6ef39-142">hello change feed is sorted by order of modification within each partition key value.</span></span> <span data-ttu-id="6ef39-143">Nincs garantált rendelés partíciókulcs értékek között.</span><span class="sxs-lookup"><span data-stu-id="6ef39-143">There is no guaranteed order across partition-key values.</span></span>
* <span data-ttu-id="6ef39-144">Módosítások is szinkronizálhatja a bármely-időpontban, ez azt jelenti, hogy nincs nem rögzített adatmegőrzési időtartam, amelynek módosítások érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6ef39-144">Changes can be synchronized from any point-in-time, that is, there is no fixed data retention period for which changes are available.</span></span>
* <span data-ttu-id="6ef39-145">Változások a partíció kulcstartományokkal adattömböket írnak érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6ef39-145">Changes are available in chunks of partition key ranges.</span></span> <span data-ttu-id="6ef39-146">Ez a funkció lehetővé teszi, hogy a módosítások feldolgozása párhuzamosan több fogyasztók/kiszolgáló nagy gyűjteményekre toobe.</span><span class="sxs-lookup"><span data-stu-id="6ef39-146">This capability allows changes from large collections toobe processed in parallel by multiple consumers/servers.</span></span>
* <span data-ttu-id="6ef39-147">Alkalmazások kérhet több változás-hírcsatornák egyidejűleg hello ugyanaz a gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="6ef39-147">Applications can request for multiple change feeds simultaneously on hello same collection.</span></span>

<span data-ttu-id="6ef39-148">Az összes fiók alapértelmezés szerint engedélyezve van a Azure Cosmos-adatbázis módosítása adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="6ef39-148">Azure Cosmos DB's change feed is enabled by default for all accounts.</span></span> <span data-ttu-id="6ef39-149">Használhatja a [kiosztott átviteli sebesség](request-units.md) a írási régióban vagy bármely [olvassa el a régió](distribute-data-globally.md) a hello tooread módosítása adatcsatorna, csakúgy, mint bármilyen más műveletet a Azure Cosmos-Adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="6ef39-149">You can use your [provisioned throughput](request-units.md) in your write region or any [read region](distribute-data-globally.md) tooread from hello change feed, just like any other operation from Azure Cosmos DB.</span></span> <span data-ttu-id="6ef39-150">hello módosítás adatcsatorna beszúrások és a frissítési műveletek toodocuments hello gyűjteményben végzett tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ef39-150">hello change feed includes inserts and update operations made toodocuments within hello collection.</span></span> <span data-ttu-id="6ef39-151">Rögzítheti a törlések úgy, hogy a "soft-törlés" jelzőt belül a dokumentumok törlése helyett.</span><span class="sxs-lookup"><span data-stu-id="6ef39-151">You can capture deletes by setting a "soft-delete" flag within your documents in place of deletes.</span></span> <span data-ttu-id="6ef39-152">Azt is megteheti, beállíthat egy véges lejárati időt a dokumentumok keresztül hello [TTL funkció](time-to-live.md), például 24 óra és használata hello érték tulajdonság toocapture törli.</span><span class="sxs-lookup"><span data-stu-id="6ef39-152">Alternatively, you can set a finite expiration period for your documents via hello [TTL capability](time-to-live.md), for example, 24 hours and use hello value of that property toocapture deletes.</span></span> <span data-ttu-id="6ef39-153">Ebben a megoldásban ez az informatikai részleg tooprocess módosítások hello TTL lejárata időszaknál rövidebb időintervallumon belül.</span><span class="sxs-lookup"><span data-stu-id="6ef39-153">With this solution, you have tooprocess changes within a shorter time interval than hello TTL expiration period.</span></span> <span data-ttu-id="6ef39-154">hello módosítás adatcsatorna minden partíció kulcs tartományon belül hello dokumentumgyűjteményt érhető el, és így képes legyen elosztva párhuzamos feldolgozás egy vagy több fogyasztó számára.</span><span class="sxs-lookup"><span data-stu-id="6ef39-154">hello change feed is available for each partition key range within hello document collection, and thus can be distributed across one or more consumers for parallel processing.</span></span> 

![Elosztott feldolgozásához hírcsatorna Azure Cosmos DB módosítása](./media/change-feed/changefeedvisual.png)

<span data-ttu-id="6ef39-156">A változás az ügyfélkód a hírcsatorna megvalósításától néhány lehetőség áll.</span><span class="sxs-lookup"><span data-stu-id="6ef39-156">You have a few options in how you implement a change feed in your client code.</span></span> <span data-ttu-id="6ef39-157">hello részek, amelyek azonnali kövesse írják le, hogyan tooimplement hello módosítás adatcsatorna hello Azure Cosmos DB REST API használatával, és hello DocumentDB SDK-k.</span><span class="sxs-lookup"><span data-stu-id="6ef39-157">hello sections that immediately follow describe how tooimplement hello change feed using hello Azure Cosmos DB REST API and hello DocumentDB SDKs.</span></span> <span data-ttu-id="6ef39-158">Azonban a .NET-alkalmazásokban, azt javasoljuk, hello új [módosítás hírcsatorna processzor könyvtár](#change-feed-processor) az események feldolgozását a hello adatcsatorna módosítani, mert olvasási módosítások leegyszerűsíti a partíciók között, és lehetővé teszi, hogy működik-e a több szál párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="6ef39-158">However, for .NET applications, we recommend using hello new [Change feed processor library](#change-feed-processor) for processing events from hello change feed as it simplifies reading changes across partitions and enables multiple threads working in parallel.</span></span> 

## <span data-ttu-id="6ef39-159"><a id="rest-apis"></a>Hello REST API-t és a DocumentDB SDK-k használata</span><span class="sxs-lookup"><span data-stu-id="6ef39-159"><a id="rest-apis"></a>Working with hello REST API and DocumentDB SDKs</span></span>
<span data-ttu-id="6ef39-160">Azure Cosmos DB biztosít a tárolási és átviteli nevű rugalmas tárolók **gyűjtemények**.</span><span class="sxs-lookup"><span data-stu-id="6ef39-160">Azure Cosmos DB provides elastic containers of storage and throughput called **collections**.</span></span> <span data-ttu-id="6ef39-161">Gyűjtemények adatainak logikailag csoportosított használatával [kulcsok partícióazonosító](partition-data.md) a méretezhetőség és teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="6ef39-161">Data within collections is logically grouped using [partition keys](partition-data.md) for scalability and performance.</span></span> <span data-ttu-id="6ef39-162">Azure Cosmos-adatbázis különböző API felületeket biztosít ezeket az adatokat, beleértve a keresési azonosítója (olvasás/Get), a lekérdezés és a olvasás-hírcsatornák (vizsgálatok) eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6ef39-162">Azure Cosmos DB provides various APIs for accessing this data, including lookup by ID (Read/Get), query, and read-feeds (scans).</span></span> <span data-ttu-id="6ef39-163">hello módosítás adatcsatorna kérhetők le feltöltése két új kérelem fejlécek toohello DocumentDB `ReadDocumentFeed` API, és azokat a rendszer párhuzamos tartományok partíciós kulcsok között.</span><span class="sxs-lookup"><span data-stu-id="6ef39-163">hello change feed can be obtained by populating two new request headers toohello DocumentDB `ReadDocumentFeed` API, and can be processed in parallel across ranges of partition keys.</span></span>

### <a name="readdocumentfeed-api"></a><span data-ttu-id="6ef39-164">ReadDocumentFeed API</span><span class="sxs-lookup"><span data-stu-id="6ef39-164">ReadDocumentFeed API</span></span>
<span data-ttu-id="6ef39-165">Most egy pillantást rövid ReadDocumentFeed működése.</span><span class="sxs-lookup"><span data-stu-id="6ef39-165">Let's take a brief look at how ReadDocumentFeed works.</span></span> <span data-ttu-id="6ef39-166">Azure Cosmos DB támogatja a dokumentumok hello segítségével egy gyűjteményen belül hírcsatorna olvasása `ReadDocumentFeed` API.</span><span class="sxs-lookup"><span data-stu-id="6ef39-166">Azure Cosmos DB supports reading a feed of documents within a collection via hello `ReadDocumentFeed` API.</span></span> <span data-ttu-id="6ef39-167">Például hello következő kérelem adja vissza egy hello dokumentumok oldalán `serverlogs` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="6ef39-167">For example, hello following request returns a page of documents inside hello `serverlogs` collection.</span></span> 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="6ef39-168">Hello használatával eredmények lehet korlátozni `x-ms-max-item-count` fejlécet, és olvasási műveletek folytathatók hello kérelem Újraküldés egy `x-ms-continuation` hello előző válaszban visszaadott fejléc.</span><span class="sxs-lookup"><span data-stu-id="6ef39-168">Results can be limited by using hello `x-ms-max-item-count` header, and reads can be resumed by resubmitting hello request with a `x-ms-continuation` header returned in hello previous response.</span></span> <span data-ttu-id="6ef39-169">Az egyetlen ügyfél végrehajtásakor `ReadDocumentFeed` telepítéseket eredmények között partíciók Feladattervek.</span><span class="sxs-lookup"><span data-stu-id="6ef39-169">When performed from a single client, `ReadDocumentFeed` iterates through results across partitions serially.</span></span> 

<span data-ttu-id="6ef39-170">**Soros dokumentum hírcsatorna olvasása**</span><span class="sxs-lookup"><span data-stu-id="6ef39-170">**Serial read document feed**</span></span>

<span data-ttu-id="6ef39-171">Beolvasni a dokumentumok hello támogatott egyikével hello adatcsatorna [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="6ef39-171">You can also retrieve hello feed of documents using one of hello supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="6ef39-172">Például, hogy a következő kódrészletet mutat be hello toouse hello [ReadDocumentFeedAsync metódus](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) a .NET.</span><span class="sxs-lookup"><span data-stu-id="6ef39-172">For example, hello following snippet shows how toouse hello [ReadDocumentFeedAsync method](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) in .NET.</span></span>

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a><span data-ttu-id="6ef39-173">ReadDocumentFeed elosztott végrehajtása</span><span class="sxs-lookup"><span data-stu-id="6ef39-173">Distributed execution of ReadDocumentFeed</span></span>
<span data-ttu-id="6ef39-174">Az adatokat, vagy több terabájt tartalmazó, vagy frissítéseket nagy mennyiségű betöltési gyűjtemények soros végrehajtásának egyetlen ügyfélgépről olvasási adatcsatorna nem feltétlenül gyakorlati.</span><span class="sxs-lookup"><span data-stu-id="6ef39-174">For collections that contain terabytes of data or more, or ingest a large volume of updates, serial execution of read feed from a single client machine might not be practical.</span></span> <span data-ttu-id="6ef39-175">A sorrend toosupport a big Data típusú adatok Azure Cosmos DB forgatókönyvek API-k toodistribute `ReadDocumentFeed` hívások transzparens módon több ügyfél olvasók/fogyasztók között.</span><span class="sxs-lookup"><span data-stu-id="6ef39-175">In order toosupport these big data scenarios, Azure Cosmos DB provides APIs toodistribute `ReadDocumentFeed` calls transparently across multiple client readers/consumers.</span></span> 

<span data-ttu-id="6ef39-176">**Elosztott olvasási dokumentum adatcsatorna**</span><span class="sxs-lookup"><span data-stu-id="6ef39-176">**Distributed Read Document Feed**</span></span>

<span data-ttu-id="6ef39-177">méretezhető feldolgozása tooprovide Azure Cosmos DB a növekményes változásokat egy kibővített modell hello módosítás hírcsatorna partíciókulcsok-címtartományok alapján API támogatja.</span><span class="sxs-lookup"><span data-stu-id="6ef39-177">tooprovide scalable processing of incremental changes, Azure Cosmos DB supports a scale-out model for hello change feed API based on ranges of partition keys.</span></span>

* <span data-ttu-id="6ef39-178">Ezt úgy szerezheti be a partíció listáját egy gyűjtemény végrehajtásához kulcstartományokkal egy `ReadPartitionKeyRanges` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="6ef39-178">You can obtain a list of partition key ranges for a collection performing a `ReadPartitionKeyRanges` call.</span></span> 
* <span data-ttu-id="6ef39-179">Az egyes kulcs partíciótartomány, végezhet egy `ReadDocumentFeed` tooread dokumentumok partíciókulcsok adott tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="6ef39-179">For each partition key range, you can perform a `ReadDocumentFeed` tooread documents with partition keys within that range.</span></span>

### <a name="retrieving-partition-key-ranges-for-a-collection"></a><span data-ttu-id="6ef39-180">Egy gyűjtemény partíció kulcstartományokkal beolvasása</span><span class="sxs-lookup"><span data-stu-id="6ef39-180">Retrieving partition key ranges for a collection</span></span>
<span data-ttu-id="6ef39-181">Hello partíció kulcstartományokkal kérelmező hello olvashatók be `pkranges` erőforrás egy gyűjteményen belül.</span><span class="sxs-lookup"><span data-stu-id="6ef39-181">You can retrieve hello partition key ranges by requesting hello `pkranges` resource within a collection.</span></span> <span data-ttu-id="6ef39-182">Például hello következő kérelem hello listájának beolvasása partíció kulcstartományokkal hello `serverlogs` gyűjtemény:</span><span class="sxs-lookup"><span data-stu-id="6ef39-182">For example hello following request retrieves hello list of partition key ranges for hello `serverlogs` collection:</span></span>

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

<span data-ttu-id="6ef39-183">A kérelem a következő hello partíció kulcstartományokkal metaadatokat tartalmazó válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="6ef39-183">This request returns hello following response containing metadata about hello partition key ranges:</span></span>

    HTTP/1.1 200 Ok
    Content-Type: application/json
    x-ms-item-count: 25
    x-ms-schemaversion: 1.1
    Date: Tue, 15 Nov 2016 07:26:51 GMT

    {
       "_rid":"qYcAAPEvJBQ=",
       "PartitionKeyRanges":[
          {
             "_rid":"qYcAAPEvJBQCAAAAAAAAUA==",
             "id":"0",
             "_etag":"\"00002800-0000-0000-0000-580ac4ea0000\"",
             "minInclusive":"",
             "maxExclusive":"05C1CFFFFFFFF8",
             "_self":"dbs\/qYcAAA==\/colls\/qYcAAPEvJBQ=\/pkranges\/qYcAAPEvJBQCAAAAAAAAUA==\/",
             "_ts":1477100776
          },
          ...
       ],
       "_count": 25
    }


<span data-ttu-id="6ef39-184">**Kulcs tartomány tulajdonságai partícióazonosító**: minden kulcs partíciótartomány hello metaadat-tulajdonságainak hello a következő táblázat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6ef39-184">**Partition key range properties**: Each partition key range includes hello metadata properties in hello following table:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="6ef39-185">Fejléc neve</span><span class="sxs-lookup"><span data-stu-id="6ef39-185">Header name</span></span></th>
        <th><span data-ttu-id="6ef39-186">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ef39-186">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-187">id</span><span class="sxs-lookup"><span data-stu-id="6ef39-187">id</span></span></td>
        <td>
            <p><span data-ttu-id="6ef39-188">hello partíciós kulcs tartomány hello Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="6ef39-188">hello ID for hello partition key range.</span></span> <span data-ttu-id="6ef39-189">Ez az egyes gyűjteményeken belül egy stabil és egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6ef39-189">This is a stable and unique ID within each collection.</span></span></p>
            <p><span data-ttu-id="6ef39-190">Használata a következő hívást tooread módosítások hello partíciótartomány fő szerint.</span><span class="sxs-lookup"><span data-stu-id="6ef39-190">Must be used in hello following call tooread changes by partition key range.</span></span></p>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-191">maxExclusive</span><span class="sxs-lookup"><span data-stu-id="6ef39-191">maxExclusive</span></span></td>
        <td><span data-ttu-id="6ef39-192">hello maximális partíciós kulcs-kivonatot hello kulcs partíciótartomány.</span><span class="sxs-lookup"><span data-stu-id="6ef39-192">hello maximum partition key hash value for hello partition key range.</span></span> <span data-ttu-id="6ef39-193">Belső használatra.</span><span class="sxs-lookup"><span data-stu-id="6ef39-193">For internal use.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-194">minInclusive</span><span class="sxs-lookup"><span data-stu-id="6ef39-194">minInclusive</span></span></td>
        <td><span data-ttu-id="6ef39-195">hello minimális partíciós kulcs-kivonatot hello kulcs partíciótartomány.</span><span class="sxs-lookup"><span data-stu-id="6ef39-195">hello minimum partition key hash value for hello partition key range.</span></span> <span data-ttu-id="6ef39-196">Belső használatra.</span><span class="sxs-lookup"><span data-stu-id="6ef39-196">For internal use.</span></span></td>
    </tr>       
</table>

<span data-ttu-id="6ef39-197">Ehhez hello támogatott egyikével [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="6ef39-197">You can do this using one of hello supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="6ef39-198">Például hello alábbi kódrészletben láthatja, hogyan tooretrieve partíciókulcs tartományok .NET használatával hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metódust.</span><span class="sxs-lookup"><span data-stu-id="6ef39-198">For example, hello following snippet shows how tooretrieve partition key ranges in .NET using hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) method.</span></span>

```csharp
string pkRangesResponseContinuation = null;
List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

do
{
    FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri, 
        new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
}
while (pkRangesResponseContinuation != null);
```

<span data-ttu-id="6ef39-199">Azure Cosmos-adatbázis beolvasása a partíciós kulcs tartomány / dokumentumok támogatja választható beállítás hello `x-ms-documentdb-partitionkeyrangeid` fejléc.</span><span class="sxs-lookup"><span data-stu-id="6ef39-199">Azure Cosmos DB supports retrieval of documents per partition key range by setting hello optional `x-ms-documentdb-partitionkeyrangeid` header.</span></span> 

### <a name="performing-an-incremental-readdocumentfeed"></a><span data-ttu-id="6ef39-200">Egy növekményes ReadDocumentFeed végrehajtása</span><span class="sxs-lookup"><span data-stu-id="6ef39-200">Performing an incremental ReadDocumentFeed</span></span>
<span data-ttu-id="6ef39-201">ReadDocumentFeed forgatókönyvek/feladatok a növekményes feldolgozás Azure Cosmos DB gyűjtemények a változásokkal a következő hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="6ef39-201">ReadDocumentFeed supports hello following scenarios/tasks for incremental processing of changes in Azure Cosmos DB collections:</span></span>

* <span data-ttu-id="6ef39-202">Olvassa el az összes toodocuments hello elejétől a webhelycsoport létrehozása Ez azt jelenti, hogy változik.</span><span class="sxs-lookup"><span data-stu-id="6ef39-202">Read all changes toodocuments from hello beginning, that is, from collection creation.</span></span>
* <span data-ttu-id="6ef39-203">Olvassa el az összes módosítja toofuture frissítések toodocuments aktuális idő, illetve a felhasználó által megadott idő óta.</span><span class="sxs-lookup"><span data-stu-id="6ef39-203">Read all changes toofuture updates toodocuments from current time, or any changes since a user-specified time.</span></span>
* <span data-ttu-id="6ef39-204">Olvassa el az összes módosítások toodocuments hello gyűjtemény (ETag) logikai verziójából származó.</span><span class="sxs-lookup"><span data-stu-id="6ef39-204">Read all changes toodocuments from a logical version of hello collection (ETag).</span></span> <span data-ttu-id="6ef39-205">A felhasználók alapján ETag növekményes olvasás-hírcsatorna kérelem által visszaadott hello ellenőrzőpont is.</span><span class="sxs-lookup"><span data-stu-id="6ef39-205">You can checkpoint your consumers based on hello returned ETag from incremental read-feed requests.</span></span>

<span data-ttu-id="6ef39-206">hello változtatások közé tartozik a beszúrások, és a frissítések toodocuments.</span><span class="sxs-lookup"><span data-stu-id="6ef39-206">hello changes include inserts and updates toodocuments.</span></span> <span data-ttu-id="6ef39-207">toocapture törli, kell egy "helyreállítható törlésre" tulajdonsággal belül a dokumentumokhoz, vagy használjon hello [beépített TTL tulajdonsága](time-to-live.md) toosignal a függőben lévő törlés hello a hírcsatorna módosítása.</span><span class="sxs-lookup"><span data-stu-id="6ef39-207">toocapture deletes, you must use a "soft delete" property within your documents, or use hello [built-in TTL property](time-to-live.md) toosignal a pending deletion in hello change feed.</span></span>

<span data-ttu-id="6ef39-208">a következő tábla listák hello hello [kérelem](/rest/api/documentdb/common-documentdb-rest-request-headers.md) és [válaszfejlécek](/rest/api/documentdb/common-documentdb-rest-response-headers.md) ReadDocumentFeed műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="6ef39-208">hello following table lists hello [request](/rest/api/documentdb/common-documentdb-rest-request-headers.md) and [response headers](/rest/api/documentdb/common-documentdb-rest-response-headers.md) for ReadDocumentFeed operations.</span></span>

<span data-ttu-id="6ef39-209">**Kérelem fejlécei a növekményes ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="6ef39-209">**Request headers for incremental ReadDocumentFeed**:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="6ef39-210">Fejléc neve</span><span class="sxs-lookup"><span data-stu-id="6ef39-210">Header name</span></span></th>
        <th><span data-ttu-id="6ef39-211">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ef39-211">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-212">A-IM</span><span class="sxs-lookup"><span data-stu-id="6ef39-212">A-IM</span></span></td>
        <td><span data-ttu-id="6ef39-213">Be kell állítani túl "Növekményes adatcsatorna", vagy ellenkező esetben nincs megadva</span><span class="sxs-lookup"><span data-stu-id="6ef39-213">Must be set too"Incremental feed", or omitted otherwise</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-214">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="6ef39-214">If-None-Match</span></span></td>
        <td>
            <p><span data-ttu-id="6ef39-215">Nincs fejléc: összes módosítás vissza hello kezdve (gyűjtemény létrehozása)</span><span class="sxs-lookup"><span data-stu-id="6ef39-215">No header: returns all changes from hello beginning (collection creation)</span></span></p>
            <p><span data-ttu-id="6ef39-216">"*": minden új módosítások toodata hello gyűjteményen belül adja vissza</span><span class="sxs-lookup"><span data-stu-id="6ef39-216">"*": returns all new changes toodata within hello collection</span></span></p>         
            <p><span data-ttu-id="6ef39-217">&lt;ETag&gt;: Ha tooa gyűjtemény ETag megadásához adja vissza az adott logikai időbélyeg óta végrehajtott valamennyi módosítást</span><span class="sxs-lookup"><span data-stu-id="6ef39-217">&lt;etag&gt;: If set tooa collection ETag, returns all changes made since that logical timestamp</span></span></p>
        </td>
    </tr>
    <tr>    
        <td><span data-ttu-id="6ef39-218">If-Modified-Since</span><span class="sxs-lookup"><span data-stu-id="6ef39-218">If-Modified-Since</span></span></td> 
        <td><span data-ttu-id="6ef39-219">RFC 1123 időformátum; Ha meg van adva If-None-Match figyelmen kívül hagyva</span><span class="sxs-lookup"><span data-stu-id="6ef39-219">RFC 1123 time format; ignored if If-None-Match is specified</span></span></td> 
    </tr> 
    <tr>
        <td><span data-ttu-id="6ef39-220">x-ms-documentdb-partitionkeyrangeid</span><span class="sxs-lookup"><span data-stu-id="6ef39-220">x-ms-documentdb-partitionkeyrangeid</span></span></td>
        <td><span data-ttu-id="6ef39-221">hello partíciós kulcs tartomány azonosítója adatainak olvasása.</span><span class="sxs-lookup"><span data-stu-id="6ef39-221">hello partition key range ID for reading data.</span></span></td>
    </tr>
</table>

<span data-ttu-id="6ef39-222">**A növekményes ReadDocumentFeed válaszfejlécek**:</span><span class="sxs-lookup"><span data-stu-id="6ef39-222">**Response headers for incremental ReadDocumentFeed**:</span></span>

<table> <tr>
        <th><span data-ttu-id="6ef39-223">Fejléc neve</span><span class="sxs-lookup"><span data-stu-id="6ef39-223">Header name</span></span></th>
        <th><span data-ttu-id="6ef39-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ef39-224">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-225">ETag</span><span class="sxs-lookup"><span data-stu-id="6ef39-225">etag</span></span></td>
        <td>
            <p><span data-ttu-id="6ef39-226">hello logikai sorszáma (LSN) hello válaszként visszaadott utolsó dokumentum.</span><span class="sxs-lookup"><span data-stu-id="6ef39-226">hello logical sequence number (LSN) of last document returned in hello response.</span></span></p>
            <p><span data-ttu-id="6ef39-227">Ez az érték If-None-Match Újraküldés növekményes ReadDocumentFeed folytathatók.</span><span class="sxs-lookup"><span data-stu-id="6ef39-227">Incremental ReadDocumentFeed can be resumed by resubmitting this value in If-None-Match.</span></span></p>
        </td>
    </tr>
</table>

<span data-ttu-id="6ef39-228">Íme egy minta kérelem tooreturn hello logikai verzió/ETag gyűjteményben lévő összes növekményes változásokat `28535` és legfontosabb tartomány partícióazonosító = `16`:</span><span class="sxs-lookup"><span data-stu-id="6ef39-228">Here's a sample request tooreturn all incremental changes in collection from hello logical version/ETag `28535` and partition key range = `16`:</span></span>

    GET https://mydocumentdb.documents.azure.com/dbs/bigdb/colls/bigcoll/docs HTTP/1.1
    x-ms-max-item-count: 1
    If-None-Match: "28535"
    A-IM: Incremental feed
    x-ms-documentdb-partitionkeyrangeid: 16
    x-ms-date: Tue, 22 Nov 2016 20:43:01 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dzdpL2QQ8TCfiNbW%2fEcT88JHNvWeCgDA8gWeRZ%2btfN5o%3d
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="6ef39-229">Módosítások belül minden partíciós kulcs értéke hello partíciós kulcs tartományon belüli idő szerint rendezett.</span><span class="sxs-lookup"><span data-stu-id="6ef39-229">Changes are ordered by time within each partition key value within hello partition key range.</span></span> <span data-ttu-id="6ef39-230">Nincs garantált rendelés partíciókulcs értékek között.</span><span class="sxs-lookup"><span data-stu-id="6ef39-230">There is no guaranteed order across partition-key values.</span></span> <span data-ttu-id="6ef39-231">Ha nincsenek további eredmények fér is egyetlen lap, hello következő oldalra, olvashatják hello hello kérelem Újraküldés `If-None-Match` fejléc a következő érték egyenlő toohello `etag` a hello előző válaszban.</span><span class="sxs-lookup"><span data-stu-id="6ef39-231">If there are more results than can fit in a single page, you can read hello next page of results by resubmitting hello request with hello `If-None-Match` header with value equal toohello `etag` from hello previous response.</span></span> <span data-ttu-id="6ef39-232">Ha több dokumentumok beszúrni vagy tárolt eljárás vagy eseményindító belül tranzakciós úton frissített, azok összes visszaadott hello belül azonos válasz lap.</span><span class="sxs-lookup"><span data-stu-id="6ef39-232">If multiple documents were inserted or updated transactionally within a stored procedure or trigger, they will all be returned within hello same response page.</span></span>

> [!NOTE]
> <span data-ttu-id="6ef39-233">Változás hírcsatornával kaphat több elemet adott vissza a lapon, mint a megadott `x-ms-max-item-count` hello esetben több dokumentumok beszúrt vagy frissített belül a tárolt eljárások és eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="6ef39-233">With change feed, you might get more items returned in a page than specified in `x-ms-max-item-count` in hello case of multiple documents inserted or updated inside a stored procedures or triggers.</span></span> 

<span data-ttu-id="6ef39-234">Hello .NET SDK-val (1.17.0) használata esetén állítsa be a hello mezőt `StartTime` a `ChangeFeedOptions` óta megváltozott visszatérési toodirectly dokumentumok `StartTime` meghívásakor `CreateDocumentChangeFeedQuery`.</span><span class="sxs-lookup"><span data-stu-id="6ef39-234">When using hello .NET SDK (1.17.0), set hello field `StartTime` in `ChangeFeedOptions` toodirectly return changed documents since `StartTime` when calling  `CreateDocumentChangeFeedQuery`.</span></span> <span data-ttu-id="6ef39-235">Megadásával `If-Modified-Since` hello REST API-t használ, a kérelem visszaadható nem hello dokumentumok magukat, de ahelyett, hogy a hello folytatási kód vagy `etag` a hello válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="6ef39-235">By specifying `If-Modified-Since` using hello REST API, your request will return not hello documents themselves, but rather hello continuation token or `etag` in hello response header.</span></span> <span data-ttu-id="6ef39-236">módosított hello megadott idő, hello folytatási kód tooreturn hello dokumentumok `etag` kell használni a következő kérelmet hello `If-None-Match` tooreturn hello tényleges dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="6ef39-236">tooreturn hello documents modified hello specified time, hello continuation token `etag` must then be used in hello next request with `If-None-Match` tooreturn hello actual documents.</span></span> 

<span data-ttu-id="6ef39-237">hello .NET SDK biztosít hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) és [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) segítő osztályok tooaccess módosítások tooa gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="6ef39-237">hello .NET SDK provides hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) and [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper classes tooaccess changes made tooa collection.</span></span> <span data-ttu-id="6ef39-238">hello alábbi kódrészletben láthatja, hogyan tooretrieve minden módosítja az egyetlen ügyfél .NET SDK hello segítségével hello elejétől.</span><span class="sxs-lookup"><span data-stu-id="6ef39-238">hello following snippet shows how tooretrieve all changes from hello beginning using hello .NET SDK from a single client.</span></span>

```csharp
private async Task<Dictionary<string, string>> GetChanges(
    DocumentClient client,
    string collection,
    Dictionary<string, string> checkpoints)
{
    string pkRangesResponseContinuation = null;
    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

    do
    {
        FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
            collectionUri, 
            new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

        partitionKeyRanges.AddRange(pkRangesResponse);
        pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    }
    while (pkRangesResponseContinuation != null);

    foreach (PartitionKeyRange pkRange in partitionKeyRanges)
    {
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);

        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collection,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = 1
            });

        while (query.HasMoreResults)
        {
            FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

            foreach (DeviceReading changedDocument in readChangesResponse)
            {
                Console.WriteLine(changedDocument.Id);
            }

            checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
        }
    }

    return checkpoints;
}
```
<span data-ttu-id="6ef39-239">Hello alábbi kódrészletben láthatja, hogyan tooprocess valós időben változik, és az Azure Cosmos DB hello módosítása a hírcsatorna a támogatási és hello megelőző függvény.</span><span class="sxs-lookup"><span data-stu-id="6ef39-239">And hello following snippet shows how tooprocess changes in real-time with Azure Cosmos DB by using hello change feed support and hello preceding function.</span></span> <span data-ttu-id="6ef39-240">hello első hívás összes hello dokumentumok hello gyűjteményben, és a hello második csak beolvasása hello létrehozott hello utolsó ellenőrzőpont óta létrehozott két dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="6ef39-240">hello first call returns all hello documents in hello collection, and hello second only returns hello two documents created that were created since hello last checkpoint.</span></span>

```csharp
// Returns all documents in hello collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only hello two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

<span data-ttu-id="6ef39-241">Hello módosítás adatcsatorna ügyfél oldalán logika tooselectively eseményeinek használatával is végezhet.</span><span class="sxs-lookup"><span data-stu-id="6ef39-241">You can also filter hello change feed using client side logic tooselectively process events.</span></span> <span data-ttu-id="6ef39-242">Például ez egy csak hőmérséklet események módosíthatja az eszköz érzékelők ügyfél oldalán LINQ tooprocess használó részlet.</span><span class="sxs-lookup"><span data-stu-id="6ef39-242">For example, here's a snippet that uses client side LINQ tooprocess only temperature change events from device sensors.</span></span>

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <span data-ttu-id="6ef39-243"><a id="change-feed-processor"></a>Változáskezelési hírcsatorna processzor könyvtár</span><span class="sxs-lookup"><span data-stu-id="6ef39-243"><a id="change-feed-processor"></a>Change Feed Processor library</span></span>
<span data-ttu-id="6ef39-244">Másik lehetőség is toouse hello [Azure Cosmos DB módosítása hírcsatorna processzor könyvtár](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), ez elősegítheti a könnyen eloszthatók több felhasználóból közötti adatcsatorna származó Eseményfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="6ef39-244">Another option is toouse hello [Azure Cosmos DB Change Feed Processor library](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), which can help you easily distribute event processing from a change feed across multiple consumers.</span></span> <span data-ttu-id="6ef39-245">hello könyvtár kiváló adatcsatorna-olvasók hello .NET platformon módosítás készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6ef39-245">hello library is great for building change feed readers on hello .NET platform.</span></span> <span data-ttu-id="6ef39-246">Hello módosítás hírcsatorna processzor könyvtár használatával hello szereplő hello módszerek keresztül kellene egyszerűsíteni néhány munkafolyamatok más Cosmos DB SDK tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6ef39-246">Some workflows that would be simplified by using hello Change Feed Processor library over hello methods included in hello other Cosmos DB SDKs include:</span></span> 

* <span data-ttu-id="6ef39-247">Adatlekérő frissítéseket, a módosítás hírcsatornát, amikor több partíciót az adatok tárolásához</span><span class="sxs-lookup"><span data-stu-id="6ef39-247">Pulling updates from change feed when data is stored across multiple partitions</span></span>
* <span data-ttu-id="6ef39-248">Áthelyezése vagy az adatok replikálásához több gyűjtemény tooanother</span><span class="sxs-lookup"><span data-stu-id="6ef39-248">Moving or replicating data from one collection tooanother</span></span>
* <span data-ttu-id="6ef39-249">Az adatcsatorna-frissítések toodata által indított műveleteket és párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="6ef39-249">Parallel execution of actions triggered by updates toodata and change feed</span></span> 

<span data-ttu-id="6ef39-250">Hello API-k a hello Cosmos SDK-k használatának pontos hozzáférés toochange frissítések hírcsatorna minden partíció, amíg partíciók és párhuzamosan működik több szál hello módosítás hírcsatorna processzor szalagtár használata egyszerűsíti olvasási módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6ef39-250">While using hello APIs in hello Cosmos SDKs provides precise access toochange feed updates in each partition, using hello Change Feed Processor library simplifies reading changes across partitions and multiple threads working in parallel.</span></span> <span data-ttu-id="6ef39-251">Helyett a manuális módosítások olvasásakor egyes tárolókban, és mentése a folytatási kód minden partíció esetében hello módosítás hírcsatorna processzor automatikusan kezeli a olvasási módosításokat egy bérleti mechanizmussal partíciók között.</span><span class="sxs-lookup"><span data-stu-id="6ef39-251">Instead of manually reading changes from each container and saving a continuation token for each partition, hello Change Feed Processor automatically manages reading changes across partitions using a lease mechanism.</span></span>

<span data-ttu-id="6ef39-252">hello library NuGet-csomagként érhető el: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) és a forráskódot, mint a Githubon [minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span><span class="sxs-lookup"><span data-stu-id="6ef39-252">hello library is available as a NuGet Package: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) and from source code as a Github [sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span></span> 

### <a name="understanding-change-feed-processor-library"></a><span data-ttu-id="6ef39-253">Változás hírcsatorna processzor könyvtár ismertetése</span><span class="sxs-lookup"><span data-stu-id="6ef39-253">Understanding Change Feed Processor library</span></span> 

<span data-ttu-id="6ef39-254">Négy fő összetevőből végrehajtási hello módosítás adatcsatornához processzor: hello figyelt gyűjtemény, hello bérleti gyűjtemény, hello processzor állomás és hello fogyasztók.</span><span class="sxs-lookup"><span data-stu-id="6ef39-254">There are four main components of implementing hello Change Feed Processor: hello monitored collection, hello lease collection, hello processor host, and hello consumers.</span></span> 

<span data-ttu-id="6ef39-255">**Figyelt gyűjtemény:** hello figyelt gyűjtemény mely hello módosítás adatcsatorna képzésére hello adatokat jelzi.</span><span class="sxs-lookup"><span data-stu-id="6ef39-255">**Monitored Collection:** hello monitored collection is hello data from which hello change feed is generated.</span></span> <span data-ttu-id="6ef39-256">Bármilyen beszúrások és a módosítások toohello figyelt gyűjtemény hello módosítás adatcsatorna hello gyűjtemény jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6ef39-256">Any inserts and changes toohello monitored collection are reflected in hello change feed of hello collection.</span></span> 

<span data-ttu-id="6ef39-257">**Címbérlet gyűjtemény:** hello bérleti gyűjtemény koordináták feldolgozása között több Worker hírcsatorna hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="6ef39-257">**Lease Collection:** hello lease collection coordinates processing hello change feed across multiple workers.</span></span> <span data-ttu-id="6ef39-258">Egy külön gyűjteménye használt toostore hello címbérleteket partíciónként több bérletet eredményez.</span><span class="sxs-lookup"><span data-stu-id="6ef39-258">A separate collection is used toostore hello leases with one lease per partition.</span></span> <span data-ttu-id="6ef39-259">A bérleti gyűjtemény hello egy másik fiókot a írási régió módosítása hírcsatorna processzor szorosabb toowhere hello előnyös toostore.</span><span class="sxs-lookup"><span data-stu-id="6ef39-259">It is advantageous toostore this lease collection on a different account with hello write region closer toowhere hello Change Feed Processor is running.</span></span> <span data-ttu-id="6ef39-260">A bérleti objektum tartalmazza a következő attribútumok hello:</span><span class="sxs-lookup"><span data-stu-id="6ef39-260">A lease object contains hello following attributes:</span></span> 
* <span data-ttu-id="6ef39-261">Tulajdonos: Hello bérleti birtokló hello állomás határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6ef39-261">Owner: Specifies hello host that owns hello lease</span></span>
* <span data-ttu-id="6ef39-262">Folytatási: Hello pozíciója (folytatási kód) adja meg egy adott partíció-hírcsatorna hello módosítása</span><span class="sxs-lookup"><span data-stu-id="6ef39-262">Continuation: Specifies hello position (continuation token) in hello change feed for a particular partition</span></span>
* <span data-ttu-id="6ef39-263">Időbélyeg: Legutóbbi bérleti frissült; hello időbélyeg használt toocheck lehet, hogy hello bérleti tekinthető lejárt</span><span class="sxs-lookup"><span data-stu-id="6ef39-263">Timestamp: Last time lease was updated; hello timestamp can be used toocheck whether hello lease is considered expired</span></span> 

<span data-ttu-id="6ef39-264">**Processzor-állomás:** minden állomás határozza meg, hány partíciók tooprocess alapján más gazdagépek hány példányban vannak-e aktív bérleteket.</span><span class="sxs-lookup"><span data-stu-id="6ef39-264">**Processor Host:** Each host determines how many partitions tooprocess based on how many other instances of hosts have active leases.</span></span> 
1.  <span data-ttu-id="6ef39-265">A gazdagép indításakor címbérleteket toobalance hello munkaterhelés megvásárolja minden állomáson.</span><span class="sxs-lookup"><span data-stu-id="6ef39-265">When a host starts up, it acquires leases toobalance hello workload across all hosts.</span></span> <span data-ttu-id="6ef39-266">Egy gazdagép rendszeres időközönként megújítja címbérleteket,, címbérleteket aktív marad.</span><span class="sxs-lookup"><span data-stu-id="6ef39-266">A host periodically renews leases, so leases remain active.</span></span> 
2.  <span data-ttu-id="6ef39-267">A gazdagép ellenőrzőpontokat hello utolsó folytatási token tooits címbérlet minden egyes.</span><span class="sxs-lookup"><span data-stu-id="6ef39-267">A host checkpoints hello last continuation token tooits lease for each read.</span></span> <span data-ttu-id="6ef39-268">tooensure egyidejű biztonsági, a gazdagép hello ETag minden egyes bérleti frissítés ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="6ef39-268">tooensure concurrency safety, a host checks hello ETag for each lease update.</span></span> <span data-ttu-id="6ef39-269">Más ellenőrzőpont stratégiák használata is támogatott.</span><span class="sxs-lookup"><span data-stu-id="6ef39-269">Other checkpoint strategies are also supported.</span></span>  
3.  <span data-ttu-id="6ef39-270">Leállásakor állomás kiadott összes címbérlet, de a memóriában tárolja hello folytatási információkat, így folytathatja a olvasási hello tárolt ellenőrzőponttól később.</span><span class="sxs-lookup"><span data-stu-id="6ef39-270">Upon shutdown, a host releases all leases but keeps hello continuation information, so it can resume reading from hello stored checkpoint later.</span></span> 

<span data-ttu-id="6ef39-271">Jelenleg a gazdagépek hello száma nem lehet nagyobb hello számú partíciót (címbérleteket).</span><span class="sxs-lookup"><span data-stu-id="6ef39-271">At this time hello number of hosts cannot be greater than hello number of partitions (leases).</span></span>

<span data-ttu-id="6ef39-272">**A fogyasztók:** fogyasztók, vagy a dolgozók, hajtsa végre az egyes állomások által kezdeményezett feldolgozási hírcsatorna hello módosítás szálak.</span><span class="sxs-lookup"><span data-stu-id="6ef39-272">**Consumers:** Consumers, or workers, are threads that perform hello change feed processing initiated by each host.</span></span> <span data-ttu-id="6ef39-273">Mindegyik feldolgozó gazdagépnek több felhasználóból lehet.</span><span class="sxs-lookup"><span data-stu-id="6ef39-273">Each processor host can have multiple consumers.</span></span> <span data-ttu-id="6ef39-274">Mindegyik felhasználó beolvassa hello módosítás adatcsatorna hello partíció tooand van hozzárendelve a változások gazdagépe értesíti és címbérleteket lejárt.</span><span class="sxs-lookup"><span data-stu-id="6ef39-274">Each consumer reads hello change feed from hello partition it is assigned tooand notifies its host of changes and expired leases.</span></span>

<span data-ttu-id="6ef39-275">toofurther megérteni, hogy a következő négy elemeinek módosítása hírcsatorna processzor együttműködni, például a következő diagram hello vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="6ef39-275">toofurther understand how these four elements of Change Feed Processor work together, let's look at an example in hello following diagram.</span></span> <span data-ttu-id="6ef39-276">hello figyelt gyűjtemény tárolók dokumentumok és hello "város" használja, mint a hello partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="6ef39-276">hello monitored collection stores documents and uses hello "city" as hello partition key.</span></span> <span data-ttu-id="6ef39-277">Látható, hogy kék hello partíció és így tovább tartalmaz-e "A-E" hello "város" mezőt a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="6ef39-277">We see that hello blue partition contains documents with hello "city" field from "A-E" and so on.</span></span> <span data-ttu-id="6ef39-278">Nincsenek két hello négy partíció párhuzamosan olvasásakor két fogyasztók rendelkező állomás.</span><span class="sxs-lookup"><span data-stu-id="6ef39-278">There are two hosts, each with two consumers reading from hello four partitions in parallel.</span></span> <span data-ttu-id="6ef39-279">hello nyilak hello az adott helyet olvasásakor hello fogyasztók adatcsatorna módosítása.</span><span class="sxs-lookup"><span data-stu-id="6ef39-279">hello arrows show hello consumers reading from a specific spot in hello change feed.</span></span> <span data-ttu-id="6ef39-280">Hello első partícióban hello sötétebb kék olvasatlan módosítások jelöli, amíg hello könnyű kék már olvasni a változásokat a hello módosítás adatcsatorna hello jelöli.</span><span class="sxs-lookup"><span data-stu-id="6ef39-280">In hello first partition, hello darker blue represents unread changes while hello light blue represents hello already read changes on hello change feed.</span></span> <span data-ttu-id="6ef39-281">hello állomások hello bérleti gyűjtemény toostore hello aktuális pozíciója mindegyik felhasználó olvasása "Folytatás" érték tookeep nyomon használják.</span><span class="sxs-lookup"><span data-stu-id="6ef39-281">hello hosts use hello lease collection toostore a "continuation" value tookeep track of hello current reading position for each consumer.</span></span> 

![Hello Azure Cosmos DB módosítás használatával hírcsatorna host processzor](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a><span data-ttu-id="6ef39-283">Változás használatával hírcsatorna processzor könyvtár</span><span class="sxs-lookup"><span data-stu-id="6ef39-283">Using Change Feed Processor Library</span></span> 
<span data-ttu-id="6ef39-284">hello a következő szakasz azt ismerteti, hogyan toouse hello módosítás hírcsatorna processzor könyvtár hello környezetében replikálni a módosításokat a forrás gyűjtemény tooa cél gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="6ef39-284">hello following section explains how toouse hello Change Feed Processor library in hello context of replicating changes from a source collection tooa destination collection.</span></span> <span data-ttu-id="6ef39-285">Hello forrás gyűjtemény Íme hello figyelt gyűjtemény módosítás hírcsatorna processzor.</span><span class="sxs-lookup"><span data-stu-id="6ef39-285">Here, hello source collection is hello monitored collection in Change Feed Processor.</span></span> 

<span data-ttu-id="6ef39-286">**Telepítse és hello módosítás hírcsatorna processzor NuGet-csomagot tartalmazza**</span><span class="sxs-lookup"><span data-stu-id="6ef39-286">**Install and include hello Change Feed Processor NuGet package**</span></span> 

<span data-ttu-id="6ef39-287">Változás hírcsatorna processzor NuGet-csomag telepítéséhez először telepíteni:</span><span class="sxs-lookup"><span data-stu-id="6ef39-287">Before installing Change Feed Processor NuGet Package, first install:</span></span> 
* <span data-ttu-id="6ef39-288">Microsoft.Azure.DocumentDB, 1.13.1 verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="6ef39-288">Microsoft.Azure.DocumentDB, version 1.13.1 or above</span></span> 
* <span data-ttu-id="6ef39-289">Newtonsoft.Json, 9.0.1 verzió vagy újabb telepítés `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` adja hozzá hivatkozásként.</span><span class="sxs-lookup"><span data-stu-id="6ef39-289">Newtonsoft.Json, version 9.0.1 or above Install `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` and include it as a reference.</span></span>

<span data-ttu-id="6ef39-290">**Figyelt, címbérlet és a cél-gyűjtemény létrehozása**</span><span class="sxs-lookup"><span data-stu-id="6ef39-290">**Create a monitored, lease and destination collection**</span></span> 

<span data-ttu-id="6ef39-291">A sorrend toouse hello Változáskezelési hírcsatorna processzor könyvtár a toobe hello processzor állomásokkal futtatása előtt létre szükség hello bérleti gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="6ef39-291">In order toouse hello Change Feed Processor Library, hello lease collection needs toobe created before running hello processor host(s).</span></span> <span data-ttu-id="6ef39-292">Ebben az esetben ajánlott bérleti gyűjteménye tárolja egy másik fiókot és egy írási régió szorosabb toowhere hello módosítás hírcsatorna processzor.</span><span class="sxs-lookup"><span data-stu-id="6ef39-292">Again, we recommend storing a lease collection on a different account with a write region closer toowhere hello Change Feed Processor is running.</span></span> <span data-ttu-id="6ef39-293">A példában szereplő adatok mozgása toocreate hello célgyűjtemény hello módosítás hírcsatorna Processor host futtatása előtt igazolnia kell.</span><span class="sxs-lookup"><span data-stu-id="6ef39-293">In this data movement example, we need toocreate hello destination collection before running hello Change Feed Processor host.</span></span> <span data-ttu-id="6ef39-294">Hello mintakód ezt nevezik a figyelt, segítő metódus toocreate hello bérelt, és a célként megadott gyűjtemények, ha még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="6ef39-294">In hello sample code we call a helper method toocreate hello monitored, leased, and destination collections if they do not already exist.</span></span> 

> [!WARNING]
> <span data-ttu-id="6ef39-295">Gyűjtemény létrehozása megegyezik megvalósítását, árképzési lefoglalja hello alkalmazás toocommunicate rendelkező Azure Cosmos DB adatátviteli sebességét.</span><span class="sxs-lookup"><span data-stu-id="6ef39-295">Creating a collection has pricing implications, as you are reserving throughput for hello application toocommunicate with Azure Cosmos DB.</span></span> <span data-ttu-id="6ef39-296">További részletekért látogasson el a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="6ef39-296">For more details, please visit hello [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

<span data-ttu-id="6ef39-297">*A processzor állomás létrehozása*</span><span class="sxs-lookup"><span data-stu-id="6ef39-297">*Creating a processor host*</span></span>

<span data-ttu-id="6ef39-298">Hello `ChangeFeedProcessorHost` osztály szálbiztos, több folyamatból, biztonságos futtatókörnyezetet környezetet eseményfeldolgozói megvalósításokhoz is biztosít az ellenőrzőpontok használatát és a partícióbérlés-kezelést biztosít.</span><span class="sxs-lookup"><span data-stu-id="6ef39-298">hello `ChangeFeedProcessorHost` class provides a thread-safe, multi-process, safe runtime environment for event processor implementations that also provides checkpointing and partition lease management.</span></span> <span data-ttu-id="6ef39-299">toouse hello `ChangeFeedProcessorHost` osztály, Megvalósíthat `IChangeFeedObserver`.</span><span class="sxs-lookup"><span data-stu-id="6ef39-299">toouse hello `ChangeFeedProcessorHost` class, you can implement `IChangeFeedObserver`.</span></span> <span data-ttu-id="6ef39-300">Ez a felület három metódust tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="6ef39-300">This interface contains three methods:</span></span>

* <span data-ttu-id="6ef39-301">`OpenAsync`: Ez a funkció neve módosítás adatcsatorna megfigyelő megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="6ef39-301">`OpenAsync`: This function is called when change feed observer is opened.</span></span> <span data-ttu-id="6ef39-302">Módosított tooperform egy bizonyos művelet használható, ha a fogyasztó megfigyelő már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="6ef39-302">It can be modified tooperform a specific action when consumer/observer is opened.</span></span>  
* <span data-ttu-id="6ef39-303">`CloseAsync`: Ez a függvény van meghívva, amikor az adatcsatorna megfigyelő módosítása a rendszer megszakítja.</span><span class="sxs-lookup"><span data-stu-id="6ef39-303">`CloseAsync`: This function is called when change feed observer is terminated.</span></span> <span data-ttu-id="6ef39-304">Módosított tooperform egy bizonyos művelet használható, ha a fogyasztó megfigyelő le van zárva.</span><span class="sxs-lookup"><span data-stu-id="6ef39-304">It can be modified tooperform a specific action when consumer/observer is closed.</span></span>  
* <span data-ttu-id="6ef39-305">`ProcessChangesAsync`: Ha módosítás adatcsatorna dokumentum új módosítások érhetők Ez a funkció neve.</span><span class="sxs-lookup"><span data-stu-id="6ef39-305">`ProcessChangesAsync`: This function is called when document new changes are available on change feed.</span></span> <span data-ttu-id="6ef39-306">Lehet, hogy a módosított tooperform egy bizonyos művelet minden hírcsatorna módosítása frissítés után.</span><span class="sxs-lookup"><span data-stu-id="6ef39-306">It can be modified tooperform a specific action upon every change feed update.</span></span>  

<span data-ttu-id="6ef39-307">A jelen példában hello illesztőfelület megvalósítása `IChangeFeedObserver` keresztül hello `DocumentFeedObserver` osztály.</span><span class="sxs-lookup"><span data-stu-id="6ef39-307">In our example, we implement hello interface `IChangeFeedObserver` through hello `DocumentFeedObserver` class.</span></span> <span data-ttu-id="6ef39-308">Itt hello `ProcessChangesAsync` upserts (frissítések) egy dokumentumot módosítás a hírcsatorna hello cél gyűjteménybe működik.</span><span class="sxs-lookup"><span data-stu-id="6ef39-308">Here, hello `ProcessChangesAsync` function upserts (updates) a document from change feed into hello destination collection.</span></span> <span data-ttu-id="6ef39-309">Ez a példa esetén hasznos adatok áthelyezése egy gyűjtemény tooanother rendelés toochange hello partíciós kulcs egy adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="6ef39-309">This example is useful for moving data from one collection tooanother in order toochange hello partition key of a data set.</span></span> 

<span data-ttu-id="6ef39-310">*Futó hello Processor Host*</span><span class="sxs-lookup"><span data-stu-id="6ef39-310">*Running hello Processor Host*</span></span>

<span data-ttu-id="6ef39-311">Mielőtt megkezdené az események feldolgozásával, mind az adatcsatorna beállításainak módosítása, és adatcsatorna állomás beállításainak módosítása testre szabható.</span><span class="sxs-lookup"><span data-stu-id="6ef39-311">Before beginning event processing, both change feed options and change feed host options can be customized.</span></span> 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval too15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
<span data-ttu-id="6ef39-312">a következő táblák hello szabható hello területeken foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="6ef39-312">hello specific fields that can be customized are summarized in hello following tables.</span></span> 

<span data-ttu-id="6ef39-313">**Adatcsatorna beállításainak módosítására a**:</span><span class="sxs-lookup"><span data-stu-id="6ef39-313">**Change Feed Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="6ef39-314">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6ef39-314">Property Name</span></span></th>
        <th><span data-ttu-id="6ef39-315">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ef39-315">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-316">MaxItemCount</span><span class="sxs-lookup"><span data-stu-id="6ef39-316">MaxItemCount</span></span></td>
        <td><span data-ttu-id="6ef39-317">Lekérdezi vagy beállítja a számbavételi művelet hello hello Azure Cosmos-adatbázis adatbázis-szolgáltatás által visszaadott elemek toobe hello maximális számát.</span><span class="sxs-lookup"><span data-stu-id="6ef39-317">Gets or sets hello maximum number of items toobe returned in hello enumeration operation in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-318">PartitionKeyRangeId</span><span class="sxs-lookup"><span data-stu-id="6ef39-318">PartitionKeyRangeId</span></span></td>
        <td><span data-ttu-id="6ef39-319">Lekérdezi vagy beállítja a legfontosabb tartomány partícióazonosító hello hello aktuális kérelem hello Azure Cosmos DB adatbázis szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6ef39-319">Gets or sets hello partition key range id for hello current request in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-320">RequestContinuation</span><span class="sxs-lookup"><span data-stu-id="6ef39-320">RequestContinuation</span></span></td>
        <td><span data-ttu-id="6ef39-321">Lekérdezi vagy beállítja a hello kérelem folytatási kód hello Azure Cosmos DB adatbázis szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6ef39-321">Gets or sets hello request continuation token in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="6ef39-322">SessionToken</span><span class="sxs-lookup"><span data-stu-id="6ef39-322">SessionToken</span></span></td>
        <td><span data-ttu-id="6ef39-323">Lekérdezi vagy beállítja a hello munkameneti jogkivonat használatra hello Azure Cosmos-adatbázis adatbázis-szolgáltatás a munkamenet-konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="6ef39-323">Gets or sets hello session token for use with session consistency in hello Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="6ef39-324">StartFromBeginning</span><span class="sxs-lookup"><span data-stu-id="6ef39-324">StartFromBeginning</span></span></td>
        <td><span data-ttu-id="6ef39-325">Lekérdezi vagy beállítja, hogy az adatcsatorna-hello Azure Cosmos DB adatbázis szolgáltatás módosítása hello kezdve (igaz) vagy az aktuális (false) kell kezdődnie..</span><span class="sxs-lookup"><span data-stu-id="6ef39-325">Gets or sets whether change feed in hello Azure Cosmos DB database service should start from hello beginning (true) or from current (false).</span></span> <span data-ttu-id="6ef39-326">Alapértelmezés szerint megkezdése az aktuális (false).</span><span class="sxs-lookup"><span data-stu-id="6ef39-326">By default, it starts from current (false).</span></span></td>
    </tr>
</table>

<span data-ttu-id="6ef39-327">**Adatcsatorna állomás beállításainak módosítására a**:</span><span class="sxs-lookup"><span data-stu-id="6ef39-327">**Change Feed Host Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="6ef39-328">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6ef39-328">Property Name</span></span></th>
        <th><span data-ttu-id="6ef39-329">Típus</span><span class="sxs-lookup"><span data-stu-id="6ef39-329">Type</span></span></th>
        <th><span data-ttu-id="6ef39-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ef39-330">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-331">LeaseRenewInterval</span><span class="sxs-lookup"><span data-stu-id="6ef39-331">LeaseRenewInterval</span></span></td>
        <td><span data-ttu-id="6ef39-332">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6ef39-332">TimeSpan</span></span></td>
        <td><span data-ttu-id="6ef39-333">a partíciók hello ChangeFeedEventHost példány által jelenleg tárolt összes címbérlet intervallumát hello.</span><span class="sxs-lookup"><span data-stu-id="6ef39-333">hello interval for all leases for partitions currently held by hello ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-334">LeaseAcquireInterval</span><span class="sxs-lookup"><span data-stu-id="6ef39-334">LeaseAcquireInterval</span></span></td>
        <td><span data-ttu-id="6ef39-335">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6ef39-335">TimeSpan</span></span></td>
        <td><span data-ttu-id="6ef39-336">hello időköz tookick egy feladat toocompute ki, hogy partíciók vannak elosztva között ismert állomás példányok.</span><span class="sxs-lookup"><span data-stu-id="6ef39-336">hello interval tookick off a task toocompute whether partitions are distributed evenly among known host instances.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-337">LeaseExpirationInterval</span><span class="sxs-lookup"><span data-stu-id="6ef39-337">LeaseExpirationInterval</span></span></td>
        <td><span data-ttu-id="6ef39-338">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6ef39-338">TimeSpan</span></span></td>
        <td><span data-ttu-id="6ef39-339">hello időköz mely hello bérleti lesz végrehajtva a partíció képviselő címbérletét.</span><span class="sxs-lookup"><span data-stu-id="6ef39-339">hello interval for which hello lease is taken on a lease representing a partition.</span></span> <span data-ttu-id="6ef39-340">Hello bérleti intervallum nem hosszabbítja meg, ha lejárt, és hello partíció tulajdonjogának áthelyezése tooanother ChangeFeedEventHost példány.</span><span class="sxs-lookup"><span data-stu-id="6ef39-340">If hello lease is not renewed within this interval, it is expired and ownership of hello partition moves tooanother ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-341">FeedPollDelay</span><span class="sxs-lookup"><span data-stu-id="6ef39-341">FeedPollDelay</span></span></td>
        <td><span data-ttu-id="6ef39-342">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6ef39-342">TimeSpan</span></span></td>
        <td><span data-ttu-id="6ef39-343">az új módosítások hello partíció lekérdezési hello késleltetés hírcsatorna, után minden aktuális módosítások vannak merül le.</span><span class="sxs-lookup"><span data-stu-id="6ef39-343">hello delay between polling a partition for new changes on hello feed, after all current changes are drained.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-344">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="6ef39-344">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="6ef39-345">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="6ef39-345">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="6ef39-346">hello gyakoriság toocheckpoint címbérleteket.</span><span class="sxs-lookup"><span data-stu-id="6ef39-346">hello frequency toocheckpoint leases.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-347">MinPartitionCount</span><span class="sxs-lookup"><span data-stu-id="6ef39-347">MinPartitionCount</span></span></td>
        <td><span data-ttu-id="6ef39-348">int</span><span class="sxs-lookup"><span data-stu-id="6ef39-348">Int</span></span></td>
        <td><span data-ttu-id="6ef39-349">hello minimálisan partíciószám hello állomás.</span><span class="sxs-lookup"><span data-stu-id="6ef39-349">hello minimum partition count for hello host.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-350">MaxPartitionCount</span><span class="sxs-lookup"><span data-stu-id="6ef39-350">MaxPartitionCount</span></span></td>
        <td><span data-ttu-id="6ef39-351">int</span><span class="sxs-lookup"><span data-stu-id="6ef39-351">Int</span></span></td>
        <td><span data-ttu-id="6ef39-352">a partíciók hello állomás hello maximális számát is ki tud szolgálni.</span><span class="sxs-lookup"><span data-stu-id="6ef39-352">hello maximum number of partitions hello host can serve.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="6ef39-353">DiscardExistingLeases</span><span class="sxs-lookup"><span data-stu-id="6ef39-353">DiscardExistingLeases</span></span></td>
        <td><span data-ttu-id="6ef39-354">logikai érték</span><span class="sxs-lookup"><span data-stu-id="6ef39-354">Bool</span></span></td>
        <td><span data-ttu-id="6ef39-355">E hello indítsa el hello gazdagép összes meglévő címbérlet törölni kell, és teljesen új hello állomás kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="6ef39-355">Whether on hello start of hello host all existing leases should be deleted and hello host should start from scratch.</span></span></td>
    </tr>
</table>


<span data-ttu-id="6ef39-356">toostart Eseményfeldolgozási, példányosítható `ChangeFeedProcessorHost`, hello megfelelő paraméterek megadása az Azure Cosmos DB-gyűjteménnyel.</span><span class="sxs-lookup"><span data-stu-id="6ef39-356">toostart event processing, instantiate `ChangeFeedProcessorHost`, providing hello appropriate parameters for your Azure Cosmos DB collection.</span></span> <span data-ttu-id="6ef39-357">Majd, meghívják `RegisterObserverAsync` tooregister a `IChangeFeedObserver` hello futásidejű megvalósítást (DocumentFeedObserver ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="6ef39-357">Then, call `RegisterObserverAsync` tooregister your `IChangeFeedObserver` (DocumentFeedObserver in this example) implementation with hello runtime.</span></span> <span data-ttu-id="6ef39-358">Ezen a ponton hello kísérlete tooacquire a címbérlet minden partíciós kulcs-köréről hello Azure Cosmos DB gyűjtemény "mohó" algoritmus használatával.</span><span class="sxs-lookup"><span data-stu-id="6ef39-358">At this point, hello host attempts tooacquire a lease on every partition key range in hello Azure Cosmos DB collection using a "greedy" algorithm.</span></span> <span data-ttu-id="6ef39-359">Ezek a bérletek egy adott időtartamra szólnak utolsó, és majd meg kell újítani.</span><span class="sxs-lookup"><span data-stu-id="6ef39-359">These leases last for a given timeframe and must then be renewed.</span></span> <span data-ttu-id="6ef39-360">Ahogy újabb csomópontok feldolgozópéldányok, ebben az esetben ismét online elérhető, azok helyezze, bérletfoglalásokat végeznek, és idővel hello terhelés átvált csomópontok között, minden egyes állomás próbál tooacquire több bérletet.</span><span class="sxs-lookup"><span data-stu-id="6ef39-360">As new nodes, worker instances, in this case, come online, they place lease reservations and over time hello load shifts between nodes as each host attempts tooacquire more leases.</span></span> 

<span data-ttu-id="6ef39-361">Hello kódmintában használjuk a gyári osztály (DocumentFeedObserverFactory.cs) toocreate egy megfigyelő és hello `RegistObserverFactoryAsync` tooregister hello megfigyelő.</span><span class="sxs-lookup"><span data-stu-id="6ef39-361">In hello sample code, we use a factory class (DocumentFeedObserverFactory.cs) toocreate an observer and hello `RegistObserverFactoryAsync` tooregister hello observer.</span></span> 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter toostop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
<span data-ttu-id="6ef39-362">Idővel kialakul egy egyensúlyi állapot.</span><span class="sxs-lookup"><span data-stu-id="6ef39-362">Over time, an equilibrium is established.</span></span> <span data-ttu-id="6ef39-363">Ennek a dinamikus képességnek lehetővé teszi, hogy az automatikus skálázást-tooconsumers alkalmazott toobe CPU-alapú a felfelé és lefelé méretezéshez egyaránt.</span><span class="sxs-lookup"><span data-stu-id="6ef39-363">This dynamic capability enables CPU-based auto-scaling toobe applied tooconsumers for both scale-up and scale-down.</span></span> <span data-ttu-id="6ef39-364">Ha a módosításokat, mint a fogyasztók tud feldolgozni gyorsabban Azure Cosmos DB rendelkezésre áll, hello Processzor növelésével lehet feldolgozópéldányok számának automatikus skálázása használt toocause.</span><span class="sxs-lookup"><span data-stu-id="6ef39-364">If changes are available in Azure Cosmos DB at a faster rate than consumers can process, hello CPU increase on consumers can be used toocause an auto-scale on worker instance count.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ef39-365">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ef39-365">Next steps</span></span>
* <span data-ttu-id="6ef39-366">Próbálja meg hello [Azure Cosmos DB módosítsa hírcsatorna mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span><span class="sxs-lookup"><span data-stu-id="6ef39-366">Try hello [Azure Cosmos DB Change feed code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span></span>
* <span data-ttu-id="6ef39-367">Ismerkedés a hello kódolási [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="6ef39-367">Get started coding with hello [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) or hello [REST API](/rest/api/documentdb/).</span></span>
