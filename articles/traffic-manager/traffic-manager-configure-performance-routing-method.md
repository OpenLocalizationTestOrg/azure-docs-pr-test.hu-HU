---
title: "aaaConfigure teljesítmény forgalom-útválasztási módszerrel Azure Traffic Manager |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooconfigure tooroute Traffic Manager forgalom-toohello végpont legkisebb mértékű késleltetést"
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
ms.openlocfilehash: d0ccd4c9de411844c6f36068859265244f4aa52b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-performance-traffic-routing-method"></a>Hello teljesítmény forgalom útválasztási módszer konfigurálása

hello teljesítmény forgalom-útválasztási módszert lehetővé teszi a toodirect forgalom toohello végpont a legkisebb mértékű késleltetést hello hello ügyfél hálózatról. A legkisebb mértékű késleltetést hello hello datacenter általában földrajzi távolság legközelebbi hello. A forgalom-útválasztási módszert nem fiókot használja a hálózati konfigurációban valós idejű módosításokat, vagy nem tölthető be.

##  <a name="tooconfigure-performance-routing-method"></a>tooconfigure teljesítmény útválasztási módszer

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/). 
2. A hello portal keresősávban, keresse meg a hello **Traffic Manager-profilok** majd tooconfigure hello útválasztási módszer a használni kívánt hello-profil neve.
3. A hello **Traffic Manager-profil** panelen ellenőrizze, hogy mindkét hello felhőszolgáltatások és a webhelyeket, tooinclude konfigurációs találhatók.
4. A hello **beállítások** területen kattintson **konfigurációs**, és a hello **konfigurációs** panelen, befejeződött, az alábbi módon:
    1. A **forgalom-útválasztási módszer beállításai**, a **útválasztási módszer** válasszon **teljesítmény**.
    2. Set hello **végpont figyelőbeállítások** azonos összes minden végponton belül ezt a profilt, az alábbiak szerint:
        1. Jelölje be hello megfelelő **protokoll**, és adja meg a hello **Port** számát. 
        2. A **elérési** írja be a perjellel  */* . toomonitor végpontok, adjon meg egy elérési utat és fájlnevet. A perjel "/" hello relatív elérési út érvényes bejegyzés, és azt jelenti, hogy hello fájl hello gyökérkönyvtárában (alapértelmezett).
        3. Hello hello oldal tetején, kattintson a **mentése**.
5.  Tesztelje hello módosításokat a konfigurációt az alábbiak szerint:
    1.  Hello portal keresősávban keressen a hello Traffic Manager-profil nevét, majd kattintson a hello Traffic Manager-profil hello eredmények jelenik meg, hogy hello.
    2.  A hello **Traffic Manager** profil panelen, kattintson a **áttekintése**.
    3.  Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét. Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható. Ebben az esetben az összes kérelem hello legkisebb mértékű késleltetést hello ügyfél hálózatról irányított toohello végpont.
6. A Traffic Manager-profil működik, ha szerkesztése hello DNS-rekordot a mérvadó DNS-kiszolgáló toopoint meg a vállalati tartomány neve toohello Traffic Manager szolgáltatásbeli tartománynevére.

![Teljesítmény forgalom-útválasztási módszert használja a Traffic Manager konfigurálása][1]

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [forgalom-útválasztási módszert súlyozott](traffic-manager-configure-weighted-routing-method.md).
- További tudnivalók [prioritású virtuális gép útválasztási módszer](traffic-manager-configure-priority-routing-method.md).
- További tudnivalók [földrajzi útválasztási módszer](traffic-manager-configure-geographic-routing-method.md).
- Ismerje meg, hogyan túl[Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png