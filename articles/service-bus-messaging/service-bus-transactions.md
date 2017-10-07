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
# <a name="overview-of-service-bus-transaction-processing"></a>A Service Bus tranzakciófeldolgozás áttekintése
A cikk ismerteti az Azure Service Bus hello tranzakció képességeit. Nagy részét hello Vitafórum hello ezt [elemi tranzakciókat és Service Bus minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). Ez a cikk tranzakció-feldolgozást és hello korlátozott tooan áttekintése *küldés* funkció a Service Bus, pedig hello atomi tranzakciók minta szélesebb körben és még bonyolultabbá hatókörében.

## <a name="transactions-in-service-bus"></a>A Service Bus-tranzakciók
A [tranzakció](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) csoportosítja két vagy több műveletet együtt egy *végrehajtási hatókör*. Természetüknél ilyen tranzakció győződjön meg arról, hogy adott csoportja tooa tartozó összes művelet sikeres vagy sikertelen lehet közösen. Ebben a tekintetben tranzakciók összekötőként egy egység, ami általában a hivatkozott tooas *atomicity*. 

A Service Bus egy tranzakciós üzenet broker, és biztosítja az összes belső műveleteket az üzenettároló tranzakciós integritását. Az üzenetek belül a Service Bus, például a mozgóátlag üzenetek tooa valamennyi átvitelt [kézbesítetlen levelek várólistájára](service-bus-dead-letter-queues.md) vagy [automatikus továbbítási](service-bus-auto-forwarding.md) üzenetek entitások közötti, a tranzakciós. Ha a Service Bus fogad egy üzenetet, azt már tárolt és sorozatszáma címkével. Ettől a Service Bus belül bármely üzenet átvitelek koordinált művelet entitások közötti, sem vezet tooloss (forrás sikeres és sikertelen cél) vagy tooduplication (sikertelen és a cél sikeres) hello üzenet.

A Service Bus egy tranzakció csoportosítása egyetlen üzenetküldési entitás (várólista, témakör, előfizetés) hello hatókörén belül műveleteket támogatja. Például több üzenetek tooone várólista küldhet a tranzakció hatókörén belül, és köszönőüzenetei csak lesz véglegesítve toohello várólista napló hello tranzakció sikeres befejezéséről.

## <a name="operations-within-a-transaction-scope"></a>Műveletek a tranzakció hatókörén belül
a tranzakció hatókörén belül végrehajtható hello műveletek a következők:

* **[QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient), [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender), [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient)**: küldés, sendasync metódusok párhuzamosan, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: befejeződése után CompleteAsync, Szolgáltatásműveletnek, AbandonAsync, kézbesítetlen levelek, DeadletterAsync, késleltető, DeferAsync, RenewLock, RenewLockAsync 

Fogadási műveletek nem szerepelnek, mivel feltételezzük, hogy a hello alkalmazás hello üzenetek szerez be [ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) mód, néhány belül az üzenetfogadási hurok vagy egy [OnMessage](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) visszahívási, és csak ezután megnyit egy tranzakció feldolgozása hello üzenet hatókörét.

hello rendezése üdvözlőüzenetére (teljes, szolgáltatásműveletnek, kézbesítetlen levelek, késleltető), majd belül előfordul hello körét és függ, hello hello tranzakció általános eredményét.

## <a name="transfers-and-send-via"></a>Átvitel és a "via küldési"
támogatja az adatokat egy tooa feldolgozó, és tooanother várólista tranzakciós átadási tooenable, a Service Bus *átvitelek*. Átviteli művelettel a először küldő egy üzenetet tooa "átviteli sorban", és hello átviteli sorban azonnal áthelyezi hello üzenet toohello szánt célvárólista azonos robusztus átviteli hello automatikus továbbítás képességet használó megvalósítási hello használata a. hello üzenet soha nem véglegesített toohello átviteli sorban napló oly módon, hogy azt láthatóvá válik a fogyasztók hello átviteli sorban.

Ezt a képességet tranzakciós hello power nyilvánvaló szükségszerűséggé vált Ha maga hello átviteli várólista hello feladó bemeneti üzenetet hello forrását. Ez azt jelenti, Service Bus vihetők át hello üzenet toohello célvárólista "via" hello átviteli sorban, miközben teljes (vagy késleltető, vagy a kézbesítetlen levelek) hello bemeneti üzenet egy atomi művelet az összes műveletet. 

### <a name="see-it-in-code"></a>Megtekintés a kódot
tooset fel ilyen átvitelek, létrehozhat egy üzenet küldője, amelynek célpontja hello célvárólista keresztül hello átviteli sorban. A fogadó, amely lekéri az, hogy ugyanazon várólistában lévő üzenetek is rendelkezik majd. Példa:

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Egy egyszerű tranzakció használja ezeket az elemeket, mint például a következő hello:

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

## <a name="next-steps"></a>Következő lépések

Tekintse meg a következő cikkekben további információt a Service Bus-üzenetsorok hello:

* [Automatikus-továbbítást a Service Bus-entitások láncolás](service-bus-auto-forwarding.md)
* [Automatikus továbbítás minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Elemi tranzakciókat és Service Bus-minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Az Azure várólisták és a Service Bus üzenetsorok képest](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
* [Hogyan toouse Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md)

