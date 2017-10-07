---
title: "a queue storage és a Visual Studio (webjobs-feladat projektek) kapcsolódó szolgáltatások használatába aaaGetting |} Microsoft Docs"
description: "Hogyan tooget el az Azure Queue storage segítségével a webjobs-feladat projektben szolgáltatások csatlakozás a Visual Studio használatával tooa storage-fiók összekötése után."
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 47a446aa5c6bbf25526339823db4952ac1a8802f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="98d8c-103">Ismerkedés az Azure Queue storage és a Visual Studio csatlakoztatva (webjobs-feladat projektek) szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="98d8c-103">Getting started with Azure Queue storage and Visual Studio connected services (WebJob Projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="98d8c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="98d8c-104">Overview</span></span>
<span data-ttu-id="98d8c-105">Ez a cikk ismerteti, hogyan használatának az Azure Queue storage létrehozott vagy egy Azure storage-fiók használatával hivatkozott után a Visual Studio Azure webjobs-feladat projektben hello Visual Studio **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="98d8c-105">This article describes how get started using Azure Queue storage in a Visual Studio Azure WebJob project after you have created or referenced an Azure storage account by using hello Visual Studio  **Add Connected Services** dialog box.</span></span> <span data-ttu-id="98d8c-106">Ha a tárolási fiók tooa webjobs-feladat projekt hozzáadása hello Visual Studio használatával **kapcsolódó szolgáltatások hozzáadása** párbeszédpanelen hello megfelelő Azure Storage NuGet-csomagok vannak telepítve, és hello megfelelő .NET hivatkozások hozzáadott toohello projekt és hello tárfiók kapcsolati karakterláncok frissítése hello App.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="98d8c-106">When you add a storage account tooa WebJob project by using hello Visual Studio **Add Connected Services** dialog, hello appropriate Azure Storage NuGet packages are installed, hello appropriate .NET references are added toohello project, and connection strings for hello storage account are updated in hello App.config file.</span></span>  

<span data-ttu-id="98d8c-107">Ez a cikk ismerteti a C# mintakódok hogyan toouse hello Azure WebJobs SDK-verzió megjelenítése 1.x az Azure Queue storage szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="98d8c-107">This article provides C# code samples that show how toouse hello Azure WebJobs SDK version 1.x with hello Azure Queue storage service.</span></span>

<span data-ttu-id="98d8c-108">Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához.</span><span class="sxs-lookup"><span data-stu-id="98d8c-108">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="98d8c-109">Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="98d8c-109">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span> <span data-ttu-id="98d8c-110">Lásd: [Ismerkedés az Azure Queue Storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="98d8c-110">See [Get started with Azure Queue Storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information.</span></span> <span data-ttu-id="98d8c-111">ASP.NET kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="98d8c-111">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="how-tootrigger-a-function-when-a-queue-message-is-received"></a><span data-ttu-id="98d8c-112">Hogyan tootrigger a függvény egy várólista-üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="98d8c-112">How tootrigger a function when a queue message is received</span></span>
<span data-ttu-id="98d8c-113">toowrite egy függvényt, amely a WebJobs SDK hello meghívja a várólista-üzenet fogadásakor, használja a hello **QueueTrigger** attribútum.</span><span class="sxs-lookup"><span data-stu-id="98d8c-113">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello **QueueTrigger** attribute.</span></span> <span data-ttu-id="98d8c-114">hello attribútum konstruktora a hello várólista toopoll hello nevét megadó karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="98d8c-114">hello attribute constructor takes a string parameter that specifies hello name of hello queue toopoll.</span></span> <span data-ttu-id="98d8c-115">toosee hogyan tooset hello várólistacímke dinamikusan, tekintse meg [hogyan tooset konfigurációs beállítások](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="98d8c-115">toosee how tooset hello queue name dynamically, check out [How tooset Configuration Options](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="98d8c-116">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="98d8c-116">String queue messages</span></span>
<span data-ttu-id="98d8c-117">A következő példa hello, hello várólista tartalmaz egy karakterlánc-üzenet, így **QueueTrigger** alkalmazott tooa karakterlánc paraméter nevű **logMessage** tartalmazó hello hello várólista üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="98d8c-117">In hello following example, hello queue contains a string message, so **QueueTrigger** is applied tooa string parameter named **logMessage** which contains hello content of hello queue message.</span></span> <span data-ttu-id="98d8c-118">hello függvény [írja a naplót üzenet toohello irányítópult](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="98d8c-118">hello function [writes a log message toohello Dashboard](#how-to-write-logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="98d8c-119">Emellett **karakterlánc**, hello lehet, egy bájttömböt egy **CloudQueueMessage** objektumot, vagy egy Ön által meghatározott POCO.</span><span class="sxs-lookup"><span data-stu-id="98d8c-119">Besides **string**, hello parameter may be a byte array, a **CloudQueueMessage** object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="98d8c-120">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="98d8c-120">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="98d8c-121">A következő példa hello, várólista üdvözlőüzenetére tartalmazza a JSON a **BlobInformation** objektum, amely tartalmazza a **Blobnév** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="98d8c-121">In hello following example, hello queue message contains JSON for a **BlobInformation** object which includes a **BlobName** property.</span></span> <span data-ttu-id="98d8c-122">hello SDK automatikusan deserializes hello objektum.</span><span class="sxs-lookup"><span data-stu-id="98d8c-122">hello SDK automatically deserializes hello object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="98d8c-123">hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="98d8c-123">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="98d8c-124">Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="98d8c-124">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="98d8c-125">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="98d8c-125">Async functions</span></span>
<span data-ttu-id="98d8c-126">a következő aszinkron függvény hello [írja a naplót toohello irányítópult](#how-to-write-logs).</span><span class="sxs-lookup"><span data-stu-id="98d8c-126">hello following async function [writes a log toohello Dashboard](#how-to-write-logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="98d8c-127">Aszinkron funkciók is igénybe vehet egy [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), ahogy az alábbi példa, amely egy blob másolja hello.</span><span class="sxs-lookup"><span data-stu-id="98d8c-127">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in hello following example which copies a blob.</span></span> <span data-ttu-id="98d8c-128">(Hello előzetesben **queueTrigger** helyőrző, lásd: hello [Blobok](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) szakaszában.)</span><span class="sxs-lookup"><span data-stu-id="98d8c-128">(For an explanation of hello **queueTrigger** placeholder, see hello [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-hello-queuetrigger-attribute-works-with"></a><span data-ttu-id="98d8c-129">Típusok hello QueueTrigger attribútum együttműködik</span><span class="sxs-lookup"><span data-stu-id="98d8c-129">Types hello QueueTrigger attribute works with</span></span>
<span data-ttu-id="98d8c-130">Használhat **QueueTrigger** a következő típusok hello:</span><span class="sxs-lookup"><span data-stu-id="98d8c-130">You can use **QueueTrigger** with hello following types:</span></span>

* <span data-ttu-id="98d8c-131">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="98d8c-131">**string**</span></span>
* <span data-ttu-id="98d8c-132">A JSON-ként szerializált POCO típus</span><span class="sxs-lookup"><span data-stu-id="98d8c-132">A POCO type serialized as JSON</span></span>
* <span data-ttu-id="98d8c-133">**byte]**</span><span class="sxs-lookup"><span data-stu-id="98d8c-133">**byte[]**</span></span>
* <span data-ttu-id="98d8c-134">**CloudQueueMessage**</span><span class="sxs-lookup"><span data-stu-id="98d8c-134">**CloudQueueMessage**</span></span>

## <a name="polling-algorithm"></a><span data-ttu-id="98d8c-135">Lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="98d8c-135">Polling algorithm</span></span>
<span data-ttu-id="98d8c-136">hello SDK valósítja meg a tétlen-várólista tárolási tranzakciós költségeket a lekérdezési véletlenszerű exponenciális vissza az indító algoritmus tooreduce hello hatását.</span><span class="sxs-lookup"><span data-stu-id="98d8c-136">hello SDK implements a random exponential back-off algorithm tooreduce hello effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="98d8c-137">Ha üzenet található, hello SDK két másodpercet vár, és ellenőrzi, egy másik üzenet; Ha üzenet található azt, mielőtt újra próbálkozna körülbelül négy másodpercet vár.</span><span class="sxs-lookup"><span data-stu-id="98d8c-137">When a message is found, hello SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="98d8c-138">Későbbi sikertelen bejelentkezési kísérletek tooget egy üzenetsor-üzenetet, miután a hello várakozási idő tooincrease folytatódik, amíg eléri a maximális várakozási idő hello, mely alapértelmezett tooone perc.</span><span class="sxs-lookup"><span data-stu-id="98d8c-138">After subsequent failed attempts tooget a queue message, hello wait time continues tooincrease until it reaches hello maximum wait time, which defaults tooone minute.</span></span> <span data-ttu-id="98d8c-139">[hello maximális várakozási idő az konfigurálható](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="98d8c-139">[hello maximum wait time is configurable](#how-to-set-configuration-options).</span></span>

## <a name="multiple-instances"></a><span data-ttu-id="98d8c-140">Több példánya</span><span class="sxs-lookup"><span data-stu-id="98d8c-140">Multiple instances</span></span>
<span data-ttu-id="98d8c-141">Ha több példányt a webalkalmazás fut, a egy folyamatos webjobs-feladatok minden számítógépen fut, és minden gép fog Várjon, amíg az eseményindítók és toorun funkciók kísérlet.</span><span class="sxs-lookup"><span data-stu-id="98d8c-141">If your web app runs on multiple instances, a continuous WebJobs runs on each machine, and each machine will wait for triggers and attempt toorun functions.</span></span> <span data-ttu-id="98d8c-142">Ennek eredményeképpen előfordulhat, egyes esetekben feldolgozása toosome funkciók hello ugyanazokat az adatokat kétszer, ezért funkciók idempotent (úgy, hogy az ugyanazon bemeneti adatok nem eredményez hello ismételten hívása ismétlődő eredmények írása) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="98d8c-142">In some scenarios this can lead toosome functions processing hello same data twice, so functions should be idempotent (written so that calling them repeatedly with hello same input data doesn't produce duplicate results).</span></span>  

## <a name="parallel-execution"></a><span data-ttu-id="98d8c-143">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="98d8c-143">Parallel execution</span></span>
<span data-ttu-id="98d8c-144">Ha több funkciók különböző várólisták figyel, hello SDK fogja hívni azokat párhuzamosan üzenetek fogadása egy időben.</span><span class="sxs-lookup"><span data-stu-id="98d8c-144">If you have multiple functions listening on different queues, hello SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="98d8c-145">hello azonos érvényét veszti, ha több üzenetek fogadása az egy adott sorba.</span><span class="sxs-lookup"><span data-stu-id="98d8c-145">hello same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="98d8c-146">Alapértelmezés szerint hello SDK egyszerre az 16 várólista üzenetkötegek lekérdezi és feldolgozza azokat párhuzamosan hello függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="98d8c-146">By default, hello SDK gets a batch of 16 queue messages at a time and executes hello function that processes them in parallel.</span></span> <span data-ttu-id="98d8c-147">[hello köteg mérete konfigurálható](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="98d8c-147">[hello batch size is configurable](#how-to-set-configuration-options).</span></span> <span data-ttu-id="98d8c-148">Ha feldolgozott hello szám lefelé toohalf hello kötegelt méretű lekérdezi, hello SDK lekérdezi egy másik köteg, és megkezdi az üzenetek feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="98d8c-148">When hello number being processed gets down toohalf of hello batch size, hello SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="98d8c-149">Ezért hello maximális száma párhuzamos éppen feldolgozott üzeneteinek másodpercenkénti függvény, egy és egy félig alkalommal hello kötegmérete.</span><span class="sxs-lookup"><span data-stu-id="98d8c-149">Therefore hello maximum number of concurrent messages being processed per function is one and a half times hello batch size.</span></span> <span data-ttu-id="98d8c-150">Ez a korlátozás vonatkozik külön-külön tooeach függvény, amely rendelkezik egy **QueueTrigger** attribútum.</span><span class="sxs-lookup"><span data-stu-id="98d8c-150">This limit applies separately tooeach function that has a **QueueTrigger** attribute.</span></span> <span data-ttu-id="98d8c-151">Ha nem szeretné, egy olyan sort a fogadott üzenetet párhuzamos futtatáshoz, állítsa be a hello köteg mérete too1.</span><span class="sxs-lookup"><span data-stu-id="98d8c-151">If you don't want parallel execution for messages received on one queue, set hello batch size too1.</span></span>

## <a name="get-queue-or-queue-message-metadata"></a><span data-ttu-id="98d8c-152">Várólista vagy várólista üzenet metaadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="98d8c-152">Get queue or queue message metadata</span></span>
<span data-ttu-id="98d8c-153">Kaphat a következő üzenet tulajdonságai toohello metódus-aláírás paraméterek hozzáadásával hello:</span><span class="sxs-lookup"><span data-stu-id="98d8c-153">You can get hello following message properties by adding parameters toohello method signature:</span></span>

* <span data-ttu-id="98d8c-154">**DateTimeOffset** expirationTime</span><span class="sxs-lookup"><span data-stu-id="98d8c-154">**DateTimeOffset** expirationTime</span></span>
* <span data-ttu-id="98d8c-155">**DateTimeOffset** insertionTime</span><span class="sxs-lookup"><span data-stu-id="98d8c-155">**DateTimeOffset** insertionTime</span></span>
* <span data-ttu-id="98d8c-156">**DateTimeOffset** nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="98d8c-156">**DateTimeOffset** nextVisibleTime</span></span>
* <span data-ttu-id="98d8c-157">**karakterlánc** queueTrigger (tartalmazza a szöveges üzenet)</span><span class="sxs-lookup"><span data-stu-id="98d8c-157">**string** queueTrigger (contains message text)</span></span>
* <span data-ttu-id="98d8c-158">**karakterlánc** azonosítója</span><span class="sxs-lookup"><span data-stu-id="98d8c-158">**string** id</span></span>
* <span data-ttu-id="98d8c-159">**karakterlánc** popReceipt</span><span class="sxs-lookup"><span data-stu-id="98d8c-159">**string** popReceipt</span></span>
* <span data-ttu-id="98d8c-160">**int** dequeueCount</span><span class="sxs-lookup"><span data-stu-id="98d8c-160">**int** dequeueCount</span></span>

<span data-ttu-id="98d8c-161">Ha azt szeretné, közvetlenül az Azure storage API hello toowork, azt is megteheti egy **CloudStorageAccount** paraméter.</span><span class="sxs-lookup"><span data-stu-id="98d8c-161">If you want toowork directly with hello Azure storage API, you can also add a **CloudStorageAccount** parameter.</span></span>

<span data-ttu-id="98d8c-162">hello alábbi példa írja az összes a metaadatok tooan INFO alkalmazásnaplóban.</span><span class="sxs-lookup"><span data-stu-id="98d8c-162">hello following example writes all of this metadata tooan INFO application log.</span></span> <span data-ttu-id="98d8c-163">Hello példában logMessage és a queueTrigger tartalma hello hello várólista üzenet.</span><span class="sxs-lookup"><span data-stu-id="98d8c-163">In hello example, both logMessage and queueTrigger contain hello content of hello queue message.</span></span>

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

<span data-ttu-id="98d8c-164">Íme egy mintanaplót hello mintakód szerint:</span><span class="sxs-lookup"><span data-stu-id="98d8c-164">Here is a sample log written by hello sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a><span data-ttu-id="98d8c-165">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="98d8c-165">Graceful shutdown</span></span>
<span data-ttu-id="98d8c-166">Egy függvény a folyamatos webjobs-feladat fut fogad el egy **CancellationToken** paramétert, amely lehetővé teszi, hogy ha hello hello operációs rendszer toonotify hello függvény a webjobs-feladat megszakadt toobe tárgya.</span><span class="sxs-lookup"><span data-stu-id="98d8c-166">A function that runs in a continuous WebJob can accept a **CancellationToken** parameter which enables hello operating system toonotify hello function when hello WebJob is about toobe terminated.</span></span> <span data-ttu-id="98d8c-167">Az értesítési toomake meg arról, hogy hello függvény nem bontható váratlanul körbe, hogy adatok inkonzisztens állapotban is használhatja.</span><span class="sxs-lookup"><span data-stu-id="98d8c-167">You can use this notification toomake sure hello function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="98d8c-168">a következő példa azt mutatja meg hogyan hello toocheck függvények közelgő webjobs-feladat-záráshoz.</span><span class="sxs-lookup"><span data-stu-id="98d8c-168">hello following example shows how toocheck for impending WebJob termination in a function.</span></span>

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

<span data-ttu-id="98d8c-169">**Megjegyzés:** hello irányítópult előfordulhat, hogy helyesen jelenjen meg hello állapota és kimenete, amely le lett állítva a funkciókat.</span><span class="sxs-lookup"><span data-stu-id="98d8c-169">**Note:** hello Dashboard might not correctly show hello status and output of functions that have been shut down.</span></span>

<span data-ttu-id="98d8c-170">További információkért lásd: [webjobs-feladatok szabályos leállítást](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="98d8c-170">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <a name="how-toocreate-a-queue-message-while-processing-a-queue-message"></a><span data-ttu-id="98d8c-171">Hogyan toocreate várólista üzenet-várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="98d8c-171">How toocreate a queue message while processing a queue message</span></span>
<span data-ttu-id="98d8c-172">egy függvényt, amely létrehoz egy új sor üzenetet, használjon hello toowrite **várólista** attribútum.</span><span class="sxs-lookup"><span data-stu-id="98d8c-172">toowrite a function that creates a new queue message, use hello **Queue** attribute.</span></span> <span data-ttu-id="98d8c-173">Például **QueueTrigger**, adja meg a várólistacímke hello karakterláncból, vagy beállíthatja [hello várólista nevének beállítása dinamikusan](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="98d8c-173">Like **QueueTrigger**, you pass in hello queue name as a string or you can [set hello queue name dynamically](#how-to-set-configuration-options).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="98d8c-174">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="98d8c-174">String queue messages</span></span>
<span data-ttu-id="98d8c-175">a következő példakód nem aszinkron hello hello várólista "outputqueue" nevű új várólista üzenet ugyanaz tartalom másként hello üzenetsorban található üzenetet kapott hello várólista "inputqueue" nevű hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="98d8c-175">hello following non-async code sample creates a new queue message in hello queue named "outputqueue" with hello same content as hello queue message received in hello queue named "inputqueue".</span></span> <span data-ttu-id="98d8c-176">(Aszinkron funkciók használata **IAsyncCollector<T>**  később az itt látható módon.)</span><span class="sxs-lookup"><span data-stu-id="98d8c-176">(For async functions use **IAsyncCollector<T>** as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="98d8c-177">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="98d8c-177">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="98d8c-178">toocreate egy kimeneti paraméter toohello írja egy üzenetsor-üzenetet, amely tartalmazza a karakterláncot, pass hello POCO helyett egy POCO **várólista** attribútumok konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="98d8c-178">toocreate a queue message that contains a POCO rather than a string, pass hello POCO type as an output parameter toohello **Queue** attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="98d8c-179">hello SDK automatikusan hello objektum tooJSON rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="98d8c-179">hello SDK automatically serializes hello object tooJSON.</span></span> <span data-ttu-id="98d8c-180">Egy üzenetsor mindig létrejön, még akkor is, ha hello objektum értéke null.</span><span class="sxs-lookup"><span data-stu-id="98d8c-180">A queue message is always created, even if hello object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="98d8c-181">Hozzon létre több üzenetet vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="98d8c-181">Create multiple messages or in async functions</span></span>
<span data-ttu-id="98d8c-182">toocreate több üzenetet, hogy hello paramétertípus hello kimeneti várólista **ICollector<T>**  vagy **IAsyncCollector<T>**, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="98d8c-182">toocreate multiple messages, make hello parameter type for hello output queue **ICollector<T>** or **IAsyncCollector<T>**, as shown in hello following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="98d8c-183">Minden várólista-üzenet létrehozásakor a rendszer azonnal hello **Hozzáadás** metódust.</span><span class="sxs-lookup"><span data-stu-id="98d8c-183">Each queue message is created immediately when hello **Add** method is called.</span></span>

### <a name="types-that-hello-queue-attribute-works-with"></a><span data-ttu-id="98d8c-184">Típusok hello várólista attribútum, amely együttműködik</span><span class="sxs-lookup"><span data-stu-id="98d8c-184">Types that hello Queue attribute works with</span></span>
<span data-ttu-id="98d8c-185">Használhatja a hello **várólista** hello paramétereinek típusa a következő attribútumot:</span><span class="sxs-lookup"><span data-stu-id="98d8c-185">You can use hello **Queue** attribute on hello following parameter types:</span></span>

* <span data-ttu-id="98d8c-186">**kimenő karakterlánc** (hoz létre a üzenetsor-üzenetet, ha a paraméter értéke nem null értékű hello függvény végén)</span><span class="sxs-lookup"><span data-stu-id="98d8c-186">**out string** (creates queue message if parameter value is non-null when hello function ends)</span></span>
* <span data-ttu-id="98d8c-187">**kimenő byte []** (működik, mint az **karakterlánc**)</span><span class="sxs-lookup"><span data-stu-id="98d8c-187">**out byte[]** (works like **string**)</span></span>
* <span data-ttu-id="98d8c-188">**kimenő CloudQueueMessage** (működik, mint az **karakterlánc**)</span><span class="sxs-lookup"><span data-stu-id="98d8c-188">**out CloudQueueMessage** (works like **string**)</span></span>
* <span data-ttu-id="98d8c-189">**kimenő POCO** (szerializálható típust, létrehoz egy üzenetet null objektummal rendelkező Ha hello paraméterének értéke null, ha hello függvény karakterlánccal végződik-e)</span><span class="sxs-lookup"><span data-stu-id="98d8c-189">**out POCO** (a serializable type, creates a message with a null object if hello paramter is null when hello function ends)</span></span>
* <span data-ttu-id="98d8c-190">**ICollector**</span><span class="sxs-lookup"><span data-stu-id="98d8c-190">**ICollector**</span></span>
* <span data-ttu-id="98d8c-191">**IAsyncCollector**</span><span class="sxs-lookup"><span data-stu-id="98d8c-191">**IAsyncCollector**</span></span>
* <span data-ttu-id="98d8c-192">**CloudQueue** (üzenetek manuális létrehozásához használatával hello Azure Storage API közvetlenül)</span><span class="sxs-lookup"><span data-stu-id="98d8c-192">**CloudQueue** (for creating messages manually using hello Azure Storage API directly)</span></span>

### <a name="use-webjobs-sdk-attributes-in-hello-body-of-a-function"></a><span data-ttu-id="98d8c-193">Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="98d8c-193">Use WebJobs SDK attributes in hello body of a function</span></span>
<span data-ttu-id="98d8c-194">Ha toodo kell néhány működik a függvény a WebJobs SDK attribútum használatához **várólista**, **Blob**, vagy **tábla**, használhatja a hello **IBinder** felületet.</span><span class="sxs-lookup"><span data-stu-id="98d8c-194">If you need toodo some work in your function before using a WebJobs SDK attribute such as **Queue**, **Blob**, or **Table**, you can use hello **IBinder** interface.</span></span>

<span data-ttu-id="98d8c-195">a következő példa hello időt vesz igénybe egy bemeneti várólista-üzenet, és hoz létre egy új üzenet hello azonos kimeneti várólistában lévő tartalom.</span><span class="sxs-lookup"><span data-stu-id="98d8c-195">hello following example takes an input queue message and creates a new message with hello same content in an output queue.</span></span> <span data-ttu-id="98d8c-196">hello kimeneti várólista neve hello függvénytörzs hello kóddal van beállítva.</span><span class="sxs-lookup"><span data-stu-id="98d8c-196">hello output queue name is set by code in hello body of hello function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="98d8c-197">Hello **IBinder** felület is használható hello **tábla** és **Blob** attribútumok.</span><span class="sxs-lookup"><span data-stu-id="98d8c-197">hello **IBinder** interface can also be used with hello **Table** and **Blob** attributes.</span></span>

## <a name="how-tooread-and-write-blobs-and-tables-while-processing-a-queue-message"></a><span data-ttu-id="98d8c-198">Hogyan tooread és írási blobok és táblák egy várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="98d8c-198">How tooread and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="98d8c-199">Hello **Blob** és **tábla** attribútumok lehetővé teszik a tooread, és írja blobok és táblákat.</span><span class="sxs-lookup"><span data-stu-id="98d8c-199">hello **Blob** and **Table** attributes enable you tooread and write blobs and tables.</span></span> <span data-ttu-id="98d8c-200">Ebben a szakaszban hello minták tooblobs alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="98d8c-200">hello samples in this section apply tooblobs.</span></span> <span data-ttu-id="98d8c-201">Hogyan tootrigger dolgozza fel, ha a blobok létrehozott vagy frissített megjelenítése mintakódok, lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), és mintakódok olvasási és írási táblák, lásd: [hogyan toouse Azure-tábla tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="98d8c-201">For code samples that show how tootrigger processes when blobs are created or updated, see [How toouse Azure blob storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How toouse Azure table storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="98d8c-202">Karakterlánc üzenetek blob műveletek időt.</span><span class="sxs-lookup"><span data-stu-id="98d8c-202">String queue messages triggering blob operations</span></span>
<span data-ttu-id="98d8c-203">Egy karakterláncot tartalmazó várólista üzenet **queueTrigger** hello használható helyőrző **Blob** attribútum **blobPath** hello tartalmát tartalmazó paraméter üdvözlőüzenetére.</span><span class="sxs-lookup"><span data-stu-id="98d8c-203">For a queue message that contains a string, **queueTrigger** is a placeholder you can use in hello **Blob** attribute's **blobPath** parameter that contains hello contents of hello message.</span></span>

<span data-ttu-id="98d8c-204">hello alábbi példában **adatfolyam** blobok tooread és írási objektumokat.</span><span class="sxs-lookup"><span data-stu-id="98d8c-204">hello following example uses **Stream** objects tooread and write blobs.</span></span> <span data-ttu-id="98d8c-205">várólista üdvözlőüzenetére hello textblobs tárolóban lévő blob hello neve.</span><span class="sxs-lookup"><span data-stu-id="98d8c-205">hello queue message is hello name of a blob located in hello textblobs container.</span></span> <span data-ttu-id="98d8c-206">Hello blob másolatát "-új" hozzáfűzött toohello neve jön létre az hello azonos tároló.</span><span class="sxs-lookup"><span data-stu-id="98d8c-206">A copy of hello blob with "-new" appended toohello name is created in hello same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="98d8c-207">Hello **Blob** attribútum konstruktora vesz egy **blobPath** paraméter meghatározza, hogy hello tároló és a blob neve.</span><span class="sxs-lookup"><span data-stu-id="98d8c-207">hello **Blob** attribute constructor takes a **blobPath** parameter that specifies hello container and blob name.</span></span> <span data-ttu-id="98d8c-208">A helyőrző kapcsolatos további információkért lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="98d8c-208">For more information about this placeholder, see [How toouse Azure blob storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).</span></span>

<span data-ttu-id="98d8c-209">Ha hello attribútum decorates egy **adatfolyam** objektumot egy másik konstruktor paraméterrel hello **FileAccess** olvasási, írási vagy olvasási/írási módban.</span><span class="sxs-lookup"><span data-stu-id="98d8c-209">When hello attribute decorates a **Stream** object, another constructor parameter specifies hello **FileAccess** mode as read, write, or read/write.</span></span>

<span data-ttu-id="98d8c-210">hello alábbi példában egy **CloudBlockBlob** toodelete blob objektum.</span><span class="sxs-lookup"><span data-stu-id="98d8c-210">hello following example uses a **CloudBlockBlob** object toodelete a blob.</span></span> <span data-ttu-id="98d8c-211">várólista üdvözlőüzenetére hello blob hello neve.</span><span class="sxs-lookup"><span data-stu-id="98d8c-211">hello queue message is hello name of hello blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="98d8c-212">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="98d8c-212">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="98d8c-213">Egy POCO hello üzenetsor JSON-ként tárolja, használhatja, amely hello hello objektumának neve helyőrzők **várólista** attribútum **blobPath** paraméter.</span><span class="sxs-lookup"><span data-stu-id="98d8c-213">For a POCO stored as JSON in hello queue message, you can use placeholders that name properties of hello object in hello **Queue** attribute's **blobPath** parameter.</span></span> <span data-ttu-id="98d8c-214">Metaadatok tulajdonság Várólistanevek helyőrzőként is használhatja.</span><span class="sxs-lookup"><span data-stu-id="98d8c-214">You can also use queue metadata property names as placeholders.</span></span> <span data-ttu-id="98d8c-215">Lásd: [várólista vagy várólista üzenet metaadatok](#get-queue-or-queue-message-metadata).</span><span class="sxs-lookup"><span data-stu-id="98d8c-215">See [Get queue or queue message metadata](#get-queue-or-queue-message-metadata).</span></span>

<span data-ttu-id="98d8c-216">hello alábbi példa másolja át a blob tooa új blob eltérő kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="98d8c-216">hello following example copies a blob tooa new blob with a different extension.</span></span> <span data-ttu-id="98d8c-217">hello várólista az üzenet egy **BlobInformation** , amely tartalmazza az objektum **Blobnév** és **BlobNameWithoutExtension** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="98d8c-217">hello queue message is a **BlobInformation** object that includes **BlobName** and **BlobNameWithoutExtension** properties.</span></span> <span data-ttu-id="98d8c-218">hello tulajdonságnevek helyőrzőként hello blob elérési út a hello **Blob** attribútumok.</span><span class="sxs-lookup"><span data-stu-id="98d8c-218">hello property names are used as placeholders in hello blob path for hello **Blob** attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="98d8c-219">hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="98d8c-219">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="98d8c-220">Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="98d8c-220">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="98d8c-221">Ha toodo néhány működik a függvény egy blob tooan objektum kötés előtt, használható hello attribútum hello függvénytörzs hello, ahogy az [egy függvény törzséhez hello használata a WebJobs SDK attribútumokat](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span><span class="sxs-lookup"><span data-stu-id="98d8c-221">If you need toodo some work in your function before binding a blob tooan object, you can use hello attribute in hello body of hello function, as shown in [Use WebJobs SDK attributes in hello body of a function](#use-webjobs-sdk-attributes-in-the-body-of-a-function).</span></span>

### <a name="types-you-can-use-hello-blob-attribute-with"></a><span data-ttu-id="98d8c-222">Adattípusok hello Blob-attribútum</span><span class="sxs-lookup"><span data-stu-id="98d8c-222">Types you can use hello Blob attribute with</span></span>
<span data-ttu-id="98d8c-223">Hello **Blob** attribútum is használható a következő típusok hello:</span><span class="sxs-lookup"><span data-stu-id="98d8c-223">hello **Blob** attribute can be used with hello following types:</span></span>

* <span data-ttu-id="98d8c-224">**Az adatfolyam** (olvasási vagy írási, hello FileAccess konstruktor paraméterrel megadott)</span><span class="sxs-lookup"><span data-stu-id="98d8c-224">**Stream** (read or write, specified by using hello FileAccess constructor parameter)</span></span>
* <span data-ttu-id="98d8c-225">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="98d8c-225">**TextReader**</span></span>
* <span data-ttu-id="98d8c-226">**TextWriter**</span><span class="sxs-lookup"><span data-stu-id="98d8c-226">**TextWriter**</span></span>
* <span data-ttu-id="98d8c-227">**karakterlánc** (olvasni.)</span><span class="sxs-lookup"><span data-stu-id="98d8c-227">**string** (read)</span></span>
* <span data-ttu-id="98d8c-228">**kimenő karakterlánc** (írási; hoz létre egy blobot, csak ha hello karakterlánc-paraméter null értékű hello függvény)</span><span class="sxs-lookup"><span data-stu-id="98d8c-228">**out string** (write; creates a blob only if hello string parameter is non-null when hello function returns)</span></span>
* <span data-ttu-id="98d8c-229">POCO (olvasás)</span><span class="sxs-lookup"><span data-stu-id="98d8c-229">POCO (read)</span></span>
* <span data-ttu-id="98d8c-230">kimenő POCO (írás; mindig létrehoz egy blob, mint null objektumot hoz létre, ha POCO paraméter értéke null, ha hello függvény)</span><span class="sxs-lookup"><span data-stu-id="98d8c-230">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when hello function returns)</span></span>
* <span data-ttu-id="98d8c-231">**CloudBlobStream** (írás)</span><span class="sxs-lookup"><span data-stu-id="98d8c-231">**CloudBlobStream** (write)</span></span>
* <span data-ttu-id="98d8c-232">**ICloudBlob** (olvasása vagy írása)</span><span class="sxs-lookup"><span data-stu-id="98d8c-232">**ICloudBlob** (read or write)</span></span>
* <span data-ttu-id="98d8c-233">**CloudBlockBlob** (olvasása vagy írása)</span><span class="sxs-lookup"><span data-stu-id="98d8c-233">**CloudBlockBlob** (read or write)</span></span>
* <span data-ttu-id="98d8c-234">**CloudPageBlob** (olvasása vagy írása)</span><span class="sxs-lookup"><span data-stu-id="98d8c-234">**CloudPageBlob** (read or write)</span></span>

## <a name="how-toohandle-poison-messages"></a><span data-ttu-id="98d8c-235">Hogyan toohandle elhalt üzenetek</span><span class="sxs-lookup"><span data-stu-id="98d8c-235">How toohandle poison messages</span></span>
<span data-ttu-id="98d8c-236">Neve üzeneteket, amelyek tartalmát a függvény toofail hatására *elhalt üzenetek*.</span><span class="sxs-lookup"><span data-stu-id="98d8c-236">Messages whose content causes a function toofail are called *poison messages*.</span></span> <span data-ttu-id="98d8c-237">Hello függvény meghibásodása esetén üdvözlőüzenetére várólista nem törlődik, és végül van felvételre újra, ezért hello ciklus toobe ismétlődik.</span><span class="sxs-lookup"><span data-stu-id="98d8c-237">When hello function fails, hello queue message is not deleted and eventually is picked up again, causing hello cycle toobe repeated.</span></span> <span data-ttu-id="98d8c-238">hello SDK automatikusan megszakíthatja hello ciklus korlátozott számú ismétlés után, vagy manuálisan is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="98d8c-238">hello SDK can automatically interrupt hello cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="98d8c-239">Automatikus elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="98d8c-239">Automatic poison message handling</span></span>
<span data-ttu-id="98d8c-240">hello SDK telefonhívásokhoz too5 alkalommal tooprocess egy üzenetsor be egy olyan függvényt.</span><span class="sxs-lookup"><span data-stu-id="98d8c-240">hello SDK will call a function up too5 times tooprocess a queue message.</span></span> <span data-ttu-id="98d8c-241">Hello ötödik próbálja meghibásodásakor hello üzenet áthelyezett tooa poison várólista.</span><span class="sxs-lookup"><span data-stu-id="98d8c-241">If hello fifth try fails, hello message is moved tooa poison queue.</span></span> <span data-ttu-id="98d8c-242">Láthatja, hogyan tooconfigure hello az újrapróbálkozások maximális száma [hogyan tooset konfigurációs beállítások](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="98d8c-242">You can see how tooconfigure hello maximum number of retries in [How tooset configuration options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="98d8c-243">hello poison várólista nevű *{originalqueuename}*-elhalt.</span><span class="sxs-lookup"><span data-stu-id="98d8c-243">hello poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="98d8c-244">Írhat egy függvény tooprocess üzenetek hello poison várólistából naplózásukhoz vagy, hogy szükség van-e a kézi beavatkozást értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="98d8c-244">You can write a function tooprocess messages from hello poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="98d8c-245">A következő példa hello hello **CopyBlob** függvény sikertelenek lesznek, amikor egy üzenetsor-üzenetet, amely nem létezik blob hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="98d8c-245">In hello following example hello **CopyBlob** function will fail when a queue message contains hello name of a blob that doesn't exist.</span></span> <span data-ttu-id="98d8c-246">Ebben az esetben üdvözlőüzenetére áthelyeznek hello copyblobqueue várólista toohello copyblobqueue-poison várólista.</span><span class="sxs-lookup"><span data-stu-id="98d8c-246">When that happens, hello message is moved from hello copyblobqueue queue toohello copyblobqueue-poison queue.</span></span> <span data-ttu-id="98d8c-247">Hello **ProcessPoisonMessage** majd naplók hello az elhalt üzenet.</span><span class="sxs-lookup"><span data-stu-id="98d8c-247">hello **ProcessPoisonMessage** then logs hello poison message.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed toocopy blob, name=" + blobName);
        }

<span data-ttu-id="98d8c-248">hello alábbi ábrán az alábbi funkciók a konzol kimeneti az elhalt üzenet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="98d8c-248">hello following illustration shows console output from these functions when a poison message is processed.</span></span>

![Elhalt üzenetek kezelésének a konzol kimeneti](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="98d8c-250">Manuális elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="98d8c-250">Manual poison message handling</span></span>
<span data-ttu-id="98d8c-251">Hello szám, ahányszor egy üzenet felvételre feldolgozásra kaphat hozzáadásával egy **int** nevű paraméter **dequeueCount** tooyour függvény.</span><span class="sxs-lookup"><span data-stu-id="98d8c-251">You can get hello number of times a message has been picked up for processing by adding an **int** parameter named **dequeueCount** tooyour function.</span></span> <span data-ttu-id="98d8c-252">Ezután a jelölőnégyzet hello created funkciókódot száma, és hajtsa végre a saját elhalt üzenetek kezelésének amikor hello száma meghaladja a küszöbértéket, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="98d8c-252">You can then check hello dequeue count in function code and perform your own poison message handling when hello number exceeds a threshold, as shown in hello following example.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed toocopy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-tooset-configuration-options"></a><span data-ttu-id="98d8c-253">Hogyan tooset konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="98d8c-253">How tooset configuration options</span></span>
<span data-ttu-id="98d8c-254">Használhatja a hello **JobHostConfiguration** típus tooset hello alábbi konfigurációs beállítások:</span><span class="sxs-lookup"><span data-stu-id="98d8c-254">You can use hello **JobHostConfiguration** type tooset hello following configuration options:</span></span>

* <span data-ttu-id="98d8c-255">Hello SDK kapcsolati karakterláncok beállítása a kódban.</span><span class="sxs-lookup"><span data-stu-id="98d8c-255">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="98d8c-256">Konfigurálása **QueueTrigger** beállításokat, például a maximális created száma.</span><span class="sxs-lookup"><span data-stu-id="98d8c-256">Configure **QueueTrigger** settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="98d8c-257">Várólistanevek konfigurációs beolvasása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="98d8c-257">Get queue names from configuration.</span></span>

### <a name="set-sdk-connection-strings-in-code"></a><span data-ttu-id="98d8c-258">A kód SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="98d8c-258">Set SDK connection strings in code</span></span>
<span data-ttu-id="98d8c-259">Hello SDK kapcsolati karakterláncok beállítása a kód lehetővé teszi, hogy Ön toouse saját kapcsolódási karakterlánc neve, a konfigurációs fájlok vagy a környezeti változók, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="98d8c-259">Setting hello SDK connection strings in code enables you toouse your own connection string names in configuration files or environment variables, as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a><span data-ttu-id="98d8c-260">QueueTrigger beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="98d8c-260">Configure QueueTrigger  settings</span></span>
<span data-ttu-id="98d8c-261">Hello következő toohello várólista üzenetfeldolgozást vonatkozó beállításokat konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="98d8c-261">You can configure hello following settings that apply toohello queue message processing:</span></span>

* <span data-ttu-id="98d8c-262">hello egyidejűleg végre párhuzamosan toobe átveszik várólista üzenetek maximális száma (alapértelmezett érték 16).</span><span class="sxs-lookup"><span data-stu-id="98d8c-262">hello maximum number of queue messages that are picked up simultaneously toobe executed in parallel (default is 16).</span></span>
* <span data-ttu-id="98d8c-263">Az újrapróbálkozások maximális számát hello tooa poison várólista várólista üzenet elküldése előtt (alapértelmezett érték 5).</span><span class="sxs-lookup"><span data-stu-id="98d8c-263">hello maximum number of retries before a queue message is sent tooa poison queue (default is 5).</span></span>
* <span data-ttu-id="98d8c-264">hello maximális várakozási idő előtt lekérdezési újra, ha üres-e a várólista (alapértelmezett érték 1 perc).</span><span class="sxs-lookup"><span data-stu-id="98d8c-264">hello maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="98d8c-265">a következő példa azt mutatja meg hogyan hello tooconfigure ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="98d8c-265">hello following example shows how tooconfigure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a><span data-ttu-id="98d8c-266">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="98d8c-266">Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="98d8c-267">A várólista nevét, a blob neve vagy a tároló néha szeretné toospecify, vagy egy táblázatot a merevlemez-kód helyett a kód adjon neki nevet.</span><span class="sxs-lookup"><span data-stu-id="98d8c-267">Sometimes you want toospecify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="98d8c-268">Például érdemes toospecify hello várólista nevét **QueueTrigger** a konfigurációs fájl vagy a környezeti változóban.</span><span class="sxs-lookup"><span data-stu-id="98d8c-268">For example, you might want toospecify hello queue name for **QueueTrigger** in a configuration file or environment variable.</span></span>

<span data-ttu-id="98d8c-269">A történő ehhez a **NameResolver** toohello objektum **JobHostConfiguration** típusa.</span><span class="sxs-lookup"><span data-stu-id="98d8c-269">You can do that by passing in a **NameResolver** object toohello **JobHostConfiguration** type.</span></span> <span data-ttu-id="98d8c-270">Ön százalékjel (%) jelentkezik be a WebJobs SDK attribútum konstruktorparaméterek körülvett különleges helyőrzőket tartalmaznak, és a **NameResolver** kód hello tényleges értékek toobe használja ezeket a helyőrzők helyett határozza meg.</span><span class="sxs-lookup"><span data-stu-id="98d8c-270">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your **NameResolver** code specifies hello actual values toobe used in place of those placeholders.</span></span>

<span data-ttu-id="98d8c-271">Tegyük fel, hogy azt szeretné, hogy toouse várólista neve például hello tesztkörnyezetben logqueuetest és egy elnevezett logqueueprod éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="98d8c-271">For example, suppose you want toouse a queue named logqueuetest in hello test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="98d8c-272">A kódolt várólista neve helyett toospecify hello név egyik bejegyzésének hello kívánt **appSettings** gyűjteményt, amely rendelkezik hello tényleges várólista neve.</span><span class="sxs-lookup"><span data-stu-id="98d8c-272">Instead of a hard-coded queue name, you want toospecify hello name of an entry in hello **appSettings** collection that would have hello actual queue name.</span></span> <span data-ttu-id="98d8c-273">Ha hello **appSettings** kulcs logqueue, a függvény a következő példa hello nézhet.</span><span class="sxs-lookup"><span data-stu-id="98d8c-273">If hello **appSettings** key is logqueue, your function could look like hello following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="98d8c-274">A **NameResolver** osztály majd sikerült hello várólista nevét a **appSettings** a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="98d8c-274">Your **NameResolver** class could then get hello queue name from **appSettings** as shown in hello following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="98d8c-275">Hello át **NameResolver** toohello osztály **JobHost** objektumot, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="98d8c-275">You pass hello **NameResolver** class in toohello **JobHost** object as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="98d8c-276">**Megjegyzés:** várólista, a táblának és a blob nevének feloldása minden alkalommal, amikor egy függvény hívása esetén, de a blob-tároló nevének feloldása csak akkor, ha hello alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="98d8c-276">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when hello application starts.</span></span> <span data-ttu-id="98d8c-277">A blob-tároló neve nem módosítható, hello feladat futása közben.</span><span class="sxs-lookup"><span data-stu-id="98d8c-277">You can't change blob container name while hello job is running.</span></span>

## <a name="how-tootrigger-a-function-manually"></a><span data-ttu-id="98d8c-278">Hogyan tootrigger függvény manuálisan</span><span class="sxs-lookup"><span data-stu-id="98d8c-278">How tootrigger a function manually</span></span>
<span data-ttu-id="98d8c-279">egy függvény tootrigger manuálisan, használja a hello **hívás** vagy **CallAsync** hello metódusa **JobHost** objektum és hello **NoAutomaticTrigger** hello függvény, ahogy az alábbi példa hello attribútum.</span><span class="sxs-lookup"><span data-stu-id="98d8c-279">tootrigger a function manually, use hello **Call** or **CallAsync** method on hello **JobHost** object and hello **NoAutomaticTrigger** attribute on hello function, as shown in hello following example.</span></span>

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-toowrite-logs"></a><span data-ttu-id="98d8c-280">Hogyan naplózza az toowrite</span><span class="sxs-lookup"><span data-stu-id="98d8c-280">How toowrite logs</span></span>
<span data-ttu-id="98d8c-281">hello irányítópulton látható naplók két helyen: hello webjobs-feladat hello lapját, és egy adott webjobs-feladat meghíváshoz hello lap.</span><span class="sxs-lookup"><span data-stu-id="98d8c-281">hello Dashboard shows logs in two places: hello page for hello WebJob, and hello page for a particular WebJob invocation.</span></span>

![Naplózza a webjobs-feladat lap](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Naplók függvény meghívása lap](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="98d8c-284">Egy függvény vagy hello hívó konzol módszerek kimenete **Main()** metódus a hello irányítópult-oldalon hello webjobs-feladat, és nem egy adott metódushívás hello oldalán megjelenik.</span><span class="sxs-lookup"><span data-stu-id="98d8c-284">Output from Console methods that you call in a function or in hello **Main()** method appears in hello Dashboard page for hello WebJob, not in hello page for a particular method invocation.</span></span> <span data-ttu-id="98d8c-285">Hello TextWriter objektum, hogy a paraméter a a metódus aláírásában kimenete hello irányítópult-oldalon metódushívási jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98d8c-285">Output from hello TextWriter object that you get from a parameter in your method signature appears in hello Dashboard page for a method invocation.</span></span>

<span data-ttu-id="98d8c-286">Konzol kimeneti nem lehet csatolt tooa adott metódushívás mert hello konzol egyszálas, miközben sok munkakörök futtathatnak: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="98d8c-286">Console output can't be linked tooa particular method invocation because hello Console is single-threaded, while many job functions may be running at hello same time.</span></span> <span data-ttu-id="98d8c-287">Ezért hello SDK biztosít minden függvény meghívása a saját egyedi napló író objektummal.</span><span class="sxs-lookup"><span data-stu-id="98d8c-287">That's why hello  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="98d8c-288">toowrite [alkalmazás nyomkövetési naplóit](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), használjon **Console.Out** (INFO jelölésű naplókat hoz létre) és **Console.Error** (a hiba jelöléssel naplókat hoz létre).</span><span class="sxs-lookup"><span data-stu-id="98d8c-288">toowrite [application tracing logs](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use **Console.Out** (creates logs marked as INFO) and **Console.Error** (creates logs marked as ERROR).</span></span> <span data-ttu-id="98d8c-289">A másik lehetőség az toouse [nyomkövetési vagy TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), amely biztosítja, hogy részletes, figyelmeztetés, és kritikus szintek hozzáadása tooInfo és a hiba.</span><span class="sxs-lookup"><span data-stu-id="98d8c-289">An alternative is toouse [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition tooInfo and Error.</span></span> <span data-ttu-id="98d8c-290">Alkalmazás nyomkövetési naplóit hello webes alkalmazások naplófájljainak, Azure-táblákban, jelenik meg, vagy az Azure-blobok attól függően, hogy hogyan konfigurálja az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="98d8c-290">Application tracing logs appear in hello web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="98d8c-291">Mivel minden a konzol kimeneti értéke igaz, hello legutóbbi 100 alkalmazásnaplók a hello irányítópult-oldalon hello webjobs-feladat, a függvény hívása nem hello lapján is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="98d8c-291">As is true of all Console output, hello most recent 100 application logs also appear in hello Dashboard page for hello WebJob, not hello page for a function invocation.</span></span>

<span data-ttu-id="98d8c-292">Konzol eredmény jelenik meg, csak akkor, ha a hello program fut az Azure webjobs-feladat, nem hello program helyileg fut. Ha irányítópult hello vagy néhány más környezetben.</span><span class="sxs-lookup"><span data-stu-id="98d8c-292">Console output appears in hello Dashboard only if hello program is running in an Azure WebJob, not if hello program is running locally or in some other environment.</span></span>

<span data-ttu-id="98d8c-293">Hello irányítópult kapcsolati karakterlánc toonull beállításával letiltani a naplózást.</span><span class="sxs-lookup"><span data-stu-id="98d8c-293">You can disable logging by setting hello Dashboard connection string toonull.</span></span> <span data-ttu-id="98d8c-294">További információkért lásd: [hogyan tooset konfigurációs beállítások](#how-to-set-configuration-options).</span><span class="sxs-lookup"><span data-stu-id="98d8c-294">For more information, see [How tooset Configuration Options](#how-to-set-configuration-options).</span></span>

<span data-ttu-id="98d8c-295">hello következő példa bemutatja, számos módon toowrite naplók:</span><span class="sxs-lookup"><span data-stu-id="98d8c-295">hello following example shows several ways toowrite logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="98d8c-296">A WebJobs SDK irányítópult hello, hello hello kimenete **TextWriter** jön létre, amikor egy adott toohello lapján továbblépne meghívása funkciót, és válassza ki az objektum **váltása kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="98d8c-296">In hello WebJobs SDK Dashboard, hello output from hello **TextWriter** object shows up when you go toohello page for a particular function invocation and select **Toggle Output**:</span></span>

![Hívás-hivatkozás](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Naplók függvény meghívása lap](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

<span data-ttu-id="98d8c-299">A WebJobs SDK irányítópult hello, hello legutóbbi 100 sorok a konzol kimeneti megjelenítése mentése lépjen a webjobs-feladat hello (nem a hello függvény meghívása) toohello lapra, és válassza ki **váltása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="98d8c-299">In hello WebJobs SDK Dashboard, hello most recent 100 lines of Console output show up when you go toohello page for hello WebJob (not for hello function invocation) and select **Toggle Output**.</span></span>

![Váltás kimeneti](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

<span data-ttu-id="98d8c-301">Egy folyamatos webjobs-feladat, az alkalmazás naplóiban jelennek meg az/data/feladatok/folyamatos/*{webjobname}*/job_log.txt hello web app fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="98d8c-301">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in hello web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="98d8c-302">Az Azure blob hello alkalmazásban naplók néznek ki: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, hiba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span><span class="sxs-lookup"><span data-stu-id="98d8c-302">In an Azure blob hello application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="98d8c-303">Egy Azure-tábla hello a **Console.Out** és **Console.Error** naplók néznek ki:</span><span class="sxs-lookup"><span data-stu-id="98d8c-303">And in an Azure table hello **Console.Out** and **Console.Error** logs look like this:</span></span>

![A tábla adatai napló](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Hibanapló tábla](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a><span data-ttu-id="98d8c-306">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98d8c-306">Next steps</span></span>
<span data-ttu-id="98d8c-307">Ez a cikk nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei az Azure-üzenetsorok használata.</span><span class="sxs-lookup"><span data-stu-id="98d8c-307">This article has provided code samples that show how toohandle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="98d8c-308">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="98d8c-308">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

