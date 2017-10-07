---
title: "az Azure Event Hubs használatával a .NET-szabvány aaaReceive események |} Microsoft Docs"
description: "Ismerkedés a .NET-szabvány hello EventProcessorHost üzenetek fogadása"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: c3983f2668ac8f65522e44a1609dfd2eed31b7d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-receiving-messages-with-hello-event-processor-host-in-net-standard"></a>Ismerkedés a .NET-szabvány hello Event Processor Host üzenetek fogadása

> [!NOTE]
> Ez a minta érhető el a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Ez az oktatóanyag bemutatja, hogyan toowrite a .NET Core Konzolalkalmazás, amely üzeneteket fogad az eseményközpontban lévő használatával **EventProcessorHost**. Hello futtatása [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) megoldás szerint-, lecseréli hello karakterláncok a event hub és a tárolási fiók értékek. Vagy követheti hello lépéseit az oktatóanyag toocreate saját.

## <a name="prerequisites"></a>Előfeltételek

* [A Microsoft Visual Studio 2015-öt vagy 2017](http://www.visualstudio.com). Ebben az oktatóanyag a Visual Studio 2017 hello példák, de a Visual Studio 2015-öt is támogatott.
* [A .NET core Visual Studio 2015-öt vagy 2017 eszközök](http://www.microsoft.com/net/core).
* Azure-előfizetés.
* Az Azure Event Hubs névtér.
* Egy Azure-tárfiók.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs-névtér és eseményközpont létrehozása  

hello első lépése az toouse hello [Azure-portálon](https://portal.azure.com) toocreate hello Event Hubs egy névterét adja meg, majd hello felügyeleti hitelesítő adatok, hogy az alkalmazás kell toocommunicate hello eseményközpont az beszerzése. toocreate névtér és az event hubs eljárással hello a [Ez a cikk](event-hubs-create.md), majd folytassa a következő lépéseket hello.  

## <a name="create-an-azure-storage-account"></a>Azure-tárfiók létrehozása  

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).  
2. Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, kattintson a **tárolási**, és kattintson a **Tárfiók**.  
3. Fejezze be a hello mezők hello storage-fiók panelen, majd kattintson a **létrehozása**.

    ![Storage-fiók létrehozása][1]

4. Miután meggyőződött arról, hogy hello **központi telepítések sikeres** üzenetet, kattintson az új tárfiók hello hello nevére. A hello **Essentials** panelen kattintson a **Blobok**. Ha hello **Blob szolgáltatás** panel nyílik meg, kattintson a **+ tároló** hello tetején. Nevezze el hello tárolót, és zárja be a hello **Blob szolgáltatás** panelen.  
5. Kattintson a **hívóbetűk** hello bal oldali panelen, és másolja hello nevű hello tároló hello tárfiók és hello értékének **key1**. Ezen értékek tooNotepad vagy más ideiglenes helyre mentse.  

## <a name="create-a-console-application"></a>Konzolalkalmazás létrehozása

Indítsa el a Visual Studiót. A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**. A .NET Core Konzolalkalmazás létrehozása.

![Új projekt][2]

## <a name="add-hello-event-hubs-nuget-package"></a>Hello Event Hubs NuGet-csomag hozzáadása

Adja hozzá a hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) és [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET-szabvány library NuGet csomagok tooyour projekt ezeket a lépéseket követve: 

1. Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.
2. Kattintson a hello **Tallózás** fülre, majd keresse meg a "Microsoft.Azure.EventHubs" és a select hello **Microsoft.Azure.EventHubs** csomag. Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.
3. Ismételje meg az 1. és 2, és telepítse a hello **Microsoft.Azure.EventHubs.Processor** csomag.

## <a name="implement-hello-ieventprocessor-interface"></a>Hello IEventProcessor illesztőfelület megvalósítása

1. A Megoldáskezelőben kattintson a jobb gombbal hello projektben kattintson **Hozzáadás**, és kattintson a **osztály**. Hello új osztály neve **SimpleEventProcessor**.

2. Nyissa meg a hello SimpleEventProcessor.cs fájl, és adja hozzá a következő hello `using` utasítások toohello felső hello fájl.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Alkalmazzon hello `IEventProcessor` felületet. Cserélje le a teljes tartalma hello hello `SimpleEventProcessor` hello kód a következő osztályra:

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

## <a name="write-a-main-console-method-that-uses-hello-simpleeventprocessor-class-tooreceive-messages"></a>A fő konzolon metódus írása, amely hello SimpleEventProcessor osztály tooreceive üzeneteket

1. Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítások toohello.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Adja hozzá a állandók toohello `Program` osztály hello event hub kapcsolati karakterlánc, eseményközpont neve, a tárfiók tároló neve, a tárfiók nevének és tárfiók kulcsa. Adja hozzá a következő kódot, hello helyőrzők cseréje a hozzájuk tartozó értékek hello.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Nevű új módszer `MainAsync` toohello `Program` osztály, az alábbiak szerint:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers hello Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER toostop worker.");
        Console.ReadLine();

        // Disposes of hello Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Adja hozzá a következő kód toohello üzletági hello `Main` módszert:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    A Program.cs fájlnak így kell kinéznie:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers hello Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER toostop worker.");
                Console.ReadLine();

                // Disposes of hello Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.

Gratulálunk! Az eseményközpontok hello Event Processor Host használatával most kapott üzenetek.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
