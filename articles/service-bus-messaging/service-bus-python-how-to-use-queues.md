---
title: "Azure Service Bus-üzenetsorok használata Python |} Microsoft Docs"
description: "Útmutató: a Python Azure Service Bus-üzenetsorok használata."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm;lmazuel
ms.openlocfilehash: e1e81ad1d7b4fe0e044917f090cac59dfd5b6332
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-queues-with-python"></a><span data-ttu-id="d947d-103">Service Bus-üzenetsorok használata Python</span><span class="sxs-lookup"><span data-stu-id="d947d-103">How to use Service Bus queues with Python</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="d947d-104">Ez a cikk a Service Bus-üzenetsorok használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d947d-104">This article describes how to use Service Bus queues.</span></span> <span data-ttu-id="d947d-105">A mintákat a Python és -felhasználási nyelven íródtak a [Python Azure Service Bus csomag][Python Azure Service Bus package].</span><span class="sxs-lookup"><span data-stu-id="d947d-105">The samples are written in Python and use the [Python Azure Service Bus package][Python Azure Service Bus package].</span></span> <span data-ttu-id="d947d-106">Az ismertetett forgatókönyvek **üzenetsorok üzenetek küldése és fogadása létrehozása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="d947d-106">The scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> <span data-ttu-id="d947d-107">Python telepítéséhez vagy a [Python Azure Service Bus csomag][Python Azure Service Bus package], tekintse meg a [Python a telepítési útmutató](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="d947d-107">To install Python or the [Python Azure Service Bus package][Python Azure Service Bus package], see the [Python Installation Guide](../python-how-to-install.md).</span></span>
> 
> 

## <a name="create-a-queue"></a><span data-ttu-id="d947d-108">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="d947d-108">Create a queue</span></span>
<span data-ttu-id="d947d-109">A **ServiceBusService** objektum lehetővé teszi a várólisták használata.</span><span class="sxs-lookup"><span data-stu-id="d947d-109">The **ServiceBusService** object enables you to work with queues.</span></span> <span data-ttu-id="d947d-110">Adja hozzá a következő kódot programon keresztüli eléréséhez a Service Bus kívánja bármely Python fájl felső részén:</span><span class="sxs-lookup"><span data-stu-id="d947d-110">Add the following code near the top of any Python file in which you wish to programmatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

<span data-ttu-id="d947d-111">Az alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d947d-111">The following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="d947d-112">Cserélje le `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` a névtér, közös hozzáférésű jogosultságkód (SAS) kulcs nevét és értékét.</span><span class="sxs-lookup"><span data-stu-id="d947d-112">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your namespace, shared access signature (SAS) key name, and value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="d947d-113">A SAS-kulcs nevét és értékét értékeit megtalálhatók a [Azure-portálon] [ Azure portal] kapcsolatadatok, vagy a Visual Studio **tulajdonságok** ablaktáblán, ha a szolgáltatás kiválasztása Szolgáltatásbusz-névtér a Server Explorer (ahogy az előző szakaszban).</span><span class="sxs-lookup"><span data-stu-id="d947d-113">The values for the SAS key name and value can be found in the [Azure portal][Azure portal] connection information, or in the Visual Studio **Properties** pane when selecting the Service Bus namespace in Server Explorer (as shown in the previous section).</span></span>

```python
bus_service.create_queue('taskqueue')
```

<span data-ttu-id="d947d-114">A `create_queue` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a bírálja felül az alapértelmezett várólista beállításait, például az üzenet time to live (TTL) vagy a várólista maximális hossza.</span><span class="sxs-lookup"><span data-stu-id="d947d-114">The `create_queue` method also supports additional options, which enable you to override default queue settings such as message time to live (TTL) or maximum queue size.</span></span> <span data-ttu-id="d947d-115">A következő példa a várólista maximális hossza 5 GB, és az élettartam értéke 1 percre állítja be:</span><span class="sxs-lookup"><span data-stu-id="d947d-115">The following example sets the maximum queue size to 5 GB, and the TTL value to 1 minute:</span></span>

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a><span data-ttu-id="d947d-116">Üzenetek küldése egy üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="d947d-116">Send messages to a queue</span></span>
<span data-ttu-id="d947d-117">Egy üzenet küldhető egy Service Bus-üzenetsorba, az alkalmazás hívások a `send_queue_message` metódust a **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d947d-117">To send a message to a Service Bus queue, your application calls the `send_queue_message` method on the **ServiceBusService** object.</span></span>

<span data-ttu-id="d947d-118">A következő példa bemutatja, hogyan tesztüzenet küldése az üzenetsorba nevű `taskqueue` használatával `send_queue_message`:</span><span class="sxs-lookup"><span data-stu-id="d947d-118">The following example demonstrates how to send a test message to the queue named `taskqueue` using `send_queue_message`:</span></span>

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="d947d-119">A Service Bus-üzenetsorok a [Standard csomagban](service-bus-premium-messaging.md) legfeljebb 256 KB, a [Prémium csomagban](service-bus-premium-messaging.md) legfeljebb 1 MB méretű üzeneteket támogatnak.</span><span class="sxs-lookup"><span data-stu-id="d947d-119">Service Bus queues support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="d947d-120">A szabványos és az egyéni alkalmazástulajdonságokat tartalmazó fejléc mérete legfeljebb 64 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="d947d-120">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="d947d-121">Az üzenetsorban tárolt üzenetek száma korlátlan, az üzenetsor által tárolt üzenetek teljes mérete azonban korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="d947d-121">There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue.</span></span> <span data-ttu-id="d947d-122">Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d947d-122">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="d947d-123">Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="d947d-123">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="d947d-124">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="d947d-124">Receive messages from a queue</span></span>
<span data-ttu-id="d947d-125">Üzenetek fogadása egy várólista használja a `receive_queue_message` metódust a **ServiceBusService** objektum:</span><span class="sxs-lookup"><span data-stu-id="d947d-125">Messages are received from a queue using the `receive_queue_message` method on the **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="d947d-126">Üzenetek törlődnek a várólistából, mikor olvasott a paramétere `peek_lock` értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d947d-126">Messages are deleted from the queue as they are read when the parameter `peek_lock` is set to **False**.</span></span> <span data-ttu-id="d947d-127">(A betekintés) olvashatja, és az üzenet törlése az üzenetsorból úgy, hogy a paraméter nélkül zárolása `peek_lock` való **igaz**.</span><span class="sxs-lookup"><span data-stu-id="d947d-127">You can read (peek) and lock the message without deleting it from the queue by setting the parameter `peek_lock` to **True**.</span></span>

<span data-ttu-id="d947d-128">Olvasási és az üzenet törlése a fogadási művelet részeként viselkedését a legegyszerűbb modell, és működik a legjobban az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet meghibásodása.</span><span class="sxs-lookup"><span data-stu-id="d947d-128">The behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="d947d-129">Ennek megértéséhez képzeljen el egy forgatókönyvet, amelyben a fogyasztó kiad egy fogadási kérést, majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="d947d-129">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="d947d-130">Mivel a Service Bus csak fel az üzenetet, feldolgozottként, majd az alkalmazás újraindításakor és megkezdése az üzenetek fel újra, amikor ki fogja hagyni a az összeomlás előtt feldolgozott üzenetet.</span><span class="sxs-lookup"><span data-stu-id="d947d-130">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="d947d-131">Ha a `peek_lock` paraméter értéke **igaz**, a receive két szakaszból álló művelet, amely lehetővé teszi az alkalmazások támogatását, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="d947d-131">If the `peek_lock` parameter is set to **True**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="d947d-132">Amikor a Service Bus fogad egy kérést, megkeresi és zárolja a következő feldolgozandó üzenetet, hogy más fogyasztók ne tudják fogadni, majd visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="d947d-132">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="d947d-133">A fogadási folyamat második szintjére meghívásával befejezése után az alkalmazás befejezi az üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), a **törlése** metódust a **üzenet** objektum.</span><span class="sxs-lookup"><span data-stu-id="d947d-133">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling the **delete** method on the **Message** object.</span></span> <span data-ttu-id="d947d-134">A **törlése** metódus lesz használnak az üzenetet, és távolítsa el a sorból.</span><span class="sxs-lookup"><span data-stu-id="d947d-134">The **delete** method will mark the message as being consumed and remove it from the queue.</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="d947d-135">Az alkalmazás-összeomlások és nem olvasható üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="d947d-135">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="d947d-136">A Service Bus olyan funkciókat biztosít, amelyekkel zökkenőmentesen helyreállíthatja az alkalmazás hibáit vagy az üzenetek feldolgozásának nehézségeit.</span><span class="sxs-lookup"><span data-stu-id="d947d-136">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="d947d-137">Ha egy fogadó alkalmazás nem tudja feldolgozni az üzenetet valamilyen okból kifolyólag, majd akkor meghívhatja a **zárolásának feloldásához** metódust a **üzenet** objektum.</span><span class="sxs-lookup"><span data-stu-id="d947d-137">If a receiver application is unable to process the message for some reason, then it can call the **unlock** method on the **Message** object.</span></span> <span data-ttu-id="d947d-138">Ennek hatására a Service Bus feloldja az üzenet a várólistában zárolását, és lehetővé teszi a felhasználó az alkalmazás vagy egy másik fogyasztó alkalmazás általi ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="d947d-138">This will cause Service Bus to unlock the message within the queue and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="d947d-139">Még nincs hozzárendelve az üzenetsorban lévő időtúllépés, és ha az alkalmazás nem tudja feldolgozni az üzenetet, mielőtt a zárolás időtúllépését lejárta (pl., ha az alkalmazás összeomlik), akkor a Service Bus automatikusan feloldja az üzenet zárolását, és meg fogja teszi elérhető ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="d947d-139">There is also a timeout associated with a message locked within the queue, and if the application fails to process the message before the lock timeout expires (e.g., if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="d947d-140">Abban az esetben, ha az alkalmazás összeomlik, de előtte az üzenet feldolgozása után a **törlése** metódust, akkor az üzenet újból kézbesítve lesz az alkalmazás amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="d947d-140">In the event that the application crashes after processing the message but before the **delete** method is called, then the message will be redelivered to the application when it restarts.</span></span> <span data-ttu-id="d947d-141">Ezt gyakran nevezik **legalább egyszeri feldolgozásnak**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben a a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="d947d-141">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="d947d-142">Ha a forgatókönyvben nem lehetségesek a duplikált üzenetek, akkor az alkalmazásfejlesztőnek további logikát kell az alkalmazásba építenie az üzenetek ismételt kézbesítésének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d947d-142">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="d947d-143">Ez gyakran érhető el, használja a **MessageId** az üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="d947d-143">This is often achieved using the **MessageId** property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d947d-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d947d-144">Next steps</span></span>
<span data-ttu-id="d947d-145">Most, hogy rendelkezik megismerte a Service Bus-üzenetsorok alapjait, tanulmányozza a további.</span><span class="sxs-lookup"><span data-stu-id="d947d-145">Now that you have learned the basics of Service Bus queues, see these articles to learn more.</span></span>

* <span data-ttu-id="d947d-146">[Üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="d947d-146">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

