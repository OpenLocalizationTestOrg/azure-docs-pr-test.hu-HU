---
title: "Csatlakozás az Azure-beli virtuális hálózat tooanother VNet: portál |} Microsoft Docs"
description: "Erőforrás-kezelő használatával Vnetek közötti VPN-átjáró kapcsolatot, és hello Azure-portálon."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: a529f90d976bee0f50403947d06e9da8a6c05349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-using-hello-azure-portal"></a>Konfigurálja a VNet – VNet VPN gateway-kapcsolatot hello Azure-portál használatával

Ez a cikk bemutatja, hogyan toocreate virtuális hálózatok közötti VPN gateway-kapcsolattal. hello virtuális hálózatok lehetnek azonos vagy különböző régiókban hello, és a hello azonos vagy különböző előfizetésekhez. Amikor másik előfizetésből, hello előfizetések csatlakozó Vnetek nem kell társított toobe hello azonos Active Directory-bérlő. 

a cikkben ismertetett hello toohello Resource Manager üzembe helyezési modellben vonatkoznak, és hello Azure-portált használja. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Különböző üzemi modellek összekapcsolása – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Különböző üzemi modellek összekapcsolása – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![v2v ábra](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hasonló tooconnecting egy VNet tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat. Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooconsider csatlakoztatja őket a Vnetben társviszony-létesítés használatával. A virtuális hálózatok közötti társviszony nem használ VPN-átjárót. További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).

A virtuális hálózatok közötti kommunikáció kombinálható többhelyes konfigurációkkal. Ez lehetővé teszi a felhasználó közötti kapcsolatot nyújthassanak közötti virtuális hálózati kapcsolattal rendelkező hálózati topológiák létrehozása a hello a következő ábrán látható módon:

