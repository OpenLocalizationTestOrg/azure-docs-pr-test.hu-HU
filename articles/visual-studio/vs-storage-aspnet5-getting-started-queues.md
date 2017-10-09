---
title: "a queue storage és a Visual Studio (az ASP.NET Core) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget el az Azure üzenetsorának tárolást használó az ASP.NET Core projektre a Visual Studióban"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 90c1ebcb6a2eac6bc4f6b69133bdf3135d359a72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="70d25-103">Ismerkedés a queue storage és a Visual Studio a kapcsolódó szolgáltatások (ASP.NET mag)</span><span class="sxs-lookup"><span data-stu-id="70d25-103">Get started with queue storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="70d25-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="70d25-104">Overview</span></span>
<span data-ttu-id="70d25-105">Ez a cikk ismerteti, hogyan tooget el az Azure Queue storage a Visual Studióban létrehozott vagy egy Azure storage-fiók ASP.NET Core projektben hivatkozott hello Visual Studio használatával után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="70d25-105">This article describes how tooget started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="70d25-106">Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="70d25-106">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="70d25-107">Azure a queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához.</span><span class="sxs-lookup"><span data-stu-id="70d25-107">Azure queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="70d25-108">Egyetlen üzenetsor mentése too64 kilobájt (KB) méretű lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="70d25-108">A single queue message can be up too64 kilobytes (KB) in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

<span data-ttu-id="70d25-109">tooget elindult, először egy Azure-üzenetsorba toocreate tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="70d25-109">tooget started, you first need toocreate an Azure queue in your storage account.</span></span> <span data-ttu-id="70d25-110">Mutat be, hogyan toocreate kód üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="70d25-110">We'll show you how toocreate a queue in code.</span></span> <span data-ttu-id="70d25-111">Is mutat be, hogyan tooperform basic várólistára műveletek, például a hozzáadása, módosítása, olvasása, és eltávolítása az üzenetsor-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="70d25-111">We'll also show you how tooperform basic queue operations, such as adding, modifying, reading, and removing queue messages.</span></span> <span data-ttu-id="70d25-112">hello minták C nyelven íródtak\# code, és használja a hello Azure Storage ügyféloldali kódtára a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="70d25-112">hello samples are written in C\# code and use hello Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="70d25-113">ASP.NET kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="70d25-113">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

<span data-ttu-id="70d25-114">**Megjegyzés:** hello API-hívások tooAzure tárolási hajtsa végre az ASP.NET Core aszinkron.</span><span class="sxs-lookup"><span data-stu-id="70d25-114">**NOTE:** Some of hello APIs that perform calls tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="70d25-115">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="70d25-115">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="70d25-116">az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="70d25-116">hello code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="70d25-117">Lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt a sorok programozott módon kezelésére.</span><span class="sxs-lookup"><span data-stu-id="70d25-117">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on programmatically manipulating queues.</span></span>
* <span data-ttu-id="70d25-118">Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="70d25-118">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="70d25-119">Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="70d25-119">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="70d25-120">Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="70d25-120">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="70d25-121">A kód várólisták elérése</span><span class="sxs-lookup"><span data-stu-id="70d25-121">Access queues in code</span></span>
<span data-ttu-id="70d25-122">az ASP.NET Core projektek tooaccess várólistákból, következőkre lesz szüksége tooinclude hello elemek tooany C# forrásfájl hozzáférő Azure várólista-tároló.</span><span class="sxs-lookup"><span data-stu-id="70d25-122">tooaccess queues in ASP.NET Core projects, you need tooinclude hello following items tooany C# source file that accesses Azure queue storage.</span></span>

