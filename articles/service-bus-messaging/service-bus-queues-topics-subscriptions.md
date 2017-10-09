---
title: "az Azure Service Bus üzenetkezelés üzenetsorok, témakörök és előfizetések aaaOverview |} Microsoft Docs"
description: "Service Bus üzenetküldési entitásokról áttekintése."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a306ced4-74e9-47c6-990a-d9c47efa31d5
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm
ms.openlocfilehash: 73135d2658e341c14dbb114ab938faed91578ff1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus queues, topics, and subscriptions (Service Bus-üzenetsorok, -témakörök és -előfizetések)

A Microsoft Azure Service Bus felhőalapú, üzenet célú köztes technológiák – például a megbízható üzenetsor és a tartós közzétételi/előfizetési üzenetküldési támogat. A "közvetítőalapú" üzenettovábbítási képességeket leválasztott üzenetküldési szolgáltatások támogatási közzétételi-feliratkozási, historikus leválasztása és a terheléselosztási forgatókönyveket a Service Bus üzenetküldési háló hello használatával tekinthetők. A leválasztott kommunikációnak számos előnye van: az ügyfelek és a kiszolgálók például szükség szerint kapcsolódhatnak, és aszinkron módon hajthatják végre a műveleteiket.

hello üzenetküldési entitások, a képességek a Service Bus üzenetkezelés hello hello core alkotó üzenetsorok, témakörök és előfizetések és szabályok/művelet.

## <a name="queues"></a>Üzenetsorok

Az üzenetsorok elsőnek *First In, első kimenő* (FIFO) üzenet kézbesítési tooone vagy több versengő fogyasztó számára. Ez azt jelenti, hogy az üzenetek általában várt toobe fogad és dolgoz fel, amelyben toohello várólista addig adták hozzá, és minden üzenetet kapott, és csak egy üzenetfogyasztó által feldolgozott hello sorrendben hello fogadók. Üzenetsorok használata egyik legfontosabb előnye tooachieve "időbeli elválasztás" alkalmazás-összetevő. Más szóval hello adatalkotóknak (küldőknek) és a fogyasztóknak (fogadóknak) nem rendelkezik toobe küldése és az üzenetek fogadását hello, azonos időben, mert üzenetek hello várólista tartósan tárolja. Hello készítő nem rendelkezik válaszára a hello fogyasztói toowait rendelés toocontinue tooprocess továbbá üzeneteket küldeni.

Egy kapcsolódó előny, "terhelés simítás", amely lehetővé teszi, hogy a létrehozói és felhasználói toosend, és különböző ütemben üzeneteket fogadni. Számos alkalmazás hello rendszerterhelés időnként; eltérő azonban a szükséges egyes Munkaegységek hello feldolgozási ideje pedig jellemzően állandó marad. Közé üzenetek létrehozói és felhasználói üzenetsorokat azt jelenti, hogy csak az alkalmazás fel hello toobe kiosztott toobe képes toohandle átlagos terheléssel csúcsterhelés helyett. hello várólista hello mélysége növekszik és csökken, mivel változik hello bejövő terhelés. Ez közvetlen megtakarításokkal pénz infrastruktúra szükséges tooservice hello alkalmazásterhelés toohello mennyiségű figyelembe véve. Hello szerint a terhelés növekedésével, további feldolgozó folyamatok hozzáadott tooread hello várólistában lehet. Minden üzenetet csak az egyik hello munkavégző folyamatok dolgoz fel. Ezenkívül a lekérésalapú terheléselosztás lehetővé teszi, hogy hello munkavégző számítógépek optimális használatára akkor is, ha hello munkavégző számítógépek térnek el egymástól, amely legutóbb tooprocessing rendelkezik, módon kérik le a saját maximális díj üzenetek. Ezt a mintát gyakran nevezik hello "versengő fogyasztó" mintának.

Egy rejlő laza kapcsoló hello összetevői közötti várólisták toointermediate üzenetek létrehozói és felhasználói között segítségével biztosítja. Mivel létrehozói és felhasználói nem kompatibilis, minden más, a fogyasztó frissítése anélkül, hogy hello készítő hatással.

