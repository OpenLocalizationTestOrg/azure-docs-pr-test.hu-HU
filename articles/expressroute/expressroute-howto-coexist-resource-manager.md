---
title: "Párhuzamosan fennálló ExpressRoute- és helyek közötti VPN-kapcsolatok konfigurálása: Resource Manager: Azure | Microsoft Docs"
description: "A cikk bemutatja az ExpressRoute- és egy helyek közötti VPN-kapcsolat konfigurálását, amelyek párhuzamosan használhatók a Resource Manager-alapú üzemi modellben."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: efda9f89d95617c8c4e75af91b20631dc468d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Párhuzamos ExpressRoute- és párhuzamos telephelyközi kapcsolatok konfigurálása
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Klasszikus](expressroute-howto-coexist-classic.md)
> 
> 

Az egyidejű helyek közötti VPN- és ExpressRoute-kapcsolatok konfigurálása több előnnyel jár. Biztonságos feladatátvételi elérési útnak a telephelyek közötti VPN konfigurálása ExressRoute, vagy használja a pont-pont VPN tooconnect toosites nem ExpressRoute keresztül csatlakozó. Mindkét forgatókönyvet ebben a cikkben azt fedezi hello lépéseket tooconfigure. Ez a cikk toohello Resource Manager üzembe helyezési modellben vonatkozik, és használja a PowerShell. Ez a konfiguráció hello Azure-portál nem érhető el.

> [!IMPORTANT]
> ExpressRoute-Kapcsolatcsoportok előre konfigurálni kell az alábbi hello utasítások végrehajtása előtt. Győződjön meg arról, hogy követte hello útmutatók túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és [konfigurálja az útválasztást](expressroute-howto-routing-arm.md) folytatás előtt.
> 
> 

