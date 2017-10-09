---
title: "VPN-átjáró – áttekintés: TooAzure virtuális hálózatok létrehozása létesítmények közötti VPN-kapcsolatok |} Microsoft Docs"
description: "A VPN-átjáró – áttekintés hello módon tooconnect tooAzure virtuális hálózatok hello interneten keresztül VPN-kapcsolattal ismerteti. Tartalmazza az alapvető kapcsolatkonfigurációk ábráit is."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2017
ms.author: cherylmc
ms.openlocfilehash: 899270734270632a5b12d56021c924e977725a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway"></a>Információk a VPN Gateway-ről

VPN-átjáró egy olyan virtuális hálózati átjáró által a titkosított forgalmat egy nyilvános kapcsolat tooan között a helyszíni hely. VPN-átjárók toosend titkosított forgalmat hello Microsoft hálózaton keresztül az Azure virtuális hálózat között is használható. az Azure virtuális hálózat és a helyszíni hely közötti toosend titkosított a hálózati forgalom, létre kell hoznia egy VPN-átjáró a virtuális hálózat.

Minden virtuális hálózathoz csak egy VPN-átjáró rendelkezhet, azonban több kapcsolatok toohello hozhat létre ugyanazon VPN-átjáró. Erre egy példa a Többhelyes csatlakozás konfiguráció. Ha több kapcsolatok toohello hoz létre ugyanazon VPN-átjárót, az összes VPN-alagutat, beleértve a pont-pont VPN, megosztás hello sávszélességét hello átjáró számára.

### <a name="whatis"></a>Mi az a virtuális hálózati átjáró?

A virtuális hálózati átjáró két áll, vagy további virtuális gépeket telepített hello GatewaySubnet nevű tooa alhálózatokhoz. hello GatewaySubnet jön létre, amikor hello virtuális hálózati átjáró létrehozása hello található virtuális gépek. Virtuális gépek virtuális hálózati átjáró konfigurált toocontain útválasztási táblázataiba, és a szolgáltatások adott toohello átjárók. Hello hello virtuális hálózati átjáró részét képező virtuális gépek közvetlenül nem állítható be, és további források toohello GatewaySubnet soha ne telepítsen.

A virtuális hálózati átjáró hello átjáró típusa "Vpn" használatával hoz létre, amikor létrehoz egy adott típusú virtuális hálózati átjáró, amely titkosítja a forgalmat; VPN-átjáró. VPN-átjáró too45 perc toocreate is eltarthat. Mivel a virtuális gépek hello hello VPN Gateway folyamatban van a telepített toohello GatewaySubnet, és a megadott hello beállításokat. hello kiválasztott átjáró-Termékváltozat meghatározza, hogy milyen teljesítményű hello virtuális gépeken.

## <a name="gwsku"></a>Átjáró-termékváltozatok

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

## <a name="configuring"></a>VPN Gateway-átjáró konfigurálása

A VPN-átjárós kapcsolatok több erőforrást használnak, amelyek speciális beállításokkal konfigurálhatók. Hello erőforrások többségét külön-külön konfigurálhatók, noha bizonyos esetekben egyes sorrendben kell konfigurálni.

### <a name="settings"></a>Beállítások

az egyes erőforrások hello beállításokat kritikus toocreating a sikeres kapcsolat. További információ a VPN Gateway önálló erőforrásairól és beállításairól: [About VPN Gateway settings](vpn-gateway-about-vpn-gateway-settings.md) (Információk a VPN Gateway beállításairól). a cikkben hello információk toohelp átjárótípusok, VPN-típusokat, kapcsolattípusokat, átjáró-alhálózatot, helyi hálózati átjárók és különböző más erőforrás-beállítások, amelyeknél felmerülhet tooconsider ismertetése.

### <a name="tools"></a>Üzembehelyezési eszközök

Kimenő létrehozásához és konfigurálásához egy konfigurálása eszköz, például a hello Azure-portál használatával erőforrások megkezdése. Később tooswitch tooanother eszköz, például a PowerShell tooconfigure további erőforrások, döntse el, vagy módosítsa a meglévő erőforrásokat, ha alkalmazható. Jelenleg nem lehet konfigurálni minden erőforrás- és erőforrás-beállítás hello Azure-portálon. minden kapcsolat topológia hello cikkek hello utasításait adja meg, amikor egy adott konfigurációs eszköz szükséges. 

### <a name="models"></a>Üzemi modell

