---
title: "aaaAzure tárolási sorok és a Service Bus-üzenetsorok - képest és ellentétben |} Microsoft Docs"
description: "Elemzi az Azure által kínált várólisták két típusú közötti Hasonlóságok és különbségeket."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: f8b915e73ea3c82d823a96bf23c8c9e24c96aa42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Tárolási sorok és a Service Bus-üzenetsorok - képest és ellentétben
Ez a cikk elemzi és a Microsoft Azure által kínált ma várólisták hello két típusú Hasonlóságok hello különbségeket: tárolási sorok és a Service Bus-üzenetsorok. Ez az információ segítségével is hasonlítsa össze és hello megfelelő technológiák ezzel szemben, és lehet egy több tájékozott döntést, mely megoldás kapcsolatos igényeinek megfelelő képes toomake.

## <a name="introduction"></a>Bevezetés
Azure várólista mechanizmusok két típusokat támogatja: **tárüzenetsort** és **Service Bus-üzenetsorok**.

**Tárolási sorok**, hello részét képező [az Azure storage](https://azure.microsoft.com/services/storage/) infrastruktúra, a szolgáltatás egy egyszerű, REST-alapú GET/PUT/betekintés felületet biztosít a megbízható, állandó üzenetkezelési belül, és a szolgáltatások közötti.

**Service Bus-üzenetsorok** részei egy szélesebb körű [Azure messaging](https://azure.microsoft.com/services/service-bus/) infrastruktúra, amely támogatja továbbá az közzétételi/előfizetési queuing, és további speciális integrációs kombinációját. A Service Bus üzenetsorok/témakörök/előfizetések kapcsolatos további információkért lásd: hello [Service Bus áttekintése](service-bus-messaging-overview.md).

Léteznek két üzenetsor-kezelési technológia egyszerre, tárüzenetsort először egy dedikált várólista tárolási mechanizmus épülő Azure Storage szolgáltatás lett bevezetve. Service Bus-üzenetsorok szélesebb körű "üzenetküldési" tervezett infrastruktúra toointegrate alkalmazások vagy az alkalmazás-összetevők, amelyek több kommunikációs protokollokat, adategyezményeinek, megbízhatósági tartományok, illetve hálózati környezetben előfordulhat, hogy több hello épülnek.

## <a name="technology-selection-considerations"></a>Technológia kiválasztása kapcsolatos szempontok
Tárolási sorok és a Service Bus-üzenetsorok hello a message queuing szolgáltatás a Microsoft Azure által biztosított jelenleg külső implementációja. Mindegyiknek némileg eltérő szolgáltatáskészlet, ami azt jelenti, válasszon egyet vagy egyéb hello, vagy attól függően, hogy az adott megoldás vagy üzleti vagy műszaki problémamegoldó hello igényeit is használja.

Annak meghatározása, hogy mely üzenetsor-kezelési technológia megfelel egy adott megoldás hello célját, megoldás mérnökök és a fejlesztők vegye figyelembe az alábbi hello javaslatok. További részletekért lásd a hello következő szakaszt.

Megoldás felelős mérnök vagy fejlesztők **érdemes használni a tárolási sorok** során:

* Az alkalmazás több mint 80 GB üzenetet kell tárolnia a sorhoz, ahol köszönőüzenetei élettartama rövidebb, mint 7 nap.
* Az alkalmazás hello várólista belül egy üzenet feldolgozása szeretne tootrack folyamatban van. Ez akkor hasznos, ha egy üzenet feldolgozása hello munkavégző összeomlik. A későbbi feldolgozók használhatja, hogy információkat toocontinue, ahol abbahagyta hello előzetes munkavégző a.
* Az összes, a várólisták elleni végre hello tranzakciók ügyféloldali kiszolgálónapló van szüksége.

Megoldás felelős mérnök vagy fejlesztők **érdemes használni a Service Bus-üzenetsorok** során:

* A megoldás képes tooreceive üzenetek anélkül, hogy toopoll hello várólista kell lennie. A Service busszal, ez elérhető keresztül hello hello hosszú-lekérdezési használatát fogadási művelethez hello TCP-alapú protokoll, amely támogatja a Service Bus használatával.
* A megoldáshoz szükségesek hello várólista tooprovide egy garantált első-az első-kimenő (FIFO) rendelt kézbesítését.
* Azt szeretné, hogy egy szimmetrikus tapasztalattal az Azure-ban és a Windows Server (magánfelhő). További információkért lásd: [Service Bus Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).
* A megoldás képes toosupport automatikus kettős észlelés kell lennie.
* Azt szeretné, hogy az alkalmazás tooprocess üzenetek párhuzamos hosszan futó adatfolyamokként (üzenetek társul hello segítségével adatfolyam [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) üdvözlőüzenetére tulajdonság). Ebben a modellben minden csomópontja fel az alkalmazás hello sávszélességen adatfolyamot, mint megakadályozását toomessages. Adatfolyam fel a csomópont tooa kap, amikor hello csomópont ellenőrizheti hello állapotának hello adatfolyam alkalmazásállapot tranzakciók használatával.
* A megoldás szükséges tranzakciós viselkedését és atomicity küldésekor vagy fogadásakor több üzenetet az üzenetsorból.
* hello--élettartama (TTL) jellemző hello alkalmazásspecifikus munkaterhelés azért lépheti túl a hello 7 napon belül.
* Az alkalmazás-kezelési üzenetek azért lépheti túl a 64 KB-os kezeli, de valószínűleg nem megközelítést fog hello 256 KB-os korlátot.
* Ön foglalkozik a követelmény tooprovide a szerepköralapú hozzáférés-modell toohello várólisták, és különböző rights vagy a küldő és.
* A várólista-méret nem fog 80 GB-nál nagyobb.
* Azt szeretné, hogy toouse hello AMQP 1.0 alapuló üzenetküldési protokoll. Az AMQP kapcsolatos további információkért lásd: [Service Bus AMQP áttekintése](service-bus-amqp-overview.md).
* Egy esetleges áttelepítést a várólista-alapú pont-pont kommunikáció tooa üzenet exchange mintát, amely lehetővé teszi az egyes vagy összes független példányt kap, amelyek zökkenőmentes integrációt további fogadók (előfizető), akkor is envision üzenetek küldése toohello várólista. Ez utóbbi hello toohello közzétételi/előfizetési funkció alapértelmezés szerint a Service Bus által biztosított hivatkozik.
* Az üzenetküldési megoldás képes toosupport hello "legtöbb-visszaküldést" szállítási garancia nélkül hello kell a toobuild hello további összetevőket kell lennie.
* Kívánja toobe képes toopublish, majd az üzenetek kötegek felhasználását.

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Tárolási sorok és a Service Bus-üzenetsorok összehasonlítása
a következő részekben hello hello táblák adja meg a várólista szolgáltatások logikai csoportját, és hasonlítsa össze egy pillanat alatt hello lehetőségeinek tárolási sorok és a Service Bus-üzenetsorok lehetővé teszik.

## <a name="foundational-capabilities"></a>Eligazodást képességek
Ez a szakasz néhány alapvető üzenetsor-kezelési képességek hello tárolási sorok és a Service Bus-üzenetsorok által biztosított hasonlítja össze.

| Összehasonlítási feltétel | Tárolási sorok | Service Bus által kezelt üzenetsorok |
| --- | --- | --- |
| Rendezés növekvő |**Nem** <br/><br>További információkért lásd: hello első Megjegyzés: a "További információk" című hello.</br> |**Igen – első-First Out (FIFO)**<br/><br>(a üzenetkezelési munkamenetek hello használata) |
| Garantált kézbesítés |**A legalább egyszeri** |**A legalább egyszeri**<br/><br/>**A legtöbb-visszaküldést** |
| Támogatási atomi művelet |**Nem** |**Igen**<br/><br/> |
| Viselkedés fogadása |**A nem blokkoló**<br/><br/>(befejezése azonnal, ha nincs új üzenet megtalálható) |**Időtúllépés vagy anélkül blokkolása**<br/><br/>(kínál, hosszú lekérdezési vagy hello ["Üstökös technika"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**A nem blokkoló**<br/><br/>(hello keresztül használja a .NET által felügyelt API csak) |
| Leküldéses stílusú API |**Nem** |**Igen**<br/><br/>[OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) és **OnMessage** munkamenetek .NET API-t. |
| Fogadás módban |**Betekintés & bérleti** |**Betekintés & zárolása**<br/><br/>**Kap & törlése** |
| Kizárólagos hozzáférési mód |**Címbérlet-alapú** |**Zárolási-alapú** |
| Bérleti/zárolást időtartama |**30 másodperc (alapértelmezett)**<br/><br/>**7 nap (maximum)** (újítsa meg, vagy egy üzenet bérleti hello használata kiadás [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API.) |**60 másodperc (alapértelmezett)**<br/><br/>Egy üzenet zárolási hello segítségével megújítható [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API. |
| Bérleti/zárolást pontosság |**Üzenet szint**<br/><br/>(minden üzenetet is rendelkezik különböző időtúllépési érték, amely frissítheti szükség szerint hello segítségével hello üzenet feldolgozásakor [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API) |**Várólista szintjét**<br/><br/>(minden sor egy zárolás alkalmazott pontosság tooall az üzenetek, de a hello segítségével hello zárolási megújítható [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API.) |
| Kötegelni fogadása |**Igen**<br/><br/>(explicit módon megadni üzenetek száma történő mentése tooa legfeljebb 32 üzenetek beolvasásra) |**Igen**<br/><br/>(implicit előzetes betöltési tulajdonság engedélyezése vagy explicit módon keresztül hello tranzakciók használata) |
| A kötegelt küldése |**Nem** |**Igen**<br/><br/>(a tranzakciók vagy ügyféloldali kötegelés hello használata) |

### <a name="additional-information"></a>További információ
* Tárolási sorok üzenetek általában első-az első-kimenő, de időnként el nem megfelelő sorrendben; például amikor egy állapotüzenet látható időkorlát tartama érvényessége lejár (például miatt a feldolgozás során összeomló ügyfélalkalmazás). Hello láthatósági időkorlátot lejártakor hello üzenet válik láthatóvá, egy másik munkavégző toodequeue hello várólistája meg újra azt. Ezen a ponton újonnan látható üdvözlőüzenetére előfordulhat, hogy helyezhető hello sorban (toobe várólistából kivéve újra) után egy üzenetet, amely eredetileg a várólistában levő után azt.
* hello garantált FIFO minta a Service Bus-üzenetsorok üzenetkezelési munkamenetek hello használatát igényli. A hello eseményt, amely az alkalmazás hello hello érkezett üzenet feldolgozásakor a program összeomlik **Belepillantás & zárolása** módot, amikor legközelebb hello egy várólista címzett fogad el egy üzenetkezelési munkamenet, után hello hibás üzenettel fog elindulni a -élettartama (TTL) időszakának lejártáig.
* Tárolási sorok tervezett toosupport szabványos üzenetsor-kezelési forgatókönyvek, például alkalmazás összetevők tooincrease méretezhetőséget és a hibák tolerancia leválasztásával, simítás, és a feldolgozási munkafolyamatok kialakítását.
* Service Bus-üzenetsorok támogatja hello *: legalább egyszeri* garantált kézbesítés. Ezenkívül hello *legtöbb-visszaküldést* szemantikai támogatja-e a munkamenet állapota toostore hello alkalmazásállapot használatával és a tranzakciók tooatomically üzeneteket fogadni, és hello munkamenet-állapot frissítése.
* Tárolási sorok biztosít egy egységes és egységes programozási modell várólisták, a táblák és a Blobok – a fejlesztők számára, és műveletek csoportjai.
* Service Bus-üzenetsorok támogatást nyújt a helyi tranzakciók hello környezetben egy adott sorba.
* Hello **fogadásához és törléséhez** Service Bus által támogatott mód hello képességét tooreduce hello üzenetküldési műveletek száma (és a kapcsolódó költségeket) biztosít süllyesztett kézbesítési megbízhatósági cserébe.
* Tárolási sorok hello képességét tooextend hello címbérleteket bérletét üzenetek adja meg. Ez lehetővé teszi hello munkavállalók toomaintain rövid címbérlet üzeneteket. Így ha egy munkavégző összeomlik, üdvözlőüzenetére gyorsan feldolgozható újra egy másik feldolgozónak. Emellett egy munkavégző kiterjesztheti hello bérleti üzenetben, ha a Tovább, mint az aktuális hello címbérlet ideje tooprocess.
* Tárolás az üzenetsorok elsőnek a láthatósági időkorlátot használható hello sorra helyezése után állítsa be, vagy egy üzenet üzenetmozgatót. Emellett különböző bérleti értékekkel futásidőben frissítheti az üzenetet, és azonos hello üzenetére értékei különböző frissítési várólista. A Service Bus zárolási időtúllépések definiált hello várólista metaadatok; azonban megújíthatják hello zárolási által hívó hello [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) metódust.
* hello maximális időkorlátot egy blokkolja a fogadási művelethez a Service Bus-üzenetsorok nem 24 nap. REST-alapú időtúllépések van azonban, 55 másodpercben maximális értéket.
* Ügyféloldali kötegelés által biztosított Service Bus lehetővé teszi, hogy a várólista ügyfél toobatch több üzenetet az egyetlen küldési művelet. Kötegelés érhető el csak aszinkron műveletek.
* Szolgáltatások, mint a tároló (ha virtualizálhatja a fiókok további) és korlátlan várólisták hello 200 TB-os felső teszik ideális platform a Szolgáltatottszoftver-szolgáltatók.
* Tárolási sorok adjon meg egy rugalmas és performant delegált hozzáférés-vezérlési mechanizmus.

## <a name="advanced-capabilities"></a>Speciális képességek
Ez a szakasz a tárolási sorok és a Service Bus-üzenetsorok által biztosított speciális képességek hasonlítja össze.

| Összehasonlítási feltétel | Tárolási sorok | Service Bus által kezelt üzenetsorok |
| --- | --- | --- |
| Ütemezett kézbesítését |**Igen** |**Igen** |
| Automatikus halott levelek kezelése |**Nem** |**Igen** |
| Várólista élő idő érték növelése |**Igen**<br/><br/>(keresztül láthatósági időkorlátot helybeni frissítése) |**Igen**<br/><br/>(egy dedikált API-függvénye keresztül megadott) |
| Elhalt üzenet támogatása |**Igen** |**Igen** |
| Frissítés helyben |**Igen** |**Igen** |
| Kiszolgálóoldali tranzakció napló |**Igen** |**Nem** |
| Storage mérőszámainak |**Igen**<br/><br/>**Metrikák MINUTE**: biztosít a valós idejű metrikák rendelkezésre állás érdekében TP-k, az API-hoz a számát, a hiba száma és további, az összes valós idejű (percenként összesítve, és az éles környezetben csak történtekről néhány percen belül jelentett a hívás. További információkért lásd: [kapcsolatos Storage Analytics Metrics](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Igen**<br/><br/>(tömeges lekérdezések meghívásával [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Felügyeleti állapot |**Nem** |**Igen**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus.active), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.disabled), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.senddisabled), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.receivedisabled) |
| Automatikus-üzenettovábbítással |**Nem** |**Igen** |
| Várólista függvény kiürítése |**Igen** |**Nem** |
| Üzenet csoportok |**Nem** |**Igen**<br/><br/>(a üzenetkezelési munkamenetek hello használata) |
| Alkalmazásállapot üzenet csoportonként |**Nem** |**Igen** |
| Kettős észlelés |**Nem** |**Igen**<br/><br/>(a küldő oldalon hello konfigurálható) |
| Üzenet csoportok tallózása |**Nem** |**Igen** |
| Üzenet munkamenet-azonosító szerint beolvasása |**Nem** |**Igen** |

### <a name="additional-information"></a>További információ
* Mindkét üzenetsor-kezelési technológiák segítségével egy üzenet toobe későbbi időpontra ütemezve.
* Várólista automatikus-továbbítási lehetővé teszi a várólisták tooauto-továbbító az üzenetek tooa egyetlen várólista, mely hello fogadó alkalmazás feldolgozó üdvözlőüzenetére ezer. A mechanizmus tooachieve biztonsági folyamatábrán és elkülönítheti tárolási mindegyik üzenet közzétevő között is használhatja.
* Tárolási sorok támogatásához frissítése message tartalmat. Is használhatja ezt a funkciót tárolásakor állapotadatokat és végrehajtási növekményes frissítéseket üdvözlőüzenetére, hogy hello legutóbbi ellenőrzőponttól, nem nulláról tudja feldolgozni. Service Bus-üzenetsorok, ahol engedélyezheti hello azonos forgatókönyv üzenet munkamenetek hello használata. Munkamenetek lehetővé teszik a toosave és hello feldolgozási állapotának beolvasása (használatával [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) és [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Dead megnevezéséhez](service-bus-dead-letter-queues.md), amely csak a Service Bus-üzenetsorok, által támogatott hasznosak lehetnek a hello fogadó alkalmazás nem dolgozható fel sikeresen üzenetek elkülönítése, vagy amikor az üzenetek nem érhető el a rendeltetési miatt tooan lejárt -élettartama (TTL) tulajdonsága. hello élettartam értéke határozza meg, mennyi ideig üzenet marad hello várólistában. A Service busszal üdvözlőüzenetére lesz áthelyezett tooa különleges várólista $DeadLetterQueue meghívva, amikor hello TTL időszakának lejártáig.
* "poison" üzenetek toofind tárüzenetsort, amikor egy üzenet hello alkalmazás üzenetmozgatót megvizsgálja hello  **[DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx)**  üdvözlőüzenetére tulajdonsága. Ha **DequeueCount** egy adott küszöbértéknél nagyobb hello alkalmazás helyezi át az alkalmazás által meghatározott "halott levél" tooan hello üzenetsorból.
* Tárolási sorok összes hello tranzakciók részletes naplója végre hello várólista ellen, valamint metrikák összesítése tooobtain engedélyezése. Mindkét lehetőség Hibakeresés és az ismertetése, hogy az alkalmazás által tárüzenetsort hasznosak. Is hasznosak az alkalmazás teljesítményének hangolása és-üzenetsorok használatával hello költségeinek csökkentése.
* a Service Bus által támogatott "üzenet munkamenetek" Hello koncepció lehetővé teszi a tooa tartozó egyes a megadott fogadó, ilyenkor pedig létrejön az üzenetek és a megfelelő fogadók között egy munkamenet-szerű affinitás társított logikai csoport toobe. Engedélyezheti a speciális funkció a Service Bus által hello beállítása [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) üzenetben tulajdonság. Fogadók figyelni az egy adott munkamenet-Azonosítót, majd, hogy a megosztás hello megadott munkamenet-azonosítót.
* hello ismétlődést észlelési funkció Service Bus-üzenetsorok által támogatott automatikusan eltávolítja az ismétlődő küldött üzenetek tooa üzenetsor vagy témakör, hello hello értéke alapján [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) tulajdonság.

## <a name="capacity-and-quotas"></a>Kapacitás és a kvóták
Ez a szakasz a tárolási sorok és a Service Bus-üzenetsorok hello szempontjából összehasonlítja [kapacitás és a kvóták](service-bus-quotas.md) , amely előfordulhat, hogy érvényes.

| Összehasonlítási feltétel | Tárolási sorok | Service Bus által kezelt üzenetsorok |
| --- | --- | --- |
| A várólista maximális hossza |**500 TB**<br/><br/>(korlátozott tooa [tárfiókok kapacitásával egyetlen](../storage/common/storage-introduction.md#queue-storage)) |**1 GB too80 GB**<br/><br/>(a várólista létrehozása után definiált és [particionálás engedélyezése](service-bus-partitioning.md) – lásd: hello "További információk" című) |
| Maximális méret |**64 KB**<br/><br/>(48 KB használatakor **Base64** kódolás)<br/><br/>Azure üzenetsorokat és blobokat – ekkor is be egyetlen cikkre too200GB sorba helyezni a kombinálásával nagy üzeneteket is támogatja. |**256 KB-os** vagy **1 MB**<br/><br/>(beleértve a fejléc és a szövegtörzset, maximális fejléc mérete: 64 KB).<br/><br/>Hello függ [szolgáltatásréteg](service-bus-premium-messaging.md). |
| Maximális üzenet TTL tulajdonsága |**7 nap** |**`TimeSpan.Max`** |
| Sorok maximális száma |**Korlátlan** |**10,000**<br/><br/>(egyes szolgáltatásnévtér, növelhető) |
| Egyidejű ügyfelek maximális száma |**Korlátlan** |**Korlátlan**<br/><br/>(csak érvényes tooTCP protokoll-alapú kommunikációt 100 egyidejű kapcsolat korlátai) |

### <a name="additional-information"></a>További információ
* A Service Bus várólista méretkorlátait érvénybe lépteti. hello várólista maximális hossza a hello várólista létrehozása után van megadva, és értéke 1 és 80 GB közötti lehet. A hello várólista létrehozásakor beállított hello várólista mérete érték elérésekor, további bejövő üzenetek vissza kell utasítani, és kivétel történt a kód hello fogadja. A Service Bus kvóták kapcsolatos további információkért lásd: [Service Bus kvóták](service-bus-quotas.md).
* A hello [Standard csomagra](service-bus-premium-messaging.md), 1, 2, 3, 4 vagy 5 GB méretű (hello alapértelmezett érték 1 GB-os) a Service Bus-üzenetsorok is létrehozhat. Hello prémium csomagban várólisták fel too80 GB-nál is létrehozhat. A Standard szint, a particionálás engedélyezve (ez utóbbi érték hello alapértelmezett), a Service Bus 16 partíciók hoz létre minden egyes megadott GB. Így, ha létrehoz egy sort, amely 5 GB-nál, 16 partíciókkal rendelkező hello várólista maximális hossza válik (5 * 16) = 80 GB. Megjelenik a particionált üzenetsor vagy témakör maximális méretén hello megtekintésével, hogy a belépési hello [Azure-portálon][Azure portal]. Hello prémium csomagban sor száma csak 2 partíció jönnek létre.
* A tárolási sorok, ha hello hello üzenet tartalma nem XML-biztonságos, akkor kell legyen **Base64** kódolású. Ha Ön **Base64**-üdvözlőüzenetére kódolására, hello felhasználói hasznos mentése too48 KB helyett 64 KB lehet.
* A Service Bus-üzenetsorok, az egyes üzeneteket tárolja a sorhoz két részből áll: egy fejléc és a szervezet. hello üdvözlőüzenetére teljes mérete nem haladhatja meg a hello szolgáltatási réteg által támogatott maximális üdvözlőüzenetére.
* Ügyfelek hello TCP protokollra keresztül kommunikál a Service Bus-üzenetsorok, hello maximális száma párhuzamos kapcsolatok tooa egy Service Bus-üzenetsorba esetén korlátozott too100. Ez a szám megosztott küldők és a fogadók között. Ha ez a kvóta elérése után további kapcsolatokhoz kérelmeknél vissza lesznek utasítva, és kivétel történt a kód hello fogadja. Ez a korlátozás nem írják elő toohello várólisták REST-alapú API használatával csatlakozó ügyfelek.
* Ha több mint 10 000 várólistákból egy Service Bus-névtér van szüksége, hello Azure támogatási csapata kapcsolatba, és korlátozás megnövelésére. a Service busszal 10 000 várólisták túl tooscale, hello segítségével további névteret is létrehozhat [Azure-portálon][Azure portal].

## <a name="management-and-operations"></a>Felügyelete és műveletei
Ez a szakasz hello felügyeleti szolgáltatásait tárolási sorok és a Service Bus-üzenetsorok hasonlítja össze.

| Összehasonlítási feltétel | Tárolási sorok | Service Bus üzenetsorok |
| --- | --- | --- |
| Felügyeleti protokoll |**REST-HTTP/HTTPS-KAPCSOLATON keresztül** |**REST-HTTPS-KAPCSOLATON keresztül** |
| Futásidejű protokoll |**REST-HTTP/HTTPS-KAPCSOLATON keresztül** |**REST-HTTPS-KAPCSOLATON keresztül**<br/><br/>**Az AMQP 1.0-s szabvány (TCP with TLS)** |
| .NET API |**Igen**<br/><br/>(.NET tárolási ügyfél API-ja) |**Igen**<br/><br/>(.NET a Service Bus API) |
| Natív C++ |**Igen** |**Igen** |
| Java API |**Igen** |**Igen** |
| A PHP API |**Igen** |**Igen** |
| NODE.js API |**Igen** |**Igen** |
| Tetszőleges metaadatok támogatása |**Igen** |**Nem** |
| Várólista elnevezési szabályok |**Másolatot too63 karakter**<br/><br/>(A várólistacímke betűjét kisbetűnek kell lennie.) |**Másolatot too260 karakter**<br/><br/>(Várólista elérési útja és neve nem különböztetik meg.) |
| Várólista hossza függvény beolvasása |**Igen**<br/><br/>(Hozzávetőleges érték, ha üzenetek elévülési hello Élettartamon túl nélkül törlés alatt áll.) |**Igen**<br/><br/>(Pontos, időpontban érték.) |
| Betekintés a függvényt |**Igen** |**Igen** |

### <a name="additional-information"></a>További információ
* Tárolási sorok támogatást biztosít, amely alkalmazott toohello várólista leírása, a név/érték párok hello formája tetszőleges attribútumokkal.
* Mindkét várólista technológiái nyújtanak hello képességét toopeek üzenet anélkül, hogy toolock informatikai, amelyek hasznosak lehetnek a várólista Explorerre/eszköz végrehajtása során.
* hello .NET a Service Bus közvetítőalapú üzenettovábbítás API-k előnyeinek teljes kétirányú TCP-kapcsolatok a jobb teljesítmény amikor tooREST képest HTTP Protokollon keresztül, és hello AMQP 1.0 szabványos protokollt támogatják.
* Tárolási sorok nevének 3 – 63 karakter hosszú lehet, kisbetűket, számokat és kötőjeleket tartalmazhat. További információkért lásd: [elnevezési üzenetsorok és metaadatok](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* A Service Bus Várólistanevek mentése too260 karakter hosszú lehet, és kevésbé korlátozó elnevezési szabályokat. A Service Bus Várólistanevek betűket, számokat, pontokat, kötőjeleket és aláhúzásjeleket tartalmazhat.

## <a name="authentication-and-authorization"></a>Hitelesítés és engedélyezés
Ez a szakasz ismerteti tárolási sorok és a Service Bus-üzenetsorok által támogatott hello hitelesítési és engedélyezési szolgáltatásokat.

| Összehasonlítási feltétel | Tárolási sorok | Service Bus által kezelt üzenetsorok |
| --- | --- | --- |
| Authentication |**Szimmetrikus kulcs** |**Szimmetrikus kulcs** |
| Biztonsági modell |Delegált hozzáférést SAS-tokenje keresztül. |SAS |
| Identitás-összevonási szolgáltató |**Nem** |**Igen** |

### <a name="additional-information"></a>További információ
* Minden kérelem tooeither a technológiák queuing hello hitelesíteni kell. A névtelen hozzáférés a nyilvános várólisták nem támogatottak. Használatával [SAS](service-bus-sas.md), azzal, hogy közzétesz egy csak írható SAS, csak olvasható SAS vagy akár egy teljes hozzáférési SAS ebben a forgatókönyvben meg lehet oldani.
* hello hitelesítési séma megadott tároló várólisták helyezési hello szimmetrikus kulcs, amely a kivonat-alapú üzenethitelesítési kód (HMAC), számított hello SHA-256 algoritmussal és kódolása egy **Base64** karakterlánc. Hello megfelelő protokollal kapcsolatos további információkért lásd: [hello Azure Storage szolgáltatásainak hitelesítése](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Service Bus-üzenetsorok egy szimmetrikus kulcsok használata hasonló modellt támogatja. További információkért lásd: [megosztott hozzáférési aláírást hitelesítést a Service Bus](service-bus-sas.md).

## <a name="conclusion"></a>Összegzés
Által esetlegesen hello két technológiák bemutatják, lesz a lehet egy több tájékozott döntést, amelyek a feldolgozási sor technológia toouse képes toomake, és mikor. Ha toouse tárüzenetsort vagy a Service Bus várólisták egyértelmű döntést hello számos tényezőtől függ. Ezek a tényezők függ, hogy fokozottan hello egyéni igényekhez, az alkalmazás és az architektúra. Ha az alkalmazás már a Microsoft Azure hello legfontosabb funkcióit, célszerű toochoose tárüzenetsort, különösen akkor, ha az alapvető kommunikáció és az üzenetkezelési szolgáltatások vagy 80 GB-nál nagyobb lehet szükség várólisták között van szüksége.

Service Bus-üzenetsorok, adjon meg egy számot a speciális szolgáltatások, például munkamenetek, tranzakciók, mert ismétlődő automatikus észlelési kézbesítetlen levelek kezelése és a tartós közzétételi/előfizetési képességeket, akkor lehet, hogy egy előnyben részesített választott hibrid készítésekor alkalmazás, vagy ha az alkalmazás egyéb szükséges ezeket a szolgáltatásokat.

## <a name="next-steps"></a>Következő lépések
a következő cikkek hello további útmutatást és tárüzenetsort vagy a Service Bus-üzenetsorok használatával információkat biztosítanak.

* [Bevezetés a Service Bus által kezelt üzenetsorok használatába](service-bus-dotnet-get-started-with-queues.md)
* [Hogyan tooUse hello Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Gyakorlati tanácsok a teljesítménnyel kapcsolatos fejlesztések használatával a Service Bus közvetítőalapú üzenettovábbítás](service-bus-performance-improvements.md)
* [Introducing üzenetsorok és témakörök az Azure Service Bus (blogbejegyzés)](http://www.code-magazine.com/article.aspx?quickid=1112041)
* [Fejlesztői útmutató tooService Bus hello](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Hello Üzenetsor-kezelés szolgáltatás használata az Azure-ban](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