## <a name="limits-and-limitations"></a>Korlátok és korlátozások
* **A tranzit útválasztás nem támogatott.** Nem hajthat végre útválasztást (az Azure-on keresztül) a helyek közötti VPN használatával csatlakoztatott helyi hálózat és az ExpressRoute használatával csatlakoztatott helyi hálózat között.
* **A Basic SKU-átjáró nem támogatott.** Nem alapszintű Termékváltozat átjáró kell használnia az mindkét hello [ExpressRoute-átjáró](expressroute-about-virtual-network-gateways.md) és hello [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Kizárólag az útvonalalapú VPN-átjáró támogatott.** Útvonalalapú [VPN-átjárót](../vpn-gateway/vpn-gateway-about-vpngateways.md) kell használnia.
* **A VPN-átjáróhoz statikus útvonalat kell konfigurálni.** Ha a helyi hálózathoz csatlakoztatott tooboth ExpressRoute és a pont-pont VPN, rendelkeznie kell a helyi hálózati tooroute hello pont-pont VPN-kapcsolat toohello konfigurált statikus útvonal nyilvános internethez.
* **ExpressRoute-átjárót először meg kell adni, és a kapcsolódó tooa körön.** Meg kell hello ExpressRoute-átjárót először hozza létre, és tooa áramkör hello pont-pont VPN-átjáró hozzáadása előtt.

## <a name="configuration-designs"></a>Konfigurációs tervek
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Helyek közötti VPN konfigurálása feladatátvételi útvonalként az ExpressRoute számára
Konfigurálhat egy helyek közötti VPN-kapcsolatot tartalékként az ExpressRoute számára. Ez vonatkozik csak toovirtual hálózatok csatolt toohello Azure magánhálózati társviszony-létesítési elérési útja. Az Azure nyilvános és a Microsoft társviszony-létesítésekhez nem létezik VPN-alapú feladatátvételi megoldás. hello ExpressRoute-kapcsolatcsoportot mindig hello elsődleges kapcsolat. Adatáramlás hello telephelyek közötti VPN elérési útján csak akkor, ha ExpressRoute-kapcsolatcsoportot hello sikertelen.

> [!NOTE]
> Habár ExpressRoute-kapcsolatcsoportot pont-pont VPN-kapcsolaton keresztül előnyben részesített a mindkét útvonalak vannak hello azonos, Azure használandó hello leghosszabb előtag egyezés toochoose hello útvonal hello csomagok cél felé.
> 
> 

![Egyidejű jelenlét](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a>Egy ExpressRoute keresztül nem kapcsolódik telephelyek közötti VPN-tooconnect toosites konfigurálása
Beállíthatja, hogy a hálózat, ahol egyes helyek csatlakoznak közvetlenül tooAzure pont-pont VPN-kapcsolaton keresztül, és az egyes helyek ExpressRoute keresztül csatlakoznak. 

![Egyidejű jelenlét](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Virtuális hálózatot nem konfigurálhat tranzit útválasztóként.
> 
> 

## <a name="selecting-hello-steps-toouse"></a>Hello lépéseket toouse kiválasztása
Az eljárások toochoose két csoportja van. kiválasztott hello konfigurációs eljárás attól függ, hogy van-e egy meglévő virtuális hálózatot, hogy azt szeretné, hogy tooconnect, vagy azt szeretné, hogy egy új virtuális hálózat toocreate.

* Nem rendelkezik egy VNet és egy toocreate kell.
  
    Ha még nem rendelkezik virtuális hálózattal, ez az eljárás lépésről lépésre végigvezeti az új virtuális hálózat létrehozásának folyamatán a Resource Manager-alapú üzemi modellnek megfelelően, valamint az új ExpressRoute- és helyek közötti VPN-kapcsolatok létrehozásának folyamatán. egy virtuális hálózati tooconfigure kövesse hello [toocreate egy új virtuális hálózat és vizsgálatát a kísérő kapcsolatok](#new).
* Már rendelkezem egy Resource Manager-alapú üzemi modell szerinti VNettel.
  
    Előfordulhat, hogy már rendelkezik üzemelő virtuális hálózattal egy létező helyek közötti VPN- vagy ExpressRoute-kapcsolattal. Hello [tooconfigure vizsgálatát a kísérő kapcsolatok egy már meglévő vnet](#add) szakasz végigvezeti hello átjáró törlése folyamatban, és ezután az új ExpressRoute és a pont-pont VPN-kapcsolatok létrehozására. Amikor hello hoz létre új kapcsolatokat, meghatározott sorrendben hello a lépéseket kell végrehajtania. Ne használjon más cikkek toocreate hello utasításait, az átjárók és kapcsolatok.
  
    Ebben az eljárásban, amely egyszerre is használható kapcsolatok létrehozására, akkor toodelete igényel az átjáró, és adja meg új átjárók. Hogy állásidő a létesítmények közötti kapcsolatok során törölje és hozza létre újra az átjárót és a kapcsolatok, de nem kell toomigrate bármely, a virtuális gépek vagy szolgáltatások tooa új virtuális hálózat. A virtuális gépek és szolgáltatások is ki tudja toocommunicate keresztül hello terheléselosztó amíg az átjáró konfigurálásának, ha így konfigurálva toodo.

## <a name="new"></a>új virtuális hálózat toocreate és vizsgálatát a kísérő kapcsolatok
Az eljárás a VNetek, valamint az egyidejűleg jelenlévő helyek közötti és ExpressRoute-kapcsolatok létrehozásának módját ismerteti.

1. Hello hello Azure PowerShell-parancsmagok legújabb verziójának telepítéséhez. Hello parancsmagok telepítésével kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). lehet, hogy mi, előfordulhat, hogy ismernie kell a kis mértékben eltérő hello parancsmagok, amelyekkel ehhez a konfigurációhoz. Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat.
2. Jelentkezzen be tooyour fiókot, és hello környezet beállítása.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Hozzon létre egy virtuális hálózatot az átjáró-alhálózattal együtt. Hello virtuális hálózati konfigurációjával kapcsolatos további információkért lásd: [Azure virtuális hálózat konfigurálása](../virtual-network/virtual-networks-create-vnet-arm-ps.md).
   
   > [!IMPORTANT]
   > Átjáró alhálózati hello /27 vagy egy rövidebb előtaggal (például /26 vagy /25) kell lennie.
   > 
   > 
   
    Hozzon létre egy új VNetet.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Adjon hozzá alhálózatokat.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Hello VNet-konfiguráció mentéséhez.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Hozzon létre egy ExpressRoute-átjárót. Hello ExpressRoute-átjáró konfigurációjával kapcsolatos további információkért lásd: [ExpressRoute-átjáró konfigurációs](expressroute-howto-add-gateway-resource-manager.md). hello GatewaySKU kell *szabványos*, *HighPerformance*, vagy *UltraPerformance*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Hello ExpressRoute átjáró toohello ExpressRoute-kapcsolatcsoportot hivatkozásra. Ez a lépés befejezése után, a helyszíni hálózat között az Azure ExpressRoute, keresztül hello kapcsolat létrejött. Hello hivatkozás művelettel kapcsolatos további információkért lásd: [hivatkozás Vnetek tooExpressRoute](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Ezután hozza létre a webhelyek közötti VPN-átjárót. Hello VPN gateway konfigurációjával kapcsolatos további információkért lásd: [egy virtuális hálózat konfigurálása webhelyek kapcsolattal rendelkező](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). hello GatewaySKU kell *szabványos*, *HighPerformance*, vagy *UltraPerformance*. hello VpnType kell *RouteBased*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Az Azure-os VPN-átjáró támogatja a BGP útválasztási protokollt. Megadhatja az ASN (AS-szám) virtuális hálózat hello - Asn kapcsoló hello a következő parancs hozzáadásával. Nem adja meg, hogy paramétert fog alapértelmezett tooAS number 65515.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    BGP társviszony-létesítést IP hello és hello Azure által a VPN-átjáró hello $azureVpn.BgpSettings.BgpPeeringAddress és $azureVpn.BgpSettings.Asn SZÁMOT. További információk: [A BGP konfigurálása](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) az Azure-alapú VPN-átjáróhoz.
7. Hozzon létre egy helyi VPN-átjáró entitást. Ez a parancs nem konfigurálja a helyszíni VPN-átjárót. Ehelyett tooprovide hello helyi átjáró beállításai, például a hello nyilvános IP-cím lehetővé teszi, hello helyszíni címtér, így hello Azure VPN-átjáró képes kapcsolódni tooit.
   
    Ha a helyi VPN-eszköz csak akkor támogatja a statikus útválasztást, konfigurálhatja hello statikus útvonalak a következő módon hello:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Ha a helyi VPN-eszköz támogatja a hello BGP és azt szeretné, hogy a dinamikus útválasztási tooenable, kell tooknow hello BGP társviszony-létesítés IP-cím és hello, szám, amely a helyi VPN-eszközt használ.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for hello BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. A helyi VPN-eszköz tooconnect toohello új Azure VPN átjáró konfigurálásához. A VPN-eszköz konfigurálásával kapcsolatos további információkért lásd: [VPN-eszköz konfigurálása](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
9. Hivatkozás hello telephelyek közötti VPN átjáró Azure toohello helyi átjáró.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>egy már meglévő virtuális hálózatot tooconfigure vizsgálatát a kísérő kapcsolatok
Ha egy meglévő virtuális hálózattal rendelkezik, ellenőrizze a hello átjáró alhálózati méretét. Ha hello átjáróalhálózatot /28 vagy /29, először hello virtuális hálózati átjáró törlése és hello átjáró alhálózati méretének növelése. hello lépésekből ebben a szakaszban megtudhatja, hogyan toodo, amely.

Ha hello átjáróalhálózatot /27 vagy nagyobb és hello virtuális hálózati ExpressRoute keresztül csatlakozik, hello lépéseket kihagyhatja, és folytassa túl["6. lépés – --webhelyek közötti VPN átjáró létrehozása"](#vpngw) hello előző szakaszban. 

> [!NOTE]
> Meglévő átjáró hello törlésekor a helyi helyi hello kapcsolat tooyour virtuális hálózati elvész, ezt a konfigurációt a munka során. 
> 
> 

1. Tooinstall hello legújabb verziójára hello Azure PowerShell-parancsmagok lesz szüksége. Parancsmagok telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). lehet, hogy mi, előfordulhat, hogy ismernie kell a kis mértékben eltérő hello parancsmagok, amelyekkel ehhez a konfigurációhoz. Adható meg, hogy toouse hello parancsmagok ezeket az utasításokat. 
2. Hello meglévő expressroute-on vagy a telephelyek közötti VPN-átjáró törlése.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Törölje az átjáró-alhálózatot.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Adjon hozzá egy átjáró-alhálózatot, amely legfeljebb /27.
   
   > [!NOTE]
   > Ha még nem rendelkezik elegendő a virtuális hálózati tooincrease hello átjáró alhálózat méretét az IP-címek, meg kell tooadd további IP-címterület.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Hello VNet-konfiguráció mentéséhez.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. Ezen a ponton egy átjáró nélküli VNettel rendelkezik. új toocreate-átjárók és a kapcsolatokat, folytassa a [4. lépés – hozzon létre egy ExpressRoute-átjáró](#gw), míg a talált a fenti lépéseket hello.

## <a name="tooadd-point-to-site-configuration-toohello-vpn-gateway"></a>tooadd pont-hely konfigurációs toohello VPN-átjáró
A lépésekkel hello tooadd pont-hely konfigurációs tooyour VPN-átjáró egy létezzenek beállítás alatt.

1. Adja hozzá a VPN-ügyfél címterét.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Töltse fel a hello VPN legfelső szintű tanúsítvány tooAzure a VPN-átjárót. Ebben a példában feltételezzük hello legfelső szintű tanúsítvány a következő PowerShell-parancsmagok hello futtatják hello helyi számítógép tárolja.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

A pont-hely VPN-ekkel kapcsolatos további információkért lásd: [Pont-hely kapcsolat konfigurálása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Következő lépések
ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
