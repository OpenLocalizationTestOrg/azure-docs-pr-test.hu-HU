---
title: "Események fogadása az Azure Event Hubs használatával a .NET-szabvány |} Microsoft Docs"
description: "Ismerkedés az EventProcessorHost üzenetek fogadása a .NET-szabvány"
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
ms.openlocfilehash: cc62792dad0284f9514664795fdfb32e94a85943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a><span data-ttu-id="09ab4-103">Az Event Processor Host üzenetek fogadása a .NET-szabvány első lépései</span><span class="sxs-lookup"><span data-stu-id="09ab4-103">Get started receiving messages with the Event Processor Host in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="09ab4-104">Ez a minta érhető el a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span><span class="sxs-lookup"><span data-stu-id="09ab4-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span></span>

<span data-ttu-id="09ab4-105">Ez az oktatóanyag bemutatja, hogyan írhat egy .NET Core-konzolalkalmazást, amely üzeneteket fogad az eseményközpontban lévő használatával **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-105">This tutorial shows how to write a .NET Core console application that receives messages from an event hub by using **EventProcessorHost**.</span></span> <span data-ttu-id="09ab4-106">Futtathatja a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) megoldás szerint-van, a karakterláncok cseréje a event hub és a tárolási fiók értékek.</span><span class="sxs-lookup"><span data-stu-id="09ab4-106">You can run the [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) solution as-is, replacing the strings with your event hub and storage account values.</span></span> <span data-ttu-id="09ab4-107">Vagy a lépésekkel ebben az oktatóanyagban saját.</span><span class="sxs-lookup"><span data-stu-id="09ab4-107">Or you can follow the steps in this tutorial to create your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09ab4-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="09ab4-108">Prerequisites</span></span>

