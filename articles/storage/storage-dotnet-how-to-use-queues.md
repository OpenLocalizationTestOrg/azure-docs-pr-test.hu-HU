---
title: "Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel | Microsoft Docs"
description: "Az Azure-üzenetsorok megbízható, aszinkron üzenetkezelést biztosítanak az alkalmazások összetevői között. A felhőbeli üzenetkezelésnek köszönhetően az alkalmazások összetevői függetlenül méretezhetők."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: c0f82537-a613-4f01-b2ed-fc82e5eea2a7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: robinsh
ms.openlocfilehash: 7f88aa9c50e669d1be7248346c7b1176bce61249
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a><span data-ttu-id="b5ca0-104">Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="b5ca0-104">Get started with Azure Queue storage using .NET</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a><span data-ttu-id="b5ca0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b5ca0-105">Overview</span></span>
<span data-ttu-id="b5ca0-106">Az Azure Queue Storage felhőbeli üzenetkezelést biztosít az alkalmazások összetevői között.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-106">Azure Queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="b5ca0-107">A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-107">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="b5ca0-108">A Queue Storage aszinkron üzenetkezelést biztosít az alkalmazások összetevői közötti kommunikációhoz, függetlenül attól, hogy az összetevők a felhőben, asztali gépen, egy helyszíni kiszolgálón vagy egy mobileszközön futnak.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-108">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in the cloud, on the desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="b5ca0-109">A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-109">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="b5ca0-110">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="b5ca0-110">About this tutorial</span></span>
<span data-ttu-id="b5ca0-111">Ez az oktatóanyag bemutatja, hogyan írhat .NET-kódot néhány, az Azure Queue Storage szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-111">This tutorial shows how to write .NET code for some common scenarios using Azure Queue storage.</span></span> <span data-ttu-id="b5ca0-112">Az ismertetett forgatókönyvek az üzenetsorok létrehozására és törlésére, valamint az üzenetsor üzeneteinek hozzáadására, olvasására és törlésére vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-112">Scenarios covered include creating and deleting queues and adding, reading, and deleting queue messages.</span></span>

<span data-ttu-id="b5ca0-113">**Az oktatóanyag áttekintésének becsült ideje:** 45 perc</span><span class="sxs-lookup"><span data-stu-id="b5ca0-113">**Estimated time to complete:** 45 minutes</span></span>

<span data-ttu-id="b5ca0-114">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="b5ca0-114">**Prerequisities:**</span></span>

