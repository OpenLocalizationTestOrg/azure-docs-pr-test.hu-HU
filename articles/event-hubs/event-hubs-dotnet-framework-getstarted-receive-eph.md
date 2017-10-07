---
title: "Azure Event Hubs használatával aaaReceive események a .NET-keretrendszer hello |} Microsoft Docs"
description: "Az oktatóanyag tooreceive események kövesse az Azure Event Hubs hello .NET-keretrendszer használatával."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/12/2017
ms.author: sethm
ms.openlocfilehash: a88c3feeacfd3de9622dbb86e25222e861750204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-azure-event-hubs-using-hello-net-framework"></a>Események fogadása az Azure Event Hubs hello .NET-keretrendszer használatával

## <a name="introduction"></a>Bevezetés

Az Event Hubs szolgáltatás a csatlakoztatott eszközökről és alkalmazásokból származó nagy mennyiségű eseményadatot dolgoz fel (telemetria). Miután összegyűjtötte az adatokat az Event Hubs, egy tárolási fürt használatával hello adatok tárolására, vagy átalakíthatja egy valós idejű elemzési szolgáltató használatával. A felügyeleti teendők központjaként esemény és -feldolgozási képesség, modern alkalmazásarchitektúráknak, beleértve az eszközök internetes hálózatát (IoT) hello nyilvános kulcsokra épülő.

Ez az oktatóanyag bemutatja, hogyan toowrite egy .NET-keretrendszer Konzolalkalmazás fogad üzeneteket hello használata az eseményközpontban lévő  **[Event Processor Host][EventProcessorHost]**. toosend események hello .NET-keretrendszer használatával, lásd: hello [hello .NET-keretrendszer használatával tooAzure Event Hubs üzenetküldési](event-hubs-dotnet-framework-getstarted-send.md) a következő cikket, vagy kattintson a hello küldő megfelelő nyelvi hello bal oldali tábla tartalmát.

Hello [Event Processor Host] [ EventProcessorHost] egy .NET-osztály, amely leegyszerűsíti az események fogadását az event hubs kezeli az állandó ellenőrzőpontokat és párhuzamos fogadásokat az adott event hubs eseményközpontokból. Hello segítségével [Event Processor Host][Event Processor Host], akkor is feloszthatja az eseményeket több fogadóra, akkor is, ha különböző csomópontokon üzemelnek. A példa bemutatja, hogyan toouse hello [Event Processor Host] [ EventProcessorHost] egyetlen fogadóhoz. Hello [esemény feldolgozása kibővítési] [ Scale out Event Processing with Event Hubs] bemutatja hogyan minta toouse hello [Event Processor Host] [ EventProcessorHost] több fogadóval.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:

* [Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com). Ebben az oktatóanyagban hello képernyőképek használja a Visual Studio 2017.
* Aktív Azure-fiók. Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs-névtér és eseményközpont létrehozása

