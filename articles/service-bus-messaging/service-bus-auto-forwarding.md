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
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a><span data-ttu-id="78ff1-103">Automatikus-továbbítást a Service Bus-entitások láncolás</span><span class="sxs-lookup"><span data-stu-id="78ff1-103">Chaining Service Bus entities with auto-forwarding</span></span>

<span data-ttu-id="78ff1-104">a Service Bus hello *automatikus-továbbítási* funkciója lehetővé teszi a várólista toochain vagy előfizetés tooanother várólista vagy témakör, amely része hello ugyanazt a névteret.</span><span class="sxs-lookup"><span data-stu-id="78ff1-104">hello Service Bus *auto-forwarding* feature enables you toochain a queue or subscription tooanother queue or topic that is part of hello same namespace.</span></span> <span data-ttu-id="78ff1-105">Ha automatikus-továbbítás engedélyezve van, a Service Bus automatikusan hello első sor vagy az előfizetés (forrás) helyezett üzenetek eltávolítja, és beírja hello második üzenetsor vagy témakör (cél).</span><span class="sxs-lookup"><span data-stu-id="78ff1-105">When auto-forwarding is enabled, Service Bus automatically removes messages that are placed in hello first queue or subscription (source) and puts them in hello second queue or topic (destination).</span></span> <span data-ttu-id="78ff1-106">Ne feledje, hogy továbbra is lehetséges toosend egy üzenet toohello cél entitás közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="78ff1-106">Note that it is still possible toosend a message toohello destination entity directly.</span></span> <span data-ttu-id="78ff1-107">Azt is nem lehetséges toochain egy al, például a kézbesítetlen levelek várólistájára, tooanother üzenetsor vagy témakör.</span><span class="sxs-lookup"><span data-stu-id="78ff1-107">Also, it is not possible toochain a subqueue, such as a deadletter queue, tooanother queue or topic.</span></span>

## <a name="using-auto-forwarding"></a><span data-ttu-id="78ff1-108">Automatikus-továbbítás használatával</span><span class="sxs-lookup"><span data-stu-id="78ff1-108">Using auto-forwarding</span></span>
<span data-ttu-id="78ff1-109">Automatikus-továbbító beállítása hello engedélyezéséhez [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] vagy [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] hello tulajdonságainak [QueueDescription] [ QueueDescription] vagy [SubscriptionDescription] [ SubscriptionDescription] hello forrás, mint hello objektumok Példa.</span><span class="sxs-lookup"><span data-stu-id="78ff1-109">You can enable auto-forwarding by setting hello [QueueDescription.ForwardTo][QueueDescription.ForwardTo] or [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] properties on hello [QueueDescription][QueueDescription] or [SubscriptionDescription][SubscriptionDescription] objects for hello source, as in hello following example.</span></span>

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

<span data-ttu-id="78ff1-110">hello cél entitás hello létrehozásakor hello forrásentitás kell elhelyezkednie.</span><span class="sxs-lookup"><span data-stu-id="78ff1-110">hello destination entity must exist at hello time hello source entity is created.</span></span> <span data-ttu-id="78ff1-111">Ha hello cél entitás nem létezik, akkor a Service Bus ad vissza a kivételt, ha ismételt toocreate hello forrásentitás.</span><span class="sxs-lookup"><span data-stu-id="78ff1-111">If hello destination entity does not exist, Service Bus returns an exception when asked toocreate hello source entity.</span></span>

<span data-ttu-id="78ff1-112">Használhatja az automatikus-továbbítási tooscale témakör megjelenítéséhez ki.</span><span class="sxs-lookup"><span data-stu-id="78ff1-112">You can use auto-forwarding tooscale out an individual topic.</span></span> <span data-ttu-id="78ff1-113">A Service Bus korlátok hello [adott témában előfizetések száma](service-bus-quotas.md) too2, 000.</span><span class="sxs-lookup"><span data-stu-id="78ff1-113">Service Bus limits hello [number of subscriptions on a given topic](service-bus-quotas.md) too2,000.</span></span> <span data-ttu-id="78ff1-114">Hozzon létre a második szintű témakörök további előfizetések lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="78ff1-114">You can accommodate additional subscriptions by creating second-level topics.</span></span> <span data-ttu-id="78ff1-115">Vegye figyelembe, hogy akkor is, ha nem kötik hello Service Bus-előfizetés hozzáadása a második szintű témakörök hello száma korlátozásai javíthatja a teljes teljesítményt, a témakör hello.</span><span class="sxs-lookup"><span data-stu-id="78ff1-115">Note that even if you are not bound by hello Service Bus limitation on hello number of subscriptions, adding a second level of topics can improve hello overall throughput of your topic.</span></span>

