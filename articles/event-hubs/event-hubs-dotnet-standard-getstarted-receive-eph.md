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
# <a name="get-started-receiving-messages-with-hello-event-processor-host-in-net-standard"></a><span data-ttu-id="a9e87-103">Ismerkedés a .NET-szabvány hello Event Processor Host üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="a9e87-103">Get started receiving messages with hello Event Processor Host in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="a9e87-104">Ez a minta érhető el a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span><span class="sxs-lookup"><span data-stu-id="a9e87-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span></span>

<span data-ttu-id="a9e87-105">Ez az oktatóanyag bemutatja, hogyan toowrite a .NET Core Konzolalkalmazás, amely üzeneteket fogad az eseményközpontban lévő használatával **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-105">This tutorial shows how toowrite a .NET Core console application that receives messages from an event hub by using **EventProcessorHost**.</span></span> <span data-ttu-id="a9e87-106">Hello futtatása [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) megoldás szerint-, lecseréli hello karakterláncok a event hub és a tárolási fiók értékek.</span><span class="sxs-lookup"><span data-stu-id="a9e87-106">You can run hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) solution as-is, replacing hello strings with your event hub and storage account values.</span></span> <span data-ttu-id="a9e87-107">Vagy követheti hello lépéseit az oktatóanyag toocreate saját.</span><span class="sxs-lookup"><span data-stu-id="a9e87-107">Or you can follow hello steps in this tutorial toocreate your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9e87-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a9e87-108">Prerequisites</span></span>

