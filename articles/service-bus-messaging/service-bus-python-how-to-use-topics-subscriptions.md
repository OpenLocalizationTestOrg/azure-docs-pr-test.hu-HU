---
title: "aaaHow toouse Azure Service Bus-üzenettémakörök Python |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Service Bus üzenettémák és előfizetések a Python."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 1171cbe8061bb3d73e2ce92ecc0cf45afae37054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-python"></a><span data-ttu-id="d74c0-103">Hogyan toouse Service Bus üzenettémák és előfizetések Python</span><span class="sxs-lookup"><span data-stu-id="d74c0-103">How toouse Service Bus topics and subscriptions with Python</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="d74c0-104">Ez a cikk ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések.</span><span class="sxs-lookup"><span data-stu-id="d74c0-104">This article describes how toouse Service Bus topics and subscriptions.</span></span> <span data-ttu-id="d74c0-105">hello minták Python nyelven íródtak, és használja a hello [Azure Python SDK csomag][Azure Python package].</span><span class="sxs-lookup"><span data-stu-id="d74c0-105">hello samples are written in Python and use hello [Azure Python SDK package][Azure Python package].</span></span> <span data-ttu-id="d74c0-106">hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **tooa témakör üzenetek küldésekor**, **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**.</span><span class="sxs-lookup"><span data-stu-id="d74c0-106">hello scenarios covered include **creating topics and subscriptions**, **creating subscription filters**, **sending messages tooa topic**, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="d74c0-107">Üzenettémakörökkel és előfizetésekkel kapcsolatos további információkért lásd: hello [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="d74c0-107">For more information about topics and subscriptions, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> <span data-ttu-id="d74c0-108">Ha tooinstall Python vagy hello kell [Azure Python-csomag][Azure Python package], lásd: hello [Python a telepítési útmutató](../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="d74c0-108">If you need tooinstall Python or hello [Azure Python package][Azure Python package], see hello [Python Installation Guide](../python-how-to-install.md).</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="d74c0-109">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="d74c0-109">Create a topic</span></span>
<span data-ttu-id="d74c0-110">Hello **ServiceBusService** objektum teszi lehetővé a témakörök toowork.</span><span class="sxs-lookup"><span data-stu-id="d74c0-110">hello **ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="d74c0-111">Adja hozzá a hello következő közelében hello felső bármely Python fájl tooprogrammatically access Service Bus kívánja:</span><span class="sxs-lookup"><span data-stu-id="d74c0-111">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Service Bus:</span></span>

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

<span data-ttu-id="d74c0-112">hello alábbi kód létrehoz egy **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-112">hello following code creates a **ServiceBusService** object.</span></span> <span data-ttu-id="d74c0-113">Cserélje le `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` a tényleges névterű közös hozzáférésű Jogosultságkód (SAS) kulcs nevét, és a kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="d74c0-113">Replace `mynamespace`, `sharedaccesskeyname`, and `sharedaccesskey` with your actual namespace, Shared Access Signature (SAS) key name, and key value.</span></span>

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

<span data-ttu-id="d74c0-114">Hello értékeinek hello SAS-kulcs nevét és értékét beszerezheti hello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="d74c0-114">You can obtain hello values for hello SAS key name and value from hello [Azure portal][Azure portal].</span></span>

```python
bus_service.create_topic('mytopic')
```

<span data-ttu-id="d74c0-115">Hello `create_topic` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a toooverride alapértelmezett témakör beállítások például üzenet ideje toolive vagy maximális témakör ezen méretét.</span><span class="sxs-lookup"><span data-stu-id="d74c0-115">hello `create_topic` method also supports additional options, which enable you toooverride default topic settings such as message time toolive or maximum topic size.</span></span> <span data-ttu-id="d74c0-116">hello alábbi mintakód hello maximális témakör mérete too5 GB, és egy időt toolive (TTL) értéke 1 perc:</span><span class="sxs-lookup"><span data-stu-id="d74c0-116">hello following example sets hello maximum topic size too5 GB, and a time toolive (TTL) value of 1 minute:</span></span>

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a><span data-ttu-id="d74c0-117">Előfizetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="d74c0-117">Create subscriptions</span></span>
<span data-ttu-id="d74c0-118">Előfizetések tootopics is jönnek létre hello **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-118">Subscriptions tootopics are also created with hello **ServiceBusService** object.</span></span> <span data-ttu-id="d74c0-119">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amely korlátozza a hello toohello előfizetés virtuális üzenetsorában kézbesített üzenetek készletét.</span><span class="sxs-lookup"><span data-stu-id="d74c0-119">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

