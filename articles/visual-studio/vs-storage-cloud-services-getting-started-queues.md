---
title: "Ismerkedjen meg a queue storage és a Visual Studio services (felhőszolgáltatások) kapcsolódó |} Microsoft Docs"
description: "Ismerkedés az Azure Queue storage használata a Visual Studio felhőszolgáltatás-projekt egy tárfiókot, a Visual Studio használatával történő kapcsolódás után kapcsolódó szolgáltatások"
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
ms.openlocfilehash: 7a6e58a62b4cfbf99641559363dd0c860cdf8af2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="989d4-103">Ismerkedés az Azure Queue storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)</span><span class="sxs-lookup"><span data-stu-id="989d4-103">Getting started with Azure Queue storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="989d4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="989d4-104">Overview</span></span>
<span data-ttu-id="989d4-105">Ez a cikk ismerteti az első lépések az Azure Queue storage segítségével a Visual Studio után készített vagy a Visual Studio használatával Azure-tárfiók felhőalapú szolgáltatások projektben hivatkozott **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="989d4-105">This article describes how to get started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using the  Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="989d4-106">Mutat be, hogyan várólista létrehozása a kódban.</span><span class="sxs-lookup"><span data-stu-id="989d4-106">We'll show you how to create a queue in code.</span></span> <span data-ttu-id="989d4-107">Is mutat be, hogyan hajthat végre alapszintű várólista műveletek, például a hozzáadása, módosítása, olvasási és eltávolítása az üzenetsor-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="989d4-107">We'll also show you how to perform basic queue operations, such as adding, modifying, reading and removing queue messages.</span></span> <span data-ttu-id="989d4-108">A mintákat a C#-kódban és -felhasználási nyelven íródtak a [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="989d4-108">The samples are written in C# code and use the [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="989d4-109">A **kapcsolódó szolgáltatások hozzáadása** műveletet a megfelelő NuGet-csomagok az Azure storage a projekt eléréséhez telepíti, és a tárfiók kapcsolati karakterlánca ad hozzá a projekt konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="989d4-109">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

* <span data-ttu-id="989d4-110">Lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt a kódban várólisták kezelésére.</span><span class="sxs-lookup"><span data-stu-id="989d4-110">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on manipulating queues in code.</span></span>
* <span data-ttu-id="989d4-111">Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="989d4-111">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="989d4-112">Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="989d4-112">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="989d4-113">Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="989d4-113">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

<span data-ttu-id="989d4-114">Az Azure Queue Storage szolgáltatás üzenetek nagy számban történő tárolására szolgál, amelyek HTTP- vagy HTTPS-kapcsolattal, hitelesített hívásokon keresztül a világon bárhonnan elérhetők.</span><span class="sxs-lookup"><span data-stu-id="989d4-114">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="989d4-115">Egyetlen üzenetsor akár 64 KB méretű is lehet, és a tárfiók maximális kapacitásán belül több millió üzenetet tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="989d4-115">A single queue message can be up to 64 KB in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="989d4-116">A kód várólisták elérése</span><span class="sxs-lookup"><span data-stu-id="989d4-116">Access queues in code</span></span>
<span data-ttu-id="989d4-117">A Visual Studio Felhőszolgáltatás-projektek várólisták fér hozzá, a következő elemek bármely C# forrásfájl elérhető Azure Queue storage is kell.</span><span class="sxs-lookup"><span data-stu-id="989d4-117">To access queues in Visual Studio Cloud Services projects, you need to include the following items to any C# source file that access Azure Queue storage.</span></span>