első lépés hello toouse hello [Azure-portálon](https://portal.azure.com) névtere a toocreate írja be az Event Hubs, és hello felügyeleti hitelesítő adatok az alkalmazás kell toocommunicate hello event hubs az beszerzése. toocreate névtér és az event hubs eljárással hello a [Ez a cikk](event-hubs-create.md), majd folytassa a hello ebben az oktatóanyagban a lépéseket követve.

## <a name="create-an-azure-storage-account"></a>Azure Storage-fiók létrehozása

toouse hello [Event Processor Host][EventProcessorHost], rendelkeznie kell egy [Azure Storage-fiók][Azure Storage account]:

1. Jelentkezzen be toohello [Azure-portálon][Azure portal], és kattintson **új** hello: bal felső részén üdvözlő képernyőt.
2. Kattintson a **Tárolás**, majd a **Tárfiók** elemre.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. A hello **storage-fiók létrehozása** panelen adja meg a hello storage-fiók nevét. Adja meg az Azure-előfizetéssel, erőforráscsoportot és helyet melyik toocreate hello erőforrást. Ezt követően kattintson a **Create** (Létrehozás) gombra.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Hello listában tárfiókok kattintson az újonnan létrehozott tárfiók hello.
5. Hello storage-fiók panelen kattintson **hívóbetűk**. Másolja a hello értékének **key1** toouse az oktatóanyag későbbi részében.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>Fogadó konzolalkalmazás létrehozása

1. A Visual Studióban hozzon létre egy új Visual C# Asztalialkalmazás-projektet hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **fogadó**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. A Megoldáskezelőben kattintson a jobb gombbal hello **fogadó** projektre, és kattintson a **NuGet-csomagok kezelése megoldáshoz**.
3. Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    A Visual Studio letöltések, telepíti, és hozzáad egy hivatkozást toohello [Azure Service Bus-Eseményközpont - EventProcessorHost NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), elemet minden függőségével.
4. Kattintson a jobb gombbal hello **fogadó** projektre, kattintson **Hozzáadás**, és kattintson a **osztály**. Hello új osztály neve **SimpleEventProcessor**, és kattintson a **Hozzáadás** toocreate hello osztály.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. Adja hozzá a következő utasításokat hello SimpleEventProcessor.cs fájl elejéhez hello hello:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  Ezután helyettesítse be a következő hello törzsét hello osztály kódját hello:
    
  ```csharp
  class SimpleEventProcessor : IEventProcessor
  {
    Stopwatch checkpointStopWatch;
    
    async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
    
    Task IEventProcessor.OpenAsync(PartitionContext context)
    {
        Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
        this.checkpointStopWatch = new Stopwatch();
        this.checkpointStopWatch.Start();
        return Task.FromResult<object>(null);
    }
    
    async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData eventData in messages)
        {
            string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
            Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                context.Lease.PartitionId, data));
        }
    
        //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
        if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
        {
            await context.CheckpointAsync();
            this.checkpointStopWatch.Restart();
        }
    }
  }
  ```
    
  Ez az osztály hívja hello **EventProcessorHost** tooprocess események hello eseményközpont kapott. Hello `SimpleEventProcessor` osztály stoppert tooperiodically hívás hello ellenőrzőpont módszert használ a hello **EventProcessorHost** a környezetben. A feldolgozási biztosítja, hogy hello fogadó újraindul, ha elveszíti legfeljebb öt percen feldolgozási munka.
6. A hello **Program** osztály, adja hozzá a következő hello `using` hello fájl hello tetején utasítást:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  Ezután cserélje le a hello `Main` metódus a hello `Program` hello a következő kódot az osztály, és hello eseményközpont neveként és hello névtérszintű kapcsolati karakterlánc, amikor a korábban mentett és a storage-fiók és a kulcs a hello hello korábbi szakaszokban. 
    
  ```csharp
  static void Main(string[] args)
  {
    string eventHubConnectionString = "{Event Hubs namespace connection string}";
    string eventHubName = "{Event Hub name}";
    string storageAccountName = "{storage account name}";
    string storageAccountKey = "{storage account key}";
    string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
    string eventProcessorHostName = Guid.NewGuid().ToString();
    EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
    Console.WriteLine("Registering EventProcessor...");
    var options = new EventProcessorOptions();
    options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
    eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
    Console.WriteLine("Receiving. Press enter key toostop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.
  
Gratulálunk! Az eseményközpontok hello Event Processor Host használatával most kapott üzenetek.


> [!NOTE]
> Ez az oktatóprogram az [EventProcessorHost][EventProcessorHost] egyetlen példányát használja. tooincrease átviteli, javasoljuk, hogy több példányát futtatja [EventProcessorHost][EventProcessorHost], ahogy az hello [méretezett események feldolgozásával,] [out esemény feldolgozása méretezett] minta. Ezekben az esetekben hello különböző példányok automatikusan koordinálja egymással tooload egyenleg hello fogadott események. Ha azt szeretné, hogy több fogadóval tooeach folyamat *összes* események, hello hello kell használnia **ConsumerGroup** fogalom. Események fogadása különböző gépek, amikor előfordulhat, hogy hasznos toospecify neveit [EventProcessorHost] [ EventProcessorHost] példányok alapuló hello gépeken (vagy szerepkörökön) azokat. Ezek a témakörök kapcsolatos további információkért lásd: hello [Event Hubs – áttekintés] [ Event Hubs overview] és hello [Event Hubs programozási útmutató] [ Event Hubs Programming Guide] témaköröket.
> 
> 

## <a name="next-steps"></a>Következő lépések

Most, hogy létrehozott egy működő alkalmazást, amely létrehoz egy eseményközpontot, és adatokat küld és fogad, többet is megtudhat a következő hivatkozások hello érhetők el:

* [Event Processor Host][Event Processor Host]
* [Event Hubs – áttekintés][Event Hubs overview]
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com
