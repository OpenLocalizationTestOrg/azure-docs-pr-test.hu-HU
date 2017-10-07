---
title: "aaaStorSimple Manager szolgáltatás irányítópultját |} Microsoft Docs"
description: "Hello StorSimple Manager szolgáltatás irányítópultját ismerteti és bemutatja hogyan toouse azt a StorSimple megoldásban toomonitor hello állapotát."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: dc1197eb5deac337215b260845631a4f04be1011
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-dashboard"></a>Hello StorSimple Manager szolgáltatás irányítópult
## <a name="overview"></a>Áttekintés
hello StorSimple Manager szolgáltatás irányítópult-oldalon összegzését jeleníti meg az összes hello eszközök, amelyek csatlakoztatott toohello StorSimple Manager szolgáltatás, azok a rendszergazda figyelmet igénylő kiemelve. Ez az oktatóanyag bemutatja a hello irányítópult-oldalon, hello irányítópult tartalom és funkcióját ismerteti, és azokat hello feladatokat hajthat végre ezen a lapon.

![Szolgáltatási irányítópult](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

hello StorSimple Manager szolgáltatás irányítópultját jeleníti meg a következő információ hello:

* A hello **diagram** területen láthatja a eszközök diagram hello vonatkozó mérőszámok. Megtekintheti az elsődleges tárolási hello (helyileg rögzített és Szintezett) összes hello eszközök, valamint egy meghatározott időtartamra vonatkozóan eszközök által használt hello felhőalapú tárolást használ. Hello vezérlőkkel hello diagram toospecify 1 hét, hónap 1, 3 hónapos vagy 1 év időskálára hello jobb felső sarkában.
* Hello **használatának áttekintése** mutat be hello kiosztásakor és az összes eszközön összes eszközök relatív toohello rendelkezésre álló tárhelyet által használt elsődleges tárhelyet. **Kiépített** hivatkozik, amely készen áll a testreszabásra, és használatra, miközben tárolási toohello mennyisége **használható** kötetek toousage hivatkozik, hello kezdeményezők, amelyek csatlakoztatott toohello eszközök láthatók.
* Hello **riasztások** területen láthatók hello összes aktív riasztás pillanatképet minden hello eszközön, a riasztás súlyosságát szerint csoportosítva. Hello súlyossági szint kattintva megnyílik a hello **riasztások** lap, a hatókörön belüli tooshow ezeket a riasztásokat. A hello **riasztások** lapon kattintson a riasztást, többek között a javasolt művelet egy egyéni riasztási tooview további részleteit. Hello riasztás is törölheti, ha hello problémát sikerült megoldani.
* Hello **feladatok** területen láthatók a legutóbbi feladatok pillanatképet minden csatlakoztatott eszközön tooyour szolgáltatás. Hivatkozásokat tartalmaz, amelyek toolook használhatja, amelyek jelenleg folyamatban van, az alábbiakhoz hello sikertelen volt a legutóbbi 24 órában, feladatok, vagy azokat, amelyeket ütemezett a hello toorun 24 órán át.
* Hello **gyors áttekintő** terület nyújtanak hasznos információkat, például a szolgáltatás állapotát, az eszközök csatlakoztatott toohello szolgáltatás, hello szolgáltatás helyét és hello szolgáltatás társított hello előfizetés részleteinek száma. A hivatkozás toohello műveleti napló is van. Kattintson a hello hivatkozás toosee az összes befejezett StorSimple Manager szolgáltatás műveletek listáját.

Használhatja a hello StorSimple Manager szolgáltatás irányítópult lap tooinitiate hello a következő feladatokat:

* Megtekintése vagy hello Szolgáltatásregisztrációs kulcs újragenerálása.
* Módosítsa a hello szolgáltatásadat-titkosítási kulcs.
* Hello művelet naplók megtekintése.

## <a name="view-or-regenerate-hello-service-registration-key"></a>Megtekintése vagy hello Szolgáltatásregisztrációs kulcs újragenerálása
hello szolgáltatás regisztrációs kulcsának használt tooregister hello StorSimple Manager szolgáltatásban, a Microsoft Azure StorSimple eszköz, így hello eszköz megjelenik hello további felügyeleti műveletek a klasszikus Azure portálon. hello kulcs hello első eszközön létrehozott és az eszközök hello többi megosztott.

Kattintson a **regisztrációs kulcs** (alján hello hello) megnyitja hello **Szolgáltatásregisztrációs kulcs** párbeszédpanel, ahol az alábbiakat teheti vagy másolási hello aktuális szolgáltatás regisztrációs kulcs toohello vágólapra vagy hello Szolgáltatásregisztrációs kulcs újragenerálása.

Hello kulcsának újragenerálása nincs hatással a korábban regisztrált eszközökre: csak után hello kulcs újragenerálják hello szolgáltatásban regisztrált hello eszközök hatással van.

További információ megtekintéséhez és túl generálása hello szolgáltatás regisztrációs, látogasson[hello Szolgáltatásregisztrációs kulcs lekérése](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-hello-service-data-encryption-key"></a>Szolgáltatásadat-titkosítási kulcs hello módosítása
Szolgáltatás adattitkosítási kulcsokat használt tooencrypt bizalmas felhasználói adatok, például a tárfiók hitelesítő adatait, a StorSimple Manager szolgáltatás toohello StorSimple eszközről küldött. Szüksége lesz toochange ezeket a kulcsokat rendszeres időközönként, ha az IT szervezet rendelkezik egy kulcs Elforgatás házirend hello tárolóeszközökön. hello kulcsváltozás folyamat kis mértékben eltérhet attól függően, hogy van egy egyetlen vagy több eszközön hello StorSimple Manager szolgáltatás kezeli.

Változó hello szolgáltatásadat-titkosítási kulcs egy 3 lépésben:

1. Egy eszköz toochange hello szolgáltatásadat-titkosítási kulcs hello klasszikus Azure portál használatával, engedélyezésére.
2. A Windows PowerShell használatával a StorSimple, indítsa el a hello szolgáltatás titkosítási kulcs változását.
3. Ha egynél több StorSimple eszközt, frissíteni hello szolgáltatásadat-titkosítási kulcs hello az egyéb eszközöket.

hello következő lépések azt mutatják be hello helyettesítő folyamata hello szolgáltatásadat-titkosítási kulcs.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-hello-operations-logs"></a>Hello műveletek naplók megtekintése
Hello művelet naplók hello található hivatkozásra kattintva megtekintheti a hello műveletnaplók **gyors áttekintő** hello irányítópult panelén. A rendszer ekkor toohello felügyeleti szolgáltatások lap, ahol szűrése, és tekintse meg a hello naplózza adott tooyour StorSimple Manager szolgáltatás.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[hibaelhárítása a StorSimple eszköz](storsimple-troubleshoot-operational-device.md).
* További tudnivalók túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

