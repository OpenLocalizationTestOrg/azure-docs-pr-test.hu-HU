---
title: "a queue storage és a Visual Studio (felhőszolgáltatások) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget el Azure Queue storage segítségével egy Visual Studio felhőszolgáltatás-projektet a Visual Studio használatával tooa tárfiók kapcsolódás szolgáltatások csatlakoztatása után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 1e90eeb826131cadca90dcb720c931fff5fedcb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="02c89-103">Ismerkedés az Azure Queue storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)</span><span class="sxs-lookup"><span data-stu-id="02c89-103">Getting started with Azure Queue storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="02c89-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="02c89-104">Overview</span></span>
<span data-ttu-id="02c89-105">Ez a cikk ismerteti, hogyan tooget el az Azure Queue storage a Visual Studióban létrehozott vagy egy Azure storage-fiók felhőalapú szolgáltatások projektben hivatkozott hello Visual Studio használatával után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel .</span><span class="sxs-lookup"><span data-stu-id="02c89-105">This article describes how tooget started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using hello  Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="02c89-106">Mutat be, hogyan toocreate kód üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="02c89-106">We'll show you how toocreate a queue in code.</span></span> <span data-ttu-id="02c89-107">Is mutat be, hogyan tooperform basic várólistára műveletek, például a hozzáadása, módosítása, olvasási és eltávolításakor üzenetsor-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="02c89-107">We'll also show you how tooperform basic queue operations, such as adding, modifying, reading and removing queue messages.</span></span> <span data-ttu-id="02c89-108">hello Kódminták C# kódban vannak megírva, és használják hello [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="02c89-108">hello samples are written in C# code and use hello [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="02c89-109">Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="02c89-109">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

* <span data-ttu-id="02c89-110">Lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt a kódban várólisták kezelésére.</span><span class="sxs-lookup"><span data-stu-id="02c89-110">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on manipulating queues in code.</span></span>
* <span data-ttu-id="02c89-111">Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="02c89-111">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="02c89-112">Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="02c89-112">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="02c89-113">Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="02c89-113">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

<span data-ttu-id="02c89-114">Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához.</span><span class="sxs-lookup"><span data-stu-id="02c89-114">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="02c89-115">Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="02c89-115">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="02c89-116">A kód várólisták elérése</span><span class="sxs-lookup"><span data-stu-id="02c89-116">Access queues in code</span></span>
<span data-ttu-id="02c89-117">a Visual Studio Felhőszolgáltatás-projektek tooaccess várólisták, következőkre lesz szüksége tooinclude hello elemek tooany C# forrásfájl elérhető Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="02c89-117">tooaccess queues in Visual Studio Cloud Services projects, you need tooinclude hello following items tooany C# source file that access Azure Queue storage.</span></span>

1. <span data-ttu-id="02c89-118">Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="02c89-118">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. <span data-ttu-id="02c89-119">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="02c89-119">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="02c89-120">A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="02c89-120">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="02c89-121">Első egy **CloudQueueClient** tooreference hello várólista-objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="02c89-121">Get a **CloudQueueClient** object tooreference hello queue objects in your storage account.</span></span>  
   
        // Create hello queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="02c89-122">Első egy **CloudQueue** objektum tooreference egy konkrét várólistába helyezi.</span><span class="sxs-lookup"><span data-stu-id="02c89-122">Get a **CloudQueue** object tooreference a specific queue.</span></span>
   
        // Get a reference tooa queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="02c89-123">**Megjegyzés:** a következő minták hello az összes fenti kódot hello kód előtt hello használja.</span><span class="sxs-lookup"><span data-stu-id="02c89-123">**NOTE:** Use all of hello above code in front of hello code in hello following samples.</span></span>

## <a name="create-a-queue-in-code"></a><span data-ttu-id="02c89-124">Várólista létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="02c89-124">Create a queue in code</span></span>
<span data-ttu-id="02c89-125">toocreate hello várólista a kódban, csak adjon hozzá egy túl**CreateIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="02c89-125">toocreate hello queue in code, just add a call too**CreateIfNotExists**.</span></span>

    // Create hello CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="02c89-126">Egy üzenetsor tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="02c89-126">Add a message tooa queue</span></span>
<span data-ttu-id="02c89-127">egy létező üzenetsorba, az üzenet tooinsert hozzon létre egy új **CloudQueueMessage** objektumot, majd hívás hello **AddMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="02c89-127">tooinsert a message into an existing queue, create a new **CloudQueueMessage** object, then call hello **AddMessage** method.</span></span>

<span data-ttu-id="02c89-128">A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.</span><span class="sxs-lookup"><span data-stu-id="02c89-128">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="02c89-129">Íme egy példa, ami üdvözlőüzenetére beszúrja a "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="02c89-129">Here is an example which inserts hello message 'Hello, World'.</span></span>

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="02c89-130">Olvassa el egy üzenetet a várólistában egyszerre várakozó</span><span class="sxs-lookup"><span data-stu-id="02c89-130">Read a message in a queue</span></span>
<span data-ttu-id="02c89-131">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **PeekMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="02c89-131">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessage** method.</span></span>

    // Peek at hello next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="02c89-132">Olvassa el, és távolítsa el az üzenet a várólistában lévő</span><span class="sxs-lookup"><span data-stu-id="02c89-132">Read and remove a message in a queue</span></span>
