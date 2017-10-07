---
title: "a Microsoft Azure StorSimple 8000 series eszközön aaaReplace akkumulátor |} Microsoft Docs"
description: "Leírja, hogyan tooremove, cserélje le, és a StorSimple eszköz hello biztonsági mentési akkumulátor modul karbantartása."
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 5ac767807e6c3fd817d8d522629db2aceaac9bdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a>Cserélje le a StorSimple eszköz hello biztonsági mentési akkumulátor modul

## <a name="overview"></a>Áttekintés
hello elsődleges ház teljesítmény- és hűtési modul (PCM) a Microsoft Azure StorSimple eszköz rendelkezik egy további akkumulátor csomagot. Ez a csomag biztosítja a teljesítmény, így hello StorSimple eszköz adat menthető AC power toohello elsődleges ház elvesztése esetén. Ez akkumulátor csomag hivatkozott tooas hello *biztonsági mentési akkumulátor modul*. csak az elsődleges ház hello a StorSimple eszköz létezik hello biztonsági mentési akkumulátor modul (hello EBOD ház nem tartalmaz a biztonsági mentési akkumulátor modul).

Ez az oktatóanyag azt ismerteti, hogyan:

* Hello biztonsági mentési akkumulátor modul eltávolítása
* Egy új biztonsági mentési akkumulátor modul telepítése
* Hello biztonsági mentési akkumulátor modul karbantartása

> [!IMPORTANT]
> Mielőtt eltávolítása és cseréje egy biztonsági mentési akkumulátor modult, tekintse át a biztonsági információk hello hello [bemutatása tooStorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-hello-backup-battery-module"></a>Hello biztonsági mentési akkumulátor modul eltávolítása
hello biztonsági mentési akkumulátor modul a StorSimple eszközt a egy terepen cserélhető Cisco egységet. Mielőtt hello PCM van telepítve, az eredeti csomagban hello akkumulátor modul kell tárolni. Hajtsa végre a következő lépéseket tooremove hello biztonsági mentési akkumulátor hello.

#### <a name="tooremove-hello-backup-battery-module"></a>tooremove hello biztonsági mentési akkumulátor modul
1. A hello Azure-portálon válassza a tooyour StorSimple Device Manager szolgáltatás panelre. Nyissa meg túl**eszközök** majd válassza ki az eszköz hello eszközök listáját. Keresse meg a túl**figyelő** > **hardver állapotának**. A **megosztott összetevők**, nézze meg hello akkumulátor hello állapotának.
2. Hello PCM mely hello az akkumulátor nem sikerült azonosítani. 1. ábra mutatja hello hátsó hello StorSimple eszközt.
   
    ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-battery-replacement/IC740994.png)
   
    **1. ábra** hátsó PCM és a tartományvezérlő modulok megjelenítő elsődleges eszköz
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |A vezérlő 0 |
   | 4 |1. vezérlő |
   
    Hello 2. ábra a 3-as számú eredményobjektumokról mutató figyelési hello PCM 0 megfelelő túl a kereslet az olyan**akkumulátor tartalék** kell kell kapcsolni.
   
    ![Az eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-battery-replacement/IC740992.png)
   
    **2. ábra** vissza a PCM ábrázoló hello kijelző LED figyelése
   
   | Címke | Leírás |
   |:--- |:--- |
   | 1 |AC áramszünet esetén |
   | 2 |Hiba ventilátor |
   | 3 |Akkumulátor hiba |
   | 4 |PCM OK |
   | 5 |DC áramszünet esetén |
   | 6 |Kifogástalan akkumulátor |
3. tooremove hello PCM sikertelen töltöttségű telepre vonatkozó, a kövesse hello [távolítsa el a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).
4. Hello PCM eltávolítva, a növekedési és a Elforgatás hello töltöttségű telepre vonatkozó modul felfelé kezelni, a következő ábra hello jelöltük, és húzza azt ki tooremove hello töltöttségű telepre vonatkozó.
   
    ![PCM akkumulátor eltávolítása](./media/storsimple-battery-replacement/IC741019.png)
   
    **3. ábra** hello PCM hello akkumulátor eltávolítása
