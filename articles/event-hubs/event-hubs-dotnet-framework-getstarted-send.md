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
# <a name="send-events-tooazure-event-hubs-using-hello-net-framework"></a><span data-ttu-id="e8e37-103">Események küldése tooAzure Event Hubs hello .NET-keretrendszer használatával</span><span class="sxs-lookup"><span data-stu-id="e8e37-103">Send events tooAzure Event Hubs using hello .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="e8e37-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e8e37-104">Introduction</span></span>

<span data-ttu-id="e8e37-105">Az Event Hubs szolgáltatás a csatlakoztatott eszközökről és alkalmazásokból származó nagy mennyiségű eseményadatot dolgoz fel (telemetria).</span><span class="sxs-lookup"><span data-stu-id="e8e37-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="e8e37-106">Miután összegyűjtötte az adatokat az Event Hubs, egy tárolási fürt használatával hello adatok tárolására, vagy átalakíthatja egy valós idejű elemzési szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="e8e37-106">After you collect data into Event Hubs, you can store hello data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="e8e37-107">A felügyeleti teendők központjaként esemény és -feldolgozási képesség, modern alkalmazásarchitektúráknak, beleértve az eszközök internetes hálózatát (IoT) hello nyilvános kulcsokra épülő.</span><span class="sxs-lookup"><span data-stu-id="e8e37-107">This large-scale event collection and processing capability is a key component of modern application architectures including hello Internet of Things (IoT).</span></span>

