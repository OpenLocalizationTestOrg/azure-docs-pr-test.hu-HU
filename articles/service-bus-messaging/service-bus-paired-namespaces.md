---
title: "a Service Bus aaaAzure párosítva névterek |} Microsoft Docs"
description: "Megvalósítás részletei párosított névtér és a költséghatékonyság"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: 4c44b2b95d2228e1ad8075b52634d88a1593d3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Névtér megvalósítás részletei megfeleltetni, ráadásul a költségek gyakorolt hatása
Hello [PairNamespaceAsync] [ PairNamespaceAsync] módszer használatával egy [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] példány, látható feladatokat hajtja végre az Ön nevében. Nincs vannak költség szempontok hello funkció használata esetén, mert azok feladatokat, hogy ez akkor fordul elő, amikor hello viselkedés várható hasznos toounderstand is. hello API kapcsolatba lép a hello automatikus viselkedését az Ön nevében a következő:

* Várakozó várólisták létrehozása.
* Létrehozása egy [MessageSender] [ MessageSender] objektum, amely tooqueues vagy témakörök-kiszolgálóhoz.
* Amikor egy üzenetküldési entitásra nem érhető el, elküldi ping üzenetekre toohello entitás egy kísérlet toodetect amikor újra elérhetővé, hogy az entitás válik.
* Választhatóan a "üzenet szivattyúk" áll, hogy az áthelyezés üzeneteit hello várakozó várólisták toohello elsődleges várólisták.
* Koordinálja a hello elsődleges és másodlagos bezárni vagy hibás [MessagingFactory] [ MessagingFactory] példányok.

Magas szinten, hello szolgáltatást a következőképpen működik: hello elsődleges entitás állapota kifogástalan, ha nincs viselkedés változások történnek. Ha hello [FailoverInterval] [ FailoverInterval] időtartam eltelte és hello elsődleges entitás látja nem sikeres elküldése után nem átmeneti [MessagingException] [ MessagingException] vagy egy [TimeoutException][TimeoutException], akkor fordul elő, a következő viselkedés hello:

1. Műveletek toohello elsődleges entitás le vannak tiltva, és hello rendszer ping-üzenetek hello elsődleges entitás, amíg a ping-üzenetek kézbesítése sikeresen küldése.
2. Egy véletlenszerű várakozó várólista van kiválasztva.
3. [BrokeredMessage] [ BrokeredMessage] objektum a kiválasztott várakozó várólista irányított toohello.
4. Ha nem sikerül egy küldési művelet toohello várakozó várólista választott, hello Elforgatás lekért van erre a várólistára, és egy új sor van kiválasztva. Hello az összes feladó [MessagingFactory] [ MessagingFactory] hello hiba további példányt.

a következő ábrán hello hello feladatütemezési jelzik. Első lépésként hello küldő üzeneteket küld.

![Párhuzamos névterek][0]

Hiba toosend toohello elsődleges queue, akkor a hello küldő megkezdi véletlenszerűen kiválasztott várólista várakozó üzenetek tooa küldésekor. Egy időben a ping feladat elindítása.

![Párhuzamos névterek][1]

Ezen a ponton köszönőüzenetei hello másodlagos várólista továbbra is szerepelnek, és nem szállított toohello elsődleges queue. Ha hello elsődleges queue, kifogástalan újra kell legalább egy folyamat fut hello Szifonos. hello Szifonos nyújt köszönőüzenetei összes hello különböző várakozó fájlok a várólisták toohello megfelelő cél entitásokat (üzenetsorok és témakörök).

![Párhuzamos névterek][2]

Ez a témakör további része hello hello részletes, hogyan darabot ismerteti.

## <a name="creation-of-backlog-queues"></a>Várakozó várólista létrehozása
Hello [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] toohello átadott objektum [PairNamespaceAsync] [ PairNamespaceAsync] metódus hello jelzi várakozó fájlok számát a várólisták azt szeretné, hogy toouse. Minden tartalék várólista létrejön a hello következő tulajdonságai explicit módon beállítva (minden más helyen vannak beállítva toohello [QueueDescription] [ QueueDescription] alapértelmezett):

