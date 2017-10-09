---
title: "aaaCreate particionálva Azure Service Bus-üzenetsorok és témakörök |} Microsoft Docs"
description: "Ismerteti, hogyan toopartition Service Bus-üzenetsorok és témakörök több üzenet a brókerek."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;hillaryc
ms.openlocfilehash: 6d42556a0714d6a012dc319f662521c8b0bb958b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partitioned-queues-and-topics"></a>Particionált üzenetsorok és témakörök
Az Azure Service Bus több üzenet brókerek tooprocess üzenetek alkalmazza, és toostore üzeneteket több üzenetküldési tároló. Hagyományos üzenetsor vagy témakör egyetlen üzenet ügynök által kezelt és egy üzenetküldési tárolóban tárolja. A Service Bus *partíciók* üzenetsorok és témakörök, engedélyezése vagy *üzenetküldési entitások*, toobe több üzenetet brókerek és üzenetküldési tárolók particionálva. Ez azt jelenti, hogy a teljes teljesítményt particionált entitás hello hello teljesítményét egy egyetlen üzenet broker vagy az üzenetküldési tárolóban már nem korlátozzák. Ezenkívül átmenetileg nem működik az üzenetküldési tárolóban nem képezhető le egy particionált üzenetsor vagy témakör nem érhető el. A particionált üzenetsorok és témakörök tartalmazhat összes speciális Service Bus-funkciók, például a tranzakciók és a munkamenetek támogatása.

A Service Bus internals kapcsolatos információkért lásd: hello [Service Bus-architektúra] [ Service Bus architecture] cikk.

Particionálás engedélyezve van alapértelmezés szerint a entitás létrehozása minden üzenetsorok és témakörök a Standard és prémium szintű üzenetkezelés. Standard üzenetküldési réteg entitások particionálás nélkül is létrehozhat, de az üzenetsorok és témakörök a prémium szintű névtérben mindig particionálva; Ez a beállítás nem lehet letiltani. 

Már nem lehetséges toochange hello particionálás meg egy létező üzenetsorba vagy témakör Standard vagy prémium rétegek, a beállítás, csak megadhat hello beállítást hello entitás létrehozásakor.

## <a name="how-it-works"></a>Működés

Minden egyes particionált üzenetsor vagy témakör több töredék áll. Minden egyes töredék egy másik üzenetküldési tárolóban tárolja, és egy másik üzenet broker kezeli. Egy üzenet elküldésekor tooa particionált üzenetsor vagy témakör, a Service Bus hello üzenet tooone hello töredékek rendeli hozzá. hello kiválasztása véletlenszerűen történik a Service Bus, vagy a partíciós kulcs, amely a küldő hello segítségével adhatja meg.

Amikor egy ügyfél szeretné tooreceive particionált sorból egy üzenetet, vagy az előfizetés tooa particionált témakör, a Service Bus lekérdezi az összes részlete üzenetek, majd adja vissza első üdvözlőüzenetére bármelyik üzenetküldési tárolók toohello fogadó hello előállított. A Service Bus-gyorsítótárak hello más üzenetek és fogadhatók kérések az azokat a további fogadásakor értéket ad vissza. A fogadó ügyfél nincs tisztában hello particionálás; ügyfél irányultságú viselkedését particionált üzenetsor vagy témakör hello (például Olvasás, végezze el, késlelteti a kézbesítetlen levelek, prefetching) azonos toohello viselkedés rendszeres entitás.

Nincs üzenet küldésekor, vagy egy üzenet fogadását a particionált üzenetsor vagy témakör további költség nélkül.

## <a name="enable-partitioning"></a>Particionálás engedélyezése

particionált toouse üzenetsorok és témakörök az Azure Service Bus hello Azure SDK 2.2 vagy újabb verzióját használja, vagy adjon meg `api-version=2013-10` a HTTP-kérelmek.

### <a name="standard"></a>Standard