1. <span data-ttu-id="989d4-118">Győződjön meg arról, hogy a névtér-deklarációk, a C# fájlban tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="989d4-118">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. <span data-ttu-id="989d4-119">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="989d4-119">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="989d4-120">A következő kód segítségével beolvasása a a tárolási kapcsolati karakterlánc és a tárfiók adatait az Azure-szolgáltatások konfigurációjából.</span><span class="sxs-lookup"><span data-stu-id="989d4-120">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="989d4-121">Első egy **CloudQueueClient** való hivatkozáshoz a várólista-objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="989d4-121">Get a **CloudQueueClient** object to reference the queue objects in your storage account.</span></span>  
   
        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="989d4-122">Első egy **CloudQueue** objektum való hivatkozáshoz egy konkrét várólistába helyezi.</span><span class="sxs-lookup"><span data-stu-id="989d4-122">Get a **CloudQueue** object to reference a specific queue.</span></span>
   
        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="989d4-123">**Megjegyzés:** összes a fenti kódot a kód előtt használja a következő mintában.</span><span class="sxs-lookup"><span data-stu-id="989d4-123">**NOTE:** Use all of the above code in front of the code in the following samples.</span></span>

## <a name="create-a-queue-in-code"></a><span data-ttu-id="989d4-124">Várólista létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="989d4-124">Create a queue in code</span></span>
<span data-ttu-id="989d4-125">A várólista létrehozása a kódban, csak adjon hozzá egy **CreateIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="989d4-125">To create the queue in code, just add a call to **CreateIfNotExists**.</span></span>

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="989d4-126">A várólista üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="989d4-126">Add a message to a queue</span></span>
<span data-ttu-id="989d4-127">Üzenet beszúrása egy létező üzenetsorba, hozzon létre egy új **CloudQueueMessage** objektumot, majd hívja a **AddMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="989d4-127">To insert a message into an existing queue, create a new **CloudQueueMessage** object, then call the **AddMessage** method.</span></span>

