---
title: "az Azure Event Hubs aaaProgramming útmutató |} Microsoft Docs"
description: "Az Azure Event Hubs hello Azure .NET SDK használatával írhat kódot."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64cbfd3d-4a0e-4455-a90a-7f3d4f080323
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 43bebd126c2311af9e3daeb52324132b66cf0884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-programming-guide"></a>Event Hubs programozási útmutató

A cikk ismerteti az Azure Event Hubs és hello Azure .NET SDK használatával programozás olyan gyakori forgatókönyveket tartalmaz. A témakör feltételezi az Event Hubs szolgáltatással kapcsolatos előzetes ismeretek meglétét. Az Event Hubs fogalmi áttekintése, lásd: hello [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md).

## <a name="event-publishers"></a>Esemény-közzétevők

Események tooan eseményközpont vagy HTTP POST használatával vagy egy AMQP 1.0-kapcsolaton keresztül küld. mely toouse választott hello és mikor hello adott forgatókönyv határozza függ. Az AMQP 1.0-kapcsolatok mérése közvetített kapcsolatként történik a Service Bus szolgáltatásban, és az olyan forgatókönyvekben megfelelőbbek, ahol gyakoriak a nagyobb üzenetmennyiségek és alacsony késés szükséges, mivel ezek állandó üzenetkezelési csatornát biztosítanak.

Létrehozásához és kezeléséhez az Event Hubs használatával hello [NamespaceManager][] osztály. Hello .NET felügyelt API-k, amikor elsődleges hello hoz létre az adatok tooEvent közzétételéhez hubok hello [EventHubClient](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) és [EventData][] osztályok. [EventHubClient][] hello amelyben az események küldhetők toohello event hubs AMQP kommunikációs csatornát biztosít. Hello [EventData][] osztály egy eseményt képvisel, és használt toopublish üzenetek tooan eseményközpontot. Ez az osztály hello törzs, bizonyos metaadatait és fejléc-információit hello esemény tartalmazza. Egyéb tulajdonságokkal is bővül toohello [EventData][] objektumot, ahogy keresztülhalad az eseményközpontban.

## <a name="get-started"></a>Bevezetés

