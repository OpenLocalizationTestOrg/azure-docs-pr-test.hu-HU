---
title: "aaaConfigure súlyozott ciklikus időszeletelési forgalom-használata az Azure Traffic Manager útválasztási módszer |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooload egyenleg módszerrel ciklikus multiplexelés a Traffic Manager forgalom"
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
ms.openlocfilehash: 7e2866ead0b2b653845435dd420a763c5e175f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-weighted-traffic-routing-method-in-traffic-manager"></a>Súlyozott hello forgalom-útválasztási módszert a Traffic Manager konfigurálása

A közös forgalom útválasztási módszer minta tooprovide korábbiakkal megegyező végpontokat, amelyek közé tartoznak a felhőszolgáltatások és webhelyek, és küldjön forgalmat tooeach ciklikus multiplexelés csoportja. a lépéseket követve hello szerkezeti hogyan tooconfigure ez típusú forgalom-útválasztási módszer.

> [!NOTE]
> Azure-webhelyek már meg ciklikus multiplexelés terheléselosztási funkciója (más néven régiónként) adatközponton belüli webhelyekhez. A TRAFFIC Manager lehetővé teszi toospecify ciklikus időszeletelési forgalom-útválasztási módszert webhelyekhez különböző adatközpontokban.

## <a name="tooconfigure-hello-weighted-traffic-routing-method"></a>tooconfigure súlyozott hello forgalom-útválasztási módszert

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/). 
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profilok** majd tooconfigure hello útválasztási módszer a használni kívánt hello-profil neve.
3. A hello **Traffic Manager-profil** panelen ellenőrizze, hogy mindkét hello felhőszolgáltatások és a webhelyeket, tooinclude konfigurációs találhatók.
4. A hello **beállítások** területen kattintson **konfigurációs**, és a hello **konfigurációs** panelen, befejeződött, az alábbi módon:
    1. A **forgalom-útválasztási módszer beállításai**, győződjön meg arról, hogy hello forgalom-útválasztási módszert **Weighted**. Ha nem, kattintson a **Weighted** hello legördülő listából.
    2. Set hello **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:
        1. Jelölje be hello megfelelő **protokoll**, és adja meg a hello **Port** számát. 
        2. A **elérési** írja be a perjellel  */* . toomonitor végpontok, adjon meg egy elérési utat és fájlnevet. A perjel "/" hello relatív elérési út érvényes bejegyzés, és azt jelenti, hogy hello fájl hello gyökérkönyvtárában (alapértelmezett).
        3. Hello hello oldal tetején, kattintson a **mentése**.
5. Tesztelje hello módosításokat a konfigurációt az alábbiak szerint:
    1.  Hello portal keresősávban keressen a hello Traffic Manager-profil nevét, majd kattintson a hello Traffic Manager-profil hello eredmények jelenik meg, hogy hello.
    2.  A hello **Traffic Manager** profil panelen, kattintson a **áttekintése**.
    3.  Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét. Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható. Ebben az esetben az összes rendszer kérést átirányítja a ciklikus multiplexelés végpontok.
6. A Traffic Manager-profil működik, ha szerkesztése hello DNS-rekordot a mérvadó DNS-kiszolgáló toopoint meg a vállalati tartomány neve toohello Traffic Manager szolgáltatásbeli tartománynevére.

![Súlyozott forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [prioritású virtuális gép forgalom-útválasztási módszer](traffic-manager-configure-priority-routing-method.md).
- További tudnivalók [teljesítmény forgalom-útválasztási módszer](traffic-manager-configure-performance-routing-method.md).
- További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).
- Ismerje meg, hogyan túl[Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
