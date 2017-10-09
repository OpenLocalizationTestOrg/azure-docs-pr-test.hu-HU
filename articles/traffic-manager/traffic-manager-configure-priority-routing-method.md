---
title: "aaaConfigure prioritású virtuális gép forgalom-útválasztási módszerrel Azure Traffic Manager |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooconfigure hello prioritású virtuális gép forgalom-útválasztási módszert a Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: dd3e3bb2a727e5ea087cee35962c8e6f7c357282
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Prioritás forgalom-útválasztási módszert a Traffic Manager konfigurálása

Hello webhely módot, függetlenül az Azure Websitesra már meg feladatátvételi funkcióját (más néven régiónként) adatközponton belüli webhelyekhez. A TRAFFIC Manager biztosít feladatátvételt különböző adatközpontokban webhelyekhez.

Szolgáltatás feladatátvétele egy közös mintát toosend forgalom tooa elsődleges szolgáltatása, és adja meg a feladatátvételi azonos biztonsági mentési szolgáltatások. hello következő részben megtudhatja, hogyan tooconfigure ez előrébb feladatátvétel az Azure felhőszolgáltatások és webhelyek:

## <a name="tooconfigure-hello-priority-traffic-routing-method"></a>tooconfigure hello prioritású virtuális gép forgalom-útválasztási módszert

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/). 
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profilok** majd tooconfigure hello útválasztási módszer a használni kívánt hello-profil neve.
3. A hello **Traffic Manager-profil** panelen ellenőrizze, hogy mindkét hello felhőszolgáltatások és a webhelyeket, tooinclude konfigurációs találhatók.
4. A hello **beállítások** területen kattintson **konfigurációs**, és a hello **konfigurációs** panelen, befejeződött, az alábbi módon:
    1. A **forgalom-útválasztási módszer beállításai**, győződjön meg arról, hogy hello forgalom-útválasztási módszert **prioritás**. Ha nem, kattintson a **prioritás** hello legördülő listából.
    2. Set hello **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:
        1. Jelölje be hello megfelelő **protokoll**, és adja meg a hello **Port** számát. 
        2. A **elérési** írja be a perjellel  */* . toomonitor végpontok, adjon meg egy elérési utat és fájlnevet. A perjel "/" hello relatív elérési út érvényes bejegyzés, és azt jelenti, hogy hello fájl hello gyökérkönyvtárában (alapértelmezett).
        3. Hello hello oldal tetején, kattintson a **mentése**.
5. A hello **beállítások** kattintson **végpontok**.
6. A hello **végpontok** panelt, tekintse át a végpontokat hello rangsort. Ha bejelöli hello **prioritás** forgalom-útválasztási módszert, kijelölt hello végpontok hello sorrendje számít. Ellenőrizze a végpontok hello fontossági sorrendjét.  hello elsődleges végpont felül van. Gondosan ellenőrizze hello sorrendnek jelenik meg. összes kérelmet a rendszer irányított toohello első végpont, majd a Traffic Manager azt észleli, ha nyilvánítva, hello forgalom automatikusan átkerül toohello következő végpontot. 
7. toochange hello végpont prioritásuk szerinti sorrendben történik, kattintson a hello végpont, és a hello **végpont** panel, amelyen megjelenik, kattintson a **szerkesztése** , és módosítsa a hello **prioritás** érték szükség szerint . 
8. Kattintson a **mentése** toosave hello végpont beállítások módosítása.
9. A konfigurációs módosítások elvégzése után kattintson **mentése** hello lap hello alján.
10. Tesztelje hello módosításokat a konfigurációt az alábbiak szerint:
    1.  Hello portal keresősávban keressen a hello Traffic Manager-profil nevét, majd kattintson a hello Traffic Manager-profil hello eredmények jelenik meg, hogy hello.
    2.  A hello **Traffic Manager** profil panelen, kattintson a **áttekintése**.
    3.  Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét. Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható. Ebben az esetben minden kérésnél irányított toohello első végpont, és a Traffic Manager azt észleli, ha nyilvánítva, hello forgalom automatikusan átkerül toohello következő végpontot.
11. A Traffic Manager-profil működik, ha szerkesztése hello DNS-rekordot a mérvadó DNS-kiszolgáló toopoint meg a vállalati tartomány neve toohello Traffic Manager szolgáltatásbeli tartománynevére.

![Prioritás forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a>Következő lépések


- További tudnivalók [forgalom-útválasztási módszert súlyozott](traffic-manager-configure-weighted-routing-method.md).
- További tudnivalók [teljesítmény útválasztási módszer](traffic-manager-configure-performance-routing-method.md).
- További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).
- Ismerje meg, hogyan túl[Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png