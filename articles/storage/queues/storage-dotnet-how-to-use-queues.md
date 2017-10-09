---
title: "az Azure Queue storage használatának .NET használatába aaaGet |} Microsoft Docs"
description: "Az Azure-üzenetsorok megbízható, aszinkron üzenetkezelést biztosítanak az alkalmazások összetevői között. A felhő üzenetkezelési lehetővé teszi, hogy az alkalmazás-összetevők tooscale egymástól függetlenül."
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
ms.openlocfilehash: 0cf6a71392b2fe859c7c9a9898c1ec84bcab4b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a><span data-ttu-id="42ec6-104">Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="42ec6-104">Get started with Azure Queue storage using .NET</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a><span data-ttu-id="42ec6-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="42ec6-105">Overview</span></span>
<span data-ttu-id="42ec6-106">Az Azure Queue Storage felhőbeli üzenetkezelést biztosít az alkalmazások összetevői között.</span><span class="sxs-lookup"><span data-stu-id="42ec6-106">Azure Queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="42ec6-107">A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni.</span><span class="sxs-lookup"><span data-stu-id="42ec6-107">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="42ec6-108">A Queue storage biztosítja, hogy az alkalmazás-összetevők közötti kommunikáció aszinkron üzenetkezelési e hello felhőben, hello asztalon, egy helyszíni kiszolgálón vagy egy mobileszközön futnak.</span><span class="sxs-lookup"><span data-stu-id="42ec6-108">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in hello cloud, on hello desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="42ec6-109">A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.</span><span class="sxs-lookup"><span data-stu-id="42ec6-109">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="42ec6-110">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="42ec6-110">About this tutorial</span></span>
<span data-ttu-id="42ec6-111">Ez az oktatóanyag bemutatja, hogyan toowrite .NET olyan gyakori forgatókönyveket tartalmaz Azure Queue storage segítségével helykódja.</span><span class="sxs-lookup"><span data-stu-id="42ec6-111">This tutorial shows how toowrite .NET code for some common scenarios using Azure Queue storage.</span></span> <span data-ttu-id="42ec6-112">Az ismertetett forgatókönyvek az üzenetsorok létrehozására és törlésére, valamint az üzenetsor üzeneteinek hozzáadására, olvasására és törlésére vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="42ec6-112">Scenarios covered include creating and deleting queues and adding, reading, and deleting queue messages.</span></span>

<span data-ttu-id="42ec6-113">**Becsült idő toocomplete:** 45 percben</span><span class="sxs-lookup"><span data-stu-id="42ec6-113">**Estimated time toocomplete:** 45 minutes</span></span>

<span data-ttu-id="42ec6-114">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="42ec6-114">**Prerequisities:**</span></span>

