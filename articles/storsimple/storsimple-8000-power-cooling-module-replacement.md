---
title: "a StorSimple 8000 series eszközén PCM aaaReplace |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooremove és csere hello energia- és hűtési modul (PCM) a StorSimple eszköz"
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 474fd09787c5361a81efda4de74356027ac60f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Cserélje le a energia- és hűtési modul a StorSimple eszköz
## <a name="overview"></a>Áttekintés
hello Power és hűtési modul (PCM) a Microsoft Azure StorSimple eszközt a áll egy tápegység- és hűtési elsődleges hello és EBOD csatolmányt által szabályozott ventilátor. Csak egy modellje, amely minden ház minősítéssel PCM van. hello elsődleges ház egy 764 W PCM minősítéssel, és egy 580 W PCM hello EBOD ház minősítéssel. Annak ellenére, hogy hello PCMs hello elsődleges ház és hello EBOD ház különböző, hello helyettesítő eljárás megegyezik.

Ez az oktatóanyag azt ismerteti, hogyan:

* Távolítsa el a PCM
* A helyettesítő PCM telepítése

> [!IMPORTANT]
> Mielőtt eltávolítása és cseréje egy PCM, tekintse át a hello biztonsági adatokat a [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).


## <a name="before-you-replace-a-pcm"></a>Mielőtt lecseréli a PCM
Vegye figyelembe a fontos problémákat követően lecseréli a PCM hello:

* Ha hello tápegység a meghiúsul PCM, hello hello hibás modul telepítve hagyja, de hello tápkábelét eltávolítása. hello ventilátor hello ház tooreceive tápellátás folytatódik, és folytatni a tooprovide megfelelő hűtési. Hello ventilátor meghibásodásakor hello PCM kell cserélni azonnal toobe.
* Mielőtt eltávolítaná a hello PCM, válassza le hello power hello PCM (ha van ilyen) hello fő kapcsoló kikapcsolásával vagy hello tápkábelét fizikailag eltávolításával. Ez biztosítja, hogy egy power leállítás rövidesen figyelmeztető tooyour rendszer.
* Győződjön meg arról, hogy más PCM működőképességét a hello folytatható rendszer cseréje hello hibás PCM előtt. Hibás PCM kell helyettesíteni egy teljesen működőképes PCM lehető legrövidebb időn belül.
* PCM modul helyettesítő csak néhány perc toocomplete vesz igénybe, de eltávolítása nem sikerült hello PCM tooprevent túlmelegedése követő 10 percen belül kell végezni.
* Vegye figyelembe, hogy hello helyettesítő 764 W PCM modulok hello gyárból származó szállított tartalmaz hello biztonsági mentési akkumulátor modul. Először a hibás PCM tooremove hello akkumulátor kell, majd beilleszteni hello helyettesítő modul előzetes tooperforming hello helyettesítő. További információkért lásd: hogyan túl[távolítsa el, majd szúrja be a biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md).

## <a name="remove-a-pcm"></a>Távolítsa el a PCM
Kövesse ezeket az utasításokat, ha készen áll a tooremove egy Power és hűtési modul (PCM) a Microsoft Azure StorSimple eszközön.

> [!NOTE]
> A PCM eltávolítása előtt győződjön meg arról, hogy rendelkezik-e a megfelelő helyettesítő (764 hello elsődleges ház W) vagy a hello EBOD ház W 580.

#### <a name="tooremove-a-pcm"></a>egy PCM tooremove
1. Hello a klasszikus Azure portálon, kattintson **beállítások > figyelő > hardver állapotának**. Hello PCM összetevők alatt hello állapotának ellenőrzése **összetevők megosztott** tooidentify, amely PCM sikertelen volt:
   
   * Nem sikerült a tápegységet PCM 0, ha hello állapotának **tápegység PCM 0 a** pedig piros színűvé változik.
   * Ha egy tápegység PCM az 1. sikertelen volt, hello állapotának **PCM az 1. tápegység** pedig piros színűvé változik.
   * Ha hello ventilátor PCM az 1. sikertelen volt, vagy állapotának hello **0 hűtési PCM 0 a** vagy **PCM 0 1 hűtési** pedig piros színűvé változik.
