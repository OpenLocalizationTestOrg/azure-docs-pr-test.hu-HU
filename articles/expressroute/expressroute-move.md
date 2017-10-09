---
title: "a klasszikus tooResource Manager áramkörök aaaMoving ExpressRoute |} Microsoft Docs"
description: "Ezen a lapon egy áttekintést nyújt az alábbiakra lesz szüksége tooknow adatközponthíd-képzés klasszikus hello és hello Resource Manager üzembe helyezési modellel kapcsolatos."
documentationcenter: na
services: expressroute
author: ganesr
manager: carmonm
editor: 
ms.assetid: bdf01217-1a98-4ec0-a08e-d84fd37f78af
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: ganesr
ms.openlocfilehash: c901d2cda01aec409b528d29fc937ac6afaea511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="moving-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model"></a>ExpressRoute-Kapcsolatcsoportok áthelyezése hello klasszikus toohello Resource Manager telepítési modell
Ez a cikk áttekintést jelentés toomove hello klasszikus toohello Azure Resource Manager üzembe helyezési modellben az Azure ExpressRoute-kapcsolatcsoportot.

Egy egyetlen ExpressRoute körön tooconnect toovirtual hálózatok telepített klasszikus hello és hello Resource Manager üzembe helyezési modellel egyaránt használható. ExpressRoute-kapcsolatcsoportot, függetlenül attól, hogy létrehozták, most már toovirtual hálózat összekapcsolása között két üzembe helyezési modell.

![Amely a toovirtual hálózatok két üzembe helyezési modell között ExpressRoute-kapcsolatcsoportot](./media/expressroute-move/expressroute-move-1.png)

## <a name="expressroute-circuits-that-are-created-in-hello-classic-deployment-model"></a>Hello klasszikus üzembe helyezési modellel létrehozott ExpressRoute-Kapcsolatcsoportok
Hello klasszikus üzembe helyezési modellel létrehozott ExpressRoute-Kapcsolatcsoportok kell áthelyezni toobe toohello erőforrás-kezelő központi telepítési modell első tooenable kapcsolat tooboth hello klasszikus és hello Resource Manager üzembe helyezési modellek. A kapcsolatok áthelyezésekor a kapcsolat nem veszik el, illetve nem szakad meg. Minden kapcsolat – virtuális hálózati kapcsolat hello klasszikus üzembe helyezési modellben (belül azonos hello előfizetés és az előfizetések közötti) megmaradnak.

Hello áthelyezése után sikeresen befejeződött, hello ExpressRoute-kapcsolatcsoportot keres, hajt végre, és pontosan érzi hello Resource Manager üzembe helyezési modellel létrehozott ExpressRoute-kapcsolatcsoportot. Mostantól létrehozhat kapcsolatok toovirtual hálózatok hello Resource Manager üzembe helyezési modellben.

ExpressRoute-kapcsolatcsoportot áthelyezett toohello Resource Manager üzembe helyezési modellben után kezelheti hello életciklusának hello ExpressRoute-kapcsolatcsoportot csak hello Resource Manager telepítési modell használatával. Ez azt jelenti, hogy műveleteket végezhet például hozzáadása/frissítése/törlése esetében (például a sávszélesség, SKU és számlázási típusa) kapcsolat tulajdonságainak frissítése, és csak a hello Resource Manager üzembe helyezési modellben a kapcsolatok törlése. Tekintse meg a toohello című szakaszt a Kapcsolatcsoportok hello Resource Manager üzembe helyezési modellel további tájékoztatást talál arról, hogyan kezelheti a hozzáférést tooboth üzembe helyezési modellel létrehozott.

A kapcsolat szolgáltató tooperform hello áthelyezése tooinvolve nem rendelkeznek.

## <a name="expressroute-circuits-that-are-created-in-hello-resource-manager-deployment-model"></a>Hello Resource Manager üzembe helyezési modellel létrehozott ExpressRoute-Kapcsolatcsoportok
A létrehozott hello Resource Manager deployment model toobe érhető el a két üzembe helyezési modell ExpressRoute-Kapcsolatcsoportok engedélyezheti. Az előfizetés bármely ExpressRoute-kapcsolatcsoportot lehet engedélyezett toobe két üzembe helyezési modell érhető el.