* [<span data-ttu-id="b5ca0-115">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5ca0-115">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="b5ca0-116">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="b5ca0-116">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="b5ca0-117">Azure Configuration Manager a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="b5ca0-117">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="b5ca0-118">Egy [Azure-tárfiók](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="b5ca0-118">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="b5ca0-119">Hozzáadás irányelvekkel</span><span class="sxs-lookup"><span data-stu-id="b5ca0-119">Add using directives</span></span>
<span data-ttu-id="b5ca0-120">Adja hozzá a következő `using` irányelveket a `Program.cs` fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="b5ca0-120">Add the following `using` directives to the top of the `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-the-connection-string"></a><span data-ttu-id="b5ca0-121">Kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="b5ca0-121">Parse the connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a><span data-ttu-id="b5ca0-122">A Queue szolgáltatásügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5ca0-122">Create the Queue service client</span></span>
<span data-ttu-id="b5ca0-123">A **CloudQueueClient** osztály segítségével lekérheti a Queue Storage-ban tárolt üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-123">The **CloudQueueClient** class enables you to retrieve queues stored in Queue storage.</span></span> <span data-ttu-id="b5ca0-124">A szolgáltatásügyfél létrehozásának egyik módja:</span><span class="sxs-lookup"><span data-stu-id="b5ca0-124">Here's one way to create the service client:</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
<span data-ttu-id="b5ca0-125">Most már készen áll a Queue Storage-ból adatokat olvasó és abba adatokat író kód írására.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-125">Now you are ready to write code that reads data from and writes data to Queue storage.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="b5ca0-126">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5ca0-126">Create a queue</span></span>
<span data-ttu-id="b5ca0-127">A példa bemutatja, hogyan hozhat létre üzenetsort, ha még nem rendelkezik vele:</span><span class="sxs-lookup"><span data-stu-id="b5ca0-127">This example shows how to create a queue if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a><span data-ttu-id="b5ca0-128">Üzenet beszúrása egy üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="b5ca0-128">Insert a message into a queue</span></span>
<span data-ttu-id="b5ca0-129">Ha üzenetet szeretne beszúrni egy létező üzenetsorba, először hozzon létre egy **CloudQueueMessage** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-129">To insert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="b5ca0-130">Ezután hívja meg az **AddMessage** módszert.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-130">Next, call the **AddMessage** method.</span></span> <span data-ttu-id="b5ca0-131">A **CloudQueueMessage** egy karakterláncból (UTF-8 formátumban) vagy egy **bájttömbből** hozható létre.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-131">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a **byte** array.</span></span> <span data-ttu-id="b5ca0-132">Az alábbi kód létrehoz egy üzenetsort (ha még nem létezik), és beszúrja a „Hello, World” üzenetet:</span><span class="sxs-lookup"><span data-stu-id="b5ca0-132">Here is code which creates a queue (if it doesn't exist) and inserts the message 'Hello, World':</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-the-next-message"></a><span data-ttu-id="b5ca0-133">Betekintés a következő üzenetbe</span><span class="sxs-lookup"><span data-stu-id="b5ca0-133">Peek at the next message</span></span>
<span data-ttu-id="b5ca0-134">Egy üzenetsor elején található üzenetbe anélkül is bepillanthat, hogy eltávolítaná az üzenetsorból. Ehhez hívja meg a **PeekMessage** módszert.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-134">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessage** method.</span></span>

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-the-contents-of-a-queued-message"></a><span data-ttu-id="b5ca0-135">Üzenetsorban található üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="b5ca0-135">Change the contents of a queued message</span></span>
<span data-ttu-id="b5ca0-136">Egy üzenetet tartalmát helyben, az üzenetsorban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-136">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="b5ca0-137">Ha az üzenet munkafeladatot jelöl, ezzel a funkcióval frissítheti a munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-137">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="b5ca0-138">Az alábbi kód frissíti az üzenetsorban található üzenetet az új tartalommal, és a láthatósági időkorlátot további 60 másodperccel bővíti.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-138">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="b5ca0-139">Elmenti az üzenethez társított feladat állapotát, és az ügyfél számára további egy percet biztosít az üzenet használatának folytatására.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-139">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="b5ca0-140">Ezzel a technikával többlépéses munkafolyamatokat is nyomon követhet az üzenetsor üzenetein anélkül, hogy újra kéne kezdenie, ha a folyamat valamelyik lépése hardver- vagy szoftverhiba miatt meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-140">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="b5ca0-141">A rendszer általában nyilvántartja az újrapróbálkozások számát, és ha az üzenettel *n* alkalomnál többször próbálkoznak, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-141">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="b5ca0-142">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-142">This protects against a message that triggers an application error each time it is processed.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent("Updated contents.");
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-the-next-message"></a><span data-ttu-id="b5ca0-143">A következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="b5ca0-143">De-queue the next message</span></span>
<span data-ttu-id="b5ca0-144">A kód két lépésben távolít el egy üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-144">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="b5ca0-145">A **GetMessage** meghívásával lekéri az üzenetsor következő üzenetét.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-145">When you call **GetMessage**, you get the next message in a queue.</span></span> <span data-ttu-id="b5ca0-146">A **GetMessage** módszerrel lekért üzenet láthatatlanná válik az adott üzenetsorban található üzeneteket olvasó többi kód számára.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-146">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="b5ca0-147">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-147">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="b5ca0-148">Ha végleg el szeretné távolítani az üzenetet az üzenetsorból, meg kell hívnia a **DeleteMessage** módszert is.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-148">To finish removing the message from the queue, you must also call **DeleteMessage**.</span></span> <span data-ttu-id="b5ca0-149">Az üzenetek kétlépéses eltávolítása lehetővé teszi, hogy ha a kód hardver- vagy szoftverhiba miatt nem tud feldolgozni egy üzenetet, a kód egy másik példánya megkaphassa ugyanazt az üzenetet, és újra megpróbálkozhasson a feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-149">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="b5ca0-150">A kód a **DeleteMessage** módszert rögtön az üzenet feldolgozása után hívja meg.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-150">Your code calls **DeleteMessage** right after the message has been processed.</span></span>

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a><span data-ttu-id="b5ca0-151">Async-Await mintázat használata közös Queue Storage API-kkal</span><span class="sxs-lookup"><span data-stu-id="b5ca0-151">Use Async-Await pattern with common Queue storage APIs</span></span>
<span data-ttu-id="b5ca0-152">Ez a példa bemutatja, hogyan használható az Async-Await mintázat a közös Queue Storage API-kkal.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-152">This example shows how to use the Async-Await pattern with common Queue storage APIs.</span></span> <span data-ttu-id="b5ca0-153">A minta meghívja az adott módszerek aszinkron verzióját. Az aszinkronitást az egyes módszerek *Async* utótagja jelöli.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-153">The sample calls the asynchronous version of each of the given methods, as indicated by the *Async* suffix of each method.</span></span> <span data-ttu-id="b5ca0-154">Ha Async módszert használ, az Async-Await mintázat felfüggeszti a helyi végrehajtást a hívás befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-154">When an async method is used, the async-await pattern suspends local execution until the call completes.</span></span> <span data-ttu-id="b5ca0-155">Ez a viselkedés lehetővé teszi, hogy az aktuális szál más feladatokkal foglalkozzon. Ennek segítségével elkerülhetők a szűk keresztmetszetek a teljesítményben, és az alkalmazás általános válaszkészsége is javul.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-155">This behavior allows the current thread to do other work, which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="b5ca0-156">További információk az Async-Await mintázat használatáról .NET-keretrendszerben: [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx) (Async és Await (C# és Visual Basic)).</span><span class="sxs-lookup"><span data-stu-id="b5ca0-156">For more details on using the Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="b5ca0-157">Egyéb lehetőségek az üzenetek eltávolításához az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="b5ca0-157">Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="b5ca0-158">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-158">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="b5ca0-159">Az első lehetőség az üzenetkötegek (legfeljebb 32) lekérése.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-159">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="b5ca0-160">A második lehetőség az, hogy beállít egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak lehetősége van hosszabb vagy rövidebb idő alatt teljesen feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-160">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="b5ca0-161">Az alábbi példakód a **GetMessages** módszer segítségével egyszerre 20 üzenetet kér le.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-161">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="b5ca0-162">Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-162">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="b5ca0-163">Mindemellett a láthatatlansági időkorlátot minden üzenethez öt percre állítja be.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-163">It also sets the invisibility timeout to five minutes for each message.</span></span> <span data-ttu-id="b5ca0-164">Vegye figyelembe, hogy az 5 perc minden üzenetnél ugyanakkor kezdődik, tehát 5 perccel a **GetMessages** hívása után a nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-164">Note that the 5 minutes starts for all messages at the same time, so after 5 minutes have passed since the call to **GetMessages**, any messages which have not been deleted will become visible again.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a><span data-ttu-id="b5ca0-165">Az üzenetsor hosszának lekérése</span><span class="sxs-lookup"><span data-stu-id="b5ca0-165">Get the queue length</span></span>
<span data-ttu-id="b5ca0-166">Megbecsülheti egy üzenetsorban található üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-166">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="b5ca0-167">A **FetchAttributes** módszer lekéri a Queue szolgáltatásból az üzenetsorra vonatkozó attribútumokat, amelyek között megtalálható az üzenetek száma is.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-167">The **FetchAttributes** method asks the Queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="b5ca0-168">Az **ApproximateMessageCount** tulajdonság visszaadja a **FetchAttributes** módszer által lekért legutóbbi értéket a Queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-168">The **ApproximateMessageCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the Queue service.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a><span data-ttu-id="b5ca0-169">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="b5ca0-169">Delete a queue</span></span>
<span data-ttu-id="b5ca0-170">Egy üzenetsor és az összes benne foglalt üzenet törléséhez hívja meg a **Delete** módszert az üzenetsor-objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-170">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```
    

## <a name="next-steps"></a><span data-ttu-id="b5ca0-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5ca0-171">Next steps</span></span>
<span data-ttu-id="b5ca0-172">Most, hogy már megismerte a Queue Storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről is.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-172">Now that you've learned the basics of Queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="b5ca0-173">A Queue szolgáltatás elérhető API-kat részletesen ismertető referenciadokumentációjának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="b5ca0-173">View the Queue service reference documentation for complete details about available APIs:</span></span>
  * [<span data-ttu-id="b5ca0-174">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="b5ca0-174">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [<span data-ttu-id="b5ca0-175">REST API – referencia</span><span class="sxs-lookup"><span data-stu-id="b5ca0-175">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="b5ca0-176">Megtudhatja, hogyan egyszerűsítheti az [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) használatával az Azure Storage használatához írt kódot.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-176">Learn how to simplify the code you write to work with Azure Storage by using the [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span>
* <span data-ttu-id="b5ca0-177">Az Azure-ban való adattárolás további lehetőségeiről tekintse meg a többi szolgáltatás-útmutatót.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-177">View more feature guides to learn about additional options for storing data in Azure.</span></span>
  * <span data-ttu-id="b5ca0-178">[Get started with Azure Table Storage using .NET](storage-dotnet-how-to-use-tables.md) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel) a strukturált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-178">[Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md) to store structured data.</span></span>
  * <span data-ttu-id="b5ca0-179">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel) a strukturálatlan adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-179">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) to store unstructured data.</span></span>
  * <span data-ttu-id="b5ca0-180">[Csatlakozzon az SQL Database adatbázishoz .NET (C#) használatával](../sql-database/sql-database-develop-dotnet-simple.md) a relációs adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="b5ca0-180">[Connect to SQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) to store relational data.</span></span>

[Download and install the Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
