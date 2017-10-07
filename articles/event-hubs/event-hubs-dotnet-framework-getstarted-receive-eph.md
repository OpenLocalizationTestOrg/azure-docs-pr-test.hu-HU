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
# <a name="receive-events-from-azure-event-hubs-using-hello-net-framework"></a><span data-ttu-id="05233-103">Események fogadása az Azure Event Hubs hello .NET-keretrendszer használatával</span><span class="sxs-lookup"><span data-stu-id="05233-103">Receive events from Azure Event Hubs using hello .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="05233-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="05233-104">Introduction</span></span>

<span data-ttu-id="05233-105">Az Event Hubs szolgáltatás a csatlakoztatott eszközökről és alkalmazásokból származó nagy mennyiségű eseményadatot dolgoz fel (telemetria).</span><span class="sxs-lookup"><span data-stu-id="05233-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="05233-106">Miután összegyűjtötte az adatokat az Event Hubs, egy tárolási fürt használatával hello adatok tárolására, vagy átalakíthatja egy valós idejű elemzési szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="05233-106">After you collect data into Event Hubs, you can store hello data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="05233-107">A felügyeleti teendők központjaként esemény és -feldolgozási képesség, modern alkalmazásarchitektúráknak, beleértve az eszközök internetes hálózatát (IoT) hello nyilvános kulcsokra épülő.</span><span class="sxs-lookup"><span data-stu-id="05233-107">This large-scale event collection and processing capability is a key component of modern application architectures including hello Internet of Things (IoT).</span></span>

<span data-ttu-id="05233-108">Ez az oktatóanyag bemutatja, hogyan toowrite egy .NET-keretrendszer Konzolalkalmazás fogad üzeneteket hello használata az eseményközpontban lévő  **[Event Processor Host][EventProcessorHost]**.</span><span class="sxs-lookup"><span data-stu-id="05233-108">This tutorial shows how toowrite a .NET Framework console application that receives messages from an event hub using hello **[Event Processor Host][EventProcessorHost]**.</span></span> <span data-ttu-id="05233-109">toosend események hello .NET-keretrendszer használatával, lásd: hello [hello .NET-keretrendszer használatával tooAzure Event Hubs üzenetküldési](event-hubs-dotnet-framework-getstarted-send.md) a következő cikket, vagy kattintson a hello küldő megfelelő nyelvi hello bal oldali tábla tartalmát.</span><span class="sxs-lookup"><span data-stu-id="05233-109">toosend events using hello .NET Framework, see hello [Send events tooAzure Event Hubs using hello .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) article, or click hello appropriate sending language in hello left-hand table of contents.</span></span>

<span data-ttu-id="05233-110">Hello [Event Processor Host] [ EventProcessorHost] egy .NET-osztály, amely leegyszerűsíti az események fogadását az event hubs kezeli az állandó ellenőrzőpontokat és párhuzamos fogadásokat az adott event hubs eseményközpontokból.</span><span class="sxs-lookup"><span data-stu-id="05233-110">hello [Event Processor Host][EventProcessorHost] is a .NET class that simplifies receiving events from event hubs by managing persistent checkpoints and parallel receives from those event hubs.</span></span> <span data-ttu-id="05233-111">Hello segítségével [Event Processor Host][Event Processor Host], akkor is feloszthatja az eseményeket több fogadóra, akkor is, ha különböző csomópontokon üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="05233-111">Using hello [Event Processor Host][Event Processor Host], you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="05233-112">A példa bemutatja, hogyan toouse hello [Event Processor Host] [ EventProcessorHost] egyetlen fogadóhoz.</span><span class="sxs-lookup"><span data-stu-id="05233-112">This example shows how toouse hello [Event Processor Host][EventProcessorHost] for a single receiver.</span></span> <span data-ttu-id="05233-113">Hello [esemény feldolgozása kibővítési] [ Scale out Event Processing with Event Hubs] bemutatja hogyan minta toouse hello [Event Processor Host] [ EventProcessorHost] több fogadóval.</span><span class="sxs-lookup"><span data-stu-id="05233-113">hello [Scale out event processing][Scale out Event Processing with Event Hubs] sample shows how toouse hello [Event Processor Host][EventProcessorHost] with multiple receivers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05233-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05233-114">Prerequisites</span></span>

