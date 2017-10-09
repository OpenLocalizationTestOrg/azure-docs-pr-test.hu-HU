---
title: aaaPerformance szempontok az Azure Traffic Managerben |} Microsoft Docs
description: "A Traffic Manager teljesítményének megértése, hogyan tootest webhely teljesítménye, a Traffic Manager használata esetén"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 3ba5dfa1-2922-43f1-9a23-d06969c4a516
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: fd4e6cb221a2ceee63ec57237ee90fd714e91db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-considerations-for-traffic-manager"></a>Teljesítménnyel kapcsolatos megfontolások a Traffic Manager esetében

Ezen a lapon a teljesítménnyel kapcsolatos szempontok a Traffic Managerrel ismerteti. Vegye figyelembe a következő forgatókönyv hello:

A webhely hello WestUS példányok és EastAsia régiók rendelkezik. Hello példányok egyik hello traffic manager mintavétel hello állapot-ellenőrzése sikertelen. Alkalmazás forgalom irányított toohello kifogástalan régióban. Ennél a feladatátvételnél várt, de teljesítmény hello késés hello forgalom most utazás tooa távoli régió alapján problémát okozhat.

## <a name="performance-considerations-for-traffic-manager"></a>Teljesítménnyel kapcsolatos megfontolások a Traffic Manager esetében

hello csak befolyásolta a teljesítményt, amelyeken a webhelyen a Traffic Manager hello kezdeti DNS-címkeresés. A Traffic Manager-profil neve hello DNS kérelmet hello Microsoft DNS-gyökérkiszolgáló állomások hello trafficmanager.net zóna kezeli. A TRAFFIC Manager tölti fel, és rendszeresen frissíti a hello Microsoft DNS gyökérkiszolgálók hello Traffic Manager-házirend és hello mintavételi eredmények alapján. Ezért esetén hello kezdeti DNS-címkeresést nincs DNS-lekérdezések tooTraffic Manager küldése.

A TRAFFIC Manager számos összetevőt épül fel: DNS-kiszolgálók, az API-szolgáltatás, hello tárolási réteg és szolgáltatás figyelése a végpont névhez. A Traffic Manager szolgáltatás valamelyik összetevője nem sikerül, ha nincs a Traffic Manager-profil társított hello DNS-neve nincs hatással. hello Microsoft DNS-kiszolgálók hello rekordok változatlanok maradnak. Azonban végpontmonitoring kijelző és a DNS-frissítését nem fordulhat elő. A Traffic Manager ezért nem képes tooupdate DNS toopoint tooyour feladatátvételi helyen amikor az elsődleges hely leáll.

DNS-névfeloldás gyorsan elvégezhető, és az eredmény tárolódik. hello kezdeti DNS-címkeresés hello sebességétől függ a névfeloldáshoz használt DNS-kiszolgálók hello ügyfél hello. Általában egy ügyfél egy DNS-címkeresés belül ~ 50 ms hajthatja végre. hello keresési eredményei hello gyorsítótárba kerüljenek-e DNS idő élettartamát (TTL) hello hello időtartamára. hello alapértelmezett élettartam a Traffic Manager érték 300 másodperc.

Forgalom Traffic Manager használatával nem terjeszthetők. Hello DNS-címkeresés befejeztét követően hello ügyfél rendelkezik egy példányát a webhely IP-címet. hello ügyfél toothat cím közvetlenül csatlakozik, és nem felel meg a Traffic Manager használatával. hello választja Traffic Manager-házirend hello DNS teljesítménye nincs hatással van. Teljesítmény útválasztás-metódus azonban negatívan befolyásolhatja a hello alkalmazások használati élményét. Például a házirend Ázsiában található Észak-Amerika tooan példány forgalmát átirányítja a felhasználókat, ezek a munkamenetek hello hálózati késés a teljesítménycsökkenés oka lehet.

## <a name="measuring-traffic-manager-performance"></a>A Traffic Manager teljesítményének mérése

Nincsenek több webhelye toounderstand hello teljesítmény és működése a Traffic Manager-profil is használhat. Ezeken a webhelyeken számos ingyenes azonban előfordulhat, hogy korlátozott. Bizonyos webhelyek nyújtanak a bővített figyelési és jelentéskészítési díjköteles.

a helyek hello eszközök mérésére DNS késések fordulnak elő, és megjelenítési hello megoldott hello világszerte elhelyezkedő ügyfelek helyei IP-címet. A legtöbb ezek az eszközök nem gyorsítótárazzák a hello DNS-találatokat. Ezért hello eszközök megjelenítése hello teljes DNS-címkeresés teszt minden egyes futtatásakor. A saját ügyfélről tesztelésekor csak tapasztal hello teljes DNS-keresési teljesítményét egyszer hello TTL időtartama alatt.

## <a name="sample-tools-toomeasure-dns-performance"></a>A minta eszközök toomeasure DNS teljesítményét

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS teljesítmény számos eszközt kínál. hello DNS összehasonlítás eszköz tekintheti meg, hogy mennyi ideig tart tooresolve a DNS-nevét, és hogyan, amely összehasonlítja tooother DNS-szolgáltatók.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Hello legegyszerűbb eszközök egyik WebSitePulse. Adja meg a hello URL-cím toosee DNS-feloldási idejét, az első bájtig eltelt, az utolsó bájtig eltelt és egyéb teljesítménystatisztikáit. Különböző teszt három hely közül választhat. Ebben a példában látható, hogy hello első végrehajtási jeleníti meg, hogy a DNS-címkeresés 0.204 mp vesz igénybe.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Mivel hello eredmény tárolódik, a második vizsgálat hello hello ugyanazt a Traffic Manager végpont hello DNS-címkeresés szükséges 0,002 mp.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [Hitelesítésszolgáltató App szintetikus figyelője](https://asm.ca.com/en/checkit.php)

    Korábbi nevén hello Watchmouse ellenőrzés webhely eszköz, a webhely is láthat hello DNS-feloldási idő a több földrajzi régióban elhelyezkedő egyidejűleg. Adja meg a hello URL-cím toosee DNS-feloldási idejét, a kapcsolat ideje és a sebesség több földrajzi helyről. A teszt toosee, mely üzemeltetett szolgáltatás visszaadott használjon hello világ különböző helyeken.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Ez az eszköz biztosít minden elem egy weblap teljesítménystatisztikáit. hello lap elemzés lapon hello töltött idő százalékos aránya a DNS-címkeresés jeleníti meg.

* [Újdonságok a DNS?](http://www.whatsmydns.net/)

    A hely nem a DNS-címkeresést 20 különböző helyekről, és hello eredményeit jeleníti meg a térképen.

* [Webes felület Dig](http://www.digwebinterface.com)

    Ezen a helyen látható, a DNS-adatok, beleértve a CNAME és A rekordok részletes. Győződjön meg arról, hello "Kifestés egyszínűre output" és "Statisztikák" Ellenőrizze a beállítások, és válassza az "All" Nameservers alatt.

## <a name="next-steps"></a>Következő lépések

[A Traffic Manager forgalom-útválasztási módszerei](traffic-manager-routing-methods.md)

[A Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md)

[A Traffic Manager műveletei (REST API-referencia)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-parancsmagok](http://go.microsoft.com/fwlink/p/?LinkId=400769)

