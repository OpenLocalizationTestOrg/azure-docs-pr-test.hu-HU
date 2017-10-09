---
title: "Hogyan tooconfigure útválasztás (társviszony) az ExpressRoute-kapcsolatcsoportot: erőforrás-kezelő: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés hello saját, a nyilvános és a Microsoft társviszony-létesítést az ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, a frissítést, vagy a-kör társviszony törlése."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: a8ca25f488dde728cb3b06cd2c91873f3069feaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Létrehozásához és módosításához az ExpressRoute-kör társviszony

Ez a cikk segít hozhat létre és kezelhet útválasztási konfigurációja ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel hello Azure-portál használatával. Is hello állapota, update vagy delete ellenőrizze és kiosztásának megszüntetése ExpressRoute-kör társviszony. Ha egy másik módszer toowork toouse a kapcsolatcsoport rendelkező, jelölje be a cikk a következő lista hello:


## <a name="configuration-prerequisites"></a>Konfigurációs előfeltételek

* Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) lap hello [útválasztási követelmények](expressroute-routing.md) lap és hello [munkafolyamatok](expressroute-workflows.md) lapon konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége. Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) , és folytassa a kapcsolat szolgáltatójánál előtt által engedélyezett hello áramkör. hello ExpressRoute-kapcsolatcsoportot meg toobe képes toorun hello parancsmagok a következő szakaszokban hello kiépített és engedélyezett állapotban kell lennie.
* Ha azt tervezi, hogy a megosztott kulcs/MD5 kivonatoló toouse, lehet, hogy toouse ez hello alagút és a limit hello száma karakterek tooa legfeljebb 25 mindkét oldalán.

Ezek az utasítások csak a 2. rétegbeli kapcsolatot szolgáltatásokat nyújtó szolgáltatók létre toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában egy IPVPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálja és kezeli az Ön útválasztást. 

> [!IMPORTANT]
> A Microsoft jelenleg hirdetményt hello felügyeleti portálon keresztül szolgáltatók által konfigurált esetében. Dolgozunk azon, hogy hamarosan bevezethessük ezt a képességet. A BGP társviszony konfigurálása előtt ellenőrizze a szolgáltató.
> 
> 

Egy, két vagy akár mindhárom társviszony-létesítést (Azure privát, Azure nyilvános és Microsoft) is konfigurálhatja egy adott ExpressRoute-kapcsolatcsoportban. A társviszony-létesítéseket tetszőleges sorrendben konfigurálhatja. Azonban kell győződjön meg arról, hogy elvégezte-e minden társviszony-létesítési egyszerre csak egy hello konfigurációját.

## <a name="azure-private-peering"></a>Azure privát társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Azure privát társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot.

### <a name="toocreate-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toocreate

