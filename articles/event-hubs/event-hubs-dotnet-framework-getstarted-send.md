---
title: ".NET-keretrendszer aaaSend események tooAzure Event Hubs használatával hello |} Microsoft Docs"
description: "Ismerkedés az események küldése tooEvent hubok hello .NET-keretrendszer használatával"
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
ms.openlocfilehash: 05514546a6094096e4a3c800db058190076de80a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-hello-net-framework"></a>Események küldése tooAzure Event Hubs hello .NET-keretrendszer használatával

## <a name="introduction"></a>Bevezetés

Az Event Hubs szolgáltatás a csatlakoztatott eszközökről és alkalmazásokból származó nagy mennyiségű eseményadatot dolgoz fel (telemetria). Miután összegyűjtötte az adatokat az Event Hubs, egy tárolási fürt használatával hello adatok tárolására, vagy átalakíthatja egy valós idejű elemzési szolgáltató használatával. A felügyeleti teendők központjaként esemény és -feldolgozási képesség, modern alkalmazásarchitektúráknak, beleértve az eszközök internetes hálózatát (IoT) hello nyilvános kulcsokra épülő.

Ez az oktatóanyag bemutatja, hogyan toouse hello [Azure-portálon](https://portal.azure.com) toocreate egy eseményközpontba. Azt is bemutatja, hogyan toosend események tooan eseményközpontba egy C# használatával írt Konzolalkalmazás használatával hello .NET-keretrendszer. tooreceive események hello .NET-keretrendszer használatával, lásd: hello [hello .NET-keretrendszer használatával események fogadásához](event-hubs-dotnet-framework-getstarted-receive-eph.md) a következő cikket, vagy kattintson a hello fogadó megfelelő nyelvi hello bal oldali tábla tartalmát.

toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:

* [Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com). Ebben az oktatóanyagban hello képernyőképek használja a Visual Studio 2017.
* Aktív Azure-fiók. Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs-névtér és eseményközpont létrehozása

első lépés hello toouse hello [Azure-portálon](https://portal.azure.com) névtere a toocreate írja be az Event Hubs, és hello felügyeleti hitelesítő adatok az alkalmazás kell toocommunicate hello event hubs az beszerzése. toocreate névtér és az event hubs eljárással hello a [Ez a cikk](event-hubs-create.md), majd folytassa a hello ebben az oktatóanyagban a lépéseket követve.

## <a name="create-a-sender-console-application"></a>Küldő konzolalkalmazás létrehozása

Ebben a szakaszban egy által küldött események tooyour eseményközpont Windows konzolalkalmazást fog írni.

1. A Visual Studióban hozzon létre egy új Visual C# Asztalialkalmazás-projektet hello segítségével **Konzolalkalmazás** projektsablon. Név hello projekt **küldő**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. A Megoldáskezelőben kattintson a jobb gombbal hello **küldő** projektre, és kattintson a **NuGet-csomagok kezelése megoldáshoz**. 
3. Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`. Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    A Visual Studio letöltések, telepíti, és hozzáad egy hivatkozást toohello [Azure Service Bus library NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus).
4. Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. Adja hozzá a következő mezők toohello hello **Program** osztályhoz, lecserélve a helyőrző értékeket hello hello hello hello előző szakaszban létrehozott eseményközpont, és a korábban mentett hello névtérszintű kapcsolati karakterlánc nevét.
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. Adja hozzá a következő metódus toohello hello **Program** osztály:
   
  ```csharp
  static void SendingRandomMessages()
  {
      var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
      while (true)
      {
          try
          {
              var message = Guid.NewGuid().ToString();
              Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
              eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
          }
          catch (Exception exception)
          {
              Console.ForegroundColor = ConsoleColor.Red;
              Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
              Console.ResetColor();
          }
   
          Thread.Sleep(200);
      }
  }
  ```
   
  Ez a metódus folyamatosan küldi az események tooyour eseményközpont 200-ms késleltetéssel.
7. Végül adja hozzá a következő sorokat toohello hello **fő** módszert:
   
  ```csharp
  Console.WriteLine("Press Ctrl-C toostop hello sender process");
  Console.WriteLine("Press Enter toostart now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.
  
Gratulálunk! Most elküldött üzenetek tooan eseményközpontot.

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozott egy működő alkalmazást, amely létrehoz egy eseményközpontot, és adatokat küld, továbbléphet a következő forgatókönyvek toohello:

* [Event Processor Host hello eseményeket fogadni](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [Event Processor Host-referencia](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

