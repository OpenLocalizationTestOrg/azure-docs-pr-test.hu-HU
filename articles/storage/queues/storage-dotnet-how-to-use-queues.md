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
# <a name="get-started-with-azure-queue-storage-using-net"></a>Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Áttekintés
Az Azure Queue Storage felhőbeli üzenetkezelést biztosít az alkalmazások összetevői között. A méretezhető alkalmazások tervezésekor az alkalmazás összetevői gyakran le vannak választva, hogy egymástól függetlenül lehessen őket méretezni. A Queue storage biztosítja, hogy az alkalmazás-összetevők közötti kommunikáció aszinkron üzenetkezelési e hello felhőben, hello asztalon, egy helyszíni kiszolgálón vagy egy mobileszközön futnak. A Queue Storage támogatja az aszinkron feladatok kezelését és a feldolgozási munkafolyamatok kialakítását is.

### <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag bemutatja, hogyan toowrite .NET olyan gyakori forgatókönyveket tartalmaz Azure Queue storage segítségével helykódja. Az ismertetett forgatókönyvek az üzenetsorok létrehozására és törlésére, valamint az üzenetsor üzeneteinek hozzáadására, olvasására és törlésére vonatkoznak.

**Becsült idő toocomplete:** 45 percben

**Előfeltételek:**

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager a .NET-hez](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* Egy [Azure-tárfiók](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Hozzáadás irányelvekkel
Adja hozzá a következő hello `using` irányelvek toohello felső részén hello `Program.cs` fájlt:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-hello-connection-string"></a>Hello kapcsolati karakterlánc elemzése
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-queue-service-client"></a>Hello Queue szolgáltatásügyfél létrehozása
Hello **CloudQueueClient** osztály lehetővé teszi, hogy Ön tooretrieve tárolt üzenetsorokat Queue storage-ban. Egyirányú toocreate hello szolgáltatás ügyfele a következő:

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
Most már készen áll a toowrite kódot, amely adatokat olvas és ír tooQueue adattárolás áll.

## <a name="create-a-queue"></a>Üzenetsor létrehozása
A példa bemutatja, hogyan toocreate egy üzenetsort, ha még nem létezik:

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

## <a name="insert-a-message-into-a-queue"></a>Üzenet beszúrása egy üzenetsorba
tooinsert egy létező üzenetsorba, az üzenet először létre kell hoznia egy új **CloudQueueMessage**. Ezután hívja hello **AddMessage** metódust. A **CloudQueueMessage** egy karakterláncból (UTF-8 formátumban) vagy egy **bájttömbből** hozható létre. Íme, amely létrehoz egy üzenetsort (Ha még nem létezik) és a Beszúrás üdvözlőüzenetére "Hello, World" code:

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

## <a name="peek-at-hello-next-message"></a>Betekintés a következő köszönőüzenetei
Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **PeekMessage** metódust.

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

## <a name="change-hello-contents-of-a-queued-message"></a>Hello aszinkron üzenet tartalmának módosítása
Módosíthatja egy üzenet helyben hello várólista hello tartalmát. Az üzenet jelöl, használhatja a szolgáltatás tooupdate hello munkafeladat állapotát. a következő kód hello hello üzenetsor frissíti az új tartalommal, és a készletek hello látható időtúllépés tooextend további 60 másodperccel. Hello üdvözlőüzenetére társított feladat állapotát menti, és lehetőséget ad a hello ügyfél üdvözlőüzenetére dolgozik egy másik perces toocontinue. Ez a módszer tootrack többlépéses munkafolyamatokat üzenetsor üzenetein anélkül, hogy toostart keresztül hello kezdődően, ha a folyamat valamelyik lépése toohardware-vagy szoftverhiba miatt nem használható. Általában tartja az újrapróbálkozások számát is, és ha hello üzenetet a rendszer ismét megkísérli több mint  *n*  időpontokat, akkor törlődik. Ez védelmet biztosít az ellen, hogy egy üzenetet minden feldolgozásakor kiváltson egy alkalmazáshibát.

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

## <a name="de-queue-hello-next-message"></a>Hello következő üzenet kivétele az üzenetsorból
A kód két lépésben távolít el egy üzenetet az üzenetsorból. A hívás esetén **GetMessage**, a sorhoz hello a következő üzenetet kapott. Az üzenet **GetMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik. Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan. toofinish eltávolításakor üdvözlőüzenetére hello üzenetsorból, meg kell is hívni **DeleteMessage**. A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra. A kód hívások **DeleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.

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

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Async-Await mintázat használata közös Queue Storage API-kkal
Ez a példa bemutatja, hogyan toouse hello Async-Await mintázat a közös Queue storage API-k. hello minta meghívja hello aszinkron verzióját a megadott metódusok, hello hello jelöli *aszinkron* az egyes módszerek utótag. Egy aszinkron metódus használata esetén hello async-await mintázat felfüggeszti a helyi végrehajtást hello hívás befejeződéséig. Ez a viselkedés lehetővé teszi, hogy hello aktuális szál toodo más feladatok, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége hello. További információk az Async-Await mintázat a .NET hello lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

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
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a>Egyéb lehetőségek az üzenetek eltávolításához az üzenetsorból
Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.
Először is kaphat az üzenetkötegek (felfelé too32). Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket. hello alábbi példakód a **GetMessages** metódus tooget 20 üzenetek egy hívásban. Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével. Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is. Vegye figyelembe, hogy hello 5 perc indítása: hello összes üzenet azonos idő, így miután 5 perc telt túl hello hívás**GetMessages**, nem törölt üzenetek újra láthatóvá válnak.

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

## <a name="get-hello-queue-length"></a>Első hello várólistájának hossza
A várólistában lévő üzenetek hello számának becslése kérheti le. A **FetchAttributes** módszert kéri hello Queue szolgáltatás lekérdezni hello várólista attribútumokat, beleértve a hello üzenetek száma. Hello **ApproximateMessageCount** tulajdonság hello által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus hello Queue szolgáltatás hívása nélkül.

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

## <a name="delete-a-queue"></a>Üzenetsor törlése
toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **törlése** hello várólista-objektum metódust.

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
    

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Queue storage alapjait hello, kövesse az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn.

* Nézet hello várólista szolgáltatás elérhető API-kat vonatkozó dokumentációját:
  * [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST API – referencia](http://msdn.microsoft.com/library/azure/dd179355)
* Ismerje meg, hogyan toosimplify hello kódot ír hello segítségével az Azure Storage toowork [Azure WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk.md).
* Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.
  * [Ismerkedés az Azure Table storage használatának .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) toostore strukturált adatok.
  * [Az Azure Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) toostore strukturálatlan adatok.
  * [.NET (C#) használatával kapcsolódnak a tooSQL adatbázis](../../sql-database/sql-database-connect-query-dotnet-core.md) toostore relációs adatok.

[Download and install hello Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