A várólista létrehozása folyamat. Service Bus üzenetküldési entitásokról (üzenetsorok és témakörök) keresztül hello felügyeleti műveleteinek elvégzéséhez [Microsoft.ServiceBus.NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) osztályt, amely úgy, hogy megadja a Service Bus hello hello alapszintű címéből épül névtér és hello felhasználói hitelesítő adatokat. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) módszerek toocreate biztosít enumerálása és üzenetküldési entitások törlése. Miután létrehozta a [Microsoft.ServiceBus.TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) hello SAS-nevet és a kulcs és a szolgáltatási névtér felügyeleti objektum objektum, használhatja a hello [Microsoft.ServiceBus.NamespaceManager.CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) metódus toocreate hello várólista. Példa:

```csharp
// Create management credentials
TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Ezután létrehozhat várólista-objektum és egy üzenetkezelési gyárból a Service Bus URI hello argumentumként. Példa:

```csharp
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Ezután küldhetnek üzeneteket toohello várólista. Például, ha a lista a közvetítőalapú üzenetek nevű `MessageList`, hello kód hasonló toohello következő látható:

```csharp
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Majd kapott üzenetek hello várólista az alábbiak szerint:

```csharp
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

A hello [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) módban hello fogadási művelet egylépéses; Ez azt jelenti, hogy a Service Bus hello kérelmet kap, ha azt hello üzenetet, feldolgozottként jelöli meg, és visszaadja toohello alkalmazás. **ReceiveAndDelete** mód hello legegyszerűbb modell, és működik a legjobban, mely hello az alkalmazás működését nem dolgoz fel üzenetet hello esetre, ha nem a forgatókönyvek esetén. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. A Service Bus hello üzenetet használnak, amikor hello alkalmazás újraindul, és megkezdi a üzenetek újra fel jelöli meg, mert ki fogja hagyni, de a üdvözlőüzenetére előzetes toohello összeomlási használni.

A [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) módban hello fogadási művelet válik kétszakaszos, ami lehetővé teszi lehetséges toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek. A Service Bus hello kérelmet kap, ha azt hello felhasznált tovább üzenet toobe talál, tooprevent zárolja, fogadjon más felhasználói és toohello alkalmazás visszaadja. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) hello érkezett üzenet. Amikor a Service Bus látja a hello **Complete** hívás, hello üzenetet, feldolgozottként jelöli.

