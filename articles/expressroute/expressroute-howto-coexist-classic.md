---
title: "Párhuzamosan fennálló ExpressRoute- és helyek közötti VPN-kapcsolatok konfigurálása: klasszikus: Azure | Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan ExpressRoute és hello klasszikus telepítési modell egyszerre is használható, pont-pont VPN-kapcsolat konfigurálása."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: dcf1a5af-a289-466a-b812-0bfedbd2bda0
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: abb30fff55e8ec243f2920c5b2f70c43717755fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a>Párhuzamos ExpressRoute- és párhuzamos helyek közötti kapcsolatok konfigurálása (klasszikus)
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Klasszikus](expressroute-howto-coexist-classic.md)
> 
> 

Hello képességét tooconfigure telephelyek közötti VPN és ExpressRoute több előnye is van. Telephelyek közötti VPN elérési útnak biztonságos feladatátvétel konfigurálása ExressRoute, vagy használja a pont-pont VPN tooconnect toosites nem ExpressRoute keresztül csatlakozó. Bemutatjuk hello lépéseket tooconfigure mindkét forgatókönyvet ebben a cikkben. Ez a cikk vonatkozik toohello klasszikus üzembe helyezési modellben. Ez a konfiguráció hello portál nem érhető el.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> ExpressRoute-Kapcsolatcsoportok előre konfigurálni kell az alábbi hello utasítások végrehajtása előtt. Győződjön meg arról, hogy követte hello útmutatók túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) és [konfigurálja az útválasztást](expressroute-howto-routing-classic.md) az alábbi hello lépések végrehajtása előtt.
> 
> 

