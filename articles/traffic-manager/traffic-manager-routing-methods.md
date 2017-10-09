---
title: "a Traffic Manager - aaaAzure forgalom-útválasztási módszerei |} Microsoft Docs"
description: "Ez a cikk, valamit megismerheti hello különböző forgalom-útválasztási módszerekkel Traffic Manager által használt"
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: kumud
ms.openlocfilehash: b3eeca63ab5f2b9cd4a3a6b6a8fd3e40059e32b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager útválasztási módszerek

Az Azure Traffic Manager által támogatott négy forgalom-útválasztási módszert toodetermine hogyan tooroute hálózati forgalom toohello különböző végpontok. A TRAFFIC Manager hello forgalom-útválasztási módszer tooeach DNS-lekérdezést kap vonatkozik. hello forgalom-útválasztási módszer határozza meg, melyik végponthoz visszaadott hello DNS-válaszban.

Nincsenek elérhető a Traffic Manager négy forgalom-útválasztási módszerekkel:

* **[Prioritás](#priority):** válasszon **prioritás** amikor elsődleges szolgáltatásvégpont toouse szeretné, hogy minden forgalom, és adja meg a biztonsági mentések hello elsődleges vagy a biztonsági mentési hello végpontok esetén nem érhető el.
* **[Weighted](#weighted):** válasszon **Weighted** kívánt toodistribute forgalmat olyan készlete, végpontok közötti vagy egyenletesen vagy függően tooweights, amelyet kell megadni.
* **[Teljesítmény](#performance):** válasszon **teljesítmény** Ha végpontok különböző földrajzi helyeken található rendelkezik, és azt szeretné, hogy a végfelhasználók toouse hello "legközelebbi" végpont hello legkisebb hálózati késést tekintetében.
* **[Földrajzi](#geographic):** válasszon **földrajzi** , hogy a felhasználók olyan irányított toospecific végpontok (Azure, külső vagy beágyazott), mely földrajzi helye alapján, a DNS-lekérdezés származik. Ez lehetővé teszi a Traffic Manager ügyfelek tooenable forgatókönyvekben, ahol a felhasználó földrajzi régióban ismerete, és azokat, amelyek alapján útválasztási fontos. Például megfelel az adatok közös joghatóság alá megbízás honosítási tartalom & felhasználói felület és a különböző régiókban érkező forgalmat méri.

Traffic Manager-profilokat tartalmazza, végpont állapotát és automatikus végpont feladatátvételi figyelését. További információkért lásd: [Traffic Manager-végpont figyelés](traffic-manager-monitoring.md). Egyetlen Traffic Manager-profil csak egyetlen forgalom-útválasztási módszert használhatja. A profil bármikor választhat egy másik forgalom-útválasztási módszert. A módosításai érvényesek lesznek egy percen belül, és az állásidő nélkül van szükség. Forgalom-útválasztási módszer kombinálható is beágyazott Traffic Manager-profilok használatával. A beágyazási lehetővé teszi, hogy a kifinomult és rugalmas forgalom-útválasztási konfigurációk, amely nagyobb, összetett alkalmazások hello igényeinek. További információkért lásd: [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Prioritás forgalom-útválasztási módszer

Gyakran egy szervezet szeretne tooprovide megbízhatóság a hozzá tartozó szolgáltatások telepítésével egy vagy több biztonsági mentési szolgáltatást, abban az esetben, ha az elsődleges szolgáltatás leáll. hello "Priority" forgalom-útválasztási módszer lehetővé teszi, hogy az Azure ügyfelek tooeasily valósítja meg a feladatátvételi mintát.

![Az Azure Traffic Manager "Priority" forgalom-útválasztási módszer][1]

Traffic Manager-profil hello Szolgáltatásvégpontok rangsorolt listáját tartalmazza. Alapértelmezés szerint a Traffic Manager küld minden forgalom toohello elsődleges (legnagyobb prioritás) végpont. Hello elsődleges végpont nem érhető el, ha a Traffic Manager útvonalak hello forgalom toohello második végpontnak. Ha mindkét hello elsődleges és másodlagos végpont nem érhetők el, a hello forgalom halad toohello, harmadik, és így tovább. Hello végpont rendelkezésre állását konfigurált hello állapota (engedélyezett vagy tiltott) azon alapul, és folyamatban lévő végpontmonitoring kijelző hello.

### <a name="configuring-endpoints"></a>Végpontok konfigurálása

Az Azure Resource Manager konfigurálása hello végpontprioritás explicit módon tulajdonsággal hello "priority" végpontok. Ez a tulajdonság értéke 1 és 1000 között. Alacsonyabb értékeket egy nagyobb prioritást jelölnek. Nem lehetnek közös prioritási értékek. Hello tulajdonság beállítása nem kötelező megadni. Ha nincs megadva, alapértelmezett prioritást hello végpont sorrendnek használja.

##<a name = "weighted"></a>Súlyozott forgalom-útválasztási módszer
hello "Weighted" forgalom-útválasztási módszer lehetővé teszi toodistribute forgalom egyenletesen vagy toouse egy előre meghatározott értékek súlyozását.

![Az Azure Traffic Manager az "Súlyozott" forgalom-útválasztási módszer][2]

Hello súlyozott forgalom-útválasztási módszert rendeljen hozzá egy súly tooeach végpont hello Traffic Manager-profil konfiguráció. hello súlya 1 too1000 hónapjával. Ez a paraméter nem kötelező megadni. Ha nincs megadva, a forgalom kezelők "1" alapértelmezett súlyt használja.

Minden egyes megadott DNS-lekérdezés érkezett Traffic Manager véletlenszerűen választ egy elérhető végpontot. hello valószínűség kiválasztása a végpont tooall számára elérhető végpontok hozzárendelt hello súlyok alapul. Azonos súlyozási hello segítségével is forgalomeloszlás eredményez az összes végpontok között. A meghatározott végpontokhoz magasabb vagy alacsonyabb súlyok használatával hatására az adott vissza ritkább vagy gyakoribb hello DNS-válaszok végpontok toobe.

súlyozott hello módszer lehetővé teszi, hogy néhány hasznos forgatókönyv:

* Az alkalmazásfrissítés fokozatos: forgalom tooroute tooa új végpont százalékát, és fokozatosan hello forgalmat is növekszik adott idő too100 %.
* Alkalmazás áttelepítési tooAzure:-profil létrehozása az Azure és a külső végpontok száma. Beállíthatja a hello végpontok tooprefer hello új végpontok hello súlyt.
* Felhőalapú Teljesítménynövelés a további kapacitás-érdekében: gyorsan bontsa ki a Traffic Manager-profil mögött helyezésével egy helyi központi hello felhőbe. Ha hello felhő plusz kapacitást, hozzáadása vagy további végpontok engedélyezése, és adja meg, milyen forgalom részét tooeach végpont kerül.

hello Azure Resource Manager portal hello konfigurálása súlyozott a forgalom útválasztásához támogatja.  Konfigurálhatja az Azure PowerShell, a parancssori felület és a REST API-k hello hello erőforrás-kezelő verzióival súlyt.

Fontos fontos toounderstand, hogy a DNS-válaszok gyorsítótárba kerüljenek-e az ügyfelek által, és hello rekurzív DNS-kiszolgálók, ügyfelek hello tooresolve DNS-neveket használja. A gyorsítótárazás súlyozott forgalom terjesztéseket hatással lehetnek. Ha az ügyfelek és a rekurzív DNS-kiszolgálók hello száma túl nagy, forgalomeloszlás megfelelően működik-e. Azonban ha hello ügyfelek vagy a rekurzív DNS-kiszolgálók száma alacsony, gyorsítótárazás is jelentősen eltolódhat hello forgalomeloszlás.

Gyakori használati esetek a következők:

* Fejlesztési és tesztelési környezetek
* Alkalmazások kommunikáció
* Alkalmazások célzó egy keskeny felhasználói-talál, amelyek egy közös rekurzív DNS-infrastruktúra (például egy proxyn keresztül történő kapcsolódás vállalati alkalmazottak)

A DNS-gyorsítótár hatások azok közös tooall DNS-alapú forgalom útválasztási rendszerek, nem csak az Azure Traffic Manager. Néhány esetben explicit módon a DNS-gyorsítótár hello törlésével által biztosított megoldás. Más esetekben alternatív forgalom-útválasztási módszert lehet több.

## <a name = "performance"></a>Teljesítmény forgalom-útválasztási módszer

Üzembe helyezése a végpontok két vagy több helyen teljes hello földgömb javíthatja a forgalom toohello útválasztási helytől, amely "legközelebbi" tooyou sok alkalmazások válaszképességének hello. hello "Teljesítmény" forgalom-útválasztási módszer biztosítja ezt a lehetőséget.

![Az Azure Traffic Manager "Teljesítmény" forgalom-útválasztási módszer][3]

hello "legközelebbi" végpont nincs feltétlenül legközelebbi földrajzi távolság mérve. Ehelyett hello "Teljesítmény" forgalom-útválasztási módszer hálózati késés mérésével hello legközelebbi végpont határozza meg. A TRAFFIC Manager fenntartja az Internet késés tábla tootrack hello körbejárási között eltelt idő IP-címtartományok és minden Azure-adatközpontban.

A TRAFFIC Manager hello bejövő DNS-kérelem a hello Internet késés tábla hello forrás IP-címét keresi. A TRAFFIC Manager úgy dönt, elérhető a végpont a hello Azure-adatközpontban, amely a legkisebb mértékű késleltetést hello az adott IP-címtartomány, akkor az adott végpontra hello DNS-választ ad vissza.

A [Traffic Manager működése](traffic-manager-overview.md#how-traffic-manager-works), a Traffic Manager nem kapja meg DNS-lekérdezések közvetlenül az ügyfelektől. Ahelyett, hogy DNS-lekérdezések határozza meg, hogy hello az ügyfelek kapcsolódnak-e a konfigurált toouse hello rekurzív DNS-szolgáltatás. Ezért hello IP-cím toodetermine hello "legközelebbi" végponton nincs hello ügyfél IP-címet, de hello IP-cím hello rekurzív DNS-szolgáltatás. A gyakorlatban az IP-cím helyes proxyjaként való hello ügyfél.


A TRAFFIC Manager rendszeresen frissítéseket hello Internet késés tábla tooaccount a változásokat a globális internetes és az új Azure-régiók hello. Azonban az alkalmazások teljesítményének függ a valós idejű változások a terhelési hello interneten keresztül. Teljesítmény forgalom-útválasztás nem figyeli egy adott szolgáltatás végpontjának terhelése. Azonban a végpont nem érhető el, ha a Traffic Manager foglalja a DNS-válaszokban.


Pontok toonote:

* Ha a profilban található több végpontok hello azonos Azure-régiót, majd a Traffic Manager elosztása forgalom egyenletesen hello számára elérhető végpontok az adott régióban. Régión belül egy másik forgalomeloszlás tetszés szerint használhatja [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md).
* Ha minden beállítás engedélyezett legközelebbi hello végpontok Azure-régiót csökkent, a Traffic Manager forgalom toohello végpontok helyez hello következő legközelebbi Azure-régiót. Ha azt szeretné, hogy egy előnyben részesített feladatátvételi feladatütemezési toodefine, [Traffic Manager-profilok beágyazott](traffic-manager-nested-profiles.md).
* Külső végpontok száma vagy beágyazott végpontok hello teljesítmény forgalom-útválasztási módszert használ, amikor szüksége ezekre a végpontokra toospecify hello helyét. Válassza ki a hello Azure-régiót legközelebbi tooyour központi telepítés. Azokon a helyeken értékeket hello hello Internet késés tábla által támogatott.
* hello algoritmus, amely kiválasztja hello végpont determinisztikus. Ismétlődik a DNS-lekérdezéseket a hello ugyanazt az ügyfelet vannak irányítva toohello azonos végpont. Általában a ügyfelek különböző rekurzív DNS-kiszolgálók használata utazás közben. hello ügyfél irányított tooa másik végpont lehet. Útválasztás is is által igényelt frissítések toohello Internet késés tábla. Ezért a forgalom-útválasztási módszer nem garantálható, hogy egy ügyfél mindig teljesítmény hello irányított toohello azonos végpont.
* Hello Internet késés tábla megváltozásakor Észreveheti, hogy az egyes ügyfelek legyenek irányított tooa másik végpont. Az útválasztási módosítása pontosabb aktuális késési adatok alapján. Ezek a frissítések alapvető toomaintain hello pontosságát teljesítmény forgalom-útválasztási hello Internet folyamatosan fejlődik, mint.

## <a name = "geographic"></a>Földrajzi forgalom-útválasztási módszer

Traffic Manager-profilokat lehet konfigurált toouse hello útválasztási módszerrel, hogy a felhasználók a rendszer átirányítja a mely földrajzi helye alapján toospecific végpontok (Azure, külső vagy beágyazott) a DNS-lekérdezés Geographic származik. Ez lehetővé teszi a Traffic Manager ügyfelek tooenable forgatókönyvekben, ahol a felhasználó földrajzi régióban ismerete, és azokat, amelyek alapján útválasztási fontos. Például megfelel az adatok közös joghatóság alá megbízás honosítási tartalom & felhasználói felület és a különböző régiókban érkező forgalmat méri.
Ha egy profil földrajzi útválasztásra van konfigurálva, mindegyik végpont társított, hogy a profil rendelt földrajzi régiókhoz tooit készlete toohave kell. A földrajzi régió következő részletességi szinteken is lehet. 
- A globális – bármely régió
- Regionális csoportosítás – például Afrika Közel-Kelet, Ausztrália/csendes-óceáni stb. 
- Ország vagy régió – például Írországban Perui, Hongkong, KKT stb. 
- Az állam/megye – például az USA-kaliforniai, Ausztrália-Queensland, Kanada-Alberta stb. (Megjegyzés: a részletesség szintje csak állapotok támogatott / tartományok Ausztrália, Kanada, UK és USA).

A régió vagy egy régiók tooan végpont van hozzárendelve, adott helyre érkező kérelmeket irányított csak toothat végpont kap. A TRAFFIC Manager IP-forráscím hello hello DNS lekérdezés toodetermine hello régió, amelyből a kérdezi le, a felhasználó – általában ez a hello hello lekérdezés hello felhasználó nevében módon helyi DNS-feloldási hello IP-címét használja.  

![Az Azure Traffic Manager az "Földrajzi" forgalom-útválasztási módszer](./media/traffic-manager-routing-methods/geographic.png)

A TRAFFIC Manager hello forrás IP-címe hello DNS-lekérdezés olvas, és úgy dönt, mely földrajzi régióban származó van. Ha van olyan végponttal, amely a földrajzi régióban leképezve tooit rendelkezik majd jelek toosee. Ez a keresés hello legalacsonyabb szintű részletesség (az állam/megye ahol akkor támogatott, ellenkező esetben hello ország vagy régió szinten) elindul és toohello legmagasabb szinttel, amely minden hello módon kerül **globális**. hello első egyezés hello végpont tooreturn hello lekérdezés válaszként a átjárás használatával van kijelölve. Ha egy beágyazott típus végponttal, a gyermek-profilon belül a végpont megfelelő adja vissza, az útválasztási módszer alapján. a következő pontok hello alkalmazható toothis viselkedés:

- A földrajzi régió lehet csatlakoztatott csak tooone végpont a Traffic Manager-profil földrajzi útválasztási hello útválasztási típus esetén. Ez biztosítja, hogy a felhasználók útválasztási determinisztikus, és ügyfelek forgatókönyv esetén van szükség a egyértelműen földrajzi határok használatával engedélyezheti.
- Ha egy felhasználó régió két különböző végpontokhoz földrajzi leképezés, a Traffic Manager hello végpont kiválasztása hello legalacsonyabb lépésköz, és nem veszi figyelembe, hogy régió toohello útválasztási kérelmeinek másik végpontot. Vegye figyelembe például két végpont - Endpoint1 és Endpoint2 földrajzi útválasztási típus egy profilt. Endpoint1 van konfigurálva Írországban és Endpoint2 tooreceive forgalmát van konfigurálva Európa tooreceive forgalmát. A kérelem Írországban származik, esetén mindig irányított tooEndpoint1.
- Egy csatlakoztatott csak tooone végpont lehet, mert a Traffic Manager visszaadja függetlenül attól, hogy hello végpont kifogástalan-e.

    >[!IMPORTANT]
    >Erősen ajánlott, hogy az ügyfelek hello földrajzi útválasztási módszerrel társíthatja hello beágyazott típus végpontok belül legalább két végpontot tartalmazó gyermek profillal.
- Ha egy végpont egyezést talál, és adott végpontra a hello **leállítva** állapotba kerül, a Traffic Manager NODATA választ ad. Ebben az esetben nincs további keresések legyen feljebb hello földrajzi régióban hierarchiában. Ez a viselkedés akkor is beágyazott végpont esetében alkalmazható, ha hello gyermek profil van hello **leállítva** vagy **letiltott** állapotát.
- Ha egy olyan végpont jelenít meg egy **letiltott** állapota, nem fog szerepelni folyamat megfelelő hello régióban. Ez a viselkedés akkor is beágyazott végpont esetében alkalmazható, ha hello végpont van hello **letiltott** állapotát.
- Ha egy lekérdezést a földrajzi régió, amely nem rendelkezik az adott profilhoz érkezik, a Traffic Manager NODATA választ ad. Ezért erősen ajánlott, hogy az ügyfelek használja-e a földrajzi útválasztási egy végponttal, ideális típusú beágyazott hello gyermek-profilt, hello régió belül legalább két végpontot **globális** tooit rendelve. Ez biztosítja azt is, hogy bármilyen IP-címek, amelyek nem felelnek meg a tooa régió kezeli.

A [Traffic Manager működése](traffic-manager-how-traffic-manager-works.md), a Traffic Manager nem kapja meg DNS-lekérdezések közvetlenül az ügyfelektől. Ahelyett, hogy DNS-lekérdezések határozza meg, hogy hello az ügyfelek kapcsolódnak-e a konfigurált toouse hello rekurzív DNS-szolgáltatás. Hello IP cím használt toodetermine hello terület ezért nem hello ügyfél IP-címet, de hello IP-cím hello rekurzív DNS-szolgáltatás. A gyakorlatban az IP-cím helyes proxyjaként való hello ügyfél.


## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan toodevelop magas rendelkezésre állású alkalmazások [Traffic Manager-végpont figyelése](traffic-manager-monitoring.md)

Ismerje meg, hogyan túl[Traffic Manager-profil létrehozása](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