Ha hello alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, akkor meghívhatja hello [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) metódust a fogadott üdvözlőüzenetére (ahelyett, hogy [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Ez lehetővé teszi, hogy a Service Bus toounlock üdvözlőüzenetére, és tegye elérhetővé toobe újbóli fogadását, vagy hello ugyanazon felhasználó vagy egy másik versengő fogyasztó. Másodszor nincs időtúllépés hello zárolás van társítva, és ha hello alkalmazás sikertelen tooprocess hello üzenet hello zárolás időtúllépését lejárta előtt (például ha hello alkalmazás összeomlik), akkor a Service Bus feloldja üdvözlőüzenetére, és lehetővé teszi az elérhető toobe ismételt fogadását (lényegében hajt végre egy [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) művelet alapértelmezés szerint).

Vegye figyelembe, hogy a hello eseményt, amely hello az alkalmazás összeomlik hello üzenet feldolgozása után, még mielőtt hello **Complete** kérés kiadása, hello üzenet újbóli kézbesítése toohello alkalmazás amikor újraindul. Ezt gyakran nevezik *legalább egyszeri* feldolgozása; Ez azt jelenti, hogy minden üzenet legalább egyszer fel. Azonban az egyes esetekben hello ugyanazon üzenet előfordulhat, hogy újból kézbesítve. Ha hello forgatókönyvben nem lehetségesek, akkor további logikát hello alkalmazás toodetect ismétlődések hello alapján elérhető, amely kötelező **MessageId** tulajdonság hello üzenet, amely marad állandó a kézbesítési kísérletek között. Ez más néven *pontosan egyszer* feldolgozása.

## <a name="topics-and-subscriptions"></a>Témakörök és előfizetések
Ezzel szemben tooqueues, amelyben minden üzenetet dolgoz fel egy-egy fogyasztó, a *témakörök* és *előfizetések* egy egy-a-többhöz típusú kommunikációt biztosítanak a egy *közzétételi/előfizetési* mintát. Hasznos címzettek toovery nagyszámú méretezéshez, minden egyes közzétett üzenetek legyen elérhető tooeach előfizetéssel hello témakör. Üzenetek küldése tooa témakör és kézbesített tooone vagy több kapcsolódó előfizetések, attól függően, hogy a szűrési szabályokat, amelyek egy előfizetésenként történő beállítására is beállítható. hello előfizetések további szűrőket, hogy meg kívánják-tooreceive toorestrict köszönőüzenetei használhatja. Üzenetek küldése történik tooa a témakör a hello azonos módon elküldi őket tooa várólista, de az üzenetek nem érkező hello témakör közvetlenül. Ehelyett azok érkező előfizetések. Egy üzenettémakör-előfizetésben hasonlít egy virtuális üzenetsorra, amely hello toohello témakör küldött üzenetek példányait megkapja. Üzenetek fogadása egy előfizetésből azonos az üzenetsorból érkező toohello módon.

Összehasonlításképpen hello funkció üzenet küldése egy várólista leképezi közvetlenül tooa témakör és funkciókat, üzenet-fogadás tooa előfizetés képezi le. Többek között ez azt jelenti, hogy előfizetések támogatja-e az azonos minták korábban leírt ebben a szakaszban a legutóbb tooqueues hello: versengő felhasználó, az időalapú leválasztási, terhelés simítás és terheléselosztás.

Hasonló toocreating várólista, a témakör létrehozása történik, az előző szakaszban hello hello példában látható módon. Hello szolgáltatás URI létrehozásához, és a hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály toocreate hello névtér ügyfél. Ezután létrehozhat egy témakör hello segítségével [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) metódust. Példa:

```csharp
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Ezután adja hozzá az előfizetés tetszés szerint:

```csharp
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Ezután létrehozhat egy témakör ügyfél. Példa:

```csharp
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Hello üzenet küldőjének használ, küldhet és fogadhat üzeneteket tooand hello témakör, ahogy az előző szakaszban hello. Példa:

```csharp
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Hasonló tooqueues, üzenetet kapott egy előfizetés használatával egy [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) ahelyett, hogy az objektum egy [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objektum. Hozzon létre hello előfizetés ügyfél, átadásakor hello témakör, hello előfizetés neve hello hello nevét, és (opcionálisan) hello fogadás paraméterekként módban. Például a hello **készlet** előfizetést:

```csharp
// Create hello subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Szabályok és műveletek
A sok esetben meghatározott jellemzőkkel rendelkező üzenetek különböző módon kell feldolgozni. tooenable, előfizetések toofind üzenetek tulajdonságok van szükség, és hajtsa végre az egyes módosítások toothose tulajdonságokat is beállíthat. Miközben Service Bus-előfizetések toohello témakör küldött összes üzenet megtekintéséhez csak másolhatja azokat üzenetek toohello virtuális előfizetés üzenetsorába egy részét. Mindez előfizetés-szűrők használata. Ezek a módosítások nevezzük *szűrési műveletek*. Előfizetés létrehozásakor megadhat egy kifejezést, amely üdvözlőüzenetére hello tulajdonságait, mindkét rendszer tulajdonságai hello (például **címke**) és az egyéni tulajdonságok (például **Storename –**.) hello SQL szűrőkifejezés nem kötelező ebben az esetben; egy SQL-kifejezést, nélkül bármely előfizetés definiált szűrőművelet az adott előfizetéshez tartozó összes köszönőüzenetei érvényesül.

Hello előző példában toofilter érkező üzeneteket csak használatával **Store1**, akkor kell létrehoznia hello irányítópult előfizetés az alábbiak szerint:

```csharp
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Az előfizetés szűrővel helyen, csak a hello üzenetek `StoreName` tulajdonsága túl`Store1` másolt toohello virtuális várólistája hello vannak `Dashboard` előfizetés.

Hello hello dokumentációjában talál további információt a lehetséges szűrőértékek [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) és [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) osztályok. Lásd még hello [Brokered Messaging: speciális szűrők](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) és [témakör szűrők](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) minták.

## <a name="next-steps"></a>Következő lépések
Lásd: hello alábbi témakörök további információt és példákat használatával a Service Bus üzenetkezelés speciális.

* [Service Bus messaging overview](service-bus-messaging-overview.md) (A Service Bus üzenetkezelésének áttekintése)
* [Service Bus brokered messaging .NET tutorial](service-bus-brokered-tutorial-dotnet.md) (A Service Bus által felügyelt üzenettovábbítás .NET oktatóanyaga)
* [A Service Bus által felügyelt üzenettovábbítás REST oktatóanyaga](service-bus-brokered-tutorial-rest.md)
* [A témakör szűrők minta](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/TopicFilters)
* [A közvetítőalapú üzenetek: Speciális szűrők minta](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

