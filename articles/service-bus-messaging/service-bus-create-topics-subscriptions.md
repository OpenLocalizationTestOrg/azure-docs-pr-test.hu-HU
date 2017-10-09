---
title: "Azure Service Bus-üzenettémák és előfizetések használó aaaCreate alkalmazások |} Microsoft Docs"
description: "Bevezetés toohello közzétételi-feliratkozási képességek a Service Bus-üzenettémák és előfizetések által kínált."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a48fc9b0-b7b0-464e-8187-a517bf8b4eb4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: sethm
ms.openlocfilehash: f6d7de46ace7bd5b49de612db213ced789308d91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Alkalmazások, amelyek a Service Bus-üzenettémák és előfizetések létrehozása
Az Azure Service Bus felhőalapú, üzenet célú köztes technológiák – például a megbízható üzenetsor és a tartós közzétételi/előfizetési üzenetküldési támogat. Ebben a cikkben található információk hello épít [létrehozása a Service Bus-üzenetsorokat használó alkalmazásokat](service-bus-create-queues.md) és a Bevezetés a Service Bus-témakörök által kínált toohello közzétételi/előfizetési képességeket biztosít.

## <a name="evolving-retail-scenario"></a>Fenyegető kereskedelmi forgatókönyv
Ez a cikk továbbra is hello kereskedelmi forgatókönyvben használt [létrehozása a Service Bus-üzenetsorokat használó alkalmazásokat](service-bus-create-queues.md). Visszahívása, hogy egyes pénztári pont (POS) részére értékesítési adatait irányított tooan készletkezelő rendszer adott adatok toodetermine használja, ha készlet toobe feltöltést kell lennie. Minden egyes POS terminál jelent az értékesítési adatokat küldött üzenetek toohello **DataCollectionQueue** várólista, ahol azok marad által hello készletkezelő rendszer esetén, itt látható módon:

