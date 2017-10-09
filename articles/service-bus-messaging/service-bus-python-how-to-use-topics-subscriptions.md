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
# <a name="how-toouse-service-bus-topics-and-subscriptions-with-python"></a>Hogyan toouse Service Bus üzenettémák és előfizetések Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk ismerteti, hogyan toouse Service Bus üzenettémák és előfizetések. hello minták Python nyelven íródtak, és használja a hello [Azure Python SDK csomag][Azure Python package]. hello tárgyalt forgatókönyvekben szerepel a **üzenettémák és előfizetések létrehozása**, **előfizetés-szűrők létrehozása**, **tooa témakör üzenetek küldésekor**, **fogadása előfizetés üzeneteit**, és **üzenettémák és előfizetések törlése**. Üzenettémakörökkel és előfizetésekkel kapcsolatos további információkért lásd: hello [lépések](#next-steps) szakasz.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> Ha tooinstall Python vagy hello kell [Azure Python-csomag][Azure Python package], lásd: hello [Python a telepítési útmutató](../python-how-to-install.md).

## <a name="create-a-topic"></a>Üzenettémakör létrehozása
Hello **ServiceBusService** objektum teszi lehetővé a témakörök toowork. Adja hozzá a hello következő közelében hello felső bármely Python fájl tooprogrammatically access Service Bus kívánja:

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

hello alábbi kód létrehoz egy **ServiceBusService** objektum. Cserélje le `mynamespace`, `sharedaccesskeyname`, és `sharedaccesskey` a tényleges névterű közös hozzáférésű Jogosultságkód (SAS) kulcs nevét, és a kulcs értékét.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Hello értékeinek hello SAS-kulcs nevét és értékét beszerezheti hello [Azure-portálon][Azure portal].

```python
bus_service.create_topic('mytopic')
```

Hello `create_topic` metódus is támogatja a további beállításokat, amelyek lehetővé teszik a toooverride alapértelmezett témakör beállítások például üzenet ideje toolive vagy maximális témakör ezen méretét. hello alábbi mintakód hello maximális témakör mérete too5 GB, és egy időt toolive (TTL) értéke 1 perc:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Előfizetések létrehozása
Előfizetések tootopics is jönnek létre hello **ServiceBusService** objektum. Előfizetések vannak nevezve, és rendelkezhetnek olyan szűrőkkel, amely korlátozza a hello toohello előfizetés virtuális üzenetsorában kézbesített üzenetek készletét.

> [!NOTE]
> Előfizetések állandó, és amíg vagy azok továbbra is tooexist, vagy hello témakör toowhich azok előfizetett, a rendszer törli.
> 
> 

### <a name="create-a-subscription-with-hello-default-matchall-filter"></a>Előfizetés létrehozása hello alapértelmezett (MatchAll) szűrővel
Hello **MatchAll** szűrő hello alapértelmezett szűrő, amely akkor használatos, ha nincs meghatározva szűrő egy új előfizetés létrehozásakor. Ha hello **MatchAll** szűrővel, minden üzenetek közzétett toohello témakör kerülnek hello előfizetés virtuális üzenetsorában. hello alábbi példa egy nevű előfizetést hoz létre `AllMessages` és által használt alapértelmezett hello **MatchAll** szűrő.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések létrehozása szűrőkkel
Megadhatja a szűrőket, amelyek lehetővé teszik a toospecify üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjeleníteni.

hello szűrő előfizetések által támogatott legrugalmasabb típusú van egy **SqlFilter**, amely az SQL92 egy részhalmazát valósítja meg. Az SQL-szűrők hello tulajdonságait, amelyek a közzétett toohello témakör köszönőüzenetei működik. Az SQL-szűrőkkel használható hello kifejezések kapcsolatos további információkért lásd: hello [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] szintaxist.

Szűrők tooa előfizetés hello használatával adhat hozzá **létrehozása\_szabály** hello metódusában **ServiceBusService** objektum. Ez a módszer lehetővé teszi tooadd új szűrők tooan meglévő előfizetéshez.

> [!NOTE]
> Mert hello alapértelmezett szűrőhöz automatikusan tooall új előfizetésekre, el kell távolítania hello alapértelmezett szűrő vagy hello **MatchAll** felülírják más szűrők adhatnak meg. Alapértelmezett szabály hello távolíthatja hello használatával `delete_rule` hello metódusában **ServiceBusService** objektum.
> 
> 

hello alábbi példa egy nevű előfizetést hoz létre `HighMessages` rendelkező egy **SqlFilter** , amely csak választja ki, amelyek egyéni üzenetek `messagenumber` tulajdonságának értéke nagyobb, mint 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Ehhez hasonlóan hello alábbi példa egy nevű előfizetést hoz létre `LowMessages` rendelkező egy **SqlFilter** , amely csak rendelkező üzeneteket választja ki egy `messagenumber` tulajdonságának értéke kisebb vagy egyenlő, mint too3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Most, amikor egy üzenetet kap túl`mytopic` mindig biztosítását előfizetett tooreceivers toohello **AllMessages** üzenettémakör-előfizetésre, és szelektív módon kézbesíti az előfizetett tooreceivers toohello **HighMessages**  és **LowMessages** (attól függően, hogy hello üzenettartalom) üzenettémakör-előfizetéseket.

