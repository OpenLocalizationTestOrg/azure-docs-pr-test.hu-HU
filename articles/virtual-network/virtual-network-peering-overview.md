---
title: "aaaAzure virtuális hálózati társviszony-létesítés |} Microsoft Docs"
description: "Tudnivalók az Azure-beli virtuális hálózatok közötti társviszony-létesítésről"
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: narayan
ms.openlocfilehash: 46a14b416a7d4389f79a3cd7c55e388b5d312577
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network-peering"></a>Társviszony létesítése virtuális hálózatok között
Virtuális hálózati társviszony-létesítési lehetővé teszi, hogy Ön tooconnect két virtuális hálózatok hello ugyanabban a régióban keresztül hello Azure hálózat. Ha nincsenek társviszonyban, hello két virtuális hálózatok egy kapcsolat célokra jelennek meg. a rendszer továbbra is kezeli, külön erőforrások hello két virtuális hálózatok, de hello virtuális hálózatok társítottak, azonban a virtuális gépek kommunikálhatnak egymással közvetlenül, magánhálózati IP-címek használatával.

hello forgalom hello virtuális gépek közötti társítottak, virtuális hálózatok hello Azure-infrastruktúra, lényegében ugyanúgy hello virtuális gépek közötti továbbítódik áthalad ugyanazt a virtuális hálózatot. Hello segítségével a virtuális hálózati társviszony-létesítés előnyei a következők:

* Kis késésű, nagy sávszélességű kapcsolat jön létre eltérő virtuális hálózatokba tartozó erőforrások között.
* hello képességét toouse erőforrások, például hálózati berendezéseket és VPN-átjárók átvitel pontként peered virtuális hálózatban.
* hello képességét toopeer két létrehozott virtuális hálózatokat keresztül hello Azure Resource Manager üzembe helyezési modellben vagy egy virtuális hálózati toopeer létrehozott erőforrás-kezelő tooa hello klasszikus telepítési modell használatával létrehozott virtuális hálózaton keresztül. Olvasási hello [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) cikk toolearn hello különbségei hello két Azure üzembe helyezési modellel kapcsolatos további.

## <a name="requirements-constraints"></a>Követelmények és korlátozások