<span data-ttu-id="02c89-133">A kód eltávolítása (kivétele az üzenetsorból) egy üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="02c89-133">Your code can remove (de-queue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="02c89-134">Hívás **GetMessage** tooget hello várólista következő üzenetére egy.</span><span class="sxs-lookup"><span data-stu-id="02c89-134">Call **GetMessage** tooget hello next message in a queue.</span></span> <span data-ttu-id="02c89-135">Az üzenet **GetMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="02c89-135">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="02c89-136">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="02c89-136">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="02c89-137">üdvözlőüzenetére eltávolítása hello várólista, hívás toofinish **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="02c89-137">toofinish removing hello message from hello queue, call **DeleteMessage**.</span></span>

<span data-ttu-id="02c89-138">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="02c89-138">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="02c89-139">hello következő kódot a hívások **DeleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="02c89-139">hello following code calls **DeleteMessage** right after hello message has been processed.</span></span>

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process hello message in less than 30 seconds

    // Then delete hello message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-tooprocess-and-remove-queue-messages"></a><span data-ttu-id="02c89-140">További beállítások tooprocess használja, és távolítsa el a várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="02c89-140">Use additional options tooprocess and remove queue messages</span></span>
<span data-ttu-id="02c89-141">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="02c89-141">There are two ways you can customize message retrieval from a queue.</span></span>

* <span data-ttu-id="02c89-142">Az üzenetkötegek (felfelé too32) kérheti le.</span><span class="sxs-lookup"><span data-stu-id="02c89-142">You can get a batch of messages (up too32).</span></span>
* <span data-ttu-id="02c89-143">Megadhat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="02c89-143">You can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="02c89-144">hello alábbi példakód a **GetMessages** metódus tooget 20 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="02c89-144">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="02c89-145">Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével.</span><span class="sxs-lookup"><span data-stu-id="02c89-145">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="02c89-146">Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="02c89-146">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="02c89-147">Vegye figyelembe, hogy hello 5 perc indítása: hello összes üzenet azonos idő, így miután 5 perc telt túl hello hívás**GetMessages**, nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="02c89-147">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**GetMessages**, any messages which have not been deleted will become visible again.</span></span>

<span data-ttu-id="02c89-148">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="02c89-148">Here's an example:</span></span>

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete hello message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-hello-queue-length"></a><span data-ttu-id="02c89-149">Első hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="02c89-149">Get hello queue length</span></span>
<span data-ttu-id="02c89-150">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="02c89-150">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="02c89-151">A **FetchAttributes** módszert kéri hello Queue szolgáltatás lekérdezni hello várólista attribútumokat, beleértve a hello üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="02c89-151">The **FetchAttributes** method asks hello Queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="02c89-152">Hello **ApproximateMethodCount** tulajdonság hello által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus hello Queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="02c89-152">hello **ApproximateMethodCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello Queue service.</span></span>

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-azure-queue-apis"></a><span data-ttu-id="02c89-153">Hello Async-Await mintázat használata közös Azure várólista API-k</span><span class="sxs-lookup"><span data-stu-id="02c89-153">Use hello Async-Await Pattern with common Azure Queue APIs</span></span>
<span data-ttu-id="02c89-154">Ez a példa bemutatja, hogyan toouse hello Async-Await mintázat a közös Azure várólista API-k.</span><span class="sxs-lookup"><span data-stu-id="02c89-154">This example shows how toouse hello Async-Await pattern with common Azure Queue APIs.</span></span> <span data-ttu-id="02c89-155">hello minta meghívja az egyes módszerek megadott hello hello aszinkron verziója, ez hello látható **aszinkron** az egyes módszerek utáni javítás.</span><span class="sxs-lookup"><span data-stu-id="02c89-155">hello sample calls hello async version of each of hello given methods, this can be seen by hello **Async** post-fix of each method.</span></span> <span data-ttu-id="02c89-156">Egy aszinkron metódus esetén használt hello async-await mintázat felfüggeszti a helyi végrehajtást hello hívás befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="02c89-156">When an async method is used hello async-await pattern suspends local execution until hello call completes.</span></span> <span data-ttu-id="02c89-157">Ez a viselkedés lehetővé teszi, hogy hello aktuális szál toodo más feladatok, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége hello.</span><span class="sxs-lookup"><span data-stu-id="02c89-157">This behavior allows hello current thread toodo other work which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="02c89-158">További információk az Async-Await mintázat a .NET hello lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="02c89-158">For more details on using hello Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message tooput in hello queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add hello message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete hello message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a><span data-ttu-id="02c89-159">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="02c89-159">Delete a queue</span></span>
<span data-ttu-id="02c89-160">egy üzenetsor és az összes köszönőüzenetei benne tárolt, hívás hello toodelete **törlése** hello várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="02c89-160">toodelete a queue and all hello messages contained in it, call hello **Delete** method on hello queue object.</span></span>

    // Delete hello queue.
    messageQueue.Delete();

## <a name="next-steps"></a><span data-ttu-id="02c89-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02c89-161">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

