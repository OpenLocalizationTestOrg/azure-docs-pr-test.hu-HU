---
title: "aaaAzure ExpressRoute – gyakori kérdések |} Microsoft Docs"
description: "ExpressRoute – gyakori kérdések hello támogatott Azure-szolgáltatások, költség, adatokat és kapcsolatok, SLA-t, szolgáltatókat és helyek, sávszélesség és további technikai részleteket adatait tartalmazza."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 09b17bc4-d0b3-4ab0-8c14-eed730e1446e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: c01e83f1497103e2fa85251dce6fb41844e46e9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-faq"></a>ExpressRoute – Gyakori kérdések

## <a name="what-is-expressroute"></a>Mi az az ExpressRoute?

ExpressRoute az Azure-szolgáltatások, amely lehetővé teszi a Microsoft adatközpontjaiban és infrastruktúra, amely a helyszínen vagy a közös elhelyezést intézményben közötti magánhálózati kapcsolatok létrehozása. ExpressRoute kapcsolatok ne lépje át hello nyilvános Internet és ajánlat magasabb biztonsági, megbízhatósági és sebesség késéseket okoz, mint a szokásos kapcsolatok alacsonyabb hello interneten keresztül.

### <a name="what-are-hello-benefits-of-using-expressroute-and-private-network-connections"></a>Mik azok a ExpressRoute- és magánhálózati kapcsolatok használatának előnyei egy hello?

Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. Magasabb szintű biztonságra, megbízhatóságra és sebesség, mint hello interneten keresztüli tipikus kapcsolatok alacsonyabb és egységes késések biztosítanak. Bizonyos esetekben ExpressRoute kapcsolatok tootransfer adatok között a helyszíni eszközök és Azure partíciónként akár jelentős költségelőnyökhöz juthat.

### <a name="where-is-hello-service-available"></a>Ahol lehetőség van a hello szolgáltatást?

