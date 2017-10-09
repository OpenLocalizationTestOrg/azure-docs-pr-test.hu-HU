---
title: "Csatlakozás az Azure-beli virtuális hálózat tooanother VNet: PowerShell |} Microsoft Docs"
description: "Ez a cikk lépésről lépésre bemutatja, hogyan csatlakoztathatók a virtuális hálózatok egymáshoz az Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0683c664-9c03-40a4-b198-a6529bf1ce8b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 2da30c76867cc3f71d040e63e0dd15d153e15c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-powershell"></a>Virtuális hálózatok közötti VPN Gateway-kapcsolat konfigurálása a PowerShell használatával

Ez a cikk bemutatja, hogyan toocreate virtuális hálózatok közötti VPN gateway-kapcsolattal. hello virtuális hálózatok lehetnek azonos vagy különböző régiókban hello, és a hello azonos vagy különböző előfizetésekhez. Amikor másik előfizetésből, hello előfizetések csatlakozó Vnetek nem kell társított toobe hello azonos Active Directory-bérlő. 

a cikkben ismertetett hello toohello Resource Manager üzembe helyezési modellben vonatkoznak, és PowerShell használatával. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Különböző üzemi modellek összekapcsolása – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Különböző üzemi modellek összekapcsolása – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hasonló tooconnecting egy VNet tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat. Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooconsider csatlakoztatja őket a Vnetben társviszony-létesítés használatával. A virtuális hálózatok közötti társviszony nem használ VPN-átjárót. További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).

A virtuális hálózatok közötti kommunikáció kombinálható többhelyes konfigurációkkal. Ez lehetővé teszi a felhasználó közötti kapcsolatot nyújthassanak közötti virtuális hálózati kapcsolattal rendelkező hálózati topológiák létrehozása a hello a következő ábrán látható módon:

![Tudnivalók a kapcsolatokról](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)

### <a name="why-connect-virtual-networks"></a>Miért érdemes összekapcsolni a virtuális hálózatokat?

A következő okok miatt hello érdemes lehet tooconnect virtuális hálózatok:

* **Georedundancia és földrajzi jelenlét több régióban**

  * Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.
  * Az Azure Traffic Manager és a Load Balancer segítségével magas rendelkezésre állású munkaterhelést állíthat be georedundanciával több Azure-régióban. Egy fontos példája tooset fel SQL Always On rendelkezésre állási csoportokkal több Azure-régiók keresztül terjednek.
* **Regionális többrétegű alkalmazások elkülönítéssel vagy felügyeleti határral**

  * Belül hello azonos régióban, beállíthat több rétegből álló alkalmazások összekapcsolása tooisolation vagy felügyeleti követelmények miatt több virtuális hálózaton.

VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén.

## <a name="which-set-of-steps-should-i-use"></a>Melyik eljárást használjam?

