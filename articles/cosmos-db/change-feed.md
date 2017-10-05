---
title: "A módosítást végzett adatcsatorna-támogatás az Azure Cosmos Adatbázisba |} Microsoft Docs"
description: "Azure Cosmos DB módosítás adatcsatorna támogatási használja a dokumentumok nyomon követéséhez és esemény-alapú feldolgozási például eseményindítók és gyorsítótárak és elemzési rendszerek frissítése."
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
ms.openlocfilehash: 160fbc98e0f3dcc7d17cbe0c7f7425811596a896
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a><span data-ttu-id="427e7-104">A módosítás adatcsatorna-támogatás az Azure Cosmos Adatbázisba használata</span><span class="sxs-lookup"><span data-stu-id="427e7-104">Working with the change feed support in Azure Cosmos DB</span></span>
<span data-ttu-id="427e7-105">[Az Azure Cosmos DB](../cosmos-db/introduction.md) gyors és rugalmas globálisan replikálni, amely nagy mennyiségű tranzakciós és működési adatok tárolására, előre jelezhető egyjegyű ezredmásodperces késést olvasása szolgál, és írja az adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="427e7-105">[Azure Cosmos DB](../cosmos-db/introduction.md) is a fast and flexible globally replicated database service that is used for storing high-volume transactional and operational data with predictable single-digit millisecond latency for reads and writes.</span></span> <span data-ttu-id="427e7-106">Így jól alkalmazható IoT, játékok, kiskereskedelmi, és működési naplózási alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="427e7-106">This makes it well-suited for IoT, gaming, retail, and operational logging applications.</span></span> <span data-ttu-id="427e7-107">Egy közös kialakítási mintában a ezeket az alkalmazásokat az Azure Cosmos DB adatok módosításainak nyomon és materializált nézet frissítése, hajtsa végre a valós idejű elemzési, archiválja a cold tárolóhoz, ezeket a módosításokat bizonyos eseményekről eseményindító értesítések.</span><span class="sxs-lookup"><span data-stu-id="427e7-107">A common design pattern in these applications is to track changes made to Azure Cosmos DB data, and update materialized views, perform real-time analytics, archive data to cold storage, and trigger notifications on certain events based on these changes.</span></span> <span data-ttu-id="427e7-108">A **módosítás hírcsatorna támogatási** az Azure Cosmos-Adatbázisba az egyes ezeket a mintákat méretezhető és hatékony megoldások létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="427e7-108">The **change feed support** in Azure Cosmos DB enables you to build efficient and scalable solutions for each of these patterns.</span></span>

<span data-ttu-id="427e7-109">Támogatási hírcsatorna módosítását Azure Cosmos DB biztosít egy dokumentumot egy Azure Cosmos DB gyűjteményt, amelyben a módosítás sorrendben rendezett listát.</span><span class="sxs-lookup"><span data-stu-id="427e7-109">With change feed support, Azure Cosmos DB provides a sorted list of documents within an Azure Cosmos DB collection in the order in which they were modified.</span></span> <span data-ttu-id="427e7-110">Ez az adatcsatorna segítségével hallgassa meg a gyűjteményben lévő adatok módosításait, és a műveleteket, mint:</span><span class="sxs-lookup"><span data-stu-id="427e7-110">This feed can be used to listen for modifications to data within the collection and perform actions such as:</span></span>

* <span data-ttu-id="427e7-111">Indul el a következőt hívja az API-k, ha egy dokumentum hozzáadásakor vagy módosításakor</span><span class="sxs-lookup"><span data-stu-id="427e7-111">Trigger a call to an API when a document is inserted or modified</span></span>
* <span data-ttu-id="427e7-112">Valós idejű (stream) feldolgozási végre frissítések</span><span class="sxs-lookup"><span data-stu-id="427e7-112">Perform real-time (stream) processing on updates</span></span>
* <span data-ttu-id="427e7-113">Szinkronizálja az adatokat a gyorsítótár, a keresőmotor vagy a data warehouse-bA</span><span class="sxs-lookup"><span data-stu-id="427e7-113">Synchronize data with a cache, search engine, or data warehouse</span></span>

<span data-ttu-id="427e7-114">Azure Cosmos DB változásai megmaradnak és dolgozható fel aszinkron módon történik, és egy vagy több felhasználói párhuzamos feldolgozás pontjain.</span><span class="sxs-lookup"><span data-stu-id="427e7-114">Changes in Azure Cosmos DB are persisted and can be processed asynchronously, and distributed across one or more consumers for parallel processing.</span></span> <span data-ttu-id="427e7-115">Vizsgáljuk meg az API-k módosítás adatcsatorna és hogyan használhatja őket méretezhető valós idejű alkalmazásokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="427e7-115">Let's look at the APIs for change feed and how you can use them to build scalable real-time applications.</span></span> <span data-ttu-id="427e7-116">Ez a cikk bemutatja, hogyan működik az adatcsatorna Azure Cosmos DB módosítása és a DocumentDB API.</span><span class="sxs-lookup"><span data-stu-id="427e7-116">This article shows how to work with Azure Cosmos DB change feed and the DocumentDB API.</span></span> 

