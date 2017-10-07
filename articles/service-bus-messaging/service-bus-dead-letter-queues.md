---
title: "aaaService Bus kézbesítetlen várólisták |} Microsoft Docs"
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
ms.openlocfilehash: 1638272085b8a3a59e8814f6f943caee35a2bfdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-service-bus-dead-letter-queues"></a>A Service Bus kézbesítetlen levelek várólistájának áttekintése

Service Bus-üzenetsorok, előfizetések kaphatják meg egy másodlagos alárendelt várólista nevű egy *kézbesítetlen levelek várólistájára* (DLQ). hello kézbesítetlen levelek várólistájára nem kell explicit módon létrehozott toobe, és nem lehet a törölt vagy egyéb felügyelt független hello fő entitás.

A cikk ismerteti az Azure Service Bus kézbesítetlen levelek várólistájának. Nagy részét hello Vitafórum hello ezt [kézbesítetlen levelek várólistájának minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/DeadletterQueue) a Githubon.
 
## <a name="hello-dead-letter-queue"></a>hello kézbesítetlen levelek várólistájára

hello hello kézbesítetlen levelek várólistájára célja nem lehet kézbesíteni az tooany fogadó toohold üzenetek vagy üzeneteket, amelyek nem dolgozható fel. Üzenetek majd hello DLQ távolítva, és megvizsgálja. Egy alkalmazás előfordulhat, operátor segítségével, javítsa ki a problémákat és küldje el újra a üdvözlőüzenetére, jelentkezzen hello azt, hogy hiba történt, és intézkedéseket. 

Az API és a protokoll szempontjából hello DLQ van a más várólista többnyire hasonló tooany azzal a különbséggel, hogy az üzenetek csak küldheti el hello kézbesítetlen levelek kézmozdulat hello fölérendelt entitás keresztül. Idő a működés közbeni ezenkívül nem teljesülnek, és nem lehet kézbesítetlen egy DLQ üzenetét. hello kézbesítetlen levelek várólistájára teljes mértékben támogatja a betekintés-lock kézbesítési és tranzakciós műveletek.

Vegye figyelembe, hogy van-e hello DLQ nincs automatikus karbantartása. Üzenetek hello DLQ maradnak, amíg explicit módon való lehívás hello DLQ és hívás [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CompleteAsync) hello halottlevél üzenetben.

## <a name="moving-messages-toohello-dlq"></a>Toohello DLQ áthelyezése üzenetek

Nincsenek több tevékenységet a Service Bus üzenetküldési maga motor hello belül a DLQ toohello leküldött üzenetek tooget okozhatja. Egy alkalmazás explicit módon is áthelyezheti üzenetek toohello DLQ. 

Üdvözlőüzenetére hello broker által lekérdezi áthelyezték, két tulajdonságok fel lettek véve toohello üzenet hello broker meghívja a hello belső verziójának [kézbesítetlen levelek](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeadLetter_System_String_System_String_) üdvözlőüzenetére metódusa: `DeadLetterReason` és `DeadLetterErrorDescription`.

Alkalmazások definiálhat a saját hello kódjait `DeadLetterReason` tulajdonságot, de hello rendszer beállítása hello a következő értékeket.

| Az állapot | DeadLetterReason | DeadLetterErrorDescription |
| --- | --- | --- |
| Mindig |HeaderSizeExceeded |Ez az adatfolyam hello üzenetméret-kvótája túl lett lépve. |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing és SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions |Kivétel történt. GetType(). név |Kivétel történt. Üzenet |
| EnableDeadLetteringOnMessageExpiration |TTLExpiredException |üdvözlőüzenetére lejárt, és nem volt lettered. |
| SubscriptionDescription.RequiresSession |Munkamenet-azonosító értéke null. |A munkamenet engedélyezve entitás nem engedélyezi egy üzenetet, amelynek a munkamenet-azonosító értéke null. |
| ! halott levél várósor |MaxTransferHopCountExceeded |NULL értékű |
| Explicit dead megnevezéséhez alkalmazás |A megadott alkalmazás. |A megadott alkalmazás. |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount túllépését eredményezi
A várólisták és előfizetések is egy [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) és [SubscriptionDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_MaxDeliveryCount) tulajdonság rendre; hello alapértelmezett értéke 10. Amikor egy üzenet kézbesítése megtörtént a zárolást ([ReceiveMode.PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)), de lett vagy explicit módon elhagyott vagy hello zárolása lejárt, hello üzenet [BrokeredMessage.DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) van minden. Ha [DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) meghaladja [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount), üdvözlőüzenetére áthelyezett toohello DLQ hello megadó `MaxDeliveryCountExceeded` okkód.

