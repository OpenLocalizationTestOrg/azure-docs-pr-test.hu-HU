---
title: "az ExpressRoute-kapcsolatcsoportot konfigurálása aaaWorkflows |} Microsoft Docs"
description: "Ezen a lapon bemutatja, hogyan hello munkafolyamatainak ExpressRoute-kapcsolatcsoportot és társviszony konfigurálása"
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8e1dfc137401e0d6d53608ae6c8de0085e182eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Az ExpressRoute kapcsolatcsoport-kiépítési munkafolyamatai és a kapcsolatcsoportok állapotai
Ezen a lapon bemutatja, hogyan hello szolgáltatás üzembe helyezési és útválasztási konfigurációs munkafolyamatok magas szinten.

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

hello alábbi ábra és a hozzá tartozó lépések hello feladatok megjelenítése követni kell rendelés toohave egy ExpressRoute körön kiosztott-végpontok. 

1. Használjon PowerShell tooconfigure ExpressRoute-kapcsolatcsoportot. Hello hello utasításait követve [létrehozása ExpressRoute-Kapcsolatcsoportok](expressroute-howto-circuit-classic.md) cikkben olvashat.
2. Rendelés kapcsolat hello-szolgáltatótól. Ez a folyamat függően változik. A kapcsolat szolgáltatójánál kapcsolatos további részletekért forduljon tooorder kapcsolat.
3. Győződjön meg arról, hogy hello áramkör van kiépítve sikeresen üzembe helyezési állapota a Powershellen keresztül hello ExpressRoute-kapcsolatcsoportot ellenőrzésével. 
4. Útválasztási tartományok konfigurálása. Ha a kapcsolat szolgáltatójánál, 3. rétegbeli kezeli, a vállalat konfigurálja a kapcsolatcsoport útválasztást. Ha a kapcsolat szolgáltatójánál csak a 2. rétegbeli szolgáltatásokat biztosít, konfigurálnia kell egy hello leírt irányelveket útválasztási [útválasztási követelmények](expressroute-routing.md) és [útválasztási konfigurációja](expressroute-howto-routing-classic.md) lapokat.
   
   * Azure magánhálózati társviszony-létesítés engedélyezése – kell engedélyezni a társviszony-létesítési tooconnect tooVMs / felhőszolgáltatások telepített virtuális hálózatokon belül.
   * Engedélyezze az Azure nyilvános társviszony - engedélyeznie kell az Azure nyilvános társviszony Ha tooconnect tooAzure alkalmazáskönyvtár nyilvános IP-címeket. Ez az a követelmény tooaccess Azure-erőforrások, ha a kiválasztott tooenable alapértelmezett útválasztást Azure magánhálózati társviszony-létesítés.
   * Engedélyezze a Microsoft társviszony - engedélyeznie kell az Office 365 és Dynamics 365 tooaccess. 
     
     > [!IMPORTANT]
     > Győződjön meg arról, hogy egy külön proxy használata / biztonsági tooconnect tooMicrosoft hello egyikét használja, mint hello Internet. Az ExpressRoute- és hello Internet ugyanazt biztonsági fog okozhat, aszimmetrikus Útválasztás és a kapcsolat üzemszünet korlátozza a hálózat hello segítségével.
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. TooExpressRoute Kapcsolatcsoportok csatolása a virtuális hálózatok, mert a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot társíthatja. Kövesse az utasításokat [toolink Vnetek](expressroute-howto-linkvnet-arm.md) tooyour körön. A Vnetek lehet a azonos Azure-előfizetéssel, hello ExpressRoute-kapcsolatcsoportot hello, vagy egy másik előfizetésben.

## <a name="expressroute-circuit-provisioning-states"></a>Kiépítés állapotok ExpressRoute-kapcsolatcsoportot
Minden egyes ExpressRoute-kapcsolatcsoportot két állapota van:

* Szolgáltatás szolgáltató üzembe helyezési állapota
* status

Állapotát a Microsoft a kiépítési állapotát jeleníti meg. A tulajdonság értéke tooEnabled, amikor az Expressroute-kapcsolatcsoportot létrehozni

hello kapcsolati szolgáltató üzembe helyezési állapota hello állapotát hello kapcsolat szolgáltatójánál oldalán jeleníti meg. Ez lehet *NotProvisioned*, *kiépítési*, vagy *kiépítve*. hello ExpressRoute-kapcsolatcsoportot állapotban kell lennie kiépítve az Ön toobe képes toouse azt.

