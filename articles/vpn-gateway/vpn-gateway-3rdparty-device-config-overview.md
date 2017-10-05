---
title: "3. fél VPN-eszköz konfigurációjában csatlakozni az Azure VPN gatewayek kapcsolatos |} Microsoft Docs"
description: "Ez a cikk áttekintést 3. fél VPN eszközkonfigurációk Azure VPN gatewayek való kapcsolódáshoz."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 72dab85bb882b05d72cef26bef70437695b70416
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-3rd-party-vpn-device-configurations"></a>3. fél VPN eszközkonfigurációk áttekintése
Ez a cikk áttekintést a helyszíni VPN eszközkonfigurációk Azure VPN gatewayek való kapcsolódáshoz. A minta Azure-beli virtuális hálózat és a VPN-átjáró telepítési használható különböző helyszíni VPN-eszközök az ugyanezen paraméterekkel rendelkező való kapcsolódáshoz.

## <a name="device-requirements"></a>Eszközkövetelmények
Az Azure VPN gatewayek szabványos IPsec/IKE protokoll csomagok használata S2S VPN-alagutat. Tekintse meg [kapcsolatos VPN-eszközök](vpn-gateway-about-vpn-devices.md) részletes IPsec/IKE protokoll paraméterek és alapértelmezett titkosítási algoritmusok az Azure VPN gatewayek. Opcionálisan megadhat a titkosítási algoritmusok és egy adott kapcsolat kulcs szintjeiről pontos kombinációja a leírtak szerint [titkosítási követelményekkel kapcsolatos](vpn-gateway-about-compliance-crypto.md).

## <a name ="singletunnel"></a>Egy VPN-alagút
Az első topológia áll az Azure VPN gateway és a helyszíni VPN-eszköz között egyetlen S2S VPN-alagúton. A VPN-alagúton keresztül nem kötelező beállítani a BGP Protokollt.

![az egyalagutas](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Tekintse meg [webhelyek kapcsolat beállítása](vpn-gateway-howto-site-to-site-resource-manager-portal.md) részletes, lépésenkénti útmutatót. Az alábbi szakaszok felsorolják a paraméterek, és adjon meg egy PowerShell-parancsfájlpélda a segítségére lesz.

### <a name="network-and-vpn-gateway-information"></a>Hálózati és a VPN-átjáró adatai
Ez a szakasz a paramétereket a fenti példák a listában.

| **A paraméter**                | **Érték**                    |
| ---                          | ---                          |
| VNet-címelőtagjait        | 10.11.0.0/16<br>10.12.0.0/16 |
| Az Azure VPN gateway IP-címet         | Az Azure VPN Gateway IP         |
| A helyszíni címelőtagokat | 10.51.0.0/16<br>10.52.0.0/16 |
| A helyszíni VPN-eszköz IP-címet    | A helyszíni VPN-eszköz IP-címet    |
| * VNet BGP ASN                | 65010                        |
| * Az azure BGP-Társgép IP-           | 10.12.255.30                 |
| * A helyszíni BGP ASN         | 65050                        |
| * A helyszíni BGP-Társgép IP-     | 10.52.255.254                |

* (*) Választható paraméterek: a BGP csak

### <a name="sample-powershell-script"></a>PowerShell-parancsfájlpélda
[PowerShell-lel S2S VPN-kapcsolatot](vpn-gateway-create-site-to-site-rm-powershell.md) részletes utasításokat tartalmaz. Ez a témakör egy minta parancsfájlt az első lépésekhez.

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect to your subscription and create a new resource group

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create the S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name ="policybased"></a>[Választható] A "UsePolicyBasedTrafficSelectors" egyéni IPsec/IKE házirend
Ha a VPN-eszközök nem támogatják a "bármely közöttiként" forgalom választók (útválasztó-alapú/VTI-alapú configuration), akkor egyéni IPsec/IKE-házirend létrehozása és konfigurálása "UsePolicyBasedTrafficSelectors" beállítást, a [Ez a cikk ](vpn-gateway-connect-multiple-policybased-rm-ps.md).

> [!IMPORTANT]
> IPsec/IKE-házirend létrehozása a kapcsolat "UsePolicyBasedTrafficSelectors" beállítást engedélyezni kell.

Az alábbi mintaparancsfájl házirendet hoz létre IPsec/IKE a következő algoritmusokat és a Paraméterek:
* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA1, PFS24, SA élettartama 7200 másodperc & 20480000KB (20GB)

Ezután a házirend vonatkozik, és lehetővé teszi, hogy a "UesPolicyBasedTrafficSelectors" a kapcsolaton.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>[Választható] A BGP S2S VPN-kapcsolat használata
Is használhat BGP a kapcsolaton. Lásd: [VPN Gateway a BGP](vpn-gateway-bgp-resource-manager-ps.md). Két különbségek vannak:

A helyszíni címelőtagokat lehet egy állomáshoz címet, a helyszíni BGP-Társgép IP-címe:

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

Meg kell adni "-EnableBGP" $true, ha a kapcsolat létrehozásakor:

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a>Következő lépések
A létesítmények közötti és a virtuális hálózatok közötti aktív-aktív kapcsolatok konfigurálásának lépéseit lásd: [Aktív-aktív VPN Gateway-átjárók konfigurálása létesítmények közötti és virtuális hálózatok közötti kapcsolatokhoz](vpn-gateway-activeactive-rm-powershell.md).

