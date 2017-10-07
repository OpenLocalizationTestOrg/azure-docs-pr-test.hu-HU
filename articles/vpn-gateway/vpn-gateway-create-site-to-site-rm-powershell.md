---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN: PowerShell |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. Ezen lépéseket követve létrehozhat egy helyek közötti VPN-átjáró kapcsolatot a PowerShell segítségével."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: cb8db1dab3a5488816a7f7e8e63908a4c02f55db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Helyek közötti VPN-kapcsolattal rendelkező virtuális hálózat létrehozása a PowerShell használatával

Ez a cikk bemutatja, hogyan toouse PowerShell toocreate a pont-pont VPN gateway-kapcsolatot a helyszíni hálózati toohello virtuális hálózat. a cikkben ismertetett hello alkalmazása toohello Resource Manager üzembe helyezési modellben. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Parancssori felület](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasszikus portál (klasszikus)](vpn-gateway-site-to-site-create.md)
> 
>


Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül. Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel. További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).

![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <a name="before"></a>Előkészületek

Győződjön meg arról, hogy teljesül-e az hello feltételeket a konfigurációs megkezdése előtt a következő:

* Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt. További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).
* Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára. Ez az IP-cím nem lehet NAT mögötti.
* Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki. Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani. A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül.
* Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez. PowerShell-parancsmagok gyakran frissül, és általában szüksége lesz tooupdate a PowerShell parancsmagok tooget hello legújabb funkcióhoz. Ha nem frissíti a PowerShell-parancsmagjai, a megadott hello értékek sikertelen lehet. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) letöltése és telepítése a PowerShell-parancsmagokkal kapcsolatos információkért.

### <a name="example"></a>Példaértékek

a cikkben szereplő példák hello hello a következő értékeket használja. Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.

```
#Example values

VnetName                = TestVNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.1.0/28 
GatewaySubnet           = 10.11.0.0/27
LocalNetworkGatewayName = Site2
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24, 20.0.0.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2

```


## <a name="Login"></a>1. Csatlakozás tooyour előfizetés

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="VNet"></a>2. Virtuális hálózat és átjáró-alhálózat létrehozása

Ha még nem rendelkezik virtuális hálózattal, akkor hozzon létre egyet. Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy megadott hello címterek nem fedhetnek át olyan hello címterek, hogy rendelkezik a helyi hálózaton.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="vnet"></a>toocreate egy virtuális hálózatot és egy átjáró-alhálózatot

