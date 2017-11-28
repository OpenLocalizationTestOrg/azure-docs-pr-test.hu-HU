---
title: "Service Bus-üzenettémakörök (Ruby) használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja a Service Bus-üzenettémák és előfizetések az Azure-ban. Kódminták Ruby alkalmazásokhoz íródtak."
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
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a><span data-ttu-id="a9665-104">Service Bus-üzenettémák és előfizetések használata Ruby</span><span class="sxs-lookup"><span data-stu-id="a9665-104">How to use Service Bus topics and subscriptions with Ruby</span></span>
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<span data-ttu-id="a9665-105">A cikkből megtudhatja, hogyan használhatja a Service Bus-üzenettémák és előfizetések Ruby alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="a9665-105">This article describes how to use Service Bus topics and subscriptions from Ruby applications.</span></span> <span data-ttu-id="a9665-106">Az ismertetett forgatókönyvek **üzenettémák és előfizetések üzenetküldésre, előfizetés-szűrők létrehozása létrehozása** egy témakörbe **üzenetek fogadása egy előfizetésből**, és **törlése üzenettémák és előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="a9665-106">The scenarios covered include **creating topics and subscriptions, creating subscription filters, sending messages** to a topic, **receiving messages from a subscription**, and **deleting topics and subscriptions**.</span></span> <span data-ttu-id="a9665-107">A témakörök és előfizetések további információkért lásd: a [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="a9665-107">For more information on topics and subscriptions, see the [Next Steps](#next-steps) section.</span></span>

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a><span data-ttu-id="a9665-108">Üzenettémakör létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9665-108">Create a topic</span></span>
<span data-ttu-id="a9665-109">A **Azure::ServiceBusService** objektum lehetővé teszi a témakörök együttműködni.</span><span class="sxs-lookup"><span data-stu-id="a9665-109">The **Azure::ServiceBusService** object enables you to work with topics.</span></span> <span data-ttu-id="a9665-110">Az alábbi kód létrehoz egy **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-110">The following code creates an **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="a9665-111">Létrehoz egy témát, használja a `create_topic()` metódust.</span><span class="sxs-lookup"><span data-stu-id="a9665-111">To create a topic, use the `create_topic()` method.</span></span> <span data-ttu-id="a9665-112">Az alábbi példa létrehoz egy üzenettémakört vagy jelenít meg a hibákat, ha vannak ilyenek.</span><span class="sxs-lookup"><span data-stu-id="a9665-112">The following example creates a topic or prints out the errors if there are any.</span></span>

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

<span data-ttu-id="a9665-113">Is átadhatja egy **Azure::ServiceBus::Topic** további beállításokat, amelyek lehetővé teszik a például az üzenet time to live vagy maximális Várólistaméret alapértelmezett témakör beállításainak felülbírálása objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-113">You can also pass an **Azure::ServiceBus::Topic** object with additional options, which enable you to override default topic settings such as message time to live or maximum queue size.</span></span> <span data-ttu-id="a9665-114">A következő példa bemutatja, hogy a várólista maximális hossza 1 percre élő 5 GB és idő beállítása:</span><span class="sxs-lookup"><span data-stu-id="a9665-114">The following example shows setting the maximum queue size to 5 GB and time to live to 1 minute:</span></span>

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a><span data-ttu-id="a9665-115">Előfizetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9665-115">Create subscriptions</span></span>
<span data-ttu-id="a9665-116">Üzenettémakör-előfizetéseket is jönnek létre a **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-116">Topic subscriptions are also created with the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="a9665-117">Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amelyek az előfizetés virtuális üzenetsorában kézbesített üzenetek korlátoz.</span><span class="sxs-lookup"><span data-stu-id="a9665-117">Subscriptions are named and can have an optional filter that restricts the set of messages delivered to the subscription's virtual queue.</span></span>

<span data-ttu-id="a9665-118">Előfizetések állandó, és továbbra is létezik, vagy csak, vagy a következő témakörben társítva, a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="a9665-118">Subscriptions are persistent and will continue to exist until either they, or the topic they are associated with, are deleted.</span></span> <span data-ttu-id="a9665-119">Ha az alkalmazás előfizetés létrehozása a logikát tartalmaz, azt kell először ellenőrizze, hogy az előfizetés már léteznek getSubscription módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="a9665-119">If your application contains logic to create a subscription, it should first check if the subscription already exists by using the getSubscription method.</span></span>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a><span data-ttu-id="a9665-120">Előfizetés létrehozása az alapértelmezett (MatchAll) szűrővel</span><span class="sxs-lookup"><span data-stu-id="a9665-120">Create a subscription with the default (MatchAll) filter</span></span>
<span data-ttu-id="a9665-121">A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a9665-121">The **MatchAll** filter is the default filter that is used if no filter is specified when a new subscription is created.</span></span> <span data-ttu-id="a9665-122">Ha a **MatchAll** szűrővel, a témakörbe közzétett összes üzenetet kerülnek, az előfizetés virtuális üzenetsorában.</span><span class="sxs-lookup"><span data-stu-id="a9665-122">When the **MatchAll** filter is used, all messages published to the topic are placed in the subscription's virtual queue.</span></span> <span data-ttu-id="a9665-123">Az alábbi példában a "minden-üzenetek" nevű előfizetést hoz létre, és használja az alapértelmezett **MatchAll** szűrő.</span><span class="sxs-lookup"><span data-stu-id="a9665-123">The following example creates a subscription named "all-messages" and uses the default **MatchAll** filter.</span></span>

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a><span data-ttu-id="a9665-124">Előfizetések létrehozása szűrőkkel</span><span class="sxs-lookup"><span data-stu-id="a9665-124">Create subscriptions with filters</span></span>
<span data-ttu-id="a9665-125">Is meghatározhat, amelyek lehetővé teszik, hogy adja meg, hogy szűrők egy témakörbe küldött üzenetek egy adott előfizetésen belül kell megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="a9665-125">You can also define filters that enable you to specify which messages sent to a topic should show up within a specific subscription.</span></span>

<span data-ttu-id="a9665-126">A szűrő előfizetések által támogatott legrugalmasabb típusú a **Azure::ServiceBus::SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="a9665-126">The most flexible type of filter supported by subscriptions is the **Azure::ServiceBus::SqlFilter**, which implements a subset of SQL92.</span></span> <span data-ttu-id="a9665-127">Az SQL-szűrők az üzenettémába közzétett üzenetek tulajdonságain működnek.</span><span class="sxs-lookup"><span data-stu-id="a9665-127">SQL filters operate on the properties of the messages that are published to the topic.</span></span> <span data-ttu-id="a9665-128">Az SQL-szűrőkkel használható kifejezésekkel kapcsolatos további információkért tekintse át a [SqlFilter](service-bus-messaging-sql-filter.md) szintaxist.</span><span class="sxs-lookup"><span data-stu-id="a9665-128">For more details about the expressions that can be used with a SQL filter, review the [SqlFilter](service-bus-messaging-sql-filter.md) syntax.</span></span>

<span data-ttu-id="a9665-129">Előfizetés szűrők a segítségével hozzáadhatók a `create_rule()` metódusában a **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-129">You can add filters to a subscription by using the `create_rule()` method of the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="a9665-130">Ez a módszer lehetővé teszi új szűrők hozzáadása egy meglévő előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a9665-130">This method enables you to add new filters to an existing subscription.</span></span>

<span data-ttu-id="a9665-131">Mivel minden új előfizetés automatikusan alkalmazza az alapértelmezett szűrő, távolítsa el az alapértelmezett szűrő, vagy a **MatchAll** felülírják más szűrőket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="a9665-131">Since the default filter is applied automatically to all new subscriptions, you must first remove the default filter, or the **MatchAll** will override any other filters you may specify.</span></span> <span data-ttu-id="a9665-132">Az alapértelmezett szabály eltávolításához használja a `delete_rule()` metódust a **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-132">You can remove the default rule by using the `delete_rule()` method on the **Azure::ServiceBusService** object.</span></span>

<span data-ttu-id="a9665-133">A következő példa egy "jó-üzenetek" nevű előfizetést hoz létre egy **Azure::ServiceBus::SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `message_number` tulajdonságának értéke nagyobb, mint 3:</span><span class="sxs-lookup"><span data-stu-id="a9665-133">The following example creates a subscription named "high-messages" with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a custom `message_number` property greater than 3:</span></span>

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

<span data-ttu-id="a9665-134">Hasonlóképpen, a következő példa egy nevű előfizetést hoz létre `low-messages` rendelkező egy **Azure::ServiceBus::SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `message_number` tulajdonságának értéke kisebb vagy egyenlő, mint 3:</span><span class="sxs-lookup"><span data-stu-id="a9665-134">Similarly, the following example creates a subscription named `low-messages` with an **Azure::ServiceBus::SqlFilter** that only selects messages that have a `message_number` property less than or equal to 3:</span></span>

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

<span data-ttu-id="a9665-135">Ha egy most üzenettel `test-topic`, mindig kell biztosítását előfizetett fogadó számára a `all-messages` üzenettémakör-előfizetésre, és szelektív módon kézbesíti a a `high-messages` és `low-messages` üzenettémakör-előfizetéseket (attól függően, esetén az üzenet tartalma).</span><span class="sxs-lookup"><span data-stu-id="a9665-135">When a message is now sent to `test-topic`, it is always be delivered to receivers subscribed to the `all-messages` topic subscription, and selectively delivered to receivers subscribed to the `high-messages` and `low-messages` topic subscriptions (depending upon the message content).</span></span>

## <a name="send-messages-to-a-topic"></a><span data-ttu-id="a9665-136">Üzenetek küldése egy üzenettémakörbe</span><span class="sxs-lookup"><span data-stu-id="a9665-136">Send messages to a topic</span></span>
<span data-ttu-id="a9665-137">A Service Bus-témakörbe való üzenetküldéshez az alkalmazást kell használnia a `send_topic_message()` metódust a **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-137">To send a message to a Service Bus topic, your application must use the `send_topic_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="a9665-138">Service Bus-üzenettémakörök küldött üzenetek példánya a **Azure::ServiceBus::BrokeredMessage** objektumok.</span><span class="sxs-lookup"><span data-stu-id="a9665-138">Messages sent to Service Bus topics are instances of the **Azure::ServiceBus::BrokeredMessage** objects.</span></span> <span data-ttu-id="a9665-139">**Azure::ServiceBus::BrokeredMessage** objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például `label` és `time_to_live`), amellyel az egyéni alkalmazásspecifikus tulajdonságokat, valamint egy karakterláncadatokat törzsét.</span><span class="sxs-lookup"><span data-stu-id="a9665-139">**Azure::ServiceBus::BrokeredMessage** objects have a set of standard properties (such as `label` and `time_to_live`), a dictionary that is used to hold custom application-specific properties, and a body of string data.</span></span> <span data-ttu-id="a9665-140">Az alkalmazás beállíthatja az üzenet törzsét úgy, hogy a karakterlánc-érték a `send_topic_message()` metódus, és minden szükséges szabványos tulajdonságkészlettel alapértelmezett értékek szerint lesz kitöltve.</span><span class="sxs-lookup"><span data-stu-id="a9665-140">An application can set the body of the message by passing a string value to the `send_topic_message()` method and any required standard properties will be populated by default values.</span></span>

<span data-ttu-id="a9665-141">A következő példa bemutatja, hogyan küldhető öt tesztüzenet az `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="a9665-141">The following example demonstrates how to send five test messages to `test-topic`.</span></span> <span data-ttu-id="a9665-142">Vegye figyelembe, hogy a `message_number` minden üzenetet egyéni tulajdonság értékének a az a ciklus ismétléseinek számától függően változik (Ez határozza meg, melyik előfizetéssel fogadó):</span><span class="sxs-lookup"><span data-stu-id="a9665-142">Note that the `message_number` custom property value of each message varies on the iteration of the loop (this determines which subscription receives it):</span></span>

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

<span data-ttu-id="a9665-143">A Service Bus-üzenettémakörök a [Standard csomagban](service-bus-premium-messaging.md) legfeljebb 256 KB, a [Prémium csomagban](service-bus-premium-messaging.md) legfeljebb 1 MB méretű üzeneteket támogatnak.</span><span class="sxs-lookup"><span data-stu-id="a9665-143">Service Bus topics support a maximum message size of 256 KB in the [Standard tier](service-bus-premium-messaging.md) and 1 MB in the [Premium tier](service-bus-premium-messaging.md).</span></span> <span data-ttu-id="a9665-144">A szabványos és az egyéni alkalmazástulajdonságokat tartalmazó fejléc mérete legfeljebb 64 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="a9665-144">The header, which includes the standard and custom application properties, can have a maximum size of 64 KB.</span></span> <span data-ttu-id="a9665-145">A témakörökben tárolt üzenetek száma korlátlan, a témakörök által tárolt üzenetek teljes mérete azonban korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="a9665-145">There is no limit on the number of messages held in a topic but there is a cap on the total size of the messages held by a topic.</span></span> <span data-ttu-id="a9665-146">A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.</span><span class="sxs-lookup"><span data-stu-id="a9665-146">This topic size is defined at creation time, with an upper limit of 5 GB.</span></span>

## <a name="receive-messages-from-a-subscription"></a><span data-ttu-id="a9665-147">Üzenetek fogadása egy előfizetés</span><span class="sxs-lookup"><span data-stu-id="a9665-147">Receive messages from a subscription</span></span>
<span data-ttu-id="a9665-148">Üzenetek fogadása egy előfizetés használatával a `receive_subscription_message()` metódust a **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-148">Messages are received from a subscription using the `receive_subscription_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="a9665-149">Alapértelmezés szerint az üzenetek read(peak) és anélkül, hogy törölné az előfizetés zárolva van.</span><span class="sxs-lookup"><span data-stu-id="a9665-149">By default, messages are read(peak) and locked without deleting it from the subscription.</span></span> <span data-ttu-id="a9665-150">Olvassa el, és törölje az üzenetet az előfizetésből úgy, hogy a `peek_lock` lehetőséggel **hamis**.</span><span class="sxs-lookup"><span data-stu-id="a9665-150">You can read and delete the message from the subscription by setting the `peek_lock` option to **false**.</span></span>

<span data-ttu-id="a9665-151">Az alapértelmezett viselkedés lehetővé teszi az olvasási és egy kétszakaszos művelet, ami is lehetővé teszi az alkalmazások, amelyek működését zavarják a hiányzó üzenetek támogatásához törlése.</span><span class="sxs-lookup"><span data-stu-id="a9665-151">The default behavior makes the reading and deleting a two-stage operation, which also makes it possible to support applications that cannot tolerate missing messages.</span></span> <span data-ttu-id="a9665-152">Amikor a Service Bus fogad egy kérést, megkeresi és zárolja a következő feldolgozandó üzenetet, hogy más fogyasztók ne tudják fogadni, majd visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="a9665-152">When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application.</span></span> <span data-ttu-id="a9665-153">Miután az alkalmazás befejezi az üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja a második a fogadási folyamat meghívásával `delete_subscription_message()` metódus és a törlendő paraméterként message.</span><span class="sxs-lookup"><span data-stu-id="a9665-153">After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling `delete_subscription_message()` method and providing the message to be deleted as a parameter.</span></span> <span data-ttu-id="a9665-154">A `delete_subscription_message()` metódus lesz használnak az üzenetet, és távolítsa el az előfizetésből.</span><span class="sxs-lookup"><span data-stu-id="a9665-154">The `delete_subscription_message()` method will mark the message as being consumed and remove it from the subscription.</span></span>

<span data-ttu-id="a9665-155">Ha a `:peek_lock` paraméter értéke **hamis**, olvasása, és törli az üzenetet a legegyszerűbb modell válik, és működik a legjobban forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="a9665-155">If the `:peek_lock` parameter is set to **false**, reading and deleting the message becomes the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure.</span></span> <span data-ttu-id="a9665-156">Ennek megértéséhez képzeljen el egy forgatókönyvet, amelyben a fogyasztó kiad egy fogadási kérést, majd összeomlik a feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="a9665-156">To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it.</span></span> <span data-ttu-id="a9665-157">Mivel a Service Bus csak fel az üzenetet, feldolgozottként, majd az alkalmazás újraindításakor és megkezdése az üzenetek fel újra, amikor ki fogja hagyni a az összeomlás előtt feldolgozott üzenetet.</span><span class="sxs-lookup"><span data-stu-id="a9665-157">Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.</span></span>

<span data-ttu-id="a9665-158">A következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával `receive_subscription_message()`.</span><span class="sxs-lookup"><span data-stu-id="a9665-158">The following example demonstrates how messages can be received and processed using `receive_subscription_message()`.</span></span> <span data-ttu-id="a9665-159">A példa első kap, és törli az üzenetét a `low-messages` használatával előfizetés `:peek_lock` beállítása **hamis**, majd egy másik üzenetet kap a `high-messages` majd törli az üzenet `delete_subscription_message()`:</span><span class="sxs-lookup"><span data-stu-id="a9665-159">The example first receives and deletes a message from the `low-messages` subscription by using `:peek_lock` set to **false**, then it receives another message from the `high-messages` and then deletes the message using `delete_subscription_message()`:</span></span>

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a><span data-ttu-id="a9665-160">Az alkalmazás-összeomlások és nem olvasható üzenetek kezelése</span><span class="sxs-lookup"><span data-stu-id="a9665-160">How to handle application crashes and unreadable messages</span></span>
<span data-ttu-id="a9665-161">A Service Bus olyan funkciókat biztosít, amelyekkel zökkenőmentesen helyreállíthatja az alkalmazás hibáit vagy az üzenetek feldolgozásának nehézségeit.</span><span class="sxs-lookup"><span data-stu-id="a9665-161">Service Bus provides functionality to help you gracefully recover from errors in your application or difficulties processing a message.</span></span> <span data-ttu-id="a9665-162">Ha egy fogadó alkalmazás nem tudja feldolgozni az üzenetet valamilyen okból kifolyólag, majd akkor meghívhatja a `unlock_subscription_message()` metódust a **Azure::ServiceBusService** objektum.</span><span class="sxs-lookup"><span data-stu-id="a9665-162">If a receiver application is unable to process the message for some reason, then it can call the `unlock_subscription_message()` method on the **Azure::ServiceBusService** object.</span></span> <span data-ttu-id="a9665-163">Ennek hatására a Service Bus feloldja az üzenet zárolását az előfizetésen belül, és lehetővé teszi az ugyanazon vagy egy másik fogyasztó alkalmazás általi ismételt fogadását.</span><span class="sxs-lookup"><span data-stu-id="a9665-163">This causes Service Bus to unlock the message within the subscription and make it available to be received again, either by the same consuming application or by another consuming application.</span></span>

<span data-ttu-id="a9665-164">Emellett van lévő az előfizetéshez társított időtúllépés, és ha az alkalmazás nem tudja feldolgozni az üzenetet, mielőtt a zárolás időtúllépését lejárta (például, ha az alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást az üzenet és tegye elérhetővé az újbóli fogadását.</span><span class="sxs-lookup"><span data-stu-id="a9665-164">There is also a timeout associated with a message locked within the subscription, and if the application fails to process the message before the lock timeout expires (for example, if the application crashes), then Service Bus will unlock the message automatically and make it available to be received again.</span></span>

<span data-ttu-id="a9665-165">Abban az esetben, ha az alkalmazás összeomlik, de előtte az üzenet feldolgozása után a `delete_subscription_message()` metódust, akkor az üzenet újból kézbesítve az alkalmazásnak, amikor újraindul.</span><span class="sxs-lookup"><span data-stu-id="a9665-165">In the event that the application crashes after processing the message but before the `delete_subscription_message()` method is called, then the message is redelivered to the application when it restarts.</span></span> <span data-ttu-id="a9665-166">Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben előfordulhat ugyanazon üzenet előfordulhat, hogy újbóli kézbesítése..</span><span class="sxs-lookup"><span data-stu-id="a9665-166">This is often called *At Least Once Processing*; that is, each message will be processed at least once but in certain situations the same message may be redelivered.</span></span> <span data-ttu-id="a9665-167">Ha a forgatókönyvben nem lehetségesek a duplikált üzenetek, akkor az alkalmazásfejlesztőnek további logikát kell az alkalmazásba építenie az üzenetek ismételt kézbesítésének kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a9665-167">If the scenario cannot tolerate duplicate processing, then application developers should add additional logic to their application to handle duplicate message delivery.</span></span> <span data-ttu-id="a9665-168">Ezt gyakran érhető el, használja a `message_id` az üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="a9665-168">This logic is often achieved using the `message_id` property of the message, which will remain constant across delivery attempts.</span></span>

## <a name="delete-topics-and-subscriptions"></a><span data-ttu-id="a9665-169">Témakörök és előfizetések törlése</span><span class="sxs-lookup"><span data-stu-id="a9665-169">Delete topics and subscriptions</span></span>
<span data-ttu-id="a9665-170">Üzenettémák és előfizetések következetesek, és explicit módon kell lennie vagy törölte a [Azure-portálon] [ Azure portal] vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="a9665-170">Topics and subscriptions are persistent, and must be explicitly deleted either through the [Azure portal][Azure portal] or programmatically.</span></span> <span data-ttu-id="a9665-171">Az alábbi példa bemutatja, hogyan nevű üzenettémakör törlése `test-topic`.</span><span class="sxs-lookup"><span data-stu-id="a9665-171">The example below demonstrates how to delete the topic named `test-topic`.</span></span>

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

<span data-ttu-id="a9665-172">Egy témakör törlése az adott témakörre regisztrált összes előfizetést is törli.</span><span class="sxs-lookup"><span data-stu-id="a9665-172">Deleting a topic also deletes any subscriptions that are registered with the topic.</span></span> <span data-ttu-id="a9665-173">Az előfizetések független módon is törölhetők.</span><span class="sxs-lookup"><span data-stu-id="a9665-173">Subscriptions can also be deleted independently.</span></span> <span data-ttu-id="a9665-174">A következő kód bemutatja, hogyan nevű előfizetés törlése `high-messages` a a `test-topic` témakör:</span><span class="sxs-lookup"><span data-stu-id="a9665-174">The following code demonstrates how to delete the subscription named `high-messages` from the `test-topic` topic:</span></span>

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a><span data-ttu-id="a9665-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9665-175">Next steps</span></span>
<span data-ttu-id="a9665-176">Most, hogy megismerte a Service Bus-üzenettémakörök alapjait, az alábbi hivatkozásokból további.</span><span class="sxs-lookup"><span data-stu-id="a9665-176">Now that you've learned the basics of Service Bus topics, follow these links to learn more.</span></span>

* <span data-ttu-id="a9665-177">Lásd: [üzenetsorok, témakörök és előfizetések](service-bus-queues-topics-subscriptions.md).</span><span class="sxs-lookup"><span data-stu-id="a9665-177">See [Queues, topics, and subscriptions](service-bus-queues-topics-subscriptions.md).</span></span>
* <span data-ttu-id="a9665-178">Az [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter) API-referenciája.</span><span class="sxs-lookup"><span data-stu-id="a9665-178">API reference for [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter).</span></span>
* <span data-ttu-id="a9665-179">Látogasson el a [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="a9665-179">Visit the [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) repository on GitHub.</span></span>

[Azure portal]: https://portal.azure.com
