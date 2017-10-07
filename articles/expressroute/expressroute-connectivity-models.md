---
title: "ExpressRoute modellek: tooMicrosoft Azure csatlakozhat a hálózati szolgáltatók cseréjét és Ethernet-szolgáltatók |} Microsoft Docs"
description: "Ez a cikk ismerteti a különböző módok hello hello az ügyfél hálózati és a Microsoft Azure, az Office 365 és a Dynamics 365 szolgáltatás közötti kapcsolat. Az ügyfelek használhatnak MPLS-szolgáltatókat, felhőbeli adatcserélőket vagy Ethernet-szolgáltatókat."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2017
ms.author: cherylmc
ms.openlocfilehash: 2682e6e45b2892869068f132bedb4bb08e3f89a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-connectivity-models"></a>ExpressRoute kapcsolati modellek
Három különböző módon is létrehozhat a helyszíni hálózat és a Microsoft cloud hello közötti kapcsolat [CloudExchange közös elhelyezés](#CloudExchange), [Point-to-Point protokoll Ethernet-kapcsolat](#Ethernet), és [Bármely elem közöttiként (IPVPN) kapcsolat](#IPVPN). A kapcsolatszolgáltatók egy vagy több kapcsolódási modellt kínálhatnak. A kapcsolat szolgáltatói toopick hello modell, amely a legjobb az Ön is dolgozhat.
<br><br>

![Az ExpressRoute kapcsolati modellek diagramja](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <a name="CloudExchange"></a>Közös elhelyezés felhőalapú adatcsere keretében
Közös az olyan felhőalapú exchange egy helyen találhatók, ha sorba rendezheti virtuális kereszt-kapcsolatok toohello Microsoft felhő hello közös elhelyezés szolgáltató Ethernet exchange keresztül. Közös elhelyezés szolgáltatók kínálhat, 2. rétegbeli kereszt-kapcsolatokat, vagy a felügyelt 3. rétegbeli határokon-kapcsolatok az infrastruktúrát a hello közös elhelyezés létesítmény és hello Microsoft felhő között.

## <a name="Ethernet"></a>Pontok közti Ethernet-kapcsolatok
A helyszíni adatközpontokkal/irodák toohello Microsoft felhő Point-to-Point protokoll Ethernet-kapcsolaton keresztül is elérheti. Point-to-Point protokoll Ethernet szolgáltatók kínálhat 2. rétegbeli kapcsolatot, vagy a hely és a Microsoft cloud hello 3. rétegbeli kapcsolatainak felügyelt.

## <a name="IPVPN"></a>Bármely elemek közötti (IPVPN-) hálózat
A WAN integrálható a Microsoft cloud hello. Az IPVPN-szolgáltatók (jellemzően MPLS VPN) bármilyen elemek közötti kapcsolódást kínálnak a fiókirodák és az adatközpontok közti kapcsolatokhoz. hello felhő lehet összekapcsolt tooyour WAN toomake csak keresse meg a Microsoft, mint bármely más fiókirodában. A WAN-szolgáltatók jellemzően felügyelt 3. rétegbeli kapcsolatokat kínálnak. ExpressRoute-szolgáltatásait és funkcióit megegyeznek az összes összes hello modellek fent. 

## <a name="next-steps"></a>Következő lépések
* Ismerje meg az ExpressRoute-kapcsolatokat és útválasztási tartományokat. Lásd: [ExpressRoute-kapcsolatcsoportok és útválasztási tartományok](expressroute-circuit-peerings.md).
* Tudjon meg többet az ExpressRoute funkcióiról. Lásd: hello [ExpressRoute műszaki áttekintés](expressroute-introduction.md)
* Találjon egy szolgáltatót. Lásd: [ExpressRoute-partnerek és társviszony-létesítési helyszínek](expressroute-locations.md).
* Ellenőrizze, hogy minden előfeltétel teljesül-e. Lásd: [ExpressRoute-előfeltételek](expressroute-prerequisites.md).
* Tekintse meg a toohello követelményei [útválasztás](expressroute-routing.md), [NAT](expressroute-nat.md), és [QoS](expressroute-qos.md).
* Az ExpressRoute-kapcsolat konfigurálása.
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-portal-resource-manager.md)
  * [Útválasztás konfigurálása](expressroute-howto-routing-portal-resource-manager.md)
  * [Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-portal-resource-manager.md)
