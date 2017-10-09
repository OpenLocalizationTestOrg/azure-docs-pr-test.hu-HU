---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN: CLI |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. Ezen lépéseket követve létrehozhat egy helyek közötti VPN-átjáró kapcsolatot a parancssori felület segítségével."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: c652cf2caf3928cdeb19d7dc329f6db101e5ed90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Virtuális hálózat létrehozása helyek közötti VPN-kapcsolattal a parancssori felület használatával

Ez a cikk bemutatja, hogyan toouse hello Azure CLI toocreate a helyszíni hálózati toohello VNet a pont-pont VPN gateway-kapcsolattal. a cikkben ismertetett hello alkalmazása toohello Resource Manager üzembe helyezési modellben. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:<br>

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Parancssori felület](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül. Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel. További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Előkészületek

Győződjön meg arról, hogy teljesül-e az hello feltételek konfigurációs megkezdése előtt a következő:

* Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt. További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).
* Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára. Ez az IP-cím nem lehet NAT mögötti.
* Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki. Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani. A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül.
* Győződjön meg arról, hogy telepítette-e az hello parancssori felület parancsait (2.0-s vagy újabb) legújabb verzióját. Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli) és [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).

### <a name="example"></a>Példaértékek

A következő értékek toocreate egy tesztkörnyezetben hello használhatja, vagy tekintse meg a toothese értékek toobetter megérteni a cikkben szereplő példák hello:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Csatlakozás tooyour előfizetés

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="rg"></a>2. Hozzon létre egy erőforráscsoportot

hello alábbi példa létrehoz egy erőforráscsoportot "TestRG1" hello "eastus" helyen. Ha már rendelkezik egy erőforráscsoport, amelyet toocreate a virtuális hálózat hello régióban, használhatja, hogy egy helyette.

```azurecli
az group create --name TestRG1 --location eastus
```

## <a name="VNet"></a>3. Virtuális hálózat létrehozása

Ha még nem rendelkezik virtuális hálózat, hozzon létre egyet hello segítségével [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) parancsot. Amikor egy virtuális hálózat létrehozásával, győződjön meg arról, hogy megadott hello címterek nem fedhetnek át olyan hello címterek, hogy rendelkezik a helyi hálózaton.

hello alábbi példa létrehoz egy virtuális hálózatot "TestVNet1" és az alhálózatot, "Alhalozat_1" nevű.

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## 4. <a name="gwsub"></a>Hozzon létre hello átjáró-alhálózatot

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Ehhez a konfigurációhoz átjáróalhálózat is szükséges. hello virtuális hálózati átjáró egy átjáró-alhálózatot, amely tartalmazza a hello hello VPN gateway szolgáltatások által használt IP-címeket használ. A létrehozott átjáróalhálózatnak a „GatewaySubnet” nevet kell adnia. Ha ezt az alhálózatot máshogy nevezi el, az létrejön ugyan, de az Azure nem kezeli átjáró-alhálózatként.

hello átjáróalhálózatot megadott hello mérete függ hello VPN-átjáró konfigurációs, amelyet az toocreate. Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet /27 vagy /28 kiválasztásával. Egy nagyobb átjáróalhálózat használata lehetővé teszi a elegendő IP-címek tooaccommodate lehetséges jövőbeli konfigurációk.

Használjon hello [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create) parancs toocreate hello átjáró-alhálózatot.

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Hello helyi hálózati átjáró létrehozása

hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik. Adjon hello hely egy nevet, amellyel Azure is tekintse meg a tooit, majd adja meg hello IP-címet a hello a helyszíni VPN-eszköz toowhich létrehoz egy kapcsolatot. Is meg hello IP-cím előtagokat hello VPN gateway toohello VPN-eszközön keresztül történik. megadott hello címelőtagokat a helyszíni hálózaton található hello előtagok. Ha módosítja a helyszíni hálózat, hello előtagok könnyen frissítheti.

A következő értékek hello használata:

* Hello *– ip-átjárócím* hello IP-címe a helyszíni VPN-eszköz. A VPN-eszköz nem lehet NAT mögött.
* Hello *– helyi címelőtagokat* vannak a helyszíni címtér.

