---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate, kiépítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: 200418aed6bdebace43a2f57cf532d1c8d13cb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-circuit-classic.md)
>

Ez a cikk ismerteti, hogyan toocreate használatával Azure ExpressRoute-kapcsolatcsoportot hello Azure portál és hello Azure Resource Manager telepítési modell. az alábbi lépések is hello megjelenítése hogyan hello kapcsolatcsoport állapotának toocheck hello a frissítést, vagy törölje és kiosztásának megszüntetése azt.


## <a name="before-you-begin"></a>Előkészületek
* Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
* Győződjön meg arról, hogy rendelkezik-e hozzáférési toohello [Azure-portálon](https://portal.azure.com).
* Győződjön meg arról, hogy rendelkezik-e engedélyekkel toocreate új hálózati erőforrásokhoz. Ha nem rendelkezik hello megfelelő engedélyekkel, forduljon a fiók rendszergazdájához.
* Is [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) sorrendben kezdete elé toobetter megismerheti hello lépéseit.

## <a name="create-and-provision-an-expressroute-circuit"></a>Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot
### <a name="1-sign-in-toohello-azure-portal"></a>1. Jelentkezzen be toohello Azure-portálon
Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és jelentkezzen be az Azure-fiókjával.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Hozzon létre egy új ExpressRoute-kapcsolatcsoportot
> [!IMPORTANT]
> Az ExpressRoute-kapcsolatcsoportot jelenik meg a szolgáltatási kulcs hello pillanattól lesz terhelve. Győződjön meg arról, hogy készen áll a tooprovision hello áramkör hello kapcsolat szolgáltatójánál esetén elvégzi ezt a műveletet.
> 
> 

1. Létrehozhat egy ExpressRoute-kapcsolatcsoportot hello beállítás toocreate kiválasztásával egy új erőforrást. Kattintson a **új** > **hálózati** > **ExpressRoute**, ahogy az a következő kép hello:
   
    ![ExpressRoute-kapcsolatcsoport létrehozása](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Miután rákattintott **ExpressRoute**, látni fogja, hello **létrehozása ExpressRoute-kapcsolatcsoportot** panelen. A panel hello értékek kitöltés alatt ellenőrizze, meg kell adnia hello megfelelő SKU réteg és az adatforgalom-mérést.
   
   * **Réteg** meghatározza, hogy egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e. Megadhat **szabványos** tooget hello standard Termékváltozat vagy **prémium** hello prémium bővítménnyel.
   * **Az adathasználat mérését** hello számlázási típusa határozza meg. Megadhat **Metered** díjköteles adatforgalom tervhez és **korlátlan** egy adatforgalmi a. Vegye figyelembe, hogy számlázási típusának hello módosíthatja **Metered** túl**korlátlan**, de nem módosíthatja a hello típusát **korlátlan** túl**Metered**.
     
     ![Hello SKU réteg és a mérési adatok konfigurálása](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> Felhívjuk a figyelmét arra, hogy hello társviszony-létesítés hely jelzi hello [helynév](expressroute-locations.md) hol vannak a Microsoft társviszony. Ez az **nem** kapcsolódó túl "Hely" tulajdonság, amely a toohello földrajzi hely, ahol hello Azure hálózati erőforrás-szolgáltató. Bár nem áll(nak) kapcsolatban, ez a beállítás egy célszerű toochoose a hálózati erőforrás-szolgáltató földrajzilag Bezárás toohello hello kör társviszony-létesítés helyét. 
> 
> 

### <a name="3-view-hello-circuits-and-properties"></a>3. Nézet hello Kapcsolatcsoportok és tulajdonságai
**Minden hello kapcsolatok megtekintése**

Megtekintheti az összes hello áramkör kiválasztásával létrehozott **összes erőforrás** hello bal oldali menüben.

![Kapcsolatok megtekintése](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Hello tulajdonságainak megtekintése**

    You can view hello properties of hello circuit by selecting it. On this blade, note hello service key for hello circuit. You must copy hello circuit key for your circuit and pass it down toohello service provider toocomplete hello provisioning process. hello circuit key is specific tooyour circuit.

![Tulajdonságok megtekintése](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>4. Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez
A panel **szolgáltató állapota** információt nyújt a hello aktuális állapotának kiépítés hello szolgáltatói oldalán. **Állapot áramkör** hello Microsoft ügyféloldali hello állapot biztosít. Kiépítés állapotok áramkör kapcsolatos további információkért lásd: hello [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.

Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello áramkör hello állapota a következő lehet:

Szolgáltató állapota: nincs telepítve<BR>
Áramkör állapot: engedélyezve

![Üzembe helyezési folyamat elindítása](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

hello áramkör toohello hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén a következő állapota változik:

Szolgáltató állapota: kiépítés<BR>
Áramkör állapot: engedélyezve

Az Ön toobe képes toouse ExpressRoute-kapcsolatcsoportot az állapot a következő hello kell lennie:

Szolgáltató állapota: kiépítése<BR>
Áramkör állapot: engedélyezve

### <a name="5-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>5. Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota
Megtekintheti, hogy az lehetőséget most érdekelt hello áramkör hello tulajdonságait. Ellenőrizze a hello **szolgáltató állapota** , és győződjön meg arról, hogy túl van-e áthelyezése**kiépítve** a folytatás előtt.

![Kör és szolgáltató állapota](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Az útválasztó-konfiguráció létrehozása
Lépésenkénti útmutatásért tekintse meg a toohello [ExpressRoute-áramkör útválasztási konfigurációja](expressroute-howto-routing-portal-resource-manager.md) toocreate a következő cikket, és módosítsa a kapcsolatcsoport esetében.

> [!IMPORTANT]
> Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak. A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.
> 
> 

### <a name="7-link-a-virtual-network-tooan-expressroute-circuit"></a>7. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot
A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot. Használjon hello [csatolása a virtuális hálózatok tooExpressRoute Kapcsolatcsoportok](expressroute-howto-linkvnet-arm.md) hello Resource Manager üzembe helyezési modellben való munka során a következő cikket.

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Egy ExpressRoute-kapcsolatcsoportot hello állapotának beolvasása
Expressroute-kapcsolatcsoporthoz hello állapotának megtekintéséhez jelölje ki. 

![Egy ExpressRoute-kapcsolatcsoport állapotát](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a>Egy ExpressRoute-kapcsolatcsoportot módosítása
ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.

Mindent hello állásidő nélkül a következő:

* Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.
* Az ExpressRoute-kapcsolatcsoportot növekedése hello sávszélesség megadott érhető el kapacitás hello porton. Vegye figyelembe, hogy a alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat nem támogatott. 
* Hello mérési adatok díjköteles tooUnlimited adatokat a terv módosítása Vegye figyelembe az adatok nem támogatott adatforgalmi tooMetered változó hello mérési terv.
* Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.

További információ a korlátai és korlátozásai, tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

toomodify ExpressRoute-kapcsolatcsoportot, kattintson a hello **konfigurációs** a hello az alábbi ábrán látható módon.

![Kör módosítása](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

Hello sávszélesség, SKU, számlázási modellt módosíthatja, és engedélyezi a klasszikus műveletek hello konfigurációs panel belül.

> [!IMPORTANT]
> Előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot esetén nincs elég kapacitás hello meglévő porton. Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.
>
> Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül. Alacsonyabb verziójúra változtatása sávszélesség toodeprovision hello ExpressRoute-kapcsolatcsoportot meg, és majd építenie az új ExpressRoute-kapcsolatcsoportot.
> 
> Prémium szintű bővítmény letiltása művelet sikertelen lesz, amely nagyobb, mint a megengedett hello szabványos áramkör erőforrások használata.
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése
Az ExpressRoute-kapcsolatcsoport törlése hello kiválasztásával **törlése** ikonra. Vegye figyelembe a következőket hello:

* Az ExpressRoute-kapcsolatcsoportot hello összes virtuális hálózatot kell választható. Ha ez a művelet sikertelen, ellenőrizze, hogy a virtuális hálózatok vannak-e kapcsolva toohello körön.
* Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon. Rendszer továbbra is tooreserve erőforrások és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesíti a számunkra.
* Ha hello szolgáltató rendelkezik platformelőfizetés hello áramkör (üzembe helyezési állapota hello szolgáltató értéke túl**nincs kiépítve**) hello áramkör törölhet. Ezzel leállítja a számlázási hello kör

## <a name="next-steps"></a>Következő lépések
Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy Ön hello a következő:

* [Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing](expressroute-howto-routing-portal-resource-manager.md)
* [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás](expressroute-howto-linkvnet-arm.md)

