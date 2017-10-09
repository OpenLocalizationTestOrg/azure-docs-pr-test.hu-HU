---
title: "a queue storage és a Visual Studio (az ASP.NET Core) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget el az Azure üzenetsorának tárolást használó az ASP.NET Core projektre a Visual Studióban"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 75adb7163827ab17ad89707051ff0e48dbae9c3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Ismerkedés a queue storage és a Visual Studio a kapcsolódó szolgáltatások (ASP.NET mag)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooget el az Azure Queue storage a Visual Studióban létrehozott vagy egy Azure storage-fiók ASP.NET Core projektben hivatkozott hello Visual Studio használatával után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel. Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.

Azure a queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához. Egyetlen üzenetsor mentése too64 kilobájt (KB) méretű lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.

tooget elindult, először egy Azure-üzenetsorba toocreate tárfiókba. Mutat be, hogyan toocreate kód üzenetsorból. Is mutat be, hogyan tooperform basic várólistára műveletek, például a hozzáadása, módosítása, olvasása, és eltávolítása az üzenetsor-üzeneteket. hello minták C nyelven íródtak\# code, és használja a hello Azure Storage ügyféloldali kódtára a .NET-hez. ASP.NET kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).

**Megjegyzés:** hello API-hívások tooAzure tárolási hajtsa végre az ASP.NET Core aszinkron. Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt. az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.

* Lásd: [Ismerkedés az Azure Queue storage használatának .NET](storage-dotnet-how-to-use-queues.md) további információt a sorok programozott módon kezelésére.
* Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.
* Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.
* Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.

## <a name="access-queues-in-code"></a>A kód várólisták elérése
az ASP.NET Core projektek tooaccess várólistákból, következőkre lesz szüksége tooinclude hello elemek tooany C# forrásfájl hozzáférő Azure várólista-tároló.

1. Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Első egy **CloudQueueClient** tooreference hello várólista-objektumok a tárfiókban lévő objektum.  
   
        // Create hello CloudQueueClient object for hello storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Első egy **CloudQueue** objektum tooreference egy konkrét várólistába helyezi.
   
        // Get a reference toohello CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Megjegyzés:** a következő minták hello az összes fenti kódot hello kód előtt hello használja.

### <a name="create-a-queue-in-code"></a>Várólista létrehozása a kódban
a kódban, az Azure üzenetsorának toocreate hello csak adjon hozzá egy túl**CreateIfNotExistsAsync**.

    // Create hello CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-tooa-queue"></a>Egy üzenetsor tooa hozzáadása
egy létező üzenetsorba, az üzenet tooinsert hozzon létre egy új **CloudQueueMessage** objektumot, majd hívás hello **AddMessageAsync** metódust.

A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.

Íme egy példa, ami üdvözlőüzenetére beszúrja a "Hello, World".

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Olvassa el egy üzenetet a várólistában egyszerre várakozó
Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **PeekMessageAsync** metódust.

    // Peek hello next message in hello queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Olvassa el, és távolítsa el az üzenet a várólistában lévő
A kód eltávolítása (created) egy üzenetet az üzenetsorból két lépésben.

1. Hívás **GetMessageAsync** tooget hello várólista következő üzenetére egy. Az üzenet **GetMessageAsync** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik. Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.
2. üdvözlőüzenetére eltávolítása hello várólista, hívás toofinish **DeleteMessageAsync**.

A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra. hello következő kódot a hívások **DeleteMessageAsync** jobb gombbal az üdvözlő üzenet feldolgozása után.

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process hello message in less than 30 seconds.

    // Then delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Egyéb lehetőségek az üzenetek üzenetmozgatót
Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.
Először is kaphat az üzenetkötegek (felfelé too32). Második beállíthat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket. hello alábbi példakód a **GetMessages** metódus tooget 20 üzenetek egy hívásban. Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével. Beállítja a hello láthatatlansági időtúllépés too5 percig, amíg minden üzenetet is. Vegye figyelembe, hogy hello 5 perc start összes üzenetek: hello azonos idő, így miután 5 perc telt hello hívása után túl**GetMessages**, nem törölt üzenetek újra lesz látható.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-hello-queue-length"></a>Első hello várólistájának hossza
A várólistában lévő üzenetek hello számának becslése kérheti le. A **FetchAttributes** módszert kéri hello queue szolgáltatás lekérdezni hello várólista attribútumokat, beleértve a hello üzenetek száma. Hello **ApproximateMethodCount** tulajdonság hello által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus hello queue szolgáltatás hívása nélkül.

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display hello number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-queue-apis"></a>Hello Async-Await mintázat használata közös várólista API-k
Ez a példa bemutatja, hogyan toouse hello Async-Await mintázat a közös várólista API-k. hello minta hívások hello aszinkron verzióját módszerek megadott hello. Ez hello aszinkron utáni javítás az egyes módszerek által látható. Egy aszinkron metódus használata esetén hello Async-Await mintázat felfüggeszti helyi hello hívás befejeződéséig. Ez a viselkedés lehetővé teszi, hogy hello aktuális szál toodo más feladatok, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége hello. További információk az Async-Await mintázat a .NET hello, lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

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
## <a name="delete-a-queue"></a>Üzenetsor törlése
toodelete várólista és köszönőüzenetei minden benne tárolt, hívja az **törlése** hello várólista-objektum metódust.

    // Delete hello queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Következő lépések
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

