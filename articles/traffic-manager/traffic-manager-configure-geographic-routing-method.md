---
title: "földrajzi aaaConfigure forgalom-útválasztási módszerrel Azure Traffic Manager |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooconfigure hello földrajzi forgalom-útválasztási módszert Azure Traffic Manager használatával"
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
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 4142389211ae54e7feea6564641e01e4477491e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-geographic-traffic-routing-method-using-traffic-manager"></a>Hello földrajzi forgalom-útválasztási módszert használja a Traffic Manager konfigurálása

hello földrajzi forgalom-útválasztási módszert lehetővé teszi, hogy toodirect forgalom toospecific végpontok, ahol hello kérések érkeznek hello földrajzi helye alapján. Ez az oktatóanyag bemutatja, hogyan toocreate egy Traffic Manager profilhoz, a útválasztási módszerrel, és konfigurálja a hello végpontok tooreceive adott földrajzi forgalmát.

## <a name="create-a-traffic-manager-profile"></a>A Traffic Manager-profil létrehozása

1. Egy böngészőből toohello bejelentkezés [Azure-portálon](http://portal.azure.com). Ha még nincs fiókja, regisztrálhat egy [egy hónapos ingyenes próbaverzióra](https://azure.microsoft.com/free/).
2. Hello központ menüben kattintson a **új** > **hálózati** > **láthatja az összes**, és kattintson a **Traffic Manager-profil**tooopen hello **hozzon létre Traffic Manager-profil** panelen.
3. A hello **hozzon létre Traffic Manager-profil** panel:
    1. Adja meg a profil nevét. Ezt a nevet kell toobe hello trafficmanager.net zónán belül egyedi, és hatására a hello DNS-név <profilename>, amelyek trafficmanager.net használt tooaccess a Traffic Manager-profilt kell.
    2. Jelölje be hello **Geographic** útválasztási módszer.
    3. Válassza ki ezt a profilt a toocreate kívánt hello előfizetést.
    4. Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy új erőforrás csoport tooplace a profil alapján. Ha úgy dönt, toocreate egy új erőforráscsoportot, használja a hello **az erőforráscsoport helye** hello erőforráscsoport legördülő toospecify hello helyét. Ez a beállítás hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello Traffic Manager-profilt, amely globálisan telepíti.
    5. Miután rákattintott **létrehozása**, a Traffic Manager-profil létrehozása és telepítése globálisan.

![Traffic Manager-profil létrehozása](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Végpont hozzáadása

1. Keresse meg a most létrehozott hello portál keresési sávon hello Traffic Manager-profil neve, majd kattintson a hello eredményt, kimutatható.
2. Keresse meg a túl**beállítások** -> **végpontok** hello Traffic Manager panelén.
3. Kattintson a **Hozzáadás** tooshow hello **végpont hozzáadása** panelen.
3. A hello **végpontok** panelen kattintson **hozzáadása** és hello **végpont hozzáadása** panel, amelyen jelenik meg, végezze el az alábbiak szerint:
4. Válassza ki **típus** hozzáadni kívánt végpont hello típusától függően. Éles környezetben használt földrajzi útválasztási profilokhoz erősen ajánlott több végpont gyermek profil tartalmazó beágyazott végpont típus használatával. További részletekért lásd: [földrajzi forgalom-útválasztási módszerekkel kapcsolatos gyakori kérdések](traffic-manager-FAQs.md).
5. Adjon meg egy **neve** alapjául toorecognize ezen a végponton.
6. Bizonyos mezők ezen a panelen hozzáadni kívánt végpont hello típusú függ:
    1. Ha egy Azure végpontot hozzáadni, válassza ki a hello **erőforrás Céltípust** és hello **cél** kívánt hello erőforrás alapján toodirect-forgalom
    2. Ha hozzáadni egy **külső** végpont, adja meg a hello **teljesen minősített tartománynevét (FQDN)** használ a végponthoz.
    3. Ha ad hozzá egy **beágyazott végpont**, jelölje be hello **célerőforrás** , amely megfelel a toohello gyermek profil toouse szeretne, és adja meg a hello **minimális gyermek végpontok száma**.
7. A földrajzi-hozzárendelési szakasza hello használjon hello tooadd hello régiókban található forgalom küldött toobe toothis végpont, ahová legördülő. Hozzá kell adni legalább egy régió tartozik, és akkor leképezése több régióba is.
8. Ismételje meg ezt az összes végpontok azt szeretné, hogy a profil alapján tooadd

![A Traffic Manager-végpont hozzáadása](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-hello-traffic-manager-profile"></a>Hello Traffic Manager-profil használata
1.  A hello portal keresősávban, keresse meg a hello **Traffic Manager-profil** nevű hello előző szakaszban létrehozott, és kattintson a hello hello traffic manager-profil annak az eredménye, hogy hello jelenik meg.
2. A hello **Traffic Manager-profil** panelen kattintson a **áttekintése**.
3. Hello **Traffic Manager-profil** csempe megjeleníti az újonnan létrehozott Traffic Manager-profil hello DNS-nevét. Ez minden ügyfelek (például a webböngésző segítségével tooit navigálva) irányított tooget toohello megfelelő végpont hello útválasztás típusa alapján is használható.  Földrajzi útválasztási hello esetében a Traffic Manager megvizsgálja a bejövő kérelem hello hello forrás IP-cím, és határozza meg, amelyből származó hello régióban. Adott régióban csatlakoztatott tooan végpont, forgalom akkor irányított toothere. Ha ebben a régióban nem csatlakoztatott tooan végpont, majd a Traffic Manager NODATA lekérdezés választ ad.

## <a name="next-steps"></a>Következő lépések

- További információ [Geographic forgalom-útválasztási módszer](traffic-manager-routing-methods.md#geographic).
- Ismerje meg, hogyan túl[Traffic Manager-beállítások tesztelésére](traffic-manager-testing-settings.md).
