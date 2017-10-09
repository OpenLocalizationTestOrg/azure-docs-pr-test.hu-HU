---
title: "Python várólisták aaaHow toouse Azure Service Bus |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Service Bus várólisták a Python."
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
ms.openlocfilehash: bceb84d04ff3445c3087a9c246c583d6630f07af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-python"></a><span data-ttu-id="80373-103">Hogyan toouse Service Bus várólisták Python</span><span class="sxs-lookup"><span data-stu-id="80373-103">How toouse Service Bus queues with Python</span></span>

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

<span data-ttu-id="80373-104">Ez a cikk ismerteti, hogyan toouse Service Bus-üzenetsorok.</span><span class="sxs-lookup"><span data-stu-id="80373-104">This article describes how toouse Service Bus queues.</span></span> <span data-ttu-id="80373-105">hello minták Python nyelven íródtak, és használja a hello [Python Azure Service Bus csomag][Python Azure Service Bus package].</span><span class="sxs-lookup"><span data-stu-id="80373-105">hello samples are written in Python and use hello [Python Azure Service Bus package][Python Azure Service Bus package].</span></span> <span data-ttu-id="80373-106">hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok üzenetek küldése és fogadása létrehozása**, és **várólisták törlése**.</span><span class="sxs-lookup"><span data-stu-id="80373-106">hello scenarios covered include **creating queues, sending and receiving messages**, and **deleting queues**.</span></span>

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> <span data-ttu-id="80373-107">Python vagy hello tooinstall [Python Azure Service Bus csomag][Python Azure Service Bus package], lásd: hello [Python a telepítési útmutató](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="80373-107">tooinstall Python or hello [Python Azure Service Bus package][Python Azure Service Bus package], see hello [Python Installation Guide](../python-how-to-install.md).</span></span>
> 
> 

## <a name="create-a-queue"></a><span data-ttu-id="80373-108">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="80373-108">Create a queue</span></span>
<span data-ttu-id="80373-109">Hello **ServiceBusService** objektum lehetővé teszi az üzenetsorok toowork.</span><span class="sxs-lookup"><span data-stu-id="80373-109">hello **ServiceBusService** object enables you toowork with queues.</span></span> <span data-ttu-id="80373-110">Adja hozzá a következő kód szinte bármilyen Python fájl tooprogrammatically access Service Bus kívánja hello felső hello:</span><span class="sxs-lookup"><span data-stu-id="80373-110">Add hello following code near hello top of any Python file in which you wish tooprogrammatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

<span data-ttu-id="80373-111">hello alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="80373-111">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="80373-112">Cserélje le `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` a névtér, közös hozzáférésű jogosultságkód (SAS) kulcs nevét és értékét.</span><span class="sxs-lookup"><span data-stu-id="80373-112">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your namespace, shared access signature (SAS) key name, and value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="80373-113">hello hello SAS-kulcs nevét és értékét található hello [Azure-portálon] [ Azure portal] kapcsolatadatok, vagy a Visual Studio hello **tulajdonságok** ablaktábla kiválasztása hello a Server Explorer Service Bus-névtér (hello előző részben leírtak szerint).</span><span class="sxs-lookup"><span data-stu-id="80373-113">hello values for hello SAS key name and value can be found in hello [Azure portal][Azure portal] connection information, or in hello Visual Studio **Properties** pane when selecting hello Service Bus namespace in Server Explorer (as shown in hello previous section).</span></span>

```python
bus_service.create_queue('taskqueue')
```

<span data-ttu-id="80373-114">Hello `create_queue` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a toooverride alapértelmezett várólista-beállításokat például üzenet ideje toolive (TTL) vagy a várólista maximális hossza.</span><span class="sxs-lookup"><span data-stu-id="80373-114">hello `create_queue` method also supports additional options, which enable you toooverride default queue settings such as message time toolive (TTL) or maximum queue size.</span></span> <span data-ttu-id="80373-115">hello alábbi mintakód hello várólista maximális mérete too5 GB és hello TTL érték too1 perc:</span><span class="sxs-lookup"><span data-stu-id="80373-115">hello following example sets hello maximum queue size too5 GB, and hello TTL value too1 minute:</span></span>

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-tooa-queue"></a><span data-ttu-id="80373-116">Üzenetek tooa várólista küldése</span><span class="sxs-lookup"><span data-stu-id="80373-116">Send messages tooa queue</span></span>
<span data-ttu-id="80373-117">toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `send_queue_message` hello metódusa **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="80373-117">toosend a message tooa Service Bus queue, your application calls hello `send_queue_message` method on hello **ServiceBusService** object.</span></span>

<span data-ttu-id="80373-118">hello következő példa bemutatja, hogyan toosend egy teszt üzenetsor toohello nevű `taskqueue` használatával `send_queue_message`:</span><span class="sxs-lookup"><span data-stu-id="80373-118">hello following example demonstrates how toosend a test message toohello queue named `taskqueue` using `send_queue_message`:</span></span>

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="80373-119">Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="80373-119">Service Bus queues support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="80373-120">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="80373-120">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="80373-121">Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="80373-121">There is no limit on hello number of messages held in a queue but there is a cap on hello total size of hello messages held by a queue.</span></span> <span data-ttu-id="80373-122">Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="80373-122">This queue size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="80373-123">Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="80373-123">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-queue"></a><span data-ttu-id="80373-124">Üzenetek fogadása egy üzenetsorból</span><span class="sxs-lookup"><span data-stu-id="80373-124">Receive messages from a queue</span></span>
<span data-ttu-id="80373-125">Üzenetek fogadása egy üzenetsorból hello segítségével `receive_queue_message` hello metódusa **ServiceBusService** objektum:</span><span class="sxs-lookup"><span data-stu-id="80373-125">Messages are received from a queue using hello `receive_queue_message` method on hello **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="80373-126">Üzenetek törlődnek hello sorából, amikor olvasott paraméter hello `peek_lock` értéke túl**hamis**.</span><span class="sxs-lookup"><span data-stu-id="80373-126">Messages are deleted from hello queue as they are read when hello parameter `peek_lock` is set too**False**.</span></span> <span data-ttu-id="80373-127">(A betekintés) olvashatja és üdvözlőüzenetére zárolási beállítás hello paraméterrel hello várólista törlése nélkül `peek_lock` túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="80373-127">You can read (peek) and lock hello message without deleting it from hello queue by setting hello parameter `peek_lock` too**True**.</span></span>

<span data-ttu-id="80373-128">hello olvasási viselkedését, és üdvözlőüzenetére törlése, hello részét fogadási művelethez hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="80373-128">hello behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="80373-129">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="80373-129">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="80373-130">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="80373-130">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="80373-131">Ha hello `peek_lock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="80373-131">If hello `peek_lock` parameter is set too**True**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="80373-132">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="80373-132">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="80373-133">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata által hívó hello **törlése** hello metódusa **üzenet** objektum.</span><span class="sxs-lookup"><span data-stu-id="80373-133">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling hello **delete** method on hello **Message** object.</span></span> <span data-ttu-id="80373-134">Hello **törlése** módszert fog hello üzenetet feldolgozottként és eltávolításában hello várólista.</span><span class="sxs-lookup"><span data-stu-id="80373-134">hello **delete** method will mark hello message as being consumed and remove it from hello queue.</span></span>

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="80373-135">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="80373-135">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="80373-136">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="80373-136">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="80373-137">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello **zárolásának feloldásához** hello metódusa **üzenet** objektum.</span><span class="sxs-lookup"><span data-stu-id="80373-137">If a receiver application is unable tooprocess hello message for some reason, then it can call hello **unlock** method on hello **Message** object.</span></span> <span data-ttu-id="80373-138">Ezzel a Service Bus toounlock hello üzenet hello várólistában és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="80373-138">This will cause Service Bus toounlock hello message within hello queue and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="80373-139">Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess hello előtt hello zárolás időtúllépését lejárta (pl. Ha hello alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást köszönőüzenetei és a rendelkezésre álló toobe újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="80373-139">There is also a timeout associated with a message locked within hello queue, and if hello application fails tooprocess hello message before hello lock timeout expires (e.g., if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="80373-140">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **törlése** metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="80373-140">In hello event that hello application crashes after processing hello message but before hello **delete** method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="80373-141">Ezt gyakran nevezik **legalább egyszeri feldolgozásnak**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="80373-141">This is often called **At Least Once Processing**, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="80373-142">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="80373-142">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="80373-143">Ez gyakran érhető el hello segítségével **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="80373-143">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80373-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80373-144">Next steps</span></span>
<span data-ttu-id="80373-145">Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello rendelkezik, tekintse meg a további cikkek toolearn.</span><span class="sxs-lookup"><span data-stu-id="80373-145">Now that you have learned hello basics of Service Bus queues, see these articles toolearn more.</span></span>

* <span data-ttu-id="80373-146">[Üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions]</span><span class="sxs-lookup"><span data-stu-id="80373-146">[Queues, topics, and subscriptions][Queues, topics, and subscriptions]</span></span>

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

