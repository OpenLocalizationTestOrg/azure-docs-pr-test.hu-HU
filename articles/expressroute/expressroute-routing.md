---
title: "Azure ExpressRoute aaaRouting követelményei |} Microsoft Docs"
description: "Ez az oldal ExpressRoute-kapcsolatcsoportok útválasztási konfigurálásának és kezelésének részletes követelményeit ismerteti."
documentationcenter: na
services: expressroute
author: osamazia
manager: ganesr
editor: 
ms.assetid: 5b382e79-fa3f-495a-a764-c5ff86af66a2
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: osamam
ms.openlocfilehash: dd50009974ae1a7156c52d4f714d8d97075f13ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-routing-requirements"></a>Az ExpressRoute útválasztási követelményei
tooconnect tooMicrosoft felhőszolgáltatások ExpressRoute segítségével, akkor lesz tooset fel kell és kezelése útválasztási. Egyes kapcsolatszolgáltatók az útválasztás beállítását és kezelését felügyelt szolgáltatásként kínálják. Kérje meg a kapcsolat szolgáltató toosee, ha ezt a szolgáltatást biztosítanak. Ha nem, akkor meg kell felelnie a toohello követelményeknek:

Tekintse meg a toohello [Kapcsolatcsoportok és útválasztási tartományok](expressroute-circuit-peerings.md) hello útválasztási leírását a cikk a toofacilitate kapcsolat beállítása toobe igénylő munkamenetek.

> [!NOTE]
> A Microsoft nem támogat semmilyen útválasztó-redundancia protokollt (például HSRP, VRRP) a magas rendelkezésre állású konfigurációkhoz. A magas rendelkezésre állás érdekében csak a társviszonyonként meglévő BGP-munkamenetek redundáns párjára támaszkodunk.
> 
> 

## <a name="ip-addresses-used-for-peerings"></a>Társviszony-létesítéshez használt IP-címek
Szüksége tooreserve néhány címblokkok, IP-címek tooconfigure útválasztási a hálózat és a Microsoft vállalati peremhálózati (MSEEs) útválasztók között. Ez a szakasz követelmények listáját tartalmazza, és ismerteti, hogyan az IP-címeket kell szerezte be, és használatos hello szabályokat.

### <a name="ip-addresses-used-for-azure-private-peering"></a>Magánhálózati Azure-társviszony-létesítéshez használt IP-címek
Magánhálózati IP-címek vagy nyilvános IP címek tooconfigure hello esetében is használhatja. útvonalak konfigurálásához használt hello címtartománya nem fedhetik cím használt címtartományok toocreate virtuális hálózatokat az Azure-ban. 