Ebben a cikkben kétféle lépéssorozatot láthat. Lépéseit egy készletét [lévő Vnetek hello ugyanahhoz az előfizetéshez](#samesub), majd egy másikat a [Vnetekhez különböző előfizetések lévő](#difsub). hello fő különbség hello beállítása, hogy hozhat létre, és konfigurálja a virtuális hálózat és az átjáró belül minden erőforrás hello ugyanazon PowerShell-munkamenetben.

hello a cikkben ismertetett változókkal minden szakasz elején hello deklarál. Ha már dolgozunk meglévő Vnetek, módosítsa a hello változók tooreflect hello beállításait a saját környezetében. Ha névfeloldással szeretné ellátni a virtuális hálózatokat, tekintse meg a [Névfeloldás](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.

## <a name="samesub"></a>Hogyan tooconnect Vnetek, amelyek a hello ugyanahhoz az előfizetéshez

![v2v ábra](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Előkészületek

Megkezdése előtt szüksége tooinstall hello legújabb verziójára hello Azure Resource Manager PowerShell-parancsmagok, legalább 4.0-s vagy újabb. PowerShell-parancsmagok hello telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

### <a name="Step1"></a>1. lépés – Az IP-címtartományok megtervezése

Az alábbi lépésekkel hello létrehozhatunk két virtuális hálózatok a megfelelő átjáró alhálózatok és konfigurációk mellett. Majd létrehozhatunk hello közötti VPN-kapcsolat két Vnetek. Fontos tooplan hello IP-címtartományok esetében a hálózati konfiguráció. Ügyeljen arra, hogy a virtuális hálózata tartományai és a helyi hálózata tartományai ne legyenek átfedésben egymással. Ezekben a példákban nem szerepel DNS-kiszolgáló. Ha névfeloldással szeretné ellátni a virtuális hálózatokat, tekintse meg a [Névfeloldás](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.

A következő értékek hello példákban hello használjuk:

**Értékek a TestVNet1-hez:**

* Virtuális hálózat neve: TestVNet1
* Erőforráscsoport: TestRG1
* Hely: East US
* TestVNet1: 10.11.0.0/16 és 10.12.0.0/16
* Előtér: 10.11.0.0/24
* Háttér: 10.12.0.0/24
* Átjáróalhálózat: 10.12.255.0/27
* Átjáró neve: VNet1GW
* Nyilvános IP-cím: VNet1GWIP
* VPN típusa: RouteBased
* Kapcsolat (1–4): VNet1toVNet4
* Kapcsolat (1–5): VNet1toVNet5
* Kapcsolat típusa: VNet2VNet

**Értékek a TestVNet4-hez:**

* Virtuális hálózat neve: TestVNet4
* TestVNet2: 10.41.0.0/16 és 10.42.0.0/16
* Előtér: 10.41.0.0/24
* Háttér: 10.42.0.0/24
* Átjáróalhálózat: 10.42.255.0/27
* Erőforráscsoport: TestRG4
* Hely: West US
* Átjáró neve: VNet4GW
* Nyilvános IP-cím: VNet4GWIP
* VPN típusa: RouteBased
* Kapcsolat: VNet4toVNet1
* Kapcsolat típusa: VNet2VNet


### <a name="Step2"></a>2. lépés – A TestVNet1 létrehozása és konfigurálása

1. Deklarálja a változókat. Ez a példa hello értékekkel ehhez a gyakorlathoz hello változók deklarálja. A legtöbb esetben azt le kell cserélni hello értékeket a saját. Azonban ezek változókat is használhat, ha ismeri a konfiguráció az ilyen típusú hello lépéseket toobecome keresztül futtatja. Hello változók, ha szükséges, módosítsa, majd másolja és illessze be a PowerShell-konzolban.

  ```powershell
  $Sub1 = "Replace_With_Your_Subcription_Name"
  $RG1 = "TestRG1"
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
  $GWName1 = "VNet1GW"
  $GWIPName1 = "VNet1GWIP"
  $GWIPconfName1 = "gwipconf1"
  $Connection14 = "VNet1toVNet4"
  $Connection15 = "VNet1toVNet5"
  ```

2. Csatlakozás tooyour fiók. A következő példa toohelp csatlakozás hello használata:

  ```powershell
  Login-AzureRmAccount
  ```

  Hello előfizetések hello fiók ellenőrzése.

  ```powershell
  Get-AzureRmSubscription
  ```

  Adja meg, hogy szeretné-e toouse hello előfizetés.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub1
  ```
3. Hozzon létre egy új erőforráscsoportot.

  ```powershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```
4. Hozzon létre hello TestVNet1 alhálózati konfigurációi. Ez a példa létrehoz egy TestVNet1 nevű virtuális hálózatot és három alhálózatot, amelyek neve a következő: GatewaySubnet, FrontEnd és Backend. Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen. Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.

  hello alábbi példa hello változók korábban beállított. Ebben a példában hello átjáróalhálózatot egy /27 használ. Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet legalább /28 vagy /27 kiválasztásával. Ez lehetővé teszi elég címek tooaccommodate lehetséges olyan beállításokat, amelyeket érdemes lehet a hello jövőbeli.

  ```powershell
  $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
  $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
  $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1
  ```
5. Hozza létre a TestVNet1-et.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
  ```
6. A nyilvános IP-cím toobe lefoglalt toohello átjáró fog létrehozni a Vnethez tartozó kérelmet. Figyelje meg, hogy hello AllocationMethod dinamikus. Nem adható meg, hogy szeretné-e toouse hello IP-címet. Fontos a dinamikusan kiosztott tooyour átjáró. 

  ```powershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
  ```
7. Hozzon létre hello átjáró konfigurációját. hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg. Hello példa toocreate az átjáró konfigurációját használni.

  ```powershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```
8. Hozzon létre hello átjáró TestVNet1. Ebben a lépésben a TestVNet1 hozza létre hello virtuális hálózati átjárót. A virtuális hálózatok közötti konfigurációkhoz a VpnType paraméternek RouteBased értékűnek kell lennie. Átjáró létrehozása gyakran eltarthat 45 perc vagy több, attól függően, hogy a kiválasztott átjáróeszközön hello Termékváltozat.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-3---create-and-configure-testvnet4"></a>3. lépés – A TestVNet4 létrehozása és konfigurálása

A TestVNet1 konfigurálása után a hozza létre a TestVNet4 virtuális hálózatot. Hello kövesse az alábbi hello értékeket cserélje le a saját, amikor szükséges. Ebben a lépésben végezhető hello belül azonos PowerShell-munkamenetben mert hello azonos előfizetéssel.

1. Deklarálja a változókat. Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

  ```powershell
  $RG4 = "TestRG4"
  $Location4 = "West US"
  $VnetName4 = "TestVNet4"
  $FESubName4 = "FrontEnd"
  $BESubName4 = "Backend"
  $GWSubName4 = "GatewaySubnet"
  $VnetPrefix41 = "10.41.0.0/16"
  $VnetPrefix42 = "10.42.0.0/16"
  $FESubPrefix4 = "10.41.0.0/24"
  $BESubPrefix4 = "10.42.0.0/24"
  $GWSubPrefix4 = "10.42.255.0/27"
  $GWName4 = "VNet4GW"
  $GWIPName4 = "VNet4GWIP"
  $GWIPconfName4 = "gwipconf4"
  $Connection41 = "VNet4toVNet1"
  ```
2. Hozzon létre egy új erőforráscsoportot.

  ```powershell
  New-AzureRmResourceGroup -Name $RG4 -Location $Location4
  ```
3. Hozzon létre hello TestVNet4 alhálózati konfigurációi.

  ```powershell
  $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
  $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
  $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4
  ```
4. Hozza létre a TestVNet4-et.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4
  ```
5. Kérjen egy nyilvános IP-címet.

  ```powershell
  $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
  -Location $Location4 -AllocationMethod Dynamic
  ```
6. Hozzon létre hello átjáró konfigurációját.

  ```powershell
  $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
  $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
  $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4
  ```
7. Hello TestVNet4 átjáró létrehozása. Átjáró létrehozása gyakran eltarthat 45 perc vagy több, attól függően, hogy a kiválasztott átjáróeszközön hello Termékváltozat.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
  -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-4---create-hello-connections"></a>4. lépés – hello kapcsolatok létrehozása

1. Hozza létre a virtuális hálózati átjárókat. Ha mindkét hello átjárók hello ugyanahhoz az előfizetéshez, mivel ezek hello példában, hajthatja végre ezt a lépést a hello ugyanazon PowerShell-munkamenetben.

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4
  ```
2. Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása. Ebben a lépésben készíthet TestVNet1 tooTestVNet4 hello kapcsolat. Látni fogja, a megosztott kulcs hello példák hivatkozik. Hello megosztott kulcs saját értékeket is használhat. fontos dolog hello megosztott kulcs hello egyeznie kell mindkét kapcsolatokhoz. Kapcsolat létrehozása során toocomplete rövid is igénybe vehet.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
3. Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása. Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet4 tooTestVNet1 hello kapcsolatot hoz létre. Ellenőrizze, hogy megosztott hello kulcsok felel meg. hello kapcsolat jön létre, néhány perc múlva.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
  -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. Ellenőrizze a kapcsolatot. Című rész hello [hogyan tooverify a kapcsolat](#verify).

## <a name="difsub"></a>Hogyan tooconnect Vnetek, amelyek különböző előfizetésekhez

![v2v ábra](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

Ebben a forgatókönyvben csatlakoztatjuk a TestVNet1 és a TestVNet5 virtuális hálózatot. A TestVNet1 és a TestVNet5 eltérő előfizetésekben található. hello előfizetések nem kell hello társított toobe azonos Active Directory-bérlő. hello ezeket a lépéseket és hello előző készletet közötti különbség, hogy egyes hello konfigurációs lépések szükség van egy külön PowerShell-munkamenetben hello második előfizetés hello környezetben végrehajtott toobe. Különösen amikor hello két előfizetések tartoznak toodifferent szervezetek.

### <a name="step-5---create-and-configure-testvnet1"></a>5. lépés – A TestVNet1 létrehozása és konfigurálása

Meg kell adnia a [1. lépés](#Step1) és [2. lépés](#Step2) az előző hello toocreate szakasz és a VPN-átjáró hello TestVNet1 és TestVNet1 konfigurálni. Ehhez a konfigurációhoz akkor használják, nem szükséges toocreate TestVNet4 az előző szakasz hello, bár hoz létre, ha ez nem ütközik ezeket a lépéseket. 1. lépés és a 2. lépés befejezése után folytassa a 6. lépés toocreate TestVNet5. 

### <a name="step-6---verify-hello-ip-address-ranges"></a>6. lépés - hello IP-címtartományok ellenőrzése

Arról, hogy hello IP-címtér hello új virtuális hálózat TestVNet5, nem fedi át a virtuális hálózat tartományok vagy a helyi hálózati átjáró tartományok egyik fontos toomake. Ebben a példában a virtuális hálózatok hello tartozhat toodifferent szervezetek. Ebben a gyakorlatban a következő értékek hello TestVNet5 hello használhatja:

**Értékek a TestVNet5-höz:**

* Virtuális hálózat neve: TestVNet5
* Erőforráscsoport: TestRG5
* Hely: Japan East
* TestVNet5: 10.51.0.0/16 és 10.52.0.0/16
* Előtér: 10.51.0.0/24
* Háttér: 10.52.0.0/24
* Átjáróalhálózat: 10.52.255.0.0/27
* Átjáró neve: VNet5GW
* Nyilvános IP-cím: VNet5GWIP
* VPN típusa: RouteBased
* Kapcsolat: VNet5toVNet1
* Kapcsolat típusa: VNet2VNet

### <a name="step-7---create-and-configure-testvnet5"></a>7. lépés – A TestVNet5 létrehozása és konfigurálása

Ez a lépés hello új előfizetés hello környezetében kell végrehajtani. Ez a kijelző hello hello előfizetés tulajdonosa egy másik szervezet rendszergazdája hajthatja végre.

1. Deklarálja a változókat. Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

  ```powershell
  $Sub5 = "Replace_With_the_New_Subcription_Name"
  $RG5 = "TestRG5"
  $Location5 = "Japan East"
  $VnetName5 = "TestVNet5"
  $FESubName5 = "FrontEnd"
  $BESubName5 = "Backend"
  $GWSubName5 = "GatewaySubnet"
  $VnetPrefix51 = "10.51.0.0/16"
  $VnetPrefix52 = "10.52.0.0/16"
  $FESubPrefix5 = "10.51.0.0/24"
  $BESubPrefix5 = "10.52.0.0/24"
  $GWSubPrefix5 = "10.52.255.0/27"
  $GWName5 = "VNet5GW"
  $GWIPName5 = "VNet5GWIP"
  $GWIPconfName5 = "gwipconf5"
  $Connection51 = "VNet5toVNet1"
  ```
2. Csatlakozás toosubscription 5. Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók. A következő minta toohelp csatlakozás hello használata:

  ```powershell
  Login-AzureRmAccount
  ```

  Hello előfizetések hello fiók ellenőrzése.

  ```powershell
  Get-AzureRmSubscription
  ```

  Adja meg, hogy szeretné-e toouse hello előfizetés.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName $Sub5
  ```
3. Hozzon létre egy új erőforráscsoportot.

  ```powershell
  New-AzureRmResourceGroup -Name $RG5 -Location $Location5
  ```
4. Hozzon létre hello TestVNet5 alhálózati konfigurációi.

  ```powershell
  $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
  $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
  $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5
  ```
5. Hozza létre a TestVNet5-öt.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
  -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5
  ```
6. Kérjen egy nyilvános IP-címet.

  ```powershell
  $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
  -Location $Location5 -AllocationMethod Dynamic
  ```
7. Hozzon létre hello átjáró konfigurációját.

  ```powershell
  $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
  $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
  $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5
  ```
8. Hello TestVNet5 átjáró létrehozása.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
  -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1
  ```

### <a name="step-8---create-hello-connections"></a>8. lépés – hello kapcsolatok létrehozása

Ebben a példában hello átjárók hello különböző előfizetésekhez, mert azt már ossza fel ezt a lépést két PowerShell-munkamenetekben jelölésű [előfizetés 1] és [előfizetés 5].

1. **[1. előfizetés]**  Hello virtuális hálózati átjáró előfizetés 1. Jelentkezzen be, és tooSubscription 1 csatlakozzon a következő példa hello futtatása előtt:

  ```powershell
  $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
  ```

  Másolja a következő elemek hello hello kimenetét, és küldjön e-mailben vagy más módszerrel előfizetés 5 ezek toohello rendszergazda.

  ```powershell
  $vnet1gw.Name
  $vnet1gw.Id
  ```

  Ez a két elem értékek hasonló toohello egy példa a kimenetre a következő lesz:

  ```
  PS D:\> $vnet1gw.Name
  VNet1GW
  PS D:\> $vnet1gw.Id
  /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```
2. **[Előfizetés 5]**  Hello virtuális hálózati átjáró előfizetés 5. Jelentkezzen be, és tooSubscription 5 csatlakozzon a következő példa hello futtatása előtt:

  ```powershell
  $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5
  ```

  Másolja a következő elemek hello hello kimenetét, és küldjön e-mailben vagy más módszerrel előfizetés 1 ezek toohello rendszergazda.

  ```powershell
  $vnet5gw.Name
  $vnet5gw.Id
  ```

  Ez a két elem értékek hasonló toohello egy példa a kimenetre a következő lesz:

  ```
  PS C:\> $vnet5gw.Name
  VNet5GW
  PS C:\> $vnet5gw.Id
  /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```
3. **[1. előfizetés]**  Hello TestVNet1 tooTestVNet5 kapcsolat létrehozásához. Ebben a lépésben készíthet TestVNet1 tooTestVNet5 hello kapcsolat. hello itt különbség, hogy $vnet5gw nem lehet közvetlenül beolvasni, mert egy másik előfizetésben. A fenti lépések hello előfizetés 1 közölt hello értékekkel kell toocreate egy új PowerShell-objektum. Az alábbi példa hello használata. Hello nevét, a azonosítója és a megosztott kulcs cserélje le a saját értékeit. fontos dolog hello megosztott kulcs hello egyeznie kell mindkét kapcsolatokhoz. Kapcsolat létrehozása során toocomplete rövid is igénybe vehet.

  Csatlakozás tooSubscription 1 hello például a következő futtatása előtt:

  ```powershell
  $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet5gw.Name = "VNet5GW"
  $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
  $Connection15 = "VNet1toVNet5"
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
4. **[Előfizetés 5]**  Hello TestVNet5 tooTestVNet1 kapcsolat létrehozásához. Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet5 tooTestVNet1 hello kapcsolatot hoz létre. hello azonos előfizetés 1-től kapott hello értékek alapján PowerShell-objektum létrehozásának folyamatát, valamint alkalmazza itt. Ebben a lépésben ügyeljen, hogy megfelel-e a megosztott hello kulcsok.

  Csatlakozás tooSubscription 5 hello például a következő futtatása előtt:

  ```powershell
  $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
  $vnet1gw.Name = "VNet1GW"
  $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
  New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

## <a name="verify"></a>Hogyan tooverify kapcsolat

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Következő lépések

* Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. Lásd: hello [Virtual Machines – dokumentáció](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) további információt.
* A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
