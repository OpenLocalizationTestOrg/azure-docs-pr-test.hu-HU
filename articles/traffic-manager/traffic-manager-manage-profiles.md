---
title: Azure Traffic Manager-profilok aaaManage |} Microsoft Docs
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
ms.openlocfilehash: 0c6ab0c451581d039514a9de0b525b3937e45a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a>Az Azure Traffic Manager-profilok kezelése

Traffic Manager-profilokat a forgalom-útválasztási módszert toocontrol hello terjesztési forgalom tooyour felhőszolgáltatásokat vagy webhelyvégpontokat használja. Ez a cikk azt ismerteti, hogyan toocreate és profilok kezelése.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager-profil létrehozása

Traffic Manager-profil hello Azure-portál használatával hozhat létre. A profil létrehozása után konfigurálhatja hello Azure-portálon végpontok, figyelési és egyéb beállításokat. A TRAFFIC Manager legfeljebb too200 végpontot támogat. Azonban a használati forgatókönyvhöz csak néhány végpont szükséges.

### <a name="toocreate-a-traffic-manager-profile"></a>toocreate Traffic Manager-profil

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/). 
2. A hello **Hub** menüben kattintson a **új** > **hálózati** > **láthatja az összes**, kattintson a **forgalom Manager** profil tooopen hello **hozzon létre Traffic Manager-profil** panelen, majd kattintson a **létrehozása**.
3. A hello **hozzon létre Traffic Manager-profil** panelen, befejeződött, az alábbi módon:
    1. A **Név** területen adja meg a profil nevét. Ez a név toobe hello trafficmanager.net zónán belül egyedinek kell, és hello DNS-név eredményez <name>, trafficmanager.net, amely használt tooaccess a Traffic Manager-profil.
    2. A **útválasztási módszer**, jelölje be hello **prioritás** útválasztási módszer.
    3. A **előfizetés**, válassza ki ezt a profilt a toocreate kívánt hello előfizetést
    4. A **erőforráscsoport**, hozzon létre egy új erőforrás csoport tooplace a profil alapján.
    5. A **erőforráscsoport helye**, válassza ki a hello erőforráscsoport hello helyét. Ez a beállítás hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello Traffic Manager-profilt, amely globálisan telepíti.
    6. Kattintson a **Create** (Létrehozás) gombra.
    7. Ha hello globális a Traffic Manager-profil telepítése befejeződött, szerepel a megfelelő erőforráscsoport erőforrásként hello.

## <a name="disable-enable-or-delete-a-profile"></a>Profilok letiltása, engedélyezése vagy törlése

Egy meglévő profilt úgy, hogy a Traffic Manager nem hivatkozik a felhasználói kérelmek konfigurált toohello végpontok letilthatja. Ha letilt egy Traffic Manager-profilt, hello-profil és hello jellemzői hello-profil tartalmazza változatlanok maradnak, és szerkeszthető hello Traffic Manager felületén.  Átirányítások folytatódik, amint újra engedélyeznie hello-profil. Hello Azure portál Traffic Manager-profil létrehozásakor automatikusan engedélyezve van. Ha úgy dönt, többé nincs szüksége egy profilra, törölheti.

### <a name="toodisable-a-profile"></a>a profil toodisable

1. Ha egy egyéni tartománynevet használ, módosítsa a hello CNAME rekord az internetes DNS-kiszolgálón, hogy már nem tooyour Traffic Manager-profil.
2. Leállítja a forgalom lesznek irányítva toohello végpontok hello Traffic Manager-profil beállításain keresztül.
3. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** nevet, hogy szeretné, hogy toomodify, majd kattintson a hello hello Traffic Manager-profil annak az eredménye, hogy hello jelenik meg.
3. A hello **Traffic Manager-profil** panelen kattintson **áttekintése**, hello áttekintése paneljén kattintson **tiltsa le a**, majd erősítse meg a toodisable hello Traffic Manager-profil.

### <a name="tooenable-a-profile"></a>a profil tooenable

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** nevet, hogy szeretné, hogy toomodify, majd kattintson a hello hello Traffic Manager-profil annak az eredménye, hogy hello jelenik meg.
3. A hello **Traffic Manager-profil** panelen kattintson a **áttekintése**, majd a hello áttekintése panelen kattintson **engedélyezése**.
5. Ha egy egyéni tartománynevet használ, hozzon létre egy CNAME típusú erőforrásrekordot a internetes DNS-kiszolgáló toopoint toohello tartományneve a Traffic Manager-profil a.
6. Forgalom nem irányított toohello végpontok ismét.

### <a name="toodelete-a-profile"></a>a profil toodelete

1. Győződjön meg arról, hogy a hello DNS-erőforrásrekordot az internetes DNS-kiszolgálón már nem használja-e egy olyan CNAME erőforrásrekordot, amely a Traffic Manager-profil tartománynevére toohello mutat.
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** nevet, hogy szeretné, hogy toomodify, majd kattintson a hello hello Traffic Manager-profil annak az eredménye, hogy hello jelenik meg.
3. A hello **Traffic Manager-profil** panelen kattintson **áttekintése**, hello áttekintése paneljén kattintson **törlése**, majd erősítse meg a toodelete hello Traffic Manager-profil.

## <a name="next-steps"></a>Következő lépések

* [Végpont hozzáadása](traffic-manager-endpoints.md)
* [Prioritásos útválasztási mód konfigurálása](traffic-manager-configure-priority-routing-method.md)
* [Földrajzi útválasztási mód konfigurálása](traffic-manager-configure-geographic-routing-method.md) 
* [Súlyozott útválasztási mód konfigurálása](traffic-manager-configure-weighted-routing-method.md)
* [Teljesítménycentrikus útválasztási mód beállítása](traffic-manager-configure-performance-routing-method.md)
