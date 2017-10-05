---
title: "Ismerkedés a queue storage és a Visual Studio a kapcsolódó szolgáltatások (az ASP.NET Core) |} Microsoft Docs"
description: "Ismerkedés az Azure üzenetsor-kezelési tárolási az ASP.NET Core projekt, a Visual Studio használatával"
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
ms.openlocfilehash: 394344c0e126472b97c2e8f721c8c8d6514a17dc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="627c1-103">Ismerkedés a queue storage és a Visual Studio a kapcsolódó szolgáltatások (ASP.NET mag)</span><span class="sxs-lookup"><span data-stu-id="627c1-103">Get started with queue storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="627c1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="627c1-104">Overview</span></span>
<span data-ttu-id="627c1-105">Ez a cikk ismerteti az első lépések az Azure Queue storage segítségével a Visual Studio után készített vagy a Visual Studio használatával Azure-tárfiók az ASP.NET Core projekt hivatkozik **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="627c1-105">This article describes how to get started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="627c1-106">A **kapcsolódó szolgáltatások hozzáadása** műveletet a megfelelő NuGet-csomagok az Azure storage a projekt eléréséhez telepíti, és a tárfiók kapcsolati karakterlánca ad hozzá a projekt konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="627c1-106">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="627c1-107">Azure a queue storage egy olyan szolgáltatás, amely elérhető bárhol a világon HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül üzenetek nagy számban tárolásához.</span><span class="sxs-lookup"><span data-stu-id="627c1-107">Azure queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="627c1-108">Egyetlen üzenetsor akár 64 kilobájt (KB) méretű lehet, és több millió üzenetet a tárfiók maximális kapacitásán tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="627c1-108">A single queue message can be up to 64 kilobytes (KB) in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

<span data-ttu-id="627c1-109">A kezdéshez, először hozzon létre egy Azure-üzenetsorba tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="627c1-109">To get started, you first need to create an Azure queue in your storage account.</span></span> <span data-ttu-id="627c1-110">Mutat be, hogyan várólista létrehozása a kódban.</span><span class="sxs-lookup"><span data-stu-id="627c1-110">We'll show you how to create a queue in code.</span></span> <span data-ttu-id="627c1-111">Is mutat be, hogyan hajthat végre alapszintű várólista műveletek, például a hozzáadása, módosítása, olvasása, és eltávolítása a üzenetsor-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="627c1-111">We'll also show you how to perform basic queue operations, such as adding, modifying, reading, and removing queue messages.</span></span> <span data-ttu-id="627c1-112">A Kódminták C nyelven íródtak\# code, és használja az Azure Storage ügyféloldali kódtára a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="627c1-112">The samples are written in C\# code and use the Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="627c1-113">ASP.NET kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="627c1-113">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

<span data-ttu-id="627c1-114">**Megjegyzés:** az ASP.NET Core hajtsa végre az Azure storage hívásainak API-k aszinkron.</span><span class="sxs-lookup"><span data-stu-id="627c1-114">**NOTE:** Some of the APIs that perform calls to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="627c1-115">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="627c1-115">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="627c1-116">Az alábbi kódot azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="627c1-116">The code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="627c1-117">Lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt a sorok programozott módon kezelésére.</span><span class="sxs-lookup"><span data-stu-id="627c1-117">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on programmatically manipulating queues.</span></span>
* <span data-ttu-id="627c1-118">Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="627c1-118">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="627c1-119">Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="627c1-119">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="627c1-120">Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="627c1-120">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="627c1-121">A kód várólisták elérése</span><span class="sxs-lookup"><span data-stu-id="627c1-121">Access queues in code</span></span>
<span data-ttu-id="627c1-122">Fér hozzá az ASP.NET Core projektek várólistákat, is minden C# forrásfájlhoz, aki hozzáfér az Azure a queue storage a következő elemeket kell.</span><span class="sxs-lookup"><span data-stu-id="627c1-122">To access queues in ASP.NET Core projects, you need to include the following items to any C# source file that accesses Azure queue storage.</span></span>

