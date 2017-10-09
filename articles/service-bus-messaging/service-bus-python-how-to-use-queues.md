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
# <a name="how-toouse-service-bus-queues-with-python"></a>Hogyan toouse Service Bus várólisták Python

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez a cikk ismerteti, hogyan toouse Service Bus-üzenetsorok. hello minták Python nyelven íródtak, és használja a hello [Python Azure Service Bus csomag][Python Azure Service Bus package]. hello tárgyalt forgatókönyvekben szerepel a **üzenetsorok üzenetek küldése és fogadása létrehozása**, és **várólisták törlése**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> Python vagy hello tooinstall [Python Azure Service Bus csomag][Python Azure Service Bus package], lásd: hello [Python a telepítési útmutató](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Üzenetsor létrehozása
Hello **ServiceBusService** objektum lehetővé teszi az üzenetsorok toowork. Adja hozzá a következő kód szinte bármilyen Python fájl tooprogrammatically access Service Bus kívánja hello felső hello:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

hello alábbi kód létrehoz egy **ServiceBusService** objektum. Cserélje le `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` a névtér, közös hozzáférésű jogosultságkód (SAS) kulcs nevét és értékét.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

hello hello SAS-kulcs nevét és értékét található hello [Azure-portálon] [ Azure portal] kapcsolatadatok, vagy a Visual Studio hello **tulajdonságok** ablaktábla kiválasztása hello a Server Explorer Service Bus-névtér (hello előző részben leírtak szerint).

```python
bus_service.create_queue('taskqueue')
```

Hello `create_queue` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a toooverride alapértelmezett várólista-beállításokat például üzenet ideje toolive (TTL) vagy a várólista maximális hossza. hello alábbi mintakód hello várólista maximális mérete too5 GB és hello TTL érték too1 perc:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-tooa-queue"></a>Üzenetek tooa várólista küldése
toosend üzenet tooa Service Bus-üzenetsorba, az alkalmazás meghívja hello `send_queue_message` hello metódusa **ServiceBusService** objektum.

hello következő példa bemutatja, hogyan toosend egy teszt üzenetsor toohello nevű `taskqueue` használatával `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus-üzenetsorok támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. Az üzenetsorban tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello üzenetsor által tárolt hello üzenetek teljes mérete. Az üzenetsor ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB. Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Üzenetek fogadása egy üzenetsorból
Üzenetek fogadása egy üzenetsorból hello segítségével `receive_queue_message` hello metódusa **ServiceBusService** objektum:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Üzenetek törlődnek hello sorából, amikor olvasott paraméter hello `peek_lock` értéke túl**hamis**. (A betekintés) olvashatja és üdvözlőüzenetére zárolási beállítás hello paraméterrel hello várólista törlése nélkül `peek_lock` túl**igaz**.

hello olvasási viselkedését, és üdvözlőüzenetére törlése, hello részét fogadási művelethez hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

Ha hello `peek_lock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata által hívó hello **törlése** hello metódusa **üzenet** objektum. Hello **törlése** módszert fog hello üzenetet feldolgozottként és eltávolításában hello várólista.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello **zárolásának feloldásához** hello metódusa **üzenet** objektum. Ezzel a Service Bus toounlock hello üzenet hello várólistában és révén elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van hello várólistában lévő társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess hello előtt hello zárolás időtúllépését lejárta (pl. Ha hello alkalmazás összeomlik), akkor a Service Bus automatikusan feloldást köszönőüzenetei és a rendelkezésre álló toobe újbóli fogadását.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **törlése** metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul. Ezt gyakran nevezik **legalább egyszeri feldolgozásnak**, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenetsorok alapjait hello rendelkezik, tekintse meg a további cikkek toolearn.

* [Üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

