---
title: "ARP-tábla beolvasása: erőforrás-kezelő: Azure ExpressRoute-hibaelhárítási |} Microsoft Docs"
description: "Ezen a lapon útmutatás beolvasásakor hello ARP táblák az ExpressRoute-kapcsolatcsoportot"
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
ms.openlocfilehash: c386b031814d40ef6ea3ce5e0eaaab9634470e8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-arp-tables-in-hello-resource-manager-deployment-model"></a>Hello Resource Manager üzembe helyezési modellel tábla ARP beolvasása
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – Klasszikus](expressroute-troubleshooting-arp-classic.md)
> 
> 

Ez a cikk bemutatja, hogyan hello lépéseket toolearn hello ARP táblák az ExpressRoute-kapcsolatcsoport esetében. 

> [!IMPORTANT]
> Ez a dokumentum tervezett toohelp diagnosztizálhatja és egyszerű problémák megoldásával kapcsolatban. Már nem tervezett toobe helyettesíti a Microsoft támogatási szolgálatához. Meg kell nyitnia a támogatási jegy [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) nem toosolve hello problémát az alább ismertetett hello útmutatást esetén.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Cím Resolution Protocol (ARP) és a ARP
Address Resolution Protocol (ARP) definiálva a 2. réteg protokoll [RFC 826](https://tools.ietf.org/html/rfc826). ARP használt toomap hello Ethernet-címe (MAC-cím) IP-címmel.

ARP-táblázat hello biztosítja a leképezést hello IPv4-cím és MAC-címet adott társviszony-létesítés. egy ExpressRoute-kapcsolatcsoport társviszonyt az ARP-táblázat hello biztosít hello a következő információkat az egyes csatolókra (elsődleges és másodlagos)

1. A helyi útválasztó illesztő ip cím toohello MAC-cím hozzárendelése
2. Az ExpressRoute útválasztó illesztő IP-cím toohello MAC címe leképezése
3. Hello leképezési korát

ARP-táblázatok érvényesíteni a 2. réteg segítségével, és alapvető hibaelhárítási réteg 2 kapcsolódási problémák. 

Példa ARP-táblázat: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


hello következő részben megtudhatja hogyan megtekintheti hello szerinti hello ExpressRoute peremhálózati útválasztók ARP-táblázatok. 

## <a name="prerequisites-for-learning-arp-tables"></a>ARP-táblázatok tanulási előfeltételei
Gondoskodjon arról, hogy további előrehaladás hello követően

* Egy érvényes ExpressRoute-kapcsolatcsoportot legalább egy társviszony-létesítés konfigurálva. hello áramkör a hello kapcsolat szolgáltatójánál teljesen kell konfigurálni. Ön (vagy a kapcsolat szolgáltatóját) kell konfigurált hello esetében (Azure saját, az Azure nyilvános és a Microsoft) közül legalább egy ebben a kapcsolatcsoportban.
* IP-címtartományok hello esetében (Azure saját, az Azure nyilvános és a Microsoft) konfigurálására használható. Tekintse át a hello ip cím hozzárendelés példák hello [ExpressRoute útválasztási követelmények lapon](expressroute-routing.md) tooget megismerhesse, milyen IP-címek vannak leképezve az ügyféloldali és hello ExpressRoute ügyféloldali toointerfaces. Hello társviszony-létesítési konfiguráció tájékoztatást kaphat hello megtekintésével [ExpressRoute-társviszony-létesítési konfiguráció lapon](expressroute-howto-routing-arm.md).
* A hálózati csoport adatait / ezek IP-címekkel rendelkező használt hello adapterek MAC-címek a kapcsolat szolgáltatóját.
* Hello legújabb PowerShell-modult az Azure-ba (1,50 vagy újabb verzió) kell rendelkeznie.

## <a name="getting-hello-arp-tables-for-your-expressroute-circuit"></a>Az ExpressRoute-kapcsolatcsoportot hello ARP tábla beolvasása
A szakasz ismerteti, hogyan megtekintheti utasításokat hello ARP-táblázatok / társviszony a PowerShell használatával. Ön vagy a kapcsolat szolgáltatójánál úgy kell konfigurálnia hello mielőtt elmélyedne a további társviszony-létesítés. Minden kapcsolat van két elérési útnak (elsődleges és másodlagos). ARP-táblázat az egyes elérési utakat hello egymástól függetlenül ellenőrizheti.

### <a name="arp-tables-for-azure-private-peering"></a>Az Azure magánhálózati társviszony-létesítés ARP-táblázatok
a következő parancsmag hello biztosít hello ARP táblák Azure magánhálózati társviszony-létesítés

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Minta kimenet hello elérési utak közül legalább egy alább láthatók

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Az Azure nyilvános társviszony ARP-táblázatok
a következő parancsmag hello biztosít hello ARP táblák az Azure nyilvános társviszony-létesítés

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Minta kimenet hello elérési utak közül legalább egy alább láthatók

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>A Microsoft társviszony-létesítéshez ARP-táblázatok
a következő parancsmag hello biztosít hello ARP táblák Microsoft társviszony-létesítés

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Minta kimenet hello elérési utak közül legalább egy alább láthatók

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-toouse-this-information"></a>Hogyan toouse ezt az információt
hello ARP-táblázat társviszony használható toodetermine 2. réteg konfiguráció és a kapcsolat ellenőrzése. Ez a szakasz áttekintést ARP-táblázatok megjelenését a különböző helyzetekben.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-táblázat Ha expressroute-kapcsolatcsoporthoz működési állapot (várt állapot)
* hello ARP-táblázat egy bejegyzést a hello helyszíni oldalon egy érvényes IP-cím és MAC-cím és egy hasonló bejegyzést hello Microsoft ügyféloldali lesz. 
* hello hello a helyi IP-cím utolsó oktettje mindig lesz páratlan szám.
* Microsoft IP-cím hello hello utolsó oktettje mindig lesz páros szám.
* azonos MAC-cím fog megjelenni minden 3 társviszony (elsődleges / másodlagos) a Microsoft ügyféloldali hello hello. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP táblából helyszíni / szolgáltató kiszolgálóoldali csatlakozási problémák vannak
Ha a helyszíni hello problémák vannak, vagy megjelenik, hogy a kapcsolat szolgáltatójánál láthatja, hogy vagy csak egy bejegyzés megjelenik hello ARP tábla vagy hello helyszíni MAC-cím nem teljes. Ez azt mutatja majd hello leképezési hello MAC-cím és a Microsoft ügyféloldali hello használt IP-cím között. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

vagy
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Nyisson meg egy támogatási kérést a kapcsolat szolgáltató toodebug ezek a problémák. Ha hello ARP-táblázat nem rendelkezik a hello felületek IP-címek hozzárendelt tooMAC címeket, felülvizsgálati hello a következő információkat:
> 
> 1. Ha hello hivatkozás közötti hozzárendelt első IP-cím hello hello/30-as alhálózat MSEE-PR hello és MSEE MSEE-PR. hello felületén szolgál Azure mindig MSEEs hello második IP-címet használja.
> 2. Győződjön meg arról, ha hello ügyfél (C-címke) és VLAN-címkék szolgáltatás (S-címke) felel meg a MSEE-PR és MSEE pár mindkét.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>Ha a Microsoft ügyféloldali problémák vannak az ARP-táblázat
* Nem látják az ARP-táblázat társviszony-létesítés Ha problémák vannak a hello Microsoft oldalán látható. 
* A támogatási jegy megnyitása [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Adja meg, hogy rendelkezik-e 2. rétegbeli kapcsolatot kapcsolatos problémát. 

## <a name="next-steps"></a>Következő lépések
* Ellenőrizze a ExpressRoute-kapcsolatcsoportot 3. rétegbeli konfigurációi
  * Útvonal összefoglaló toodetermine hello BGP-munkamenetek állapotának beolvasása 
  * Útválasztási táblázat toodetermine mely előtagok ExpressRoute között van-e hirdetve beolvasása
* Be- / kimeneti bájt megtekintésével adatátvitel ellenőrzése
* A támogatási jegy megnyitása [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ha továbbra is problémákat tapasztal.

