---
title: Traffic Manager-profilok aaaNested |} Microsoft Docs
description: "Ez a cikk ismerteti a hello beágyazott profilok funkció az Azure Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f1b112c4-a3b1-496e-90eb-41e235a49609
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: e476d58a82ed94d12731156280c9665f980de0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nested-traffic-manager-profiles"></a>Beágyazott Traffic Manager-profilok

A TRAFFIC Manager forgalom-útválasztási módszerekkel, amelyek lehetővé teszik a toocontrol hogyan úgy dönt, a Traffic Manager melyik végponthoz forgalmat mindegyik végfelhasználói kell fogadni számos tartalmazza. További információkért lásd: [Traffic Manager forgalom-útválasztási módszert](traffic-manager-routing-methods.md).

Minden egyes Traffic Manager-profil megadja egy forgalom-útválasztási módszert. Vannak azonban forgatókönyv esetén van szükség, kifinomultabb a forgalom útválasztásához, mint egyetlen Traffic Manager-profil által biztosított hello útválasztást. A Traffic Manager profilok toocombine hello előnyei egynél több forgalom-útválasztási módszert ágyazhatja be. Beágyazott profilok lehetővé teszik toooverride hello alapértelmezett Traffic Manager viselkedés toosupport nagyobb és összetettebb alkalmazások központi telepítéseit.

hello a következő példák bemutatják, hogyan toouse beágyazott Traffic Manager-profilokat a különböző forgatókönyvekben.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>1. példa: Kombinálásával "Teljesítmény" és "Weighted" forgalom-Útválasztás

Tegyük fel, hogy telepítette-e az alkalmazás a következő Azure-régiók hello: USA nyugati régiója, Nyugat-Európában és Kelet-Ázsia. A Traffic Manager "Teljesítmény" forgalom-útválasztási módszer toodistribute forgalom toohello régió legközelebbi toohello felhasználó használja.

![Egy Traffic Manager-profil][4]

Most tegyük fel kívánja tootest tooyour szolgáltatásfrissítés előtt kiosztását több széles körben. Azt szeretné, hogy toouse hello "súlyozott" forgalom-útválasztási módszer toodirect tooyour próbatelepítés forgalom csekély. Nyugat-Európában beállítása hello próbatelepítés hello meglévő éles környezet mellett.

Nem lehet kombinálni mindkét Weighted és "teljesítmény forgalom-útválasztási egyetlen profilban. toosupport ebben a forgatókönyvben a Traffic Manager-profilt hoz létre hello két Nyugat-Európában végpontok és hello "Weighted" forgalom-útválasztási módszer használatával. A "child" profil a következő végpont toohello "parent" profilként hozzáadásához. hello szülő profil hello teljesítmény forgalom-útválasztási módszer továbbra is használja, és tartalmazza a végpontként globális központilag olyan hello.

a következő diagram hello Ez a példa szemlélteti:

![Beágyazott Traffic Manager-profilok][2]

Ebben a konfigurációban keresztül hello szülő profil irányított forgalom osztja el a forgalmat különböző régiókban általában. Nyugat-Európában, belül hello egymásba ágyazott profilelérési osztja el a forgalmat toohello termelési és tesztelési végpontok hozzárendelt toohello súlyok alapján történik.