amely támogatja az Event Hubs hello .NET-osztályok a Microsoft.ServiceBus.dll szerelvényben hello szerepelnek. Ennek legegyszerűbb módja tooreference hello Service Bus API és tooconfigure hello az alkalmazás az összes Service Bus-függőséggel hello toodownload hello [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Másik lehetőségként használhatja a hello [Csomagkezelő konzol](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) a Visual Studióban. toodo tehát adja ki a következő parancs a hello hello [Csomagkezelő konzol](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ablakban:

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Eseményközpont létrehozása
Használhatja a hello [NamespaceManager][] toocreate Event Hubs osztályban. Példa:

```csharp
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

A legtöbb esetben ajánlott, hogy használja-e hello [CreateEventHubIfNotExists][] módszerek tooavoid kivételek generálása, ha hello szolgáltatás újraindul. Példa:

```csharp
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Minden Event Hubs létrehozási művelet, beleértve a [CreateEventHubIfNotExists][], szükséges **kezelése** hello névtérben engedélyeit. Ha azt szeretné, hogy a közzétevő vagy felhasználó alkalmazások toolimit hello engedélyeit, elkerülheti ezeket létrehozásiművelet-hívásokat az éles kódban hitelesítő adatok korlátozott engedélyekkel való használatakor.

Hello [EventHubDescription](/dotnet/api/microsoft.servicebus.messaging.eventhubdescription) osztály tartalmazza az eseményközpontban, beleértve a hello engedélyezési szabályokat, hello üzenetek megőrzési idejét, partíciók azonosítóit, állapota és az elérési utat. Ez az osztály tooupdate hello metaadatok eseményközpontban is használhatja.

## <a name="create-an-event-hubs-client"></a>Event Hubs-ügyfél létrehozása
hello az elsődleges osztály az Event Hubs kommunikáció [Microsoft.ServiceBus.Messaging.EventHubClient][EventHubClient]. Ez az osztály küldői és fogadói képességeket is biztosít. Ez az osztály hello használatával példányosítható [létrehozása](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.create) módszer, ahogy az alábbi példa hello.

```csharp
var client = EventHubClient.Create(description.Path);
```

Ez a módszer hello Service Bus kapcsolati információit használja hello App.config fájlban, hello `appSettings` szakasz. Hello példát `appSettings` XML toostore hello Service Bus kapcsolati információit használja, hello hello dokumentációjában [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) metódust.

Másik lehetőség is toocreate hello ügyfél egy kapcsolati karakterláncból. Ez a beállítás jól működik, ha Azure feldolgozói szerepköröket használ mert hello konfigurációs tulajdonságok hello dolgozó hello karakterláncot tárolhatja. Példa:

```csharp
EventHubClient.CreateFromConnectionString("your_connection_string");
```

hello kapcsolati karakterlánc formátuma azonos hello korábbi metódusok App.config fájlt hello megjelenő hello lehet:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[key]
```

Végezetül fontos is lehetséges toocreate egy [EventHubClient][] objektum egy [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) példány, ahogy az alábbi példa hello.

```csharp
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Fontos, hogy további toonote [EventHubClient][] egy üzenetkezelési gyár példányából létrehozott objektumokat szeretné újrafelhasználni hello ugyanazt az alapul szolgáló TCP-kapcsolatot. Ezért ezek az objektumok ügyféloldali korláttal rendelkeznek majd az átvitelre vonatkozóan. Hello [létrehozása](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) metódus újból felhasználja az üzenetkezelési gyárat. Amennyiben nagyon nagy átviteli kapacitás szükséges egyetlen küldőtől, létrehozhat több üzenetkezelési gyárat, illetve egy [EventHubClient][] objektumot mindegyik üzenetkezelési gyárból.

## <a name="send-events-tooan-event-hub"></a>Események tooan eseményközpont küldése
Küldött események tooan eseményközpont hozzon létre egy [EventData][] példányt, és küldje el azt keresztül hello [küldése](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) metódust. Ez a metódus egyetlen veszi [EventData][] példányparamétert és szinkron módon elküldi azt tooan eseményközpontot.

## <a name="event-serialization"></a>Eseményszerializáció
Hello [EventData][] osztályt [négy túlterhelt konstruktorral](/dotnet/api/microsoft.servicebus.messaging.eventdata#constructors_) , amely számos paraméter, például egy objektumot és szerializálót, egy bájttömböt vagy adatfolyam igénybe vehet. Az is lehetséges tooinstantiate hello [EventData][] osztály, és állítsa be ezt követően hello törzs adatfolyam. Amikor a JSON-t használ [EventData][], használhat **Encoding.UTF8.GetBytes()** tooretrieve hello bájttömböt a JSON-kódolású karakterlánc.

## <a name="partition-key"></a>Partíciókulcs
Hello [EventData][] osztály rendelkezik egy [PartitionKey][] tulajdonság, amely lehetővé teszi a hello küldő toospecify egy értéket, amely kivonatolt tooproduce egy partíció-hozzárendelés. A partíciókulcsok használatával biztosítja, hogy az összes hello események ugyanazzal a kulccsal küldött hello toohello azonos hello eseményközpont partíciójához. Az általános partíciókulcsok többek közt a felhasználói munkamenetek azonosítói és az egyedi küldőazonosítók. Hello [PartitionKey][] tulajdonság megadása nem kötelező, és megadható a hello használatakor [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) vagy [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendAsync_Microsoft_ServiceBus_Messaging_EventData_) módszerek. Ha nem ad meg értéket [PartitionKey][], küldött események olyan elosztott toopartitions egy Ciklikus időszeleteléses modell használatával.

### <a name="availability-considerations"></a>Rendelkezésre állási szempontok

A partíciós kulcs használata nem kötelező, és gondosan érdemes-e egy toouse. Sok esetben a partíciókulcsok használatával akkor hasznos, ha fontos esemény rendezés. A partíciós kulcs használata esetén ezek a partíciók egycsomópontos rendelkezésre szükség, és kiesések is időbeli; például, ha a számítási csomópontok újraindítás és a javítás. Ha el a Partícióazonosító és adott partíció valamilyen okból kifolyólag nem érhető el, egy kísérlet tooaccess hello adatok partícióban sikertelen lesz. Ha magas rendelkezésre állású legfontosabb, nem ad meg partíciókulcsot; Ebben az esetben eseményeket a rendszer küld toopartitions a fentiekben ismertetett hello Ciklikus időszeleteléses modell használatával. Ebben a forgatókönyvben teszi rendelkezésre állási (nincs Partícióazonosító) és a konzisztencia (rögzítési események tooa Partícióazonosító) között kifejezett választást.

Egy másik szempont kezeli a késlelteti az események feldolgozásával. Bizonyos esetekben az lehet, hogy jobb toodrop adatokat, és próbálja megismételni mint tootry és tartani a feldolgozás, amely további alárendelt feldolgozási késedelmeket okozhat. Például a tőzsdei árfolyamjelző jobb toowait teljes legfrissebb adatokat, de egy élő vagy gyors, hello adatokat ahelyett, hogy kellene VOIP a forgatókönyv akkor is, ha nem, akkor a teljes.

A rendelkezésre állási lehetőségekért megadott forgatókönyvekben választása hello következő hibakezelés stratégiák egyikét:

- Leállítás (olvasásakor az Event Hubs dolgot megoldásáig leállítás)
- Közvetlen (üzenetek nem fontos, dobja el őket)
- Ismételje meg a (újrapróbálkozási hello üzenetek ismertető fér el)
- [Kézbesítetlen levelek](../service-bus-messaging/service-bus-dead-letter-queues.md) (használja a várólistára vagy egy másik event hub toodead betűs csak hello üzenetek nem tudta feldolgozni)

További információt és kapcsolatos hello kompromisszumot között rendelkezésre állás és a konzisztencia döntéseken: [rendelkezésre állását és az Event Hubs következetes](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Kötegelt eseményküldési műveletek
Az események kötegekben való küldésével jelentősen növelhető az átvitel. Hello [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metódus egy **IEnumerable** típusú paramétert [EventData][] és küld hello atomi művelet toohello eseményközpontban, egész köteget.

```csharp
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Vegye figyelembe, hogy a kötegek nem haladhatja meg az esemény hello 256 KB-os korlátját. Emellett a hello kötegben lévő egyes üzenetek hello használ ugyanazzal a közzétevői identitással. Hello felelőssége, hogy a kötegelt hello hello küldő tooensure nem haladja meg a maximális eseményméret hello. Ha mégis meghaladja, az ügyfél **Küldési** hibát jelez.

## <a name="send-asynchronously-and-send-at-scale"></a>Aszinkron küldés és nagy léptékű küldés
Események tooan eseményközpont aszinkron módon is küldhet. Aszinkron küldés növelheti a hello sebesség, amellyel az ügyfél akkor tudja toosend események. Mindkét hello [küldése](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) és [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metódus egyaránt elérhetők aszinkron verzióban, amely vissza egy [feladat](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objektum. Ezzel a technikával növelhető az átvitel, amíg azt is eredményezheti, hogy hello ügyfél toocontinue toosend események hello Event Hubs szolgáltatás leszabályozta, és a hello ügyfél hibába ütközhet, vagy üzenetek veszhetnek eredményezhet közben is ha nem megfelelően megvalósított. Ezenkívül használhatja a hello [RetryPolicy](/dotnet/api/microsoft.servicebus.messaging.cliententity#Microsoft_ServiceBus_Messaging_ClientEntity_RetryPolicy) hello toocontrol ügyfél újrapróbálkozási beállításainak tulajdonságát.

## <a name="create-a-partition-sender"></a>Partícióküldő létrehozása
Bár a leggyakrabban használt toosend események tooan event hubs nem tartozik partíciós kulcs, bizonyos esetekben érdemes lehet toosend események közvetlenül az adott partíció tooa. Példa:

```csharp
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_CreatePartitionedSender_System_String_) adja vissza egy [EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender) toopublish események tooa adott event hub partíció használt objektum.

## <a name="event-consumers"></a>Eseményfelhasználók
Az Event Hubs két elsődleges modellel rendelkezik az események felhasználásához: közvetlen fogadók és a magasabb szintű absztrakciók, például az [EventProcessorHost][]. Közvetlen fogadók felelőssége saját hozzáférés toopartitions egy fogyasztói csoporton belül összehangolását.

### <a name="direct-consumer"></a>Közvetlen felhasználó
hello legközvetlenebb módja tooread egy partícióról egy fogyasztói csoporton belül toouse hello [EventHubReceiver](/dotnet/apie/microsoft.servicebus.messaging.eventhubreceiver) osztály. Ez az osztály példánya egy toocreate, használnia kell hello példányának [EventHubConsumerGroup](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup) osztály. A következő példa hello hello Partícióazonosító kötelező hello hello felhasználói csoport fogadójának létrehozásakor.

```csharp
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Hello [CreateReceiver](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup#methods_summary) metódus több túlterheléssel rendelkezik, amelyek megkönnyítése hello olvasó vezérlését. Ezek a metódusok lehetnek egy eltolás megadása karakterlánc vagy időbélyeg, és hello toospecify képességét, hogy tooinclude a megadott eltolás a hello adatfolyam, vagy azt követően indítsa el. Hello fogadó létrehozása után megkezdheti az eseményeket a hello objektumot adott vissza. Hello [Receive](/dotnet/api/microsoft.servicebus.messaging.eventhubreceiver#methods_summary) metódus négy túlterheléssel rendelkezik, hogy a vezérlő hello fogadási művelet paramétereit, például a kötegméretet és a várakozási időt. Használhat ezen módszerek tooincrease hello átviteli felhasználóinak aszinkron verzióinak hello. Példa:

```csharp
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

A tekintetben tooa adott partíciók hello üzenetek fogadása hello ahhoz, amelyben toohello eseményközpontba lettek küldve. hello eltolás egy karakterlánc-token használt tooidentify partícióra üzenet.

Vegye figyelembe, hogy egy felhasználói csoportban egy adott partíció egy adott pillanatban nem rendelkezhet 5-nél több egyidejű olvasóval. Mivel az olvasók csatlakoznak vagy lecsatlakoznak, a munkamenetek aktívak maradhatnak hello szolgáltatás észleli, hogy lecsatlakoztak több percig. Ebben az időszakban tooa partíció újracsatlakozás meghiúsulhat. Átfogó példát írása a közvetlen fogadóknak az Event Hubs számára, lásd: hello [Event Hubs közvetlen fogadók](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) minta.

### <a name="event-processor-host"></a>Event Processor Host
Hello [EventProcessorHost][] osztály Eseményközpontokból származó adatokat dolgozza fel. Ez a megvalósítás kell használni, amikor hello .NET platformon hoz létre eseményolvasókat. Az [EventProcessorHost][] egy szálbiztos, több folyamatot lehetővé tevő, biztonságos futtatókörnyezetet biztosít az eseményfeldolgozói megvalósításokhoz, ami lehetővé teszi az ellenőrzőpontok használatát és a partícióbérlés-kezelést is.

toouse hello [EventProcessorHost][] osztály, Megvalósíthat [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor). Ez a felület három metódust tartalmaz:

* [OpenAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_OpenAsync_Microsoft_ServiceBus_Messaging_PartitionContext_)
* [CloseAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_CloseAsync_Microsoft_ServiceBus_Messaging_PartitionContext_Microsoft_ServiceBus_Messaging_CloseReason_)
* [ProcessEventsAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_ProcessEventsAsync_Microsoft_ServiceBus_Messaging_PartitionContext_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__)

toostart Eseményfeldolgozási, példányosítható [EventProcessorHost][], az eseményközpont hello megfelelő paraméterek megadása. Majd, meghívják [RegisterEventProcessorAsync](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost#Microsoft_ServiceBus_Messaging_EventProcessorHost_RegisterEventProcessorAsync__1) tooregister a [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) hello futásidejű végrehajtása. Ezen a ponton a hello állomás megpróbál tooacquire a címbérlet a "mohó" algoritmus használatával hello eseményközpont minden partícióján. Ezek a bérletek egy adott időtartamra szólnak, és azt követően meg kell újítani őket. Ahogy újabb csomópontok worker-példány ebben az esetben ismét online elérhető, azok helyezze, bérletfoglalásokat végeznek, és idővel hello terhelés átvált csomópontok között, mindegyik kísérletek tooacquire több bérletet.

![Event Processor Host](./media/event-hubs-programming-guide/IC759863.png)

Idővel kialakul egy egyensúlyi állapot. A dinamikus képességnek a segítségével processzoralapú automatikus skálázás alkalmazott toobe tooconsumers a felfelé és lefelé méretezéshez egyaránt. Az Event Hubs nem rendelkeznek közvetlen megoldással az üzenetek számlálásához, mert átlagos processzorhasználat gyakran hello legjobb mechanizmus toomeasure hátsó vége vagy felhasználói lépték megállapítására. Ha a közzétevők idővel képes a fogyasztók-nál több esemény toopublish, hello Processzor növelésével feldolgozópéldányok számának automatikus skálázása használt toocause lehet.

Hello [EventProcessorHost][] osztály megvalósít továbbá egy Azure storage-alapú ellenőrzőpont-kezelési mechanizmust. A mechanizmus tárolók hello eltolás partíciónkénti alapon, így mindegyik felhasználó megállapíthatja, hogy milyen hello hello előző felhasználó utolsó ellenőrzőpontja volt. Ahogy a partíciók váltanak között a bérletek csomópontok, ez az hello szinkronizálási mechanizmus, amely elősegíti a terhelés áthelyezését.

## <a name="publisher-revocation"></a>Közzétevők visszavonása
Ezenkívül toohello speciális futásidejű szolgáltatásai [EventProcessorHost][], az Event Hubs lehetővé teszi, hogy a rendelés tooblock adott közzétevők esemény tooan eseményközpont küldjenek a közzétevők visszavonását. Ezek a szolgáltatások akkor igazán hasznosak, ha egy közzétevői token biztonsága sérült, vagy egy szoftverfrissítés okozza-e őket toobehave helytelenül eljárva. Ezekben a helyzetekben hello közzétevő identitása, SAS-token részét képező blokkolható az események közzététele.

További információ a közzétevők visszavonásával és hogyan toosend közzétevőként, tooEvent hubok: hello [Event Hubs nagy méretezési biztonságos közzétételi](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) minta.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs-forgatókönyvekkel toolearn látogasson el ezeket a hivatkozásokat:

* [Event Hubs API overview](event-hubs-api-overview.md) (Event Hubs API – áttekintés)
* [Mi az az Event Hubs](event-hubs-what-is-event-hubs.md)
* [Rendelkezésre állás és konzisztencia az Event Hubsban](event-hubs-availability-and-consistency.md)
* [Event Processor Host – API-referencia](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.servicebus.messaging.eventhubclient
[EventData]: /dotnet/api/microsoft.servicebus.messaging.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