### <a name="possible-states-of-an-expressroute-circuit"></a>Az ExpressRoute-kapcsolatcsoportot lehetséges állapota
Ez a rész felsorolja a kimenő hello lehetséges állapotok az ExpressRoute-kapcsolatcsoportot.

**A létrehozás időpontjában**

Hello ExpressRoute-kapcsolatcsoport állapotát, amint azt követő hello a hello PowerShell parancsmag toocreate hello ExpressRoute-kapcsolatcsoportot futtatása jelenik meg.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Amikor kapcsolat szolgáltatójánál hello áramkör kiépítés hello folyamatban van**

Látni fogja a hello ExpressRoute-kapcsolatcsoport állapotát, amint azt követő hello a hello szolgáltató kulcs toohello kapcsolatot adjon át, és azok elindította hello létesítésének folyamatát kell használnia.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Ha a kapcsolat szolgáltatójánál befejeződött hello létesítésének folyamatát kell használnia**

Látni fogja, amint hello kapcsolat szolgáltatójánál hello kiépítési folyamat befejeződött a következő állapotot hello az ExpressRoute-kapcsolatcsoportot hello.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Üzembe helyezve, és engedélyezve csak hello állapot hello áramkör lehet az Ön toobe képes toouse azt. Ha egy 2. rétegbeli szolgáltatót használja, beállíthatja a kapcsolatcsoport útválasztást csak akkor, ha az ebben az állapotban van.

**Ha a kapcsolat szolgáltatójánál megszüntetés hello áramkör van**

A kért hello szolgáltatás szolgáltató toodeprovision hello ExpressRoute-kapcsolatcsoportot látják hello áramkör toohello hello szolgáltató hello megszüntetés folyamat befejeződését követően a következő állapot beállítása.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Kiválaszthatja a toore engedélyezése, ha szükséges, vagy a PowerShell-parancsmagok futtatásához toodelete hello körön.  

> [!IMPORTANT]
> Ha futtatja hello PowerShell parancsmag toodelete hello áramkör hello ServiceProviderProvisioningState kiépítési vagy kiépítve hello művelet sikertelen lesz. Adjon használata a kapcsolat szolgáltató toodeprovision hello ExpressRoute-kapcsolatcsoportot először, és törölje a hello körön. Microsoft toobill hello áramkör folytatódik, amíg nem futtat PowerShell parancsmag toodelete hello áramkör hello.
> 
> 

## <a name="routing-session-configuration-state"></a>Munkamenet-konfiguráció útválasztási állapota
hello BGP üzembe helyezési állapota értesíti Önt arról, ha hello BGP munkamenet engedélyezve van a Microsoft edge hello. hello állapot engedélyezni kell az Ön toobe képes toouse hello társviszony-létesítés.

Fontos toocheck hello BGP munkamenet-állapot kifejezetten a Microsoft társviszony-létesítést is. Továbbá toohello BGP üzembe helyezési állapota, van egy másik állapothoz nevű *meghirdetett nyilvános előtag állapot*. hello meghirdetett nyilvános előtag állapotban kell lennie a *konfigurált* állapot, mind a hello BGP munkamenet toobe mentése és az útválasztási toowork-végpontok. 

Ha hello meghirdetett nyilvános előtag állapot értéke tooa *szükséges érvényesítési* állapotba kerül, a BGP-munkamenetet hello nincs engedélyezve, hello hirdetett előtagok nem felelt meg a hello hello útválasztási nyilvántartó valamelyikében SZÁMOT. 

> [!IMPORTANT]
> Hogy hello meghirdetett nyilvános előtag állapotban van-e *manuális érvényesítési* állapotba kerül, meg kell nyitnia a támogatási jegy [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , és adja meg a megbizonyosodhat róla, hogy Ön a tulajdonosa mentén meghirdetett hello IP-címek társított hello autonóm rendszer számát.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Az ExpressRoute-kapcsolat konfigurálása.
  
  * [ExpressRoute-kapcsolatcsoport létrehozása](expressroute-howto-circuit-arm.md)
  * [Útválasztás konfigurálása](expressroute-howto-routing-arm.md)
  * [Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md)

