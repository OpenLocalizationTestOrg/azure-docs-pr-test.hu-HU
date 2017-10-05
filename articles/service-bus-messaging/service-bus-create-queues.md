---
title: "Alkalmazások írását, amelyek az Azure Service Bus-üzenetsorok használata |} Microsoft Docs"
description: "Egy egyszerű várólista-alapú alkalmazás Azure Service Bus módját."
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
ms.openlocfilehash: 419caff7e8ceeb419c89a2ef9a6614c1accf3e52
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-applications-that-use-service-bus-queues"></a>Service Bus-üzenetsorokat használó alkalmazások készítése
Ez a témakör ismerteti a Service Bus-üzenetsorok, és bemutatja, hogyan használja a Service Bus egyszerű, a várólista-alapú alkalmazás.

Fontolja meg egy olyan forgatókönyvet, amelyben egyes Point-of-Sale (POS) részére ábrázolhatja az értékesítési adatokat kell irányítani olyan készletkezelő rendszer esetén, amelyek az adatok az segítségével állapítja meg, ha készlet feltöltést kiskereskedelmi a globális nézetről. A megoldás az, hogy a Service Bus üzenetkezelés a kapcsok és a készletkezelő rendszer esetén, közötti kommunikációhoz, az alábbi ábra mutatja be:

![Service Bus-üzenetsorok kép 1](./media/service-bus-create-queues/IC657161.gif)

Minden egyes POS terminál jelentések az értékesítési adatokat küldött üzeneteket a **DataCollectionQueue**. Ezek az üzenetek ebből a várólistából maradni, amíg lehívás a készletkezelő rendszer által. Ezt a mintát gyakran nevezik *aszinkron üzenetkezelési*, mert a POS terminál nem rendelkezik a készletkezelő rendszer esetén a folytatáshoz feldolgozási válasz vár.

## <a name="why-queuing"></a>Ezért queuing?
Előtt úgy tekintünk, a kód szükséges ahhoz, hogy állítsa be ezt az alkalmazást, gondolja át, ebben a forgatókönyvben, ahelyett, hogy a POS kapcsok várólista előnye közvetlenül kommunikálni (szinkron) a készlet felügyeleti rendszerbe.

### <a name="temporal-decoupling"></a>Időbeli elválasztás
Az aszinkron üzenettovábbítási mintának létrehozóknak és a felhasználóknak nem kell egyszerre online lenniük. Az üzenetküldési infrastruktúra megbízhatóan tárolja az üzeneteket, amíg a fogyasztó fél készen áll a fogadásukra. Ez azt jelenti, hogy az elosztott alkalmazás összetevőinek is leválasztását, akár önkéntesen; például karbantartás céljából, vagy egy összetevő összeomlása miatt, anélkül érinti az egész rendszerhez. A fogyasztó alkalmazás továbbá csak lehet, hogy rendelkezik a nap bizonyos időpontjaiban online lennie. Például kereskedelmi ebben a forgatókönyvben a készletkezelő rendszer esetén csak rendelkezhet a munkanapok végén online állapotba.

### <a name="load-leveling"></a>Terheléselosztás
Számos alkalmazás rendszerterhelés időnként eltérő, mivel az egyes Munkaegységek szükséges feldolgozási idő jellemzően állandó marad. Közé üzenetek létrehozói és felhasználói üzenetsorokat jelenti, hogy a felhasználó alkalmazást (a feldolgozót) csak a csúcsterhelés helyett az átlagos terheléssel szolgáltatás kell létrehozni. A várólista mélységét nő, és a szerződést, mivel a bejövő terhelés változik. Ez közvetlen megtakarításokkal pénz alkalmazásterhelés kiszolgálásához szükséges infrastruktúraméret mennyiségének tekintetében.