* Hello Resource Manager üzembe helyezési modellel létrehozott ExpressRoute-Kapcsolatcsoportok alapértelmezés szerint nem rendelkezik hozzáféréssel toohello klasszikus üzembe helyezési modellben.
* Alapértelmezés szerint két üzembe helyezési modell hello klasszikus telepítési modell toohello Resource manager üzembe helyezési modellben áthelyezett ExpressRoute-Kapcsolatcsoportok elérhetőek.
* ExpressRoute-kapcsolatcsoportot mindig hozzáférés toohello Resource Manager telepítési modell, függetlenül attól, hogy készült hello erőforrás-kezelő vagy a klasszikus üzembe helyezési modellel rendelkezik. A Ez azt jelenti, hogy létrehozhat toovirtual hálózatok létre kapcsolatokat hello az utasításokat követve által Resource Manager üzembe helyezési modellben [hogyan toolink virtuális hálózatok](expressroute-howto-linkvnet-arm.md).
* Hozzáférés toohello klasszikus üzembe helyezési modellel hello vezérli **allowClassicOperations** hello ExpressRoute-kapcsolatcsoportot paramétere.

> [!IMPORTANT]
> Hello ismertetett összes kvóták [szolgáltatási korlátait](../azure-subscription-service-limits.md) lap alkalmazni. Tegyük fel szabványos expressroute-kapcsolatcsoporthoz mind a klasszikus hello, mind a hello Resource Manager üzembe helyezési modellel rendelkezhet, legfeljebb 10 virtuális hálózati hivatkozások/kapcsolatok.
> 
> 

## <a name="controlling-access-toohello-classic-deployment-model"></a>Ellenőrző hozzáférési toohello klasszikus telepítési modell
Engedélyezheti a egyetlen ExpressRoute körön toolink toovirtual hálózatok két üzembe helyezési modell hello beállítása **allowClassicOperations** hello ExpressRoute-kapcsolatcsoportot paramétere.

Beállítás **allowClassicOperations** tooTRUE lehetővé teszi a virtuális hálózatokat toolink mindkét központi telepítési modellek toohello ExpressRoute-kapcsolatcsoportot. Kapcsolhat toovirtual hálózatok hello klasszikus telepítési modell a következő útmutatást a [hogyan toolink lévő virtuális hálózatok hello klasszikus üzembe helyezési modellel](expressroute-howto-linkvnet-classic.md). Kapcsolhat toovirtual hálózatok hello Resource Manager üzembe helyezési modellel következő útmutatást a [hogyan toolink lévő virtuális hálózatok hello Resource Manager üzembe helyezési modellben](expressroute-howto-linkvnet-arm.md).

Beállítás **allowClassicOperations** tooFALSE blokkolja toohello áramkör hozzáférést hello klasszikus telepítési modellből. Minden virtuális hálózati kapcsolat hello klasszikus üzembe helyezési modellben azonban megmaradnak. Ebben az esetben nincs látható hello klasszikus üzembe helyezési modellel hello ExpressRoute-kapcsolatcsoportot.

## <a name="supported-operations-in-hello-classic-deployment-model"></a>Támogatott műveletek hello klasszikus üzembe helyezési modellel
a következő klasszikus műveletek hello támogatottak az ExpressRoute áramkör mikor **allowClassicOperations** tooTRUE beállítása:

* Az ExpressRoute-kapcsolatcsoport információinak lekérése
* Virtuális hálózat létrehozása/frissítése/get/törlés tooclassic virtuális hálózatok hivatkozásokat tartalmaz.
* Virtuális hálózati kapcsolatok hitelesítéseinek létrehozása/frissítése/lekérése/törlése előfizetések közötti kapcsolatokhoz

Nem hajtható végre a következő klasszikus műveletek hello amikor **allowClassicOperations** tooTRUE beállítása:

* Border Gateway Protocol- (BGP-) társviszonyok létrehozása/frissítése/lekérése/törlése Azure privát, Azure nyilvános és Microsoft társviszony-létesítéshez
* ExpressRoute-kapcsolatcsoportok törlése

## <a name="communication-between-hello-classic-and-hello-resource-manager-deployment-models"></a>Klasszikus hello és hello Resource Manager üzembe helyezési modellek közötti kommunikáció
hello ExpressRoute-kapcsolatcsoportot egyfajta hidat klasszikus hello és hello Resource Manager üzembe helyezési modellek között. Ha mindkét virtuális hálózat csatolt toohello migrálhatók hello klasszikus üzembe helyezési modellel virtuális hálózatok és hello erőforrás-kezelő telepítési modell adatfolyamok ExpressRoute keresztül a virtuális hálózatok közötti forgalom azonos ExpressRoute-kapcsolatcsoportot.

