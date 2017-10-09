---
title: "ExpressRoute-Kapcsolatcsoportok aaaNAT követelményei |} Microsoft Docs"
description: "Ez az oldal ExpressRoute-kapcsolatcsoportok NAT-konfigurálásának és -kezelésének részletes követelményeit ismerteti."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 867bf936-c851-485f-84c8-d8d6e33fee9f
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 09a0e841235de3f6b85e32172d7f99f20b5baf54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-nat-requirements"></a>Az ExpressRoute NAT-követelményei
tooconnect tooMicrosoft felhőszolgáltatások ExpressRoute segítségével, akkor lesz tooset be kell és NAT kezelése. Egyes kapcsolatszolgáltatók felügyelt szolgáltatásként kínálják a NAT beállítását és kezelését. Kérje meg a kapcsolat szolgáltató toosee, ha ilyen szolgáltatás kínálnak. Ha nem, akkor meg kell felelnie az alábbiakban toohello követelményeinek. 

Felülvizsgálati hello [ExpressRoute Kapcsolatcsoportok és útválasztási tartományok](expressroute-circuit-peerings.md) tooget hello áttekintése lapon a különböző útválasztási tartományok. toomeet hello nyilvános IP-cím követelmények a nyilvános Azure és a Microsoft társviszony-létesítést, azt javasoljuk, hogy beállította NAT a hálózat és a Microsoft között. Ez a szakasz részletesen hello NAT infrastruktúra tooset fel van szüksége.

## <a name="nat-requirements-for-azure-public-peering"></a>Az Azure nyilvános társviszony-létesítés NAT-követelményei
hello Azure nyilvános társviszony-létesítési elérési lehetővé teszi, hogy Ön tooconnect tooall tárolt szolgáltatások az Azure-ban a nyilvános IP-címek keresztül. Ezek közé tartozik a hello felsorolt szolgáltatások [ExpessRoute gyakran ismételt kérdések](expressroute-faqs.md) és ISV-k, a Microsoft Azure által üzemeltetett szolgáltatások. 

> [!IMPORTANT]
> Kapcsolat tooMicrosoft Azure Services szolgáltatás nyilvános társviszony-létesítés mindig kezdeményezi a hálózat hello Microsoft hálózatba. Ezért munkamenetek nem kezdeményezhető, Microsoft Azure-szolgáltatások tooyour hálózatról ExpressRoute keresztül. Megkísérelt, ha küldött csomagok toothese meghirdetett IP-címet fogja használni hello internet ExpressRoute helyett.
> 

Adatforgalmat tooMicrosoft Azure nyilvános társviszony nyilvános IPv4-címek előtt hello Microsoft hálózati SNATed toovalid kell lennie. az alábbi ábra hello hello NAT sikerült kell beállítására toomeet hello fent követelmény magas szintű képe biztosít.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### <a name="nat-ip-pool-and-route-advertisements"></a>NAT IP-készlet és útvonalhirdetések
Győződjön meg arról, hogy a forgalom hello Azure nyilvános társviszony-létesítési elérési út érvényes nyilvános IPv4-címmel rendelkező írja be. A Microsoft képes toovalidate hello tulajdonjogát hello IPv4 NAT-címkészlet egy regionális útválasztási internetcím-kezelőt (RIR) vagy egy útválasztási internetcím-kezelőt (BMR) kell lennie. Egy ellenőrzés fog futni hello alapján, számát az éppen társviszonyban pedig hello IP-címekre vonatkozó hello hálózati címfordítást. Tekintse meg a toohello [ExpressRoute útválasztási követelmények](expressroute-routing.md) lap útválasztási nyilvántartó olvashat.

Nincsenek a hello hálózati Címfordítás IP-előtagja a társviszony keresztül meghirdetett hello hosszát korlátozások. NAT-készlete hello figyelni kell, és győződjön meg arról, hogy Ön nem a NAT-munkamenetek vannak függeszteni.

> [!IMPORTANT]
> hálózati Címfordítás IP-készlet hello meghirdetett tooMicrosoft nem lehet Internet hirdetett toohello. Ezzel megszünteti a kapcsolatot tooother Microsoft-szolgáltatások.
> 
> 