![Tudnivalók a kapcsolatokról](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Tudnivalók a kapcsolatokról")

### <a name="why-connect-virtual-networks"></a>Miért érdemes összekapcsolni a virtuális hálózatokat?

A következő okok miatt hello érdemes lehet tooconnect virtuális hálózatok:

* **Georedundancia és földrajzi jelenlét több régióban**
  
  * Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.
  * Az Azure Traffic Manager és a Load Balancer segítségével magas rendelkezésre állású munkaterhelést állíthat be georedundanciával több Azure-régióban. Egy fontos példája tooset fel SQL Always On rendelkezésre állási csoportokkal több Azure-régiók keresztül terjednek.
* **Regionális többrétegű alkalmazások elkülönítéssel vagy felügyeleti határral**
  
  * Belül hello azonos régióban, beállíthat több rétegből álló alkalmazások összekapcsolása tooisolation vagy felügyeleti követelmények miatt több virtuális hálózaton.

VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén. Vegye figyelembe, hogy ha a Vnetekhez különböző előfizetésekhez, akkor nem hozható létre hello kapcsolat hello portálon. Ehelyett használhatja a [PowerShellt](vpn-gateway-vnet-vnet-rm-ps.md) vagy a [parancssori felületet](vpn-gateway-howto-vnet-vnet-cli.md).

### <a name="values"></a>Példabeállítások

Ha ezeket a lépéseket a-t egy gyakorlatot, hello példa beállítások értékeket is használhat. Csupán a példa kedvéért több címteret használunk az egyes virtuális hálózatokhoz. Azonban a virtuális hálózatok közötti kapcsolat konfigurálásához nincs szükség több címtérre.

**Értékek a TestVNet1-hez:**

* Virtuális hálózat neve: TestVNet1
* Címtér: 10.11.0.0/16
  * Alhálózat neve: FrontEnd
  * Alhálózati címtartomány: 10.11.0.0/24
* Erőforráscsoport: TestRG1
* Hely: East US
* Címtér: 10.12.0.0/16
  * Alhálózat neve: BackEnd
  * Alhálózati címtartomány: 10.12.0.0/24
* Átjáró alhálózati név: GatewaySubnet (ez fog automatikus kitöltés hello portálon)
  * Átjáróalhálózat címtartománya: 10.11.255.0/27
* DNS-kiszolgáló: Hello IP-címet a DNS-kiszolgáló használata
* Virtuális hálózati átjáró neve: TestVNet1GW
* Átjáró típusa: VPN
* VPN típusa: Útvonalalapú
* Termékváltozat: Válassza ki a kívánt toouse átjáró-Termékváltozat hello
* Nyilvános IP-cím neve: TestVNet1GWIP
* Csatlakozási értékek:
  * Név: TestVNet1toTestVNet4
  * Megosztott kulcs: hello megosztott kulcs saját kezűleg hozhat létre. Ebben a példában az abc123-at használjuk. hello fontos, hogy amikor hello Vnetek hello kapcsolatot hoz létre, hello értékének meg kell felelnie.

**Értékek a TestVNet4-hez:**

* Virtuális hálózat neve: TestVNet4
* Címtér: 10.41.0.0/16
  * Alhálózat neve: FrontEnd
  * Alhálózati címtartomány: 10.41.0.0/24
* Erőforráscsoport: TestRG1
* Hely: West US
* Címtér: 10.42.0.0/16
  * Alhálózat neve: BackEnd
  * Alhálózati címtartomány: 10.42.0.0/24
* A GatewaySubnet name: GatewaySubnet (ez fog automatikus kitöltés hello portálon)
  * Átjáró-alhálózat címtartománya: 10.41.255.0/27
* DNS-kiszolgáló: Hello IP-címet a DNS-kiszolgáló használata
* Virtuális hálózati átjáró neve: TestVNet4GW
* Átjáró típusa: VPN
* VPN típusa: Útvonalalapú
* Termékváltozat: Válassza ki a kívánt toouse átjáró-Termékváltozat hello
* Nyilvános IP-cím neve: TestVNet4GWIP
* Csatlakozási értékek:
  * Név: TestVNet4toTestVNet1
  * Megosztott kulcs: hello megosztott kulcs saját kezűleg hozhat létre. Ebben a példában az abc123-at használjuk. hello fontos, hogy amikor hello Vnetek hello kapcsolatot hoz létre, hello értékének meg kell felelnie.

## <a name="CreatVNet"></a>1. A TestVNet1 létrehozása és konfigurálása
Ha már rendelkezik egy Vnetet, győződjön meg arról, hogy a VPN-átjáró kialakítás kompatibilisek legyenek-e hello-beállítások. Különösen figyeljen az egyéb hálózatokkal átfedhetik tooany alhálózatokat. Egymást átfedő alhálózatok esetén a kapcsolat nem fog megfelelően működni. Ha a virtuális hálózat hello megfelelő beállításokkal van konfigurálva, elkezdheti hello hello lépéseit [adjon meg egy DNS-kiszolgáló](#dns) szakasz.

### <a name="toocreate-a-virtual-network"></a>a virtuális hálózati toocreate
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="subnets"></a>2. További címterek hozzáadása és alhálózatok létrehozása
Miután létrehozta a virtuális hálózatot, további címtereket adhat hozzá és alhálózatokat hozhat létre.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. Átjáróalhálózat létrehozása
A virtuális hálózati tooa átjáró csatlakozás előtt először toocreate hello átjáróalhálózatot hello virtuális hálózati toowhich tooconnect keresi. Ha lehetséges a legjobb toocreate egy átjáró-alhálózatot CIDR-blokkja /28 vagy /27 használatát rendelés tooprovide elegendő IP-címek tooaccommodate jövőbeli további konfigurációs követelményeket is.

Ha ezt a konfigurációt, mint egy gyakorlatot hoz létre, tekintse meg a toothese [példa beállítások](#values) az átjáró-alhálózat létrehozásakor.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="toocreate-a-gateway-subnet"></a>toocreate egy átjáró-alhálózatot
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>4. DNS-kiszolgáló megadása (nem kötelező)
A virtuális hálózatok közötti kapcsolatokhoz nincs szükség DNS-re. Ha szeretne toohave névfeloldás telepített tooyour virtuális hálózati erőforrásokhoz, a DNS-kiszolgáló kell megadnia. Ezzel a beállítással adható meg, amelyet az toouse névfeloldás a virtuális hálózat hello DNS-kiszolgáló. A beállítás nem hoz létre új DNS-kiszolgálót.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. Virtuális hálózati átjáró létrehozása
Ebben a lépésben a virtuális hálózat hozza létre hello virtuális hálózati átjárót. Átjáró létrehozása gyakran eltarthat 45 perc vagy több, attól függően, hogy a kiválasztott átjáróeszközön hello Termékváltozat. Ha ezt a konfigurációt, mint egy gyakorlatot hoz létre, olvassa el a toohello [példa beállítások](#values).

### <a name="toocreate-a-virtual-network-gateway"></a>a virtuális hálózati átjáró toocreate
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="CreateTestVNet4"></a>6. A TestVNet4 létrehozása és konfigurálása
TestVNet1 konfigurálása után létrehozhat TestVNet4 ismétlődő hello előző lépést, az TestVNet4 hello értékek cseréje. Toowait nem kell, amíg hello virtuális hálózati átjáró TestVNet1 TestVNet4 konfigurálása előtt létrehozása befejeződött. Használatakor a saját értékeit, győződjön meg arról, hogy hello címterek nem fednek át hello tooconnect a kívánt Vnetek bármelyikét.

## <a name="TestVNet1Connection"></a>7. Hello TestVNet1 kapcsolat konfigurálása
Hello virtuális hálózati átjárók TestVNet1 és TestVNet4 rendelkezik befejezése után is létrehozhat a virtuális hálózati átjáró kapcsolatok. Ebben a szakaszban a VNet1 tooVNet4 kapcsolatot hoz létre. Ezek a lépések csak hello a Vnetek azonos előfizetéssel. Ha a Vnetekhez különböző előfizetésekhez, PowerShell toomake hello kapcsolatot kell használnia. Lásd: hello [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) cikk.

1. A **összes erőforrás**, keresse meg a virtuális hálózati átjáró toohello a Vnethez tartozó. Például: **TestVNet1GW**. Kattintson a **TestVNet1GW** tooopen hello virtuális hálózati átjáró panelen.
   
    ![Kapcsolatpanel](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Kapcsolatpanel")
2. Kattintson a **+ Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen.
3. A hello **kapcsolat hozzáadása a** panelen hello név mezőbe írja be a kapcsolat nevét. Például: **TestVNet1toTestVNet4**.
   
    ![Kapcsolat neve](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Kapcsolat neve")
4. A **Kapcsolat típusa** mezőben. Válassza ki **VNet – VNet** hello legördülő menüből.
5. Hello **első virtuális hálózati átjáró** mezőérték automatikusan kitölti, mert a megadott virtuális hálózati átjáró hello alapján hoz létre a kapcsolat.
6. Hello **második virtuális hálózati átjáró** mezője hello virtuális hálózati átjáró hello VNet a megjeleníteni kívánt toocreate kapcsolatot. Kattintson a **egy másik virtuális hálózati átjáró kiválasztása** tooopen hello **válasszon virtuális hálózati átjáró** panelen.
   
    ![Kapcsolat hozzáadása](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Kapcsolat hozzáadása")
7. Nézet hello virtuális hálózati átjárók ezen a panelen található. Csak azok a virtuális hálózati átjárók jelennek meg, amelyek az előfizetésében szerepelnek. Ha azt szeretné, hogy tooconnect tooa virtuális hálózati átjáró, amely nem szerepel az előfizetés, használjon hello [PowerShell cikk](vpn-gateway-vnet-vnet-rm-ps.md). 
8. Kattintson a tooconnect kívánt hello virtuális hálózati átjáró.
9. A hello **megosztott kulcs** mezőbe írja be a kapcsolat egy megosztott kulcsot. A kulcsot generálhatja, vagy saját maga is létrehozhatja. A pont-pont kapcsolatban hello kulcs használata volna kell pontosan hello ugyanazt a helyszíni eszközök és a virtuális hálózati átjáró kapcsolat. hello koncepciójuk hasonló itt, kivéve, amelyek ahelyett, hogy a kapcsolódó tooa VPN-eszköz tooanother virtuális hálózati átjáró csatlakozik.
   
    ![Megosztott kulcs](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Megosztott kulcs")
10. Kattintson a **OK** : hello hello panel toosave alján a módosításokat.

## <a name="TestVNet4Connection"></a>8. Hello TestVNet4 kapcsolat konfigurálása
Következő lépésként hozzon létre kapcsolatot TestVNet4 tooTestVNet1. Használja az azonos hello toocreate hello kapcsolatot TestVNet1 tooTestVNet4 használt módszer. Győződjön meg arról, hogy hello használja ugyanazt a közös kulcs.

## <a name="VerifyConnection"></a>9. A kapcsolat ellenőrzése
Hello kapcsolat ellenőrzéséhez. Az hello minden egyes virtuális hálózati átjáró a következő:

1. Keresse meg a hello virtuális hálózati átjáró hello paneljét. Például: **TestVNet4GW**. 
2. Hello virtuális hálózati átjáró paneljén kattintson **kapcsolatok** tooview hello kapcsolatok panelt hello virtuális hálózati átjáró.

Hello kapcsolatok megtekintése és hello állapotának ellenőrzése. Hello kapcsolat létrehozása után megjelenik **sikeres** és **csatlakoztatva** , hello állapotértékeket.

![Sikeres](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Sikeres")

Minden egyes kapcsolathoz külön-külön duplán kattintva tooview hello kapcsolat további információt.

![Alapvető szolgáltatások](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Alapvető szolgáltatások")

## <a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések
További információt a VNet – VNet kapcsolatokhoz hello – gyakori kérdések a részletek megtekintéséhez.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Következő lépések
Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. Lásd: hello [Virtual Machines – dokumentáció](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) további információt.
