---
title: "Az Azure VPN Gatewayek beállítani a BGP Protokollt: erőforrás-kezelő: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan BGP konfigurálása az Azure VPN Gatewayek Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: a9d13ae6b319e2efa8965dc2955c9b89ac3fd12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-bgp-on-azure-vpn-gateways-using-powershell"></a>Hogyan tooconfigure BGP az Azure VPN Gatewayek PowerShell használatával
Ez a cikk bemutatja, hogyan hello lépéseket tooenable BGP létesítmények közötti pont-pont (S2S) VPN-kapcsolat és egy VNet – VNet-kapcsolatot hello Resource Manager üzembe helyezési modellben és a PowerShell használatával.

## <a name="about-bgp"></a>A BGP ismertetése
A BGP egy szabványos útválasztási protokoll hello általánosan használt az hello Internet tooexchange az Útválasztás és reachability information legalább két hálózat között. BGP hello Azure VPN Gatewayek és a helyszíni VPN-eszközök, a BGP-társakat vagy szomszédok teszi lehetővé, tooexchange "irányítja a", amely tájékoztatja a mindkét átjárók hello rendelkezésre állásáról és azok reachability előtagok toogo hello átjárók és az érintett útválasztók keresztül. BGP is engedélyezhető tranzit útválasztást több hálózat között útvonalak BGP átjáró egy BGP-társ tooall való megtanulja propagálására más BGP-társak.

Lásd: [áttekintése a BGP az Azure VPN Gatewayek](vpn-gateway-bgp-overview.md) előnyeit a BGP és toounderstand hello technikai követelmények és szempontok BGP használatával további leírását.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Az Azure VPN gatewayek a BGP-első lépések

Ez a cikk bemutatja, hogyan hello lépéseket toodo hello a következő feladatokat:

* [Engedélyezze az Azure VPN gateway a BGP, 1 -. rész](#enablebgp)
* [2. rész – a BGP-létesítmények közötti kapcsolat létrehozása](#crossprembgp)
* [3. rész - BGP VNet – VNet-kapcsolatot létrehozni](#v2vbgp)

Hello utasításokat részét képező összes a BGP engedélyezve van a hálózati kapcsolatot az alapvető építőelemre képezi. Ha befejezte az összes három részből, hello topológia elkészítette a hello a következő ábrán látható módon:

![BGP-topológiákkal](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Egyesítheti a kijelzők együtt toobuild egy összetettebb, Többugrásos, átmenő hálózatról, amely megfelel az igényeinek.

## <a name ="enablebgp"></a>1. rész – hello Azure VPN Gateway a BGP konfigurálása
hello konfigurációs lépések beállítása hello hello Azure VPN-átjáró BGP-paraméterek a hello a következő ábrán látható módon:

![BGP-átjáró](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Előkészületek
* Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
* Hello Azure Resource Manager PowerShell-parancsmagjainak telepítése. PowerShell-parancsmagok hello telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>1. lépés – létrehozása és VNet1 konfigurálása
#### <a name="1-declare-your-variables"></a>1. A változók deklarálása
Ehhez a gyakorlathoz először is deklarálni kell a változókat. hello alábbi példa azt deklarálja hello változók ehhez a gyakorlathoz hello értékekkel. Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor. Ezek a változók is használhatja, ha ismeri a konfiguráció az ilyen típusú hello lépéseket toobecome keresztül futtatja. Hello változók módosítása, majd másolja és illessze be a PowerShell-konzolban.

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
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
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Csatlakozás tooyour előfizetés, és hozzon létre egy új erőforráscsoportot
Erőforrás-kezelő parancsmagok toouse hello váltson át tooPowerShell mód. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók. A következő minta toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. A TestVNet1 létrehozása
hello alábbi minta létrehoz egy TestVNet1 és három alhálózatok, egy hívott GatewaySubnet, egy hívott előtér és egy hívott háttér nevű virtuális hálózatot. Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen. Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>2. lépés – hello VPN-átjáró BGP-paraméterekkel rendelkező TestVNet1 létrehozása
#### <a name="1-create-hello-ip-and-subnet-configurations"></a>1. Hello IP-cím és alhálózat-konfigurációk létrehozása
A nyilvános IP-cím toobe lefoglalt toohello átjáró fog létrehozni a Vnethez tartozó kérelmet. Szükséges hello alhálózatot és IP-konfigurációk is fogja definiálni.

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-hello-vpn-gateway-with-hello-as-number"></a>2. Hozzon létre hello VPN-átjáró hello SZÁMOT
TestVNet1 hello virtuális hálózati átjáró létrehozása A BGP TestVNet1 egy Útvonalalapú VPN-átjáró, valamint a hello hozzáadása paraméter - Asn-tooset hello ASN (AS-szám) kell rendelkeznie. Ha hello ASN paraméter nincs megadva, a ASN 65515 hozzá van rendelve. Átjáró létrehozása időt vehet igénybe (30 perc vagy több toocomplete).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-hello-azure-bgp-peer-ip-address"></a>3. Hello Azure BGP-Társgép IP-cím beszerzése
Hello átjáró létrehozása után kell tooobtain hello hello Azure VPN Gateway a BGP-Társgép IP-cím. Ez a cím szükséges tooconfigure hello Azure VPN Gateway, mint a BGP-partner a helyszíni VPN-eszközök.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

hello utolsó parancs hello megfelelő BGP konfigurációk láthatók a hello Azure VPN Gateway; Példa:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Hello átjáró létrehozása után is használhatja az átjáró tooestablish létesítmények közötti vagy VNet – VNet-kapcsolatot a BGP. a következő szakaszok a lépésein végighaladva hello lépéseket toocomplete hello gyakorlat hello.

## <a name ="crossprembbgp"></a>2. rész – a BGP-létesítmények közötti kapcsolat létrehozása

tooestablish létesítmények közötti kapcsolatot, a helyi hálózati átjáró toorepresent toocreate szüksége, és a helyszíni VPN-eszköz kapcsolat tooconnect hello VPN-átjáró hello helyi hálózati átjáró. Nincsenek cikkek, amely ismerteti a fenti lépéseket, amíg a cikkben hello további tulajdonságok szükséges toospecify hello BGP konfigurációs paramétereket.

![A létesítmények közötti BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

A továbblépés előtt ellenőrizze, hogy befejeződött [1. rész](#enablebgp) ebben a gyakorlatban.

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a>1. lépés – létrehozása és hello helyi hálózati átjáró konfigurálása

#### <a name="1-declare-your-variables"></a>1. A változók deklarálása

Ebben a gyakorlatban továbbra is toobuild hello konfigurációs hello ábrán is látható. Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Néhány dolgot toonote hello helyi hálózati átjáró paraméterek kapcsolatban:

* hello helyi hálózati átjáró lehetnek azonos hello, vagy más helyre és az erőforrás csoport mint hello VPN-átjáró. Ez a példa bemutatja azokat a különböző erőforráscsoportokra különböző helyeken.
* a előtag minimális hello toodeclare hello helyi hálózati átjáró van szüksége az hello állomás cím a VPN-eszköz a BGP-Társgép IP-cím. Egy /32 ebben az esetben a "10.52.255.254/32" előtaggal.
* Ne feledje a helyszíni hálózatokhoz és az Azure VNet közötti különböző BGP ASN kell használnia. Ha azok hello vannak ugyanaz, meg kell toochange a VNet ASN Ha a helyszíni VPN-eszköz már használ hello ASN toopeer más BGP szomszédok.

A folytatás előtt győződjön meg arról, hogy továbbra is csatlakoztatott tooSubscription 1.

#### <a name="2-create-hello-local-network-gateway-for-site5"></a>2. Site5 hello helyi hálózati átjáró létrehozása

Lehet, hogy toocreate hello erőforráscsoport, abban az esetben, ha nem létrehozott hello helyi hálózati átjáró létrehozása előtt. Figyelje meg hello két további paraméterek hello helyi hálózati átjáró: Asn és BgpPeerAddress.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a>2. lépés – hello hálózatok átjáró és a helyi hálózati átjáró

#### <a name="1-get-hello-two-gateways"></a>1. Hello két átjáró beolvasása

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a>2. Hello TestVNet1 tooSite5 kapcsolat létrehozása

Ebben a lépésben készíthet TestVNet1 tooSite5 hello kapcsolat. Adjon meg "-EnableBGP $True" tooenable BGP ehhez a kapcsolathoz. A fentiekben taglaltak, akkor lehetséges toohave BGP- és -BGP kapcsolatokat ugyanazon Azure VPN Gateway hello. Ha BGP hello kapcsolat tulajdonságban nincs engedélyezve, Azure teszik lehetővé a BGP ehhez a kapcsolathoz annak ellenére, hogy a BGP-paraméterek már be van állítva mindkét átjárókon.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

hello alábbi példa felsorolja a helyszíni VPN-eszköz ebben a gyakorlatban hello BGP konfigurációs szakasz kezdenek hello paraméterek:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being hello VPN tunnel interface on your device
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

hello kapcsolat létrejötte után néhány percet, és hello BGP társviszony-létesítési munkamenet indítása hello IPsec-kapcsolat létrejötte után.

## <a name ="v2vbgp"></a>3. rész - BGP VNet – VNet-kapcsolatot létrehozni

Ez a szakasz egy VNet – VNet-kapcsolatot, a BGP-vel ad hozzá, ahogy az ábra a következő hello:

![A VNet – VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

az utasításoknak hello hello előző lépéseiből továbbra is. Meg kell adnia a [i. rész](#enablebgp) toocreate hello VPN Gateway a BGP és TestVNet1 konfigurálni. 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a>1. lépés – TestVNet2 és hello VPN-átjáró létrehozása

Fontos, hogy egyetlen virtuális hálózat tartományt nem átfedésben hello IP-címtér hello új virtuális hálózat TestVNet2, toomake.

Ebben a példában a virtuális hálózatok hello toohello tartozik ugyanahhoz az előfizetéshez. Beállíthatja a VNet – VNet kapcsolatokhoz különböző előfizetések között. További információkért lásd: [VNet – VNet-kapcsolatot konfiguráló](vpn-gateway-vnet-vnet-rm-ps.md). Ellenőrizze, hogy hello "-EnableBgp $True" Ha hello kapcsolatok tooenable BGP létrehozása.

#### <a name="1-declare-your-variables"></a>1. A változók deklarálása

Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
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
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
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

#### <a name="3-create-hello-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Hello VPN-átjáró BGP-paraméterekkel rendelkező TestVNet2 létrehozása

A nyilvános IP-cím toobe lefoglalt toohello átjáró a Vnethez tartozó létrehoz, és adja meg a szükséges hello alhálózatot és IP-konfigurációk kérelmet.

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

Hozzon létre hello VPN-átjáró hello SZÁMOT. Az Azure VPN gatewayek a hello alapértelmezett ASN felül kell bírálnia. hello ASN-eket a hello csatlakoztatott Vnetekhez különböző tooenable BGP és a tranzit útválasztást kell lennie.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
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

Ebben a lépésben hoz létre TestVNet1 tooTestVNet2 hello kapcsolatot, és hello kapcsolat TestVNet2 tooTestVNet1.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Lehet, hogy tooenable BGP mindkét kapcsolatokhoz.
> 
> 

A lépések elvégzése után hello kapcsolat létrejön, néhány perc múlva. hello BGP társviszony-létesítési munkamenetet működik, amikor befejeződött a hello VNet – VNet-kapcsolatot.

Ha befejezte az ebben a gyakorlatban három része, a következő hálózati topológia hello hozott létre:

![A VNet – VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Következő lépések

Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
