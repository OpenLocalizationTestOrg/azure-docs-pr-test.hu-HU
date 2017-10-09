---
title: "az Azure Event Hubs .NET szabványos API-k hello aaaOverview |} Microsoft Docs"
description: ".NET-szabvány API – áttekintés"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a173f8e4-556c-42b8-b856-838189f7e636
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: c97acecb35b69039e06ded7203c75fca41ce98f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-net-standard-api-overview"></a>Event Hubs .NET-szabvány API – áttekintés
Ez a cikk foglal össze néhányat hello kulcs Event Hubs .NET szabványos ügyfél API-k. Jelenleg két .NET-szabvány klienskódtárak segítségével:
* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
  *  Ezt a szalagtárat biztosít minden alapvető futásidejű műveletet.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
  * Ebben a könyvtárban, amely lehetővé teszi, hogy nyomon követése céljából feldolgozott események, és legegyszerűbb módja tooread hello az eseményközpontban lévő további funkciókat ad hozzá.

## <a name="event-hubs-client"></a>Event Hubs-ügyfél
[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) hello toosend események használja elsődleges objektum létezik fogadók és tooget futásidejű információt. Ez az ügyfél csatolt tooa adott eseményközpontban, és létrehoz egy új kapcsolat toohello Event Hubs-végpontot.

### <a name="create-an-event-hubs-client"></a>Event Hubs-ügyfél létrehozása
Egy [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) objektum létrehozása egy kapcsolati karakterláncból. hello legegyszerűbb módja tooinstantiate egy új ügyfél hello a következő példa látható:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

tooprogrammatically hello kapcsolati karakterlánc szerkesztése, használhatja a hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) osztály számára pedig hello kapcsolati karakterlánc paraméterként túl[EventHubClient.CreateFromConnectionString ](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Események küldése
toosend események tooan eseményközpont, használjon hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) osztály. hello törzs kell lennie egy `byte` tömb, vagy egy `byte` tömb szegmens.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Események fogadása
ajánlott módja tooreceive események az Event Hubs hello használ hello [Event Processor Host](#event-processor-host-apis), biztosító funkciókat tooautomatically nyomon követésére eltolás, és információt partícióazonosító. Vannak azonban olyan helyzetekben, amelyekben érdemes lehet toouse hello rugalmasságot hello core Event Hubs könyvtár tooreceive események.

#### <a name="create-a-receiver"></a>Fogadó létrehozása
Fogadók kapcsolt toospecific partíciók, ezért a rendezés tooreceive összes esemény eseményközpontban, toocreate több példánya lesz szüksége. Általánosan fogalmazva akkor célszerű tooget hello Partícióinformációk programozottan, hello partíciók azonosítóit rögzített megadás helyett. A rendezés toodo, használhatja a hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metódust.

```csharp
// Create a list tookeep track of hello receivers
var receivers = new List<PartitionReceiver>();
// Use hello eventHubClient created above tooget hello runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over hello resulting partition ids
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create hello receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add hello receiver toohello list
    receivers.Add(receiver);
}
```

Mivel az események soha nem törlődnek az eseményközpontok (és csak lejár), toospecify hello megfelelő kiindulási pontot kell. hello következő példa bemutatja kombináció.

```csharp
// partitionId is assumed toocome from GetRuntimeInformationAsync()

// Using hello constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Egy esemény felhasználása
```csharp
// Receive a maximum of 100 messages in this call tooReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop tooprocess
    foreach (var ehEvent in ehEvents)
    {
        // Decode hello byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load hello custom property that we set in hello send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a>Esemény processzor állomás API-k
Ezen API-k olyan rugalmassági tooworker folyamatok, elérhetetlenné válhatnak a, partíciók keresztül elérhető munkavállalók elosztásával.

```csharp
// Checkpointing is done within hello SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{event hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes of hello Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

hello az alábbiakban látható egy minta végrehajtásának hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
        return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
        Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
        return Task.CompletedTask;
    }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
        Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
        return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (var eventData in messages)
        {
            var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
            Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
        }

        return context.CheckpointAsync();
    }
}
```

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs-forgatókönyvekkel toolearn látogasson el ezeket a hivatkozásokat:

* [Mi az Azure Event Hubs?](event-hubs-what-is-event-hubs.md)
* [Rendelkezésre álló Event Hubs API-k](event-hubs-api-overview.md)

hello .NET API hivatkozásokat itt áll:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)