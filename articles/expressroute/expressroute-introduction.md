---
title: "ExpressRoute – áttekintés: Kiterjesztése a helyszíni hálózati tooAzure titkos kapcsolaton keresztül |} Microsoft Docs"
description: "Az ExpressRoute műszaki áttekintés bemutatja ExpressRoute kapcsolat működése tooextend a helyszíni hálózati tooAzure titkos kapcsolaton keresztül."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: fd95dcd5-df1d-41d6-85dd-e91d0091af05
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 01301e1205c12ecdab34dc9d9b92bc7489e7826c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-overview"></a>ExpressRoute – áttekintés
A Microsoft Azure ExpressRoute lehetővé teszi a helyszíni hálózatokhoz kiterjeszti a Microsoft cloud hello könnyíteni a kapcsolat szolgáltatójánál titkos kapcsolaton keresztül. Az ExpressRoute kapcsolatok tooMicrosoft felhőszolgáltatások, például a Microsoft Azure, az Office 365 és a Dynamics 365 hozhatnak létre.

A kapcsolatok lehetnek: bármely elemek közötti (IP VPN) hálózat, pontok közötti Ethernet-hálózat vagy egy virtuális keresztkapcsolat egy kapcsolatszolgáltatón keresztül egy közös elhelyezési létesítményben. Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. Ez lehetővé teszi ExpressRoute kapcsolatok toooffer további megbízhatóságát, gyorsabb sebességű, kisebb késések fordulnak elő, és nagyobb biztonságot nyújtana tipikus kapcsolatok hello interneten keresztül. Információ tooconnect a hálózati tooMicrosoft használatával ExpressRoute, lásd: [ExpressRoute modellek](expressroute-connectivity-models.md).

