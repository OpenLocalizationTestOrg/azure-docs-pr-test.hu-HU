---
title: "aaaUser által definiált útvonalak és IP-továbbítást az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure felhasználó által definiált útvonalak (UDR) és az IP-továbbítás tooforward forgalom virtuális készülékek toonetwork az Azure-ban."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: c39076c4-11b7-4b46-a904-817503c4b486
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f1f1d46166d5a7c776f472b7ade1354d943ece10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-routes-and-ip-forwarding"></a>Felhasználó által megadott útvonalak és IP-továbbítás

Amikor az Azure virtuális gépek (VM) tooa virtuális hálózathoz (VNet), megfigyelheti, hogy hello virtuális gépek automatikusan legyenek-e egymással képes toocommunicate hello hálózaton keresztül. Az átjáró toospecify nem kell annak ellenére, hogy a virtuális gépek hello külön alhálózatokon vannak. hello is igaz hello virtuális gépek toohello közti kommunikációhoz nyilvános interneten, és még akkor is, tooyour a helyszíni hálózatra is, ha egy hibrid kapcsolat az Azure tooyour saját datacenter jelen.

Kommunikáció ilyen típusú áramlása azért lehetséges, mert az Azure rendszer útvonalak toodefine hogyan IP a forgalom sorát használja. Rendszerútvonalak vezérlő a következő forgatókönyvek hello kommunikáció hello áramlását:

* Belül hello ugyanazon az alhálózaton.
* Az alhálózati tooanother, egy Vneten belül.
* A virtuális gépek toohello Internet.
* Az egy VNet tooanother VNet egy VPN-átjárón keresztül.
* A virtuális hálózat tooanother VNet Vnetben társviszony-létesítés (szolgáltatás láncolás) keresztül.
* Virtuális hálózat tooyour a helyi hálózaton keresztül egy VPN-átjárón keresztül.

hello az alábbi ábra egy egyszerű beállítás egy Vnetet két alhálózattal és néhány virtuális géppel, valamint hello rendszerútvonalak, amelyek lehetővé teszik az IP-forgalom tooflow.

![Rendszerútvonalak az Azure-ban](./media/virtual-networks-udr-overview/Figure1.png)

Bár a rendszerútvonalak hello használata elősegíti a forgalom automatikusan az üzembe helyezéshez, vannak esetek használni kívánt toocontrol hello a csomagok útválasztását egy virtuális készüléken keresztül. Úgy, hogy felhasználó által megadott útvonalak létrehozásával megadhatja hello következő ugrás tooa alhálózatot toogo tooyour virtuális készülék helyette áramló csomagok számára, és a továbbítási hello IP-engedélyezése a virtuális gép futtatásához használt hello virtuális készülék.

hello az alábbi ábra szemlélteti, felhasználó által definiált útvonalak és IP-továbbítás tooforce csomagok tooone alhálózati küldött egy másik toogo a harmadik alhálózat egy virtuális készüléken keresztül.

![Rendszerútvonalak az Azure-ban](./media/virtual-networks-udr-overview/Figure2.png)

> [!IMPORTANT]
> Felhasználó által definiált útvonalak alkalmazott tootraffic alhálózatot elhagyó bármely erőforrás (például a hálózati adapterek csatolt tooVMs) hello alhálózatban. Nem hozhat létre útvonalakat toospecify hogyan forgalom ad meg egy alhálózatot a hello Internet, például. hello készülék, forgalom toocannot hello lennie ugyanazon az alhálózaton Ha hello forgalom származik. Mindig hozzon létre egy külön alhálózatot a készülékeinek. 
> 
> 

## <a name="route-resource"></a>Útvonal-erőforrás
Történik a csomagok irányítása alapján egy útválasztási táblázatot hello fizikai hálózat minden csomópontján definiált TCP/IP-hálózaton keresztül. Egy útválasztási táblázatot a külön útvonalak gyűjteménye használt toodecide ahol tooforward csomagok alapján hello cél IP-címet. Egy olyan útvonalat hello következő tevődik össze:

| Tulajdonság | Leírás | Korlátozások | Megfontolások |
| --- | --- | --- | --- |
| Címelőtag |hello cél CIDR toowhich hello vonatkozik, például 10.1.0.0/16. |Hello címek képviselő érvényes CIDR tartománynak kell lennie nyilvános interneten, Azure-beli virtuális hálózat vagy a helyszíni adatközpontban. |Győződjön meg arról, hogy hello **címelőtag** nem tartalmaz hello hello címet **következő ugrási cím**, ellenkező esetben a csomagok bekerülnek egy hurokba, anélkül, hogy valaha is elérnék hello forrás toohello következő ugrás át hello cél. |
| A következő ugrás típusa |hello típusú Azure Ugrás hello csomagot kell küldeni. |Hello a következő értékek egyike lehet: <br/> **Virtuális hálózat**. Hello helyi virtuális hálózatot jelenti. Például, ha két alhálózat, 10.1.0.0/16 és 10.2.0.0/16 található ugyanabban a virtuális hálózatban hello, hello útvonal minden hello útválasztási táblázatot az alhálózathoz kell egy következő ugrás értéke *virtuális hálózati*. <br/> **Virtuális hálózati átjáró**. Egy Azure S2S VPN Gateway átjárót jelöl. <br/> **Internet**. Hello hello Azure infrastruktúrája által biztosított alapértelmezett internetes átjárót jelöli. <br/> **Virtuális készülék**. Azure-beli virtuális hálózat tooyour hozzáadott virtuális készüléket jelöli. <br/> **Nincs**. Egy fekete lyukat jelöl. Tooa fekete lyuk továbbított csomagok egyáltalán nem lesznek továbbítva. |Érdemes lehet **virtuális készülék** toodirect forgalom tooa virtuális gép vagy az Azure Load Balancer belső IP-címet.  Ez a típus lehetővé teszi, hogy egy IP-címet az alább ismertetett hello megadását. Érdemes lehet egy **nincs** írja be a megadott cél szereplő tooa toostop-csomagokat. |
| A következő ugrás címe |hello következő ugrási cím csomagokat továbbítani kell hello IP-címet tartalmazza. Következő ugrás értékei csak engedélyezettek hol található a következő ugrás típusa hello útvonalak *virtuális készülék*. |Virtuális hálózat, amikor a felhasználó által definiált útvonal hello érvényben, áthaladás nélkül hello belül elérhető IP-címnek kell lennie egy **virtuális hálózati átjáró**. hello IP-cím toobe van, ugyanazon virtuális hálózat az alkalmazása hello, vagy peered virtuális hálózaton. |Ha hello IP-cím egy virtuális Gépet jelöl, ellenőrizze, hogy engedélyezi [IP-továbbítás](#IP-forwarding) hello virtuális gép az Azure-ban. Ha hello IP cím jelöli hello belső IP-cím az Azure terheléselosztó, ellenőrizze, hogy a megfelelő terheléselosztási szabály minden port tooload egyenleg kívánja.|

Az Azure PowerShell hello "NextHopType" értékek némelyike neve nem lehet ugyanaz:

* A virtuális hálózat a VnetLocal
* A virtuális hálózati átjáró a VirtualNetworkGateway
* A virtuális készülék a VirtualAppliance
* Az internet az Internet
* A nincs a Nincs

### <a name="system-routes"></a>Rendszerútvonalak
A virtuális hálózatban létrehozott összes alhálózatot automatikusan hozzárendeli egy útválasztási táblázatot, amely tartalmazza a következő útvonal rendszerszabályok hello:

* **Helyi Vnet szabály**: Ez a szabály a virtuális hálózat miden alhálózatában automatikusan létrejön. Meghatározza, hogy hello hello virtuális hálózat virtuális gépek között közvetlen kapcsolat van, és nincs köztes Ugrás.
* **Helyszíni szabály**: Ez a szabály vonatkozik tooall adatforgalmat toohello helyszíni címtartomány és VPN-átjárót használ hello következő ugrási célként.
* **Internet szabály**: Ez a szabály kezeli az összes adatforgalmat toohello nyilvános Internet (cím előtag 0.0.0.0/0) és a használt hello infrastruktúra internetes átjáróját hello, a következő az összes adatforgalmat toohello Internet az Ugrás.

### <a name="user-defined-routes"></a>Felhasználó által megadott útvonalak
A legtöbb környezetben csak akkor hello Azure által meghatározott rendszerútvonalakra. Azonban előfordulhat, hogy toocreate egy útválasztási táblázatot kell, és adjon hozzá egy vagy több útvonal például az adott esetben:

* A kényszerített bújtatás toohello Internet a helyi hálózaton keresztül.
* Virtuális készülékek használata Azure környezetben.

Hello a fenti helyzetekben toocreate rendelkezik egy útválasztási táblázatot, és adja hozzá a felhasználó által definiált útvonalak tooit. Több útválasztási táblázattal is rendelkezhet, és hello ugyanazt az útválasztási táblázatot lehet társított tooone vagy további alhálózatokat. És minden alhálózathoz csak társított tooa egy útválasztási táblázatot. Minden virtuális gépe és felhőszolgáltatása ugyanazt az alhálózat hello útvonal kapcsolódó tábla toothat alhálózati használata

Alhálózatok rendszerútvonalakra támaszkodnak, amíg egy útválasztási táblázatot társított toohello alhálózat. Miután egy hozzárendelés létrejött, az útválasztás a felhasználó által megadott útvonalaknál és a rendszerútvonalaknál is a leghosszabb előtag-megfeleltetés (LPM) alapján történik. Ha egynél több útvonal rendelkezik hello azonos LPM egyezik egy útvonal van kiválasztva, majd a sorrend hello Kiindulás alapján:

1. Felhasználó által megadott útvonal
2. BGP-útvonal (ExpressRoute használatánál)
3. Rendszerútvonal

toolearn hogyan toocreate felhasználó által megadott útvonalak, lásd: [hogyan tooCreate útvonalak és IP-továbbítás az Azure-ban engedélyezése](virtual-network-create-udr-arm-template.md).

> [!IMPORTANT]
> Felhasználó által megadott útvonalakat csak alkalmazott tooAzure virtuális gépek és a felhőalapú szolgáltatások. Például ha azt szeretné, hogy tooadd virtuális készülékként egy tűzfalat a helyszíni hálózat és az Azure között, akkor toocreate az Azure útválasztási táblázatban egy felhasználó által megadott útvonalat, amely továbbítja a teljes forgalmat toohello a helyi cím terület toohello virtuális készüléket. A felhasználó által definiált útvonal (UDR) toohello GatewaySubnet tooforward helyszíni tooAzure származó összes forgalmat hello virtuális készülék keresztül is hozzáadhat. Ez a lehetőség a közelmúltban lett hozzáadva.
> 
> 

### <a name="bgp-routes"></a>BGP-útvonalak
Ha a helyszíni hálózat és az Azure között ExpressRoute kapcsolat van, a helyszíni hálózati tooAzure a engedélyezheti toopropagate BGP-útvonalakat. Hello használatosak, a BGP-útvonalakat minden Azure alhálózatban ugyanúgy, mint a rendszerútvonalakat és a felhasználó által megadott útvonalak. További információ: [ExpressRoute Introduction](../expressroute/expressroute-introduction.md) (Az ExpressRoute bemutatása).

> [!IMPORTANT]
> Konfigurálhatja az Azure-alapú környezetben toouse kényszerített a helyszíni hálózaton hozzon létre egy felhasználó által megadott útvonalat a 0.0.0.0/0 alhálózat számára, amely a következő ugrás hello hello VPN-átjárót használ. Ez azonban csak akkor működik, ha VPN-átjárót használ, nem ExpressRoute-ot. Az ExpressRoute-nál a kényszerített bújtatás konfigurálása a BGP-n keresztül történik.
> 
> 

## <a name="ip-forwarding"></a>IP-továbbítás
A fent említett egyik fő oka annak toocreate hello egy felhasználó által megadott útvonalat tooforward forgalom tooa virtuális készülék. A virtuális készülék értéke "Nothing" több mint egy virtuális Gépet, amelyen az alkalmazás toohandle hálózati forgalom valamilyen módon, például egy tűzfal vagy NAT-eszköz.

A virtuális gép kell lennie, amely képes tooreceive bejövő forgalom virtuális készüléknek tooitself nem foglalkozik. a virtuális gépek tooreceive forgalma tooallow címzett tooother a célok, engedélyeznie kell az IP-továbbítás az hello VM. Ez az egy Azure beállítás, nem a megfelelő értéket hello vendég operációs rendszer.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hozhat létre útvonalakat hello Resource Manager üzembe helyezési modellel](virtual-network-create-udr-arm-template.md) és toosubnets rendelje hozzá őket. 
* Ismerje meg, hogyan túl[hozhat létre útvonalakat a klasszikus üzembe helyezési modellel hello](virtual-network-create-udr-classic-ps.md) és toosubnets rendelje hozzá őket.