1. <span data-ttu-id="70d25-123">Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="70d25-123">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="70d25-124">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="70d25-124">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="70d25-125">A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="70d25-125">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="70d25-126">Első egy **CloudQueueClient** tooreference hello várólista-objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="70d25-126">Get a **CloudQueueClient** object tooreference hello queue objects in your storage account.</span></span>  
   
        // Create hello CloudQueueClient object for hello storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="70d25-127">Első egy **CloudQueue** objektum tooreference egy konkrét várólistába helyezi.</span><span class="sxs-lookup"><span data-stu-id="70d25-127">Get a **CloudQueue** object tooreference a specific queue.</span></span>
   
        // Get a reference toohello CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="70d25-128">**Megjegyzés:** a következő minták hello az összes fenti kódot hello kód előtt hello használja.</span><span class="sxs-lookup"><span data-stu-id="70d25-128">**NOTE:** Use all of hello above code in front of hello code in hello following samples.</span></span>

### <a name="create-a-queue-in-code"></a><span data-ttu-id="70d25-129">Várólista létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="70d25-129">Create a queue in code</span></span>
<span data-ttu-id="70d25-130">a kódban, az Azure üzenetsorának toocreate hello csak adjon hozzá egy túl**CreateIfNotExistsAsync**.</span><span class="sxs-lookup"><span data-stu-id="70d25-130">toocreate hello Azure queue in code, just add a call too**CreateIfNotExistsAsync**.</span></span>

    // Create hello CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="70d25-131">Egy üzenetsor tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="70d25-131">Add a message tooa queue</span></span>
<span data-ttu-id="70d25-132">egy létező üzenetsorba, az üzenet tooinsert hozzon létre egy új **CloudQueueMessage** objektumot, majd hívás hello **AddMessageAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="70d25-132">tooinsert a message into an existing queue, create a new **CloudQueueMessage** object, then call hello **AddMessageAsync** method.</span></span>

<span data-ttu-id="70d25-133">A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.</span><span class="sxs-lookup"><span data-stu-id="70d25-133">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="70d25-134">Íme egy példa, ami üdvözlőüzenetére beszúrja a "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="70d25-134">Here is an example which inserts hello message 'Hello, World'.</span></span>

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="70d25-135">Olvassa el egy üzenetet a várólistában egyszerre várakozó</span><span class="sxs-lookup"><span data-stu-id="70d25-135">Read a message in a queue</span></span>
<span data-ttu-id="70d25-136">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **PeekMessageAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="70d25-136">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessageAsync** method.</span></span>

    // Peek hello next message in hello queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="70d25-137">Olvassa el, és távolítsa el az üzenet a várólistában lévő</span><span class="sxs-lookup"><span data-stu-id="70d25-137">Read and remove a message in a queue</span></span>
