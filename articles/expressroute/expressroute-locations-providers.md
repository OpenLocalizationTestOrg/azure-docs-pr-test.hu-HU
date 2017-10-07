---
title: "Helyek és kapcsolatszolgáltatók: Azure ExpressRoute | Microsoft Docs"
description: "Ez a cikk részletes áttekintést nyújt a helyek és azokat a szolgáltatásokat kínálják hogyan tooconnect tooAzure régiók. Hely alapján rendezve."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2017
ms.author: kaanan
ms.openlocfilehash: 838d52701d177aa7f13e845b7bde66d07b5efed6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-partnerek és társviszony-létesítési helyszínek.

> [!div class="op_single_selector"]
> * [Helyek szolgáltató alapján](expressroute-locations.md)
> * [Szolgáltatók hely alapján](expressroute-locations-providers.md)


Ebben a cikkben hello táblák ExpressRoute kapcsolat szolgáltatók, melyek ExpressRoute, ExpressRoute és ExpressRoute rendszerintegrátorok (SIs) keresztül támogatja a Microsoft más felhőszolgáltatásaival tájékoztatást nyújt.

## <a name="partners"></a>ExpressRoute-kapcsolatszolgáltatók
Az ExpressRoute az összes Azure-régióban és -helyen támogatott. a következő térkép hello Azure-régiók és a helyek ExpressRoute listáját tartalmazza. Helyek ExpressRoute toothose, ahol a Microsoft több szolgáltatókkal állomásokhoz hivatkozik.

![Helyek térképe][0]

Között geopolitikai régión belül minden egyes tooAzure szolgáltatást lesz, ha létrejött a kapcsolat tooat legalább egy ExpressRoute hely hello geopolitikai régión belül. 

### <a name="azure-regions-tooexpressroute-locations-within-a-geopolitical-region"></a>Azure-régiók tooExpressRoute helyek geopolitikai régión belül
a következő táblázat hello biztosít Azure-régiók térképe tooExpressRoute helyek geopolitikai régión belül.

| **Geopolitikai régió** | **Azure-régiók** | **ExpressRoute-helyek** |
| --- | --- | --- |
| **Észak-Amerika** |USA keleti régiója, USA nyugati régiója, USA 2. keleti régiója, USA 2. nyugati régiója, USA középső régiója, USA déli középső régiója, USA északi középső régiója, USA középnyugati régiója, Közép-Kanada, Kelet-Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Szilícium-völgy, Washington, D.C., Montreal, Québec város, Toronto |
| **Dél-Amerika** |Dél-Brazília |Sao Paulo |
| **Európa** |Észak-Európa, Nyugat-Európa, az Egyesült Királyság nyugati része, az Egyesült Királyság déli része |Amszterdam, Dublin, London, Newport (Wales), Párizs |
| **Ázsia** |Kelet-Ázsia, Délkelet-Ázsia |Hongkong, Szingapúr |
| **Japán** |Nyugat-Japán, Kelet-Japán |Oszaka, Tokió |
| **Ausztrália** |Délkelet-Ausztrália, Kelet-Ausztrália |Melbourne, Sydney |
| **India** |Nyugat-India, Közép-India, Dél-India |Csennai, Mumbai |
| **Dél-Korea** |Korea középső régiója, Korea déli régiója |Busan, Szöul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>A régiók és az országos felhők geopolitikai határai
hello az alábbi táblázat bemutatja, régiók és geopolitikai határok nemzeti felhők.

| **Geopolitikai régió** | **Azure-régiók** | **ExpressRoute-helyek** |
| --- | --- | --- |
| **Az Egyesült Államok kormányának felhője** |USA-beli államigazgatás – Iowa, USA-beli államigazgatás – Virginia, USA Védelmi Minisztériuma – Középső régió, USA Védelmi Minisztériuma – Keleti régió  |Chicago, Dallas, New York, Seattle, Szilícium-völgy, Washington, D.C. |
| **Kína** |Észak-Kína, Kelet-Kína |Peking, Sanghaj |
| **Németország** |Közép-Németország, Kelet-Németország |Berlin, Frankfurt |

Geopolitikai régiók közötti kapcsolat hello standard ExpressRoute SKU nem támogatott. Szüksége lesz tooenable hello ExpressRoute prémium bővítmény toosupport globális kapcsolatot. Kapcsolat toonational felhőalapú környezetben nem támogatott. Igény esetén tájékozódjon kapcsolatszolgáltatójánál a lehetőségekről.

## <a name="locations"></a>Kapcsolatszolgáltatói helyek

