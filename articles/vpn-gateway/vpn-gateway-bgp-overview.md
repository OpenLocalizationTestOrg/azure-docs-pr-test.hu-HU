---
title: a BGP az Azure VPN Gatewayek aaaOverview |} Microsoft Docs
description: "Ez a cikk bemutatja, hogyan használható a BGP az Azure VPN Gateway megoldással együtt."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: ced3f77ecd791c84fb72b96447e839be3bf94846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>A BGP és az Azure VPN Gateway együttműködésének áttekintése
Ez a cikk ismerteti az Azure VPN Gateway megoldásban a BGP (Border Gateway Protocol) támogatását.

A BGP egy szabványos útválasztási protokoll hello általánosan használt az hello Internet tooexchange az Útválasztás és reachability information legalább két hálózat között. Ha az Azure virtuális hálózatok hello környezetben használják, BGP engedélyezi hello Azure VPN-átjáróként és BGP-társakat nevű a helyi VPN-eszközök vagy szomszédok tooexchange "irányítja a", amely mindkét átjárókat a hello rendelkezésre állását és elérhetőség tájékoztatja azok előtagok toogo hello átjárók és az érintett útválasztók keresztül. BGP is engedélyezhető tranzit útválasztást több hálózat között útvonalak BGP átjáró egy BGP-társ tooall való megtanulja propagálására más BGP-társak. 

## <a name="why-use-bgp"></a>Miért érdemes a BGP-t használni?
A BGP egy olyan opcionális szolgáltatás, amely az Azure útvonalalapú VPN-átjáróival együtt használható. Is győződjön meg arról, hogy a helyszíni VPN-eszközök támogatják a BGP hello szolgáltatás engedélyezése előtt. Folytathatja toouse Azure VPN-átjáróként és BGP nélkül a helyszíni VPN-eszközök. Hello (BGP) nélkül statikus útvonalak használatával egyenértékű *és* használata a dinamikus útválasztási BGP a hálózatok és az Azure között.

A BGP használata számos előnyt és új képességet biztosít:

### <a name="support-automatic-and-flexible-prefix-updates"></a>Az automatikus és rugalmas előtagfrissítések támogatása
A BGP-vel csak akkor kell toodeclare előtag minimális tooa adott BGP társ hello IPsec S2S VPN-alagúton keresztül. Ez lehet a mérete legyen a gazdagép előtag (/ 32) a BGP társ IP-címét a helyszíni VPN-eszköz hello. Azt is szabályozhatja, amely a helyszíni hálózati előtagok az Azure Virtual Network tooaccess tooadvertise tooAzure tooallow szeretné.

Is előfordulhat, hogy tartalmazza a VNet-címelőtagjait, például a nagy privát IP-címterület (például: 10.0.0.0/8) némelyike nagyobb előtagok hirdethető meg. Vegye figyelembe, ha hello előtagok nem lehet azonos a VNet-előtagok egyike. A program elutasítja azonos tooyour VNet-előtagok útvonalak.

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Többcsatornás üzemeltetés támogatása BGP-alapú automatikus feladatátvétellel, egy virtuális hálózat és egy helyszín között
Létrehozhat több kapcsolatot az Azure virtuális hálózat és a helyszíni VPN-eszközök közötti hello az ugyanazon a helyen. Ez a funkció aktív-aktív konfigurációban hello a két hálózat között, több alagutat (elérési út) biztosít. Ha valamelyik hello alagutak le van választva, hello megfelelő útvonalak BGP keresztül visszavonja, és hello forgalom automatikusan toohello fennmaradó alagutak lesz.

a következő diagram hello a magas rendelkezésre állású telepítés egy egyszerű példa látható:

![Több aktív elérési út](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Tranzit jellegű útválasztás támogatása a helyszíni hálózatok és több Azure virtuális hálózat között
A BGP lehetővé teszi több átjárók toolearn, és különböző hálózatokon előtagjait propagálására, hogy közvetlenül vagy közvetve csatlakoznak. Ezzel engedélyezhető az Azure VPN-átjárók használatával történő tranzit útválasztás a helyszínek között vagy több Azure virtuális hálózaton.

hello alábbi ábrán egy példa egy Többugrásos topológia több elérési utat, amely lehet átmenő forgalom hello két helyszíni hálózat között keresztül Azure VPN gatewayek hello Microsoft Networks belül:

![Többugrásos átvitel](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a>A BGP – GYAKORI KÉRDÉSEK
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a>Következő lépések
Lásd: [BGP-első lépések az Azure VPN gatewayek](vpn-gateway-bgp-resource-manager-ps.md) vonatkozó lépéseket tooconfigure BGP a létesítmények közötti és VNet – VNet kapcsolatokhoz.

