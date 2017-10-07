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
# <a name="get-started-sending-messages-tooazure-event-hubs-in-net-standard"></a>Ismerkedés a .NET-szabvány tooAzure Event Hubs üzenetek küldése

> [!NOTE]
> Ez a minta érhető el a [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Ez az oktatóanyag bemutatja, hogyan toowrite a .NET Core console alkalmazást, amely olyan üzenetek tooan eseményközpontot. Hello futtatása [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) megoldás,-, váltja fel hello `EhConnectionString` és `EhEntityPath` a event hub értékű karakterláncokat. Vagy követheti hello lépéseit az oktatóanyag toocreate saját.

## <a name="prerequisites"></a>Előfeltételek

* [A Microsoft Visual Studio 2015-öt vagy 2017](http://www.visualstudio.com). Ebben az oktatóanyag a Visual Studio 2017 hello példák, de a Visual Studio 2015-öt is támogatott.
* [A .NET core Visual Studio 2015-öt vagy 2017 eszközök](http://www.microsoft.com/net/core).
* Azure-előfizetés.
* Az event hub névtér.

toosend üzenetek tooan eseményközpontba egy C#-konzolalkalmazást a Visual Studio toowrite használjuk.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs-névtér és eseményközpont létrehozása

hello első lépése az toouse hello [Azure-portálon](https://portal.azure.com) toocreate hub hello eseménytípus, a névtér és hello felügyeleti hitelesítő adatok, hogy az alkalmazás kell toocommunicate hello eseményközpont az beszerzése. toocreate névtér és az eseményközpontok felé, hajtsa végre a hello eljárását [Ez a cikk](event-hubs-create.md), majd folytassa a következő lépéseket hello.

## <a name="create-a-console-application"></a>Konzolalkalmazás létrehozása

Indítsa el a Visual Studiót. A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**. A .NET Core Konzolalkalmazás létrehozása.

![Új projekt][1]

## <a name="add-hello-event-hubs-nuget-package"></a>Hello Event Hubs NuGet-csomag hozzáadása

Adja hozzá a hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET-szabvány library NuGet csomag tooyour projekt ezeket a lépéseket követve: 

1. Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.
2. Kattintson a hello **Tallózás** fülre, majd keresse meg a "Microsoft.Azure.EventHubs" és a select hello **Microsoft.Azure.EventHubs** csomag. Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.

## <a name="write-some-code-toosend-messages-toohello-event-hub"></a>Néhány kódot toosend üzenetek toohello eseményközpont írása

1. Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítások toohello.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Adja hozzá a állandók toohello `Program` osztály hello Event Hubs kapcsolati karakterlánc és egyéb entitások az elérési úthoz (egyéni esemény központ neve). Szögletes zárójelbe hello helyőrzőket cserélje le hello eseményközpont létrehozásakor beszerzett hello megfelelő értékeket.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Nevű új módszer `MainAsync` toohello `Program` osztály, az alábbiak szerint:

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

4. Nevű új módszer `SendMessagesToEventHub` toohello `Program` osztály, az alábbiak szerint:

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

5. Adja hozzá a következő kód toohello hello `Main` metódus a hello `Program` osztály.

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   A Program.cs fájlnak így kell kinéznie.

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

6. Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.

Gratulálunk! Most elküldött üzenetek tooan eseményközpontot.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Események fogadása az Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
