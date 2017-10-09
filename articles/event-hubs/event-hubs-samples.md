---
title: "az Event Hubs minták aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: f01f52e6c13f9e885999a6726143440bbc70446d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-samples"></a><span data-ttu-id="659f8-103">Event Hubs – minták</span><span class="sxs-lookup"><span data-stu-id="659f8-103">Event Hubs samples</span></span> 

<span data-ttu-id="659f8-104">hello Azure Event Hubs kódmintát bemutatja a legfontosabb jellemzők [Azure Event Hubs](/azure/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="659f8-104">hello set of Azure Event Hubs samples demonstrates key features in [Azure Event Hubs](/azure/event-hubs/).</span></span> <span data-ttu-id="659f8-105">Ez a cikk kategorizálja, és elérhető, hivatkozások tooeach hello mintákat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="659f8-105">This article categorizes and describes hello samples available, with links tooeach.</span></span>

<span data-ttu-id="659f8-106">Az Event Hubs minták írásának hello időpontban számos különböző helyeken találhatók:</span><span class="sxs-lookup"><span data-stu-id="659f8-106">At hello time of this writing, Event Hubs samples are located in several different places:</span></span>

- [<span data-ttu-id="659f8-107">MSDN fejlesztői mintakódok</span><span class="sxs-lookup"><span data-stu-id="659f8-107">MSDN developer code samples</span></span>](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [<span data-ttu-id="659f8-108">GitHub</span><span class="sxs-lookup"><span data-stu-id="659f8-108">GitHub</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)

<span data-ttu-id="659f8-109">Hello .NET-keretrendszer különböző verzióival kapcsolatos további információkért lásd: [keretrendszerek és célok](/dotnet/articles/standard/frameworks).</span><span class="sxs-lookup"><span data-stu-id="659f8-109">For more information about different versions of hello .NET Framework, see [Frameworks and Targets](/dotnet/articles/standard/frameworks).</span></span>

<span data-ttu-id="659f8-110">Több minta lesz hozzáadva adott idő alatt, ezért vissza ide gyakran frissítések keresése.</span><span class="sxs-lookup"><span data-stu-id="659f8-110">More samples will be added over time, so check back here frequently for updates.</span></span>

## <a name="net-standard"></a><span data-ttu-id="659f8-111">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="659f8-111">.NET Standard</span></span>

<span data-ttu-id="659f8-112">hello következő mintákat bemutatják, hogyan toosend és hello eseményeket fogadni [Event Hubs-ügyfél](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) a hello [.NET szabványos függvénytár](/dotnet/articles/standard/library).</span><span class="sxs-lookup"><span data-stu-id="659f8-112">hello following samples demonstrate how toosend and receive events using hello [Event Hubs client](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) for hello [.NET Standard library](/dotnet/articles/standard/library).</span></span>

### <a name="send-events"></a><span data-ttu-id="659f8-113">Események küldése</span><span class="sxs-lookup"><span data-stu-id="659f8-113">Send events</span></span> 

<span data-ttu-id="659f8-114">Hello [Ismerkedés küldése](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) a példa bemutatja, hogyan toowrite a .NET Core Konzolalkalmazás által küldött események tooan eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="659f8-114">hello [Get started sending](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) sample shows how toowrite a .NET Core console application that sends events tooan event hub.</span></span>

### <a name="receive-events"></a><span data-ttu-id="659f8-115">Események fogadása</span><span class="sxs-lookup"><span data-stu-id="659f8-115">Receive events</span></span> 

<span data-ttu-id="659f8-116">Hello [Ismerkedés fogadását az Event Processor Host hello](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) minta a .NET Core-Konzolalkalmazás, amely üzeneteket fogad az eseményközpontok hello Event Processor Host használatával.</span><span class="sxs-lookup"><span data-stu-id="659f8-116">hello [Get started receiving with hello Event Processor Host](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) sample is a .NET Core console application that receives messages from an event hub using hello Event Processor Host.</span></span>

## <a name="net-framework"></a><span data-ttu-id="659f8-117">.NET-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="659f8-117">.NET Framework</span></span>   

<span data-ttu-id="659f8-118">Azure Event hubs hello célzó különböző más funkciókat fogunk bemutatni, ezeket a mintákat [.NET Framework könyvtár](/dotnet/framework/index).</span><span class="sxs-lookup"><span data-stu-id="659f8-118">These samples demonstrate various other features of Azure Event Hubs, targeting hello [.NET Framework library](/dotnet/framework/index).</span></span>
 
### <a name="notify-users-of-events-received"></a><span data-ttu-id="659f8-119">Értesítse a felhasználókat a fogadott események</span><span class="sxs-lookup"><span data-stu-id="659f8-119">Notify users of events received</span></span>

<span data-ttu-id="659f8-120">Hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) minta értesíti a felhasználókat az érzékelők vagy más rendszerek érkező adatokat.</span><span class="sxs-lookup"><span data-stu-id="659f8-120">hello [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) sample notifies users of data received from sensors or other systems.</span></span>

### <a name="get-started-with-event-hubs"></a><span data-ttu-id="659f8-121">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="659f8-121">Get started with Event Hubs</span></span> 

<span data-ttu-id="659f8-122">Hello [Event Hubs használatának első lépéseit](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) hello alapszintű Event Hubs, például hogyan toocreate eseményközpontban, tooan eseményközpont események küldése és hello segítségével események felhasználásához képességeit mutatja be [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span><span class="sxs-lookup"><span data-stu-id="659f8-122">hello [Event Hubs Getting Started](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) sample demonstrates hello basic capabilities of Event Hubs, such as how toocreate an event hub, send events tooan event hub, and consume events using hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).</span></span>

### <a name="scale-out-event-processing"></a><span data-ttu-id="659f8-123">Horizontális felskálázás esemény feldolgozása</span><span class="sxs-lookup"><span data-stu-id="659f8-123">Scale out event processing</span></span> 

<span data-ttu-id="659f8-124">Hello [bővített kapacitású esemény feldolgozása](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) minta bemutatja, hogyan toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello munkaterhelés Event Hubs adatfolyam-fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="659f8-124">hello [Scale out event processing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) sample demonstrates how toouse hello [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) toodistribute hello workload of Event Hubs stream consumption.</span></span> <span data-ttu-id="659f8-125">Azt illusztrálja, hogyan tooimplement hello **EventProcessor** és **EventProcessorFactory** objektumok toomanage hello eseményfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="659f8-125">It shows how tooimplement hello **EventProcessor** and **EventProcessorFactory** objects toomanage hello event stream.</span></span> 

###  <a name="pull-data-from-sql-into-an-event-hub"></a><span data-ttu-id="659f8-126">SQL eseményközpontnak olvasnak be adatokat</span><span class="sxs-lookup"><span data-stu-id="659f8-126">Pull data from SQL into an event hub</span></span>

<span data-ttu-id="659f8-127">Hello [húzza SQL adatok](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) a példa bemutatja, hogyan toopull adatait egy SQL táblázat, és leküldeni tooan eseményközpontot, az alárendelt analitikus alkalmazásokban bemenetként toouse.</span><span class="sxs-lookup"><span data-stu-id="659f8-127">hello [Pulling SQL data](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) sample shows how toopull data from a SQL table and push it tooan event hub, toouse as an input in downstream analytical applications.</span></span>

### <a name="pull-web-data-into-an-event-hub"></a><span data-ttu-id="659f8-128">Lekéréses webhely adatok eseményközpontba</span><span class="sxs-lookup"><span data-stu-id="659f8-128">Pull web data into an event hub</span></span> 

<span data-ttu-id="659f8-129">Hello [adatokat importálhat hello webes](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) a példa bemutatja, hogyan toopull adatait a nyilvánosság elől hírcsatornák (például egy részleg a szállítására a forgalmi információk hírcsatorna hello), és leküldeni tooan eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="659f8-129">hello [Import data from hello web](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) sample shows how toopull data from public feeds (such as hello Department of Transportation's traffic information feed) and push it tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="659f8-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="659f8-130">Next steps</span></span>

<span data-ttu-id="659f8-131">További tudnivalók a .NET-keretrendszer-verziók érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="659f8-131">Learn more about .NET Framework versions by visiting hello following links:</span></span>

- [<span data-ttu-id="659f8-132">Keretrendszerek és célok</span><span class="sxs-lookup"><span data-stu-id="659f8-132">Frameworks and targets</span></span>](/dotnet/articles/standard/frameworks)
- [<span data-ttu-id="659f8-133">.NET-keretrendszer 4.6 és 4.5</span><span class="sxs-lookup"><span data-stu-id="659f8-133">.NET Framework 4.6 and 4.5</span></span>](/dotnet/framework/index)

<span data-ttu-id="659f8-134">További információ az Event Hubs a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="659f8-134">You can learn more about Event Hubs in hello following articles:</span></span>

- [<span data-ttu-id="659f8-135">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="659f8-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
- [<span data-ttu-id="659f8-136">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="659f8-136">Create an event hub</span></span>](event-hubs-create.md)
- [<span data-ttu-id="659f8-137">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="659f8-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)