Standard szintű üzenetkezelési réteg hello Service Bus-üzenetsorok és témakörök (hello alapértelmezett érték 1 GB-os), 1, 2, 3, 4 vagy 5 GB méretű hozhat létre. A particionálás engedélyezve van, a Service Bus hello entitás (16 partíciók) 16 másolatokat készít a megadott GB alapú egységárát. Így, ha létrehoz egy sort, amely 5 GB-nál, 16 partíciókkal rendelkező hello várólista maximális hossza válik (5 \* 16) = 80 GB. Megjelenik a particionált üzenetsor vagy témakör maximális méretén hello megtekintésével, hogy a belépési hello [Azure-portálon][Azure portal], a hello **áttekintése** panel az adott entitáshoz.

### <a name="premium"></a>Prémium

Prémium szint névtérben Service Bus-üzenetsorok és témakörök (hello alapértelmezett érték 1 GB-os), 1, 2, 3, 4, 5, 10, 20, 40 vagy 80 GB méretű is létrehozhat. Particionálás, alapértelmezés szerint engedélyezve van, a Service Bus két partíció egy entitás hoz létre. Megjelenik a particionált üzenetsor vagy témakör maximális méretén hello megtekintésével, hogy a belépési hello [Azure-portálon][Azure portal], a hello **áttekintése** panel az adott entitáshoz.

Hello prémium szintű üzenetkezelési réteg partícionálásra vonatkozó további információkért lásd: [Service Bus prémium és standard szintű üzenetkezelési szintek](service-bus-premium-messaging.md). 

### <a name="create-a-partitioned-entity"></a>A particionált entitás létrehozása

Nincsenek számos módon toocreate egy particionált várólista vagy témakör. Amikor az alkalmazás hello üzenetsor vagy témakör hoz létre, engedélyezheti a particionálás által rendre beállítás hello hello üzenetsor vagy témakör [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] vagy [TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] tulajdonság túl**igaz**. Ezek a Tulajdonságok hello idő hello várólista be kell állítani, vagy témakör jön létre. Ahogy korábban is hangsúlyoztuk, már nem lehetséges toochange ezeket a tulajdonságokat a egy meglévő üzenetsor vagy témakör. Példa:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternatív megoldásként létrehozhat egy particionált üzenetsor vagy témakör hello [Azure-portálon] [ Azure portal] vagy a Visual Studióban. Amikor létrehoz egy üzenetsor vagy témakör hello portálon, hello **particionálás engedélyezése** hello üzenetsor vagy témakör beállítás **létrehozása** panel alapértelmezés szerint be van jelölve. Csak letilthatja ezt a beállítást, a Standard csomag entitásban; a hello Premium szint particionálás mindig engedélyezve van. A Visual Studióban, kattintson a hello **particionálás engedélyezése** hello jelölőnégyzet **új várólista** vagy **új témakör** párbeszédpanel megnyitásához.

## <a name="use-of-partition-keys"></a>Partíciós kulcsok használata
Ha üzenetet a várólistában levő particionált üzenetsor vagy témakör azokat, Service Bus partíciókulcsot hello jelenléte keresi. Ha megtalálja, hello töredék kulcs alapján választja ki. Ha nem talál egy partíciókulcsot, egy belső algoritmus alapján hello töredék kiválasztva.

### <a name="using-a-partition-key"></a>A partíciókulcsok használatával
Egyes esetekben, például a munkamenetet vagy tranzakciók, egy adott töredék tárolt üzenetek toobe igényelnek. Ezek a forgatókönyvek a partíciós kulcs hello használata szükséges. Összes üzenetet, hogy ugyanazzal a partíciókulccsal rendelt használata hello toohello azonos darabolható. Hello töredék átmenetileg nem érhető el, ha a Service Bus hibát ad vissza.

Hello forgatókönyvtől függően különböző üzenettulajdonságok egy partíció kulcsaként vannak használatban:

