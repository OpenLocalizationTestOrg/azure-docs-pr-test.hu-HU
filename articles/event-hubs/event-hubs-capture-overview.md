---
title: "Rögzítheti az Azure Event Hubs áttekintése |} Microsoft Docs"
description: "Az Event Hubs rögzítése telemetrikus adatokat rögzítése"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: sethm;darosa
ms.openlocfilehash: 9ae6aa57200b99f382c6e60565db9cfc69f1d3c6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-event-hubs-capture"></a><span data-ttu-id="f6811-103">Az Azure Event Hubs rögzítése</span><span class="sxs-lookup"><span data-stu-id="f6811-103">Azure Event Hubs Capture</span></span>

<span data-ttu-id="f6811-104">Az Azure Event Hubs rögzítése lehetővé teszi, hogy automatikusan a streamelési adatok az Event Hubs egy [Azure Blob Storage tárolóban](https://azure.microsoft.com/services/storage/blobs/) vagy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) hozzáadott rugalmasan választhassa a kiválasztott fiók egy alkalommal vagy méret időköz megadása.</span><span class="sxs-lookup"><span data-stu-id="f6811-104">Azure Event Hubs Capture enables you to automatically deliver the streaming data in Event Hubs to an [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) or [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account of your choice, with the added flexibility of specifying a time or size interval.</span></span> <span data-ttu-id="f6811-105">Rögzítési beállítása gyors, nem futtatható. felügyeleti költségek van, és automatikusan méretezi az Event Hubs [átviteli egységek](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="f6811-105">Setting up Capture is fast, there are no administrative costs to run it, and it scales automatically with Event Hubs [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="f6811-106">Event Hubs rögzítése a legegyszerűbb módja a streamelési adatok betöltése az Azure, és lehetővé teszi az adatok feldolgozása és nem a rögzítés összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="f6811-106">Event Hubs Capture is the easiest way to load streaming data into Azure, and enables you to focus on data processing rather than on data capture.</span></span>

<span data-ttu-id="f6811-107">Event Hubs rögzítése lehetővé teszi az azonos adatfolyamon valós idejű és kötegelt alapú folyamatok feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="f6811-107">Event Hubs Capture enables you to process real-time and batch-based pipelines on the same stream.</span></span> <span data-ttu-id="f6811-108">Ez azt jelenti, hogy idővel igényeknek nő megoldásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="f6811-108">This means you can build solutions that grow with your needs over time.</span></span> <span data-ttu-id="f6811-109">Batch-alapú rendszerek ma felé jövőbeli valós idejű feldolgozással követheti most felépítése, vagy hozzá szeretne adni egy hatékony cold útvonalat ad egy meglévő valós idejű megoldáshoz, Event Hubs rögzítése lehetővé teszi a könnyebb adatfolyam használata.</span><span class="sxs-lookup"><span data-stu-id="f6811-109">Whether you're building batch-based systems today with an eye towards future real-time processing, or you want to add an efficient cold path to an existing real-time solution, Event Hubs Capture makes working with streaming data easier.</span></span>

## <a name="how-event-hubs-capture-works"></a><span data-ttu-id="f6811-110">Event Hubs rögzítése működése</span><span class="sxs-lookup"><span data-stu-id="f6811-110">How Event Hubs Capture works</span></span>

<span data-ttu-id="f6811-111">Az Event Hubs egy idő-megőrzési telemetriai érkező, egy elosztott napló hasonló tartós puffer.</span><span class="sxs-lookup"><span data-stu-id="f6811-111">Event Hubs is a time-retention durable buffer for telemetry ingress, similar to a distributed log.</span></span> <span data-ttu-id="f6811-112">A kulcs az Event Hubs skálázás a [particionált felhasználói modell](event-hubs-features.md#partitions).</span><span class="sxs-lookup"><span data-stu-id="f6811-112">The key to scaling in Event Hubs is the [partitioned consumer model](event-hubs-features.md#partitions).</span></span> <span data-ttu-id="f6811-113">Mindegyik partíció az adatok egy független szegmense, és egymástól függetlenül felhasznált.</span><span class="sxs-lookup"><span data-stu-id="f6811-113">Each partition is an independent segment of data and is consumed independently.</span></span> <span data-ttu-id="f6811-114">Idővel ezek az adatok elavulnak, ki, a konfigurálható megőrzési időtartam alapján.</span><span class="sxs-lookup"><span data-stu-id="f6811-114">Over time this data ages off, based on the configurable retention period.</span></span> <span data-ttu-id="f6811-115">Emiatt egy adott eseményközpont soha nem megtelik "túl."</span><span class="sxs-lookup"><span data-stu-id="f6811-115">As a result, a given event hub never gets "too full."</span></span>

<span data-ttu-id="f6811-116">Event Hubs rögzítése lehetővé teszi a saját Azure Blob storage-fiók és a tároló vagy az Azure Data Lake Store-fiókot, a rögzített adatok tárolására használt megadását.</span><span class="sxs-lookup"><span data-stu-id="f6811-116">Event Hubs Capture enables you to specify your own Azure Blob storage account and container, or Azure Data Lake Store account, which are used to store the captured data.</span></span> <span data-ttu-id="f6811-117">Ezek a fiókok lehet az eseményközpont ugyanabban a régióban, vagy egy másik régióban, a rugalmasságot az Event Hubs rögzítése funkció hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="f6811-117">These accounts can be in the same region as your event hub or in another region, adding to the flexibility of the Event Hubs Capture feature.</span></span>

<span data-ttu-id="f6811-118">A rögzített adatok [Apache Avro] [ Apache Avro] formátum: kompakt, gyors és bináris formátum, amely a beágyazott sémák gazdag adatstruktúrák biztosít.</span><span class="sxs-lookup"><span data-stu-id="f6811-118">Captured data is written in [Apache Avro][Apache Avro] format: a compact, fast, binary format that provides rich data structures with inline schema.</span></span> <span data-ttu-id="f6811-119">Ezt a formátumot a Hadoop ökoszisztémájának, a Stream Analytics és az Azure Data Factory széles körben használja.</span><span class="sxs-lookup"><span data-stu-id="f6811-119">This format is widely used in the Hadoop ecosystem, Stream Analytics, and Azure Data Factory.</span></span> <span data-ttu-id="f6811-120">Az Avro kezelésével kapcsolatos további információk a cikk későbbi részében érhető el.</span><span class="sxs-lookup"><span data-stu-id="f6811-120">More information about working with Avro is available later in this article.</span></span>

### <a name="capture-windowing"></a><span data-ttu-id="f6811-121">Leképezési rögzítése</span><span class="sxs-lookup"><span data-stu-id="f6811-121">Capture windowing</span></span>

<span data-ttu-id="f6811-122">Event Hubs rögzítése lehetővé teszi, hogy meg kell adnia egy ablak rögzítése szabályozására.</span><span class="sxs-lookup"><span data-stu-id="f6811-122">Event Hubs Capture enables you to set up a window to control capturing.</span></span> <span data-ttu-id="f6811-123">Ebben az ablakban a minimális és időpontjának beállítása "első wins házirendnek," ami azt jelenti, hogy az első észlelt az eseményindító a rögzítési művelet.</span><span class="sxs-lookup"><span data-stu-id="f6811-123">This window is a minimum size and time configuration with a "first wins policy," meaning that the first trigger encountered causes a capture operation.</span></span> <span data-ttu-id="f6811-124">Ha egy 15 perc, 100 MB Rögzítés ablak, és 1 MB protokollüzenetek másodpercenkénti, a méret ablak eseményindítók a időszak előtt.</span><span class="sxs-lookup"><span data-stu-id="f6811-124">If you have a fifteen-minute, 100 MB capture window and send 1 MB per second, the size window triggers before the time window.</span></span> <span data-ttu-id="f6811-125">Mindegyik partíció egymástól függetlenül rögzíti, és írási műveletek befejezett blokkblob rögzítési, időpontjában, ahol a rögzítési időköz történt alkalommal nevű.</span><span class="sxs-lookup"><span data-stu-id="f6811-125">Each partition captures independently and writes a completed block blob at the time of capture, named for the time at which the capture interval was encountered.</span></span> <span data-ttu-id="f6811-126">A tároló elnevezési egyezménynek a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="f6811-126">The storage naming convention is as follows:</span></span>

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-to-throughput-units"></a><span data-ttu-id="f6811-127">Átviteli egységek méretezhetők</span><span class="sxs-lookup"><span data-stu-id="f6811-127">Scaling to throughput units</span></span>

<span data-ttu-id="f6811-128">Event Hubs forgalom által szabályozott [átviteli egységek](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="f6811-128">Event Hubs traffic is controlled by [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="f6811-129">Egy átviteli egység lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyiségű második vagy 1000 esemény.</span><span class="sxs-lookup"><span data-stu-id="f6811-129">A single throughput unit allows 1 MB per second or 1000 events per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="f6811-130">Standard Event Hubs 1 – 20 átviteli egység konfigurálhatók, és vásárolhat további, a kvóta növeléséhez [támogatási kérelem][support request].</span><span class="sxs-lookup"><span data-stu-id="f6811-130">Standard Event Hubs can be configured with 1-20 throughput units, and you can purchase more with a quota increase [support request][support request].</span></span> <span data-ttu-id="f6811-131">Használat meghaladja a megvásárolt átviteli egységek folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="f6811-131">Usage beyond your purchased throughput units is throttled.</span></span> <span data-ttu-id="f6811-132">Event Hubs rögzítése másol adatokat közvetlenül a belső tárolóban az Event Hubs átviteli egység kilépő kvóták kihagyásával, és a kimenő forgalom mentése más feldolgozási olvasók, például a Stream Analytics vagy Spark.</span><span class="sxs-lookup"><span data-stu-id="f6811-132">Event Hubs Capture copies data directly from the internal Event Hubs storage, bypassing throughput unit egress quotas and saving your egress for other processing readers, such as Stream Analytics or Spark.</span></span>

<span data-ttu-id="f6811-133">Beállítása után Event Hubs rögzítése automatikusan fut, amikor az első esemény küldi el, és továbbra is futnak.</span><span class="sxs-lookup"><span data-stu-id="f6811-133">Once configured, Event Hubs Capture runs automatically when you send your first event, and continues running.</span></span> <span data-ttu-id="f6811-134">Könnyebben tudnia, hogy működik-e a folyamat az alárendelt feldolgozásra, az Event Hubs üres fájlok ír, amikor nincsenek adatok.</span><span class="sxs-lookup"><span data-stu-id="f6811-134">To make it easier for your downstream processing to know that the process is working, Event Hubs writes empty files when there is no data.</span></span> <span data-ttu-id="f6811-135">Ez a folyamat előre jelezhető ütemben történik és jelző, amely képes a kötegelt processzor biztosít.</span><span class="sxs-lookup"><span data-stu-id="f6811-135">This process provides a predictable cadence and marker that can feed your batch processors.</span></span>

## <a name="setting-up-event-hubs-capture"></a><span data-ttu-id="f6811-136">Event Hubs rögzítése beállítása</span><span class="sxs-lookup"><span data-stu-id="f6811-136">Setting up Event Hubs Capture</span></span>

<span data-ttu-id="f6811-137">Rögzítési konfigurálhatja a event hub létrehozási ideje az az [Azure-portálon](https://portal.azure.com), vagy az Azure Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="f6811-137">You can configure Capture at the event hub creation time using the [Azure portal](https://portal.azure.com), or using Azure Resource Manager templates.</span></span> <span data-ttu-id="f6811-138">További információkért tekintse át a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="f6811-138">For more information, see the following articles:</span></span>

- [<span data-ttu-id="f6811-139">Engedélyezze az Event Hubs rögzítheti az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="f6811-139">Enable Event Hubs Capture using the Azure portal</span></span>](event-hubs-capture-enable-through-portal.md)
- [<span data-ttu-id="f6811-140">Hozzon létre egy Event Hubs névtér eseményközpontban, és engedélyezze a rögzítést az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="f6811-140">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-the-captured-files-and-working-with-avro"></a><span data-ttu-id="f6811-141">A rögzített fájlok fel, és az Avro használata</span><span class="sxs-lookup"><span data-stu-id="f6811-141">Exploring the captured files and working with Avro</span></span>

<span data-ttu-id="f6811-142">Az Avro formátum, a megadott időszak megadott fájlok Event Hubs rögzítése hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6811-142">Event Hubs Capture creates files in Avro format, as specified on the configured time window.</span></span> <span data-ttu-id="f6811-143">Megtekintheti ezeket a fájlokat, mint bármely eszköz [Azure Tártallózó][Azure Storage Explorer].</span><span class="sxs-lookup"><span data-stu-id="f6811-143">You can view these files in any tool such as [Azure Storage Explorer][Azure Storage Explorer].</span></span> <span data-ttu-id="f6811-144">Letöltheti a fájlokat helyileg őket.</span><span class="sxs-lookup"><span data-stu-id="f6811-144">You can download the files locally to work on them.</span></span>

<span data-ttu-id="f6811-145">Az Event Hubs rögzítése által előállított fájlokat a következő az Avro-séma rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f6811-145">The files produced by Event Hubs Capture have the following Avro schema:</span></span>

![][3]

<span data-ttu-id="f6811-146">Egy egyszerű módja felfedezése, mely az Avro-fájlok a [Avro eszközök] [ Avro Tools] az Apache jar.</span><span class="sxs-lookup"><span data-stu-id="f6811-146">An easy way to explore Avro files is by using the [Avro Tools][Avro Tools] jar from Apache.</span></span> <span data-ttu-id="f6811-147">A jar a letöltés után megtekintheti a séma egy konkrét az Avro-fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f6811-147">After downloading this jar, you can see the schema of a specific Avro file by running the following command:</span></span>

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

<span data-ttu-id="f6811-148">Ez a parancs visszaadja</span><span class="sxs-lookup"><span data-stu-id="f6811-148">This command returns</span></span>

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

<span data-ttu-id="f6811-149">Az Avro eszközök segítségével a fájl konvertálása JSON formátumú, és végezze el a más feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="f6811-149">You can also use Avro Tools to convert the file to JSON format and perform other processing.</span></span>

<span data-ttu-id="f6811-150">Speciális feldolgozási műveleteket, töltse le és telepítse a Avro a választott platform számára.</span><span class="sxs-lookup"><span data-stu-id="f6811-150">To perform more advanced processing, download and install Avro for your choice of platform.</span></span> <span data-ttu-id="f6811-151">A cikk írásának időpontjában érhetők el megvalósítások C, C++, C\#, Java, NodeJS, Perl, PHP, Python vagy Ruby.</span><span class="sxs-lookup"><span data-stu-id="f6811-151">At the time of this writing, there are implementations available for C, C++, C\#, Java, NodeJS, Perl, PHP, Python, and Ruby.</span></span>

<span data-ttu-id="f6811-152">Apache Avro rendelkezik teljes bevezetés útmutatói [Java] [ Java] és [Python][Python].</span><span class="sxs-lookup"><span data-stu-id="f6811-152">Apache Avro has complete Getting Started guides for [Java][Java] and [Python][Python].</span></span> <span data-ttu-id="f6811-153">Is a [Bevezetés az Event Hubs rögzítése](event-hubs-capture-python.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f6811-153">You can also read the [Getting started with Event Hubs Capture](event-hubs-capture-python.md) article.</span></span>

## <a name="how-event-hubs-capture-is-charged"></a><span data-ttu-id="f6811-154">Hogyan Event Hubs rögzítése feladata</span><span class="sxs-lookup"><span data-stu-id="f6811-154">How Event Hubs Capture is charged</span></span>

<span data-ttu-id="f6811-155">Event Hubs rögzítése forgalmi díjas hasonlóan az átviteli egységek: mint egy óránkénti kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="f6811-155">Event Hubs Capture is metered similarly to throughput units: as an hourly charge.</span></span> <span data-ttu-id="f6811-156">Az elsők között közvetlenül a névtér megvásárolt átviteli egységek számával arányos.</span><span class="sxs-lookup"><span data-stu-id="f6811-156">The charge is directly proportional to the number of throughput units purchased for the namespace.</span></span> <span data-ttu-id="f6811-157">Átviteli egységek növelhető és csökkenthető, Event Hubs rögzítése mérőszámok növelheti és csökkentheti a megfelelő teljesítmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="f6811-157">As throughput units are increased and decreased, Event Hubs Capture meters increase and decrease to provide matching performance.</span></span> <span data-ttu-id="f6811-158">A mérőszámok párhuzamosan történik.</span><span class="sxs-lookup"><span data-stu-id="f6811-158">The meters occur in tandem.</span></span> <span data-ttu-id="f6811-159">Díjszabása, lásd: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="f6811-159">For pricing details, see [Event Hubs pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f6811-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6811-160">Next steps</span></span>

<span data-ttu-id="f6811-161">A legegyszerűbben az adatok lekérése az Azure Event Hubs rögzítése.</span><span class="sxs-lookup"><span data-stu-id="f6811-161">Event Hubs Capture is the easiest way to get data into Azure.</span></span> <span data-ttu-id="f6811-162">Az Azure Data Lake, az Azure Data Factory és az Azure HDInsight, kötegfeldolgozási hajthat végre, és más jól ismert eszközökkel és a platformok elemzés, bármilyen léptékben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f6811-162">Using Azure Data Lake, Azure Data Factory, and Azure HDInsight, you can perform batch processing and other analytics using familiar tools and platforms of your choosing, at any scale you need.</span></span>

<span data-ttu-id="f6811-163">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="f6811-163">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="f6811-164">Küldés és fogadás események első lépései</span><span class="sxs-lookup"><span data-stu-id="f6811-164">Get started sending and receiving events</span></span>](event-hubs-dotnet-framework-getstarted-send.md)
* <span data-ttu-id="f6811-165">[Az Event Hubsot használó teljes mintaalkalmazás][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="f6811-165">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs]</span></span>
* <span data-ttu-id="f6811-166">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="f6811-166">[Event Hubs overview][Event Hubs overview]</span></span>

[Apache Avro]: http://avro.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: http://www-us.apache.org/dist/avro/avro-1.8.2/java/avro-tools-1.8.2.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