![](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Főbb előnyök

* A helyszíni hálózat és a kapcsolat-szolgáltató használatával a Microsoft Cloud hello 3 összekapcsolását réteg. A kapcsolatok lehetnek: bármely elemek közötti (IPVPN) hálózat, pontok közötti Ethernet-kapcsolat vagy egy virtuális keresztkapcsolat egy Ethernet-adatcserélőn keresztül.
* Kapcsolat tooMicrosoft felhő szolgáltatásainak minden régióban hello geopolitikai régióban.
* Globális kapcsolatot tooMicrosoft szolgáltatásainak minden régióban a prémium szintű ExpressRoute-bővítmény.
* Dinamikus útválasztás a hálózata és a Microsoft közt iparági szabványnak megfelelő protokollokon (BGP) keresztül.
* Beépített redundancia minden társviszony-létesítési helyszínen a nagyobb megbízhatóság érdekében.
* Kapcsolat-üzemidőre vonatkozó [SLA](https://azure.microsoft.com/support/legal/sla/).
* QoS-támogatás a Skype Vállalati verziójához.

További információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

## <a name="features"></a>Szolgáltatások

### <a name="layer-3-connectivity"></a>3. rétegbeli kapcsolatok
A Microsoft által használt iparági szabványos dinamikus útválasztási protokoll (BGP) tooexchange irányítja a helyszíni hálózat, a példányok az Azure-ban, és a Microsoft közötti nyilvános címek.  Több BGP-munkamenetet létesítünk a hálózattal, különböző forgalomprofilokkal. További részletek találhatók a hello [ExpressRoute körön és útválasztási tartományok](expressroute-circuit-peerings.md) cikk.

### <a name="redundancy"></a>Redundancia
Két kapcsolatok tootwo Microsoft Enterprise peremhálózati útválasztók (MSEEs) hello kapcsolat szolgáltató áll minden ExpressRoute-kapcsolatcsoportot / a hálózati biztonsági. Microsoft kettős BGP kapcsolatot igényel hello kapcsolat szolgáltató vagy a kiszolgálóoldali – egy tooeach MSEE. Úgy is dönthet, nem toodeploy redundáns eszközök / Ethernet áramkörök a végén. Kapcsolat szolgáltatók azonban, hogy a kapcsolatok elküldés tooMicrosoft redundáns módon redundáns eszközök tooensure használja. A redundáns 3. rétegbeli kapcsolatot konfigurációs feltétele az [SLA](https://azure.microsoft.com/support/legal/sla/) toobe érvényes.

### <a name="connectivity-toomicrosoft-cloud-services"></a>Kapcsolat tooMicrosoft cloud services csomag
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

ExpressRoute-kapcsolatok engedélyezése a szolgáltatások a következő hozzáférési toohello:

* Microsoft Azure-szolgáltatások
* Microsoft Office 365-szolgáltatások
* Microsoft Dynamics 365

Hello ellátogathat [ExpressRoute – gyakori kérdések](expressroute-faqs.md) lap ExpressRoute keresztül támogatott szolgáltatások részletes listáját.

### <a name="connectivity-tooall-regions-within-a-geopolitical-region"></a>Kapcsolat tooall régiók geopolitikai régión belül
Az egyik tooMicrosoft a Kapcsolódás a [társviszony-létesítési helyek](expressroute-locations.md) és hozzáférési tooall régiók hello geopolitikai régión belül. 

Például ha az Amszterdami tooMicrosoft ExpressRoute keresztül csatlakoztatott, akkor hozzáférési tooall Microsoft felhőszolgáltatások Észak-Európa és Nyugat-Európa tárolt. Lásd: hello [ExpressRoute partnerek és társviszony-létesítési helye](expressroute-locations.md) hello geopolitikai régiókban, a kapcsolódó Microsoft felhőalapú területeket és a megfelelő ExpressRoute társviszony-létesítési helyek a cikk áttekintése.

### <a name="global-connectivity-with-expressroute-premium-add-on"></a>Globális kapcsolódás az ExpressRoute prémium bővítmény használatával
Hello ExpressRoute prémium bővítmény szolgáltatás tooextend kapcsolat geopolitikai határokon keresztül történő engedélyezéséhez. Például ha az Amszterdami ExpressRoute keresztül csatlakoztatott tooMicrosoft, hogy hozzáférési tooall Microsoft felhőszolgáltatások között hello world minden régióban üzemeltetett (a nemzeti felhők ki vannak zárva). Dél-Amerika vagy Ausztrália hello azonos módon férhet hello északi és Nyugat-Európában régiókban telepített szolgáltatások érheti el.

### <a name="rich-connectivity-partner-ecosystem"></a>A kapcsolati partnerek gazdag ökoszisztémája
Az ExpressRoute kapcsolati és SI-partnerek egyre bővülő ökoszisztémájával rendelkezik. Olvassa el a toohello [ExpressRoute szolgáltatók és a helyek](expressroute-locations.md) hello legújabb információt.

### <a name="connectivity-toonational-clouds"></a>Kapcsolat toonational felhők
A Microsoft elkülönített felhőkörnyezeteket tart fenn speciális geopolitikai régiók és ügyfélszegmensek számára. Tekintse meg a toohello [ExpressRoute szolgáltatók és a helyek](expressroute-locations.md) lap nemzeti felhők és a szolgáltatók listáját.

### <a name="bandwidth-options"></a>Sávszélesség-lehetőségek
A sávszélességek széles választékához vásárolhat ExpressRoute-kapcsolatcsoportokat. A támogatott sávszélességeket az alábbi lista tartalmazza. A kapcsolat szolgáltató toodetermine hello listáját támogatott sávszélesség ezek biztosítják, hogy toocheck lehet.

* 50 Mbps
* 100 Mbps
* 200 Mbps
* 500 Mbps
* 1 Gbps
* 2 Gbps
* 5 Gbps
* 10 Gbps

### <a name="dynamic-scaling-of-bandwidth"></a>Dinamikus sávszélesség-méretezés
(Az elérhető legjobb alapján) hello ExpressRoute-kapcsolatcsoport sávszélessége növelhető anélkül, hogy a kapcsolatok le tootear. 

### <a name="flexible-billing-models"></a>Rugalmas számlázási modellek
Kiválaszthatja az Ön számára optimális számlázási modellt. Válasszon az alábbi hello számlázási modellek. További információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

* **Korlátlan adatforgalom**. hello ExpressRoute-kapcsolatcsoportot fel van töltve egy havi díj alapján, és az összes bejövő és kimenő adatforgalom ingyenes legyen. 
* **Forgalmi díjas adatforgalom**. hello ExpressRoute-kapcsolatcsoportot havi díjért alapján számítjuk fel. Az összes bejövő adatátvitel ingyenes. A kimenő adatátvitel számlázása az átvitt adatok GB-jai alapján történik. Az adatátviteli árak régiónként eltérnek.
* **ExpressRoute prémium bővítmény**. hello ExpressRoute prémium hello ExpressRoute-kapcsolatcsoportot keresztül az bővítménye. hello ExpressRoute prémium szintű bővítmény hello a következő lehetőségeket biztosítja: 
  * Az Azure nyilvános és Azure magánhálózati társviszony-létesítés 4000 útvonalak too10, 000 útvonalak a megnövekedett útvonal határértékeit.
  * Globális kapcsolódás a szolgáltatásokhoz. Létre bármely régióban (kivéve a nemzeti felhők) ExpressRoute-kapcsolatcsoportot lesz hozzáférés tooresources hello world bármely más régió között. Egy Nyugat-Európában létrehozott virtuális hálózat például elérhető egy, a Szilícium-völgyben kiosztott ExpressRoute-kapcsolatcsoportról is.
  * Virtuális hálózat hivatkozások száma 10 tooa nagyobb korlát, attól függően, hogy a kapcsolatcsoport hello hello sávszélesség ExpressRoute-kapcsolatcsoportot nagyobb száma.

## <a name="faq"></a>GYIK

ExpressRoute kapcsolatos gyakori kérdésekre, lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

## <a name="next-steps"></a>Következő lépések

* [Az ExpressRoute kapcsolati modelljeinek](expressroute-connectivity-models.md) ismertetése.
* Ismerje meg az ExpressRoute-kapcsolatokat és útválasztási tartományokat. Lásd: [ExpressRoute-kapcsolatcsoportok és útválasztási tartományok](expressroute-circuit-peerings.md).
* Találjon egy szolgáltatót. Lásd: [ExpressRoute-partnerek és társviszony-létesítési helyszínek](expressroute-locations.md).
* Ellenőrizze, hogy minden előfeltétel teljesül-e. Lásd: [ExpressRoute-előfeltételek](expressroute-prerequisites.md).
* Tekintse meg a toohello követelményei [útválasztás](expressroute-routing.md), [NAT](expressroute-nat.md), és [QoS](expressroute-qos.md).
* Az ExpressRoute-kapcsolat konfigurálása.
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-portal-resource-manager.md)
  * [Az ExpressRoute-kapcsolatcsoport társviszony-létesítésének konfigurálása](expressroute-howto-routing-portal-resource-manager.md)
  * [Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-portal-resource-manager.md)
* Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure.
