---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN: portál |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. A lépések segítségével hozhat létre egy hello portál használatával létesítmények közötti pont-pont VPN Gateway-kapcsolatot."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 6f0acbaf1bf016026cefade048a116e94686103d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-in-hello-azure-portal"></a>Pont-pont kapcsolat létrehozása hello Azure-portálon

Ez a cikk bemutatja, hogyan toouse hello Azure portál toocreate a helyszíni hálózati toohello VNet a pont-pont VPN gateway-kapcsolattal. a cikkben ismertetett hello alkalmazása toohello Resource Manager üzembe helyezési modellben. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Parancssori felület](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül. Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel. További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).

![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Előkészületek

Győződjön meg arról, hogy teljesül-e az hello feltételeket a konfigurációs megkezdése előtt a következő:

* Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt. További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).
* Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára. Ez az IP-cím nem lehet NAT mögötti.
* Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki. Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani. A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül. 

### <a name="values"></a>Példaértékek

a cikkben szereplő példák hello hello a következő értékeket használja. Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.

* **Virtuális hálózat neve:** TestVNet1
* **Címtér:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (nem kötelező ehhez a gyakorlathoz)
* **Alhálózatok:**
  * Előtér: 10.11.0.0/24
  * Háttér: 10.12.0.0/24 (nem kötelező ehhez a gyakorlathoz)
* **Átjáróalhálózat:** 10.11.255.0/27
* **Erőforráscsoport:** TestRG1
* **Hely:** az USA keleti régiója
* **DNS-kiszolgáló:** Nem kötelező. a DNS-kiszolgáló hello IP-címe.
* **Virtuális hálózati átjáró neve:** VNet1GW
* **Nyilvános IP-cím:** VNet1GWIP
* **VPN típusa:** útvonalalapú
* **Kapcsolat típusa:** helyek közötti kapcsolat (IPsec)
* **Átjáró típusa:** VPN
* **Helyi hálózati átjáró neve:** Site2
* **Kapcsolat neve:** VNet1toSite2

## <a name="CreatVNet"></a>1. Virtuális hálózat létrehozása

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. DNS-kiszolgáló megadása

DNS nincs szükség toocreate a pont-pont kapcsolat. Ha szeretne toohave névfeloldás telepített tooyour virtuális hálózati erőforrásokhoz, a DNS-kiszolgáló kell megadnia. Ezzel a beállítással adható meg, amelyet az toouse névfeloldás a virtuális hálózat hello DNS-kiszolgáló. A beállítás nem hoz létre új DNS-kiszolgálót. A névfeloldással kapcsolatos további információkért tekintse meg [A virtuális gépek és szerepkörpéldányok névfeloldása](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) című cikket.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Hozzon létre hello átjáró-alhálózatot

[!INCLUDE [vpn-gateway-aboutgwsubnet](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. Hello VPN-átjáró létrehozása

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Hello helyi hálózati átjáró létrehozása

hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik. Adjon hello hely egy nevet, amellyel Azure is tekintse meg a tooit, majd adja meg hello IP-címet a hello a helyszíni VPN-eszköz toowhich létrehoz egy kapcsolatot. Is meg hello IP-cím előtagokat hello VPN gateway toohello VPN-eszközön keresztül történik. megadott hello címelőtagokat a helyszíni hálózaton található hello előtagok. Ha a helyszíni hálózati módosítások vagy toochange hello nyilvános IP-cím szükséges hello VPN-eszközön, később könnyen frissítheti hello értékeket.

[!INCLUDE [Add local network gateway](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. VPN-eszköz konfigurálása

Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség. Ebben a lépésben a VPN-eszköz konfigurálása következik. A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:

- Megosztott kulcs. Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs. A példákban alapvető megosztott kulcsot használunk. Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.
- hello a virtuális hálózati átjáró nyilvános IP-címét. Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg. toofind hello nyilvános IP-cím a VPN-átjáró használatával hello Azure-portálon lépjen túl**virtuális hálózati átjárók**, majd kattintson az átjárója nevére hello.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Hello VPN-kapcsolat létrehozása

A virtuális hálózati átjáró és a helyszíni VPN-eszköz közötti pont-pont hello VPN-kapcsolat létrehozása.

[!INCLUDE [Add connections](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Hello VPN-kapcsolat ellenőrzése

[!INCLUDE [Verify - Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="connectVM"></a>tooconnect tooa virtuális gép

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="reset"></a>Hogyan tooreset VPN-átjáró

Az Azure VPN Gateway alaphelyzetbe állítása akkor hasznos, ha egy vagy több helyek közötti VPN-alagúton elveszíti a létesítmények közötti VPN-kapcsolatot. Ebben a helyzetben a helyszíni VPN-eszközök minden megfelelően működik-e, de nem tudta tooestablish hello Azure VPN gatewayek az IPsec-alagutak. A lépéseket lásd: [VPN Gateway alaphelyzetbe állítása](vpn-gateway-resetgw-classic.md).

## <a name="resize"></a>Hogyan toochange egy átjáró-Termékváltozat (átméretezési átjáró)

Hello lépések toochange egy átjáró-Termékváltozat, a következő témakörben: [Gateway SKU-n](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="next-steps"></a>Következő lépések

* A BGP kapcsolatos információkért lásd: hello [BGP áttekintése](vpn-gateway-bgp-overview.md) és [hogyan tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Információk a kényszerített bújtatásról: [Információk a kényszerített bújtatásról](vpn-gateway-forced-tunneling-rm.md).
* Információk a magas rendelkezésre állású aktív-aktív kapcsolatokról: [Magas rendelkezésre állású kapcsolatok létesítmények, illetve virtuális hálózatok között](vpn-gateway-highlyavailable.md).
