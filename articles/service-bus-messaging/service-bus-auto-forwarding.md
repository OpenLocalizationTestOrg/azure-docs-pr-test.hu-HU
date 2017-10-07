---
title: "Azure Service Bus üzenetküldési entitások aaaAuto-továbbítási |} Microsoft Docs"
description: "Hogyan toochain egy Service Bus üzenetsor vagy előfizetés tooanother várólista vagy témakör."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: af18273e4e2f81c5363eb830c7decf313afd8307
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Automatikus-továbbítást a Service Bus-entitások láncolás

a Service Bus hello *automatikus-továbbítási* funkciója lehetővé teszi a várólista toochain vagy előfizetés tooanother várólista vagy témakör, amely része hello ugyanazt a névteret. Ha automatikus-továbbítás engedélyezve van, a Service Bus automatikusan hello első sor vagy az előfizetés (forrás) helyezett üzenetek eltávolítja, és beírja hello második üzenetsor vagy témakör (cél). Ne feledje, hogy továbbra is lehetséges toosend egy üzenet toohello cél entitás közvetlenül. Azt is nem lehetséges toochain egy al, például a kézbesítetlen levelek várólistájára, tooanother üzenetsor vagy témakör.

## <a name="using-auto-forwarding"></a>Automatikus-továbbítás használatával
Automatikus-továbbító beállítása hello engedélyezéséhez [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] vagy [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] hello tulajdonságainak [QueueDescription] [ QueueDescription] vagy [SubscriptionDescription] [ SubscriptionDescription] hello forrás, mint hello objektumok Példa.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

hello cél entitás hello létrehozásakor hello forrásentitás kell elhelyezkednie. Ha hello cél entitás nem létezik, akkor a Service Bus ad vissza a kivételt, ha ismételt toocreate hello forrásentitás.

Használhatja az automatikus-továbbítási tooscale témakör megjelenítéséhez ki. A Service Bus korlátok hello [adott témában előfizetések száma](service-bus-quotas.md) too2, 000. Hozzon létre a második szintű témakörök további előfizetések lehetővé teszi. Vegye figyelembe, hogy akkor is, ha nem kötik hello Service Bus-előfizetés hozzáadása a második szintű témakörök hello száma korlátozásai javíthatja a teljes teljesítményt, a témakör hello.

![Automatikus-továbbítási forgatókönyv][0]

Használhatja az automatikus-továbbítási toodecouple üzenetküldők a fogadók is. Vegye figyelembe például az ERP rendszer, amely három modulok alkotja: rendelés feldolgozása, a készletkezelést és a felhasználói kapcsolatok kezelése. Az egyes modulok hoz létre, amelyek megfelelő témakörbe várólistán lévő üzenetek. Alice, Bob és, amely az összes üzenet ki az egymáshoz kapcsolódó tootheir ügyfelek érdekelt értékesítési képviselő. tooreceive azokat az üzeneteket, Ágnes és Bob minden hozzon létre egy személyes várólista és előfizetés egyes hello ERP foglalkozó témakörök, amelyek automatikusan továbbítja az üzeneteket tootheir összes várólista.

![Automatikus-továbbítási forgatókönyv][1]

Alice szabadsága, saját személyes várólista ahelyett, hogy hello ERP témakör állapotba, ha kitölti-e. Ebben a forgatókönyvben mivel értékesítési képviselőink nem kapta meg a minden üzenet hello ERP témakör sem legalább egyszer elérni kvóta.

## <a name="auto-forwarding-considerations"></a>Automatikus-továbbítási kapcsolatos szempontok

Ha hello cél entitás túl sok üzenetek összesít és hello kvóta meghaladja, vagy hello cél entitás le van tiltva, a hello forrásentitás hozzáadja hello üzenetek tooits [kézbesítetlen levelek várólistájára](service-bus-dead-letter-queues.md) addig, amíg nincs hely a hello cél (vagy a hello entitás újraengedélyezése). Az üzenetek toolive hello kézbesítetlen levelek várólistájára, a továbbra is, explicit módon kell fogadni, és dolgozza fel őket a hello kézbesítetlen levelek várólistájára.

Láncolás együtt témakörök tooobtain egy összetett témakör számos előfizetésekkel, esetén ajánlott, hogy rendelkezik-e mérsékelt több hello első szintű témakör előfizetések és sok a lekérdezés hello második szintű kérdésekben. Például egy 20 előfizetések első szintű témakör, azok kapcsolt tooa második szintű témakör 200 előfizetések lehetővé teszi a nagyobb átviteli sebesség eléréséhez, mint 200 előfizetések első szintű témakör egyes kapcsolt tooa második szintű témakör 20 előfizetések .

A Service Bus egy művelet egyes a továbbított üzenet váltók stb. Például egy üzenet tooa témakör 20 előfizetések küld, azok konfigurálva tooauto-továbbító üzenetek tooanother üzenetsor vagy témakör, terheli 21 műveletek Ha előfizetéseket első szintű üdvözlőüzenetére példányát fogadja.

toocreate láncolt tooanother üzenetsor vagy témakör van, a hello előfizetés létrehozója hello kell **kezelése** engedéllyel hello forrás és cél entitás hello. Üzenetek toohello forrás témakör egyetlen követelménye küldése **küldése** hello forrás témakör engedélyeit.

## <a name="next-steps"></a>Következő lépések

Automatikus-továbbítási kapcsolatos részletes információkért tekintse meg a következő témaköröket hello:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

toolearn Service Bus teljesítménnyel kapcsolatos fejlesztések kapcsolatos további információkért lásd: 

* [Ajánlott eljárások használatával a Service Bus üzenetkezelés teljesítménnyel kapcsolatos fejlesztések](service-bus-performance-improvements.md)
* [Particionált üzenetküldési entitások][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
