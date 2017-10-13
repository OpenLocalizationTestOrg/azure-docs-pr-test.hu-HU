---
title: "A Service Bus kézbesítetlen levelek várólistájának |} Microsoft Docs"
description: "Azure Service Bus kézbesítetlen levelek várólistájának áttekintése"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68b2aa38-dba7-491a-9c26-0289bc15d397
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: clemensv;sethm
ms.openlocfilehash: c16bcf30ab96f79e59404a41852e4cd227e28b08
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>A Service Bus kézbesítetlen levelek várólistájának áttekintése

Service Bus-üzenetsorok, előfizetések kaphatják meg egy másodlagos alárendelt várólista nevű egy *kézbesítetlen levelek várólistájára* (DLQ). A kézbesítetlen levelek várólistájára nem kell explicit módon hozható létre és nem törölhető, illetve egyéb felügyelt független a fő entitás.

A cikk ismerteti az Azure Service Bus kézbesítetlen levelek várólistájának. Nagy részét a vitafórum ezt a [kézbesítetlen levelek várólistájának minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) a Githubon.
 
## <a name="the-dead-letter-queue"></a>A kézbesítetlen levelek várólistájára

A kézbesítetlen levelek várólistájára célja üzeneteket egyetlen fogadó nem kézbesíthetők, vagy nem dolgozható fel üzenetek tárolásához. Üzenetek majd eltávolítja a DLQ és megvizsgálni. Egy alkalmazás előfordulhat, operátor segítségével, javítsa ki a problémákat és küldje el újra a message, naplózza, hogy hiba történt, és intézkedéseket. 

API és a protokoll szempontból a DLQ hasonlít többnyire más várólista, azzal a különbséggel, hogy az üzenetek csak küldheti el a fölérendelt entitás a kézbesítetlen levelek kézmozdulat keresztül. Idő a működés közbeni ezenkívül nem teljesülnek, és nem lehet kézbesítetlen egy DLQ üzenetét. A kézbesítetlen levelek várólistájára teljes mértékben támogatja a betekintés-lock kézbesítési és tranzakciós műveletek.

Vegye figyelembe, hogy van-e a DLQ nincs automatikus karbantartása. Üzenetek a DLQ maradnak, amíg meg explicit módon le azokat a DLQ és hívás [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CompleteAsync) a kézbesítetlen levelek üzenethez.

## <a name="moving-messages-to-the-dlq"></a>A DLQ üzenetek áthelyezése

Nincsenek Service Bus több olyan tevékenységet, a DLQ belül az üzenetkezelési motor magát a az beszerzése leküldött üzenetek okozhatja. Az alkalmazás a DLQ üzeneteket is explicit módon helyezheti. 

Az üzenet a broker által lekérdezi áthelyezték, két tulajdonságok fel lettek véve az üzenetek a broker meghívja a belső verzióját a [kézbesítetlen levelek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeadLetter_System_String_System_String_) metódust a üzenethez: `DeadLetterReason` és `DeadLetterErrorDescription`.

Alkalmazások saját kódokat adhat meg a `DeadLetterReason` tulajdonságot, de a rendszer beállítja a következő értékeket.

| Az állapot | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Mindig |HeaderSizeExceeded |Ez az adatfolyam üzenetméret-kvótája túl lett lépve. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing és SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |Kivétel történt. GetType(). név |Kivétel történt. Üzenet |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |Az üzenet lejárt, és nem volt lettered. |
| SubscriptionDescription.RequiresSession |Munkamenet-azonosító értéke null. |A munkamenet engedélyezve entitás nem engedélyezi egy üzenetet, amelynek a munkamenet-azonosító értéke null. |
| ! halott levél várósor |MaxTransferHopCountExceeded |NULL értékű |
| Explicit dead megnevezéséhez alkalmazás |A megadott alkalmazás. |A megadott alkalmazás. |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount túllépését eredményezi
A várólisták és előfizetések is egy [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) és [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_MaxDeliveryCount) tulajdonság az alapértelmezett értéke 10. Amikor egy üzenet kézbesítése megtörtént a zárolást ([ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)), de lett vagy explicit módon elhagyott vagy lejárt a zárolási, az üzenet [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) van minden. Ha [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) meghaladja [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount), az üzenet átkerül a DLQ megadása a `MaxDeliveryCountExceeded` okkód.

Ez a viselkedés nem lehet letiltani, de beállíthatja [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) nagyon sok.

## <a name="exceeding-timetolive"></a>A TimeToLive tulajdonság túllépését eredményezi
Ha a [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) vagy [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) tulajdonsága **Igaz** (az alapértelmezett érték **hamis**), minden lejáró üzeneteket a rendszer áthelyezi a DLQ megadása a `TTLExpiredException` okkód.

Vegye figyelembe, hogy a lejárt üzenetek csak törlődnek, és ezért át a DLQ jogosultak-e a fő várakozási sorba vagy előfizetés; legalább egy aktív fogadó esetén adott szándékosan van így.

## <a name="errors-while-processing-subscription-rules"></a>Hiba történt az előfizetési szabályok feldolgozása
Ha a [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions) tulajdonság engedélyezve van az előfizetéshez, esetlegesen egy előfizetés SQL szűrési szabály végrehajtása során előforduló hibákat a rendszer rögzíti a mentén DLQ a hibás üzenettel.

## <a name="application-level-dead-lettering"></a>Alkalmazásszintű kézbesítetlen levelek kezelése
A rendszer által biztosított kézbesítetlen levelek kezelése szolgáltatások alkalmazások is használhatnak a DLQ kifejezetten elutasítja az üzenet nem fogadható el. Ide tartozhat, amely nem lehet megfelelően feldolgozni tetszőleges rendszer probléma miatt üzenetek, helytelen formátumú hasznos adat található rendelkező üzenetek vagy üzeneteket, amelyek bizonyos üzenet szintű biztonsági rendszer használata esetén a hitelesítés sikertelen.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Kézbesítetlen levelek kezelése ForwardTo vagy SendVia esetekben

Üzenetek kapnak az átviteli kézbesítetlen levelek várólistájára az alábbi feltételek:

- Egy üzenet keresztülhalad várólisták vagy olyan témakörök, amelyek több mint 3 [összeláncolt](service-bus-auto-forwarding.md).
- A célvárólista vagy témakör le van tiltva, vagy törölték.
- A célvárólista vagy témakör meghaladja a maximális entitás méretét.

Ezek kézbesítetlen lettered üzeneteket beolvasni, a fogadó használatával hozhat létre a [FormatTransferDeadletterPath](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_FormatTransferDeadLetterPath_System_String_) segédprogram metódust.

## <a name="example"></a>Példa
A következő kódrészletet a üzenetet fogadó hoz létre. A fogadási ciklusban a fő várólista, a kód lekéri adatokkal rendelkező üzenetben [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_), amely rákérdez, hogy a broker azonnal vissza az összes üzenet azonnal elérhetők legyenek, vagy nincs eredményt adja vissza. A kód üzenetet kap, ha azt azonnal használja tovább azt, milyen lépésekkel a `DeliveryCount`. Miután a rendszer az üzenetet a DLQ helyezi, a fő várakozási sorba üres, és a hurok kilépjen-e, mint a [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) adja vissza **null**.

```csharp
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Következő lépések
További információt a Service Bus-üzenetsorok a következő cikkekben talál:

* [Bevezetés a Service Bus által kezelt üzenetsorok használatába](service-bus-dotnet-get-started-with-queues.md)
* [Az Azure várólisták és a Service Bus üzenetsorok képest](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

