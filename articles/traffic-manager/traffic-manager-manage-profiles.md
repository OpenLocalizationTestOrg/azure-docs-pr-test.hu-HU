---
title: "Azure Traffic Manager-profilok kezelése | Microsoft Docs"
description: "Ennek a cikknek a segítségével létrehozhatja, letilthatja, engedélyezheti és törölheti az Azure Traffic Manager-profilokat."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.openlocfilehash: a5164282264124835692bc72a4ab61891aa7af9d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a>Az Azure Traffic Manager-profilok kezelése

A Traffic Manager-profilok forgalom-útválasztási módszereket használnak a felhőszolgáltatások vagy webhelyek végpontjaira érkező forgalom elosztásának szabályozásához. Ez a cikk ismerteti a profilok létrehozásának és kezelésének módját.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása

Az Azure Portal használatával Traffic Manager-profilokat hozhat létre. A profil létrehozása után végpontokat, megfigyelést és más beállításokat konfigurálhat az Azure Portalon. A Traffic Manager profilonként legfeljebb 200 végpontot támogat. Azonban a használati forgatókönyvhöz csak néhány végpont szükséges.

### <a name="to-create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása

1. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/). 
2. A **Hub** menüben kattintson az **Új** > **Hálózatkezelés** > **Az összes megjelenítése** elemre, kattintson a **Traffic Manager**-profilra a **Traffic Manager-profil létrehozása** panel megnyitásához, majd kattintson a **Létrehozás** gombra.
3. A **Traffic Manager-profil létrehozása** panelen adja meg a következőket:
    1. A **Név** területen adja meg a profil nevét. Ennek a névnek egyedinek kell lennie a trafficmanager.net zónában és a(z) <name>, trafficmanager.net DNS-nevet eredményezi, amellyel elérhető a Traffic Manager-profil.
    2. Az **Útválasztási módszer** területen válassza a **Prioritás** útválasztási módszert.
    3. Az **Előfizetés** területen válassza ki azt az előfizetést, amely alatt létre szeretné hozni ezt a profilt
    4. Az **Erőforráscsoport** mezőben hozzon létre egy új erőforráscsoportot, amely alá ezt a profilt helyezi.
    5. Az **Erőforráscsoport helye** területen válassza ki az erőforráscsoport helyét. Ez a beállítás az erőforráscsoport helyére vonatkozik, és nincs hatással a globálisan üzembe helyezendő Traffic Manager-profilra.
    6. Kattintson a **Létrehozás** gombra.
    7. Amikor befejeződött a Traffic Manager-profil globális üzembe helyezése, az egyik erőforrásként szerepel majd a megfelelő erőforráscsoportban.

## <a name="disable-enable-or-delete-a-profile"></a>Profilok letiltása, engedélyezése vagy törlése

Letilthat létező profilokat, így a Traffic Manager nem hivatkozik a konfigurált végpontokra irányuló felhasználói kérelmekre. Amikor letilt egy Traffic Manager-profilt, a profil és az általa tartalmazott információk használhatók és a Traffic Manager-felületen szerkeszthetők maradnak.  Amikor újra engedélyezi a profilt, a hivatkozások ismét elérhetővé válnak. Amikor létrehoz egy Traffic Manager-profilt az Azure Portalon, a rendszer automatikusan engedélyezi a használatát. Ha úgy dönt, többé nincs szüksége egy profilra, törölheti.

### <a name="to-disable-a-profile"></a>A profilok letiltása

1. Ha egyéni tartománynevet használ, módosítsa az internetes DNS-kiszolgáló CNAME-rekordját, hogy többé ne mutasson a Traffic Manager-profilra.
2. A forgalom többé nem lesz a végpontokra irányítva a Traffic Manager-profil beállításain keresztül.
3. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panelen kattintson az **Áttekintés** gombra, az Áttekintés panelen kattintson a **Letiltás** gombra, majd erősítse meg a Traffic Manager-profil letiltását.

### <a name="to-enable-a-profile"></a>A profilok engedélyezése

1. Egy böngészőben jelentkezzen be az [Azure Portalra](http://portal.azure.com).
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panelen kattintson az **Áttekintés** gombra, majd az Áttekintés panelen kattintson az **Engedélyezés** gombra.
5. Ha egyéni tartománynevet használ, hozzon létre egy CNAME-erőforrásrekordot az internetes DNS-kiszolgálón, amely a Traffic Manager-profil tartománynevére mutat.
6. A forgalom ismét a végpontokra lesz irányítva.

### <a name="to-delete-a-profile"></a>A profilok törlése

1. Győződjön meg arról, hogy az internetes DNS-kiszolgálón érvényes DNS-erőforrásrekord már nem használ olyan CNAME-erőforrásrekordot, amely a Traffic Manager-profil tartománynevére mutat.
2. A portál keresősávjában keressen rá a módosítani kívánt **Traffic Manager-profil** nevére, majd kattintson a Traffic Manager-profilra a megjelenített eredmények között.
3. A **Traffic Manager-profil** panelen kattintson az **Áttekintés** gombra, az Áttekintés panelen kattintson a **Törlés** gombra, majd erősítse meg a Traffic Manager-profil törlését.

## <a name="next-steps"></a>Következő lépések

* [Végpont hozzáadása](traffic-manager-endpoints.md)
* [Prioritásos útválasztási mód konfigurálása](traffic-manager-configure-priority-routing-method.md)
* [Földrajzi útválasztási mód konfigurálása](traffic-manager-configure-geographic-routing-method.md) 
* [Súlyozott útválasztási mód konfigurálása](traffic-manager-configure-weighted-routing-method.md)
* [Teljesítménycentrikus útválasztási mód beállítása](traffic-manager-configure-performance-routing-method.md)