* hello társítottak, virtuális hálózatok már léteznie kell hello azonos Azure-régiót. Különböző Azure-régióban lévő virtuális hálózatok [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) használatával kapcsolhatók össze.
* hello virtuális hálózatok társítottak, rendelkeznie kell egymást nem átfedő IP-címterületeken.
* Miután egy virtuális hálózat társviszonyba lépett egy másik virtuális hálózattal, nem adható hozzá és nem törölhető belőle címtér.
* A virtuális hálózati társviszony két virtuális hálózat között jön létre. A társviszonyok nem adódnak tovább tranzitív módon. Például ha virtualNetworkA nincsenek társviszonyban, a virtualNetworkB, és virtualNetworkB nincsenek társviszonyban, a virtualNetworkC, virtualNetworkA van *nem* társviszonyban toovirtualNetworkC.
* Akkor is partnert hosszú szintű jogosultságokkal rendelkező felhasználóként két különböző előfizetésekhez, meglévő virtuális hálózatok (lásd: [konkrét engedélyeket](create-peering-different-deployment-models-subscriptions.md#permissions)) együtt előfizetések engedélyezi hello társviszony-létesítést, és hello előfizetések társított toohello azonos Az Azure Active Directory-bérlő. Használhatja a [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect virtuális hálózatok előfizetésekhez tartozó toodifferent Active Directory-bérlő.
* Virtuális hálózatok is társítottak, ha mindkét keresztül hello Resource Manager üzembe helyezési modellben jönnek létre, vagy ha egy virtuális hálózat létrehozása révén hello Resource Manager üzembe helyezési modellben, és más hello hello klasszikus telepítési modell használatával jön létre. Két, hello klasszikus telepítési modell használatával létrehozott virtuális hálózatokat társviszonyban tooeach más, azonban nem lehet. Használhatja a [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect két hello klasszikus telepítési modell használatával létrehozott virtuális hálózatokat.
* Bár a virtuális gépek nincsenek társviszonyban, virtuális hálózatok közötti hello kommunikáció nincs további sávszélesség-korlátozás van, nincs maximális hálózati sávszélesség még mindig érvényes hello virtuális gép méretétől függően. További információk a különböző virtuális gépek méretét, olvassa el a hello maximális hálózati sávszélességének toolearn [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) vagy [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuális gép méretének cikkeket.
* Az Azure által a virtuális gépekre vonatkozóan biztosított belső DNS-névfeloldás nem működik két társviszonyban álló virtuális hálózat között. A virtuális gépek feloldható hello helyi virtuális hálózaton belül csak belső DNS-nevek rendelkeznek. Ugyanakkor csatlakozó virtuális gépek toopeered virtuális hálózatok konfigurálása a virtuális hálózat DNS-kiszolgálóként. További tudnivalókért olvassa el a hello [névfeloldáshoz a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) cikk.

![Társviszony létesítése virtuális hálózatok között alapszinten](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Kapcsolatok
Miután két virtuális hálózatok társviszonyban van, vagy virtuális hálózatán lévő erőforrásokat közvetlenül kapcsolódhatnak hello társviszonyban virtuális hálózatán lévő erőforrásokat. hello két virtuális hálózat is létezik a teljes IP-szintű kapcsolat.

hello hálózati késés a két virtuális gépek nincsenek társviszonyban, virtuális hálózatok közötti oda-vissza hello ugyanaz, mint egy virtuális hálózaton belül oda-vissza. hello hálózati teljesítményt hello sávszélesség hello virtuális gép esetén arányos tooits mérete engedélyezett alapul. Nem áll rendelkezésre további korlátozás hello társviszony-létesítés sávszélességét.

virtuális gépek nincsenek társviszonyban, virtuális hálózatok közötti hello forgalom közvetlenül hello Azure háttér-infrastruktúra, nem az átjárón keresztül továbbít.

Virtuális gépek csatlakoztatott tooa virtuális hozzáférhet hello belső elosztott terhelésű végpont hello társviszonyban virtuális hálózatban. Hálózati biztonsági csoportok vagy a virtuális hálózati tooblock hozzáférési tooother virtuális hálózatok, vagy az alhálózatok, a alkalmazhatók, ha szükséges.

Amikor konfigurálja a virtuális hálózati társviszony-létesítést, nyissa meg, vagy zárja be a hello hálózati biztonsági csoportszabályok hello virtuális hálózatok között. Nyissa meg a teljes kapcsolat társítottak, virtuális hálózatok közötti (amely hello alapértelmezett beállítás), ha hálózati biztonsági csoportok toospecific alhálózatok és virtuális gépek tooblock alkalmazni, vagy adott megtagadja. További részletek toolearn hálózati biztonsági csoportok, olvassa el a hello [hálózati biztonsági csoportok – áttekintés](virtual-networks-nsg.md) cikk.

## <a name="service-chaining"></a>Szolgáltatásláncolás
Felhasználó által definiált útvonalak adott pont toovirtual gépeket konfigurálhat a virtuális hálózatok társviszonyban, "a következő ugrás" IP cím tooenable szolgáltatás láncolás hello. Szolgáltatás-láncolás lehetővé teszi, hogy Ön toodirect érkező forgalom egy virtuális hálózati tooa virtuális készülék egy felhasználó által definiált útvonalak peered virtuális hálózatot.

Hub és küllős típus környezetekben, ahol hello hub tárolhatja, például a hálózati virtuális készülék összetevőket is hatékonyan hozhat létre. Az összes hello küllős virtuális hálózatot is majd társkapcsolatot létesíteni hello hub virtuális hálózat. Virtuális készülékek hálózati hello hub virtuális hálózaton futó keresztül áramolhasson az adatforgalom. Virtuális hálózati társviszony-létesítés rövid, lehetővé teszi a hello következő ugrás IP-címét a felhasználó által definiált hello útvonal toobe hello IP-címe hello társviszonyban virtuális hálózatban lévő virtuális gép. További információk a felhasználó által definiált útvonalak, olvassa el a hello toolearn [felhasználó által definiált útvonalak áttekintése](virtual-networks-udr-overview.md) cikk.

## <a name="gateways-and-on-premises-connectivity"></a>Átjárók és kapcsolat helyszíni rendszerekkel
Minden virtuális hálózathoz, függetlenül attól, hogy nincsenek társviszonyban egy másik virtuális hálózathoz, továbbra is saját átjárót, és tooconnect tooan a helyszíni hálózat használni. Beállíthatja úgy is [virtuális hálózat – virtuális hálózati kapcsolatok](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) átjáró, használatával, annak ellenére, hogy a virtuális hálózatok hello társviszonyban van.

Ha mindkét lehetőséget a virtuális hálózati összekapcsolhatóság vannak beállítva, hello hello virtuális hálózatok közötti forgalom hello társviszony-létesítési konfiguráció keresztül (Ez azt jelenti, hogy a hello, Azure gerincét).

Virtuális hálózatok vannak társviszonyban, amikor beállíthatja úgy is hello átjáró hello virtuális hálózaton nincsenek társviszonyban, a helyszíni hálózati átvitel során pont tooan. Ebben az esetben a hello virtuális hálózat által használt távoli átjáró nem lehet saját átjáró. Egy virtuális hálózatnak csak egy átjárója lehet. hello átjáró lehet egy helyi vagy távoli átjáró (hello társviszonyban virtuális hálózat), ahogy az alábbi képen hello:

![Átvitel virtuális társhálózat esetén](./media/virtual-networks-peering-overview/figure02.png)

Átjáró átvitel nem támogatott hello segítségével különböző üzembe helyezési modellel létrehozott virtuális hálózatok közötti társviszony-létesítési kapcsolatban. Mindkét virtuális hálózat hello társviszony-létesítési kapcsolatban kell létrehozott erőforrás-kezelő egy átjáró átvitel toowork.

Virtuális hálózatok hello egyetlen Azure ExpressRoute-kapcsolat által megosztott társviszonyban van, amikor hello adatforgalom végighalad hello társviszony-létesítési kapcsolat (Ez azt jelenti, hogy a hello, Azure hálózat). A virtuális hálózati tooconnect toohello helyszíni kapcsolatok továbbra is használhatja helyi átjárók. Közös átjárót is használhat, és átvitel konfigurálásával létesíthet kapcsolatot a helyszíni rendszerrel.

## <a name="provisioning"></a>Kiépítés
A virtuális hálózatok közötti társviszony-létesítés jogosultsághoz van kötve. Hello VirtualNetworks wsrmp funkcióhoz. A felhasználói jogok tooauthorize társviszony-létesítés adható meg. Egy felhasználó, aki rendelkezik olvasási és írási hozzáférése toohello virtuális hálózati automatikusan örökli ezeket a jogokat.

Egy felhasználó vagy rendszergazda vagy a kiemelt felhasználó hello társviszony-létesítési képességeit egy másik virtuális hálózati társviszony-létesítési műveletet is kezdeményezhető. Társviszony egyező kérelem esetén a másik oldalon hello, és egyéb követelmények teljesülése esetén hello társviszony-létesítés jön létre.

## <a name="limits"></a>Korlátok
Nincsenek korlátozások hello esetében, amelyek jogosultak a egyetlen virtuális hálózat számára. További információkért tekintse át a hello [Azure hálózati korlátok](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Díjszabás
Egy névleges díj vonatkozik a társhálózati viszonyt használó bejövő és kimenő forgalomra. További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Következő lépések

* Végezzen el egy oktatóanyagot a virtuális hálózatok közötti társviszony létesítéséhez. Virtuális hálózati társviszony-létesítés jön létre, hello létre azonos virtuális hálózatok között, vagy meglévő különböző üzembe helyezési modellel hello ugyanazon vagy másik előfizetést. A következő forgatókönyvek hello egyike egy oktatóanyag elvégzéséhez:
 
    |Azure üzembehelyezési modell  | Előfizetés  |
    |---------|---------|
    |Mindkét Resource Manager |[Ugyanaz](virtual-network-create-peering.md)|
    | |[Különböző](create-peering-different-subscriptions.md)|
    |Egy Resource Manager, egy klasszikus     |[Ugyanaz](create-peering-different-deployment-models.md)|
    | |[Különböző](create-peering-different-deployment-models-subscriptions.md)|

* Megtudhatja, hogyan toocreate egy [küllős hálózati topológia](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
* További tudnivalók az összes [virtuális hálózati társviszony-létesítési beállításait, és hogyan toochange őket](virtual-network-manage-peering.md)
