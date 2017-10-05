---
title: "Az Azure Queue Storage használata a WebJobs SDK-val"
description: "Ismerje meg az Azure a queue storage használata a WebJobs SDK-val. Hozzon létre vagy töröljön a várólisták; Helyezze, Belepillantás, get, és törölje az üzenetsor-üzeneteket, és több."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: dbfac5d9-f4a0-4e3e-9ecc-af3d7bf80463
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 63b987a2c9471f2929b8d2dd605323910d2ad43b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a><span data-ttu-id="cc95e-104">Az Azure Queue Storage használata a WebJobs SDK-val</span><span class="sxs-lookup"><span data-stu-id="cc95e-104">How to use Azure queue storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="cc95e-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cc95e-105">Overview</span></span>
<span data-ttu-id="cc95e-106">Ez az útmutató C# mintakódok az Azure WebJobs SDK-verzió használatát mutatják be az Azure üzenetsor-kezelési tárolási szolgáltatással 1.x.</span><span class="sxs-lookup"><span data-stu-id="cc95e-106">This guide provides C# code samples that show how to use the Azure WebJobs SDK version 1.x with the Azure queue storage service.</span></span>

<span data-ttu-id="cc95e-107">Az útmutató azt feltételezi, hogy tudja [hogyan webjobs-feladat-projekt létrehozása a Visual Studio kapcsolati karakterláncok a tárfiókhoz adott pontra](websites-dotnet-webjobs-sdk-get-started.md) vagy [több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="cc95e-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="cc95e-108">A legtöbb kódrészletek csak megjelenítése funkciók, nem a kódot, amely létrehozza a `JobHost` objektum ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="cc95e-108">Most of the code snippets only show functions, not the code that creates the `JobHost` object as in this example:</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

<span data-ttu-id="cc95e-109">Az útmutató a következő témakörökből áll:</span><span class="sxs-lookup"><span data-stu-id="cc95e-109">The guide includes the following topics:</span></span>

* [<span data-ttu-id="cc95e-110">Egy függvény események várólista üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="cc95e-110">How to trigger a function when a queue message is received</span></span>](#trigger)
  * <span data-ttu-id="cc95e-111">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-111">String queue messages</span></span>
  * <span data-ttu-id="cc95e-112">POCO üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-112">POCO queue messages</span></span>
  * <span data-ttu-id="cc95e-113">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="cc95e-113">Async functions</span></span>
  * <span data-ttu-id="cc95e-114">Együttműködve biztosítja a QueueTrigger attribútum típusa</span><span class="sxs-lookup"><span data-stu-id="cc95e-114">Types the QueueTrigger attribute works with</span></span>
  * <span data-ttu-id="cc95e-115">Lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="cc95e-115">Polling algorithm</span></span>
  * <span data-ttu-id="cc95e-116">Több példánya</span><span class="sxs-lookup"><span data-stu-id="cc95e-116">Multiple instances</span></span>
  * <span data-ttu-id="cc95e-117">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="cc95e-117">Parallel execution</span></span>
  * <span data-ttu-id="cc95e-118">Várólista vagy várólista üzenet metaadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="cc95e-118">Get queue or queue message metadata</span></span>
  * <span data-ttu-id="cc95e-119">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="cc95e-119">Graceful shutdown</span></span>
* [<span data-ttu-id="cc95e-120">Egy várólista üzenet feldolgozása közben egy üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc95e-120">How to create a queue message while processing a queue message</span></span>](#createqueue)
  * <span data-ttu-id="cc95e-121">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-121">String queue messages</span></span>
  * <span data-ttu-id="cc95e-122">POCO üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-122">POCO queue messages</span></span>
  * <span data-ttu-id="cc95e-123">Hozzon létre több üzenetet vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="cc95e-123">Create multiple messages or in async functions</span></span>
  * <span data-ttu-id="cc95e-124">A várólista attribútum együttműködve típusok</span><span class="sxs-lookup"><span data-stu-id="cc95e-124">Types the Queue attribute works with</span></span>
  * <span data-ttu-id="cc95e-125">Egy függvény törzséhez a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="cc95e-125">Use WebJobs SDK attributes in the body of a function</span></span>
* [<span data-ttu-id="cc95e-126">Olvasási és írási blobok várólista üzenet feldolgozásakor a program hogyan</span><span class="sxs-lookup"><span data-stu-id="cc95e-126">How to read and write blobs while processing a queue message</span></span>](#blobs)
  * <span data-ttu-id="cc95e-127">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-127">String queue messages</span></span>
  * <span data-ttu-id="cc95e-128">POCO üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-128">POCO queue messages</span></span>
  * <span data-ttu-id="cc95e-129">A Blob attribútum együttműködve típusok</span><span class="sxs-lookup"><span data-stu-id="cc95e-129">Types the Blob attribute works with</span></span>
* [<span data-ttu-id="cc95e-130">Az elhalt üzenetek kezelésének módját</span><span class="sxs-lookup"><span data-stu-id="cc95e-130">How to handle poison messages</span></span>](#poison)
  * <span data-ttu-id="cc95e-131">Automatikus elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="cc95e-131">Automatic poison message handling</span></span>
  * <span data-ttu-id="cc95e-132">Manuális elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="cc95e-132">Manual poison message handling</span></span>
* [<span data-ttu-id="cc95e-133">Hogyan konfigurációs beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="cc95e-133">How to set configuration options</span></span>](#config)
  * <span data-ttu-id="cc95e-134">A kód SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="cc95e-134">Set SDK connection strings in code</span></span>
  * <span data-ttu-id="cc95e-135">QueueTrigger beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc95e-135">Configure QueueTrigger settings</span></span>
  * <span data-ttu-id="cc95e-136">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="cc95e-136">Set values for WebJobs SDK constructor parameters in code</span></span>
* [<span data-ttu-id="cc95e-137">Hogyan kell manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="cc95e-137">How to trigger a function manually</span></span>](#manual)
* [<span data-ttu-id="cc95e-138">Naplók írásával</span><span class="sxs-lookup"><span data-stu-id="cc95e-138">How to write logs</span></span>](#logs)
* [<span data-ttu-id="cc95e-139">Hibák kezelésének és időtúllépések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc95e-139">How to handle errors and configure timeouts</span></span>](#errors)
* [<span data-ttu-id="cc95e-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc95e-140">Next steps</span></span>](#nextsteps)

## <span data-ttu-id="cc95e-141"><a id="trigger"></a>Egy függvény események várólista üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="cc95e-141"><a id="trigger"></a> How to trigger a function when a queue message is received</span></span>
<span data-ttu-id="cc95e-142">Egy függvényt, amely a WebJobs SDK meghívja a várólista-üzenet fogadásakor használja a `QueueTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-142">To write a function that the WebJobs SDK calls when a queue message is received, use the `QueueTrigger` attribute.</span></span> <span data-ttu-id="cc95e-143">Az attribútum konstruktora karakterlánc paramétert, és kérdezze le a várólista nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="cc95e-143">The attribute constructor takes a string parameter that specifies the name of the queue to poll.</span></span> <span data-ttu-id="cc95e-144">Emellett [dinamikusan beállítani a várólista nevét](#config).</span><span class="sxs-lookup"><span data-stu-id="cc95e-144">You can also [set the queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="cc95e-145">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-145">String queue messages</span></span>
<span data-ttu-id="cc95e-146">A következő példában a várólista tartalmaz egy karakterlánc-üzenet, így `QueueTrigger` egy karakterlánc-paramétert nevű alkalmazott `logMessage` tartalmazó az üzenetsorban lévő üzenetet tartalmát.</span><span class="sxs-lookup"><span data-stu-id="cc95e-146">In the following example, the queue contains a string message, so `QueueTrigger` is applied to a string parameter named `logMessage` which contains the content of the queue message.</span></span> <span data-ttu-id="cc95e-147">A függvény [egy naplófájlüzenetre ír az irányítópult](#logs).</span><span class="sxs-lookup"><span data-stu-id="cc95e-147">The function [writes a log message to the Dashboard](#logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="cc95e-148">Emellett `string`, a paraméter lehet egy bájttömböt egy `CloudQueueMessage` objektumot, vagy egy Ön által meghatározott POCO.</span><span class="sxs-lookup"><span data-stu-id="cc95e-148">Besides `string`, the parameter may be a byte array, a `CloudQueueMessage` object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="cc95e-149">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-149">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="cc95e-150">A következő példában az üzenetsorban lévő üzenetet tartalmazza a JSON a `BlobInformation` objektum, amely tartalmazza a `BlobName` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cc95e-150">In the following example, the queue message contains JSON for a `BlobInformation` object which includes a `BlobName` property.</span></span> <span data-ttu-id="cc95e-151">Az SDK automatikusan deserializes az objektum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-151">The SDK automatically deserializes the object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="cc95e-152">Az SDK-t használ a [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) szerializálása és deszerializálása üzenetek.</span><span class="sxs-lookup"><span data-stu-id="cc95e-152">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="cc95e-153">Üzenetsor-üzeneteket hoz létre egy programot, amely a WebJobs SDK nem használja, ha írhat kódot létrehozni egy POCO üzenetsor-üzenetet, amely az SDK tudja értelmezni az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="cc95e-153">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="cc95e-154">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="cc95e-154">Async functions</span></span>
<span data-ttu-id="cc95e-155">A következő aszinkron függvény [napló ír az irányítópult](#logs).</span><span class="sxs-lookup"><span data-stu-id="cc95e-155">The following async function [writes a log to the Dashboard](#logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="cc95e-156">Aszinkron funkciók is igénybe vehet egy [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), ahogy az az alábbi példa, amely egy blob másolja.</span><span class="sxs-lookup"><span data-stu-id="cc95e-156">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in the following example which copies a blob.</span></span> <span data-ttu-id="cc95e-157">(További tájékoztatás a `queueTrigger` helyőrző, tekintse meg a [Blobok](#blobs) szakasz.)</span><span class="sxs-lookup"><span data-stu-id="cc95e-157">(For an explanation of the `queueTrigger` placeholder, see the [Blobs](#blobs) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <span data-ttu-id="cc95e-158"><a id="qtattributetypes"></a>Együttműködve biztosítja a QueueTrigger attribútum típusa</span><span class="sxs-lookup"><span data-stu-id="cc95e-158"><a id="qtattributetypes"></a> Types the QueueTrigger attribute works with</span></span>
<span data-ttu-id="cc95e-159">Használhat `QueueTrigger` a következő típusú:</span><span class="sxs-lookup"><span data-stu-id="cc95e-159">You can use `QueueTrigger` with the following types:</span></span>

* `string`
* <span data-ttu-id="cc95e-160">A JSON-ként szerializált POCO típus</span><span class="sxs-lookup"><span data-stu-id="cc95e-160">A POCO type serialized as JSON</span></span>
* `byte[]`
* `CloudQueueMessage`

### <span data-ttu-id="cc95e-161"><a id="polling"></a>Lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="cc95e-161"><a id="polling"></a> Polling algorithm</span></span>
<span data-ttu-id="cc95e-162">Az SDK-t egy véletlenszerű exponenciális vissza az indító algoritmus üresjárati-várólista tárolási tranzakciós költségeket a lekérdezési hatásainak csökkentése érdekében valósít meg.</span><span class="sxs-lookup"><span data-stu-id="cc95e-162">The SDK implements a random exponential back-off algorithm to reduce the effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="cc95e-163">Ha üzenet található, az SDK két másodpercet vár, és ellenőrzi, egy másik üzenet; Ha üzenet található azt, mielőtt újra próbálkozna körülbelül négy másodpercet vár.</span><span class="sxs-lookup"><span data-stu-id="cc95e-163">When a message is found, the SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="cc95e-164">A várakozási idő után további sikertelenül megpróbálják a várólista üzenet jelenik meg, folyamatosan nő, nem éri a maximális várakozási idő, alapértelmezett értéke, egy perc alatt.</span><span class="sxs-lookup"><span data-stu-id="cc95e-164">After subsequent failed attempts to get a queue message, the wait time continues to increase until it reaches the maximum wait time, which defaults to one minute.</span></span> <span data-ttu-id="cc95e-165">[A maximális várakozási idő az konfigurálható](#config).</span><span class="sxs-lookup"><span data-stu-id="cc95e-165">[The maximum wait time is configurable](#config).</span></span>

### <span data-ttu-id="cc95e-166"><a id="instances"></a>Több példánya</span><span class="sxs-lookup"><span data-stu-id="cc95e-166"><a id="instances"></a> Multiple instances</span></span>
<span data-ttu-id="cc95e-167">Ha több példányt a webalkalmazás fut, egy folyamatos webjobs-feladat fut az egyes gépek, és minden gép fog Várjon, amíg az eseményindítók és funkciók futtatására tett kísérlet.</span><span class="sxs-lookup"><span data-stu-id="cc95e-167">If your web app runs on multiple instances, a continuous WebJob runs on each machine, and each machine will wait for triggers and attempt to run functions.</span></span> <span data-ttu-id="cc95e-168">A WebJobs SDK várólista eseményindító automatikusan megakadályozza, hogy a függvény egy várólista üzenet feldolgozása többször; funkciók nem kell az idempotent kell írni.</span><span class="sxs-lookup"><span data-stu-id="cc95e-168">The WebJobs SDK queue trigger automatically prevents a function from processing a queue message multiple times; functions do not have to be written to be idempotent.</span></span> <span data-ttu-id="cc95e-169">Azonban ha biztos szeretne lenni abban, hogy csak egy példányát a függvény fut, akkor is, ha a gazdagép webes alkalmazás több példánya van, használja a `Singleton` attribútum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-169">However, if you want to ensure that only one instance of a function runs even when there are multiple instances of the host web app, you can use the `Singleton` attribute.</span></span>

### <span data-ttu-id="cc95e-170"><a id="parallel"></a>Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="cc95e-170"><a id="parallel"></a> Parallel execution</span></span>
<span data-ttu-id="cc95e-171">Ha több funkciók különböző várólisták figyeli, az SDK-t fogja hívni azokat párhuzamosan üzenetek fogadása egy időben.</span><span class="sxs-lookup"><span data-stu-id="cc95e-171">If you have multiple functions listening on different queues, the SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="cc95e-172">Ugyanez vonatkozik több üzenetet egyetlen várólista fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="cc95e-172">The same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="cc95e-173">Alapértelmezés szerint az SDK egyszerre az 16 várólista üzenetkötegek lekérdezi és feldolgozza azokat párhuzamosan függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="cc95e-173">By default, the SDK gets a batch of 16 queue messages at a time and executes the function that processes them in parallel.</span></span> <span data-ttu-id="cc95e-174">[A Köteg mérete nem konfigurálható](#config).</span><span class="sxs-lookup"><span data-stu-id="cc95e-174">[The batch size is configurable](#config).</span></span> <span data-ttu-id="cc95e-175">A feldolgozott számát a Köteg mérete felének lekérdezi, ha az SDK-t egy másik köteg lekérdezi, és megkezdi az üzenetek feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="cc95e-175">When the number being processed gets down to half of the batch size, the SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="cc95e-176">Ezért a maximális száma párhuzamos éppen feldolgozott üzeneteinek másodpercenkénti függvény, egy és egy félig kötegelt méretének.</span><span class="sxs-lookup"><span data-stu-id="cc95e-176">Therefore the maximum number of concurrent messages being processed per function is one and a half times the batch size.</span></span> <span data-ttu-id="cc95e-177">Ezt a határt külön vonatkozik minden funkció, amely rendelkezik egy `QueueTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-177">This limit applies separately to each function that has a `QueueTrigger` attribute.</span></span>

<span data-ttu-id="cc95e-178">Ha nem szeretné, egy olyan sort a fogadott üzenetet párhuzamos futtatáshoz, beállíthatja a köteg méretének 1.</span><span class="sxs-lookup"><span data-stu-id="cc95e-178">If you don't want parallel execution for messages received on one queue, you can set the batch size to 1.</span></span> <span data-ttu-id="cc95e-179">Lásd még: **várólista feldolgozása teljesebb körű vezérlése** a [Azure WebJobs SDK 1.1.0-ás RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span><span class="sxs-lookup"><span data-stu-id="cc95e-179">See also **More control over Queue processing** in [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span></span>

### <span data-ttu-id="cc95e-180"><a id="queuemetadata"></a>Várólista vagy várólista üzenet metaadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="cc95e-180"><a id="queuemetadata"></a>Get queue or queue message metadata</span></span>
<span data-ttu-id="cc95e-181">A következő üzenet tulajdonságai kaphat paramétereket ad a metódus-aláírás:</span><span class="sxs-lookup"><span data-stu-id="cc95e-181">You can get the following message properties by adding parameters to the method signature:</span></span>

* <span data-ttu-id="cc95e-182">`DateTimeOffset`expirationTime</span><span class="sxs-lookup"><span data-stu-id="cc95e-182">`DateTimeOffset` expirationTime</span></span>
* <span data-ttu-id="cc95e-183">`DateTimeOffset`insertionTime</span><span class="sxs-lookup"><span data-stu-id="cc95e-183">`DateTimeOffset` insertionTime</span></span>
* <span data-ttu-id="cc95e-184">`DateTimeOffset`nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="cc95e-184">`DateTimeOffset` nextVisibleTime</span></span>
* <span data-ttu-id="cc95e-185">`string`queueTrigger (tartalmazza a szöveges üzenet)</span><span class="sxs-lookup"><span data-stu-id="cc95e-185">`string` queueTrigger (contains message text)</span></span>
* <span data-ttu-id="cc95e-186">`string`azonosítója</span><span class="sxs-lookup"><span data-stu-id="cc95e-186">`string` id</span></span>
* <span data-ttu-id="cc95e-187">`string`popReceipt</span><span class="sxs-lookup"><span data-stu-id="cc95e-187">`string` popReceipt</span></span>
* <span data-ttu-id="cc95e-188">`int`dequeueCount</span><span class="sxs-lookup"><span data-stu-id="cc95e-188">`int` dequeueCount</span></span>

<span data-ttu-id="cc95e-189">Ha közvetlenül az Azure storage API dolgozni szeretne, azt is megteheti egy `CloudStorageAccount` paraméter.</span><span class="sxs-lookup"><span data-stu-id="cc95e-189">If you want to work directly with the Azure storage API, you can also add a `CloudStorageAccount` parameter.</span></span>

<span data-ttu-id="cc95e-190">A következő példa egy információ alkalmazás naplófájlba írja a metaadatok mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="cc95e-190">The following example writes all of this metadata to an INFO application log.</span></span> <span data-ttu-id="cc95e-191">A példában logMessage és queueTrigger is tartalmazhat az üzenetsorban lévő üzenetet tartalmát.</span><span class="sxs-lookup"><span data-stu-id="cc95e-191">In the example, both logMessage and queueTrigger contain the content of the queue message.</span></span>

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

<span data-ttu-id="cc95e-192">Íme egy minta naplót kell írni a minta kóddal:</span><span class="sxs-lookup"><span data-stu-id="cc95e-192">Here is a sample log written by the sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <span data-ttu-id="cc95e-193"><a id="graceful"></a>Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="cc95e-193"><a id="graceful"></a>Graceful shutdown</span></span>
<span data-ttu-id="cc95e-194">Egy függvény a folyamatos webjobs-feladat fut fogad el egy `CancellationToken` paramétert, amely lehetővé teszi, hogy az operációs rendszer, a függvény értesíti, amikor a webjobs-feladat megszakítása.</span><span class="sxs-lookup"><span data-stu-id="cc95e-194">A function that runs in a continuous WebJob can accept a `CancellationToken` parameter which enables the operating system to notify the function when the WebJob is about to be terminated.</span></span> <span data-ttu-id="cc95e-195">Az értesítés segítségével győződjön meg arról, hogy a függvény nem bontható váratlanul oly módon, hogy az adatok inkonzisztens állapotban hagyja.</span><span class="sxs-lookup"><span data-stu-id="cc95e-195">You can use this notification to make sure the function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="cc95e-196">A következő példa bemutatja, hogyan közelgő webjobs-feladat futása függvények kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="cc95e-196">The following example shows how to check for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="cc95e-197">**Megjegyzés:** az irányítópult előfordulhat, hogy helyesen jelenjen meg az állapot és a kimeneti funkciót le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="cc95e-197">**Note:** The Dashboard might not correctly show the status and output of functions that have been shut down.</span></span>

<span data-ttu-id="cc95e-198">További információkért lásd: [webjobs-feladatok szabályos leállítást](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="cc95e-198">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <span data-ttu-id="cc95e-199"><a id="createqueue"></a>Egy várólista üzenet feldolgozása közben egy üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc95e-199"><a id="createqueue"></a> How to create a queue message while processing a queue message</span></span>
<span data-ttu-id="cc95e-200">Egy függvényt, amely létrehoz egy új sor írása, használja a `Queue` attribútum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-200">To write a function that creates a new queue message, use the `Queue` attribute.</span></span> <span data-ttu-id="cc95e-201">Például `QueueTrigger`, adja meg a várólista neve karakterláncként vagy beállíthatja a [dinamikusan beállítani a várólista nevét](#config).</span><span class="sxs-lookup"><span data-stu-id="cc95e-201">Like `QueueTrigger`, you pass in the queue name as a string or you can [set the queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="cc95e-202">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-202">String queue messages</span></span>
<span data-ttu-id="cc95e-203">A következő nem aszinkron kódminta létrehoz egy új várólista-üzenetet a várólistában, ugyanahhoz a tartalomhoz, mint az üzenetsorban lévő üzenetet kapott a várólista "inputqueue" nevű, "outputqueue" nevű.</span><span class="sxs-lookup"><span data-stu-id="cc95e-203">The following non-async code sample creates a new queue message in the queue named "outputqueue" with the same content as the queue message received in the queue named "inputqueue".</span></span> <span data-ttu-id="cc95e-204">(Aszinkron funkciók használata `IAsyncCollector<T>` később az itt látható módon.)</span><span class="sxs-lookup"><span data-stu-id="cc95e-204">(For async functions use `IAsyncCollector<T>` as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="cc95e-205">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-205">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="cc95e-206">Hozzon létre egy üzenetsor-üzenetet, amely egy karakterlánc helyett egy POCO tartalmaz, adja át a POCO típus az output paraméterként a `Queue` attribútumok konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="cc95e-206">To create a queue message that contains a POCO rather than a string, pass the POCO type as an output parameter to the `Queue` attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="cc95e-207">Az SDK automatikusan a JSON-objektum rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="cc95e-207">The SDK automatically serializes the object to JSON.</span></span> <span data-ttu-id="cc95e-208">Egy üzenetsor mindig létrejön, még akkor is, ha az objektum értéke null.</span><span class="sxs-lookup"><span data-stu-id="cc95e-208">A queue message is always created, even if the object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="cc95e-209">Hozzon létre több üzenetet vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="cc95e-209">Create multiple messages or in async functions</span></span>
<span data-ttu-id="cc95e-210">Több üzenetet létrehozni, győződjön meg a paraméter típusa a kimeneti várólista `ICollector<T>` vagy `IAsyncCollector<T>`, a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-210">To create multiple messages, make the parameter type for the output queue `ICollector<T>` or `IAsyncCollector<T>`, as shown in the following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="cc95e-211">Minden várólista üzenet jön létre azonnal amikor a `Add` metódust.</span><span class="sxs-lookup"><span data-stu-id="cc95e-211">Each queue message is created immediately when the `Add` method is called.</span></span>

### <a name="types-that-the-queue-attribute-works-with"></a><span data-ttu-id="cc95e-212">Amely a várólista attribútum kompatibilis típusok</span><span class="sxs-lookup"><span data-stu-id="cc95e-212">Types that the Queue attribute works with</span></span>
<span data-ttu-id="cc95e-213">Használhatja a `Queue` attribútum a következő paraméter típusa:</span><span class="sxs-lookup"><span data-stu-id="cc95e-213">You can use the `Queue` attribute on the following parameter types:</span></span>

* <span data-ttu-id="cc95e-214">`out string`(hoz létre üzenetsor-üzenetet, ha a paraméter értéke nem null értékű akkor, ha a függvény karakterlánccal végződik-e)</span><span class="sxs-lookup"><span data-stu-id="cc95e-214">`out string` (creates queue message if parameter value is non-null when the function ends)</span></span>
* <span data-ttu-id="cc95e-215">`out byte[]`(működik, mint az `string`)</span><span class="sxs-lookup"><span data-stu-id="cc95e-215">`out byte[]` (works like `string`)</span></span>
* <span data-ttu-id="cc95e-216">`out CloudQueueMessage`(működik, mint az `string`)</span><span class="sxs-lookup"><span data-stu-id="cc95e-216">`out CloudQueueMessage` (works like `string`)</span></span>
* <span data-ttu-id="cc95e-217">`out POCO`(egy szerializálható típust, létrehoz egy üzenetet null objektummal rendelkező, ha a paraméter értéke null, ha a függvény karakterlánccal végződik-e)</span><span class="sxs-lookup"><span data-stu-id="cc95e-217">`out POCO` (a serializable type, creates a message with a null object if the paramter is null when the function ends)</span></span>
* `ICollector`
* `IAsyncCollector`
* <span data-ttu-id="cc95e-218">`CloudQueue`(a manuális közvetlenül az Azure Storage API használatával üzenet létrehozása)</span><span class="sxs-lookup"><span data-stu-id="cc95e-218">`CloudQueue` (for creating messages manually using the Azure Storage API directly)</span></span>

### <span data-ttu-id="cc95e-219"><a id="ibinder"></a>Egy függvény törzséhez a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="cc95e-219"><a id="ibinder"></a>Use WebJobs SDK attributes in the body of a function</span></span>
<span data-ttu-id="cc95e-220">Ha néhány a munkáját a függvény használata, mint a WebJobs SDK-attribútum előtt kell `Queue`, `Blob`, vagy `Table`, használhatja a `IBinder` felületet.</span><span class="sxs-lookup"><span data-stu-id="cc95e-220">If you need to do some work in your function before using a WebJobs SDK attribute such as `Queue`, `Blob`, or `Table`, you can use the `IBinder` interface.</span></span>

<span data-ttu-id="cc95e-221">Az alábbi példa időt vesz igénybe egy bemeneti várólista-üzenet, és létrehoz egy új üzenetet a kimeneti várólistában lévő ugyanahhoz a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="cc95e-221">The following example takes an input queue message and creates a new message with the same content in an output queue.</span></span> <span data-ttu-id="cc95e-222">A kimeneti várólista neve van beállítva, a függvény törzséhez tartozó kóddal.</span><span class="sxs-lookup"><span data-stu-id="cc95e-222">The output queue name is set by code in the body of the function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="cc95e-223">A `IBinder` felület is használható a `Table` és `Blob` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="cc95e-223">The `IBinder` interface can also be used with the `Table` and `Blob` attributes.</span></span>

## <span data-ttu-id="cc95e-224"><a id="blobs"></a>Hogyan olvashatja és írhatja a blobok és táblák egy várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="cc95e-224"><a id="blobs"></a> How to read and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="cc95e-225">A `Blob` és `Table` attribútumok lehetővé teszik a olvasását és írását, blobok és táblákat.</span><span class="sxs-lookup"><span data-stu-id="cc95e-225">The `Blob` and `Table` attributes enable you to read and write blobs and tables.</span></span> <span data-ttu-id="cc95e-226">Ebben a szakaszban a mintákat a blobok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc95e-226">The samples in this section apply to blobs.</span></span> <span data-ttu-id="cc95e-227">Mutatják be, akkor a blobok létrehozott vagy frissített folyamatok vált mintakódok, lásd: [Azure blob storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), és mintakódok olvasási és írási táblák, lásd: [Azure table storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="cc95e-227">For code samples that show how to trigger processes when blobs are created or updated, see [How to use Azure blob storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="cc95e-228">Karakterlánc üzenetek blob műveletek időt.</span><span class="sxs-lookup"><span data-stu-id="cc95e-228">String queue messages triggering blob operations</span></span>
<span data-ttu-id="cc95e-229">Egy karakterláncot tartalmazó várólista üzenet `queueTrigger` használható helyőrző a `Blob` attribútum `blobPath` paraméter, amely tartalmazza az üzenet tartalmát.</span><span class="sxs-lookup"><span data-stu-id="cc95e-229">For a queue message that contains a string, `queueTrigger` is a placeholder you can use in the `Blob` attribute's `blobPath` parameter that contains the contents of the message.</span></span>

<span data-ttu-id="cc95e-230">Az alábbi példában `Stream` objektumok olvasását és írását blobokat.</span><span class="sxs-lookup"><span data-stu-id="cc95e-230">The following example uses `Stream` objects to read and write blobs.</span></span> <span data-ttu-id="cc95e-231">Az üzenetsorban lévő üzenetet a textblobs tárolóban lévő blob neve.</span><span class="sxs-lookup"><span data-stu-id="cc95e-231">The queue message is the name of a blob located in the textblobs container.</span></span> <span data-ttu-id="cc95e-232">Blob másolatát "-új" hozzáfűzi a nevet ugyanabban a tárolóban jön létre.</span><span class="sxs-lookup"><span data-stu-id="cc95e-232">A copy of the blob with "-new" appended to the name is created in the same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="cc95e-233">A `Blob` attribútum konstruktora vesz egy `blobPath` paraméter meghatározza, hogy a tároló és a blob neve.</span><span class="sxs-lookup"><span data-stu-id="cc95e-233">The `Blob` attribute constructor takes a `blobPath` parameter that specifies the container and blob name.</span></span> <span data-ttu-id="cc95e-234">A helyőrző kapcsolatos további információkért lásd: [Azure blob storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span><span class="sxs-lookup"><span data-stu-id="cc95e-234">For more information about this placeholder, see [How to use Azure blob storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span></span>

<span data-ttu-id="cc95e-235">Ha az attribútum decorates egy `Stream` objektumot egy másik konstruktor paraméter határozza meg a `FileAccess` olvasási, írási vagy olvasási/írási módban.</span><span class="sxs-lookup"><span data-stu-id="cc95e-235">When the attribute decorates a `Stream` object, another constructor parameter specifies the `FileAccess` mode as read, write, or read/write.</span></span>

<span data-ttu-id="cc95e-236">Az alábbi példában egy `CloudBlockBlob` blob törlendő objektum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-236">The following example uses a `CloudBlockBlob` object to delete a blob.</span></span> <span data-ttu-id="cc95e-237">Az üzenetsorban lévő üzenetet a blob neve.</span><span class="sxs-lookup"><span data-stu-id="cc95e-237">The queue message is the name of the blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <span data-ttu-id="cc95e-238"><a id="pocoblobs"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="cc95e-238"><a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="cc95e-239">Egy POCO az üzenetsorban lévő üzenetet JSON-ként tárolja, a helyőrzők, amelyek neve az objektum tulajdonságainak használhatják a `Queue` attribútum `blobPath` paraméter.</span><span class="sxs-lookup"><span data-stu-id="cc95e-239">For a POCO stored as JSON in the queue message, you can use placeholders that name properties of the object in the `Queue` attribute's `blobPath` parameter.</span></span> <span data-ttu-id="cc95e-240">Is [metaadatok tulajdonságnevek várólistára](#queuemetadata) helyőrzőként.</span><span class="sxs-lookup"><span data-stu-id="cc95e-240">You can also use [queue metadata property names](#queuemetadata) as placeholders.</span></span>

<span data-ttu-id="cc95e-241">A következő példa egy új blobot egy másik kiterjesztést a blob másolja.</span><span class="sxs-lookup"><span data-stu-id="cc95e-241">The following example copies a blob to a new blob with a different extension.</span></span> <span data-ttu-id="cc95e-242">A várólista az üzenet egy `BlobInformation` , amely tartalmazza az objektum `BlobName` és `BlobNameWithoutExtension` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="cc95e-242">The queue message is a `BlobInformation` object that includes `BlobName` and `BlobNameWithoutExtension` properties.</span></span> <span data-ttu-id="cc95e-243">A tulajdonságnevek helyőrzőként a blob elérési útját a `Blob` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="cc95e-243">The property names are used as placeholders in the blob path for the `Blob` attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="cc95e-244">Az SDK-t használ a [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) szerializálása és deszerializálása üzenetek.</span><span class="sxs-lookup"><span data-stu-id="cc95e-244">The SDK uses the [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) to serialize and deserialize messages.</span></span> <span data-ttu-id="cc95e-245">Üzenetsor-üzeneteket hoz létre egy programot, amely a WebJobs SDK nem használja, ha írhat kódot létrehozni egy POCO üzenetsor-üzenetet, amely az SDK tudja értelmezni az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="cc95e-245">If you create queue messages in a program that doesn't use the WebJobs SDK, you can write code like the following example to create a POCO queue message that the SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="cc95e-246">Ha néhány célra a függvény előtt blob kötés olyan objektumra kell, a függvény törzséhez tartozó, a attribútumot használhatja [ahogy korábban a várólista attribútum](#ibinder).</span><span class="sxs-lookup"><span data-stu-id="cc95e-246">If you need to do some work in your function before binding a blob to an object, you can use the attribute in the body of the function, [as shown earlier for the Queue attribute](#ibinder).</span></span>

### <span data-ttu-id="cc95e-247"><a id="blobattributetypes"></a>A Blob attribútumot is használhatja típusok</span><span class="sxs-lookup"><span data-stu-id="cc95e-247"><a id="blobattributetypes"></a> Types you can use the Blob attribute with</span></span>
<span data-ttu-id="cc95e-248">A `Blob` attribútum a következő típusú használható:</span><span class="sxs-lookup"><span data-stu-id="cc95e-248">The `Blob` attribute can be used with the following types:</span></span>

* <span data-ttu-id="cc95e-249">`Stream`(olvasására vagy írására, a FileAccess konstruktor paraméterrel megadott)</span><span class="sxs-lookup"><span data-stu-id="cc95e-249">`Stream` (read or write, specified by using the FileAccess constructor parameter)</span></span>
* `TextReader`
* `TextWriter`
* <span data-ttu-id="cc95e-250">`string`(olvasás)</span><span class="sxs-lookup"><span data-stu-id="cc95e-250">`string` (read)</span></span>
* <span data-ttu-id="cc95e-251">`out string`(írási; hoz létre egy blobot, csak akkor, ha a karakterlánc-paraméter null értékű akkor, ha a függvény)</span><span class="sxs-lookup"><span data-stu-id="cc95e-251">`out string` (write; creates a blob only if the string parameter is non-null when the function returns)</span></span>
* <span data-ttu-id="cc95e-252">POCO (olvasás)</span><span class="sxs-lookup"><span data-stu-id="cc95e-252">POCO (read)</span></span>
* <span data-ttu-id="cc95e-253">kimenő POCO (írás; mindig létrehoz egy blobot, mint null objektumot hoz létre, ha POCO paraméter értéke null, ha a függvény)</span><span class="sxs-lookup"><span data-stu-id="cc95e-253">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when the function returns)</span></span>
* <span data-ttu-id="cc95e-254">`CloudBlobStream`(írja)</span><span class="sxs-lookup"><span data-stu-id="cc95e-254">`CloudBlobStream` (write)</span></span>
* <span data-ttu-id="cc95e-255">`ICloudBlob`(olvasás vagy írás)</span><span class="sxs-lookup"><span data-stu-id="cc95e-255">`ICloudBlob` (read or write)</span></span>
* <span data-ttu-id="cc95e-256">`CloudBlockBlob`(olvasás vagy írás)</span><span class="sxs-lookup"><span data-stu-id="cc95e-256">`CloudBlockBlob` (read or write)</span></span>
* <span data-ttu-id="cc95e-257">`CloudPageBlob`(olvasás vagy írás)</span><span class="sxs-lookup"><span data-stu-id="cc95e-257">`CloudPageBlob` (read or write)</span></span>

## <span data-ttu-id="cc95e-258"><a id="poison"></a>Az elhalt üzenetek kezelésének módját</span><span class="sxs-lookup"><span data-stu-id="cc95e-258"><a id="poison"></a> How to handle poison messages</span></span>
<span data-ttu-id="cc95e-259">Neve üzeneteket, amelyek tartalmát a sikertelen függvény hatására *elhalt üzenetek*.</span><span class="sxs-lookup"><span data-stu-id="cc95e-259">Messages whose content causes a function to fail are called *poison messages*.</span></span> <span data-ttu-id="cc95e-260">Ha a függvény nem sikerül, az üzenetsorban lévő üzenetet nem törlődik, és végül van felvenni, ebben az esetben meg kell ismételni a ciklus, amely.</span><span class="sxs-lookup"><span data-stu-id="cc95e-260">When the function fails, the queue message is not deleted and eventually is picked up again, causing the cycle to be repeated.</span></span> <span data-ttu-id="cc95e-261">Az SDK automatikusan megszakíthatja a ciklus korlátozott számú ismétlés után, vagy manuálisan is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="cc95e-261">The SDK can automatically interrupt the cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="cc95e-262">Automatikus elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="cc95e-262">Automatic poison message handling</span></span>
<span data-ttu-id="cc95e-263">Az SDK telefonhívásokhoz függvény legfeljebb 5-ször feldolgozni egy üzenetsor-üzenetet.</span><span class="sxs-lookup"><span data-stu-id="cc95e-263">The SDK will call a function up to 5 times to process a queue message.</span></span> <span data-ttu-id="cc95e-264">Ha ötödik ismételje meg a sikertelen, az üzenet poison sor került.</span><span class="sxs-lookup"><span data-stu-id="cc95e-264">If the fifth try fails, the message is moved to a poison queue.</span></span> <span data-ttu-id="cc95e-265">[Az újrapróbálkozások maximális száma: konfigurálható](#config).</span><span class="sxs-lookup"><span data-stu-id="cc95e-265">[The maximum number of retries is configurable](#config).</span></span>

<span data-ttu-id="cc95e-266">Az elhalt várólista nevű *{originalqueuename}*-elhalt.</span><span class="sxs-lookup"><span data-stu-id="cc95e-266">The poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="cc95e-267">Írhat egy folyamat üzenetek működnek, mint az elhalt üzenetsorból naplózásukhoz, vagy értesítést küld, hogy manuális beavatkozást van szükség.</span><span class="sxs-lookup"><span data-stu-id="cc95e-267">You can write a function to process messages from the poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="cc95e-268">Az alábbi példában a `CopyBlob` függvény sikertelenek lesznek, amikor egy üzenetsor-üzenetet, amely nem létezik blob nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cc95e-268">In the following example the `CopyBlob` function will fail when a queue message contains the name of a blob that doesn't exist.</span></span> <span data-ttu-id="cc95e-269">Ebben az esetben az üzenet átkerül a copyblobqueue várólistából a copyblobqueue-poison várólista.</span><span class="sxs-lookup"><span data-stu-id="cc95e-269">When that happens, the message is moved from the copyblobqueue queue to the copyblobqueue-poison queue.</span></span> <span data-ttu-id="cc95e-270">A `ProcessPoisonMessage` naplózza az elhalt üzenet.</span><span class="sxs-lookup"><span data-stu-id="cc95e-270">The `ProcessPoisonMessage` then logs the poison message.</span></span>

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
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

<span data-ttu-id="cc95e-271">A következő ábrán az alábbi funkciók a konzol kimeneti az elhalt üzenet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="cc95e-271">The following illustration shows console output from these functions when a poison message is processed.</span></span>

![Elhalt üzenetek kezelésének a konzol kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="cc95e-273">Manuális elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="cc95e-273">Manual poison message handling</span></span>
<span data-ttu-id="cc95e-274">Kaphat a szám, ahányszor egy üzenet felvételre feldolgozásra hozzáadása egy `int` nevű paraméter `dequeueCount` a függvénynek.</span><span class="sxs-lookup"><span data-stu-id="cc95e-274">You can get the number of times a message has been picked up for processing by adding an `int` parameter named `dequeueCount` to your function.</span></span> <span data-ttu-id="cc95e-275">Ezután ellenőrizze a funkciókódot dequeue száma, és hajtsa végre a saját elhalt üzenetek kezelésének száma meghaladja a küszöbértéket, amikor a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-275">You can then check the dequeue count in function code and perform your own poison message handling when the number exceeds a threshold, as shown in the following example.</span></span>

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <span data-ttu-id="cc95e-276"><a id="config"></a>Hogyan konfigurációs beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="cc95e-276"><a id="config"></a> How to set configuration options</span></span>
<span data-ttu-id="cc95e-277">Használhatja a `JobHostConfiguration` típus a következő konfigurációs beállítások megadásához:</span><span class="sxs-lookup"><span data-stu-id="cc95e-277">You can use the `JobHostConfiguration` type to set the following configuration options:</span></span>

* <span data-ttu-id="cc95e-278">Az SDK-kapcsolati karakterláncok beállítása a kódban.</span><span class="sxs-lookup"><span data-stu-id="cc95e-278">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="cc95e-279">Konfigurálása `QueueTrigger` beállításokat, például a maximális created száma.</span><span class="sxs-lookup"><span data-stu-id="cc95e-279">Configure `QueueTrigger` settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="cc95e-280">Várólistanevek konfigurációs beolvasása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="cc95e-280">Get queue names from configuration.</span></span>

### <span data-ttu-id="cc95e-281"><a id="setconnstr"></a>A kód SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="cc95e-281"><a id="setconnstr"></a>Set SDK connection strings in code</span></span>
<span data-ttu-id="cc95e-282">Az SDK-kapcsolati karakterláncok beállítása a kód lehetővé teszi, hogy a saját kapcsolódási karakterlánc neve, a konfigurációs fájlok vagy a környezeti változók használatát a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-282">Setting the SDK connection strings in code enables you to use your own connection string names in configuration files or environment variables, as shown in the following example.</span></span>

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

### <span data-ttu-id="cc95e-283"><a id="configqueue"></a>QueueTrigger beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc95e-283"><a id="configqueue"></a>Configure QueueTrigger  settings</span></span>
<span data-ttu-id="cc95e-284">A várólista-üzenet feldolgozása a következő beállításokat konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="cc95e-284">You can configure the following settings that apply to the queue message processing:</span></span>

* <span data-ttu-id="cc95e-285">Az üzenetsor-üzeneteket, amelyek átveszik egyidejűleg hajthatnak végre párhuzamosan maximális száma (alapértelmezett érték 16).</span><span class="sxs-lookup"><span data-stu-id="cc95e-285">The maximum number of queue messages that are picked up simultaneously to be executed in parallel (default is 16).</span></span>
* <span data-ttu-id="cc95e-286">A maximális számú újrapróbálkozást poison várólistába várólista üzenet elküldése előtt (alapértelmezett érték 5).</span><span class="sxs-lookup"><span data-stu-id="cc95e-286">The maximum number of retries before a queue message is sent to a poison queue (default is 5).</span></span>
* <span data-ttu-id="cc95e-287">A maximális várakozási idő előtt lekérdezési újra, ha üres-e a várólista (alapértelmezett érték 1 perc).</span><span class="sxs-lookup"><span data-stu-id="cc95e-287">The maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="cc95e-288">A következő példa bemutatja, hogyan konfigurálhatja ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="cc95e-288">The following example shows how to configure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="cc95e-289"><a id="setnamesincode"></a>Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="cc95e-289"><a id="setnamesincode"></a>Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="cc95e-290">Néha szeretne hozzáadni, adja meg a várólista nevét, a blob neve vagy a tároló, vagy egy táblázatot a merevlemez-kód helyett a kód adjon neki nevet.</span><span class="sxs-lookup"><span data-stu-id="cc95e-290">Sometimes you want to specify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="cc95e-291">Például előfordulhat, hogy a várólista nevét adja meg szeretné `QueueTrigger` a konfigurációs fájl vagy a környezeti változóban.</span><span class="sxs-lookup"><span data-stu-id="cc95e-291">For example, you might want to specify the queue name for `QueueTrigger` in a configuration file or environment variable.</span></span>

<span data-ttu-id="cc95e-292">A történő ehhez a `NameResolver` az objektum a `JobHostConfiguration` típusa.</span><span class="sxs-lookup"><span data-stu-id="cc95e-292">You can do that by passing in a `NameResolver` object to the `JobHostConfiguration` type.</span></span> <span data-ttu-id="cc95e-293">Ön százalékjel (%) jelentkezik be a WebJobs SDK attribútum konstruktorparaméterek körülvett különleges helyőrzőket tartalmaznak, és a `NameResolver` kód határozza meg azokat a helyőrzők helyett használandó tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="cc95e-293">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your `NameResolver` code specifies the actual values to be used in place of those placeholders.</span></span>

<span data-ttu-id="cc95e-294">Tegyük fel, hogy a tesztkörnyezetben logqueuetest és éles környezetben egy elnevezett logqueueprod nevű várólista használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="cc95e-294">For example, suppose you want to use a queue named logqueuetest in the test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="cc95e-295">A kódolt várólista neve helyett adja meg a nevét, a bejegyzés szeretné a `appSettings` gyűjteményt, amely rendelkezik a tényleges várólista neve.</span><span class="sxs-lookup"><span data-stu-id="cc95e-295">Instead of a hard-coded queue name, you want to specify the name of an entry in the `appSettings` collection that would have the actual queue name.</span></span> <span data-ttu-id="cc95e-296">Ha a `appSettings` kulcs logqueue, a függvény a következő példa nézhet.</span><span class="sxs-lookup"><span data-stu-id="cc95e-296">If the `appSettings` key is logqueue, your function could look like the following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="cc95e-297">A `NameResolver` osztály majd lehetett beolvasni a várólista nevét a `appSettings` a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="cc95e-297">Your `NameResolver` class could then get the queue name from `appSettings` as shown in the following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="cc95e-298">Adja meg a `NameResolver` az osztályt a `JobHost` objektum az az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-298">You pass the `NameResolver` class in to the `JobHost` object as shown in the following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="cc95e-299">**Megjegyzés:** várólista, a táblának és a blob nevének feloldása minden alkalommal, amikor egy függvény hívása esetén, de csak akkor, ha az alkalmazás indítása a rendszer feloldja a blob tároló neveit.</span><span class="sxs-lookup"><span data-stu-id="cc95e-299">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when the application starts.</span></span> <span data-ttu-id="cc95e-300">A blob-tároló neve nem módosítható, a feladat futása közben.</span><span class="sxs-lookup"><span data-stu-id="cc95e-300">You can't change blob container name while the job is running.</span></span>

## <span data-ttu-id="cc95e-301"><a id="manual"></a>Hogyan kell manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="cc95e-301"><a id="manual"></a>How to trigger a function manually</span></span>
<span data-ttu-id="cc95e-302">Egy függvény manuálisan kezdeményezi, használja a `Call` vagy `CallAsync` metódust a `JobHost` objektum és a `NoAutomaticTrigger` attribútuma a függvény a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-302">To trigger a function manually, use the `Call` or `CallAsync` method on the `JobHost` object and the `NoAutomaticTrigger` attribute on the function, as shown in the following example.</span></span>

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

## <span data-ttu-id="cc95e-303"><a id="logs"></a>Naplók írásával</span><span class="sxs-lookup"><span data-stu-id="cc95e-303"><a id="logs"></a>How to write logs</span></span>
<span data-ttu-id="cc95e-304">Az irányítópult az naplók két helyen jeleníti meg: a lap a webjobs-feladat, és egy adott webjobs-feladat meghívása tartozó lapon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-304">The Dashboard shows logs in two places: the page for the WebJob, and the page for a particular WebJob invocation.</span></span>

![Naplózza a webjobs-feladat lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="cc95e-307">Konzol módszerekkel, amely meghívja a függvényben vagy a a kimenetét a `Main()` módszer akkor jelenik meg, az irányítópult-oldalon a webjobs-feladat, és nem egy adott metódus meghívása tartozó lapon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-307">Output from Console methods that you call in a function or in the `Main()` method appears in the Dashboard page for the WebJob, not in the page for a particular method invocation.</span></span> <span data-ttu-id="cc95e-308">A TextWriter objektumból, hogy a paraméter a a metódus aláírásában kimeneti metódushívási irányítópult-oldalon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cc95e-308">Output from the TextWriter object that you get from a parameter in your method signature appears in the Dashboard page for a method invocation.</span></span>

<span data-ttu-id="cc95e-309">Konzol kimeneti egy adott metódushívás nem csatolható, mivel a konzol egyszálas, miközben sok munkakörök esetleg fut egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cc95e-309">Console output can't be linked to a particular method invocation because the Console is single-threaded, while many job functions may be running at the same time.</span></span> <span data-ttu-id="cc95e-310">Ezért az SDK-t biztosít minden függvény meghívása a saját egyedi napló író objektummal.</span><span class="sxs-lookup"><span data-stu-id="cc95e-310">That's why the  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="cc95e-311">Írni [alkalmazás nyomkövetési naplóit](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), használjon `Console.Out` (hoz létre a naplók adatai jelölésű) és `Console.Error` (hoz létre naplókat hiba jelöléssel).</span><span class="sxs-lookup"><span data-stu-id="cc95e-311">To write [application tracing logs](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use `Console.Out` (creates logs marked as INFO) and `Console.Error` (creates logs marked as ERROR).</span></span> <span data-ttu-id="cc95e-312">A másik lehetőség az használandó [nyomkövetési vagy TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), amely biztosítja, hogy részletes, figyelmeztetés, és a kritikus szintek mellett adatai és a hiba.</span><span class="sxs-lookup"><span data-stu-id="cc95e-312">An alternative is to use [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition to Info and Error.</span></span> <span data-ttu-id="cc95e-313">Alkalmazás nyomkövetési naplók jelennek meg a webes alkalmazások naplófájljainak, Azure-táblákban, vagy Azure-blobok attól függően, hogy hogyan konfigurálja az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cc95e-313">Application tracing logs appear in the web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="cc95e-314">Mivel minden a konzol kimeneti értéke igaz, a legutóbbi 100 alkalmazásnaplók is megjelennek az irányítópult-oldalon a webjobs-feladat, nem egy függvény meghívása tartozó lapon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-314">As is true of all Console output, the most recent 100 application logs also appear in the Dashboard page for the WebJob, not the page for a function invocation.</span></span>

<span data-ttu-id="cc95e-315">Konzol eredmény jelenik meg, csak akkor, ha a program fut az Azure webjobs-feladat, nem, ha a program helyben fut az irányítópulton vagy más környezetben.</span><span class="sxs-lookup"><span data-stu-id="cc95e-315">Console output appears in the Dashboard only if the program is running in an Azure WebJob, not if the program is running locally or in some other environment.</span></span>

<span data-ttu-id="cc95e-316">Tiltsa le a magas teljesítmény forgatókönyvek irányítópult naplózást.</span><span class="sxs-lookup"><span data-stu-id="cc95e-316">Disable dashboard logging for high throughput scenarios.</span></span> <span data-ttu-id="cc95e-317">Alapértelmezés szerint az SDK-t menti el a naplókat tárhelyre, és ez a tevékenység ronthatja a teljesítményt, hány üzenet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="cc95e-317">By default, the SDK writes logs to storage, and this activity could degrade performance when you are processing many messages.</span></span> <span data-ttu-id="cc95e-318">Tiltsa le a naplózást, állítsa be az irányítópult a karakterlánc null értékű a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc95e-318">To disable logging, set the dashboard connection string to null as shown in the following example.</span></span>

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

<span data-ttu-id="cc95e-319">A következő példa bemutatja a naplók írni többféle módon:</span><span class="sxs-lookup"><span data-stu-id="cc95e-319">The following example shows several ways to write logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="cc95e-320">A WebJobs SDK irányítópulton kimenetét a `TextWriter` objektum mutat be, amikor egy adott oldalt lépjen be meghívása működik, és kattintson a **váltása kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="cc95e-320">In the WebJobs SDK Dashboard, the output from the `TextWriter` object shows up when you go to the page for a particular function invocation and click **Toggle Output**:</span></span>

![Függvény meghívása hivatkozásra](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="cc95e-323">A WebJobs SDK-irányítópulton a legutóbbi 100 sor konzol kimeneti megjelenítése fel a webjobs-feladat (nem a függvény hívása) lépjen a lapra, és kattintson **váltása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="cc95e-323">In the WebJobs SDK Dashboard, the most recent 100 lines of Console output show up when you go to the page for the WebJob (not for the function invocation) and click **Toggle Output**.</span></span>

![Kattintson a Váltás kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

<span data-ttu-id="cc95e-325">Egy folyamatos webjobs-feladat, az alkalmazás naplóiban jelennek meg az/data/feladatok/folyamatos/*{webjobname}*/job_log.txt a webes alkalmazás fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="cc95e-325">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in the web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="cc95e-326">Egy Azure blob-ehhez hasonló alkalmazások naplók megjelenését: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span><span class="sxs-lookup"><span data-stu-id="cc95e-326">In an Azure blob the application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="cc95e-327">Az Azure tábla és a `Console.Out` és `Console.Error` naplók néznek ki:</span><span class="sxs-lookup"><span data-stu-id="cc95e-327">And in an Azure table the `Console.Out` and `Console.Error` logs look like this:</span></span>

![A tábla adatai napló](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Hibanapló tábla](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

<span data-ttu-id="cc95e-330">Ha azt szeretné, hogy csatlakoztassák a saját naplózó, lásd: [ebben a példában](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="cc95e-330">If you want to plug in your own logger, see [this example](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span></span>

## <span data-ttu-id="cc95e-331"><a id="errors"></a>Hibák kezelésének és időtúllépések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc95e-331"><a id="errors"></a>How to handle errors and configure timeouts</span></span>
<span data-ttu-id="cc95e-332">A WebJobs SDK is magában foglalja a [időtúllépés](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) , amely segítségével okozhat a működnek, ha nem fejezi be a megadott időn belül.</span><span class="sxs-lookup"><span data-stu-id="cc95e-332">The WebJobs SDK also includes a [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribute that you can use to cause a function to be canceled if doesn't complete within a specified amount of time.</span></span> <span data-ttu-id="cc95e-333">Ha szeretne egy riasztás, ha túl sok hiba fordulhat elő, a megadott időn belül, és a `ErrorTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="cc95e-333">And if you want to raise an alert when too many errors happen within a specified period of time, you can use the `ErrorTrigger` attribute.</span></span> <span data-ttu-id="cc95e-334">Íme egy [ErrorTrigger példa](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span><span class="sxs-lookup"><span data-stu-id="cc95e-334">Here is an [ErrorTrigger example](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span></span>

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

<span data-ttu-id="cc95e-335">Dinamikusan is letilthatja, és engedélyezi a vezérlőhöz funkcióit, hogy akkor is elindítható, a konfiguráció kapcsoló, amelyet egy Alkalmazásbeállítás vagy a környezeti változó neve.</span><span class="sxs-lookup"><span data-stu-id="cc95e-335">You can also dynamically disable and enable functions to control whether they can be triggered, by using a configuration switch that could be an app setting or environment variable name.</span></span> <span data-ttu-id="cc95e-336">Mintakód, tekintse meg a `Disable` attribútumnak [a WebJobs SDK-minták tárház](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span><span class="sxs-lookup"><span data-stu-id="cc95e-336">For sample code, see the `Disable` attribute in [the WebJobs SDK samples repository](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span></span>

## <span data-ttu-id="cc95e-337"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc95e-337"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="cc95e-338">Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezeli az Azure-üzenetsorok használata gyakori forgatókönyvei.</span><span class="sxs-lookup"><span data-stu-id="cc95e-338">This guide has provided code samples that show how to handle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="cc95e-339">Azure webjobs-feladatok és a WebJobs SDK használatával kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="cc95e-339">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
