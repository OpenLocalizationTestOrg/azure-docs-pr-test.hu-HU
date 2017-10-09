---
title: "az Event Hubs aaaAzure szolgáltatások áttekintése |} Microsoft Docs"
description: "Áttekintés és az Azure Event Hubs szolgáltatások részleteit"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 8094e48abc8455ed725d4d5d3f9895f431441e2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-features-overview"></a>Event Hubs szolgáltatások – áttekintés

Az Azure Event Hubs egy méretezhető Eseményfeldolgozási szolgáltatás, amely ingests és dolgozza fel az eseményeket és adatok, nagy mennyiségű, alacsony késéssel és nagy megbízhatósággal. Lásd: [Mi az az Event Hubs?](event-hubs-what-is-event-hubs.md) hello szolgáltatás magas szintű áttekintését.

Ez a cikk épít, hello hello információk [áttekintése](event-hubs-what-is-event-hubs.md), és műszaki és megvalósítási részleteit az Event Hubs-összetevők és a szolgáltatások.

## <a name="event-publishers"></a>Esemény-közzétevők

Minden entitás, amely elküldi az adatokat tooan eseményközpont egy esemény termelő vagy *esemény-közzétevő*. Az esemény-közzétevők a HTTPS vagy az AMQP 1.0 használatával tehetik közzé az eseményeket. Esemény-közzétevők egy közös hozzáférésű Jogosultságkód (SAS) token tooidentify maguk tooan eseményközpontjának használata, és elvégezheti rendelkezhetnek egyedi azonosítóval, vagy közös SAS-tokennel.

### <a name="publishing-an-event"></a>Esemény közzététele

