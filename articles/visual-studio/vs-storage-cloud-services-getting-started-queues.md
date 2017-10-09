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
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Ismerkedés az Azure Queue storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooget el az Azure Queue storage a Visual Studióban létrehozott vagy egy Azure storage-fiók felhőalapú szolgáltatások projektben hivatkozott hello Visual Studio használatával után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel .

Mutat be, hogyan toocreate kód üzenetsorból. Is mutat be, hogyan tooperform basic várólistára műveletek, például a hozzáadása, módosítása, olvasási és eltávolításakor üzenetsor-üzeneteket. hello Kódminták C# kódban vannak megírva, és használják hello [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.

* Lásd: [Ismerkedés az Azure Queue storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt a kódban várólisták kezelésére.
* Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.
* Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.
* Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.

Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához. Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.

## <a name="access-queues-in-code"></a>A kód várólisták elérése
a Visual Studio Felhőszolgáltatás-projektek tooaccess várólisták, következőkre lesz szüksége tooinclude hello elemek tooany C# forrásfájl elérhető Azure Queue storage.

1. Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Első egy **CloudQueueClient** tooreference hello várólista-objektumok a tárfiókban lévő objektum.  
   
        // Create hello queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Első egy **CloudQueue** objektum tooreference egy konkrét várólistába helyezi.
   
        // Get a reference tooa queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Megjegyzés:** a következő minták hello az összes fenti kódot hello kód előtt hello használja.

## <a name="create-a-queue-in-code"></a>Várólista létrehozása a kódban
toocreate hello várólista a kódban, csak adjon hozzá egy túl**CreateIfNotExists**.

    // Create hello CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-tooa-queue"></a>Egy üzenetsor tooa hozzáadása
egy létező üzenetsorba, az üzenet tooinsert hozzon létre egy új **CloudQueueMessage** objektumot, majd hívás hello **AddMessage** metódust.

A **CloudQueueMessage** objektum vagy egy karakterláncból (UTF-8 formátumban), vagy egy bájttömböt hozható létre.

Íme egy példa, ami üdvözlőüzenetére beszúrja a "Hello, World".

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Olvassa el egy üzenetet a várólistában egyszerre várakozó
Eltávolítása hello által hívó hello anélkül is bepillanthat hello betekintés a várólista elejére hello **PeekMessage** metódust.

    // Peek at hello next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Olvassa el, és távolítsa el az üzenet a várólistában lévő
A kód eltávolítása (kivétele az üzenetsorból) egy üzenetet az üzenetsorból két lépésben.

1. Hívás **GetMessage** tooget hello várólista következő üzenetére egy. Az üzenet **GetMessage** láthatatlan tooany ebből a várólistából üzeneteket olvasó többi kód válik. Alapértelmezés szerint az üzenet 30 másodpercig marad láthatatlan.
2. üdvözlőüzenetére eltávolítása hello várólista, hívás toofinish **DeleteMessage**.

A kétlépéses folyamat eltávolításával előállított üzenet biztosítja, hogy a kód meghibásodásakor tooprocess miatt toohardware vagy szoftver-hiba, a kód egy másik példánya üzenet kérheti le hello ugyanazt az üzenetet, és próbálkozzon újra. hello következő kódot a hívások **DeleteMessage** jobb gombbal az üdvözlő üzenet feldolgozása után.

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process hello message in less than 30 seconds

    // Then delete hello message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-tooprocess-and-remove-queue-messages"></a>További beállítások tooprocess használja, és távolítsa el a várólista-üzenetek
Két módon szabhatja testre az üzenetek lekérését egy üzenetsorból.

* Az üzenetkötegek (felfelé too32) kérheti le.
* Megadhat egy hosszabb vagy rövidebb láthatatlansági időkorlátot, így a kódnak több vagy kevesebb idő toofully feldolgozni az egyes üzeneteket. hello alábbi példakód a **GetMessages** metódus tooget 20 üzenetek egy hívásban. Ezután minden üzenetet feldolgoz egy **foreach** hurok segítségével. Beállítja a hello láthatatlansági időtúllépés toofive percig, amíg minden üzenetet is. Vegye figyelembe, hogy hello 5 perc indítása: hello összes üzenet azonos idő, így miután 5 perc telt túl hello hívás**GetMessages**, nem törölt üzenetek újra láthatóvá válnak.

Íme egy példa:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete hello message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-hello-queue-length"></a>Első hello várólistájának hossza
A várólistában lévő üzenetek hello számának becslése kérheti le. A **FetchAttributes** módszert kéri hello Queue szolgáltatás lekérdezni hello várólista attribútumokat, beleértve a hello üzenetek száma. Hello **ApproximateMethodCount** tulajdonság hello által lekért legutóbbi értéket adja vissza a **FetchAttributes** metódus hello Queue szolgáltatás hívása nélkül.

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-azure-queue-apis"></a>Hello Async-Await mintázat használata közös Azure várólista API-k
Ez a példa bemutatja, hogyan toouse hello Async-Await mintázat a közös Azure várólista API-k. hello minta meghívja az egyes módszerek megadott hello hello aszinkron verziója, ez hello látható **aszinkron** az egyes módszerek utáni javítás. Egy aszinkron metódus esetén használt hello async-await mintázat felfüggeszti a helyi végrehajtást hello hívás befejeződéséig. Ez a viselkedés lehetővé teszi, hogy hello aktuális szál toodo más feladatok, amely segít a elkerülhetők a szűk keresztmetszetek, és növeli az alkalmazás általános válaszkészsége hello. További információk az Async-Await mintázat a .NET hello lásd: [Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

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

## <a name="delete-a-queue"></a>Üzenetsor törlése
egy üzenetsor és az összes köszönőüzenetei benne tárolt, hívás hello toodelete **törlése** hello várólista-objektum metódust.

    // Delete hello queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

