---
title: "aaaGet Azure Service Bus-üzenetsorok használatába |} Microsoft Docs"
description: "Írjon egy Service Bus üzenetkezelési sorokat használó C# konzolalkalmazást."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.openlocfilehash: eaa362ab0eabd2427977398c1deab5dc00105ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-queues"></a>Bevezetés a Service Bus által kezelt üzenetsorok használatába
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Az oktatóanyag célja
Ez az oktatóanyag ismerteti a lépéseket követve hello:

1. Hozzon létre egy Service Bus-névtér, hello Azure-portál használatával.
2. Hozzon létre egy Service Bus-üzenetsorba, hello Azure-portál használatával.
3. A konzol alkalmazás toosend egy üzenetet ír.
4. A konzol alkalmazás tooreceive hello hello előző lépésben küldött üzenetek írni.

## <a name="prerequisites"></a>Előfeltételek
1. [Visual Studio 2015 vagy újabb](http://www.visualstudio.com). az oktatóanyagban szereplő példák hello használja a Visual Studio 2017.
2. Azure-előfizetés.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Hello Azure-portál használatával névtér létrehozása
Ha már létrehozott egy Service Bus üzenetkezelés névtér, jump toohello [hello Azure-portál használatával várólista létrehozása](#2-create-a-queue-using-the-azure-portal) szakasz.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-hello-azure-portal"></a>2. Létrehoz egy sort hello Azure-portál használatával
Ha már létrehozott egy Service Bus-üzenetsorba, jump toohello [küldési üzenetek toohello sor](#3-send-messages-to-the-queue) szakasz.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-toohello-queue"></a>3. Üzenetek toohello várólista küldése
toosend üzenetek toohello várólista, azt írni egy C#-konzolalkalmazást a Visual Studio használatával.

### <a name="create-a-console-application"></a>Konzolalkalmazás létrehozása

Indítsa el a Visual Studiót, majd hozzon létre egy új **Konzolalkalmazás (.NET-keretrendszer)** projektet.

### <a name="add-hello-service-bus-nuget-package"></a>Hello Service Bus NuGet-csomag hozzáadása
1. Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.
2. Hello kattintson **Tallózás** lapra, keressen rá **Microsoft Azure Service Bus**, majd válassza ki a hello **WindowsAzure.ServiceBus** elemet. Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.
   
    ![NuGet-csomag kiválasztása][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-queue"></a>Néhány kódot toosend toohello üzenet-várólista írása
1. Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Adja hozzá a következő kód toohello hello `Main` metódust. Set hello `connectionString` változó toohello kapcsolati karakterlánc, amikor kapja meg, amikor hello névtér létrehozása, és állítsa be `queueName` toohello várólista hello várólista létrehozásakor használt név.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    A Program.cs fájlnak így kell kinéznie.
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Hello program futtatása, és ellenőrizze a hello Azure-portálon: kattintson a sor hello névtér nevére hello **áttekintése** panelen. hello várólista **Essentials** panel jelenik meg. Figyelje meg, hogy hello **aktív üzenetek száma** értéket kell 1. Minden egyes futtatásakor hello küldő alkalmazás köszönőüzenetei, lekérdezése nélkül ez az érték 1 nő. Vegye figyelembe, hogy hello hello várólista aktuális méretét növeli-e minden alkalommal hello alkalmazás bővíti ezenkívül toohello üzenet-várólista.
   
      ![Üzenet mérete][queue-message]

## <a name="4-receive-messages-from-hello-queue"></a>4. Üzenetek fogadása hello üzenetsorból

1. tooreceive köszönőüzenetei csak küldte, hozzon létre egy új konzolalkalmazást, és adja hozzá egy hivatkozást toohello Service Bus NuGet-csomagot, hasonló toohello előző küldő alkalmazás.
2. Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Adja hozzá a következő kód toohello hello `Main` metódust. Set hello `connectionString` változó toohello kapcsolati karakterlánc lett kapja meg, amikor hello névtér létrehozása, és állítsa be `queueName` toohello várólista hello várólista létrehozásakor használt név.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    A Program.cs fájlnak így kell kinéznie:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER tooexit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Hello program futtatása, és hello portal ismételt ellenőrzéséhez. Figyelje meg, hogy hello **aktív üzenetek száma** és **aktuális** értékek most: 0.
   
    ![Várólista hossza][queue-message-receive]

Gratulálunk! Ezzel létrehozott egy üzenetsort, illetve küldött és fogadott is üzenetet.

## <a name="next-steps"></a>Következő lépések

Tekintse meg a [minták a GitHub-tárházban](https://github.com/Azure/azure-service-bus/tree/master/samples) , amelyek bemutatják, hogy néhány hello fejlettebb funkciókat a Service Bus üzenetkezelés.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
