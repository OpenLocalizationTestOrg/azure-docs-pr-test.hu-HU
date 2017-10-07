---
title: "aaaManage végpontok Azure Traffic Managerben |} Microsoft Docs"
description: "Ez a cikk a végpontok Azure Traffic Managerben végzett felvételében, eltávolításában, engedélyezésében és letiltásában segít."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kumud
ms.openlocfilehash: fc65874ae2eaeb6fca5d8c4f33403c258307bdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-disable-enable-or-delete-endpoints"></a>Végpontok felvétele, letiltása, engedélyezése és törlése

hello Azure App Service Web Apps szolgáltatása is biztosít feladatátvételi és ciklikus időszeletelési forgalom-útválasztási funkciót az adatközponton, függetlenül hello webhely működési belüli webhelyekhez. Az Azure Traffic Manager lehetővé teszi toospecify feladatátvételi és ciklikus időszeletelési forgalom-útválasztás webhelyek és felhőszolgáltatások szolgáltatások különböző adatközpontokban. hello első lépés szükséges tooprovide, hogy a funkció tooadd hello felhőalapú szolgáltatás, vagy a webhely végpont tooTraffic Manager is.

A Traffic Manager-profil részét képező egyedi végpontok is letilthatók. A végpont letiltása elhagyja hello-profil részeként, de hello-profil úgy viselkedik, mintha hello végpont nem szerepelne benne. Ez a művelet a karbantartási módban lévő vagy újratelepítés alatt álló végpont átmeneti eltávolítására való. Ha hello végpont újra működik, engedélyezhető.

> [!NOTE]
> A végpont letiltása rendelkezik semmi toodo rendszerbeli üzembe helyezési állapotát az Azure-ban. A kifogástalan állapotú végpontok maradjon naprakész, és képes forgalom tooreceive, akkor is, ha le van tiltva a Traffic Manager. Továbbá a végpont egyik profilban végzett letiltása nem befolyásolja a többi profilban érvényes állapotát.

## <a name="tooadd-a-cloud-service-or-an-app-service-endpoint-tooa-traffic-manager-profile"></a>egy felhőalapú szolgáltatás tooadd vagy egy App service végpont tooa Traffic Manager-profil

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** nevet, hogy szeretné, hogy toomodify, majd kattintson a hello hello Traffic Manager-profil annak az eredménye, hogy hello jelenik meg.
3. A hello **Traffic Manager-profil** paneljén, hello **beállítások** kattintson **végpontok**.
4. A hello **végpontok** panel, amelyen megjelenik, kattintson a **Hozzáadás**.
5. A hello **végpont hozzáadása** panelen, befejeződött, az alábbi módon:
    1. A **Típusnál** kattintson az **Azure-végpont** lehetőségre.
    2. Adjon meg egy **neve** alapjául toorecognize ezen a végponton.
    3. A **céloz erőforrástípus**, a legördülő lista hello, válasszon hello megfelelő erőforrástípust.
    4. A **célerőforrás**, a legördülő hello, majd a megfelelő célerőforrása hello tooshow hello listaelem alatt lévő erőforrások hello ugyanahhoz az előfizetéshez a hello **erőforrások panel**. A hello **erőforrás** panel akkor jelenik meg, mentse hello szolgáltatást, amelyet tooadd, hello első végpont.
    5. A **Prioritásnál** válassza az **1-es** értéket. Az eredmény teljes forgalmat toothis végpont kifogástalan esetén.
    6. A **Beállítás letiltottként** jelölőnégyzetet ne jelölje ki.
    7. Kattintson az **OK** gombra
6.  Ismételje meg a 4. és 5 tooadd hello következő Azure végpontot. Győződjön meg arról, hogy tooadd azt a **prioritás** beállított érték **2**.
7.  Ha mindkét végpont hello hozzáadása befejeződött, azok megjelennek hello **Traffic Manager-profil** panelről és figyelési állapotuk **Online**.

> [!NOTE]
> Ad hozzá, vagy távolítsa el a végpont a profilnak az hello *feladatátvételi* forgalom-útválasztási módszer, hello feladatátvételi prioritási listát rendelhetik azok igényeinek megfelelően. Hello sorrendjének hello feladatátvételi prioritási listát hello konfigurálása lapon állíthatja be. További információkért tekintse meg a [Feladatátvételi forgalom-útválasztás beállítása](traffic-manager-configure-failover-routing-method.md) szakaszt.

## <a name="toodisable-an-endpoint"></a>a végpont toodisable

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** , hogy azt szeretné, hogy toomodify, és kattintson a hello hello megjelenített eredmények a Traffic Manager-profil neve.
3. A hello **Traffic Manager-profil** paneljén, hello **beállítások** kattintson **végpontok**. 
4. Kattintson hello végpontra, amelyet az toodisable, majd a hello **végpont** panel, amelyen megjelenik, kattintson **szerkesztése**.
5. A hello **végpont** panelen állapotmódosítás hello végpont túl**letiltott**, és kattintson a **mentése**.
6. Ügyfelek toosend forgalom toohello endpoint idejére hello idő élettartamát (TTL) továbbra is. Hello TTL hello hello Traffic Manager-profil konfiguráció lapján módosítható.

## <a name="tooenable-an-endpoint"></a>a végpont tooenable

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** , hogy azt szeretné, hogy toomodify, és kattintson a hello hello megjelenített eredmények a Traffic Manager-profil neve.
3. A hello **Traffic Manager-profil** paneljén, hello **beállítások** kattintson **végpontok**. 
4. Kattintson hello végpontra, amelyet az toodisable, majd a hello **végpont** panel, amelyen megjelenik, kattintson **szerkesztése**.
5. A hello **végpont** panelen állapotmódosítás hello végpont túl**engedélyezve**, és kattintson a **mentése**.
6. Ügyfelek toosend forgalom toohello endpoint idejére hello idő élettartamát (TTL) továbbra is. Hello TTL hello hello Traffic Manager-profil konfiguráció lapján módosítható.

## <a name="toodelete-an-endpoint"></a>a végpont toodelete

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com).
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** , hogy azt szeretné, hogy toomodify, és kattintson a hello hello megjelenített eredmények a Traffic Manager-profil neve.
3. A hello **Traffic Manager-profil** paneljén, hello **beállítások** kattintson **végpontok**. 
4. Kattintson hello végpontra, amelyet az toodisable, majd a hello **végpont** panel, amelyen megjelenik, kattintson **szerkesztése**.
5. A hello **végpont** panelen állapotmódosítás hello végpont túl**engedélyezve**, és kattintson a **mentése**.


## <a name="next-steps"></a>Következő lépések

* [Traffic Manager-profilok kezelése](traffic-manager-manage-profiles.md)
* [Útválasztási módszerek konfigurálása](traffic-manager-configure-routing-method.md)
* [A Traffic Manager csökkentett teljesítményének elhárítása](traffic-manager-troubleshooting-degraded.md)
* [A Traffic Manager teljesítményével kapcsolatos megfontolások](traffic-manager-performance-considerations.md)
* [A Traffic Manager műveletei (REST API-referencia)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

