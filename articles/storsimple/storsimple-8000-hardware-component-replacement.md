---
title: "aaaStorSimple 8000 sorozat hardver összetevő cseréje |} Microsoft Docs"
description: "Ismerteti, hogyan toosafely cseréje hello PCMs, akkumulátor, vezérlő modulok, EBOD vezérlők, meghajtók és a StorSimple eszköz váz."
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
ms.custom: 
ms.openlocfilehash: 5baca8ff630a1c064cb8bf7e1024b6590f0d6b81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-hardware-component-on-your-storsimple-8000-series-device"></a>Cserélje le a StorSimple 8000 series eszközön egy hardverösszetevő

## <a name="overview"></a>Áttekintés
hello hardver összetevő helyettesítő oktatóanyagok hello hardverösszetevők, a Microsoft Azure StorSimple 8000 series eszköz és hello lépéseket szükséges tooremove írják le, és cserélje le a. Ez a cikk azt ismerteti, hello biztonsági ikonok, mutatókat biztosít toohello részletes oktatóprogramjai, és listák hello cserélhető összetevőket.

> [!IMPORTANT]
> Tooremove megkísérlése előtt vagy StorSimple összetevők közül bármelyik lecseréléséhez győződjön meg arról, hogy tekintse át a hello [biztonsági ikon egyezmények](#safety-icon-conventions) és egyéb [biztonsági óvintézkedéseket](storsimple-safety.md).


### <a name="safety-icon-conventions"></a>Biztonsági ikon konvenciók
hello következő táblázatban szereplő használt hello biztonsági ikonok. Figyelmesen elolvassa az toothese biztonsági ikonok hello lépéseket tooremove keresztül halad, és cserélje le az eszköz összetevőket.

| Ikon | Szöveg | További információ |
|:--- |:--- |:--- |
| ![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/Warning.png) |**VESZÉLY!** |Azt jelzi, hogy egy eredményező, nem elkerülhető, ha halál vagy súlyosan veszélyes helyzetben. Ez a jel szó korlátozott toohello rendkívüli helyzetekben. |
| ![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/Warning.png) |**FIGYELMEZTETÉS!** |Azt jelzi, hogy egy veszélyes helyzetben, ha nem elkerülhető, halállal vagy komoly kárt okozhat. |
| ![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/Caution.png) |**FIGYELEM!** |Azt jelzi, hogy nem elkerülhető, ha a kisebb vagy mérsékelt kárt okozhat veszélyes helyzet. |
| ![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/NoticeIcon.png) |**FIGYELMEZTETÉS:** |Azt jelzi, fontos, de nem a veszély kapcsolatos információt. |
| ![Elektromos ütés ikon](./media/storsimple-hardware-component-replacement/Electric.png) |**Elektromos ütés veszély** |Azt jelzi, hogy magas feszültségérzékelő. |
| ![Nagy súlyozást ikon](./media/storsimple-hardware-component-replacement/Weight.png) |**Nehéz súlyozás** | |
| ![Nincs felhasználói használható kijelzők ikonja](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png) |**Nincsenek felhasználói használható részei** |Nem érhető el, ha megfelelően be van tanítva. |
| ![Olvasható utasításokat ikon](./media/storsimple-hardware-component-replacement/ReadInstructions.png) |**Először olvassa el az összes utasításokat** | |
| ![Tipp veszély ikon](./media/storsimple-hardware-component-replacement/TipHazard.png) |**Tipp veszély** | |

### <a name="before-you-begin"></a>Előkészületek
Ismerkedjen meg a jelen oktatóanyagban használt eszköz és a biztonsági ikonok hello biztonsági információt. Nyissa meg túl[biztonságosan telepítéséhez, és a StorSimple eszköz üzemeltetéséhez](storsimple-safety.md) kapcsolatos részletes információkért. Lehet, hogy tooreview hello [biztonsági óvintézkedéseket](storsimple-safety.md#handling-precautions) előtt a StorSimple eszközt kezeli.

Tooreplace összetevő-megkezdése előtt fontolja meg a következő információ hello.

![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/Warning.png) ![elektromos ütés ikon](./media/storsimple-hardware-component-replacement/Electric.png) **figyelmeztetés!**

* Szabad saját kezűleg megfelelően kisülés vagy antisztatikus mat használatával modulok és a StorSimple eszköz összetevői kezelésekor.
* Bármely áramkört nem használja. Használja a megadott hello leírók és útmutatók összetevők, előfordulhat, hogy rendelkezik kitett áramkört kezelése közben.

![Figyelmeztetés ikon](./media/storsimple-hardware-component-replacement/Warning.png) ![ikont láthatja](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **értesítés:**

Ha egy modul **SOSEM hagyják el egy üres bay hello hátsó a hello ház**. Szerezzen be egy cseréje vagy üres modul hello probléma rész eltávolítása előtt.

## <a name="hardware-component-replacement-procedures"></a>Hardver összetevő helyettesítő eljárások
A StorSimple 8000 series eszköz több elsődleges hello a beépülő modulok és/vagy EBOD ház áll. hello 8100 rendelkezik egy önálló elsődleges ház, mivel hello 8600 rendelkező elsődleges ház és egy EBOD ház kettős ház eszköz.

fő hardverösszetevőinek hello az eszköz a következő táblák hello foglalja össze. Hello hello hivatkozásra **helyettesítő eljárás** oszlop toogo toohello tartozó oktatóanyag.

| Összetevők | # Jelen van | Beépülő modult? | Csere eljárás |
|:--- |:--- |:--- |:--- |
| Készülékház |1 |Nem |[Cserélje le a StorSimple eszköz hello készülékház](storsimple-8000-chassis-replacement.md) |
| Elsődleges tartományvezérlők |2 |Igen |[Cserélje le a StorSimple eszköz vezérlő modul](storsimple-8000-controller-replacement.md) |
| 764W energia- és hűtési modulok (PCMs) |2 |Igen |[Cserélje le a energia- és hűtési modul a StorSimple eszköz](storsimple-8000-power-cooling-module-replacement.md) |
| Biztonsági mentési töltöttség esetén |2 |Igen |[Cserélje le a StorSimple eszköz hello biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md) |
| Lemezmeghajtó |12 |Igen |[Cserélje le a lemezmeghajtó a StorSimple eszköz](storsimple-8000-disk-drive-replacement.md) |

**1. táblázat** hardverösszetevők hello elsődleges szolgáltatással

hello elsődleges ház és hello EBOD ház különböznek az i/o-modulok. Ezenkívül hello PCMs kell különböző teljesítményt. hello PCMs hello elsődleges szolgáltatással 764 W, mivel a hello EBOD ház 580 w hello PCMs elsődleges hello a ház is tartalmazhat, egy biztonsági mentési akkumulátor modul.

| Összetevők | # Jelen van | Beépülő modult? | Csere eljárás |
|:--- |:--- |:--- |:--- |
| Készülékház |1 |Nem |[Cserélje le a StorSimple eszköz hello készülékház](storsimple-8000-chassis-replacement.md) |
| EBOD tartományvezérlők |2 |Igen |[Cserélje le az EBOD vezérlőhöz a StorSimple eszköz](storsimple-8000-ebod-controller-replacement.md) |
| 580W energia- és hűtési modulok (PCMs) |2 |Igen |[Cserélje le a energia- és hűtési modul a StorSimple eszköz](storsimple-8000-power-cooling-module-replacement.md) |
| Lemezmeghajtó |12 |Igen |[Cserélje le a lemezmeghajtó a StorSimple eszköz](storsimple-8000-disk-drive-replacement.md) |

**2. táblázat** hello EBOD ház hardverösszetevőinek

hello a beépülő modulok hello eszköz az első és hátsó diagramok a következő hello vannak kiemelve. Használhatja a diagramok toodetermine hello helyét hello különféle beépülő modulok Ha helyettesítő telepítésére szükség. hello első ábrán látható hello meghajtók, valamint hello hátsó az hello EBOD ház és elsődleges ház hello megjelenítése hello a beépülő modulok.

![Az eszköz lemezmeghajtók Frontplane](./media/storsimple-hardware-component-replacement/IC741028.png)

**1. ábra** első hello eszköz

| Címke | Leírás |
|:--- |:--- |
| 0 - 11 |Lemezmeghajtók (összesen 12) |

Hello elsődleges ház és hello EBOD ház is rendelkezzen a meghajtó vivőjel-modulok. hello váz Kezelőkód tizenkét 3.5-ös "lemezmeghajtó rendezett 3 által 4 formátumban.

![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-hardware-component-replacement/IC740994.png)

**2. ábra** hátsó hello elsődleges ház

| Címke | Leírás |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |A vezérlő 0 |
| 4 |1. vezérlő |

![Az eszköz EBOD ház beépülő modulok Csatlakozópanel](./media/storsimple-hardware-component-replacement/IC769599.png)

**3. ábra** hátsó hello EBOD ház

| Címke | Leírás |
|:--- |:--- |
| 1 |PCM 0 |
| 2 |PCM 1 |
| 3 |EBOD vezérlő 0 |
| 4 |1. EBOD vezérlő |

## <a name="field-replaceable-units"></a>A mező egységek
a StorSimple eszközt a következő mező egységek (részegysége) hello érhetők el:

* Váz (beleértve a hello integrált műveletek panel)
* 764 W AC PCM
* 580 W AC PCM
* Merevlemez-meghajtó meghajtó szolgáltatónként modullal
* A tartományvezérlő modul
* EBOD vezérlő modul
* Biztonsági mentési akkumulátor modul
* Állvány oldalon kit csatlakoztatása

Adjon [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder bármelyik a csere egységeket.

## <a name="next-steps"></a>Következő lépések
Tekintse át az összes [biztonsági információk](storsimple-safety.md) tooreplace StorSimple hardverösszetevőt megkísérlése előtt.

