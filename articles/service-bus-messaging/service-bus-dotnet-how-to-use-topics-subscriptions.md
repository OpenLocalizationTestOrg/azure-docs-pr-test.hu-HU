---
title: "az Azure Service Bus-üzenettémák és előfizetések lépései aaaGet |} Microsoft Docs"
description: "Írjon Service Bus-üzenettémákat és előfizetéseket használó C# konzolalkalmazást."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.openlocfilehash: 619d602599d97ecff2ded0681a383b19f1a8b7ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-topics"></a>Bevezetés a Service Bus-üzenettémák használatába

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a>Az oktatóanyag célja

Ez az oktatóanyag ismerteti a lépéseket követve hello:

1. Hozzon létre egy Service Bus-névtér, hello Azure-portál használatával.
2. Hozzon létre egy Service Bus-témakörbe, hello Azure-portál használatával.
3. Hozzon létre egy Service Bus előfizetés toothat témakör, hello Azure-portál használatával.
4. A konzol alkalmazás toosend írni egy üzenet toohello témakör.
5. A konzol alkalmazás tooreceive beírni az üzenetet, hogy hello előfizetésből.

## <a name="prerequisites"></a>Előfeltételek

1. [Visual Studio 2015 vagy újabb](http://www.visualstudio.com). az oktatóanyagban szereplő példák hello használja a Visual Studio 2017.
2. Azure-előfizetés.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Hello Azure-portál használatával névtér létrehozása

Ha már létrehozott egy Service Bus üzenetkezelés névtér, jump toohello [létrehoz egy témát, hello Azure-portál használatával](#2-create-a-topic-using-the-azure-portal) szakasz.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-hello-azure-portal"></a>2. Létrehoz egy témát, hello Azure-portál használatával

1. Jelentkezzen be toohello [Azure-portálon][azure-portal].
2. Hello portal hello bal oldali navigációs ablaktábláján kattintson **Service Bus** (Ha nem lát **Service Bus**, kattintson a **további szolgáltatások**).
3. Kattintson a hello névtérre, amelyben szeretné toocreate hello témakör. hello névtér áttekintése panel jelenik meg:
   
    ![Üzenettémakör létrehozása][createtopic1]
4. A hello **Service Bus-névtér** panelen kattintson a **témakörök**, kattintson a **Hozzáadás témakör**.
   
    ![Üzenettéma kiválasztása][createtopic2]
5. Adjon meg egy nevet a hello témakör, és törölje a jelet hello **particionálás engedélyezése** lehetőséget. Hello a többi beállítást az alapértelmezett értékükön hagyja.
   
    ![Új kiválasztása][createtopic3]
6. Hello hello panel alsó részén, kattintson a **létrehozása**.

## <a name="3-create-a-subscription-toohello-topic"></a>3. Egy előfizetés toohello téma

1. Hello portál erőforrások ablaktáblában kattintson az 1. lépésben létrehozott hello névtér, majd a 2. lépésben létrehozott hello témakör nevét.
2. Hello áttekintés ablaktábláján, a hello tetején kattintson hello plus túl Ezután jelentkezzen**előfizetés** tooadd egy előfizetés toothis témakör.

    ![Előfizetés létrehozása][createtopic4]

3. Adja meg a hello előfizetés nevét. Hello a többi beállítást az alapértelmezett értékükön hagyja.

## <a name="4-send-messages-toohello-topic"></a>4. Üzenetek toohello témakör küldése

toosend üzenetek toohello témakör azt írni egy C#-konzolalkalmazást a Visual Studio használatával.

### <a name="create-a-console-application"></a>Konzolalkalmazás létrehozása

Indítsa el a Visual Studiót, majd hozzon létre egy új **Konzolalkalmazás (.NET-keretrendszer)** projektet.

### <a name="add-hello-service-bus-nuget-package"></a>Hello Service Bus NuGet-csomag hozzáadása

1. Kattintson a jobb gombbal az újonnan létrehozott hello projektet, és válassza ki **NuGet-csomagok kezelése**.
2. Hello kattintson **Tallózás** lapra, keressen rá **Microsoft Azure Service Bus**, majd válassza ki a hello **WindowsAzure.ServiceBus** elemet. Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a párbeszédpanelt.
   
    ![NuGet-csomag kiválasztása][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-topic"></a>Néhány kódot toosend írni egy üzenet toohello témakör

1. Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Adja hozzá a következő kód toohello hello `Main` metódust. Set hello `connectionString` változó toohello kapcsolati karakterlánc, amikor kapja meg, amikor hello névtér létrehozása, és állítsa be `topicName` toohello hello témakör létrehozásakor használt név.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
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

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Hello program futtatása, és ellenőrizze a hello Azure-portálon: kattintson a téma hello névtér nevére hello **áttekintése** panelen. hello témakör **Essentials** panel jelenik meg. A hello előfizetés(ek) felsorolt közelében hello hello panel alján, figyelje meg, hogy hello **üzenetek száma** minden előfizetésben kell 1 érték. Minden alkalommal, amikor hello küldő alkalmazás futtatása nélkül hello beolvasásra (hello tovább című részben leírtak szerint), ez az érték 1-gyel növekszik. Ne feledje, hogy hello hello témakör lépésekben hello aktuális méretét **aktuális** hello értéke **Essentials** panel minden alkalommal, amikor hello alkalmazást ad hozzá egy üzenet toohello témakört/előfizetést.
   
      ![Üzenet mérete][topic-message]

## <a name="5-receive-messages-from-hello-subscription"></a>5. Üzenetek fogadása hello előfizetés

1. tooreceive üdvözlőüzenetére vagy üzenetek csak küldte, hozzon létre egy új konzolalkalmazást, és adja hozzá egy hivatkozást toohello Service Bus NuGet-csomagot, hasonló toohello előző küldő alkalmazás.
2. Adja hozzá a következő hello `using` hello Program.cs fájl elejéhez utasítás toohello.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Adja hozzá a következő kód toohello hello `Main` metódust. Set hello `connectionString` változó toohello kapcsolati karakterlánc kapja meg, amikor hello névtér létrehozása, és beállíthatja `topicName` toohello hello témakör létrehozásakor használt név.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. Hello program futtatása, és hello portal ismételt ellenőrzéséhez. Figyelje meg, hogy hello **üzenetek száma** és **aktuális** értékek most: 0.
   
    ![A témakör hossza][topic-message-receive]

Gratulálunk! Létrehozott egy üzenettémát és egy előfizetést, elküldött egy üzenetet, és fogadta is azt.

## <a name="next-steps"></a>Következő lépések

Tekintse meg a [minták a GitHub-tárházban](https://github.com/Azure/azure-service-bus/tree/master/samples) , amelyek bemutatják, hogy néhány hello fejlettebb funkciókat a Service Bus üzenetkezelés.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