1. <span data-ttu-id="627c1-123">Győződjön meg arról, hogy a névtér-deklarációk, a C# fájlban tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="627c1-123">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="627c1-124">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="627c1-124">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="627c1-125">A következő kód segítségével beolvasása a a tárolási kapcsolati karakterlánc és a tárfiók adatait az Azure-szolgáltatások konfigurációjából.</span><span class="sxs-lookup"><span data-stu-id="627c1-125">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="627c1-126">Első egy **CloudQueueClient** való hivatkozáshoz a várólista-objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="627c1-126">Get a **CloudQueueClient** object to reference the queue objects in your storage account.</span></span>  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="627c1-127">Első egy **CloudQueue** objektum való hivatkozáshoz egy konkrét várólistába helyezi.</span><span class="sxs-lookup"><span data-stu-id="627c1-127">Get a **CloudQueue** object to reference a specific queue.</span></span>
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="627c1-128">**Megjegyzés:** összes a fenti kódot a kód előtt használja a következő mintában.</span><span class="sxs-lookup"><span data-stu-id="627c1-128">**NOTE:** Use all of the above code in front of the code in the following samples.</span></span>

### <a name="create-a-queue-in-code"></a><span data-ttu-id="627c1-129">Várólista létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="627c1-129">Create a queue in code</span></span>
<span data-ttu-id="627c1-130">Az Azure üzenetsor-kezelési létrehozása a kódban, csak adjon hozzá egy **CreateIfNotExistsAsync**.</span><span class="sxs-lookup"><span data-stu-id="627c1-130">To create the Azure queue in code, just add a call to **CreateIfNotExistsAsync**.</span></span>

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="627c1-131">A várólista üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="627c1-131">Add a message to a queue</span></span>
<span data-ttu-id="627c1-132">Üzenet beszúrása egy létező üzenetsorba, hozzon létre egy új **CloudQueueMessage** objektumot, majd hívja a **AddMessageAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="627c1-132">To insert a message into an existing queue, create a new **CloudQueueMessage** object, then call the **AddMessageAsync** method.</span></span>