* Egy /29 vagy két /30 alhálózatot le kell foglalnia az útválasztási felületek számára.
* útválasztási használt hello alhálózatok lehet, vagy a magánhálózati IP-címét, vagy a nyilvános IP-címeket.
* hello alhálózatok nem ütközhet hello tartomány hello ügyfél hello Microsoft felhő használatra fenntartva.
* Ha egy /29 alhálózatot használ, az két /30 alhálózatra lesz felosztva. 
  * először hello/30 alhálózati hello elsődleges kapcsolathoz használja, és hello második/30-as alhálózat használt hello másodlagos kapcsolathoz.
  * Az egyes hello/30-as alhálózat első IP-cím hello hello/30-as alhálózat az útválasztón kell használnia. A Microsoft hello/30-as alhálózat tooset BGP munkamenet hello második IP-címét használja.
  * Be kell állítania a mindkét BGP-munkamenetek a [SLA-elérhetőséget](https://azure.microsoft.com/support/legal/sla/) toobe érvényes.  

#### <a name="example-for-private-peering"></a>Példa a privát társviszony-létesítésre
Ha úgy dönt, hogy toouse a.b.c.d/29 tooset társviszony-létesítés hello fel, akkor lesznek osztva a két/30-as alhálózatokra. Hello az alábbi példában szereplő úgy tekintünk hello a.b.c.d/29 alhálózati használatáról. 

a.b.c.d/29 vegyes tooa.b.c.d/30 és a.b.c.d+4/30 lesz, és le tooMicrosoft keresztül hello kiépítés API-k átadott. Mivel az elsődleges PE hello és a Microsoft fogyaszt a.b.c.d+2, mivel VRF IP-Címek hello hello elsődleges MSEE hello VRF IP a.b.c.d+1 fogja használni. Mivel hello VRF IP hello másodlagos PE és a Microsoft fogja használni, VRF IP-Címek hello a.b.c.d+6 hello másodlagos MSEE a.b.c.d+5 fogja használni.

Fontolja meg egy olyan esetben, ahol ki kell választania 192.168.100.128/29 tooset mentése magánhálózati társviszony-létesítés. 192.168.100.128/29 192.168.100.128 címei szerepelnek too192.168.100.135, többek között:

* 192.168.100.128/30 rendeli toolink1, 192.168.100.129 és a Microsoftnak 192.168.100.130 szolgáltatónál.
* 192.168.100.132/30 rendeli toolink2, 192.168.100.133 és a Microsoftnak 192.168.100.134 szolgáltatónál.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>Nyilvános Azure- és Microsoft-társviszony-létesítéshez használt IP-címek
Nyilvános IP-címek saját hello BGP-munkamenetek beállításának kell használnia. A Microsoft hello IP-címek a útválasztási Internet nyilvántartó és internetes útválasztási nyilvántartó képes tooverify hello tulajdonjogát kell lennie. 

* Használjon egy egyedi/29 alhálózati vagy két/30-as alhálózat tooset mentése hello BGP társviszony-létesítés a minden társviszony-létesítéshez ExpressRoute-kapcsolatcsoportot száma (Ha egynél több). 
* Ha egy /29 alhálózatot használ, az két /30 alhálózatra lesz felosztva. 
  * először hello/30 alhálózati hello elsődleges hivatkozáshoz használja, hello második/30-as alhálózat használt hello másodlagos kapcsolathoz.
  * Az egyes hello/30-as alhálózat első IP-cím hello hello/30-as alhálózat az útválasztón kell használnia. A Microsoft hello/30-as alhálózat tooset BGP munkamenet hello második IP-címét használja.
  * Be kell állítania a mindkét BGP-munkamenetek a [SLA-elérhetőséget](https://azure.microsoft.com/support/legal/sla/) toobe érvényes.

## <a name="public-ip-address-requirement"></a>Nyilvános IP-cím-követelmények

### <a name="private-peering"></a>Magánhálózati társviszony-létesítés
Választhat toouse a magánhálózati társviszony-létesítés nyilvános és magánhálózati IPv4-címeket. Mi biztosítjuk a forgalom végpontok közötti elkülönítését, így elkerülhető, hogy a címek átfedésben legyenek más ügyfelekkel magánhálózati társviszony-létesítés esetén. Ezek a címek csak hirdetett tooInternet. 


### <a name="public-peering"></a>Nyilvános társviszony-létesítés
hello Azure nyilvános társviszony-létesítési elérési lehetővé teszi, hogy Ön tooconnect tooall tárolt szolgáltatások az Azure-ban a nyilvános IP-címek keresztül. Ezek közé tartozik a hello felsorolt szolgáltatások [ExpessRoute gyakran ismételt kérdések](expressroute-faqs.md) és ISV-k, a Microsoft Azure által üzemeltetett szolgáltatások. Kapcsolat tooMicrosoft Azure Services szolgáltatás nyilvános társviszony-létesítés mindig kezdeményezi a hálózat hello Microsoft hálózatba. Nyilvános IP-címek hello forgalom végcélként tooMicrosoft hálózathoz kell használnia.

### <a name="microsoft-peering"></a>Microsoft társviszony-létesítés
hello Microsoft társviszony-létesítési elérési lehetővé teszi a csatlakozást a nem támogatott tooMicrosoft felhőszolgáltatások hello Azure nyilvános társviszony-létesítési elérési útján. szolgáltatások hello listája az Office 365-szolgáltatásokhoz, például az Exchange Online, SharePoint online-hoz, a Skype vállalati verzió, és Dynamics 365 tartalmazza. A Microsoft hello Microsoft társviszony kétirányú kapcsolatot támogat. Adatforgalmat tooMicrosoft felhőszolgáltatások érvényes nyilvános IPv4-címet kell használnia, előtt hello Microsoft hálózati.

Győződjön meg arról, hogy az IP-cím és mivel number nyilvántartó következő regisztrált tooyou hello egyikében:

* [ARIN](https://www.arin.net/)
* [APNIC](https://www.apnic.net/)
* [AFRINIC](https://www.afrinic.net/)
* [LACNIC](http://www.lacnic.net/)
* [RIPENCC](https://www.ripe.net/)
* [RADB](http://www.radb.net/)
* [ALTDB](http://altdb.net/)

> [!IMPORTANT]
> Nyilvános IP-cím címek meghirdetett tooMicrosoft ExpressRoute keresztül nem lehet Internet hirdetett toohello. Ez megszakadhat a kapcsolat tooother Microsoft-szolgáltatások. Azonban azok a nyilvános IP-címek, amelyeket a hálózatban található kiszolgálók használnak, amelyek O365-végpontokkal kommunikálnak a Microsofton belül, meg lehetnek hirdetve az ExpressRoute-on. 
> 
> 

## <a name="dynamic-route-exchange"></a>Dinamikus útvonalcsere
Az útválasztás cseréje az eBGP protokollon keresztül történik. Hello MSEEs és az útválasztók közötti EBGP munkamenet jött létre. A BGP-munkamenetek hitelesítése nem szükséges. Szükség esetén konfigurálható egy MD5-kivonat. Lásd: hello [útválasztás konfigurálása](expressroute-howto-routing-classic.md) és [létesítési munkafolyamatok áramkör és állapotok áramkör](expressroute-workflows.md) BGP-munkamenetek konfigurálásával kapcsolatos információkat.

## <a name="autonomous-system-numbers"></a>Autonóm rendszerek számai
A Microsoft az AS 12076 számot használja az Azure nyilvános, az Azure privát és a Microsoft társviszony-létesítéshez. A belső használatra 65515 too65520 azt előtagjuk fenntartott ASN-eket. A 16 és a 32 bites AS-számok is támogatottak.

Az adatátvitel szimmetriájára nem vonatkoznak követelmények. hello cím- és elérési utak előfordulhat, hogy haladnak át a különböző útválasztó párokat. Az azonos útvonalakat az Önhöz tartozó kapcsolatcsoport-párokon mindkét oldalról meg kell hirdetni. Útvonal metrikák nincsenek szükséges toobe azonos.

## <a name="route-aggregation-and-prefix-limits"></a>Útvonal-összevonások és előtagkorlátozások
Támogatott too4000 mentése előtagok meghirdetett toous hello Azure magánhálózati társviszony-létesítés keresztül. Ezt a másolatot too10, 000 előtagok hello ExpressRoute prémium szintű bővítmény engedélyezése növelhető. Fogadunk too200 előtagok minden BGP-munkamenetben az Azure nyilvános és a Microsoft társviszony-létesítés. 

előtagok hello száma meghaladja hello hello BGP munkamenet eldobja. Elfogadja a Microsoft hello magánhálózati társviszony-létesítési hivatkozás csak az alapértelmezett útvonalak. Szolgáltató az alapértelmezett útvonal és a privát IP-címek (az RFC 1918) hello Azure nyilvános és a Microsoft társviszony-létesítési elérési utak kiszűrhetők kell. 

## <a name="transit-routing-and-cross-region-routing"></a>Tranzit útválasztás és régiók közötti útválasztás
Az ExpressRoute nem konfigurálható tranzit útválasztóként. Toorely lesz, ha a kapcsolat szolgáltatójánál átvitel útválasztási szolgáltatásokat.

## <a name="advertising-default-routes"></a>Alapértelmezett útvonalak meghirdetése
Az alapértelmezett útvonalak használata csak az Azure privát társviszony-létesítési munkamenetek esetében engedélyezett. Ebben az esetben azt fogja átirányítani a hello társított virtuális hálózatokat tooyour hálózati származó összes forgalmat. Alapértelmezett útvonalak hirdetési a magánhálózati társviszony-létesítés hello internet elérési blokkolja az Azure-ból eredményez. Használ, a vállalati peremhálózati tooroute forgalmat kell, és az Azure-ban üzemeltetett szolgáltatások internet toohello. 

 tooenable kapcsolat tooother Azure szolgáltatások és az infrastruktúra-szolgáltatásokat, meg kell győződnie arról, hogy a következő elemek hello egyik helyen:

* Az Azure nyilvános társviszony engedélyezett tooroute forgalom toopublic végpontok
* Minden alhálózati megkövetelését az internetkapcsolat útválasztási tooallow internetkapcsolat felhasználói használja.

> [!NOTE]
> Az alapértelmezett útvonalak meghirdetése megszakítja a Windows- és az egyéb virtuálisgép-licencek aktiválását. Kövesse az utasításokat [Itt](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) kerülheti toowork.
> 
> 

## <a name="bgp"></a>BGP-közösségek támogatása
Ez a szakasz áttekinti, hogyan használhatók a BGP-közösségek az ExpressRoute-tal. Microsoft hirdetését, megfelelő közösségi értékekkel címkézett útvonalak hello nyilvános és a Microsoft társviszony-létesítési elérési útvonalat. így indoklása hello és hello közösségi értékek leírása a következő részleteket. A Microsoft, azonban nem megtartja közösségi értékek címkézett tooroutes tooMicrosoft hirdetve.

TooMicrosoft geopolitikai régión belül egy társviszony-létesítési bárhol ExpressRoute keresztül kapcsolódik, ha hozzáférési tooall Microsoft felhőszolgáltatások között hello geopolitikai határon belül minden egyes fog. 

Például ha az Amszterdami tooMicrosoft ExpressRoute keresztül csatlakoztatott, akkor hozzáférési tooall Microsoft felhőszolgáltatások Észak-Európában és Nyugat-Európában tárolt. 

Tekintse meg a toohello [ExpressRoute partnerek és társviszony-létesítési helye](expressroute-locations.md) lap geopolitikai régiók, a kapcsolódó Azure-régiók és a megfelelő társviszony-létesítés helyek ExpressRoute részletes listáját.

Geopolitikai régiónként több ExpressRoute-kapcsolatcsoportot is vásárolhat. Kellene több kapcsolatot biztosít a magas rendelkezésre állás jelentős előnyt esedékes toogeo-redundancia. Azokban az esetekben, ahol több ExpressRoute-Kapcsolatcsoportok rendelkezik a Microsoft hello nyilvános társviszony és a Microsoft társviszony-létesítés elérési utak ugyanazokat a előtagok meghirdetett hello fog kapni. Ez azt jelenti, hogy a hálózatából több útvonal fog irányulni a Microsoft felé. Ez is okozhat a hálózaton belül optimális útválasztási döntések toobe. Ennek eredményeképpen optimális csatlakozási élmény toodifferent szolgáltatások tapasztalhat. Hello közösségi értékek toomake megfelelő útválasztási döntések toooffer számíthat [optimális útválasztási toousers](expressroute-optimize-routing.md).

| **Microsoft Azure-régió** | **BGP-közösségérték** |
| --- | --- |
| **Észak-Amerika** | |
| USA keleti régiója |12076:51004 |
| USA 2. keleti régiója |12076:51005 |
| USA nyugati régiója |12076:51006 |
| USA nyugati régiója, 2. |12076:51026 |
| USA nyugati középső régiója |12076:51027 |
| USA északi középső régiója |12076:51007 |
| USA déli középső régiója |12076:51008 |
| USA középső régiója |12076:51009 |
| Közép-Kanada |12076:51020 |
| Kelet-Kanada |12076:51021 |
| **Dél-Amerika** | |
| Dél-Brazília |12076:51014 |
| **Európa** | |
| Észak-Európa |12076:51003 |
| Nyugat-Európa |12076:51002 |
| Az Egyesült Királyság déli régiója | 12076:51024 |
| Az Egyesült Királyság nyugati régiója | 12076:51025 |
| **Ázsia és a Csendes-óceáni térség** | |
| Kelet-Ázsia |12076:51010 |
| Délkelet-Ázsia |12076:51011 |
| **Japán** | |
| Kelet-Japán |12076:51012 |
| Nyugat-Japán |12076:51013 |
| **Ausztrália** | |
| Kelet-Ausztrália |12076:51015 |
| Délkelet-Ausztrália |12076:51016 |
| **India** | |
| Dél-India |12076:51019 |
| Nyugat-India |12076:51018 |
| Közép-India |12076:51017 |
| **Korea** | |
| Korea déli régiója |12076:51028 |
| Korea középső régiója |12076:51029 |

Az összes útvonal hirdetése a Microsoft hello megfelelő közösségi értékű címkével fog rendelkezni. 

> [!IMPORTANT]
> A globális előtagok egy megfelelő közösségértéket tartalmazó címkével rendelkeznek, és csak akkor lesznek meghirdetve, ha az ExpressRoute prémium bővítmény engedélyezve van.
> 
> 

Továbbá a fenti toohello, Microsoft lesz is címke előtagok alapján hello szolgáltatás tartoznak. Ez vonatkozik csak toohello Microsoft társviszony-létesítés. hello az alábbi táblázat egy táblázatot a szolgáltatás tooBGP közösségi értékét.

| **Szolgáltatás** | **BGP-közösségérték** |
| --- | --- |
| Exchange Online |12076:5010 |
| SharePoint Online |12076:5020 |
| Skype Vállalati online verzió |12076:5030 |
| Dynamics 365 |12076:5040 |
| Egyéb Office 365-szolgáltatások |12076:5100 |

> [!NOTE]
> A Microsoft nem veszi figyelembe, hogy be kell állítani hello útvonalak hirdetett tooMicrosoft BGP közösségi értékeket.
> 
> 

### <a name="bgp-community-support-in-national-clouds-preview"></a>BGP-közösségek támogatása országos felhőkörnyezetekben (Előzetes verzió)

| **Országos felhőkörnyezetek – Azure-régió**| **BGP-közösségérték** |
| --- | --- |
| **USA-beli államigazgatás** |  |
| USA-beli államigazgatás – Arizona | 12076:51106 |
| USA-beli államigazgatás – Iowa | 12076:51109 |
| USA-beli államigazgatás – Virginia | 12076:51105 |
| USA-beli államigazgatás – Texas | 12076:51108 |
| US DoD – Középső régió | 12076:51209 |
| US DoD – Kelet | 12076:51205 |


| **Szolgáltatás országos felhőkörnyezetekben** | **BGP-közösségérték** |
| --- | --- |
| **USA-beli államigazgatás** |  |
| Exchange Online |12076:5110 |
| SharePoint Online |12076:5120 |
| Skype Vállalati online verzió |12076:5130 |
| Dynamics 365 |12076:5140 |
| Egyéb Office 365-szolgáltatások |12076:5200 |

## <a name="next-steps"></a>Következő lépések
* Az ExpressRoute-kapcsolat konfigurálása.
  
  * [Hello klasszikus telepítési modell ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) vagy [létrehozása és módosítása az Azure Resource Manager használatával ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-arm.md)
  * [Konfigurálja az útválasztást hello klasszikus telepítési modell](expressroute-howto-routing-classic.md) vagy [hello Resource Manager üzembe helyezési modellben az útválasztás konfigurálása](expressroute-howto-routing-arm.md)
  * [Hivatkozásra egy klasszikus virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md) vagy [hivatkozás egy erőforrás-kezelő virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md)