1. ExpressRoute-kapcsolatcsoportot hello konfigurálása. Bejelölheti, hello áramkör teljesen hello kapcsolat szolgáltató folytatása előtt.

  ![lista](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Az Azure magánhálózati társviszony-létesítés hello kör megadása Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:

  * / 30-as alhálózat hello elsődleges kapcsolathoz. hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. hello alhálózat nem virtuális hálózatok számára fenntartott címtartomány részének kell lennie.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat. Ehhez a társviszony-létesítéshez használhat privát AS-számokat is. Ne használja a 65515 számot.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.
3. Válassza ki a hello Azure magánhálózati társviszony-létesítési sorban, ahogy az alábbi példa hello:

  ![Magánfelhő](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Konfigurálja a privát társviszony-létesítést. a következő kép hello egy konfigurációs példát mutat be:

  ![Konfigurálja a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Hello konfiguráció mentéséhez, miután megadta az összes paramétert. Hello konfigurációja sikeresen elfogadva, miután a következő példa valami hasonló toohello jelenik meg:

  ![Mentse a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooview-azure-private-peering-details"></a>tooview Azure magánhálózati társviszony-létesítés részletei

Azure magánhálózati társviszony-létesítés hello társviszony-létesítés kiválasztásával hello tulajdonságait tekintheti meg.

![magánhálózati társviszony-létesítés megtekintése](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure magánhálózati társviszony-létesítési konfiguráció

Jelölje ki a társviszony-létesítéshez hello sort, és társviszony-létesítési hello tulajdonságainak módosítása.

![Frissítse a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="toodelete-azure-private-peering"></a>az Azure magánhálózati társviszony-létesítés toodelete

Eltávolíthatja a társviszony-létesítési konfiguráció kiválasztásával hello Törlés ikonra, ahogy az a következő kép hello:

![Törölje a magánhálózati társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Azure nyilvános társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és hello Azure nyilvános társviszony-létesítési konfiguráció ExpressRoute-kapcsolatcsoportot törli.

### <a name="toocreate-azure-public-peering"></a>az Azure nyilvános társviszony toocreate

1. Konfigurálja az ExpressRoute-kapcsolatcsoportot. Bejelölheti, hello áramkör teljesen hello kapcsolat szolgáltató további folytatása előtt.

  ![nyilvános társviszony felsorolása](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Az Azure nyilvános társviszony-létesítés hello kör megadása Győződjön meg arról, hogy rendelkezik-e hello hello lépések folytatása előtt a következő elemek:

  * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.
3. Válassza ki az Azure nyilvános társviszony-létesítési sor hello, ahogy az a következő kép hello:

  ![Válassza ki a nyilvános társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Konfigurálja a nyilvános társviszony-létesítést. a következő kép hello egy konfigurációs példát mutat be:

  ![Nyilvános társviszony konfigurálása](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Hello konfiguráció mentéséhez, miután megadta az összes paramétert. Hello konfigurációja sikeresen elfogadva, miután a következő példa valami hasonló toohello jelenik meg:

  ![Nyilvános társviszony-létesítési konfiguráció mentése](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooview-azure-public-peering-details"></a>tooview Azure nyilvános társviszony-létesítés részletei

Az Azure nyilvános társviszony-létesítés hello társviszony-létesítés kiválasztásával hello tulajdonságait tekintheti meg.

![nyilvános társviszony-létesítési tulajdonságainak megtekintése](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooupdate-azure-public-peering-configuration"></a>az Azure nyilvános társviszony-létesítési konfiguráció tooupdate

Jelölje ki a társviszony-létesítéshez hello sort, és társviszony-létesítési hello tulajdonságainak módosítása.

![Válassza ki a nyilvános társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="toodelete-azure-public-peering"></a>az Azure nyilvános társviszony toodelete

Eltávolíthatja a társviszony-létesítési konfiguráció hello Törlés ikonra, kiválasztásával, ahogy az alábbi példa hello:

![nyilvános társviszony törlése](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Microsoft társviszony-létesítés

Ez a szakasz segítséget nyújt a létrehozása, beolvasása, frissítése és törlése hello Microsoft társviszony-létesítési konfiguráció ExpressRoute-kör.

> [!IMPORTANT]
> Előzetes tooAugust 1 Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok volt beállítva, akkor 2017 fog rendelkezni az összes szolgáltatás előtagok keresztül hello Microsoft társviszony-létesítést, meghirdetett, akkor is, ha nincsenek megadva útvonal szűrők. A Microsoft társviszony-létesítést az ExpressRoute-Kapcsolatcsoportok vannak konfigurálva, vagy azt követően 2017. augusztus 1. nem rendelkezik a előtagokat meghirdetett, amíg egy útvonal-szűrő nem csatlakoztatja toohello körön. További információkért lásd: [konfigurálása a Microsoft társviszony-létesítéshez útvonal szűrő](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft társviszony-létesítés

1. Konfigurálja az ExpressRoute-kapcsolatcsoportot. Bejelölheti, hello áramkör teljesen hello kapcsolat szolgáltató további folytatása előtt.

  ![a Microsoft társviszony-létesítés felsorolása](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurálja a Microsoft hello-kör társviszony-létesítés. Győződjön meg arról, hogy rendelkezik-e folytatni a következő információk előtt hello.

  * / 30-as alhálózat hello elsődleges kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
  * / 30-as alhálózat hello másodlagos kapcsolathoz. Ennek egy érvényes nyilvános IPv4-előtagnak kell lennie, amely az Ön tulajdonában van, és regisztrálva van egy RIR-/IRR-jegyzékben.
  * Egy érvényes VLAN-azonosító tooestablish a társviszony. Győződjön meg arról, hogy egyetlen másik társviszonya se a hello áramkör használ hello azonos VLAN-azonosítót.
  * Egy AS-szám a társviszony-létesítéshez. 2 és 4 bájtos AS-számokat is használhat.
  * Meghirdetett előtagok: meg kell adnia a lista az összes előtagok hello BGP munkameneten keresztül tervezi tooadvertise. A rendszer kizárólag a nyilvános IP-cím-előtagokat fogadja el. Ha azt tervezi, toosend előtagok készlete, elküldheti a vesszővel tagolt listáját. Ezeket az előtagokat kell lenniük egy RIR-ben regisztrált tooyou vagy IRR-ben.
  * **Választható -** ügyfél ASN: Ha hirdetési-előtagok nem regisztrált toohello társviszony-létesítés SZÁMOT, megadhatja hello számú toowhich regisztrálva vannak.
  * Útválasztási beállításjegyzék-név: Hello RIR megadhat / BMR mely hello ellen, számát, és előtagok regisztrálva van.
  * **Választható -** az MD5 kivonatoló, ha úgy dönt, hogy egy toouse.
3. Kiválaszthatja a hello társviszony-létesítés kívánja tooconfigure, ahogy az alábbi hello példa. Válassza ki a Microsoft társviszony-létesítési sor hello.

  ![Válassza ki a Microsoft társviszony-létesítési sor](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Konfigurálja a Microsoft társviszony-beállítást. a következő kép hello egy konfigurációs példát mutat be:

  ![Konfigurálja a Microsoft társviszony-létesítés](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Hello konfiguráció mentéséhez, miután megadta az összes paramétert.

  Ha a kapcsolatcsoport lekérdezi tooa "Validation szükséges" állapotba kerül (hello ábrának megfelelően), meg kell nyitnia egy támogatási jegy tooshow igazoló hello előtagok tooour támogatási csoport tulajdonjogának.

  ![A Microsoft társviszony-létesítési konfiguráció mentése](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Támogatási jegy megnyithatja közvetlenül portálról hello, ahogy az alábbi példa hello:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Miután hello konfigurációja sikeresen elfogadva, valami hasonló toohello kép a következő jelenik meg:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="tooview-microsoft-peering-details"></a>tooview Microsoft társviszony-létesítési részletei

Az Azure nyilvános társviszony-létesítés hello társviszony-létesítés kiválasztásával hello tulajdonságait tekintheti meg.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="tooupdate-microsoft-peering-configuration"></a>a Microsoft társviszony-létesítési konfiguráció tooupdate

Jelölje ki a társviszony-létesítéshez hello sort, és társviszony-létesítési hello tulajdonságainak módosítása.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft társviszony-létesítés

Eltávolíthatja a társviszony-létesítési konfiguráció kiválasztásával hello Törlés ikonra, ahogy az a következő kép hello:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Következő lépések

Következő lépés, [csatolása a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-portal-resource-manager.md)
* Az ExpressRoute-munkafolyamatokkal kapcsolatos további információkért lásd: [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-munkafolyamatok).
* A kapcsolatcsoportok társviszony-létesítéseivel kapcsolatos további információkért lásd: [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-kapcsolatcsoportok és útválasztási tartományok).
* További információkért a virtuális hálózatok használatáról lásd: [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtuális hálózatok áttekintése).