## <a name="send-messages-tooa-topic"></a>Üzenetek tooa témakör küldése
toosend üzenet tooa Service Bus-témakörbe, az alkalmazásnak használnia kell a hello `send_topic_message` hello metódusában **ServiceBusService** objektum.

hello következő példa bemutatja, hogyan toosend öt teszt túl üzenetek`mytopic`. Vegye figyelembe, hogy hello `messagenumber` egyes üzenetek tulajdonság értéke a hello iterációs hello hurok változik (Ez határozza meg, melyik előfizetések fogják megkapni):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus-üzenettémakörök támogatja a maximális üzenet mérete 256 KB-os hello [Standard csomagra](service-bus-premium-messaging.md) hello 1 MB [prémium csomagban](service-bus-premium-messaging.md). hello fejléc, beleértve a hello normál és egyéni alkalmazás tulajdonságai, a 64 KB méretű lehet. A témakörökben tárolt üzenetek hello száma nincs korlátozva, de nincs a tengelysapka a hello, a témakörök által tárolt hello üzenetek teljes mérete. A témakör ezen méretét a létrehozáskor kell meghatározni, és a felső korlátja 5 GB. Kvóták kapcsolatos további információkért lásd: [Service Bus kvóták][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Üzenetek fogadása egy előfizetés
Üzenetek fogadása egy előfizetésből hello segítségével `receive_subscription_message` hello metódusa **ServiceBusService** objektum:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Az üzenetek törlődnek hello előfizetésből, mikor olvasott paraméter hello `peek_lock` értéke túl**hamis**. (A betekintés) olvashatja és üdvözlőüzenetére zárolási beállítás hello paraméterrel hello várólista törlése nélkül `peek_lock` túl**igaz**.

hello olvasási viselkedését, és üdvözlőüzenetére törlése, hello részét fogadási művelethez hello legegyszerűbb modell, és az forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet a hello esetre, ha nem működik a legjobban. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Csak a Service Bus fel hello üzenetet, feldolgozottként, majd amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

Ha hello `peek_lock` paraméter értéke túl**igaz**, hello kap két szakaszból álló művelet, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek lesz. A Service Bus kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával `delete` hello metódusa **üzenet** objektum. Hello `delete` metódus hello üzenetet, feldolgozottként jelöli meg, és eltávolítja a hello előfizetés.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-toohandle-application-crashes-and-unreadable-messages"></a>Hogyan toohandle omlik össze és nem olvasható üzenetek
Service Bus zökkenőmentesen helyreállíthatja az alkalmazás vagy egy üzenet feldolgozása problémák hibáinak funkció toohelp biztosít. Ha egy fogadó alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, majd akkor meghívhatja hello `unlock` hello metódusa **üzenet** objektum. Ezzel a Service Bus toounlock hello üzenet hello előfizetésen belül, és könnyebben elérhető toobe újbóli fogadását, vagy az hello ugyanaz az alkalmazás vagy egy másik fogyasztó alkalmazás általi fel.

Emellett van lévő hello előfizetéssel társított időtúllépés, és ha hello alkalmazás sikertelen tooprocess üdvözlőüzenetére előtt hello zárolás időtúllépését lejárta (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldja köszönőüzenetei automatikusan és lehetővé teszi az újbóli fogadását elérhető toobe.

A hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello `delete` metódust, akkor üdvözlőüzenetére lesz újból kézbesítve toohello alkalmazás, amikor újraindul. Ezt gyakran nevezik *legalább egyszeri feldolgozásnak*, ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd alkalmazásfejlesztőnek további logikát tootheir alkalmazás toohandle ismétlődő üzenetkézbesítését. Ez gyakran érhető el hello segítségével **MessageId** hello üzenet, amely állandó marad a kézbesítési kísérletek tulajdonságát.

## <a name="delete-topics-and-subscriptions"></a>Témakörök és előfizetések törlése
Üzenettémák és előfizetések következetesek, és explicit módon kell törölni vagy hello [Azure-portálon] [ Azure portal] vagy programon keresztül. hello következő példa bemutatja, hogyan toodelete hello témakör nevű `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Egy témakör törlése a hello témakörben regisztrált előfizetésekkel is törli. Az előfizetések független módon is törölhetők. hello következő kód bemutatja, hogyan toodelete előfizetés nevű `HighMessages` a hello `mytopic` témakör:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus-üzenettémakörök hello alapjait, kövesse az alábbi hivatkozások további toolearn.

* Lásd: [üzenetsorok, témakörök és előfizetések][Queues, topics, and subscriptions].
* A hivatkozási [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
