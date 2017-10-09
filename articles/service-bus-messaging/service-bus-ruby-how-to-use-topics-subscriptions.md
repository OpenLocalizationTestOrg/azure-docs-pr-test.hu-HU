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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-ruby"></a>Hogyan toouse Service Bus üzenettémák és előfizetések a Rubyhoz
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések Ruby alkalmazásokból. hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések üzenetküldésre, előfizetés-szűrők létrehozása létrehozása** tooa témakör **üzenetek fogadása egy előfizetésből**, és  **üzenettémák és előfizetések törlése**. A témakörök és előfizetések további információkért lásd: hello [lépések](#next-steps) szakasz.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Üzenettémakör létrehozása
Hello **Azure::ServiceBusService** objektum teszi lehetővé a témakörök toowork. hello alábbi kód létrehoz egy **Azure::ServiceBusService** objektum. a témakör toocreate hello használata `create_topic()` metódust. a következő példa hello létrehoz egy üzenettémakört, vagy nyomtatása hello hibát jelez, ha vannak ilyenek.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Is átadhatja egy **Azure::ServiceBus::Topic** objektum további beállításokat, amelyek lehetővé teszik a témakör alapbeállítások toooverride például üzenet ideje toolive és maximális várólista-méret. hello alábbi példa azt mutatja meg a várólista maximális hello beállítása too5 GB méretet és toolive too1 perc idő:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Előfizetések létrehozása
Üzenettémakör-előfizetéseket is jönnek létre hello **Azure::ServiceBusService** objektum. Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amely korlátozza a hello toohello előfizetés virtuális üzenetsorában kézbesített üzenetek készletét.

Előfizetések állandó, és amíg vagy azok továbbra is tooexist, vagy hello témakör azok tartoznak, a rendszer törli. Ha az alkalmazás logika toocreate előfizetés tartalmaz, akkor először győződjön meg ha hello már létezik az előfizetés hello getSubscription módszer használatával.

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel
Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor. Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek hello előfizetés virtuális üzenetsorában. hello alábbi példa egy nevű előfizetést hoz létre "minden-üzenetek", és használja az alapértelmezett hello **MatchAll** szűrő.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések létrehozása szűrőkkel
Megadhatja a szűrőket, amelyek lehetővé teszik a toospecify üzenetet küldött tooa témakör kell jelenik meg egy adott előfizetésen belül.

hello szűrő előfizetések által támogatott legrugalmasabb típusú hello **Azure::ServiceBus::SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg. Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik. Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további tudnivalókért tekintse meg a hello [SqlFilter](service-bus-messaging-sql-filter.md) szintaxist.

Szűrők tooa előfizetés hello használatával adhat hozzá `create_rule()` hello metódusában **Azure::ServiceBusService** objektum. Ez a módszer lehetővé teszi tooadd új szűrők tooan meglévő előfizetéshez.

Mivel hello alapértelmezett szűrőhöz automatikusan tooall új előfizetésekre, először távolítsa el az alapértelmezett szűrő hello, vagy hello **MatchAll** felülírják más szűrőket is megadhat. Alapértelmezett szabály hello távolíthatja hello használatával `delete_rule()` hello metódusa **Azure::ServiceBusService** objektum.

hello alábbi példakód létrehozza a "nagy-üzenetek" nevű előfizetést egy **Azure::ServiceBus::SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `message_number` tulajdonságának értéke nagyobb, mint 3:

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

Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `low-messages` rendelkező egy **Azure::ServiceBus::SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `message_number` tulajdonságának értéke kisebb vagy egyenlő, mint too3:

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

Amikor egy most üzenettel túl`test-topic`, fontos, hogy mindig lesz fizetve kézbesített tooreceivers toohello `all-messages` üzenettémakör-előfizetésre, és szelektív módon kézbesíti az előfizetett tooreceivers toohello `high-messages` és `low-messages` témakör előfizetések () attól függően, hello üzenettartalom).

