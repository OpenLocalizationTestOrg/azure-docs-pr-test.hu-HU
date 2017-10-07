---
title: "Csatlakozás a virtuális hálózati tooanother VNet: Azure parancssori Felülettel |} Microsoft Docs"
description: "Ez a cikk lépésről lépésre bemutatja, hogyan csatlakoztathatók a virtuális hálózatok egymáshoz az Azure Resource Manager és az Azure CLI használatával."
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
ms.openlocfilehash: 70113914bcae03c80f9ad133ff081d1cf37fc309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-azure-cli"></a>Virtuális hálózatok közötti VPN Gateway-kapcsolat konfigurálása az Azure CLI használatával

Ez a cikk bemutatja, hogyan toocreate virtuális hálózatok közötti VPN gateway-kapcsolattal. hello virtuális hálózatok lehetnek azonos vagy különböző régiókban hello, és a hello azonos vagy különböző előfizetésekhez. Amikor másik előfizetésből, hello előfizetések csatlakozó Vnetek nem kell társított toobe hello azonos Active Directory-bérlő. 

a cikkben ismertetett hello toohello Resource Manager üzembe helyezési modellben vonatkoznak, és az Azure parancssori felület használata. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

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

![Tudnivalók a kapcsolatokról](./media/vpn-gateway-howto-vnet-vnet-cli/aboutconnections.png)

### <a name="why"></a>Miért érdemes összekapcsolni a virtuális hálózatokat?

A következő okok miatt hello érdemes lehet tooconnect virtuális hálózatok:

* **Georedundancia és földrajzi jelenlét több régióban**

  * Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.
  * Az Azure Traffic Manager és a Load Balancer segítségével magas rendelkezésre állású munkaterhelést állíthat be georedundanciával több Azure-régióban. Egy fontos példája tooset fel SQL Always On rendelkezésre állási csoportokkal több Azure-régiók keresztül terjednek.
* **Regionális többrétegű alkalmazások elkülönítéssel vagy felügyeleti határral**

  * Belül hello azonos régióban, beállíthat több rétegből álló alkalmazások összekapcsolása tooisolation vagy felügyeleti követelmények miatt több virtuális hálózaton.

VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén.

### <a name="which-set-of-steps-should-i-use"></a>Melyik eljárást használjam?

Ebben a cikkben kétféle lépéssorozatot láthat. Lépéseit egy készletét [lévő Vnetek hello ugyanahhoz az előfizetéshez](#samesub), majd egy másikat a [Vnetekhez különböző előfizetések lévő](#difsub).

## <a name="samesub"></a>Csatlakozzon a hello Vnetek ugyanahhoz az előfizetéshez

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-cli/v2vrmps.png)

### <a name="before-you-begin"></a>Előkészületek

Megkezdése előtt telepítse a legújabb verziójának hello hello parancssori felület parancsait (2.0-s vagy újabb). Hello parancssori felület parancsait telepítésével kapcsolatos információkért lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).

### <a name="Plan"></a>Az IP-címtartományok megtervezése

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


### <a name="Connect"></a>1. lépés – kapcsolódás tooyour előfizetés

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-numbers-include.md)]

### <a name="TestVNet1"></a>2. lépés – A TestVNet1 létrehozása és konfigurálása

1. Hozzon létre egy erőforráscsoportot.

  ```azurecli
  az group create -n TestRG1  -l eastus
  ```
2. Hozzon létre TestVNet1 és hello alhálózat TestVNet1. Ez a példa létrehoz egy TestVNet1 nevű virtuális hálózatot és egy FrontEnd nevű alhálózatot.

  ```azurecli
  az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.11.0.0/16 -l eastus --subnet-name FrontEnd --subnet-prefix 10.11.0.0/24
  ```
