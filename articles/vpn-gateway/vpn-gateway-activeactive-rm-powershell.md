---
title: "Aktív-aktív S2S VPN-kapcsolatokat a VPN-átjárók konfigurálása: Azure Resource Manager: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan aktív-aktív kapcsolatok konfigurálása az Azure VPN Gatewayek Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: yushwang
ms.openlocfilehash: 964eedc7698e42bf0e082f0105845f2a339daf57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Az Azure VPN Gatewayek aktív-aktív S2S VPN-kapcsolatok konfigurálása

Ez a cikk bemutatja, hogyan hello lépéseket toocreate aktív-aktív létesítmények közötti és VNet – VNet kapcsolatokhoz hello Resource Manager üzembe helyezési modellben és a PowerShell használatával.

## <a name="about-highly-available-cross-premises-connections"></a>Magas rendelkezésre állású létesítmények közötti kapcsolatok
tooachieve magas rendelkezésre állású létesítmények közötti és VNet – VNet-kapcsolatot, kell több VPN-átjáró telepítése és a hálózatok és az Azure közötti több párhuzamos kapcsolatok létesítéséhez. Ellenőrizze a [magas rendelkezésre álló létesítmények közötti és VNet – VNet-kapcsolatot](vpn-gateway-highlyavailable.md) kapcsolati lehetőségek és topológiájának áttekintését.

Ez a cikk bemutatja, hello fel egy aktív-aktív tooset létesítmények közötti VPN-kapcsolat és két virtuális hálózatok közötti aktív-aktív kapcsolat:

* [1. rész – létrehozása és konfigurálása az Azure VPN gateway aktív-aktív módban](#aagateway)
* [2. rész – aktív-aktív létesítmények közötti kapcsolatot.](#aacrossprem)
* [3. rész – aktív-aktív VNet – VNet-kapcsolatot létrehozni](#aav2v)
* [4. rész - frissítés meglévő átjáró aktív-aktív és aktív-készenléti állapotban lévő között](#aaupdate)

Ezek együtt toobuild egy összetettebb, magas rendelkezésre állású hálózati topológia az igényeinek megfelelő kombinálhatja.

> [!IMPORTANT]
> Vegye figyelembe, hogy hello aktív-aktív üzemmód használja a következő termékváltozatok csak hello: 
  * VpnGw1, VpnGw2, VpnGw3
  * (A régi örökölt SKU) HighPerformance
> 
> 

## <a name ="aagateway"></a>1. rész – létrehozása és aktív-aktív VPN-átjárók konfigurálása
a lépéseket követve hello konfigurálja az Azure VPN gateway aktív-aktív üzemmódban. hello hello aktív-aktív és az aktív-készenléti állapotban lévő átjárók közötti fontosabb különbségeket:

* Két átjáró IP-konfigurációk toocreate két nyilvános IP-címmel van szüksége
* Hello EnableActiveActiveFeature jelzőt kell beállítani
* hello gateway SKU VpnGw1, VpnGw2, VpnGw3 vagy HighPerformance (örökölt SKU) kell lennie.

hello más tulajdonságainak vannak hello ugyanaz, mint a hello nem aktív-aktív átjárókat. 

### <a name="before-you-begin"></a>Előkészületek
* Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
* Tooinstall hello Azure Resource Manager PowerShell-parancsmagok lesz szüksége. Lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.

### <a name="step-1---create-and-configure-vnet1"></a>1. lépés – létrehozása és VNet1 konfigurálása
#### <a name="1-declare-your-variables"></a>1. A változók deklarálása
Ezt a gyakorlatot a változók deklarálásával kezdjük. az alábbi példa hello hello értékekkel ehhez a gyakorlathoz hello változók deklarálja. Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor. Ezek a változók is használhatja, ha ismeri a konfiguráció az ilyen típusú hello lépéseket toobecome keresztül futtatja. Hello változók módosítása, majd másolja és illessze be a PowerShell-konzolban.

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Csatlakozás tooyour előfizetés, és hozzon létre egy új erőforráscsoportot
Váltson át tooPowerShell mód toouse hello erőforrás-kezelő parancsmagokat. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók. A következő minta toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. A TestVNet1 létrehozása
hello minta az alábbi nevű TestVNet1 és három alhálózatok, egy hívott GatewaySubnet, egy hívott előtér és egy hívott háttér virtuális hálózatot hoz létre. Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen. Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-active-active-mode"></a>2. lépés – hello VPN-átjáró létrehozása TestVNet1 aktív-aktív üzemmódban
#### <a name="1-create-hello-public-ip-addresses-and-gateway-ip-configurations"></a>1. Hello nyilvános IP-címek és az átjáró IP-konfigurációk létrehozása
Kérelem két nyilvános IP-címek toobe lefoglalt toohello átjáró fog létrehozni a virtuális hálózat számára. Hello alhálózatot és IP-konfiguráció szükséges is fogja definiálni.

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-hello-vpn-gateway-with-active-active-configuration"></a>2. Aktív-aktív konfigurációval hello VPN-átjáró létrehozása
TestVNet1 hello virtuális hálózati átjáró létrehozása Vegye figyelembe, hogy két GatewayIpConfig bejegyzést is tartalmaz, és hello EnableActiveActiveFeature jelző be van állítva. Átjáró létrehozása időt vehet igénybe (45 perc vagy több toocomplete).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-hello-gateway-public-ip-addresses-and-hello-bgp-peer-ip-address"></a>3. Hello átjáró nyilvános IP-címek és hello BGP-Társgép IP-cím beszerzése
Hello átjáró létrehozása után kell tooobtain hello hello Azure VPN Gateway a BGP-Társgép IP-cím. Ez a cím szükséges tooconfigure hello Azure VPN Gateway, mint a BGP-partner a helyszíni VPN-eszközök.

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

A következő parancsmagok tooshow hello két nyilvános IP-címet a VPN-átjáró és az egyes átjárópéldány megfelelő BGP-Társgép IP-címeit számára lefoglalt hello használata:

```powershell

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }
```

hello ahhoz, hogy hello példányai és a megfelelő BGP társviszony-létesítés cím hello hello nyilvános IP-címek hello azonos. Ebben a példában hello átjáró nyilvános IP-címe 40.112.190.5 rendelkező virtuális gépet a BGP társviszony-létesítés cím 10.12.255.4 fog használni, és 138.91.156.129 hello átjáró 10.12.255.5 fogja használni. Ezek az információk szükségesek beállításakor a helyszíni összekötő toohello aktív-aktív átjáró VPN-eszközök. hello átjáró összes címre alatt hello ábrán látható:

![aktív-aktív átjáró](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Hello átjáró létrehozása után a átjáró tooestablish aktív-aktív létesítmények közötti és VNet – VNet-kapcsolatot is használhatja. a következő szakaszok hello keresztül hello lépéseket toocomplete hello gyakorlat részletesen ismerteti.

## <a name ="aacrossprem"></a>2. rész – egy aktív-aktív létesítmények közötti kapcsolat
tooestablish létesítmények közötti kapcsolatot, a helyi hálózati átjáró toorepresent toocreate szüksége, és a helyszíni VPN-eszköz kapcsolat tooconnect hello Azure VPN-átjáró hello helyi hálózati átjáró. Ebben a példában hello Azure VPN gateway aktív-aktív módban van. Ennek eredményeképpen ellenére, hogy csak az egyiket a helyszíni VPN-eszközön (helyi hálózati átjáró) és egy kapcsolati erőforrást, mindkét Azure VPN gateway példányok fog létrehozni a S2S VPN-alagutat hello helyszíni eszköz.

A folytatás előtt ellenőrizze, hogy befejeződött [1. rész](#aagateway) ebben a gyakorlatban.

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a>1. lépés – létrehozása és hello helyi hálózati átjáró konfigurálása
#### <a name="1-declare-your-variables"></a>1. A változók deklarálása
Ebben a gyakorlatban továbbra is toobuild hello konfigurációs hello ábrán is látható. Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Néhány dolgot toonote hello helyi hálózati átjáró paraméterek kapcsolatban:

* hello helyi hálózati átjáró lehetnek azonos hello, vagy más helyre és az erőforrás csoport mint hello VPN-átjáró. Ez a példa bemutatja azokat a különböző erőforráscsoportokra, de hello Azure ugyanazon a helyen.
* Ha csak egy helyszíni VPN-eszköz a fentiek szerint, hello aktív-aktív kapcsolat együttműködhet, függetlenül a BGP protokollt. A példa BGP hello létesítmények közötti kapcsolathoz.
* Ha a BGP engedélyezve van, a toodeclare hello helyi hálózati átjáró szükséges hello előtag az hello állomás címe a VPN-eszköz a BGP-Társgép IP-cím. Egy /32 ebben az esetben a "10.52.255.253/32" előtaggal.
* Ne feledje a helyszíni hálózatokhoz és az Azure VNet közötti különböző BGP ASN kell használnia. Ha azok hello vannak ugyanaz, meg kell toochange a VNet ASN Ha a helyszíni VPN-eszköz már használ hello ASN toopeer más BGP szomszédok.

#### <a name="2-create-hello-local-network-gateway-for-site5"></a>2. Site5 hello helyi hálózati átjáró létrehozása
A folytatás előtt győződjön meg arról, hogy továbbra is csatlakoztatott tooSubscription 1. Hello erőforráscsoport létrehozása, ha az még nem jött létre.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a>2. lépés – hello hálózatok átjáró és a helyi hálózati átjáró
#### <a name="1-get-hello-two-gateways"></a>1. Hello két átjáró beolvasása

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a>2. Hello TestVNet1 tooSite5 kapcsolat létrehozása
Ebben a lépésben együtt fogja létrehozni hello kapcsolat a TestVNet1 tooSite5_1 túl beállítása "EnableBGP" $True.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. A helyszíni VPN-eszköz VPN és BGP paraméterei
az alábbi példa hello beírja hello BGP konfigurációs szakasz azokat a helyszíni VPN-eszköz ebben a gyakorlatban hello paramétereket tartalmazza:

    - Site5 ASN: 65050
    - BGP-IP-Site5: 10.52.255.253
    - Előtagok tooannounce: (példa) 10.51.0.0/16 és 10.52.0.0/16
    - Az Azure VNet ASN: 65010
    - Az Azure VNet BGP-IP-1: 10.12.255.4 az alagutat too40.112.190.5
    - Az Azure VNet BGP IP 2: 10.12.255.5 az alagutat too138.91.156.129
    - Statikus útvonal: cél 10.12.255.4/32, a következő ugrás hello VPN alagút felület too40.112.190.5 cél 10.12.255.5/32, a következő ugrás hello VPN alagút felület too138.91.156.129
    - Többszörös ugrási eBGP: az ebgp-t az eszközön engedélyezve van, ha szükséges, győződjön meg arról, hogy hello "Többszörös ugrási" beállítás

hello kell kapcsolatot után néhány percet, és hello BGP társviszony-létesítési munkamenetet hello IPsec-kapcsolat létrejötte után elindul. Ebben a példában, amennyiben van beállítva csak egy helyszíni VPN-eszközön, lent látható módon hello diagram eredményezi:

![aktív-aktív-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-toohello-active-active-vpn-gateway"></a>3. lépés - csatlakozás két helyszíni VPN eszközök toohello aktív-aktív VPN-átjáró
Ha két VPN-eszközök: hello megegyezik a helyszíni hálózat, kettős redundancia érhet el, kapcsolódó hello Azure VPN gateway toohello második VPN-eszköz.

#### <a name="1-create-hello-second-local-network-gateway-for-site5"></a>1. Site5 hello második helyi hálózati átjáró létrehozása
Vegye figyelembe, hogy hello átjáró IP-cím, a címelőtagot és hello második helyi hálózati átjáró BGP társviszony-létesítési címe nem lehet átfedésben hello hello előző helyi hálózati átjáró megegyezik a helyi hálózaton.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-hello-vnet-gateway-and-hello-second-local-network-gateway"></a>2. Hello hálózatok átjáró és hello második helyi hálózati átjáró
Hozzon létre hello kapcsolat a TestVNet1 tooSite5_2 túl beállítása "EnableBGP" $True

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. A második a helyszíni VPN-eszköz VPN és BGP paraméterei
Hasonlóan alábbi listák hello paraméter megadja a hello második VPN-eszköz:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5
                         Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

Miután hello (alagutak) van kapcsolat, meg kell kettős redundáns VPN-eszközök és a helyszíni hálózat és az Azure csatlakozás alagutak:

![kettős-redundancia-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name ="aav2v"></a>3. rész – egy aktív-aktív VNet – VNet-kapcsolatot létesíteni
Ez a szakasz egy aktív-aktív VNet – VNet-kapcsolatot hoz létre a BGP. 

az alábbi utasítások hello hello előző lépéseiből fent felsorolt továbbra is. Meg kell adnia a [1. rész](#aagateway) toocreate hello VPN Gateway a BGP és TestVNet1 konfigurálni. 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a>1. lépés – TestVNet2 és hello VPN-átjáró létrehozása
Fontos, hogy egyetlen virtuális hálózat tartományt nem átfedésben hello IP-címtér hello új virtuális hálózat TestVNet2, toomake.

Ebben a példában a virtuális hálózatok hello toohello tartozik ugyanahhoz az előfizetéshez. Beállíthatja a VNet – VNet kapcsolatokhoz különböző előfizetések; Tekintse meg a túl[VNet – VNet-kapcsolatot konfiguráló](vpn-gateway-vnet-vnet-rm-ps.md) toolearn további részletek. Ellenőrizze, hogy hello "-EnableBgp $True" Ha hello kapcsolatok tooenable BGP létrehozása.

#### <a name="1-declare-your-variables"></a>1. A változók deklarálása
Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a>2. TestVNet2 hello új erőforráscsoport létrehozása

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-active-active-vpn-gateway-for-testvnet2"></a>3. TestVNet2 hello aktív-aktív VPN-átjáró létrehozása
Kérelem két nyilvános IP-címek toobe lefoglalt toohello átjáró fog létrehozni a virtuális hálózat számára. Hello alhálózatot és IP-konfiguráció szükséges is fogja definiálni.

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

MINT száma és hello "EnableActiveActiveFeature" jelző hello hello VPN-átjáró hozható létre. Vegye figyelembe, hogy az Azure VPN gatewayek kell bírálja felül hello alapértelmezett ASN. hello ASN-eket a hello csatlakoztatott Vnetekhez különböző tooenable BGP és a tranzit útválasztást kell lennie.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a>2. lépés – hello TestVNet1 és TestVNet2 átjárók csatlakozás
Ebben a példában mindkét átjárók vannak hello ugyanahhoz az előfizetéshez. Ennek végrehajtásáról a hello ugyanazon PowerShell-munkamenetben.

#### <a name="1-get-both-gateways"></a>1. Mindkét átjárók beolvasása
Ellenőrizze, hogy jelentkezzen be, és csatlakozzon a tooSubscription 1.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Mindkét kapcsolatok létrehozása
Ebben a lépésben létre fog hozni TestVNet1 tooTestVNet2 hello kapcsolatát, illetve hello kapcsolat TestVNet2 tooTestVNet1 a.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Lehet, hogy tooenable BGP mindkét kapcsolatokhoz.
> 
> 

A lépések elvégzése után hello kapcsolat fog létrehozni, néhány perc múlva, és hello BGP társviszony-létesítési munkamenetet kell mentése hello VNet – VNet-kapcsolatot kettős redundanciával befejezése után:

![aktív-aktív-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>4. rész - frissítés meglévő átjáró aktív-aktív és aktív-készenléti állapotban lévő között
hello utolsó részből megtudhatja, hogyan konfigurálhat egy meglévő Azure VPN gateway aktív tooactive aktív készenléti, vagy fordítva.

> [!NOTE]
> Ez a szakasz egy már létrehozott VPN-átjáró a szabványos tooHighPerformance hello lépéseket tooresize egy örökölt Termékváltozat (régi SKU) tartalmazza. Ezeket a lépéseket ne frissítse egy régi örökölt SKU tooone hello az új SKU.
> 
> 

### <a name="configure-an-active-standby-gateway-tooactive-active-gateway"></a>Egy aktív-készenléti állapotban lévő tooactive aktív átjárók konfigurálása
#### <a name="1-gateway-parameters"></a>1. Átjáró paraméterek
a következő példa hello egy aktív-készenléti állapotban lévő átjáró alakít át egy aktív-aktív átjárót. Toocreate kell egy másik nyilvános IP-címet, majd adja hozzá a második átjáró IP-konfigurációt. Az alábbi hello paramétereket használni:

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"

$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-hello-public-ip-address-then-add-hello-second-gateway-ip-configuration"></a>2. Hello nyilvános IP-cím létrehozása, és vegye fel a hello második átjáró IP-konfiguráció

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-hello-gateway"></a>3. Aktív-aktív mód és a frissítés hello átjáró engedélyezése
Meg kell adni hello átjáró objektum PowerShell tootrigger hello tényleges frissítés. hello hello virtuális hálózati átjáró Termékváltozata is módosítani kell (átméretezett) tooHighPerformance szabványként korábban létrehozása óta.

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

A frissítés too45 30 percet is igénybe vehet.

### <a name="configure-an-active-active-gateway-tooactive-standby-gateway"></a>Egy aktív-aktív tooactive-készenléti átjárók konfigurálása
#### <a name="1-gateway-parameters"></a>1. Átjáró paraméterek
Használjon hello azonos, a fenti paraméterek hello IP-konfigurációt kívánja hello nevének beolvasása tooremove.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-hello-gateway-ip-configuration-and-disable-hello-active-active-mode"></a>2. Távolítsa el a hello átjáró IP-konfigurációt, és hello aktív-aktív mód letiltása
Hasonlóképpen be kell állítani a hello átjáró objektum PowerShell tootrigger hello tényleges frissítés.

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

A frissítés be too30 túl 45 percig is tarthat.

## <a name="next-steps"></a>Következő lépések
Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
