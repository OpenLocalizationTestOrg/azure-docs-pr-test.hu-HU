---
title: "Azure Service Bus-üzenetsorokat használó alkalmazásokat aaaWrite |} Microsoft Docs"
description: "Hogyan toowrite egy egyszerű várólista-alapú alkalmazás Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 754d91b3-1426-405e-84b4-fd36d65b114a
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 93b75902a06becd6e33e05298e09f5669d0e2aef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-applications-that-use-service-bus-queues"></a>Service Bus-üzenetsorokat használó alkalmazások készítése
Ez a témakör ismerteti a Service Bus-üzenetsorok, és bemutatja, hogyan toowrite várólista alapú egy egyszerű alkalmazást, amely a Service Bus használja.

Vegye figyelembe a kiskereskedelmi értékesítési adatait az egyes Point-of-Sale (POS) terminálok kell lennie történik irányítása tooan készletkezelő rendszer által használt hello adatok toodetermine, ha készlet feltöltést toobe hello world egy olyan forgatókönyvet. A megoldás az, hogy a Service Bus üzenetkezelés hello kommunikációját hello terminálok és hello készletkezelő rendszer, a következő ábra hello ismertetett módon:

![Service Bus-üzenetsorok kép 1](./media/service-bus-create-queues/IC657161.gif)

Minden egyes POS terminál jelent az értékesítési adatokat küldött üzenetek toohello **DataCollectionQueue**. Ezek az üzenetek ebből a várólistából maradni, amíg lehívás hello készletkezelő rendszer által. Ezt a mintát gyakran nevezik *aszinkron üzenetkezelési*, mert hello POS terminál nincs toowait válaszára a hello készlet felügyeleti rendszer toocontinue feldolgozása.

## <a name="why-queuing"></a>Ezért queuing?
Előtt úgy tekintünk hello kódot a szükséges tooset fel ezt az alkalmazást, gondolja át, hello előnyei várólista ebben a forgatókönyvben, ahelyett, hogy hello POS terminálok beszélgetés közvetlen (szinkron) toohello szoftverleltár-kezelő rendszer.

### <a name="temporal-decoupling"></a>Időbeli elválasztás
Hello aszinkron üzenettovábbítási mintának köszönhetően létrehozóknak és a felhasználóknak nem kell toobe online: hello ugyanannyi időt vesz igénybe. hello üzenetküldési infrastruktúra megbízhatóan tárolja az üzeneteket csak hello fogyasztó fél készen tooreceive őket. Ez azt jelenti, hogy hello elosztott alkalmazások összetevői hello is leválasztását, akár önkéntesen; például karbantartás céljából, vagy tooa összetevő összeomlása, az nem befolyásolja a teljes rendszer hello miatt. Ezenkívül hello fel az alkalmazás csak lehet toobe online hello nap bizonyos időpontjaiban. Például ilyenkor kereskedelmi hello készletkezelő rendszer esetén csak rendelkezhetnek toocome online hello munkanap hello vége után.

### <a name="load-leveling"></a>Terheléselosztás
Számos alkalmazás rendszerterhelés időnként eltérő, mivel az egyes Munkaegységek szükséges hello feldolgozási idő jellemzően állandó marad. Létrehozói és felhasználói egy sor azt jelenti, hogy hello fogyasztó alkalmazás (hello munkavégző) csak akkor van toobe közé üzenet helyett a csúcsterhelés betölteni az átlagos tooservice kiépítve. hello mélysége hello várólista mérete akkor nő, és szerződés, a bejövő terhelés hello függően változik. Ez közvetlen megtakarításokkal pénz infrastruktúra szükséges tooservice hello alkalmazásterhelés toohello mennyiségű figyelembe véve.

