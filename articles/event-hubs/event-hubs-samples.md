---
title: "Azure Event Hubs-minták |} Microsoft Docs"
description: "Azure Event Hubs-minták"
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
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: ae9fbd97a1747d8f14c561f247a0973bb11fd039
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-samples"></a><span data-ttu-id="28f06-103">Event Hubs – minták</span><span class="sxs-lookup"><span data-stu-id="28f06-103">Event Hubs samples</span></span> 

<span data-ttu-id="28f06-104">Az Azure Event Hubs minták készletét bemutatja a legfontosabb jellemzők [Azure Event Hubs](/azure/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="28f06-104">The set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/).</span></span> <span data-ttu-id="28f06-105">Ez a cikk kategorizálja, és az egyes hivatkozásokkal elérhető mintákat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="28f06-105">This article categorizes and describes the samples available, with links to each.</span></span>

<span data-ttu-id="28f06-106">Az Event Hubs minták írásának időpontjában számos különböző helyeken találhatók:</span><span class="sxs-lookup"><span data-stu-id="28f06-106">At the time of this writing, Event Hubs samples are located in several different places:</span></span>

- [<span data-ttu-id="28f06-107">MSDN fejlesztői mintakódok</span><span class="sxs-lookup"><span data-stu-id="28f06-107">MSDN developer code samples</span></span>](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [<span data-ttu-id="28f06-108">GitHub</span><span class="sxs-lookup"><span data-stu-id="28f06-108">GitHub</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)

<span data-ttu-id="28f06-109">A .NET-keretrendszer különböző verzióival kapcsolatos további információkért lásd: [keretrendszerek és célok](/dotnet/articles/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="28f06-109">For more information about different versions of the .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).</span></span>

<span data-ttu-id="28f06-110">Több minta lesz hozzáadva adott idő alatt, ezért vissza ide gyakran frissítések keresése.</span><span class="sxs-lookup"><span data-stu-id="28f06-110">More samples will be added over time, so check back here frequently for updates.</span></span>

## <a name="net-standard"></a><span data-ttu-id="28f06-111">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="28f06-111">.NET Standard</span></span>

<span data-ttu-id="28f06-112">A következő minták bemutatják, hogyan lehet küldeni és fogadni az eseményeket a [Event Hubs-ügyfél](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) a a [.NET szabványos függvénytár](/dotnet/articles/standard/library).</span><span class="sxs-lookup"><span data-stu-id="28f06-112">The following samples demonstrate how to send and receive events using the [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for the [.NET Standard library](/dotnet/articles/standard/library).</span></span>

### <a name="send-events"></a><span data-ttu-id="28f06-113">Események küldése</span><span class="sxs-lookup"><span data-stu-id="28f06-113">Send events</span></span> 

<span data-ttu-id="28f06-114">A [Ismerkedés küldése](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) minta bemutatja, hogyan írása a .NET Core console alkalmazást, amely események az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="28f06-114">The [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how to write a .NET Core console application that sends events to an event hub.</span></span>

### <a name="receive-events"></a><span data-ttu-id="28f06-115">Események fogadása</span><span class="sxs-lookup"><span data-stu-id="28f06-115">Receive events</span></span> 

<span data-ttu-id="28f06-116">A [fogadását az Event Processor Host az első lépések](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) minta a .NET Core-Konzolalkalmazás, amely üzeneteket fogad egy eseményközpontot, az Event Processor Host használatával.</span><span class="sxs-lookup"><span data-stu-id="28f06-116">The [Get started receiving with the Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using the Event Processor Host.</span></span>

## <a name="net-framework"></a><span data-ttu-id="28f06-117">.NET-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="28f06-117">.NET Framework</span></span>   

<span data-ttu-id="28f06-118">Azure Event hubs célcsoport-kezelési különböző más funkciókat fogunk bemutatni, ezeket a mintákat a [.NET Framework könyvtár](/dotnet/framework/index).</span><span class="sxs-lookup"><span data-stu-id="28f06-118">These samples demonstrate various other features of Azure Event Hubs, targeting the [.NET Framework library](/dotnet/framework/index).</span></span>
 
### <a name="notify-users-of-events-received"></a><span data-ttu-id="28f06-119">Értesítse a felhasználókat a fogadott események</span><span class="sxs-lookup"><span data-stu-id="28f06-119">Notify users of events received</span></span>

<span data-ttu-id="28f06-120">A [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) minta értesíti a felhasználókat az érzékelők vagy más rendszerek érkező adatokat.</span><span class="sxs-lookup"><span data-stu-id="28f06-120">The [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.</span></span>

### <a name="get-started-with-event-hubs"></a><span data-ttu-id="28f06-121">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="28f06-121">Get started with Event Hubs</span></span> 

<span data-ttu-id="28f06-122">A [Event Hubs használatának első lépéseit](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) például létrehoz egy eseményközpontot, események küldése az eseményközpont, és segítségével események felhasználásához az Event Hubs alapvető képességeit mutatja be a [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .</span><span class="sxs-lookup"><span data-stu-id="28f06-122">The [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates the basic capabilities of Event Hubs, such as how to create an event hub, send events to an event hub, and consume events using the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>

### <a name="scale-out-event-processing"></a><span data-ttu-id="28f06-123">Horizontális felskálázás esemény feldolgozása</span><span class="sxs-lookup"><span data-stu-id="28f06-123">Scale out event processing</span></span> 

<span data-ttu-id="28f06-124">A [esemény feldolgozása kibővítési](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) minta bemutatja, hogyan használható a [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) Event Hubs adatfolyam fogyasztás okozott terhelés elosztásához.</span><span class="sxs-lookup"><span data-stu-id="28f06-124">The [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how to use the [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) to distribute the workload of Event Hubs stream consumption.</span></span> <span data-ttu-id="28f06-125">Azt illusztrálja, hogyan megvalósításához a **EventProcessor** és **EventProcessorFactory** az eseménystream kezelendő objektumokat.</span><span class="sxs-lookup"><span data-stu-id="28f06-125">It shows how to implement the **EventProcessor** and **EventProcessorFactory** objects to manage the event stream.</span></span> 

###  <a name="pull-data-from-sql-into-an-event-hub"></a><span data-ttu-id="28f06-126">SQL eseményközpontnak olvasnak be adatokat</span><span class="sxs-lookup"><span data-stu-id="28f06-126">Pull data from SQL into an event hub</span></span>

<span data-ttu-id="28f06-127">A [húzza SQL adatok](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) minta bemutatja, hogyan olvasnak be adatokat az SQL-táblából, és hogy egy eseményközpontot, az alárendelt analitikus alkalmazásokban bemenetként használandó.</span><span class="sxs-lookup"><span data-stu-id="28f06-127">The [Pulling SQL data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) sample shows how to pull data from a SQL table and push it to an event hub, to use as an input in downstream analytical applications.</span></span>

### <a name="pull-web-data-into-an-event-hub"></a><span data-ttu-id="28f06-128">Lekéréses webhely adatok eseményközpontba</span><span class="sxs-lookup"><span data-stu-id="28f06-128">Pull web data into an event hub</span></span> 

<span data-ttu-id="28f06-129">A [adatokat importálhat a webes](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) minta bemutatja, hogyan olvasnak be adatokat (például a szállítására részleg forgalom információk hírcsatorna) nyilvános hírcsatornák a, és hogy egy eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="28f06-129">The [Import data from the web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) sample shows how to pull data from public feeds (such as the Department of Transportation's traffic information feed) and push it to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28f06-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28f06-130">Next steps</span></span>

<span data-ttu-id="28f06-131">További tudnivalók a .NET-keretrendszer-verziók érhetők el a következőket:</span><span class="sxs-lookup"><span data-stu-id="28f06-131">Learn more about .NET Framework versions by visiting the following links:</span></span>

- [<span data-ttu-id="28f06-132">Keretrendszerek és célok</span><span class="sxs-lookup"><span data-stu-id="28f06-132">Frameworks and targets</span></span>](/dotnet/articles/standard/frameworks)
- [<span data-ttu-id="28f06-133">.NET-keretrendszer 4.6 és 4.5</span><span class="sxs-lookup"><span data-stu-id="28f06-133">.NET Framework 4.6 and 4.5</span></span>](/dotnet/framework/index)

<span data-ttu-id="28f06-134">Ön többet is megtudhat az Event Hubs a következő cikkekben:</span><span class="sxs-lookup"><span data-stu-id="28f06-134">You can learn more about Event Hubs in the following articles:</span></span>

- [<span data-ttu-id="28f06-135">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="28f06-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="28f06-136">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="28f06-136">Create an event hub</span></span>](event-hubs-create.md)
- [<span data-ttu-id="28f06-137">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="28f06-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)