5. Tegyen hello modul hello terepen cserélhető Cisco egységet-csomagban.
6. Térjen vissza a hello hibás egység tooMicrosoft megfelelő szervizelési és kezelése.

## <a name="install-a-new-backup-battery-module"></a>Egy új biztonsági mentési akkumulátor modul telepítése
Hajtsa végre a következő lépéseket akkumulátor tooinstall hello helyettesítő modul hello PCM hello elsődleges szolgáltatással a StorSimple eszköz hello.

#### <a name="tooinstall-hello-battery-module"></a>tooinstall hello akkumulátor modul
1. Hely hello biztonsági mentési akkumulátor modul a hello PCM hello megfelelő helyzetben.
2. Lefelé hello akkumulátor modul nyomja le az összes hello módon tooseat hello összekötő kezelni.
3. Cserélje le PCM hello hello elsődleges szolgáltatással hello útmutatását követve [energia- és hűtési modul lecseréli a StorSimple eszköz](storsimple-power-cooling-module-replacement.md).
4. Hello helyettesítő befejezése után nyissa meg tooyour eszközt, és keresse meg a túl**figyelő** > **hardver állapotának** a hello Azure-portálon. Hello hello akkumulátor toomake meg arról, hogy sikeres volt-e hello telepítési állapotának ellenőrzése. Zöld állapot azt jelzi, hogy hello akkumulátorról működik megfelelően.

## <a name="maintain-hello-backup-battery-module"></a>Hello biztonsági mentési akkumulátor modul karbantartása
A StorSimple eszköz hello biztonsági mentési akkumulátor modul biztosít power toohello vezérlő power adatvesztési esemény során. Ez lehetővé teszi, hogy hello StorSimple eszköz toosave kritikus fontosságú adatok előzetes tooshutting le szabályozott módon. A két akkumulátor feltöltött hello PCMs hello rendszer kezelni tud a két egymást követő adatvesztési eseményeket.

Hello Azure-portálon, a hello **hardver állapotának** alatt hello **figyelő** panel jelzi, hogy a hello akkumulátor hibásan működik, vagy hello end életciklusa közeledik. hello akkumulátor állapotát jelzi **PCM 0 akkumulátor** vagy **PCM 1 akkumulátor** alatt **megosztott összetevők**. Ezen a panelen megjelenik egy **csökkentett teljesítményű** állapotban a(z) élettartam végi közeledik, és **sikertelen** end életciklusa elérte a.

> [!NOTE]
> hello akkumulátor is tud jelentéseket **sikertelen** ha egyszerűen kell toobe számítjuk fel.


Ha hello **csökkentett teljesítményű** állapot jelenik meg, azt javasoljuk, hogy a következő lépések hello:

* Előfordulhat, hogy hello rendszer észlelt egy friss az áramellátás megszakadása miatt, vagy előfordulhat, hogy a hello akkumulátorok rendszeres karbantartás zajlik. Figyelje meg hello rendszer a folytatás előtt 12 óra.
  
  * Ha hello állapot továbbra is **csökkentett teljesítményű** után 12 óra folyamatos kapcsolat tooAC power hello, tartományvezérlői és PCMs fut, majd hello akkumulátor toobe cserélni kell-e. Adjon [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) helyettesítő biztonsági mentési akkumulátor modulként.
  * Hello állapotba kerül, 12 óra elteltével az OK gombra, ha hello akkumulátorról működik, és csak szükséges a karbantartási járnak.
* Ha nem történt AC power és hello PCM társított megszűnését be van kapcsolva, és tooAC power csatlakoztatva, hello akkumulátor kell cserélni toobe. [Forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder helyettesítő biztonsági mentési akkumulátor modul.

> [!IMPORTANT]
> Számolja fel hello toonational és regionális előírások szerint akkumulátor nem sikerült.

## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).