<span data-ttu-id="989d4-128">A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.</span><span class="sxs-lookup"><span data-stu-id="989d4-128">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="989d4-129">Íme egy példa, amely beszúrja a "Hello, World" üzenet.</span><span class="sxs-lookup"><span data-stu-id="989d4-129">Here is an example which inserts the message 'Hello, World'.</span></span>

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="989d4-130">Olvassa el egy üzenetet a várólistában egyszerre várakozó</span><span class="sxs-lookup"><span data-stu-id="989d4-130">Read a message in a queue</span></span>
<span data-ttu-id="989d4-131">Egy üzenetsor elején található üzenetbe anélkül is bepillanthat, hogy eltávolítaná az üzenetsorból. Ehhez hívja meg a **PeekMessage** módszert.</span><span class="sxs-lookup"><span data-stu-id="989d4-131">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessage** method.</span></span>

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="989d4-132">Olvassa el, és távolítsa el az üzenet a várólistában lévő</span><span class="sxs-lookup"><span data-stu-id="989d4-132">Read and remove a message in a queue</span></span>
<span data-ttu-id="989d4-133">A kód eltávolítása (kivétele az üzenetsorból) egy üzenetet az üzenetsorból két lépésben.</span><span class="sxs-lookup"><span data-stu-id="989d4-133">Your code can remove (de-queue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="989d4-134">Hívás **GetMessage** számára a következő üzenet jelenik meg a sorhoz.</span><span class="sxs-lookup"><span data-stu-id="989d4-134">Call **GetMessage** to get the next message in a queue.</span></span> <span data-ttu-id="989d4-135">A **GetMessage** módszerrel lekért üzenet láthatatlanná válik az adott üzenetsorban található üzeneteket olvasó többi kód számára.</span><span class="sxs-lookup"><span data-stu-id="989d4-135">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="989d4-136">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="989d4-136">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="989d4-137">Szeretné távolítani az üzenetet az üzenetsorból, hívja **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="989d4-137">To finish removing the message from the queue, call **DeleteMessage**.</span></span>

<span data-ttu-id="989d4-138">Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="989d4-138">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="989d4-139">A következő kódot a hívások **DeleteMessage** jobb gombbal az üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="989d4-139">The following code calls **DeleteMessage** right after the message has been processed.</span></span>

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a><span data-ttu-id="989d4-140">További beállítások segítségével feldolgozni, és távolítsa el a várólista-üzenetek</span><span class="sxs-lookup"><span data-stu-id="989d4-140">Use additional options to process and remove queue messages</span></span>
<span data-ttu-id="989d4-141">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="989d4-141">There are two ways you can customize message retrieval from a queue.</span></span>

* <span data-ttu-id="989d4-142">Az üzenetkötegek (legfeljebb 32) kérheti le.</span><span class="sxs-lookup"><span data-stu-id="989d4-142">You can get a batch of messages (up to 32).</span></span>
* <span data-ttu-id="989d4-143">Beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak hosszabb vagy rövidebb idő teljesen feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="989d4-143">You can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="989d4-144">Az alábbi példakód a **GetMessages** módszer segítségével egyszerre 20 üzenetet kér le.</span><span class="sxs-lookup"><span data-stu-id="989d4-144">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="989d4-145">Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével.</span><span class="sxs-lookup"><span data-stu-id="989d4-145">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="989d4-146">Mindemellett a láthatatlansági időkorlátot minden üzenethez öt percre állítja be.</span><span class="sxs-lookup"><span data-stu-id="989d4-146">It also sets the invisibility timeout to five minutes for each message.</span></span> <span data-ttu-id="989d4-147">Vegye figyelembe, hogy az 5 perc minden üzenetnél ugyanakkor kezdődik, tehát 5 perccel a **GetMessages** hívása után a nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="989d4-147">Note that the 5 minutes starts for all messages at the same time, so after 5 minutes have passed since the call to **GetMessages**, any messages which have not been deleted will become visible again.</span></span>

<span data-ttu-id="989d4-148">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="989d4-148">Here's an example:</span></span>

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a><span data-ttu-id="989d4-149">Az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="989d4-149">Get the queue length</span></span>
<span data-ttu-id="989d4-150">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="989d4-150">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="989d4-151">A **FetchAttributes** módszer lekéri a Queue szolgáltatásból az üzenetsorra vonatkozó attribútumokat, amelyek között megtalálható az üzenetek száma is.</span><span class="sxs-lookup"><span data-stu-id="989d4-151">The **FetchAttributes** method asks the Queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="989d4-152">A **ApproximateMethodCount** tulajdonság által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus, a Queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="989d4-152">The **ApproximateMethodCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the Queue service.</span></span>

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a><span data-ttu-id="989d4-153">Az Async-Await mintázat használata közös Azure várólista API-k</span><span class="sxs-lookup"><span data-stu-id="989d4-153">Use the Async-Await Pattern with common Azure Queue APIs</span></span>
<span data-ttu-id="989d4-154">Ez a példa bemutatja, hogyan használható az Async-Await mintázat a közös Azure várólista API-k.</span><span class="sxs-lookup"><span data-stu-id="989d4-154">This example shows how to use the Async-Await pattern with common Azure Queue APIs.</span></span> <span data-ttu-id="989d4-155">A minta meghívja az adott módszerek aszinkron verzióját, ez látható által a **aszinkron** az egyes módszerek utáni javítás.</span><span class="sxs-lookup"><span data-stu-id="989d4-155">The sample calls the async version of each of the given methods, this can be seen by the **Async** post-fix of each method.</span></span> <span data-ttu-id="989d4-156">Egy aszinkron metódus használatakor az async-await mintázat felfüggeszti a helyi végrehajtást a hívás befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="989d4-156">When an async method is used the async-await pattern suspends local execution until the call completes.</span></span> <span data-ttu-id="989d4-157">Ez a viselkedés lehetővé teszi, hogy az aktuális szál más a munkáját, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége.</span><span class="sxs-lookup"><span data-stu-id="989d4-157">This behavior allows the current thread to do other work which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="989d4-158">További információk az Async-Await mintázat használatáról .NET-keretrendszerben: [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx) (Async és Await (C# és Visual Basic)).</span><span class="sxs-lookup"><span data-stu-id="989d4-158">For more details on using the Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a><span data-ttu-id="989d4-159">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="989d4-159">Delete a queue</span></span>
<span data-ttu-id="989d4-160">Egy üzenetsor és az összes benne foglalt üzenet törléséhez hívja meg a **Delete** módszert az üzenetsor-objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="989d4-160">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a><span data-ttu-id="989d4-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="989d4-161">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