3. Létrehozhat egy további címtartományt a hello backend alhálózathoz. Figyelje meg, hogy ebben a lépésben azt adja meg a korábban létrehozott mindkét hello címtér, és hogy szeretnénk tooadd további címtartományok hello. Ennek az az oka hello [az hálózat virtuális hálózat frissítése](https://docs.microsoft.com/cli/azure/network/vnet#update) parancs felülírja az előző hello-beállítások. Győződjön meg arról, hogy toospecify minden hello címelőtag a parancs használatakor.

  ```azurecli
  az network vnet update -n TestVNet1 --address-prefixes 10.11.0.0/16 10.12.0.0/16 -g TestRG1
  ```
4. Hozzon létre hello backend alhálózathoz.
  
  ```azurecli
  az network vnet subnet create --vnet-name TestVNet1 -n BackEnd -g TestRG1 --address-prefix 10.12.0.0/24 
  ```
5. Hozzon létre hello átjáró-alhálózatot. Figyelje meg, hogy hello átjáró alhálózatának elnevezése "GatewaySubnet". A név megadása kötelező. Ebben a példában hello átjáróalhálózatot egy /27 használ. Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet legalább /28 vagy /27 kiválasztásával. Ez lehetővé teszi elég címek tooaccommodate lehetséges olyan beállításokat, amelyeket érdemes lehet a hello jövőbeli.

  ```azurecli 
  az network vnet subnet create --vnet-name TestVNet1 -n GatewaySubnet -g TestRG1 --address-prefix 10.12.255.0/27
  ```
6. A nyilvános IP-cím toobe lefoglalt toohello átjáró fog létrehozni a Vnethez tartozó kérelmet. Figyelje meg, hogy hello AllocationMethod dinamikus. Nem adható meg, hogy szeretné-e toouse hello IP-címet. Fontos a dinamikusan kiosztott tooyour átjáró.

  ```azurecli
  az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
  ```
7. TestVNet1 hello virtuális hálózati átjáró létrehozása A virtuális hálózatok közötti konfigurációkhoz a VpnType paraméternek RouteBased értékűnek kell lennie. A parancs futtatásakor hello használatával – nincs - wait paramétert, ha bármely visszajelzést vagy kimeneti nem látható. hello – nincs - wait paramétert lehetővé teszi, hogy hello átjáró toocreate hello háttérben. Azt nem jelenti azt, hogy hello VPN-átjáró végzett a létrehozással azonnal. Átjáró létrehozása gyakran eltarthat 45 percig vagy tovább hello gateway SKU, amelyekkel függően.

  ```azurecli
  az network vnet-gateway create -n VNet1GW -l eastus --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="TestVNet4"></a>3. lépés – A TestVNet4 létrehozása és konfigurálása

1. Hozzon létre egy erőforráscsoportot.

  ```azurecli
  az group create -n TestRG4  -l westus
  ```
2. Hozza létre a TestVNet4-et.

  ```azurecli
  az network vnet create -n TestVNet4 -g TestRG4 --address-prefix 10.41.0.0/16 -l westus --subnet-name FrontEnd --subnet-prefix 10.41.0.0/24
  ```

3. Hozzon létre további alhálózatokat a TestVNet4-hez.

  ```azurecli
  az network vnet update -n TestVNet4 --address-prefixes 10.41.0.0/16 10.42.0.0/16 -g TestRG4 
  az network vnet subnet create --vnet-name TestVNet4 -n BackEnd -g TestRG4 --address-prefix 10.42.0.0/24 
  ```
4. Hozzon létre hello átjáró-alhálózatot.

  ```azurecli
   az network vnet subnet create --vnet-name TestVNet4 -n GatewaySubnet -g TestRG4 --address-prefix 10.42.255.0/27
  ```
5. Kérjen egy nyilvános IP-címet.

  ```azurecli
  az network public-ip create -n VNet4GWIP -g TestRG4 --allocation-method Dynamic
  ```
6. Hello TestVNet4 virtuális hálózati átjáró létrehozása.

  ```azurecli
  az network vnet-gateway create -n VNet4GW -l westus --public-ip-address VNet4GWIP -g TestRG4 --vnet TestVNet4 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="createconnect"></a>4. lépés – hello kapcsolatok létrehozása

Most már két, VPN-átjáróval rendelkező virtuális hálózata van. következő lépés hello toocreate VPN-átjáró kapcsolatok hello virtuális hálózati átjárók között. Ha követte a fenti példákban hello, a virtuális hálózat átjárók különböző erőforráscsoportokra szerepelnek. Ha az átjárók különböző erőforráscsoportok vannak, ehhez tooidentify kell és hello erőforrás-azonosítók minden átjáró, ha a kapcsolat. Ha a Vnetek hello ugyanabban az erőforráscsoportban, használhatja a hello [utasításokat második együttesét](#samerg) mert toospecify hello erőforrás-azonosítók nem szükséges.

### <a name="diffrg"></a>tooconnect Vnetekhez különböző erőforráscsoportokra található

1. Hello VNet1GW Azonosítójú erőforrás lekérése a következő parancs hello hello kimenete:

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  Hello kimenet, megkeresi a hello "azonosító:" sor. hello hello idézőjelek értékei szükséges toocreate hello kapcsolat hello a következő szakaszban. Ezen értékek tooa szövegszerkesztőben, például a Jegyzettömbben másolja, így könnyen illessze be őket a kapcsolat létrehozásakor.

  Példa a kimenetre:

  ```
  "activeActive": false, 
  "bgpSettings": { 
    "asn": 65515, 
    "bgpPeeringAddress": "10.12.255.30", 
    "peerWeight": 0 
   }, 
  "enableBgp": false, 
  "etag": "W/\"ecb42bc5-c176-44e1-802f-b0ce2962ac04\"", 
  "gatewayDefaultSite": null, 
  "gatewayType": "Vpn", 
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW", 
  "ipConfigurations":
  ```

  Miután hello értékek másolása **"id":** hello ajánlatok belül.

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
 ```

2. Első hello VNet4GW erőforrás azonosítója, és másolja hello értékek tooa szövegszerkesztőben.

  ```azurecli
  az network vnet-gateway show -n VNet4GW -g TestRG4
  ```

3. Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása. Ebben a lépésben készíthet TestVNet1 tooTestVNet4 hello kapcsolat. Nincs a megosztott kulcs hello példák hivatkozik. Hello megosztott kulcs saját értékeket is használhat. fontos dolog hello megosztott kulcs hello egyeznie kell mindkét kapcsolatokhoz. Toocomplete közben rövid kapcsolat létrehozása időt vesz igénybe.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW 
  ```
4. Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása. Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet4 tooTestVNet1 hello kapcsolatot hoz létre. Ellenőrizze, hogy megosztott hello kulcsok felel meg. Ez néhány percet vesz igénybe tooestablish hello kapcsolat.

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG4 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG4/providers/Microsoft.Network/virtualNetworkGateways/VNet4GW -l westus --shared-key "aabbcc" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1G
  ```
5. Ellenőrizze a kapcsolatokat. Lásd: [A kapcsolat ellenőrzése](#verify).

### <a name="samerg"></a>hello megtalálható azonos Vnetek tooconnect erőforráscsoport

1. Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása. Ebben a lépésben készíthet TestVNet1 tooTestVNet4 hello kapcsolat. Értesítés hello erőforráscsoportok vannak hello hello példákban azonos. Látható egy megosztott kulcsot hivatkozott hello példákban is. Hello megosztott kulcs saját értékeket is használhat, azonban a hello megosztott kulcs egyeznie kell mindkét kapcsolatok. Toocomplete közben rövid kapcsolat létrehozása időt vesz igénybe.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet4 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet4GW
  ```
2. Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása. Ez a lépés akkor hasonló toohello valamelyik fenti, kivéve a TestVNet4 tooTestVNet1 hello kapcsolatot hoz létre. Ellenőrizze, hogy megosztott hello kulcsok felel meg. Ez néhány percet vesz igénybe tooestablish hello kapcsolat.

  ```azurecli
  az network vpn-connection create -n VNet4ToVNet1 -g TestRG1 --vnet-gateway1 VNet4GW -l eastus --shared-key "eeffgg" --vnet-gateway2 VNet1GW
  ```
3. Ellenőrizze a kapcsolatokat. Lásd: [A kapcsolat ellenőrzése](#verify).

## <a name="difsub"></a>Különböző előfizetésben található virtuális hálózatok összekapcsolása

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-cli/v2vdiffsub.png)

Ebben a forgatókönyvben csatlakoztatjuk a TestVNet1 és a TestVNet5 virtuális hálózatot. hello Vnetekhez különböző előfizetések találhatók. hello előfizetések nem kell hello társított toobe azonos Active Directory-bérlő. Ehhez a konfigurációhoz hello lépéseket rendelés tooconnect TestVNet1 tooTestVNet5 egy további VNet – VNet-kapcsolatot hozzáadni.

### <a name="TestVNet1diff"></a>5. lépés – A TestVNet1 létrehozása és konfigurálása

Ezek az utasítások továbbra is a fentebbi szakaszokban hello hello lépéseit. Meg kell adnia a [1. lépés](#Connect) és [2. lépés](#TestVNet1) toocreate TestVNet1 hello VPN Gateway és TestVNet1 konfigurálni. Ehhez a konfigurációhoz akkor használják, nem szükséges toocreate TestVNet4 az előző szakasz hello, bár hoz létre, ha ez nem ütközik ezeket a lépéseket. Miután elvégezte az 1. lépést és a 2. lépést, folytassa a 6. lépéssel (lásd alább).

### <a name="verifyranges"></a>6. lépés - hello IP-címtartományok ellenőrzése

További kapcsolatok létrehozása esetén, amelyek hello IP-címtér hello új virtuális hálózat nem fedi át a többi virtuális hálózat tartomány vagy helyi hálózati átjáró tartományok egyik fontos tooverify. Ebben a gyakorlatban a következő értékek hello TestVNet5 hello használhatja:

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

### <a name="TestVNet5"></a>7. lépés – A TestVNet5 létrehozása és konfigurálása

Ez a lépés hello új előfizetés, előfizetés 5 hello környezetében kell végrehajtani. Ez a kijelző hello hello előfizetés tulajdonosa egy másik szervezet rendszergazdája hajthatja végre. előfizetések használata között tooswitch "az fióklista--minden" toolist hello előfizetések elérhető tooyour fiókot, majd használja "az fiók beállítása – előfizetési <subscriptionID>" tooswitch toohello előfizetést, amelyet az toouse.

1. Győződjön meg arról, hogy csatlakoztatott tooSubscription 5, akkor hozzon létre egy erőforráscsoportot.

  ```azurecli
  az group create -n TestRG5  -l japaneast
  ```
2. Hozza létre a TestVNet5-öt.

  ```azurecli
  az network vnet create -n TestVNet5 -g TestRG5 --address-prefix 10.51.0.0/16 -l japaneast --subnet-name FrontEnd --subnet-prefix 10.51.0.0/24
  ```

3. Adjon hozzá alhálózatokat.

  ```azurecli
  az network vnet update -n TestVNet5 --address-prefixes 10.51.0.0/16 10.52.0.0/16 -g TestRG5
  az network vnet subnet create --vnet-name TestVNet5 -n BackEnd -g TestRG5 --address-prefix 10.52.0.0/24
  ```

4. Hello átjáró alhálózatának hozzáadása.

  ```azurecli
  az network vnet subnet create --vnet-name TestVNet5 -n GatewaySubnet -g TestRG5 --address-prefix 10.52.255.0/27
  ```

5. Kérjen egy nyilvános IP-címet.

  ```azurecli
  az network public-ip create -n VNet5GWIP -g TestRG5 --allocation-method Dynamic
  ```
6. Hello TestVNet5 átjáró létrehozása

  ```azurecli
  az network vnet-gateway create -n VNet5GW -l japaneast --public-ip-address VNet5GWIP -g TestRG5 --vnet TestVNet5 --gateway-type Vpn --sku VpnGw1 --vpn-type RouteBased --no-wait
  ```

### <a name="connections5"></a>8. lépés – hello kapcsolatok létrehozása

Azt ossza fel ezt a lépést olyan jelölésű két parancssori munkamenetbe **[előfizetés 1]**, és **[előfizetés 5]** mert hello átjárók hello különböző előfizetésekhez. előfizetések használata között tooswitch "az fióklista--minden" toolist hello előfizetések elérhető tooyour fiókot, majd használja "az fiók beállítása – előfizetési <subscriptionID>" tooswitch toohello előfizetést, amelyet az toouse.

1. **[1. előfizetés]**  Jelentkezzen be, és csatlakozzon a tooSubscription 1. Futtatási hello következő parancsot a tooget hello nevét és Azonosítóját hello hello kimenetből átjáró:

  ```azurecli
  az network vnet-gateway show -n VNet1GW -g TestRG1
  ```

  A hello kimenetének másolása HTML "azonosító:". Küldési hello azonosítója és neve hello hello hálózatok átjáró (VNet1GW) toohello rendszergazda előfizetés 5 e-mailben vagy más módszerrel.

  Példa a kimenetre:

  ```
  "id": "/subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW"
  ```

2. **[Előfizetés 5]**  Jelentkezzen be, és csatlakozzon a tooSubscription 5. Futtatási hello következő parancsot a tooget hello nevét és Azonosítóját hello hello kimenetből átjáró:

  ```azurecli
  az network vnet-gateway show -n VNet5GW -g TestRG5
  ```

  A hello kimenetének másolása HTML "azonosító:". Küldjön hello azonosítója és neve hello hello VNet átjáró (VNet5GW) toohello rendszergazda az előfizetés 1 e-mailben vagy más módszerrel.

3. **[1. előfizetés]**  Ebben a lépésben hoz létre hello kapcsolat TestVNet1 tooTestVNet5. Hello megosztott kulcs saját értékeket is használhat, azonban a hello megosztott kulcs egyeznie kell mindkét kapcsolatok. Kapcsolat létrehozása során toocomplete rövid is igénybe vehet. Ellenőrizze, hogy tooSubscription 1 kapcsolódik.

  ```azurecli
  az network vpn-connection create -n VNet1ToVNet5 -g TestRG1 --vnet-gateway1 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW -l eastus --shared-key "eeffgg" --vnet-gateway2 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW
  ```

4. **[Előfizetés 5]**  Ezt a lépést nem hasonló toohello valamelyik fenti, kivéve a TestVNet5 tooTestVNet1 hello kapcsolatot hoz létre. Győződjön meg arról, hogy hello megosztott kulcsok megfeleltetéséhez és a tooSubscription 5 csatlakozni.

  ```azurecli
  az network vpn-connection create -n VNet5ToVNet1 -g TestRG5 --vnet-gateway1 /subscriptions/e7e33b39-fe28-4822-b65c-a4db8bbff7cb/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW -l japaneast --shared-key "eeffgg" --vnet-gateway2 /subscriptions/d6ff83d6-713d-41f6-a025-5eb76334fda9/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW
  ```

## <a name="verify"></a>Hello kapcsolatok ellenőrzése
[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

[!INCLUDE [verify connections v2v cli](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

## <a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések
[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Következő lépések

* Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: hello [Virtual Machines – dokumentáció](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