hello következő táblázatban kapcsolat legyen, és hello szolgáltatók mindegyik helyen. Ha azt szeretné, tooview szolgáltatók és hello helyek, amelynek szolgáltatás is biztosítanak, lásd: [szolgáltató helyek](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Termelési Azure
| **Hely** | **Szolgáltatók** |
| --- | --- |
| **Amszterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions – Cloud Connect, Interxion, KPN, Level 3 Communications, Megaport, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Verizon, Zayo Group |
| **Atlanta** |Equinix |
| **Busan** |LG CNS |
| **Csennai** | Airtel+, Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group+ |
| **Denver** |CoreSite |
| **Dublin** |Colt, Interxion, Telecity Group |
| **Hongkong** |Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Las Vegas** |Level 3 Communications, Megaport |
| **London** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions – Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telehouse – KDDI, Telenor, Verizon, Vodafone, Zayo Group+ |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Miami** |C3ntro+, Megaport, Neutrona Networks |
| **Montreal** |Bell Canada, Cologix |
| **Mumbai** |Airtel+, Sify, Tata Communications |
| **New York** |Coresite, Equinix, Megaport, Zayo Group |
| **Newport (Wales)** |Következő generációs adatok |
| **Oszaka** |Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Párizs** |Colt, Interxion, Equinix, Orange |
| **Quebec város** | Megaport |
| **San Antonio** |Megaport |
| **Sao Paulo** |Ascenty Data Centers+, Equinix, Level 3 Communications, Neutrona Networks, Telefonica, UOLDIVEO |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Szöul** |KINX, LG CNS, Sejong Telecom |
| **Szilícium-völgy** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Console, Equinix, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |
| **Szingapúr** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokió** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |AT&T NetBond, Bell Canada, Cologix, Console, Equinix, Megaport, Telus, Zayo Group |
| **Washington, D.C.** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** = hamarosan elérhető

### <a name="national-cloud-environments"></a>Országos felhőkörnyezetek

### <a name="us-government-cloud"></a>Az Egyesült Államok kormányának felhője
| **Hely** | **Szolgáltatók** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, Level 3 Communications+, Verizon |
| **Szilícium-völgy** | Equinix, Level 3 Communications |
| **Seattle** | Equinix |
| **Washington, D.C.** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>Kína
| **Hely** | **Szolgáltatók** |
| --- | --- |
| **Peking** |China Telecom |
| **Sanghaj** |China Telecom |

több, lásd: toolearn [ExpressRoute Kínában](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Németország
| **Hely** | **Szolgáltatók** |
| --- | --- |
| **Berlin** |Colt+, e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Kapcsolódás adatcsere-szolgáltatókon keresztül
Ha a kapcsolatszolgáltató nincs felsorolva az előző szakaszokban, akkor is létesíthet kapcsolatot.

* Kérje meg a kapcsolat szolgáltató toosee, ha azok a fenti hello tábla hello cseréjét csatlakoztatott tooany. Ellenőrizheti a hello következő hivatkozásait toogather exchange szolgáltatók által kínált szolgáltatások további információt. Több kapcsolat szolgáltatók a következők: már csatlakoztatott tooEthernet cseréjét.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* A kapcsolat szolgáltatójánál, a hálózati toohello társviszony-létesítési helye választott kiterjesztése rendelkezik.
  * Győződjön meg róla, hogy a kapcsolatszolgáltató magas rendelkezésre állással terjesztette ki a kapcsolatot, tehát nincsenek kritikus hibapontok a rendszeren belül.
* Rendelés az hello exchange ExpressRoute-kapcsolatcsoportot, a kapcsolat szolgáltatójánál tooconnect tooMicrosoft.
  * Kövesse lépéseket [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) tooset fel a kapcsolatot.

## <a name="c1partners"></a>Kapcsolódás további szolgáltatókon keresztül
| **Hely** | **Exchange** | **Kapcsolatszolgáltatók** |
| --- | --- | --- |
| **Amszterdam** | Equinix, Telecity | Eurofiber, Fastweb S.p.A, Nianet |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | Nianet, QSC AG |
| **Hongkong** | Equinix | Macroview Telecom |
| **London** | Equinix, euNetworks, Telecity | Bezeq International Ltd., Epsilon, Exponential E, HSO, NexGen Networks, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **Seattle** |Equinix | Alaska Communications |
| **Szilícium-völgy** |Equinix | Cox Business, Windstream |
| **Szingapúr** |Equinix |1CLOUDSTAR, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokió** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Thinktel, Zirro|
| **Washington, D.C.** |Equinix | Altice Business, Gtt Communications Inc, Epsilon, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute-rendszerintegrátorok
A hálózat méretezését hello alapján igényeinek kihívást jelenthet magánhálózati kapcsolat toofit engedélyezése esetén. Dolgozhat sem szerepel a következő tábla tooassist hello hello rendszerintegrátorok meg bevezetési tooExpressRoute.

| **Kontinens** | **Rendszerintegrátorok** |
| --- | --- |
| **Ázsia** |Avanade Inc., OneAs1a |
| **Ausztrália** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Európa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Észak-Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Perficient, Presidio |
| **Dél-Amerika** |Avanade Inc. |
## <a name="next-steps"></a>Következő lépések
* ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
* Ellenőrizze, hogy minden előfeltétel teljesül-e. Lásd: [ExpressRoute-előfeltételek](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Helyek térképe"
