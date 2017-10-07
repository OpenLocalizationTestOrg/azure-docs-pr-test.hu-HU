---
title: "Azure Service Bus használata a teljesítmény fokozása aaaBest eljárásai |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Service Bus toooptimize teljesítmény adatcsere közvetítőalapú üzenetek."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e756c15d-31fc-45c0-8df4-0bca0da10bb2
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2017
ms.author: sethm
ms.openlocfilehash: 52764d227757cbb11246675878933f21685817f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Ajánlott eljárások használatával a Service Bus üzenetkezelés teljesítménnyel kapcsolatos fejlesztések

Ez a cikk ismerteti, hogyan toouse [Azure Service Bus üzenetkezelés](https://azure.microsoft.com/services/service-bus/) toooptimize teljesítmény adatcsere közvetítőalapú üzenetek. Ez a témakör első része hello hello különböző mechanizmusok felkínált toohelp növekedése teljesítmény ismerteti. hello második rész hogyan toouse Service Bus oly módon, hogy kínálhat hello egy adott forgatókönyv esetén a legjobb teljesítményt nyújt útmutatást.

Ez a témakör teljes hello "ügyfél" kifejezés tooany entitás, aki hozzáfér a Service Bus. Egy ügyfél a feladó vagy egy címzett hello szerepkör is igénybe vehet. hello "feladó" kifejezést egy Service Bus-üzenetsor vagy témakör ügyfél által küldött üzenetek tooa Service Bus-üzenetsor vagy témakör. hello "fogadó" kifejezés tooa Service Bus várólista vagy előfizetés ügyfél, amely egy Service Bus-üzenetsorba vagy előfizetés üzeneteket fogad.

Ezek a szakaszok vezethet, hogy a Service Bus toohelp program teljesítmény használja több fogalmakat.

## <a name="protocols"></a>Protokollok
A Service Bus lehetővé teszi az ügyfelek toosend, és három protokollok segítségével üzenetek fogadására:

1. Speciális üzenetsor-kezelési protokoll (AMQP)
2. A Service Bus üzenetkezelés protokoll (SBMP)
3. HTTP

AMQP és SBMP hatékonyabbak, mert megőriznek hello kapcsolat tooService Bus mindaddig, amíg hello üzenetkezelési gyárból létezik. Megvalósít továbbá kötegelés és prefetching. Kivéve, ha explicit módon azt korábban említettük, az összes tartalom ebben a témakörben azt feltételezi, hogy AMQP vagy SBMP hello használata.

## <a name="reusing-factories-and-clients"></a>Újbóli felhasználása a gyárat, illetve az ügyfelek
Service Bus-ügyfélalkalmazást objektumok, például a [QueueClient] [ QueueClient] vagy [MessageSender][MessageSender], keresztül jönnek létre a [MessagingFactory] [ MessagingFactory] objektum, amely belső felügyeleti kapcsolatok is biztosít. Meg nem után zárja be a üzenetkezelési gyárat vagy várólista, a témakör és előfizetés-ügyfelek elküldeni egy üzenetet, és ezután hozza létre őket hello tovább üzenet küldésekor. Hello kapcsolat toohello Service Bus szolgáltatás törlése egy üzenetkezelési gyárból bezárása, és egy új kapcsolat jön létre, amikor hello gyári újbóli létrehozása. A kapcsolat azért, hogy elkerülhető újrafelhasználása drága művelet hello azonos factory és az ügyfél létrehozó objektumokhoz több műveletet. Biztonságosan használhatja hello [QueueClient] [ QueueClient] üzenetküldésre egyidejű aszinkron műveletek és több szál objektum. 

## <a name="concurrent-operations"></a>Párhuzamos műveletek
Egy műveletet (Küldés, kapni, törlés, stb.) némi időt vesz igénybe. Most hello Service Bus szolgáltatás által hello feldolgozási hello művelet szerepel továbbá toohello késését hello helykérelemmel és válasszal hello. műveletek másodpercenkénti idő tooincrease hello száma műveletek végre kell hajtani egyidejűleg. Ez a különféle módokon teheti meg:

* **Az aszinkron műveletek**: hello ügyfél ütemezi műveletek aszinkron műveletek elvégzésével. hello következő küld tartalomkérelmet hello előző kérelem befejeződése előtt. hello az alábbiakban látható egy példa egy aszinkron küldés művelet:
  
 ```csharp
  BrokeredMessage m1 = new BrokeredMessage(body);
  BrokeredMessage m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  Itt látható egy példa egy aszinkron fogadás művelet:
  
  ```csharp
  Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  
  Task.WaitAll(receive1, receive2);
  Console.WriteLine("All messages received");
  
  async void ProcessReceivedMessage(Task<BrokeredMessage> t)
  {
    BrokeredMessage m = t.Result;
    Console.WriteLine("{0} received", m.Label);
    await m.CompleteAsync();
    Console.WriteLine("{0} complete", m.Label);
  }
  ```
* **Több előállítók**: minden ügyfél (hozzáadása tooreceivers feladók listájához) által létrehozott hello azonos gyári megosztás egy TCP-kapcsolatot. hello üzenetek maximális átviteli sebesség Lépkedjen végig a TCP-kapcsolat műveletek hello száma korlátozza. az elérhető egyetlen Factory hello átviteli jelentősen is különbözhet a TCP körbejárási alkalommal és az üzenet mérete. nagyobb átviteli sebesség tooobtain, használjon több üzenetkezelési gyárat.

## <a name="receive-mode"></a>Fogadás módban
A várólista vagy előfizetés ügyfél létrehozásakor megadhatja egy fogadási módot: *betekintés-lock* vagy *fogadásához és törléséhez*. hello alapértelmezett fogadás módban van [PeekLock][PeekLock]. Ebben a módban működő, hello ügyfél egy kérelem tooreceive üzenetet küld a Service Bus. Miután hello ügyfél hello üzenetet kapott, a kérelem toocomplete hello üzenetet küld.

Ha hello beállítása fogadás módban túl[ReceiveAndDelete][ReceiveAndDelete], két lépést a rendszer kombinálja az egy kérelemhez. Ez csökkenti a hello műveletek teljes száma, és javíthatja a hello teljes üzenet teljesítményt. Ez jobb teljesítménye hello üzenetek elvesztését kockáztatja származnak.

A Service Bus fogadni-és-delete művelet esetén nem támogatja a tranzakciókat. Emellett betekintés-lock szemantikáját szükségesek az összes olyan forgatókönyvet, mely hello az ügyfél toodefer szeretne vagy [kézbesítetlen levelek](service-bus-dead-letter-queues.md) üzenetet.

## <a name="client-side-batching"></a>Ügyféloldali kötegelés
Ügyféloldali kötegelés lehetővé teszi, hogy üzenetsor vagy témakör ügyfél toodelay hello küld egy üzenet egy bizonyos ideig. Ha hello ügyfél további üzeneteket küld ebben az időszakban, továbbítja a köszönőüzenetei egyetlen kötegben. A várólista vagy előfizetés ügyfél toobatch is ügyféloldali kötegelés hatására több **Complete** kérelmek az egy kérelemhez. Kötegelés érhető el csak aszinkron **küldése** és **Complete** műveletek. Szinkron műveletek azonnal küldött toohello Service Bus szolgáltatás. Kötegelés nem fordulnak elő a betekintés és fogadási műveletek, és nem az ügyfélre kötegelés fordul elő.

Alapértelmezés szerint az ügyfél egy kötegelt időköz 20ms használja. Hello kötegelt időköz módosításához hello beállítása [BatchFlushInterval] [ BatchFlushInterval] tulajdonság létrehozása előtt hello üzenetkezelési gyárból. Ez a beállítás az adat-előállító által létrehozott összes ügyfélre vonatkozik.

toodisable kötegelés, állítsa be a hello [BatchFlushInterval] [ BatchFlushInterval] tulajdonság túl**TimeSpan.Zero**. Példa:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Kötegelés nem befolyásolja a hello számlázható üzenetkezelési műveletek számát, és csak a Service Bus ügyfél protokoll hello érhető el. hello HTTP protokollt nem támogatja a kötegelés.

## <a name="batching-store-access"></a>Kötegelési áruházhoz való hozzáférés
tooincrease hello átviteli sebességgel üzenetsor, témakör vagy előfizetés, a Service Bus kötegek több üzenetet tooits belső tároló írásakor. Ha engedélyezve van, az üzenetsor vagy témakör, a üzeneteket írna hello tárolóba kötegelni lesz. Ha engedélyezve van a várólista vagy előfizetés, a törlés üzenetek hello áruházból kötegelni lesz. Egy entitás kötegelt áruházhoz való hozzáférés engedélyezve van, ha a Service Bus késlelteti mentése too20ms által érintett entitásként vonatkozó tárolási írási művelet. Ez az időtartam alatt során felmerülő további tárolási műveletek toohello kötegelt kerülnek. Tároló hozzáférés csak befolyásolja kötegelni **küldése** és **Complete** műveletekkel; fogadási műveletek nem érinti. Kötegelt áruházhoz való hozzáférés entitás tulajdonság értéke. Kötegelt áruházhoz való hozzáférés engedélyezése az összes entitások közötti kötegelés következik be.

Amikor egy új várólistát, üzenettémakört vagy előfizetést hoz létre, az alapértelmezés szerint engedélyezve van a kötegelt áruházhoz való hozzáférés. toodisable kötegelni áruházhoz való hozzáférés, set hello [EnableBatchedOperations] [ EnableBatchedOperations] tulajdonság túl**hamis** hello entitás létrehozása előtt. Példa:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Kötegelt áruházhoz való hozzáférés nem befolyásolja a hello számlázható üzenetkezelési műveletek számát, és üzenetsor, témakör vagy előfizetés tulajdonsága. Hello független is kap egy ügyfél és a Service Bus szolgáltatás hello között használt mód és hello protokoll.

## <a name="prefetching"></a>Prefetching
Várólista vagy előfizetés ügyfél tooload további köszönőüzenetei hello szolgáltatás prefetching lehetővé teszi a fogadási művelet végrehajtása során. hello ügyfél a helyi gyorsítótárban tárolja ezeket az üzeneteket. hello hello gyorsítótár mérete határozza meg hello [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] vagy [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] tulajdonságok. Minden ügyfél, amely lehetővé teszi, hogy prefetching tart fenn a saját gyorsítótárába. A gyorsítótár nincsenek megosztva, az ügyfelek között. Ha hello ügyfél kezdeményez egy fogadási művelet és a gyorsítótár üres, a hello szolgáltatás továbbítja az üzenetkötegek. hello köteg mérete hello egyenlő hello gyorsítótár vagy 256 KB-os hello méretét, attól függően, kisebb. Ha hello ügyfél kezdeményez egy fogadási művelet, és hello gyorsítótár tartalmaz egy üzenetet, üdvözlőüzenetére hello gyorsítótárból lesz végrehajtva.

Ha egy üzenet van prefetched, hello szolgáltatás zárolások hello prefetched üzenetet. Ezzel hello prefetched üzenet egy másik címzett nem fogadható. Hello fogadó nem tudja végrehajtani a üdvözlőüzenetére hello zárolási lejárata előtt, ha a üdvözlőüzenetére elérhető tooother fogadók válik. hello üzenet másolatának hello prefetched hello gyorsítótárában marad. hello hello feldolgozó fogadó lejárt gyorsítótárazott másolatot kapnak kivétel, amikor megkísérli toocomplete az üzenet. Alapértelmezés szerint hello üzenet zárolási 60 másodperc múlva lejár. Az érték lehet kiterjesztett too5 perc. a lejárt üzenetek tooprevent hello fogyasztásának hello gyorsítótárméret mindig kisebbnek kell lennie mint hello belül hello zárolási időtúllépés ügyfél által felhasználható üzenetek száma.

Hello alapértelmezett zárolás lejáratának 60 másodperc használatakor jó értékének [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] 20 alkalommal hello maximális feldolgozása hello gyári az összes fogadó mértékét. Például olyan adat-előállítóval 3 fogadók hoz létre, és mindegyik fogadó képes a too10 üzenetek száma másodpercenként. hello előzetes betöltési száma nem lehet hosszabb 20 X 3 X 10 = 600. Alapértelmezés szerint [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] set too0, ami azt jelenti, hogy további üzenetek hello szolgáltatásból beolvasott van.

Prefetching üzenetek növeli a várólista vagy előfizetés teljes teljesítményt hello, mivel csökkenti hello üzenetművelet, vagy a kiszolgálókkal való adatváltások számát a teljes száma. Első üdvözlőüzenetére, beolvasása azonban hosszabb ideig tart (miatt nőtt toohello üzenet mérete). Prefetched üzenetek fogadása gyorsabb lesz, mivel ezek az üzenetek a hello ügyfél már töltve.

hello--élettartama (TTL) tulajdonsága egy üzenet be van jelölve hello kiszolgáló hello a kiszolgáló elküldi az üdvözlő üzenet toohello ügyfél hello időpontban. hello ügyfél nem ellenőrzi a hello üzenet TTL tulajdonsága hello üzenet fogadásakor. Ehelyett üdvözlőüzenetére fogadhatók akkor is, ha az üdvözlő üzenet TTL-t ment át került a gyorsítótárba hello ügyfél köszönőüzenetei során.

Prefetching számlázható üzenetkezelési műveletek hello száma nem érinti, és csak a Service Bus ügyfél protokoll hello érhető el. hello HTTP protokollt nem támogatja a prefetching. Prefetching cím is szinkron és aszinkron műveletek fogadására.

## <a name="express-queues-and-topics"></a>Express üzenetsorok és témakörök

Expressz entitások engedélyezése a nagy mennyiségre és csökkentett késési igényű helyzetekben, és csak a Standard üzenetkezelési csomagra hello támogatott. A létrehozott entitásokat [prémium névterekben](service-bus-premium-messaging.md) nem támogatják a hello expressz lehetőséget. Az expressz entitások Ha egy üzenetet kap tooa üzenetsor vagy témakör, üdvözlőüzenetére nem azonnal tárolja hello üzenetküldési tárolóban. Ehelyett a memóriában vannak gyorsítótárazva. Egy üzenet hello sorban csak néhány másodpercig marad, ha automatikusan írás toostable tárolására, így védelmet alkalmazott rá elvesztésének tooan leállás miatt. A memória-gyorsítótár üdvözlőüzenetére növeli az adatátviteli sebességet, valamint csökkenti a késést, mert nincs semmilyen hozzáférési toostable hello idő üdvözlőüzenetére tárolás zajlik. Néhány másodpercen belül felhasznált üzenetek nem írt toohello üzenetküldési tárolóban. a következő példa hello hoz létre egy express témakör.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Ha nem lehet nem kritikus információkat tartalmazó üzenetet küldött tooan expressz entitás, hello küldő kényszerítheti a Service Bus tooimmediately megőrzéséhez hello üzenet toostable tárolási beállítás hello által [ForcePersistence] [ ForcePersistence] tulajdonság túl**igaz**.

> [!NOTE]
> Expressz entitások nem támogatja a tranzakciókat.

## <a name="use-of-partitioned-queues-or-topics"></a>A particionált várólisták vagy kapcsolatos témakörök
A Service Bus által használt belső, hello azonos csomópont és üzenetküldési tároló tooprocess, és egy üzenetküldési entitásra (üzenetsor vagy témakör) üzenetek tárolásához. A particionált üzenetsor vagy témakör, a hello ugyanakkor, elosztott, több csomópontot és üzenetküldési tárolók. A particionált üzenetsorok és témakörök csak nem fed fel rendszeres üzenetsorok és témakörök-nál nagyobb átviteli, is mutathat felső szintű elérhetőség. toocreate egy particionált entitást set hello [EnablePartitioning] [ EnablePartitioning] tulajdonság túl**igaz**, ahogy az alábbi példa hello. Particionált entitások kapcsolatos további információkért lásd: [particionált üzenetküldési entitások][Partitioned messaging entities].

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Több várólisták használata

Ha nem particionált üzenetsor vagy témakör vagy hello várt nem terhelés egyetlen particionált üzenetsor vagy témakör kezelhetik lehetséges toouse, több üzenetküldési entitások kell használnia. Több entitás használata esetén minden egyes entitásnál egy dedikált ügyfél létrehozása, használata helyett hello azonos ügyfél entitásokhoz.

## <a name="development-and-testing-features"></a>Fejlesztési és tesztelési szolgáltatások

A Service Bus rendelkezik egy olyan funkció, amely kifejezetten a fejlesztési szolgál amely **soha nem használható termelési konfigurációk**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Ha új szabályokat vagy a szűrők toohello témakör ad hozzá, [TopicDescription.EnableFilteringMessagesBeforePublishing][] , amely új szűrőkifejezés hello tooverify a várt módon működik.

## <a name="scenarios"></a>Forgatókönyvek
hello alábbiakban leíró jellemző üzenetkezelési forgatókönyvek és szerkezeti előnyben részesített hello Service Bus-beállítások. (Kevesebb mint 1 üzenet/másodperc) átviteli sebességet sorolják legkisebb, közepes szintű (üzenet/másodperc 1 vagy nagyobb, de 100-nál kevesebb üzenetek/másodperc) és nagy (100 üzenetek/második vagy nagyobb). hello ügyfelek száma besorolt kis (5 vagy kevesebb), mérsékelt (5-nél több értéknek vagy egyenlő too20), és nagy (több mint 20).

### <a name="high-throughput-queue"></a>Nagy átviteli várólista
Cél: Teljes méret hello sebességét, egy adott sorba. küldő és hello száma nem nagy.

* Használjon egy particionált várólista javítja a teljesítményt és rendelkezésre állás.
* általános tooincrease hello küldési – hello várólistára helyezve, több előállítók toocreate üzenetküldők használja. Minden egyes feladó aszinkron műveletek vagy több szál használja.
* általános tooincrease hello arány beérkezés hello várólistából, üzenet előállítók toocreate több fogadóval.
* Az aszinkron műveletek tootake előnyeit, az ügyféloldali kötegelés használja.
* Időköz too50ms tooreduce hello száma a Service Bus ügyfél protokoll átvitelt kötegelés hello beállítása. Több feladók használata esetén növelje a kötegelés időköz too100ms hello.
* Hagyja meg a kötegelt áruházhoz való hozzáférés engedélyezve. Ez növeli a teljes hello hello várólista írható üzenetek továbbításának sebessége.
* Hello előzetes betöltési száma too20 idejének hello maximális feldolgozási sebességet az összes fogadó gyár beállítására. Ez csökkenti a Service Bus ügyfél protokoll átvitelt hello száma.

### <a name="multiple-high-throughput-queues"></a>Több nagy átviteli várólisták
Cél: Több várólisták teljes átviteli sebesség maximalizálása érdekében. hello átviteli sebességgel egyedi várólista közepes vagy magas.

tooobtain maximális átviteli sebesség több várólistában, hello-beállítások használata című részben foglaltak szerint toomaximize hello sebességét, egy adott sorba. Ezenkívül a különböző előállítók toocreate ügyfelek elküldeni vagy fogadni a különböző várólistákból.

### <a name="low-latency-queue"></a>Kis késleltetésű várólista
Cél: Az üzenetsor vagy témakör végpontok közötti késés minimalizálása érdekében. küldő és hello száma nem nagy. hello átviteli hello várólista a kis és közepes.

* Használjon egy particionált várólista jobb rendelkezésre állása.
* Tiltsa le az ügyféloldali kötegelés. hello ügyfél azonnal üzenetet küld.
* Tiltsa le a kötegelt áruházhoz való hozzáférés. hello szolgáltatás azonnal hello toohello üzenettároló ír.
* Ha egy ügyfél használ, állítsa be a hello előzetes betöltési száma too20 alkalommal hello feldolgozási sebessége hello fogadó. Ha több üzenet érkezik hello hello várakozási azonos idő, a Service Bus ügyfél protokoll hello továbbítja azokat minden a hello ugyanannyi időt vesz igénybe. Amikor hello ügyfél hello a következő üzenetet kap, az üzenet már hello helyi gyorsítótárában. hello gyorsítótár kis kell lennie.
* Ha több ügyfél használja, állítsa be a hello előzetes betöltési száma too0. Ezzel az eljárással hello második ügyfél fogadhat második üdvözlőüzenetére hello első ügyfél továbbra is hello első üzenet feldolgozása közben.

### <a name="queue-with-a-large-number-of-senders"></a>Feldolgozási sor küldők sok
Cél: Átviteli sebességgel üzenetsor vagy témakör feladók nagy számú maximalizálása érdekében. Minden egyes küldő mérsékelt sebességet üzeneteket küld. hello fogadók kisméretű.

A Service Bus használatával mentése too1000 létesített egyidejű kapcsolatok tooa üzenetküldési entitás (vagy 5000 AMQP használatával). Ezt a határt hello névtér szinten van érvényben, várólisták/témakörök/előfizetések fedett által névtér egyidejű kapcsolatok hello korlátot. A várólisták Ez a szám megosztott küldők és a fogadók között. Ha az összes 1000 kapcsolat feladók szükségesek, akkor kell cserélni hello várólista témakör és egy-egy előfizetéshez. A témakör too1000 egyidejű kapcsolatok feladóktól, fogad el, mivel hello előfizetés egy további 1000 egyidejű érkező kapcsolatokat fogad fogadók. Ha több mint 1000 egyidejű feladók szükség, hello feladók küldött e-üzenetek toohello Service Bus protokoll HTTP Protokollon keresztül.

toomaximize átviteli hello a következő:

* Használjon egy particionált várólista javítja a teljesítményt és rendelkezésre állás.
* Ha feladók található, egy másik folyamat, használja csak egyetlen gyári folyamatonként.
* Az aszinkron műveletek tootake előnyeit, az ügyféloldali kötegelés használja.
* Használja a kötegelés időköz 20ms tooreduce hello száma a Service Bus ügyfél protokoll átvitelt hello alapértelmezett.
* Hagyja meg a kötegelt áruházhoz való hozzáférés engedélyezve. Ez növeli a teljes hello sebesség, amellyel üzenetek írható hello üzenetsor vagy témakör.
* Hello előzetes betöltési száma too20 idejének hello maximális feldolgozási sebességet az összes fogadó gyár beállítására. Ez csökkenti a Service Bus ügyfél protokoll átvitelt hello száma.

### <a name="queue-with-a-large-number-of-receivers"></a>Feldolgozási sor fogadók nagy számú
Cél: Teljes méret hello fogadási várólista vagy fogadók nagy számú előfizetés aránya. Minden egyes fogadó fogadja az üzeneteket mérsékelt sebességgel. hello feladók kisméretű.

A Service Bus használatával too1000 létesített egyidejű kapcsolatok tooan entitás fel. Ha a várólista 1000-nél több fogadóval igényel, és egy témakör több előfizetéssel kell cserélje le a hello várólista. Minden előfizetés too1000 egyidejű kapcsolatok is támogatja. Azt is megteheti a fogadók hozzáférhet hello várólista hello HTTP protokollon keresztül.

toomaximize átviteli hello a következő:

* Használjon egy particionált várólista javítja a teljesítményt és rendelkezésre állás.
* Minden egyes fogadó egy másik folyamat helyezkedik el, ha csak egyetlen gyári folyamatonként használata
* Fogadók használhatja a szinkron vagy aszinkron műveletek. Megadott hello mérsékelt kap egy egyedi fogadó aránya, teljes kérelem kötegelés ügyféloldali fogadó átviteli nincs hatással.
* Hagyja meg a kötegelt áruházhoz való hozzáférés engedélyezve. Ez csökkenti a hello hello entitás teljes terhelése. Hello is csökkenti, amellyel üzenetek írható hello üzenetsor vagy témakör általános arány.
* Hello előzetes betöltési száma tooa kis érték (például PrefetchCount = 10). Ez megakadályozza, hogy fogadók tétlen, míg a többi fogadó gyorsítótárazott üzenetek nagy számban.

### <a name="topic-with-a-small-number-of-subscriptions"></a>A témakör egy kis mennyiségű előfizetések
Cél: Hello sebességét, a témakör egy kis mennyiségű előfizetések maximalizálása érdekében. Egy üzenet jelenik meg több előfizetés, ami azt jelenti, hogy a kombinált hello kap feletti előfizetéseket, hello küldések nagyobb. hello feladók kisméretű. előfizetésenként fogadók hello száma nem nagy.

toomaximize átviteli hello a következő:

* Használjon egy particionált témakör javítja a teljesítményt és rendelkezésre állás.
* általános tooincrease hello küldési – hello témakörbe, több előállítók toocreate üzenetküldők használja. Minden egyes feladó aszinkron műveletek vagy több szál használja.
* általános tooincrease hello arány beérkezés előfizetésből, üzenet előállítók toocreate több fogadóval. A címzettek, az aszinkron műveletek vagy több szál használja.
* Az aszinkron műveletek tootake előnyeit, az ügyféloldali kötegelés használja.
* Használja a kötegelés időköz 20ms tooreduce hello száma a Service Bus ügyfél protokoll átvitelt hello alapértelmezett.
* Hagyja meg a kötegelt áruházhoz való hozzáférés engedélyezve. Ez növeli a teljes hello hello témakör írható üzenetek továbbításának sebessége.
* Hello előzetes betöltési száma too20 idejének hello maximális feldolgozási sebességet az összes fogadó gyár beállítására. Ez csökkenti a Service Bus ügyfél protokoll átvitelt hello száma.

### <a name="topic-with-a-large-number-of-subscriptions"></a>A témakör egy nagy mennyiségű előfizetések
Cél: Hello sebességét, a témakör az előfizetések nagy számú maximalizálása érdekében. Egy üzenet jelenik meg több előfizetés, ami azt jelenti, hogy a kombinált hello kap feletti összes előfizetését, sokkal nagyobb hello küldések. hello feladók kisméretű. előfizetésenként fogadók hello száma nem nagy.

Témakörök, előfizetések nagy számú általában tehetnek közzé a egy kis teljes teljesítményt, ha az összes üzenet irányított tooall előfizetések tartoznak. Ennek oka hello tényt, hogy minden üzenet jelenik meg többször, és a témakörben található összes üzenet és az összes hello tárolja az előfizetések azonos tárolja. Feltételezzük, hogy hello küldők és száma előfizetésenként fogadók kis. A Service Bus legfeljebb too2, a témakör egy 000 előfizetések támogat.

toomaximize átviteli hello a következő:

* Használjon egy particionált témakör javítja a teljesítményt és rendelkezésre állás.
* Az aszinkron műveletek tootake előnyeit, az ügyféloldali kötegelés használja.
* Használja a kötegelés időköz 20ms tooreduce hello száma a Service Bus ügyfél protokoll átvitelt hello alapértelmezett.
* Hagyja meg a kötegelt áruházhoz való hozzáférés engedélyezve. Ez növeli a teljes hello hello témakör írható üzenetek továbbításának sebessége.
* Hello előzetes betöltési száma too20 idejének fogadni várt hello beállítására meg gyakorisága másodpercben. Ez csökkenti a Service Bus ügyfél protokoll átvitelt hello száma.

## <a name="next-steps"></a>Következő lépések
További információ a Service Bus teljesítmény optimalizálása toolearn lásd: [particionált üzenetküldési entitások][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.servicebus.messaging.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.servicebus.messaging.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval#Microsoft_ServiceBus_Messaging_NetMessagingTransportSettings_BatchFlushInterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations#Microsoft_ServiceBus_Messaging_QueueDescription_EnableBatchedOperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.servicebus.messaging.queueclient.prefetchcount#Microsoft_ServiceBus_Messaging_QueueClient_PrefetchCount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.servicebus.messaging.subscriptionclient.prefetchcount#Microsoft_ServiceBus_Messaging_SubscriptionClient_PrefetchCount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence#Microsoft_ServiceBus_Messaging_BrokeredMessage_ForcePersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing#Microsoft_ServiceBus_Messaging_TopicDescription_EnableFilteringMessagesBeforePublishing