![Service Bus-üzenetsorok kép 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Terheléselosztás
Ha a terhelés növekszik, további feldolgozó folyamatok adhatók a worker-várólistájának olvasásakor. Az egyes üzeneteket a feldolgozó folyamatoknak csak az egyike dolgozza fel. Ezenkívül a lekérésalapú terheléselosztás lehetővé teszi a munkavégző számítógépek optimális használat akkor is, ha a munkavégző számítógépek feldolgozási teljesítmény tekintetében különböznek, kérik le a saját maximális díj üzenetek. Ezt a mintát gyakran nevezik versengő fogyasztó mintát.

![Service Bus-üzenetsorok kép 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Laza kapcsoló
Az összetevők közötti egy belső laza kapcsoló üzenetsor-kezelési üzenetek létrehozói és felhasználói között haladó segítségével biztosítja. Mivel létrehozói és felhasználói nem kompatibilis, minden más, a fogyasztó anélkül, hogy a hatása, ha a gyártó a frissítése. Ezenkívül a üzenetküldés topológiája is fejleszteni a meglévő végpontok befolyásolása nélkül. Mutatjuk Ez több amikor döntésről bővebben közzétételi/előfizetési.

## <a name="show-me-the-code"></a>A kód bemutatása
A következő szakasz bemutatja, hogyan hozható létre a alkalmazás Service Bus használatára.

### <a name="sign-up-for-an-azure-account"></a>Regisztráljon az Azure-fiók
Ahhoz, hogy dolgozni a Service Bus egy Azure-fiókra lesz szüksége. Ha még nem rendelkezik egy, egy ingyenes fiókot regisztrálhat [Itt](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Névtér létrehozása
Ha rendelkezik előfizetéssel, akkor [létre kell hoznia egy szolgáltatásnévteret](service-bus-create-namespace-portal.md). Minden névtér egy hatókörkezelési tárolót állítja be a Service Bus-entitások funkcionál. Az új névtér adjon meg egy egyedi nevet összes Service Bus-fiók. 

### <a name="install-the-nuget-package"></a>Telepítse a NuGet-csomagot
A Service Bus-névtér használatához az alkalmazás a Service Bus-összeállításra, konkrétan Microsoft.ServiceBus.dll kell hivatkoznia. A Microsoft Azure SDK részeként a szerelvény található, és a letöltés érhető el: a [Azure SDK letöltési oldala](https://azure.microsoft.com/downloads/). Azonban a [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) van a Service Bus API beszerzésének, valamint az alkalmazások az összes Service Bus-függőséggel való konfigurálásának legegyszerűbb módja.

### <a name="create-the-queue"></a>Az üzenetsor létrehozása
Felügyeleti műveletek a Service Bus üzenetküldési entitásokat (üzenetsorok és a közzétételi/előfizetési témakörök) keresztül hajtja végre a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály. A Service Bus használ egy [közös hozzáférésű Jogosultságkód (SAS)](service-bus-sas.md) -alapú biztonsági modellt. A [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) osztály vissza néhány jól ismert jogkivonat-szolgáltatót beépített gyári metódusokat tartalmazó jelenti a biztonsági jogkivonat-szolgáltató. Fogjuk használni egy [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) módszer a SAS-hitelesítő adatok tárolásához. A [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) példány majd össze a Service Bus-névtér és jogkivonat-szolgáltató alapszintű címéhez.

A [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály létrehozása, enumerálása és üzenetküldési entitások törlése metódusokat biztosít. A kód itt látható bemutatja hogyan a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) példány létre, amellyel a **DataCollectionQueue** várólista.

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

Vegye figyelembe, hogy nincsenek a túlterhelések a [CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_) módszer, amelynek kell beállítani a várólista-tulajdonságok engedélyezése. Például beállíthatja az alapértelmezett--élettartama (TTL) érték a várólistára küldött üzenetek.

### <a name="send-messages-to-the-queue"></a>Üzenetek küldése az üzenetsorba
A futásidejű műveletek a Service Bus-entitások; például üzenetek küldése és fogadása, egy alkalmazás először létre kell hoznia egy [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objektum. Hasonló a [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) osztály, a [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) példány létrehozása az alapszintű címéből a szolgáltatásnévtér és a jogkivonat-szolgáltató.

```csharp
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Küldött üzeneteket, és kapott a Service Bus üzenetsorok példánya a [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) osztály. Ez az osztály áll szabványos tulajdonságait (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) és [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), alkalmazástulajdonságok tárolására használt, valamint egy tetszőleges alkalmazásadatokból álló törzzsel. Az alkalmazás beállíthatja a szervezet történő bármilyen szerializálható objektumnak a (a következő példa továbbítja a egy **SalesData** objektum, amely az értékesítési adatait jelöli a POS terminál), amely fogja használni a [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) szerializálja az objektumot. Másik megoldásként egy [adatfolyam](https://msdn.microsoft.com/library/system.io.stream.aspx) objektum megadható.

Üzenetek küldése egy adott várólistában, abban az esetben, ha a legegyszerűbb módja a **DataCollectionQueue**, használatával [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) létrehozásához egy [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) közvetlenül objektum az a [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) példány.

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Üzenetek fogadása az üzenetsorból
Üzenetek fogadása az üzenetsorból, használhatja a [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objektumhoz, melyhez közvetlenül hoz létre a [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) használatával [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Üzenet fogadók a két különböző módban tudnak működni: **ReceiveAndDelete** és **PeekLock**. A [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) van beállítva, amikor az üzenetet fogadó jön létre, mint egy paraméterrel a [CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_) hívható meg.

Használatakor a **ReceiveAndDelete** , módot a fogadás egy egylépéses művelet –; Ez azt jelenti, hogy a Service Bus a kérelmet kap, ha azt az üzenetet, feldolgozottként jelöli meg, és visszaadja az alkalmazásnak. **ReceiveAndDelete** mód a legegyszerűbb modell, és működik a legjobban forgatókönyvek, amelyben az alkalmazás működését nem dolgoz fel üzenetet, ha hiba történik. Ennek megértéséhez képzeljen el egy forgatókönyvet, amelyben a fogyasztó kiad egy fogadási kérést, majd összeomlik a feldolgozása előtt. Mivel a Service Bus az üzenetet, feldolgozottként jelölte meg, ha az alkalmazás újraindulna és üzenetek újbóli felhasználása indítása ki fogja hagyni a az összeomlás előtt feldolgozott üzenetet.

A **PeekLock** módban, a fogadás válik a kétszakaszos művelet, amely lehetővé teszi az alkalmazások támogatását, amelyek működését zavarják a hiányzó üzenetek. Amikor a Service Bus a kérést kap, azt a következő feldolgozandó üzenetet talál, zárolja azt, hogy más fogyasztók ne fogadni és majd visszaadja az alkalmazásnak. Miután az alkalmazás befejezi az üzenet feldolgozását (vagy megbízható módon tárolja a jövőbeli feldolgozáshoz), végrehajtja a fogadási folyamat második a [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) meghívásával a fogadott üzenethez. Amikor a Service Bus látja a [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) hívás, azt az üzenetet, feldolgozottként jelöli.

Két más eredményekkel is előfordulhatnak. Először, ha az alkalmazás nem tudja feldolgozni az üzenetet valamilyen okból kifolyólag, akkor meghívhatja [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) metódus a fogadott üzenethez (ahelyett, hogy [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Ennek hatására a Service Bus feloldja az üzenet zárolását, és lehetővé teszi az ugyanazon felhasználó vagy épp egy másik fogyasztó ismételt fogadását. Második a zárolás társított egy időtúllépés van és ha az alkalmazás nem tudja feldolgozni az üzenetet a zárolási idő lejárta előtt (például, ha az alkalmazás összeomlik), majd Service Bus feloldja az üzenet zárolását, és meg fogja teszi érhető fogadott újra) lényegében hajt végre egy [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) művelet alapértelmezés szerint).

Vegye figyelembe, hogy ha az alkalmazás összeomlik után feldolgozza az üzenetet előtt azonban a [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) kérelmet adtak ki, az üzenet újból kézbesítve lesz az alkalmazás amikor újraindul. Ezt gyakran nevezik * legalább egyszer * feldolgozása. Ez azt jelenti, hogy minden üzenet legalább egyszer dolgoz fel, de bizonyos helyzetekben a a ugyanazon üzenet újbóli kézbesítése is lehet. Ha a forgatókönyvben nem lehetségesek, majd további logikát ismétlődések észlelése az alkalmazásban van szükség. Ez megvalósítható alapján a [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) az üzenet tulajdonságát. Ez a tulajdonság értékének állandó marad a kézbesítési kísérletek során. Ezt nevezik *pontosan egyszer* feldolgozása.

Az itt látható kóddal fogadja és dolgozza fel, egy üzenet használatával a **PeekLock** módját, amely az alapértelmezett beállítás, ha nincs [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode) érték explicit módon valósul meg.

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

### <a name="use-the-queue-client"></a>A várólista-ügyfél használata
A Példák korábbi részében létrehozott [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) és [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) közvetlenül a következő helyről objektumokat a [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) küldeni és fogadni az üzenetek a feldolgozási sor, illetve. Egy másik módszert is, hogy használja a [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) objektum, amely támogatja a műveletek küldésének és fogadásának összetettebb funkciók, például a munkamenetek mellett.

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
Most, hogy megismerte az üzenetsorok alapjait, lásd: [alkalmazásokat, amelyek a Service Bus-üzenettémák és előfizetések](service-bus-create-topics-subscriptions.md) folytatja az ismertető a Service Bus-üzenettémák és előfizetések közzététel/előfizetés képességeivel.

