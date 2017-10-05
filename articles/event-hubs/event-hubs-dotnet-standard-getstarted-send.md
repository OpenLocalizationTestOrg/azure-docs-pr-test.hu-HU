---
title: "Események küldése az Azure Event Hubs használatával a .NET-szabvány |} Microsoft Docs"
description: "Események küldése az Event Hubs .NET-szabvány az első lépések"
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
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a><span data-ttu-id="a1c00-103">Ismerkedés az Azure Event Hubs a .NET-szabvány üzenetküldésre</span><span class="sxs-lookup"><span data-stu-id="a1c00-103">Get started sending messages to Azure Event Hubs in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="a1c00-104">Ez a minta érhető el a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span><span class="sxs-lookup"><span data-stu-id="a1c00-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span></span>

<span data-ttu-id="a1c00-105">Ez az oktatóanyag bemutatja, hogyan írhat egy olyan konzolalkalmazást a .NET Core által küldött üzenetek készleteit az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="a1c00-105">This tutorial shows how to write a .NET Core console application that sends a set of messages to an event hub.</span></span> <span data-ttu-id="a1c00-106">Futtathatja a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) megoldás,-, váltja fel a `EhConnectionString` és `EhEntityPath` a event hub értékű karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="a1c00-106">You can run the [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) solution as-is, replacing the `EhConnectionString` and `EhEntityPath` strings with your event hub values.</span></span> <span data-ttu-id="a1c00-107">Vagy a lépésekkel ebben az oktatóanyagban saját.</span><span class="sxs-lookup"><span data-stu-id="a1c00-107">Or you can follow the steps in this tutorial to create your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1c00-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a1c00-108">Prerequisites</span></span>

* <span data-ttu-id="a1c00-109">[A Microsoft Visual Studio 2015-öt vagy 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="a1c00-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="a1c00-110">A példák a Visual Studio 2017 oktatóanyag használja, de a Visual Studio 2015-öt is támogatott.</span><span class="sxs-lookup"><span data-stu-id="a1c00-110">The examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="a1c00-111">[A .NET core Visual Studio 2015-öt vagy 2017 eszközök](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="a1c00-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="a1c00-112">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a1c00-112">An Azure subscription.</span></span>
* <span data-ttu-id="a1c00-113">Az event hub névtér.</span><span class="sxs-lookup"><span data-stu-id="a1c00-113">An event hub namespace.</span></span>

<span data-ttu-id="a1c00-114">Üzenetek küldése egy eseményközpontot, a írása C# konzolalkalmazást a Visual Studio használjuk.</span><span class="sxs-lookup"><span data-stu-id="a1c00-114">To send messages to an event hub, we will use Visual Studio to write a C# console application.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="a1c00-115">Event Hubs-névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1c00-115">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="a1c00-116">Az első lépés az, hogy használja a [Azure-portálon](https://portal.azure.com) hub eseménytípushoz névtér létrehozása, és szerezze be a felügyeleti hitelesítő adatokat az alkalmazásban az event hubs folytatott kommunikációhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1c00-116">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace for the event hub type, and obtain the management credentials that your application needs to communicate with the event hub.</span></span> <span data-ttu-id="a1c00-117">A névtér és az eseményközpontok létrehozásához kövesse a [Ez a cikk](event-hubs-create.md), majd folytassa a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a1c00-117">To create a namespace and an event hub, follow the procedure in [this article](event-hubs-create.md), and then proceed with the following steps.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="a1c00-118">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1c00-118">Create a console application</span></span>

<span data-ttu-id="a1c00-119">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="a1c00-119">Start Visual Studio.</span></span> <span data-ttu-id="a1c00-120">Kattintson a **File** (Fájl) menüben a **New** (Új), majd a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="a1c00-120">From the **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="a1c00-121">A .NET Core Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a1c00-121">Create a .NET Core console application.</span></span>

![Új projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a><span data-ttu-id="a1c00-123">Az Event Hubs NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a1c00-123">Add the Event Hubs NuGet package</span></span>

<span data-ttu-id="a1c00-124">Adja hozzá a [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET-szabvány library NuGet-csomagot a projekthez az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a1c00-124">Add the [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard library NuGet package to your project by following these steps:</span></span> 

1. <span data-ttu-id="a1c00-125">Kattintson a jobb gombbal az újonnan létrehozott projektre, és válassza a **Manage Nuget Packages** (NuGet-csomagok kezelése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a1c00-125">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="a1c00-126">Kattintson a **Tallózás** fülre, majd keresse meg a "Microsoft.Azure.EventHubs", és válassza ki a **Microsoft.Azure.EventHubs** csomag.</span><span class="sxs-lookup"><span data-stu-id="a1c00-126">Click the **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select the **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="a1c00-127">Kattintson a **Telepítés** gombra a telepítés befejezéséhez, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="a1c00-127">Click **Install** to complete the installation, then close this dialog box.</span></span>

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a><span data-ttu-id="a1c00-128">Kódírást az üzenetek küldése az event hubs</span><span class="sxs-lookup"><span data-stu-id="a1c00-128">Write some code to send messages to the event hub</span></span>

1. <span data-ttu-id="a1c00-129">Adja hozzá az alábbi `using` utasításokat a Program.cs fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="a1c00-129">Add the following `using` statements to the top of the Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="a1c00-130">Adja hozzá a állandók a `Program` osztály az Event Hubs kapcsolati karakterlánc és egyéb entitások elérési utat (egyéni esemény központ neve).</span><span class="sxs-lookup"><span data-stu-id="a1c00-130">Add constants to the `Program` class for the Event Hubs connection string and entity path (individual event hub name).</span></span> <span data-ttu-id="a1c00-131">Cserélje le a helyőrzőket zárójelek közé a megfelelő értékeket az eseményközpont létrehozásakor beszerzett.</span><span class="sxs-lookup"><span data-stu-id="a1c00-131">Replace the placeholders in brackets with the proper values that were obtained when creating the event hub.</span></span>

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. <span data-ttu-id="a1c00-132">Nevű új módszer `MainAsync` számára a `Program` osztály, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a1c00-132">Add a new method named `MainAsync` to the `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. <span data-ttu-id="a1c00-133">Nevű új módszer `SendMessagesToEventHub` számára a `Program` osztály, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a1c00-133">Add a new method named `SendMessagesToEventHub` to the `Program` class, as follows:</span></span>

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. <span data-ttu-id="a1c00-134">Adja hozzá a következő kódot a `Main` metódust a `Program` osztály.</span><span class="sxs-lookup"><span data-stu-id="a1c00-134">Add the following code to the `Main` method in the `Program` class.</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   <span data-ttu-id="a1c00-135">A Program.cs fájlnak így kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="a1c00-135">Here is what your Program.cs should look like.</span></span>

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. <span data-ttu-id="a1c00-136">Futtassa a programot, és ellenőrizze, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="a1c00-136">Run the program, and ensure that there are no errors.</span></span>

<span data-ttu-id="a1c00-137">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a1c00-137">Congratulations!</span></span> <span data-ttu-id="a1c00-138">Üzeneteket küldött egy eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="a1c00-138">You have now sent messages to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a1c00-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1c00-139">Next steps</span></span>
<span data-ttu-id="a1c00-140">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="a1c00-140">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="a1c00-141">Események fogadása az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a1c00-141">Receive events from Event Hubs</span></span>](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [<span data-ttu-id="a1c00-142">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="a1c00-142">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="a1c00-143">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1c00-143">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="a1c00-144">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a1c00-144">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