Amikor hello szülő profil hello "Teljesítmény" forgalom-útválasztási módszert használja, minden egyes végpont hozzá kell rendelni egy helyre. hello hely van hozzárendelve, hello végpont konfigurálásakor. Válassza ki a hello Azure-régiót legközelebbi tooyour központi telepítés. hello Azure-régiók értékeket hello hely hello Internet késés tábla által támogatott. További információkért lásd: ["Teljesítmény" Traffic Manager forgalom-útválasztási módszer](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>2. példa: A Végpontmonitoring kijelző a beágyazott profilok

A TRAFFIC Manager aktívan figyeli a szolgáltatás végpontok hello állapotát. A végpont nem kifogástalan, ha a Traffic Manager arra utasítja a felhasználók tooalternative végpontok toopreserve hello a szolgáltatás elérhetőségét. A figyelés és a feladatátvételi végpontviselkedéshez tooall forgalom-útválasztási módszer vonatkozik. További információkért lásd: [Traffic Manager-végpont figyelés](traffic-manager-monitoring.md). Végpontmonitoring kijelző eltérő módon működik a beágyazott profilok. A beágyazott profilok hello szülő profil nem ellenőrzi a rendszerállapot hello gyermek közvetlenül. Ehelyett hello gyermek profil végpontok hello állapotának használt toocalculate hello hello gyermek profil általános állapotát. Ez állapottal kapcsolatos adatok mentése beágyazott hello-profil hierarchia propagálja. hello szülő profil használ a összesített állapotát toodetermine e toodirect forgalom toohello gyermek profil. Lásd: hello [gyakran ismételt kérdések](traffic-manager-FAQs.md#traffic-manager-nested-profiles) a beágyazott profilok állapotfigyelését teljes leírását.

Vissza az előző példa toohello, tegyük fel, Nyugat-Európában hello éles központi telepítése sikertelen. Alapértelmezés szerint a hello "child" profil minden forgalom toohello próbatelepítés irányítja. Próbatelepítés hello szintén meghiúsul, ha a hello szülő profil határozza meg, hogy hello gyermek profil nem kell kapnia forgalom, mivel az összes gyermek-végpontok a nem megfelelő. Ezt követően hello szülő profil osztja el a forgalmat toohello más régiókban.

![Beágyazott profil feladatátvevő (alapértelmezés)][3]

Előfordulhat, hogy ezzel az elrendezéssel elégedett. Vagy előfordulhat, hogy összes forgalmat a Nyugat-Európa most fog toohello az üzembe helyezési teszt helyett egy korlátozott részhalmaza forgalom érintett. Függetlenül hello hello állapotát a központi telepítés tesztelése, kívánja toofail toohello keresztül más régiókban hello éles környezet Nyugat-Európában meghibásodása esetén. tooenable ennél a feladatátvételnél megadhat hello "MinChildEndpoints" paraméter hello szülő profilban végpontjaként hello gyermek profil konfigurálásakor. hello paraméter hello hello gyermek profil számára elérhető végpontok minimális számát határozza meg. hello alapértelmezett értéke "1". Ebben az esetben be hello MinChildEndpoints érték too2. Az ezen küszöbérték alatti hello szülő profil úgy ítéli meg, hello teljes gyermek profil toobe nem érhető el, és arra utasítja a forgalom toohello végpontja.

hello következő ábra azt mutatja be ezt a konfigurációt:

![A beágyazott "MinChildEndpoints" profil feladatátvétel = 2][4]

> [!NOTE]
> hello "Priority" forgalom-útválasztási módszer elosztása az összes forgalom tooa végpontot. Így nincs kevés célja a MinChildEndpoints beállítás csak az "1" egy gyermek profil.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>3. példa: Rangsorolt feladatátvételi régiók "Teljesítmény" forgalom-Útválasztás

hello alapértelmezett viselkedése hello "Teljesítmény" forgalom-útválasztási módszer tervezett tooavoid túlzott betöltése hello ezután a legközelebbi végpont és kaszkádolt sorozatos hibák okozza. A végpont nem sikerül, ha összes forgalom volna lett irányított toothat végpont egyenlően van elosztva toohello végpontja minden egyes.

![Útválasztás és alapértelmezett feladatátvétel "Teljesítmény" forgalom][5]

Előfordulhat azonban hogy inkább hello Nyugat-Európában forgalom feladatátvételi tooWest SZÁMUNKRA, és csak közvetlen forgalom tooother régiók, ha mindkét végpont nem érhetők el. Ez a megoldás gyermek profillal rendelkező hello "Priority" forgalom-útválasztási módszert hozhat létre.

!["Teljesítmény" forgalom-útválasztási kedvezményes feladatátvételi][6]

Hello Nyugat-Európában végpont hello USA nyugati régiója végpont-nál magasabb prioritással bír, mivel minden adatforgalom toohello Nyugat-Európában végpont mindkét végpont online állapotba kerülnek. Nyugat-Európában nem sikerül, a forgalom esetén irányított tooWest USA. Beágyazott hello profillal forgalom irányított tooEast Ázsia csak akkor Nyugat-Európában, mind az USA nyugati régiója sikertelen.

Minden régió esetében ismételje meg az ebben a mintában. Cserélje le hello szülő profil három végpontjai három alárendelt profilok, egyes a rangsorolt feladatátvételi feladatütemezési biztosítása.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-hello-same-region"></a>4. példa: "Teljesítmény" forgalom-útválasztási hello több végpontok közötti azonos ellenőrzése régió

Tegyük fel, hogy hello forgalom-útválasztási módszert használ, amely több végpont rendelkezik az adott profil teljesítményét. Alapértelmezés szerint a forgalom toothat régió egyenlően elosztva az adott régióban elérhető végpontjai irányítja.

!["Teljesítmény" forgalom-útválasztási régió a forgalom eloszlása (alapértelmezés)][7]

Több végpont hozzáadása a Nyugat-Európában, helyett ezekre a végpontokra egy külön gyermek profil parancsfájlblokkjában találhatók. hello gyermek profil hozzá lesz adva toohello szülő Nyugat-Európában hello csak végpontként. hello gyermek profilon hello-beállítások engedélyezése a prioritásalapú vagy súlyozott forgalom-útválasztás adott régión belül hello forgalomeloszlás, Nyugat-Európa szabályozhatja.

!["Teljesítmény" forgalom-útválasztási az egyéni terület a forgalom eloszlása][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>5. példa: A végpont figyelési beállításokat

Tegyük fel, hogy azok be Traffic Manager toosmoothly forgalom verziójáról végez áttelepítést örökölt helyszíni webhely tooa új felhőalapú Azure-ban üzemeltetett. Hello örökölt helyhez érdemes toouse hello kezdőlap URI toomonitor állapota. De hello új felhőalapú verzióhoz tartozó, egy egyéni figyelési lapjának webkiszolgálókból (elérési út "/ monitor.aspx"), amely tartalmazza a további ellenőrzéseket.

![Traffic Manager-végpont figyelése (alapértelmezés)][9]

hello figyelési beállítások Traffic Manager-profil tooall végpontok egyetlen profilon belül érvényesek. A beágyazott profilok hely toodefine különböző figyelési beállításokat egy másik alárendelt profil használni.

![Figyelés végpont beállítások a TRAFFIC Manager-végpontot][10]

## <a name="next-steps"></a>Következő lépések

További információ [Traffic Manager-profilok](traffic-manager-overview.md)

Ismerje meg, hogyan túl[Traffic Manager-profil létrehozása](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png
