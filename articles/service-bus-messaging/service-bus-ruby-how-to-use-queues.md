---
title: "Azure Service Bus aaaHow toouse várólisták, a Ruby |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Service Bus várólisták az Azure-ban. Kódminták Ruby."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 7270154583f98e3372e82efbb967ea7a5acd1686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-bus-queues-with-ruby"></a>Hogyan toouse Service Bus várólisták, a Ruby

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez az útmutató ismerteti, hogyan toouse Service Bus-üzenetsorok. hello minták Ruby nyelven íródtak, és használja a hello Azure gem. hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok üzenetek küldése és fogadása létrehozása**, és **várólisták törlése**. Service Bus-üzenetsorokkal kapcsolatos további információkért lásd: hello [lépések](#next-steps) szakasz.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-toocreate-a-queue"></a>Hogyan toocreate várólista
Hello **Azure::ServiceBusService** objektum lehetővé teszi az üzenetsorok toowork. egy várólista toocreate hello használata `create_queue()` metódust. hello alábbi példa létrehoz egy üzenetsort vagy jelenít meg az esetleges hibákat.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Is átadhatja a **Azure::ServiceBus::Queue** objektum további beállításokat, amely lehetővé teszi, hogy toooverride hello alapértelmezett várólista-beállításokat, például az üzenet idő toolive és maximális várólista-méret. hello a következő példa bemutatja, hogyan tooset hello várólista maximális mérete too5 GB-e, és toolive too1 perc idő:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-toosend-messages-tooa-queue"></a>Hogyan toosend üzenetek tooa várólista
toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `send_queue_message()` hello metódusa **Azure::ServiceBusService** objektum. Üzenetek túl elküldött (és a fogadott) Service Bus-üzenetsor **Azure::ServiceBus::BrokeredMessage** objektumokat, és rendelkezik egy szabványos tulajdonságkészlettel (például `label` és `time_to_live`), amely használt toohold dictionary egyéni alkalmazásspecifikus tulajdonságokat, és egy tetszőleges alkalmazásadatokból álló törzzsel. Az alkalmazás beállíthatja hello üzenet törzsét hello úgy, hogy a karakterlánc-érték hello üzenetet, és egyéb szabványos tulajdonságokat a rendszer feltölti az alapértelmezett értékekkel.

hello következő példa bemutatja, hogyan toosend egy teszt üzenetsor toohello nevű `test-queue` használatával `send_queue_message()`:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete. Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB.

## <a name="how-tooreceive-messages-from-a-queue"></a>Hogyan tooreceive üzenetek várólistából való várólistából
Üzenetek fogadása egy üzenetsorból hello segítségével `receive_queue_message()` hello metódusa **Azure::ServiceBusService** objektum. Alapértelmezés szerint üzenetek olvasni és zárolni hello várólista törlése nélkül. Azonban is törölnie üzenetek hello várólistából hello beállítása által olvasott `:peek_lock` beállítás túl**hamis**.

hello alapértelmezett viselkedés teszi hello olvasása és törlése egy kétszakaszos művelet, így az is lehetséges toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `delete_queue_message()` metódus és a hello üzenet toobe paraméterként törölték. Hello `delete_queue_message()` módszert fog hello üzenetet feldolgozottként és eltávolításában hello várólista.

Ha hello `:peek_lock` paraméter értéke túl**hamis**, olvasási és üdvözlőüzenetére törlése hello legegyszerűbb modell válik, és működik a legjobban forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet hello esetén egy Hiba történt. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. A Service Bus hello üzenetet használnak, amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel jelölte meg, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

hello következő példa bemutatja, hogyan tooreceive és folyamat-üzenetek használatával `receive_queue_message()`. hello például először kap, és törli az üzenetet a `:peek_lock` túl beállítása**hamis**, majd egy másik üzenetet kap, és ezután törlések hello üzenetet `delete_queue_message()`:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlock_queue_message()` hello metódusa **Azure::ServiceBusService** objektum. A hívás okok Service Bus toounlock hello üzenet-várólista hello belül, és tegye elérhetővé toobe ismételt fogadását vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldja köszönőüzenetei automatikusan és lehetővé teszi az újbóli fogadását elérhető toobe.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `delete_queue_message()` metódus lehívásra kerül, majd hello üzenet újbóli kézbesítése toohello alkalmazás amikor újraindul. Ez a folyamat gyakran nevezik *legalább egyszeri feldolgozásnak*; Ez azt jelenti, hogy minden üzenet legalább egyszer fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével `message_id` hello üzenet, amely állandó marad a kézbesítési kísérletek során tulajdonságát.

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello, kövesse az alábbi hivatkozások további toolearn.

* Áttekintés [üzenetsorok, témakörök és előfizetések](service-bus-queues-topics-subscriptions.md).
* A Microsoft hello [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) GitHub tárházából.

A cikkben szereplő hello Azure Service Bus-üzenetsorok és hello tárgyalt Azure várólisták összehasonlítása [hogyan toouse a Queue storage a Ruby](../storage/queues/storage-ruby-how-to-use-queue-storage.md) cikk című [Azure várólisták és az Azure Service Bus-üzenetsorok - képest és szokásos testhelyzetből](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