| Elérési út | [elsődleges névtér] / x-servicebus-átvitel / [index] [0, BacklogQueueCount) érték esetén a [index] |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| maxDeliveryCount |egész szám. MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| LockDuration |1 perc |
| EnableDeadLetteringOnMessageExpiration |Igaz |
| EnableBatchedOperations |Igaz |

Például hello első várakozó várólista létrehozott névtér **contoso** nevű `contoso/x-servicebus-transfer/0`.

Hello várólisták létrehozásakor a hello kód először ellenőrzi az toosee, ha ilyen a várólista létezik. Ha hello várólista nem létezik, hello várólista létrejön. hello kód nem "extra" Várakozó várólisták karbantartása. Pontosabban, ha hello alkalmazás hello elsődleges névtér **contoso** öt várakozó várólistára, de hello elérési úttal rendelkező várólista várakozó kérelmek `contoso/x-servicebus-transfer/7` létezik, várólistához további várakozó továbbra is megtalálható, de nem használatos. hello rendszer explicit módon engedélyezi a felesleges várakozó várólisták tooexist, amelyek nem használhatók. Hello névtér tulajdonosként Ön felelősséggel tartozik törölje a nem használt vagy nemkívánatos várakozó üzenetsorokkal. hello ehhez a döntéshez oka, hogy a Service Bus nem tudja, milyen célból minden hello várólista a névtérben található. Továbbá ha a várólista hello a megadott névvel, azonban nem felel meg a feltételezett hello [QueueDescription][QueueDescription], majd az OK: a saját hello változó alapértelmezés. Nincs garancia hozhatók módosítások toohello várakozó várólisták a kódot. Győződjön meg arról, hogy tootest a módosítások alaposan.

## <a name="custom-messagesender"></a>Egyéni MessageSender
Küldésekor, az összes üzenet végighaladnia belső [MessageSender] [ MessageSender] objektum, amely a szokásos módon viselkedik, ha minden működik, és átirányítja a toohello várakozó várólisták, amikor dolgot "törés." Nem átmeneti hiba fogadását követően a felszabadításakor kezdődik. Miután egy [TimeSpan] [ TimeSpan] hello álló időszak [FailoverInterval] [ FailoverInterval] során, ami nem sikeres üzenetküldés tulajdonság értéke , hello feladatátvételi be kell kapcsolni. Ezen a ponton hello következő dolgokat okozhat minden egyes entitásnál:

* A ping feladatot hajt végre minden [PingPrimaryInterval] [ PingPrimaryInterval] toocheck Ha hello entitás érhető el. Ez a feladat sikeres, ha azonnal hello entitás használó összes ügyfél kódot elindul, elsődleges névtér új üzenetek toohello küldése.
* Ugyanaz az entitás más feladóktól hello eredményez a későbbi kérelmek toosend toothat [BrokeredMessage] [ BrokeredMessage] toobe küldött módosított toosit hello várakozó várólistában. hello módosítása egyes tulajdonságok eltávolítása hello [BrokeredMessage] [ BrokeredMessage] objektumra, és a máshol tárolja azokat. hello következő tulajdonság nincs bejelölve, és hozzáadása az új aliast, amely lehetővé teszi a Service Bus és hello SDK tooprocess üzenetek egységesen:

| Régi tulajdonság neve | Új tulajdonság neve |
| --- | --- |
| Munkamenet-azonosító |x-ms-munkamenet-azonosító |
| TimeToLive |az x-ms-élettartam |
| ScheduledEnqueueTimeUtc |x-ms-elérési út |

