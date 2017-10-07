---
title: "aaaVPN átjáró beállításainak létesítmények közötti Azure-kapcsolatok |} Microsoft Docs"
description: "További tudnivalók az Azure virtuális hálózati átjárók VPN-átjáró beállításainak."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 01229d99fa37e30e00aa00f939f488d631b5593c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a>VPN-átjáró konfigurációs beállításairól

VPN-átjáró olyan virtuális hálózati átjáró által a titkosított forgalmat a virtuális hálózat és a helyszíni hely közötti nyilvános kapcsolaton keresztül. A VPN-átjáró toosend forgalmát hello Azure gerincét között virtuális hálózatok közötti is használható.

VPN gateway-kapcsolattal konfigurálható beállításokat tartalmaz, amelyek mindegyike több erőforrás konfigurációjának hello támaszkodik. Ez a cikk hello részeiben hello erőforrások és tooa VPN-átjáró a Resource Manager üzembe helyezési modellel létrehozott virtuális hálózatban a szoftverközponthoz kapcsolódó beállításokat ismertetik. Minden kapcsolat megoldás hello megtalálhatja leírásokat és topológiai diagramot [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikk.

## <a name="gwtype"></a>Átjáró típusa

Minden egyes virtuális hálózati csak egy virtuális hálózati átjáró típusonkénti van. Ha a virtuális hálózati átjáró hoz létre, győződjön meg arról, hogy helyesek-e hello átjáró típusa a konfigurációhoz.

hello érhető el - GatewayType értékei:

* Vpn
* ExpressRoute

A VPN-átjáró által igényelt hello `-GatewayType` *Vpn*.

Példa:

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gwsku"></a>Átjáró-termékváltozatok

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-hello-gateway-sku"></a>Hello gateway SKU konfigurálása

#### <a name="azure-portal"></a>Azure Portal

Az Azure portál toocreate a Resource Manager virtuális hálózati átjáró hello használatakor választhatja hello gateway SKU hello legördülő lista használatával. hello beállítások lehetősége lesz toohello átjáró típusa és a választott VPN-típus felel meg.

#### <a name="powershell"></a>PowerShell

hello következő PowerShell-példa meghatározza hello `-GatewaySku` VpnGw1 szerint.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="resize"></a>Módosítás (átméretezési) egy átjáró-Termékváltozat

Ha azt szeretné, tooupgrade az átjáró-Termékváltozat tooa nagyobb teljesítményű Termékváltozat hello használható `Resize-AzureRmVirtualNetworkGateway` PowerShell-parancsmagot. Visszaállítható hello átjáró Termékváltozat-méretét a parancsmag segítségével is.

a következő PowerShell-példa hello mutatja egy átjáró SKU átméretezett tooVpnGw2.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <a name="connectiontype"></a>Kapcsolat típusai

Hello Resource Manager üzembe helyezési modellel minden egyes konfiguráció szükséges egy adott virtuális hálózati átjáró kapcsolattípus. hello érhető el erőforrás-kezelő PowerShell értékei `-ConnectionType` vannak:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

A következő PowerShell-példa hello, hello kapcsolattípus igénylő S2S kapcsolatot létrehozhatunk *IPsec*.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpntype"></a>VPN-típusai

Hello virtuális hálózati átjáró VPN-átjáró konfigurációt létrehozásakor meg kell adnia egy VPN-típus. VPN-típus az Ön által hello függ hello kapcsolat topológia, amelyet az toocreate. P2S kapcsolatot például egy RouteBased VPN típust igényli. A VPN-típus hello hardver által használt is függ. S2S-konfigurációk esetén van szükség, a VPN-eszközön. VPN-eszközök csak egy meghatározott VPN-típus támogatja.

VPN-típus kiválasztott hello meg kell felelnie a összes hello kapcsolat hello megoldás követelményei toocreate szeretné. Például, ha azt szeretné, hogy toocreate egy S2S VPN gateway-kapcsolatot és a P2S VPN gateway-kapcsolattal hello azonos virtuális hálózatban, VPN-típus használhatja *RouteBased* mert P2S egy RouteBased VPN-típus szükséges. Módosítania kell, hogy a VPN-eszköz támogatja-e RouteBased VPN-kapcsolat tooverify. 

A virtuális hálózati átjáró létrehozása után hello VPN típusa nem módosítható. Lehetősége van toodelete hello virtuális hálózati átjáró, és hozzon létre egy újat. Kétféle VPN-típus létezik:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

hello következő PowerShell-példa meghatározza hello `-VpnType` , *RouteBased*. Amikor egy átjáró hoz létre, meg kell győződnie arról, hogy hello – VpnType a konfigurációja helyességéről.

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="requirements"></a>Átjáró követelményei

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gwsub"></a>Átjáró-alhálózatot

VPN-átjáró létrehozása előtt létre kell hoznia egy átjáró-alhálózatot. hello átjáróalhálózatot hello IP-cím szerepel, hogy hello virtuális hálózati átjáró virtuális gépek és szolgáltatások használatát. A virtuális hálózati átjáró létrehozásakor átjáró virtuális gépeket telepített toohello átjáró-alhálózatot, és szükséges hello VPN gateway beállításokat. Semmi másnak (például további virtuális gépek) toohello átjáróalhálózatot soha nem kell telepítenie. hello átjáróalhálózatot névvel kell ellátni "GatewaySubnet" toowork megfelelően. Hello átjáró alhálózatának elnevezése "GatewaySubnet" megadásával engedélyezhető Azure arról, hogy ez hello alhálózati toodeploy hello virtuális hálózati átjáró virtuális gépek és szolgáltatások.

Amikor hello átjáró-alhálózatot hoz létre, megadhatja a hello száma alhálózat hello IP-címeket tartalmaz. hello átjáróalhálózatot hello IP-címek lefoglalt toohello átjáró virtuális gépek és szolgáltatások átjáró. Bizonyos konfigurációk több IP-címre, mint a többi szükséges. Szeretné, hogy toocreate, és győződjön meg arról, hogy azt szeretné, hogy toocreate hello átjáróalhálózatot megfelel ezeknek a követelményeknek hello konfigurációs hello kapcsolatos útmutatásért tekintse meg. Emellett érdemes lehet, hogy az átjáró-alhálózatot tartalmaz elég IP-címek tooaccommodate lehetséges jövőbeli további konfigurációk toomake. Létrehozhat egy átjáró-alhálózat mérete /29 legyen, de javasolt, hogy hozzon létre egy átjáró-alhálózatot /28 vagy nagyobb (/ 28, /27, /26 stb.). Így ha a jövőbeli, hello funkciókkal, nem rendelkezik tootear az átjáró, majd törölje és hozza létre hello átjáró alhálózati tooallow további IP-címekhez.

hello következő erőforrás-kezelő PowerShell példa bemutatja egy GatewaySubnet nevű átjáró-alhálózatot. Hello CIDR-jelöléssel Meghatározza, hogy egy /27 lehetővé teszi, hogy elegendő IP-címek a jelenleg létező legtöbb konfiguráció látható.

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="lng"></a>Helyi hálózati átjáró

A VPN-átjáró konfigurációs létrehozásakor hello helyi hálózati átjáró gyakran jelöli a helyszíni hely. Hello klasszikus üzembe helyezési modellel hello helyi hálózati átjáró volt a hivatkozott tooas a helyi webhelyhez. 

Nevezze el hello helyi hálózati átjáró, hello hello a helyszíni VPN-eszköz nyilvános IP-címe és hello címelőtagokat hello helyszíni helyen található meg. Azure hello cél címelőtagokat a hálózati forgalom megvizsgálja, a helyi hálózati átjáró megadott hello konfigurációs olvas, és ennek megfelelően irányítja a csomagokat. Helyi hálózati átjáró VPN gateway-kapcsolatot használó VNet – VNet-konfigurációk is adja meg.

a következő PowerShell-példa hello hoz létre egy új helyi hálózati átjáró:

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Néha kell toomodify hello helyi hálózati átjáró beállításainak. Például ha fel vagy módosít hello címtartományt, vagy ha hello VPN-eszköz IP-címe hello megváltozik. A klasszikus vnet ezeket a beállításokat a klasszikus portálon hello hello helyi hálózatok lapon módosíthatja. Az erőforrás-kezelő, lásd: [PowerShell használatával a helyi hálózati átjáró beállításainak módosítása](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API-k és a PowerShell-parancsmagok

További technikai erőforrások és a konkrét szintaxis használatával kapcsolatos követelményeket REST API-k, a PowerShell-parancsmagok vagy az Azure CLI VPN Gateway-konfigurációk esetén lásd a következő lapok hello:

| **Klasszikus** | **Resource Manager** |
| --- | --- |
| [PowerShell](/powershell/module/azure#networking) |[PowerShell](/powershell/module/azurerm.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Nem támogatott | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Következő lépések

Használható kapcsolat konfigurációkkal kapcsolatos további információkért lásd: [VPN-átjáró](vpn-gateway-about-vpngateways.md).