**Munkamenet-azonosító**: Ha egy üzenet hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] tulajdonság, akkor a Service Bus ezt a tulajdonságot használja, mint hello partíciós kulcs. Ezzel a módszerrel, toohello tartozó összes üzenet ugyanabban a munkamenetben kezeli hello azonos üzenet broker. Ez lehetővé teszi, hogy a Service Bus tooguarantee üzenet munkamenet-állapot hello konzisztenciájának valamint rendezés.

**PartitionKey**: Ha egy üzenet hello [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] tulajdonságot, de nem hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] tulajdonság, akkor a Service Bus használ a hello [PartitionKey] [ PartitionKey] hello partíciókulcs tulajdonság. Ha üdvözlőüzenetére mindkét hello [SessionId] [ SessionId] és hello [PartitionKey] [ PartitionKey] tulajdonságok beállítása tulajdonságot is meg kell egyeznie. Ha hello [PartitionKey] [ PartitionKey] tulajdonsága eltérő értéket tooa hello [SessionId] [ SessionId] tulajdonság, a Service Bus eredménye egy érvénytelen a művelet kivétel. Hello [PartitionKey] [ PartitionKey] tulajdonság használandó, ha a munkamenet-kompatibilis tranzakciós üzeneteket küldő. hello partíciós kulcs biztosítja, hogy egy tranzakción belül küldött összes üzenet kezeli hello azonos üzenetkezelési broker.

**MessageId**: Ha hello üzenetsor vagy témakör hello [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] tulajdonsága túl**igaz** és hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] vagy [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] tulajdonságai nincsenek beállítva, majd hello [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] tulajdonság hello partíció kulcsaként szolgál. (Vegye figyelembe, hogy hello Microsoft .NET- és AMQP szalagtárak automatikus hozzárendelése egy üzenet azonosítója, ha hello küldő alkalmazás nem.) Ebben az esetben a hello ugyanaz az üzenet által kezelt összes példánya azonos üzenet broker hello. Ez lehetővé teszi, hogy a Service Bus toodetect, és kiküszöbölhetők a duplikált üzenetek. Ha hello [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] tulajdonság értéke nem túl**igaz**, a Service Bus nem tekinti hello [MessageId] [ MessageId] partíciókulcsként tulajdonság.

### <a name="not-using-a-partition-key"></a>Nem a partíciókulcsok használatával
Hello hiányában a partíciós kulcs a Service Bus egy ciklikus multiplexeléssel tooall hello darabjai hello particionált üzenetsor vagy témakör üzenetek osztja el. A kiválasztott hello töredéket nem érhető el, ha a Service Bus hello üzenet tooa különböző töredék rendeli hozzá. Ezzel a módszerrel hello küldési művelet sikeres, annak ellenére, hogy hello ideiglenes hiányában egy üzenetküldési tárolóban. Azonban Ön nem érhető el rendezést, hogy a partíciós kulcs biztosít garantált hello.

Rendelkezésre állás (nincs partíciós kulcs) és a konzisztencia (a partíciókulcsok használatával) közötti hello kompromisszumot részletesebb tárgyalását lásd: [Ez a cikk](../event-hubs/event-hubs-availability-and-consistency.md). Ezek az információk a toopartitioned Service Bus-entitások és az Event Hubs partíciók egyaránt vonatkoznak.

toogive szolgáltatás busz elég ideje tooenqueue üdvözlőüzenetére be egy másik töredék hello [MessagingFactorySettings.OperationTimeout] [ MessagingFactorySettings.OperationTimeout] hello ügyfél üdvözlőüzenetére küldő által megadott értéket 15 másodperc nagyobbnak kell lennie. Javasoljuk, hogy állítsa a hello [OperationTimeout] [ OperationTimeout] tulajdonság toohello alapértelmezett értéke 60 másodperc.

