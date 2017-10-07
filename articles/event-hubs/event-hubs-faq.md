---
title: "aaaAzure Event Hubs – gyakori kérdések |} Microsoft Docs"
description: "Az Azure Event Hubs gyakran ismételt kérdések (GYIK)"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: bfa10984-eb22-4671-861a-f377a90d9372
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm;shvija
ms.openlocfilehash: cc0844edcc38ad167cde9497d58d44155fc90b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-frequently-asked-questions"></a>Az Event Hubs gyakran ismételt kérdések

## <a name="general"></a>Általános kérdések

### <a name="what-is-hello-difference-between-event-hubs-basic-and-standard-tiers"></a>Mi az az Event Hubs egyszerű és szabványos rétegek hello különbségének?

Standard szint hello Azure Event hubs túl a hello alapszintű rétegben elérhető szolgáltatásokat biztosítja. Standard hello a következő funkciók találhatók:
* Esemény hosszabb megőrzési
* További közvetített kapcsolatok, egy keretét kell fizetni az ennél hosszabb hello-szám
* Több mint egy egyetlen felhasználói csoport
* [Rögzítése](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)

További információ a tarifacsomagok, beleértve az Event Hubs dedikált, lásd: hello [díjszabása Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Mik az Event Hubs átviteli egységek?

Explicit módon választja az Event Hubs átviteli egységek, vagy hello Azure-portálon keresztül vagy Event Hubs Resource Manager-sablonok. Átviteli egységek alkalmazása tooall az event hubs egy Event Hubs névtérben, és átviteli egységenként feljogosítja a hello névtér toohello képességek a következő:

* Érkező jelzések (események küldése eseményközpontba), de több mint 1000 érkező jelzések, felügyeleti műveletek vagy vezérlő másodpercenkénti too1 MB API meghívja a másodpercenkénti számát.
* Too2 MB másodpercenként kilépő események (felhasznált az eseményközpontban lévő események).
* Másolatot too84 GB (elegendő hello alapértelmezett megőrzési 24 órás időszak) esemény tárhelyet.

Event Hubs átviteli egységek vannak számlázva hourly, hello hello megadott óra során kiválasztott egységek maximális száma alapján.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hogyan tartatja be az Event Hubs átviteli egység korlátai?
Amennyiben hello érkező teljes átviteli sebesség vagy hello teljes érkező események száma egy névtér összes eseményközpontjában között meghaladja a hello összesített egység támogatások, feladók szabályozott és hibáját jelzik, hogy hello érkező kvóta túl lett lépve.

Ha hello teljes kimenő átviteli vagy hello teljes kimenő eseménygyakoriság egy névtér összes eseményközpontjában között meghaladja a hello összesített egység támogatások, a fogadók szabályozott, és a fogadási hibáját jelzik, hogy hello kilépő kvóta túl lett lépve. Bemenő és kimenő kvóták külön-külön tartatja be, hogy a feladó okozhat esemény fogyasztás tooslow le, és nem a fogadó megakadályozhatja események küldése eseményközpontba.

### <a name="is-there-a-limit-on-hello-number-of-throughput-units-that-can-be-selected"></a>Az átviteli egységek választható hello száma korlátozva van?
Nincs a névtér 20 átviteli egység alapértelmezett kvótát. Átviteli egységek nagyobb kvótát egy támogatási jegy tervátalakítási által kérhet. Túl hello 20 átviteli egység határt a csomagok érhetők el 20 és 100 átviteli egységek. Vegye figyelembe, hogy a több mint 20 átviteli egység szükségtelenné hello képességét toochange hello átviteli egységek száma a támogatási jegy bejelentés nélkül.

### <a name="can-i-use-a-single-amqp-connection-toosend-and-receive-from-multiple-event-hubs"></a>Használjon egy AMQP egyetlen kapcsolat toosend és több eseményközpontokból származó?
Igen, mindaddig, amíg az összes hello az event hubs szerepelnek hello ugyanazt a névteret.

### <a name="what-is-hello-maximum-retention-period-for-events"></a>Mi az a maximális megőrzési idő hello események?
Event Hubs Standard csomagra jelenleg támogatja a maximális megőrzési időtartam 7 nap. Vegye figyelembe, hogy az event hubs nem feltétlenül állandó adattárként. Nagyobb, mint 24 óra szánt forgatókönyvben tetszés szerinti tooreplay egy eseményt az adatfolyam azt is megőrzési időtartamú hello azonos rendszerek; például tootrain vagy a meglévő adatok új gépi tanulási modell ellenőrzése. Megőrzési 7 napon túl kell hibaüzenet, amely lehetővé teszi [Event Hubs rögzítése](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) az esemény hub az event hub toohello tárfiók vagy az Azure Data Lake szolgáltatásfiók a hello adatait kéri le. Rögzítési engedélyezése azt eredményezi, a megvásárolt átviteli egység alapján díj azok háromszorosa.

### <a name="where-is-azure-event-hubs-available"></a>Ahol lehetőség van az Azure Event Hubs?
Az Azure Event Hubs minden támogatott Azure-régió nem áll rendelkezésre. A listáját, és a Microsoft hello [Azure-régiók](https://azure.microsoft.com/regions/) lap.  

## <a name="best-practices"></a>Ajánlott eljárások

### <a name="how-many-partitions-do-i-need"></a>Hány partíciókkal van szükségem?
Ne feledje, hogy a partíciók száma eseményközpontban hello adja a telepítés után nem módosítható. A, szem előtt fontos toothink kapcsolatos első lépések előtt kell hány partíciók esetén. 

Az Event Hubs tervezett tooallow fogyasztói csoportonként egypartíciós olvasó. A legtöbb használati esetek hello alapértelmezett beállítás négy partíció is használhatók. Ha a tooscale a Eseményfeldolgozási, érdemes lehet tooconsider két további partíció hozzáadása. Nincs megadott átviteli korlát partíciók, azonban az átviteli egységek hello száma korlátozza hello összesített átviteli sebesség a névtérben. A névtér hello átviteli egységek számának növelésével, érdemes lehet további partíciókat tooallow egyidejű olvasók tooachieve saját maximális átviteli sebesség.

Azonban ha egy modell, amelyben az alkalmazás rendelkezik egy kapcsolat tooa adott partíció, hello a partíciók számának növelése nem lehet bármely juttatás tooyou. További információ: [rendelkezésre állás és a konzisztencia](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Díjszabás

### <a name="where-can-i-find-more-pricing-information"></a>Hol találnak további információkat?
Az Event Hubs árazással kapcsolatos részletes információkért lásd: hello [díjszabása Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Az Event Hubs események megőrzése 24 óránál díjat van?
hello Event Hubs Standard csomag lehetővé teszi üzenet megőrzési 7 nap legfeljebb 24 óránál hosszabb. Hello hello tárolt események teljes száma meghaladja hello tárolási támogatás számára kijelölt átviteli egységek (átviteli egységenként 84 GB) hello száma, ha a hello mérete nagyobb hello támogatás díjakon hello közzé az Azure Blob storage sebessége. hello tárolási támogatás átviteli egységenként hozzá van rendelve minden tárolási költségek megőrzési időszakokra 24 óra (hello alapértelmezett), még akkor is, ha a hello átviteli egység toohello maximális érkező támogatás másolatot használja.

### <a name="how-is-hello-event-hubs-storage-size-calculated-and-charged"></a>Hogyan hello Event Hubs tárméret kiszámítása és felszámított?
minden tárolt eseményeket, többek között a bármely esemény fejlécek vagy a lemez tárolási struktúrák belső terhelés az összes eseményközpontjában hello összmérete hello nap folyamán mérése történik. Hello a hello nap végén hello maximális tárolómérete számítja ki. hello napi tárolási támogatás hello átviteli egységek (átviteli egységenként biztosít egy 84 GB-os támogatás) hello napra kijelölt minimális száma alapján van kiszámítva. Ha hello teljes mérete meghaladja a hello számítása naponta tárolási támogatás, az Azure Blob storage díjszabásának hello túlzott tárolási Számlázva (hello: **helyileg redundáns tárolás** sebessége).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Az Event Hubs érkező jelzések kiszámítási módját?
Minden esemény küldött tooan eseményközpont számlázható üzenet számít. Egy *érkező esemény* van definiálva, amely kisebb vagy egyenlő, mint adategység, too64 KB. Kisebb vagy egyenlő, mint bármely esemény too64 KB méretű toobe egy számlázható esemény tekinthető. Ha hello esemény 64 KB-nál nagyobb, számlázható események száma hello kiszámítása szerint toohello eseményméret 64 KB-os többszörösével növekednek. Például egy esemény toohello eseményközpont küldött 8 KB-os esemény terheli, de 96 KB üzeneteket toohello event hubs két esemény terheli.

Az eseményközpontok felé, valamint a felügyeleti műveleteket és a vezérlő indított, például az ellenőrzőpontok nem számítanak számlázható érkező jelzések, de másolatot toohello átviteli egység támogatás keletkeznek a felhasznált események.

### <a name="do-brokered-connection-charges-apply-tooevent-hubs"></a>Közvetítőalapú kapcsolat díjak vonatkoznak tooEvent Hubs?
Kapcsolat többletköltségeket csak hello AMQP protokollt használja. Nincsenek nincs kapcsolat költségekkel függetlenül hello küld a rendszer vagy az eszközök száma a HTTP protokollal események küldése. Ha azt tervezi, hogy toouse AMQP (például tooachieve hatékonyabb esemény streaming vagy tooenable kétirányú kommunikáció az IoT-parancs és vezérlési forgatókönyvek esetén), lásd: hello [árakról Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) lap részletei sok kapcsolatok az egyes szolgáltatásszinteken szerepelnek.

### <a name="how-is-event-hubs-capture-billed"></a>Hogyan számítjuk az Event Hubs Rögzítés díját?
Rögzítés engedélyezve van, ha bármely eseményközpont hello névtérben hello rögzítési beállítás engedélyezve van. Event Hubs rögzítése lesz számlázva óránkénti megvásárolt átviteli egységenként. Hello átviteli egységek száma nő vagy csökken, mivel az Event Hubs rögzítése számlázási teljes óránkénti növekményekben módosítások tükrözi. Event Hubs rögzítése számlázással kapcsolatos további információkért lásd: [árakról Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-hello-storage-account-i-select-for-event-hubs-capture"></a>I alapján számlázzuk jelölhető ki az Event Hubs rögzítése hello tárfiók?
Rögzítési megadnia, ha az eseményközpontok a storage-fiókot használ. Mivel ez a tárfiók, ehhez a konfigurációhoz módosítások számlázott tooyour Azure-előfizetés.

## <a name="quotas"></a>Kvóták

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Minden Event Hubs társított kvóták vannak?
Minden Event Hubs kvóták listájáért lásd: [kvóták](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="what-are-some-of-hello-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Melyek azok az Event Hubs és a javasolt lépések által generált hello kivételek?
A lehetséges az Event Hubs kivételek listájáért lásd: [kivételek áttekintése](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Diagnosztikai naplók
Az Event Hubs támogatja a két típusú [diagnosztikai naplók](event-hubs-diagnostic-logs.md) -rögzítési hibanaplójában és a műveleti naplókat - mindkettőnek jelennek meg a JSON-ban, és keresztül bekapcsolhatók hello Azure-portálon.

### <a name="support-and-sla"></a>Támogatás és szolgáltatásszintek
Az Event Hubs számára a technikai támogatási szolgálathoz hello keresztül érhető el [közösségi fórumok](https://social.msdn.microsoft.com/forums/azure/home). A számlázás és az előfizetések kezelésének támogatása díjmentesen igénybe vehető.

toolearn az SLA-t, kapcsolatos további információkért lásd: hello [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/) lap.

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Event Hub létrehozása](event-hubs-create.md)
