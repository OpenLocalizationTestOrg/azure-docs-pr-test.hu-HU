---
title: "tranzakció feldolgozása az Azure Service Bus aaaOverview |} Microsoft Docs"
description: "Azure Service Bus atomi tranzakciók és a küldési keresztül áttekintése"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: 5ed4d1fd3a089b8ebcd69a568f4ac863e753aecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-transaction-processing"></a><span data-ttu-id="e26c6-103">A Service Bus tranzakciófeldolgozás áttekintése</span><span class="sxs-lookup"><span data-stu-id="e26c6-103">Overview of Service Bus transaction processing</span></span>
<span data-ttu-id="e26c6-104">A cikk ismerteti az Azure Service Bus hello tranzakció képességeit.</span><span class="sxs-lookup"><span data-stu-id="e26c6-104">This article discusses hello transaction capabilities of Azure Service Bus.</span></span> <span data-ttu-id="e26c6-105">Nagy részét hello Vitafórum hello ezt [elemi tranzakciókat és Service Bus minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span><span class="sxs-lookup"><span data-stu-id="e26c6-105">Much of hello discussion is illustrated by hello [Atomic Transactions with Service Bus sample](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions).</span></span> <span data-ttu-id="e26c6-106">Ez a cikk tranzakció-feldolgozást és hello korlátozott tooan áttekintése *küldés* funkció a Service Bus, pedig hello atomi tranzakciók minta szélesebb körben és még bonyolultabbá hatókörében.</span><span class="sxs-lookup"><span data-stu-id="e26c6-106">This article is limited tooan overview of transaction processing and hello *send via* feature in Service Bus, while hello Atomic Transactions sample is broader and more complex in scope.</span></span>

## <a name="transactions-in-service-bus"></a><span data-ttu-id="e26c6-107">A Service Bus-tranzakciók</span><span class="sxs-lookup"><span data-stu-id="e26c6-107">Transactions in Service Bus</span></span>
<span data-ttu-id="e26c6-108">A [tranzakció](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) csoportosítja két vagy több műveletet együtt egy *végrehajtási hatókör*.</span><span class="sxs-lookup"><span data-stu-id="e26c6-108">A [transaction](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) groups two or more operations together into an *execution scope*.</span></span> <span data-ttu-id="e26c6-109">Természetüknél ilyen tranzakció győződjön meg arról, hogy adott csoportja tooa tartozó összes művelet sikeres vagy sikertelen lehet közösen.</span><span class="sxs-lookup"><span data-stu-id="e26c6-109">By nature, such a transaction must ensure that all operations belonging tooa given group of operations either succeed or fail jointly.</span></span> <span data-ttu-id="e26c6-110">Ebben a tekintetben tranzakciók összekötőként egy egység, ami általában a hivatkozott tooas *atomicity*.</span><span class="sxs-lookup"><span data-stu-id="e26c6-110">In this respect transactions act as one unit, which is often referred tooas *atomicity*.</span></span> 

<span data-ttu-id="e26c6-111">A Service Bus egy tranzakciós üzenet broker, és biztosítja az összes belső műveleteket az üzenettároló tranzakciós integritását.</span><span class="sxs-lookup"><span data-stu-id="e26c6-111">Service Bus is a transactional message broker and ensures transactional integrity for all internal operations against its message stores.</span></span> <span data-ttu-id="e26c6-112">Az üzenetek belül a Service Bus, például a mozgóátlag üzenetek tooa valamennyi átvitelt [kézbesítetlen levelek várólistájára](service-bus-dead-letter-queues.md) vagy [automatikus továbbítási](service-bus-auto-forwarding.md) üzenetek entitások közötti, a tranzakciós.</span><span class="sxs-lookup"><span data-stu-id="e26c6-112">All transfers of messages inside of Service Bus, such as moving messages tooa [dead-letter queue](service-bus-dead-letter-queues.md) or [automatic forwarding](service-bus-auto-forwarding.md) of messages between entities, are transactional.</span></span> <span data-ttu-id="e26c6-113">Ha a Service Bus fogad egy üzenetet, azt már tárolt és sorozatszáma címkével.</span><span class="sxs-lookup"><span data-stu-id="e26c6-113">As such, if Service Bus accepts a message, it has already been stored and labeled with a sequence number.</span></span> <span data-ttu-id="e26c6-114">Ettől a Service Bus belül bármely üzenet átvitelek koordinált művelet entitások közötti, sem vezet tooloss (forrás sikeres és sikertelen cél) vagy tooduplication (sikertelen és a cél sikeres) hello üzenet.</span><span class="sxs-lookup"><span data-stu-id="e26c6-114">From then on, any message transfers within Service Bus are coordinated operations across entities, and will neither lead tooloss (source succeeds and target fails) or tooduplication (source fails and target succeeds) of hello message.</span></span>

<span data-ttu-id="e26c6-115">A Service Bus egy tranzakció csoportosítása egyetlen üzenetküldési entitás (várólista, témakör, előfizetés) hello hatókörén belül műveleteket támogatja.</span><span class="sxs-lookup"><span data-stu-id="e26c6-115">Service Bus supports grouping operations against a single messaging entity (queue, topic, subscription) within hello scope of a transaction.</span></span> <span data-ttu-id="e26c6-116">Például több üzenetek tooone várólista küldhet a tranzakció hatókörén belül, és köszönőüzenetei csak lesz véglegesítve toohello várólista napló hello tranzakció sikeres befejezéséről.</span><span class="sxs-lookup"><span data-stu-id="e26c6-116">For example, you can send several messages tooone queue from within a transaction scope, and hello messages will only be committed toohello queue's log when hello transaction successfully completes.</span></span>

## <a name="operations-within-a-transaction-scope"></a><span data-ttu-id="e26c6-117">Műveletek a tranzakció hatókörén belül</span><span class="sxs-lookup"><span data-stu-id="e26c6-117">Operations within a transaction scope</span></span>
<span data-ttu-id="e26c6-118">a tranzakció hatókörén belül végrehajtható hello műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="e26c6-118">hello operations that can be performed within a transaction scope are as follows:</span></span>

* <span data-ttu-id="e26c6-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: küldés, sendasync metódusok párhuzamosan, SendBatch, SendBatchAsync</span><span class="sxs-lookup"><span data-stu-id="e26c6-119">**[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: Send, SendAsync, SendBatch, SendBatchAsync</span></span> 
* <span data-ttu-id="e26c6-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: befejeződése után CompleteAsync, Szolgáltatásműveletnek, AbandonAsync, kézbesítetlen levelek, DeadletterAsync, késleltető, DeferAsync, RenewLock, RenewLockAsync</span><span class="sxs-lookup"><span data-stu-id="e26c6-120">**[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Complete, CompleteAsync, Abandon, AbandonAsync, Deadletter, DeadletterAsync, Defer, DeferAsync, RenewLock, RenewLockAsync</span></span> 

<span data-ttu-id="e26c6-121">Fogadási műveletek nem szerepelnek, mivel feltételezzük, hogy a hello alkalmazás hello üzenetek szerez be [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mód, néhány belül az üzenetfogadási hurok vagy egy [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) visszahívási, és csak ezután megnyit egy tranzakció feldolgozása hello üzenet hatókörét.</span><span class="sxs-lookup"><span data-stu-id="e26c6-121">Receive operations are not included, because it is assumed that hello application acquires messages using hello [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mode, inside some receive loop or with an [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) callback, and only then opens a transaction scope for processing hello message.</span></span>

<span data-ttu-id="e26c6-122">hello rendezése üdvözlőüzenetére (teljes, szolgáltatásműveletnek, kézbesítetlen levelek, késleltető), majd belül előfordul hello körét és függ, hello hello tranzakció általános eredményét.</span><span class="sxs-lookup"><span data-stu-id="e26c6-122">hello disposition of hello message (complete, abandon, dead-letter, defer) then occurs within hello scope of, and dependent on, hello overall outcome of hello transaction.</span></span>

## <a name="transfers-and-send-via"></a><span data-ttu-id="e26c6-123">Átvitel és a "via küldési"</span><span class="sxs-lookup"><span data-stu-id="e26c6-123">Transfers and "send via"</span></span>
<span data-ttu-id="e26c6-124">támogatja az adatokat egy tooa feldolgozó, és tooanother várólista tranzakciós átadási tooenable, a Service Bus *átvitelek*.</span><span class="sxs-lookup"><span data-stu-id="e26c6-124">tooenable transactional handover of data from a queue tooa processor, and then tooanother queue, Service Bus supports *transfers*.</span></span> <span data-ttu-id="e26c6-125">Átviteli művelettel a először küldő egy üzenetet tooa "átviteli sorban", és hello átviteli sorban azonnal áthelyezi hello üzenet toohello szánt célvárólista azonos robusztus átviteli hello automatikus továbbítás képességet használó megvalósítási hello használata a.</span><span class="sxs-lookup"><span data-stu-id="e26c6-125">In a transfer operation, a sender first sends a message tooa "transfer queue" and hello transfer queue immediately moves hello message toohello intended destination queue using hello same robust transfer implementation that hello auto-forward capability relies on.</span></span> <span data-ttu-id="e26c6-126">hello üzenet soha nem véglegesített toohello átviteli sorban napló oly módon, hogy azt láthatóvá válik a fogyasztók hello átviteli sorban.</span><span class="sxs-lookup"><span data-stu-id="e26c6-126">hello message is never committed toohello transfer queue's log in a way that it becomes visible for hello transfer queue's consumers.</span></span>

<span data-ttu-id="e26c6-127">Ezt a képességet tranzakciós hello power nyilvánvaló szükségszerűséggé vált Ha maga hello átviteli várólista hello feladó bemeneti üzenetet hello forrását.</span><span class="sxs-lookup"><span data-stu-id="e26c6-127">hello power of this transactional capability becomes apparent when hello transfer queue itself is hello source of hello sender's input messages.</span></span> <span data-ttu-id="e26c6-128">Ez azt jelenti, Service Bus vihetők át hello üzenet toohello célvárólista "via" hello átviteli sorban, miközben teljes (vagy késleltető, vagy a kézbesítetlen levelek) hello bemeneti üzenet egy atomi művelet az összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="e26c6-128">In other words, Service Bus can transfer hello message toohello destination queue "via" hello transfer queue, while performing a complete (or defer, or dead-letter) operation on hello input message, all in one atomic operation.</span></span> 

### <a name="see-it-in-code"></a><span data-ttu-id="e26c6-129">Megtekintés a kódot</span><span class="sxs-lookup"><span data-stu-id="e26c6-129">See it in code</span></span>
<span data-ttu-id="e26c6-130">tooset fel ilyen átvitelek, létrehozhat egy üzenet küldője, amelynek célpontja hello célvárólista keresztül hello átviteli sorban.</span><span class="sxs-lookup"><span data-stu-id="e26c6-130">tooset up such transfers, you create a message sender that targets hello destination queue via hello transfer queue.</span></span> <span data-ttu-id="e26c6-131">A fogadó, amely lekéri az, hogy ugyanazon várólistában lévő üzenetek is rendelkezik majd.</span><span class="sxs-lookup"><span data-stu-id="e26c6-131">You will also have a receiver that pulls messages from that same queue.</span></span> <span data-ttu-id="e26c6-132">Példa:</span><span class="sxs-lookup"><span data-stu-id="e26c6-132">For example:</span></span>

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

<span data-ttu-id="e26c6-133">Egy egyszerű tranzakció használja ezeket az elemeket, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e26c6-133">A simple transaction then uses these elements, as in hello following example:</span></span>

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package hello result 

    msg.Complete(); // mark hello message as done
    sender.Send(newmsg); // forward hello result

    scope.Complete(); // declare hello transaction done
} 
```

## <a name="next-steps"></a><span data-ttu-id="e26c6-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e26c6-134">Next steps</span></span>

<span data-ttu-id="e26c6-135">Tekintse meg a következő cikkekben további információt a Service Bus-üzenetsorok hello:</span><span class="sxs-lookup"><span data-stu-id="e26c6-135">See hello following articles for more information about Service Bus queues:</span></span>

* [<span data-ttu-id="e26c6-136">Automatikus-továbbítást a Service Bus-entitások láncolás</span><span class="sxs-lookup"><span data-stu-id="e26c6-136">Chaining Service Bus entities with auto-forwarding</span></span>](service-bus-auto-forwarding.md)
* [<span data-ttu-id="e26c6-137">Automatikus továbbítás minta</span><span class="sxs-lookup"><span data-stu-id="e26c6-137">Auto-forward sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [<span data-ttu-id="e26c6-138">Elemi tranzakciókat és Service Bus-minta</span><span class="sxs-lookup"><span data-stu-id="e26c6-138">Atomic Transactions with Service Bus sample</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [<span data-ttu-id="e26c6-139">Az Azure várólisták és a Service Bus üzenetsorok képest</span><span class="sxs-lookup"><span data-stu-id="e26c6-139">Azure Queues and Service Bus queues compared</span></span>](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [<span data-ttu-id="e26c6-140">Hogyan toouse Service Bus-üzenetsorok</span><span class="sxs-lookup"><span data-stu-id="e26c6-140">How toouse Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)

