---
title: "az Azure ExpressRoute elfogadása aaaPrerequisites |} Microsoft Docs"
description: "Ezen a lapon a követelmények toobe Azure ExpressRoute-kapcsolatcsoportot sorba rendezheti felelni listáját tartalmazza."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/30/2017
ms.author: cherylmc
ms.openlocfilehash: 524c86f6570dc6e6505fe55323b8508e8eeff791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute-előfeltételek és ellenőrzőlista
tooconnect tooMicrosoft felhőalapú szolgáltatások ExpressRoute segítségével, meg kell tooverify, hogy a következő részekben hello felsorolt követelményeknek hello teljesült.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-fiók
* Egy érvényes és aktív Microsoft Azure-fiók. Ez a fiók akkor szükséges tooset hello ExpressRoute-kapcsolatcsoportot fel. Az ExpressRoute-kapcsolatcsoportok az Azure-előfizetéseken belüli erőforrások. Azure-előfizetés követelmény akkor is, ha egy korlátozott toonon-Azure Microsoft felhőszolgáltatások, például az Office 365-szolgáltatásokhoz és Dynamics 365.
* Egy aktív Office 365-előfizetés (ha az Office 365 szolgáltatásokat használja). További információkért lásd: hello [Office 365 kapcsolatos követelmények](#office-365-specific-requirements) című szakaszát.

## <a name="connectivity-provider"></a>Kapcsolatszolgáltató

* Használhatja az [ExpressRoute kapcsolat partner](expressroute-locations.md#partners) tooconnect toohello Microsoft felhő. A helyszíni hálózata és a Microsoft között [háromféleképpen](expressroute-introduction.md) állíthat be kapcsolatot.
* Ha a szolgáltató nem egy ExpressRoute-kapcsolat partner, toohello Microsoft felhő keresztül is kapcsolódhatnak a [exchange felhőszolgáltatóként](expressroute-locations.md#connectivity-through-exchange-providers).

## <a name="network-requirements"></a>A hálózatra vonatkozó követelmények
* **Redundáns kapcsolat**: az Ön és a szolgáltató közötti fizikai kapcsolatra nem vonatkoznak redundanciakövetelmények. Microsoft igényel a BGP-munkamenetek redundáns toobe beállítása a Microsoft útválasztók és hello társviszony-létesítési útválasztók között, még ha éppen rendelkezik [egy fizikai kapcsolat tooa felhőalapú exchange](expressroute-faqs.md#onep2plink).
* **Útválasztás**: Csatlakozás a Microsoft Cloud toohello, akkor vagy a szolgáltató tooset fel kell és függően hello BGP munkameneteket kezelhessen a [útválasztási tartományok](expressroute-circuit-peerings.md). Egyes Ethernet-kapcsolatszolgáltatók vagy felhőalapú adatcsere-szolgáltatók a BGP-felügyeletet értéknövelt szolgáltatásként kínálhatják.
* **NAT**: A Microsoft csak nyilvános IP-címeket fogad el a Microsoft társviszony-létesítésen keresztül. Magánhálózati IP-címek használatakor a helyszíni hálózat, és a szolgáltató kell tootranslate hello privát IP-címek toohello nyilvános IP-címek [NAT használata hello](expressroute-nat.md).
* **QoS**: A Skype Vállalati verzió különböző szolgáltatásokat tartalmaz (például hang, videó, szöveg), amelyek különböző QoS-kezelést igényelnek. Ön és a szolgáltató érdemes követnie hello [QoS követelmények](expressroute-qos.md).
* **Hálózati biztonság**: fontolja meg [hálózati biztonság](../best-practices-network-security.md) toohello ExpressRoute segítségével a Microsoft Cloud kapcsolódáskor.

## <a name="office-365"></a>Office 365
Ha az Office 365 tooenable ExpressRoute tervezi, tekintse át a dokumentumok Office 365-követelményeivel kapcsolatos további információt a következő hello.

* [Az Office 365-höz használt ExpressRoute áttekintése](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Útválasztás az Office 365-höz használt ExpressRoute-tal](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [Office 365 URL-címek és IP-címtartományok](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Hálózattervezés és teljesítményhangolás az Office 365-höz](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Hálózatisávszélesség-kalkulátorok és eszközök](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Az Office 365 integrációja helyszíni környezetekkel](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [ExpressRoute az Office 365-ön haladó szintű oktatóvideók](https://channel9.msdn.com/series/aer/)

## <a name="dynamics-365"></a>Dynamics 365
Ha ExpressRoute tooenable Dynamics 365 tervezi, tekintse át a dokumentumok Dynamics 365 további információt a következő hello

* [Tanulmány – Dynamics 365 és ExpressRoute](http://download.microsoft.com/download/B/2/8/B2896B38-9832-417B-9836-9EF240C0A212/Microsoft%20Dynamics%20365%20and%20ExpressRoute.pdf)
* [Dynamics 365-ös URL-címek](https://support.microsoft.com/kb/2655102) és [IP-címtartományok](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Következő lépések
* ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
* Egy ExpressRoute-kapcsolatszolgáltató keresése. Lásd: [ExpressRoute-partnerek és társviszony-létesítési helyszínek](expressroute-locations.md).
* Tekintse meg a toorequirements [útválasztás](expressroute-routing.md), [NAT](expressroute-nat.md), és [QoS](expressroute-qos.md).
* Az ExpressRoute-kapcsolat konfigurálása.
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-classic.md)
  * [Útválasztás konfigurálása](expressroute-howto-routing-classic.md)
  * [Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md)