Vegye figyelembe, hogy a partíciós kulcs "PIN" üzenet tooa adott töredéket. Hello üzenetküldési tárolóban, amely tárolja az e töredékben nem érhető el, ha a Service Bus hibát ad vissza. A partíciós kulcs hello hiányában Service Bus használhatja a különböző töredékkel, és hello művelet sikeres. Ezért ajánlott, hogy nem ad meg partíciókulcsot csak szükség esetén.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Speciális témakörök: tranzakciók használata a particionált entitások
Egy tranzakció részeként küldött üzenetek meg kell adnia egy partíciókulcsot. Ez lehet az egyik hello következő tulajdonságai: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], vagy [ BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Adjon meg ugyanabban a tranzakcióban hello részeként küldött összes üzenet hello ugyanazzal a partíciókulccsal. Nem tartozik partíciós kulcs tranzakción belül üzenet toosend kísérli meg, ha a Service Bus érvénytelen művelet kivételt adja vissza. Ha toosend belül több üzenetet hello ugyanabban a tranzakcióban, amely különböző a partíciós kulcsok, a Service Bus érvénytelen művelet kivételt adja vissza. Példa:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Ha partíciókulcsként átadott hello tulajdonságok valamelyikét, a Service Bus PIN-kódok hello üzenet tooa adott töredéket. Ez akkor fordul elő, attól függetlenül, hogy egy tranzakció használható. Javasoljuk, hogy nincs megadva a partíciós kulcs esetén nem szükséges.

## <a name="using-sessions-with-partitioned-entities"></a>Munkamenetek használata a particionált entitások
a tranzakciós üzenet tooa munkamenet-kompatibilis témakör toosend vagy várólista, üdvözlőüzenetére rendelkeznie kell hello [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] tooltipservice.ToolTip tulajdonság. Ha hello [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] tulajdonság is meg van adva, az azonos toohello lehet [SessionId] [ SessionId] tulajdonság. Ha ezek eltérnek, a Service Bus érvénytelen művelet kivételt adja vissza.

Normál (nem particionált) üzenetsorok és témakörök eltérően nincs lehetséges toouse egy egyetlen tranzakció toosend több üzenetek toodifferent munkamenetet. Kísérlet történt, ha a Service Bus érvénytelen művelet kivételt adja vissza. Példa:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>A particionált entitások automatikus üzenetek továbbítása
A Service Bus továbbítási az, hogy vagy particionált entitások közötti automatikus üzenet támogatja. tooenable: az üzenet automatikus továbbítása, állítsa be a hello [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] hello forrásvárólista vagy előfizetési tulajdonság. Ha üdvözlőüzenetére ad meg partíciókulcsot ([SessionId][SessionId], [PartitionKey][PartitionKey], vagy [MessageId] [ MessageId]), a partíciós kulcs hello cél entitás szolgál.

## <a name="considerations-and-guidelines"></a>Szempontok és irányelveket
* **Magas konzisztencia szolgáltatások**: Ha egy entitás például munkamenetek, kettős észlelés vagy partíciós kulcs explicit irányítását funkciókat használ, akkor hello üzenetkezelési műveletek még mindig irányított toospecific töredék. Ha bármelyik hello töredék nagy forgalmú tapasztalnak, vagy hello a mögöttes tároló állapota nem megfelelő, ezek a műveletek sikertelenek, és a rendelkezésre állási csökken. A teljes hello konzisztencia, továbbra is sokkal nagyobb mint nem particionált entitások; a forgalom csak egy részhalmazát, hibákat észlelt megakadályozását tooall hello adatforgalom. További információkért tekintse meg a [rendelkezésre állási és konzisztencia](../event-hubs/event-hubs-availability-and-consistency.md).
* **Felügyeleti**: műveletek, például a létrehozási, frissítési és törlési hello entitás összes hello töredék kell végrehajtani. Ha bármely töredék állapota nem megfelelő az ezekhez a műveletekhez hibákat okozhat. Hello Get művelethez információ például üzenetben számlálás kell összesíteni az összes részlete. Bármely töredék állapota nem kifogástalan, ha korlátozott eltulajdonítja hello entitás rendelkezésre állási állapotát.
* **Üzenet forgatókönyvek kevés**: ilyen helyzetekben, különösen akkor, ha hello HTTP protokollt használó, előfordulhat, hogy tooperform több műveletek rendelés tooobtain az összes hello üzeneteket fogadni. A fogadási kérelmek hello receive végez összes hello töredék és gyorsítótárba helyezi azt minden hello válasz érkezett. Hello ugyanazt a kapcsolatot ehhez kihasználhassa a gyorsítótárazást és fogadására késések fordulnak elő a következő receive kérelmek alacsonyabb lesz. Azonban ha több kapcsolatot vagy a HTTP Protokollt használja, amely kapcsolatot hoz létre új egyes kérésekre vonatkozóan. Nem biztos, hogy, akkor mindkét hello nincs ilyen ugyanahhoz a csomóponthoz. Ha az összes meglévő üzenetek zárolva van, és tárolja a rendszer egy másik előtér hello fogadási művelet értéket ad vissza **null**. Üzenetek végül hagyja elévülni, majd újra fogadhatja. HTTP életben tartási ajánlott.
* **A tallózási/betekintés üzenetek**: [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) nem mindig hello számának visszaadása hello megadott üzenetek [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MessageCount) tulajdonság. Nincsenek két gyakori okai az. Egyik oka, hogy hello hello gyűjteménye üzenetek összesített mérete meghaladja a 256KB méretű hello. Egy másik oka az, hogy ha hello üzenetsor vagy témakör hello [EnablePartitioning tulajdonság](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning) túl beállítása**igaz**, a partíció nincs elég üzenetek toocomplete hello kért üzenetek száma. Általában, ha egy alkalmazás tooreceive üzenetek adott számú azt szeretné, akkor meg kell hívnia az [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) ismételten, amíg azt lekérdezi, hogy az üzenetek száma, vagy nincs nem további üzenetek toopeek. További információ mintakódjainak megtekintése, beleértve: [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) vagy [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_PeekBatch_System_Int32_).

## <a name="latest-added-features"></a>Legújabb szolgáltatással
* Hozzáadni vagy eltávolítani a szabály mostantól támogatják a particionált entitások. Nem particionált entitások eltérő, ezek a műveletek nem támogatottak a tranzakciók. 
* AMQP küldését és fogadását az üzenetek tooand particionált entitásból mostantól támogatott.
* AMQP mostantól támogatott a következő műveletek hello: [kötegelt küldése](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_BrokeredMessage__), [kötegelt kap](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ReceiveBatch_System_Int32_), [fogadási sorszáma szerint](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Receive_System_Int64_), [Belepillantás](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Peek), [ Újítsa meg a zárolást](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_RenewMessageLock_System_Guid_), [üzenet ütemezése](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ScheduleMessageAsync_Microsoft_ServiceBus_Messaging_BrokeredMessage_System_DateTimeOffset_), [ütemezett üzenet törlése](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_CancelScheduledMessageAsync_System_Int64_), [szabály hozzáadása](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [szabály eltávolítása](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Munkamenet Renew zárolási](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_RenewLock), [munkamenet-állapot beállítása](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_), [Get munkamenet-állapot](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_GetState), és [munkamenetek enumerálása](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessionsAsync).

## <a name="partitioned-entities-limitations"></a>Particionált entitások korlátozásai
Jelenleg a Service Bus ró a következő korlátozások vonatkoznak a particionált üzenetsorok és témakörök hello:

* Particionált üzenetsorok és témakörök nem támogatják az üzenetek küldését toodifferent tartozó munkamenetek egy tranzakción belül.
* A Service Bus jelenleg lehetővé teszi a particionált too100 várólisták vagy témakörök / névtér. Minden egyes particionált üzenetsor vagy témakör álló hello kvótát (nem érvényes tooPremium réteg) névtér száma 10 000 entitások felé.

## <a name="next-steps"></a>Következő lépések
Lásd: hello értékelése [AMQP 1.0 támogatása a Service Bus üzenetsorok és témakörök particionálva] [ AMQP 1.0 support for Service Bus partitioned queues and topics] üzenetküldési entitások partícionálásra vonatkozó további toolearn. 

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription#Microsoft_ServiceBus_Messaging_TopicDescription_EnablePartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