Összesített hello átviteli kapacitásának hello virtuális hálózati átjáró korlátozza. Forgalom nem lép hello kapcsolat szolgáltatójánál hálózatok és a hálózati ebben az esetben. Hello virtuális hálózatok közötti adatforgalom teljes hello Microsoft hálózati tartalmazza.

## <a name="access-tooazure-public-and-microsoft-peering-resources"></a>Hozzáférés tooAzure nyilvános és a Microsoft társviszony-létesítési erőforrások
Folytatás tooaccess erőforrásokat, amelyek általában elérhető az Azure nyilvános társviszony-létesítést, és a Microsoft társviszony-létesítés bármely megszakítása nélkül.  

## <a name="whats-supported"></a>Támogatott műveletek
Ez a szakasz az ExpressRoute-kapcsolatcsoportok esetében támogatott műveleteket ismerteti:

* Egy egyetlen ExpressRoute körön tooaccess virtuális hálózatok vannak telepítve, a klasszikus hello és hello Resource Manager üzembe helyezési modellel is használhatja.
* Hello klasszikus toohello Resource Manager üzembe helyezési modellben az ExpressRoute-kapcsolatcsoportot helyezhetők. Áthelyezés, miután a hello ExpressRoute-kapcsolatcsoportot keres, érzi, és mint bármely más ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel létrehozott.
* Áthelyezheti csak hello ExpressRoute-kapcsolatcsoportot. A kapcsolatcsoportok kapcsolatai, a virtuális hálózatok és a VPN-átjárók nem helyezhetők át ezzel a művelettel.
* ExpressRoute-kapcsolatcsoportot áthelyezett toohello Resource Manager üzembe helyezési modellben után kezelheti hello életciklusának hello ExpressRoute-kapcsolatcsoportot csak hello Resource Manager telepítési modell használatával. Ez azt jelenti, hogy műveleteket végezhet például hozzáadása/frissítése/törlése esetében (például a sávszélesség, SKU és számlázási típusa) kapcsolat tulajdonságainak frissítése, és csak a hello Resource Manager üzembe helyezési modellben a kapcsolatok törlése.
* hello ExpressRoute-kapcsolatcsoportot egyfajta hidat klasszikus hello és hello Resource Manager üzembe helyezési modellek között. Ha mindkét virtuális hálózat csatolt toohello migrálhatók hello klasszikus üzembe helyezési modellel virtuális hálózatok és hello erőforrás-kezelő telepítési modell adatfolyamok ExpressRoute keresztül a virtuális hálózatok közötti forgalom azonos ExpressRoute-kapcsolatcsoportot.
* Klasszikus hello és hello Resource Manager üzembe helyezési modellel is támogatott előfizetések közötti kapcsolathoz.
* Miután ExpressRoute-kapcsolatcsoportot áthelyezése hello Klasszikus modell toohello Azure Resource Manager modellt, [hello virtuális hálózatok csatolt toohello ExpressRoute-kapcsolatcsoportot áttelepítése](expressroute-migration-classic-resource-manager.md).

## <a name="whats-not-supported"></a>Nem támogatott műveletek
Ez a szakasz az ExpressRoute-kapcsolatcsoportok esetében nem támogatott műveleteket ismerteti:

* ExpressRoute-kapcsolatcsoportot hello életciklusának kezelésére hello klasszikus telepítési modellből.
* Szerepköralapú hozzáférés-vezérlés (RBAC) támogatása hello klasszikus üzembe helyezési modellben. Nem hajtható végre RBAC vezérlők tooa áramkör hello klasszikus üzembe helyezési modellben. Minden rendszergazda/coadministrator hello előfizetés csatolása vagy leválasztása a virtuális hálózatok toohello kör is.

## <a name="configuration"></a>Konfiguráció
Útmutatás alapján hello ismertetett [ExpressRoute-kapcsolatcsoportot áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben](expressroute-howto-move-arm.md).

## <a name="next-steps"></a>Következő lépések
* [Hello virtuális hálózatok csatolt toohello ExpressRoute-kapcsolatcsoportot át hello Klasszikus modell toohello Azure Resource Manager modellt](expressroute-migration-classic-resource-manager.md)
* További információkért lásd: [ExpressRoute-kapcsolatcsoportok kiépítési munkafolyamatai és kapcsolatcsoport-állapotok](expressroute-workflows.md).
* tooconfigure az ExpressRoute-kapcsolatot:
  
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-arm.md)
  * [Útválasztás konfigurálása](expressroute-howto-routing-arm.md)
  * [Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md)