hello eredeti elérési utat célként is tárolja belül üdvözlőüzenetére x-ms-path nevű tulajdonság. Ez a kialakítás lehetővé teszi, hogy sok entitások toocoexist üzeneteket egyetlen várakozó várólista. hello tulajdonságok vissza által hello Szifonos van lefordítva.

egyéni hello [MessageSender] [ MessageSender] objektum is tapasztal, amikor üzenetek hello 256 KB-os korlát közelítse meg, és feladatátvételi be kell kapcsolni. egyéni hello [MessageSender] [ MessageSender] objektum tárolja az üzeneteket az összes üzenetsorok és témakörök hello várakozó várólistára együtt. Ez az objektum keveri sok elsődleges együtt várólistákon hello várakozó üzenetek. sok ügyfél, amely nem tudja, hogy minden más, hello SDK véletlenszerűen közötti terheléselosztás toohandle szerzi várakozó fájlok – egy sor minden [QueueClient] [ QueueClient] vagy [TopicClient] [ TopicClient] hoz létre a kódban.

## <a name="pings"></a>Ping-üzenetek
A ping üzenetre egy üres [BrokeredMessage] [ BrokeredMessage] rendelkező a [ContentType] [ ContentType] tulajdonsága tooapplication/vnd.ms-szolgáltatásbusz-ping és egy [TimeToLive] [ TimeToLive] értéke 1 másodperc. A ping rendelkezik egy különös jellemző a Service Bus: hello server soha nem továbbítja a pingelés, ha a hívó kér egy [BrokeredMessage][BrokeredMessage]. Ebből kifolyólag sosem toolearn hogyan tooreceive és figyelmen kívül. Minden entitás (egyedi üzenetsor vagy témakör) / [MessagingFactory] [ MessagingFactory] ügyfelenként példányt fog kell program ping üzenetet küld azok által szabott toobe nem érhető el. Alapértelmezés szerint ez akkor fordul elő percenként. Ping üzenetekre toobe rendszeres Service Bus üzenetek minősülnek, és sávszélesség és üzenetek eredményezhet. Amint hello-ügyfelek észlelik, hogy rendelkezésre áll-e hello rendszer, köszönőüzenetei leállítása.

## <a name="hello-syphon"></a>hello Szifonos
Legalább egy végrehajtható program hello alkalmazásban aktívan futnia kell hello Szifonos. hello Szifonos hajt végre egy hosszú lekérdezési kap, hogy 15 percig tart. Amikor entitásokhoz érhető el, és 10 várakozó várólisták, hello hello Szifonos hívások hello fogadási művelethez 40 alkalommal óránkénti, napi 960 időpontot és 30 napban 28800 alkalommal futtató alkalmazáshoz. Hello Szifonos aktívan üzenetek áthelyezése hello várakozó toohello elsődleges queue-ből, ha minden üzenetet észlel hello költségek (az üzenet mérete és a sávszélesség szabványos díja valamennyi szakaszában vonatkozik) a következő:

1. Toohello várakozó küldési.
2. Hello várakozó kapott.
3. Toohello elsődleges küldése.
4. Elsődleges hello kapott.

## <a name="closefault-behavior"></a>Zárja be/tartalék viselkedése
Hello Szifonos, egyszer hello elsődleges vagy másodlagos üzemeltető alkalmazáson belüli [MessagingFactory] [ MessagingFactory] hibákat, vagy le van zárva a partner is hibát jelzett/lezárult és hello nélkül Szifonos észleli, ebben az állapotban , hello Szifonos működik. Ha más hello [MessagingFactory] [ MessagingFactory] nem zárt 5 másodpercen belül hello Szifonos hello továbbra is nyitva fog fault [MessagingFactory] [ MessagingFactory] .

## <a name="next-steps"></a>Következő lépések
Lásd: [aszinkron üzenetkezelési mintákat és magas rendelkezésre állású] [ Asynchronous messaging patterns and high availability] részletes információt a Service Bus aszinkron üzenetkezelési. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions#Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_FailoverInterval
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