* <span data-ttu-id="a9e87-109">[A Microsoft Visual Studio 2015-öt vagy 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="a9e87-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="a9e87-110">Ebben az oktatóanyag a Visual Studio 2017 hello példák, de a Visual Studio 2015-öt is támogatott.</span><span class="sxs-lookup"><span data-stu-id="a9e87-110">hello examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="a9e87-111">[A .NET core Visual Studio 2015-öt vagy 2017 eszközök](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="a9e87-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="a9e87-112">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a9e87-112">An Azure subscription.</span></span>
* <span data-ttu-id="a9e87-113">Az Azure Event Hubs névtér.</span><span class="sxs-lookup"><span data-stu-id="a9e87-113">An Azure Event Hubs namespace.</span></span>
* <span data-ttu-id="a9e87-114">Egy Azure-tárfiók.</span><span class="sxs-lookup"><span data-stu-id="a9e87-114">An Azure storage account.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="a9e87-115">Event Hubs-névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9e87-115">Create an Event Hubs namespace and an event hub</span></span>  

<span data-ttu-id="a9e87-116">hello első lépése az toouse hello [Azure-portálon](https://portal.azure.com) toocreate hello Event Hubs egy névterét adja meg, majd hello felügyeleti hitelesítő adatok, hogy az alkalmazás kell toocommunicate hello eseményközpont az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="a9e87-116">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace for hello Event Hubs type, and obtain hello management credentials that your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="a9e87-117">toocreate névtér és az event hubs eljárással hello a [Ez a cikk](event-hubs-create.md), majd folytassa a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="a9e87-117">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), and then proceed with hello following steps.</span></span>  

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="a9e87-118">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9e87-118">Create an Azure storage account</span></span>  

1. <span data-ttu-id="a9e87-119">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a9e87-119">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="a9e87-120">Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, kattintson a **tárolási**, és kattintson a **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-120">In hello left navigation pane of hello portal, click **New**, click **Storage**, and then click **Storage Account**.</span></span>  
3. <span data-ttu-id="a9e87-121">Fejezze be a hello mezők hello storage-fiók panelen, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-121">Complete hello fields in hello storage account blade, and then click **Create**.</span></span>

    ![Storage-fiók létrehozása][1]

4. <span data-ttu-id="a9e87-123">Miután meggyőződött arról, hogy hello **központi telepítések sikeres** üzenetet, kattintson az új tárfiók hello hello nevére.</span><span class="sxs-lookup"><span data-stu-id="a9e87-123">After you see hello **Deployments Succeeded** message, click hello name of hello new storage account.</span></span> <span data-ttu-id="a9e87-124">A hello **Essentials** panelen kattintson a **Blobok**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-124">In hello **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="a9e87-125">Ha hello **Blob szolgáltatás** panel nyílik meg, kattintson a **+ tároló** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="a9e87-125">When hello **Blob service** blade opens, click **+ Container** at hello top.</span></span> <span data-ttu-id="a9e87-126">Nevezze el hello tárolót, és zárja be a hello **Blob szolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="a9e87-126">Give hello container a name, and then close hello **Blob service** blade.</span></span>  
5. <span data-ttu-id="a9e87-127">Kattintson a **hívóbetűk** hello bal oldali panelen, és másolja hello nevű hello tároló hello tárfiók és hello értékének **key1**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-127">Click **Access keys** in hello left blade and copy hello name of hello storage container, hello storage account, and hello value of **key1**.</span></span> <span data-ttu-id="a9e87-128">Ezen értékek tooNotepad vagy más ideiglenes helyre mentse.</span><span class="sxs-lookup"><span data-stu-id="a9e87-128">Save these values tooNotepad or some other temporary location.</span></span>  

## <a name="create-a-console-application"></a><span data-ttu-id="a9e87-129">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9e87-129">Create a console application</span></span>

<span data-ttu-id="a9e87-130">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="a9e87-130">Start Visual Studio.</span></span> <span data-ttu-id="a9e87-131">A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-131">From hello **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="a9e87-132">A .NET Core Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a9e87-132">Create a .NET Core console application.</span></span>

![Új projekt][2]

## <a name="add-hello-event-hubs-nuget-package"></a><span data-ttu-id="a9e87-134">Hello Event Hubs NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9e87-134">Add hello Event Hubs NuGet package</span></span>

<span data-ttu-id="a9e87-135">Adja hozzá a hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) és [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET-szabvány library NuGet csomagok tooyour projekt ezeket a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="a9e87-135">Add hello [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) and [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet packages tooyour project by following these steps:</span></span> 

1. <span data-ttu-id="a9e87-136">Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-136">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="a9e87-137">Kattintson a hello **Tallózás** fülre, majd keresse meg a "Microsoft.Azure.EventHubs" és a select hello **Microsoft.Azure.EventHubs** csomag.</span><span class="sxs-lookup"><span data-stu-id="a9e87-137">Click hello **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select hello **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="a9e87-138">Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="a9e87-138">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
3. <span data-ttu-id="a9e87-139">Ismételje meg az 1. és 2, és telepítse a hello **Microsoft.Azure.EventHubs.Processor** csomag.</span><span class="sxs-lookup"><span data-stu-id="a9e87-139">Repeat steps 1 and 2, and install hello **Microsoft.Azure.EventHubs.Processor** package.</span></span>

## <a name="implement-hello-ieventprocessor-interface"></a><span data-ttu-id="a9e87-140">Hello IEventProcessor illesztőfelület megvalósítása</span><span class="sxs-lookup"><span data-stu-id="a9e87-140">Implement hello IEventProcessor interface</span></span>

1. <span data-ttu-id="a9e87-141">A Megoldáskezelőben kattintson a jobb gombbal hello projektben kattintson **Hozzáadás**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-141">In Solution Explorer, right-click hello project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="a9e87-142">Hello új osztály neve **SimpleEventProcessor**.</span><span class="sxs-lookup"><span data-stu-id="a9e87-142">Name hello new class **SimpleEventProcessor**.</span></span>

2. <span data-ttu-id="a9e87-143">Nyissa meg a hello SimpleEventProcessor.cs fájl, és adja hozzá a következő hello `using` utasítások toohello felső hello fájl.</span><span class="sxs-lookup"><span data-stu-id="a9e87-143">Open hello SimpleEventProcessor.cs file and add hello following `using` statements toohello top of hello file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. <span data-ttu-id="a9e87-144">Alkalmazzon hello `IEventProcessor` felületet.</span><span class="sxs-lookup"><span data-stu-id="a9e87-144">Implement hello `IEventProcessor` interface.</span></span> <span data-ttu-id="a9e87-145">Cserélje le a teljes tartalma hello hello `SimpleEventProcessor` hello kód a következő osztályra:</span><span class="sxs-lookup"><span data-stu-id="a9e87-145">Replace hello entire contents of hello `SimpleEventProcessor` class with hello following code:</span></span>

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

## <a name="write-a-main-console-method-that-uses-hello-simpleeventprocessor-class-tooreceive-messages"></a><span data-ttu-id="a9e87-146">A fő konzolon metódus írása, amely hello SimpleEventProcessor osztály tooreceive üzeneteket</span><span class="sxs-lookup"><span data-stu-id="a9e87-146">Write a main console method that uses hello SimpleEventProcessor class tooreceive messages</span></span>

1. <span data-ttu-id="a9e87-147">Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítások toohello.</span><span class="sxs-lookup"><span data-stu-id="a9e87-147">Add hello following `using` statements toohello top of hello Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="a9e87-148">Adja hozzá a állandók toohello `Program` osztály hello event hub kapcsolati karakterlánc, eseményközpont neve, a tárfiók tároló neve, a tárfiók nevének és tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="a9e87-148">Add constants toohello `Program` class for hello event hub connection string, event hub name, storage account container name, storage account name, and storage account key.</span></span> <span data-ttu-id="a9e87-149">Adja hozzá a következő kódot, hello helyőrzők cseréje a hozzájuk tartozó értékek hello.</span><span class="sxs-lookup"><span data-stu-id="a9e87-149">Add hello following code, replacing hello placeholders with their corresponding values.</span></span>

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. <span data-ttu-id="a9e87-150">Nevű új módszer `MainAsync` toohello `Program` osztály, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a9e87-150">Add a new method named `MainAsync` toohello `Program` class, as follows:</span></span>

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

3. <span data-ttu-id="a9e87-151">Adja hozzá a következő kód toohello üzletági hello `Main` módszert:</span><span class="sxs-lookup"><span data-stu-id="a9e87-151">Add hello following line of code toohello `Main` method:</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    <span data-ttu-id="a9e87-152">A Program.cs fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="a9e87-152">Here is what your Program.cs file should look like:</span></span>

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

4. <span data-ttu-id="a9e87-153">Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="a9e87-153">Run hello program, and ensure that there are no errors.</span></span>

<span data-ttu-id="a9e87-154">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a9e87-154">Congratulations!</span></span> <span data-ttu-id="a9e87-155">Az eseményközpontok hello Event Processor Host használatával most kapott üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a9e87-155">You have now received messages from an event hub by using hello Event Processor Host.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9e87-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9e87-156">Next steps</span></span>
<span data-ttu-id="a9e87-157">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="a9e87-157">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="a9e87-158">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="a9e87-158">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="a9e87-159">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9e87-159">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="a9e87-160">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a9e87-160">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
