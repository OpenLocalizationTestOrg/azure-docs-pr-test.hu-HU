---
title: "aaaAzure Service Bus üzenetfeldolgozási architektúrát áttekintése |} Microsoft Docs"
description: "Az Azure Service Bus hello üzenet feldolgozási architektúráját ismerteti."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: baf94c2d-0e58-4d5d-a588-767f996ccf7f
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: f7606e40cdf6db3797a0db2de9365453ff2a158e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-architecture"></a>Service Bus-architektúra
Ez a cikk ismerteti az Azure Service Bus hello üzenet feldolgozási architektúráját.

## <a name="service-bus-scale-units"></a>A Service Bus skálázási egységei
A Service Bus rendszerezése *skálázási egységek* szerint történik. A skálázási egység egy telepítési egység, amely, és minden összetevők szükséges futtatási hello szolgáltatást tartalmazza. Minden régió telepít egy vagy több Service Bus skálázási egységet.

A Service Bus-névtér csatlakoztatott tooa méretezési egység. hello skálázási egység kezeli az összes entitástípus Service Bus (üzenetsorok, témakörök, előfizetések). A Service Bus skálázási egység a következő összetevők hello áll:

* **Átjárócsomópontok készlete.** Az átjárócsomópontok hitelesítik a bejövő kérelmeket. Minden átjáró nyilvános IP-címmel rendelkezik.
* **Üzenetközvetítő csomópontok készlete.** Az üzenetközvetítő csomópontok az üzenetküldési entitásokkal kapcsolatos kérelmeket dolgozzák fel.
* **Egy átjárótároló.** hello átjárótároló a skálázási egységben definiált összes entitás hello adatokat. hello átjárótároló SQL Azure-adatbázis jön létre.
* **Több üzenetküldési tároló.** Üzenetküldési tárolók várólisták, témakörök és előfizetések a skálázási egységben definiált köszönőüzenetei tárolásához. Emellett az előfizetések összes adata is megtalálható bennük. Ha [üzenetküldési entitások particionálás](service-bus-partitioning.md) van engedélyezve, üzenetsor vagy témakör megfeleltetve tooone üzenetküldési tárolóban. Hello tárolódnak az előfizetések ugyanabban az üzenetküldési tárolóban mint a szülő témakörük. A Service Bus kivételével [prémium szintű üzenetkezelés](service-bus-premium-messaging.md), hello üzenetküldési tárolók fölött SQL Azure-adatbázisok vannak megvalósítva.

## <a name="containers"></a>Tárolók
Minden üzenetküldési entitás egy adott tárolóhoz van társítva. Tároló egy logikai szerkezet, amely pontosan egy üzenetküldési tároló toostore minden vonatkozó adatok használ a tároló lehet. Minden egyes tároló tooa üzenetküldési csomópont van hozzárendelve. Általában több tároló van, mint üzenetközvetítő csomópont. Ezért az egyes üzenetközvetítő csomópontok több tárolót is betöltenek. a tárolók tooa üzenetközvetítő csomópont hello terjesztési vannak rendezve, úgy, hogy az összes üzenetközvetítő csomópontok terhelése egyenlő legyen. Ha hello terhelés mintát módosításokat (például az egyik hello tároló terhelése túlzottan megnő), vagy ha egy üzenetküldési csomópont átmenetileg elérhetetlenné válik, hello tárolók hello üzenetközvetítő csomópontok között terjeszti.

## <a name="processing-of-incoming-messaging-requests"></a>Bejövő üzenetküldési kérelmek feldolgozása
Amikor egy ügyfél küld egy kérelem tooService Bus, a hello Azure load balancer továbbítja azt tooany hello átjáró csomópontok. hello átjárócsomópont hello kérelem. Hello kérelem egy üzenetküldési entitásra (várólista, témakör, előfizetés) vonatkozik, ha a hello átjárócsomópont hello átjárótároló hello entitásra keres, és meghatározza, hogy melyik üzenetküldési tárolóban hello az entitás található. Ezután megkeresi, melyik üzenetközvetítő csomópont szolgálja ki ebben a tárolóban, és elküldi a hello kérelem toothat üzenetküldési csomópont. üzenetküldési csomópont hello hello kérést dolgoz fel, és frissíti az entitás állapotát hello hello tárolóban. majd üzenetküldési csomópont hello hello válasz hátsó toohello átjárócsomópontnak, amely egy megfelelő választ hátsó toohello ügyfél kiállított hello eredeti kérelem küldése.

![Bejövő üzenetküldési kérelmek feldolgozása](./media/service-bus-architecture/ic690644.png)

## <a name="next-steps"></a>Következő lépések
Most, hogy elolvasta a Service Bus-architektúra áttekintése, látogasson el a következő hivatkozások további információkat hello:

* [Service Bus messaging overview](service-bus-messaging-overview.md) (A Service Bus üzenetkezelésének áttekintése)
* [A Service Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
* [Üzenetsor-kezelési megoldás a Service Bus által kezelt üzenetsorok használatával](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)