![Automatikus-továbbítási forgatókönyv][0]

<span data-ttu-id="78ff1-117">Használhatja az automatikus-továbbítási toodecouple üzenetküldők a fogadók is.</span><span class="sxs-lookup"><span data-stu-id="78ff1-117">You can also use auto-forwarding toodecouple message senders from receivers.</span></span> <span data-ttu-id="78ff1-118">Vegye figyelembe például az ERP rendszer, amely három modulok alkotja: rendelés feldolgozása, a készletkezelést és a felhasználói kapcsolatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="78ff1-118">For example, consider an ERP system that consists of three modules: order processing, inventory management, and customer relations management.</span></span> <span data-ttu-id="78ff1-119">Az egyes modulok hoz létre, amelyek megfelelő témakörbe várólistán lévő üzenetek.</span><span class="sxs-lookup"><span data-stu-id="78ff1-119">Each of these modules generates messages that are enqueued into a corresponding topic.</span></span> <span data-ttu-id="78ff1-120">Alice, Bob és, amely az összes üzenet ki az egymáshoz kapcsolódó tootheir ügyfelek érdekelt értékesítési képviselő.</span><span class="sxs-lookup"><span data-stu-id="78ff1-120">Alice and Bob are sales representatives that are interested in all messages that relate tootheir customers.</span></span> <span data-ttu-id="78ff1-121">tooreceive azokat az üzeneteket, Ágnes és Bob minden hozzon létre egy személyes várólista és előfizetés egyes hello ERP foglalkozó témakörök, amelyek automatikusan továbbítja az üzeneteket tootheir összes várólista.</span><span class="sxs-lookup"><span data-stu-id="78ff1-121">tooreceive those messages, Alice and Bob each create a personal queue and a subscription on each of hello ERP topics that automatically forward all messages tootheir queue.</span></span>

![Automatikus-továbbítási forgatókönyv][1]

<span data-ttu-id="78ff1-123">Alice szabadsága, saját személyes várólista ahelyett, hogy hello ERP témakör állapotba, ha kitölti-e.</span><span class="sxs-lookup"><span data-stu-id="78ff1-123">If Alice goes on vacation, her personal queue, rather than hello ERP topic, fills up.</span></span> <span data-ttu-id="78ff1-124">Ebben a forgatókönyvben mivel értékesítési képviselőink nem kapta meg a minden üzenet hello ERP témakör sem legalább egyszer elérni kvóta.</span><span class="sxs-lookup"><span data-stu-id="78ff1-124">In this scenario, because a sales representative has not received any messages, none of hello ERP topics ever reach quota.</span></span>

## <a name="auto-forwarding-considerations"></a><span data-ttu-id="78ff1-125">Automatikus-továbbítási kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="78ff1-125">Auto-forwarding considerations</span></span>

<span data-ttu-id="78ff1-126">Ha hello cél entitás túl sok üzenetek összesít és hello kvóta meghaladja, vagy hello cél entitás le van tiltva, a hello forrásentitás hozzáadja hello üzenetek tooits [kézbesítetlen levelek várólistájára](service-bus-dead-letter-queues.md) addig, amíg nincs hely a hello cél (vagy a hello entitás újraengedélyezése).</span><span class="sxs-lookup"><span data-stu-id="78ff1-126">If hello destination entity accumulates too many messages and exceeds hello quota, or hello destination entity is disabled, hello source entity adds hello messages tooits [dead-letter queue](service-bus-dead-letter-queues.md) until there is space in hello destination (or hello entity is re-enabled).</span></span> <span data-ttu-id="78ff1-127">Az üzenetek toolive hello kézbesítetlen levelek várólistájára, a továbbra is, explicit módon kell fogadni, és dolgozza fel őket a hello kézbesítetlen levelek várólistájára.</span><span class="sxs-lookup"><span data-stu-id="78ff1-127">Those messages will continue toolive in hello dead-letter queue, so you must explicitly receive and process them from hello dead-letter queue.</span></span>

