---
title: "aaaDynamic hely gyorsítás Azure CDN használatával"
description: "Dinamikus gyorsítás részletes bemutatója"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: 37e6312ae5e83448f2d87c95ef48c387355748bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Az Azure CDN használatával dinamikus gyorsulás

Közösségi, elektronikus kereskedelmi és hello hyper személyre szabott webes hello felbontására gyorsan növekszik százalékaként tartalom hello kiszolgált tooend felhasználók valós idejű jönnek létre. A felhasználók gyors, megbízható és személyre szabott webes feladatait, a böngésző, helyről, eszközről vagy hálózati független várnak el. Azonban hello nagyon innovációinak, amelyek a feladatait, amelyek révén is lap letöltések lassú, és veszélynek hello minőséget hello fogyasztói. 

Standard CDN-t a képességhez hozzátartoznak az hello képességét toocache fájlok szorosabb tooend felhasználók toospeed kézbesítési statikus fájlok mentése. Azonban a dinamikus webes alkalmazásokhoz, a gyorsítótár helye tartalmat nem lehet hello server hello tartalom válasz toouser viselkedés állít elő, mert. Felgyorsítható a tartalom kézbesítésével hello bonyolultabb, mint a hagyományos peremhálózati gyorsítótár, és egy végpont megoldás, amely a beállítás a minden elem mellett hello teljes elérési útja a kezdete toodelivery igényel. Az Azure CDN dinamikus hely Acceleration (DSA), hello dinamikus tartalmú weblapok kimutathatóan javul a teljesítmény.

Akamai és Verizon Azure CDN kínál DSA optimalizálási keresztül hello **optimalizálva** menü végpont létrehozása során.

## <a name="configuring-cdn-endpoint-tooaccelerate-delivery-of-dynamic-files"></a>CDN végpont tooaccelerate kézbesítési dinamikus fájlok konfigurálása

A CDN-végpont toooptimize az dinamikus fájlok Azure-portálon kézbesítését hello kiválasztásával konfigurálhatja **dinamikus acceleration** hello csoportban **optimalizálva** során tulajdonság kiválasztása hello végpont létrehozásához. Használhatja a REST API-k vagy programozott módon hello hello ügyfél SDK-k toodo bármelyikét ugyanaz. 

### <a name="probe-path"></a>Mintavételi elérési útja
Mintavételi elérési út egy szolgáltatás adott tooDynamic hely gyorsítás, és érvénytelen a létrehozásához szükséges. DSA használja kis *mintavételi elérési* fájl hello származási toooptimize hálózati útválasztási konfigurációi hello CDN helyezve. Töltse le és töltse fel a minta fájl tooyour helyet, vagy egy meglévő eszközt használja a forrás, amely nagyjából 10 Tudásbázis hello mintavételi elérési helyette hello eszköz létezésének.

> [!Note]
> DSA költségeket terhel extra. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cdn/) további információt.

a következő képernyőképeket hello hello folyamatot Azure-portálon szemlélteti.
 