## <a name="nat-requirements-for-microsoft-peering"></a>A Microsoft társviszony-létesítésre vonatkozó NAT-követelmények
hello Microsoft társviszony-létesítési elérési lehetővé teszi a csatlakozást a nem támogatott tooMicrosoft felhőszolgáltatások hello Azure nyilvános társviszony-létesítési elérési útján. szolgáltatások hello listája az Office 365-szolgáltatásokhoz, például az Exchange Online, SharePoint online-hoz, a Skype vállalati verzió, és Dynamics 365 tartalmazza. A Microsoft hello Microsoft társviszony toosupport kétirányú kapcsolatot vár. Adatforgalmat tooMicrosoft felhőszolgáltatások nyilvános IPv4-címek előtt hello Microsoft hálózati SNATed toovalid kell lennie. A Microsoft más felhőszolgáltatásaival adatforgalmat tooyour hálózati SNATed el kell érnie az internetes biztonsági tooprevent [aszimmetrikus útválasztási](expressroute-asymmetric-routing.md). az alábbi ábra hello hogyan hello NAT kell lennie a telepítő a Microsoft társviszony-létesítéshez magas szintű képe biztosít.

![](./media/expressroute-nat/expressroute-nat-microsoft.png) 

### <a name="traffic-originating-from-your-network-destined-toomicrosoft"></a>A hálózati szánt tooMicrosoft származó forgalmat
* Győződjön meg arról, hogy a forgalom hello Microsoft társviszony-létesítési elérési útja egy érvényes nyilvános IPv4-címet írja be. A Microsoft hello IPv4 NAT címkészlet elleni hello regionális útválasztási internetcím-kezelőt (RIR) képes toovalidate hello tulajdonosa vagy egy útválasztási internetcím-kezelőt (BMR) kell lennie. Egy ellenőrzés fog futni hello alapján, számát az éppen társviszonyban pedig hello IP-címekre vonatkozó hello hálózati címfordítást. Tekintse meg a toohello [ExpressRoute útválasztási követelmények](expressroute-routing.md) lap útválasztási nyilvántartó olvashat.
* Az Azure nyilvános társviszony-létesítési beállítása hello használt IP-címek és más ExpressRoute-Kapcsolatcsoportok nem lehet hirdetett tooMicrosoft hello BGP-kapcsolaton keresztül. Hello hálózati Címfordítás IP-előtagja a társviszony keresztül meghirdetett hello hosszát nincs korlátozva van.
  
  > [!IMPORTANT]
  > hálózati Címfordítás IP-készlet hello meghirdetett tooMicrosoft nem lehet Internet hirdetett toohello. Ezzel megszünteti a kapcsolatot tooother Microsoft-szolgáltatások.
  > 
  > 

### <a name="traffic-originating-from-microsoft-destined-tooyour-network"></a>Tooyour hálózati forgalmat a Microsofttól származó felé.
* Egyes esetekben szükséges Microsoft tooinitiate kapcsolat tooservice végpontok a hálózaton belül található. Hello forgatókönyvben például a hálózaton lévő Office 365 futtatott kapcsolat tooADFS kiszolgálók lenne. Ebben az esetben meg kell szivárgás lépett fel, megfelelő előtagok a hálózatról való hello Microsoft társviszony-létesítés. 
* A hálózati tooprevent belül kell SNAT Microsoft-forgalom zárása a hello szolgáltatás végpontok internetes biztonsági [aszimmetrikus útválasztási](expressroute-asymmetric-routing.md). Az ExpressRoute-on keresztül kapott útvonallal megegyező cél IP-címmel ellátott kérelmeket **és válaszokat** a rendszer minden esetben ExpressRoute-on keresztül küldi el. Ha hello kérelem érkezik a hello Internet hello keresztül aszimmetrikus útválasztási létezik ExpressRoute keresztül küldött válasz. SNATing hello bejövő Microsoft-forgalom zárása a hello internetes biztonsági válasz forgalom hátsó toohello Internet peremhálózati hello probléma megoldása kényszeríti.

![Aszimmetrikus útválasztás az ExpressRoute-tal](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## <a name="next-steps"></a>Következő lépések
* Tekintse meg a toohello követelményei [útválasztás](expressroute-routing.md) és [QoS](expressroute-qos.md).
* További információkért lásd: [ExpressRoute-kapcsolatcsoportok kiépítési munkafolyamatai és kapcsolatcsoport-állapotok](expressroute-workflows.md).
* Az ExpressRoute-kapcsolat konfigurálása.
  
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-classic.md)
  * [Útválasztás konfigurálása](expressroute-howto-routing-classic.md)
  * [Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md)