## <a name="limits-and-limitations"></a>Korlátok és korlátozások
* **A tranzit útválasztás nem támogatott.** Nem hajthat végre útválasztást (az Azure-on keresztül) a helyek közötti VPN használatával csatlakoztatott helyi hálózat és az ExpressRoute használatával csatlakoztatott helyi hálózat között.
* **A pont–hely kapcsolat nem támogatott.** Nem lehet engedélyezni a pont-pont VPN kapcsolatok toohello ugyanazt a virtuális hálózatot, amely csatlakoztatott tooExpressRoute. Pont-pont VPN- és ExpressRoute nem lehet hello az azonos virtuális hálózaton.
* **A kényszerített bújtatás hello telephelyek közötti VPN-átjáró nem engedélyezhető.** Csak "kényszerítheti" minden Internet adathoz kötött forgalom hátsó tooyour a helyi hálózaton keresztül ExpressRoute.
* **A Basic SKU-átjáró nem támogatott.** Nem alapszintű Termékváltozat átjáró kell használnia az mindkét hello [ExpressRoute-átjáró](expressroute-about-virtual-network-gateways.md) és hello [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Kizárólag az útvonalalapú VPN-átjáró támogatott.** Útvonalalapú [VPN-átjárót](../vpn-gateway/vpn-gateway-about-vpngateways.md) kell használnia.
* **A VPN-átjáróhoz statikus útvonalat kell konfigurálni.** Ha a helyi hálózathoz csatlakoztatott tooboth ExpressRoute és a pont-pont VPN, rendelkeznie kell a helyi hálózati tooroute hello pont-pont VPN-kapcsolat toohello konfigurált statikus útvonal nyilvános internethez.
* **Elsőként az ExpressRoute-átjárót kell konfigurálnia.** Létre kell hoznia hello ExpressRoute-átjárót először hello telephelyek közötti VPN átjáró hozzáadása előtt.

## <a name="configuration-designs"></a>Konfigurációs tervek
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Helyek közötti VPN konfigurálása feladatátvételi útvonalként az ExpressRoute számára
Konfigurálhat egy helyek közötti VPN-kapcsolatot tartalékként az ExpressRoute számára. Ez vonatkozik csak toovirtual hálózatok csatolt toohello Azure magánhálózati társviszony-létesítési elérési útja. Az Azure nyilvános és a Microsoft társviszony-létesítésekhez nem létezik VPN-alapú feladatátvételi megoldás. hello ExpressRoute-kapcsolatcsoportot mindig hello elsődleges kapcsolat. Adatok halad keresztül hello telephelyek közötti VPN elérési út csak akkor, ha ExpressRoute-kapcsolatcsoportot hello sikertelen. 

> [!NOTE]
> Habár ExpressRoute-kapcsolatcsoportot pont-pont VPN-kapcsolaton keresztül előnyben részesített a mindkét útvonalak vannak hello azonos, Azure használandó hello leghosszabb előtag egyezés toochoose hello útvonal hello csomagok cél felé.
> 
> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a>Egy ExpressRoute keresztül nem kapcsolódik telephelyek közötti VPN-tooconnect toosites konfigurálása
Beállíthatja, hogy a hálózat, ahol egyes helyek csatlakoznak közvetlenül tooAzure pont-pont VPN-kapcsolaton keresztül, és az egyes helyek ExpressRoute keresztül csatlakoznak. 

![Egyidejű jelenlét](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> Virtuális hálózatot nem konfigurálhat tranzit útválasztóként.
> 
> 

## <a name="selecting-hello-steps-toouse"></a>Hello lépéseket toouse kiválasztása
Nincsenek az eljárások toochoose a rendelés tooconfigure kapcsolatok, amely egyszerre is használható két különböző csoportjai számára. hello konfigurációs eljárás választhat, hogy van-e egy meglévő virtuális hálózatot, hogy azt szeretné, hogy tooconnect, vagy azt szeretné, hogy egy új virtuális hálózat toocreate függ.

* Nem rendelkezik egy VNet és egy toocreate kell.
  
    Ha még nem rendelkezik virtuális hálózat, ez az eljárás végigvezeti hello klasszikus üzembe helyezési modellel, és új ExpressRoute és a pont-pont VPN-kapcsolatok létrehozása új virtuális hálózat létrehozása. tooconfigure, hajtsa végre hello hello cikk szakaszban található lépéseket [toocreate egy új virtuális hálózat és vizsgálatát a kísérő kapcsolatok](#new).
* Már rendelkezem egy klasszikus üzembehelyezési modell szerinti VNettel.
  
    Előfordulhat, hogy már rendelkezik üzemelő virtuális hálózattal egy létező helyek közötti VPN- vagy ExpressRoute-kapcsolattal. a cikk szakasz hello [tooconfigure coexsiting kapcsolatok egy már meglévő vnet](#add) hello átjáró törlése folyamatban, és új ExpressRoute- és telephelyek közötti VPN-kapcsolatok létrehozására, majd részletesen ismerteti. Vegye figyelembe, hogy ha hello új kapcsolatok létrehozására, hello lépéseket kell végrehajtania nagyon meghatározott sorrendben. Ne használjon más cikkek toocreate hello utasításait, az átjárók és kapcsolatok.
  
    Ebben az eljárásban, amely egyszerre is használható kapcsolatok létrehozására lesz igényelnek, toodelete az átjáró, és adja meg új átjárók. Ez azt jelenti, hogy a létesítmények közötti kapcsolatok állásidő során törölje és hozza létre újra az átjárót és a kapcsolatok, de nem kell toomigrate bármely, a virtuális gépek vagy szolgáltatások tooa új virtuális hálózat. A virtuális gépek és szolgáltatások is ki tudja toocommunicate keresztül hello terheléselosztó amíg az átjáró konfigurálásának, ha így konfigurálva toodo.

## <a name="new"></a>új virtuális hálózat toocreate és vizsgálatát a kísérő kapcsolatok
Az eljárás a VNetek, valamint az egyidejűleg jelenlévő helyek közötti és ExpressRoute-kapcsolatok létrehozásának módját ismerteti.

1. Tooinstall hello legújabb verziójára hello Azure PowerShell-parancsmagok lesz szüksége. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt. Előfordulhat, hogy ehhez a konfigurációhoz fogjuk hello parancsmagok némileg eltérő mi, előfordulhat, hogy ismernie kell. Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat. 
2. Hozzon létre egy sémát a virtuális hálózat számára. Hello konfigurációs séma kapcsolatos további információkért lásd: [Azure Virtual Network konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).
   
    A séma létrehozásakor ellenőrizze, a következő értékek hello használja:
   
   * hello átjáróalhálózatot hello virtuális hálózat /27 vagy egy rövidebb előtaggal (például /26 vagy /25) kell lennie.
   * hello átjáró kapcsolat típusa "Dedikálnak".
     
             <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
               <AddressSpace>
                 <AddressPrefix>10.17.159.192/26</AddressPrefix>
               </AddressSpace>
               <Subnets>
                 <Subnet name="Subnet-1">
                   <AddressPrefix>10.17.159.192/27</AddressPrefix>
                 </Subnet>
                 <Subnet name="GatewaySubnet">
                   <AddressPrefix>10.17.159.224/27</AddressPrefix>
                 </Subnet>
               </Subnets>
               <Gateway>
                 <ConnectionsToLocalNetwork>
                   <LocalNetworkSiteRef name="MyLocalNetwork">
                     <Connection type="Dedicated" />
                   </LocalNetworkSiteRef>
                 </ConnectionsToLocalNetwork>
               </Gateway>
             </VirtualNetworkSite>
3. Miután létrehozásához, és az XML-sémafájl konfigurálásához, hello-fájl feltöltése. Ezzel létrejön a virtuális hálózat.
   
    A következő parancsmag tooupload hello a fájlt, hello érték helyett a saját használja.
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <a name="gw"></a>Hozzon létre egy ExpressRoute-átjárót. Lehet, hogy toospecify, GatewaySKU hello *szabványos*, *HighPerformance*, vagy *UltraPerformance* és hello GatewayType, *DynamicRouting*.
   
    A következő mintát, és hello értékeket a saját hello használata.
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. Hello ExpressRoute átjáró toohello ExpressRoute-kapcsolatcsoportot hivatkozásra. Ez a lépés befejezése után, a helyszíni hálózat között az Azure ExpressRoute, keresztül hello kapcsolat létrejött.
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <a name="vpngw"></a>Ezután hozza létre a webhelyek közötti VPN-átjárót. hello GatewaySKU kell *szabványos*, *HighPerformance*, vagy *UltraPerformance* és hello GatewayType kell *DynamicRouting*.
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    tooretrieve hello virtuális hálózati átjáró beállításának, köztük hello Átjáróazonosító és hello nyilvános IP-cím, használja a hello `Get-AzureVirtualNetworkGateway` parancsmag.
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for hello following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded
7. Hozzon létre egy helyi VPN-átjáró entitást. Ez a parancs nem konfigurálja a helyszíni VPN-átjárót. Ehelyett tooprovide hello helyi átjáró beállításai, például a hello nyilvános IP-cím lehetővé teszi, hello helyszíni címtér, így hello Azure VPN-átjáró képes kapcsolódni tooit.
   
   > [!IMPORTANT]
   > hello hello telephelyek közötti VPN a hely nincs definiálva a hello netcfg. Ehelyett ez a parancsmag toospecify hello helyi paraméterek kell használnia. Nem lehet definiálni, vagy a portálon, vagy hello netcfg fájl használatával.
   > 
   > 
   
    A következő mintát, hello értékeket cserélje le a saját hello használata.
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > Ha a helyi hálózat több útvonalat is tartalmaz, tömbként egyszerre mindet megadhatja.  $MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")  
   > 
   > 

    tooretrieve hello virtuális hálózati átjáró beállításának, köztük hello Átjáróazonosító és hello nyilvános IP-cím, használja a hello `Get-AzureVirtualNetworkGateway` parancsmag. Tekintse meg a következő példa hello.

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. A helyi VPN-eszköz tooconnect toohello új átjáró konfigurálása. A VPN-eszköz konfigurálásakor az 6. lépésben lekért hello információkat használja. A VPN-eszköz konfigurálásával kapcsolatos további információkért lásd: [VPN-eszköz konfigurálása](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
2. Hivatkozás hello telephelyek közötti VPN átjáró Azure toohello helyi átjáró.
   
    Ebben a példában a connectedEntityId: hello helyi átjáró azonosítója, amely futtatásával `Get-AzureLocalNetworkGateway`. Hello segítségével virtualNetworkGatewayId található `Get-AzureVirtualNetworkGateway` parancsmag. Ez a lépés után hello telephelyek közötti VPN-kapcsolaton keresztül a helyi hálózat és az Azure közötti hello kapcsolat létrejött.

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <a name="add"></a>egy már meglévő virtuális hálózatot tooconfigure coexsiting kapcsolatok
Ha egy meglévő virtuális hálózattal rendelkezik, ellenőrizze a hello átjáró alhálózati méretét. Ha hello átjáróalhálózatot /28 vagy /29, először hello virtuális hálózati átjáró törlése és hello átjáró alhálózati méretének növelése. hello lépéseket ebben a szakaszban megtudhatja, hogyan toodo, amely.

Ha hello átjáróalhálózatot /27 vagy nagyobb és hello virtuális hálózati ExpressRoute keresztül csatlakozik, hello lépéseket kihagyhatja, és folytassa túl["6. lépés – --webhelyek közötti VPN átjáró létrehozása"](#vpngw) hello előző szakaszban.

> [!NOTE]
> Meglévő átjáró hello törlésekor a helyi helyi hello kapcsolat tooyour virtuális hálózati elvész, ezt a konfigurációt a munka során.
> 
> 

1. Tooinstall hello legújabb verziójára hello Azure Resource Manager PowerShell-parancsmagok lesz szüksége. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt. Előfordulhat, hogy ehhez a konfigurációhoz fogjuk hello parancsmagok némileg eltérő mi, előfordulhat, hogy ismernie kell. Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat. 
2. Hello meglévő expressroute-on vagy a telephelyek közötti VPN-átjáró törlése. A következő parancsmag, hello értékeket cserélje le a saját hello használata.
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. Hello virtuális hálózati séma exportálása. A következő PowerShell-parancsmag, hello értékeket cserélje le a saját hello használata.
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. Hello hálózati konfigurációs fájl séma módosítsa hello átjáróalhálózatot, de /27 egy rövidebb előtaggal (például /26 vagy /25). Tekintse meg a következő példa hello. 
   
   > [!NOTE]
   > Ha még nem rendelkezik elegendő a virtuális hálózati tooincrease hello átjáró alhálózat méretét az IP-címek, meg kell tooadd további IP-címterület. Hello konfigurációs séma kapcsolatos további információkért lásd: [Azure Virtual Network konfigurációs séma](https://msdn.microsoft.com/library/azure/jj157100.aspx).
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. Ha az előző átjáró volt a telephelyek közötti VPN, módosítania kell hello kapcsolat típusa túl**dedikált**.
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. Ezen a ponton egy átjáró nélküli VNettel rendelkezik. új toocreate-átjárók és a kapcsolatokat, folytassa a [4. lépés – hozzon létre egy ExpressRoute-átjáró](#gw), míg a talált a fenti lépéseket hello.

## <a name="next-steps"></a>Következő lépések
ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md)

