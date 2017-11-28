---
title: "az Azure Event Hubs rögzítése aaaOverview |} Microsoft Docs"
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
ms.openlocfilehash: 0238cae712a0ed7bdf3e87ee93a069a553cb65df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-event-hubs-capture"></a><span data-ttu-id="268b6-103">Az Azure Event Hubs rögzítése</span><span class="sxs-lookup"><span data-stu-id="268b6-103">Azure Event Hubs Capture</span></span>

<span data-ttu-id="268b6-104">Az Azure Event Hubs rögzítése lehetővé teszi a streamelési adatok az Event Hubs tooan tooautomatically kézbesítése hello [Azure Blob Storage tárolóban](https://azure.microsoft.com/services/storage/blobs/) vagy [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) fiók az Ön által választott, a hello további rugalmasságot egy alkalommal vagy méret időszakban megadására.</span><span class="sxs-lookup"><span data-stu-id="268b6-104">Azure Event Hubs Capture enables you tooautomatically deliver hello streaming data in Event Hubs tooan [Azure Blob storage](https://azure.microsoft.com/services/storage/blobs/) or [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) account of your choice, with hello added flexibility of specifying a time or size interval.</span></span> <span data-ttu-id="268b6-105">Rögzítési beállítása gyors, nem azt, és azt méretezi automatikusan az Event Hubs adminisztrációs költségek toorun van [átviteli egységek](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="268b6-105">Setting up Capture is fast, there are no administrative costs toorun it, and it scales automatically with Event Hubs [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="268b6-106">Event Hubs rögzítése hello streamelési adatok az Azure legegyszerűbb módja tooload, és lehetővé teszi az adatok feldolgozása és nem a rögzítés toofocus.</span><span class="sxs-lookup"><span data-stu-id="268b6-106">Event Hubs Capture is hello easiest way tooload streaming data into Azure, and enables you toofocus on data processing rather than on data capture.</span></span>

<span data-ttu-id="268b6-107">Event Hubs rögzítése lehetővé teszi a valós idejű tooprocess, és a batch-alapú folyamatok hello ugyanaz az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="268b6-107">Event Hubs Capture enables you tooprocess real-time and batch-based pipelines on hello same stream.</span></span> <span data-ttu-id="268b6-108">Ez azt jelenti, hogy idővel igényeknek nő megoldásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="268b6-108">This means you can build solutions that grow with your needs over time.</span></span> <span data-ttu-id="268b6-109">Batch-alapú rendszerek ma felé jövőbeli valós idejű feldolgozással követheti most felépítése, kívánó tooadd hatékony cold elérési tooan meglévő valós idejű megoldás, Event Hubs rögzítése révén könnyebben adatfolyam használata.</span><span class="sxs-lookup"><span data-stu-id="268b6-109">Whether you're building batch-based systems today with an eye towards future real-time processing, or you want tooadd an efficient cold path tooan existing real-time solution, Event Hubs Capture makes working with streaming data easier.</span></span>

## <a name="how-event-hubs-capture-works"></a><span data-ttu-id="268b6-110">Event Hubs rögzítése működése</span><span class="sxs-lookup"><span data-stu-id="268b6-110">How Event Hubs Capture works</span></span>

<span data-ttu-id="268b6-111">Az Event Hubs egy idő-megőrzési telemetriai érkező, hasonló tooa elosztott napló tartós puffer.</span><span class="sxs-lookup"><span data-stu-id="268b6-111">Event Hubs is a time-retention durable buffer for telemetry ingress, similar tooa distributed log.</span></span> <span data-ttu-id="268b6-112">hello kulcs tooscaling az Event Hubs hello [particionált felhasználói modell](event-hubs-features.md#partitions).</span><span class="sxs-lookup"><span data-stu-id="268b6-112">hello key tooscaling in Event Hubs is hello [partitioned consumer model](event-hubs-features.md#partitions).</span></span> <span data-ttu-id="268b6-113">Mindegyik partíció az adatok egy független szegmense, és egymástól függetlenül felhasznált.</span><span class="sxs-lookup"><span data-stu-id="268b6-113">Each partition is an independent segment of data and is consumed independently.</span></span> <span data-ttu-id="268b6-114">Ezek az adatok elavulnak ki idővel hello konfigurálható megőrzési időtartam alapján.</span><span class="sxs-lookup"><span data-stu-id="268b6-114">Over time this data ages off, based on hello configurable retention period.</span></span> <span data-ttu-id="268b6-115">Emiatt egy adott eseményközpont soha nem megtelik "túl."</span><span class="sxs-lookup"><span data-stu-id="268b6-115">As a result, a given event hub never gets "too full."</span></span>

<span data-ttu-id="268b6-116">Event Hubs rögzítése lehetővé teszi, hogy Ön toospecify a saját Azure Blob storage-fiók és a tároló vagy az Azure Data Lake Store-fiók, amelyek használt toostore hello rögzített adatok.</span><span class="sxs-lookup"><span data-stu-id="268b6-116">Event Hubs Capture enables you toospecify your own Azure Blob storage account and container, or Azure Data Lake Store account, which are used toostore hello captured data.</span></span> <span data-ttu-id="268b6-117">Ezek a fiókok hello lehet ugyanabban a régióban, az eseményközpont vagy egy másik régióban, toohello rugalmasságot hello Event Hubs rögzítése funkció hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="268b6-117">These accounts can be in hello same region as your event hub or in another region, adding toohello flexibility of hello Event Hubs Capture feature.</span></span>

<span data-ttu-id="268b6-118">A rögzített adatok [Apache Avro] [ Apache Avro] formátum: kompakt, gyors és bináris formátum, amely a beágyazott sémák gazdag adatstruktúrák biztosít.</span><span class="sxs-lookup"><span data-stu-id="268b6-118">Captured data is written in [Apache Avro][Apache Avro] format: a compact, fast, binary format that provides rich data structures with inline schema.</span></span> <span data-ttu-id="268b6-119">Ebben a formátumban széles körben használt hello Hadoop ökoszisztémájának, a Stream Analytics és az Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="268b6-119">This format is widely used in hello Hadoop ecosystem, Stream Analytics, and Azure Data Factory.</span></span> <span data-ttu-id="268b6-120">Az Avro kezelésével kapcsolatos további információk a cikk későbbi részében érhető el.</span><span class="sxs-lookup"><span data-stu-id="268b6-120">More information about working with Avro is available later in this article.</span></span>

### <a name="capture-windowing"></a><span data-ttu-id="268b6-121">Leképezési rögzítése</span><span class="sxs-lookup"><span data-stu-id="268b6-121">Capture windowing</span></span>

<span data-ttu-id="268b6-122">Event Hubs rögzítése lehetővé teszi, hogy a tooset fel egy ablak toocontrol rögzítése.</span><span class="sxs-lookup"><span data-stu-id="268b6-122">Event Hubs Capture enables you tooset up a window toocontrol capturing.</span></span> <span data-ttu-id="268b6-123">Ebben az ablakban egy minimális és időpontjának beállítása "első wins házirendnek,", amely hello első észlelt eseményindító okok rögzítési művelet jelentését.</span><span class="sxs-lookup"><span data-stu-id="268b6-123">This window is a minimum size and time configuration with a "first wins policy," meaning that hello first trigger encountered causes a capture operation.</span></span> <span data-ttu-id="268b6-124">Ha egy 15 perc, 100 MB Rögzítés ablak, és 1 MB protokollüzenetek másodpercenkénti, hello mérete ablak eseményindítók hello időszak előtt.</span><span class="sxs-lookup"><span data-stu-id="268b6-124">If you have a fifteen-minute, 100 MB capture window and send 1 MB per second, hello size window triggers before hello time window.</span></span> <span data-ttu-id="268b6-125">Mindegyik partíció egymástól függetlenül rögzíti, és befejezett blokkblob írja a rögzítést, hello időpontjában az nevű hello ideje, mely hello rögzítési időköz történt.</span><span class="sxs-lookup"><span data-stu-id="268b6-125">Each partition captures independently and writes a completed block blob at hello time of capture, named for hello time at which hello capture interval was encountered.</span></span> <span data-ttu-id="268b6-126">hello tároló elnevezési egyezmény a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="268b6-126">hello storage naming convention is as follows:</span></span>

```
[namespace]/[event hub]/[partition]/[YYYY]/[MM]/[DD]/[HH]/[mm]/[ss]
```

### <a name="scaling-toothroughput-units"></a><span data-ttu-id="268b6-127">Toothroughput méretezési egységek</span><span class="sxs-lookup"><span data-stu-id="268b6-127">Scaling toothroughput units</span></span>

<span data-ttu-id="268b6-128">Event Hubs forgalom által szabályozott [átviteli egységek](event-hubs-features.md#capacity).</span><span class="sxs-lookup"><span data-stu-id="268b6-128">Event Hubs traffic is controlled by [throughput units](event-hubs-features.md#capacity).</span></span> <span data-ttu-id="268b6-129">Egy átviteli egység lehetővé teszi, hogy 1 MB másodpercenkénti bemenő és kimenő forgalom kétszer adott mennyiségű második vagy 1000 esemény.</span><span class="sxs-lookup"><span data-stu-id="268b6-129">A single throughput unit allows 1 MB per second or 1000 events per second of ingress and twice that amount of egress.</span></span> <span data-ttu-id="268b6-130">Standard Event Hubs 1 – 20 átviteli egység konfigurálhatók, és vásárolhat további, a kvóta növeléséhez [támogatási kérelem][support request].</span><span class="sxs-lookup"><span data-stu-id="268b6-130">Standard Event Hubs can be configured with 1-20 throughput units, and you can purchase more with a quota increase [support request][support request].</span></span> <span data-ttu-id="268b6-131">Használat meghaladja a megvásárolt átviteli egységek folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="268b6-131">Usage beyond your purchased throughput units is throttled.</span></span> <span data-ttu-id="268b6-132">Event Hubs rögzítése másol adatokat közvetlenül hello belső Event Hubs tároló átviteli egység kilépő kvóták kihagyásával, és a kimenő forgalom mentése más feldolgozási olvasók, például a Stream Analytics vagy Spark.</span><span class="sxs-lookup"><span data-stu-id="268b6-132">Event Hubs Capture copies data directly from hello internal Event Hubs storage, bypassing throughput unit egress quotas and saving your egress for other processing readers, such as Stream Analytics or Spark.</span></span>

<span data-ttu-id="268b6-133">Beállítása után Event Hubs rögzítése automatikusan fut, amikor az első esemény küldi el, és továbbra is futnak.</span><span class="sxs-lookup"><span data-stu-id="268b6-133">Once configured, Event Hubs Capture runs automatically when you send your first event, and continues running.</span></span> <span data-ttu-id="268b6-134">toomake könnyebb az alárendelt feldolgozási tooknow hello folyamat működik, az Event Hubs a ír, üres fájlok amikor nincsenek adatok.</span><span class="sxs-lookup"><span data-stu-id="268b6-134">toomake it easier for your downstream processing tooknow that hello process is working, Event Hubs writes empty files when there is no data.</span></span> <span data-ttu-id="268b6-135">Ez a folyamat előre jelezhető ütemben történik és jelző, amely képes a kötegelt processzor biztosít.</span><span class="sxs-lookup"><span data-stu-id="268b6-135">This process provides a predictable cadence and marker that can feed your batch processors.</span></span>

## <a name="setting-up-event-hubs-capture"></a><span data-ttu-id="268b6-136">Event Hubs rögzítése beállítása</span><span class="sxs-lookup"><span data-stu-id="268b6-136">Setting up Event Hubs Capture</span></span>

<span data-ttu-id="268b6-137">Létrehozáskor hello event hub hello segítségével konfigurálhatja a rögzítési [Azure-portálon](https://portal.azure.com), vagy az Azure Resource Manager-sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="268b6-137">You can configure Capture at hello event hub creation time using hello [Azure portal](https://portal.azure.com), or using Azure Resource Manager templates.</span></span> <span data-ttu-id="268b6-138">További információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="268b6-138">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="268b6-139">Engedélyezze az Event Hubs rögzítése hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="268b6-139">Enable Event Hubs Capture using hello Azure portal</span></span>](event-hubs-capture-enable-through-portal.md)
- [<span data-ttu-id="268b6-140">Hozzon létre egy Event Hubs névtér eseményközpontban, és engedélyezze a rögzítést az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="268b6-140">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)

## <a name="exploring-hello-captured-files-and-working-with-avro"></a><span data-ttu-id="268b6-141">Hello rögzített fájlok fel, és az Avro használata</span><span class="sxs-lookup"><span data-stu-id="268b6-141">Exploring hello captured files and working with Avro</span></span>

<span data-ttu-id="268b6-142">Event Hubs rögzítése fájlokat hozza létre az Avro formátum megadott hello beállított időszak.</span><span class="sxs-lookup"><span data-stu-id="268b6-142">Event Hubs Capture creates files in Avro format, as specified on hello configured time window.</span></span> <span data-ttu-id="268b6-143">Megtekintheti ezeket a fájlokat, mint bármely eszköz [Azure Tártallózó][Azure Storage Explorer].</span><span class="sxs-lookup"><span data-stu-id="268b6-143">You can view these files in any tool such as [Azure Storage Explorer][Azure Storage Explorer].</span></span> <span data-ttu-id="268b6-144">Letöltheti a hello fájlok helyi toowork rajtuk.</span><span class="sxs-lookup"><span data-stu-id="268b6-144">You can download hello files locally toowork on them.</span></span>

<span data-ttu-id="268b6-145">Event Hubs rögzítése által előállított hello fájlok az Avro-séma a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="268b6-145">hello files produced by Event Hubs Capture have hello following Avro schema:</span></span>

![][3]

<span data-ttu-id="268b6-146">Egy egyszerű módot tooexplore az Avro-fájlok segítségével hello el [Avro eszközök] [ Avro Tools] az Apache jar.</span><span class="sxs-lookup"><span data-stu-id="268b6-146">An easy way tooexplore Avro files is by using hello [Avro Tools][Avro Tools] jar from Apache.</span></span> <span data-ttu-id="268b6-147">A jar a letöltés után megtekintheti egy konkrét Avro fájl hello séma hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="268b6-147">After downloading this jar, you can see hello schema of a specific Avro file by running hello following command:</span></span>

```
java -jar avro-tools-1.8.2.jar getschema <name of capture file>
```

<span data-ttu-id="268b6-148">Ez a parancs visszaadja</span><span class="sxs-lookup"><span data-stu-id="268b6-148">This command returns</span></span>

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

<span data-ttu-id="268b6-149">Is Avro eszközök tooconvert hello tooJSON formátumát használja, és hajtsa végre a más feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="268b6-149">You can also use Avro Tools tooconvert hello file tooJSON format and perform other processing.</span></span>

<span data-ttu-id="268b6-150">Speciális feldolgozása, letöltése és telepítése Avro a választott platform tooperform.</span><span class="sxs-lookup"><span data-stu-id="268b6-150">tooperform more advanced processing, download and install Avro for your choice of platform.</span></span> <span data-ttu-id="268b6-151">Hello írásának időpontjában, megvalósítások rendelkezésre állnak olyan C, C++, C\#, Java, NodeJS, Perl, PHP, Python vagy Ruby.</span><span class="sxs-lookup"><span data-stu-id="268b6-151">At hello time of this writing, there are implementations available for C, C++, C\#, Java, NodeJS, Perl, PHP, Python, and Ruby.</span></span>

<span data-ttu-id="268b6-152">Apache Avro rendelkezik teljes bevezetés útmutatói [Java] [ Java] és [Python][Python].</span><span class="sxs-lookup"><span data-stu-id="268b6-152">Apache Avro has complete Getting Started guides for [Java][Java] and [Python][Python].</span></span> <span data-ttu-id="268b6-153">Hello is olvasható [Bevezetés az Event Hubs rögzítése](event-hubs-capture-python.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="268b6-153">You can also read hello [Getting started with Event Hubs Capture](event-hubs-capture-python.md) article.</span></span>

## <a name="how-event-hubs-capture-is-charged"></a><span data-ttu-id="268b6-154">Hogyan Event Hubs rögzítése feladata</span><span class="sxs-lookup"><span data-stu-id="268b6-154">How Event Hubs Capture is charged</span></span>

<span data-ttu-id="268b6-155">Event Hubs rögzítése forgalmi díjas hasonlóképpen toothroughput egységek: mint egy óránkénti kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="268b6-155">Event Hubs Capture is metered similarly toothroughput units: as an hourly charge.</span></span> <span data-ttu-id="268b6-156">hello kell fizetni közvetlenül arányos toohello hello névtér megvásárolt átviteli egységek száma.</span><span class="sxs-lookup"><span data-stu-id="268b6-156">hello charge is directly proportional toohello number of throughput units purchased for hello namespace.</span></span> <span data-ttu-id="268b6-157">Átviteli egységek növelhető és csökkenthető, Event Hubs rögzítése mérőszámok növelheti és csökkentheti a megfelelő teljesítmény tooprovide.</span><span class="sxs-lookup"><span data-stu-id="268b6-157">As throughput units are increased and decreased, Event Hubs Capture meters increase and decrease tooprovide matching performance.</span></span> <span data-ttu-id="268b6-158">hello mérőszámok párhuzamosan történik.</span><span class="sxs-lookup"><span data-stu-id="268b6-158">hello meters occur in tandem.</span></span> <span data-ttu-id="268b6-159">Díjszabása, lásd: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="268b6-159">For pricing details, see [Event Hubs pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="268b6-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="268b6-160">Next steps</span></span>

<span data-ttu-id="268b6-161">Hello legegyszerűbb módja tooget adatokat az Azure Event Hubs rögzítése funkció.</span><span class="sxs-lookup"><span data-stu-id="268b6-161">Event Hubs Capture is hello easiest way tooget data into Azure.</span></span> <span data-ttu-id="268b6-162">Az Azure Data Lake, az Azure Data Factory és az Azure HDInsight, kötegfeldolgozási hajthat végre, és más jól ismert eszközökkel és a platformok elemzés, bármilyen léptékben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="268b6-162">Using Azure Data Lake, Azure Data Factory, and Azure HDInsight, you can perform batch processing and other analytics using familiar tools and platforms of your choosing, at any scale you need.</span></span>

<span data-ttu-id="268b6-163">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="268b6-163">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="268b6-164">Küldés és fogadás események első lépései</span><span class="sxs-lookup"><span data-stu-id="268b6-164">Get started sending and receiving events</span></span>](event-hubs-dotnet-framework-getstarted-send.md)
* <span data-ttu-id="268b6-165">[Az Event Hubsot használó teljes mintaalkalmazás][sample application that uses Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="268b6-165">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs]</span></span>
* <span data-ttu-id="268b6-166">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="268b6-166">[Event Hubs overview][Event Hubs overview]</span></span>

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
