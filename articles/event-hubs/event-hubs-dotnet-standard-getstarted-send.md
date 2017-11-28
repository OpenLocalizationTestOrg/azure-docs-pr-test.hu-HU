---
title: "aaaSend események tooAzure Event Hubs használatával a .NET-szabvány |} Microsoft Docs"
description: "Ismerkedés a .NET-szabvány tooEvent hubok események küldése"
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
ms.openlocfilehash: caa9747a8a72aa8e7aea1348a116f6e4b406460e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-sending-messages-tooazure-event-hubs-in-net-standard"></a><span data-ttu-id="5572e-103">Ismerkedés a .NET-szabvány tooAzure Event Hubs üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="5572e-103">Get started sending messages tooAzure Event Hubs in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="5572e-104">Ez a minta érhető el a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span><span class="sxs-lookup"><span data-stu-id="5572e-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span></span>

<span data-ttu-id="5572e-105">Ez az oktatóanyag bemutatja, hogyan toowrite a .NET Core console alkalmazást, amely olyan üzenetek tooan eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="5572e-105">This tutorial shows how toowrite a .NET Core console application that sends a set of messages tooan event hub.</span></span> <span data-ttu-id="5572e-106">Hello futtatása [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) megoldás,-, váltja fel hello `EhConnectionString` és `EhEntityPath` a event hub értékű karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="5572e-106">You can run hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) solution as-is, replacing hello `EhConnectionString` and `EhEntityPath` strings with your event hub values.</span></span> <span data-ttu-id="5572e-107">Vagy követheti hello lépéseit az oktatóanyag toocreate saját.</span><span class="sxs-lookup"><span data-stu-id="5572e-107">Or you can follow hello steps in this tutorial toocreate your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5572e-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5572e-108">Prerequisites</span></span>