<span data-ttu-id="78ff1-128">Láncolás együtt témakörök tooobtain egy összetett témakör számos előfizetésekkel, esetén ajánlott, hogy rendelkezik-e mérsékelt több hello első szintű témakör előfizetések és sok a lekérdezés hello második szintű kérdésekben.</span><span class="sxs-lookup"><span data-stu-id="78ff1-128">When chaining together individual topics tooobtain a composite topic with many subscriptions, it is recommended that you have a moderate number of subscriptions on hello first-level topic and many subscriptions on hello second-level topics.</span></span> <span data-ttu-id="78ff1-129">Például egy 20 előfizetések első szintű témakör, azok kapcsolt tooa második szintű témakör 200 előfizetések lehetővé teszi a nagyobb átviteli sebesség eléréséhez, mint 200 előfizetések első szintű témakör egyes kapcsolt tooa második szintű témakör 20 előfizetések .</span><span class="sxs-lookup"><span data-stu-id="78ff1-129">For example, a first-level topic with 20 subscriptions, each of them chained tooa second-level topic with 200 subscriptions, allows for higher throughput than a first-level topic with 200 subscriptions, each chained tooa second-level topic with 20 subscriptions.</span></span>

<span data-ttu-id="78ff1-130">A Service Bus egy művelet egyes a továbbított üzenet váltók stb.</span><span class="sxs-lookup"><span data-stu-id="78ff1-130">Service Bus bills one operation for each forwarded message.</span></span> <span data-ttu-id="78ff1-131">Például egy üzenet tooa témakör 20 előfizetések küld, azok konfigurálva tooauto-továbbító üzenetek tooanother üzenetsor vagy témakör, terheli 21 műveletek Ha előfizetéseket első szintű üdvözlőüzenetére példányát fogadja.</span><span class="sxs-lookup"><span data-stu-id="78ff1-131">For example, sending a message tooa topic with 20 subscriptions, each of them configured tooauto-forward messages tooanother queue or topic, is billed as 21 operations if all first-level subscriptions receive a copy of hello message.</span></span>

<span data-ttu-id="78ff1-132">toocreate láncolt tooanother üzenetsor vagy témakör van, a hello előfizetés létrehozója hello kell **kezelése** engedéllyel hello forrás és cél entitás hello.</span><span class="sxs-lookup"><span data-stu-id="78ff1-132">toocreate a subscription that is chained tooanother queue or topic, hello creator of hello subscription must have **Manage** permissions on both hello source and hello destination entity.</span></span> <span data-ttu-id="78ff1-133">Üzenetek toohello forrás témakör egyetlen követelménye küldése **küldése** hello forrás témakör engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="78ff1-133">Sending messages toohello source topic only requires **Send** permissions on hello source topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78ff1-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78ff1-134">Next steps</span></span>

<span data-ttu-id="78ff1-135">Automatikus-továbbítási kapcsolatos részletes információkért tekintse meg a következő témaköröket hello:</span><span class="sxs-lookup"><span data-stu-id="78ff1-135">For detailed information about auto-forwarding, see hello following reference topics:</span></span>

* <span data-ttu-id="78ff1-136">[ForwardTo][QueueDescription.ForwardTo]</span><span class="sxs-lookup"><span data-stu-id="78ff1-136">[ForwardTo][QueueDescription.ForwardTo]</span></span>
* <span data-ttu-id="78ff1-137">[QueueDescription][QueueDescription]</span><span class="sxs-lookup"><span data-stu-id="78ff1-137">[QueueDescription][QueueDescription]</span></span>
* <span data-ttu-id="78ff1-138">[SubscriptionDescription][SubscriptionDescription]</span><span class="sxs-lookup"><span data-stu-id="78ff1-138">[SubscriptionDescription][SubscriptionDescription]</span></span>

<span data-ttu-id="78ff1-139">toolearn Service Bus teljesítménnyel kapcsolatos fejlesztések kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="78ff1-139">toolearn more about Service Bus performance improvements, see</span></span> 

* [<span data-ttu-id="78ff1-140">Ajánlott eljárások használatával a Service Bus üzenetkezelés teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="78ff1-140">Best Practices for performance improvements using Service Bus Messaging</span></span>](service-bus-performance-improvements.md)
* <span data-ttu-id="78ff1-141">[Particionált üzenetküldési entitások][Partitioned messaging entities].</span><span class="sxs-lookup"><span data-stu-id="78ff1-141">[Partitioned messaging entities][Partitioned messaging entities].</span></span>

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
