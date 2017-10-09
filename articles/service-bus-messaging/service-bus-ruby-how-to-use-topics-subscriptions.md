---
title: "aaaHow toouse Service Bus-üzenettémakörök (Ruby) |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Service Bus üzenettémák és előfizetések az Azure-ban. Kódminták Ruby alkalmazásokhoz íródtak."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 236d6495825e68e336c23e1b500d0764ee512e49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-ruby"></a><span data-ttu-id="1c173-104">Hogyan toouse Service Bus üzenettémák és előfizetések a Rubyhoz</span><span class="sxs-lookup"><span data-stu-id="1c173-104">How toouse Service Bus topics and subscriptions with Ruby</span></span>
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="1c173-105">Ez a cikk ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések Ruby alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="1c173-105">This article describes how toouse Service Bus topics and subscriptions from Ruby applications.</span></span> <span data-ttu-id="1c173-106">hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések üzenetküldésre, előfizetés-szűrők létrehozása létrehozása** tooa témakör **üzenetek fogadása egy előfizetésből**, és  **üzenettémák és előfizetések törlése**.</span><span class="sxs-lookup"><span data-stu-id="1c173-106">hello scenarios covered include **creating topics and subscriptions, creating subscription filters, sending messages** tooa topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="1c173-107">A témakörök és előfizetések további információkért lásd: hello [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="1c173-107">For more information on topics and subscriptions, see hello [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a><span data-ttu-id="1c173-108">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c173-108">Create a topic</span></span>
<span data-ttu-id="1c173-109">Hello **Azure::ServiceBusService** objektum teszi lehetővé a témakörök toowork.</span><span class="sxs-lookup"><span data-stu-id="1c173-109">hello **Azure::ServiceBusService** object enables you toowork with topics.</span></span> <span data-ttu-id="1c173-110">hello alábbi kód létrehoz egy **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-110">hello following code creates an **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1c173-111">a témakör toocreate hello használata `create_topic()` metódust.</span><span class="sxs-lookup"><span data-stu-id="1c173-111">toocreate a topic, use hello `create_topic()` method.</span></span> <span data-ttu-id="1c173-112">a következő példa hello létrehoz egy üzenettémakört, vagy nyomtatása hello hibát jelez, ha vannak ilyenek.</span><span class="sxs-lookup"><span data-stu-id="1c173-112">hello following example creates a topic or prints out hello errors if there are any.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

<span data-ttu-id="1c173-113">Is átadhatja egy **Azure::ServiceBus::Topic** objektum további beállításokat, amelyek lehetővé teszik a témakör alapbeállítások toooverride például üzenet ideje toolive és maximális várólista-méret.</span><span class="sxs-lookup"><span data-stu-id="1c173-113">You can also pass an **Azure::ServiceBus::Topic** object with additional options, which enable you toooverride default topic settings such as message time toolive or maximum queue size.</span></span> <span data-ttu-id="1c173-114">hello alábbi példa azt mutatja meg a várólista maximális hello beállítása too5 GB méretet és toolive too1 perc idő:</span><span class="sxs-lookup"><span data-stu-id="1c173-114">hello following example shows setting hello maximum queue size too5 GB and time toolive too1 minute:</span></span>

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a><span data-ttu-id="1c173-115">Előfizetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c173-115">Create subscriptions</span></span>
<span data-ttu-id="1c173-116">Üzenettémakör-előfizetéseket is jönnek létre hello **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-116">Topic subscriptions are also created with hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1c173-117">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amely korlátozza a hello toohello előfizetés virtuális üzenetsorában kézbesített üzenetek készletét.</span><span class="sxs-lookup"><span data-stu-id="1c173-117">Subscriptions are named and can have an optional filter that restricts hello set of messages delivered toohello subscription's virtual queue.</span></span>

<span data-ttu-id="1c173-118">Előfizetések állandó, és amíg vagy azok továbbra is tooexist, vagy hello témakör azok tartoznak, a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="1c173-118">Subscriptions are persistent and will continue tooexist until either they, or hello topic they are associated with, are deleted.</span></span> <span data-ttu-id="1c173-119">Ha az alkalmazás logika toocreate előfizetés tartalmaz, akkor először győződjön meg ha hello már létezik az előfizetés hello getSubscription módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="1c173-119">If your application contains logic toocreate a subscription, it should first check if hello subscription already exists by using hello getSubscription method.</span></span>

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a><span data-ttu-id="1c173-120">Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="1c173-120">Create a subscription with hello default (MatchAll) filter</span></span>
<span data-ttu-id="1c173-121">Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1c173-121">hello **MatchAll** filter is hello default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="1c173-122">Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek hello előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="1c173-122">When hello **MatchAll** filter is used, all messages published toohello topic are placed in hello subscription's virtual queue.</span></span> <span data-ttu-id="1c173-123">hello alábbi példa egy nevű előfizetést hoz létre "minden-üzenetek", és használja az alapértelmezett hello **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="1c173-123">hello following example creates a subscription named "all-messages" and uses hello default **MatchAll** filter.</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="1c173-124">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="1c173-124">Create subscriptions with filters</span></span>
<span data-ttu-id="1c173-125">Megadhatja a szűrőket, amelyek lehetővé teszik a toospecify üzenetet küldött tooa témakör kell jelenik meg egy adott előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="1c173-125">You can also define filters that enable you toospecify which messages sent tooa topic should show up within a specific subscription.</span></span>

<span data-ttu-id="1c173-126">hello szűrő előfizetések által támogatott legrugalmasabb típusú hello **Azure::ServiceBus::SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="1c173-126">hello most flexible type of filter supported by subscriptions is hello **Azure::ServiceBus::SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="1c173-127">Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik.</span><span class="sxs-lookup"><span data-stu-id="1c173-127">SQL filters operate on hello properties of hello messages that are published toohello topic.</span></span> <span data-ttu-id="1c173-128">Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további tudnivalókért tekintse meg a hello [SqlFilter](service-bus-messaging-sql-filter.md) szintaxist.</span><span class="sxs-lookup"><span data-stu-id="1c173-128">For more details about hello expressions that can be used with a SQL filter, review hello [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span></span>

<span data-ttu-id="1c173-129">Szűrők tooa előfizetés hello használatával adhat hozzá `create_rule()` hello metódusában **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-129">You can add filters tooa subscription by using hello `create_rule()` method of hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1c173-130">Ez a módszer lehetővé teszi tooadd új szűrők tooan meglévő előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="1c173-130">This method enables you tooadd new filters tooan existing subscription.</span></span>

<span data-ttu-id="1c173-131">Mivel hello alapértelmezett szűrőhöz automatikusan tooall új előfizetésekre, először távolítsa el az alapértelmezett szűrő hello, vagy hello **MatchAll** felülírják más szűrőket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="1c173-131">Since hello default filter is applied automatically tooall new subscriptions, you must first remove hello default filter, or hello **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="1c173-132">Alapértelmezett szabály hello távolíthatja hello használatával `delete_rule()` hello metódusa **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-132">You can remove hello default rule by using hello `delete_rule()` method on hello **Azure::ServiceBusService** object.</span></span>

<span data-ttu-id="1c173-133">hello alábbi példakód létrehozza a "nagy-üzenetek" nevű előfizetést egy **Azure::ServiceBus::SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `message_number` tulajdonságának értéke nagyobb, mint 3:</span><span class="sxs-lookup"><span data-stu-id="1c173-133">hello following example creates a subscription named "high-messages" with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a custom `message_number` property greater than 3:</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

<span data-ttu-id="1c173-134">Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `low-messages` rendelkező egy **Azure::ServiceBus::SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `message_number` tulajdonságának értéke kisebb vagy egyenlő, mint too3:</span><span class="sxs-lookup"><span data-stu-id="1c173-134">Similarly, hello following example creates a subscription named `low-messages` with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a `message_number` property less than or equal too3:</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

<span data-ttu-id="1c173-135">Amikor egy most üzenettel túl`test-topic`, fontos, hogy mindig lesz fizetve kézbesített tooreceivers toohello `all-messages` üzenettémakör-előfizetésre, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `high-messages` és `low-messages` témakör előfizetések () attól függően, hello üzenettartalom).</span><span class="sxs-lookup"><span data-stu-id="1c173-135">When a message is now sent too`test-topic`, it is always be delivered tooreceivers subscribed toohello `all-messages` topic subscription, and selectively delivered tooreceivers subscribed toohello `high-messages` and `low-messages` topic subscriptions (depending upon hello message content).</span></span>

## <a name="send-messages-tooa-topic"></a><span data-ttu-id="1c173-136">Üzenetek tooa témakör küldése</span><span class="sxs-lookup"><span data-stu-id="1c173-136">Send messages tooa topic</span></span>
<span data-ttu-id="1c173-137">toosend üzenet tooa Service Bus-témakörbe, az alkalmazásnak használnia kell a hello `send_topic_message()` hello metódusa **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-137">toosend a message tooa Service Bus topic, your application must use hello `send_topic_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1c173-138">Üzenetek küldése tooService Bus-üzenettémakörök hello példánya **Azure::ServiceBus::BrokeredMessage** objektumok.</span><span class="sxs-lookup"><span data-stu-id="1c173-138">Messages sent tooService Bus topics are instances of hello **Azure::ServiceBus::BrokeredMessage** objects.</span></span> <span data-ttu-id="1c173-139">**Azure::ServiceBus::BrokeredMessage** objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például `label` és `time_to_live`), amely használt toohold egyéni alkalmazásspecifikus tulajdonságokat, valamint egy karakterlánc típusú adatok törzsét.</span><span class="sxs-lookup"><span data-stu-id="1c173-139">**Azure::ServiceBus::BrokeredMessage** objects have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used toohold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="1c173-140">Az alkalmazás beállíthatja hello hello üzenet törzsét egy karakterlánc-érték toohello átadásával `send_topic_message()` metódus, és minden szükséges szabványos tulajdonságkészlettel alapértelmezett értékek szerint lesz kitöltve.</span><span class="sxs-lookup"><span data-stu-id="1c173-140">An application can set hello body of hello message by passing a string value toohello `send_topic_message()` method and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="1c173-141">hello következő példa bemutatja, hogyan toosend öt teszt túl üzenetek`test-topic`.</span><span class="sxs-lookup"><span data-stu-id="1c173-141">hello following example demonstrates how toosend five test messages too`test-topic`.</span></span> <span data-ttu-id="1c173-142">Vegye figyelembe, hogy hello `message_number` egyes üzenetek egyéni tulajdonság értékének a hello iterációs hello hurok változik (Ez határozza meg, melyik előfizetéssel fogadó):</span><span class="sxs-lookup"><span data-stu-id="1c173-142">Note that hello `message_number` custom property value of each message varies on hello iteration of hello loop (this determines which subscription receives it):</span></span>

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

<span data-ttu-id="1c173-143">Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md).</span><span class="sxs-lookup"><span data-stu-id="1c173-143">Service Bus topics support a maximum message size of 256 KB in hello [Standard tier](service-bus-premium-messaging.md) and 1 MB in hello [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="1c173-144">hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="1c173-144">hello header, which includes hello standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="1c173-145">A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete.</span><span class="sxs-lookup"><span data-stu-id="1c173-145">There is no limit on hello number of messages held in a topic but there is a cap on hello total size of hello messages held by a topic.</span></span> <span data-ttu-id="1c173-146">A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="1c173-146">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="1c173-147">Üzenetek fogadása egy előfizetés</span><span class="sxs-lookup"><span data-stu-id="1c173-147">Receive messages from a subscription</span></span>
<span data-ttu-id="1c173-148">Üzenetek fogadása egy előfizetésből hello segítségével `receive_subscription_message()` hello metódusa **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-148">Messages are received from a subscription using hello `receive_subscription_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1c173-149">Alapértelmezés szerint az üzenetek read(peak) és anélkül, hogy törölné hello előfizetés zárolva van.</span><span class="sxs-lookup"><span data-stu-id="1c173-149">By default, messages are read(peak) and locked without deleting it from hello subscription.</span></span> <span data-ttu-id="1c173-150">Olvassa el, és törölje üdvözlőüzenetére hello előfizetésből hello beállítása `peek_lock` beállítás túl**hamis**.</span><span class="sxs-lookup"><span data-stu-id="1c173-150">You can read and delete hello message from hello subscription by setting hello `peek_lock` option too**false**.</span></span>

<span data-ttu-id="1c173-151">hello alapértelmezett viselkedés teszi hello olvasása és törlése egy kétszakaszos művelet, így az is lehetséges toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek.</span><span class="sxs-lookup"><span data-stu-id="1c173-151">hello default behavior makes hello reading and deleting a two-stage operation, which also makes it possible toosupport applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="1c173-152">A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása.</span><span class="sxs-lookup"><span data-stu-id="1c173-152">When Service Bus receives a request, it finds hello next message toobe consumed, locks it tooprevent other consumers receiving it, and then returns it toohello application.</span></span> <span data-ttu-id="1c173-153">Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `delete_subscription_message()` metódus és a hello üzenet toobe paraméterként törölték.</span><span class="sxs-lookup"><span data-stu-id="1c173-153">After hello application finishes processing hello message (or stores it reliably for future processing), it completes hello second stage of hello receive process by calling `delete_subscription_message()` method and providing hello message toobe deleted as a parameter.</span></span> <span data-ttu-id="1c173-154">Hello `delete_subscription_message()` metódus lesz hello üzenetet feldolgozottként, és távolítsa el a hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1c173-154">hello `delete_subscription_message()` method will mark hello message as being consumed and remove it from hello subscription.</span></span>

<span data-ttu-id="1c173-155">Ha hello `:peek_lock` paraméter értéke túl**hamis**, olvasási és üdvözlőüzenetére törlése hello legegyszerűbb modell válik, és működik a legjobban forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hello esetén egy Hiba történt.</span><span class="sxs-lookup"><span data-stu-id="1c173-155">If hello `:peek_lock` parameter is set too**false**, reading and deleting hello message becomes hello simplest model, and works best for scenarios in which an application can tolerate not processing a message in hello event of a failure.</span></span> <span data-ttu-id="1c173-156">toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="1c173-156">toounderstand this, consider a scenario in which hello consumer issues hello receive request and then crashes before processing it.</span></span> <span data-ttu-id="1c173-157">Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.</span><span class="sxs-lookup"><span data-stu-id="1c173-157">Because Service Bus will have marked hello message as being consumed, then when hello application restarts and begins consuming messages again, it will have missed hello message that was consumed prior toohello crash.</span></span>

<span data-ttu-id="1c173-158">hello következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával `receive_subscription_message()`.</span><span class="sxs-lookup"><span data-stu-id="1c173-158">hello following example demonstrates how messages can be received and processed using `receive_subscription_message()`.</span></span> <span data-ttu-id="1c173-159">hello például először kap, és törli az üzenet hello `low-messages` használatával előfizetés `:peek_lock` túl beállítása**hamis**, majd egy másik üzenetet kap hello `high-messages` és majd törlések hello üzenet használatával `delete_subscription_message()`:</span><span class="sxs-lookup"><span data-stu-id="1c173-159">hello example first receives and deletes a message from hello `low-messages` subscription by using `:peek_lock` set too**false**, then it receives another message from hello `high-messages` and then deletes hello message using `delete_subscription_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="1c173-160">Hogyan toohandle omlik össze és nem olvasható üzenetek</span><span class="sxs-lookup"><span data-stu-id="1c173-160">How toohandle application crashes and unreadable messages</span></span>
<span data-ttu-id="1c173-161">Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít.</span><span class="sxs-lookup"><span data-stu-id="1c173-161">Service Bus provides functionality toohelp you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="1c173-162">Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlock_subscription_message()` hello metódusa **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="1c173-162">If a receiver application is unable tooprocess hello message for some reason, then it can call hello `unlock_subscription_message()` method on hello **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="1c173-163">A Service Bus toounlock hello üzenet hello előfizetésen belül, és tegye elérhetővé toobe ismételt fogadását oka vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.</span><span class="sxs-lookup"><span data-stu-id="1c173-163">This causes Service Bus toounlock hello message within hello subscription and make it available toobe received again, either by hello same consuming application or by another consuming application.</span></span>

<span data-ttu-id="1c173-164">Emellett van lévő hello előfizetéssel társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldást köszönőüzenetei automatikusan, és teszi elérhetővé toobe újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="1c173-164">There is also a timeout associated with a message locked within hello subscription, and if hello application fails tooprocess hello message before hello lock timeout expires (for example, if hello application crashes), then Service Bus will unlock hello message automatically and make it available toobe received again.</span></span>

<span data-ttu-id="1c173-165">A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `delete_subscription_message()` metódus lehívásra kerül, majd hello üzenet újbóli kézbesítése toohello alkalmazás amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="1c173-165">In hello event that hello application crashes after processing hello message but before hello `delete_subscription_message()` method is called, then hello message is redelivered toohello application when it restarts.</span></span> <span data-ttu-id="1c173-166">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*; Ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet.</span><span class="sxs-lookup"><span data-stu-id="1c173-166">This is often called *At Least Once Processing*; that is, each message will be processed at least once but in certain situations hello same message may be redelivered.</span></span> <span data-ttu-id="1c173-167">Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését.</span><span class="sxs-lookup"><span data-stu-id="1c173-167">If hello scenario cannot tolerate duplicate processing, then application developers should add additional logic tootheir application toohandle duplicate message delivery.</span></span> <span data-ttu-id="1c173-168">Ezt gyakran elért hello segítségével `message_id` hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="1c173-168">This logic is often achieved using hello `message_id` property of hello message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="1c173-169">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="1c173-169">Delete topics and subscriptions</span></span>
<span data-ttu-id="1c173-170">Üzenettémák és előfizetések következetesek, és explicit módon kell törölni vagy hello [Azure-portálon] [ Azure portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="1c173-170">Topics and subscriptions are persistent, and must be explicitly deleted either through hello [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="1c173-171">hello az alábbi példa bemutatja, hogyan toodelete hello témakör nevű `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="1c173-171">hello example below demonstrates how toodelete hello topic named `test-topic`.</span></span>

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

<span data-ttu-id="1c173-172">Egy témakör törlése a hello témakörben regisztrált előfizetésekkel is törli.</span><span class="sxs-lookup"><span data-stu-id="1c173-172">Deleting a topic also deletes any subscriptions that are registered with hello topic.</span></span> <span data-ttu-id="1c173-173">Az előfizetések független módon is törölhetők.</span><span class="sxs-lookup"><span data-stu-id="1c173-173">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="1c173-174">hello következő kód bemutatja, hogyan toodelete hello előfizetés nevű `high-messages` a hello `test-topic` témakör:</span><span class="sxs-lookup"><span data-stu-id="1c173-174">hello following code demonstrates how toodelete hello subscription named `high-messages` from hello `test-topic` topic:</span></span>

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a><span data-ttu-id="1c173-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c173-175">Next steps</span></span>
<span data-ttu-id="1c173-176">Most, hogy megismerte a Service Bus-üzenettémakörök hello alapjait, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="1c173-176">Now that you've learned hello basics of Service Bus topics, follow these links toolearn more.</span></span>

* <span data-ttu-id="1c173-177">Lásd: [üzenetsorok, témakörök és előfizetések](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="1c173-177">See [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="1c173-178">Az [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter) API-referenciája.</span><span class="sxs-lookup"><span data-stu-id="1c173-178">API reference for [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span></span>
* <span data-ttu-id="1c173-179">A Microsoft hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="1c173-179">Visit hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

[Azure portal]: https://portal.azure.com