Ez a viselkedés nem lehet letiltani, de beállíthatja [MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MaxDeliveryCount) tooa nagyon nagy száma.

## <a name="exceeding-timetolive"></a>A TimeToLive tulajdonság túllépését eredményezi
Ha hello [QueueDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableDeadLetteringOnMessageExpiration) vagy [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnMessageExpiration) tulajdonsága túl**Igaz** (hello alapértelmezett érték a **hamis**), minden lejáró üzenetek áthelyezett toohello DLQ hello megadó `TTLExpiredException` okkód.

Vegye figyelembe, hogy a lejárt üzenetek csak kiürítésekor, és ezért át toohello DLQ, húzza a hello fő várakozási sorba vagy előfizetés; legalább egy aktív fogadó esetén adott szándékosan van így.

## <a name="errors-while-processing-subscription-rules"></a>Hiba történt az előfizetési szabályok feldolgozása
Ha hello [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_EnableDeadLetteringOnFilterEvaluationExceptions) tulajdonság engedélyezve van az előfizetéshez, esetlegesen egy előfizetés SQL szűrési szabály végrehajtása során előforduló hibákat a rendszer rögzíti a hello DLQ együtt hello problémát okozó üzenet.

## <a name="application-level-dead-lettering"></a>Alkalmazásszintű kézbesítetlen levelek kezelése
Alkalmazások hozzáadása toohello rendszer által biztosított kézbesítetlen levelek kezelése funkciói, DLQ tooexplicitly utasítsa el az elfogadhatatlan köszönőüzenetei használhatják. Ide tartozhat, amely nem lehet megfelelően feldolgozni miatt a rendszer a probléma, helytelen formátumú hasznos adat található rendelkező üzenetek vagy néhány üzenet szintű biztonsági rendszer használata esetén a hitelesítés sikertelen üzenetek tooany rendezési üzeneteket.

## <a name="dead-lettering-in-forwardto-or-sendvia-scenarios"></a>Kézbesítetlen levelek kezelése ForwardTo vagy SendVia esetekben

Üzenetek lesznek elküldött toohello átviteli kézbesítetlen levelek várólistájára hello a következő feltételek alapján:

- Egy üzenet keresztülhalad várólisták vagy olyan témakörök, amelyek több mint 3 [összeláncolt](service-bus-auto-forwarding.md).
- hello cél üzenetsor vagy témakör le van tiltva, vagy törölték.
- hello cél üzenetsor vagy témakör meghaladja a hello entitás maximális méretét.

tooretrieve ezek kézbesítetlen lettered üzeneteket hozhat létre a fogadó hello segítségével [FormatTransferDeadletterPath](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_FormatTransferDeadLetterPath_System_String_) segédprogram metódust.

## <a name="example"></a>Példa
a következő kódrészletet hello létrehoz egy üzenetet fogadó. A hello az üzenetfogadási hurok hello fő várólista, hello kód lekéri a üdvözlőüzenetére [Receive(TimeSpan.Zero)](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_Receive_System_TimeSpan_), amely rákérdez hello broker tooinstantly visszatérési bármely üzenet azonnal elérhetők, vagy nincs eredménnyel tooreturn. Hello kód üzenetet kap, ha azt azonnal használja tovább, ami növeli a hello `DeliveryCount`. Miután hello rendszer hello üzenet toohello DLQ helyezi, hello fő üres és hello hurok kilép, mint a [ReceiveAsync](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_ReceiveAsync_System_TimeSpan_) adja vissza **null**.

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
Tekintse meg a következő cikkekben további információt a Service Bus-üzenetsorok hello:

* [Bevezetés a Service Bus által kezelt üzenetsorok használatába](service-bus-dotnet-get-started-with-queues.md)
* [Az Azure várólisták és a Service Bus üzenetsorok képest](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