Használjon hello [az hálózati helyi-átjáró létrehozása](/cli/azure/network/local-gateway#create) parancs tooadd egy helyi hálózati átjáró több címelőtagok:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Nyilvános IP-cím kérése

Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel. Először igényelnie hello IP-cím erőforrás, és ezután tooit hivatkozni, a virtuális hálózati átjáró létrehozása során. hello IP-cím dinamikusan toohello erőforrás van hozzárendelve, hello VPN-átjáró létrehozásakor. A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja. Nem kérheti statikus IP-cím hozzárendelését. Ez azonban nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja. hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza. Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.

Használjon hello [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create) parancs toorequest dinamikus nyilvános IP-címnek.

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Hello VPN-átjáró létrehozása

Hello virtuális hálózat VPN-átjáró létrehozásához. VPN-átjáró létrehozása akár is igénybe vehet too45 perc vagy több toocomplete.

A következő értékek hello használata:

* Hello *– átjáró típusú* konfiguráció esetén a pont-pont *Vpn*. hello átjáró típus mindig valósít meg adott toohello-konfiguráció. További információért lásd: [Átjárótípusok](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* Hello *– vpn-típus* lehet *RouteBased* (említett tooas bizonyos dokumentációkban dinamikus átjáró), vagy *PolicyBased* (említett tooas egyes statikus átjáró a dokumentációt). hello beállítás adott toorequirements hello eszköz, amely csatlakozik. További információk a VPN-átjárótípusokról: [Információk a VPN Gateway konfigurációs beállításairól](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* Válassza ki a megjeleníteni kívánt toouse átjáró-Termékváltozat hello. Egyes termékváltozatok konfigurációs korlátokkal rendelkeznek. További információkért lásd: [Az átjárók termékváltozatai](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Hozzon létre hello VPN-átjáró hello segítségével [az hálózati vnet-átjáró létrehozása](/cli/azure/network/vnet-gateway#create) parancsot. A parancs futtatásakor hello használatával – nincs - wait paramétert, ha bármely visszajelzést vagy kimeneti nem látható. Ez a paraméter lehetővé teszi, hogy hello átjáró toocreate hello háttérben. Körülbelül 45 percig toocreate átjáró szükséges.

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <a name="VPNDevice"></a>8. VPN-eszköz konfigurálása

Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség. Ebben a lépésben a VPN-eszköz konfigurálása következik. A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:

- Megosztott kulcs. Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs. A példákban alapvető megosztott kulcsot használunk. Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.
- hello a virtuális hálózati átjáró nyilvános IP-címét. Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg. toofind hello nyilvános IP-címet a virtuális hálózati átjáró használata hello [az nyilvános ip-lista](/cli/azure/network/public-ip#list) parancsot. Olvasás, hello eredménye a nyilvános IP-címek táblázatos formátumú formázott toodisplay hello listája.

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>9. Hello VPN-kapcsolat létrehozása

A virtuális hálózati átjáró és a helyszíni VPN-eszköz közötti pont-pont hello VPN-kapcsolat létrehozása. Fizetési különös figyelmet toohello megosztott kulcs értékét, amelynek meg kell egyeznie a konfigurált hello megosztott kulcs értéke a VPN-eszköz.

Hello hello szolgáltatás kapcsolatának létrehozása [az hálózati vpn-kapcsolat létrehozása](/cli/azure/network/vpn-connection#create) parancsot.

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

Hello közben rövid után kapcsolat jön létre.

## <a name="toverify"></a>10. Hello VPN-kapcsolat ellenőrzése

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

Ha egy másik módszer tooverify toouse szeretné a hálózati kapcsolatot, lásd: [ellenőrizni a VPN-átjáró kapcsolatot](vpn-gateway-verify-connection-resource-manager.md).

## <a name="connectVM"></a>tooconnect tooa virtuális gép

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="tasks"></a>Gyakori feladatok

Ez a szakasz a helyek közötti konfigurációk használatakor hasznos gyakori parancsokat tartalmazza. A hálózati parancssori felület parancsait hello teljes listáját, lásd: [Azure CLI - hálózati](/cli/azure/network).

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a>Következő lépések

* Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Információk a kényszerített bújtatásról: [Információk a kényszerített bújtatásról](vpn-gateway-forced-tunneling-rm.md).
* Információk a magas rendelkezésre állású aktív-aktív kapcsolatokról: [Magas rendelkezésre állású kapcsolatok létesítmények, illetve virtuális hálózatok között](vpn-gateway-highlyavailable.md).
* Az Azure CLI hálózati parancsainak listáját lásd: [Azure CLI](https://docs.microsoft.com/cli/azure/network).