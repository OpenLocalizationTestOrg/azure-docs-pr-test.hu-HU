---
title: "A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hivatkozás: Azure-portál |} Microsoft Docs"
description: "Ez a dokumentum áttekintést hogyan toolink virtuális hálózatok (Vnetek) tooExpressRoute kapcsolatok."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8bedcb11df7e30281fd439afdfb76cc67626a8f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a>Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-linkvnet-classic.md)
> 

Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) tooAzure ExpressRoute-Kapcsolatcsoportok hivatkozás hello Resource Manager telepítési modell segítségével, és hello Azure-portálon. Virtuális hálózatok lehet a hello ugyanazt az előfizetést, vagy egy másik előfizetés része lehet.

## <a name="before-you-begin"></a>Előkészületek
* Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
* Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.
  
  * Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) és hello áramkör szerint a kapcsolat szolgáltatójánál engedélyezve van.
  * Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva. Lásd: hello [útválasztás konfigurálása](expressroute-howto-routing-portal-resource-manager.md) útválasztási miként.
  * Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés van konfigurálva, és hello BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.
  * Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve. Útmutatás alapján hello túl[hozzon létre egy virtuális hálózati átjáró ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Az ExpressRoute a virtuális hálózati átjáró nem VPN GatewayType "ExpressRoute" hello használja.

* Too10 virtuális hálózatok tooa szabványos ExpressRoute-kapcsolatcsoportot is csatolhatja. Az összes virtuális hálózatot kell hello geopolitikai régión szabványos ExpressRoute-kapcsolatcsoportot használatakor. 
* Egy virtuális hálózaton kívül hello geopolitikai területet hello ExpressRoute-kapcsolatcsoportot hivatkozásra, vagy csatlakoztassa a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú, ha engedélyezte a hello ExpressRoute prémium szintű bővítmény. Ellenőrizze a hello [– gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény olvashat.
* Is [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) előtt kezdete toobetter megismerheti hello lépéseit.

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>A virtuális hálózat a hello azonos előfizetés tooa áramkör

### <a name="toocreate-a-connection"></a>a kapcsolat toocreate

> [!NOTE]
> BGP-konfigurációs információk nem jelennek meg ha hello 3 rétegbeli konfigurálva, a esetében. Ha a kapcsolatcsoport kiépített állapotban van, meg kell tudni toocreate kapcsolatok.
>

1. Győződjön meg arról, hogy az ExpressRoute-kapcsolatcsoportot és Azure magánhálózati társviszony-létesítés rendelkezik konfigurálása sikeresen megtörtént. Hello utasításait követve [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és [útválasztás konfigurálása](expressroute-howto-routing-arm.md). Az ExpressRoute-kapcsolatcsoportot kép a következő hello hasonlóan kell kinéznie:

    ![Az ExpressRoute-kapcsolatcsoport képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. Most elindíthatja a kapcsolat toolink kiépítése a virtuális hálózati átjáró tooyour ExpressRoute-kapcsolatcsoportot. Kattintson a **kapcsolat** > **Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen, majd konfigurálja a hello értékeket.

    ![Vegye fel a kapcsolatot képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. Után a kapcsolat sikeresen konfigurálva, akkor a kapcsolati objektum hello kapcsolat hello információk jelennek meg.

     ![Kapcsolat objektum képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="toodelete-a-connection"></a>a kapcsolat toodelete
A kapcsolat hello kiválasztásával törölheti **törlése** hello panelen a kapcsolat ikon.

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot
Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani. hello az alábbi ábra egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések.
- Hello szervezeten belül hello osztályok mindegyikének saját előfizetés használhatja a szolgáltatások telepítéséhez, de egyetlen ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózathoz is megoszthatják.
- Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot. Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.

    > [!NOTE]
    > Kapcsolat és a sávszélesség díjak dedikált hello kör lesz alkalmazott toohello ExpressRoute-kapcsolatcsoport tulajdonosát. Az összes virtuális hálózatot megosztása hello azonos sávszélesség.
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a>Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók

hello "kapcsolatcsoport tulajdonosát" hello ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága. hello kapcsolatcsoport tulajdonosát hozhat létre, amely a "kör felhasználók" is váltható engedélyek. Kör felhasználók tulajdonosai virtuális hálózati átjárók, amelyek nincsenek belül hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello. Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).

hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik. Az összes hivatkozás kapcsolatok törlődnek, amelyek hozzáférését visszavonták hello előfizetésből engedélyezési eredményez visszavonása.

### <a name="circuit-owner-operations"></a>Kapcsolatcsoport-tulajdonos műveletek

**egy kapcsolat hitelesítési toocreate**

hello kapcsolatcsoport tulajdonosát engedélyezési hoz létre. Az eredmény lehet engedélykulcs hello létrehozását a virtuális hálózati átjárók toohello ExpressRoute-kapcsolatcsoportot egy kör felhasználói tooconnect használják. Az engedély csak egy kapcsolat érvénytelen.

1. Hello ExpressRoute paneljén kattintson **engedélyek** és írja be a **neve** hello engedélyezési, majd kattintson **mentése**.

    ![Engedélyek](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. Ha mentett hello konfigurációs, másolja a hello **erőforrás-azonosító** és hello **Engedélykulcs**.

    ![Hitelesítési kulcs](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**egy kapcsolat hitelesítési toodelete**

A kapcsolat hello kiválasztásával törölheti **törlése** hello panelen a kapcsolat ikon.

### <a name="circuit-user-operations"></a>Kör felhasználói műveletek

hello áramkör felhasználói hello erőforrás-azonosító és engedélykulcs hello kapcsolatcsoport tulajdonosát a kell. 

**egy kapcsolat hitelesítési tooredeem**

1.  Kattintson a hello **+ új** gombra.

    ![Új kattintson](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  Keresse meg **"Kapcsolat"** hello piactér, válassza ki azt, majd kattintson **létrehozása**.

    ![Keresse meg a kapcsolat](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  Győződjön meg arról, hogy hello **kapcsolattípus** túl értéke "ExpressRoute".


4.  Töltse ki hello részleteit, majd kattintson az **OK** hello alapvető beállítások panelen.

    ![Alapvető beállítások panel](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  A hello **beállítások** panelen, jelölje be hello **virtuális hálózati átjáró** , és ellenőrizze a hello **engedélyezési beváltani** jelölőnégyzetet.

6.  Adja meg a hello **engedélykulcs** és hello **áramkör URI partnert** és nevezze el hello kapcsolat. Kattintson az **OK** gombra.

    ![Beállítások panel](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. Tekintse át a hello hello információkat **összegzés** panel megnyitásához, és kattintson **OK**.


**egy kapcsolat hitelesítési toorelease**

Az engedély törölni kell, amely a hello ExpressRoute körön toohello virtuális hálózati kapcsolat hello is megjelenhetnek.

## <a name="next-steps"></a>Következő lépések
ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).