<span data-ttu-id="627c1-133">A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.</span><span class="sxs-lookup"><span data-stu-id="627c1-133">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="627c1-134">Íme egy példa, amely beszúrja a "Hello, World" üzenet.</span><span class="sxs-lookup"><span data-stu-id="627c1-134">Here is an example which inserts the message 'Hello, World'.</span></span>

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="627c1-135">Olvassa el egy üzenetet a várólistában egyszerre várakozó</span><span class="sxs-lookup"><span data-stu-id="627c1-135">Read a message in a queue</span></span>
<span data-ttu-id="627c1-136">Is bepillanthat, hogy egy sor elején található üzenetbe anélkül, hogy eltávolítaná az üzenetsorból meghívásával a **PeekMessageAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="627c1-136">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessageAsync** method.</span></span>

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="627c1-137">Olvassa el, és távolítsa el az üzenet a várólistában lévő</span><span class="sxs-lookup"><span data-stu-id="627c1-137">Read and remove a message in a queue</span></span>
<span data-ttu-id="627c1-138">A kód eltávolítása (created) egy üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="627c1-138">Your code can remove (dequeue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="627c1-139">Hívás **GetMessageAsync** számára a következő üzenet jelenik meg a sorhoz.</span><span class="sxs-lookup"><span data-stu-id="627c1-139">Call **GetMessageAsync** to get the next message in a queue.</span></span> <span data-ttu-id="627c1-140">Az üzenet **GetMessageAsync** ebből a várólistából üzeneteket olvasó többi kód láthatatlanná válik.</span><span class="sxs-lookup"><span data-stu-id="627c1-140">A message returned from **GetMessageAsync** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="627c1-141">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="627c1-141">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="627c1-142">Szeretné távolítani az üzenetet az üzenetsorból, hívja **DeleteMessageAsync**.</span><span class="sxs-lookup"><span data-stu-id="627c1-142">To finish removing the message from the queue, call **DeleteMessageAsync**.</span></span>

<span data-ttu-id="627c1-143">Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="627c1-143">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="627c1-144">A következő kódot a hívások **DeleteMessageAsync** jobb gombbal az üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="627c1-144">The following code calls **DeleteMessageAsync** right after the message has been processed.</span></span>

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a><span data-ttu-id="627c1-145">Egyéb lehetőségek az üzenetek üzenetmozgatót</span><span class="sxs-lookup"><span data-stu-id="627c1-145">Leverage additional options for dequeuing messages</span></span>
<span data-ttu-id="627c1-146">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="627c1-146">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="627c1-147">Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése.</span><span class="sxs-lookup"><span data-stu-id="627c1-147">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="627c1-148">A második lehetőség az, hogy beállít egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak lehetősége van hosszabb vagy rövidebb idő alatt teljesen feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="627c1-148">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="627c1-149">Az alábbi példakód a **GetMessages** módszer segítségével egyszerre 20 üzenetet kér le.</span><span class="sxs-lookup"><span data-stu-id="627c1-149">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="627c1-150">Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével.</span><span class="sxs-lookup"><span data-stu-id="627c1-150">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="627c1-151">Azt is állítja be a láthatatlansági időkorlátot 5 perc minden egyes üzenet esetében.</span><span class="sxs-lookup"><span data-stu-id="627c1-151">It also sets the invisibility timeout to 5 minutes for each message.</span></span> <span data-ttu-id="627c1-152">Vegye figyelembe, hogy minden üzenet egyszerre, indítsa el a 5 perc, miután 5 perccel átadott hívása után **GetMessages**, nem törölt üzenetek újra lesz látható.</span><span class="sxs-lookup"><span data-stu-id="627c1-152">Note that the 5 minutes start for all messages at the same time, so after 5 minutes have passed after the call to **GetMessages**, any messages which have not been deleted become visible again.</span></span>

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a><span data-ttu-id="627c1-153">Az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="627c1-153">Get the queue length</span></span>
<span data-ttu-id="627c1-154">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="627c1-154">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="627c1-155">A **FetchAttributes** módszert kéri a queue szolgáltatás olvashatók be a várólista attribútumai, az üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="627c1-155">The **FetchAttributes** method asks the queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="627c1-156">A **ApproximateMethodCount** tulajdonság által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus, a queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="627c1-156">The **ApproximateMethodCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the queue service.</span></span>

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a><span data-ttu-id="627c1-157">Az Async-Await mintázat használata közös várólista API-k</span><span class="sxs-lookup"><span data-stu-id="627c1-157">Use the Async-Await pattern with common queue APIs</span></span>
<span data-ttu-id="627c1-158">Ez a példa bemutatja, hogyan használható az Async-Await mintázat a közös várólista API-k.</span><span class="sxs-lookup"><span data-stu-id="627c1-158">This example shows how to use the Async-Await pattern with common queue APIs.</span></span> <span data-ttu-id="627c1-159">A minta meghívja az adott módszerek aszinkron verzióját.</span><span class="sxs-lookup"><span data-stu-id="627c1-159">The sample calls the async version of each of the given methods.</span></span> <span data-ttu-id="627c1-160">Ez az aszinkron utáni javítás az egyes módszerek által látható.</span><span class="sxs-lookup"><span data-stu-id="627c1-160">This can be seen by the Async post-fix of each method.</span></span> <span data-ttu-id="627c1-161">Egy aszinkron metódus használata esetén az Async-Await mintázat felfüggeszti helyi a hívás befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="627c1-161">When an async method is used, the Async-Await pattern suspends local execution until the call is completed.</span></span> <span data-ttu-id="627c1-162">Ez a viselkedés lehetővé teszi, hogy az aktuális szál más a munkáját, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége.</span><span class="sxs-lookup"><span data-stu-id="627c1-162">This behavior allows the current thread to do other work which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="627c1-163">Az Async-Await mintázat használatáról .NET a további részletekért lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="627c1-163">For more details on using the Async-Await pattern in .NET, see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a><span data-ttu-id="627c1-164">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="627c1-164">Delete a queue</span></span>
<span data-ttu-id="627c1-165">Egy üzenetsor és az összes benne foglalt üzenet törléséhez hívja meg a **Delete** módszert az üzenetsor-objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="627c1-165">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a><span data-ttu-id="627c1-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="627c1-166">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