> [!NOTE]
> <span data-ttu-id="d74c0-120">Előfizetések állandó, és amíg vagy azok továbbra is tooexist, vagy hello témakör toowhich azok előfizetett, a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="d74c0-120">Subscriptions are persistent and will continue tooexist until either they, or hello topic toowhich they are subscribed, are deleted.</span></span>
> 
> 

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="d74c0-121">Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="d74c0-121">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="d74c0-122">Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="d74c0-122">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="d74c0-123">Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek hello előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="d74c0-123">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="d74c0-124">hello alábbi példa egy nevű előfizetést hoz létre `AllMessages` és által használt alapértelmezett hello **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="d74c0-124">hello following example creates a subscription named `AllMessages` and uses hello default **MatchAll** filter.</span></span>

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="d74c0-125">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="d74c0-125">Create subscriptions with filters</span></span>
<span data-ttu-id="d74c0-126">Megadhatja a szűrőket, amelyek lehetővé teszik a toospecify üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="d74c0-126">You can also define filters that enable you toospecify which messages sent tooa topic should show up within a specific topic subscription.</span></span>

<span data-ttu-id="d74c0-127">hello szűrő előfizetések által támogatott legrugalmasabb típusú van egy **SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="d74c0-127">hello most flexible type of filter supported by subscriptions is a **SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="d74c0-128">Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik.</span><span class="sxs-lookup"><span data-stu-id="d74c0-128">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="d74c0-129">Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további információkért lásd: hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.</span><span class="sxs-lookup"><span data-stu-id="d74c0-129">For more information about hello expressions that can be used with a SQL filter, see hello [SqlFilter.SqlExpression][SqlFilter.SqlExpression] syntax.</span></span>

<span data-ttu-id="d74c0-130">Szűrők tooa előfizetés hello használatával adhat hozzá **létrehozása\_szabály** hello metódusában **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-130">You can add filters tooa subscription by using hello **create\_rule** method of hello **ServiceBusService** object.</span></span> <span data-ttu-id="d74c0-131">Ez a módszer lehetővé teszi tooadd új szűrők tooan meglévő előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d74c0-131">This method allows you tooadd new filters tooan existing subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d74c0-132">Mert hello alapértelmezett szűrőhöz automatikusan tooall új előfizetésekre, el kell távolítania hello alapértelmezett szűrő vagy hello **MatchAll** felülírják más szűrők adhatnak meg.</span><span class="sxs-lookup"><span data-stu-id="d74c0-132">Because hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter or hello **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="d74c0-133">Alapértelmezett szabály hello távolíthatja hello használatával `delete_rule` hello metódusában **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-133">You can remove hello default rule by using hello `delete_rule` method of hello **ServiceBusService** object.</span></span>
> 
> 

<span data-ttu-id="d74c0-134">hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `messagenumber` tulajdonságának értéke nagyobb, mint 3:</span><span class="sxs-lookup"><span data-stu-id="d74c0-134">hello following example creates a subscription named `HighMessages` with a **SqlFilter** that only selects messages that have a custom `messagenumber` property greater than 3:</span></span>

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="d74c0-135">Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy **SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `messagenumber` tulajdonságának értéke kisebb vagy egyenlő, mint too3:</span><span class="sxs-lookup"><span data-stu-id="d74c0-135">Similarly, hello following example creates a subscription named `LowMessages` with a **SqlFilter** that only selects messages that have a `messagenumber` property less than or equal too3:</span></span>

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

