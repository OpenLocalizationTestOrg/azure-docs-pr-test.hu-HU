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
# <a name="event-hubs-net-standard-api-overview"></a><span data-ttu-id="3504c-103">Event Hubs .NET-szabvány API – áttekintés</span><span class="sxs-lookup"><span data-stu-id="3504c-103">Event Hubs .NET Standard API overview</span></span>
<span data-ttu-id="3504c-104">Ez a cikk foglal össze néhányat hello kulcs Event Hubs .NET szabványos ügyfél API-k.</span><span class="sxs-lookup"><span data-stu-id="3504c-104">This article summarizes some of hello key Event Hubs .NET Standard client APIs.</span></span> <span data-ttu-id="3504c-105">Jelenleg két .NET-szabvány klienskódtárak segítségével:</span><span class="sxs-lookup"><span data-stu-id="3504c-105">There are currently two .NET Standard client libraries:</span></span>
* [<span data-ttu-id="3504c-106">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="3504c-106">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
  *  <span data-ttu-id="3504c-107">Ezt a szalagtárat biztosít minden alapvető futásidejű műveletet.</span><span class="sxs-lookup"><span data-stu-id="3504c-107">This library provides all basic runtime operations.</span></span>
* [<span data-ttu-id="3504c-108">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="3504c-108">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)
  * <span data-ttu-id="3504c-109">Ebben a könyvtárban, amely lehetővé teszi, hogy nyomon követése céljából feldolgozott események, és legegyszerűbb módja tooread hello az eseményközpontban lévő további funkciókat ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="3504c-109">This library adds additional functionality that allows for keeping track of processed events, and is hello easiest way tooread from an event hub.</span></span>

## <a name="event-hubs-client"></a><span data-ttu-id="3504c-110">Event Hubs-ügyfél</span><span class="sxs-lookup"><span data-stu-id="3504c-110">Event Hubs client</span></span>
<span data-ttu-id="3504c-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) hello toosend események használja elsődleges objektum létezik fogadók és tooget futásidejű információt.</span><span class="sxs-lookup"><span data-stu-id="3504c-111">[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) is hello primary object you use toosend events, create receivers, and tooget run-time information.</span></span> <span data-ttu-id="3504c-112">Ez az ügyfél csatolt tooa adott eseményközpontban, és létrehoz egy új kapcsolat toohello Event Hubs-végpontot.</span><span class="sxs-lookup"><span data-stu-id="3504c-112">This client is linked tooa particular event hub, and creates a new connection toohello Event Hubs endpoint.</span></span>

### <a name="create-an-event-hubs-client"></a><span data-ttu-id="3504c-113">Event Hubs-ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="3504c-113">Create an Event Hubs client</span></span>
<span data-ttu-id="3504c-114">Egy [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) objektum létrehozása egy kapcsolati karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="3504c-114">An [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) object is created from a connection string.</span></span> <span data-ttu-id="3504c-115">hello legegyszerűbb módja tooinstantiate egy új ügyfél hello a következő példa látható:</span><span class="sxs-lookup"><span data-stu-id="3504c-115">hello simplest way tooinstantiate a new client is shown in hello following example:</span></span>

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

<span data-ttu-id="3504c-116">tooprogrammatically hello kapcsolati karakterlánc szerkesztése, használhatja a hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) osztály számára pedig hello kapcsolati karakterlánc paraméterként túl[EventHubClient.CreateFromConnectionString ](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span><span class="sxs-lookup"><span data-stu-id="3504c-116">tooprogrammatically edit hello connection string, you can use hello [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) class, and pass hello connection string as a parameter too[EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).</span></span>

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a><span data-ttu-id="3504c-117">Események küldése</span><span class="sxs-lookup"><span data-stu-id="3504c-117">Send events</span></span>
<span data-ttu-id="3504c-118">toosend események tooan eseményközpont, használjon hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) osztály.</span><span class="sxs-lookup"><span data-stu-id="3504c-118">toosend events tooan event hub, use hello [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) class.</span></span> <span data-ttu-id="3504c-119">hello törzs kell lennie egy `byte` tömb, vagy egy `byte` tömb szegmens.</span><span class="sxs-lookup"><span data-stu-id="3504c-119">hello body must be a `byte` array, or a `byte` array segment.</span></span>

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a><span data-ttu-id="3504c-120">Események fogadása</span><span class="sxs-lookup"><span data-stu-id="3504c-120">Receive events</span></span>
<span data-ttu-id="3504c-121">ajánlott módja tooreceive események az Event Hubs hello használ hello [Event Processor Host](#event-processor-host-apis), biztosító funkciókat tooautomatically nyomon követésére eltolás, és információt partícióazonosító.</span><span class="sxs-lookup"><span data-stu-id="3504c-121">hello recommended way tooreceive events from Event Hubs is using hello [Event Processor Host](#event-processor-host-apis), which provides functionality tooautomatically keep track of offset, and partition information.</span></span> <span data-ttu-id="3504c-122">Vannak azonban olyan helyzetekben, amelyekben érdemes lehet toouse hello rugalmasságot hello core Event Hubs könyvtár tooreceive események.</span><span class="sxs-lookup"><span data-stu-id="3504c-122">However, there are certain situations in which you may want toouse hello flexibility of hello core Event Hubs library tooreceive events.</span></span>

#### <a name="create-a-receiver"></a><span data-ttu-id="3504c-123">Fogadó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3504c-123">Create a receiver</span></span>
<span data-ttu-id="3504c-124">Fogadók kapcsolt toospecific partíciók, ezért a rendezés tooreceive összes esemény eseményközpontban, toocreate több példánya lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="3504c-124">Receivers are tied toospecific partitions, so in order tooreceive all events in an event hub, you will need toocreate multiple instances.</span></span> <span data-ttu-id="3504c-125">Általánosan fogalmazva akkor célszerű tooget hello Partícióinformációk programozottan, hello partíciók azonosítóit rögzített megadás helyett.</span><span class="sxs-lookup"><span data-stu-id="3504c-125">Generally speaking, it is a good practice tooget hello partition information programatically, rather than hard-coding hello partition ids.</span></span> <span data-ttu-id="3504c-126">A rendezés toodo, használhatja a hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) metódust.</span><span class="sxs-lookup"><span data-stu-id="3504c-126">In order toodo so, you can use hello [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) method.</span></span>

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