2. Keresse meg a hello sikertelen PCM biztonsági a hello elsődleges ház hello. Ha futtat egy 8600 modell, azonosítsa a hello elsődleges ház hello hello előlapja LED képernyőjén látható rendszer azonosító száma alapján. az alapértelmezett egység azonosítója: hello elsődleges ház megjelenő hello **00**, mivel egység azonosítója: hello EBOD ház megjelenő hello alapértelmezett **01**. hello következő ábra és táblázat ismertetik hello előlapja hello LED megjelenítése.
   
    ![Előlapján OPS azonosító](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **1. ábra** első panel hello eszköz  
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |Némító gomb |
   | 2 |Rendszer energiagazdálkodási |
   | 3 |A modul hiba |
   | 4 |Logikai hiba |
   | 5 |Egység azonosító megjelenítése |
3. kijelző LED hello hello elsődleges ház oldalán a figyelési hello is tooidentify hello hibás PCM. Hello következő ábra és táblázat hogyan toounderstand toouse hello LED toolocate hello hibás PCM. Például ha hello vezetett megfelelő toohello **ventilátor sikertelen** van bekapcsolásával; hello ventilátor sikertelen volt. Hasonlóképpen, ha hello vezetett túl megfelelő**AC sikertelen** van bekapcsolásával; hello tápegység sikertelen volt. 
   
    ![Eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **2. ábra** vissza a PCM a kijelző LED
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |AC áramszünet esetén |
   | 2 |Hiba ventilátor |
   | 3 |Akkumulátor hiba |
   | 4 |PCM OK |
   | 5 |DC áramszünet esetén |
   | 6 |Kifogástalan akkumulátor |
4. Tekintse meg a következő hátsó hello StorSimple eszköz toolocate sikertelen hello PCM modul hello ábrája toohello. Hello bal oldali van PCM 0 és PCM 1 hello jobb. hello táblázatot hello modulok ismerteti.
   
     ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **3. ábra** oldalán a beépülő modulok eszköz 
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |A vezérlő 0 |
   | 4 |1. vezérlő |
5. Kapcsolja ki hibás PCM hello, vagy válassza le hello tápegység csatlakozója. Most már eltávolíthatja hello PCM.
6. Hello zárolás és PCM kezelni a mutatóujj között görgetőgomb hello hello oldalán megfogható, és együtt tooopen hello leíró nyomja össze.
   
    ![Nyitó PCM leíró](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **4. ábra** nyitó hello PCM kezelése
7. Fogantyú hello hello PCM eltávolítása és kezelésére.
   
    ![PCM-eszközök eltávolítása](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **5. ábra** eltávolításával hello PCM

## <a name="install-a-replacement-pcm"></a>A helyettesítő PCM telepítése
Hajtsa végre az ezen utasításokat tooinstall egy PCM a StorSimple eszköz. Győződjön meg arról, hogy beszúrt hello biztonsági mentési akkumulátor modul előzetes tooinstalling hello helyettesítő PCM (too764 W PCMs vonatkozik). További információkért lásd: hogyan túl[távolítsa el, majd szúrja be a biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md).

#### <a name="tooinstall-a-pcm"></a>egy PCM tooinstall
1. Győződjön meg arról, hogy rendelkezik-e hello megfelelő váltja fel PCM a ház. hello elsődleges ház kell egy 764 W PCM és hello EBOD ház kell egy 580 W PCM. Ne próbáljon toouse 580 W PCM hello elsődleges szolgáltatással hello, vagy a hello EBOD ház 764 W PCM hello. a következő kép hello jeleníti meg, ahol ezt az információt a hello címke, amely tooidentify elhelyezni toohello PCM.
   
    ![Eszköz PCM címke](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **6. ábra** PCM címke
2. Ellenőrizze, hogy a sérülés toohello ház, különös tekintettel toohello összekötők. 
   
   > [!NOTE]
   > **Ha bármely összekötő elgörbülve ne telepítse hello modul.**
   > 
   > 
3. Hello PCM hello kezelni a nyissa meg a pozíció, hello ház hello modult menüpontban húzással állíthatja be.
   
    ![PCM eszköz telepítése](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **7. ábra** telepítése hello PCM
4. Zárja be kézzel a hello PCM leíró. Egy kattintással kell hall, mivel hello leíró zárolás kapcsolatba lép.
   
   > [!NOTE]
   > amely összekötő PIN-kódok hello tooensure végző rendelkezik, akkor is óvatosan tug hello leírón hello zárolás feloldása nélkül. Hello PCM diák ki, ha ez arra utalhat, hogy hello zárolás előtt hello összekötők részt vevő be lett zárva.
   
5. Csatlakozás hello power kábelek toohello áramforrásról és közös toohello PCM.
6. Biztonságos hello törzs mentesség bálákban.
7. Hello PCM bekapcsolása.
8. Győződjön meg arról, hogy sikeres volt-e helyettesítő hello: hello Azure-portálon a StorSimple Device Manager szolgáltatást, lépjen a tooyour eszközt, majd túl**beállítások > figyelő > hardver állapotának**. A hello **összetevők megosztott**, hello PCM hello állapota zöld kell lennie.
   
   > [!NOTE]
   > Hello helyettesítő PCM toocompletely inicializálása néhány percig is eltarthat.

## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).

