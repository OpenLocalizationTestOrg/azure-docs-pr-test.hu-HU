---
title: "Az Azure-webhelyek kapcsolatok kényszerített bújtatás konfigurálása: erőforrás-kezelő |} Microsoft Docs"
description: "Hogyan tooredirect vagy \"kényszerített\" az összes internetes adathoz kötött hátsó forgalom-tooyour helyszíni hely."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 6bc52c04ab0749a674c9863be5e4f9a9f7c98df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-azure-resource-manager-deployment-model"></a>Konfigurálja a kényszerített bújtatás használata hello Azure Resource Manager telepítési modell

A kényszerített bújtatás lehetővé átirányítási vagy "kényszerített" minden Internet adathoz kötött forgalom hátsó tooyour helyszíni hely vizsgálati és naplózási pont-pont VPN-alagúton keresztül. Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek. Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban mindig halad át közvetlenül kimenő Internet, toohello nélkül hello beállítás tooallow Azure hálózati infrastruktúráról, tooinspect vagy a napló hello forgalmat. Jogosulatlan Internet-hozzáférés is eredményezhet, tooinformation nyilvánosságra vagy más típusú biztonsági problémákat.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Ez a cikk bemutatja, hogyan konfigurálása, a kényszerített bújtatás hello Resource Manager üzembe helyezési modellben használatával létrehozott virtuális hálózatokat. A kényszerített bújtatás powershellel hello portálon keresztül nem konfigurálható. Ha a kényszerített bújtatás hello klasszikus telepítési modell tooconfigure, jelölje be a klasszikus cikk a legördülő listából a következő hello:

> [!div class="op_single_selector"]
> * [PowerShell – Klasszikus](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Hamarosan kényszerített bújtatás

hello a következő ábra bemutatja, hogyan kényszerített bújtatás működik. 

![Alagúthasználat kényszerítése](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Hello a fenti példában a Frontend alhálózathoz nem kényszeríti bújtatott hello. hello munkaterhelések hello Frontend alhálózaton továbbra is tooaccept és hello Internet toocustomer kérelmek közvetlenül válaszol. hello közepes réteg és a háttérkiszolgáló alhálózatok kényszerítve vannak-e bújtatott. A kimenő kapcsolatokat, az alábbi két alhálózat toohello Internet lesz kényszerített vagy átirányított vissza tooan hely helyszíni S2S VPN-je bújtatja hello keresztül.

Ez lehetővé teszi a toorestrict és vizsgálhatja meg a virtuális gépek Internet-hozzáféréssel, vagy felhőalapú szolgáltatások Azure, miközben továbbra tooenable a többrétegű szolgáltatások architektúra szükséges. Ha a virtuális hálózatok egyetlen internetre munkaterhelés, is alkalmazhat a kényszerített bújtatás toohello teljes virtuális hálózatok.

## <a name="requirements-and-considerations"></a>Követelmények és szempontok

Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak keresztül. Irányít át forgalmat tooan helyszíni hely alapértelmezett útvonalat toohello Azure VPN-átjáróként van kifejezve. További információ a felhasználó által definiált útválasztást és a virtuális hálózatok: [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md).

* Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához. hello rendszer útvonaltábla rendelkezik a következő három csoport útvonalak hello:
  
  * **Helyi VNet útvonalak:** közvetlenül toohello cél virtuális gépek hello az azonos virtuális hálózatban.
  * **A helyi útvonalak:** toohello Azure VPN gateway.
  * **Alapértelmezett útvonal:** közvetlenül toohello Internet. Eldobott csomagok toohello magánhálózati IP-címek nem fedi le hello előző két útvonalak.
* Ezt az eljárást használja a felhasználó által definiált útvonalak (UDR) toocreate útválasztási táblázat tooadd alapértelmezett útvonalat, és társíthatja az útválasztási táblázat tooyour VNet alhálózat tooenable kényszerített bújtatás ezek alhálózatok hello.
* A kényszerített bújtatás kell rendelni egy Vnetet, amelynek az útvonalalapú VPN-átjáró. Egy "alapértelmezett"nevű hely tooset kell hello létesítmények közötti helyi helyek csatlakoztatott toohello virtuális hálózat között. Emellett hello a helyszíni VPN-eszköz 0.0.0.0/0 forgalom választók használatával kell konfigurálni. 
* A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet keresztül hello ExpressRoute BGP társviszony-létesítési munkameneteket. További információkért lásd: hello [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Konfigurálása – áttekintés

hello a következő eljárás segítségével hozhat létre, az erőforráscsoportot és egy virtuális hálózatot. Majd hozhat létre a VPN-átjáró, és konfigurálja a kényszerített bújtatást. Ezzel az eljárással hello virtuális hálózati MultiTier – VNet három alhálózatot tartalmaz: "Előtér", "Midtier" és "Háttér", a négy létesítmények közötti kapcsolatok: "DefaultSiteHQ", és három ágak.

hello lépésekkel állítsa be "DefaultSiteHQ" hello, hello alapértelmezett webhely kapcsolat a kényszerített bújtatás, és hello "Midtier" és "Háttér" alhálózatok toouse kényszerített bújtatás konfigurálásakor.

## <a name="before-you-begin"></a>Előkészületek

Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.

### <a name="toolog-in"></a>a toolog

[!INCLUDE [toolog in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Kényszerített bújtatás konfigurálása

1. Hozzon létre egy erőforráscsoportot.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Hozzon létre egy virtuális hálózatot, és adja meg az alhálózatot.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Hello helyi hálózati átjáró létrehozása.

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. Hello útvonal tábla- és útvonal-szabály létrehozása.

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. Hello útvonal tábla toohello Midtier és a háttérkiszolgáló alhálózatok társítása.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Hozzon létre egy alapértelmezett webhelyet hello átjáró. Ez a lépés bizonyos idő toocomplete, néha 45 percig vagy tovább vesz igénybe, mert létrehozandó és hello átjáró konfigurálása.<br> Hello **- GatewayDefaultSite** , amely lehetővé teszi, hogy hello kényszerített útválasztási konfigurációs toowork parancsmag-paraméterben hello, így telhet, mire gondot tooconfigure megfelelően ezt a beállítást. Ez a paraméter értéke a PowerShell 1.0-s vagy újabb.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. Pont-pont hello VPN-kapcsolatok létesítéséhez.

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```