<span data-ttu-id="e8e37-108">Ez az oktatóanyag bemutatja, hogyan toouse hello [Azure-portálon](https://portal.azure.com) toocreate egy eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="e8e37-108">This tutorial shows how toouse hello [Azure portal](https://portal.azure.com) toocreate an event hub.</span></span> <span data-ttu-id="e8e37-109">Azt is bemutatja, hogyan toosend események tooan eseményközpontba egy C# használatával írt Konzolalkalmazás használatával hello .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="e8e37-109">It also shows how toosend events tooan event hub using a console application written in C# using hello .NET Framework.</span></span> <span data-ttu-id="e8e37-110">tooreceive események hello .NET-keretrendszer használatával, lásd: hello [hello .NET-keretrendszer használatával események fogadásához](event-hubs-dotnet-framework-getstarted-receive-eph.md) a következő cikket, vagy kattintson a hello fogadó megfelelő nyelvi hello bal oldali tábla tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e8e37-110">tooreceive events using hello .NET Framework, see hello [Receive events using hello .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) article, or click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="e8e37-111">toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e8e37-111">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="e8e37-112">[Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="e8e37-112">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="e8e37-113">Ebben az oktatóanyagban hello képernyőképek használja a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e8e37-113">hello screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="e8e37-114">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="e8e37-114">An active Azure account.</span></span> <span data-ttu-id="e8e37-115">Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="e8e37-115">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="e8e37-116">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="e8e37-116">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="e8e37-117">Event Hubs-névtér és eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8e37-117">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="e8e37-118">első lépés hello toouse hello [Azure-portálon](https://portal.azure.com) névtere a toocreate írja be az Event Hubs, és hello felügyeleti hitelesítő adatok az alkalmazás kell toocommunicate hello event hubs az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="e8e37-118">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace of type Event Hubs, and obtain hello management credentials your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="e8e37-119">toocreate névtér és az event hubs eljárással hello a [Ez a cikk](event-hubs-create.md), majd folytassa a hello ebben az oktatóanyagban a lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="e8e37-119">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), then proceed with hello following steps in this tutorial.</span></span>

## <a name="create-a-sender-console-application"></a><span data-ttu-id="e8e37-120">Küldő konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8e37-120">Create a sender console application</span></span>

<span data-ttu-id="e8e37-121">Ebben a szakaszban egy által küldött események tooyour eseményközpont Windows konzolalkalmazást fog írni.</span><span class="sxs-lookup"><span data-stu-id="e8e37-121">In this section, you'll write a Windows console app that sends events tooyour event hub.</span></span>

1. <span data-ttu-id="e8e37-122">A Visual Studióban hozzon létre egy új Visual C# Asztalialkalmazás-projektet hello segítségével **Konzolalkalmazás** projektsablon.</span><span class="sxs-lookup"><span data-stu-id="e8e37-122">In Visual Studio, create a new Visual C# Desktop App project using hello **Console Application** project template.</span></span> <span data-ttu-id="e8e37-123">Név hello projekt **küldő**.</span><span class="sxs-lookup"><span data-stu-id="e8e37-123">Name hello project **Sender**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. <span data-ttu-id="e8e37-124">A Megoldáskezelőben kattintson a jobb gombbal hello **küldő** projektre, és kattintson a **NuGet-csomagok kezelése megoldáshoz**.</span><span class="sxs-lookup"><span data-stu-id="e8e37-124">In Solution Explorer, right-click hello **Sender** project, and then click **Manage NuGet Packages for Solution**.</span></span> 
3. <span data-ttu-id="e8e37-125">Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="e8e37-125">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="e8e37-126">Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="e8e37-126">Click **Install**, and accept hello terms of use.</span></span> 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    <span data-ttu-id="e8e37-127">A Visual Studio letöltések, telepíti, és hozzáad egy hivatkozást toohello [Azure Service Bus library NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="e8e37-127">Visual Studio downloads, installs, and adds a reference toohello [Azure Service Bus library NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus).</span></span>
4. <span data-ttu-id="e8e37-128">Adja hozzá a következő hello `using` hello hello tetején utasítások **Program.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="e8e37-128">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. <span data-ttu-id="e8e37-129">Adja hozzá a következő mezők toohello hello **Program** osztályhoz, lecserélve a helyőrző értékeket hello hello hello hello előző szakaszban létrehozott eseményközpont, és a korábban mentett hello névtérszintű kapcsolati karakterlánc nevét.</span><span class="sxs-lookup"><span data-stu-id="e8e37-129">Add hello following fields toohello **Program** class, substituting hello placeholder values with hello name of hello event hub you created in hello previous section, and hello namespace-level connection string you saved previously.</span></span>
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. <span data-ttu-id="e8e37-130">Adja hozzá a következő metódus toohello hello **Program** osztály:</span><span class="sxs-lookup"><span data-stu-id="e8e37-130">Add hello following method toohello **Program** class:</span></span>
   
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
   
  <span data-ttu-id="e8e37-131">Ez a metódus folyamatosan küldi az események tooyour eseményközpont 200-ms késleltetéssel.</span><span class="sxs-lookup"><span data-stu-id="e8e37-131">This method continuously sends events tooyour event hub with a 200-ms delay.</span></span>
7. <span data-ttu-id="e8e37-132">Végül adja hozzá a következő sorokat toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="e8e37-132">Finally, add hello following lines toohello **Main** method:</span></span>
   
  ```csharp
  Console.WriteLine("Press Ctrl-C toostop hello sender process");
  Console.WriteLine("Press Enter toostart now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. <span data-ttu-id="e8e37-133">Hello program futtatása, és győződjön meg arról, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="e8e37-133">Run hello program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="e8e37-134">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="e8e37-134">Congratulations!</span></span> <span data-ttu-id="e8e37-135">Most elküldött üzenetek tooan eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="e8e37-135">You have now sent messages tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8e37-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8e37-136">Next steps</span></span>
<span data-ttu-id="e8e37-137">Most, hogy létrehozott egy működő alkalmazást, amely létrehoz egy eseményközpontot, és adatokat küld, továbbléphet a következő forgatókönyvek toohello:</span><span class="sxs-lookup"><span data-stu-id="e8e37-137">Now that you've built a working application that creates an event hub and sends data, you can move on toohello following scenarios:</span></span>

* [<span data-ttu-id="e8e37-138">Event Processor Host hello eseményeket fogadni</span><span class="sxs-lookup"><span data-stu-id="e8e37-138">Receive events using hello Event Processor Host</span></span>](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [<span data-ttu-id="e8e37-139">Event Processor Host-referencia</span><span class="sxs-lookup"><span data-stu-id="e8e37-139">Event Processor Host reference</span></span>](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [<span data-ttu-id="e8e37-140">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e8e37-140">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

