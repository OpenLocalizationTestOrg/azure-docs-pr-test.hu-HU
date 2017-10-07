---
title: "aaaUse a StorSimple 8000 series eszköz összefoglaló |} Microsoft Docs"
description: "Hello StorSimple Device Manager szolgáltatás eszközök összefoglaló ismerteti, hogyan toouse azt tooview tárolási metrikák és a csatlakoztatott kezdeményezők és a keresés hello sorozatszámát és IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: b45ffc6ec52ebb6549c25a00c68c62460b208b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-summary-in-storsimple-device-manager-service"></a>Hello az eszköz StorSimple Device Manager szolgáltatás összegzése

## <a name="overview"></a>Áttekintés
hello StorSimple eszköz összefoglaló panel lehetővé teszi az adott StorSimple eszköz információk áttekintését, ezzel szemben a toohello szolgáltatás összefoglaló panel, amelyen a Microsoft Azure StorSimple megoldásban szereplő összes hello eszközök adatait jeleníti meg.

hello eszköz összefoglaló panel összegzését jeleníti meg a StorSimple 8000 series eszköz regisztrálva van-e egy adott StorSimple az Eszközkezelő kiemelés egy rendszergazda figyelmet igénylő eszköz ismertetünk. Ez az oktatóanyag hello eszköz összefoglaló panel vezet be, hello tartalom és funkcióját ismerteti, és azokat hello feladatokat hajthat végre ezen a panelen.

hello eszköz összefoglaló csempe megjeleníti a következő információ hello:

![Eszköz összefoglaló panel](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a>Felügyeleti parancssáv

Hello StorSimple eszköz panelen látható hello-beállítások a StorSimple eszköz kezeléséhez. Hello parancsok hello panelről és hello bal oldalán hello tetején megjelenik. Használja a beállítások tooadd megosztások vagy kötetek, vagy frissíteni, vagy az eszköz a feladatátvétel.

![Felügyeleti parancssáv](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a>Alapvető erőforrások

hello essentials terület rögzíti, hello fontos tulajdonságok többek között, hello állapota, modell, cél IQN és hello szoftverének verziójával. 

![Eszköz alapjai](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a>Figyelés

* Hello **riasztások** csempe hello összes aktív riasztás pillanatképe biztosít az eszközt, a riasztás súlyosságát szerint csoportosítva.

    ![Riasztások csempe](./media/storsimple-8000-device-dashboard/device-summary4.png)

    Kattintson a hello csempe tooopen hello **riasztások** panel megnyitásához, és kattintson egy adott riasztás tooview további részleteit a riasztást, többek között a javasolt műveletek. Hello riasztás is törölheti, ha hello problémát sikerült megoldani.

    ![Kattintson a riasztások csempe](./media/storsimple-8000-device-dashboard/device-summary10.png)

* Hello **állapotát és állapotfigyelő** csempe hello hardver összetevő állapota betekintést biztosít egy eszközt, beleértve a hello Eszközállapot. hello Eszközállapot lehet offline, online, inaktív vagy készen tooset fel.

    ![Állapot és állapota csempe](./media/storsimple-8000-device-dashboard/device-summary5.png)

* Hello **kötetek** csempe állapot szerint csoportosítva az eszköz a kötetek száma hello összegzését tartalmazza.

    ![Adatkötetek meghajtóbetűjelei](./media/storsimple-8000-device-dashboard/device-summary6.png)

    Kattintson a hello csempe tooopen hello **kötetek** panelt, és majd kattintson az egy egyéni kötet tooview vagy annak tulajdonságait.
    
    ![Kattintson az adatkötetek meghajtóbetűjelei](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    További információkért lásd: hogyan túl[kötetek kezelése](storsimple-8000-manage-volumes-u2.md).

* A hello **használati** diagram, megtekintheti a eszköz, mind a hello felhőalapú tárolás alatt hello felhasználásra elmúlt 7 nap, hello alapértelmezett időszakban használt elsődleges tároló hello.

     ![Használata csempe](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     egy másik idő méretezési toochoose hello használata **szerkesztése** hello diagram hello jobb felső sarkában található beállítás.

     ![Használati diagram szerkesztése](./media/storsimple-8000-device-dashboard/device-summary12.png)

     Ezen a diagramon hello teljes elsődleges tárterület (állomások tooyour eszköz által írt adatok mennyiségét hello) metrikáit megtekintheti és hello teljes felhőbeli tárhelyén, az eszköz által felhasznált, egy meghatározott időtartamra vonatkozóan.
  
     Ebben a környezetben *elsődleges tárolási* hello állomás által írt adatok teljes mennyiségének toohello hivatkozik, és a kötet típusa szerint sorolhatók: *elsődleges rétegzett tárolás* egyaránt helyben tárolt adatok és az adatok rétegzett toohello felhő. *Elsődleges helyileg rögzített tárolási* csak helyben tárolja az adatokat tartalmazza. *Felhőbeli tárhely*, a hello ugyanakkor, a hello hello felhőben tárolt adatok teljes mennyiségének mérését. Ez a tároló rétegzett adatok és a biztonsági mentések tartalmaz. hello felhőben tárolt hello adatok deduplikált, és a tömörített, mivel elsődleges tárolási tárhelyet előtt hello adatok deduplikációja és tömörített hello összegét mutatja. (Ezek két szám tooget hello tömörítési arány képet is összehasonlíthatja.) A mind az elsődleges és a felhőbeli tárhelyén, hello összegek konfigurált gyakoriságnak követési hello alapulnak. Például ha úgy dönt, hogy az egy hét gyakoriságát, majd hello diagram adatainak megjelenítése minden nap az előző hét a hello.

     felhőalapú tárolás felhasznált idő, jelölje be hello keresztül toosee hello mennyisége **FELHŐALAPÚ TÁROLÓT használja a** lehetőséget. toosee hello tárhelyet hello állomás, jelölje be hello írt **elsődleges RÉTEGZETT TÁROLÓT használja a** és **elsődleges HELYILEG rögzített TÁROLÓT használja a** beállítások. 
     További információkért lásd: [használata hello StorSimple Device Manager szolgáltatás toomonitor a StorSimple eszköz](storsimple-monitor-device.md).


* Hello **kapacitás** csempe megjeleníti hello elsődleges tárolási ellátott és keresztül hello eszköz relatív toohello teljes tárterület érhető el a fennmaradó hello azonos. **Kiépített** toohello összeg elő és használatra, tároló hivatkozik **fennmaradó** maradt kapacitás, amelyek között az eszköz létesíthetők toohello hivatkozik. 

    ![Használata csempe](./media/storsimple-8000-device-dashboard/device-summary8.png)

    Kattintson a csempére tooview hogyan ki van építve hello kapacitás rétegzett és helyileg rögzített kötetek között. Hello **fennmaradó rétegzett** kapacitása hello elérhető kapacitás, amelyek többek között a felhőbe, miközben hello létesíthetők **fennmaradó helyi** hello lemezek fennmaradó hello kapacitás toothis eszköz csatlakoztatva.

    ![Kattintson a használati diagramon](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a>Következő lépések
* További tudnivalók hello [StorSimple szolgáltatás összefoglaló panel](storsimple-8000-service-dashboard.md).
* További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