![A Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Ebben az esetben egy új követelményt lett tooevolve toohello rendszer hozzáadott: hello áruház gazdája által toobe képes toomonitor hogyan hello azonosításakor valós időben.

tooaddress ezt a követelményt hello rendszer kell "koppintson a" hello értékesítési adatfolyam ki. Szeretnénk továbbra is minden által küldött üzenet hello POS terminálok toobe toohello készletkezelő rendszer esetén, mielőtt küldött, de azt szeretnénk, hogy használhatunk toopresent hello irányítópult nézet toohello áruház gazdája minden üzenetet egy másik példánya.

Ez minden üzenet toobe több felek által felhasznált szükség például minden helyzetben is használhatja a Service Bus *témakörök*. A témakörök a közzététel/előfizetés mintát, amelyben egyes közzétett üzenetek tooone elérhetővé szeretné tenni, vagy legalább egy előfizetésre regisztrálva a hello témakör. Ezzel szemben az üzenetsorok minden üzenetet kapja egy-egy fogyasztó.

Üzenetek küldése tooa témakör hello azonos módon tooa várólista elküldi őket. Azonban üzenetek nem érkező hello témakör közvetlenül; az előfizetések kaptak. Az eltolásokat tekintheti egy előfizetés tooa témakör, amely megkapja a hello toothat témakör küldött üzenetek példányait virtuális várólista. Üzenetek fogadása egy előfizetésből hello ugyanaz, mint az üzenetsorból érkező.

Ha visszalép toohello kereskedelmi forgatókönyv, hello várólista helyébe a témakör és előfizetés jelenik meg, melyik hello készlet felügyeleti rendszerösszetevő használhatja. hello rendszer megjelenik a következőképpen:

![A Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Itt hello konfigurációs azonos toohello előző várólista alapú tervezési végez. Ez azt jelenti, hogy toohello témakör küldött üzenetek irányított toohello **készlet** előfizetést, mely hello **készletkezelő rendszer** használ fel őket.

A rendelés toosupport hello kezelési irányítópult azt hozzon létre egy második előfizetés hello témakör, itt látható módon:

![A Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Ezzel a konfigurációval a minden üzenet hello POS részére legyen elérhető tooboth hello **irányítópult** és **készlet** előfizetések.

## <a name="show-me-hello-code"></a>Hello kód megjelenítése
hello cikk [létrehozása a Service Bus-üzenetsorokat használó alkalmazásokat](service-bus-create-queues.md) ismerteti, hogyan toosign Azure-fiókot és létre kell hoznia egy szolgáltatásnévteret. hello legegyszerűbb módja tooreference Service Bus-függőséggel a Service Bus tooinstall hello [Nuget-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus/). Megtalálhat hello Service Bus-kódtárak hello Azure SDK részeként. hello letöltési érhető el: hello [Azure SDK letöltési oldala](https://azure.microsoft.com/downloads/).

### <a name="create-hello-topic-and-subscriptions"></a>Hello témakör és előfizetés létrehozása
Felügyeleti műveletek a Service Bus üzenetküldési entitásokat (üzenetsorok és a közzétételi/előfizetési témakörök) hello keresztül hajtja végre [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) osztály. Megfelelő hitelesítő adatok szükségesek a rendelés toocreate egy **NamespaceManager** egy adott névtér-példányt. A Service Bus használ egy [közös hozzáférésű Jogosultságkód (SAS)](service-bus-sas.md) -alapú biztonsági modellt. Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) osztály vissza néhány jól ismert jogkivonat-szolgáltatót beépített gyári metódusokat tartalmazó jelenti a biztonsági jogkivonat-szolgáltató. Fogjuk használni egy [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metódus toohold hello SAS-hitelesítőadatait. Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) példány majd össze hello alapszintű címéből hello Service Bus-névtér és jogkivonat-szolgáltató hello.

Hello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) osztály módszerek toocreate biztosít, enumerálása és üzenetküldési entitások törlése. az itt látható, hogyan hello látható szövegrészt kód hello **NamespaceManager** példány létrehozott és használt toocreate hello **DataCollectionTopic** témakör.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Vegye figyelembe, hogy vannak-e a hello túlterhelések [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) , amelyek lehetővé teszik hello témakör tulajdonságainak tooset metódust. Például beállíthat (hello alapértelmezett--élettartama TTL) értékének toohello témakör üzeneteket. Ezután adja hozzá a hello **készlet** és **irányítópult** előfizetések.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-toohello-topic"></a>Üzenetek toohello témakör küldése
A futásidejű műveletek a Service Bus-entitások; például üzenetek küldése és fogadása, egy alkalmazás először létre kell hoznia egy [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory) objektum. Hasonló toohello [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) osztály, hello **MessagingFactory** hello alapszintű címéből hello szolgáltatás névtér és jogkivonat-szolgáltató hello példány készült.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

A Service Bus-témaköröktől, kapott küldött üzenetek tooand hello példánya [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) osztály. Ez az osztály áll szabványos tulajdonságait (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) és [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), amely használt toohold alkalmazás tulajdonságait, valamint egy tetszőleges alkalmazásadatokból álló törzzsel. Az alkalmazás beállíthatja hello törzs történő bármilyen szerializálható objektumnak a (hello alábbi példa továbbítja a egy **SalesData** hello értékesítési adatait a hello POS terminál képviselő objektum), amely fogja használni a hello [ DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) tooserialize hello objektum. Másik megoldásként egy [adatfolyam](https://msdn.microsoft.com/library/system.io.stream.aspx) objektum megadható.

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

hello legegyszerűbb módja toosend üzenetek toohello témakör toouse [CreateMessageSender](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) toocreate egy [MessageSender](/dotnet/api/microsoft.servicebus.messaging.messagesender) objektum közvetlenül hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) példány:

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Üzenetek fogadása egy előfizetés
Hasonló toousing várólistákat, is használhat előfizetés üzeneteit tooreceive egy [MessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) objektumhoz, melyhez közvetlenül hoz létre hello [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) használatával [ CreateMessageReceiver](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Használhatja a hello két különböző kap módok (**ReceiveAndDelete** és **PeekLock**), a bemutatott [létrehozása a Service Bus-üzenetsorokat használó alkalmazásokat](service-bus-create-queues.md).

Vegye figyelembe, hogy amikor létrehoz egy **MessageReceiver** előfizetések hello *entityPath* paraméter hello űrlap `topicPath/subscriptions/subscriptionName`. Ezért toocreate egy **MessageReceiver** a hello **készlet** hello-előfizetése **DataCollectionTopic** témakörben *entityPath*be kell állítani túl`DataCollectionTopic/subscriptions/Inventory`. hello kódot a következőképpen jelenik meg:

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
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

## <a name="subscription-filters"></a>Előfizetés-szűrők
Ebben a forgatókönyvben az eddigi toohello témakör küldött összes üzenet válnak regisztrált elérhető tooall előfizetések. hello itt kulcs kifejezés "legyen elérhető." Miközben Service Bus-előfizetések toohello témakör küldött összes üzenet megtekintéséhez másolhatja azokat üzenetek toohello virtuális előfizetés üzenetsorába csak egy részét. Ez történik, az előfizetést *szűrők*. Előfizetés létrehozásakor megadhat egy kifejezést üdvözlőüzenetére hello tulajdonságainak keresztül működő SQL92 stílus predikátum hello formájában, mindkét rendszer tulajdonságai hello (például [címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) és hello alkalmazás Tulajdonságok, például a **storename –** hello előző példában.

Folyamatosan változó hello forgatókönyv tooillustrate ez, egy második tároló a toobe hozzáadott tooour kereskedelmi lehetőséget. Az összes hello POS terminálok mindkét áruházakból ábrázolhatja az értékesítési adatokat továbbra is fennáll irányított toobe központi toohello készletkezelő rendszer esetén, de egy tárolókezelő hello irányítópult eszközzel csak érdekli hello teljesítményének ezt a tárolót. Szűrés tooachieve ez előfizetéseket használhat. Vegye figyelembe, hogy közzétételekor hello POS terminálok üzenetek, azokat be hello **storename –** üdvözlőüzenetére alkalmazás tulajdonságát. Két áruházak, például adott **Redmond** és **Budapest**, hello Redmond hello POS terminálok tárolja az üzeneteket az értékesítési adatokat stamp egy **storename –** túl egyenlő **Redmond**, mivel hello Budapest tárolására POS terminálok használja egy **storename –** túl egyenlő**Budapest**. hello tárolókezelő hello Redmond tárolására csak a szeretne toosee adatok a POS részére. hello rendszer a következőképpen jelenik meg:

![Szolgáltatás-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

az Útválasztás tooset, létrehozhat hello **irányítópult** előfizetés az alábbiak szerint:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Ennek [előfizetésszűrő](/dotnet/api/microsoft.servicebus.messaging.sqlfilter), csak a hello üzenetek **storename –** tulajdonsága túl**Redmond** lesz másolt toohello virtuális várólistája hello **Irányítópult** előfizetés. Szűrés, azonban sokkal több toosubscription van. Alkalmazások is rendelkeznek több Állapotszűrő szabályok hozzáadása toohello képességét toomodify hello tulajdonságai üzenet előfizetésenként tooa előfizetés virtuális üzenetsorában adja át.

## <a name="summary"></a>Összefoglalás
Összes hello okok toouse queuing ismertetett [létrehozása a Service Bus-üzenetsorokat használó alkalmazásokat](service-bus-create-queues.md) alkalmazni tootopics, kifejezetten is:

* Időbeli elválasztás – üzenetek létrehozói és felhasználói nem rendelkezik toobe online hello ugyanannyi időt vesz igénybe.
* Terheléselosztás – hello témakör engedélyezése helyett csúcsterhelés átlagos terheléssel kiépítve fogyasztó alkalmazások toobe által az csúcsait a terhelés vannak Görbített.
* Terheléselosztás – hasonló tooa várólista, akkor segítségével figyelést egy-egy előfizetéshez, a minden üzenet tooonly hello fogyasztók, ezáltal terheléselosztás feladatkiosztás több versengő fogyasztó számára.
* Laza kapcsoló – akkor is fejlődnek hello hálózati üzenetküldési nem befolyásolja a már meglévő végpontok; például előfizetések hozzáadásának vagy kicserélésének szűrők tooa témakör tooallow új fogyasztók számára.

## <a name="next-steps"></a>Következő lépések

Lásd: [létrehozása a Service Bus-üzenetsorokat használó alkalmazásokat](service-bus-create-queues.md) hogyan toouse várólisták hello POS kereskedelmi forgatókönyvben kapcsolatos információkat.