<span data-ttu-id="3504c-127">Mivel az események soha nem törlődnek az eseményközpontok (és csak lejár), toospecify hello megfelelő kiindulási pontot kell.</span><span class="sxs-lookup"><span data-stu-id="3504c-127">Since events are never removed from an event hub (and only expire), you need toospecify hello proper starting point.</span></span> <span data-ttu-id="3504c-128">hello következő példa bemutatja kombináció.</span><span class="sxs-lookup"><span data-stu-id="3504c-128">hello following example shows possible combinations.</span></span>

```csharp
// partitionId is assumed toocome from GetRuntimeInformationAsync()

// Using hello constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a><span data-ttu-id="3504c-129">Egy esemény felhasználása</span><span class="sxs-lookup"><span data-stu-id="3504c-129">Consume an event</span></span>
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

## <a name="event-processor-host-apis"></a><span data-ttu-id="3504c-130">Esemény processzor állomás API-k</span><span class="sxs-lookup"><span data-stu-id="3504c-130">Event Processor Host APIs</span></span>
<span data-ttu-id="3504c-131">Ezen API-k olyan rugalmassági tooworker folyamatok, elérhetetlenné válhatnak a, partíciók keresztül elérhető munkavállalók elosztásával.</span><span class="sxs-lookup"><span data-stu-id="3504c-131">These APIs provide resiliency tooworker processes that may become unavailable, by distributing partitions across available workers.</span></span>

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

<span data-ttu-id="3504c-132">hello az alábbiakban látható egy minta végrehajtásának hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span><span class="sxs-lookup"><span data-stu-id="3504c-132">hello following is a sample implementation of hello [IEventProcessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3504c-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3504c-133">Next steps</span></span>
<span data-ttu-id="3504c-134">További információ az Event Hubs-forgatókönyvekkel toolearn látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="3504c-134">toolearn more about Event Hubs scenarios, visit these links:</span></span>

* [<span data-ttu-id="3504c-135">Mi az Azure Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="3504c-135">What is Azure Event Hubs?</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="3504c-136">Rendelkezésre álló Event Hubs API-k</span><span class="sxs-lookup"><span data-stu-id="3504c-136">Available Event Hubs apis</span></span>](event-hubs-api-overview.md)

<span data-ttu-id="3504c-137">hello .NET API hivatkozásokat itt áll:</span><span class="sxs-lookup"><span data-stu-id="3504c-137">hello .NET API references are here:</span></span>

* [<span data-ttu-id="3504c-138">Microsoft.Azure.EventHubs</span><span class="sxs-lookup"><span data-stu-id="3504c-138">Microsoft.Azure.EventHubs</span></span>](/dotnet/api/microsoft.azure.eventhubs)
* [<span data-ttu-id="3504c-139">Microsoft.Azure.EventHubs.Processor</span><span class="sxs-lookup"><span data-stu-id="3504c-139">Microsoft.Azure.EventHubs.Processor</span></span>](/dotnet/api/microsoft.azure.eventhubs.processor)