VPN-átjáró konfigurálásakor hello lépései függenek hello telepítési modell amellyel toocreate a virtuális hálózat. Például ha hozott létre a virtuális hálózat használatával hello klasszikus üzembe helyezési modellel, használatakor hello irányelvek és hello klasszikus üzembe helyezési utasításokat toocreate modell, és a VPN-átjáró beállításainak konfigurálásában. További információk az üzembe helyezési modellekről: [Az erőforrás-kezelői és a klasszikus üzembe helyezési modellek ismertetése](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="diagrams"></a>Kapcsolati topológia-diagramok

Fontos, hogy az érhetők el más konfigurációk VPN-átjáró kapcsolatok tooknow. Ajánlott konfiguráció megfelel az igényeinek toodetermine van szüksége. Hello lentebbi, megtekintheti a következő virtuális Magánhálózati átjárókapcsolatokhoz hello kapcsolatos információkat és topológia diagramok: a következő szakaszok hello tartalmazhat, amely a listában:

* Elérhető üzemi modell
* Elérhető konfigurációs eszközök
* Hivatkozásokra közvetlenül tooan cikk, ha elérhető

Használjon hello diagramok és leírások toohelp hello kapcsolat topológia toomatch adja meg a szükséges követelményeket. hello diagramokon hello fő alapterv topológiákat, de lehetséges toobuild összetettebb konfigurációknál hello ábrák használatával az eszközöket.

## <a name="s2smulti"></a>Helyek közötti és többhelyes (IPsec/IKE VPN-alagút)

### <a name="S2S"></a>Helyek közötti kapcsolat

A helyek közötti (Site-to-Site, S2S) VPN Gateway-kapcsolat egy IPsec/IKE (IKEv1 vagy IKEv2) VPN-alagúton keresztüli kapcsolat. Az S2S kapcsolathoz kell egy VPN található helyszíni Eszközkezelési, egy nyilvános IP-cím tooit és a NAT mögött nem található A helyek közötti kapcsolatok létesítmények közötti és hibrid konfigurációk esetében is alkalmazhatók.   

![Azure VPN Gateway helyek közti kapcsolat – példa](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Többhelyes kapcsolat

Ez a kapcsolat típus hello pont-pont kapcsolat egy változata. A virtuális hálózati átjáró egynél több VPN-kapcsolatot hoz létre, általában a toomultiple csatlakozás helyszíni hely. Ha több kapcsolatot használ, RouteBased (útvonalalapú) VPN-típust kell alkalmaznia (klasszikus virtuális hálózatok használatakor ezt dinamikus átjárónak nevezik). Mivel minden virtuális hálózathoz csak egy VPN-átjáró, az összes kapcsolat hello átjárón keresztül megosztása hello rendelkezésre álló sávszélesség. Ezt gyakran „többhelyes” kapcsolatnak nevezik.

![Azure VPN Gateway többhelyes kapcsolat – példa](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Üzembe helyezési modellek és módszerek a helyek közötti és többhelyes kapcsolatokhoz

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Pont–hely típusú (SSTP-alapú VPN)

Pont-pont (P2S) VPN-átjáró lehetővé teszi a biztonságos kapcsolat tooyour virtuális hálózat létrehozása az egyéni ügyfél-számítógépről. Pont-pont VPN-kapcsolatok akkor hasznos, ha azt szeretné, hogy a virtuális hálózat egy távoli helyről, például amikor, amelyek dolgozzon home vagy konferencia tooconnect tooyour. P2S VPN esetén is egy hasznos megoldás toouse helyett a telephelyek közötti VPN tooconnect tooa VNet kell csak néhány ügyféllel. 

A helyek közötti kapcsolatoktól eltérően a pont–hely kapcsolatok nem igényelnek helyszíni nyilvános IP-címet vagy VPN-eszközt a működéshez. P2S-kapcsolatok használható az S2S-kapcsolatok keresztül VPN-átjárón azonos hello mindaddig, amíg az összes hello konfigurációs követelmények, a mindkét kapcsolatok kompatibilis.

P2S használja a Secure Socket Tunneling Protocol (SSTP), amely SSL-alapú VPN-protokoll hello. P2S VPN-kapcsolatot létesít hello ügyfélszámítógépről elindításával.

![Azure VPN Gateway pont–hely kapcsolat – példa](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>Üzembe helyezési modellek és módszerek pont–hely kapcsolatokhoz

[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>Virtuális hálózatok közötti kapcsolatok (IPsec/IKE VPN-alagút)

Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hasonló tooconnecting egy VNet tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat. A virtuális hálózatok közötti kommunikációt kombinálhatja többhelyes kapcsolati konfigurációkkal is. Így létrehozhat olyan hálózati topológiákat, amelyek a létesítmények közötti kapcsolatokat a virtuális hálózatok közötti kapcsolatokkal kombinálják.

Csatlakozás Vnetek hello lehet:

* a hello azonos vagy különböző régiókban
* a hello ugyanazon vagy másik előfizetések 
* a hello ugyanazon vagy másik üzembe helyezési modellek

![Az Azure VPN Gateway VNet tooVNet kapcsolat – példa](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

### <a name="connections-between-deployment-models"></a>Üzemi modellek közötti kapcsolat

Az Azure jelenleg kétféle üzemi modellt kínál: a klasszikust és a Resource Managert. Ha már egy ideje használja az Azure-t, valószínűleg futtat Azure virtuális gépeket és példányszerepköröket egy klasszikus virtuális hálózaton. Lehetséges, hogy az újabb virtuális gépek és szerepkörpéldányok egy Resource Managerben létrehozott virtuális hálózatban futnak. A közvetlenül egy másik erőforrásaival egy VNet toocommunicate hello Vnetek tooallow közötti kapcsolat hello erőforrások hozhat létre.

### <a name="vnet-peering"></a>Virtuális hálózatok közötti társviszony

Képes toouse hálózatokból toocreate a hálózati kapcsolatot, lehet, amíg a virtuális hálózat bizonyos követelményeknek. A virtuális hálózatok közötti társviszony nem használ virtuális hálózati átjárót. További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md).

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Üzembe helyezési modellek és módszerek virtuális hálózatok közötti kapcsolatokhoz

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (dedikált privát kapcsolat)

A Microsoft Azure ExpressRoute lehetővé teszi a helyszíni hálózatokhoz kiterjeszti a Microsoft cloud hello könnyíteni a kapcsolat szolgáltatóját egy dedikált titkos kapcsolaton keresztül. Az ExpressRoute létrehozhat kapcsolatok tooMicrosoft felhőszolgáltatások, például a Microsoft Azure, a Office 365 és a CRM Online-hoz. A kapcsolatok lehetnek: bármely elemek közötti (IP VPN) hálózat, pontok közötti Ethernet-hálózat vagy egy virtuális keresztkapcsolat egy kapcsolatszolgáltatón keresztül egy közös elhelyezési létesítményben.

Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. Ez lehetővé teszi ExpressRoute kapcsolatok toooffer további megbízhatóságát, gyorsabb sebességű, kisebb késések fordulnak elő, és nagyobb biztonságot nyújtana tipikus kapcsolatok hello interneten keresztül.

Az ExpressRoute-kapcsolatok nem használnak VPN-átjárót, azonban használnak virtuális hálózati átjárót a szükséges konfiguráció részeként. Egy ExpressRoute-kapcsolatot, a hello virtuális hálózati átjáró hello átjáró típusa "Vpn" helyett "ExpressRoute" van konfigurálva. ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Egyidejű helyek közötti és ExpressRoute-kapcsolatok

ExpressRoute-e a WAN hálózaton való közvetlen, dedikált kapcsolat (nem pedig a nyilvános interneten hello) tooMicrosoft szolgáltatásokat, például az Azure. Pont-pont típusú VPN-forgalom titkosítva hello utazás nyilvános internethez. Sem tudja tooconfigure telephelyek közötti VPN és ExpressRoute kapcsolatok hello azonos virtuális hálózatban több előnye is van.

Biztonságos feladatátvételi elérési útnak a telephelyek közötti VPN konfigurálása ExpressRoute, vagy használja a pont-pont VPN tooconnect toosites, amelyek nem részei a hálózaton, de ExpressRoute keresztül csatlakoznak. Figyelje meg, hogy ezt a konfigurációt igényel a két virtuális hálózati átjáró hello azonos virtuális hálózatban, egyet átjáró típusa "Vpn" hello, és egyéb használatával hello átjáró típusa "ExpressRoute" hello.

![ExpressRoute és VPN egyidejű kapcsolatok – példa](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Üzembe helyezési modellek és módszerek a helyek közti (S2S) és ExpressRoute-kapcsolatokhoz

[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Díjszabás

[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

További információ a VPN Gateway átjáró-termékváltozatokról: [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Átjáró-termékváltozatok).

## <a name="faq"></a>GYIK

VPN-átjáróval kapcsolatos gyakori kérdésekre, lásd: hello [VPN Gateway – gyakori kérdések](vpn-gateway-vpn-faq.md).

## <a name="next-steps"></a>Következő lépések

- A VPN-átjáró konfigurációjának megtervezése Lásd: [A VPN-átjáró tervezése és kialakítása](vpn-gateway-plan-design.md).
- Nézet hello [VPN Gateway – gyakori kérdések](vpn-gateway-vpn-faq.md) további információt.
- Nézet hello [előfizetés és a szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md#networking-limits).
- Megismerhet néhány hello más kulcs [hálózati lehetőségeket](../networking/networking-overview.md) Azure.