![Új CDN-végpont hozzáadása](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*1. ábra: Új CDN-végpont hozzáadása a hello CDN-profil*
 
![DSA új CDN-végpont létrehozása](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*2. ábra: A CDN-végpont létrehozása dinamikus gyorsítás kijelölt optimalizálása*

Hello CDN-végpont létrehozása után hello DSA optimalizálásokat feltételeknek megfelelő összes fájl vonatkozik. a következő szakasz hello DSA optimalizálási részletesen ismerteti.

## <a name="dsa-optimization-using-azure-cdn"></a>Azure CDN használatával DSA optimalizálása

Azure CDN szolgáltatás használata a dinamikus hely gyorsítás felgyorsítja a kézbesítési dinamikus eszközök a következő módszerek hello használata:

-   Útvonal optimalizálása
-   TCP optimalizálás.
-   Objektum előzetes betöltési (csak Akamai)
-   Mobil kép tömörítési (csak Akamai)

### <a name="route-optimization"></a>Útvonal optimalizálása

Útvonal optimalizálási fontos, mert hello Internet a dinamikus hely, ahol forgalmat, és átmenetileg leállások folyamatosan változnak hello hálózati topológia. Border Gateway Protocol (BGP) hello hello útválasztási protokoll, az hello Internet, de előfordulhat, hogy gyorsabb útvonalak keresztül közbülső jelenléti pontra (PoP) kiszolgálók. 

Útvonal optimalizálási úgy dönt, hello legoptimálisabb elérési toohello forrása, hogy a hely folyamatosan elérhető és a dinamikus tartalmat a rendszer tooend felhasználókat hello leggyorsabb és legmegbízhatóbb útvonal lehet. 

hello Akamai hálózati technikák toocollect valós idejű adatokat használ, és hasonlítsa össze a hello Akamai kiszolgáló, valamint hello alapértelmezett BGP-útvonal csomópontjai különböző útvonalai hello nyissa meg az Internet toodetermine hello leggyorsabb és közötti útvonal hello származási hello között CDN peremhálózati. Ezek a technológiák elkerülése Internet torlódás pontok és a hosszú útvonalakat. 

Ehhez hasonlóan hello Verizon hálózat által használt nem egyedi DNS, nagy kapacitású kombinációja támogatja a POP és állapot-ellenőrzést, toodetermine hello legjobb átjárók toobest útvonal adatait hello ügyfél toohello forrása.
 
Ennek eredményeképpen teljesen dinamikus és tranzakciós tartalom céleszközökre gyorsabb és megbízhatóbb tooend felhasználók, akkor is, amikor az uncacheable. 

### <a name="tcp-optimizations"></a>TCP optimalizálás.

Transmission Control Protocol (TCP) a hello Internet protocol suite hello standard használt toodeliver információt az IP-hálózat az alkalmazások között.  Alapértelmezés szerint számos biztonsági és oda-kérelmek szükséges tooset TCP-kapcsolatot, valamint a hatékonyság hiánya léptékű eredményez korlátok tooavoid hálózati congestions fel. Ez a probléma három területeken optimalizálásával Akamai Azure CDN foglalkozik: 

 - Távolítsa el az indexkulcsból lassú indítása
 - Állandó kapcsolatot kihasználva
 - TCP-csomag paraméter (csak Akamai) hangolása

#### <a name="eliminating-slow-start"></a>Távolítsa el az indexkulcsból lassú indítása

*Kezdő lassú* hello TCP protokoll, amely megakadályozza, hogy a hálózati torlódás hello hálózaton keresztül küldött adatok mennyiségének hello korlátozásával része. Kezdődik ki a küldő és fogadó között kis torlódás méretek mindaddig, amíg eléri maximális hello vagy csomagvesztés észlel.

Akamai és Verizon Azure CDN három lépésben lassú start megoldással:

1.  Akamai és a Verizon tartozó hálózati állapotát és figyelési toomeasure hello sávszélesség peremhálózati PoP kiszolgálók közötti kapcsolatok sávszélesség használata.
2. hello metrikák megosztott peremhálózati PoP kiszolgálók között, hogy kiszolgálónként hello hálózati feltételek mellett tisztában, és a kiszolgáló állapotának hello kapcsolódási felhasználókat ezekbe a csoportokba.  
3. hello CDN peremhálózati kiszolgálóinak is képes toomake feltételezéseket néhány átviteli paraméterek, például milyen hello optimális ablakméret kell lennie a közelségi kapcsolat él CDN kiszolgálóival való kommunikáció során. Ez a lépés azt jelenti, hogy hello kezdeti torlódás ablakméret növelhető, ha hello kapcsolatot az hello CDN peremhálózati kiszolgálóinak hello állapotának magasabb packet adatátvitel képes.  

#### <a name="leveraging-persistent-connections"></a>Állandó kapcsolatot kihasználva

A CDN-t használ, kevesebb egyedi gépek csatlakozni tooyour eredeti kiszolgálóra tooyour származási közvetlenül csatlakozó felhasználók közvetlenül képest. Akamai és Verizon Azure CDN is készletbe felhasználói kérelmek együtt tooestablish hello származási kevesebb kapcsolatok.

A korábban említett TCP-kapcsolatok a hálózatról több kérést oda-vissza a kézfogás tooestablish új kapcsolatot. Állandó kapcsolat, más néven "HTTP életben tartási," használja fel meglévő TCP-kapcsolatok a HTTP kérelmeket toosave körbejárási többször, és kézbesítési felgyorsításában. 

hello Verizon hálózati is rendszeres életben tartási csomagok küldi hello TCP-kapcsolati tooprevent a lezárási folyamatban nyitott kapcsolat.

#### <a name="tuning-tcp-packet-parameters"></a>TCP-csomag paraméter hangolása

Akamai Azure CDN is beállítja a kiszolgálók kapcsolatok hello paraméterek, és csökkenti hello távolsági round szükséges utazgatással tooretrieve tartalom beágyazott hello hely a következő módszerek hello segítségével:

1.  Hello kezdeti torlódás ablak növelését, így további csomagokat is küldhetők nyugtázást várakozás nélkül.
2.  Csökkenő hello kezdeti újraküldési időkorlát, hogy a rendszer észlelt egy adatvesztés, és újraküldési gyorsabban következik be.
3.  Csökkenő hello minimum és maximum újból elküldenek időtúllépés tooreduce hello várakozási időt feltételezve csomagok elvesztek a továbbítás előtt.

### <a name="object-prefetch-akamai-only"></a>Objektum előzetes betöltési (csak Akamai)

A legtöbb webhelyek tartalmazzák a HTML-lapot, amely különböző más erőforrások, például képek és parancsfájlok hivatkozik. Általában amikor egy ügyfél egy weblap, hello böngésző először letölti és elemez hello HTML-objektumot, és majd miatt toolinked eszközök, amelyek a szükséges toofully betöltése hello lap további kérelmeket. 

*Előzetes betöltés* a technika tooretrieve képek és parancsfájlok ágyazott hello HTML-weblap hello HTML kiszolgált toohello böngészőt, és hello előtt böngésző még akkor is lehetővé teszi a objektum kérelmeket. 

A hello **előzetesen lehívott lapok** beállítást, ha a CDN szolgál hello HTML kiinduló lap toohello ügyfél böngészője, hello hello CDN hello időpontban bekapcsolva értelmezi hello HTML-fájlt, és további erőforrásokra vonatkozó kérelmeket a csatolt és gyorsítótárában tárolja. Hello ügyfél küld hello kérelmek hello kapcsolódó eszközök, a hello CDN biztonsági kiszolgáló már rendelkezik a kért objektumok hello, és a képes kiszolgálni azokat közvetlenül egy oda-vissza toohello származási nélkül. Az optimalizálás előnyökkel jár a tartalmat kérelmeznék és a nem gyorsítótárazható.

### <a name="adaptive-image-compression-akamai-only"></a>Adaptív kép tömörítési (csak Akamai)

Egyes eszközök, különösen a mobil is, felhasználói élmény idő tootime a lassabb hálózati átviteli sebességek esetén. Ezekben az esetekben célszerű több hello felhasználói tooreceive kisebb a képekhez a weblap gyorsabban ahelyett, hogy a teljes megoldás képek hosszú ideig vár.

Ez a szolgáltatás automatikusan figyeli a hálózatban, és alkalmazta JPEG-tömörítési szabványos módszerek a lassabb tooimprove kézbesítési időhöz hálózati sebességek esetén.

Adaptív kép tömörítés | Fájlkiterjesztések  
--- | ---  
JPEG-tömörítés | .jpg, .jpeg, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Gyorsítótárazás

A DSA gyorsítótárazás ki van kapcsolva hello CDN, alapértelmezés szerint akkor is, ha hello származási fejlécek cache-control/lejár hello válaszként. Ez az alapértelmezett ki van kapcsolva, mert DSA jellemzően a kell nem gyorsítótárazható, mert azok egyedi tooeach ügyfél dinamikus eszközök, és bekapcsolja a gyorsítótárazás alapértelmezés szerint ez a viselkedés érvénytelenné.

Rendelkezik statikus és dinamikus eszközök vegyesen webhellyel, esetén ajánlott tootake egy hibrid megközelítés tooget hello legjobb teljesítmény érdekében. 

ADN használatakor az Verizon Premium kapcsolja be újra be szabálymotor hello segítségével bizonyos esetekben a gyorsítótárazás.  

A másik lehetőség az toouse két CDN-végpontokat. DSA toodeliver dinamikus eszközök másikat, és egy másik végpont ezzel a statikus optimalizálása írja be, például az általános webes kézbesítési toodelivery kérelmeznék eszközök. Rendelés tooaccomplish ezen alternatív a weblap URL-címek toolink fog módosítani, közvetlenül a toohello eszköz hello toouse tervezi a CDN-végponthoz. 

Például: `mydynamic.azureedge.net/index.html` egy dinamikus oldalra, és be töltve a hello DSA végpont.  a betöltött képek statikus CDN-végpont, például a hello vagy hello html-weblap hivatkozik több statikus erőforrásokat, például a JavaScript szalagtárak `mystatic.azureedge.net/banner.jpg` és `mystatic.azureedge.net/scripts.js`. 

Egy példa található [Itt](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) hogyan toouse tartományvezérlőinek az ASP.NET webes alkalmazás tooserve tartalmat adott CDN URL-a.