Ebben a példában egy virtuális hálózatot és egy átjáróalhálózatot hozunk létre. Ha már van egy virtuális hálózatot, amelyekre szüksége van egy átjáróalhálózat megjelenítéséhez, tooadd [tooadd átjáró alhálózati tooa virtuális hálózat már létrehozott](#gatewaysubnet).

Hozzon létre egy erőforráscsoportot:

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location 'East US'
```

Hozza létre a virtuális hálózatot.

1. Hello változók megadása.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix 10.11.1.0/28
  ```
2. Hello virtuális hálózat létrehozása.

  ```powershell
  New-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1 `
  -Location 'East US' -AddressPrefix 10.11.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>átjáró alhálózati tooa virtuális hálózat már létrehozott tooadd

1. Hello változók megadása.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
  ```
2. Hozzon létre hello átjáró-alhálózatot.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27 -VirtualNetwork $vnet
  ```
3. Hello konfigurációjának beállítása

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>Hello helyi hálózati átjáró létrehozása

hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik. Adjon hello hely egy nevet, amellyel Azure is tekintse meg a tooit, majd adja meg hello IP-címet a hello a helyszíni VPN-eszköz toowhich létrehoz egy kapcsolatot. Is meg hello IP-cím előtagokat hello VPN gateway toohello VPN-eszközön keresztül történik. megadott hello címelőtagokat a helyszíni hálózaton található hello előtagok. Ha módosítja a helyszíni hálózat, hello előtagok könnyen frissítheti.

A következő értékek hello használata:

* Hello *GatewayIPAddress* hello IP-címe a helyszíni VPN-eszköz. A VPN-eszköz nem lehet NAT mögött.
* Hello *címelőtagja* van a helyszíni címtér.

a helyi hálózati átjáró egyetlen címelőtaggal rendelkező tooadd:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.0.0.0/24'
  ```

a helyi hálózati átjáró több címelőtagokat a tooadd:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

IP-címelőtagokat toomodify a helyi hálózati átjáró:<br>
A helyi hálózati átjáró előtagjai bizonyos esetekben változnak. hello lépések végrehajtása toomodify az IP-cím előtagokat, attól függ, hogy létrehozott egy VPN gateway-kapcsolatot. Lásd: hello [módosítása IP-cím előtagokat helyi hálózati átjáró](#modify) című szakaszát.

## <a name="PublicIP"></a>4. Nyilvános IP-cím kérése

Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel. Először igényelnie hello IP-cím erőforrás, és ezután tooit hivatkozni, a virtuális hálózati átjáró létrehozása során. hello IP-cím dinamikusan toohello erőforrás van hozzárendelve, hello VPN-átjáró létrehozásakor. A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja. Nem kérheti statikus IP-cím hozzárendelését. Ez azonban nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja. hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza. Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.

A hozzárendelt tooyour virtuális hálózat VPN-átjáró nyilvános IP-cím kérése.

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Hello átjáró IP-konfiguráció címzési létrehozása

hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg. A következő példa toocreate hello használata az átjáró konfigurációját:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. Hello VPN-átjáró létrehozása

Hello virtuális hálózat VPN-átjáró létrehozásához. VPN-átjáró létrehozása akár is igénybe vehet too45 perc vagy több toocomplete.

A következő értékek hello használata:

* Hello *- GatewayType* konfiguráció esetén a pont-pont *Vpn*. hello átjáró típus mindig valósít meg adott toohello-konfiguráció. Más átjáró-konfigurációk például -GatewayType ExpressRoute típus alkalmazását igényelhetik.
* Hello *– VpnType* lehet *RouteBased* (említett tooas bizonyos dokumentációkban dinamikus átjáró), vagy *PolicyBased* (említett tooas bizonyos dokumentációkban statikus átjáró ). További információk a VPN-átjárótípusokról: [Információk a VPN Gateway-ről](vpn-gateway-about-vpngateways.md).
* Válassza ki a megjeleníteni kívánt toouse átjáró-Termékváltozat hello. Egyes termékváltozatok konfigurációs korlátokkal rendelkeznek. További információkért lásd: [Az átjárók termékváltozatai](vpn-gateway-about-vpn-gateway-settings.md#gwsku). Ha hibaüzenetet kap hello VPN-átjáróval kapcsolatos hello létrehozásakor - GatewaySku, győződjön meg arról, hogy telepítette-e az hello hello PowerShell-parancsmagok legújabb verzióját.

```powershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="ConfigureVPNDevice"></a>7. VPN-eszköz konfigurálása

Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség. Ebben a lépésben a VPN-eszköz konfigurálása következik. A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:

- Megosztott kulcs. Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs. A példákban alapvető megosztott kulcsot használunk. Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.
- hello a virtuális hálózati átjáró nyilvános IP-címét. Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg. toofind hello a PowerShell, a következő példa használata hello használó virtuális hálózati átjáró nyilvános IP-cím:

  ```powershell
  Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>8. Hello VPN-kapcsolat létrehozása

Ezután hozzon létre hello telephelyek közötti VPN kapcsolatot a virtuális hálózati átjáró és a VPN-eszköz. Lehet, hogy tooreplace hello értékeket a saját. hello megosztott kulcsot meg kell egyeznie a VPN-eszköz konfigurációjában használt hello érték. Figyelje meg, hogy hello "-ConnectionType" hely-hely *IPsec*.

1. Hello változók megadása.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  $local = Get-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1
  ```

2. Hello kapcsolat létrehozása.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite2 -ResourceGroupName TestRG1 `
  -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Hello közben rövid után kapcsolat jön létre.

## <a name="toverify"></a>9. Hello VPN-kapcsolat ellenőrzése

Van néhány különböző módokon tooverify a VPN-kapcsolatot.

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="connectVM"></a>tooconnect tooa virtuális gép

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <a name="modify"></a>Helyi hálózati átjáró IP-címelőtagjainak módosítása

Ha hello IP-cím előtagokat, amelyet irányítása tooyour helyszíni hely módosításához hello helyi hálózati átjáró módosíthatja. Két utasításcsoportot adtunk meg. hello utasításokat úgy dönt, attól függ, hogy már létrehozta a gateway-kapcsolatot.

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Hello átjáró IP-címet a helyi hálózati átjáró módosítása

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Következő lépések

*  Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