<span data-ttu-id="70d25-138">A kód eltávolítása (created) egy üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="70d25-138">Your code can remove (dequeue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="70d25-139">Hívás **GetMessageAsync** tooget hello várólista következő üzenetére egy.</span><span class="sxs-lookup"><span data-stu-id="70d25-139">Call **GetMessageAsync** tooget hello next message in a queue.</span></span> <span data-ttu-id="70d25-140">Az üzenet **GetMessageAsync** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="70d25-140">A message returned from **GetMessageAsync** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="70d25-141">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="70d25-141">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="70d25-142">üdvözlőüzenetére eltávolítása hello várólista, hívás toofinish **DeleteMessageAsync**.</span><span class="sxs-lookup"><span data-stu-id="70d25-142">toofinish removing hello message from hello queue, call **DeleteMessageAsync**.</span></span>

<span data-ttu-id="70d25-143">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="70d25-143">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="70d25-144">hello következő kódot a hívások **DeleteMessageAsync** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="70d25-144">hello following code calls **DeleteMessageAsync** right after hello message has been processed.</span></span>

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process hello message in less than 30 seconds.

    // Then delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a><span data-ttu-id="70d25-145">Egyéb lehetőségek az üzenetek üzenetmozgatót</span><span class="sxs-lookup"><span data-stu-id="70d25-145">Leverage additional options for dequeuing messages</span></span>
<span data-ttu-id="70d25-146">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="70d25-146">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="70d25-147">Először is kaphat az üzenetkötegek (felfelé too32).</span><span class="sxs-lookup"><span data-stu-id="70d25-147">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="70d25-148">Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="70d25-148">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="70d25-149">hello alábbi példakód a **GetMessages** metódus tooget 20 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="70d25-149">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="70d25-150">Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével.</span><span class="sxs-lookup"><span data-stu-id="70d25-150">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="70d25-151">Beállítja a hello láthatatlansági időtúllépés too5 percig, amíg minden üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="70d25-151">It also sets hello invisibility timeout too5 minutes for each message.</span></span> <span data-ttu-id="70d25-152">Vegye figyelembe, hogy hello 5 perc start összes üzenetek: hello azonos idő, így miután 5 perc telt hello hívása után túl**GetMessages**, nem törölt üzenetek újra lesz látható.</span><span class="sxs-lookup"><span data-stu-id="70d25-152">Note that hello 5 minutes start for all messages at hello same time, so after 5 minutes have passed after hello call too**GetMessages**, any messages which have not been deleted become visible again.</span></span>

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-hello-queue-length"></a><span data-ttu-id="70d25-153">Első hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="70d25-153">Get hello queue length</span></span>
<span data-ttu-id="70d25-154">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="70d25-154">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="70d25-155">A **FetchAttributes** módszert kéri hello queue szolgáltatás lekérdezni hello várólista attribútumokat, beleértve a hello üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="70d25-155">The **FetchAttributes** method asks hello queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="70d25-156">Hello **ApproximateMethodCount** tulajdonság hello által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus hello queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="70d25-156">hello **ApproximateMethodCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello queue service.</span></span>

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display hello number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-queue-apis"></a><span data-ttu-id="70d25-157">Hello Async-Await mintázat használata közös várólista API-k</span><span class="sxs-lookup"><span data-stu-id="70d25-157">Use hello Async-Await pattern with common queue APIs</span></span>
<span data-ttu-id="70d25-158">Ez a példa bemutatja, hogyan toouse hello Async-Await mintázat a közös várólista API-k.</span><span class="sxs-lookup"><span data-stu-id="70d25-158">This example shows how toouse hello Async-Await pattern with common queue APIs.</span></span> <span data-ttu-id="70d25-159">hello minta hívások hello aszinkron verzióját módszerek megadott hello.</span><span class="sxs-lookup"><span data-stu-id="70d25-159">hello sample calls hello async version of each of hello given methods.</span></span> <span data-ttu-id="70d25-160">Ez hello aszinkron utáni javítás az egyes módszerek által látható.</span><span class="sxs-lookup"><span data-stu-id="70d25-160">This can be seen by hello Async post-fix of each method.</span></span> <span data-ttu-id="70d25-161">Egy aszinkron metódus használata esetén hello Async-Await mintázat felfüggeszti helyi hello hívás befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="70d25-161">When an async method is used, hello Async-Await pattern suspends local execution until hello call is completed.</span></span> <span data-ttu-id="70d25-162">Ez a viselkedés lehetővé teszi, hogy hello aktuális szál toodo más feladatok, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége hello.</span><span class="sxs-lookup"><span data-stu-id="70d25-162">This behavior allows hello current thread toodo other work which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="70d25-163">További információk az Async-Await mintázat a .NET hello, lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="70d25-163">For more details on using hello Async-Await pattern in .NET, see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message tooadd toohello queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue hello message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a><span data-ttu-id="70d25-164">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="70d25-164">Delete a queue</span></span>
<span data-ttu-id="70d25-165">toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **törlése** hello várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="70d25-165">toodelete a queue and all hello messages contained in it, call the **Delete** method on hello queue object.</span></span>

    // Delete hello queue.
    messageQueue.Delete();


## <a name="next-steps"></a><span data-ttu-id="70d25-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70d25-166">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

