---
title: "ARP-tábla beolvasása: klasszikus: Azure ExpressRoute-hibaelhárítási |} Microsoft Docs"
description: "Ezen a lapon leírja az első hello ARP táblák ExpressRoute-kapcsolatcsoportot."
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: b5856acf-03c2-4933-8111-6ce12998d92a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: 2b01304a38fa0e0def27dbd7c391d7ad8bbdabff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-classic-deployment-model"></a>Hello klasszikus üzembe helyezési modellel tábla ARP beolvasása
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – Klasszikus](expressroute-troubleshooting-arp-classic.md)
> 
> 

Ez a cikk végigvezeti hello hello Address Resolution Protocol (ARP) táblák beolvasása az Azure ExpressRoute-kapcsolatcsoport esetében.

> [!IMPORTANT]
> Ez a dokumentum tervezett toohelp diagnosztizálhatja és egyszerű problémák megoldásával kapcsolatban. Már nem tervezett toobe helyettesíti a Microsoft támogatási szolgálatához. Ha hello probléma a következő útmutatást hello segítségével sem tudja megoldani, a támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Cím Resolution Protocol (ARP) és a ARP
ARP egy 2. rétegbeli protokoll, amely meghatározott [RFC 826](https://tools.ietf.org/html/rfc826). ARP használt toomap Ethernet-címét (MAC) tooan IP-cím.

Az ARP-táblázat hello IPv4-cím és MAC-címet adott társviszony-létesítés leképezéseket. hello egy ExpressRoute-kapcsolatcsoport társviszonyt az ARP-táblázat minden egyes (elsődleges és másodlagos) kapcsolat adatai a következő hello biztosítja:

1. A helyi útválasztó illesztő IP-cím tooa MAC cím leképezése
2. Az ExpressRoute útválasztó illesztő IP-cím tooa MAC címének leképezése
3. hello korát hello leképezése

ARP-táblázatok segíthet a 2. rétegbeli konfigurációjának ellenőrzése és a 2. rétegbeli kapcsolatot alapvető problémák elhárításához.

Az alábbiakban egy egy ARP-táblázata – példa látható:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


hello a következő szakasz ismerteti, hogyan tooview hello láthatók hello ExpressRoute peremhálózati útválasztók ARP-táblázatok.

## <a name="prerequisites-for-using-arp-tables"></a>ARP-táblázatok használatára vonatkozó Előfeltételek
Győződjön meg arról, hogy rendelkezik hello következő, a folytatás előtt:

* Egy érvényes ExpressRoute-kapcsolatcsoportot legalább egy társviszony-létesítés konfigurálva van. hello áramkör a hello kapcsolat szolgáltatójánál teljesen kell konfigurálni. Ön (vagy a kapcsolat szolgáltatóját) kell konfigurálnia hello esetében (Azure saját, az Azure nyilvános, vagy a Microsoft) közül legalább egy ebben a kapcsolatcsoportban.
* IP-címtartományok hello esetében (Azure saját, az Azure nyilvános, és a Microsoft) konfigurálásához használt. Tekintse át a hello IP cím hozzárendelés példák hello [ExpressRoute útválasztási követelmények lapon](expressroute-routing.md) tooget megismerhesse, milyen IP-címek vannak leképezve a aise és hello ExpressRoute oldalán toointerfaces. Hello társviszony-létesítési konfiguráció információt kaphat a hello megtekintésével [ExpressRoute-társviszony-létesítési konfiguráció lapon](expressroute-howto-routing-classic.md).
* Információ a hálózati csoport vagy a kapcsolat szolgáltatójánál hello MAC-címek hello felületek, amelyek a következő IP-címek fel.
* hello legújabb Windows PowerShell-modult az Azure-ba (1,50 vagy újabb verzió).

## <a name="arp-tables-for-your-expressroute-circuit"></a>Az ExpressRoute-kapcsolatcsoportot ARP-táblázatok
Ez a szakasz bemutatja, hogyan tooview hello ARP táblák az egyes társviszony-létesítés PowerShell használatával. A folytatás előtt, vagy a kapcsolat szolgáltatójánál kell a tooconfigure hello társviszony-létesítés. Minden kapcsolat van két elérési útnak (elsődleges és másodlagos). ARP-táblázat az egyes elérési utakat hello egymástól függetlenül ellenőrizheti.

### <a name="arp-tables-for-azure-private-peering"></a>Az Azure magánhálózati társviszony-létesítés ARP-táblázatok
a következő parancsmag hello nyújt hello ARP táblák Azure magánhálózati társviszony-létesítés.

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Az alábbiakban látható minta kimenet hello elérési utak egyike:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Az Azure nyilvános társviszony ARP-táblázatok:
a következő parancsmag hello biztosít hello ARP táblák az Azure nyilvános társviszony:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Az alábbiakban látható minta kimenet hello elérési utak egyike:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Az alábbiakban látható minta kimenet hello elérési utak egyike:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>A Microsoft társviszony-létesítéshez ARP-táblázatok
a következő parancsmag hello táblák biztosít hello ARP a Microsoft társviszony-létesítéshez:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Minta kimenet hello elérési utak közül legalább egy alább találja:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a>Hogyan toouse ezt az információt
ARP-táblázat társviszony hello használt toovalidate 2. rétegbeli konfiguráció és a kapcsolat lehet. Ez a szakasz áttekintést ARP-táblázatok megjelenését különböző helyzetekben.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP táblából, ha expressroute-kapcsolatcsoporthoz (várt) működési állapotban van.
* ARP-táblázat hello bejegyzést tartalmaz, hello helyszíni oldalának egy érvényes IP-cím és MAC-címmel rendelkező, és egy hasonló bejegyzést hello Microsoft oldalán.
* hello hello a helyi IP-cím utolsó oktettje mindig páratlan szám.
* Microsoft IP-cím hello hello utolsó oktettje mindig páros szám-e.
* azonos MAC-cím jelenik meg az összes három esetében (elsődleges és másodlagos) Microsoft ügyféloldali hello hello.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-hello-connectivity-provider-side-has-problems"></a>Ha a helyszíni, vagy ha hello kapcsolat-szolgáltató ügyféloldali problémák vannak az ARP-táblázat
 Csak egy bejegyzés hello ARP-táblázat jelenik meg. Azt illusztrálja, hello leképezése hello MAC és a Microsoft ügyféloldali hello használt hello IP-címet.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Ehhez hasonló problémát tapasztal, nyissa meg a támogatási kérelem és a kapcsolat szolgáltató tooresolve.
> 
> 

### <a name="arp-table-when-hello-microsoft-side-has-problems"></a>Ha Microsoft ügyféloldali hello problémák vannak az ARP-táblázat
* Nem látják az ARP-táblázat társviszony-létesítés Ha problémák vannak a hello Microsoft oldalán látható.
* A támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Adja meg, hogy rendelkezik-e 2. rétegbeli kapcsolatot kapcsolatos problémát.

## <a name="next-steps"></a>Következő lépések
* Ellenőrizze a ExpressRoute-kapcsolatcsoportot 3. rétegbeli konfigurációi:
  * A BGP-munkamenetek útvonal összefoglaló toodetermine hello állapotának beolvasása.
  * Egy útvonal tábla toodetermine mely előtagok ExpressRoute között van-e hirdetve beolvasása.
* Adatátvitel érvényesítése küldött bájtok megtekintésével.
* A támogatási kérést nyithat [Azure a Microsoft Súgó és támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha továbbra is problémákat tapasztal.

