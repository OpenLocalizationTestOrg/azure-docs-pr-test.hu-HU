---
title: "A Service Bus árak és számlázás |} Microsoft Docs"
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
ms.openlocfilehash: 5161b555db96886f556a4fe96eab4415d8ccf047
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="service-bus-pricing-and-billing"></a>A Service Bus árak és számlázás
A Service Bus tartományregisztráció a Basic, Standard szintű, és [prémium](service-bus-premium-messaging.md) rétegek. Kiválaszthatja, hogy a szolgáltatási rétegben, az egyes Service Bus-szolgáltatásnévtér az Ön által létrehozott és a kiválasztása az adott névtérben létrehozott összes entitások közötti alkalmaz.

> [!NOTE]
> A Service Bus aktuális árazással kapcsolatos részletes információkért lásd: a [Azure Service Bus árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/service-bus/), és a [Service Bus gyakran ismételt kérdések](service-bus-faq.md#pricing).
>
>

A Service Bus üzenetsorok és témakörök/előfizetések a következő két mérőszámok használ:

1. **Üzenetküldési műveletek**: API-hívásokat indítani várólista vagy témakört/előfizetést végpontok definiálva. A mérési felül fogja írni az üzeneteket küldhetnek vagy fogadhatnak, üzenetsorok és témakörök/előfizetések számlázható használatát elsődleges egységei.
2. **Kapcsolatok közvetítőalapú**: definiált várólisták, témakörök és előfizetések ellen, egy adott egyórás mintavételi időszakban nyílt állandó kapcsolatok maximális száma. A mérési csak a normál rétegben, amely további kapcsolatokat megnyithatja vonatkozik (korábban kapcsolatok volt legfeljebb 100 / várólista-üzenettémakör-előfizetésre) névleges kapcsolati díjköteles.

A **szabványos** réteg vezet be, hogy az üzenetsorok és témakörök/előfizetések, ami azt eredményezi, hogy kötet-alapú kedvezményeket 80 %-a legmagasabb szintű használati végrehajtott műveletek-es díjszabása. Standard csomagra alap díj havonta, amely lehetővé teszi további költségek nélkül havonta legfeljebb 12,5 millió műveletek 10 $ is van.

A **prémium** réteg el vannak különítve erőforrás a Processzor- és a rétegben, hogy minden ügyfél számítási feladata elkülönítve. Ennek az erőforrás-tárolónak a neve *üzenetkezelési egység*. Legalább egy üzenetkezelési egység van lefoglalva minden prémium névtérhez. Az egyes Service Bus prémium névterekhez 1, 2 vagy 4 üzenetkezelési egység vásárolható. Egyetlen számítási feladat vagy entitás több üzenetkezelési egységre is kiterjedhet, az üzenetkezelési egységek száma pedig tetszés szerint módosítható, bár a számlázás 24 órás vagy napi díjszabás szerint történik. Az eredmény a Service Bus-alapú megoldás kiszámítható és ismételhető teljesítménye. Nem csak kiszámíthatóbb és nagyobb rendelkezésre állású a teljesítmény, de gyorsabb is.

Vegye figyelembe, hogy a Standard csomag alap kell fizetni havi Azure előfizetésenként csak egyszer díjfizetéssel. Ez azt jelenti, hogy egyetlen Standard szint Service Bus-névtér létrehozása után lesz annyi további Standard névterek létrehozásához ahányat csak szeretne, hogy azonos Azure-előfizetésre, az alap további költségek nélkül.

A [Service Bus árképzési](https://azure.microsoft.com/pricing/details/service-bus/) táblázat összefoglalja a funkcionális eltérések a Basic, Standard és Premium rétegek között.

## <a name="messaging-operations"></a>Üzenetküldési műveletek
Az új árképzési modellt részeként üzenetsorok és témakörök/előfizetések számlázás módosítja. Ezeket az entitásokat a művelet számlázással vannak átállás számlázási egy üzenet. Egy "művelet" minden API-hívás felé irányuló várólista vagy témakört/előfizetést szolgáltatásvégpont hivatkozik. Ez magában foglalja a felügyeleti, a Küldés/fogadás és a munkamenet állapota műveleteket.

| Művelettípus | Leírás |
| --- | --- |
| Kezelés |Hozzon létre, Olvasás, frissítés, Törlés (CRUD) üzenetsorok és témakörök/előfizetések ellen. |
| Üzenetküldés |Üzenetek küldése és fogadása az üzenetsorok és témakörök/előfizetések. |
| Munkamenet állapota |Első vagy a munkamenet-állapot beállítást üzenetsor vagy témakör/előfizetést. |

Költség további információkért lásd: a felsorolt árak a [Service Bus árképzési](https://azure.microsoft.com/pricing/details/service-bus/) lap.

## <a name="brokered-connections"></a>Felügyelt kapcsolatok
*Kapcsolatok közvetítőalapú* megfeleljen az ügyfelek használati szokásokról, például az "tartósan csatlakoztatott" feladók/fogadók elleni várólisták, témakörök és előfizetések nagy számú. Tartósan csatlakoztatott feladók/fogadók, amelyekkel AMQP vagy a HTTP használata egy nem nulla fogadási időtúllépés (például HTTP hosszú lekérdezési). HTTP küldő és egy közvetlen időkorlát közvetített nem hoznak létre.

Kapcsolat kvóták és egyéb szolgáltatásra vonatkozó korlátozások: a [Service Bus kvóták](service-bus-quotas.md) cikk.

Standard csomagra eltávolítja a névtér közvetített kapcsolathoz megadott korlátot, és összesített közvetített kapcsolat használati adatokra az Azure-előfizetés között. További információkért lásd: a [kapcsolatok Közvetítőalapú](https://azure.microsoft.com/pricing/details/service-bus/) tábla.

> [!NOTE]
> 1000 közvetített kapcsolatok, és a standard szintű üzenetkezelési réteg (keresztül az alap ingyenesen elérhető), és minden üzenetsorok, témakörök és előfizetések belül a társított Azure-előfizetés között megosztható legyen.
>
>

<br />

> [!NOTE]
> A szolgáltatás díjszabását az egyidejű kapcsolatok maximális száma határozza meg. Az elszámolás óraszám alapján, havi 744 óra figyelembe vételével történik.
>
>

| Prémium szintű csomag |
| --- |
| Prémium szinten a felügyelt kapcsolatokért nem számítunk fel díjat. |

Közvetített kapcsolatos további információkért tekintse meg a [gyakran ismételt kérdések](#faq) későbbi szakaszában talál.

## <a name="faq"></a>GYIK

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Mik azok a közvetítőalapú kapcsolatok, és hogyan tegye I get felszámított őket?
Felügyelt kapcsolat a következők egyike lehet:

1. Egy AMQP kapcsolat egy ügyfél és egy Service Bus-üzenetsorba, illetve témakört/előfizetést.
2. Egy HTTP-hívás üzenetet fogad egy Service Bus-üzenettémától vagy -várólistától, amelynek fogadási időkorlátja nagyobb, mint nulla.

A Service Bus költségekkel, amelyek mérete meghaladja a keretbe (a normál rétegben, mint 1000) egyidejű közvetített kapcsolatok maximális száma. A maximális szám mérése óránként történik, majd havi 744 órával elosztva elkészül az elszámolás, amely a havi számlázási időszak alatt halmozódik. A szolgáltatásban foglalt mennyiség (1000 felügyelt kapcsolat havonta) a számlázási időszak végén kerül felszámításra az óránként megállapított maximális kapcsolatok összege alapján.

Példa:

1. Minden 10 000 egyetlen AMQP-kapcsolaton keresztül kapcsolódik, és parancsok kapott egy Service Bus-témakörbe. Az eszközök telemetriai események küldése az Eseményközpontba. Ha minden eszköz 12 óra naponta, a következő kapcsolat díjak vonatkoznak-e (mellett egyéb Service Bus témakör díjak): 10 000 kapcsolatok * 12 óra * 31 nap / 744 = 5000 közvetítőalapú kapcsolatok. Után 1000 közvetített kapcsolatok havi támogatás akkor felszámított os 0,03 $ $120 összesen a(z) közvetítőalapú kapcsolatonként 4000 közvetített kapcsolatok.
2. 10 000 üzenetek fogadása egy Service Bus-üzenetsorba, nem nulla időtúllépés megadása HTTP-n keresztül. Ha minden eszköz csatlakozni 12 óra minden nap, jelenik meg a következő kapcsolat költségek (mellett egyéb Service Bus díjak): 10 000 HTTP-fogadási kapcsolatok * 12 órát * 31 napra / 744 óra = 5000 közvetítőalapú kapcsolatok.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>A felügyelt kapcsolatok díjai a várólistákra és az üzenettémákra/előfizetésekre is vonatkoznak?
Igen. Nincsenek nincs kapcsolat költségekkel függetlenül küld a rendszer vagy az eszközök számát a HTTP protokollal események küldése. Események fogadása http használatával nagyobb, mint nulla, más néven a "hosszú lekérdezési," időtúllépés állít elő, közvetített kapcsolat díjakat. Az AMQP-kapcsolatok küldésre és fogadásra használt kapcsolatok esetén is felügyelt kapcsolati díjat generálnak. Vegye figyelembe, hogy 100 közvetített kapcsolatok engedélyezve legyenek-e egy alapszintű névtér díjmentesen. Azt is engedélyezett az Azure-előfizetés a(z) közvetítőalapú kapcsolatok maximális számát. Az első 1000 közvetített kapcsolatok Azure-előfizetés az összes szabványos névtér között megtalálhatók (túl az alap kell fizetni) külön díjfizetés nélkül. Mivel e támogatás elég sok szolgáltatások közötti üzenetküldő forgatókönyvekhez, közvetített kapcsolat díjak általában csak lesz megfelelő, ha azt tervezi, AMQP vagy HTTP hosszú lekérdezési használata nagy mennyiségű ügyfelet; például eléréséhez a hatékonyabb esemény streaming vagy kétirányú kommunikáció engedélyezése az sok eszköz vagy alkalmazás-példányokat.

## <a name="next-steps"></a>Következő lépések
* A Service Bus árazással kapcsolatos részleteket lásd: a [árképzést ismertető oldalra a Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).
* Tekintse meg a [Service Bus gyakran ismételt kérdések](service-bus-faq.md#pricing) az egyes közös – gyakori kérdések a Service bus árak és számlázás kapcsolatban.

[Azure portal]: https://portal.azure.com
