---
title: "Tervezési és kialakítási létesítmények közötti kapcsolatok: Azure VPN Gateway |} Microsoft Docs"
description: "További tudnivalók a VPN-átjáró tervezése és kialakítása létesítmények közötti, hibrid és VNet – VNet kapcsolatokhoz"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 3d4587ba31d163384212eca88a7e2c0ba8f3b21f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a>A VPN Gateway tervezése és kialakítása

Megtervezéséről és kialakításáról a létesítmények közötti és VNet – VNet konfigurációkkal lehet egyszerű vagy összetett, hálózati igényeitől függően. Ez a cikk bemutatja, hogyan alapvető tervezési és kialakítási szempontjai.

## <a name="planning"></a>Tervezése

### <a name="compare"></a>Létesítmények közötti kapcsolati lehetőségek

Ha azt szeretné, hogy tooconnect a helyszíni helyek biztonságosan tooa virtuális hálózati telepítette, három különböző módon toodo ezért:-webhelyek, pont-pont és az ExpressRoute. Hasonlítsa össze a rendelkezésre álló hello különböző létesítmények közötti kapcsolatok. a kiválasztott hello lehetőség például különböző szempontok függ:

* Mekkora átviteli sebesség szükséges a megoldásához?
* Szeretné, hogy toocommunicate keresztül hello nyilvános Internet biztonságos VPN-en keresztül, vagy egy személyes kapcsolaton keresztül?
* Nyilvános IP cím elérhető toouse van?
* A VPN-eszköz toouse tervezési? Ha igen, ez kompatibilis a rendszerrel?
* Csak néhány számítógépet csatlakoztatna, vagy állandó kapcsolatra van szüksége a helyéhez?
* Milyen típusú VPN-átjárót kell hello megoldás toocreate keresi?
* Melyik átjárót SKU használja?

### <a name="planningtable"></a>Tervezési táblázat

hello a következő táblázat segítségével eldöntheti, hogy a megoldás legjobb hello kapcsolódási beállítást választja.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>Átjáró-termékváltozatok

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>Munkafolyamat

hello következő felsorolás vázolja hello közös munkafolyamat felhő kapcsolathoz:

1. Tervezési és a kapcsolat topológia és a lista hello címterük összes hálózat akkor terv szeretné tooconnect.
2. Hozzon létre egy Azure virtuális hálózatra. 
3. Hozzon létre egy VPN-átjáró hello virtuális hálózat.
4. Létrehozhat és konfigurálhat kapcsolatok tooon helyi hálózatok és a többi virtuális hálózatok (szükség szerint).
5. Létrehozhat és konfigurálhat egy pont – hely kapcsolat az Azure VPN gateway (szükség szerint).

## <a name="design"></a>Tervezési
### <a name="topologies"></a>Kapcsolat topológiák