Az eseményeket az AMQP 1.0 vagy HTTPS használatával teheti közzé. Az Event Hubs biztosít [klienskódtárak és -osztályok](event-hubs-dotnet-framework-api-overview.md) közzétételéhez tooan eseményközpont események a .NET-ügyfelekről. Egyéb futtatókörnyezetek és platformok esetén használhatja bármelyik AMQP 1.0-ügyfelet, ilyen például az [Apache Qpid](http://qpid.apache.org/). Az eseményeket közzéteheti egyenként vagy kötegelve is. Az egyes közzétételekre (eseményadat-példány) 256 KB-os korlát érvényes, függetlenül attól, hogy önálló vagy kötegelt közzétételről van-e szó. Nagyobb, mint a küszöbérték eredmények események közzététele hibát eredményez. Ajánlott eljárás a közzétevők toobe tudjanak a partíciókról az eseményközpontban hello és tooonly adjon meg egy *partíciókulcs* (bevezetett hello a következő szakaszban), vagy az azonosságukat SAS-token használatával.

hello választott toouse AMQP vagy HTTPS adott toohello használat forgatókönyvének. AMQP használatához ki kell alakítani egy állandó kétirányú szoftvercsatornát hello hozzáadása tootransport szintű security (TLS) vagy SSL/TLS. AMQP rendelkezik magasabb hálózati költségek hello munkamenet inicializálásakor azonban HTTPS további SSL megkövetelése terhet minden kérelem esetén. Gyakori közzététel esetén az AMQP nagyobb teljesítményt biztosít.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Az Event Hubs biztosítja, hogy a partíciós kulcs értéke események kézbesíti a rendszer sorrendje, illetve toohello azonos partíció. Ha partíciókulcsok használata közzétevői házirendekkel, majd hello hello közzétevő identitásának és hello hello partíciókulcs értékének egyeznie kell. Különben hiba történik.

### <a name="publisher-policy"></a>Közzétevői házirend

Az Event Hubs lehetővé teszi az esemény-közzétevők részletes szabályozását a *közzétevői házirendek* révén. Közzétevői házirendek tervezett futásidő szolgáltatások toofacilitate nagyszámú független esemény-közzétevők. A közzétevői házirendek mindegyik közzétevő a saját egyedi azonosítót használ események tooan eseményközpont közzétételekor hello mechanizmus a következő használatával:

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Nem kell előre toocreate olyan gyártó nevét, azoknak azonban egyezniük kell hello rendelés tooensure független közzétevő-azonosságok az esemény közzétételekor használt SAS-jogkivonatot. Közzétevői házirendek használatakor hello **PartitionKey** értéke toohello közzétevő neve. toowork megfelelően, ezeket az értékeket meg kell egyeznie.

## <a name="capture"></a>Rögzítés

[Event Hubs rögzítése](event-hubs-capture-overview.md) tooautomatically rögzítési hello adatfolyam-adatokat az Event Hubs lehetővé teszi, és mentse a Blob storage-fiók, vagy egy Azure Data Lake szolgáltatásfiók tooyour választott. Engedélyezhetik a rögzítési hello Azure-portálon, és adja meg a minimális méret és idő ablak tooperform hello rögzítése. Használja az Event Hubs rögzítéséhez, megadhatja a saját Azure Blob Storage-fiók és a tároló, vagy az Azure Data Lake szolgáltatás fiókhoz használt toostore hello rögzített adatok. A rögzített adatok hello Apache Avro formátum nyelven van megírva.

## <a name="partitions"></a>Partíciók

Az Event Hubs egy particionált felhasználói mintát, amelyben mindegyik felhasználó csak olvassa be egy adott részhalmazát, vagyis partícióját hello üzenetstream keresztül üzenetstreamelést biztosít. Ez a minta biztosítja a horizontális skálázhatóságot az eseményfeldolgozáshoz, és egyéb, streamközpontú szolgáltatásokat is nyújt, amelyek az üzenetsorokban vagy témakörökben nem érhetők el.

A partíció események egy rendezett sorozata az eseményközpontban. Ha új esemény érkezik, az a sorozat végére toohello hozzáadásuk után. A partíció elképzelhető egy „véglegesítési naplóként”.

![Event Hubs](./media/event-hubs-features/partition.png)

Az Event Hubs adatok őrzi meg a konfigurált megőrzési időtartamig a hello eseményközpont összes partíciójára érvényes. Az események időalapon évülnek el – nem törölhetők külön. Mivel a partíciók függetlenek egymástól, és saját adatsorozataikat tartalmazzák, gyakran különböző ütemben nőnek.

![Event Hubs](./media/event-hubs-features/multiple_partitions.png)

a partíciók számának hello a létrehozásakor van megadva, és 2 és 32 között kell lennie. hello partíciók száma érték nem módosítható, ezért érdemes a hosszú távú skálázási partíciószám beállításakor. Partíció egy adatrendezési mechanizmus, amely a felhasználó alkalmazásokban szükséges alárendeltségi párhuzamosság toohello vonatkozik található. hello eseményközpontban partíciók száma közvetlenül kapcsolódik toohello számát az egyidejű olvasók várt toohave. Lépjen kapcsolatba az Event Hubs team hello túl 32 partíciók hello száma növelhető.

Partíciók azonosíthatóak, és elküldheti toodirectly, küldése közvetlenül tooa partíció nem ajánlott. Ehelyett használhatja a hello bemutatott magasabb szintű szerkezeteket [esemény-közzétevő](#event-publishers) és [kapacitás](#capacity) szakaszok. 

Partíciók hello hello esemény törzsét, a felhasználó által definiált tulajdonságcsomagot és metaadatokat például az eltolást hello partíció vagy a hello streamsorozatban számát tartalmazó eseményadatok sorozatát kitöltődnek.

Partíciók és rendelkezésre állásának és megbízhatóságának közötti hello kompromisszum kapcsolatos további információkért lásd: hello [Event Hubs programozási útmutató](event-hubs-programming-guide.md#partition-key) és hello [rendelkezésre állását és az Event Hubs következetes](event-hubs-availability-and-consistency.md) cikk .

### <a name="partition-key"></a>Partíciókulcs

Használhatja a [partíciókulcs](event-hubs-programming-guide.md#partition-key) toomap beérkező eseményadatok hello céllal történik, a szervezeti adatok adott partíciókra. hello partíciókulcs az eseményközpontnak átadott küldő által megadott értéket. A feldolgozása egy statikus kivonatoló függvénnyel történik, amely hello partíció-hozzárendelést. Ha nem ad meg partíciókulcsot az események közzétételekor, a rendszer ciklikus időszeleteléses hozzárendelést használ.

hello esemény-közzétevő csak a partíciós kulcs tudatában a hello partíció toowhich hello esemény közzé lesz téve. A kulcs és a partíció leválasztásával insulates hello küldőnek nem szükséges behatóan kapcsolatos hello alárendelt feldolgozási tooknow. Egy eszköz vagy a felhasználói identitás remek partíciókulcs lehetővé teszi, de más tulajdonságok, például a földrajzi is használt toogroup egyedi kapcsolódó események egyetlen partícióra.

## <a name="sas-tokens"></a>SAS-tokenek

Az Event Hubs használ *megosztott hozzáférési aláírásokkal*, amelyek elérhetők hello névtér és szinten event hub. A SAS-tokent egy SAS-kulcsból hozza létre a rendszer, és egy URL SHA-kivonata egy meghatározott formátumban kódolva. Hello kulcs (házirend) és hello token hello nevét, az Event Hubs segítségével generálja újra hello kivonatoló és számukra a hitelesítéshez hello küldő. Az esemény-közzétevők SAS-tokenje általában egy adott eseményközpontban, csak **küldési** jogosultságokkal hozható létre. A SAS-tokenes URL-mechanizmus hello közzétevői házirendben bevezetett publisher azonosításának hello alapját képezi. További információ a SAS használatával kapcsolatban: [Shared Access Signature Authentication with Service Bus](../service-bus-messaging/service-bus-sas.md) (Közös hozzáférésű jogosultságkóddal való hitelesítés a Service Bus használatával).

## <a name="event-consumers"></a>Eseményfelhasználók

Minden entitás, amely eseményadatokat olvas egy eseményközpontból, *eseményfelhasználó*. Minden Event Hubs-felhasználó hello AMQP 1.0-munkameneten keresztül csatlakozni, és az események azonnal hello munkameneten keresztül elérhetővé válnak. hello ügyfélnek nincs szüksége toopoll az adatok rendelkezésre állását.

### <a name="consumer-groups"></a>Felhasználói csoportok

az Event hubs hello közzétételi/előfizetési mechanizmus keresztül engedélyezhető, *fogyasztói csoportok*. A felhasználói csoport a teljes eseményközpont egyik nézete (állapot, pozíció vagy eltolás). Fogyasztói csoportok lehetővé teszik több felhasználó alkalmazás tooeach hello eseményfelhasználó, és egymástól függetlenül saját tempójában tooread hello adatfolyam külön nézetével rendelkezik, és a saját tolódnak.

Architektúrákban Stream mindegyik alárendelt alkalmazás megfelel a tooa fogyasztói csoportot. Ha azt szeretné, hogy toowrite esemény toolong távú tárolására, adott tárhelyírási alkalmazás is egy felhasználói csoport. Az összetett eseményfeldolgozást már egy másik, külön felhasználói csoport végzi. A partíciókat csak a felhasználói csoportokon keresztül érheti el. Lehet, legfeljebb 5 egyidejű olvasók egy partíciót engedélyez fogyasztói csoportot; azonban **javasoljuk, hogy nincs-e csak egy aktív fogadó egy fogyasztói csoporton partíciók**. Nincs mindig egy alapértelmezett felhasználói csoport eseményközpontban, és mentése egy Standard csomagra eseményközpont too20 felhasználói csoportot is létrehozhat.

hello következő példákban hello felhasználói csoportok URI-szabályaira:

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

hello következő ábrán látható hello Event Hubs adatfolyam feldolgozási architektúráját:

![Event Hubs](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Streameltolások

Egy *eltolás* hello pozíciója a partíción belüli esemény van. Az eltolásokat tekintheti ügyféloldali kurzorként. hello eltolás egy olyan bájtot számozására vonatkozó hello esemény. Az eltolás egy esemény fogyasztói (olvasó) toospecify hello eseményfelhasználó, amelyből a események olvasását toobegin kívánja az ponttá lehetővé teszi. Hello eltolás megadható időbélyegzőként vagy eltolásértékként. Felhasználók felelőssége saját eltolásértékeik kívül hello Event Hubs szolgáltatás tárolásához. A partíciókon belül mindegyik esemény rendelkezik eltolással.

![Event Hubs](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Ellenőrzőpontok használata

Az *ellenőrzőpontok használatával* az olvasók megjelölhetik vagy véglegesíthetik pozíciójukat a partíciók eseménysorozatában. Ellenőrzőpontok hello fogyasztó hello feladata, és egy fogyasztói csoporton belül partíciónkénti alapon történik. Ezt a feladatot azt jelenti, hogy mindegyik felhasználói csoport minden partíció olvasó kell nyomon követjük, hogy a jelenlegi állapotában a hello eseményfelhasználó, hello szolgáltatás értesítheti, ha szükségesnek hello adatfolyam befejezése.

Ha egy olvasó lecsatlakozik egy partícióról, az újracsatlakozáskor kezdi az olvasást hello utolsó olvasója-az adott partíció az adott felhasználói csoportban által elküldött ellenőrzőpontnál hello ellenőrzőpont. Amikor hello olvasó csatlakozik, átadja a eltolási toohello események hub toospecify hello helye a mely toostart olvasása. Ezzel a módszerrel ellenőrzőpontok tooboth megjelölhetik az eseményeket, "complete" alsóbb rétegbeli alkalmazásoknak is használhatja, és akkor fordul elő, ha a különböző gépeken futó olvasók közötti feladatátvétel tooprovide rugalmasság. Az ellenőrzőpontok használata során egy alacsonyabb értékű eltolás megadásával is lehetséges tooreturn tooolder adatokat. Ezzel a mechanizmussal az ellenőrzőpontok használata rugalmasságot biztosít feladatátvétel esetén, és lehetővé teszi az eseménystream visszajátszását.

### <a name="common-consumer-tasks"></a>Általános felhasználói feladatok

Minden Event Hubs-felhasználó keresztül egy AMQP 1.0-munkamenet, egy állapot-kompatibilis kétirányú kommunikációs csatorna csatlakozzon. Mindegyik partíció rendelkezik, amely elősegíti a partíció szerint elkülönített események hello átviteli AMQP 1.0-munkamenet.

#### <a name="connect-tooa-partition"></a>Csatlakozás tooa partíció

Toopartitions összekötésével esetén bérleti általános gyakorlat toouse mechanizmus toocoordinate olvasó kapcsolatok toospecific partíciókat. Így lehetőség a felhasználói csoport toohave csak egyetlen aktív olvasóval minden partíció esetében. Ellenőrzőpontok, bérbeadás és kezelje az olvasók egyszerűsítettek hello segítségével [EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) osztály a .NET-ügyfelek. Event Processor Host hello egy intelligens Felhasználóügynök.

#### <a name="read-events"></a>Események olvasása

Miután egy AMQP 1.0-munkamenet és kapcsolat egy adott partícióra van megnyitva, a események toohello AMQP 1.0-ügyfél hello Event Hubs szolgáltatás érkeznek. Ez a kézbesítési mechanizmus nagyobb átviteli teljesítményt és kisebb késést biztosít a lekérésalapú mechanizmusoknál, amilyen például a HTTP GET. Események lettek elküldve toohello ügyfél, akkor minden eseményadat-példány például hello eltolást és sorszámot, amelyek hello eseménysorozatában használt toofacilitate ellenőrzőpontokkal fontos metaadatokat tartalmaz.

Eseményadatok:
* Eltolás
* Sorozat száma
* Törzs
* Felhasználói tulajdonságok
* Rendszertulajdonságok

A felelősség toomanage hello eltolás.

## <a name="capacity"></a>Kapacitás

Az Event Hubs egy kiválóan méretezhető párhuzamos architektúra rendelkezik, és számos kulcsfontosságú szerepet játszik tooconsider méretezése és a méretezés.

### <a name="throughput-units"></a>Átviteli egységek

az Event Hubs átviteli kapacitásának hello vezérli *átviteli egységek*. Az átviteli egységek előre megvásárolt kapacitásegységek. Egy átviteli egység tartalmazza a következő kapacitás hello:

* Bemenő forgalom: mentése too1 MB második vagy 1000 esemény / másodperc (amelyik előbb eléri)
* Kimenő forgalom: mentése too2 MB / s

Befelé meghaladja a megvásárolt átviteli egységek hello hello kapacitását, és egy [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) adja vissza. Kimenő forgalom nem eredményez szabályozási kivételeket, de továbbra is korlátozott toohello kapacitásának megvásárolt átviteli egységek hello. Ha közzétételi sebességhez kapcsolódó kivételeket kap, vagy toosee nagyobb kimenő forgalomra számított, lehet, hogy toocheck hány átviteli egységet vásárolt ahhoz hello névtér. Kezelheti a hello átviteli egységeket **méretezési** panelen található hello hello névterek [Azure-portálon](https://portal.azure.com). Átviteli egységek hello programozott módon is kezelhetők [Event Hubs API-k](event-hubs-api-overview.md).

Az átviteli egységek óraalapú díjszabással rendelkeznek, és előre kell megvásárolni őket. Miután megvásárolta, az átviteli egységek után legalább egy órányi díjat ki kell fizetni. Too20 átviteli egységek az Event Hubs névtér megvásárolható és hello névtér összes Eseményközpontjában érvényesülnek.

A megvásárolt átviteli egységeket blokkokban 20-környezet too100 átviteli egység, lépjen kapcsolatba az Azure-támogatás. Efölött 100-as csomagokban vásárolhat átviteli egységeket.

Azt javasoljuk, hogy a átviteli egységek és partíciók tooachieve optimális mértékben elosztása. Egy partíció legfeljebb egy átviteli egységgel rendelkezhet. hello átviteli egységek száma kisebb legyen, vagy egyenlő, mint a partíciók az eseményközpont toohello száma.

Részletes Event Hubs árakról, lásd: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Következő lépések

Az Event Hubs kapcsolatos további információkért látogasson el a következő hivatkozások hello:

* Bevezetés az [Event Hubs használatába oktatóanyag][Event Hubs tutorial]
* [Event Hubs programozási útmutató](event-hubs-programming-guide.md)
* [Rendelkezésre állás és konzisztencia az Event Hubsban](event-hubs-availability-and-consistency.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)
* [Event Hubs – minták][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Event Hubs – minták]: https://github.com/Azure/azure-event-hubs/tree/master/samples