## <a name="send-messages-tooa-topic"></a>Üzenetek tooa témakör küldése
toosend üzenet tooa Service Bus-témakörbe, az alkalmazásnak használnia kell a hello `send_topic_message()` hello metódusa **Azure::ServiceBusService** objektum. Üzenetek küldése tooService Bus-üzenettémakörök hello példánya **Azure::ServiceBus::BrokeredMessage** objektumok. **Azure::ServiceBus::BrokeredMessage** objektumok rendelkeznek egy szabványos tulajdonságkészlettel (például `label` és `time_to_live`), amely használt toohold egyéni alkalmazásspecifikus tulajdonságokat, valamint egy karakterlánc típusú adatok törzsét. Az alkalmazás beállíthatja hello hello üzenet törzsét egy karakterlánc-érték toohello átadásával `send_topic_message()` metódus, és minden szükséges szabványos tulajdonságkészlettel alapértelmezett értékek szerint lesz kitöltve.

hello következő példa bemutatja, hogyan toosend öt teszt túl üzenetek`test-topic`. Vegye figyelembe, hogy hello `message_number` egyes üzenetek egyéni tulajdonság értékének a hello iterációs hello hurok változik (Ez határozza meg, melyik előfizetéssel fogadó):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete. A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Üzenetek fogadása egy előfizetés
Üzenetek fogadása egy előfizetésből hello segítségével `receive_subscription_message()` hello metódusa **Azure::ServiceBusService** objektum. Alapértelmezés szerint az üzenetek read(peak) és anélkül, hogy törölné hello előfizetés zárolva van. Olvassa el, és törölje üdvözlőüzenetére hello előfizetésből hello beállítása `peek_lock` beállítás túl**hamis**.

hello alapértelmezett viselkedés teszi hello olvasása és törlése egy kétszakaszos művelet, így az is lehetséges toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `delete_subscription_message()` metódus és a hello üzenet toobe paraméterként törölték. Hello `delete_subscription_message()` metódus lesz hello üzenetet feldolgozottként, és távolítsa el a hello előfizetés.

Ha hello `:peek_lock` paraméter értéke túl**hamis**, olvasási és üdvözlőüzenetére törlése hello legegyszerűbb modell válik, és működik a legjobban forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hello esetén egy Hiba történt. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

hello következő példa bemutatja, hogyan lehet üzeneteket fogadni, és a feldolgozott használatával `receive_subscription_message()`. hello például először kap, és törli az üzenet hello `low-messages` használatával előfizetés `:peek_lock` túl beállítása**hamis**, majd egy másik üzenetet kap hello `high-messages` és majd törlések hello üzenet használatával `delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlock_subscription_message()` hello metódusa **Azure::ServiceBusService** objektum. A Service Bus toounlock hello üzenet hello előfizetésen belül, és tegye elérhetővé toobe ismételt fogadását oka vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van lévő hello előfizetéssel társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldást köszönőüzenetei automatikusan, és teszi elérhetővé toobe újbóli fogadását.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `delete_subscription_message()` metódus lehívásra kerül, majd hello üzenet újbóli kézbesítése toohello alkalmazás amikor újraindul. Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*; Ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ezt gyakran elért hello segítségével `message_id` hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.

## <a name="delete-topics-and-subscriptions"></a>Témakörök és előfizetések törlése
Üzenettémák és előfizetések következetesek, és explicit módon kell törölni vagy hello [Azure-portálon] [ Azure portal] vagy programon keresztül. hello az alábbi példa bemutatja, hogyan toodelete hello témakör nevű `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Egy témakör törlése a hello témakörben regisztrált előfizetésekkel is törli. Az előfizetések független módon is törölhetők. hello következő kód bemutatja, hogyan toodelete hello előfizetés nevű `high-messages` a hello `test-topic` témakör:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenettémakörök hello alapjait, kövesse az alábbi hivatkozások további toolearn.

* Lásd: [üzenetsorok, témakörök és előfizetések](service-bus-queues-topics-subscriptions.md).
* Az [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter) API-referenciája.
* A Microsoft hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) GitHub tárházából.

[Azure portal]: https://portal.azure.com