* [<span data-ttu-id="42ec6-115">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42ec6-115">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="42ec6-116">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="42ec6-116">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="42ec6-117">Azure Configuration Manager a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="42ec6-117">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="42ec6-118">Egy [Azure-tárfiók](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="42ec6-118">An [Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="42ec6-119">Hozzáadás irányelvekkel</span><span class="sxs-lookup"><span data-stu-id="42ec6-119">Add using directives</span></span>
<span data-ttu-id="42ec6-120">Adja hozzá a következő hello `using` irányelvek toohello felső részén hello `Program.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="42ec6-120">Add hello following `using` directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="42ec6-121">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="42ec6-121">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-queue-service-client"></a><span data-ttu-id="42ec6-122">Hello Queue szolgáltatásügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="42ec6-122">Create hello Queue service client</span></span>
<span data-ttu-id="42ec6-123">Hello **CloudQueueClient** osztály lehetővé teszi, hogy Ön tooretrieve tárolt üzenetsorokat Queue storage-ban.</span><span class="sxs-lookup"><span data-stu-id="42ec6-123">hello **CloudQueueClient** class enables you tooretrieve queues stored in Queue storage.</span></span> <span data-ttu-id="42ec6-124">Egyirányú toocreate hello szolgáltatás ügyfele a következő:</span><span class="sxs-lookup"><span data-stu-id="42ec6-124">Here's one way toocreate hello service client:</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
<span data-ttu-id="42ec6-125">Most már készen áll a toowrite kódot, amely adatokat olvas és ír tooQueue adattárolás áll.</span><span class="sxs-lookup"><span data-stu-id="42ec6-125">Now you are ready toowrite code that reads data from and writes data tooQueue storage.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="42ec6-126">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="42ec6-126">Create a queue</span></span>
<span data-ttu-id="42ec6-127">A példa bemutatja, hogyan toocreate egy üzenetsort, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="42ec6-127">This example shows how toocreate a queue if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create hello queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a><span data-ttu-id="42ec6-128">Üzenet beszúrása egy üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="42ec6-128">Insert a message into a queue</span></span>
<span data-ttu-id="42ec6-129">tooinsert egy létező üzenetsorba, az üzenet először létre kell hoznia egy új **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="42ec6-129">tooinsert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="42ec6-130">Ezután hívja hello **AddMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="42ec6-130">Next, call hello **AddMessage** method.</span></span> <span data-ttu-id="42ec6-131">A **CloudQueueMessage** egy karakterláncból (UTF-8 formátumban) vagy egy **bájttömbből** hozható létre.</span><span class="sxs-lookup"><span data-stu-id="42ec6-131">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a **byte** array.</span></span> <span data-ttu-id="42ec6-132">Íme, amely létrehoz egy üzenetsort (Ha még nem létezik) és a Beszúrás üdvözlőüzenetére "Hello, World" code:</span><span class="sxs-lookup"><span data-stu-id="42ec6-132">Here is code which creates a queue (if it doesn't exist) and inserts hello message 'Hello, World':</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create hello queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it toohello queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-hello-next-message"></a><span data-ttu-id="42ec6-133">Betekintés a következő köszönőüzenetei</span><span class="sxs-lookup"><span data-stu-id="42ec6-133">Peek at hello next message</span></span>
<span data-ttu-id="42ec6-134">Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **PeekMessage** metódust.</span><span class="sxs-lookup"><span data-stu-id="42ec6-134">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessage** method.</span></span>

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at hello next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-hello-contents-of-a-queued-message"></a><span data-ttu-id="42ec6-135">Hello aszinkron üzenet tartalmának módosítása</span><span class="sxs-lookup"><span data-stu-id="42ec6-135">Change hello contents of a queued message</span></span>
<span data-ttu-id="42ec6-136">Módosíthatja egy üzenet helyben hello várólista hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="42ec6-136">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="42ec6-137">Az üzenet jelöl, használhatja a szolgáltatás tooupdate hello munkafeladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="42ec6-137">If the message represents a work task, you could use this feature tooupdate the status of hello work task.</span></span> <span data-ttu-id="42ec6-138">a következő kód hello hello üzenetsor frissíti az új tartalommal, és a készletek hello látható időtúllépés tooextend további 60 másodperccel.</span><span class="sxs-lookup"><span data-stu-id="42ec6-138">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="42ec6-139">Hello üdvözlőüzenetére társított feladat állapotát menti, és lehetőséget ad a hello ügyfél üdvözlőüzenetére dolgozik egy másik perces toocontinue.</span><span class="sxs-lookup"><span data-stu-id="42ec6-139">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="42ec6-140">Ez a módszer tootrack többlépéses munkafolyamatokat üzenetsor üzenetein anélkül, hogy toostart keresztül hello kezdődően, ha a folyamat valamelyik lépése toohardware-vagy szoftverhiba miatt nem használható.</span><span class="sxs-lookup"><span data-stu-id="42ec6-140">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="42ec6-141">Általában tartja az újrapróbálkozások számát is, és ha hello üzenetet a rendszer ismét megkísérli több mint  *n*  időpontokat, akkor törlődik.</span><span class="sxs-lookup"><span data-stu-id="42ec6-141">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="42ec6-142">Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.</span><span class="sxs-lookup"><span data-stu-id="42ec6-142">This protects against a message that triggers an application error each time it is processed.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get hello message from hello queue and update hello message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent("Updated contents.");
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-hello-next-message"></a><span data-ttu-id="42ec6-143">Hello következő üzenet kivétele az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="42ec6-143">De-queue hello next message</span></span>
<span data-ttu-id="42ec6-144">A kód két lépésben távolít el egy üzenetet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="42ec6-144">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="42ec6-145">A hívás esetén **GetMessage**, a sorhoz hello a következő üzenetet kapott.</span><span class="sxs-lookup"><span data-stu-id="42ec6-145">When you call **GetMessage**, you get hello next message in a queue.</span></span> <span data-ttu-id="42ec6-146">Az üzenet **GetMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik.</span><span class="sxs-lookup"><span data-stu-id="42ec6-146">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="42ec6-147">Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.</span><span class="sxs-lookup"><span data-stu-id="42ec6-147">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="42ec6-148">toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="42ec6-148">toofinish removing hello message from hello queue, you must also call **DeleteMessage**.</span></span> <span data-ttu-id="42ec6-149">A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="42ec6-149">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="42ec6-150">A kód hívások **DeleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.</span><span class="sxs-lookup"><span data-stu-id="42ec6-150">Your code calls **DeleteMessage** right after hello message has been processed.</span></span>

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get hello next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process hello message in less than 30 seconds, and then delete hello message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a><span data-ttu-id="42ec6-151">Async-Await mintázat használata közös Queue Storage API-kkal</span><span class="sxs-lookup"><span data-stu-id="42ec6-151">Use Async-Await pattern with common Queue storage APIs</span></span>
<span data-ttu-id="42ec6-152">Ez a példa bemutatja, hogyan toouse hello Async-Await mintázat a közös Queue storage API-k.</span><span class="sxs-lookup"><span data-stu-id="42ec6-152">This example shows how toouse hello Async-Await pattern with common Queue storage APIs.</span></span> <span data-ttu-id="42ec6-153">hello minta meghívja hello aszinkron verzióját a megadott metódusok, hello hello jelöli *aszinkron* az egyes módszerek utótag.</span><span class="sxs-lookup"><span data-stu-id="42ec6-153">hello sample calls hello asynchronous version of each of hello given methods, as indicated by hello *Async* suffix of each method.</span></span> <span data-ttu-id="42ec6-154">Egy aszinkron metódus használata esetén hello async-await mintázat felfüggeszti a helyi végrehajtást hello hívás befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="42ec6-154">When an async method is used, hello async-await pattern suspends local execution until hello call completes.</span></span> <span data-ttu-id="42ec6-155">Ez a viselkedés lehetővé teszi, hogy hello aktuális szál toodo más feladatok, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége hello.</span><span class="sxs-lookup"><span data-stu-id="42ec6-155">This behavior allows hello current thread toodo other work, which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="42ec6-156">További információk az Async-Await mintázat a .NET hello lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="42ec6-156">For more details on using hello Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

```csharp
// Create hello queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message tooput in hello queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue hello message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue hello message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete hello message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="42ec6-157">Egyéb lehetőségek az üzenetek eltávolításához az üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="42ec6-157">Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="42ec6-158">Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="42ec6-158">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="42ec6-159">Először is kaphat az üzenetkötegek (felfelé too32).</span><span class="sxs-lookup"><span data-stu-id="42ec6-159">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="42ec6-160">Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="42ec6-160">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="42ec6-161">hello alábbi példakód a **GetMessages** metódus tooget 20 üzenetek egy hívásban.</span><span class="sxs-lookup"><span data-stu-id="42ec6-161">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="42ec6-162">Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével.</span><span class="sxs-lookup"><span data-stu-id="42ec6-162">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="42ec6-163">Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is.</span><span class="sxs-lookup"><span data-stu-id="42ec6-163">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="42ec6-164">Vegye figyelembe, hogy hello 5 perc indítása: hello összes üzenet azonos idő, így miután 5 perc telt túl hello hívás**GetMessages**, nem törölt üzenetek újra láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="42ec6-164">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**GetMessages**, any messages which have not been deleted will become visible again.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-hello-queue-length"></a><span data-ttu-id="42ec6-165">Első hello várólistájának hossza</span><span class="sxs-lookup"><span data-stu-id="42ec6-165">Get hello queue length</span></span>
<span data-ttu-id="42ec6-166">A várólistában lévő üzenetek hello számának becslése kérheti le.</span><span class="sxs-lookup"><span data-stu-id="42ec6-166">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="42ec6-167">A **FetchAttributes** módszert kéri hello Queue szolgáltatás lekérdezni hello várólista attribútumokat, beleértve a hello üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="42ec6-167">The **FetchAttributes** method asks hello Queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="42ec6-168">Hello **ApproximateMessageCount** tulajdonság hello által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus hello Queue szolgáltatás hívása nélkül.</span><span class="sxs-lookup"><span data-stu-id="42ec6-168">hello **ApproximateMessageCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello Queue service.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch hello queue attributes.
queue.FetchAttributes();

// Retrieve hello cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a><span data-ttu-id="42ec6-169">Üzenetsor törlése</span><span class="sxs-lookup"><span data-stu-id="42ec6-169">Delete a queue</span></span>
<span data-ttu-id="42ec6-170">toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **törlése** hello várólista-objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="42ec6-170">toodelete a queue and all hello messages contained in it, call the **Delete** method on hello queue object.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete hello queue.
queue.Delete();
```
    

## <a name="next-steps"></a><span data-ttu-id="42ec6-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42ec6-171">Next steps</span></span>
<span data-ttu-id="42ec6-172">Most, hogy megismerte a Queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="42ec6-172">Now that you've learned hello basics of Queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="42ec6-173">Nézet hello várólista szolgáltatás elérhető API-kat vonatkozó dokumentációját:</span><span class="sxs-lookup"><span data-stu-id="42ec6-173">View hello Queue service reference documentation for complete details about available APIs:</span></span>
  * [<span data-ttu-id="42ec6-174">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="42ec6-174">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [<span data-ttu-id="42ec6-175">REST API – referencia</span><span class="sxs-lookup"><span data-stu-id="42ec6-175">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="42ec6-176">Ismerje meg, hogyan toosimplify hello kódot ír hello segítségével az Azure Storage toowork [Azure WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="42ec6-176">Learn how toosimplify hello code you write toowork with Azure Storage by using hello [Azure WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span>
* <span data-ttu-id="42ec6-177">Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.</span><span class="sxs-lookup"><span data-stu-id="42ec6-177">View more feature guides toolearn about additional options for storing data in Azure.</span></span>
  * <span data-ttu-id="42ec6-178">[Ismerkedés az Azure Table storage használatának .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) toostore strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="42ec6-178">[Get started with Azure Table storage using .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) toostore structured data.</span></span>
  * <span data-ttu-id="42ec6-179">[Az Azure Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) toostore strukturálatlan adatok.</span><span class="sxs-lookup"><span data-stu-id="42ec6-179">[Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) toostore unstructured data.</span></span>
  * <span data-ttu-id="42ec6-180">[.NET (C#) használatával kapcsolódnak a tooSQL adatbázis](../../sql-database/sql-database-connect-query-dotnet-core.md) toostore relációs adatok.</span><span class="sxs-lookup"><span data-stu-id="42ec6-180">[Connect tooSQL Database by using .NET (C#)](../../sql-database/sql-database-connect-query-dotnet-core.md) toostore relational data.</span></span>

[Download and install hello Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