Indítsa el a hello hello diagramok megtekintésével [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikk. a cikkben hello egyszerű diagramokat, hello üzembe helyezési modellel minden topológia, és hello elérhető eszközök toodeploy a konfigurációt használhatja.

### <a name="designbasics"></a>Tervezési alapjai

a következő szakaszok hello hello VPN gateway alapjai tárgyalja. 

#### <a name="servicelimits"></a>Hálózatkezelési szolgáltatások korlátok

Hello táblák tooview görgetéséhez [hálózati szolgáltatási korlátok](../azure-subscription-service-limits.md#networking-limits). felsorolt hello korlátok hatással lehet a tervező.

#### <a name="subnets"></a>Alhálózatok kapcsolatos

Kapcsolatok létrehozásakor meg kell fontolnia alhálózati tartományt. Nem lehet átfedésben lévő alhálózati címtartományt. Egy átfedő alhálózattal akkor, ha egy virtuális hálózat vagy a helyszíni helyen található hello ugyanazt a címtartományt, amely más helyen hello tartalmazza. Ez azt jelenti, hogy van szüksége a hálózati mérnökök a helyszíni helyi hálózatok toocarve ki egy tartományt az Ön toouse a Azure IP-címzés terület/alhálózatok. Címterületek használatát, amelyek nem használják a hello helyi a helyi hálózaton van szüksége.

Egymást átfedő alhálózatokat elkerülve az során is fontos dolgozik VNet – VNet kapcsolatokhoz. Ha az alhálózat átfedésben vannak, és egy IP-cím szerepel a hello küld, és a célkiszolgáló Vnetek, VNet – VNet kapcsolatokhoz sikertelen. Azure nem útvonal hello adatok toohello más VNet mert hello célcím hello küldése a virtuális hálózat része.

VPN-átjárók nevű egy átjáró-alhálózatot a megadott alhálózat szükséges. Minden átjáró-alhálózat névvel kell ellátni a GatewaySubnet toowork megfelelően. Ne feledje nem tooname az átjáró alhálózatának egy másik nevet, és nem telepítheti a virtuális gépek vagy semmi mást toohello átjáró-alhálózatot. Lásd: [átjáró alhálózatok](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Tudnivalók a helyi hálózati átjáró

hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik. Hello klasszikus üzembe helyezési modellel hello helyi hálózati átjáró hivatkozott tooas egy helyi hálózati telephely. A helyi hálózati átjáró konfigurálásakor ehhez adjon neki egy nevet, adja meg a nyilvános IP-címe hello hello a helyszíni VPN-eszközön, és hello helyszíni helyen hello címelőtagok. Azure ellenőrzi, hogy az hello cél címelőtagokat a hálózati forgalom, hello helyi hálózati átjáró megadott hello konfigurációs olvas, és ennek megfelelően irányítja a csomagokat. Hello címelőtagokat igény szerint módosíthatók. További információkért lásd: [helyi hálózati átjárók](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Átjáró típusával kapcsolatban

A topológia hello megfelelő átjáró típusának kiválasztása fontos. Ha hello megfelelő típusú választja, az átjáró nem fog megfelelően működni. hello átjáró típusa határozza meg, hogyan hello átjáró csatlakozik, és a szükséges konfigurációs beállítást hello Resource Manager üzembe helyezési modellben.

hello átjáró típusok a következők:

* Vpn
* ExpressRoute

#### <a name="connectiontype"></a>Kapcsolattípusok kapcsolatos

Minden egyes konfiguráció esetében egy adott kapcsolattípusra van szükség. hello kapcsolattípusai a következők:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

#### <a name="vpntype"></a>Információ a VPN-típusai

Minden egyes konfiguráció szükséges egy adott VPN-típus. Ha meg vannak kombinálásával két konfigurációkat, például egy webhelyek kapcsolat és egy pont – hely kapcsolat toohello hozhat létre ugyanazon virtuális kell használnia a VPN-típus, amely eleget tesz a mindkét kapcsolódási követelményeihez.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

hello alábbi táblázatokban hello VPN-típus, le kell képezni tooeach kapcsolat konfigurációját. Győződjön meg arról, hogy hello az átjáró megfelel hello konfigurációjában, amelyet az toocreate VPN-típus. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>A pont-pont kapcsolatok VPN-eszközök

a következő elemek hello kell tooconfigure a pont-pont kapcsolat, függetlenül a telepítési modell:

* A VPN-eszköz, amely kompatibilis a Azure VPN gatewayek
* Egy nyilvánosan elérhető IPv4 IP-címet, amely nincs NAT mögött.

A VPN-eszköz konfigurálása toohave élmény van szüksége, vagy rendelkezik valaki, amely hello eszköz konfigurálása.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Fontolja meg kényszerített bújtatás Útválasztás

A legtöbb konfigurációk esetén konfigurálhatja a kényszerített bújtatást. A kényszerített bújtatás lehetővé átirányítási vagy "kényszerített" minden Internet adathoz kötött forgalom hátsó tooyour helyszíni hely vizsgálati és naplózási pont-pont VPN-alagúton keresztül. Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek. 

Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban lesz mindig haladnak át közvetlenül kimenő Internet, toohello nélkül hello beállítás tooallow Azure hálózati infrastruktúráról, tooinspect vagy a napló hello forgalom. Jogosulatlan Internet-hozzáférés is eredményezhet, tooinformation nyilvánosságra vagy más típusú biztonsági problémákat.

A kényszerített bújtatás kapcsolat két üzembe helyezési modell és a különböző eszközök használatával konfigurálható. További információkért lásd: [kényszerített bújtatás konfigurálása](vpn-gateway-forced-tunneling-rm.md).

**A kényszerített bújtatás diagramja**

![Az Azure VPN Gateway kényszerített bújtatás diagramja](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Következő lépések

Lásd: hello [VPN Gateway – gyakori kérdések](vpn-gateway-vpn-faq.md) és [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikkek esetében további információk toohelp, a kialakítással.

Adott átjáró beállításaival kapcsolatos további információkért lásd: [kapcsolatos VPN-átjáró beállítások](vpn-gateway-about-vpn-gateway-settings.md).