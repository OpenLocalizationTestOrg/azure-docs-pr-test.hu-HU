---
title: "a WebJobs SDK hello Azure üzenetsor-kezelési tárolási aaaHow toouse"
description: "Ismerje meg, hogyan toouse Azure várólista hello WebJobs SDK szolgáltatással. Hozzon létre vagy töröljön a várólisták; Helyezze, Belepillantás, get, és törölje az üzenetsor-üzeneteket, és több."
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
ms.openlocfilehash: 49f844436b0453489800b2762a5c7dc30b9db805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-queue-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="83553-104">Hogyan toouse Azure várólista hello WebJobs SDK szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="83553-104">How toouse Azure queue storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="83553-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="83553-105">Overview</span></span>
<span data-ttu-id="83553-106">Ez az útmutató C# mintakódok hogyan toouse hello Azure WebJobs SDK-verzió megjelenítése 1.x az Azure queue storage szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="83553-106">This guide provides C# code samples that show how toouse hello Azure WebJobs SDK version 1.x with hello Azure queue storage service.</span></span>

<span data-ttu-id="83553-107">hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md) vagy túl[több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="83553-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

<span data-ttu-id="83553-108">A legtöbb hello kódtöredékek csak megjelenítése funkciók nem hello hello létrehozó kód mellől `JobHost` objektum ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="83553-108">Most of hello code snippets only show functions, not hello code that creates hello `JobHost` object as in this example:</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

<span data-ttu-id="83553-109">hello útmutató hello a következő témaköröket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="83553-109">hello guide includes hello following topics:</span></span>

* [<span data-ttu-id="83553-110">Hogyan tootrigger a függvény egy várólista-üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="83553-110">How tootrigger a function when a queue message is received</span></span>](#trigger)
  * <span data-ttu-id="83553-111">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-111">String queue messages</span></span>
  * <span data-ttu-id="83553-112">POCO üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-112">POCO queue messages</span></span>
  * <span data-ttu-id="83553-113">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="83553-113">Async functions</span></span>
  * <span data-ttu-id="83553-114">Típusok hello QueueTrigger attribútum együttműködik</span><span class="sxs-lookup"><span data-stu-id="83553-114">Types hello QueueTrigger attribute works with</span></span>
  * <span data-ttu-id="83553-115">Lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="83553-115">Polling algorithm</span></span>
  * <span data-ttu-id="83553-116">Több példánya</span><span class="sxs-lookup"><span data-stu-id="83553-116">Multiple instances</span></span>
  * <span data-ttu-id="83553-117">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="83553-117">Parallel execution</span></span>
  * <span data-ttu-id="83553-118">Várólista vagy várólista üzenet metaadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="83553-118">Get queue or queue message metadata</span></span>
  * <span data-ttu-id="83553-119">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="83553-119">Graceful shutdown</span></span>
* [<span data-ttu-id="83553-120">Hogyan toocreate várólista üzenet-várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="83553-120">How toocreate a queue message while processing a queue message</span></span>](#createqueue)
  * <span data-ttu-id="83553-121">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-121">String queue messages</span></span>
  * <span data-ttu-id="83553-122">POCO üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-122">POCO queue messages</span></span>
  * <span data-ttu-id="83553-123">Hozzon létre több üzenetet vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="83553-123">Create multiple messages or in async functions</span></span>
  * <span data-ttu-id="83553-124">Típusok hello várólista attribútum együttműködik</span><span class="sxs-lookup"><span data-stu-id="83553-124">Types hello Queue attribute works with</span></span>
  * <span data-ttu-id="83553-125">Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="83553-125">Use WebJobs SDK attributes in hello body of a function</span></span>
* [<span data-ttu-id="83553-126">Hogyan tooread és írási blobok egy várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="83553-126">How tooread and write blobs while processing a queue message</span></span>](#blobs)
  * <span data-ttu-id="83553-127">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-127">String queue messages</span></span>
  * <span data-ttu-id="83553-128">POCO üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-128">POCO queue messages</span></span>
  * <span data-ttu-id="83553-129">Típusok hello Blob attribútum együttműködik</span><span class="sxs-lookup"><span data-stu-id="83553-129">Types hello Blob attribute works with</span></span>
* [<span data-ttu-id="83553-130">Hogyan toohandle elhalt üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-130">How toohandle poison messages</span></span>](#poison)
  * <span data-ttu-id="83553-131">Automatikus elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="83553-131">Automatic poison message handling</span></span>
  * <span data-ttu-id="83553-132">Manuális elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="83553-132">Manual poison message handling</span></span>
* [<span data-ttu-id="83553-133">Hogyan tooset konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="83553-133">How tooset configuration options</span></span>](#config)
  * <span data-ttu-id="83553-134">A kód SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="83553-134">Set SDK connection strings in code</span></span>
  * <span data-ttu-id="83553-135">QueueTrigger beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83553-135">Configure QueueTrigger settings</span></span>
  * <span data-ttu-id="83553-136">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="83553-136">Set values for WebJobs SDK constructor parameters in code</span></span>
* [<span data-ttu-id="83553-137">Hogyan tootrigger függvény manuálisan</span><span class="sxs-lookup"><span data-stu-id="83553-137">How tootrigger a function manually</span></span>](#manual)
* [<span data-ttu-id="83553-138">Hogyan naplózza az toowrite</span><span class="sxs-lookup"><span data-stu-id="83553-138">How toowrite logs</span></span>](#logs)
* [<span data-ttu-id="83553-139">Hogyan toohandle hibák és időtúllépések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83553-139">How toohandle errors and configure timeouts</span></span>](#errors)
* [<span data-ttu-id="83553-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83553-140">Next steps</span></span>](#nextsteps)

## <span data-ttu-id="83553-141"><a id="trigger"></a>Hogyan tootrigger a függvény egy várólista-üzenet fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="83553-141"><a id="trigger"></a> How tootrigger a function when a queue message is received</span></span>
<span data-ttu-id="83553-142">toowrite egy függvényt, amely a WebJobs SDK hello meghívja a várólista-üzenet fogadásakor, használja a hello `QueueTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="83553-142">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello `QueueTrigger` attribute.</span></span> <span data-ttu-id="83553-143">hello attribútum konstruktora a hello várólista toopoll hello nevét megadó karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="83553-143">hello attribute constructor takes a string parameter that specifies hello name of hello queue toopoll.</span></span> <span data-ttu-id="83553-144">Emellett [hello várólista nevének beállítása dinamikusan](#config).</span><span class="sxs-lookup"><span data-stu-id="83553-144">You can also [set hello queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="83553-145">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-145">String queue messages</span></span>
<span data-ttu-id="83553-146">A következő példa hello, hello várólista tartalmaz egy karakterlánc-üzenet, így `QueueTrigger` alkalmazott tooa karakterlánc paraméter nevű `logMessage` tartalmazó hello hello várólista üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="83553-146">In hello following example, hello queue contains a string message, so `QueueTrigger` is applied tooa string parameter named `logMessage` which contains hello content of hello queue message.</span></span> <span data-ttu-id="83553-147">hello függvény [írja a naplót üzenet toohello irányítópult](#logs).</span><span class="sxs-lookup"><span data-stu-id="83553-147">hello function [writes a log message toohello Dashboard](#logs).</span></span>

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

<span data-ttu-id="83553-148">Emellett `string`, hello lehet, egy bájttömböt egy `CloudQueueMessage` objektumot, vagy egy Ön által meghatározott POCO.</span><span class="sxs-lookup"><span data-stu-id="83553-148">Besides `string`, hello parameter may be a byte array, a `CloudQueueMessage` object, or a POCO  that you define.</span></span>

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="83553-149">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-149">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="83553-150">A következő példa hello, várólista üdvözlőüzenetére tartalmazza a JSON a `BlobInformation` objektum, amely tartalmazza a `BlobName` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="83553-150">In hello following example, hello queue message contains JSON for a `BlobInformation` object which includes a `BlobName` property.</span></span> <span data-ttu-id="83553-151">hello SDK automatikusan deserializes hello objektum.</span><span class="sxs-lookup"><span data-stu-id="83553-151">hello SDK automatically deserializes hello object.</span></span>

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="83553-152">hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="83553-152">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="83553-153">Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="83553-153">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a><span data-ttu-id="83553-154">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="83553-154">Async functions</span></span>
<span data-ttu-id="83553-155">a következő aszinkron függvény hello [írja a naplót toohello irányítópult](#logs).</span><span class="sxs-lookup"><span data-stu-id="83553-155">hello following async function [writes a log toohello Dashboard](#logs).</span></span>

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

<span data-ttu-id="83553-156">Aszinkron funkciók is igénybe vehet egy [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), ahogy az alábbi példa, amely egy blob másolja hello.</span><span class="sxs-lookup"><span data-stu-id="83553-156">Async functions may take a [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), as shown in hello following example which copies a blob.</span></span> <span data-ttu-id="83553-157">(Hello előzetesben `queueTrigger` helyőrző, lásd: hello [Blobok](#blobs) szakaszában.)</span><span class="sxs-lookup"><span data-stu-id="83553-157">(For an explanation of hello `queueTrigger` placeholder, see hello [Blobs](#blobs) section.)</span></span>

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <span data-ttu-id="83553-158"><a id="qtattributetypes"></a>Típusok hello QueueTrigger attribútum együttműködik</span><span class="sxs-lookup"><span data-stu-id="83553-158"><a id="qtattributetypes"></a> Types hello QueueTrigger attribute works with</span></span>
<span data-ttu-id="83553-159">Használhat `QueueTrigger` a következő típusok hello:</span><span class="sxs-lookup"><span data-stu-id="83553-159">You can use `QueueTrigger` with hello following types:</span></span>

* `string`
* <span data-ttu-id="83553-160">A JSON-ként szerializált POCO típus</span><span class="sxs-lookup"><span data-stu-id="83553-160">A POCO type serialized as JSON</span></span>
* `byte[]`
* `CloudQueueMessage`

### <span data-ttu-id="83553-161"><a id="polling"></a>Lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="83553-161"><a id="polling"></a> Polling algorithm</span></span>
<span data-ttu-id="83553-162">hello SDK valósítja meg a tétlen-várólista tárolási tranzakciós költségeket a lekérdezési véletlenszerű exponenciális vissza az indító algoritmus tooreduce hello hatását.</span><span class="sxs-lookup"><span data-stu-id="83553-162">hello SDK implements a random exponential back-off algorithm tooreduce hello effect of idle-queue polling on storage transaction costs.</span></span>  <span data-ttu-id="83553-163">Ha üzenet található, hello SDK két másodpercet vár, és ellenőrzi, egy másik üzenet; Ha üzenet található azt, mielőtt újra próbálkozna körülbelül négy másodpercet vár.</span><span class="sxs-lookup"><span data-stu-id="83553-163">When a message is found, hello SDK waits two seconds and then checks for another message; when no message is found it waits about four seconds before trying again.</span></span> <span data-ttu-id="83553-164">Későbbi sikertelen bejelentkezési kísérletek tooget egy üzenetsor-üzenetet, miután a hello várakozási idő tooincrease folytatódik, amíg eléri a maximális várakozási idő hello, mely alapértelmezett tooone perc.</span><span class="sxs-lookup"><span data-stu-id="83553-164">After subsequent failed attempts tooget a queue message, hello wait time continues tooincrease until it reaches hello maximum wait time, which defaults tooone minute.</span></span> <span data-ttu-id="83553-165">[hello maximális várakozási idő az konfigurálható](#config).</span><span class="sxs-lookup"><span data-stu-id="83553-165">[hello maximum wait time is configurable](#config).</span></span>

### <span data-ttu-id="83553-166"><a id="instances"></a>Több példánya</span><span class="sxs-lookup"><span data-stu-id="83553-166"><a id="instances"></a> Multiple instances</span></span>
<span data-ttu-id="83553-167">Ha több példányt a webalkalmazás fut, egy folyamatos webjobs-feladat fut az egyes gépek, és minden gép fog Várjon, amíg az eseményindítók és toorun funkciók kísérlet.</span><span class="sxs-lookup"><span data-stu-id="83553-167">If your web app runs on multiple instances, a continuous WebJob runs on each machine, and each machine will wait for triggers and attempt toorun functions.</span></span> <span data-ttu-id="83553-168">hello WebJobs SDK várólista eseményindító automatikusan megakadályozza, hogy egy függvény feldolgozási sor üzenetet többször; funkciók nincs toobe toobe idempotent írása.</span><span class="sxs-lookup"><span data-stu-id="83553-168">hello WebJobs SDK queue trigger automatically prevents a function from processing a queue message multiple times; functions do not have toobe written toobe idempotent.</span></span> <span data-ttu-id="83553-169">Azonban ha azt szeretné, hogy tooensure, hogy egy függvény csak egy példányban fut, akkor is, ha hello állomás webes alkalmazás több példánya van, használhatja a hello `Singleton` attribútum.</span><span class="sxs-lookup"><span data-stu-id="83553-169">However, if you want tooensure that only one instance of a function runs even when there are multiple instances of hello host web app, you can use hello `Singleton` attribute.</span></span>

### <span data-ttu-id="83553-170"><a id="parallel"></a>Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="83553-170"><a id="parallel"></a> Parallel execution</span></span>
<span data-ttu-id="83553-171">Ha több funkciók különböző várólisták figyel, hello SDK fogja hívni azokat párhuzamosan üzenetek fogadása egy időben.</span><span class="sxs-lookup"><span data-stu-id="83553-171">If you have multiple functions listening on different queues, hello SDK will call them in parallel when messages are received simultaneously.</span></span>

<span data-ttu-id="83553-172">hello azonos érvényét veszti, ha több üzenetek fogadása az egy adott sorba.</span><span class="sxs-lookup"><span data-stu-id="83553-172">hello same is true when multiple messages are received for a single queue.</span></span> <span data-ttu-id="83553-173">Alapértelmezés szerint hello SDK egyszerre az 16 várólista üzenetkötegek lekérdezi és feldolgozza azokat párhuzamosan hello függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="83553-173">By default, hello SDK gets a batch of 16 queue messages at a time and executes hello function that processes them in parallel.</span></span> <span data-ttu-id="83553-174">[hello köteg mérete konfigurálható](#config).</span><span class="sxs-lookup"><span data-stu-id="83553-174">[hello batch size is configurable](#config).</span></span> <span data-ttu-id="83553-175">Ha feldolgozott hello szám lefelé toohalf hello kötegelt méretű lekérdezi, hello SDK lekérdezi egy másik köteg, és megkezdi az üzenetek feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="83553-175">When hello number being processed gets down toohalf of hello batch size, hello SDK gets another batch and starts processing those messages.</span></span> <span data-ttu-id="83553-176">Ezért hello maximális száma párhuzamos éppen feldolgozott üzeneteinek másodpercenkénti függvény, egy és egy félig alkalommal hello kötegmérete.</span><span class="sxs-lookup"><span data-stu-id="83553-176">Therefore hello maximum number of concurrent messages being processed per function is one and a half times hello batch size.</span></span> <span data-ttu-id="83553-177">Ez a korlátozás vonatkozik külön-külön tooeach függvény, amely rendelkezik egy `QueueTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="83553-177">This limit applies separately tooeach function that has a `QueueTrigger` attribute.</span></span>

<span data-ttu-id="83553-178">Ha nem szeretné, egy olyan sort a fogadott üzenetet párhuzamos futtatáshoz, hello köteg mérete too1 állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="83553-178">If you don't want parallel execution for messages received on one queue, you can set hello batch size too1.</span></span> <span data-ttu-id="83553-179">Lásd még: **várólista feldolgozása teljesebb körű vezérlése** a [Azure WebJobs SDK 1.1.0-ás RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span><span class="sxs-lookup"><span data-stu-id="83553-179">See also **More control over Queue processing** in [Azure WebJobs SDK 1.1.0 RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).</span></span>

### <span data-ttu-id="83553-180"><a id="queuemetadata"></a>Várólista vagy várólista üzenet metaadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="83553-180"><a id="queuemetadata"></a>Get queue or queue message metadata</span></span>
<span data-ttu-id="83553-181">Kaphat a következő üzenet tulajdonságai toohello metódus-aláírás paraméterek hozzáadásával hello:</span><span class="sxs-lookup"><span data-stu-id="83553-181">You can get hello following message properties by adding parameters toohello method signature:</span></span>

* <span data-ttu-id="83553-182">`DateTimeOffset`expirationTime</span><span class="sxs-lookup"><span data-stu-id="83553-182">`DateTimeOffset` expirationTime</span></span>
* <span data-ttu-id="83553-183">`DateTimeOffset`insertionTime</span><span class="sxs-lookup"><span data-stu-id="83553-183">`DateTimeOffset` insertionTime</span></span>
* <span data-ttu-id="83553-184">`DateTimeOffset`nextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="83553-184">`DateTimeOffset` nextVisibleTime</span></span>
* <span data-ttu-id="83553-185">`string`queueTrigger (tartalmazza a szöveges üzenet)</span><span class="sxs-lookup"><span data-stu-id="83553-185">`string` queueTrigger (contains message text)</span></span>
* <span data-ttu-id="83553-186">`string`azonosítója</span><span class="sxs-lookup"><span data-stu-id="83553-186">`string` id</span></span>
* <span data-ttu-id="83553-187">`string`popReceipt</span><span class="sxs-lookup"><span data-stu-id="83553-187">`string` popReceipt</span></span>
* <span data-ttu-id="83553-188">`int`dequeueCount</span><span class="sxs-lookup"><span data-stu-id="83553-188">`int` dequeueCount</span></span>

<span data-ttu-id="83553-189">Ha azt szeretné, közvetlenül az Azure storage API hello toowork, azt is megteheti egy `CloudStorageAccount` paraméter.</span><span class="sxs-lookup"><span data-stu-id="83553-189">If you want toowork directly with hello Azure storage API, you can also add a `CloudStorageAccount` parameter.</span></span>

<span data-ttu-id="83553-190">hello alábbi példa írja az összes a metaadatok tooan INFO alkalmazásnaplóban.</span><span class="sxs-lookup"><span data-stu-id="83553-190">hello following example writes all of this metadata tooan INFO application log.</span></span> <span data-ttu-id="83553-191">Hello példában logMessage és a queueTrigger tartalma hello hello várólista üzenet.</span><span class="sxs-lookup"><span data-stu-id="83553-191">In hello example, both logMessage and queueTrigger contain hello content of hello queue message.</span></span>

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

<span data-ttu-id="83553-192">Íme egy mintanaplót hello mintakód szerint:</span><span class="sxs-lookup"><span data-stu-id="83553-192">Here is a sample log written by hello sample code:</span></span>

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <span data-ttu-id="83553-193"><a id="graceful"></a>Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="83553-193"><a id="graceful"></a>Graceful shutdown</span></span>
<span data-ttu-id="83553-194">Egy függvény a folyamatos webjobs-feladat fut fogad el egy `CancellationToken` paramétert, amely lehetővé teszi, hogy ha hello hello operációs rendszer toonotify hello függvény a webjobs-feladat megszakadt toobe tárgya.</span><span class="sxs-lookup"><span data-stu-id="83553-194">A function that runs in a continuous WebJob can accept a `CancellationToken` parameter which enables hello operating system toonotify hello function when hello WebJob is about toobe terminated.</span></span> <span data-ttu-id="83553-195">Az értesítési toomake meg arról, hogy hello függvény nem bontható váratlanul körbe, hogy adatok inkonzisztens állapotban is használhatja.</span><span class="sxs-lookup"><span data-stu-id="83553-195">You can use this notification toomake sure hello function doesn't terminate unexpectedly in a way that leaves data in an inconsistent state.</span></span>

<span data-ttu-id="83553-196">a következő példa azt mutatja meg hogyan hello toocheck függvények közelgő webjobs-feladat-záráshoz.</span><span class="sxs-lookup"><span data-stu-id="83553-196">hello following example shows how toocheck for impending WebJob termination in a function.</span></span>

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

<span data-ttu-id="83553-197">**Megjegyzés:** hello irányítópult előfordulhat, hogy helyesen jelenjen meg hello állapota és kimenete, amely le lett állítva a funkciókat.</span><span class="sxs-lookup"><span data-stu-id="83553-197">**Note:** hello Dashboard might not correctly show hello status and output of functions that have been shut down.</span></span>

<span data-ttu-id="83553-198">További információkért lásd: [webjobs-feladatok szabályos leállítást](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span><span class="sxs-lookup"><span data-stu-id="83553-198">For more information, see [WebJobs Graceful Shutdown](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).</span></span>   

## <span data-ttu-id="83553-199"><a id="createqueue"></a>Hogyan toocreate várólista üzenet-várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="83553-199"><a id="createqueue"></a> How toocreate a queue message while processing a queue message</span></span>
<span data-ttu-id="83553-200">egy függvényt, amely létrehoz egy új sor üzenetet, használjon hello toowrite `Queue` attribútum.</span><span class="sxs-lookup"><span data-stu-id="83553-200">toowrite a function that creates a new queue message, use hello `Queue` attribute.</span></span> <span data-ttu-id="83553-201">Például `QueueTrigger`, adja meg a várólistacímke hello karakterláncból, vagy beállíthatja [hello várólista nevének beállítása dinamikusan](#config).</span><span class="sxs-lookup"><span data-stu-id="83553-201">Like `QueueTrigger`, you pass in hello queue name as a string or you can [set hello queue name dynamically](#config).</span></span>

### <a name="string-queue-messages"></a><span data-ttu-id="83553-202">Karakterlánc-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-202">String queue messages</span></span>
<span data-ttu-id="83553-203">a következő példakód nem aszinkron hello hello várólista "outputqueue" nevű új várólista üzenet ugyanaz tartalom másként hello üzenetsorban található üzenetet kapott hello várólista "inputqueue" nevű hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="83553-203">hello following non-async code sample creates a new queue message in hello queue named "outputqueue" with hello same content as hello queue message received in hello queue named "inputqueue".</span></span> <span data-ttu-id="83553-204">(Aszinkron funkciók használata `IAsyncCollector<T>` később az itt látható módon.)</span><span class="sxs-lookup"><span data-stu-id="83553-204">(For async functions use `IAsyncCollector<T>` as shown later in this section.)</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a><span data-ttu-id="83553-205">POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-205">POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="83553-206">toocreate egy kimeneti paraméter toohello írja egy üzenetsor-üzenetet, amely tartalmazza a karakterláncot, pass hello POCO helyett egy POCO `Queue` attribútumok konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="83553-206">toocreate a queue message that contains a POCO rather than a string, pass hello POCO type as an output parameter toohello `Queue` attribute constructor.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

<span data-ttu-id="83553-207">hello SDK automatikusan hello objektum tooJSON rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="83553-207">hello SDK automatically serializes hello object tooJSON.</span></span> <span data-ttu-id="83553-208">Egy üzenetsor mindig létrejön, még akkor is, ha hello objektum értéke null.</span><span class="sxs-lookup"><span data-stu-id="83553-208">A queue message is always created, even if hello object is null.</span></span>

### <a name="create-multiple-messages-or-in-async-functions"></a><span data-ttu-id="83553-209">Hozzon létre több üzenetet vagy aszinkron függvény</span><span class="sxs-lookup"><span data-stu-id="83553-209">Create multiple messages or in async functions</span></span>
<span data-ttu-id="83553-210">toocreate több üzenetet, hogy hello paramétertípus hello kimeneti várólista `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="83553-210">toocreate multiple messages, make hello parameter type for hello output queue `ICollector<T>` or `IAsyncCollector<T>`, as shown in hello following example.</span></span>

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="83553-211">Minden várólista-üzenet létrehozásakor a rendszer azonnal hello `Add` metódust.</span><span class="sxs-lookup"><span data-stu-id="83553-211">Each queue message is created immediately when hello `Add` method is called.</span></span>

### <a name="types-that-hello-queue-attribute-works-with"></a><span data-ttu-id="83553-212">Típusok hello várólista attribútum, amely együttműködik</span><span class="sxs-lookup"><span data-stu-id="83553-212">Types that hello Queue attribute works with</span></span>
<span data-ttu-id="83553-213">Használhatja a hello `Queue` hello paramétereinek típusa a következő attribútumot:</span><span class="sxs-lookup"><span data-stu-id="83553-213">You can use hello `Queue` attribute on hello following parameter types:</span></span>

* <span data-ttu-id="83553-214">`out string`(hoz létre a üzenetsor-üzenetet, ha a paraméter értéke nem null értékű hello függvény végén)</span><span class="sxs-lookup"><span data-stu-id="83553-214">`out string` (creates queue message if parameter value is non-null when hello function ends)</span></span>
* <span data-ttu-id="83553-215">`out byte[]`(működik, mint az `string`)</span><span class="sxs-lookup"><span data-stu-id="83553-215">`out byte[]` (works like `string`)</span></span>
* <span data-ttu-id="83553-216">`out CloudQueueMessage`(működik, mint az `string`)</span><span class="sxs-lookup"><span data-stu-id="83553-216">`out CloudQueueMessage` (works like `string`)</span></span>
* <span data-ttu-id="83553-217">`out POCO`(egy szerializálható típust, létrehoz egy üzenetet null objektummal rendelkező Ha hello paraméterének értéke null, ha hello függvény karakterlánccal végződik-e)</span><span class="sxs-lookup"><span data-stu-id="83553-217">`out POCO` (a serializable type, creates a message with a null object if hello paramter is null when hello function ends)</span></span>
* `ICollector`
* `IAsyncCollector`
* <span data-ttu-id="83553-218">`CloudQueue`(az üzenet segítségével manuálisan közvetlenül hello Azure Storage API létrehozása)</span><span class="sxs-lookup"><span data-stu-id="83553-218">`CloudQueue` (for creating messages manually using hello Azure Storage API directly)</span></span>

### <span data-ttu-id="83553-219"><a id="ibinder"></a>Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="83553-219"><a id="ibinder"></a>Use WebJobs SDK attributes in hello body of a function</span></span>
<span data-ttu-id="83553-220">Ha toodo kell néhány működik a függvény a WebJobs SDK attribútum használatához `Queue`, `Blob`, vagy `Table`, használhatja a hello `IBinder` felületet.</span><span class="sxs-lookup"><span data-stu-id="83553-220">If you need toodo some work in your function before using a WebJobs SDK attribute such as `Queue`, `Blob`, or `Table`, you can use hello `IBinder` interface.</span></span>

<span data-ttu-id="83553-221">a következő példa hello időt vesz igénybe egy bemeneti várólista-üzenet, és hoz létre egy új üzenet hello azonos kimeneti várólistában lévő tartalom.</span><span class="sxs-lookup"><span data-stu-id="83553-221">hello following example takes an input queue message and creates a new message with hello same content in an output queue.</span></span> <span data-ttu-id="83553-222">hello kimeneti várólista neve hello függvénytörzs hello kóddal van beállítva.</span><span class="sxs-lookup"><span data-stu-id="83553-222">hello output queue name is set by code in hello body of hello function.</span></span>

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

<span data-ttu-id="83553-223">Hello `IBinder` felület is használható hello `Table` és `Blob` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="83553-223">hello `IBinder` interface can also be used with hello `Table` and `Blob` attributes.</span></span>

## <span data-ttu-id="83553-224"><a id="blobs"></a>Hogyan tooread és írási blobok és táblák egy várólista üzenet feldolgozása közben</span><span class="sxs-lookup"><span data-stu-id="83553-224"><a id="blobs"></a> How tooread and write blobs and tables while processing a queue message</span></span>
<span data-ttu-id="83553-225">Hello `Blob` és `Table` attribútumok lehetővé teszik a tooread, és írja blobok és táblákat.</span><span class="sxs-lookup"><span data-stu-id="83553-225">hello `Blob` and `Table` attributes enable you tooread and write blobs and tables.</span></span> <span data-ttu-id="83553-226">Ebben a szakaszban hello minták tooblobs alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="83553-226">hello samples in this section apply tooblobs.</span></span> <span data-ttu-id="83553-227">Hogyan tootrigger dolgozza fel, ha a blobok létrehozott vagy frissített megjelenítése mintakódok, lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), és mintakódok olvasási és írási táblák, lásd: [hogyan toouse Azure-tábla tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="83553-227">For code samples that show how tootrigger processes when blobs are created or updated, see [How toouse Azure blob storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), and for code samples that read and write tables, see [How toouse Azure table storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).</span></span>

### <a name="string-queue-messages-triggering-blob-operations"></a><span data-ttu-id="83553-228">Karakterlánc üzenetek blob műveletek időt.</span><span class="sxs-lookup"><span data-stu-id="83553-228">String queue messages triggering blob operations</span></span>
<span data-ttu-id="83553-229">Egy karakterláncot tartalmazó várólista üzenet `queueTrigger` hello használható helyőrző `Blob` attribútum `blobPath` üdvözlőüzenetére hello tartalmát tartalmazó paraméter.</span><span class="sxs-lookup"><span data-stu-id="83553-229">For a queue message that contains a string, `queueTrigger` is a placeholder you can use in hello `Blob` attribute's `blobPath` parameter that contains hello contents of hello message.</span></span>

<span data-ttu-id="83553-230">hello alábbi példában `Stream` blobok tooread és írási objektumokat.</span><span class="sxs-lookup"><span data-stu-id="83553-230">hello following example uses `Stream` objects tooread and write blobs.</span></span> <span data-ttu-id="83553-231">várólista üdvözlőüzenetére hello textblobs tárolóban lévő blob hello neve.</span><span class="sxs-lookup"><span data-stu-id="83553-231">hello queue message is hello name of a blob located in hello textblobs container.</span></span> <span data-ttu-id="83553-232">Hello blob másolatát "-új" hozzáfűzött toohello neve jön létre az hello azonos tároló.</span><span class="sxs-lookup"><span data-stu-id="83553-232">A copy of hello blob with "-new" appended toohello name is created in hello same container.</span></span>

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="83553-233">Hello `Blob` attribútum konstruktora vesz egy `blobPath` paraméter meghatározza, hogy hello tároló és a blob neve.</span><span class="sxs-lookup"><span data-stu-id="83553-233">hello `Blob` attribute constructor takes a `blobPath` parameter that specifies hello container and blob name.</span></span> <span data-ttu-id="83553-234">A helyőrző kapcsolatos további információkért lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span><span class="sxs-lookup"><span data-stu-id="83553-234">For more information about this placeholder, see [How toouse Azure blob storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),</span></span>

<span data-ttu-id="83553-235">Ha hello attribútum decorates egy `Stream` objektumot egy másik konstruktor paraméterrel hello `FileAccess` olvasási, írási vagy olvasási/írási módban.</span><span class="sxs-lookup"><span data-stu-id="83553-235">When hello attribute decorates a `Stream` object, another constructor parameter specifies hello `FileAccess` mode as read, write, or read/write.</span></span>

<span data-ttu-id="83553-236">hello alábbi példában egy `CloudBlockBlob` toodelete blob objektum.</span><span class="sxs-lookup"><span data-stu-id="83553-236">hello following example uses a `CloudBlockBlob` object toodelete a blob.</span></span> <span data-ttu-id="83553-237">várólista üdvözlőüzenetére hello blob hello neve.</span><span class="sxs-lookup"><span data-stu-id="83553-237">hello queue message is hello name of hello blob.</span></span>

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <span data-ttu-id="83553-238"><a id="pocoblobs"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-238"><a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) queue messages</span></span>
<span data-ttu-id="83553-239">Egy POCO hello üzenetsor JSON-ként tárolja, használhatja, amely hello hello objektumának neve helyőrzők `Queue` attribútum `blobPath` paraméter.</span><span class="sxs-lookup"><span data-stu-id="83553-239">For a POCO stored as JSON in hello queue message, you can use placeholders that name properties of hello object in hello `Queue` attribute's `blobPath` parameter.</span></span> <span data-ttu-id="83553-240">Is [metaadatok tulajdonságnevek várólistára](#queuemetadata) helyőrzőként.</span><span class="sxs-lookup"><span data-stu-id="83553-240">You can also use [queue metadata property names](#queuemetadata) as placeholders.</span></span>

<span data-ttu-id="83553-241">hello alábbi példa másolja át a blob tooa új blob eltérő kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="83553-241">hello following example copies a blob tooa new blob with a different extension.</span></span> <span data-ttu-id="83553-242">hello várólista az üzenet egy `BlobInformation` , amely tartalmazza az objektum `BlobName` és `BlobNameWithoutExtension` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="83553-242">hello queue message is a `BlobInformation` object that includes `BlobName` and `BlobNameWithoutExtension` properties.</span></span> <span data-ttu-id="83553-243">hello tulajdonságnevek helyőrzőként hello blob elérési út a hello `Blob` attribútumok.</span><span class="sxs-lookup"><span data-stu-id="83553-243">hello property names are used as placeholders in hello blob path for hello `Blob` attributes.</span></span>

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

<span data-ttu-id="83553-244">hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="83553-244">hello SDK uses hello [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize and deserialize messages.</span></span> <span data-ttu-id="83553-245">Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="83553-245">If you create queue messages in a program that doesn't use hello WebJobs SDK, you can write code like hello following example toocreate a POCO queue message that hello SDK can parse.</span></span>

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

<span data-ttu-id="83553-246">Ha néhány működik toodo a függvényben előtt kötelező egy blob tooan objektum, használhatja a hello attribútum hello függvénytörzs hello, [ahogy korábban hello várólista attribútum](#ibinder).</span><span class="sxs-lookup"><span data-stu-id="83553-246">If you need toodo some work in your function before binding a blob tooan object, you can use hello attribute in hello body of hello function, [as shown earlier for hello Queue attribute](#ibinder).</span></span>

### <span data-ttu-id="83553-247"><a id="blobattributetypes"></a>Adattípusok hello Blob-attribútum</span><span class="sxs-lookup"><span data-stu-id="83553-247"><a id="blobattributetypes"></a> Types you can use hello Blob attribute with</span></span>
<span data-ttu-id="83553-248">Hello `Blob` attribútum is használható a következő típusok hello:</span><span class="sxs-lookup"><span data-stu-id="83553-248">hello `Blob` attribute can be used with hello following types:</span></span>

* <span data-ttu-id="83553-249">`Stream`(olvasására vagy írására, hello FileAccess konstruktor paraméterrel megadott)</span><span class="sxs-lookup"><span data-stu-id="83553-249">`Stream` (read or write, specified by using hello FileAccess constructor parameter)</span></span>
* `TextReader`
* `TextWriter`
* <span data-ttu-id="83553-250">`string`(olvasás)</span><span class="sxs-lookup"><span data-stu-id="83553-250">`string` (read)</span></span>
* <span data-ttu-id="83553-251">`out string`(írási; hoz létre egy blobot, csak ha hello karakterlánc-paraméter null értékű hello függvény)</span><span class="sxs-lookup"><span data-stu-id="83553-251">`out string` (write; creates a blob only if hello string parameter is non-null when hello function returns)</span></span>
* <span data-ttu-id="83553-252">POCO (olvasás)</span><span class="sxs-lookup"><span data-stu-id="83553-252">POCO (read)</span></span>
* <span data-ttu-id="83553-253">kimenő POCO (írás; mindig létrehoz egy blob, mint null objektumot hoz létre, ha POCO paraméter értéke null, ha hello függvény)</span><span class="sxs-lookup"><span data-stu-id="83553-253">out POCO (write; always creates a blob, creates as null object if POCO parameter is null when hello function returns)</span></span>
* <span data-ttu-id="83553-254">`CloudBlobStream`(írja)</span><span class="sxs-lookup"><span data-stu-id="83553-254">`CloudBlobStream` (write)</span></span>
* <span data-ttu-id="83553-255">`ICloudBlob`(olvasás vagy írás)</span><span class="sxs-lookup"><span data-stu-id="83553-255">`ICloudBlob` (read or write)</span></span>
* <span data-ttu-id="83553-256">`CloudBlockBlob`(olvasás vagy írás)</span><span class="sxs-lookup"><span data-stu-id="83553-256">`CloudBlockBlob` (read or write)</span></span>
* <span data-ttu-id="83553-257">`CloudPageBlob`(olvasás vagy írás)</span><span class="sxs-lookup"><span data-stu-id="83553-257">`CloudPageBlob` (read or write)</span></span>

## <span data-ttu-id="83553-258"><a id="poison"></a>Hogyan toohandle elhalt üzenetek</span><span class="sxs-lookup"><span data-stu-id="83553-258"><a id="poison"></a> How toohandle poison messages</span></span>
<span data-ttu-id="83553-259">Neve üzeneteket, amelyek tartalmát a függvény toofail hatására *elhalt üzenetek*.</span><span class="sxs-lookup"><span data-stu-id="83553-259">Messages whose content causes a function toofail are called *poison messages*.</span></span> <span data-ttu-id="83553-260">Hello függvény meghibásodása esetén üdvözlőüzenetére várólista nem törlődik, és végül van felvételre újra, ezért hello ciklus toobe ismétlődik.</span><span class="sxs-lookup"><span data-stu-id="83553-260">When hello function fails, hello queue message is not deleted and eventually is picked up again, causing hello cycle toobe repeated.</span></span> <span data-ttu-id="83553-261">hello SDK automatikusan megszakíthatja hello ciklus korlátozott számú ismétlés után, vagy manuálisan is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="83553-261">hello SDK can automatically interrupt hello cycle after a limited number of iterations, or you can do it manually.</span></span>

### <a name="automatic-poison-message-handling"></a><span data-ttu-id="83553-262">Automatikus elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="83553-262">Automatic poison message handling</span></span>
<span data-ttu-id="83553-263">hello SDK telefonhívásokhoz too5 alkalommal tooprocess egy üzenetsor be egy olyan függvényt.</span><span class="sxs-lookup"><span data-stu-id="83553-263">hello SDK will call a function up too5 times tooprocess a queue message.</span></span> <span data-ttu-id="83553-264">Hello ötödik próbálja meghibásodásakor hello üzenet áthelyezett tooa poison várólista.</span><span class="sxs-lookup"><span data-stu-id="83553-264">If hello fifth try fails, hello message is moved tooa poison queue.</span></span> <span data-ttu-id="83553-265">[Az újrapróbálkozások maximális számát hello konfigurálható](#config).</span><span class="sxs-lookup"><span data-stu-id="83553-265">[hello maximum number of retries is configurable](#config).</span></span>

<span data-ttu-id="83553-266">hello poison várólista nevű *{originalqueuename}*-elhalt.</span><span class="sxs-lookup"><span data-stu-id="83553-266">hello poison queue is named *{originalqueuename}*-poison.</span></span> <span data-ttu-id="83553-267">Írhat egy függvény tooprocess üzenetek hello poison várólistából naplózásukhoz vagy, hogy szükség van-e a kézi beavatkozást értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="83553-267">You can write a function tooprocess messages from hello poison queue by logging them or sending a notification that manual attention is needed.</span></span>

<span data-ttu-id="83553-268">A következő példa hello hello `CopyBlob` függvény sikertelenek lesznek, amikor egy üzenetsor-üzenetet, amely nem létezik blob hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="83553-268">In hello following example hello `CopyBlob` function will fail when a queue message contains hello name of a blob that doesn't exist.</span></span> <span data-ttu-id="83553-269">Ebben az esetben üdvözlőüzenetére áthelyeznek hello copyblobqueue várólista toohello copyblobqueue-poison várólista.</span><span class="sxs-lookup"><span data-stu-id="83553-269">When that happens, hello message is moved from hello copyblobqueue queue toohello copyblobqueue-poison queue.</span></span> <span data-ttu-id="83553-270">Hello `ProcessPoisonMessage` majd naplók hello az elhalt üzenet.</span><span class="sxs-lookup"><span data-stu-id="83553-270">hello `ProcessPoisonMessage` then logs hello poison message.</span></span>

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

<span data-ttu-id="83553-271">hello alábbi ábrán az alábbi funkciók a konzol kimeneti az elhalt üzenet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="83553-271">hello following illustration shows console output from these functions when a poison message is processed.</span></span>

![Elhalt üzenetek kezelésének a konzol kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a><span data-ttu-id="83553-273">Manuális elhalt üzenetek kezelésének</span><span class="sxs-lookup"><span data-stu-id="83553-273">Manual poison message handling</span></span>
<span data-ttu-id="83553-274">Hello szám, ahányszor egy üzenet felvételre feldolgozásra kaphat hozzáadásával egy `int` nevű paraméter `dequeueCount` tooyour függvény.</span><span class="sxs-lookup"><span data-stu-id="83553-274">You can get hello number of times a message has been picked up for processing by adding an `int` parameter named `dequeueCount` tooyour function.</span></span> <span data-ttu-id="83553-275">Ezután a jelölőnégyzet hello created funkciókódot száma, és hajtsa végre a saját elhalt üzenetek kezelésének amikor hello száma meghaladja a küszöbértéket, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="83553-275">You can then check hello dequeue count in function code and perform your own poison message handling when hello number exceeds a threshold, as shown in hello following example.</span></span>

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

## <span data-ttu-id="83553-276"><a id="config"></a>Hogyan tooset konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="83553-276"><a id="config"></a> How tooset configuration options</span></span>
<span data-ttu-id="83553-277">Használhatja a hello `JobHostConfiguration` típus tooset hello alábbi konfigurációs beállítások:</span><span class="sxs-lookup"><span data-stu-id="83553-277">You can use hello `JobHostConfiguration` type tooset hello following configuration options:</span></span>

* <span data-ttu-id="83553-278">Hello SDK kapcsolati karakterláncok beállítása a kódban.</span><span class="sxs-lookup"><span data-stu-id="83553-278">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="83553-279">Konfigurálása `QueueTrigger` beállításokat, például a maximális created száma.</span><span class="sxs-lookup"><span data-stu-id="83553-279">Configure `QueueTrigger` settings such as maximum dequeue count.</span></span>
* <span data-ttu-id="83553-280">Várólistanevek konfigurációs beolvasása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="83553-280">Get queue names from configuration.</span></span>

### <span data-ttu-id="83553-281"><a id="setconnstr"></a>A kód SDK kapcsolati karakterláncok beállítása</span><span class="sxs-lookup"><span data-stu-id="83553-281"><a id="setconnstr"></a>Set SDK connection strings in code</span></span>
<span data-ttu-id="83553-282">Hello SDK kapcsolati karakterláncok beállítása a kód lehetővé teszi, hogy Ön toouse saját kapcsolódási karakterlánc neve, a konfigurációs fájlok vagy a környezeti változók, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="83553-282">Setting hello SDK connection strings in code enables you toouse your own connection string names in configuration files or environment variables, as shown in hello following example.</span></span>

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

### <span data-ttu-id="83553-283"><a id="configqueue"></a>QueueTrigger beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83553-283"><a id="configqueue"></a>Configure QueueTrigger  settings</span></span>
<span data-ttu-id="83553-284">Hello következő toohello várólista üzenetfeldolgozást vonatkozó beállításokat konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="83553-284">You can configure hello following settings that apply toohello queue message processing:</span></span>

* <span data-ttu-id="83553-285">hello egyidejűleg végre párhuzamosan toobe átveszik várólista üzenetek maximális száma (alapértelmezett érték 16).</span><span class="sxs-lookup"><span data-stu-id="83553-285">hello maximum number of queue messages that are picked up simultaneously toobe executed in parallel (default is 16).</span></span>
* <span data-ttu-id="83553-286">Az újrapróbálkozások maximális számát hello tooa poison várólista várólista üzenet elküldése előtt (alapértelmezett érték 5).</span><span class="sxs-lookup"><span data-stu-id="83553-286">hello maximum number of retries before a queue message is sent tooa poison queue (default is 5).</span></span>
* <span data-ttu-id="83553-287">hello maximális várakozási idő előtt lekérdezési újra, ha üres-e a várólista (alapértelmezett érték 1 perc).</span><span class="sxs-lookup"><span data-stu-id="83553-287">hello maximum wait time before polling again when a queue is empty (default is 1 minute).</span></span>

<span data-ttu-id="83553-288">a következő példa azt mutatja meg hogyan hello tooconfigure ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="83553-288">hello following example shows how tooconfigure these settings:</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <span data-ttu-id="83553-289"><a id="setnamesincode"></a>Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="83553-289"><a id="setnamesincode"></a>Set values for WebJobs SDK constructor parameters in code</span></span>
<span data-ttu-id="83553-290">A várólista nevét, a blob neve vagy a tároló néha szeretné toospecify, vagy egy táblázatot a merevlemez-kód helyett a kód adjon neki nevet.</span><span class="sxs-lookup"><span data-stu-id="83553-290">Sometimes you want toospecify a queue name, a blob name or container, or a table name in code rather than hard-code it.</span></span> <span data-ttu-id="83553-291">Például érdemes toospecify hello várólista nevét `QueueTrigger` a konfigurációs fájl vagy a környezeti változóban.</span><span class="sxs-lookup"><span data-stu-id="83553-291">For example, you might want toospecify hello queue name for `QueueTrigger` in a configuration file or environment variable.</span></span>

<span data-ttu-id="83553-292">A történő ehhez a `NameResolver` toohello objektum `JobHostConfiguration` típusa.</span><span class="sxs-lookup"><span data-stu-id="83553-292">You can do that by passing in a `NameResolver` object toohello `JobHostConfiguration` type.</span></span> <span data-ttu-id="83553-293">Ön százalékjel (%) jelentkezik be a WebJobs SDK attribútum konstruktorparaméterek körülvett különleges helyőrzőket tartalmaznak, és a `NameResolver` kód hello tényleges értékek toobe használja ezeket a helyőrzők helyett határozza meg.</span><span class="sxs-lookup"><span data-stu-id="83553-293">You include special placeholders surrounded by percent (%) signs in WebJobs SDK attribute constructor parameters, and your `NameResolver` code specifies hello actual values toobe used in place of those placeholders.</span></span>

<span data-ttu-id="83553-294">Tegyük fel, hogy azt szeretné, hogy toouse várólista neve például hello tesztkörnyezetben logqueuetest és egy elnevezett logqueueprod éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="83553-294">For example, suppose you want toouse a queue named logqueuetest in hello test environment and one named logqueueprod in production.</span></span> <span data-ttu-id="83553-295">A kódolt várólista neve helyett toospecify hello név egyik bejegyzésének hello kívánt `appSettings` gyűjteményt, amely rendelkezik hello tényleges várólista neve.</span><span class="sxs-lookup"><span data-stu-id="83553-295">Instead of a hard-coded queue name, you want toospecify hello name of an entry in hello `appSettings` collection that would have hello actual queue name.</span></span> <span data-ttu-id="83553-296">Ha hello `appSettings` kulcs logqueue, a függvény a következő példa hello nézhet.</span><span class="sxs-lookup"><span data-stu-id="83553-296">If hello `appSettings` key is logqueue, your function could look like hello following example.</span></span>

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

<span data-ttu-id="83553-297">A `NameResolver` osztály majd sikerült hello várólista nevét a `appSettings` a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="83553-297">Your `NameResolver` class could then get hello queue name from `appSettings` as shown in hello following example:</span></span>

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

<span data-ttu-id="83553-298">Hello át `NameResolver` toohello osztály `JobHost` objektumot, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="83553-298">You pass hello `NameResolver` class in toohello `JobHost` object as shown in hello following example.</span></span>

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

<span data-ttu-id="83553-299">**Megjegyzés:** várólista, a táblának és a blob nevének feloldása minden alkalommal, amikor egy függvény hívása esetén, de a blob-tároló nevének feloldása csak akkor, ha hello alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="83553-299">**Note:** Queue, table, and blob names are resolved each time a function is called, but blob container names are resolved only when hello application starts.</span></span> <span data-ttu-id="83553-300">A blob-tároló neve nem módosítható, hello feladat futása közben.</span><span class="sxs-lookup"><span data-stu-id="83553-300">You can't change blob container name while hello job is running.</span></span>

## <span data-ttu-id="83553-301"><a id="manual"></a>Hogyan tootrigger függvény manuálisan</span><span class="sxs-lookup"><span data-stu-id="83553-301"><a id="manual"></a>How tootrigger a function manually</span></span>
<span data-ttu-id="83553-302">egy függvény tootrigger manuálisan, használja a hello `Call` vagy `CallAsync` hello metódusa `JobHost` objektum és hello `NoAutomaticTrigger` hello függvény, ahogy az alábbi példa hello attribútuma.</span><span class="sxs-lookup"><span data-stu-id="83553-302">tootrigger a function manually, use hello `Call` or `CallAsync` method on hello `JobHost` object and hello `NoAutomaticTrigger` attribute on hello function, as shown in hello following example.</span></span>

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

## <span data-ttu-id="83553-303"><a id="logs"></a>Hogyan naplózza az toowrite</span><span class="sxs-lookup"><span data-stu-id="83553-303"><a id="logs"></a>How toowrite logs</span></span>
<span data-ttu-id="83553-304">hello irányítópulton látható naplók két helyen: hello webjobs-feladat hello lapját, és egy adott webjobs-feladat meghíváshoz hello lap.</span><span class="sxs-lookup"><span data-stu-id="83553-304">hello Dashboard shows logs in two places: hello page for hello WebJob, and hello page for a particular WebJob invocation.</span></span>

![Naplózza a webjobs-feladat lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="83553-307">Egy függvény vagy hello hívó konzol módszerek kimenete `Main()` metódus a hello irányítópult-oldalon hello webjobs-feladat, és nem egy adott metódushívás hello oldalán megjelenik.</span><span class="sxs-lookup"><span data-stu-id="83553-307">Output from Console methods that you call in a function or in hello `Main()` method appears in hello Dashboard page for hello WebJob, not in hello page for a particular method invocation.</span></span> <span data-ttu-id="83553-308">Hello TextWriter objektum, hogy a paraméter a a metódus aláírásában kimenete hello irányítópult-oldalon metódushívási jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="83553-308">Output from hello TextWriter object that you get from a parameter in your method signature appears in hello Dashboard page for a method invocation.</span></span>

<span data-ttu-id="83553-309">Konzol kimeneti nem lehet csatolt tooa adott metódushívás mert hello konzol egyszálas, miközben sok munkakörök futtathatnak: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="83553-309">Console output can't be linked tooa particular method invocation because hello Console is single-threaded, while many job functions may be running at hello same time.</span></span> <span data-ttu-id="83553-310">Ezért hello SDK biztosít minden függvény meghívása a saját egyedi napló író objektummal.</span><span class="sxs-lookup"><span data-stu-id="83553-310">That's why hello  SDK provides each function invocation with its own unique log writer object.</span></span>

<span data-ttu-id="83553-311">toowrite [alkalmazás nyomkövetési naplóit](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), használjon `Console.Out` (INFO jelölésű naplókat hoz létre) és `Console.Error` (a hiba jelöléssel naplókat hoz létre).</span><span class="sxs-lookup"><span data-stu-id="83553-311">toowrite [application tracing logs](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), use `Console.Out` (creates logs marked as INFO) and `Console.Error` (creates logs marked as ERROR).</span></span> <span data-ttu-id="83553-312">A másik lehetőség az toouse [nyomkövetési vagy TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), amely biztosítja, hogy részletes, figyelmeztetés, és kritikus szintek hozzáadása tooInfo és a hiba.</span><span class="sxs-lookup"><span data-stu-id="83553-312">An alternative is toouse [Trace or TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), which provides Verbose, Warning, and Critical levels in addition tooInfo and Error.</span></span> <span data-ttu-id="83553-313">Alkalmazás nyomkövetési naplóit hello webes alkalmazások naplófájljainak, Azure-táblákban, jelenik meg, vagy az Azure-blobok attól függően, hogy hogyan konfigurálja az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="83553-313">Application tracing logs appear in hello web app log files, Azure tables, or Azure blobs depending on how you configure your Azure web app.</span></span> <span data-ttu-id="83553-314">Mivel minden a konzol kimeneti értéke igaz, hello legutóbbi 100 alkalmazásnaplók a hello irányítópult-oldalon hello webjobs-feladat, a függvény hívása nem hello lapján is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="83553-314">As is true of all Console output, hello most recent 100 application logs also appear in hello Dashboard page for hello WebJob, not hello page for a function invocation.</span></span>

<span data-ttu-id="83553-315">Konzol eredmény jelenik meg, csak akkor, ha a hello program fut az Azure webjobs-feladat, nem hello program helyileg fut. Ha irányítópult hello vagy néhány más környezetben.</span><span class="sxs-lookup"><span data-stu-id="83553-315">Console output appears in hello Dashboard only if hello program is running in an Azure WebJob, not if hello program is running locally or in some other environment.</span></span>

<span data-ttu-id="83553-316">Tiltsa le a magas teljesítmény forgatókönyvek irányítópult naplózást.</span><span class="sxs-lookup"><span data-stu-id="83553-316">Disable dashboard logging for high throughput scenarios.</span></span> <span data-ttu-id="83553-317">Alapértelmezés szerint hello SDK írja a naplókat toostorage, és ez a tevékenység ronthatja a teljesítményt, hány üzenet feldolgozásakor.</span><span class="sxs-lookup"><span data-stu-id="83553-317">By default, hello SDK writes logs toostorage, and this activity could degrade performance when you are processing many messages.</span></span> <span data-ttu-id="83553-318">naplózás, toodisable hello irányítópult kapcsolati karakterlánc toonull állítsa be, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="83553-318">toodisable logging, set hello dashboard connection string toonull as shown in hello following example.</span></span>

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

<span data-ttu-id="83553-319">hello következő példa bemutatja, számos módon toowrite naplók:</span><span class="sxs-lookup"><span data-stu-id="83553-319">hello following example shows several ways toowrite logs:</span></span>

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

<span data-ttu-id="83553-320">A WebJobs SDK irányítópult hello, hello hello kimenete `TextWriter` jön létre, amikor egy adott toohello lapján továbblépne meghívása funkciót, és kattintson az objektum **váltása kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="83553-320">In hello WebJobs SDK Dashboard, hello output from hello `TextWriter` object shows up when you go toohello page for a particular function invocation and click **Toggle Output**:</span></span>

![Függvény meghívása hivatkozásra](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

<span data-ttu-id="83553-323">A WebJobs SDK irányítópult hello, hello legutóbbi 100 sorok konzol kimeneti megjelenítése mentése során, lépjen a webjobs-feladat hello (nem a hello függvény meghívása) toohello lapra, majd kattintson az **váltása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="83553-323">In hello WebJobs SDK Dashboard, hello most recent 100 lines of Console output show up when you go toohello page for hello WebJob (not for hello function invocation) and click **Toggle Output**.</span></span>

![Kattintson a Váltás kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

<span data-ttu-id="83553-325">Egy folyamatos webjobs-feladat, az alkalmazás naplóiban jelennek meg az/data/feladatok/folyamatos/*{webjobname}*/job_log.txt hello web app fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="83553-325">In a continuous WebJob, application logs show up in /data/jobs/continuous/*{webjobname}*/job_log.txt in hello web app file system.</span></span>

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

<span data-ttu-id="83553-326">Az Azure blob hello alkalmazásban naplók néznek ki: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, hiba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span><span class="sxs-lookup"><span data-stu-id="83553-326">In an Azure blob hello application logs look like this: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,</span></span>

<span data-ttu-id="83553-327">Egy Azure-tábla hello a `Console.Out` és `Console.Error` naplók néznek ki:</span><span class="sxs-lookup"><span data-stu-id="83553-327">And in an Azure table hello `Console.Out` and `Console.Error` logs look like this:</span></span>

![A tábla adatai napló](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Hibanapló tábla](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

<span data-ttu-id="83553-330">Ha azt szeretné, a saját naplózó tooplug, lásd: [ebben a példában](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="83553-330">If you want tooplug in your own logger, see [this example](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).</span></span>

## <span data-ttu-id="83553-331"><a id="errors"></a>Hogyan toohandle hibák és időtúllépések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83553-331"><a id="errors"></a>How toohandle errors and configure timeouts</span></span>
<span data-ttu-id="83553-332">hello WebJobs SDK is magában foglalja a [időtúllépés](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) , melyekkel egy függvény toobe megszűnik, ha toocause attribútum nem fejezi be a megadott időn belül.</span><span class="sxs-lookup"><span data-stu-id="83553-332">hello WebJobs SDK also includes a [Timeout](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) attribute that you can use toocause a function toobe canceled if doesn't complete within a specified amount of time.</span></span> <span data-ttu-id="83553-333">Ha azt szeretné tooraise riasztást, ha túl sok hiba fordulhat elő, a megadott időn belül, hello is használható `ErrorTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="83553-333">And if you want tooraise an alert when too many errors happen within a specified period of time, you can use hello `ErrorTrigger` attribute.</span></span> <span data-ttu-id="83553-334">Íme egy [ErrorTrigger példa](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span><span class="sxs-lookup"><span data-stu-id="83553-334">Here is an [ErrorTrigger example](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).</span></span>

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    too= "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors toohello Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

<span data-ttu-id="83553-335">Dinamikusan is letiltja, és a funkciók toocontrol engedélyezi, hogy akkor is elindítható, a konfiguráció kapcsoló, amelyet egy Alkalmazásbeállítás vagy a környezeti változó neve.</span><span class="sxs-lookup"><span data-stu-id="83553-335">You can also dynamically disable and enable functions toocontrol whether they can be triggered, by using a configuration switch that could be an app setting or environment variable name.</span></span> <span data-ttu-id="83553-336">Mintakód, lásd: hello `Disable` attribútumnak [hello WebJobs SDK-minták tárház](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span><span class="sxs-lookup"><span data-stu-id="83553-336">For sample code, see hello `Disable` attribute in [hello WebJobs SDK samples repository](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).</span></span>

## <span data-ttu-id="83553-337"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83553-337"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="83553-338">Ez az útmutató nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei az Azure-üzenetsorok használata.</span><span class="sxs-lookup"><span data-stu-id="83553-338">This guide has provided code samples that show how toohandle common scenarios for working with Azure queues.</span></span> <span data-ttu-id="83553-339">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="83553-339">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>