![Service Bus-üzenetsorok kép 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Terheléselosztás
Hello szerint a terhelés növekedésével, további feldolgozó folyamatok hozzáadott tooread hello munkavégző várólistában lehet. Minden üzenetet csak az egyik hello munkavégző folyamatok dolgoz fel. Továbbá a lekérésalapú terheléselosztás lehetővé teszi az optimális használat hello munkavégző számítógépek akkor is, ha hello munkavégző számítógépek térnek el egymástól, amely legutóbb tooprocessing rendelkezik, kérik le a saját maximális díj üzenetek. Ezt a mintát gyakran nevezik hello versengő fogyasztó mintát.

![Service Bus-üzenetsorok kép 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Laza kapcsoló
Message queuing toointermediate közötti üzenetek létrehozói és felhasználói használatának hello összetevői között egy belső laza kapcsoló. Mivel létrehozói és felhasználói nem kompatibilis, minden más, a fogyasztó frissítése anélkül, hogy hello készítő hatással. Ezenkívül topológia üzenetküldési hello is fejlődnek hello meglévő végpontok befolyásolása nélkül. Mutatjuk Ez több amikor döntésről bővebben közzétételi/előfizetési.

## <a name="show-me-hello-code"></a>Hello kód megjelenítése
a következő szakasz bemutatja hogyan hello toouse Service Bus toobuild ezt az alkalmazást.

### <a name="sign-up-for-an-azure-account"></a>Regisztráljon az Azure-fiók
Rendelés toostart Service Bus használata egy Azure-fiókra lesz szüksége. Ha még nem rendelkezik egy, egy ingyenes fiókot regisztrálhat [Itt](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Névtér létrehozása
Ha rendelkezik előfizetéssel, akkor [létre kell hoznia egy szolgáltatásnévteret](service-bus-create-namespace-portal.md). Minden névtér egy hatókörkezelési tárolót állítja be a Service Bus-entitások funkcionál. Az új névtér adjon meg egy egyedi nevet összes Service Bus-fiók. 

### <a name="install-hello-nuget-package"></a>Hello NuGet-csomag telepítése
toouse hello Service Bus-névtér, egy alkalmazás hello Service Bus-összeállításra, konkrétan Microsoft.ServiceBus.dll kell hivatkoznia. A Microsoft Azure SDK hello részeként szerelvény található, és hello letöltési érhető el: hello [Azure SDK letöltési oldala](https://azure.microsoft.com/downloads/). Azonban hello [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) hello legegyszerűbb módja tooget hello Service Bus API és tooconfigure az alkalmazás az összes Service Bus-függőséggel hello van.

### <a name="create-hello-queue"></a>Hello várólista létrehozása
Felügyeleti műveletek a Service Bus üzenetküldési entitásokat (üzenetsorok és a közzétételi/előfizetési témakörök) hello keresztül hajtja végre [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály. A Service Bus használ egy [közös hozzáférésű Jogosultságkód (SAS)](service-bus-sas.md) -alapú biztonsági modellt. Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) osztály vissza néhány jól ismert jogkivonat-szolgáltatót beépített gyári metódusokat tartalmazó jelenti a biztonsági jogkivonat-szolgáltató. Fogjuk használni egy [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metódus toohold hello SAS-hitelesítőadatait. Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) példány majd össze hello alapszintű címéből hello Service Bus-névtér és jogkivonat-szolgáltató hello.

Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály módszerek toocreate biztosít, enumerálása és üzenetküldési entitások törlése. az itt látható, hogyan hello látható szövegrészt kód hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) példány létrehozott és használt toocreate hello **DataCollectionQueue** várólista.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Vegye figyelembe, hogy vannak-e a hello túlterhelések [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) , amelyek lehetővé teszik a hello várólista toobe beállított tulajdonságainak metódust. Megadhatja például, hogy (hello alapértelmezett--élettartama TTL) értékének toohello várólistára küldött üzenet.

### <a name="send-messages-toohello-queue"></a>Üzenetek toohello várólista küldése
A futásidejű műveletek a Service Bus-entitások; például üzenetek küldése és fogadása, egy alkalmazás először létre kell hoznia egy [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objektum. Hasonló toohello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály, hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) hello alapszintű címéből hello szolgáltatás névtér és jogkivonat-szolgáltató hello példány készült.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Küldött üzeneteket, és kapott a Service Bus üzenetsorok hello példánya [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) osztály. Ez az osztály áll szabványos tulajdonságait (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) és [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), amely használt toohold alkalmazás tulajdonságait, valamint egy tetszőleges alkalmazásadatokból álló törzzsel. Az alkalmazás beállíthatja hello törzs történő bármilyen szerializálható objektumnak a (hello alábbi példa továbbítja a egy **SalesData** hello értékesítési adatait a hello POS terminál képviselő objektum), amely fogja használni a hello [ DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) tooserialize hello objektum. Másik megoldásként egy [adatfolyam](https://msdn.microsoft.com/library/system.io.stream.aspx) objektum megadható.

Ennek legegyszerűbb módja toosend üzenetek tooa megadott várólista, a nagybetűk hello hello **DataCollectionQueue**, toouse van [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) toocreate egy [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objektum közvetlenül a hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) példány.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-hello-queue"></a>Üzenetek fogadása hello várólista
hello várólistában lévő üzenetek tooreceive, használhatja a [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objektumhoz, melyhez közvetlenül hoz létre hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) használatával [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Üzenet fogadók a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**. Hello [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) van beállítva, amikor hello üzenetet fogadó jön létre, mint egy paraméterrel toohello [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) hívható meg.

Hello használatakor **ReceiveAndDelete** módban hello kap egy egylépéses művelet; Ez azt jelenti, hogy a Service Bus hello kérelmet kap, ha azt hello üzenetet, feldolgozottként jelöli meg és toohello alkalmazás visszaadja. **ReceiveAndDelete** mód hello legegyszerűbb modell, és működik a legjobban, mely hello az alkalmazás működését nem dolgoz fel üzenetet, ha a hiba toooccur forgatókönyvek esetén. toounderstand, ez egy olyan esetet esetén, amikor hello fogyasztói hello kérés fogadásához, és majd összeomlik a feldolgozása előtt. Mivel a Service Bus hello üzenetet, feldolgozottként jelölte meg, amikor hello alkalmazás újraindul, és az üzenetek újbóli felhasználása indítása ki fogja hagyni hello hello összeomlás előtt feldolgozott üzenetet.

A **PeekLock** módban hello kap, így a lehető toosupport alkalmazások, amelyek működését zavarják a hiányzó üzenetek kétszakaszos művelet lesz. A Service Bus hello kérelmet kap, ha úgy találja, hello tovább üzenet toobe felhasznált, lezárja tooprevent más fogyasztók fogadni, és toohello alkalmazás visszaadása. Miután hello alkalmazás befejezi hello üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja hello második hello fogadási folyamata meghívásával [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) hello érkezett üzenet. Amikor a Service Bus látja a hello [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) hívás, hello üzenetet, feldolgozottként jelöli.

Két más eredményekkel is előfordulhatnak. Először, ha hello alkalmazás nem tudja tooprocess hello üzenet valamilyen okból kifolyólag, akkor meghívhatja [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) hello fogadott üzenet (ahelyett, hogy [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Ez a Service Bus toounlock üdvözlőüzenetére okoz, és könnyebben elérhető toobe újbóli fogadását, vagy azonos felhasználói hello vagy épp egy másik fogyasztó. Második egy társított hello zárolási időtúllépés van, és ha hello alkalmazás nem tudja feldolgozni a üdvözlőüzenetére hello zárolási idő lejárta előtt (például ha hello alkalmazás összeomlik), majd a Service Bus feloldást üzenet hello révén elérhető toobe ismételt fogadását (lényegében hajt végre egy [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) művelet alapértelmezés szerint).

Vegye figyelembe, hogy ha hello alkalmazás összeomlása után üdvözlőüzenetére feldolgozza, még mielőtt hello [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) kérelmet adtak ki, üdvözlőüzenetére újra toohello alkalmazás lesz, amikor újraindul. Ezt gyakran nevezik * legalább egyszer * feldolgozása. Ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben hello a ugyanazon üzenet újbóli kézbesítése is lehet. Ha hello forgatókönyvben nem lehetségesek, majd további logikát kell megadni hello alkalmazás toodetect duplikált elemek. Ez megvalósítható hello alapján [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) hello üzenet tulajdonságát. Ez a tulajdonság értékének hello állandó marad a kézbesítési kísérletek során. Ezt nevezik *pontosan egyszer* feldolgozása.

hello kód itt látható fogadó és feldolgozó hello üzeneteket **PeekLock** módban, amely hello alapértelmezett Ha nincs [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) érték explicit módon valósul meg.

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

### <a name="use-hello-queue-client"></a>Hello várólista-ügyfél használata
Példák korábbi részében létrehozott hello [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) és [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objektumok közvetlenül a hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) toosend és az üzenetek fogadásához hello várólistában illetve. Egy másik módszert toouse egy [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objektum, amely támogatja a műveletek küldésének és fogadásának hozzáadása toomore a speciális funkció, például munkamenetek.

```csharp
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);

BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a várólisták hello alapjait, lásd: [alkalmazásokat, amelyek a Service Bus-üzenettémák és előfizetések](service-bus-create-topics-subscriptions.md) toocontinue hello közzétételi/előfizetési képességeket a Service Bus-üzenettémakörök használata az ismertető és előfizetések.