* <span data-ttu-id="5572e-109">[A Microsoft Visual Studio 2015-öt vagy 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="5572e-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="5572e-110">Ebben az oktatóanyag a Visual Studio 2017 hello példák, de a Visual Studio 2015-öt is támogatott.</span><span class="sxs-lookup"><span data-stu-id="5572e-110">hello examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="5572e-111">[A .NET core Visual Studio 2015-öt vagy 2017 eszközök](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="5572e-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="5572e-112">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5572e-112">An Azure subscription.</span></span>
* <span data-ttu-id="5572e-113">Az event hub névtér.</span><span class="sxs-lookup"><span data-stu-id="5572e-113">An event hub namespace.</span></span>

<span data-ttu-id="5572e-114">toosend üzenetek tooan eseményközpontba egy C#-konzolalkalmazást a Visual Studio toowrite használjuk.</span><span class="sxs-lookup"><span data-stu-id="5572e-114">toosend messages tooan event hub, we will use Visual Studio toowrite a C# console application.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="5572e-115">Event Hubs-névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="5572e-115">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="5572e-116">hello első lépése az toouse hello [Azure-portálon](https://portal.azure.com) toocreate hub hello eseménytípus, a névtér és hello felügyeleti hitelesítő adatok, hogy az alkalmazás kell toocommunicate hello eseményközpont az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="5572e-116">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace for hello event hub type, and obtain hello management credentials that your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="5572e-117">toocreate névtér és az eseményközpontok felé, hajtsa végre a hello eljárását [Ez a cikk](event-hubs-create.md), majd folytassa a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="5572e-117">toocreate a namespace and an event hub, follow hello procedure in [this article](event-hubs-create.md), and then proceed with hello following steps.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="5572e-118">Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5572e-118">Create a console application</span></span>

<span data-ttu-id="5572e-119">Indítsa el a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="5572e-119">Start Visual Studio.</span></span> <span data-ttu-id="5572e-120">A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5572e-120">From hello **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="5572e-121">A .NET Core Konzolalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5572e-121">Create a .NET Core console application.</span></span>

![Új projekt][1]

## <a name="add-hello-event-hubs-nuget-package"></a><span data-ttu-id="5572e-123">Hello Event Hubs NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5572e-123">Add hello Event Hubs NuGet package</span></span>

<span data-ttu-id="5572e-124">Adja hozzá a hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET-szabvány library NuGet csomag tooyour projekt ezeket a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="5572e-124">Add hello [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard library NuGet package tooyour project by following these steps:</span></span> 

1. <span data-ttu-id="5572e-125">Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="5572e-125">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="5572e-126">Kattintson a hello **Tallózás** fülre, majd keresse meg a "Microsoft.Azure.EventHubs" és a select hello **Microsoft.Azure.EventHubs** csomag.</span><span class="sxs-lookup"><span data-stu-id="5572e-126">Click hello **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select hello **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="5572e-127">Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="5572e-127">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>

## <a name="write-some-code-toosend-messages-toohello-event-hub"></a><span data-ttu-id="5572e-128">Néhány kódot toosend üzenetek toohello eseményközpont írása</span><span class="sxs-lookup"><span data-stu-id="5572e-128">Write some code toosend messages toohello event hub</span></span>

1. <span data-ttu-id="5572e-129">Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítások toohello.</span><span class="sxs-lookup"><span data-stu-id="5572e-129">Add hello following `using` statements toohello top of hello Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="5572e-130">Adja hozzá a állandók toohello `Program` osztály hello Event Hubs kapcsolati karakterlánc és egyéb entitások az elérési úthoz (egyéni esemény központ neve).</span><span class="sxs-lookup"><span data-stu-id="5572e-130">Add constants toohello `Program` class for hello Event Hubs connection string and entity path (individual event hub name).</span></span> <span data-ttu-id="5572e-131">Szögletes zárójelbe hello helyőrzőket cserélje le hello eseményközpont létrehozásakor beszerzett hello megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="5572e-131">Replace hello placeholders in brackets with hello proper values that were obtained when creating hello event hub.</span></span>

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. <span data-ttu-id="5572e-132">Nevű új módszer `MainAsync` toohello `Program` osztály, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5572e-132">Add a new method named `MainAsync` toohello `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from hello connection string, and sets hello EntityPath.
        // Typically, hello connection string should have hello entity path in it, but for hello sake of this simple scenario
        // we are using hello connection string from hello namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
    }
    ```

4. <span data-ttu-id="5572e-133">Nevű új módszer `SendMessagesToEventHub` toohello `Program` osztály, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5572e-133">Add a new method named `SendMessagesToEventHub` toohello `Program` class, as follows:</span></span>

    ```csharp
    // Creates an event hub client and sends 100 messages toohello event hub.
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

5. <span data-ttu-id="5572e-134">Adja hozzá a következő kód toohello hello `Main` metódus a hello `Program` osztály.</span><span class="sxs-lookup"><span data-stu-id="5572e-134">Add hello following code toohello `Main` method in hello `Program` class.</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   <span data-ttu-id="5572e-135">A Program.cs fájlnak így kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="5572e-135">Here is what your Program.cs should look like.</span></span>

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
                // Creates an EventHubsConnectionStringBuilder object from hello connection string, and sets hello EntityPath.
                // Typically, hello connection string should have hello entity path in it, but for hello sake of this simple scenario
                // we are using hello connection string from hello namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER tooexit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages toohello event hub.
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

6. <span data-ttu-id="5572e-136">Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="5572e-136">Run hello program, and ensure that there are no errors.</span></span>

<span data-ttu-id="5572e-137">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="5572e-137">Congratulations!</span></span> <span data-ttu-id="5572e-138">Most elküldött üzenetek tooan eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="5572e-138">You have now sent messages tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5572e-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5572e-139">Next steps</span></span>
<span data-ttu-id="5572e-140">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="5572e-140">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="5572e-141">Események fogadása az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5572e-141">Receive events from Event Hubs</span></span>](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [<span data-ttu-id="5572e-142">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="5572e-142">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="5572e-143">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="5572e-143">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="5572e-144">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="5572e-144">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