<span data-ttu-id="d74c0-136">Most, amikor egy üzenetet kap túl`mytopic` mindig biztosítását előfizetett tooreceivers toohello **AllMessages** üzenettémakör-előfizetésre, és szelektív módon kézbesíti az előfizetett tooreceivers toohello **HighMessages**  és **LowMessages** (attól függően, hogy hello üzenettartalom) üzenettémakör-előfizetéseket.</span><span class="sxs-lookup"><span data-stu-id="d74c0-136">Now, when a message is sent too`mytopic` it is always delivered tooreceivers subscribed toohello **AllMessages** topic subscription, and selectively delivered tooreceivers subscribed toohello **HighMessages** and **LowMessages** topic subscriptions (depending on hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="d74c0-137">Üzenetek tooa témakör küldése</span><span class="sxs-lookup"><span data-stu-id="d74c0-137">Send messages tooa topic</span></span>
<span data-ttu-id="d74c0-138">toosend üzenet tooa Service Bus-témakörbe, az alkalmazásnak használnia kell a hello `send_topic_message` hello metódusában **ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-138">toosend a message tooa Service Bus topic, your application must use hello `send_topic_message` method of hello **ServiceBusService** object.</span></span>

<span data-ttu-id="d74c0-139">hello következő példa bemutatja, hogyan toosend öt teszt túl üzenetek`mytopic`.</span><span class="sxs-lookup"><span data-stu-id="d74c0-139">hello following example demonstrates how toosend five test messages too`mytopic`.</span></span> <span data-ttu-id="d74c0-140">Vegye figyelembe, hogy hello `messagenumber` egyes üzenetek tulajdonság értéke a hello iterációs hello hurok változik (Ez határozza meg, melyik előfizetések fogják megkapni):</span><span class="sxs-lookup"><span data-stu-id="d74c0-140">Note that hello `messagenumber` property value of each message varies on hello iteration of hello loop (this determines which subscriptions receive it):</span></span>

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

<span data-ttu-id="d74c0-141">Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="d74c0-141">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="d74c0-142">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="d74c0-142">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="d74c0-143">A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="d74c0-143">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="d74c0-144">A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="d74c0-144">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span> <span data-ttu-id="d74c0-145">Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].</span><span class="sxs-lookup"><span data-stu-id="d74c0-145">For more information about quotas, see [Service Bus quotas][Service Bus quotas].</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="d74c0-146">Üzenetek fogadása egy előfizetés</span><span class="sxs-lookup"><span data-stu-id="d74c0-146">Receive messages from a subscription</span></span>
<span data-ttu-id="d74c0-147">Üzenetek fogadása egy előfizetésből hello segítségével `receive_subscription_message` hello metódusa **ServiceBusService** objektum:</span><span class="sxs-lookup"><span data-stu-id="d74c0-147">Messages are received from a subscription using hello `receive_subscription_message` method on hello **ServiceBusService** object:</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

<span data-ttu-id="d74c0-148">Az üzenetek törlődnek hello előfizetésből, mikor olvasott paraméter hello `peek_lock` értéke túl**hamis**.</span><span class="sxs-lookup"><span data-stu-id="d74c0-148">Messages are deleted from hello subscription as they are read when hello parameter `peek_lock` is set too**False**.</span></span> <span data-ttu-id="d74c0-149">(A betekintés) olvashatja és üdvözlőüzenetére zárolási beállítás hello paraméterrel hello várólista törlése nélkül `peek_lock` túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="d74c0-149">You can read (peek) and lock hello message without deleting it from hello queue by setting hello parameter `peek_lock` too**True**.</span></span>