* <span data-ttu-id="09ab4-109">[A Microsoft Visual Studio 2015-öt vagy 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="09ab4-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="09ab4-110">A példák a Visual Studio 2017 oktatóanyag használja, de a Visual Studio 2015-öt is támogatott.</span><span class="sxs-lookup"><span data-stu-id="09ab4-110">The examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="09ab4-111">[A .NET core Visual Studio 2015-öt vagy 2017 eszközök](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="09ab4-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="09ab4-112">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="09ab4-112">An Azure subscription.</span></span>
* <span data-ttu-id="09ab4-113">Az Azure Event Hubs névtér.</span><span class="sxs-lookup"><span data-stu-id="09ab4-113">An Azure Event Hubs namespace.</span></span>
* <span data-ttu-id="09ab4-114">Egy Azure-tárfiók.</span><span class="sxs-lookup"><span data-stu-id="09ab4-114">An Azure storage account.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="09ab4-115">Event Hubs-névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="09ab4-115">Create an Event Hubs namespace and an event hub</span></span>  

<span data-ttu-id="09ab4-116">Az első lépés az, hogy használja a [Azure-portálon](https://portal.azure.com) az Event Hubs típus névtér létrehozása, és szerezze be a felügyeleti hitelesítő adatokat az alkalmazásban az event hubs folytatott kommunikációhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="09ab4-116">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace for the Event Hubs type, and obtain the management credentials that your application needs to communicate with the event hub.</span></span> <span data-ttu-id="09ab4-117">Egy névtér és az event hub létrehozásához kövesse a [Ez a cikk](event-hubs-create.md), majd folytassa a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="09ab4-117">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), and then proceed with the following steps.</span></span>  

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="09ab4-118">Azure-tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="09ab4-118">Create an Azure storage account</span></span>  

1. <span data-ttu-id="09ab4-119">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="09ab4-119">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="09ab4-120">A portál bal oldali navigációs ablaktábláján kattintson **új**, kattintson a **tárolási**, és kattintson a **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-120">In the left navigation pane of the portal, click **New**, click **Storage**, and then click **Storage Account**.</span></span>  
3. <span data-ttu-id="09ab4-121">Töltse ki a mezőket a storage-fiók panelen, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-121">Complete the fields in the storage account blade, and then click **Create**.</span></span>

    ![Storage-fiók létrehozása][1]

4. <span data-ttu-id="09ab4-123">Miután meggyőződött arról a **központi telepítések sikeres** üzenetet, kattintson az új tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="09ab4-123">After you see the **Deployments Succeeded** message, click the name of the new storage account.</span></span> <span data-ttu-id="09ab4-124">Az a **Essentials** panelen kattintson a **Blobok**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-124">In the **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="09ab4-125">Ha a **Blob szolgáltatás** panel nyílik meg, kattintson a **+ tároló** tetején.</span><span class="sxs-lookup"><span data-stu-id="09ab4-125">When the **Blob service** blade opens, click **+ Container** at the top.</span></span> <span data-ttu-id="09ab4-126">Nevezze el a tárolót, és zárja be a **Blob szolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="09ab4-126">Give the container a name, and then close the **Blob service** blade.</span></span>  
5. <span data-ttu-id="09ab4-127">Kattintson a **hívóbetűk** a bal oldali panelen és a példány nevét és a tároló, a tárfiók, értékének **key1**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-127">Click **Access keys** in the left blade and copy the name of the storage container, the storage account, and the value of **key1**.</span></span> <span data-ttu-id="09ab4-128">Ezeket az értékeket a Jegyzettömb vagy más ideiglenes helyre mentse.</span><span class="sxs-lookup"><span data-stu-id="09ab4-128">Save these values to Notepad or some other temporary location.</span></span>  

## <a name="create-a-console-application"></a><span data-ttu-id="09ab4-129">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="09ab4-129">Create a console application</span></span>

<span data-ttu-id="09ab4-130">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="09ab4-130">Start Visual Studio.</span></span> <span data-ttu-id="09ab4-131">Kattintson a **File** (Fájl) menüben a **New** (Új), majd a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="09ab4-131">From the **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="09ab4-132">A .NET Core Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="09ab4-132">Create a .NET Core console application.</span></span>

![Új projekt][2]

## <a name="add-the-event-hubs-nuget-package"></a><span data-ttu-id="09ab4-134">Az Event Hubs NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="09ab4-134">Add the Event Hubs NuGet package</span></span>

<span data-ttu-id="09ab4-135">Adja hozzá a [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) és [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET-szabvány library NuGet-csomagok a projekthez az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="09ab4-135">Add the [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) and [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet packages to your project by following these steps:</span></span> 

1. <span data-ttu-id="09ab4-136">Kattintson a jobb gombbal az újonnan létrehozott projektre, és válassza a **Manage Nuget Packages** (NuGet-csomagok kezelése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="09ab4-136">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="09ab4-137">Kattintson a **Tallózás** fülre, majd keresse meg a "Microsoft.Azure.EventHubs", és válassza ki a **Microsoft.Azure.EventHubs** csomag.</span><span class="sxs-lookup"><span data-stu-id="09ab4-137">Click the **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select the **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="09ab4-138">Kattintson a **Telepítés** gombra a telepítés befejezéséhez, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="09ab4-138">Click **Install** to complete the installation, then close this dialog box.</span></span>
3. <span data-ttu-id="09ab4-139">Ismételje meg az 1. és 2, és telepítse a **Microsoft.Azure.EventHubs.Processor** csomag.</span><span class="sxs-lookup"><span data-stu-id="09ab4-139">Repeat steps 1 and 2, and install the **Microsoft.Azure.EventHubs.Processor** package.</span></span>

## <a name="implement-the-ieventprocessor-interface"></a><span data-ttu-id="09ab4-140">A IEventProcessor illesztőfelület megvalósítása</span><span class="sxs-lookup"><span data-stu-id="09ab4-140">Implement the IEventProcessor interface</span></span>

1. <span data-ttu-id="09ab4-141">A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **Hozzáadás**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-141">In Solution Explorer, right-click the project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="09ab4-142">Az új osztály neve **SimpleEventProcessor**.</span><span class="sxs-lookup"><span data-stu-id="09ab4-142">Name the new class **SimpleEventProcessor**.</span></span>

2. <span data-ttu-id="09ab4-143">Nyissa meg a SimpleEventProcessor.cs fájl, és adja hozzá a következő `using` utasítást, hogy a fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="09ab4-143">Open the SimpleEventProcessor.cs file and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. <span data-ttu-id="09ab4-144">Alkalmazzon a `IEventProcessor` felületet.</span><span class="sxs-lookup"><span data-stu-id="09ab4-144">Implement the `IEventProcessor` interface.</span></span> <span data-ttu-id="09ab4-145">Cserélje le a teljes tartalmát a `SimpleEventProcessor` osztály az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="09ab4-145">Replace the entire contents of the `SimpleEventProcessor` class with the following code:</span></span>

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

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a><span data-ttu-id="09ab4-146">A fő konzolon metódus írása, amely a SimpleEventProcessor osztály üzenetek fogadásához használt</span><span class="sxs-lookup"><span data-stu-id="09ab4-146">Write a main console method that uses the SimpleEventProcessor class to receive messages</span></span>

1. <span data-ttu-id="09ab4-147">Adja hozzá az alábbi `using` utasításokat a Program.cs fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="09ab4-147">Add the following `using` statements to the top of the Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="09ab4-148">Adja hozzá a állandók a `Program` osztály a event hub kapcsolati karakterlánc, a eseményközpont neve, a tárfiók tároló neve, a tárfiók neve és a tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="09ab4-148">Add constants to the `Program` class for the event hub connection string, event hub name, storage account container name, storage account name, and storage account key.</span></span> <span data-ttu-id="09ab4-149">Adja hozzá a következő kódot, a helyőrzők cseréje a hozzájuk tartozó értékek.</span><span class="sxs-lookup"><span data-stu-id="09ab4-149">Add the following code, replacing the placeholders with their corresponding values.</span></span>

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. <span data-ttu-id="09ab4-150">Nevű új módszer `MainAsync` számára a `Program` osztály, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="09ab4-150">Add a new method named `MainAsync` to the `Program` class, as follows:</span></span>

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

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. <span data-ttu-id="09ab4-151">Adja hozzá a következő kódsort a `Main` módszert:</span><span class="sxs-lookup"><span data-stu-id="09ab4-151">Add the following line of code to the `Main` method:</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    <span data-ttu-id="09ab4-152">A Program.cs fájlnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="09ab4-152">Here is what your Program.cs file should look like:</span></span>

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

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. <span data-ttu-id="09ab4-153">Futtassa a programot, és ellenőrizze, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="09ab4-153">Run the program, and ensure that there are no errors.</span></span>

<span data-ttu-id="09ab4-154">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="09ab4-154">Congratulations!</span></span> <span data-ttu-id="09ab4-155">Most kapott üzenetek eseményközpontban az Event Processor Host használatával.</span><span class="sxs-lookup"><span data-stu-id="09ab4-155">You have now received messages from an event hub by using the Event Processor Host.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09ab4-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09ab4-156">Next steps</span></span>
<span data-ttu-id="09ab4-157">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="09ab4-157">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="09ab4-158">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="09ab4-158">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="09ab4-159">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="09ab4-159">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="09ab4-160">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="09ab4-160">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