<span data-ttu-id="05233-115">toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="05233-115">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="05233-116">[Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="05233-116">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="05233-117">Ebben az oktatóanyagban hello képernyőképek használja a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="05233-117">hello screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="05233-118">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="05233-118">An active Azure account.</span></span> <span data-ttu-id="05233-119">Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="05233-119">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="05233-120">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="05233-120">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="05233-121">Event Hubs-névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="05233-121">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="05233-122">első lépés hello toouse hello [Azure-portálon](https://portal.azure.com) névtere a toocreate írja be az Event Hubs, és hello felügyeleti hitelesítő adatok az alkalmazás kell toocommunicate hello event hubs az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="05233-122">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace of type Event Hubs, and obtain hello management credentials your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="05233-123">toocreate névtér és az event hubs eljárással hello a [Ez a cikk](event-hubs-create.md), majd folytassa a hello ebben az oktatóanyagban a lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="05233-123">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), then proceed with hello following steps in this tutorial.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="05233-124">Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="05233-124">Create an Azure Storage account</span></span>

<span data-ttu-id="05233-125">toouse hello [Event Processor Host][EventProcessorHost], rendelkeznie kell egy [Azure Storage-fiók][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="05233-125">toouse hello [Event Processor Host][EventProcessorHost], you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="05233-126">Jelentkezzen be toohello [Azure-portálon][Azure portal], és kattintson **új** hello: bal felső részén üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="05233-126">Log on toohello [Azure portal][Azure portal], and click **New** at hello top left of hello screen.</span></span>
2. <span data-ttu-id="05233-127">Kattintson a **Tárolás**, majd a **Tárfiók** elemre.</span><span class="sxs-lookup"><span data-stu-id="05233-127">Click **Storage**, then click **Storage account**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. <span data-ttu-id="05233-128">A hello **storage-fiók létrehozása** panelen adja meg a hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="05233-128">In hello **Create storage account** blade, type a name for hello storage account.</span></span> <span data-ttu-id="05233-129">Adja meg az Azure-előfizetéssel, erőforráscsoportot és helyet melyik toocreate hello erőforrást.</span><span class="sxs-lookup"><span data-stu-id="05233-129">Choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> <span data-ttu-id="05233-130">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="05233-130">Then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. <span data-ttu-id="05233-131">Hello listában tárfiókok kattintson az újonnan létrehozott tárfiók hello.</span><span class="sxs-lookup"><span data-stu-id="05233-131">In hello list of storage accounts, click hello newly created storage account.</span></span>
5. <span data-ttu-id="05233-132">Hello storage-fiók panelen kattintson **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="05233-132">In hello storage account blade, click **Access keys**.</span></span> <span data-ttu-id="05233-133">Másolja a hello értékének **key1** toouse az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="05233-133">Copy hello value of **key1** toouse later in this tutorial.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a><span data-ttu-id="05233-134">Fogadó konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="05233-134">Create a receiver console application</span></span>

1. <span data-ttu-id="05233-135">A Visual Studióban hozzon létre egy új Visual C# Asztalialkalmazás-projektet hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="05233-135">In Visual Studio, create a new Visual C# Desktop App project using hello **Console  Application** project template.</span></span> <span data-ttu-id="05233-136">Név hello projekt **fogadó**.</span><span class="sxs-lookup"><span data-stu-id="05233-136">Name hello project **Receiver**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. <span data-ttu-id="05233-137">A Megoldáskezelőben kattintson a jobb gombbal hello **fogadó** projektre, és kattintson a **NuGet-csomagok kezelése megoldáshoz**.</span><span class="sxs-lookup"><span data-stu-id="05233-137">In Solution Explorer, right-click hello **Receiver** project, and then click **Manage NuGet Packages for Solution**.</span></span>
3. <span data-ttu-id="05233-138">Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span><span class="sxs-lookup"><span data-stu-id="05233-138">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span></span> <span data-ttu-id="05233-139">Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="05233-139">Click **Install**, and accept hello terms of use.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    <span data-ttu-id="05233-140">A Visual Studio letöltések, telepíti, és hozzáad egy hivatkozást toohello [Azure Service Bus-Eseményközpont - EventProcessorHost NuGet-csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), elemet minden függőségével.</span><span class="sxs-lookup"><span data-stu-id="05233-140">Visual Studio downloads, installs, and adds a reference toohello [Azure Service Bus Event Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), with all its dependencies.</span></span>
4. <span data-ttu-id="05233-141">Kattintson a jobb gombbal hello **fogadó** projektre, kattintson **Hozzáadás**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="05233-141">Right-click hello **Receiver** project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="05233-142">Hello új osztály neve **SimpleEventProcessor**, és kattintson a **Hozzáadás** toocreate hello osztály.</span><span class="sxs-lookup"><span data-stu-id="05233-142">Name hello new class **SimpleEventProcessor**, and then click **Add** toocreate hello class.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. <span data-ttu-id="05233-143">Adja hozzá a következő utasításokat hello SimpleEventProcessor.cs fájl elejéhez hello hello:</span><span class="sxs-lookup"><span data-stu-id="05233-143">Add hello following statements at hello top of hello SimpleEventProcessor.cs file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  <span data-ttu-id="05233-144">Ezután helyettesítse be a következő hello törzsét hello osztály kódját hello:</span><span class="sxs-lookup"><span data-stu-id="05233-144">Then, substitute hello following code for hello body of hello class:</span></span>
    
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
    
  <span data-ttu-id="05233-145">Ez az osztály hívja hello **EventProcessorHost** tooprocess események hello eseményközpont kapott.</span><span class="sxs-lookup"><span data-stu-id="05233-145">This class is called by hello **EventProcessorHost** tooprocess events received from hello event hub.</span></span> <span data-ttu-id="05233-146">Hello `SimpleEventProcessor` osztály stoppert tooperiodically hívás hello ellenőrzőpont módszert használ a hello **EventProcessorHost** a környezetben.</span><span class="sxs-lookup"><span data-stu-id="05233-146">hello `SimpleEventProcessor` class uses a stopwatch tooperiodically call hello checkpoint method on hello **EventProcessorHost** context.</span></span> <span data-ttu-id="05233-147">A feldolgozási biztosítja, hogy hello fogadó újraindul, ha elveszíti legfeljebb öt percen feldolgozási munka.</span><span class="sxs-lookup"><span data-stu-id="05233-147">This processing ensures that, if hello receiver is restarted, it loses no more than five minutes of processing work.</span></span>
6. <span data-ttu-id="05233-148">A hello **Program** osztály, adja hozzá a következő hello `using` hello fájl hello tetején utasítást:</span><span class="sxs-lookup"><span data-stu-id="05233-148">In hello **Program** class, add hello following `using` statement at hello top of hello file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  <span data-ttu-id="05233-149">Ezután cserélje le a hello `Main` metódus a hello `Program` hello a következő kódot az osztály, és hello eseményközpont neveként és hello névtérszintű kapcsolati karakterlánc, amikor a korábban mentett és a storage-fiók és a kulcs a hello hello korábbi szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="05233-149">Then, replace hello `Main` method in hello `Program` class with hello following code, substituting hello event hub name and hello namespace-level connection string that you saved previously, and hello storage account and key that you copied in hello previous sections.</span></span> 
    
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

7. <span data-ttu-id="05233-150">Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="05233-150">Run hello program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="05233-151">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="05233-151">Congratulations!</span></span> <span data-ttu-id="05233-152">Az eseményközpontok hello Event Processor Host használatával most kapott üzenetek.</span><span class="sxs-lookup"><span data-stu-id="05233-152">You have now received messages from an event hub using hello Event Processor Host.</span></span>


> [!NOTE]
> <span data-ttu-id="05233-153">Ez az oktatóprogram az [EventProcessorHost][EventProcessorHost] egyetlen példányát használja.</span><span class="sxs-lookup"><span data-stu-id="05233-153">This tutorial uses a single instance of [EventProcessorHost][EventProcessorHost].</span></span> <span data-ttu-id="05233-154">tooincrease átviteli, javasoljuk, hogy több példányát futtatja [EventProcessorHost][EventProcessorHost], ahogy az hello [méretezett események feldolgozásával,] [out esemény feldolgozása méretezett] minta.</span><span class="sxs-lookup"><span data-stu-id="05233-154">tooincrease throughput, it is recommended that you run multiple instances of [EventProcessorHost][EventProcessorHost], as shown in hello [Scaled out event processing][Scaled out event processing] sample.</span></span> <span data-ttu-id="05233-155">Ezekben az esetekben hello különböző példányok automatikusan koordinálja egymással tooload egyenleg hello fogadott események.</span><span class="sxs-lookup"><span data-stu-id="05233-155">In those cases, hello various instances automatically coordinate with each other tooload balance hello received events.</span></span> <span data-ttu-id="05233-156">Ha azt szeretné, hogy több fogadóval tooeach folyamat *összes* események, hello hello kell használnia **ConsumerGroup** fogalom.</span><span class="sxs-lookup"><span data-stu-id="05233-156">If you want multiple receivers tooeach process *all* hello events, you must use hello **ConsumerGroup** concept.</span></span> <span data-ttu-id="05233-157">Események fogadása különböző gépek, amikor előfordulhat, hogy hasznos toospecify neveit [EventProcessorHost] [ EventProcessorHost] példányok alapuló hello gépeken (vagy szerepkörökön) azokat.</span><span class="sxs-lookup"><span data-stu-id="05233-157">When receiving events from different machines, it might be useful toospecify names for [EventProcessorHost][EventProcessorHost] instances based on hello machines (or roles) in which they are deployed.</span></span> <span data-ttu-id="05233-158">Ezek a témakörök kapcsolatos további információkért lásd: hello [Event Hubs – áttekintés] [ Event Hubs overview] és hello [Event Hubs programozási útmutató] [ Event Hubs Programming Guide] témaköröket.</span><span class="sxs-lookup"><span data-stu-id="05233-158">For more information about these topics, see hello [Event Hubs overview][Event Hubs overview] and hello [Event Hubs programming guide][Event Hubs Programming Guide] topics.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="05233-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05233-159">Next steps</span></span>

<span data-ttu-id="05233-160">Most, hogy létrehozott egy működő alkalmazást, amely létrehoz egy eseményközpontot, és adatokat küld és fogad, többet is megtudhat a következő hivatkozások hello érhetők el:</span><span class="sxs-lookup"><span data-stu-id="05233-160">Now that you've built a working application that creates an event hub and sends and receives data, you can learn more by visiting hello following links:</span></span>

* <span data-ttu-id="05233-161">[Event Processor Host][Event Processor Host]</span><span class="sxs-lookup"><span data-stu-id="05233-161">[Event Processor Host][Event Processor Host]</span></span>
* <span data-ttu-id="05233-162">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="05233-162">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="05233-163">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="05233-163">Event Hubs FAQ</span></span>](event-hubs-faq.md)

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