<span data-ttu-id="d74c0-150">hello olvasási viselkedését, és üdvözlőüzenetére törlése, hello részét fogadási művelethez hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban.</span><span class="sxs-lookup"><span data-stu-id="d74c0-150">hello behavior of reading and deleting hello message as part of hello receive operation is hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="d74c0-151">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="d74c0-151">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="d74c0-152">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="d74c0-152">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="d74c0-153">Ha hello `peek_lock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz.</span><span class="sxs-lookup"><span data-stu-id="d74c0-153">If hello `peek_lock` parameter is set too**True**, hello receive becomes a two stage operation, which makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="d74c0-154">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="d74c0-154">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="d74c0-155">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `delete` hello metódusa **üzenet** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-155">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete` method on hello **Message** object.</span></span> <span data-ttu-id="d74c0-156">Hello `delete` metódus hello üzenetet, feldolgozottként jelöli meg, és eltávolítja a hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d74c0-156">hello `delete` method marks hello message as being consumed and removes it from hello subscription.</span></span>

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="d74c0-157">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="d74c0-157">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="d74c0-158">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="d74c0-158">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="d74c0-159">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlock` hello metódusa **üzenet** objektum.</span><span class="sxs-lookup"><span data-stu-id="d74c0-159">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock` method on hello **Message** object.</span></span> <span data-ttu-id="d74c0-160">Ezzel a Service Bus toounlock hello üzenet hello előfizetésen belül, és könnyebben elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="d74c0-160">This will cause Service Bus toounlock hello message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="d74c0-161">Emellett van lévő hello előfizetéssel társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldja köszönőüzenetei automatikusan és lehetővé teszi az újbóli fogadását elérhető toobe.</span><span class="sxs-lookup"><span data-stu-id="d74c0-161">There is also a timeout associated with a message locked within hello subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus unlocks hello message automatically and makes it available toobe received again.</span></span>

<span data-ttu-id="d74c0-162">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `delete` metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="d74c0-162">In hello event that hello application crashes after processing hello message but before hello `delete` method is called, then hello message will be redelivered toohello application when it restarts.</span></span> <span data-ttu-id="d74c0-163">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="d74c0-163">This is often called *At Least Once Processing*, that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="d74c0-164">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="d74c0-164">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="d74c0-165">Ez gyakran érhető el hello segítségével **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="d74c0-165">This is often achieved using hello **MessageId** property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="d74c0-166">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="d74c0-166">Delete topics and subscriptions</span></span>
<span data-ttu-id="d74c0-167">Üzenettémák és előfizetések következetesek, és explicit módon kell törölni vagy hello [Azure-portálon] [ Azure portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d74c0-167">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="d74c0-168">hello következő példa bemutatja, hogyan toodelete hello témakör nevű `mytopic`:</span><span class="sxs-lookup"><span data-stu-id="d74c0-168">hello following example shows how toodelete hello topic named `mytopic`:</span></span>

```python
bus_service.delete_topic('mytopic')
```

<span data-ttu-id="d74c0-169">Egy témakör törlése a hello témakörben regisztrált előfizetésekkel is törli.</span><span class="sxs-lookup"><span data-stu-id="d74c0-169">Deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="d74c0-170">Az előfizetések független módon is törölhetők.</span><span class="sxs-lookup"><span data-stu-id="d74c0-170">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="d74c0-171">hello következő kód bemutatja, hogyan toodelete előfizetés nevű `HighMessages` a hello `mytopic` témakör:</span><span class="sxs-lookup"><span data-stu-id="d74c0-171">hello following code shows how toodelete a subscription named `HighMessages` from hello `mytopic` topic:</span></span>

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a><span data-ttu-id="d74c0-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d74c0-172">Next steps</span></span>
<span data-ttu-id="d74c0-173">Most, hogy megismerte a Service Bus-üzenettémakörök hello alapjait, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="d74c0-173">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="d74c0-174">Lásd: [üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions].</span><span class="sxs-lookup"><span data-stu-id="d74c0-174">See [Queues, topics, and subscriptions][Queues, topics, and subscriptions].</span></span>
* <span data-ttu-id="d74c0-175">A hivatkozási [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span><span class="sxs-lookup"><span data-stu-id="d74c0-175">Reference for [SqlFilter.SqlExpression][SqlFilter.SqlExpression].</span></span>

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
