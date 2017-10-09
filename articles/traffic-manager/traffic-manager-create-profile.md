---
title: az Azure Traffic Manager-profil aaaCreate |} Microsoft Docs
description: Ez a cikk ismerteti, hogyan toocreate Traffic Manager-profil
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: kumud
ms.openlocfilehash: 5cd3d2903552c9a0427da41a73f6f38f6b0afc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása

Ez a cikk ismerteti, hogyan profil **prioritás** útválasztási típus tooroute felhasználók tootwo Azure Web Apps végpontok hozhatók létre. Hello segítségével **prioritás** útválasztás típusa, az összes forgalom az irányított toohello első végpont közben hello második másolatok biztonsági mentéséhez. Ennek eredményeképpen felhasználók lehet irányított toohello második végpont, ha hello első végpont akkor kerül sérült állapotba.

Ebben a cikkben korábban létrehozott Azure Web Apps végpontokat az újonnan létrehozott társított toothis Traffic Manager-profil. További kapcsolatos toolearn toocreate Azure Web Apps végpontok, látogasson el a hello [Azure Web Apps dokumentációs oldal](https://docs.microsoft.com/azure/app-service-web/). Hozzáadhat a tetszőleges végpontot, amely egy DNS-névvel rendelkezik, és elérhető-e keresztül hello nyilvános internetes és az, hogy használjuk Azure Web Apps végpontok példaként.

### <a name="create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása
1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com). Ha még nem rendelkezik fiókkal, akkor az egy [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/free/). 
2. A hello **Hub** menüben kattintson a **új** > **hálózati** > **láthatja az összes**, kattintson a **forgalom Manager** profil tooopen hello **hozzon létre Traffic Manager-profil** panelen, majd kattintson a **létrehozása**.
3. A hello **hozzon létre Traffic Manager-profil** panelen, befejeződött, az alábbi módon:
    1. A **Név** területen adja meg a profil nevét. Ez a név toobe hello trafficmanager.net zónán belül egyedinek kell, és hello DNS-név eredményez <name>, amely használt tooaccess trafficmanager.net a Traffic Manager-profil.
    2. A **útválasztási módszer**, jelölje be hello **prioritás** útválasztási módszer.
    3. A **előfizetés**, válassza ki ezt a profilt a toocreate kívánt hello előfizetést
    4. A **erőforráscsoport**, hozzon létre egy új erőforrás csoport tooplace a profil alapján.
    5. A **erőforráscsoport helye**, válassza ki a hello erőforráscsoport hello helyét. Ez a beállítás hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello Traffic Manager-profilt, amely globálisan telepíti.
    6. Kattintson a **Create** (Létrehozás) gombra.
    7. Ha hello globális a Traffic Manager-profil telepítése befejeződött, szerepel a megfelelő erőforráscsoport erőforrásként hello.

    ![Traffic Manager-profil létrehozása](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>A Traffic Manager-végpont-hozzáadáshoz

1. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** szakaszt, és kattintson hello traffic manager-profil megelőző hello hello létrehozott név annak az eredménye, hogy hello jelenik meg.
2. A hello **Traffic Manager-profil** paneljén, hello **beállítások** kattintson **végpontok**.
3. A hello **végpontok** panel, amelyen megjelenik, kattintson a **Hozzáadás**.
4. A hello **végpont hozzáadása** panelen, befejeződött, az alábbi módon:
    1. A **Típusnál** kattintson az **Azure-végpont** lehetőségre.
    2. Adjon meg egy **neve** alapjául toorecognize ezen a végponton.
    3. A **céloz erőforrástípus**, kattintson a **App Service**.
    4. A **célerőforrás**, kattintson a **alkalmazásszolgáltatás kiválasztása** tooshow hello listája alatt hello webalkalmazások hello ugyanahhoz az előfizetéshez. A hello **erőforrás** panel akkor jelenik meg, mentse hello megjeleníteni kívánt tooadd hello első végpont, az App service.
    5. A **Prioritásnál** válassza az **1-es** értéket. Az eredmény teljes forgalmat toothis végpont kifogástalan esetén.
    6. A **Beállítás letiltottként** jelölőnégyzetet ne jelölje ki.
    7. Kattintson az **OK** gombra
5.  Hello következő Azure Web Apps végpont ismételje meg a 3. és 4. Győződjön meg arról, hogy tooadd azt a **prioritás** beállított érték **2**.
6.  Ha mindkét végpont hello hozzáadása befejeződött, azok megjelennek hello **Traffic Manager-profil** panelről és figyelési állapotuk **Online**.

    ![A Traffic Manager-végpont hozzáadása](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-hello-traffic-manager-profile"></a>Hello Traffic Manager-profil használata
1.  A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** hello előző szakaszban létrehozott nevét. A megjelenített eredmények hello kattintson a hello traffic manager-profil.
2. A hello **Traffic Manager-profil** panelen kattintson a **áttekintése**.
3. Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét. Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható. Ebben az esetben összes kérelem irányított toohello első végpont, és ha Traffic Manager azt észleli, hogy nyilvánítva, hello forgalom automatikusan átkerül toohello következő végpontot.

## <a name="delete-hello-traffic-manager-profile"></a>Hello Traffic Manager-profil törlése
Már nincs szükség, amikor hello erőforráscsoportot és az Ön által létrehozott hello Traffic Manager-profil törlése. toodo tehát válasszon hello erőforráscsoport hello **Traffic Manager-profil** panel megnyitásához, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

- További információ [útválasztási típusok](traffic-manager-routing-methods.md).
- További tudnivalók típusú végpontok esetében [típusú végpontok esetében](traffic-manager-endpoint-types.md).
- További információ [végpontmonitoring kijelző](traffic-manager-monitoring.md).