Ezen a lapon, a szolgáltatás helyét és a rendelkezésre állási lásd: [ExpressRoute partnerek és a helyek](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-tooconnect-toomicrosoft-if-i-dont-have-partnerships-with-one-of-hello-expressroute-carrier-partners"></a>Hogyan használhatom ExpressRoute tooconnect tooMicrosoft, ha nincs partnerségei hello ExpressRoute-szolgáltatója partnerek egyike?

Válassza ki a regionális szolgáltatónként, és Ethernet kapcsolatok tooone hello támogatott Exchange-szolgáltató helyei megnyílik. A Microsoft hello szolgáltató helyen majd partnert. Ellenőrizze a hello utolsó szakasza [ExpressRoute partnerek és a helyek](expressroute-locations.md) toosee, ha a szolgáltató hello exchange helyeken található. Egy ExpressRoute-kapcsolatcsoportot keresztül hello szolgáltatás szolgáltató tooconnect tooAzure majd rendezheti.

### <a name="how-much-does-expressroute-cost"></a>Mennyibe ExpressRoute költség?

Ellenőrizze [díjszabása](https://azure.microsoft.com/pricing/details/expressroute/) díjszabási információkat.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-hello-vpn-connection-i-purchase-from-my-network-service-provider-have-toobe-hello-same-speed"></a>Ha az adott sávszélesség, ExpressRoute-kapcsolatcsoportot hello VPN-kapcsolatot a hálózati szolgáltató vásárlása kell fizetni toobe rendelkezik hello azonos sebességű?

Nem. A szolgáltató minden sebességű VPN-kapcsolat lehet vásárolni. A kapcsolat tooAzure azonban korlátozott toohello ExpressRoute kapcsolatcsoport sávszélessége megvásárolt.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-hello-ability-tooburst-up-toohigher-speeds-if-necessary"></a>Ha kell fizetni az adott sávszélesség ExpressRoute-kapcsolatcsoportot, kell hello képességét tooburst mentése toohigher sebességű szükség?

Igen. ExpressRoute-Kapcsolatcsoportok tooburst tootwo alkalommal be hello sávszélességre vonatkozó korlátját, akkor az nem jelent többletköltséget beszerzett konfigurált tooallow. Ez a funkció támogatja-e, kérje meg a service provider toosee.

### <a name="can-i-use-hello-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Használhatok hello azonos titkos hálózati kapcsolat a virtuális hálózati és más Azure-szolgáltatásokkal egyidejűleg?

Igen. ExpressRoute-kapcsolatcsoportot, beállítása után, lehetővé teszi az tooaccess szolgáltatások a virtuális hálózaton belül és más Azure-szolgáltatásokkal egyidejűleg. Hello nyilvános társviszony-létesítési útvonalon keresztül hello magánhálózati társviszony-létesítési elérési utat, és tooother szolgáltatások keresztül kapcsolatba toovirtual hálózatok.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>Nem ExpressRoute kínálnak a szolgáltatási szint szerződés (SLA)?

További információ: hello [ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) lap.

## <a name="supported-services"></a>Támogatott szolgáltatások

Támogatja az ExpressRoute [három útválasztási tartományok](expressroute-circuit-peerings.md) különböző típusú szolgáltatásokhoz.

### <a name="private-peering"></a>Magánhálózati társviszony-létesítés

* Virtuális hálózatok, beleértve a virtuális gépek és a cloud services csomag

### <a name="public-peering"></a>Nyilvános társviszony-létesítés

* Power BI
* Dynamics 365 pénzügyi és műveletek (korábbi nevén Dynamics AX Online)
* A legtöbb hello Azure szolgáltatások hello néhány kivételtől eltekintve a következő esetében:
  * Tartalomkézbesítési hálózat (CDN)
  * A Visual Studio Team Services terhelés tesztelése
  * Multi-Factor Authentication
  * Traffic Manager

### <a name="microsoft-peering"></a>Microsoft társviszony-létesítés

* [Office 365](http://aka.ms/ExpressRouteOffice365)
* Dynamics 365 felhasználói Engagement-alkalmazást (korábbi nevén CRM Online-hoz)
  * Dynamics 365 az értékesítés
  * Dynamics 365 az ügyfélszolgálat
  * Dynamics 365 mező szolgáltatás
  * Dynamics 365 projekt szolgáltatás

## <a name="data-and-connections"></a>Adatok és kapcsolatok

### <a name="are-there-limits-on-hello-amount-of-data-that-i-can-transfer-using-expressroute"></a>Vannak-e hello adatmennyiséget ExpressRoute segítségével átvihetők vonatkozó korlátozások?

Jelenleg nem korlátozhatja hello adatforgalom mennyisége. Tekintse meg a túl[díjszabása](https://azure.microsoft.com/pricing/details/expressroute/) sávszélesség díjszabás olvashat.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Milyen kapcsolat sebessége ExpressRoute által támogatott?

Sávszélesség ajánlatok támogatja:

50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps

### <a name="which-service-providers-are-available"></a>Mely szolgáltatók állnak rendelkezésre?

Lásd: [ExpressRoute partnerek és a helyek](expressroute-locations.md) hello szolgáltatók és helyek listáját.

## <a name="technical-details"></a>Technikai részletek

### <a name="what-are-hello-technical-requirements-for-connecting-my-on-premises-location-tooazure"></a>Mik azok a helyszíni hely tooAzure kapcsolódás műszaki követelményeinek hello?

Lásd: [ExpressRoute Előfeltételek lapon](expressroute-prerequisites.md) követelményeket.

### <a name="are-connections-tooexpressroute-redundant"></a>Kapcsolatok tooExpressRoute redundáns vannak?

Igen. Minden egyes ExpressRoute-kapcsolatcsoportot konfigurált kapcsolatokat tooprovide magas rendelkezésre állású közötti redundáns két rendelkezik.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>I megszakad a kapcsolat, ha az egyik a ExpressRoute-hivatkozások nem?

Nem megszakad a kapcsolat egy a hello közötti kapcsolatok meghiúsul. Egy redundáns kapcsolat a hálózaton elérhető toosupport hello terhelését. Létrehozhat több Kapcsolatcsoportok emellett az egy másik társviszony-létesítési helye tooachieve meghibásodások rugalmas kiküszöbölése.

### <a name="onep2plink"></a>Ha nem vagyok a felhőalapú exchange a közös helyen, és a szolgáltató biztosít a pontok közötti kapcsolat, kell tooorder két fizikai kapcsolat a helyszíni hálózat és a Microsoft között?

A szolgáltató hozhatnak létre két virtuális Ethernet-kapcsolatok hello fizikai kapcsolaton keresztül, ha csak egyetlen fizikai kapcsolat van szükség. hello fizikai (például egy optikai szál) megszakad, a réteg 1 (1.) eszközön (lásd a hello kép). hello két Ethernet virtuális kapcsolatok vannak látva eltérő VLAN-azonosítót, egy hello elsődleges kör, és egy másodlagos hello a. Adott VLAN-azonosítók hello külső 802.1Q Ethernet-fejlécben vannak. belső 802.1Q hello Ethernet fejléc (nincs ábrázolva) az adott csatlakoztatott tooa [ExpressRoute útválasztási tartomány](expressroute-circuit-peerings.md).

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-tooazure-using-expressroute"></a>Ki lehet terjeszteni a VLAN-OK tooAzure ExpressRoute segítségével egyik?

Nem. Az Azure réteg 2 kapcsolat bővítmények nem támogatott.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Az előfizetésben is rendelkezem egynél több ExpressRoute-kapcsolatcsoportot?

Igen. Az előfizetés csak egy ExpressRoute-kapcsolatcsoportot lehet. alapértelmezett korlát hello too10 van beállítva. Felveheti a kapcsolatot a Microsoft Support tooincrease hello korlátot, ha szükséges.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Beállítható, hogy a különböző szolgáltatók ExpressRoute-Kapcsolatcsoportok?

Igen. ExpressRoute-Kapcsolatcsoportok sok szolgáltatókkal lehet. Minden egyes ExpressRoute-kapcsolatcsoportot rendelve egy szolgáltató. 

### <a name="can-i-have-multiple-expressroute-circuits-in-hello-same-location"></a>Beállítható, hogy több ExpressRoute-Kapcsolatcsoportok hello az ugyanazon a helyen?

Igen. Több ExpressRoute-Kapcsolatcsoportok rendelkezik, a hello azonos vagy különböző szolgáltatók a hello ugyanazon a helyen. Azonban, ezért nem csatolható több ExpressRoute-kapcsolatcsoport toohello azonos virtuális hálózati hello az azonos helyen.

### <a name="how-do-i-connect-my-virtual-networks-tooan-expressroute-circuit"></a>Csatlakozás a virtuális hálózatok tooan ExpressRoute-kapcsolatcsoportot

hello alapvető lépések a következők:

* ExpressRoute-kapcsolatcsoportot létrehozni, és az engedélyezéshez hello szolgáltató rendelkezik.
* Ön vagy a hello szolgáltató hello BGP társviszony-létesítés (s) kell konfigurálni.
* Virtuális hálózati toohello ExpressRoute-kapcsolatcsoportot hello hivatkozásra.

További információkért lásd: [kiépítésével és a kapcsolatcsoport állapotok az ExpressRoute-munkafolyamatokat](expressroute-workflows.md).

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Vannak-e kapcsolat határok saját ExpressRoute-kapcsolatcsoportot?

Igen. Hello [ExpressRoute partnerek és a helyek](expressroute-locations.md) cikk hello kapcsolat határok áttekintést nyújt az ExpressRoute-kapcsolatcsoportot. Az ExpressRoute-kapcsolatcsoportot kapcsolatra korlátozva tooa egyetlen geopolitikai régióban. Kapcsolat bővített toocross geopolitikai régiók lehet hello ExpressRoute prémium funkció engedélyezésével.

### <a name="can-i-link-toomore-than-one-virtual-network-tooan-expressroute-circuit"></a>Csatolhatja toomore, mint egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot?

Igen. Lehet too10 virtuális hálózatok kapcsolatok egy szabványos ExpressRoute körön, és too100 másolatot egy [prémium ExpressRoute-kapcsolatcsoportot](#expressroute-premium). 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-tooa-single-expressroute-circuit"></a>Van több Azure-előfizetések virtuális hálózatokat tartalmaznak. Kapcsolódhatnak a virtuális hálózatok, amelyek a külön előfizetések tooa egyetlen ExpressRoute-kapcsolatcsoportot?

Igen. Engedélyezheti too10 fel más Azure-előfizetések toouse egyetlen ExpressRoute-kapcsolatcsoportot. Ez a korlát növelhető hello ExpressRoute prémium funkció engedélyezése.

További információkért lásd: [ExpressRoute-kapcsolatcsoportot megosztása több előfizetésekhez](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-toohello-same-circuit-isolated-from-each-other"></a>Virtuális hálózatok ugyanazon áramkör elkülönített csatlakoztatott toohello vannak egymástól?

Nem. Az egy útválasztási perspektíva, a virtuális hálózatok csatolt toohello azonos ExpressRoute-kapcsolatcsoportot részét képező hello ugyanabban az útválasztási tartományba, és nem el különítve egymástól. Ha elkülönítési kell továbbítani, meg kell toocreate egy külön ExpressRoute-kapcsolatcsoportot.

### <a name="can-i-have-one-virtual-network-connected-toomore-than-one-expressroute-circuit"></a>Beállítható, hogy egy csatlakoztatott virtuális hálózati toomore, mint egy ExpressRoute-kapcsolatcsoportot?

Igen. Egyetlen virtuális hálózat mentése toofour ExpressRoute-Kapcsolatcsoportok társíthatja. Négy különböző keresztül lehetnek rendezve [helyek ExpressRoute](expressroute-locations.md).

### <a name="can-i-access-hello-internet-from-my-virtual-networks-connected-tooexpressroute-circuits"></a>Elérhető hello Internet a saját virtuális hálózatok csatlakoztatott tooExpressRoute Kapcsolatcsoportok?

Igen. Ha nincs közzététel az alapértelmezett útvonalak (0.0.0.0/0) vagy Internet útvonal előtagok hello BGP-kapcsolaton keresztül, a társított virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot toohello Internet lehet csatlakoztatni.

### <a name="can-i-block-internet-connectivity-toovirtual-networks-connected-tooexpressroute-circuits"></a>Letilthatom Internet kapcsolat toovirtual hálózatok csatlakoztatott tooExpressRoute Kapcsolatcsoportok?

Igen. Alapértelmezett útvonalak (0.0.0.0/0) tooblock toovirtual gépek összes internetes kapcsolatot a virtuális hálózaton belül hatályba, és minden forgalmat kimenő hello ExpressRoute-kapcsolatcsoportot keresztül hirdethető meg.

Alapértelmezett útvonalak forgalmaz hirdetményt, ha azt kényszerített forgalom tooservices nyilvános társviszony-létesítés (például az Azure storage és SQL-adatbázis) hátsó tooyour helyi keresztül érhető el. Hogy tooconfigure az útválasztók tooreturn forgalom tooAzure, hello nyilvános társviszony-létesítési elérési útja, illetve hello interneten keresztül.

### <a name="can-virtual-networks-linked-toohello-same-expressroute-circuit-talk-tooeach-other"></a>Virtuális hálózatok csatolt toohello is azonos ExpressRoute-kapcsolatcsoportot beszélgetés tooeach más?

Igen. Virtuális gépek üzembe helyezett virtuális hálózatok csatlakoztatott toohello azonos ExpressRoute-kapcsolatcsoportot kommunikálhatnak egymással.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Használható együtt ExpressRoute virtuális hálózatok hely-hely kapcsolatot?

Igen. ExpressRoute a pont-pont VPN egyszerre is használható.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-toouse-expressroute"></a>Áthelyezhető egy virtuális hálózatot a pont-pont / pont-hely konfigurációs toouse ExpressRoute?

Igen. Hogy toocreate egy ExpressRoute-átjárót a virtuális hálózaton belül. Egy kis állásidő hello folyamathoz tartozó van.

### <a name="why-is-there-a-public-ip-address-associated-with-hello-expressroute-gateway-on-a-virtual-network"></a>Miért van egy nyilvános IP-cím társított hello ExpressRoute-átjárót a virtuális hálózaton?

hello nyilvános IP-cím csak a belső felügyeletére használható. A nyilvános IP-cím nincs kitett toohello Internet, és nem a virtuális hálózat egy biztonsági kockázatot jelent.

### <a name="what-do-i-need-tooconnect-tooazure-storage-over-expressroute"></a>Mire van szükségem az tooconnect tooAzure tárolási ExpressRoute keresztül?

Egy ExpressRoute-kapcsolatcsoportot létrehozni és a nyilvános társviszony-létesítéshez útvonalak konfigurálnia kell.

### <a name="are-there-limits-on-hello-number-of-routes-i-can-advertise"></a>Vannak-e I hirdethető útvonalak hello száma vonatkozó korlátozások?

Igen. Fogadunk too4000 útvonal előtaglistát kezel a magánhálózati társviszony-létesítés és 200 minden nyilvános társviszony-létesítést és a Microsoft társviszony-létesítés. A too10 növelhető a magánhálózati társviszony-létesítés hello ExpressRoute prémium funkció engedélyezésével 000 útvonalait.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-hello-bgp-session"></a>Vannak-e IP-címtartományok I hello BGP keresztüli hirdethető korlátozásai?

Jelenleg nem fogadja el a nyilvános hello titkos előtagok (RFC1918) és a Microsoft társviszony-létesítési BGP-munkamenetet.

### <a name="what-happens-if-i-exceed-hello-bgp-limits"></a>Mi történik, ha I ennél nagyobb hello BGP korlátai?

A program eltávolítja a BGP-munkamenetek. Miután hello előtag száma nem éri hello korlátja el azok vissza kell állítani.

### <a name="what-is-hello-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Mi az az ExpressRoute BGP tartsa idő hello? Ez módosítható?

hello fenntartási ideje 180. hello életben tartási üzenetek küldése történik 60 másodpercenként. Ezek rögzített beállítások hello Microsoft oldalon, és nem módosítható. Lehetséges, hogy Ön tooconfigure más időzítők, és hello BGP munkamenet paraméterek ennek megfelelően egyezteti.

### <a name="after-i-advertise-hello-default-route-00000-toomy-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-tooi-fix-this"></a>Miután I hirdetési hello alapértelmezett útvonalat (0.0.0.0/0) toomy virtuális hálózatok, az Azure virtuális gépeken futó Windows nem aktiválható. Hogyan tooI elhárításához?

a lépéseket követve hello Azure ismeri fel a hello aktiválási kérelmet küldjön segítséget:

1. Hello nyilvános társviszony az ExpressRoute-kör létesítéséhez.
2. Hajtsa végre a DNS-címkeresést, és a hello IP-címének **kms.core.windows.net**
3. hello kulcskezelő szolgáltatás kell azonosítani, hogy hello aktiválási kérelem érkezik, az Azure és elfogadása hello kérelem. Hajtsa végre a következő három feladatok hello egyikét:

   * A helyszíni hálózaton irányíthatja a forgalmat a hello szánt hello 2. lépés hátsó tooAzure hello nyilvános társviszony keresztül beszerzett IP-címet.
   * Rendelkezik a NSP szolgáltató haj PIN-kód hello forgalom hátsó tooAzure keresztül hello nyilvános társviszony-létesítés.
   * Hozzon létre felhasználói útvonalat adott pontok hello IP-cím a következő ugrásként internetkapcsolattal, és alkalmazza azt toohello alhálózat ezek a virtuális gépek esetén.

### <a name="can-i-change-hello-bandwidth-of-an-expressroute-circuit"></a>Módosíthatja a hello sávszélesség az ExpressRoute-kapcsolatcsoportot?

Igen, próbálja meg tooincrease hello sávszélesség az ExpressRoute-kapcsolatcsoportot hello Azure-portálon, vagy a PowerShell használatával. Ha van kapacitása elérhető hello tartozó fizikai port a kapcsolatcsoport létrehozásának időpontja, a módosítás sikeres lesz. 

Ha a módosítás nem sikerül, akkor nincs elég kapacitás balra hello aktuális port az és a toocreate egy új ExpressRoute-kapcsolatcsoportot hello nagyobb sávszélességgel, vagy nincs további kapacitást az adott helyhez, a ebben az esetben sem lesz szüksége tooincrease hello sávszélesség. 

Ki is toofollow a kapcsolat szolgáltató tooensure az, hogy frissítse a hálózatok toosupport hello sávszélesség növekedése belül hello szabályozások. Az ExpressRoute-kapcsolatcsoport sávszélessége hello azonban nem csökken. Toocreate egy új ExpressRoute-kapcsolatcsoportot alacsonyabb sávszélességgel rendelkezik, és hello régi kapcsolatcsoport törlése.

### <a name="how-do-i-change-hello-bandwidth-of-an-expressroute-circuit"></a>Hogyan változtathatom hello sávszélesség az ExpressRoute-kapcsolatcsoportot?

Hello sávszélesség hello ExpressRoute-kapcsolatcsoportot hello REST API-t vagy a PowerShell-parancsmag használatával frissítheti.

## <a name="expressroute-premium"></a>Az ExpressRoute-támogatás

### <a name="what-is-expressroute-premium"></a>Mi az az ExpressRoute-támogatás?

ExpressRoute prémium gyűjteménye a következő funkciók hello:

* Útválasztási táblázat korlát 4000 útvonalak too10, a nagyobb a magánhálózati társviszony-létesítés 000 útvonalait.
* Növekedett, amelyek csatlakoztatott toohello ExpressRoute-kapcsolatcsoportot lehetnek Vnetek száma (alapértelmezett érték 10). További információkért lásd: hello [ExpressRoute korlátok](#limits) tábla.
* Kapcsolat tooOffice 365 és Dynamics 365.
* Globális kapcsolatot hello Microsoft core hálózaton keresztül. Most hozzákapcsolhatja egy geopolitikai régióban egy Vnetet az ExpressRoute-kapcsolatcsoportot egy másik régióban.<br>
    **Példák:**

    *  Európa nyugati tooan szilícium Valley létrehozott ExpressRoute-kapcsolatcsoportot létrehozni egy Vnetet társíthatja. 
    *  Hello nyilvános társviszony, más geopolitikai régiók előtagokat hirdet úgy, hogy Ön kapcsolódhatnak, például az SQL Azure-Európában Nyugat-India szilícium Valley az expressroute-kapcsolatcsoporthoz.


### <a name="limits"></a>Hány Vnetek I társíthatja tooan ExpressRoute-kapcsolatcsoportot I ExpressRoute-támogatás engedélyezése?

hello alábbi táblázatokban hello ExpressRoute korlátozásai és hello száma Vnetek ExpressRoute-kapcsolatcsoportot:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>Hogyan engedélyezhető az ExpressRoute-támogatás?

ExpressRoute prémium szolgáltatások akkor engedélyezhető, ha hello szolgáltatás engedélyezve van, és hello kapcsolatcsoport állapotát frissítésével leállíthat. ExpressRoute prémium engedélyezheti a kapcsolatcsoport létrehozáskor, vagy felhívhatja hello REST API vagy PowerShell-parancsmagot.

### <a name="how-do-i-disable-expressroute-premium"></a>Hogyan tiltsa le a ExpressRoute prémium?

Letilthatja a ExpressRoute prémium hello REST API-t vagy a PowerShell-parancsmag meghívásával. Meg kell győződnie arról, hogy rendelkezik méretezni a kapcsolódási igények toomeet hello alapértelmezett korlátokat ExpressRoute-támogatás letiltása előtt. Ha a kihasználtsági méretezi hello alapértelmezett korlátokat túl, hello kérelem toodisable ExpressRoute prémium sikertelen lesz.

### <a name="can-i-pick-and-choose-hello-features-i-want-from-hello-premium-feature-set"></a>Képes vagyok kiválasztani, hello premium szolgáltatás készletből kívánt hello szolgáltatásokat?

Nem. Hello funkciók nem választhat. Ha bekapcsolja az ExpressRoute-támogatás engedélyezzük minden funkcióját.

### <a name="how-much-does-expressroute-premium-cost"></a>Mennyibe ExpressRoute prémium költség?

Tekintse meg a túl[díjszabása](https://azure.microsoft.com/pricing/details/expressroute/) költség.

### <a name="do-i-pay-for-expressroute-premium-in-addition-toostandard-expressroute-charges"></a>Kell fizetni ExpressRoute prémium szintű szolgáltatásra továbbá toostandard ExpressRoute díjak?

Igen. ExpressRoute támogatási díjak vonatkoznak fölött ExpressRoute körön és hello kapcsolat szolgáltatója szükséges díjakat.

## <a name="expressroute-for-office-365-and-dynamics-365"></a>Az Office 365 és Dynamics 365 ExpressRoute

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-tooconnect-toooffice-365-services-and-dynamics-365"></a>Hogyan hozható létre egy ExpressRoute-kapcsolatcsoport tooconnect tooOffice 365 szolgáltatások és Dynamics 365?

1. Felülvizsgálati hello [ExpressRoute Előfeltételek lapon](expressroute-prerequisites.md) toomake meg arról, hogy hello követelményeknek.
2. a kapcsolatot igénylő tooensure-e, tekintse át a szolgáltatók és helye a hello hello listája [ExpressRoute partnerek és a helyek](expressroute-locations.md) cikk.
3. Tervezze meg a kapacitásigények megtekintésével [az alaphálózati topológia tervezése és az Office 365 teljesítményhangolás](http://aka.ms/tune/).
4. Hello munkafolyamatok tooset fel a kapcsolatot a felsorolt hello lépésekkel [kiépítésével és a kapcsolatcsoport állapotok az ExpressRoute-munkafolyamatokat](expressroute-workflows.md).

> [!IMPORTANT]
> Győződjön meg arról, hogy engedélyezte ExpressRoute prémium szintű bővítmény kapcsolat tooOffice 365 szolgáltatások és Dynamics 365 beállításakor.
> 
> 

### <a name="do-i-need-tooenable-azure-public-peering-tooconnect-toooffice-365-services-and-dynamics-365"></a>Szükséges tooenable Azure nyilvános társviszony-létesítés tooconnect tooOffice 365 szolgáltatások és Dynamics 365?

Nem, csak akkor kell tooenable Microsoft Peering. Hitelesítési forgalom tooAzure AD Microsoft Peering keresztül küld el. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-toooffice-365-services-and-dynamics-365"></a>A meglévő ExpressRoute-Kapcsolatcsoportok kapcsolat tooOffice 365 szolgáltatások és Dynamics 365 támogatására?

Igen. A meglévő ExpressRoute-kapcsolatcsoportot konfigurált toosupport internetkapcsolati tooOffice 365 szolgáltatás lehet. Győződjön meg arról, hogy rendelkezik-e elegendő kapacitása tooconnect tooOffice 365 szolgáltatások, és a prémium szintű bővítmény engedélyezte. [Az alaphálózati topológia tervezése és az Office 365 teljesítményhangolás](http://aka.ms/tune/) kell azt tervezi, hogy a kapcsolat segítségével. Lásd még [létrehozása és módosítása az ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Milyen Office 365 szolgáltatások ExpressRoute-kapcsolaton keresztül elérhető?

Tekintse meg a túl[Office 365 URL-címei és IP-címtartományok](http://aka.ms/o365endpoints) lap ExpressRoute keresztül támogatott szolgáltatások naprakész listáját.

### <a name="how-much-does-expressroute-for-office-365-services-and-dynamics-365-cost"></a>Mennyibe ExpressRoute az Office 365-szolgáltatásokhoz és Dynamics 365 költség?

Office 365-szolgáltatások és a Dynamics 365 követelik meg prémium bővítmény toobe engedélyezve van. Lásd: hello [díjszabás részleteit megjelenítő oldalra](https://azure.microsoft.com/pricing/details/expressroute/) költségek.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Milyen régiók ExpressRoute az Office 365 támogatott?

Lásd: [ExpressRoute partnerek és a helyek](expressroute-locations.md) információt.

### <a name="can-i-access-office-365-over-hello-internet-even-if-expressroute-was-configured-for-my-organization"></a>Elérhető Office 365 hello interneten keresztül akkor is, ha ExpressRoute lett konfigurálva a szervezetem számára?

Igen. Office 365 szolgáltatás végpontok elérhetők az hello Internet, annak ellenére, hogy ExpressRoute engedélyezve van a hálózaton. Ha egy helyen konfigurált tooconnect tooOffice 365 szolgáltatások ExpressRoute keresztül, ExpressRoute keresztül fog csatlakozni.

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Elérhető Office 365 US Government közösségi (ÖET) szolgáltatások keresztül Azure US Government ExpressRoute-kapcsolatcsoportot?

Igen. Az Office 365 ÖET Szolgáltatásvégpontok hello Azure US Government ExpressRoute keresztül érhetők el. Azonban Ön első kell tooopen egy támogatási jegyet a hello Azure portál tooprovide hello előtagok tooadvertise tooMicrosoft szeretné. A kapcsolat tooOffice 365 ÖET szolgáltatások jön létre, miután hello támogatási jegy meg van oldva. 

### <a name="can-dynamics-365-for-operations-formerly-known-as-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics 365 műveletek (korábbi nevén Dynamics AX Online) keresztül is elérhető az ExpressRoute-kapcsolatot?

Igen. [Dynamics 365 műveletekhez](https://www.microsoft.com/dynamics365/operations) Azure-platformon futó. Az Azure nyilvános társviszony az ExpressRoute-kapcsolatcsoport tooconnect tooit engedélyezheti.

## <a name="route-filters-for-microsoft-peering"></a>A Microsoft társviszony-létesítéshez útvonal szűrők

### <a name="i-am-turning-on-microsoft-peering-for-hello-first-time-what-routes-will-i-see"></a>Szeretnék a Microsoft társviszony-létesítés hello az első alkalommal vagyok bekapcsolását, milyen útvonalak látható?

Bármely útvonalak nem látják. Tooattach rendelkezik egy útvonal szűrő tooyour áramkör toostart előtag hirdetéseket. Útmutatásért lásd: [konfigurálása útvonal szűrők a Microsoft társviszony-létesítéshez](how-to-routefilter-powershell.md).

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-tooselect-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-toodo-it"></a>Automatikus Microsoft társviszony, és most vagyok közben tooselect Exchange online-hoz, de, ezért meg, hogy a számítógépen nem engedélyezett toodo hiba azt.

Útvonal szűrők használatakor minden ügyfél bekapcsolja a Microsoft társviszony-létesítés. Azonban az Office 365-szolgáltatásokhoz fel, továbbra is szükség Office 365 által engedélyezett tooget.

### <a name="do-i-need-tooget-authorization-for-turning-on-dynamics-365-over-microsoft-peering"></a>Szükséges tooget engedélyezési bekapcsolható Dynamics 365 a Microsoft társviszony-létesítés keresztül?

Nem, nem kell engedélyezési Dynamics 365. Hozzon létre egy szabályt, és válassza ki a Dynamics 365 közösségi engedély nélkül.

### <a name="i-already-have-microsoft-peering-how-can-i-take-advantage-of-route-filters"></a>Már van a Microsoft társviszony-létesítést, hogyan lehet előnyének kihasználása útvonal szűrők?

Létrehozhat egy útvonalat szűrőt, válassza hello szolgáltatások toouse szeretne, és csatlakoztassa hello szűrő tooyour Microsoft társviszony-létesítés. Útmutatásért lásd: [konfigurálása útvonal szűrők a Microsoft társviszony-létesítéshez](how-to-routefilter-powershell.md).

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-tooenable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>A Microsoft társviszony-létesítés egy helyen, most próbálok tooenable kell azt egy másik helyen, és nem látható a előtagokat.

* Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni a Microsoft társviszony-létesítést, meghirdetett összes szolgáltatás előtagot, akkor is, ha nincsenek megadva útvonal szűrők.

* A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön. Alapértelmezés szerint nincs előtagok jelenik meg.