![Energiagazdálkodási valós idejű elemzési és számítógépes forgatókönyvek eseményvezérelt hírcsatorna használata Azure Cosmos DB módosítása](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> <span data-ttu-id="427e7-118">Támogatási hírcsatorna módosítása csak valósul meg a DocumentDB API jelenleg; a Graph API és a tábla API jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="427e7-118">Change feed support is only provided for the DocumentDB API at this time; the Graph API and Table API are not currently supported.</span></span>

## <a name="use-cases-and-scenarios"></a><span data-ttu-id="427e7-119">Használati esetek és forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="427e7-119">Use cases and scenarios</span></span>
<span data-ttu-id="427e7-120">Változás adatcsatorna lehetővé teszi, hogy a nagy mennyiségű írási műveletek nagy adatkészletek hatékony feldolgozás, és alternatívája azonosítására, hogy mi változott teljes adatkészletek lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="427e7-120">Change feed allows for efficient processing of large datasets with a high volume of writes, and offers an alternative to querying entire datasets to identify what has changed.</span></span> <span data-ttu-id="427e7-121">Például a következő feladatok hatékony hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="427e7-121">For example, you can perform the following tasks efficiently:</span></span>

* <span data-ttu-id="427e7-122">Frissítse a gyorsítótárat, search-index vagy adatraktár Azure Cosmos DB-ben tárolt adatokkal.</span><span class="sxs-lookup"><span data-stu-id="427e7-122">Update a cache, search index, or a data warehouse with data stored in Azure Cosmos DB.</span></span>
* <span data-ttu-id="427e7-123">Alkalmazásszintű adatok rétegezési és archiválási alkalmazására, ez azt jelenti, hogy "gyakran használt adatokkal" tárolása Azure Cosmos DB és elavulnak "ritkán használt adatok", a [Azure Blob Storage](../storage/common/storage-introduction.md) vagy [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="427e7-123">Implement application-level data tiering and archival, that is, store "hot data" in Azure Cosmos DB, and age out "cold data" to [Azure Blob Storage](../storage/common/storage-introduction.md) or [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).</span></span>
* <span data-ttu-id="427e7-124">Kötegelt elemzés megvalósítása az adatok használatával [Apache Hadoop](run-hadoop-with-hdinsight.md).</span><span class="sxs-lookup"><span data-stu-id="427e7-124">Implement batch analytics on data using [Apache Hadoop](run-hadoop-with-hdinsight.md).</span></span>
* <span data-ttu-id="427e7-125">Alkalmazzon [lambda folyamatok Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) rendelkező Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="427e7-125">Implement [lambda pipelines on Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) with Azure Cosmos DB.</span></span> <span data-ttu-id="427e7-126">Azure Cosmos-adatbázis, amely kezeli az adatfeldolgozást és a lekérdezés, és az alacsony TCO lambda architektúrák megvalósítása adatbázis méretezhető megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="427e7-126">Azure Cosmos DB provides a scalable database solution that can handle both ingestion and query, and implement lambda architectures with low TCO.</span></span> 
* <span data-ttu-id="427e7-127">Egy másik particionálási sémát rendelkező Azure Cosmos DB fiókot nulla várakozási-ideje áttelepítés végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="427e7-127">Perform zero down-time migrations to another Azure Cosmos DB account with a different partitioning scheme.</span></span>

<span data-ttu-id="427e7-128">**Az Azure Cosmos DB adatfeldolgozást és lekérdezés lambda folyamatok:**</span><span class="sxs-lookup"><span data-stu-id="427e7-128">**Lambda Pipelines with Azure Cosmos DB for ingestion and query:**</span></span>

![Azure-alapú Cosmos DB lambda folyamat adatfeldolgozást és lekérdezés](./media/change-feed/lambda.png)

<span data-ttu-id="427e7-130">Kapnak, és az eszközök, érzékelőket, infrastruktúra és az alkalmazások esemény adatok tárolásához Azure Cosmos Adatbázist kíván használni, és ezek az események feldolgozása valós időben [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [alatt futó Apache Storm](../hdinsight/hdinsight-storm-overview.md), vagy [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="427e7-130">You can use Azure Cosmos DB to receive and store event data from devices, sensors, infrastructure, and applications, and process these events in real-time with [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/hdinsight-storm-overview.md), or [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md).</span></span> 

<span data-ttu-id="427e7-131">Belül a [kiszolgáló nélküli](http://azure.com/serverless) webes és mobilalkalmazások, például a felhasználói profil, beállítások, vagy helyre való bizonyos műveletek, például az eszközök leküldéses értesítések küldése változásokat nyomon követheti az eseményeket is [ Az Azure Functions](../azure-functions/functions-bindings-documentdb.md) vagy [alkalmazásszolgáltatások](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="427e7-131">Within your [serverless](http://azure.com/serverless) web and mobile apps, you can track events such as changes to your customer's profile, preferences, or location to trigger certain actions like sending push notifications to their devices using [Azure Functions](../azure-functions/functions-bindings-documentdb.md) or [App Services](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="427e7-132">Azure Cosmos DB játék összeállításához használata, is, például használata módosítása adatcsatorna megvalósításához a valós idejű ranglisták befejezett játékok származó eredmények alapján.</span><span class="sxs-lookup"><span data-stu-id="427e7-132">If you're using Azure Cosmos DB to build a game, you can, for example, use change feed to implement real-time leaderboards based on scores from completed games.</span></span>

## <a name="how-change-feed-works-in-azure-cosmos-db"></a><span data-ttu-id="427e7-133">Az Azure Cosmos Adatbázisba módosítás adatcsatorna működése</span><span class="sxs-lookup"><span data-stu-id="427e7-133">How change feed works in Azure Cosmos DB</span></span>
<span data-ttu-id="427e7-134">Az Azure Cosmos DB lehetővé teszi a Növekményesen olvasni egy Azure Cosmos DB gyűjteményhez végrehajtott frissítések.</span><span class="sxs-lookup"><span data-stu-id="427e7-134">Azure Cosmos DB provides the ability to incrementally read updates made to an Azure Cosmos DB collection.</span></span> <span data-ttu-id="427e7-135">Ez a változás-hírcsatorna tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="427e7-135">This change feed has the following properties:</span></span>

* <span data-ttu-id="427e7-136">Módosítások állandó az Azure Cosmos Adatbázisba, és aszinkron módon képes lehet feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="427e7-136">Changes are persistent in Azure Cosmos DB and can be processed asynchronously.</span></span>
* <span data-ttu-id="427e7-137">A gyűjteményen belül dokumentumok módosításai azonnal a hírcsatorna módosítás érhetők el.</span><span class="sxs-lookup"><span data-stu-id="427e7-137">Changes to documents within a collection are available immediately in the change feed.</span></span>
* <span data-ttu-id="427e7-138">A dokumentum minden módosításakor pontosan egyszer jelenik meg a hírcsatorna módosítás, és ügyfelek kezelése az ellenőrzőpontok használata logikát.</span><span class="sxs-lookup"><span data-stu-id="427e7-138">Each change to a document appears exactly once in the change feed, and clients manage their checkpointing logic.</span></span> <span data-ttu-id="427e7-139">A módosítás adatcsatorna processzor kódtár biztosít automatikus ellenőrzőpont-készítés és a "legalább egyszeri" szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="427e7-139">The change feed processor library provides automatic checkpointing and "at least once" semantics.</span></span>
* <span data-ttu-id="427e7-140">Egy adott dokumentum csak a legutóbbi változás a módosítási napló tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="427e7-140">Only the most recent change for a given document is included in the change log.</span></span> <span data-ttu-id="427e7-141">Előfordulhat, hogy köztes módosítások nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="427e7-141">Intermediate changes may not be available.</span></span>
* <span data-ttu-id="427e7-142">A módosítás hírcsatorna szolgáló belül minden partíciókulcs-értékkel módosítást van rendezve.</span><span class="sxs-lookup"><span data-stu-id="427e7-142">The change feed is sorted by order of modification within each partition key value.</span></span> <span data-ttu-id="427e7-143">Nincs garantált rendelés partíciókulcs értékek között.</span><span class="sxs-lookup"><span data-stu-id="427e7-143">There is no guaranteed order across partition-key values.</span></span>
* <span data-ttu-id="427e7-144">Módosítások is szinkronizálhatja a bármely-időpontban, ez azt jelenti, hogy nincs nem rögzített adatmegőrzési időtartam, amelynek módosítások érhetők el.</span><span class="sxs-lookup"><span data-stu-id="427e7-144">Changes can be synchronized from any point-in-time, that is, there is no fixed data retention period for which changes are available.</span></span>
* <span data-ttu-id="427e7-145">Változások a partíció kulcstartományokkal adattömböket írnak érhetők el.</span><span class="sxs-lookup"><span data-stu-id="427e7-145">Changes are available in chunks of partition key ranges.</span></span> <span data-ttu-id="427e7-146">Ez a funkció lehetővé teszi, hogy a módosítások több fogyasztók/kiszolgáló párhuzamos feldolgozásra nagy gyűjteményekre.</span><span class="sxs-lookup"><span data-stu-id="427e7-146">This capability allows changes from large collections to be processed in parallel by multiple consumers/servers.</span></span>
* <span data-ttu-id="427e7-147">Alkalmazások egyidejűleg a ugyanaz a gyűjtemény több módosítás hírcsatornák kérheti.</span><span class="sxs-lookup"><span data-stu-id="427e7-147">Applications can request for multiple change feeds simultaneously on the same collection.</span></span>

<span data-ttu-id="427e7-148">Az összes fiók alapértelmezés szerint engedélyezve van a Azure Cosmos-adatbázis módosítása adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="427e7-148">Azure Cosmos DB's change feed is enabled by default for all accounts.</span></span> <span data-ttu-id="427e7-149">Használhatja a [kiosztott átviteli sebesség](request-units.md) a írási régióban vagy bármely [olvassa el a régió](distribute-data-globally.md) olvasni a hírcsatorna váltás, ugyanúgy, mint bármilyen más műveletet a Azure Cosmos-Adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="427e7-149">You can use your [provisioned throughput](request-units.md) in your write region or any [read region](distribute-data-globally.md) to read from the change feed, just like any other operation from Azure Cosmos DB.</span></span> <span data-ttu-id="427e7-150">A módosítás hírcsatorna beszúrások és a frissítési műveleteket a gyűjteményben lévő dokumentumokon végzett tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="427e7-150">The change feed includes inserts and update operations made to documents within the collection.</span></span> <span data-ttu-id="427e7-151">Rögzítheti a törlések úgy, hogy a "soft-törlés" jelzőt belül a dokumentumok törlése helyett.</span><span class="sxs-lookup"><span data-stu-id="427e7-151">You can capture deletes by setting a "soft-delete" flag within your documents in place of deletes.</span></span> <span data-ttu-id="427e7-152">Azt is megteheti, beállíthat egy véges lejárati időt a dokumentumok keresztül a [TTL funkció](time-to-live.md), például 24 óra és használja a törlések rögzítéséhez tulajdonság értékeként.</span><span class="sxs-lookup"><span data-stu-id="427e7-152">Alternatively, you can set a finite expiration period for your documents via the [TTL capability](time-to-live.md), for example, 24 hours and use the value of that property to capture deletes.</span></span> <span data-ttu-id="427e7-153">A megoldás dolgozni a változásokat a TTL lejárata időszaknál rövidebb időintervallumon belül van.</span><span class="sxs-lookup"><span data-stu-id="427e7-153">With this solution, you have to process changes within a shorter time interval than the TTL expiration period.</span></span> <span data-ttu-id="427e7-154">A módosítás hírcsatorna minden partíció kulcs tartományon belül a dokumentumgyűjteményt érhető el, és így képes legyen elosztva párhuzamos feldolgozás egy vagy több fogyasztó számára.</span><span class="sxs-lookup"><span data-stu-id="427e7-154">The change feed is available for each partition key range within the document collection, and thus can be distributed across one or more consumers for parallel processing.</span></span> 

![Elosztott feldolgozásához hírcsatorna Azure Cosmos DB módosítása](./media/change-feed/changefeedvisual.png)

<span data-ttu-id="427e7-156">A változás az ügyfélkód a hírcsatorna megvalósításától néhány lehetőség áll.</span><span class="sxs-lookup"><span data-stu-id="427e7-156">You have a few options in how you implement a change feed in your client code.</span></span> <span data-ttu-id="427e7-157">A következő szakaszok azonnal ismertetik a változások hírcsatorna az Azure Cosmos DB REST API és a DocumentDB SDK-k használatával.</span><span class="sxs-lookup"><span data-stu-id="427e7-157">The sections that immediately follow describe how to implement the change feed using the Azure Cosmos DB REST API and the DocumentDB SDKs.</span></span> <span data-ttu-id="427e7-158">A .NET-alkalmazásokban, érdemes azonban az új [módosítás hírcsatorna processzor könyvtár](#change-feed-processor) a Váltás események feldolgozását a hírcsatorna olvasási módosítások leegyszerűsíti a partíciók között, és lehetővé teszi, hogy működik-e a több szál párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="427e7-158">However, for .NET applications, we recommend using the new [Change feed processor library](#change-feed-processor) for processing events from the change feed as it simplifies reading changes across partitions and enables multiple threads working in parallel.</span></span> 

## <span data-ttu-id="427e7-159"><a id="rest-apis"></a>A REST API-t és a DocumentDB SDK-k használata</span><span class="sxs-lookup"><span data-stu-id="427e7-159"><a id="rest-apis"></a>Working with the REST API and DocumentDB SDKs</span></span>
<span data-ttu-id="427e7-160">Azure Cosmos DB biztosít a tárolási és átviteli nevű rugalmas tárolók **gyűjtemények**.</span><span class="sxs-lookup"><span data-stu-id="427e7-160">Azure Cosmos DB provides elastic containers of storage and throughput called **collections**.</span></span> <span data-ttu-id="427e7-161">Gyűjtemények adatainak logikailag csoportosított használatával [kulcsok partícióazonosító](partition-data.md) a méretezhetőség és teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="427e7-161">Data within collections is logically grouped using [partition keys](partition-data.md) for scalability and performance.</span></span> <span data-ttu-id="427e7-162">Azure Cosmos-adatbázis különböző API felületeket biztosít ezeket az adatokat, beleértve a keresési azonosítója (olvasás/Get), a lekérdezés és a olvasás-hírcsatornák (vizsgálatok) eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="427e7-162">Azure Cosmos DB provides various APIs for accessing this data, including lookup by ID (Read/Get), query, and read-feeds (scans).</span></span> <span data-ttu-id="427e7-163">A módosítás hírcsatorna kérhetők le a documentdb két új kérelemfejléc feltöltése `ReadDocumentFeed` API, és azokat a rendszer párhuzamos tartományok partíciós kulcsok között.</span><span class="sxs-lookup"><span data-stu-id="427e7-163">The change feed can be obtained by populating two new request headers to the DocumentDB `ReadDocumentFeed` API, and can be processed in parallel across ranges of partition keys.</span></span>

### <a name="readdocumentfeed-api"></a><span data-ttu-id="427e7-164">ReadDocumentFeed API</span><span class="sxs-lookup"><span data-stu-id="427e7-164">ReadDocumentFeed API</span></span>
<span data-ttu-id="427e7-165">Most egy pillantást rövid ReadDocumentFeed működése.</span><span class="sxs-lookup"><span data-stu-id="427e7-165">Let's take a brief look at how ReadDocumentFeed works.</span></span> <span data-ttu-id="427e7-166">Azure Cosmos DB támogatja a dokumentumok segítségével egy gyűjteményen belül hírcsatorna olvasása a `ReadDocumentFeed` API.</span><span class="sxs-lookup"><span data-stu-id="427e7-166">Azure Cosmos DB supports reading a feed of documents within a collection via the `ReadDocumentFeed` API.</span></span> <span data-ttu-id="427e7-167">Például a következő kérés adja vissza a dokumentumok oldalát a `serverlogs` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="427e7-167">For example, the following request returns a page of documents inside the `serverlogs` collection.</span></span> 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

<span data-ttu-id="427e7-168">Eredmények használatával korlátozni tudja a `x-ms-max-item-count` fejlécet, és olvasási műveletek folytathatók Újraküldés a kérelmet egy `x-ms-continuation` fejléc adott vissza az előző válaszban.</span><span class="sxs-lookup"><span data-stu-id="427e7-168">Results can be limited by using the `x-ms-max-item-count` header, and reads can be resumed by resubmitting the request with a `x-ms-continuation` header returned in the previous response.</span></span> <span data-ttu-id="427e7-169">Az egyetlen ügyfél végrehajtásakor `ReadDocumentFeed` telepítéseket eredmények között partíciók Feladattervek.</span><span class="sxs-lookup"><span data-stu-id="427e7-169">When performed from a single client, `ReadDocumentFeed` iterates through results across partitions serially.</span></span> 

<span data-ttu-id="427e7-170">**Soros dokumentum hírcsatorna olvasása**</span><span class="sxs-lookup"><span data-stu-id="427e7-170">**Serial read document feed**</span></span>

<span data-ttu-id="427e7-171">A hírcsatorna a támogatott egyikével dokumentumok is le [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="427e7-171">You can also retrieve the feed of documents using one of the supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="427e7-172">Például az alábbi kódrészletben láthatja, hogyan használható a [ReadDocumentFeedAsync metódus](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) a .NET.</span><span class="sxs-lookup"><span data-stu-id="427e7-172">For example, the following snippet shows how to use the [ReadDocumentFeedAsync method](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) in .NET.</span></span>

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a><span data-ttu-id="427e7-173">ReadDocumentFeed elosztott végrehajtása</span><span class="sxs-lookup"><span data-stu-id="427e7-173">Distributed execution of ReadDocumentFeed</span></span>
<span data-ttu-id="427e7-174">Az adatokat, vagy több terabájt tartalmazó, vagy frissítéseket nagy mennyiségű betöltési gyűjtemények soros végrehajtásának egyetlen ügyfélgépről olvasási adatcsatorna nem feltétlenül gyakorlati.</span><span class="sxs-lookup"><span data-stu-id="427e7-174">For collections that contain terabytes of data or more, or ingest a large volume of updates, serial execution of read feed from a single client machine might not be practical.</span></span> <span data-ttu-id="427e7-175">A big data forgatókönyvéhez támogatásához Azure Cosmos DB API felületeket biztosít számára terjeszteni `ReadDocumentFeed` hívások transzparens módon több ügyfél olvasók/fogyasztók között.</span><span class="sxs-lookup"><span data-stu-id="427e7-175">In order to support these big data scenarios, Azure Cosmos DB provides APIs to distribute `ReadDocumentFeed` calls transparently across multiple client readers/consumers.</span></span> 

<span data-ttu-id="427e7-176">**Elosztott olvasási dokumentum adatcsatorna**</span><span class="sxs-lookup"><span data-stu-id="427e7-176">**Distributed Read Document Feed**</span></span>

<span data-ttu-id="427e7-177">Méretezhető, a növekményes változásokat feldolgozása Azure Cosmos DB támogat egy kibővített modell a változtatás hírcsatorna API címtartományok partíciós kulcsok alapján.</span><span class="sxs-lookup"><span data-stu-id="427e7-177">To provide scalable processing of incremental changes, Azure Cosmos DB supports a scale-out model for the change feed API based on ranges of partition keys.</span></span>

* <span data-ttu-id="427e7-178">Ezt úgy szerezheti be a partíció listáját egy gyűjtemény végrehajtásához kulcstartományokkal egy `ReadPartitionKeyRanges` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="427e7-178">You can obtain a list of partition key ranges for a collection performing a `ReadPartitionKeyRanges` call.</span></span> 
* <span data-ttu-id="427e7-179">Az egyes kulcs partíciótartomány, végezhet egy `ReadDocumentFeed` dokumentumok olvasás partíciókulcsok adott tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="427e7-179">For each partition key range, you can perform a `ReadDocumentFeed` to read documents with partition keys within that range.</span></span>

### <a name="retrieving-partition-key-ranges-for-a-collection"></a><span data-ttu-id="427e7-180">Egy gyűjtemény partíció kulcstartományokkal beolvasása</span><span class="sxs-lookup"><span data-stu-id="427e7-180">Retrieving partition key ranges for a collection</span></span>
<span data-ttu-id="427e7-181">A partíció kulcstartományokkal kérésével kérheti le a `pkranges` erőforrás egy gyűjteményen belül.</span><span class="sxs-lookup"><span data-stu-id="427e7-181">You can retrieve the partition key ranges by requesting the `pkranges` resource within a collection.</span></span> <span data-ttu-id="427e7-182">Például a következő kérés partíciós kulcs tartományok listájának beolvasása a `serverlogs` gyűjtemény:</span><span class="sxs-lookup"><span data-stu-id="427e7-182">For example the following request retrieves the list of partition key ranges for the `serverlogs` collection:</span></span>

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

<span data-ttu-id="427e7-183">A kérelem a következő választ, a partíció kulcstartományokkal metaadatokat tartalmazó adja vissza:</span><span class="sxs-lookup"><span data-stu-id="427e7-183">This request returns the following response containing metadata about the partition key ranges:</span></span>

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


<span data-ttu-id="427e7-184">**Kulcs tartomány tulajdonságai partícióazonosító**: minden kulcs partíciótartomány metaadat-tulajdonságainak tartalmazza a következő táblázatban:</span><span class="sxs-lookup"><span data-stu-id="427e7-184">**Partition key range properties**: Each partition key range includes the metadata properties in the following table:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="427e7-185">Fejléc neve</span><span class="sxs-lookup"><span data-stu-id="427e7-185">Header name</span></span></th>
        <th><span data-ttu-id="427e7-186">Leírás</span><span class="sxs-lookup"><span data-stu-id="427e7-186">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-187">id</span><span class="sxs-lookup"><span data-stu-id="427e7-187">id</span></span></td>
        <td>
            <p><span data-ttu-id="427e7-188">A partíciós kulcs tartomány azonosítója.</span><span class="sxs-lookup"><span data-stu-id="427e7-188">The ID for the partition key range.</span></span> <span data-ttu-id="427e7-189">Ez az egyes gyűjteményeken belül egy stabil és egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="427e7-189">This is a stable and unique ID within each collection.</span></span></p>
            <p><span data-ttu-id="427e7-190">Használata a következő hívást olvasni a változásokat a partíciós kulcs tartomány.</span><span class="sxs-lookup"><span data-stu-id="427e7-190">Must be used in the following call to read changes by partition key range.</span></span></p>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-191">maxExclusive</span><span class="sxs-lookup"><span data-stu-id="427e7-191">maxExclusive</span></span></td>
        <td><span data-ttu-id="427e7-192">A maximális partíció kulcskivonat értéke a partíciós kulcs tartományon.</span><span class="sxs-lookup"><span data-stu-id="427e7-192">The maximum partition key hash value for the partition key range.</span></span> <span data-ttu-id="427e7-193">Belső használatra.</span><span class="sxs-lookup"><span data-stu-id="427e7-193">For internal use.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-194">minInclusive</span><span class="sxs-lookup"><span data-stu-id="427e7-194">minInclusive</span></span></td>
        <td><span data-ttu-id="427e7-195">A minimális partíció kulcskivonat értéke a partíciós kulcs tartományon.</span><span class="sxs-lookup"><span data-stu-id="427e7-195">The minimum partition key hash value for the partition key range.</span></span> <span data-ttu-id="427e7-196">Belső használatra.</span><span class="sxs-lookup"><span data-stu-id="427e7-196">For internal use.</span></span></td>
    </tr>       
</table>

<span data-ttu-id="427e7-197">Ehhez a támogatott egyikével [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="427e7-197">You can do this using one of the supported [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="427e7-198">Például az alábbi kódrészletben láthatja, hogyan lehet lekérni a .NET használatával partíció kulcstartományokkal a [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metódust.</span><span class="sxs-lookup"><span data-stu-id="427e7-198">For example, the following snippet shows how to retrieve partition key ranges in .NET using the [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) method.</span></span>

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

<span data-ttu-id="427e7-199">Azure Cosmos DB beolvasása a partíciós kulcs tartomány / dokumentumok támogatja úgy, hogy a nem kötelező `x-ms-documentdb-partitionkeyrangeid` fejléc.</span><span class="sxs-lookup"><span data-stu-id="427e7-199">Azure Cosmos DB supports retrieval of documents per partition key range by setting the optional `x-ms-documentdb-partitionkeyrangeid` header.</span></span> 

### <a name="performing-an-incremental-readdocumentfeed"></a><span data-ttu-id="427e7-200">Egy növekményes ReadDocumentFeed végrehajtása</span><span class="sxs-lookup"><span data-stu-id="427e7-200">Performing an incremental ReadDocumentFeed</span></span>
<span data-ttu-id="427e7-201">ReadDocumentFeed a növekményes feldolgozás Azure Cosmos DB gyűjtemények a változásokkal a következő forgatókönyvek feladatokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="427e7-201">ReadDocumentFeed supports the following scenarios/tasks for incremental processing of changes in Azure Cosmos DB collections:</span></span>

* <span data-ttu-id="427e7-202">Olvassa el az összes módosítja az elejétől dokumentumok Ez azt jelenti, hogy webhelycsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="427e7-202">Read all changes to documents from the beginning, that is, from collection creation.</span></span>
* <span data-ttu-id="427e7-203">Olvassa el az összes módosítása az aktuális időponthoz képest dokumentumok jövőbeli frissítései, illetve a felhasználó által megadott idő óta.</span><span class="sxs-lookup"><span data-stu-id="427e7-203">Read all changes to future updates to documents from current time, or any changes since a user-specified time.</span></span>
* <span data-ttu-id="427e7-204">Olvasási minden változást a dokumentumok logikai verziójából a gyűjtemény (ETag).</span><span class="sxs-lookup"><span data-stu-id="427e7-204">Read all changes to documents from a logical version of the collection (ETag).</span></span> <span data-ttu-id="427e7-205">A felhasználók, a növekményes olvasás-hírcsatorna a kérelmek visszaadott ETag alapján is ellenőrzőpont.</span><span class="sxs-lookup"><span data-stu-id="427e7-205">You can checkpoint your consumers based on the returned ETag from incremental read-feed requests.</span></span>

<span data-ttu-id="427e7-206">A változtatások közé tartozik a Beszúrás és a dokumentumok frissítését.</span><span class="sxs-lookup"><span data-stu-id="427e7-206">The changes include inserts and updates to documents.</span></span> <span data-ttu-id="427e7-207">Törlések rögzítéséhez kell egy "helyreállítható törlésre" tulajdonság belül a dokumentumokhoz, vagy a [beépített TTL tulajdonsága](time-to-live.md) , hogy a függőben lévő törlés a hírcsatorna változásával jelezze.</span><span class="sxs-lookup"><span data-stu-id="427e7-207">To capture deletes, you must use a "soft delete" property within your documents, or use the [built-in TTL property](time-to-live.md) to signal a pending deletion in the change feed.</span></span>

<span data-ttu-id="427e7-208">A következő táblázat felsorolja a [kérelem](/rest/api/documentdb/common-documentdb-rest-request-headers.md) és [válaszfejlécek](/rest/api/documentdb/common-documentdb-rest-response-headers.md) ReadDocumentFeed műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="427e7-208">The following table lists the [request](/rest/api/documentdb/common-documentdb-rest-request-headers.md) and [response headers](/rest/api/documentdb/common-documentdb-rest-response-headers.md) for ReadDocumentFeed operations.</span></span>

<span data-ttu-id="427e7-209">**Kérelem fejlécei a növekményes ReadDocumentFeed**:</span><span class="sxs-lookup"><span data-stu-id="427e7-209">**Request headers for incremental ReadDocumentFeed**:</span></span>

<table>
    <tr>
        <th><span data-ttu-id="427e7-210">Fejléc neve</span><span class="sxs-lookup"><span data-stu-id="427e7-210">Header name</span></span></th>
        <th><span data-ttu-id="427e7-211">Leírás</span><span class="sxs-lookup"><span data-stu-id="427e7-211">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-212">A-IM</span><span class="sxs-lookup"><span data-stu-id="427e7-212">A-IM</span></span></td>
        <td><span data-ttu-id="427e7-213">Kell "Növekményes adatcsatorna" értékűre, vagy ellenkező esetben nincs megadva</span><span class="sxs-lookup"><span data-stu-id="427e7-213">Must be set to "Incremental feed", or omitted otherwise</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-214">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="427e7-214">If-None-Match</span></span></td>
        <td>
            <p><span data-ttu-id="427e7-215">Nincs fejléc: összes módosítás vissza (gyűjtemény létrehozása) kezdete</span><span class="sxs-lookup"><span data-stu-id="427e7-215">No header: returns all changes from the beginning (collection creation)</span></span></p>
            <p><span data-ttu-id="427e7-216">"*": minden új módosítások visszatér a gyűjteményben lévő adatok</span><span class="sxs-lookup"><span data-stu-id="427e7-216">"*": returns all new changes to data within the collection</span></span></p>           
            <p><span data-ttu-id="427e7-217">&lt;ETag&gt;: egy gyűjteményhez ETag, állítsa vissza az logikai időbélyeg óta végrehajtott valamennyi módosítást</span><span class="sxs-lookup"><span data-stu-id="427e7-217">&lt;etag&gt;: If set to a collection ETag, returns all changes made since that logical timestamp</span></span></p>
        </td>
    </tr>
    <tr>    
        <td><span data-ttu-id="427e7-218">If-Modified-Since</span><span class="sxs-lookup"><span data-stu-id="427e7-218">If-Modified-Since</span></span></td> 
        <td><span data-ttu-id="427e7-219">RFC 1123 időformátum; Ha meg van adva If-None-Match figyelmen kívül hagyva</span><span class="sxs-lookup"><span data-stu-id="427e7-219">RFC 1123 time format; ignored if If-None-Match is specified</span></span></td> 
    </tr> 
    <tr>
        <td><span data-ttu-id="427e7-220">x-ms-documentdb-partitionkeyrangeid</span><span class="sxs-lookup"><span data-stu-id="427e7-220">x-ms-documentdb-partitionkeyrangeid</span></span></td>
        <td><span data-ttu-id="427e7-221">A partíciós kulcs tartomány azonosítója adatainak olvasása.</span><span class="sxs-lookup"><span data-stu-id="427e7-221">The partition key range ID for reading data.</span></span></td>
    </tr>
</table>

<span data-ttu-id="427e7-222">**A növekményes ReadDocumentFeed válaszfejlécek**:</span><span class="sxs-lookup"><span data-stu-id="427e7-222">**Response headers for incremental ReadDocumentFeed**:</span></span>

<table> <tr>
        <th><span data-ttu-id="427e7-223">Fejléc neve</span><span class="sxs-lookup"><span data-stu-id="427e7-223">Header name</span></span></th>
        <th><span data-ttu-id="427e7-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="427e7-224">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-225">ETag</span><span class="sxs-lookup"><span data-stu-id="427e7-225">etag</span></span></td>
        <td>
            <p><span data-ttu-id="427e7-226">A logikai sorszáma (LSN) a válasz utolsó dokumentum.</span><span class="sxs-lookup"><span data-stu-id="427e7-226">The logical sequence number (LSN) of last document returned in the response.</span></span></p>
            <p><span data-ttu-id="427e7-227">Ez az érték If-None-Match Újraküldés növekményes ReadDocumentFeed folytathatók.</span><span class="sxs-lookup"><span data-stu-id="427e7-227">Incremental ReadDocumentFeed can be resumed by resubmitting this value in If-None-Match.</span></span></p>
        </td>
    </tr>
</table>

<span data-ttu-id="427e7-228">Íme egy minta kérelem gyűjteményben lévő összes növekményes változásokat visszaadására a logikai verzió ETag `28535` és legfontosabb tartomány partícióazonosító = `16`:</span><span class="sxs-lookup"><span data-stu-id="427e7-228">Here's a sample request to return all incremental changes in collection from the logical version/ETag `28535` and partition key range = `16`:</span></span>

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

<span data-ttu-id="427e7-229">Módosítások belül minden partíciós kulcs értéke a partíciós kulcs tartományon belüli idő szerint rendezett.</span><span class="sxs-lookup"><span data-stu-id="427e7-229">Changes are ordered by time within each partition key value within the partition key range.</span></span> <span data-ttu-id="427e7-230">Nincs garantált rendelés partíciókulcs értékek között.</span><span class="sxs-lookup"><span data-stu-id="427e7-230">There is no guaranteed order across partition-key values.</span></span> <span data-ttu-id="427e7-231">Ha nincsenek további eredmények fér is egyetlen lap, Ön olvashat a következő oldalra, a kérelem ismételt elküldése a `If-None-Match` fejléc a következő egyenlő értéket a `etag` a az előző válaszban.</span><span class="sxs-lookup"><span data-stu-id="427e7-231">If there are more results than can fit in a single page, you can read the next page of results by resubmitting the request with the `If-None-Match` header with value equal to the `etag` from the previous response.</span></span> <span data-ttu-id="427e7-232">Ha több dokumentumok volt vagy tranzakciós úton tárolt eljárás vagy eseményindító belül, akkor minden visszaadott válasz ugyanazon az oldalon belüli.</span><span class="sxs-lookup"><span data-stu-id="427e7-232">If multiple documents were inserted or updated transactionally within a stored procedure or trigger, they will all be returned within the same response page.</span></span>

> [!NOTE]
> <span data-ttu-id="427e7-233">Módosítás hírcsatornával kaphat több elemet adott vissza a lapon, mint a megadott `x-ms-max-item-count` több dokumentumok beszúrt vagy frissített belül a tárolt eljárások és eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="427e7-233">With change feed, you might get more items returned in a page than specified in `x-ms-max-item-count` in the case of multiple documents inserted or updated inside a stored procedures or triggers.</span></span> 

<span data-ttu-id="427e7-234">A .NET SDK-val (1.17.0) használata esetén állítsa be a mezőt `StartTime` a `ChangeFeedOptions` közvetlenül vissza a módosított dokumentumokat óta `StartTime` meghívásakor `CreateDocumentChangeFeedQuery`.</span><span class="sxs-lookup"><span data-stu-id="427e7-234">When using the .NET SDK (1.17.0), set the field `StartTime` in `ChangeFeedOptions` to directly return changed documents since `StartTime` when calling  `CreateDocumentChangeFeedQuery`.</span></span> <span data-ttu-id="427e7-235">Megadásával `If-Modified-Since` REST API használatával, a kérelem visszaadható nem a dokumentumok magukat, de ahelyett, hogy a folytatási kód vagy `etag` a válasz-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="427e7-235">By specifying `If-Modified-Since` using the REST API, your request will return not the documents themselves, but rather the continuation token or `etag` in the response header.</span></span> <span data-ttu-id="427e7-236">Vissza a dokumentumok módosítva a megadott időpontban, a folytatási kód `etag` kell majd használható a következő kérelmet `If-None-Match` a tényleges dokumentumok vissza.</span><span class="sxs-lookup"><span data-stu-id="427e7-236">To return the documents modified the specified time, the continuation token `etag` must then be used in the next request with `If-None-Match` to return the actual documents.</span></span> 

<span data-ttu-id="427e7-237">A .NET SDK biztosít a [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) és [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) segítőosztályok a gyűjtemény módosításait eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="427e7-237">The .NET SDK provides the [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) and [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) helper classes to access changes made to a collection.</span></span> <span data-ttu-id="427e7-238">Az alábbi kódrészletben láthatja, hogyan lehet lekérni az összes módosítást az elejéről, az egyetlen ügyfél .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="427e7-238">The following snippet shows how to retrieve all changes from the beginning using the .NET SDK from a single client.</span></span>

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
<span data-ttu-id="427e7-239">Az alábbi kódrészletben láthatja, hogyan kell feldolgozni a módosításokat, és Azure Cosmos DB használatával valós időben a módosítás hírcsatorna támogatása és az előző függvény.</span><span class="sxs-lookup"><span data-stu-id="427e7-239">And the following snippet shows how to process changes in real-time with Azure Cosmos DB by using the change feed support and the preceding function.</span></span> <span data-ttu-id="427e7-240">Az első hívás a dokumentumok a gyűjteményben, és a második csak hozta létre, mert az utolsó ellenőrzőpont létrehozott két dokumentumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="427e7-240">The first call returns all the documents in the collection, and the second only returns the two documents created that were created since the last checkpoint.</span></span>

```csharp
// Returns all documents in the collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only the two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

<span data-ttu-id="427e7-241">A módosítás hírcsatorna ügyfél oldalán logika használatával szelektív feldolgozni az eseményeket is végezhet.</span><span class="sxs-lookup"><span data-stu-id="427e7-241">You can also filter the change feed using client side logic to selectively process events.</span></span> <span data-ttu-id="427e7-242">Például itt található egy kódrészletet, amely az eszköz érzékelők csak hőmérséklet események feldolgozása ügyféloldali LINQ használja.</span><span class="sxs-lookup"><span data-stu-id="427e7-242">For example, here's a snippet that uses client side LINQ to process only temperature change events from device sensors.</span></span>

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <span data-ttu-id="427e7-243"><a id="change-feed-processor"></a>Változáskezelési hírcsatorna processzor könyvtár</span><span class="sxs-lookup"><span data-stu-id="427e7-243"><a id="change-feed-processor"></a>Change Feed Processor library</span></span>
<span data-ttu-id="427e7-244">Egy másik lehetőség az [Azure Cosmos DB módosítása hírcsatorna processzor könyvtár](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), ez elősegítheti a könnyen eloszthatók több felhasználóból közötti adatcsatorna származó Eseményfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="427e7-244">Another option is to use the [Azure Cosmos DB Change Feed Processor library](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), which can help you easily distribute event processing from a change feed across multiple consumers.</span></span> <span data-ttu-id="427e7-245">A könyvtárban kiváló olvasók hírcsatorna a .NET platformon módosítás készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="427e7-245">The library is great for building change feed readers on the .NET platform.</span></span> <span data-ttu-id="427e7-246">Bizonyos munkafolyamatok, amelyek a volna egyszerűsíteni a módosítás hírcsatorna processzor-könyvtár használatával keresztül a Cosmos DB Csomagjától szereplő módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="427e7-246">Some workflows that would be simplified by using the Change Feed Processor library over the methods included in the other Cosmos DB SDKs include:</span></span> 

* <span data-ttu-id="427e7-247">Adatlekérő frissítéseket, a módosítás hírcsatornát, amikor több partíciót az adatok tárolásához</span><span class="sxs-lookup"><span data-stu-id="427e7-247">Pulling updates from change feed when data is stored across multiple partitions</span></span>
* <span data-ttu-id="427e7-248">Áthelyezése vagy az adatok replikálásához egy gyűjteményt a másikra</span><span class="sxs-lookup"><span data-stu-id="427e7-248">Moving or replicating data from one collection to another</span></span>
* <span data-ttu-id="427e7-249">Adatokhoz és az adatcsatorna-módosítás frissítések által indított tevékenységek párhuzamos végrehajtására</span><span class="sxs-lookup"><span data-stu-id="427e7-249">Parallel execution of actions triggered by updates to data and change feed</span></span> 

<span data-ttu-id="427e7-250">Az API-kat a Cosmos SDK-k használatával módosíthatja az egyes partíciók adatcsatorna frissítések pontos hozzáférést biztosít, amíg a hírcsatorna processzor módosítása szalagtárat használó egyszerűbbé teszi a olvasási módosítások partíciók és több szál párhuzamosan működik.</span><span class="sxs-lookup"><span data-stu-id="427e7-250">While using the APIs in the Cosmos SDKs provides precise access to change feed updates in each partition, using the Change Feed Processor library simplifies reading changes across partitions and multiple threads working in parallel.</span></span> <span data-ttu-id="427e7-251">Helyett a manuális módosítások olvasásakor egyes tárolókban, és mentése a folytatási kód minden partíció esetében a módosítás hírcsatorna processzor automatikusan kezeli a olvasási módosításokat egy bérleti mechanizmussal partíciók között.</span><span class="sxs-lookup"><span data-stu-id="427e7-251">Instead of manually reading changes from each container and saving a continuation token for each partition, the Change Feed Processor automatically manages reading changes across partitions using a lease mechanism.</span></span>

<span data-ttu-id="427e7-252">A könyvtár NuGet-csomagként érhető el: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) és a forráskódot, mint a Githubon [minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span><span class="sxs-lookup"><span data-stu-id="427e7-252">The library is available as a NuGet Package: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) and from source code as a Github [sample](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor).</span></span> 

### <a name="understanding-change-feed-processor-library"></a><span data-ttu-id="427e7-253">Változás hírcsatorna processzor könyvtár ismertetése</span><span class="sxs-lookup"><span data-stu-id="427e7-253">Understanding Change Feed Processor library</span></span> 

<span data-ttu-id="427e7-254">A módosítás hírcsatorna processzor végrehajtási négy fő összetevőből: a figyelt gyűjteményhez, a címbérlet gyűjteményt, a processzor gazdagép és a fogyasztók.</span><span class="sxs-lookup"><span data-stu-id="427e7-254">There are four main components of implementing the Change Feed Processor: the monitored collection, the lease collection, the processor host, and the consumers.</span></span> 

<span data-ttu-id="427e7-255">**Figyelt gyűjtemény:** a figyelt gyűjteményhez az adatok, a módosítás hírcsatorna képzésére szolgáló.</span><span class="sxs-lookup"><span data-stu-id="427e7-255">**Monitored Collection:** The monitored collection is the data from which the change feed is generated.</span></span> <span data-ttu-id="427e7-256">Bármely beszúrások és a figyelt gyűjtemény módosításait a módosítási hírcsatorna a gyűjtemény is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="427e7-256">Any inserts and changes to the monitored collection are reflected in the change feed of the collection.</span></span> 

<span data-ttu-id="427e7-257">**Címbérlet gyűjtemény:** feldolgozása a módosítás több Worker közötti adatcsatorna bérleti gyűjtemény koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="427e7-257">**Lease Collection:** The lease collection coordinates processing the change feed across multiple workers.</span></span> <span data-ttu-id="427e7-258">Egy külön gyűjteményt a bérletek egy bérletet eredményez. partíciónként tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="427e7-258">A separate collection is used to store the leases with one lease per partition.</span></span> <span data-ttu-id="427e7-259">Célszerű a címbérlet gyűjtemény tárolását a írási terület közelebb a módosítás hírcsatorna processzor futtató egy másik fiókot.</span><span class="sxs-lookup"><span data-stu-id="427e7-259">It is advantageous to store this lease collection on a different account with the write region closer to where the Change Feed Processor is running.</span></span> <span data-ttu-id="427e7-260">A bérleti objektum tartalmazza a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="427e7-260">A lease object contains the following attributes:</span></span> 
* <span data-ttu-id="427e7-261">Tulajdonos: Adja meg a címbérlet birtokló állomás</span><span class="sxs-lookup"><span data-stu-id="427e7-261">Owner: Specifies the host that owns the lease</span></span>
* <span data-ttu-id="427e7-262">Folytatási: Meghatározza a módosítás egy adott partíció-hírcsatorna helyét (folytatási kód)</span><span class="sxs-lookup"><span data-stu-id="427e7-262">Continuation: Specifies the position (continuation token) in the change feed for a particular partition</span></span>
* <span data-ttu-id="427e7-263">Időbélyeg: Legutóbbi bérleti frissült; az időbélyeg segítségével ellenőrizze, hogy a bérlet lejárt tekinthető</span><span class="sxs-lookup"><span data-stu-id="427e7-263">Timestamp: Last time lease was updated; the timestamp can be used to check whether the lease is considered expired</span></span> 

<span data-ttu-id="427e7-264">**Processzor-állomás:** minden állomás határozza meg, hány particionálja alapján más gazdagépek hány példányban vannak-e aktív bérletek folyamathoz.</span><span class="sxs-lookup"><span data-stu-id="427e7-264">**Processor Host:** Each host determines how many partitions to process based on how many other instances of hosts have active leases.</span></span> 
1.  <span data-ttu-id="427e7-265">A gazdagép indításakor minden gazdagép elosztásához címbérleteket szerez be.</span><span class="sxs-lookup"><span data-stu-id="427e7-265">When a host starts up, it acquires leases to balance the workload across all hosts.</span></span> <span data-ttu-id="427e7-266">Egy gazdagép rendszeres időközönként megújítja címbérleteket,, címbérleteket aktív marad.</span><span class="sxs-lookup"><span data-stu-id="427e7-266">A host periodically renews leases, so leases remain active.</span></span> 
2.  <span data-ttu-id="427e7-267">A gazdagép ellenőrzőpontokat az egyes bérletekhez utolsó folytatási olvasni.</span><span class="sxs-lookup"><span data-stu-id="427e7-267">A host checkpoints the last continuation token to its lease for each read.</span></span> <span data-ttu-id="427e7-268">Párhuzamossági biztonsága érdekében a gazdagép minden egyes bérleti frissítés ETag ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="427e7-268">To ensure concurrency safety, a host checks the ETag for each lease update.</span></span> <span data-ttu-id="427e7-269">Más ellenőrzőpont stratégiák használata is támogatott.</span><span class="sxs-lookup"><span data-stu-id="427e7-269">Other checkpoint strategies are also supported.</span></span>  
3.  <span data-ttu-id="427e7-270">Leállásakor egy állomás kiadott összes címbérlet, de a folytatási információkért tartja, így újból engedélyezheti az olvasást a tárolt ellenőrzőponttól később.</span><span class="sxs-lookup"><span data-stu-id="427e7-270">Upon shutdown, a host releases all leases but keeps the continuation information, so it can resume reading from the stored checkpoint later.</span></span> 

<span data-ttu-id="427e7-271">Jelenleg a gazdagépek száma nem lehet nagyobb, mint a partíciók (címbérleteket).</span><span class="sxs-lookup"><span data-stu-id="427e7-271">At this time the number of hosts cannot be greater than the number of partitions (leases).</span></span>

<span data-ttu-id="427e7-272">**A fogyasztók:** fogyasztók vagy dolgozó a szál által az egyes állomások által kezdeményezett hírcsatorna módosítása feldolgozási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="427e7-272">**Consumers:** Consumers, or workers, are threads that perform the change feed processing initiated by each host.</span></span> <span data-ttu-id="427e7-273">Mindegyik feldolgozó gazdagépnek több felhasználóból lehet.</span><span class="sxs-lookup"><span data-stu-id="427e7-273">Each processor host can have multiple consumers.</span></span> <span data-ttu-id="427e7-274">Mindegyik felhasználó olvassa be, a módosítás hírcsatorna a partícióból hozzá van rendelve, és változások a gazdagép értesítést küld, és címbérleteket lejárt.</span><span class="sxs-lookup"><span data-stu-id="427e7-274">Each consumer reads the change feed from the partition it is assigned to and notifies its host of changes and expired leases.</span></span>

<span data-ttu-id="427e7-275">További megértéséhez, hogy a következő négy elemeinek módosítása hírcsatorna processzor együttműködni, vizsgáljuk meg az alábbi ábra egy példát.</span><span class="sxs-lookup"><span data-stu-id="427e7-275">To further understand how these four elements of Change Feed Processor work together, let's look at an example in the following diagram.</span></span> <span data-ttu-id="427e7-276">A figyelt gyűjtemény tárolja a dokumentumokat, és a "város" használja, mint a partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="427e7-276">The monitored collection stores documents and uses the "city" as the partition key.</span></span> <span data-ttu-id="427e7-277">Látható, hogy a kék partíció és így tovább tartalmaz-e "A-E" a "város" mezőt a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="427e7-277">We see that the blue partition contains documents with the "city" field from "A-E" and so on.</span></span> <span data-ttu-id="427e7-278">Nincsenek két olvasásakor a négy partíció párhuzamosan két fogyasztók rendelkező állomás.</span><span class="sxs-lookup"><span data-stu-id="427e7-278">There are two hosts, each with two consumers reading from the four partitions in parallel.</span></span> <span data-ttu-id="427e7-279">A nyilak a fogyasztók olvasásakor a hírcsatorna változásával konkrét helyet.</span><span class="sxs-lookup"><span data-stu-id="427e7-279">The arrows show the consumers reading from a specific spot in the change feed.</span></span> <span data-ttu-id="427e7-280">Az első partícióban a sötétebb kék olvasatlan módosítások jelöli, míg a ritka kék jelenti. a módosítás adatcsatorna már olvasási változásaival.</span><span class="sxs-lookup"><span data-stu-id="427e7-280">In the first partition, the darker blue represents unread changes while the light blue represents the already read changes on the change feed.</span></span> <span data-ttu-id="427e7-281">A gazdagépek a bérleti gyűjtemény a "Folytatás" érték az aktuális olvasási pozíció az egyes felhasználók nyomon követéséhez tárolására használható.</span><span class="sxs-lookup"><span data-stu-id="427e7-281">The hosts use the lease collection to store a "continuation" value to keep track of the current reading position for each consumer.</span></span> 

![Az Azure Cosmos DB módosítás használatával hírcsatorna processzor állomás](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a><span data-ttu-id="427e7-283">Változás használatával hírcsatorna processzor könyvtár</span><span class="sxs-lookup"><span data-stu-id="427e7-283">Using Change Feed Processor Library</span></span> 
<span data-ttu-id="427e7-284">A következő szakasz ismerteti, hogyan használható a módosítási hírcsatorna processzor könyvtár replikálni a módosításokat a forrás-gyűjteményből a célgyűjtemény környezetében.</span><span class="sxs-lookup"><span data-stu-id="427e7-284">The following section explains how to use the Change Feed Processor library in the context of replicating changes from a source collection to a destination collection.</span></span> <span data-ttu-id="427e7-285">A forrás gyűjtemény Íme a figyelt gyűjtemény módosítás hírcsatorna processzor.</span><span class="sxs-lookup"><span data-stu-id="427e7-285">Here, the source collection is the monitored collection in Change Feed Processor.</span></span> 

<span data-ttu-id="427e7-286">**Telepítse, és tartalmazza a módosítás hírcsatorna processzor NuGet-csomag**</span><span class="sxs-lookup"><span data-stu-id="427e7-286">**Install and include the Change Feed Processor NuGet package**</span></span> 

<span data-ttu-id="427e7-287">Változás hírcsatorna processzor NuGet-csomag telepítéséhez először telepíteni:</span><span class="sxs-lookup"><span data-stu-id="427e7-287">Before installing Change Feed Processor NuGet Package, first install:</span></span> 
* <span data-ttu-id="427e7-288">Microsoft.Azure.DocumentDB, 1.13.1 verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="427e7-288">Microsoft.Azure.DocumentDB, version 1.13.1 or above</span></span> 
* <span data-ttu-id="427e7-289">Newtonsoft.Json, 9.0.1 verzió vagy újabb telepítés `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` adja hozzá hivatkozásként.</span><span class="sxs-lookup"><span data-stu-id="427e7-289">Newtonsoft.Json, version 9.0.1 or above Install `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` and include it as a reference.</span></span>

<span data-ttu-id="427e7-290">**Figyelt, címbérlet és a cél-gyűjtemény létrehozása**</span><span class="sxs-lookup"><span data-stu-id="427e7-290">**Create a monitored, lease and destination collection**</span></span> 

<span data-ttu-id="427e7-291">Ahhoz, hogy a módosítás hírcsatorna processzor könyvtárban, a címbérlet gyűjteményt kell létrehozni a processzor állomásokkal futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="427e7-291">In order to use the Change Feed Processor Library, the lease collection needs to be created before running the processor host(s).</span></span> <span data-ttu-id="427e7-292">Ebben az esetben ajánlott bérleti gyűjteménye tárolja egy írási régióban közelebb legyen, a módosítás hírcsatorna processzor futtató egy másik fiókot.</span><span class="sxs-lookup"><span data-stu-id="427e7-292">Again, we recommend storing a lease collection on a different account with a write region closer to where the Change Feed Processor is running.</span></span> <span data-ttu-id="427e7-293">A példában szereplő adatok mozgása kell létrehozni a célgyűjtemény a módosítás hírcsatorna Processor host futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="427e7-293">In this data movement example, we need to create the destination collection before running the Change Feed Processor host.</span></span> <span data-ttu-id="427e7-294">A mintakód hívása egy segédmetódust létrehozása a figyelt bérelt, és cél gyűjtemények, ha még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="427e7-294">In the sample code we call a helper method to create the monitored, leased, and destination collections if they do not already exist.</span></span> 

> [!WARNING]
> <span data-ttu-id="427e7-295">Gyűjtemény létrehozása megegyezik árképzési hatással vannak, az alkalmazás Azure Cosmos DB kommunikálni átviteli lefoglalja.</span><span class="sxs-lookup"><span data-stu-id="427e7-295">Creating a collection has pricing implications, as you are reserving throughput for the application to communicate with Azure Cosmos DB.</span></span> <span data-ttu-id="427e7-296">További részletekért tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="427e7-296">For more details, please visit the [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

<span data-ttu-id="427e7-297">*A processzor állomás létrehozása*</span><span class="sxs-lookup"><span data-stu-id="427e7-297">*Creating a processor host*</span></span>

<span data-ttu-id="427e7-298">A `ChangeFeedProcessorHost` osztály szálbiztos, több folyamatból, biztonságos futtatókörnyezetet környezetet eseményfeldolgozói megvalósításokhoz is biztosít az ellenőrzőpontok használatát és a partícióbérlés-kezelést biztosít.</span><span class="sxs-lookup"><span data-stu-id="427e7-298">The `ChangeFeedProcessorHost` class provides a thread-safe, multi-process, safe runtime environment for event processor implementations that also provides checkpointing and partition lease management.</span></span> <span data-ttu-id="427e7-299">Használatához a `ChangeFeedProcessorHost` osztály, Megvalósíthat `IChangeFeedObserver`.</span><span class="sxs-lookup"><span data-stu-id="427e7-299">To use the `ChangeFeedProcessorHost` class, you can implement `IChangeFeedObserver`.</span></span> <span data-ttu-id="427e7-300">Ez a felület három metódust tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="427e7-300">This interface contains three methods:</span></span>

* <span data-ttu-id="427e7-301">`OpenAsync`: Ez a funkció neve módosítás adatcsatorna megfigyelő megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="427e7-301">`OpenAsync`: This function is called when change feed observer is opened.</span></span> <span data-ttu-id="427e7-302">Olyan adott műveletet végrehajtani, ha a fogyasztó megfigyelő már meg van nyitva módosítható.</span><span class="sxs-lookup"><span data-stu-id="427e7-302">It can be modified to perform a specific action when consumer/observer is opened.</span></span>  
* <span data-ttu-id="427e7-303">`CloseAsync`: Ez a függvény van meghívva, amikor az adatcsatorna megfigyelő módosítása a rendszer megszakítja.</span><span class="sxs-lookup"><span data-stu-id="427e7-303">`CloseAsync`: This function is called when change feed observer is terminated.</span></span> <span data-ttu-id="427e7-304">Olyan adott műveletet végrehajtani, ha a fogyasztó megfigyelő le van zárva módosítható.</span><span class="sxs-lookup"><span data-stu-id="427e7-304">It can be modified to perform a specific action when consumer/observer is closed.</span></span>  
* <span data-ttu-id="427e7-305">`ProcessChangesAsync`: Ha módosítás adatcsatorna dokumentum új módosítások érhetők Ez a funkció neve.</span><span class="sxs-lookup"><span data-stu-id="427e7-305">`ProcessChangesAsync`: This function is called when document new changes are available on change feed.</span></span> <span data-ttu-id="427e7-306">Minden frissítés hírcsatorna változása esetén egy adott művelet végrehajtására módosítható.</span><span class="sxs-lookup"><span data-stu-id="427e7-306">It can be modified to perform a specific action upon every change feed update.</span></span>  

<span data-ttu-id="427e7-307">A példánkban a felület megvalósítása `IChangeFeedObserver` keresztül a `DocumentFeedObserver` osztály.</span><span class="sxs-lookup"><span data-stu-id="427e7-307">In our example, we implement the interface `IChangeFeedObserver` through the `DocumentFeedObserver` class.</span></span> <span data-ttu-id="427e7-308">Itt a `ProcessChangesAsync` upserts (frissítések) egy dokumentumot módosítás a hírcsatorna a célként megadott gyűjteménybe működik.</span><span class="sxs-lookup"><span data-stu-id="427e7-308">Here, the `ProcessChangesAsync` function upserts (updates) a document from change feed into the destination collection.</span></span> <span data-ttu-id="427e7-309">Ez a példa esetén hasznos adatok áthelyezése egy gyűjteményből másik ahhoz, hogy módosítsa a partíciókulcs egy adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="427e7-309">This example is useful for moving data from one collection to another in order to change the partition key of a data set.</span></span> 

<span data-ttu-id="427e7-310">*A processzor gazdagépen fut*</span><span class="sxs-lookup"><span data-stu-id="427e7-310">*Running the Processor Host*</span></span>

<span data-ttu-id="427e7-311">Mielőtt megkezdené az események feldolgozásával, mind az adatcsatorna beállításainak módosítása, és adatcsatorna állomás beállításainak módosítása testre szabható.</span><span class="sxs-lookup"><span data-stu-id="427e7-311">Before beginning event processing, both change feed options and change feed host options can be customized.</span></span> 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval to 15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
<span data-ttu-id="427e7-312">Az alábbi táblázatban az adott mezők szabható foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="427e7-312">The specific fields that can be customized are summarized in the following tables.</span></span> 

<span data-ttu-id="427e7-313">**Adatcsatorna beállításainak módosítására a**:</span><span class="sxs-lookup"><span data-stu-id="427e7-313">**Change Feed Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="427e7-314">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="427e7-314">Property Name</span></span></th>
        <th><span data-ttu-id="427e7-315">Leírás</span><span class="sxs-lookup"><span data-stu-id="427e7-315">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-316">MaxItemCount</span><span class="sxs-lookup"><span data-stu-id="427e7-316">MaxItemCount</span></span></td>
        <td><span data-ttu-id="427e7-317">Lekérdezi vagy beállítja a számbavételi művelet az Azure Cosmos DB szolgáltatásban a visszaadandó elemek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="427e7-317">Gets or sets the maximum number of items to be returned in the enumeration operation in the Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-318">PartitionKeyRangeId</span><span class="sxs-lookup"><span data-stu-id="427e7-318">PartitionKeyRangeId</span></span></td>
        <td><span data-ttu-id="427e7-319">Lekérdezi vagy beállítja a partícióazonosító legfontosabb tartomány a jelenlegi kérelem az Azure Cosmos DB szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="427e7-319">Gets or sets the partition key range id for the current request in the Azure Cosmos DB database service.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-320">RequestContinuation</span><span class="sxs-lookup"><span data-stu-id="427e7-320">RequestContinuation</span></span></td>
        <td><span data-ttu-id="427e7-321">Lekérdezi vagy beállítja a kérelem folytatási kód az Azure Cosmos DB szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="427e7-321">Gets or sets the request continuation token in the Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="427e7-322">SessionToken</span><span class="sxs-lookup"><span data-stu-id="427e7-322">SessionToken</span></span></td>
        <td><span data-ttu-id="427e7-323">Lekérdezi vagy beállítja a munkameneti jogkivonat használható az Azure Cosmos DB szolgáltatásban munkamenet-konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="427e7-323">Gets or sets the session token for use with session consistency in the Azure Cosmos DB database service.</span></span></td>
    </tr>
        <tr>
        <td><span data-ttu-id="427e7-324">StartFromBeginning</span><span class="sxs-lookup"><span data-stu-id="427e7-324">StartFromBeginning</span></span></td>
        <td><span data-ttu-id="427e7-325">Lekérdezi vagy beállítja, hogy módosítsa az Azure Cosmos DB szolgáltatásban hírcsatorna kell kezdődnie, az elejétől (igaz) vagy a jelenlegi (false).</span><span class="sxs-lookup"><span data-stu-id="427e7-325">Gets or sets whether change feed in the Azure Cosmos DB database service should start from the beginning (true) or from current (false).</span></span> <span data-ttu-id="427e7-326">Alapértelmezés szerint megkezdése az aktuális (false).</span><span class="sxs-lookup"><span data-stu-id="427e7-326">By default, it starts from current (false).</span></span></td>
    </tr>
</table>

<span data-ttu-id="427e7-327">**Adatcsatorna állomás beállításainak módosítására a**:</span><span class="sxs-lookup"><span data-stu-id="427e7-327">**Change Feed Host Options**:</span></span>
<table>
    <tr>
        <th><span data-ttu-id="427e7-328">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="427e7-328">Property Name</span></span></th>
        <th><span data-ttu-id="427e7-329">Típus</span><span class="sxs-lookup"><span data-stu-id="427e7-329">Type</span></span></th>
        <th><span data-ttu-id="427e7-330">Leírás</span><span class="sxs-lookup"><span data-stu-id="427e7-330">Description</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-331">LeaseRenewInterval</span><span class="sxs-lookup"><span data-stu-id="427e7-331">LeaseRenewInterval</span></span></td>
        <td><span data-ttu-id="427e7-332">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="427e7-332">TimeSpan</span></span></td>
        <td><span data-ttu-id="427e7-333">A intervallumát összes címbérlet ChangeFeedEventHost példány által jelenleg birtokolt partíciók.</span><span class="sxs-lookup"><span data-stu-id="427e7-333">The interval for all leases for partitions currently held by the ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-334">LeaseAcquireInterval</span><span class="sxs-lookup"><span data-stu-id="427e7-334">LeaseAcquireInterval</span></span></td>
        <td><span data-ttu-id="427e7-335">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="427e7-335">TimeSpan</span></span></td>
        <td><span data-ttu-id="427e7-336">E partíciók vannak elosztva között ismert állomás-példányok számítási feladatot indítsa a időközt.</span><span class="sxs-lookup"><span data-stu-id="427e7-336">The interval to kick off a task to compute whether partitions are distributed evenly among known host instances.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-337">LeaseExpirationInterval</span><span class="sxs-lookup"><span data-stu-id="427e7-337">LeaseExpirationInterval</span></span></td>
        <td><span data-ttu-id="427e7-338">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="427e7-338">TimeSpan</span></span></td>
        <td><span data-ttu-id="427e7-339">Az időköz, amelynek a címbérlet partíció képviselő címbérletet végzett.</span><span class="sxs-lookup"><span data-stu-id="427e7-339">The interval for which the lease is taken on a lease representing a partition.</span></span> <span data-ttu-id="427e7-340">A bérlet intervallum nem hosszabbítja meg, ha lejárt, és a partíció tulajdonjogának áthelyezése egy másik ChangeFeedEventHost példány.</span><span class="sxs-lookup"><span data-stu-id="427e7-340">If the lease is not renewed within this interval, it is expired and ownership of the partition moves to another ChangeFeedEventHost instance.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-341">FeedPollDelay</span><span class="sxs-lookup"><span data-stu-id="427e7-341">FeedPollDelay</span></span></td>
        <td><span data-ttu-id="427e7-342">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="427e7-342">TimeSpan</span></span></td>
        <td><span data-ttu-id="427e7-343">A késleltetés között lekérdezési egy partíciót, az új módosítások az adatcsatorna, amikor az összes módosítások vannak merül le.</span><span class="sxs-lookup"><span data-stu-id="427e7-343">The delay between polling a partition for new changes on the feed, after all current changes are drained.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-344">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="427e7-344">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="427e7-345">CheckpointFrequency</span><span class="sxs-lookup"><span data-stu-id="427e7-345">CheckpointFrequency</span></span></td>
        <td><span data-ttu-id="427e7-346">Ellenőrzőpont-bérletek gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="427e7-346">The frequency to checkpoint leases.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-347">MinPartitionCount</span><span class="sxs-lookup"><span data-stu-id="427e7-347">MinPartitionCount</span></span></td>
        <td><span data-ttu-id="427e7-348">int</span><span class="sxs-lookup"><span data-stu-id="427e7-348">Int</span></span></td>
        <td><span data-ttu-id="427e7-349">A gazdagép minimálisan partíciószám.</span><span class="sxs-lookup"><span data-stu-id="427e7-349">The minimum partition count for the host.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-350">MaxPartitionCount</span><span class="sxs-lookup"><span data-stu-id="427e7-350">MaxPartitionCount</span></span></td>
        <td><span data-ttu-id="427e7-351">int</span><span class="sxs-lookup"><span data-stu-id="427e7-351">Int</span></span></td>
        <td><span data-ttu-id="427e7-352">A gazdagép ki tud szolgálni partíciók maximális száma.</span><span class="sxs-lookup"><span data-stu-id="427e7-352">The maximum number of partitions the host can serve.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="427e7-353">DiscardExistingLeases</span><span class="sxs-lookup"><span data-stu-id="427e7-353">DiscardExistingLeases</span></span></td>
        <td><span data-ttu-id="427e7-354">logikai érték</span><span class="sxs-lookup"><span data-stu-id="427e7-354">Bool</span></span></td>
        <td><span data-ttu-id="427e7-355">Hogy a gazdagép elindítása az összes meglévő címbérlet törölni kell, és az állomás teljesen új kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="427e7-355">Whether on the start of the host all existing leases should be deleted and the host should start from scratch.</span></span></td>
    </tr>
</table>


<span data-ttu-id="427e7-356">Az események feldolgozásának indításához példányosítható `ChangeFeedProcessorHost`, a megfelelő paramétereket biztosít az Azure Cosmos DB gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="427e7-356">To start event processing, instantiate `ChangeFeedProcessorHost`, providing the appropriate parameters for your Azure Cosmos DB collection.</span></span> <span data-ttu-id="427e7-357">Majd, meghívják `RegisterObserverAsync` regisztrálni a `IChangeFeedObserver` (ebben a példában DocumentFeedObserver) megvalósítást a futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="427e7-357">Then, call `RegisterObserverAsync` to register your `IChangeFeedObserver` (DocumentFeedObserver in this example) implementation with the runtime.</span></span> <span data-ttu-id="427e7-358">Ezen a ponton az állomás megkísérli a "mohó" algoritmus használatával Azure Cosmos DB gyűjtemény minden partíció kulcs tartományához bérletet szerezni.</span><span class="sxs-lookup"><span data-stu-id="427e7-358">At this point, the host attempts to acquire a lease on every partition key range in the Azure Cosmos DB collection using a "greedy" algorithm.</span></span> <span data-ttu-id="427e7-359">Ezek a bérletek egy adott időtartamra szólnak utolsó, és majd meg kell újítani.</span><span class="sxs-lookup"><span data-stu-id="427e7-359">These leases last for a given timeframe and must then be renewed.</span></span> <span data-ttu-id="427e7-360">Ahogy újabb csomópontok feldolgozópéldányok, ebben az esetben ismét online elérhető, azok helyezze, bérletfoglalásokat végeznek, és idővel a terhelés átvált csomópontok között, több bérletet szerezni megkísérli minden gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="427e7-360">As new nodes, worker instances, in this case, come online, they place lease reservations and over time the load shifts between nodes as each host attempts to acquire more leases.</span></span> 

<span data-ttu-id="427e7-361">A mintakód használjuk (DocumentFeedObserverFactory.cs) gyári osztály megfigyelő létrehozásához és a `RegistObserverFactoryAsync` a megfigyelő regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="427e7-361">In the sample code, we use a factory class (DocumentFeedObserverFactory.cs) to create an observer and the `RegistObserverFactoryAsync` to register the observer.</span></span> 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter to stop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
<span data-ttu-id="427e7-362">Idővel kialakul egy egyensúlyi állapot.</span><span class="sxs-lookup"><span data-stu-id="427e7-362">Over time, an equilibrium is established.</span></span> <span data-ttu-id="427e7-363">A dinamikus képességnek a segítségével processzoralapú automatikus skálázás felfelé és lefelé méretezéshez egyaránt a fogyasztók alkalmazandó.</span><span class="sxs-lookup"><span data-stu-id="427e7-363">This dynamic capability enables CPU-based auto-scaling to be applied to consumers for both scale-up and scale-down.</span></span> <span data-ttu-id="427e7-364">Ha a módosításokat, mint a fogyasztók tud feldolgozni gyorsabban Azure Cosmos DB rendelkezésre áll, a Processzor növelésével feldolgozópéldányok számának automatikus skálázása okozhat használható.</span><span class="sxs-lookup"><span data-stu-id="427e7-364">If changes are available in Azure Cosmos DB at a faster rate than consumers can process, the CPU increase on consumers can be used to cause an auto-scale on worker instance count.</span></span>

## <a name="next-steps"></a><span data-ttu-id="427e7-365">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="427e7-365">Next steps</span></span>
* <span data-ttu-id="427e7-366">Próbálja meg a [Azure Cosmos DB módosítsa hírcsatorna mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span><span class="sxs-lookup"><span data-stu-id="427e7-366">Try the [Azure Cosmos DB Change feed code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)</span></span>
* <span data-ttu-id="427e7-367">Ismerkedés a kódolási a [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md) vagy a [REST API](/rest/api/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="427e7-367">Get started coding with the [Azure Cosmos DB SDKs](documentdb-sdk-dotnet.md) or the [REST API](/rest/api/documentdb/).</span></span>
