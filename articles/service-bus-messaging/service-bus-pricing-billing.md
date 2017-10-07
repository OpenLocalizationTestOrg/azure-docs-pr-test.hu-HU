---
title: "aaaService busz árak és számlázás |} Microsoft Docs"
description: "A Service Bus struktúra árképzési áttekintése."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2017
ms.author: sethm
ms.openlocfilehash: 4d06fe015baba45fef04e198363447c5541d1724
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-pricing-and-billing"></a>A Service Bus árak és számlázás
A Service Bus tartományregisztráció a Basic, Standard szintű, és [prémium](service-bus-premium-messaging.md) rétegek. Kiválaszthatja, hogy a szolgáltatási rétegben, az egyes Service Bus-szolgáltatásnévtér az Ön által létrehozott és a kiválasztása az adott névtérben létrehozott összes entitások közötti alkalmaz.

> [!NOTE]
> A Service Bus aktuális árazással kapcsolatos részletes információkért lásd: hello [Azure Service Bus árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/service-bus/), és hello [Service Bus gyakran ismételt kérdések](service-bus-faq.md#pricing).
>
>

A Service Bus üzenetsorok és témakörök/előfizetések szintjén használt két mérőszám következő hello használ:

1. **Üzenetküldési műveletek**: API-hívásokat indítani várólista vagy témakört/előfizetést végpontok definiálva. A mérési felül fogja írni a küldött vagy fogadott az üzenetsorok és témakörök/előfizetések számlázható használati hello elsődleges egységként üzenetek.
2. **Kapcsolatok közvetítőalapú**: egy adott egyórás mintavételi időszakban elleni várólisták, témakörök és előfizetések nyílt állandó kapcsolatok maximális száma hello definiálva. A mérési csak érvényes hello szabványos rétegben, amely további kapcsolatokat megnyithatja (korábban kapcsolatok várólista-üzenettémakör-előfizetésben száma korlátozott too100 volt) névleges kapcsolati díjköteles.

Hello **szabványos** réteg vezet be, hogy az üzenetsorok és témakörök/előfizetésekkel, kötet-alapú kedvezményeket a too80 % szinten hello legmagasabb használati eredményezve végrehajtott műveletek-es árképzési. Standard csomagra alap díj havonta, amely lehetővé teszi, hogy a tooperform too12.5 millió műveletek további költségek nélkül havonta be 10 $ is van.

Hello **prémium** réteg el vannak különítve erőforrás hello CPU és memória rétegben, hogy minden ügyfél számítási feladata elkülönítve. Ennek az erőforrás-tárolónak a neve *üzenetkezelési egység*. Legalább egy üzenetkezelési egység van lefoglalva minden prémium névtérhez. Az egyes Service Bus prémium névterekhez 1, 2 vagy 4 üzenetkezelési egység vásárolható. Egy egyetlen számítási feladat vagy entitás több üzenetkezelési egységre is kiterjedhet, és az üzenetkezelési egységek száma hello tetszés szerint módosítható lesz, bár a számlázás 24 órás vagy napi díjszabás szerint történik. hello eredménye a Service Bus-alapú megoldás kiszámítható és ismételhető teljesítménye. Nem csak kiszámíthatóbb és nagyobb rendelkezésre állású a teljesítmény, de gyorsabb is.

Vegye figyelembe, hogy hello Standard csomagra alap kell fizetni havi Azure előfizetésenként csak egyszer díjfizetéssel. Ez azt jelenti, hogy egyetlen Standard szint Service Bus-névtér létrehozása után fog tudni toocreate annyi további Standard névterek alatt, hogy azonos Azure-előfizetéssel, nélkül további kívánt kiinduló díjakat.

Hello [Service Bus árképzési](https://azure.microsoft.com/pricing/details/service-bus/) táblázat összefoglalja hello funkcionális eltérések hello Basic, Standard és Premium rétegek között.

## <a name="messaging-operations"></a>Üzenetküldési műveletek
Új árképzési modellt hello részeként üzenetsorok és témakörök/előfizetések számlázás módosítja. Ezeket az entitásokat / üzenet toobilling műveletenként számlázási rendszer átállás. Egy "művelet" felé irányuló várólista vagy témakört/előfizetést szolgáltatásvégpont tooany API-hívás hivatkozik. Ez magában foglalja a felügyeleti, a Küldés/fogadás és a munkamenet állapota műveleteket.

| Művelettípus | Leírás |
| --- | --- |
| Kezelés |Hozzon létre, Olvasás, frissítés, Törlés (CRUD) üzenetsorok és témakörök/előfizetések ellen. |
| Üzenetküldés |Üzenetek küldése és fogadása az üzenetsorok és témakörök/előfizetések. |
| Munkamenet állapota |Első vagy a munkamenet-állapot beállítást üzenetsor vagy témakör/előfizetést. |

Költség további információkért lásd: hello felsorolt hello árak [Service Bus árképzési](https://azure.microsoft.com/pricing/details/service-bus/) lap.

## <a name="brokered-connections"></a>Felügyelt kapcsolatok
*Kapcsolatok közvetítőalapú* megfeleljen az ügyfelek használati szokásokról, például az "tartósan csatlakoztatott" feladók/fogadók elleni várólisták, témakörök és előfizetések nagy számú. Tartósan csatlakoztatott feladók/fogadók, amelyekkel AMQP vagy a HTTP használata egy nem nulla fogadási időtúllépés (például HTTP hosszú lekérdezési). HTTP küldő és egy közvetlen időkorlát közvetített nem hoznak létre.

Kapcsolat kvóták és egyéb szolgáltatásra vonatkozó korlátozások: hello [Service Bus kvóták](service-bus-quotas.md) cikk.

hello Standard csomagra eltávolítja hello névtér közvetített kapcsolathoz megadott korlátot, és összesített közvetített kapcsolat használati adatokra hello Azure-előfizetés között. További információkért lásd: hello [kapcsolatok Közvetítőalapú](https://azure.microsoft.com/pricing/details/service-bus/) tábla.

> [!NOTE]
> 1000 közvetített kapcsolatok érhetők el a hello üzenetkezelési Standard csomagra (keresztül hello alap ingyenesen elérhető), és minden várólisták, témakörök és előfizetések belül hello társított Azure-előfizetés között megosztható legyen.
>
>

<br />

> [!NOTE]
> Számlázási hello létesített egyidejű kapcsolatok maximális számát alapul, és van arányosítva havonta 744 óra óránkénti alapján.
>
>

| Prémium szintű csomag |
| --- |
| Közvetített nem van szó, a hello prémium csomagban. |

Közvetített kapcsolatos további információkért lásd: hello [gyakran ismételt kérdések](#faq) későbbi szakaszában talál.

## <a name="faq"></a>GYIK

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Mik azok a közvetítőalapú kapcsolatok, és hogyan tegye I get felszámított őket?
A közvetítőalapú kapcsolat hello következő típusúként van definiálva:

1. Egy AMQP kapcsolat egy ügyfél tooa Service Bus-üzenetsorba vagy témakört/előfizetést.
2. Egy HTTP hívás tooreceive egy Service Bus-témakörbe vagy a fogadási időtúllépés értéke csak nullánál nagyobb várólista üzenetét.

A Service Bus költségekkel hello maximális száma párhuzamos közvetített kapcsolatok, mint a hello mennyiség (hello Standard csomagra 1000) tartalmazza. Csúcsait óránként mérni, hónap 744 óra elosztásával arányosítva, és adja meg a hello havi számlázási időszak alatt. hello szereplő mennyiség (1000 közvetített kapcsolatok havonta) elleni arányosítva hello óránkénti csúcsait hello összege hello a számlázott időszak végén hello lesz alkalmazva.

Példa:

1. Minden 10 000 egyetlen AMQP-kapcsolaton keresztül kapcsolódik, és parancsok kapott egy Service Bus-témakörbe. hello eszközök elküldik telemetriai események tooan Eseményközpontot. Ha minden eszköz 12 óra naponta, a következő kapcsolat díjak hello alkalmazni (hozzáadása tooany a más Service Bus témakör díjak): 10 000 kapcsolatok * 12 óra * 31 nap / 744 = 5000 közvetítőalapú kapcsolatok. Hello havi juttatást 1000 közvetített kapcsolatok, után meg 4000 közvetített kapcsolatok hello sebessége 0,03 $ $120 összesen a(z) közvetítőalapú kapcsolatonként szeretné számlázni.
2. 10 000 üzenetek fogadása egy Service Bus-üzenetsorba, nem nulla időtúllépés megadása HTTP-n keresztül. Ha minden eszköz csatlakozni 12 óra minden nap, jelenik meg a következő kapcsolat díjak hello (hozzáadása tooany a más Service Bus díjak): 10 000 HTTP-fogadási kapcsolatok * 12 órát * 31 napra / 744 óra = 5000 közvetítőalapú kapcsolatok.

### <a name="do-brokered-connection-charges-apply-tooqueues-and-topicssubscriptions"></a>Közvetítőalapú kapcsolat díjak vonatkoznak tooqueues és témakörök/előfizetések?
Igen. Nincsenek nincs kapcsolat költségekkel függetlenül hello küld a rendszer vagy az eszközök száma a HTTP protokollal események küldése. Események fogadása http használatával nagyobb, mint nulla, más néven a "hosszú lekérdezési," időtúllépés állít elő, közvetített kapcsolat díjakat. AMQP-kapcsolatok létrehozása közvetített kapcsolat költségek, függetlenül attól, hogy hello kapcsolatok használt toosend folyamatban van, vagy fogadni. Vegye figyelembe, hogy 100 közvetített kapcsolatok engedélyezve legyenek-e egy alapszintű névtér díjmentesen. Ez egyben hello engedélyezett hello Azure-előfizetés a(z) közvetítőalapú kapcsolatok maximális számát. hello első 1000 közvetített kapcsolatok Azure-előfizetés az összes szabványos névtér keresztül érhetők el (túl hello alap kell fizetni) külön díjfizetés nélkül. Mivel e támogatás elég toocover sok, szolgáltatások közötti üzenetkezelési forgatókönyveket, általában a közvetítőalapú kapcsolat díjak csak lesz megfelelő, ha azt tervezi, toouse AMQP vagy HTTP hosszú-lekérdezési a nagy mennyiségű ügyfelet; például tooachieve hatékonyabb esemény streaming vagy engedélyezése kétirányú kommunikációt sok eszköz vagy alkalmazás-példányokat.

## <a name="next-steps"></a>Következő lépések
* A Service Bus árazással kapcsolatos részleteket lásd: hello [árképzést ismertető oldalra a Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).
* Lásd: hello [Service Bus gyakran ismételt kérdések](service-bus-faq.md#pricing) az egyes közös – gyakori kérdések a Service bus árak és számlázás kapcsolatban.

[Azure portal]: https://portal.azure.com
